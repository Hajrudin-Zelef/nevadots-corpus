---
id: collect-261121-veille-ia/langages-dev/postgresql-19-release-date-moved-to-nov-19-after-ai-review-found-too-many-bugs
title: "PostgreSQL 19 Release Date Moved to Nov 19 After AI Review Found Too Many Bugs"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["release", "postgresql", "review", "pettus", "date", "november", "found", "bugs"]
source: https://dev.to/alanwest/postgresql-19-release-date-moved-to-nov-19-after-ai-review-found-too-many-bugs-48n
source_anchor: ""
source_lines: [1, 102]
section: "Langages_Dev"
---
PostgreSQL 19 was supposed to be out by now. Postgres 18 shipped on September 25, 2025, and 19 was tracking the same autumn slot. The new PostgreSQL 19 release date is November 19, 2026, and somewhere between eight and ten significant features got ripped out on the way there.

The reason is the part that gets people arguing. Peter Eisentraut, a long-time committer who works at EDB, wrote on October 9 that LLM-assisted code review was at least partly behind the reverts. In his words, review problems "can get found much faster, and then you're staring at a list of like forty defect reports." The Register picked it up with the headline that AI reviewers found more bugs than humans can fix.

As far as I know, this is the first major open source database whose release schedule visibly bent because machine review outpaced the people doing triage. Below is what changed and what I'd do about it if you run Postgres in production.

From the RC1 announcement and Christophe Pettus's schedule comparison:

| Milestone | PostgreSQL 18 | PostgreSQL 19 | 
|---|---|---|
| Feature freeze | April 8, 2025 | April 8, 2026 | 
| Beta 1 | May 8, 2025 | June 4, 2026 | 
| Beta 4 | none | September 24, 2026 | 
| RC 1 | September 4, 2025 | October 15, 2026 | 
| RC 2 | none | November 12, 2026 | 
| GA | September 25, 2025 | November 19, 2026 (planned) | 

RC 2 exists because the November 12 minor release "will include a large number of fixes," and the release team wanted those in 19 before GA. Note the wording: GA happens on November 19 "unless an issue is discovered that requires a delay or an additional release candidate." I wouldn't put that date in a change calendar yet.

The Beta 4 notes list the big reverts:

- SQL/PGQ property graph queries
- Online enabling and disabling of data checksums
- 
`UPDATE/DELETE ... FOR PORTION OF` (temporal updates)
- 
`ALTER TABLE ... MERGE PARTITIONS` and`SPLIT PARTITIONS`
- 
`pg_get_role_ddl()` ,`pg_get_tablespace_ddl()` ,`pg_get_database_ddl()`

Pettus's post adds more that went earlier or quietly: `GROUP BY ALL`, non-plain `pg_dumpall` output formats, and the batched foreign-key fast path. That last one is the scary one. Amit Langote's revert commit warned that the bug "can leave a buffered check unperformed, allowing a transaction to commit a permanent foreign key violation without reporting an error."

`REPACK (CONCURRENTLY)` survived, with its scope trimmed. Pettus's advice on it is the best line in the whole saga: "Use it the way you'd use a 1.0 of anything that rewrites your tables: on something you have a backup of."

Two kinds, per Eisentraut. Some features "had significant architectural defects that would have been hard to fix quickly and during beta." The rest was a long tail of edge cases, each one cheap to fix but collectively eating the beta window.

The numbers that stuck with me come from Pettus. On August 25, Robert Haas started a "scary patch contest" thread on -hackers and had Claude rank v19 patches by post-freeze bug fixes. `REPACK` came out at 28 fixes, including data loss, online checksums at about 25, `FOR PORTION OF` at 17, SQL/PGQ at 17, and RI batching at about 16. Noah Misch ran a separate late-August sweep of master with Claude Opus 5.

SQL/PGQ came out over a design problem. As Pettus describes it, prerequisites were only checked at `CREATE PROPERTY GRAPH` time and could be dropped afterwards. Haas wrote: "To me, this class of problem seems completely unacceptable in a committed feature." A model can flag that pattern. A human still has to decide it's a reason to delete 15,859 lines.

Eisentraut was explicit that this is "not due to 'vibe coding'." Most of these features were written before the tools were much good at review. What changed is that "the effective quality bar has been raised after the time the feature was written and committed."

Partly, yes, and nobody seems keen to say it out loud.

Most reverted code never shipped, so 18 doesn't contain SQL/PGQ bugs. But Pettus notes that a cast-cache invalidation bug, found while testing the RI batching code, turned out to affect 18.6 as well. And the 2026 security load says more: more than forty CVEs fixed by August, 28 of them in the August 13 releases alone, and the 18.6 release notes credit Claude-assisted and OpenAI Codex Security-assisted researchers among the reporters. All of that was sitting in versions people run in production right now.

The Hacker News thread split on exactly this. One camp says the bugs always existed and we're just seeing them earlier. The other says "more bugs than humans can fix" also means more reports than humans can verify. One commenter's anecdote: "out of 82 bugs reported only 6-8 were actually bugs." Nobody budgets for the time it takes to sort through that.

My take: the false-positive argument doesn't fit Postgres. These weren't junk reports. Committers confirmed them, fixed them, or reverted whole features over them. The trouble is that Postgres keeps a fixed annual schedule, and the slow step has moved from finding bugs to fixing them.

Yes. I'd have said that even without the delay, and the delay makes it easier.

- On 18.x: stay. It's supported until November 14, 2030. Apply the November 12 minor release, since the RC 2 note says it carries a large batch of fixes.
- On 14.x: this one's urgent. Postgres 14 reaches end of life on November 12, 2026, a week before 19 GA. Go to 18, not to a 19 RC.
- On 15 to 17: no rush. Plan for 18 now, or wait for 19.2 or so.
- Building something new on 19 features: test RC1 in staging, never in production. The project's own wording is that RC1 is "mostly identical" to GA, not identical.

I'd wait for the first couple of 19 minor releases regardless. Eisentraut expects 19 to be "very robust, after all the additional scrutiny," and he may be right. It's also a release where features were cut weeks before GA, and code that took those features out is new code too.

Testing against 19 now is cheap and turns up the boring incompatibilities early. Do this on a staging copy, not your primary.

First, look for the things the 19 release notes say will break:


```
-- Pre-19 dumps taken with this off won't load into 19
SHOW standard_conforming_strings;
-- JIT is off by default in 19; check if you rely on it
SHOW jit;
-- MULE_INTERNAL encoding is removed
SELECT datname FROM pg_database
WHERE pg_encoding_to_char(encoding) = 'MULE_INTERNAL';
-- MD5 passwords now log a warning on every login
SELECT rolname FROM pg_authid WHERE rolpassword LIKE 'md5%';
```
```
# RADIUS auth is gone in 19
grep -i radius "$PGDATA/pg_hba.conf"
# Not a 19 change, but it bites here: initdb enables data checksums by
# default since 18, and pg_upgrade needs matching settings. "0" here means your old cluster has none.
pg_controldata "$PGDATA" | grep -i checksum
```
Then a dry run. Install the 19 RC binaries next to 18, init a new cluster (add `--no-data-checksums` if the old one has none), and run `--check`, which validates without touching data:


```
/usr/lib/postgresql/19/bin/initdb -D /var/lib/postgresql/19/main
/usr/lib/postgresql/19/bin/pg_upgrade --check \
  -b /usr/lib/postgresql/18/bin \
  -B /usr/lib/postgresql/19/bin \
  -d /var/lib/postgresql/18/main \
  -D /var/lib/postgresql/19/main
```
Adjust paths for your packaging. On Debian and Ubuntu, `postgresql.conf` sits in `/etc/postgresql/`, so you'll need `-o`/`-O` to point at the config files. Two new hard blockers in 19 that `--check` will catch: database, role or tablespace names containing carriage returns or line feeds, and btree_gist indexes on `inet`/`cidr`, which per the notes "can exclude rows that should be returned." If you have the second one, you have a correctness bug today, on 18.

After a real test upgrade, raise `max_locks_per_transaction` if you'd tuned it. The default went from 64 to 128 because lock allocation changed, and the notes say settings "must now be doubled" to keep the same capacity.

Postgres has some of the most careful code review in open source. If an LLM-assisted pass can hand committers "like forty" defect reports for features that already got through that review, nobody's codebase is getting a pass. The Postgres team at least had the discipline to revert instead of shipping and patching later.

So: if AI review finds this many bugs in Postgres, what do you think it would find in your codebase? And would your team revert a feature over it, or ship it on schedule and hope?

## Top comments (0)
