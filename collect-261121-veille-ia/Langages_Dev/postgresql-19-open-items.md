---
id: collect-261121-veille-ia/langages-dev/postgresql-19-open-items
title: "PostgreSQL 19 Open Items"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["owner", "commit", "patch", "open", "postgresql", "items", "thread", "lane"]
source: https://wiki.postgresql.org/wiki/PostgreSQL_19_Open_Items
source_anchor: ""
source_lines: [1, 1125]
section: "Langages_Dev"
---
# PostgreSQL 19 Open Items

From PostgreSQL wiki

				
				
				Jump to navigationJump to search
				
				

## Important Dates

Current schedule:

- GA: November 19, 2026
- RC 2: November 12, 2026
- RC 1: October 15, 2026
- Beta 4: September 24, 2026
- Beta 3: August 13, 2026
- Beta 2: July 16, 2026
- Beta 1: June 4, 2026
- Feature Freeze: April 8, 2026 12:00 UTC

## Decisions to Recheck Mid-Beta

(none)

## Open Issues

**NOTE**: Please place new open items at the end of the list.

**NOTE**: Please list the Owner of the open item. If you're unsure who the Owner is, by default the owner should be the committer who made the target commit.

## Resolved Issues

### resolved before 19.1

- test: avoid redundant standby catchup in 049_wait_for_lsn
  - Commit: 447aae13b03
  - Owner: Alexander Korotkov
  - Fix: 19aa5713be2b

### resolved before 19.0

- BGWORKER_INTERRUPTIBLE workers are never restarted after DROP/ALTER/CREATE DATABASE
  - Commit: f1e251be80a0
  - Owner: Michael Paquier
  - Fix: 5bf919736b20

### resolved before 19rc2

- Text search and pg_trgm indexes from 18 miss rows after pg_upgrade (ICU or builtin provider with LC_CTYPE C)
  - Commit: e113f9c102b
  - Owner: Jeff Davis
  - Patch: in thread
  - Fix: 960626b936 123cad88f0 14870d5a18

- Eager aggregation runs a volatile join clause once per group
  - Commit: 8e11859102f
  - Owner: Richard Guo
  - Fix: 27c844034f827b3910bc8a358780c6609ba87a41

- ILIKE index scan misses rows with a libc UTF-8 collation on macOS, still there after b264391a846
  - Commit: 630706ced04
  - Owner: Jeff Davis
  - Patch: in thread
  - Fix: 75a5fef57d

- RI fast-path behavior diverges from the SPI path when FK value needs cast
  - Commit: 2da86c1ef9b
  - Owner: Amit Langote
  - Fix: 6aecabcd5ed54

- Parallel autovacuum: DROP DATABASE WITH (FORCE) fails on the parallel workers
  - Commit: 1ff3180ca01
  - Owner: Masahiko Sawada
  - Fix: a39e53a700c5b728f2f7dab964e33b157979fb3a

- psql backslash commands fail with translated messages and a non UTF-8 client encoding
  - Commit: 41d69e6dcca
  - Owner: Tom Lane
  - Patch: https://commitfest.postgresql.org/patch/7445/
  - Fix: e7ba5eb899884693ad8d9c62fa7a23f155a4a350

- Promotion and standby replay hang behind a backend blocked on a stuck client
  - Commit: 67c20979ce7
  - Owner: Masahiko Sawada
  - Fix: df1be4ddc8345235ffc736adc7dc954f0f944daa

- A canceled notify read leaves the NOTIFY queue stuck
  - Commit: 282b1cde9de
  - Owner: Tom Lane
  - Patch: in thread
  - Fix: 871dee0ccbe292cf4c2874aa7b460d242262d242

- CREATE SCHEMA puts an index or trigger on a same-named temp table, still there after 962e0a723
  - Commit: a9c350d9ee6
  - Owner: Tom Lane
  - Patch: in thread
  - Fix: b6dae498dfa60c9d15abf791d51434e8d23e5cae

- Hash join holds null-keyed outer rows until the outer scan ends, spilling to temp files and defeating LIMIT
  - Commit: 1811f1af98f
  - Owner: Tom Lane
  - Patch: in thread
  - Fix: 126976a8ffcfa47f40d3df09243ebf4606001912

- BUG #19759: Hashed SetOp / RecursiveUnion pre-allocate up to 64 GB from the row estimate since 8f29467c57f
  - Commit: 8f29467c57f
  - Owner: ~~Tom Lane~~ David Rowley
  - Patch: David's Proposed fix
  - Fix: 747dbf30d14804bac400729a670ba3e3e88c7879

- pg_*_advice: tsv load failure, etc.
  - Commit: 5883ff3
  - Owner: Robert Haas
  - Fix: dc33cca1859e2d2500ee8f3c0e564561428b4fcf
  - Fix: 1129570f654e96d45bbc63397079171af29710b9
  - Fix: 5e595d659fdb4829e910a1c4122ad4dec9dd0bd3
  - Fix: 63920eb099ab7e0e9d9eb49a3aa392c9441d52ef
  - Fix: c9c660e6ae03027615adff90cf4b26da5f8f97c1
  - Fix: e1d8f81d4961ca6c4228eb9a3ab5d36168008e42
  - Fix: 0f7dfd7936b9e99f9aeca00f52ab1692898ffff4
  - Fix: 8606fb3f449301cb6c7636eed9eecf5dd267e8fb
  - Fix: 38475f23c83358eafe42a410e4342eb8d64ba24c
  - Fix: f71a9969d58e3e70239fc51bbeae668db973b265
  - Fix: f6065c56d47eceb70c75de37a5ba214c67f73533
  - Fix: 3269ccc961189572ebd8cff4742461ed0d172105
  - Fix: a054b77e40d0579a9ee1218e2447d5af045d1083
  - Fix: 2a55a15d6b7c1374716df427e701c46c009fbe2d
  - Fix: 61553eacadbca0a084c7ae46214ffdee71208823

- Issue with new deforming code and GROUPING SETS
  - Commit: c456e391138
  - Owner: David Rowley
  - Fix: 04e0891fe0b8790b3c414bb5782b69d6491ff20b

- CHECK constraint enforced only by the child is not enforced on some paths
  - Commit: 0cd17fdd3c0
  - Owner: Andrew Dunstan
  - Patch: https://commitfest.postgresql.org/patch/7426/
  - Fix: 8356d8e4dba

- first_value()/nth_value() with IGNORE NULLS loop forever (EXCLUDE GROUP/TIES, frame past partition end, no ORDER BY)
  - Commit: 25a30bbd423
  - Owner: Tatsuo Ishii
  - Fix: 50f4c427224

- nodeWindowAgg.c IGNORE NULLS abs overflow
  - Commit: 25a30bbd423
  - Owner: Tatsuo Ishii
  - Fix: 24f39139668

- JSON_ARRAY()/JSON_OBJECT() expression indexes accepted since ecd9288624a can go stale
  - Commit: ecd9288624a
  - Owner: Andrew Dunstan
  - Fix: 4b80cba35a1 (reverted)

### resolved before 19rc1

- ON CONFLICT does not find a unique expression index on a NOT NULL column
  - Commit: 10c4fe074a9
  - Owner: Richard Guo
  - Fix: 62e46c00c657fc2e092ceec0cd79d16035798afe

- COALESCE on a NOT NULL column loses its collation and typmod
  - Commit: 10c4fe074a9
  - Owner: Richard Guo
  - Fix: d8ce0050a6cff3efefa5fce8abe3c6df7bd492ce

- NOT IN becomes an anti join over a NOT NULL virtual generated column that can read as NULL
  - Commit: 383eb21ebff
  - Owner: Richard Guo
  - Fix: d8444163d22e576c9f284a55eab7999ba67c4ceb

- pg_verifybackup never finishes on a gzip-compressed tar backup
  - Commit: b15c1513984
  - Owner: Andrew Dunstan
  - Fix: f884f359f5acb29bd423001b4878b85f51406b42

- RI fast-path behavior diverges from the SPI path in two other cases
  - Commit: 2da86c1ef9b
  - Owner: Amit Langote
  - Fix: c993d9f7c8
  - Fix: 061065e28f

- RI fast path doesn't check EXECUTE privilege on functions it invokes
  - Commit: 2da86c1ef9b
  - Owner: Amit Langote
  - Fix: f8eb70f05062

- REPACK (CONCURRENTLY) loses missing values of columns added without a rewrite
  - Commit: 28d534e
  - Owner: Álvaro Herrera
  - Fix: a4052efe2aa

- RI fastpath doesn't call ExecutorCheckPerms_hook
  - Commit: 2da86c1ef9b
  - Owner: Amit Langote
  - Fix: 5aeb36f3273

- REPACK (CONCURRENTLY) automates without warning: Race conditions in logical decoding
- Race conditions in logical decoding

- Add REPACK progress phases for logical decoding setup
  - Commit: 28d534e
  - Owner: Álvaro Herrera
  - Fix: bd2aebd7a7fe38b

- REPACK (CONCURRENTLY) loses updates when the TOAST relation is rewritten during startup
  - Commit: 28d534e
  - Owner: Álvaro Herrera

- self-join elimination drops RLS qual across view permission contexts
  - Commit: 2ebf25e (back-patched; open item exists since v19 releases first)
  - Owner: Tom Lane
  - Fix: dca6a9e320e0272f0ea8d7e076cda04b06050866

- 64-bit MultiXactOffset vs. 9.3->9.4 upgrade, pg_resetwal, "wraparound" msg
  - Commit: bd8d9c9
  - Owner: Heikki Linnakangas
  - Fix: fcc0e27f45e2d8dc6046435908a48626c91d3d84
  - Fix: 7b879c485243e61a0d4cb169717a8e96e9e875c2
  - Fix: 4545cee303c257e58195e3d033c05bf38e2cd4d6
  - Fix: b69356cd789fe963447177a0077c12c6c322c203

- Two remaining shmem attachment issues in single-user mode
  - Commit: bd8d9c9
  - Owner: Heikki Linnakangas
  - Fix: 4e277b424a5312cdee468d92078a5359f3e7d4e7
  - Fix: c712b0d49b971fe7cf521c057bc7adb63b17760c

- Parallel autovacuum leader doesn't propagate cost parameter changes while waiting for workers
  - Commit: 1ff3180ca01
  - Owner: Masahiko Sawada
  - Fix: 42e96cf2fe095c822

- Session in aborted transaction misses effective_wal_level change
  - Commit: 67c20979ce7
  - Owner: Masahiko Sawada
  - Fix: 10b6e2a3d66098bf26b236c880bab5b3fafc42bd

- Issue in slotsync when logical decoding is disabled and re-enabled
  - Commit: 67c20979ce72b8
  - Owner: Masahiko Sawada
  - Fix: 1f2245371e31f0def347546f5ad250552a4eb8a4

- COPY (query) TO ... (FORMAT json) uses the table's column names
  - Commit: 7dadd38cda9
  - Owner: Andrew Dunstan
  - Fix: c652cd3f94d

- Fix a wal_debug crash with the new shmem allocation API
  - Commit: 9b5acad3f40f
  - Owner: Heikki Linnakangas
  - Fix: 6873a138f7

### resolved before 19beta4

- RI fastpath misses pg_amop updates
  - Commit: 2da86c1ef9b
  - Owner: Amit Langote
  - Fix: c86738fd4a

- RI fastpath handles permissions incorrectly
  - Commit: 2da86c1ef9b
  - Owner: Amit Langote
  - Fix: 2fc654e1133

- Improve WAIT FOR read-your-writes consistency doc
  - Commits: 447aae13b030
  - Owner: Alexander Korotkov
  - Fix: 2af020eacb8

- Reject WAIT FOR earlier in transaction-snapshot mode
  - Commits: 447aae13b030
  - Owner: Alexander Korotkov
  - Fix: 26a3c0a45cc

- Routed ON CONFLICT inserts broken by partition-local deferrable unique constraints in 19 and master
  - Commits: 2bc7e886fc1b, 90eae926abbb
  - Owner: Álvaro Herrera
  - Fix: 67fd7002b6d

- REPACK (CONCURRENTLY) decoding worker is canceled by lock_timeout
  - Commit: 28d534e2ae0a
  - Owner: Álvaro Herrera
  - Fix: 746a507a391

- REPACK decoding worker startup/teardown differs from parallel.c
  - REPACK (CONCURRENTLY) backend waits indefinitely when decoding worker fails to start
  - Avoid a REPACK worker startup hang
  - Commit: 28d534e
  - Owner: Álvaro Herrera
  - Fix: 47b5b23a78

- Offline data checksum changes can cause incorrect checksum state on standbys
  - Commit: f19c0eccae96
  - Owner: Daniel Gustafsson
  - Fix: c05d5ce1236 (Revert)

- CLUSTER progress: wrong index_rebuild_count for tables with TOAST
  - Commit: 28d534e
  - Owner: Álvaro Herrera
  - Fix: 4b445479f9e

- REPACK (CONCURRENTLY) doesn't handle invalid indexes
  - Commit: 28d534e
  - Owner: Álvaro Herrera
  - Tentative fix: e14ae14a235
  - Fix: 0cabab44f1

- REPACK (CONCURRENTLY) fails when replica identity index is dropped
  - Commit: 28d534e
  - Owner: Álvaro Herrera
  - Fix: 434bde9ce6b

- RI fastpath handles mismatching collations incorrectly
  - Commit: 2da86c1ef9b
  - Owner: Amit Langote
  - Fix: 0b0b4132921

- Identifier downcase change for LATIN1, in v19
  - Commit: 87b2968
  - Owner: Jeff Davis
  - Fix: c58566d06f

- REPACK docs have the b-tree requirement backwards
  - Commit: ac58465
  - Owner: Álvaro Herrera
  - Fix 07aef52eba

- RI fast-path batching fails during nested SET CONSTRAINTS
  - Commit: 6fc2a486417d
  - Owner: Amit Langote
  - Fix 25649d6e791c

- RI fast-path batching wrongly used by ALTER TABLE inside a trigger
  - Commit: b7b27eb41a5
  - Owner: Amit Langote
  - Fix 25649d6e791c

- Stale snapshot in RI fast-path leads to spurious failure
  - Commit: 2da86c1ef9b
  - Owner: Amit Langote
  - Fix 139018268375

- EUC_* ILIKE index scan stopped matching seq scan in v19
  - Commit: 630706c
  - Owner: Jeff Davis
  - Fix b264391a84

- plperl LC_COLLATE change: revert would be the better thing, else add missing release note
  - Commit: 5e6e42e
  - Owner: Jeff Davis
  - Fix 1956251afa

- REPACK (CONCURRENTLY) fails with wrong error for materialized views
  - Commit: 28d534e
  - Owner: Álvaro Herrera
  - Fix 412ef97d925

- REPACK (ANALYZE) within transaction block segfaults
  - Commit: ac58465
  - Owner: Álvaro Herrera
  - Fix 25b21c0bb71

- AfterTriggerEndSubXact() vs. firing_depth
  - Commit: 5c54c3e
  - Owner: Amit Langote
  - Fix e2c812f1475

- Tighten ACL check in repack_is_permitted_for_relation()
  - Commit: 28d534e2ae0ac888b5460f977a10cd9bb017ef98
  - Owner: Álvaro Herrera
  - Fix e13851080c

- InvalidateConstraintCacheCallBack() can free fpmeta while it's in use
  - Commit: e484b0eea614
  - Owner: Amit Langote
  - Fix 170c9344defa

- Fast-path FK metadata leaks the FmgrInfo scratch state on invalidation
  - Commit: 2da86c1ef9b5
  - Owner: Amit Langote
  - Note: to be committed with the fix for "InvalidateConstraintCacheCallBack() can free fpmeta while it's in use"
  - Fix 590b8e89a53

- pg_control_checkpoint(): add "data_checksum_version" (Pg19
  - Commit: f19c0eccae
  - Owner: Daniel Gustafsson
  - Fix 397f0fd06ed

- Re-read conindid under the referenced table's lock in the RI fast path
  - Commit: b7b27eb41a5
  - Owner: Amit Langote
  - Fix d02a9084d91

- ri_FastPathFlushArray() gets cross-type foreign keys wrong
  - Commit: b7b27eb41a5
  - Owner: Amit Langote
  - Fix cc449a504bfe

- pgbench --continue-on-error: clarify TPS and failure reporting
  - Commit: 0ab208fa505
  - Owner: Fujii Masao
  - Fix bcb15f48786

- use ANALYZE ONLY when analyzing a partitioned table
  - Commit: 6429e5b771d
  - Owner: Fujii Masao
  - Fix 72e6184cd5f

- CLUSTER (ANALYZE) shouldn't be accepted
  - Commit: ac58465e06
  - Owner: Álvaro Herrera
  - Fix ac5cea86e8

- DELETE FOR PORTION OF bypasses view WITH CHECK OPTION for leftover rows
  - Commit: 8e72d914c52
  - Owner: Peter Eisentraut / Paul Jungwirth
  - Fix f3a116a26ff

- Limit GRAPH_TABLE path combinations to prevent memory exhaustion
  - Commit: 2f094e7ac69
  - Owner: Peter Eisentraut
  - Fix 41a300ae3df

- Clarify or fix SIGINT handling in data checksums launcher
  - Commit: f19c0eccae96
  - Owner: Daniel Gustafsson
  - Fix ed6775db84db7d28c787bbe4b9e8989f3e8f107a

- SIGSEGV in dynahash
  - Commit: 9fe9ecd516b09434df59d6e7396d508f9c4477c8
  - Owner: Heikki Linnakangas
  - Fix 86481d9f4930750a9c62c25216bf7783f69d6ff0

- RegisterShmemCallbacks() does nothing in single-user mode
  - Commit: 283e823f9dc
  - Owner: Heikki Linnakangas
  - Fix edc095a2652971d6b2a95082c22dbdf34fc01d30

- MERGE/SPLIT PARTITIONS issues
  - Commit: 4b3d173629f4
  - Owner: Alexander Korotkov
  - Fix 7612ae8 (reverted)

- pg_plan_advice: fix empty FOREIGN_JOIN sublist validation
  - Commit: 5883ff30b02ceed3c5eabba4d9c09a7766f9a8fc
  - Owner: Robert Haas

- Error handling in after-startup shmem requests
  - Commit: 283e823f9dc
  - Owner: Heikki Linnakangas
  - Fix c54b6979e033c6182518d4b33feb35771ae738ae, 07fdee7c8a8b415fb3a2991e7aea34f08975d445

- pg_stat_get_autovacuum_scores ignores the main table's reloptions for TOAST tables
  - Commit: 87f61f0c82
  - Owner: Nathan Bossart
  - Fix 7ddb9c4

- COPY FROM SIMD read-ahead bug
  - Commit: e0a3a3fd53
  - Owner: Nathan Bossart
  - Fix: 348f4ab

- autovacuum scoring corner case
  - Commit: d7965d65fc
  - Owner: Nathan Bossart
  - Fix: 49e098b

- re-add redundant filter clause to stats query
  - Commit: 4b5ba0c4ca
  - Owner: Nathan Bossart
  - Fix: a0edc1c

- initdb: pad rewritten GUC lines with spaces instead of tabs
  - Commit: f63ae72bbc
  - Owner: Nathan Bossart
  - Fix: d870be1

- pg_event_trigger_ddl_commands can't process GRANT ON PROPERTY GRAPH
  - Commit: 2f094e7
  - Owner: Peter Eisentraut
  - Fix: 63f8e9773aa

- FOR PORTION OF assertion failure in ExecInitPartitionInfo()
  - Commit: 8e72d91
  - Owner: Peter Eisentraut
  - Fix: 938f4f8f343

- GRAPH_TABLE pattern WHERE clause is not coerced to boolean
  - Commit: 2f094e7ac691abc9d2fe0f4dcf0feac4a6ce1d9c
  - Owner: Peter Eisentraut
  - Fix: 1c8c790ec10

- Correct some doc items due to the REPACK
  - Commit: ac58465
  - Owner: Álvaro Herrera
  - Fix: 872b20aa5649f9b90781961cbc404409783dab2b

- GRAPH_TABLE issues with aggregates and subqueries
  - Commit: 2f094e7ac
  - Owner: Peter Eisentraut
  - Fix: 2b9e1aff4d3 (reverted)

- PGQ catalog representation and pg_dump support
  - Commit: 2f094e7ac
  - Owner: Peter Eisentraut
  - Fix: 2b9e1aff4d3 (reverted)

- FOR PORTION OF bounds not coerced in PREPARE
  - Commit: 8e72d914c52
  - Owner: Peter Eisentraut / Paul Jungwirth
  - Fix: f1940321687

- EXPLAIN ... FOR PORTION OF should not evaluate bounds
  - Commit: 8e72d914c52
  - Owner: Peter Eisentraut / Paul Jungwirth
  - Fix: 98f6984e5c0

- replicating v18->v19 w/ publish_via_partition_root fails at tablesync
  - Commit: 266543a
  - Owner: Masahiko Sawada
  - Fix 11fb89fdbd31

- REPACK (CONCURRENTLY) doesn't check the table AM
  - Commit: 28d534e
  - Owner: Álvaro Herrera
  - Fix: e8636f2bed258ca5be115aca33de00346d687de4

- REPACK (CONCURRENTLY) fails when table owner lacks CONNECT
  - Commit: 28d534e
  - Owner: Álvaro Herrera
  - Fix: 0717ed2623683cbbe206d0b651817a3a1e0cf0de

- REPACK (CONCURRENTLY) rewrites tables marked with user_catalog_table
  - Commit: 28d534e
  - Owner: Álvaro Herrera
  - Fix: 86f7c82cf1023e3599f40f939727791a7090cd44

- Segmentation fault planning self-join IN subquery with LATERAL UNION ALL
  - Commit: e2debb64, fc069a3
  - Owner: Alexander Korotkov
  - Fix: 0ab90a5c9418

- Change to meaning of CREATE SCHEMA statements that v18 already accepts (proposal needs votes)

- REPACK (CONCURRENTLY) can crash a logical decoding session
  - Commit: 28d534e
  - Owner: Álvaro Herrera
  - Fix: 7164dcb6653242f757b5bdc4a26714a24207b1db

- Deadlock problems with WAIT FOR LSN
  - Commit: 447aae13b030
  - Owner: Alexander Korotkov
  - Fix: e0c160c307264

- pg_get_*_ddl() needs a redesign
  - Commit: 76e514e
  - Owner: Andrew Dunstan
  - db169985c10 (reverted)

- Crashes on a partition whose concurrent detach never finished
  - Commit: fd36606
  - Owner: Amit Kapila
  - Fix: 090b44557d4

- timeout value overflow in wait for lsn
  - Commit: 447aae1
  - Owner: Alexander Korotkov
  - Fix: f260c03bccb7

- Divergence with all-visible, all-frozen and freespace handling post-promotion with CREATE DATABASE WAL_LOG
  - Commit: add323da40a
  - Owner: Melanie Plageman
  - Fix: 3245795e13c

- Review items for EXCEPT TABLE publication
  - Commit: fd36606
  - Owner: Amit Kapila
  - Fix: 5624de9904c
  - Fix: 7056ed4663f

- FOR PORTION OF leftovers not inserted if BEFORE INSERT trigger returns NULL
  - Commit: 8e72d914c52
  - Owner: Peter Eisentraut / Paul Jungwirth
  - Fix: a9d2f728240 (reverted)

- FOR PORTION OF leftovers not inserted with DO INSTEAD rule
  - Commit: 8e72d914c52
  - Owner: Peter Eisentraut / Paul Jungwirth
  - Fix: a9d2f728240 (reverted)

- FOR PORTION OF elog on non-polymorphic opcintype
  - Commit: 8e72d914c52
  - Owner: Peter Eisentraut / Paul Jungwirth
  - Fix: a9d2f728240 (reverted)

- FOR PORTION OF leftovers should fire triggers on the child table
  - Commit: 8e72d914c52
  - Owner: Peter Eisentraut / Paul Jungwirth
  - Fix: a9d2f728240 (reverted)

- FOR PORTION OF memory leak checking domains
  - Commit: 8e72d914c52
  - Owner: Peter Eisentraut / Paul Jungwirth
  - Fix: a9d2f728240 (reverted)

- WAIT FOR NO_THROW option could use some documentation
  - Commit: 447aae13b03
  - Owner: Alexander Korotkov
  - Fix: 3f8b8ddea08a

- Improve WAIT FOR read-your-writes consistency doc
  - Commit: 447aae13b030
  - Owner: Alexander Korotkov
  - Fix: a203d2019005

- Publication EXCEPT clause errors double-quote qualified relation names
  - Commit: a49b9cfd72d
  - Owner: Amit Kapila
  - Fix: 926627bf90211

- WAIT FOR reports a misleading isolation-level error for cursors and exported snapshots
  - Commit: 447aae13b030
  - Owner: Alexander Korotkov
  - Fix: 26a3c0a45cc6

### resolved before 19beta3

- ri_Fast* crash w/ nullable UNIQUE constraint
  - Commit: 2da86c1
  - Owner: Amit Langote
  - Fix: 0ec3f048bf

- Remove the assertions added by commit 7dcea51c2a4d
  - Removed by commit cb2053dbde7a7

- CREATE SUBSCRIPTION ... SERVER vs. pg_dump etc.
  - Commit: 8185bb5
  - Owner: Jeff Davis
  - Fix: 7f4fb05807 (and earlier commits in series)

- REASSIGN OWNED vs. relisshared dep on !relisshared
  - Commit: 8185bb5
  - Owner: Jeff Davis
  - Fix: e727c6f2ef

- postgres_fdw ArrayCoerceExpr-specific wrong query results
  - Commit: 62c3b4c
  - Owner: Alexander Korotkov
  - Fix: 2349b106b666

- Fix GROUP BY ALL handling of ORDER BY operator semantics
  - Commit: ef38a4d97
  - Owner: Tom Lane
  - Fix: 372b8d1adb

- pg_plan_advice: fix parsing underscore in numbers
  - Commit: 5883ff30b02
  - Owner: Robert Haas
  - Fix: f87375bfba

- fix FOR PORTION OF with RLS
  - also reported here
  - Commit: 8e72d914c528
  - Owner: Peter Eisentraut
  - Fix: c58c83ce50c

- FOR PORTION OF should forbid BEFORE triggers changing the application-time
  - You aren't allowed to SET this in an UPDATE; you shouldn't be allowed to change it with a BEFORE trigger either.
  - Commit: 8e72d914c
  - Owner: Peter Eisentraut / Paul Jungwirth
  - Fix: 5454fe4a6e7

- Fix missing FORMAT when deparsing JSON_ARRAY(query)
  - Commit: 8d829f5a0
  - Owner: Richard Guo
  - Fix: f4c850d11afc60a4fc4bc782fc40d7b752bf8d7f

- Fix RETURNING side effects for FOR PORTION OF leftover rows
  - Commit: 8e72d914c528
  - Owner: Peter Eisentraut

- Fix "unexpected logical decoding status change" error; from concurrent logical decoding activation
  - Commit: 67c20979ce7
  - Owner: Masahiko Sawada
  - Fix: 6aba42c660c
  - Fix: 0fd30e2119e

- sequencesync worker race with REFRESH SEQUENCES
  - Commit: 5509055
  - Owner: Amit Kapila
  - Fix: 45cf7b1e5bf
  - Fix: 4cd02d55226
  - Fix: c12c101b084
  - Fix: 57e7c52c0fe
  - Fix: 55f51868489
  - Fix: 13b7a8a0ef5
  - Fix: b8d9cf512c1
  - Fix: 00b3e50054d

- Fix tripped assert from foreign-IO wait time being recorded with a zero read count
  - Commit: 999dec9ec6a
  - Owner: Melanie Plageman
  - Fix: c9a66949271



### resolved before 19beta2

- UTF8 LIKE behavior change in refactor commit; 2 other findings
  - Commit: 5a38104
  - Owner: Jeff Davis
  - Fix: ef5d080b45
  - Fix: dffd7fb0a0
  - Fix: 404fe01e0c

- NULL pointer dereference in syslogger with load_libraries() and -DEXEC_BACKEND at startup
  - Commit: 0c8e082fba8d
  - Owner: Alvaro Herrera
  - Fix: fb23cc7e81db181bfb3dcfed6ad0731a3473d4e1

- Fix regression in vacuumdb --analyze-in-stages for partitioned tables
  - Commit: c4067383cb2c
  - Owner: Alvaro Herrera
  - Fix: 95b6ec52e684e5de743e5bf4a2303f6cac466682

- SERVICEFILE shows wrong file after servicefile fallback
  - Commit: 092f3c63efc6
  - Owner: Michael Paquier
  - Fix: 90354030b8fc

- Fix tuple deformation with virtual generated NOT NULL columns
  - Commit: c456e3911
  - Owner: David Rowley
  - Fix: 89eafad297a9b01ad77cfc1ab93a433e0af894b0

- pg_createsubscriber: allow duplicate publication names
  - Commit: 85ddcc2f4cde
  - Owner: Amit Kapila
  - Fix: 6ce035ffff444479e63ea6276d56e33cc8b991d9

- Fix REPACK decoding worker not cleaned up on FATAL exit
  - Owner: Álvaro Herrera
  - Commit: 28d534e2ae0ac888b5460f977a10cd9bb017ef98
  - Fix: 0160143ad9a686a7e705348da1f90e63a2a31536

- Fix bug of UPDATE/DELETE FOR PORTION OF with inheritance tables
  - When update/delete a inherited table, leftover rows are wrongly inserted into the parent table.
  - Commit: 8e72d914c528
  - Owner: Peter Eisentraut
  - Fix: 7d13b03a2e677b78047cf537e519166dcbafa117

- Fix race during concurrent logical decoding activation
  - Commit: 67c20979ce7
  - Owner: Masahiko Sawada
  - Fix: 93a3e6839bf8d

- Reject negative max_retention_duration values
  - Commit: a850be2fe
  - Owner: Amit Kapila
  - Fix: 987440b33a511482232c59a190cc16ae4feff9aa

- Inconsistent trigger behavior between two temporal leftovers
  - Commit: 8e72d914c52
  - Owner: Peter Eisentraut / Paul Jungwirth

- fix FOR PORTION OF not updating dependent generated columns
  - Commit: 8e72d914c528
  - Owner: Peter Eisentraut

- Fix md5_password_warnings for role and database settings
  - Commit: bc60ee860
  - Owner: Nathan Bossart
  - Fix: 44196fd4f378d776c4cd7282256f4d6df03b5734

- BUG #19508: pg_buffercache_pages() crashes the backend with an incompatible caller-supplied record definition
  - Commit: 257c8231bf
  - Owner: Heikki Linnakangas
  - Fix: b70d5672d0c5d4310965fcbc1c846c9d111a3046

- Miscellaneous issues with FK fast path buffering
  - Commit: b7b27eb41
  - Fix: 0e47bb5fbeec64d776d49dee242bac39d4616f8b
  - Fix: 4113873a5ab0fb83a6f772f455b2842359d5ec50
  - Owner: Amit Langote

- Bug in ALTER SUBSCRIPTION ... SERVER / ... CONNECTION with broken old server
  - Commit: 8185bb53476
  - Owner: Jeff Davis
  - Fix: e5c40584a712e8f8c6bd9a7a87bdb855cfd2f20b

- Fix DROP PROPERTY GRAPH "unsupported object class" error
  - It's failing when an event trigger is active.
  - Commit: 2f094e7ac69
  - Owner: Peter Eisentraut

- (SQL/PGQ) cache lookup failed for label
  - Commit: 2f094e7ac69
  - Owner: Peter Eisentraut

- Assertion failure in LATERAL GRAPH_TABLE with multi-label pattern
  - Commit: 2f094e7ac69
  - Owner: Peter Eisentraut

- document standard_conforming_strings dump/restore incompatibility
  - Owner: Tom Lane
  - Commit: 45762084545e
  - Fix: 62c09cdc1

- autovacuum launcher crash: assert in pgstat_count_io_op (IOOP_EXTEND on pg_database's VM)
  - Commit: b46e1e54d078
  - Owner: Melanie Plageman
  - Fix: 4abf411e232

- bytea(uuid) missing proleakproof
  - Commit: ba21f5bf8 ba21f5bf8
  - Owner: Masahiko Sawada
  - Fix: 6468f7a853c

- Improving EXPLAIN (IO) docs
  - Commit: 681daed93169
  - Owner: Tomas Vondra

- non-plain pg_dumpall: multiple defects
  - Owner: Andrew Dunstan
  - Commit: 763aaa0
  - Fix(revert): 7ca548f23a6

- Fix bug of CHECK constraint enforceability recursion
  - Commit: 342051d73b38
  - Owner: Andrew Dunstan
  - Fix: d16be8605f5

- What to do about Fix \crosstabview to honor \pset display_true/display_false?
  - Commit: 645cb44c5 645cb44c5
  - Owner: Álvaro Herrera
  - Fix: 4df5fe3833a87f6629eb888ca5a385bcb0b179d9

- transformForPortionOfClause performs various checks too early
  - It mustn't check properties that could change between parsing and execution of a function containing FOR PORTION OF.
  - Commit: 8e72d914c
  - Owner: Peter Eisentraut / Paul Jungwirth
  - Fix: a272a58b942, a40fdf65886

- Fix ALTER DOMAIN VALIDATE CONSTRAINT locking
  - Commit: 16a0039dc
  - Owner: Peter Eisentraut
  - Fix: 64797ad97d6

- FOR PORTION OF should not allow WHERE CURRENT OF
  - Commit: 8e72d914c
  - Owner: Peter Eisentraut / Paul Jungwirth
  - Fix: bc3ae886a75

- FOR PORTION OF should reject GENERATED columns
  - Commit: 8e72d914c52
  - Owner: Peter Eisentraut / Paul Jungwirth
  - Fix: c8d49ffb007

- SECURITY LABEL ON PROPERTY GRAPH: documented as supported, but ERROR say it's not
  - Commit: 2f094e7
  - Owner: Peter Eisentraut
  - Fix: 33bfad0f3ca

- Fix SET EXPRESSION for virtual columns with whole-row dependencies
  - Commit: f80bedd52
  - Owner: Peter Eisentraut
  - Fix: 95308989403

- Wrong query result w/ propgraph single lateral col reference
  - Commit: 2f094e7
  - Owner: Peter Eisentraut
  - Fix: 7883ea13ffd

- FOR PORTION OF should skip leftovers after INSTEAD OF trigger
  - Originally reported here.
  - Commit: 8e72d914c52
  - Owner: Peter Eisentraut / Paul Jungwirth
  - Fix: 5b5e99047ab

- BM_IO_ERROR flag is lost in TerminateBufferIO due to order of operations in UnlockBufHdrExt
  - Commit: c75ebc657ffc ?
  - Fix: 5b5e99047ab
  - Owner: Andres Freund

- SPLIT/MERGE use of is_internal
  - Commit: f2e4cc4
  - Fix: 6120d01c9f
  - Owner: Alexander Korotkov

- FSM Update Missing when Setting VM On-access
  - Commit: b46e1e54d078de
  - Fix: eed6c0d33e0
  - Owner: Melanie Plageman

### resolved before 19beta1

- WAL accumulation during REPACK CONCURRENTLY
  - Commit: 28d534e2a
  - Owner: Alvaro Herrera
  - Fix: 45b02984e2fa9ee7bd83f7e58797b160baf0cc13

- Fix errhint messages for REPACK (CONCURRENTLY) restrictions
  - Owner: Álvaro Herrera
  - Commit: 28d534e2ae0ac888b5460f977a10cd9bb017ef98
  - Fix: 3bf63730cb041a834c618082ba3d5e8bf96a31a5

- Re: Speed up ICU case conversion by using ucasemap_utf8To*()
  - Commit: c4ff35f10441de7dbed4e87737bca205dcca698e
  - Fix: 06ce97b9994eca91250d7c28f0df9b1ae7829f36
  - Owner: Jeff Davis

- Compressed TOAST data corruption with REPACK CONCURRENTLY
  - Commit: 28d534e2ae0a
  - Owner: Álvaro Herrera

- CI reports 038_walsnd_shutdown_timeout failure on FreeBSD
  - Commit: a8f45dee917
  - Fix: c0b24b32b0d
  - Owner: Fujii Masao

- Add tests for locks statistics
  - Commit: 7c64d56fd9765bb577937c07eabed971e605c67c removed the tests as unstable in the CI for Windows, at least.
  - Fix: 557a9f1e3e62
  - Owner: Michael Paquier

- Deferred foreign key checks mistakenly don't use batched path
  - Commit: b7b27eb41a5
  - Fix: 5c54c3ed1b934
  - Owner: Amit Langote

- pg_upgrade fails when extension_control_path is used
  - Commit: 4f7f7b0375854e2f89876473405a8f21c95012af added this feature to 18; upgrading to 19 fails.
  - Fix: 1e67508730e
  - Owner: Peter Eisentraut

- Fast-path FK batch path crashes under nested SPI from C extensions (resource owner mismatch).
  - Commit: b7b27eb41a5
  - Fix: 34a307862930
  - Owner: Amit Langote

- FK fast path check column order mismatach
  - Commit: 2da86c1ef9b54
  - Fix: 980c1a85d81
  - Owner: Amit Langote / Junwang Zhao

- COPY TO FORMAT JSON does not handle generated column properly
  - Commit: 7dadd38cda95
  - Fix: f30d0c720f2ec
  - Owner: Andrew Dunstan

- GetLocalPinLimit() is too aggressive making it easy for backend to run out of pins
  - Easier to hit after 378a216187aea1 but not caused by this
  - Owner: Melanie Plageman

- UPDATE/DELETE FOR PORTION OF with rule system is not working
  - Commit: 8e72d914c52
  - Fix: 33db6c4baf17e
  - Owner: Peter Eisentraut / Paul Jungwirth

- REPACK should not fallback to use deferrable primary key concurrent mode
  - Commit: 28d534e2ae0
  - Fix: 832e220d99a
  - Owner: Alvaro Herrera

- WAIT FOR LSN crashes with assertion failure inside PL/pgSQL DO blocks and procedures
  - Commit: 447aae13b030
  - Fix: a8b61c23c5ff
  - Owner: Alexander Korotkov

- Inconsistencies with Datum conversions
  - Second report
  - Commit: 6dcfac9696cb
  - Fix: ac59a90bef45
  - Owner: Michael Paquier

- Not leak UID to psql client
  - Commit: 55890a91945
  - Fix: b772f3fcad1
  - Owner: Andrew Dunstan

- Bug: trailing comma syntax error in postgres_fdw fetch_attstats()
  - Commit: 28972b6fc3dc954e57708ea6
  - Fix: 3f7a1afbaef0
  - Owner: Etsuro Fujita

- Fix REPACK with WITHOUT OVERLAPS replica identity indexes
  - Commit: 28d534e2ae0a
  - Fix: 36f52a59b315
  - Owner: Álvaro Herrera

- Fix jsonpath .split_part() to honor silent mode
  - Commit: bd4f879a9cdd
  - Fix: 954e57708ea6
  - Owner: Andrew Dunstan

- Fix array-element quoting in postgres_fdw import statistics
  - Second report
  - Commit: 28972b6fc3dc
  - Fix: 5107398e6d5e
  - Owner: Etsuro Fujita

- Fix bug of COPY TO support partition table
  - Owner: Masahiko Sawada
  - Commit: 4bea91f21f61
  - Fix: 82f0135a2630

- UPDATE/DELETE FOR PORTION OF updatable view some case not working
  - Commit: 8e72d914c52
  - Owner: Peter Eisentraut / Paul Jungwirth

- Coverity: Improper use of strncpy()
  - Commit: 28972b6fc3dc
  - Fix: aa1f93a3387a
  - Owner: Etsuro Fujita

- Fix IGNORE NULLS nullness cache for volatile window arguments
  - Commit: 2d7b247cb41
  - Fix: 26269fe3c83
  - Owner: Tatsuo Ishii

- Fix SPLIT PARTITION bound-overlap bug
  - Commit: 4b3d173629f4
  - Fix: 9354896920e4
  - Owner: Alexander Korotkov

- Set notice receiver before libpq connection startup
  - Commit: 112faf1378e
  - Fix: 06a5c3cdef02
  - Owner: Fujii Masao

- Fix LISTEN startup race with direct advancement
  - Commit: 282b1cde9dedf456ecf02eb27caf086023a7bb71
  - Fix: 0f24332aeb4f43409c2a7bec9fef1e3317689bc5
  - Owner: Tom Lane

- Startup process deadlock: WaitForProcSignalBarriers vs aux process
  - Commit: 67c20979ce72
  - Fix: d79bf7612a07a6f05
  - Owner: Masahiko Sawada

- Assertion failure in pg_plan_advice with EXISTS subquery and DO_NOT_SCAN advice
  - Commit: 5883ff30b02c
  - Fix: b1901e2895e08f57a5fedfd27375babd6c633893
  - Owner: Robert Haas

- pg_stash_advice dump file overlong stash name crashes worker
  - Commit: c10edb102a
  - Fix: 878839bafe259e6df355e7adc5a040a1541b63b8
  - Owner: Robert Haas

- Avoid leaking system path from pg_available_extensions
  - Commit: f3c9e341cdf1
  - Fix: db5ed03217b
  - Owner: Andrew Dunstan

- Apply ENCODING option for COPY TO FORMAT JSON
  - Commit: 7dadd38cda
  - Fix: 7dc5bbcf220
  - Owner: Andrew Dunstan

## Non-bugs

- No method to block effective_wal_level=logical when wal_level=replica is needed
  - the REPLICATION privilege already grants very powerful capabilities, including the ability to read all data via physical streaming and to hold back horizons. A user trusted with REPLICATION is already trusted enough that allowing them to create a logical slot does not represent a fundamentally new class of risk.

- Avoid evaluating FOR PORTION OF bounds more than once
  - Commit: 8e72d914c52
  - Owner: Peter Eisentraut / Paul Jungwirth

- postgres_fdw statistics import is not atomic
  - Commit: 28972b6fc3dc
  - Owner: Etsuro Fujita

## Won't Fix

- Should FOR PORTION OF use null values for unbounded ranges or a separate keyword?
  - Commit: 8e72d914c52
  - Owner: Peter Eisentraut / Paul Jungwirth

- What to do about concurrent repack lock upgrade?
  - Owner: Álvaro Herrera
  - Commit: 28d534e2ae0ac888b5460f977a10cd9bb017ef98

- Improve error hint for already-modified rows under FOR PORTION OF
  - Commit: 8e72d914c52
  - Owner: Peter Eisentraut / Paul Jungwirth

- Improve error message of FOR PORTION OF violations of WITH CHECK OPTION
  - Commit: 8e72d914c52
  - Owner: Peter Eisentraut / Paul Jungwirth

- Document the privileges FOR PORTION OF needs on the range column
  - Commit: 8e72d914c52
  - Owner: Peter Eisentraut / Paul Jungwirth

- Improve error message for multiple assignment via view with FOR PORTION OF
  - Commit: 8e72d914c52
  - Owner: Peter Eisentraut / Paul Jungwirth

- pg_dump --statistics restores extended statistics by attribute number
  - Commit: c32fb29e979
  - Owner: Michael Paquier

## Older bugs affecting stable branches

### Live issues

- Foreign key cast cache not invalidated properly
  - Commit: N/A This is an old bug predating fast path added in 19 but found during its testing.
  - Owner: Amit Langote

- bogus relcache state with ERROR during invalidation
  - Not related to REPACK at all Unexpected behavior after OOM errors

- type cast disallow UNKNOWN target types
- ALTER COLUMN SET EXPRESSION need cope with whole-row references

- pg_upgrade --check passes, then the upgrade fails (standard_conforming_strings = off, escape_string_warning, user uuid/bytea casts)
  - Commit: 45762084545, ba21f5bf8af
  - Owner: Tom Lane, Masahiko Sawada
