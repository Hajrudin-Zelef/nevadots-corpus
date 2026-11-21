---
id: collect-261121-veille-ia/langages-dev/postgresql-vs-mysql-which-database-should-you-choose-in-2026-bytebase
title: "PostgreSQL vs MySQL: Which Database Should You Choose in 2026? | Bytebase"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["mysql", "postgres", "postgresql", "most", "database", "license", "still", "release"]
source: https://www.bytebase.com/blog/postgres-vs-mysql/?utm_content=257418031&utm_medium=social&utm_source=twitter&hss_channel=tw-2599580401
source_anchor: ""
source_lines: [1, 185]
section: "Langages_Dev"
---
This is a series of articles between MySQL and PostgreSQL:

- MySQL vs. Postgres (this one)
- PlanetScale vs. Neon
- TiDB vs. CockroachDB

For the last three years, the Postgres vs MySQL comparison had a boring answer. Postgres was what you picked, MySQL was what you inherited. 2026 made the question worth asking again, because three things landed within a few months of each other:

1. MySQL 8.0, still the most deployed version in the wild, reached end of life in April 2026. The largest install base in the industry has to move whether it wants to or not.
2. MySQL 9.7 LTS shipped on April 21, 2026, the first LTS since 8.4, and pushed the new optimizer, Group Replication observability, and OpenTelemetry into free Community (my full read). Three months later Oracle changed how MySQL is numbered: MySQL 26.7.0, released July 28, 2026, is the first calendar-versioned release. There is no MySQL 9.8 and there will not be one.
3. PostgreSQL 19 froze its feature set in April and shipped Beta 1 on June 4, then slipped. Beta 4 is dated September 24, there is no release candidate date, and the release team's stated goal is GA "by the end of October". At least five features were pulled during the betas, including SQL/PGQ property graphs, `FOR PORTION OF` , and online data checksums. What survived is still the biggest operational release in years: in-core plan advice, online`REPACK` , parallel autovacuum, and the end of the MultiXact ceiling (details).

So a lot of teams are re-picking right now, most of them on 2023 information.

The long-term trend has not reversed. In the September 2026 DB-Engines ranking MySQL still sits at #2 and Postgres at #4, but MySQL lost 47 points year over year while Postgres gained 26. The gap is 161 points and shrinks every year. In the Stack Overflow survey (2025, the 2026 edition is not out yet) Postgres is the most used database among professional developers (58.2% vs 39.6% for MySQL), the most admired database for the fourth year in a row (65.5%), and the most desired (47%). MySQL still leads among people learning to code, which tells you where each one's install base comes from.

MySQL is probably still the world's most popular open source database by install base, while Postgres positions itself as the world's most advanced open source relational database.

At Bytebase, we work with both databases extensively since the Bytebase product needs to integrate with both databases as well as their derivatives. Our founders also build Google Cloud SQL, one of the largest hosted MySQL & Postgres cloud services.

Based on our operating experience, below we compare the two across license, connection model, performance, features, extensibility, usability, operability, and ecosystem.

*Unless otherwise specified, the comparison below is between the current stable releases, Postgres 18 vs. MySQL 9.7 LTS (using InnoDB). Postgres 19 items come from Beta 3 and Beta 4 and are not GA yet. MySQL 26.7 items come from the 26.7.0 and 26.7.1 Innovation releases, which are supported only until the next Innovation release. We also use Postgres instead of PostgreSQL throughout the article, though we know the latter is the official name, which is considered as the biggest mistake in Postgres History*.

MySQL Community Edition is licensed under GPL. Postgres is released under the PostgreSQL License, a liberal Open Source license similar to the BSD or MIT licenses.

GPL is infectious: distribute MySQL as part of your software and you owe your source under a GPL-compatible license, unless you buy a commercial license from Oracle. And it's Oracle's ownership, not the license, that is why MariaDB forked, and why a few of the capabilities below (masking, JavaScript stored programs) live only in the paid Enterprise Edition. Postgres has no such split.

The Enterprise line does move, in one direction. The Thread Pool plugin was Enterprise-only since 5.5. 26.7 puts it in Community. The Group Replication diagnostics went the same way in 9.7. To be fair to Oracle, the paywall is shrinking, just slowly.

Postgres uses process per connection where each connection spawns a new process. MySQL uses thread per connection where each connection spawns a new thread.

Postgres gets better isolation this way: an invalid memory access bug crashes one process instead of the whole server. The process model also costs more resources, so a Postgres production deployment should proxy connections through a pooler such as PgBouncer or pgcat. A serverless application that opens connections carelessly will melt a Postgres instance long before it bothers MySQL.

MySQL 26.7 widens that gap. The Thread Pool plugin now ships in Community Edition, so a connection storm is absorbed inside the server by a fixed set of worker threads instead of one thread per client. Postgres still has nothing in core for this. A pooler in front of Postgres is not optional in 2026, and it is now optional in MySQL.

For most workloads, the performance between Postgres and MySQL is comparable with at most 30% variations. On the other hand, regardless of which database you choose, if your query misses an index, it could be 10x ~ 1000x degradation. The index matters more than the engine.

Most published "Postgres vs MySQL benchmark" posts measure configuration, not the engine. Both ship with defaults sized for a laptop: `shared_buffers` at 128MB, `innodb_buffer_pool_size` at 128MB. Whichever side the author tuned wins by 2x. When someone shows me a chart with a 2x gap, I ask for those two settings before I read the chart. If you must benchmark, run pgbench and sysbench against your own schema on the instance size you will actually buy, with the buffer pool set to the same fraction of RAM on both.

Where the engines genuinely moved this cycle:

- Postgres 18 introduced an asynchronous I/O subsystem, selected by the `io_method` variable (`io_uring` on Linux, a worker-based fallback elsewhere). The first wave covers reads: sequential scans, bitmap heap scans, and vacuum. Postgres 19 scales the worker pool automatically (`io_min_workers` ,`io_max_workers` ) instead of making you guess a fixed count.
- Postgres 19 also turns JIT compilation off by default. If you have ever watched Postgres spend 200ms compiling a query that runs in 2ms, that default was the reason. It changes the TOAST compression default from `pglz` to`lz4` , adds a radix sort, and vectorizes`COPY FROM` . Foreign-key checks got a per-row fast path; the batched version was reverted on September 10, so do not expect the bigger number from the early beta posts.
- MySQL's performance news this cycle is on the planning side, the Hypergraph Optimizer described below. 26.7 adds two smaller things that matter on hot tables: `innodb_autoinc_preallocate` (default 50) persists the auto-increment counter once per batch instead of once per insert (a crash can skip the reserved values, which is the price), and the Thread Pool above.

Saying that, MySQL does have an edge for extreme write-intensive workloads, as Uber and OtterTune both documented. InnoDB keeps old row versions in the undo log rather than the heap, so a table that is updated ten thousand times a second does not bloat and does not need vacuum. Unless your business reaches Uber-like scale, sheer performance is not a deciding factor. Companies like Instagram and Notion are also able to herd Postgres at super scale.

MySQL employs a 4 level system: `Instance.Database.Table.Column`

Postgres employs a 5 level system: `Instance.Database.Schema.Table.Column` (Instance in Postgres is often called `Cluster`).

The extra level is the first thing that bites during a migration. A MySQL "database" is closer to a Postgres schema than to a Postgres database.

Both databases provide ACID transactions. Overall, Postgres provides stronger transaction support:

The practical consequence is migration safety. A failed multi-statement migration rolls back cleanly on Postgres and leaves MySQL half-applied. Check out Postgres vs. MySQL: DDL Transaction Difference for a detailed analysis.

Postgres gives you Row Level Security (RLS) in core. MySQL gives you column masking, and only in Enterprise.

Both support RBAC. Postgres supports RLS out of the box, while MySQL needs extra views to emulate it. MySQL 9.7 Enterprise moves the other way with a first-class Dynamic Data Masking policy object: attach it to a column and every read path goes through it, including `SELECT *` and `mysqldump`. Postgres has no in-core equivalent. Each engine covers the half the other misses. MySQL hides the value, Postgres hides the row.

For authentication, Postgres 18 adds OAuth 2.0 and MySQL 9.1 added WebAuthn. Both are retiring the old stuff: Postgres 19 removes RADIUS authentication and logs a warning on every MD5 login, so a fleet still on `md5` should move to `scram-sha-256` before the upgrade, not after. MySQL 26.7 goes further on the wire and adds post-quantum key exchange and handshake signatures for TLS 1.3 (OpenSSL 3.5 or later, `tls_kex` and `force_pqc`). Nobody's auditor asks for that yet. In two years they will.

Postgres still has the better optimizer out of the box. MySQL 9.7 narrowed the gap for join-heavy queries by moving the Hypergraph Optimizer into Community Edition.

The Postgres advantage is long-standing, and this rant explains why practitioners feel it. Postgres 18 adds skip scan for multicolumn B-tree indexes, and 19 adds `pg_plan_advice`, an in-core planner-advisor framework, plus a companion `pg_stash_advice` that applies stored advice by query automatically, after years of telling users to fix bad plans with `pg_hint_plan` or `SET enable_*` flags. Both extensions survived the beta reverts.

MySQL's classical optimizer is a left-deep, greedy join enumerator: fast to plan, fine on OLTP, out of its depth once an analytical query joins many tables. The Hypergraph Optimizer considers bushy plans and picks hash versus nested-loop per join on cost. It is experimental and off by default, still off in 26.7, and on Oracle's published TPC-DS run 14 queries regressed by 50% or more even though the wins outnumbered the losses. The 9.7.2 and 26.7.0 release notes each carry a stack of hypergraph bug fixes, which is what you would expect from a planner this young. Turn it on per statement with `SET_VAR`, not globally.

MySQL has the more complete online DDL story. Postgres closes part of the gap in 19.

Postgres provides online DDL for the following cases:

1. `ADD COLUMN` without a default value.
2. (Postgres 11+) `ADD COLUMN` with a default value.
3. Specify `CONCURRENTLY` when running`CREATE INDEX` .
4. (Postgres 18+) `NOT NULL` constraints can be added without a full table scan.
5. (Postgres 19 beta) `REPACK CONCURRENTLY` rebuilds a bloated table without an`ACCESS EXCLUSIVE` lock, replacing`VACUUM FULL` ,`CLUSTER` , and the third-party`pg_repack` . It survived to Beta 4 with its scope cut in September: heap tables only, no materialized views or catalog tables, one`REPACK` per cluster at a time, and the patch author documents that a transaction running alongside it can see rows it should not. Plan on it, but keep`pg_repack` installed until 19.2.

MySQL covers more of it natively. `ALTER TABLE` takes an `ALGORITHM` of `INSTANT`, `INPLACE` or `COPY`, and gh-ost and pt-online-schema-change fill the gaps where it is limited.

Postgres replicates physically by default, MySQL logically. The bigger difference is failover: MySQL ships it inside the server, Postgres hands it to external tools.

For Postgres, the standard replication is physical replication using WAL, with logical replication available via Publish/Subscribe. Failover orchestration is not in core, so production clusters run Patroni or an equivalent. Postgres 19 does fix the worst logical-replication footgun: sequences behind `SERIAL` and `IDENTITY` columns were never replicated, so promoting a subscriber produced primary-key violations at cutover. Publications now support sequences. Two more 19 items help the read-replica story: logical replication can be switched on without a restart (`effective_wal_level`), and a new `WAIT FOR` command blocks until a standby has replayed past a given LSN, which is the read-your-writes primitive every ORM has been faking with sleeps.

For MySQL, the standard replication is logical replication using binlog, and Group Replication provides a multi-primary-capable HA topology with automatic primary election and node eviction inside the server. Until 9.7 the components that told you why the group made a decision were Enterprise-only, so you had a built-in HA story you could not debug at 3am without a contract. 9.7 moves them into Community, which turns a paywalled lead into a real one. 26.7 then rewrites the apply side: the Change Stream Applier is a new replica applier, opt-in per channel (`APPLIER_VERSION = 2`), with up to 1,024 worker threads. It needs GTIDs and row-based binlogs, and it is the first real answer to replica lag under a write burst since multi-threaded replication in 5.7. 26.7 also flips the Group Replication default communication stack from `XCOM` to `MYSQL` and deprecates `group_replication_ip_allowlist`. That change is Innovation-only; 9.7.x keeps the old default.

Postgres 18 introduces a native `uuidv7()` function for timestamp-ordered UUIDs, which keeps global uniqueness without wrecking index locality. MySQL has no native v7 generator. The closest equivalent is `UUID_TO_BIN(UUID(), 1)`, which byte-swaps a v1 UUID into time-ordered binary form.

Both Postgres and MySQL support a native JSON column. Postgres has the richer query surface: more operators, and indexes on JSON fields via `jsonb` plus GIN.

MySQL 9.7 answers with something Postgres does not have. JSON Duality Views let an application read and write a JSON document while the server stores normalized relational rows, with `INSERT`, `UPDATE` and `DELETE` now in Community. A constraint violation on the base tables rolls back the document write, instead of being swallowed by an ORM in the middle. 26.7 adds nothing new here beyond a fix to the order in which a document update applies deletes.

Detailed breakdown in Postgres vs. MySQL: JSON Support.

Postgres has a more comprehensive support for CTE:

- SELECT, UPDATE, INSERT, DELETE inside a CTE.
- SELECT, UPDATE, INSERT, DELETE following a CTE.

MySQL supports:

- SELECT inside a CTE.
- SELECT, UPDATE, and DELETE following a CTE.

Both support the standard window function set, `ROWS` and `RANGE` frames included. MySQL 8.0 closed the function gap too, so `LAG()`, `LEAD()`, `FIRST_VALUE()`, and `LAST_VALUE()` are all supported. What Postgres still has over MySQL: the `GROUPS` frame type, the frame `EXCLUDE` clause, `IGNORE NULLS` on the value functions as of 19, and an implementation generally considered more efficient.

Postgres wins the vector workload today. pgvector has become the de-facto standard, with both IVFFlat and HNSW indexing. MySQL 9 introduced a VECTOR type with up to 16,383 dimensions, but there is no ANN index in the core server, and 26.7 did not add one, so approximate search at scale means HeatWave or an external vector store.

Postgres supports extensions, and this is its deepest structural advantage. PostGIS brings geospatial capabilities, pgvector brings vector search, Foreign Data Wrapper (FDW) queries into other data systems, and `pg_stat_statements` tracks planning and execution statistics. None of these required a change to core Postgres.

MySQL has a pluggable storage engine architecture that gave birth to InnoDB. But InnoDB has become the dominant engine, so the pluggable architecture is now an API boundary rather than an extension point. MySQL 9.0+ adds JavaScript stored programs via GraalVM, in Enterprise Edition only.

For auth, both support pluggable authentication module (PAM).

Postgres is more rigorous while MySQL is more forgivable, though the gap is smaller than its reputation suggests.

- MySQL is case-insensitive by default. Postgres is case-sensitive by default.
- MySQL lets you join tables across different databases. Postgres can only join tables inside a single database, unless using the FDW extension.
- MySQL historically allowed non-aggregated columns in a `SELECT` with`GROUP BY` .`ONLY_FULL_GROUP_BY` has been in the default`sql_mode` since 5.7, so this only bites legacy configurations that turned it off.

Where the difference is real is the first week. A new engineer is productive with MySQL in a day. Postgres asks more up front: roles, schemas, `search_path`, vacuum, autovacuum, TOAST, wraparound.

This is where the 2026 decision actually gets made, and it is the section most comparisons skip.

On the MySQL side, the calendar decides for you, and Oracle just changed the calendar. MySQL 8.0 went end of life in April 2026, so the most-deployed version in the world is now unsupported. Starting with 26.7, Innovation releases carry a `YY.M.P` number (26.7 is July 2026) and each one is supported only until the next one lands, roughly quarterly. LTS releases still get five years of premier support plus three extended, and the upgrade rules now run on "compatibility lineages": an LTS may move only into the immediately following lineage, and 9.7 is the only sequentially numbered LTS that can upgrade directly into the first calendar-versioned one. Put simply, an 8.0 fleet that stops at 8.4 will have to hop through 9.7 anyway. Go to 9.7 now. Plan that upgrade as a project, not a patch. We also encountered a few replication bugs when operating a huge MySQL fleet at Google Cloud, though those only surface at extreme load.

On the Postgres side, the tax is vacuum. The XID wraparound issue is the famous one. Its quieter sibling, MultiXact member exhaustion, does not show up on standard XID dashboards and took Metronome down four times in a month in 2025. Postgres 19 widens MultiXact members to 64 bits, eliminating that ceiling rather than raising it, adds parallel autovacuum, and adds a scoring view (`pg_stat_autovacuum_scores`) so you can finally see why autovacuum picked the table it picked.

The other Postgres tax this year is the release itself. Postgres 19 is late: Beta 4 on September 24, no release candidate date, and a target of "end of October" for a version the roadmap still says is "planned for September 2026". The project fixed more than forty CVEs across the February, May, and August minor releases, and the release team said plainly that the beta backlog is what pushed GA to "end of October". Bruce Momjian called it unprecedented. If your upgrade window is Q4, 19 lands in it with a `.0` and one minor release (November 12) behind it. That is fine for most fleets. It is not fine for the ones that wait for `.2`, which now means 2027.

Both are mature. Only the shape of the tax differs: MySQL asks for an upgrade project on Oracle's schedule, Postgres asks for a standing understanding of vacuum.

All common SQL tools support both well. But because Postgres is extensible and still community-owned, its ecosystem has thrived: every application platform offering a hosted database picks Postgres, from Heroku in the early days to Supabase, render, and Fly.io today.

There is also a series of Postgres derived databases targeting different workloads:

- TigerData (the TimescaleDB extension, renamed from Timescale in 2025) for time series.
- FerretDB for MongoDB compatibility.
- RisingWave for streaming.
- Neon for serverless.
- PostgresML for AI.

This is the one dimension where neither 9.7 nor 26.7 changes anything.

|  | Postgres | MySQL | 
|---|---|---|
| Current release | 18 (19 Beta 4, GA targeted end of October) | 9.7 LTS, 26.7 Innovation (calendar-versioned) | 
| License | Postgres License (MIT alike) | GPL Community + paid Enterprise | 
| Connection Model | Process per connection, pooler required | Thread per connection, Thread Pool in Community (26.7) | 
| Performance | Internet scale, async I/O for reads (18) | Better in extreme write-intensive workload | 
| Optimizer | Better default planner, plan advice in 19 beta | Hypergraph Optimizer in Community, experimental | 
| Security | Row Level Security in core, MD5 on its way out | Column masking (Enterprise), post-quantum TLS (26.7) | 
| Online DDL | Partial, `REPACK CONCURRENTLY` in 19 beta (heap only) | `INSTANT` /`INPLACE` plus gh-ost, pt-osc | 
| HA | Streaming replication, failover via Patroni | Group Replication with failover in the server | 
| JSON | Richer operators and indexing | Read-write Duality Views | 
| Vector | pgvector with HNSW and IVFFlat | `VECTOR` type, no ANN index in core | 
| Extensibility | Extensions | Storage engine API only | 
| Usability | Rigorous, more up front | Forgivable, productive in a day | 
| Operability | Vacuum literacy required, 19 delayed to late October | Upgrade on Oracle's calendar, now literally | 
| Ecosystem | More hosting providers and derivatives | Large install base | 

Choosing between Postgres and MySQL is still hard and often causes heated debate.

The table settles nothing on its own. The answer depends on which situation you are in, and four of them come up over and over.

Starting a new application. Pick Postgres. Extensions, the ecosystem, and the hiring market all point the same way, and you inherit no upgrade debt. This is where the consensus is right.

Running a MySQL 8.0 fleet. Your decision is not Postgres versus MySQL, it is an in-place upgrade versus a migration. Stacking a cross-engine migration onto a forced upgrade is two risky projects on one deadline. And if you upgrade, target 9.7, not 8.4: the new lineage rules make 8.4 a dead end that has to pass through 9.7 later anyway.

Write-heavy OLTP with short transactions. MySQL is at home here. InnoDB keeps old row versions in the undo log rather than the heap, and that design difference is behind most of the vacuum complaints on the Postgres side.

AI, vector search, or analytics next to OLTP. Postgres, with the extension ecosystem doing the specialized work.

The tiebreaker beats all four: what your team already knows. The sophistication of Postgres does cost some handiness, and a team fluent in MySQL will ship a safer system on MySQL than a shaky one on Postgres. If you are unfamiliar with Postgres, spin up an instance from a cloud provider and run a couple of queries before you commit.

It's also common that Postgres and MySQL co-exist inside an organization. And if you want to manage the database development lifecycle for both of them, please check out Bytebase.

- MySQL 26.7 Reference Manual: MySQL Releases: Innovation and LTS
- Christophe Pettus: 19th Nervous Breakdown (the PostgreSQL 19 schedule) - Sep 15, 2026
- Dimitri Fontaine: Getting Ready for PostgreSQL 19 - Sep 3, 2026

- GitHub: Automating MySQL schema migrations with GitHub Actions and more - Feb 14, 2020
- Goldman Sachs: Introducing Obevo: Get Your Database SDLC under Control - Dec 1, 2017
