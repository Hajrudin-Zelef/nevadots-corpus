---
id: collect-261121-veille-ia/langages-dev/postgresql-19-release-date-what-s-new-and-how-to-upgrade-pandromeda
title: "PostgreSQL 19 Release Date: What's New and How to Upgrade | Pandromeda"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-11-19"]
keywords: ["postgresql", "release", "november", "date", "upgrade", "vacuum", "autovacuum", "confirmed"]
source: https://pandromeda.com/software/postgresql-19-release-date-confirmed-whats-new-how-to-prepare
source_anchor: ""
source_lines: [1, 138]
section: "Langages_Dev"
---
# PostgreSQL 19 Release Date Confirmed: What's New, How to Prepare

PostgreSQL 19 general availability is confirmed for November 19, 2026, after RC2. Here's what's new and how DBAs should prepare to upgrade.

PostgreSQL 19 is now officially set to become generally available on **November 19, 2026**, just two days from now, after the project shipped a second release candidate on November 12. The confirmed GA date closes out a release cycle that ran later than the community originally planned, with the PostgreSQL Global Development Group (PGDG) citing a heavier-than-usual volume of bug reports during beta testing as the reason the schedule slipped. Here is what actually changes in PostgreSQL 19, and what database administrators should check before upgrading.

## GA date confirmed: November 19, 2026

The PostgreSQL 19 RC1 announcement set November 19, 2026 as the general availability target, "unless an issue is discovered that requires a delay or an additional release candidate." That target held through RC2, which the PGDG published on November 12, 2026 alongside a scheduled batch of maintenance releases: PostgreSQL 18.7, 17.12, 16.16, 15.20, and 14.25, the last of which is the final release of the now end-of-life PostgreSQL 14 branch under the project's versioning policy. That maintenance round also closed out 52 security vulnerabilities and more than 140 bug fixes across supported versions, several of which the release team folded into PostgreSQL 19 ahead of GA.

Getting to this point took longer than the project's original fall target implied. During the PostgreSQL 19 beta period, release team member Jonathan Katz told the pgsql-hackers mailing list that a beta release had slipped "primarily due to handling the influx of issues that have been reported," which took the team additional time to triage and fix. That dynamic repeated across multiple beta cycles: Beta 4 arrived later than scheduled and, in the same announcement, the community confirmed it was pulling a handful of previously announced features, including SQL/PGQ graph-query support, so they could get more testing time ahead of a future major release instead of shipping half-baked in 19.

**Key facts at a glance**

- **GA date:** November 19, 2026
- **RC2 shipped:** November 12, 2026
- **RC1 shipped:** October 15, 2026
- **Companion maintenance releases:** 18.7, 17.12, 16.16, 15.20, 14.25 (PostgreSQL 14's final release)
- **Upgrade path:**`pg_upgrade` or`pg_dump` /`pg_restore` required from any earlier major version

## The headline features in PostgreSQL 19

PostgreSQL's official release notes list a long run of changes, but a handful stand out as the ones DBAs and application teams will actually notice day one.

### REPACK replaces VACUUM FULL and CLUSTER

PostgreSQL 19 introduces a new `REPACK` command that takes over the jobs previously split between `VACUUM FULL` and `CLUSTER`, reclaiming bloated disk space by rewriting a table. Critically, `REPACK (CONCURRENTLY)` can rebuild a table without blocking ordinary reads and writes against it — something `VACUUM FULL` has never been able to do. Progress is now visible through a new `pg_stat_progress_repack` monitoring view.

### Parallel and prioritized autovacuum

Autovacuum can now use parallel workers to vacuum a table's indexes, controlled by a new `autovacuum_max_parallel_workers` setting, which should meaningfully cut vacuum times on wide, heavily-indexed tables. A new scoring system (exposed through settings like `autovacuum_vacuum_score_weight` and the `pg_stat_autovacuum_scores` view) also lets autovacuum prioritize which tables get attention first, rather than working strictly in discovery order.

### Planner and execution improvements

The query planner picks up several targeted optimizations: certain `NOT IN` and `LEFT JOIN` patterns now convert into more efficient anti-joins (and can use Memoize), some aggregation can execute before joins to reduce row volume earlier in a plan, semijoin planning improves, and foreign-key constraint checks run faster. `EXPLAIN ANALYZE` gains a new `IO` option for surfacing I/O timing directly in query plans. One change that needs a callout: **JIT compilation is now disabled by default**, so sites that rely on it for large analytical workloads will need to explicitly re-enable it.

### Replication: read-your-writes and sequence replication

A new `WAIT` command lets a client block until a standby has written, flushed, or replayed a given WAL position — enabling "read-your-writes" consistency patterns against replicas. Logical replication also grows up in two important ways: sequences can now be replicated (via `CREATE PUBLICATION ... FOR ALL SEQUENCES` and `ALTER SUBSCRIPTION ... REFRESH SEQUENCES`), and logical replication can be enabled on a running server without a restart when `wal_level = replica`, exposed through a new `effective_wal_level` setting.

### I/O and storage defaults

The `io_method = worker` asynchronous I/O mode now scales its background worker count automatically via new `io_min_workers`/`io_max_workers` settings, replacing the old static `io_workers` parameter entirely. The default TOAST compression method switches from `pglz` to `lz4`, and `COPY FROM` gets a SIMD-accelerated path for text and CSV parsing.

### Security hardening

RADIUS authentication support has been removed outright, on the grounds that RADIUS over UDP is considered unfixably insecure. MD5 password authentication is formally on its way out too: successful MD5 logins now trigger a warning (disable it with `md5_password_warnings`), nudging shops still on MD5 toward SCRAM. Server-side SNI support also arrives, configured through a new `pg_hosts.conf` file.

For teams still weighing which data store fits a given workload, it's worth revisiting how a relational engine like PostgreSQL stacks up against in-memory options — see this comparison of Redis and Valkey for in-memory database workloads for the other side of that decision.

| Area | PostgreSQL 18 behavior | PostgreSQL 19 change | 
|---|---|---|
| Table rewrites | `VACUUM FULL` /`CLUSTER` (blocking) | New `REPACK` , with non-blocking`REPACK (CONCURRENTLY)` | 
| Autovacuum | Single-threaded per table | Parallel index vacuum + priority scoring | 
| JIT | Enabled by default (threshold-based) | Disabled by default | 
| Async I/O workers | Static `io_workers` count | Auto-scaled via `io_min_workers` /`io_max_workers` | 
| Logical replication scope | Tables only | Tables + sequences; can enable without restart | 
| TOAST compression default | `pglz` | `lz4` | 
| RADIUS auth | Supported | Removed | 

## SQL syntax and observability additions

Beyond the headline items, PostgreSQL 19 adds several developer-facing conveniences. `INSERT ... ON CONFLICT DO SELECT ... RETURNING` can now return the conflicting row (optionally with a lock), which simplifies common upsert-and-read patterns. Window functions `lead()`, `lag()`, `first_value()`, `last_value()`, and `nth_value()` gain `IGNORE NULLS`/`RESPECT NULLS` clauses. `COPY TO` can emit JSON directly, and `COPY FROM` supports `ON_ERROR SET_NULL` for more forgiving bulk loads. `CHECK` constraints can be toggled with `ALTER CONSTRAINT ... [NOT] ENFORCED`, and `GRANT`/`REVOKE` support a `GRANTED BY` clause.

On the monitoring side, new views — `pg_stat_lock`, `pg_stat_recovery`, `pg_stat_progress_repack`, and `pg_stat_autovacuum_scores` — give operators more visibility into lock contention, recovery progress, and vacuum behavior. `log_lock_waits` is now on by default, and the transaction-ID wraparound warning now fires at 100 million transactions remaining (up from 40 million), giving teams more runway to react.

## Breaking changes to review before upgrading

A few changes in PostgreSQL 19 are not purely additive and are worth checking against your own schema and tooling before you upgrade:

- **Dumps with non-standard string escaping:** dumps taken from older clusters running with`standard_conforming_strings = off` will not load into PostgreSQL 19 as-is, since that setting is now always on.
- **Lock table sizing:** the default`max_locks_per_transaction` increases from 64 to 128, but the underlying lock allocation has changed too — sites that tuned this setting explicitly may need to roughly double their prior value to preserve the same effective capacity.
- **`io_workers` removal:** the static`io_workers` setting is gone, replaced by the four new min/max worker settings. Check any provisioning scripts or configuration management that still reference the old parameter.
- **Identifier restrictions:** carriage returns and line feeds are no longer allowed in database, role, or tablespace names.
- **Encoding removal:** the little-used`MULE_INTERNAL` encoding has been dropped.
- **Parallel vacuum cost delay:** parallel vacuum workers now share a single cost-based delay budget, so a parallel`VACUUM` run with cost delay enabled may take longer than it did on PostgreSQL 18.
- **Beta-era JSON expression objects:** anyone who built expression indexes, generated columns, or partition keys using`JSON_ARRAY()` /`JSON_OBJECT()` during PostgreSQL 19 beta testing needs to drop and recreate them — that support was reverted between RC1 and RC2, and`pg_dump` /`pg_restore` and`pg_upgrade` will fail on them otherwise.

## Upgrade checklist for DBAs

PostgreSQL's own guidance is unchanged in principle: minor version updates (like the 18.7/17.12 releases shipped alongside RC2) never require `pg_upgrade` or a dump/reload, but moving to a new major version — PostgreSQL 19 included — does. Before scheduling that jump, work through the following.

| Step | Why it matters | 
|---|---|
| Read the full release-19 notes for your specific feature usage | Several changes (JIT default, lock sizing, io_workers) are config-sensitive, not universal | 
| Inventory extensions for PostgreSQL 19 compatibility | Extensions pinned to internal APIs (not just SQL-level ones) may need rebuilds; check each extension's own release notes | 
| Test `pg_upgrade --check` against a staging copy first | Surfaces incompatible objects (e.g. reverted JSON expression indexes) before a production run | 
| Re-benchmark JIT-sensitive analytical queries | JIT is now off by default; workloads that depended on it may regress until re-enabled | 
| Review `max_locks_per_transaction` and any manual`io_workers` settings | Both have materially changed semantics or been removed in PostgreSQL 19 | 
| Take a fresh full backup (not just an incremental) immediately before and after the upgrade | PostgreSQL's own guidance after major updates is to start a new incremental backup chain from a fresh full backup | 
| Confirm logical replication subscriber versions | New sequence-replication and restart-free enablement features require matching publisher/subscriber support | 

As with every major version, `pg_upgrade` (or a dump/restore cycle for larger structural changes) remains the supported path from PostgreSQL 14 through 18 into 19 — see the project's own pg_upgrade documentation for the full procedure, including the `--link` mode many teams use to avoid copying an entire data directory. Given how many fixes landed between beta and RC2, treat a staging-environment test run as mandatory rather than optional, particularly if your schema leans on any of the features called out above.

## Why the release slipped — and why that's a good sign

It's worth separating the schedule story from the quality story. Yes, PostgreSQL 19 is arriving later than the project's early roadmap implied, and the public record shows at least one beta cycle slipping specifically because triaging a larger-than-expected wave of bug reports took longer than planned. But the alternative — shipping on the original date with those issues unresolved — is the outcome PostgreSQL's all-volunteer release process is explicitly built to avoid. The community's decision mid-cycle to defer features like SQL/PGQ graph queries to a future release, rather than ship them half-finished, points the same way: the extra weeks bought more testing, not more features bolted on at the last minute.

## What's next

Assuming no last-minute issue forces another release candidate, PostgreSQL 19.0 ships November 19, 2026, with its first minor update (19.1) expected in PostgreSQL's usual quarterly maintenance cadence early in 2026's successor year. Cloud providers running managed PostgreSQL services typically add support within weeks of GA — check your provider's own changelog before planning a production migration window. For now, the actionable step for most teams is the same one that applies to every major PostgreSQL release: stand up a staging cluster on the final build, run your actual schema and extension set through `pg_upgrade --check`, and budget time to address the handful of breaking changes — JIT defaults, lock sizing, and `io_workers` among them — documented above.

## Frequently asked questions

### When is PostgreSQL 19 officially released?

The PostgreSQL Global Development Group has set general availability for November 19, 2026, following release candidate 2 on November 12, 2026, unless a last-minute issue forces another release candidate.

### Why was PostgreSQL 19 delayed?

The release ran later than the project's original schedule because beta testing surfaced more bug reports than usual. Release team member Jonathan Katz told the pgsql-hackers mailing list that at least one beta cycle slipped specifically to give the team time to triage and fix the additional issues.

### What are the biggest new features in PostgreSQL 19?

The headline additions are a new REPACK command (with a non-blocking CONCURRENTLY mode) that replaces VACUUM FULL and CLUSTER, parallel and priority-scored autovacuum, a WAIT command for read-your-writes consistency on replicas, logical replication of sequences, and auto-scaling asynchronous I/O workers.

### Is JIT compilation still enabled by default in PostgreSQL 19?

No. JIT compilation is now disabled by default in PostgreSQL 19. Sites that depend on it for large analytical queries need to explicitly re-enable it after upgrading.

### Do I need pg_upgrade to move to PostgreSQL 19?

Yes. Minor version updates do not require pg_upgrade, but moving to any new major version, including PostgreSQL 19, requires either pg_upgrade or a pg_dump/pg_restore cycle.

### What should I check before upgrading to PostgreSQL 19?

Review extension compatibility, re-test JIT-dependent queries, check any custom max_locks_per_transaction or io_workers settings (both changed), run pg_upgrade --check against a staging copy, and drop any JSON expression indexes created during PostgreSQL 19 beta testing, since that feature was reverted before GA.

## Sources

- PostgreSQL 19 RC1 announcement (postgresql.org)postgresql.org
- PostgreSQL 19 RC2 / 18.7 / 17.12 / 16.16 / 15.20 / 14.25 announcement (postgresql.org)postgresql.org
- PostgreSQL 19 release notes (release-19.html)postgresql.org
- pg_upgrade documentation (postgresql.org)postgresql.org
- Jonathan Katz, pgsql-hackers: PostgreSQL 19 Beta 4 release datepostgresql.org
- PostgreSQL versioning policypostgresql.org

Felix Moreau writes Pandromeda's software coverage and how-to guides. He covers Windows, macOS and Linux updates, the apps people rely on, emulators and developer tools, and turns official documentation into clear, numbered steps that work on the current version.
