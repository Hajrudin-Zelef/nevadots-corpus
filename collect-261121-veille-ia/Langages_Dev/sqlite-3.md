---
id: collect-261121-veille-ia/langages-dev/sqlite-3
title: "SQLite"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["check-in", "user", "tags", "trunk", "stephan", "sqlite", "database", "broken-lock-defense-1"]
source: https://www.sqlite.org/src/timeline?bt=version-3.47.0&p=trunk&advm=1&udc=1
source_anchor: ""
source_lines: [3001, 4500]
section: "Langages_Dev"
---
| 10:36 |  | Prevent a harmless signed integer overflow in the CLI if the argument to the --skip option of the ".inport" command is -9223372036854775808. (check-in: 247277d937 ... user: drh tags: trunk) | 
| 02:21 |  | A experiment addressing the embedded-NUL behavior of sqlite3_colum/value_text() in the JS bindings, as discussed in forum post d77281aec2. This is an experiment only. It works, in that existing tests do not see the difference, but it has implicications which require careful consideration before deploying. (check-in: d0e803b903 ... user: stephan tags: wasm-post-3.51) | 
| 01:43 |  | Remove an OMIT_WAL from the wasm build because it breaks with the current amalgamation (and isn't strictly needed here anyway). (check-in: a7be7d155a ... user: stephan tags: trunk) | 
| 2025-10-29 |  |  | 
| 20:57 |  | Experimental change to allow triggers in the temp schema to use fully qualified table names in INSERT/UPDATE/DELETE statements. (check-in: 48e168007c ... user: dan tags: temp-trigger-refs) | 
| 20:47 |  | Fix a logic error introduced by [cea8bf79e18d55a8] which caused "0 OR 2" to be evaluated as "2" instead of as "1". Problem reported at forum post d5f32040c. (check-in: 095cc4f22e ... user: drh tags: trunk) | 
| 13:03 |  | Striving for better names for fields in the spec structure. (check-in: 9644e2a8db ... user: drh tags: qrf) | 
| 11:19 |  | Documentation tweaks. Change the name of the eQuote setting to eText for consistency. (check-in: 6123267501 ... user: drh tags: qrf) | 
| 2025-10-28 |  |  | 
| 22:25 |  | Add support for the bTxtJsonb flag in sqlite3_qrf_spec, which if true causes JSONB blobs to be displayed as JSON text. (check-in: 3fcf9ecb03 ... user: drh tags: qrf) | 
| 22:09 |  | Tiny cosmetic tweak to configure script output. No functional changes. (check-in: e2c20aa592 ... user: stephan tags: trunk) | 
| 19:57 |  | Typo fixes in LICENSE.md from BrickViking. (check-in: 26ddec2916 ... user: stephan tags: trunk) | 
| 19:37 |  | Fix a harmless compiler warning. (check-in: d00c549126 ... user: drh tags: qrf) | 
| 19:35 |  | Merge the latest trunk enhancements into the qrf branch. (check-in: fa5661ea8b ... user: drh tags: qrf) | 
| 13:24 |  | Fix an signed integer overflow problem that can occur when using an oversize value for the SQLITE_MAX_LENGTH compile-time option together with multi-gigabyte operands to the OP_Concat operator. (check-in: 724f2299f2 ... user: drh tags: trunk) | 
| 10:50 |  | Add enhanced resistance to corruption after posix advisory locks on the database file been unintentionally broken on unix by running close(open(DBFILE)) from a separate thread. (check-in: 6385a1962c ... user: drh tags: trunk) | 
| 10:44 |  | Enhance the CLI so that it can handle inputs larger than 2 billion lines. (check-in: 5daebf2d60 ... user: drh tags: trunk) | 
| 01:28 |  | Additional comments explaining what unixIsSharingShmNode() does. No changes to code. (closed check-in: 819fc87385 ... user: drh tags: broken-lock-defense-1) | 
| 2025-10-27 |  |  | 
| 23:57 |  | Omit unnecessary branches from the unixIsSharingShmNode() implementation. (check-in: 2d5623bc27 ... user: drh tags: broken-lock-defense-1) | 
| 15:44 |  | The unixIsSharingShmNode() function should always return false for the unix-excl VFS. (check-in: 770f35dae4 ... user: drh tags: broken-lock-defense-1) | 
| 14:24 |  | Implement an experimental defensive measure to try to prevent the -wal and -shm files of an active WAL-mode database connection from being deleted out from under the database connection in the case where the database SHARED lock has been released by an accidental call to close() in a different thread, or similar. (check-in: ed1b920664 ... user: drh tags: broken-lock-defense-1) | 
| 11:39 |  | Fix a minor over-allocation of heap memory in wal.c. (check-in: 1803cb9bb6 ... user: dan tags: trunk) | 
| 11:20 |  | Fix sanitizer complaints in the regexp extension. (check-in: 6ff9ecb89d ... user: dan tags: trunk) | 
| 2025-10-26 |  |  | 
| 21:58 |  | Merge the OPFS/SEE tests into trunk (where they should have been committed). (check-in: f8a4d7abf0 ... user: stephan tags: trunk) | 
| 21:45 |  | Demonstrate the opfs-sahpool VFS with SEE. (check-in: b4cffc00f3 ... user: stephan tags: wasm-post-3.51) | 
| 21:30 |  | Add a test which uses SEE via the "opfs" VFS. "opfs-sahpool" is pending. (check-in: 1eb5a7ea39 ... user: stephan tags: wasm-post-3.51) | 
| 18:43 |  | Merge trunk into the wasm-post-3.51 branch. (check-in: be5d2bae50 ... user: stephan tags: wasm-post-3.51) | 
| 18:36 |  | A better/cleaner fix for [1c80ff88c0ce]. (check-in: 6138043bdd ... user: stephan tags: trunk) | 
| 17:59 |  | c-pp-lite: Fix another newline-related off-by-one which could cause newlines to get lost in @token@ filtering. Problem found in out-of-tree testing. (check-in: 1c80ff88c0 ... user: stephan tags: trunk) | 
| 12:34 |  | Doc typo fix reported in the forum. No code changes. (check-in: 6e065b6aa0 ... user: stephan tags: trunk) | 
| 2025-10-25 |  |  | 
| 22:22 |  | Add the SQLITE_FCNTL_FILESTAT file control and the sqlite_filestat() SQL function. (check-in: 9870afcb2a ... user: drh tags: trunk) | 
| 20:46 |  | Rename the file control to SQLITE_FCNTL_FILESTAT. Rename the SQL function to sqlite_filestat(). Rename the enable macro to SQLITE_ENABLE_FILESTAT. (closed check-in: ee797fec4e ... user: drh tags: file-info) | 
| 19:51 |  | Fix a bug in sqlite_file_info() output: It was showing the "pal" for the main file descriptor when traversing other unused file descriptors for the same inode. (check-in: 3c4020d76e ... user: drh tags: file-info) | 
| 19:36 |  | New --normal option for the ".open" command in the CLI. Also, when opening the database for deduceDatabaseType(), use the same open-flags that will be used subsequently for the official open, to avoid creating unusable file descriptors that need to be held until all database instances close. (check-in: 5e20a0435b ... user: drh tags: file-info) | 
| 19:26 |  | Add additional information about unix file descriptors to the same database file in the sqlite_file_info() output. (check-in: 64045f0a4d ... user: drh tags: file-info) | 
| 18:40 |  | Adjust unixPosixAdvisorLocks() to use redirectable system calls so that it can be tested. (check-in: eafe88b782 ... user: drh tags: file-info) | 
| 18:36 |  | Merge the CLI deduceDatabaseType fix from trunk. (check-in: 82f907016e ... user: drh tags: file-info) | 
| 18:35 |  | Fix the CLI so that it does not try to open what might be an SQLite database using open() in the deduceDatabaseType() routine, as this can lead to broken posix advisory locks. (check-in: 2a3a02715f ... user: drh tags: trunk) | 
| 14:06 |  | Additional information about the unixShm shown. (check-in: 85846c7fbe ... user: drh tags: file-info) | 
| 13:55 |  | Do not attempt to show posix-advisory-locking status on journal files. (check-in: 7f8baf64bb ... user: drh tags: file-info) | 
| 13:16 |  | The sqlite_file_info() function should always return JSON, unless there is an OOM. (check-in: 65aed79c29 ... user: drh tags: file-info) | 
| 12:53 |  | New symbolic representation of posix advisory locking information (check-in: b7d991ae04 ... user: drh tags: file-info) | 
| 11:47 |  | Merge trunk enhancements into the file-info branch. (check-in: 73e1e081e9 ... user: drh tags: file-info) | 
| 11:37 |  | Stricter assert() statements on unixShmSystemLock(). (check-in: ed01ed337c ... user: drh tags: trunk) | 
| 2025-10-24 |  |  | 
| 23:12 |  | Improvements to the design of the /proc lock analysis. (check-in: f2809dd3b2 ... user: drh tags: file-info) | 
| 21:01 |  | Random tiny tweaks in internal JS and flesh out the tester1 test selection list to include both 32-/64-bit entries for all tester1XYZ.html combinations. For after the 3.51 release. (check-in: c087a1d23d ... user: stephan tags: wasm-post-3.51) | 
| 20:29 |  | Make it particularly clear in the wasm build output that unsupported builds are exactly that. Cosmetic tweak only, no functional changes. (check-in: 86f53a2eb3 ... user: stephan tags: trunk) | 
| 20:26 |  | Easier to read /proc lock interpretation. (check-in: d7bcc54beb ... user: drh tags: file-info) | 
| 20:04 |  | Flesh out the sqlite_file_info() function further, and in particular show locks based in /proc/PID/fdinfo/FD, when that information is available. (check-in: ce75111cce ... user: drh tags: file-info) | 
| 17:42 |  | New diagnostic SQL function "sqlite_file_info()" and its associated file-control SQLITE_FCNTL_GET_INFO. (check-in: 56ab969258 ... user: drh tags: file-info) | 
| 17:16 |  | Fix a duplicate SQLITE_API on sqlite3_carray_bind(), as reported in forum post 7499e8a19e. (check-in: 62917cd429 ... user: stephan tags: trunk) | 
| 16:39 |  | ext/wasm: dependencies fix for parallel builds attempted immediately after a distclean. Add version-info to the distclean rules. (check-in: b99233fe3d ... user: stephan tags: trunk) | 
| 16:25 |  | ext/wasm/GNUmakefile: (A) When using a custom sqlite3.c, use sqlite3.h from the same directory instead of the top of this tree. (B) Re-add preprocessor flags for SEE which were misplaced during recent build refactoring. (check-in: 1b1e530a91 ... user: stephan tags: trunk) | 
| 16:01 |  | Fix [4e713558f6c6ea3d] so that it supports separate builds of rtree. (check-in: d030e955d7 ... user: drh tags: trunk) | 
| 15:18 |  | Document minor discrepancies in how sqlite3_bind_pointer() and sqlite3_carray_bind() react to the SQLITE_TRANSIENT and SQLITE_STATIC finalizers. (check-in: b92478c389 ... user: stephan tags: trunk) | 
| 14:30 |  | Make explicit that sqlite3_bind_pointer() and sqlite3_carray_bind() call the provided destructor even if those routines fail, per forum feedback. (check-in: 1fcb0b6079 ... user: stephan tags: trunk) | 
| 13:23 |  | Update the MSVC makefile for the amalgamation distribution. (check-in: 440ce1cff2 ... user: drh tags: trunk) | 
| 13:18 |  | Enhance the tokenizer so that it is able to deal with individual tokens larger than 2GiB. (check-in: 4e713558f6 ... user: drh tags: trunk) | 
| 12:32 |  | In the CLI, improved error messages on input errors. Limit the size of a single SQL statement to a little more than 2GB. (check-in: 0b50e7e10a ... user: drh tags: trunk) | 
| 09:24 |  | Limit the size of --hexdb files in the CLI to 2 billion lines, to avoid overflowing the line number counter. (check-in: 2adfd0f47b ... user: drh tags: trunk) | 
| 2025-10-23 |  |  | 
| 23:07 |  | Change the documentation name to README.md so that it displays automatically when browsing the source directory. (check-in: 0cda773e76 ... user: drh tags: qrf) | 
| 23:03 |  | Bring the documentation into sync with the current implementation. (check-in: b14e05759a ... user: drh tags: qrf) | 
| 20:13 |  | Add support for QUOTE and EXPLAIN modes. (check-in: c4d612a572 ... user: drh tags: qrf) | 
| 18:57 |  | Add support for EQP mode. (check-in: 61b2011d8e ... user: drh tags: qrf) | 
| 17:09 |  | Add CSV mode. (check-in: bf0fec0d49 ... user: drh tags: qrf) | 
| 16:57 |  | Implement LINE mode. (check-in: b4c4ba0ad5 ... user: drh tags: qrf) | 
| 16:40 |  | Add HTML and INSERT modes. (check-in: 6d695ecdfd ... user: drh tags: qrf) | 
| 16:22 |  | Implement JSON mode. (check-in: c3b6339162 ... user: drh tags: qrf) | 
| 14:05 |  | Additional defenses against over-sized inputs in the (unused) amatch.c demonstration code. (check-in: b9f6ae0767 ... user: drh tags: trunk) | 
| 13:10 |  | Fix a faulty error message in the ext/misc/fileio.c extension. (check-in: 1fdef271cd ... user: drh tags: trunk) | 
| 2025-10-22 |  |  | 
| 21:24 |  | Assigned widths and alignments on columns. (check-in: f6359f2fa7 ... user: drh tags: qrf) | 
| 21:00 |  | Column modes are now working. (check-in: ed1c8a62ff ... user: drh tags: qrf) | 
| 18:09 |  | Code in place to do column formats. Compiles, but does not work. This is an incremental check-in. (check-in: a05e021354 ... user: drh tags: qrf) | 
| 12:59 |  | Merge va_list misuse fix. (check-in: e33ca68fee ... user: drh tags: qrf) | 
| 12:38 |  | Fix a va_list misuse. (leaf check-in: 3d04412176 ... user: stephan tags: resfmt) | 
| 12:33 |  | Refactor: Name change to "query result formatter" or "QRF". Simplify names. put all the code into its own subdirectory of ext/ (check-in: 8646a2ee22 ... user: drh tags: qrf) | 
| 12:09 |  | Simplify the interface to use just a single API. (check-in: 82dc13ec48 ... user: drh tags: resfmt) | 
| 00:38 |  | Adjust some of the markdown in the LICENSE.md file so that it will hopefully work better on GitHub. (check-in: 773f9d3dcf ... user: drh tags: trunk) | 
| 2025-10-21 |  |  | 
| 20:10 |  | Simplify the set of formatting choices. (check-in: 1f364ea3c0 ... user: drh tags: resfmt) | 
| 19:56 |  | Add TCL and JSON output formatting. Add the ability to test the xRender. (check-in: 1a28eae74e ... user: drh tags: resfmt) | 
| 19:03 |  | Text quoting formats CSV and HTML. (check-in: ea69f1f81f ... user: drh tags: resfmt) | 
| 16:15 |  | Back out [b144f875d6bddce6] and remove the former check for strchrnul() from the configure script. (check-in: f4e059b224 ... user: stephan tags: trunk) | 
| 15:30 |  | Add separate quoting style for BLOB values. (check-in: dfe537175e ... user: drh tags: resfmt) | 
| 13:07 |  | Control-character escaping (check-in: fbb6976455 ... user: drh tags: resfmt) | 
| 11:00 |  | Recognize the various quoting styles and escape modes in the resfmt-test program. (check-in: 3ee2a8775f ... user: drh tags: resfmt) | 
| 2025-10-20 |  |  | 
| 19:55 |  | More capabilities being added. Incremental check-in. (check-in: 9adaf791f8 ... user: drh tags: resfmt) | 
| 17:19 |  | Add test program for resfmt.c. Now compiles and runs. (check-in: 7aaaeea319 ... user: drh tags: resfmt) | 
| 14:28 |  | Notes on the result-formatter utility. None of the code works. These are just ideas I have jotted down as I try to figure out how to do this. (check-in: c38f1c6324 ... user: drh tags: resfmt) | 
| 10:55 |  | Merge recent trunk enhancements into the resfmt branch. (check-in: 20c79a2cc1 ... user: drh tags: resfmt) | 
| 2025-10-19 |  |  | 
| 17:30 |  | Reduce the maximum size of LIKE and GLOB patterns in the ossfuzz.c test module to avoid false-positive infinite loop reports from OSS-Fuzz based on long and complex fuzzer-generated LIKE/GLOB patterns that are not actually infinite loops, but which do take a long time to resolve. (check-in: 53edea286b ... user: drh tags: trunk) | 
| 2025-10-18 |  |  | 
| 11:19 |  | Use 64-bit memory allocations everywhere in the expert extension. (check-in: 6caf793b8f ... user: drh tags: trunk) | 
| 11:04 |  | Increase the range of integer indexes in the the merge() family of SQL functions. (check-in: 8d3943890e ... user: drh tags: trunk) | 
| 2025-10-17 |  |  | 
| 10:06 |  | Fix a minor memory leak in the CLI that can occur following an error in the ".output" dot-command. (check-in: d1044bc061 ... user: drh tags: trunk) | 
| 2025-10-16 |  |  | 
| 23:42 |  | Resolve the MacOS-specific build issue reported in forum post 95edc7a8d7d1de59, with help from Bo Lindbergh. (Edit: this was merged in from [0428ee1ecf24a49], but a newly-reproducible libfossil bug caused that second P-card to be lost as a side effect of the one merged file having had both a merge-change state and a local-edit state at the time.) (check-in: b144f875d6 ... user: stephan tags: trunk) | 
| 14:17 |  | Improve performance of the shell tool ".ar" command when it is given a large number of arguments. (check-in: 8fed9de84b ... user: dan tags: trunk) | 
| 12:43 |  | In dbpage.c, disable truncation on any update error, including errors in sqlite3PagerWrite(). (check-in: eee5ba5d8a ... user: drh tags: trunk) | 
| 01:44 |  | Fix an off-by-one in c-pp-lite.c which could cause @token@-processed output to get one duplicated byte. (check-in: aa2f4c5160 ... user: stephan tags: trunk) | 
| 2025-10-15 |  |  | 
| 23:28 |  | Minor JS doc updates. No code changes. (check-in: fc2a026901 ... user: stephan tags: trunk) | 
| 23:13 |  | Resolve a valgrind-reported error in ext/wasm/c-pp-lite.c caused by successful exit colliding with the atexit() handler. Add -e SCRIPT support to c-pp-lite to facilitate testing. (check-in: 7801c7225a ... user: stephan tags: trunk) | 
| 15:54 |  | Improve performance of the shell tool ".ar" command when it is given a large number of arguments. (closed check-in: 0398897067 ... user: dan tags: ar-performance-improvement) | 
| 10:52 |  | Fix memory allocation in the ".table" command of the .cli when the number of tables in the database file approaches 2 billion. (check-in: 5cbccab499 ... user: drh tags: trunk) | 
| 2025-10-14 |  |  | 
| 19:21 |  | Reestablish APIARMOR for the sqlite3_db_status() interface. (check-in: 1738f0bdf5 ... user: drh tags: trunk) | 
| 18:23 |  | JS/WASM: Expose sqlite3_db_status64(), sqlite3changeset_apply_v3(), sqlite3changeset_apply_v3_strm(), and SQLITE_DBSTATUS_TEMPBUF_SPILL. (check-in: b13eafc9b6 ... user: stephan tags: trunk) | 
| 17:50 |  | Add the test/testloadext.c loadable extension to test the ability to use the latest APIs using the loadable extension mechanism. (check-in: a9bd7ed6f7 ... user: drh tags: trunk) | 
| 17:09 |  | Add the sqlite3_db_status64() interface and the SQLITE_DBSTATUS_TEMPBUF_SPILL option for use in that interface. (check-in: 9f8739ddea ... user: drh tags: trunk) | 
| 16:57 |  | Make the new sqlite3_db_status64() API accessible to extensions. (closed check-in: 2ebd733031 ... user: drh tags: dbstatus-spill) | 
| 14:07 |  | Fix two links in ext/wasm/README.md. (check-in: 0f96eaeaa8 ... user: stephan tags: trunk) | 
| 14:03 |  | Extra defenses against integer overflow in the decimal extension. (check-in: 1a7414d9b5 ... user: drh tags: trunk) | 
| 13:47 |  | Add a section to ext/wasm/README.md about the dependency on wabt/wasm-strip for release builds. (check-in: 0dbb13863b ... user: stephan tags: trunk) | 
| 13:25 |  | Fix a makefile typo which caused a build directory to not get mkdir'd for the fiddle build. (check-in: d98d5abf6d ... user: stephan tags: trunk) | 
| 11:18 |  | Merge all the latest trunk enhancements into the dbstatus-spill branch. (check-in: 55f2613c1b ... user: drh tags: dbstatus-spill) | 
| 2025-10-13 |  |  | 
| 16:07 |  | Avoid integer overflows (a) when dealing with zeroblob(N) calls when 2*N is larger than 2^31, and (b) in fts5 when a corrupt record is close to 2^31 bytes in size. Both of these require special builds with SQLITE_MAX_LENGTH set to a non-default value. (check-in: 2792751969 ... user: dan tags: trunk) | 
| 12:36 |  | Fix a bug in concat_ws() in which an initial empty string was treated as if it was a NULL value. (check-in: eb2e4e4617 ... user: drh tags: trunk) | 
| 10:35 |  | Enable CLI documentation for .imposter (check-in: c639c7be02 ... user: drh tags: trunk) | 
| 2025-10-12 |  |  | 
| 19:17 |  | Fix broken hyperlinks in the documentation. (The multiple edits/moves were the result of confusion between sqlite3_sourceid() and its UDF, sqlite_source_id().) (check-in: 62e6b120c4 ... user: drh tags: trunk) | 
| 10:31 |  | Fix order of operations in the OP_Concat operator of the byte-code engine to avoid any possibility of an integer overflow. (check-in: 765e656982 ... user: drh tags: trunk) | 
| 2025-10-11 |  |  | 
| 18:44 |  | Remove an unused variable from the generate_series implementation. (check-in: a523e4adbf ... user: drh tags: trunk) | 
| 16:23 |  | Fix an assert() that could fail when reading a blob or text value larger than 1GiB from the database (requires a non-default build configuration). This is an assert() fix only, production builds were functioning correctly. (check-in: 3e5ec5731f ... user: dan tags: trunk) | 
| 15:45 |  | Improvements to the (rarely used) home-grown varients of seriesCeil() and seriesFloor() based on ideas from forum post 2025-10-11T12:36:56z. (check-in: e6b9bfdb6f ... user: drh tags: trunk) | 
| 2025-10-10 |  |  | 
| 19:43 |  | Provide alternative implementations of ceil() and floor() to the generate_series() function so that it will build even when SQLite is compiled using --disable-math. Forum post 91690fa646. (check-in: 97d6fd2c99 ... user: drh tags: trunk) | 
| 16:04 |  | Back out [8bf26c956e]. It is not required following [8e7da8cf50]. (check-in: ea29180797 ... user: dan tags: trunk) | 
| 14:31 |  | Fix harmless compiler warnings. (check-in: 4966d7a1ce ... user: drh tags: trunk) | 
| 14:22 |  | Makefile updates to suppose carray() and percentile(). Add Carray to the CLI and fuzzcheck to support debugging. (check-in: fe9cf68b51 ... user: drh tags: trunk) | 
| 14:00 |  | Regenerate autoconf/Makefile.msc to squelch a 'make test' warning. (check-in: f7f55dce18 ... user: stephan tags: trunk) | 
| 13:43 |  | Fix the carrayfault.test script so that it works in configurations that do not support virtual tables. (check-in: 767a667b91 ... user: drh tags: trunk) | 
| 10:40 |  | Avoid (apparently harmless) undefined behavior in CARRAY if the rowid is used as a constraint in the WHERE clause. (check-in: 200b3c8f6c ... user: drh tags: trunk) | 
| 00:59 |  | Fix the sqlite3_bind_blob() interface so that it sets the encoding of the underlying sqlite3_value object to the database encoding. Fix for the problem reported by forum post 2025-10-07T12:47:25z. (check-in: 8e7da8cf50 ... user: drh tags: trunk) | 
| 00:26 |  | Fix the xBestIndex method on carray so that it gives a coherient query plan in a join when some of the arguments to carray() come from other table in the join. (check-in: c8417b3261 ... user: drh tags: trunk) | 
| 2025-10-09 |  |  | 
| 19:23 |  | Fix carrayfault.test so that it works with the new built-in CARRAY. (check-in: 22f6946821 ... user: drh tags: trunk) | 
| 15:28 |  | Update c-pp.c and rename it to c-pp-lite.c to match its new upstream name. Adapt the JS files and mkwasmbuilds.c for c-pp syntactic changes. (check-in: bb13e46ddf ... user: stephan tags: trunk) | 
| 14:50 |  | Avoid corrupting the fts5 index if a value that was inserted via sqlite3_bind_blob() in a non-utf8 db is deleted. (check-in: 8bf26c956e ... user: dan tags: trunk) | 
| 14:14 |  | Update the result formatter spec such that column width are stored in 16-bit signed integers. (check-in: d5dfbcf077 ... user: drh tags: resfmt) | 
| 14:01 |  | In the CLI, limit column widths specified by the ".width" command to have an absolute value not more than 30,000. (check-in: b3741f1101 ... user: drh tags: trunk) | 
| 2025-10-08 |  |  | 
| 20:10 |  | Add the pRender callback to the query result formatter spec. (check-in: 642cf4568a ... user: drh tags: resfmt) | 
| 19:34 |  | Merge the latest trunk enhancements into the resfmt branch. (check-in: 550c48f7c3 ... user: drh tags: resfmt) | 
| 19:01 |  | Move the carray() extension and the percentile extension into the amalgamation. Both are still turned off by default and must be enable using SQLITE_ENABLE_CARRAY and SQLITE_ENABLE_PERCENTILE compile-time options, so there is no increase in object code size. However, these extensions are now more easily available. (check-in: 2d40dc2173 ... user: drh tags: trunk) | 
| 18:51 |  | Add the SQLITE_ prefix to the type constants used by sqlite3_carray_bind(). (closed check-in: e11e9913e5 ... user: drh tags: ext-to-core) | 
| 18:17 |  | Make use of the new SQLITE_ENABLE_PERCENTILE in the wasm build. (check-in: 937c084c2d ... user: stephan tags: ext-to-core) | 
| 18:11 |  | Add ENABLE compile-time options to enable carray() and median(). Hence there is no new code and no increase in library size without the ENABLE macros. (check-in: 7dec290b41 ... user: drh tags: ext-to-core) | 
| 16:29 |  | This is how the percentile() family of functions might be integrated into the amalgamation, should we elect to do such a thing. (check-in: c2cfc9c92e ... user: drh tags: ext-to-core) | 
| 15:32 |  | Were we to choose to integrate the carray() table-valued function into the amalgamation, that integration might look something like this. (check-in: 2b43d5d7dc ... user: drh tags: ext-to-core) | 
| 12:37 |  | Fix harmless integer overflow in the CLI when arguments to --lookaside are ridiculously large. (check-in: 8bf337e6e6 ... user: drh tags: trunk) | 
| 11:00 |  | Fix an uninitialized variable problem in test1.c. (check-in: 059203f658 ... user: dan tags: trunk) | 
| 09:52 |  | The ".dbtotxt" command in the CLI should not fail if the database is uninitialized. It should instead be a no-op. (check-in: 2818ef64d9 ... user: drh tags: trunk) | 
| 02:05 |  | Function name doc typo fixes for sqlite3_source_id() (sic.). (Edit: this unexpectedly broke something. Edit again: it did not - the broken code was using the mis-typed function names.) (check-in: 03c3f12853 ... user: stephan tags: trunk) | 
| 2025-10-07 |  |  | 
| 21:02 |  | Add test cases for the carray module. (check-in: dcfc0164a9 ... user: dan tags: trunk) | 
| 20:11 |  | Initial prototype spec for the SQLite Query Result Formatter. (check-in: 87b5e41b99 ... user: drh tags: resfmt) | 
| 18:06 |  | Improve the invariant checker module so that so that it added "+" before "column ISNULL" in queries where the base query contains a GROUP BY, to prevent the ISNULL term from being pushed down into the subquery, since that can cause ambiguities if column is UNIQUE. (check-in: b4ff920fbe ... user: drh tags: trunk) | 
| 15:14 |  | Fix some integer overflow warnings in the base64 and base85 extensions. (check-in: 06b4bd2aba ... user: dan tags: trunk) | 
| 2025-10-06 |  |  | 
| 20:11 |  | Fix a minor problem with building sqlite3.c without manifest.tags. (check-in: 140f0f9fd3 ... user: dan tags: trunk) | 
| 17:52 |  | Avoid attempting to delete a file that is still open in sessionnoop2.test. (check-in: 56f6d6c538 ... user: dan tags: trunk) | 
| 17:20 |  | Avoid using randomblob() in walckptnoop.test, as it produces different results for different test permutations. (check-in: 94ceb0f0cf ... user: dan tags: trunk) | 
| 16:57 |  | Change the testrunner.tcl option from "--case" (singular) to "--cases". Allow the argument to be a glob pattern, to pick up multiple cases. (check-in: e205f87f5c ... user: drh tags: trunk) | 
| 16:56 |  | Fix typo in the previous check-in that prevented the script from running. (closed check-in: 7976647007 ... user: drh tags: testrunner-enhancements) | 
| 16:54 |  | Add info from the SQLITE_SCM_... macros to tool/version-info.c's --json output, prompted by forum post 0c513ae3fc. (check-in: 951882aa4e ... user: stephan tags: trunk) | 
| 16:50 |  | Update testrunner.tcl so that the new option is --cases rather than singular --case, and so that it is a glob pattern. (check-in: 9488b4f00c ... user: drh tags: testrunner-enhancements) | 
| 16:23 |  | Fix typo in the "testprefix" for walckptnoop.test (check-in: ff205f2993 ... user: drh tags: trunk) | 
| 15:53 |  | New testrunner.tcl option "--case DISPLAYLNAME" only runs the single case (and its build dependencies) that exactly matches DISPLAYNAME. (check-in: 8ea2e097b7 ... user: drh tags: trunk) | 
| 14:30 |  | Fix testrunner.tcl so that --help output goes to stdout, not stderr. (check-in: 441a759c22 ... user: drh tags: trunk) | 
| 11:50 |  | Fix a subtle problem with short-circuit evaluation introduced by [cea8bf79e18d55a8] and detected by the dbsqlfuzz fuzzer. Test cases in TH3. (check-in: d71cd722c4 ... user: drh tags: trunk) | 
| 2025-10-05 |  |  | 
| 22:41 |  | Another small performance increase in the text format by using strchr() to search for '%'. (check-in: 59ad89ba07 ... user: drh tags: trunk) | 
| 19:56 |  | Fix a syntax error in the tcl script part of [199b6567bb41cd]. (check-in: a7ed85f270 ... user: stephan tags: trunk) | 
| 19:30 |  | Performance optimization in text formatter by using a hash table to look up the operator character that follows %-subsitutions. (check-in: 199b6567bb ... user: drh tags: trunk) | 
| 2025-10-03 |  |  | 
| 18:17 |  | Minor clarifications to the JSONB format documentation. (check-in: 0fc708b41f ... user: drh tags: trunk) | 
| 2025-10-02 |  |  | 
| 22:48 |  | Remove the memvfs.c extension, to prevent it from being misused. See forum post 91811439aa for the complaint that prompted me to take down this demonstration code. I have other things to do rather than argue about posts such as the foregoing, so it is easier to just take the code down. If you need this code, you can downloading older check-ins. The beauty of DAG-based version control systems like Fossil (and Git) is that nothing is ever lost; it is just stops app... (check-in: eb79110dca ... user: drh tags: trunk) | 
| 19:35 |  | Fix typo in the previous check-in. Remove surplus end-of-line whitespace from the ext/misc/fossildelta.c extension. (check-in: cc87551805 ... user: drh tags: trunk) | 
| 19:19 |  | Harden the debugging vtab "delta_parse()" which is part of the ext/misc/fossildelta.c extension against malicious inputs. forum post be139437c3. (check-in: 4be6deee64 ... user: drh tags: trunk) | 
| 18:31 |  | In the ".open" command of the CLI if using the --new option with a URI filename, then decode the URI to extract the actual filename prior to trying to delete that file. (check-in: 14ee3c1f03 ... user: drh tags: trunk) | 
| 16:14 |  | Fix harmless compiler warnings. (check-in: 963a4c4067 ... user: drh tags: alter-table-constraints) | 
| 15:30 |  | Merge recent trunk fixes and enhancements into the alter-table-constraints branch. (check-in: 26f8a70078 ... user: drh tags: alter-table-constraints) | 
| 14:48 |  | Fix -DSQLITE_OS_OTHER=0 builds. (check-in: 2971d74701 ... user: dan tags: trunk) | 
| 11:28 |  | Correction to [ae303dab2f534e2c] to use the right loop over the solver. (check-in: 22b2700ac2 ... user: drh tags: trunk) | 
| 11:16 |  | Fix a buffer overrun in the zipfile extension that could occur while processing zip archives with extremely long file-names and comment fields. (check-in: c284fda848 ... user: dan tags: trunk) | 
| 2025-10-01 |  |  | 
| 20:41 |  | Add the --ifexists option to the CLI and to the ".open" command of the CLI. Also the undocumented and unsupported --exclusive option on the same. (check-in: 9a1947814d ... user: drh tags: trunk) | 
| 19:01 |  | Prevent embedded 0x00 characters in the output strings from confusing the completion extension and causing it to read past the end of an array. Forum post 88a08b3201. (check-in: 26f6d41f9d ... user: drh tags: trunk) | 
| 17:27 |  | Fix some problems with DROP CONSTRAINT and DROP NOT NULL on generated columns. (check-in: 66c47e18e6 ... user: dan tags: alter-table-constraints) | 
| 14:28 |  | Fix a buffer overread in demo code in ext/misc/memvfs.c. (check-in: 90f7318308 ... user: dan tags: trunk) | 
| 2025-09-30 |  |  | 
| 19:20 |  | Fix handling of FK constraints that immediately follow named table constraints in DROP CONSTRAINT. (check-in: 1c6f09f9f9 ... user: dan tags: alter-table-constraints) | 
| 19:11 |  | Enable the CLI to be compiled with SQLITE_OMIT_AUTHORIZATION. (check-in: afb2feb380 ... user: drh tags: trunk) | 
| 17:37 |  | Fix various compiler warnings. (check-in: 24ee6c25b4 ... user: drh tags: trunk) | 
| 17:13 |  | Fix some DROP CONSTRAINT and DROP NOT NULL cases involving SQL comments or multiple table constraints without separating commas. (check-in: b0b02e58ac ... user: dan tags: alter-table-constraints) | 
| 17:12 |  | Fix the new generate_series() to correctly deal with an explicit 0 as the step. (check-in: b059413c44 ... user: drh tags: trunk) | 
| 11:17 |  | Fix another potential buffer overread in the zipfile extension. (check-in: 0819fe670f ... user: dan tags: trunk) | 
| 10:11 |  | Strengthen the fts5_locale() function against integer overflow attacks, even when SQLite is compiled with oversize values for SQLITE_MAX_LENGTH. (check-in: f0dbd9d22f ... user: drh tags: trunk) | 
| 2025-09-29 |  |  | 
| 20:40 |  | Fix a problem with the merge at [cb604efdb40da95a]. (check-in: ee6102d4fe ... user: drh tags: dbstatus-spill) | 
| 19:57 |  | Change the name to SQLITE_DBSTATUS_TEMPBUF_SPILL. (check-in: 4a60a9c635 ... user: drh tags: dbstatus-spill) | 
| 18:55 |  | Fix a buffer overread in the zipfile extension code. (check-in: 70c2c99b6f ... user: dan tags: trunk) | 
| 18:48 |  | Merge the latest trunk enhancements into the dbstatus-spill branch (check-in: cb604efdb4 ... user: drh tags: dbstatus-spill) | 
| 18:17 |  | Improve whereSolver() so that it always finds a unique solution. (check-in: ae303dab2f ... user: drh tags: trunk) | 
| 16:49 |  | Add further tests for the new ALTER TABLE commands on this branch. (check-in: 17a6f4e69e ... user: dan tags: alter-table-constraints) | 
| 14:09 |  | Fix JSONB logic so that it correctly interprets large JSON5 hexadecimal integer literals as positive numbers. (check-in: 0ad5d77fc0 ... user: drh tags: trunk) | 
| 13:43 |  | Large JSON5 hexadecimal integer literals are positive unless preceded by a negative sign. (closed check-in: 5a93d9dce2 ... user: drh tags: json5-hex-fix) | 
| 2025-09-28 |  |  | 
| 23:10 |  | Back out check-in [9abaa0ac2b304934] as the code removed there is necessary after all. (check-in: 27ae986294 ... user: drh tags: trunk) | 
| 18:25 |  | Revamp the implementation of generate_series to simplify and improve long-term maintainability. (check-in: 9041ca8543 ... user: drh tags: trunk) | 
| 18:03 |  | Remove the long-defunct wasmfs build of speedtest1 from index.html and re-enable the wasmfs scratchpad app (which still works). Fix the CFLAGS for the wasmfs build and the path to the wasm file for one of the JS. (check-in: acaaf8cce9 ... user: stephan tags: trunk) | 
| 17:24 |  | Minor bug fixes. Many new test cases. (closed check-in: 86dcc68d18 ... user: drh tags: series-refactor) | 
| 12:47 |  | Use new c-pp features to simplify generation of the tester1 family of files. (check-in: f7cab028c9 ... user: stephan tags: trunk) | 
| 11:01 |  | All legacy tests are passing. Want to add new test cases before merging. (check-in: adb80761d2 ... user: drh tags: series-refactor) | 
| 10:36 |  | The generate_series rewrite is working better now, but is still not completely correct. (check-in: 6c41d14707 ... user: drh tags: series-refactor) | 
| 09:25 |  | Refactor the generate_series extension. Compiles but does not yet work. (check-in: 25e395a82f ... user: drh tags: series-refactor) | 
| 08:59 |  | Fix harmless typo in a comment. (check-in: 1f6deabfff ... user: drh tags: trunk) | 
| 02:30 |  | Internal js/wasm tooling doc updates. (check-in: 0d6ed44278 ... user: stephan tags: trunk) | 
| 00:53 |  | Improve #if support in, and add #assert to, c-pp. Rename target=... preprocessor symbols to target:... because changes in the semantics for symbols which have an '=' makes some of those #if blocks mutually exclusive (which we won't want in rare cases involving the bundler-friendly builds). (check-in: 5428748779 ... user: stephan tags: trunk) | 
| 2025-09-27 |  |  | 
| 19:17 |  | Add test cases for ALTER TABLE and attached databases. (check-in: 8f2e066e9a ... user: dan tags: alter-table-constraints) | 
| 18:25 |  | Add OOM injection tests for new commands on this branch. (check-in: 34b47c9b1b ... user: dan tags: alter-table-constraints) | 
| 18:08 |  | Add tests for ALTER TABLE authorization callbacks and other things. (check-in: 55cad8d150 ... user: dan tags: alter-table-constraints) | 
| 16:13 |  | Fix authorization callbacks for the new commands on this branch. (check-in: 0165e2afa8 ... user: dan tags: alter-table-constraints) | 
| 15:03 |  | Rework the bounds calculations on generate_series() in an attempt to avoid signed integer overflow when the bounds are ridiculous values. (check-in: 3d9148c7d7 ... user: drh tags: trunk) | 
| 14:59 |  | Add experimental implementations of ALTER TABLE commands to add and drop CHECK and NOT NULL constraints. (check-in: d939b25d76 ... user: dan tags: alter-table-constraints) | 
| 13:34 |  | ext/wasm/c-pp.c: add #savepoint support. Consolidate how the diverse sqlite3_stmt handles are managed. (check-in: b44650f907 ... user: stephan tags: trunk) | 
| 13:00 |  | Use sqlite3_malloc64() and sqlite3_realloc64() everywhere in the CLI. (check-in: b05c470091 ... user: drh tags: trunk) | 
| 12:35 |  | Fix a bug in the ".dbtotxt" command of the CLI that causes a NULL pointer dereference if run on an in-memory database. (check-in: 8653524992 ... user: drh tags: trunk) | 
| 12:29 |  | Avoid integer overflow in memory allocation for the ".import --csv" command in the CLI. (check-in: a8093d45e0 ... user: drh tags: trunk) | 
| 11:54 |  | Additional cleanup and refinement of the regexp.c extension. (check-in: 2b34b750b5 ... user: drh tags: trunk) | 
| 11:38 |  | Teach c-pp how to replace configure-script-like @tokens@, giving us a much simpler way to get the proper WASM file name injected into each JS build. (check-in: ffd5c8eaa8 ... user: stephan tags: trunk) | 
| 00:07 |  | Remove and edit some comments. (check-in: 1fa7e32d26 ... user: stephan tags: trunk) | 
| 00:05 |  | Add wasm.ptr to the module-symbols.html output. (check-in: dced5a7c43 ... user: stephan tags: trunk) | 
| 00:04 |  | Latest upstream c-pp.c for the ability to process multiple inputs and outputs in a single invocation. (check-in: c763337304 ... user: stephan tags: trunk) | 
| 2025-09-26 |  |  | 
| 19:41 |  | Delay replacing the wasm file name strings until they're needed. Add target b-all to run all known wasm builds. (check-in: a4edaecc9b ... user: stephan tags: trunk) | 
| 19:30 |  | Consolidate some duplicate code in mkwasmbuilds.c. (check-in: 1078aa4f1e ... user: stephan tags: trunk) | 
| 19:19 |  | Clean up how the speedtest1 64-bit test/demo files are generated. Make log output very slightly less verbose. (check-in: 64715e9270 ... user: stephan tags: trunk) | 
| 18:29 |  | Get 64-bit builds of speedtest1.wasm running in order to better compare 32-/64-bit performance (spoiler alert: the latter is consistently roughly 10% slower). Add WASM heap usage info to speedtest1. (check-in: 70b944cc01 ... user: stephan tags: trunk) | 
| 17:11 |  | Reinstate the Module.instantiateWasm() override and simplify how the wasm filename gets injected into the JS files. (check-in: e878ffd2ea ... user: stephan tags: trunk) | 
| 15:38 |  | Limit the complexity of a REGEXP pattern using SQLITE_LIMIT_LIKE_PATTERN_LENGTH (check-in: 869c968569 ... user: drh tags: trunk) | 
| 13:14 |  | In the regexp extension, limit the maximum value of integers in the "{p,q}" syntax, as performance of the NFA used to do pattern matching is linear in the maximum such integer. The limit is SQLITE_MAX_REGEXP_REPEAT which defaults to 999. This helps to prevent DoS attacks in systems that make use of the regexp extension. (check-in: 911c745f88 ... user: drh tags: trunk) | 
| 12:49 |  | Get wasmfs build working in the new setup. Disable the custom wasm loader because its generated makefile code is currently broken. (check-in: 36bc2514f7 ... user: stephan tags: trunk) | 
| 11:53 |  | Fix harmless integer overflows caused by over-sized integer constants in the "{m,n}" syntax of the regexp extension. (check-in: 8f17e1d869 ... user: drh tags: trunk) | 
| 11:47 |  | Docs and dead code removal in mkwasmbuilds.c. (check-in: d022ee167b ... user: stephan tags: trunk) | 
| 11:45 |  | Fix some integer overflow problems when handling corrupt hexdb dumps in the shell tool. Forum post 82b513b116. (check-in: 730323f95d ... user: dan tags: trunk) | 
| 11:36 |  | Add new table-valued functions jsonb_each() and jsonb_tree() that work just like json_each() and json_tree() except that the "value" column is JSONB instead of JSON text when the "type" is 'object' or 'array'. (check-in: dfc41cb3aa ... user: drh tags: trunk) | 
| 11:28 |  | Fix a harmless compiler warning. (closed check-in: c50a3c45a2 ... user: drh tags: jsonb_each) | 
| 11:14 |  | In c-pp.c, use (void)x instead of an empty if(x){} to flag unused arguments. (check-in: df9ab8a355 ... user: stephan tags: trunk) | 
| 10:29 |  | On-demand loading of JSON virtual tables. (check-in: 118f97ddcd ... user: drh tags: jsonb_each) | 
| 02:18 |  | More build docs. Add a speedtest1 64-bit build but it's not hooked up to any test apps yet. Improve invalid-arg detection in c-pp. (check-in: 69086be8a1 ... user: stephan tags: trunk) | 
| 01:38 |  | Build cleanups and dead code removal. (check-in: b516109e5a ... user: stephan tags: trunk) | 
| 2025-09-25 |  |  | 
| 23:32 |  | Add JSON table-valued functions jsonb_each() and jsonb_tree(). (check-in: 2f8b461c63 ... user: drh tags: jsonb_each) | 
| 19:32 |  | Include writes to temp files by sort operations in the SQLITE_DBSTATUS_SPILL values. (check-in: 4c1118ba40 ... user: dan tags: dbstatus-spill) | 
| 18:53 |  | wasm dist: add --snapshot flag, add -64 flag to include 64-bit builds, and add module-symbols.html to the dist. (check-in: e9a6391b37 ... user: stephan tags: trunk) | 
| 17:29 |  | Replace ext/wasm/dist.make with mkdist.sh and get dist working with the new build. Fix the preprocessing flags for tester1-esm.html. (check-in: ab6eb4d3b7 ... user: stephan tags: trunk) | 
| 15:17 |  | Overhaul the wasm build to (A) support more coexisting variants, e.g. 32/64-bit of both vanilla and esm, and (B) build each variant to its own subdir so that they can build in parallel. It can, with make -j4, now build all new variants in half the time it previously took for just the 32-bit builds. The new build logging output, though unconventional, serves two purposes: (A) improve my build-time situational awareness and (B) it help demystify some of the build steps and output files. (check-in: 2f4be98614 ... user: stephan tags: trunk) | 
| 15:06 |  | Fix the translation of "\v" in JSON5. Forum post 28e21085f9. (check-in: 2914e8fc7b ... user: drh tags: trunk) | 
| 14:44 |  | Remove some superfluous tabs and add the mkwasmbuilds ability to emit just its prologue. (closed check-in: 7507940175 ... user: stephan tags: wasm-rebuild-2025) | 
| 14:39 |  | Minor logging fix for the wasm-opt build step. (check-in: 918eca02d1 ... user: stephan tags: wasm-rebuild-2025) | 
| 14:29 |  | Move fiddle.make into the main makefile. Various adjacent cleanups and docs. (check-in: fabd6160d0 ... user: stephan tags: wasm-rebuild-2025) | 
| 11:10 |  | Avoid an integer overflow in the shell tool when outputing hex dumps of blobs greater than 1GB in size. (check-in: 4a1bb874f1 ... user: dan tags: trunk) | 
| 10:55 |  | Convert the ShellText object in the CLI to use sqlite3_malloc64() rather than plain malloc(). Avoid unnecessary quoting of table names in .dump output and similar. Improved detection of OOM in set_table_name(). (check-in: 6ce4f6dc15 ... user: drh tags: trunk) | 
| 10:50 |  | Avoid an integer overflow (forum post b3abe06121) and a compiler warning (forum post 3db6aa9ed6) in the shell tool code. (check-in: 39925489b3 ... user: dan tags: trunk) | 
| 02:30 |  | Minor build cleanups. (check-in: 05e7ea0213 ... user: stephan tags: wasm-rebuild-2025) | 
| 02:02 |  | Consolidate the speedtest1 wasm build into the new mkwasmbuilds.c model. Remove the long-unused batch-runner JS tools. (check-in: 980c033c05 ... user: stephan tags: wasm-rebuild-2025) | 
| 00:53 |  | Wasm build deps fixes. (check-in: fd0c649047 ... user: stephan tags: wasm-rebuild-2025) | 
| 2025-09-24 |  |  | 
| 23:07 |  | Move the static makefile code from mkwamsbuilds.c into the makefile. Move util.make back into the main makefile. Remove a bunch of dead makefile code and docs. (check-in: 18beeb26bf ... user: stephan tags: wasm-rebuild-2025) | 
| 20:53 |  | Cosmetic makefile tweaks. (check-in: 4ec7e6800e ... user: stephan tags: wasm-rebuild-2025) | 
| 20:44 |  | Fix the makefile so that " `test/testrunner.tcl release --buildonly` " works on mac.  Apparently broken by [b7751d144330593b].  (check-in: 023365228f ...  user: drh tags: trunk) | 
| 20:39 |  | Get the tester1 worker1/promiser demos building again. Show WASM heap usage in tester1 if. (check-in: 6691a84872 ... user: stephan tags: wasm-rebuild-2025) | 
| 20:09 |  | Add an assert() to fix a harmless compiler warning in the session extension. (check-in: d77b8ad96c ... user: drh tags: trunk) | 
| 19:51 |  | Fix a harmless compiler warning in FTS5. (check-in: 89f8d02075 ... user: drh tags: trunk) | 
| 19:10 |  | Rework the set_table_name() function in the CLI to use modern APIs. Forum post 2baa4acd64. (check-in: 821cc0e421 ... user: drh tags: trunk) | 
| 17:49 |  | Special case the "decimal(-0.0)" output to show exactly 0.0, just as it does for positive zero. Forum post adc719d88. (check-in: 9ca4163bd9 ... user: drh tags: trunk) | 
| 17:46 |  | Generate 64-bit variants of tester1.js/html. Enhance c-pp to accept multiple input files. (check-in: ef6c55e449 ... user: stephan tags: wasm-rebuild-2025) | 
| 17:38 |  | Fix a harmless compiler warning in IEEE754. (check-in: 7f72308322 ... user: drh tags: trunk) | 
| 17:07 |  | Additional short-circuit optimizations for AND/OR operators. Also fix bugs in prior implementations (such as [0083d5169a46104a]). (check-in: 63b686fe4c ... user: drh tags: trunk) | 
| 16:26 |  | Simplification to the AND/OR short-circuit logic. (leaf check-in: 7be555a3b8 ... user: drh tags: short-circuit) | 
| 16:10 |  | Improve performance of window function queries that use "BETWEEN :x FOLLOWING AND :y FOLLOWING" where :y is a very large number. (check-in: 1f0b714357 ... user: dan tags: trunk) | 
| 14:43 |  | Fixes to short-circuit AND/OR evaluation. (check-in: bc4cadbd60 ... user: drh tags: short-circuit) | 
| 13:59 |  | Add docs to mkwasmbuilds.c explaining the purpose of each of the builds. Remove some inadvertent copy/paste hard tabs in some C-side makefile code. (check-in: a2bd965e10 ... user: stephan tags: wasm-rebuild-2025) | 
| 13:10 |  | Factor out the code that handles AND and OR short-circuiting into a separate subroutine, for performance and legibility. (check-in: cab84a65c3 ... user: drh tags: short-circuit) | 
| 12:01 |  | Fix incorrect buffer size computation for the base64() extension function. Forum post b1993c858f. (check-in: 603efcd404 ... user: drh tags: trunk) | 
| 11:12 |  | Restrict the size of the LIMIT on a generate_series() query to avoid an integer overflow when computing the final output value. Forum post 479bfb0d3b. (check-in: 266aacb475 ... user: drh tags: trunk) | 
| 07:31 |  | Cosmetic build tweaks. (check-in: 431725330d ... user: stephan tags: wasm-rebuild-2025) | 
| 07:00 |  | Fix the positioning of the wasm-bare-bones flag init. (check-in: f5167fb361 ... user: stephan tags: wasm-rebuild-2025) | 
| 06:59 |  | In speedtest1, always use fatal_error() to exit fatally, instead of an fprintf/exit() pair, so that stdout/stderr get flushed consistently (which is significant for the WASM build). (check-in: 1bd15cb343 ... user: stephan tags: wasm-rebuild-2025) | 
| 06:43 |  | Further wasm build refactoring. Get speedtest1 building again. (check-in: 5434307dcd ... user: stephan tags: wasm-rebuild-2025) | 
| 04:58 |  | Move some of the generated wasm makefile utility code out of C and into util.make. (check-in: a2935758f5 ... user: stephan tags: wasm-rebuild-2025) | 
| 03:05 |  | Generic build simplifications. (check-in: 61648610c3 ... user: stephan tags: wasm-rebuild-2025) | 
| 01:50 |  | Get fiddle building properly again and fiddle/fiddle.debug building in parallel. (check-in: 85fe9c6155 ... user: stephan tags: wasm-rebuild-2025) | 
| 00:13 |  | Improved short-circuit evaluation of AND and OR. All appears to work, but there are still testing and performance issues to be worked out. (check-in: 0d4447aa61 ... user: drh tags: short-circuit) | 
| 2025-09-23 |  |  | 
| 23:22 |  | Get fiddle/fiddle.debug building again but they're incomplete and don't run. (check-in: d725a075bc ... user: stephan tags: wasm-rebuild-2025) | 
| 17:00 |  | Improvements to short-circuit evaluation of AND and OR operators. This is a partial and incomplete response to forum post f5adeb59ff77c056. (check-in: cea8bf79e1 ... user: drh tags: trunk) | 
| 16:01 |  | In the ieee754 extension, render -0.0 as 'ieee754(-1,-3071)' instead of 'ieee754(1,-3071)' as that makes the sign clearer. Both strings still convert back to -0.0. Note that negative zero and positive zero are distinct values in ieee754. Forum post 1eab7e4a08. (check-in: 136188c161 ... user: drh tags: trunk) | 
| 13:49 |  | Get most of the wasm build (re)working and much prettier. (check-in: a5a7890c75 ... user: stephan tags: wasm-rebuild-2025) | 
| 10:46 |  | Cosmetic wasm build tweaks. Fixed wasm-opt use, broken by this branch's refactoring. (check-in: 4d7eff2afb ... user: stephan tags: wasm-rebuild-2025) | 
| 08:44 |  | Make it easier to pick out which parallel build messages belong to which wasm build. (check-in: 913af3dd6a ... user: stephan tags: wasm-rebuild-2025) | 
| 08:23 |  | More build restructuring. (check-in: 7bee8ee763 ... user: stephan tags: wasm-rebuild-2025) | 
| 06:31 |  | Create each distinct wasm build into its own dir, instead of all going to in jswasm/sqlite3.wasm, so that the build can now run in parallel. (check-in: 47526a75f2 ... user: stephan tags: wasm-rebuild-2025) | 
| 05:10 |  | Start reworking the wasm build (again) with an eye towards simplifying it. Currently only the canonical and esm builds work, not fiddle, speedtest, nor the bundler stuff. (check-in: 102c4a35b8 ... user: stephan tags: wasm-rebuild-2025) | 
| 02:04 |  | Add makefile target '64bit' to ext/wasm/GNUmakefile to build the 64-bit bundles. It does not yet generate copies of the test apps to load/test them, pending other reworking of the build. (check-in: 3e2ea990ad ... user: stephan tags: trunk) | 
| 00:24 |  | Slight correction to the diagram in the previous checkin. (check-in: 6455af2703 ... user: stephan tags: trunk) | 
| 2025-09-22 |  |  | 
| 23:48 |  | Add a high-level diagram showing how sqlite3.js gets assembled. (check-in: e0dfa1e62b ... user: stephan tags: trunk) | 
| 21:04 |  | Refactor and simplify the JS API bootstrap steps. (check-in: a4f36fd937 ... user: stephan tags: trunk) | 
| 18:05 |  | Update ext/wasm/api/README.md for recent changes. (check-in: 03b7068693 ... user: stephan tags: trunk) | 
| 17:29 |  | Cleanups and docs in the internal parts of the JS lib bootstrapping. More tinkering with the custom (but still disabled) wasm file loader. (check-in: 35651d9ab5 ... user: stephan tags: trunk) | 
| 17:28 |  | Revise [a4430d262b3e129d] to preserve legacy behavior of the imposter test-control. The control argument must be 2 now to get read-only behavior. The control argument of 1 goes back to the prior read/write behavior. (check-in: 3e7ede2e6d ... user: drh tags: trunk) | 
| 13:13 |  | In wasm.ptr.coerce(), treat the undefined value the same as 0 for Number-type pointers for consistency with the BigInt coercion. The bindings have long treated both null and undefined as === 0 for pointer operations. (check-in: ef73bb1dda ... user: stephan tags: trunk) | 
| 11:52 |  | Minor doc updates. No functional changes. (check-in: 9c67299b45 ... user: stephan tags: trunk) | 
| 11:41 |  | Rework the JS/WASM pieces for compatibility with both 32-bit and 64-bit WASM environments. This affects only the browser-side components, not the --with-wasi-sdk build nor the core library. (check-in: 8ac367686f ... user: stephan tags: trunk) | 
| 10:48 |  | Fix a problem with handling of 4-byte utf-8 characters in the shell tool (not the sqlite library). Forum post 184b372388. (check-in: f3e845cf38 ... user: dan tags: trunk) | 
| 02:40 |  | Experimenting with a custom wasm loader, but don't enable it without an explicit build flag. The goal here is to make the WebAssembly.Module object available internally (the default loader hides it from us). (closed check-in: e5f0256fea ... user: stephan tags: wasm-64bit) | 
| 2025-09-21 |  |  | 
| 23:15 |  | Minor internal optimizations in the wasm glue. (check-in: fb053a27d9 ... user: stephan tags: wasm-64bit) | 
| 22:17 |  | A minor optimization in wasm.realloc(). (check-in: 6a070f450a ... user: stephan tags: wasm-64bit) | 
| 22:08 |  | Remove some stale makefile comments. Add a little widget to fiddle's toolbar which shows whether it's running in 32- or 64-bit mode. Reduce fiddle's starting memory to 8MB. (check-in: e99730f96e ... user: stephan tags: wasm-64bit) | 
| 21:37 |  | Improve db-close-time cleanup in the face of WASM environments which wrap wasm.exports in nullary wrappers. (check-in: fbb63634ea ... user: stephan tags: wasm-64bit) | 
| 21:17 |  | Fix a Number/BigInt descrepancy in oo1.DB() when passed a WASM-string db filename. Consolidate some duplicated internal code and adjacent minor cleanups. (check-in: d078aff781 ... user: stephan tags: wasm-64bit) | 
| 20:26 |  | Fix a StructType.dispose()/ondispose() memleak in Jaccwabyt bindings in 64-bit builds and update its docs for 64-bit additions. Remove the parts of tester1.js related to skipping specific tests in 64-bit builds. (check-in: 6baea1d719 ... user: stephan tags: wasm-64bit) | 
| 19:39 |  | General cleanups and dead code removal. (check-in: 0336fa95e1 ... user: stephan tags: wasm-64bit) | 
| 19:01 |  | Remove some dead code and stray debug output. Fix (again) the JS SQLTester for 64-bit (the previous build was set to 32-bit). (check-in: a6b9567001 ... user: stephan tags: wasm-64bit) | 
| 18:42 |  | Update the JS SQLTester for recent sqlite3.wasm changes. (check-in: 0fa1830540 ... user: stephan tags: wasm-64bit) | 
| 17:55 |  | Generic cleanups and simplifications in JS code. (check-in: 074cf4e6c1 ... user: stephan tags: wasm-64bit) | 
| 14:25 |  | Rename wasm config entry pointerSizeof to pointerSize for consistency with wasm.ptr.size. (check-in: 2cd8ba740f ... user: stephan tags: wasm-64bit) | 
| 13:53 |  | Restructure the interface for the sqlite3.wasm pointer-size-dependent details. (check-in: 8ac12e1f51 ... user: stephan tags: wasm-64bit) | 
| 11:49 |  | Reduce sqlite3.js build size by about 30kb by stripping out Emscripten's makeInvalidEarlyAccess() wrappers. (check-in: a65bd978cb ... user: stephan tags: trunk) | 
| 11:47 |  | Reduce sqlite3.js build size by about 30kb by stripping out Emscripten's makeInvalidEarlyAccess() wrappers. (check-in: 9fafe075d6 ... user: stephan tags: wasm-64bit) | 
| 11:39 |  | Remove some dead code. (check-in: 1c2cd58d1a ... user: stephan tags: wasm-64bit) | 
| 11:15 |  | Minor cleanup in sqlite3.wasm.ptrAdd(). (check-in: f659595d2e ... user: stephan tags: wasm-64bit) | 
| 01:03 |  | Remove some fiddle debug output. (check-in: c7cc374ff5 ... user: stephan tags: wasm-64bit) | 
| 00:59 |  | Resolve the non-functional 64-bit JS vtabs and VFSes. All wasm tests now pass on a 64-bit build. Investigation of the fiddle failure on a second db import is still pending. (check-in: 7c44b48b62 ... user: stephan tags: wasm-64bit) | 
| 2025-09-20 |  |  | 
| 23:21 |  | Determine the WASM environment's pointer size at runtime instead of baking it in at build-time via the preprocessor. (check-in: 21ac6aaa03 ... user: stephan tags: wasm-64bit) | 
| 22:11 |  | Add missing sqlite3_vtab::xIntegrity wasm mapping. Correct signatures for the (unused) xDlOpen and xDlError bindings. Disable OPFS-related tests in tester1.js in 64-bit builds. (check-in: dccfa70989 ... user: stephan tags: wasm-64bit) | 
| 22:10 |  | Add missing sqlite3_vtab::xIntegrity wasm mapping. Correct signatures for the (unused) xDlOpen and xDlError bindings. Disable OPFS-related tests in tester1.js in 64-bit builds. (check-in: a106be90a2 ... user: stephan tags: wasm-64bit) | 
| 21:03 |  | Reinstate the working-again commit/rollback/update hook tests. The bug breaking those was the one-letter change at the top of this diff. (check-in: c582513a77 ... user: stephan tags: wasm-64bit) | 
| 20:23 |  | Further tweaks towards 64-bit wasm. Fiddle is failing intermittently from the C level when importing a db and it's not yet clear what's happening there. (check-in: 9fd82ebe31 ... user: stephan tags: wasm-64bit) | 
| 20:19 |  | Get speedtest1's wasm build working. (check-in: 2a61e3f1cc ... user: stephan tags: wasm-64bit) | 
| 20:17 |  | Improve deps handling of fiddle's gzipped output files to help avoid stale gz files during development (althttpd will prefer those over the uncompressed ones). Only compress the larger files, not the small html/js pieces. (check-in: bb945f5290 ... user: stephan tags: wasm-64bit) | 
| 18:51 |  | Get all but 3 of the UI-thread tester1.js test groups running in 64-bit builds. The outliers are virtual tables and commit/rollback/update hooks, plus there's still some Worker-thread-only tests to do. (check-in: c135d97436 ... user: stephan tags: wasm-64bit) | 
| 17:11 |  | Get several hundred more lines of tester1.js running in 64-bit mode. (check-in: f5105ee5de ... user: stephan tags: wasm-64bit) | 
| 15:16 |  | Further work on 64-bit WASM builds. It now gets through just shy of half of tester1.js. (check-in: c8deed3136 ... user: stephan tags: wasm-64bit) | 
| 14:48 |  | If the file-system supports f2fs style atomic-writes, sync the directory after deleting a journal file in journal_mode=DELETE mode in synchronous=NORMAL or synchronous=FULL modes (as well as synchronous=EXTRA). (check-in: e24b216996 ... user: dan tags: trunk) | 
| 14:32 |  | Fix the decimal_cmp() extension function to give correct results in corner cases such as negative zero and two equivalent values where one has extra trailing zeros to the right of the decimal point. Forum post 367c770224. (check-in: 25b3626a01 ... user: drh tags: trunk) | 
| 11:09 |  | Get the wasm tests running in a 64-bit build up through (but not including) the oo1 bits. (check-in: a5af46174a ... user: stephan tags: wasm-64bit) | 
| 03:02 |  | Get about 1/3rd of the tests running with MEMORY64=1, but the code noise level added by the BigInt/Number discrepancy is making this very unattractive. There are apparently irreconcilable differences between MEMORY64=1 and 2, in that they have different argument type expectations for methods such as WebAssembly.Table.get(), where MEMORY64=1 requires a BigInt a MEMORY64=2 requires a Number. We have no way to make that distinction from the JS code, and don't know what other APIs are affected by th... (check-in: 1e3b003ff9 ... user: stephan tags: wasm-64bit) | 
| 00:43 |  | Baby steps towards 64-bit pointers. Get it building and testing again in 32-bit builds. (check-in: f35bb66e3e ... user: stephan tags: wasm-64bit) | 
| 2025-09-19 |  |  | 
| 23:21 |  | Initial experimentation with a -sMEMORY64=1 wasm build (full 64-bit). This compiles but does not pass tests due to friction between BigInt and Number types (e.g. Number(null)===0 but BigInt(null) throws, many functions are fussy about which of those types they'll take, and we cannot simply mix and match the two types transparently (1n+1 is not legal (but 1n>=1 is), so we can no longer do pointer arithmetic without hoop-jumping)). The library bootstraps but it's failing early on in tests due to t... (check-in: cfd5c746a6 ... user: stephan tags: wasm-64bit) | 
| 20:10 |  | Add some comments describing the ways in which building sqlite3.wasm with -sMEMORY64=(non-zero) fails. (check-in: 0b14fd35ca ... user: stephan tags: trunk) | 
| 19:28 |  | Initial build-side tweaks to pave the way for a 64-bit WASM build. Such a build does not yet work for reasons given in the makefile comments, but is an eventual goal. (check-in: fe2e1681b6 ... user: stephan tags: trunk) | 
| 17:33 |  | Improve imposter tables: (1) Read-only unless writeable_schema=ON (2) --unsafe-testing no longer required (3) Does not interfere with PRAGMA integrity_check. (check-in: a4430d262b ... user: drh tags: trunk) | 
| 17:24 |  | Diverse cleanups and docs in the JS and kvvfs pieces. Functional changes: (A) ensure that the 'i32' JS/WASM func arg/result conversion works properly with a 64-bit-memory WASM build (which we neither use nor test but the newly-ratified WASM 3.0 brings within potential reach). (B) Fix sqlite3_js_posix_create_file() to not deallocate its input array if the client passes in raw memory (the library has never used it that way but the API permits i... (check-in: 79af65a694 ... user: stephan tags: trunk) | 
| 17:24 |  | Improvements to imposter tables: (1) They are read-only unless writable-schema is enabled. (2) Because they are now read-only, the ".imposter" command in the CLI no longer requires the --unsafe-testing command-line option. (3) Imposter tables do not participate in a PRAGMA integrity_check and hence do not cause false-positive errors. However, you can still do an integrity_check on the the imposter table itself by naming the imposter table as the argument to integrity_check. (closed check-in: ad152ddc4b ... user: drh tags: imposter-tables) | 
| 14:21 |  | Wasm: (A) diverse internal doc updates. (B) when generating automated JS-to-WASM function proxies for converters which require an additional middle-man proxy, e.g. sqlite3_exec(), use the client-provided function, not the proxy function, as the cache key, to keep from re-generating the conversion in some common use patterns. (check-in: 5e5139c2a1 ... user: stephan tags: trunk) | 
| 09:18 |  | Fix typo in the name of the CursorHints optimization in the CLI. (check-in: 468a11fd41 ... user: drh tags: trunk) | 
| 01:17 |  | Fix a harmless compiler warning. (check-in: 42c225a2ed ... user: drh tags: trunk) | 
| 2025-09-18 |  |  | 
| 23:23 |  | Fix the new walckptnoop.test test module so that it works in various alignments. (check-in: 08689dacc1 ... user: drh tags: trunk) | 
| 19:59 |  | Try to make the carray extension easier to compile on VxWorks. (check-in: 1a34c3f49e ... user: drh tags: trunk) | 
| 18:56 |  | Include I/O consumed by ephermeral cursors in the SQLITE_DBSTATUS_SPILL values. Still need to add sorter. (check-in: 118905b26a ... user: drh tags: dbstatus-spill) | 
| 16:48 |  | Fix harmless compiler warning in the CLI. Forum post ffdcdaba19. (check-in: 1626b6082d ... user: drh tags: trunk) | 
| 14:51 |  | Initial implementation of SQLITE_DBSTATUS_SPILL. Does not yet take into account sorters or transient tables. (check-in: 4e63ee20f8 ... user: drh tags: dbstatus-spill) | 
| 2025-09-17 |  |  | 
| 20:05 |  | Improve the quote-stripping when importing a db into fiddle, as reported in forum post 264050a3f3. (check-in: 1a87c16fdd ... user: stephan tags: trunk) | 
| 19:19 |  | Add the sqlite3_db_status64() interface. (leaf check-in: bb5b982dba ... user: drh tags: db-status64) | 
| 17:09 |  | Remove newly unreachable code in the solver of the query planner. (check-in: 9abaa0ac2b ... user: drh tags: trunk) | 
| 14:55 |  | In the date/time functions, a timezone suffix of "+00:00" should work the same as "Z". Forum post 2025-09-17T10:12:14z. (check-in: 3b7a5babf0 ... user: drh tags: trunk) | 
| 2025-09-16 |  |  | 
| 19:52 |  | If the file-system supports f2fs style atomic-writes, sync the directory after deleting a journal file in journal_mode=DELETE mode in synchronous=NORMAL or synchronous=FULL modes (as well as synchronous=EXTRA). (closed check-in: c1e37cb70a ... user: dan tags: atomic-write-fix) | 
| 2025-09-15 |  |  | 
| 17:06 |  | Updates to where.c to handle vtabs in joins more similarly to regular tables. (check-in: 9138223013 ... user: dan tags: trunk) | 
| 16:09 |  | Merge latest trunk with this branch. (closed check-in: 0bb8c42951 ... user: dan tags: vtab-planner-fixes) | 
| 14:11 |  | Diverse internal cleanups in the JS/WASM pieces. A potential fix for a hypothetical db-close-time resource leak of a subset of automated JS-to-WASM function conversions in Safari. That browser exposes WASM-exported functions via nullary wrappers, which causes a handful of them to misbehave (not clean up) at sqlite3_close_v2()-time. (check-in: fabbc8b6d1 ... user: stephan tags: trunk) | 
| 2025-09-14 |  |  | 
| 12:55 |  | Expose sqlite3_set_errmsg() to the JNI bindings. (check-in: 292866a469 ... user: stephan tags: trunk) | 
| 12:14 |  | Expose the new sqlite3_set_errmsg() to wasm. Refactor JS's sqlite3__wasm_db_error() to wrap that instead of the WASM-specific routine which previously did that job. This resolves the TODO added in [ead8a3a94e]. (check-in: e447a50f3a ... user: stephan tags: trunk) | 
| 2025-09-13 |  |  | 
| 19:23 |  | Updates to where.c to handle vtabs in joins more similarly to regular tables. (check-in: 991383262b ... user: dan tags: vtab-planner-fixes) | 
| 18:28 |  | Initialize the sqlite3_set_errmsg entry on the loadable extension thunk. Also fix a harmless compiler warning in sqlite_dbpage. (check-in: 031a43ae2b ... user: drh tags: trunk) | 
| 18:15 |  | Add --enable-dbstat and --enable-dbpage options to configure. Fix the ".dbtotxt" command in the CLI so that it uses the correct filename of the database it is rendering as text. (check-in: e04e6e6818 ... user: drh tags: trunk) | 
| 17:53 |  | Enhance the integerValue() routine in the CLI so that when its text input specifies an out-of-range integer, the routine returns the nearest integer that is representable as 64-bit twos-complement. (check-in: 5d50279fcb ... user: drh tags: trunk) | 
| 2025-09-12 |  |  | 
| 17:36 |  | Add a TODO regarding replacing the internal sqlite3__wasm_db_error() with the new sqlite3_set_errmsg(), which serves the same role. (check-in: ead8a3a94e ... user: stephan tags: trunk) | 
| 17:13 |  | Add sqlite3_set_errmsg(). Use this in sqlite3changeset_apply() to return any error code and error message via the SQLite handle. (check-in: e34eda113c ... user: dan tags: trunk) | 
| 15:31 |  | Typo fixes. Make sqlite3_set_errmsg() accessible to run-time loadable extensions. (leaf check-in: 93a41e3132 ... user: drh tags: sqlite3-set-errmsg) | 
| 15:20 |  | Add more details to comments in the previous check-in. (check-in: ed36b3aa4b ... user: drh tags: sqlite3-set-errmsg) | 
| 15:02 |  | Add experimental API sqlite3_set_errmsg(). Use this in sqlite3changeset_apply() to return any error code and error message via the SQLite handle. (check-in: 4d5b60a1e5 ... user: dan tags: sqlite3-set-errmsg) | 
| 2025-09-11 |  |  | 
| 10:58 |  | Fix an off-by-one error in sqlite3_rsync. Forum post 46753431d4. (check-in: ef3b7be6f2 ... user: drh tags: trunk) | 
| 10:43 |  | Improved support for VxWorks. (check-in: 5bc6b93522 ... user: drh tags: trunk) | 
| 10:37 |  | Fix a harmless compiler warning in the CLI. Gcc generates identical machine code before and after this change. Forum post 8bda0d896f. (check-in: dbca4fe344 ... user: drh tags: trunk) | 
| 10:14 |  | Latest upstream autosetup, with project-local s/--debug/--autodebug-debug/ patch, for HP-UX-specific config bootstrapping code and latest jimsh. (check-in: 1d2352a16c ... user: stephan tags: trunk) | 
| 10:09 |  | Configure script portability improvements for HP-UX builds, with the help of HP-UX sysadmin Michael Osipov. (check-in: 93cf2afa8c ... user: stephan tags: trunk) | 
| 10:08 |  | In mkautoconfamal.sh use $MAKE if it's set so that it can be made to work on platforms where 'make' from the PATH is not usable, e.g. HP-UX's standard make. This means that 'make snapshot-tarball' will inherit that value from the outer makefile and set it appropriate when the in-use make is named gmake. (check-in: a8635cae05 ... user: stephan tags: trunk) | 
| 2025-09-10 |  |  | 
| 20:28 |  | Minor #ifdef changes to the carray.c extension so that the source file can be more easily imported into a larger program (such as TH3). (check-in: f16fd1afbb ... user: drh tags: trunk) | 
| 18:34 |  | Use a separate mutex (SQLITE_MUTEX_STATIC_VFS2) for the VxWorks file list in os_unix.c. (closed check-in: 8f45f1a9d9 ... user: drh tags: vxworks) | 
| 16:57 |  | Change the order of some paragraphs within documentation comments in sqlite3session.h. (check-in: f0102d7b25 ... user: dan tags: trunk) | 
| 14:28 |  | The GetSystemTimePreciseAsFileTime() API does not exist on 32-bit windows. Adjust [c5dbe93114d318fe] so that it works with this limitation. (check-in: 61d9e204c5 ... user: drh tags: trunk) | 
| 2025-09-09 |  |  | 
| 19:00 |  | If open files in VxWorks, do not unlink the file unless the file was marked as delete-on-close. (check-in: bb6114dbbf ... user: drh tags: vxworks) | 
| 17:39 |  | Do not allow attempts to open files in VxWorks unless the full pathname is provided. (check-in: 4720205249 ... user: drh tags: vxworks) | 
| 10:28 |  | Do not allow the local_getline() function in the CLI to allocate more memory than can be counted using a 32-bit integer, thus limiting the length of an input line to about one gigabyte. Forum post c83b9affa2. (check-in: 0f31711591 ... user: drh tags: trunk) | 
| 2025-09-08 |  |  | 
| 19:56 |  | Fix a potential memory leak in the VxWorks VFS that can occur following an OOM or I/O error during xOpen(). (check-in: cb880145d8 ... user: drh tags: vxworks) | 
| 18:45 |  | In VxWorks, temporary filenames must be full absolute pathnames in order for vxworksFindFileId() to work. (check-in: 1fc94becc5 ... user: drh tags: vxworks) | 
| 17:52 |  | Add the ability to compile with -DOS_VXWORKS=1 on Linux systems, in an effort to debug the VxWorks capabilities without actually having to run VxWorks. (check-in: c87e68d791 ... user: drh tags: vxworks) | 
| 15:15 |  | Add "PRAGMA wal_checkpoint = noop" and SQLITE_CHECKPOINT_NOOP. To request a checkpoint that checkpoints zero frames. (check-in: 4384ad8918 ... user: dan tags: trunk) | 
| 14:40 |  | Merge trunk changes into this branch. (leaf check-in: 3bac76a86b ... user: dan tags: wal-checkpoint-noop) | 
| 14:32 |  | Add documentation comment for SQLITE_CHECKPOINT_NOOP. (check-in: 2b9c495d02 ... user: dan tags: wal-checkpoint-noop) | 
| 2025-09-06 |  |  | 
| 15:27 |  | Improve documentation of SQLITE_DBCONFIG_ENABLE_FTS3_TOKENIZER. (check-in: d7b2fb5363 ... user: dan tags: trunk) | 
| 2025-09-05 |  |  | 
| 13:26 |  | WAL API doc tweaks based on suggestions from forum post 16e94b4bdcf. (check-in: 1bb7eaf784 ... user: stephan tags: trunk) | 
| 12:14 |  | Build portability fixes for HP-UX, reported in forum post d80ecdaddd8. The full fix also requires a patch to autosetup/cc-shared.tcl, but that's pending because it's upstream code. (check-in: 5f38c8aa8f ... user: stephan tags: trunk) | 
| 11:20 |  | Add tests to check that SQLITE_DBCONFIG_ENABLE_FTS3_TOKENIZER is working. (check-in: beb092d810 ... user: dan tags: trunk) | 
| 2025-09-04 |  |  | 
| 19:33 |  | Add "PRAGMA wal_checkpoint = noop" and SQLITE_CHECKPOINT_NOOP. To request a checkpoint that checkpoints zero frames. (check-in: 48be3d020c ... user: dan tags: wal-checkpoint-noop) | 
| 18:00 |  | Generic doc updates in ext/wasm's makefiles. No functional changes. (check-in: 02721457ce ... user: stephan tags: trunk) | 
| 09:26 |  | Use Pgno instead of int for the pgno field in the DbpageCursor object. The same machine code is generated, but using an unsigned value seems safer in the long run. Forum post ccec20a858. (check-in: 26ecdde06c ... user: drh tags: trunk) | 
| 2025-09-03 |  |  | 
| 21:19 |  | Fix a typo in a comment in carray.h. (check-in: a4e137de0e ... user: dan tags: trunk) | 
| 13:39 |  | Random wasm internal doc cleanups. No code changes. (check-in: 70923a00cf ... user: stephan tags: trunk) | 
| 2025-09-02 |  |  | 
| 14:59 |  | Add test case for pushing WHERE constraints into a UNION sub-query that uses virtual tables. (check-in: 13c80e903e ... user: dan tags: trunk) | 
| 2025-09-01 |  |  | 
| 13:01 |  | Latest side-stream autosetup/proj.tcl for improved compile-commands feature detection. (check-in: 55744ca8b8 ... user: stephan tags: trunk) | 
| 00:32 |  | Fix a line-wrap change made in [50234bc9e9fe58da] which broke the docsrc build by splitting a link reference across two lines, which the doc processor does not like. Problem reported in forum post ea1fe5f4215. (check-in: 321938063e ... user: stephan tags: trunk) | 
| 2025-08-29 |  |  | 
| 16:51 |  | Update test script reservebytes.test so that it works with the "prepare" permutation. (check-in: 4ac774364b ... user: dan tags: trunk) | 
| 15:56 |  | Ensure POSIX builds work as expected (no extra usleep() calls) if SQLITE_ENABLE_SETLK_TIMEOUT is defined. (check-in: f04a826bac ... user: dan tags: trunk) | 
| 11:21 |  | Add 'from a check-out' to the mksqlite3h.tcl manifest instructions. (check-in: 7728a74ca8 ... user: stephan tags: trunk) | 
| 11:09 |  | Fix the mksqlite3h.tcl SQLITE_SCM_... changes to work from out-of-tree builds. Problem reported in forum post 812f8d6ff4. (check-in: 127f0ab163 ... user: stephan tags: trunk) | 
| 2025-08-28 |  |  | 
| 17:10 |  | Remove some commented-out debugging lines and fix a doc typo. No code changes. (check-in: af32cb3e30 ... user: stephan tags: trunk) | 
| 17:05 |  | Add macros SQLITE_SCM_BRANCH/TAGS/DATETIME to sqlite3.h, pulled from the fossil-generated manifest.tags. (check-in: db6b6240ad ... user: stephan tags: trunk) | 
| 15:04 |  | Remove sqlite3expert.h from the compiler invocation for that tool, as it (A) doesn't need to be there and (B) it causes some compilers problems (as reported in forum post a1085109e7). This bug was faithfully carried over from the legacy build. (check-in: 9f0a16f6e8 ... user: stephan tags: trunk) | 
| 2025-08-27 |  |  | 
| 11:21 |  | Add test to ensure that changing the number of reserved bytes in a database using VACUUM does not confuse other connections in stock SQLite. (check-in: 6bf54bbc4d ... user: dan tags: trunk) | 
| 2025-08-25 |  |  | 
| 16:41 |  | Fix a typo in a comment. (check-in: d2a74d5c36 ... user: dan tags: trunk) | 
| 14:27 |  | Check that certain ON clauses do not refer to tables to their right as a separate step after parsing a SELECT statement, instead of as part of the planner stage. Fix for forum post ccfb3b5052. (check-in: e0beda7ffe ... user: dan tags: trunk) | 
| 13:04 |  | For the --pagecache option of the CLI, if the SIZE parameter is a power-of-two, automatically round it up to using the PCACHE_HDRSZ. (check-in: e7a2ba842c ... user: drh tags: trunk) | 
| 11:47 |  | Add test case to this branch to show that it handles cases where ON clauses use select-list aliases to refer to columns to their right. (closed check-in: 4d34ab4acf ... user: dan tags: on-clause-fix-3rd-try) | 
| 2025-08-23 |  |  | 
| 18:18 |  | Small simplification to the query flattener logic. (check-in: cbfe3f0c84 ... user: drh tags: trunk) | 
| 16:26 |  | Fix the cases in joinI.test on this branch. (check-in: 23ac237731 ... user: dan tags: on-clause-fix-3rd-try) | 
| 11:09 |  | Fix cksumvfs test case added by [e3bd1feccaee8ff2] so that it works with varying page sizes. Test case change only - no changes to code. (check-in: b6f8f086da ... user: drh tags: trunk) | 
| 2025-08-22 |  |  | 
| 21:46 |  | Fix the sqlite3SetJoinExpr() routine so that it correctly sets the join flags on CASE expressions. (check-in: a4107c947c ... user: drh tags: trunk) | 
| 19:30 |  | Steal the test/joinI.test from the on-clause-error-fix branch that demonstrates that this branch does not work. (check-in: d163535ec8 ... user: drh tags: on-clause-fix-3rd-try) | 
| 19:17 |  | Remember if a SELECT statement contains an ON clause. If it does, then after name resolution, scan the WHERE clause to verify that the ON clause does not reference tables to its right. (check-in: 0ec8a8f644 ... user: drh tags: on-clause-fix-3rd-try) | 
| 18:19 |  | Enhance treeview so that it shows the contents of ON clauses while they are still attached to the SrcItem object and before they are moved into the WHERE clause. (check-in: 3e285bc117 ... user: drh tags: trunk) | 
| 16:34 |  | Experimental minor simplification of the query flattener. (closed check-in: 953ea637d5 ... user: drh tags: simplify-flattener) | 
| 2025-08-21 |  |  | 
| 18:47 |  | Fix a minor typo in a comment. (check-in: 9ada44eb6d ... user: drh tags: trunk) | 
| 17:59 |  | Add config as an alias for the reconfigure makefile target. Minor makefile doc tweaks. Random typo fixes in shell.c.in and tclsqlite.h pointed out by the misspell makefile target. No code changes. (check-in: 9f15182776 ... user: stephan tags: trunk) | 
| 17:39 |  | Remove some resolved TODOs from the makefile. Ensure that testfixture links against TCL_LIBS. (check-in: b7751d1443 ... user: stephan tags: trunk) | 
| 2025-08-20 |  |  | 
| 23:29 |  | The sqlite3_prepare() and similar interfaces should not return SQLITE_ERROR_RETRY. Forum post aa61ab56. (check-in: 4e473cf0c2 ... user: drh tags: trunk) | 
| 23:13 |  | Add an assert() to prove that sqlite3_prepare() does not return SQLITE_ERROR_RETRY. (closed check-in: 6c6a8a596f ... user: drh tags: create-index-error-retry-fix) | 
| 21:12 |  | Change a newly unreachable branch into an ALWAYS(). (check-in: 99556be3fd ... user: drh tags: create-index-error-retry-fix) | 
| 15:42 |  | Avoid returning SQLITE_ERROR_RETRY if collation sequences associated with the table's PK cannot be found when creating an index. Possible fix for forum post aa61ab56ad. (check-in: ac649e5b9c ... user: dan tags: create-index-error-retry-fix) | 
| 11:21 |  | Fix a problem with SQLITE_CHANGESET_CONFLICT conflicts caused by DELETE triggers within a call to sqlite3changset_apply_v2() with SQLITE_CHANGESETAPPLY_IGNORENOOP specified. (check-in: 138e2aab43 ... user: dan tags: trunk) | 
| 09:43 |  | In builds without HAVE_FCHMOD (e.g. WASI), make the chmod() of temp files a no-op, analog to how lack of HAVE_FCHOWN is handled, as discussed in forum post 98d5158d3f. (check-in: 7d13e8e403 ... user: stephan tags: trunk) | 
| 2025-08-19 |  |  | 
| 21:35 |  | Fix a problem causing an SQLITE_CHANGESET_DATA conflict of a DELETE operation to be incorrectly ignored if the SQLITE_CHANGESETAPPLY_IGNORENOOP is specified. (check-in: 78b543f85a ... user: dan tags: trunk) | 
| 2025-08-16 |  |  | 
| 10:17 |  | Add a doc sentence about sqlite3_set_clientdata()'s result value, per request in forum post bda6c22b6a. No code changes. (check-in: c664cee321 ... user: stephan tags: trunk) | 
| 2025-08-14 |  |  | 
| 19:36 |  | Avoid depending on HAVE_STDINT_H and SQLITE_PTRSIZE and other things in non-amalgamation builds of fts5. (check-in: 309fdd4966 ... user: dan tags: trunk) | 
| 2025-08-13 |  |  | 
| 19:59 |  | Minor internal cleanups in the configure script. No behavioral changes. (check-in: c42269e5ce ... user: stephan tags: trunk) | 
| 19:13 |  | Have cksumvfs write checksums to the database file only, not the wal file. Writing them to the wal file breaks wal file recovery. (check-in: e3bd1fecca ... user: dan tags: trunk) | 
| 18:37 |  | In cksumvfs, do not write checksums to the wal file, as this prevents recovery from working. (closed check-in: 5bf48c7e31 ... user: dan tags: cksumvfs-wal-fix) | 
| 18:30 |  | Minor autosetup/README.md tweaks. (check-in: bd7530bf6a ... user: stephan tags: trunk) | 
| 17:58 |  | Move the forced NDEBUG in fts3Int.h up above the #include of assert.h, as reported in forum post 8f16dd9480aad85f. (check-in: 3b06d1c9b6 ... user: stephan tags: trunk) | 
| 16:49 |  | Doc updates in the configure script. No functional changes. (check-in: 47c4a2ddd1 ... user: stephan tags: trunk) | 
| 14:40 |  | Fix a problem with the checksum-vfs and direct-overflow-reads. Forum post 350e147cd1. (check-in: 7b7ce5f17f ... user: dan tags: trunk) | 
| 14:35 |  | Fix a problem with the checksum-vfs and direct-overflow-reads. (closed check-in: 8288edb9f9 ... user: dan tags: cksum-direct-ovfl-fix) | 
| 11:46 |  | Doc cleanups in main.mk. No functional changes. (check-in: 839c998443 ... user: stephan tags: trunk) | 
| 2025-08-12 |  |  | 
| 19:02 |  | Fix a problem with reading wal mode databases from UNC shares on windows introduced by [e88212b10a7829ff]. (check-in: f341e78129 ... user: dan tags: trunk) | 
| 18:57 |  | Fix a problem building testfixture on non SQLITE_OS_WIN platforms. (leaf check-in: 9e6fa73467 ... user: dan tags: win-unc-fix) | 
| 17:55 |  | On windows, when opening a UNC path, fall back to using a single file handle shared between all connections for locking. (check-in: 5c0202d96c ... user: dan tags: win-unc-fix) | 
| 2025-08-11 |  |  | 
| 13:17 |  | The off-by-one fix at [3e627d66ebdef8df] is insufficient. An index can hold twice as many columns as a table, if the table is a WITHOUT ROWID table. The limit should be twice the maximum, not just one more than the maximum. Problem discovered by OSSFuzz. (check-in: a836126bbe ... user: drh tags: trunk) | 
| 10:54 |  | Replace some 32-bit arithmetic in fts3_write.c with 64-bit to avoid the possibility of integer overflow. (check-in: 6711110b1c ... user: dan tags: trunk) | 
| 2025-08-07 |  |  | 
| 20:20 |  | Add an explicit call to sqlite3_initialize() to sqlite3_rsync so that it works when built with SQLITE_OMIT_AUTOINIT, as suggested in forum post c4ec6b33097b8536. (check-in: 8c9db62371 ... user: stephan tags: trunk) | 
| 2025-08-06 |  |  | 
| 19:16 |  | Skip checking for dlopen() on mingw builds and those which include "windows" in their host tuple, as suggested in forum post 2436c8ffed. Those environments identify as Windows for SQLite's purposes so use LoadLibrary(). (check-in: 69b87d4fa8 ... user: stephan tags: trunk) | 
| 19:12 |  | Do not allow the number of terms in an ORDER BY or GROUP BY clause to exceed the maximum number of columns in a table. (check-in: 139e587c7b ... user: drh tags: trunk) | 
| 19:05 |  | Fix Windows makefile breakage caused by [ae9d7c9c922bb241]. (check-in: c41324139d ... user: drh tags: trunk) | 
| 16:51 |  | Add *.obj to the make clean target for the sake of the Unix-on-Windows environments. (check-in: da07e0c02f ... user: stephan tags: trunk) | 
| 15:57 |  | Extend the sqlite3_rsync PATH to include /opt/local/bin, per request in forum post ca3c1bb7ca8e03. (check-in: 79a14a5ebd ... user: stephan tags: trunk) | 
| 12:47 |  | Minor bug fixes in the script added by the previous check-in (check-in: 2e1a2db3f9 ... user: drh tags: trunk) | 
| 12:43 |  | Add a script to build ARM64EC binaries on Windows11 for ARM64. (check-in: 2d5b74b84a ... user: drh tags: trunk) | 
| 2025-08-05 |  |  | 
| 23:01 |  | Fix a possible printf() of a NULL string in fuzzcheck. (Passing a NULL to "%s" in SQLite's internal printf() prints the text "NULL", but it apparently segfaults glibc. Whatever.) Forum post e07d56579b. (check-in: 4ec69baf4f ... user: drh tags: trunk) | 
| 12:01 |  | Remove the *experimental* lsm1 extension from trunk, in as much as readers were thinking that this was a supported extension and were reporting bugs against it.  (check-in: ae9d7c9c92 ...  user: drh tags: trunk) | 
| 10:54 |  | Add bounds checking and error messages and improved comments to the (unused) zorder extension function. Forum post e3f1ede174 (check-in: 6bb717acf7 ... user: drh tags: trunk) | 
| 01:53 |  | Improved defenses against corrupt ZIP archives in the zipfile extension. (check-in: 642e89191d ... user: drh tags: trunk) | 
| 00:16 |  | Back out a recently added NEVER(). (check-in: 4fcdd5bdb0 ... user: drh tags: trunk) | 
| 2025-08-04 |  |  | 
| 23:37 |  | Fix a bug in the fuzzcheck.c test program introduced by [1cccea0508f5c8b8]. (check-in: 90b217e11c ... user: drh tags: trunk) | 
| 23:05 |  | When doing an indexed row-value comparison using an IN operator where the order of the columns in the row-value need to be rearranged in order to match the index, be sure to make affinity conversions before the rearranging of columns so that the correct affinity is applied. Fix for the bug reported by forum post eab63506cf. This problem goes back almost nine years to [ddb5f0558c445699]. (check-in: 8800c13dec ... user: drh tags: trunk) | 
| 20:48 |  | Add an assert() to verify that IN_INDEX_NOOP is not used for vector operations. (check-in: 49c587c6d2 ... user: dan tags: trunk) | 
| 19:35 |  | Remove the "experimental" mark the snapshot interfaces. (check-in: 015dc43f62 ... user: drh tags: trunk) | 
| 11:50 |  | Fix harmless compiler warnings arising from [1cccea0508f5c8b8]. (check-in: d682557464 ... user: drh tags: trunk) | 
| 11:20 |  | Minor doc typo fix in sqliteLimit.h. (check-in: 8bb315ef74 ... user: stephan tags: trunk) | 
| 2025-08-03 |  |  | 
| 08:56 |  | Add a note to sqlite3_deserialize() that a schema name of NULL implies the main db, a former shortcoming pointed out in forum post 0543d8aa8e. (check-in: 50234bc9e9 ... user: stephan tags: trunk) | 
| 2025-08-02 |  |  | 
| 17:37 |  | Tag a condition in os_unix.c as unreachable. (check-in: 151844ad54 ... user: drh tags: trunk) | 
| 15:04 |  | Fiddle: move the About button into the main toolbar. Extend the About text with some info which only shows up (or is only hidden) when terminal mode is available. (check-in: da524d6c61 ... user: stephan tags: trunk) | 
| 14:50 |  | Fiddle: make the toolbar visible in terminal mode and hide non-terminal-mode options when in terminal mode. Move the terminal/split-view toggle button into the toolbar. Improve the view-switching handling. (check-in: 711bf423d2 ... user: stephan tags: trunk) | 
| 13:21 |  | Fiddle: move the About button into the header bar and get it working together with the optional jquery.terminal console mode. (check-in: 9639382c54 ... user: stephan tags: trunk) | 
| 12:06 |  | Fiddle: add an about/info/help view and a button to toggle it. Change the color schema to a more conventional black/white. Force imported databases out of WAL mode, which doesn't work in fiddle. (check-in: 27d1d0100c ... user: stephan tags: trunk) | 
| 2025-07-31 |  |  | 
| 12:25 |  | Ensure that stack space allocated for a flexible array has the correct alignment. Fix to [d4307a0d43f42e96] to for some compilers (DeveloperStudio) and platforms (Sparc). (check-in: 1cccea0508 ... user: drh tags: trunk) | 
| 09:54 |  | Generic JS doc touchups. No functional changes. (check-in: cc5f126ea4 ... user: stephan tags: trunk) | 
| 09:45 |  | On MacOS, when increasing the size of files on FAT filesystems from 0 to 1 byte in order to preserve their inode numbers, be sure to fsync() the file. (check-in: 727efa8559 ... user: drh tags: trunk) | 
| 07:44 |  | Minor doc and consistency improvements to [cf7163f82ca380958]. (check-in: 690a727322 ... user: stephan tags: trunk) | 
| 2025-07-30 |  |  | 
| 21:39 |  | Back out [e283d817e3a9] because further testing has proven it unnecessary. (closed check-in: 7318a00de6 ... user: stephan tags: solaris-alignment) | 
| 21:34 |  | Move the flexarray union members from [527bbc1676a85a9] to the end of their respective objects. (check-in: 3082e07926 ... user: stephan tags: solaris-alignment) | 
| 19:48 |  | Apply alignment-related patches suggested in forum post 41cd226375. (check-in: 527bbc1676 ... user: stephan tags: solaris-alignment) | 
| 18:23 |  | Default pointer size for the __SUNPRO_C compiler is 4 bytes. (check-in: e283d817e3 ... user: drh tags: solaris-alignment) | 
| 16:17 |  | JS: ensure that sqlite3WorkerPromiser.v2.defaultConfig initially refers to sqlite3WorkerPromiser.defaultConfig. This fixes a longstanding oversight rather than a known in-the-wild problem. (check-in: cf7163f82c ... user: stephan tags: trunk) | 
| 15:02 |  | Adjust the previous check-in to replace the db's WASM address with yet another random number, and increase the range of random numbers, reducing the chance of cross-worker db ID collision to 1 in many billions. (check-in: f5e2a72c57 ... user: stephan tags: trunk) | 
| 14:50 |  | JS: add a random element when assigning db IDs to Worker1 db instances. This addresses the npm project's ticket #113, which demonstrates that two independent workers can end up generating non-globally-unique IDs. That's not a bug, per se, but it makes the IDs useless for client-side mapping of db connection IDs to client-side state, so we add some randomness to them. (check-in: 6d2a43075f ... user: stephan tags: trunk) | 
| 13:37 |  | Enhance the printf formatter for %#f such that the minus sign is omitted if the '+' is missing and all digits shown in the rendering are zero. Thus, for example, '%#.2f' renders -0.004 as "0.00" instead of "-0.00". (check-in: 09e1d7c7b4 ... user: drh tags: trunk) | 
| 2025-07-29 |  |  | 
| 11:04 |  | Fix SQLITE_OMIT_WAL builds on windows. (check-in: 575271c3ae ... user: dan tags: trunk) | 
| 2025-07-26 |  |  | 
| 12:51 |  | Add new extended error codes to SQLITE_ERROR and SQLITE_IOERR, for use by extensions. (check-in: 02b241bc4c ... user: drh tags: trunk) | 
| 2025-07-23 |  |  | 
| 12:48 |  | Do not apply the EXISTS-to-JOIN optimization if the subquery contains a LIMIT clause. forum post 2025-07-23T10:59:14z. (check-in: 314c493f27 ... user: drh tags: trunk) | 
| 2025-07-22 |  |  | 
| 15:51 |  | Fix a potentially uninitialized field in the WhereLoop object that is used as part of the skip-ahead-distinct optimization. This fixes various recent forum reports from yxchen. (check-in: c10f9970c6 ... user: drh tags: trunk) | 
| 2025-07-21 |  |  | 
| 18:00 |  | Apply [ebb346c5aa]'s change to sqlite3.1, as reported in forum post bf92f78344. (check-in: 477211be60 ... user: stephan tags: trunk) | 
| 2025-07-20 |  |  | 
| 18:29 |  | Ensure that certain local files are not inadvertently carried over into the snapshot tarball and causing confusing build results. (check-in: 47fdf7ed2c ... user: stephan tags: trunk) | 
| 18:05 |  | Teach autoconf/tea/configure to be able to run from the canonical source tree so that a snapshot build or appropriate symlinks are not necessary in order to test the extension. (check-in: 71b9365b84 ... user: stephan tags: trunk) | 
| 2025-07-19 |  |  | 
| 20:10 |  | Factor out the unusual encoding-change case from vdbeCompareMemString() for a performance improvement in the common case, and a small size reduction. (check-in: cbd46ab8a8 ... user: drh tags: trunk) | 
| 19:54 |  | Small performance optimization in sqlite3VdbeRecordUnpack(). (check-in: eae168def2 ... user: drh tags: trunk) | 
| 18:46 |  | Split the (internal) sqlite3VdbeCheckFk() routine into two variants, sqlite3VdbeCheckFkImmediate() and sqlite3VdbeCheckFkDeferred(), which run faster than the combined general-purpose variant. (check-in: 872b1b52ed ... user: drh tags: trunk) | 
| 18:17 |  | Use fewer CPU cycles to commit a read transaction. (check-in: 861e55656a ... user: drh tags: trunk) | 
| 2025-07-18 |  |  | 
| 22:18 |  | Add SQLITE_SELFORDER1 to JS's sqlite3.capi namespace. It's only useful when built with an sqlite3.c which itself was created with -DSQLITE_ENABLE_ORDERED_SET_AGGREGATES, which the canonical build does not do but custom builds may. (check-in: fdd15e938c ... user: stephan tags: trunk) | 
| 19:17 |  | Fix a minor comment inaccuracy. (check-in: b66fcb61a1 ... user: drh tags: trunk) | 
| 17:40 |  | Increase the precision of the "real time" output from the ".timer" command in the CLI from milliseconds to microseconds. (check-in: c5dbe93114 ... user: drh tags: trunk) | 
| 12:10 |  | Ensure that the accumulator for an aggregate always gets initialized, even when the aggregate is on the right side of a LEFT JOIN and never gets evaluated. This fixes a problem introduced by [663f5dd32d9db832] and found by dbsqlfuzz. Test cases in TH3. (check-in: 235cf6586b ... user: drh tags: trunk) | 
| 2025-07-17 |  |  | 
| 19:18 |  | Pedantic internal build doc fix. (check-in: 9ef429a850 ... user: stephan tags: trunk) | 
| 2025-07-16 |  |  | 
| 20:50 |  | Diverse wasm build cleanups. (check-in: 14ca18f72a ... user: stephan tags: trunk) | 
| 17:24 |  | Fix vtabH.test so that it works on windows even if there are files that begin with "$" in the root directory. (check-in: 19a79219a7 ... user: dan tags: trunk) | 
| 16:56 |  | Remove some stray makefile debug output. (check-in: 7ef22c3d11 ... user: stephan tags: trunk) | 
| 14:51 |  | In the wasm makefiles, replace X:=Y with X=Y because := has different semantics depending on the make impl and its use is arguably a bad muscle-memory habit worth breaking. These makefiles are very specifically for GNU make, but long-term it would be nice to eliminate that requirement. (check-in: d4203311a2 ... user: stephan tags: trunk) | 
| 13:22 |  | wasm: remove some extraneous levels of makefile var and fix the (unsupported) wasmfs speedtest1 build to account for var renaming which happened long before this. (check-in: 35b2315343 ... user: stephan tags: trunk) | 
| 11:56 |  | Remove an unnecessary step from the wasm builds and account for a recent Emscripten-internal code-generation change which caused a post-build cleanup step to not actually clean up (harmless, but adds about 26k of useless stuff to the resulting JS). (check-in: a8ed16989d ... user: stephan tags: trunk) | 
| 11:09 |  | Further internal wasm build cleanups. No functional changes. (check-in: 8364d89c3b ... user: stephan tags: trunk) | 
| 10:34 |  | Internal refactoring of the WASM build makefile rule generator, working towards parallelizing the various build modes (which is not currently possible because most modes share the same sqlite3.wasm output file name). (check-in: 1062b61e9c ... user: stephan tags: trunk) | 
| 09:27 |  | Teach the sqlite3 CLI shell to look in XDG_STATE_HOME for the CLI history file before falling back to its historical location in the user's home directory. (check-in: 1e663374c3 ... user: stephan tags: trunk) | 
| 09:20 |  | Squelch a legitimate but harmless 'discards const' warning. (closed check-in: 73539fe093 ... user: stephan tags: shell-xdg-vars) | 
| 2025-07-15 |  |  | 
| 19:00 |  | Avoid evaluating special vtab operators (e.g. MATCH) that are part of ON clauses attached to left joins from being evaluated too early. Fix for forum post 428ef7c468. (check-in: 9f184f8dfa ... user: dan tags: trunk) | 
| 18:51 |  | Add test case to between.test. (closed check-in: ffebbb7ae9 ... user: dan tags: vtab-leftjoin-fix) | 
| 14:06 |  | Optimize allocation of large tombstone arrays in fts5. (check-in: 0fcc3cbdfa ... user: dan tags: trunk) | 
| 2025-07-14 |  |  | 
| 18:27 |  | Add the snapshot-zip makefile targets for both Windows and Posix. (check-in: 8c73c0c593 ... user: drh tags: trunk) | 
| 16:05 |  | Enhance the "mktoolzip.tcl" script (used by the "tool-zip" makefile target) so that it uses "fossil test-filezip" to build the ZIP archive if available, and so that it also generates windows-dll-*.zip on Windows platforms. (check-in: 63ddc215eb ... user: drh tags: trunk) | 
| 15:40 |  | In rtree.c, ensure that the pReadAux statement is reset whenever the bAuxValid flag is cleared. Fix for forum post 3e45ed31d8. (check-in: 3c0afda372 ... user: dan tags: trunk) | 
| 15:34 |  | In rtree.c, ensure that the pReadAux statement is reset whenever the bAuxValid flag is cleared. Possible fix for forum post 3e45ed31d8. (closed check-in: 1740f9a098 ... user: dan tags: rtree-aux-column-fix) | 
| 14:51 |  | Add new sessions function sqlite3changeset_apply_v3() and its streaming equivalent. This allows changesets to be filtered on a per-change basis, not just per-table. (check-in: 10ebd7a119 ... user: dan tags: trunk) | 
| 09:41 |  | Fix harmless compiler warnings in builds that use SQLITE_OMIT_DATETIME_FUNCS. (check-in: e11fbf9fd6 ... user: drh tags: trunk) | 
| 2025-07-12 |  |  | 
| 18:14 |  | Avoid evaluating special vtab operators (e.g. MATCH) that are part of ON clauses attached to left joins from being evaluated too early. Possible fix for forum post 428ef7c468. (check-in: 18ba07c152 ... user: dan tags: vtab-leftjoin-fix) | 
| 2025-07-11 |  |  | 
| 19:52 |  | wasm: introduce the sqlite3.oo1.DB.wrapHandle() and Stmt.wrapHandle() APIs, which enable clients to wrap (sqlite3*) resp. (sqlite3_stmt*) pointers in their oo1 API counterparts, optionally with or without taking over ownership of the pointer. (check-in: e5d0795495 ... user: stephan tags: trunk) | 
| 19:44 |  | Clearer docs for the unowned db/stmt handle mechanism. (closed check-in: 53401b5435 ... user: stephan tags: oo1-unowned-handles) | 
| 19:34 |  | Correct a one-too-many-exclamation-points bug which caused the opposite of intended semantics in oo1.Stmt.pointer ownership. (check-in: 91e709f36d ... user: stephan tags: oo1-unowned-handles) | 
| 17:02 |  | Additional header comment documentation in the ext/misc/vtablog.c test extension. (check-in: 3656acfaa3 ... user: drh tags: trunk) | 
| 11:06 |  | speedtest1: in WASM builds, disable the "app" test set by default because that one set is triggering file-not-found errors for the db file for reasons not yet understood. (check-in: 9d68971c58 ... user: stephan tags: trunk) | 
| 10:27 |  | Do not export the numerous Emscripten HEAPxy symbols and remove the last remaining JS code which references them. They have long-since been replaced by internal equivalents and a specific feature check on one of them (which is also no longer needed) is triggering an exception from Emscripten. More details are in Emscripten ticket #24656, noting that this is not an Emscripten bug but an Emscripten behavior change brought it to light. (check-in: b9462dc146 ... user: stephan tags: trunk) | 
| 2025-07-10 |  |  | 
| 20:45 |  | Improve the accuracy of the ETC on both devtest and releasetest. (check-in: a3f96cafdc ... user: drh tags: trunk) | 
| 17:19 |  | Add the estkey column to the jobs table of testrunner.db. The plan is to use this to collect measurements that will help generate improved ETAs for a test run. (check-in: 9eccf5267c ... user: drh tags: trunk) | 
| 09:27 |  | Minor internal doc correction in vdbeInt.h. (check-in: c3a2ef639c ... user: stephan tags: trunk) | 
| 09:25 |  | Minor JS doc tweaks. (check-in: 34d893b483 ... user: stephan tags: oo1-unowned-handles) | 
| 2025-07-09 |  |  | 
| 20:11 |  | Fix the empty-table optimization on INTERSECT so that it does not try to use an uninitialized register for LIMIT processing. (check-in: 6918ada008 ... user: drh tags: trunk) | 
| 13:43 |  | wasm: DB.wrapHandle() no longer accepts a DB object as its first argument, as there's no apparent use case for proxying one DB object with another. Doc improvements for the new code. (check-in: 0ee19db02a ... user: stephan tags: oo1-unowned-handles) | 
| 13:13 |  | wasm: add a few tests demonstrating that oo1.Stmt.paramaterCount's new impl does not change visible behaviors. Add the c-pp-filtered files to 'make clean'. (check-in: 3fe6154596 ... user: stephan tags: oo1-unowned-handles) | 
| 2025-07-08 |  |  | 
| 22:11 |  | Fix a possible user-after free following OOM in the EXISTS-to-JOIN optimization. (check-in: 498ee8d514 ... user: drh tags: trunk) | 
| 20:28 |  | Adjustments to test/incrblob4.test so that it works on both Linux and Mac in spite of error message differences between those platforms. Minor change to termIsEquivalent() to provide 100% MC/DC. (check-in: 720387f860 ... user: drh tags: trunk) | 
| 19:53 |  | New optimizations to detect early when queries return no rows due to tables being empty. This includes the EXISTS-to-JOIN optimization that tries to transform EXISTS constraints into additional terms of the FROM clause. Response to forum post 2025-06-27T09:10:26Z. (check-in: e33da6d5dc ... user: drh tags: trunk) | 
| 19:02 |  | Fix a problem with the fix in [5cb8e342e1]. (check-in: 043ff54fb7 ... user: dan tags: trunk) | 
| 18:44 |  | Fix another problem with running fts5 integrity-check in SQLITE_DEBUG builds, this one causing an assert() failure. (check-in: 5cb8e342e1 ... user: dan tags: trunk) | 
| 17:28 |  | Enable the EXISTS-to-JOIN optimization if the outer query has no FROM clause. (closed check-in: 1b9b124f9a ... user: drh tags: empty-table-optimizations) | 
| 14:54 |  | If SQLITE_DEBUG is defined, fts5 does extra checks as part of integrity-check. Ensure that errors from these extra checks are only reported if the other, normal, tests all pass. This fixes a test case in fts5corrupt3.test that was failing if SQLITE_DEBUG was defined. (check-in: 98a53fb276 ... user: dan tags: trunk) | 
| 12:37 |  | Fix test case results in FTS5 that were changed error message improvements in [48044a6b57c0a16c] but which were omitted from that check-in. (check-in: e1f8e0240b ... user: drh tags: trunk) | 
| 2025-07-07 |  |  | 
| 20:47 |  | Only run preupdate-hook tests in incrblob4.test if the preupdate-hook is enabled in the build. (check-in: 56314b6b53 ... user: dan tags: trunk) | 
| 19:03 |  | Small performance optimization in the resolver. (check-in: 840646df06 ... user: drh tags: empty-table-optimizations) | 
| 18:35 |  | Merge the latest trunk enhancements and fixes into the empty-table-optimizations branch. (check-in: f15cdf0757 ... user: drh tags: empty-table-optimizations) | 
| 16:19 |  | Rework the fix to the problem described by forum post b9647a113b so that it provides a more complete fix that covers cases that were not resolved by the original fix, and so that it does not cause performance regressions. (check-in: 28db0d152d ... user: drh tags: trunk) | 
| 15:40 |  | Simplifications to the row-value IN operator logic. Do not let the query planner accept a WhereLoop for a row-value IN operator that uses the same index column more than once. (closed check-in: d2adf61f21 ... user: drh tags: redundant-idx-columns) | 
| 12:11 |  | Work around the Emscripten 4.10 regression described in Emscripten ticket #24656. Problem reported off-list by BrickViking. (check-in: c385475b25 ... user: stephan tags: trunk) | 
| 11:37 |  | Avoid invoking the preupdate hook from within sqlite3_blob_write() if the cursor is already invalid. (check-in: 9f335b9a4e ... user: dan tags: trunk) | 
| 10:54 |  | Back out the fix at [ba7d5bad32ad6aac] because it does not always work and because it causes a performance regression. Add new test cases for row-value lookups of indexes that contain redundant columns, three of which are currently failing. This branch is seeking an improved solution to the redundant index column problem for row-value lookups. (check-in: ad8ddcefab ... user: drh tags: redundant-idx-columns) | 
| 02:18 |  | Fix parser error introduced by [325e547a2195571e]. See forum post 095dbfc06e5. (check-in: 4eefab4494 ... user: drh tags: trunk) | 
| 2025-07-06 |  |  | 
| 15:36 |  | Remove some dead JS code and some extraneous debug output from test code. (check-in: 6e73770a7f ... user: stephan tags: oo1-unowned-handles) | 
| 15:13 |  | JS doc additions. (check-in: 7933505f0c ... user: stephan tags: oo1-unowned-handles) | 
| 15:01 |  | Experimentally add sqlite3.oo1.DB/Stmt.wrapHandle(), which allow DB/Stmt instances to wrap a (sqlite3*)/(sqlite3_stmt*) optionally with or without taking ownership of it. The intent is to enable mix-and-match use of the C API, the oo1 API, and any other hypothetical API which exposes those pointers. oo1.Stmt.parameterCount is now a property access interceptor like Stmt.columnCount is, but that doesn't change how it's used. (check-in: 1227543b87 ... user: stephan tags: oo1-unowned-handles) | 
| 01:19 |  | Improvements to the EXPLAIN QUERY PLAN output for EXISTS-to-JOIN. (check-in: 6b1ecbaa2e ... user: drh tags: empty-table-optimizations) | 
| 2025-07-05 |  |  | 
| 23:33 |  | Enhance the EXISTS-to-JOIN optimization so that it works on EXISTS subqueries that do not have a WHERE clause, and so that it works on nested EXISTS subqueries. (check-in: c1d5295724 ... user: drh tags: empty-table-optimizations) | 
| 2025-07-04 |  |  | 
| 18:32 |  | Add 'reconfigure' target to Makefile.in to re-run the configure script with the same flags it was generated with. (check-in: c60907e77b ... user: stephan tags: trunk) | 
| 17:20 |  | Propagate the -ldl and -lpthread flags, if needed, into sqlite3.pc, per request in forum post 44a58c807353162f. (check-in: 64f5f14322 ... user: stephan tags: trunk) | 
| 14:24 |  | Merge fixes from trunk into the empty-table-optimizations branch (check-in: 6f98b16d21 ... user: drh tags: empty-table-optimizations) | 
| 12:25 |  | Fix harmless compiler warnings. (check-in: 13af4acebe ... user: drh tags: trunk) | 
| 11:48 |  | Remove an ALWAYS() added by [960a8e6fc91f4] that turns out to be false in some cases of malformed SQL. (check-in: cc8171461b ... user: drh tags: trunk) | 
| 11:06 |  | Do not allow the EXISTS-to-JOIN optimization if the EXISTS clause is based on a view, since that view might expand into a join. (check-in: 872c41fedd ... user: drh tags: empty-table-optimizations) | 
| 10:26 |  | Improve the early-termination optimization so that it works in queries which use the LIKE optimization in the outer loop. (check-in: b4e4d14824 ... user: drh tags: empty-table-optimizations) | 
| 2025-07-03 |  |  | 
| 20:51 |  | Merge the latest trunk fixes and enhancements into the empty-table-optimizations branch (check-in: d4f47e04f5 ... user: drh tags: empty-table-optimizations) | 
| 16:05 |  | Fix a few cases where LIMIT clauses that were part of scalar sub-queries on virtual tables were not being passed to xBestIndex methods correctly. (check-in: 960a8e6fc9 ... user: dan tags: trunk) | 
| 15:50 |  | Improvements to sqlite3_vtab_rhs_value() logging in the ext/misc/vtablog.c extension. (closed check-in: 25131ee84f ... user: drh tags: vtab-limit-fixes) | 
| 15:32 |  | Make the value of an explicit LIMIT clause on a scalar sub-query available to xBestIndex for simple "LIMIT 0" and "LIMIT 1" queries. (check-in: 33b6a63caa ... user: dan tags: vtab-limit-fixes) | 
| 14:28 |  | Make handling of LIMIT clauses in correlated sub-queries on virtual tables more efficient. (check-in: 7214cb2a5b ... user: dan tags: vtab-limit-fixes) | 
| 14:10 |  | Enhancements to the xBestIndex output from the ext/misc/vtablog.c extension. (check-in: 8b31acc0b1 ... user: drh tags: trunk) | 
| 11:52 |  | Fix an uninitialized variable added yesterday by [d27d34fb746280e7]. This problem was discovered overnight by OSSFuzz. (check-in: 6db4703f11 ... user: drh tags: trunk) | 
| 00:17 |  | Minor tweaks to the exists-to-join optimization. (check-in: 9cb600ad57 ... user: drh tags: empty-table-optimizations) | 
| 2025-07-02 |  |  | 
| 20:46 |  | Merge in the exists-to-join optimization that has been modified to relax the requirement of having an indexed join constraint. (check-in: 1c1aef2b7f ... user: drh tags: empty-table-optimizations) | 
| 17:43 |  | Fix VDBE coverage (check-in: ff593a16d6 ... user: drh tags: empty-table-optimizations) | 
| 14:53 |  | Early exit if one of the inner loops of a 3-way or larger join is an empty table. (check-in: eaad6ac707 ... user: drh tags: empty-table-optimizations) | 
| 13:19 |  | If the LHS for an EXCEPT or INTERSECT operator is empty, skip over the computation of the RHS. (check-in: 13f096ae8a ... user: drh tags: empty-table-optimizations) | 
| 11:47 |  | Improve the bytecode for joins such that it exits earlier if it determines that no output is possible. (check-in: 2d2b61cba4 ... user: drh tags: trunk) | 
| 02:03 |  | Ensure that Expr.op2 values for TK_AGG_FUNCTION nodes are adjusted when query flattening. (check-in: d27d34fb74 ... user: drh tags: trunk) | 
| 2025-07-01 |  |  | 
| 23:17 |  | Improved comments on bytecode used to implement aggregate queries, to aid in debugging. (check-in: a1a8b85cdb ... user: drh tags: trunk) | 
| 20:32 |  | Improved byte-code comments for the short-circuit optimization of [0083d5169a46104a], to aid in debugging. (check-in: 113f9d10e3 ... user: drh tags: trunk) | 
| 17:36 |  | Cache and reuse virtual table cursors in the bytecode engine. (check-in: 2d187d4232 ... user: drh tags: trunk) | 
| 16:21 |  | Avoid an assert() failure in fts5 that may occur when processing corrupt records. (check-in: 8afd6ca857 ... user: dan tags: trunk) | 
| 15:13 |  | Merge trunk fixes into the empty-table-optimizations branch. (closed check-in: 63306e447e ... user: drh tags: empty-table-optimizations) | 
| 12:43 |  | When attempting to optimize "expr AND false" to "false" and "expr IN ()" to "false", take care not to delete aggregate functions in the "expr" as doing so can change the meaning of the query. See forum thread f4878de3e7. (check-in: 77397bd67d ... user: drh tags: trunk) | 
| 2025-06-30 |  |  | 
| 21:07 |  | More aggressive optimization of addrHalt for RIGHT JOIN. (check-in: 5e51d1c0df ... user: drh tags: empty-table-optimizations) | 
| 20:19 |  | Compute WhereLevel.addrBrk and .addrHalt early so that those labels can be used to abort loops early. Use this to improve performance on two more of the cases described by forum post 52651713ac. (check-in: 6fc0b9ac23 ... user: drh tags: empty-table-optimizations) | 
| 16:41 |  | Strive to skip the evaluation of scalar subqueries that are part of a larger expression if the result from the scalar subquery does not change the result of the overall expression. (check-in: 0083d5169a ... user: drh tags: trunk) | 
| 15:56 |  | Correct ~/.local/config/... to ~/.config/... when looking for sqliterc when XDG_CONFIG_HOME is not set. Internal doc touchups. (check-in: ebb346c5aa ... user: stephan tags: shell-xdg-vars) | 
| 15:21 |  | Add support for using $XDG_STATE_HOME/sqlite_history or ~/.local/state/sqlite_history before fallback back to the historical default of ~/.sqlite_history. Update sqlite3.1 (man page) with the new semantics. (check-in: 6b780655cc ... user: stephan tags: shell-xdg-vars) | 
| 12:14 |  | For all binary operators, try to avoid computing subquery operands if the other operand is NULL. (closed check-in: d86eb16283 ... user: drh tags: optimize-null-values) | 
| 11:46 |  | Preliminary refactoring of the XDG_CONFIG_HOME support to support the pending addition of other XDG-configurable options. In response to forum post 31db1a23f9. (check-in: 69b9244e3a ... user: stephan tags: shell-xdg-vars) | 
| 11:04 |  | Slightly smaller and faster version of the previous check-in. (check-in: f6e6fd02f4 ... user: drh tags: optimize-null-values) | 
| 11:00 |  | Minor API doc typo fixes from brickviking. (check-in: b48d951916 ... user: stephan tags: trunk) | 
| 10:30 |  | Factor out the code that tries to avoid evaluating subquery operands if the other operand is NULL into a subroutine, so that it can be more easily reused by other parts of the code generator. (check-in: 3c6c71bcea ... user: drh tags: optimize-null-values) | 
| 2025-06-29 |  |  | 
| 07:32 |  | Minor API doc typo fixes from brickviking. (check-in: c7cf9dcb69 ... user: stephan tags: trunk) | 
| 2025-06-28 |  |  | 
| 17:59 |  | Improve the bytecode generated for comparisons so that if one operand is a subquery and the other operand evaluates to NULL, the subquery operand is not even computed. This fixes 5 of the 11 slow queries described in forum post 52651713ac. (check-in: f147bc0477 ... user: drh tags: optimize-null-values) | 
| 2025-06-27 |  |  | 
| 19:02 |  | Raise an error right away if the number of aggregate terms in a query exceeds the maximum number of columns. (check-in: 5508b56fd2 ... user: drh tags: trunk) | 
| 12:46 |  | API doc typo fixes and closing DD element tags from brickviking. (check-in: c9ddd15b01 ... user: stephan tags: trunk) | 
| 2025-06-26 |  |  | 
| 18:57 |  | Work around an apparent GCC UBSAN bug. See forum thread 1d7c25d4a2d for details. (check-in: 23056532e5 ... user: drh tags: trunk) | 
| 2025-06-25 |  |  | 
| 20:42 |  | Minor API doc fixes sent off-list from brickviking. (check-in: cb4d05633a ... user: stephan tags: trunk) | 
| 2025-06-24 |  |  | 
| 18:27 |  | Similar fix to the previous check-in, but this time for sqlite3_preupdate_new(). (check-in: 9aaff764d5 ... user: drh tags: trunk) | 
| 15:58 |  | Range check the column index on the sqlite3_preupdate_old() interface and return SQLITE_MISUSE if too large. Forum post b617e49728. (check-in: 6a5701e6c7 ... user: drh tags: trunk) | 
| 07:38 |  | API doc typo fixes and one rephrasing improvement from brickviking. (check-in: 85e8060c47 ... user: stephan tags: trunk) | 
| 2025-06-23 |  |  | 
| 19:38 |  | Improve some of the error messages emitted by fts5 when it encounters corruption. (check-in: 48044a6b57 ... user: dan tags: trunk) | 
| 19:00 |  | Fix an SQL typo introduced by the previous check-in. (check-in: 72a81d247b ... user: drh tags: trunk) | 
| 16:51 |  | Escape the "_" character in LIKE patterns in the CLI. Forum post 6a89702f5d. (check-in: cf61cd359e ... user: drh tags: trunk) | 
| 13:42 |  | Fix a harmless compiler warning injected by [c978aed3b6f82b3d]. (check-in: f46500247b ... user: drh tags: trunk) | 
| 13:28 |  | Improved "statement aborts at ..." log-file messages that identify the trigger that caused the abort. (check-in: 7691264fe3 ... user: drh tags: trunk) | 
| 2025-06-22 |  |  | 
| 22:48 |  | Add the --disable-rpath configure script flag to address forum post 13cac3b56516f849. (check-in: a59d9bb25e ... user: stephan tags: trunk) | 
| 12:04 |  | API doc typo fixes from brickviking. (check-in: 18e07f8f53 ... user: stephan tags: trunk) | 
| 2025-06-21 |  |  | 
| 16:33 |  | Minor JS test cleanups prompted by a linter. (check-in: 764235a3d1 ... user: stephan tags: trunk) | 
| 16:18 |  | Remove some duplicated JS tests. (check-in: d1cb9ed79d ... user: stephan tags: trunk) | 
| 16:02 |  | Reduce the maximum --size flag for JS kvvfs speedtest1 from 4 to 2, as --size 3 and 4 are overflowing the kvvfs storage limits. (check-in: b77b3adc82 ... user: stephan tags: trunk) | 
| 15:58 |  | Give oo1.Stmt.get() similar treatment to [8c187140a60b]. This is an internal change only - the API is unaffected. (check-in: f5a7abc0a4 ... user: stephan tags: trunk) | 
| 15:38 |  | Extend the SEE-via-kvvfs tests to include all three key types. (check-in: fc001aa5ee ... user: stephan tags: trunk) | 
| 14:38 |  | Rework how JS's oo1.DB.exec() flags its Stmt objects to make certain Stmt APIs illegal (i.e. throwing) if called while that Stmt is being step()ped by DB.exec() (which can happen via client-provided per-result-row callbacks). This is an internal change only - the API is unaffected. Remove some unrelated dead code. (check-in: 8c187140a6 ... user: stephan tags: trunk) | 
| 06:07 |  | Numerous small doc typo from BrickViking. (check-in: 3aab4415de ... user: stephan tags: trunk) | 
| 2025-06-20 |  |  | 
| 13:12 |  | Enhance sqlite3BtreeSetPageSize() so that it detects early if no changes are needed and exists with SQLITE_OK. (check-in: c978aed3b6 ... user: drh tags: trunk) | 
| 2025-06-19 |  |  | 
| 20:19 |  | Fix an out-of-order local variable declaration in ext/misc/fileio.c. (check-in: a88bb75288 ... user: drh tags: trunk) | 
| 20:00 |  | Improve the accuracy of sqlite3BtreeRowCountEst(). (check-in: e553fb538e ... user: drh tags: trunk) | 
| 19:33 |  | Generalize the indexCellCompare() so that works on any index page, not just the current page that a cursor is pointing to. (check-in: b305a7f5db ... user: drh tags: trunk) | 
| 12:35 |  | A slight doc rephrasing for clarity, suggested in the forum. (check-in: 0d6084a398 ... user: stephan tags: trunk) | 
| 10:46 |  | Doc typo fixes from forum post 0bce273669. (check-in: 8f97e3f00d ... user: stephan tags: trunk) | 
| 10:25 |  | Expose the column metadata APIs to WASM (which does not require a non-default sqlite3.c build, contrary to my prior mistaken claims). This adds only 424 bytes to sqlite3.wasm and 660 bytes to sqlite3.js. (check-in: 3cf983816c ... user: stephan tags: trunk) | 
| 2025-06-18 |  |  | 
| 19:04 |  | Extend the pedantic enforcement of type in STRICT tables to cover VIRTUAL columns. (check-in: b734c74e55 ... user: drh tags: trunk) | 
| 16:17 |  | Enforce judgmental typing on STORED generated columns for STRICT tables. Forum post 6caf195248. (check-in: 5e9279bff0 ... user: drh tags: trunk) | 
| 15:37 |  | Export the column-metadata APIs to WASM. ~~Doing so requires a non-default build of sqlite3.c, so this is a proof-of-concept branch saved just in case SQLITE_ENABLE_COLUMN_METADATA ever becomes the default for sqlite3.c.~~ Edit: that was a misunderstanding on my part. This build increases the .wasm file by only 424 bytes and the JS by 660, so there's no blocker to merging this.  (closed check-in: 59db3f639d ...  user: stephan tags: wasm-column-metadata) | 
| 15:22 |  | Add the --enable-column-metadata flag to the configure script (off by default). It's only available in the canonical build, not the autoconf build, because it changes how sqlite3.c gets generated. (check-in: 986b601db1 ... user: stephan tags: trunk) | 
| 14:17 |  | Eliminate configure/build discrepancies in how 'make tclextension-...' works vs other makefile-side handling of the extension, conforming to the former. (check-in: 7ca545fd3b ... user: stephan tags: trunk) | 
| 14:14 |  | Change the definition of SQLITE_DYNAMIC to a function that has exactly the same type as sqlite3_destructor_type, in an effort to work around possible legacy compiler bugs. (check-in: 83553edf78 ... user: drh tags: trunk) | 
| 11:11 |  | Add the --tclConfig.sh FILE flag to buildtclext.tcl to eliminate the discrepancy between the configure-time-detected tclConfig.sh and the one auto-detected by the tclextension family of makefile targets. Add the tclextension-all target to run all of the various tclextension-... targets in their natural order to facilitate testing changes like this one. Update the --help text to note that --with-tclsh should only be used if there's a specific need, and that --with-tcl is generall preferred. This ... (check-in: 928976abcb ... user: stephan tags: trunk) | 
| 2025-06-17 |  |  | 
| 20:50 |  | tea: remove the vsatisfies 8.6- enforcement from the extension because at least one platform with tcl 8.6 is inexplicably failing it. Forum post fde857fb8101a4be (check-in: 7331e32a76 ... user: stephan tags: trunk) | 
| 19:10 |  | Avoid writing frames with no checksums into the wal file if a savepoint is rolled back after dirty pages have already been spilled into the wal file. (check-in: 5973f9b9aa ... user: drh tags: trunk) | 
| 18:57 |  | Fix a corner-case for [9441fff52cc4e19c]. (check-in: 336a59eb3a ... user: drh tags: trunk) | 
| 18:17 |  | Update walcksum.test so that it works with SQLITE_DEFAULT_AUTOVACUUM=1 builds. (closed check-in: 0da16f6c62 ... user: dan tags: forum-b490f726db-fix) | 
| 17:15 |  | Extend [b5c6cb13cff5] to use a wildcard on darwin* because some OSes report a version number in the suffix. Forum post 0c4bbc2962dfb06e. (check-in: d960bd6f92 ... user: stephan tags: trunk) | 
| 14:56 |  | Minor proj.tcl portability fixes and cleanups suggested in forum post 7b218c3c9f. Tested on Linux, Msys, Cygwin, and Haiku. (check-in: b5c6cb13cf ... user: stephan tags: trunk) | 
| 14:37 |  | proj.tcl portability fixes and cleanups suggested in forum post 7b218c3c9f. Branching so that it can be tested on msys/cygwin/haiku before committing to it. (closed check-in: 89552e9137 ... user: stephan tags: linux-named-apple) | 
| 11:36 |  | Avoid writing frames with no checksums into the wal file if a savepoint is rolled back after dirty pages have already been spilled into the wal file. Possible fix for forum post b490f726db. (check-in: 3a3269fe8e ... user: dan tags: forum-b490f726db-fix) | 
| 2025-06-16 |  |  | 
| 17:36 |  | Fix an issue going back to version 3.39.0 with transitive IS constraints in queries that make use of RIGHT JOIN. Problem reported by forum post 68f29a2005. (check-in: 9441fff52c ... user: drh tags: trunk) | 
| 16:07 |  | Improved debugging output for the transitive constraint optimization. (check-in: 94b53c20e9 ... user: drh tags: trunk) | 
| 15:34 |  | Make the show-%p-az-zero hack of the previous check-in configurable at run-time using the 0x100000 bit of either .treetrace or .wheretrace. As before, this is all a no-op except for debugging builds. (check-in: a29627d7e7 ... user: drh tags: trunk) | 
| 13:51 |  | Add an "#if 0" that can be changed to "1" to cause all %p output to render as 0, thus making comparisons of .treetrace and .wheretrace output easier. (check-in: 65e2704c0b ... user: drh tags: trunk) | 
| 2025-06-14 |  |  | 
| 18:02 |  | Generalize the indexCellCompare() so that works on any index page, not just the current page that a cursor is pointing to. (leaf check-in: eda518028f ... user: drh tags: flex-search) | 
| 2025-06-12 |  |  | 
| 07:35 |  | Have sqlite3_setlk_timeout() take the database handle mutex. This fixes an assert() failure that could occur if sqlite3_setlk_timeout() were called on a threadsafe handle. (check-in: a95d126e13 ... user: dan tags: trunk) | 
| 2025-06-11 |  |  | 
| 15:03 |  | Fix a problem with UPDATEs on fts5 tables that contain blob values. (check-in: badf3014bd ... user: dan tags: trunk) | 
| 00:01 |  | Fix the concat_ws() SQL function so that it includes empty strings in the concatenation. Forum post 52503ac21d. (check-in: 80a78987da ... user: drh tags: trunk) | 
| 2025-06-10 |  |  | 
| 19:52 |  | Improved selection of the divisor when subdividing nested Bitvec objects. This fixes a potential stack overflow that can occur when the database size is within 60 pages of the maximum allowed by the file format. (check-in: f7ab764ed9 ... user: drh tags: trunk) | 
| 18:26 |  | Minor corrections to the new Bitvec testing logic. (check-in: 77b79ca127 ... user: drh tags: trunk) | 
| 17:22 |  | Enhancements to sqlite3BitvecBuiltinTest() that allow testing code to create very large Bitvec objects that do not use the linear array cross-check. (check-in: c5680672ca ... user: drh tags: trunk) | 
| 16:02 |  | Improved diagnostics for Bitvec: Add the sqlite3ShowBitvec() routine that can be called from a debugger (only available with SQLITE_DEBUG). Add new output opcodes for sqlite3BitvecBuiltinTest(). (check-in: dea1e37fa6 ... user: drh tags: trunk) | 
| 2025-06-09 |  |  | 
| 22:38 |  | Adjustments to ext/misc/fileio.c in an attempt to get it to build using mingw. (check-in: 96b14a3f11 ... user: drh tags: trunk) | 
| 2025-06-06 |  |  | 
| 23:10 |  | Remove unnecessary whitespace and otherwise improve comments in the wherecode.c module. No coding changes. (check-in: 2eb4e9bf0f ... user: drh tags: trunk) | 
| 23:02 |  | Update the "msort" function in Lemon so that it works with lists of any length, and also so that the sort is stable. This patch was motivated by forum post 63750d717c but was independently developed, then tested by temporarily setting LISTSIZE to 2. (check-in: aba5c3135e ... user: drh tags: trunk) | 
| 2025-06-05 |  |  | 
| 20:12 |  | Remove the clunky test_windirent.h and test_windirent.c files from src/ and replace them with a much cleaner and more compact ext/misc/windirent.h. (check-in: acc978df52 ... user: drh tags: trunk) | 
| 18:28 |  | Enhance the FSDIR virtual table with a new "level" column. The query planner knows how to optimize to avoid search deeper than the maximum requested level. (check-in: 1ddc0f9e79 ... user: drh tags: trunk) | 
| 2025-06-04 |  |  | 
| 18:34 |  | tea build: add an info-exists check after a 'scan' call, as scan does not create its target vars on error. Problem reported at forum post fde857fb8101a4be and triggers when the 'vsatisfies' test for the host's Tcl version fails (so the build would fail anyway, but will fail more informatively with this fix). (check-in: 4f21874d5d ... user: stephan tags: trunk) | 
| 2025-06-03 |  |  | 
| 18:10 |  | Apply the duplicate 'export default' workaround to the (untested and unsupported) node-specific build rules in mkwasmbuilds.c to resolve a problem reported off-list by Thomas Steiner. (check-in: 2f8a1b7953 ... user: stephan tags: trunk) | 
| 10:49 |  | Enhance sqlite3_rsync so that it works even if the replica database is initially malformed. (check-in: ea1754f7d8 ... user: drh tags: trunk) | 
| 10:28 |  | Fix an off-by-one error in sqlite3_rsync, reported in forum post b6d78f60fc. (check-in: 27d9e8e79b ... user: drh tags: trunk) | 
| 10:18 |  | Fix harmless compiler warning introduced by the setlk-snapshot-fix merge. (check-in: c1f20f89d9 ... user: drh tags: trunk) | 
| 2025-06-02 |  |  | 
| 23:34 |  | Fix JSONB edit so that when it is trying to reduce the size of an element it understands 0xf0 (8-byte) sizes. (check-in: 5b3de6e8ab ... user: drh tags: trunk) | 
| 18:58 |  | Increase the version number to 3.51.0 (check-in: ed69d44327 ... user: drh tags: trunk) | 
| 18:48 |  | Fixes to ensure SQLITE_ENABLE_SETLK_TIMEOUT builds use a blocking lock and do not call xSleep() when (a) opening a snapshot transaction, and (b) when blocked by another process running recovery. (check-in: 7f9c0cdd06 ... user: dan tags: trunk) | 
| 18:37 |  | Fix os_win.c so that SQLITE_ENABLE_SETLK_TIMEOUT=2 builds work on windows. (closed check-in: 8efb95e0d4 ... user: dan tags: setlk-snapshot-fix) | 
| 18:34 |  | Improve the accuracy of affinity and collating sequence analysis for NATURAL JOINs to the left of RIGHT JOINs where source tables are views or subqueries. Initial problem report in forum post 829306db47. (check-in: f184d1d236 ... user: drh tags: trunk) | 
| 18:09 |  | Updates to new test cases to run with SQLITE_ENABLE_SETLK_TIMEOUT=2 builds. (check-in: c6d3e3542f ... user: dan tags: setlk-snapshot-fix) | 
| 17:44 |  | Merge trunk changes into this branch. (check-in: 7d27451804 ... user: dan tags: setlk-snapshot-fix) | 
| 15:17 |  | Slight modernization of the TEA README.txt. (check-in: bf7be67e3f ... user: stephan tags: trunk) | 
| 15:10 |  | TEA: remove the stale man page and references to the MSC makefiles from the README.txt. Based on discussion at forum post 87e6660191a472c5. A couple of weeks ago we internally discussed pulling in the MSC makefiles from the 3.49 tree but they are stale and possibly unused, so opted against it for the time being. (check-in: c9888a4095 ... user: stephan tags: trunk) | 
| 13:54 |  | Remove an unnecessary parameter from sqlite3VdbeRecordUnpack(). Improved comments and assert()s on KeyInfo. (check-in: 387f4c4d98 ... user: drh tags: trunk) | 
| 09:49 |  | Fix stale comments related to KeyInfo. Add new assert()s associated with memory management of KeyInfo. (check-in: abd805bc76 ... user: drh tags: trunk) | 
| 2025-06-01 |  |  | 
| 21:38 |  | Fix an off-by-one error in the size computation of a vdbe-sorter. Forum post c1cc8b057a. Problem introduced by checkin [d4307a0d43f42e96]. (check-in: 8b7a7fcf62 ... user: drh tags: trunk) | 
| 16:10 |  | Fix VACUUM so that it works even when ATTACH_WRITE is disabled. (check-in: 42494f85ac ... user: drh tags: trunk) | 
| 2025-05-31 |  |  | 
| 20:51 |  | Enhance "box" and "column" mode formatting in the CLI to better deal with double-wide characters. (check-in: b0de22ed0a ... user: drh tags: trunk) | 
| 19:55 |  | Update a few test scripts so that they run on windows. (check-in: 14a18f4e3a ... user: dan tags: setlk-snapshot-fix) | 
| 18:26 |  | New makefile target "xdevtest" works like "releasetest" except that it omits the "verify-source" dependency so that it can be run with uncommitted changes in the source tree. (check-in: 1afb1ac3e9 ... user: drh tags: trunk) | 
| 18:16 |  | Fix an affinity problem caused by a USING or NATURAL JOIN on the LHS of a FULL JOIN. Forum post 5028c785b6. (check-in: 8d393ca07f ... user: dan tags: trunk) | 
| 16:17 |  | Relax query flattener constraint (3b) and thereby allow flattening the RHS of a LEFT JOIN even if the RHS contains a virtual table. This was previously disallowed by [9dbae1df75219e2a] as a performance optimization. It turns out that the constraint causes performance issues, and we do not have a record of any performance issue that it solves. (check-in: 1ddaa92057 ... user: drh tags: trunk) | 
| 15:10 |  | Fix assert() statements in os_unix.c and os_win.c. Allow walsetlk_recover.test to run in non-SQLITE_ENABLE_SNAPSHOT builds. (check-in: 9f521ecda2 ... user: dan tags: setlk-snapshot-fix) | 
| 11:08 |  | Move a mis-located makefile comment block. (check-in: 7d884386be ... user: stephan tags: trunk) | 
| 11:02 |  | tcl extension: UDFs may now 'break' to return an SQL NULL. Add the (eval -asdict) flag to use a dict, instead of an array, for the eval row data. (check-in: 413a626b5c ... user: stephan tags: trunk) | 
| 09:44 |  | Add some missing UNUSED_PARAMETER() annotations to squelch downstream build warnings when using -Wextra -pedantic. (check-in: a98a2f4935 ... user: stephan tags: trunk) | 
| 2025-05-30 |  |  | 
| 22:58 |  | Follow-up to the previous: The same optimization suppression needs to happen if the left-hand side is coming from a LEFT JOIN. (check-in: cf5b37b3a3 ... user: drh tags: trunk) | 
| 20:52 |  | If blocking locks are enabled, avoid using the busy handler when blocked by another process running recovery. (check-in: a35236757a ... user: dan tags: setlk-snapshot-fix) | 
| 19:55 |  | When synthesizing an ON constraint from a USING or NATURAL, if the left-hand side is coming from a RIGHT JOIN, be sure to set the EP_CanBeNull flag so that the optimizer knows to check for NULL even if the column has a NOT NULL constraint. Fix for the problem reported by forum post 4fc70203b61 (check-in: 60adc78a22 ... user: drh tags: trunk) | 
| 18:23 |  | Avoid invoking the busy-handler if a blocking lock times out while attempting to open a snapshot transaction. (check-in: fe11f85fd3 ... user: dan tags: setlk-snapshot-fix) | 
| 16:08 |  | Configure-related fixes and additions, most notably integration of self-tests for proj.tcl's APIs. Teaish make-install fixes based on the discussion in forum thread 87e6660191a472c5. (check-in: 2e486f8fd0 ... user: stephan tags: trunk) | 
| 15:46 |  | Random typo fixes in JNI docs. (check-in: f63608a384 ... user: stephan tags: trunk) | 
| 15:43 |  | Use a more robust backup definition for offsetof(). (check-in: 22441955e0 ... user: drh tags: trunk) | 
| 11:14 |  | Add "include <stddef.h>" to fts5 and rtree to ensure that they use the system version of the offsetof() macro when it is available, as the built-in version triggers ubsan errors with clang. (check-in: 838deb7f34 ... user: dan tags: trunk) | 
| 10:18 |  | Squelch an interesting but harmless struct initialization warning emitted after an emsdk update. Fix JS breakage introduced by changes in Emscripten 4.0.7: manually export the HEAPxyz symbols which used to be exposed by default. (check-in: 10d0897cc9 ... user: stephan tags: trunk) | 
| 2025-05-29 |  |  | 
| 20:29 |  | Fix the missing -lm link flag for the sqlite3 shell when building the autoconf bundle with --disable-static-shell, as reported in forum post 5adf1c932a. (check-in: f6318c4a6b ... user: stephan tags: trunk) | 
| 18:44 |  | Extend the fix for ticket [623eff57e76d45f6] so that it covers RIGHT JOIN in addition to LEFT JOIN. Problem reported by forum post 2025-05-29T15:10:14Z. (check-in: 29b1e1b976 ... user: drh tags: trunk) | 
| 17:46 |  | Fix a goofy hash function in Lemon. No changes to SQLite itself. (check-in: d6cbabe23d ... user: drh tags: trunk) | 
| 14:26 |  | Version 3.50.0 (check-in: dfc790f998 ... user: drh tags: trunk, release, major-release, version-3.50.0) | 
| 2025-05-28 |  |  | 
| 16:56 |  | Do not use a partial index unless the WHERE clause uses one or more columns from the table being indexed. This resolves the issue reported by forum post 2025-05-28T13:03:40Z. Test cases are in TH3. (check-in: f037ca064d ... user: drh tags: trunk) | 
| 14:59 |  | Do not use a partial index if the truth of the WHERE clause does not depend on at least one column from the table being indexed. (closed check-in: cbe2a392e4 ... user: drh tags: silly-partial-indexes) | 
| 2025-05-26 |  |  | 
| 23:20 |  | Slight change to tool/split-sqlite3c.tcl to make it compatible with jimsh (check-in: fe670ddda9 ... user: drh tags: trunk) | 
| 17:29 |  | Fix a bug in the speedtest.tcl testing script that was introduced by [7e9845433ff26bdc] (check-in: e4ffa86aa6 ... user: drh tags: trunk) | 
| 16:59 |  | In the TEA build, enable USE_TCL_STUBS on Mac platforms. (check-in: 0b5415151c ... user: stephan tags: trunk) | 
| 16:17 |  | On OpenBSD, do not test fuzzcheck with ubsan as part of "make devtest". (check-in: a3dabd599c ... user: dan tags: trunk) | 
| 15:36 |  | Avoid calls to sprintf() in Lemon, since OpenBSD hates sprintf(). (check-in: ed17858ddf ... user: drh tags: trunk) | 
| 15:20 |  | When detecting TCLLIBDIR, skip over //zipfs paths, as the (file isdirectory) command will actually return true for those, but they're useless for installation purposes. This resolves the tea build's inability to install on stock openbsd. (check-in: 5b29724a32 ... user: stephan tags: trunk) | 
| 07:15 |  | tclsqlite.c doc typo fix. (check-in: de0dfac1e5 ... user: stephan tags: trunk) | 
| 2025-05-24 |  |  | 
| 20:20 |  | Change json_group_object() so that it ignores entries where the label is NULL. Forum post e5bd251fb5. (check-in: 28215d131c ... user: drh tags: trunk) | 
| 16:01 |  | vtablog.c doc fixes reported in forum post 416d1e37b2. (check-in: 5d25f62bcd ... user: stephan tags: trunk) | 
| 01:34 |  | Amend the previous: Set SQLITE_JSON_MAX_DEPTH to 500 on *all* builds of fuzzcheck. (check-in: c266e38c5b ... user: drh tags: trunk) | 
| 2025-05-23 |  |  | 
| 20:50 |  | Limit JSON recursion depth to 500 when running ASAN in fuzzcheck, to prevent stack overflow on ARM64. (check-in: 5e96e177a0 ... user: drh tags: trunk) | 
| 14:52 |  | Add the --osmalloc option to the test/speedtest.tcl testing script. Adjust ./configure so that it does not check for malloc_usable_size(). (check-in: 7e9845433f ... user: drh tags: trunk) | 
| 2025-05-22 |  |  | 
| 18:04 |  | Fix a problem with using streaming iterators with sqlite3changegroup_add_change(). (check-in: 3dbde72714 ... user: dan tags: trunk) | 
| 2025-05-19 |  |  | 
| 14:50 |  | Clarify some malloc size computations to simplify the proof that they are safe. Remove some code associated with cygwin that is marked "#if 0". (check-in: ba8184d132 ... user: drh tags: trunk) | 
| 14:04 |  | Make the new sqlite3_setlk_timeout() interface accessible to loadable extensions. (check-in: 8819b7285b ... user: drh tags: trunk) | 
| 12:46 |  | Remove stray tab characters from source files. (check-in: 298ff5a1dc ... user: drh tags: trunk) | 
| 12:34 |  | Fix a harmless comment typo (check-in: c4d25acec3 ... user: drh tags: trunk) | 
| 11:08 |  | Adjust the tool/warnings.sh script so that it works on the latest versions of clang on Mac. (check-in: 05a3dd9a17 ... user: drh tags: trunk) | 
| 2025-05-17 |  |  | 
| 11:06 |  | Latest teaish pieces, most significantly for tcl portability fixes. Move autoconf/teaish/autosetup/... to autosetup/teaish/. to simplify maintenance and deployment via the autoconf bundle. (check-in: 381d3e82e8 ... user: stephan tags: trunk) | 
| 10:35 |  | Minor tcl doc update. (closed check-in: 43259e8c3f ... user: stephan tags: tea-cleanups) | 
| 10:32 |  | Rename feature-tests.tcl to feature.tcl. Haiku tcl portability fixes. (check-in: 15bd9e581a ... user: stephan tags: tea-cleanups) | 
| 07:02 |  | Latest upstream teaish pieces for minor fixes. Restructure this copy of teaish to simplify maintenance and the autoconf bundle build. (check-in: 2b8d9b75ec ... user: stephan tags: tea-cleanups) | 
| 2025-05-16 |  |  | 
| 18:19 |  | Improved version of the previous check-in. (check-in: 036c97e36c ... user: drh tags: trunk) | 
| 17:30 |  | Fix the optimization of check-in [663f5dd32d9db832] that strives to avoid duplicate compuations in the GROUP BY clause so that it works even if the GROUP BY term is a subquery on the RHS of a LEFT JOIN. Problem found by dbsqlfuzz. Test cases in TH3. (check-in: 955a026996 ... user: drh tags: trunk) | 
| 2025-05-15 |  |  | 
| 18:50 |  | Add the --enablefk option to the "changeset apply" command of the changeset program. (check-in: e98d46d436 ... user: drh tags: trunk) | 
| 17:33 |  | Enhance the "changeset" utility program with new command-line options for the "changeset apply" command. (check-in: 428daca4f1 ... user: drh tags: trunk) | 
| 11:20 |  | Rework the showHelp() function in the CLI implementation so that its purpose and operation are well described by the header commit. Omit the use of enums that cause issues for MSVC 2025. (check-in: 336ceeccc6 ... user: drh tags: trunk) | 
| 2025-05-14 |  |  | 
| 16:40 |  | Back out the "low-quality index" query planner hack of check-in [bcac937526d9a6ef]. Subsequent query planner enhancements for dealing with star-queries make that change unnecessary and the change was recently found to cause a performance regression in an unrelated query. Also fix a typo in a debugging message. (check-in: e7dcf25efa ... user: drh tags: trunk) | 
| 2025-05-13 |  |  | 
| 19:06 |  | Fix trunk fork. (check-in: 53644c42c5 ... user: drh tags: trunk) | 
| 18:58 |  | Adjust the strftime() test in test/date4.test to remove flags not supported in musl libc if that environment is detected. (check-in: fc254c1eb7 ... user: stephan tags: trunk) | 
| 16:58 |  | First cut at enhancing the fsdir virtual table so that it works with unicode characters on Windows. (check-in: c9e04dadfd ... user: drh tags: trunk) | 
| 15:09 |  | Omit the unused readdir_r() routine from test_windirent.c (check-in: f9a216e3ad ... user: drh tags: trunk) | 
| 2025-05-12 |  |  | 
| 19:12 |  | Session extension doc typo fix reported in forum post 75e9408acb. (check-in: b96cfff52b ... user: stephan tags: trunk) | 
| 11:48 |  | Fix a coding mistake in vfstrace. (check-in: f0054cc0bc ... user: drh tags: trunk) | 
| 2025-05-11 |  |  | 
| 10:48 |  | Remove a redundant typedef from the sqlite3_rsync.c source file. (check-in: b4c37c6bcc ... user: drh tags: trunk) | 
| 2025-05-10 |  |  | 
| 17:09 |  | Provide the SQLITE_BUG_COMPATIBLE_20250510 compile-time option that restores the JSON5 bug fixed in the previous check-in, in case some applications need it for legacy compatibility. (check-in: 491cf31904 ... user: drh tags: trunk) | 
| 15:53 |  | Add enforcement of the obscure JSON5 syntax rule that the \0 escape sequence must not be followed by a digit. Forum post c061e87faf. (check-in: 83c7477f2b ... user: drh tags: trunk) | 
| 2025-05-08 |  |  | 
| 16:18 |  | Fix PRAGMA trusted_schema=OFF and similar so that it restricts the kinds of functions in CHECK constraints that the documentation says it does. It was letting through some function that it ought not have. This is a defect in [5720924cb07766cd]. See forum thread 2025-05-08T08:50Z. Additional test cases will be added separately. (check-in: 25920beebf ... user: drh tags: trunk) | 
| 13:51 |  | Internal doc typo fix. No code changes. (check-in: ac2aa39f7e ... user: stephan tags: trunk) | 
| 2025-05-07 |  |  | 
| 17:13 |  | Fix a harmless warning about and oversize shift operation on malformed JSONB inputs. OSSFuzz 415850463. (check-in: cf8b55b3b6 ... user: drh tags: trunk) | 
| 2025-05-06 |  |  | 
| 17:56 |  | Add test cases for the NOT NULL/IS NULL optimization in CHECK constraints fix. (check-in: 6eb2939a60 ... user: dan tags: trunk) | 
| 17:53 |  | Fix a bug in the NOT NULL/IS NULL optimization of check-in [cb94350185f555c3] that can cause invalid data to be used for a column if that column has a CHECK constraint that includes the NOT NULL or IS NULL operator. Problem discovered by the Chromium fuzzer. Never seen in the wild, as far as anybody knows. (check-in: 2adaee9aa9 ... user: drh tags: trunk) | 
| 16:28 |  | Fix the sqlite3VdbeTypeofColumn() function so that it works correctly even when SQLITE_DEBUG is defined. (check-in: 1d5021533e ... user: drh tags: trunk) | 
| 15:39 |  | Fix console-I/O on Windows for DEBUG=3 builds. Broken by [925e97e6f4238f02]. (check-in: ccef4f7058 ... user: drh tags: trunk) | 
| 2025-05-03 |  |  | 
| 15:17 |  | Enhance sqlite3_rsync so that if the first attempt to invoke a copy of itself on the remote system using ssh fails, try again after augmenting the PATH. This enables sqlite3_rsync to work without the --exe option when the remote system is a Mac. (check-in: 38d4c94d8c ... user: drh tags: trunk) | 
| 10:55 |  | Fix a harmless redundant variable declaration in sqlite3_rsync. (check-in: f8f15eff6a ... user: drh tags: trunk) | 
| 10:49 |  | Addition summary results output when using -vvv on sqlite3_rsync. (check-in: c702999cfa ... user: drh tags: trunk) | 
| 10:35 |  | Promote the --protocol option to sqlite3_rsync from being an undocumented debug option to being a supported and user-visible option. This is sometimes needed to work around bugs in prior versions running on the remote. (check-in: 4855e04e44 ... user: drh tags: trunk) | 
| 08:17 |  | Enhance sqlite3_rsync (in a backwards-compatible way) so that it has the ability to send hashes for blocks of pages in addition to individual pages. By judicious use of this capability, network bandwidth requirement to sync two similar databases is reduced. (check-in: e5d87aaa8f ... user: drh tags: trunk) | 
| 07:00 |  | Improvements to protocol negotiation. (closed check-in: 4f5a06e420 ... user: drh tags: faster-rsync) | 
| 2025-05-02 |  |  | 
| 23:50 |  | Clean up command-line parsing. Add the undocumented -protocol option for debugging. (check-in: 2a52b174e6 ... user: drh tags: faster-rsync) | 
| 22:25 |  | Now appears to be working. More testing needed. Refinement of the version-2 algorithm needed. (check-in: cb035181d9 ... user: drh tags: faster-rsync) | 
| 18:32 |  | Bug fixes. Added new debugging features to better visualize the protocol. (check-in: c703306686 ... user: drh tags: faster-rsync) | 
| 17:39 |  | This is the start of an experiment in getting sqlite3_rsync to use less bandwidth when the two databases are very similar, by sending hashes over blocks of pages initially, rather than over individual pages, then requesting more detail when hashes do not match. (check-in: 266b4b8f01 ... user: drh tags: faster-rsync) | 
| 11:18 |  | Do not allow sqlite3_rsync to convert the replica from WAL-mode into DELETE-mode, as that can disrupt existing clients on the replica side. DELETE-mode to WAL-mode conversions are allowed, however. See forum thread 6b575b66156. (check-in: 660a035b6c ... user: drh tags: trunk) | 
| 2025-05-01 |  |  | 
| 18:07 |  | Enhance sqlite3_rsync so that, by default, it will sync non-WAL-mode database files. Add a new command-line option --wal-only that restricts the sync to WAL-mode databases only (the former default). Improve command-line option parsing so that only a single "-" is required before each option. (check-in: 4b53603fe4 ... user: drh tags: trunk) | 
| 16:07 |  | Allow sqlite3_rsync to work on non-WAL-mode databases, as long as the --wal-only flag is not used. (closed check-in: e4126dcd1e ... user: drh tags: rsync-non-wal-mode) | 
| 16:02 |  | Mistake: Started from the wrong check-out. (leaf check-in: 9f88f73ce3 ... user: drh tags: mistake) | 
| 2025-04-30 |  |  | 
| 14:37 |  | Fix a harmless problem in the CLI in which SQL errors that occur during the ".schema" command are properly ignored, yes still appear in the ".log" output. Forum post 42fe6520b8 (check-in: 20abf1ec10 ... user: drh tags: trunk) | 
| 12:48 |  | Fix an issue in Bloom filters on RHS subsqueries to IN operators. See forum post 792a09cb3d for a description of the problem. Also improve comments related to [baa83b460c677c21] which was origin of the problem. (check-in: cdef486e21 ... user: drh tags: trunk) | 
| 2025-04-29 |  |  | 
| 17:30 |  | Upstream teaish for a tcl portability fix on Haiku and a much nicer impl of proj-tclConfig-sh-to-autosetup. (check-in: ca0d30a43b ... user: stephan tags: trunk) | 
| 16:30 |  | Remove the run-fuzzcheck makefile targets, since testrunner now accomplishes that for us, and does a better job of it. (check-in: 3ffd867ed3 ... user: drh tags: trunk) | 
| 14:23 |  | Add the "--fuzzdb FILENAME" to testrunner.tcl as an alternative to setting the FUZZDB environment variable (as that can be awkward to do on Windows). Further improvements to the testrunner.tcl documentation. (check-in: 6fb84156a2 ... user: drh tags: trunk) | 
| 11:35 |  | Update testrunner.tcl documentation. Fix typo in testrunner.tcl output. (check-in: ba8800b4ba ... user: drh tags: trunk) | 
| 10:41 |  | Improved --explain display of FUZZDB test plans in testrunner.tcl. (check-in: 42bdd7262e ... user: drh tags: trunk) | 
| 10:29 |  | Do not run FUZZDB on either Windows-Memdebug or Windows-Win32Heap. (check-in: 10d8401d59 ... user: drh tags: trunk) | 
| 01:37 |  | Make testrunner.tcl more restrictive about which configurations run fuzzcheck-asan and fuzzcheck-ubsan. (check-in: 785599394d ... user: drh tags: trunk) | 
| 2025-04-28 |  |  | 
| 19:15 |  | Testrunner.tcl now runs fuzzcheck-asan and fuzzcheck-ubsan tests. (check-in: 3d8f3a2558 ... user: drh tags: trunk) | 
| 17:56 |  | If the FUZZDB environment variable is set when running testrunner.tcl and that env-var points to a fuzzcheck database, then suppliment the fuzzcheck tests with that extra database. (check-in: c08530dd6a ... user: drh tags: trunk) | 
| 16:28 |  | Enhance testrunner.tcl so that it does not build testfixture unnecessarily - if the pattern does not match any TCL test. (check-in: 896d767f1c ... user: drh tags: trunk) | 
| 16:17 |  | Fix testrunner.tcl so that it honors pattern matches in non-testfixture tests, such as fuzzcheck and sessionfuzz. (check-in: 81857474fe ... user: drh tags: trunk) | 
| 15:00 |  | Remove a stray blank line from the test/testrunner.tcl script. (check-in: 7d6005b0d1 ... user: drh tags: trunk) | 
| 13:32 |  | Remove long-obsolete performance testing scripts from the tools/ folder. The test/speedtest.tcl script is what should be used these days. This is just cruft clean-up. (check-in: 956af8c294 ... user: drh tags: trunk) | 
| 09:25 |  | tea: pull in upstream teaish. Consolidate the build definition from the upstream build and this one to ease maintenace. Improve compatibility with historical TEA usage of --with-tcl=... (it insists on a /lib suffix for that path). (check-in: 0ea5993dd8 ... user: stephan tags: trunk) | 
| 2025-04-27 |  |  | 
| 06:01 |  | proj.tcl: correct the auto-reconfigure rules to include any X=Y passed to configure. Improve handling of quoted CFLAGS. (check-in: 7a2f4f319c ... user: stephan tags: trunk) | 
| 04:21 |  | autoconf/tea: the default value of --threadsafe=X is now based on a pkgconfig query using the target tclsh, which is much more reliable than grepping Tcl's linker flags. Doc touchups. (check-in: 4aeec30443 ... user: stephan tags: trunk) | 
| 2025-04-25 |  |  | 
| 12:39 |  | Fix an off-by-one error in an assert(), discovered by oss-fuzz. This is a harmless error in as much as assert()s are disabled in production builds, and because the off-by-one only occurs on nonsensical CREATE INDEX statements. (check-in: 3e627d66eb ... user: drh tags: trunk) | 
| 11:27 |  | Teaish internal API tweaks and ensure that -encoding utf-8 is used when source'ing test scripts. (check-in: f878f578f8 ... user: stephan tags: trunk) | 
| 2025-04-23 |  |  | 
| 14:54 |  | Upstream teaish fixes and improvements. (check-in: 235ea2c0e1 ... user: stephan tags: trunk) | 
| 2025-04-22 |  |  | 
| 18:45 |  | Fix a problem in the fts5matchinfo.test file that prevented it from working on Mac. (check-in: 5a411c609b ... user: drh tags: trunk) | 
| 09:29 |  | Latest upstream jimtcl to fix rare segfault cases. (check-in: b6c5516940 ... user: stephan tags: trunk) | 
| 01:01 |  | Generic cleanups and fixes in the teaish build. (check-in: 734823a5a8 ... user: stephan tags: trunk) | 
| 2025-04-21 |  |  | 
| 23:44 |  | Remove an extra conditional that was inserted earlier today and which is unreachable. (check-in: 5ea56af2d2 ... user: drh tags: trunk) | 
| 20:58 |  | Further improvements to the decision of whether or not a BLOB input is JSONB. (check-in: 6538813cb8 ... user: drh tags: trunk) | 
| 19:53 |  | Do a better job of providing bug compatibility with SQLite 3.44.0. See forum thread 07e206fcd6 for background. (check-in: 614d061b32 ... user: drh tags: trunk) | 
| 2025-04-19 |  |  | 
| 20:08 |  | Update to comments in sqlite3session.h. (check-in: ea9acb5573 ... user: dan tags: trunk) | 
| 2025-04-18 |  |  | 
| 14:47 |  | Add an appropriate _declspec to the declaration of the sqlite3_dbdata_init() function in the recovery extension. (check-in: 00313d1527 ... user: drh tags: trunk) | 
| 2025-04-17 |  |  | 
| 19:41 |  | Upstream teaish, which simplifies how extension metadata are initialized and shrinks proj.tcl by 4k. (check-in: c72c00d34d ... user: stephan tags: trunk) | 
| 19:01 |  | When logging errors using sqlite3_log() and the error text includes the text of an SQL statement, but the SQL statement text at the end so that if the error message buffer overflows it is the SQL statement text that gets truncated, not the statement of the problem. (check-in: de0968226e ... user: drh tags: trunk) | 
| 17:46 |  | Increase the size of the output buffer for sqlite3_log(). (check-in: a64e8491c9 ... user: drh tags: trunk) | 
| 2025-04-16 |  |  | 
| 22:25 |  | Add docs explaining how to test the teaish build. (check-in: b53619ddb7 ... user: stephan tags: trunk) | 
| 21:53 |  | Latest upstream teaish. Most notably it adds the ability for the extension to specify a minimum Tcl version, and this build now enforces a minimum Tcl of 8.6. (check-in: c86315e538 ... user: stephan tags: trunk) | 
| 17:47 |  | Update doc/tcl-extension-testing.md for Unix systems, consolidating the Tcl 8.x and 9.x sections. (check-in: c4c5dcb79c ... user: stephan tags: trunk) | 
| 17:36 |  | Attempt to provide EBCDIC translations to the tables in the JSON implementation. The SQLite developers do not have access to any computers using EBCDIC and so have no way to test this patch, and cannot vouch for its accuracy. (check-in: 84e698f384 ... user: drh tags: trunk) | 
| 15:40 |  | Change the target libdir name in the teaish build to match historical tea builds. (check-in: 8ae437301e ... user: stephan tags: trunk) | 
| 14:21 |  | A portability fix for proj.tcl: some Tcl builds do not like (array set x $someArray). Also better --mandir handling on Haiku (where the former problem was found).7 (check-in: 849534499d ... user: stephan tags: trunk) | 
| 12:48 |  | Latest upstream teaish for fixes and improvements. (check-in: 8f9148b33e ... user: stephan tags: trunk) | 
| 2025-04-15 |  |  | 
| 21:59 |  | Correctly handle the case of a multi-column UNIQUE constraint that contains the ROWID as one of it columns, and then the columns of that UNIQUE are used in a row-value IN operator as a WHERE clause constraint. Reported by forum post b9647a113b. Problem introduced by [723f1be3d4a905a6], part of ticket [da78413751863]. (check-in: d22475b81c ... user: drh tags: trunk) | 
| 19:53 |  | Fix a minor typo in a code comment. (check-in: 158e8c4fd7 ... user: drh tags: trunk) | 
| 15:20 |  | Cleanups and refactoring in proj.tcl and teaish. (check-in: 6b7ca8176e ... user: stephan tags: trunk) | 
| 11:06 |  | Avoid a potential integer overflow in non-default builds of the fts3 matchinfo() function. (check-in: aecc0100ce ... user: dan tags: trunk) | 
| 02:02 |  | Further cleanups and code consolidation between teaish.tcl and sqlite-config.tcl parts. (check-in: 07e9ceee51 ... user: stephan tags: trunk) | 
| 00:47 |  | Slight simplifications in the teaish build. (check-in: 6b2d0e09e4 ... user: stephan tags: trunk) | 
| 2025-04-14 |  |  | 
| 23:52 |  | Update teaish to the latest upstream copy. Move some of its utility code into proj.tcl for reuse in the top-level configure bits. (check-in: 921c5ea769 ... user: stephan tags: trunk) | 
| 19:43 |  | Improved estimate on the number of output rows in a recursive common table expression that uses the UNION operator. Follow-up to [f911f1c4977fbcae] and it's output row estimate changes associated with DISTINCT queries. (check-in: 92513f6bce ... user: drh tags: trunk) | 
| 13:31 |  | JNI: part 3 (of 3) of typos and Java style tweaks suggested in forum post 99ac7961d82f57f3. Tested with jdk v8 and v21. (check-in: 914d4c9d6c ... user: stephan tags: trunk) | 
| 12:09 |  | JNI: part 2 of typos and Java style tweaks suggested in forum post 99ac7961d82f57f3. (check-in: 5e6e9aee5b ... user: stephan tags: trunk) | 
| 11:31 |  | JNI: part 1 of typos and Java style tweaks suggested in forum post 99ac7961d82f57f3. (check-in: ab00af4e48 ... user: stephan tags: trunk) | 
| 10:20 |  | Doc/help text tweaks in autoconf/tea. (check-in: d6889f6b9a ... user: stephan tags: trunk) | 
| 10:18 |  | Minor cleanups and fixes in the autoconf/tea pieces. (check-in: 4d6536bd0a ... user: stephan tags: trunk) | 
| 2025-04-13 |  |  | 
| 16:22 |  | Latest upstream teaish, which restructures the teaish files a bit. (check-in: 4f54f7d07d ... user: stephan tags: trunk) | 
| 2025-04-12 |  |  | 
| 21:47 |  | tea build: use autosetup's file-normalize instead of Tcl's (file normalize) because JimTcl's (file normalize) throws on cygwin for names like '.' and './'. Update a doc URL. (check-in: 0fe77341a0 ... user: stephan tags: trunk) | 
| 21:24 |  | Port autoconf/tea from the autotools to autosetup. (check-in: 691708642b ... user: stephan tags: trunk) | 
| 19:26 |  | Move the post-configure dot-in-file validation from sqlite-config.tcl to proj.tcl for reuse in the tea port and other downstream trees. (check-in: 0bac496d5c ... user: stephan tags: trunk) | 
| 18:37 |  | Doc typo fix. (closed check-in: 10b917040b ... user: stephan tags: teaish) | 
| 10:33 |  | Eliminate a duplicate -lz flag in the teaish build. (check-in: 5efb372c9b ... user: stephan tags: teaish) | 
| 03:20 |  | Add missing teaish.test.tcl. (check-in: 3946a3ad9c ... user: stephan tags: teaish) | 
| 03:08 |  | Add missing exports of various LDFLAGS and CFLAGS for the teaish build. (check-in: 7462b1c66e ... user: stephan tags: teaish) | 
| 02:44 |  | Correct out-of-tree build for autoconf/tea. (check-in: 955b516618 ... user: stephan tags: teaish) | 
| 02:30 |  | Initial port of the TEA build (autoconf/tea) from autotools to autosetup. (check-in: 95227341cf ... user: stephan tags: teaish) | 
| 2025-04-10 |  |  | 
| 20:52 |  | Fix a memory leak in sqlite3session_diff(). (check-in: a6cbc5db1c ... user: dan tags: trunk) | 
| 19:54 |  | Improved detection of run-time errors (ex: OOM errors) in speedtest1. (check-in: 0e5e0e2f55 ... user: drh tags: trunk) | 
| 19:39 |  | Provide new command-line options --hard-heap-limit and --soft-heap-limit for speedtest1. (check-in: 578e9fedea ... user: drh tags: trunk) | 
| 19:03 |  | Be sure that speedtest1 reports any errors that it encounters while running the speed tests. (check-in: 8c8b1a99bc ... user: drh tags: trunk) | 
| 17:25 |  | Fix a comment in sessionD.test. (check-in: 3516354ee5 ... user: dan tags: trunk) | 
| 16:48 |  | Improve the error messages returned by sqlite3session_diff(). (check-in: a3217cdb75 ... user: dan tags: trunk) | 
| 15:01 |  | Fix an obscure problem allowing the propagate-constants optimization to improperly substitute a column of a sub-query with NONE affinity. Forum post 2025-04-08T14:18:45Z. (check-in: 979f384a93 ... user: dan tags: trunk) | 
| 14:53 |  | Fix an obscure problem allowing the propagate-constants optimization to improperly substitute a column of a sub-query with NONE affinity. (closed check-in: d82725dcae ... user: dan tags: forum-0109bca824) | 
| 10:18 |  | Remove unnecessary "www." prefixes on domain names in URLs. (check-in: 20acd630b9 ... user: drh tags: trunk) | 
| 2025-04-09 |  |  | 
| 13:32 |  | Doc updates in proj.tcl. Improve configure check for whether fdatasync or nanosleep require -lrt. Remove references to 'tcl-config' build mode from sqlite-config.tcl, as that build is now implemented without a depency on sqlite-config.tcl because the TEA build's needs are considerably more modest. (check-in: 394bef5441 ... user: stephan tags: trunk) | 
| 2025-04-08 |  |  | 
| 20:00 |  | Remove an incorrect ALWAYS() macro. Forum post 2025-04-08T19:16:42Z. (check-in: 062cca9c63 ... user: drh tags: trunk) | 
| 17:18 |  | Have sqlite3session_diff() automatically attach tables to session objects, as it has always been documented to do. (check-in: 99827144f1 ... user: dan tags: trunk) | 
| 15:58 |  | Have sqlite3session_diff() automatically attach tables to session objects, as it has always been documented to do. (closed check-in: ec1641858e ... user: dan tags: sessions-diff-autoattach) | 
| 2025-04-07 |  |  | 
| 18:29 |  | During the post-configure validation of @VAR@ references, ignore commented-out references in files which seem (based on their name) to be makefiles, as it's sometimes handy to comment-out such vars during development of a configure script and its corresponding makefile(s). (check-in: 25d936b7b2 ... user: stephan tags: trunk) | 
| 02:16 |  | Rename proj-current-proc-name to proj-current-scope and have it distinguish between global scope and an invalid scope level. (check-in: 1c7e5b6cab ... user: stephan tags: trunk) | 
| 02:08 |  | Make it legal to call proj-fatal from the global scope. (check-in: f1cf373b73 ... user: stephan tags: trunk) | 
| 2025-04-06 |  |  | 
| 10:22 |  | Fix a harmless code typo introduced by [b57e3c3db00a6bc6] and reported by forum post 09957d8b2a. (check-in: f3a6cdb89b ... user: drh tags: trunk) | 
| 2025-04-05 |  |  | 
| 09:17 |  | Extend proj-dot-ins-append to provide a way to use it for the special-case emcc.sh.in handling. (check-in: 0e98bbddc7 ... user: stephan tags: trunk) | 
| 08:48 |  | Add a note that SQLite's copy of proj.tcl can be considered the "canonical copy" for practical purposes. We now have at least 3 copies of it floating around in other trees. (check-in: 77d2c8e9ef ... user: stephan tags: trunk) | 
| 07:12 |  | Move the automatic reconfigure tcl code from sqlite-config.tcl to proj.tcl so that the TEA bits can reuse it. (check-in: 52839db2fc ... user: stephan tags: trunk) | 
| 02:59 |  | Refactor proj-make-from-dot-in and friends to be more useful and remove some annoying limitations. (check-in: 501ff84e12 ... user: stephan tags: trunk) | 
| 2025-04-04 |  |  | 
| 23:12 |  | Rename makefile var libtclsqlite3.SO to libtclsqlite3.DLL for consistency. (check-in: 6fb364c853 ... user: stephan tags: trunk) | 
| 22:59 |  | Correct a makefile var name type in the configure script, introduced in [4947c1c782]. (check-in: 84d77c5fe8 ... user: stephan tags: trunk) | 
| 22:14 |  | Add more vars to the tclConfig.sh-to-tcl exporter, for use in the TEA build. (check-in: b0e1e49c7a ... user: stephan tags: trunk) | 
| 20:50 |  | Account for the Mac-specific -install_name linker flag. See forum post 5651662b8875ec0a. (check-in: 4947c1c782 ... user: stephan tags: trunk) | 
| 16:17 |  | Adapt the --all flag to apply to a different set of features, and default to enabled, for the tcl-extension build. (check-in: b229c8f65a ... user: stephan tags: trunk) | 
| 15:41 |  | Improve --all flag handling so that combinations of (--all --disable-FEATURE) and (--disable-all --FEATURE) work more intuitively. Add missing handling of the --fts3 flag (it was accepted but ignored before). Add TCL_LIBS to the set of vars exported via tclConfig.sh for use in the tea build. (check-in: 26ef0ff4bf ... user: stephan tags: trunk) | 
| 13:23 |  | The win32lock and win32longpath tests don't work on Cygwin (check-in: 32ef3f26e7 ... user: jan.nijtmans tags: trunk) | 
| 13:08 |  | Minor configure-internal cleanups to support the autosetup port of the TEA build. (check-in: 244c2b75c1 ... user: stephan tags: trunk) | 
| 03:26 |  | Minor configure script internal doc edits. No functional changes. (check-in: 357563bc7c ... user: stephan tags: trunk) | 
| 03:16 |  | Make the --static-cli-shell flag available in the autoconf build mode. (check-in: b78a474ecb ... user: stephan tags: trunk) | 
| 02:40 |  | On HaikuOS, if --prefix is not explicitly provided then default it to /boot/home/config/non-packaged, based on an off-list discussion with HaikuOS user BrickViking. (check-in: c4a5582911 ... user: stephan tags: trunk) | 
| 2025-04-03 |  |  | 
| 13:24 |  | Add the --static-cli-shell configure flag to the canonical build, which works like --static-tclsqlite3 but applies to the CLI shell (statically linking it, which only works if all requisite libs are available in static form). It is not called --static-shell because that flag has a completely different legacy meaning in the autoconf build and this flight might eventually be added to that build, so would collide there. (check-in: fd2c1638ec ... user: stephan tags: trunk) | 
| 00:46 |  | Add the --static-tclsqlite3 configure flag (canonical build only) to build tclsqlite3 as a static binary. This will only work on systems which have static versions of all requisite libraries. (check-in: 6183632436 ... user: stephan tags: trunk) | 
| 00:40 |  | Minor cleanups to the --static-tclsqlite3 support. (closed check-in: 06f1143870 ... user: stephan tags: static-tclsqlite3) | 
| 2025-04-02 |  |  | 
| 23:34 |  | Add experimental --static-tclsqlite3 configure flag to build tclsqlite3 statically, which only works if static libtcl/etc. are available. Currently intended for docker builds via Alpine Linux. (check-in: bef3c8b1b9 ... user: stephan tags: static-tclsqlite3) | 
| 12:41 |  | Fix harmless compiler warnings in the memstat.c extension. No changes to the core. (check-in: 9e903953ba ... user: drh tags: trunk) | 
| 11:21 |  | Remove some stray debug output from proj.tcl. (check-in: 5fc7150a20 ... user: stephan tags: trunk) | 
| 10:42 |  | Increase the size of a variable in FTS3 to help static analyzers see that there are no integer overflow problems. (check-in: a921a4682f ... user: drh tags: trunk) | 
| 2025-04-01 |  |  | 
| 16:32 |  | The json-blob-overwrite optimization seeks to prevent unnecessary I/O and data movement when making small changes to the middle of a large JSONB. (check-in: 4a3d7b0421 ... user: drh tags: trunk) | 
| 16:26 |  | Fix a typo accidently added to the prior check-in. (closed check-in: fb2f283038 ... user: drh tags: json-opt) | 
| 15:17 |  | This is an experimental optimization that attempts to keep a JSONB value the same size (same number of bytes) after doing a replace of an elements with a slightly smaller element, by denormalizing the size field. This can perhaps avoid unnecessary page updates and memmove() operations when making small changes in the middle of a large JSONB value. (check-in: b5de9584b7 ... user: drh tags: json-opt) | 
| 2025-03-31 |  |  | 
| 23:18 |  | Fix a harmless typo in a code comment. (check-in: dd251377bd ... user: drh tags: trunk) | 
| 13:08 |  | Ensure that the compilation of extensions get the same CFLAGS as the core lib. Move the feature flags enabled by --dev out of TARGET_DEBUG and into OPT_FEATURE_FLAGS (for consistency). Rename the make-internal gcov-related flags to be more descriptive. At the end of the configure script, if SQLITE_DEBUG is active then emit a note reminding the builder that performance will suffer. (check-in: e64c6a3856 ... user: stephan tags: trunk) | 
| 10:54 |  | Ensure that extension sources also inherit the debugging-related flags from --debug or --dev. (check-in: 3e96b772a4 ... user: stephan tags: trunk) | 
| 10:29 |  | Correct the ordering of the configure script's handling of the --dev and --debug flags so that --dev's forcing of --debug get picked up early enough. (check-in: e48189ed33 ... user: stephan tags: trunk) | 
| 09:53 |  | The --dev option on ./configure implies --debug and adds -DSQLITE_DEBUG. (check-in: 67dbd9ff0f ... user: drh tags: trunk) | 
| 2025-03-29 |  |  | 
| 11:50 |  | Minor shell-internal doc addition. No functional changes. (check-in: 01b99c8a28 ... user: stephan tags: trunk) | 
| 2025-03-28 |  |  | 
| 16:31 |  | Correct two tests in shell1.test which were broken by the addition of new output in [0f11087b8475]. (check-in: 909dd0a02a ... user: stephan tags: trunk) | 
| 15:36 |  | CLI shell: make (.output off) an alias for (.output /dev/null) or (.output nul), depending on the platform. Discussed in forum post 633979ce307f1cc6. (check-in: 0f11087b84 ... user: stephan tags: trunk) | 
| 14:32 |  | Configure/make doc tweaks. No functional changes. (check-in: dbcc03bd7a ... user: stephan tags: trunk) | 
| 2025-03-27 |  |  | 
| 23:29 |  | Merge cygwin and tclsqlite fixes into trunk. (check-in: 121f4d97f9 ... user: drh tags: trunk) | 
| 18:42 |  | Add an explicit db close to fix a file-is-opened failure in session1.test in cygwin. (check-in: c6e4e1cf2b ... user: stephan tags: cygwin-fixes) | 
| 18:22 |  | Rearrange #includes in FTS3 to avoid a harmless compiler warning. (check-in: c5206cee83 ... user: drh tags: trunk) | 
| 17:30 |  | Fix for forum-post b5fde3596c. Also fix encoding issue for non-ASCII characters. Also includes a fix from Stephan Beal, about a missing <stdint.h> include. (closed check-in: e60198001e ... user: jan.nijtmans tags: tclsqlite-db-copy-fix) | 
| 17:25 |  | shell1.test: == => eq (check-in: 99fdd19193 ... user: jan.nijtmans tags: cygwin-fixes) | 
| 14:37 |  | More ==/!= => eq/ne fixes (check-in: 2aa6c4fa6c ... user: jan.nijtmans tags: cygwin-fixes) | 
| 14:32 |  | In Tcl, always use eq/ne for comparing strings, not ==/!= (check-in: f96a5346e3 ... user: jan.nijtmans tags: cygwin-fixes) | 
| 14:13 |  | Merge trunk. Some additional test-fixes for Cygwin (check-in: b36adadfa2 ... user: jan.nijtmans tags: cygwin-fixes) | 
| 10:59 |  | Fix an off-by-one bug in the ".dbtotxt" command from the shell that results in incorrect output. (check-in: 2345094e87 ... user: drh tags: trunk) | 
| 2025-03-26 |  |  | 
| 22:23 |  | Reduce the delay caused by anti-virus retries in Windows when attempting to open a database file with insufficient permission. (check-in: d25c8a6222 ... user: drh tags: trunk) | 
| 17:05 |  | On winOpen(), redo the read-only test on every retry attempt. Just don't retry the winAccess() call. (closed check-in: ccdd67ad9a ... user: drh tags: winopen-retry) | 
| 15:51 |  | Further attempted improvements at fast-fail for an inaccessible database file. (check-in: cdeb21a312 ... user: drh tags: winopen-retry) | 
| 14:45 |  | Attempt to reduce the amount of retry time when trying to open an inaccessible database file on Windows. See forum thread e7991420f5 (check-in: 986e45912c ... user: drh tags: winopen-retry) | 
| 00:02 |  | Merge trunk into cygwin-fixes branch. Add .fossil-settings/binary-glob to squelch warnings about *.db files on Cygwin. (check-in: a8328b921c ... user: stephan tags: cygwin-fixes) | 
| 2025-03-25 |  |  | 
| 01:32 |  | Configure script internal cleanups. Factor out the superfluous proj-lshift_ and use lassign instead. Add -ro flag to proj-file-write. (check-in: 1f98fc07fd ... user: stephan tags: trunk) | 
| 2025-03-24 |  |  | 
| 23:08 |  | Clarify and reduce redundancy in an assert() in walChecksumBytes(). (check-in: 3c53abf5e6 ... user: drh tags: trunk) | 
| 19:58 |  | Test that the sqlite3changeset_apply() function is properly appling indirect changes. (check-in: 1dadea3a79 ... user: dan tags: trunk) | 
| 11:13 |  | Add test cases to session1.test. (check-in: dda9d7cf31 ... user: dan tags: trunk) | 
| 2025-03-23 |  |  | 
| 23:04 |  | Correct a --force flag which should be -force in a TCL file delete call. Some TCL versions (like the one on my system) accepts either but others don't. Problem reported in forum post c02224d862. (check-in: 172f4e4772 ... user: stephan tags: trunk) | 
| 22:49 |  | Remove tool/tclConfigShToAutoDef.sh, as that functionality was moved into autosetup/proj.tcl so that it can be reused in, e.g., autoconf/tea. (check-in: c9c5a15d4d ... user: stephan tags: trunk) | 
| 21:38 |  | Internal configure refactoring to support an ongoing conversion of ./autoconf/tea to autosetup. (Edit: this was merged from [63228f7746] but either an obscure fossil bug or an as-yet-unexplained PEBKAC lost that linkage for the second time - the same thing happened with [301ceba6432a882f].) (check-in: e1d483e342 ... user: stephan tags: trunk) | 
| 2025-03-22 |  |  | 
| 22:55 |  | Fix the generate_series() enhancement from check-in [d50b784807333c54] so that it works even if the number that "value" is being compared against is a non-integer floating point number. Bug reported by forum post 0d5d63257. (check-in: c113e31b81 ... user: drh tags: trunk) | 
| 19:07 |  | Replace use of autosetup/lib/*.auto for sqlite-custom-... features to avoid the possibility of multiple files being loaded (in an unpredictable order) for that purpose. Instead look for autosetup/sqlite-custom.tcl and source it if it exists. The intent is that sqlite-custom.tcl only ever be added in vendor-specific branches and never in the trunk. (check-in: f3c0071284 ... user: stephan tags: trunk) | 
| 18:41 |  | In the post-configure validation, ensure than no more than one autosetup/lib/*.auto file is found, for reasons explained in the validation's error message. (check-in: 50bbd69a68 ... user: stephan tags: trunk) | 
| 17:56 |  | Slight corrections to the new docs in autosetup/README.md. (check-in: 55029e41e0 ... user: stephan tags: trunk) | 
| 17:28 |  | Document the new configuration customization approach and extend it to enable sqlite-custom-flags to return an empty string to denote that it does not require any new flags (it may still customize flag default values in such cases). (check-in: b0c7800b01 ... user: stephan tags: trunk) | 
| 17:00 |  | Teach mkautoconfamal.sh to avoid copying autosetup/local.tcl and autosetup/*.auto except for autosetup/lib/CURRENT-BRANCH-NAME.auto (if any), the intent being to use lib/CURRENT-BRANCH-NAME.auto for branch-specific configuration customizations. (check-in: dfe24f13c4 ... user: stephan tags: trunk) | 
| 16:14 |  | Add a mechanism to the configure script to allow certain client-specific builds to extend or override the configure options without having to edit sqlite-config.tcl, the goal being to reduce merge conflicts in those builds when updating sqlite-config.tcl from the canonical copy. (check-in: bafab4ee55 ... user: stephan tags: trunk) | 
| 12:43 |  | Configure script internal cleanups and re-orgs. No functional changes. (check-in: f619e40fb0 ... user: stephan tags: trunk) | 
| 12:15 |  | In the autoconf bundle, do not strip binaries during installation, for parity with the canonical build and the legacy build. Discussed in forum post 9a67df63eda9925c. A potential TODO here is to add a configure flag which either enables or disables stripping. (check-in: 6d2e57bd34 ... user: stephan tags: trunk) | 
| 2025-03-21 |  |  | 
| 22:14 |  | Slightly simplify proj-current-proc-name. (check-in: 6a0ca9f236 ... user: stephan tags: trunk) | 
| 21:13 |  | Fix a multi-arg expr call in proj-current-proc-name (must be single-arg for portability). (check-in: 914768f3f5 ... user: stephan tags: trunk) | 
| 18:15 |  | Teach the CLI that VT100-escape codes that do things like change font colors have zero-width for the purpose of laying out the columns of a table. (check-in: 2d0a8a6c38 ... user: drh tags: trunk) | 
| 16:49 |  | Flesh out the new proc-debug and its infrastructure a bit. (check-in: ba7f1ff0d7 ... user: stephan tags: trunk) | 
| 16:06 |  | Proxy configure's msg-debug with proc-debug, which works the same except that it prepends the name of the calling proc to the debug message. No functional changes. (check-in: f0298c773d ... user: stephan tags: trunk) | 
| 2025-03-20 |  |  | 
| 13:41 |  | Teach the configure script to be able find a default installation of libreadline on Haiku OS. (check-in: 260e988411 ... user: stephan tags: trunk) | 
| 11:47 |  | Fix a problem in the sqlite_dbpage() table-valued function when it is trying to truncate a file in locking-mode=EXCLUSIVE and the file was obtained via sqlite3_deserialize(). Problem found by dbsqlfuzz. (check-in: 346cf9794c ... user: drh tags: trunk) | 
| 2025-03-19 |  |  | 
| 15:21 |  | Correct part of [505d9e49f7] and [7126a51ed8] to get sessioninvert tests working on cygwin. (check-in: 83c34decbe ... user: stephan tags: cygwin-fixes) | 
| 14:57 |  | Correct part of [7126a51e] which changed the implicit return value of a test function. (check-in: 505d9e49f7 ... user: stephan tags: cygwin-fixes) | 
| 14:52 |  | Add some explicit db close calls to work around a process-reaping timing problem on cygwin builds. (check-in: 7126a51ed8 ... user: stephan tags: cygwin-fixes) | 
| 13:49 |  | Test script patches from Jan Nijtmans: always use -DUSE_TCL_STUBS in buildtclext.tcl and improvements in how TRG is populated in testrunner.tcl. (check-in: aa192203aa ... user: stephan tags: cygwin-fixes) | 
| 13:13 |  | Merge trunk into the cygwin-fixes branch for the shell test improvements on Windows. An MSC build's make test now has zero failures. (check-in: 2de4efe10c ... user: stephan tags: cygwin-fixes) | 
| 11:53 |  | Avoid running test cases involving ANSI control characters or Unicode on Windows in a slave interpreter, as that combination does not work. (check-in: c7fd71c77f ... user: stephan tags: trunk) | 
| 10:14 |  | Merge trunk into the cygwin-fixes branch. (check-in: 34eadd374b ... user: stephan tags: cygwin-fixes) | 
| 2025-03-18 |  |  | 
| 20:15 |  | Change the generate_series() table-valued function so that its rowid is just an alias for its value. This allows it to be used as the RHS operand of a RIGHT JOIN. This fixes the issue raised by forum post 1e17219c88. (check-in: 77db4d85e7 ... user: drh tags: trunk) | 
| 19:21 |  | Fix a problem that could occur when the RHS of an IN operator was a compound SELECT featuring an ORDER BY on a subquery that was flattened into one of the component SELECTs introduced by [baa83b460c677c21]. Problem reported in forum post 1e17219c88. (check-in: 7101ccd533 ... user: dan tags: trunk) | 
| 13:52 |  | Internal doc touchups in ext/wasm/mkwasmbuilds.c. No functional changes. (check-in: 47d34260e7 ... user: stephan tags: trunk) | 
| 12:31 |  | Very slight simplification of the run-fuzzcheck rules. (check-in: c858a39fad ... user: stephan tags: trunk) | 
| 11:36 |  | General updates to autosetup/README.md. (check-in: be8ad5cf57 ... user: stephan tags: trunk) | 
| 10:28 |  | Update the docs in tool/mkccode.tcl to reflect that it's more generic than it was when the docs were written. Change the shebang line to use /bin/env tclsh instead of a hard-coded tclsh path. (check-in: 9300f7f42d ... user: stephan tags: trunk) | 
| 2025-03-17 |  |  | 
| 15:13 |  | Prevent integer overflow when parsing NEAR queries in FTS5. (check-in: 1a5283d7da ... user: drh tags: trunk) | 
| 14:59 |  | Add support for the --with-wasi-sdk configure flag to the autoconf build. (check-in: 44880fa3f0 ... user: stephan tags: trunk) | 
| 2025-03-16 |  |  | 
| 14:05 |  | Fix a long-standing filename digest computation bug in the OPFS SAHPool VFS which caused all VFS-stored filenames to have a digest value of 0. See /forumpost/042d53c928382021 for full details. (check-in: 493cbe7450 ... user: stephan tags: trunk) | 
| 13:09 |  | Add --asan-fsanitize=... configure flag to the canonical build to optionally set -fsantize flags for the fuzzcheck-asan tool. Teach proj-check-fsanitiz to fail for flags which the compiler emits any warning for, for reasons described in its comments. (check-in: 013730e9b9 ... user: stephan tags: trunk) | 
| 12:27 |  | Configure-internal doc cleanups. No functional changes. (check-in: be3a2e6311 ... user: stephan tags: trunk) | 
| 11:24 |  | Consolidate some much-duplicated run-fuzzcheck recipe code in main.mk. (check-in: c0d9b9fad3 ... user: stephan tags: trunk) | 
| 00:13 |  | Rework the run-fuzzcheck makefile target so that it better exploit parallelism. Test case " `make -j16 run-fuzzcheck FUZZDB=20250222.db` " went from 596 seconds down to 107 seconds.  (check-in: 18bda13e19 ...  user: drh tags: trunk) | 
| 2025-03-15 |  |  | 
| 23:42 |  | Enhance the fuzzcheck testing tool with new command-line options: --brief, and --slice M N. (check-in: e64132723d ... user: drh tags: trunk) | 
| 19:55 |  | Make use of the C99 flexible array feature, when available, so that the -fsanitize=bounds-strict option can be used, when available. Forum thread 311dbf9a1c. (check-in: d4307a0d43 ... user: drh tags: trunk) | 
| 19:00 |  | Work around compilers that do not understand flexible arrays, in the recovery extension and in the fuzzcheck test module. (closed check-in: f101c46cf8 ... user: drh tags: flex-array) | 
| 18:26 |  | Fix alignment problems on Linux with -m32 and on Mac PPC. (check-in: 8a91aeca60 ... user: drh tags: flex-array) | 
| 16:58 |  | Speed up parsing of very long fts3 query expressions. (check-in: 2dd5b6895a ... user: dan tags: trunk) | 
| 15:19 |  | Configure-internal build cleanups (no functional changes). Add EXTRA_SRC to the deps of sqlite3.c. (check-in: 8afb8bbce8 ... user: stephan tags: trunk) | 
| 13:50 |  | -fsanitize is a CFLAG, not LDFLAG, so rename some vars accordingly and simplify the feature check to not run the linker. (check-in: 44f2c64ec1 ... user: stephan tags: flex-array) | 
| 13:36 |  | For fuzzcheck-asan, dynamically determine the list of -fsanitize flags to use based on configure-time feature tests. (check-in: b70f9cc815 ... user: stephan tags: flex-array) | 
| 13:11 |  | Omit the -fsanitize=bounds-strict for now, as that is still not widely implemented. In particular, it does not work on Macs. (check-in: 3e1c2ac781 ... user: drh tags: flex-array) | 
| 13:04 |  | Use flexible arrays in the recovery extension and in the fuzzcheck test program. Adjust the unix makefile to use -fsanitize=bounds-strict when building fuzzcheck-asan. (check-in: 6ea6a6b211 ... user: drh tags: flex-array) | 
| 12:22 |  | Use flexible arrays whereever appropriate in FTS5. (check-in: 16dfc415b6 ... user: drh tags: flex-array) | 
| 00:11 |  | Convert the Fts5Sorter.aIdx field to a flexible array. (check-in: 28ac776a23 ... user: drh tags: flex-array) | 
| 2025-03-14 |  |  | 
| 23:57 |  | Turn Fts5Colset.aiCol into a flexible array. (check-in: 0c4d9c7474 ... user: drh tags: flex-array) | 
| 23:20 |  | In FTS3, rename the MatchinfoBuffer.aMatchinfo field to aMI, to avoid confusing it with MatchInfo.aMatchinfo. Make aMI a flexiable array. (check-in: bb00b97398 ... user: drh tags: flex-array) | 
| 21:15 |  | Fix one of two flexible arrays in FTS3. (check-in: ddfa87c179 ... user: drh tags: flex-array) | 
| 20:19 |  | Use flexible arrays for RTREE. (check-in: 2b41776179 ... user: drh tags: flex-array) | 
| 19:07 |  | KeyInfo is now an indeterminate size, so we cannot declare a variable of that type, only a pointer to an instance of that type. (check-in: 37b687dc2d ... user: drh tags: flex-array) | 
| 18:10 |  | Make use of the flexible-array feature of C99, when available, to try to pacify -fsanitize=strict-bounds. This check-in fixes the core. There is more yet to do in FTS3, RTREE, and in FTS5. (check-in: 6fd6b32d06 ... user: drh tags: flex-array) | 
| 12:37 |  | Fix an internal doc typo reported in forum post e25e581f917. (check-in: fa6f6ccdff ... user: stephan tags: trunk) | 
| 11:14 |  | Minor doc corrections for the sahpool-digest fix and merge in current trunk. (closed check-in: 500f2e6ec7 ... user: stephan tags: sahpool-digest) | 
| 09:34 |  | Cherrypick the [2b582c0097e33] doc addition, which was initially committed to the wrong branch. (check-in: f786de8d18 ... user: stephan tags: trunk) | 
| 2025-03-13 |  |  | 
| 18:51 |  | Fix the generate_series extension for the case where the termination value is not an even multiple of the step from the start value and there is also a value=NNN constraint in the WHERE clause. Forum post bf2dc8e9 (check-in: 75e72e3b0d ... user: drh tags: trunk) | 
| 2025-03-12 |  |  | 
| 15:17 |  | The --echo flag on the CLI also echos dot-commands provided on the command-line. (check-in: 6ec0c03b95 ... user: drh tags: trunk) | 
| 11:41 |  | Document that sqlite_update_hook() can unset the current hook by passing a NULL callback, to address forum post 652aef4747. (check-in: 2b582c0097 ... user: stephan tags: cygwin-fixes) | 
| 2025-03-11 |  |  | 
| 15:46 |  | Avoid running test cases involving ANSI control characters or Unicode on Windows in a slave interpreter, as that combination does not work. (closed check-in: f6745a7355 ... user: drh tags: windows-ansi) | 
| 12:19 |  | The substitute "puts" command used by the Windows implementation of sqlite3_analyzer must invoke fflush() after each line of output. Otherwise the output can be truncated when redirected into a file. (check-in: ba058ce90a ... user: drh tags: trunk) | 
| 11:24 |  | Teach testrunner.tcl to distinguish Cygwin from Windows. This gets it running but then all downstream tests fail, at least in part because of the build target name discrepancies between the platform which requires .exe and those which do not. (check-in: 2861788e47 ... user: stephan tags: cygwin-fixes) | 
| 2025-03-10 |  |  | 
| 22:31 |  | Ensure that the TEMP database has been initialized at the beginning of a call to sqlite3_open_blob() for the TEMP database. Fix for the issue reported by forum post 0a556d619b. (check-in: 2cfccdbe08 ... user: drh tags: trunk) | 
| 17:35 |  | Merge trunk into the cygwin-fixes branch. (check-in: f3d14433f2 ... user: stephan tags: cygwin-fixes) | 
| 17:28 |  | Add an explicit db close to test/walsetlk.test to work around an unjustified test failure on Windows when the walsetlk tests are run in the same invocation of testfixture.exe in Windows. (check-in: f418de1093 ... user: stephan tags: trunk) | 
| 15:35 |  | Patch an unnecessary size_t-to-int conversion which warns on some compilers. (check-in: 8681dfdb59 ... user: stephan tags: cygwin-fixes) | 
| 15:27 |  | Ensure that the 'clean' target for Makefile.msc cleans up the testdir dir, analog to how the canonical makefile works, as stale files there have been leading to test failures. (check-in: a591c01855 ... user: stephan tags: cygwin-fixes) | 
| 15:15 |  | Reimplement pathname conversion on Cygwin. MSC test passes/failures are on par with trunk and 'make test' running on cygwin is down to a single failure. (check-in: 57a4de0914 ... user: stephan tags: cygwin-fixes) | 
| 14:10 |  | Tweak [6f24da264cf8d] so that it has no side-effects on autoconf/Makefile.msc. (check-in: d3d322f122 ... user: stephan tags: cygwin-fixes) | 
| 10:32 |  | Disable the [count-of-view optimization] if any subquery is DISTINCT, as the optimization does not work in that case. Bug reported by forum post a860f5fb2e. (check-in: d7013b6393 ... user: drh tags: trunk) | 
| 09:54 |  | Add tf.bat target to Makefile.msc to facilitate running of individual test scripts via testfixture.exe. (check-in: 6f24da264c ... user: stephan tags: cygwin-fixes) | 
| 2025-03-08 |  |  | 
| 06:53 |  | Test app/script patches from Jan Nijtmans for cygwin. Add/replace many more sentinel arguments for TCL variadic functions which specifically need a NULL trailing argument. With this, 'make test' on cygwin runs to completion for me, with 134 of 329227 tests failing. MSC build fails the same 13 tests which fail for me in trunk. (check-in: 84d8fcfca1 ... user: stephan tags: cygwin-fixes) | 
| 2025-03-07 |  |  | 
| 06:54 |  | os_win.c patches for Cygwin (described in the cygwin-fixes branch wiki page). Makefile.msc's test target on trunk fails the same 13 tests as this branch. (check-in: 655c60d192 ... user: stephan tags: cygwin-fixes) | 
| 2025-03-06 |  |  | 
| 14:40 |  | Merge trunk into the cygwin-fixes branch (which was inadvertently branched too early). (check-in: a7863b2e2f ... user: stephan tags: cygwin-fixes) | 
| 13:49 |  | Cygwin-centric test script tweaks from Jan Nijtmans. (check-in: 9868615b7d ... user: stephan tags: cygwin-fixes) | 
| 13:38 |  | Cygwin-centric fixes from Jan Nijtmans. (check-in: 205979813c ... user: stephan tags: cygwin-fixes) | 
| 09:29 |  | Back out the most significant part of [5c28a17253e2f], as Cygwin is a hybrid. With SQLITE_OS_UNIX it will use POSIX locking, which will misinteract with apps using Windows-style locking. (check-in: 44adf8f387 ... user: stephan tags: trunk) | 
| 09:08 |  | Various typo fixes reported by Daniel Dumitriu. No functional changes. (check-in: 37e6ec7774 ... user: stephan tags: trunk) | 
| 07:48 |  | Additional 'array index is signed char' warning cleanups for the shell and its embedded extensions, analog to [44bd44532d]. (check-in: f31042595b ... user: stephan tags: trunk) | 
| 07:09 |  | Fix a tcl typo in the previous checkin which triggers an error on one machine but not another. (check-in: 646c2821ad ... user: stephan tags: trunk) | 
| 06:25 |  | Have the configure script report cygwin as SQLITE_OS_UNIX instead of SQLITE_OS_WIN, per off-list discussion with Jan Nijtmans. (check-in: 5c28a17253 ... user: stephan tags: trunk) | 
| 2025-03-05 |  |  | 
| 19:39 |  | Have the recover module add "PRAGMA foreign_keys = off" to SQL output. Have the shell tool add ".dbconfig defensive off". (check-in: dcfe3d3292 ... user: dan tags: trunk) | 
| 18:18 |  | Always ignore comments in the schema of a database, even if SQLITE_DBCONFIG_ENABLE_COMMENTS is turned off. (check-in: 373ae3f4de ... user: drh tags: trunk) | 
| 17:26 |  | Update the recovery extension so that it works with encrypted databases. (check-in: b0b66f2115 ... user: dan tags: trunk) | 
| 17:12 |  | Use AtomicRead() and AtomicWrite() to access the pcache1_g.bUnderPressure global, to forestall unnecessary angst from thread analyzers. (check-in: 41ec85637a ... user: drh tags: trunk) | 
| 16:35 |  | Improvement output for ".schema --indent" in the CLI when the schema contains partial indexes with long and complicated WHERE clauses. (check-in: defd7187ff ... user: drh tags: trunk) | 
| 16:30 |  | Improvement output for ".schema --indent" in the CLI when the schema contains partial indexes with long and complicated WHERE clauses. (leaf check-in: 94c6e8d971 ... user: drh tags: wrong-branch) | 
| 2025-03-04 |  |  | 
| 21:37 |  | Minor configure script cleanups which started out as cygwin-specific fixes but ended up just being minor platform-agnostic cleanups. (check-in: 2cda90410a ... user: stephan tags: trunk) | 
| 21:31 |  | Roll back part of [6d87a8efe]: the check for tclsh90.exe (as opposed to tclsh9.0.exe) is incorrect on these platforms (it's an msvc build). Also remove an unused function added in that checkin. (closed check-in: dc84976a7c ... user: stephan tags: cygwin-fixes) | 
| 21:25 |  | tclsqlite3 patch from Christian Werner: replace FILE handles with TCL channels for the db copy command. (check-in: ea1f7f8de4 ... user: stephan tags: trunk) | 
| 19:38 |  | Teach autosetup how to find tclsh v9.0 on cygwin. (check-in: 6d87a8efe5 ... user: stephan tags: cygwin-fixes) | 
| 07:29 |  | Makefile doc updates. Remove a couple extraneous targets. No functional changes. (check-in: 5a3e22999f ... user: stephan tags: trunk) | 
| 06:29 |  | Minor doc typo fixes via forum post 65bd941da8. (check-in: 6f6a03e93c ... user: stephan tags: trunk) | 
| 05:16 |  | Document the EXTRA_SRC makefile var and add --amalgamation-extra-src=list to the canonical-build configure script as the formal way to pass that at configure-time. (check-in: 44de0ec29a ... user: stephan tags: trunk) | 
| 2025-03-03 |  |  | 
| 14:15 |  | Internal configure script cleanups. Resolve an as-yet-hypothetical corner case involving the --dev flag mixed with custom CFLAGS containing SQLITE_ENABLE... or SQLITE_OMIT... flags. Fix an unrelated API doc typo reported in forum post 606ea661df. (check-in: 0554c00f32 ... user: stephan tags: trunk) | 
| 11:48 |  | Minor configure script doc tweaks. This is also a note that the basic elements of the build run as-is on Haiku OS Beta 5 but the tcl bits do not (for lack of tclConfig.sh). (check-in: acf9babf0e ... user: stephan tags: trunk) | 
| 2025-03-02 |  |  | 
| 20:29 |  | Ensure that detection of control characters by comparison against 0x1f uses unsigned characters. Forum post 4c344ca61f. (check-in: b7c5ce8421 ... user: drh tags: trunk) | 
| 2025-03-01 |  |  | 
| 23:44 |  | Use SQLITE_EXTRA_INIT_MUTEXED instead of SQLITE_EXTRA_INIT for the SQLITE_WASM_EXTRA_INIT feature, as suggested in forum post 14183b98fc0b1dea. This doesn't make a functional difference now - this is in the name of future-proofing against eventual threading support in wasm. (check-in: 46479c2e30 ... user: stephan tags: trunk) | 
| 11:47 |  | Add an assert() in vdbesort.c to help both humans and static analyzer AIs understand why a particular array cannot overflow. (check-in: a14d4afd50 ... user: drh tags: trunk) | 
| 2025-02-28 |  |  | 
| 15:54 |  | In the CLI, ensure that all calls to ctype.h macros are cast to unsigned char. Apparently cygwin needs this. (check-in: 44bd44532d ... user: drh tags: trunk) | 
| 04:58 |  | Latest upstream autosetup. Fixes two minor corner-case issues which we haven't yet seen in practice. (check-in: 5c2438a719 ... user: stephan tags: trunk) | 
| 2025-02-27 |  |  | 
| 21:17 |  | Approximately 100 typo corrections spanning the whole tree, submitted via forum post 0db9827f0464bc33 and individually audited and verified. Affects only code comments, innocuous test strings, error message text in tool (not library) code, and configure-level help text. (check-in: f50c21484d ... user: stephan tags: trunk) | 
| 19:40 |  | Reimplement testrunner_data.tcl::trd_get_bin_name() to assume a .exe extension on all platforms not matching "unix", to avoid having to do file-exists checks. (check-in: ce5519102a ... user: stephan tags: trunk) | 
| 16:07 |  | Strive to make sorts stable in the mkpragmatab.tcl build script, so that we get consistent amalgamations regardless of platform. Forum thread c9914addebf3da51. (check-in: 3f57584710 ... user: drh tags: trunk) | 
| 15:55 |  | Merge back a sequence of three changes that were branched due to a build break. (check-in: 6780971597 ... user: drh tags: trunk) | 
| 15:52 |  | Fix build break in [e2bd23f251359e7a]. (closed check-in: 6ed8421103 ... user: drh tags: build-break) | 
| 11:03 |  | Add support for SQLITE_EXTRA_INIT_MUTEXED - like SQLITE_EXTRA_INIT, but is called from within sqlite3_initialize() when the SQLITE_MUTEX_STATIC_MAIN mutex is still held. (check-in: 3cf88d429f ... user: dan tags: build-break) | 
| 03:23 |  | Internal doc typo fixes reported in forum post 01d15f21b6. (check-in: d6514c20ff ... user: stephan tags: build-break) | 
| 2025-02-26 |  |  | 
| 19:22 |  | *(Edit: Moved to a branch because this breaks "./configure && make devtest")*
 Teach testrunner how to run local binaries which have a .exe extension. Also teach it to recognized mingw environments (but actually building the test programs there currently doesn't work). (check-in: e2bd23f251 ... user: stephan tags: build-break) | 
| 18:31 |  | Teach testrunner.tcl to identify an msys environment. (check-in: e9f777ceda ... user: stephan tags: trunk) | 
| 17:11 |  | Undo part of [69e83ab859c], as that order of operations breaks the --prefix=... configure flag. Found a workaround for the library version and OS/environment info being emitted when --help. (check-in: cd2a84a72c ... user: stephan tags: trunk) | 
| 16:15 |  | configure: extend the readline detection a bit so that it can find readline on termux environments without having to be told where it is. Fix a typo in a libedit-related message. (check-in: 52f528c4be ... user: stephan tags: trunk) | 
| 09:14 |  | Shift some bootstrapping configure bits to avoid emitting extra output if --help is specified. (check-in: 69e83ab859 ... user: stephan tags: trunk) | 
| 03:32 |  | Merge trunk into the sahpool-digest branch. (check-in: fc1eeb7d1f ... user: stephan tags: sahpool-digest) | 
| 03:20 |  | Fix a bug in mkwasmbuilds.c, introduced in [e4539ebebd89], which caused sqlite3.wasm to get deleted directly after it was built. (check-in: 1a72d1d13e ... user: stephan tags: trunk) | 
| 03:03 |  | More work on the sahpool digest fix. New/fixed versions can read legacy (no digest) files but the reverse is only possible in limited circumstances (when files originated from a legacy version). The burning question is whether the real fix would be to remove this digest check altogether, as it only applies in a very limited context, and the fact that it was broken for some 18 months unnoticed suggests that its value might not be worth the CPU cycles. (check-in: 0df62b776c ... user: stephan tags: sahpool-digest) | 
| 2025-02-25 |  |  | 
| 21:16 |  | Disable some misuse tests that sometimes cause segfaults during "make releasetest". (check-in: 5856251a7e ... user: drh tags: trunk) | 
| 20:55 |  | Approximately 50 typo fixes, spanning the whole tree, contributed via forum post 006c8fa165083ac3 and individually checked for correctness. Affects only code comments, docs, and a single line of debug output in a test app. (check-in: af928818a0 ... user: stephan tags: trunk) | 
| 20:36 |  | Fix a test case that was broken when the default --escape type was changed by the previous check-in. (check-in: 5f2531e068 ... user: drh tags: trunk) | 
| 20:10 |  | Change the default control-character escape algorithm from "symbol" to "ascii", since the "ascii" algorithm works even on terminals that do not support unicode. (check-in: d35320495e ... user: drh tags: trunk) | 
| 20:02 |  | Fix harmless "unused parameter" compiler warnings. (check-in: 7a68a3ead8 ... user: drh tags: trunk) | 
| 18:10 |  | Improved help messages for the --escape option in the CLI. (check-in: e6784af6d5 ... user: drh tags: trunk) | 
| 16:39 |  | Configure script cosmetics: emit the host/build system info before the directory info. (Edit: this mysteriously broke the --prefix=... flag again) (Edit - no, it didn't - it was a test methodology failure.) (check-in: 6f0b6d95db ... user: stephan tags: trunk) | 
| 16:11 |  | Restructure [826bad10e9ccd6f71e] as that structure mysteriously breaks the --prefix=... configure flag. (check-in: d284230f68 ... user: stephan tags: trunk) | 
| 15:57 |  | Enhancements to help avoid problems in the CLI when trying display content that contains ANSI escape codes: (1) Add the --escape MODE option to the CLI where MODE is one of "symbol", "ascii", "off" where the default is "symbol". (2) Add the unistr() SQL function. (3) Add the unistr_quote() SQL function. (4) Add the %#Q and %#q conversions in the built-in printf. (check-in: e3e509ae14 ... user: drh tags: trunk) | 
| 15:54 |  | Diverse configure tweaks to better support package maintainers on the unix-on-windows environments like msys2, cygwin, and mingw, based largely on feedback in forum posts e6cf2bbb70da2922 and 828fdfe9041fd725. (check-in: 85b56fb0cc ... user: stephan tags: trunk) | 
| 15:27 |  | Reinstate the assert() removed by [0f6223b8]. Avoid holding a wal-mode write lock after the transaction has been rolled back if an IO error occurs while restarting the wal file. (check-in: 277e150d6a ... user: dan tags: trunk) | 
| 13:33 |  | Fix a test script problem causing errors with Tcl 9. (check-in: dbec2c47d0 ... user: dan tags: trunk) | 
| 12:18 |  | Small performance improvement for the new %#Q conversion in printf. (closed check-in: 17e440781e ... user: drh tags: unistr) | 
| 12:11 |  | Update the autoconf Makefile for nmake to support SETLK_TIMEOUT. (check-in: 46909fe334 ... user: drh tags: trunk) | 
| 11:48 |  | Avoid running certain tests in walsetlk2.test if SQLITE_ENABLE_SETLK_TIMEOUT is set to 2, not 1. (check-in: 86788c08fa ... user: dan tags: trunk) | 
| 11:47 |  | Consolidate two different UTF8 encoders into a single subroutine. (check-in: 6208e49485 ... user: drh tags: unistr) | 
| 11:29 |  | Remove an assert() added by [e88212b1] that is sometimes false. (check-in: 0f6223b8f6 ... user: dan tags: trunk) | 
| 2025-02-24 |  |  | 
| 23:34 |  | Fix the new shellA.test test script so that it works with Valgrind. (check-in: c3949852ce ... user: drh tags: unistr) | 
| 21:27 |  | Support SQLITE_ENABLE_SETLK_TIMEOUT on windows. (check-in: e88212b10a ... user: dan tags: trunk) | 
| 20:53 |  | Slightly simplify auto.def and autoconf/auto.def. (closed check-in: d84f591b96 ... user: stephan tags: configure-dll-support) | 
| 20:13 |  | Use an assert() to fix a harmless (false-positive) scan-build warning in the CLI. (check-in: 4c56cd7392 ... user: drh tags: unistr) | 
| 18:45 |  | autoconf/auto.def: remove a define which made superfluous by [b8bf966628c0]. Add a check/status message for whether to statically link the library into the CLI shell. (check-in: df95e908ca ... user: stephan tags: configure-dll-support) | 
| 18:14 |  | configure: in autoconf build if both --disable-shared and --disable-static-shell are used together, ignore the former and emit a notice to that effect. (check-in: b8bf966628 ... user: stephan tags: configure-dll-support) | 
| 17:50 |  | Only use unistr() in columnar formats when strictly needed. Do not use unistr() in insert mode when --escape is off. More test cases. (check-in: e029828de9 ... user: drh tags: unistr) | 
| 13:51 |  | Add the unistr_quote() function that works like quote(), but also escape control characters using unistr() if necessary. (check-in: e99e37b54b ... user: drh tags: unistr) | 
| 13:27 |  | Bug fix and initial test-case infrastructure for control-character escaping in the CLI. (check-in: c809997792 ... user: drh tags: unistr) | 
| 12:41 |  | Ongoing work to get all the quoting and escaping variations in the CLI working correctly. (check-in: b77aea93e7 ... user: drh tags: unistr) | 
| 12:19 |  | Configure doc tweaks. Move the package version/build dir output so that it does not appear at the start of --help text, but otherwise still appears before the first output from low-level config initialization. (check-in: 826bad10e9 ... user: stephan tags: configure-dll-support) | 
| 11:21 |  | Further consolidation of auto.def and autoconf/auto.def. Note in configure --help that --disable-largefile is a legacy option which has no effect on the library but may influence the generated sqlite_cfg.h. (check-in: 17fd6836f3 ... user: stephan tags: configure-dll-support) | 
| 10:52 |  | Merge latest changes from trunk into this branch. (closed check-in: 55324d1c86 ... user: dan tags: win32-enable-setlk) | 
| 10:30 |  | Move the configure output about the SQLite version and build dirs up, so that they appear before any tests for the C compiler and such. (check-in: 83e0f3d138 ... user: stephan tags: configure-dll-support) | 
| 08:42 |  | Further consolidation of the main auto.def and the autoconf auto.def content into sqlite-config.tcl. (check-in: 76a4efa966 ... user: stephan tags: configure-dll-support) | 
| 07:06 |  | Configure-internal doc tweaks. Fix the distclean rules for libsqlite3.dll.a on msys/cygwin/mingw, broken when the DLL basename was changed for those platforms. (check-in: 8641e2c7c8 ... user: stephan tags: configure-dll-support) | 
| 05:10 |  | Add some internal notes about potential changes for the rpath flag handling. (check-in: 6f904f0695 ... user: stephan tags: configure-dll-support) | 
| 04:47 |  | Update jimsh0.c from upstream autosetup to address autosetup ticket #75, triggered by this build in the Gentoo project. (check-in: ffd05de8a3 ... user: stephan tags: configure-dll-support) | 
| 00:40 |  | Make column output modes in the CLI responsive to the --escape setting. (check-in: 14ff5557d4 ... user: drh tags: unistr) | 
| 00:18 |  | Three different --escape modes: symbol, ascii, off. (check-in: b5adb52fc0 ... user: drh tags: unistr) | 
| 2025-02-23 |  |  | 
| 20:50 |  | Further improvements to control-character escapes in the CLI. (check-in: 9c2f974c17 ... user: drh tags: unistr) | 
| 20:20 |  | Work toward VT100-safe output from the CLI by default. (check-in: 44c44620e8 ... user: drh tags: unistr) | 
| 17:55 |  | Add (disabled) infrastructure for disabling use of the rpath linker flag. This is a proactive precautionary measure because how the rpath is used does not match up with what the unix-on-windows environments do with their DLLs. (check-in: 38ba8e837c ... user: stephan tags: configure-dll-support) | 
| 17:24 |  | Internal cleanups to how certain linker flags are tested. (check-in: 6743e3e5b8 ... user: stephan tags: configure-dll-support) | 
| 15:34 |  | Internal configure doc cleanups. Apply the [5ecbe7ddc4] unix-on-windows DLL install dir fix to the canonical build. (check-in: 51b9812df9 ... user: stephan tags: configure-dll-support) | 
| 11:48 |  | The %#Q conversion now adds unistr('...') around the converted string if escape characters were inserted. %#w now works just like %w as escape sequences inside of identifiers are not recognized. (check-in: 997391d420 ... user: drh tags: unistr) | 
| 00:09 |  | Enhance the %Q, %q, and %w printf conversions so that if the alternate-form flag # is present, they transform backslash and control characters into unistr()-style backslash escape codes. (check-in: ffbfcc2bbb ... user: drh tags: unistr) | 
| 2025-02-22 |  |  | 
| 23:18 |  | Prototype implementation of the unistr() SQL function. (check-in: 7cc302de05 ... user: drh tags: unistr) | 
| 17:33 |  | Update the help text to account for [35b1c32f80b9]. (check-in: f2e7355d87 ... user: stephan tags: configure-dll-support) | 
| 17:30 |  | Replace --dll-basename=none with --dll-basename=default. (check-in: 35b1c32f80 ... user: stephan tags: configure-dll-support) | 
| 16:44 |  | Tamp down various harmless compiler warnings. Use "int" in places instead of "u16" or "i16" since the compiler complains less and generates faster code. (check-in: 742827f049 ... user: drh tags: trunk) | 
| 16:31 |  | On unix-on-windows platforms, if either of --out-implib or --dll-basename are not provided, auto-enable them. Add the 'none' special value to those flags to specifically disable the environment-specific handling of those flags. (check-in: 486400801a ... user: stephan tags: configure-dll-support) | 
| 14:58 |  | Ensure that [08c9e56f60] uses an absolute path when appending .exe to mksourceid. (check-in: 906d27f7a6 ... user: stephan tags: configure-dll-support) | 
| 14:53 |  | Random non-functional configure script cleanups. (check-in: 2ee7310980 ... user: stephan tags: configure-dll-support) | 
| 14:53 |  | tool/mksqlite3h.tcl: when invoking the mksourceid binary, add a .exe to it, if needed, to appease msys2. (check-in: 08c9e56f60 ... user: stephan tags: configure-dll-support) | 
| 11:40 |  | Fix an incorrect assert added by [d7729dbbf231d57c]. (check-in: eeea11278b ... user: drh tags: trunk) | 
| 00:01 |  | Fix installation rules for import lib in the autoconf bundle, broken by recent refactoring in this branch. (check-in: 95b71fc7e9 ... user: stephan tags: configure-dll-support) | 
| 2025-02-21 |  |  | 
| 23:43 |  | Rename configure-defined ENABLE_SHARED/STATIC makefile vars to ENABLE_LIB_... so that they're consistent between the canonical and autoconf builds (baby steps towards further configure code consolidation). (check-in: a7647fd077 ... user: stephan tags: configure-dll-support) | 
| 23:27 |  | Configure docs/help text tweaks. No functional changes. (check-in: 701cc1aff4 ... user: stephan tags: configure-dll-support) | 
| 22:46 |  | Only use the platform-specific --dll-basename if that option is explicitly provided, else use libsqlite3. (check-in: 69bacfdce5 ... user: stephan tags: configure-dll-support) | 
| 22:39 |  | Don't quote dir name in makefile deps, as the quotes are part of the name in that context. (check-in: b0b9e92599 ... user: stephan tags: configure-dll-support) | 
| 22:35 |  | Ensure that the prefix/bin dir exists on msys/cygwin installations before installing the DLL there. (check-in: 5ecbe7ddc4 ... user: stephan tags: configure-dll-support) | 
| 21:58 |  | Add --dll-basename flag to give clients control over the DLL's base name. Split the DLL installation makefile rules into digestible, platform-specific chunks. On Windows-side Unix-like environments, install the DLL into the bin dir. Requires testing and feedback from those platforms' package maintainers to confirm it follows the expectated conventions. (check-in: 8d5fed807b ... user: stephan tags: configure-dll-support) | 
| 20:35 |  | The number of declared columns in an index is limited to SQLITE_LIMIT_COLUMN. But the actual number of columns in the implementation might need to be twice as much to account for the primary key at the end. Ensure that the code is able to deal with this. This is a correction to check-in [d7729dbbf231d57c]. (check-in: 5822feec43 ... user: drh tags: trunk) | 
| 20:28 |  | Extend --out-implib to optionally specify the import lib's name. Add as-yet-unused internals for specifying the DLL's base name (which differs across the Windows-side Unix-esque environments). (check-in: 12a55d343f ... user: stephan tags: configure-dll-support) | 
| 20:22 |  | Makefile-internal var renaming in prep for pending portability-related changes in the handling of DLLs. No functional/build interface changes. (check-in: ebf41fc90a ... user: stephan tags: trunk) | 
| 17:03 |  | Detect when a UNIQUE or PRIMARY KEY on a WITHOUT ROWID table would need to use more than SQLITE_LIMIT_COLUMN columns and raise an error. Also include some unrelated compiler warning fixes. (check-in: d7729dbbf2 ... user: drh tags: trunk) | 
| 03:19 |  | configure: in several places where support for -Wl,... linker flags are checked, ensure that the check invokes the linker (not just the compiler) to avoid false positives. This allows us to remove the AIX-specific handling and --disable-rpath bits added in [a15e0f680], as well as make several similar checks more robust. (check-in: 4e81e2c707 ... user: stephan tags: trunk) | 
| 2025-02-20 |  |  | 
| 17:14 |  | configure: automatically fail the check for rpath on AIX systems and provide a --disable-rpath flag as a fallback for use on platforms which pass the configure-time rpath check but then fail at link-time. Based on discussion in forum thread ae5bd8a84b. (check-in: a15e0f6802 ... user: stephan tags: trunk) | 
| 16:45 |  | Fix autoconf bundle to honor the --disable-static and --disable-shared flags, as reported in forum post ae5bd8a84b. Problem introduced in 3.49.0. (check-in: 56027220cc ... user: stephan tags: trunk) | 
| 05:39 |  | wasm: do not build the (untested/unsupported) node-for-node build by default, to cut build time by about 15%. Adjacent cleanups in mkwasmbuilds.c. (check-in: e4539ebebd ... user: stephan tags: trunk) | 
| 04:45 |  | wasm makefile docs: make explicit that the node.js-for-node.js builds (as opposed to the node.js-for-browser builds) are both untested and unsupported. (check-in: e1f184889f ... user: stephan tags: trunk) | 
| 04:14 |  | Add the pause/unpause capability to the opfs-sahpool VFS, as discussed in forum thread fe8cdb8431c. Summary: this gives clients a way to eke some degree of multi-page/tab/Worker concurrency out of this VFS but requires that coordination to be implemented client-side, e.g. via a SharedWorker or WebLocks. (check-in: b5dbd52195 ... user: stephan tags: trunk) | 
| 03:27 |  | configure: when running proj-check-function-in-lib, strip -Werror from CFLAGS for the duration of the test. This enables CFLAGS='-Wall -Werror' and the like to be passed to configure without breaking these configure-time checks. (check-in: 4ae9d6c642 ... user: stephan tags: trunk) | 
| 2025-02-19 |  |  | 
| 13:05 |  | Increase default CLI shell .prompt buffer length to 128 bytes, based on discussion in forum post 362f185a6aa. (check-in: 628407f03d ... user: stephan tags: trunk) | 
| 13:02 |  | Slight tweak to the CLI shell help output to help convey that it can accept multiple SQL arguments, as suggested in forum post 20e617feee. (check-in: 82fc67070f ... user: stephan tags: trunk) | 
| 2025-02-18 |  |  | 
| 15:11 |  | Have SQLITE_FTS5_ENABLE_TEST_MI builds avoid reading the database schema from within sqlite3_open(). (check-in: 15dc524fd4 ... user: dan tags: trunk) | 
| 01:16 |  | Update path on the web server for test/snapshot instances of fiddle. (check-in: 57caa3136d ... user: stephan tags: trunk) | 
| 2025-02-17 |  |  | 
| 20:13 |  | Add an ORDER BY to a test case for skip-scan to make the output consistent. (check-in: 9c9c19414a ... user: drh tags: trunk) | 
| 19:44 |  | Fix problems in test script fkey6.test. (check-in: 088e476519 ... user: dan tags: trunk) | 
| 18:09 |  | Additional changes making it easier to prove that integer overflow does not occur. No problems found. (check-in: e846743a87 ... user: drh tags: trunk) | 
| 17:33 |  | Code changes that make it easier to prove that no 32-bit integer overflows happen during memory allocation. No problems fixed; this change is just to make future maintenance easier. (check-in: 215650a5a1 ... user: drh tags: trunk) | 
| 16:14 |  | Add two generated files to the distclean rules of the autoconf bundle. (check-in: b59d0ebb22 ... user: stephan tags: trunk) | 
| 16:04 |  | Replace TEXE (legacy name) with T.exe (3.48+ name) in two places in makefiles. This fixes distclean of jimsh.exe in the canonical build in non-native Windows environments. (check-in: edb8a78c02 ... user: stephan tags: trunk) | 
| 14:16 |  | Harden the SQLITE_DBCONFIG_LOOKASIDE interface against misuse, such as described in forum post 48f365daec, CVE-2025-29088, and CVE-2025-52099. Enhancements to the SQLITE_DBCONFIG_LOOKASIDE documentation. Test cases in TH3. (check-in: 1ec4c308c7 ... user: drh tags: trunk) | 
| 10:58 |  | Fix a typo (a missing ")") in a comment that is used to generate documentation. No changes to code. (check-in: ea21685658 ... user: drh tags: trunk) | 
| 2025-02-16 |  |  | 
| 19:16 |  | Internal reordering/re-grouping of some configure flags. No functional changes. (check-in: 0c085a5ab5 ... user: stephan tags: trunk) | 
| 18:14 |  | Re-add the --disable-static-shell configure feature to the autoconf bundle. It got lost in the autoconf-to-autosetup port, as reported in forum post cc219ee7044. (check-in: 47e817f663 ... user: stephan tags: trunk) | 
| 13:49 |  | Update autoconf/Makefile.msc from tool/mkmsvcmin.tcl. (check-in: 9ffe94d2ea ... user: stephan tags: trunk) | 
| 13:13 |  | API naming typo reported in forum post 416f2d8dec. No functional changes. (check-in: 0cfb9e234e ... user: stephan tags: trunk) | 
| 10:57 |  | Add a typecast to avoid 32-bit integer overflow in the concat_ws() function with an enormous separator values and many arguments. (check-in: 498e3f1cf5 ... user: drh tags: trunk) | 
| 2025-02-15 |  |  | 
| 17:29 |  | Remove tea version check from tool/srctree-check.tcl, as it's obsoleted by [be265559]. (check-in: 1860ea060b ... user: stephan tags: trunk) | 
| 16:24 |  | Automate update of the library version number in autoconf/tea/configure.ac as part of the tool/mkautoconfamal.sh process, per /chat discussion. (check-in: be265559a3 ... user: stephan tags: trunk) | 
| 16:12 |  | DLL installation rules no longer create versioned symlinks on platforms where the DLL extension is '.dll' (cygwin, msys2, etc.), as suggested in forum post 28bb79638844c328. (check-in: d743410665 ... user: stephan tags: trunk) | 
| 14:12 |  | A cleaner workaround for [6a21d6a2], provided by autosetup's creator. (check-in: 334ed723d0 ... user: stephan tags: trunk) | 
| 2025-02-14 |  |  | 
| 18:17 |  | More informative output for the pthread feature test. (check-in: 01b9ed73ca ... user: stephan tags: trunk) | 
| 18:06 |  | Make failure to find pthreads support non-fatal unless --enable-threadsafe is specifically passed in. Build regression reported in forum post 8e0fdb8c0d. (check-in: 806ad1ac17 ... user: stephan tags: trunk) | 
| 17:52 |  | Simplify the --help workaround introduced in [6a21d6a2ed]. (check-in: b0928f2043 ... user: stephan tags: trunk) | 
| 17:21 |  | Work around a autosetup --help bug which only(?) triggers on tcl 8.6, introduced by [9978c87139b7b042] and reported upstream as ticket #73. Summary: calling 'options' from a proc, instead of the global scope, triggers an error about an invalid use of 'break'. (check-in: 6a21d6a2ed ... user: stephan tags: trunk) | 
| 16:42 |  | ICU config support: add more details to error messages and correct a typo which would cause a configure crash if --with-icu-config=X refered to a non-executable X. (check-in: ff50892665 ... user: stephan tags: trunk) | 
| 10:28 |  | Fix test cases added for [4b4f33d791fe4318] so that they also work when compiling with ICU support. Forum post 2ca8a09a7e (check-in: 5964616dc9 ... user: drh tags: trunk) | 
| 2025-02-13 |  |  | 
| 16:20 |  | When using the --out-implib build option, install the generated .dll.a file to PREFIX/lib. (check-in: 0dadea0ec8 ... user: stephan tags: trunk) | 
| 14:47 |  | Ensure the counts of "deferred FK violations" and "deferred immediate FK violations" are kept separate when "PRAGMA defer_foreign_keys" is used. (check-in: c5190b0fd9 ... user: dan tags: trunk) | 
| 2025-02-12 |  |  | 
| 20:19 |  | Fix a few 32-bit overflow problems in showdb.c. (check-in: 9377766647 ... user: dan tags: trunk) | 
| 17:21 |  | Have the win32 VFS take a temporary shared lock (instead of the current exclusive) on the pending-byte when taking a SHARED lock on a db. Do not lock the pending-byte at all when taking an EXCLUSIVE lock if RESERVED is not already held. (check-in: 5127509abb ... user: dan tags: win32-enable-setlk) | 
| 16:59 |  | configure --help cleanups and eliminate the use of a JS-esque inner function in sqlite-config.tcl. No functional changes. (check-in: 6df859cd18 ... user: stephan tags: trunk) | 
| 15:31 |  | Remove old function declaration accidentally left in sqlite3session.h. (check-in: 0cfbe349d4 ... user: dan tags: trunk) | 
| 14:51 |  | Tiny tcl comment tweaks. No functional changes. (check-in: da94e551c0 ... user: stephan tags: trunk) | 
| 08:07 |  | Fix test script errors in walsetlk3.test. Tests still don't all pass. (check-in: 56eb4114f8 ... user: dan tags: win32-enable-setlk) | 
| 2025-02-11 |  |  | 
| 19:54 |  | Omit the src/pragma.h file. It is generated by the tool/mkpragmatab.tcl script. The makefiles now know how to build this file for themselves. (check-in: 9709ed1cdf ... user: drh tags: trunk) | 
| 19:40 |  | Omit the src/ctime.c source file, since it is automatically generated by a TCL script. Instead, add rules to the various makefiles to generate ctime.c on demand. (check-in: 958bb5de7c ... user: drh tags: trunk) | 
| 18:32 |  | Update tool/mkctimec.tcl so that sqlite3_compileoption_xxx() APIs report on the SQLITE_ENABLE_SETLK_TIMEOUT option. (check-in: 303e8009ab ... user: dan tags: win32-enable-setlk) | 
| 18:29 |  | Fix "unused variable" warnings in os_unix.c and os_win.c. (check-in: 8e809632ba ... user: dan tags: win32-enable-setlk) | 
| 17:20 |  | Rename a configure-internal function and fix handling of --dump-defines for the autoconf bundle. (check-in: 6aa54cc180 ... user: stephan tags: trunk) | 
| 17:10 |  | Merge latest changes from trunk into this branch. (check-in: e5ec5bb9f4 ... user: dan tags: win32-enable-setlk) | 
| 17:10 |  | Move the configure flags definition and handling into autosetup/sqlite-config.tcl to avoid duplication between auto.def and autoconf/auto.def while still giving us a way to filter the canonical-tree-only flags out of the autoconf build. (check-in: 9978c87139 ... user: stephan tags: trunk) | 
| 13:13 |  | Make the --out-implib support ([6092b0b86bf93a3d]) specifically opt-in because the feature check for it passes on some platforms where it is not recognized at link-time. (check-in: 75535f2355 ... user: stephan tags: trunk) | 
| 13:00 |  | Adapt Fossil SCM patch 3bff7b92d6d, applying -lrt for platforms which need it for one of {fdatasync, nanosleep}. (check-in: 1768de6e9e ... user: stephan tags: trunk) | 
| 2025-02-10 |  |  | 
| 20:46 |  | Experimental change to allow clients to block when taking a SHARED lock to connect to a wal mode database. (check-in: d2d6a000fb ... user: dan tags: win32-enable-setlk) | 
| 19:48 |  | Remove a pointless line of code. (check-in: 57b2b812c8 ... user: drh tags: trunk) | 
| 19:12 |  | Add the --size option to the test/speedtest.tcl script. (check-in: 6b9007cb30 ... user: drh tags: trunk) | 
| 18:41 |  | Minor autosetup doc tweaks. (check-in: bb53beb267 ... user: stephan tags: trunk) | 
| 16:34 |  | Make the "mix1" testset the default for speedtest1. (check-in: 7a0c3d0e14 ... user: drh tags: trunk) | 
| 16:13 |  | Enhance speedtest1 with a new testset that calls sqlite3_open() and uses the connection many times. (check-in: ce307addb0 ... user: drh tags: trunk) | 
| 15:58 |  | Add jimsh.exe and jimsh0.exe to the clean rules in Makefile.msc so that stale builds of those files from msys2 and friends do not cause mysterious build errors. (check-in: 44b4e4db32 ... user: stephan tags: trunk) | 
| 15:01 |  | configure: when dlopen() is not found, only fail fatally if --enable-load-extension is explicitly provided, else warn instead. Based on discussion around forum post 2efe9c33bd9021ca. Update proj-indented-notice to behave like its docs say it should when the -error flag is used. (check-in: f28e52cbf9 ... user: stephan tags: trunk) | 
| 11:16 |  | Convert some expensive NEVER() and ASSERT() macros into assert()s. (check-in: 4aad891802 ... user: drh tags: trunk) | 
| 00:20 |  | Small performance increase in jsonTranslateBlobToText(). (check-in: 3b1dcac2ee ... user: drh tags: trunk) | 
| 2025-02-09 |  |  | 
| 20:23 |  | Performance optimization to the substr() SQL function. (check-in: ce228ce3d0 ... user: drh tags: trunk) | 
| 19:49 |  | Performance and size optimization for the sqlite3ColumnIndex() routine. (check-in: a93e3fe0ee ... user: drh tags: trunk) | 
| 05:37 |  | Update the 'clean' rules for autoconf/Makefile.in to account for [6092b0b8]. (check-in: f3a35fdc91 ... user: stephan tags: trunk) | 
| 05:30 |  | Apply [6092b0b8] to autoconf/Makefile.in. (check-in: 9ae245cff4 ... user: stephan tags: trunk) | 
| 04:38 |  | configure: if the linker supports --out-implib, generate libsqlite3.X.a, where X is the platform's DLL file extension. Discussion in/around forum post 0c7fc097b2. (check-in: 6092b0b86b ... user: stephan tags: trunk) | 
| 04:16 |  | wasm: add some build-time validation to ensure that the problem fixed in [65798c09a00662a3] does not recur. Ensure that files generated via mkwasmbuilds.c have the generated makefile as a dependency. (check-in: 55e0136594 ... user: stephan tags: trunk) | 
| 03:24 |  | wasm: when building in -O0 mode (typical dev mode), use -sASSERTIONS=2, else -sASSERTIONS=0, in response Emscripten checkin 7e3e35cbff9, which adds assertions to check for the condition reported in Emscripten ticket 23420. Update some unrelated JS-side internal docs. (check-in: 1f554610ba ... user: stephan tags: trunk) | 
| 02:41 |  | JS: add a mechanism to the Worker1 exec API to fetch the last_insert_rowid(), as requested in forum post 56bc353901. (check-in: c22c483607 ... user: stephan tags: trunk) | 
| 01:50 |  | wasm: add some build-time validation to ensure that the problem fixed in [65798c09a00662a3] does not recur. Ensure that files generated via mkwasmbuilds.c have the generated makefile as a dependency. (check-in: 012b308b04 ... user: stephan tags: branch-3.49) | 
| 01:25 |  | configure: when transfering ENABLE/OMIT flags from CFLAGS to OPT_FEATURE_FLAGS, also do the same for CPPFLAGS and remove those ENABLE/OMIT flags from CFLAGS/CPPFLAGS to mimic legacy build behavior. Strip ENABLE/OMIT flags from BUILD_CFLAGS but do not transfer those to OPT_FEATURE_FLAGS, also to mimic legacy behavior. This is the second part of a fix discussed at forum post 9801e54665afd728. (check-in: 16d307cc6c ... user: stephan tags: trunk) | 
| 00:54 |  | Add the "star" testset to speedtest1. Include it as part of "mix1". (check-in: f1345b84ea ... user: drh tags: trunk) | 
| 2025-02-08 |  |  | 
| 16:16 |  | Put a 16-byte hash table for column names on each Table object, to speed up column name lookups. (check-in: 11eb8f99e5 ... user: drh tags: trunk) | 
| 14:15 |  | Use the sqlite3ColumnIndex() routine to look up a column in a table, rather than using a custom loop. Performance improvement, size reduction, and complexity decrease. (check-in: 351dbbc2bf ... user: drh tags: trunk) | 
| 13:34 |  | Fix GCC-isms and compiler warnings introduced by recent check-ins [c56092507c967230] and [6e57848fe1e0e2b5]. (check-in: 91102c0437 ... user: drh tags: trunk) | 
| 12:04 |  | Improvements to the symbol table hash: faster lookups with fewer calls to sqlite3StrICmp() for schemas with many symbols and large names. (check-in: 6e57848fe1 ... user: drh tags: trunk) | 
| 11:15 |  | Improvements to the hash table used to store symbols in the schema, so that it works better (requires fewer calls to sqlite3StrICmp()) for large schemas, and uses less code space. (closed check-in: 0318b68c84 ... user: drh tags: hash-improvements) | 
| 2025-02-07 |  |  | 
| 19:18 |  | build: work around a report of (install -d DIR) failing in one environment if DIR already exists. (check-in: 8f7b7840d4 ... user: stephan tags: trunk) | 
| 19:15 |  | build: work around a report of (install -d DIR) failing in one environment if DIR already exists. (check-in: 4bc6fe30c9 ... user: stephan tags: branch-3.49) | 
| 19:09 |  | Fix comments on the Parse.nMaxArgs field so that they are correct. Add assert()s to ensure they are correct. Other Parse changes to reduce the amount of memset() needed to initialize it. (check-in: c56092507c ... user: drh tags: trunk) | 
| 15:49 |  | Further reduction in the amount of memset() needed to initialize the Parse object. (check-in: 45e462c006 ... user: drh tags: trunk) | 
| 14:44 |  | Mac-specific build fixes discussed in forum thread 9dfd5b8fd525a5d7: rename dylib links and add legacy-compatibility versioning stamps to libsqlite3.dylib. (Edit: this was merged from [aa0033d5aa] but a post-merge update on my end lost that link.) (check-in: 668bcf327a ... user: stephan tags: trunk) | 
| 13:37 |  | Reduce the amount of memset() needed to initialize the Parse object. (check-in: 284538d848 ... user: drh tags: trunk) | 
| 12:58 |  | configure: change extension of static libraries from .lib to .a on msys/cygwin, as per discussion in forum post 02db2d4240. Replace unidiomatic JS-style use of inner procs in autosetup/proj.tcl with TCL lambdas. (check-in: 46f51e2a37 ... user: stephan tags: trunk) | 
| 12:33 |  | Reorder a piece of ext/wasm/GNUmakefile to correct the timing of various var accesses. Fixes a problem reported in the downstream npm subproject and explains the confusion in [4aa025a943a4024094b9] (which has been reverted). (check-in: 65798c09a0 ... user: stephan tags: trunk) | 
| 11:15 |  | Fix out-of-tree builds of the autoconf bundle, as reported in forum post a0cd0beb1baa6bef. (check-in: d47964a65b ... user: stephan tags: trunk) | 
| 01:43 |  | Bump TEA configure.ac version number. (check-in: a1a9c780d1 ... user: stephan tags: trunk) | 
| 2025-02-06 |  |  | 
| 17:29 |  | Adjust the test/speedtest.tcl script so that it uses an on-disk database rather than an in-memory database. (check-in: e93d7aa1db ... user: drh tags: trunk) | 
| 17:10 |  | Fix a harmless typo in a comment. (check-in: ed82272904 ... user: drh tags: trunk) | 
| 13:36 |  | Bump version number to 3.50.0 for the next dev cycle. (check-in: 46d08071f3 ... user: stephan tags: trunk) | 
| 13:18 |  | Fix a dependencies/order-of-operations bug in ext/wasm/GNUmakefile which causes creation of files filtered via c-pp to fail if the ext/wasm/jswasm dir did not exist beforehand. (check-in: 4bac761388 ... user: stephan tags: trunk) | 
| 11:55 |  | Version 3.49.0 (check-in: 4a7dd425dc ... user: drh tags: trunk, release, major-release, version-3.49.0) | 
| 2025-02-05 |  |  | 
| 16:08 |  | Fix a harmless unused-parameter compiler warning introduced by [75f3d8744879]. (check-in: 9f28f6694f ... user: drh tags: trunk) | 
| 12:56 |  | Summarize known incompatibilities/gotches between JimTCL and canonical TCL, and their workarounds. Doc changes only. (check-in: 60c932e67b ... user: stephan tags: trunk) | 
| 12:49 |  | Upstream JimTCL patch and minor tcl script tweaks to support (fconfigure -translation binary) for better cross-platform build portability. (check-in: 0974a17c45 ... user: stephan tags: trunk) | 
| 12:15 |  | Portability tweaks to the code generators so that they produce identical output with JimTCL on Windows and Unix systems (that is: no Windows-style line endings). (leaf check-in: fddcfbcafd ... user: stephan tags: branch-3.48) | 
| 12:02 |  | Attempted improvements to the SQLITE_DBCONFIG_... documentation. (check-in: baac853871 ... user: drh tags: trunk) | 
| 2025-02-04 |  |  | 
| 02:38 |  | Detect and report signed integer overflow in the sumInverse() routine, used by window functions. (check-in: 317e9a470f ... user: drh tags: trunk) | 
| 2025-02-03 |  |  | 
| 23:27 |  | Test case for the FTS3 fix in the previous check-in. This check-in also accidentally includes a change to session that is intended to remove a static analysis warning. (check-in: 459d8f695e ... user: drh tags: trunk) | 
| 23:19 |  | Fix a potential UAF in FTS3. (check-in: 75f3d87448 ... user: drh tags: trunk) | 
| 21:24 |  | New dbsqlfuzz case added to fuzzdata8.db. (closed check-in: 7ee20cfc23 ... user: drh tags: fuzzer-20250203) | 
| 21:04 |  | The Parse.addrExplain field is never even if SQLITE_OMIT_EXPLAIN is defined. (check-in: 5d81a984c6 ... user: drh tags: trunk) | 
| 18:53 |  | Enhance fuzzcheck so that the --sqlid and --dbid options can take a range of IDs to run. (check-in: 408fa57f04 ... user: drh tags: trunk) | 
| 18:49 |  | Fixes and improved documentation to the new --sqlid and --dbid range options in fuzzcheck. (closed check-in: 59e2663244 ... user: drh tags: fuzzcheck-improvements) | 
| 18:36 |  | Enhance fuzzcheck so that the --sqlid and --dbid options can specify a range of tests to be run. (check-in: cdd0af6f86 ... user: drh tags: fuzzcheck-improvements) | 
| 18:05 |  | Fix for a potential UAF in FTS5. (check-in: e33f2fedda ... user: drh tags: trunk) | 
| 18:01 |  | Improve the JS-side sqlite3_set_auxdata() test to also trigger the case that the aux data actually gets reused. Test changes only, no library code. (check-in: 9f27379d86 ... user: stephan tags: trunk) | 
| 17:54 |  | Fix a use-after-free case in fts5 provoked by fuzzdata8.db. Tcl test case pending. (closed check-in: a4962df665 ... user: dan tags: fuzz-data) | 
| 17:45 |  | New test case for test/fuzzdata8.db (check-in: 8a882f976e ... user: drh tags: fuzz-data) | 
| 17:34 |  | Rework [76c8435a] to eliminate automatic JS-to-WASM function conversions of sqlite3_set_auxdata() destructors because it can leads to leaks on every call of a UDF. This feature never worked before [76c8435a] but fixing it was ill-conceived because of the memory leakage it introduces. WASM function pointers can still be used as destructors in this context. (check-in: 3fb993af0c ... user: stephan tags: trunk) | 
| 17:21 |  | Add a test app to assist in validating the SAHPool digest calculation fix. (check-in: a1e304b802 ... user: stephan tags: sahpool-digest) | 
| 16:26 |  | Initial work on a fix for the SAHPool VFS's effectively-no-op digest calculation, as reported in ticket #97 of the downstream npm subproject. This requires more testing alongside databases created before this version to ensure that it's backwards-compatible. (check-in: 9234c33f92 ... user: stephan tags: sahpool-digest) | 
| 14:55 |  | Add a more complete test for [76c8435a] and add some commentary about (A) the inability to automatically clean up automatically-generated WASM proxy functions for sqlite3_set_auxdata() destructors and (B) how to deal with (A) to avoid leaking WASM proxy functions. (check-in: d693c2dddb ... user: stephan tags: trunk) | 
| 14:44 |  | Fix the build process on Windows so that it generates identical sqlite3.c, sqlite3.h, and shell.c files on Windows and Unix. This patch also includes a change to JS bindings that got caught up in the branch. (check-in: 91ef45fc29 ... user: drh tags: trunk) | 
| 14:40 |  | Bring the autoconf-tarball Makefile.msc into alignment with the main Makefile.msc. (closed check-in: 52a7a162d1 ... user: drh tags: windows-build-issue) | 
| 14:04 |  | Correct the FuncPtrAdapter signature for the JS binding of sqlite3_set_auxdata(). Reported in the npm subproject, ticket #92. (check-in: 76c8435a5f ... user: stephan tags: windows-build-issue) | 
| 13:20 |  | Adjust the build process to avoid extra CR characters inserted by jimsh on Windows. (check-in: 1f54853932 ... user: drh tags: windows-build-issue) | 
| 12:42 |  | The "clean" target on Makefile.msc should not delete the auto.def file. Bug: causes "make devtest" to fail. Moved to a branch until repaired. (check-in: 72fbcedfb0 ... user: drh tags: windows-build-issue) | 
| 2025-02-02 |  |  | 
| 18:01 |  | Add a header comment to test/speedtest1.c that outlines how to compile the program using historical amalgamation sources, for comparison testing. (check-in: 602d4dd69e ... user: drh tags: trunk) | 
| 16:52 |  | Update to the "--help" output from speedtest1. (check-in: 5216452047 ... user: drh tags: trunk) | 
| 12:15 |  | URL typo fix in code comments. No code changes. (check-in: 002a123094 ... user: stephan tags: trunk) | 
| 2025-02-01 |  |  | 
| 23:49 |  | Add a missing "db2 close" to the ext/session/session_gen.test test module. (check-in: 4068f586dd ... user: drh tags: trunk) | 
| 23:34 |  | Fix a comment typo - in a comment used to generate documentation. (check-in: b8de75eede ... user: drh tags: trunk) | 
| 23:28 |  | Remove an ALWAYS() added by [2567298f4b0fdfeb] because dbsqlfuzz found a way to reach it. The test case was added to TH3. (check-in: d08a7aa898 ... user: drh tags: trunk) | 
| 21:06 |  | Enable SQLITE_ENABLE_NORMALIZE for several test configurations. (check-in: b07c64077a ... user: drh tags: trunk) | 
| 20:53 |  | Fix an issue with sqlite3_normalized_sql() caused by changes needed to support SQLITE_DBCONFIG_ENABLE_COMMENT. (check-in: 79d287a394 ... user: drh tags: trunk) | 
| 09:44 |  | JS doc typo fixes. No functional changes. (check-in: e0d9c74c1c ... user: stephan tags: trunk) | 
| 2025-01-31 |  |  | 
| 20:18 |  | Tweaks to [4b4f33d791fe4318] to make it easier to test. (check-in: 2567298f4b ... user: drh tags: trunk) | 
| 18:56 |  | Fix test case numbering in tests recently added to like3.test. (check-in: 6df2098c54 ... user: drh tags: trunk) | 
| 18:45 |  | Expose the new SQLITE_DBCONFIG_ENABLE_ATTACH_CREATE/WRITE and SQLITE_DBCONFIG_ENABLE_COMMENTS (from [325e547a21955]) to JS/WASM. (check-in: 8da1a9af66 ... user: stephan tags: trunk) | 
| 18:33 |  | Remove an assert() that is not true if a trace-callback is deregistered while there are active statements. Forum post 3547aa1078510686. (check-in: 0ecfc4d0eb ... user: dan tags: trunk) | 
| 17:47 |  | Minor cleanups in the opfs-sahpool pause/unpause API demo. (closed check-in: e205cdc468 ... user: stephan tags: opfs-sahpool-pause) | 
| 16:34 |  | Add the conventional license header to sahpool-worker.js and correct the date on the header in sahpool-pausing.js. (check-in: f7c3026b0d ... user: stephan tags: opfs-sahpool-pause) | 
| 16:25 |  | Add a small test app demonstrating cooperative semi-concurrency of the opfs-sahpool VFS using its un/pauseVfs() APIs. (check-in: 09570c55a2 ... user: stephan tags: opfs-sahpool-pause) | 
| 14:52 |  | Fix a problem with LIKE and GLOB processing in utf-16be databases in cases where the utf-8 encoding of a character ends with the byte 0xBF. Forum post d7b90d92ffbfc61f (check-in: 4b4f33d791 ... user: dan tags: trunk) | 
| 14:52 |  | If any errors occur while processing sqlite_dbpage changes, cancel pending truncate operations. (check-in: 6138e0dc4e ... user: drh tags: trunk) | 
| 14:25 |  | Cleanups in the opfs-sahpool VFS pause/unpause feature and its tests. (check-in: 184ba37702 ... user: stephan tags: opfs-sahpool-pause) | 
| 13:32 |  | Three new options to sqlite3_db_config(): ATTACH_CREATE, ATTACH_WRITE, and COMMENTS. (check-in: 325e547a21 ... user: drh tags: trunk) | 
| 12:39 |  | Merge trunk into opfs-sahpool-pause branch. (check-in: 775a547eca ... user: stephan tags: opfs-sahpool-pause) | 
| 12:30 |  | Minor wasm-specific doc touchups. (check-in: 56b618da90 ... user: stephan tags: trunk) | 
| 12:09 |  | Improvements to the SQLITE_DBCONFIG option documentation. (closed check-in: 59f0ba1006 ... user: drh tags: extra-security) | 
| 11:45 |  | Correct a typo in tool/emcc.sh.in which could cause all of the configure-time work to locate the emcc binary to go unused. Reported in forum post feb325cdde5b6f37. (check-in: f66efd5b53 ... user: stephan tags: trunk) | 
| 01:34 |  | Add the SQLITE_DBCONFIG_ENABLE_COMMENTS setting (default on) to enable or disable the ability to include comments in SQL input text. (check-in: 393749a2e2 ... user: drh tags: extra-security) | 
| 00:54 |  | Merge all the latest trunk changes into the extra-security branch. (check-in: 86ba57561a ... user: drh tags: extra-security) | 
| 2025-01-30 |  |  | 
| 21:12 |  | The reuse-subroutine optimization [c9a3498113074bbc] might have generated byte-code that loops forever. This check-in fixes the problem. (check-in: 0cc4ed8c6e ... user: drh tags: trunk) | 
| 17:04 |  | Fix a problem causing the write-lock to be held when it should not be in some circumstances following a SEH exception. (check-in: 7eb5accb7c ... user: dan tags: win32-enable-setlk) | 
| 16:07 |  | Remove an unused parameter from an internal-use subroutine in the TCL interface. (check-in: a700692b39 ... user: drh tags: trunk) | 
| 16:00 |  | Improvements to the TCL interface for Tcl9 as suggested by Jan Nijtmans. (check-in: d76c5db866 ... user: drh tags: trunk) | 
| 15:26 |  | Have sqlite3_enable_setlk(-1) configure indefinite blocking locks where they are supported. (check-in: 62009565d2 ... user: dan tags: win32-enable-setlk) | 
| 13:54 |  | Updates to the compile-for-windows.md document. (check-in: bcd22ed686 ... user: drh tags: trunk) | 
| 12:19 |  | Minor code de-duplication across the top-level auto.def and autoconf/auto.def. (check-in: 3c990d77cb ... user: stephan tags: trunk) | 
| 12:01 |  | Disable a test case that (intentionally) does use-after-free, as it does occasionally cause problems even for non-sanitizer builds. (check-in: 23c7d5bea3 ... user: drh tags: trunk) | 
| 11:19 |  | Fix build regression, introduced in [d2fe6b05f38d9d] (3.48.0), in which SQLITE_OMIT and SQLITE_ENABLE flags passed to configure via CFLAGS were not propagated to the OPT_FEATURE_FLAGS list. Reported in forum post 9801e54665afd728. (check-in: ec71d9dcd5 ... user: stephan tags: trunk) | 
| 2025-01-29 |  |  | 
| 18:53 |  | Fix bug in sessions handling of FK constraints introduced by [e09a0c02] (released in 3.48.0). Bug was preventing a changeset containing FK violations from being applied even when the xConflict(CHANGESET_FOREIGN_KEY) returned OMIT. (check-in: d7c0758120 ... user: dan tags: trunk) | 
| 11:28 |  | Simplify how OpfsSAHPoolUtil.unpauseVfs()'s returned promise is handled. (check-in: d651b8da5a ... user: stephan tags: opfs-sahpool-pause) | 
| 11:08 |  | Add experimental support to pause/unpause an SAHPool OPFS VFS, as discussed in forum post fe8cdb8431c32455, the intent being enable a page to relinquish, perhaps temporarily, the VFS such that the VFS's storage can be accessed by another page/tab. (check-in: 1d2683fe9e ... user: stephan tags: opfs-sahpool-pause) | 
| 2025-01-28 |  |  | 
| 20:32 |  | Enhance the if() and iif() SQL functions so that they support any number of arguments greater than or equal to two. Suggested by forum post 40f7867f75f80. (check-in: fb76d184ee ... user: drh tags: trunk) | 
| 19:03 |  | Fix sessions module handling of tables with generated columns. (check-in: 437fb31638 ... user: dan tags: trunk) | 
| 18:03 |  | Fix a copy/paste typo in the output of vfstrace for xDlClose(). (check-in: 1d57b57c85 ... user: drh tags: trunk) | 
| 12:50 |  | Simplify the IdList object to remove unnecessary fields. Performance increases by about 0.8%. (check-in: a4625bb995 ... user: drh tags: trunk) | 
| 10:56 |  | Fix typo in speedtest.md. (check-in: 984a9cc22c ... user: drh tags: trunk) | 
| 01:10 |  | Apparently I got the logic of [abfe488ed67e2e35] confused, even backwards. Change it so that the SQLITE_USE_W32_FOR_CONSOLE_IO macro causes Win32 APIs to be used for console I/O and for stdio to be used otherwise. This is reported to be necessary for builds that use a C-language runtime other than the one provided by Microsoft. This changes if for Windows only. It is a bug fix, though we don't have a test case that will demonstrate a malfunction. (check-in: 925e97e6f4 ... user: drh tags: trunk) | 
| 00:48 |  | Remove an ALWAYS() in the star-query heuristic that is sometimes false if you have a corrupt database. dbsqlfuzz c37ba7728d79859b79c8341b59297e88fba017d3. Test case in TH3. (check-in: 6b9a339628 ... user: drh tags: trunk) | 
| 2025-01-27 |  |  | 
| 21:18 |  | Use hashing to accelerate column matching on INSERT statements. Code is smaller and about 1.8% faster overall according to test/speedtest.tcl. (check-in: 8be956383e ... user: drh tags: trunk) | 
| 17:48 |  | Enhance ./configure to issue an unambiguious error if the pathname to either the source tree or the build directory contains any space characters. (check-in: 614ae5c74f ... user: drh tags: trunk) | 
| 14:12 |  | Avoid splitting a hyperlink across lines in the documentation comment for sqlite3_serialize(). (check-in: 74b770bd44 ... user: drh tags: trunk) | 
| 11:50 |  | Add the sqlite3_setlk_timeout() API. For setting the timeout used by SQLITE_ENABLE_SETLK_TIMEOUT blocking locks without also setting the regular retry-based busy-timeout. (check-in: 4a7eb49279 ... user: dan tags: win32-enable-setlk) | 
| 2025-01-26 |  |  | 
| 23:34 |  | Make two private routines in FTS "static" so that they are not callable from outside of SQLite itself. (check-in: 9b11bed044 ... user: drh tags: trunk) | 
| 20:09 |  | Further comment improvements in the star-query heuristic. Add an ALWAYS() on an unreachable branch to achieve MC/DC. (check-in: 5e18ce68fb ... user: drh tags: trunk) | 
| 17:29 |  | Small size and complexity reduction on the star-query heuristic. Improved comments for the star-query heuristic. (check-in: a7ecb2f4b7 ... user: drh tags: trunk) | 
| 2025-01-25 |  |  | 
| 23:04 |  | Revise the strategy used by the star-query heuristic: Instead of decreasing the cost of all fact-table WhereLoops, increase the run-cost of WhereLoops that are SCANs of dimension tables. (check-in: 1bc09c9e8b ... user: drh tags: trunk) | 
| 14:30 |  | Improvments to debug output on the star-query heuristic. (check-in: b3ebeb0682 ... user: drh tags: trunk) | 
| 00:07 |  | Avoid calling computeMxChoice() after an OOM as some assert()s can fail in that routine if a prior OOM occurred while building the WhereLoop array. (check-in: 8b9e621dbf ... user: drh tags: trunk) | 
| 2025-01-24 |  |  | 
| 20:59 |  | Remove an incorrect ALWAYS() added in check-in [7cfbe14d199bb631]. (check-in: 431a1a29f9 ... user: drh tags: trunk) | 
| 18:37 |  | Clarify handling of a NULL for the 2nd argument to sqlite3_serialize(), as pointed out in forum post 3df7168b90. (check-in: 340edbe554 ... user: stephan tags: trunk) | 
| 16:37 |  | Improve the star-query heuristic so that it does a better job of identifying actual star queries. Also includes improved diagnostic output from the query planner. (check-in: 7cfbe14d19 ... user: drh tags: trunk) | 
| 16:27 |  | The debugging output for WhereLoop objects now shows cost estimate changes due to the star-query heuristic. (closed check-in: a280f5f548 ... user: drh tags: star-query-heuristic) | 
| 15:55 |  | Minor tweaks to the star-query detection and processing. (check-in: 61f76a45ac ... user: drh tags: star-query-heuristic) | 
| 15:49 |  | Fix a race condition causing SQLite to use a busy-handler for an operation that should not. (check-in: 6ab9ed8eef ... user: dan tags: win32-enable-setlk) | 
| 15:41 |  | Correct a version number in autosetup/README.md. (check-in: c338caf0fd ... user: stephan tags: trunk) | 
| 14:51 |  | When looking for star-queries, do not count a table as a dimension table if that table is separated from the fact table by an OUTER or CROSS join or if the table is a self-join. (check-in: 5aebd7df0d ... user: drh tags: star-query-heuristic) | 
| 14:13 |  | Update autosetup/README.md to reflect recent refactoring. (check-in: 601636829b ... user: stephan tags: trunk) | 
| 12:54 |  | Replace a handful of hard tabs in string literals with \t in mkwasmbuilds.c. (check-in: e8e4d497a7 ... user: stephan tags: trunk) | 
| 09:21 |  | Minor additions to ext/wasm/README.md. (check-in: 85747ea165 ... user: stephan tags: trunk) | 
| 09:18 |  | Update ext/wasm/README.md to reflect simplifications permitted by the current configure script behavior. (check-in: 9abb56c6ac ... user: stephan tags: trunk) | 
| 08:58 |  | Remove extraneous are-we-making-clean guards from the generated wasm build rules, as that guard is set at a higher level. Rename a makefile call()able for consistency. (check-in: 7f9074e2a7 ... user: stephan tags: trunk) | 
| 2025-01-23 |  |  | 
| 22:23 |  | wasm: de-list demo-worker1-promiser-esm.html from the test pages because (per Chrome 131) "Module scripts don't support importScripts()" ~~, where "don't" means "they used to but no longer do"~~ . Edit: this was caused by a build bug, introduced in [0a426a549577b883], which was fixed in [65798c09a00662a3].  (check-in: 4aa025a943 ...  user: stephan tags: trunk) | 
| 21:06 |  | Fix a possible infinity loop in debugging-printf logic in the query planner. No changes to production code. (check-in: 9a20b94080 ... user: drh tags: trunk) | 
| 19:44 |  | When running wasm-opt, ignore any failure because it will fail for unknown flags and the set of legal flags will change from version to version. Document the size-reduction effect (or non-effect) of a dozen-odd wasm-opt flags and retain those which demonstrate a wasm file size reduction in -Oz builds (our production build mode). Total size savings: roughly 13kb. (check-in: c9dc581e02 ... user: stephan tags: trunk) | 
| 16:11 |  | Remove some now-dead makefile code, rename a var for consistency, and fix a recipe bug introduced in the previous checkin in the bundler-friendly build. (check-in: 0acd4ef3ad ... user: stephan tags: trunk) | 
| 15:58 |  | Initial support for post-processing wasm files with wasm-opt (if it's available). This currently shaves 12kb off of the release-mode build of sqlite3.wasm but there are many dozens of wasm-opt flags left to try. (check-in: 5d16e3f283 ... user: stephan tags: trunk) | 
| 14:09 |  | Move small parts of ext/wasm/GNUmakefile into ext/wasm/config.make.in and have the configure script populate that, rather than dynamically determining those values on each 'make' invocation. Add a configure-time check for the optional wasm-opt binary in prep for pending experimentation with using it to reduce the wasm file sizes. (check-in: 0a426a5495 ... user: stephan tags: trunk) | 
| 11:21 |  | Remove unnecessary --minify 0 emcc flag from the wasm build, as -g3 implies that capability along with other anti-minification features we rely on. (check-in: 10c91f9cd0 ... user: stephan tags: trunk) | 
| 06:10 |  | Do not strip binaries during 'make install', for consistency with the legacy build and per request from package maintainers. (check-in: cd3fed5c20 ... user: stephan tags: trunk) | 
| 2025-01-22 |  |  | 
