---
id: collect-261121-veille-ia/langages-dev/postgresql-19-repack-concurrently-replaces-vacuum-full
title: "PostgreSQL 19: REPACK CONCURRENTLY Replaces VACUUM FULL"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-11-19"]
keywords: ["repack", "postgresql", "advice", "plan", "release", "concurrently", "without", "changes"]
source: https://thestacksentinel.com/articles/postgresql-19-repack-concurrently-replaces-vacuum-full-ab21f7b3/
source_anchor: ""
source_lines: [1, 78]
section: "Langages_Dev"
---
TL;DR

The PostgreSQL Global Development Group released **PostgreSQL 19 on 2026-11-19**. It adds REPACK (CONCURRENTLY) for reorganizing tables without blocking reads and writes, the pg_plan_advice extension for plan stability, and the companion pg_stash_advice extension. It also changes several defaults and removes features, so the compatibility items need checking before any upgrade.

- REPACK replaces the VACUUM FULL and CLUSTER commands, which remain available for compatibility (PostgreSQL 19 release announcement).
- REPACK (CONCURRENTLY) rebuilds a table without access-exclusive locking, and the **max_repack_replication_slots** setting was added (PostgreSQL 19 release notes).
- pg_plan_advice stabilizes planner choices, and pg_stash_advice applies stored advice to queries by query identifier.
- Just-in-time compilation (JIT) is disabled by default.
- RADIUS authentication support is removed, and md5 password authentication now logs a warning after success.
- The io_workers setting is removed and replaced by io_min_workers and io_max_workers.
- The default of max_locks_per_transaction changes from 64 to 128.
- Migration requires a dump and restore with pg_dumpall, pg_upgrade, or logical replication.

## REPACK Rebuilds Tables Without Blocking Access #

Tables that take many updates and deletes accumulate unused space, which the announcement calls bloat. PostgreSQL 19 introduces the REPACK command, which reclaims that space and can reorder a table's data to match an index. The VACUUM FULL and CLUSTER commands it replaces had the drawback of preventing access to the table until cleanup finished. **REPACK (CONCURRENTLY)** is the option the announcement says can do this work without blocking reads or writes.

The option is enabled through the CONCURRENTLY keyword, and the release notes describe it as allowing a table rebuild without access-exclusive locking. The release notes also list the new max_repack_replication_slots setting, which was added alongside the option (PostgreSQL 19 release notes). They also add a pg_stat_progress_repack view that reports the progress of REPACK, CLUSTER and VACUUM FULL.

## Plan Advice Extensions Pin Query Plans Without Code Changes #

The planner usually picks an efficient plan, but the plan can change as your data changes, and sometimes the new plan is slower. The pg_plan_advice extension lets administrators stabilize plan choices and experiment with alternatives using a "plan advice" mini-language. The companion pg_stash_advice extension stores advice and **applies it automatically based on each query's query identifier**, so a slow plan can be fixed without changing the application.

Because pg_stash_advice matches advice to a query identifier, the fix lives in the database rather than in application code, which is the practical difference the announcement highlights.

## Compatibility Changes to Check Before Upgrading #

Several changes can break existing workloads or dumps. The PostgreSQL 19 release notes state that **standard_conforming_strings is forced to always be on**. Dumps made with pre-19 pg_dump or pg_dumpall while standard_conforming_strings = off will not load properly into PostgreSQL 19 servers. Those users should create dumps with PostgreSQL 19 tools or set standard_conforming_strings = on.

Clusters with **btree_gist inet and cidr indexes** cannot be upgraded with pg_upgrade. The release notes say those opclasses can exclude rows that should be returned, and the default opclasses for inet and cidr move to GiST. Databases whose names or roles contain carriage returns or line feeds are also blocked by pg_upgrade, because those names are now disallowed. Databases that use the MULE_INTERNAL encoding must be dumped and restored with a different encoding.

The default of max_locks_per_transaction changes from 64 to 128. The release notes add that lock size allocation has changed, so **settings must now be doubled** to match their capacity in previous releases. The io_workers setting is removed and replaced by io_min_workers, io_max_workers, io_worker_idle_timeout and io_worker_launch_interval.

## Security and Authentication Removals in PostgreSQL 19 #

Authentication changes need attention. PostgreSQL 19 also now issues a warning after a successful md5 password authentication, which can be turned off with the md5_password_warnings setting. md5 authentication was deprecated in PostgreSQL 18 and **will be removed in a future release**, and the announcement recommends SCRAM for password-based authentication. RADIUS support is removed; the release notes say it was supported only over UDP, which they call unfixably insecure.

The new password_expiration_warning_threshold setting warns users before a password expires, and its default warning period is seven days. Server-side SNI support lets one server present different TLS certificates by hostname, configured in the new PGDATA/pg_hosts.conf file.

## Default Changes: JIT Off and LZ4 TOAST Compression #

JIT is now disabled by default. The release notes explain that the cost estimates used to trigger it were determined to be unreliable. Sites that run many large analytical queries must enable JIT manually (PostgreSQL 19 release notes). Separately, when PostgreSQL is built with LZ4 support, **the default TOAST compression method is now lz4**, set through default_toast_compression.

Parallel vacuum changes behaviour too. Leaders and workers now share the cost-based vacuum delay limit, so the release notes warn that VACUUM with parallel workers and cost-based delay enabled might run slower than in previous releases.

## Logical Replication Can Start Without a Restart #

Logical replication previously required a server restart to enable. PostgreSQL 19 automatically raises the effective WAL level to logical when you create the **first logical replication slot**, and the new effective_wal_level setting shows the level in use. Sequence values can now be published with FOR ALL SEQUENCES, and the new retain_dead_tuples option lets subscribers detect updates that conflict with a concurrent delete.

## Also in This Release #

- Autovacuum can vacuum indexes with parallel workers, controlled by **autovacuum_max_parallel_workers** , and uses a new scoring system to order tables.
- INSERT ... ON CONFLICT DO SELECT lets RETURNING return conflicting rows.
- WAIT FOR LSN lets a standby session wait for replay before a read.
- postgres_fdw can import remote statistics with the import_stats option.
- COPY can output JSON, and lead, lag, first_value, last_value and nth_value accept IGNORE NULLS.
- The new pg_stat_lock, pg_stat_recovery and pg_stat_autovacuum_scores views add monitoring.

The announcement and release notes list many more features, fixes and minor changes, so the PostgreSQL 19 release notes remain the full list.

The Sentinel’s Call

Check the migration and compatibility items first: standard_conforming_strings, btree_gist inet and cidr indexes, max_locks_per_transaction, io_workers, and the JIT default. Plan any move away from md5 passwords and RADIUS before the upgrade, since those are the removals. REPACK and the plan advice extensions can be adopted after the upgrade is in place.

Our judgement, drawn only from the facts and sources above.

## Sources

1. PostgreSQL: PostgreSQL 19 Released! postgresql.org · Nov 19, 2026
2. PostgreSQL: Documentation: 19: E.1. Release 19 postgresql.org · Nov 19, 2026

## More on PostgreSQL

3 other articles, newest first

Edited and approved by Jarosław Krzemiński. Spotted a mistake? Report a correction or read the corrections policy.

## Comments
