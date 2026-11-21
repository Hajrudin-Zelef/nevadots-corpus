---
id: collect-261121-veille-ia/langages-dev/what-s-new-in-postgresql-19-repack-concurrently-sequence-replication-wait-on-conflict-do-select-and-
title: "What's New in PostgreSQL 19: REPACK CONCURRENTLY, Sequence Replication, WAIT, ON CONFLICT DO SELECT and Behavior Changes — Mert Özen"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["repack", "concurrently", "vacuum", "postgresql", "table", "full", "post", "orders"]
source: https://www.mertozen.com/en/blog/postgresql-19-yenilikleri-repack-sequence-replication-wait
source_anchor: ""
source_lines: [1, 195]
section: "Langages_Dev"
---
# What's New in PostgreSQL 19: REPACK CONCURRENTLY, Sequence Replication, WAIT, ON CONFLICT DO SELECT and Behavior Changes

PostgreSQL 19's REPACK (CONCURRENTLY) shrinks a table like VACUUM FULL but does not stop writes: under a pgbench load VACUUM FULL dropped to 4 transactions per second while REPACK kept the traffic going. On 19 RC 2: ON CONFLICT DO SELECT, read-your-writes with WAIT FOR LSN on a standby, sequence replication, pg_plan_advice, the changed defaults, and what to expect from Npgsql/EF Core 11.

Version note: The examples in this post were run with PostgreSQL 19's second release candidate, the official `postgres:19rc2` Docker image (12 November 2026), .NET 10 and Npgsql 10.0.3; comparisons with 18 are on 18.7. On the day this post was written (14 November 2026) no GA date had been announced for PostgreSQL 19; what follows is based on the official draft release notes ("as of 2026-10-09") and on real runs against RC 2. Note: PostgreSQL 19 is not a stable release yet; once it is, this post will be checked against the official release notes and updated.

## Guess First

Bookstore, the online bookstore used as the shared example across this site's series, updates the `orders` table all day long. In post 08 we saw that the only core way to shrink a bloated table was `VACUUM FULL`, and that it locks the table. On PostgreSQL 19 RC 2, under a pgbench load where two connections run thousands of `UPDATE`s per second, we ran `VACUUM FULL orders` first and then 19's `REPACK (CONCURRENTLY) orders`:

```
\set id random(1, 1000000)
UPDATE orders SET status = status WHERE order_id = :id;
```
`pgbench -U bookstore_owner -n -c 2 -T 8 -P 1 -f /lab/17-writer.pgbench bookstore`
Both commands rewrite the table from scratch and take almost the same time. How is the write traffic affected in each case?

```
/* VACUUM FULL orders: 1485 ms */
progress: 3.0 s, 5303.6 tps, lat 0.187 ms stddev 0.088, 0 failed
progress: 4.0 s, 4.0 tps, lat 751.253 ms stddev 749.326, 0 failed
progress: 5.0 s, 10663.9 tps, lat 0.187 ms stddev 0.127, 0 failed
/* REPACK (CONCURRENTLY) orders: 1441 ms */
progress: 3.0 s, 10681.5 tps, lat 0.187 ms stddev 0.273, 0 failed
progress: 4.0 s, 9550.7 tps, lat 0.209 ms stddev 0.238, 0 failed
progress: 5.0 s, 13886.2 tps, lat 0.144 ms stddev 0.103, 0 failed
```
During `VACUUM FULL` writes stopped: only 4 transactions completed in that second and latency rose to 751 ms. During `REPACK (CONCURRENTLY)` the traffic slowed a little but did not stop. That is the first topic of the last post in this series. After it come `ON CONFLICT DO SELECT`, the `WAIT` command on a standby, sequence replication, the autovacuum and planner additions, the behavior changes, and what to expect from Npgsql/EF Core 11.

## REPACK and REPACK (CONCURRENTLY)

19 brings the jobs of `VACUUM FULL` and `CLUSTER` together in a single command: `REPACK` writes the table to a new file without dead tuples, and with `USING INDEX` it orders the rows by that index. The two old commands remain for compatibility. We repeated the experiment from post 08:

`docker exec -it bookstore-pg19 psql -U bookstore_owner -d bookstore -f /lab/17-repack.sql````
 fresh                      2040 kB
 after_update_and_vacuum    4080 kB
 after_repack               2040 kB
 after_repack_concurrently  2048 kB
```
After updating every row and running `VACUUM`, the table stayed at 4080 kB; `REPACK` brought it down to 2040 kB. Like `VACUUM FULL` in post 08, `REPACK` does not reset the `n_dead_tup` counter (it stayed at 80,000); monitoring that looks at this counter may think the table is still bloated.

The `CONCURRENTLY` option does most of the work under a `SHARE UPDATE EXCLUSIVE` lock, which blocks neither reads nor writes. It captures changes that arrive during the copy with logical decoding and, at the very end, takes a short `ACCESS EXCLUSIVE` lock to swap the files. That is why the 4 tps second from the guess did not happen. The documentation is clear about what you need to know before using it: the table needs a primary key or an index-based replica identity, the operation uses a replication slot (`max_repack_replication_slots`, default 5), it does not run inside a transaction block, and it is not MVCC-safe: a long transaction that took its snapshot before `REPACK` started can see the table as empty after the work is done. This is the core counterpart of the third-party `pg_repack` extension we recommended in post 08; its progress shows up in the `pg_stat_progress_repack` view.

## ON CONFLICT DO SELECT: Get-or-Create

The cart service wants to return the customer's cart if there is one and create it otherwise. Up to 18 there was no clean way to do this; `DO NOTHING` does not return the existing row:

```
INSERT INTO carts (customer_id) VALUES (42)
ON CONFLICT (customer_id) DO SELECT
RETURNING cart_id, customer_id, old.cart_id IS NULL AS created;
```
```
 cart_id | customer_id | created
---------+-------------+---------
       1 |          42 | t
/* same customer, ON CONFLICT DO NOTHING RETURNING cart_id */
 cart_id
---------
(0 rows)
/* same customer, ON CONFLICT DO SELECT FOR UPDATE */
 cart_id | customer_id | created
---------+-------------+---------
       1 |          42 | f
```
The first call created the cart, the third returned the existing one and locked it with `FOR UPDATE`. The `RETURNING OLD/NEW` from post 16 helps here too: `old.cart_id IS NULL` tells you whether the row is new. The alternatives in 18 were either a second `SELECT` after `DO NOTHING` (two queries with a race in between) or the `DO UPDATE SET customer_id = excluded.customer_id` trick; the latter really updates the row on every call, producing a dead tuple and WAL and firing triggers. One detail: the command tag says `INSERT 0 1` for the existing row as well, so code that looks at the affected row count may think a new row was inserted; tell them apart through `RETURNING`.

## WAIT FOR LSN: Read-Your-Writes on a Standby

In an application that sends reads to a replica, a user changes a price and sees the old price when the page reloads: the asynchronous replica has not applied the change yet. The read/write splitting post of the Veritabanı Performansı ve Scaling series covers this problem and the application-side solutions. 19 brings the `WAIT` command, which tells a standby to wait until it has applied everything up to a given LSN. `db/17-standby.ps1` sets up a primary and a standby whose replay is delayed by 3 seconds (`recovery_min_apply_delay`). The .NET side:

```
newPrice = (decimal)(await update.ExecuteScalarAsync())!;
// the UPDATE has committed (autocommit), so the current WAL position is at or after its commit record
await using NpgsqlCommand lsn = new("SELECT pg_current_wal_lsn()", connection);
commitLsn = (NpgsqlLogSequenceNumber)(await lsn.ExecuteScalarAsync())!;
...
// WAIT is a utility command and takes no parameters; the LSN comes from the server, not from user input
await using (NpgsqlCommand wait = new(
    $"WAIT FOR LSN '{commitLsn}' WITH (TIMEOUT '10s', NO_THROW)", replica))
```
`dotnet run --project src/Bookstore.PgLab -- read-your-writes````
primary: price = 40.00, commit LSN 0/5001F208
standby, no wait: price = 39.00
standby, WAIT FOR LSN: success after 2980 ms
standby, after wait: price = 40.00
```
The price read without waiting is the old one; `WAIT` waited 2.98 seconds and returned `success`, and the next read saw the new price. Without `NO_THROW` a timeout is an error, with `NO_THROW` it returns the status `timeout`; if you run the command on the primary you get `recovery is not in progress`:

```
 status
---------
 timeout
ERROR:  timed out while waiting for target LSN 0/500211C0 to be replayed; current standby_replay LSN 0/50021198
```
In practice you carry the LSN to the client with the response to the write (cookie, header or session) and wait for it on the replica with `WAIT` before the next read; reading from the primary on timeout is a reasonable fallback. The documentation has an important warning: run `WAIT` at the start of the transaction, before any statement that takes locks; otherwise it is refused, because a cycle could form that keeps replay waiting. This experiment used Npgsql 10.0.3, sending the command as plain SQL.

## Logical Replication: Sequences and Enabling Without a Restart

One of the classic problems when moving to another server with logical replication was sequences: the tables are copied, but sequence values are not, and after the switch the first `INSERT` fails with a duplicate key. In 19 sequences can be added to a publication. We promoted the standby from the previous experiment into a separate server and subscribed it to the primary's sequences:

```
CREATE PUBLICATION bookstore_sequences FOR ALL SEQUENCES;                 /* primary */
CREATE SUBSCRIPTION bookstore_sequences CONNECTION 'host=bookstore-pg19 dbname=bookstore user=postgres password=<password>'
    PUBLICATION bookstore_sequences;                                       /* new server */
ALTER SUBSCRIPTION bookstore_sequences REFRESH SEQUENCES;                  /* later synchronizations */
```
```
primary   orders_order_id_seq.last_value  1000005
subscriber, before CREATE SUBSCRIPTION     1000000
subscriber, after CREATE SUBSCRIPTION      1000005
primary, three more orders                 1000008
subscriber, before REFRESH SEQUENCES       1000005
subscriber, after REFRESH SEQUENCES        1000008
```
Sequences do not stream continuously; they are synchronized when the subscription is created and with `REFRESH SEQUENCES`. In a migration plan this command should be the step right before you point the application at the new server. The second addition shows up in the same experiment: with `wal_level` at `replica`, `effective_wal_level` was `replica` at first and became `logical` without a restart once the subscription created a logical slot on the primary. Up to 18, enabling logical replication meant `wal_level = logical` and a restart. The details of upgrade and migration scenarios are in the Veritabanı Performansı ve Scaling series.

## Autovacuum and the Planner: Parallel Autovacuum, Scoring and pg_plan_advice

Autovacuum can now clean a table's indexes with parallel workers; but `autovacuum_max_parallel_workers` defaults to 0, so turning it on is a deliberate decision. A new scoring system decides which table is processed first, and the `pg_stat_autovacuum_scores` view shows each table's score (`xid_score`, `vacuum_score`, `analyze_score`…); the answer to post 08's "why did autovacuum not get to this table" lives here.

The `pg_plan_advice` contrib module shows the planner's decisions in a readable "advice" language and, if you want, pins them. Loading the module with `LOAD` needs a superuser (as the owner role we got `access to library "pg_plan_advice" is not allowed`):

```
LOAD 'pg_plan_advice';
EXPLAIN (COSTS OFF, PLAN_ADVICE)
SELECT b.title, count(*) FROM order_items oi JOIN books b ON b.book_id = oi.book_id
WHERE oi.order_id BETWEEN 1 AND 100 GROUP BY b.title;
```
```
         ->  Nested Loop
               ->  Index Scan using ix_order_items_order_id on order_items oi
               ->  Index Scan using books_pkey on books b
 Generated Plan Advice:
   JOIN_ORDER(oi b)
   NESTED_LOOP_PLAIN(b)
   INDEX_SCAN(oi public.ix_order_items_order_id b public.books_pkey)
   NO_GATHER(oi b)
/* SET pg_plan_advice.advice = 'HASH_JOIN(b)'; */
         ->  Hash Join
               Hash Cond: (oi.book_id = b.book_id)
 Supplied Plan Advice:
   HASH_JOIN(b) /* matched */
```
If the plan of a query like the one in post 07 goes bad as the data changes, you can take the advice of the good plan and pin it; the `pg_stash_advice` extension that comes with 19 applies it automatically by query ID. The documentation warns that you take away the planner's ability to adapt when the data distribution changes; this is a last resort and does not replace fixing statistics and indexes.

## Behavior Changes

The defaults that will affect you on upgrade, with the same query on 18.7 and 19 RC 2:

| Setting / behavior | 18.7 | 19 RC 2 | 
|---|---|---|
| `jit` | on | off | 
| `default_toast_compression` | pglz | lz4 | 
| `log_lock_waits` | off | on | 
| `max_locks_per_transaction` | 64 | 128 | 
| `io_workers` | 3 | removed; `io_min_workers` 2,`io_max_workers` 8 | 
| `json_array(SELECT ... WHERE false)` | NULL | `[]` | 
| `effective_wal_level` | does not exist | replica (logical when needed) | 

JIT was turned off because the cost estimate that decides when it kicks in was found to be unreliable; systems running large analytical queries should turn it on themselves. `max_locks_per_transaction` at 128 does not mean more capacity: the way the lock table is sized changed, and keeping the old setting requires doubling the value. `log_lock_waits`, which we said in post 09 is off in 18, is now on. Code that checks the result of `json_array()` with `IS NULL` behaves differently in 19. JSON output for the `COPY` from post 11 has arrived as well: `COPY ... TO STDOUT (FORMAT json)` writes each row as a JSON object, and with `FORCE_ARRAY` as a single array.

Removed: RADIUS support, which only worked over UDP and could not be made secure; `standard_conforming_strings = off` is no longer possible, and old dumps taken that way do not load correctly into 19, so take the dump with 19's `pg_dump`. A warning is now issued after a successful login with an md5 password (post 14). Full-text search and `pg_trgm` started using the database's locale provider for character classification, and several stemmers, Turkish among them, were updated; we saw no difference for the words from post 10 (kasaba, öğretmenler, aşkı…), but the notes recommend rebuilding those indexes and recomputing stored `tsvector`s after an upgrade.

## What to Expect from Npgsql and EF Core 11

On the day this post was written the latest Npgsql on NuGet was 10.0.3; Npgsql 11 is in development and not on NuGet yet. The draft release notes say the multiplexing mode will be removed and that deprecating the synchronous APIs (`Open`, `ExecuteNonQuery`) so that they throw by default is under consideration; every example in this series is already async. Npgsql.EntityFrameworkCore.PostgreSQL 11 is at 11.0.0-rc.3. According to its notes, what is coming: the default target moves from PostgreSQL 14 to 16; the temporal keys from posts 04 and 16 can be modeled with `WithoutOverlaps()` and `WithPeriod()`; 40001 and 40P01 errors during `SaveChanges` (post 09) will surface as `DbUpdateConcurrencyException`; and JSON members mapped with `ToJson()` can be indexed (like the `Details.Pages` exercise in post 12). The rc.3 notes contain no item specific to PostgreSQL 19. This is release-candidate information and may change at GA. Commands in this post such as `WAIT` and `REPACK` are plain SQL, so they work with Npgsql 10 too.

## Production Notes

Do not move production to 19 before GA; an RC is for trying your application's tests and monitoring queries in a test environment. When GA ships, prepare a probe file the way we did in post 15 and verify the table above on your own system.

Before running `REPACK (CONCURRENTLY)` on a large table for the first time, work out the disk space: the table and all its indexes are written a second time, and the changes are kept on disk separately. Make sure long transactions have finished.

If you have reporting queries that rely on JIT, measure their times after the upgrade; if needed, turn on `jit = on` only for that role or database.

## Common Pitfalls

Assuming features you read about in beta news are in 19: items discussed during beta, such as GROUP BY ALL or SQL/PGQ, are not in the RC notes. Use only the official release notes as your source.

Thinking `REPACK (CONCURRENTLY)` is safe for old snapshots: a transaction that is open at the time and uses an old snapshot can see the table as empty.

Concluding from the affected row count that `ON CONFLICT DO SELECT` inserted a row: the command tag is `INSERT 0 1` in both cases.

Calling `WAIT` in the middle of a transaction that has taken locks: it is refused. Call it before reading, as the first statement of the transaction.

## A Small Experiment

★☆☆ Export Bookstore's first ten books with `COPY (SELECT ...) TO STDOUT (FORMAT json)` and look at how `null` values appear in a table with a `jsonb` column.

★★☆ Set up the standby with `recovery_min_apply_delay = '3s'`, run the `read-your-writes` command with `TIMEOUT '1s'`, and add a fallback that reads from the primary on timeout.

★★★ While `REPACK (CONCURRENTLY) orders` runs, in another session run `SELECT count(*) FROM orders` inside a `REPEATABLE READ` transaction that started before `REPACK`. What does the count become in the same transaction after `REPACK` finishes? See the documentation's MVCC warning for yourself.

The next posts return to everyday PostgreSQL work: we start with string, regex and formatting functions and continue with functions, procedures, triggers and psql. Scaling topics such as replication, partitioning and connection pooling continue in the Veritabanı Performansı ve Scaling series.
