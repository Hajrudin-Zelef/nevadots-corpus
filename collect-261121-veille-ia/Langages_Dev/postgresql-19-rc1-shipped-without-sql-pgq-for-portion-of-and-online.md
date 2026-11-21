---
id: collect-261121-veille-ia/langages-dev/postgresql-19-rc1-shipped-without-sql-pgq-for-portion-of-and-online
title: "PostgreSQL 19 RC1 Shipped Without SQL/PGQ, FOR PORTION OF and Online"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-10-15"]
keywords: ["revert", "release", "postgresql", "portion", "beta", "features", "online", "support"]
source: https://errooe.com/en/articles/postgresql-19-rc1-reverted-features-verification
source_anchor: ""
source_lines: [1, 166]
section: "Langages_Dev"
---
The PostgreSQL 19 RC 1 announcement of 2026-10-15 says the release candidate "will be mostly identical to the initial release of PostgreSQL 19". It does not list which features that release no longer contains, and the PostgreSQL 19 release notes inside `REL_19_RC1` mention none of SQL/PGQ, `FOR PORTION OF`, `GROUP BY ALL` or `MERGE PARTITIONS`. We built the candidate from the official tarball and ran it; the gaps are larger than the announcements imply.

The Beta 1 announcement of 2026-06-04 promised SQL/PGQ support for "property graph queries using SQL standard syntax", `UPDATE` and `DELETE` support for the `FOR PORTION OF` clause, "the new `GROUP BY ALL` syntax", and data checksums that can be "enabled or disabled online, without requiring a cluster restart or reinitialization".

The Beta 4 announcement of 2026-09-24 reversed four of those items, stating that "This beta release reverted several features that were originally planned for PostgreSQL 19" and listing "Revert SQL/PGQ (property graph query) support", "Revert online enabling and disabling of data checksums", "Revert support for temporal updates/deletes provided by the `FOR PORTION OF` clause" and "Revert `ALTER TABLE ... MERGE PARTITIONS` and `ALTER TABLE ... SPLIT PARTITIONS`". The reason given is that the community "chose to remove some features from the PostgreSQL 19 release so they can potentially be included in a later major release".

The schedule then moved twice. The release team announcement of 2026-10-10 states that "The release team decided to move the PostgreSQL 19 GA to November 19, one week after the planned scheduled update release", and explains the second candidate: "the release team wanted to make those available in a PostgreSQL 19 RC2 that would be scheduled for November 12 before proceeding with the GA."

| Milestone | Date | What it changed | 
|---|---|---|
| Feature freeze | 2026-04-08 | The branch stops taking new features | 
| Beta 1 | 2026-06-04 | The advertised feature list | 
| Beta 4 | 2026-09-24 | Four features reverted, `REPACK` fixes, DDL functions removed | 
| RC 1 | 2026-10-15 | The candidate under test here | 
| RC 2 | 2026-11-12 | Scheduled, carries the November update-release fixes | 
| GA | 2026-11-19 | Planned general availability | 

Source: PostgreSQL Global Development Group announcements for Beta 1, Beta 4 and RC 1, the release team date change, and LWN.net for the feature freeze date.

The Register, writing on 2026-10-16, noted that PostgreSQL 19, "once slated for release in late September or early October", is "now expected to be generally available around November 19", and that "Developers have also dropped between eight and 10 significant features."

Counting commits whose subject begins with `Revert` gives 16 on the `REL_19_BETA1..REL_19_RC1` range, against 11 in the `REL_18_BETA1..REL_18_RC1` cycle and 5 in `REL_17_BETA1..REL_17_RC1`. Here is each one, with the file count from `git show --numstat`:

| Commit | Date | Author | Files | Subject | 
|---|---|---|---|---|
| `c05d5ce123` | 2026-09-16 | Daniel Gustafsson | 82 | Revert online data checksum transitions | 
| `a9d2f72824` | 2026-09-15 | Peter Eisentraut | 71 | Revert UPDATE/DELETE FOR PORTION OF | 
| `db169985c1` | 2026-09-13 | Andrew Dunstan | 10 | Revert pg_get_role_ddl(), pg_get_tablespace_ddl(), and pg_get_database_ddl() | 
| `e0fdc3f54b` | 2026-09-11 | Tom Lane | 9 | Revert "Don't try to re-order the subcommands of CREATE SCHEMA" | 
| `3c5d28ba64` | 2026-09-11 | Tom Lane | 11 | Revert "Support more object types within CREATE SCHEMA" | 
| `c58566d06f` | 2026-09-10 | Jeff Davis | 6 | Revert 87b2968df0 and 0a90df58cf | 
| `d0518cb3e4` | 2026-09-08 | Alexander Korotkov | 1 | Revert "Mark modified the FSM buffer as dirty during recovery" | 
| `2b9e1aff4d` | 2026-09-07 | Peter Eisentraut | 125 | Revert SQL Property Graph Queries (SQL/PGQ) | 
| `1956251afa` | 2026-09-01 | Jeff Davis | 6 | Revert "Force LC_COLLATE to C in postmaster" | 
| `3e8bcc8644` | 2026-08-27 | Alexander Korotkov | 29 | Revert support for ALTER TABLE ... MERGE/SPLIT PARTITION(S) commands | 
| `45b479a836` | 2026-08-05 | Jeff Davis | 5 | Revert "Validate subscription conninfo on owner change" | 
| `fd27c7e64f` | 2026-07-18 | Amit Kapila | 2 | Revert "Reject concurrent sequence refreshes" | 
| `372b8d1adb` | 2026-07-17 | Tom Lane | 12 | Revert "Add GROUP BY ALL" | 
| `b464e498ce` | 2026-07-15 | Michael Paquier | 3 | Revert "Rename routines for write/read of pgstats file" | 
| `7ca548f23a` | 2026-06-18 | Andrew Dunstan | 16 | Revert non-text output formats for pg_dumpall | 
| `a0354e29c4` | 2026-06-08 | Andrew Dunstan | 5 | Revert "Enable fast default for domains with non-volatile constraints" | 

Source: first-hand measurement on the postgres/postgres mirror, `git log --format='%h|%cI|%an|%s' REL_19_BETA1..REL_19_RC1` filtered on a `Revert` subject prefix, with file counts from `git show --numstat`.

Three of those commits removed whole features rather than patching them: the SQL/PGQ revert deleted 15,857 lines across 125 files, the `FOR PORTION OF` revert 12,938 lines across 71 files, and the data-checksum revert 6,673 lines across 82 files. Their messages give the reasons. The `GROUP BY ALL` revert states that "A postcommit review discovered that GROUP BY ALL missed our special handling of entries that also appear in an ORDER BY in the query. This caused the query to return wrong results when ORDER BY specifies non-default equality semantics." The data-checksum revert cites "a number of postcommit fixes during the beta period" and "the risk of more issues surfacing after GA."

We built PostgreSQL 19rc1 from `https://ftp.postgresql.org/pub/source/v19rc1/postgresql-19rc1.tar.bz2` with `--without-readline --without-icu --without-libxml --without-zlib` and ran a server on port 55432. The reverted syntax families fail at parse time; the retained features work:

```
ERROR:  syntax error at or near "MATCH"
LINE 1: SELECT * FROM GRAPH_TABLE (graph1 MATCH (n) COLUMNS (n.id));
ERROR:  syntax error at or near "FOR"
LINE 1: CREATE TABLE t1(a int); UPDATE t1 SET a=1 FOR PORTION OF t1;
ERROR:  syntax error at or near ";"
LINE 1: SELECT count(*) FROM (SELECT 1) t GROUP BY ALL;
ERROR:  syntax error at or near "MERGE"
LINE 1: ALTER TABLE t1 MERGE PARTITIONS;
ERROR:  parameter "data_checksums" cannot be changed
default_toast_compression | pglz
effective_wal_level|internal
password_expiration_warning_threshold|604800
```
| Feature | Status announced for Beta 1 / 4 | Result on our 19rc1 server | 
|---|---|---|
| SQL/PGQ `GRAPH_TABLE` | Advertised in Beta 1, reverted in Beta 4 | `syntax error at or near "MATCH"` | 
| `UPDATE` /`DELETE ... FOR PORTION OF` | Advertised in Beta 1, reverted in Beta 4 | `syntax error at or near "FOR"` | 
| `GROUP BY ALL` | Advertised in Beta 1, reverted in Beta 4 | `syntax error at or near ";"` | 
| `ALTER TABLE ... MERGE PARTITIONS` | Advertised in Beta 1, reverted in Beta 4 | `syntax error at or near "MERGE"` | 
| Online data checksum transitions | Advertised in Beta 1, reverted in Beta 4 | `data_checksums` is`PGC_INTERNAL` ;`ALTER SYSTEM SET` refused | 
| `REPACK` | Retained | Succeeds; `REPACK (VERBOSE)` reports`found 0 removable, 1 nonremovable row versions in 1 pages` | 
| `REPACK (CONCURRENTLY)` | Retained | Refused on `pg_class` with`This operation is not supported for system catalogs` | 
| `effective_wal_level` | Retained | Present, context `internal` | 
| `password_expiration_warning_threshold` | Retained | `604800` seconds | 
| `pg_stat_lock` | Retained | Present | 

Two results need care, because a plausible reading of them is wrong. `REPACK (CONCURRENTLY)` is not broken; its two preconditions produce an error rather than a wrong answer. And `default_toast_compression` reporting `pglz` is a property of our build rather than a removal: the release branch does contain the lz4 default change, and the header selects it only under `USE_LZ4`.

Commit `34dfca2934`, which changed the default to lz4, is an ancestor of both `REL_19_BETA1` and `REL_19_RC1`, so it reached the release branch. `configure.ac` makes the option opt-in with `PGAC_ARG_BOOL(with, lz4, no, ...)`, and our configure line carries no `--with-lz4`. A distribution build linking liblz4 reports `lz4`; ours reports `pglz`. The release notes in `REL_19_RC1` still describe the change, which is why lz4 is not among the four items Beta 4 reverted.

The reverts that removed whole features split cleanly by branch, and the split tells you what the next major release will contain.

| Feature | Reverted on `REL_19_STABLE` | Reverted on master | Present on master today | 
|---|---|---|---|
| SQL/PGQ | `2b9e1aff4d` , 2026-09-07 | `b1f106c80c` , 2026-09-07 | No | 
| `FOR PORTION OF` | `a9d2f72824` , 2026-09-15 | `a4b26b8f7c` , 2026-09-15 | No | 
| Online data checksum transitions | `c05d5ce123` , 2026-09-16 | Not reverted | Yes, `datachecksum_state.c` ships | 
| RI fast-path batching | `25649d6e79` , 2026-09-10 | `425daf545d` , 2026-10-03 | Removed, same change | 

Source: first-hand measurement on the postgres/postgres mirror, `git branch -a --contains <sha>` for each commit.

Two of the four removals were pushed upstream as well, so those features are gone from the development tree and will not arrive in a later major release as written. The other two exist only on the release branch: master still ships `src/backend/postmaster/datachecksum_state.c`, the online checksum transition worker, which `REL_19_RC1` does not have.

The RI fast-path case is where the removal was narrower than the feature name suggests. Commit `25649d6e79` removed 900 lines from `src/backend/utils/adt/ri_triggers.c` and 202 from `src/backend/commands/trigger.c`, but the fast path itself survives in RC1 as `ri_FastPathCheck`, `ri_FastPathProbeOne` and `ri_check_fastpath_index`. What went was the batching layer, which the commit message calls unsafe to ship: "Failure to handle one of those cases can leave a buffered check unperformed, allowing a transaction to commit a permanent foreign key violation without reporting an error." It buffered referencing rows across trigger invocations and flushed them at batch end, so a trigger state the flush did not handle lost the check entirely.

*The batch buffer is the step REL_19_RC1 removed: a referencing row waited in a 64-tuple buffer until the batch flushed, and a trigger state the flush missed left the check unperformed. The per-row path RC1 ships has no buffer between the row and the probe.* Source: first-hand measurement on the postgres/postgres mirror, commit 25649d6e79.

On 2026-08-25, Robert Haas opened a thread titled "scary patch contest" on the hackers list, writing that "I asked Claude to evaluate which v19 patches were the scariest based on the number and type of bugs fixed post-freeze." He ranked six patches by the post-freeze fixes each drew.

| Patch | Commits | Post-freeze fixes | Named defect classes | 
|---|---|---|---|
| RI fast-path FK checks | `2da86c1ef9b` ,`b7b27eb41a5` | about 16 | Out-of-bounds write on re-entry, crash under nested SPI, wrong-user-id index scan, five classes of incorrect FK enforcement | 
| REPACK / REPACK CONCURRENTLY | `ac58465e061` ,`28d534e2ae0` | 28 | Data loss from discarded missing values, broken toasted-tuple and generated-column handling, two ACL fixes | 
| Online data checksums | `f19c0eccae9` | about 25 | Invalid checkpoint and promotion transitions, initial state missing from the control file, basebackup false checksum failures, wrong WAL for unlogged relations | 
| `UPDATE` /`DELETE FOR PORTION OF` | `8e72d914c52` | 17 | Three of them security: leftover-row inserts bypassing INSERT RLS policies, unenforced `WITH CHECK OPTION` on DELETE, missing UPDATE permission on the range column | 
| SQL/PGQ property graphs | `2f094e7ac69` | 17 | Wrong collation, unresolved literals, broken LATERAL references, deparse and pg_dump ACL bugs | 
| `postgres_fdw` statistics import | `28972b6fc3d` | 7 | Syntax error, quoted-column deparsing bug, SPI removal, user-facing option rename | 

Haas also noted that REPACK CONCURRENTLY's prerequisite, database-specific logical replication snapshots commit `0d3dba38c777`, "was reverted as fundamentally flawed, leaving v19's REPACK CONCURRENTLY restricted to one process cluster-wide."

Peter Eisentraut, who developed SQL/PGQ and wrote its revert, put the count in context on 2026-10-09: "Having some reverts is not unusual, maybe one per cycle could be expected. But this time around, about 8 to 10 significant features, depending on how you count, have been reverted, and some of them quite late in the beta period." He attributes the pressure to several factors at once, including that "with LLM-assisted code review, these kinds of things can get found much faster, and then you're staring at a list of like forty defect reports." He rules out one explanation: "this situation is not due to 'vibe coding'. This code was written well before that was a possibility." He also names a competing cause, the Vulnpocalypse, which kept "many senior developers were busy between, say, April (the start of feature freeze for PostgreSQL 19, and also the start of the Vulnpocalypse) and August."

Snowflake's engineering blog treated the counting problem separately on 2026-09-16, counting "53 since the beta began in June 2025" and attributing the reversions to design flaws found in post-commit review, wrong results, or compatibility concerns discovered too late. Its rule differs from ours, and the count of 16 on the release branch differs again.

| Measure | v17 cycle | v18 cycle | v19 cycle | 
|---|---|---|---|
| Commits in the Beta 1 to RC 1 range | 374 | 458 | 894 | 
| Commits with a subject starting `Revert` | 5 | 11 | 16 | 
| Commits with a subject starting `Fix` | 93 | 90 | 247 | 
| Commits naming an AI tool in subject or body | 0 | 1 | 18 | 
| Of those, with a subject starting `Fix` | 0 | 0 | 4 | 

The v19 cycle carries roughly twice the commit volume of the previous one and roughly the same number of reverts per commit. The `Fix` count is the sharper signal, at 247 against 93 and 90 in the two prior cycles.

The reverts also cluster in time. Nine of the sixteen landed in September 2026, a month whose 195 commits carry nearly as many removals as the four preceding months combined.

*The removal pressure is concentrated in one month rather than spread across the beta; the branch returned to zero reverts in October, the month RC 1 shipped.* Source: first-hand measurement on the postgres/postgres mirror.

The AI-mentioning commits need the same care as the lz4 default. Of the 18 in the v19 cycle, 4 have a subject beginning with `Fix` and 14 do not; the single AI-mentioning commit in the v18 cycle is a documentation change. Several are bug fixes that such a tool located, including the hash-index bucket-split fix, whose message reads: "The committed test case is a simplified version of the one that it wrote to demonstrate this bug."

That is a correlation, and we cannot turn it into a cause. The reverts' own commit messages cite architectural defects, state-machine holes and wrong results, not AI assistance. Eisentraut frames LLM-assisted review as contributing pressure alongside insufficient time and the Vulnpocalypse, and LWN.net quotes Melanie Plageman wondering "if the ease with which LLMs allow people to pressure test features means we are finding more bugs sooner than we have in the past."

Three things can block an upgrade to PostgreSQL 19, and they are not equally serious.

*The two reverted syntax families and the missing online checksum transition are upgrade blockers; the lz4 toast default is a build-configuration difference that changes which compression algorithm you get, not which features you can use.* Source: first-hand runtime verification on PostgreSQL 19rc1 and the PostgreSQL 19 release notes.

If your application issues `GRAPH_TABLE`, `FOR PORTION OF` or `GROUP BY ALL`, or uses `MERGE PARTITIONS`, PostgreSQL 19 is not a candidate, and waiting for RC 2 does not change that: the removals are already in the release branch and two are also on master. The same holds if you change data checksums on a running cluster, since the transition machinery is not in the RC1 tree.

If you depend on lz4 TOAST compression, check your binary rather than the release notes. Run `SHOW default_toast_compression;`:

| Observation | Meaning | Action | 
|---|---|---|
| `lz4` | Your build links liblz4 and the default applies | None | 
| `pglz` | Your build was configured without `--with-lz4` | Rebuild with `--with-lz4` , or set the GUC per session | 
| `ALTER SYSTEM SET data_checksums = off` refused with`parameter "data_checksums" cannot be changed` | The online transition is gone from this release | Keep checksum changes offline via `pg_checksums` | 

For the features that survived, verify them on the candidate. These are the commands we ran against our 19rc1 server, with each result:

Three adoption paths exist, and each loses in a specific case.

| Path | Wins when | Loses when | 
|---|---|---|
| Adopt RC 1 now | You need a retained PostgreSQL 19 feature such as `REPACK` ,`pg_plan_advice` ,`WAIT FOR LSN` or`pg_stat_lock` | You need SQL/PGQ, `FOR PORTION OF` ,`GROUP BY ALL` ,`MERGE PARTITIONS` or the online checksum transition, because none returns in RC 2 or GA | 
| Wait for GA on 2026-11-19 | You can hold the release until the branch stops moving | Your workload needs a feature the project deferred to "a later major release", which for SQL/PGQ and `FOR PORTION OF` means a rewritten implementation on a branch that did not exist at the time of writing | 
| Stay on PostgreSQL 18 | You need none of the retained features and want a stable major version | You need the retained PostgreSQL 19 features, which are absent from PostgreSQL 18 | 

Source: the retained-feature list is from the PostgreSQL 19 release notes and our first-hand runtime verification on PostgreSQL 19rc1; the GA date is from the PostgreSQL 19 RC 1 announcement.

We could not benchmark the retained PostgreSQL 19 features. The release candidate was built without optional dependencies and no comparable PostgreSQL 18 instance was provisioned, so any timing comparison would be an unmeasured claim. The vendor statement that PostgreSQL 19 delivers "up to 2x better performance on inserts when foreign key checks are present" comes from the Beta 1 announcement and we did not measure it.

We cannot show that LLM-assisted review caused the reverts. The evidence is the correlation only, and Eisentraut, Haas and LWN.net frame AI as contributing pressure rather than sole cause.

We could not measure `default_toast_compression` on a PostgreSQL 19 build compiled with LZ4, because this host has the liblz4 runtime library but not its development headers, so we never observed the `lz4` default on a binary that has it.

The feature set RC 2 will ship is not knowable from here: RC 2 did not exist at the time of writing, and the open items page still carries items, whose count moved during our research window.

Two counting conventions differ across sources, and we report ours. Our 16 counts commits whose subject begins with `Revert` on the release branch; the union of that branch and master since the 19devel branch point gives 41 reverts on master and 43 distinct reverts across both. Snowflake's 53 uses a different window and rule, and neither number is more correct without the convention attached.
