---
id: collect-261121-veille-ia/langages-dev/postgresql-19-released-with-online-table-maintenance-faster-queries
title: "PostgreSQL 19 Released with Online Table Maintenance, Faster Queries"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["postgresql", "also", "maintenance", "queries", "table", "more", "automatically", "command"]
source: https://linuxiac.com/postgresql-19-released-with-online-table-maintenance-faster-queries/
source_anchor: ""
source_lines: [1, 25]
section: "Langages_Dev"
---
The PostgreSQL Global Development Group has announced PostgreSQL 19, the latest major version of the widely used open-source relational database management system.

One of the most important additions is the new `REPACK` command, designed to reclaim unused disk space and reorganize table data. Until now, administrators relied on commands such as `VACUUM FULL` and `CLUSTER` for these tasks, both of which require exclusive locks that block normal access to the affected tables.

PostgreSQL 19 resolves this limitation with `REPACK (CONCURRENTLY)`, which allows tables to be reorganized without blocking reads or writes, which is especially valuable for busy production databases, where maintenance operations can cause significant service interruptions.

Another major improvement concerns query optimization. The new `pg_plan_advice` extension gives administrators more control over the execution plans selected by PostgreSQL’s query planner. In addition, its companion, `pg_stash_advice`, can store these recommendations and automatically apply them to matching queries.

Database maintenance also receives considerable improvements. Autovacuum can now process indexes using parallel workers, and a new scoring mechanism helps determine which tables should receive maintenance first. Queries can also automatically mark table pages as all-visible, reducing work during subsequent vacuum operations.

On the performance side, PostgreSQL 19 improves foreign key constraint checks, with project testing showing nearly double the performance for some bulk insert workloads. The query planner can also convert certain `NOT IN` conditions into more efficient anti-joins and perform aggregation before joins to reduce rows processed.

The asynchronous I/O subsystem, introduced in PostgreSQL 18, has been improved as well. PostgreSQL can now automatically adjust the number of I/O workers based on workload requirements, rather than relying on a fixed configuration.

Regarding logical replication, PostgreSQL 19 can automatically enable the necessary WAL logging level when the first logical replication slot is created, removing the server restart previously needed in configurations using the `replica` WAL level.

The release also introduces logical replication of sequence values, along with more flexible publication and subscription configuration options. For applications that distribute reads across standby servers, the new `WAIT FOR LSN` command provides read-your-writes consistency by allowing queries to wait until a standby has replayed a specified write-ahead log position.

Developers will also find several SQL enhancements. The new `INSERT ... ON CONFLICT DO SELECT` functionality simplifies common get-or-create operations by returning existing rows when an insert encounters a conflict. Additionally, the `COPY` command now supports JSON output, and several window functions, including `lead()`, `lag()`, and `first_value()`, support ignoring `NULL` values.

On the security side, PostgreSQL 19 introduces server-side SNI, allowing different TLS certificates to be presented depending on the hostname used by connecting clients. It also adds warnings for deprecated MD5 password authentication and removes support for RADIUS authentication.

Finally, the release changes several defaults. JIT compilation is now disabled by default because its previous cost-based activation could produce unreliable performance benefits. LZ4 also becomes the default TOAST compression method when PostgreSQL is built with LZ4 support.

For more information, see the official announcement or the complete release notes. PostgreSQL 19 is available for download from the project’s website, with packages and installers provided for Linux, Windows, macOS, and other supported platforms.
