---
id: collect-261121-veille-ia/actu-business/release-history-1
title: "Release History"
domain: actu-business
role: reference
task: article
actors: []
dates: []
keywords: ["sqlite", "option", "release", "command", "history", "prompt", "default", "timeline"]
source: https://sqlite.org/draft/changes.html
source_anchor: ""
source_lines: [1, 1500]
section: "Actu_Business"
---
*** DRAFT ***

 
# Release History

This page provides a high-level summary of changes to SQLite.
For more detail, see the Fossil checkin logs at
https://sqlite.org/src/timeline and
https://sqlite.org/src/timeline?t=release.
See the chronology a succinct listing of releases.

### 2027-04-09 (3.55.0)

1.  New SQLITE_LIMIT_CTES limit, helps to harden the system from DoS
     attacks when given hostile SQL inputs.
2.  CLI enhancements:
       1.  New SQL function "capable(X)" allows a script to determine
          compile time options and adjust processing accordingly.
     
  2.  New dot-commands ".if", ".elseif", ".else", and ".endif".
     
3.  It is now an error to set the FTS5 
     secure-delete option on a 
     contentless-delete FTS5 table. Prior to this it was not an error, but
     setting the option had no effect.
4.  Various performance optimizations

### 2026-10-16 (3.54.1)

1.  Fix a possible heap-buffer overflow on 32-bit builds.
     (64-bit builds are not affected by this problem.)
2.  Fixes other problems in 3.54.0 reported by users.  See the
     check-in timeline
     for details.
**Hashes:**
3. SQLITE_SOURCE_ID: 2026-10-16 14:45:20 555e07ef33f69a0957d24d8e555b702464a131d6f1607f4c3c1e4551e8e6ca76
4. SHA3-256 for sqlite3.c: e2b869d3003b33c942b7dd7063604fbc53553e1b66609570df39b9b7b558ee81

### 2026-10-09 (3.54.0)

1.  CLI enhancements:
       1.  CLI prompt strings may contain escape sequences that
          expand to show state information.  The initial CLI
          prompt is now be initialized the values of the
          SQLITE_PS1 and SQLITE_PS2 environment variables, if
          those variables exist.
     
  2.  The default CLI prompt has been enhanced to take
          advantage of the new escape sequences described in
          the previous.
     
  3.  For columnar output modes, the CLI now shows the column name header
          by default even if the query returns no rows.
     
  4.  Use the new diskused() extension to implement the
          ".diskused" dot-command.  The replaces the separate sqlite3_analyzer
          utility program, which is now deprecated.
     
  5.  On unix, the CLI preserves zero bytes when outputing
          unformatted BLOB values.
     
  6.  Enhance the .mode command with new options
          "--rowcount on"
          and "--titles always".
          The "--titles always" is now the default behavior
          (see item 3a above).  Legacy behavior can be restored
          using "-titles on".
     
  7.  Enhance the .mode command with the new --ifmt option and
          --fpfmt option that allow the rendering of integer and floating-point
          values to be more precisely controlled using a printf-style format string.
     
  8.  The -csv command-line option automatically disables
          display limits, for legacy compatibility.
     
  9.  The default CLI prompt honors the NO_COLOR environment variable.
     
  10.  The ability to end an SQL command using the line that contains just
          "go" or "/" (for compatibility with SQL Server and Oracle, respectively)
          is discontinued, unless the CLI is compiled with
          option -DSQLITE_SHELL_LEGACY_COMMAND_TERMINATOR.
     
2.  Query planner improvements:
       1.  When doing an UPDATE on a table that has expression indexes,
          do not change the expression index if the value being indexed
          is unchanged.
     
  2.  Improved optimization of "*expr* OR TRUE" and
          "*expr* OR FALSE".
  3.  Ignore DISTINCT in the SELECT statement on the right-hand
          side of an IN operator.
     
  4.  UNION queries with a LIMIT of exactly 1 are now handled faster
          and with less memory.
     
3.  The ALTER TABLE command raises an error on any attempt to add, drop, or rename columns
     with the special names "ROWID", "_ROWID_", or "OID".
4.  Add the diskused() SQL function as an optional extension.
5.  Added the %J and %j conversions used to render JSON string
     literals to the built-in printf() and the format() SQL function.
6.  Add the sqlite3_result_str() interface and use it internally to
     simplify the code.
7.  Add the sqlite3_incomplete() interface and use it as part of the newly
     enhanced CLI prompt string management.
8.  Enhancements to date/time functions:
       1.  Added the "weekday -N" form of the weekday modifier.
     
  2.  Added the end of modifier.
     
9.  Use O_TMPFILE when generating temporary files, on systems that
     support that feature.
10.  Drop support for WindowsXP and earlier Windows versions,
     including WindowsCE. SQLite for Windows now requires
     Windows Vista (circa 2007) or later.
       1.  Improve performance by using
          Slim Reader/Writer Locks
          (SRWLs) to implement non-recursive mutexes, instead of
          CriticalSection,
          since all supported Windows platforms now provide SRWLs.
     
  2.  Use the -DSQLITE_UWP=1 compile-time option for UWP and WebUI applications.
     
  3.  To be clear: This changes above only affects SQLite on Windows.
          Builds for Linux, Mac, BSD, and other unix variants are unaffected.
     
11.  The authorizer callback is now invoked for SQL functions found in
     the DEFAULT clauses of CREATE TABLE statements.  This happens both when
     the CREATE TABLE statement is first issued and when the DEFAULT value is
     actually used, but not when reading the schema while opening a database.
12.  Added SQLITE_LIMIT_SCHEMA and SQLITE_LIMIT_TRIGGER_STEPS to the
     sqlite3_limit() interface.
13.  About 4% faster, as measured by counting CPU cycles in the
     speedtest1
     preformance benchmark using valgrind on Linux x64.  About 7.5%
     faster compared to one year ago (version 3.51.0).  Your mileage
     may vary depending on workload, build options, CPU, compiler,
     and operating system.
**Hashes:**
14. SQLITE_SOURCE_ID: 2026-10-09 15:46:58 be8d059e9a49089ab2dce5ed26dd87aaf598fdc5fbe0b107c0dd758464bcd5e3
15. SHA3-256 for sqlite3.c: 8efd453d08a7cfc4c79de5923153036cdc4dcaa254d4f1e617be6a2a98828f9e

### 2026-07-24 (3.53.4)

1.  Fixes for problems in 3.53.0 (and 3.53.1, 3.53.2, and 3.53.3) mostly coming from AIs.  See the
     check-in timeline
     for details.
**Hashes:**
2. SQLITE_SOURCE_ID: 2026-07-24 19:02:57 bf7c7f30031888f4e796e429ab3978879485813aaca6f641c7b33e4e09459bcc
3. SHA3-256 for sqlite3.c: 67f423e9ebbbdc473cbc4772c872ee6b89f31fde4ed0279a5c25d5f65c043a16

### 2026-06-26 (3.53.3)

1.  Fixes for problems in 3.53.0 (and 3.53.1 and 3.53.2) mostly coming from AIs.  See the
     check-in timeline
     for details.
**Hashes:**
2. SQLITE_SOURCE_ID: 2026-06-26 20:14:12 d4c0e51e4aeb96955b99185ab9cde75c339e2c29c3f3f12428d364a10d782c62
3. SHA3-256 for sqlite3.c: 28e484abdaa43630e34040ef6ed92be973a1ad54107803d8af5145b889c23ed7

### 2026-06-03 (3.53.2)

1.  Fixes for problems in 3.53.0 reported by users.  See the
     check-in timeline
     for details.
**Hashes:**
2. SQLITE_SOURCE_ID: 2026-06-03 19:12:13 d6e03d8c777cfa2d35e3b60d8ec3e0187f3e9f99d8e2ee9cac695fd6fcdf1a24
3. SHA3-256 for sqlite3.c: 44fd61b9f93b4155105cb2d80c957ae6c64a8b5bd6ed51a4992f0dbd438e4e11

### 2026-05-05 (3.53.1)

1.  Fixes for problems in 3.53.1 reported by users.  See the
     check-in timeline
     for details.
**Hashes:**
2. SQLITE_SOURCE_ID: 2026-05-05 10:34:17 c88b22011a54b4f6fbd149e9f8e4de77658ce58143a1af0e3785e4e6475127e9
3. SHA3-256 for sqlite3.c: 414432ae5719f6cdc485f3927e12c7ad107e2b8c6b434e5df2eadb5312bfabb5

### 2026-04-09 (3.53.0)

1.  Fix the WAL-reset database corruption bug.
2.  Add the Query Result Formatter (QRF)
     library for formatting the results of SQL queries for human readability
     on a fixed-pitch font screen.
       1.  Add the format method to the TCL Interface so that QRF is
          accessible from TCL.
     
  2.  QRF is used for result formatting in the CLI, resulting in improved
          display capabilities.
     
3.  New SQL language features:
       1.  Enhance ALTER TABLE to permit adding and removing NOT NULL
          and CHECK constraints.
     
  2.  The REINDEX EXPRESSIONS statement rebuilds expression indexes.
          (Useful to repair stale expression indexes.)
     
  3.  The body of TEMP triggers may now modify and/or query tables in
          the main schema.
     
  4.  Enhance VACUUM INTO so that if a URI filename is used as the
          target and that filename has a reserve=N query parameter with N
          between 0 and 255, then the reserve amount for
          the generated database copy is set to N.
     
4.  New SQL functions:
       1.  json_array_insert()
     
  2.  jsonb_array_insert()
     
5.  Renovations to the CLI:
       1.  Major enhancements to the .mode command.
     
  2.  Improved result formatting, due to the addition of the 
          QRF extension.
          For example, numeric values are now right-justified by default
          in tabular output modes.
     
  3.  The default output mode for interactive CLI sessions now uses
          QRF to display query results in boxes formed using Unicode
          box-drawing characters, for improved legibility.  Batch CLI
          sessions use the legacy output format for compatibility.
     
  4.  Bare (unquoted) semicolons at the end of dot-commands are
          silently ignored.  ← Potential incompatibility!
     
  5.  Fix the .testcase and .check commands so that they actually
          work, and use those commands in scripts that are part of
          the standard SQLite test suite included with the source tree.
     
  6.  Command-line arguments that match *.sql or *.txt and are
          the names of non-empty files are read and interpreted as scripts
          of SQL statements and/or dot-commands.
     
  7.  The argument to the ".timer" command can now be "once", to run
          the timer on only the next SQL statement.
     
  8.  The new "--timeout S" option to the ".progress" dot-command
          causes SQL statements to interrupt after S seconds.
     
  9.  The ".indexes" command was changed so that the PATTERN argument
          matches the name of the index, not the name of the table
          being indexed (thus making the PATTERN argument actually useful).
          And, several new options were added to ".indexes".
     
6.  New C-language interfaces:
       1.  sqlite3_str_truncate()
     
  2.  sqlite3_str_free()
     
  3.  sqlite3_carray_bind_v2()
     
  4.  Add the SQLITE_PREPARE_FROM_DDL option to sqlite3_prepare_v3() which
          permits virtual table implementations to safely prepare SQL statements
          that are derived from the database schema.
     
  5.  Added the SQLITE_UTF8_ZT constant which can be used as the encoding parameter to
          sqlite3_result_text64() or sqlite3_bind_text64() to indicate that the value
          is UTF-8 encoded and zero terminated.
     
  6.  The SQLITE_LIMIT_PARSER_DEPTH option is added to sqlite3_limit().
     
  7.  The SQLITE_DBCONFIG_FP_DIGITS option is added to sqlite3_db_config().
          See also item 9b below.
     
7.  Query planner improvements:
       1.  Always use a sort-and-merge algorithm for EXCEPT, INTERSECT, and
          UNION, since this is almost always faster than using a hash table.
     
  2.  Improvements to join order selection in large multi-way joins on
          a star schema.
     
  3.  Enhance the EXISTS-to-JOIN optimization so that the inserted JOIN
          terms are not required to be on the inner-most loops, as long as
          all dependencies for the EXISTS-to-JOIN loops are in outer loops.
     
  4.  Enhance the omit-noop-join optimization so that it is able to omit
          a chain of joins that do not affect the output.
     
  5.  Allow queries that use "GROUP BY e1 ORDER BY e2" where e1 and e2
          are identical apart from ASC/DESC sort-orders to be optimized using
          a single index. 
     
  6.  Allow virtual tables to optimize DISTINCT in cases where the
          result-set of a query does not exactly match the ORDER BY clause.
     
8.  Add new interfaces to the session extension
     that enable an application to add changes one at
     a time to the sqlite3_changegroup object:
       1.  sqlite3changegroup_change_begin()
     
  2.  sqlite3changegroup_change_blob()
     
  3.  sqlite3changegroup_change_double()
     
  4.  sqlite3changegroup_change_int64()
     
  5.  sqlite3changegroup_change_null()
     
  6.  sqlite3changegroup_change_text()
     
  7.  sqlite3changegroup_change_finish()
     
  8.  sqlite3changegroup_config()
     
9.  Improvements to floating-point ↔ text conversions.
       1.  Reimplemented to improve performance.
     
  2.  Rounding is now done by default to 17 significant digits,
          instead of 15, as was the case for all prior versions.
          The sqlite3_db_config(SQLITE_DBCONFIG_FP_DIGITS) API (item 6g above)
          can change this, if desired.
     
10.  Added the self-healing index feature to deal with the
     stale expression index problem.
11.  Add the "-p|--port" option to sqlite3_rsync.
12.  Discontinue support for Windows RT.
13.  JavaScript/WASM
       1.  Add the "opfs-wl" VFS, functionally identical to the "opfs" VFS but
          using Web Locks for locking, which can promise fairer lock sharing
          than the "opfs" bespoke protocol can. "opfs-wl" requires
          `Atomics.waitAsync()` , so requires newer browsers than "opfs"
          does.
**Hashes:**

14. SQLITE_SOURCE_ID: 2026-04-09 11:41:38 4525003a53a7fc63ca75c59b22c79608659ca12f0131f52c18637f829977f20b
15. SHA3-256 for sqlite3.c: bb317fbbd2b3bc53233ddd5894bf4d2dc6f533445f350d4235dbcc86f65af4ec

### 2026-03-13 (3.51.3)

1.  Fix the WAL-reset database corruption bug.
2.  Other minor bug fixes.
3. SQLITE_SOURCE_ID: 2026-03-13 10:38:09 737ae4a34738ffa0c3ff7f9bb18df914dd1cad163f28fd6b6e114a344fe6d618
4. SHA3-256 for sqlite3.c: 32d5424f97e0a7fc5ed2f6335afbb58be4e0298bd7117a34e39d345ff13d859e

### 2026-03-06 (3.52.0)

*Withdrawn*
*All of the features that were originally scheduled for the 3.52.0
release have been moved forward into version 3.53.0.*


### 2026-01-09 (3.51.2)

1.  Fix an obscure deadlock in the new broken-posix-lock detection logic
     in item 17 above.
2.  Fix multiple problems in the EXISTS-to-JOIN optimization that was added
     as part of optimization item 6b above.
3.  Other minor bug fixes.
4. SQLITE_SOURCE_ID: 2026-01-09 17:27:48 b270f8339eb13b504d0b2ba154ebca966b7dde08e40c3ed7d559749818cb2075
5. SHA3-256 for sqlite3.c: 733b3fcc6cccb1e334424b9b91a9d68b618385b76ebfcbb106690bd3a9e61367

### 2025-11-28 (3.51.1)

1.  Fix incorrect results from nested EXISTS queries caused by the
     optimization in item 6b in the 3.51.0 release.
2.  Fix a latent bug in fts5vocab virtual table, exposed by
     new optimizations in the 3.51.0 release
**Hashes:**
3. SQLITE_SOURCE_ID: 2025-11-28 17:28:25 281fc0e9afc38674b9b0991943b9e9d1e64c6cbdb133d35f6f5c87ff6af38a88
4. SHA3-256 for sqlite3.c: 7cec3a104797bea93970408168197af37f178ab608ea55efd48d28daa87a8ce3

### 2025-11-04 (3.51.0)

1.  New macros in `sqlite3.h` :  1.  `SQLITE_SCM_BRANCH` → the name of the branch from which the source code is taken.
  2.  `SQLITE_SCM_TAGS` → space-separated list of tags on the source code check-in.
  3.  `SQLITE_SCM_DATETIME` → ISO-8601 date and time of the source code check-in.
2. Two new JSON functions, jsonb_each() and jsonb_tree() work the same as the existing
    json_each() and json_tree() functions except that they return JSONB for the "value" column
    when the "type" is 'array' or 'object'.
3. The carray and percentile extensions are now built
    into the amalgamation, though they are disabled by default and must be activated
    at compile-time using the
    -DSQLITE_ENABLE_CARRAY and/or -DSQLITE_ENABLE_PERCENTILE options, respectively.
4. Enhancements to TCL Interface:
      1.  Add the `-asdict` flag to the`eval` command
         to have it set the row data as a dict instead of an array.
  2.  User-defined functions may now `break` to return an
         SQL NULL.
5. CLI enhancements:
      1.  Increase the precision of ".timer" to microseconds.
    
  2.  Enhance the "box" and "column" formatting modes to deal
         with double-wide characters.
    
  3.  The ".imposter" command provides read-only imposter tables
         that work with VACUUM and do not require the --unsafe-testing
         option.
    
  4.  Add the --ifexists option to the CLI command-line option and to
         the .open command.
    
  5.  Limit columns widths set by the ".width" command to 30,000 or less,
         as there is not good reason to have wider columns, but supporting
         wider columns provides opportunity to malefactors.
    
6. Performance enhancements:
      1.  Use fewer CPU cycles to commit a read transaction.
    
  2.  Early detection of joins that return no rows due to one or more
         of the tables containing no rows.
    
  3.  Avoid evaluation of scalar subqueries if the result of the subquery
         does not change the result of the overall expression.
    
  4.  Faster window function queries when using 
         "`BETWEEN :x FOLLOWING AND :y FOLLOWING` " with a large :y.
7. Add the PRAGMA wal_checkpoint=NOOP; command and the
    SQLITE_CHECKPOINT_NOOP argument for sqlite3_wal_checkpoint_v2().
8. Add the sqlite3_set_errmsg() API for use by extensions.
9. Add the sqlite3_db_status64() API, which works just like the existing
    sqlite3_db_status() API except that it returns 64-bit results.
10. Add the SQLITE_DBSTATUS_TEMPBUF_SPILL option to the sqlite3_db_status()
    and sqlite3_db_status64() interfaces.
11. In the session extension add the sqlite3changeset_apply_v3() interface.
12. For the built-in printf() and the format() SQL function,
    omit the leading '-' from negative floating point numbers if the
    '+' flag is omitted and the "#" flag is present and all displayed digits are '0'.
    Use '%#f' or similar to avoid outputs like '-0.00' and instead show just '0.00'.
13. Improved error messages generated by FTS5.
14. Enforce STRICT typing on computed columns.
15. Improved support for VxWorks
16.  JavaScript/WASM now supports 64-bit WASM. The canonical builds continue to be
     32-bit but creating one's own 64-bit build is now as simple
     as running "make".
17. Improved resistance to database corruption caused by an application
    breaking Posix advisory locks using close().
**Hashes:**
18. SQLITE_SOURCE_ID: 2025-11-04 19:38:17 fb2c931ae597f8d00a37574ff67aeed3eced4e5547f9120744ae4bfa8e74527b
19. SHA3-256 for sqlite3.c: e2add951748f73587cadd1b2684defb4f39fa58dca14b16162d4237e50af9afa

### 2025-07-30 (3.50.4)

1.  Fix two long-standings cases of the use of uninitialized variables
     in obscure circumstances.
**Hashes:**
2. SQLITE_SOURCE_ID: 2025-07-30 19:33:53 4d8adfb30e03f9cf27f800a2c1ba3c48fb4ca1b08b0f5ed59a4d5ecbf45e20a3
3. SHA3-256 for sqlite3.c: 9145255e83da6529e70121ee4d7a4c88fe83ca4511da0c9ed13d10842df36782

### 2025-07-17 (3.50.3)

1.  Fix a possible memory error that can occur if a query is made against
     against FTS5 index that has been deliberately corrupted in a very specific
     way.
2.  Fix the parser so that it ignored SQL comments in all places of a
     CREATE TRIGGER statement.  This resolves a problem that was introduced
     by the introduction of the SQLITE_DBCONFIG_ENABLE_COMMENTS feature
     in version 3.49.0.
3.  Fix an incorrect answer due to over-optimization of an AND operator.
     Forum post f4878de3e.
4.  Fix minor makefile issues and documentation typos.
**Hashes:**
5. SQLITE_SOURCE_ID: 2025-07-17 13:25:10 3ce993b8657d6d9deda380a93cdd6404a8c8ba1b185b2bc423703e41ae5f2543
6. SHA3-256 for sqlite3.c: 934fafe96caa7f4c16e82e0c2b674441a715c038acc9780bf15e09411daba70c

### 2025-06-28 (3.50.2)

1.  Fix the concat_ws() SQL function so that it includes empty strings
     in the concatenation.
     Forum post 52503ac21d.
2.  Fix the file-io extension (used by the CLI) so that it can be built
     using the MinGW compiler chain.
3.  Avoid writing frames with no checksums into the wal file if a savepoint
     is rolled back after dirty pages have already been spilled into the
     wal file.
     Forum post b490f726db.
4.  Fix the Bitvec object to avoid stack overflow when the database is within
     60 pages of its maximum size.
5.  Fix a problem with UPDATEs on fts5 tables that contain BLOB values.
6.  Fix an issue with transitive IS constraints on a RIGHT JOIN.
7.  Raise an error early if the number of aggregate terms in a query
     exceeds the maximum number of columns, to avoid downstream assertion
     faults.
8.  Ensure that sqlite3_setlk_timeout() holds the database mutex.
9.  Fix typos in API documentation.
**Hashes:**
10. SQLITE_SOURCE_ID: 2025-06-28 14:00:48 2af157d77fb1304a74176eaee7fbc7c7e932d946bf25325e9c26c91db19e3079
11. SHA3-256 for sqlite3.c: 889bf23942c52f38a6f182e71d3f0a03db490fd731f02147b1142c11153f3b3a

### 2025-06-06 (3.50.1)

1.  Fix a long-standing bug in jsonb_set() and similar that was exposed by
     new optimizations added in version 3.50.0.
2.  Fix an apparently harmless ASAN warning that can occur on builds that
     use -DSQLITE_DEFAULT_MEMSTATUS=0.
3.  Fix an off-by-one bug in sqlite3_rsync that can result in the last
     page not being transferred for the replicate database.
4.  Query planner optimization: Allow the right-hand side of a LEFT JOIN
     to be flattened even if it is a virtual table.
5.  Fix sqlite3_setlk_timeout() to use a blocking lock when opening a
     snapshot transaction and when blocked by another process running recovery.
6.  Other minor fixes that were reported after the 3.50.0 release.
**Hashes:**
7. SQLITE_SOURCE_ID: 2025-06-06 14:52:32 b77dc5e0f596d2140d9ac682b2893ff65d3a4140aa86067a3efebe29dc914c95
8. SHA3-256 for sqlite3.c: c3bdffa01dab94be370bdcaeb819cda4a6e0c61f0c6e19a6f94ccba76fc9eeca

### 2025-05-29 (3.50.0)

1. Add the sqlite3_setlk_timeout() interface which sets a separate timeout,
    distinct from the sqlite3_busy_timeout(), for blocking locks on builds
    that support blocking locks.
2. The SQLITE_DBCONFIG_ENABLE_COMMENTS constraint (added in the
    previous release) is relaxed slightly so that comments are
    always allowed when reading the schema out of a pre-existing sqlite_schema
    table.  Comments are only blocked in new SQL.
3. New SQL functions:
      1.  unistr()
    
  2.  unistr_quote()
    
4. For the %Q and %q conversions in the built-in printf() (which covers
    the sqlite3_mprintf() API and the format() SQL function and similar)
    the alternate-form-1 flag ("#") causes control characters
    to be converted into backslash-escapes suitable for unistr().
5. CLI enhancements:
      1.  Avoids direct output of most control characters.
    
  2.  The output of the .dump command makes use of the new unistr()
         SQL function to encode special characters, unless the --escape mode
         is set to off.
    
  3.  Better formatting of complex partial indexes in the output
         from the ".schema --indent" command.
    
6. Enhancements to sqlite3_rsync:
      1.  The requirement that the database be in WAL mode has been removed.
    
  2.  The sync protocol is enhanced to use less network bandwidth when both
         sides start out being very similar to one another.
    
  3.  The sqlite3_rsync program now works on Macs without having to specify
         the full pathname of the sqlite3_rsync executable on the remote side
         as long as you install the sqlite3_rsync executable in
         one of these directories:  $HOME/bin:/usr/local/bin:/opt/homebrew/bin
    
7. Changes to JSON functions:
      1.  Bug fix: Enforce the JSON5 restriction that the
         "\0" escape must not be followed by a digit.
    
  2.  Bug fix: When the LABEL argument to json_group_object(LABEL,VALUE)
         is NULL, that element of the resulting object is omitted.
    
  3.  Optimization:
         If the jsonb_set() or jsonb_replace() functions make a change in the
         interior of a large JSONB object, they strive to keep the size of the
         JSONB object unchanged and to modify as few bytes as possible on the interior
         of the object.  This helps reduce I/O as it allows SQLite to write only
         the page that contains the changed bytes and not all the surrounding pages.
    
8. Improved support for building on Cygwin and MinGW and similar, as well
    as Termux.
9. Typo fixes in the documentation and in the source code comments.
10. Miscellaneous performance improvements.
11.  JavaScript/WASM:
       1.  Fix a long-standing filename digest calculation bug in the OPFS
          SAHPool VFS. Databases created in that VFS by 3.50.0+ cannot
          be read by older versions of the VFS, but 3.50.0 can
          backwards-compatibly work with existing databases created
          by older versions.
 **Hashes:**
12. SQLITE_SOURCE_ID: 2025-05-29 14:26:00 dfc790f998f450d9c35e3ba1c8c89c17466cb559f87b0239e4aab9d34e28f742
13. SHA3-256 for sqlite3.c: b8a2bcdd2ec60f87c3bbe7a2c25eb65e476e40fe80cd066ab8aa98e5d516c1e4

### 2025-05-07 (3.49.2)

1.  Fix a bug in the NOT NULL optimization of version 3.40.0
     (item 3c in the version 3.40.0 change log) that can lead to
     a memory error if abused.
2.  Fix the count-of-view optimization so that it does not give an incorrect
     answer for a DISTINCT query.
3.  Fix a possible incorrect answer that can result if a UNIQUE constraint
     of a table contains the PRIMARY KEY column and that UNIQUE constraint is
     used by an IN operator.
4.  Fix obscure problems with the generate_series() extension function.
5.  Incremental improvements to the configure/make.
**Hashes:**
6. SQLITE_SOURCE_ID: 2025-05-07 10:39:52 17144570b0d96ae63cd6f3edca39e27ebd74925252bbaf6723bcb2f6b4861fb1
7. SHA3-256 for sqlite3.c: 17f4857fc6a0def2749d248d5365c59282c044270533c6c2b21287295f01eb23

### 2025-02-18 (3.49.1)

1.  Improve portability of makefiles and configure scripts.
2.  Fix a bug in the concat_ws() function, introduced in version 3.44.0,
     that could lead to a memory error, specifically a write past the end of
     allocated space, if the separator string is larger than two megabytes.
3.  Enhanced the SQLITE_DBCONFIG_LOOKASIDE interface to make it more
     robust against misuse.
**Hashes:**
4. SQLITE_SOURCE_ID: 2025-02-18 13:38:58 873d4e274b4988d260ba8354a9718324a1c26187a4ab4c1cc0227c03d0f10e70
5. SHA3-256 for sqlite3.c: 22eab05c842463a3705c63922f12f68531200185ced510ed6aed2065ca44e816

### 2025-02-06 (3.49.0)

1.  Enhancements to the query planner:
       1.  Improve the query-time index optimization so that it works on
          WITHOUT ROWID tables.
     
  2.  Better query plans for large star-query joins.  This fixes
          three different performance regressions that were reported on
          the SQLite Forum.
     
  3.  When two or more queries have the same estimated cost, use
          the one with the fewer bytes per row.
     
2.  Enhance the iif() SQL function so that it can accept any number of
     arguments greater than or equal to two.
3.  Enhance the session extension so that it works on databases that
     make use of generated columns.
4.  Omit the SQLITE_USE_STDIO_FOR_CONSOLE compile-time option which was
     not implemented correctly and never worked right.  In its place add
     the SQLITE_USE_W32_FOR_CONSOLE_IO compile-time option.  This option
     applies to command-line tools like the CLI only, not to the SQLite
     core.  It causes Win32 APIs to be used for console I/O instead of
     stdio.  This option affects Windows builds only.
5.  Three new options to sqlite3_db_config().  All default to "on".
       1.  SQLITE_DBCONFIG_ENABLE_ATTACH_CREATE
     
  2.  SQLITE_DBCONFIG_ENABLE_ATTACH_WRITE
     
  3.  SQLITE_DBCONFIG_ENABLE_COMMENTS
     
6.  Replace Autotools
     with Autosetup for the
     configure script used in the precompiled amalgamation tarball.
     The configure script for the canonical source code was changed
     to Autosetup in the previous (3.48.0) release.
     Only the main SQLite configure script in the amalgamation tarball
     is changed. The (deprecated) configuration script use by
     TEA subdirectory
     of the amalgamation tarball still relies on Autotools.
7.  Various minor patches and fixes for problems seen in the 3.48.0 release.
**Hashes:**
8. SQLITE_SOURCE_ID: 2025-02-06 11:55:18 4a7dd425dc2a0e5082a9049c9b4a9d4f199a71583d014c24b4cfe276c5a77cde
9. SHA3-256 for sqlite3.c: 030c764eba64a3fa3bbf87447abd175bfef94851f970d62126a92877b30b611e

### 2025-01-14 (3.48.0)

1.  Refactor the "configure" script used to help build SQLite from canonical sources,
     to fix bugs, improve performance, and make the code more maintainable.
       -  This does not affect the "configure" script in the sqlite3-autoconf-NNNNNNN.tar.gz
          "amalgamation tarball", only the canonical sources.  The build system for
          the amalgamation tarball is unchanged.  If you are using the amalgamation tarball,
          nothing about this change log entry applies to you.
     
  -  The key innovation here is that
          Autosetup is now used instead of
          GNU Autoconf.  That seems like a big change,
          but it is really just an implementation detail.  The ./configure
          script is coded very differently, but should work the same as before.
     
  -  One advantage of the new configure is that you no longer need
          to install TCL in order to build most
          SQLite targets.  TCL is still required to run tests or to build accessory
          programs (like sqlite3_analyzer) that use TCL, but it is not required for
          most common targets.  Hence, as of this release, the only build dependencies
          are a C compiler and "make" or "nmake".
     
2.  Improved EXPLAIN QUERY PLAN output for covering indexes.
3.  Allow a two-argument version of the iif() SQL function.  Also allow
     if() as an alternative spelling for iif().
4.  Add the ".dbtotxt" command to the CLI.
5.  Add the SQLITE_IOCAP_SUBPAGE_READ property to the xDeviceCharacteristics
     method of the sqlite3_io_methods object.
6.  Add the SQLITE_PREPARE_DONT_LOG option to sqlite3_prepare_v3() that
     prevents warning messages being sent to the error log if the SQL is
     ill-formed.  This allows sqlite3_prepare_v3() to be used to do test compiles
     of SQL to check for validity without polluting the error log with false
     messages.
7.  Increase the minimum allowed value of SQLITE_LIMIT_LENGTH from 1 to 30.
8.  Added the SQLITE_FCNTL_NULL_IO file control.
9.  Extend the FTS5 auxiliary API xInstToken() to work with prefix queries
     via the insttoken configuration option and the
     fts5_insttoken() SQL function.
10.  Increase the maximum number of arguments to an SQL function
     from 127 to 1000.
11.  Remove vestigial traces of SQLITE_USER_AUTHENTICATION.
12.  Various obscure bug fixes.
**Hashes:**
13. SQLITE_SOURCE_ID: 2025-01-14 11:05:00 d2fe6b05f38d9d7cd78c5d252e99ac59f1aea071d669830c1ffe4e8966e84010
14. SHA3-256 for sqlite3.c: 522bd9492526147d12fc8521028eb960557353d8f08c2035f15ac737d4b91e21

### 2024-12-07 (3.47.2)

1.  Fix a problem in text-to-floating-point conversion for SQLite that can cause values
     between '1.8446744073709550592eNNN' and '1.8446744073709551609eNNN' for any exponent NNN
     to be rendered incorrectly.  In other words, some numeric text values where the first 16
     significant digits are '1844674407370955' might be converted into the wrong floating-point
     value.  See forum thread 569a7209179a7f5e.
     This problem only arises on x64 and i386 hardware.  The problem was introduced in 3.47.0.
2.  Other minor bug fixes.
**Hashes:**
3. SQLITE_SOURCE_ID: 2024-12-07 20:39:59 2aabe05e2e8cae4847a802ee2daddc1d7413d8fc560254d93ee3e72c14685b6c
4. SHA3-256 for sqlite3.c: 586b5789453ee5c51e8d858cd836f20ce60df882323f2070547cbaf23a5898af

### 2024-11-25 (3.47.1)

1.  Fix the makefiles so that they once again honored DESTDIR for the "install" target.
2.  Add the SQLITE_IOCAP_SUBPAGE_READ capability to the VFS, to work around issues
     on some non-standard VFSes caused by making SQLITE_DIRECT_OVERFLOW_READ the default
     in version 3.45.0.
3.  Fix problems with line endings in the new sqlite3_rsync.exe utility on Windows.
4.  Fix incorrect answers to certain obscure IN queries caused by new query optimizations
     added in the 3.47.0 release.
5.  Other minor bug fixes.
**Hashes:**
6. SQLITE_SOURCE_ID: 2024-11-25 12:07:48 b95d11e958643b969c47a8e5857f3793b9e69700b8f1469371386369a26e577e
7. SHA3-256 for sqlite3.c: d91b8b5671a127bd41057bc758d6d59fb7aceda67934a1c0166ab2e133f2f902

### 2024-10-21 (3.47.0)

1.  Allow arbitrary expressions in the second argument to the RAISE function.
2.  If the RHS of the ->> operator is negative, then access array elements counting from the right.
3.  Fix a problem with rolling back hot journal files in the seldom-used
     unix-dotfile VFS.
4.  FTS5 tables can now be dropped even if they use a non-standard tokenizer that
     has not been registered.
5.  Fix the group_concat() aggregate function so that it returns an empty string,
     not a NULL, if it receives a single input value which is an empty string.
6.  Enhance the generate_series() table-valued function so that it is able to
     recognize and use constraints on its output value.
7.  Preupdate hooks now recognize when a column added by ALTER TABLE ADD COLUMN
     has a non-null default value.
8.  Performance optimizations:
       1.  Improved reuse of subqueries associated with the IN operator, especially when
          the IN operator has been duplicated due to predicate push-down.
     
  2.  Use a Bloom filter on subqueries on the right-hand side of the
          IN operator, in cases where that seems likely to improve performance.
     
  3.  Ensure that queries like "`SELECT func(a) FROM tab GROUP BY 1` " only
          invoke the func() function once per row.
  4.  No attempt is made to create automatic indexes on a column that is known
          to be non-selective because of its use in other indexes that have been analyzed.
     
  5.  Adjustments to the query planner so that it produces better plans for
          star queries with a large number of dimension tables.
     
  6.  Add the "order-by-subquery" optimization, that seeks to disable sort operations
          in outer queries if the desired order is obtained naturally due to ORDER BY
          clauses in subqueries.
     
  7.  The "indexed-subtype-expr" optimization strives to use expressions that
          are part of an index rather than recomputing the expression based on table values,
          as long as the query planner can prove that the subtype
          of the expression will never be used.
     
  8.  Miscellaneous coding tweaks for faster runtimes.
     
9.  Enhancements to SQLite-related command-line programs:
       1.  Add the experimental sqlite3_rsync program.
     
  2.  Add extension functions median(), percentile(), percentile_cont(),
          and percentile_disc() to the CLI.
     
  3.  Add the .www dot-command to the CLI.
     
  4.  The sqlite3_analyzer utility now provides a break-out of statistics
          for WITHOUT ROWID tables.
     
  5.  The sqldiff utility avoids creating an empty database if its second argument
          does not exist.
     
10.  Enhance the sqlite_dbpage table-valued function such that INSERT can be used
     to increase or decrease the size of the database file.
11.  SQLite no longer makes any use of the "long double" data type, as hardware support
     for long double is becoming less common and long double creates challenges for
     some compiler tool chains.  Instead, SQLite uses
     Dekker's algorithm
     when extended precision is needed.
12.  The TCL Interface for SQLite supports TCL9.  Everything probably still works for
     TCL 8.5 and later, though this is not guaranteed.  Users are encouraged to upgrade
     to TCL9.
13.  JavaScript/WASM:
       1.  Fix a corruption-causing bug in the JavaScript "opfs" VFS.
     
  2.  Correct "mode=ro" handling for the "opfs" VFS.
     
  3.  Work around a couple of browser-specific OPFS quirks.
     
14.  FTS5 Changes:
       1.  Add the fts5_tokenizer_v2 API and the locale=1 option, for creating custom
          locale-aware tokenizers and fts5 tables that may take advantage of them.
     
  2.  Add the contentless_unindexed=1 option, for creating contentless fts5 tables
          that store the values of any UNINDEXED columns persistently in the database.
     
  3.  Allow an FTS5 table to be dropped even if it uses a custom tokenizer whose
          implementation is not available.
     
 **Hashes:**
15. SQLITE_SOURCE_ID: 2024-10-21 16:30:22 03a9703e27c44437c39363d0baf82db4ebc94538a0f28411c85dda156f82636e
16. SHA3-256 for sqlite3.c: bcec3a4fbc97e973547924677332996ef64e06a6d10d22e2c2344f447536cc29

### 2024-08-13 (3.46.1)

1.  Improved robustness while parsing the tokenize= arguments in FTS5.
     Forum post 171bcc2bcd.
2.  Enhancements to covering index prediction in the query planner. 
     Add early detection of over-prediction of covering indexes so that
     sqlite3_prepare() will return an error rather than just generate bad bytecode. 
     Forum post e60e4c295d22f8ce.
3.  Do not let the number of terms on a VALUES clause be limited by
     SQLITE_LIMIT_COMPOUND_SELECT, even if the VALUES clause contains
     elements that appear to be variables due to 
     double-quoted string literals.
4.  Fix the window function version of group_concat() so that it returns
     an empty string if it has one or more empty string inputs.
5.  In FTS5 secure-delete mode, fix false-positive
     integrity-check reports about corrupt indexes.
6.  Syntax errors in ALTER TABLE should always return SQLITE_ERROR.  In
     some cases, they were formerly returning SQLITE_INTERNAL.
7.  JavaScript/WASM:
       1.  Fix a corruption-causing bug in the JavaScript "opfs" VFS.
     
  2.  Work around a couple of browser-specific OPFS quirks.
     
8.  Other minor fixes.
**Hashes:**
9. SQLITE_SOURCE_ID: 2024-08-13 09:16:08 c9c2ab54ba1f5f46360f1b4f35d849cd3f080e6fc2b6c60e91b16c63f69a1e33
10. SHA3-256 for sqlite3.c: 186a1baa476b6d546de155160ca6d30ff7b7e6ee375f0bb6445e1a3d180a7dad

### 2024-05-23 (3.46.0)

1.  Enhance PRAGMA optimize in multiple ways, to make it
     simpler to use:
       1.  PRAGMA optimize automatically implements a temporary
          analysis limit to prevent excess runtime
          on large databases.
     
  2.  Added the new 0x10000 bitmask option to check for updates on all tables.
     
  3.  Automatically re-analyze tables that do not have sqlite_stat1 entries.
     
2.  Enhancements to the date and time functions:
       1.  The strftime() SQL function now supports %G, %g, %U, and %V.
     
  2.  New modifiers 'ceiling' and 'floor' control the algorithm used to
          resolve ambiguous dates when shifting a date by an integer number
          of months and/or years.
     
  3.  The 'utc' and 'localtime' modifiers are now no-ops if SQLite knows
          that the time is already in UTC or in the localtime, respectively.
     
3.  Add support for underscore ("_") characters between digits in
     numeric literals.
4.  Add the json_pretty() SQL function.
5.  Query planner improvements:
       1.  The "VALUES-as-coroutine" optimization enables INSERT statements with 
          thousands of rows in the VALUES clause to parse and run in about half
          the time and using about half as much memory.
     
  2.  Allow the use of an index for queries like "SELECT count(DISTINCT col) FROM ...",
          even if the index records are not smaller than the table records.
     
  3.  Improved recognition of cases where the value of an SQL function is
          constant because all its arguments are constant.
     
  4.  Enhance the WHERE-clause push-down optimization so that it is able to
          push down WHERE clause terms containing uncorrelated subqueries.
     
6.  Allocate additional memory from the heap for the SQL parser stack if
     that stack overflows, rather than reporting a "parser stack overflow" error.
7.  JSON changes:
       1.  Allow ASCII control characters within JSON5 string literals.
     
  2.  Fix the -> and ->> operators so that when the right-hand side operand is a string
          that looks like an integer it is still treated as a string, because that is what
          PostgreSQL does.
     
8.  Allow large hexadecimal literals to be used as the DEFAULT value to a table column.
**Hashes:**
9. SQLITE_SOURCE_ID: 2024-05-23 13:25:27 96c92aba00c8375bc32fafcdf12429c58bd8aabfcadab6683e35bbb9cdebf19e
10. SHA3-256 for sqlite3.c: 094429ea827fcd32275e767134bc6c7b9ea394a2c5a9e653dd0a0690b2c11358

### 2024-04-15 (3.45.3)

1.  Fix a long-standing bug (going back to version 3.24.0)
     that might (rarely) cause the "old.*" values of an UPDATE trigger
     to be incorrect if that trigger fires in response to an UPSERT.
     Forum post 284955a3cd454a15.
2.  Fix a bug in sum() that could cause it to return NULL when it should return
     Infinity.  Forum post 23b8688ef4.
3.  Other trifling corrections and compiler warning fixes that have come up
     since the previous patch release.  See the
     timeline
     for details.
**Hashes:**
4. SQLITE_SOURCE_ID: 2024-04-15 13:34:05 8653b758870e6ef0c98d46b3ace27849054af85da891eb121e9aaa537f1e8355
5. SHA3-256 for sqlite3.c: 21dbe688a71b449d28e2a8ec6a43e7520e54df456e02b6d4f6a1d1c7a998c826

### 2024-03-12 (3.45.2)

1.  Fix an error in UPSERT, introduced by enhancement 3a in version 3.35.0
     (2021-03-12), that could cause an index to get out-of-sync with its table.
     Forum thread 919c6579c8.
2.  Reduce the scope of the NOT NULL strength reduction optimization that was
     added as item 8e in version 3.35.0 (2021-03-12).  The optimization
     was being attempted in some contexts where it did not work, resulting in
     incorrect query results.
     Forum thread 440f2a2f17.
3.  Other trifling corrections and compiler warning fixes that have come up
     since the previous patch release.  See the
     timeline
     for details.
**Hashes:**
4. SQLITE_SOURCE_ID: "2024-03-12 11:06:23 d8cd6d49b46a395b13955387d05e9e1a2a47e54fb99f3c9b59835bbefad6af77"
5. SHA3-256 for sqlite3.c: bd76ad2dc9cde151e469e86627a7e8753aa8ef1a6f657c5a80ba48324b53226b

### 2024-01-30 (3.45.1)

1.  Restore the JSON BLOB input bug, and promise to support the anomaly in
     subsequent releases, for backward compatibility.
2.  Fix the PRAGMA integrity_check command so that it works on read-only
     databases that contain FTS3 and FTS5 tables. This resolves an issue
     introduced in version 3.44.0 but was undiscovered until after the 3.45.0 release.
3.  Fix issues associated with processing corrupt JSONB inputs:
       1.  Prevent exponential runtime when converting a corrupt JSONB into text.
     
  2.  Fix a possible read of one byte past the end of the JSONB blob when converting
          a corrupt JSONB into text.
     
  3.  Enhanced testing using jfuzz to prevent any future JSONB problems such
          as the above.
     
4.  Fix a long-standing bug in which a read of a few bytes past the end of a
     memory-mapped segment might occur when accessing a craftily corrupted database
     using memory-mapped database.
5.  Fix a long-standing bug in which a NULL pointer dereference might occur in
     the bytecode engine due to incorrect bytecode being generated for a class
     of SQL statements that are deliberately designed to stress the query planner
     but which are otherwise pointless.
**Hashes:**
6. SQLITE_SOURCE_ID: 2024-01-30 16:01:20 e876e51a0ed5c5b3126f52e532044363a014bc594cfefa87ffb5b82257cc467a
7. SHA3-256 for sqlite3.c: 0474604df9e1b69a5544295dd046aad954749279780d557da80f44b958100295

### 2024-01-15 (3.45.0)

1.  Added the SQLITE_RESULT_SUBTYPE property for
     application-defined SQL functions.  
     All application defined SQL functions that invokes
     sqlite3_result_subtype() must be registered with this new property.
     Failure to do so might cause the call to sqlite3_result_subtype() to
     behave as a no-op.  Compile with -DSQLITE_STRICT_SUBTYPE=1 to cause an
     SQL error to be raised if a function that is not SQLITE_RESULT_SUBTYPE
     tries invokes sqlite3_result_subtype().  The use of -DSQLITE_STRICT_SUBTYPE=1
     is a recommended compile-time option for every application that makes
     use of subtypes.
2.  Enhancements to the JSON SQL functions:
      1.  All JSON functions are rewritten to use a new internal parse tree
         format called JSONB.  The new parse-tree format is serializable 
         and hence can be stored in the database to avoid unnecessary re-parsing
         whenever the JSON value is used.
    
  2.  New versions of JSON-generating functions generate binary JSONB instead
         of JSON text.
    
  3.  The json_valid() function adds an optional second argument that
         specifies what it means for the first argument to be "well-formed".
    
3.  Add the FTS5 tokendata option to the FTS5 virtual table.
4.  The SQLITE_DIRECT_OVERFLOW_READ optimization is now enabled by default.
     Disable it at compile-time using -DSQLITE_DIRECT_OVERFLOW_READ=0.
5.  Query planner improvements:
       1.  Do not allow the transitive constraint optimization to trick the
          query planner into using a range constraint when a better equality
          constraint is available.
          (Forum post 2568d1f6e6.)
     
  2.  The query planner now does a better job of disregarding
          indexes that ANALYZE identifies as low-quality.
          (Forum post 6f0958b03b.)
     
6.  Increase the default value for SQLITE_MAX_PAGE_COUNT from 1073741824 to
     4294967294.
7.  Enhancements to the CLI:
       1.  Improvements to the display of UTF-8 content on Windows
     
  2.  Automatically detect playback of ".dump" scripts and make appropriate
          changes to settings such as ".dbconfig defensive off" and
          ".dbconfig dqs_dll on".
     
 **Hashes:**
8. SQLITE_SOURCE_ID: 2024-01-15 17:01:13 1066602b2b1976fe58b5150777cced894af17c803e068f5918390d6915b46e1d
9. SHA3-256 for sqlite3.c: f56d8e5e8c61d87b957f1cc60b3042c134d7bc0ca3aba002e6999e8f0af310a3

### 2023-11-24 (3.44.2)

1.  Fix a mistake in the CLI that was introduced by the fix (item 15 above) in 3.44.1.
2.  Fix a problem in FTS5 that was discovered during internal fuzz testing only
     minutes after the 3.44.1 release was tagged.
3.  Fix incomplete assert() statements that the fuzzer discovered the day after
     the previous release.
4.  Fix a couple of harmless compiler warnings that appeared in debug builds with GCC 16.
**Hashes:**
5. SQLITE_SOURCE_ID: 2023-11-24 11:41:44 ebead0e7230cd33bcec9f95d2183069565b9e709bf745c9b5db65cc0cbf92c0f
6. SHA3-256 for sqlite3.c: bd70b012e2d1b3efa132d905224cd0ab476a69b892f8c6b21135756ec7ffbb13

### 2023-11-22 (3.44.1)

1.  Change the CLI so that it uses UTF-16 for console I/O on Windows.  This
     enables proper display of unicode text on old Windows7 machines.
2.  Other obscure bug fixes.
**Hashes:**
3. SQLITE_SOURCE_ID: 2023-11-22 14:18:12 d295f48e8f367b066b881780c98bdf980a1d550397d5ba0b0e49842c95b3e8b4
4. SHA3-256 for sqlite3.c: e359dc502a73f3a8ad8e976a51231134d25cb93ad557a724dd92fe0c5897113a

### 2023-11-01 (3.44.0)

1.  Aggregate functions can now include an ORDER BY clause after their last
     parameter.  The arguments to the function are processed in the order
     specified.  This can be important for functions like
     string_agg() and json_group_array().
2.  Add support for the concat() and concat_ws() scalar SQL functions,
     compatible with PostgreSQL, SQLServer, and MySQL.
3.  Add support for the string_agg() aggregate SQL function, compatible
     with PostgreSQL and SQLServer.
4.  New conversion letters on the strftime() SQL function: %e %F %I %k %l %p %P %R %T %u
5.  Add new C-language APIs:  sqlite3_get_clientdata() and sqlite3_set_clientdata().
6.  Many errors associated with CREATE TABLE are now raised when the CREATE TABLE statement
     itself is run, rather than being deferred until the first time the table is actually
     used.
7.  The PRAGMA integrity_check command now verifies the consistency of the
     content in various built-in virtual tables using the new xIntegrity method.
     This works for the FTS3, FTS4, FTS5, RTREE, and GEOPOLY extensions.
8.  The SQLITE_DBCONFIG_DEFENSIVE setting now prevents PRAGMA writable_schema
     from being turned on.  Previously writable_schema could be turned on, but would
     not actually allow the schema to be writable.  Now it simply cannot be turned on.
9.  Tag the built-in FTS3, FTS4, FTS5, RTREE, and GEOPOLY virtual tables as
     SQLITE_VTAB_INNOCUOUS so that they can be used inside of triggers in
     high-security deployments.
10.  The PRAGMA case_sensitive_like statement is deprecated, as its use when the
     schema contains LIKE operators can lead to reports of database corruption
     by PRAGMA integrity_check.
11.  SQLITE_USE_SEH (Structured Exception Handling) is now enabled by default whenever
     SQLite is built using the Microsoft C compiler.  It can be disabled using
     -DSQLITE_OMIT_SEH.
12.  Query planner optimizations:
       1.  In partial index scans, if the WHERE clause implies a constant value for a table
          column, replace occurrences of that table column with the constant. This
          increases the likelihood of the partial index being a covering index.
     
  2.  Disable the view-scan optimization (added in version 3.40.0 - item 3d) 
          as it was causing multiple performance regressions. In its place, reduce
          the estimated row count for DISTINCT subqueries by a factor of 8.
     
13.  SQLite now performs run-time detection of whether or not the underlying hardware
     supports "long double" with precision greater than "double" and uses appropriate
     floating-point routines depending on what it discovered.
14.  The CLI for Windows now defaults to using UTF-8 for both input
     and output on platforms that support it.  The --no-utf8 option is available
     to disable UTF8 support.
**Hashes:**
15. SQLITE_SOURCE_ID: 2023-11-01 11:23:50 17129ba1ff7f0daf37100ee82d507aef7827cf38de1866e2633096ae6ad8130
16. SHA3-256 for sqlite3.c: d9e6530096136067644b1cb2057b3b0fa51070df99ec61971f73c9ba6aa9a36e

### 2023-10-10 (3.43.2)

1.  Fix a couple of obscure UAF errors and an obscure memory leak.
2.  Omit the use of the sprintf() function from the standard library
     in the CLI, as this now generates warnings on some platforms.
3.  Avoid conversion of a double into unsigned long long integer, as
     some platforms do not do such conversions correctly.
**Hashes:**
4. SQLITE_SOURCE_ID: 2023-10-10 12:14:04 4310099cce5a487035fa535dd3002c59ac7f1d1bec68d7cf317fd3e769484790
5. SHA3-256 for sqlite3.c: e17a3dc69330bd109256fb5a6e2b3ce8fbec48892a800389eb7c0f8856703161

### 2023-09-11 (3.43.1)

1.  Fix a regression in the way that the sum(), avg(), and total()
     aggregate functions handle infinities.
2.  Fix a bug in the json_array_length() function that occurs when the
     argument comes directly from json_remove().
3.  Fix the omit-unused-subquery-columns optimization (introduced in
     in version 3.42.0) so that it works correctly if the subquery is a
     compound where one arm is DISTINCT and the other is not.
4.  Other minor fixes.
**Hashes:**
5. SQLITE_SOURCE_ID: 2023-09-11 12:01:27 2d3a40c05c49e1a49264912b1a05bc2143ac0e7c3df588276ce80a4cbc9bd1b0
6. SHA3-256 for sqlite3.c: 391af0a4755e31ae8b29776a4a060b678823ffe4c4db558567567c688a578589

### 2023-08-24 (3.43.0)

1.  Add support for Contentless-Delete FTS5 Indexes.  This is a variety
     of FTS5 full-text search index that omits storing the content that is being indexed
     while also allowing records to be deleted.
2.  Enhancements to the date and time functions:
    1.  Added new time shift modifiers of the form `±YYYY-MM-DD HH:MM:SS.SSS` .
  2.  Added the timediff() SQL function.
  
3.  Added the octet_length(X) SQL function.
4.  Added the sqlite3_stmt_explain() API.
5.  Query planner enhancements:
    1.  Generalize the LEFT JOIN strength reduction optimization so that it works
       for RIGHT and FULL JOINs as well.  Rename it to
       OUTER JOIN strength reduction.
  
  2.  Enhance the theorem prover in the OUTER JOIN strength reduction optimization
       so that it returns fewer false-negatives.
  
6.  Enhancements to the decimal extension:
    1.  New function decimal_pow2(N) returns the N-th power of 2 for integer N
       between -20000 and +20000.
  
  2.  New function decimal_exp(X) works like decimal(X) except that it returns
       the result in exponential notation - with a "e+NN" at the end.
  
  3.  If X is a floating-point value, then the decimal(X) function now does a full
       expansion of that value into its exact decimal equivalent.
  
7.  Performance enhancements to JSON processing results in a 2x performance
     improvement for some kinds of processing on large JSON strings.
8.  New makefile target "verify-source" checks to ensure that there are no
     unintentional changes in the source tree.  (Works for 
     canonical source code only - 
     not for precompiled amalgamation tarballs.)
9.  Added the SQLITE_USE_SEH compile-time option that enables Structured
     Exception Handling on Windows while working with the memory-mapped
     shm file that is part of WAL mode processing.  This option is enabled
     by default when building on Windows using Makefile.msc.
10.  The VFS for unix now assumes that the nanosleep() system call is
     available unless compiled with -DHAVE_NANOSLEEP=0.
**Hashes:**
11. SQLITE_SOURCE_ID: 2023-08-24 12:36:59 0f80b798b3f4b81a7bb4233c58294edd0f1156f36b6ecf5ab8e83631d468778c
12. SHA3-256 for sqlite3.c: a6fc5379891d77b69a7d324cd24a437307af66cfdc3fef5dfceec3c82c8d4078

### 2023-05-16 (3.42.0)

1.  Add the FTS5 secure-delete command.  This option causes all forensic traces
     to be removed from the FTS5 inverted index when content is deleted.
2.  Enhance the JSON SQL functions to support JSON5 extensions.
3.  The SQLITE_CONFIG_LOG and SQLITE_CONFIG_PCACHE_HDRSZ calls to sqlite3_config()
     are now allowed to occur *after* sqlite3_initialize().
4.  New sqlite3_db_config() options: SQLITE_DBCONFIG_STMT_SCANSTATUS and
     SQLITE_DBCONFIG_REVERSE_SCANORDER.
5.  Query planner improvements:
       1.  Enable the "count-of-view" optimization by default.
     
  2.  Avoid computing unused columns in subqueries.
     
  3.  Improvements to the WHERE-clause push-down optimization.
     
6.  Enhancements to the CLI:
       1.  Add the --unsafe-testing command-line option.  Without this option, some
          dot-commands (ex: ".testctrl") are now disabled because those commands
          that are intended for testing only and can cause malfunctions if misused.
     
  2.  Allow commands ".log on" and ".log off", even in --safe mode.
     
  3.  "--" as a command-line argument means all subsequent arguments that
          start with "-" are interpreted as normal non-option argument.
     
  4.  Magic parameters ":inf" and ":nan" bind to floating point literals
          Infinity and NaN, respectively.
     
  5.  The --utf8 command-line option omits all translation to or from
          MBCS on the Windows console for interactive sessions, and sets
          the console code page for UTF-8 I/O during such sessions.
          The --utf8 option is a no-op on all other platforms.
     
7.  Add the ability for application-defined SQL functions to have the same name
     as join keywords: CROSS, FULL, INNER, LEFT, NATURAL, OUTER, or RIGHT.
8.  Enhancements to PRAGMA integrity_check:
       1.  Detect and raise an error when a NaN value is stored in a NOT NULL column.
     
  2.  Improved error message output identifies the root page of a b-tree when
          an error is found within a b-tree.
     
9.  Allow the session extension to be configured to capture changes from
     tables that lack an explicit ROWID.
10.  Added the subsecond modifier to the date and time functions.
11.  Negative values passed into sqlite3_sleep() are henceforth interpreted as 0.
12.  The maximum recursion depth for JSON arrays and objects is lowered from 2000
     to 1000.
13.  Extended the built-in printf() function so the comma option now works with
     floating-point conversions in addition to integer conversions.
14.  Miscellaneous bug fixes and performance optimizations
**Hashes:**
15. SQLITE_SOURCE_ID: 2023-05-16 12:36:15 831d0fb2836b71c9bc51067c49fee4b8f18047814f2ff22d817d25195cf350b0
16. SHA3-256 for sqlite3.c: 6aa3fadf000000625353bbaa1e83af114c40c240a0aa5a2c1c2aabcfc28d4f92

### 2023-03-22 (3.41.2)

1.  Multiple fixes for reads past the end of memory buffers
     (NB: *reads* not*writes* ) in the following circumstances:  1.  When processing a corrupt database file  using the non-standard
         SQLITE_ENABLE_STAT4 compile-time option.
     
  2.  In the CLI when the sqlite3_error_offset() routine returns an out-of-range value
          (see also the fix to sqlite3_error_offset() below).
     
  3.  In the recovery extension.
     
  4.  In FTS3 when processing a corrupt database file.
     
2.  Fix the sqlite3_error_offset() so that it does not return out-of-range values when
     reporting errors associated with generated columns.
3.  Multiple fixes in the query optimizer for problems that cause incorrect
     results for bizarre, fuzzer-generated queries.
4.  Increase the size of the reference counter in the page cache object to 64 bits to
     ensure that the counter never overflows.
5.  Fix a performance regression caused by a bug fix in patch release 3.41.1.
6.  Fix a few incorrect assert() statements.
**Hashes:**
7. SQLITE_SOURCE_ID: 2023-03-22 11:56:21 0d1fc92f94cb6b76bffe3ec34d69cffde2924203304e8ffc4155597af0c191da
8. SHA3-256 for sqlite3.c: c83f68b7aac1e7d3ed0fcdb9857742f024449e1300bfb3375131a6180b36cf7c

### 2023-03-10 (3.41.1)

1.  Provide compile-time options -DHAVE_LOG2=0 and -DHAVE_LOG10=0 to enable SQLite to be
     compiled on systems that omit the standard library functions log2() and log10(), repectively.
2.  Ensure that the datatype for column t1.x in 
     "`CREATE TABLE t1 AS SELECT CAST(7 AS INT) AS x;` " continues to be INT and is not NUM,
     for historical compatibility.
3.  Enhance PRAGMA integrity_check to detect when extra bytes appear at the end of an
     index record.
4.  Fix various obscure bugs reported by the user community.  See the
     timeline of changes
     for details.
     
**Hashes:**
5. SQLITE_SOURCE_ID: 2023-03-10 12:13:52 20399f3eda5ec249d147ba9e48da6e87f969d7966a9a896764ca437ff7e737ff
6. SHA3-256 for sqlite3.c: d0d9db8425570f4a57def04fb8f4ac84f5c3e4e71d3d4d10472e3639c5fdf09f

### 2023-02-21 (3.41.0)

1. Query planner improvements:
      1.  Make use of indexed expressions within an aggregate query that
         includes a GROUP BY clause.
    
  2.  The query planner has improved awareness of when an index is
         a covering index and adjusts predicted runtimes accordingly.
    
  3.  The query planner is more aggressive about using co-routines rather
         than materializing subqueries and views.
    
  4.  Queries against the built-in table-valued functions json_tree() and
         json_each() will now usually treat "ORDER BY rowid" as a no-op.
    
  5.  Enhance the ability of the query planner to use indexed expressions
         even if the expression has been modified by the
         constant-propagation optimization.
         (See forum thread 0a539c7.)
    
2. Add the built-in unhex() SQL function.
3. Add the base64 and base85 application-defined functions as an extension and
    include that extension in the CLI.
4. Add the sqlite3_stmt_scanstatus_v2() interface. (This interface is only
    available if SQLite is compiled using SQLITE_ENABLE_STMT_SCANSTATUS.)
5. In-memory databases created using sqlite3_deserialize() now report their
    filename as an empty string, not as 'x'.
6. Changes to the CLI:
      1.  Add the new base64() and base85() SQL functions
    
  2.  Enhanced EXPLAIN QUERY PLAN output using the new sqlite3_stmt_scanstatus_v2()
         interface when compiled using SQLITE_ENABLE_STMT_SCANSTATUS.
    
  3.  The "`.scanstats est` " command provides query planner estimates in profiles.
  4.  The continuation prompt indicates if the input is currently inside of a
         string literal, identifier literal, comment, trigger definition, etc.
    
  5.  Enhance the --safe command-line option to disallow dangerous SQL functions.
    
  6.  The double-quoted string misfeature is now disabled by default for CLI
         builds.  Legacy use cases can reenable the misfeature at run-time using
         the "`.dbconfig dqs_dml on` " and "`.dbconfig dqs_ddl on` "
         commands.
7. Enhance the PRAGMA integrity_check command so that it detects when text strings in
    a table are equivalent to but not byte-for-byte identical to the same strings in the index.
8. Enhance the carray table-valued function so that it is able to bind an array of
    BLOB objects.
9. Added the sqlite3_is_interrupted() interface.
10. Long-running calls to sqlite3_prepare() and similar now invoke the
    progress handler callback and react to sqlite3_interrupt().
11. The sqlite3_vtab_in_first() and sqlite3_vtab_in_next() functions are enhanced so that
    they reliably detect if they are invoked on a parameter that was not selected for
    multi-value IN processing using sqlite3_vtab_in().
    They return SQLITE_ERROR instead of SQLITE_MISUSE in this case.
12. The parser now ignores excess parentheses around a subquery on the right-hand side
    of an IN operator, so that SQLite now works the same as PostgreSQL in this regard.
    Formerly, SQLite treated the subquery as an expression with an implied "LIMIT 1".
13. Added the SQLITE_FCNTL_RESET_CACHE option to the sqlite3_file_control() API.
14. Makefile improvements:
      1. The new makefile target "sqlite3r.c" builds an amalgamation that includes
        the recovery extension.
    
  2. New makefile targets "devtest" and "releasetest" for running a
        quick developmental test prior to doing a check-in and for doing a full
        release test, respectively.
    
15. Miscellaneous performance enhancements.
**Hashes:**
16. SQLITE_SOURCE_ID: 2023-02-21 18:09:37 05941c2a04037fc3ed2ffae11f5d2260706f89431f463518740f72ada350866d
17. SHA3-256 for sqlite3.c: 02bd9e678460946810801565667fdb8f0c29c78e51240512d2e5bb3dbdee7464

### 2022-12-28 (3.40.1)

1.  Fix the --safe command-line option to the CLI
     such that it correctly disallows the
     use of SQL functions like writefile() that can cause harmful side-effects.
2.  Fix a potential infinite loop in the memsys5 alternative memory allocator.  This
     bug was introduced by a performance optimization in version 3.39.0.
3.  Various other obscure fixes.
**Hashes:**
4. SQLITE_SOURCE_ID: 2022-12-28 14:03:47 df5c253c0b3dd24916e4ec7cf77d3db5294cc9fd45ae7b9c5e82ad8197f38a24
5. SHA3-256 for sqlite3.c: 4d6800e9032ff349376fe612e422b49ba5eb4e378fac0b3e405235d09dd366ab

### 2022-11-16 (3.40.0)

1.  Add support for compiling SQLite to WASM
     and running it in web browsers.  NB:  The WASM build and its interfaces
     are considered "beta" and are subject to minor changes if the need
     arises.  We anticipate finalizing the interface for the next release.
2.  Add the recovery extension that might be able to recover some content
     from a corrupt database file.
3.  Query planner enhancements:
       1.  Recognize covering indexes on tables with more than 63 columns where
          columns beyond the 63rd column are used in the query and/or are
          referenced by the index.
     
  2.  Extract the values of expressions contained within expression indexes
          where practical, rather than recomputing the expression.
     
  3.  The NOT NULL and IS NULL operators (and their equivalents) avoid
          loading the content of large strings and BLOB values from disk.
     
  4.  Avoid materializing a view on which a full scan is performed
          exactly once.  Use and discard the rows of the view as they are computed.
     
  5.  Allow flattening of a subquery that is the right-hand operand of
          a LEFT JOIN in an aggregate query.
     
4.  A new typedef named sqlite3_filename is added and used to represent
     the name of a database file.  Various interfaces are
     modified to use the new typedef instead of "char*".  This interface
     change should be fully backwards compatible, though it might cause
     (harmless) compiler warnings when rebuilding some legacy applications.
5.  Add the sqlite3_value_encoding() interface.
6.  Security enhancement: SQLITE_DBCONFIG_DEFENSIVE is augmented to prohibit
     changing the schema_version.  The schema_version
     becomes read-only in defensive mode.
7.  Enhancements to the PRAGMA integrity_check statement:
       1.  Columns in non-STRICT tables with TEXT affinity should not contain numeric values.
     
  2.  Columns in non-STRICT tables with NUMERIC affinity should not
          contain TEXT values that could be converted into numbers.
     
  3.  Verify that the rows of a WITHOUT ROWID table are in the correct order.
     
8.  Enhance the VACUUM INTO statement so that it honors the
     PRAGMA synchronous setting.
9.  Enhance the sqlite3_strglob() and sqlite3_strlike() APIs so that they are able
     to accept NULL pointers for their string parameters and still generate a sensible
     result.
10.  Provide the new SQLITE_MAX_ALLOCATION_SIZE compile-time option for limiting
     the size of memory allocations.
11.  Change the algorithm used by SQLite's built-in pseudo-random number generator (PRNG)
     from RC4 to Chacha20.
12.  Allow two or more indexes to have the same name as long as they are all in
     separate schemas.
13.  Miscellaneous performance optimizations result in about 1% fewer CPU cycles
     used on typical workloads.
**Hashes:**
14. SQLITE_SOURCE_ID: 2022-11-16 12:10:08 89c459e766ea7e9165d0beeb124708b955a4950d0f4792f457465d71b158d318
15. SHA3-256 for sqlite3.c: ab8da6bc754642989e67d581f26683dc705b068cea671970f0a7d32cfacbad19

### 2022-09-29 (3.39.4)

1.  Fix the build on Windows so that it works with -DSQLITE_OMIT_AUTOINIT
2.  Fix a long-standing problem in the btree balancer that might, in rare cases,
     cause database corruption if the application uses an
     application-defined page cache.
3.  Enhance SQLITE_DBCONFIG_DEFENSIVE so that it disallows CREATE TRIGGER
     statements if one or more of the statements in the body of the trigger write
     into shadow tables.
4.  Fix a possible integer overflow in the size computation for a memory allocation
     in FTS3.
5.  Fix a misuse of the sqlite3_set_auxdata() interface in the
     ICU Extension.
**Hashes:**
6. SQLITE_SOURCE_ID: 2022-09-29 15:55:41 a29f9949895322123f7c38fbe94c649a9d6e6c9cd0c3b41c96d694552f26b309
7. SHA3-256 for sqlite3.c: f65082298127e2ddae6539beb94f5204b591df64ba2c7da83c7d0faffd6959d8

### 2022-09-05 (3.39.3)

1.  Use a statement journal on DML statement affecting two or more database
     rows if the statement makes use of a SQL functions that might abort.  See
     forum thread 9b9e4716c0d7bbd1.
2.  Use a mutex to protect the PRAGMA temp_store_directory and
     PRAGMA data_store_directory statements, even though they are deprecated and
     documented as not being threadsafe. See
     forum post 719a11e1314d1c70.
3.  Other bug and warning fixes.  See the
     timeline
     for details.
**Hashes:**
4. SQLITE_SOURCE_ID: 2022-09-05 11:02:23 4635f4a69c8c2a8df242b384a992aea71224e39a2ccab42d8c0b0602f1e826e8
5. SHA3-256 for sqlite3.c: 2fc273cf8032b601c9e06207efa0ae80eb73d5a1d283eb91096c815fa9640257

### 2022-07-21 (3.39.2)

1.  Fix a performance regression in the query planner associated with rearranging
     the order of FROM clause terms in the presences of a LEFT JOIN.
2.  Apply fixes for CVE-2022-35737,  Chromium bugs 1343348 and 1345947,
     forum post 3607259d3c, and
     other minor problems discovered by internal testing.
**Hashes:**
3. SQLITE_SOURCE_ID: 2022-07-21 15:24:47 698edb77537b67c41adc68f9b892db56bcf9a55e00371a61420f3ddd668e6603
4. SHA3-256 for sqlite3.c: bffbaafa94706f0ed234f183af3eb46e6485e7e2c75983173ded76e0da805f11

### 2022-07-13 (3.39.1)

1.  Fix an incorrect result from a query that uses a view that contains a compound
     SELECT in which only one arm contains a RIGHT JOIN and where the view is not
     the first FROM clause term of the query that contains the view.
     forum post 174afeae5734d42d.
2.  Fix some harmless compiler warnings.
3.  Fix a long-standing problem with ALTER TABLE RENAME that can only arise
     if the sqlite3_limit(SQLITE_LIMIT_SQL_LENGTH) is set to a very small value.
4.  Fix a long-standing problem in FTS3 that can only arise when compiled with
     the SQLITE_ENABLE_FTS3_PARENTHESIS compile-time option.
5.  Fix the build so that is works when the SQLITE_DEBUG and
     SQLITE_OMIT_WINDOWFUNC compile-time options are both provided at the
     same time.
6.  Fix the initial-prefix optimization for the REGEXP extension so that it works
     correctly even if the prefix contains characters that require a 3-byte UTF8
     encoding.
7.  Enhance the sqlite_stmt virtual table so that it buffers all of its output.
**Hashes:**
8. SQLITE_SOURCE_ID: 2022-07-13 19:41:41 7c16541a0efb3985578181171c9f2bb3fdc4bad6a2ec85c6e31ab96f3eff201f
9. SHA3-256 for sqlite3.c: 6d13fcf1c31133da541d1eb8a83552d746f39b81a0657bd4077fed0221749511

### 2022-06-25 (3.39.0)

1.  Add (long overdue) support for RIGHT and FULL OUTER JOIN.
2.  Add new binary comparison operators IS NOT DISTINCT FROM and IS DISTINCT FROM
     that are equivalent to IS and IS NOT, respective, for compatibility with
     PostgreSQL and SQL standards.
3.  Add a new return code (value "3") from the sqlite3_vtab_distinct()
     interface that indicates a query that has both DISTINCT and ORDER BY
     clauses.
4.  Added the sqlite3_db_name() interface.
5.  The unix os interface resolves all symbolic links in database
     filenames to create a canonical name for the database before the
     file is opened.
     If the SQLITE_OPEN_NOFOLLOW flag is used with sqlite3_open_v2()
     or similar, the open will fail if any element of the path is a
     symbolic link.
6.  Defer materializing views until the materialization
     is actually needed, thus avoiding unnecessary work if the materialization turns
     out to never be used.
7.  The HAVING clause of a SELECT statement is now allowed on any aggregate query,
     even queries that do not have a GROUP BY clause.
8.  Many microoptimizations collectively reduce CPU cycles by about 2.3%.
**Hashes:**
9. SQLITE_SOURCE_ID: 2022-06-25 14:57:57 14e166f40dbfa6e055543f8301525f2ca2e96a02a57269818b9e69e162e98918
10. SHA3-256 for sqlite3.c: d9c439cacad5e4992d0d25989cfd27a4c4f59a3183c97873bc03f0ad1aa78b7a

### 2022-05-06 (3.38.5)

1.  Fix a blunder in the CLI of the 3.38.4 release.
**Hashes:**
2. SQLITE_SOURCE_ID: 2022-05-06 15:25:27 78d9c993d404cdfaa7fdd2973fa1052e3da9f66215cff9c5540ebe55c407d9fe
3. SHA3-256 for sqlite3.c: b05ef42ed234009b4b3dfb36c5f5ccf6d728da80f25ee560291269cf6cfe635f

### 2022-05-04 (3.38.4)

1.  Fix a byte-code problem in the Bloom filter pull-down optimization added by release
     3.38.0 in which an error in the byte code causes the byte code engine to enter an
     infinite loop when the pull-down optimization encounters a NULL key.
     Forum thread 2482b32700384a0f.
2.  Other minor patches.  See the
     timeline for
     details.
**Hashes:**
3. SQLITE_SOURCE_ID: 2022-05-04 15:45:55 d402f49871152670a62f4f28cacb15d814f2c1644e9347ad7d258e562978e45e
4. SHA3-256 for sqlite3.c: e6a50effb021858c200e885664611ed3c5e949413ff2dca452ac7ee336b9de1d

### 2022-04-27 (3.38.3)

1.  Fix a case of the query planner be overly aggressive with optimizing automatic-index
     and Bloom-filter construction, using inappropriate ON clause terms to restrict the
     size of the automatic-index or Bloom filter, and resulting in missing rows in the
     output.
     Forum thread 0d3200f4f3bcd3a3.
2.  Other minor patches.  See the
     timeline for
     details.
**Hashes:**
3. SQLITE_SOURCE_ID: 2022-04-27 12:03:15 9547e2c38a1c6f751a77d4d796894dec4dc5d8f5d79b1cd39e1ffc50df7b3be4
4. SHA3-256 for sqlite3.c: d4d66feffad66ea82073fbb97ae9c84e3615887ebc5168226ccee28d82424517

### 2022-03-26 (3.38.2)

1.  Fix a user-discovered problem with the new Bloom filter optimization
     that might cause an incorrect answer when doing a LEFT JOIN with a WHERE
     clause constraint that says that one of the columns on the right table of
     the LEFT JOIN is NULL. See
     forum thread 031e262a89b6a9d2.
2.  Other minor patches.  See the
     timeline for
     details.
**Hashes:**
