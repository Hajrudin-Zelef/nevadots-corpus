---
id: collect-261121-veille-ia/langages-dev/disqlite3-version-history-yunqa-1
title: "DISQLite3: Version History [Yunqa"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["sqlite3", "sqlite", "result", "disqlite3", "version", "extension", "table", "added"]
source: https://www.yunqa.de/delphi/products/sqlite3/history
source_anchor: ""
source_lines: [1, 1500]
section: "Langages_Dev"
---
# DISQLite3: Version History

## DISQLite3 v6.2.0 – 22 Oct 2026

### Breaking Changes

#### ''sqlite3_result_str()'' → ''sqlite3_result_str8()''

SQLite added the new `sqlite3_result_str` API for its `sqlite3_str` dynamically-sized string. This conflicted with the existing Delphi RawByteString wrapper `sqlite3_result_str()`, which wraps `sqlite3_result_text64`.

Resolution: The old function was renamed to `sqlite3_result_str8`.

Migration: Replace old calls to `sqlite3_result_str` with `sqlite3_result_str8` in your codebase.

#### Removed ''blobio'' and ''closure'' extensions

- Removed the`blobio` extension, which was “unused, untested, experiemental” according to the SQLite developers.
- Removed the`closure` extension, which was “obsolete and unsupported” and “not important for anything anymore” in the words of the SQLite developers.

#### Drop Windows XP support

- Drop support for Windows XP and earlier Windows versions, including WindowsCE. DISQLite now requires Windows Vista (circa 2007) or later.

### New Functionality

- Add the vec1 extension to provide approximate nearest-neighbor (ANN) vector search.
- Add the diskused() SQL function extension to report database storage statistics.
- Add the`sqlite3_result_str` interface to which makes a [sqlite3_str] the return value for an application-defined function or virtual table. See breaking change note above.
- Add the`sqlite3_incomplete` interface.
- Add the`YuSQLite3Helpers` unit.

### SQLite Changes

Update SQLite to version 3.54.1.

- Fix a possible heap-buffer overflow on 32-bit builds.
- Query planner improvements:
  - When doing an UPDATE on a table that has expression indexes, do not change the expression index if the value being indexed is unchanged.
  - Improved optimization of “*expr* OR TRUE” and “*expr* OR FALSE”.
  - Ignore DISTINCT in the SELECT statement on the right-hand side of an IN operator.
  - UNION queries with a LIMIT of exactly 1 are now handled faster and with less memory.
- The ALTER TABLE command raises an error on any attempt to add, drop, or rename columns with the special names “ROWID”, “_ROWID_”, or “OID”.
- Added the %J and %j conversions used to render JSON string literals to the built-in printf() and the format() SQL function.
- Enhancements to date/time functions:
  - Added the “weekday -N” form of the weekday modifier.
  - Added the end of modifier.
  - Improve performance by using Slim Reader/Writer Locks (SRWLs) to implement non-recursive mutexes, instead of CriticalSection, since all supported Windows platforms now provide SRWLs.
- The authorizer callback is now invoked for SQL functions found in the DEFAULT clauses of CREATE TABLE statements. This happens both when the CREATE TABLE statement is first issued and when the DEFAULT value is actually used, but not when reading the schema while opening a database.
- Added`SQLITE_LIMIT_SCHEMA` and`SQLITE_LIMIT_TRIGGER_STEPS` to the`sqlite3_limit` interface.

## DISQLite3 v6.1.3 – 9 Sep 2026

Update SQLite to version 3.53.4.

- This is a maintenance patch release with fixes for (mostly) AI-discovered bugs.
- Deprecate`sqlite3_closure_init()` as the`closure` extension is now marked as experimental and obsolete in SQLite.

## DISQLite3 v6.1.2 – 8 Jun 2026

Update SQLite to version 3.53.2:

- Various fixes.
- Remove the`scrub` extension and its`sqlite3_scrub_backup()` function. Use VACUUM INTO instead.

## DISQLite3 v6.1.1 – 5 May 2026

Update SQLite to version 3.53.1:

- Fix`sqlite3_snprintf` so it does not incorrectly truncate floating-point conversions.
- Bug fix in`sqlite3_str_free` : Do not crash if called on an`sqlite3_str` object returned from`sqlite3_str_new` following an OOM.
- Handle the case where the name of the constraint in an “ALTER TABLE DROP CONSTRAINT” command is quoted.
- Handle a special case of a corrupt changeset in`sqlite3changegroup_add` .
- Fix a bug in EXISTS-to-JOIN having to do with an OFFSET clause of a LIMIT.
- Fix a single-byte OOB read that could occur in the session module when concatenating patchsets.
- Fix a crash that could occur if`sqlite3_deserialize` was used to overwrite a database with an open transaction on it.
- Harden the recovery extension against SQL injections coming from the`sqlite_schema` table of the database being recovered.
- Fix OR-optimization on a loop created by exists-to-join.
- Other minor bug fixes.

## DISQLite3 v6.1.0 – 16 Apr 2026

Update SQLite to version 3.53.0:

- New SQL language features:
  - The REINDEX EXPRESSIONS statement rebuilds expression indexes. (Useful to repair stale expression indexes.)
- Add new interfaces to the session extension that enable an application to add changes one at a time to the sqlite3_changegroup object:
  - `sqlite3changegroup_change_begin` .
  - `sqlite3changegroup_change_blob` .
  - `sqlite3changegroup_change_double` .
  - `sqlite3changegroup_change_int64` .
  - `sqlite3changegroup_change_null` .
  - `sqlite3changegroup_change_text` .
  - `sqlite3changegroup_change_finish` .
  - `sqlite3changegroup_config` .
- Added the self-healing index feature to deal with the stale expression index problem.

## DISQLite3 v6.0.0 – 7 Mar 2026

DISQLite changes:

- Improve`DISQLite3Ext` :
  - New`sqlite3_api` variable which can be set by extensions.
  - Remove`sqlite3_…` prefix from sqlite3_api_routines method names, so they now match the original SQLite declarations.
  - Add new`sqlite3_…` functions and procedures which call the respective sqlite3_api_routines method of`sqlite3_api` . For this to work, the`sqlite3_api` variable must have been assigned by the extension's entry point.
- The carray() table-valued function is now enabled by default.`sqlite3_carray_init` is deprecated, no longer needed, and does nothing.
- Update zlib to v1.3.2.

Update SQLite to version 3.52.0:

- Fix the WAL-reset database corruption bug.
- Enhance ALTER TABLE to permit adding and removing NOT NULL and CHECK constraints.
- Add the Query Result Formatter (QRF) extension for formatting the results of SQL queries for human readability or interchange output formats.
- New built-in SQL functions: json_array_insert, and jsonb_array_insert.
- New C-language interfaces:
  - `sqlite3_str_truncate`
  - `sqlite3_str_free`
  - `sqlite3_carray_bind_v2`
  - Add the`SQLITE_PREPARE_FROM_DDL` option to`sqlite3_prepare_v3` which permits virtual table implementations to safely prepare SQL statements that are derived from the database schema.
  - Added the`SQLITE_UTF8_ZT` constant which can be used as the encoding parameter to`sqlite3_result_text64` or`sqlite3_bind_text64` to indicate that the value is UTF-8 encoded and zero terminated.
  - The`SQLITE_LIMIT_PARSER_DEPTH` option is added to`sqlite3_limit` .
  - The`SQLITE_DBCONFIG_FP_DIGITS` option is added to`sqlite3_db_config` .
- Query planner improvements:
  - Always use a sort-and-merge algorithm for EXCEPT, INTERSECT, and UNION, since this is almost always faster than using a hash table.
  - Improvements to join order selection in large multi-way joins on a star schema.
  - Enhance the EXISTS-to-JOIN optimization so that the inserted JOIN terms are not required to be on the inner-most loops, as long as all dependencies for the EXISTS-to-JOIN loops are in outer loops.
  - Enhance the omit-noop-join optimization so that it is able to omit a chain of joins that do not affect the output.
  - Allow queries that use “GROUP BY e1 ORDER BY e2” where e1 and e2 are identical apart from ASC/DESC sort-orders to be optimized using a single index.
  - Allow virtual tables to optimize DISTINCT in cases where the result-set of a query does not exactly match the ORDER BY clause.
- Improvements to floating-point ↔ text conversions.
  - Reimplemented to improve performance.
  - Rounding is now done by default to 17 significant digits, instead of 15, as was the case for all prior versions. The`sqlite3_db_config` (`SQLITE_DBCONFIG_FP_DIGITS` ) API can change this, if desired.
- The body of TEMP triggers may now modify and/or query tables in the main schema.
- Enhance VACUUM INTO so that if a URI filename is used as the target and that filename has a reserve=N query parameter with N between 0 and 255, then the reserve amount for the generated database copy is set to N.

Removals:

- The experimental`checkfreelist` and`checkindex` extensions were removed from SQLite.

## DISQLite3 v5.54.2 – 29 Jan 2026

Update to SQLite Version 3.51.2.

- Fix multiple problems in the EXISTS-to-JOIN optimization.
- Other minor bug fixes.

## DISQLite3 v5.54.1 – 3 Dec 2025

Update to SQLite Version 3.51.1.

- Fix incorrect results from nested EXISTS queries.
- Fix a latent bug in fts5vocab virtual table.

## DISQLite3 v5.54.0 – 5 Nov 2025

Update to SQLite Version 3.51.0.

- Two new JSON functions, jsonb_each and jsonb_tree work the same as the existing json_each() and json_tree() functions except that they return JSONB for the “value” column when the “type” is 'array' or 'object'.
- Performance enhancements:
  - Use fewer CPU cycles to commit a read transaction.
  - Early detection of joins that return no rows due to one or more of the tables containing no rows.
  - Avoid evaluation of scalar subqueries if the result of the subquery does not change the result of the overall expression.
  - Faster window function queries when using`BETWEEN :x FOLLOWING AND :y FOLLOWING` with a large`:y` .
- Add the PRAGMA wal_checkpoint=NOOP; command and the`SQLITE_CHECKPOINT_NOOP` argument for`sqlite3_wal_checkpoint_v2` .
- Add the`sqlite3_api_routines.set_errmsg` API for use by extensions.
- Add the`sqlite3_db_status64` API, which works just like the existing`sqlite3_db_status` API except that it returns 64-bit results.
- Add the`SQLITE_DBSTATUS_TEMPBUF_SPILL` option to the`sqlite3_db_status` and`sqlite3_db_status64` interfaces.
- In the session extension, add the`sqlite3changeset_apply_v3` interface.
- For the built-in printf() and the format() SQL function, omit the leading '-' from negative floating point numbers if the '+' flag is omitted and the “#” flag is present and all displayed digits are '0'. Use '%#f' or similar to avoid outputs like '-0.00' and instead show just '0.00'.
- Improved error messages generated by FTS5.
- Enforce STRICT typing on computed columns.
- Deprecate the LSM extension and the LSM database functions. All LSM code has been removed from SQLite.

## DISQLite3 v5.53.0 – 18 Sep 2025

- Support Delphi 13 Florence Win32 and Win64.
- Update DIUtils.pas Unicode functions to Unicode 17.0.0.

## DISQLite3 v5.52.3 – 7 Aug 2025

Update to SQLite Version 3.50.4.

- Fix two long-standings cases of the use of uninitialized variables in obscure circumstances.

## DISQLite3 v5.52.2 – 22 Jul 2025

Update to SQLite Version 3.50.3.

- Fix a possible memory error that can occur if a query is made against against a FTS5 index that has been deliberately corrupted in a very specific way.
- Fix the parser so that it ignores SQL comments in all places of a CREATE TRIGGER statement.
- Fix an incorrect answer due to over-optimization of an AND operator.

## DISQLite3 v5.52.1 – 1 Jul 2025

Update to SQLite Version 3.50.2.

- Fix the concat_ws SQL function so that it includes empty strings in the concatenation.
- Avoid writing frames with no checksums into the wal file if a savepoint is rolled back after dirty pages have already been spilled into the wal file.
- Fix the Bitvec object to avoid stack overflow when the database is within 60 pages of its maximum size.
- Fix a problem with UPDATEs on fts5 tables that contain BLOB values.
- Fix an issue with transitive IS constraints on a RIGHT JOIN.
- Raise an error early if the number of aggregate terms in a query exceeds the maximum number of columns, to avoid downstream assertion faults.

## DISQLite3 v5.52.0 – 13 Jun 2025

Update to SQLite Version 3.50.1.

- Enhancements to the query planner:
  - Allow the right-hand side of a LEFT JOIN to be flattened even if it is a virtual table.
  - Improve the query-time index optimization so that it works on WITHOUT ROWID tables.
  - Better query plans for large star-query joins.
  - When two or more queries have the same estimated cost, use the one with the fewer bytes per row.
  - Fix a bug in the NOT NULL optimization that can lead to a memory error if abused.
  - Fix the count-of-view optimization so that it does not give an incorrect answer for a DISTINCT query.
- Fix a possible incorrect answer that can result if a UNIQUE constraint of a table contains the PRIMARY KEY column and that UNIQUE constraint is used by an IN operator.
- New SQL functions:
  - unistr
  - unistr_quote
- Fix a bug in the concat_ws function that could lead to a memory error, specifically a write past the end of allocated space, if the separator string is larger than two megabytes.
- Enhance the iif() SQL function so that it can accept any number of arguments greater than or equal to two.
- For the %Q and %q conversions in the built-in printf() (which covers the`sqlite3_mprintf` API and the format() SQL function and similar) the alternate-form-1 flag (“#”) causes control characters to be converted into backslash-escapes suitable for unistr.
- Changes to JSON functions:
  - Bug fix: Enforce the JSON5 restriction that the “\0” escape must not be followed by a digit.
  - Bug fix: When the LABEL argument to json_group_object(LABEL,VALUE) is NULL, that element of the resulting object is omitted.
  - Optimization: If the jsonb_set or jsonb_replace functions make a change in the interior of a large JSONB object, they strive to keep the size of the JSONB object unchanged and to modify as few bytes as possible on the interior of the object. This helps reduce I/O as it allows SQLite to write only the page that contains the changed bytes and not all the surrounding pages.
  - Fix a long-standing bug in jsonb_set and similar.
- Fix obscure problems with the generate_series() extension function.
- Enhance the session extension so that it works on databases that make use of generated columns.
- Three new options to`sqlite3_db_config` . All default to “on”.
  - `SQLITE_DBCONFIG_ENABLE_ATTACH_CREATE`
  - `SQLITE_DBCONFIG_ENABLE_ATTACH_WRITE`
  - `SQLITE_DBCONFIG_ENABLE_COMMENTS`
- Enhanced the`SQLITE_DBCONFIG_LOOKASIDE` interface to make it more robust against misuse.
- Add the`sqlite3_setlk_timeout` interface which sets a separate timeout, distinct from the`sqlite3_busy_timeout` , for blocking locks on builds that support blocking locks.
- Miscellaneous performance improvements.

## DISQLite3 v5.51.0 – 21 Jan 2025

Update to SQLite Version 3.48.0.

- Remove the async extension. It was superseded by WAL mode about 11 years ago, and has long been deprecated.
- Improved EXPLAIN QUERY PLAN output for covering indexes.
- Allow a two-argument version of the iif() SQL function. Also allow if as an alternative spelling for iif.
- Add the SQLITE_IOCAP_SUBPAGE_READ property to the`xDeviceCharacteristics` method of the sqlite3_io_methods object.
- Add the SQLITE_PREPARE_DONT_LOG option to`sqlite3_prepare_v3` that prevents warning messages being sent to the error log if the SQL is ill-formed. This allows`sqlite3_prepare_v3` to be used to do test compiles of SQL to check for validity without polluting the error log with false messages.
- Increase the minimum allowed value of SQLITE_LIMIT_LENGTH from 1 to 30.
- Added the SQLITE_FCNTL_NULL_IO file control.
- Extend the FTS5 auxiliary API xInstToken() to work with prefix queries via the insttoken configuration option and the fts5_insttoken SQL function.
- Increase the maximum number of arguments to an SQL function from 127 to 1000.

## DISQLite3 v5.50.2 – 10 Dec 2024

Update to SQLite Version 3.47.2.

- Fix a problem in text-to-floating-point conversion for SQLite that can cause values between '1.8446744073709550592eNNN' and '1.8446744073709551609eNNN' for any exponent NNN to be rendered incorrectly. In other words, some numeric text values where the first 16 significant digits are '1844674407370955' might be converted into the wrong floating-point value.
- Other minor bug fixes.

## DISQLite3 v5.50.1 – 26 Nov 2024

Update to SQLite Version 3.47.1.

- Add the`SQLITE_IOCAP_SUBPAGE_READ` capability to the VFS, to work around issues on some non-standard VFSes.
- Fix incorrect answers to certain obscure IN queries.
- Other minor bug fixes.

## DISQLite3 v5.50.0 – 8 Nov 2024

Update to SQLite Version 3.47.0.

- Allow arbitrary expressions in the second argument to the RAISE function.
- If the RHS of the`→>` operator is negative, then access array elements counting from the right.
- FTS5 tables can now be dropped even if they use a non-standard tokenizer that has not been registered.
- Fix the group_concat() aggregate function so that it returns an empty string, not a`NULL` , if it receives a single input value which is an empty string.
- Enhance the generate_series table-valued function so that it is able to recognize and use constraints on its output value.
- Preupdate hooks now recognize when a column added by ALTER TABLE ADD COLUMN has a non-`NULL` default value.
- Enhance the sqlite_dbpage table-valued function such that INSERT can be used to increase or decrease the size of the database file.

**Performance optimizations:**

- Improved reuse of subqueries associated with the IN operator, especially when the IN operator has been duplicated due to predicate push-down.
- Use a Bloom filter on subqueries on the right-hand side of the IN operator, in cases where that seems likely to improve performance.
- Ensure that queries like “`SELECT func(a) FROM tab GROUP BY 1` ” only invoke the func() function once per row.
- No attempt is made to create automatic indexes on a column that is known to be non-selective because of its use in other indexes that have been analyzed.
- Adjustments to the query planner so that it produces better plans for star queries with a large number of dimension tables.
- Add the “order-by-subquery” optimization, that seeks to disable sort operations in outer queries if the desired order is obtained naturally due to ORDER BY clauses in subqueries.
- The “indexed-subtype-expr” optimization strives to use expressions that are part of an index rather than recomputing the expression based on table values, as long as the query planner can prove that the subtype of the expression will never be used.

**FTS5 Changes:**

- Add the`fts5_tokenizer_v2` API and the`locale=1` option, for creating custom locale-aware tokenizers and fts5 tables that may take advantage of them. See`DISQLite3Fts5Tokenizer_v2` for a`fts5_tokenizer_v2` example tokenizer.
- Add the`contentless_unindexed=1` option, for creating contentless fts5 tables that store the values of any UNINDEXED columns persistently in the database.
- Allow an FTS5 table to be dropped even if it uses a custom tokenizer whose implementation is not available.

## DISQLite3 v5.49.1 – 14 Aug 2024

Update to SQLite Version 3.46.1.

- Improved robustness while parsing the tokenize= arguments in FTS5.
- Enhancements to covering index prediction in the query planner. Add early detection of over-prediction of covering indexes so that`sqlite3_prepare` will return an error rather than just generate bad bytecode.
- Do not let the number of terms on a VALUES clause be limited by SQLITE_LIMIT_COMPOUND_SELECT, even if the VALUES clause contains elements that appear to be variables due to double-quoted string literals.
- Fix the window function version of lang_aggfunc.html#group_concat so that it returns an empty string if it has one or more empty string inputs.
- In FTS5 secure-delete mode, fix false-positive integrity-check reports about corrupt indexes.
- Syntax errors in ALTER TABLE should always return`SQLITE_ERROR` . In some cases, they were formerly returning`SQLITE_INTERNAL` .
- Other minor fixes.

## DISQLite3 v5.49.0 – 30 Mar 2024

Update to SQLite Version 3.46.0.

- Enhance PRAGMA optimize in multiple ways, to make it simpler to use:
  - PRAGMA optimize automatically implements a temporary analysis limit to prevent excess runtime on large databases.
  - Added the new 0x10000 bitmask option to check for updates on all tables.
  - Automatically re-analyze tables that do not have sqlite_stat1 entries.
- Enhancements to the date and time functions:
  - The strftime() SQL function now supports %G, %g, %U, and %V.
  - New modifiers 'ceiling' and 'floor' control the algorithm used to resolve ambiguous dates when shifting a date by an integer number of months and/or years.
  - The 'utc' and 'localtime' modifiers are now no-ops if SQLite knows that the time is already in UTC or in the localtime, respectively.
- Add support for underscore (“_”) characters between digits in numeric literals.
- Add the json_pretty SQL function.
- Query planner improvements:
  - The “VALUES-as-coroutine” optimization enables INSERT statements with thousands of rows in the VALUES clause to parse and run in about half the time and using about half as much memory.
  - Allow the use of an index for queries like “SELECT count(DISTINCT col) FROM …”, even if the index records are not smaller than the table records.
  - Improved recognition of cases where the value of an SQL function is constant because all its arguments are constant.
  - Enhance the WHERE-clause push-down optimization so that it is able to push down WHERE clause terms containing uncorrelated subqueries.
- Allocate additional memory from the heap for the SQL parser stack if that stack overflows, rather than reporting a “parser stack overflow” error.
- JSON changes:
  - Allow ASCII control characters within JSON5 string literals.
  - Fix the → and →> operators so that when the right-hand side operand is a string that looks like an integer it is still treated as a string, because that is what PostgreSQL does.
- Allow large hexadecimal literals to be used as the DEFAULT value to a table column.
- Add the randomjson extension.

## DISQLite3 v5.48.3 – 17 Apr 2024

- Update to SQLite Version 3.45.3.
  - Fix a long-standing bug that might (rarely) cause the “old.*” values of an UPDATE trigger to be incorrect if that trigger fires in response to an UPSERT.
  - Fix a bug in sum that could cause it to return NULL when it should return Infinity.
  - Enforce that application defined SQL functions that invoke`sqlite3_result_subtype` must be registered with`SQLITE_RESULT_SUBTYPE` . Failing to do so now causes`sqlite3_result_subtype` to raise an SQL error whereas before it might have behaved as a no-op.

## DISQLite3 v5.48.2 – 13 Mar 2024

- Update to SQLite Version 3.45.2.
  - Fix an error in UPSERT that could cause an index to get out-of-sync with its table.
  - Reduce the scope of the NOT NULL strength reduction optimization. The optimization was being attempted in some contexts where it did not work, resulting in incorrect query results.
  - Other trifling corrections and fixes that have come up since the previous patch release.

## DISQLite3 v5.48.1 – 31 Jan 2024

- Update to SQLite Version 3.45.1.
  - Restore the JSON BLOB input bug, and promise to support the anomaly in subsequent releases, for backward compatibility.
  - Fix the PRAGMA integrity_check command so that it works on read-only databases that contain FTS3 and FTS5 tables.
  - Fix issues associated with processing corrupt JSONB inputs:
    - Prevent exponential runtime when converting a corrupt JSONB into text.
    - Fix a possible read of one byte past the end of the JSONB blob when converting a corrupt JSONB into text.
  - Fix a long-standing bug in which a read of a few bytes past the end of a memory-mapped segment might occur when accessing a craftily corrupted database using memory-mapped database.
  - Fix a long-standing bug in which a`nil` pointer dereference might occur in the bytecode engine due to incorrect bytecode being generated for a class of SQL statements that are deliberately designed to stress the query planner but which are otherwise pointless.

## DISQLite3 v5.48.0 – 19 Jan 2024

- Update to SQLite Version 3.45.0.
  - Added the`SQLITE_RESULT_SUBTYPE` property for application-defined SQL functions. All application defined SQL functions that invokes`sqlite3_result_subtype` must be registered with this new property. Failure to do so might cause the call to`sqlite3_result_subtype` to behave as a no-op.
  - Enhancements to the JSON SQL functions:
    - All JSON functions are rewritten to use a new internal parse tree format called JSONB. The new parse-tree format is serializable and hence can be stored in the database to avoid unnecessary re-parsing whenever the JSON value is used.
    - New versions of JSON-generating functions generate binary JSONB instead of JSON text.
    - The json_valid() function adds an optional second argument that specifies what it means for the first argument to be “well-formed”.
  - Add the FTS5 tokendata option to the FTS5 virtual table.
  - Query planner improvements:
    - Do not allow the transitive constraint optimization to trick the query planner into using a range constraint when a better equality constraint is available.
    - The query planner now does a better job of disregarding indexes that ANALYZE identifies as low-quality.
  - Increase the default value for SQLITE_MAX_PAGE_COUNT from 1073741824 to 4294967294.

## DISQLite3 v5.47.1 – 27 Nov 2023

- Update to SQLite Version 3.44.2.
  - Fix a problem in FTS5 that was discovered during internal fuzz testing.
  - Other obscure bug fixes.

## DISQLite3 v5.47.0 – 22 Nov 2023

- Support Delphi 12 Athens Win32 and Win64.
- Update to SQLite Version 3.44.0.
  - Aggregate functions can now include an ORDER BY clause after their last parameter. The arguments to the function are processed in the order specified. This can be important for functions like string_agg and json_group_array.
  - Add support for the concat and concat_ws scalar SQL functions, compatible with PostgreSQL, SQLServer, and MySQL.
  - Add support for the string_agg aggregate SQL function, compatible with PostgreSQL and SQLServer.
  - New conversion letters on the strftime() SQL function: %e %F %I %k %l %p %P %R %T %u
  - Add new APIs:`sqlite3_get_clientdata` and`sqlite3_set_clientdata` .
  - Many errors associated with CREATE TABLE are now raised when the CREATE TABLE statement itself is run, rather than being deferred until the first time the table is actually used.
  - The PRAGMA integrity_check command now verifies the consistency of the content in various built-in virtual tables using the new xIntegrity method. This works for the FTS3, FTS4, FTS5, RTREE, and GEOPOLY extensions.
  - The SQLITE_DBCONFIG_DEFENSIVE setting now prevents PRAGMA writable_schema from being turned on. Previously writable_schema could be turned on, but would not actually allow the schema to be writable. Now it simply cannot be turned on.
  - Tag the built-in FTS3, FTS4, FTS5, RTREE, and GEOPOLY virtual tables as SQLITE_VTAB_INNOCUOUS so that they can be used inside of triggers in high-security deployments.
  - The PRAGMA case_sensitive_like statement is deprecated, as its use when the schema contains LIKE operators can lead to reports of database corruption by PRAGMA integrity_check.
  - Query planner optimizations:
    - In partial index scans, if the WHERE clause implies a constant value for a table column, replace occurrences of that table column with the constant. This increases the likelihood of the partial index being a covering index.
    - Disable the view-scan optimization as it was causing multiple performance regressions. In its place, reduce the estimated row count for DISTINCT subqueries by a factor of 8.

## DISQLite3 v5.46.1 – 11 Oct 2023

Update to SQLite Version 3.43.2.

- Fix obscure use after free (UAF) errors in JSON and FTS5.
- Fix an obscure memory leak in`sqlite3_blob_reopen` .
- Improve detection of corrupt databases.

## DISQLite3 v5.46.0 – 13 Sep 2023

Update to SQLite Version 3.43.1.

- Fix a regression in the way that the sum(), avg(), and total() aggregate functions handle infinities.
- Fix a bug in the json_array_length() function that occurs when the argument comes directly from json_remove.
- Fix the omit-unused-subquery-columns optimization so that it works correctly if the subquery is a compound where one arm is DISTINCT and the other is not.
- Other minor fixes.

## DISQLite3 v5.45.0 – 26 Aug 2023

Update to SQLite Version 3.43.0.

Key enhancements include added support for Contentless-Delete FTS5 Indexes, and performance improvements in JSON processing.

Details:

- Add support for Contentless-Delete FTS5 Indexes. This is a variety of FTS5 full-text search index that omits storing the content that is being indexed while also allowing records to be deleted.
- Enhancements to the date and time functions:
  - Added new time shift modifiers of the form`±YYYY-MM-DD HH:MM:SS.SSS` .
  - Added the timediff() SQL function.
- Added the octet_length(X) SQL function.
- Added the`sqlite3_stmt_explain` API.
- Query planner enhancements:
  - Generalize the LEFT JOIN strength reduction optimization so that it works for RIGHT and FULL JOINs as well. Rename it to OUTER JOIN strength reduction.
  - Enhance the theorem prover in the OUTER JOIN strength reduction optimization so that it returns fewer false-negatives.
- Enhancements to the decimal extension:
  - New function decimal_pow2(N) returns the N-th power of 2 for integer N between -20000 and +20000.
  - New function decimal_exp(X) works like decimal(X) except that it returns the result in exponential notation - with a “e+NN” at the end.
  - If X is a floating-point value, then the decimal(X) function now does a full expansion of that value into its exact decimal equivalent.
- Performance enhancements to JSON processing results in a 2x performance improvement for some kinds of processing on large JSON strings.

## DISQLite3 v5.44.0 – 17 May 2023

- Add the FTS5 secure-delete command. This option causes all forensic traces to be removed from the FTS5 inverted index when content is deleted.
- Enhance the JSON SQL functions to support JSON5 extensions.
- The`SQLITE_CONFIG_LOG` and`SQLITE_CONFIG_PCACHE_HDRSZ` calls to`sqlite3_config` are now allowed to occur*after*`sqlite3_initialize` .
- New`sqlite3_db_config` options:`SQLITE_DBCONFIG_STMT_SCANSTATUS` and`SQLITE_DBCONFIG_REVERSE_SCANORDER` .
- Query planner improvements:
  - Enable the “count-of-view” optimization by default.
  - Avoid computing unused columns in subqueries.
  - Improvements to the push-down optimization.
- Add the ability for application-defined SQL functions to have the same name as join keywords: CROSS, FULL, INNER, LEFT, NATURAL, OUTER, or RIGHT.
- Enhancements to PRAGMA integrity_check:
  - Detect and raise an error when a NaN value is stored in a`NOT NULL` column.
  - Improved error message output identifies the root page of a b-tree when an error is found within a b-tree.
- Allow the session extension to be configured to capture changes from tables that lack an explicit ROWID.
- Added the subsecond modifier to the date and time functions.
- Negative values passed into`sqlite3_sleep` are henceforth interpreted as 0.
- The maximum recursion depth for JSON arrays and objects is lowered from 2000 to 1000.
- Extended the built-in printf() function so the comma option now works with floating-point conversions in addition to integer conversions.
- Miscellaneous bug fixes and performance optimizations

## DISQLite3 v5.43.2 – 24 Mar 2023

- Multiple fixes for reads past the end of memory buffers (NB:*reads* not*writes* ) in the following circumstances:
  - When processing a corrupt database file.
  - In the recovery extension.
  - In FTS3 when processing a corrupt database file.
- Fix`sqlite3_error_offset` so that it does not return out-of-range values when reporting errors associated with generated columns.
- Multiple fixes in the query optimizer for problems that cause incorrect results for bizarre, fuzzer-generated queries.
- Increase the size of the reference counter in the page cache object to 64 bits to ensure that the counter never overflows.
- Fix a performance regression caused by a bug fix in patch release v5.43.1.

## DISQLite3 v5.43.1 – 13 Mar 2023

- Ensure that the datatype for column t1.x in`CREATE TABLE t1 AS SELECT CAST(7 AS INT) AS x;` continues to be INT and is not NUM, for historical compatibility.
- Enhance PRAGMA integrity_check to detect when extra bytes appear at the end of an index record.
- Fix various obscure bugs.

## DISQLite3 v5.43.0 – 24 Feb 2023

- Query planner improvements:
  - Make use of indexed expressions within an aggregate query that includes a GROUP BY clause.
  - The query planner has improved awareness of when an index is a covering index and adjusts predicted runtimes accordingly.
  - The query planner is more aggressive about using co-routines rather than materializing subqueries and views.
  - Queries against the built-in table-valued functions json_tree and json_each will now usually treat “ORDER BY rowid” as a no-op.
  - Enhance the ability of the query planner to use indexed expressions even if the expression has been modified by the constant-propagation optimization.
- Add the built-in unhex SQL function.
- Add the base64 and base85 application-defined functions as an extension.
- Add the`sqlite3_stmt_scanstatus_v2` interface.
- In-memory databases created using`sqlite3_deserialize` now report their filename as an empty string, not as 'x'.
- Enhance the PRAGMA integrity_check command so that it detects when text strings in a table are equivalent to but not byte-for-byte identical to the same strings in the index.
- Enhance the carray table-valued function so that it is able to bind an array of BLOB objects.
- Added the`sqlite3_is_interrupted` interface.
- Long-running calls to`sqlite3_prepare` and similar now invoke the progress handler callback and react to`sqlite3_interrupt` .
- The`sqlite3_vtab_in_first` and`sqlite3_vtab_in_next` functions are enhanced so that they reliably detect if they are invoked on a parameter that was not selected for multi-value IN processing using`sqlite3_vtab_in` . They return`SQLITE_ERROR` instead of`SQLITE_MISUSE` in this case.
- The parser now ignores excess parentheses around a subquery on the right-hand side of an IN operator, so that SQLite now works the same as PostgreSQL in this regard. Formerly, SQLite treated the subquery as an expression with an implied “LIMIT 1”.
- Added the`SQLITE_FCNTL_RESET_CACHE` option to the`sqlite3_file_control` API.
- Fix problems with large compressed LSM databases.
- Miscellaneous performance enhancements.
- Remove the long-deprecated FTS1 and FTS2 full text search modules.

## DISQLite3 v5.42.1 – 3 Jan 2023

- Fix a potential infinite loop in the memsys5 alternative memory allocator.
- Various other obscure fixes.

## DISQLite3 v5.42.0 – 16 Nov 2022

- Add the recovery extension that might be able to recover some content from a corrupt database file.
- Query planner enhancements:
  - Recognize covering indexes on tables with more than 63 columns where columns beyond the 63rd column are used in the query and/or are referenced by the index.
  - Extract the values of expressions contained within expression indexes where practical, rather than recomputing the expression.
  - The NOT NULL and IS NULL operators (and their equivalents) avoid loading the content of large strings and BLOB values from disk.
  - Avoid materializing a view on which a full scan is performed exactly once. Use and discard the rows of the view as they are computed.
  - Allow flattening of a subquery that is the right-hand operand of a LEFT JOIN in an aggregate query.
- A new typedef named`sqlite3_filename` is added and used to represent the name of a database file. Various interfaces are modified to use the new typedef instead of`C_char_ptr` . This interface change should be fully backwards compatible, though it might cause (harmless) compiler warnings when rebuilding some legacy applications.
- Add the`sqlite3_value_encoding` interface.
- Security enhancement: SQLITE_DBCONFIG_DEFENSIVE is augmented to prohibit changing the schema_version. The schema_version becomes read-only in defensive mode.
- Enhancements to the PRAGMA integrity_check statement:
  - Columns in non-STRICT tables with TEXT affinity should not contain numeric values.
  - Columns in non-STRICT tables with NUMERIC affinity should not contain TEXT values that could be converted into numbers.
  - Verify that the rows of a WITHOUT ROWID table are in the correct order.
- Enhance the VACUUM INTO statement so that it honors the PRAGMA synchronous setting.
- Enhance the`sqlite3_strglob` and`sqlite3_strlike` APIs so that they are able to accept`nil` pointers for their string parameters and still generate a sensible result.
- Change the algorithm used by SQLite's built-in pseudo-random number generator (PRNG) from RC4 to Chacha20.
- Allow two or more indexes to have the same name as long as they are all in separate schemas.
- Miscellaneous performance optimizations result in about 1% fewer CPU cycles used on typical workloads.

## DISQLite3 v5.41.4 – 5 Oct 2022

This is a security release that addresses a single long-standing problem in the FTS3 extension. An attacker who is able to execute arbitrary SQL statements and who can create a corrupt database that is 2GB or larger in size might be able to trick FTS3 into overflowing an integer used to size a memory allocation, causing the allocation to be too small and ultimately resulting in a buffer overrun. It also includes fixes for other obscure weaknesses.

- Fix a long-standing problem in the btree balancer that might, in rare cases, cause database corruption if the application uses an application-defined page cache.
- Enhance SQLITE_DBCONFIG_DEFENSIVE so that it disallows CREATE TRIGGER statements if one or more of the statements in the body of the trigger write into shadow tables.
- Fix a possible integer overflow in the size computation for a memory allocation in FTS3.

## DISQLite3 v5.41.3 – 7 Sep 2022

- Use a statement journal on DML statement affecting two or more database rows if the statement makes use of SQL functions that might abort.
- Use a mutex to protect the PRAGMA temp_store_directory and PRAGMA data_store_directory statements, even though they are deprecated and documented as not being threadsafe.
- Other bug and warning fixes.

## DISQLite3 v5.41.2 – 22 Jul 2022

- Fix a performance regression in the query planner associated with rearranging the order of FROM clause terms in the presences of a LEFT JOIN.
- Apply fixes for CVE-2022-35737 and other minor problems discovered by internal testing.

## DISQLite3 v5.41.1 – 14 Jul 2022

- Fix an incorrect result from a query that uses a view that contains a compound SELECT in which only one arm contains a RIGHT JOIN and where the view is not the first FROM clause term of the query that contains the view.
- Fix a long-standing problem with ALTER TABLE RENAME that can only arise if the sqlite3_limit(SQLITE_LIMIT_SQL_LENGTH) is set to a very small value.
- Enhance the sqlite_stmt virtual table so that it buffers all of its output.

## DISQLite3 v5.41.0 – 6 Jul 2022

- Add support for RIGHT and FULL OUTER JOIN.
- Add new binary comparison operators IS NOT DISTINCT FROM and IS DISTINCT FROM that are equivalent to IS and IS NOT, respective, for compatibility with PostgreSQL and SQL standards.
- Add a new return code (value “3”) from the`sqlite3_vtab_distinct` interface that indicates a query that has both DISTINCT and ORDER BY clauses.
- Added the`sqlite3_db_name` interface.
- Defer materializing views until the materialization is actually needed, thus avoiding unnecessary work if the materialization turns out to never be used.
- The HAVING clause of a SELECT statement is now allowed on any aggregate query, even queries that do not have a GROUP BY clause.
- Many microoptimizations collectively reduce CPU cycles by about 2.3%.

## DISQLite3 v5.40.2 – 5 Apr 2022

- Fix a problem with the new Bloom filter optimization that might cause an incorrect answer when doing a LEFT JOIN with a WHERE clause constraint that says that one of the columns on the right table of the LEFT JOIN is NULL.
- Other minor patches.

## DISQLite3 v5.40.1 – 14 Mar 2022

- Fix problems with the new Bloom filter optimization that might cause some obscure queries to get an incorrect answer.
- Fix the localtime modifier of the date and time functions so that it preserves fractional seconds.
- Fix the sqlite_offset SQL function so that it works correctly even in corner cases such as when the argument is a virtual column or the column of a view.
- Fix row value IN operator constraints on virtual tables so that they work correctly even if the virtual table implementation relies on bytecode to filter rows that do not satisfy the constraint.

## DISQLite3 v5.40.0 – 25 Feb 2022

- Added the LSM database and the LSM virtual table extension.
- JSON functions are now built-ins; JSON is on by default. It is no longer necessary to call`sqlite3_json_init()` , that function is now deprecated and does nothing.
- Added the`->` and`->>` operators for easier processing of JSON. The new operators are compatible with MySQL and PostgreSQL.
- Enhancements to date and time functions:
  - Added the unixepoch() function.
  - Added the auto modifier and the julianday modifier.
- Rename the printf() SQL function to format for better compatibility. The original printf() name is retained as an alias for backwards compatibility.
- Added the`sqlite3_error_offset` interface, which can sometimes help to localize an SQL error to a specific character in the input SQL text, so that applications can provide better error messages.
- Enhanced the interface to virtual tables as follows:
  - Added the`sqlite3_vtab_distinct` interface.
  - Added the`sqlite3_vtab_rhs_value` interface.
  - Added new operator types SQLITE_INDEX_CONSTRAINT_LIMIT and SQLITE_INDEX_CONSTRAINT_OFFSET.
  - Added the`sqlite3_vtab_in` interface (and related) to enable a virtual table to process IN operator constraints all at once, rather than processing each value of the right-hand side of the IN operator separately.
- Query planner enhancements:
  - Use a Bloom filter to speed up large analytic queries.
  - Use a balanced merge tree to evaluate UNION or UNION ALL compound SELECT statements that have an ORDER BY clause.
- The ALTER TABLE statement is changed to silently ignores entries in the sqlite_schema table that do not parse when PRAGMA writable_schema=ON.

## DISQLite3 v5.39.2 – 7 Jan 2022

- Fix a bug that can cause database corruption if a SAVEPOINT is rolled back while in PRAGMA temp_store=MEMORY mode, and other changes are made, and then the outer transaction commits.
- Fix a long-standing problem with ON DELETE CASCADE and ON UPDATE CASCADE in which a cache of the bytecode used to implement the cascading change was not being reset following a local DDL change.

## DISQLite3 v5.39.1 – 3 Jan 2022

- Fix a bug in UPSERT that can cause incorrect byte-code to be generated for some obscure but valid SQL, possibly resulting in a`nil` -pointer dereference.
- Fix an out of bounds read that can occur in FTS5 when reading corrupt database files.

## DISQLite3 v5.39.0 – 6 Dec 2021

- STRICT tables provide a prescriptive style of data type management, for developers who prefer that kind of thing.
- When adding columns that contain a CHECK constraint or a generated column containing a NOT NULL constraint, the ALTER TABLE ADD COLUMN now checks new constraints against preexisting rows in the database and will only proceed if no constraints are violated.
- Added the PRAGMA table_list statement.
- Added the`sqlite3_autovacuum_pages` interface.
- The`sqlite3_deserialize` does not and has never worked for the TEMP database. That limitation is now noted in the documentation.
- The query planner now omits ORDER BY clauses on subqueries and views if removing those clauses does not change the semantics of the query.
- The generate_series table-valued function extension is modified so that the first parameter (“START”) is now required. This is done as a way to demonstrate how to write table-valued functions with required parameters.
- Added new`sqlite3_changes64` and`sqlite3_total_changes64` interfaces.
- Added the SQLITE_OPEN_EXRESCODE flag option to`sqlite3_open_v2` .
- Use less memory to hold the database schema.

## DISQLite3 5.38.0 – 16 Sep 2021

- Support Delphi 11 Alexandria Win32 and Win64.

## DISQLite3 5.37.0 – 24 Jun 2021

Incremental feature and performance improvements:

- Improvement to the EXPLAIN QUERY PLAN output to make it easier to understand.
- Byte-order marks at the start of a token are skipped as if they were whitespace.
- An error is raised on any attempt to access the rowid of a VIEW or subquery. Formerly, the rowid of a VIEW would be indeterminate and often would be NULL.
- The “memdb” VFS now allows the same in-memory database to be shared among multiple database connections in the same process as long as the database name begins with “/”.
- Back out the EXISTS-to-IN optimization as it was found to slow down queries more often than speed them up.
- Improve the constant-propagation optimization so that it works on non-join queries.

New APIs:

- `sqlite3_preupdate_blobwrite` : Return the index of the column being written in a preupdate callback.
- `sqlite3session_object_config` : Configure a session object.
- `sqlite3session_changeset_size` : Return an upper-limit for the size of a session changeset.

## DISQLite3 5.36.5 – 20 Apr 2021

- Fix defects in the new ALTER TABLE DROP COLUMN feature that could corrupt the database file.
- Fix an obscure query optimizer problem that might cause an incorrect query result.

## DISQLite3 5.36.4 – 7 Apr 2021

Yet another patch release to fix obscure problems in features associated with SQLite 3.35.0:

- Fix a segmentation fault if the new RETURNING syntax contains “table.*”.
- Fix a defect in the query planner optimization attempting to process EXISTS operators in the WHERE clause as if they were IN operators.
- Fix the new RETURNING feature so that it raises an error if one of the terms in the RETURNING clause references a unknown table, instead of silently ignoring that error.

## DISQLite3 5.36.3 – 27 Mar 2021

Fix a handful of problems discovered in prior releases:

- Enhance the OP_OpenDup opcode of the bytecode engine so that it works even if the cursor being duplicated itself came from OP_OpenDup. This problem only came to light due to the recent MATERIALIZED hint enhancement.
- When materializing correlated common table expressions, do so separately for each use case, as that is required for correctness. This fixes a problem that was introduced by the MATERIALIZED hint enhancement.
- Improvements to error messages generated by faulty common table expressions.
- Fix to the SELECT statement syntax diagram so that the FROM clause syntax is shown correctly.
- Improvements the xBestIndex method in the implementation of the wholenumber virtual table extension so that it does a better job of convincing the query planner to avoid trying to materialize a table with an infinite number of rows.

## DISQLite3 5.36.2 – 18 Mar 2021

This is a patch release to fix problems that were discovered shortly after the previous release:

- Fix a problem with growing databases in the Append VFS extension.
- Ensure that date/time functions with no arguments (which generate responses that depend on the current time) are treated as non-deterministic functions.
- Limit the new UNION ALL optimization so that it does not try to make too many new subqueries.

## DISQLite3 5.36.1 – 16 Mar 2021

- Fix a bug in the new ALTER TABLE DROP COLUMN feature when used on columns that are indexed and that are quoted in the index definition.

## DISQLite3 5.36.0 – 13 Mar 2021

- Added built-in SQL math functions.
- Added support for ALTER TABLE DROP COLUMN.
- Generalize UPSERT:
  - Allow multiple ON CONFLICT clauses that are evaluated in order,
  - The final ON CONFLICT clause may omit the conflict target and yet still use DO UPDATE.
- Add support for the RETURNING clause on DELETE, INSERT, and UPDATE statements.
- Use less memory when running VACUUM on databases containing very large TEXT or BLOB values. It is no longer necessary to hold the entire TEXT or BLOB in memory all at once.
- Add support for the MATERIALIZED and NOT MATERIALIZED hints when specifying common table expressions. The default behavior was formerly NOT MATERIALIZED, but is now changed to MATERIALIZED for CTEs that are used more than once.
- The SQLITE_DBCONFIG_ENABLE_TRIGGER and SQLITE_DBCONFIG_ENABLE_VIEW settings are modified so that they only control triggers and views in the main database schema or in attached database schemas and not in the TEMP schema. TEMP triggers and views are always allowed.
- Query planner/optimizer improvements:
  - Enhancements to the min/max optimization so that it works better with the IN operator and the OP_SeekScan optimization of the previous release.
  - Attempt to process EXISTS operators in the WHERE clause as if they were IN operators, in cases where this is a valid transformation and seems likely to improve performance.
  - Allow UNION ALL sub-queries to be flattened even if the parent query is a join.
  - Use an index, if appropriate, on IS NOT NULL expressions in the WHERE clause, even if STAT4 is disabled.
  - Expressions of the form “x IS NULL” or “x IS NOT NULL” might be converted to simply FALSE or TRUE, if “x” is a column that has a “NOT NULL” constraint and is not involved in an outer join.
  - Avoid checking foreign key constraints on an UPDATE statement if the UPDATE does not modify any columns associated with the foreign key.
  - Allow WHERE terms to be pushed down into sub-queries that contain window functions, as long as the WHERE term is made up of entirely of constants and copies of expressions found in the PARTITION BY clauses of all window functions in the sub-query.

**Bug fixes:**

- Fix a bug in the IN-operator optimization that can cause an incorrect answer.
- Fix incorrect answers from the LIKE operator if the pattern ends with “%” and there is an “ESCAPE '_'” clause.
- Fix a long-standing bug with Delphi Win64 which prevented SQL parameter extension in trace and`sqlite3_expanded_sql` .

## DISQLite3 5.35.1 – 26 Jan 2021

- Fix a potential use-after-free bug when processing a a subquery with both a correlated WHERE clause and a “HAVING 0” clause and where the parent query is an aggregate.
- Fix minor problems in extensions.

## DISQLite3 5.35.0 – 3 Dec 2020

- New`TDISQLite3UniDirQuery.IntegerPrimaryKeyField` option to work around the 32-bit limitation of Delphi's ftAutoInc DataType and its corresponding TAutoIncField. Data loss may occur if an INTEGER PRIMARY KEY column uses the full 64-bit range available to SQLite3 integers.
- Added the`sqlite3_txn_state` interface for reporting on the current transaction state of the database connection.
- Enhance recursive common table expressions to support two or more recursive terms as is done by SQL Server, since this helps make queries against graphs easier to write and faster to execute.
- Improved error messages on CHECK constraint failures.
- Query planner improvements:
  - Improved estimates for the cost of running a DISTINCT operator.
  - When doing an UPDATE or DELETE using a multi-column index where only a few of the earlier columns of the index are useful for the index lookup, postpone doing the main table seek until after all WHERE clause constraints have been evaluated, in case those constraints can be covered by unused later terms of the index, thus avoiding unnecessary main table seeks.
  - The new OP_SeekScan opcode is used to improve performance of multi-column index look-ups when later columns are constrained by an IN operator.
- The BEGIN IMMEDIATE and BEGIN EXCLUSIVE commands now work even if one or more attached database files are read-only.
- Enhanced FTS5 to support trigram indexes.
- Improved performance of WAL mode locking primitives in cases where there are hundreds of connections all accessing the same database file at once.
- Enhanced the carray() table-valued function to include a single-argument form that is bound using the auxiliary`sqlite3_carray_bind` interface.
- The substr() SQL function can now also be called “substring()” for compatibility with SQL Server.
- The syntax diagrams are now rendered as SVG for improved legibility and ease of maintenance. SVG requires at least Internet Explorer 9. If you experience display problems, you may need to update.

## DISQLite3 5.34.0 – 19 Aug 2020

- Support for UPDATE FROM following the PostgreSQL syntax.
- Increase the maximum size of database files to 281 TB.
- Extended the PRAGMA integrity_check statement so that it can optionally be limited to verifying just a single table and its indexes, rather than the entire database file.
- Added the decimal extension for doing arbitrary-precision decimal arithmetic.
- Enhancements to the ieee754 extension for working with IEEE 754 binary64 numbers.
- Query planner improvements:
  1. Add the ability to find a full-index-scan query plan for queries using INDEXED BY which previously would fail with “no query solution”.
  2. Do a better job of detecting missing, incomplete, and/or dodgy sqlite_stat1 data and generates good query plans in spite of the misinformation.
  3. Improved performance of queries like “SELECT min(x) FROM t WHERE y IN (?,?,?)” assuming an index on t(x,y).
- In WAL mode, if a writer crashes and leaves the shm file in an inconsistent state, subsequent transactions are now able to recover the shm file even if there are active read transactions. Before this enhancement, shm file recovery that scenario would result in an`SQLITE_PROTOCOL` error.

## DISQLite3 5.33.2 – 26 Jun 2020

- Various minor bug fixes.

## DISQLite3 5.33.1 – 8 Jun 2020

- Fix a long-standing bug in the byte-code engine that can cause a COMMIT command report as success when in fact it failed to commit. The problem was present since SQLite 3.17.0 (2017-02-13). If you were to retry a COMMIT command over and over after it returned`SQLITE_BUSY` , it might have eventually reported success, even though it was still blocked.
- Updates to demo projects.

## DISQLite3 5.33.0 – 5 Jun 2020

- Support Delphi 10.4 Sydney Win32 and Win64.

## DISQLite3 5.32.0 – 27 May 2020

- Added support for approximate ANALYZE using the PRAGMA analysis_limit command.
- Added the bytecode virtual table.
- Add the checksum VFS shim to the set of run-time loadable extensions included in the source tree.
- Added the iif() SQL function.
- INSERT and UPDATE statements now always apply column affinity before computing CHECK constraints. This bug fix could, in theory, cause problems for legacy databases with unorthodox CHECK constraints the require the input type for an INSERT is different from the declared column type.
- Added the`sqlite3_create_filename` ,`sqlite3_free_filename` , and`sqlite3_database_file_object` interfaces to better support of VFS shim implementations.
- Increase the default upper bound on the number of parameters from 999 to 32766.
- Added code for the UINT collating sequence as an optional loadable extension.
- The ESCAPE clause of a LIKE operator now overrides wildcard characters, so that the behavior now matches what PostgreSQL does.
- Fix two long-standing bugs that allow malicious SQL statements to crash the process that is running SQLite.

## DISQLite3 5.31.1 – 5 Feb 2020

- DISQLite3's UTF-16 to UTF-8 conversions (and vice versa) now convert Unicode Code Points from $100000 to $10FFFF (Supplemental Planes). Previously, conversion was limited to the Basic Multilingual Plane (BMP) from $0000 to $FFFF. This primarily improves`sqlite3_decode_utf8` and`sqlite3_encode_utf8` , but also internal conversions, i.e. in`TDISQLite3UniDirQuery` , which use these functions.
- Follow up renaming parameter names which caused a compile error in Delphi 2007 and before.

## DISQLite3 5.31.0 – 31 Jan 2020

- Add support for generated columns.
- Add the`sqlite3_hard_heap_limit64` interface and the corresponding PRAGMA hard_heap_limit command.
- Enhance the function_list pragma to show the number of arguments on each function, the type of function (scalar, aggregate, window), and the function property flags`SQLITE_DETERMINISTIC` ,`SQLITE_DIRECTONLY` ,`SQLITE_INNOCUOUS` , and/or`SQLITE_SUBTYPE` .
- Add the aggregated mode feature to the DBSTAT virtual table.
- Add the`SQLITE_OPEN_NOFOLLOW` option to`sqlite3_open_v2` that prevents SQLite from opening symbolic links.
- Added the “#-N” array notation for JSON function path arguments.
- Added the`SQLITE_DBCONFIG_TRUSTED_SCHEMA` connection setting which is also controllable via the new trusted_schema pragma.
- Added APIs`sqlite3_filename_database` ,`sqlite3_filename_journal` , and`sqlite3_filename_wal` which are useful for specialized extensions.
- Add the`sqlite3_uri_key` interface.
- Upgraded the`sqlite3_uri_parameter` function so that it works with the rollback journal or WAL filename in addition to the database filename.
- Provide the ability to tag application-defined SQL functions with new properties`SQLITE_INNOCUOUS` or`SQLITE_DIRECTONLY` .
- Add new verbs to`sqlite3_vtab_config` so that the xConnect method of virtual tables can declare the virtual table as`SQLITE_VTAB_INNOCUOUS` or`SQLITE_VTAB_DIRECTONLY` .
- Faster response to`sqlite3_interrupt` .
- Added the UUID extension module implementing functions for processing RFC-4122 UUIDs.
- Added the urifuncs SQL function extension.
- The lookaside memory allocator is enhanced to support two separate memory pools with different sized allocations in each pool. This allows more memory allocations to be covered by lookaside while at the same time reducing the heap memory usage to 48KB per connection, down from 120KB.
- The legacy_file_format pragma is deactivated. It is now a no-op. In its place, the`SQLITE_DBCONFIG_LEGACY_FILE_FORMAT` option to`sqlite3_db_config` is provided. The legacy_file_format pragma is deactivated because (1) it is rarely useful and (2) it is incompatible with VACUUM in schemas that have tables with both generated columns and descending indexes.
- `TDISQLite3UniDirQuery` :`ftMemo` and`ftWideMemo` fields return string representations of`SQLITE_INTEGER` and`SQLITE_FLOAT` content.
- For unknown reasons, the following Delphi compilers crash when creating`.hpp` and`.obj` with the`-JPHNE` option:
  - Win32: Delphi 2005. No error message.
  - Win64: Delphi XE3, Delphi XE4, Delphi XE4, Delphi XE5, Delphi XE6, Delphi XE7, Delphi XE8, Delphi 10.0 Seattle, Delphi 10.1 Berlin. Error message: “*../../../shared/util-common/elfout.c:300: Assertion `(ef->sht[shndx].sh_type == SHT_NOBITS) && (ftell (ef->fp) == (long)ef->sht[shndx].sh_offset)' failed.* ” followed by “*Abnormal program termination* ”.
  - `.hpp` and`.obj` files are not included for these compilers.

## DISQLite3 5.30.0 – 15 Oct 2019

- Add support for the FILTER clause on aggregate functions.
- Add support for the NULLS FIRST and NULLS LAST syntax in ORDER BY clauses.
- The index_info and index_xinfo pragmas are enhanced to provide information about the on-disk representation of WITHOUT ROWID tables.
- Add the`sqlite3_drop_modules` interface, allowing applications to disable automatically loaded virtual tables that they do not need.
- Enhance the RBU extension to support indexes on expressions.
- Change the schema parser so that it will error out if any of the type, name, and tbl_name columns of the sqlite_schema table have been corrupted and the database connection is not in writable_schema mode.
- Add the`SQLITE_DBCONFIG_ENABLE_VIEW` option for`sqlite3_db_config` .
- Added the`SQLITE_DIRECTONLY` flag for application-defined SQL functions to prevent those functions from being used inside triggers and views.

## DISQLite3 5.29.0 – 8 Oct 2019

- Added the`SQLITE_DBCONFIG_DQS_DML` and`SQLITE_DBCONFIG_DQS_DDL` actions to`sqlite3_db_config` for activating and deactivating the double-quoted string literal misfeature. Both default to “on” for legacy compatibility, but developers are encouraged to turn them “off”.
- Improvements to the query planner:
  - Improved optimization of AND and OR operators when one or the other operand is a constant.
  - Enhancements to the LIKE optimization for cases when the left-hand side column has numeric affinity.
- Added the sqlite_dbdata virtual table for extracting raw low-level content from an SQLite database, even a database that is corrupt.

## DISQLite3 5.28.0 – 27 May 2019

- Enhanced window functions:
  - Add support for the EXCLUDE clause.
  - Add support for window chaining.
  - Add support for GROUPS frames.
  - Add support for “<expr> PRECEDING” and “<expr> FOLLOWING” boundaries in RANGE frames.
- Added the new`sqlite3_stmt_isexplain` interface for determining whether or not a prepared statement is an EXPLAIN.
- Enhanced VACUUM INTO so that it works for read-only databases.
- New query optimizations:
  - Enable the LIKE optimization for cases when the ESCAPE keyword is present and PRAGMA case_sensitive_like is on.
  - In queries that are driven by a partial index, avoid unnecessary tests of the constraint named in the WHERE clause of the partial index, since we know that constraint must always be true.
- Added the fossildelta extension that can create, apply, and deconstruct the Fossil DVCS file delta format that is used by the RBU extension.
- Added the`SQLITE_DBCONFIG_WRITABLE_SCHEMA` verb for the`sqlite3_db_config` interface, that does the same work as PRAGMA writable_schema without using the SQL parser.
- Added the`sqlite3_value_frombind` API for determining if the argument to an SQL function is from a bound parameter.
- Added the`blobio` extension.
- Security and compatibilities enhancements to fts3_tokenizer():
  - The fts3_tokenizer() function always returns NULL unless either the legacy application-defined FTS3 tokenizers interface are enabled using the`(` SQLITE_DBCONFIG_ENABLE_FTS3_TOKENIZER`)` setting, or unless the first argument to fts3_tokenizer() is a bound parameter.
  - The two-argument version of fts3_tokenizer() accepts a pointer to the tokenizer method object even without the`(` SQLITE_DBCONFIG_ENABLE_FTS3_TOKENIZER`)` setting if the second argument is a bound parameter
- Improved robustness against corrupt database files.
- Miscellaneous performance enhancements

## DISQLite3 5.27.0 – 1 Mar 2019

- Added the VACUUM INTO command.
- Added the prefixes extension.
- Issue an SQLITE_WARNING message on the error log if a double-quoted string literal is used.
- The`sqlite3_normalized_sql` interface works on any prepared statementcreated using`sqlite3_prepare_v2` or`sqlite3_prepare_v3` . It is no longer necessary to use`sqlite3_prepare_v3` with`SQLITE_PREPARE_NORMALIZE` in order to use`sqlite3_normalized_sql` .
- Added the remove_diacritics=2 option to FTS3 and FTS5.
- Added the`SQLITE_PREPARE_NO_VTAB` option to`sqlite3_prepare_v3` . Use that option to prevent circular references to shadow tables from causing resource leaks.
- Enhancements to the`sqlite3_deserialize` interface:
  - Add the`SQLITE_FCNTL_SIZE_LIMIT` file-control for setting an upper bound on the size of the in-memory database created by sqlite3_deserialize. The default upper bound is 1GiB, or whatever alternative value is specified by`sqlite3_config` (`SQLITE_CONFIG_MEMDB_MAXSIZE` ).
  - Honor the`SQLITE_DESERIALIZE_READONLY` flag, which was previously described in the documentation, but was previously a no-op.
- Increased robustness against malicious SQL that is run against a maliciously corrupted database.

**Bug fixes:**

- Do not use a partial index to do a table scan on an IN operator.
- Fix the query flattener so that it works on queries that contain subqueries that use window functions.
- Ensure that ALTER TABLE modifies table and column names embedded in WITH clauses that are part of views and triggers.
- Fix a parser bug that prevented the use of parentheses around table-valued functions.
- Fix a problem with the OR optimization on indexes on expressions.
- Fix a problem with the LEFT JOIN strength reduction optimization in which the optimization was being applied inappropriately due to an IS NOT NULL operator.
- Fix the REPLACE command so that it is no longer able to sneak a NULL value into a NOT NULL column even if the NOT NULL column has a default value of NULL.
- Fix a problem with the use of window functions used within correlated subqueries.
- Fix the ALTER TABLE RENAME COLUMN command so that it works for tables that have redundant UNIQUE constraints.
- Fix a bug that caused zeroblob values to be truncated when inserted into a table that uses an expression index.
- Fix a bug in the query optimizer: an adverse interaction between the OR optimization and the optimization that tries to use values read directly from an expression index instead of recomputing the expression.
- Fix a bug causing a crash when a window function is misused.

## DISQLite3 5.26.1 – 4 Jan 2019

- Fix`sqlite3_rekey` and`sqlite3_rekey_v2` encryption errors with databases greater than about 4 MB.

## DISQLite3 5.26.0 – 24 Dec 2018

- Support Delphi 10.3 Rio Win32 and Win64.

**Enhancements:**

- Add support for window functions.
- Enhancements the ALTER TABLE command:
  - Add support for renaming columns within a table using ALTER TABLE*table* RENAME COLUMN*oldname* TO*newname* .
- Add support for PostgreSQL-style UPSERT.
  - Fix table rename feature so that it also updates references to the renamed table in triggers and views.
- Query optimizer improvements:
  - Avoid unnecessary loads of columns in an aggregate query that are not within an aggregate function and that are not part of the GROUP BY clause.
  - The IN-early-out optimization: When doing a look-up on a multi-column index and an IN operator is used on a column other than the left-most column, then if no rows match against the first IN value, check to make sure there exist rows that match the columns to the right before continuing with the next IN value.
  - Use the transitive property to try to propagate constant values within the WHERE clause. For example, convert “a=99 AND b=a” into “a=99 AND b=99”.
- Enhance the PRAGMA integrity_check command for improved detection of problems on the page freelist.
- Allow the xBestIndex method of virtual table implementations to return`SQLITE_CONSTRAINT` to indicate that the proposed query plan is unusable and should not be given further consideration.
- Added the`SQLITE_DBCONFIG_DEFENSIVE` option which disables the ability to create corrupt database files using ordinary SQL.
- Added support for read-only shadow tables when the`SQLITE_DBCONFIG_DEFENSIVE` option is enabled.
- Added the PRAGMA legacy_alter_table command, which if enabled causes the ALTER TABLE command to behave like older version of SQLite (prior to version 3.25.0) for compatibility.
- Added PRAGMA table_xinfo that works just like PRAGMA table_info except that it also shows hidden columns in virtual tables.
- Added the EXPLAIN virtual table as a run-time loadable extension.
- Add a limit counter to the query planner to prevent excessive`sqlite3_prepare` times for certain pathological SQL inputs.
- Added support for the`sqlite3_normalized_sql` interface.
- Enhanced triggers so that they can use table-valued functions that exist in schemas other than the schema where the trigger is defined.
- Added the`SQLITE_FCNTL_DATA_VERSION` file-control.
- Added the Geopoly module.
- Add support for auxiliary columns in r-tree tables.
- Add APIs for discovering SQL keywords used by SQLite:`sqlite3_keyword_count` ,`sqlite3_keyword_name` , and`sqlite3_keyword_check` .
- Add APIs for dynamic strings based on the`sqlite3_str` object.
- Enhance ALTER TABLE so that it recognizes “true” and “false” as valid arguments to DEFAULT.
- Add the sorter-reference optimization as a compile-time option.
- Improve the format of the EXPLAIN QUERY PLAN raw output, so that it gives better information about the query plan and about the relationships between the various components of the plan.
- Added the`SQLITE_DBCONFIG_RESET_DATABASE` option to the`sqlite3_db_config` API.
- Enhancements to the session extension:
  - Added the`SQLITE_CHANGESETAPPLY_INVERT` flag
  - Added the`sqlite3changeset_start_v2` interface and the`SQLITE_CHANGESETSTART_INVERT` flag.

**Performance:**

- Optimization: When doing an UPDATE on a table with indexes on expressions, do not update the expression indexes if they do not refer to any of the columns of the table being updated.
- UPDATE avoids unnecessary low-level disk writes when the contents of the database file do not actually change. For example, “UPDATE t1 SET x=25 WHERE y=?” generates no extra disk I/O if the value in column x is already 25. Similarly, when doing UPDATE on records that span multiple pages, only the subset of pages that actually change are written to disk. This is a low-level performance optimization only and does not affect the behavior of TRIGGERs or other higher level SQL structures.
- Queries that use ORDER BY and LIMIT now try to avoid computing rows that cannot possibly come in under the LIMIT. This can greatly improve performance of ORDER BY LIMIT queries, especially when the LIMIT is small relative to the number of unrestricted output rows.
- The OR optimization is allowed to proceed even if the OR expression has also been converted into an IN expression. Uses of the OR optimization are now also more clearly shown in the EXPLAIN QUERY PLAN output.
- The query planner is more aggressive about using automatic indexes for views and subqueries for which it is not possible to create a persistent index.
- Make use of the one-pass UPDATE and DELETE query plans in the R-Tree extension where appropriate.
- Performance improvements in the parser.

**Bug fixes:**

- The ORDER BY LIMIT optimization might have caused an infinite loop in the byte code of the prepared statement under very obscure circumstances, due to a confluence of minor defects in the query optimizer.
- On an UPSERT when the order of constraint checks is rearranged, ensure that the affinity transformations on the inserted content occur before any of the constraint checks.
- The LIKE optimization was generating incorrect byte-code and hence getting the wrong answer if the left-hand operand has numeric affinity and the right-hand-side pattern is '/%' or if the pattern begins with the ESCAPE character.
- For the right-hand table of a LEFT JOIN, compute the values of expressions directly rather than loading precomputed values out of an expression index as the expression index might not contain the correct value.
- Do not attempt to use terms from the WHERE clause to enable indexed lookup of the right-hand table of a LEFT JOIN.
- Fix a memory leak that can occur following a failure to open error in the CSV virtual table.
- Fix a long-standing problem wherein a corrupt schema on the sqlite_sequence table used by AUTOINCREMENT can lead to a crash.
- Fix the json_each() function so that it returns valid results on its “fullkey” column when the input is a simple value rather than an array or object.
- Fix a memory leak when reading`TDISQLite3UniDirQuery` string fields.

## DISQLite3 5.24.0 – 18 Apr 2018

- Performance enhancements in the parser.
- Add the`sqlite3_serialize` and`sqlite3_deserialize` interfaces.
- Recognize TRUE and FALSE as constants. (For compatibility, if there exist columns named “true” or “false”, then the identifiers refer to the columns rather than Boolean constants.)
- Support operators IS TRUE, IS FALSE, IS NOT TRUE, and IS NOT FALSE.
- Added the`SQLITE_DBSTATUS_CACHE_SPILL` option to`sqlite3_db_status` for reporting the number of cache spills that have occurred.
- The “alternate-form-2” flag (“!”) on the built-in printf implementation now causes string substitutions to measure the width and precision in characters instead of bytes.
- If the xColumn method in a virtual table implementation returns an error message using`sqlite3_result_error` then give that error message preference over internally-generated messages.
- Add support for INSERT OR REPLACE, INSERT OR IGNORE, and UPDATE OR REPLACE in the Zipfile virtual table.
- Enhance the sqlite3changeset_apply interface so that it is hardened against attacks from deliberately corrupted changeset objects.
- Added the`sqlite3_normalize` extension function.
- Query optimizer enhancements:
  - Improve the omit-left-join optimization so that it works in cases where the right-hand table is UNIQUE but not necessarily NOT NULL.
  - Improve the push-down optimization so that it works for many LEFT JOINs.
  - Avoid unnecessary writes to the sqlite_sequence table when an AUTOINCREMENT table is updated with an rowid that is less than the maximum.
  - Add the LEFT JOIN strength reduction optimization that converts a LEFT JOIN into an ordinary JOIN if there exist terms in the WHERE clause that would prevent the extra all-NULL row of the LEFT JOIN from appearing in the output set.
- Bug fixes:
  - Fix two problems in the new LEFT JOIN strength reduction optimization.
  - Fix misbehavior of the FTS5 xBestIndex method.
  - Fix a harmless reference to an uninitialized virtual machine register.
  - Fix the eval() SQL function extension so that it works with PRAGMA empty_result_callbacks=ON.
  - Fix the generate_series virtual table so that it correctly returns no rows if any of its constraints are NULL.
  - Fix the parser to accept valid row value syntax.
  - Fix the query planner so that it takes into account dependencies in the arguments to table-valued functions in subexpressions in the WHERE clause.
  - Fix incorrect result with complex OR-connected WHERE and STAT4.
  - Fix potential corruption in indexes on expressions due to automatic datatype conversions.
  - Assertion fault in FTS4.
  - Incorrect result on the less-than operator in row values.
  - Always interpret non-zero floating-point values as TRUE, even if the integer part is zero.
  - Issue an error rather instead of an assertion-fault or null-pointer dereference when the sqlite_schema table is corrupted so that the sqlite_sequence table root page is really a btree-index page.
  - Fix the ANALYZE command so that it computes statistics on tables whose names begin with “sqlite”.
  - Fix a possible infinite loop on VACUUM for corrupt database files.
  - Disallow parameters in the WITH clause of triggers and views.
  - Fix a potential memory leak in row value processing.
  - Improve the performance of the replace() SQL function for cases where there are many substitutions on megabyte-sized strings.
  - Provide an appropriate error message when the sqlite_schema table contains a CREATE TABLE AS statement. Formerly this caused either an assertion fault or null pointer dereference.
  - Fix a problem with using the LIKE optimization on an INTEGER PRIMARY KEY.

## DISQLite3 5.23.0 – 26 Jan 2018

- The output of`sqlite3_trace_v2` now shows each individual SQL statement run within a trigger.
- Add the ability to read from WAL mode databases even if the application lacks write permission on the database and its containing directory, as long as the -shm and -wal files exist in that directory.
- Added the rtreecheck scalar SQL function to the R-Tree extension.
- Added the`sqlite3_vtab_nochange` and`sqlite3_value_nochange` interfaces to help virtual table implementations optimize UPDATE operations.
- Added the`sqlite3_vtab_collation` interface.
- Added support for the “^” initial token syntax in FTS5.
- New extensions:
  - The Zipfile virtual table can read and write a ZIP Archive.
  - The sqlite_btreeinfo eponymous virtual table for introspecting and estimating the sizes of the btrees in a database.
  - The Append VFS is a VFS shim that allows an SQLite database to be appended to some other file. This allows (for example) a database to be appended to an executable that then opens and reads the database.
  - The`incremental_index_check` is a read-only eponymous-only virtual table that checkes indexes.
  - SQLAR compatible SQL compress and decompress functions.
- Query planner enhancements:
  - The optimization that uses an index to quickly compute an aggregate min() or max() is extended to work with indexes on expressions.
  - The decision of whether to implement a FROM-clause subquery as a co-routine or using query flattening now considers whether the result set of the outer query is “complex” (if it contains functions or expression subqueries). A complex result set biases the decision toward the use of co-routines.
  - The planner avoids query plans that use indexes with unknown collating functions.
  - The planner omits unused LEFT JOINs even if they are not the right-most joins of a query.
- Other performance optimizations:
  - A smaller and faster implementation of text to floating-point conversion subroutine: sqlite3AtoF().
  - Faster SQL parser.
- Provide the sqlite_offset(X) SQL function that returns the byte offset into the database file to the beginning of the record holding value X.
- Bug fixes:
  - Infinite loop on an UPDATE that uses an OR operator in the WHERE clause.
  - Incorrect query results when the skip-ahead-distinct optimization is used.
  - Incorrect query results on a join with a ORDER BY DESC.
  - Inconsistent result set column names between CREATE TABLE AS and a simple SELECT.
  - Assertion fault when doing REPLACE on an index on an expression.
  - Assertion fault when doing an IN operator on a constant index.
  - Correct`sqlite3_update_hook` callback`zDB` argument type.

## DISQLite3 5.22.0 – Nov 16 2017

- Allow ATTACH and DETACH commands to work inside of a transaction.
- Allow WITHOUT ROWID virtual tables to be writable if the PRIMARY KEY contains exactly one column.
- Improved de-quoting of column names for CREATE TABLE AS statements with an aggregate query on the right-hand side.
- Enhanced the LIKE optimization so that it works with an ESCAPE clause.
- Enhanced PRAGMA integrity_check and PRAGMA quick_check to detect obscure row corruption that they were formerly missing. Also update both pragmas so that they return error text rather than SQLITE_CORRUPT when encountering corruption in records.
- The query planner now prefers to implement FROM-clause subqueries using co-routines rather using the query flattener optimization. Support for the use of co-routines for subqueries may no longer be disabled.
- Pass information about !=, IS, IS NOT, NOT NULL, and IS NULL constraints into the xBestIndex method of virtual tables.
- Enhanced the CSV virtual table so that it accepts the last row of input if the final new-line character is missing.
- Remove the rarely-used “scratch” memory allocator. Replace it with the`SQLITE_CONFIG_SMALL_MALLOC` configuration setting that gives SQLite a hint that large memory allocations should be avoided when possible.
- Added the swarm virtual table to the existing union virtual table extension.
- Added the sqlite_dbpage virtual table for providing direct access to pages of the database file.
- Add a new type of fts5vocab virtual table – “instance” – that provides direct access to an FTS5 full-text index at the lowest possible level.
- Miscellaneous microoptimizations reduce CPU usage by about 2.1%.

**Bug fixes:**

- Fix an obscure memory leak in`sqlite3_result_pointer` .
- Avoid a possible use-after-free error by deferring schema resets until after the query planner has finished running.
- Only use indexes-on-expressions to optimize ORDER BY or GROUP BY if the COLLATE is correct.
- Fix an assertion fault that was coming up when the expression in an index-on-expressions is really a constant.
- Fix an assertion fault that could occur following PRAGMA reverse_unordered_selects.
- Fix a segfault that can occur for queries that use table-valued functions in an IN or EXISTS subquery.
- Fix a potential integer overflow problem when compiling a particular horrendous common table expression.
- Fix a potential out-of-bound read when querying a corrupt database file.

## DISQLite3 5.21 – Aug 10 2017

- Update the text of error messages returned by`sqlite3_errmsg` for some error codes.
- Add new pointer passing interfaces.
- Backwards-incompatible changes to some extensions in order to take advantage of the improved security offered by the new pointer passing interfaces:
  - Extending FTS5 → requires`sqlite3_bind_pointer` to find the fts5_api pointer.
  - carray(PTR,N) → requires`sqlite3_bind_pointer` to set the PTR parameter.
  - remember(V,PTR) → requires`sqlite3_bind_pointer` to set the PTR parameter.
- Added the SQLITE_STMT virtual table extension.
- Added the COMPLETION extension – designed to suggest tab-completions for interactive user interfaces. This is a work in progress. Expect further enhancements in future releases.
- Added the UNION virtual table extension.
- The built-in date and time functions have been enhanced so that they can be used in CHECK constraints, in indexes on expressions, and in the WHERE clauses of partial indexes, provided that they do not use the 'now', 'localtime', or'utc' keywords. Futher information.
- Added the`sqlite3_prepare_v3` and`sqlite3_prepare16_v3` interfaces with the extra “prepFlags” parameters.
- Provide the`SQLITE_PREPARE_PERSISTENT` flag for`sqlite3_prepare_v3` and use it to limit lookaside memory misuse by FTS3, FTS5, and the R-Tree extension.
- Added the PRAGMA secure_delete=FAST command. When secure_delete is set to FAST, old content is overwritten with zeros as long as that does not increase the amount of I/O. Deleted content might still persist on the free-page list but will be purged from all b-tree pages.
- Query planner enhancements:
  - When generating individual loops for each ORed term of an OR scan, move any constant WHERE expressions outside of the loop, as is done for top-level loops.
  - The query planner examines the values of bound parameters to help determine if a partial index is usable.
  - When deciding between two plans with the same estimated cost, bias the selection toward the one that does not use the sorter.
  - Evaluate WHERE clause constraints involving correlated subqueries last, in the hope that they never have be evaluated at all.
  - Do not use the flattening optimization for a sub-query on the RHS of a LEFT JOIN if that subquery reads data from a virtual table as doing so prevents the query planner from creating automatic indexes on the results of the sub-query, which can slow down the query.
- Add`SQLITE_STMTSTATUS_REPREPARE` ,`SQLITE_STMTSTATUS_RUN` , and`SQLITE_STMTSTATUS_MEMUSED` options for the`sqlite3_stmt_status` interface.
- Provide PRAGMA functions for PRAGMA integrity_check, PRAGMA quick_check, and PRAGMA foreign_key_check.
- The`SQLITE_DBCONFIG_ENABLE_QPSG` run-time option enables the query planner stability guarantee.
- Miscellaneous optimizations result in a 2% reduction in CPU cycles used.

**Bug Fixes:**

- Fix the behavior of`sqlite3_column_name` for queries that use the flattening optimization so that the result is consistent with other queries that do not use that optimization, and with PostgreSQL, MySQL, and SQLServer.
- Fix the query planner so that it knows not to use automatic indexes on the right table of LEFT JOIN if the WHERE clause uses the IS operator.
- Ensure that the query planner knows that any column of a flattened LEFT JOIN can be NULL even if that column is labeled with “NOT NULL”.
- Fix rare false-positives in PRAGMA integrity_check when run on a database connection with ATTACH|attached databases.
- Fix a bug that causes an assertion fault if certain dodgy CREATE TABLE declarations are used.

## DISQLite3 5.20.1 – Jun 14 2017

- Emergency patch release to fix a bug in the auto_vacuum logic that can lead to database corruption. Though the bug is obscure and rarely encountered, upgrading is recommended for all users, and especially for users who turn on auto_vacuum.

## DISQLite3 5.20.0 – May 30 2017

- The`SQLITE_READ` authorizer callback is invoked once with a column name that is an empty string for every table referenced in a query from which no columns are extracted.
- When using an index on an expression, try to use expression values already available in the index, rather than loading the original columns and recomputing the expression.
- Enhance the flattening optimization so that it is able to flatten views on the right-hand side of a LEFT JOIN.
- Use replace() instead of char() for escaping newline and carriage-return characters embedded in strings in the dbdump extension.
- Avoid unnecessary foreign key processing in UPDATE statements that do not touch the columns that are constrained by the foreign keys.
- On a DISTINCT query that uses an index, try to skip ahead to the next distinct entry using the index rather than stepping through rows, when an appropriate index is available.
- Avoid unnecessary invalidation of`sqlite3_blob` handles when making changes to unrelated tables.
- Transfer any terms of the HAVING clause that use only columns mentioned in the GROUP BY clause over to the WHERE clause for faster processing.
- Reuse the same materialization of a VIEW if that VIEW appears more than once in the same query.
- Enhance PRAGMA integrity_check so that it identifies tables that have two or more rows with the same rowid.
- Enhance the FTS5 query syntax so that column filters may be applied to arbitrary expressions.
- Enhance the json_extract() function to cache and reuse parses of JSON input text.
- Added the anycollseq loadable extension that allows a generic SQLite database connection to read a schema that contains unknown and/or application-specific collating sequences.

**Bug Fixes:**

- Fix a problem in REPLACE that can result in a corrupt database containing two or more rows with the same rowid.
- Fix a problem in PRAGMA integrity_check that was causing a subsequent VACUUM to behave suboptimally.
- Fix the PRAGMA foreign_key_check command so that it works correctly with foreign keys on WITHOUT ROWID tables.
- Disallow leading zeros in numeric constants in JSON.
- Disallow control characters inside of strings in JSON.
- Limit the depth of recursion for JSON objects and arrays in order to avoid excess stack usage in the recursive descent parser.
- Fix bugs in the LEFT JOIN flattening optimization.

## DISQLite3 5.19.0 – 3 Apr 2017

- Support Delphi 10.2 Tokyo Win32 and Win64.
- Added the PRAGMA optimize command.
- The SQLite version identifier returned by the sqlite_source_id() SQL function and the`sqlite3_sourceid` C API is now a 64-digit SHA3-256 hash instead of a 40-digit SHA1 hash.
- Added the json_patch() SQL function to the JSON1 extension.
- Enhance the LIKE optimization so that it works for arbitrary expressions on the left-hand side as long as the LIKE pattern on the right-hand side does not begin with a digit or minus sign.
- Added the`sqlite3_set_last_insert_rowid` interface and use the new interface in the FTS3, FTS4, and FTS5 extensions to ensure that the`sqlite3_last_insert_rowid` interface always returns reasonable values.
- Enhance PRAGMA integrity_check and PRAGMA quick_check so that they verify CHECK constraints.
- Enhance the query plans for joins to detect empty tables early and halt without doing unnecessary work. Enhance the`sqlite3_mprintf` family of interfaces and the printf SQL function to put comma separators at the thousands marks for integers, if the “,” format modifier is used in between the “%” and the “d” (example: “%,d”).
- Begin enforcing`SQLITE_LIMIT_VDBE_OP` . This can be used, for example, to prevent excessively large prepared statements in systems that accept SQL queries from untrusted users.
- Various performance improvements.
- Add the dbdump extension.

**Bug Fixes:**

- Ensure that indexed expressions with collating sequences are handled correctly.
- Fix a bug in the 'start of …' modifiers for the date and time functions.
- Fix a potential segfault in complex recursive triggers.
- In the RBU extension, add extra sync operations to avoid the possibility of corruption following a power failure.
- The`sqlite3_trace_v2` output for nested SQL statements should always begin with a “–” comment marker.

## DISQLite3 5.18.0 – 16 Feb 2017

- Better performance from the R-Tree extension.
  - Uses the`sqlite3_blob` key/value access object instead of SQL for pulling content out of R-Tree nodes.
  - Other miscellaneous enhancements such as loop unrolling.
- Increase the default lookaside size from 512,125 to 1200,100 as this provides better performance while only adding 56KB of extra memory per connection. Memory-sensitive applications can restore the old default at compile-time, start-time, or run-time.
- Reduce the default number of pages initially allocated for the pagecache from 100 to 20, for improved performance.
- Perform some UPDATE operations in a single pass instead of in two passes.
- Enhance the session extension to support WITHOUT ROWID tables.
- Fixed performance problems and potential stack overflows when creating views from multi-row VALUES clauses with hundreds of thousands of rows.
- Added the sha1 extension.
- Other performance improvements. Uses about 6.5% fewer CPU cycles.

**Bug Fixes:**

- Throw an error if the ON clause of a LEFT JOIN references tables to the right of the ON clause. This is the same behavior as PostgreSQL. Formerly, SQLite silently converted the LEFT JOIN into an INNER JOIN.
- Use the correct affinity for columns of automatic indexes.
- Ensure that the`sqlite3_blob_reopen` interface can correctly handle short rows.

## DISQLite3 5.17.1 – 7 Jan 2017

- Fix the REPLACE statement for WITHOUT ROWID tables that lack secondary indexes so that it works correctly with triggers and foreign keys. This was a new bug caused by performance optimizations added in DISQLite3 5.17.0.
- Fix the`sqlite3_value_text` interface so that it correctly translates content generated by zeroblob() into a string of all 0x00 characters. This is a long-standing issue discovered after the DISQLite3 5.17.0 release.
- Fix the bytecode generator to deal with a subquery in the FROM clause that is itself a UNION ALL where one side of the UNION ALL is a view that contains an ORDER BY. This is a long-standing issue that was discovered after the release of DISQLite3 5.17.0.
- Adjust the`sqlite3_column_count` API so it more often returns the same values for PRAGMA statements as it did in prior releases, to minimize disruption to applications that might be using that interface in unexpected ways.

## DISQLite3 5.17.0 – 4 Jan 2017

- Uses 9% fewer CPU cycles. (See the CPU performance measurement report for details on how this performance increase was computed.)
- Added experimental support for PRAGMA functions.
- Enhance the date and time functions so that the 'unixepoch' modifier works for the full span of supported dates.
- Changed the default configuration of the lookaside memory allocator from 500 slots of 128 bytes each into 125 slots of 512 bytes each.
- Enhanced the LIKE and GLOB matching algorithm to be faster for cases when the pattern contains multiple wildcards.
- Added the remember(V,PTR) SQL function as a loadable extension.

**Bug Fixes:**

- Fix a long-standing bug in the query planner that caused incorrect results on a LEFT JOIN where the left-hand table is a subquery and the join constraint is a bare column name coming from the left-hand subquery.
- Correctly handle the integer literal -0x8000000000000000 in the query planner.
- Fix a bug concerning the use of row values within triggers.

## DISQLite3 5.16.1 – 9 Dec 2016

- Bug fixes to the row value logic that was introduced in version 3.15.0.
- Fix a NULL pointer dereference in ATTACH/DETACH following a maliciously constructed syntax error.
- Fix a crash that can occur following an out-of-memory condition in the built-in instr() function.
- In the JSON extension, fix the JSON validator so that it correctly rejects invalid backslash escapes within strings.

## DISQLite3 5.16.0 – 3 Nov 2016

- Added the`SQLITE_DBCONFIG_NO_CKPT_ON_CLOSE` option to`sqlite3_db_config` .
- Enhanced “WHERE x NOT NULL” partial indexes so that they are usable if the “x” column appears in a LIKE or GLOB operator that has not been overloaded.
- Enhanced`sqlite3_interrupt` so that it interrupts checkpoint operations that are in process.
- Added`SQLITE_FCNTL_WIN32_GET_HANDLE` file control opcode.

**Bug Fixes:**

- Fix the VACUUM command so that it spills excess content to disk rather than holding everything in memory, and possible causing an out-of-memory error for larger database files.
- Fix a case where OR-connected terms in the ON clause of a LEFT JOIN might cause incorrect results.
- Fix a case where the use of row values in the ON clause of a LEFT JOIN might cause incorrect results.

## DISQLite3 5.15.0 – 21 Oct 2016

- Added support for row values.
- Allow deterministic SQL functions in the WHERE clause of a partial index.
- Added support for`SQLITE_DBCONFIG_MAINDBNAME` .
- Added the ability to VACUUM an ATTACH-ed database.
- Enhance the fts5vocab virtual table to handle “ORDER BY term” efficiently.
- Miscellaneous micro-optimizations reduce CPU usage by more than 7% on common workloads. Most optimization in this release has been on the front-end (`sqlite3_prepare_v2` ).

**Bug Fixes:**

- The multiply operator now correctly detects 64-bit integer overflow and promotes to floating point in all corner-cases.
- Correct handling of columns with redundant unique indexes when those columns are used on the LHS of an IN operator.
- Skip NULL entries on range queries in indexes on expressions.
- Ensure that the AUTOINCREMENT counters in the sqlite_sequence table are initialized doing “Xfer Optimization” on “INSERT … SELECT” statements.
- Make sure the ORDER BY LIMIT optimization works with IN operators on INTEGER PRIMARY KEYs.

## DISQLite3 5.14.0 – 20 Sep 2016

- Added support for WITHOUT ROWID virtual tables.
- Improved the query planner so that the OR optimization can be used on virtual tables even if one or more of the disjuncts use the LIKE, GLOB, REGEXP, MATCH operators.
- Added the CSV virtual table for reading RFC 4180 formatted comma-separated value files.
- Added the carray() table-valued function extension.
- Enabled persistent loadable extensions using the new`SQLITE_OK_LOAD_PERMANENTLY` return code from the extension entry point.
- Added the`SQLITE_DBSTATUS_CACHE_USED_SHARED` option to`sqlite3_db_status` .
- Add the vfsstat loadable extension – a VFS shim that measures I/O together with an eponymous virtual table that provides access to the measurements.
- Improved algorithm for running queries with both an ORDER BY and a LIMIT where only the inner-most loop naturally generates rows in the correct order.
- The PRAGMA compile_options command now attempts to show the version number of the compiler that generated the library.
- Enhance PRAGMA table_info so that it provides information about eponymous virtual tables.
- Added the “win32-none” VFS, analogous to the “unix-none” VFS, that works like the default “win32” VFS except that it ignores all file locks.
- The query planner uses a full scan of a partial index instead of a full scan of the main table, in cases where that makes sense.
- Allow table-valued functions to appear on the right-hand side of an IN operator.
- Added two new interfaces:`sqlite3_expanded_sql` and`sqlite3_trace_v2` .
- Added the json_quote() SQL function to the json1 extension.
- Disable the authorizer callback while reparsing the schema.
- A performance enhancement to the page-cache “truncate” operation reduces COMMIT time by dozens of milliseconds on systems with a large page cache.

 **Bug Fixes:** 

- Fix the ALTER TABLE command so that it does not corrupt descending indexes when adding a column to a legacy file format database.
- Fix a NULL-pointer dereference/crash that could occurs when a transitive WHERE clause references a non-existent collating sequence.
- Improved the cost estimation for an index scan which includes a WHERE clause that can be partially or fully evaluated using columns in the index and without having to do a table lookup.
- Fix the`sqlite3_trace_v2` interface so that it is disabled if either the callback or the mask arguments are zero, in accordance with the documentation.
- Correct affinity computations for a SELECT on the RHS of an IN operator.
- The ORDER BY LIMIT optimization is not valid unless the inner-most IN operator loop is actually used by the query plan.
- Fix an internal code generator problem that was causing some DELETE operations to no-op.

## DISQLite3 5.13.0 – 21 May 2016

- Postpone I/O associated with TEMP files for as long as possible, with the hope that the I/O can ultimately be avoided completely.
- Added the`(db,` SQLITE_DBCONFIG_ENABLE_LOAD_EXTENSION`)` interface which allows the`sqlite3_load_extension` C-API to be enabled while keeping the load_extension() SQL function disabled for security.

Bug fixes:

- Fix a problem with the multi-row one-pass DELETE optimization that was causing it to compute incorrect answers with a self-referential subquery in the WHERE clause.
- Fix a possible segfault with DELETE when table is a rowid table with an INTEGER PRIMARY KEY and the WHERE clause contains a OR and the table has one or more indexes that are able to trigger the OR optimization, but none of the indexes reference any table columns other than the INTEGER PRIMARY KEY.
- When checking for the WHERE-clause push-down optimization, verify that all terms of the compound inner SELECT are non-aggregate, not just the last term. Fix for ticket f7f8c97e97597.
- Fix a locking race condition in Windows that can occur when two or more processes attempt to recover the same hot journal at the same time.
- Fix a bug that can cause the DELETE operation to miss rows if PRAGMA reverse_unordered_selects is turned on.
- Fix a bug in the code generator that can cause incorrect results if two or more virtual tables are joined and the virtual table used in outer loop of the join has an IN operator constraint.
- Correctly interpret negative “PRAGMA cache_size” values when determining the cache size used for sorting large amounts of data.

## DISQLite3 5.12.0 – 7 May 2016

- Support Delphi 10.1 Berlin Win32 and Win64.

## DISQLite3 5.11.0 – 19 Apr 2016

- Add the Session Extension.
- Add the pre-update hook APIs.
- Enhanced the query planner so that the IS and IS NULL operators is able to drive indexes in a LEFT OUTER JOIN.
- Improved resistance against goofy query planner decisions caused by incomplete or incorrect modifications to the sqlite_stat1 table by the application.
- Fix a boundary condition error that can result in a crash during heavy SAVEPOINT usage.
- Fix views so that they inherit column datatypes from the table that they are defined against, when possible.
- Fix the query planner so that IS and IS NULL operators are able to drive an index on a LEFT OUTER JOIN.

## DISQLite3 5.10.0 – 30 Mar 2016

- Added the`sqlite3rbu_bp_progress` interface to the RBU extension.
- Added the`sqlite3_system_errno` interface.

## DISQLite3 5.9.0 – 18 Mar 2016

**Potentially Disruptive Change:**

- The default page size is increased from 1024 to 4096. The default cache size is changed from 2000 to -2000 so the same amount of cache memory is used by default. See the application note on the version 3.12.0 page size change for further information.

**Backwards Compatibility:**

- Because of continuing security concerns, the two-argument version of of the seldom-used and little-known fts3_tokenizer SQL function is disabled. The`SQLITE_DBCONFIG_ENABLE_FTS3_TOKENIZER` option to`sqlite3_db_config` allows to enable or disable it at run-time.

**New features:**

- The PRAGMA defer_foreign_keys=ON statement now also disables RESTRICT actions on foreign key.
- Added the FTS5 detail option.
- Added the “EXTRA” option to PRAGMA synchronous that does a sync of the containing directory when a rollback journal is unlinked in DELETE mode, for better durability.
- Add the`SQLITE_FCNTL_JOURNAL_POINTER` file control.
- Added support for LIKE, GLOB, and REGEXP operators on virtual tables.
- Added the`colUsed` field to`sqlite3_index_info` for use by the`sqlite3_module.xBestIndex` method.
- Enhance the PRAGMA cache_spill statement to accept a 32-bit integer parameter which is the threshold below which cache spilling is prohibited.
- Added the`sqlite3_db_cacheflush` interface.
- Added the`sqlite3_strlike` interface.
- Added the experimental`sqlite3_snapshot_get` ,`sqlite3_snapshot_open` , and`sqlite3_snapshot_free` interfaces. These are subject to change or removal in a subsequent release.
- Added the json_group_array() and json_group_object SQL functions in the json extension.

**Performance enhancements:**

- The query planner considers the LIMIT clause when estimating the cost of ORDER BY.
- More efficient handling of application-defined SQL functions, especially in cases where the application defines hundreds or thousands of custom functions.
- Smaller and faster SQL parser.
- Only create statement journal files if two or more attached databases are (1) modified, (2) do not have PRAGMA synchronous set to OFF, and (3) do not have the journal_mode set to OFF, MEMORY, or WAL.
- Only create statement journal files when their size exceeds a threshold. Otherwise the journal is held in memory and no I/O occurs. The threshold can be configured at start-time using`(` SQLITE_CONFIG_STMTJRNL_SPILL`)` .
- The query planner is able to optimize IN operators on virtual tables even if the xBestIndex method does not set the`sqlite3_index_constraint_usage.omit` flag of the virtual table column to the left of the IN operator.
- The query planner now does a better job of optimizing virtual table accesses in a 3-way or higher join where constraints on the virtual table are split across two or more other tables of the join.
- Enhanced WAL mode so that it works efficiently with transactions that are larger than the cache_size.
- Enhanced the query planner so that it is able to use a covering index as part of the OR optimization.
- Avoid recomputing NOT NULL and CHECK constraints on unchanged columns in UPDATE statement.
- Reduce the amount of memory needed to hold the schema.
- Many small and micro-optimizations, resulting in a library that is faster than the previous release.

**General improvements:**

- Fix an FTS5 issue in which the 'optimize' command could cause index corruption.
- Fix a buffer overread that might occur if FTS5 is used to query a corrupt database file.
- Increase the maximum “scope” value for the spellfix1 extension from 6 to 30.
- When using memory-mapped I/O map the database file read-only so that stray pointers and/or array overruns in the application cannot accidently modify the database file.
- Enhance the 'utc' modifier in the date and time functions so that it is a no-op if the date/time is known to already be in UTC. (This is not a compatibility break since the behavior has long been documented as “undefined” in that case.)

**Bug fixes:**

- Make sure the`sqlite3_set_auxdata` values from multiple triggers within a single statement do not interfere with one another.
- Fix the code generator for expression of the form “x IN (SELECT…)” where the SELECT statement on the RHS is a correlated subquery.
- Fix an issue with incorrect sharing of VDBE temporary registers between co-routines that could cause incorrect query results in obscure cases.
- Fix a problem in the`sqlite3_result_subtype` interface that could cause problems for the json1 extension under obscure circumstances.
- Escape control characters in JSON strings.
- Fix a bug in the query planner that could generate incorrect results when a scalar subquery attempts to use the block sorting optimization.
- Fix inconsistent integer to floating-point comparison operations that could result in a corrupt index if the index is created on a table column that contains both large integers and floating point values of similar magnitude.
- Fix an infinite-loop in the query planner that could occur on malformed common table expressions.

## DISQLite3 5.8.4 – 5 Nov 2015

- Fix the schema parser so that it interprets certain (obscure and ill-formed) CREATE TABLE statements the same as legacy.
- Fix a query planner problem that could result in an incorrect answer due to the use of automatic indexing in subqueries in the FROM clause of a correlated scalar subqueries.

## DISQLite3 5.8.3 – 20 Oct 2015

- Added the`sqlite3_value_subtype` and`sqlite3_result_subtype` interfaces.
- Enhanced the dbstat virtual table so that it can be used as a table-valued function where the argument is the schema to be analyzed.
- Do not allow a comma at the end of a JSON array or object.
- Performance optimizations on the JSON parser.

## DISQLite3 5.8.2 – 22 Sep 2015

This is a bug fix release:

- Do not allow rowid in a UNIQUE or PRIMARY KEY constraint.
- Do not allow table-valued function syntax to be used on a view.
- Fix a problem with FTS5 “ORDER BY rowid DESC” queries and large terms.
- Fix a crash in FTS5 caused by specifying NULL as the rhs of a MATCH operator.
- Fix a memory leak and off-by-one error in the json1 extension.
- Fix json_set() so that it can overwrite a value that was previously overwritten during the same call.
- Fix an obscure bug in the initial output of json_tree() when using a path to an object contained within an array.
- Remove old refactoring code from DISQLite3_Full_Text_Search_Form_Main.pas which caused a (harmless) runtime error.

## DISQLite3 5.8.1 – 15 Sep 2015

**New Features And Enhancements:**

- Support Delphi 10 Seattle Win32 and Win64.
- The CREATE VIEW statement now accepts an optional list of column names following the view name.
- Added support for indexes on expressions.
- Added support for table-valued functions in the FROM clause of a SELECT statement.
- Added support for eponymous virtual tables.
- Added the json1 extension.
- A VIEW may now reference undefined tables and functions when initially created. Missing tables and functions are reported when the VIEW is used in a query.
- The query planner is now able to use partial indexes that contain AND-connected terms in the WHERE clause.

**Other changes:**

- Modify the FTS5 leaf page format to permit faster seek operations. This is a file-format change. Any existing databases can be upgraded by running the fts5 'rebuild' command.
- Modify the FTS5 custom tokenizer interface to permit synonym support. The`fts5_api.iVersion` value is now set to 2. Existing FTS5 custom tokenizers will need to be updated to use the new api version.
- The`sqlite3_memory_alarm` interface, which has been deprecated and undocumented for 8 years, is changed into a no-op.

**Fixes:**

- Added a memory barrier in the implementation of`sqlite3_initialize` to help ensure that it is thread-safe.
- Fix the OR optimization so that it always ignores subplans that do not use an index.
- Do not apply the WHERE-clause pushdown optimization on terms that originate in the ON or USING clause of a LEFT JOIN.

## DISQLite3 5.8.0 – 5 Aug 2015

- Added the experimental FTS5 extension. Note that this extension is experimental and subject to change in incompatible ways.
- Added the`sqlite3_value_dup` and`sqlite3_value_free` interfaces.
- Added the`sqlite3_result_zeroblob64` and`sqlite3_bind_zeroblob64` interfaces.
- Enhance the spellfix1 extension to support ON CONFLICT clauses.
- The IS operator is now able to drive indexes.
- Enhance the query planner to permit automatic indexing on FROM-clause subqueries that are implemented by co-routine.
- Disallow the use of “rowid” in common table expressions.
- Added the PRAGMA cell_size_check command for better and earlier detection of database file corruption.
- Added the matchinfo 'b' flag to the matchinfo() function in FTS3.
- The`sqlite3_profile` callback is invoked (by`sqlite3_reset` or`sqlite3_finalize` ) for statements that did not run to completion.
- Enhance the page cache so that it can preallocate a block of memory to use for the initial set page cache lines. Set the default preallocation to 100 pages. Yields about a 5% performance increase on common workloads.
- Miscellaneous micro-optimizations result in 22.3% more work for the same number of CPU cycles relative to the previous release. DISQLite now runs twice as fast as two years ago.
- Add`TDISQLite3Database.BusyTimeout` property.
- Improve`sqlite3_exec_with_callback` error results.

## DISQLite3 5.7.8 – 21 May 2015

- Fix an index corruption issue introduced by DISQLite3 5.7.0. An index with a TEXT key could be corrupted by an INSERT into the corresponding table if the table had two nested triggers that converted the key value to INTEGER and back to TEXT again.

## DISQLite3 5.7.7 – 14 May 2015

- Added the y format string to the matchinfo() function of FTS3.
- Add the dbstat virtual table.
- Performance improvements for ORDER BY, VACUUM, CREATE INDEX, PRAGMA integrity_check, and PRAGMA quick_check.
- Fix many obscure problems discovered while SQL fuzzing.

## DISQLite3 5.7.6 – 30 Apr 2015

- Added the`sqlite3_status64` interface.
- Fix memory size tracking so that it works even if SQLite uses more than 2GiB of memory.
- Added the PRAGMA index_xinfo command.
- Fix a potential 32-bit integer overflow problem in the`sqlite3_blob_read` and`sqlite3_blob_write` interfaces.
- Improve the performance of fts3/4 queries that use the OR operator and at least one auxiliary fts function.
- Fix a bug in the fts3 snippet() function causing it to omit leading separator characters from snippets that begin with the first token in a column.

## DISQLite3 5.7.5 – 25 Apr 2015

- Add support for Delphi XE8 Win32 and Win64.

## DISQLite3 5.7.4 – 27 Feb 2015

- Fix a bug that could lead to incorrect results if the qualifying constraint of a partial index appears in the ON clause of a LEFT JOIN.
- Improve the performance of fts3/4 queries that use the OR operator and at least one auxiliary fts function.

## DISQLite3 5.7.3 – 13 Feb 2015

 **New Features:** 

- Added the PRAGMA data_version command that can be used to determine if a database file has been modified by another process.
- Added the`SQLITE_CHECKPOINT_TRUNCATE` option to the`sqlite3_wal_checkpoint_v2` interface, with corresponding enhancements to PRAGMA wal_checkpoint.`(` SQLITE_CHECKPOINT_TRUNCATE`)` truncates the WAL file even if there is no checkpoint work to be done.
- Added the`sqlite3_stmt_scanstatus` interface.
- The`sqlite3_table_column_metadata` is enhanced to work correctly on WITHOUT ROWID tables and to check for the existence of a table if the column name parameter is`nil` .
- Added the`SQLITE_CONFIG_PMASZ` start-time option.
- Added the`SQLITE_CONFIG_PCACHE_HDRSZ` option to`sqlite3_config` which makes it easier for applications to determine the appropriate amount of memory for use with`SQLITE_CONFIG_PAGECACHE` .
- The number of rows in a VALUES clause is no longer limited.
- Added the eval loadable extension that implements an eval() SQL function that will recursively evaluate SQL.

 **Performance Enhancements:** 

- Reduce the number of memcopy operations involved in balancing a b-tree, for 3.2% overall performance boost.
- Improvements to cost estimates for the skip-scan optimization.
- The automatic indexing optimization is now capable of generating a partial index if that is appropriate.

 **Bug fixes:** 

- Ensure durability following a power loss with “PRAGMA journal_mode=TRUNCATE” by calling fsync() right after truncating the journal file.
- The query planner now recognizes that any column in the right-hand table of a LEFT JOIN can be NULL, even if that column has a NOT NULL constraint. Avoid trying to optimize out NULL tests in those cases.
- Make sure ORDER BY puts rows in ascending order even if the DISTINCT operator is implemented using a descending index.
- Fix data races that might occur under stress when running with many threads in shared cache mode where some of the threads are opening and closing connections.
- Fix a bug in the sorting logic that can cause output to appear in the wrong order on queries that contains an ORDER BY clause, a LIMIT clause, and that have approximately 60 or more columns in the result set.
- Fix obscure crash bugs.

## DISQLite3 5.7.2 – 16 Dec 2014

- Bug fix: Ensure the cached KeyInfo objects (an internal abstraction not visible to the application) do not go stale when operating in shared cache mode and frequently closing and reopening some database connections while leaving other database connections on the same shared cache open continuously.
- Bug fix: Recognize that any column in the right-hand table of a LEFT JOIN can be NULL even if the column has a NOT NULL constraint. Do not apply optimizations that assume the column is never NULL.

## DISQLite3 5.7.1 – 19 Nov 2014

- Enhance the ROLLBACK command so that pending queries are allowed to continue as long as the schema is unchanged. Formerly, a ROLLBACK would cause all pending queries to fail with an`SQLITE_ABORT` or`SQLITE_ABORT_ROLLBACK` error. That error is still returned if the ROLLBACK modifies the schema.
- Bug fix: Make sure that NULL results from OP_Column are fully and completely NULL and do not have the MEM_Ephem bit set.
- Bug fix: The`%c` format in`sqlite3_mprintf` is able to handle precisions greater than 70.
- Bug fix: Do not automatically remove the DISTINCT keyword from a SELECT that forms the right-hand side of an IN operator since it is necessary if the SELECT also contains a LIMIT.

## DISQLite3 5.7.0 – 29 Oct 2014

 **Performance Enhancements:** 

- Many micro-optimizations result in 20.3% more work for the same number of CPU cycles relative to the previous release. The cumulative performance increase since DISQLite3 5.0.0 is 61%. Your performance may vary.
- The sorter can use auxiliary helper threads to increase real-time response. This feature is off by default and may be enabled using the PRAGMA threads command. DISQLite3 Professional only.
- Enhance the skip-scan optimization so that it is able to skip index terms that occur in the middle of the index, not just as the left-hand side of the index.
- Improved optimization of CAST operators.
- Various improvements in how the query planner uses sqlite_stat4 information to estimate plan costs.

 **New Features:** 

- Added new interfaces with 64-bit length parameters:
  - `sqlite3_malloc64`
  - `sqlite3_realloc64`
  - `sqlite3_bind_blob64`
  - `sqlite3_result_blob64`
  - `sqlite3_bind_text64` and
  - `sqlite3_result_text64` .
- Added the new interface`sqlite3_msize` that returns the size of a memory allocation obtained from`sqlite3_malloc64` and its variants.
- Added the`SQLITE_LIMIT_WORKER_THREADS` option to`sqlite3_limit` and PRAGMA threads command for configuring the number of available worker threads.
- The spellfix1 extension allows the application to optionally specify the rowid for each INSERT.

 **Bug Fixes:** 

- Fix a bug in the partial index implementation that might result in an incorrect answer if a partial index is used in a subquery or in a view.
- Fix a query planner bug that might cause a table to be scanned in the wrong direction (thus reversing the order of output) when a DESC index is used to implement the ORDER BY clause on a query that has an identical GROUP BY clause.
- Fix a bug in`sqlite3_trace` that was causing it to sometimes fail to print an SQL statement if that statement needed to be re-prepared.
- In PRAGMA journal_mode=TRUNCATE mode, call fsync() immediately after truncating the journal file to ensure that the transaction is durable across a power loss.
- Fix a couple of problems associated with running an UPDATE or DELETE on a VIEW with a rowid in the WHERE clause.

## DISQLite3 5.6.0 – 3 Oct 2014

- Support Delphi XE7 Win32 and Win64.

## DISQLite3 5.5.2 – 20 Aug 2014

- Added support for hexadecimal integer literals in the SQL parser. (Ex: 0x123abc)
- Enhanced the PRAGMA integrity_check command to detect UNIQUE and NOT NULL constraint violations.
- Increase the maximum number of attached databases from 62 to 125.
- Increase the timeout in WAL mode before issuing an`SQLITE_PROTOCOL` error from 1 second to 10 seconds.
- Added the likely(X) SQL function.
- Trigger automatic reprepares on all prepared statements when ANALYZE is run.

 **Performance Enhancements:** 

- Deactivate the DISTINCT keyword on subqueries on the right-hand side of the IN operator.
- Add the capability of evaluating an IN operator as a sequence of comparisons as an alternative to using a table lookup. Use the sequence of comparisons implementation in circumstances where it is likely to be faster, such as when the right-hand side of the IN operator is small and/or changes frequently.
- The query planner now uses sqlite_stat4 information (created by ANALYZE) to help determine if the skip-scan optimization is appropriate.
- Ensure that the query planner never tries to use a self-made transient index in place of a schema-defined index.
- Other minor tweaks to improve the quality of VDBE code.

 **Bug Fixes:** 

- Fix a bug in CREATE UNIQUE INDEX, introduced when WITHOUT ROWID support was added, that allows a non-unique NOT NULL column to be given a UNIQUE index.
- Fix a bug in R-Tree extension, introduced in the previous release, that can cause an incorrect results for queries that use the rowid of the R-Tree on the left-hand side of an IN operator.
- Fix the`sqlite3_stmt_busy` interface so that it gives the correct answer for ROLLBACK statements that have been stepped but never reset.
- Fix a bug in that would cause a null pointer to be dereferenced if a column with a DEFAULT that is an aggregate function tried to usee its DEFAULT.
- Fix a column affinity problem with the IN operator.
- Fix the ANALYZE command so that it adds correct samples for WITHOUT ROWID tables in the sqlite_stat4 table.

## DISQLite3 5.5.1 – 4 Jun 2014
