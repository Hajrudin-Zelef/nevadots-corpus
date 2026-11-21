---
id: collect-261121-veille-ia/actu-business/e-1-release-19
title: "E.1. Release 19"
domain: actu-business
role: reference
task: article
actors: []
dates: ["2026-11-19"]
keywords: ["postgresql", "server", "release", "warning", "default", "using", "variable", "password"]
source: https://www.postgresql.org/docs/19/release-19.html
source_anchor: ""
source_lines: [1, 1287]
section: "Actu_Business"
---
**Release date:** 2026-11-19

PostgreSQL 19 contains many new features and enhancements, including:

New REPACK command that reclaims disk space and reorganizes table contents, which can improve query performance. `REPACK (CONCURRENTLY)` allows repacking without blocking reads and writes to the table.

pg_plan_advice extension for controlling query planner decisions and pg_stash_advice extension that applies plan advice automatically based on the query.

Better performance in many areas, including automatic scaling of the number of I/O worker processes, quicker foreign-key checks, and further planning and execution optimizations.

Logical replication can now be enabled without a server restart when wal_level is set to `replica`.

Logical replication of sequence values.

Support for “read-your-writes” consistency on standbys via the new WAIT command.

Parallel autovacuum of table indexes and autovacuum prioritization through a new scoring system.

The above items and other new features of PostgreSQL 19 are explained in more detail in the sections below.

A dump/restore using pg_dumpall or use of pg_upgrade or logical replication is required for those wishing to migrate data from any previous release. See Section 18.6 for general information on migrating to new major releases.

Version 19 contains a number of changes that may affect compatibility with previous releases. Observe the following incompatibilities:

Add server variable `password_expiration_warning_threshold` to warn about password expiration (Gilles Darold, Nathan Bossart) §

The default warning period is seven days.

Issue a warning after successful MD5 password authentication (Nathan Bossart) §

The warning can be disabled via server variable `md5_password_warnings`. MD5 passwords were marked as deprecated in PostgreSQL 18.

Remove RADIUS support (Thomas Munro) §

PostgreSQL only supported RADIUS over UDP, which is unfixably insecure.

Force `standard_conforming_strings` to always be `on` in the database server (Tom Lane) §

Dumps created using pre-PostgreSQL 19 versions of pg_dump or pg_dumpall, and using `standard_conforming_strings = off`, will not properly load into PostgreSQL 19 and later servers. Users should create dumps using PostgreSQL 19 or later versions of these applications, or use `standard_conforming_strings = on`.

Client applications still support operations with servers having `standard_conforming_strings = off`, for compatibility with old servers. The server variable `escape_string_warning` has been removed as unnecessary.

Disallow carriage returns and line feeds in database, role, and tablespace names (Mahendra Singh Thalor) §

pg_upgrade will also disallow upgrading of clusters that use such names. This was changed to avoid security problems.

Change the default index opclasses for `inet` and `cidr` data types from btree_gist to GiST (Tom Lane) § §

The btree_gist `inet`/`cidr` opclasses are broken because they can exclude rows that should be returned. pg_upgrade will disallow upgrading of clusters with btree_gist `inet`/`cidr` indexes.

Disallow system columns from being used in `COPY FROM ... WHERE` (Tom Lane) §

The values of such columns were not well-defined.

Change a `json_array()` call which returns no rows to return an empty JSON array (Richard Guo) §

This previously returned `NULL`.

Cause transactions to pass their `READ ONLY` and `DEFERRABLE` status to postgres_fdw sessions (Etsuro Fujita) §

This means `READ ONLY` transactions can no longer modify rows processed by postgres_fdw sessions.

Change default of `max_locks_per_transaction` from 64 to 128 (Heikki Linnakangas) §

Lock size allocation has changed, so effectively settings must now be doubled to match their capacity in previous releases.

Remove server variable `io_workers` (Thomas Munro) §

This has been replaced by four new variables; see Section E.1.3.1.2.

Change JIT to be disabled by default (Jelte Fennema-Nio) §

Previously JIT was enabled by default, and activated based on optimizer costs, but this costing has been determined to be unreliable. This change requires sites that are doing many large analytical queries to manually enable JIT.

Change parallel vacuum leaders and workers to share the cost-based vacuum delay limit (Bharath Rupireddy) §

Previously each worker process slept based only on the cost it incurred itself, so a parallel vacuum could consume several times the budget set by `vacuum_cost_limit`. With this change, `VACUUM` with parallel workers and cost-based delay enabled might run slower than in previous releases.

Rename column `sync_error_count` to `sync_table_error_count` in system view `pg_stat_subscription_stats` (Vignesh C) §

This is necessary since sequence errors are now tracked separately.

Rename wait event *type* `BUFFERPIN` to `BUFFER` and rename wait event `BufferPin` to `BufferCleanup` (Andres Freund) §

Remove wait event `BufferContent` and add more precise wait events `BufferShared`, `BufferShareExclusive`, and `BufferExclusive` (Andres Freund) §

Change index access method handlers to use a static `IndexAmRoutines` structure, rather than dynamically allocated ones (Matthias van de Meent) §

Remove optimizer hook get_relation_info_hook and add better-placed hook build_simple_rel_hook (Robert Haas) §

Change psql's `\crosstabview` command to honor the `\pset null` setting (Chao Li) §

Change full text search and pg_trgm to use the database's locale provider for character classification when parsing text (Jeff Davis) §

Previously, characters were classified using libc, regardless of the locale provider. In databases with a locale provider other than libc (e.g., `ICU`, `builtin`) and where the locale configured through that locale provider behaves differently from the `LC_CTYPE` setting configured for the database, particularly a LC_CTYPE of `C`, the output of some full text search and pg_trgm functions could change. When upgrading such databases using pg_upgrade, it is recommended to reindex all indexes which use full text search and pg_trgm after the upgrade, and to recompute any stored `tsvector` values.

Change how fuzzystrmatch functions handle non-ASCII characters (Peter Eisentraut, Jeff Davis) § §

`dmetaphone()` and `dmetaphone_alt()` now upper-case their input using the database's default collation rather than always using libc, and `soundex()`, `metaphone()`, and `difference()` now treat only ASCII letters as letters. When upgrading with pg_upgrade, it is recommended to reindex any indexes that use these functions after the upgrade.

Remove `MULE_INTERNAL` encoding (Thomas Munro) §

This encoding was complex and rarely used. Databases using it will need to be dumped and restored with a different encoding.

Below you will find a detailed account of the changes between PostgreSQL 19 and the previous major release.

Allow `NOT IN` clauses to be converted to more efficient `ANTI JOIN`s when NULLs are not present (Richard Guo) §

Allow more `LEFT JOIN`s to be converted to `ANTI JOIN`s (Tender Wang, Richard Guo) §

Allow use of Memoize for `ANTI JOIN`s with unique inner sides (Richard Guo) §

Allow some aggregate processing to be performed before joins (Richard Guo, Antonin Houska) § § §

This can reduce the number of rows needed to be processed.

Improve hash join's handling of tuples with `NULL` join keys (Tom Lane) §

Improve the planning of semijoins (Richard Guo) §

Allow Append and MergeAppend to consider explicit incremental sorts (Richard Guo) §

Convert `IS [NOT] DISTINCT FROM NULL` to `IS [NOT] NULL` during constant folding (Richard Guo) §

The latter form is more easily optimized.

Simplify `IS [NOT] DISTINCT FROM` to equality/inequality operators when inputs are proven non-nullable (Richard Guo) §

Perform earlier constant folding of `var``IS [NOT] NULL` in the optimizer (Richard Guo) §

This allows for later optimizations.

Simplify `COALESCE()` and `ROW(...) IS [NOT] NULL` to avoid evaluating unnecessary arguments (Richard Guo) § §

Simplify `IS [NOT] TRUE/FALSE/UNKNOWN` to plain boolean expressions when the input is proven non-nullable (Richard Guo) §

Speed up join selectivity computations for large optimizer statistics targets (Ilia Evdokimov, David Geier) §

Add proper optimizer statistics for functions returning boolean values (Tom Lane) §

Allow extended statistics on virtual generated columns (Yugo Nagata) §

Allow function `pg_restore_extended_stats()` to restore optimizer extended statistics (Corey Huinker, Michael Paquier, Chao Li) § § § §

Add function `pg_clear_extended_stats()` to remove extended statistics (Corey Huinker, Michael Paquier) §

Adjust the optimizer to consider startup costs of partial paths (Robert Haas, Tomas Vondra) §

Allow negative values of `pg_aggregate`.`aggtransspace` to indicate unbounded memory usage (Richard Guo) §

This information is used by the optimizer in planning memory usage.

Improve performance of foreign key constraint checks (Junwang Zhao, Amit Langote, Chao Li) § §

Improve asynchronous I/O read-ahead scheduling for large requests (Andres Freund) § § §

Allow `io_method` method `worker` to automatically control needed background workers (Thomas Munro) §

The new server variables are `io_min_workers`, `io_max_workers`, `io_worker_idle_timeout`, and `io_worker_launch_interval`.

Allow query table scans to mark pages as all-visible in the visibility map (Melanie Plageman) §

Previously only `VACUUM` and `COPY ... FREEZE` could do this.

Allow autovacuum to use `parallel autovacuum workers` (Daniil Davydov) § §

The maximum number of workers is controlled by server variable `autovacuum_max_parallel_workers` and per-table storage parameter `autovacuum_parallel_workers`.

Allow `TID` Range Scans to be parallelized (Cary Huang, David Rowley) §

Improve `COPY FROM` performance for text and CSV input using SIMD CPU instructions (Nazir Bilal Yavuz, Shinya Kato) §

Improve `NOTIFY` to only wake up backends that are listening to specified notifications (Joel Jacobson) §

Previously most backends were woken by `NOTIFY`.

Change `wal_compression` equals `on` to use the best WAL compression method available (wenhui qiu) §

Previously this always used `pglz`.

Change the default TOAST compression method from `pglz` to the more efficient `lz4` (Euler Taveira) §

This is done by changing the default for server variable `default_toast_compression`.

Improve performance of internal row deformation (David Rowley) §

Improve performance of repeated UTF-8 case-folding operations (Andreas Karlsson) §

Improve performance of hash index bulk-deletion and `GIN` index vacuuming using streaming reads (Xuneng Zhou) § §

Improve sort performance using radix sort (John Naylor) §

Improve timing performance measurements (Lukas Fittl, Andres Freund, David Geier) § §

This benefits `EXPLAIN (ANALYZE, TIMING)` and pg_test_timing, and is controlled via server variable `timing_clock_source`.

Optimize plpgsql syntax `SELECT simple-expression INTO` *`var`* (Tom Lane) §

Add system view `pg_stat_lock` and function `pg_stat_get_lock()` to report per-lock-type statistics (Bertrand Drouvot) §

Add system view `pg_stat_recovery` to report recovery status (Xuneng Zhou, Shinya Kato) § §

Add system view `pg_stat_progress_repack` to report the progress of `REPACK`, `CLUSTER`, and `VACUUM FULL` (Álvaro Herrera) §

Add system view `pg_stat_autovacuum_scores` to report per-table autovacuum details (Sami Imseih) §

Add vacuum initiation details to system view `pg_stat_progress_vacuum` (Shinya Kato) §

The new `started_by` column reports the initiator of the vacuum, and `mode` indicates its aggressiveness.

Add analyze initiation details to system view `pg_stat_progress_analyze` (Shinya Kato) §

The new `started_by` column reports the initiator of the analyze.

Add system view `pg_dsm_registry_allocations` to report dynamic shared memory details (Florents Tselai, Nathan Bossart) § §

Add `mem_exceeded_count` column to system view `pg_stat_replication_slots` (Bertrand Drouvot) §

This reports the number of times that `logical_decoding_work_mem` was exceeded.

Add slot synchronization skip information to `pg_stat_replication_slots` and `pg_replication_slots` (Shlok Kyal) § § §

The new columns are `slotsync_skip_count`, `slotsync_last_skip`, and `slotsync_skip_reason`.

Add `update_deleted` column to system view `pg_stat_subscription_stats` (Zhijie Hou) §

This reports the number of rows where updates were ignored due to concurrent deletes. This requires the subscriber have `retain_dead_tuples` enabled.

Add `sync_seq_error_count` column to system view `pg_stat_subscription_stats` to report sequence synchronization errors (Vignesh C) § §

Add `stats_reset` column to system views `pg_stat_all_tables`, `pg_stat_all_indexes`, and `pg_statio_all_sequences` (Bertrand Drouvot, Sami Imseih, Shihao Zhong) §

It also appears in the `sys` and `user` view variants.

Add `stats_reset` column to system views `pg_stat_user_functions` and `pg_stat_database_conflicts` (Bertrand Drouvot, Shihao Zhong) § §

Add `location` column to system views `pg_available_extensions` and `pg_available_extension_versions` to report the file system directory of extensions (Matheus Alcantara) §

Add `backup_type` column to system view `pg_stat_progress_basebackup` to report the type of backup (Shinya Kato) §

Possible values are `full` or `incremental`.

Add `connecting` value to system view column `pg_stat_wal_receiver`.`status` (Xuneng Zhou) §

Add reporting of the bytes written to WAL for full page images (Shinya Kato) §

This is accessible via system view `pg_stat_wal` and function `pg_stat_get_backend_wal()`.

Add columns to system views `pg_stats`, `pg_stats_ext`, and `pg_stats_ext_exprs` (Corey Huinker) §

Adds table `OID` and attribute number columns to `pg_stats`, and table `OID` and statistics object `OID` columns to the other two.

Add information about range type extended statistics to system view `pg_stats_ext_exprs` (Corey Huinker, Michael Paquier) §

Allow `log_min_messages` log levels to be specified by process type (Euler Taveira) §

The new format is *`type`*:*`level`*. A value without a colon controls all process types, allowing backward compatibility.

Add server variable `log_autoanalyze_min_duration` to log long-running analyze operations by autovacuum operations (Shinya Kato) §

Server variable `log_autovacuum_min_duration` now only controls logging of vacuum operations by autovacuum.

Enable server variable `log_lock_waits` by default (Laurenz Albe) §

Add server variable `debug_print_raw_parse` to log raw parse trees (Chao Li) §

This is also enabled when the server is started with debug level three and higher.

Make messages coming from remote servers appear in the server logs in the same format as local server messages (Vignesh C) §

These include replication, postgres_fdw, and dblink servers.

Add reporting of WAL full-page write bytes to `VACUUM` and `ANALYZE` logging (Shinya Kato) §

Add IO wait events for `COPY FROM/TO` on a pipe, file, or program (Nikolay Samokhvalov) §

Add wait events for commit_delay and for WAL write and flush LSNs (Xuneng Zhou) § §

Have `pg_get_sequence_data()` return the sequence page LSN (Vignesh C) §

Add function `pg_get_multixact_stats()` to report multixact activity (Naga Appani) §

Issue warnings when the wraparound of transaction IDs or multixacts is less than 100 million away (Nathan Bossart) §

The previous warning was 40 million. Warnings are issued to clients and in the server log.

Add scoring system to control the order that tables are processed by autovacuum (Nathan Bossart) §

The new server variables are `autovacuum_freeze_score_weight`, `autovacuum_multixact_freeze_score_weight`, `autovacuum_vacuum_score_weight`, `autovacuum_vacuum_insert_score_weight`, and `autovacuum_analyze_score_weight`.

Add server-side support for SNI (Server Name Indication) (Daniel Gustafsson, Jacob Champion) §

New configuration file `PGDATA/pg_hosts.conf` specifies hostname/key pairs.

Allow OAuth validators to register custom `pg_hba.conf` authentication options (Jacob Champion, Zsolt Parragi) §

Allow roles `pg_read_all_data` and `pg_write_all_data` to read/write large objects (Nitin Motiani, Nathan Bossart) §

These roles are designed to allow non-super users to run pg_dump.

Allow background workers to be configured to terminate before database-level operations (Aya Iwata) §

This allows database-level operations to complete more quickly since blocking background workers can now be terminated.

Allow server variables that represent lists to be emptied by setting the value to `NULL` (Tom Lane) §

Update GB18030 encoding from version 2000 to 2022 (Chao Li, Zheng Tao) §

See the commit message for compatibility details.

Add `WAIT` command to allow standbys to wait for LSN values to be written, flushed, or replayed (Kartyshov Ivan, Alexander Korotkov, Xuneng Zhou) § §

Improve function `pg_sync_replication_slots()` to wait for replication synchronization completion (Ajin Cherian, Zhijie Hou) §

Previously, certain synchronization failures would not be reported.

Add server variable `wal_sender_shutdown_timeout` to limit replica synchronization waits during shutdown (Andrey Silitskiy, Hayato Kuroda) §

By default, senders still wait forever for synchronization.

Allow sequence values stored in subscribers to match the publisher (Vignesh C) § § §

This occurs during `ALTER SUBSCRIPTION ... [ADD|SET|REFRESH|DROP] PUBLICATION`. Function `pg_get_sequence_data()` allows inspection of sequence synchronization.

Allow `CREATE`/`ALTER PUBLICATION` to publish all sequences (Vignesh C, Tomas Vondra) §

This is enabled with the `ALL SEQUENCES` clause.

Allow `ALTER SUBSCRIPTION` on publications to synchronize the existence of sequences on subscribers to match the publisher (Vignesh C) §

This is enabled with the `REFRESH SEQUENCES` clause.

Allow `CREATE`/`ALTER PUBLICATION` to exclude some tables (Vignesh C, Shlok Kyal) § § § §

This is controlled with the `EXCEPT` clause, and is useful when specifying `ALL TABLES`.

Add `CREATE`/`ALTER SUBSCRIPTION` setting `retain_dead_tuples` to retain information needed for conflict detection (Zhijie Hou) § § §

Also add setting `max_retention_duration` to limit `retain_dead_tuples` retention.

Allow `CREATE SUBSCRIPTION` to use postgres_fdw foreign data wrapper connection parameters (Jeff Davis) §

The connection parameters are referenced via `CREATE SUBSCRIPTION ... SERVER`.

When server variable `wal_level` is `replica`, allow automatic enablement of logical replication when needed (Masahiko Sawada) §

New server variable `effective_wal_level`, application pg_controldata, and function `pg_control_checkpoint()` report the effective WAL level.

Allow `wal_receiver_timeout` to be set per-subscription and user (Fujii Masao) § §

This allows subscribers to use different `wal_receiver_timeout` values.

Add optional pid parameter to `pg_replication_origin_session_setup()` to allow parallelization of SQL-level replication solutions (Doruk Yilmaz, Hayato Kuroda) §

Allow `GROUP BY` to process target list subqueries that have expressions referencing non-subquery columns (Tom Lane) §

Also fix a bug in how `GROUPING()` handles target list subquery aliases.

Allow window functions to ignore NULLs with the `IGNORE NULLS`/`RESPECT NULLS` clause (Oliver Ford, Tatsuo Ishii) §

Supported window functions are `lead()`, `lag()`, `first_value()`, `last_value()`, and `nth_value()`.

Add support for `INSERT ... ON CONFLICT DO SELECT ... RETURNING` (Andreas Karlsson, Marko Tiikkaja, Viktor Holmberg) §

This allows conflicting rows to be returned, and optionally locked with `FOR UPDATE`/`SHARE`.

Add `REPACK` command which replaces `VACUUM FULL` and `CLUSTER` (Antonin Houska) §

The two former commands did similar things, but with confusing names, so unify them as `REPACK`. The old commands have been retained for compatibility.

Allow `REPACK` to rebuild tables without access-exclusive locking (Antonin Houska, Mihail Nikalayeu, Álvaro Herrera) § § §

This is enabled via the `CONCURRENTLY` option. Server variable `max_repack_replication_slots` was also added.

Allow `GRANT`/`REVOKE` to specify the effective role performing the privileges adjustment (Nathan Bossart, Tom Lane) §

The `GRANTED BY` clause controls this.

Support forward references in `FOREIGN KEY` constraint clauses within `CREATE SCHEMA` (Jian He, Tom Lane) §

Allow `CHECKPOINT` to accept a list of options (Christoph Berg) § § §

Supported options are `MODE` and `FLUSH_UNLOGGED`.

Add `CONNECTION` clause to `CREATE FOREIGN DATA WRAPPER` to specify a function to be called for subscription connection parameters (Jeff Davis, Noriyoshi Shinoda) § §

Add `SUPPORT` clause to `CREATE AGGREGATE` so extensions can specify support functions to optimize aggregate calls (Tom Lane) §

Add memory usage and parallelism reporting to `VACUUM (VERBOSE)` and autovacuum logs (Tatsuya Kawata, Daniil Davydov) § §

Allow `ALTER TABLE ALTER CONSTRAINT ... [NOT] ENFORCED` for `CHECK` constraints (Jian He) §

Previously enforcement changes were only supported for foreign key constraints.

Allow `ALTER TABLE ... COLUMN SET EXPRESSION` to succeed on virtual columns with `CHECK` constraints (Jian He) §

This was previously prohibited.

Allow multiple headers lines to be skipped by `COPY FROM` (Shinya Kato, Fujii Masao) §

Previously only a single header line could be skipped.

Allow `COPY FROM` to set invalid input values to `NULL` (Jian He, Kirill Reshke) §

This is done using the `COPY` option `ON_ERROR SET_NULL`.

Allow `COPY TO` to output JSON format (Joe Conway, Jian He, Andrew Dunstan) § §

JSON output can also be a single JSON array using the `COPY` option `FORCE_ARRAY`.

Allow `COPY TO` to process partitioned tables (Jian He, Ajin Cherian) § §

Previously `COPY (SELECT ...)` had to be used to output partitioned tables. This also improves logical replication table synchronization.

Add `EXPLAIN ANALYZE` option `IO` to report asynchronous IO activity (Tomas Vondra) § § §

Add reporting of WAL full-page write bytes to `EXPLAIN (ANALYZE, WAL)` output (Shinya Kato) §

Add Memoize cache and lookup estimates to `EXPLAIN` output (Ilia Evdokimov, Lukas Fittl) §

This can show why Memoize was chosen.

Add the 64-bit unsigned data type `oid8` (Michael Paquier) §

Add more `jsonpath` string methods (Florents Tselai, David E. Wheeler) §

They are `ltrim()`, `rtrim()`, `btrim()`, `lower()`, `upper()`, `initcap()`, `replace()`, and `split_part()`. These are immutable like their non-JSON string variants.

Allow casts between `bytea` and `uuid` data types (Dagfinn Ilmari Mannsåker, Aleksander Alekseev) §

Add ability to cast between database names and `oid`s using `regdatabase` (Ian Lawrence Barwick) §

Add functions `tid_block()` and `tid_offset()` to extract block numbers and offsets from `tid` values (Ayush Tiwari) §

Add `date`, `timestamp`, and `timestamptz` versions of `random(min, max)` (Damien Clochard, Dean Rasheed) § §

Allow `encode()` and `decode()` to process data in `base64url` and `base32hex` formats (Andrey Borodin, Aleksander Alekseev, Florents Tselai) § § §

This format retains ordering, unlike `base32`.

Add functions to return a set of ranges resulting from range subtraction (Paul A. Jungwirth) §

The functions are `range_minus_multi()` and `multirange_minus_multi()`. This is useful to represent range subtraction results with gaps.

Add function `error_on_null()` to return the supplied parameter, or error on `NULL` input (Joel Jacobson) §

Allow `IS JSON` to work on domains defined over supported base types (Jian He) §

The supported base types are `TEXT`, `JSON`, `JSONB`, and `BYTEA`.

Add full text stemmers for Polish and Esperanto (Tom Lane) §

The Dutch stemmer now uses a different algorithm, which changes the stems of many words; the previous algorithm is available via `dutch_porter`.

The English, French, German, Lithuanian, Norwegian, Spanish, Swedish, and Turkish stemmers have also been updated and produce different stems for some words.

Allow event triggers to be written using PL/Python (Euler Taveira, Dimitri Fontaine) §

Add special libpq protocol parameters for compatibility testing (Jelte Fennema-Nio, Jacob Champion) §

Otherwise known as protocol grease, which was active during the 19 beta period. These will no longer be sent as of the PostgreSQL 19 official release, but the parameter reservations will remain for potential use in the future.

Add libpq connection parameter `oauth_ca_file` to specify an alternate certificate authority when communicating with OAuth providers (Jonathan Gonzalez V., Jacob Champion) §

This can also be set via the `PGOAUTHCAFILE` environment variable. The default is to use curl's built-in certificates.

Allow libpq environment variable `PGOAUTHDEBUG` to specify particular debug options (Zsolt Parragi, Jacob Champion) §

The `UNSAFE` option still generates all debugging output.

Improve custom OAuth client flows (Jacob Champion) § §

Previously, libpq reported only a generic error message during custom flow failures, and each flow implementation was required to calculate the issuer identifier independently. Add a new authdata hook type `PQAUTHDATA_OAUTH_BEARER_TOKEN_V2` that precomputes the issuer ID and lets hooks provide error details.

Add libpq function `PQgetThreadLock()` to retrieve the current locking callback (Jacob Champion) §

Allow the search path to appear in the psql prompt via `%S` (Florents Tselai) §

This works when psql is connected to PostgreSQL 18 or later.

Allow the hot standby status to appear in the psql prompt via `%i` (Jim Jones) §

Modify psql backslash commands to show comments for publications, subscriptions, and extended statistics (Fujii Masao, Jim Jones) §

The modified commands are `\dRp+`, `\dRs+`, and `\dX+`.

Allow control over how booleans are displayed in psql (David G. Johnston) §

The \pset variables are `display_true` and `display_false`.

Allow psql to more accurately determine if the pager is needed (Erik Wienhold) §

Add or improve psql tab completion (Yamaguchi Atsuo, Yugo Nagata, Haruna Miwa, Xuneng Zhou, Dagfinn Ilmari Mannsåker, Fujii Masao, Álvaro Herrera, Jian He, Tatsuya Kawata, Ian Lawrence Barwick, Vasuki M) § § § § § § § § § § § § § §

Allow pg_dump to include restorable extended statistics (Corey Huinker) §

Change vacuumdb's `--analyze-only` and `--analyze-in-stages` options to analyze partitioned tables when no targets are specified (Laurenz Albe, Mircea Cadariu, Chao Li) § §

Previously it skipped partitioned tables. This now matches the behavior of `ANALYZE`.

Allow vacuumdb to report its commands without running them using option `--dry-run` (Corey Huinker) §

Allow pg_verifybackup to read WAL files stored in tar archives (Amul Sul) §

Add option `--wal-path` as an alias for the existing and deprecated `--wal-directory` option.

Allow pg_waldump to read WAL files stored in tar archives (Amul Sul) §

Improve performance of pg_upgrade copying large object metadata (Nathan Bossart) § § § §

Various methods are used, depending on the PostgreSQL version of the old cluster.

Allow pg_upgrade to process non-default tablespaces stored in the `PGDATA` directory (Nathan Bossart) §

Previously such tablespaces generated an error.

Add pgbench option `--continue-on-error` to continue after SQL errors (Rintaro Ikeda, Yugo Nagata, Fujii Masao) §

Improve the usability of pg_test_timing (Hannu Krosing, Tom Lane) § §

Report nanoseconds instead of microseconds. In addition to histogram output, output a second table that reports exact timings, with an optional cutoff set by `--cutoff`.

Allow pg_createsubscriber to ignore specified publications that already exist (Shubham Khanna) §

Previously this generated an error.

Change the way pg_createsubscriber stores recovery parameters (Alyona Vinter) §

Changes are stored in optionally-included `pg_createsubscriber.conf` rather than directly in `postgresql.auto.conf`.

Add pg_createsubscriber option `-l`/`--logdir` to redirect output to files (Gyan Sreejith, Hayato Kuroda) §

Restore support for AIX (Aditya Kamath, Srirama Kucherlapati, Peter Eisentraut) § §

This uses gcc and only supports 64-bit builds.

Change Solaris to use unnamed POSIX semaphores (Tom Lane) §

Previously it used System V semaphores.

Require Visual Studio 2019 or later (Peter Eisentraut) §

Allow MSVC to create PL/Python using the Python Limited API (Bryan Green) §

Allow building on AArch64 using MSVC (Niyas Sait, Greg Burd, Dave Cramer) §

Allow execution stack backtraces on Windows using `DbgHelp` (Bryan Green) §

Change the supported C language version to C11 (Peter Eisentraut) § §

Previously C99 was used.

Use standard C23 and C++ attributes if available (Peter Eisentraut) §

Use AVX2 CPU instructions for calculating page checksums (Matthew Sterrett, Andrew Kim) §

Use ARM Crypto Extension to Compute CRC32C (John Naylor) §

Change `hex_encode()` and `hex_decode()` to use SIMD CPU instructions (Nathan Bossart, Chiranmoy Bhattacharya) §

Require Meson version 0.57.2 or later (Peter Eisentraut) §

Add Meson option to build both shared and static libraries, or only shared (Peter Eisentraut) §

Update Unicode data to version 17.0.0 (Peter Eisentraut) §

Add hooks `planner_setup_hook`, `planner_shutdown_hook`, `joinrel_setup_hook`, and `join_path_setup_hook` (Robert Haas) § §

Allow extensions to replace set-returning functions in the `FROM` clause with SQL queries (Paul A. Jungwirth) §

Make multixact members 64-bit (Maxim Orlov) §

Change function prototypes to use `uint*` instead of `bit*` typedefs (Nathan Bossart) §

Allow OAuth validators to supply failure details (Jacob Champion) §

This is done by setting the `ValidatorModuleResult` structure member error_detail.

Add simplified and improved shared memory registration function `ShmemRequestStruct()` (Heikki Linnakangas, Ashutosh Bapat) §

Functions `ShmemInitStruct()` and `ShmemInitHash()` remain for backward compatibility.

Add server variable `debug_exec_backend` to report how parameters are passed to new backends (Daniel Gustafsson) §

Add documentation section about temporal tables (Paul A. Jungwirth) §

Document the environment variables that control the regression tests (Michael Paquier) §

Update documented systemd example to include a restart setting (Andrew Jackson) §

Add pg_plan_advice module to stabilize and control planner decisions (Robert Haas) § §

Add extension pg_stash_advice to allow per-query-id advice to be specified (Robert Haas, Lukas Fittl) § §

Show sizes of `FETCH` queries as constants in pg_stat_statements (Sami Imseih) §

Fetches of different sizes will now be grouped together in pg_stat_statements output.

Add generic and custom plan counts to pg_stat_statements (Sami Imseih) §

Refactor pg_buffercache reporting of shared memory mapping (Bertrand Drouvot) §

New function `pg_buffercache_os_pages()` and system view `pg_buffercache_os_pages` allow reporting of shared memory mapping; the function optionally includes NUMA details. Function `pg_buffercache_numa_pages()` remains for backward compatibility.

Add functions to pg_buffercache to mark buffers as dirty (Nazir Bilal Yavuz) §

The functions are `pg_buffercache_mark_dirty()`, `pg_buffercache_mark_dirty_relation()`, and `pg_buffercache_mark_dirty_all()`.

Allow pushdown of array comparisons in prepared statements to postgres_fdw foreign servers (Alexander Pyhalov) §

Allow the retrieval of statistics from foreign data wrapper servers (Corey Huinker, Etsuro Fujita) § §

This is enabled for postgres_fdw by using the option `import_stats`. The default is for `ANALYZE` to retrieve rows from the remote server to locally generate statistics.

Allow file_fdw to read files or program output that uses multi-line headers (Shinya Kato) §

Add server variable `auto_explain.log_io` to add IO reporting to auto_explain (Tomas Vondra) §

Allow auto_explain to add extension-specific `EXPLAIN` options via server variable `auto_explain.log_extension_options` (Robert Haas) §

Change btree_gin to support all btree-supported cross-type comparisons (Tom Lane) § §

Improve performance of bloom indexes by using streaming reads (Xuneng Zhou) § §

Improve performance of pgstattuple by using streaming reads (Xuneng Zhou) § §

Modify oid2name `--extended` to report the relation file path (David Bidoc) §

The following individuals (in alphabetical order) have contributed to this release as patch authors, committers, reviewers, testers, or reporters of issues.

| Adam Lee | 
| Adam Pickering | 
| Adam Warland | 
| Aditya Gollamudi | 
| Aditya Kamath | 
| Afrah Razzak | 
| Aidar Imamov | 
| Ajay Pal | 
| Ajin Cherian | 
| Akshay Joshi | 
| Alberto Piai | 
| Aleksander Alekseev | 
| Aleksey Solovev | 
| Alex Guo | 
| Alex Shapalov | 
| Alex Young | 
| Alexander Borisov | 
| Alexander Kjäll | 
| Alexander Korotkov | 
| Alexander Kozhemyakin | 
| Alexander Kukushkin | 
| Alexander Kuzmenkov | 
| Alexander Kuznetsov | 
| Alexander Lakhin | 
| Alexander Pyhalov | 
| Alexandra Wang | 
| Alexandre Felipe | 
| Alexey Makhmutov | 
| Alexey Shishkin | 
| Ali Akbar | 
| Altan Birler | 
| Álvaro Herrera | 
| Alyona Vinter | 
| Amit Kapila | 
| Amit Langote | 
| Amjad Shahzad | 
| Amul Sul | 
| Amy Burnett | 
| Andreas Karlsson | 
| Andreas Lind | 
| Andrei Klychkov | 
| Andrei Lepikhov | 
| Andrei Varashen | 
| Andres Freund | 
| Andrew Bille | 
| Andrew Dunstan | 
| Andrew Gierth | 
| Andrew Jackson | 
| Andrew Kim | 
| Andrew Krylosov | 
| Andrew Watkins | 
| Andrey Borodin | 
| Andrey Chernyy | 
| Andrey Rachitskiy | 
| Andrey Rudometov | 
| Andrey Silitskiy | 
| Andrzej Doros | 
| Andy Fan | 
| Anh Khoa | 
| Ante Kresic | 
| Anthonin Bonnefoy | 
| Antoine Violin | 
| Anton Voloshin | 
| Antonin Houska | 
| Ants Aasma | 
| Arne Roland | 
| Arseniy Mukhin | 
| Artem Gavrilov | 
| Arthur Nascimento | 
| Artur Zakirov | 
| Ashutosh Bapat | 
| Ashutosh Sharma | 
| Atsuo Yamaguchi | 
| Atsushi Torikoshi | 
| Aya Iwata | 
| Ayoub Kazar | 
| Ayush Tiwari | 
| Ayush Vatsa | 
| Baji Shaik | 
| Ben Morris | 
| Benoit Lobréau | 
| Bernd Helmle | 
| Bernd Reiß | 
| Bernice Southey | 
| Bertrand Drouvot | 
| Bharath Rupireddy | 
| Bihua Wang | 
| Bill Kim | 
| Bing Tian | 
| Bing’e Jin | 
| Bob Kline | 
| Bowen Shi | 
| Brian Carpenter | 
| Bruce Dang | 
| Bruce Momjian | 
| Bryan Fox | 
| Bryan Green | 
| Bykov Ivan | 
| Cai Mengjuan | 
| Cándido Antonio Martínez Descalzo | 
| Cary Huang | 
| Casey Shobe | 
| Chaitanya Choudhary | 
| ChangAo Chen | 
| Changhong Fei | 
| Chao Li | 
| Charles Samborski | 
| Chee Wooson | 
| Chengpeng Yan | 
| Chengxi Sun | 
| Chi Zhang | 
| Chiranmoy Bhattacharya | 
| Chong Peng | 
| Christian Ullrich | 
| Christoph Berg | 
| Christophe Pettus | 
| Chuanwen Hu | 
| Corey Huinker | 
| DaeMyung Kang | 
| Dagfinn Ilmari Mannsåker | 
| Daisuke Higuchi | 
| Damien Clochard | 
| Dan Stefura | 
| Daniel Bakker | 
| Daniel Bauman | 
| Daniel Farkaš | 
| Daniel Firer | 
| Daniel Gustafsson | 
| Daniel Vérité | 
| Daniel Woelfel | 
| Daniele Varrazzo | 
| Daniil Davydov | 
| Dapeng Wang | 
| Daria Shanina | 
| Dave Cramer | 
| David Bidoc | 
| David Christensen | 
| David Fetter | 
| David G. Johnston | 
| David Geier | 
| David Rowley | 
| David Steele | 
| David Wheeler | 
| Davin Shearer | 
| Dean Rasheed | 
| Dennis Björklund | 
| Devanga Susmitha | 
| Devrim Gündüz | 
| Dewei Dai | 
| Dharin Shah | 
| Dhruv Chauhan | 
| Dian Fay | 
| Diego Frias | 
| Dilip Kumar | 
| Dimitre Radoulov | 
| Dimitri Fontaine | 
| Dimitrios Apostolou | 
| Dipesh Dhameliya | 
| Dmitriy Kuzmin | 
| Dmitry Astapov | 
| Dmitry Dolgov | 
| Dmitry Fomin | 
| Dmitry Igrishin | 
| Dmitry Koval | 
| Dmitry Kovalenko | 
| Dmitry Mityugov | 
| Dmitry Yurichev | 
| Domenico Sgarbossa | 
| Dominique Devienne | 
| Donghang Lin | 
| Doruk Yilmaz | 
| Egor Chindyaskin | 
| Ekaterina Sokolova | 
| Emmanuel Sibi | 
| Emmanuel Touzery | 
| Eric Ridge | 
| Erik Nordström | 
| Erik Rijkers | 
| Erik Wienhold | 
| Ethan Mertz | 
| Etsuro Fujita | 
| Eugene Morgunov | 
| Euler Taveira | 
| Evan Montgomery-Recht | 
| Evan Si | 
| Evgeniy Gorbanev | 
| Evgeny Voropaev | 
| Ewan Young | 
| Fabrice Chapuis | 
| Fabrízio de Royes Mello | 
| Feilong Meng | 
| Feng Wu | 
| Filip Janus | 
| Florents Tselai | 
| Florin Irion | 
| Francesco Verardi | 
| François Jehl | 
| Frédéric Yhuel | 
| Fredrik Widlert | 
| Gabriel Gibson | 
| Gaurav Singh | 
| Gavin Panella | 
| George Tarasov | 
| Gilles Darold | 
| Gilles Parc | 
| Giovanni Fabris | 
| Glauber Batista | 
| Gleb Kashkin | 
| Greg Burd | 
| Greg Sabino Mullane | 
| Gregory Smith | 
| Grem Snoort | 
| Grigorev Jurij | 
| Grigory Smolkin | 
| Guillaume Lelarge | 
| Gunnar Morling | 
| Guoqing Yang | 
| Gurjeet Singh | 
| Gyan Sreejith | 
| Haibo Yan | 
| Haiyang Li | 
| Hang Ammmkilo | 
| Hannu Krosing | 
| Hans Buschmann | 
| Haowu Ge | 
| Haoyan Wang | 
| Haruka Takatsuka | 
| Haruna Miwa | 
| Hayato Kuroda | 
| Heewon Song | 
| Heikki Linnakangas | 
| Henrik TJ | 
| Henson Choi | 
| Hillai Ben Sasson | 
| Holger Hoffstätte | 
| Hubert Lubaczewski | 
| Hugo Dubois | 
| Hüseyin Demir | 
| Ian Barwick | 
| Ignat Remizov | 
| Igor Stepansky | 
| Ilia Evdokimov | 
| Ilia Kashintsev | 
| Ilya Cherdakov | 
| Ilya Portnov | 
| Ilya Staroverov | 
| Ilyasov Ian | 
| Imran Zaheer | 
| Ivan Kush | 
| Izumi Igi | 
| Jack Bonatakis | 
| Jacob Brazeal | 
| Jacob Champion | 
| Jacques Combrink | 
| Jakob Egger | 
| Jakub Wartak | 
| Jan Nidzwetzki | 
| Japin Li | 
| Jasper Smit | 
| Javier Maestro | 
| Jeevan Chalke | 
| Jeff Davis | 
| Jeff Janes | 
| Jehan-Guillaume de Rorthais | 
| Jelte Fennema-Nio | 
| Jérémie Hodnik | 
| Jeroen Gui | 
| Ji Xu | 
| Jian He | 
| Jianghua Yang | 
| Jiao Shuntian | 
| Jie Wang | 
| Jihe Wang | 
| Jim Jones | 
| Jim Nasby | 
| Jingtang Zhang | 
| Jingzhou Fu | 
| Jinhui Lai | 
| Jinyang Liu | 
| João Costa | 
| Jochen Bandhauer | 
| Joe Conway | 
| Joel Jacobson | 
| John Hsu | 
| John Naylor | 
| Jonas Boberg | 
| Jonathan Gonzalez V. | 
| Jonathan S. Katz | 
| Jorge Solórzano | 
| Josef Šimánek | 
| Joseph Koshakow | 
| Joshua Shanks | 
| Julien Rouhaud | 
| Julien Tachoires | 
| Junwang Zhao | 
| Junwen An | 
| Justin Kwan | 
| Justin Pryzby | 
| Kamil Monicz | 
| Karavaev Alexey | 
| Karina Litskevich | 
| Karthik S | 
| Kartyshov Ivan | 
| Ke Lan | 
| Keisuke Kuroda | 
| Keith Paskett | 
| Kenny Chen | 
| Kentaro Sato | 
| Kevin K Biju | 
| Kevin Oommen Anish | 
| Kiran Kaki | 
| Kirill Reshke | 
| Konstantin Knizhnik | 
| Krasiyan Andreev | 
| Kristian Lejao | 
| Kuan-Ting Kuo | 
| Kunpeng Yi | 
| Kuntal Ghosh | 
| Kyotaro Horiguchi | 
| Lakshmi G | 
| Lakshmi N | 
| Lars Kanis | 
| Laurenz Albe | 
| Lauri Siltanen | 
| Lev Nikolaev | 
| Lihui Zhang | 
| Lori Corbani | 
| Lu Feng | 
| Lucas Velgus | 
| Lucio Chiessi | 
| Lukas Eder | 
| Lukas Fittl | 
| Maciek Sakrejda | 
| Madhukar Prasad | 
| Magnus Hagander | 
| Mahendra Singh Thalor | 
| Maksim Logvinenko | 
| Maksim Melnikov | 
| Maksim Milyutin | 
| Man Zeng | 
| Manni Wood | 
| Manuel Reyes Bravo | 
| Marco Nenciarini | 
| Marcos Pegoraro | 
| Marcus Gartner | 
| Marian Muller Rebeyrol | 
| Mario González | 
| Mark Callaghan | 
| Mark Dilger | 
| Mark Frost | 
| Mark Wong | 
| Marko Grujic | 
| Marko Tiikkaja | 
| Martin Heistermann | 
| Martin Rakhmanov | 
| Masahiko Sawada | 
| Masahiro Ikeda | 
| Masao Fujii | 
| Masashi Kamura | 
| Matheus Alcantara | 
| Mats Kindahl | 
| Matt Long | 
| Matthew Sterrett | 
| Matthias van de Meent | 
| Matthieu Denais | 
| Matthijs van der Vleuten | 
| Maxim Boguk | 
| Maxim Orlov | 
| Maxime Schoemans | 
| Mehmet D. Ince | 
| Melanie Plageman | 
| Melih Mutlu | 
| Meng Zhang | 
| Merlin Moncure | 
| Mert Alev | 
| Michael Banck | 
| Michael Malis | 
| Michael Paquier | 
| Michael Schönitzer | 
| Mikael Gustavsson | 
| Mikhail Gribkov | 
| Mikhail Kot | 
| Mikhail Litsarev | 
| Mikhail Nikalayeu | 
| Milosz Bieniek | 
| Mingyang Zhen | 
| Mircea Cadariu | 
| Mohamed Ali | 
| Moritz Sanft | 
| Muhammad Taha Naveed | 
| Mutaamba Maasha | 
| Naga Appani | 
| Natalya Aksman | 
| Nathan Bossart | 
| Nazir Bilal Yavuz | 
| Neil Chen | 
| Neil Conway | 
| Niall Newman | 
| Niccolò Fei | 
| Nick Ivanov | 
| Nicolas Adenis-Lamarre | 
| Nikhil Kumar Veldanda | 
| Nikhil Sontakke | 
| Nikita Kalinin | 
| Nikolay Samokhvalov | 
| Nikolay Shaplov | 
| Ning Sun | 
| Ning Wu | 
| Nisha Moond | 
| Nishant Sharma | 
| Nitin Jadhav | 
| Nitin Motiani | 
| Niyas Sait | 
| Noah Misch | 
| Noboru Saito | 
| Noriyoshi Shinoda | 
| Oleg Ivanov | 
| Oleg Samoylov | 
| Oleg Sibiryakov | 
| Oleg Tkachenko | 
| Oleg Tselebrovskiy | 
| Oli Sennhauser | 
| Oliver Ford | 
| Oreo Yang | 
| Orestis Markou | 
| Osama Abdul Qader | 
| Palak Chaturvedi | 
| Palayret Jacques | 
| Patrick Reynolds | 
| Patrick Stählin | 
| Paul Bunn | 
| Paul Gerste | 
| Paul Jungwirth | 
| Paul Kim | 
| Paul Ramsey | 
| Pavel Borisov | 
| Pavel Kohout | 
| Pavel Kopylov | 
| Pavel Luzanov | 
| Pavel Seleznev | 
| Pavel Stehule | 
| Pavel Tyslacki | 
| Pavlo Golub | 
| Pedro Brantes | 
| Peng Wang | 
| Peter Eisentraut | 
| Peter Geoghegen | 
| Peter Mitteregger | 
| Peter Smith | 
| Pierre Ducroquet | 
| Pierre Forstmann | 
| Pierre Senellart | 
| Pierrick Chovelon | 
| Potapov Alexander | 
| Pradeep Kumar | 
| Przemyslaw Sztoch | 
| Qifan Liu | 
| Qing Zhang | 
| Quan Zongliang | 
| Radim Marek | 
| Rafia Sabih | 
| Rahila Syed | 
| Rambabu V | 
| Ran Benita | 
| Ranier Vilela | 
| Renan Davanzo | 
| Renli Zou | 
| Renzo Dani | 
| Richard Guo | 
| Richard Neill | 
| Rick de Jager | 
| Rintaro Ikeda | 
| Rithvika Devisetti | 
| Robert Haas | 
| Robert Pang | 
| Robert Treat | 
| Rogers Wang | 
| Rohit Prasad | 
| Roman Khapov | 
| Ronan Dunklau | 
| Ruben Ruiz | 
| Rui Zhao | 
| Ruikai Peng | 
| Runyuan He | 
| Ruohang Feng | 
| Rustam Allakov | 
| Rustam Khamidullin | 
| Ryan Bird | 
| Ryo Kanbayashi | 
| Ryo Matsumura | 
| Sadhuprasad Patro | 
| Sahitya Chandra | 
| Sajeeb Lohani | 
| Sami Imseih | 
| Samuel Thibault | 
| Sandeep Thakkar | 
| Sandro Santilli | 
| Sanjaya Waruna | 
| Sarath Kumar | 
| Satya Narlapuram | 
| Satyanarayana Narlapuram | 
| Sehrope Sarkuni | 
| Semih Dogan | 
| Sergei Glukhov | 
| Sergei Kornilov | 
| Sergei Patiakin | 
| Sergey Fukanchik | 
| Sergey Koposov | 
| Sergey Prokhorenko | 
| Sergey Shinderuk | 
| Sergey Soloviev | 
| Sergey Tatarintsev | 
| Sergey Zhuravlev | 
| Shengbin Zhao | 
| Shenhao Wang | 
| Shihao Zhong | 
| Shinya Kato | 
| Shinya Sugamoto | 
| Shishir Sharma | 
| Shixin Wang | 
| Shiyu Qin | 
| Shlok Kyal | 
| Shruthi Gowda | 
| Shubham Khanna | 
| Shveta Malik | 
| Siddharth Kothari | 
| Sirisha Chamarthi | 
| Solaimurugan V | 
| Songjin Zhou | 
| Sophie Alpert | 
| Soumya S Murali | 
| Srinath Reddy Sadipiralla | 
| Srirama Kucherlapati | 
| Stepan Neretin | 
| Stephan Springl | 
| Stéphane Schildknecht | 
| Stephane Tachoires | 
| Stephen Frost | 
| Steve Baldwin | 
| Steve Lau | 
| Steven Niu | 
| Stolpovskikh Danil | 
| Sungwoo Chang | 
| Sunil S | 
| Surya Poondla | 
| Suyang Zhong | 
| Sven Klemm | 
| Swirl Smog Dowry | 
| Sylvie Mayer | 
| Taiki Koshino | 
| Takamichi Osumi | 
| Tan Yang | 
| Tan Zhen | 
| Tatsuhito Kasahara | 
| Tatsuo Ishii | 
| Tatsuro Yamada | 
| Tatsuya Kawata | 
| Temuri Doghonadze | 
| Tender Wang | 
| Tenglong Gu | 
| Thom Brown | 
| Thomas Gabriel de Medeiros Silva | 
| Thomas Kellerer | 
| Thomas Munro | 
| Tianchen Zhang | 
| Tiancheng Ge | 
| Tim Wood | 
| Timur Magomedov | 
| Tingchuan Sun | 
| Tobias Bussmann | 
| Todd Lang | 
| Todd Liebenschutz-Jones | 
| Tom Lane | 
| Tomas Vondra | 
| Tomer Fichman | 
| Torsten Förtsch | 
| Torsten Rupp | 
| Tristan Partin | 
| Tushar Ahuja | 
| Tyler Smart | 
| Ulisses de Mello dos Santos | 
| Vaibhav Dalvi | 
| Vaibhav Jain | 
| Vaibhave Sekar | 
| Valery Gubanov | 
| Varik Matevosyan | 
| Vasuki M | 
| Victor Yegorov | 
| Vignesh C | 
| Vik Fearing | 
| Viktor Holmberg | 
| Viktor Leis | 
| Vinod Sridharan | 
| Vishal Prasanna | 
| Vismay Tiwari | 
| Vitaly Davydov | 
| Vlad Lesin | 
| Vladimir Tokarev | 
| Walid Ibrahim | 
| Wenhui Qiu | 
| Xiangyu Liang | 
| Xiaohui Liu | 
| Xiaopeng Wang | 
| Xing Guo | 
| Xingbin She | 
| Xingwang Xiang | 
| Xueyu Gao | 
| Xuneng Zhou | 
| Xunqi Hu | 
| Yang Zhu | 
| Yanjie Zhao | 
| Yanliang Lei | 
| Yaroslav Saburov | 
| Yaroslav Syrytsia | 
| Yasuo Honda | 
| Yi Ding | 
| Yilin Zhang | 
| Yingying Chen | 
| Yiyang Liu | 
| Yonghao Lee | 
| Yonghwa Lee | 
| Yu Wang | 
| Yuan Li | 
| Yuanchao Zhang | 
| Yuanzhou Yang | 
| Yuchen Li | 
| Yuefei Shi | 
| Yuelin Wang | 
| Yugo Nagata | 
| Yuhang Qiu | 
| Yuhang Wu | 
| Yura Sokolov | 
| Yuri Zamyatin | 
| Yuxiao Guo | 
| Zane Duffield | 
| Zechman, Derek S | 
| Zengqi Gao | 
| Zepeng Zhang | 
| Zexin Li | 
| Zhang Hu | 
| Zhang Mingli | 
| Zheng Tao | 
| Zheng Wang | 
| Zheng Yu | 
| Zhenpeng Lin | 
| Zhenwei Shang | 
| Zhibai Song | 
| Zhibin Wang | 
| Zhihong Yu | 
| Zhijie Hou | 
| Zhiyuan Su | 
| Zhou Digoal | 
| Zizhen Qiao | 
| Zizhuan Liu | 
| Zsolt Parragi | 

If you see anything in the documentation that is not correct, does not match your experience with the particular feature or requires further clarification, please use this form to report a documentation issue.
