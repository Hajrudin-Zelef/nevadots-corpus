---
id: collect-261121-veille-ia/actu-business/release-history-2
title: "Release History"
domain: actu-business
role: reference
task: article
actors: []
dates: ["2022-03-26"]
keywords: ["added", "sqlite3", "sqlite", "interface", "json", "virtual", "error", "source"]
source: https://sqlite.org/draft/changes.html
source_anchor: ""
source_lines: [1501, 3000]
section: "Actu_Business"
---
3. SQLITE_SOURCE_ID: 2022-03-26 13:51:10 d33c709cc0af66bc5b6dc6216eba9f1f0b40960b9ae83694c986fbf4c1d6f08f
4. SHA3-256 for sqlite3.c: 0fbac6b6999f894184899431fb77b9792324c61246b2a010d736694ccaa6d613

### 2022-03-12 (3.38.1)

1.  Fix problems with the new Bloom filter optimization that might cause
     some obscure queries to get an incorrect answer.
2.  Fix the localtime modifier of the date and time functions so that
     it preserves fractional seconds.
3.  Fix the sqlite_offset SQL function so that it works correctly even
     in corner cases such as when the argument is a virtual column or the
     column of a view.
4.  Fix row value IN operator constraints on virtual tables so that they
     work correctly even if the virtual table implementation relies on bytecode
     to filter rows that do not satisfy the constraint.
5.  Other minor fixes to assert() statements, test cases, and documentation.
     See the source code timeline
     for details.
**Hashes:**
6. SQLITE_SOURCE_ID: 2022-03-12 13:37:29 38c210fdd258658321c85ec9c01a072fda3ada94540e3239d29b34dc547a8cbc
7. SHA3-256 for sqlite3.c: 262ba071e960a8a0a6ce39307ae30244a2b0dc9fe1c4c09d0e1070d4353cd92c

### 2022-02-22 (3.38.0)

1.  Added the -> and ->> operators for easier processing of JSON.
     The new operators are compatible with MySQL and PostgreSQL.
2.  The JSON functions are now built-ins.  It is no longer necessary
     to use the -DSQLITE_ENABLE_JSON1 compile-time option to enable JSON
     support.  JSON is on by default.  Disable the JSON interface using
     the new -DSQLITE_OMIT_JSON compile-time option.
3.  Enhancements to date and time functions:
       1.  Added the unixepoch() function.
     
  2.  Added the auto modifier and the julianday modifier.
     
4.  Rename the printf() SQL function to format() for better
     compatibility.  The original printf() name is retained as an alias
     for backwards compatibility.
5.  Added the sqlite3_error_offset() interface, which can sometimes
     help to localize an SQL error to a specific character in the input
     SQL text, so that applications can provide better error messages.
6.  Enhanced the interface to virtual tables as follows:
       1.  Added the sqlite3_vtab_distinct() interface.
     
  2.  Added the sqlite3_vtab_rhs_value() interface.
     
  3.  Added new operator types SQLITE_INDEX_CONSTRAINT_LIMIT
          and SQLITE_INDEX_CONSTRAINT_OFFSET.
     
  4.  Added the sqlite3_vtab_in() interface (and related) to enable
          a virtual table to process IN operator constraints all at once,
          rather than processing each value of the right-hand side of the
          IN operator separately.
     
7.  CLI enhancements:
       1.  Columnar output modes are enhanced to correctly handle tabs
          and newlines embedded in text.
     
  2.  Added options like "--wrap N", "--wordwrap on", and "--quote"
          to the columnar output modes.
     
  3.  Added the .mode qbox alias.
     
  4.  The .import command automatically disambiguates column names.
     
  5.  Use the new sqlite3_error_offset() interface to provide better
          error messages.
     
8.  Query planner enhancements:
       1.  Use a Bloom filter to speed up large analytic queries.
     
  2.  Use a balanced merge tree to evaluate UNION or UNION ALL
          compound SELECT statements that have an ORDER BY clause.
     
9.  The ALTER TABLE statement is changed to silently ignores entries in the
     sqlite_schema table that do not parse when PRAGMA writable_schema=ON.
**Hashes:**
10. SQLITE_SOURCE_ID: 2022-02-22 18:58:40 40fa792d359f84c3b9e9d6623743e1a59826274e221df1bde8f47086968a1bab
11. SHA3-256 for sqlite3.c: a69af0a88d59271a2dd3c846a3e93cbd29e7c499864f6c0462a3b4160bee1762

### 2022-01-06 (3.37.2)

1.  Fix a bug introduced
     in version 3.35.0 (2021-03-12) that
     can cause database corruption
     if a SAVEPOINT is rolled back while in PRAGMA temp_store=MEMORY mode,
     and other changes are made, and then the outer transaction commits.
     Check-in 73c2b50211d3ae26
2.  Fix a long-standing problem with ON DELETE CASCADE and ON UPDATE CASCADE
     in which a cache of the bytecode used to implement the cascading change
     was not being reset following a local DDL change.
     Check-in 5232c9777fe4fb13.
3.  Other minor fixes that should not impact production builds.
**Hashes:**
4. SQLITE_SOURCE_ID: 2022-01-06 13:25:41 872ba256cbf61d9290b571c0e6d82a20c224ca3ad82971edc46b29818d5d17a0
5. SHA3-256 for sqlite3.c: 1bb01c382295cba85ec4685cedc52a7477cdae71cc37f1ad0f48719a17af1e1e

### 2021-12-30 (3.37.1)

1.  Fix a bug introduced by the UPSERT enhancements of version 3.35.0 that
     can cause incorrect byte-code to be generated for some obscure but valid
     SQL, possibly resulting in a NULL-pointer dereference.
2.  Fix an OOB read that can occur in FTS5 when reading corrupt database files.
3.  Improved robustness of the --safe option in the CLI.
4.  Other minor fixes to assert() statements and test cases.
**Hashes:**
5. SQLITE_SOURCE_ID: 2021-12-30 15:30:28 378629bf2ea546f73eee84063c5358439a12f7300e433f18c9e1bddd948dea62
6. SHA3-256 for sqlite3.c: 915afb3f29c2d217ea0c283326a9df7d505e6c73b40236f0b33ded91f812d174

### 2021-11-27 (3.37.0)

1.  STRICT tables provide a prescriptive style of data type management,
     for developers who prefer that kind of thing.
2.  When adding columns that contain a
     CHECK constraint or a generated column containing a NOT NULL constraint,
     the ALTER TABLE ADD COLUMN now checks new constraints against
     preexisting rows in the database and will only proceed if no constraints
     are violated.
3.  Added the PRAGMA table_list statement.
4.  CLI enhancements:
       1.  Add the .connection command, allowing the CLI to keep multiple database
          connections open at the same time.
     
  2.  Add the --safe command-line option that disables
          dot-commands and SQL statements that might cause side-effects that extend
          beyond the single database file named on the command-line.
     
  3.  Performance improvements when reading SQL statements
          that span many lines.
     
5.  Added the sqlite3_autovacuum_pages() interface.
6.  The sqlite3_deserialize() does not and has never worked for the TEMP
     database.  That limitation is now noted in the documentation.
7.  The query planner now omits ORDER BY clauses on subqueries and views
     if removing those clauses does not change the semantics of the query.
8.  The generate_series table-valued function extension is modified so that
     the first parameter ("START") is now required.  This is done as a way to
     demonstrate how to write table-valued functions with required parameters.
     The legacy behavior is available using the -DZERO_ARGUMENT_GENERATE_SERIES
     compile-time option.
9.  Added new sqlite3_changes64() and sqlite3_total_changes64() interfaces.
10.  Added the SQLITE_OPEN_EXRESCODE flag option to sqlite3_open_v2().
11.  Use less memory to hold the database schema.
**Hashes:**
12. SQLITE_SOURCE_ID: 2021-11-27 14:13:22 bd41822c7424d393a30e92ff6cb254d25c26769889c1499a18a0b9339f5d6c8a
13. SHA3-256 for sqlite3.c: a202a950ab401cda052e81259e96d6e64ad91faaaaf5690d769f64c2ab962f27

### 2021-06-18 (3.36.0)

1.  Improvement to the EXPLAIN QUERY PLAN output to make it easier to
     understand.
2.  Byte-order marks at the start of a token are skipped as if they
     were whitespace.
3.  An error is raised on any attempt to access the rowid of a VIEW or subquery.
     Formerly, the rowid of a VIEW would be indeterminate and often would be NULL.
     The -DSQLITE_ALLOW_ROWID_IN_VIEW compile-time option is available to restore
     the legacy behavior for applications that need it.
4.  The sqlite3_deserialize() and sqlite3_serialize() interfaces are now
     enabled by default.  The -DSQLITE_ENABLE_DESERIALIZE compile-time option is
     no longer required.  Instead, there is a new -DSQLITE_OMIT_DESERIALIZE
     compile-time option to omit those interfaces.
5.  The "memdb" VFS now allows the same in-memory database to be shared among
     multiple database connections in the same process as long as the
     database name begins with "/".
6.  Back out the EXISTS-to-IN optimization (item 8b in the
     SQLite 3.35.0 change log)
     as it was found to slow down queries more often than speed them up.
7.  Improve the constant-propagation optimization so that it works on
     non-join queries.
8.  The REGEXP extension is
     now included in CLI builds.
**Hashes:**
9. SQLITE_SOURCE_ID: 2021-06-18 18:36:39 5c9a6c06871cb9fe42814af9c039eb6da5427a6ec28f187af7ebfb62eafa66e5
10. SHA3-256 for sqlite3.c: 2a8e87aaa414ac2d45ace8eb74e710935423607a8de0fafcb36bbde5b952d157

### 2021-04-19 (3.35.5)

1.  Fix defects in the new ALTER TABLE DROP COLUMN feature that could
     corrupt the database file.
2.  Fix an obscure query optimizer problem that might cause an incorrect
     query result.
**Hashes:**
3. SQLITE_SOURCE_ID: 2021-04-19 18:32:05 1b256d97b553a9611efca188a3d995a2fff712759044ba480f9a0c9e98fae886
4. SHA3-256 for sqlite3.c: e42291343e8f03940e57fffcf1631e7921013b94419c2f943e816d3edf4e1bbe

### 2021-04-02 (3.35.4)

1.  Fix a defect in the query planner optimization identified by
     item 8b above.  Ticket
     de7db14784a08053.
2.  Fix a defect in the new RETURNING syntax.  Ticket
     132994c8b1063bfb.
3.  Fix the new RETURNING feature so that it raises an error if one of
     the terms in the RETURNING clause references a unknown table, instead
     of silently ignoring that error.
4.  Fix an assertion associated with aggregate function processing that
     was incorrectly triggered by the push-down optimization.
**Hashes:**
5. SQLITE_SOURCE_ID: 2021-04-02 15:20:15 5d4c65779dab868b285519b19e4cf9d451d50c6048f06f653aa701ec212df45e
6. SHA3-256 for sqlite3.c: 528b8a26bf5ffd4c7b4647b5b799f86e8fb1a075f715b87a414e94fba3d09dbe

### 2021-03-26 (3.35.3)

1.  Enhance the OP_OpenDup opcode of the bytecode engine so that it works even if the
     cursor being duplicated itself came from OP_OpenDup.  Fix for
     ticket bb8a9fd4a9b7fce5.
     This problem only came to light due to the recent MATERIALIZED hint enhancement.
2.  When materializing correlated common table expressions, do so separately for each
     use case, as that is required for correctness.  This fixes a problem that was
     introduced by the MATERIALIZED hint enhancement.
3.  Fix a problem in the filename normalizer of the unix VFS.
4.  Fix the "box" output mode in the CLI so that it works with statements that
     returns one or more rows of zero columns (such as PRAGMA incremental_vacuum).
     Forum post afbbcb5b72.
5.  Improvements to error messages generated by faulty common table expressions.
     Forum post aa5a0431c99e.
6.  Fix some incorrect assert() statements.
7.  Fix to the SELECT statement syntax diagram so that the FROM clause
     syntax is shown correctly.
     Forum post 9ed02582fe.
8.  Fix the EBCDIC character classifier so that it understands newlines as whitespace.
     Forum post 58540ce22dcd.
9.  Improvements the xBestIndex method in the implementation of the
     (unsupported) wholenumber virtual table
     extension so that it does a better job of convincing the query planner to
     avoid trying to materialize a table with an infinite number of rows.
     Forum post b52a020ce4.
**Hashes:**
10. SQLITE_SOURCE_ID: 2021-03-26 12:12:52 4c5e6c200adc8afe0814936c67a971efc516d1bd739cb620235592f18f40be2a
11. SHA3-256 for sqlite3.c: 91ca6c0a30ebfdba4420bb35f4fd9149d13e45fc853d86ad7527db363e282683

### 2021-03-17 (3.35.2)

1.  Fix a problem in the
     appendvfs.c
     extension that was introduced into version 3.35.0.
2.  Ensure that date/time functions with no arguments (which generate
     responses that depend on the current time) are treated as
     non-deterministic functions. Ticket
     2c6c8689fb5f3d2f
3.  Fix a problem in the sqldiff utility program having to do with
     unusual whitespace characters in a virtual table definition.
4.  Limit the new UNION ALL optimization described by item 8c in the
     3.35.0 release so that it does not try to make too many new subqueries.
     See forum thread 140a67d3d2
     for details.
**Hashes:**
5. SQLITE_SOURCE_ID: 2021-03-17 19:07:21 ea80f3002f4120f5dcee76e8779dfdc88e1e096c5cdd06904c20fd26d50c3827
6. SHA3-256 for sqlite3.c: e8edc7b1512a2e050d548d0840bec6eef83cc297af1426c34c0ee8720f378a11

### 2021-03-15 (3.35.1)

1.  Fix a bug in the new DROP COLUMN
     feature when used on columns that are indexed and that are quoted in the index
     definition.
2.  Improve the built-in documentation for the .dump command in the CLI.
**Hashes:**
3. SQLITE_SOURCE_ID: 2021-03-15 16:53:57 aea12399bf1fdc76af43499d4624c3afa17c3e6c2459b71c195804bb98def66a
4. SHA3-256 for sqlite3.c: fc79e27fd030226c07691b7d7c23aa81c8d46bc3bef5af39060e1507c82b0523

### 2021-03-12 (3.35.0)

1.  Added built-in SQL math functions(). (Requires the
     -DSQLITE_ENABLE_MATH_FUNCTIONS compile-time option.)
2.  Added support for ALTER TABLE DROP COLUMN.
3.  Generalize UPSERT:
       1.  Allow multiple ON CONFLICT clauses that are evaluated in order,
     
  2.  The final ON CONFLICT clause may omit the conflict target and
          yet still use DO UPDATE.
     
4.  Add support for the RETURNING clause on DELETE, INSERT, and
     UPDATE statements.
5.  Use less memory when running VACUUM on databases containing very large
     TEXT or BLOB values.  It is no longer necessary to hold the entire TEXT
     or BLOB in memory all at once.
6.  Add support for the MATERIALIZED and NOT MATERIALIZED hints when
     specifying common table expressions.  The default behavior was
     formerly NOT MATERIALIZED, but is now changed to MATERIALIZED for
     CTEs that are used more than once.
7.  The SQLITE_DBCONFIG_ENABLE_TRIGGER and SQLITE_DBCONFIG_ENABLE_VIEW
     settings are modified so that they only control triggers and views
     in the main database schema or in attached database schemas and not in
     the TEMP schema. TEMP triggers and views are always allowed.
8.  Query planner/optimizer improvements:
       1.  Enhancements to the min/max optimization so that it works better
          with the IN operator and the OP_SeekScan optimization of the
          previous release.
     
  2.  Attempt to process EXISTS operators in the WHERE clause as if
          they were IN operators, in cases where this is a valid transformation
          and seems likely to improve performance.
     
  3.  Allow UNION ALL sub-queries to be flattened even if the parent query is a join.
     
  4.  Use an index, if appropriate, on IS NOT NULL expressions in the WHERE clause,
          even if STAT4 is disabled.
     
  5.  Expressions of the form "x IS NULL" or "x IS NOT NULL" might be converted to simply
          FALSE or TRUE, if "x" is a column that has a "NOT NULL" constraint and is not
          involved in an outer join.
     
  6.  Avoid checking foreign key constraints on an UPDATE statement if the UPDATE does
          not modify any columns associated with the foreign key.
     
  7.  Allow WHERE terms to be pushed down into sub-queries
          that contain window functions,
          as long as the WHERE term is made up of entirely of constants and copies of expressions
          found in the PARTITION BY clauses of all window functions in the sub-query.
     
9.  CLI enhancements:
       1.  Enhance the ".stats" command to accept new arguments "stmt" and
          "vmstep", causing prepare statement statistics and only the
          virtual-machine step count to be shown, respectively.
     
  2.  Add the ".filectrl data_version" command.
     
  3.  Enhance the ".once" and ".output" commands so that if the destination argument
          begins with "|" (indicating that output is redirected into a pipe) then the
          argument does not need to be quoted.
     
10.  Bug fixes:
       1.  Fix a potential NULL pointer dereference when processing a
          syntactically incorrect SELECT statement with a correlated WHERE
          clause and a "HAVING 0" clause.  (Also fixed in the 3.34.1 patch release.)
     
  2.  Fix a bug in the IN-operator optimization
          of version 3.33.0 that can cause an incorrect answer.
     
  3.  Fix incorrect answers from the LIKE operator if the pattern ends with "%" and there
          is an "ESCAPE '_'" clause.
     
 **Hashes:**
11. SQLITE_SOURCE_ID: 2021-03-12 15:10:09 acd63062eb06748bfe9e4886639e4f2b54ea6a496a83f10716abbaba4115500b
12. SHA3-256 for sqlite3.c: 73a740d881735bef9de7f7bce8c9e6b9e57fe3e77fa7d76a6e8fc5c262fbaedf

### 2021-01-20 (3.34.1)

1.  Fix a potential use-after-free bug when processing a a subquery with both
     a correlated WHERE clause and a "HAVING 0" clause and where the parent
     query is an aggregate.
2.  Fix documentation typos
3.  Fix minor problems in extensions.
**Hashes:**
4. SQLITE_SOURCE_ID: 2021-01-20 14:10:07 10e20c0b43500cfb9bbc0eaa061c57514f715d87238f4d835880cd846b9ebd1f
5. SHA3-256 for sqlite3.c: 799a7be90651fc7296113b641a70b028c142d767b25af1d0a78f93dcf1a2bf20

### 2020-12-01 (3.34.0)

1.  Added the sqlite3_txn_state() interface for reporting on the current
     transaction state of the database connection.
2.  Enhance recursive common table expressions to support two or more
     recursive terms as is done by SQL Server, since this helps make
     queries against graphs easier to write and faster to execute.
3.  Improved error messages on CHECK constraint failures.
4.  CLI enhancements:
       1.  The .read dot-command now accepts a pipeline in addition to
          a filename.
     
  2.  Added options --data-only and --nosys to the .dump dot-command.
     
  3.  Added the --nosys option to the .schema dot-command.
     
  4.  Table name quoting works correctly for the .import dot-command.
     
  5.  The generate_series(START,END,STEP) table-valued function
          extension is now built into the CLI.
     
  6.  The .databases dot-command now shows the status of each database
          file as determined by sqlite3_db_readonly() and
          sqlite3_txn_state().
     
  7.  Added the --tabs command-line option that sets
          .mode tabs.
     
  8.  The --init option reports an error if the file named as its argument
          cannot be opened.  The --init option also now honors the --bail option.
     
5.  Query planner improvements:
       1.  Improved estimates for the cost of running a DISTINCT operator.
     
  2.  When doing an UPDATE or DELETE using a multi-column index where
          only a few of the earlier columns of the index are useful for the
          index lookup, postpone doing the main table seek until after all
          WHERE clause constraints have been evaluated, in case those
          constraints can be covered by unused later terms of the index,
          thus avoiding unnecessary main table seeks.
     
  3.  The new OP_SeekScan opcode is used to improve performance of
          multi-column index look-ups when later columns are constrained
          by an IN operator.
     
6.  The BEGIN IMMEDIATE and BEGIN EXCLUSIVE commands now work even
     if one or more attached database files are read-only.
7.  Enhanced FTS5 to support trigram indexes.
8.  Improved performance of WAL mode locking primitives in cases where
     there are hundreds of connections all accessing the same database file
     at once.
9.  Enhanced the carray() table-valued function to include a single-argument
     form that is bound using the auxiliary sqlite3_carray_bind() interface.
10.  The substr() SQL function can now also be called "substring()" for
     compatibility with  SQL Server.
11.  The syntax diagrams are now implemented as
     Pikchr scripts and rendered
     as SVG for improved legibility and ease of maintenance.
**Hashes:**
12. SQLITE_SOURCE_ID: 2020-12-01 16:14:00 a26b6597e3ae272231b96f9982c3bcc17ddec2f2b6eb4df06a224b91089fed5b
13. SHA3-256 for sqlite3.c: fbd895b0655a337b2cd657675f314188a4e9fe614444cc63dfeb3f066f674514

### 2020-08-14 (3.33.0)

1.  Support for UPDATE FROM following the PostgreSQL syntax.
2.  Increase the maximum size of database files to 281 TB.
3.  Extended the PRAGMA integrity_check statement so that it can optionally be
     limited to verifying just a single table and its indexes, rather than the
     entire database file.
4.  Added the decimal extension for doing arbitrary-precision decimal arithmetic.
5.  Enhancements to the ieee754 extension for working with IEEE 754 binary64 numbers.
6.  CLI enhancements:
      1.  Added four new output modes: "box", "json", "markdown",
         and "table".
    
  2.  The "column" output mode automatically expands columns to
         contain the longest output row and automatically turns
         ".header" on if it has not been previously set.
    
  3.  The "quote" output mode honors ".separator"
    
  4.  The decimal extension and the ieee754 extension are built-in to the CLI
    
7.  Query planner improvements:
       1.  Add the ability to find a
          full-index-scan query plan for queries using INDEXED BY
          which previously would fail with "no query solution".
     
  2.  Do a better job of
          detecting missing, incomplete, and/or dodgy sqlite_stat1
          data and generates good query plans in spite of the
          misinformation.
     
  3.  Improved performance of queries like "SELECT min(x) FROM t WHERE y IN (?,?,?)"
          assuming an index on t(x,y).
     
8.  In WAL mode, if a writer crashes and leaves the shm file in an inconsistent
     state, subsequent transactions are now able to recover the shm file even if
     there are active read transactions.  Before this enhancement, shm file recovery
     that scenario would result in an SQLITE_PROTOCOL error.
**Hashes:**
9. SQLITE_SOURCE_ID: 2020-08-14 13:23:32 fca8dc8b578f215a969cd899336378966156154710873e68b3d9ac5881b0ff3f
10. SHA3-256 for sqlite3.c: d00b7fffa6d33af2303430eaf394321da2960604d25a4471c7af566344f2abf9

### 2020-06-18 (3.32.3)

1.  Various minor bug fixes including fixes for tickets
     8f157e8010b22af0,
     9fb26d37cefaba40,
     e367f31901ea8700,
     b706351ce2ecf59a,
     7c6d876f84e6e7e2, and
     c8d3b9f0a750a529.
**Hashes:**
2. SQLITE_SOURCE_ID: 2020-06-18 14:00:33 7ebdfa80be8e8e73324b8d66b3460222eb74c7e9dfd655b48d6ca7e1933cc8fd
3. SHA3-256 for sqlite3.c: b62b77ee1c561a69a71bb557694aaa5141f1714c1ff6cc1ba8aa8733c92d4f52

### 2020-06-04 (3.32.2)

1.  Fix a long-standing bug in the byte-code engine that can cause a
     COMMIT command report as success when in fact it failed
     to commit.  Ticket
     810dc8038872e212
**Hashes:**
2. SQLITE_SOURCE_ID: 2020-06-04 12:58:43 ec02243ea6ce33b090870ae55ab8aa2534b54d216d45c4aa2fdbb00e86861e8c
3. SHA3-256 for sqlite3.c: f17a2a57f7eebc72d405f3b640b4a49bcd02364a9c36e04feeb145eccafa3f8d

### 2020-05-25 (3.32.1)

1.  Fix two long-standing bugs that allow malicious SQL statements
     to crash the process that is running SQLite.  These bugs were announced
     by a third-party approximately 24 hours after the 3.32.0 release but are
     not specific to the 3.32.0 release.
2.  Other minor compiler-warning fixes and whatnot.
**Hashes:**
3. SQLITE_SOURCE_ID: 2020-05-25 16:19:56 0c1fcf4711a2e66c813aed38cf41cd3e2123ee8eb6db98118086764c4ba83350
4. SHA3-256 for sqlite3.c: f695ae21abf045e4ee77980a67ab2c6e03275009e593ee860a2eabf840482372

### 2020-05-22 (3.32.0)

1.  Added support for approximate ANALYZE using the
     PRAGMA analysis_limit command.
2.  Added the bytecode virtual table.
3.  Add the checksum VFS shim to the set of run-time loadable
     extensions included in the source tree.
4.  Added the iif() SQL function.
5.  INSERT and UPDATE statements now always apply column affinity
     before computing CHECK constraints.  This bug fix could, in
     theory, cause problems for legacy databases with unorthodox
     CHECK constraints the require the input type for an INSERT
     is different from the declared column type. See ticket
     86ba67afafded936
     for more information.
6.  Added the sqlite3_create_filename(), sqlite3_free_filename(),
     and sqlite3_database_file_object()
     interfaces to better support of VFS shim implementations.
7.  Increase the default upper bound
     on the number of parameters from 999 to 32766.
8.  Added code for the UINT collating sequence as an optional
     loadable extension.
9.  Enhancements to the CLI:
       1.  Add options to the .import command: --csv, --ascii, --skip
     
  2.  The .dump command now accepts multiple LIKE-pattern arguments
          and outputs the union of all matching tables.
     
  3.  Add the .oom command in debugging builds
     
  4.  Add the --bom option to the .excel, .output, and .once
          commands.
     
  5.  Enhance the .filectrl command to support the --schema option.
     
  6.  The UINT collating sequence extension is automatically loaded
     
10.  The ESCAPE clause of a LIKE operator now overrides wildcard
     characters, so that the behavior matches what PostgreSQL does.
11. SQLITE_SOURCE_ID: 2020-05-22 17:46:16 5998789c9c744bce92e4cff7636bba800a75574243d6977e1fc8281e360f8d5a
12. SHA3-256 for sqlite3.c: 33ed868b21b62ce1d0352ed88bdbd9880a42f29046497a222df6459fc32a356f

### 2020-01-27 (3.31.1)

1.  Revert the data layout for an internal-use-only SQLite data structure.
     Applications that use SQLite should never reference internal SQLite
     data structures, but some do anyhow, and a change to one such
     data structure in 3.30.0 broke a popular and widely-deployed
     application.  Reverting that change in SQLite, at least temporarily,
     gives developers of misbehaving applications time to fix their code.
2.  Fix a typos in the sqlite3ext.h header file that prevented the
     sqlite3_stmt_isexplain() and sqlite3_value_frombind() interfaces
     from being called from run-time loadable extensions.
**Hashes:**
3. SQLITE_SOURCE_ID: 2020-01-27 19:55:54 3bfa9cc97da10598521b342961df8f5f68c7388fa117345eeb516eaa837bb4d6
4. SHA3-256 for sqlite3.c: de465c64f09529429a38cbdf637acce4dfda6897f93e3db3594009e0fed56d27

### 2020-01-22 (3.31.0)

1. Add support for generated columns.
2. Add the sqlite3_hard_heap_limit64() interface and the corresponding
    PRAGMA hard_heap_limit command.
3. Enhance the function_list pragma to show the number of arguments on each
    function, the type of function (scalar, aggregate, window), and the function
    property flags SQLITE_DETERMINISTIC, SQLITE_DIRECTONLY,
    SQLITE_INNOCUOUS, and/or SQLITE_SUBTYPE.
4. Add the aggregated mode feature to the
    DBSTAT virtual table.
5. Add the SQLITE_OPEN_NOFOLLOW option to sqlite3_open_v2() that
    prevents SQLite from opening symbolic links.
6. Added the "#-N" array notation for JSON function path arguments.
7. Added the SQLITE_DBCONFIG_TRUSTED_SCHEMA connection setting which is
    also controllable via the new trusted_schema pragma and at compile-time
    using the -DSQLITE_TRUSTED_SCHEMA compile-time option.
8. Added APIs sqlite3_filename_database(), sqlite3_filename_journal(), and
    sqlite3_filename_wal() which are useful for specialized extensions.
9. Add the sqlite3_uri_key() interface.
10. Upgraded the sqlite3_uri_parameter() function so that it works with the
    rollback journal or WAL filename in addition to the database filename.
11. Provide the ability to tag application-defined SQL functions with
    new properties SQLITE_INNOCUOUS or SQLITE_DIRECTONLY.
12. Add new verbs to sqlite3_vtab_config() so that the xConnect method
    of virtual tables can declare the virtual table as
    SQLITE_VTAB_INNOCUOUS or SQLITE_VTAB_DIRECTONLY.
13. Faster response to sqlite3_interrupt().
14. Added the uuid.c extension module
    implementing functions for processing RFC-4122 UUIDs.
15. The lookaside memory allocator is enhanced to support two separate memory
    pools with different sized allocations in each pool.  This allows more memory
    allocations to be covered by lookaside while at the same time reducing the
    heap memory usage to 48KB per connection, down from 120KB.
16. The legacy_file_format pragma is deactivated.  It is now a no-op.  In its place,
    the SQLITE_DBCONFIG_LEGACY_FILE_FORMAT option to sqlite3_db_config() is
    provided.  The legacy_file_format pragma is deactivated because (1) it is
    rarely useful and (2) it is incompatible with VACUUM in schemas that have
    tables with both generated columns and descending indexes.
    Ticket 6484e6ce678fffab
**Hashes:**
17. SQLITE_SOURCE_ID: 2020-01-22 18:38:59 f6affdd41608946fcfcea914ece149038a8b25a62bbe719ed2561c649b86d824
18. SHA3-256 for sqlite3.c: a5fca0b9f8cbf80ac89b97193378c719d4af4b7d647729d8df9c0c0fca7b1388

### 2019-10-10 (3.30.1)

1.  Fix a bug in the query flattener that might cause a segfault
for nested queries that use the new
FILTER clause on aggregate functions.
Ticket 1079ad19993d13fa
2.  Cherrypick fixes for other obscure problems found since the 3.30.0
     release
**Hashes:**
3. SQLITE_SOURCE_ID: 2019-10-10 20:19:45 18db032d058f1436ce3dea84081f4ee5a0f2259ad97301d43c426bc7f3df1b0b
4. SHA3-256 for sqlite3.c: f96fafe4c110ed7d77fc70a7d690e5edd1e64fefb84b3b5969a722d885de1f2d

### 2019-10-04 (3.30.0)

1.  Add support for the FILTER clause on aggregate functions.
2.  Add support for the NULLS FIRST and NULLS LAST syntax in ORDER BY clauses.
3.  The index_info and index_xinfo pragmas are enhanced to provide
     information about the on-disk representation of WITHOUT ROWID
     tables.
4.  Add the sqlite3_drop_modules() interface, allowing applications
     to disable automatically loaded virtual tables that they do not
     need.
5.  Improvements to the .recover dot-command in the CLI so that
     it recovers more content from corrupt database files.
6.  Enhance the RBU extension to support indexes on expressions.
7.  Change the schema parser so that it will error out if any of
     the type, name, and tbl_name columns of the sqlite_master table
     have been corrupted and the database connection is not in
     writable_schema mode.
8.  The PRAGMA function_list, PRAGMA module_list, and
     PRAGMA pragma_list commands are now
     enabled in all builds by default.  Disable them using
     -DSQLITE_OMIT_INTROSPECTION_PRAGMAS.
9.  Add the SQLITE_DBCONFIG_ENABLE_VIEW option for sqlite3_db_config().
10.  Added the TCL Interface config method in order to be able to
     disable SQLITE_DBCONFIG_ENABLE_VIEW as well as control other
     sqlite3_db_config() options from TCL.
11.  Added the SQLITE_DIRECTONLY flag for
     application-defined SQL functions to prevent those functions from
     being used inside triggers and views.
12.  The legacy SQLITE_ENABLE_STAT3 compile-time option is now a no-op.
**Hashes:**
13. SQLITE_SOURCE_ID: 2019-10-04 15:03:17 c20a35336432025445f9f7e289d0cc3e4003fb17f45a4ce74c6269c407c6e09f
14. SHA3-256 for sqlite3.c: f04393dd47205a4ee2b98ff737dc51a3fdbcc14c055b88d58f5b27d0672158f5

### 2019-07-10 (3.29.0)

1.  Added the SQLITE_DBCONFIG_DQS_DML and SQLITE_DBCONFIG_DQS_DDL
     actions to sqlite3_db_config() for activating and deactivating
     the double-quoted string literal misfeature.  Both default to "on"
     for legacy compatibility, but developers are encouraged to turn them
     "off", perhaps using the -DSQLITE_DQS=0 compile-time option.
2.  -DSQLITE_DQS=0 is now a recommended compile-time option.
3.  Improvements to the query planner:
  1.  Improved optimization of AND and OR operators when one or the other
       operand is a constant.
  
  2.  Enhancements to the LIKE optimization for cases when the left-hand
       side column has numeric affinity.
  
4.  Added the "sqlite_dbdata"
       virtual table for extracting raw low-level content from an SQLite database,
       even a database that is corrupt.
  
5.  Improvements to rounding behavior, so that the results of rounding
       binary numbers using the round() function are closer to what people
       who are used to thinking in decimal actually expect.
  
6.  Enhancements to the CLI:
  1.  Add the ".recover" command which tries to recover as much content
       as possible from a corrupt database file.
  
  2.  Add the ".filectrl" command useful for testing.
  
  3.  Add the long-standing ".testctrl" command to the ".help" menu.
  
  4.  Added the ".dbconfig" command
 **Hashes:**
7. SQLITE_SOURCE_ID: 2019-07-10 17:32:03 fc82b73eaac8b36950e527f12c4b5dc1e147e6f4ad2217ae43ad82882a88bfa6
8. SHA3-256 for sqlite3.c: d9a5daf7697a827f4b2638276ce639fa04e8e8bb5fd3a6b683cfad10f1c81b12

### 2019-04-16 (3.28.0)

1.  Enhanced window functions:
  1.  Add support the EXCLUDE clause.
  
  2.  Add support for window chaining.
  
  3.  Add support for GROUPS frames.
  
  4.  Add support for "<expr> PRECEDING" and
       "<expr> FOLLOWING" boundaries
       in RANGE frames.
2.  Added the new sqlite3_stmt_isexplain(S) interface for determining
     whether or not a prepared statement is an EXPLAIN.
3.  Enhanced VACUUM INTO so that it works for read-only databases.
4.  New query optimizations:
  1.  Enable the LIKE optimization for cases when the ESCAPE keyword
       is present and PRAGMA case_sensitive_like is on.
  
  2.  In queries that are driven by a partial index, avoid unnecessary
       tests of the constraint named in the WHERE clause of the partial
       index, since we know that constraint must always be true.
5.  Enhancements to the TCL Interface:
  1.  Added the -returntype option to the function method.
  
  2.  Added the new bind_fallback method.
6.  Enhancements to the CLI:
  1.  Added support for bound parameters and the .parameter command.
  
  2.  Fix the readfile() function so that it returns
       an empty BLOB rather than throwing an out-of-memory error when
       reading an empty file.
  
  3.  Fix the writefile() function so that when it
       creates new directories along the path of a new file, it gives them
       umask permissions rather than the same permissions as the file.
  
  4.  Change --update option in the .archive command so that it skips
       files that are already in the archive and are unchanged.  Add the
       new --insert option that works like --update used to work.
7.  Added the fossildelta.c
     extension that can create, apply, and deconstruct the
 Fossil DVCS file delta format
     that is used by the RBU extension.
8.  Added the SQLITE_DBCONFIG_WRITABLE_SCHEMA verb for the sqlite3_db_config()
     interface, that does the same work as PRAGMA writable_schema without using the
     SQL parser.
9.  Added the sqlite3_value_frombind() API for determining if the argument
     to an SQL function is from a bound parameter.
10.  Security and compatibilities enhancements to fts3_tokenizer():
  1.  The fts3_tokenizer() function always returns NULL
       unless either the legacy application-defined FTS3 tokenizers interface
       are enabled using
       the sqlite3_db_config(SQLITE_DBCONFIG_ENABLE_FTS3_TOKENIZER)
       setting, or unless the first argument to fts3_tokenizer() is a bound parameter.
  
  2.  The two-argument version of fts3_tokenizer() accepts a pointer to the
       tokenizer method object even without
       the sqlite3_db_config(SQLITE_DBCONFIG_ENABLE_FTS3_TOKENIZER) setting
       if the second argument is a bound parameter
11.  Improved robustness against corrupt database files.
12.  Miscellaneous performance enhancements
13.  Established a Git mirror of the offical SQLite source tree.
     The canonical sources for SQLite are maintained using the
     Fossil DVCS at https://sqlite.org/src.
     The Git mirror can be seen at https://github.com/sqlite/sqlite.
**Hashes:**
14. SQLITE_SOURCE_ID: 2019-04-16 19:49:53 884b4b7e502b4e991677b53971277adfaf0a04a284f8e483e2553d0f83156b50
15. SHA3-256 for sqlite3.c: 411efca996b65448d9798eb203d6ebe9627b7161a646f5d00911e2902a57b2e9

### 2019-02-25 (3.27.2)

1.  Fix a bug in the IN operator that was introduced by an
     attempted optimization in version 3.27.0. Ticket
     df46dfb631f75694
2.  Fix a bug causing a crash when a window function is misused.  Ticket
     4feb3159c6bc3f7e33959.
3.  Fix various documentation typos
**Hashes:**
4. SQLITE_SOURCE_ID: bd49a8271d650fa89e446b42e513b595a717b9212c91dd384aab871fc1d0f6d7
5. SHA3-256 for sqlite3.c: 1dbae33bff261f979d0042338f72c9e734b11a80720fb32498bae9150cc576e7

### 2019-02-08 (3.27.1)

1.  Fix a bug in the query optimizer: an adverse interaction between
the OR optimization and the optimization that tries to use values
read directly from an expression index instead of recomputing the
expression.
Ticket 4e8e4857d32d401f
**Hashes:**
2. SQLITE_SOURCE_ID: 2019-02-08 13:17:39 0eca3dd3d38b31c92b49ca2d311128b74584714d9e7de895b1a6286ef959a1dd
3. SHA3-256 for sqlite3.c: 11c14992660d5ac713ea8bea48dc5e6123f26bc8d3075fe5585d1a217d090233

### 2019-02-07 (3.27.0)

1. Added the VACUUM INTO command
2. Issue an SQLITE_WARNING message on the error log if a
double-quoted string literal is used.
3. The sqlite3_normalized_sql() interface works on any prepared statement
created using sqlite3_prepare_v2() or sqlite3_prepare_v3().  It is no
longer necessary to use sqlite3_prepare_v3() with SQLITE_PREPARE_NORMALIZE
in order to use sqlite3_normalized_sql().
4. Added the remove_diacritics=2 option to FTS3 and FTS5.
5. Added the SQLITE_PREPARE_NO_VTAB option to sqlite3_prepare_v3().
Use that option to prevent circular references to shadow tables from
causing resource leaks.
6. Enhancements to the sqlite3_deserialize() interface:
  1.  Add the SQLITE_FCNTL_SIZE_LIMIT file-control for
       setting an upper bound on the size of the in-memory database created
       by sqlite3_deserialize.  The default upper bound is 1GiB, or whatever
       alternative value is specified by
       sqlite3_config(SQLITE_CONFIG_MEMDB_MAXSIZE)
       and/or SQLITE_MEMDB_DEFAULT_MAXSIZE.
  
  2.  Honor the SQLITE_DESERIALIZE_READONLY flag, which was previously described
       in the documentation, but was previously a no-op.
  
  3.  Enhance the "deserialize" command of the TCL Interface to give it
       new "--maxsize N" and "--readonly BOOLEAN" options.
7. Enhancements to the CLI, mostly to support testing and debugging
of the SQLite library itself:
  1.  Add support for ".open --hexdb". The
       "dbtotxt" utility
       program used to generate the text for the "hexdb" is added to the
       source tree.
  
  2.  Add support for the "--maxsize N" option on ".open --deserialize".
  
  3.  Add the "--memtrace" command-line option, to show all memory allocations
       and deallocations.
  
  4.  Add the ".eqp trace" option on builds with SQLITE_DEBUG, to enable
       bytecode program listing with indentation and
       PRAGMA vdbe_trace all in one step.
  
  5.  Add the ".progress" command for accessing
       the sqlite3_progress_handler() interface.
  
  6.  Add the "--async" option to the ".backup" command.
  
  7.  Add options "--expanded", "--normalized", "--plain", "--profile", "--row",
       "--stmt", and "--close" to the ".trace" command.
8.  Increased robustness against malicious SQL that is run against a
     maliciously corrupted database.
 **Bug fixes:**
9. Do not use a partial index to do a table scan on an IN operator.
Ticket 1d958d90596593a774.
10. Fix the query flattener so that it works on queries that contain
subqueries that use window functions.
Ticket 709fcd17810f65f717
11. Ensure that ALTER TABLE modifies table and column names embedded in WITH
clauses that are part of views and triggers.
12. Fix a parser bug that prevented the use of parentheses around table-valued
functions.
13. Fix a problem with the OR optimization on indexes on expressions.
Ticket d96eba87698a428c1d.
14. Fix a problem with the
LEFT JOIN strength reduction optimization in which the optimization
was being applied inappropriately due to an IS NOT NULL operator.
Ticket 5948e09b8c415bc45d.
15. Fix the REPLACE command so that it is no longer able to sneak a
NULL value into a NOT NULL column even if the NOT NULL column has a default
value of NULL.
Ticket e6f1f2e34dceeb1ed6
16. Fix a problem with the use of window functions used within
correlated subqueries.
Ticket d0866b26f83e9c55e3
17. Fix the ALTER TABLE RENAME COLUMN command so that it works for tables
that have redundant UNIQUE constraints.
Ticket bc8d94f0fbd633fd9a
18. Fix a bug that caused zeroblob values to be truncated when inserted into
a table that uses an expression index.
Ticket bb4bdb9f7f654b0bb9
**Hashes:**
19. SQLITE_SOURCE_ID: "2019-02-07 17:02:52 97744701c3bd414e6c9d7182639d8c2ce7cf124c4fce625071ae65658ac61713
"
20. SHA3-256 for sqlite3.c: ca011a10ee8515b33e5643444b98ee3d74dc45d3ac766c3700320def52bc6aba

### 2018-12-01 (3.26.0)

1. Optimization: When doing an UPDATE on a table with indexes on expressions,
    do not update the expression indexes if they do not refer to any of the columns
    of the table being updated.
2. Allow the xBestIndex() method of virtual table implementations to return
    SQLITE_CONSTRAINT to indicate that the proposed query plan is unusable and
    should not be given further consideration.
3. Added the SQLITE_DBCONFIG_DEFENSIVE option which disables the ability to
    create corrupt database files using ordinary SQL.
4. Added support for read-only shadow tables when the SQLITE_DBCONFIG_DEFENSIVE
    option is enabled.
5. Added the PRAGMA legacy_alter_table command, which if enabled causes the
    ALTER TABLE command to behave like older version of SQLite (prior to
    version 3.25.0) for compatibility.
6. Added PRAGMA table_xinfo that works just like PRAGMA table_info
    except that it also shows hidden columns in virtual tables.
7. Added the explain virtual table
    as a run-time loadable extension.
8. Add a limit counter to the query planner to prevent excessive
    sqlite3_prepare() times for certain pathological SQL inputs.
9. Added support for the sqlite3_normalized_sql() interface, when compiling
    with SQLITE_ENABLE_NORMALIZE.
10. Enhanced triggers so that they can use table-valued functions that
    exist in schemas other than the schema where the trigger is defined.
11. Enhancements to the CLI:
  1. Improvements to the ".help" command.
  
  2. The SQLITE_HISTORY environment variable, if it exists,
      specifies the name of the command-line editing history file
  
  3. The --deserialize option associated with opening a new database cause the
      database file to be read into memory and accessed using the
      sqlite3_deserialize() API. This simplifies running tests on a database
      without modifying the file on disk.
12. Enhancements to the geopoly extension:
  1. Always stores polygons
      using the binary format, which is faster and uses less space.
  
  2. Added the geopoly_regular() function.
  
  3. Added the geopoly_ccw() function.
13. Enhancements to the session extension:
  1. Added the SQLITE_CHANGESETAPPLY_INVERT flag
  
  2. Added the sqlite3changeset_start_v2() interface and the
      SQLITE_CHANGESETSTART_INVERT flag.
  
  3. Added the
      changesetfuzz.c
      test-case generator utility.
 **Hashes:**
14. SQLITE_SOURCE_ID: "2018-12-01 12:34:55 bf8c1b2b7a5960c282e543b9c293686dccff272512d08865f4600fb58238b4f9"
15. SHA3-256 for sqlite3.c: 72c08830da9b5d1cb397c612c0e870d7f5eb41a323b41aa3d8aa5ae9ccedb2c4

### 2018-11-05 (3.25.3)

1.  Disallow the use of window functions in the recursive part of
     a CTE. Ticket e8275b415a2f03bee
2.  Fix the behavior of typeof() and length() on virtual tables.  Ticket
     69d642332d25aa3b7315a6d385
3.  Strengthen defenses against deliberately corrupted database files.
4.  Fix a problem in the query planner that results when a row-value expression
     is used with a PRIMARY KEY with redundant columns.  Ticket
     1a84668dcfdebaf12415d
5.  Fix the query planner so that it works correctly for IS NOT NULL operators
     in the ON clause of a LEFT JOIN with the SQLITE_ENABLE_STAT4 compile-time option.
     65eb38f6e46de8c75e188a17ec
**Hashes:**
6. SQLITE_SOURCE_ID: "2018-11-05 20:37:38 89e099fbe5e13c33e683bef07361231ca525b88f7907be7092058007b75036f2"
7. SHA3-256 for sqlite3.c: 45586e4df74de3a43f3a1f8c7a78c3c3f02edce01af7d10cafe68bb94476a5c5

### 2018-09-25 (3.25.2)

1.  Add the PRAGMA legacy_alter_table=ON command that causes the
     "ALTER TABLE RENAME" command to behave as it did in SQLite versions 3.24.0
     and earlier: references to the renamed table inside the bodies of triggers
     and views are not updated.  This new pragma provides a compatibility
     work around for older programs that expected the older, wonky behavior
     of ALTER TABLE RENAME.
2.  Fix a problem with the new window functions implementation that caused
     a malfunction when complicated expressions involving window functions were used
     inside of a view.
3.  Fixes for various other compiler warnings and minor problems associated
     with obscure configurations.
**Hashes:**
4. SQLITE_SOURCE_ID: "2018-09-25 19:08:10 fb90e7189ae6d62e77ba3a308ca5d683f90bbe633cf681865365b8e92792d1c7"
5. SHA3-256 for sqlite3.c: 34c23ff91631ae10354f8c9d62fd7d65732b3d7f3acfd0bbae31ff4a62fe28af

### 2018-09-18 (3.25.1)

1.  Extra sanity checking added to ALTER TABLE in the 3.25.0 release
     sometimes raises a false-positive
     when the table being modified has a trigger that
     updates a virtual table.  The false-positive caused the ALTER
     TABLE to rollback, thus leaving the schema unchanged.
     Ticket b41031ea2b537237.
2.  The fix in the 3.25.0 release for the endless-loop in the byte-code
     associated with the ORDER BY LIMIT optimization did not work for
     some queries involving window functions.  An additional correction
     is required.  Ticket
     510cde277783b5fb
**Hashes:**
3. SQLITE_SOURCE_ID: "2018-09-18 20:20:44 2ac9003de44da7dafa3fbb1915ac5725a9275c86bf2f3b7aa19321bf1460b386"
4. SHA3-256 for sqlite3.c: 1b2302e7a54cc99c84ff699a299f61f069a28e1ed090b89e4430ca80ae2aab06

### 2018-09-15 (3.25.0)

1.  Add support for window functions
2.  Enhancements the ALTER TABLE command:
  1.  Add support for renaming columns within a table using
       ALTER TABLE *table* RENAME COLUMN*oldname* TO*newname* .
  2.  Fix table rename feature so that it also updates references
       to the renamed table in triggers and views.
3.  Query optimizer improvements:
  1.  Avoid unnecessary loads of columns in an aggregate query that
       are not within an aggregate function and that are not part
       of the GROUP BY clause.
  
  2.  The IN-early-out optimization: When doing a look-up on a
       multi-column index and an IN operator is used on a column
       other than the left-most column, then if no rows match against
       the first IN value, check to make sure there exist rows that
       match the columns to the right before continuing with the
       next IN value.
  
  3.  Use the transitive property to try to propagate constant
       values within the WHERE clause.  For example, convert
       "a=99 AND b=a" into "a=99 AND b=99".
4.  Use a separate mutex on every inode in the unix VFS, rather than
     a single mutex shared among them all, for slightly better concurrency
     in multi-threaded environments.
5.  Enhance the PRAGMA integrity_check command for improved detection
     of problems on the page freelist.
6.  Output infinity as 1e999 in the ".dump" command of the
     command-line shell.
7.  Added the SQLITE_FCNTL_DATA_VERSION file-control.
8.  Added the Geopoly module
**Bug fixes:**
9.  The ORDER BY LIMIT optimization might have caused an infinite loop
     in the byte code of the prepared statement under very obscure
     circumstances,
     due to a confluence of minor defects in the query optimizer.
     Fix for ticket
     9936b2fa443fec03ff25
10.  On an UPSERT when the order of constraint checks is rearranged,
     ensure that the affinity transformations on the inserted content
     occur before any of the constraint checks. Fix for ticket
     79cad5e4b2e219dd197242e9e.
11.  Avoid using a prepared statement for ".stats on" command of the
     CLI after it has been closed by the ".eqp full" logicc. Fix for ticket
     7be932dfa60a8a6b3b26bcf76.
12.  The LIKE optimization was generating incorrect byte-code and hence
     getting the wrong answer
     if the left-hand operand has numeric affinity and the right-hand-side
     pattern is '/%' or if the pattern begins with the ESCAPE character.
     Fix for ticket
     c94369cae9b561b1f996d0054b
**Hashes:**
13. SQLITE_SOURCE_ID: "2018-09-15 04:01:47 b63af6c3bd33152742648d5d2e8dc5d5fcbcdd27df409272b6aea00a6f761760"
14. SHA3-256 for sqlite3.c: 989e3ff37f2b5eea8e42205f808ccf0ba86c6ea6aa928ad2c011f33a108ac45d

### 2018-06-04 (3.24.0)

1.  Add support for PostgreSQL-style UPSERT.
2.  Add support for auxiliary columns in r-tree tables.
3.  Add C-language APIs for discovering SQL keywords used by
     SQLite: sqlite3_keyword_count(), sqlite3_keyword_name(), and
     sqlite3_keyword_check().
4.  Add C-language APIs for dynamic strings based on the
     sqlite3_str object.
5.  Enhance ALTER TABLE so that it recognizes "true" and "false" as
     valid arguments to DEFAULT.
6.  Add the sorter-reference optimization as a compile-time option.
     Only available if compiled with SQLITE_ENABLE_SORTER_REFERENCES.
7.  Improve the format of the EXPLAIN QUERY PLAN raw output, so that
     it gives better information about the query plan and about the
     relationships between the various components of the plan.
8.  Added the SQLITE_DBCONFIG_RESET_DATABASE option to the
     sqlite3_db_config() API.
**CLI Enhancements:**
9.  Automatically intercepts the raw EXPLAIN QUERY PLAN
     output and reformats it into an ASCII-art graph.
10.  Lines that begin with "#" and that are not in the middle of an
     SQL statement are interpreted as comments.
11.  Added the --append option to the ".backup" command.
12.  Added the ".dbconfig" command.
**Performance:**
13.  UPDATE avoids unnecessary low-level disk writes when the contents
     of the database file do not actually change.
     For example, "UPDATE t1 SET x=25 WHERE y=?" generates no extra
     disk I/O if the value in column x is already 25.  Similarly,
     when doing UPDATE on records that span multiple pages, only
     the subset of pages that actually change are written to disk.
     This is a low-level performance optimization only and does not
     affect the behavior of TRIGGERs or other higher level SQL
     structures.
14.  Queries that use ORDER BY and LIMIT now try to avoid computing
     rows that cannot possibly come in under the LIMIT. This can greatly
     improve performance of ORDER BY LIMIT queries, especially when the
     LIMIT is small relative to the number of unrestricted output rows.
15.  The OR optimization is allowed to proceed
     even if the OR expression has also been converted into an IN
     expression.  Uses of the OR optimization are now also
     more clearly shown in the EXPLAIN QUERY PLAN output.
16.  The query planner is more aggressive about using
     automatic indexes for views and subqueries for which it is
     not possible to create a persistent index.
17.  Make use of the one-pass UPDATE and DELETE query plans in the
     R-Tree extension where appropriate.
18.  Performance improvements in the LEMON-generated parser.
**Bug fixes:**
19.  For the right-hand table of a LEFT JOIN, compute the values
     of expressions directly rather than loading precomputed values
     out of an expression index as the expression index might
     not contain the correct value.  Ticket
     7fa8049685b50b5aeb0c2
20.  Do not attempt to use terms from the WHERE clause to enable
     indexed lookup of the right-hand table of a LEFT JOIN.  Ticket
     4ba5abf65c5b0f9a96a7a
21.  Fix a memory leak that can occur following a failure to open error
     in the CSV virtual table
22.  Fix a long-standing problem wherein a corrupt schema on the
     sqlite_sequence table used by AUTOINCREMENT can lead to
     a crash. Ticket
     d8dc2b3a58cd5dc2918a1
23.  Fix the json_each() function so that it returns
     valid results on its "fullkey" column when the input is a simple value
     rather than an array or object.
**Hashes:**
24. SQLITE_SOURCE_ID: "2018-06-04 19:24:41 c7ee0833225bfd8c5ec2f9bf62b97c4e04d03bd9566366d5221ac8fb199a87ca"
25. SHA3-256 for sqlite3.c: 0d384704e1c66026228336d1e91771d295bf688c9c44c7a44f25a4c16c26ab3c

### 2018-04-10 (3.23.1)

1.  Fix two problems in the new LEFT JOIN strength reduction optimization.
     Tickets 1e39b966ae9ee739
     and fac496b61722daf2.
2.  Fix misbehavior of the FTS5 xBestIndex method.  Ticket
     2b8aed9f7c9e61e8.
3.  Fix a harmless reference to an uninitialized virtual machine register.
     Ticket 093420fc0eb7cba7.
4.  Fix the CLI so that it builds with -DSQLITE_UNTESTABLE
5.  Fix the eval.c extension
     so that it works with PRAGMA empty_result_callbacks=ON.
6.  Fix the generate_series virtual table so that it correctly returns
     no rows if any of its constraints are NULL.
7.  Performance enhancements in the parser.
**Hashes:**
8. SQLITE_SOURCE_ID: "2018-04-10 17:39:29 4bb2294022060e61de7da5c227a69ccd846ba330e31626ebcd59a94efd148b3b"
9. SHA3-256 for sqlite3.c: 65750d1e506f416a0b0b9dd22d171379679c733e3460549754dc68c92705b5dc

### 2018-04-02 (3.23.0)

1.  Add the sqlite3_serialize() and sqlite3_deserialize() interfaces when
     the SQLITE_ENABLE_DESERIALIZE compile-time option is used.
2.  Recognize TRUE and FALSE as constants.  (For compatibility, if there
     exist columns named "true" or "false", then the identifiers refer to the
     columns rather than Boolean constants.)
3.  Support operators IS TRUE, IS FALSE, IS NOT TRUE, and IS NOT FALSE.
4.  Added the SQLITE_DBSTATUS_CACHE_SPILL option to sqlite3_db_status() for
     reporting the number of cache spills that have occurred.
5.  The "alternate-form-2" flag ("!") on the built-in printf implementation
     now causes string substitutions to measure the width and precision in
     characters instead of bytes.
6.  If the xColumn method in a virtual table implementation returns
     an error message using sqlite3_result_error() then give that error
     message preference over internally-generated messages.
7.  Added the -A command-line option to the CLI to make it easier to manage
     SQLite Archive files.
8.  Add support for INSERT OR REPLACE, INSERT OR IGNORE, and UPDATE OR REPLACE
     in the Zipfile virtual table.
9.  Enhance the sqlite3changeset_apply() interface so that it is hardened
     against attacks from deliberately corrupted changeset objects.
10.  Added the sqlite3_normalize()
     extension function.
11.  Query optimizer enhancements:
  1.  Improve the omit-left-join optimization so that it works in cases where
       the right-hand table is UNIQUE but not necessarily NOT NULL.
  
  2.  Improve the push-down optimization so that it works for many LEFT JOINs.
  
  3.  Add the LEFT JOIN strength reduction optimization that converts a LEFT
       JOIN into an ordinary JOIN if there exist terms in the WHERE clause
       that would prevent the extra all-NULL row of the LEFT JOIN from
       appearing in the output set.
  
  4.  Avoid unnecessary writes to the sqlite_sequence table when an
       AUTOINCREMENT table is updated with an rowid that is less than the
       maximum.
12.  Bug fixes:
  1.  Fix the parser to accept valid row value syntax.
       Ticket 7310e2fb3d046a5
  
  2.  Fix the query planner so that it takes into account dependencies in
       the arguments to table-valued functions in subexpressions in
       the WHERE clause.
       Ticket 80177f0c226ff54
  
  3.  Fix incorrect result with complex OR-connected WHERE and STAT4.
       Ticket ec32177c99ccac2
  
  4.  Fix potential corruption in indexes on expressions due to automatic
       datatype conversions.
       Ticket 343634942dd54ab
  
  5.  Assertion fault in FTS4.
       Ticket d6ec09eccf68cfc
  
  6.  Incorrect result on the less-than operator in row values.
       Ticket f484b65f3d62305
  
  7.  Always interpret non-zero floating-point values as TRUE, even if
       the integer part is zero.
       Ticket 36fae083b450e3a
  
  8.  Fix an issue in the fsdir(PATH) table-valued function to the
       fileio.c extension,
       that caused a segfault if the fsdir() table was used as the inner table
       of a join. Problem reported on the mailing list and fixed by check-in
       7ce4e71c1b7251be
  
  9.  Issue an error rather instead of an assertion-fault or null-pointer
       dereference when the sqlite_master table is corrupted so that the
       sqlite_sequence table root page is really a btree-index page. Check-in
       525deb7a67fbd647
  
  10.  Fix the ANALYZE command so that it computes statistics on tables
       whose names begin with "sqlite".  Check-in
       0249d9aecf69948d
13.  Additional fixes for issues detected by
     OSSFuzz:
  1.  Fix a possible infinite loop on VACUUM for corrupt database files.
       Check-in 27754b74ddf64
  
  2.  Disallow parameters in the WITH clause of triggers and views.
       Check-in b918d4b4e546d
  
  3.  Fix a potential memory leak in row value processing.
       Check-in 2df6bbf1b8ca8
  
  4.  Improve the performance of the replace() SQL function for cases where
       there are many substitutions on megabyte-sized strings, in an attempt
       to avoid OSSFuzz timeouts during testing.
       Check-in fab2c2b07b5d3
  
  5.  Provide an appropriate error message when the sqlite_master table
       contains a CREATE TABLE AS statement. Formerly this caused either an
       assertion fault or null pointer dereference.  Problem found by OSSFuzz
       on the GDAL project. Check-in
       d75e67654aa96
  
  6.  Incorrect assert() statement removed. Check-in
       823779d31eb09cda.
  
  7.  Fix a problem with using the LIKE optimization on an
       INTEGER PRIMARY KEY.  Check-in
       b850dd159918af56.
 **Hashes:**
14. SQLITE_SOURCE_ID: "2018-04-02 11:04:16 736b53f57f70b23172c30880186dce7ad9baa3b74e3838cae5847cffb98f5cd2"
15. SHA3-256 for sqlite3.c: 4bed3dc2dc905ff55e2c21fd2725551fc0ca50912a9c96c6af712a4289cb24fa

### 2018-01-22 (3.22.0)

1.  The output of sqlite3_trace_v2() now shows each individual SQL statement
     run within a trigger.
2.  Add the ability to read from WAL mode databases even if the application
     lacks write permission on the database and its containing directory, as long as
     the -shm and -wal files exist in that directory.
3.  Added the rtreecheck() scalar SQL function to the R-Tree extension.
4.  Added the sqlite3_vtab_nochange() and sqlite3_value_nochange() interfaces
     to help virtual table implementations optimize UPDATE operations.
5.  Added the sqlite3_vtab_collation() interface.
6.  Added support for the "^" initial token syntax in FTS5.
7.  New extensions:
  1.  The Zipfile virtual table can read and write a
       ZIP Archive.
  
  2.  Added the fsdir(PATH) table-valued function to the
       fileio.c extension,
       for listing the files in a directory.
  
  3.  The sqlite_btreeinfo
       eponymous virtual table for introspecting and estimating the sizes of
       the btrees in a database.
  
  4.  The Append VFS is a
       VFS shim that allows an SQLite database to be appended to some other
       file.  This allows (for example) a database to be appended to an
       executable that then opens and reads the database.
8.  Query planner enhancements:
  1.  The optimization that uses an index to quickly compute an
       aggregate min() or max() is extended to work with
       indexes on expressions.
  
  2.  The decision of whether to implement a FROM-clause subquery
       as a co-routine or using query flattening
       now considers whether
       the result set of the outer query is "complex" (if it
       contains functions or expression subqueries).  A complex result
       set biases the decision toward the use of co-routines.
  
  3.  The planner avoids query plans that use indexes with unknown
       collating functions.
  
  4.  The planner omits unused LEFT JOINs even if they are not the
       right-most joins of a query.
9.  Other performance optimizations:
  1.  A smaller and faster implementation of text to floating-point
       conversion subroutine: sqlite3AtoF().
  
  2.  The Lemon parser generator creates a faster parser.
  
  3.  Use the strcspn() C-library routine to speed up the LIKE and
       GLOB operators.
10.  Improvements to the command-line shell:
  1.  The ".schema" command shows the structure of virtual tables.
  
  2.  Added support for reading and writing
       SQLite Archive files using
       the .archive command.
  
  3.  Added the experimental .expert command
  
  4.  Added the ".eqp trigger" variant of the ".eqp" command
  
  5.  Enhance the ".lint fkey-indexes" command so that it works with
       WITHOUT ROWID tables.
  
  6.  If the filename argument to the shell is a ZIP archive rather than
       an SQLite database, then the shell automatically opens that ZIP
       archive using the Zipfile virtual table.
  
  7.  Added the edit() SQL function.
  
  8.  Added the .excel command to simplify exporting
       database content to a spreadsheet.
  
  9.  Databases are opened using
       Append VFS when
       the --append flag is used on the command line or with the
       .open command.
11.  Enhance the SQLITE_ENABLE_UPDATE_DELETE_LIMIT compile-time option so
     that it works for WITHOUT ROWID tables.
12.  Provide the sqlite_offset(X) SQL function that returns
     the byte offset into the database file to the beginning of the record
     holding value X, when compiling with -DSQLITE_ENABLE_OFFSET_SQL_FUNC.
13.  Bug fixes:
  1.  Infinite loop on an UPDATE that uses an OR operator in the WHERE clause.
       Problem introduced with 3.17.0 and reported on the mailing list about
       one year later.  Ticket
       47b2581aa9bfecec.
  
  2.  Incorrect query results when the skip-ahead-distinct optimization is
       used.
       Ticket ef9318757b152e3a.
  
  3.  Incorrect query results on a join with a ORDER BY DESC.  Ticket
       123c9ba32130a6c9.
  
  4.  Inconsistent result set column names between CREATE TABLE AS
       and a simple SELECT.  Ticket
       3b4450072511e621
  
  5.  Assertion fault when doing REPLACE on an index on an expression.
       Ticket dc3f932f5a147771
  
  6.  Assertion fault when doing an IN operator on a constant index.
       Ticket aa98619ad08ddcab
 **Hashes:**
14. SQLITE_SOURCE_ID: "2018-01-22 18:45:57 0c55d179733b46d8d0ba4d88e01a25e10677046ee3da1d5b1581e86726f2171d"
15. SHA3-256 for sqlite3.c: 206df47ebc49cd1710ac0dd716ce5de5854826536993f4feab7a49d136b85069

### 2017-10-24 (3.21.0)

1.  Take advantage of the atomic-write capabilities in the
     F2FS filesystem when available, for
     greatly reduced transaction overhead.  This currently requires the
     SQLITE_ENABLE_BATCH_ATOMIC_WRITE compile-time option.
2.  Allow ATTACH and DETACH commands to work inside of a transaction.
3.  Allow WITHOUT ROWID virtual tables to be writable if the PRIMARY KEY
     contains exactly one column.
4.  The "fsync()" that occurs after the header is written in a WAL reset
     now uses the sync settings for checkpoints.  This means it will use a
     "fullfsync" on macs if PRAGMA checkpoint_fullfsync set on.
5.  The sqlite3_sourceid() function tries to detect if the source code has
     been modified from what is checked into version control and if there are
     modifications, the last four characters of the version hash are shown as
     "alt1" or "alt2".  The objective is to detect accidental and/or careless
     edits.  A forger can subvert this feature.
6.  Improved de-quoting of column names for CREATE TABLE AS statements with
     an aggregate query on the right-hand side.
7.  Fewer "stat()" system calls issued by the unix VFS.
8.  Enhanced the LIKE optimization so that it works with an ESCAPE clause.
9.  Enhanced PRAGMA integrity_check and PRAGMA quick_check to detect obscure
     row corruption that they were formerly missing.  Also update both pragmas
     so that they return error text rather than SQLITE_CORRUPT when encountering
     corruption in records.
10.  The query planner now prefers to implement FROM-clause subqueries using
     co-routines rather using the query flattener optimization.  Support for
     the use of co-routines for subqueries may no longer be disabled.
11.  Pass information about !=, IS, IS NOT, NOT NULL, and IS NULL constraints
     into the xBestIndex method of virtual tables.
12.  Enhanced the CSV virtual table so that it accepts the last row of
     input if the final new-line character is missing.
13.  Remove the rarely-used "scratch" memory allocator.  Replace it with the
     SQLITE_CONFIG_SMALL_MALLOC configuration setting that gives SQLite
     a hint that large memory allocations should be avoided when possible.
14.  Added the
     swarm virtual table
     to the existing union virtual table extension.
15.  Added the
     sqlite_dbpage virtual table
     for providing direct access to pages
     of the database file.  The source code is built into the amalgamation and
     is activated using the -DSQLITE_ENABLE_DBPAGE_VTAB compile-time option.
16.  Add a new type of fts5vocab virtual table - "instance" - that provides
     direct access to an FTS5 full-text index at the lowest possible level.
17.  Remove a call to rand_s() in the Windows VFS since it was causing problems
     in Firefox on some older laptops.
18.  The src/shell.c source code
     to the command-line shell is no longer under version control.  That file
     is now generated as part of the build process.
19.  Miscellaneous microoptimizations reduce CPU usage by about 2.1%.
20.  Bug fixes:
  1.  Fix a faulty assert() statement discovered by OSSFuzz.
       Ticket cb91bf4290c211d
  
  2.  Fix an obscure memory leak in sqlite3_result_pointer().
       Ticket 7486aa54b968e9b
  
  3.  Avoid a possible use-after-free error by deferring schema resets until
       after the query planner has finished running.
       Ticket be436a7f4587ce5
  
  4.  Only use indexes-on-expressions to optimize ORDER BY or GROUP BY if
       the COLLATE is correct.
       Ticket e20dd54ab0e4383
  
  5.  Fix an assertion fault that was coming up when the expression in an
       index-on-expressions is really a constant.
       Ticket aa98619ad08ddca
  
  6.  Fix an assertion fault that could occur following
       PRAGMA reverse_unordered_selects.
       Ticket cb91bf4290c211d
  
  7.  Fix a segfault that can occur for queries that use table-valued functions
       in an IN or EXISTS subquery.
       Ticket b899b6042f97f5
  
  8.  Fix a potential integer overflow problem when compiling a particular
       horrendous common table expression.  This was another problem discovered
       by OSSFuzz.  Check-in 6ee8cb6ae5.
  
  9.  Fix a potential out-of-bound read when querying a corrupt database file,
       a problem detected by Natalie Silvanovich of Google Project Zero.
       Check-in 04925dee41a21f.
 **Hashes:**
21. SQLITE_SOURCE_ID: "2017-10-24 18:55:49 1a584e499906b5c87ec7d43d4abce641fdf017c42125b083109bc77c4de48827"
22. SHA3-256 for sqlite3.c: 84c181c0283d0320f488357fc8aab51898370c157601459ebee49d779036fe03

### 2017-08-24 (3.20.1)

1.  Fix a potential memory leak in the new sqlite3_result_pointer() interface.
     Ticket 7486aa54b968e9b5.
**Hashes:**
2. SQLITE_SOURCE_ID: "2017-08-24 16:21:36 8d3a7ea6c5690d6b7c3767558f4f01b511c55463e3f9e64506801fe9b74dce34"
3. SHA3-256 for sqlite3.c: 93b1a6d69b48dc39697d1d3a1e4c30b55da0bdd2cad0c054462f91081832954a

### 2017-08-01 (3.20.0)

1.  Update the text of error messages returned by sqlite3_errmsg() for some
     error codes.
2.  Add new pointer passing interfaces.
3.  Backwards-incompatible changes to some extensions in order to take
     advantage of the improved security offered by the new
     pointer passing interfaces:
       1.  Extending FTS5 → requires sqlite3_bind_pointer() to find
          the fts5_api pointer.
     
  2.  carray(PTR,N) → requires sqlite3_bind_pointer() to set the PTR parameter.
     
  3.  remember(V,PTR)
          → requires sqlite3_bind_pointer() to set the PTR parameter.
     
4.  Added the SQLITE_STMT virtual table extension.
5.  Added the COMPLETION extension - designed to suggest
     tab-completions for interactive user interfaces.  This is a work in progress.
     Expect further enhancements in future releases.
6.  Added the UNION virtual table extension.
7.  The built-in date and time functions have been enhanced so that they can be
     used in CHECK constraints, in indexes on expressions, and in the WHERE clauses
     of partial indexes, provided that they do not use the 'now', 'localtime', or
     'utc' keywords.  More information.
8.  Added the sqlite3_prepare_v3() and sqlite3_prepare16_v3() interfaces
     with the extra "prepFlags" parameters.
9.  Provide the SQLITE_PREPARE_PERSISTENT flag for sqlite3_prepare_v3() and
     use it to limit lookaside memory misuse by FTS3, FTS5, and the
     R-Tree extension.
10.  Added the PRAGMA secure_delete=FAST command.  When secure_delete is
     set to FAST, old content is overwritten with zeros as long as that does
     not increase the amount of I/O.  Deleted content might still persist on
     the free-page list but will be purged from all b-tree pages.
11.  Enhancements to the command-line shell:
  1.  Add support for tab-completion using the COMPLETION extension, for
       both readline and linenoise.
  
  2.  Add the ".cd" command.
  
  3.  Enhance the ".schema" command to show the schema of all attached
       databases.
  
  4.  Enhance ".tables" so that it shows the schema names for all attached
       if the name is anything other than "main".
  
  5.  The ".import" command ignores an initial UTF-8 BOM.
  
  6.  Added the "--newlines" option to the ".dump" command to cause U+000a and
       U+000d characters to be output literally rather than escaped using the
       replace() function.
12.  Query planner enhancements:
  1.  When generating individual loops for each ORed term of an OR scan,
       move any constant WHERE expressions outside of the loop, as is
       done for top-level loops.
  
  2.  The query planner examines the values of bound parameters to help
       determine if a partial index is usable.
  
  3.  When deciding between two plans with the same estimated cost, bias
       the selection toward the one that does not use the sorter.
  
  4.  Evaluate WHERE clause constraints involving correlated subqueries
       last, in the hope that they never have be evaluated at all.
  
  5.  Do not use the flattening optimization for a sub-query on the RHS
       of a LEFT JOIN if that subquery reads data from a virtual table as
       doing so prevents the query planner from creating automatic indexes
       on the results of the sub-query, which can slow down the query.
13.  Add SQLITE_STMTSTATUS_REPREPARE, SQLITE_STMTSTATUS_RUN,
     and SQLITE_STMTSTATUS_MEMUSED options for the
     sqlite3_stmt_status() interface.
14.  Provide PRAGMA functions for
     PRAGMA integrity_check, PRAGMA quick_check, and
     PRAGMA foreign_key_check.
15.  Add the -withoutnulls option to the TCL interface eval method.
16.  Enhance the sqlite3_analyzer.exe utility program so that it shows
     the number of bytes of metadata on btree pages.
17.  The SQLITE_DBCONFIG_ENABLE_QPSG run-time option and the
     SQLITE_ENABLE_QPSG compile-time option enable the
     query planner stability guarantee.  See also ticket
     892fc34f173e99d8
18.  Miscellaneous optimizations result in a 2% reduction in CPU cycles used.
**Bug Fixes:**
19.  Fix the behavior of sqlite3_column_name() for queries that use the
