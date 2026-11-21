---
id: collect-261121-veille-ia/langages-dev/postgresql
title: "PostgreSQL¶"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["postgresql", "data", "database", "major", "october", "backend", "constraints", "citus"]
source: https://kb.ekarisky.com/databases/postgresql/
source_anchor: ""
source_lines: [1, 147]
section: "Langages_Dev"
---
# PostgreSQL¶

Summary

PostgreSQL is a community-governed, permissively licensed open-source relational database. It is known for strict ACID semantics, a rich SQL dialect (JSONB, full-text search, window functions, temporal constraints) and an extension ecosystem (PostGIS, pgvector, TimescaleDB, Citus) that makes it the default "general-purpose" database for new projects. The current major version is **PostgreSQL 18** (GA 2025-09-25, latest minor **18.6** on 2026-08-13). Its headline features are an asynchronous I/O subsystem, B-tree skip scan, `uuidv7()`, virtual generated columns and OAuth authentication. **PostgreSQL 19** reached Beta 4 on 2026-09-24, with GA targeted for October 2026. **PostgreSQL 14 reaches end-of-life on 2026-11-12.**

## Key Facts¶

| Attribute | Detail | 
|---|---|
| **Website** | postgresql.org | 
| **Latest Version** | 18.6 (2026-08-13). 18.5 was never released | 
| **Next Major** | 19 Beta 4 (2026-09-24). RC expected early October, GA targeted for October 2026 | 
| **Supported Majors** | 14 (EOL 2026-11-12), 15, 16, 17, 18. See the support matrix | 
| **Release Cadence** | One major per year (September/October), 5 years of support each. Minors quarterly (second Thursday of February/May/August/November). Next: 2026-11-12 | 
| **License** | PostgreSQL License (OSI-approved, BSD/MIT-style) | 
| **Governance** | PostgreSQL Global Development Group (PGDG), led by a core team. No single company owns the project | 
| **Language** | C | 
| **Default Port / Protocol** | 5432. Frontend/backend protocol 3.0, and 3.2 since PG 18 | 
| **Source** | git.postgresql.org (read-only mirror: github.com/postgres/postgres) | 

## Architecture at a Glance¶

PostgreSQL forks one backend process per connection. The backends share a buffer pool and WAL buffers, and every change is written to the WAL before data files, which also feeds replicas and archives. The full diagrams are in Explanation.

```
flowchart LR
    APP["App / PgBouncer"] --> PM["postmaster :5432"]
    PM --> BE["backend per connection"]
    BE --- SB["shared_buffers"]
    BE --> WALB["WAL buffers"] --> WAL["pg_wal/"]
    SB --> CK["checkpointer / bgwriter"] --> DATA["data files (base/)"]
    IOW["io workers or io_uring (PG 18 AIO)"] --> SB
    DATA --> IOW
    WAL --> WS["walsender"] --> STBY["hot standby / logical subscriber"]
    WAL --> ARCH["archiver"] --> REPO["WAL archive / pgBackRest repo"]
```
## Evaluation¶

| Pros | Cons | 
|---|---|
| Very feature-rich SQL: JSONB, full-text search, ranges and temporal constraints, `MERGE` , window functions | Scales vertically. Write scale-out needs sharding (Citus) or a distributed SQL database | 
| AIO in v18: faster scans and VACUUM, especially on cloud block storage | Process-per-connection model, so a pooler (PgBouncer) is needed at high connection counts | 
| Native `uuidv7()` (v18) for index-friendly UUID keys | Heap MVCC means VACUUM, bloat and XID-wraparound management | 
| Extensions: PostGIS, pgvector, TimescaleDB, Citus, pgAudit | No built-in automatic failover. HA needs Patroni, CloudNativePG or a managed service | 
| Permissive PostgreSQL License, with no single-vendor relicensing risk | No built-in TDE (column-level pgcrypto, disk encryption, or vendor TDE builds) | 
| OAuth 2.0 authentication (v18), SCRAM with channel binding | Major upgrades need `pg_upgrade` , dump/restore, or logical replication | 
| Mature logical replication (parallel apply and conflict logging in 18, sequences in 19) | Aggressive quarterly security fixes: 18.6 fixed 28 security issues, so plan for regular patching | 

### When It Fits¶

- A general-purpose OLTP backend for most web and SaaS applications, especially when you want one database for relational, JSON, geospatial and vector data.
- Workloads that need strict correctness (constraints, serializable isolation, transactional DDL).
- Kubernetes-native platforms, using CloudNativePG, or managed services (RDS/Aurora, Cloud SQL/AlloyDB, Azure Flexible Server, Neon, Supabase).

### When to Look Elsewhere¶

- Multi-region active-active writes with automatic sharding: see CockroachDB.
- Very high connection counts without a pooler, or teams already standardized on MySQL tooling: see MySQL.
- Very large analytical scans: a columnar engine is usually better, unless you add a columnar extension.

## v18 Highlights¶

| Feature | Detail | 
|---|---|
| **Async I/O** | `io_method` =`worker` (default),`io_uring` (Linux) or`sync` . Speeds up sequential scans, bitmap heap scans and VACUUM. The PGDG announcement reported up to 3x faster reads from storage in its tests | 
| **B-tree Skip Scan** | Multicolumn indexes can be used without a condition on the leading column | 
| **UUIDv7** | Native `uuidv7()` generates time-ordered keys with better B-tree locality than random v4 | 
| **Virtual generated columns** | Computed when read. This is now the default kind of generated column | 
| **`OLD`/`NEW` in `RETURNING`** | Before and after values in `INSERT` /`UPDATE` /`DELETE` /`MERGE` | 
| **Temporal constraints** | `WITHOUT OVERLAPS` primary and unique keys,`PERIOD` foreign keys | 
| **Data checksums default** | `initdb` enables them for new clusters | 
| **pg_upgrade keeps statistics** | No planner "cold start" after a major upgrade. New `--swap` mode | 
| **OAuth 2.0** | `oauth` method in`pg_hba.conf` plus pluggable token validators | 
| **Logical replication** | Parallel streaming by default, conflict logging, replication of generated columns | 
| **MD5 deprecated** | Warnings when MD5 passwords are set. Move to SCRAM-SHA-256 | 

The complete feature and compatibility tables are in Reference — PostgreSQL 18 Feature Reference.

## PostgreSQL 19 Status¶

As of 2026-09-25, PostgreSQL 19 has shipped Beta 1 (2026-06-04), Beta 2 (2026-07-16), Beta 3 (2026-08-13) and Beta 4 (2026-09-24). Headline features are `REPACK (CONCURRENTLY)`, which replaces `VACUUM FULL`/`CLUSTER`, the `pg_plan_advice` modules, auto-scaling I/O workers, logical replication without a restart and replication of sequences, `WAIT FOR LSN` on standbys, and parallel autovacuum. **SQL/PGQ (property graph queries) and online checksum toggling were reverted in Beta 4.** JIT becomes off by default and RADIUS authentication is removed. Details are in Reference — PostgreSQL 19 (Beta) Reference.

## Ecosystem¶

| Layer | Main projects (latest as of 2026-09-25) | 
|---|---|
| Extensions | pgvector 0.8.6, PostGIS 3.6.4 (3.7 in RC), TimescaleDB 2.30.1, Citus 14.0.0, pgAudit, pg_stat_statements | 
| Connection pooling | PgBouncer 1.26.0 (security release 2026-09-23), Supavisor, managed-service proxies | 
| HA / failover | Patroni 4.1.5, CloudNativePG 1.30.1 (Kubernetes operator, CNCF Sandbox) | 
| Backup | pgBackRest 2.59.1, Barman, `pg_basebackup` | 
| Managed services | Amazon RDS/Aurora, Google Cloud SQL/AlloyDB, Azure Database for PostgreSQL, Neon, Supabase, Crunchy Bridge | 

The version table with licenses and links is in Reference — Ecosystem Versions.

## Licensing and Governance¶

PostgreSQL is released under the PostgreSQL License, a short permissive license similar to BSD/MIT. The PGDG, a volunteer community with a core team, develops it, and many companies contribute (EDB, Microsoft, AWS, Google, Crunchy Data, and others). The project has never been relicensed, and it has no single corporate owner. That is a key difference from databases that moved to source-available licenses. Some extensions have their own licenses: PostGIS is GPL-2.0, Citus is AGPL-3.0, and TimescaleDB is Apache-2.0 plus the Timescale License.

## Topic Map¶

- How-to Guides: install, upgrade, CloudNativePG deployment, tuning, backup, monitoring, security setup, pgvector, Commands & Recipes
- Reference: version and support matrix, PG 18/19 feature tables, configuration parameters, limits, auth and privilege tables, hardening checklist, benchmarks
- Explanation: architecture, WAL, MVCC and VACUUM, asynchronous I/O, replication, design trade-offs, security model

## Related Topics¶

- Database Comparison — PostgreSQL vs MySQL vs CockroachDB
- MySQL and CockroachDB, which speaks the PostgreSQL wire protocol
- Kubernetes: the platform for CloudNativePG deployments
- HashiCorp Vault: dynamic PostgreSQL credentials through the database secrets engine
- LLM Fundamentals: pgvector as a RAG vector store
- Longhorn and Ceph: persistent volumes under Kubernetes-hosted PostgreSQL
- ZITADEL: an example application that uses PostgreSQL as its event store

## Sources¶

- PostgreSQL Documentation (18)
- PostgreSQL 18 Release Notes
- PostgreSQL 18.6 Release Notes
- PostgreSQL 18.6, 17.11, 16.15, 15.19, 14.24 and 19 Beta 3 Released!
- PostgreSQL 19 Beta 4 Released!
- PostgreSQL 19 Release Notes (draft)
- PostgreSQL Versioning Policy and endoflife.date — PostgreSQL
- PostgreSQL Security Information
- PostgreSQL License
- GitHub mirror (postgres/postgres): `release-18.sgml` ,`release-19.sgml` and`config.sgml` were used to cross-check dates and defaults
- The Internals of PostgreSQL
- pgvector: vector search
- CloudNativePG: Kubernetes operator
- PgBouncer NEWS

## Questions¶

### Answered¶

- **Q: UUIDv4 or UUIDv7?** Use UUIDv7 for primary keys. It is time-ordered, so inserts go to the right-hand edge of the B-tree (less page splitting and better cache locality than random v4). It is native as`uuidv7()` in PG 18. Keep in mind that it exposes the creation timestamp.
- **Q: Is SQL/PGQ (graph queries) in PostgreSQL 19?** No. It was in Betas 1–3 but reverted in Beta 4 (2026-09-24). PostgreSQL 20 (about September 2027) is the earliest release that could include it.
- **Q: When must PostgreSQL 14 be upgraded?** Its final release is 2026-11-12. Move to 17 or 18. See the support matrix.
- **Q: Which features besides SQL/PGQ and online checksums did Beta 4 revert?** Four:`ALTER TABLE ... MERGE/SPLIT PARTITION(S)` ,`UPDATE` /`DELETE ... FOR PORTION OF` , more object types inside`CREATE SCHEMA` , and the`pg_get_role_ddl()` ,`pg_get_tablespace_ddl()` and`pg_get_database_ddl()` functions. All were reverted on`REL_19_STABLE` between Beta 3 and Beta 4 (`REL_19_STABLE` commit log, checked 2026-09-28).

### Open¶

- **Q: What is the best connection pooling strategy for PG 18?** PgBouncer (transaction mode, now tracking`search_path` by default in 1.26) vs Supavisor vs managed-service poolers. Evaluate session vs transaction mode trade-offs for AIO-heavy workloads.
- **Q: How does pgvector compare to dedicated vector databases (Qdrant, Weaviate) for RAG workloads at scale?** No controlled benchmark is recorded in this vault yet.
- **Q: How much does `io_method = io_uring` gain over `worker` on typical cloud block storage?** Only the PGDG "up to 3x" read figure is recorded here, and there are no independent benchmarks yet.
- **Q: When exactly will PostgreSQL 19 go GA?** The release team targets October 2026. As of 2026-09-28 the newest tag is`REL_19_BETA4` and no RC or GA date is announced; check the PGDG announcements after the RC.
