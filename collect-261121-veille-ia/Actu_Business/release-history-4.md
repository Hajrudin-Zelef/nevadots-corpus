---
id: collect-261121-veille-ia/actu-business/release-history-4
title: "Release History"
domain: actu-business
role: reference
task: article
actors: []
dates: []
keywords: ["sqlite", "extension", "sqlite3", "table", "database", "added", "error", "application"]
source: https://sqlite.org/draft/changes.html
source_anchor: ""
source_lines: [4501, 6000]
section: "Actu_Business"
---
    on an older snapshot.
18. Enhancements to the sqlite3_analyzer utility program to provide size
    information separately for each individual index of a table, in addition to
    the aggregate size.
19. Allow read transactions to be freely opened and closed by SQL statements run
    from within the implementation of application-defined SQL functions if the
    function is called by a SELECT statement that does not access any database table.
20. Disable the use of posix_fallocate() on all (unix) systems unless the
    HAVE_POSIX_FALLOCATE compile-time option is used.
21. Update the ".import" command in the command-line shell to support multi-line
    fields and correct RFC-4180 quoting and to issue warning and/or error messages
    if the input text is not strictly RFC-4180 compliant.
22. Bug fix: In the unicode61 tokenizer of FTS4, treat all private code points
    as identifier symbols.
23. Bug fix: Bare identifiers in ORDER BY clauses bind more tightly to output column
    names, but identifiers in expressions bind more tightly to input column names.
    Identifiers in GROUP BY clauses always prefer output column names, however.
24. Bug fixes: Multiple problems in the legacy query optimizer were fixed by the
    move to NGQP.
25. SQLITE_SOURCE_ID:
    "2013-08-26 04:50:08 f64cd21e2e23ed7cff48f7dafa5e76adde9321c2"
26. SHA1 for sqlite3.c: b7347f4b4c2a840e6ba12040093d606bd16ea21e

### 2013-05-20 (3.7.17)

1. Add support for memory-mapped I/O.
2. Add the sqlite3_strglob() convenience interface.
3. Assigned the integer at offset 68 in the database header as the
    Application ID for when SQLite is used as an application file-format.
    Added the PRAGMA application_id command to query and set the Application ID.
4. Report rollback recovery in the error log as SQLITE_NOTICE_RECOVER_ROLLBACK.
    Change the error log code for WAL recover from
    SQLITE_OK to SQLITE_NOTICE_RECOVER_WAL.
5. Report the risky uses of unlinked database files and
   database filename aliasing as SQLITE_WARNING messages in the error log.
6. Added the SQLITE_TRACE_SIZE_LIMIT compile-time option.
7. Increase the default value of SQLITE_MAX_SCHEMA_RETRY to 50 and make sure
    that it is honored in every place that a schema change might force a statement
    retry.
8. Add a new test harness called "mptester" used to verify correct operation
    when multiple processes are using the same database file at the same time.
9. Enhance the extension loading mechanism to be more flexible (while
    still maintaining backwards compatibility) in two ways:
      1. If the default entry point "sqlite3_extension_init" is not present in
        the loadable extension, also try an entry point "sqlite3_X_init" where
        "X" is based on the shared library filename.  This allows every extension
        to have a different entry point, which allows them to be statically linked
        with no code changes.
    
  2. The shared library filename passed to sqlite3_load_extension() may
        omit the filename suffix, and an appropriate architecture-dependent
        suffix (".so", ".dylib", or ".dll") will be added automatically.
    
10. Added many new loadable extensions to the source tree, including
    amatch, closure, fuzzer, ieee754, nextchar, regexp, spellfix,
    and wholenumber.  See header comments on each extension source file
    for further information about what that extension does.
11. Enhance FTS3 to avoid using excess stack space when there are a huge
    number of terms on the right-hand side of the MATCH operator.  A side-effect
    of this change is that the MATCH operator can only accommodate 12 NEAR
    operators at a time.
12. Enhance the fts4aux virtual table so that it can be a TEMP table.
13. Added the fts3tokenize virtual table to the full-text search logic.
14. Query planner enhancement: Use the transitive property of constraints
    to move constraints into the outer loops of a join whenever possible,
    thereby reducing the amount of work that needs to occur in inner loops.
15. Discontinue the use of posix_fallocate() on unix, as it does not work on all
    filesystems.
16. Improved tracing and debugging facilities in the Windows VFS.
17. Bug fix: Fix a potential **database corruption bug** in shared cache mode when one
    database connection is closed while another is in the middle of a write
    transaction.
    Ticket e636a050b7
18. Bug fix:
    Only consider AS names from the result set as candidates for resolving
    identifiers in the WHERE clause if there are no other matches. In the
    ORDER BY clause, AS names take priority over any column names.
    Ticket 2500cdb9be05
19. Bug fix: Do not allow a virtual table to cancel the ORDER BY clause unless
    all outer loops are guaranteed to return no more than one row result.
    Ticket ba82a4a41eac1.
20. Bug fix: Do not suppress the ORDER BY clause on a virtual table query if
    an IN constraint is used.
    Ticket f69b96e3076e.
21. Bug fix: The command-line shell gives an exit code of 0 when terminated
    using the ".quit" command.
22. Bug fix: Make sure PRAGMA statements appear in sqlite3_trace() output.
23. Bug fix: When a compound query that uses an ORDER BY clause
    with a COLLATE operator, make sure that the sorting occurs
    according to the specified collation and that the comparisons associate with
    the compound query use the native collation.  Ticket
    6709574d2a8d8.
24. Bug fix: Makes sure the authorizer callback gets
    a valid pointer to the string "ROWID" for the column-name parameter when
    doing an UPDATE that changes the rowid.  Ticket
    0eb70d77cb05bb2272
25. Bug fix: Do not move WHERE clause terms inside OR expressions that are
    contained within an ON clause of a LEFT JOIN.  Ticket
    f2369304e4
26. Bug fix: Make sure an error is always reported when attempting to preform
    an operation that requires a collating sequence that is missing.
    Ticket 0fc59f908b
27. SQLITE_SOURCE_ID:
    "2013-05-20 00:56:22 118a3b35693b134d56ebd780123b7fd6f1497668"
28. SHA1 for sqlite3.c: 246987605d0503c700a08b9ee99a6b5d67454aab

### 2013-04-12 (3.7.16.2)

1. Fix a bug (present since version 3.7.13) that could result in database corruption
    on windows if two or more processes try to access the same database file at the
    same time and immediately after third process crashed in the middle of committing
    to that same file.  See ticket
    7ff3120e4f for further
    information.
2. SQLITE_SOURCE_ID:
    "2013-04-12 11:52:43 cbea02d93865ce0e06789db95fd9168ebac970c7"
3. SHA1 for sqlite3.c: d466b54789dff4fb0238b9232e74896deaefab94

### 2013-03-29 (3.7.16.1)

1. Fix for a bug in the ORDER BY optimizer that was introduced in
    version 3.7.15 which would sometimes optimize out the sorting step
    when in fact the sort was required.
    Ticket a179fe7465
2. Fix a long-standing bug in the CAST expression that would recognize UTF16
    characters as digits even if their most-significant-byte was not zero.
    Ticket 689137afb6da41.
3. Fix a bug in the NEAR operator of FTS3 when applied to subfields.
    Ticket 38b1ae018f.
4. Fix a long-standing bug in the storage engine that would (very rarely)
    cause a spurious report of an SQLITE_CORRUPT error but which was otherwise
    harmless.
    Ticket 6bfb98dfc0c.
5. The SQLITE_OMIT_MERGE_SORT option has been removed.  The merge sorter is
    now a required component of SQLite.
6. Fixed lots of spelling errors in the source-code comments
7. SQLITE_SOURCE_ID:
    "2013-03-29 13:44:34 527231bc67285f01fb18d4451b28f61da3c4e39d"
8. SHA1 for sqlite3.c: 7a91ceceac9bcf47ceb8219126276e5518f7ff5a

### 2013-03-18 (3.7.16)

1. Added the PRAGMA foreign_key_check command.
2. Added new extended error codes for all SQLITE_CONSTRAINT errors
3. Added the SQLITE_READONLY_ROLLBACK extended error code for when a database
    cannot be opened because it needs rollback recovery but is read-only.
4. Added SQL functions unicode(A) and char(X1,...,XN).
5. Performance improvements for PRAGMA incremental_vacuum, especially in
    cases where the number of free pages is greater than what will fit on a
    single trunk page of the freelist.
6. Improved optimization of queries containing aggregate min() or max().
7. Enhance virtual tables so that they can potentially use an index when
    the WHERE clause contains the IN operator.
8. Allow indices to be used for sorting even if prior terms of the index
    are constrained by IN operators in the WHERE clause.
9. Enhance the PRAGMA table_info command so that the "pk" column is an
    increasing integer to show the order of columns in the primary key.
10. Enhance the query optimizer to exploit transitive join constraints.
11. Performance improvements in the query optimizer.
12. Allow the error message from PRAGMA integrity_check to be longer than
    20000 bytes.
13. Improved name resolution for deeply nested queries.
14. Added the test_regexp.c module as a demonstration of how to implement
    the REGEXP operator.
15. Improved error messages in the RTREE extension.
16. Enhance the command-line shell so that a non-zero argument to the
    ".exit" command causes the shell to exit immediately without cleanly
    shutting down the database connection.
17. Improved error messages for invalid boolean arguments to dot-commands
    in the command-line shell.
18. Improved error messages for "foreign key mismatch" showing the names of
    the two tables involved.
19. Remove all uses of umask() in the unix VFS.
20. Added the PRAGMA vdbe_addoptrace and PRAGMA vdbe_debug commands.
21. Change to use strncmp() or the equivalent instead of memcmp() when
    comparing non-zero-terminated strings.
22. Update cygwin interfaces to omit deprecated API calls.
23. Enhance the spellfix1 extension so that the edit distance cost table can
    be changed at runtime by inserting a string like 'edit_cost_table=TABLE'
    into the "command" field.
24. Bug fix: repair a long-standing problem that could cause incorrect query
    results in a 3-way or larger join that compared INTEGER fields against TEXT
    fields in two or more places.
    Ticket fc7bd6358f
25. Bug fix: Issue an error message if the 16-bit reference counter on a
    view overflows due to an overly complex query.
26. Bug fix: Avoid leaking memory on LIMIT and OFFSET clauses in deeply
    nested UNION ALL queries.
27. Bug fix: Make sure the schema is up-to-date prior to running pragmas
    table_info, index_list, index_info, and foreign_key_list.
28. SQLITE_SOURCE_ID:
    "2013-03-18 11:39:23 66d5f2b76750f3520eb7a495f6247206758f5b90"
29. SHA1 for sqlite3.c: 7308ab891ca1b2ebc596025cfe4dc36f1ee89cf6

### 2013-01-09 (3.7.15.2)

1. Fix a bug, introduced in version 3.7.15, that causes an ORDER BY clause
    to be optimized out of a three-way join when the ORDER BY is actually
    required.	
    Ticket 598f5f7596b055
2. SQLITE_SOURCE_ID:
    "2013-01-09 11:53:05 c0e09560d26f0a6456be9dd3447f5311eb4f238f"
3. SHA1 for sqlite3.c: 5741f47d1bc38aa0a8c38f09e60a5fe0031f272d

### 2012-12-19 (3.7.15.1)

1. Fix a bug, introduced in version 3.7.15, that causes a segfault if
    the AS name of a result column of a SELECT statement is used as a logical
    term in the WHERE clause.  Ticket
    a7b7803e8d1e869.
2. SQLITE_SOURCE_ID:
    "2012-12-19 20:39:10 6b85b767d0ff7975146156a99ad673f2c1a23318"
3. SHA1 for sqlite3.c: bbbaa68061e925bd4d7d18d7e1270935c5f7e39a

### 2012-12-12 (3.7.15)

1. Added the sqlite3_errstr() interface.
2. Avoid invoking the sqlite3_trace() callback multiple times when a
    statement is automatically reprepared due to SQLITE_SCHEMA errors.
3. Added support for Windows Phone 8 platforms
4. Enhance IN operator processing to make use of indices with numeric
    affinities.
5. Do full-table scans using covering indices when possible, under the
    theory that an index will be smaller and hence can be scanned with
    less I/O.
6. Enhance the query optimizer so that ORDER BY clauses are more aggressively
    optimized, especially in joins where various terms of the ORDER BY clause
    come from separate tables of the join.
7. Add the ability to implement FROM clause subqueries as coroutines rather
    that manifesting the subquery into a temporary table.
8. Enhancements the command-line shell:
      1. Added the ".print" command
    
  2. Negative numbers in the ".width" command cause right-alignment
    
  3. Add the ".wheretrace" command when compiled with SQLITE_DEBUG
    
9. Added the busy_timeout pragma.
10. Added the instr() SQL function.
11. Added the SQLITE_FCNTL_BUSYHANDLER file control, used to allow VFS
    implementations to get access to the busy handler callback.
12. The xDelete method in the built-in VFSes now return
    SQLITE_IOERR_DELETE_NOENT if the file to be deleted does not exist.
13. Enhanced support for QNX.
14. Work around an optimizer bug in the MSVC compiler when targeting ARM.
15. Bug fix: Avoid various concurrency problems in shared cache mode.
16. Bug fix: Avoid a deadlock or crash if the backup API, shared cache,
    and the SQLite Encryption Extension are all used at once.
17. Bug fix: SQL functions created using the TCL interface honor the
    "nullvalue" setting.
18. Bug fix: Fix a 32-bit overflow problem on CREATE INDEX for databases
    larger than 16GB.
19. Bug fix: Avoid segfault when using the COLLATE operator inside of a
    CHECK constraint or view in shared cache mode.
20. SQLITE_SOURCE_ID:
    "2012-12-12 13:36:53 cd0b37c52658bfdf992b1e3dc467bae1835a94ae"
21. SHA1 for sqlite3.c: 2b413611f5e3e3b6ef5f618f2a9209cdf25cbcff"

### 2012-10-04 (3.7.14.1)

1. Fix a bug (ticket
[d02e1406a58ea02d]])
that causes a segfault on a LEFT JOIN that includes an OR in the ON clause.
2. Work around a bug in the optimizer in the VisualStudio-2012 compiler that
causes invalid code to be generated when compiling SQLite on ARM.
3. Fix the TCL interface so that the "nullvalue" setting is honored for
TCL implementations of SQL functions.
4. SQLITE_SOURCE_ID:
    "2012-10-04 19:37:12 091570e46d04e84b67228e0bdbcd6e1fb60c6bdb"
5. SHA1 for sqlite3.c: 62aaecaacab3a4bf4a8fe4aec1cfdc1571fe9a44

### 2012-09-03 (3.7.14)

1. Drop built-in support for OS/2. If you need to upgrade an OS/2
    application to use this or a later version of SQLite,
    then add an application-defined VFS using the
    sqlite3_vfs_register() interface.  The code removed in this release can
    serve as a baseline for the application-defined VFS.
2. Ensure that floating point values are preserved exactly when reconstructing
    a database from the output of the ".dump" command of the
    command-line shell.
3. Added the sqlite3_close_v2() interface.
4. Updated the command-line shell so that it can be built using
    SQLITE_OMIT_FLOATING_POINT and SQLITE_OMIT_AUTOINIT.
5. Improvements to the windows makefiles and build processes.
6. Enhancements to PRAGMA integrity_check and PRAGMA quick_check so that
    they can optionally check just a single attached database instead of all
    attached databases.
7. Enhancements to WAL mode processing that ensure that at least one
    valid read-mark is available at all times, so that read-only processes
    can always read the database.
8. Performance enhancements in the sorter used by ORDER BY and CREATE INDEX.
9. Added the SQLITE_DISABLE_FTS4_DEFERRED compile-time option.
10. Better handling of aggregate queries where the aggregate functions are
    contained within subqueries.
11. Enhance the query planner so that it will try to use a covering index
    on queries that make use of or optimization.
12. SQLITE_SOURCE_ID:
    "2012-09-03 15:42:36 c0d89d4a9752922f9e367362366efde4f1b06f2a"
13. SHA1 for sqlite3.c: 5fdf596b29bb426001f28b488ff356ae14d5a5a6

### 2012-06-11 (3.7.13)

1. In-memory databases that are specified using
    URI filenames are allowed to use shared cache,
    so that the same
    in-memory database can be accessed from multiple database connections.
2. Recognize and use the mode=memory query parameter in
    URI filenames.
3. Avoid resetting the schema of shared cache connections when any one
    connection closes.  Instead, wait for the last connection to close before
    resetting the schema.
4. In the RTREE extension, when rounding 64-bit floating point numbers
    to 32-bit for storage, always round in a direction that causes the
    bounding box to get larger.
5. Adjust the unix driver to avoid unnecessary calls to fchown().
6. Add interfaces sqlite3_quota_ferror() and sqlite3_quota_file_available()
    to the test_quota.c module.
7. The sqlite3_create_module() and sqlite3_create_module_v2() interfaces
    return SQLITE_MISUSE on any attempt to overload or replace a virtual table
    module.  The destructor is always called in this case, in accordance with
    historical and current documentation.
8. SQLITE_SOURCE_ID:
    "2012-06-11 02:05:22 f5b5a13f7394dc143aa136f1d4faba6839eaa6dc"
9. SHA1 for sqlite3.c: ff0a771d6252545740ba9685e312b0e3bb6a641b

### 2012-05-22 (3.7.12.1)

1. Fix a bug
    (ticket c2ad16f997)
    in the 3.7.12 release that can cause a segfault for certain
    obscure nested aggregate queries.
2. Fix various other minor test script problems.
3. SQLITE_SOURCE_ID:
    "2012-05-22 02:45:53 6d326d44fd1d626aae0e8456e5fa2049f1ce0789"
4. SHA1 for sqlite3.c: d494e8d81607f0515d4f386156fb0fd86d5ba7df

### 2012-05-14 (3.7.12)

1. Add the SQLITE_DBSTATUS_CACHE_WRITE option for sqlite3_db_status().
2. Optimize the typeof() and length() SQL functions so that they avoid
    unnecessary reading of database content from disk.
3. Add the FTS4 "merge" command, the FTS4 "automerge" command, and
    the FTS4 "integrity-check" command.
4. Report the name of specific CHECK constraints that fail.
5. In the command-line shell, use popen() instead of fopen() if the first
    character of the argument to the ".output" command is "|".
6. Make use of OVERLAPPED in the windows VFS to avoid some system calls
    and thereby obtain a performance improvement.
7. More aggressive optimization of the AND operator when one side or the
    other is always false.
8. Improved performance of queries with many OR-connected terms in the
    WHERE clause that can all be indexed.
9. Add the SQLITE_RTREE_INT_ONLY compile-time option to force the
    R*Tree Extension Module to use integer instead of
    floating point values for both storage and computation.
10. Enhance the PRAGMA integrity_check command to use much less memory when
    processing multi-gigabyte databases.
11. New interfaces added to the test_quota.c add-on module.
12. Added the ".trace" dot-command to the command-line shell.
13. Allow virtual table constructors to be invoked recursively.
14. Improved optimization of ORDER BY clauses on compound queries.
15. Improved optimization of aggregate subqueries contained within an
    aggregate query.
16. Bug fix: Fix the RELEASE command so that it does not cancel pending
    queries.  This repairs a problem introduced in 3.7.11.
17. Bug fix: Do not discard the DISTINCT as superfluous unless a subset of
    the result set is subject to a UNIQUE constraint *and* it none
    of the columns in that subset can be NULL.
    Ticket 385a5b56b9.
18. Bug fix: Do not optimize away an ORDER BY clause that has the same terms
    as a UNIQUE index unless those terms are also NOT NULL.
    Ticket 2a5629202f.
19. SQLITE_SOURCE_ID:
    "2012-05-14 01:41:23 8654aa9540fe9fd210899d83d17f3f407096c004"
20. SHA1 for sqlite3.c: 57e2104a0f7b3f528e7f6b7a8e553e2357ccd2e1

### 2012-03-20 (3.7.11)

1. Enhance the INSERT syntax to allow multiple rows to be inserted
    via the VALUES clause.
2. Enhance the CREATE VIRTUAL TABLE command to support the
    IF NOT EXISTS clause.
3. Added the sqlite3_stricmp() interface as a counterpart to
    sqlite3_strnicmp().
4. Added the sqlite3_db_readonly() interface.
5. Added the SQLITE_FCNTL_PRAGMA file control, giving VFS implementations
    the ability to add new PRAGMA statements or to override built-in
    PRAGMAs.
6. Queries of the form:  "SELECT max(x), y FROM table" returns the
    value of y on the same row that contains the maximum x value.
7. Added support for the FTS4 languageid option.
8. Documented support for the FTS4 content option.  This feature has
    actually been in the code since version 3.7.9 but is only now considered
    to be officially supported.
9. Pending statements no longer block ROLLBACK.  Instead, the pending
    statement will return SQLITE_ABORT upon next access after the ROLLBACK.
10. Improvements to the handling of CSV inputs in the command-line shell
11. Fix a bug introduced
    in version 3.7.10 that might cause a LEFT JOIN
    to be incorrectly converted into an INNER JOIN if the WHERE clause
    indexable terms connected by OR.
12. SQLITE_SOURCE_ID:
    "2012-03-20 11:35:50 00bb9c9ce4f465e6ac321ced2a9d0062dc364669"
13. SHA1 for sqlite3.c: d460d7eda3a9dccd291aed2a9fda868b9b120a10

### 2012-01-16 (3.7.10)

1. The default schema format number is changed from 1 to 4.
    This means that, unless
    the PRAGMA legacy_file_format=ON statement is
    run, newly created database files will be unreadable by version of SQLite
    prior to 3.3.0 (2006-01-10).  It also means that the descending indices
    are enabled by default.
2. The sqlite3_pcache_methods structure and the SQLITE_CONFIG_PCACHE
    and SQLITE_CONFIG_GETPCACHE configuration parameters are deprecated.
    They are replaced by a new sqlite3_pcache_methods2 structure and
    SQLITE_CONFIG_PCACHE2 and SQLITE_CONFIG_GETPCACHE2 configuration
    parameters.
3. Added the powersafe overwrite property to the VFS interface.  Provide
    the SQLITE_IOCAP_POWERSAFE_OVERWRITE I/O capability, the
    SQLITE_POWERSAFE_OVERWRITE compile-time option, and the
    "psow=BOOLEAN" query parameter for URI filenames.
4. Added the sqlite3_db_release_memory() interface and the
    shrink_memory pragma.
5. Added the sqlite3_db_filename() interface.
6. Added the sqlite3_stmt_busy() interface.
7. Added the sqlite3_uri_boolean() and sqlite3_uri_int64() interfaces.
8. If the argument to PRAGMA cache_size is negative N, that means to use
    approximately -1024*N bytes of memory for the page cache regardless of
    the page size.
9. Enhanced the default memory allocator to make use of _msize() on windows,
    malloc_size() on Mac, and malloc_usable_size() on Linux.
10. Enhanced the query planner to support index queries with range constraints
    on the rowid.
11. Enhanced the query planner flattening logic to allow UNION ALL compounds
    to be promoted upwards to replace a simple wrapper SELECT even if the
    compounds are joins.
12. Enhanced the query planner so that the xfer optimization can be used with
    INTEGER PRIMARY KEY ON CONFLICT as long as the destination table is
    initially empty.
13. Enhanced the windows VFS so that all system calls can be overridden
    using the xSetSystemCall interface.
14. Updated the "unix-dotfile" VFS to use locking directories with mkdir()
    and rmdir() instead of locking files with open() and unlink().
15. Enhancements to the test_quota.c extension to support stdio-like interfaces
    with quotas.
16. Change the unix VFS to be tolerant of read() system calls that return
    less then the full number of requested bytes.
17. Change both unix and windows VFSes to report a sector size of 4096
    instead of the old default of 512.
18. In the TCL Interface, add the -uri option to the "sqlite3" TCL command
    used for creating new database connection objects.
19. Added the SQLITE_TESTCTRL_EXPLAIN_STMT test-control option with the
    SQLITE_ENABLE_TREE_EXPLAIN compile-time option to enable the
    command-line shell to display ASCII-art parse trees of SQL statements
    that it processes, for debugging and analysis.
20. **Bug fix:** Add an additional xSync when restarting a WAL in order to prevent an
    exceedingly unlikely but theoretically possible
    database corruption following power-loss.
    Ticket ff5be73dee.
21. **Bug fix:** Change the VDBE so that all registers are initialized to Invalid
    instead of NULL.
    Ticket 7bbfb7d442
22. **Bug fix:** Fix problems that can result from 32-bit integer overflow.
    Ticket ac00f496b7e2
23. SQLITE_SOURCE_ID:
    "2012-01-16 13:28:40 ebd01a8deffb5024a5d7494eef800d2366d97204"
24. SHA1 for sqlite3.c: 6497cbbaad47220bd41e2e4216c54706e7ae95d4

### 2011-11-01 (3.7.9)

1. If a search token (on the right-hand side of the MATCH operator) in
    FTS4 begins with "^" then that token must be the first in its field
    of the document.  **** Potentially Incompatible Change ****
2. Added options SQLITE_DBSTATUS_CACHE_HIT and SQLITE_DBSTATUS_CACHE_MISS
    to the sqlite3_db_status() interface.
3. Removed support for SQLITE_ENABLE_STAT2, replacing it with the much
    more capable SQLITE_ENABLE_STAT3 option.
4. Enhancements to the sqlite3_analyzer utility program, including the
    --pageinfo and --stats options and support for multiplexed databases.
5. Enhance the sqlite3_data_count() interface so that it can be used to
    determine if SQLITE_DONE has been seen on the prepared statement.
6. Added the SQLITE_FCNTL_OVERWRITE file-control by which the SQLite core
    indicates to the VFS that the current transaction will overwrite the
    entire database file.
7. Increase the default lookaside memory allocator allocation size from
    100 to 128 bytes.
8. Enhanced the query planner so that it can factor terms in and out of
    OR expressions in the WHERE clause in an effort to find better indices.
9. Added the SQLITE_DIRECT_OVERFLOW_READ compile-time option, causing
    overflow pages to be read directly from the database file,
    bypassing the page cache.
10. Remove limits on the magnitude of precision and width value in the
    format specifiers of the sqlite3_mprintf() family of string rendering
    routines.
11. Fix a bug that prevent ALTER TABLE ... RENAME from working
    on some virtual tables in a database with a UTF16 encoding.
12. Fix a bug in ASCII-to-float conversion that causes slow performance and
    incorrect results when converting numbers with ridiculously large exponents.
13. Fix a bug that causes incorrect results in aggregate queries that use
    multiple aggregate functions whose arguments contain complicated expressions
    that differ only in the case of string literals contained within those
    expressions.
14. Fix a bug that prevented the page_count and quick_check pragmas from
    working correctly if their names were capitalized.
15. Fix a bug that caused VACUUM to fail if the count_changes pragma was
    engaged.
16. Fix a bug in virtual table implementation that causes a crash if
    an FTS4 table is dropped inside a transaction and
    a SAVEPOINT occurs afterwards.
17. SQLITE_SOURCE_ID:
    "2011-11-01 00:52:41 c7c6050ef060877ebe77b41d959e9df13f8c9b5e"
18. SHA1 for sqlite3.c: becd16877f4f9b281b91c97e106089497d71bb47

### 2011-09-19 (3.7.8)

1.  Orders of magnitude performance improvement for CREATE INDEX on
     very large tables.
2.  Improved the windows VFS to better defend against interference
     from anti-virus software.
3.  Improved query plan optimization when the DISTINCT keyword is present.
4.  Allow more system calls to be overridden in the unix VFS - to provide
     better support for chromium sandboxes.
5.  Increase the default size of a lookahead cache line from 100 to 128 bytes.
6.  Enhancements to the test_quota.c module so that it can track
     preexisting files.
7.  Bug fix: Virtual tables now handle IS NOT NULL constraints correctly.
8.  Bug fixes: Correctly handle nested correlated subqueries used with
     indices in a WHERE clause.
9.  SQLITE_SOURCE_ID:
    "2011-09-19 14:49:19 3e0da808d2f5b4d12046e05980ca04578f581177"
10.  SHA1 for sqlite3.c: bfcd74a655636b592c5dba6d0d5729c0f8e3b4de

### 2011-06-28 (3.7.7.1)

1.  Fix a bug causing
     PRAGMA case_sensitive_like statements compiled using sqlite3_prepare()
     to fail with an SQLITE_SCHEMA error.
2.  SQLITE_SOURCE_ID:
    "2011-06-28 17:39:05 af0d91adf497f5f36ec3813f04235a6e195a605f"
3.  SHA1 for sqlite3.c: d47594b8a02f6cf58e91fb673e96cb1b397aace0

### 2011-06-23 (3.7.7)

1.  Add support for URI filenames
2.  Add the sqlite3_vtab_config() interface in
     support of ON CONFLICT clauses with virtual tables.
3.  Add the xSavepoint, xRelease and xRollbackTo methods in
     virtual tables in support of SAVEPOINT for virtual tables.
4.  Update the built-in FTS3/FTS4 and RTREE virtual tables to support
     ON CONFLICT clauses and REPLACE.
5.  Avoid unnecessary reparsing of the database schema.
6.  Added support for the FTS4 prefix option and the FTS4 order option.
7.  Allow WAL-mode databases to be opened read-only as long as
     there is an existing read/write connection.
8.  Added support for short filenames.
9.  SQLITE_SOURCE_ID:
     "2011-06-23 19:49:22 4374b7e83ea0a3fbc3691f9c0c936272862f32f2"
10.  SHA1 for sqlite3.c: 5bbe79e206ae5ffeeca760dbd0d66862228db551

### 2011-05-19 (3.7.6.3)

1.  Fix a problem with WAL mode which could cause transactions to
     silently rollback if the cache_size is set very small (less than 10)
     and SQLite comes under memory pressure.

### 2011-04-17 (3.7.6.2)

1.  Fix the function prototype for the open(2) system call to agree with
     POSIX.  Without this fix, pthreads does not work correctly on NetBSD.
2.  SQLITE_SOURCE_ID:
     "2011-04-17 17:25:17 154ddbc17120be2915eb03edc52af1225eb7cb5e"
3.  SHA1 for sqlite3.c: 806577fd524dd5f3bfd8d4d27392ed2752bc9701

### 2011-04-13 (3.7.6.1)

1.  Fix a bug in 3.7.6 that only appears if the SQLITE_FCNTL_SIZE_HINT
     file control is used with a build of SQLite that makes use of the
     HAVE_POSIX_FALLOCATE compile-time option and which has
     SQLITE_ENABLE_LOCKING_MODE turned off.
2.  SQLITE_SOURCE_ID:
     "2011-04-13 14:40:25 a35e83eac7b185f4d363d7fa51677f2fdfa27695"
3.  SHA1 for sqlite3.c: b81bfa27d3e09caf3251475863b1ce6dd9f6ab66

### 2011-04-12 (3.7.6)

1.  Added the sqlite3_wal_checkpoint_v2() interface and enhanced the
     wal_checkpoint pragma to support blocking checkpoints.
2.  Improvements to the query planner so that it makes better estimates of
     plan costs and hence does a better job of choosing the right plan,
     especially when SQLITE_ENABLE_STAT2 is used.
3.  Fix a bug which prevented deferred foreign key constraints from being
     enforced when sqlite3_finalize() was not
     called by one statement with a failed foreign key constraint prior to
     another statement with foreign key constraints running.
4.  Integer arithmetic operations that would have resulted in overflow
     are now performed using floating-point instead.
5.  Increased the version number on the VFS object to
     3 and added new methods xSetSysCall, xGetSysCall, and xNextSysCall
     used for doing full-coverage testing.
6.  Increase the maximum value of SQLITE_MAX_ATTACHED from 30 to 62
     (though the default value remains at 10).
7.  Enhancements to FTS4:
       1.  Added the fts4aux table
     
  2.  Added support for compressed FTS4 content
     
8.  Enhance the ANALYZE command to support the name of an index
     as its argument, in order to analyze just that one index.
9.  Added the "unix-excl" built-in VFS on unix and unix-like platforms.
10.  SQLITE_SOURCE_ID:
     "2011-04-12 01:58:40 f9d43fa363d54beab6f45db005abac0a7c0c47a7"
11.  SHA1 for sqlite3.c: f38df08547efae0ff4343da607b723f588bbd66b

### 2011-02-01 (3.7.5)

1.  Added the sqlite3_vsnprintf() interface.
2.  Added the SQLITE_DBSTATUS_LOOKASIDE_HIT,
     SQLITE_DBSTATUS_LOOKASIDE_MISS_SIZE, and
     SQLITE_DBSTATUS_LOOKASIDE_MISS_FULL options for the
     sqlite3_db_status() interface.
3.  Added the SQLITE_OMIT_AUTORESET compile-time option.
4.  Added the SQLITE_DEFAULT_FOREIGN_KEYS compile-time option.
5.  Updates to sqlite3_stmt_readonly() so that its result is well-defined
     for all prepared statements and so that it works with VACUUM.
6.  Added the "-heap" option to the command-line shell
7.  Fix a bug involving
     frequent changes in and out of WAL mode and
     VACUUM that could (in theory) cause database corruption.
8.  Enhance the sqlite3_trace() mechanism so that nested SQL statements
     such as might be generated by virtual tables are shown but are shown
     in comments and without parameter expansion.  This
     greatly improves tracing output when using the FTS3/4 and/or RTREE
     virtual tables.
9.  Change the xFileControl() methods on all built-in VFSes to return
     SQLITE_NOTFOUND instead of SQLITE_ERROR for an unrecognized
     operation code.
10.  The SQLite core invokes the SQLITE_FCNTL_SYNC_OMITTED
     file control
     to the VFS in place of a call to xSync if the database has
     PRAGMA synchronous set to OFF.

### 2010-12-07 (3.7.4)

1.  Added the sqlite3_blob_reopen() interface to allow an existing
     sqlite3_blob object to be rebound to a new row.
2.  Use the new sqlite3_blob_reopen() interface to improve the performance
     of FTS.
3.  VFSes that do not support shared memory are allowed
     to access WAL databases if PRAGMA locking_mode is set to EXCLUSIVE.
4.  Enhancements to EXPLAIN QUERY PLAN.
5.  Added the sqlite3_stmt_readonly() interface.
6.  Added PRAGMA checkpoint_fullfsync.
7.  Added the SQLITE_FCNTL_FILE_POINTER option
     to sqlite3_file_control().
8.  Added support for FTS4 and enhancements
     to the FTS matchinfo() function.
9.  Added the test_superlock.c module which provides example
     code for obtaining an exclusive lock to a rollback or WAL database.
10.  Added the test_multiplex.c module which provides
     an example VFS that provides multiplexing (sharding)
     of a DB, splitting it over multiple files of fixed size.
11.  A very obscure bug
     associated with the or optimization was fixed.

### 2010-10-08 (3.7.3)

1.  Added the sqlite3_create_function_v2() interface that includes a
     destructor callback.
2.  Added support for custom r-tree queries using application-supplied
     callback routines to define the boundary of the query region.
3.  The default page cache strives more diligently to avoid using memory
     beyond what is allocated to it by SQLITE_CONFIG_PAGECACHE.  Or if
     using page cache is allocating from the heap, it strives to avoid
     going over the sqlite3_soft_heap_limit64(), even if
     SQLITE_ENABLE_MEMORY_MANAGEMENT is not set.
4.  Added the sqlite3_soft_heap_limit64() interface as a replacement for
     sqlite3_soft_heap_limit().
5.  The ANALYZE command now gathers statistics on tables even if they
     have no indices.
6.  Tweaks to the query planner to help it do a better job of finding the
     most efficient query plan for each query.
7.  Enhanced the internal text-to-numeric conversion routines so that they
     work with UTF8 or UTF16, thereby avoiding some UTF16-to-UTF8 text
     conversions.
8.  Fix a problem that was causing excess memory usage with large WAL
     transactions in win32 systems.
9.  The interface between the VDBE and B-Tree layer is enhanced such that
     the VDBE provides hints to the B-Tree layer letting the B-Tree layer
     know when it is safe to use hashing instead of B-Trees for transient
     tables.
10.  Miscellaneous documentation enhancements.

### 2010-08-24 (3.7.2)

1.  Fix an 
     old and very obscure bug that can lead to corruption of the
     database free-page list when incremental_vacuum is used.

### 2010-08-23 (3.7.1)

1.  Added new commands SQLITE_DBSTATUS_SCHEMA_USED and
     SQLITE_DBSTATUS_STMT_USED to the sqlite3_db_status() interface, in
     order to report out the amount of memory used to hold the schema and
     prepared statements of a connection.
2.  Increase the maximum size of a database pages from 32KiB to 64KiB.
3.  Use the LIKE optimization even if the right-hand side string contains
     no wildcards.
4.  Added the SQLITE_FCNTL_CHUNK_SIZE verb to the sqlite3_file_control()
     interface for both unix and windows, to cause database files to grow in
     large chunks in order to reduce disk fragmentation.
5.  Fixed a bug in the query planner that caused performance regressions
     relative to 3.6.23.1 on some complex joins.
6.  Fixed a typo in the OS/2 backend.
7.  Refactored the pager module.
8.  The SQLITE_MAX_PAGE_SIZE compile-time option is now silently ignored.
     The maximum page size is hard-coded at 65536 bytes.

### 2010-08-04 (3.7.0.1)

1.  Fix a potential database corruption bug that can occur if version 3.7.0
     and version 3.6.23.1 alternately write to the same database file.
     
     Ticket [51ae9cad317a1]
2.  Fix a performance regression related to the query planner enhancements
     of version 3.7.0.

### 2010-07-21 (3.7.0)

1.  Added support for write-ahead logging.
2.  Query planner enhancement - automatic transient indices are created
     when doing so reduces the estimated query time.
3.  Query planner enhancement - the ORDER BY becomes a no-op if the query
     also contains a GROUP BY clause that forces the correct output order.
4.  Add the SQLITE_DBSTATUS_CACHE_USED verb for sqlite3_db_status().
5.  The logical database size is now stored in the database header so that
     bytes can be appended to the end of the database file without corrupting
     it and so that SQLite will work correctly on systems that lack support
     for ftruncate().

### 2010-03-26 (3.6.23.1)

1.  Fix a bug in the offsets() function of FTS3
2.  Fix a missing "sync" that when omitted could lead to database
     corruption if a power failure or OS crash occurred just as a
     ROLLBACK operation was finishing.

### 2010-03-09 (3.6.23)

1.  Added the secure_delete pragma.
2.  Added the sqlite3_compileoption_used() and
     sqlite3_compileoption_get() interfaces as well as the
     compile_options pragma and the sqlite_compileoption_used() and
     sqlite_compileoption_get() SQL functions.
3.  Added the sqlite3_log() interface together with the
     SQLITE_CONFIG_LOG verb to sqlite3_config().  The ".log" command
     is added to the Command Line Interface.
4.  Improvements to FTS3.
5.  Improvements and bug-fixes in support for SQLITE_OMIT_FLOATING_POINT.
6.  The integrity_check pragma is enhanced to detect out-of-order rowids.
7.  The ".genfkey" operator has been removed from the
     Command Line Interface.
8.  Updates to the co-hosted Lemon LALR(1) parser generator. (These
     updates did not affect SQLite.)
9.  Various minor bug fixes and performance enhancements.

### 2010-01-06 (3.6.22)

1. Fix bugs that can (rarely) lead to incorrect query results when
    the CAST or OR operators are used in the WHERE clause of a query.
2. Continuing enhancements and improvements to FTS3.
3. Other miscellaneous bug fixes.

### 2009-12-07 (3.6.21)

1. The SQL output resulting from sqlite3_trace() is now modified to include
the values of bound parameters.
2. Performance optimizations targeting a specific use case from
a single high-profile user of SQLite.  A 12% reduction in the number of
CPU operations is achieved (as measured by Valgrind).  Actual performance
improvements in practice may vary depending on workload.  Changes
include:
  1. The ifnull() and coalesce() SQL functions are now implemented
  using in-line VDBE code rather than calling external functions, so that
  unused arguments need never be evaluated.
  
  2. The substr() SQL function does not bother to measure the length
  its entire input string if it is only computing a prefix
  
  3. Unnecessary OP_IsNull, OP_Affinity, and OP_MustBeInt VDBE opcodes
  are suppressed
  
  4. Various code refactorizations for performance
3. The FTS3 extension has undergone a major rework and cleanup.
New FTS3 documentation is now available.
4. The SQLITE_SECURE_DELETE compile-time option fixed to make sure that
content is deleted even when the truncate optimization applies.
5. Improvements to "dot-command" handling in the
Command Line Interface.
6. Other minor bug fixes and documentation enhancements.

### 2009-11-04 (3.6.20)

1. Optimizer enhancement: prepared statements are automatically
re-compiled when a binding on the RHS of a LIKE operator changes or
when any range constraint changes under SQLITE_ENABLE_STAT2.
2. Various minor bug fixes and documentation enhancements.

### 2009-10-30 (3.6.16.1)

1. A small patch to version 3.6.16 to fix
the OP_If bug.

### 2009-10-14 (3.6.19)

1. Added support for foreign key constraints.  Foreign key constraints
    are disabled by default.  Use the foreign_keys pragma to turn them on.
2. Generalized the IS and IS NOT operators to take arbitrary expressions
    on their right-hand side.
3. The TCL Interface has been enhanced to use the
    Non-Recursive Engine (NRE)
    interface to the TCL interpreter when linked against TCL 8.6 or later.
4. Fix a bug introduced in 3.6.18 that can lead to a segfault when an
    attempt is made to write on a read-only database.

### 2009-09-11 (3.6.18)

1. Versioning of the SQLite source code has transitioned from CVS to
    Fossil.
2. Query planner enhancements.
3. The SQLITE_ENABLE_STAT2 compile-time option causes the ANALYZE
    command to collect a small histogram of each index, to help SQLite better
    select among competing range query indices.
4. Recursive triggers can be enabled using the PRAGMA recursive_triggers
    statement.
5. Delete triggers fire when rows are removed due to a
    REPLACE conflict resolution.  This feature is only
    enabled when recursive triggers are enabled.
6. Added the SQLITE_OPEN_SHAREDCACHE and SQLITE_OPEN_PRIVATECACHE
    flags for sqlite3_open_v2() used to override the global
    shared cache mode settings for individual database connections.
7. Added improved version identification features:
    C-Preprocessor macro SQLITE_SOURCE_ID,
    C/C++ interface sqlite3_sourceid(), and SQL function sqlite_source_id().
8. Obscure bug fix on triggers
([efc02f9779]).

### 2009-08-10 (3.6.17)

1. Expose the sqlite3_strnicmp() interface for use by extensions and
    applications.
2. Remove the restriction on virtual tables and shared cache mode.
    Virtual tables and shared cache can now be used at the same time.
3. Many code simplifications and obscure bug fixes in support of
    providing 100% branch test coverage.

### 2009-06-27 (3.6.16)

1. Fix a bug (ticket #3929) that occasionally causes INSERT or UPDATE
    operations to fail on an indexed table that has a self-modifying trigger.
2. Other minor bug fixes and performance optimizations.

### 2009-06-15 (3.6.15)

1. Refactor the internal representation of SQL expressions so that they
    use less memory on embedded platforms.
2. Reduce the amount of stack space used
3. Fix an 64-bit alignment bug on HP/UX and Sparc
4. The sqlite3_create_function() family of interfaces now return
    SQLITE_MISUSE instead of SQLITE_ERROR when passed invalid
    parameter combinations.
5. When new tables are created using CREATE TABLE ... AS SELECT ... the
    datatype of the columns is the simplified SQLite datatype (TEXT, INT,
    REAL, NUMERIC, or BLOB) instead of a copy of the original datatype from
    the source table.
6. Resolve race conditions when checking for a hot rollback journal.
7. The sqlite3_shutdown() interface frees all mutexes under windows.
8. Enhanced robustness against corrupt database files
9. Continuing improvements to the test suite and fixes to obscure
    bugs and inconsistencies that the test suite improvements are
    uncovering.

### 2009-05-25 (3.6.14.2)

1. Fix a code generator bug introduced in version 3.6.14.  This bug
    can cause incorrect query results under obscure circumstances.
    Ticket #3879.

### 2009-05-19 (3.6.14.1)

1. Fix a bug in group_concat(), ticket #3841
2. Fix a performance bug in the pager cache, ticket #3844
3. Fix a bug in the sqlite3_backup implementation that can lead
    to a corrupt backup database.  Ticket #3858.

### 2009-05-07 (3.6.14)

1. Added the optional asynchronous VFS module.
2. Enhanced the query optimizer so that virtual tables are able to
    make use of OR and IN operators in the WHERE clause.
3. Speed improvements in the btree and pager layers.
4. Added the SQLITE_HAVE_ISNAN compile-time option which will cause
    the isnan() function from the standard math library to be used instead
    of SQLite's own home-brew NaN checker.
5. Countless minor bug fixes, documentation improvements, new and
    improved test cases, and code simplifications and cleanups.

### 2009-04-13 (3.6.13)

1. Fix a bug in version 3.6.12 that causes a segfault when running
    a count(*) on the sqlite_master table of an empty database. Ticket #3774.
2. Fix a bug in version 3.6.12 that causes a segfault that when
    inserting into a table using a DEFAULT value where there is a
    function as part of the DEFAULT value expression.  Ticket #3791.
3. Fix data structure alignment issues on Sparc.  Ticket #3777.
4. Other minor bug fixes.

### 2009-03-31 (3.6.12)

1. Fixed a bug that caused database corruption when an incremental_vacuum is
    rolled back in an in-memory database.  Ticket #3761.
2. Added the sqlite3_unlock_notify() interface.
3. Added the reverse_unordered_selects pragma.
4. The default page size on windows is automatically adjusted to match the
    capabilities of the underlying filesystem.
5. Add the new ".genfkey" command in the CLI for generating triggers to
    implement foreign key constraints.
6. Performance improvements for "count(*)" queries.
7. Reduce the amount of heap memory used, especially by TRIGGERs.
8. 

### 2009-02-18 (3.6.11)

1. Added the hot-backup interface.
2. Added new commands ".backup" and ".restore" to the CLI.
3. Added new methods backup and
    restore to the TCL interface.
4. Improvements to the syntax bubble
    diagrams
5. Various minor bug fixes

### 2009-01-15 (3.6.10)

1. Fix a cache coherency problem that could lead to database corruption.
    Ticket #3584.

### 2009-01-14 (3.6.9)

1. Fix two bugs, which when combined might result in incorrect
    query results.  Both bugs were harmless by themselves; only when
    they team up do they cause problems.  Ticket #3581.

### 2009-01-12 (3.6.8)

1. Added support for nested transactions
2. Enhanced the query optimizer so that it is able to use
    multiple indices to efficiently process
    OR-connected constraints
    in a WHERE clause.
3. Added support for parentheses in FTS3 query patterns using the
    SQLITE_ENABLE_FTS3_PARENTHESIS compile-time option.

### 2008-12-16 (3.6.7)

1. Reorganize the Unix interface in os_unix.c
2. Added support for "Proxy Locking" on Mac OS X.
3. Changed the prototype of the sqlite3_auto_extension() interface in a
    way that is backwards compatible but which might cause warnings in new
    builds of applications that use that interface.
4. Changed the signature of the xDlSym method of the sqlite3_vfs object
    in a way that is backwards compatible but which might cause
    compiler warnings.
5. Added superfluous casts and variable initializations in order
    to suppress nuisance compiler warnings.
6. Fixes for various minor bugs.

### 2008-11-26 (3.6.6.2)

1. Fix a bug in the b-tree delete algorithm that seems like it might be
    able to cause database corruption.  The bug was first introduced in
    version 3.6.6 by check-in [5899] on 2008-11-13.
2. Fix a memory leak that can occur following a disk I/O error.

### 2008-11-22 (3.6.6.1)

1. Fix a bug in the page cache that can lead database corruption following
    a rollback.  This bug was first introduced in version 3.6.4.
2. Two other very minor bug fixes

### 2008-11-19 (3.6.6)

1. Fix a #define that prevented memsys5 from compiling
2. Fix a problem in the virtual table commit mechanism that was causing
    a crash in FTS3.  Ticket #3497.
3. Add the application-defined page cache
4. Added built-in support for VxWorks

### 2008-11-12 (3.6.5)

1. Add the MEMORY option to the journal_mode pragma.
2. Added the sqlite3_db_mutex() interface.
3. Added the SQLITE_OMIT_TRUNCATE_OPTIMIZATION compile-time option.
4. Fixed the truncate optimization so that sqlite3_changes() and
    sqlite3_total_changes() interfaces and the count_changes pragma
    return the correct values.
5. Added the sqlite3_extended_errcode() interface.
6. The COMMIT command now succeeds even if there are pending queries.
    It returns SQLITE_BUSY if there are pending incremental BLOB I/O requests.
7. The error code is changed to SQLITE_BUSY (instead of SQLITE_ERROR)
    when an attempt is made to ROLLBACK while one or more queries are
    still pending.
8. Drop all support for the experimental memory allocators memsys4 and
    memsys6.
9. Added the SQLITE_ZERO_MALLOC compile-time option.

### 2008-10-15 (3.6.4)

1. Add option support for LIMIT and ORDER BY clauses on DELETE and
    UPDATE statements.  Only works if SQLite is compiled with
    SQLITE_ENABLE_UPDATE_DELETE_LIMIT.
2. Added the sqlite3_stmt_status() interface for performance monitoring.
3. Add the INDEXED BY clause.
4. The LOCKING_STYLE extension is now enabled by default on Mac OS X
5. Added the TRUNCATE option to PRAGMA journal_mode
6. Performance enhancements to tree balancing logic in the B-Tree layer.
7. Added the
  
  source code and
  
  documentation for the **genfkey** program for automatically generating
  triggers to enforce foreign key constraints.
8. Added the SQLITE_OMIT_TRUNCATE_OPTIMIZATION compile-time option.
9. The SQL language documentation is converted to use
syntax diagrams instead of BNF.
10. Other minor bug fixes

### 2008-09-22 (3.6.3)

1. Fix for a bug in the SELECT DISTINCT logic that was introduced by the
    prior version.
2. Other minor bug fixes

### 2008-08-30 (3.6.2)

1. Split the pager subsystem into separate pager and pcache subsystems.
2. Factor out identifier resolution procedures into separate files.
3. Bug fixes

### 2008-08-06 (3.6.1)

1. Added the lookaside memory allocator for a speed improvement in excess
    of 15% on some workloads.  (Your mileage may vary.)
2. Added the SQLITE_CONFIG_LOOKASIDE verb to sqlite3_config() to control
    the default lookaside configuration.
3. Added verbs SQLITE_STATUS_PAGECACHE_SIZE and
    SQLITE_STATUS_SCRATCH_SIZE to the sqlite3_status() interface.
4. Modified SQLITE_CONFIG_PAGECACHE and SQLITE_CONFIG_SCRATCH to remove
    the "+4" magic number in the buffer size computation.
5. Added the sqlite3_db_config() and sqlite3_db_status() interfaces for
    controlling and monitoring the lookaside allocator separately on each
    database connection.
6. Numerous other performance enhancements
7. Miscellaneous minor bug fixes

### 2008-07-16 (3.6.0 beta)

1. Modifications to the virtual file system interface
    to support a wider range of embedded systems.
    See 35to36.html for additional information.
    *** Potentially incompatible change ***
2. All C-preprocessor macros used to control compile-time options
    now begin with the prefix "SQLITE_".  This may require changes to
    applications that compile SQLite using their own makefiles and with
    custom compile-time options, hence we mark this as a
    *** Potentially incompatible change ***
3. The SQLITE_MUTEX_APPDEF compile-time option is no longer supported.
    Alternative mutex implementations can now be added at run-time using
    the sqlite3_config() interface with the SQLITE_CONFIG_MUTEX verb.
    *** Potentially incompatible change ***
4. The handling of IN and NOT IN operators that contain a NULL on their
    right-hand side expression is brought into compliance with the SQL
    standard and with other SQL database engines.  This is a bug fix,
    but as it has the potential to break legacy applications that depend
    on the older buggy behavior, we mark that as a
    *** Potentially incompatible change ***
5. The result column names generated for compound subqueries have been
    simplified to show only the name of the column of the original table and
    omit the table name.  This makes SQLite operate more like other SQL
    database engines.
6. Added the sqlite3_config() interface for doing run-time configuration
    of the entire SQLite library.
7. Added the sqlite3_status() interface used for querying run-time status
    information about the overall SQLite library and its subsystems.
8. Added the sqlite3_initialize() and sqlite3_shutdown() interfaces.
9. The SQLITE_OPEN_NOMUTEX option was added to sqlite3_open_v2().
10. Added the PRAGMA page_count command.
11. Added the sqlite3_next_stmt() interface.
12. Added a new R*Tree virtual table

### 2008-05-14 (3.5.9)

1. Added *experimental* support for the journal_mode PRAGMA and persistent journal.
2. Journal mode PERSIST is the default behavior in
    exclusive locking mode.
3. Fix a performance regression on LEFT JOIN (see ticket #3015)
    that was mistakenly introduced in version 3.5.8.
4. Performance enhancement: Reengineer the internal routines used
    to interpret and render variable-length integers.
5. Fix a buffer-overrun problem in sqlite3_mprintf() which occurs
    when a string without a zero-terminator is passed to "%.*s".
6. Always convert IEEE floating point NaN values into NULL during
    processing.  (Ticket #3060)
7. Make sure that when a connection blocks on a RESERVED lock that
    it is able to continue after the lock is released.  (Ticket #3093)
8. The "configure" scripts should now automatically configure Unix
    systems for large file support.  Improved error messages for
    when large files are encountered and large file support is disabled.
9. Avoid cache pages leaks following disk-full or I/O errors
10. And, many more minor bug fixes and performance enhancements....

### 2008-04-16 (3.5.8)

1. Expose SQLite's internal pseudo-random number generator (PRNG)
    via the sqlite3_randomness() interface
2. New interface sqlite3_context_db_handle() that returns the
    database connection handle that has invoked an application-defined
    SQL function.
3. New interface sqlite3_limit() allows size and length limits to be
    set on a per-connection basis and at run-time.
4. Improved crash-robustness: write the database page size into the rollback
    journal header.
5. Allow the VACUUM command to change the page size of a database file.
6. The xAccess() method of the VFS is allowed to return -1 to signal
    a memory allocation error.
7. Performance improvement: The OP_IdxDelete opcode uses unpacked records,
    obviating the need for one OP_MakeRecord opcode call for each index
    record deleted.
8. Performance improvement: Constant subexpressions are factored out of
    loops.
9. Performance improvement: Results of OP_Column are reused rather than
    issuing multiple OP_Column opcodes.
10. Fix a bug in the RTRIM collating sequence.
11. Fix a bug in the SQLITE_SECURE_DELETE option that was causing
    Firefox crashes.  Make arrangements to always test SQLITE_SECURE_DELETE
    prior to each release.
12. Other miscellaneous performance enhancements.
13. Other miscellaneous minor bug fixes.

### 2008-03-17 (3.5.7)

1. Fix a bug (ticket #2927) in the register allocation for
compound selects - introduced by the new VM code in version 3.5.5.
2. ALTER TABLE uses double-quotes instead of single-quotes for quoting
filenames.
3. Use the WHERE clause to reduce the size of a materialized VIEW in
an UPDATE or DELETE statement. (Optimization)
4. Do not apply the flattening optimization if the outer query is an
aggregate and the inner query contains ORDER BY.  (Ticket #2943)
5. Additional OS/2 updates
6. Added an experimental power-of-two, first-fit memory allocator.
7. Remove all instances of sprintf() from the code
8. Accept "Z" as the zulu timezone at the end of date strings
9. Fix a bug in the LIKE optimizer that occurs when the last character
before the first wildcard is an upper-case "Z"
10. Added the "bitvec" object for keeping track of which pages have
been journalled.  Improves speed and reduces memory consumption, especially
for large database files.
11. Get the SQLITE_ENABLE_LOCKING_STYLE macro working again on Mac OS X.
12. Store the statement journal in the temporary file directory instead of
collocated with the database file.
13. Many improvements and cleanups to the configure script

### 2008-02-06 (3.5.6)

1. Fix a bug (ticket #2913)
that prevented virtual tables from working in a LEFT JOIN.
The problem was introduced into shortly before the 3.5.5 release.
2. Bring the OS/2 porting layer up-to-date.
3. Add the new sqlite3_result_error_code() API and use it in the
implementation of ATTACH so that proper error codes are returned
when an ATTACH fails.

### 2008-01-31 (3.5.5)

1. Convert the underlying virtual machine to be a register-based machine
rather than a stack-based machine.  The only user-visible change
is in the output of EXPLAIN.
2. Add the build-in RTRIM collating sequence.

### 2007-12-14 (3.5.4)

1. Fix a critical bug in UPDATE or DELETE that occurs when an
OR REPLACE clause or a trigger causes rows in the same table to
be deleted as side effects.  (See ticket #2832.)  The most likely
result of this bug is a segmentation fault, though database
corruption is a possibility.
2. Bring the processing of ORDER BY into compliance with the
SQL standard for case where a result alias and a table column name
are in conflict.  Correct behavior is to prefer the result alias.
Older versions of SQLite incorrectly picked the table column.
(See ticket #2822.)
3. The VACUUM command preserves
the setting of the
legacy_file_format pragma.
(Ticket #2804.)
4. Productize and officially support the group_concat() SQL function.
5. Better optimization of some IN operator expressions.
6. Add the ability to change the
auto_vacuum status of a
database by setting the auto_vaccum pragma and VACUUMing the database.
7. Prefix search in FTS3 is much more efficient.
8. Relax the SQL statement length restriction in the CLI so that
the ".dump" output of databases with very large BLOBs and strings can
be played back to recreate the database.
9. Other small bug fixes and optimizations.

### 2007-11-27 (3.5.3)

1. Move website and documentation files out of the source tree into
a separate CM system.
2. Fix a long-standing bug in INSERT INTO ... SELECT ... statements
where the SELECT is compound.
3. Fix a long-standing bug in RAISE(IGNORE) as used in BEFORE triggers.
4. Fixed the operator precedence for the ~ operator.
5. On Win32, do not return an error when attempting to delete a file
that does not exist.
6. Allow collating sequence names to be quoted.
7. Modify the TCL interface to use sqlite3_prepare_v2().
8. Fix multiple bugs that can occur following a malloc() failure.
9. sqlite3_step() returns SQLITE_MISUSE instead of crashing when
called with a NULL parameter.
10. FTS3 now uses the SQLite memory allocator exclusively.  The
FTS3 amalgamation can now be appended to the SQLite amalgamation to
generate a super-amalgamation containing both.
11. The DISTINCT keyword now will sometimes use an INDEX if an
appropriate index is available and the optimizer thinks its use
might be advantageous.

### 2007-11-05 (3.5.2)

1. Dropped support for the SQLITE_OMIT_MEMORY_ALLOCATION compile-time
option.
2. Always open files using FILE_FLAG_RANDOM_ACCESS under Windows.
3. The 3rd parameter of the built-in SUBSTR() function is now optional.
4. Bug fix: do not invoke the authorizer when reparsing the schema after
a schema change.
5. Added the experimental malloc-free memory allocator in mem3.c.
6. Virtual machine stores 64-bit integer and floating point constants
in binary instead of text for a performance boost.
7. Fix a race condition in test_async.c.
8. Added the ".timer" command to the CLI

### 2007-10-04 (3.5.1)

1. ***Nota Bene:** We are not using terms "alpha" or "beta" on this
    release because the code is stable and because if we use those terms,
    nobody will upgrade.  However, we still reserve the right to make
    incompatible changes to the new VFS interface in future releases.*
2. Fix a bug in the handling of SQLITE_FULL errors that could lead
    to database corruption.  Ticket #2686.
3. The test_async.c drive now does full file locking and works correctly
    when used simultaneously by multiple processes on the same database.
4. The CLI ignores whitespace (including comments) at the end of lines
5. Make sure the query optimizer checks dependencies on all terms of
    a compound SELECT statement.  Ticket #2640.
6. Add demonstration code showing how to build a VFS for a raw
    mass storage without a filesystem.
7. Added an output buffer size parameter to the xGetTempname() method
    of the VFS layer.
8. Sticky SQLITE_FULL or SQLITE_IOERR errors in the pager are reset
    when a new transaction is started.

### 2007-09-04 (3.5.0) alpha

1. Redesign the OS interface layer.  See
    34to35.html for details.
    *** Potentially incompatible change ***
2. The sqlite3_release_memory(), sqlite3_soft_heap_limit(),
    and sqlite3_enable_shared_cache() interfaces now work cross all
    threads in the process, not just the single thread in which they
    are invoked.
    *** Potentially incompatible change ***
3. Added the sqlite3_open_v2() interface.
4. Reimplemented the memory allocation subsystem and made it
    replaceable at compile-time.
5. Created a new mutex subsystem and made it replicable at
    compile-time.
6. The same database connection may now be used simultaneously by
    separate threads.

### 2007-08-13 (3.4.2)

1. Fix a database corruption bug that might occur if a ROLLBACK command
is executed in auto-vacuum mode
and a very small sqlite3_soft_heap_limit is set.
Ticket #2565.
2. Add the ability to run a full regression test with a small
sqlite3_soft_heap_limit.
3. Fix other minor problems with using small soft heap limits.
4. Work-around for
GCC bug 32575.
5. Improved error detection of misused aggregate functions.
6. Improvements to the amalgamation generator script so that all symbols
are prefixed with either SQLITE_PRIVATE or SQLITE_API.

### 2007-07-20 (3.4.1)

1. Fix a bug in VACUUM that can lead to
    database corruptio if two
    processes are connected to the database at the same time and one
    VACUUMs then the other then modifies the database.
2. The expression "+column" is now considered the same as "column"
    when computing the collating sequence to use on the expression.
3. In the TCL language interface,
   "@variable" instead of "$variable" always binds as a blob.
4. Added PRAGMA freelist_count
    for determining the current size of the freelist.
5. The 
    PRAGMA auto_vacuum=incremental setting is now persistent.
6. Add FD_CLOEXEC to all open files under Unix.
7. Fix a bug in the 
    min()/max() optimization when applied to
    descending indices.
8. Make sure the TCL language interface works correctly with 64-bit
    integers on 64-bit machines.
9. Allow the value -9223372036854775808 as an integer literal in SQL
    statements.
10. Add the capability of "hidden" columns in virtual tables.
11. Use the macro SQLITE_PRIVATE (defaulting to "static") on all
    internal functions in the amalgamation.
12. Add pluggable tokenizers and ICU
    tokenization support to FTS2
13. Other minor bug fixes and documentation enhancements

### 2007-06-18 (3.4.0)

1. Fix a bug that can lead to database corruption if an SQLITE_BUSY error
    occurs in the middle of an explicit transaction and that transaction
    is later committed.  Ticket #2409.
2. Fix a bug that can lead to database corruption if autovacuum mode is
    on and a malloc() failure follows a CREATE TABLE or CREATE INDEX statement
    which itself follows a cache overflow inside a transaction.  See
    ticket #2418.
    
3. Added explicit upper bounds on the sizes and
    quantities of things SQLite can process.  This change might cause
    compatibility problems for
    applications that use SQLite in the extreme, which is why the current
    release is 3.4.0 instead of 3.3.18.
4. Added support for Incremental BLOB I/O.
5. Added the sqlite3_bind_zeroblob() API
    and the zeroblob() SQL function.
6. Added support for 
    Incremental Vacuum.
7. Added the SQLITE_MIXED_ENDIAN_64BIT_FLOAT compile-time option to support
    ARM7 processors with goofy endianness.
8. Removed all instances of sprintf() and strcpy() from the core library.
9. Added support for
    International Components for Unicode (ICU)
    to the full-text search extensions.
10. In the Windows OS driver, reacquire a SHARED lock if an attempt to
    acquire an EXCLUSIVE lock fails.  Ticket #2354
11. Fix the REPLACE() function so that it returns NULL if the second argument
    is an empty string.  Ticket #2324.
12. Document the hazards of type conversions in
    sqlite3_column_blob()
    and related APIs.  Fix unnecessary type conversions.  Ticket #2321.
13. Internationalization of the TRIM() function.  Ticket #2323
14. Use memmove() instead of memcpy() when moving between memory regions
    that might overlap.  Ticket #2334
15. Fix an optimizer bug involving subqueries in a compound SELECT that has
    both an ORDER BY and a LIMIT clause.  Ticket #2339.
16. Make sure the sqlite3_snprintf()
    interface does not zero-terminate the buffer if the buffer size is
    less than 1.  Ticket #2341
17. Fix the built-in printf logic so that it prints "NaN" not "Inf" for
    floating-point NaNs.  Ticket #2345
18. When converting BLOB to TEXT, use the text encoding of the main database.
    Ticket #2349
19. Keep the full precision of integers (if possible) when casting to
    NUMERIC.  Ticket #2364
20. Fix a bug in the handling of UTF16 codepoint 0xE000
21. Consider explicit collate clauses when matching WHERE constraints
    to indices in the query optimizer.  Ticket #2391
22. Fix the query optimizer to correctly handle constant expressions in
    the ON clause of a LEFT JOIN.  Ticket #2403
23. Fix the query optimizer to handle rowid comparisons to NULL
    correctly.  Ticket #2404
24. Fix many potential segfaults that could be caused by malicious SQL
    statements.

### 2007-04-25 (3.3.17)

1. When the "write_version" value of the database header is larger than
    what the library understands, make the database read-only instead of
    unreadable.
2. Other minor bug fixes

### 2007-04-18 (3.3.16)

1. Fix a bug that caused VACUUM to fail if NULLs appeared in a
    UNIQUE column.
2. Reinstate performance improvements that were added in
    Version 3.3.14
    but regressed in Version 3.3.15.
3. Fix problems with the handling of ORDER BY expressions on
    compound SELECT statements in subqueries.
4. Fix a potential segfault when destroying locks on WinCE in
    a multi-threaded environment.
5. Documentation updates.

### 2007-04-09 (3.3.15)

1. Fix a bug introduced in 3.3.14 that caused a rollback of
    CREATE TEMP TABLE to leave the database connection wedged.
2. Fix a bug that caused an extra NULL row to be returned when
    a descending query was interrupted by a change to the database.
3. The FOR EACH STATEMENT clause on a trigger now causes a syntax
    error.  It used to be silently ignored.
4. Fix an obscure and relatively harmless problem that might have caused
    a resource leak following an I/O error.
5. Many improvements to the test suite.  Test coverage now exceeded 98%

### 2007-04-02 (3.3.14)

1. Fix a bug (ticket #2273)
    that could cause a segfault when the IN operator
    is used one term of a two-column index and the right-hand side of
    the IN operator contains a NULL.
2. Added a new OS interface method for determining the sector size
    of underlying media:  sqlite3OsSectorSize().
3. A new algorithm for statements of the form
    INSERT INTO *table1* SELECT * FROM*table2* is faster and reduces fragmentation.  VACUUM uses statements of
    this form and thus runs faster and defragments better.
4. Performance enhancements through reductions in disk I/O:
  1. Do not read the last page of an overflow chain when
      deleting the row - just add that page to the freelist.
  2. Do not store pages being deleted in the
      rollback journal.
  3. Do not read in the (meaningless) content of
      pages extracted from the freelist.
  4. Do not flush the page cache (and thus avoiding
      a cache refill) unless another process changes the underlying
      database file.
  5. Truncate rather than delete the rollback journal when committing
      a transaction in exclusive access mode, or when committing the TEMP
      database.
5. Added support for exclusive access mode using
    
    "PRAGMA locking_mode=EXCLUSIVE"
6. Use heap space instead of stack space for large buffers in the
    pager - useful on embedded platforms with  stack-space
    limitations.
7. Add a makefile target "sqlite3.c" that builds an amalgamation containing
    the core SQLite library C code in a single file.
8. Get the library working correctly when compiled
    with GCC option "-fstrict-aliasing".
9. Removed the vestigal SQLITE_PROTOCOL error.
10. Improvements to test coverage, other minor bugs fixed,
    memory leaks plugged,
    code refactored and/or recommended in places for easier reading.

### 2007-02-13 (3.3.13)

1. Add a "fragmentation" measurement in the output of sqlite3_analyzer.
2. Add the COLLATE operator used to explicitly set the collating sequence
used by an expression.  This feature is considered experimental pending
additional testing.
3. Allow up to 64 tables in a join - the old limit was 32.
4. Added two new experimental functions:
randomblob() and
hex().
Their intended use is to facilitate generating
UUIDs.
5. Fix a problem where
PRAGMA count_changes was
causing incorrect results for updates on tables with triggers
6. Fix a bug in the ORDER BY clause optimizer for joins where the
left-most table in the join is constrained by a UNIQUE index.
7. Fixed a bug in the "copy" method of the TCL interface.
8. Bug fixes in fts1 and fts2 modules.

### 2007-01-27 (3.3.12)

1. Fix another bug in the IS NULL optimization that was added in
version 3.3.9.
2. Fix an assertion fault that occurred on deeply nested views.
3. Limit the amount of output that
PRAGMA integrity_check
generates.
4. Minor syntactic changes to support a wider variety of compilers.

### 2007-01-22 (3.3.11)

1. Fix another bug in the implementation of the new
sqlite3_prepare_v2() API.
We'll get it right eventually...
2. Fix a bug in the IS NULL optimization that was added in version 3.3.9 -
the bug was causing incorrect results on certain LEFT JOINs that included
