---
id: collect-261121-veille-ia/langages-dev/new-in-postgresql-19-10-new-features-with-visual-results
title: "New in PostgreSQL 19: 10 New Features With Visual Results"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["postgresql", "postgres", "pg19", "new", "same", "features", "query", "results"]
source: https://visualeaf.com/blog/postgresql-19-new-features/
source_anchor: ""
source_lines: [1, 461]
section: "Langages_Dev"
---
PostgreSQL 19 is out, released on November 19, 2026. So what does it actually bring? Instead of summarizing the release notes, we installed it next to PostgreSQL 18 and tried the new features ourselves.

This release is less about new syntax and more about control: over maintenance, over the query planner, and over replication. Some results were easy to see right away. A table we had bloated by deleting 80% of its rows went from 70 MB to 14 MB with the new `REPACK` command. The same `NOT IN` query got a better plan without changing a line of SQL.

We ran every test in VisuaLeaf's SQL Editor, with PostgreSQL 19 (its final release candidate, RC 2) and PostgreSQL 18.7 open in split editors, running the same SQL on both. These are hands-on lab tests on small datasets, not performance benchmarks.

## What we tested

| # | Feature | What we tested | Result | 
|---|---|---|---|
| 1 | REPACK | Reclaiming space after deleting 80% of a table | 70 MB → 14 MB, same 24,000 rows | 
| 2 | NOT IN → anti join | The same NOT IN query on 18 and 19 | Hashed subplan → Hash Anti Join | 
| 3 | LZ4 by default | Storing the same 32 KB value on both versions | 419 bytes → 179 bytes | 
| 4 | Plan advice | Steering the planner, then stashing advice by query ID | Advice applied and marked `matched` | 
| 5 | Auto-scaling I/O workers | Watching the worker pool under repeated scans | 2 → 8 → back to 2 workers | 
| 6 | Logical decoding, no restart | Creating a logical slot on a `replica` server | Effective WAL level became `logical` | 
| 7 | Sequence replication | Syncing a sequence to a subscriber | Both sides at 2000 after refresh | 
| 8 | Autovacuum scores & parallel workers | Ranking tables and vacuuming indexes in parallel | Top score 6.93; 1 parallel worker launched | 
| 9 | WAIT FOR LSN | Read-your-writes on a physical standby | Standby returned `success` | 
| 10 | IGNORE NULLS & ON CONFLICT DO SELECT | Two everyday SQL additions | Last known value; existing row returned | 
|  | Upgrade checklist | Default settings, 18 vs 19 | JIT off, LZ4, 128 locks, removed settings | 

Want to follow along? Start with the test environment. Full setup steps are in the reproduction notes at the end.

## The test environment

Everything ran in Docker, inside a dedicated `pg19_lab` schema.

| Connection | Purpose | 
|---|---|
| PostgreSQL 18.7 | Baseline for plans, compression and settings | 
| PostgreSQL 19 (primary) | Writes and most feature tests | 
| PostgreSQL 19 (subscriber database) | Logical sequence synchronization | 
| PostgreSQL 19 (physical standby) | Replay and `WAIT` | 

To start the two comparison servers (with Docker running; the passwords are local demo credentials):

```
docker run -d --name pg18-demo -p 127.0.0.1:5548:5432 -e POSTGRES_PASSWORD=pg18demo -e POSTGRES_DB=pg18_demo postgres:18.7
docker run -d --name pg19-demo -p 127.0.0.1:5549:5432 -e POSTGRES_PASSWORD=pg19demo -e POSTGRES_DB=pg19_demo postgres:19
```
In VisuaLeaf's Connection Manager, add a PostgreSQL connection for each one: host `localhost`, the port above, user `postgres`. Then confirm which server you're on:

```
SELECT current_setting('server_version') AS postgres_version,
       current_database() AS database_name;
```
Create the lab schema on both servers:

```
CREATE SCHEMA IF NOT EXISTS pg19_lab;
```
Keep auto-commit on, and run maintenance commands (`VACUUM`, `REPACK (CONCURRENTLY)`, `ALTER SYSTEM`) as separate statements.

## 1. REPACK reclaims space after large deletes

Audit and event tables are a classic case: old rows get deleted, but the space they leave stays inside the table file. PostgreSQL can reuse that space, but it doesn't give it back to the operating system.

We created 120,000 events and deleted four out of every five:

```
CREATE TABLE pg19_lab.audit_events (
    id bigint PRIMARY KEY,
    payload text NOT NULL
);
 
INSERT INTO pg19_lab.audit_events (id, payload)
SELECT n, repeat(md5(n::text), 16)
FROM generate_series(1, 120000) AS g(n);
 
DELETE FROM pg19_lab.audit_events
WHERE id % 5 <> 0;
```
After each step, we measured row count and total size (table plus indexes):

```
SELECT count(*) AS event_count,
       pg_size_pretty(pg_total_relation_size('pg19_lab.audit_events')) AS total_size
FROM pg19_lab.audit_events;
```
Then a regular vacuum, followed by the new native command:

```
VACUUM pg19_lab.audit_events;
```
```
REPACK (CONCURRENTLY) pg19_lab.audit_events;
```
| Stage | Rows | Total size | 
|---|---|---|
| Initial data | 120,000 | 70 MB | 
| After DELETE | 24,000 | 70 MB | 
| After VACUUM | 24,000 | 70 MB | 
| After REPACK (CONCURRENTLY) | 24,000 | **14 MB** | 

Until now, this needed `VACUUM FULL` (which blocks the table) or the third-party `pg_repack` extension. `REPACK (CONCURRENTLY)` keeps the table readable and writable for most of the rewrite and takes a brief `ACCESS EXCLUSIVE` lock only for the final swap.

Before using it in production, note that it needs a primary key or a suitable replica identity, enough free disk for a full copy of the table, and must run outside a transaction block. See the REPACK documentation. Routine vacuuming is still required.

## 2. The planner turns NOT IN into an anti join

"Find customers who never placed an order" is often written with `NOT IN`. PostgreSQL 19 can now rewrite that into an anti join when it can prove that no nulls are involved.

We loaded identical data on both versions: 10,000 customers and 4,000 orders, with a `NOT NULL` customer reference.

```
CREATE TABLE pg19_lab.demo_customers (
    id integer PRIMARY KEY
);
 
CREATE TABLE pg19_lab.demo_orders (
    id integer PRIMARY KEY,
    customer_id integer NOT NULL REFERENCES pg19_lab.demo_customers(id)
);
 
INSERT INTO pg19_lab.demo_customers
SELECT n FROM generate_series(1, 10000) AS g(n);
 
INSERT INTO pg19_lab.demo_orders
SELECT n, n FROM generate_series(1, 4000) AS g(n);
 
ANALYZE pg19_lab.demo_customers;
ANALYZE pg19_lab.demo_orders;
```
Same query on both servers:

```
EXPLAIN (ANALYZE, COSTS OFF, TIMING OFF)
SELECT c.id
FROM pg19_lab.demo_customers AS c
WHERE c.id NOT IN (
    SELECT o.customer_id
    FROM pg19_lab.demo_orders AS o
);
```
PostgreSQL 18 used a sequential scan with a hashed subplan filter. PostgreSQL 19 chose a **Hash Anti Join**. Both returned the same 6,000 customers.

The `NOT NULL` constraint is what makes this possible. `NOT IN` behaves differently when nulls are present, so the planner applies the rewrite only when it can prove they aren't.

## 3. LZ4 is the new default TOAST compression

TOAST is how PostgreSQL stores and compresses large values. LZ4 has been available since PostgreSQL 14, but the default was `pglz`. In PostgreSQL 19, the default is `lz4` (when the server is built with LZ4 support, as the official images are).

We stored the same 32,768-byte text value on both servers:

```
CREATE TABLE pg19_lab.demo_toast (
    payload text NOT NULL
);
 
INSERT INTO pg19_lab.demo_toast
VALUES (repeat(md5('audit-event'), 1024));
 
SELECT current_setting('default_toast_compression') AS default_codec,
       pg_column_compression(payload) AS actual_codec,
       octet_length(payload) AS original_bytes,
       pg_column_size(payload) AS stored_bytes
FROM pg19_lab.demo_toast;
```
| Server | Default | Actual codec | Original | Stored | 
|---|---|---|---|---|
| PostgreSQL 18.7 | pglz | pglz | 32,768 | 419 | 
| PostgreSQL 19 | lz4 | lz4 | 32,768 | **179** | 

On this deliberately repetitive value, LZ4 stored 57% fewer bytes. Real data will compress differently, and LZ4's main advantage is usually speed. Changing the default doesn't recompress existing data either: only new values use the new codec. See client connection defaults.

## 4. Plan advice: telling the planner what you want

When a plan goes bad after a statistics change, PostgreSQL users have long had few options besides rewriting the query or installing an extension. PostgreSQL 19 adds `pg_plan_advice`, which can describe a plan, reproduce it and adjust it.

To make the effect obvious, we asked for a sequential scan on a primary-key lookup, which the planner wouldn't normally choose:

```
LOAD 'pg_plan_advice';
 
SET pg_plan_advice.advice = 'SEQ_SCAN(c)';
 
EXPLAIN (COSTS OFF, PLAN_ADVICE)
SELECT c.id
FROM pg19_lab.demo_customers AS c
WHERE c.id = 5000;
```
The plan switched to `Seq Scan`, and the output marked the advice as `matched`. This was only a demonstration: you'd use advice to fix a real plan problem, not to force a worse plan. Reset it afterwards:

```
RESET pg_plan_advice.advice;
```
### Stashed advice applies automatically

The companion `pg_stash_advice` module ties advice to a query identifier, so it applies every time that query runs, with no `SET` in the application.

```
CREATE EXTENSION IF NOT EXISTS pg_stash_advice;
LOAD 'pg_stash_advice';
SET compute_query_id = on;
 
EXPLAIN (VERBOSE, COSTS OFF, PLAN_ADVICE)
SELECT c.id
FROM pg19_lab.demo_customers AS c
WHERE c.id = 5000;
```
The output includes a `Query Identifier`. We used it to create a stash:

```
SELECT pg_create_advice_stash('pg19_demo_advice');
 
-- Use the query identifier from your own EXPLAIN output.
SELECT pg_set_stashed_advice('pg19_demo_advice', 74909709696979601, 'SEQ_SCAN(c)');
 
SET pg_stash_advice.stash_name = 'pg19_demo_advice';
```
Running the same `EXPLAIN` again showed the sequential scan, with the advice coming from the stash.

Query identifiers vary between environments, so get your own. Our stash lived in shared memory for the session; keeping it across restarts needs extra setup, described in the pg_stash_advice documentation. See also the plan advice module.

```
RESET pg_stash_advice.stash_name;
```
## 5. I/O workers scale automatically

PostgreSQL 18 introduced asynchronous I/O with a fixed pool of `io_workers`. PostgreSQL 19 replaces that with a pool that grows and shrinks between a minimum and a maximum.

```
SELECT name, setting AS value, unit
FROM pg_settings
WHERE name IN ('io_method', 'io_workers', 'io_min_workers', 'io_max_workers',
               'io_worker_idle_timeout', 'io_worker_launch_interval')
ORDER BY name;
```
| Setting | PostgreSQL 18.7 | PostgreSQL 19 | 
|---|---|---|
| `io_method` | worker | worker | 
| Worker pool | `io_workers = 3` (fixed) | `io_min_workers = 2` ,`io_max_workers = 8` | 
| `io_worker_idle_timeout` | — | 60 s | 
| `io_worker_launch_interval` | — | 100 ms | 

We then ran repeated scans on a 500,000-row table and watched the pool from a second editor:

```
SELECT count(*) AS io_workers
FROM pg_stat_activity
WHERE backend_type = 'io worker';
```
The count climbed from **2 to 8** under load, then dropped back to **2** once the scans stopped. Your workload may peak at a different number. See I/O configuration.

## 6. Logical decoding without a restart

Turning on logical replication used to mean changing `wal_level` to `logical` and restarting the server. In PostgreSQL 19, a server with `wal_level = replica` raises its **effective** WAL level automatically when the first logical replication slot is created.

```
SELECT current_setting('wal_level') AS configured_wal_level,
       current_setting('effective_wal_level') AS effective_wal_level,
       pg_postmaster_start_time() AS server_started_at;
```
Before any logical slot exists, both levels read `replica`. Then we created a slot:

```
SELECT * FROM pg_create_logical_replication_slot('pg19_demo_slot', 'pgoutput');
```
Re-running the query showed `configured_wal_level = replica`, `effective_wal_level = logical`, and the **same server start time**: no restart happened.

Connection limits, permissions and worker capacity still need to be in place. See WAL configuration. Drop the test slot when you're done, because an unused slot holds back WAL:

```
SELECT pg_drop_replication_slot('pg19_demo_slot');
```
## 7. Sequences now replicate

Migrating with logical replication used to leave a trap: table rows got copied, but sequences on the target stayed behind, so the first insert after cutover could fail on a duplicate key. PostgreSQL 19 adds sequence synchronization to logical replication.

Our test:

1. Create `order_number_seq` on the publisher and the subscriber, starting at 1000.
2. Advance the publisher to 1050.
3. Publish all sequences and subscribe; both sides show 1050.
4. Advance the publisher to 2000 and refresh the subscriber.

```
-- Publisher
CREATE PUBLICATION pg19_sequence_pub FOR ALL SEQUENCES;
SELECT setval('pg19_lab.order_number_seq', 2000, true);
```
```
-- Subscriber
ALTER SUBSCRIPTION pg19_sequence_sub REFRESH SEQUENCES;
```
```
-- Both
SELECT current_database() AS database_name, last_value, is_called
FROM pg19_lab.order_number_seq;
```
**Sequences are copied at subscription time and then on demand with `REFRESH SEQUENCES`. They don't follow every increment continuously.** Run a refresh as part of your cutover. See sequence replication.

## 8. Autovacuum: easier to inspect, faster on indexes

### See which tables need attention

The new `pg_stat_autovacuum_scores` view shows how urgently each table needs vacuuming or analyzing.

We deleted 35,000 of 50,000 rows from a table with autovacuum temporarily paused, then checked the scores:

```
SELECT relname AS table_name, score AS priority_score,
       vacuum_score, analyze_score, do_vacuum, do_analyze
FROM pg_stat_autovacuum_scores
WHERE schemaname = 'pg19_lab'
ORDER BY score DESC, relname;
```
Until now, answering "why hasn't autovacuum touched this table?" took threshold math by hand. Now it's one query. The scores change live as statistics and maintenance change.

### Parallel index vacuuming

Autovacuum can now process indexes with parallel workers. This is off by default (`autovacuum_max_parallel_workers = 0`). We set it to 2, deleted 150,000 of 200,000 rows from a table with two indexes, and the server log confirmed it:

```
tuples: 150000 removed, 50000 remain, 0 are dead but not yet removable
parallel workers: index vacuum: 1 planned, 1 launched in total
```
On large tables with many indexes, this is where autovacuum spends most of its time. See vacuum configuration.

## 9. WAIT: read-your-writes on a standby

A classic replica bug: the application writes to the primary, immediately reads from an async standby, and gets stale data. PostgreSQL 19's `WAIT FOR LSN` makes the reader wait until the standby has replayed up to a given WAL position.

On the primary, after the write commits, capture the position:

```
SELECT pg_is_in_recovery() AS is_standby,
       CASE WHEN pg_is_in_recovery() THEN NULL
            ELSE pg_current_wal_insert_lsn()
       END AS target_lsn;
```
On the standby, wait for it:

```
WAIT FOR LSN '0/24002738'
WITH (MODE 'standby_replay', TIMEOUT '2s', NO_THROW);
```
With `NO_THROW`, check the returned status: `success` means it's safe to read, and `timeout` means the standby is still behind. Run `WAIT` as its own top-level command, before starting the read's snapshot. See the WAIT reference.

## 10. Two SQL additions for everyday queries

### IGNORE NULLS in window functions

Sensor data with gaps? `lag()` can now skip nulls and return the last known value:

```
WITH readings(sample_no, reading) AS (
    VALUES (1, 100), (2, NULL), (3, 120), (4, NULL), (5, 150)
)
SELECT sample_no, reading,
       lag(reading) OVER (ORDER BY sample_no) AS previous_row,
       lag(reading) IGNORE NULLS OVER (ORDER BY sample_no) AS previous_known
FROM readings
ORDER BY sample_no;
```
| Sample | Reading | Previous row | Previous known | 
|---|---|---|---|
| 1 | 100 | NULL | NULL | 
| 2 | NULL | 100 | 100 | 
| 3 | 120 | NULL | **100** | 
| 4 | NULL | 120 | 120 | 
| 5 | 150 | NULL | **120** | 

See window functions.

### ON CONFLICT DO SELECT

Get-or-create in one statement, without the old `DO UPDATE SET email = EXCLUDED.email` workaround:

```
CREATE TABLE pg19_lab.demo_contacts (
    id integer PRIMARY KEY,
    email text NOT NULL UNIQUE,
    display_name text NOT NULL
);
 
INSERT INTO pg19_lab.demo_contacts VALUES (1, 'ana@example.com', 'Ana');
 
INSERT INTO pg19_lab.demo_contacts (id, email, display_name)
VALUES (2, 'ana@example.com', 'Another contact')
ON CONFLICT (email) DO SELECT
RETURNING id, email, display_name;
```
It returns Ana's existing row (`1, ana@example.com, Ana`) with no write and no dead tuple. See INSERT.

## More visibility into the server

| New view | What it shows | 
|---|---|
| `pg_stat_autovacuum_scores` | Per-table vacuum/analyze priority | 
| `pg_stat_lock` | Lock statistics aggregated by lock type | 
| `pg_stat_recovery` | Recovery status on a standby | 
| `pg_dsm_registry_allocations` | Registered dynamic shared-memory allocations | 
| `pg_stat_progress_repack` | Progress of a running `REPACK` | 

See the statistics documentation.

## Defaults and compatibility changes to check before upgrading

We ran the same settings query on both versions side by side:

| Setting | PostgreSQL 18.7 | PostgreSQL 19 | 
|---|---|---|
| `default_toast_compression` | pglz | **lz4** | 
| `jit` | on | **off** | 
| `max_locks_per_transaction` | 64 | **128** | 
| `password_expiration_warning_threshold` | — | 7 days | 
| `escape_string_warning` | on | **removed** | 

From the migration notes:

- **`standard_conforming_strings` is always on.** Dumps taken with older tools with it set to`off` won't restore correctly. Re-dump with PostgreSQL 19's tools.
- **JIT is off by default.** If your analytical queries relied on it, turn it back on explicitly.
- **RADIUS authentication is removed.**
- **`io_workers` is replaced** by`io_min_workers` and`io_max_workers` .
- **Database, role and tablespace names** can no longer contain carriage returns or line feeds.
- **`max_locks_per_transaction` is 128,** but lock allocation also changed, so this isn't simply double the old capacity.

Read the full notes for index, locale and extension items that may affect your schema.

### What didn't make it

If you read PostgreSQL 19 coverage from the beta period, some features you saw won't be in the release. The Beta 4 announcement reverted, among others:

- SQL/PGQ property graph queries (`GRAPH_TABLE` )
- Online enabling and disabling of data checksums
- Temporal `UPDATE` /`DELETE` with`FOR PORTION OF`
- `MERGE PARTITIONS` /`SPLIT PARTITION`

## What to test first

Start with what hurts today:

- **Bloated tables?** Measure size, run`REPACK (CONCURRENTLY)` on a copy, and measure again.
- **Critical queries?** Compare their plans on 18 and 19, side by side.
- **Reading from replicas?** Try`WAIT FOR LSN` on the paths where stale reads cause bugs.
- **Upgrading?** Diff your settings and check the JIT and`standard_conforming_strings` changes first.

PostgreSQL 19 also includes features we didn't test here: faster foreign-key checks, earlier aggregation in some join plans, JSON output from `COPY`, new SQL/JSON path string methods, and server-side TLS SNI. The release notes cover them all.

## Try PostgreSQL 19 in VisuaLeaf

Every result in this article came from the same workflow: two servers, two SQL Editors side by side, one query. That's how we saw a 70 MB table drop to 14 MB, a hashed subplan turn into a Hash Anti Join, and a standby report `success` the moment it caught up. You can't see a difference that clearly by switching between terminal windows.

You can run the same lab in a few minutes:

1. **Download VisuaLeaf** .
2. Start PostgreSQL 18 and 19 with the two Docker commands at the top of this article.
3. Add both connections, open a SQL Editor for each, and paste in the queries from this article.

Begin with the settings comparison and the `NOT IN` plan. Those two take under a minute, and they show you what changes when you upgrade.

## Read more about PostgreSQL

Want to go deeper? These guides from the VisuaLeaf blog are a good next step:

- **PostgreSQL JSONB: Query, Update, and Index JSON Data** : query nested JSON, update single fields in place, and speed up searches with GIN indexes.
- **PostgreSQL JSONB vs MongoDB BSON: The Real Architectural Tradeoffs** : how the two formats differ on disk, and what that means for updates, indexes and reads.
- **MongoDB vs PostgreSQL: The Arguments Everyone Gets Wrong** : the myths about schemas, durability and benchmarks that keep coming up in this debate.
- **How to Copy and Sync a MongoDB Collection to PostgreSQL** : move a collection into PostgreSQL, map nested fields to JSONB, and keep the two in sync.
