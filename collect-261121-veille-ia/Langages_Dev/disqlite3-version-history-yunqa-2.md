---
id: collect-261121-veille-ia/langages-dev/disqlite3-version-history-yunqa-2
title: "DISQLite3: Version History [Yunqa"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["disqlite3", "tdisqlite3database", "query", "index", "column", "added", "number", "optimization"]
source: https://www.yunqa.de/delphi/products/sqlite3/history
source_anchor: ""
source_lines: [1501, 2146]
section: "Langages_Dev"
---
- Added support for partial sorting by index.
- Enhance the query planner so that it always prefers an index that uses a superset of WHERE clause terms relative to some other index.
- Improvements to the automerge command of FTS4 to better control the index size for a full-text index that is subject to a large number of updates.
- Added the`sqlite3_rtree_query_callback` interface to R-Tree extension
- Added new URI query parameters “nolock” and “immutable”.
- Use less memory by not remembering CHECK constraints on read-only database connections.
- Enable the OR optimization for WITHOUT ROWID tables.
- Render expressions of the form “x IN (?)” (with a single value in the list on the right-hand side of the IN operator) as if they where “x==?”, Similarly optimize “x NOT IN (?)”
- Added the`SQLITE_IOCAP_IMMUTABLE` bit to the set of bits that can be returned by the xDeviceCharacteristics method of a VFS.

**Bug Fixes:**

- OFFSET clause ignored on queries without a FROM clause.
- Assertion fault on queries involving expressions of the form “x IN (?)”.
- Incorrect column datatype reported.
- Duplicate row returned on a query against a table with more than 16 indices, each on a separate column, and all used via OR-connected constraints.
- Partial index causes assertion fault on UPDATE OR REPLACE.
- Crash when calling undocumented SQL function sqlite_rename_parent() with NULL parameters.
- ORDER BY ignored if the query has an identical GROUP BY.
- The group_concat(x,'') SQL function returns NULL instead of an empty string when all inputs are empty strings.
- Fix a bug in the VDBE code generator that caused crashes when doing an INSERT INTO … SELECT statement where the number of columns being inserted is larger than the number of columns in the destination table.
- Fix a problem in FTS4 where the left-most column that contained the notindexed column name as a prefix was not indexed rather than the column whose name matched exactly.
- Fix the`sqlite3_db_readonly` interface so that it returns true if the database is read-only due to the file format write version number being too large.

## DISQLite3 5.5.0 – 28 Apr 2014

- Support Delphi XE6 Win32 and Win64.
- New functions`QuoteIdent` and`QuoteIdent16` . Very much like`QuotedStr` and`QuotedStr16` but they also quote SQLite keywords.
- `SqlBufToFloat` and related functions:
  - `Length` parameter is now an Integer type.
  - Process trailing whitespace and control characters similar to SysUtils.StrToFloat.
  - `SqlBufToFloatDef` and`SqlStrToFloatDef` functions must successfully process the entire string, otherwise the default value is returned.
- The DISQLite3_Full_Text_Search demo adjusts line breaks before displaying the file's text. This solves display problems with Mac and Unix files.

## DISQLite3 5.0.7 – 8 Apr 2014

- Add a fix for a problem that might cause incorrect query results on a query that mixes DISTINCT, GROUP BY in a subquery, and ORDER BY.

## DISQLite3 5.0.6 – 27 Mar 2014

- Fix a potential buffer overread that could result when trying to search a corrupt database file.

## DISQLite3 5.0.5 – 12 Mar 2014

- Add`TDISQLite3Database` design-time properties:`TDISQLite3Database.Access` ,`TDISQLite3Database.Cache` ,`TDISQLite3Database.Mutex` ,`TDISQLite3Database.Options` ,`TDISQLite3Database.VFS` .
- New`TDISQLite3Database.ReadOnly` and`TDISQLite3Database.ReadOnly16` methods.
- Apply the`SQLITE_DETERMINISTIC` optimization when creating these functions: math, regexp, regextract and zlib.
- Code optimization and refactoring for improved performance.
- When computing the cost of the skip-scan optimization, take into account the fact that multiple seeks are required.
- Bug fix: Fix the char() SQL function so that it returns an empty string rather than an “out of memory” error when called with zero arguments.
- Bug fix: DISTINCT now recognizes that a zeroblob and a blob of all 0x00 bytes are the same thing.
- Bug fix: Compute the correct answer for queries that contain an IS NOT NULL term in the WHERE clause and also contain an OR term in the WHERE clause.
- Bug fix: Make sure “rowid” columns are correctly resolved in joins between normal tables and WITHOUT ROWID tables.
- Bug fix: Make sure the same temporary registers are not used in concurrent co-routines used to implement compound SELECT statements containing ORDER BY clauses, as such use can lead to incorrect answers.
- Bug fix: Ensure that “ORDER BY random()” clauses do not get optimized out.
- Bug fix: Repair a name-resolution error that can occur in sub-select statements contained within a TRIGGER.
- Bug fix: Fix column default values expressions of the form “DEFAULT(-(-9223372036854775808))” so that they work correctly, initializing the column to a floating point value approximately equal to +9223372036854775808.0.
- Bug fix:`sqlite3_is_memory_name` is now case sensitive, the same way SQLite handles the`:memory:` file name.

## DISQLite3 5.0.4 – 11 Feb 2014

- Fix a bug hat causes rows to go missing on some queries with OR clauses and IS NOT NULL operators in the WHERE clause.

## DISQLite3 5.0.3 – 7 Feb 2014

- Add support for common table expressions. It is now possible to write a single SELECT statement that will query a tree or graph, using either a depth-first or a breadth-first search. As part of this change, SQLite now accepts a VALUES clause anyplace that a SELECT statement is valid.
- Enhancements:
  - Added the printf() SQL function.
  - Added`SQLITE_DETERMINISTIC` as an optional bit in the 4th argument to the`sqlite3_create_function` and related interfaces, providing applications with the ability to create new functions that can be factored out of inner loops when they have constant arguments.
  - Add`SQLITE_READONLY_DBMOVED` error code, returned at the beginning of a transaction, to indicate that the underlying database file has been renamed or moved out from under SQLite.
  - Allow arbitrary expressions, including function calls and subqueries, in the filename argument to ATTACH.
  - Reseed the PRNG used by`sqlite3_randomness(N,P)` when invoked with`N=0` .
  - Enhance the spellfix1 virtual table so that it can search efficiently by rowid.
- Many small performance enhancements which should give a small speed boost to legacy applications.

## DISQLite3 5.0.2 – 2 Jan 2014

- Support for WITHOUT ROWID tables. This is a significant extension to SQLite. Database files that contain WITHOUT ROWID tables are not readable or writable by prior versions of SQLite, however databases that do not use WITHOUT ROWID tables are fully backwards and forwards compatible.
- **Potential incompatibility change:** In all prior versions of SQLite, a cast from a very large positive floating point number into an integer resulted in the most negative integer. In other words, CAST(+99.9e99 to INT) would yield -9223372036854775808. This behavior came about because it is what x86/x64 hardware does for the equivalent cast in the C language. But the behavior is bizarre. And so it has been changed effective with this release so that a cast from a floating point number into an integer returns the integer between the floating point value and zero that is closest to the floating point value. Hence, CAST(+99.9e99 to INT) now returns +9223372036854775807. Since routines like`sqlite3_column_int64` do an implicit cast if the value being accessed is really a floating point number, they are also affected by this change.
- Performance enhancements: The skip-scan optimization is now available for databases that have been processed by ANALYZE. Constant SQL functions are now factored out of inner loops, which can result in a significant speedup for queries that contain WHERE clause terms like “date>datetime('now','-2 days')”. And various high-runner internal routines have been refactored for reduced CPU load.
- Extended the virtual table interface, and in particular the`sqlite3_index_info` object to allow a virtual table to report its estimate on the number of rows that will be returned by a query.
- Update the R-Tree extension to make use of the enhanced virtual table interface.

## DISQLite3 5.0.1 – 21 Oct 2013

- Added the unlikely() and likelihood() SQL functions to be used as hints to the query planner.
- Enhancements to the query planner:
  1. Take into account the fact WHERE clause terms that cannot be used with indices still probably reduce the number of output rows.
  2. Estimate the sizes of table and index rows and use the smallest applicable B-Tree for full scans and “count(*)” operations.
- Added the soft_heap_limit pragma.
- Added support for the sqlite_stat4 table. This is an enhancement over the sqlite_stat3 table which only recorded histogram data for the left-most column of each index whereas sqlite_stat4 tables record histograph data from all columns of each index. sqlite_stat3 tables might still read but are no longer written.
- Added support for “sz=NNN” parameters at the end of sqlite_stat1.stat fields used to specify the average length in bytes for table and index rows.
- Avoid running foreign-key constraint checks on an UPDATE if none of the modified columns are associated with foreign keys.
- Added the`win32-longpath` VFS, permitting filenames up to 32K characters in length (DISQLite3 Pro only).
- The Date And Time Functions are enhanced so that the current time (ex: julianday('now')) is always the same for multiple function invocations within the same`sqlite3_step` call.
- Add the “totype” extension, implementing the`tointeger()` and`toreal()` SQL functions.
- FTS4 queries are better able to make use of`docid < limit` constraints to limit the amount of I/O required.
- The VACUUM command packs the database about 1% tighter.
- Refactor the implementation of PRAGMA statements to improve parsing performance.
- **Bug fix:** Return the correct answer for “SELECT count(*) FROM table” even if there is a partial index on the table.

## DISQLite3 5.0.0 – 25 Sep 2013

- Support Delphi XE5 Win32 and Win64.

## DISQLite3 4.5.3 – 6 Sep 2013

- Fix a bug in the optimization that attempts to omit unused LEFT JOINs.

## DISQLite3 4.5.2 – 30 Aug 2013

- Fix a query planner bug involving a LEFT JOIN with a BETWEEN or LIKE/GLOB constraint and then another INNER JOIN to the right that involves an OR constraint.
- Fix a query planner bug that could result in a segfault when querying tables with a UNIQUE or PRIMARY KEY constraint with more than four columns.

## DISQLite3 4.5.1 – 28 Aug 2013

- Add support for partial indexes.
- Cut-over to the next generation query planner for faster and better query plans.
- The EXPLAIN QUERY PLAN output no longer shows an estimate of the number of rows generated by each loop in a join.
- Added the FTS4 notindexed option, allowing non-indexed columns in an FTS4 table.
- Added the`SQLITE_STMTSTATUS_VM_STEP` option to`sqlite3_stmt_status` .
- Added the cache_spill pragma.
- Added the query_only pragma.
- Added the defer_foreign_keys pragma and the`sqlite3_db_status``(db,` `SQLITE_DBSTATUS_DEFERRED_FKS``,…)` interface.
- Added the “`percentile``()` ” function as a loadable extension.
- Add the`sqlite3_cancel_auto_extension``(X)` interface.
- A running SELECT statement that lacks a FROM clause (or any other statement that never reads or writes from any database file) will not prevent a read transaction from closing.
- Issue an`SQLITE_WARNING_AUTOINDEX` warning on the`SQLITE_CONFIG_LOG` whenever the query planner uses an automatic index.
- Added an optional 5th parameter defining the collating sequence to the “`next_char()` ” extension SQL function.
- The`SQLITE_BUSY_SNAPSHOT` extended error code is returned in WAL mode when a read transaction cannot be upgraded to a write transaction because the read is on an older snapshot.
- Allow read transactions to be freely opened and closed by SQL statements run from within the implementation of application-defined SQL functions if the function is called by a SELECT statement that does not access any database table.
- Bug fix: In the unicode61 tokenizer of FTS4, treat all private code points as identifier symbols.
- Bug fix: Bare identifiers in ORDER BY clauses bind more tightly to output column names, but identifiers in expressions bind more tightly to input column names. Identifiers in GROUP BY clauses always prefer output column names, however.
- Bug fixes: Multiple problems in the legacy query optimizer were fixed by the move to NGQP.

## DISQLite3 4.5.0 – 14 Jun 2013

- Support Delphi XE4 Win32 and Win64.
- Add support for memory-mapped I/O.
- Add the`sqlite3_strglob` convenience interface.
- Assigned the integer at offset 68 in the database header as the Application ID for when SQLite is used as an application file-format. Added the PRAGMA application_id command to query and set the Application ID.
- Report rollback recovery in the error log as`SQLITE_NOTICE_RECOVER_ROLLBACK` . Change the error log code for WAL recover from`SQLITE_OK` to`SQLITE_NOTICE_RECOVER_WAL` .
- Report the risky uses of unlinked database files and database filename aliasing as`SQLITE_WARNING` messages in the error log.
- Enhance the extension loading mechanism to be more flexible (while still maintaining backwards compatibility) in two ways:
  1. If the default entry point “sqlite3_extension_init” is not present in the loadable extension, also try an entry point “sqlite3_X_init” where “X” is based on the shared library filename. This allows every extension to have a different entry point, which allows them to be statically linked with no code changes.
  2. The shared library filename passed to`sqlite3_load_extension` may omit the filename suffix, and an appropriate architecture-dependent suffix (“.dll”) will be added automatically.
- Added many new loadable extensions, including amatch,`closure` , fuzzer, ieee754, and nextchar.
- Enhance FTS3 to avoid using excess stack space when there are a huge number of terms on the right-hand side of the MATCH operator. A side-effect of this change is that the MATCH operator can only accommodate 12 NEAR operators at a time.
- Enhance the fts4aux virtual table so that it can be a TEMP table.
- Added the fts3tokenize virtual table to the full-text search logic.
- Query planner enhancement: Use the transitive property of constraints to move constraints into the outer loops of a join whenever possible, thereby reducing the amount of work that needs to occur in inner loops.
- Bug fix: Fix a potential database corruption bug in shared cache mode when one database connection is closed while another is in the middle of a write transaction.
- Bug fix: Only consider AS names from the result set as candidates for resolving identifiers in the WHERE clause if there are no other matches. In the ORDER BY clause, AS names take priority over any column names.
- Bug fix: Do not allow a virtual table to cancel the ORDER BY clause unless all outer loops are guaranteed to return no more than one row result.
- Bug fix: Do not suppress the ORDER BY clause on a virtual table query if an IN constraint is used.
- Bug fix: Make sure PRAGMA statements appear in`sqlite3_trace` output.
- Bug fix: When a compound query that uses an ORDER BY clause with a COLLATE operator, make sure that the sorting occurs according to the specified collation and that the comparisons associate with the compound query use the native collation.
- Bug fix: Makes sure the authorizer callback gets a valid pointer to the string “ROWID” for the column-name parameter when doing an UPDATE that changes the rowid.
- Bug fix: Do not move WHERE clause terms inside OR expressions that are contained within an ON clause of a LEFT JOIN.
- Bug fix: Make sure an error is always reported when attempting to preform an operation that requires a collating sequence that is missing.

## DISQLite3 4.2.1 – 19 Apr 2013

- Fix a bug that could result in database corruption two or more processes try to access the same database file at the same time and immediately after third process crashed in the middle of committing to that same file.

## DISQLite3 4.2.0 – 31 Mar 2013

- Fix for a bug in the ORDER BY optimizer that was introduced in DISQLite3 4.1.2 which would sometimes optimize out the sorting step when in fact the sort was required.
- Fix a long-standing bug in the CAST expression that would recognize UTF16 characters as digits even if their most-significant-byte was not zero.
- Fix a bug in the NEAR operator of FTS3 when applied to subfields.
- Fix a long-standing bug in the storage engine that would (very rarely) cause a spurious report of an`SQLITE_CORRUPT` error but which was otherwise harmless.
- Added the PRAGMA foreign_key_check command.
- Added new extended error codes for all`SQLITE_CONSTRAINT` errors
- Added the`SQLITE_READONLY_ROLLBACK` extended error code for when a database cannot be opened because it needs rollback recovery but is read-only.
- Added SQL functions unicode(A) and char(X1,…,XN)].
- Performance improvements for PRAGMA incremental_vacuum, especially in cases where the number of free pages is greater than what will fit on a single trunk page of the freelist.
- Improved optimization of queries containing aggregate min() or max().
- Enhance virtual tables so that they can potentially use an index when the WHERE clause contains the IN operator.
- Allow indices to be used for sorting even if prior terms of the index are constrained by IN operators in the WHERE clause.
- Enhance the PRAGMA table_info command so that the “pk” column is an increasing integer to show the order of columns in the primary key.
- Enhance the query optimizer to exploit transitive join constraints.
- Performance improvements in the query optimizer.
- Allow the error message from PRAGMA integrity_check to be longer than 20000 bytes.
- Improved name resolution for deeply nested queries.
- Improved error messages in the RTREE extension.
- Improved error messages for “foreign key mismatch” showing the names of the two tables involved.
- Enhance the spellfix1 extension so that the edit distance cost table can be changed at runtime by inserting a string like 'edit_cost_table=TABLE' into the “command” field.
- Bug fix: repair a long-standing problem that could cause incorrect query results in a 3-way or larger join that compared INTEGER fields against TEXT fields in two or more places.
- Bug fix: Issue an error message if the 16-bit reference counter on a view overflows due to an overly complex query.
- Bug fix: Avoid leaking memory on LIMIT and OFFSET clauses in deeply nested UNION ALL queries.
- Bug fix: Make sure the schema is up-to-date prior to running pragmas table_info, index_list, index_info, and foreign_key_list.
- `DISQLite3Database` : Remove deprecated functions and procedures.
- `DISQLite3Database` : User helper functions and procedures from DIUtils.pas to remove duplicates.

## DISQLite3 4.1.2 – 9 Jan 2013

- Fix a bug that causes an ORDER BY clause to be optimized out of a three-way join when the ORDER BY is actually required.

## DISQLite3 4.1.1 – 20 Dec 2012

- Fix a bug, introduced in DISQLite3 4.1.0, that causes a segfault if the AS name of a result column of a SELECT statement is used as a logical term in the WHERE clause.

## DISQLite3 4.1.0 – 13 Dec 2012

- Added the`sqlite3_errstr` interface.
- Avoid invoking the`sqlite3_trace` callback multiple times when a statement is automatically reprepared due to`SQLITE_SCHEMA` errors.
- Enhance IN operator processing to make use of indices with numeric affinities.
- Do full-table scans using covering indices when possible, under the theory that an index will be smaller and hence can be scanned with less I/O.
- Enhance the query optimizer so that ORDER BY clauses are more aggressively optimized, especially in joins where various terms of the ORDER BY clause come from separate tables of the join.
- Add the ability to implement FROM clause subqueries as coroutines rather that manifesting the subquery into a temporary table.
- Added the busy_timeout pragma.
- Added the instr SQL function.
- Added the`SQLITE_FCNTL_BUSYHANDLER` file control, used to allow VFS implementations to get access to the busy handler callback.
- The xDelete method in the built-in VFSes now return`SQLITE_IOERR_DELETE_NOENT` if the file to be deleted does not exist.
- Bug fix: Avoid various concurrency problems in shared-cache mode.
- Bug fix: Avoid a deadlock or crash if the backup API, shared cache, and the Encryption Extension are all used at once.
- Bug fix: Fix a 32-bit overflow problem on CREATE INDEX for databases larger than 16GB.
- Bug fix: Avoid segfault when using the COLLATE operator inside of a CHECK constraint or view in shared cache mode.

## DISQLite3 4.0.0 – 4 Oct 2012

- Support Delphi XE3 Win32 and Win64.
- Add the`sqlite3_close_v2` interface.
- Enhancements to PRAGMA integrity_check and PRAGMA quick_check so that they can optionally check just a single attached database instead of all attached databases.
- Enhancements to WAL mode processing that ensure that at least one valid read-mark is available at all times, so that read-only processes can always read the database.
- Performance enhancements in the sorter used by ORDER BY and CREATE INDEX.
- Better handling of aggregate queries where the aggregate functions are contained within subqueries.
- Enhance the query planner so that it tries to use a covering index on queries that make use of or optimization.

## DISQLite3 3.1.5 – 14 Jun 2012

- In-memory databases that are specified using URI filenames are allowed to use shared cache, so that the same in-memory database can be accessed from multiple database connections.
- Recognize and use the mode=memory query parameter in URI filenames.
- Avoid resetting the schema of shared cache connections when any one connection closes. Instead, wait for the last connection to close before reseting the schema.
- In the RTREE extension, when rounding 64-bit floating point numbers to 32-bit for storage, always round in a direction that causes the bounding box to get larger.
- The`sqlite3_create_module` and`sqlite3_create_module_v2` interfaces return`SQLITE_MISUSE` on any attempt to overload or replace a virtual table module. The destructor is always called in this case, in accordance with historical and current documentation.
- New FTS3 / FTS4 unicode61 tokenizer with full unicode case folding according to rules in Unicode Version 6.1 and recognizes unicode space and punctuation characters and uses those to separate tokens.

## DISQLite3 3.1.4.1 – 23 May 2012

- Fix a bug introduced in DISQLite3 3.1.4 that can cause a segfault for certain obscure nested aggregate queries.

## DISQLite3 3.1.4 – 14 May 2012

- Add the`SQLITE_DBSTATUS_CACHE_WRITE` option for`sqlite3_db_status` .
- Optimize the typeof() and length() SQL functions so that they avoid unnecessary reading of database content from disk.
- Add the FTS4 “merge” command, the FTS4 “automerge” command, and the FTS4 “integrity-check” command.
- Report the name of specific CHECK constraints that fail.
- Make use of OVERLAPPED in the windows VFS to avoid some system calls and thereby obtain a performance improvement.
- More aggressive optimization of the AND operator when one side or the other is always false.
- Improved performance of queries with many OR-connected terms in the WHERE clause that can all be indexed.
- Add the SQLITE_RTREE_INT_ONLY compile-time option to force the R*Tree Extension Module to use integer instead of floating point values for both storage and computation (DISQLite3 Source Code edition only).
- Enhance the PRAGMA integrity_check command to use much less memory when processing multi-gigabyte databases.
- Allow virtual table constructors to be invoked recursively.
- Improved optimization of ORDER BY clauses on compound queries.
- Improved optimization of aggregate subqueries contained within an aggregate query.
- Bug fix: Fix the RELEASE command so that it does not cancel pending queries. This repairs a problem introduced in DISQLite3 3.1.3.
- Bug fix: Do not discard the DISTINCT as superfluous unless a subset of the result set is subject to a UNIQUE constraint and it none of the columns in that subset can be NULL.
- Do not optimize away an ORDER BY clause that has the same terms as a UNIQUE index unless those terms are also NOT NULL.
- Add`TParam` filtering to the DISQLite3_ClientDataSet_Grid demo project.

## DISQLite3 3.1.3 – 23 Mar 2012

- Enhance the INSERT syntax to allow multiple rows to be inserted via the VALUES clause.
- Enhance the CREATE VIRTUAL TABLE command to support the IF NOT EXISTS clause.
- Added the`sqlite3_stricmp` interface as a counterpart to`sqlite3_strnicmp` .
- Added the`sqlite3_db_readonly` interface.
- Added the`SQLITE_FCNTL_PRAGMA` file control, giving VFS implementations the ability to add new PRAGMA statements or to override built-in PRAGMAs.
- Queries of the form: “SELECT max(x), y FROM table” returns the value of y on the same row that contains the maximum x value.
- Added support for the FTS4 languageid option.
- Documented support for the FTS4 content option. This feature has actually been in the code since DISQLite3 3.1.1 but is only now considered to be officially supported.
- Pending statements no longer block ROLLBACK. Instead, the pending statement will return`SQLITE_ABORT` upon next access after the ROLLBACK.
- Fix a bug introduced in SQLite3 3.7.10 that might cause a LEFT JOIN to be incorrectly converted into an INNER JOIN if the WHERE clause indexable terms connected by OR.

## DISQLite3 3.1.2 – 19 Jan 2012

- The default schema format number is changed from 1 to 4. This means that, unless the PRAGMA legacy_file_format=ON statement is run, newly created database files will be unreadable by version of SQLite prior to 3.3.0 (2006-01-10), DISQLite3 1.2.4 (2005-12-28). It also means that the descending indices are enabled by default.
- The`sqlite3_pcache_methods` structure and the`SQLITE_CONFIG_PCACHE` and`SQLITE_CONFIG_GETPCACHE` configuration parameters are deprecated. They are replaced by a new sqlite3_pcache_methods2 structure and`SQLITE_CONFIG_PCACHE2` and`SQLITE_CONFIG_GETPCACHE2` configuration parameters.
- Added the powersafe overwrite property to the VFS interface. Provide the`SQLITE_IOCAP_POWERSAFE_OVERWRITE` I/O capability, the SQLITE_POWERSAFE_OVERWRITE compile-time option, and the “psow=BOOLEAN” query parameter for URI filenames.
- Added the`sqlite3_db_release_memory` interface and the shrink_memory pragma. Added the`sqlite3_db_filename` interface.
- Added the`sqlite3_stmt_busy` interface.
- Added the`sqlite3_uri_boolean` and`sqlite3_uri_int64` interfaces.
- If the argument to PRAGMA cache_size is negative N, that means to use approximately -1024*N bytes of memory for the page cache regardless of the page size.
- Enhanced the query planner to support index queries with range constraints on the rowid.
- Enhanced the query planner flattening logic to allow UNION ALL compounds to be promoted upwards to replace a simple wrapper SELECT even if the compounds are joins.
- Enhanced the query planner so that the xfer optimization can be used with INTEGER PRIMARY KEY ON CONFLICT as long as the destination table is initially empty.
- Enhanced the windows VFS so that all system calls can be overridden using the xSetSystemCall interface.
- Change the windows VFS to report a sector size of 4096 instead of the old default of 512.
- Bug fix: Add an additional xSync when restarting a WAL in order to prevent an exceedingly unlikely but theoretically possible database corruption following power-loss.
- Bug fix: Change the VDBE so that all registers are initialized to Invalid instead of NULL.
- Bug fix: Fix problems that can result from 32-bit integer overflow.

## DISQLite3 3.1.1 – 3 Dec 2011

- The FTS4 full-text search engine has been enhanced such that tokens in the search string (on the right-hand side of the MATCH operator) that begin with “^” must be the first token in their respective columns in order to match.**Potentially Incompatible Change:** Formerly, “^” characters in the search string were simply ignored. Hence, if a legacy application was including “^” characters in FTS4 search strings, thinking that they would always be ignored, then those legacy applications might break with this update. The fix is simply remove the “^” characters from the search string.
- Added options`SQLITE_DBSTATUS_CACHE_HIT` and`SQLITE_DBSTATUS_CACHE_MISS` to the`sqlite3_db_status` interface.
- Improved ANALYZE now creates the`sqlite_stat3` table, a much more capbable enhancement of the previous`sqlite_stat2` .
- Enhance the`sqlite3_data_count` interface so that it can be used to determine if`SQLITE_DONE` has been seen on the prepared statement.
- Added the`SQLITE_FCNTL_OVERWRITE` file-control by which the SQLite core indicates to the VFS that the current transaction will overwrite the entire database file.
- Increase the default lookaside memory allocator allocation size from 100 to 128 bytes.
- Enhanced the query planner so that it can factor terms in and out of OR expressions in the WHERE clause in an effort to find better indices.
- Overflow pages can now be read directly from the database file, bypassing the page cache. This might improve read performance of large BLOBs (DISQLite3 Pro only).
- Remove limits on the magnitude of precision and width value in the format specifiers of the`sqlite3_mprintf` family of string rendering routines.
- Fix a bug that prevent ALTER TABLE | ALTER TABLE … RENAME from working on some virtual tables in a database with a UTF16 encoding.
- Fix a bug in ASCII-to-float conversion that causes slow performance and incorrect results when converting numbers with ridiculously large exponents.
- Fix a bug that causes incorrect results in aggregate queries that use multiple aggregate functions whose arguments contain complicated expressions that differ only in the case of string literals contained within those expressions.
- Fix a bug that prevented the page_count and quick_check pragmas from working correctly if their names were capitalized.
- Fix a bug that caused VACUUM to fail if the count_changes pragma was engaged.
- Fix a bug in virtual table implementation that causes a crash if an FTS4 table is dropped inside a transaction and a SAVEPOINT occurs afterwards.
- New “external merge sort” algorithm used to implement ORDER BY and GROUP BY and also to presort the content of an index for CREATE INDEX (DISQLite3 Pro only). The new algorithm does approximately the same number of comparisons and I/Os as before, but the I/Os are much more sequential and so runtimes are greatly reduced when the size of the set being sorted is larger than the filesystem cache. The performance improvement can be dramatic – orders of magnitude faster for large CREATE INDEX commands. On the other hand, the code is slightly slower (1% or 2%) for a small CREATE INDEX. Since CREATE INDEX is not an operation that commonly occurs on a speed-critical path, we feel that this tradeoff is a good one. The slight slowdown for small CREATE INDEX statements might be recovered in a future release. ORDER BY and GROUP BY operations should now be faster for all cases, large and small.
- Improved the windows VFS to better defend against interference from anti-virus software.
- Improved query plan optimization when the DISTINCT keyword is present on SELECT statements.
- Increase the default size of a lookahead cache line from 100 to 128 bytes.
- Bug fix: Virtual tables now handle IS NOT NULL constraints correctly.
- Bug fixes: Correctly handle nested correlated subqueries used with indices in a WHERE clause.

## DISQLite3 3.1.0 – 8 Nov 2011

- Support Delphi XE2 Win64.
- SQLite's ATTACH stopped to recognize the DISQLite3 encryption codec so no password was set for attached databases. Fixed.
- Fix buffer allocation in Demos\DISQLite3_Full_Text_Search\DISQLite3Tokenizer.pas. Long tokens were not recognized at full length.
- Add column sorting to the DISQLite3_Full_Text_Search demo.

## DISQLite3 3.0.0 – 15 Oct 2011

- Support Delphi XE2 Win32.
- Add DISQLite3_FastReport demo project.

## DISQLite3 2.4.5 – 1 Jul 2011

- Add support for URI filenames. URI filenames are disabled by default (for backwards compatibility) but applications are encouraged to enable them since incompatibilities are likely to be exceedingly rare and the feature is useful.
- Add the`sqlite3_vtab_config` interface in support of ON CONFLICT clauses with virtual tables.
- The virtual table interface has been enhanced to support SAVEPOINT and ON CONFLICT clause processing, and the built-in RTREE and FTS3/FTS4 have been augmented to take advantage of the new capability. This means, for example, that it is now possible to use the REPLACE command on FTS3/FTS4 and RTREE tables.
- Avoid unnecessary reparsing of the database schema.
- Added support for the FTS4 prefix option and the FTS4 order option.
- Allow WAL-mode databases to be opened read-only as long as there is an existing read/write connection.
- Fix a problem with WAL mode which could cause transactions to silently rollback if the cache_size is set very small (less than 10) and DISQLite3 comes under memory pressure.

## DISQLite3 2.4.4 – 12 Apr 2011

- Integer arithmetic operations that would have resulted in overflow are now performed using floating-point instead.
- New “fuzzer” Virtual Table (DISQLite3 Pro only).
- New “wholenumber” Virtual Table.
- Increased the version number on the VFS object to 3 and added new methods xSetSysCall, xGetSysCall, and xNextSysCall used for testing.
- Increase the maximum number of attached databases from 30 to 62, even though the default value remains at 10.
- Enhance the ANALYZE command to support the name of an index as its argument, in order to analyze just that one index.

## DISQLite3 2.4.3 – 2 Mar 2011

This is an intermediate release based on an a pre SQLite 3.7.6 snapshot. Recommended only to make use of the newly added functionality.

- Enable support for loadable extensions. Extension modules may be build using`DISQLite3Ext` .
- Added the`sqlite3_wal_checkpoint_v2` interface and enhanced the wal_checkpoint pragma to support blocking checkpoints.
- Improvements to the query planner so that it makes better estimates of plan costs and hence does a better job of choosing the right plan.
- Fix a bug which prevented deferred foreign key constraints from being enforced when`sqlite3_finalize` was not called by one statement with a failed foreign key constraint prior to another statement with foreign key constraints running.
- Enhancements to FTS4: Add the compress and uncompres options and direct access to the full-text index.
- Bug fixes.

## DISQLite3 2.4.2 – 1 Feb 2011

- Added the`SQLITE_DBSTATUS_LOOKASIDE_HIT` ,`SQLITE_DBSTATUS_LOOKASIDE_MISS_SIZE` , and`SQLITE_DBSTATUS_LOOKASIDE_MISS_FULL` options for the`sqlite3_db_status` interface.
- Updates to`sqlite3_stmt_readonly` so that its result is well-defined for all prepared statements and so that it works with VACUUM.
- Fix a bug involving frequent changes in and out of WAL mode and VACUUM that could (in theory) cause database corruption.
- Enhance the`sqlite3_trace` mechanism so that nested SQL statements such as might be generated by virtual tables are shown but are shown in comments and without parameter expansion. This greatly improves tracing output when using the FTS3/4 and/or RTREE virtual tables.
- Change the xFileControl() methods on all built-in VFSes to return`SQLITE_NOTFOUND` instead of`SQLITE_ERROR` for an unrecognized operation code.
- The SQLite core invokes the`SQLITE_FCNTL_SYNC_OMITTED` file control to the VFS in place of a call to xSync if the database has PRAGMA synchronous set to OFF.
- Testing shows that this release is about 1% or 2% faster than the previous release for most operations.

## DISQLite3 2.4.1 – 29 Dec 2010

- Added the`sqlite3_blob_reopen` interface to allow an existing`sqlite3_blob` object to be rebound to a new row. Also use this to improve the performance of FTS3.
- Added the`sqlite3_create_function_v2` interface that includes a destructor callback.
- VFSes that do not support shared memory are allowed to access WAL databases if PRAGMA locking_mode is set to EXCLUSIVE.
- Added support for custom r-tree queries using application-supplied callback routines to define the boundary of the query region.
- Enhancements to EXPLAIN QUERY PLAN.
- Added the`sqlite3_stmt_readonly` interface.
- Added PRAGMA checkpoint_fullfsync.
- Added the`SQLITE_FCNTL_FILE_POINTER` option to`sqlite3_file_control` .
- Added support for FTS4 and enhancements to the FTS matchinfo function.
- Added the`sqlite3_soft_heap_limit64` interface as a replacement for`sqlite3_soft_heap_limit` .
- The default page cache strives more diligently to avoid using memory beyond what is allocated to it by`SQLITE_CONFIG_PAGECACHE` . Or if using page cache is allocating from the heap, it strives to avoid going over the`sqlite3_soft_heap_limit64` , even if SQLITE_ENABLE_MEMORY_MANAGEMENT is not defined at compile time.
- The ANALYZE command now gathers statistics on tables even if they have no indices.
- Tweaks to the query planner to help it do a better job of finding the most efficient query plan for each query.
- Enhanced the internal text-to-numeric conversion routines so that they work with UTF8 or UTF16, thereby avoiding some UTF16-to-UTF8 text conversions.
- Fix a problem that was causing excess memory usage with large WAL transactions in win32 systems.
- The interface between the VDBE and B-Tree layer is enhanced such that the VDBE provides hints to the B-Tree layer letting the B-Tree layer know when it is safe to use hashing instead of B-Trees for transient tables.
- A very obscure bug associated with the or optimization was fixed.
- Miscellaneous documentation enhancements.

## DISQLite3 2.4.0 – 28 Sep 2010

- Delphi XE support.
- Add thread safety to help index and update FAQ.

## DISQLite3 2.3.1 – 31 Aug 2010

- Fix an old and very obscure bug that can lead to corruption of the database free-page list when incremental_vacuum is used multiple times to partially reduce the size of a database file that contains many hundreds of unused database pages. The bug is impossible to hit without using incremental_vacuum and is very difficult to hit even with incremental_vacuum. And the kind of corruption that the bug causes can usually be fixed simply by running VACUUM.
- Added new commands`SQLITE_DBSTATUS_SCHEMA_USED` and`SQLITE_DBSTATUS_STMT_USED` to the`sqlite3_db_status` interface, in order to report out the amount of memory used to hold the schema and prepared statements of a connection.
- Increase the maximum size of a database pages from 32KiB to 64KiB. A database with 64KiB pages will not be readable or writable by older versions of SQLite. Note that further increases in page size are not feasible since the file format uses 16-bit offsets to structures within each page.
- Use the LIKE optimization even if the right-hand side string contains no wildcards.
- Added the`SQLITE_FCNTL_CHUNK_SIZE` verb to the`sqlite3_file_control` interface, to cause database files to grow in large chunks in order to reduce disk fragmentation.
- Fixed a bug in the query planner that caused performance regresssions relative to SQLite 3.6.23.1 on some complex joins.
- Performance improvement to`TDISQLite3Database.Execute` when processing very large SQL scripts.

## DISQLite3 2.3.0.1 – 05 Aug 2010

- Fix a potential database corruption problem that can result if the same database file is alternately written by DISQLite3 version 2.3.0 (SQLite version 3.7.0) and DISQLite3 2.2.2 (SQLite 3.6.23.1) or earlier.
- Fix a possible performance regression caused by the introduction of automatic indexing.

The differences between DISQLite3 2.3.0.1 and DISQLite3 2.3.0 are minimal, but because of the possibility of database corruption, upgrading is highly recommended.

## DISQLite3 2.3.0 – 28 Jul 2010

- Added support for write-ahead logging (DISQLite3 Pro only).
- Query planner enhancement – automatic transient indices are created when doing so reduces the estimated query time.
- Query planner enhancement – the ORDER BY becomes a no-op if the query also contains a GROUP BY clause that forces the correct output order.
- Add the`SQLITE_DBSTATUS_CACHE_USED` verb for`sqlite3_db_status` .
- The logical database size is now stored in the database header so that bytes can be appended to the end of the database file without corrupting it.

## DISQLite3 2.2.2 – 31 Mar 2010

- Fix a missing “sync” that when omitted could lead to database corruption if a power failure or OS crash occurred just as a ROLLBACK operation was finishing.
- Fix that INSERTing into an RTREE returns the correct`sqlite3_last_insert_rowid` .
- Correctly handle strings with zero tokens in the FTS3 offsets() function.
- Enhance FTS3 to take advantage of the MAX() optimization.
- Improved error message when the right-hand operand of MATCH in FTS3 is not a valid search expression.
- Fix crash compiling “INSERT INTO tbl DEFAULT VALUES” if tbl has BEFORE or INSTEAD OF triggers.
- Fix an out of memory error on ANALYZE with STAT2 enabled with zero-length data.

## DISQLite3 2.2.1 – 17 Mar 2010

- Added the secure_delete pragma.
- Added the`sqlite3_compileoption_used` and`sqlite3_compileoption_get` interfaces as well as the compile_options pragma and the sqlite_compileoption_used() and sqlite_compileoption_get() SQL functions.
- Added the`sqlite3_log` interface together with the`SQLITE_CONFIG_LOG` verb to`sqlite3_config` .
- Improvements to FTS3.
- The integrity_check pragma is enhanced to detect out-of-order rowids.
- Various minor bug fixes and performance enhancements.
- Completely revised the help generator to ease navigation and improve readability. Send your comments!

## DISQLite3 2.2.0 – 9 Jan 2010

***IMPORTANT INCOMPATIBILITY CHANGE*** – please read carefully:

Up to this version, the `sqlite3` type was an untyped `Pointer`. This worked fine when used correctly, but caused hard to identify errors when not. Incompatible variable types are usually detected by the compiler's type checking. But as a matter of fact, Delphi does not generate type errors for untyped `Pointer`. Code would compile happily if `sqlite3` was used instead of `sqlite3_stmt`, and Delphi would not even object to `TDISQLite3Database` in place of `sqlite3`.

Instead, problems surfaced at runtime. They often materialized as access violations with no meaningful error message and could drive helpless developers into despair. When contacting support, some of them even attributed the problem to a bug in DISQLite3. Even though this was not correct, they certainly had a point. So after careful consideration I decided to change all untyped `Pointer` types to pointers to `record` structures.

For `record` pointers, Delphi can detect type conflicts at compile time, so the likelihood of critical runtime errors is greatly reduced. For most stringent type checking, I recommend to compile with “Typed @ operator” always enabled. This can be set as a project option or in code using `{$T+}` or `{$TYPEDADDRESS ON}`. With typed @ operator enabled, Delphi also detects incompatible output variable types passed to `sqlite3_open`, `sqlite3_prepare_v2`, and similar routines.

Unfortunately, the modified strong typing might ask to change existing code before it compiles. Luckily, however, with `{$TYPEDADDRESS ON}` Delphi now catches all type incompatibilities so they can be fixed easily. For smaller projects, this is a simple search and replace (older IDEs) or code refactoring (newer ones).

**Automatic conversion** for larger projects is at hand here:

- https://www.yunqa.de/delphi/downloads/DISQLite3_Type_Converter.zip

DISQLite3_Type_Converter.zip contains a precompiled version of the JVCLConverter utility plus the conversion data file. It takes less than a minute to update the DISQLite3 Demos from previous versions: Run JVCLConverter.exe, select the source code folder, and convert. Converting other projects should be just as quickly.

**Limit to auto-conversion:** The `PPointerArray` type (mostly used in `sqlite3_create_function` callbacks) is a standard Delphi type. Converting this automatically might cause unintended side effects if used outside the DISQLite3 context. Therefore `PPointerArray` must be manually changed to `sqlite3_value_ptr_array_ptr`.

This table lists all modified, renamed, or deleted type names and their new counterparts. Only those at the top of the list should be in frequent use:

| Old | New | 
|---|---|
| `sqlite3_ptr` | `sqlite3_ptr_ptr` | 
| `sqlite3` | `sqlite3_ptr` | 
| `TDISQLite3DatabaseHandle` | `sqlite3_ptr` | 
| `PDISQLite3DatabaseHandle` | `sqlite3_ptr_ptr` | 
| `sqlite3_stmt_ptr` | `sqlite3_stmt_ptr_ptr` | 
| `sqlite3_stmt` | `sqlite3_stmt_ptr` | 
| `TDISQLite3StatementHandle` | `sqlite3_stmt_ptr` | 
| `PDISQLite3StatementHandle` | `sqlite3_stmt_ptr_ptr` | 
| `sqlite3_blob` | `sqlite3_blob_ptr` | 
| `TDISQLite3BlobHandle` | `sqlite3_blob_ptr` | 
| `PDISQLite3BlobHandle` | `sqlite3_blob_ptr_ptr` | 
| `TSQLite3_Module` | `sqlite3_module` | 
| `Psqlite3_module` | `sqlite3_module_ptr` | 
| `Tsqlite3_tokenizer` | `sqlite3_tokenizer` | 
| `Psqlite3_tokenizer` | `sqlite3_tokenizer_ptr` | 
| `PPsqlite3_tokenizer` | `sqlite3_tokenizer_ptr_ptr` | 
| `Tsqlite3_tokenizer_module` | `sqlite3_tokenizer_module` | 
| `Psqlite3_tokenizer_module` | `sqlite3_tokenizer_module_ptr` | 
| `Tsqlite3_tokenizer_cursor` | `sqlite3_tokenizer_cursor` | 
| `Psqlite3_tokenizer_cursor` | `sqlite3_tokenizer_cursor_ptr` | 
| `PPsqlite3_tokenizer_cursor` | `sqlite3_tokenizer_cursor_ptr_ptr` | 
| `Tsqlite3_vtab` | `sqlite3_vtab` | 
| `Psqlite3_vtab` | `sqlite3_vtab_ptr` | 
| `PPsqlite3_vtab` | `sqlite3_vtab_ptr_ptr` | 
| `Tsqlite3_vtab_cursor` | `sqlite3_vtab_cursor` | 
| `Psqlite3_vtab_cursor` | `sqlite3_vtab_cursor_ptr` | 
| `PPsqlite3_vtab_cursor` | `sqlite3_vtab_cursor_ptr_ptr` | 
| `TSQLite3_index_info` | `sqlite3_index_info` | 
| `Psqlite3_index_info` | `sqlite3_index_info_ptr` | 
| `Tsqlite3_index_constraint` | `sqlite3_index_constraint` | 
| `Tsqlite3_index_constraint_array` | `sqlite3_index_constraint_array` | 
| `Psqlite3_index_constraint_array` | `sqlite3_index_constraint_array_ptr` | 
| `Tsqlite3_index_orderby` | `sqlite3_index_orderby` | 
| `Tsqlite3_index_orderby_array` | `sqlite3_index_orderby_array` | 
| `Psqlite3_index_orderby_array` | `sqlite3_index_orderby_array_ptr` | 
| `Tsqlite3_index_constraint_usage` | `sqlite3_index_constraint_usage` | 
| `Tsqlite3_index_constraint_usage_array` | `sqlite3_index_constraint_usage_array` | 
| `Psqlite3_index_constraint_usage_array` | `sqlite3_index_constraint_usage_array_ptr` | 

Bugs fixed in this version:

- Problems that might cause incorrect query results:
  - Queries that have a WHERE clause of the form (x AND y) OR z where x and z come from one table of a join and y comes from a different table.
  - Queries where the use of the CAST operator in the WHERE clause could lead to incorrect results if the column being cast to a new datatype is also used in the same WHERE clause without being cast.
  - Expressions with different COLLATIONs could compare equal and lead to wrong query results.
- When reusing an instance of`TDISQLite3Database` , the internal transaction counter was not reset when opening another database so its InTransaction property could return wrong results. Fixed.
- Other minor bug fixes and performance enhancements, especially for the FTS3 full text search.

## DISQLite3 2.1.2 – 8 Dec 2009

- The SQL output resulting from`sqlite3_trace` is now modified to include the values of bound parameters.
- Optimizer enhancement: prepared statements are automatically re-compiled when a binding on the right hand side of a LIKE operator changes or when any range constraint changes for analyzed tables.
- Performance optimizations (12% reduction in the number of CPU operations) targetting specific use cases. Changes include the ifnull(), coalesce(), and substr() SQL functions plus various code refactorizations.
- More flexible Unicode tokenizer for the DISQLite3_Full_Text_Search demo. Added option for word stemming in 15 languages using YuStemmer.
- New FTS3 documentation is now available.
- Various minor bug fixes and documentation enhancements.
- New DISQLite3_Backup demo is a fully functional console application to back up any “hot” SQLite database.

## DISQLite3 2.1.1 – 15 Oct 2009

- Native support for foreign key constraints. Foreign key constraints are disabled by default. Use the foreign_keys pragma to turn them on.
- Generalized the IS and IS NOT operators to take arbitrary expressions on their right-hand side.
- Recursive triggers support. The older non-recursive behavior of triggers is still the default behavior. Recursive triggers are activated using the recursive_triggers pragma. In addition to allowing triggers to call themselves (either directly or indirectly) the new capability also fires DELETE triggers on rows that are removed from a table as a result of REPLACE conflict resolution processing. Non-recursive triggers are still the default behavior since this is least likely to cause problems for existing applications. However, it is anticipated that triggers will become recursive by default sometime in the future. At that point, applications that want to continue using the older non- recursive trigger behavior will need to use the recursive_triggers pragma to disable recursive triggers.
- Refinements to the query planner that help to choose better plans for joins where in the past it was selecting suboptimal query plans. Running the ANALYZE command will collect histogram data on indices. The use of histograms improves the query planning performance even more.
- Added the`SQLITE_OPEN_SHAREDCACHE` and`SQLITE_OPEN_PRIVATECACHE` flags for`sqlite3_open_v2` used to override the global shared cache mode settings for individual database connections.
- Added improved version identification features: New`sqlite3_sourceid` interface and SQL function sqlite_source_id().
- Obscure bug fix on triggers, plus other bux fixes.

## DISQLite3 2.1.0 – 14 Sep 2009

- Delphi 2010 support.
- Fix a threading issue with TDISQLite3Database by wrapping all calls to its FStatements field with mutexes.
- Fix`sqlite3_key` so that it successfully sets a correct password after an incorrect one.

## DISQLite3 2.0.9 – 18 Aug 2009

- Mark`sqlite3_prepare` and`sqlite3_prepare16` as deprecated. They are retained for backwards compatibility, but their use is discounraged. Instead,`sqlite3_prepare_v2` and`sqlite3_prepare16_v2` should be used, which work much better with`SQLITE_SCHEMA` errors.
- Expose the`sqlite3_strnicmp` interface for use by extensions and applications.
- Remove the restriction on virtual tables and shared cache mode. Virtual tables and shared cache can now be used at the same time.
- Modify FTS3 so that in terms like 'column_name:token' the column_name is interpreted in a case-insenstive fashion.
- Fix a double-free that can occur when using the FTS3 '-' operator.
- Other minor bug fixes.

## DISQLite3 2.0.8 – 14 Jul 2009

- Fix a bug that occasionally causes INSERT or UPDATE operations to fail on an indexed table that has a self-modifying trigger.
- Internal representation of SQL expressions use less memory.
- Reduce the amount of stack space used.
- The`sqlite3_create_function` family of interfaces now return`SQLITE_MISUSE` instead of`SQLITE_ERROR` when passed invalid parameter combinations.
- When new tables are created using CREATE TABLE … AS SELECT … the datatype of the columns is the simplified SQLite datatype (TEXT, INT, REAL, NUMERIC, or BLOB) instead of a copy of the original datatype from the source table.
- Resolve race conditions when checking for a hot rollback journal.
- The`sqlite3_shutdown` interface frees all mutexes.
- Enhanced robustness against corrupt database files.
- Fix a code generator bug that can cause incorrect query results under obscure circumstances.
- Fix a bug in group_concat().
- Fix a performance bug in the pager cache.
- Fix a bug in the`sqlite3_backup` implementation that can lead to a corrupt backup database.
- Allow empty`TDISQLite3Database.DatabaseName` to create temporary databases. A warning is issued a design time.
- Other minor bug fixes and performance optimizations.

## DISQLite3 2.0.7 – 8 May 2009

- New asynchronous backend module (DISQLite3 Pro only). It gives the illusion of faster response times by pushing slow write operations into a background thread. The tradeoff for faster response time is that more memory is required (to hold the content of the pending writes) and if a power failure or program crash occurs, some transactions that appeared to have committed might end up being rolled back upon restart.
- Performance enhancements in the btree and pager layers and in the query optimizer. Certain workloads can be as much as twice as fast as the previous release, though 10% faster is a more typical result.
- Enhanced the query optimizer so that queries against virtual tables that contain OR and IN operators in the WHERE clause are now able to use indexes.
- Many minor bug fixes.

## DISQLite3 2.0.6 – 28 Apr 2009

- Fix a bug that causesd a segfault when running a count(*) on the sqlite_schema table of an empty database.
- Fix a bug that causesd a segfault that when inserting into a table using a DEFAULT value where there is a function as part of the DEFAULT value expression.
- Other minor bug fixes.

## DISQLite3 2.0.5 – 7 Apr 2009

- Fixed a bug that caused database corruption when an incremental_vacuum is rolled back in an in-memory database. If an incremental_vacuum was rolled back in an in-memory database, the database would often go corrupt. This only happened for in-memory databases. On-disk databases were unaffected. And the corruption only appeared if an incremental vacuum was rolled back. Upgrading is recommended for all applications, especially those that make use of in-memory databases and/or incremental vacuum.
- Fixed a bug in the lookaside memory allocator relating to shared cache mode. If a database connection closed before others, it could prematurely deallocate critical elements of the shared schema.
- Added the`sqlite3_unlock_notify` interface (DISQLite3 Pro only).
- Added the reverse_unordered_selects pragma (DISQLite3 Pro only).
- The default page size is automatically adjusted to match the capabilities of the underlying filesystem.
- Performance improvements for “count(*)” queries (DISQLite3 Pro only).
- Reduce the amount of heap memory used, especially by TRIGGERs.

## DISQLite3 2.0.4 – 5 Mar 2009

- New Hot Backup Interface (DISQLite3 Pro only).
- Improve`TDISQLite3UniDirQuery` Provider Support when inserting and updating BLOB fields with #0 values and NULL BLOBs.
- For PRIMATY KEY columns, Use the ftAutoInc field type and exclude pfInUpdate from ProviderFlags in`TDISQLite3UniDirQuery` . This helps to handle multiple insert problems with TClientDataSet.
- Update DISQLite3_ClientDataSet_Grid.dpr demo to use decreasing negative RowIDs so multiple inserts do not conflict.
- Various minor bug fixes.
- Type changes to`sqlite3_context` and`sqlite3_context_ptr` . This an unfortunate incompatibility change. To find out if you are affected, recompile your code and watch for compiler errors. If so, update your code by replacing “sqlite3_context” with “sqlite3_context_ptr”.
- Improvements to the syntax bubble diagrams.

## DISQLite3 2.0.3 – 31 Jan 2009

- Added support for nested transactions.
- Enhanced the query optimizer so that it is able to use multiple indices to efficiently process OR-connected constraints in a WHERE clause.
- Added support for parentheses in FTS3 query patterns using the SQLITE_ENABLE_FTS3_PARENTHESIS compile-time option (source code only).
- Fix a cache coherency problem that could lead to database corruption.
- Fix two bugs, which when combined might result in incorrect query results. Both bugs were harmless by themselves; only when they team up do they cause problems.

## DISQLite3 2.0.2 – 20 Dec 2008

- Add`TDISQLite3UniDirQuery` support for TClientDataSet.RefreshRecord.
- Fixes for various minor bugs.

## DISQLite3 2.0.1 – 28 Nov 2008

- Fix a bug in the b-tree delete algorithm that seems like it might be able to cause database corruption.
- Fix a memory leak that can occur following a disk I/O error.
- Fix a bug in the page cache that can lead database corruption following a rollback.
- Add the application-defined page cache.
- `TDISQLite3UniDirQuery` now sets the TField.Required property for columns declared as NOT NULL.
- Add the`TDISQLite3DataSetImporter.OnImportError` event to catch and log exceptions (which may be caused by, for example, invalid data conversions on certain records) and continue the import.
- Also in`TDISQLite3DataSetImporter` , call the progress event for every single record instead of every 16th one only.
- Fix a memory overrun in`TDISQLite3DataSetImporter` in the line wrap functions for textual blob data.
- Other minor bug fixes and optimizations.

## DISQLite3 2.0.0 – 17 Nov 2008

- Delphi 2009 support.
- Add the MEMORY option to PRAGMA journal_mode.
- Added the`sqlite3_db_mutex` interface.
- Added the`sqlite3_extended_errcode` interface.
- The COMMIT command now succeeds even if there are pending queries. It returns`SQLITE_BUSY` if there are pending incremental BLOB I/O requests.
- The error code is changed to`SQLITE_BUSY` (instead of`SQLITE_ERROR` ) when an attempt is made to ROLLBACK while one or more queries are still pending.
- Add option support for LIMIT and ORDER BY clauses on DELETE and UPDATE statements (DISQLite3 Professional only).
- Added the`sqlite3_stmt_status` interface for performance monitoring.
- Add the INDEXED BY clause.
- Added the TRUNCATE option to PRAGMA journal_mode
- Performance enhancements to tree balancing logic in the B-Tree layer.
- The SQL language documentation is converted to use syntax diagrams instead of BNF.
- Bug fixes.

## DISQLite3 1.6.1 – 8 Aug 2008

- Added the lookaside memory allocator for a speed improvement in excess of 15% on some workloads. (Your mileage may vary.)
- Added the`SQLITE_CONFIG_LOOKASIDE` verb to`sqlite3_config` to control the default lookaside configuration.
- Added verbs`SQLITE_STATUS_PAGECACHE_SIZE` and`SQLITE_STATUS_SCRATCH_SIZE` to the`sqlite3_status` interface.
- Modified`SQLITE_CONFIG_PAGECACHE` and`SQLITE_CONFIG_SCRATCH` to remove the “+4” magic number in the buffer size computation.
- Added the`sqlite3_db_config` and`sqlite3_db_status` interfaces for controlling and monitoring the lookaside allocator separately on each database connection.
- Numerious other performance enhancements.
- Miscellaneous minor bug fixes.
