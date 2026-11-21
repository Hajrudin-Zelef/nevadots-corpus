---
id: collect-261121-veille-ia/langages-dev/sqlite-3-54-stops-rewriting-an-expression-index-when-the-indexed-value-hasn-t-changed
title: "SQLite 3.54 stops rewriting an expression index when the indexed value hasn't changed"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["index", "sqlite", "update", "email", "expression", "value", "table", "both"]
source: https://dev.to/alexgeorgiev17/sqlite-354-stops-rewriting-an-expression-index-when-the-indexed-value-hasnt-changed-56bb
source_anchor: ""
source_lines: [1, 141]
section: "Langages_Dev"
---
I update a column, the value SQLite computes from it for an index doesn't actually change, and until 3.54 that index still got rewritten anyway. That's a real line from SQLite's own changelog, not a bug report I filed: "when doing an UPDATE on a table that has expression indexes, do not change the expression index if the value being indexed is unchanged." I wanted to know what that was worth in bytes written, not just in changelog prose.

SQLite 3.54.0 shipped on 9 October 2026, with a point release, 3.54.1, on 16 October fixing an unrelated 32-bit buffer bug. I compared 3.54.1 against 3.53.0, the previous stable line, using the precompiled Linux CLI builds from sqlite.org. No Docker needed, no beta flag, no cloud account.

A table of 200,000 users with an expression index on the lowercased email:


```
CREATE TABLE users (id INTEGER PRIMARY KEY, email TEXT, unrelated TEXT, n INTEGER);
CREATE INDEX idx_email_lower ON users(lower(email));
```
Emails are stored as `User123@Example.com`. A common real operation is normalising case on write, or re-saving a row after some unrelated field changed, without the lowercase form ever moving. I ran the same UPDATE against identical copies of the database on both SQLite versions and measured the WAL pages each one wrote, using `PRAGMA wal_checkpoint(PASSIVE)` right after the statement, before the connection could auto-checkpoint and hide the number.

```
UPDATE users SET email = upper(email) WHERE id BETWEEN 1 AND 50000;
```
`lower(email)` is identical before and after; only the stored case changes.

| SQLite version | WAL pages written | Wall time (fastest of 5) | 
|---|---|---|
| 3.53.0 | 2,143 | 0.1213s | 
| 3.54.1 | 511 | 0.0792s | 

That's a 76% drop in pages and roughly a third off the wall clock. Both numbers were identical across three repeated runs on fresh copies of the database, which makes sense for a deterministic btree write path with no cache warm-up involved in the measurement window.

The 511-page figure is the interesting part. I reran the same UPDATE against a copy of the table with no expression index at all, on both versions:


```
old (no index): 0|511|511
new (no index): 0|511|511
```
511 pages, on both SQLite versions, with zero index overhead at all. 3.54.1's cost for the same update, with the expression index present, is also 511. SQLite 3.54 isn't reducing the index-maintenance cost here. It's eliminating it, down to the exact figure you'd get if the index didn't exist.

The saving depends entirely on the computed value staying the same. I ran a second version of the same UPDATE where the email genuinely changes:


```
UPDATE users SET email = 'new' || id || '@example.com' WHERE id BETWEEN 1 AND 50000;
```
```
old: 0|2294|2294
new: 0|2294|2294
```
Identical, on both versions. No benefit whatsoever once the indexed value actually moves. I also timed this case across five runs each: 3.53.0 ranged from 0.150s to 0.175s, 3.54.1 from 0.146s to 0.219s. The ranges overlap and neither version is reliably faster. There's no hidden tax for the extra comparison work either, at least not one big enough to show up against normal run-to-run noise at this scale.

Running the same case-only UPDATE across the whole 200,000-row table, rather than a 50,000-row slice, gave a smaller but still real gap:

| Scope | 3.53.0 WAL pages | 3.54.1 WAL pages | Reduction | 
|---|---|---|---|
| 50,000 rows | 2,143 | 511 | 76% | 
| 200,000 rows (whole table) | 3,779 | 2,148 | 43% | 

The reduction shrinks at full-table scale because more of the write cost there is the base table pages, which both versions have to touch regardless of what happens to the index.

I checkpointed both databases fully after the 50,000-row update and compared the resulting files:


```
old main db size after checkpoint: 17862656 bytes, page_count=4361
new main db size after checkpoint: 17862656 bytes, page_count=4361
```
Identical, down to the byte. The saving is entirely in how much gets written while the operation runs, not in a smaller database afterwards. If you're measuring this feature by checking disk usage after the fact, you won't see anything. You have to catch it mid-write.

The release notes single out expression indexes specifically, which made me wonder whether an ordinary index on a plain column already had this optimisation and 3.54 was just catching expression indexes up. I built a second index, a plain one on an integer column, and ran a no-op self-assignment:


```
CREATE INDEX idx_n ON users(n);
UPDATE users SET n = n WHERE id BETWEEN 1 AND 50000;
```
```
old: 0|133|133
new: 0|133|133
```
Identical on both versions. A plain-column index already skipped the pointless rewrite before 3.54 arrived; this release is specifically closing that gap for expression indexes, exactly as the changelog says, no more and no less.

I confirmed the mechanism by diffing the actual bytecode `EXPLAIN` produces for the two UPDATE statements. For the plain-column case, the only difference between versions is an internal flag encoding on the `IdxDelete` opcode, nothing behavioural. For the expression-index case, 3.54.1 inserts one extra opcode that isn't in 3.53.0's plan:


```
< 27      IdxDelete      1     14    2     idx_email_lower 0   key=r[14..15]
---
> 27      IdxDelete      1     14    1     idx_email_lower 2   key=r[14..15]
...
> 29      IsNull         1     31    0                    0   if r[1]==NULL goto 31
> 30      IdxInsert      1     1     2     2              0   key=r[1]
```
That `IsNull` jump is the whole feature. If the recomputed expression value comes back equal to the old one, execution skips straight past the `IdxInsert`.

I wanted to see how forgiving the underlying expression-index machinery is, since this whole feature only matters if you're actually using one. Indexing a genuinely random function is rejected outright, on both versions:


```
$ sqlite3 t.db "CREATE INDEX idx_bad ON t(random());"
Parse error in 2nd command line argument: non-deterministic functions prohibited in index expressions
  CREATE INDEX idx_bad ON t(random());
              error here ---^
```
But `datetime('now')` is not rejected. It's just as non-deterministic as `random()` in any practical sense, and SQLite let me create the index on both versions without complaint:


```
CREATE INDEX idx_bad2 ON t(datetime('now'));
-- succeeds, no error, on 3.53.0 and 3.54.1 alike
```
SQLite's determinism check is narrower than "the value changes between calls." It catches functions explicitly flagged non-deterministic in its function table, not every function whose result depends on wall-clock time. If you build an index on something like `datetime('now')` by accident, nothing stops you at creation time. SQLite 3.53 added `REINDEX EXPRESSIONS` specifically to repair indexes that drift for reasons like this, which only makes sense if the creation-time check doesn't catch every case, and this test suggests it genuinely doesn't.

`PRAGMA wal_checkpoint(PASSIVE)` right after the statement is the reliable signal, as above. `EXPLAIN` on its own is not: the opcode counts for the two versions differ by exactly one line, and nothing in a casual read of the plan flags that line as the thing that matters. You have to diff the full output, or already know to look for an `IsNull` guard ahead of an `IdxInsert` on your expression index, to notice the feature is there at all.

My first attempt at measuring WAL pages gave me nothing: I ran the UPDATE through the `sqlite3` CLI, closed the connection, then checked the `.db-wal` file on disk, and it didn't exist. For a moment that looked like 3.54.1 wasn't writing to the WAL at all, which would have been a much bigger and much less plausible finding than the one I actually had. What was happening is that the CLI checkpoints and can remove the WAL file on a clean connection close, so by the time I looked, the evidence was already gone, on both versions, which is exactly the kind of harness problem that hides the real effect instead of revealing it. Querying `PRAGMA wal_checkpoint(PASSIVE)` inside the same session, immediately after the UPDATE and before the connection closed, fixed it and is the number this whole post is built on.

```
mkdir sqlite-test && cd sqlite-test
curl -sS -o new.zip https://www.sqlite.org/2026/sqlite-tools-linux-x64-3540100.zip
curl -sS -o old.zip  https://www.sqlite.org/2026/sqlite-tools-linux-x64-3530000.zip
unzip -q new.zip -d new && unzip -q old.zip -d old
chmod +x new/sqlite3 old/sqlite3
for ver in old new; do
  ./$ver/sqlite3 $ver.db <<'EOF'
PRAGMA journal_mode=WAL;
CREATE TABLE users (id INTEGER PRIMARY KEY, email TEXT, unrelated TEXT, n INTEGER);
CREATE INDEX idx_email_lower ON users(lower(email));
INSERT INTO users(id, email, unrelated, n)
  SELECT v, 'User' || v || '@Example.com', 'pad' || v, v
  FROM (WITH RECURSIVE c(v) AS (SELECT 1 UNION ALL SELECT v+1 FROM c WHERE v < 200000) SELECT v FROM c);
EOF
  ./$ver/sqlite3 $ver.db "PRAGMA wal_checkpoint(TRUNCATE);" > /dev/null
  cp $ver.db ${ver}_test.db
  echo -n "$ver: "
  ./$ver/sqlite3 ${ver}_test.db \
    "PRAGMA wal_autocheckpoint=0;" \
    "UPDATE users SET email = upper(email) WHERE id BETWEEN 1 AND 50000;" \
    "PRAGMA wal_checkpoint(PASSIVE);"
done
```
That prints two lines per version: a bare `0` (the autocheckpoint pragma confirming itself), then `0|2143|2143` for 3.53.0 and `0|511|511` for 3.54.1 on my machine. The second and third numbers in the triple are the WAL pages seen and checkpointed.

A profile-save endpoint that rewrites the whole row on every save, email box touched or not, gets 3.54's full saving for free on upgrade, no code change required. A bulk migration that rewrites every email to a genuinely different value gets nothing, and neither version will warn you if the expression index itself is built on something like `datetime('now')`, which both 3.53 and 3.54 create without complaint. Grep your schema for `CREATE INDEX` statements with a function call inside the parentheses, then check which of those two situations each one is actually in, rather than assuming either way.

## Top comments (0)
