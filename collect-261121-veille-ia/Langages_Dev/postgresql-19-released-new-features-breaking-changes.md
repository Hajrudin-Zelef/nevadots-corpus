---
id: collect-261121-veille-ia/langages-dev/postgresql-19-released-new-features-breaking-changes
title: "PostgreSQL 19 Released: New Features & Breaking Changes"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-11-19"]
keywords: ["postgresql", "plan", "advice", "logical", "features", "repack", "table", "without"]
source: https://tecadmin.net/postgresql-19-released/
source_anchor: ""
source_lines: [1, 180]
section: "Langages_Dev"
---
**PostgreSQL 19.0** was released on **19 November 2026**. That is later than usual: recent major versions have shipped in late September, and PostgreSQL 18 came out on 25 September 2025. The headline features are the new **`REPACK`** command, which can shrink a bloated table **without blocking reads or writes**, the **`pg_plan_advice`** extension for keeping query plans stable, and **“read-your-writes” consistency on standbys** with `WAIT FOR LSN`. Logical replication can now be enabled without a restart, and developers get `INSERT ... ON CONFLICT DO SELECT`, JSON output from `COPY` and `IGNORE NULLS` in window functions.

Some defaults have changed as well: **JIT is now off**, `max_locks_per_transaction` doubles to 128 and RADIUS authentication is gone. Read the breaking changes before you upgrade.

## PostgreSQL 19 at a Glance

| Release date | 19 November 2026 | 
|---|---|
| Version | PostgreSQL 19.0 | 
| Headline features | `REPACK (CONCURRENTLY)` ,`pg_plan_advice` +`pg_stash_advice` ,`WAIT FOR LSN` , logical replication without restart, parallel autovacuum | 
| Developer features | `ON CONFLICT DO SELECT` ,`COPY ... (FORMAT json)` ,`IGNORE NULLS` , SQL/JSON string methods | 
| Changed defaults | JIT off, `max_locks_per_transaction` = 128, TOAST compression = lz4,`standard_conforming_strings` always on | 
| Removed | RADIUS authentication, the `io_workers` setting | 
| Support | 5 years, until November 2031 | 
| Previous release | PostgreSQL 18 (25 September 2025) | 

We tested the examples below on the official `postgres:19` Docker image and on Ubuntu 24.04 and Rocky Linux 10 with the PGDG packages.

## What’s New in PostgreSQL 19

### REPACK: remove table bloat without downtime

Tables with many updates and deletes collect dead space (“bloat”) that slows down queries. Until now, the fix was `VACUUM FULL` or `CLUSTER`, and both lock the table for the whole run. PostgreSQL 19 replaces them with **`REPACK`**, and with the `CONCURRENTLY` option, reads and writes keep working while the table is rebuilt:

```
```
`REPACK (CONCURRENTLY) users;`
In our test, a table with half its rows deleted shrank from **7504 kB to 3144 kB**. `REPACK` can also reorder rows to match an index, like `CLUSTER` did. `VACUUM FULL` and `CLUSTER` still work, for compatibility.

### pg_plan_advice: keep query plans stable

The planner usually picks a good plan, but as data grows it can switch to a slower one overnight. The new **`pg_plan_advice`** extension lets you pin plan choices, or try other plans, with a small “plan advice” language. The companion **`pg_stash_advice`** extension stores advice and applies it automatically to queries with a matching query ID, so you can fix a slow plan **without changing application code**.

### Smarter autovacuum

- Autovacuum can process indexes with **parallel workers** , controlled by the new`autovacuum_max_parallel_workers` setting.
- A new **scoring system** decides which tables to vacuum first. You can see the scores in the`pg_stat_autovacuum_scores` view.
- Queries that read a table can mark pages as all-visible while they run, which leaves less work for later vacuums.
- `vacuumdb --analyze-only` now also analyzes partitioned tables when you don’t name specific tables.

### Better monitoring

New views include `pg_stat_lock` (statistics per lock type), `pg_stat_recovery` (recovery status) and `pg_stat_autovacuum_scores`. `EXPLAIN ANALYZE` gets an `IO` option that reports asynchronous I/O activity, and the WAL written for full-page images now shows up in `pg_stat_wal`, `EXPLAIN (ANALYZE, WAL)` and the vacuum logs.

### Logical replication without a restart

Logical replication used to require `wal_level = logical` and a server restart. With the default `wal_level = replica`, PostgreSQL 19 now raises the effective level to logical automatically when you create the first logical replication slot. A new read-only setting shows the level in use:

```
```
```
SHOW effective_wal_level;
 effective_wal_level
---------------------
 replica
```
Logical replication also learned to:

- Replicate **sequence values** with`CREATE PUBLICATION ... FOR ALL SEQUENCES` .
- Skip tables in `FOR ALL TABLES` publications with a new`EXCEPT` clause.
- Take connection settings for `CREATE SUBSCRIPTION` from a foreign server (the`SERVER` clause).
- Detect updates that conflict with a concurrent delete, with the `retain_dead_tuples` subscription option.

### Read-your-writes on standbys: WAIT FOR LSN

Applications that send reads to a replica can miss a row they just wrote on the primary, because the replica is a little behind. The new **`WAIT FOR LSN`** command makes a standby session wait until it has replayed the WAL up to a given log sequence number, then run the query. The application gets the LSN after its write on the primary, and the read on the standby is guaranteed to see that write.

`postgres_fdw` also improves: with the new `import_stats` option, `ANALYZE` copies table statistics from the remote server instead of sampling remote rows.

### INSERT … ON CONFLICT DO SELECT

The common “get or create” pattern now takes a single statement. When the row already exists, `RETURNING` returns the existing row instead of nothing:

```
```
```
INSERT INTO users (email, name) VALUES ('a@example.com', 'Ignored')
ON CONFLICT (email) DO SELECT
RETURNING id, email, name;
 id |     email     | name
----+---------------+-------
  1 | a@example.com | Alice
```
### JSON output from COPY

`COPY` can now write JSON, one object per row or as a single JSON array:

```
```
```
COPY (SELECT id, email FROM users ORDER BY id LIMIT 2) TO STDOUT (FORMAT json);
{"id":1,"email":"a@example.com"}
{"id":3,"email":"u1@example.com"}
```
### Other SQL additions

- `lead()` ,`lag()` ,`first_value()` ,`last_value()` and`nth_value()` accept**`IGNORE NULLS`** , for example`lag(x) IGNORE NULLS OVER (ORDER BY i)` .
- The SQL/JSON path language gets string methods such as `lower()` ,`upper()` ,`replace()` ,`split_part()` and`btrim()` .
- `random()` can generate random dates and timestamps within a range.

### Faster without code changes

- **Foreign key checks** are faster, which made bulk inserts nearly 2x faster in the project’s testing.
- The planner turns `NOT IN` into a faster anti-join when no NULLs are involved, and can**aggregate before a join** (eager aggregation) to process fewer rows.
- With the default `io_method = worker` , the number of asynchronous I/O workers now scales automatically between`io_min_workers` (default 2) and`io_max_workers` (default 8).
- `LISTEN` /`NOTIFY` scales better with many channels.

### Security

- **TLS SNI support:** one server can present different certificates depending on the hostname a client connects to. You map hostnames to certificates in the new`pg_hosts.conf` file.
- A successful **md5 password login now logs a warning** (turn it off with`md5_password_warnings` ). md5 was deprecated in PostgreSQL 18 and will be removed, so move to SCRAM.
- The new `password_expiration_warning_threshold` setting warns users before their password expires (default: 7 days).

## Breaking Changes: Check Before You Upgrade

These changes from the official migration notes are the ones most likely to affect existing systems:

- **JIT is disabled by default.** If you run many large analytical queries, test them and set`jit = on` if they got slower.
- **RADIUS authentication is removed.** Change any`radius` lines in`pg_hba.conf` before upgrading.
- **`standard_conforming_strings` is always on.** Dumps from older`pg_dump` versions taken with it set to`off` won’t load correctly. Use the PostgreSQL 19`pg_dump` for dump-and-restore upgrades.
- **`max_locks_per_transaction` default goes from 64 to 128** , and lock memory is sized differently. If you set it yourself, double your value to keep the same capacity.
- **`io_workers` is removed** and replaced by new settings such as`io_min_workers` and`io_max_workers` . Remove it from`postgresql.conf` .
- **`pg_upgrade` refuses some clusters:** ones with btree_gist indexes on`inet` /`cidr` columns, and ones with carriage returns or line feeds in database, role or tablespace names.
- `json_array()` over zero rows now returns`[]` instead of NULL.
- `pg_stat_subscription_stats.sync_error_count` is renamed to`sync_table_error_count` . Update any monitoring queries that use it.
- Parallel vacuum workers now share the cost-based delay limit, so a throttled parallel `VACUUM` may run slower than before.
- When built with LZ4 (as the PGDG and Docker packages are), new TOAST data is compressed with **lz4** by default.

## How to Install PostgreSQL 19

PostgreSQL 19 packages are already in the official PGDG repositories for Ubuntu 26.04 and 24.04, Debian 13 and 12, and RHEL, Rocky Linux and AlmaLinux 10 and 9. Distribution repositories will keep their current versions, so use PGDG.

### Ubuntu and Debian

`sudo apt install -y postgresql-common`
`sudo /usr/share/postgresql-common/pgdg/apt.postgresql.org.sh`
`sudo apt install -y postgresql-19`

### RHEL, Rocky Linux and AlmaLinux 10

`sudo dnf install -y https://download.postgresql.org/pub/repos/yum/reporpms/EL-10-x86_64/pgdg-redhat-repo-latest.noarch.rpm`
`sudo dnf install -y postgresql19-server`
`sudo /usr/pgsql-19/bin/postgresql-19-setup initdb`
`sudo systemctl enable --now postgresql-19`

On version 9, use `EL-9-x86_64` in the URL and run `sudo dnf -qy module disable postgresql` before installing, so the built-in PostgreSQL module doesn’t get in the way.

### Docker

```
```
`docker run -d --name pg19 -e POSTGRES_PASSWORD=secret -p 5432:5432 postgres:19`
For a full setup with users, remote access and security, see our guides to install PostgreSQL on Ubuntu 24.04 and install PostgreSQL on RHEL 9.

## How to Upgrade from PostgreSQL 18

A major upgrade needs `pg_upgrade`, a dump and restore, or logical replication. **Take a full backup first**, and test the upgrade on a copy of your data. On Ubuntu and Debian, `pg_upgradecluster` wraps `pg_upgrade` and handles the config files and ports for you. We tested this sequence from 18 to 19:

`sudo apt install -y postgresql-19`
`sudo pg_upgradecluster --method=upgrade 18 main`
`pg_lsclusters`

```
```
```
Ver Cluster Port Status Owner    Data directory              Log file
18  main    5433 down   postgres /var/lib/postgresql/18/main /var/log/postgresql/postgresql-18-main.log
19  main    5432 online postgres /var/lib/postgresql/19/main /var/log/postgresql/postgresql-19-main.log
```
The new cluster takes over port 5432, and the old one is stopped on 5433. Once your applications work on 19, remove the old cluster with `sudo pg_dropcluster 18 main`. Run `ANALYZE` (or `vacuumdb --all --analyze-in-stages`) after the upgrade so the planner has fresh statistics.

On RHEL-based systems, stop both services and run `/usr/pgsql-19/bin/pg_upgrade` with the old and new `bin` and `data` directories, starting with the `--check` option.

## Should You Upgrade?

For **new projects**, start on PostgreSQL 19: it is supported until November 2031. For **existing databases**, there is no rush. PostgreSQL 18 is supported until November 2030, and many teams wait for the first point release (19.1) and for their extensions (PostGIS, TimescaleDB, pgvector and others) to publish builds for 19. Upgrade sooner if table bloat, unstable query plans or replica read consistency are real problems for you, because `REPACK`, `pg_plan_advice` and `WAIT FOR LSN` address them directly.

Track every PostgreSQL version’s support dates on our PostgreSQL release tracker. Sources: the official release announcement and the PostgreSQL 19 release notes.

How did it make you feel?
