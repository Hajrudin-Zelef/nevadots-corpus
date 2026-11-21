---
id: collect-261121-veille-ia/actu-business/release-history-5
title: "Release History"
domain: actu-business
role: reference
task: article
actors: []
dates: ["2007-01-09"]
keywords: ["added", "sqlite3", "support", "function", "error", "default", "optimizer", "clause"]
source: https://sqlite.org/draft/changes.html
source_anchor: ""
source_lines: [6001, 7500]
section: "Actu_Business"
---
in the WHERE clause an IS NULL constraint for the right table of the
LEFT JOIN.
3. Make AreFileApisANSI() a no-op macro in WinCE since WinCE does not
support this function.

### 2007-01-09 (3.3.10)

1. Fix bugs in the implementation of the new
sqlite3_prepare_v2() API
that can lead to segfaults.
2. Fix 1-second round-off errors in the
strftime() function
3. Enhance the Windows OS layer to provide detailed error codes
4. Work around a win2k problem so that SQLite can use single-character
database file names
5. The
user_version and
schema_version pragmas
correctly set their column names in the result set
6. Documentation updates

### 2007-01-04 (3.3.9)

1. Fix bugs in pager.c that could lead to database corruption if two
processes both try to recover a hot journal at the same instant
2. Added the sqlite3_prepare_v2()
API.
3. Fixed the ".dump" command in the command-line shell to show
indices, triggers and views again.
4. Change the table_info pragma so that it returns NULL for the default
value if there is no default value
5. Support for non-ASCII characters in win95 filenames
6. Query optimizer enhancements:
  1. Optimizer does a better job of using indices to satisfy ORDER BY
  clauses that sort on the integer primary key
  2. Use an index to satisfy an IS NULL operator in the WHERE clause
  3. Fix a bug that was causing the optimizer to miss an OR optimization
  opportunity
  4. The optimizer has more freedom to reorder tables in the FROM clause
  even in there are LEFT joins.
7. Extension loading supported added to WinCE
8. Allow constraint names on the DEFAULT clause in a table definition
9. Added the ".bail" command to the command-line shell
10. Make CSV (comma separate value) output from the command-line shell
more closely aligned to accepted practice
11. Experimental FTS2 module added
12. Use sqlite3_mprintf() instead of strdup() to avoid libc dependencies
13. VACUUM uses a temporary file in the official TEMP folder, not in the
same directory as the original database
14. The prefix on temporary filenames on Windows is changed from "sqlite"
to "etilqs".

### 2006-10-09 (3.3.8)

1. Support for full text search using the FTS1 module (beta)
2. Added Mac OS X locking patches (beta - disabled by default)
3. Introduce extended error codes and add error codes for various
kinds of I/O errors.
4. Added support for IF EXISTS on CREATE/DROP TRIGGER/VIEW
5. Fix the regression test suite so that it works with Tcl8.5
6. Enhance sqlite3_set_authorizer() to provide notification of calls to
    SQL functions.
7. Added experimental API:  sqlite3_auto_extension()
8. Various minor bug fixes

### 2006-08-12 (3.3.7)

1. Added support for virtual tables (beta)
2. Added support for dynamically loaded extensions (beta)
3. The
sqlite3_interrupt()
routine can be called for a different thread
4. Added the MATCH operator.
5. The default file format is now 1.

### 2006-06-06 (3.3.6)

1. Plays better with virus scanners on Windows
2. Faster :memory: databases
3. Fix an obscure segfault in UTF-8 to UTF-16 conversions
4. Added driver for OS/2
5. Correct column meta-information returned for aggregate queries
6. Enhanced output from EXPLAIN QUERY PLAN
7. LIMIT 0 now works on subqueries
8. Bug fixes and performance enhancements in the query optimizer
9. Correctly handle NULL filenames in ATTACH and DETACH
10. Improved syntax error messages in the parser
11. Fix type coercion rules for the IN operator

### 2006-04-05 (3.3.5)

1. CHECK constraints use conflict resolution algorithms correctly.
2. The SUM() function throws an error on integer overflow.
3. Choose the column names in a compound query from the left-most SELECT
    instead of the right-most.
4. The sqlite3_create_collation() function
    honors the SQLITE_UTF16_ALIGNED flag.
5. SQLITE_SECURE_DELETE compile-time option causes deletes to overwrite
    old data with zeros.
6. Detect integer overflow in abs().
7. The random() function provides 64 bits of randomness instead of
    only 32 bits.
8. Parser detects and reports automaton stack overflow.
9. Change the round() function to return REAL instead of TEXT.
10. Allow WHERE clause terms on the left table of a LEFT OUTER JOIN to
    contain aggregate subqueries.
11. Skip over leading spaces in text to numeric conversions.
12. Various minor bug and documentation typo fixes and
    performance enhancements.

### 2006-02-11 (3.3.4)

1. Fix a blunder in the Unix mutex implementation that can lead to
deadlock on multithreaded systems.
2. Fix an alignment problem on 64-bit machines
3. Added the fullfsync pragma.
4. Fix an optimizer bug that could have caused some unusual LEFT OUTER JOINs
to give incorrect results.
5. The SUM function detects integer overflow and converts to accumulating
an approximate result using floating point numbers
6. Host parameter names can begin with '@' for compatibility with SQL Server.
7. Other miscellaneous bug fixes

### 2006-01-31 (3.3.3)

1. Removed support for an ON CONFLICT clause on CREATE INDEX - it never
worked correctly so this should not present any backward compatibility
problems.
2. Authorizer callback now notified of ALTER TABLE ADD COLUMN commands
3. After any changes to the TEMP database schema, all prepared statements
are invalidated and must be recreated using a new call to
sqlite3_prepare()
4. Other minor bug fixes in preparation for the first stable release
of version 3.3

### 2006-01-24 (3.3.2 beta)

1. Bug fixes and speed improvements.  Improved test coverage.
2. Changes to the OS-layer interface: mutexes must now be recursive.
3. Discontinue the use of thread-specific data for out-of-memory
exception handling

### 2006-01-16 (3.3.1 alpha)

1. Countless bug fixes
2. Speed improvements
3. Database connections can now be used by multiple threads, not just
the thread in which they were created.

### 2006-01-11 (3.3.0 alpha)

1. CHECK constraints
2. IF EXISTS and IF NOT EXISTS clauses on CREATE/DROP TABLE/INDEX.
3. DESC indices
4. More efficient encoding of boolean values resulting in smaller database
files
5. More aggressive SQLITE_OMIT_FLOATING_POINT
6. Separate INTEGER and REAL affinity
7. Added a virtual function layer for the OS interface
8. "exists" method added to the TCL interface
9. Improved response to out-of-memory errors
10. Database cache can be optionally shared between connections
in the same thread
11. Optional READ UNCOMMITTED isolation (instead of the default
isolation level of SERIALIZABLE) and table level locking when
database connections share a common cache.

### 2005-12-19 (3.2.8)

1. Fix an obscure bug that can cause database corruption under the
following unusual circumstances: A large INSERT or UPDATE statement which
is part of an even larger transaction fails due to a uniqueness constraint
but the containing transaction commits.

### 2005-12-19 (2.8.17)

1. Fix an obscure bug that can cause database corruption under the
following unusual circumstances: A large INSERT or UPDATE statement which
is part of an even larger transaction fails due to a uniqueness contraint
but the containing transaction commits.

### 2005-09-24 (3.2.7)

1. GROUP BY now considers NULLs to be equal again, as it should
2. Now compiles on Solaris and OpenBSD and other Unix variants
that lack the fdatasync() function
3. Now compiles on MSVC++6 again
4. Fix uninitialized variables causing malfunctions for various obscure
queries
5. Correctly compute a LEFT OUTER JOINs that is constrained on the
left table only

### 2005-09-17 (3.2.6)

1. Fix a bug that can cause database corruption if a VACUUM (or
    autovacuum) fails and is rolled back on a database that is
    larger than 1GiB
2. LIKE optimization now works for columns with COLLATE NOCASE
3. ORDER BY and GROUP BY now use bounded memory
4. Added support for COUNT(DISTINCT expr)
5. Change the way SUM() handles NULL values in order to comply with
    the SQL standard
6. Use fdatasync() instead of fsync() where possible in order to speed
    up commits slightly
7. Use of the CROSS keyword in a join turns off the table reordering
    optimization
8. Added the experimental and undocumented EXPLAIN QUERY PLAN capability
9. Use the unicode API in Windows

### 2005-08-27 (3.2.5)

1. Fix a bug effecting DELETE and UPDATE statements that changed
more than 40960 rows.
2. Change the makefile so that it no longer requires GNUmake extensions
3. Fix the --enable-threadsafe option on the configure script
4. Fix a code generator bug that occurs when the left-hand side of an IN
operator is constant and the right-hand side is a SELECT statement
5. The PRAGMA synchronous=off statement now disables syncing of the
master journal file in addition to the normal rollback journals

### 2005-08-24 (3.2.4)

1. Fix a bug introduced in the previous release
that can cause a segfault while generating code
for complex WHERE clauses.
2. Allow floating point literals to begin or end with a decimal point.

### 2005-08-21 (3.2.3)

1. Added support for the CAST operator
2. Tcl interface allows BLOB values to be transferred to user-defined
functions
3. Added the "transaction" method to the Tcl interface
4. Allow the DEFAULT value of a column to call functions that have constant
operands
5. Added the ANALYZE command for gathering statistics on indices and
using those statistics when picking an index in the optimizer
6. Remove the limit (formerly 100) on the number of terms in the
WHERE clause
7. The right-hand side of the IN operator can now be a list of expressions
instead of just a list of constants
8. Rework the optimizer so that it is able to make better use of indices
9. The order of tables in a join is adjusted automatically to make
better use of indices
10. The IN operator is now a candidate for optimization even if the left-hand
side is not the left-most term of the index.  Multiple IN operators can be
used with the same index.
11. WHERE clause expressions using BETWEEN and OR are now candidates
for optimization
12. Added the "case_sensitive_like" pragma and the SQLITE_CASE_SENSITIVE_LIKE
compile-time option to set its default value to "on".
13. Use indices to help with GLOB expressions and LIKE expressions too
when the case_sensitive_like pragma is enabled
14. Added support for grave-accent quoting for compatibility with MySQL
15. Improved test coverage
16. Dozens of minor bug fixes

### 2005-06-12 (3.2.2)

1. Added the sqlite3_db_handle() API
2. Added the sqlite3_get_autocommit() API
3. Added a REGEXP operator to the parser.  There is no function to back
up this operator in the standard build but users can add their own using
sqlite3_create_function()
4. Speed improvements and library footprint reductions.
5. Fix byte alignment problems on 64-bit architectures.
6. Many, many minor bug fixes and documentation updates.

### 2005-03-29 (3.2.1)

1. Fix a memory allocation error in the new ADD COLUMN comment.
2. Documentation updates

### 2005-03-21 (3.2.0)

1. Added support for ALTER TABLE ADD COLUMN.
2. Added support for the "T" separator in ISO-8601 date/time strings.
3. Improved support for Cygwin.
4. Numerous bug fixes and documentation updates.

### 2005-03-17 (3.1.6)

1. Fix a bug that could cause database corruption when inserting
    record into tables with around 125 columns.
2. sqlite3_step() is now much more likely to invoke the busy handler
    and less likely to return SQLITE_BUSY.
3. Fix memory leaks that used to occur after a malloc() failure.

### 2005-03-11 (3.1.5)

1. The ioctl on Mac OS X to control syncing to disk is F_FULLFSYNC,
    not F_FULLSYNC.  The previous release had it wrong.

### 2005-03-11 (3.1.4)

1. Fix a bug in autovacuum that could cause database corruption if
a CREATE UNIQUE INDEX fails because of a constraint violation.
This problem only occurs if the new autovacuum feature introduced in
version 3.1 is turned on.
2. The F_FULLSYNC ioctl (currently only supported on Mac OS X) is disabled
if the synchronous pragma is set to something other than "full".
3. Add additional forward compatibility to the future version 3.2 database
file format.
4. Fix a bug in WHERE clauses of the form (rowid<'2')
5. New SQLITE_OMIT_... compile-time options added
6. Updates to the man page
7. Remove the use of strcasecmp() from the shell
8. Windows DLL exports symbols Tclsqlite_Init and Sqlite_Init

### 2005-02-19 (3.1.3)

1. Fix a problem with VACUUM on databases from which tables containing
AUTOINCREMENT have been dropped.
2. Add forward compatibility to the future version 3.2 database file
format.
3. Documentation updates

### 2005-02-15 (3.1.2)

1. Fix a bug that can lead to database corruption if there are two
open connections to the same database and one connection does a VACUUM
and the second makes some change to the database.
2. Allow "?" parameters in the LIMIT clause.
3. Fix VACUUM so that it works with AUTOINCREMENT.
4. Fix a race condition in AUTOVACUUM that can lead to corrupt databases
5. Add a numeric version number to the sqlite3.h include file.
6. Other minor bug fixes and performance enhancements.

### 2005-02-15 (2.8.16)

1. Fix a bug that can lead to database corruption if there are two
open connections to the same database and one connection does a VACUUM
and the second makes some change to the database.
2. Correctly handle quoted names in CREATE INDEX statements.
3. Fix a naming conflict between sqlite.h and sqlite3.h.
4. Avoid excess heap usage when copying expressions.
5. Other minor bug fixes.

### 2005-02-01 (3.1.1 BETA)

1. Automatic caching of prepared statements in the TCL interface
2. ATTACH and DETACH as well as some other operations cause existing
    prepared statements to expire.
3. Numerous minor bug fixes

### 2005-01-21 (3.1.0 ALPHA)

1. Autovacuum support added
2. CURRENT_TIME, CURRENT_DATE, and CURRENT_TIMESTAMP added
3. Support for the EXISTS clause added.
4. Support for correlated subqueries added.
5. Added the ESCAPE clause on the LIKE operator.
6. Support for ALTER TABLE ... RENAME TABLE ... added
7. AUTOINCREMENT keyword supported on INTEGER PRIMARY KEY
8. Many SQLITE_OMIT_ macros inserts to omit features at compile-time
    and reduce the library footprint.
9. The REINDEX command was added.
10. The engine no longer consults the main table if it can get
    all the information it needs from an index.
11. Many nuisance bugs fixed.

### 2004-10-12 (3.0.8)

1. Add support for DEFERRED, IMMEDIATE, and EXCLUSIVE transactions.
2. Allow new user-defined functions to be created when there are
already one or more precompiled SQL statements.
3. 
4. Fix portability problems for MinGW/MSYS.
5. Fix a byte alignment problem on 64-bit Sparc machines.
6. Fix the ".import" command of the shell so that it ignores \r
characters at the end of lines.
7. The "csv" mode option in the shell puts strings inside double-quotes.
8. Fix typos in documentation.
9. Convert array constants in the code to have type "const".
10. Numerous code optimizations, specially optimizations designed to
make the code footprint smaller.

### 2004-09-18 (3.0.7)

1. The BTree module allocates large buffers using malloc() instead of
    off of the stack, in order to play better on machines with limited
    stack space.
2. Fixed naming conflicts so that versions 2.8 and 3.0 can be
    linked and used together in the same ANSI-C source file.
3. New interface: sqlite3_bind_parameter_index()
4. Add support for wildcard parameters of the form: "?nnn"
5. Fix problems found on 64-bit systems.
6. Removed encode.c file (containing unused routines) from the
    version 3.0 source tree.
7. The sqlite3_trace() callbacks occur before each statement
    is executed, not when the statement is compiled.
8. Makefile updates and miscellaneous bug fixes.

### 2004-09-02 (3.0.6 beta)

1. Better detection and handling of corrupt database files.
2. The sqlite3_step() interface returns SQLITE_BUSY if it is unable
    to commit a change because of a lock
3. Combine the implementations of LIKE and GLOB into a single
    pattern-matching subroutine.
4. Miscellaneous code size optimizations and bug fixes

### 2004-08-29 (3.0.5 beta)

1. Support for ":AAA" style bind parameter names.
2. Added the new sqlite3_bind_parameter_name() interface.
3. Support for TCL variable names embedded in SQL statements in the
    TCL bindings.
4. The TCL bindings transfer data without necessarily doing a conversion
    to a string.
5. The database for TEMP tables is not created until it is needed.
6. Add the ability to specify an alternative temporary file directory
    using the "sqlite_temp_directory" global variable.
7. A compile-time option (SQLITE_BUSY_RESERVED_LOCK) causes the busy
    handler to be called when there is contention for a RESERVED lock.
8. Various bug fixes and optimizations

### 2004-08-09 (3.0.4 beta)

1. CREATE TABLE and DROP TABLE now work correctly as prepared statements.
2. Fix a bug in VACUUM and UNIQUE indices.
3. Add the ".import" command to the command-line shell.
4. Fix a bug that could cause index corruption when an attempt to
    delete rows of a table is blocked by a pending query.
5. Library size optimizations.
6. Other minor bug fixes.

### 2004-07-22 (2.8.15)

1. This is a maintenance release only.  Various minor bugs have been
fixed and some portability enhancements are added.

### 2004-07-22 (3.0.3 beta)

1. The second beta release for SQLite 3.0.
2. Add support for "PRAGMA page_size" to adjust the page size of
the database.
3. Various bug fixes and documentation updates.

### 2004-06-30 (3.0.2 beta)

1. The first beta release for SQLite 3.0.

### 2004-06-22 (3.0.1 alpha)

1. 
    ***** Alpha Release - Research And Testing Use Only *****
2. Lots of bug fixes.

### 2004-06-18 (3.0.0 alpha)

1. 
    ***** Alpha Release - Research And Testing Use Only *****
2. Support for internationalization including UTF-8, UTF-16, and
    user defined collating sequences.
3. New file format that is 25% to 35% smaller for typical use.
4. Improved concurrency.
5. Atomic commits for ATTACHed databases.
6. Remove cruft from the APIs.
7. BLOB support.
8. 64-bit rowids.
9. More information.

### 2004-06-09 (2.8.14)

1. Fix the min() and max() optimizer so that it works when the FROM
    clause consists of a subquery.
2. Ignore extra whitespace at the end of "." commands in the shell.
3. Bundle sqlite_encode_binary() and sqlite_decode_binary() with the
    library.
4. The TEMP_STORE and DEFAULT_TEMP_STORE pragmas now work.
5. Code changes to compile cleanly using OpenWatcom.
6. Fix VDBE stack overflow problems with INSTEAD OF triggers and
    NULLs in IN operators.
7. Add the global variable sqlite_temp_directory which if set defines the
    directory in which temporary files are stored.
8. sqlite_interrupt() plays well with VACUUM.
9. Other minor bug fixes.

### 2004-03-08 (2.8.13)

1. Refactor parts of the code in order to make the code footprint
    smaller.  The code is now also a little bit faster.
2. sqlite_exec() is now implemented as a wrapper around sqlite_compile()
    and sqlite_step().
3. The built-in min() and max() functions now honor the difference between
    NUMERIC and TEXT datatypes.  Formerly, min() and max() always assumed
    their arguments were of type NUMERIC.
4. New HH:MM:SS modifier to the built-in date/time functions.
5. Experimental sqlite_last_statement_changes() API added.  Fixed
    the last_insert_rowid() function so that it works correctly with
    triggers.
6. Add functions prototypes for the database encryption API.
7. Fix several nuisance bugs.

### 2004-02-08 (2.8.12)

1. Fix a bug that will might corrupt the rollback journal if a power failure
    or external program halt occurs in the middle of a COMMIT.  The corrupt
    journal can lead to database corruption when it is rolled back.
2. Reduce the size and increase the speed of various modules, especially
    the virtual machine.
3. Allow "<expr> IN <table>" as a shorthand for
    "<expr> IN (SELECT * FROM <table>".
4. Optimizations to the sqlite_mprintf() routine.
5. Make sure the MIN() and MAX() optimizations work within subqueries.

### 2004-01-14 (2.8.11)

1. Fix a bug in how the IN operator handles NULLs in subqueries.  The bug
    was introduced by the previous release.

### 2004-01-14 (2.8.10)

1. Fix a potential database corruption problem on Unix caused by the fact
    that all POSIX advisory locks are cleared whenever you close() a file.
    The work around it to embargo all close() calls while locks are
    outstanding.
2. Performance enhancements on some corner cases of COUNT(*).
3. Make sure the in-memory backend response sanely if malloc() fails.
4. Allow sqlite_exec() to be called from within user-defined SQL
    functions.
5. Improved accuracy of floating-point conversions using "long double".
6. Bug fixes in the experimental date/time functions.

### 2004-01-06 (2.8.9)

1. Fix a 32-bit integer overflow problem that could result in corrupt
    indices in a database if large negative numbers (less than -2147483648)
    were inserted into an indexed numeric column.
2. Fix a locking problem on multi-threaded Linux implementations.
3. Always use "." instead of "," as the decimal point even if the locale
    requests ",".
4. Added UTC to localtime conversions to the experimental date/time
    functions.
5. Bug fixes to date/time functions.

### 2003-12-18 (2.8.8)

1. Fix a critical bug introduced into 2.8.0 which could cause
    database corruption.
2. Fix a problem with 3-way joins that do not use indices
3. The VACUUM command now works with the non-callback API
4. Improvements to the "PRAGMA integrity_check" command

### 2003-12-04 (2.8.7)

1. Added experimental sqlite_bind() and sqlite_reset() APIs.
2. If the name of the database is an empty string, open a new database
    in a temporary file that is automatically deleted when the database
    is closed.
3. Performance enhancements in the Lemon-generated parser
4. Experimental date/time functions revised.
5. Disallow temporary indices on permanent tables.
6. Documentation updates and typo fixes
7. Added experimental sqlite_progress_handler() callback API
8. Removed support for the Oracle8 outer join syntax.
9. Allow GLOB and LIKE operators to work as functions.
10. Other minor documentation and makefile changes and bug fixes.

### 2003-08-22 (2.8.6)

1. Moved the CVS repository to www.sqlite.org
2. Update the NULL-handling documentation.
3. Experimental date/time functions added.
4. Bug fix: correctly evaluate a view of a view without segfaulting.
5. Bug fix: prevent database corruption if you dropped a
    trigger that had the same name as a table.
6. Bug fix: allow a VACUUM (without segfaulting) on an empty
    database after setting the EMPTY_RESULT_CALLBACKS pragma.
7. Bug fix: if an integer value will not fit in a 32-bit int, store it in
    a double instead.
8. Bug fix: Make sure the journal file directory entry is committed to disk
    before writing the database file.

### 2003-07-22 (2.8.5)

1. Make LIMIT work on a compound SELECT statement.
2. LIMIT 0 now shows no rows.  Use LIMIT -1 to see all rows.
3. Correctly handle comparisons between an INTEGER PRIMARY KEY and
    a floating point number.
4. Fix several important bugs in the new ATTACH and DETACH commands.
5. Updated the NULL-handling document.
6. Allow NULL arguments in sqlite_compile() and sqlite_step().
7. Many minor bug fixes

### 2003-06-29 (2.8.4)

1. Enhanced the "PRAGMA integrity_check" command to verify indices.
2. Added authorization hooks for the new ATTACH and DETACH commands.
3. Many documentation updates
4. Many minor bug fixes

### 2003-06-04 (2.8.3)

1. Fix a problem that will corrupt the indices on a table if you
    do an INSERT OR REPLACE or an UPDATE OR REPLACE on a table that
    contains an INTEGER PRIMARY KEY plus one or more indices.
2. Fix a bug in Windows locking code so that locks work correctly
    when simultaneously accessed by Win95 and WinNT systems.
3. Add the ability for INSERT and UPDATE statements to refer to the
    "rowid" (or "_rowid_" or "oid") columns.
4. Other important bug fixes

### 2003-05-17 (2.8.2)

1. Fix a problem that will corrupt the database file if you drop a
    table from the main database that has a TEMP index.

### 2003-05-17 (2.8.1)

1. Reactivated the VACUUM command that reclaims unused disk space in
    a database file.
2. Added the ATTACH and DETACH commands to allow interacting with multiple
    database files at the same time.
3. Added support for TEMP triggers and indices.
4. Added support for in-memory databases.
5. Removed the experimental sqlite_open_aux_file().  Its function is
    subsumed in the new ATTACH command.
6. The precedence order for ON CONFLICT clauses was changed so that
    ON CONFLICT clauses on BEGIN statements have a higher precedence than
    ON CONFLICT clauses on constraints.
7. Many, many bug fixes and compatibility enhancements.

### 2003-02-16 (2.8.0)

1. Modified the journal file format to make it more resistant to corruption
    that can occur after an OS crash or power failure.
2. Added a new C/C++ API that does not use callback for returning data.

### 2003-01-25 (2.7.6)

1. Performance improvements.  The library is now much faster.
2. Added the **sqlite_set_authorizer()** API.  Formal documentation has
    not been written - see the source code comments for instructions on
    how to use this function.
3. Fix a bug in the GLOB operator that was preventing it from working
    with upper-case letters.
4. Various minor bug fixes.

### 2002-12-28 (2.7.5)

1. Fix an uninitialized variable in pager.c which could (with a probability
    of about 1 in 4 billion) result in a corrupted database.

### 2002-12-17 (2.7.4)

1. Database files can now grow to be up to 2^41 bytes.  The old limit
    was 2^31 bytes.
2. The optimizer will now scan tables in the reverse if doing so will
    satisfy an ORDER BY ... DESC clause.
3. The full pathname of the database file is now remembered even if
    a relative path is passed into sqlite_open().  This allows
    the library to continue operating correctly after a chdir().
4. Speed improvements in the VDBE.
5. Lots of little bug fixes.

### 2002-10-31 (2.7.3)

1. Various compiler compatibility fixes.
2. Fix a bug in the "expr IN ()" operator.
3. Accept column names in parentheses.
4. Fix a problem with string memory management in the VDBE
5. Fix a bug in the "table_info" pragma"
6. Export the sqlite_function_type() API function in the Windows DLL
7. Fix locking behavior under Windows
8. Fix a bug in LEFT OUTER JOIN

### 2002-09-25 (2.7.2)

1. Prevent journal file overflows on huge transactions.
2. Fix a memory leak that occurred when sqlite_open() failed.
3. Honor the ORDER BY and LIMIT clause of a SELECT even if the
    result set is used for an INSERT.
4. Do not put write locks on the file used to hold TEMP tables.
5. Added documentation on SELECT DISTINCT and on how SQLite handles NULLs.
6. Fix a problem that was causing poor performance when many thousands
    of SQL statements were executed by a single sqlite_exec() call.

### 2002-08-31 (2.7.1)

1. Fix a bug in the ORDER BY logic that was introduced in version 2.7.0
2. C-style comments are now accepted by the tokenizer.
3. INSERT runs a little faster when the source is a SELECT statement.

### 2002-08-25 (2.7.0)

1. Make a distinction between numeric and text values when sorting.
    Text values sort according to memcmp().  Numeric values sort in
    numeric order.
2. Allow multiple simultaneous readers under Windows by simulating
    the reader/writers locks that are missing from Win95/98/ME.
3. An error is now returned when trying to start a transaction if
    another transaction is already active.

### 2002-08-13 (2.6.3)

1. Add the ability to read both little-endian and big-endian databases.
    So a database created under SunOS or Mac OS X can be read and written
    under Linux or Windows and vice versa.
2. Convert to the new website: https://sqlite.org/
3. Allow transactions to span Linux Threads
4. Bug fix in the processing of the ORDER BY clause for GROUP BY queries

### 2002-07-31 (2.6.2)

1. Text files read by the COPY command can now have line terminators
    of LF,  CRLF, or CR.
2. SQLITE_BUSY is handled correctly if encountered during database
    initialization.
3. Fix to UPDATE triggers on TEMP tables.
4. Documentation updates.

### 2002-07-19 (2.6.1)

1. Include a static string in the library that responds to the RCS
    "ident" command and which contains the library version number.
2. Fix an assertion failure that occurred when deleting all rows of
    a table with the "count_changes" pragma turned on.
3. Better error reporting when problems occur during the automatic
    2.5.6 to 2.6.0 database format upgrade.

### 2002-07-18 (2.6.0)

1. Change the format of indices to correct a design flaw the originated
    with version 2.1.0.  *** This is an incompatible
    file format change ***  When version 2.6.0 or later of the
    library attempts to open a database file created by version 2.5.6 or
    earlier, it will automatically and irreversibly convert the file format.
    **Make backup copies of older database files before opening them with
    version 2.6.0 of the library.**

### 2002-07-07 (2.5.6)

1. Fix more problems with rollback.  Enhance the test suite to exercise
    the rollback logic extensively in order to prevent any future problems.
    

### 2002-07-06 (2.5.5)

1. Fix a bug which could cause database corruption during a rollback.
    This bugs was introduced in version 2.4.0 by the freelist
    optimization of checkin [410].
2. Fix a bug in aggregate functions for VIEWs.
3. Other minor changes and enhancements.

### 2002-07-01 (2.5.4)

1. Make the "AS" keyword optional again.
2. The datatype of columns now appear in the 4th argument to the
    callback.
3. Added the **sqlite_open_aux_file()** API, though it is still
    mostly undocumented and untested.
4. Added additional test cases and fixed a few bugs that those
    test cases found.

### 2002-06-25 (2.5.3)

1. Bug fix:  Database corruption can occur due to the optimization
    that was introduced in version 2.4.0 (check-in [410]).  The problem
    should now be fixed.  The use of versions 2.4.0 through 2.5.2 is
    not recommended.

### 2002-06-25 (2.5.2)

1. Added the new **SQLITE_TEMP_MASTER** table which records the schema
    for temporary tables in the same way that**SQLITE_MASTER** does for
    persistent tables.
2. Added an optimization to UNION ALL
3. Fixed a bug in the processing of LEFT OUTER JOIN
4. The LIMIT clause now works on subselects
5. ORDER BY works on subselects
6. There is a new TypeOf() function used to determine if an expression
    is numeric or text.
7. Autoincrement now works for INSERT from a SELECT.

### 2002-06-19 (2.5.1)

1. The query optimizer now attempts to implement the ORDER BY clause
    using an index.  Sorting is still used if not suitable index is
    available.

### 2002-06-17 (2.5.0)

1. Added support for row triggers.
2. Added SQL-92 compliant handling of NULLs.
3. Add support for the full SQL-92 join syntax and LEFT OUTER JOINs.
4. Double-quoted strings interpreted as column names not text literals.
5. Parse (but do not implement) foreign keys.
6. Performance improvements in the parser, pager, and WHERE clause code
    generator.
7. Make the LIMIT clause work on subqueries.  (ORDER BY still does not
    work, though.)
8. Added the "%Q" expansion to sqlite_*_printf().
9. Bug fixes too numerous to mention (see the change log).

### 2002-05-10 (2.4.12)

1. Added logic to detect when the library API routines are called out
    of sequence.

### 2002-05-08 (2.4.11)

1. Bug fix: Column names in the result set were not being generated
    correctly for some (rather complex) VIEWs.  This could cause a
    segfault under certain circumstances.

### 2002-05-03 (2.4.10)

1. Bug fix: Generate correct column headers when a compound SELECT is used
    as a subquery.
2. Added the sqlite_encode_binary() and sqlite_decode_binary() functions to
    the source tree.  But they are not yet linked into the library.
3. Documentation updates.
4. Export the sqlite_changes() function from Windows DLLs.
5. Bug fix: Do not attempt the subquery flattening optimization on queries
    that lack a FROM clause.  To do so causes a segfault.

### 2002-04-22 (2.4.9)

1. Fix a bug that was causing the precompiled binary of SQLITE.EXE to
    report "out of memory" under Windows 98.

### 2002-04-20 (2.4.8)

1. Make sure VIEWs are created after their corresponding TABLEs in the
    output of the **.dump** command in the shell.
2. Speed improvements: Do not do synchronous updates on TEMP tables.
3. Many improvements and enhancements to the shell.
4. Make the GLOB and LIKE operators functions that can be overridden
    by a programmer.  This allows, for example, the LIKE operator to
    be changed to be case sensitive.

### 2002-04-12 (2.4.7)

1. Add the ability to put TABLE.* in the column list of a
    SELECT statement.
2. Permit SELECT statements without a FROM clause.
3. Added the **last_insert_rowid()** SQL function.
4. Do not count rows where the IGNORE conflict resolution occurs in
    the row count.
5. Make sure functions expressions in the VALUES clause of an INSERT
    are correct.
6. Added the **sqlite_changes()** API function to return the number
    of row that changed in the most recent operation.

### 2002-04-02 (2.4.6)

1. Bug fix: Correctly handle terms in the WHERE clause of a join that
    do not contain a comparison operator.

### 2002-04-02 (2.4.5)

1. Bug fix: Correctly handle functions that appear in the WHERE clause
    of a join.
2. When the PRAGMA vdbe_trace=ON is set, correctly print the P3 operand
    value when it is a pointer to a structure rather than a pointer to
    a string.
3. When inserting an explicit NULL into an INTEGER PRIMARY KEY, convert
    the NULL value into a unique key automatically.

### 2002-03-30 (2.4.4)

1. Allow "VIEW" to be a column name
2. Added support for CASE expressions (patch from Dan Kennedy)
3. Added RPMS to the delivery (patches from Doug Henry)
4. Fix typos in the documentation
5. Cut over configuration management to a new CVS repository with
    its own CVSTrac bug tracking system.

### 2002-03-23 (2.4.3)

1. Fix a bug in SELECT that occurs when a compound SELECT is used as a
    subquery in the FROM of a SELECT.
2. The **sqlite_get_table()** function now returns an error if you
    give it two or more SELECTs that return different numbers of columns.

### 2002-03-20 (2.4.2)

1. Bug fix: Fix an assertion failure that occurred when ROWID was a column
    in a SELECT statement on a view.
2. Bug fix: Fix an uninitialized variable in the VDBE that would could an
    assert failure.
3. Make the os.h header file more robust in detecting when the compile is
    for Windows and when it is for Unix.

### 2002-03-13 (2.4.1)

1. Using an unnamed subquery in a FROM clause would cause a segfault.
2. The parser now insists on seeing a semicolon or the end of input before
    executing a statement.  This avoids an accidental disaster if the
    WHERE keyword is misspelled in an UPDATE or DELETE statement.

### 2002-03-11 (2.4.0)

1. Change the name of the sanity_check PRAGMA to **integrity_check** and make it available in all compiles.
2. SELECT min() or max() of an indexed column with no WHERE or GROUP BY
    clause is handled as a special case which avoids a complete table scan.
3. Automatically generated ROWIDs are now sequential.
4. Do not allow dot-commands of the command-line shell to occur in the
    middle of a real SQL command.
5. Modifications to the Lemon parser generator so that the parser tables
    are 4 times smaller.
6. Added support for user-defined functions implemented in C.
7. Added support for new functions: **coalesce()** ,**lower()** ,**upper()** , and**random()**
8. Added support for VIEWs.
9. Added the subquery flattening optimizer.
10. Modified the B-Tree and Pager modules so that disk pages that do not
    contain real data (free pages) are not journaled and are not
    written from memory back to the disk when they change.  This does not
    impact database integrity, since the
    pages contain no real data, but it does make large INSERT operations
    about 2.5 times faster and large DELETEs about 5 times faster.
11. Made the CACHE_SIZE pragma persistent
12. Added the SYNCHRONOUS pragma
13. Fixed a bug that was causing updates to fail inside of transactions when
    the database contained a temporary table.

### 2002-02-19 (2.3.3)

1. Allow identifiers to be quoted in square brackets, for compatibility
    with MS-Access.
2. Added support for sub-queries in the FROM clause of a SELECT.
3. More efficient implementation of sqliteFileExists() under Windows.
    (by Joel Luscy)
4. The VALUES clause of an INSERT can now contain expressions, including
    scalar SELECT clauses.
5. Added support for CREATE TABLE AS SELECT
6. Bug fix: Creating and dropping a table all within a single
    transaction was not working.

### 2002-02-14 (2.3.2)

1. Bug fix: There was an incorrect assert() in pager.c.  The real code was
    all correct (as far as is known) so everything should work OK if you
    compile with -DNDEBUG=1.  When asserts are not disabled, there
    could be a fault.

### 2002-02-13 (2.3.1)

1. Bug fix: An assertion was failing if "PRAGMA full_column_names=ON;" was
    set and you did a query that used a rowid, like this:
    "SELECT rowid, * FROM ...".

### 2002-02-03 (2.3.0)

1. Fix a serious bug in the INSERT command which was causing data to go
    into the wrong columns if the data source was a SELECT and the INSERT
    clauses specified its columns in some order other than the default.
2. Added the ability to resolve constraint conflicts is ways other than
    an abort and rollback.  See the documentation on the "ON CONFLICT"
    clause for details.
3. Temporary files are now automatically deleted by the operating system
    when closed.  There are no more dangling temporary files on a program
    crash.  (If the OS crashes, fsck will delete the file after reboot
    under Unix.  I do not know what happens under Windows.)
4. NOT NULL constraints are honored.
5. The COPY command puts NULLs in columns whose data is '\N'.
6. In the COPY command, backslash can now be used to escape a newline.
7. Added the SANITY_CHECK pragma.

### 2002-01-28 (2.2.5)

1. Important bug fix: the IN operator was not working if either the
    left-hand or right-hand side was derived from an INTEGER PRIMARY KEY.
2. Do not escape the backslash '\' character in the output of the
    **sqlite** command-line access program.

### 2002-01-22 (2.2.4)

1. The label to the right of an AS in the column list of a SELECT can now
    be used as part of an expression in the WHERE, ORDER BY, GROUP BY, and/or
    HAVING clauses.
2. Fix a bug in the **-separator** command-line option to the**sqlite** command.
3. Fix a problem with the sort order when comparing upper-case strings against
    characters greater than 'Z' but less than 'a'.
4. Report an error if an ORDER BY or GROUP BY expression is constant.

### 2002-01-16 (2.2.3)

1. Fix warning messages in VC++ 7.0.  (Patches from nicolas352001)
2. Make the library thread-safe.  (The code is there and appears to work
    but has not been stressed.)
3. Added the new **sqlite_last_insert_rowid()** API function.

### 2002-01-14 (2.2.2)

1. Bug fix: An assertion was failing when a temporary table with an index
    had the same name as a permanent table created by a separate process.
2. Bug fix: Updates to tables containing an INTEGER PRIMARY KEY and an
    index could fail.

### 2002-01-09 (2.2.1)

1. Bug fix: An attempt to delete a single row of a table with a WHERE
    clause of "ROWID=x" when no such rowid exists was causing an error.
2. Bug fix: Passing in a NULL as the 3rd parameter to **sqlite_open()** would sometimes cause a coredump.
3. Bug fix: DROP TABLE followed by a CREATE TABLE with the same name all
    within a single transaction was causing a coredump.
4. Makefile updates from A. Rottmann

### 2001-12-22 (2.2.0)

1. Columns of type INTEGER PRIMARY KEY are actually used as the primary
    key in underlying B-Tree representation of the table.
2. Several obscure, unrelated bugs were found and fixed while
    implemented the integer primary key change of the previous bullet.
3. Added the ability to specify "*" as part of a larger column list in
    the result section of a SELECT statement.  For example:
    "**SELECT rowid, * FROM table1;** ".
4. Updates to comments and documentation.

### 2001-12-15 (2.1.7)

1. Fix a bug in **CREATE TEMPORARY TABLE** which was causing the
    table to be initially allocated in the main database file instead
    of in the separate temporary file.  This bug could cause the library
    to suffer an assertion failure and it could cause "page leaks" in the
    main database file.
2. Fix a bug in the b-tree subsystem that could sometimes cause the first
    row of a table to be repeated during a database scan.

### 2001-12-14 (2.1.6)

1. Fix the locking mechanism yet again to prevent
    **sqlite_exec()** from returning SQLITE_PROTOCOL
    unnecessarily.  This time the bug was a race condition in
    the locking code.  This change affects both POSIX and Windows users.

### 2001-12-06 (2.1.5)

1. Fix for another problem (unrelated to the one fixed in 2.1.4)
    that sometimes causes **sqlite_exec()** to return SQLITE_PROTOCOL
    unnecessarily.  This time the bug was
    in the POSIX locking code and should not effect Windows users.

### 2001-12-05 (2.1.4)

1. Sometimes **sqlite_exec()** would return SQLITE_PROTOCOL when it
    should have returned SQLITE_BUSY.
2. The fix to the previous bug uncovered a deadlock which was also
    fixed.
3. Add the ability to put a single .command in the second argument
    of the sqlite shell
4. Updates to the FAQ

### 2001-11-24 (2.1.3)

1. Fix the behavior of comparison operators
    (ex: "**<** ", "**==** ", etc.)
    so that they are consistent with the order of entries in an index.
2. Correct handling of integers in SQL expressions that are larger than
    what can be represented by the machine integer.

### 2001-11-23 (2.1.2)

1. Changes to support 64-bit architectures.
2. Fix a bug in the locking protocol.
3. Fix a bug that could (rarely) cause the database to become
    unreadable after a DROP TABLE due to corruption to the SQLITE_MASTER
    table.
4. Change the code so that version 2.1.1 databases that were rendered
    unreadable by the above bug can be read by this version of
    the library even though the SQLITE_MASTER table is (slightly)
    corrupted.

### 2001-11-13 (2.1.1)

1. Bug fix: Sometimes arbitrary strings were passed to the callback
    function when the actual value of a column was NULL.

### 2001-11-12 (2.1.0)

1. Change the format of data records so that records up to 16MB in size
    can be stored.
2. Change the format of indices to allow for better query optimization.
3. Implement the "LIMIT ... OFFSET ..." clause on SELECT statements.

### 2001-11-03 (2.0.8)

1. Made selected parameters in API functions **const** . This should
    be fully backwards compatible.
2. Documentation updates
3. Simplify the design of the VDBE by restricting the number of sorters
    and lists to 1.
    In practice, no more than one sorter and one list was ever used anyhow.
    

### 2001-10-22 (2.0.7)

1. Any UTF-8 character or ISO8859 character can be used as part of
    an identifier.
2. Patches from Christian Werner to improve ODBC compatibility and to
    fix a bug in the round() function.
3. Plug some memory leaks that use to occur if malloc() failed.
    We have been and continue to be memory leak free as long as
    malloc() works.
4. Changes to some test scripts so that they work on Windows in
    addition to Unix.

### 2001-10-19 (2.0.6)

1. Added the EMPTY_RESULT_CALLBACKS pragma
2. Support for UTF-8 and ISO8859 characters in column and table names.
3. Bug fix: Compute correct table names with the FULL_COLUMN_NAMES pragma
    is turned on.

### 2001-10-15 (2.0.5)

1. Added the COUNT_CHANGES pragma.
2. Changes to the FULL_COLUMN_NAMES pragma to help out the ODBC driver.
3. Bug fix: "SELECT count(*)" was returning NULL for empty tables.
    Now it returns 0.

### 2001-10-13 (2.0.4)

1. Bug fix: an obscure and relatively harmless bug was causing one of
    the tests to fail when gcc optimizations are turned on.  This release
    fixes the problem.

### 2001-10-13 (2.0.3)

1. Bug fix: the **sqlite_busy_timeout()** function was delaying 1000
    times too long before failing.
2. Bug fix: an assertion was failing if the disk holding the database
    file became full or stopped accepting writes for some other reason.
    New tests were added to detect similar problems in the future.
3. Added new operators: **&** (bitwise-and)**|** (bitwise-or),**~** (ones-complement),**<<** (shift left),**>>** (shift right).
4. Added new functions: **round()** and**abs()** .

### 2001-10-09 (2.0.2)

1. Fix two bugs in the locking protocol.  (One was masking the other.)
2. Removed some unused "#include " that were causing problems
    for VC++.
3. Fixed **sqlite.h** so that it is usable from C++
4. Added the FULL_COLUMN_NAMES pragma.  When set to "ON", the names of
    columns are reported back as TABLE.COLUMN instead of just COLUMN.
5. Added the TABLE_INFO() and INDEX_INFO() pragmas to help support the
    ODBC interface.
6. Added support for TEMPORARY tables and indices.

### 2001-10-02 (2.0.1)

1. Remove some C++ style comments from btree.c so that it will compile
    using compilers other than gcc.
2. The ".dump" output from the shell does not work if there are embedded
    newlines anywhere in the data.  This is an old bug that was carried
    forward from version 1.0.  To fix it, the ".dump" output no longer
    uses the COPY command.  It instead generates INSERT statements.
3. Extend the expression syntax to support "expr NOT NULL" (with a
    space between the "NOT" and the "NULL") in addition to "expr NOTNULL"
    (with no space).

### 2001-09-28 (2.0.0)

1. Automatically build binaries for Linux and Windows and put them on
    the website.

### 2001-09-28 (2.0-alpha-4)

1. Incorporate makefile patches form A. Rottmann to use LIBTOOL

### 2001-09-27 (2.0-alpha-3)

1. SQLite now honors the UNIQUE keyword in CREATE UNIQUE INDEX.  Primary
    keys are required to be unique.
2. File format changed back to what it was for alpha-1
3. Fixes to the rollback and locking behavior

### 2001-09-20 (2.0-alpha-2)

1. Initial release of version 2.0.  The idea of renaming the library
    to "SQLus" was abandoned in favor of keeping the "SQLite" name and
    bumping the major version number.
2. The pager and btree subsystems added back. They are now the only
    available backend.
3. The Dbbe abstraction and the GDBM and memory drivers were removed.
4. Copyright on all code was disclaimed.  The library is now in the
    public domain.

### 2001-07-23 (1.0.32)

1. Pager and btree subsystems removed.  These will be used in a follow-on
    SQL server library named "SQLus".
2. Add the ability to use quoted strings as table and column names in
    expressions.

### 2001-04-15 (1.0.31)

1. Pager subsystem added but not yet used.
2. More robust handling of out-of-memory errors.
3. New tests added to the test suite.

### 2001-04-06 (1.0.30)

1. Remove the **sqlite_encoding** TCL variable that was introduced
    in the previous version.
2. Add options **-encoding** and**-tcl-uses-utf** to the**sqlite** TCL command.
3. Add tests to make sure that tclsqlite was compiled using Tcl header
    files and libraries that match.

### 2001-04-05 (1.0.29)

1. The library now assumes data is stored as UTF-8 if the --enable-utf8
    option is given to configure.  The default behavior is to assume
    iso8859-x, as it has always done.  This only makes a difference for
    LIKE and GLOB operators and the LENGTH and SUBSTR functions.
2. If the library is not configured for UTF-8 and the Tcl library
    is one of the newer ones that uses UTF-8 internally,
    then a conversion from UTF-8 to iso8859 and
    back again is done inside the TCL interface.

### 2001-04-04 (1.0.28)

1. Added limited support for transactions.  At this point, transactions
    will do table locking on the GDBM backend.  There is no support (yet)
    for rollback or atomic commit.
2. Added special column names ROWID, OID, and _ROWID_ that refer to the
    unique random integer key associated with every row of every table.
3. Additional tests added to the regression suite to cover the new ROWID
    feature and the TCL interface bugs mentioned below.
4. Changes to the Lemon parser generator to help it work better when
    compiled using MSVC.
5. Bug fixes in the TCL interface identified by Oleg Oleinick.

### 2001-03-20 (1.0.27)

1. When doing DELETE and UPDATE, the library used to write the record
    numbers of records to be deleted or updated into a temporary file.
    This is changed so that the record numbers are held in memory.
2. The DELETE command without a WHILE clause just removes the database
    files from the disk, rather than going through and deleting record
    by record.

### 2001-03-20 (1.0.26)

1. A serious bug fixed on Windows.  Windows users should upgrade.
    No impact to Unix.

### 2001-03-15 (1.0.25)

1. Modify the test scripts to identify tests that depend on system
    load and processor speed and
    to warn the user that a failure of one of those (rare) tests does
    not necessarily mean the library is malfunctioning.  No changes to
    code.
    

### 2001-03-14 (1.0.24)

1. Fix a bug which was causing
    the UPDATE command to fail on systems where "malloc(0)" returns
    NULL.  The problem does not appear on Windows, Linux, or HPUX but does
    cause the library to fail on QNX.
    

### 2001-02-20 (1.0.23)

1. An unrelated (and minor) bug from Mark Muranwski fixed.  The algorithm
    for figuring out where to put temporary files for a "memory:" database
    was not working quite right.
    

### 2001-02-19 (1.0.22)

1. The previous fix was not quite right.  This one seems to work better.
    

### 2001-02-19 (1.0.21)

1. The UPDATE statement was not working when the WHERE clause contained
    some terms that could be satisfied using indices and other terms that
    could not.  Fixed.

### 2001-02-11 (1.0.20)

1. Merge development changes into the main trunk.  Future work toward
    using a BTree file structure will use a separate CVS source tree.  This
    CVS tree will continue to support the GDBM version of SQLite only.

### 2001-02-06 (1.0.19)

1. Fix a strange (but valid) C declaration that was causing problems
    for QNX.  No logical changes.

### 2001-01-04 (1.0.18)

1. Print the offending SQL statement when an error occurs.
2. Do not require commas between constraints in CREATE TABLE statements.
3. Added the "-echo" option to the shell.
4. Changes to comments.

### 2000-12-10 (1.0.17)

1. Rewrote **sqlite_complete()** to make it faster.
2. Minor tweaks to other code to make it run a little faster.
3. Added new tests for **sqlite_complete()** and for memory leaks.

### 2000-11-28 (1.0.16)

1. Documentation updates.  Mostly fixing of typos and spelling errors.

### 2000-10-23 (1.0.15)

1. Documentation updates
2. Some sanity checking code was removed from the inner loop of vdbe.c
    to help the library to run a little faster.  The code is only
    removed if you compile with -DNDEBUG.

### 2000-10-19 (1.0.14)

1. Added a "memory:" backend driver that stores its database in an
    in-memory hash table.

### 2000-10-19 (1.0.13)

1. Break out the GDBM driver into a separate file in anticipation
    to added new drivers.
2. Allow the name of a database to be prefixed by the driver type.
    For now, the only driver type is "gdbm:".

### 2000-10-17 (1.0.12)

1. Fixed an off-by-one error that was causing a coredump in
    the '%q' format directive of the new
    **sqlite_..._printf()** routines.
2. Added the **sqlite_interrupt()** interface.
3. In the shell, **sqlite_interrupt()** is invoked when the
    user presses Control-C
4. Fixed some instances where **sqlite_exec()** was
    returning the wrong error code.

### 2000-10-11 (1.0.10)

1. Added notes on how to compile for Windows95/98.
2. Removed a few variables that were not being used.  Etc.

### 2000-10-09 (1.0.9)

1. Added the **sqlite_..._printf()** interface routines.
2. Modified the **sqlite** shell program to use the new interface
    routines.
3. Modified the **sqlite** shell program to print the schema for
    the built-in SQLITE_MASTER table, if explicitly requested.

### 2000-09-30 (1.0.8)

1. Begin writing documentation on the TCL interface.

### 2000-09-29 (Not Released)

1. Added the **sqlite_get_table()** API
2. Updated the documentation for due to the above change.
3. Modified the **sqlite** shell to make use of the new
    sqlite_get_table() API in order to print a list of tables
    in multiple columns, similar to the way "ls" prints filenames.
4. Modified the **sqlite** shell to print a semicolon at the
    end of each CREATE statement in the output of the ".schema" command.

### 2000-09-21 (Not Released)

1. Change the tclsqlite "eval" method to return a list of results if
    no callback script is specified.
2. Change tclsqlite.c to use the Tcl_Obj interface
3. Add tclsqlite.c to the libsqlite.a library

### 2000-09-14 (1.0.5)

1. Changed the print format for floating point values from "%g" to "%.15g".
    
2. Changed the comparison function so that numbers in exponential notation
    (ex: 1.234e+05) sort in numerical order.

### 2000-08-28 (1.0.4)

1. Added functions **length()** and**substr()** .
2. Fix a bug in the **sqlite** shell program that was causing
    a coredump when the output mode was "column" and the first row
    of data contained a NULL.

### 2000-08-22 (1.0.3)

1. In the sqlite shell, print the "Database opened READ ONLY" message
    to stderr instead of stdout.
2. In the sqlite shell, now print the version number on initial startup.
3. Add the **sqlite_version[]** string constant to the library
4. Makefile updates
5. Bug fix: incorrect VDBE code was being generated for the following
    circumstance: a query on an indexed table containing a WHERE clause with
    an IN operator that had a subquery on its right-hand side.

### 2000-08-18 (1.0.1)

1. Fix a bug in the configure script.
2. Minor revisions to the website.

### 2000-08-17 (1.0)

1. Change the **sqlite** program so that it can read
    databases for which it lacks write permission.  (It used to
    refuse all access if it could not write.)

### 2000-08-09

1. Treat carriage returns as white space.

### 2000-08-08

1. Added pattern matching to the ".table" command in the "sqlite"
command shell.

### 2000-08-04

1. Documentation updates
2. Added "busy" and "timeout" methods to the Tcl interface

### 2000-08-03

1. File format version number was being stored in sqlite_master.tcl
    multiple times. This was harmless, but unnecessary. It is now fixed.

### 2000-08-02

1. The file format for indices was changed slightly in order to work
    around an inefficiency that can sometimes come up with GDBM when
    there are large indices having many entries with the same key.
    ** Incompatible Change **

### 2000-08-01

1. The parser's stack was overflowing on a very long UPDATE statement.
    This is now fixed.

### 2000-07-31

1. Finish the VDBE tutorial.
2. Added documentation on compiling to WinNT.
3. Fix a configuration program for WinNT.
4. Fix a configuration problem for HPUX.

### 2000-07-29

1. Better labels on column names of the result.

### 2000-07-28

1. Added the **sqlite_busy_handler()** and**sqlite_busy_timeout()** interface.

### 2000-06-23

1. Begin writing the VDBE tutorial.

### 2000-06-21

1. Clean up comments and variable names.  Changes to documentation.
    No functional changes to the code.

### 2000-06-19

1. Column names in UPDATE statements were case sensitive.
    This mistake has now been fixed.

### 2000-06-18

1. Added the concatenate string operator (||)

### 2000-06-12

1. Added the fcnt() function to the SQL interpreter.  The fcnt() function
    returns the number of database "Fetch" operations that have occurred.
    This function is designed for use in test scripts to verify that
    queries are efficient and appropriately optimized.  Fcnt() has no other
    useful purpose, as far as I know.
2. Added a bunch more tests that take advantage of the new fcnt() function.
    The new tests did not uncover any new problems.

### 2000-06-08

1. Added lots of new test cases
2. Fix a few bugs discovered while adding test cases
3. Begin adding lots of new documentation

### 2000-06-06

1. Added compound select operators: **UNION** ,**UNION ALL** ,**INTERSECT** , and**EXCEPT**
2. Added support for using **(SELECT ...)** within expressions
3. Added support for **IN** and**BETWEEN** operators
4. Added support for **GROUP BY** and**HAVING**
5. NULL values are now reported to the callback as a NULL pointer
    rather than an empty string.

### 2000-06-03

1. Added support for default values on columns of a table.
2. Improved test coverage.  Fixed a few obscure bugs found by the
improved tests.

### 2000-06-02
