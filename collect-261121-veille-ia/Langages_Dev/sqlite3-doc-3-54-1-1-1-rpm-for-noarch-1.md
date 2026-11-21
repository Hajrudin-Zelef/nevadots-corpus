---
id: collect-261121-veille-ia/langages-dev/sqlite3-doc-3-54-1-1-1-rpm-for-noarch-1
title: "sqlite3-doc-3.54.1-1.1 RPM for noarch"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-10-20"]
keywords: ["sqlite", "sqlite3", "index", "opensuse", "version", "documentation", "other", "limit"]
source: https://rpmfind.net/linux/RPM/opensuse/tumbleweed/noarch/sqlite3-doc-3.54.1-1.1.noarch.html
source_anchor: ""
source_lines: [1, 1500]
section: "Langages_Dev"
---
| Index | index by Group | index by Distribution | index by Vendor | index by creation date | index by Name | Mirrors | Help | Search | 

| Name: sqlite3-doc | Distribution: openSUSE Tumbleweed | 
| Version: 3.54.1 | Vendor: openSUSE | 
| Release: 1.1 | Build date: Tue Oct 20 16:37:31 2026 | 
| Group: Documentation/Other | Build host: reproducible | 
| Size: 37670614 | Source RPM: sqlite3-3.54.1-1.1.src.rpm | 
| Packager: https://bugs.opensuse.org |  | 
| Url: https://www.sqlite.org/ |  | 
| Summary: Documentation for sqlite3 |  | 

Contains HTML documentation for SQLite: SQL Syntax, C/C++ API and
other documentation found on sqlite.org. The files can be found in
/usr/share/doc/packages/sqlite3-doc.

LicenseRef-SUSE-Public-Domain

```
* Tue Oct 20 2026 Reinhard Max <max@suse.com>
  - Update to version 3.54.1:
    * https://sqlite.org/releaselog/3_54_1.html
    * Fix a possible heap-buffer overflow on 32-bit builds (64-bit
      builds are not affected).
    * Fix other problems in 3.54.0 reported by users.
* Mon Oct 12 2026 Reinhard Max <max@suse.com>
  - Update to version 3.54.0:
    * https://sqlite.org/releaselog/3_54_0.html
    * CLI enhancements: prompt strings may contain escape sequences
      that expand to show state information, and the initial prompt
      is taken from the SQLITE_PS1 and SQLITE_PS2 environment
      variables if set. The default prompt honors NO_COLOR.
    * CLI: columnar output modes now show the column name header by
      default even if the query returns no rows.
    * CLI: the new ".diskused" dot-command (based on the new
      diskused() extension) replaces the separate sqlite3_analyzer
      utility, which is now deprecated upstream.
    * CLI: the .mode command gained the --rowcount, --titles,
    - -ifmt and --fpfmt options.
    * CLI: ending an SQL command with a line containing just "go" or
      "/" is discontinued unless compiled with
    - DSQLITE_SHELL_LEGACY_COMMAND_TERMINATOR.
    * Query planner improvements, including better optimization of
      "expr OR TRUE" / "expr OR FALSE", skipping needless updates of
      expression indexes, ignoring DISTINCT on the right-hand side
      of IN, and faster UNION queries with a LIMIT of exactly 1.
    * ALTER TABLE now raises an error on any attempt to add, drop,
      or rename columns named "ROWID", "_ROWID_" or "OID".
    * New %J and %j conversions to render JSON string literals in
      the built-in printf() and the format() SQL function.
    * New C-language interfaces: sqlite3_result_str() and
      sqlite3_incomplete().
    * Date/time functions: added the "weekday -N" form of the
      weekday modifier and the "end of" modifier.
    * The authorizer callback is now invoked for SQL functions found
      in the DEFAULT clauses of CREATE TABLE statements.
    * Added SQLITE_LIMIT_SCHEMA and SQLITE_LIMIT_TRIGGER_STEPS to
      the sqlite3_limit() interface.
    * Use O_TMPFILE when generating temporary files on systems that
      support it.
    * About 4% faster as measured by counting CPU cycles in the
      speedtest1 benchmark.
  - Raise RLIMIT_DATA in %check so that the ASAN based tests can
    reserve their shadow memory region.
* Tue Jun 30 2026 Holden Fried <holden@opensuse.org>
  - Add sqlite3-tools and sqlite3-tools-tcl subpackages
    * Most other distributions have included a separate -tools
      package with these for a while, but we seemingly haven't been
      packaging them at all in openSUSE.
    * New tools include the ones packaged by debian trixie (showdb,
      showjournal, showstat4, showwal, sqldiff, and sqlite3_analyzer)
      along with sqlite3_rsync, which is recommended in sqlite3 docs
    * All tools are in -tools subpackage except for sqlite3_analyzer
      which is in -tools-tcl
* Fri Jun 05 2026 Reinhard Max <max@suse.com>
  - Update to version 3.53.2:
    * Fixes for problems in 3.53.0 reported by users.
    * bsc#1268013, CVE-2026-11824: heap-based buffer overflow
      vulnerability in the FTS5 full-text search extension.
    * bsc#1268012, CVE-2026-11822: memory corruption vulnerabilities
      in the FTS5 full-text search extension.
    * See the check-in timeline for details:
      https://sqlite.org/src/timeline?from=version-3.53.1&to=version-3.53.2
* Tue May 05 2026 Reinhard Max <max@suse.com>
  - Update to version 3.53.1:
    * Fixes for problems in 3.53.0 reported by users.
    * See the check-in timeline for details:
      https://sqlite.org/src/timeline?from=version-3.53.0&to=version-3.53.1
* Thu Apr 09 2026 Reinhard Max <max@suse.com>
  - Update to version 3.53.0:
    * https://sqlite.org/releaselog/3_53_0.html
    * Add the Query Result Formatter (QRF) library for formatting the
      results of SQL queries for human readability on a fixed-pitch
      font screen.
    * Enhance ALTER TABLE to permit adding and removing NOT NULL and
      CHECK constraints.
    * The REINDEX EXPRESSIONS statement rebuilds expression indexes.
    * The body of TEMP triggers may now modify and/or query tables
      in the main schema.
    * Enhance VACUUM INTO so that if a URI filename is used as the
      target and that filename has a reserve=N query parameter with
      N between 0 and 255, then the reserve amount for the generated
      database copy is set to N.
    * New SQL functions json_array_insert() and jsonb_array_insert().
    * Renovations to the CLI.
    * New C-language interfaces: sqlite3_str_truncate(),
      sqlite3_str_free(), sqlite3_carray_bind_v2().
    * Add the SQLITE_PREPARE_FROM_DDL option to sqlite3_prepare_v3().
    * Added the SQLITE_UTF8_ZT constant which can be used as the
      encoding parameter to sqlite3_result_text64() or
      sqlite3_bind_text64() to indicate that the value is UTF-8
      encoded and zero terminated.
    * The SQLITE_LIMIT_PARSER_DEPTH option is added to
      sqlite3_limit().
    * The SQLITE_DBCONFIG_FP_DIGITS option is added to
      sqlite3_db_config().
    * Query planner improvements.
    * Add new interfaces to the session extension that enable an
      application to add changes one at a time to the
      sqlite3_changegroup object.
    * Improvements to floating-point ↔ text conversions.
    * Added the self-healing index feature to deal with the stale
      expression index problem.
    * Add the "-p|--port" option to sqlite3_rsync.
    * Add the "opfs-wl" VFS, functionally identical to the "opfs" VFS
      but using Web Locks for locking, which can promise fairer lock
      sharing than the "opfs" bespoke protocol can. "opfs-wl"
      requires Atomics.waitAsync(), so requires newer browsers than
      "opfs" does.
* Fri Mar 13 2026 Reinhard Max <max@suse.com>
  - Update to version 3.51.3:
    * Fix the WAL-reset database corruption bug:
      https://sqlite.org/wal.html#walresetbug
    * Other minor bug fixes.
* Thu Jan 22 2026 Reinhard Max <max@suse.com>
  - Update to version 3.51.2:
    * bsc#1259619, CVE-2025-70873: zipfile extension may disclose
      uninitialized heap memory during inflation.
    * Fix an obscure deadlock in the new broken-posix-lock detection
      logic.
    * Fix multiple problems in the EXISTS-to-JOIN optimization.
    * Other minor bug fixes.
* Fri Nov 28 2025 Reinhard Max <max@suse.com>
  - Update to version 3.51.1:
    * Fix incorrect results from nested EXISTS queries caused by the
      optimization in item 6b in the 3.51.0 release.
    * Fix a latent bug in fts5vocab virtual table, exposed by new
      optimizations in the 3.51.0 release
  - Changes in version 3.51.0:
    * New macros in sqlite3.h:
    - SQLITE_SCM_BRANCH → the name of the branch from which the
      source code is taken.
    - SQLITE_SCM_TAGS → space-separated list of tags on the source
      code check-in.
    - SQLITE_SCM_DATETIME → ISO-8601 date and time of the source
      code check-in.
    * Two new JSON functions, jsonb_each() and jsonb_tree() work the
      same as the existing json_each() and json_tree() functions
      except that they return JSONB for the "value" column when the
      "type" is 'array' or 'object'.
    * The carray and percentile extensions are now built into the
      amalgamation, though they are disabled by default and must be
      activated at compile-time using the -DSQLITE_ENABLE_CARRAY
      and/or -DSQLITE_ENABLE_PERCENTILE options, respectively.
    * Enhancements to TCL Interface:
    - Add the -asdict flag to the eval command to have it set the
      row data as a dict instead of an array.
    - User-defined functions may now break to return an SQL NULL.
    * CLI enhancements:
    - Increase the precision of ".timer" to microseconds.
    - Enhance the "box" and "column" formatting modes to deal with
      double-wide characters.
    - The ".imposter" command provides read-only imposter tables
      that work with VACUUM and do not require the --unsafe-testing
      option.
    - Add the --ifexists option to the CLI command-line option and
      to the .open command.
    - Limit columns widths set by the ".width" command to 30,000 or
      less, as there is not good reason to have wider columns, but
      supporting wider columns provides opportunity to malefactors.
    * Performance enhancements:
    - Use fewer CPU cycles to commit a read transaction.
    - Early detection of joins that return no rows due to one or
      more of the tables containing no rows.
    - Avoid evaluation of scalar subqueries if the result of the
      subquery does not change the result of the overall expression.
    - Faster window function queries when using
      "BETWEEN :x FOLLOWING AND :y FOLLOWING" with a large :y.
    * Add the PRAGMA wal_checkpoint=NOOP; command and the
      SQLITE_CHECKPOINT_NOOP argument for sqlite3_wal_checkpoint_v2().
    * Add the sqlite3_set_errmsg() API for use by extensions.
    * Add the sqlite3_db_status64() API, which works just like the
      existing sqlite3_db_status() API except that it returns 64-bit
      results.
    * Add the SQLITE_DBSTATUS_TEMPBUF_SPILL option to the
      sqlite3_db_status() and sqlite3_db_status64() interfaces.
    * In the session extension add the sqlite3changeset_apply_v3()
      interface.
    * For the built-in printf() and the format() SQL function, omit
      the leading '-' from negative floating point numbers if the '+'
      flag is omitted and the "#" flag is present and all displayed
      digits are '0'. Use '%#f' or similar to avoid outputs like
      '-0.00' and instead show just '0.00'.
    * Improved error messages generated by FTS5.
    * Enforce STRICT typing on computed columns.
    * Improved support for VxWorks
    * JavaScript/WASM now supports 64-bit WASM. The canonical builds
      continue to be 32-bit but creating one's own 64-bit build is
      now as simple as running "make".
    * Improved resistance to database corruption caused by an
      application breaking Posix advisory locks using close().
* Tue Oct 21 2025 Reinhard Max <max@suse.com>
  - bsc#1252217: Add a %license file.
* Tue Aug 26 2025 Reinhard Max <max@suse.com>
  - bsc#1248586: Fix icu-enabled build.
* Mon Aug 11 2025 Reinhard Max <max@suse.com>
  - Update to version 3.50.4:
    * Fix two long-standings cases of the use of uninitialized
      variables in obscure circumstances.
* Thu Jul 17 2025 Reinhard Max <max@suse.com>
  - Update to version 3.50.3:
    * Fix a possible memory error that can occur if a query is made
      against against FTS5 index that has been deliberately corrupted
      in a very specific way (CVE-2025-7709, bsc#1254670).
    * Fix the parser so that it ignored SQL comments in all places of
      a CREATE TRIGGER statement. This resolves a problem that was
      introduced by the introduction of the
      SQLITE_DBCONFIG_ENABLE_COMMENTS feature in version 3.49.0.
    * Fix an incorrect answer due to over-optimization of an AND
      operator.
* Tue Jul 01 2025 Reinhard Max <max@suse.com>
  - Update to 3.50.2:
    * Fix the concat_ws() SQL function so that it includes empty
      strings in the concatenation.
    * Avoid writing frames with no checksums into the wal file if a
      savepoint is rolled back after dirty pages have already been
      spilled into the wal file.
    * Fix the Bitvec object to avoid stack overflow when the
      database is within 60 pages of its maximum size.
    * Fix a problem with UPDATEs on fts5 tables that contain BLOB
      values.
    * Fix an issue with transitive IS constraints on a RIGHT JOIN.
    * CVE-2025-6965, bsc#1246597:
      Raise an error early if the number of aggregate terms in a
      query exceeds the maximum number of columns, to avoid
      downstream assertion faults.
    * Ensure that sqlite3_setlk_timeout() holds the database mutex.
* Mon Jun 09 2025 Andreas Stieger <andreas.stieger@gmx.de>
  - Update to 3.50 (3.50.1):
    * Improved handling and robust output of control characters
    * sqlite3_rsync no longer requires WAL mode and needs less
      bandwidth
    * Bug fixes and optimized JSON handling
    * Performance optimizations and developer visible fixes
* Wed May 07 2025 Reinhard Max <max@suse.com>
  - Update to release 3.49.2:
    * Fix a bug in the NOT NULL optimization of version 3.40.0 that
      can lead to a memory error if abused.
    * Fix the count-of-view optimization so that it does not give an
      incorrect answer for a DISTINCT query.
    * Fix a possible incorrect answer that can result if a UNIQUE
      constraint of a table contains the PRIMARY KEY column and that
      UNIQUE constraint is used by an IN operator.
    * Fix obscure problems with the generate_series() extension
      function.
    * Incremental improvements to the configure/make.
* Thu Apr 17 2025 Martin Hauke <mardnh@gmx.de>
  - Add subpackage for the lemon parser generator.
  - Add patches:
    * sqlite-3.49.0-fix-lemon-missing-cflags.patch
    * sqlite-3.6.23-lemon-system-template.patch
* Tue Mar 11 2025 Jan Engelhardt <jengelh@inai.de>
  - Mark build recipe as POSIX-sh-incompatible
  - Run mkdir/rm with verbose mode for the build log
* Tue Feb 18 2025 Reinhard Max <max@suse.com>
  - Update to release 3.49.1:
    * Improve portability of makefiles and configure scripts.
    * CVE-2025-29087, CVE-2025-3277, bsc#1241020:
      Fix a bug in the concat_ws() function, introduced in version
      3.44.0, that could lead to a memory error if the separator
      string is very large (hundreds of megabytes).
    * CVE-2025-29088, bsc#1241078: Enhanced the
      SQLITE_DBCONFIG_LOOKASIDE interface to make it  more robust
      against misuse.
* Thu Feb 06 2025 Reinhard Max <max@suse.com>
  - Update to release 3.49.0:
    * Enhancements to the query planner:
    - Improve the query-time index optimization so that it works on
      WITHOUT ROWID tables.
    - Better query plans for large star-query joins. This fixes
      three different performance regressions that were reported
      on the SQLite Forum.
    - When two or more queries have the same estimated cost, use
      the one with the fewer bytes per row.
    * Enhance the iif() SQL function so that it can accept any number
      of arguments greater than or equal to two.
    * Enhance the session extension so that it works on databases
      that make use of generated columns.
    * Omit the SQLITE_USE_STDIO_FOR_CONSOLE compile-time option which
      was not implemented correctly and never worked right. In its place
      add the SQLITE_USE_W32_FOR_CONSOLE_IO compile-time option. This
      option applies to command-line tools like the CLI only, not to the
      SQLite core. It causes Win32 APIs to be used for console I/O
      instead of stdio. This option affects Windows builds only.
    * Three new options to sqlite3_db_config(). All default "on".
      SQLITE_DBCONFIG_ENABLE_ATTACH_CREATE
      SQLITE_DBCONFIG_ENABLE_ATTACH_WRITE
      SQLITE_DBCONFIG_ENABLE_COMMENTS
* Thu Jan 16 2025 Reinhard Max <max@suse.com>
  - Re-enable SONAME which got disabled by default in 3.48.0.
    * https://www.sqlite.org/src/forumpost/5a3b44f510df8ded
    * https://sqlite.org/forum/forumpost/ab8f15697a
* Tue Jan 14 2025 Reinhard Max <max@suse.com>
  - Update to release 3.48.0:
    * Improved EXPLAIN QUERY PLAN output for covering indexes.
    * Allow a two-argument version of the iif() SQL function.
    * Also allow if() as an alternative spelling for iif().
    * Add the ".dbtotxt" command to the CLI.
    * Add the SQLITE_IOCAP_SUBPAGE_READ property to the
      xDeviceCharacteristics method of the sqlite3_io_methods object.
    * Add the SQLITE_PREPARE_DONT_LOG option to sqlite3_prepare_v3()
      that prevents warning messages being sent to the error log if
      the SQL is ill-formed. This allows sqlite3_prepare_v3() to be
      used to do test compiles of SQL to check for validity without
      polluting the error log with false messages.
    * Increase the minimum allowed value of SQLITE_LIMIT_LENGTH from
      1 to 30.
    * Added the SQLITE_FCNTL_NULL_IO file control.
    * Extend the FTS5 auxiliary API xInstToken() to work with prefix
      queries via the insttoken configuration option and the
      fts5_insttoken() SQL function.
    * Increase the maximum number of arguments to an SQL function
      from 127 to 1000.
    * Obsoletes sqlite3-6216bfcb.patch .
* Mon Dec 16 2024 Reinhard Max <max@suse.com>
  - Add sqlite3-6216bfcb.patch to fix a test suite regression in
    3.47.0 on s390x. Only the test was broken, not the code itself.
    https://sqlite.org/forum/forumpost/7b2bab04c5
* Mon Dec 09 2024 Reinhard Max <max@suse.com>
  - Update to release 3.47.2:
    * Fix a problem in text-to-floating-point conversion that affects
      text values where the first 16 significant digits are
      '1844674407370955'. This issue was introduced in 3.47.0 and
      only arises on x64 and i386 hardware.
    * Other minor bug fixes.
  - Enable the session extension, because NodeJS 22 needs it.
* Wed Nov 27 2024 Reinhard Max <max@suse.com>
  - Update to release 3.47.1:
    * Fix the makefiles so that they once again honored DESTDIR for
      the "install" target.
    * Add the SQLITE_IOCAP_SUBPAGE_READ capability to the VFS, to
      work around issues on some non-standard VFSes caused by making
      SQLITE_DIRECT_OVERFLOW_READ the default in version 3.45.0.
    * Fix incorrect answers to certain obscure IN queries caused by
      new query optimizations added in the 3.47.0 release.
    * Other minor bug fixes.
* Tue Oct 22 2024 Reinhard Max <max@suse.com>
  - Update to release 3.47.0:
    * Allow arbitrary expressions in the second argument to the RAISE
      function.
    * If the RHS of the ->> operator is negative, then access array
      elements counting from the right.
    * Fix a problem with rolling back hot journal files in the
      seldom-used unix-dotfile VFS.
    * FTS5 tables can now be dropped even if they use a non-standard
      tokenizer that has not been registered.
    * Fix the group_concat() aggregate function so that it returns an
      empty string, not a NULL, if it receives a single input value
      which is an empty string.
    * Enhance the generate_series() table-valued function so that it
      is able to recognize and use constraints on its output value.
      Preupdate hooks now recognize when a column added by ALTER
      TABLE ADD COLUMN has a non-null default value.
    * Improved reuse of subqueries associated with the IN operator,
      especially when the IN operator has been duplicated due to
      predicate push-down.
    * Use a Bloom filter on subqueries on the right-hand side of the
      IN operator, in cases where that seems likely to improve
      performance.
    * Ensure that queries like "SELECT func(a) FROM tab GROUP BY 1"
      only invoke the func() function once per row.
    * No attempt is made to create automatic indexes on a column
      that is known to be non-selective because of its use in other
      indexes that have been analyzed.
    * Adjustments to the query planner so that it produces better
      plans for star queries with a large number of dimension
      tables.
    * Add the "order-by-subquery" optimization, that seeks to
      disable sort operations in outer queries if the desired order
      is obtained naturally due to ORDER BY clauses in subqueries.
    * The "indexed-subtype-expr" optimization strives to use
      expressions that are part of an index rather than recomputing
      the expression based on table values, as long as the query
      planner can prove that the subtype of the expression will
      never be used.
    * Miscellaneous coding tweaks for faster runtimes.
    * Add the experimental sqlite3_rsync program.
    * Add extension functions median(), percentile(),
      percentile_cont(), and percentile_disc() to the CLI.
    * Add the .www dot-command to the CLI.
    * The sqlite3_analyzer utility now provides a break-out of
      statistics for WITHOUT ROWID tables.
    * The sqldiff utility avoids creating an empty database if its
      second argument does not exist.
    * Enhance the sqlite_dbpage table-valued function such that
      INSERT can be used to increase or decrease the size of the
      database file.
    * SQLite no longer makes any use of the "long double" data type,
      as hardware support for long double is becoming less common
      and long double creates challenges for some compiler tool
      chains. Instead, SQLite uses Dekker's algorithm when extended
      precision is needed.
    * The TCL Interface for SQLite supports TCL9. Everything
      probably still works for TCL 8.5 and later, though this is not
      guaranteed. Users are encouraged to upgrade to TCL9.
    * Fix a corruption-causing bug in the JavaScript "opfs" VFS.
      Correct "mode=ro" handling for the "opfs" VFS.  Work around a
      couple of browser-specific OPFS quirks.
    * Add the fts5_tokenizer_v2 API and the locale=1 option, for
      creating custom locale-aware tokenizers and fts5 tables that
      may take advantage of them.
    * Add the contentless_unindexed=1 option, for creating
      contentless fts5 tables that store the values of any UNINDEXED
      columns persistently in the database.
    * Allow an FTS5 table to be dropped even if it uses a custom
      tokenizer whose implementation is not available.
* Tue Sep 24 2024 Reinhard Max <max@suse.com>
  - Update to release 3.46.1:
    * Improved robustness while parsing the tokenize= arguments in
      FTS5.
    * Enhancements to covering index prediction in the query planner.
    * Do not let the number of terms on a VALUES clause be limited by
      SQLITE_LIMIT_COMPOUND_SELECT, even if the VALUES clause
      contains elements that appear to be variables due to
      double-quoted string literals.
    * Fix the window function version of group_concat() so that it
      returns an empty string if it has one or more empty string
      inputs.
    * In FTS5 secure-delete mode, fix false-positive integrity-check
      reports about corrupt indexes.
    * Syntax errors in ALTER TABLE should always return SQLITE_ERROR.
      In some cases, they were formerly returning SQLITE_INTERNAL.
    * Other minor fixes.
* Thu May 23 2024 Reinhard Max <max@suse.com>
  - Update to release 3.46.0:
    * https://sqlite.org/releaselog/3_46_0.html
    * Enhance PRAGMA optimize in multiple ways.
    * Enhancements to the date and time functions.
    * Add support for underscore ("_") characters between digits in
      numeric literals.
    * Add the json_pretty() SQL function.
    * Query planner improvements.
    * Allocate additional memory from the heap for the SQL parser
      stack if that stack overflows, rather than reporting a "parser
      stack overflow" error.
    * Allow ASCII control characters within JSON5 string literals.
    * Fix the -> and ->> JSON operators so that when the right-hand
      side operand is a string that looks like an integer it is still
      treated as a string, because that is what PostgreSQL does.
    * Obsoletes sqlite3-float-i586.patch.
* Wed Apr 17 2024 Reinhard Max <max@suse.com>
  - Update to release 3.45.3:
    * Fix a long-standing bug (going back to version 3.24.0) that
      might (rarely) cause the "old.*" values of an UPDATE trigger
      to be incorrect if that trigger fires in response to an UPSERT.
    * Reduce the scope of the NOT NULL strength reduction
      optimization that was added as item 8e in version 3.35.0. The
      optimization was being attempted in some contexts where it did
      not work, resulting in incorrect query results.
  - Add SQLITE_STRICT_SUBTYPE=1 as recommended by upstream.
* Tue Mar 12 2024 Reinhard Max <max@suse.com>
  - Update to release 3.45.2:
    * Added the SQLITE_RESULT_SUBTYPE property for application-
      defined SQL functions.
    * Enhancements to the JSON SQL functions
    * Add the FTS5 tokendata option to the FTS5 virtual table.
    * The SQLITE_DIRECT_OVERFLOW_READ optimization is now enabled by
      default.
    * Query planner improvements
    * Increase the default value for SQLITE_MAX_PAGE_COUNT from
      1073741824 to 4294967294.
    * Enhancements to the CLI
    * Restore the JSON BLOB input bug, and promise to support the
      anomaly in subsequent releases, for backward compatibility.
    * Fix the PRAGMA integrity_check command so that it works on
      read-only databases that contain FTS3 and FTS5 tables.
    * Fix issues associated with processing corrupt JSONB inputs.
    * Fix a long-standing bug in which a read of a few bytes past the
      end of a memory-mapped segment might occur when accessing a
      craftily corrupted database using memory-mapped database.
    * Fix a long-standing bug in which a NULL pointer dereference
      might occur in the bytecode engine due to incorrect bytecode
      being generated for a class of SQL statements that are
      deliberately designed to stress the query planner but which
      are otherwise pointless.
    * Fix an error in UPSERT, introduced in version 3.35.0.
    * Reduce the scope of the NOT NULL strength reduction
      optimization that was added in version 3.35.0.
  - Add sqlite3-float-i586.patch to fix build on i586.
  - sqlite3-rtree-i686.patch is not needed anymore.
  - Abort build when %version and %tarversion don't match.
* Thu Dec 07 2023 Dominique Leuenberger <dimstar@opensuse.org>
  - Fix Version to advertise as 3.44.2, matching the tarball version.
* Mon Nov 27 2023 Reinhard Max <max@suse.com>
  - Update to release 3.44.2
    * Fix a mistake in the CLI that was introduced by the fix in
      3.44.1.
    * Fix a problem in FTS5 that was discovered during internal fuzz
      testing only minutes after the 3.44.1 release was tagged.
    * Fix incomplete assert() statements that the fuzzer discovered.
    * Fix a couple of harmless compiler warnings that appeared in
      debug builds with GCC 16.
* Wed Nov 22 2023 Reinhard Max <max@suse.com>
  - Update to release 3.44.1
    * Change the CLI so that it uses UTF-16 for console I/O on Windows.
    * Other obscure bug fixes.
* Fri Nov 03 2023 Jan Engelhardt <jengelh@inai.de>
  - Update to release 3.44.0
    * Aggregate functions can now include an ORDER BY clause after
      their last parameter. The arguments to the function are
      processed in the order specified. This can be important for
      functions like string_agg() and json_group_array().
    * Add support for the concat() and concat_ws() scalar SQL
      functions, compatible with PostgreSQL, SQLServer, and MySQL.
    * Add support for the string_agg() aggregate SQL function,
      compatible with PostgreSQL and SQLServer.
    * New conversion letters on the strftime() SQL function: %e %F
      %I %k %l %p %P %R %T %u
    * Add new C-language APIs: sqlite3_get_clientdata() and
      sqlite3_set_clientdata().
    * Many errors associated with CREATE TABLE are now raised when
      the CREATE TABLE statement itself is run, rather than being
      deferred until the first time the table is actually used.
* Thu Oct 19 2023 Reinhard Max <max@suse.com>
  - sqlite3-rtree-i686.patch: temporary build fix for 32-bit x86.
* Tue Oct 10 2023 Reinhard Max <max@suse.com>
  - Update to: 3.43.2:
    * Fix a couple of obscure UAF errors and an obscure memory leak.
    * Omit the use of the sprintf() function from the standard
      library in the CLI, as this now generates warnings on some
      platforms.
    * Avoid conversion of a double into unsigned long long integer,
      as some platforms do not do such conversions correctly.
* Mon Sep 11 2023 Reinhard Max <max@suse.com>
  - Update to: 3.43.1
    * Fix a regression in the way that the sum(), avg(), and total()
      aggregate functions handle infinities.
    * Fix a bug in the json_array_length() function that occurs when
      the argument comes directly from json_remove().
    * Fix the omit-unused-subquery-columns optimization (introduced
      in in version 3.42.0) so that it works correctly if the
      subquery is a compound where one arm is DISTINCT and the other
      is not.
* Sat Aug 26 2023 Andrea Manzini <andrea.manzini@suse.com>
  - Update to 3.43.0:
    * Add support for Contentless-Delete FTS5 Indexes. This is a
      variety of FTS5 full-text search index that omits storing the
      content that is being indexed while also allowing records to
      be deleted.
    * Enhancements to the date and time functions:
      + Added new time shift modifiers of the form
      ±YYYY-MM-DD HH:MM:SS.SSS.
      + Added the timediff() SQL function.
    * Added the octet_length(X) SQL function.
    * Added the sqlite3_stmt_explain() API.
    * Query planner enhancements:
      + Generalize the LEFT JOIN strength reduction optimization so
      that it works for RIGHT and FULL JOINs as well. Rename it to
      OUTER JOIN strength reduction.
      + Enhance the theorem prover in the OUTER JOIN strength
      reduction optimization so that it returns fewer
      false-negatives.
    * Enhancements to the decimal extension:
      + New function decimal_pow2(N) returns the N-th power of 2 for
      integer N between -20000 and +20000.
      + New function decimal_exp(X) works like decimal(X) except that
      it returns the result in exponential notation - with a "e+NN"
      at the end.
      + If X is a floating-point value, then the decimal(X) function
      now does a full expansion of that value into its exact
      decimal equivalent.
    * Performance enhancements to JSON processing results in a 2x
      performance improvement for some kinds of processing on large
      JSON strings.
    * The VFS for unix now assumes that the nanosleep() system call
      is available unless compiled with -DHAVE_NANOSLEEP=0.
* Wed May 17 2023 Reinhard Max <max@suse.com>
  - Update to 3.42.0:
    * Add the FTS5 secure-delete command. This option causes all
      forensic traces to be removed from the FTS5 inverted index when
      content is deleted.
    * Enhance the JSON SQL functions to support JSON5 extensions.
    * The SQLITE_CONFIG_LOG and SQLITE_CONFIG_PCACHE_HDRSZ calls to
      sqlite3_config() are now allowed to occur after
      sqlite3_initialize().
    * New sqlite3_db_config() options:
      SQLITE_DBCONFIG_STMT_SCANSTATUS and
      SQLITE_DBCONFIG_REVERSE_SCANORDER.
    * Query planner improvements.
    * Add the --unsafe-testing command-line option.
    * Allow commands ".log on" and ".log off", even in --safe mode.
    * "--" as a command-line argument means all subsequent arguments
      that start with "-" are interpreted as normal non-option
      argument.
    * Magic parameters ":inf" and ":nan" bind to floating point
      literals Infinity and NaN, respectively.
    * Add the ability for application-defined SQL functions to have
      the same name as join keywords: CROSS, FULL, INNER, LEFT,
      NATURAL, OUTER, or RIGHT.
    * Enhancements to PRAGMA integrity_check
    * Allow the session extension to be configured to capture changes
      from tables that lack an explicit ROWID.
    * Added the subsecond modifier to the date and time functions.
    * Negative values passed into sqlite3_sleep() are henceforth
      interpreted as 0.
    * The maximum recursion depth for JSON arrays and objects is
      lowered from 2000 to 1000.
    * Extended the built-in printf() function so the comma option now
      works with floating-point conversions in addition to integer
      conversions.
    * Miscellaneous bug fixes and performance optimizations.
* Wed Mar 22 2023 Reinhard Max <max@suse.com>
  - Update to 3.41.2:
    * Multiple fixes for reads past the end of memory buffers
    * Fix the sqlite3_error_offset() so that it does not return
      out-of-range values when reporting errors associated with
      generated columns.
    * Multiple fixes in the query query optimizer for problems that
      cause incorrect results for bizarre, fuzzer-generated queries.
    * Increase the size of the reference counter in the page cache
      object to 64 bits to ensure that the counter never overflows.
    * Fix a performance regression caused by a bug fix in patch
      release 3.41.1.
    * Fix a few incorrect assert() statements.
* Fri Mar 17 2023 Andreas Stieger <andreas.stieger@gmx.de>
  - Update to 3.41.1:
    * Ensure that the datatype for column t1.x in "CREATE TABLE t1 AS
      SELECT CAST(7 AS INT) AS x;" continues to be INT and is not
      NUM, for historical compatibility.
    * Enhance PRAGMA integrity_check to detect when extra bytes
      appear at the end of an index record.
    * Fix various obscure bugs reported by the user community
* Wed Mar 08 2023 Martin Pluskal <mpluskal@suse.com>
  - Build AVX2 enabled hwcaps library for x86_64-v3
* Wed Feb 22 2023 Reinhard Max <max@suse.com>
  - Update to 3.41.0:
    * https://www.sqlite.org/releaselog/3_41_0.html
    * Various query planner improvements.
    * Add the built-in unhex() SQL function.
    * Add the base64 and base85 application-defined functions as an
      extension and include that extension in the CLI.
    * In-memory databases created using sqlite3_deserialize() now
      report their filename as an empty string, not as 'x'.
    * The ".scanstats est" command provides query planner estimates
      in profiles.
    * Enhance the --safe command-line option to disallow dangerous
      SQL functions.
    * The double-quoted string misfeature is now disabled by default
      for CLI builds.
    * Various other improvements and performance enhancements.
  - The new version obsoletes sqlite-src-3390000-func7-pg-181.patch
```
/usr/share/doc/packages/sqlite3-doc
/usr/share/doc/packages/sqlite3-doc/34to35.html
/usr/share/doc/packages/sqlite3-doc/35to36.html
/usr/share/doc/packages/sqlite3-doc/about.html
/usr/share/doc/packages/sqlite3-doc/aff_short.html
/usr/share/doc/packages/sqlite3-doc/affcase1.html
/usr/share/doc/packages/sqlite3-doc/amalg-v-canon.html
/usr/share/doc/packages/sqlite3-doc/amalgamation.html
/usr/share/doc/packages/sqlite3-doc/appfileformat.html
/usr/share/doc/packages/sqlite3-doc/appfunc.html
/usr/share/doc/packages/sqlite3-doc/arch.html
/usr/share/doc/packages/sqlite3-doc/assert.html
/usr/share/doc/packages/sqlite3-doc/atomiccommit.html
/usr/share/doc/packages/sqlite3-doc/autoinc.html
/usr/share/doc/packages/sqlite3-doc/backup.html
/usr/share/doc/packages/sqlite3-doc/base64.html
/usr/share/doc/packages/sqlite3-doc/base85.html
/usr/share/doc/packages/sqlite3-doc/bindptr.html
/usr/share/doc/packages/sqlite3-doc/books.html
/usr/share/doc/packages/sqlite3-doc/bytecodevtab.html
/usr/share/doc/packages/sqlite3-doc/c3ref
/usr/share/doc/packages/sqlite3-doc/c3ref/aggregate_context.html
/usr/share/doc/packages/sqlite3-doc/c3ref/aggregate_count.html
/usr/share/doc/packages/sqlite3-doc/c3ref/api_routines.html
/usr/share/doc/packages/sqlite3-doc/c3ref/auto_extension.html
/usr/share/doc/packages/sqlite3-doc/c3ref/autovacuum_pages.html
/usr/share/doc/packages/sqlite3-doc/c3ref/backup.html
/usr/share/doc/packages/sqlite3-doc/c3ref/backup_finish.html
/usr/share/doc/packages/sqlite3-doc/c3ref/bind_blob.html
/usr/share/doc/packages/sqlite3-doc/c3ref/bind_parameter_count.html
/usr/share/doc/packages/sqlite3-doc/c3ref/bind_parameter_index.html
/usr/share/doc/packages/sqlite3-doc/c3ref/bind_parameter_name.html
/usr/share/doc/packages/sqlite3-doc/c3ref/blob.html
/usr/share/doc/packages/sqlite3-doc/c3ref/blob_bytes.html
/usr/share/doc/packages/sqlite3-doc/c3ref/blob_close.html
/usr/share/doc/packages/sqlite3-doc/c3ref/blob_open.html
/usr/share/doc/packages/sqlite3-doc/c3ref/blob_read.html
/usr/share/doc/packages/sqlite3-doc/c3ref/blob_reopen.html
/usr/share/doc/packages/sqlite3-doc/c3ref/blob_write.html
/usr/share/doc/packages/sqlite3-doc/c3ref/busy_handler.html
/usr/share/doc/packages/sqlite3-doc/c3ref/busy_timeout.html
/usr/share/doc/packages/sqlite3-doc/c3ref/c_abort.html
/usr/share/doc/packages/sqlite3-doc/c3ref/c_abort_rollback.html
/usr/share/doc/packages/sqlite3-doc/c3ref/c_access_exists.html
/usr/share/doc/packages/sqlite3-doc/c3ref/c_alter_table.html
/usr/share/doc/packages/sqlite3-doc/c3ref/c_any.html
/usr/share/doc/packages/sqlite3-doc/c3ref/c_blob.html
/usr/share/doc/packages/sqlite3-doc/c3ref/c_carray_blob.html
/usr/share/doc/packages/sqlite3-doc/c3ref/c_checkpoint_full.html
/usr/share/doc/packages/sqlite3-doc/c3ref/c_config_covering_index_scan.html
/usr/share/doc/packages/sqlite3-doc/c3ref/c_copy.html
/usr/share/doc/packages/sqlite3-doc/c3ref/c_dbconfig_defensive.html
/usr/share/doc/packages/sqlite3-doc/c3ref/c_dbstatus_options.html
/usr/share/doc/packages/sqlite3-doc/c3ref/c_deny.html
/usr/share/doc/packages/sqlite3-doc/c3ref/c_deserialize_freeonclose.html
/usr/share/doc/packages/sqlite3-doc/c3ref/c_deterministic.html
/usr/share/doc/packages/sqlite3-doc/c3ref/c_fail.html
/usr/share/doc/packages/sqlite3-doc/c3ref/c_fcntl_begin_atomic_write.html
/usr/share/doc/packages/sqlite3-doc/c3ref/c_index_constraint_eq.html
/usr/share/doc/packages/sqlite3-doc/c3ref/c_index_scan_hex.html
/usr/share/doc/packages/sqlite3-doc/c3ref/c_iocap_atomic.html
/usr/share/doc/packages/sqlite3-doc/c3ref/c_limit_attached.html
/usr/share/doc/packages/sqlite3-doc/c3ref/c_lock_exclusive.html
/usr/share/doc/packages/sqlite3-doc/c3ref/c_mutex_fast.html
/usr/share/doc/packages/sqlite3-doc/c3ref/c_open_autoproxy.html
/usr/share/doc/packages/sqlite3-doc/c3ref/c_prepare_dont_log.html
/usr/share/doc/packages/sqlite3-doc/c3ref/c_scanstat_complex.html
/usr/share/doc/packages/sqlite3-doc/c3ref/c_scanstat_est.html
/usr/share/doc/packages/sqlite3-doc/c3ref/c_scm_branch.html
/usr/share/doc/packages/sqlite3-doc/c3ref/c_serialize_nocopy.html
/usr/share/doc/packages/sqlite3-doc/c3ref/c_setlk_block_on_connect.html
/usr/share/doc/packages/sqlite3-doc/c3ref/c_shm_exclusive.html
/usr/share/doc/packages/sqlite3-doc/c3ref/c_shm_nlock.html
/usr/share/doc/packages/sqlite3-doc/c3ref/c_static.html
/usr/share/doc/packages/sqlite3-doc/c3ref/c_status_malloc_count.html
/usr/share/doc/packages/sqlite3-doc/c3ref/c_stmtstatus_counter.html
/usr/share/doc/packages/sqlite3-doc/c3ref/c_sync_dataonly.html
/usr/share/doc/packages/sqlite3-doc/c3ref/c_testctrl_always.html
/usr/share/doc/packages/sqlite3-doc/c3ref/c_trace.html
/usr/share/doc/packages/sqlite3-doc/c3ref/c_txn_none.html
/usr/share/doc/packages/sqlite3-doc/c3ref/c_vtab_constraint_support.html
/usr/share/doc/packages/sqlite3-doc/c3ref/c_win32_data_directory_type.html
/usr/share/doc/packages/sqlite3-doc/c3ref/cancel_auto_extension.html
/usr/share/doc/packages/sqlite3-doc/c3ref/carray_bind.html
/usr/share/doc/packages/sqlite3-doc/c3ref/changes.html
/usr/share/doc/packages/sqlite3-doc/c3ref/clear_bindings.html
/usr/share/doc/packages/sqlite3-doc/c3ref/close.html
/usr/share/doc/packages/sqlite3-doc/c3ref/collation_needed.html
/usr/share/doc/packages/sqlite3-doc/c3ref/column_blob.html
/usr/share/doc/packages/sqlite3-doc/c3ref/column_count.html
/usr/share/doc/packages/sqlite3-doc/c3ref/column_database_name.html
/usr/share/doc/packages/sqlite3-doc/c3ref/column_decltype.html
/usr/share/doc/packages/sqlite3-doc/c3ref/column_name.html
/usr/share/doc/packages/sqlite3-doc/c3ref/commit_hook.html
/usr/share/doc/packages/sqlite3-doc/c3ref/compileoption_get.html
/usr/share/doc/packages/sqlite3-doc/c3ref/complete.html
/usr/share/doc/packages/sqlite3-doc/c3ref/config.html
/usr/share/doc/packages/sqlite3-doc/c3ref/constlist.html
/usr/share/doc/packages/sqlite3-doc/c3ref/context.html
/usr/share/doc/packages/sqlite3-doc/c3ref/context_db_handle.html
/usr/share/doc/packages/sqlite3-doc/c3ref/create_collation.html
/usr/share/doc/packages/sqlite3-doc/c3ref/create_filename.html
/usr/share/doc/packages/sqlite3-doc/c3ref/create_function.html
/usr/share/doc/packages/sqlite3-doc/c3ref/create_module.html
/usr/share/doc/packages/sqlite3-doc/c3ref/data_count.html
/usr/share/doc/packages/sqlite3-doc/c3ref/data_directory.html
/usr/share/doc/packages/sqlite3-doc/c3ref/database_file_object.html
/usr/share/doc/packages/sqlite3-doc/c3ref/db_cacheflush.html
/usr/share/doc/packages/sqlite3-doc/c3ref/db_config.html
/usr/share/doc/packages/sqlite3-doc/c3ref/db_filename.html
/usr/share/doc/packages/sqlite3-doc/c3ref/db_handle.html
/usr/share/doc/packages/sqlite3-doc/c3ref/db_mutex.html
/usr/share/doc/packages/sqlite3-doc/c3ref/db_name.html
/usr/share/doc/packages/sqlite3-doc/c3ref/db_readonly.html
/usr/share/doc/packages/sqlite3-doc/c3ref/db_release_memory.html
/usr/share/doc/packages/sqlite3-doc/c3ref/db_status.html
/usr/share/doc/packages/sqlite3-doc/c3ref/declare_vtab.html
/usr/share/doc/packages/sqlite3-doc/c3ref/deserialize.html
/usr/share/doc/packages/sqlite3-doc/c3ref/drop_modules.html
/usr/share/doc/packages/sqlite3-doc/c3ref/enable_load_extension.html
/usr/share/doc/packages/sqlite3-doc/c3ref/enable_shared_cache.html
/usr/share/doc/packages/sqlite3-doc/c3ref/errcode.html
/usr/share/doc/packages/sqlite3-doc/c3ref/exec.html
/usr/share/doc/packages/sqlite3-doc/c3ref/expanded_sql.html
/usr/share/doc/packages/sqlite3-doc/c3ref/experimental.html
/usr/share/doc/packages/sqlite3-doc/c3ref/extended_result_codes.html
/usr/share/doc/packages/sqlite3-doc/c3ref/file.html
/usr/share/doc/packages/sqlite3-doc/c3ref/file_control.html
/usr/share/doc/packages/sqlite3-doc/c3ref/filename.html
/usr/share/doc/packages/sqlite3-doc/c3ref/filename_database.html
/usr/share/doc/packages/sqlite3-doc/c3ref/finalize.html
/usr/share/doc/packages/sqlite3-doc/c3ref/free.html
/usr/share/doc/packages/sqlite3-doc/c3ref/free_table.html
/usr/share/doc/packages/sqlite3-doc/c3ref/funclist.html
/usr/share/doc/packages/sqlite3-doc/c3ref/get_autocommit.html
/usr/share/doc/packages/sqlite3-doc/c3ref/get_auxdata.html
/usr/share/doc/packages/sqlite3-doc/c3ref/get_clientdata.html
/usr/share/doc/packages/sqlite3-doc/c3ref/hard_heap_limit64.html
/usr/share/doc/packages/sqlite3-doc/c3ref/index_info.html
/usr/share/doc/packages/sqlite3-doc/c3ref/initialize.html
/usr/share/doc/packages/sqlite3-doc/c3ref/int64.html
/usr/share/doc/packages/sqlite3-doc/c3ref/interrupt.html
/usr/share/doc/packages/sqlite3-doc/c3ref/intro.html
/usr/share/doc/packages/sqlite3-doc/c3ref/io_methods.html
/usr/share/doc/packages/sqlite3-doc/c3ref/keyword_check.html
/usr/share/doc/packages/sqlite3-doc/c3ref/last_insert_rowid.html
/usr/share/doc/packages/sqlite3-doc/c3ref/libversion.html
/usr/share/doc/packages/sqlite3-doc/c3ref/limit.html
/usr/share/doc/packages/sqlite3-doc/c3ref/load_extension.html
/usr/share/doc/packages/sqlite3-doc/c3ref/log.html
/usr/share/doc/packages/sqlite3-doc/c3ref/mem_methods.html
/usr/share/doc/packages/sqlite3-doc/c3ref/memory_highwater.html
/usr/share/doc/packages/sqlite3-doc/c3ref/module.html
/usr/share/doc/packages/sqlite3-doc/c3ref/mprintf.html
/usr/share/doc/packages/sqlite3-doc/c3ref/mutex.html
/usr/share/doc/packages/sqlite3-doc/c3ref/mutex_alloc.html
/usr/share/doc/packages/sqlite3-doc/c3ref/mutex_held.html
/usr/share/doc/packages/sqlite3-doc/c3ref/mutex_methods.html
/usr/share/doc/packages/sqlite3-doc/c3ref/next_stmt.html
/usr/share/doc/packages/sqlite3-doc/c3ref/objlist.html
/usr/share/doc/packages/sqlite3-doc/c3ref/open.html
/usr/share/doc/packages/sqlite3-doc/c3ref/overload_function.html
/usr/share/doc/packages/sqlite3-doc/c3ref/pcache.html
/usr/share/doc/packages/sqlite3-doc/c3ref/pcache_methods2.html
/usr/share/doc/packages/sqlite3-doc/c3ref/pcache_page.html
/usr/share/doc/packages/sqlite3-doc/c3ref/prepare.html
/usr/share/doc/packages/sqlite3-doc/c3ref/preupdate_blobwrite.html
/usr/share/doc/packages/sqlite3-doc/c3ref/profile.html
/usr/share/doc/packages/sqlite3-doc/c3ref/progress_handler.html
/usr/share/doc/packages/sqlite3-doc/c3ref/randomness.html
/usr/share/doc/packages/sqlite3-doc/c3ref/release_memory.html
/usr/share/doc/packages/sqlite3-doc/c3ref/reset.html
/usr/share/doc/packages/sqlite3-doc/c3ref/reset_auto_extension.html
/usr/share/doc/packages/sqlite3-doc/c3ref/result_blob.html
/usr/share/doc/packages/sqlite3-doc/c3ref/result_str.html
/usr/share/doc/packages/sqlite3-doc/c3ref/result_subtype.html
/usr/share/doc/packages/sqlite3-doc/c3ref/serialize.html
/usr/share/doc/packages/sqlite3-doc/c3ref/set_authorizer.html
/usr/share/doc/packages/sqlite3-doc/c3ref/set_errmsg.html
/usr/share/doc/packages/sqlite3-doc/c3ref/set_last_insert_rowid.html
/usr/share/doc/packages/sqlite3-doc/c3ref/setlk_timeout.html
/usr/share/doc/packages/sqlite3-doc/c3ref/sleep.html
/usr/share/doc/packages/sqlite3-doc/c3ref/snapshot.html
/usr/share/doc/packages/sqlite3-doc/c3ref/snapshot_cmp.html
/usr/share/doc/packages/sqlite3-doc/c3ref/snapshot_free.html
/usr/share/doc/packages/sqlite3-doc/c3ref/snapshot_get.html
/usr/share/doc/packages/sqlite3-doc/c3ref/snapshot_open.html
/usr/share/doc/packages/sqlite3-doc/c3ref/snapshot_recover.html
/usr/share/doc/packages/sqlite3-doc/c3ref/soft_heap_limit.html
/usr/share/doc/packages/sqlite3-doc/c3ref/sqlite3.html
/usr/share/doc/packages/sqlite3-doc/c3ref/status.html
/usr/share/doc/packages/sqlite3-doc/c3ref/step.html
/usr/share/doc/packages/sqlite3-doc/c3ref/stmt.html
/usr/share/doc/packages/sqlite3-doc/c3ref/stmt_busy.html
/usr/share/doc/packages/sqlite3-doc/c3ref/stmt_explain.html
/usr/share/doc/packages/sqlite3-doc/c3ref/stmt_isexplain.html
/usr/share/doc/packages/sqlite3-doc/c3ref/stmt_readonly.html
/usr/share/doc/packages/sqlite3-doc/c3ref/stmt_scanstatus.html
/usr/share/doc/packages/sqlite3-doc/c3ref/stmt_scanstatus_reset.html
/usr/share/doc/packages/sqlite3-doc/c3ref/stmt_status.html
/usr/share/doc/packages/sqlite3-doc/c3ref/str.html
/usr/share/doc/packages/sqlite3-doc/c3ref/str_append.html
/usr/share/doc/packages/sqlite3-doc/c3ref/str_errcode.html
/usr/share/doc/packages/sqlite3-doc/c3ref/str_finish.html
/usr/share/doc/packages/sqlite3-doc/c3ref/str_new.html
/usr/share/doc/packages/sqlite3-doc/c3ref/strglob.html
/usr/share/doc/packages/sqlite3-doc/c3ref/stricmp.html
/usr/share/doc/packages/sqlite3-doc/c3ref/strlike.html
/usr/share/doc/packages/sqlite3-doc/c3ref/system_errno.html
/usr/share/doc/packages/sqlite3-doc/c3ref/table_column_metadata.html
/usr/share/doc/packages/sqlite3-doc/c3ref/temp_directory.html
/usr/share/doc/packages/sqlite3-doc/c3ref/test_control.html
/usr/share/doc/packages/sqlite3-doc/c3ref/threadsafe.html
/usr/share/doc/packages/sqlite3-doc/c3ref/total_changes.html
/usr/share/doc/packages/sqlite3-doc/c3ref/trace_v2.html
/usr/share/doc/packages/sqlite3-doc/c3ref/txn_state.html
/usr/share/doc/packages/sqlite3-doc/c3ref/unlock_notify.html
/usr/share/doc/packages/sqlite3-doc/c3ref/update_hook.html
/usr/share/doc/packages/sqlite3-doc/c3ref/uri_boolean.html
/usr/share/doc/packages/sqlite3-doc/c3ref/user_data.html
/usr/share/doc/packages/sqlite3-doc/c3ref/value.html
/usr/share/doc/packages/sqlite3-doc/c3ref/value_blob.html
/usr/share/doc/packages/sqlite3-doc/c3ref/value_dup.html
/usr/share/doc/packages/sqlite3-doc/c3ref/value_encoding.html
/usr/share/doc/packages/sqlite3-doc/c3ref/value_subtype.html
/usr/share/doc/packages/sqlite3-doc/c3ref/vfs.html
/usr/share/doc/packages/sqlite3-doc/c3ref/vfs_find.html
/usr/share/doc/packages/sqlite3-doc/c3ref/vtab.html
/usr/share/doc/packages/sqlite3-doc/c3ref/vtab_collation.html
/usr/share/doc/packages/sqlite3-doc/c3ref/vtab_config.html
/usr/share/doc/packages/sqlite3-doc/c3ref/vtab_cursor.html
/usr/share/doc/packages/sqlite3-doc/c3ref/vtab_distinct.html
/usr/share/doc/packages/sqlite3-doc/c3ref/vtab_in.html
/usr/share/doc/packages/sqlite3-doc/c3ref/vtab_in_first.html
/usr/share/doc/packages/sqlite3-doc/c3ref/vtab_nochange.html
/usr/share/doc/packages/sqlite3-doc/c3ref/vtab_on_conflict.html
/usr/share/doc/packages/sqlite3-doc/c3ref/vtab_rhs_value.html
/usr/share/doc/packages/sqlite3-doc/c3ref/wal_autocheckpoint.html
/usr/share/doc/packages/sqlite3-doc/c3ref/wal_checkpoint.html
/usr/share/doc/packages/sqlite3-doc/c3ref/wal_checkpoint_v2.html
/usr/share/doc/packages/sqlite3-doc/c3ref/wal_hook.html
/usr/share/doc/packages/sqlite3-doc/c3ref/win32_set_directory.html
/usr/share/doc/packages/sqlite3-doc/c_interface.html
/usr/share/doc/packages/sqlite3-doc/capi3.html
/usr/share/doc/packages/sqlite3-doc/capi3ref.html
/usr/share/doc/packages/sqlite3-doc/carray.html
/usr/share/doc/packages/sqlite3-doc/changes.html
/usr/share/doc/packages/sqlite3-doc/chronology.html
/usr/share/doc/packages/sqlite3-doc/cintro.html
/usr/share/doc/packages/sqlite3-doc/cksumvfs.html
/usr/share/doc/packages/sqlite3-doc/cli.html
/usr/share/doc/packages/sqlite3-doc/climode.html
/usr/share/doc/packages/sqlite3-doc/cliprompt.html
/usr/share/doc/packages/sqlite3-doc/codeofconduct.html
/usr/share/doc/packages/sqlite3-doc/codeofethics.html
/usr/share/doc/packages/sqlite3-doc/compile.html
/usr/share/doc/packages/sqlite3-doc/completion.html
/usr/share/doc/packages/sqlite3-doc/conflict.html
/usr/share/doc/packages/sqlite3-doc/consortium.html
/usr/share/doc/packages/sqlite3-doc/consortium_agreement-20071201.html
/usr/share/doc/packages/sqlite3-doc/copyright-release.html
/usr/share/doc/packages/sqlite3-doc/copyright-release.pdf
/usr/share/doc/packages/sqlite3-doc/copyright.html
/usr/share/doc/packages/sqlite3-doc/cpu.html
/usr/share/doc/packages/sqlite3-doc/crew.html
/usr/share/doc/packages/sqlite3-doc/csv.html
/usr/share/doc/packages/sqlite3-doc/custombuild.html
/usr/share/doc/packages/sqlite3-doc/cves.html
/usr/share/doc/packages/sqlite3-doc/cvstrac.css
/usr/share/doc/packages/sqlite3-doc/datatype3.html
/usr/share/doc/packages/sqlite3-doc/datatypes.html
/usr/share/doc/packages/sqlite3-doc/dbhash.html
/usr/share/doc/packages/sqlite3-doc/dbpage.html
/usr/share/doc/packages/sqlite3-doc/dbstat.html
/usr/share/doc/packages/sqlite3-doc/debugging.html
/usr/share/doc/packages/sqlite3-doc/deterministic.html
/usr/share/doc/packages/sqlite3-doc/different.html
/usr/share/doc/packages/sqlite3-doc/diskusedfunc.html
/usr/share/doc/packages/sqlite3-doc/doc_backlink_crossref.html
/usr/share/doc/packages/sqlite3-doc/doc_keyword_crossref.html
/usr/share/doc/packages/sqlite3-doc/doc_pagelink_crossref.html
/usr/share/doc/packages/sqlite3-doc/doc_target_crossref.html
/usr/share/doc/packages/sqlite3-doc/doclist.html
/usr/share/doc/packages/sqlite3-doc/docs.html
/usr/share/doc/packages/sqlite3-doc/download.html
/usr/share/doc/packages/sqlite3-doc/eqp.html
/usr/share/doc/packages/sqlite3-doc/errlog.html
/usr/share/doc/packages/sqlite3-doc/expridx.html
/usr/share/doc/packages/sqlite3-doc/famous.html
/usr/share/doc/packages/sqlite3-doc/faq.html
/usr/share/doc/packages/sqlite3-doc/fasterthanfs.html
/usr/share/doc/packages/sqlite3-doc/favicon.ico
/usr/share/doc/packages/sqlite3-doc/features.html
/usr/share/doc/packages/sqlite3-doc/fileformat.html
/usr/share/doc/packages/sqlite3-doc/fileformat2.html
/usr/share/doc/packages/sqlite3-doc/flextypegood.html
/usr/share/doc/packages/sqlite3-doc/floatingpoint.html
/usr/share/doc/packages/sqlite3-doc/footprint.html
/usr/share/doc/packages/sqlite3-doc/foreignkeys.html
/usr/share/doc/packages/sqlite3-doc/formatchng.html
/usr/share/doc/packages/sqlite3-doc/fts3.html
/usr/share/doc/packages/sqlite3-doc/fts5.html
/usr/share/doc/packages/sqlite3-doc/fullsql.html
/usr/share/doc/packages/sqlite3-doc/gencol.html
/usr/share/doc/packages/sqlite3-doc/geopoly.html
/usr/share/doc/packages/sqlite3-doc/getthecode.html
/usr/share/doc/packages/sqlite3-doc/glossary.html
/usr/share/doc/packages/sqlite3-doc/hirely.html
/usr/share/doc/packages/sqlite3-doc/howitworks.html
/usr/share/doc/packages/sqlite3-doc/howtocompile.html
/usr/share/doc/packages/sqlite3-doc/howtocorrupt.html
/usr/share/doc/packages/sqlite3-doc/hp1.html
/usr/share/doc/packages/sqlite3-doc/images
/usr/share/doc/packages/sqlite3-doc/images/2005osaward.gif
/usr/share/doc/packages/sqlite3-doc/images/SQLite.gif
/usr/share/doc/packages/sqlite3-doc/images/SQLite_big.gif
/usr/share/doc/packages/sqlite3-doc/images/ac
/usr/share/doc/packages/sqlite3-doc/images/ac/commit-0.gif
/usr/share/doc/packages/sqlite3-doc/images/ac/commit-1.gif
/usr/share/doc/packages/sqlite3-doc/images/ac/commit-2.gif
/usr/share/doc/packages/sqlite3-doc/images/ac/commit-3.gif
/usr/share/doc/packages/sqlite3-doc/images/ac/commit-4.gif
/usr/share/doc/packages/sqlite3-doc/images/ac/commit-5.gif
/usr/share/doc/packages/sqlite3-doc/images/ac/commit-6.gif
/usr/share/doc/packages/sqlite3-doc/images/ac/commit-7.gif
/usr/share/doc/packages/sqlite3-doc/images/ac/commit-8.gif
/usr/share/doc/packages/sqlite3-doc/images/ac/commit-9.gif
/usr/share/doc/packages/sqlite3-doc/images/ac/commit-A.gif
/usr/share/doc/packages/sqlite3-doc/images/ac/commit-B.gif
/usr/share/doc/packages/sqlite3-doc/images/ac/multi-0.gif
/usr/share/doc/packages/sqlite3-doc/images/ac/multi-1.gif
/usr/share/doc/packages/sqlite3-doc/images/ac/multi-2.gif
/usr/share/doc/packages/sqlite3-doc/images/ac/multi-3.gif
/usr/share/doc/packages/sqlite3-doc/images/ac/multi-4.gif
/usr/share/doc/packages/sqlite3-doc/images/ac/multi-5.gif
/usr/share/doc/packages/sqlite3-doc/images/ac/rollback-0.gif
/usr/share/doc/packages/sqlite3-doc/images/ac/rollback-1.gif
/usr/share/doc/packages/sqlite3-doc/images/ac/rollback-2.gif
/usr/share/doc/packages/sqlite3-doc/images/ac/rollback-3.gif
/usr/share/doc/packages/sqlite3-doc/images/ac/rollback-4.gif
/usr/share/doc/packages/sqlite3-doc/images/ac/rollback-5.gif
/usr/share/doc/packages/sqlite3-doc/images/apple-touch-icon.png
/usr/share/doc/packages/sqlite3-doc/images/books
/usr/share/doc/packages/sqlite3-doc/images/books/aditya.jpg
/usr/share/doc/packages/sqlite3-doc/images/books/darocha2016.jpg
/usr/share/doc/packages/sqlite3-doc/images/books/das.jpg
/usr/share/doc/packages/sqlite3-doc/images/books/droessler.jpg
/usr/share/doc/packages/sqlite3-doc/images/books/haldar.gif
/usr/share/doc/packages/sqlite3-doc/images/books/haldar2.jpg
/usr/share/doc/packages/sqlite3-doc/images/books/kreibich.gif
/usr/share/doc/packages/sqlite3-doc/images/books/newman.jpg
/usr/share/doc/packages/sqlite3-doc/images/books/nishizawa2.jpg
/usr/share/doc/packages/sqlite3-doc/images/books/owens.jpg
/usr/share/doc/packages/sqlite3-doc/images/books/sanderson2018.jpg
/usr/share/doc/packages/sqlite3-doc/images/books/symbiansql.jpg
/usr/share/doc/packages/sqlite3-doc/images/books/vanderLans.jpg
/usr/share/doc/packages/sqlite3-doc/images/btreemodule_balance_deeper.svg
/usr/share/doc/packages/sqlite3-doc/images/btreemodule_balance_quick.svg
/usr/share/doc/packages/sqlite3-doc/images/btreemodule_balance_shallower.svg
/usr/share/doc/packages/sqlite3-doc/images/btreemodule_delete1.svg
/usr/share/doc/packages/sqlite3-doc/images/btreemodule_overview.svg
/usr/share/doc/packages/sqlite3-doc/images/chart.wiki
/usr/share/doc/packages/sqlite3-doc/images/chw.jpg
/usr/share/doc/packages/sqlite3-doc/images/cpuusage.png
/usr/share/doc/packages/sqlite3-doc/images/direct1b.gif
/usr/share/doc/packages/sqlite3-doc/images/faster-read-blobapi.jpg
/usr/share/doc/packages/sqlite3-doc/images/faster-read-mmap.jpg
/usr/share/doc/packages/sqlite3-doc/images/faster-read-sql.jpg
/usr/share/doc/packages/sqlite3-doc/images/faster-write-safe.jpg
/usr/share/doc/packages/sqlite3-doc/images/faster-write-unsafe.jpg
/usr/share/doc/packages/sqlite3-doc/images/fileformat
/usr/share/doc/packages/sqlite3-doc/images/fileformat/db_connection.gif
/usr/share/doc/packages/sqlite3-doc/images/fileformat/examplepop.gif
/usr/share/doc/packages/sqlite3-doc/images/fileformat/fileio_diagrams.odg
/usr/share/doc/packages/sqlite3-doc/images/fileformat/filesystem1.gif
/usr/share/doc/packages/sqlite3-doc/images/fileformat/filesystem2.gif
/usr/share/doc/packages/sqlite3-doc/images/fileformat/freelistpage.gif
/usr/share/doc/packages/sqlite3-doc/images/fileformat/indexlongrecord.gif
/usr/share/doc/packages/sqlite3-doc/images/fileformat/indexpage.gif
/usr/share/doc/packages/sqlite3-doc/images/fileformat/indexpage.odg
/usr/share/doc/packages/sqlite3-doc/images/fileformat/indexshortrecord.gif
/usr/share/doc/packages/sqlite3-doc/images/fileformat/indextree.gif
/usr/share/doc/packages/sqlite3-doc/images/fileformat/journal_format.gif
/usr/share/doc/packages/sqlite3-doc/images/fileformat/journal_header.gif
/usr/share/doc/packages/sqlite3-doc/images/fileformat/journal_record.gif
/usr/share/doc/packages/sqlite3-doc/images/fileformat/master_journal_ptr.gif
/usr/share/doc/packages/sqlite3-doc/images/fileformat/overflowpage.gif
/usr/share/doc/packages/sqlite3-doc/images/fileformat/pointermapentry.gif
/usr/share/doc/packages/sqlite3-doc/images/fileformat/recordformat.gif
/usr/share/doc/packages/sqlite3-doc/images/fileformat/rtdocs.css
/usr/share/doc/packages/sqlite3-doc/images/fileformat/rtdocs.js
/usr/share/doc/packages/sqlite3-doc/images/fileformat/tablelongrecord.gif
/usr/share/doc/packages/sqlite3-doc/images/fileformat/tablenodecell.gif
/usr/share/doc/packages/sqlite3-doc/images/fileformat/tableshortrecord.gif
/usr/share/doc/packages/sqlite3-doc/images/fileformat/tabletree.gif
/usr/share/doc/packages/sqlite3-doc/images/fileformat/vfs_role.gif
/usr/share/doc/packages/sqlite3-doc/images/fileformat/write_transaction.gif
/usr/share/doc/packages/sqlite3-doc/images/foreignlogos
/usr/share/doc/packages/sqlite3-doc/images/foreignlogos/adobe-logo.gif
/usr/share/doc/packages/sqlite3-doc/images/foreignlogos/adobe.gif
/usr/share/doc/packages/sqlite3-doc/images/foreignlogos/airbus1.gif
/usr/share/doc/packages/sqlite3-doc/images/foreignlogos/airbus2.gif
/usr/share/doc/packages/sqlite3-doc/images/foreignlogos/apple.gif
/usr/share/doc/packages/sqlite3-doc/images/foreignlogos/bentley.gif
/usr/share/doc/packages/sqlite3-doc/images/foreignlogos/bentley.jpg
/usr/share/doc/packages/sqlite3-doc/images/foreignlogos/bloomberg.png
/usr/share/doc/packages/sqlite3-doc/images/foreignlogos/bosch_logo1.gif
/usr/share/doc/packages/sqlite3-doc/images/foreignlogos/dropbox.gif
/usr/share/doc/packages/sqlite3-doc/images/foreignlogos/expensify.png
/usr/share/doc/packages/sqlite3-doc/images/foreignlogos/fb.gif
/usr/share/doc/packages/sqlite3-doc/images/foreignlogos/firefox.gif
/usr/share/doc/packages/sqlite3-doc/images/foreignlogos/ge.gif
/usr/share/doc/packages/sqlite3-doc/images/foreignlogos/google.gif
/usr/share/doc/packages/sqlite3-doc/images/foreignlogos/intuit.gif
/usr/share/doc/packages/sqlite3-doc/images/foreignlogos/loc.gif
/usr/share/doc/packages/sqlite3-doc/images/foreignlogos/loc.jpg
/usr/share/doc/packages/sqlite3-doc/images/foreignlogos/mcaffee.gif
/usr/share/doc/packages/sqlite3-doc/images/foreignlogos/microsoft.gif
/usr/share/doc/packages/sqlite3-doc/images/foreignlogos/monotone.gif
/usr/share/doc/packages/sqlite3-doc/images/foreignlogos/mozilla.gif
/usr/share/doc/packages/sqlite3-doc/images/foreignlogos/nds.png
/usr/share/doc/packages/sqlite3-doc/images/foreignlogos/nokia.gif
/usr/share/doc/packages/sqlite3-doc/images/foreignlogos/oracle.gif
/usr/share/doc/packages/sqlite3-doc/images/foreignlogos/philips.gif
/usr/share/doc/packages/sqlite3-doc/images/foreignlogos/php.gif
/usr/share/doc/packages/sqlite3-doc/images/foreignlogos/python.gif
/usr/share/doc/packages/sqlite3-doc/images/foreignlogos/reportr.png
/usr/share/doc/packages/sqlite3-doc/images/foreignlogos/rpm_logo.png
/usr/share/doc/packages/sqlite3-doc/images/foreignlogos/skype.gif
/usr/share/doc/packages/sqlite3-doc/images/foreignlogos/sony.gif
/usr/share/doc/packages/sqlite3-doc/images/foreignlogos/sunmicro.gif
/usr/share/doc/packages/sqlite3-doc/images/foreignlogos/symbian.gif
/usr/share/doc/packages/sqlite3-doc/images/foreignlogos/tcl.gif
/usr/share/doc/packages/sqlite3-doc/images/foreignlogos/xojo.png
/usr/share/doc/packages/sqlite3-doc/images/fts3_doclist.png
/usr/share/doc/packages/sqlite3-doc/images/fts3_doclist.svg
/usr/share/doc/packages/sqlite3-doc/images/fts3_doclist2.png
/usr/share/doc/packages/sqlite3-doc/images/fts3_doclist2.svg
/usr/share/doc/packages/sqlite3-doc/images/fts3_interior_node.png
/usr/share/doc/packages/sqlite3-doc/images/fts3_interior_node.svg
/usr/share/doc/packages/sqlite3-doc/images/fts3_leaf_node.png
/usr/share/doc/packages/sqlite3-doc/images/fts3_leaf_node.svg
/usr/share/doc/packages/sqlite3-doc/images/fts5_formula.odf
/usr/share/doc/packages/sqlite3-doc/images/fts5_formula1.png
/usr/share/doc/packages/sqlite3-doc/images/fts5_formula2.png
/usr/share/doc/packages/sqlite3-doc/images/fts5_formula3.png
/usr/share/doc/packages/sqlite3-doc/images/fullscanb.gif
/usr/share/doc/packages/sqlite3-doc/images/harmony.gif
/usr/share/doc/packages/sqlite3-doc/images/index-ex1-x-b.gif
/usr/share/doc/packages/sqlite3-doc/images/indirect1b1.gif
/usr/share/doc/packages/sqlite3-doc/images/loc.jpg
/usr/share/doc/packages/sqlite3-doc/images/ne.gif
/usr/share/doc/packages/sqlite3-doc/images/ne.png
/usr/share/doc/packages/sqlite3-doc/images/nocopy.gif
/usr/share/doc/packages/sqlite3-doc/images/nw.gif
/usr/share/doc/packages/sqlite3-doc/images/nw.png
/usr/share/doc/packages/sqlite3-doc/images/powered_by_sqlite.gif
/usr/share/doc/packages/sqlite3-doc/images/private_branch.gif
/usr/share/doc/packages/sqlite3-doc/images/qp
/usr/share/doc/packages/sqlite3-doc/images/qp/fqp1.gif
/usr/share/doc/packages/sqlite3-doc/images/qp/fqp1.pikchr
/usr/share/doc/packages/sqlite3-doc/images/qp/fruitobstate.gif
/usr/share/doc/packages/sqlite3-doc/images/qp/fruitobstate0.gif
/usr/share/doc/packages/sqlite3-doc/images/qp/fullscan.gif
/usr/share/doc/packages/sqlite3-doc/images/qp/idx1.gif
/usr/share/doc/packages/sqlite3-doc/images/qp/idx1lu1.gif
/usr/share/doc/packages/sqlite3-doc/images/qp/idx1lu2.gif
/usr/share/doc/packages/sqlite3-doc/images/qp/idx1lu3.gif
/usr/share/doc/packages/sqlite3-doc/images/qp/idx2.gif
/usr/share/doc/packages/sqlite3-doc/images/qp/idx2lu1.gif
/usr/share/doc/packages/sqlite3-doc/images/qp/idx3.gif
/usr/share/doc/packages/sqlite3-doc/images/qp/idx3lu1.gif
/usr/share/doc/packages/sqlite3-doc/images/qp/idx3lu2.gif
/usr/share/doc/packages/sqlite3-doc/images/qp/idx4.gif
/usr/share/doc/packages/sqlite3-doc/images/qp/idx4lu1.gif
/usr/share/doc/packages/sqlite3-doc/images/qp/obfruitidx1.gif
/usr/share/doc/packages/sqlite3-doc/images/qp/obfruitidx4.gif
/usr/share/doc/packages/sqlite3-doc/images/qp/obfruitnoidx.gif
/usr/share/doc/packages/sqlite3-doc/images/qp/obrowid.gif
/usr/share/doc/packages/sqlite3-doc/images/qp/orquery.gif
/usr/share/doc/packages/sqlite3-doc/images/qp/partial-sort.gif
/usr/share/doc/packages/sqlite3-doc/images/qp/rowidlu.gif
/usr/share/doc/packages/sqlite3-doc/images/qp/tab.gif
/usr/share/doc/packages/sqlite3-doc/images/qp/tpchq8.gif
/usr/share/doc/packages/sqlite3-doc/images/qp/tpchq8.pikchr
/usr/share/doc/packages/sqlite3-doc/images/se.gif
/usr/share/doc/packages/sqlite3-doc/images/se.png
/usr/share/doc/packages/sqlite3-doc/images/shane1.jpg
/usr/share/doc/packages/sqlite3-doc/images/shared.gif
/usr/share/doc/packages/sqlite3-doc/images/sqlite370.jpg
/usr/share/doc/packages/sqlite3-doc/images/sqlite370_banner.gif
/usr/share/doc/packages/sqlite3-doc/images/sqlite370_banner.svg
/usr/share/doc/packages/sqlite3-doc/images/sqlitepie.jpg
/usr/share/doc/packages/sqlite3-doc/images/src_logo.gif
/usr/share/doc/packages/sqlite3-doc/images/sschart20221116.jpg
/usr/share/doc/packages/sqlite3-doc/images/sw.gif
/usr/share/doc/packages/sqlite3-doc/images/sw.png
/usr/share/doc/packages/sqlite3-doc/images/table-ex1b2.gif
/usr/share/doc/packages/sqlite3-doc/images/vfs1.gif
/usr/share/doc/packages/sqlite3-doc/imposter.html
/usr/share/doc/packages/sqlite3-doc/index.html
/usr/share/doc/packages/sqlite3-doc/inmemorydb.html
/usr/share/doc/packages/sqlite3-doc/intern-v-extern-blob.html
/usr/share/doc/packages/sqlite3-doc/invalidutf.html
/usr/share/doc/packages/sqlite3-doc/isolation.html
/usr/share/doc/packages/sqlite3-doc/json1.html
/usr/share/doc/packages/sqlite3-doc/keyword_index.html
/usr/share/doc/packages/sqlite3-doc/lang.html
/usr/share/doc/packages/sqlite3-doc/lang_aggfunc.html
/usr/share/doc/packages/sqlite3-doc/lang_altertable.html
/usr/share/doc/packages/sqlite3-doc/lang_analyze.html
/usr/share/doc/packages/sqlite3-doc/lang_attach.html
/usr/share/doc/packages/sqlite3-doc/lang_comment.html
/usr/share/doc/packages/sqlite3-doc/lang_conflict.html
/usr/share/doc/packages/sqlite3-doc/lang_corefunc.html
/usr/share/doc/packages/sqlite3-doc/lang_createindex.html
/usr/share/doc/packages/sqlite3-doc/lang_createtable.html
/usr/share/doc/packages/sqlite3-doc/lang_createtrigger.html
/usr/share/doc/packages/sqlite3-doc/lang_createview.html
/usr/share/doc/packages/sqlite3-doc/lang_createvtab.html
/usr/share/doc/packages/sqlite3-doc/lang_datefunc.html
/usr/share/doc/packages/sqlite3-doc/lang_delete.html
/usr/share/doc/packages/sqlite3-doc/lang_detach.html
/usr/share/doc/packages/sqlite3-doc/lang_dropindex.html
/usr/share/doc/packages/sqlite3-doc/lang_droptable.html
/usr/share/doc/packages/sqlite3-doc/lang_droptrigger.html
/usr/share/doc/packages/sqlite3-doc/lang_dropview.html
/usr/share/doc/packages/sqlite3-doc/lang_explain.html
/usr/share/doc/packages/sqlite3-doc/lang_expr.html
/usr/share/doc/packages/sqlite3-doc/lang_indexedby.html
/usr/share/doc/packages/sqlite3-doc/lang_insert.html
/usr/share/doc/packages/sqlite3-doc/lang_keywords.html
/usr/share/doc/packages/sqlite3-doc/lang_mathfunc.html
/usr/share/doc/packages/sqlite3-doc/lang_naming.html
/usr/share/doc/packages/sqlite3-doc/lang_reindex.html
/usr/share/doc/packages/sqlite3-doc/lang_replace.html
/usr/share/doc/packages/sqlite3-doc/lang_returning.html
/usr/share/doc/packages/sqlite3-doc/lang_savepoint.html
/usr/share/doc/packages/sqlite3-doc/lang_select.html
/usr/share/doc/packages/sqlite3-doc/lang_transaction.html
/usr/share/doc/packages/sqlite3-doc/lang_update.html
/usr/share/doc/packages/sqlite3-doc/lang_upsert.html
/usr/share/doc/packages/sqlite3-doc/lang_vacuum.html
/usr/share/doc/packages/sqlite3-doc/lang_with.html
/usr/share/doc/packages/sqlite3-doc/lemon.html
/usr/share/doc/packages/sqlite3-doc/limits.html
/usr/share/doc/packages/sqlite3-doc/loadext.html
/usr/share/doc/packages/sqlite3-doc/lockingv3.html
/usr/share/doc/packages/sqlite3-doc/locrsf.html
/usr/share/doc/packages/sqlite3-doc/lts.html
/usr/share/doc/packages/sqlite3-doc/malloc.html
/usr/share/doc/packages/sqlite3-doc/memstat.html
/usr/share/doc/packages/sqlite3-doc/mingw.html
/usr/share/doc/packages/sqlite3-doc/mmap.html
/usr/share/doc/packages/sqlite3-doc/mostdeployed.html
/usr/share/doc/packages/sqlite3-doc/news.html
/usr/share/doc/packages/sqlite3-doc/np1queryprob.html
/usr/share/doc/packages/sqlite3-doc/nulinstr.html
/usr/share/doc/packages/sqlite3-doc/nulls.html
/usr/share/doc/packages/sqlite3-doc/oldnews.html
/usr/share/doc/packages/sqlite3-doc/omitted.html
/usr/share/doc/packages/sqlite3-doc/onefile.html
/usr/share/doc/packages/sqlite3-doc/opcode.html
/usr/share/doc/packages/sqlite3-doc/optoverview.html
/usr/share/doc/packages/sqlite3-doc/partialindex.html
/usr/share/doc/packages/sqlite3-doc/percentile.html
/usr/share/doc/packages/sqlite3-doc/pgszchng2016.html
/usr/share/doc/packages/sqlite3-doc/pragma.html
/usr/share/doc/packages/sqlite3-doc/pressrelease-20071212.html
/usr/share/doc/packages/sqlite3-doc/printf.html
/usr/share/doc/packages/sqlite3-doc/privatebranch.html
/usr/share/doc/packages/sqlite3-doc/profile.html
/usr/share/doc/packages/sqlite3-doc/prosupport.html
/usr/share/doc/packages/sqlite3-doc/psow.html
/usr/share/doc/packages/sqlite3-doc/qmplan.html
/usr/share/doc/packages/sqlite3-doc/queryplanner-ng.html
/usr/share/doc/packages/sqlite3-doc/queryplanner.html
/usr/share/doc/packages/sqlite3-doc/quickstart.html
/usr/share/doc/packages/sqlite3-doc/quirks.html
/usr/share/doc/packages/sqlite3-doc/rbu.html
/usr/share/doc/packages/sqlite3-doc/recovery.html
/usr/share/doc/packages/sqlite3-doc/releaselog
/usr/share/doc/packages/sqlite3-doc/releaselog/3_0_0.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_0_1.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_0_2.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_0_3.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_0_4.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_0_5.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_0_6.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_0_7.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_0_8.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_10_0.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_10_1.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_10_2.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_11_0.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_11_1.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_12_0.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_12_1.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_12_2.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_13_0.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_14.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_14_1.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_14_2.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_15_0.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_15_1.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_15_2.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_16_0.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_16_1.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_16_2.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_17_0.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_18_0.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_18_1.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_18_2.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_19_0.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_19_1.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_19_2.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_19_3.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_1_0.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_1_1.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_1_2.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_1_3.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_1_4.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_1_5.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_1_6.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_20_0.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_20_1.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_21_0.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_22_0.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_23_0.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_23_1.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_24_0.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_25_0.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_25_1.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_25_2.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_25_3.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_26_0.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_27_0.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_27_1.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_27_2.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_28_0.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_29_0.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_2_0.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_2_1.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_2_2.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_2_3.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_2_4.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_2_5.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_2_6.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_2_7.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_2_8.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_30_0.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_30_1.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_31_0.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_31_1.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_32_0.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_32_1.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_32_2.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_32_3.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_33_0.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_34_0.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_34_1.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_35_0.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_35_1.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_35_2.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_35_3.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_35_4.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_35_5.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_36_0.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_37_0.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_37_1.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_37_2.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_38_0.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_38_1.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_38_2.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_38_3.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_38_4.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_38_5.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_39_0.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_39_1.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_39_2.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_39_3.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_39_4.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_3_0.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_3_1.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_3_10.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_3_11.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_3_12.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_3_13.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_3_14.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_3_15.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_3_16.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_3_17.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_3_2.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_3_3.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_3_4.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_3_5.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_3_6.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_3_7.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_3_8.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_3_9.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_40_0.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_40_1.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_41_0.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_41_1.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_41_2.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_42_0.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_43_0.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_43_1.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_43_2.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_44_0.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_44_1.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_44_2.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_45_0.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_45_1.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_45_2.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_45_3.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_46_0.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_46_1.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_47_0.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_47_1.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_47_2.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_48_0.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_49_0.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_49_1.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_49_2.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_4_0.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_4_1.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_4_2.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_50_0.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_50_1.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_50_2.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_50_3.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_50_4.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_51_0.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_51_1.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_51_2.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_51_3.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_52_0.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_53_0.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_53_1.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_53_2.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_53_3.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_53_4.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_54_0.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_54_1.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_5_0.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_5_1.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_5_2.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_5_3.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_5_4.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_5_5.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_5_6.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_5_7.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_5_8.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_5_9.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_6_0.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_6_1.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_6_10.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_6_11.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_6_12.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_6_13.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_6_14.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_6_14_1.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_6_14_2.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_6_15.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_6_16.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_6_16_1.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_6_17.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_6_18.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_6_19.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_6_2.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_6_20.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_6_21.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_6_22.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_6_23.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_6_23_1.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_6_3.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_6_4.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_6_5.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_6_6.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_6_6_1.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_6_6_2.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_6_7.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_6_8.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_6_9.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_7_0.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_7_0_1.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_7_1.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_7_10.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_7_11.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_7_12.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_7_12_1.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_7_13.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_7_14.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_7_14_1.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_7_15.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_7_15_1.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_7_15_2.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_7_16.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_7_16_1.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_7_16_2.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_7_17.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_7_2.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_7_3.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_7_4.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_7_5.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_7_6.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_7_6_1.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_7_6_2.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_7_6_3.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_7_7.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_7_7_1.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_7_8.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_7_9.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_8_0.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_8_0_1.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_8_0_2.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_8_1.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_8_10.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_8_10_1.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_8_10_2.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_8_11.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_8_11_1.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_8_2.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_8_3.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_8_3_1.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_8_4.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_8_4_1.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_8_4_2.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_8_4_3.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_8_5.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_8_6.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_8_7.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_8_7_1.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_8_7_2.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_8_7_3.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_8_7_4.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_8_8.html
/usr/share/doc/packages/sqlite3-doc/releaselog/3_8_8_1.html
