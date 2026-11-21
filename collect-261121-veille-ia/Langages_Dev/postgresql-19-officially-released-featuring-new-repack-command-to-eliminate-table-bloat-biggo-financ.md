---
id: collect-261121-veille-ia/langages-dev/postgresql-19-officially-released-featuring-new-repack-command-to-eliminate-table-bloat-biggo-financ
title: "PostgreSQL 19 Officially Released, Featuring New 'REPACK' Command to Eliminate Table Bloat — BigGo Finance"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["postgresql", "repack", "command", "table", "advice", "bloat", "changes", "logical"]
source: https://finance.biggo.com/news/4bb32f7f-e55a-4ba6-9c2e-96ee6adfe06c
source_anchor: ""
source_lines: [1, 40]
section: "Langages_Dev"
---
# PostgreSQL 19 Officially Released, Featuring New "REPACK" Command to Eliminate Table Bloat

The PostgreSQL Global Development Group officially released PostgreSQL 19, its open-source relational database (RDB) system, on November 19. The changes center on simplifying both operations management and development for large-scale workloads.

The headline feature is the addition of a new "REPACK" command that eliminates table bloat. It can reclaim unused space accumulated in tables subject to frequent updates and deletes, and reorder data to match indexes. The conventional "VACUUM FULL" and "CLUSTER" commands block access to the table until cleanup completes, but "REPACK (CONCURRENTLY)" allows reads and writes to continue for most of the process. The two existing commands remain available for backward compatibility.

On the query planner side, a new extension called "pg_plan_advice" has been added to stabilize the selection of execution plans. It uses a mini-language called "plan advice" to try out different execution plans, and the accompanying "pg_stash_advice" extension can store advice and apply it automatically based on query identifiers. This mechanism allows slow execution plans to be corrected without changes on the application side.

In addition, autovacuum can now process indexes with parallel workers, and a new scoring system for prioritizing tables has been introduced. When "wal_level" is set to "replica," the effective WAL level is automatically raised to "logical" when the first logical replication slot is created, enabling logical replication without a restart. Logical replication of sequence values is also supported.

There are also many changes affecting availability and development efficiency. The "WAIT FOR LSN" command on the standby side supports "read-after-write" consistency, in which data written on the primary can be read. The addition of "INSERT ... ON CONFLICT DO SELECT" simplifies the "get or create" pattern, and the "COPY" command can now handle output in JSON format. Foreign key checks have been sped up, with bulk inserts roughly twice as fast in testing.

On the security front, server-side support for Server Name Indication (SNI) allows different TLS certificates to be presented for each destination host name. A warning is issued when md5 password authentication succeeds, while support for RADIUS authentication has been removed. JIT (just-in-time compilation) is disabled by default, and users who run many large analytical queries are encouraged to enable it manually.

Support runs through November 19, 2031. The older "PostgreSQL 14" version reached end of life with the update on November 12. PostgreSQL supports Windows, macOS, Linux, BSD, Solaris and other platforms, and is available for free download from the official website "postgresql.org." The Windows installer can be obtained from EDB's website.

Migrating data from older versions requires either dump and restore, "pg_upgrade," or logical replication. Because the release includes changes that affect compatibility, users are advised to check the release notes before migrating.

Key changes are as follows.

| Item | Details | 
|---|---|
| REPACK | Eliminates table bloat; CONCURRENTLY option keeps reads and writes running | 
| pg_plan_advice / pg_stash_advice | Stabilizes execution plans and automatically applies advice | 
| Autovacuum | Parallel-worker index processing, priority scoring | 
| Logical replication | Supports sequence values, enables without restart | 
| Syntax and commands | INSERT ... ON CONFLICT DO SELECT, JSON output for COPY | 
| Performance | Faster foreign key checks (roughly 2x for bulk inserts) | 
| Security | Server-side SNI support, warning on md5 authentication, RADIUS authentication removed | 
| Default changes | JIT disabled by default | 

Note: Support runs through November 19, 2031. PostgreSQL 14 reached end of life as of November 12, 2026.

This release came several weeks later than planned, as the development community was swamped with a flood of bug reports and security responses stemming from AI/LLM (large language model) activity. Even so, the combination of performance improvements and reduced operational burden makes this an update that companies with large-scale systems cannot ignore for their database infrastructure. In particular, REPACK's concurrent execution capability addresses a long-standing challenge for operations teams that want to perform maintenance without stopping running services.

For enterprises, this is also a moment when migration planning from the end-of-life 14 line becomes pressing. They are expected to carefully examine compatibility impacts in advance and proceed with upgrades in a planned manner.

Once added, BigGo Finance appears first in Google Search Top Stories, so you get the broadest, most up-to-the-minute, and most comprehensive global financial news first.

Add preferred source
