---
id: collect-261121-veille-ia/langages-dev/postgresql-19-beta-4-release-date-new-features-reverts-and-breaking-changes
title: "PostgreSQL 19 Beta 4 Release Date: New Features, Reverts, and Breaking Changes"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["postgresql", "beta", "features", "repack", "command", "replication", "release", "table"]
source: https://www.linuxcompatible.org/story/postgresql-19-beta-4-release-date-new-features-reverts-and-breaking-changes/
source_anchor: ""
source_lines: [1, 92]
section: "Langages_Dev"
---
### PostgreSQL 19 Beta 4 Ships With a New REPACK Command and a Surprisingly Bold Retreat

**PostgreSQL 19 Beta 4 arrived today. It adds features you'll actually use, then quietly pulls several ambitious ones back off the shelf to protect a tight October schedule.** **The penultimate pre-release adds a unified REPACK command and read-your-writes replication, then yanks several high-profile features to hit a predictable release date.**

This is the fourth and penultimate beta of the cycle. The release candidate is set for early October, with general availability possibly landing later that same month. PostgreSQL 18 shipped exactly a year earlier, on September 25, 2025, so 19 is keeping the project's usual annual rhythm. But this pre-release is doing something unusual. It's using the final checkpoint to say "not yet."

The project itself, the elephant-logo RDBMS some call the world's most advanced open source database, has a long history behind that discipline. It started as POSTGRES at Berkeley in 1982, led by Michael Stonebraker, who'd later take the 2014 Turing Award. A pair of graduate students, Andrew Yu and Jolly Chen, swapped the original query language for SQL in 1994, birthing Postgres95, soon renamed PostgreSQL. That background shows up in how the maintainers are handling this beta. They'd rather ship reliably than ship everything.

#### Features that actually ship

Let's start with the one most operators will reach for.

A unified REPACK command. For years, people reclaiming disk space or reorganizing tables had to choose between VACUUM FULL, which rewrites a table to recover space from deleted rows, and CLUSTER, which reorders it according to an index. Both did similar jobs, both carried confusing names, and both locked the table against reads and writes for the entire operation.

The new REPACK command, written by Antonin Houska, folds both into a single name while keeping backward compatibility with the originals. The real selling point is CONCURRENTLY. It rebuilds a table without an access-exclusive lock, so reads and writes keep flowing during the process. Under the hood it uses logical replication slots, now governed by a new server variable called max_repack_replication_slots. Offline table reorganization has been a genuine thorn for a long time. This one removes it.

Read-your-writes, finally. Splitting reads across replica servers works fine until a transaction you just wrote isn't visible on a standby. That's a subtle class of bug, and it's been around forever. PostgreSQL 19 adds the WAIT command to kill it. Execution pauses until a standby has replayed changes up to a chosen write or flush point.

An app that inserts a row then immediately queries a replica can now be sure the row is there. Kartyshov Ivan, Alexander Korotkov, and Xuneng Zhou wrote it.

Autovacuum with weights. Vacuuming reclaims space and prevents transaction-ID wraparound, and it gets a big upgrade in 19. Autovacuum can now use parallel workers to vacuum a table's indexes, controlled by autovacuum_max_parallel_workers and a per-table storage parameter.

There's also a new scoring system that prioritizes the tables most in need of attention. You can tune weights for freeze, multixact-freeze, vacuum, insert, and analyze pressure. pg_stat_autovacuum_scores exposes all of it.

Planner pinning. Plan changes between versions have famously broken applications. PostgreSQL 19 answers with pg_plan_advice, an extension for pinning the planner's decisions, plus a companion pg_stash_advice that applies the advice automatically based on the query. Operators now have a formal way to dodge post-upgrade surprises.

Logical replication, beefed up. Replication can now propagate sequence values so subscriber sequences match publishers, and it can be enabled without a server restart when wal_level is replica, using a new "effective WAL level" concept. Publishers can exclude tables with a new EXCEPT clause, and retain_dead_tuples improves conflict detection.

#### The performance work

The features grab the press release. The performance work is often what practitioners actually feel.

- The worker IO method now dynamically scales its background I/O workers using new variables like io_min_workers and io_max_workers, improving read-ahead scheduling for big requests.
- Foreign-key constraint validation gets faster, easing a common bottleneck during bulk loads.
- SIMD CPU instructions accelerate COPY FROM for text and CSV ingestion.
- Radix sort replaces older comparison-heavy sorts more often, while hash-index bulk deletion and GIN vacuuming use streaming reads.
- The default TOAST compression shifts from pglz to lz4, and wal_compression = on now picks the best available method.
- Parallel table scans can mark pages as all-visible, and TID range scans can run in parallel.
- NOTIFY now wakes only the backends actually listening, instead of most of them.

A fair chunk of incremental engineering. Some of it marginal. However, at the same time, a good bit is genuinely fast.

#### Optimizer, SQL language, and data types

The optimizer gets refinements for common patterns. NOT IN clauses convert to more efficient ANTI JOINs when NULLs are absent, and more LEFT JOINs benefit from the same trick. Aggregate processing can sometimes happen before joins, trimming the rows that must be processed.

On the SQL side, window functions — lead(), lag(), first_value(), last_value(), nth_value() — gain an IGNORE NULLS / RESPECT NULLS clause. INSERT ... ON CONFLICT DO SELECT ... RETURNING lets conflicting rows be returned and optionally locked. GROUP BY can now handle target-list subqueries referencing non-subquery columns.

Data types and functions get a grab bag: a 64-bit unsigned oid8 type, casts between bytea and uuid, a regdatabase cast for database names, date/timestamp versions of random(min, max), and full-text stemmers for Polish and Esperanto.

#### The deliberate pullback

Here's the part worth a second look. The most-discussed aspect of Beta 4 isn't what it adds. It's what it took away.

The announcement is blunt: "this beta release reverted several features that were originally planned for PostgreSQL 19." The reasoning is equally straight. The community "strongly believes that, first and foremost, PostgreSQL must be reliable" and wants a predictable schedule. So it's cutting features to protect both.

The removed items:

- SQL/PGQ (Property Graph Query) support
- Online enabling and disabling of data checksums
- Temporal updates/deletes via the FOR PORTION OF clause
- ALTER TABLE ... MERGE PARTITIONS and SPLIT PARTITIONS
- The experimental pg_get_role_ddl(), pg_get_tablespace_ddl(), and pg_get_database_ddl() functions

The Open Items wiki confirms these went in as deliberate "revert" commits. It's a candid acknowledgment that shipping reliably trumps shipping on a marketing calendar. Some of these features get a cleaner path back in a later major release, which is honestly better than something half-baked to hit a date.

#### Breaking changes to check

Since 19 is a major release, migration means pg_upgrade, pg_dumpall/pg_restore, or logical replication. Several changes bite, and you'll want to scan them before planning an upgrade.

- JIT is off by default now. It previously ran by default, but the costing was deemed unreliable, so analytical workloads must enable it manually.
- RADIUS authentication is gone. PostgreSQL only supported it over UDP, which the project calls "unfixably insecure." Fair enough.
- standard_conforming_strings is always forced on. Dumps taken with pre-19 tools under the old setting won't load. Recreate them with 19+. The related escape_string_warning variable is removed.
- The MULE_INTERNAL encoding is removed.
- Default max_locks_per_transaction jumps from 64 to 128, because lock-size allocation changed.
- The default index opclasses for inet/cidr shift from btree_gist to plain GiST. pg_upgrade will refuse clusters with the old indexes.
- Carriage returns and line feeds are no longer allowed in database, role, and tablespace names.
- json_array() returning no rows now yields an empty array instead of NULL.
- A new password_expiration_warning_threshold variable (default seven days) warns about expiring passwords, and MD5 authentication now emits a warning.

#### Fixes since Beta 3

Standard pre-release housekeeping, but a heavy batch. Beta 4 corrected the foreign-key performance work and hardened the new commands. REPACK crashes and odd behavior with invalid indexes and materialized views are handled. WAIT FOR deadlocks are addressed, with clearer isolation-level error reporting. Autovacuum scoring corner cases, including TOAST tables, got fixed. There's also a crash fix for accessing a partition whose concurrent detach didn't complete.

Keep in mind that a few open items remain, including race conditions in REPACK (CONCURRENTLY) logical decoding and some postgres_fdw statistics-import atomicity concerns. All publicly tracked, all discussed openly on the mailing lists.

#### How to test it

The project emphasizes that each release's stability depends heavily on community testing with real workloads. Because it's a beta, "minor changes to database behaviors, feature details, and APIs are still possible," and the project strongly advises against using betas in production. Tools and drivers that depend on PostgreSQL are encouraged to test now.


Download the source, read the docs, and keep an eye on the open items list. Head here for the PostgreSQL 19 Beta 4 source, and head here for the release notes Submit bugs through the project's bug form. As the cycle closes toward the RC and GA, testing volume will remain the clearest signal of when 19 is truly ready.
