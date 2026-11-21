---
id: collect-261121-veille-ia/actu-business/release-history-3
title: "Release History"
domain: actu-business
role: reference
task: article
actors: []
dates: ["2017-08-01"]
keywords: ["ticket", "sqlite", "hashes", "source", "sha3-256", "sqlite3", "optimization", "left"]
source: https://sqlite.org/draft/changes.html
source_anchor: ""
source_lines: [3001, 4500]
section: "Actu_Business"
---
     flattening optimization so that the result is consistent with other
     queries that do not use that optimization, and with PostgreSQL, MySQL,
     and SQLServer.  Ticket de3403bf5ae.
20.  Fix the query planner so that it knows not to use automatic indexes
     on the right table of LEFT JOIN if the WHERE clause uses the IS operator.
     Fix for ce68383bf6aba.
21.  Ensure that the query planner knows that any column of a
     flattened LEFT JOIN can be NULL even
     if that column is labeled with "NOT NULL". Fix for ticket
     892fc34f173e99d8.
22.  Fix rare false-positives in PRAGMA integrity_check when run on a database connection
     with attached databases. Ticket
     a4e06e75a9ab61a12
23.  Fix a bug (discovered by OSSFuzz) that causes an assertion fault if certain
     dodgy CREATE TABLE declarations are used.  Ticket
     bc115541132dad136
**Hashes:**
24. SQLITE_SOURCE_ID: "2017-08-01 13:24:15 9501e22dfeebdcefa783575e47c60b514d7c2e0cad73b2a496c0bc4b680900a8"
25. SHA3-256 for sqlite3.c: 79b7f3b977360456350219cba0ba0e5eb55910565eab68ea83edda2f968ebe95

### 2017-06-17 (3.18.2)

1. Fix a bug that might cause duplicate output rows when an IN operator is
    used in the WHERE clause.
    Ticket 61fe9745.
**Hashes:**
2. SQLITE_SOURCE_ID: "2017-06-17 09:59:36 036ebf729e4b21035d7f4f8e35a6f705e6bf99887889e2dc14ebf2242e7930dd"
3. SHA3-256 for sqlite3.c: b0bd014f2776b9f9508a3fc6432f70e2436bf54475369f88f0aeef75b0eec93e

### 2017-06-16 (3.18.1)

1. Fix a bug associated with auto_vacuum that can lead to database
    corruption.  The bug was introduced in version 3.16.0 (2017-01-02).
    Ticket fda22108.
**Hashes:**
2. SQLITE_SOURCE_ID: "2017-06-16 13:41:15 77bb46233db03a3338bacf7e56f439be3dfd1926ea0c44d252eeafa7a7b31c06"
3. SHA3-256 for sqlite3.c: 334eaf776db9d09a4e69d6012c266bc837107edc2c981739ef82081cb11c5723

### 2017-06-08 (3.19.3)

1. Fix a bug associated with auto_vacuum that can lead to database
    corruption.  The bug was introduced in version 3.16.0 (2017-01-02).
    Ticket fda22108.
**Hashes:**
2. SQLITE_SOURCE_ID: "2017-06-08 14:26:16 0ee482a1e0eae22e08edc8978c9733a96603d4509645f348ebf55b579e89636b"
3. SHA3-256 for sqlite3.c: 368f1d31272b1739f804bcfa5485e5de62678015c4adbe575003ded85c164bb8

### 2017-05-25 (3.19.2)

1. Fix more bugs in the LEFT JOIN flattening optimization. Ticket
    7fde638e94287d2c.
**Hashes:**
2. SQLITE_SOURCE_ID: "2017-05-25 16:50:27 edb4e819b0c058c7d74d27ebd14cc5ceb2bad6a6144a486a970182b7afe3f8b9"
3. SHA3-256 for sqlite3.c: 1be0c457869c1f7eba58c3b5097b9ec307a15be338308bee8e5be8570bcf5d1e

### 2017-05-24 (3.19.1)

1. Fix a bug in the LEFT JOIN flattening optimization. Ticket
    cad1ab4cb7b0fc.
2. Remove a surplus semicolon that was causing problems for older versions of MSVC.
**Hashes:**
3. SQLITE_SOURCE_ID: "2017-05-24 13:08:33 f6d7b988f40217821a382bc298180e9e6794f3ed79a83c6ef5cae048989b3f86"
4. SHA3-256 for sqlite3.c: 996b2aff37b6e0c6663d0312cd921bbdf6826c989cbbb07dadde5e9672889bca

### 2017-05-22 (3.19.0)

1.  The SQLITE_READ authorizer callback is invoked once
     with a column name that is an empty string
     for every table referenced in a query from which no columns are extracted.
2.  When using an index on an expression, try to use expression values already
     available in the index, rather than loading the original columns and recomputing
     the expression.
3.  Enhance the flattening optimization so that it is able to flatten views
     on the right-hand side of a LEFT JOIN.
4.  Use replace() instead of char() for escaping newline and carriage-return
     characters embedded in strings in the .dump output from the command-line shell.
5.  Avoid unnecessary foreign key processing in UPDATE statements that do not
     touch the columns that are constrained by the foreign keys.
6.  On a DISTINCT query that uses an index, try to skip ahead to the next distinct
     entry using the index rather than stepping through rows, when an appropriate
     index is available.
7.  Avoid unnecessary invalidation of sqlite3_blob handles when making
     changes to unrelated tables.
8.  Transfer any terms of the HAVING clause that use only columns mentioned in
     the GROUP BY clause over to the WHERE clause for faster processing.
9.  Reuse the same materialization of a VIEW if that VIEW appears more than
     once in the same query.
10.  Enhance PRAGMA integrity_check so that it identifies tables that have two
     or more rows with the same rowid.
11.  Enhance the FTS5 query syntax so that column filters
     may be applied to arbitrary expressions.
12.  Enhance the json_extract() function to cache and reuse parses of JSON
     input text.
13.  Added the anycollseq.c
     loadable extension that allows a generic SQLite database connection to
     read a schema that contains unknown and/or
     application-specific collating sequences.
**Bug Fixes:**
14.  Fix a problem in REPLACE that can result in a corrupt database containing
     two or more rows with the same rowid.  Fix for ticket
     f68dc596c4e6018d.
15.  Fix a problem in PRAGMA integrity_check that was causing a subsequent
     VACUUM to behave suboptimally.
16.  Fix the PRAGMA foreign_key_check command so that it works correctly with
     foreign keys on WITHOUT ROWID tables.
17.  Fix a bug in the b-tree logic that can result in incorrect duplicate answers
     for IN operator queries.  Ticket
     61fe9745
18.  Disallow leading zeros in numeric constants in JSON.  Fix for ticket
     b93be8729a895a528e2.
19.  Disallow control characters inside of strings in JSON.  Fix for ticket
     6c9b5514077fed34551.
20.  Limit the depth of recursion for JSON objects and arrays in order to avoid
     excess stack usage in the recursive descent parser.  Fix for ticket
     981329adeef51011052.
**Hashes:**
21. SQLITE_SOURCE_ID: "2017-05-22 13:58:13 28a94eb282822cad1d1420f2dad6bf65e4b8b9062eda4a0b9ee8270b2c608e40"
22. SHA3-256 for sqlite3.c: c30326aa1a9cc342061b755725eac9270109acf878bc59200dd4b1cea6bc2908

### 2017-03-30 (3.18.0)

1. Added the PRAGMA optimize command
2. The SQLite version identifier returned by the sqlite_source_id() SQL function
    and the sqlite3_sourceid() C API and found in the SQLITE_SOURCE_ID macro is
    now a 64-digit SHA3-256 hash instead of a 40-digit SHA1 hash.
3. Added the json_patch() SQL function to the JSON1 extension.
4. Enhance the LIKE optimization so that it works for arbitrary expressions on
    the left-hand side as long as the LIKE pattern on the right-hand side does not
    begin with a digit or minus sign.
5. Added the sqlite3_set_last_insert_rowid() interface and use the new interface in
    the FTS3, FTS4, and FTS5 extensions to ensure that the sqlite3_last_insert_rowid()
    interface always returns reasonable values.
6. Enhance PRAGMA integrity_check and PRAGMA quick_check so that they verify
    CHECK constraints.
7. Enhance the query plans for joins to detect empty tables early and
    halt without doing unnecessary work.
8. Enhance the sqlite3_mprintf() family of interfaces and the printf SQL function
    to put comma separators at the thousands marks for integers, if the "," format modifier
    is used in between the "%" and the "d" (example: "%,d").
9. Added the -DSQLITE_MAX_MEMORY=*N* compile-time option.
10. Added the .sha3sum dot-command and the .selftest dot-command
    to the command-line shell
11. Begin enforcing SQLITE_LIMIT_VDBE_OP. This can be used, for example, to prevent
    excessively large prepared statements in systems that accept SQL queries from
    untrusted users.
12. Various performance improvements.
**Bug Fixes:**
13. Ensure that indexed expressions with collating sequences are handled correctly.
    Fix for ticket eb703ba7b50c1a5.
14. Fix a bug in the 'start of ...' modifiers for the date and time functions.
    Ticket 6097cb92745327a1
15. Fix a potential segfault in complex recursive triggers, resulting from a
    bug in the OP_Once opcode introduced as part of a performance optimization in
    version 3.15.0.
    Ticket 06796225f59c057c
16. In the RBU extension, add extra sync operations to avoid the possibility of
    corruption following a power failure.
17. The sqlite3_trace_v2() output for nested SQL statements should always begin
    with a "--" comment marker.
**Hashes:**
18. SQLITE_SOURCE_ID: "2017-03-28 18:48:43 424a0d380332858ee55bdebc4af3789f74e70a2b3ba1cf29d84b9b4bcf3e2e37"
19. SHA3-256 for sqlite3.c: cbf322df1f76be57fb3be84f3da1fc71d1d3dfdb7e7c2757fb0ff630b3bc2e5d

### 2017-02-13 (3.17.0)

1. Approximately 25% better performance from the R-Tree extension.
      1.  Uses compiler built-ins (ex: __builtin_bswap32() or _byteswap_ulong())
         for byteswapping when available.
    
  2.  Uses the sqlite3_blob key/value access object instead of SQL
         for pulling content out of R-Tree nodes
    
  3.  Other miscellaneous enhancements such as loop unrolling.
    
2. Add the SQLITE_DEFAULT_LOOKASIDE compile-time option.
3. Increase the default lookaside
    size from 512,125 to 1200,100
    as this provides better performance while only adding 56KB
    of extra memory per connection.  Memory-sensitive
    applications can restore the old
    default at compile-time, start-time, or run-time.
4. Use compiler built-ins __builtin_sub_overflow(), __builtin_add_overflow(),
    and __builtin_mul_overflow() when available.  (All compiler
    built-ins can be omitted with the SQLITE_DISABLE_INTRINSIC compile-time
    option.)
5. Added the SQLITE_ENABLE_NULL_TRIM compile-time option, which
    can result in significantly smaller database files for some
    applications, at the risk of being incompatible with older
    versions of SQLite.
6. Change SQLITE_DEFAULT_PCACHE_INITSZ from 100 to 20, for
    improved performance.
7. Added the SQLITE_UINT64_TYPE compile-time option as an
    analog to SQLITE_INT64_TYPE.
8. Perform some UPDATE operations in a single pass instead of
    in two passes.
9. Enhance the session extension to support WITHOUT ROWID
    tables.
10. Fixed performance problems and potential stack overflows
    when creating views from multi-row VALUES clauses with
    hundreds of thousands of rows.
11. Added the sha1.c
    extension.
12. In the command-line shell, enhance the ".mode" command so that it
    restores the default column and row separators for modes "line",
    "list", "column", and "tcl".
13. Enhance the SQLITE_DIRECT_OVERFLOW_READ option so that it works
    in WAL mode as long as the pages being read are not in the WAL file.
14. Enhance the
    Lemon parser generator
    so that it can store the parser object as a stack variable rather than
    allocating space from the heap and make use of that enhancement in
    the amalgamation.
15. Other performance improvements. Uses about 6.5% fewer CPU cycles.
**Bug Fixes:**
16. Throw an error if the ON clause of a LEFT JOIN references tables
    to the right of the ON clause.  This is the same behavior as
    PostgreSQL.  Formerly, SQLite silently converted the LEFT JOIN
    into an INNER JOIN.  Fix for ticket
    25e335f802dd.
17. Use the correct affinity for columns of automatic indexes. Ticket
    7ffd1ca1d2ad4ec.
18. Ensure that the sqlite3_blob_reopen() interface can correctly
    handle short rows.  Fix for ticket
    e6e962d6b0f06f46e.
**Hashes:**
19. SQLITE_SOURCE_ID: "2017-02-13 16:02:40 ada05cfa86ad7f5645450ac7a2a21c9aa6e57d2c"
20. SHA1 for sqlite3.c: cc7d708bb073c44102a59ed63ce6142da1f174d1

### 2017-01-06 (3.16.2)

1. Fix the REPLACE statement for
    WITHOUT ROWID tables that lack secondary indexes so that
    it works correctly with triggers and foreign keys.  This was a new bug
    caused by performance optimizations added in version 3.16.0.
    Ticket 30027b613b4
2. Fix the sqlite3_value_text() interface so that it correctly
    translates content generated by zeroblob() into a string of all
    0x00 characters.  This is a long-standing issue discovered after the
    3.16.1 release by OSS-Fuzz
3. Fix the bytecode generator to deal with a subquery in the FROM clause
    that is itself a UNION ALL where one side of the UNION ALL is a view
    that contains an ORDER BY.  This is a long-standing issue that was
    discovered after the release of 3.16.1.  See ticket
    190c2507.
4. Adjust the sqlite3_column_count() API so it more often returns the same
    values for PRAGMA statements as it did in prior releases, to
    minimize disruption to applications that might be using that
    interface in unexpected ways.
**Hashes:**
5. SQLITE_SOURCE_ID: "2017-01-06 16:32:41 a65a62893ca8319e89e48b8a38cf8a59c69a8209"
6. SHA1 for sqlite3.c: 2bebdc3f24911c0d12b6d6c0123c3f84d6946b08

### 2017-01-03 (3.16.1)

1. Fix a bug concerning the use of row values within triggers
    (see ticket 8c9458e7)
    that was in version 3.15.0 but was not reported until moments after the 3.16.0
    release was published.
**Hashes:**
2. SQLITE_SOURCE_ID: "2017-01-03 18:27:03 979f04392853b8053817a3eea2fc679947b437fd"
3. SHA1 for sqlite3.c: 354f6223490b30fd5320b4066b1535e4ce33988d

### 2017-01-02 (3.16.0)

1. Uses 9% fewer CPU cycles. (See the CPU performance measurement report for
    details on how this performance increase was computed.)
2. Added experimental support for PRAGMA functions.
3. Added the SQLITE_DBCONFIG_NO_CKPT_ON_CLOSE option to sqlite3_db_config().
4. Enhance the date and time functions so that the 'unixepoch' modifier works
    for the full span of supported dates.
5. Changed the default configuration of the lookaside memory allocator from
    500 slots of 128 bytes each into 125 slots of 512 bytes each.
6. Enhanced "WHERE x NOT NULL" partial indexes so that they are usable if
    the "x" column appears in a LIKE or GLOB operator.
7. Enhanced sqlite3_interrupt() so that it interrupts checkpoint operations that
    are in process.
8. Enhanced the LIKE and GLOB matching algorithm to be faster
    for cases when the pattern contains multiple wildcards.
9. Added the SQLITE_FCNTL_WIN32_GET_HANDLE file control opcode.
10. Added ".mode quote" to the command-line shell.
11. Added ".lint fkey-indexes" to the command-line shell.
12. Added the .imposter dot-command to the command-line shell.
13. Added the remember(V,PTR)
    SQL function as a loadable extension.
14. Rename the SQLITE_OMIT_BUILTIN_TEST compile-time option to
    SQLITE_UNTESTABLE to better reflect the implications of using it.
**Bug Fixes:**
15. Fix a long-standing bug in the query planner that caused incorrect results
    on a LEFT JOIN where the left-hand table is a subquery and the join constraint
    is a bare column name coming from the left-hand subquery.  Ticket
    2df0107b.
16. Correctly handle the integer literal -0x8000000000000000 in the query planner.
**Hashes:**
17. SQLITE_SOURCE_ID: "2017-01-02 11:57:58 04ac0b75b1716541b2b97704f4809cb7ef19cccf"
18. SHA1 for sqlite3.c: e2920fb885569d14197c9b7958e6f1db573ee669

### 2016-11-28 (3.15.2)

1.  Multiple bug fixes to the row value logic that was introduced in version 3.15.0.
2.  Fix a NULL pointer dereference in ATTACH/DETACH following a maliciously constructed
     syntax error.  Ticket
     2f1b168ab4d4844.
3.  Fix a crash that can occur following an out-of-memory condition
     in the built-in instr() function.
4.  In the JSON extension, fix the JSON validator so that it correctly rejects
     invalid backslash escapes within strings.
**Hashes:**
5. SQLITE_SOURCE_ID: "2016-11-28 19:13:37 bbd85d235f7037c6a033a9690534391ffeacecc8"
6. SHA1 for sqlite3.c: 06d77b42a3e70609f8d4bbb97caf53652f1082cb

### 2016-11-04 (3.15.1)

1.  Added SQLITE_FCNTL_WIN32_GET_HANDLE file control opcode.
2.  Fix the VACUUM command so that it spills excess content to disk rather
     than holding everything in memory, and possible causing an out-of-memory
     error for larger database files.  This fixes an issue introduced by
     version 3.15.0.
3.  Fix a case (present since 3.8.0 - 2013-08-26)
     where OR-connected terms in the ON clause of a LEFT JOIN
     might cause incorrect results.  Ticket
     34a579141b2c5ac.
4.  Fix a case where the use of row values in the ON clause of a LEFT JOIN
     might cause incorrect results.  Ticket
     fef4bb4bd9185ec8f.
**Hashes:**
5. SQLITE_SOURCE_ID: "2016-11-04 12:08:49 1136863c76576110e710dd5d69ab6bf347c65e36"
6. SHA1 for sqlite3.c: e7c26a7be3e431dd06898f8d262c4ef240c07366

### 2016-10-14 (3.15.0)

1.  Added support for row values.
2.  Allow deterministic SQL functions in the WHERE clause of a partial index.
3.  Added the "modeof=*filename* " URI parameter on the unix VFS
4.  Added support for SQLITE_DBCONFIG_MAINDBNAME.
5.  Added the ability to VACUUM an ATTACH-ed database.
6.  Enhancements to the command-line shell:
       1.  Add the ".testcase" and ".check" dot-commands.
     
  2.  Added the --new option to the ".open" dot-command, causing
          any prior content in the database to be purged prior to
          opening.
     
7.  Enhance the fts5vocab virtual table to handle "ORDER BY term" efficiently.
8.  Miscellaneous micro-optimizations reduce CPU usage by more than 7%
     on common workloads.  Most optimization in this release has been on the
     front-end (sqlite3_prepare_v2()).
**Bug Fixes:**
9.  The multiply operator now correctly detects 64-bit integer overflow
     and promotes to floating point in all corner-cases.  Fix for ticket
     1ec41379c9c1e400.
10.  Correct handling of columns with redundant unique indexes when those
     columns are used on the LHS of an IN operator.  Fix for ticket
     0eab1ac759.
11.  Skip NULL entries on range queries in indexes on expressions.
     Fix for ticket
     4baa46491212947.
12.  Ensure that the AUTOINCREMENT counters in the sqlite_sequence
     table are initialized doing "Xfer Optimization" on "INSERT ... SELECT"
     statements.  Fix for ticket
     7b3328086a5c116c.
13.  Make sure the ORDER BY LIMIT optimization
     (from check-in 559733b09e)
     works with IN operators on INTEGER PRIMARY KEYs.  Fix for ticket
     96c1454c
**Hashes:**
14. SQLITE_SOURCE_ID: "2016-10-14 10:20:30 707875582fcba352b4906a595ad89198d84711d8"
15. SHA1 for sqlite3.c: fba106f8f6493c66eeed08a2dfff0907de54ae76

### 2016-09-12 (3.14.2)

1.  Improved support for using the STDCALL calling convention in winsqlite3.dll.
2.  Fix the sqlite3_trace_v2() interface so that it is disabled if either the
callback or the mask arguments are zero, in accordance with the documentation.
3.  Fix commenting errors and improve the comments generated on EXPLAIN listings
when the -DSQLITE_ENABLE_EXPLAIN_COMMENTS compile-time option is used.
4.  Fix the ".read" command in the command-line shell so that it understands
that its input is not interactive.
5.  Correct affinity computations for a SELECT on the RHS of an IN operator.
Fix for ticket 199df4168c.
6.  The ORDER BY LIMIT optimization is not valid unless the inner-most IN operator
loop is actually used by the query plan. Fix for
ticket 0c4df46116e90f92.
7.  Fix an internal code generator problem that was causing some DELETE operations
to no-op.  Ticket ef360601
**Hashes:**
8. SQLITE_SOURCE_ID: "2016-09-12 18:50:49 29dbef4b8585f753861a36d6dd102ca634197bd6"
9. SHA1 for sqlite3.c: bcc4a1989db45e7f223191f2d0f66c1c28946383

### 2016-08-11 (3.14.1)

1. A performance enhancement to the page-cache "truncate" operation
    reduces COMMIT time by dozens of milliseconds on systems with a
    large page cache.
2. Fix to the --rbu option of sqldiff.
**Hashes:**
3. SQLITE_SOURCE_ID: "2016-08-11 18:53:32 a12d8059770df4bca59e321c266410344242bf7b"
4. SHA1 for sqlite3.c: d545b24892278272ce4e40e0567d69c8babf12ea

### 2016-08-08 (3.14)

Celebrating the SQLite "π release"
with a home-baked pie.
 
1. Added support for WITHOUT ROWID virtual tables.
2. Improved the query planner so that the OR optimization can
    be used on virtual tables even if one or more of the disjuncts
    use the LIKE, GLOB, REGEXP, MATCH operators.
3. Added the CSV virtual table for reading
    RFC 4180 formatted comma-separated
    value files.
4. Added the carray() table-valued function extension.
5. Enabled persistent loadable extensions using the new
    SQLITE_OK_LOAD_PERMANENTLY return code from the extension
    entry point.
6. Added the SQLITE_DBSTATUS_CACHE_USED_SHARED option to sqlite3_db_status().
7. Add the
    vfsstat.c
    loadable extension - a VFS shim that measures I/O
    together with an eponymous virtual table that provides access to the measurements.
8. Improved algorithm for running queries with both an ORDER BY and a LIMIT where
    only the inner-most loop naturally generates rows in the correct order.
9. Enhancements to Lemon parser generator, so that it generates a
    faster parser.
10. The PRAGMA compile_options command now attempts to show the version number
    of the compiler that generated the library.
11. Enhance PRAGMA table_info so that it provides information about
    eponymous virtual tables.
12. Added the "win32-none" VFS, analogous to the "unix-none" VFS, that works like
    the default "win32" VFS except that it ignores all file locks.
13. The query planner uses a full scan of a partial index instead of a
    full scan of the main table, in cases where that makes sense.
14. Allow table-valued functions to appear on the right-hand side of an IN operator.
15. Created the dbhash.exe command-line utility.
16. Added two new C-language interfaces: sqlite3_expanded_sql() and
    sqlite3_trace_v2().  These new interfaces subsume the functions of
    sqlite3_trace() and sqlite3_profile() which are now deprecated.
17. Added the json_quote() SQL function to the json1 extension.
18. Disable the authorizer callback while reparsing the schema.
19. Added the SQLITE_ENABLE_UNKNOWN_SQL_FUNCTION compile-time option and turned that
    option on by default when building the command-line shell.
**Bug Fixes:**
20. Fix the ALTER TABLE command so that it does not corrupt descending indexes
    when adding a column to a legacy file format database.  Ticket
    f68bf68513a1c15f
21. Fix a NULL-pointer dereference/crash that could occurs when a transitive WHERE
    clause references a non-existent collating sequence.  Ticket
    e8d439c77685eca6.
22. Improved the cost estimation for an index scan which includes a WHERE clause
    that can be partially or fully evaluated using columns in the index and without
    having to do a table lookup.  This fixes a performance regression that occurred
    for some obscure queries following the ORDER BY LIMIT optimization introduced
    in version 3.12.0.
**Hashes:**
23. SQLITE_SOURCE_ID: "2016-08-08 13:40:27 d5e98057028abcf7217d0d2b2e29bbbcdf09d6de"
24. SHA1 for sqlite3.c: 234a3275d03a287434ace3ccdf1afb208e6b0e92

### 2016-05-18 (3.13.0)

1. Postpone I/O associated with TEMP files for as long as possible, with the hope
    that the I/O can ultimately be avoided completely.
2. Merged the session extension into trunk.
3. Added the ".auth ON|OFF" command to the command-line shell.
4. Added the "--indent" option to the ".schema" and ".fullschema" commands of
    the command-line shell, to turn on pretty-printing.
5. Added the ".eqp full" option to the command-line shell, that does both EXPLAIN
    and EXPLAIN QUERY PLAN on each statement that is evaluated.
6. Improved unicode filename handling in the command-line shell on Windows.
7. Improved resistance against goofy query planner decisions caused by
    incomplete or incorrect modifications to the sqlite_stat1
    table by the application.
8. Added the sqlite3_db_config(db,SQLITE_DBCONFIG_ENABLE_LOAD_EXTENSION) interface
    which allows the sqlite3_load_extension() C-API to be enabled while keeping the
    load_extension() SQL function disabled for security.
9. Change the temporary directory search algorithm on Unix to allow directories with
    write and execute permission, but without read permission, to serve as temporary
    directories.  Apply this same standard to the "." fallback directory.
**Bug Fixes:**
10. Fix a problem with the multi-row one-pass DELETE optimization that was
    causing it to compute incorrect answers with a self-referential subquery in
    the WHERE clause.  Fix for ticket
    dc6ebeda9396087
11. Fix a possible segfault with DELETE when table is a rowid table with an
    INTEGER PRIMARY KEY and the WHERE clause contains a OR and
    the table has one or more indexes that are able to trigger the OR optimization,
    but none of the indexes reference any table columns other than the INTEGER PRIMARY KEY.
    Ticket 16c9801ceba49.
12. When checking for the WHERE-clause push-down optimization, verify that all terms
    of the compound inner SELECT are non-aggregate, not just the last term. Fix for ticket
    f7f8c97e97597.
13. Fix a locking race condition in Windows that can occur when two or more processes
    attempt to recover the same hot journal at the same time.
**Hashes:**
14. SQLITE_SOURCE_ID: "2016-05-18 10:57:30 fc49f556e48970561d7ab6a2f24fdd7d9eb81ff2"
15. SHA1 for sqlite3.c: 9b9171b1e6ce7a980e6b714e9c0d9112657ad552
**Bug fixes backported into patch release 3.12.2 (2016-04-18):**
16. Fix a backwards compatibility problem in version 3.12.0 and 3.12.1:
    Columns declared as `"INTEGER" PRIMARY KEY` (with quotes around
    the datatype keyword) were not being recognized as an
    INTEGER PRIMARY KEY, which resulted in an incompatible database file.
    Ticket 7d7525cb01b68
17. Fix a bug (present since version 3.9.0) that can cause the DELETE
    operation to miss rows if PRAGMA reverse_unordered_selects is turned on.
    Ticket a306e56ff68b8fa5
18. Fix a bug in the code generator that can cause incorrect results if
    two or more virtual tables are joined and the virtual table used in
    outer loop of the join has an IN operator constraint.
19. Correctly interpret negative "PRAGMA cache_size" values when determining
    the cache size used for sorting large amounts of data.
**Bug fixes backported into patch release 3.12.1 (2016-04-08):**
20. Fix a boundary condition error introduced by version 3.12.0
    that can result in a crash during heavy SAVEPOINT usage.
    Ticket 7f7f8026eda38.
21. Fix views so that they inherit column datatypes from the
    table that they are defined against, when possible.
22. Fix the query planner so that IS and IS NULL operators are able
    to drive an index on a LEFT OUTER JOIN.

### 2016-04-18 (3.12.2)

1. Fix a backwards compatibility problem in version 3.12.0 and 3.12.1:
    Columns declared as `"INTEGER" PRIMARY KEY` (with quotes around
    the datatype keyword) were not being recognized as an
    INTEGER PRIMARY KEY, which resulted in an incompatible database file.
    Ticket 7d7525cb01b68
2. Fix a bug (present since version 3.9.0) that can cause the DELETE
    operation to miss rows if PRAGMA reverse_unordered_selects is turned on.
    Ticket a306e56ff68b8fa5
3. Fix a bug in the code generator that can cause incorrect results if
    two or more virtual tables are joined and the virtual table used in
    outer loop of the join has an IN operator constraint.
4. Correctly interpret negative "PRAGMA cache_size" values when determining
    the cache size used for sorting large amounts of data.
**Hashes:**
5. SQLITE_SOURCE_ID: "2016-04-18 17:30:31 92dc59fd5ad66f646666042eb04195e3a61a9e8e"
6. SHA1 for sqlite3.c: de5a5898ebd3a3477d4652db143746d008b24c83

### 2016-04-08 (3.12.1)

1. Fix a boundary condition error introduced by version 3.12.0
    that can result in a crash during heavy SAVEPOINT usage.
    Ticket 7f7f8026eda38.
2. Fix views so that they inherit column datatypes from the
    table that they are defined against, when possible.
3. Fix the query planner so that IS and IS NULL operators are able
    to drive an index on a LEFT OUTER JOIN.
**Hashes:**
4. SQLITE_SOURCE_ID: "2016-04-08 15:09:49 fe7d3b75fe1bde41511b323925af8ae1b910bc4d"
5. SHA1 for sqlite3.c: ebb18593350779850e3e1a930eb84a70fca8c1d1

### 2016-04-01 (3.9.3)

1. Backport a
    simple query planner optimization
    that allows the IS operator
    to drive an index on a LEFT OUTER JOIN.  No other changes from the
    version 3.9.2 baseline.

### 2016-03-29 (3.12.0)

**Potentially Disruptive Change:**

1. The SQLITE_DEFAULT_PAGE_SIZE is increased from 1024 to 4096.
    The SQLITE_DEFAULT_CACHE_SIZE is changed from 2000 to -2000 so
    the same amount of cache memory is used by default.
    See the application note on the
    version 3.12.0 page size change for further information.
**Performance enhancements:**
2. Enhancements to the Lemon parser generator
    so that it creates a smaller and faster SQL parser.
3. Only create master journal files if two or more attached databases are all
    modified, do not have PRAGMA synchronous set to OFF, and
    do not have the journal_mode set to OFF, MEMORY, or WAL.
4. Only create statement journal files when their size exceeds a threshold.
    Otherwise the journal is held in memory and no I/O occurs.  The threshold
    can be configured at compile-time using SQLITE_STMTJRNL_SPILL or at
    start-time using sqlite3_config(SQLITE_CONFIG_STMTJRNL_SPILL).
5. The query planner is able to optimize IN operators on virtual tables
    even if the xBestIndex method does not set the
    sqlite3_index_constraint_usage.omit flag of the
    virtual table column to the left of the IN operator.
6. The query planner now does a better job of optimizing virtual table
    accesses in a 3-way or higher join where constraints on the virtual
    table are split across two or more other tables of the join.
7. More efficient handling of application-defined SQL functions, especially
    in cases where the application defines hundreds or thousands of
    custom functions.
8. The query planner considers the LIMIT clause when estimating the cost
    of ORDER BY.
9. The configure script (on unix) automatically detects
    pread() and pwrite() and sets compile-time options to use those OS
    interfaces if they are available.
10. Reduce the amount of memory needed to hold the schema.
11. Other miscellaneous micro-optimizations for improved performance and reduced
    memory usage.
**New Features:**
12. Added the SQLITE_DBCONFIG_ENABLE_FTS3_TOKENIZER option to sqlite3_db_config()
    which allows the two-argument version of the fts3_tokenizer() SQL function to
    be enabled or disabled at run-time.
13. Added the sqlite3rbu_bp_progress()
    interface to the RBU extension.
14. The PRAGMA defer_foreign_keys=ON statement now also disables
    RESTRICT actions on foreign key.
15. Added the sqlite3_system_errno() interface.
16. Added the SQLITE_DEFAULT_SYNCHRONOUS and SQLITE_DEFAULT_WAL_SYNCHRONOUS
    compile-time options.  The SQLITE_DEFAULT_SYNCHRONOUS compile-time option
    replaces the SQLITE_EXTRA_DURABLE option, which is no longer supported.
17. Enhanced the ".stats" command in the command-line shell to show more
    information about I/O performance obtained from /proc, when available.
**Bug fixes:**
18. Make sure the sqlite3_set_auxdata() values from multiple triggers
    within a single statement do not interfere with one another.
    Ticket dc9b1c91.
19. Fix the code generator for expressions of the form "x IN (SELECT...)" where
    the SELECT statement on the RHS is a correlated subquery.
    Ticket 5e3c886796e5512e.
20. Fix a harmless TSAN warning associated with the sqlite3_db_readonly() interface.
**Hashes:**
21. SQLITE_SOURCE_ID: "2016-03-29 10:14:15 e9bb4cf40f4971974a74468ef922bdee481c988b"
22. SHA1 for sqlite3.c: cba2be96d27cb51978cd4a200397a4ad178986eb

### 2016-03-03 (3.11.1)

1. Improvements to the Makefiles and build scripts used by VisualStudio.
2. Fix an FTS5 issue in which the 'optimize' command could cause index corruption.
3. Fix a buffer overread that might occur if FTS5 is used to query a corrupt
    database file.
4. Increase the maximum "scope" value for the spellfix1 extension from 6 to 30.
5. SQLITE_SOURCE_ID: "2016-03-03 16:17:53 f047920ce16971e573bc6ec9a48b118c9de2b3a7"
6. SHA1 for sqlite3.c: 3da832fd2af36eaedb05d61a8f4c2bb9f3d54265

### 2016-02-15 (3.11.0)

**General improvements:**

1. Enhanced WAL mode so that it works efficiently with transactions that are
    larger than the cache_size.
2. Added the FTS5 detail option.
3. Added the "EXTRA" option to PRAGMA synchronous that does a sync of the
    containing directory when a rollback journal is unlinked in DELETE mode,
    for better durability.  The SQLITE_EXTRA_DURABLE compile-time option enables
    PRAGMA synchronous=EXTRA by default.
4. Enhanced the query planner so that it is able to use
    a covering index as part of the OR optimization.
5. Avoid recomputing NOT NULL and CHECK constraints on unchanged
    columns in UPDATE statement.
6. Many micro-optimizations, resulting in a library that is
    faster than the previous release.
**Enhancements to the command-line shell:**
7. By default, the shell is now in "auto-explain" mode.  The output of
    EXPLAIN commands is automatically formatted.
8. Added the ".vfslist" dot-command.
9. The SQLITE_ENABLE_EXPLAIN_COMMENTS compile-time option is now turned
    on by default in the standard builds.
**Enhancements to the TCL Interface:**
10. If a database connection is opened with the "-uri 1" option, then
    URI filenames are honored by the "backup" and "restore" commands.
11. Added the "-sourceid" option to the "sqlite3" command.
**Makefile improvements:**
12. Improved pthreads detection in configure scripts.
13. Add the ability to do MSVC Windows builds from the amalgamation tarball.
**Bug fixes**
14. Fix an issue with incorrect sharing of VDBE temporary registers between
    co-routines that could cause incorrect query results in obscure cases.  Ticket
    d06a25c84454a.
15. Fix a problem in the sqlite3_result_subtype() interface that could
    cause problems for the json1 extension under obscure circumstances.
    Fix for ticket
    f45ac567eaa9f9.
16. Escape control characters in JSON strings.  Fix for ticket
    ad2559db380abf8.
17. Reenable the xCurrentTime and xGetLastError methods in the built-in
    unix VFSes as long as SQLITE_OMIT_DEPRECATED is not defined.
**Backwards Compatibility:**
18. Because of continuing security concerns, the two-argument version
    of the seldom-used and little-known fts3_tokenizer() function is
    disabled unless SQLite is compiled with the SQLITE_ENABLE_FTS3_TOKENIZER.
**Hashes:**
19. SQLITE_SOURCE_ID: "2016-02-15 17:29:24 3d862f207e3adc00f78066799ac5a8c282430a5f"
20. SHA1 for sqlite3.c: df01436c5fcfe72d1a95bc172158219796e1a90b

### 2016-01-20 (3.10.2)

**Critical bug fix:**

1. Version 3.10.0 introduced a case-folding bug in the LIKE operator which is fixed
    by this patch release.  Ticket
    80369eddd5c94.
**Other miscellaneous bug fixes:**
2. Fix a use-after-free that can occur when SQLite is compiled with -DSQLITE_HAS_CODEC.
3. Fix the build so that it works with -DSQLITE_OMIT_WAL.
4. Fix the configure script for the amalgamation so that the --readline option works again
    on Raspberry PIs.
**Hashes:**
5. SQLITE_SOURCE_ID: "2016-01-20 15:27:19 17efb4209f97fb4971656086b138599a91a75ff9"
6. SHA1 for sqlite3.c: f7088b19d97cd7a1c805ee95c696abd54f01de4f

### 2016-01-14 (3.10.1)

**New feature:**

1. Add the SQLITE_FCNTL_JOURNAL_POINTER file control.
**Bug fix:**
2. Fix a 16-month-old bug in the query planner that could generate incorrect results
    when a scalar subquery attempts to use the block sorting optimization.  Ticket
    cb3aa0641d9a4.
**Hashes:**
3. SQLITE_SOURCE_ID: "2016-01-13 21:41:56 254419c36766225ca542ae873ed38255e3fb8588"
4. SHA1 for sqlite3.c: 1398ba8e4043550a533cdd0834bfdad1c9eab0f4

### 2016-01-06 (3.10.0)

**General improvements:**

1. Added support for LIKE, GLOB, and REGEXP operators on virtual tables.
2. Added the colUsed field to sqlite3_index_info for use by
    the sqlite3_module.xBestIndex method.
3. Enhance the PRAGMA cache_spill statement to accept a 32-bit integer
    parameter which is the threshold below which cache spilling is prohibited.
4. On unix, if a symlink to a database file is opened, then the corresponding
    journal files are based on the actual filename, not the symlink name.
5. Added the "--transaction" option to sqldiff.
6. Added the sqlite3_db_cacheflush() interface.
7. Added the sqlite3_strlike() interface.
8. When using memory-mapped I/O map the database file read-only so that stray pointers
    and/or array overruns in the application cannot accidentally modify the database file.
9. Added the *experimental* sqlite3_snapshot_get(), sqlite3_snapshot_open(),
    and sqlite3_snapshot_free() interfaces.  These are subject to change or removal in
    a subsequent release.
10. Enhance the 'utc' modifier in the date and time functions so that it is a no-op if
    the date/time is known to already be in UTC.  (This is not a compatibility break since
    the behavior has long been documented as "undefined" in that case.)
11. Added the json_group_array() and json_group_object() SQL functions in the
    json extension.
12. Added the SQLITE_LIKE_DOESNT_MATCH_BLOBS compile-time option.
13. Many small performance optimizations.
**Portability enhancements:**
14. Work around a sign-extension bug in the optimizer of the HP C compiler on HP/UX.
    (details)
**Enhancements to the command-line shell:**
15. Added the ".changes ON|OFF" and ".vfsinfo" dot-commands.
16. Translate between MBCS and UTF8 when
    running in cmd.exe on Windows.
**Enhancements to makefiles:**
17. Added the --enable-editline and --enable-static-shell options
    to the various autoconf-generated configure scripts.
18. Omit all use of "awk" in the makefiles, to make building easier for MSVC users.
**Important fixes:**
19. Fix inconsistent integer to floating-point comparison operations that
    could result in a corrupt index if the index is created on a table
    column that contains both large integers and floating point values
    of similar magnitude.  Ticket
    38a97a87a6.
20. Fix an infinite-loop in the query planner that could occur on
    malformed common table expressions.
21. Various bug fixes in the sqldiff tool.
**Hashes:**
22. SQLITE_SOURCE_ID: "2016-01-06 11:01:07 fd0a50f0797d154fefff724624f00548b5320566"
23. SHA1 for sqlite3.c: b92ca988ebb6df02ac0c8f866dbf3256740408ac

### 2015-11-02 (3.9.2)

1. Fix the schema parser so that it interprets certain
    (obscure and ill-formed)
    CREATE TABLE statements the same as legacy.  Fix for ticket
    ac661962a2aeab3c331
2. Fix a query planner problem that could result in an incorrect
    answer due to the use of automatic indexing in subqueries in
    the FROM clause of a correlated scalar subqueries.  Fix for ticket
    8a2adec1.
3. SQLITE_SOURCE_ID: "2015-11-02 18:31:45 bda77dda9697c463c3d0704014d51627fceee328"
4. SHA1 for sqlite3.c: 1c4013876f50bbaa3e6f0f98e0147c76287684c1

### 2015-10-16 (3.9.1)

1. Fix the json1 extension so that it does __not__ recognize ASCII form-feed as a
    whitespace character, in order to comply with RFC-7159.  Fix for ticket
    57eec374ae1d0a1d
2. Add a few #ifdef and build script changes to address compilation issues that
    appeared after the 3.9.0 release.
3. SQLITE_SOURCE_ID: ""2015-10-16 17:31:12 767c1727fec4ce11b83f25b3f1bfcfe68a2c8b02"
4. SHA1 for sqlite3.c: 5e6d1873a32d82c2cf8581f143649940cac8ae49

### 2015-10-14 (3.9.0)

**Policy Changes:**

1. The version numbering conventions for SQLite are revised to use the
    emerging standard of semantic versioning.
**New Features And Enhancements:**
2. Added the json1 extension module in the source tree, and in the amalgamation.
    Enable support using the SQLITE_ENABLE_JSON1 compile-time option.
3. Added Full Text Search version 5 (FTS5) to the amalgamation, enabled
    using SQLITE_ENABLE_FTS5.  FTS5 will be considered "experimental" (subject
    to incompatible changes) for at least one more release cycle.
4. The CREATE VIEW statement now accepts an optional list of
    column names following the view name.
5. Added support for indexes on expressions.
6. Added support for table-valued functions in the FROM clause of a
    SELECT statement.
7. Added support for eponymous virtual tables.
8. A VIEW may now reference undefined tables and functions when
    initially created.  Missing tables and functions are reported when
    the VIEW is used in a query.
9. Added the sqlite3_value_subtype() and sqlite3_result_subtype()
    interfaced (used by the json1 extension).
10. The query planner is now able to use partial indexes that contain
    AND-connected terms in the WHERE clause.
11. The sqlite3_analyzer.exe utility is updated to report the depth of
    each btree and to show the average fanout for indexes and
    WITHOUT ROWID tables.
12. Enhanced the dbstat virtual table so that it can be used as a
    table-valued function where the argument is the schema to be
    analyzed.
**Other changes:**
13. The sqlite3_memory_alarm() interface, which has been deprecated and
    undocumented for 8 years, is changed into a no-op.
**Important fixes:**
14. Fixed a critical bug in the
    SQLite Encryption Extension that
    could cause the database to become unreadable and unrecoverable if a VACUUM command
    changed the size of the encryption nonce.
15. Added a memory barrier in the implementation of
    sqlite3_initialize() to help ensure that it is thread-safe.
16. Fix the OR optimization so that it always ignores subplans that
    do not use an index.
17. Do not apply the WHERE-clause push-down optimization on terms that originate
    in the ON or USING clause of a LEFT JOIN.  Fix for ticket
    c2a19d81652f40568c.
18. SQLITE_SOURCE_ID: "2015-10-14 12:29:53 a721fc0d89495518fe5612e2e3bbc60befd2e90d"
19. SHA1 for sqlite3.c: c03e47e152ddb9c342b84ffb39448bf4a2bd4288

### 2015-07-29 (3.8.11.1)

1. Restore an undocumented side-effect of PRAGMA cache_size: force
    the database schema to be parsed if the database has not been previously accessed.
2. Fix a long-standing problem in sqlite3_changes() for WITHOUT ROWID
    tables that was reported a few hours after the 3.8.11 release.
3. SQLITE_SOURCE_ID: "2015-07-29 20:00:57 cf538e2783e468bbc25e7cb2a9ee64d3e0e80b2f"
4. SHA1 for sqlite3.c: 3be71d99121fe5b17f057011025bcf84e7cc6c84

### 2015-07-27 (3.8.11)

1. Added the experimental RBU extension.  Note that this extension is experimental
    and subject to change in incompatible ways.
2. Added the experimental FTS5 extension.  Note that this extension is experimental
    and subject to change in incompatible ways.
3. Added the sqlite3_value_dup() and sqlite3_value_free() interfaces.
4. Enhance the spellfix1 extension to support ON CONFLICT clauses.
5. The IS operator is now able to drive indexes.
6. Enhance the query planner to permit automatic indexing on FROM-clause
    subqueries that are implemented by co-routine.
7. Disallow the use of "rowid" in common table expressions.
8. Added the PRAGMA cell_size_check command for better and earlier
    detection of database file corruption.
9. Added the matchinfo 'b' flag to the matchinfo() function in FTS3.
10. Improved fuzz-testing of database files, with fixes for problems found.
11. Add the fuzzcheck test program and automatically run this program
    using both SQL and database test cases on "make test".
12. Added the SQLITE_MUTEX_STATIC_VFS1 static mutex and use it in the
    Windows VFS.
13. The sqlite3_profile() callback is invoked (by sqlite3_reset() or
    sqlite3_finalize()) for statements that did not run to completion.
14. Enhance the page cache so that it can preallocate a block of memory to
    use for the initial set page cache lines.  Set the default preallocation
    to 100 pages.  Yields about a 5% performance increase on common workloads.
15. Miscellaneous micro-optimizations result in 22.3% more work for the same
    number of CPU cycles relative to the previous release.
    SQLite now runs twice as fast as version 3.8.0 and three times as
    fast as version 3.3.9.
    (Measured using
    cachegrind on the
    speedtest1.c workload on
    Ubuntu 14.04 x64 with gcc 4.8.2 and -Os. Your performance may vary.)
16. Added the sqlite3_result_zeroblob64() and sqlite3_bind_zeroblob64()
    interfaces.
**Important bug fixes:**
17. Fix CREATE TABLE AS so that columns of type TEXT never end up
    holding an INT value.  Ticket
    f2ad7de056ab1dc9200
18. Fix CREATE TABLE AS so that it does not leave NULL entries in the
    sqlite_master table if the SELECT statement on the right-hand side
    aborts with an error.  Ticket
    873cae2b6e25b
19. Fix the skip-scan optimization so that it works correctly when
    the OR optimization is used on WITHOUT ROWID tables.  Ticket
    8fd39115d8f46
20. Fix the sqlite3_memory_used() and sqlite3_memory_highwater() interfaces
    so that they actually do provide a 64-bit answer.
**Hashes:**
21. SQLITE_SOURCE_ID: "2015-07-27 13:49:41 b8e92227a469de677a66da62e4361f099c0b79d0"
22. SHA1 for sqlite3.c: 719f6891abcd9c459b5460b191d731cd12a3643e

### 2015-05-20 (3.8.10.2)

1. Fix an index corruption issue introduced by version 3.8.7.  An index
    with a TEXT key can be corrupted by an INSERT into the corresponding
    table if the table has two nested triggers that convert the key value to INTEGER
    and back to TEXT again.
    Ticket 34cd55d68e0
2. SQLITE_SOURCE_ID: "2015-05-20 18:17:19 2ef4f3a5b1d1d0c4338f8243d40a2452cc1f7fe4"
3. SHA1 for sqlite3.c: 638abb77965332c956dbbd2c8e4248e84da4eb63

### 2015-05-09 (3.8.10.1)

1. Make sqlite3_compileoption_used() responsive to the SQLITE_ENABLE_DBSTAT_VTAB
    compile-time option.
2. Fix a harmless warning in the command-line shell on some versions of MSVC.
3. Fix minor issues with the dbstat virtual table.
4. SQLITE_SOURCE_ID: "2015-05-09 12:14:55 05b4b1f2a937c06c90db70c09890038f6c98ec40"
5. SHA1 for sqlite3.c: 85e4e1c08c7df28ef61bb9759a0d466e0eefbaa2

### 2015-05-07 (3.8.10)

1. Added the sqldiff.exe utility program for computing the differences between two
    SQLite database files.
2. Added the matchinfo y flag to the
    matchinfo() function of FTS3.
3. Performance improvements for ORDER BY, VACUUM, CREATE INDEX,
    PRAGMA integrity_check, and PRAGMA quick_check.
4. Fix many obscure problems discovered while SQL fuzzing.
5. Identify all methods for important objects in the interface documentation.
    (example)
6. Made the American Fuzzy Lop fuzzer
    a standard part of SQLite's testing strategy.
7. Add the ".binary" and ".limits" commands to the command-line shell.
8. Make the dbstat virtual table part of standard builds when
    compiled with the SQLITE_ENABLE_DBSTAT_VTAB option.
9. SQLITE_SOURCE_ID: "2015-05-07 11:53:08 cf975957b9ae671f34bb65f049acf351e650d437"
10. SHA1 for sqlite3.c: 0b34f0de356a3f21b9dfc761f3b7821b6353c570

### 2015-04-08 (3.8.9)

1. Add VxWorks-7 as an officially supported and tested platform.
2. Added the sqlite3_status64() interface.
3. Fix memory size tracking so that it works even if SQLite uses more
    than 2GiB of memory.
4. Added the PRAGMA index_xinfo command.
5. Fix a potential 32-bit integer overflow problem in the
    sqlite3_blob_read() and sqlite3_blob_write() interfaces.
6. Ensure that prepared statements automatically reset on extended
    error codes of SQLITE_BUSY and SQLITE_LOCKED even when compiled
    using SQLITE_OMIT_AUTORESET.
7. Correct miscounts in the sqlite3_analyzer.exe utility related
    to WITHOUT ROWID tables.
8. Added the ".dbinfo" command to the command-line shell.
9. Improve the performance of fts3/4 queries that use the OR operator
    and at least one auxiliary fts function.
10. Fix a bug in the fts3 snippet() function causing it to omit
    leading separator characters from snippets that begin with the
    first token in a column.
11. SQLITE_SOURCE_ID: "2015-04-08 12:16:33 8a8ffc862e96f57aa698f93de10dee28e69f6e09"
12. SHA1 for sqlite3.c: 49f1c3ae347e1327b5aaa6c7f76126bdf09c6f42

### 2015-02-25 (3.8.8.3)

1. Fix a bug (ticket
    2326c258d02ead33) that can lead
    to incorrect results if the qualifying constraint of a partial index appears in the
    ON clause of a LEFT JOIN.
2. Added the ability to link against the
    "linenoise"
    command-line editing library in unix builds of the command-line shell.
3. SQLITE_SOURCE_ID: "2015-02-25 13:29:11 9d6c1880fb75660bbabd693175579529785f8a6b"
4. SHA1 for sqlite3.c: 74ee38c8c6fd175ec85a47276dfcefe8a262827a

### 2015-01-30 (3.8.8.2)

1. Enhance sqlite3_wal_checkpoint_v2(TRUNCATE) interface so that it truncates the
    WAL file even if there is no checkpoint work to be done.
2. SQLITE_SOURCE_ID: "2015-01-30 14:30:45 7757fc721220e136620a89c9d28247f28bbbc098"
3. SHA1 for sqlite3.c: 85ce79948116aa9a087ec345c9d2ce2c1d3cd8af

### 2015-01-20 (3.8.8.1)

1. Fix a bug in the sorting logic, present since version 3.8.4, that can cause
    output to appear in the wrong order on queries that contains an ORDER BY clause,
    a LIMIT clause, and that have approximately 60 or more columns in the result set.
    Ticket f97c4637102a3ae72b79.
2. SQLITE_SOURCE_ID: "2015-01-20 16:51:25 f73337e3e289915a76ca96e7a05a1a8d4e890d55"
3. SHA1 for sqlite3.c: 33987fb50dcc09f1429a653d6b47672f5a96f19e

### 2015-01-16 (3.8.8)

**New Features:**

1. Added the PRAGMA data_version command that can be used to determine if
    a database file has been modified by another process.
2. Added the SQLITE_CHECKPOINT_TRUNCATE option to the
    sqlite3_wal_checkpoint_v2() interface, with corresponding enhancements
    to PRAGMA wal_checkpoint.
3. Added the sqlite3_stmt_scanstatus() interface, available only when
    compiled with SQLITE_ENABLE_STMT_SCANSTATUS.
4. The sqlite3_table_column_metadata() is enhanced to work correctly on
    WITHOUT ROWID tables and to check for the existence of a
    a table if the column name parameter is NULL.  The interface is now
    also included in the build by default, without requiring
    the SQLITE_ENABLE_COLUMN_METADATA compile-time option.
5. Added the SQLITE_ENABLE_API_ARMOR compile-time option.
6. Added the SQLITE_REVERSE_UNORDERED_SELECTS compile-time option.
7. Added the SQLITE_SORTER_PMASZ compile-time option and SQLITE_CONFIG_PMASZ
    start-time option.
8. Added the SQLITE_CONFIG_PCACHE_HDRSZ option to sqlite3_config()
    which makes it easier for applications to determine the appropriate
    amount of memory for use with SQLITE_CONFIG_PAGECACHE.
9. The number of rows in a VALUES clause is no longer limited by
    SQLITE_LIMIT_COMPOUND_SELECT.
10. Added the eval.c
    loadable extension that implements an eval() SQL function that will recursively
    evaluate SQL.
**Performance Enhancements:**
11. Reduce the number of memcpy() operations involved in balancing a b-tree,
    for 3.2% overall performance boost.
12. Improvements to cost estimates for the skip-scan optimization.
13. The automatic indexing optimization is now capable of generating
    a partial index if that is appropriate.
**Bug fixes:**
14. Ensure durability following a power loss with
    "PRAGMA journal_mode=TRUNCATE" by calling fsync() right after truncating
    the journal file.
15. The query planner now recognizes that any column in the right-hand
    table of a LEFT JOIN can be NULL, even if that column has a NOT NULL
    constraint.  Avoid trying to optimize out NULL tests in those cases.
    Fix for ticket
    6f2222d550f5b0ee7ed.
16. Make sure ORDER BY puts rows in ascending order even if the DISTINCT
    operator  is implemented using a descending index.  Fix for ticket
    c5ea805691bfc4204b1cb9e.
17. Fix data races that might occur under stress when running with many threads
    in shared cache mode where some of the threads are opening and
    closing connections.
18. Fix obscure crash bugs found by
    american fuzzy lop.  Ticket
    a59ae93ee990a55.
19. Work around a GCC optimizer bug (for gcc 4.2.1 on MacOS 10.7) that caused the
    R-Tree extension to compute incorrect results when compiled with -O3.
**Other changes:**
20. Disable the use of the strchrnul() C-library routine unless it is
    specifically enabled using the -DHAVE_STRCHRNULL compile-time option.
21. Improvements to the effectiveness and accuracy of the
    likelihood(), likely(), and unlikely() SQL hint functions.
22. SQLITE_SOURCE_ID: "2015-01-16 12:08:06 7d68a42face3ab14ed88407d4331872f5b243fdf"
23. SHA1 for sqlite3.c: 91aea4cc722371d58aae3d22e94d2a4165276905

### 2014-12-09 (3.8.7.4)

1. Bug fix: Add in a mutex that was omitted from the previous release.
2. SQLITE_SOURCE_ID: "2014-12-09 01:34:36 f66f7a17b78ba617acde90fc810107f34f1a1f2e"
3. SHA1 for sqlite3.c: 0a56693a3c24aa3217098afab1b6fecccdedfd23

### 2014-12-05 (3.8.7.3)

1. Bug fix: Ensure the cached KeyInfo objects (an internal abstraction not visible to the
    application) do not go stale when operating in shared cache mode and frequently closing
    and reopening some database connections while leaving other database connections on the
    same shared cache open continuously.  Ticket
    e4a18565a36884b00edf.
2. Bug fix: Recognize that any column in the right-hand table of a LEFT JOIN can be
    NULL even if the column has a NOT NULL constraint.  Do not apply optimizations that
    assume the column is never NULL.  Ticket
    6f2222d550f5b0ee7ed.
3. SQLITE_SOURCE_ID: "2014-12-05 22:29:24 647e77e853e81a5effeb4c33477910400a67ba86"
4. SHA1 for sqlite3.c: 3ad2f5ba3a4a3e3e51a1dac9fda9224b359f0261

### 2014-11-18 (3.8.7.2)

1. Enhance the ROLLBACK command so that pending queries are allowed to continue as long
    as the schema is unchanged.  Formerly, a ROLLBACK would cause all pending queries to
    fail with an SQLITE_ABORT or SQLITE_ABORT_ROLLBACK error.  That error is still returned
    if the ROLLBACK modifies the schema.
2. Bug fix: Make sure that NULL results from OP_Column are fully and completely NULL and
    do not have the MEM_Ephem bit set.
    Ticket 094d39a4c95ee4.
3. Bug fix:  The %c format in sqlite3_mprintf() is able to handle precisions greater than 70.
4. Bug fix:  Do not automatically remove the DISTINCT keyword from a SELECT that forms
    the right-hand side of an IN operator since it is necessary if the SELECT also
    contains a LIMIT.
    Ticket db87229497.
5. SQLITE_SOURCE_ID: "2014-11-18 20:57:56 2ab564bf9655b7c7b97ab85cafc8a48329b27f93"
6. SHA1 for sqlite3.c: b2a68d5783f48dba6a8cb50d8bf69b238c5ec53a

### 2014-10-29 (3.8.7.1)

1. In PRAGMA journal_mode=TRUNCATE mode, call fsync() immediately after truncating
    the journal file to ensure that the transaction is durable across a power loss.
2. Fix an assertion fault that can occur when updating the NULL value of a field
    at the end of a table that was added using ALTER TABLE ADD COLUMN.
3. Do not attempt to use the strchrnul() function from the standard C library unless
    the HAVE_STRCHRNULL compile-time option is set.
4. Fix a couple of problems associated with running an UPDATE or DELETE on a
    VIEW with a rowid in the WHERE clause.
5. SQLITE_SOURCE_ID: "2014-10-29 13:59:56 3b7b72c4685aa5cf5e675c2c47ebec10d9704221"
6. SHA1 for sqlite3.c: 2d25bd1a73dc40f538f3a81c28e6efa5999bdf0c

### 2014-10-17 (3.8.7)

**Performance Enhancements:**

1. Many micro-optimizations result in 20.3% more work for the same number
    of CPU cycles relative to the previous release.
    The cumulative performance increase since version 3.8.0 is 61%.
    (Measured using
    cachegrind on the
    speedtest1.c workload on
    Ubuntu 13.10 x64 with gcc 4.8.1 and -Os. Your performance may vary.)
2. The sorter can use auxiliary helper threads to increase real-time response.
    This feature is off by default and may be
    enabled using the PRAGMA threads command or the SQLITE_DEFAULT_WORKER_THREADS
    compile-time option.
3. Enhance the skip-scan optimization so that it is able to skip index terms that
    occur in the middle of the index, not just as the left-hand side of the index.
4. Improved optimization of CAST operators.
5. Various improvements in how the query planner uses sqlite_stat4
    information to estimate plan costs.
**New Features:**
6. Added new interfaces with 64-bit length parameters:
    sqlite3_malloc64(),
    sqlite3_realloc64(),
    sqlite3_bind_blob64(),
    sqlite3_result_blob64(),
    sqlite3_bind_text64(), and
    sqlite3_result_text64().
7. Added the new interface sqlite3_msize() that returns the size of a memory allocation
    obtained from sqlite3_malloc64() and its variants.
8. Added the SQLITE_LIMIT_WORKER_THREADS option to sqlite3_limit() and
    PRAGMA threads command for configuring the number of available worker threads.
9. The spellfix1 extension allows the application to optionally specify the rowid for
    each INSERT.
10. Added the User Authentication
    extension.
**Bug Fixes:**
11. Fix a bug in the partial index implementation that might result in an incorrect
    answer if a partial index is used in a subquery or in a view.
    Ticket 98d973b8f5.
12. Fix a query planner bug that might cause a table to be scanned in the wrong direction
    (thus reversing the order of output) when a DESC index is used to implement the ORDER BY
    clause on a query that has an identical GROUP BY clause.
    Ticket ba7cbfaedc7e6.
13. Fix a bug in sqlite3_trace() that was causing it to sometimes fail to print
    an SQL statement if that statement needed to be re-prepared.
    Ticket 11d5aa455e0d98f3c1e6a08
14. Fix a faulty assert() statement.
    Ticket 369d57fb8e5ccdff06f1
**Test, Debug, and Analysis Changes:**
15. Show ASCII-art abstract syntax tree diagrams using the ".selecttrace"
    and ".wheretrace" commands in the
    command-line shell when compiled with SQLITE_DEBUG, SQLITE_ENABLE_SELECTTRACE,
    and SQLITE_ENABLE_WHERETRACE.  Also provide the sqlite3TreeViewExpr() and
    sqlite3TreeViewSelect() entry points that can be invoked from with the
    debugger to show the parse tree when stopped at a breakpoint.
16. Drop support for SQLITE_ENABLE_TREE_EXPLAIN.  The SELECTTRACE mechanism provides
    more useful diagnostics information.
17. New options to the command-line shell for configuring auxiliary
    memory usage: --pagecache, --lookaside, and --scratch.
18. SQLITE_SOURCE_ID: "2014-10-17 11:24:17 e4ab094f8afce0817f4074e823fabe59fc29ebb4"
19. SHA1 for sqlite3.c: 56dcf5e931a9e1fa12fc2d600cd91d3bf9b639cd

### 2014-08-15 (3.8.6)

1. Added support for hexadecimal integer literals in the SQL parser.
    (Ex: 0x123abc)
2. Enhanced the PRAGMA integrity_check command to detect UNIQUE and
    NOT NULL constraint violations.
3. Increase the maximum value of SQLITE_MAX_ATTACHED from 62 to 125.
4. Increase the timeout in WAL mode before issuing an SQLITE_PROTOCOL
    error from 1 second to 10 seconds.
5. Added the likely(X) SQL function.
6. The unicode61 tokenizer is now included in FTS4 by default.
7. Trigger automatic reprepares on all prepared statements when ANALYZE is
    run.
8. Added a new
    loadable extension source code file to the source tree:
    fileio.c
9. Add extension functions readfile(X) and writefile(X,Y)
    (using code copy/pasted from fileio.c in the previous bullet) to the
    command-line shell.
10. Added the .fullschema dot-command to the command-line shell.
**Performance Enhancements:**
11. Deactivate the DISTINCT keyword on subqueries on the
    right-hand side of the IN operator.
12. Add the capability of evaluating an IN operator as a sequence
    of comparisons as an alternative to using a table lookup.  Use the sequence
    of comparisons implementation in circumstances where it is likely to be
    faster, such as when the right-hand side of the IN operator
    is small and/or changes frequently.
13. The query planner now uses sqlite_stat4 information (created by ANALYZE)
    to help determine if the skip-scan optimization is appropriate.
14. Ensure that the query planner never tries to use a self-made transient
    index in place of a schema-defined index.
15. Other minor tweaks to improve the quality of VDBE code.
**Bug Fixes:**
16. Fix a bug in CREATE UNIQUE INDEX, introduced when WITHOUT ROWID
    support added in version 3.8.2, that allows a non-unique NOT NULL column to be
    given a UNIQUE index.
    Ticket 9a6daf340df99ba93c
17. Fix a bug in R-Tree extension, introduced in the previous release,
    that can cause an
    incorrect results for queries that use the rowid of the R-Tree on the
    left-hand side of an IN operator.
    Ticket d2889096e7bdeac6.
18. Fix the sqlite3_stmt_busy() interface so that it gives the correct answer
    for ROLLBACK statements that have been stepped but never reset.
19. Fix a bug in that would cause a null pointer to be dereferenced
    if a column with a DEFAULT that is an aggregate function tried to usee its
    DEFAULT.
    Ticket 3a88d85f36704eebe1
20. CSV output from the command-line shell now always uses CRLF for the
    row separator and avoids inserting CR in front of NLs contained in
    data.
21. Fix a column affinity problem with the IN operator.
    Ticket 9a8b09f8e6.
22. Fix the ANALYZE command so that it adds correct samples for
    WITHOUT ROWID tables in the sqlite_stat4 table.
    Ticket b2fa5424e6fcb15.
23. SQLITE_SOURCE_ID: "2014-08-15 11:46:33 9491ba7d738528f168657adb43a198238abde19e"
24. SHA1 for sqlite3.c: 72c64f05cd9babb9c0f9b3c82536d83be7804b1c

### 2014-06-04 (3.8.5)

1. Added support for partial sorting by index.
2. Enhance the query planner so that it always prefers an index that uses a superset of
    WHERE clause terms relative to some other index.
3. Improvements to the automerge command of FTS4 to better control the index size
    for a full-text index that is subject to a large number of updates.
4. Added the sqlite3_rtree_query_callback() interface to R-Tree extension
5. Added new URI query parameters "nolock" and "immutable".
6. Use less memory by not remembering CHECK constraints on read-only
    database connections.
7. Enable the OR optimization for WITHOUT ROWID tables.
8. Render expressions of the form "x IN (?)" (with a single value in
    the list on the right-hand side of the IN operator) as if they where "x==?",
    Similarly optimize "x NOT IN (?)"
9. Add the ".system" and ".once" commands to the command-line shell.
10. Added the SQLITE_IOCAP_IMMUTABLE bit to the set of bits that can be returned by
    the xDeviceCharacteristics method of a VFS.
11. Added the SQLITE_TESTCTRL_BYTEORDER test control.
**Bug Fixes:**
12. OFFSET clause ignored on queries without a FROM clause.
    Ticket 07d6a0453d
13. Assertion fault on queries involving expressions of the form
    "x IN (?)".  Ticket e39d032577.
14. Incorrect column datatype reported.
    Ticket a8a0d2996a
15. Duplicate row returned on a query against a table with more than
    16 indices, each on a separate column, and all used via OR-connected constraints.
    Ticket 10fb063b11
16. Partial index causes assertion fault on UPDATE OR REPLACE.
    Ticket 2ea3e9fe63
17. Crash when calling undocumented SQL function sqlite_rename_parent()
    with NULL parameters.
    Ticket 264b970c43
18. ORDER BY ignored if the query has an identical GROUP BY.
    Ticket b75a9ca6b0
19. The group_concat(x,'') SQL function returns NULL instead of an empty string
    when all inputs are empty strings.
    Ticket 55746f9e65
20. Fix a bug in the VDBE code generator that caused crashes when
    doing an INSERT INTO ... SELECT statement where the number of columns
    being inserted is larger than the number of columns in the destination
    table.
    Ticket e9654505cfd
21. Fix a problem in CSV import in the command-line shell
    where if the leftmost field of the first row
    in the CSV file was both zero bytes in size and unquoted no data would
    be imported.
22. Fix a problem in FTS4 where the left-most column that contained
    the notindexed column name as a prefix
    was not indexed rather than the column whose name matched exactly.
23. Fix the sqlite3_db_readonly() interface so that it returns true if
    the database is read-only due to the file format write version number
    being too large.
24. SQLITE_SOURCE_ID: "2014-06-04 14:06:34 b1ed4f2a34ba66c29b130f8d13e9092758019212"
25. SHA1 for sqlite3.c: 7bc194957238c61b1a47f301270286be5bc5208c

### 2014-04-03 (3.8.4.3)

1. Add a
    one-character fix
    for a problem that might cause incorrect query results on a query that mixes
    DISTINCT, GROUP BY in a subquery, and ORDER BY.
    Ticket 98825a79ce14.
2. SQLITE_SOURCE_ID: "2014-04-03 16:53:12 a611fa96c4a848614efe899130359c9f6fb889c3"
3. SHA1 for sqlite3.c: 310a1faeb9332a3cd8d1f53b4a2e055abf537bdc

### 2014-03-26 (3.8.4.2)

1. Fix a potential buffer overread that could result when trying to search a
    corrupt database file.
2. SQLITE_SOURCE_ID: "2014-03-26 18:51:19 02ea166372bdb2ef9d8dfbb05e78a97609673a8e"
3. SHA1 for sqlite3.c: 4685ca86c2ea0649ed9f59a500013e90b3fe6d03

### 2014-03-11 (3.8.4.1)

1. Work around a C-preprocessor macro conflict that breaks the build for some
    configurations with Microsoft Visual Studio.
2. When computing the cost of the skip-scan optimization, take into account the
    fact that multiple seeks are required.
3. SQLITE_SOURCE_ID: "2014-03-11 15:27:36 018d317b1257ce68a92908b05c9c7cf1494050d0"
4. SHA1 for sqlite3.c: d5cd1535053a50aa8633725e3595740b33709ac5

### 2014-03-10 (3.8.4)

1. Code optimization and refactoring for improved performance.
2. Add the ".clone" and ".save" commands to the command-line shell.
3. Update the banner on the command-line shell to alert novice users when they
    are using an ephemeral in-memory database.
4. Fix editline support in the command-line shell.
5. Add support for coverage testing of VDBE programs using the
    SQLITE_TESTCTRL_VDBE_COVERAGE verb of sqlite3_test_control().
6. Update the _FILE_OFFSET_BITS macro so that builds work again on QNX.
7. Change the datatype of SrcList.nSrc from type u8 to type int to work around
    an issue in the C compiler on AIX.
8. Get extension loading working on Cygwin.
9. Bug fix: Fix the char() SQL function so that it returns an empty string
    rather than an "out of memory" error when called with zero arguments.
10. Bug fix: DISTINCT now recognizes that a zeroblob and a blob of all
    0x00 bytes are the same thing.
    Ticket [fccbde530a]
11. Bug fix: Compute the correct answer for queries that contain an IS NOT NULL
    term in the WHERE clause and also contain an OR term in the WHERE clause and
    are compiled with SQLITE_ENABLE_STAT4.
    Ticket [4c86b126f2]
12. Bug fix: Make sure "rowid" columns are correctly resolved in joins between
    normal tables and WITHOUT ROWID tables.
    Ticket [c34d0557f7]
13. Bug fix: Make sure the same temporary registers are not used in concurrent
    co-routines used to implement compound SELECT statements containing ORDER
    BY clauses, as such use can lead to incorrect answers.
    Ticket [8c63ff0eca]
14. Bug fix: Ensure that "ORDER BY random()" clauses do not get optimized out.
    Ticket [65bdeb9739]
15. Bug fix: Repair a name-resolution error that can occur in sub-select statements
    contained within a TRIGGER.
    Ticket [4ef7e3cfca]
16. Bug fix: Fix column default values expressions of the form
    "DEFAULT(-(-9223372036854775808))" so that they work correctly, initializing
    the column to a floating point value approximately equal to
    +9223372036854775808.0.
17. SQLITE_SOURCE_ID: "2014-03-10 12:20:37 530a1ee7dc2435f80960ce4710a3c2d2bfaaccc5"
18. SHA1 for sqlite3.c: b0c22e5f15f5ba2afd017ecd990ea507918afe1c

### 2014-02-11 (3.8.3.1)

1. Fix a bug (ticket 4c86b126f2)
    that causes rows to go missing on some queries with OR clauses and
    IS NOT NULL operators in the WHERE clause, when the SQLITE_ENABLE_STAT3
    or SQLITE_ENABLE_STAT4 compile-time options are used.
2. Fix a harmless compiler warning that was causing problems for VS2013.
3. SQLITE_SOURCE_ID: "2014-02-11 14:52:19 ea3317a4803d71d88183b29f1d3086f46d68a00e"
4. SHA1 for sqlite3.c: 990004ef2d0eec6a339e4caa562423897fe02bf0

### 2014-02-03 (3.8.3)

1. Added support for common table expressions and the WITH clause.
2. Added the printf() SQL function.
3. Added SQLITE_DETERMINISTIC as an optional bit in the 4th argument to the
    sqlite3_create_function() and related interfaces, providing applications with
    the ability to create new functions that can be factored out of inner loops when
    they have constant arguments.
4. Add SQLITE_READONLY_DBMOVED error code, returned at the beginning of a
    transaction, to indicate that the underlying database file has been renamed
    or moved out from under SQLite.
5. Allow arbitrary expressions, including function calls and subqueries, in
    the filename argument to ATTACH.
6. Allow a VALUES clause to be used anywhere a SELECT statement is valid.
7. Reseed the PRNG used by sqlite3_randomness(N,P) when invoked with N==0.
    Automatically reseed after a fork() on unix.
8. Enhance the spellfix1 virtual table so that it can search efficiently by rowid.
9. Performance enhancements.
10. Improvements to the comments in the VDBE byte-code display when running EXPLAIN.
11. Add the "%token_class" directive to Lemon parser generator and use it to simplify
    the grammar.
12. Change the Lemon source code to avoid calling C-library functions that OpenBSD
    considers dangerous.  (Ex: sprintf).
13. Bug fix: In the command-line shell CSV import feature, do not end a field
    when an escaped double-quote occurs at the end of a CRLN line.
14. SQLITE_SOURCE_ID:
    "2014-02-03 13:52:03 e816dd924619db5f766de6df74ea2194f3e3b538"
15. SHA1 for sqlite3.c: 98a07da78f71b0275e8d9c510486877adc31dbee

### 2013-12-06 (3.8.2)

1. Changed the defined behavior for the CAST expression when floating point values
    greater than  +9223372036854775807 are cast into integers so that the
    result is the largest possible integer, +9223372036854775807, instead of
    the smallest possible integer, -9223372036854775808.  After this change,
    CAST(9223372036854775809.0 as INT) yields +9223372036854775807 instead
    of -9223372036854775808.
    **← Potentially Incompatible Change!**
2. Added support for WITHOUT ROWID tables.
3. Added the skip-scan optimization to the query planner.
4. Extended the virtual table interface, and in particular the
    sqlite3_index_info object to allow a virtual table to report its estimate
    on the number of rows that will be returned by a query.
5. Update the R-Tree extension to make use of the enhanced virtual table
    interface.
6. Add the SQLITE_ENABLE_EXPLAIN_COMMENTS compile-time option.
7. Enhanced the comments that are inserted into EXPLAIN output when the
    SQLITE_ENABLE_EXPLAIN_COMMENTS compile-time option is enabled.
8. Performance enhancements in the VDBE, especially to the OP_Column opcode.
9. Factor constant subexpressions in inner loops out to the initialization code
    in prepared statements.
10. Enhanced the ".explain" output formatting of the command-line shell
    so that loops are indented to better show the structure of the program.
11. Enhanced the ".timer" feature of the command-line shell so that it
    shows wall-clock time in addition to system and user times.
12. SQLITE_SOURCE_ID:
    "2013-12-06 14:53:30 27392118af4c38c5203a04b8013e1afdb1cebd0d"
13. SHA1 for sqlite3.c: 6422c7d69866f5ea3db0968f67ee596e7114544e

### 2013-10-17 (3.8.1)

1. Added the unlikely() and likelihood() SQL functions to be used
    as hints to the query planner.
2. Enhancements to the query planner:
  1. Take into account the fact WHERE clause terms that cannot be used with indices
      still probably reduce the number of output rows.
  
  2. Estimate the sizes of table and index rows and use the smallest applicable B-Tree
      for full scans and "count(*)" operations.
3. Added the soft_heap_limit pragma.
4. Added support for SQLITE_ENABLE_STAT4
5. Added support for "sz=NNN" parameters at the end of
    sqlite_stat1.stat fields
    used to specify the average length in bytes for table and index rows.
6. Avoid running foreign-key constraint checks on an UPDATE if none of the
    modified columns are associated with foreign keys.
7. Added the SQLITE_MINIMUM_FILE_DESCRIPTOR compile-time option
8. Added the win32-longpath VFS on windows, permitting filenames up to 32K
    characters in length.
9. The Date And Time Functions are enhanced so that the current time
    (ex: julianday('now')) is always the same for multiple function invocations
    within the same sqlite3_step() call.
10. Add the "totype.c" extension, implementing the tointeger() and toreal()
    SQL functions.
11. FTS4 queries are better able to make use of docid<$limit constraints to
    limit the amount of I/O required.
12. Added the hidden fts4aux languageid column to the fts4aux virtual table.
13. The VACUUM command packs the database about 1% tighter.
14. The sqlite3_analyzer utility program is updated to provide better descriptions
    and to compute a more accurate estimate for "Non-sequential pages"
15. Refactor the implementation of PRAGMA statements to improve parsing performance.
16. The directory used to hold temporary files on unix can now be set using
    the SQLITE_TMPDIR environment variable,  which takes precedence over the
    TMPDIR environment variable.  The sqlite3_temp_directory global variable
    still has higher precedence than both environment variables, however.
17. Added the PRAGMA stats statement.
18. **Bug fix:** Return the correct answer for "SELECT count(*) FROM table" even if
    there is a partial index on the table. Ticket
    a5c8ed66ca.
19. SQLITE_SOURCE_ID:
    "2013-10-17 12:57:35 c78be6d786c19073b3a6730dfe3fb1be54f5657a"
20. SHA1 for sqlite3.c: 0a54d76566728c2ba96292a49b138e4f69a7c391

### 2013-09-03 (3.8.0.2)

1. Fix a bug in the optimization that attempts to omit unused LEFT JOINs
2. SQLITE_SOURCE_ID:
    "2013-09-03 17:11:13 7dd4968f235d6e1ca9547cda9cf3bd570e1609ef"
3. SHA1 for sqlite3.c: 6cf0c7b46975a87a0dc3fba69c229a7de61b0c21

### 2013-08-29 (3.8.0.1)

1. Fix an off-by-one error that caused quoted empty string at the end of a
CRLF-terminated line of CSV input to be misread by the command-line shell.
2. Fix a query planner bug involving a LEFT JOIN with a BETWEEN or LIKE/GLOB
constraint and then another INNER JOIN to the right that involves an OR constraint.
3. Fix a query planner bug that could result in a segfault when querying tables
with a UNIQUE or PRIMARY KEY constraint with more than four columns.
4. SQLITE_SOURCE_ID:
    "2013-08-29 17:35:01 352362bc01660edfbda08179d60f09e2038a2f49"
5. SHA1 for sqlite3.c: 99906bf63e6cef63d6f3d7f8526ac4a70e76559e

### 2013-08-26 (3.8.0)

1. Add support for partial indexes
2. Cut-over to the next generation query planner for faster and better query plans.
3. The EXPLAIN QUERY PLAN output no longer shows an estimate of the number of
    rows generated by each loop in a join.
4. Added the FTS4 notindexed option, allowing non-indexed columns in an FTS4 table.
5. Added the SQLITE_STMTSTATUS_VM_STEP option to sqlite3_stmt_status().
6. Added the cache_spill pragma.
7. Added the query_only pragma.
8. Added the defer_foreign_keys pragma and the
    sqlite3_db_status(db, SQLITE_DBSTATUS_DEFERRED_FKS,...) C-language interface.
9. Added the "percentile()" function as a loadable extension in the ext/misc
    subdirectory of the source tree.
10. Added the SQLITE_ALLOW_URI_AUTHORITY compile-time option.
11. Add the sqlite3_cancel_auto_extension(X) interface.
12. A running SELECT statement that lacks a FROM clause (or any other statement that
    never reads or writes from any database file) will not prevent a read
    transaction from closing.
13. Add the SQLITE_DEFAULT_AUTOMATIC_INDEX compile-time option.  Setting this option
    to 0 disables automatic indices by default.
14. Issue an SQLITE_WARNING_AUTOINDEX warning on the SQLITE_CONFIG_LOG whenever
    the query planner uses an automatic index.
15. Added the SQLITE_FTS3_MAX_EXPR_DEPTH compile-time option.
16. Added an optional 5th parameter defining the collating sequence to the
    next_char() extension SQL function.
17. The SQLITE_BUSY_SNAPSHOT extended error code is returned in WAL mode when
    a read transaction cannot be upgraded to a write transaction because the read is
