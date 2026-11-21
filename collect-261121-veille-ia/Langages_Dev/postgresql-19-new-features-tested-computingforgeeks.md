---
id: collect-261121-veille-ia/langages-dev/postgresql-19-new-features-tested-computingforgeeks
title: "PostgreSQL 19 New Features, Tested | ComputingForGeeks"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-11-19"]
keywords: ["postgresql", "reverted", "release", "json", "ships", "sudo", "features", "ubuntu"]
source: https://computingforgeeks.com/postgresql-19-new-features/
source_anchor: ""
source_lines: [1, 338]
section: "Langages_Dev"
---
Four of the PostgreSQL 19 new features that ran in the early beta builds are gone. Online data checksums, partition MERGE and SPLIT, SQL property graph queries and GROUP BY ALL now fail with “does not exist” or a syntax error. Each was reverted on the release branch between July and September. What remains works. REPACK CONCURRENTLY cut a 355 MB table to 178 MB in 6.96 seconds on our test box, and an INSERT issued mid-rebuild returned in 125 ms.

We reran every example from our earlier beta write-up on the second release candidate (tested November 2026 on Ubuntu 26.04) and added the three late changes that matter for upgrades: two query fixes, and a revert that leaves some JSON indexes unrestorable from a dump. The release candidate announcement plans general availability for November 19, 2026. Later steps cover what pg_upgrade refuses to carry from 18.

## Results: what ships, what was pulled, what was fixed late

We reran twelve features from the beta write-up and the release notes on the same VM. Commit IDs in brackets are the reverts on the release branch, and Step 6 links them:

| Feature | Status in 19 | Measured on the test box | 
|---|---|---|
| REPACK (CONCURRENTLY) | Ships | 355 MB to 178 MB in 6,964 ms; in a second run an INSERT took 124.7 ms | 
| Parallel autovacuum | Ships, off by default | autovacuum_max_parallel_workers = 0 | 
| pg_stat_autovacuum_scores | Ships | Bloated table scored 29.97, catalogs under 1 | 
| INSERT … ON CONFLICT DO SELECT | Ships | Returns the existing row, with or without FOR UPDATE | 
| COPY TO (FORMAT json) | Ships | One JSON object per line | 
| Online data checksums | Reverted (c05d5ce1236) | pg_enable_data_checksums() does not exist | 
| MERGE / SPLIT PARTITION | Reverted (3e8bcc8644f) | syntax error at or near “MERGE” | 
| SQL/PGQ property graphs | Reverted (2b9e1aff4d3) | syntax error at or near “PROPERTY” | 
| GROUP BY ALL | Reverted (372b8d1adb7) | syntax error at or near “ORDER” | 
| JSON_ARRAY() / JSON_OBJECT() indexes over arrays, ranges and composites | Reverted (4b80cba35a1) | Existing indexes stay, pg_restore fails with 5 errors | 
| GROUPING SETS NULL handling | Fixed late | Wrong value on the first candidate, NULL now | 
| IGNORE NULLS with EXCLUDE | Fixed late | Endless loop at 99.7% CPU before, 0.266 ms now | 

Every row was measured on one e2-standard-2 cloud VM (2 vCPU, 8 GB RAM) with the PGDG packages. Timings are single runs, so read them as orders of magnitude rather than benchmarks. The JSON and query-fix rows compare the first and second release candidates on the same cluster, upgraded in place with apt.

## Step 1: Install the PostgreSQL 19 packages from PGDG

PGDG publishes PostgreSQL 19 builds in a separate `19` component, so the standard repository setup script is only half the job. Run the script first, then add the component to the sources file it writes. The full install walkthrough for other distributions is in our guide to installing PostgreSQL 19 on Ubuntu, Debian, Rocky Linux and AlmaLinux.

```
sudo apt install -y postgresql-common
sudo /usr/share/postgresql-common/pgdg/apt.postgresql.org.sh -y
sudo sed -i 's/^Components: main$/Components: main 19/' /etc/apt/sources.list.d/pgdg.sources
sudo apt update
sudo apt install -y postgresql-19 postgresql-19-jit
```
Keep `postgresql-19-jit` in the list. It is not optional if you plan to upgrade an 18 cluster that has `postgresql-18-jit` installed, because `pg_upgradecluster` refuses to start without it, as Step 9 shows. Confirm the installed build before running anything else:

```
apt policy postgresql-19
sudo -u postgres psql -XAtc 'SELECT version();'
```
Both the package version and the server string report the same build, and the compiler line shows the Ubuntu 26.04 toolchain:

```
postgresql-19:
  Installed: 19~rc2-1.pgdg26.04+1
  Candidate: 19~rc2-1.pgdg26.04+1
PostgreSQL 19rc2 (Ubuntu 19~rc2-1.pgdg26.04+1) on x86_64-pc-linux-gnu, compiled by gcc (Ubuntu 15.2.0-16ubuntu1) 15.2.0, 64-bit
```
Moving from the first to the second release candidate was a plain `apt upgrade`. The existing cluster restarted on the new binaries with no initdb and no dump, which is what made the before and after comparisons in Steps 7 and 8 possible on a single data directory.

## Step 2: Shrink a bloated table with REPACK CONCURRENTLY

REPACK replaces both VACUUM FULL and CLUSTER, and the CONCURRENTLY option rebuilds the table without holding an access exclusive lock while it copies. Changes made during the copy are tracked through a replication slot, which is why the release adds `max_repack_replication_slots`, set to 5 on the test box. We built a table with exactly 50% dead tuples to measure it:

```
CREATE TABLE bloated (id int PRIMARY KEY, data text);
INSERT INTO bloated SELECT g, repeat(md5(g::text), 10) FROM generate_series(1, 500000) g;
UPDATE bloated SET data = data || 'x';
SELECT pg_size_pretty(pg_relation_size('bloated')) AS before_repack;
\timing on
REPACK (CONCURRENTLY) bloated;
SELECT pg_size_pretty(pg_relation_size('bloated')) AS after_repack;
```
On the test box the heap went from 355 MB to 178 MB in just under 7 seconds, and all 500,000 rows were still there afterwards:

```
 before_repack
---------------
 355 MB
REPACK
Time: 6963.690 ms (00:06.964)
 after_repack
--------------
 178 MB
```
A more useful number is what a second session experiences. We rebuilt the same bloat, started the repack again, and two seconds in ran an INSERT and a query against `pg_stat_progress_repack` from another connection:

```
SELECT pid, phase, heap_blks_total, heap_blks_scanned FROM pg_stat_progress_repack;
INSERT INTO bloated VALUES (500001, 'written during repack');
```
Our insert returned in 124.669 ms while the repack was in its “seq scanning heap” phase across 45,455 blocks. The repack finished in 7,601 ms and the new row was in the rebuilt table:

VACUUM FULL would have blocked that INSERT for the whole rebuild. The cost moves to disk and WAL: the table exists twice during the copy, so check free space for one extra copy of the largest table before you schedule it. A table that changes heavily during the run also makes the replay phase longer, and that phase was too short to measure on 500,000 rows.

## Step 3: Enable parallel autovacuum and read the new scores

Parallel autovacuum is in the release, but the instance-wide cap `autovacuum_max_parallel_workers` defaults to 0, which disables it. The per-table storage parameter `autovacuum_parallel_workers` can only lower that cap, so setting it alone changes nothing. The parallelism covers the index vacuuming and cleanup phases, which means it only helps tables with several indexes:

```
SHOW autovacuum_max_parallel_workers;
CREATE TABLE big_events (id bigint PRIMARY KEY, a int, b text, c timestamptz);
CREATE INDEX ON big_events (a);
CREATE INDEX ON big_events (b);
CREATE INDEX ON big_events (c);
ALTER TABLE big_events SET (autovacuum_parallel_workers = 2);
SELECT relname, reloptions FROM pg_class WHERE relname = 'big_events';
```
PostgreSQL accepts the storage parameter, but with the cap at zero autovacuum still runs every index pass in one process:

```
 autovacuum_max_parallel_workers
---------------------------------
 0
  relname   |           reloptions
------------+---------------------------------
 big_events | {autovacuum_parallel_workers=2}
```
You can only set the cap in `postgresql.conf` or on the server command line, and the workers still come out of `max_parallel_workers`, so raise both together. The second new piece is `pg_stat_autovacuum_scores`, which exposes how urgently autovacuum rates each table and whether it plans to vacuum or analyze it:

```
SELECT relname, round(score::numeric, 2) AS score, do_vacuum, do_analyze, for_wraparound
FROM pg_stat_autovacuum_scores ORDER BY score DESC LIMIT 3;
```
During the lab run, the bloated table from Step 2 sat at 29.97 with both vacuum and analyze due, while the busiest catalog tables stayed below 1:

```
   relname    | score | do_vacuum | do_analyze | for_wraparound
--------------+-------+-----------+------------+----------------
 bloated      | 29.97 | t         | t          | f
 pg_class     |  0.53 | f         | f          | f
 pg_attribute |  0.33 | f         | f          | f
```
With this view you can answer the old question of why autovacuum has not touched a table yet without hand-computing thresholds from `pg_stat_user_tables`. It also splits the score into `xid_score`, `mxid_score`, `vacuum_score`, `vacuum_insert_score` and `analyze_score`, so a wraparound-driven run is visible before it starts. It is a good candidate for an exporter query if you already monitor PostgreSQL with Prometheus and Grafana.

## Step 4: Try the smaller SQL additions

Dozens of small changes are in the release notes. We ran the ones most likely to replace application code:

- **ON CONFLICT DO SELECT:** an upsert that returns the existing row instead of doing a dummy update, with optional FOR UPDATE or FOR SHARE.
- **random(min, max) for dates:** uniform random dates and timestamps without date arithmetic.
- **base64url and base32hex:** new formats for`encode()` and`decode()` , useful for URL tokens.
- **bytea to uuid:** a direct cast for 16-byte values.
- **COPY TO JSON:** exports rows as JSON objects, one per line.

ON CONFLICT DO SELECT is the one with the clearest payoff. Until now, getting the id of a row that already existed meant either a second SELECT or `DO UPDATE SET email = EXCLUDED.email`, which writes a new tuple version for nothing:

```
CREATE TABLE users (id serial PRIMARY KEY, email text UNIQUE);
INSERT INTO users (email) VALUES ('[email protected]');
INSERT INTO users (email) VALUES ('[email protected]')
  ON CONFLICT (email) DO SELECT RETURNING id, email;
```
Our second insert hits the unique constraint and returns the existing row, and the FOR UPDATE variant returned the same row while locking it:

```
 id |      email
----+-----------------
  1 | [email protected]
```
All four remaining additions fit in one session. The seed makes the random date repeatable, and the first two statements create the small table that COPY exports:

```
CREATE TABLE hosts (id int PRIMARY KEY, name text);
INSERT INTO hosts VALUES (1,'lb01'),(2,'web01'),(3,'db01');
SELECT setseed(0.19);
SELECT random('2026-01-01'::date, '2026-12-31'::date) AS random_day;
SELECT encode('\xfbff'::bytea, 'base64') AS base64, encode('\xfbff'::bytea, 'base64url') AS base64url, encode('hello'::bytea, 'base32hex') AS base32hex;
SELECT '\x0192f1a2b3c47d8e9f00112233445566'::bytea::uuid AS from_bytea;
COPY (SELECT * FROM hosts ORDER BY id) TO STDOUT (FORMAT json);
```
One pair of bytes encodes as `+/8=` in base64 and `-_8` in base64url, which is the whole point: no plus, slash or padding to escape in a URL. COPY wrote newline-delimited JSON rather than one array:

```
 random_day
------------
 2026-06-10
 base64 | base64url | base32hex
--------+-----------+-----------
 +/8=   | -_8       | D1IMOR3F
              from_bytea
--------------------------------------
 0192f1a2-b3c4-7d8e-9f00-112233445566
{"id":1,"name":"lb01"}
{"id":2,"name":"web01"}
{"id":3,"name":"db01"}
```
Newline-delimited output loads straight into most log and analytics tools. If a consumer expects one JSON array, the release notes document a `FORCE_ARRAY` option for COPY TO. One of our beta examples also broke for a different reason: `pg_stat_lock` has no `requests` column. Its columns are `locktype`, `waits`, `wait_time`, `fastpath_exceeded` and `stats_reset`.

## Step 5: Compare the defaults against PostgreSQL 18

Four defaults change between a fresh 18 cluster and a fresh 19 cluster, and one setting is new. We created one of each on the same VM and queried `pg_settings` with no configuration edits:

```
SELECT name, setting FROM pg_settings
WHERE name IN ('jit','default_toast_compression','log_lock_waits','max_locks_per_transaction','autovacuum_max_parallel_workers','io_method')
ORDER BY 1;
```
The 18 cluster returns no row for `autovacuum_max_parallel_workers` because the setting does not exist there:

Here are the measured values next to what each change means for an existing workload:

| Setting | PostgreSQL 18 | PostgreSQL 19 | What to do | 
|---|---|---|---|
| jit | on | off | Set `jit = on` again if large analytical queries relied on it | 
| default_toast_compression | pglz | lz4 | Only new values change; existing TOAST data keeps pglz | 
| log_lock_waits | off | on | Expect more log lines on lock-heavy workloads | 
| max_locks_per_transaction | 64 | 128 | Double any custom value to keep the same capacity | 
| autovacuum_max_parallel_workers | not present | 0 | Raise it to use parallel autovacuum | 
| io_method | worker | worker | Unchanged | 

The `max_locks_per_transaction` row is the one that bites during an upgrade. Lock size allocation changed, per the release notes. A custom value of 256 on 18 needs to become 512 on 19 for the same capacity. `pg_upgradecluster` copies the old configuration files, so a tuned value silently halves its effective capacity. JIT is the other change to plan for, since its costing was judged unreliable and reporting queries that gained from it will now run without it.

## Step 6: Confirm which beta features were pulled

Four headline features from the beta builds were reverted from the PostgreSQL 19 branch on the dates below, and a fifth change, JSON constructor indexability, was rolled back in November. We reran the exact statements from our earlier write-up and each of the four fails. The revert commits on the `REL_19_STABLE` branch give the reason, and the GROUP BY ALL revert names version 20 for another attempt:

| Feature | Revert commit | Date | Stated reason | 
|---|---|---|---|
| GROUP BY ALL | 372b8d1adb7 | 2026-07-17 | Wrong results with ORDER BY using a non-default equality | 
| ALTER TABLE MERGE / SPLIT PARTITION | 3e8bcc8644f | 2026-08-27 | Design issues too late to address in this cycle | 
| SQL/PGQ property graphs | 2b9e1aff4d3 | 2026-09-07 | Reverted from the release branch | 
| Online data checksum changes | c05d5ce1236 | 2026-09-16 | Several fixes needed during beta, risk of more after release | 
| Complete immutability tests in to_json and to_jsonb | 4b80cba35a1 | 2026-11-06 | Objects already created this way fail dump and restore; in our lab this hit JSON_ARRAY() and JSON_OBJECT() indexes (Step 8) | 

Two smaller additions were reverted on the same branch: UPDATE and DELETE FOR PORTION OF (a9d2f728240) and the `pg_get_role_ddl()` family of functions (db169985c10). Our beta write-up did not cover either, so we did not retest them. Online checksum functions fail as missing functions. The SQL features fail in the parser, so the error points at the keyword rather than at a missing object:

For operations, the checksum revert matters most. Checksums are still on by default for a new cluster, and `SHOW data_checksums` returned `on`, but a cluster that was initialised without them still needs the offline `pg_checksums --enable` run with the server stopped. Neither `pg_enable_data_checksums()` nor the `pg_stat_progress_data_checksums` view exists. Partition merge and split need the old detach, create and reattach sequence, and a change script written against the beta syntax will stop at its first statement.

## Step 7: Rerun the GROUPING SETS and IGNORE NULLS fixes

Two query bugs present in the first release candidate are fixed in the second, and both were easy to reproduce. The GROUPING SETS bug returns a value in a column that is not part of the current grouping set, where the result should be NULL. Our reproducer disables hash aggregation to force the sort-based path:

```
CREATE TABLE gs (id int PRIMARY KEY, a int, b int, c int);
INSERT INTO gs VALUES (1, 10, NULL, 100);
SET enable_hashagg = off;
SELECT a, b, c, GROUPING(a, b) AS g
FROM gs GROUP BY GROUPING SETS ((id, a), (id, b));
```
The second row belongs to the `(id, b)` set, so `a` must be NULL. The first candidate printed 10 there, and the same query on the upgraded cluster prints NULL:

A wrong subtotal label is the kind of bug that passes tests and ships in a report, so if you ran reporting queries against an earlier build, rerun them now. The second bug is worse operationally. `first_value()` or `nth_value()` with IGNORE NULLS and some EXCLUDE frame options looped forever:

`SELECT first_value(x) IGNORE NULLS OVER (ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING EXCLUDE GROUP) FROM (VALUES (1)) v(x);`
On the first candidate, the backend kept running after psql gave up. `pg_cancel_backend()` returned true, yet five seconds later the process was still active at 99.7% CPU, and only SIGKILL stopped it. The second candidate returns NULL in 0.266 ms:

`statement_timeout` is delivered through the same query cancel mechanism, so expect it to fail the same way. Killing a backend with SIGKILL makes the postmaster end every connection and run crash recovery, so on a shared server that one query takes everyone down. Anyone still running the first candidate in a test environment should upgrade the packages before testing window functions.

## Step 8: Find JSON_ARRAY and JSON_OBJECT indexes before you upgrade

Commit ecd9288624a made the immutability tests in `to_json` and `to_jsonb` complete by recursing into arrays, composites, ranges, multiranges and domains. Commit 4b80cba35a1 reverted it on November 6 and restored the earlier checks. The revert message warns that objects already created this way are not removed, but dump and restore or pg_upgrade will fail on them. Neither commit names the SQL/JSON constructors. Our lab shows they are where it bites: the first candidate accepted `JSON_ARRAY()` and `JSON_OBJECT()` over those types in index expressions and generated columns, and the second refuses them. On the first candidate we created two such indexes, over a `text[]` and an `int4range`, and one generated column:

```
CREATE TABLE docs (id int PRIMARY KEY, tags text[], span int4range);
CREATE INDEX docs_tags_json_idx ON docs ((JSON_ARRAY(tags RETURNING jsonb)));
CREATE INDEX docs_span_json_idx ON docs ((JSON_OBJECT('span' VALUE span RETURNING jsonb)));
CREATE TABLE docs_gen (id int, tags text[], tags_json json GENERATED ALWAYS AS (JSON_OBJECT('tags' VALUE tags)) STORED);
```
After the apt upgrade, the cluster started normally and the indexes were still listed in `pg_index`. Nothing warns you. Creating the same objects again fails, and so does restoring a dump of the database into a new one:

pg_restore reported five errors, not three, because the failed `CREATE TABLE` for the generated column cascades into its `ALTER TABLE ... OWNER` and its `COPY`, so the table and its rows are missing from the restored database. A nightly `pg_dump` that “succeeds” therefore produces a backup that cannot be fully restored, which is worth checking against your backup and point-in-time recovery runbook. Constructors over plain scalar columns are not affected: on the second candidate, an index on `JSON_OBJECT('id' VALUE id RETURNING jsonb)` over an `int` column was created without error, while the same `JSON_ARRAY(tags RETURNING jsonb)` index failed. This query lists every candidate in all three object types by matching on the deparsed expression:

```
SELECT 'index' AS kind, indexrelid::regclass::text AS object, pg_get_indexdef(indexrelid) AS definition
FROM pg_index
WHERE pg_get_indexdef(indexrelid) ~ 'JSON_(ARRAY|OBJECT)\('
UNION ALL
SELECT 'generated column', attrelid::regclass || '.' || attname, pg_get_expr(d.adbin, d.adrelid)
FROM pg_attribute a JOIN pg_attrdef d ON d.adrelid = a.attrelid AND d.adnum = a.attnum
WHERE a.attgenerated <> '' AND pg_get_expr(d.adbin, d.adrelid) ~ 'JSON_(ARRAY|OBJECT)\('
UNION ALL
SELECT 'partition key', partrelid::regclass::text, pg_get_partkeydef(partrelid)
FROM pg_partitioned_table
WHERE pg_get_partkeydef(partrelid) ~ 'JSON_(ARRAY|OBJECT)\(';
```
Run it in every database, since the catalogs it reads are per database, then check each hit for an argument of an array, composite, range, multirange or domain type. Hits over scalar columns can stay. On the lab cluster it found the two indexes and the generated column, all three over a container type. Our fix dropped the indexes, turned the generated column into a plain column that keeps its stored values, and replaced the lookup with a GIN index on the array itself:

```
DROP INDEX docs_tags_json_idx, docs_span_json_idx;
ALTER TABLE docs_gen ALTER COLUMN tags_json DROP EXPRESSION;
CREATE INDEX docs_tags_gin ON docs USING gin (tags);
```
Afterwards the detection query returned 0 rows. A fresh `pg_dump`, restored into a recreated target database, exited 0:

The fix only helps dumps taken after it. Backups made before the fix still carry the failing definitions, so restore them with a filtered list: write the table of contents with `pg_restore -l`, comment out the failing index and table entries, and pass the edited list back with `-L`. A table you skip this way also loses its rows, so create it by hand first with a plain column in place of the generated one, then run the filtered restore so its data entry loads into it. Do not swap the constructor for `to_jsonb()` in an index: `pg_proc.provolatile` for it is `s` (stable), so it fails the same immutability check. If the application queries the JSON form, index the source columns and build the JSON in the query. Partition keys cannot be altered, so a table partitioned on a JSON constructor has to be recreated and its rows copied into the new table.

## Step 9: Clear the objects pg_upgrade refuses to move

We upgraded an 18 cluster to 19 with `pg_upgradecluster --method=upgrade`, which wraps pg_upgrade on Debian and Ubuntu. The test database had btree_gist, pg_trgm and fuzzystrmatch installed, an inet column with a btree_gist index, a trigram index, a full text search index and a soundex index. The first run failed before pg_upgrade even started:

`sudo pg_upgradecluster --method=upgrade -v 19 18 app`
Debian’s wrapper checks that every version-specific package installed for 18 has a 19 counterpart, and the JIT package is now separate:

```
Error: pg_upgradecluster pre-upgrade checks failed
postgresql-18-jit is installed, but postgresql-19-jit is missing
1 package(s) from version 18 missing for version 19
Install the missing packages and try pg_upgradecluster again. To ignore the issue, use --force-packages
```
After `sudo apt install -y postgresql-19-jit`, the second run reached the consistency checks and stopped on a new one. The release notes explain why: the btree_gist inet and cidr opclasses can exclude rows that should be returned. The default GiST opclass for these types moves to the core one, and pg_upgrade now refuses clusters that still use the btree_gist versions:

On failure the wrapper removed the half-built 19 cluster and restarted 18, so the failure costs time but no data. Our lab created the offending index as `CREATE INDEX hosts_addr_btgist ON hosts USING gist (addr gist_inet_ops)`. You can have one without naming the opclass, because on 18 the btree_gist opclass is the default for inet and cidr once the extension is installed, which is the default the release notes say 19 changes. The test table already had a second GiST index on the same column built with the core `inet_ops` opclass, so on the 18 cluster we only had to drop the old one before retrying:

`DROP INDEX hosts_addr_btgist;`
Without a spare index, build the replacement first with `USING gist (addr inet_ops)` and drop the old one second, so queries are never left without one. The third run completed and ran the staged analyze. pg_upgrade also printed a notice and wrote `update_extensions.sql`, which held `ALTER EXTENSION "btree_gist" UPDATE;`, so run that file with psql as the superuser once the new cluster is up. Three more items need checking before you start, and none of them shows up as a failed index check:

- **RADIUS in pg_hba.conf:** support is removed. A`radius` line made`pg_hba_file_rules` report “invalid authentication method”, and a reload logged “pg_hba.conf was not reloaded”, so the server kept its old rules.
- **MULE_INTERNAL databases:** the encoding is removed, so those databases need a dump and restore into another encoding.
- **Tuned lock settings:** double a custom`max_locks_per_transaction` , as covered in Step 5.

A RADIUS line in a carried-over `pg_hba.conf` is the most dangerous of the three, because a reload that is refused leaves the running rules in place until the next restart, when the server fails to start. Replace RADIUS with LDAP or certificate authentication on 18, before the upgrade. On a Patroni cluster with HAProxy, check the generated `pg_hba` section in the Patroni configuration as well, since it rewrites the file.

## Step 10: Reindex trigram, text search and fuzzystrmatch indexes

pg_upgrade moves these indexes without complaint, but their contents may no longer match what 19 computes. The release notes recommend a reindex after pg_upgrade for two groups of functions:

- **Full text search and pg_trgm:** characters are now classified with the database’s locale provider instead of libc, which matters most on ICU or builtin databases whose LC_CTYPE differs.
- **fuzzystrmatch:** soundex, metaphone and difference treat only ASCII letters as letters, and dmetaphone upper-cases with the default collation.

Expression indexes on any of these functions are affected, as are trigram GIN and GiST indexes. This query lists the candidates in one database with their size, which is what decides the rebuild window:

```
SELECT indexrelid::regclass AS index, pg_size_pretty(pg_relation_size(indexrelid)) AS size
FROM pg_index
WHERE pg_get_indexdef(indexrelid) ~* '(gist|gin)_trgm_ops|to_tsvector|tsvector|soundex|metaphone|difference\('
ORDER BY 1;
```
REINDEX CONCURRENTLY keeps writes flowing, and on our 20,000-row table each index rebuilt in well under a second:

Measured times were 129.1 ms for the 1,208 kB trigram index, 107.3 ms for the 1,168 kB text search index and 16.8 ms for the 152 kB soundex index. Scale that by your index size to plan the window. Our database used the libc provider with `C.UTF-8`, which the release notes treat as the low-risk case, and we reindexed anyway because a mismatched index returns wrong rows rather than errors. Databases on ICU or the builtin provider should also recompute any stored `tsvector` columns.

## PostgreSQL 18 vs 19: upgrade now or wait

Moving now has a narrow but real case. REPACK CONCURRENTLY removes the maintenance window that VACUUM FULL forced on bloated tables, and the autovacuum scores make vacuum behaviour visible. Against that, the cycle ended with at least seven reverts and two wrong-result or hang fixes in its last weeks. Teams that are already on a fresh PostgreSQL 18 install on Ubuntu 26.04 lose nothing by waiting.

| Situation | Recommendation | Reason | 
|---|---|---|
| Tables need VACUUM FULL regularly | Test 19 now | REPACK CONCURRENTLY kept INSERTs at 124.7 ms during a rebuild | 
| Planned online checksums or partition merge | Stay on 18 | Both were reverted and are not in 19 | 
| Heavy analytical queries | Test with `jit = on` | JIT is now off by default | 
| btree_gist on inet or cidr | Replace the indexes first | pg_upgrade stops with a fatal check | 
| Any cluster that ran a 19 test build | Run the JSON query from Step 8 | Constructor indexes over arrays or ranges break dumps | 
| General production OLTP | Wait for the first minor release | Late-cycle fixes landed in the last weeks | 

What to watch before the first minor release: the release notes page and new commits on `REL_19_STABLE`. We would run the pg_upgrade rehearsal from Steps 8 to 10 against a copy of production now, because every blocker we hit was in the upgrade path rather than in the new features themselves.
