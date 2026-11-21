---
id: collect-261121-veille-ia/langages-dev/postgresql-19-features-i-m-excited-about-bytebase
title: "PostgreSQL 19 features I'm excited about | Bytebase"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["advice", "lock", "plan", "postgresql", "repack", "stash", "table", "excited"]
source: https://www.bytebase.com/blog/postgres-19-features-im-excited-about/
source_anchor: ""
source_lines: [1, 101]
section: "Langages_Dev"
---
PostgreSQL 18 shipped asynchronous I/O as raw capability. PostgreSQL 19 is the operational counterpart, and the items below are the ones I'm most excited about. Bundled plan-advice modules give operators a new way to control planner decisions. `REPACK` allows concurrent table rewrites. Lock contention and standby recovery state get first-class system views. The 32-bit MultiXact member-offset ceiling that has caused real production outages is gone. Autovacuum becomes parallel and, just as important, observable.

PostgreSQL's planner is excellent at the average case and occasionally wrong at the long tail. Skewed distributions, stale statistics, and many-table joins can push it onto a plan that is simply unacceptable in production. For years the workarounds all lived outside core: the pg_hint_plan extension, application-level `SET enable_*` flags, third-party plan-locking extensions. Each one has its own semantics and its own lifecycle to babysit.

This is not a theoretical pain. In February 2026, Clerk traced an outage to exactly this pattern: a column whose values were 99.9996% NULL, an `ANALYZE` whose sample happened to contain only NULLs, and a planner that concluded the column was 100% NULL. The plan that followed assumed zero non-null rows where there were over 17,000.

PostgreSQL 19 brings the answer inside core, in two layers.

`pg_plan_advice` introduces a planner-advisor framework. It generates a plan-advice string that captures the relevant plan choice (join order, scan method, and so on) and exposes a hook for other modules to inject advice at planning time. `EXPLAIN (PLAN_ADVICE)` generates advice; `SET pg_plan_advice.advice` supplies it. See the module documentation.

`pg_stash_advice` stores advice by stash name and query ID in shared memory. Sessions using that stash apply it when planning matching queries. A background worker saves stashes to disk by default when configured at server startup.

Scope is controlled by the `pg_stash_advice.stash_name` GUC: `ALTER DATABASE`, `ALTER ROLE`, or session-level `SET`.

My verdict: this is plan stability without touching the application. One stash entry guides future planning in sessions configured to use it. Already cached plans are unaffected. The right use case is a small set of queries with known planner regressions on production data. Applying advice has a cost even when it does not change the plan, so don't reach for it across a whole workload. Use it as a scalpel, not a blanket.

`REPACK` unifies the table-rewrite jobs of `VACUUM FULL` and `CLUSTER`, which remain available for compatibility. The concurrent option brings an important reason for installing `pg_repack` into PostgreSQL itself.

`USING INDEX` orders the table by that index. It does not repack an index independently.

The concurrent form allows reads and writes during most of the rebuild, then takes an `ACCESS EXCLUSIVE` lock for the final swap. It needs a primary key or index-based replica identity, a replication slot permitted by `max_repack_replication_slots`, and disk space for another copy of the table and indexes. It cannot run inside a transaction block or directly on a partitioned table.

My verdict: a much smaller blocking window is a real operational win. But concurrent does not mean lock-free. Monitor progress through `pg_stat_progress_repack`, and account for WAL retention and the final lock acquisition. The docs also flag an MVCC caveat: transactions with snapshots taken before repacking started can see the table as empty afterward.

Two diagnostic gaps close in 19. Lock history and standby recovery state get first-class system views.

`pg_stat_lock` collects cluster-wide counters by lock type. It records waits, wait time, and fast-path slot exhaustion. Wait counters update after a lock is acquired following a wait longer than `deadlock_timeout`.

`log_lock_waits` is on by default. Long lock waits are now logged out of the box.

My verdict: useful trend data, but no per-table or per-lock-mode history. Use `pg_stat_lock` to spot a rise in contention, then `pg_locks`, `pg_stat_activity`, and the logs to identify the objects and queries involved.

`pg_stat_recovery` brings replay positions, timelines, promotion state, the last replayed transaction timestamp, and recovery pause state into one view.

It returns no rows outside recovery or without `pg_read_all_stats` privileges. `current_chunk_start_time` records when replay caught up with the latest received streaming-WAL chunk; it is not the start time of the current WAL record.

My verdict: fewer scattered function calls in standby dashboards. Keep comparing WAL positions too: an old transaction timestamp alone does not prove lag on an idle primary.

Writing to the primary and immediately reading from an asynchronous replica can return stale data. PostgreSQL 19 adds `WAIT`, so the reader can wait for replay to reach a known WAL position.

The target must include the transaction's commit record. Run `WAIT` before taking a snapshot or locks on the standby. With this timeout, failure raises an error instead of letting the application silently read stale data.

My verdict: a small command that removes application-side polling. Connection routing still has to carry the LSN, and failover still needs timeline handling: `WAIT` compares numeric positions, not timeline identity.

Logical replication has reproduced table data faithfully since 10. What it has not reproduced is the sequences backing `SERIAL` and `IDENTITY` columns. Promote a subscriber to primary and the next `INSERT` triggers a primary-key violation against rows the application thought it owned. A nasty surprise to discover at cutover.

PostgreSQL 19 adds sequence synchronization. Initial subscription setup copies values by default; `REFRESH PUBLICATION` picks up newly published sequences, and `REFRESH SEQUENCES` re-synchronizes those already subscribed.

`pg_subscription_rel.srsubstate` exposes per-sequence progress (`i` = INIT, `r` = READY).

My verdict: planned cutovers get safer, provided the runbook stops publisher writes, lets table replication catch up, refreshes sequences, and waits for synchronization to finish before allowing subscriber writes. Values do not track continuously. The publisher must also be running PostgreSQL 19 or later, so this does not solve sequence migration from an older primary into 19.

The release pairs this with another operational improvement: logical decoding can be activated on demand when `wal_level` is `replica`. The read-only `effective_wal_level` reports the active level. Setting up logical replication no longer requires restarting that primary just to change `wal_level`.

PostgreSQL tracks row-level locks shared across transactions in MultiXact structures. The pointer into the member array (MultiXactOffset) was 32 bits, capping total members at roughly 4 billion. Transaction-ID wraparound is well-known and widely monitored. MultiXact member exhaustion is not. It does not show up on standard XID-consumption dashboards, and high-concurrency workloads with foreign keys can hit it in days, not years. That asymmetry is what makes it dangerous.

In May 2025, Metronome experienced four separate outages from this exact ceiling during a data migration, requiring hours of emergency vacuuming on a 30TB cluster.

PostgreSQL 19 widens MultiXact member offsets to 64 bits, removing the old member-addressing limit.

My verdict: one nasty exhaustion failure is gone. But MultiXact IDs themselves remain 32-bit, and anti-wraparound vacuuming still matters. Member storage also needs cleanup to keep disk usage under control. This is a reason to upgrade, not a reason to stop monitoring vacuum.

Two long-standing complaints land together. Autovacuum can finally use parallel workers, and the `pg_stat_*` views expose enough state that diagnosing a slow vacuum no longer relies on guesswork.

Parallel autovacuum. `autovacuum_max_parallel_workers` enables parallel index processing for autovacuum runs. Per-table tuning lives in the new `autovacuum_parallel_workers` storage parameter. Manual `VACUUM (PARALLEL N)` has worked since 13; autovacuum has been single-threaded throughout. About time.

Autovacuum priority is now visible. `pg_stat_autovacuum_scores` exposes *why* a given table is queued ahead of another. A family of `autovacuum_*_score_weight` GUCs (covering freeze, vacuum, vacuum-insert, multixact-freeze, and analyze pressure) lets operators tune the priority function. The view reflects current inputs, which may differ from those a worker saw when building its work list. Treat it as a diagnostic, not an exact queue.

`pg_stat_progress_vacuum` gains two columns. `started_by` distinguishes `manual` / `autovacuum` / `autovacuum_wraparound`. `mode` distinguishes `normal` / `aggressive` / `failsafe`. Routine sweep or emergency, in one column.

Planned vs launched parallel workers logged. `VACUUM (VERBOSE)` output and autovacuum logs gain two numbers: how many parallel workers the operation planned, and how many it actually launched. The two diverge often. `max_parallel_maintenance_workers`, `min_parallel_index_scan_size`, the eligible-index count, and runtime worker availability all gate the count. The counts expose the shortfall; they do not by themselves identify which limit caused it.

My verdict: faster autovacuum on heavily-indexed tables, and for once a feedback loop to verify it actually ran the way you expected. Parallel autovacuum gates on the same conditions as manual `VACUUM (PARALLEL N)` (eligible-index count, `min_parallel_index_scan_size`, available worker slots), so verify workers actually launch before assuming you got the speedup.

The investigation order changes too. The old loop (raise `maintenance_work_mem`, lower `autovacuum_vacuum_cost_delay`, hope) was guesswork because the inputs were not observable. The new sequence:

1. Read `pg_stat_autovacuum_scores` to inspect current priority scores.
2. Check the autovacuum log for planned vs launched workers. If the gap is large, the bottleneck is gating, not throughput.
3. Watch `pg_stat_progress_vacuum.mode` for`failsafe` events. Investigate transaction age, blockers, vacuum capacity, and scheduling before choosing a tuning change.

PostgreSQL 19 arrived late and smaller than its beta. During that cycle, the committers' own diagnosis was that LLM-assisted review raised the quality bar after the code was written. Robert Haas reported the numbers: back-patches to the 18 branch were up over 60% year over year while new commits on master were flat. His reading: the same mistakes were being caught faster, not made more often. The community delayed the release and pulled features rather than ship known problems. That was the right call.

Two features on my original list were removed in Beta 4: DDL extraction and online checksum toggling. Here's what they promised and why I wanted them.

PostgreSQL has never shipped a way to pull the `CREATE` statement for a database, role, or tablespace out of the catalog. The data is there. The DDL is not. So every team that needs it ends up writing its own extractor: a shell loop around `pg_dump --schema-only`, a hand-rolled catalog query inside a migration tool, or a schema-management platform like Bytebase that reads the catalog directly and assembles the DDL. The implementations differ. The maintenance burden does not. Every major Postgres release brings new catalog columns and new attribute encodings to absorb, and somebody has to keep up.

The beta feature aimed to replace those bespoke layers with a stable, in-core function-call contract. Three functions, one option shape, returning the DDL as text:

All three accepted `pretty := true`. `pg_get_database_ddl()` additionally accepted `owner := false` and `tablespace := false` to suppress those clauses when the target environment differs.

My verdict at the time: a stable in-core contract would retire every team's homegrown extractor. Migration prep, CI/CD schema diffs, audit snapshots of role grants and database properties, embedded calls from schema-management tools, all of it could stop parsing `pg_dump` text.

These functions were designed to complement `pg_dump` rather than replace it: single-object only, no dependency ordering, no indexes, triggers, or constraints. Permissions were scoped sensibly: `CONNECT` on the database, `USAGE` on the tablespace, superuser or self for roles.

The beta feature allowed checksums to be enabled or disabled progressively in a running cluster, without stopping it and running `pg_checksums` against a halted data directory.

My verdict at the time: removing that shutdown would eliminate another maintenance window. I still want it. For now, `pg_checksums` still requires a stopped cluster.

`pg_plan_advice` is the one I'm most excited about, and it closes another long-standing gap with Oracle and SQL Server. It defines an in-core hook for injecting planner advice, which is the foundation a proper SQL Plan Management (SPM) module can be built on: automatic baseline capture, plan evolution, regression detection, the pieces Oracle SPM and SQL Server's Query Store users have taken for granted for years. PostgreSQL 19 doesn't ship SPM itself, but for the first time it ships the substrate that lets the community build one without forking the planner. That distinction is the whole point.

The rest of the list is the kind of steady, hard-won progress that keeps PostgreSQL ahead in production. Vacuum and MultiXact are really the same (debatable) architectural choice surfacing as two different problems: to implement MVCC, PostgreSQL keeps a separate version of every row an `UPDATE` touches, and a background process cleans them up. Until something like OrioleDB (a storage engine that implements MVCC via undo log instead of duplicating tuples) lands in core, we will keep duct-taping. So the open question for 19: is this the release where the duct tape finally starts to come off, or just a sturdier roll of it?
