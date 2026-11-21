---
id: collect-261121-veille-ia/langages-dev/sqlite-4
title: "SQLite"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["check-in", "user", "tags", "trunk", "autoconf", "stephan", "autoconf-to-autosetup", "configure"]
source: https://www.sqlite.org/src/timeline?bt=version-3.47.0&p=trunk&advm=1&udc=1
source_anchor: ""
source_lines: [4501, 5122]
section: "Langages_Dev"
---
| 21:34 |  | Replace the use of the Autools in ./autoconf/ with Autosetup and extend its configure script to include many of the flags available in the canonical build. (check-in: 8c60d4c901 ... user: stephan tags: trunk) | 
| 19:37 |  | Add two new sqlite3_db_config() options that enable the ATTACH command to create new database files and to open databases read/write. Both default to on for backwards compatibility. (check-in: fe0c58d00b ... user: drh tags: extra-security) | 
| 17:18 |  | Add the -q/--quiet option to test/speedtest.tcl. Automatically enable -DSQLITE_OMIT_LOAD_EXTENSION and -DSQLITE_THREADSAFE=0 so that no extra libraries are required. (check-in: c20d21b473 ... user: drh tags: trunk) | 
| 2025-01-21 |  |  | 
| 17:37 |  | Fix date/time computations to deal with the sub-millisecond rounding problem identified in forum post 766a2c9231. (check-in: afb0a5923a ... user: drh tags: trunk) | 
| 16:30 |  | Performance improvements to the (debug-use only) Mem.pScopyFrom logic, resulting in about 8x faster performance under -DSQLITE_DEBUG for the query in from forum post 0025389d0860af82. This change only affects builds that use -DSQLITE_DEBUG. (check-in: 7fb1ae25d1 ... user: drh tags: trunk) | 
| 15:45 |  | Explain the lack of docs in autoconf/Makefile.in and refer interested readers to main.mk in canonical tree. Minor cleanups and fixes in tool/mkautoconfamal.sh. (closed check-in: e5bbc2428f ... user: stephan tags: autoconf-to-autosetup) | 
| 15:12 |  | The FuncDev.nArg field values -3 and -4 now have special meansing of 1 or more or 2 or more arguments, respectively. This saves space in the built-in function table, resulting in slightly faster performance and a reduced binary size. (check-in: 753fd747f2 ... user: drh tags: trunk) | 
| 14:40 |  | Remove now-extraneous autoconf/INSTALL and autoconf/configure.ac and update autoconf/README.* to account for the port to autosetup. (check-in: 646667af8c ... user: stephan tags: autoconf-to-autosetup) | 
| 14:34 |  | Have fts5 better handle OOM errors from sqlite3_blob_close(). (check-in: f418350f3f ... user: dan tags: trunk) | 
| 11:10 |  | Use Tcl_GetString() instead of Tcl_GetCharLength() to test for a zero-length string in the TCL interface, since that is much more efficient. (check-in: a8d9dcfd23 ... user: drh tags: trunk) | 
| 2025-01-20 |  |  | 
| 20:48 |  | configure script: work around msys's inability to 'exec' a '.sh' file by prefixing the call with an explicit 'sh', as reported in forum post befb352a42a7cd6d. (check-in: 25b01f1c9a ... user: stephan tags: autoconf-to-autosetup) | 
| 19:57 |  | Add 'dist' and missing 'install' pieces to the autoconf bundle. (check-in: 16fa20413e ... user: stephan tags: autoconf-to-autosetup) | 
| 19:19 |  | Remove an assert() in the unix file locking logic that is not true if alternative VFS "unix-excl" is used for a read-only connection. (check-in: bd5dc92368 ... user: drh tags: trunk) | 
| 18:26 |  | Fix an assert() that could fail if a virtual table called sqlite3_step() from within the xSync() method while committing a "PRAGMA defer_foreign_keys=1" transaction. (check-in: 39bdbb3f6d ... user: dan tags: trunk) | 
| 16:14 |  | configure script: remove an overzealous is-a-file check in the tclsh search which fails to account for implicit .exe extensions on Windows builds. Reported in forum post c27403ef974df9f1. (Same change as [89306d1a4905] but to a different file, as that content was moved since the 3.48 release.) (check-in: 239a3d1573 ... user: stephan tags: trunk) | 
| 2025-01-19 |  |  | 
| 21:47 |  | Get libsqlite3.so and .a building in the autoconf bundle. (check-in: 88cee3fc7c ... user: stephan tags: autoconf-to-autosetup) | 
| 19:51 |  | Get the CLI shell building in the autoconf bundle. (check-in: 241cdbc40a ... user: stephan tags: autoconf-to-autosetup) | 
| 19:14 |  | Add an SQLITE_TESTCTRL_OPTIMIZATION mask that can disable the query planner heuristics that are designed to help with star queries. (check-in: fec4ff185a ... user: drh tags: trunk) | 
| 18:58 |  | Initial work towards converting the autoconf bundle to autosetup. Currently non-functional. (check-in: d7708372ff ... user: stephan tags: autoconf-to-autosetup) | 
| 18:32 |  | Further refactoring of auto.def to simplify creation of variant builds like the autoconf bundle. (check-in: f806c563a2 ... user: stephan tags: trunk) | 
| 16:26 |  | Minor internal refactoring of auto.def to support the pending autoconf subdir port to autosetup. No functional changes. (check-in: 01ff375847 ... user: stephan tags: trunk) | 
| 14:53 |  | Move some of the auto.def functions into autosetup/sqlite-config.tcl for re-use in the pending migration of the autoconf bundle to autosetup. This is just reorg, no functional changes. (check-in: 2f1e94994f ... user: stephan tags: trunk) | 
| 2025-01-18 |  |  | 
| 23:44 |  | Enable automatic query-time indexes for WITHOUT ROWID tables. (check-in: 8534af5b94 ... user: drh tags: trunk) | 
| 21:19 |  | Add a simple test case (many more are coming soon to TH3). Fix an obsolete assert(). (closed check-in: aa829a131a ... user: drh tags: without-rowid-autoidx) | 
| 21:00 |  | Add support for automatic query-time indexes on WITHOUT ROWID tables. (check-in: 89c4cbd952 ... user: drh tags: without-rowid-autoidx) | 
| 16:05 |  | When cross-compiling, change the CFLAGS default to exclude -g, per /chat discussion prompted by forum post 9a67df63eda9925c. (check-in: 4309da2ca5 ... user: stephan tags: trunk) | 
| 13:51 |  | When running make install, avoid the -s (strip) flag when cross-compiling, as it only works for the build platform. Reported in forum post 9a67df63eda9925c. Also remove the undocumented and unused --soname=auto option. (check-in: 230e49c10e ... user: stephan tags: trunk) | 
| 2025-01-17 |  |  | 
| 23:49 |  | For the purpose of the query planner heuristic added by [38db9b5c83], a query should only count as a star query if the fact tables are connected to the dimension table by an INNER JOIN. If a LEFT JOIN is used, then the fact tables are constrained to be in inner loops anyhow and so the heuristic does not make any sense. But it does interfere with AUTOMATIC index creation, which causes the performance regression reported by forum post d87570a1455. (check-in: 0852c57ee2 ... user: drh tags: trunk) | 
| 17:09 |  | Enhance a comment on a test case to add recent context. (check-in: c2647d1bd1 ... user: drh tags: trunk) | 
| 15:56 |  | Typo fix in speedtest.md. (check-in: 36027cf340 ... user: stephan tags: trunk) | 
| 15:39 |  | Improvements to the speedtest.tcl script. Add documentation on how to use the script. (check-in: eb3853d081 ... user: drh tags: trunk) | 
| 12:32 |  | Add the test/speedtest.tcl script to simplify performance and size testing. (check-in: ad7b386726 ... user: drh tags: trunk) | 
| 10:39 |  | Fix a potential one-byte buffer overrun when reading from the Windows console in the CLI. Forum post 95e17b8f5c. This problem was introduced by check-in [abfe488ed67e2e35], which was an attempt to get the Windows build working on MingGW. (check-in: 4d96759694 ... user: drh tags: trunk) | 
| 2025-01-16 |  |  | 
| 20:46 |  | speedtest1: further improvements to the "json" testset. (check-in: c4750f7cf7 ... user: drh tags: trunk) | 
| 20:08 |  | speedtest1: Improvements to the "json" testset. Better balance in "mix1". (check-in: 2cc73c5e08 ... user: drh tags: trunk) | 
| 17:19 |  | Revamp the "json" testset in speedtest1. (check-in: d33508836b ... user: drh tags: trunk) | 
| 14:37 |  | Add the "json" test case to speedtest1 and include it in "mix1". (check-in: 2db3a3ee37 ... user: drh tags: trunk) | 
| 14:06 |  | Speedtest1 enhancements: (1) Add the ability to scale performance of tests sets using "/NNN" after the testset name, even for testsets in a comma-separated list. (2) Add the "mix1" macro testset. (check-in: 85667b3a57 ... user: drh tags: trunk) | 
| 13:10 |  | wasm: after generating the .js/.mjs file, strip out all of the generated pieces which create Emscripten call() bindings for the sqlite3 APIs, as we don't use those binding, so both the setup time and the memory they use installing WASM proxy bindings is wasted. This eliminates some 200 superfluous/unused bindings from the init process. (check-in: 020d2c7528 ... user: stephan tags: trunk) | 
| 12:15 |  | When generating the autoconf snapshot, replaces the Libs.private line from sqlite3.pc.in with one compatible with the legacy build, as reported in forum post e40b9b424a. (check-in: fe47154799 ... user: stephan tags: trunk) | 
| 09:27 |  | Replace use of Emscripten's Module.postRun() with a custom callback so that we get consistent library init timing with both Emscripten 3.1.x and 4.0.x. Details and discussion are in Emscripten ticket #23420. (check-in: 4863a70ac6 ... user: stephan tags: trunk) | 
| 01:47 |  | When two indexes have the same cost, use the narrower one (the one with the smaller average on-disk row width). (check-in: 398559678f ... user: drh tags: trunk) | 
| 2025-01-15 |  |  | 
| 21:13 |  | Check-in [da9124fee28c155c] broken the new SQLITE_IOCAP_SUBPAGE_READ mechanism for inhibiting direct-overflow-read. This check-in fixes the problem. (check-in: 113078d555 ... user: drh tags: trunk) | 
| 21:09 |  | Teach tool/stripccomments.c that a backslash immediately preceeding a forward slash means that that forward slash cannot be the start of a comment. This is intended to allow JavaScript regular expressions containing \/* (like the one introduced in emsdk 4.0.0) to pass through properly. Re-enable comment-stripping in the JS dist builds. (check-in: db21d6cc9d ... user: stephan tags: trunk) | 
| 20:53 |  | Work around a behavior change in emsdk 4.0.0 which breaks the load/init timing of the wasm module. (check-in: 00a750184b ... user: stephan tags: trunk) | 
| 20:23 |  | When choosing between two indexes with the same cost, pick the one with the smaller predicted number of bytes per row. (closed check-in: d4bd0d4214 ... user: drh tags: consider-idx-width) | 
| 19:30 |  | Variable names and conditional logic simplified in where.c. These changes are cosmetic only and do not affect the resuling machine code. (check-in: dbc2d6a244 ... user: drh tags: trunk) | 
| 15:27 |  | Fix a typo in a wasm makefile var name, noting that this doesn't actually fix anything because the var in question is not used when building 'clean' or 'distclean'. (check-in: 7cfc75a625 ... user: stephan tags: trunk) | 
| 14:28 |  | Disable the C-style comment stripper in the JS dist build, as explained in forum post 529c20d344. (check-in: a9475e7614 ... user: stephan tags: trunk) | 
| 12:45 |  | Have windows SQLITE_ENABLE_SETLK_TIMEOUT builds block indefinitely if the busy-timeout is set to 0x7FFFFFFF. (check-in: daefcafe79 ... user: dan tags: win32-enable-setlk) | 
| 2025-01-14 |  |  | 
| 20:47 |  | Change the version number to 3.49.0 to begin the next development cycle. Any patches to 3.48.0 will go on a branch. (check-in: 8165a3d3a4 ... user: drh tags: trunk) | 
| 16:10 |  | Trying to remove a warning from some compiler that I do not have access to. (check-in: bc6de90c70 ... user: drh tags: trunk) | 
| 11:05 |  | Version 3.48.0 (check-in: d2fe6b05f3 ... user: drh tags: trunk, release, major-release, version-3.48.0) | 
| 2025-01-13 |  |  | 
| 13:32 |  | Remove a stray tab character from a comment. (check-in: 315079b150 ... user: drh tags: trunk) | 
| 11:28 |  | GCC 13 has become more quite pedantic about the signature of functions matching the type of pointers through which the functions are called. Make adjustments to extension functions and test procedures to work around this. No changes to the core. (check-in: ed83b79100 ... user: drh tags: trunk) | 
| 2025-01-11 |  |  | 
| 16:28 |  | Fix harmless "implicit fall through" warnings that suddenly appeared when I upgraded to gcc-13. (check-in: 3e2875dac2 ... user: drh tags: trunk) | 
| 14:43 |  | Better job at suppressing harmless scan-build warnings. This time testing and working. (check-in: c847973947 ... user: drh tags: trunk) | 
| 13:59 |  | Fix harmless scan-build warnings. Actually, this fixed nothing. The scan-build warnings persist. I should have tested before I checked in. Look for a follow-up shortly. (check-in: b93af6feb7 ... user: drh tags: trunk) | 
| 09:02 |  | Fix an age-old bug in the lower-level wasm/js helper bits which (A) caused removal of customized WASM func arguments/results conversion to silently fail and (B) triggered a legitimate warning in the Google closure toolchain. Reported in jaccwabyt ticket c5c296e85a7c01360820. (check-in: 99917a5bb0 ... user: stephan tags: trunk) | 
| 2025-01-09 |  |  | 
| 19:57 |  | Update the makefile so that it puts the amalgamation files in a subdirectory in the amalgamation-zip. Forum post b8dd1941e1. (check-in: cef8e88bed ... user: drh tags: trunk) | 
| 14:10 |  | When removing an fts5 in secure-delete mode, defer setting the table version to SECUREDELETE until flushing data to disk. This prevents problems that can occur if there is a rollback or statement rollback operation. (check-in: c359e555ce ... user: dan tags: trunk) | 
| 2025-01-08 |  |  | 
| 20:43 |  | Fix another assert() failure in fts5. (check-in: 6da37893f5 ... user: dan tags: trunk) | 
| 15:54 |  | Fix a crash in fts5 that could occur if shadow tables are modified or removed. (check-in: c0b691095a ... user: dan tags: trunk) | 
| 12:51 |  | Cross-link and otherwise improve the various how-to-compile documents. No code changes. (check-in: 5e6ede92af ... user: drh tags: trunk) | 
| 12:25 |  | In the doc/tcl-extension-testing.md document, provide enhanced explanation for why the tcl library needs to be copied into the install directory. (check-in: cff70c859f ... user: drh tags: trunk) | 
| 2025-01-07 |  |  | 
| 18:50 |  | Further refinement to the Tcl extension testing procedure. (check-in: 32b8b078d1 ... user: drh tags: trunk) | 
| 16:36 |  | Improvements to [14b38ae6ab86a314] so that the Tcl interface is better able to work with boolean values in both Tcl86 and Tcl90. (check-in: 4e85343d61 ... user: drh tags: trunk) | 
| 15:39 |  | Fix a memory error in test logic introduced by [8704034254938662]. (check-in: 8a56e98d25 ... user: drh tags: trunk) | 
| 15:00 |  | Typo in the previous check-in. (check-in: 5872d7a0a7 ... user: drh tags: trunk) | 
| 14:55 |  | Enhancements to the Tcl SQLite extension testing procedures so that they install a full-featured SQLite and so that Tcl is build statically so that there is no need to worry with LD_LIBRARY_PATH. (check-in: c8972e652e ... user: drh tags: trunk) | 
| 12:14 |  | Make the TCL extension aware of the booleanString type within TCL. (check-in: 14b38ae6ab ... user: drh tags: trunk) | 
| 11:54 |  | Fix a typo in the previous check-in. (check-in: 7d41885e85 ... user: drh tags: trunk) | 
| 00:17 |  | Minor fixes to tclsqlite.c to promote portability. (check-in: dd934f032f ... user: drh tags: trunk) | 
| 2025-01-06 |  |  | 
| 23:33 |  | Enhance the makefile to make it easier to build from a read-only source tree. (check-in: f99a70eca2 ... user: drh tags: trunk) | 
| 21:36 |  | Fix a problem with tool/mksqlite3h.tcl that prevents it from running out of a read-only check-out. (check-in: cb54f0063e ... user: drh tags: trunk) | 
| 18:32 |  | Fix a minor problem with the sqlite3_get_table_printf() test interface. No core changes. (check-in: a0df29c7a3 ... user: drh tags: trunk) | 
| 17:01 |  | Add test case for using both SQLITE_CHANGESETAPPLY_IGNORENOOP and SQLITE_CHANGESETAPPLY_FKNOACTION. (check-in: b1cc53fa3f ... user: dan tags: trunk) | 
| 2025-01-05 |  |  | 
| 19:58 |  | Further refactoring of the TCL extension test procedure document, for improved clarity and usability. (check-in: bcdaef4341 ... user: drh tags: trunk) | 
| 17:16 |  | Refactor the TCL extension test procedure to deal with TCL8.6 and TCL9.0 separately, to simplify the procedures and reduce cognative stress on the tester. (check-in: 5281536327 ... user: drh tags: trunk) | 
| 11:40 |  | Fix typo in the tcl-extension-testing.md document. (check-in: 28150c615c ... user: drh tags: trunk) | 
| 11:19 |  | Improvements to TCL extension test procedure description. Improvements to the tclextension-list and tclextension-verify makefile targets to suppress unnecessary output. (check-in: 3e92fea09a ... user: drh tags: trunk) | 
| 2025-01-04 |  |  | 
| 20:50 |  | Improvements to testing and validation of the SQLite TCL extension. (check-in: 9d7597cad4 ... user: drh tags: trunk) | 
| 20:13 |  | Show the values of $(JIM_TCLSH) and $(VISUALSTUDIOVERSION) in the output of "nmake tcl-env". (check-in: 4270abc071 ... user: drh tags: trunk) | 
| 19:50 |  | Allow the 2nd argument to ".param set" to use previously bound parameters, as suggested by forum post 823e1bd746. (check-in: 13a35ad792 ... user: drh tags: trunk) | 
| 16:30 |  | Fix a problem in the sessions extension allowing changesets containing foreign key violations to be committed under some circumstances. (check-in: e09a0c0229 ... user: dan tags: trunk) | 
| 15:52 |  | Add the document describing test procedures for the TCL extension build process. Update the Windows makefile so that it builds the TCL extensions successfully with a default installation of Tcl8.6. (check-in: 3263db9249 ... user: drh tags: trunk) | 
| 15:51 |  | Adjust the Windows Makefile.msc so that it can build the tclextension with Tcl8.6 successfully. Updates to the tcl-extension test procedure document. (closed check-in: 0c2cdc632f ... user: drh tags: test-procedures) | 
| 14:10 |  | Add new tcl-extension-testing.md document. The Windows side is not yet working. (check-in: 9dc805df1b ... user: drh tags: test-procedures) | 
| 2025-01-03 |  |  | 
| 11:51 |  | Add comment to the columnIsGoodIndexCandidate() routine to record the results of a failed experiment. No changes to code. (check-in: 9ee57a30a4 ... user: drh tags: trunk) | 
| 11:22 |  | Avoid using Int32x32To64() with a 64-bit argument in fileio.c - this level of micro-optimization is not really necessary there. (check-in: 1291b013a8 ... user: dan tags: trunk) | 
| 2025-01-02 |  |  | 
| 21:23 |  | Improve the treeview output for CteUse objects. (check-in: 2b16d6947c ... user: drh tags: trunk) | 
| 18:43 |  | Improvements to the display of subqueries in the FROM clause for treeview output. (Debug and analysis code only - does not affect production builds.) (check-in: 4a2d65cdcd ... user: drh tags: trunk) | 
| 17:56 |  | Close database connections in test/dbpage.test, for Windows. (check-in: 322d255ed8 ... user: drh tags: trunk) | 
| 15:39 |  | Fix recent test cases so that they work even when auto_vacuum defaults to on. (check-in: 41f6e46695 ... user: drh tags: trunk) | 
| 15:27 |  | Add a test case for ROLLBACK TO of database truncate operations made through the sqlite_dbpage vtab. (check-in: eb335beb1e ... user: dan tags: trunk) | 
| 15:03 |  | Improvements to the way that truncation is implemented in sqlite_dbpage(). (check-in: ac4bb2e4ec ... user: drh tags: trunk) | 
| 12:14 |  | Update the build instructions for Windows to note that VS2015 or later is required to avoid the need to install tclsh.exe. (check-in: da0ef0567b ... user: drh tags: trunk) | 
| 2025-01-01 |  |  | 
| 18:18 |  | Fix the tool/omittest.tcl script, broken by [d8c0e0184226bdae]. (check-in: 4f6c36a61c ... user: drh tags: trunk) | 
| 12:24 |  | Fix the vfstrace.c extension so that it supports xFetch and xUnfetch. (check-in: c7132b7e62 ... user: drh tags: trunk) | 
| 2024-12-30 |  |  | 
| 21:23 |  | Add the convenience makefile target (unix-only) "src-archives" that builds the various tarballs and ZIP archives that go on the download page. This is intended to make it easier and less error prone to put up new "draft" download pages for testing. (check-in: 2b17bc4965 ... user: drh tags: trunk) | 
| 13:54 |  | New assert() statements to show that the sqlite3_value.db field is initialized for MemArrays. (check-in: 7cd8ccf57d ... user: drh tags: trunk) | 
| 12:29 |  | Add an extra assert() to releaseMemArray() just to prove that the sqlite3_value.db field is never NULL. (check-in: b969ef1def ... user: drh tags: trunk) | 
| 2024-12-29 |  |  | 
| 11:54 |  | Four new assert() statements to help with static analysis. (check-in: e7f7c9d22b ... user: drh tags: trunk) | 
| 2024-12-28 |  |  | 
| 13:04 |  | Show ETC in fuzzcheck with the --spinner option when there is only one input file. (check-in: 809699aeaa ... user: drh tags: trunk) | 
| 12:32 |  | Fixes to the substr() SQL function so that it can handle ridiculously large numbers in its 2nd and 3rd arguments without signed integer overflows. (check-in: c1de8f916e ... user: drh tags: trunk) | 
| 2024-12-26 |  |  | 
| 16:10 |  | Omit code to link all winShm objects open on a single node into a list. This is no longer required. (check-in: b400ab4ba9 ... user: dan tags: win32-enable-setlk) | 
| 14:40 |  | Merge trunk changes into this branch. (check-in: 3e2893097c ... user: dan tags: win32-enable-setlk) | 
| 2024-12-24 |  |  | 
| 14:44 |  | Properly wait for asynchronous results for calls to LockFileEx() on FILE_FLAG_OVERLAPPED files even if LOCKFILE_FAIL_IMMEDIATELY, which usually causes LockFileEx() to return synchronously, is specified. (check-in: 35b3e73c5a ... user: dan tags: win32-enable-setlk) | 
| 2024-12-22 |  |  | 
| 21:17 |  | In the (debugging) rtreenode() function, do not override an error coming out of sqlite3_result_text(). (check-in: 286559dfb3 ... user: drh tags: trunk) | 
| 2024-12-19 |  |  | 
| 20:29 |  | Fix a test case in sqllimits1.test so that it works with the Apple configuration which changes the default SQLITE_MAX_LENGTH. (check-in: 536fff14ac ... user: drh tags: trunk) | 
| 19:52 |  | Correction to check-in [a9759fc78d6cb0df] - printf() parameters values must be integers. (check-in: 2db531d191 ... user: drh tags: trunk) | 
| 19:02 |  | The BTree mutex must be held when calling sqlite3BtreeLastPage(). This check-in fixes a bug introduced by [cf8b99e17872c054]. (check-in: e6c30ee52c ... user: drh tags: trunk) | 
| 14:20 |  | Fix the sort4.test module so that the first two test cases are omitted when SQLite has been compiled using SQLITE_MAX_WORKER_THREADS=0. (check-in: 5b96dcf5f6 ... user: drh tags: trunk) | 
| 14:09 |  | configure script: only set the SQLITE_TEMP_STORE feature flag if --with-tempstore is explicitly set, to avoid colliding with that flag being set by other means via the test fixture scripts. (check-in: c7839b8097 ... user: stephan tags: trunk) | 
| 14:08 |  | Fix the Microsoft makefile so that it does not set SQLITE_TEMP_STORE unnecessarily. (check-in: f9b92f9513 ... user: drh tags: trunk) | 
| 13:36 |  | Enhance lemon so that it accepts the -U command-line option that undefines a preprocessor macro. (check-in: e2188a3edf ... user: drh tags: trunk) | 
| 12:08 |  | Fix additional integer overflow problems in the substr() function. (check-in: 472abb492f ... user: drh tags: trunk) | 
| 2024-12-18 |  |  | 
| 20:29 |  | Fix possible integer oveflow in the second and third argument to substr(). (check-in: b04b4006f3 ... user: drh tags: trunk) | 
| 18:29 |  | Avoid 32-bit roundoff error on the second argument to round(). Forum post 170aeab92a. (check-in: a9759fc78d ... user: drh tags: trunk) | 
| 03:41 |  | Rename some var refs in ext/lsm1/Makefile for the new build process. (check-in: 0ce42fa586 ... user: stephan tags: trunk) | 
| 2024-12-17 |  |  | 
| 14:32 |  | Do not attempt to truncate a database in sqlite_dbpage if the database is not larger than the requested truncation size. (check-in: cf8b99e178 ... user: drh tags: trunk) | 
| 2024-12-16 |  |  | 
| 19:31 |  | Fix release test errors on winrt. (check-in: 39bebd50b8 ... user: dan tags: win32-enable-setlk) | 
| 18:04 |  | Code formatting changes to make trunk more like wal2. (check-in: 8f725472b0 ... user: drh tags: trunk) | 
| 15:13 |  | Fix a problem on this branch causing a file-handle to be leaked following an OOM condition. (check-in: 5ca12dc964 ... user: dan tags: win32-enable-setlk) | 
| 13:25 |  | Changes for better alignment with the wal2 branch. (check-in: 4fab85b3d0 ... user: drh tags: trunk) | 
| 12:08 |  | Remove unnecessary end-of-line whitespace. (check-in: ae580443d2 ... user: drh tags: trunk) | 
| 07:03 |  | Tiny makefile/configure script doc tweaks. No functional changes. (check-in: ed5e912622 ... user: stephan tags: trunk) | 
| 2024-12-13 |  |  | 
| 01:29 |  | Fix a harmless compiler warning that comes up when using SQLITE_DEBUG in separate compilation mode. (check-in: 52e0f8cab9 ... user: drh tags: trunk) | 
| 2024-12-12 |  |  | 
| 20:59 |  | Fix a buffer overread in test code. Add test configurations for ENABLE_SETLK=1 and FOR_WINRT=1 builds to release testing. (check-in: b558db5db7 ... user: dan tags: win32-enable-setlk) | 
| 15:11 |  | Increase the maximum number of arguments on an SQL function to 1000 with the capability to increase it further up to 32767 using a compile-time option. (check-in: e8d7d68ba0 ... user: drh tags: trunk) | 
| 2024-12-11 |  |  | 
| 15:30 |  | Ensure that all win32 API functions invocations in os_win.c are made via the function table. Fix asan error in test code. (check-in: 2ffdc799c6 ... user: dan tags: win32-enable-setlk) | 
| 2024-12-10 |  |  | 
| 21:24 |  | Fix a problem with walsetlk.test on windows. (check-in: 704e6567c1 ... user: dan tags: win32-enable-setlk) | 
| 19:00 |  | Ensure windows may block on a SHARED (but not EXCLUSIVE) DMS lock. (check-in: 55e4e7bb00 ... user: dan tags: win32-enable-setlk) | 
| 18:12 |  | Fix walsetlk.test to work on unix. (check-in: 798dff041b ... user: dan tags: win32-enable-setlk) | 
| 17:36 |  | Merge latest trunk changes into this branch. (check-in: d67a42d625 ... user: dan tags: win32-enable-setlk) | 
| 12:32 |  | Fix harmless typo in a comment describing the columnMallocFailure() function. (check-in: 0f3b484fd7 ... user: drh tags: trunk) | 
| 2024-12-09 |  |  | 
| 20:46 |  | Make the TCL interface more rebust against very large strings coming out of TCL9. (check-in: e2bae4143a ... user: drh tags: trunk) | 
| 20:37 |  | Enhance the ".import" command of the CLI so that it is able to insert into a view that has an instead-of trigger. Forum post 3e03c73150f8b9f8. (check-in: 7dcc3731a9 ... user: drh tags: trunk) | 
| 20:36 |  | Enhance the ".import" command of the CLI so that it is able to insert into a view that has an instead-of trigger. (Originally checked into the wrong branch.) (check-in: c71acee1cf ... user: drh tags: commit-and-continue) | 
| 17:30 |  | Test the case where a db connection blocks for a while and then successfully obtains the lock. (check-in: 9cbc97ff84 ... user: dan tags: win32-enable-setlk) | 
| 16:01 |  | Enhance tests to ensure that blocking locks, and not calls to xSleep(), are being used by SQLITE_ENABLE_SETLK_TIMEOUT builds when they should be. (check-in: c892242bb6 ... user: dan tags: win32-enable-setlk) | 
| 13:02 |  | Add NEVER() around branches that seems to have been made obsolete by [a350ea7c6b89725b]. (check-in: cb5bad5c74 ... user: drh tags: trunk) | 
| 11:47 |  | Further improvements to the decimal-to-float conversion for values close to LARGEST_UINT64. (check-in: 453c949914 ... user: drh tags: trunk) | 
| 11:37 |  | Fix an obscure problem with multiple outer joins, ON clauses and query flattening. Forum thread 5c8a069d23. (check-in: a350ea7c6b ... user: dan tags: trunk) | 
| 11:12 |  | Fix an obscure problem with multiple outer joins, ON clauses and query flattening. Forum thread 5c8a069d23. (closed check-in: 289daf6cee ... user: dan tags: forum-5c8a069d23-fix) | 
| 10:57 |  | Add a note in Makefile.msc about EXTRA_SRC files possibly requiring manual editing. (check-in: a0a36bad45 ... user: drh tags: trunk) | 
| 10:52 |  | Resynchronize autoconf/Makefile.msc (closed check-in: 8f9c640818 ... user: drh tags: makefile-fix) | 
| 2024-12-08 |  |  | 
| 18:38 |  | Add a note in Makefile.msc about EXTRA_SRC files possibly requiring manual editing, as discussed in forum thread 903f721f3e7c0d25. (check-in: 229c2f013c ... user: stephan tags: makefile-fix) | 
| 2024-12-07 |  |  | 
| 19:57 |  | Yet another iteration of the solution to the floating-point conversion problem - this what avoids complaints about oversize double values from -fsanitize. (check-in: fc6904a508 ... user: drh tags: trunk) | 
| 19:06 |  | A cleaner and more robust solution to the floating-point conversion problem originally fixed by [81342fa6dd03fffb]. (check-in: 351de57f80 ... user: drh tags: trunk) | 
| 17:08 |  | Fix more harmless compiler warnings. (check-in: f5b8fd7763 ... user: drh tags: trunk) | 
| 16:53 |  | Fix harmless compiler warning caused by the previous check-in. (check-in: 462700aeb7 ... user: drh tags: trunk) | 
| 14:48 |  | On x64 hardware, round-trip uint64_t→double→uint64_t conversions fail for values greater than UINT64_MAX-2047. This caused the SQLite text-to-float converter routine to give incorrect results for values between '1.8446744073709550592eNNN' and '1.8446744073709551609eNNN' for any exponent NNN. This problem was introduced by check-in [761d8fd18b0ee868] and first appeared in version 3.47.0 and was reported by forum post 569a7209179a7f5e. Fixed by this... (check-in: 81342fa6dd ... user: drh tags: trunk) | 
| 2024-12-06 |  |  | 
| 18:35 |  | Add the SQLITE_PREPARE_DONT_LOG option for sqlite3_prepare_v3(), that prevents errors in the compilation of the SQL from being sent to sqlite3_log(). (check-in: 8704034254 ... user: drh tags: trunk) | 
| 00:12 |  | Minor doc update in auto.def. No functional changes. (check-in: d324be296d ... user: stephan tags: trunk) | 
| 00:09 |  | Ensure that the post-config checks for @UNEXPANDED_VARS@ pass even if --disable-tcl is used, as reported in forum post 74ef8059fc. (check-in: a38606bf44 ... user: stephan tags: trunk) | 
| 2024-12-05 |  |  | 
| 23:53 |  | Fix the build of sqlite3_analyzer.exe on Windows that was broken by check-in [80f3bf8c2ee31ba1]. (check-in: 223f47b2db ... user: drh tags: trunk) | 
| 2024-12-04 |  |  | 
| 19:32 |  | Fix filectrl.test so that it works with winrt builds. (check-in: 70b0cfb049 ... user: dan tags: win32-enable-setlk) | 
| 18:27 |  | Assume an NT-based OS when running tests if sqlite_os_type is not set. (check-in: 22e4aeded0 ... user: dan tags: win32-enable-setlk) | 
| 16:01 |  | Minor doc correction in ext/misc/shathree.c, as reported in the forum. No functional changes. (check-in: 3b82d2c6b7 ... user: stephan tags: trunk) | 
| 2024-12-03 |  |  | 
| 19:41 |  | Back out minor changes not required by this branch. (check-in: 68c34ffd23 ... user: dan tags: win32-enable-setlk) | 
| 19:30 |  | Merge latest trunk into this branch. (check-in: eb13b8b3ef ... user: dan tags: win32-enable-setlk) | 
| 19:09 |  | Further test file tweaks to run with SQLITE_OS_WINRT builds. (check-in: 54853421ba ... user: dan tags: win32-enable-setlk) | 
| 15:19 |  | Fix the lock_win32_file test command so that it works with winrt builds. (check-in: 5994656074 ... user: dan tags: win32-enable-setlk) | 
| 2024-12-02 |  |  | 
| 20:48 |  | Fix a problem with SQLITE_OS_WINRT builds. (check-in: baf43e8c7d ... user: dan tags: win32-enable-setlk) | 
| 19:55 |  | Improve the output from the ".testctrl opt" command in the CLI. (check-in: de7064d118 ... user: drh tags: trunk) | 
| 17:21 |  | Ensure that the query flattener does not change an ON clause term to a WHERE clause term. Forum post 3f676b1196. Follow-up to [f1eae192315335d7]. (check-in: bdd408a255 ... user: dan tags: trunk) | 
| 16:24 |  | Fix a comment typo on the sqlite3ExprIsSingleTableConstraint() routine. (check-in: 346a845bf1 ... user: drh tags: trunk) | 
| 16:07 |  | Fix a harmless typo in debugging output. (check-in: 578f76cfb2 ... user: drh tags: trunk) | 
| 14:14 |  | Reformulate [9e09d5d6] so that memsys5 shows up in the late-config summary as being enabled and does the right thing if both --debug and --memsys3 are used. (check-in: 447db1cd0a ... user: stephan tags: trunk) | 
| 13:47 |  | Enable MEMSYS5 with the --dev configure option. (check-in: 9e09d5d60a ... user: drh tags: trunk) | 
| 13:29 |  | Slighly less confusing output from treeview during debugging. (check-in: 7aef0b9305 ... user: drh tags: trunk) | 
| 2024-12-01 |  |  | 
| 15:23 |  | Trim a bit more from the sqlite370.svg border to get it closer in scaled size to the logo currently on the docsrc site. (check-in: 81c7277fc5 ... user: stephan tags: trunk) | 
| 14:20 |  | Correct the fill color for the background of sqlite370.svg - it was slightly darker than it should have been. (check-in: 732132407b ... user: stephan tags: trunk) | 
| 14:14 |  | Re-export sqlite370.svg to retain the gradient part and clip the size using the cutting guides in sqlite370.eps. (check-in: 12017b01c8 ... user: stephan tags: trunk) | 
| 14:02 |  | Export a clipped copy of sqlite370.eps as sqlite370.svg, based on discussion in forum post 1bbd6d17ca. (check-in: dd3a13c120 ... user: stephan tags: trunk) | 
| 2024-11-30 |  |  | 
| 20:00 |  | On windows, use a separate handle for each connection for xShmLock() locks. (check-in: 272d552f12 ... user: dan tags: win32-enable-setlk) | 
| 17:48 |  | At the end of the configure script ensure that none of the files which are filtered for @VARS@ contain any unresolved @VAR@ placeholders, failing fatally if any do. (check-in: 301df5c2be ... user: stephan tags: trunk) | 
| 14:13 |  | In sqlite_dbpage, cancel any pending truncate operation if there an error occurs later in the transaction. (check-in: 1abab10f85 ... user: drh tags: trunk) | 
| 12:00 |  | A NEVER() that was added by [eb5ac9e5b9a4f9c8] is violated by the ifnull() in-line function. This check-in fixes that problem. (check-in: 2220ccf4d6 ... user: drh tags: trunk) | 
| 2024-11-29 |  |  | 
| 11:49 |  | Fix a NEVER() in the iif() logic that can be true if compiled with SQLITE_ENABLE_UNKNOWN_SQL_FUNCTIONS. Problem introduced by [eb5ac9e5b9a4f9c8]. (check-in: 3ec2df5a6c ... user: drh tags: trunk) | 
| 2024-11-28 |  |  | 
| 23:55 |  | Remove some outdated docs from Makefile.in. (check-in: cc2c5fc98b ... user: stephan tags: trunk) | 
| 20:46 |  | Typo fixes and cleanups in autosetup/README.md. (check-in: 7f366565f4 ... user: stephan tags: trunk) | 
| 16:14 |  | Configure script doc updates and cleanups. Skip check for EMSDK when doing an out-of-tree build, as ext/wasm does not support that build mode. (check-in: 9d2f4148db ... user: stephan tags: trunk) | 
| 15:52 |  | ext/icu/README.txt: clean up EOL whitespace and add a mention of the --with-icu-... configure flags available as of version 3.48. (check-in: 4976ac717b ... user: stephan tags: trunk) | 
| 11:52 |  | Improvements to sqlite3ExprImpliesExpr() so that it recognizes that expressions like "iif(X,Y)" and "CASE WHEN X THEN Y END" imply X. (check-in: eb5ac9e5b9 ... user: drh tags: trunk) | 
| 02:09 |  | Simplification to the fix in the prior check-in. (closed check-in: f79cb748c9 ... user: drh tags: expr-implies-expr) | 
| 01:47 |  | The sqlite3ExprCompare() routine should always compare the same variables as equal to one another, regardless of whether or not QPSG is set. (check-in: df95d5209a ... user: drh tags: expr-implies-expr) | 
| 00:58 |  | Add two NEVER() conditions. (check-in: 4f358ad20d ... user: drh tags: expr-implies-expr) | 
| 00:34 |  | Enhancements to sqlite3ExprImpliesExpr() so that it realizes that "iif(A,B)" implies "A". (check-in: d96ecbee59 ... user: drh tags: expr-implies-expr) | 
| 2024-11-27 |  |  | 
| 18:01 |  | Fix an inaccuracy in the documentation in fts5.h. (check-in: ec3ca6f179 ... user: dan tags: trunk) | 
| 2024-11-26 |  |  | 
| 23:40 |  | Allow the iif() function to work with two arguments. The third argument is then assumed to be NULL. Also allow if() as an alternative spelling for iif(). (check-in: 1e405c39a8 ... user: drh tags: trunk) | 
| 23:30 |  | New test cases. (closed check-in: f1cb1a815a ... user: drh tags: two-argument-iif) | 
| 20:56 |  | Provide a two-argument version of the iif() function, plus an alternative spelling that only requires a single "i". (check-in: a251ee645e ... user: drh tags: two-argument-iif) | 
| 20:46 |  | Update fts5 so that the xInstToken() extension API works with prefix queries. (check-in: 41b79bde4a ... user: dan tags: trunk) | 
| 20:16 |  | Update documentation in fts5.h. (closed check-in: f1e44f703a ... user: dan tags: fts5-tokendata-prefix) | 
| 19:19 |  | Patch autosetup to rename --debug to --autosetup-debug so we can use --debug/--enable-debug for its historical purpose. Update autosetup/README.md with details of the patch. (check-in: 00dffd10f0 ... user: stephan tags: trunk) | 
| 19:02 |  | Patch autosetup to rename --debug to --autosetup-debug so we can use --debug/--enable-debug for its historical purpose. --with-debug is now an alias for --enable-debug but can be removed entirely if all scripts which have been adjusted for the autosetup build are edited to (re)use the older flag name. (closed check-in: 3296c8d305 ... user: stephan tags: autosetup-debug-flag) | 
| 16:12 |  | Move some of the in-comment configure script help text into the --help text. No functional changes. (check-in: ee8449c482 ... user: stephan tags: trunk) | 
| 16:04 |  | Minor tweak to ./configure --help output. (check-in: 7e634d0569 ... user: drh tags: trunk) | 
| 03:15 |  | Replace some outdated docs re. soname with a link to the soname forum post. (check-in: 9225701cb8 ... user: stephan tags: trunk) | 
| 2024-11-25 |  |  | 
| 18:47 |  | Ensure that file-handle winShmNode.hFile.h is used in a thread-safe manner. (check-in: 7a32b497ab ... user: dan tags: win32-enable-setlk) | 
| 17:07 |  | Correct duplicated soname linker flags, one of them mis-named. (check-in: 87dfd92d3a ... user: stephan tags: trunk) | 
| 16:54 |  | Fixes for code on this branch. Now appears to work, but is not tested well. (check-in: 0568fc6dde ... user: dan tags: win32-enable-setlk) | 
| 2024-11-22 |  |  | 
| 21:24 |  | Supports SQLITE_ENABLE_SETLK_TIMEOUT on windows. Does not work properly yet. (check-in: 737ca8a9fb ... user: dan tags: win32-enable-setlk) | 
| 17:41 |  | Fix another issue in argument expansion on Windows for tclsqlite3.c in interpreter mode. Problem introduced by check-in [9b87ea219bce5689] and unfixed by [cd942dce148c9d8f]. (check-in: 0fe1622cec ... user: drh tags: trunk) | 
| 16:45 |  | Remove a flaky JS test which has a result depending on unrepredictable context. That same feature is more reliably tested at a later point in the same script. (check-in: 3d6ae13805 ... user: stephan tags: trunk) | 
| 16:24 |  | In order to support package builds, like OpenBSD's, which set a custom soname on libsqlite3.so, extend the --soname configure flag to allow arbitrary soname values. (check-in: 9c1c1b9983 ... user: stephan tags: trunk) | 
| 14:18 |  | Move handling of the tempstore feature flag into the OPT_FEATURE_FLAGS list and remove the CFLAGS.libsqlite3 makefile var which exists solely to account for the tempstore being tracked separately from the other feature flags. (check-in: 2df5065d11 ... user: stephan tags: trunk) | 
| 14:12 |  | Makefile doc cleanups. No functional changes. (check-in: 64add0ac70 ... user: stephan tags: trunk) | 
| 13:47 |  | Remove the libsqlite3.so.3 link from the installation process, as it now serves no functional purpose. We retain libsqlite3.so.0 (A) for compatibility with clients linked against legacy builds and (B) 0 is still valid as the library's ABI version. (check-in: 91bd9813e0 ... user: stephan tags: trunk) | 
| 13:22 |  | Add links to the SONAME discussion at relevant places in auto.def and the makefiles. No functional changes. (check-in: c78dcc318a ... user: stephan tags: trunk) | 
| 12:29 |  | Fix harmless scanbuild warnings caused by the introduction of the ".dbtotxt" command into the CLI by check-in [b43acf5a8cd4a5ef]. (check-in: 554d8fbd86 ... user: drh tags: trunk) | 
| 12:07 |  | Fix some harmless scanbuild warnings in the shell. (check-in: 9ba1c9b505 ... user: drh tags: trunk) | 
| 12:00 |  | Fix harmless compiler warning caused by [c77a4a42f2e3d164]. (check-in: 063690d2c5 ... user: drh tags: trunk) | 
| 2024-11-21 |  |  | 
| 20:57 |  | Make ".scanstatus" an undocumented alternative name for ".scanstats" in the CLI. (check-in: f20688efc2 ... user: drh tags: trunk) | 
| 20:10 |  | Fix a bug-in-waiting (one too few list elements) in [b6bd25bd769e]. (check-in: e527dcd016 ... user: stephan tags: trunk) | 
| 20:07 |  | Add the --scanstatus configure flag to set -DSQLITE_ENABLE_STMT_SCANSTATUS. (check-in: b6bd25bd76 ... user: stephan tags: trunk) | 
| 15:53 |  | Fix a JS test which was broken when OMIT_PROGRESS_CALLBACK was _not_ used. (check-in: fba23150b5 ... user: stephan tags: trunk) | 
| 01:50 |  | Document that -nofollow does not work on Windows. Fix Windows symlink test cases for when the test suite is run as administrator. (check-in: 4de8a75ec5 ... user: drh tags: trunk) | 
| 2024-11-20 |  |  | 
| 20:39 |  | Fix compiler warnings caused by variable shadowing. (check-in: 211b305791 ... user: dan tags: fts5-tokendata-prefix) | 
| 16:21 |  | New configuration option --dynlink-tools causes some command-line tools like sqldiff and sqlite3_analyzer to link against the libsqlite3.so system library rather than being built-in. Caution: sqlite3_analyzer requires the SQLITE_ENABLE_DBSTAT_VTAB compile-time option on its SQLite library in order to work, so do not use --dynlink-tools to build sqlite3_analyzer without it. (check-in: 314c606dd3 ... user: drh tags: trunk) | 
| 16:17 |  | Use shared-library flags, not shared-object flags, to create shared libraries. This makes no difference on Linux, but is required for Mac. (closed check-in: bfaa3ee758 ... user: drh tags: link-tools-dynamically) | 
| 14:59 |  | Bug fix in the SubrtnSig logic from [c9a3498113074bbc], if a subquery is copied and then changes are made to the copy, be sure to give the copy a unique Select.selId value so that the original will not be substituted in place of the modified copy. Forum post 0b9ded2f8428ac00. (check-in: 19d1bede56 ... user: drh tags: trunk) | 
| 14:19 |  | Provide the sqlite3ShowWhereTerm() interface callable interactively from a debugger, when compiling with SQLITE_DEBUG. (check-in: c77a4a42f2 ... user: drh tags: trunk) | 
| 11:34 |  | Do not report an sqlite3_error_offset() for errors that occur inside of views or triggers, since the text of those elements is not part of the original query. (check-in: bf66c6dfc2 ... user: drh tags: trunk) | 
| 2024-11-19 |  |  | 
| 21:19 |  | Add fix from [bfab759611b0] to sqltclsh. Remove some now-unneeded has_tclsh84/5 checks in cases where jimsh can be used. (check-in: 9d389b9983 ... user: stephan tags: link-tools-dynamically) | 
| 21:14 |  | Add missing $TCL_LIBS to the link for sqlite3_analyzer. (check-in: bfab759611 ... user: drh tags: link-tools-dynamically) | 
| 20:21 |  | Rename --link-tools-dynamically to --dynlink-tools, per discussion. (check-in: 50b9f6fde4 ... user: stephan tags: link-tools-dynamically) | 
| 20:14 |  | Move the in-comment-code commentary about --with-debug, from [7b14309be4], into the --help text for that flag, where it's readily visible. (check-in: c938e5d783 ... user: stephan tags: link-tools-dynamically) | 
| 19:47 |  | Get sqlite3_analyzer optionally linking against libsqlite3.so instead of embedding sqlite3.c. Patch mkccode.tcl to accept digits in its IFDEF/IFNDEF checks and sqlite3_analyzer.c.in to only include sqlite3.c if -DINCLUDE_SQLITE3_C is passed to mkccode.tcl. (check-in: 80f3bf8c2e ... user: stephan tags: link-tools-dynamically) | 
| 19:16 |  | Merge trunk into this branch. Fix a small jimtcl/tclsh regex incompatibility in mkccode.tcl. (check-in: 7fb3ebfec6 ... user: stephan tags: link-tools-dynamically) | 
| 18:26 |  | Enhancements to tool/mkccode.tcl such that it recognizes -D command line arguments and can use them in internal IFDEF and IFNDEF macros. Update the tool/sqlite3_analyzer.c.in script such that it omits the SQLite amalgamation if -DSQLITE_ENABLE_DBSTAT_VTAB is defined. (check-in: 7fec209290 ... user: drh tags: trunk) | 
| 17:41 |  | Revert linking the shell to the dll for reasons explained in new makefile comments. Move the --with-debug configure flag into the developer options set and add commentary about why it should never be used for production builds. (check-in: 7b14309be4 ... user: stephan tags: link-tools-dynamically) | 
| 16:40 |  | Reformulate sqldiff deps and rules so that the target matches the resulting file name, to avoid rebuilding it on every make invocation. Apply the same treatment to the sqlite3 CLI shell. (check-in: 9a17b83f85 ... user: stephan tags: link-tools-dynamically) | 
| 15:20 |  | Initial configure support for linking certain binaries to libsqlite3.so instead of embedding it dynamically (which they typically do). This currently applies only to sqldiff, but support for adding the same for other tools is planned. This requires disabling the soname because setting it causes the build to link to whatever libsqlite3.so.0 is installed system-wide, so the soname now defaults to off. (check-in: 9192c146e2 ... user: stephan tags: link-tools-dynamically) | 
| 11:58 |  | Fix a problem handling OOM errors in fts3 that could occur when parsing multi-token strings. (check-in: 4c4d1db00b ... user: dan tags: trunk) | 
| 2024-11-18 |  |  | 
| 18:45 |  | Reduce divergence from begin-concurrent. (check-in: 84d41e9d04 ... user: drh tags: trunk) | 
| 17:05 |  | Attempt to reduce divergence with the wal2 branch. (check-in: e0d8f9916c ... user: drh tags: trunk) | 
| 16:44 |  | Attempt to reduce divergence with begin-concurrent. (check-in: f783d90187 ... user: drh tags: trunk) | 
| 14:15 |  | Remove the .POSIX entries from the makefiles because they cause portability problems rather than solve them. (check-in: e1330ecf17 ... user: stephan tags: trunk) | 
| 14:08 |  | Fix a "applying zero offset to null pointer" usan error in the fts5 trigram tokenizer. (check-in: 9b79b999d4 ... user: dan tags: trunk) | 
| 13:29 |  | Fix minor problems in testrunner.tcl that pop up when the command-line arguments are such that no tests are run. (check-in: 321ded32f6 ... user: drh tags: trunk) | 
| 2024-11-17 |  |  | 
| 11:42 |  | Make the --vfstrace output from the CLI go to the same output channel as everything else. (check-in: f71d490020 ... user: drh tags: trunk) | 
| 09:49 |  | buildtclext.tcl: add docs explaining the DESTDIR check and break out of the auto_path search loop early if the DESTDIR is not writable, rather than re-checking that repeatedly. (check-in: d07be336ea ... user: stephan tags: trunk) | 
| 2024-11-16 |  |  | 
| 18:54 |  | Call fflush() on ".echo" output from the shell, so that the output to stdout is aligned with output to stderr. (check-in: c38b9db3c4 ... user: drh tags: trunk) | 
| 17:39 |  | Fix argument expansion in sqlite-tclsh on Windows such that if an argument does not match a filename even after glob expansion, it is appended to the argument list verbatim. (check-in: cd942dce14 ... user: drh tags: trunk) | 
| 17:09 |  | Handle DESTDIR at an earlier phase in buildtclext.tcl to account for the is-writable-dir check and to filter out //zipfs: dirs as (im)possible installation targets. (check-in: d2e8c161a1 ... user: stephan tags: trunk) | 
| 14:30 |  | Add the --destdir flag to the tclextension-install makefile target. (check-in: d1663cf05f ... user: stephan tags: trunk) | 
| 14:29 |  | Add --destdir flag support to buildtclext.tcl, but do not yet add that to the makefile (so that this change can be cherrypicked to the 3.47 build). (check-in: 67a3ca0c01 ... user: stephan tags: trunk) | 
| 10:42 |  | Rename tool/tclConfigShToTcl.sh to tool/tclConfigShToAutoDef.sh in the name of pedantic correctness. (check-in: a7dd196d99 ... user: stephan tags: trunk) | 
| 09:00 |  | configure: tiny simplification of proj-assert. (check-in: bfdc416b56 ... user: stephan tags: trunk) | 
| 2024-11-15 |  |  | 
| 20:39 |  | Enhance the vfstrace extension such that the output can be controlled using the "PRAGMA vfstrace('...');" statement. See header comment on the source code for details. (check-in: 96105d3359 ... user: drh tags: trunk) | 
| 19:42 |  | In the interest of minimizing downstream disruption, set the soname of libsqlite3.so to (by default) its legacy value of libsqlite3.so.0 and unconditionally create (or replace) a symlink with that name at install-time, in addition to the newer-named symlinks. (check-in: 0773677b55 ... user: stephan tags: trunk) | 
| 16:35 |  | Generic auto.def cleanups. No functional differences. (check-in: 02aceb8c13 ... user: stephan tags: trunk) | 
| 15:31 |  | buildtclext.tcl: work around a case, reported in forum post 0683a49cb0, in which package maintainers edit their copy of tclConfig.sh to change the TCL_SHLIB_LD command. (check-in: e24a3efec8 ... user: stephan tags: trunk) | 
| 10:53 |  | configure: add optional pkg-config support for detecting ICU. (check-in: 3e5b8077c6 ... user: stephan tags: trunk) | 
| 10:47 |  | When checking pkg-config for ICU support use icu-io instead of icu-uc, as that contains all requires libs on Linux and BSD. (closed check-in: 6ca457542e ... user: stephan tags: autosetup-icu-pkg-config) | 
| 10:12 |  | configure script doc additions for the ICU feature check. (check-in: 1925a68fc2 ... user: stephan tags: trunk) | 
| 10:07 |  | An experiment in optionally using pkg-config to determine the libs to link in for ICU support, but its ldflags is missing one required lib on both Linux and OpenBSD. Keeping this for later reference, as it demonstrates how to use pkg-config from autosetup. (check-in: 09caa94c9e ... user: stephan tags: autosetup-icu-pkg-config) | 
| 2024-11-14 |  |  | 
| 19:34 |  | Fix typo in the handling of the new --dev flag which caused it to set the --debug flag instead of the --with-debug flag (the former is for autosetup's internal use). (check-in: 81202d2ab5 ... user: stephan tags: trunk) | 
| 19:25 |  | Remove $prefix/include from the default -I path because it can cause the build to pick up an unintended copy of sqlite3.h. Extend the ICU configure support (the origin of -I$prefix/include) to enable fetching the -I path from icu-config and apply it only to those objects which need it. (check-in: f778dfdd6d ... user: stephan tags: trunk) | 
| 19:06 |  | Add new makefile target "sqlite3d" (where the "d" means either "development" or "debug") that always uses separate source files, regardless of the --disable-amalgmation setting. (check-in: 91da205beb ... user: drh tags: trunk) | 
| 17:52 |  | Add --enable-dev configure flag which sets various other flags. (check-in: ea79c363a4 ... user: stephan tags: trunk) | 
| 16:06 |  | Remove unused sqlite_cfg.h.in from the build. (check-in: bba54e26de ... user: stephan tags: trunk) | 
| 16:00 |  | Fix a state makefile dependency which refered to the now-removed sqlite_cfg.h.in. (closed check-in: 9a726b4be8 ... user: stephan tags: makefile-simplification) | 
| 15:55 |  | Remove some obsolete macros from the CLI. (check-in: 5c4eb62570 ... user: drh tags: trunk) | 
| 15:25 |  | Remove unused sqlite_cfg.h.in (sqlite_cfg.h gets generated without an input template). (check-in: 6148f2d392 ... user: stephan tags: makefile-simplification) | 
| 14:38 |  | Fix a problem with window functions min() and max() when used with a FILTER clause. Forum post e9126d554a. (check-in: d15fb0f75e ... user: dan tags: trunk) | 
| 12:23 |  | Document the if block at the end of sqlite-check-tcl. (check-in: 6bfd09408b ... user: stephan tags: trunk) | 
| 12:09 |  | configure: avoid performing multiple checks for -lm on behalf of --enable-fts4 and --enable-fts5. (check-in: 6940caa192 ... user: stephan tags: trunk) | 
| 2024-11-13 |  |  | 
| 18:23 |  | Enhance the vfstrace.c extension to show symbolic names for the various SHM locks. (check-in: c0dd7de8f1 ... user: drh tags: trunk) | 
| 18:04 |  | Fix a memory leak in the ".dump" command of the CLI that can occur if an error other than database corruption is seen while trying to query the database. (check-in: 2560cc7072 ... user: drh tags: trunk) | 
| 16:08 |  | Add the test/fork-test.c test program. (check-in: 0611e2b0cf ... user: drh tags: trunk) | 
| 14:58 |  | Add the SQLITE_FCNTL_NULL_IO file-control. (check-in: f0e917fcf5 ... user: drh tags: trunk) | 
| 2024-11-12 |  |  | 
| 13:37 |  | Ensure that the sqlite3_index_info.idxStr string coming back from FTS5 is always zero-terminated, even if the xBestIndex call fails with an SQLITE_CONSTRAINT error. (check-in: a4e976a030 ... user: drh tags: trunk) | 
| 2024-11-11 |  |  | 
| 21:11 |  | Clarify the documentation to make it clear that rows inserted by a CREATE TABLE AS SELECT statement are not counted by sqlite3_count64(). Forum post 1e6cde5648. (check-in: 5c5982e393 ... user: drh tags: trunk) | 
| 19:49 |  | Fix the fts5 trigram tokenizer so that it handles non-nul-terminated strings. (check-in: 84f4e37178 ... user: dan tags: trunk) | 
| 19:07 |  | Add the ".dbtotxt" command to the CLI. (check-in: b43acf5a8c ... user: drh tags: trunk) | 
| 18:15 |  | Wrap some exceptionally long lines in main.mk. Add option to override LDFLAGS on the sqlite3.dll target. Audit: all targets for which it is hypothetically relevant can now inherit user-supplied LDFLAGS, but only those provided to the configure script, not at make-time, in order to mimic the historical build's restriction in that regard. (check-in: 073080cae3 ... user: stephan tags: trunk) | 
| 17:02 |  | Use Win32 APIs to read/write the console in Windows unless the SQLITE_USE_STDIO_FOR_CONSOLE option is defined. This is an attempt to get the build working on MinGW. (check-in: abfe488ed6 ... user: drh tags: trunk) | 
| 09:53 |  | Doc update to account for [05073350087b]. (check-in: b81976c520 ... user: stephan tags: trunk) | 
| 09:37 |  | Rename proj-define-if-opt-truthy to the more accurate, and less verbose, proj-define-for-opt. (check-in: 0507335008 ... user: stephan tags: trunk) | 
| 2024-11-09 |  |  | 
| 17:54 |  | Fix a case in fts3 where a corrupt database record was not being handled correctly. (check-in: cbcb53759b ... user: dan tags: trunk) | 
| 14:34 |  | Fix a deps problem, introduced in [1bd9de719], which breaks the build on systems where HAVE_TCL=0. (check-in: 7953a8bdcb ... user: stephan tags: trunk) | 
| 10:23 |  | Simplify usage of proj-assert by making the expr prefix implicit. Add an optional description arg to proj-assert, defaulting to the body of the assertion script. (check-in: ada7b36c37 ... user: stephan tags: trunk) | 
| 09:19 |  | Remove three unused files from autosetup/. (check-in: 906563a7e0 ... user: stephan tags: trunk) | 
| 06:35 |  | Remove an unused lib import from auto.def. (check-in: 53a71981ab ... user: stephan tags: trunk) | 
| 2024-11-08 |  |  | 
| 20:57 |  | Increase the minimum SQLITE_LENGTH_LIMIT from 1 to 30 to avoid problems doing simple things like converting strings into integers. See also [8fd5b8ec4ab9b555]. (check-in: 6aa01707af ... user: drh tags: trunk) | 
| 20:44 |  | Fix an assert() failure in sqlite3recover.c. (check-in: f52bb19281 ... user: dan tags: trunk) | 
| 14:34 |  | Remove one dep from .tclenv.sh which is only valid for configure-driven builds, not static makefiles. (check-in: 22986767da ... user: stephan tags: trunk) | 
| 13:37 |  | Add 'tcl' makefile target which builds all but tclextension (which does not have a well-defined name and dependencies). Improve the deps for .tclenv.sh to avoid getting a stale tclsh when re-configuring with a different --with-tcl(sh). (check-in: 1bd9de719b ... user: stephan tags: trunk) | 
| 08:01 |  | Add missing CFLAGS.intree_includes to T.compile.tcl to fix build of tclsqlite3. (check-in: bb3c6dc126 ... user: stephan tags: trunk) | 
| 07:37 |  | More closely emulate the legacy build's handling of LDFLAGS, permitting them to be passed to configure but not to make. We cannot 100% enforce that because main.mk is not filtered by the configure script, so we instead add a level of indirection to make passing of LDFLAGS=... to make a no-op. (check-in: d1af9f3183 ... user: stephan tags: trunk) | 
| 06:22 |  | Support clients passing in LDFLAGS to configure/make, but in a more limited form than the legacy build it (i.e. only to select targets rather than all targets). Rename make-side internal uses of CFLAGS to CFLAGS.env for consistency with the new LDFLAGS.env. See discussion in forum thread 5fcbea40f3. (check-in: a5e07e8063 ... user: stephan tags: trunk) | 
| 05:26 |  | Disable setting of the SONAME (enabled by [2a2419ef742]), as it's not clear whether blindly setting the SONAME, which now differs from its historical value, will cause more grief than it solves. Add a (disabled) experiment which permits setting (or not) the SONAME to the legacy or current values. This change is up for further change as experimentation proves whether we truly need/want the SONAME. See discussion in/around forum post 0c6fc6f46b2cb3. (check-in: d931456805 ... user: stephan tags: trunk) | 
| 2024-11-07 |  |  | 
| 17:34 |  | Fix an OOB write that could occur in fts3 when dealing with corrupt database records. (check-in: 108863ec79 ... user: dan tags: trunk) | 
| 16:59 |  | Extend the set of --flags which get cleared/unset when using --with-wasi-sdk. Enable the soname linker option for libsqlite3.so. (check-in: 2a2419ef74 ... user: stephan tags: trunk) | 
| 15:23 |  | Doc touchups in the line-editing feature check code. (check-in: bb5656bdc9 ... user: stephan tags: trunk) | 
| 15:04 |  | tcl configuration: --with-tcl=prefix is equivalent to passing the --prefix dir to it. If --with-tcl or --enable-tcl are explicitly passed in and tclConfig.sh is not found, fail fatally. When TCL is either explicitly disabled or default search for it fails non-fatally, be more explicit about which components are not available. (check-in: c5389d39a9 ... user: stephan tags: trunk) | 
| 12:03 |  | Further improvements to the ".mode json" output in the CLI. (check-in: 6201b5707f ... user: drh tags: trunk) | 
| 10:07 |  | Autosetup doc touchups. (check-in: 5745152900 ... user: stephan tags: trunk) | 
| 09:36 |  | Extend the list of "truthy" config values to include "true". Related doc updates. (check-in: 4455718e48 ... user: stephan tags: trunk) | 
| 08:57 |  | Add some internal docs to auto.def explaining certain passages. (check-in: 4749967e1b ... user: stephan tags: trunk) | 
| 08:29 |  | Document the purpose of a global auto.def var added in [0d558318172d]. (check-in: f5b6604716 ... user: stephan tags: trunk) | 
| 07:31 |  | When --with-linenoise refers to the msteveb flavor and jimsh is the TCL used for code generation, enable linenoise in jimsh. Remove some dead auto.def code. (check-in: 0d55831817 ... user: stephan tags: trunk) | 
| 06:02 |  | Add mention of file-isexec in autosetup/README.md. (check-in: f469356749 ... user: stephan tags: trunk) | 
| 06:00 |  | Change several 'file exists' checks for tclsh to file-isexec because that checks for .exe extensions on Windows. (check-in: 052a1b4d7c ... user: stephan tags: trunk) | 
| 05:26 |  | For platforms where tclsh is found but tclConfig.sh is not, set HAVE_TCL to 0. We can't build the TCL components on those. Problem reported in forum post 5106aee3a8. (check-in: 04d2576b2e ... user: stephan tags: trunk) | 
| 2024-11-06 |  |  | 
| 19:19 |  | Fix an FTS3 corruption test case that depends on the specific pseudo-random byte sequence generated by sqlite3_randomness(), which is different on big-endian platforms than it is on little-endian platforms. (check-in: 6216bfcb74 ... user: drh tags: trunk) | 
| 17:31 |  | Merge latest trunk changes into this branch. (check-in: edb8423493 ... user: dan tags: fts5-tokendata-prefix) | 
| 16:20 |  | Add tests for xInstToken() and prefix queries with various fts5 configurations. (check-in: 9cc04331a0 ... user: dan tags: fts5-tokendata-prefix) | 
| 12:58 |  | Re-phrase some (#if !SQLITE_CORE) to (#ifndef SQLITE_CORE), as discussed in forum post cea40371c5e34b09. (check-in: 5495b12569 ... user: stephan tags: trunk) | 
| 12:13 |  | Elaborate on how autosetup selects a tclsh to use. (check-in: d3887895a3 ... user: stephan tags: trunk) | 
| 10:09 |  | Fix typo in the LICENSE.md file. (check-in: 0588cc5c2f ... user: drh tags: trunk) | 
| 08:29 |  | Minor doc tweaks. (check-in: 2919a61bd4 ... user: stephan tags: trunk) | 
| 07:20 |  | When constructing the auto-reconfigure commands, only quote args which look like they need it. Minor doc updates. (check-in: d4fbd34f7a ... user: stephan tags: trunk) | 
| 06:44 |  | Minor doc tweaks and typo fixes. (check-in: 406d9122b7 ... user: stephan tags: trunk) | 
| 06:06 |  | Split the motivation of the makefile var naming convention into its doc own section. (check-in: bf42b93cc7 ... user: stephan tags: trunk) | 
| 05:54 |  | Add a section to autosetup/README.md describing the motivations for the more glaring design decisions. (check-in: c0940a822a ... user: stephan tags: trunk) | 
| 04:38 |  | Add autosetup/README.md - maintenance-related docs for SQLite developers (e.g. how to update autosetup). Start marking up the proj.tcl APIs with autosetup's doc markup so that they appear in the ./configure --reference output. (check-in: aa6213767f ... user: stephan tags: trunk) | 
| 02:59 |  | Enhance the --with-emsdk flag to use a default value of 'auto', meaning to search the environment for it, and to fail fatally if --with-emsdk is explicitly provided but the SDK is not found. (check-in: 9724b747ca ... user: stephan tags: trunk) | 
| 2024-11-05 |  |  | 
| 23:26 |  | Improve the ".mode json" output of the CLI so that it encodes U+007f using an escape sequence. (check-in: 8b58cf9bbd ... user: drh tags: trunk) | 
| 18:25 |  | Have the xBestIndex method of the generate_series virtual table ignore contraints on the "value" column with usable=0. (check-in: f5113a2ef8 ... user: dan tags: trunk) | 
| 13:34 |  | Work around a TCL {list-quoting} quirk when escaped/deferred var derefs are passed in as values to the autotools-conventional --XYZdir flags (as seen in a downstream package build script). (check-in: 6adfca7e85 ... user: stephan tags: trunk) | 
| 13:03 |  | Remove the --enable-rbu flag from the configure script, per /chat discussion, because it's a highly niche feature not in active use. (check-in: 77db0cd736 ... user: stephan tags: trunk) | 
| 11:59 |  | Ensure that the one-line running status output from testrunner.tcl does not exceed the 80-char line width of a standard terminal. (check-in: 1eae0f9ed3 ... user: drh tags: trunk) | 
| 09:49 |  | auto.def: remove an extraneous global var, some dead tcl/makefile code/comments, and some extraneous output. Rename an internal-use proc. Disable the ext/wasm cleanup in the top-level (dist)clean because the noise from gmake is irritating. (check-in: ee9eb8b157 ... user: stephan tags: trunk) | 
| 08:21 |  | Enhance the rpath configure check to honor --exec-prefix=... Remove some debugging code and move other debugging code around. (check-in: a8c1a82b56 ... user: stephan tags: trunk) | 
| 08:14 |  | Roll back [47e50fa84dacf83c] for consistency with the legacy build. sqlite3.pc is now generated equivalently to prior versions. (check-in: af79d11e38 ... user: stephan tags: trunk) | 
| 07:55 |  | Add the --enable-rbu configure flag and building of the commented-out rbu binary. (check-in: 6c6fa3adfe ... user: stephan tags: trunk) | 
| 07:29 |  | Remove some dead makefile code. (check-in: b69ce89d22 ... user: stephan tags: trunk) | 
| 02:38 |  | Change a (#define HAVE_FCHMOD) to (#define HAVE_FCHMOD 1) per user request in forum post 24cf6020c6. There's no semantic change for this tree, but this is consistent with how auto-config tools would define it and the empty #define interferes with some downstream code. (check-in: 9c46d84f3b ... user: stephan tags: trunk) | 
| 02:14 |  | sqlite3_stdio.c now uses sqlite3_malloc()/sqlite3_free() instead of malloc()/free(). Reported in forum post 6b6cb3dd. (check-in: 1982471da1 ... user: stephan tags: trunk) | 
| 2024-11-04 |  |  | 
| 20:16 |  | Fix the percentile extension so that works as an independent extension. Forum post ab25469a350e0488 (check-in: 27b829c344 ... user: drh tags: trunk) | 
| 19:18 |  | Fix harmless compiler warnings. (check-in: bc93e17924 ... user: drh tags: trunk) | 
| 19:08 |  | Ensure that the database encoding is detected before the code generator gets too far down into byte-code generation and execution, but not so early that it interferes with initialization. Forum thread bc75a4d20b756044. (check-in: af7173a10e ... user: drh tags: trunk) | 
| 16:59 |  | Avoid loading the entire record into memory for an sqlite3_preupdate_old() call that retrieves an IPK value. (check-in: 7f4de43733 ... user: dan tags: trunk) | 
| 13:57 |  | Fix two mismatched uses of malloc() and sqlite3_free() in sqlite3_stdio.c, as reported in forum post 7dd7c70038. (check-in: af0a345b3b ... user: stephan tags: trunk) | 
| 12:11 |  | Fix typo of --libexec ==> --libexecdir, discovered via audit of a downstream build script. Unrelated doc touchups. (check-in: a60e5d76d0 ... user: stephan tags: trunk) | 
| 10:16 |  | Ensure that the ext/wasm fiddle build undefines certain shell feature flags which it cannot use but might be inherited from the top-level makefile. (check-in: 99bb5d9b68 ... user: stephan tags: trunk) | 
| 08:29 |  | When installing the shared lib, re-activate the accommodation of legacy-style .so links (removed in [80584e165e4]) based on two reports that not having those will cause all clients linked against the legacy naming convention to fail to dynamically link (which would preclude an update of a system-level libsqlite3 package). Set up the infrastructure needed for adding an SONAME to the library but do not yet activate it. See discussion in/around forum post 046133a7da. (check-in: 1586eaceb1 ... user: stephan tags: trunk) | 
| 06:38 |  | Minor internal doc additions. (check-in: 9edc8582c9 ... user: stephan tags: trunk) | 
| 05:27 |  | Latest upstream autosetup to get handling of (==ignoring) autotools' x-includes and x-libraries flags and stop leakage of some autosetup-init-level vars into auto.def's global scope. (check-in: 307349bf91 ... user: stephan tags: trunk) | 
| 04:12 |  | Latest upstream jimsh0.c, which accounts for the problem patched locally by [29b944959568]. (check-in: c7a5b7d2db ... user: stephan tags: trunk) | 
| 03:43 |  | Minor doc correction and proj.tcl-internal API renaming. (check-in: c8b24c590e ... user: stephan tags: trunk) | 
| 2024-11-03 |  |  | 
| 21:32 |  | Move the .default-CFLAGS handling into a utility function, rename the corresponding file to .env-VAR, and apply that lookup to a couple more places. (check-in: f01b61f21d ... user: stephan tags: trunk) | 
| 20:15 |  | Fix a CFLAGS.jimsh typo in Makefile.in. Change default jimsh flags to include -O1, which slows down its build but speeds up its runtime considerably. (check-in: a912c16915 ... user: stephan tags: trunk) | 
| 19:42 |  | If any autotools-conventional dir names are explicitly overridden via ./configure --dirname=X then export those names as-is to the makefile, otherwise derive them from $prefix. This is, at least in part, a compromise to provide package maintainers a way to specify a libdir which will be incorporated into the -rpath link flag (which we can only determine at configure-time). (check-in: 832abe8a8a ... user: stephan tags: trunk) | 
| 18:30 |  | Workaround for a compiler-specific jimsh compilation error reported in forum post 18e420d0. Reported upstream as ticket #322. (check-in: 29b9449595 ... user: stephan tags: trunk) | 
| 07:45 |  | Minor doc updates. Remove two outdated todos. (check-in: ad19237845 ... user: stephan tags: trunk) | 
| 05:33 |  | Fix another T.exe typo and remove an outdated TODO. (check-in: 02208e6bfa ... user: stephan tags: trunk) | 
| 01:31 |  | Fix T.exe typos in main.mk which break installation on platforms where that value is not empty. Also some unrelated doc tweaks. (check-in: ab97ff44e6 ... user: stephan tags: trunk) | 
| 2024-11-02 |  |  | 
| 19:10 |  | Allow an fts5 table or query to be configured to collect xInstToken data for any prefix terms as part of the first parse of the main index, if any. (check-in: 46929ae92b ... user: dan tags: fts5-tokendata-prefix) | 
| 05:50 |  | An experiment in hybridizing overriding of autotools-conventional XYZdir vars, the goal being to be able to provide both overrriding of them at configure-time and make-time with sensible semantics. Based on notes from forum post 00d12a41f7. (closed check-in: cc259bf5f1 ... user: stephan tags: autosetup-dir-overrides) | 
| 03:34 |  | Add autotools-compatible overridable dir name vars for the various installation targets, calculated at make-time instead of exported at configure-time for reasons explained at length in the accompanying comments. (check-in: 24aba7ee58 ... user: stephan tags: trunk) | 
| 2024-11-01 |  |  | 
| 19:41 |  | Merge latest trunk changes into this branch. (check-in: 790c56d493 ... user: dan tags: fts5-tokendata-prefix) | 
| 01:56 |  | When configuring with --with-wasi-sdk, ensure that we use that SDK's 'ar' tool instead of whatever is in the PATH. When pushing/popping the define var scope for the jimsh compatility check, include more defines per consultation with autosetup's creator. (check-in: 2dcc465a70 ... user: stephan tags: trunk) | 
| 2024-10-31 |  |  | 
| 18:29 |  | In testrunner.tcl, only show the ETC on the status line if it will fit within the 80-character line limit. (check-in: 45a3213d23 ... user: drh tags: trunk) | 
| 17:38 |  | Fix the clean-autosetup target in Makefile.in so that it does not fail even in cases where gmake is unavailable. (check-in: 207ca21bd7 ... user: drh tags: trunk) | 
| 17:23 |  | Extra defenses against UAF when failing to allocate a transient cursor. No known path to a UAF currently exists. This change just helps with the static analysis to prove it. (check-in: bae0581111 ... user: drh tags: trunk) | 
| 11:53 |  | When building a shared library on Mac, one must specify the original *.o files that go into that library. It does not work to specify a prior shared library containing a subset of the files to be included. (check-in: 5adc7d5dab ... user: drh tags: trunk) | 
| 11:21 |  | Omit unnecessary bold text in ./configure output. (check-in: d1368dc12b ... user: drh tags: trunk) | 
| 11:19 |  | Improved differentiation between antirez and msteveb linenoise. (check-in: cfb4bfcf14 ... user: drh tags: trunk) | 
| 09:25 |  | Add a missing 'close' call to proj-file-content-list. (check-in: 005702f334 ... user: stephan tags: trunk) | 
| 08:12 |  | Add -trim flag to proj-file-content. (check-in: babf66f5c7 ... user: stephan tags: trunk) | 
| 08:03 |  | Remove some proj-assert checks which are not valid on OpenBSD. Ensure that queued up notices about TCL warnings are displayed before leaving sqlite-check-tcl. (check-in: 3447308f3f ... user: stephan tags: trunk) | 
| 07:30 |  | When reading in .default-CFLAGS, trim it. (check-in: a62a5171ac ... user: stephan tags: trunk) | 
| 07:20 |  | Add the .POSIX special target to the main makefiles to hint to the make impl that it should behave POSIXly-correct. Fix the default file extension for static libs in main.mk. (check-in: 2c615d3a32 ... user: stephan tags: trunk) | 
| 07:01 |  | Add more deps to T.tcl.env.sh so that it gets rebuilt if configure is run with a different --with-tcl value. Remove all references to the superfluous XYZdir makefile vars conventionally set by the autotools (they're just noise in this build). (check-in: 831665101e ... user: stephan tags: trunk) | 
| 06:52 |  | If a file named .default-CFLAGS exists in the build dir, use its contents as the default value of the CFLAGS unless it's overridden by being passed in or in the environment. (check-in: 731d333c22 ... user: stephan tags: trunk) | 
| 06:38 |  | When --with-wasi-sdk is active, temporarily swap CC and CC_FOR_BUILD for purposes of looking for APIs required by B.cc. (check-in: 435f2ee818 ... user: stephan tags: trunk) | 
| 05:47 |  | Change default CFLAGS to {-g -O2} to match the legacy build. (check-in: 5117749429 ... user: stephan tags: trunk) | 
| 01:51 |  | Fix a corner case in automatic TCLLIBDIR detection: if the $auto_path list is not empty but none of them refer to an existing dir, the last dir in that list would have been accepted as the TCLLIBDIR. It will now error out instead. (check-in: 1d2ecae130 ... user: stephan tags: trunk) | 
| 00:56 |  | Reimplement how the TCL-related environment vars are made available to make recipes which need them, reducing the console noise and consolidating it with the has_tclconfig flag file. It now uses a generated shell snippet which it sources, rather than inlining all of that shell code. (check-in: 0cdfd142e1 ... user: stephan tags: trunk) | 
| 2024-10-30 |  |  | 
| 23:10 |  | Minor cleanups to the linenoise flavor detection test. (check-in: 3be32de162 ... user: stephan tags: trunk) | 
| 22:49 |  | Move the linenoise flavor check from auto.def to proj.tcl for re-use in downstream projects. (check-in: 84e503dc1e ... user: stephan tags: trunk) | 
| 22:41 |  | Expand [c0048e4482e9] to determine the linenoise API flavor via a compile test rather than guessing based on the filename. (check-in: dbf0079190 ... user: stephan tags: trunk) | 
| 22:09 |  | proj.tcl/main.mk doc updates. No functional changes. (check-in: e06574c6b4 ... user: stephan tags: trunk) | 
| 19:13 |  | Adjust Makefile.in to honor the CC environment variable. Add the "show-variables" target to main.mk, for debugging. (check-in: e5f1a01f9f ... user: drh tags: trunk) | 
| 14:03 |  | Fix the CLI so that it can use either the canonical Antirez linenoise (with HAVE_LINENOISE=1) or Steve Bennett's enhanced linenoise that works on the Win32 console as well as on Unix (with HAVE_LINENOISE=2). The ./configure script detects which one to use and sets HAVE_LINENOISE accordingly. (check-in: c0048e4482 ... user: drh tags: trunk) | 
| 05:07 |  | Clean up and add docs to the TCL vetting steps. Make warning and error messages bold if stdout isatty. (check-in: 47157dcf9e ... user: stephan tags: trunk) | 
| 04:07 |  | Remove accommodation of the legacy-named shared libraries from the installation rules, per discussion. Rename install-includes to install-headers. Quote installation target dir names "just in case". (check-in: 80584e165e ... user: stephan tags: trunk) | 
| 03:51 |  | Generic makefile cleanups. (check-in: 6f86ff2e8c ... user: stephan tags: trunk) | 
| 02:06 |  | Re-enable exporting of TCLLIBDIR at configure-time so that clients can override it once there rather than having to override it on an arbitrary number of make invocations (which they can still do, but now need not). Based on feedback in forum post 38f6988e57b. (check-in: ee6e15f12e ... user: stephan tags: trunk) | 
| 01:47 |  | When calculating the TCLLIBDIR in main.mk, allow the user to override it using an environment var or make var assignment, per feedback in forum post 38f6988e57b. (check-in: 838633182c ... user: stephan tags: trunk) | 
| 2024-10-29 |  |  | 
| 23:42 |  | Split the core-most CFLAGS, which should not be overridable (like -fPIC), into CFLAGS.core, so that went CFLAGS=... is passed on in a make invocation the -fPIC flag is not lost (as reported in forum post 39f8c543). (check-in: f5899d22c6 ... user: stephan tags: trunk) | 
| 23:19 |  | Try to detect if --with-readline-cflags is passed a non-cflags value, e.g. readline/readline.h, and fail loudly if it is. The legacy configure accepted that formulation but silently did nothing useful with it and that formulation now causes the auto-completion test to fail, as reported in forum post f13b8e8f38402367. (check-in: c97f34d064 ... user: stephan tags: trunk) | 
| 20:27 |  | General auto.def cleanups and docs. (check-in: e2f41c2540 ... user: stephan tags: trunk) | 
| 19:03 |  | Reformulate the readline completion signature compatibility test so that -Wunused-variable in the CFLAGS does not cause it to fail. Problem reported via email. (check-in: 3891669a3f ... user: stephan tags: trunk) | 
| 18:50 |  | Rename an almost-name-colliding auto.def function. Fix a refactoring-induced change which broke implicit lookup of tclConfig.sh. Add msg-debug proc to enable toggling of developer-level debug messages via a configure argument. (check-in: 265ba15df1 ... user: stephan tags: trunk) | 
| 17:55 |  | Remove some extraneous configure output. (check-in: 2234569edb ... user: stephan tags: trunk) | 
| 14:22 |  | Fix a typo in LICENSE.md (check-in: decc600348 ... user: drh tags: trunk) | 
| 08:42 |  | When either --readline or --editline are explicitly provided but the corresponding feature is not found, fail fatally. If not explicitly requested, simply disable that feature if the lib is not found. (check-in: 1d24a29c6e ... user: stephan tags: trunk) | 
| 08:18 |  | Minor --help text tweak. (check-in: 80ac10ed8d ... user: stephan tags: trunk) | 
| 08:14 |  | Add proj-assert proc and add a couple of asserts to the editline/readline checks. (check-in: db9870caa8 ... user: stephan tags: trunk) | 
| 08:01 |  | Teach configure --editline to report itself to shell.c as either HAVE_EDITLINE or HAVE_READLINE, depending on which headers are available (editline/readline.h or readline/readline.h, both of which work and some systems only have the latter), but then link against libedit regardless of which one shell.c thinks it is using. (check-in: 85077b20e6 ... user: stephan tags: trunk) | 
| 04:21 |  | Remove LDFLAGS_LIBZ from sqlite3.pc.in (pkg-config) because it's not needed by the library, only the shell. Re-enable the notice about --disable-static only being partially true. (check-in: e03c564aff ... user: stephan tags: trunk) | 
| 03:29 |  | Remove some dead auto.def code. Link tclsqlite3 shell against the .so instead of .a, analog to [45315f8f275d]. Move some header file tests which are only needed for jimsh into the jimsh compilability check block. (check-in: 9a259026ae ... user: stephan tags: trunk) | 
| 2024-10-28 |  |  | 
| 22:35 |  | Enclose the generated "sqlite3.c" and "sqlite3.h" in a single big #ifdef so that if they get truncated by more than a little whitespace, they will not compile and the truncation can be easily detected. (check-in: c3b624e2a9 ... user: drh tags: trunk) | 
| 19:19 |  | Remove unnecessary install-sh and spec.template files. (check-in: 3acb6d789e ... user: drh tags: trunk) | 
| 18:41 |  | Minor makefile formatting cleanups. No functional changes. (check-in: 10b2cfdccd ... user: stephan tags: trunk) | 
| 18:30 |  | Calculate TCLLIBDIR in the makefile targets which use it, rather than via the configure script. This enables its use in static makefiles. (check-in: 6b1494cecb ... user: stephan tags: trunk) | 
| 17:30 |  | Remove the ltmain.sh autotools remnant. (check-in: dad5eb9393 ... user: stephan tags: trunk) | 
| 17:27 |  | Remove the never-used and never-documented and long-ago deprecated user-authentication feature option. [65884d4f81a4705b]. (check-in: 3a3f7bf430 ... user: drh tags: trunk) | 
| 17:20 |  | Perform some makefile acrobatics to get the tclConfig.sh state applied for static makefiles. (check-in: 9b141d108b ... user: stephan tags: trunk) | 
| 16:31 |  | The async extension was superseded by WAL mode about 11 years ago, and has long been deprecated. Remove it from the source tree. (Anybody who really needs it can still dig it out of the historical records.) (check-in: f98da150a9 ... user: drh tags: trunk) | 
| 16:28 |  | Have sqlite3_rsync avoid write-locking the origin database. (check-in: b7eb653050 ... user: dan tags: trunk) | 
| 15:38 |  | Omit the antiquated and long-unsupport async extension since it has been superseded by WAL mode for over a decade. (closed check-in: 10b1b86821 ... user: drh tags: omit-async) | 
| 14:53 |  | Omit ext/consio from the tree. No longer needed or supported. (check-in: 1ce8507f73 ... user: drh tags: trunk) | 
| 14:46 |  | Update Makefile.linux-generic to account for recent var renaming. (check-in: 6cb7cb7e33 ... user: stephan tags: trunk) | 
| 14:24 |  | Link libtclsqlite3.so to libsqlite3.so, not libsqlite3.a. (check-in: 45315f8f27 ... user: stephan tags: trunk) | 
| 14:16 |  | Rename the 'hwaci' auto.def utility API to the more generic 'proj' API, per /chat discussion. Fix a function name typo which caused readline detection to kill the configure in one code path. (check-in: e20610f06b ... user: stephan tags: trunk) | 
| 13:16 |  | Remove the long-unused, legacy vsixtest/ logic from the source tree. (check-in: 45ec9415b2 ... user: drh tags: trunk) | 
| 11:13 |  | Simplify how hwaci-xfer-options-aliases figures out whether flags are provided. (check-in: f06122a543 ... user: stephan tags: trunk) | 
| 10:39 |  | Merge latest trunk changes into this branch. (check-in: bce9a524de ... user: dan tags: fts5-tokendata-prefix) | 
| 02:49 |  | Document why -IPREFIX/include is now part of T.cc.sqlite. (check-in: d64dcb1a89 ... user: stephan tags: trunk) | 
| 02:46 |  | Correct the default config value for HAVE_TCL (0 until proven otherwise) and fix the starting dir for SQLITE_AUTORECONFIG (formerly SQLITE_AUTOREMAKE), as reported in forum post 8ab69387008f2f6c. (check-in: 55f78b6396 ... user: stephan tags: trunk) | 
| 02:11 |  | Have --enable-editline fail rather than emit a warning which would easily be overlooked by automated builds. The error message explains a potential workaround for activating libedit. (check-in: 2bd1b9557a ... user: stephan tags: trunk) | 
| 01:13 |  | Docs and small code cleanups in hwaci-common.tcl. (check-in: 1353d4b600 ... user: stephan tags: trunk) | 
| 00:56 |  | Add --with-icu-config flag to use the icu-config binary to find the required ldflags for linking the ICU libraries. (check-in: 64f33bb125 ... user: stephan tags: trunk) | 
| 2024-10-27 |  |  | 
| 22:34 |  | Expand the 'make help' target and clean up some overly-noisy configure output. (check-in: c895766ed3 ... user: stephan tags: trunk) | 
| 22:18 |  | Re-order the auto.def flags logical groups. Add some discrete bold formatting to select configure output. Remove some dead code. Account for behavior misunderstanding of user-notice. Add the start of a 'make help' target. General cosmetic cleanups. (check-in: ab95ee33df ... user: stephan tags: trunk) | 
| 20:46 |  | Generic cleanups in auto.def and hwaci-common.tcl. (check-in: 428e542452 ... user: stephan tags: trunk) | 
| 20:14 |  | Doc additions and minor cleanups in the --flag alias handling. (check-in: 37a1da0381 ... user: stephan tags: trunk) | 
| 20:04 |  | Rename --with-readline-lib/inc to --with-readline-ldflags/cflags, for clarity, but retain the older names as aliases using autosetup's "hidden alias" feature. Rename the newly-added --with-icu-lib to --with-icu-ldflags (with no backwards compatibility). (check-in: e50a03f9f2 ... user: stephan tags: trunk) | 
| 19:26 |  | When ICU is enabled, ensure that the CLI shell build enables its feature flag and include LDFLAGS_ICU in sqlite3.pc. (check-in: abec913c00 ... user: stephan tags: trunk) | 
| 18:46 |  | When readline is enabled, attempt to determine whether the completion API is compatible and, if it's not, disable it in the shell app. (check-in: 4564dbe4ac ... user: stephan tags: trunk) | 
| 16:12 |  | Fix build errors in fts5_tcl.c on Mac. (check-in: 133fff8bd7 ... user: drh tags: trunk) | 
| 15:58 |  | Fix test code added by [ba358d26]. (check-in: b7da7980d3 ... user: dan tags: trunk) | 
| 14:41 |  | Merge latest trunk into this branch. (check-in: 740a37c5d5 ... user: dan tags: fts5-tokendata-prefix) | 
| 10:48 |  | Improvements to the PATTERN option of the "testrunner.tcl joblist" command. The pattern match is case insensitive and applies to the state and displaytype in addition to displayname. (check-in: 63a8f60f98 ... user: drh tags: trunk) | 
| 10:33 |  | Help systems still using Tcl8.6 to compile tclsqlite.c by changing a typedef into a #define. (check-in: 82ab8ff399 ... user: drh tags: trunk) | 
| 09:28 |  | Remove some incorrect code comments. (check-in: 6dfda7f579 ... user: stephan tags: trunk) | 
| 08:52 |  | Re-add the --disable-static flag to (mostly) disable static lib build. Some components require the static lib and will trump this preference if they are activated. Delete temp files created by mksqlite3h.tcl. (check-in: e296cd11a2 ... user: stephan tags: trunk) | 
| 08:29 |  | Generic auto.def cleanups. (check-in: 19a5377e60 ... user: stephan tags: trunk) | 
| 08:10 |  | Ensure that --disable-tcl is honored. Move LDFLAGS.icu from the CLI shell target to LDFLAGS.libsqlite3. (check-in: 39ce3f6350 ... user: stephan tags: trunk) | 
| 07:06 |  | configure: add --with-icu-lib=LDFLAGS and --enable-icu-collations. Fix auto-reconfigure when flags contain spaces. (check-in: 2a881a2e1b ... user: stephan tags: trunk) | 
| 06:03 |  | Makefile.in: include CPPFLAGS in the CFLAGS, as the legacy Makefile.in did, because some downstream scripts rely on that. Remove a duplicated TCLLIBDIR assignment. (check-in: 8e352f4199 ... user: stephan tags: trunk) | 
| 05:28 |  | configure: document why --enable-editline does not work and emit a warning with a potential alternative/workaround if it's used. Various cosmetic cleanups. (check-in: ccb8f16f8a ... user: stephan tags: trunk) | 
| 04:27 |  | Bring the automated readline detection up to parity with the legacy configure script. (check-in: 2ddeb7a8f5 ... user: stephan tags: trunk) | 
| 02:27 |  | Rename --with-readline-ldflags/cflags to --with-readline-lib/inc because it turns out that ldflags/cflags have (when passed an explicit value) the same semantics the legacy lib/inc flags. Still to-fix is that the no-flag-given readline search behavior differs, and is much more limited, from the legacy configure behavior. (check-in: 8f6897b92c ... user: stephan tags: trunk) | 
| 02:08 |  | Correct sqlite3.pc to represent the library, not the CLI shell. Remove the half-baked an unused READLINE_H config define. Internal tcl doc fixes. (check-in: c8c70353bb ... user: stephan tags: trunk) | 
| 2024-10-26 |  |  | 
| 21:51 |  | Get --with-linenoise=DIR configure flag working. (check-in: 66ce47e5de ... user: stephan tags: trunk) | 
| 19:16 |  | Remove the install-lib makefile dep from install-tcl, as it breaks non-root-user invocations of 'make install-tcl' when the --prefix is left at its default and --with-tcl is explicitly provided. (check-in: e7eabfb61f ... user: stephan tags: trunk) | 
| 18:34 |  | configure flag --disable-readline now trumps --with-readline-... (check-in: b66076e51b ... user: stephan tags: trunk) | 
| 18:17 |  | Add new configure --with-readline-ldflags/cflags/header flags as brute-force method for clients to tell configure how to compile and link against readline. (check-in: eaa3a8053e ... user: stephan tags: trunk) | 
| 18:09 |  | Add test case for fts5 trigram tokenizer. (check-in: ba358d265b ... user: dan tags: trunk) | 
| 17:47 |  | In Makefile.msc, build jimsh0.exe locally, not in the source directory. (check-in: 1b9eb4564b ... user: drh tags: trunk) | 
| 16:04 |  | Remove LDFLAGS_ZLIB from sqlite3.pc.in because -lsqlite3 already includes that. (check-in: 47e50fa84d ... user: stephan tags: trunk) | 
| 16:03 |  | Patch bundle accumulated via /chat: add missing --enable/disable-shared flag to configure script and update testrunner.tcl for other configure script flag changes. (check-in: 32fc9c3f62 ... user: stephan tags: trunk) | 
| 11:18 |  | Fix a bug in the computation of LDFLAGS.libsqlite3 introduced by [63218898ed0a6d46]. (check-in: 58373d523c ... user: drh tags: trunk) | 
| 04:31 |  | Add missing B.exe extension to src-verify, as reported in the forum. (check-in: 2801fb6507 ... user: stephan tags: trunk) | 
| 03:30 |  | Doc typo fix. (check-in: da50e85d9a ... user: stephan tags: trunk) | 
| 03:22 |  | Rephrase 'older' as 'legacy' in the docs from [0a50e33051] and [2b2ca7dec18d]. (check-in: d212cc3682 ... user: stephan tags: trunk) | 
| 03:19 |  | Expand the libtool-style link support from [0a50e33051] such that if INSTALL_SO_086_LINKS=1 is passed to 'make install' then the libtool-style names are always linked in, regardless of whether they already existed or not. In either case, we unconditionally remove libsqlite3.la because it cannot work with the newly-installed non-libtool .so file. (check-in: 2b2ca7dec1 ... user: stephan tags: trunk) | 
| 03:05 |  | Disable generation of config.defines.json until/unless it proves interesting. Honor the --disable-largefile flag. When installing libsqlite3.so and an older-style libtool installation is found, re-link the libtool-generated files to the newly-installed ones to retain their historical names. (check-in: 0a50e33051 ... user: stephan tags: trunk) | 
| 2024-10-25 |  |  | 
| 23:45 |  | Correct unresolved @LIBS@ placeholder in sqlite3.pc.in. (check-in: 63218898ed ... user: stephan tags: trunk) | 
| 23:25 |  | Use (cc-with {-includes stdint.h}) when checking for the various int types because, despite it being C99, it turns out that we do indeed use it if it's available. (check-in: 51a9278134 ... user: stephan tags: trunk) | 
| 23:13 |  | When --with-tclsh=X is used, use X for all TCL purposes, including in-tree code generation, per developer request. (check-in: 12498e55c0 ... user: stephan tags: trunk) | 
| 23:04 |  | Fix BUILD_CFLAGS (for B.cc) to not inherit CFLAGS (which are only for T.cc). (check-in: ffdce13deb ... user: stephan tags: trunk) | 
| 23:03 |  | Experimentally: when ./configure CC=foo is used in a non-cross-compilation build and CC_FOR_BUILD is not explicitly provided, force CC_FOR_BUILD to default to CC. This is debatable - see the code comments for the justification. (check-in: a49bee6841 ... user: stephan tags: trunk) | 
| 22:44 |  | Rename VERSION_XYZ to the more conventional PACKAGE_VERSION and remove the unused VERSION_XY. (check-in: 2d2f6dfdeb ... user: stephan tags: trunk) | 
| 15:28 |  | Update the compile-for-*.md documents. (check-in: c4da7fa279 ... user: drh tags: trunk) | 
| 14:39 |  | Rework the configure+make system to use autosetup rather than autoconf. Autosetup runs faster and is easier to maintain, and it allows for common targets (such as "sqlite3" and "sqlite3.c") to be built within having to install "tclsh". (check-in: d8c0e01842 ... user: drh tags: trunk) | 
| 14:36 |  | Restore the amalgamation-tarball and snapshot-tarball targets, at least for now. (closed check-in: 1baf4b9488 ... user: drh tags: autosetup) | 
| 13:56 |  | Minor auto.def cleanups and doc addtions. (check-in: ea53aba82b ... user: stephan tags: autosetup) | 
| 12:22 |  | New jimsh0.c file for autosetup. (check-in: cfacf86411 ... user: drh tags: autosetup) | 
| 12:06 |  | Update the LICENSE.md file to describe BSD-licensed code that is included in the repository as a convenience to developers but which is not itself actually a part of SQLite. (check-in: deb5994bb1 ... user: drh tags: autosetup) | 
| 05:53 |  | Ensure that jimsh is built in the proper dir in an out-of-tree build and that jimsh0 is cleaned up by distclean. (check-in: e0286e258b ... user: stephan tags: autosetup) | 
| 04:48 |  | Rename config-defines.json to config.defines.json for consistent with other generated config.* files. Rename --defs-json-include-lowercase to the even less wieldy --defines-json-include-lowercase. (check-in: 131a2a7ef8 ... user: stephan tags: autosetup) | 
| 04:39 |  | Add --defs-json-include-lowercase configure flag to cause config-defs.json to include lower-case defines, which are primarily the various build-related system paths. (check-in: 0f2555ea3c ... user: stephan tags: autosetup) | 
| 04:16 |  | Internal cleanups and docs in hwaci-common.tcl. (check-in: 30699b57d2 ... user: stephan tags: autosetup) | 
| 03:56 |  | Docs related to the -array defs-dump option. Rename some internal-use hwaci-common.tcl APIs after discovering that a - as a symbol suffix fails for var derefs (but works for procs calls). (check-in: 0d5da3888e ... user: stephan tags: autosetup) | 
| 03:35 |  | Experimentally add the -array formatting option to hwaci-dump-defs-json and emit OPT_FEATURE_FLAGS/OPT_SHELL in both flat string and array forms. (check-in: ce6bc4603c ... user: stephan tags: autosetup) | 
| 03:18 |  | ./configure now emits config-defs.json, a JSON-formatted counterpart of the DEFS=... info which the pre-built autotools bundles emit. (check-in: 800b083c3a ... user: stephan tags: autosetup) | 
| 2024-10-24 |  |  | 
| 19:56 |  | The "WITHOUT_JIMSH=1" option on the nmake command-line forces the use of standard TCL for building, instead of jimsh0. (check-in: 9f861f66e2 ... user: drh tags: autosetup) | 
| 19:17 |  | Basic builds now appear to work using jimsh0 on Windows. (check-in: d8f5193970 ... user: drh tags: autosetup) | 
| 17:16 |  | Fix a bug in jimsh0.c for when it is compiled using MSVC. (check-in: f7def0be74 ... user: drh tags: autosetup) | 
| 15:58 |  | Add missing ZERO_ARGUMENT_GENERATE_SERIES checks to ext/misc/series.c, as reported via support mail. (check-in: 5a8e3915ee ... user: stephan tags: trunk) | 
| 15:57 |  | Add missing ZERO_ARGUMENT_GENERATE_SERIES checks to ext/misc/series.c, as reported via support mail. (closed check-in: cd82e4c0f5 ... user: stephan tags: ifdef-SQLITE_CORE) | 
| 15:57 |  | Fix the new tool/cp.tcl so that it works with older TCL versions, such as jimtcl. (check-in: 61f18c9618 ... user: drh tags: autosetup) | 
| 15:36 |  | Fix harmless compiler warnings. One of the warnings was code that deliberately committed memory errors to test the systems ability to cope. But compilers don't allow that any more, so we'll have to leave that capability untested. (check-in: 7e7b3b2edb ... user: drh tags: autosetup) | 
| 15:14 |  | Add the tool/cp.tcl script and use it to simplify Makefile.msc. (check-in: b7db2146a8 ... user: drh tags: autosetup) | 
| 14:33 |  | Back out [bd66222721] because it causes conflicts with test runs, apparently due to .o files being built with different flags. This means that the CLI shell currently does not honor --disable-amalgamation. (check-in: b063317352 ... user: stephan tags: autosetup) | 
| 13:27 |  | Tweaks to help make jimsh0 usable to build using nmake. Does not quite work yet, but getting closer. (check-in: e911303b8d ... user: drh tags: autosetup) | 
| 12:57 |  | Minor makefile tweaks to account for customizations via a custom hand-built makefile. (check-in: 7f92e820e6 ... user: stephan tags: autosetup) | 
| 12:18 |  | Fix jimsh0.c so that it compiles using MSVC: "cl jimsh0.c" (check-in: f531825d4e ... user: drh tags: autosetup) | 
| 12:06 |  | Fix an inconsistency in the way SQLITE_CORE is used in fts3.c. (check-in: d4816e534a ... user: dan tags: trunk) | 
| 12:03 |  | Fix the sqlite3(.EXE) build to honor --disable-amalgamation. (check-in: bd66222721 ... user: stephan tags: autosetup) | 
| 11:20 |  | Merge all the latest trunk enhancements into the autosetup branch. (check-in: d0554ac468 ... user: drh tags: autosetup) | 
| 11:11 |  | Update the version number for the TEA extension. (check-in: 56353bcffa ... user: drh tags: autosetup) | 
| 07:44 |  | Beginnings of rephrasing #if SQLITE_CORE to #ifdef/ifndef for consistency, as discussed in forum post cea40371c5e34b09. (check-in: 58d91abf0c ... user: stephan tags: ifdef-SQLITE_CORE) | 
| 07:31 |  | Various build cleanups centered around straightening out various uses of CFLAGS and its cousins. Teach Makefile.linux-generic to figure out the TOP dir on its own (a GNU Make-ism, but it's a Linux-specific makefile). (check-in: 58a0f7e79c ... user: stephan tags: autosetup) | 
| 05:33 |  | More potentially-controversial makefile symbol renaming. This is simply more readable to my eyes. (check-in: 6d4d1d5fef ... user: stephan tags: autosetup) | 
| 05:03 |  | Bump version number to 3.48.0 (in this branch only - in trunk, doing so requires a specific autoconf version) and rename the RELEASE and VERSION makefile symbols for clarity's sake. (check-in: 4193d90f21 ... user: stephan tags: autosetup) | 
| 04:56 |  | Potentially controversial/unconventional makefile symbol renamings and docs explaining them. (check-in: 7eceb7539d ... user: stephan tags: autosetup) | 
| 04:34 |  | Generic build cleanups. (check-in: be7b32a77f ... user: stephan tags: autosetup) | 
| 03:50 |  | Get Makefile.linux-generic (formerly Makefile.linux-gcc) working with jimsh in out-of-tree builds. Pass on -DHAVE_READLINE=1 to the sqlite3 shell if configure detects it. (check-in: a555ff6dbc ... user: stephan tags: autosetup) | 
| 03:14 |  | General make cleanups. Start adding a sanity-check mechanism to main.mk which does basic validation of the vars it expects to be set by the file which includes it. Get Makefile.linux-gcc working for the core-most rules. (check-in: 85b2c73ccb ... user: stephan tags: autosetup) | 
| 01:26 |  | Rename some build vars for legibility. Fix hwaci-make-from-dot-in when the input file list is multi-line. (check-in: fdb5844215 ... user: stephan tags: autosetup) | 
| 00:02 |  | Fix out-of-tree builds configured with --disable-amalgamation. (check-in: 182dac1c46 ... user: stephan tags: autosetup) | 
| 2024-10-23 |  |  | 
| 17:40 |  | Remove unused configure check for clang-18. (check-in: d73ac7286a ... user: stephan tags: autosetup) | 
| 17:17 |  | Remove three autotools files which are not needed in the autosetup port. (check-in: fc20861443 ... user: stephan tags: autosetup) | 
| 16:59 |  | Fix a #define typo in jimsh0.c, eliminating the need to explicitly pass -DJIM_COMPAT when building it. This fix has since made its way upstream. (check-in: 9b105abf6f ... user: stephan tags: autosetup) | 
| 16:46 |  | After compiling jimsh, run sanity tests on it to ensure that it is built with -DJIM_COMPAT and one of (-DHAVE_REALPATH, -DHAVE__FULLPATH). (check-in: ea6a14a6e6 ... user: stephan tags: autosetup) | 
| 16:03 |  | Work around a minor JimTCL regexp incompatibility in tool/vdbe-compress.tcl. Summary: it thinks that backslash-escaped octal values are back-references, which it does not like. (check-in: aeac23359b ... user: stephan tags: autosetup) | 
| 15:13 |  | Remove the configure-script VERSION check from tool/srctree-check.tcl, as it's not relevant in the autosetup build. (check-in: 6c5826d752 ... user: stephan tags: autosetup) | 
| 15:05 |  | Add missing export of LDFLAGS_DLOPEN to Makefile.in. (check-in: d18af84bf7 ... user: stephan tags: autosetup) | 
| 11:33 |  | Reorder conditions in sqlite3PagerDirectReadOk() for coverage. (check-in: da9124fee2 ... user: drh tags: trunk) | 
| 11:06 |  | Fix harmless compiler warnings in wherecode.c. One such warning was identified by forum post 721675f007 and the other was found by tool/warnings.sh. (check-in: 987b96aa63 ... user: drh tags: trunk) | 
| 10:36 |  | Add two new #include statements to the composite "fts5.c" file that is constructed as part of the build process. These #includes are no-ops in the SQLite amalgamation (and are commented out automatically by the amalgamation builder) but are needed if the FTS5 extension is built separately, it seems. Enhancement request. (check-in: 1bf8daeb24 ... user: drh tags: trunk) | 
| 02:36 |  | Another fix for autosetup/autosetup-find-tclsh for the case where a tclsh is found on the system. (check-in: 19d78fcef4 ... user: stephan tags: autosetup) | 
| 02:22 |  | Latest autosetup/autosetup-find-tclsh from autosetup ticket #67. (check-in: 0e33f6cd48 ... user: stephan tags: autosetup) | 
| 02:17 |  | Reorder some auto.def code to avoid breaking --help. (check-in: 151d7dfc8d ... user: stephan tags: autosetup) | 
| 02:11 |  | Minor cleanups in the --with-wasi-sdk bits. (check-in: d952c17981 ... user: stephan tags: autosetup) | 
| 01:59 |  | Get auto.def handling the case that no tclsh is installed in a default location. If not, use jimsh if we can find realpath(), else bail out. (check-in: 359741bc76 ... user: stephan tags: autosetup) | 
| 01:33 |  | Fix /dev/null and stderr redirection ordering in autosetup/autosetup-find-tclsh so that the extraneous strlcpy()/snprintf()/rand() warning output from ld on OpenBSD does not break detection of jimsh0. (check-in: cd447e50d1 ... user: stephan tags: autosetup) | 
| 01:18 |  | Use pkg-config for readline detection, if available. Unrelated minor tcl cleanups. (check-in: 3a41ac08a4 ... user: stephan tags: autosetup) | 
| 2024-10-22 |  |  | 
| 20:16 |  | Changes to EXPLAIN QUERY PLAN to identify covering index plan that use indexes on expressions. (check-in: 29fb4919b6 ... user: dan tags: trunk) | 
| 20:09 |  | Add the "halt" command to testrunner.tcl (check-in: 2cc25d5dbb ... user: drh tags: trunk) | 
| 19:33 |  | Add the SQLITE_IOCAP_SUBPAGE_READ bit to the possible returns values from xDeviceCharacteristics method of the system-IO abstract class. The direct-overflow-read optimization is disabled for any VFS that does not set this bit. The bit is set for standard VFSes. (check-in: 6e7d498cda ... user: drh tags: trunk) | 
| 18:26 |  | Rename to SQLITE_IOCAP_SUBPAGE_READ. (closed check-in: dd446ef181 ... user: drh tags: bypass-iocap) | 
| 18:00 |  | Add the SQLITE_IOCAP_BYPASS device characteristic. Do not allow the SQLITE_DIRECT_OVERFLOW_READ optimization if that capability is missing. (check-in: f50ae00ce9 ... user: drh tags: bypass-iocap) | 
| 17:57 |  | Minor build docs and cleanups. (check-in: 0eb96ea2d1 ... user: stephan tags: autosetup) | 
| 16:19 |  | Do not typedef Tcl_Size if it is already #defined. (check-in: 53491688cf ... user: drh tags: trunk) | 
| 13:02 |  | Minor cleanups to how configure generates tool/emcc.sh. (check-in: 4484b46236 ... user: stephan tags: autosetup) | 
| 12:56 |  | Teach ext/wasm/GNUmakefile to use tool/emcc.sh and fix a syntax error in tool/emcc.sh.in. Work around a JimTCL incompatibility in tool/mkshellc.tcl. (check-in: 24e0f6ecc6 ... user: stephan tags: autosetup) | 
| 10:29 |  | Further changes to sqlite3_rsync.c to work around Windows issues. (check-in: e2bd3219d9 ... user: drh tags: trunk) | 
| 09:43 |  | Set I/O mode to binary in Windows in sqlite3_rsync. (check-in: 6717528744 ... user: drh tags: trunk) | 
| 03:56 |  | After discussing [4d4423df8d14] with Steve Bennett, do not prepend $prefix/bin to the search path for binaries, as that path is commonly used for cross-compiled targets and we want binaries which will run on the build host. (check-in: f2008a7d79 ... user: stephan tags: autosetup) | 
| 03:33 |  | Touch configure-generated files at configure-time even if autosetup does not update them because their contents would not be changed. Works around wonky deps causing too-frequent rebuilds. (check-in: 339b48af17 ... user: stephan tags: autosetup) | 
| 03:12 |  | Rename hwaci-error to the more descriptive hwaci-fatal. Use autosetup's file-isexec instead of [file executable] for portability. Remove the binary file lookup cache - unnecessary complexity. When searching for tools like tclsh, check under $prefix/bin before checking the $PATH. This seems like the right thing to do, but the fact that autosetup's file-search API's do not do that by default leaves some room for doubt about the wisdom of this change. (check-in: 4d4423df8d ... user: stephan tags: autosetup) | 
| 2024-10-21 |  |  | 
| 22:45 |  | In testrunner.tcl: Better estimates for ETC (Estimated Time to Completion). Show the ETC even for the single-line status reports. (check-in: 2a2f5f4e37 ... user: drh tags: trunk) | 
| 22:15 |  | Bring checksymbols target up to date and add a TODO based on a feature request from the forum. (check-in: c00a03256b ... user: stephan tags: autosetup) | 
| 21:53 |  | Add sqlite3rebaser_... to the API symbols accepted by the 'checksymbols' makefile target. (check-in: 0284590f21 ... user: stephan tags: trunk) | 
| 21:32 |  | Resolve two build FIXMEs. (check-in: 4de51c165c ... user: stephan tags: autosetup) | 
| 21:22 |  | Get most of the numerous misc tool binaries building. (check-in: fa74cbb40c ... user: stephan tags: autosetup) | 
| 20:12 |  | Simplify how the targets which may or may not be enabled, depending on config flags, are formulated in main.mk. (check-in: 7bea793ce4 ... user: stephan tags: autosetup) | 
| 19:51 |  | More build doc tweaks and get ./startup building. (check-in: cee285029c ... user: stephan tags: autosetup) | 
| 17:50 |  | Start moving most Makefile.in docs over to main.mk. Fix compilation of tclsqlite.c. (check-in: 5b154e08ab ... user: stephan tags: autosetup) | 
| 16:30 |  | Version 3.47.0 (check-in: 03a9703e27 ... user: drh tags: trunk, release, major-release, version-3.47.0) |
