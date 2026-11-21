---
id: collect-261121-veille-ia/langages-dev/sqlite-2
title: "SQLite"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-06-14"]
keywords: ["check-in", "user", "tags", "trunk", "sqlite", "buffer", "using", "could"]
source: https://www.sqlite.org/src/timeline?bt=version-3.47.0&p=trunk&advm=1&udc=1
source_anchor: ""
source_lines: [1501, 3000]
section: "Langages_Dev"
---
| 20:10 |  | Do not allow ridiculous "columns=N" values in the (unused) csv virtual table in the extensions folder. Bug 2026-06-14T15:22:47Z (check-in: ecae4a2ddf ... user: drh tags: trunk) | 
| 2026-06-13 |  |  | 
| 20:16 |  | Speed up SQL aggregate functions percentile() and median() by avoiding a full sort of the array of arguments. (check-in: 9232b5f21d ... user: dan tags: pecentile-partial-sort) | 
| 18:04 |  | Make the --header option to the CLI sticky, so that it does not get turned off by subsequence --csv or similar mode change options. Bug 2026-06-13T11:49:18Z (check-in: b19cef1862 ... user: drh tags: trunk) | 
| 2026-06-12 |  |  | 
| 18:35 |  | When reading a super-journal name from a journal file, allocate a new buffer rather than using Pager.pTmpSpace. This prevents a buffer overrun that could occur when using a VFS with a large sqlite3_vfs.mxPathname value with a database with a small page size. (check-in: 7f71859841 ... user: dan tags: trunk) | 
| 17:32 |  | Fix errors in comments on this branch. Also prefer SQLITE_NOMEM_BKPT to SQLITE_NOMEM. (closed check-in: ac17669e84 ... user: dan tags: super-journal-malloc) | 
| 17:16 |  | Fix a memory leak that could follow an OOM error in the new code on this branch. (check-in: d4774a8987 ... user: dan tags: super-journal-malloc) | 
| 16:47 |  | Improved documentation for sqlite3_deserialize(). (check-in: f15d076820 ... user: drh tags: trunk) | 
| 16:26 |  | Do not use shared locks on SHM files when the filename is a long DOS-device path. Only used shared locks for UNC paths. Do not confuse the long-DOS-device syntax with UNC paths. (check-in: a4691489f2 ... user: drh tags: trunk) | 
| 15:36 |  | Fix a buffer overwrite in fts3 that could occur while processing NEAR queries against corrupt records. Bug 2026-06-11T23:11:26Z. (check-in: b0f773e99e ... user: dan tags: trunk) | 
| 12:15 |  | When using a read-only WAL/SHM, handle a corrupt page size in the WAL file correctly. Bug 2026-06-11T22:20:03Z. (check-in: 09ab0a1bac ... user: drh tags: trunk) | 
| 11:55 |  | Add the --raw option to the dbtotxt command-line tool, for debugging. (check-in: abfecc0820 ... user: drh tags: trunk) | 
| 11:24 |  | Fix a signed integer overflow that could occur in fts3 when processing corrupt database records. Bug 2026-06-11T23:12:25Z. (check-in: 978d04f051 ... user: dan tags: trunk) | 
| 2026-06-11 |  |  | 
| 23:11 |  | Comment improvements on the ".ar -x" command of the CLI. No changes to code. (check-in: 5b939fb1a2 ... user: drh tags: trunk) | 
| 22:48 |  | Fix harmless compiler warnings. (check-in: dd0c161fcd ... user: drh tags: trunk) | 
| 16:39 |  | Further improvements to the ".ar" command of the CLI. (check-in: 6149b938c7 ... user: drh tags: trunk) | 
| 14:28 |  | Three-pass algorithm for the ".ar x" command in the CLI, so that symlinks are created after all regular files are created. (check-in: 37a80d20fe ... user: drh tags: trunk) | 
| 14:14 |  | Add the (undocumented) --debug option to the ".ar" command in the CLI (check-in: 3a040ffc47 ... user: drh tags: trunk) | 
| 01:31 |  | Improved detection of corrupt node IDs in RTree shadow tables. Test case in TH3. (check-in: e01a185a69 ... user: drh tags: trunk) | 
| 2026-06-10 |  |  | 
| 20:10 |  | When reading a super-journal name from a journal file, allocate a new buffer rather than using Pager.pTmpSpace. This prevents a buffer overrun that could occur when using a VFS with a large sqlite3_vfs.mxPathname value with a database with a small page size. (check-in: 37aa82a50a ... user: dan tags: super-journal-malloc) | 
| 16:51 |  | Fix a signed-integer overflow in fts5 that might occur when dealing with strategicly corrupted records. Bug 2026-06-10T03:56:42Z. (check-in: fc6442ee54 ... user: dan tags: trunk) | 
| 10:40 |  | Detect an OOM condition in the realpath() function of the fileio.c extension and cause that function to return NULL. To Do: we should go back in and fix realpath() to raise an SQLITE_NOMEM error on OOM rather than returning NULL. But we will delay that, in as much as OOMs are all but impossible on modern OSes. Bug 2026-06-10T07:46:32Z. (check-in: 8b961dc3d2 ... user: drh tags: trunk) | 
| 10:13 |  | Fix a possible signed integer overflow in the RBU extension given a maliciously crafted delta. Bug 2026-06-10T06:41:54Z. (check-in: 8531c0c3b6 ... user: drh tags: trunk) | 
| 09:51 |  | Harden code that processes Fossil Deltas against OOM and maliciously malformed delta blobs. Bug 2026-06-10T07:01:00Z and Bug 2026-06-10T07:06:43Z. (check-in: 67271c3129 ... user: drh tags: trunk) | 
| 2026-06-09 |  |  | 
| 10:43 |  | Avoid a possible integer overflow when fts5 tokenizes a very large document. Only possible with non-standard builds that use large values of SQLITE_MAX_LENGTH. Bug 2026-06-09T05:27:16Z. (check-in: d562e91374 ... user: dan tags: trunk) | 
| 10:16 |  | Change loop counter variables from int to i64 to avoid a potential integer overflow inside an assert() statement when SQLite is compiled with SQLITE_DEBUG and an extra-large SQLITE_MAX_LENGTH. Does not affect production builds. Bug 2026-06-09T08:53:14Z. (check-in: 77f615d983 ... user: drh tags: trunk) | 
| 2026-06-08 |  |  | 
| 18:15 |  | Fix builds on i486 CPUs. Forum 2025-01-25T12:58:44Z. (check-in: 4cb349370d ... user: drh tags: trunk) | 
| 17:48 |  | Fix another buffer overread in fts5 when processing corrupt records. Bug 2026-06-08T16:03:39Z . (check-in: 8ba5acabf9 ... user: dan tags: trunk) | 
| 15:20 |  | Fix a buffer overread that could occur in fts5 when processing corrupt records. Bug 2026-06-08T11:15:52Z (check-in: b07441cfc0 ... user: dan tags: trunk) | 
| 12:03 |  | Avoid a potential use-after-free in fts5. Report 2026-06-08T08:45:27Z. (check-in: 9c018b02db ... user: dan tags: trunk) | 
| 11:24 |  | Clamp the nToken parameter to the fts5 snippet() function between 0 and 64. It has always been documented this way, but not previously implemented. Report 2026-06-08T08:29:00Z. (check-in: 4af1d9b3e5 ... user: dan tags: trunk) | 
| 10:47 |  | Add extra tests to check that non-deterministic functions may not be used in indexes. (check-in: beeef9dc2b ... user: dan tags: trunk) | 
| 2026-06-06 |  |  | 
| 18:32 |  | Improvements to the GCC compiler bug work-around of check-in [7fae321095ebec77]. (check-in: 247894f70d ... user: drh tags: trunk) | 
| 17:20 |  | Minor performance enhancement and improved comments on the quicksort algorithm used by the median() implementation in func.c. (check-in: 2800f6a75b ... user: drh tags: trunk) | 
| 2026-06-05 |  |  | 
| 00:11 |  | A new approach to working around the GCC bug. (closed check-in: 2f95186ea5 ... user: drh tags: gcc-bug-114659) | 
| 2026-06-04 |  |  | 
| 16:56 |  | Disable the vector-IN-SELECT optimization if the number of columns is so large that it would cause the WhereTerm.nChild column to wrap. Bug 2026-06-04T10:00:49Z. (check-in: bb49dfc948 ... user: drh tags: trunk) | 
| 11:48 |  | Fix a crash that could be caused by configuring the pager-cache with a bulk allocation too small to fit even one page. Bug 2026-06-04T07:03:12Z. (check-in: b4b9dc632b ... user: dan tags: trunk) | 
| 11:42 |  | Fix a possible NULL pointer derefence in the (experimental and untested) uuid.c extension. Bug 2026-06-04T09:50:59Z. (check-in: fa6374fe3a ... user: drh tags: trunk) | 
| 11:39 |  | Extra testcase() macros to ensure adequate testing of the previous check-in. (check-in: 003013ccab ... user: drh tags: trunk) | 
| 11:19 |  | Fix problems with expressions like "(?,?,?) IN (SELECT c,b,a FROM ...)" when there is an index on columns "c", "b" and "a", but not in that order. Bug 2026-06-04T07:02:53Z. (check-in: 07667afd0f ... user: dan tags: trunk) | 
| 11:18 |  | Clearly mark the ext/misc/wholenumber.c extension as "testing and debugging use only". Fix a potential integer overflow that can occur if the above warning is ignored. Bug 2026-06-04T09:40:28Z. (check-in: 44154ea9ae ... user: drh tags: trunk) | 
| 2026-06-03 |  |  | 
| 17:21 |  | Update the utf-8 decoder in the ICU extension to treat invalid sequences as codepoint 0xFFFD, matching the core. Report 2026-06-03T04:04:46Z. (check-in: 840be0ee62 ... user: dan tags: trunk) | 
| 16:46 |  | Fix a faulty assert() in rtree that could fire when processing an unusually large record. Bug 2026-06-03T14:44:46Z. (check-in: e7335dd377 ... user: dan tags: trunk) | 
| 16:21 |  | Use tail recursion on the larger of the two partitions when doing a quicksort as part of the implementation of the median() function, to avoid excess stack usage on pathological inputs. Bug 2026-06-03T07:26:42Z. (check-in: 7dc987165c ... user: drh tags: trunk) | 
| 15:29 |  | Fix an integer overflow problem in fts3 that could lead to a buffer overwrite on platforms where size_t is 32-bits. Bug 2026-06-03T04:28:51Z. (check-in: ef0c66d2a2 ... user: dan tags: trunk) | 
| 14:58 |  | Fix another buffer overread in fts5 that could occur when processing corrupt records. Bug 2026-06-03T03:54:40Z. (check-in: b5337c87cc ... user: dan tags: trunk) | 
| 13:49 |  | Fix another potential buffer overrun that could occur in fts5 when processing corrupt records. (check-in: 6ee44b1995 ... user: dan tags: trunk) | 
| 13:12 |  | Fix a potential UAF bug in the zipfile extension. Bug 2026-06-03T10:58:51Z. (check-in: 1fb5e9169a ... user: drh tags: trunk) | 
| 12:43 |  | Fix negative array index in "testing and debugging uses only" extension qpvtab.c. Bug 2026-06-03T07:26:26Z. (check-in: d066699fca ... user: drh tags: trunk) | 
| 11:37 |  | Avoid excessive recursion and stack overflow in fts3 when processing a corrupt database. (check-in: 2136937876 ... user: dan tags: trunk) | 
| 11:07 |  | The prefix_length() function should stop at the first NUL terminator. Bug 2026-06-03T07:42:00Z. (check-in: a9b8bdea06 ... user: drh tags: trunk) | 
| 10:55 |  | Make the (unused, untested, and unsupported) ext/misc/compress.c routines responsive to OOM conditions. Bug 2026-06-03T08:28:36Z. (check-in: e3120e2a43 ... user: drh tags: trunk) | 
| 2026-06-02 |  |  | 
| 15:20 |  | Fix a case where sqlite3expert could be tricked into executing arbitrary SQL by a corrupt database schema. (check-in: 8a633070e6 ... user: dan tags: trunk) | 
| 11:11 |  | Add extra test case to zipfile.test. No code changes. (check-in: 83fe72bcdf ... user: dan tags: trunk) | 
| 10:08 |  | Fix the CLI so that it works when compiled using STDCALL on Microsoft x86. Change should not affect other platforms. Forum 2026-06-02T09:44:12Z. (check-in: 83adece349 ... user: drh tags: trunk) | 
| 2026-06-01 |  |  | 
| 20:02 |  | Enhance PRAGMA integrity_check so that it better detects incorrect header sizes at the beginning of cells. (check-in: 08bb2878e7 ... user: drh tags: trunk) | 
| 18:01 |  | Disable transitive WHERE constraints when there are explicit COLLATE operators. Fix for report 2026-05-31T08:33:10Z. (check-in: 18ee3a17c5 ... user: dan tags: trunk) | 
| 17:56 |  | Minor performance improvement for the patch on this branch. (closed check-in: 2702fecc5c ... user: dan tags: transitive-where-collate-bugs) | 
| 17:40 |  | Fix minor problems surrounding transitive WHERE constraints and explicit COLLATE terms. (check-in: 9500238a1d ... user: dan tags: transitive-where-collate-bugs) | 
| 16:49 |  | Make sure the authenticator is called for columns that are part of a USING in a FULL JOIN. Bug 2026-05-31T02:00:37Z. (check-in: 4ef88a36f9 ... user: drh tags: trunk) | 
| 16:20 |  | For expressions of the form "(x, y...) IN (SELECT a, b ...)" where the result is not true, consider the collation sequences of columns "a" and "b" when determinining if the result should be false or NULL. Bug report 2026-05-31T02:10:44Z. (check-in: 04d7a9d788 ... user: dan tags: trunk) | 
| 15:03 |  | Better handle a (possibly) potential buffer overrun in fts5. Based on the report in 2026-05-31T09:06:13Z. (check-in: 9d0e394080 ... user: dan tags: trunk) | 
| 13:14 |  | Remove the data type ANY added to the shell's CSV imports in [aff74e71ea734e1a], as discussed in forum post 2026-06-01T12:01:59Z. (check-in: a6a347aec5 ... user: stephan tags: trunk) | 
| 12:36 |  | Do not allow the argument to a table-valued function to reference a table to its right, even if the SELECT is inside a common table expression. This is a replacement for the fix at [3c0a277e6741c722]. Fix for multiple dbsqlfuzz reports and probably also a fix for Bug 2026-06-01T07:23:11Z. (check-in: d96271db6a ... user: drh tags: trunk) | 
| 12:30 |  | Test case to show that the change fixes the original carray problem. (closed check-in: 8df6671ae8 ... user: drh tags: right-ref-cte) | 
| 12:19 |  | Improvements to sqlite3SelectCheckOnClauses() to be more accurate in identifying table-valued functions that references tables to the right. (check-in: e5bae3687d ... user: drh tags: right-ref-cte) | 
| 12:07 |  | Back out the recent change to carray. The intent is to fix this problem in a different way so that it works for all table-valued functions. (check-in: 43e02dbb0d ... user: drh tags: right-ref-cte) | 
| 11:52 |  | Fix an integer overflow that could lead to a buffer overrun in the zipfile extension. Bug 2026-06-01T10:16:47Z. (check-in: a8dac6af35 ... user: dan tags: trunk) | 
| 11:50 |  | Update new test in zipfile.test so that it does not run if SQLITE_MAX_LENGTH is too small. (closed check-in: cc86820e19 ... user: dan tags: zipfile-buffer-overrun) | 
| 10:56 |  | Fix an integer overflow that could lead to a buffer overrun in the zipfile extension. Bug 2026-06-01T10:16:47Z. (check-in: 7c740d2cfd ... user: dan tags: zipfile-buffer-overrun) | 
| 10:53 |  | Extra defenses against integer overflows in the untested, unused, and unsupported transliterate() SQL extension function found in the ext/misc/spellfix.c extension. Bug 2026-06-01T10:34:57Z. (check-in: 2b073519b6 ... user: drh tags: trunk) | 
| 2026-05-31 |  |  | 
| 19:41 |  | Fix the carray virtual table so that it gives no solution if it cannot find a usable first parameter. dbsqlfuzz find. (check-in: 3c0a277e67 ... user: drh tags: trunk) | 
| 17:01 |  | Test case to cover the bug fix in the previous check-in. (check-in: 5b28d49b61 ... user: drh tags: trunk) | 
| 15:49 |  | Fix the zipfile extension so that ZIP archives containing files whose names contain embedded \000 bytes do not cause problems. Bug 2026-05-31T11:43:05Z. (check-in: c12ff342a9 ... user: drh tags: trunk) | 
| 09:38 |  | Fiddle should default to using NO_COLOR since the JS console does not support ANSI X3.64. (check-in: bde6190fde ... user: drh tags: trunk) | 
| 09:18 |  | Fix the format() SQL function so that it reports TOOBIG and NOMEM errors. Fix a possible integer overflow on %#Q formatting. Bug 2026-05-31T02:00:07Z. (check-in: 3bfe0510ae ... user: drh tags: trunk) | 
| 2026-05-30 |  |  | 
| 13:23 |  | Add a test to ensure an application does not try to create a geopoly virtual table with too many columns. Bug 2026-05-30T12:47:27Z. (check-in: 2c605bfb15 ... user: drh tags: trunk) | 
| 10:24 |  | Harden the diskused.c extension against NULL pointer deferences that might have previously occurred when given a corrupt database file. Bug 2026-05-30T07:08:55Z. (check-in: 7487a1c59d ... user: drh tags: trunk) | 
| 10:09 |  | Remove the untested and undocumented "scrub" utility program. It was written in 2016, but made obsolete by the VACUUM INTO command in 2019. (check-in: 27d67fb175 ... user: drh tags: trunk) | 
| 2026-05-29 |  |  | 
| 23:14 |  | Be sure to do floating-point to text conversions in JSON to the full 17-digits. (check-in: 61a776650c ... user: drh tags: trunk) | 
| 14:57 |  | Rename the ext/misc/analyze.c extension to ext/misc/diskused.c, to avoid confusion with the src/analyze.c file. The function is now called "diskused(X)" instead of "analyze(X)". The CLI command is renamed from ".dbstat" to ".diskused". (check-in: c7839f7a17 ... user: drh tags: trunk) | 
| 12:23 |  | The -csv option to the CLI also sets "-limits off", for legacy compatibility, and because that seems to make sense. Forum thread 2026-05-28T16:23:36Z. (check-in: d15cd64160 ... user: drh tags: trunk) | 
| 2026-05-28 |  |  | 
| 11:29 |  | Avoid a resource leak in an error path in the icu extension. (check-in: 87c37dab7e ... user: dan tags: trunk) | 
| 11:16 |  | Fix an infinite loop that can occur in the unused, untested, and unsupported ext/misc/btreeinfo.c extension if it is presented with a corrupt database file. Bug 2026-05-28T05:41:01Z. (check-in: 646e9b4915 ... user: drh tags: trunk) | 
| 11:12 |  | Fix a problem with shell tool function analyze() and schema objects with name containing "%" characters. (check-in: c6d5793d5d ... user: dan tags: trunk) | 
| 10:14 |  | Fix potential integer overflow in btree overflow page cache computation, reported by Project Fortify. Test cases in TH3. (check-in: dfa674d6e6 ... user: drh tags: trunk) | 
| 2026-05-26 |  |  | 
| 19:25 |  | Patch test/indexexpr1.test to work when built with SQLITE_DQS=0. Addresses forum post 2026-05-26T18:08:20Z. (check-in: b470a5d69e ... user: stephan tags: trunk) | 
| 17:15 |  | Fix an OOM check in shell.c.in. Addresses forum post 2026-05-26T15:02:35Z. (check-in: e78198e6ae ... user: stephan tags: trunk) | 
| 15:03 |  | Fix a potential 1-byte overread in sqlite3changeset_invert() when processing a corrupt buffer.
 We did not track how this problem was discovered, but it *might* have
 come from a email message on 2026-03-19 from a Korean "security researcher"
 who goes by the handle "UH4CK3R".  (check-in: 78eaa605cb ...  user: dan tags: trunk) | 
| 14:18 |  | Fix a 32-bit integer overflow in sqlite3changegroup_change_blob() that could lead to a buffer overwrite. (check-in: 8a289158e2 ... user: dan tags: trunk) | 
| 13:54 |  | Fix QRF so that it works sensibly with "--wrap 1" (check-in: 48f950b2a1 ... user: drh tags: trunk) | 
| 11:23 |  | Strength the defense against malicious command-line arguments in the sqlite3_rsync utility program. (check-in: c84d596b6d ... user: drh tags: trunk) | 
| 2026-05-25 |  |  | 
| 23:44 |  | Fix HTML ">" quoting in QRF. Bug f72dfbdeff. (check-in: 5cee1097b4 ... user: drh tags: trunk) | 
| 20:45 |  | Fix a minor problem with test case mjournal-3.3. (check-in: 80093dc061 ... user: drh tags: trunk) | 
| 19:47 |  | Early malformed JSON errors in json_each() and json_tree() to prevent excess output. (check-in: eb4d7e1b86 ... user: drh tags: trunk) | 
| 19:32 |  | Fix to testrunner.tcl so that it will run on FreeBSD. Forum post 2026-05-25T18:50:36Z (check-in: 97b66687b3 ... user: drh tags: trunk) | 
| 18:46 |  | Enhance the defenses against malformed JSONB in the jsonbPayloadSize() routine. (check-in: 32c9f71a98 ... user: drh tags: trunk) | 
| 18:14 |  | Use "-encoding iso8859-1" instead of "-encoding binary" in mjournal.test so that the script works with both Tcl 8 and 9. (check-in: 897b443fb3 ... user: dan tags: trunk) | 
| 17:58 |  | Fix a potential 1 byte overwrite that could occur when processing a carefully crafted super-journal file. (check-in: b3766c3afd ... user: dan tags: trunk) | 
| 17:09 |  | Fix a potential buffer overwrite that could occur when in fts5 when handling corrupt records. (check-in: de009593f6 ... user: dan tags: trunk) | 
| 2026-05-23 |  |  | 
| 12:14 |  | Make the /A substitution in the CLI prompt expansion responsive to the SQLITE_CLI_APPNAME compile-time option. (check-in: 624dc12e9e ... user: drh tags: trunk) | 
| 2026-05-22 |  |  | 
| 17:51 |  | Strengthen the statement about not accepting agentic code. (check-in: 64b707f4b3 ... user: drh tags: trunk) | 
| 14:29 |  | Update the session module so that it can apply changesets containing two or more UPDATE changes that form a dependency loop - so that no single UPDATE can be applied independently without violating a constraint. (check-in: 32c762bbb1 ... user: dan tags: trunk) | 
| 14:22 |  | Add the SQLITE_CHANGESETAPPLY_NOUPDATELOOP option to the sqlite3_changeset_apply_v2/3() method. To disable the extra processing to commit changesets that swap two or more values subject to a UNIQUE constraint between rows. (closed check-in: eba02092bc ... user: dan tags: session-update-loops) | 
| 11:15 |  | Add a prototype AGENTS.md file (check-in: 3dd1181120 ... user: drh tags: trunk) | 
| 2026-05-21 |  |  | 
| 15:14 |  | Fix the VFSes so that there is no 32-bit integer overflow in the xShmMap method if the WAL file size exceeds about 25 terabytes, or if the header is corrupted to make SQLite think that the WAL file size is that big. Bug 2026-05-21T03:53:03Z. (check-in: 9ac4a33a29 ... user: drh tags: trunk) | 
| 15:13 |  | Merge latest trunk changes with this branch. (check-in: c6226d508d ... user: dan tags: session-update-loops) | 
| 14:58 |  | Fix a buffer overread in the session module that could occur when processing a corrupt changeset. (check-in: 869a51ae84 ... user: dan tags: trunk) | 
| 14:48 |  | Performance optimization to the sqlite3ExprCollSeqMatch() routine. (check-in: 035f1d2f28 ... user: drh tags: trunk) | 
| 14:16 |  | Remove dead code created by [20c2f8ce9242c3db]. (check-in: 68c5fd5fa0 ... user: drh tags: trunk) | 
| 13:37 |  | Limit the length of inputs to the spellfix1_editdist() extension function (not part of the standard SQLite build and rarely used) to 99999 characters, to avoid any possibility of integer overflow. Bug 5da728247a. (check-in: 51ebf0e04e ... user: drh tags: trunk) | 
| 13:03 |  | A join constraint is not an equivalence unless both operands have the same collation. Bug 2026-05-21T03:39:28Z. Error goes back to version 3.7.17, 13 years ago. (check-in: 20c2f8ce92 ... user: drh tags: trunk) | 
| 12:04 |  | Enhance the xfer-optimization to verify access to the source table using the authorizer, if an authorizer is registered. Bug 2026-05-21T03:31:22Z. (check-in: d1cdb817ca ... user: drh tags: trunk) | 
| 2026-05-20 |  |  | 
| 20:03 |  | Improved entropy collection on Windows. (check-in: b734b76af3 ... user: drh tags: trunk) | 
| 15:06 |  | Fix a bug causing the session module to dereference a NULL pointer when applying a corrupt changeset. (check-in: e807d4e379 ... user: dan tags: trunk) | 
| 11:53 |  | Check-in [7cf841f3613c8302] was incomplete in that it did not remove the incorrect assert() statement. Fixed here. (check-in: c19bacca13 ... user: drh tags: trunk) | 
| 11:41 |  | Avoid returning an SQLITE_SCHEMA error from within sqlite3session_diff() if the schema has been recently modified. Bug report 269f1e9cef. (check-in: 20110547a1 ... user: dan tags: trunk) | 
| 01:32 |  | Fix a test case added by [1249eeb1b9759038] that does not work for the config=prepare permutation. (check-in: 6aaeeffc2c ... user: drh tags: trunk) | 
| 00:16 |  | Improvements to testrunner.tcl: Make C-program tests responsive to the pattern list. Do not remove build steps until the very end when we are certain that the build step is not required. The pattern list matches the exact text of the displayname column of the jobs table. (check-in: 3f409c0e1b ... user: drh tags: trunk) | 
| 2026-05-19 |  |  | 
| 20:27 |  | Fix a potential buffer overread in fts5 that could occur when handling corrupt records. (check-in: 70021c3291 ... user: dan tags: trunk) | 
| 19:33 |  | Silently ignore nested porter tokenizers in FTS5. Having nested porter tokenizers is pointless, but it does use stack space unnecessarily. Bug report a7766198f1. (check-in: 0bdeedf56c ... user: drh tags: trunk) | 
| 18:58 |  | Remove new unreachable code from this branch. (check-in: 07f81e256f ... user: dan tags: session-update-loops) | 
| 18:56 |  | Prevent integer overflow in the ICU extension. Bug report 844bdd7d5c. (check-in: c20cb75ca0 ... user: drh tags: trunk) | 
| 18:45 |  | Fix an assertion fault that can occur in RTree given a carefully corrupted database. Bug report 2026-05-18T06:46:01Z. (check-in: 7cf841f361 ... user: drh tags: trunk) | 
| 18:28 |  | Fix problems with error handling when applying changesets with update loops. (check-in: f797925df0 ... user: dan tags: session-update-loops) | 
| 17:15 |  | Merge trunk changes into this branch. (check-in: e480e126fc ... user: dan tags: session-update-loops) | 
| 17:12 |  | Avoid a potential 1 byte overread in sqlite3changegroup_add() when processing a corrupt changeset buffer. (check-in: 4d8c3a2919 ... user: dan tags: trunk) | 
| 17:05 |  | Add an sqlite3FaultSim() call to the changes at [6193e4105b6a58ea] to make them testable. (check-in: 0de3d95500 ... user: drh tags: trunk) | 
| 16:07 |  | Merge latest trunk changes into this branch. (check-in: 1f71b530c9 ... user: dan tags: session-update-loops) | 
| 16:07 |  | Fix a incorrect assert() from [52dd437d51d3c1af] reported by OSSFuzz. (check-in: 64b49e4d43 ... user: drh tags: trunk) | 
| 15:13 |  | Add the "make" subcommand to testrunner.tcl (check-in: 00d5468128 ... user: drh tags: trunk) | 
| 12:40 |  | Early detection of attempts to overwrite an in-use cache page due to database corruption. Chromium 513858286. (check-in: 6193e4105b ... user: drh tags: trunk) | 
| 11:15 |  | Security enhancements to the untested and unused fossildelta.c extension. Bug reports 3ac3fe3d71 and e7e470b760. (check-in: 2d3fbbe421 ... user: drh tags: trunk) | 
| 10:30 |  | Limit the size of input strings to the (disused) spellfix extension to avoid excessive runtime and integer overflows. Bugs report 2ba45960f4. (check-in: 4b16b80cf2 ... user: drh tags: trunk) | 
| 2026-05-18 |  |  | 
| 23:58 |  | Fix a NULL pointer dereference in the obscure (and unused) prefixes extension, which I had forgotten even existed. Bug report 24a33e5d10. (check-in: 4880adc053 ... user: drh tags: trunk) | 
| 21:45 |  | Fix an off-by-one error in matchinfo('b') for FTS3 when there are a large number of columns. Bug report 42d5f799d1. (check-in: c0f2cd6d29 ... user: drh tags: trunk) | 
| 20:43 |  | Limit the NEAR span in FTS3 to 1 billion, to avoid integer overflow. Bug report 4cfcf164d2. (check-in: adcb44f9dc ... user: drh tags: trunk) | 
| 20:34 |  | Fix a problem applying changesets that contain multiple UPDATE constraint loops. (check-in: 8e89aafab1 ... user: dan tags: session-update-loops) | 
| 20:27 |  | Prevent integer overflow due to database corruption in. fts3auxNextMethod(). Bug report bce19272cf. (check-in: d903795e3b ... user: drh tags: trunk) | 
| 19:44 |  | When a subquery is an argument to an SQLITE_SUBTYPE function, then set the EP_SubtArg flag on the result-set expressions of that subquery. Bug report 8de44412fd. (check-in: d75c08c841 ... user: drh tags: trunk) | 
| 18:59 |  | Update the session module so that it can apply changesets containing two or more UPDATE changes that form a dependency loop - so that no single UPDATE can be applied independently without violating a constraint. (check-in: 5f8010599f ... user: dan tags: session-update-loops) | 
| 17:55 |  | Fix the window-function variant of the json_group_object() function so that it correctly handles NULL entries. Bug report 0de87b23b3. (check-in: ac3a958b0a ... user: drh tags: trunk) | 
| 14:28 |  | Do not attempt the OR-optimization on conjuncts that contain COLLATE operators because (1) such cases are rare and (2) they are tricky to get right and are thus prone to bugs and hard to test. Bugs report 329521b269 is one such bug that goes back about 20 years and thus demonstrates both points of the previous sentence. (check-in: 6228825295 ... user: drh tags: trunk) | 
| 11:44 |  | Fix a harmless compiler warning caused by [460243ee4c28873d]. (check-in: e076ac7b64 ... user: drh tags: trunk) | 
| 11:35 |  | Correction to the bugfix at [212c68249cc0e890] so that it works for 2nd and subsequent terms of a row value. Bugs report ba8084256b. (check-in: 49a3c6ade4 ... user: drh tags: trunk) | 
| 10:19 |  | Raise an error in the dbpage virtual table if the page number on an insert is too large. Bug report f363b34881. (check-in: 2bbc8f1404 ... user: drh tags: trunk) | 
| 10:00 |  | Add a reference to the new sqlite.org/bugs forum. (check-in: f2fe7e7a18 ... user: stephan tags: trunk) | 
| 2026-05-17 |  |  | 
| 19:21 |  | Fix the ".prompt" command so that it honors -bail. Bug report 99cd9319f8. (check-in: 5b58bdd33f ... user: drh tags: trunk) | 
| 17:50 |  | Fix the new %J and %j formats so they work correctly with a zero precision. Bug report 277c67d8eb (check-in: 428b924dd6 ... user: drh tags: trunk) | 
| 17:18 |  | Fix the sqlite3_value_numeric_type() interface so that it works even if the argument is an sqlite3_value object that came from sqlite3_value_dup(). Bug report 15c4a323bb. (check-in: 460243ee4c ... user: drh tags: trunk) | 
| 16:35 |  | In the CLI, make the NO_COLOR prompt the same as the default prompt, only without any color. (check-in: cc05e9749c ... user: drh tags: trunk) | 
| 2026-05-16 |  |  | 
| 20:21 |  | When two tables are joined by USING, the affinity and collating sequence of the join column will be the same as one or the other of the columns in the two tables (which are hopefully the same). (check-in: cbb6f72c10 ... user: drh tags: trunk) | 
| 19:40 |  | When two tables are joined by USING, the affinity of the join column can be the affinity of the corresponding column in either table. But it must be one or the other. (check-in: 8cc0b52cca ... user: drh tags: trunk) | 
| 17:28 |  | Fix false-negatives in the sqlite3ExprCanReturnSubtype() routine having to do with CASE, CAST, and COLLATE expressions and the "+" no-op operator. forum:/forumpost/1f0168a120:Forum post 1f0168a120. (check-in: 9be94e9614 ... user: drh tags: trunk) | 
| 16:02 |  | Improved comment on sqlite3ExprCanReturnSubtype() as well as one very minor efficiency improvement. (closed check-in: b57d6b76b0 ... user: drh tags: index-on-case-expr-subtype) | 
| 2026-05-15 |  |  | 
| 19:44 |  | When using an index on a CASE expression that may return a value with a subtype, avoid discarding any subtype. Possible fix for forum 1f0168a120. (check-in: 69c9a82cba ... user: dan tags: index-on-case-expr-subtype) | 
| 18:36 |  | Have window functions sum(), total() and avg() correctly handle the obscure case where integer overflow occurs as part of the same step that adds the first real value to the running total. Fix for forum post ec538b04ce. (check-in: 614f8cbc37 ... user: dan tags: trunk) | 
| 17:24 |  | Have the sqlite_dbpage module correctly handle constraints of the form "WHERE pgno=$pgno" when $pgno is an integer that cannot be represented as a signed, 32-bit value. (check-in: 350152d512 ... user: dan tags: trunk) | 
| 2026-05-14 |  |  | 
| 22:39 |  | If there is a WHERE clause constraint of the form "a=$var" where the value of $var affects the query plan, then automatically reprepare the query if the binding of $var changes. (check-in: 1249eeb1b9 ... user: drh tags: trunk) | 
| 17:54 |  | Handle constraints like "a=$var" where the value bound to $var is -1, 0 or 1 in the same way as a literal -1, 0, 1. This might cause excessive reprepares. (leaf check-in: 224e827e87 ... user: dan tags: reprepare-a=var) | 
| 13:29 |  | Harden the QRF extension against integer overflow problems. (check-in: 83d3a0d39b ... user: drh tags: trunk) | 
| 12:46 |  | In QRF, relax the constraint on the screen width so that it can be as wide as a 32-bit signed integer can count. Forum post 2570051b60. (check-in: 91439d95dc ... user: drh tags: trunk) | 
| 11:35 |  | Raise an error if an ALTER TABLE ADD COLUMN statement tries to add a CHECK constraint that contains an internal-use-only SQL function. (check-in: 52dd437d51 ... user: drh tags: trunk) | 
| 10:36 |  | Correction to the documentation for sqlite3_uri_int64(). (check-in: 58472fdab0 ... user: drh tags: trunk) | 
| 2026-05-13 |  |  | 
| 20:37 |  | Fix the sqlite3changegroup_change_finish() API so that it correctly handles NULL being passed for the pzErr parameter. (check-in: f81d6d7bc8 ... user: dan tags: trunk) | 
| 20:06 |  | Additional changes to the Windows VFS to try to get the system to a point where it will compile for UWP. (check-in: 3ddc07110d ... user: drh tags: trunk) | 
| 18:07 |  | Change to compile-time option that omits non-UWP system calls to SQLITE_UWP. Takes steps to eliminate all non-UWP system calls when that compile-time option is used. (check-in: b167c4c96e ... user: drh tags: trunk) | 
| 15:11 |  | In the Windows OS interface, use CreateFileMappingFromApp() and MapViewOfFilFromApp() if SQLITE_OS_WINRT is defined. (check-in: 3c51436b43 ... user: drh tags: trunk) | 
| 13:55 |  | On Windows, use only GetSystemTimeAsFileTime() and not GetSystemTime(). Also add a TCL script that will renumber the systemcall thunk for Windows. (check-in: 4b7f0130d6 ... user: drh tags: trunk) | 
| 12:12 |  | Modify [2990dd27dd] to use setlocale(LC_CTYPE) instead of setlocale(LC_ALL). Same effect but hypothetically a smaller scope. (check-in: 8be57f96f6 ... user: stephan tags: trunk) | 
| 12:06 |  | Slightly rework the --editline configure flag for purposes of distinguishing between the two conventional libedit headers. When using libedit, invoke setlocale(), which appears to resolve forum post aad7a63491's report of non-ASCII input failng to work when linking against libedit. (check-in: 2990dd27dd ... user: stephan tags: trunk) | 
| 10:47 |  | Use GetTickCount64() instead of GetTickCount() in Windows, for portability to UWP. (check-in: ea65ae964e ... user: drh tags: trunk) | 
| 07:33 |  | In Haiku OS builds, fix the --editline configure flag to actually link against libedit rather than libreadline. (check-in: 642168d386 ... user: stephan tags: trunk) | 
| 2026-05-12 |  |  | 
| 19:33 |  | Do not allow internal-use-only functions to be used in CHECK constraints that are added as part of ALTER TABLE ADD COLUMN. (closed check-in: 28272746fb ... user: drh tags: add-column-safety) | 
| 18:06 |  | Fix a copy/paste error in a comment. No changes to code. (check-in: 2741193e81 ... user: drh tags: trunk) | 
| 12:34 |  | Fix the insert4.test test module so that it works when SQLite is compiled with SQLITE_OMIT_VIRTUALTABLE. (check-in: bff9db9b5e ... user: drh tags: trunk) | 
| 11:47 |  | Attempt to disable false-positive GCC compiler warning. (check-in: e377b4d375 ... user: drh tags: trunk) | 
| 2026-05-11 |  |  | 
| 16:58 |  | Fix the xfer-optimization such that if the destination table has constraints on the hidden rowid, the optimization is not attempted. Forum post 6dcc95e3ca. (check-in: da9ca357fb ... user: drh tags: trunk) | 
| 14:22 |  | Prevent internal-use-only SQL functions from being used inside of a CHECK constraint added using ALTER TABLE ADD CONSTRAINT. Forum post 6256ee34ed. (check-in: 07beb966da ... user: drh tags: trunk) | 
| 14:14 |  | Fix comment typos. Add test cases. (closed check-in: 7a85921ae5 ... user: drh tags: add-constraint-safety-v2) | 
| 12:38 |  | An alternative fix to ALTER TABLE ADD CONSTRAINT. (check-in: a231fc6d58 ... user: drh tags: add-constraint-safety-v2) | 
| 11:53 |  | Improve detection of corrupt database records in fts5. (check-in: 87b6537311 ... user: dan tags: trunk) | 
| 11:12 |  | Fix potential buffer overwrite that could occur in fts5 when processing corrupt records. (check-in: 4a5ad516ea ... user: dan tags: trunk) | 
| 11:05 |  | Fix harmless bug in testing logic. No changes to deliverables. (check-in: c4a6e62b5f ... user: drh tags: trunk) | 
| 10:25 |  | Hush-up a false-positive compiler warning from GCC. See forum thread 281ace196961082c. (check-in: 45a2152908 ... user: drh tags: trunk) | 
| 00:53 |  | Fix the xfer optimization so that it does not work if the destination is a STRICT table that is incompatible with the source. Forum post 4955d2235c22ef4e. (check-in: 5e916b2a8f ... user: drh tags: trunk) | 
| 2026-05-09 |  |  | 
| 14:32 |  | Minor config script doc tweaks. (check-in: 678954553e ... user: stephan tags: trunk) | 
| 14:01 |  | In sqlite-config.tcl remove a reference to the platform name apple when checking for Mac platforms, relying on a check for darwin instead, analog to [b5c6cb13cff53f]. Intended to resolve forum post c26b856db7dd5caf. (check-in: 7ccffe38b4 ... user: stephan tags: trunk) | 
| 13:54 |  | Fix upward propagation of expression properties into row-value expressions. Forum post 8387cc287240643e. (check-in: 0372dd717b ... user: drh tags: trunk) | 
| 13:16 |  | Improved detection of OOM in the regexp_bytecode() SQLite function (available in debug builds only). Forum post a3a10e1c7b5dd1d2. (check-in: 172e8c35ba ... user: drh tags: trunk) | 
| 12:41 |  | Fix an integer truncation problem due to oversized inputs to the approximate_match virtual table extension. Forum post 625f687e4d25c392. (check-in: 2881e17cc0 ... user: drh tags: trunk) | 
| 12:03 |  | Fix a corner-case floating-point rounding problem in the generate_series() virtual table, introduced by [c113e31b818d1677] and reported by forum post 6e6cf9054bea2b1d. (check-in: ff56785674 ... user: drh tags: trunk) | 
| 2026-05-07 |  |  | 
| 20:45 |  | Remove unnecessary and incorrect assert() statements in the new %J code. (check-in: 5a8497dbec ... user: drh tags: trunk) | 
| 15:04 |  | Add the %J and %j conversions to printf() - rendering JSON string literals. (check-in: 7e6fd838a1 ... user: drh tags: trunk) | 
| 14:47 |  | Fix harmless UB by declaring a constant 1 to be unsigned. (closed check-in: c5235734a6 ... user: drh tags: format-json) | 
| 14:30 |  | Fix another off-by-one error. (check-in: 8ae724a647 ... user: drh tags: format-json) | 
| 14:14 |  | Bug fixes in the new %J and %j logic. (check-in: d1b55572c7 ... user: drh tags: format-json) | 
| 10:46 |  | Add support for width to %J and %j. (check-in: 0b102f2076 ... user: drh tags: format-json) | 
| 08:58 |  | Separate conversion letters %J and %j. %J includes surrounding quotes and %j omits them. Some test cases added. (check-in: ea7c2c1a6b ... user: drh tags: format-json) | 
| 00:02 |  | Get precision working on %J, both with and with the ! flag. Width is still a no-op, and there are no test cases. Incremental check-in. (check-in: 18166bce20 ... user: drh tags: format-json) | 
| 2026-05-06 |  |  | 
| 20:48 |  | Experimental prototype of the %J conversion in printf(). (check-in: 7f6b1bae08 ... user: drh tags: format-json) | 
| 19:13 |  | Fix a minor typo in a comment in the json module. (check-in: 5c6d3e9d83 ... user: drh tags: trunk) | 
| 06:57 |  | Minor internal doc typo fixes. (check-in: d588680240 ... user: stephan tags: trunk) | 
| 2026-05-05 |  |  | 
| 15:31 |  | Delete the OPFS SEE test dbs when done with them. (check-in: cbb9f7bcd0 ... user: stephan tags: trunk) | 
| 15:26 |  | Add importDb() tests of SEE databases for the opfs/opfs-wl VFSes, analog to the opfs-sahpool test in [a5a9ac7ad6622b]. (check-in: 534d388031 ... user: stephan tags: trunk) | 
| 08:51 |  | For the various OPFS VFS db-import routines, in SEE builds (only) relax the is-this-a-db check to allow for SEE dbs not having the usual header string in cleartext. Addresses SEE forum post f84bef3552. (check-in: a5a9ac7ad6 ... user: stephan tags: trunk) | 
| 08:11 |  | Fix a predicate which causes a false error in the OPFS SAHPool tests when building with SEE. (check-in: 97ab0a2fd5 ... user: stephan tags: trunk) | 
| 07:57 |  | Add #include time.h to shell.c.in to get fiddle building with emcc 5.x. (check-in: ae675dfe79 ... user: stephan tags: trunk) | 
| 2026-05-04 |  |  | 
| 18:36 |  | Fix a test case on CLI ".help" output due to the output changing because of check-in [1590b401ff0670b6]. (check-in: b6ad35096d ... user: drh tags: trunk) | 
| 18:16 |  | When using the OR-optimization on a loop created by exists-to-join ([e33da6d5dc964db8]), do the early-exit logic for the whole loop, not for each individual OR branch. Possible fix for forum post c8b205720c. (check-in: 70a5d09124 ... user: dan tags: trunk) | 
| 17:16 |  | When using the OR-optimization on a loop created by exists-to-join, do the early-exit logic for the whole loop, not for each individual OR branch. Possible fix for forum post c8b205720c. (closed check-in: 3d197dd03c ... user: dan tags: exists-or-expr-fix) | 
| 16:18 |  | Add a missing tcl quote which breaks the build on msys2. (check-in: 9380be51c7 ... user: stephan tags: trunk) | 
| 12:58 |  | Add the --color and --no-color options to the ".prompt" command of the CLI. (check-in: 1590b401ff ... user: drh tags: trunk) | 
| 10:14 |  | Honor the NO_COLOR environment variable. If NO_COLOR is set and SQLITE_PS1 and SQLITE_PS2 are not set, then the default prompt is monochrome and ANSI escapes are omitted. Also change the name of the SQLITE_PS_NOANSI compile-time option to SQLITE_NO_COLOR. (check-in: 7e4134e3ff ... user: drh tags: trunk) | 
| 2026-05-03 |  |  | 
| 18:31 |  | New test cases for the shell prompt (check-in: 2340cbbd23 ... user: drh tags: trunk) | 
| 15:53 |  | Improvements to the non-printing character delimiter logic in the CLI prompt, for improved accuracy and for improved testability. (check-in: f5649795cd ... user: drh tags: trunk) | 
| 2026-05-02 |  |  | 
| 23:40 |  | Change the non-printing character escapes for editline to U+0001 and U+0002. Also insert those escapes for readline. Readline does not need it (because it can figure them out on its own) but they are needed when editline is being used in readline-compatibility mode, and we have no way of distinguishing between editline and readline in that case, so we might as well include them. (check-in: b9545b0f0b ... user: drh tags: trunk) | 
| 23:03 |  | Attempt to automatically insert U+0001 escape bytes before and after ANSI X3.64 escape sequences when using editline, to work around the limitations of editline. (check-in: 63050bee6e ... user: drh tags: trunk) | 
| 20:19 |  | Fix a harmless compiler warning on some MSVC versions. (check-in: bb8f6deeff ... user: drh tags: trunk) | 
| 17:30 |  | Minor optimization to the authorizer. (check-in: 625f78a630 ... user: drh tags: trunk) | 
| 15:58 |  | Fix harmless compiler warnings. (check-in: a0d0b5aff5 ... user: drh tags: trunk) | 
| 11:47 |  | Close database connections in a timely manner to fix test script problems on Windows. (check-in: 068a53865a ... user: drh tags: trunk) | 
| 2026-05-01 |  |  | 
| 17:42 |  | Fix an SQLITE_MISUSE memory error in check-ini [1f940357f7bb160b]. (check-in: fc64113e8f ... user: drh tags: trunk) | 
| 16:06 |  | Harden the recovery extension against SQL injections coming from the sqlite_schema table of the database being recovered. (check-in: 9190f1b9b8 ... user: drh tags: trunk) | 
| 16:03 |  | Check-in [1786fcd5b4ee6cd9] works great and generates correct code, but it upset UBSAN. This check-in fixes the UBSAN complaint. (check-in: 7cd76847e8 ... user: drh tags: trunk) | 
| 13:56 |  | Harden the recovery extension against SQL injections coming from the sqlite_schema table of the database being recovered. (check-in: 555401fe04 ... user: drh tags: branch-3.53) | 
| 2026-04-30 |  |  | 
| 18:23 |  | The value for /f and other filename substitutions in SQLITE_PS1 is now "memory" if open on an in-memory database. (check-in: 4aac1057ee ... user: drh tags: trunk) | 
| 16:52 |  | Remove lots of code used to support legacy Windows version (WindowsXP and before). Use Slim Reader/Writer Locks on Windows for non-recursive mutexes, since SRWLs are now available on all supported Windows platforms. (check-in: 6f7a5ff22d ... user: drh tags: trunk) | 
| 15:41 |  | Fix a harmless compiler warning in src/mutex_w32.c. (check-in: 8de796ca45 ... user: drh tags: trunk) | 
| 15:33 |  | Fix the memdb1.test test module so that it runs correctly on Windows. (check-in: 9012f8ea74 ... user: drh tags: trunk) | 
| 15:19 |  | Change the CLI warning that appears in the header about opening a in-memory database, to be more readable. (check-in: cc64cafdfe ... user: drh tags: trunk) | 
| 15:07 |  | Add the /m escape to the CLI prompt string. Use it to show "memory" in italic for the default prompt for in-memory databases. (check-in: 1e3c953930 ... user: drh tags: trunk) | 
| 2026-04-29 |  |  | 
| 19:14 |  | Fix a crash that could occur if the destination database of a backup is replaced using sqlite3_deserialize() between the call to sqlite3_backup_init() and the first call to sqlite3_backup_step(). Forum post 15d82885e2. (check-in: 1f940357f7 ... user: dan tags: trunk) | 
| 16:09 |  | Reduce the number of system calls in the os_win.c thunk from 81 down to 58, as well as other simplifications. (closed check-in: c49521739c ... user: drh tags: omit-legacy-windows) | 
| 15:03 |  | Further simplifications to the interface to the Windows family of OSes. An incremental check-in. (check-in: f2aa6b66d2 ... user: drh tags: omit-legacy-windows) | 
| 14:02 |  | Simplify the Windows interface by removing support for various WindowsXP and WindowsCE. This is a work-in-progress. More to follow. (check-in: 4dbc4d19c4 ... user: drh tags: omit-legacy-windows) | 
| 2026-04-28 |  |  | 
| 20:01 |  | Updates and simplifications to the mutex implementation on Windows. Uses Slim Reader/Writer Locks for performance, for non-reentrant mutexes. Requires WindowsVista or later. (check-in: a75aa5400f ... user: drh tags: omit-legacy-windows) | 
| 15:12 |  | Attempt to align instances of the sqlite3_mutex object at 128-byte boundaries to prevent false-sharing in multi-core machines. See the discussion at and around forum post 2026-03-25T23:15:03Z. (check-in: 1786fcd5b4 ... user: drh tags: trunk) | 
| 12:49 |  | Fix comment typo (check-in: d64a1dbe0f ... user: drh tags: trunk) | 
| 2026-04-27 |  |  | 
| 14:01 |  | Improved detection of when the alternative windows-compatible main() is needed in the sqldiff.exe utility. (check-in: e25b849a60 ... user: drh tags: trunk) | 
| 12:43 |  | Relax arbitrary length restrictions on some error messages in the CLI. (check-in: 52f5531ded ... user: drh tags: trunk) | 
| 12:05 |  | Increase a stack-local buffer size to resolve a test regression introduced in [ff084ae341eab5c], in which some tests fail a pattern match check if their absolute file names are too long. Resolves forum post 205f73db1b2806f5. (check-in: 286fc22c7c ... user: stephan tags: trunk) | 
| 11:32 |  | Fix a crash that could occur if sqlite3_deserialize() was used to overwrite a database with an open transaction on it. Bug introduced by [fc42d31d6fca21ab] on 2018-03-07 and first released in version 3.23.0. This is not a vulnerability as an attacker that can exploit this already has the ability to execute arbitrary code. Forum post 39134ba029. (check-in: 77662cce9a ... user: dan tags: trunk) | 
| 06:48 |  | Refactor the uint128 type check to consolidate duplicated rules. (check-in: 362ef7bc00 ... user: stephan tags: trunk) | 
| 05:55 |  | Feature detection fix for uint128 on RISCV 32-bit, reported in forum post 2026-04-26T11:41:01Z and validated by the OP. (check-in: c4a2c20839 ... user: stephan tags: trunk) | 
| 2026-04-26 |  |  | 
| 18:35 |  | Improvements to temp-file cleanup on close in the CLI. (check-in: 048c969c34 ... user: drh tags: trunk) | 
| 2026-04-25 |  |  | 
| 16:17 |  | In the CLI, eliminate the 2-second delay before getting the prompt back after running a ".www" or ".excel". Instead, remember the name of the temporary file that is to be deleted and delete it later. (check-in: 4e9ee211ca ... user: drh tags: trunk) | 
| 15:05 |  | Simplify the modePush()/modePop() implementation in the CLI. (check-in: 92cad2cd72 ... user: drh tags: trunk) | 
| 14:32 |  | Do not run expridx2.test as part of the "journaltest" permutation, as it requires wal mode. (check-in: 11cfbae735 ... user: dan tags: trunk) | 
| 14:00 |  | Fix a single-byte OOB read that could occur in the session module when concatenating patchsets. (check-in: 60d7cd625a ... user: dan tags: trunk) | 
| 10:45 |  | Minor code simplification in the CLI. (check-in: 59795c71e5 ... user: drh tags: trunk) | 
| 00:29 |  | Fix a problem in the CLI in which the ".www" command followed by ".dbstat" does not restore the mode to its original setting afterwards. (check-in: 4a5cac1d00 ... user: drh tags: trunk) | 
| 2026-04-24 |  |  | 
| 20:27 |  | Fix test cases due to recent CLI default prompt changes. Make sure ANSI escape codes are recognized on Windows terminals. (check-in: 22b791e404 ... user: drh tags: trunk) | 
| 09:21 |  | Improvements to the default prompt for the CLI. (check-in: 9ab0bb487f ... user: drh tags: trunk) | 
| 2026-04-23 |  |  | 
| 22:15 |  | Improved default prompt in the CLI. (check-in: 78193b6034 ... user: drh tags: trunk) | 
| 20:13 |  | Fix harmless compiler warnings. Enhance the CLI prompt mechanism to make it more easily reusable by other projects, and to add the /h, /H, and /u escape sequences. (check-in: 354ce4f5e2 ... user: drh tags: trunk) | 
| 20:06 |  | Fix a bug in [dbc1d6f0a3452607] that disrupts the display when running on a Windows console (not redirecting to a file). (check-in: 75b5df2b00 ... user: drh tags: trunk) | 
| 16:40 |  | Improvements to the #define mechanism in the CLI that allows other programs that incorporate the shell.c source file to customize some of the prompt expansions. (check-in: e5f8840d11 ... user: drh tags: trunk) | 
| 15:54 |  | Have testrunner.tcl return a non-zero exit status if one or more tests have failed. (check-in: 50ddc36d32 ... user: dan tags: trunk) | 
| 15:05 |  | New CLI prompt escape sequences: /r, /A, /v, /V, and /D../D. (check-in: e7c7acd1a4 ... user: drh tags: trunk) | 
| 14:49 |  | Fix harmless compiler warning. (check-in: 706373dcbe ... user: drh tags: trunk) | 
| 14:05 |  | CLI shell: handle the special case of .prompt "" to not incorrectly report that as an OOM. Reported in the Fossil SCM forum. (check-in: 3fa0500b96 ... user: stephan tags: trunk) | 
| 2026-04-22 |  |  | 
| 19:38 |  | When updating indexes on expressions, omit the update if the new indexed values are the same as the old. (check-in: f0fc4e5ec3 ... user: dan tags: trunk) | 
| 19:25 |  | Add OOM test for the new code on this branch. (closed check-in: 685be705a2 ... user: dan tags: index-detect-noop) | 
| 19:12 |  | Mark branches that are believed to be unreachable. (check-in: 18ed9736fb ... user: drh tags: index-detect-noop) | 
| 18:19 |  | Instead of adding a branch to OP_IdxInsert, use an extra OP_IfNull when updating indexes on expressions. (check-in: 5112f96cb1 ... user: dan tags: index-detect-noop) | 
| 17:50 |  | Enable the optimization on this branch for indexes on expressions only. (check-in: 3576b69304 ... user: dan tags: index-detect-noop) | 
| 17:31 |  | Fix P3/P5 confusion in the comment above OP_IdxDelete. (check-in: 0fb6ca8e22 ... user: dan tags: index-detect-noop) | 
| 16:58 |  | Detect and omit cases where an index key is being replaced with an identical key. (check-in: 7a01caf0d0 ... user: dan tags: index-detect-noop) | 
| 13:41 |  | Fix a integer type bug in the previous check-in. (check-in: 5e1b5ff1c1 ... user: drh tags: trunk) | 
| 12:22 |  | QRF and the CLI preserve zero bytes when outputing unformatted BLOB values. (check-in: dbc1d6f0a3 ... user: drh tags: trunk) | 
| 2026-04-21 |  |  | 
| 22:19 |  | Fix harmless compiler warning in QRF. (check-in: dcfb505ac7 ... user: drh tags: trunk) | 
| 20:40 |  | Disable optimization of nested EXIST loops. Possible fix for forum post 77c02e8ed. (check-in: d0d95a39bc ... user: dan tags: trunk) | 
| 20:28 |  | Fix the BETWEEN operator so that it propagates the EP_Subquery, EP_Collate, and EP_HasFunc flags in the expression tree, as it ought. Fix for the problem reported at forum post 2026-04-21T19:10:55Z and probably a bunch of other problems too. This has been incorrect for a long time. (check-in: 2d1d2947b2 ... user: drh tags: trunk) | 
| 18:07 |  | Correctly dequote the constraint name passed to ALTER TABLE DROP CONSTRAINT. This improves upon the partial fix in [7f5afb12f4]. Forum post a746b08b87. (check-in: 9387238cb0 ... user: dan tags: trunk) | 
| 16:40 |  | Disable optimization of nested EXIST loops. Possible fix for forum post 77c02e8ed. (closed check-in: 64f0d9eb63 ... user: dan tags: disable-nested-exist-opt) | 
| 16:07 |  | Fix an off-by-one buffer size calculation in the decimal.c extension. (check-in: 24df42104d ... user: drh tags: trunk) | 
| 14:39 |  | Do not unnecessarily delay opening the database file in the CLI. (check-in: ab6ee76c12 ... user: drh tags: trunk) | 
| 12:59 |  | Fix another bug in EXISTS-to-JOIN - this one having to do with an OFFSET clause of a LIMIT. Reported by forum post 2026-04-21T10:57:09Z. (check-in: 1dd3c6a5e5 ... user: drh tags: trunk) | 
| 10:19 |  | Reformulate how the OPFS async proxy Worker is loaded for the bundler-friendly builds to work around a bug in Vite in which it incompatibly rewrites the Worker's URL arguments. Addresses the discussion at npm package PR 159 and patch validated by Jure Rotar. (check-in: ec877a30f5 ... user: stephan tags: trunk) | 
| 10:15 |  | Reformulate how the OPFS async proxy Worker is loaded for the bundler-friendly builds to work around a bug in Vite in which it incompatibly rewrites the Worker's URL arguments. Addresses the discussion at npm package PR 159 and patch validated by Jure Rotar. (check-in: 6215c9dd27 ... user: stephan tags: branch-3.53) | 
| 2026-04-20 |  |  | 
| 10:28 |  | Fix the fileio extension for Windows builds so that it does not depend on the sqlite3_win32_utf8_to_unicode() and sqlite3_win32_unicode_to_utf() routines that are found in the SQLite core. Forum post 2026-04-20T02:02:56Z. (check-in: fe0414a9a3 ... user: drh tags: trunk) | 
| 2026-04-18 |  |  | 
| 21:51 |  | Fix a harmless compiler warning in test/speedtest1.c. (check-in: 13f0fd1daa ... user: drh tags: trunk) | 
| 12:31 |  | Performance improvement in the substr() and length() SQL functions. Caution: The new code may give different results for malformed UTF8. I do not consider that to be a problem by the GI/GO principle. (check-in: 6124d27a33 ... user: drh tags: trunk) | 
| 2026-04-17 |  |  | 
| 19:12 |  | Fix a test case that started failing due to a change in the help text for the ".prompt" command of the CLI. (check-in: bfe9df1bf4 ... user: drh tags: trunk) | 
| 15:09 |  | Update interrupt2.test to avoid using Tcl_GetCommandInfo() from within calls to Tcl_DeleteCommand(). (check-in: 1979aa0902 ... user: dan tags: trunk) | 
| 11:41 |  | Help-text and comment fixes in the CLI. Add the output-redir-test.txt testing script. (check-in: ec58a133b4 ... user: drh tags: trunk) | 
| 01:50 |  | Less error-prone parsing of ".once" options. Fix for the problem reported by forum post 2026-04-17T01:26:56Z. (check-in: b235a80937 ... user: drh tags: trunk) | 
| 2026-04-16 |  |  | 
| 19:50 |  | Fix testrunner.tcl so that it does not create directories with names ending in "sqlite3.o" or "sqlite3.lo". Use "sqlite3obj" for those directory names instead. Otherwise "make clean" gets confused. (check-in: a4bf9289c9 ... user: drh tags: trunk) | 
| 19:33 |  | Improved comments explaining the logic, and improved diagnostic messages when testrunner.tcl encounters trouble loading the sqlite3 extension. (check-in: 8d820b0ccf ... user: drh tags: trunk) | 
| 17:07 |  | Minor updates to testrunner documentation. (check-in: f8507bd5e2 ... user: drh tags: trunk) | 
| 16:43 |  | Run C-language tests in tests/c/ as part of release test. (check-in: e6929022b7 ... user: dan tags: trunk) | 
| 2026-04-15 |  |  | 
| 14:41 |  | The sqlite3_str_new() interface never returns NULL, even after an OOM. Code that uses that interface should invoke sqlite3_str_errcode() to see if an OOM has occurred. (check-in: cb910047ed ... user: drh tags: trunk) | 
| 14:17 |  | Bug fix in sqlite3_str_free(): Do not crash if called on an sqlite3_str object returned from sqlite3_str_new() following an OOM. (check-in: 92d8ac0c49 ... user: drh tags: trunk) | 
| 13:05 |  | Add support for the analyze() extension function to the fuzzcheck test program. (check-in: abd94554d2 ... user: drh tags: trunk) | 
| 12:47 |  | Improve the analyze() extension so that it returns a simple error message as text (it does not throw an error) if the database is empty or is "temp" or does not exist. (check-in: 35379f6092 ... user: drh tags: trunk) | 
| 12:35 |  | Fix harmless compiler warnings seen by MSVC in the previous check-in. Also add ext/misc/analyze.c to the makefile dependencies for the CLI. (check-in: 5a0ee6e227 ... user: drh tags: trunk) | 
| 12:20 |  | Add the analyze() SQL function implemented the ext/misc/analyze.c extension (but not part of the amalgamation). Make that function available in the CLI and add a new ".dbstat" command to the CLI using that function. (check-in: 4c4988e56d ... user: drh tags: trunk) | 
| 11:58 |  | Add a missing open_db() call in the new ".dbstat" command of the CLI. (closed check-in: a138e44a24 ... user: drh tags: analyze-sql-func) | 
| 11:41 |  | Add the .dbstat command to the CLI. (check-in: fdba76df2b ... user: drh tags: analyze-sql-func) | 
| 11:40 |  | On platforms which support ./configure --with-icu-config, ensure that 'make tclextension' includes the ICU pieces in the tcl extension. Intended to resolve forum post 92a88de4b88929fe. (check-in: 404fde8bac ... user: stephan tags: trunk) | 
| 11:23 |  | Avoid a potentially undefined left-shift in the fts5 porter tokenizer. (check-in: 46a288e43e ... user: dan tags: trunk) | 
| 11:07 |  | Handle the case where the name of the constraint in an "ALTER TABLE DROP CONSTRAINT" command is quoted. Forum post 8bfbaec404. (check-in: 7f5afb12f4 ... user: dan tags: trunk) | 
| 10:49 |  | Compute the average fanout for b-trees of depth 2 or more. (check-in: 2f8598bb09 ... user: drh tags: analyze-sql-func) | 
| 2026-04-14 |  |  | 
| 23:53 |  | Minor doc typo fixes contributed by BrickViking. (check-in: c4660ce205 ... user: stephan tags: trunk) | 
| 23:19 |  | Code simplifications. Return NULL is the specified SCHEMA does not exist. (check-in: 953969c881 ... user: drh tags: analyze-sql-func) | 
| 20:18 |  | Handle a special case of a corrupt changeset in sqlite3changegroup_add(). (check-in: eba625f917 ... user: dan tags: trunk) | 
| 20:17 |  | Have the session module detect some corrupt changesets earlier. (closed check-in: 0de91ff0d7 ... user: dan tags: corrupt-changeset_fix) | 
| 20:17 |  | Merge trunk enhancements and fixes into the analyze-sql-func branch. (check-in: bb4a9beac8 ... user: drh tags: analyze-sql-func) | 
| 20:07 |  | Various minor tweaks and refinements to the analyze() function. (check-in: 48d94e72ea ... user: drh tags: analyze-sql-func) | 
| 20:02 |  | Handle a special case of a corrupt changeset in sqlite3changegroup_add(). (check-in: 49b3bac482 ... user: dan tags: corrupt-changeset_fix) | 
| 19:36 |  | Implementation of the analyze() SQL function is now mostly complete. (check-in: 1ca0386ab2 ... user: drh tags: analyze-sql-func) | 
| 19:02 |  | Much, but not all, of the report generation is added. An incremental check-in. (check-in: 289fefc3ed ... user: drh tags: analyze-sql-func) | 
| 18:12 |  | Ensure c-tests use the locally built sqlite3.h file, not the system copy. (closed check-in: 2d81ee65ff ... user: dan tags: c-tests) | 
| 18:08 |  | In RBU vacuum commands, do as SQLite does and ignore any trailing SQL statements in the sql column of the sqlite_schema table. (check-in: cf7dad1b10 ... user: dan tags: trunk) | 
| 18:06 |  | Check-in [495e4851d4296b49] breaks the build for Windows. Fixed here. (check-in: 5b004348c3 ... user: drh tags: trunk) | 
| 13:18 |  | Further fixes and clarifications. Now ready to start generating the actual report. (check-in: 037a0e586c ... user: drh tags: analyze-sql-func) | 
| 12:46 |  | Further improvements to error messages and robustness. (check-in: 45917c1124 ... user: drh tags: analyze-sql-func) | 
| 12:23 |  | Improved error messages and robustness. (check-in: c09ed9eb91 ... user: drh tags: analyze-sql-func) | 
| 11:25 |  | Initial prototype implementation of a new run-time loadable SQL function to do approximately the same work as sqlite3_analyze. (check-in: 03934b5a6c ... user: drh tags: analyze-sql-func) | 
| 02:09 |  | Minor README typo fix reported off-list by BrickViking. (check-in: aa3432af90 ... user: stephan tags: trunk) | 
| 00:48 |  | Pass the ICU-related build flags through the 'tclextension' target, not just 'tclextension-install'. (closed check-in: 499c89f874 ... user: stephan tags: tclextension-icu) | 
| 00:11 |  | Tweak 'make tclextension' to pass on any --with-icu-config build flags so that it can link with the various ICU libs. This works for me but needs testing on a wider selection of systems before being moved to trunk. Intended to resolve forum post 92a88de4b88929fe. (check-in: a8c022d794 ... user: stephan tags: tclextension-icu) | 
| 2026-04-13 |  |  | 
| 17:33 |  | Fix typos and inaccuracies in comments. No changes to code. (check-in: a6c2d7ce84 ... user: drh tags: trunk) | 
| 12:22 |  | ".prompt" command improvements in the CLI: Guard against mistakenly saying ".prompt show" without the "--" before "show". Add the --hard-reset option to .prompt to make it more easily testable. (check-in: 495e4851d4 ... user: drh tags: trunk) | 
| 11:34 |  | Improvements to the prompt_filename() function in the CLI so that it finds the database filename even if the connection is not yet fully open. (check-in: efb3e808a0 ... user: drh tags: trunk) | 
| 11:20 |  | Update test/c/snprintf1.c to test the result of the sqlite3_snprintf() call. Also add an SQLITE_OMIT_AUTOINIT configuration and update C tests to account for it. (check-in: 30b597d797 ... user: dan tags: c-tests) | 
| 10:56 |  | Latest upstream jimsh0.c. (check-in: 414ae4dd65 ... user: stephan tags: trunk) | 
| 2026-04-12 |  |  | 
| 17:47 |  | Doc typo fix which broke the docsrc build, reported by BrickViking. (check-in: 3c212b5d85 ... user: stephan tags: trunk) | 
| 10:46 |  | Bug fix in the revised ".prompt" command of the CLI, with test cases. (check-in: 7583385593 ... user: drh tags: trunk) | 
| 05:41 |  | Fix a typo which causes .headers to not show up in the shell's .help. (check-in: d379b14a48 ... user: stephan tags: trunk) | 
| 2026-04-11 |  |  | 
| 23:58 |  | Change the prompt escape design yet again, to use / as the escape character, since % and just about every other punctuation character is special to cmd.exe, batch files, and nmake, on Windows. (check-in: 79a8d3edf8 ... user: drh tags: trunk) | 
| 22:58 |  | CLI prompt design change: The escape character is changed from \ to %#37;. This avoids lots of quoting problems when trying to specify prompt strings in various programming languages and shells. (check-in: e7e6ecc45d ... user: drh tags: trunk) | 
| 22:18 |  | Two C-preprocessor macros SQLITE_PS1 and SQLITE_PS2 can be set to override the default prompt strings in the CLI. (check-in: 6fa0216a19 ... user: drh tags: trunk) | 
| 20:53 |  | Make sqlite3_incomplete() available to extensions. (check-in: 50d81300ba ... user: drh tags: trunk) | 
| 20:43 |  | When fiddle terminal mode is enabled, add a link to the jquery.terminal project in the About box. (check-in: d64f0c8845 ... user: stephan tags: trunk) | 
| 20:41 |  | Fix harmless UB in sqlite3_incomplete(). (check-in: 8dfdff559f ... user: drh tags: trunk) | 
| 20:31 |  | fiddle: disable jquery.terminal's pre-processing of user input, as discussed in forum post c6665017c0dbba1f, with many thanks to the jquery.terminal team for this workaround. (check-in: ea7e121ab2 ... user: stephan tags: trunk) | 
| 18:38 |  | Improvements to the interactive prompt in the CLI: (1) CLI honors SQLITE_PS1 and SQLITE_PS2 envvars as prompt strings, (2) CLI prompt strings expand various backslash escapes, (3) No arbitrary length limits on CLI prompt strings, (4) New default prompt shows the database filename using the \f escape, (5) SQLite core adds the sqlite3_incomplete() API, used by the \H escape in prompt string rendering, (6) Enhancements to the ".prompt" dot-command. (check-in: 55af98054e ... user: drh tags: trunk) | 
| 18:05 |  | Fixes for running C tests on windows. (check-in: 1fcacdc41a ... user: dan tags: c-tests) | 
| 17:47 |  | Fix harmless compiler warnings. (closed check-in: 3b4cc8d3be ... user: drh tags: cli-prompt) | 
| 17:03 |  | Enhance testrunner.tcl to run individual tests written in C from the test/c/ directory. (check-in: 6f140f76f4 ... user: dan tags: c-tests) | 
| 16:12 |  | Initial test cases for the CLI prompt enhancements. (check-in: b7ecc84735 ... user: drh tags: cli-prompt) | 
| 15:45 |  | Add --reset, --show, and -- options to the ".prompt" command of the CLI. (check-in: 8075576ba2 ... user: drh tags: cli-prompt) | 
| 14:11 |  | Enhancements to shell_prompt_test() for better testing. Enhance the ".open" command so that it does ~/ expansion on filenames. (check-in: ec6830fe60 ... user: drh tags: cli-prompt) | 
| 12:35 |  | Remove arbitrary limits on the length of CLI prompts. Use environment variables SQLITE_PS1 and SQLITE_PS2 (if they exist) as the default CLI prompts. (check-in: a46ceaeab9 ... user: drh tags: cli-prompt) | 
| 11:24 |  | Fix the \B escape for prompts so that it uses wcwidth() instead of strlen() to compute spacing. (check-in: c6ff4f701d ... user: drh tags: cli-prompt) | 
| 2026-04-10 |  |  | 
| 23:31 |  | Ensure that sqlite3-opfs-async-proxy.js is copied into the fiddle target dirs. (check-in: 149200861e ... user: stephan tags: trunk) | 
| 23:25 |  | Ensure that sqlite3-opfs-async-proxy.js is copied into the fiddle target dirs. (check-in: 7c9998de1b ... user: stephan tags: cli-prompt) | 
| 23:05 |  | A protype of .prompt support in fiddle but its utility there is in doubt because .open does not work there, so there's very little dynamic info worth showing in a prompt. Also, ANSI colorization can't work there, nor can we colorize the prompts using HTML because of limitations in jquery.terminal. (check-in: 3b17500dc6 ... user: stephan tags: cli-prompt) | 
| 21:58 |  | Expose the main /fiddle object to the browser dev console as globalThis.fiddle for experimenting with a dynamic prompt. (check-in: 8574e5c9da ... user: stephan tags: trunk) | 
| 20:44 |  | Fix minor issues on Windows. (check-in: 48e40d3f6a ... user: drh tags: cli-prompt) | 
| 20:29 |  | Fix prompt for in-memory databases. Fix the continuation mechanism so that it looks at all accumulated lines, not just the most recent line. (check-in: 1668947255 ... user: drh tags: cli-prompt) | 
| 20:14 |  | Replace the former dynamic-continuation prompt logic with backslash-escape prompt expansion, include some expansions that serve the same role as the old dynamic-continuation prompts. This allows greater user configurability. (check-in: 052631202e ... user: drh tags: cli-prompt) | 
| 20:06 |  | Add the \B escape for CLI prompts. Change the default prompts to use \f, \H, and \B. (closed check-in: 354325b6f3 ... user: drh tags: cli-prompt-redo) | 
| 19:33 |  | The \H escape on a prompt string fills in with the characters needed to make the input "complete". Add the shell_expand_prompt() SQL function for testing purposes. (check-in: 6aafb76c74 ... user: drh tags: cli-prompt-redo) | 
| 18:53 |  | Merge trunk fixes and enhancements into the cli-prompt-redo branch (check-in: f3354ebf51 ... user: drh tags: cli-prompt-redo) | 
| 18:14 |  | Improve defenses in QRF against trying to run the same prepared statement twice, concurrently. (check-in: 6ab6f622be ... user: drh tags: trunk) | 
| 17:55 |  | API doc correction for sqlite3_js_retry_busy(). No code changes. (check-in: 749c537812 ... user: stephan tags: trunk) | 
| 17:11 |  | Use sqlite3_malloc() for memory allocation inside of sqlite3_str_vappendf(), since it invokes sqlite3_initialize(), whereas sqlite3DbMallocRaw() does not. (check-in: a2f84c5631 ... user: drh tags: trunk) | 
| 15:55 |  | Fix the printf() optimization added at [ccb6b6c4ac21742d] so that sqlite3_snprintf() does not incorrectly truncate floating-point conversions that are close to filling the buffer, as reported in forum post 2026-04-10T13:48:12z. (check-in: a50521a160 ... user: drh tags: trunk) | 
| 13:43 |  | Prototype sqlite3_incomplete() - a variant of sqlite3_complete() that returns additional information about what is needed to complete the statement. (check-in: a698fbfb8b ... user: drh tags: cli-prompt-redo) | 
| 10:48 |  | Attempt to work around a bug in legacy Microsoft compilers. Forum post 2026-04-10T06:33:11z. (check-in: 661e141362 ... user: drh tags: trunk) | 
| 10:27 |  | Change a variable name to try to quash a compiler warning reported by forum post 2026-04-10T09:33:27z. This is just a guess, as we have no way of reproducing the warning, and hence no way of knowing if it fixes the problem. (check-in: e065c019d2 ... user: drh tags: trunk) | 
| 08:36 |  | Correct shell result code propagation regression introduced in [ff084ae341eab5c4] and reported in forum post 6fa3247e9724844c. (check-in: a76682a3b1 ... user: stephan tags: trunk) | 
| 00:55 |  | Improvements to the way \x works so that it can be used to change the prompt color. This really messes with linenoise, though. (check-in: bbee56f970 ... user: drh tags: cli-prompt-redo) | 
| 2026-04-09 |  |  | 
| 23:37 |  | A prototype of code that shows how to expand backslash escapes in the CLI prompt. (check-in: 9cf2895967 ... user: drh tags: cli-prompt-redo) | 
| 22:14 |  | Store the prompts as part of the ShellState object, not as global variables. (check-in: 39f6c5a8a7 ... user: drh tags: cli-prompt-redo) | 
| 20:21 |  | Strip out the legacy dynamic prompt generation. It will go back in as I add the user-defined prompt escapes. (check-in: ab08b7a8ef ... user: drh tags: cli-prompt-redo) | 
| 18:51 |  | Clean-ups in prompt generation in the CLI. (check-in: e2c9d4c6b7 ... user: drh tags: cli-prompt) | 
| 16:16 |  | Use O_TMPFILE (on systems that support it) to securely create a temporary file that is inaccessible to any other process. (check-in: 4609ba34b8 ... user: drh tags: trunk) | 
| 16:08 |  | Updates to testrunner.tcl: Shorten the prefix on created subdirectories from "testrunner_bld_" to just "testrun_". Do not append ".exe" to the names of created subdirectories. Add the "clean" command to recursively remove all subdirectories and files created by a test run. (check-in: 450c7edf12 ... user: drh tags: trunk) | 
| 16:03 |  | Version number to 3.54.0 to begin the next development cycle. (check-in: 4c2a5c9bb4 ... user: drh tags: trunk) | 
| 14:12 |  | Make use of O_TMPFILE for more secure temporary file creation, when that option is available. (closed check-in: 094fc6da07 ... user: drh tags: o_tmpfile) | 
| 13:21 |  | Merge the 3.53.0 release changes into the cli-prompt branch (check-in: 8fd48b9fe4 ... user: drh tags: cli-prompt) | 
| 11:41 |  | Version 3.53.0 (check-in: 4525003a53 ... user: drh tags: trunk, release, major-release, version-3.53.0) | 
| 10:39 |  | Fix some buffer overreads that might occur in the session module when handling corrupt changesets. (check-in: cda5dd9bcb ... user: dan tags: trunk) | 
| 06:38 |  | When generating make.sh for testrunner, correctly quote the value of any options passed via environment variable OPTS. (check-in: 0837933b49 ... user: dan tags: trunk) | 
| 05:33 |  | Fix some buffer overreads that might occur in the session module when handling corrupt changesets. (closed check-in: 8fcf92e15d ... user: dan tags: session-fixes) | 
| 2026-04-08 |  |  | 
| 17:00 |  | Fix a buffer overflow bug in a recent check-in, reported by unsolicted email from OpenAI/Codex. (check-in: be891a137a ... user: drh tags: trunk) | 
| 2026-04-07 |  |  | 
| 15:54 |  | Fix a few doc typos and inconsistencies. No code changes. (check-in: 025abd4cf4 ... user: stephan tags: trunk) | 
| 12:51 |  | Fix incorrect datatype on the auto_ext_leak_tester() function in the CLI. (check-in: 4a36454904 ... user: drh tags: trunk) | 
| 11:31 |  | Fix redundant typedef in qrf.c when that file is amalgamated into tclsqlite3.c. (check-in: b8f0c71b48 ... user: drh tags: trunk) | 
| 09:15 |  | Replace a doc reference to a renamed file, as reported by forum post 5d9d70c618. No code changes. (check-in: c8121593fa ... user: stephan tags: trunk) | 
| 2026-04-06 |  |  | 
| 22:00 |  | Add a dummy auto-extension to the shell in SQLITE_DEBUG builds to validate that [c4dd7e8b36] does the right thing. (check-in: 164af3703b ... user: stephan tags: trunk) | 
| 20:24 |  | Add a call to sqlite3_reset_auto_extension() near the end of shell.c.in:main() to squelch a leak message when SQLITE_EXTRA_INIT adds auto-extensions, as reported in forum post 310cb231b07c80d1. (check-in: c4dd7e8b36 ... user: stephan tags: trunk) | 
| 2026-04-04 |  |  | 
| 14:43 |  | Mask the index to the WalHashLoc.aPgno array so that it OOB read/write is never possible. (check-in: 9d56df17b4 ... user: drh tags: trunk) | 
| 08:07 |  | Add some comments to the OPFS async proxy about potential future experimentation. No code changes. (check-in: 9c14ee7430 ... user: stephan tags: trunk) | 
| 2026-04-03 |  |  | 
| 19:48 |  | Enhance testrunner.tcl so that a PATTERN argument that begins with "~" is an "anti-pattern". If one or more PATTERN arguments are supplied, then only run tests that match one or more patterns *and* that do not match any anti-pattern. (check-in: c313d91bb6 ... user: drh tags: trunk) | 
| 13:54 |  | configure script: replace two 'file normalize' calls with file-normalize (a tcl/jimsh compatibility issue) to fix --with-linenoise. Problem reported in forum post ce688d13793bd78f. (check-in: 2be1c5a3e4 ... user: stephan tags: trunk) | 
| 11:48 |  | Fix Tcl9 compatibility issues in test_session.c. (check-in: 1387b58190 ... user: drh tags: trunk) | 
| 2026-04-02 |  |  | 
| 16:21 |  | Minor patch to test/json/json-speed-check.sh. That script is not maintained, but this fix at least gets it working for now. (check-in: 9d92e371ca ... user: drh tags: trunk) | 
| 11:43 |  | Improvements to the syntax hints in the continuation prompt of the CLI (check-in: 5c544f1696 ... user: drh tags: cli-prompt) | 
| 2026-04-01 |  |  | 
| 17:43 |  | Fix a potential 4-byte overread in the sessions module when processing a corrupt changeset. (check-in: c1c7e024c3 ... user: dan tags: trunk) | 
| 16:56 |  | Always use sqlite3_malloc64() in extensions, where possible. (check-in: 6194f3b531 ... user: drh tags: trunk) | 
| 16:11 |  | Fix a problem in the sessions module allowing a NULL pointer to be passed to memcpy(). (check-in: 91ead3c62d ... user: dan tags: trunk) | 
| 11:54 |  | Minor improvements to the documentation on how to compile for Unix. No changes to code. (check-in: 7bc1d0c457 ... user: drh tags: trunk) | 
| 11:39 |  | Doc cleanups in opfs-wl. No code changes. (check-in: 009b576dcd ... user: stephan tags: trunk) | 
| 2026-03-31 |  |  | 
| 18:49 |  | Add appropriate headers to ext/session/sqlite3session.h so that the documentation generator will pick up the API documentation for the new one-at-a-time changegroup APIs. (check-in: da514f10b7 ... user: drh tags: trunk) | 
| 10:59 |  | Add an API to the sessions module to add changes one at a time to an sqlite3_changegroup object. (check-in: cf1122ea64 ... user: dan tags: trunk) | 
| 10:52 |  | Update this branch with changes from trunk. (closed check-in: 716e62e3d1 ... user: dan tags: changegroup-change-api) | 
| 10:32 |  | Turn an incorrect assert() into a testcase(). Fix over-length code lines in the sqlite.h.in source file. Fix the documentation on OOM conditions in the sqlite3_value_text() family of APIs. (check-in: 9345827061 ... user: drh tags: trunk) | 
| 2026-03-30 |  |  | 
| 15:44 |  | Move some build docs around for clarity. Move the TARGET_DEBUG flags from the end of T.cc to the beginning so that -Ox flags can be passed to make for specific binaries without having to reconfigure. (check-in: 240294653c ... user: stephan tags: trunk) | 
| 10:00 |  | Remove an unused variable in sqlite3Reindex(). Minor enhancements to the header comment on sqlite3AtoF(). (check-in: fbba900728 ... user: drh tags: trunk) | 
| 2026-03-29 |  |  | 
| 23:37 |  | Enhance the printf() formatter to write floating point conversions directly into the output buffer, rather than staging into an intermediate buffer. (check-in: ccb6b6c4ac ... user: drh tags: trunk) | 
| 22:41 |  | Fix harmless compiler warnings about unused variable assignments. (leaf check-in: afe7e9fd1c ... user: drh tags: direct-printf) | 
| 19:23 |  | Merge trunk performance enhancements and fixes into the direct-printf branch. (check-in: 9fa9cd11f8 ... user: drh tags: direct-printf) | 
| 19:06 |  | Avoid using unsigned 64-bit integer division on platforms that do not support it in hardware. (check-in: 2197677491 ... user: drh tags: trunk) | 
| 10:32 |  | The _umulh() intrinsic function is not available on 32-bit windows. So don't use it there. (check-in: e75f30c7e6 ... user: drh tags: trunk) | 
| 2026-03-28 |  |  | 
| 20:24 |  | In sqlite3_str_vappendf(), write floating-point renderings directly into the output buffer, saving a memcpy(). (check-in: e4e040e7aa ... user: drh tags: direct-printf) | 
| 14:21 |  | Remove a harmless unused variable from the fp-speed-2.c benchmark. (check-in: ff9008e761 ... user: drh tags: trunk) | 
| 14:17 |  | Enhancements to the fp-speed-N.c programs, suggested by Andreas Kupries. (check-in: 7d81ff4c50 ... user: drh tags: trunk) | 
| 13:59 |  | Reduce the compiled size of the quote() SQL function. (check-in: 9ea980a356 ... user: drh tags: trunk) | 
| 13:30 |  | Turn an sqlite3VMPrintf() into an sqlite3DbStrDup(). (check-in: 75e8703664 ... user: drh tags: trunk) | 
| 2026-03-27 |  |  | 
| 20:18 |  | Fix up the floating-point speed tests so that they only need to be run once to get a performance comparision. Provide the fp-speed-test makefile target to build and run them both. (check-in: 09e0e78a82 ... user: drh tags: trunk) | 
| 19:25 |  | Add a comment to test/fptest01.sql to remind us that failures in that script might not be the fault of SQLite. (check-in: 65efcb0a2e ... user: drh tags: trunk) | 
| 18:58 |  | New test cases for text→float conversions. (check-in: bbf2d64837 ... user: drh tags: trunk) | 
| 16:09 |  | Enable the use of intrinsic high-precision multiplies on Windows ARM. Make the SQLITE_DISABLE_INTRINSIC option effective on high-precision multiple subroutines, for testing purposes. (check-in: efb6212de4 ... user: drh tags: trunk) | 
| 14:54 |  | Change the print resolution the fp-speed-1.c test to 17 digits. (check-in: c687b32a95 ... user: drh tags: trunk) | 
| 14:19 |  | Update fp-speed-1.c and fp-speed-2.c to include its own timers, so that it works on platforms that lack the "time" command. (check-in: 9b083070de ... user: drh tags: trunk) | 
| 14:03 |  | Add the fp-speed-2.c performance test program. (check-in: b788256f19 ... user: drh tags: trunk) | 
| 11:46 |  | Add a comment showing reserved file-control numbers to sqlite3.h (check-in: cd472d7c85 ... user: drh tags: trunk) | 
| 2026-03-26 |  |  | 
| 19:11 |  | Add SQLITE_STRICT_SUBTYPE to PRAGMA compile_options, an omission reported in forum post 3906a71526. (check-in: c5af6a1024 ... user: stephan tags: trunk) | 
| 18:19 |  | Enhance the return value from sqlite3AtoF() so that it reports if the input includes more than 19.25 significant digits. (check-in: 1721c8da76 ... user: drh tags: trunk) | 
| 16:33 |  | Revamp the return code from sqlite3AtoF() for additional information and for small speed improvement and size reduction. (check-in: f10a431ead ... user: drh tags: trunk) | 
| 14:19 |  | Improvements to the sqlite3AtoF() interface. Add a test-control for direct testing of sqlite3AtoF(). (check-in: 97ee48b6ef ... user: drh tags: trunk) | 
| 12:09 |  | Back out the sqlite3_atof() API, for now. (check-in: 7568d264be ... user: drh tags: trunk) | 
| 2026-03-25 |  |  | 
| 21:23 |  | Fix an off-by-one error that cause a loop to (harmlessly) run more times than necessary. Performance optimization, not a bug fix. (check-in: 58f4e058f1 ... user: drh tags: trunk) | 
| 21:05 |  | Performance optimization when zero-padding in printf(). (check-in: dbd81abdc6 ... user: drh tags: trunk) | 
| 19:36 |  | Small size reduction and performance increase in sqlite3Atoi64(). (check-in: c818165fd7 ... user: drh tags: trunk) | 
| 17:59 |  | Signification optimizations to layout of text for floating point conversions in sqlite3_str_vappendf(). (check-in: 6a74df6d6e ... user: drh tags: trunk) | 
| 11:19 |  | Handle the case where the compile-time default page-size is greater than 8192 bytes. (check-in: 3aea251f23 ... user: dan tags: trunk) | 
| 2026-03-24 |  |  | 
| 19:26 |  | Prevent overly long JSON paths from using too much stack space. (check-in: 82a0794814 ... user: drh tags: trunk) | 
| 12:39 |  | Limit JSONB recursion depth in the json_patch() function. (check-in: f8659b700c ... user: drh tags: trunk) | 
| 11:54 |  | Improved error messages for JSONB nested too deep. (check-in: 82036fafa1 ... user: drh tags: trunk) | 
| 11:33 |  | Remove a stale reference to the long-removed SQLITE_MUTEX_APPDEF, as pointed out in forum post 348453389b. (check-in: cb8e55b591 ... user: stephan tags: trunk) | 
| 11:25 |  | Minor simplification to JSON path error reporting. (check-in: 1cd30c4827 ... user: drh tags: trunk) | 
| 00:31 |  | Recognize artifical JSONB blobs with deep recursion as being ill-formed. (check-in: adac4ed1f1 ... user: drh tags: trunk) | 
| 2026-03-23 |  |  | 
| 21:03 |  | Move datatype and structure definitions up near the top in the CLI source code, to facilitate future changes. No changes to the actual logic (yet). (check-in: 6b3d84d34d ... user: drh tags: trunk) | 
| 13:17 |  | Require a button click to start the OPFS concurrency tester, rather than simply launching all N workers immediately on page load. (check-in: 3342d6786e ... user: stephan tags: trunk) | 
| 13:00 |  | Use sqlite3_atof() instead of atof() in the CLI. Dogfooding. (check-in: 276c350313 ... user: drh tags: trunk) | 
| 12:25 |  | Move the definition of the new sqlite3_atof() function in sqlite3.h so that it is adjacent to sqlite3_mprintf(). (check-in: 1553a60506 ... user: drh tags: trunk) | 
| 10:58 |  | Add the sqlite3_atof() interface. (check-in: 52f76c59f2 ... user: drh tags: trunk) | 
| 2026-03-22 |  |  | 
| 20:52 |  | Proposed new API routine: sqlite3_atof(). (closed check-in: db370f0f62 ... user: drh tags: atof) | 
| 18:26 |  | Performance enhancements and improved documentation for sqlite3AtoF(). (check-in: 62f0115096 ... user: drh tags: trunk) | 
| 17:50 |  | Additional performance improvements in sqlite3AtoF(). Also update the header comments. (closed check-in: 8c3a1b450d ... user: drh tags: optimize-atof) | 
| 02:51 |  | Further simplification and performance improvement (check-in: 6c3dc2430a ... user: drh tags: optimize-atof) | 
| 02:26 |  | Performance improvements in the floating-point number parser. (check-in: aea648f366 ... user: drh tags: optimize-atof) | 
| 2026-03-21 |  |  | 
| 19:49 |  | Because version 3.52.0 was withdrawn, that means QRF has never been officially released. Hence the new nMultiInsert field of the spec can be moved out of the extensions area and into the main part of the spec, and the version number can drop back from 2 to 1. (check-in: cf19982241 ... user: drh tags: trunk) | 
| 13:21 |  | Try to fix harmless compiler warnings from legacy C compilers in the CLI. (check-in: 8e1d0c4eac ... user: drh tags: trunk) | 
| 13:05 |  | Add support for REINDEX EXPRESSIONS (check-in: d56954146c ... user: drh tags: trunk) | 
| 12:55 |  | Undo "indices"→"indexes" changes in comments, to avoid diff clutter. Fix a couple of unreachable branches. (closed check-in: 0042ff9afd ... user: drh tags: reindex-expressions) | 
| 2026-03-20 |  |  | 
| 20:35 |  | Fix harmless compiler warnings in the csv.c. (check-in: 6ac701f89e ... user: drh tags: trunk) | 
| 19:30 |  | If the argument to REINDEX is EXPRESSIONS (with no schema prefix) then it updates both expression indexes, and any indexes named "expressions" or all indexes of any tables name "expressions". (check-in: 72650dc152 ... user: drh tags: reindex-expressions) | 
| 14:37 |  | If there are not collating sequences, tables, or indexes named "EXPRESSIONS" then the "REINDEX EXPRESSIONS" command rebuilds all expression indexes. (check-in: df5c5aa267 ... user: drh tags: reindex-expressions) | 
| 11:35 |  | Ensure that collation sequences on non-PK columns of a WITHOUT ROWID table are used correctly when they are part of a row value comparison. Fix for forum post 7a308e933d. (check-in: 078b3162d0 ... user: dan tags: trunk) | 
| 2026-03-19 |  |  | 
| 23:49 |  | Ensure that sqlite3RCStr values returned by json_group_array() and json_group_object() are zero-terminated. (check-in: 88a931ea2a ... user: drh tags: trunk) | 
| 17:56 |  | Update fts5 scan costs to take into account that a prefix query combined with a rowid equality constraint is still quite expensive. (check-in: 539769b63e ... user: dan tags: trunk) | 
| 17:08 |  | Restructure some internal subroutine signatures for improved performance of affinity type conversions. (check-in: 2bc6ce87b3 ... user: drh tags: trunk) | 
| 13:40 |  | Use compiler intrinsics to improve the performance of sqlite3Multiply160(). (check-in: 3c64f31392 ... user: drh tags: trunk) | 
| 10:51 |  | Do not assume that a recursive CTE emits rows in ORDER BY order. Fix for forum post deb1eadf4d677bd5. (check-in: cadb6cdd32 ... user: dan tags: trunk) | 
| 10:39 |  | Reverse the if() condition added by this branch. (closed check-in: a9a0dd521c ... user: dan tags: forum-deb1eadf4d) | 
| 2026-03-18 |  |  | 
| 22:31 |  | Continue enforcing the legacy declaration-before-statement C-language rule, since some downstream still uses it. (check-in: 5c237f1f86 ... user: drh tags: trunk) | 
| 20:17 |  | Add logic to silently and transparently deal with stale expression index values when deleting or update a table row, instead of reporting a corrupt database. Enhance PRAGMA integrity_check to identify non-serious stale expression index entries as such, rather than proclaiming that the indexes is broken. (check-in: 0ed040128b ... user: drh tags: trunk) | 
| 18:14 |  | Minor optimization to error message generation in integrity_check. (closed check-in: e0fa7c5000 ... user: drh tags: idxdelete-tolerance) | 
| 17:16 |  | Comment improvements. Ensure that tests in shellB.test that require virtual tables do not run when compiling without virtual table support. (check-in: a270c6cc0e ... user: drh tags: idxdelete-tolerance) | 
| 15:51 |  | Improve the integrity-check error message when floating point index values are off by one or two ULPs. Change the name of the eiib1.test test module to expridx1.test. (check-in: 4e996c183e ... user: drh tags: idxdelete-tolerance) | 
| 15:40 |  | Do not assume that a recursive CTE emits rows in ORDER BY order. Forum deb1eadf4d677bd5. (check-in: 971aa34b3f ... user: dan tags: forum-deb1eadf4d) | 
| 14:47 |  | Take care not to overread the record header when decoding the record in vdbeIsMatchingIndexKey(). (check-in: 3a275b5848 ... user: drh tags: idxdelete-tolerance) | 
| 14:01 |  | Pad the allocation in vdbeIsMatchingKey() a little to avoid undefined behaviour if the record is corrupt and getVarint32() reads past the end of it. (check-in: 9b0671a4f5 ... user: dan tags: idxdelete-tolerance) | 
| 13:49 |  | Adjust the fix at [f7389cdb129d3386] so that integrity_check output still appears prior to the SQLITE_CORRUPT return. (check-in: efd9a7a6c8 ... user: drh tags: idxdelete-tolerance) | 
| 12:36 |  | Simplification to the logic that decides whether or not an index field matches when floating point values are close but not exact. (check-in: 3897017c0e ... user: drh tags: idxdelete-tolerance) | 
| 11:15 |  | Fix harmless signed/unsigned compiler warnings. (check-in: aa211fa1d4 ... user: drh tags: idxdelete-tolerance) | 
| 11:06 |  | Fix some cases where integrity-check would fail to identify real values in indexed expressions that are with 2 ULPs of each other. (check-in: 40c5966f7b ... user: dan tags: idxdelete-tolerance) | 
| 11:04 |  | Fix harmless "unused variable" compiler warning. (check-in: 3380eb1ea7 ... user: drh tags: idxdelete-tolerance) | 
| 2026-03-17 |  |  | 
| 21:17 |  | Modify integrity-check so that it is only tolerant of very small distortions of real values in indexed expression fields. (check-in: 82e6de835b ... user: dan tags: idxdelete-tolerance) | 
| 18:55 |  | Improved error handling in OP_IFindKey and in vdbeIsMatchingIndexKey(). (check-in: f7389cdb12 ... user: drh tags: idxdelete-tolerance) | 
| 17:05 |  | When integrity-check finds a missing index entry, search a small range (currently 10) of entries around the missing key for an entry that matches all non-virtual and non-expression fields. (check-in: 978cedcbd3 ... user: dan tags: idxdelete-tolerance) | 
| 13:13 |  | Do not run index healing on an invalid cursor. (check-in: eb6403f02c ... user: drh tags: idxdelete-tolerance) | 
| 12:16 |  | Merge recent trunk enhancements into the idxdelete-tolerance branch (check-in: 9d84fdb781 ... user: drh tags: idxdelete-tolerance) | 
| 12:07 |  | Fix an assertion fault that could occur in RTree when given a corrupt database. (check-in: ed29fe973e ... user: drh tags: trunk) | 
| 11:26 |  | Fix display of P4_INDEX values in EXPLAIN output. (check-in: e717017f02 ... user: dan tags: idxdelete-tolerance) | 
| 11:22 |  | Fix compiler warning an inefficiency introduced by previous commit. (check-in: 03058d73d3 ... user: dan tags: idxdelete-tolerance) | 
| 11:19 |  | Typo fix in sqlite3Multiply128(): The datatype should be *unsigned*. (check-in: b459f6ff63 ... user: drh tags: trunk) | 
| 11:17 |  | Change the sqlite3VdbeFindDeleteKey() routine so that all index fields that are not expressions or virtual columns must match the table for the delete to succeed. (check-in: eccd9fcfa7 ... user: dan tags: idxdelete-tolerance) | 
| 10:19 |  | Fix a harmless compiler warning. (check-in: aa23d6cc69 ... user: drh tags: trunk) | 
| 09:53 |  | Merge trunk enhancements into the idxdelete-tolerance branch (check-in: 5f5ab3eb36 ... user: drh tags: idxdelete-tolerance) | 
| 09:49 |  | Enhance text→float conversions for exact rounding in all cases. (check-in: 704ddfd0df ... user: drh tags: trunk) | 
| 09:45 |  | Include the float-point table generator utility in the tools directory. (closed check-in: 4ab10b251d ... user: drh tags: fp-accuracy) | 
| 09:18 |  | Reorder bounds checking in sqlite3Fp10Convert2() (check-in: 23ad656edb ... user: drh tags: fp-accuracy) | 
| 2026-03-16 |  |  | 
| 19:14 |  | Remove a couple of unused variables. (check-in: f66cb3dc27 ... user: dan tags: idxdelete-tolerance) | 
| 16:44 |  | Further tests for the code on this branch. (check-in: f928a38091 ... user: dan tags: idxdelete-tolerance) | 
| 16:21 |  | If OP_IdxDelete cannot find an index entry to delete, first search 10 entries in either direction of where the entry should be in the index. If that fails, search the entire index for an entry with matching PK fields. (check-in: 2cb9f4e417 ... user: dan tags: idxdelete-tolerance) | 
| 15:49 |  | Fix a problem with the find-delete-key method on this branch. (check-in: 6f49b5ecad ... user: dan tags: idxdelete-tolerance) | 
| 15:35 |  | Rework the ".indexes" command of the CLI. (1) The PATTERN now matches the name of the index itself, not the name of the table containing the index, thus making the pattern actually useful. (2) System-generated indexes (for UNIQUE constraints and similar) are omitted unless the -a\|-all or --sys options are added. (3) The --expr option causes only expression indexes to be shown. (check-in: 13e2cad810 ... user: drh tags: trunk) | 
| 15:16 |  | Make OP_IdxDelete tolerant of small variations in index key values. (check-in: c4b0081f08 ... user: dan tags: idxdelete-tolerance) | 
| 11:14 |  | Properly fix temp triggers created as part of FK processing to their schemas. Otherwise they may become confused by similarly named child tables in other attached databases. Fix for forum post 636bd0180a. (check-in: 80bc5bc07e ... user: dan tags: trunk) | 
| 09:40 |  | Improve accuracy of text→floating-point conversions by moving from 64-bit to 96-bit approximations to powers of ten. (check-in: a19c0785dc ... user: drh tags: fp-accuracy) | 
| 2026-03-14 |  |  | 
| 16:30 |  | Add a missing return for the invalid arguments error case in sqlite3_test_control_fault_install. No current scripts trigger this error. (check-in: dc78d25874 ... user: stephan tags: trunk) | 
| 2026-03-13 |  |  | 
| 15:03 |  | Changes some legacy testing targets in the makefiles into aliases for "devtest" and "releasetest" (check-in: 4c54f22f7a ... user: drh tags: trunk) | 
| 14:31 |  | Changes some legacy testing targets in the makefiles into aliases for "devtest" and "releasetest" (check-in: 238e70fec0 ... user: drh tags: branch-3.51) | 
| 2026-03-12 |  |  | 
| 08:33 |  | Update the canonical JimTcl home page link (it was moved earlier today). (check-in: 568573f872 ... user: stephan tags: trunk) | 
| 2026-03-11 |  |  | 
| 15:17 |  | Replace several comment-based switch-fall-through notices with attribute based ones, as suggested in forum post b6ab8dc547. (check-in: 2b0209a3bd ... user: stephan tags: trunk) | 
| 14:43 |  | Improved defenses against mischief. (check-in: 8bdc0d485e ... user: drh tags: trunk) | 
| 2026-03-10 |  |  | 
| 22:43 |  | Fix harmless compiler warnings. (check-in: d7b4a4a3d3 ... user: drh tags: trunk) | 
| 21:01 |  | Fix the behavior dot-commands that appear as command line arguments to the CLI so that their effect persists into subsequent commands on the command line. This fix also improves error messages caused by command line arguments. Forum post 2026-03-10T19:26:06z. (check-in: ff084ae341 ... user: drh tags: trunk) | 
| 18:13 |  | Revamp the QRF insert-mode optimization of [659ff6ab55802507]. The control value is now nMultiInsert. Multiple rows are added to each INSERT statement until the number of bytes in that statement exceeds the nMultiInsert value. The CLI uses a default value of 3000, which provides a good balance between speed and prepare-statement size. The output from ".dump" now loads nearly 2x faster in some cases. (check-in: 55b0ce9f93 ... user: drh tags: trunk) | 
| 15:43 |  | Bug fix to the ".dump" command in the CLI: Temporarily disable character limit to QRF so as not to truncate values in the generated output. (check-in: f6787e4b50 ... user: drh tags: trunk) | 
| 14:02 |  | Enhance QRF with the mxTuple field, which if greater than 1 causes the QRF_STYLE_Insert style to group as many as mxTuple adjacent rows into a single INSERT statement. The field is accessible using the -mxtuple option in the TCL interface, and the --mxtuple option in ".mode". The output of the ".dump" and ".fullschema" commands responds to the current --mxtuple setting. (check-in: 659ff6ab55 ... user: drh tags: trunk) | 
| 2026-03-09 |  |  | 
| 14:04 |  | Remove an obsolete pragraph from the header comment on the fileio.c extension. (check-in: 17613b72f3 ... user: drh tags: trunk) | 
| 13:53 |  | In QRF, fix line-wrap indentation for "line" mode when using a non-standard column separator. (check-in: 00c96cca3d ... user: drh tags: trunk) | 
| 11:49 |  | Fix a problem with RIGHT JOIN and expressions on indexes. Forum post e3dba5426a. (check-in: f27d578eda ... user: dan tags: trunk) | 
| 2026-03-08 |  |  | 
| 19:31 |  | Fix the completion.c TVF so that the xNext method reports errors it encounters while running queries. (check-in: eebcdcceef ... user: drh tags: trunk) | 
| 18:50 |  | In the decimal.c extension, set a default MAX_DIGITS to 100 million. Do not allow integer overflow of the N parameter to the internal decimal_round() function. (check-in: d0e23423d1 ... user: drh tags: trunk) | 
| 17:07 |  | Consolidate the opfs and opfs-wl tester1 tests using the preprocessor. (check-in: 77846c8c7a ... user: stephan tags: trunk) | 
| 16:03 |  | Upgrade ext/wasm/c-pp-lite.c to its newer sibling because we've reached the older one's limits. This renames c-pp-lite.c to libcmpp.c to maintain both the SCM- and code lineage but that may end up looking a bit weird because the diff between the two is vast. (check-in: 2e2339bd9e ... user: stephan tags: trunk) | 
| 12:42 |  | JS doc updates. (check-in: 699295f84e ... user: stephan tags: trunk) | 
| 10:51 |  | Increase the version number on trunk to 3.53.0 (check-in: 0152b445ff ... user: drh tags: trunk) | 
| 08:32 |  | Merge opfs-wl into trunk, not so much for the "opfs-wl" VFS, which is stil tentative (working fine but with little perceived benefit), but for the "opfs" VFS cleanups which necessarily happened around it. (check-in: bcaa876c6d ... user: stephan tags: trunk) | 
| 08:09 |  | Update the tester1 SEE tests to account for opfs-wl. (closed check-in: 616d32c838 ... user: stephan tags: opfs-wl) | 
| 07:58 |  | Refactor tester1 OPFS test to support both the "opfs" and "opfs-wl" VFSes. (check-in: 17ac8d7bdf ... user: stephan tags: opfs-wl) | 
| 07:02 |  | Add parens around an expression in the CLI shell to account for operator precedence, as reported in forum post 856ff7a2f1. (check-in: da3ad170ae ... user: stephan tags: trunk) | 
| 05:53 |  | Expose sqlite3_bind_zeroblob() to JS/WASM. (check-in: 0c5d4fe0bd ... user: stephan tags: trunk) | 
| 2026-03-07 |  |  | 
| 22:37 |  | Use the UINT64_C() macro rather than the LLU suffix on integer literals for portability to older Microsoft compilers. Forum thread 0a9ffaff6fcdedb3. (check-in: 27be26566f ... user: drh tags: trunk) | 
| 21:07 |  | Give the fuzzcheck test program access to all the same extensions that dbsqlfuzz has access to. (check-in: d9c23b49e3 ... user: drh tags: trunk) | 
| 17:50 |  | Fix harmless compiler warnings on Windows. (check-in: 358d78da75 ... user: drh tags: trunk) | 
| 15:15 |  | Minor typo fixes from forum post 8fc8bc34291d6f45. (check-in: c8fb88c19e ... user: stephan tags: trunk) | 
| 12:00 |  | An attempt to get the CLI build to work with mingw32. Forum post 086cb1ef66. (check-in: d95b9e7c17 ... user: drh tags: trunk) | 
| 11:23 |  | Fix another memory leak following OOM in the decimal extension. (check-in: 414f6dbc09 ... user: drh tags: trunk) | 
| 06:10 |  | Extend the JS pre-bootstrapping configuration to include an option to disable inclusion of any given extension VFS (not the default VFSes, like memdb). The primary motivation for this is to give people who don't use OPFS to a way to keep the OPFS VFSes from loading their proxy workers, which are expensive. (check-in: 9a07eb7a94 ... user: stephan tags: opfs-wl) | 
| 04:19 |  | Fix the mangling of the opfs async proxy script's URI caused by both OPFS VFSes modifying it to accound for sqlite3.dir. Add a link to speedtest1 with opfw-wl to index.html. (check-in: c1cb61f473 ... user: stephan tags: opfs-wl) | 
| 03:32 |  | Cleanups and docs in the opfs vfs and its concurrency tester. Experimentally add sqlite3.capi.sqlite3_js_retry_busy(), which runs a callback repeatedly until it _stops_ returning (or throwing) an SQLITE_BUSY error. (check-in: 45c02ed216 ... user: stephan tags: opfs-wl) | 
| 03:29 |  | Elide more of the 64-bit was stuff from the default build target. (check-in: 3b74ad9081 ... user: stephan tags: opfs-wl) | 
| 02:42 |  | Enhancements and fixes to some of the extensions used in the CLI so that they can be fuzzed using dbsqlfuzz. (check-in: d800ea06d2 ... user: drh tags: trunk) | 
| 02:36 |  | The optional SQLITE_DECIMAL_MAX_DIGIT compile-time option limits the maximum number of digits that the decimal extension will operate on. (closed check-in: 4214485b2f ... user: drh tags: extfunc-fuzz) | 
| 02:19 |  | Cleanups and docs in the new opfs code structure. (check-in: 3b470c4c7a ... user: stephan tags: opfs-wl) | 
| 01:01 |  | For backwards compatibility, ensure that the "opfs" VFS does not specifically require Atomics.waitAsync() (a new requirement of "opfs-wl"), but make use of it if available. Only apply jitter to the concurrency test runs at random intervals. (check-in: f2175f526c ... user: stephan tags: opfs-wl) | 
| 00:14 |  | Reenable the is_base85() extension function that was disabled by [af18b68978ec9b1f]. (check-in: be07c16e3a ... user: drh tags: extfunc-fuzz) | 
| 00:10 |  | Fix another NULL ptr deref following OOM in the sha1 extension. (check-in: c4843e4cce ... user: drh tags: extfunc-fuzz) | 
| 00:03 |  | Fix a NULL pointer dereference following OOM in the sha1 extension. (check-in: 530b3f97ca ... user: drh tags: extfunc-fuzz) | 
| 2026-03-06 |  |  | 
| 23:58 |  | Fix a NULL pointer dereference following OOM in the decimal extension. (check-in: de37f30b21 ... user: drh tags: extfunc-fuzz) | 
| 23:56 |  | Fix a memory leak following OOM in the completion extension. (check-in: ff5b1efbf8 ... user: drh tags: extfunc-fuzz) | 
| 23:50 |  | Add some jitter and duration info to the opfs concurrency tester. Ensure that Atomics.waitAsync() is available for opfs-wl. Further testing has shown that Web opfs-wl is consistently fairer about doling out contested locks but it's dog slow compared to the original VFS under moderate concurrency. In single-connection use they're effectively on par. (check-in: a9aecc9875 ... user: stephan tags: opfs-wl) | 
| 23:37 |  | Fix a memory leak in the decimal extension that follows an OOM. (check-in: 8d59e47e7a ... user: drh tags: extfunc-fuzz) | 
| 22:21 |  | Get "opfs" and "opfs-wl" properly split. Speedtest1 and the concurrency tester are happy with opfs-wl but it is not yet plugged in to tester1. (check-in: 12dcd54259 ... user: stephan tags: opfs-wl) | 
| 22:09 |  | Rename the initializer functions for base64() and base85() to include the number at the end. (check-in: af18b68978 ... user: drh tags: extfunc-fuzz) | 
| 21:34 |  | Increase the version number to 3.52.1. (check-in: 94a650b169 ... user: drh tags: trunk) | 
| 21:31 |  | Fix a NULL pointer dereference in the (undocumented) two-argument version of the decimal() extension SQL function. Forum post fa9c2dc83f. (check-in: a0266351df ... user: drh tags: trunk) | 
| 19:39 |  | Small typo fix in the FP_DIGITS API docs. (check-in: 0286f97f34 ... user: stephan tags: trunk) | 
| 19:33 |  | Minor cleanups and docs. Teach the OPFS concurrency tester to deal with SQLITE_BUSY instead of failing. (check-in: 247ffed141 ... user: stephan tags: opfs-wl) | 
| 18:51 |  | Merge fixes that were deferred until after the 3.52.0 release, including: (1) Fix handling of DISTINCT with OUTER JOIN, (2) Enhance configure to auto-detect linenoise, (3) Add recent configuration settings to WASM. (check-in: 9d51bec1b1 ... user: drh tags: trunk) | 
| 18:26 |  | Fix the linenoise auto-detection added by the previous check-in. (closed check-in: e369585845 ... user: drh tags: pending-3.52) | 
| 17:10 |  | This one reliably runs 5 workers. Checking in before subsequent cleanups and debug output removal break it. (check-in: ba81d95feb ... user: stephan tags: opfs-wl) | 
| 16:15 |  | Correct an internal doc falsehood. (check-in: 53aa080e35 ... user: stephan tags: opfs-wl) | 
| 16:04 |  | This Web Lock impl can reliably run a single OPFS connection but rather unreliably 'loses' workers with higher counts, presumably due to deadlock or deadly embrace (how _all_ of them can deadlock at once is unclear, but clearly a bug). (check-in: d4e8583e2e ... user: stephan tags: opfs-wl) | 
| 16:01 |  | Version 3.52.0 (check-in: 557aeb4386 ... user: drh tags: trunk, release, major-release, version-3.52.0) | 
| 15:41 |  | Expose SQLITE_{DBCONFIG_FP_DIGITS,LIMIT_PARSER_DEPTH,PREPARE_FROM_DDL} to WASM and add support for DBCONFIG_FP_DIGITS to the variadic wrapper for sqlite3_db_config(). These unfortunately went overlooked before the 3.52 release. (closed check-in: 837069635e ... user: stephan tags: wasm-post-3.52) | 
| 14:33 |  | Add the new realpath() extension function to the list of functions prohibited in --safe mode of the CLI. (check-in: 5fa49c4d59 ... user: drh tags: trunk) | 
| 11:49 |  | Remove an extraneous OPFS metrics increment. (check-in: bf3548a377 ... user: stephan tags: opfs-wl) | 
| 11:18 |  | Uninstall faultsim at the end of the walrestart.test script. (check-in: 798905a127 ... user: dan tags: trunk) | 
| 09:11 |  | A slight simplification/optimization in the OPFS proxy's waitLoop(). (check-in: 521bb140b7 ... user: stephan tags: opfs-wl) | 
| 09:06 |  | Reimplement the OPFS async proxy's wait-forever loop to be more async-friendly for upcoming changes. (check-in: fd775772fb ... user: stephan tags: opfs-wl) | 
| 2026-03-05 |  |  | 
| 18:28 |  | Fix the build for -DSQLITE_OMIT_FLOATING_POINT. (check-in: f41b9ad98c ... user: drh tags: trunk) | 
| 18:16 |  | Enhance ./configure to automatically detect linenoise in ../linenoise relative to the build directory, or ../linenoise relative to the source directory, or at $HOME/linenoise. (check-in: eb6b47c667 ... user: drh tags: pending-3.52) | 
| 16:23 |  | Ensure xMutexInit() is called in SQLITE_THREAD_MISUSE_WARNINGS builds. (check-in: ac6a389e09 ... user: dan tags: trunk) | 
| 14:39 |  | Fix a long-standing problem with DISTINCT LEFT JOIN queries. (check-in: f43294a558 ... user: dan tags: pending-3.52) | 
| 11:51 |  | Strip the opfs-wl "back to formula", removing the current false starts so that this can be tried again without tripping over any cruft. The current impl is subject, with no obvious way out of it, to starvation in the async proxy. (check-in: d022f1f5e7 ... user: stephan tags: opfs-wl) | 
| 06:30 |  | The opfs-wl lock hang has been traced to starvation between the WebLock and the tight wait-on-VFS-calls Atomics.wait() loop. This can reportedly be resolved with another level of indirection in which the WebLock takes over the wait-on-VFS-calls part until it's unlocked, returning to the global loop when it's done. That exceeds this morning's ambitions but is next to try out. (check-in: 113bd910e1 ... user: stephan tags: opfs-wl) | 
| 00:30 |  | Add a little flexibility to checkpoint sizes in the walrestart.test script. (check-in: e438b564ca ... user: drh tags: trunk) | 
| 2026-03-04 |  |  | 
| 21:16 |  | Add some debugging output to opfs and track down the breakage to the initial WebLock request, which is never reaching its callback. Still broken, but this is progress. (check-in: 62fc8b35ae ... user: stephan tags: opfs-wl) | 
| 20:33 |  | Get opfs-wl plugged in to the concurrency tester. Somewhat ironically, all competing workers fail with locking errors while worker 1 is busy running off the rails somewhere. Stashing for closer investigation later. (check-in: 3b27310aa2 ... user: stephan tags: opfs-wl) | 
| 19:21 |  | Consolidate the last 200 lines of common OPFS VFS code. "opfs" still works, "opfs-wl" registers fine but is still otherwise untested. (check-in: 5978ee4902 ... user: stephan tags: opfs-wl) | 
| 17:54 |  | Factor out about 300 lines of common OPFS VFS bootstrapping code. (check-in: 57adecbab7 ... user: stephan tags: opfs-wl) | 
| 16:30 |  | Consolidate the OPFS VFS's metrics-tracking code. (check-in: b71c79ef96 ... user: stephan tags: opfs-wl) | 
| 14:33 |  | Baby steps in consolidating common OPFS VFS code. (check-in: b0dd23299e ... user: stephan tags: opfs-wl) | 
| 13:08 |  | Remove dead (commented-out) code that came to my attention because it ws nearby the fix of the previous check-in. (check-in: 61f8a28591 ... user: drh tags: trunk) | 
| 13:02 |  | Fix a code-generator bug introduced by the UNION/INTERSECT/EXISTS optimization of check-in [132ba781b031972c] and reported in forum post 2026-03-04T05:06:26Z. (check-in: 6ba7797cac ... user: drh tags: trunk) | 
| 11:37 |  | Consolidate much of the OPFS utility code into a new file for use by two of the OPFS VFSes. (check-in: db19a6e966 ... user: stephan tags: opfs-wl) | 
| 00:48 |  | A potential fix for the probable breakage of bundler-friendly builds in the previous check-in. Pending review from someone who uses those tools. (check-in: 8ea8577611 ... user: stephan tags: opfs-wl) | 
| 00:37 |  | opfs-wl is now loading and registering but it's still untested. Edit: this very possibly breaks bundler-friendly builds because the proxy Worker URLs now have a URL argument to distinguish between the two distinct instances. Maybe bundlers won't choke on that. (check-in: 9a471f7491 ... user: stephan tags: opfs-wl) | 
| 2026-03-03 |  |  | 
| 23:43 |  | An initial attempt to plugging opfs-wl into the build but its initial handshake with the async half collides with the opfs VFS's handshake, causing bootstrapping to fail miserably. We'll need to either devise a handshake which can differentiate between the two instances or we'll need to preprocess sqlite3-opfs-async-proxy into two copies. Move the (now) three copies of some common code shared by the opfs pieces into a preprocessor #include. (check-in: 1e0b72631a ... user: stephan tags: opfs-wl) | 
| 21:46 |  | Add a missing reset of the lock handshake SharedArrayBuffer slot. (check-in: 5910ed7ac8 ... user: stephan tags: opfs-wl) | 
| 21:07 |  | Implementations of WebLock-based opfs::xLock() and xUnlock(). Still completely untested and not yet integrated into the build. (check-in: 3343e3aabe ... user: stephan tags: opfs-wl) | 
| 19:43 |  | Avoid an obscure race condition between a checkpointer and a writer wrapping around to the start of the wal file. (check-in: 7168988acb ... user: dan tags: trunk) | 
| 18:34 |  | Merge trunk into the opfs-wl branch. (check-in: 81bc4b3b6a ... user: stephan tags: opfs-wl) | 
| 18:34 |  | Do not run test script walrestart.test with the "memsubsys1" permutation. (closed check-in: 703cbb0f76 ... user: dan tags: wal-restart-fix) | 
| 18:21 |  | Minor tweaks to the previous, for testability. (check-in: decb690154 ... user: drh tags: wal-restart-fix) | 
| 17:34 |  | Avoid an obscure race condition between a checkpointer and a writer wrapping around to the start of the wal file. (check-in: 053bd3930f ... user: dan tags: wal-restart-fix) | 
| 14:13 |  | Add JS tests for sqlite3_bind_zeroblob(). (closed check-in: 0473eeb3ce ... user: stephan tags: wasm-zeroblob) | 
| 01:48 |  | Expose sqlite3_bind_zeroblob() to JS/WASM, per request in forum post e7acf225f9. This is currently missing tests and is not planned for merge until the 3.53 dev cycle starts. (check-in: b0c2ef6046 ... user: stephan tags: wasm-zeroblob) | 
| 2026-03-02 |  |  | 
| 17:11 |  | Convert many sqlite3_realloc() calls to sqlite3_realloc64(). (check-in: 88dce64242 ... user: drh tags: trunk) | 
| 16:40 |  | Omit explain_i calls from TCL test scripts. (check-in: 204f25c882 ... user: drh tags: trunk) | 
| 15:34 |  | Fix TEMP INSTEAD OF triggers so that they work on TEMP views. Forum post 2026-02-28T23:33:04z. (check-in: 0596bd508f ... user: drh tags: trunk) | 
| 15:06 |  | Change some sqlite3_realloc() calls into sqlite3_realloc64(). (closed check-in: a391f56469 ... user: drh tags: realloc64) | 
| 13:44 |  | Remove the experimental and incomplete ext/repair extension, to prevent AIs from scanning the (incomplete) code and reporting bugs against it. (check-in: 213b1c6608 ... user: drh tags: trunk) | 
| 13:43 |  | Disable test shell1-5.0 as it is causing a use of uninitialized deep inside of TCL. (check-in: 440bd6091e ... user: drh tags: trunk) | 
| 11:41 |  | Use only 64-bit memory allocation in FTS5. Fix for UAF reported by Zijie Zhao. (check-in: e8976d5041 ... user: drh tags: trunk) | 
| 11:22 |  | Fix a possible 9-byte buffer overread in the zipfile extension hit when processing a corrupt zip file. Forum post 721a05d2c5. (check-in: 5db21813d1 ... user: dan tags: trunk) | 
| 00:06 |  | The intck01.sql test should only be run if virtual tables work. (check-in: 641d6f31a7 ... user: drh tags: trunk) | 
| 2026-03-01 |  |  | 
| 23:23 |  | More floating point conversion test cases. (check-in: 44a736a700 ... user: drh tags: trunk) | 
| 23:01 |  | New floating-point test case. (check-in: f353399932 ... user: drh tags: trunk) | 
| 22:36 |  | Fix an OOB read in the incremental integrity-check extension. (check-in: abecc8e388 ... user: drh tags: trunk) | 
| 20:22 |  | Simplified rounding logic in sqlite3Fp10Convert2(). (check-in: 8ac63ebc5c ... user: drh tags: trunk) | 
| 2026-02-28 |  |  | 
| 20:22 |  | Code changes in sqlite3Fp10Convert2() for easier maintenance. Does not affect the generated machine code. (check-in: 21c8fc7100 ... user: drh tags: trunk) | 
| 2026-02-27 |  |  | 
| 22:59 |  | Improvements to power-of-ten computations used for text ↔ binary64 conversions. (check-in: 0780bce854 ... user: drh tags: trunk) | 
| 20:37 |  | Minor simplification to the text→binary64 conversion algorithm. (check-in: 6632d51f16 ... user: drh tags: trunk) | 
| 18:32 |  | Two code comment typo fixes reported in the forum. No code changes. (check-in: 3fbd28fabc ... user: stephan tags: trunk) | 
| 13:25 |  | Fix an off-by-one error in the indentation of multi-line outputs in QRF_STYLE_Line mode. (check-in: d47499b21c ... user: drh tags: trunk) | 
| 11:36 |  | Add an assert() to sqlite3RegisterLikeFunctions() to hopefully prevent future false-positive bug reports coming out of CodeQL. (check-in: 7c5f4dcd74 ... user: drh tags: trunk) | 
| 06:33 |  | Another nano-optimization in kvvfs v2. (check-in: a9d60190d6 ... user: stephan tags: trunk) | 
| 03:21 |  | JS: slight performance improvements in KVVfsStorage. (check-in: 56edf2addb ... user: stephan tags: trunk) | 
| 03:11 |  | An initial copy/paste stub for experimenting with using WebLocks for a new copy of the OPFS vfs. This is not yet functional, or even loading, but needs stashing to avoid potential loss. (check-in: 372f18ae90 ... user: stephan tags: opfs-wl) | 
| 2026-02-26 |  |  | 
| 16:19 |  | Fix typos in the SQLITE_PREPARE_FROM_DDL documentation. (check-in: 9174e7dacf ... user: drh tags: trunk) | 
| 13:35 |  | Improvements to the SQLITE_PREPARE_FROM_DDL documentation. (check-in: c3288f1684 ... user: drh tags: trunk) | 
| 12:17 |  | Fix an inconsequential typo in the output of the datedebug() SQL function. (check-in: 3a92a53d8c ... user: drh tags: trunk) | 
| 11:03 |  | Fix a problem where the wrong collation could be used as part of a row-value comparison between columns with different collation sequences. Forum post 6ceca07fc3. (check-in: 212c68249c ... user: dan tags: trunk) | 
| 2026-02-25 |  |  | 
| 21:19 |  | The GCC bug is fixed in GCC-15. (check-in: 21f9abe6c9 ... user: drh tags: trunk) | 
| 12:44 |  | Reinstant the work-around for GCC compiler bug 96270 which is apparently still an issue as of gcc 13.3.0 with -m32 on Mint Linux. The test case in the bug report is fixed, but the vdbeMemRenderNum() routine in SQLite still shows the problem. (check-in: 7fae321095 ... user: drh tags: trunk) | 
| 01:04 |  | Use a 64-bit hash for the testing option SQLITE_CHECK_PAGES to maintain page alignment. (check-in: 75a35cd8f9 ... user: drh tags: trunk) | 
| 2026-02-24 |  |  | 
| 19:50 |  | Adjust the changes to shell8.test from [5da9bf09cc00faf9] so that they work with older versions of TCL. (check-in: c3b9e2cee3 ... user: drh tags: trunk) | 
| 19:37 |  | Rename SQLITE_ENABLE_MULTITHREADED_CHECKS to SQLITE_THREAD_MISUSE_WARNINGS. Run test cases with that option. Also add the SQLITE_THREAD_MISUSE_ABORT option. (check-in: be8c8b9cb7 ... user: drh tags: trunk) | 
| 2026-02-23 |  |  | 
| 19:51 |  | Fix an assert() that is incorrect when the SQLITE_DBCONFIG_FP_DIGITS setting is on a non-default value. (check-in: 3ca1ed81c4 ... user: drh tags: trunk) | 
| 13:29 |  | Add test cases for the symlink defenses of the previous check-in. (check-in: 5da9bf09cc ... user: drh tags: trunk) | 
| 12:19 |  | Change the SQLAR archive extraction algorithm in the CLI so that it uses the newly enhanced realpath() SQL function to guard against attacks that use symlinks to try to write files outside of the destination directory. Forum post 641b09daa17d9086. (check-in: 7cced53e8c ... user: drh tags: trunk) | 
| 11:44 |  | Enhance the realpath() SQL function in the fileio.c extension so that it works ever for pathnames that do not exist. (check-in: 27a5735fb1 ... user: drh tags: trunk) | 
| 01:34 |  | When doing an SQLAR archive extraction in the CLI, postpone creating symlinks until after all files and directories have been created. This prevents a hostile archive from creating a symlink through which it can subsequently write content outside of the target directory. Forum post 9e176adfef91c207. (check-in: 9719034d4d ... user: drh tags: trunk) | 
| 00:57 |  | Improved implementation of realpath() in the fileio.c extension that does not require the last element of the path to actually exist. (check-in: 4df4999484 ... user: drh tags: trunk) | 
| 2026-02-22 |  |  | 
| 20:44 |  | Add the realpath() SQL function to the fileio.c extension. And clean up the UTF8 handling on the Windows side of that extension while we are at it. (check-in: 8bb8941930 ... user: drh tags: trunk) | 
| 2026-02-21 |  |  | 
| 21:19 |  | Fix typos and include wording in the documentation for SQLITE_DBCONFIG_FP_DIGITS. (check-in: c30cee1224 ... user: drh tags: trunk) | 
| 20:20 |  | Fix a harmless warning in the decimal extension. (check-in: f16c7c4000 ... user: drh tags: trunk) | 
| 20:03 |  | Improved rounding of double→text conversions for rounding digits of 17. Add the new sqlite3_db_config(), SQLITE_DBCONFIG_FP_DIGITS, to specify the number of significant digits to preserve on conversions. (check-in: 78c12b4f6a ... user: drh tags: trunk) | 
| 19:26 |  | New sqlite3_db_config(SQLITE_DBCONFIG_FP_DIGITS) that let's the application specify the number of significant digits that double→text conversions will attempt to preserve. (closed check-in: 3f16985dcc ... user: drh tags: fp-performance-v2) | 
| 13:57 |  | New test cases for floating-point conversions. (check-in: 3033fe97b1 ... user: drh tags: fp-performance-v2) | 
| 13:40 |  | Remove an unreachable branch. (check-in: 93f90eacc0 ... user: drh tags: fp-performance-v2) | 
| 10:57 |  | Increase the precision of double → text conversions that happen in sqlite3_column_text(), or sqlite3_value_text(), or in the CAST() operator, or similar, so that round-tripping the value back to double results in exactly the same value. (check-in: ef0049e3ad ... user: drh tags: fp-performance-v2) | 
| 00:56 |  | Further simplification, performance gain, and size reduction in sqlite3FpDecode(). (check-in: 5954f22ceb ... user: drh tags: fp-performance-v2) | 
| 00:20 |  | Small performance increase and size reduction in sqlite3FpDecode() by using local variables instead of structure elements. (check-in: b4c378bba5 ... user: drh tags: fp-performance-v2) | 
| 2026-02-20 |  |  | 
| 20:43 |  | Inconsequential changes to floating-point conversion, amounting to mere code cleanup to aid comprehension. (check-in: 6d9c29123b ... user: drh tags: trunk) | 
| 16:21 |  | In the decimal extension, an optional second argument to the decimal() and decimal_exp() functions rounds the value to the number of significant digits specified by that argument. (check-in: cb24edf1af ... user: drh tags: trunk) | 
| 14:22 |  | Fix a cse in the fiddle.debug build where it could not overwrite its read-only/generated index.html. (check-in: 7adb2c0f43 ... user: stephan tags: trunk) | 
| 14:20 |  | Add new functions ieee754_to_int() and ieee754_from_int() to the ieee754 extension. No changes to the core SQLite. (check-in: 8660d3e94c ... user: drh tags: trunk) | 
| 12:27 |  | Improved rounding of floating-point conversions in some corner cases. (check-in: 436dccd426 ... user: drh tags: trunk) | 
| 2026-02-19 |  |  | 
| 17:20 |  | In the CLI, the ".quit" command in a --cmd argument causes an immediate exit. Forum post f057122b68. (check-in: 4731cc8856 ... user: drh tags: trunk) | 
| 12:59 |  | Improvements to rounding behavior in the new floating-point conversion logic. (check-in: e540f6c370 ... user: drh tags: trunk) | 
| 2026-02-17 |  |  | 
| 20:28 |  | Restore the CLI function dtostr() back to its original purpose of converting floating point values to decimal using the host computer's C-library. (check-in: f3cc05ba7f ... user: drh tags: trunk) | 
| 14:28 |  | Minor fixes to tmstmpvfs.c. (check-in: 091bd90ed5 ... user: drh tags: trunk) | 
| 14:02 |  | Increase the number of floating-point precision test cases by 9x, and increase the require precision to 16 digits. (check-in: 62c2cf74db ... user: drh tags: trunk) | 
| 12:23 |  | Fix a harmless compiler warning from MSVC. (check-in: 12f3a6d0aa ... user: drh tags: trunk) | 
| 11:28 |  | New assert()s for the return value from sqlite3SchemaToIndex(). (check-in: 2610105a43 ... user: drh tags: trunk) | 
| 01:04 |  | Further simplifications, performance improvements, and size reductions to the numeric to text conversion logic. (check-in: dd5af703e1 ... user: drh tags: trunk) | 
| 00:37 |  | Faster conversion of integers to text, by converting two digits at a time. (closed check-in: 90300e02f0 ... user: drh tags: numeric-conversion) | 
| 2026-02-16 |  |  | 
| 23:52 |  | Simplifications to sqlite3AtoF(), resulting in slightly faster performance and slightly smaller size. (check-in: 6b9ab64176 ... user: drh tags: numeric-conversion) | 
| 17:22 |  | Fix a bad assert() in the previous check-in. (check-in: a4540582b5 ... user: drh tags: numeric-conversion) | 
| 16:56 |  | Refactor the sqlite3AtoF() routine so that it requires a zero-terminated UTF-8 input. When the need arises to convert UTF16 or non-terminated strings, wrapper functions are used. Together, this makes the code slightly smaller and faster. (check-in: 67c7c72e9b ... user: drh tags: numeric-conversion) | 
| 16:56 |  | Fix another case where SQLite might fail to identify an ON clause on a RIGHT or FULL join that refers to FROM-clause elements to its right. (check-in: e956b36063 ... user: dan tags: trunk) | 
| 11:59 |  | Fix a problem with identifying ON clauses attached to RIGHT or FULL JOINs (which is an error) if the join appeared in a flattened sub-query. (check-in: cf2dc6dfad ... user: dan tags: trunk) | 
| 11:14 |  | Performance improvement in floating-point conversions. (check-in: b5ebbd0041 ... user: drh tags: trunk) | 
| 2026-02-15 |  |  | 
| 18:41 |  | Avoid the big power-of-ten lookup table. (closed check-in: 245ac7d9ec ... user: drh tags: fp-performance) | 
| 2026-02-14 |  |  | 
| 22:04 |  | Fix to [9d3a12d3926d55ef] to force the static const string literal to have two-byte alignment. Necessary to get that patch to work in Mac-ARM. (check-in: a80c847169 ... user: drh tags: fp-performance) | 
| 21:47 |  | Add the "LLU" suffix to 64-bit integer constants, for portability to older compilers. (check-in: 4bf0811066 ... user: drh tags: fp-performance) | 
| 21:31 |  | Fix to the generic version of the 128-bit multiple subroutine. (check-in: 9c3d09ee75 ... user: drh tags: fp-performance) | 
| 20:48 |  | Another minor performance refinement. (check-in: 9d3a12d392 ... user: drh tags: fp-performance) | 
| 19:42 |  | Minor corrections. (check-in: 93fdee2002 ... user: drh tags: fp-performance) | 
| 19:36 |  | Improved comment. No code changes. (check-in: c5a4da1bf7 ... user: drh tags: fp-performance) | 
| 17:58 |  | Use a precomputed table of 696 powers of 10 to improve the performance of floating-point to decimal and decimal to floating-point conversions. (check-in: 64d9b7becf ... user: drh tags: fp-performance) | 
| 2026-02-13 |  |  | 
| 16:02 |  | Add new assert() statements to demonstrate that the complaint given in forum post 56ad8b632c is not a real bug. (check-in: d62999907d ... user: drh tags: trunk) | 
| 12:20 |  | Use only a single bit, rather than a whole byte, to store infrequently accessed boolean values in the Parse object. (check-in: e1bcd7e1cf ... user: drh tags: trunk) | 
| 11:40 |  | Add a "const" to avoid a harmless false-positive compiler warning. (check-in: d44af6cc09 ... user: drh tags: trunk) | 
| 2026-02-12 |  |  | 
| 18:15 |  | Respect "const" strings in the sqlite3ShadowTableName() function. Patch moved to trunk, where it was intended. (check-in: 51b5afdfac ... user: drh tags: trunk) | 
| 12:47 |  | Respect "const" strings in the sqlite3ShadowTableName() function. (closed check-in: 0343ecfaa7 ... user: drh tags: wrong-branch, fp-performance) | 
| 2026-02-11 |  |  | 
| 19:42 |  | Minor performance enhancement to floating-point rendering. (check-in: 38d8c0d8a0 ... user: drh tags: fp-performance) | 
| 2026-02-10 |  |  | 
| 19:33 |  | Correction to date/time computations associated with the timestamp-vfs. (check-in: b67889e4f1 ... user: drh tags: trunk) | 
| 18:28 |  | Minor tweaks to the star-query optimization in the query planner. (check-in: 5021e114b2 ... user: drh tags: trunk) | 
| 2026-02-09 |  |  | 
| 19:58 |  | Immediately reset all pending prepared statements on any call to sqlite3_set_authorizer(), even if the authorizer is being disabled. (check-in: 5c0468acd1 ... user: drh tags: trunk) | 
| 14:45 |  | Do not allow table-function argument expressions on the RHS of an OUTER join to refer to FROM clause elements to their right. (check-in: 021e48e289 ... user: dan tags: trunk) | 
| 2026-02-08 |  |  | 
| 12:13 |  | Fix a method name typo in test-only code which could hide the triggering error condition. (check-in: b2c1a4184c ... user: stephan tags: trunk) | 
| 05:08 |  | JS doc touchups. (check-in: dcd22fd6d1 ... user: stephan tags: trunk) | 
| 2026-02-07 |  |  | 
| 13:41 |  | Improved output from calls to sqlite3ShowSrcList() made from a symbolic debugger. (check-in: 241d23f8d3 ... user: drh tags: trunk) | 
| 2026-02-05 |  |  | 
| 21:14 |  | Do a better job of not generating unnecessary Bloom filters associated with IN operators. (check-in: 3861df5c12 ... user: drh tags: trunk) | 
| 13:03 |  | Fix harmless compiler warning when the CLI is compiled with SQLITE_OMIT_PROGRESS_CALLBACK. (check-in: d6150c813c ... user: drh tags: trunk) | 
| 2026-02-04 |  |  | 
| 20:51 |  | Display scanstatus results in neat columns. (check-in: c476d956d0 ... user: drh tags: trunk) | 
| 18:10 |  | Improved output for ".scanstatus est" in the CLI. (check-in: e6902937ec ... user: drh tags: trunk) | 
| 17:52 |  | Improved display of scan-status estimates. (closed check-in: 70ef095740 ... user: drh tags: scanstatus-improvements) | 
| 13:35 |  | Render scan-status values with 3 or 4 significant digits only, for improved readability. (check-in: d9102ff0c4 ... user: drh tags: scanstatus-improvements) | 
| 11:51 |  | Fix scanstatus output in QRF so that it shows the total number of cycles on the "QUERY PLAN" line. (check-in: 611cef0ec2 ... user: drh tags: scanstatus-improvements) | 
| 2026-02-03 |  |  | 
| 17:21 |  | Improve the query planner by reducing the estimated number of output rows for each stage of a join if there are LIKE, GLOB, REGEXP, or MATCH constraints on the table where the pattern string is a constant. The reduction in the output row estimate is proportional to the length of the pattern string. (check-in: 6852843e6a ... user: drh tags: trunk) | 
| 17:07 |  | Minor change to test/dotcmd01.test so that it works with STAT4. (check-in: 8aaf5b6b6c ... user: drh tags: trunk) | 
| 16:29 |  | Improved comments on this enhancement. No code changes. (closed check-in: 510a6738e5 ... user: drh tags: prune-by-like-glob) | 
| 14:00 |  | Refactor the implementation of this enhancement to keep all the code inside the query planner, not leaking out into SQL function implementations. Expand the enhancement to cover MATCH and REGEXP operators and overloads of LIKE and GLOB. (check-in: 96f8ce225e ... user: drh tags: prune-by-like-glob) | 
| 2026-02-02 |  |  | 
| 20:54 |  | When reducing the truth probability of a LIKE/GLOB constraint, only consider non-wildcard characters in the pattern. (check-in: 2ba8fc290d ... user: drh tags: prune-by-like-glob) | 
| 16:03 |  | Do not reduce the estimated truth probability of LIKE and GLOB operators by quite so much. Fix a single test case whose output changed. (check-in: fdebbedbd9 ... user: drh tags: prune-by-like-glob) | 
| 14:24 |  | Merge the latest trunk enhancements into the prune-by-like-glob branch. (check-in: d9d3d3a807 ... user: drh tags: prune-by-like-glob) | 
| 11:42 |  | Update session module test code to correctly use Tcl_Size instead of int. (check-in: 619060c226 ... user: dan tags: trunk) | 
| 2026-02-01 |  |  | 
| 21:10 |  | Add the "--timeout SECONDS" option to the ".progress" command in the CLI. (check-in: 8eb5c88aeb ... user: drh tags: trunk) | 
| 15:58 |  | Enhance the ".timer" command in the CLI to accept the "once" argument, and so that it leaves its last real-time result in the $TIMER variable. Also fix a harmless warning from an earlier check-in. (check-in: 8ad7dffc26 ... user: drh tags: trunk) | 
| 00:37 |  | An experimental query-planner change that reduces the estimated number of output rows for FROM clause terms that are restricted by a LIKE or GLOB operator, based on the number of bytes in the pattern. The idea is that longer patterns will match fewer records and hence should reduce the estimated output count. The implementation is not workable as it stands now. This is just a crazy idea, saved for future reference. (check-in: 97bcb56a20 ... user: drh tags: prune-by-like-glob) | 
| 2026-01-31 |  |  | 
| 02:17 |  | Fix a missed optimization opportunity due to a typo in check-in [898bfa1afd8260ea]. Reported by forum post 2026-01-31T00:49:53z. (check-in: 3a4f9a323d ... user: drh tags: trunk) | 
| 2026-01-30 |  |  | 
| 17:10 |  | Make CROSS JOIN a join reorder barrier again, as the SQLite documentation says it is. It mistakenly stopped being a join barrier with check-in [c1ea064948ba08c4]. (check-in: d294106ecb ... user: drh tags: trunk) | 
| 16:03 |  | Fix the new CLI so that the ".eqp" setting does not affect the output of other dot commands such as .schema or .tables. (check-in: a5a1b69b3f ... user: drh tags: trunk) | 
| 13:11 |  | Change the new escape-character mechanism for CSV import so that there are separate options for an escape character for quoted and unquoted fields of the CSV. (check-in: 293ec20e29 ... user: drh tags: trunk) | 
| 12:27 |  | Undo the previous check-in. In its place, add the --escape option to the ".import" command. (check-in: f11a05d52a ... user: drh tags: trunk) | 
| 12:15 |  | Enhance the CSV import capability in the CLI so that it understands backslash-escaped double-quotes. (check-in: 7ba0594d3c ... user: drh tags: trunk) | 
| 06:37 |  | kvvfs fix for npm ticket #146: use of a test-mode-only symbol in non-test runs leads to a null deref in xFileControl(). (check-in: 407724c4e8 ... user: stephan tags: trunk) | 
| 2026-01-28 |  |  | 
| 17:25 |  | Capture sqlite3_rsync's remote-end result code so the local side can exit with non-0 if, e.g., the remote sqlite3_rsync binary is found but fails to start. Confirmation received that it resolves the motivating problem report. (check-in: 971d51374e ... user: stephan tags: trunk) | 
| 16:59 |  | Improved (faster) bytecode for the merge algorithm. (check-in: 0b7f4b97e6 ... user: drh tags: trunk) | 
| 10:52 |  | Add a comment to sqlite3Stat4Value() explaining that it read a few bytes past the end of the specified buffer. (check-in: b95644eafd ... user: dan tags: trunk) | 
| 2026-01-27 |  |  | 
| 23:33 |  | Avoid unsigned integer overflow when evaluating an array index in a JSON path expression. Forum post 2026-01-27T14:18:49z. (check-in: 631c8d44cd ... user: drh tags: trunk) | 
| 22:27 |  | Improved byte-code coverage testing verification in the merge algorithm. (check-in: e49ec0e24b ... user: drh tags: trunk) | 
| 16:18 |  | The tmstmpvfs.c extension should not modify the content of pages going into the WAL file, as that would corrupt the page checksum used for recovery. Instead, only insert timestamp information as content is written into the database file. (check-in: dabaeeb1da ... user: drh tags: trunk) | 
| 16:15 |  | Teach sqlite3_rsync to increment its error count when a child process fails, based on forum post 8fe404e547faa42e. This passes basic sanity tests but requires more testing and needs a review of the final 'else' block in the new code. (closed check-in: 8be55d405f ... user: stephan tags: rsync-child-rc) | 
| 14:59 |  | For CSV import in the CLI, when the table is created automatically because it does not previously exist, make the column types "ANY" instead of "TEXT" so that they will automatically adjust to different datatypes in the input text. (check-in: aff74e71ea ... user: drh tags: trunk) | 
| 14:27 |  | Add the --csv option to the showtmlog utility program. (check-in: 2e96e8efa9 ... user: drh tags: trunk) | 
| 14:00 |  | Fix trivial buffer overreads in the sessions module that could occur when parsing changeset blobs. (check-in: 661878a628 ... user: dan tags: trunk) | 
| 12:50 |  | Additional usage notes added to the header comment of the tmstmpvfs.c source file. No code changes. (check-in: d1b8e7740b ... user: drh tags: trunk) | 
| 12:02 |  | Enhanced documentation in the header comment of tmstmpvfs.c. (check-in: 3d37da3cb5 ... user: drh tags: trunk) | 
| 11:19 |  | Fix frame number computation in the log generated by tmstmpvfs.c. (check-in: 57fda9dd85 ... user: drh tags: trunk) | 
| 2026-01-26 |  |  | 
| 21:20 |  | Enhancements to tmstmpvfs.c: (1) Use the exact same timestamp on logfile entries as on the pages that control, where appropriate. (2) Include the WAL frame number in ELOG_CKPT_PAGE logfile entries. (check-in: fcf1629785 ... user: drh tags: trunk) | 
| 20:17 |  | Add test for fts3 compress= and uncompress= options. (check-in: bace9de67d ... user: dan tags: trunk) | 
| 19:37 |  | Improve the comments on the generated byte-code for the merge algorithm. (check-in: 4468c9e1a2 ... user: drh tags: trunk) | 
| 13:54 |  | Fix the initialization of the sqlite3Api[] constant to conform with the previous check-in. (check-in: 0a31a74084 ... user: drh tags: trunk) | 
| 13:44 |  | Add alternative sqlite3_carray_bind_v2() interface. (check-in: 925384d771 ... user: drh tags: trunk) | 
| 10:53 |  | Fix speedtest1.wasm build regression introduced with Emscripten 5.0.0. (check-in: 4733d351ec ... user: stephan tags: trunk) | 
| 00:11 |  | Fix a possible problem following OOM in a WITH statement. (check-in: c38797d400 ... user: drh tags: trunk) | 
| 2026-01-25 |  |  | 
| 23:28 |  | Always use the sort-and-merge algorithm for EXCEPT, INTERSECT, and UNION, even if there is no ORDER BY clause. (check-in: 132ba781b0 ... user: drh tags: trunk) | 
| 20:15 |  | The Select.addrOpenEphm field is no longer needed. Remove it and simplify related code. (closed check-in: 1fe5a1ca72 ... user: drh tags: union-by-merge) | 
| 18:21 |  | Refactor one function name. Comment improvements. No functional changes. (check-in: 5b4c0a5e86 ... user: drh tags: union-by-merge) | 
| 15:18 |  | Fix the jsonb_group_array() and jsonb_group_object() functions so that they return JSONB as they should, and not text JSON, when the array or object is empty. Forum post 2026-01-25T00:47:06z. (check-in: 8b53b97833 ... user: drh tags: trunk) | 
| 13:26 |  | Additional simplifications to achieve 100% MC/DC. (check-in: 781b7ec0bc ... user: drh tags: union-by-merge) | 
| 11:56 |  | Fix harmless compiler warnings in FTS5. (check-in: 29d9132a76 ... user: drh tags: trunk) | 
| 11:41 |  | Remove additional unnecessary code associated with the setup phase of a recursive CTE. (check-in: 8273551376 ... user: drh tags: union-by-merge) | 
| 2026-01-24 |  |  | 
| 17:18 |  | Assert() statements added to sqlite3_result_text64() and sqlite3_bind_text64() to detect misuse of SQLITE_UTF8_ZT that can result in undefined behavior. (check-in: e761fcc683 ... user: drh tags: trunk) | 
| 12:19 |  | Expound upon the meaning of SQLITE_UTF8_ZT, in an attempt to head off confusion. (check-in: 9a74d0c3e7 ... user: drh tags: trunk) | 
| 01:17 |  | Expose SQLITE_UTF_ZT to WASM for completeness's sake, but the WASM build does not expose sqlite3_bind_text64() or sqlite3_result_text64() because it only supports UTF8 encoding, so ZT does not currently have a genuine use there. (check-in: d931831ce9 ... user: stephan tags: trunk) | 
| 00:33 |  | Fix an incorrect assert(). (check-in: b9ce349e1e ... user: drh tags: union-by-merge) | 
| 2026-01-23 |  |  | 
| 20:46 |  | In the cost metrics of the query planner, the estimated number of output rows from an EXISTS-to-JOIN loop should not be more than 1. Forum post 989880d0aa. (check-in: fc4e93a381 ... user: drh tags: trunk) | 
| 18:52 |  | More code simplifications. (check-in: 3d76732a45 ... user: drh tags: union-by-merge) | 
| 17:06 |  | Fix a minor EXPLAIN QUERY PLAN formatting problem. Remove a NEVER() that is reachable. (check-in: 423173079e ... user: drh tags: union-by-merge) | 
| 16:07 |  | Remove code that is no longer used. (check-in: 8d7f91e07b ... user: drh tags: union-by-merge) | 
| 14:37 |  | Continuing work toward using sort-and-merge for UNION. (check-in: 94bf43634b ... user: drh tags: union-by-merge) | 
| 11:44 |  | Merge the latest trunk enhancements into the union-by-merge branch. (check-in: bdf4d6a939 ... user: drh tags: union-by-merge) | 
| 00:55 |  | Add the SQLITE_UTF8_ZT value which can be the encoding argument to sqlite3_result_text64() or sqlite3_bind_test64() to indicate that the argument is UTF-8 text that is zero-terminated. (check-in: cef0eed805 ... user: drh tags: trunk) | 
| 2026-01-22 |  |  | 
| 19:29 |  | Merge the latest trunk enhancements into the utf8-zt branch. (closed check-in: 0dd1633d34 ... user: drh tags: utf8-zt) | 
| 19:02 |  | Enhance the EXISTS-to-JOIN optimization so that the inserted JOIN terms are not required to be on the inner-most loops, as long as all dependencies for the EXISTS-to-JOIN loops are in outer loops. This addresses the performance concern of forum post 2026-01-21T19:49:04z. Test cases in TH3. (check-in: 298d5c8fa6 ... user: drh tags: trunk) | 
| 15:59 |  | For the ".eqp full" show both the QUERY PLAN and the full byte code, just as it has in the past. (check-in: 756484fcaa ... user: drh tags: trunk) | 
| 2026-01-21 |  |  | 
| 19:24 |  | Add a new encoding constant SQLITE_UTF8_ZT, which if used with sqlite3_result_text64() or sqlite3_bind_text64() declares that the string provided is UTF8 and zero-terminated at the length specified. (check-in: 2d84ce88fe ... user: drh tags: utf8-zt) | 
| 11:29 |  | Add a new assert(). (check-in: bc450aba67 ... user: drh tags: union-by-merge) | 
| 2026-01-20 |  |  | 
| 20:10 |  | Strive to use sort-and-merge for compounds in CTEs. (check-in: 73ed1c790b ... user: drh tags: union-by-merge) | 
| 18:30 |  | Update the shell tool to work with SQLITE_OMIT_AUTOINIT builds. (check-in: 2b3b36da9d ... user: dan tags: trunk) | 
| 18:20 |  | Fix another test case to work with union-by-merge. (check-in: 8279c10e24 ... user: drh tags: union-by-merge) | 
| 18:07 |  | Tiny JS doc touchup. (check-in: 6d9ec0a21a ... user: stephan tags: trunk) | 
| 18:01 |  | wasm: make pre-js.c-pp.js a no-op in bundler-friendly builds. Edit: done at the request of the npm team. (check-in: 982a91abc0 ... user: stephan tags: trunk) | 
| 17:10 |  | wasm: a potential workaround for cases such as forum post 1eec339854c935bd, where client-local needs require overriding Emscripten config defaults (which we very intentionally do not document or support for reasons explained in that thread). This is untested because it requires a special-needs setup, but it doesn't break anything for run-of-the-mill cases. (check-in: c8e6be9241 ... user: stephan tags: trunk) | 
| 15:07 |  | Minor fixes. Tests are passing now. (check-in: 305ae331f5 ... user: drh tags: union-by-merge) | 
| 10:57 |  | Add an SQLITE_API_ARMOR check to sqlite3_get_clientdata(), per request in forum post a011a001f4. (check-in: 346ad366a8 ... user: stephan tags: trunk) | 
| 2026-01-19 |  |  | 
| 22:20 |  | Minor doc updates in mkwasmbuilds.c. (check-in: 81840bd4b2 ... user: stephan tags: trunk) | 
| 22:01 |  | wasm: remove the bundler-friend builds from the 'all' target, as they're only needed for the 'npm' target. (check-in: dfe4b788ca ... user: stephan tags: trunk) | 
| 21:57 |  | Minor doc touchups in the opfs-sahpool VFS. No functional changes. (check-in: 67c0d11701 ... user: stephan tags: trunk) | 
| 19:51 |  | Be more aggressive about using a sort-and-merge to compute the UNION of two SELECT statements. Experimental branch. This check-in is failing some tests. (check-in: e534af8a78 ... user: drh tags: union-by-merge) | 
| 11:44 |  | Add two new JSON functions: json_array_insert() and jsonb_array_insert(). Patterned after the json_array_insert() function of MySQL. Forum thread 2026-01-17T10:40:39z. (check-in: c1e5513241 ... user: drh tags: trunk) | 
| 2026-01-17 |  |  | 
| 19:01 |  | Raise an error if any PATH argument to json_array_insert() is not the path of an array element. Add some test cases for json_array_insert(). (closed check-in: 96b7d2c60a ... user: drh tags: json_array_insert) | 
| 14:23 |  | Add new SQL function: json_array_insert(). Suggested in forum thread 2026-01-17T10:40:39z. This check-in seems to work, but contains no test cases. Also, no error is raised if the PATH does not reference an array. Prototype only. (check-in: d6c82c8248 ... user: drh tags: json_array_insert) | 
| 2026-01-16 |  |  | 
| 20:04 |  | Fix further problems with deleting from corrupt fts5 databases in secure-delete mode. (check-in: f8f89d2e10 ... user: dan tags: trunk) | 
| 17:48 |  | Try to prevent false positives in fuzzinvariants.c due to scalar subqueries. (check-in: c20be882a6 ... user: drh tags: trunk) | 
| 15:53 |  | The optimization at [8bdda827a3d26800] is incorrect for the case where one of the two expressions has been commuted such that the collating sequence has changed. This check-in fixes the problem. Problem reported by forum post 2026-01-16T11:35:28Z. (check-in: 9d8fb7a9e2 ... user: drh tags: trunk) | 
| 12:13 |  | Change the copyright on the tool/dbtotxt.c source code file to the standard SQLite blessing. (check-in: 766c89e009 ... user: drh tags: trunk) | 
| 2026-01-15 |  |  | 
| 17:17 |  | Fix an error in new test script fts5corrupt9.test. (check-in: a5918835e1 ... user: dan tags: trunk) | 
| 17:00 |  | Add extra test for handling corrupt delta records in rbu. (check-in: 7dba05100f ... user: dan tags: trunk, artiphishell) | 
| 16:09 |  | Fix a buffer overrun that could occur when deleting rows in secure-delete mode from a strategically corrupted fts5 database. (check-in: 1e66d6e327 ... user: dan tags: trunk, artiphishell) | 
| 2026-01-14 |  |  | 
| 19:22 |  | Adjust the output of the fuzzcheck test program so that testrunner.tcl can capture the number of tests run even when the --slice option is used. (check-in: dddaeff4ce ... user: drh tags: trunk) | 
| 17:52 |  | Update sessions module so that it takes the db mutex before, instead of just after, opening a savepoint when generating a changeset. (check-in: 41bbd13d73 ... user: dan tags: trunk) | 
| 17:02 |  | Improve some of the documentation comments for the new code on this branch. (check-in: f352485db4 ... user: dan tags: changegroup-change-api) | 
| 16:01 |  | Merge latest trunk changes into this branch. (check-in: fae9f376b7 ... user: dan tags: changegroup-change-api) | 
| 15:27 |  | Add tests for edge cases in the new code on this branch. (check-in: cd115f0f44 ... user: dan tags: changegroup-change-api) | 
| 13:43 |  | Guard against oversized index entries in databases larger than 2GiB. (check-in: ed17a878e5 ... user: drh tags: trunk, artiphishell) | 
| 2026-01-13 |  |  | 
| 19:59 |  | Allow the APIs on this branch to be used to create patchsets as well as changesets. (check-in: 8699fe5b24 ... user: dan tags: changegroup-change-api) | 
| 02:35 |  | Fix integer overflow problems and error reporting memory leaks in the zipfile extension. Forum post 2026-01-13T00:09:06Z. (check-in: 8e656a4835 ... user: drh tags: trunk, artiphishell) | 
| 2026-01-12 |  |  | 
| 20:16 |  | Fix an OOB read in the sessions module that could occur while processing a corrupt changeset. (check-in: 3c46295487 ... user: dan tags: trunk) | 
| 19:56 |  | Avoid unsigned integer overflow in the delta_apply() extension function. Not part of any standard deliverable. Forum thread d41879b367c7f7ec. (check-in: b354dd12c2 ... user: drh tags: trunk, artiphishell) | 
| 19:13 |  | Fix an infinite loop in the sessions module that could occur when processing a corrupt changeset blob. (check-in: d98653bdbc ... user: dan tags: trunk) | 
| 16:39 |  | Add an API to the sessions module to add changes one at a time to an sqlite3_changegroup object. (check-in: 27150a8c22 ... user: dan tags: changegroup-change-api) | 
| 15:43 |  | wasm: filter the custom Module.instantiateWasm() out of node builds, per request from the npm project. (check-in: b57a8215f4 ... user: stephan tags: trunk) | 
| 14:45 |  | Improved documentation (in the header comment) for tmstmpvfs.c. (check-in: 70b1da718c ... user: drh tags: trunk) | 
| 2026-01-11 |  |  | 
| 21:18 |  | Add sqlite3_carray_bind() to sqlite3_api_routines. Forum post 257e642a4c. (check-in: 7c40dbcec8 ... user: stephan tags: trunk) | 
| 16:21 |  | Add the @preserve tag to the standalone file sqlite3-opfs-async-proxy.js. (check-in: 334b2be57e ... user: stephan tags: trunk) | 
| 14:14 |  | wasm: add @preserve tags to two comment blocks (license header and build version info) so that the npm tools know to retain those comments when minifying. (check-in: 596f1ea084 ... user: stephan tags: trunk) | 
| 2026-01-10 |  |  | 
| 14:12 |  | Update speedtest.md to point out xdg-open on Linux. (check-in: 47c7fbcb46 ... user: stephan tags: trunk) | 
| 00:38 |  | Fix a help text typo in speedtest1.tcl, pointed out off-list by BrickViking. (check-in: 1fd02a75a9 ... user: stephan tags: trunk) | 
| 2026-01-09 |  |  | 
| 19:44 |  | Add the 'npm' ext/wasm makefile target to create npm-bundle.zip for use by the downstream npm package. This target is intended to become their path for pulling npm-relevant deliverables, rather than the canonical downloads, and they have our/my commitment not to break it without involving them first. (check-in: a4752c18a8 ... user: stephan tags: trunk) | 
| 18:42 |  | Fix the wasm snapshot build's zipfile name to include '-snapshot' and correct the update-docs target for recent build refactoring. (check-in: 0f53dbf060 ... user: stephan tags: trunk) | 
| 00:41 |  | Fix potential OOB read on the undocumented test function rtreenode() that is part of the RTREE extension, as described in forum post 2026-01-08T23:32:19Z. The problem is almost certainly harmless since any memory allocation will be a multiple of 8 bytes, and once the input buffer size gets rounded up to the next multiple of 8 bytes, the access will still be within bounds. Nevertheless, it still needs to be fixed. (check-in: 9adab8b2be ... user: drh tags: trunk, artiphishell) | 
| 2026-01-08 |  |  | 
| 20:29 |  | Enhance VACUUM INTO so that if a URI filename is used as the target and that filename as a reserve=N query parameter with N between 0 and 255, then the reserve amount for the generated database copy is set to N. This simplifies making a copy of a database file with a reduced or reset reserve. (check-in: a482f98365 ... user: drh tags: trunk) | 
| 20:20 |  | In the reserve=N query parameter on the VACUUM INTO target file, the parameter is a silent no-op if N is out of range. (closed check-in: 48e59a2733 ... user: drh tags: reset-reserve) | 
| 18:55 |  | Change the name of the log file for tmstmpvfs.c to be an ISO8601 date/time, for improved readability and situational awareness. (check-in: 66018b85a9 ... user: drh tags: trunk) | 
| 18:15 |  | Fix the sqlite3-worker1.mjs (ESM, non-bundler) build and have demo-worker1-promiser use that file rather than the vanilla one, as a test case. Delete the global symbol installed by that file in ESM and bundler builds, as it's unnecessary there. The first change should resolve downstream sqlite-wasm/issues/123, with thanks to Jure Rotar for his help. (check-in: 11f5ab0dd8 ... user: stephan tags: trunk) | 
| 18:14 |  | Merge all the latest trunk changes into the reset-reserve branch. (check-in: 6e5f1fbaec ... user: drh tags: reset-reserve) | 
| 17:52 |  | Fix an off-by-one error in the page number of ELOG_CKPT_PAGE log messages in tmstmpvfs.c (check-in: 6fafd010de ... user: drh tags: trunk) | 
| 15:26 |  | Add a missing #if check for SQLITE_OMIT_LOAD_EXTENSION to shell.c.in, trigged by the fiddle build. (check-in: 1091f3371f ... user: stephan tags: trunk) | 
| 15:25 |  | Minor bug fixes in tmstmpvfs.c, showdb.c, and showtmlog.c. (check-in: 1fee8265f7 ... user: drh tags: trunk) | 
| 15:06 |  | Remove an obsolete '32-bit' label from tester1-worker. (check-in: e14d9a9803 ... user: stephan tags: trunk) | 
| 15:03 |  | Fix the --size X flag for speedtest1.html when X=0 and force the job size to 5 for kvvfs so that it doesn't block the main thread for too awful long. (check-in: e2634e500c ... user: stephan tags: trunk) | 
| 13:43 |  | Enhance the --vfs command-line option to the CLI such that if the argument is not a valid VFS name, but is the name of a file, attempt to load that file as an extension, which presumably adds the desired VFS. (check-in: 509954b44e ... user: drh tags: trunk) | 
| 13:20 |  | In the showdb utility, the -tmstmp option works without -csv for the pgidx command. (check-in: c0aecd2189 ... user: drh tags: trunk) | 
| 12:43 |  | Fix ESCALE typo in shell.c, reported in forum post b8f6a41c8b. (check-in: 6fa0944973 ... user: stephan tags: trunk) | 
| 12:39 |  | Do not show LIKE wildcards on table names in the output of the ".schema" command in the CLI. Forum post 6ebb57b41691293d. (check-in: 4c6f646bd2 ... user: drh tags: trunk) | 
| 12:22 |  | Add the showtmlog utility for decoding the tmstmpvfs log files. Fix tmstmpvfs so that it records timestamps in milliseconds since the Unix epoch, not in JDN milliseconds. (check-in: 762d3cbf06 ... user: drh tags: trunk) | 
| 01:37 |  | Improve showdb so that it does a better job of handling reserve-bytes. Add the --tmstmp option that causes pgidx to interpret tmstmpvfs tags if they are available. (check-in: d7e6e9a578 ... user: drh tags: trunk) | 
| 00:41 |  | Add the tmstmpvfs.c extension to trunk. It is not a part of any deliverable and can easily be removed later, if found to not be desirable. (check-in: 869f306592 ... user: drh tags: trunk) | 
| 2026-01-07 |  |  | 
| 23:46 |  | Add the --csv option to the "pgidx" option to the "showdb" utility. (check-in: b6ad93f62e ... user: drh tags: trunk) | 
| 21:19 |  | Enhance the VACUUM INTO command such that if the target file is a URI with a reserve=N query parameter, then the reserve_bytes value is set to N provided that N is within the allowed range. This can even reduce the size of the reserve, all the way to zero. This provides a way to remove reserve from a database without having to do a complete dump/restore. (check-in: 9cec3b67d9 ... user: drh tags: reset-reserve) | 
| 19:06 |  | Fix the static build. (closed check-in: 4e1a1b6aef ... user: drh tags: timestamp-vfs) | 
| 18:45 |  | Improvements to logging. Better log-file names. Log files are in a subdirectory named after the database. Only connections that write to the database create logs. Better documentation in the header comment. (check-in: 5b1cf0fe73 ... user: drh tags: timestamp-vfs) | 
| 16:19 |  | Enhance QRF so that it terminates more quickly if it encounters an OOM or oversize string error, and fix it so that it reports such errors. (check-in: ebedeb6169 ... user: drh tags: trunk) | 
| 2026-01-06 |  |  | 
| 21:39 |  | Add the -p or --port option to sqlite3_rsync. (check-in: 53f806fdab ... user: drh tags: trunk) | 
| 21:18 |  | Documentation tweaks. (check-in: b10b0aa765 ... user: drh tags: timestamp-vfs) | 
| 20:51 |  | Enhance the timestamp-vfs prototype so that it also keeps an event log. (check-in: f087c6c0cd ... user: drh tags: timestamp-vfs) | 
| 13:00 |  | Update to the sqlite3_get_clientdata() documentation. No code changes. (check-in: 395b7168b8 ... user: drh tags: trunk) | 
| 2026-01-05 |  |  | 
| 18:29 |  | Update the regexp extension so that the "-" character may be escaped with a backslash. Forum post c5e7358f82. (check-in: 8001312936 ... user: dan tags: trunk) | 
| 17:28 |  | Add the experimental tmstmpvfs.c extension that adds a timestamp and other debugging information on each page of a database as that page is written, if the database is configured for exactly 16 bytes of reserve space. (check-in: 703e593dfc ... user: drh tags: timestamp-vfs) | 
| 2026-01-04 |  |  | 
| 11:05 |  | Fix an uninitialized var use reported in forum post 88561a4a1e. (check-in: 707c0f6442 ... user: stephan tags: trunk) | 
| 2026-01-03 |  |  | 
| 15:11 |  | Fix yet another bug in the EXISTS-to-JOIN optimization, reported by forum post 2026-01-03T14:05:48z. (check-in: f60e863e0c ... user: drh tags: trunk) | 
| 2026-01-02 |  |  | 
| 19:42 |  | Minor change to an #if in the Windows VFS, which I am told will allow SQLite to be compiled for 64-bit Windows7. (check-in: 2afc54b91f ... user: drh tags: trunk) | 
| 17:10 |  | Ensure that loops created by the exists-to-join optimization use unique cursor numbers, even if the EXISTS expression in the WHERE clause is a copy of an expression from the result-set of the SELECT. dbsqlfuzz 4f6a5213cf74fcf2a8a3e48abc2333100348c38e. (check-in: 19e57a18cb ... user: dan tags: trunk) | 
| 00:23 |  | Change the shelltest target on the windows makefile to use testrunner.tcl (check-in: db4d076235 ... user: drh tags: trunk) | 
| 2026-01-01 |  |  | 
| 21:35 |  | Further improvements to testrunner.tcl documentation. (check-in: c71402031c ... user: drh tags: trunk) | 
| 15:41 |  | Updates to the testrunner documentation. (check-in: bcac99f962 ... user: drh tags: trunk) | 
| 2025-12-31 |  |  | 
| 20:43 |  | Verify the effect of "--titles on" in the "insert" mode in the CLI. (check-in: a5f922b083 ... user: drh tags: trunk) | 
| 20:35 |  | The header setting from the legacy ".headers" dot-command takes precedence over the current ".mode" setting, for backwards compatibility. Forum post 2025-12-31T19:14:24z. (check-in: d2c5df4128 ... user: drh tags: trunk) | 
| 19:45 |  | In the (completely unused, as far as I know) btreeinfo.c extension (not a part of any standard build) do a better job of detecting corrupt database files and provide better error messages when corruption is detected. Forum post e66a458339. (check-in: 326d29ac7e ... user: drh tags: trunk) | 
| 18:30 |  | Use 64-bit integers to track memory allocation sizes in the CSV extension. Forum post e66a458339. (check-in: f293a2b3ff ... user: drh tags: trunk) | 
| 17:21 |  | Add the new "retest" command to testrunner.tcl, which reruns any tests that failed or never completed in the previous invocation. New makefile target "retest" for both unix and Windows that runs testrunner.tcl with "retest". (check-in: 9b668d56bf ... user: drh tags: trunk) | 
| 14:17 |  | Enhance testrunner.tcl so that it can always be run from an ordinary tclsh as long as that tclsh can load the sqlite3 package. (check-in: 1a08b85a86 ... user: drh tags: trunk) | 
| 11:46 |  | Fix test/shellB.test so that it works under Device-One. Also bring the header comment of shellB up to date. (check-in: 836d02468c ... user: drh tags: trunk) | 
| 01:19 |  | Use sqlite3_malloc64() in all places in the compress.c extension to avoid integer overflow. Forum post 2025-12-31T01:04:00z. (check-in: 7ceaf1f0c3 ... user: drh tags: trunk) | 
| 01:01 |  | Fix a typo in [0819fe670f9ceec7] that lead to incomplete protection against buffer overflow in the zipfile extension. Forum post 2025-12-30T23:57:19z (check-in: a6abbadacb ... user: drh tags: trunk) | 
| 2025-12-30 |  |  | 
| 13:10 |  | Fix the output from the (undocumented) --test-argv option of the CLi so that it correctly displays UTF8 text on Windows. (check-in: 39e15ce3d6 ... user: drh tags: trunk) | 
| 13:01 |  | Add missing free() to winmain.c (check-in: 44dff1a1bb ... user: drh tags: trunk) | 
| 12:38 |  | Improved handling of the process entry point in Windows, to translate arguments into UTF8. (check-in: 1d4ee6c35e ... user: drh tags: trunk) | 
| 11:36 |  | Avoid signed integer overflow when an fts5 'merge' command is passed -2147483648 as a parameter. (check-in: 52738908b0 ... user: dan tags: trunk) | 
| 11:33 |  | Fix the change on this branch so that it works with amalgamation builds. (closed check-in: 7f0b9e7a8b ... user: dan tags: fts5-int-overflow-fix) | 
| 11:19 |  | Avoid signed integer overflow when an fts5 'merge' command is passed -2147483648 as a parameter. (check-in: 4c5d18d7f5 ... user: dan tags: fts5-int-overflow-fix) | 
| 2025-12-29 |  |  | 
| 23:56 |  | Fix the new .import logic in the CLI so that it honors ".bail on". Forum post 6c1c0e213d. (check-in: d58846e74b ... user: drh tags: trunk) | 
| 21:39 |  | Fix error messages associated with .import, along with corresponding test cases. (check-in: 2084e805d0 ... user: drh tags: trunk) | 
| 19:04 |  | Incremental improvements to .import, and new test cases. (check-in: 5a1e931b19 ... user: drh tags: trunk) | 
| 12:46 |  | Update to the unix build documentation. (check-in: 004cf4ad3c ... user: drh tags: trunk) | 
| 12:34 |  | Fix the "testfixture" makefile target so that it builds with a static TCL library on Mac-ARM. Fix the "tool-zip" target so that it uses ordinary tclsh rather than testfixture to run the tool/mmktoolzip.tcl script that builds the archive. (check-in: adf5d624cf ... user: drh tags: trunk) | 
| 2025-12-28 |  |  | 
| 21:37 |  | Test cases for .import, as well as minor enhancements to .import and to .check. (check-in: 0252ac1c94 ... user: drh tags: trunk) | 
| 14:32 |  | If the filename argument to the .import command is of the form "<<endmark" then the content is read from the script (or stdin) until the first line that begins with "endmark". (check-in: 6e8ab51fbe ... user: drh tags: trunk) | 
| 14:01 |  | Improvements to the .import command of the CLI. (check-in: 436ed7937b ... user: drh tags: trunk) | 
| 2025-12-27 |  |  | 
| 19:57 |  | Change a single integer variable to 64-bit in the tokenizer to avoid any possibility of an integer overflow. (check-in: 69cf692a24 ... user: drh tags: trunk) | 
| 2025-12-26 |  |  | 
| 20:13 |  | Work around the Windows "Worst-Fit" bug in the "sqldiff.exe" program. (check-in: f418515873 ... user: drh tags: trunk) | 
| 17:23 |  | To ensure that the sqlite3_hard_heap_limit() call it makes is effective, add "sqlite3_config(SQLITE_CONFIG_MEMSTATUS, 1);" to fuzzcheck.c. (check-in: f7736af849 ... user: dan tags: trunk) | 
| 2025-12-24 |  |  | 
| 18:12 |  | Doc typo fixes in the configure script pieces. (check-in: 8ab37d9fb9 ... user: stephan tags: trunk) | 
| 14:03 |  | Enhance the omit-noop-join optimization so that it is able to omit a chain of noop-joins. Optimization improvement suggested by forum post 11a53f2bad. (check-in: 5a85b83b9f ... user: drh tags: trunk) | 
| 2025-12-23 |  |  | 
| 17:24 |  | Additional changes to support the ".schema" command on builds that omit virtual tables. (check-in: 6b5c0374fa ... user: drh tags: trunk) | 
| 16:51 |  | Fix the ".schema" command in the CLI so that it works on builds that omit virtual table support. (check-in: b4e3c8170a ... user: drh tags: trunk) | 
| 11:13 |  | Harmless typo fixes in shell.c (check-in: 519877cdbd ... user: drh tags: trunk) | 
| 2025-12-22 |  |  | 
| 17:06 |  | API documentation enhancements. (check-in: 99ac94cb9a ... user: drh tags: trunk) | 
| 15:22 |  | Add the SQLITE_PREPARE_FROM_DDL flag to sqlite3_prepare_v3(). Use this to prevent fts3/4 tables in non-trusted schemas from calling unsafe SQL functions. (check-in: 26f39ac806 ... user: dan tags: trunk) | 
| 12:33 |  | Fix spurious error log reports caused by [d25c8a6222d4e3f2]. See forum post d2326687662. (check-in: 403dafc779 ... user: drh tags: trunk) | 
| 12:00 |  | Fix a harmless signed integer overflow warning in the ieee754() extension function. (check-in: f43d3339c1 ... user: drh tags: trunk) | 
| 11:28 |  | Remove unused thunk cases from os_win.c, so that it will (hopefully) compile again on WinXP. (check-in: eb2c020481 ... user: drh tags: trunk) | 
| 2025-12-19 |  |  | 
| 13:17 |  | Add experimental compile-time option SQLITE_ALLOW_TRIGGERS_ON_SYSTEM_TABLES. (check-in: f916bcc865 ... user: drh tags: trunk) | 
| 2025-12-18 |  |  | 
| 20:44 |  | Bug fix in the vt02.c test virtual table. (check-in: a7a73d30c5 ... user: drh tags: trunk) | 
| 18:47 |  | Allow queries that use "GROUP BY e1 ORDER BY e2" where e1 and e2 are identical aprt from ASC/DESC sort-orders to be optimized using a single index. Also allow virtual tables to optimize DISTINCT in cases where the result-set of a query does not exactly match the ORDER BY clause. (check-in: fba29a8b56 ... user: dan tags: trunk) | 
| 16:14 |  | New options for vt02.c that allow tests to configure it to partially de-duplicate a DISTINCT query. (check-in: 1c65bb6e5d ... user: drh tags: trunk) | 
| 16:05 |  | Improve tests in bestindexF.test. No changes to non-test code. (check-in: 347d4d34c1 ... user: dan tags: trunk) | 
| 15:35 |  | Better orderByConsumed logic in the vt02.c test virtual table. (check-in: 612c1ece67 ... user: drh tags: trunk) | 
| 14:01 |  | Enhance the sqlite3_load_extension() so that when the entry point is not specified, after trying sqlite3_extension_init() and sqlite3_X_init() where X is the alphabetic part of the base filename, if neither or found it tries again with X as the alpha*numeric* part of the base filename. Hence, a shared library named "vt02.so" can be loaded with just ".load vt02" and without having to specify the entry point separately. (check-in: acc95f24cc ... user: drh tags: trunk) | 
| 12:49 |  | Enhance the vt02.c test virtual table with the ability to process ORDER BY DESC. (check-in: 35461a8d5a ... user: drh tags: trunk) | 
| 2025-12-17 |  |  | 
| 21:00 |  | Fix the return value of sqlite3_vtab_distinct() in cases where a virtual table query has similar GROUP BY and ORDER BY expression lists. (check-in: d017ae8640 ... user: dan tags: trunk) | 
| 13:10 |  | Fix a FILE* leak in the CLI (check-in: 7986a35d27 ... user: drh tags: trunk) | 
| 2025-12-16 |  |  | 
| 13:43 |  | CLI: The .imposter command matches the index name regardless of case. Minor fixes to .check. (check-in: 989ebba9ad ... user: drh tags: trunk) | 
| 13:25 |  | CLI improvements. In the .check command, ignore leading and trailing whitespace unless the --exact option is used. In the .limit command, do not print the new limit when changing the limit, but only when requesting the current value of the limit. (check-in: d72cb66d67 ... user: drh tags: trunk) | 
| 13:13 |  | Stricter enforcement of the LIKE_PATTERN_LENGTH for the regexp extension. (check-in: c94b1e9c56 ... user: drh tags: trunk) | 
| 2025-12-15 |  |  | 
| 18:45 |  | Fix for optimizing "SELECT DISTINCT ...cols... FROM vtab" in cases where the virtual table implementation is able to deliver results sorted by "...cols...". (check-in: 80c0d92293 ... user: dan tags: trunk) | 
| 17:32 |  | Enhance the vtablog extension so that it shows the value of sqlite3_vtab_distinct() in xBestIndex, and so that provides the new consume_order_by option that can cause xBestIndex to set the orderByConsumed flag. (check-in: b158fe9299 ... user: drh tags: trunk) | 
| 13:58 |  | Clarify the affect of nCharLimit, nLineLimit, and nTitleLimit on the xRender output from QRF. (check-in: 881534858d ... user: drh tags: trunk) | 
| 13:26 |  | Fix the test/vt100-a.sql script to use .testcase and .check. (check-in: 5cf9e99ff8 ... user: drh tags: trunk) | 
| 11:59 |  | Enable an ORDER BY clause to refer to columns in outer queries. (check-in: ef9b4b1e46 ... user: drh tags: trunk) | 
| 2025-12-13 |  |  | 
| 23:18 |  | Use the correct wildcard character in sqlite3_strlike() in the previous check-in. (check-in: 87bca1250c ... user: drh tags: trunk) | 
| 21:11 |  | In the CLI, if a command-line argument names a file and the filename ends with .sql or .txt, then process that file as an SQL script. Use this feature for better testing. (check-in: 19cc0522e2 ... user: drh tags: trunk) | 
| 00:13 |  | Back out the --compat option from the CLI. (check-in: 850d5dbfb0 ... user: drh tags: trunk) | 
| 2025-12-12 |  |  | 
| 18:22 |  | Get the .testcase and .check commands working in the CLI. (check-in: 1ca31e1a29 ... user: drh tags: trunk) | 
| 12:06 |  | Add new assert() statements designed to detect and prevent the kind of system interface numbering errors that happened in [108691a3cb2f296f] and that were not fixed until [fe49703034bd23fa]. (check-in: 4e6e551c89 ... user: drh tags: trunk) | 
| 11:50 |  | Omit unused entries in the aSysCall array for the Windows driver. Fix incorrect numbering in alter entries of that array. (check-in: fe49703034 ... user: drh tags: trunk) | 
| 11:02 |  | Remove from os_win.c a reference to unused system call CreateEventExW() left over from the now removed winrt support. (check-in: c68ef69ec9 ... user: dan tags: trunk) | 
| 10:21 |  | Fix an inaccuracy in the qrfRelaxable() routine in QRF when the input is an empty string. (check-in: eb8a519571 ... user: drh tags: trunk) | 
| 00:21 |  | Include the latest Makefile.msc changes and the new "make.bat" script in the amalgamation tarballs. (check-in: 6deab75874 ... user: drh tags: trunk) | 
| 2025-12-11 |  |  | 
| 23:24 |  | Prevent a malicious delta from causing an integer overflow in the fossildelta extension. This code is not used in the SQLite core. (check-in: 01409738af ... user: drh tags: trunk, artiphishell) | 
| 18:16 |  | Update build instructions for Windows to explain how to link against ZLIB. (check-in: e785a80e41 ... user: drh tags: trunk) | 
| 17:49 |  | Adjust the MSVC Makefile so that when it invokes the zlib makefile, it changes CFLAGS to use the correct -M compiler option to match SQLite. (check-in: ba003c7c74 ... user: drh tags: trunk) | 
| 11:05 |  | Fix calls to sqlite3_wal_checkpoint_v2(SQLITE_CHECKPOINT_NOOP) so that they do not return SQLITE_MISUSE. (check-in: 117f4093f0 ... user: dan tags: trunk) | 
| 2025-12-10 |  |  | 
| 19:43 |  | Use macros for the default values for QRF limits in the CLI, rather than hard-coded magic numbers. (check-in: 4ef66da15b ... user: drh tags: trunk) | 
| 19:04 |  | Various improvements to the .mode command of the CLI, and especially to how it handles the --limits option. (check-in: f883b85497 ... user: drh tags: trunk) | 
| 16:20 |  | In fiddle, remove the default .dot-commands it runs, as they do not reflect current shell defaults. (check-in: 27b6aeda5d ... user: stephan tags: trunk) | 
| 16:10 |  | Add the --titlelimit option to CLI, defaulting to 20. In "line" mode of the CLI, the default seperator is changed from " = " to ": ". This can be overridden using the --colsep option. (check-in: 291677ebb3 ... user: drh tags: trunk) | 
| 15:55 |  | Do not show the --titlelimit setting in ".mode" setting output unless -v is used or unless the current style is tabular or line. (closed check-in: a9b0a3ce04 ... user: drh tags: title-limit) | 
| 15:50 |  | Test cases and documentation tweaks. (check-in: cc9e47a205 ... user: drh tags: title-limit) | 
| 15:24 |  | Add the nTitleLimit option to QRF. Also change "line" style so that it uses zColumnSep as the divider between the title and the value. (check-in: d67a299c86 ... user: drh tags: title-limit) | 
| 2025-12-09 |  |  | 
| 13:41 |  | In fts5, avoid holding an open blob handle after an INSERT/UPDATE/DELETE statement, as this can interfere with sqlite3_interrupt(). Forum post beba85aa1b. (check-in: f88e1d0357 ... user: dan tags: trunk) | 
| 2025-12-08 |  |  | 
| 13:11 |  | Update the added-in-version stamp to some kvvfs JS APIs. (check-in: 939275c6c5 ... user: stephan tags: trunk) | 
| 13:06 |  | Introducing kvvfs v2 for the JS bindings. Summary: no longer hard-coded to session/localStorage, available outside of the main UI thread (non-persistently), a simpler-to-use import/export API, and adds an asynchronous event interface intended for creating page-by-page db backups. (check-in: ec866b04d0 ... user: stephan tags: trunk) | 
| 12:24 |  | Replace some double-quotes with single quotes an SQL doc snippet in the csv example virtual table. (closed check-in: 1781e5e8d0 ... user: stephan tags: kvvfs-v2) | 
| 2025-12-07 |  |  | 
| 18:19 |  | Minor tweaks to the QRF documentation. No changes to code. (check-in: b2517d01e6 ... user: drh tags: trunk) | 
| 2025-12-06 |  |  | 
| 23:58 |  | Do not return too many bytes from the zipfile extension given a malformed ZIP file. CVE-2025-70873. (check-in: 9766b47beb ... user: drh tags: trunk) | 
| 20:41 |  | In the zipfile extension, only return as many bytes as Inflate actually generated. Forum post 761eac3c82. Adjust ./configure so that it builds zipfile into testfixture if ZLIB is available, so that tests get run on unix platforms. (closed check-in: 3d459f1fb1 ... user: drh tags: zipfile-fix) | 
| 19:56 |  | Remove #ifdefs intended to make SQLite compatible with Windows RT. WinRT was short-lived and has been discontinued for a long time. (check-in: 108691a3cb ... user: drh tags: trunk) | 
| 18:40 |  | Fix a harmless compiler warning inside an assert() in FTS3. (check-in: 005e176b4e ... user: drh tags: trunk) | 
| 11:54 |  | Fox the "box" format, use rounded corners and use a double-line to separate the header from the content. Add the "-quote relaxed" text rendering mode and make it the default for TTYs. (check-in: b3da6a8728 ... user: drh tags: trunk) | 
| 11:50 |  | Fix typos and harmless compiler warnings. Omit the use of ctypes.h in QRF. (closed check-in: 4f7edadbde ... user: drh tags: qrf-box-enhancements) | 
| 11:35 |  | New test cases to cover the "--quote relaxed" feature. (check-in: d00d9556ed ... user: drh tags: qrf-box-enhancements) | 
| 11:02 |  | Update test cases to work with the new box format and with --quote relaxed. (check-in: e3f442d08f ... user: drh tags: qrf-box-enhancements) | 
| 10:07 |  | New text rendering style of QRF_TEXT_Relaxed. (check-in: d2049bffe6 ... user: drh tags: qrf-box-enhancements) | 
| 2025-12-05 |  |  | 
| 22:04 |  | When drawing boxes using QRF, use rounded corners and use a double-line to separate column headers from the content. (check-in: 8c1ca30707 ... user: drh tags: qrf-box-enhancements) | 
| 19:45 |  | In the Windows Makefile.msc, never set -DNDEBUG. And add the "env" target which prints out the value of all 127 configuration variables that Makefile.msc uses. (check-in: f82376f080 ... user: drh tags: trunk) | 
| 19:23 |  | Fix a memory leak in the fileio extension on windows. (check-in: 19e5ae6e20 ... user: dan tags: trunk) | 
| 16:06 |  | Omit obsolete features (Ex: Windows Phone support) from the Windows Makefile.msc. More of this kind of thing is pending... (check-in: c320c55858 ... user: drh tags: trunk) | 
| 15:09 |  | Minor adjustment to the zipfile.test script so that it works on Windows. (check-in: 7bb633df97 ... user: drh tags: trunk) | 
| 12:22 |  | Enhance the unixIsSharingShmNode() code (check-in [6385a1962c69c69c]) so that it more understandable by humans, and so that it does not trigger false positive deadlock warnings in TSAN. (check-in: aa0a28776b ... user: drh tags: trunk) | 
| 01:33 |  | Another fix to [e33da6d5dc964db8]: Ensure that the EXISTS-to-JOIN break happens even if the inner loop is a "no-op" loop that never executes more than once because it is controlled by a UNIQUE index. This resolves the error reported in forum post 7992838ba2. Test cases are in TH3. (check-in: bbded64770 ... user: drh tags: trunk) | 
| 00:56 |  | Update the autoconf Makefile.msc based on the Makefile.msc changes in the previous check-in. (check-in: 8dce366989 ... user: drh tags: trunk) | 
| 2025-12-04 |  |  | 
| 21:16 |  | Automatically add the /NODEFAULTLIB:msvcrt flag to the Makefile.msc so that Windows builds are able to link against zlib.lib by default. (check-in: 0b75057d61 ... user: drh tags: trunk) | 
| 20:22 |  | Make the shell_exec() routine in the CLI robust against NULL SQL text. Fix harmless compiler warnings in ext/misc/zipfile.c. (check-in: 2ecba7dfd1 ... user: drh tags: trunk) | 
| 16:44 |  | Limit compile time value SQLITE_MAX_SQL_LENGTH to 1024 less than 2^31, or 2147482624. This is to avoid having to deal with edge cases related to 32-bit integer overflow that can only occur in custom builds. (check-in: d73364555d ... user: dan tags: trunk) | 
| 13:42 |  | Enable TEMP triggers to reference tables in the main schema. (check-in: 307a500aa6 ... user: drh tags: trunk) | 
| 13:28 |  | Mark an always-true conditional as such. (closed check-in: 4f79434f5b ... user: drh tags: temp-trigger-refs) | 
| 13:08 |  | Fix an error introduced by the previous check-in. (check-in: 948e24ae38 ... user: drh tags: temp-trigger-refs) | 
| 11:59 |  | Performance optimization in the language grammar. (check-in: 1c1ad5e39c ... user: drh tags: temp-trigger-refs) | 
| 11:20 |  | Merge all the latest trunk enhancements and fixes into the temp-trigger-refs branch. (check-in: 8e8a27c77a ... user: drh tags: temp-trigger-refs) | 
| 00:05 |  | Fix an incorrect answer that might arise if a scalar query is both DISTINCT and contains an OFFSET clause. Forum post 2025-12-03T09:25:59Z. (check-in: aef5397569 ... user: drh tags: trunk) | 
| 2025-12-03 |  |  | 
| 21:31 |  | More aggressive optimization of IS NULL and IS NOT NULL operators in the parser. New sqlite3ExprInt32() internal interface for creating integer nodes in an expression tree. (check-in: 560082d61f ... user: drh tags: trunk) | 
| 18:18 |  | More aggressive optimization of IS NULL and IS NOT NULL operators. (closed check-in: c69eec9909 ... user: drh tags: expr-opt) | 
| 16:54 |  | Create a new internal subroutine specifically designed to allocate Expr nodes that hold a 32-bit integer. (check-in: 38d06f69e5 ... user: drh tags: expr-opt) | 
| 14:39 |  | Fix a problem that could occur when a scalar subquery had both a DISTINCT and OFFSET clause. First reported by forum post 41bd7af485. (closed check-in: 496bbff910 ... user: dan tags: distinct-offset-subquery-fix) | 
| 12:07 |  | Clarification of comment text in expr.c. No changes to code. (check-in: afe5ee64f1 ... user: drh tags: trunk) | 
| 02:48 |  | Fix recently added test cases for Windows. (check-in: bccc0d65b0 ... user: drh tags: trunk) | 
| 2025-12-02 |  |  | 
| 19:55 |  | Improvements to the ".schema indent" logic. Forum post 2025-12-02T19:04:04Z. (check-in: 71ea7f38ff ... user: drh tags: trunk) | 
| 18:37 |  | New test cases for QRF (check-in: e49c63e318 ... user: drh tags: trunk) | 
| 18:28 |  | kvvfs: make the filename suffixes -wal and -shm illegal and teach xAccess() to silently not fail for illegal names (else it breaks things when a -wal file is checked for). (check-in: 2ea29c77a8 ... user: stephan tags: kvvfs-v2) | 
| 18:10 |  | Get kvvfs::xAccess() fleshed out. (check-in: d947e9bed5 ... user: stephan tags: kvvfs-v2) | 
| 16:29 |  | Consolidate kvvfs's page size workaround into a central flag and add some logging to facilitate testing and debugging. (check-in: 8d683130d2 ... user: stephan tags: kvvfs-v2) | 
| 15:47 |  | Add more logging to kvvfs to try to trace down why it cannot recover from a page size change. (check-in: 0bf0b8a98a ... user: stephan tags: kvvfs-v2) | 
| 15:01 |  | Fix a harmless compiler warning in the previous check-in. (check-in: 601c40a1d9 ... user: drh tags: trunk) | 
| 14:43 |  | Fix a problem with ALTER TABLE DROP COLUMN commands where the column being dropped is the rightmost in table and is immediately preceded by a comment containing a "," character. Forum post a0edd9acd8. (check-in: c872bd1ee7 ... user: dan tags: trunk) | 
| 12:23 |  | Add developer note on how to measure coverage of QRF. (check-in: 0fdd453c03 ... user: drh tags: trunk) | 
| 2025-12-01 |  |  | 
| 22:20 |  | Begrudingly allow sqlite3_js_kvvfs_clear() to work for opened storage only for the local/session storage cases and only (one of the demo apps reminds me) for backwards compatiblity. Likewise, the JsStorageDb.clearStorage() inherited method is now deprecated because it requires an opened db (but continues to work for those two stores), whereas its 'static' method of the same name is unaffected. Rename kvvfs.size() to kvvfs.estimateSize(). (check-in: 09e9255828 ... user: stephan tags: kvvfs-v2) | 
| 21:14 |  | Harden a change to SqliteTestUtil.assert() in the previous checkin to handle a null second argument. (check-in: 9cbd5be803 ... user: stephan tags: kvvfs-v2) | 
| 21:10 |  | Minor JS docs and cleanups. Add some #if'd-out kvvfs and oo1 pieces to experiment with later. (check-in: 3db29b00db ... user: stephan tags: kvvfs-v2) | 
| 19:19 |  | Fix a potential UAF in sqlite3.oo1.DB.exec()'s teardown pieces. (Edit: false alarm - the ostensibly offending pieces are not part of the transient allocation stack.) (check-in: 7e99e93cdd ... user: stephan tags: trunk) | 
| 18:11 |  | Fix a memory leak that could follow an OOM on this branch. (check-in: 3223fee0b9 ... user: dan tags: temp-trigger-refs) | 
| 17:39 |  | Fix another crash on this branch that could follow an OOM. (check-in: 75c49267ec ... user: dan tags: temp-trigger-refs) | 
| 17:22 |  | Merge latest trunk changes into this branch. (check-in: 230fd5533e ... user: dan tags: temp-trigger-refs) | 
| 17:22 |  | Fix a crash that could occur following a parse error on this branch. (check-in: 951b453350 ... user: dan tags: temp-trigger-refs) | 
| 16:28 |  | Relax the name limits on kvvfs dbs. (check-in: 9901cf8e4a ... user: stephan tags: kvvfs-v2) | 
| 16:25 |  | Reformulate sqlite3-wasm.c's exports so that they export properly with the wask-sdk compiler. (check-in: d71849958a ... user: stephan tags: kvvfs-v2) | 
| 15:49 |  | Teach kvvfs to handle a NULL db file name by generating a random name, and to honor the SQLITE_OPEN_CREATE flag. (check-in: 1042914691 ... user: stephan tags: kvvfs-v2) | 
| 11:07 |  | Disallow the undocumented -recovery-db option on the ".recover" command of the CLI when in --safe mode. (check-in: 6520244087 ... user: drh tags: trunk) | 
| 2025-11-30 |  |  | 
| 23:09 |  | Tweaks to the wcwidth() algorithm in QRF, for better agreement with the Terminal on Ubuntu. There is no perfect solution, as different ttys do things differently, but we should try to get as close as we can. (check-in: 35f39f7cb1 ... user: drh tags: trunk) | 
| 18:59 |  | Fix filtering of the 64-bit speedtest1 wasm test pages to again load the 64-bit wasm files. (check-in: f507a8af41 ... user: stephan tags: kvvfs-v2) | 
| 18:43 |  | kvvfs docs and 64-bit fixes. (check-in: cf58e17fa2 ... user: stephan tags: kvvfs-v2) | 
| 17:47 |  | Improvement implementation of the previous check-in: runs faster. (check-in: 484d1fd2a2 ... user: drh tags: trunk) | 
| 17:26 |  | Block injection of hyper-oversized SQL inputs into the parser. (check-in: 42b6dab924 ... user: drh tags: trunk) | 
| 16:37 |  | Optimize out a kvvfs event notification call in the common case where there are no listeners. (check-in: 8405c19d32 ... user: stephan tags: kvvfs-v2) | 
| 11:12 |  | More tweaks to the README.md file. (check-in: 1d79220ac9 ... user: drh tags: trunk) | 
| 10:49 |  | Correct to the README.md file. (check-in: 3cd3256c7c ... user: drh tags: trunk) | 
| 08:32 |  | Rename kvvfs's 'localThread' storage object to '.'. Swap kvvfs.listen()'s elideJournal(=false) option with includeJournal(=false) (i.e. opt in instead of opt out). (check-in: 7f0eca9d0a ... user: stephan tags: kvvfs-v2) | 
| 07:56 |  | Doc typo fix. (check-in: a2b53a71e9 ... user: stephan tags: trunk) | 
| 07:25 |  | Add sqlite3_db_handle() to the list of functions which are guaranteed to preserve the current error state, per request in forum post 8e79e8ddd5. (check-in: 78bdc76633 ... user: stephan tags: trunk) | 
| 07:08 |  | Add sync events to kvvfs.listen() so that streaming can maybe get a better sense of when it's okay to process the stream. (check-in: a4b6dadffa ... user: stephan tags: kvvfs-v2) | 
| 06:20 |  | More work on the kvvfs events. Demonstrate re-importing a streamed-out set of db pages. (check-in: b8fee839b0 ... user: stephan tags: kvvfs-v2) | 
| 05:25 |  | Give kvvfs.unlisten() a useful result value. (check-in: bbd08b67ce ... user: stephan tags: kvvfs-v2) | 
| 05:20 |  | Extend the kvvfs.listen() config to enable posting of raw binary db pages instead of the kvvfs-encoding. This is much more expensive but was added to... Demonstrate basic async streaming of kvvfs db page-level changes via logging of kvvfs write/delete ops. (check-in: 0f2bad2855 ... user: stephan tags: kvvfs-v2) | 
| 03:02 |  | Extend kvvfs export to optionally export the raw binary db pages as a list of Uint8Array instead of kvvfs-encoded strings. This is typically much larger but the pages can then be used as-is. (check-in: a4f59496a5 ... user: stephan tags: kvvfs-v2) | 
| 2025-11-29 |  |  | 
| 23:41 |  | Add a comment explaining the new kvvfsDecode() return -1 from [cd81cb70525e]. (check-in: ed9ab366d1 ... user: stephan tags: kvvfs-v2) | 
| 23:29 |  | Fix kvvfs event notification to notify _all_ listeners and optimize better for the common case of no listeners. Squelch some debug output. (check-in: 989d097b13 ... user: stephan tags: kvvfs-v2) | 
| 23:29 |  | Add a WASM-side export for kvvfsDecode() for pending private use in the kvvfs export bits and ensure that it consistently returns -1 for malformed input. (check-in: cd81cb7052 ... user: stephan tags: kvvfs-v2) | 
| 21:19 |  | More tests for the kvvfs vtab. (check-in: 0358a99eed ... user: stephan tags: kvvfs-v2) | 
| 18:59 |  | Additional defenses against using the ".open" command of the CLI in --safe mode. (check-in: eec0b80e53 ... user: drh tags: trunk) | 
| 18:32 |  | Update the documentation to sqlite3_str_finish(). That function might not return NULL on an empty string now, if the empty string is created through the use of sqlite3_str_truncate(). (check-in: b2e980ede5 ... user: drh tags: trunk) | 
| 18:18 |  | Handle empty string values in columnar modes without NULL pointer dereferences. (check-in: 7927dbc5eb ... user: drh tags: trunk) | 
| 16:41 |  | Add a kvvfs vtab for inspecting the storage state. This is for testing and my own edification, not part of the public API. (check-in: eaff2e8b7f ... user: stephan tags: kvvfs-v2) | 
| 12:06 |  | Fix an assertion fault in sqlite3Dequote() that can occur with ALTER TABLE DROP CONSTRAINT on a corrupt schema. dbsqlfuzz 509a778e8a0c21a6448003feb773a1e55ed751e7. Test case in TH3. (check-in: 2dc73eb2d2 ... user: drh tags: trunk) | 
| 2025-11-28 |  |  | 
| 22:17 |  | Merge trunk into the kvvfs-v2 branch. (check-in: 0e1a6ca0f1 ... user: stephan tags: kvvfs-v2) | 
| 17:56 |  | Correct a doc falsehood - jquery.terminal does not require pre-building. (check-in: 4384c9a108 ... user: stephan tags: trunk) | 
| 17:50 |  | Fiddle build: if JQTERM is set to the dir name of a built jquery.terminal checkout then copy the pieces fiddle needs to enable that input mode. By default it looks in ~/src/jquery.terminal. (check-in: 9dd16f8e3b ... user: stephan tags: trunk) | 
| 16:23 |  | Minor doc updates. (check-in: 66b0f28958 ... user: stephan tags: trunk) | 
| 11:52 |  | Fix a problem in the xFilter() method of the fts5_vocab virtual table causing it to omit some result rows in some cases where a virtual cursor is used more than once by SQLite. (check-in: 6c99e81e6c ... user: dan tags: trunk) | 
| 02:34 |  | Rename c-pp::ARGV to c-pp::argv and remove its use from the license header (where it doesn't get applied). (check-in: a2e18d299c ... user: stephan tags: trunk) | 
| 2025-11-27 |  |  | 
| 12:15 |  | Ensure that the CLI always jumps to the code that frees memory allocations prior to exiting when it encounters errors on the command line - just so that fuzzers won't call that memory leak, even though it is not. (check-in: 50eaf4266f ... user: drh tags: trunk) | 
| 11:40 |  | Fix an utterly harmless memory leak that occurs when the CLI exits due to an error in the command-line arguments. (check-in: 55942e6d58 ... user: drh tags: trunk) | 
| 11:29 |  | Ensure that the split-column transformer in QRF correctly transforms the abNum[] array. (check-in: ea67d8b001 ... user: drh tags: trunk) | 
| 10:57 |  | Fix a CLI test case due to a change in error message text. (check-in: cf45d534d6 ... user: drh tags: trunk) | 
| 01:32 |  | Fix two harmless compiler warnings in FTS3. (check-in: fd9158e635 ... user: drh tags: trunk) | 
| 2025-11-26 |  |  | 
| 22:58 |  | Improvements to the QRF README.md file to help clarify how to use it. (check-in: ff589e9e0d ... user: drh tags: trunk) | 
| 21:56 |  | Do not omit the beginning and ending margins when setting bBorder to QRF_Off, except when the margins need to be squeezed to fit. Those extra margins give a more pleasing appearance, and also make the output the same as psql. (check-in: 2892aaeed9 ... user: drh tags: trunk) | 
| 20:20 |  | Minor kvvfs error handling cleanup. (check-in: 5ec20ebe05 ... user: stephan tags: kvvfs-v2) | 
| 20:18 |  | Teach kvvf's xClose() and xFileControl() to return more specific result codes for exceptions. (check-in: 6b48375972 ... user: stephan tags: kvvfs-v2) | 
| 20:12 |  | kvvfs: silently reject attempts to change the page size, as changing it corrupts this vfs for reasons not yet understood. Add more kvvfs tests. (check-in: 65272adfab ... user: stephan tags: kvvfs-v2) | 
| 17:23 |  | In QRF, if the horizontal alignment is QRF_ALIGN_Auto, then use right alignment for numeric values and left alignment for everything else. (check-in: 5fdedc69b1 ... user: drh tags: trunk) | 
| 17:19 |  | Factor out an obsolete malloc failure check and an snprintf() for keys in transient kvvfs storage. (check-in: e2e94f9094 ... user: stephan tags: kvvfs-v2) | 
| 17:17 |  | Remove a JS-specific block from os_kv.c. (check-in: 3c1d5eac27 ... user: stephan tags: kvvfs-v2) | 
| 16:40 |  | Fix tests broken by the previous commit. (check-in: 430d4d668f ... user: stephan tags: kvvfs-v2) | 
| 16:27 |  | Remove some dead code. Resolve the remaining kvvfs v1/v2 API compatibility confusion by eliding the v1 API from Workers like we've always done. Add some tests. (check-in: 450b4bcca6 ... user: stephan tags: kvvfs-v2) | 
| 16:26 |  | Wasm speedtest1: replace some 'self' with 'globalThis' and expose the sqlite3 object to the global scope for inspection via the dev console (for inspecting resulting kvvfs storage objects). (check-in: c18154bc9a ... user: stephan tags: kvvfs-v2) | 
| 16:21 |  | Add the bBorder option to the QRF spec. Reflect this in the -border option on the TCL format method, and the --border option to ".mode" in the CLI. Also add the "psql" mode to the CLI. (check-in: 02cbeb6988 ... user: drh tags: trunk) | 
| 14:22 |  | Minor optimization in kvvfs event notification. (check-in: fd85b96b12 ... user: stephan tags: kvvfs-v2) | 
| 13:58 |  | Fix a buffer overread in fts5 that might occur while processing a corrupt db. (check-in: 8b0cbc18be ... user: dan tags: trunk) | 
| 13:55 |  | Internal kvvfs docs and cleanups. Wasm speedtest1: remove the hard-coded --size=2 limit for kvvfs and use a transient storage object instead of session storage (which is limited to --size=2). For small job sizes kvvfs is roughly 3x the speed of memdb and that gap grows as the --size increases, with kvvfs _really_ bogging down on the --size=50 jsonb tests. (check-in: 4317326784 ... user: stephan tags: kvvfs-v2) | 
| 13:24 |  | Consistently use IsSpace()/IsDigit() instead of isspace()/isdigit() in shell.c.in to squelch warnings on Cygwin. From Jan Nijtmans. (check-in: aa9d7365bb ... user: stephan tags: trunk) | 
| 11:19 |  | Fix the sha1() extension function so that it returns actual TEXT instead of a BLOB containing the UTF-8 TEXT. Forum post d15eefea7c (check-in: 6f806656a3 ... user: drh tags: trunk) | 
| 00:41 |  | Tweak testrunner.tcl to allow it to use more cores. (check-in: 71b916d4de ... user: drh tags: trunk) | 
| 2025-11-25 |  |  | 
| 23:59 |  | When "testrunner.tcl status -d N" is run but the test database is not yet available, it enters a wait loop until the testing begins. (check-in: 7abc25ff3b ... user: drh tags: trunk) | 
| 22:50 |  | Fix typo in the MAKE.BAT (check-in: bf54e5fccd ... user: drh tags: trunk) | 
| 22:38 |  | Add two new makefile targets, "errors" and "status", for checking on testrunner errors and for running testrunner status -d 2, respectively. Add a simple make.bat script at the top level that runs nmake for us, and thus make the command-line to test on Windows identical to Unix. (check-in: 838b7fd5d4 ... user: drh tags: trunk) | 
| 19:29 |  | Cherry-pick JS fixes/cleanups/docs from [206275292217], [256c1dd2c367], and [dec0c6073cc1] from the kvvfs-v2 branch into trunk. (check-in: cdd3e700f5 ... user: stephan tags: trunk) | 
| 19:20 |  | Guard access to a piece of Emscripten-related scaffolding so that sqlite3-api.js does not hit it when imported into other builds. Reported in forum post 6eacbb9c447a5665. (check-in: cb9cf028fc ... user: stephan tags: trunk) | 
| 19:06 |  | Remove some debug output. (check-in: 9fc6b811d6 ... user: stephan tags: kvvfs-v2) | 
| 19:05 |  | Remove the ostensible support for empty kvvfs names, as those names apparently never reach kvvfs.xOpen (the empty name is special). That actually simplifies things with regards to backwards compatibility. (check-in: a5c61fa567 ... user: stephan tags: kvvfs-v2) | 
| 18:52 |  | Improve kvvfs file name validation. Add sqlite3.kvvfs.unlink(). (check-in: 0dfdf4681c ... user: stephan tags: kvvfs-v2) | 
| 18:20 |  | Fix subtle inaccuracies in qrfWrapLine(). Omit an unused variable from shell.c.in. (check-in: 5804ba4874 ... user: drh tags: trunk) | 
| 16:41 |  | Add the sqlite3.kvvfs namespace for the new kvvfs APIs instead of adding more sqlite3_js_kvvfs_...() methods. Reinstate that clearing kvvfs storage is illegal when db handles are opened, solely for sanity's sake (they can actually recover from that but supporting such use feels ill-advised). (check-in: 02793c5905 ... user: stephan tags: kvvfs-v2) | 
| 16:07 |  | Add an explicit rollback test for kvvfs. (check-in: 2bf31ef802 ... user: stephan tags: kvvfs-v2) | 
| 15:59 |  | A more compact formulation of [ac8af73c16ab821] which uncovered two remaining direct calls to kvrecordRead(), now replaced with calls through the thunk. Fix a typo which caused the storage object name to not be passed on to event listener. (check-in: d391f1ce69 ... user: stephan tags: kvvfs-v2) | 
| 15:35 |  | In wasm builds, #if out the impls for the 3 core kvvfs i/o routines which get overwritten from JS (and thus aren't used) to reduce dependencies on POSIX APIs. (check-in: ac8af73c16 ... user: stephan tags: kvvfs-v2) | 
| 15:11 |  | Fix a typo in fts3/4 debugging code allowing 'maxpendinXY' to act as a synonym for 'maxpending=' in special INSERT statements. (check-in: 5af554337a ... user: dan tags: trunk) | 
| 13:58 |  | Work around an incorrect compiler warning on Mac. (check-in: d4c1d3e30b ... user: drh tags: trunk) | 
| 12:16 |  | Fixes to .mode in the CLI. "qbox" sets the correct null value. The "--quote on" option sets the null value to NULL. (check-in: 5454ec46e1 ... user: drh tags: trunk) | 
| 10:53 |  | Omit the unused bTextNull flag from the QRF spec object. (check-in: 4bbf176a87 ... user: drh tags: trunk) | 
| 05:44 |  | kvvfs internal cleanups. Experimentally add async event listeners to kvvfs to explore backing up a kvvfs a page at a time. (check-in: f355fd4849 ... user: stephan tags: kvvfs-v2) | 
| 04:07 |  | Factor out some now-superfluous JS-side kvvfs code. Factor out a superfluous allocation. Shorten the public API names of the new methods. (check-in: be435b668f ... user: stephan tags: kvvfs-v2) | 
| 03:09 |  | Disallow a filename extension of '-journal' for db files in kvvfs, as that name has a special meaning there. (check-in: 357cc42633 ... user: stephan tags: kvvfs-v2) | 
| 02:08 |  | Add xGetLastError() to kvvfs but it does not appear to be called for xOpen() failures. (check-in: 7e7944f007 ... user: stephan tags: kvvfs-v2) | 
| 02:08 |  | Fix a 32-/64-bit incompatibility in opfs-sahpool. (check-in: 256c1dd2c3 ... user: stephan tags: kvvfs-v2) | 
| 00:21 |  | Fix typos in the help text for ".mode" of the CLI. (check-in: fddb99c516 ... user: drh tags: trunk) | 
| 2025-11-24 |  |  | 
| 20:43 |  | Do not allow the kvvfs transient flag to flag a previously-non-transient storage as transient. i.e. only applies when first opening a db. Rename the transient flag to delete-on-close to avoid confusion with other meanings of transient in that context. (check-in: 46d6b63630 ... user: stephan tags: kvvfs-v2) | 
| 20:36 |  | Add the sqlite3_db_filename() to the sqlite3.oo1.DB tracing output. Document an unfortunate design choice which was only recently recognized as such. (check-in: dec0c6073c ... user: stephan tags: kvvfs-v2) | 
| 20:14 |  | Add kvvfs URI flag transient=1 to cause the storage object to be removed when the last handle to the db is closed. By default it behaves like a filesystem would, retaining the storage objects. (check-in: c6183261be ... user: stephan tags: kvvfs-v2) | 
| 18:31 |  | Add the --blob-quote option to the ".mode" command in the CLI. (check-in: 76a986b10d ... user: drh tags: trunk) | 
| 18:17 |  | Fix a typeof check so that it works in Worker threads. (check-in: 3f2ed39e40 ... user: stephan tags: kvvfs-v2) | 
| 18:08 |  | Have kvvfs enforce ASCII-only names to avoid truncating multibyte characters in kvvfs's key buffer. (check-in: 03cb2a13c7 ... user: stephan tags: kvvfs-v2) | 
| 17:44 |  | Change some 32-bit integers in fts3_write.c to 64-bit to reduce the chances of integer overflow. (check-in: 8f8a7224c1 ... user: dan tags: trunk) | 
| 16:46 |  | Add the QRF_BLOB_Size encoding for BLOBs in QRF. (check-in: bafff695d3 ... user: drh tags: trunk) | 
| 16:04 |  | Avoid an assert() failure caused by processing a CREATE TABLE AS ... command that generates very long column names. (check-in: 06554a079d ... user: dan tags: trunk) | 
| 15:40 |  | Avoid signed integer overflow that may occur in a non-default build when processing a very large CREATE TABLE AS ... statement. (check-in: b7417497d1 ... user: dan tags: trunk) | 
| 15:17 |  | Get kvvfs v2 storage import working by disallowing it when there are opened db/journal handles. Move import/export out of the JsStorageDb class and into sqlite3_js_kvvfs_import/export_storage(). (check-in: c9e0b32278 ... user: stephan tags: kvvfs-v2) | 
| 13:26 |  | Show just the list of available modes and omit the current mode display for the ".mode --list" command in the CLI. (check-in: b9b2761fe7 ... user: drh tags: trunk) | 
| 13:24 |  | In the CLI on the ".mode" command, replace the recently added --no-limits option with "--limits L,C". And add --sw as an alias for --screenwidth. (check-in: cff0978587 ... user: drh tags: trunk) | 
| 13:22 |  | Doc typo fix. (check-in: d96cc772f0 ... user: stephan tags: trunk) | 
| 13:19 |  | Show just the list of available modes and omit the current mode display for the ".mode --list" command in the CLI. (leaf check-in: f3eb6ed253 ... user: drh tags: commits-to-wrong-branch) | 
| 13:10 |  | Add the fail-on-malloc()-error patch to the autosetup maintenance docs. (check-in: 526b88fb9b ... user: stephan tags: trunk) | 
| 12:27 |  | In the CLI on the ".mode" command, replace the recently added --no-limits option with "--limits L,C". And add --sw as an alias for --screenwidth. (check-in: caefe4c746 ... user: drh tags: commits-to-wrong-branch) | 
| 11:00 |  | Change our local copy of the third-party jimsh0.c program (used only during the build process) so that it prints an error message and exits if malloc() fails. This does not fix any actual bugs nor make the build process any "safer", but it might help prevent bothersome warnings from pedantic static analyzers. (check-in: 72c8a5b94c ... user: drh tags: trunk) | 
| 2025-11-23 |  |  | 
| 22:18 |  | Add the long-missing sqlite3_next_stmt() binding to JS/WASM. (check-in: de27d2d3f2 ... user: stephan tags: trunk) | 
| 22:13 |  | More work on kvvfs v2. Db imports work, in the sense that the storage is properly replaced, but the native side is not yet able to recover from that (and how to make it able to do so is not clear). (check-in: 4857c9d2fe ... user: stephan tags: kvvfs-v2) | 
| 22:09 |  | Add the long-missing sqlite3_next_stmt() binding to JS/WASM. (check-in: 53a99ff4ed ... user: stephan tags: kvvfs-v2) | 
| 20:05 |  | Docs, cleanups, and dead code removal. (check-in: 49db59aa9c ... user: stephan tags: kvvfs-v2) | 
| 16:54 |  | Get the kvvfs v2 working in 64-bit wasm builds (a 0 vs null vs BigInt issue). (check-in: 4811742688 ... user: stephan tags: kvvfs-v2) | 
| 16:35 |  | Eliminate the superfluous 'kvvfs-' part of keys for transient storage and cleanup their export a bit. Reminder for later: we could potentially use the dbpage vtab to serialize any db to this JSON-friendly form. (check-in: f63014f70f ... user: stephan tags: kvvfs-v2) | 
| 15:38 |  | Initial work on eliminating the superfluous-for-transient-storage kvvfs key prefixes and implementing kvvfs db import. (check-in: 6bc6405941 ... user: stephan tags: kvvfs-v2) | 
| 13:33 |  | Add tests demonstrating basic concurrent use of kvvfs storage and that kvvfs still works after wiping the storage out from under it). (check-in: cbbb9e61ae ... user: stephan tags: kvvfs-v2) | 
| 12:14 |  | Bump kvvfs's sqlite3_vfs::iVersion to 2 so that its xCurrentTimeInt64() can be used. Add JsStorageDb.exportToObject() to serialize a kvvfs to a JSON-friendly form. (check-in: 393c1beee0 ... user: stephan tags: kvvfs-v2) | 
| 11:28 |  | Replace an alloc/free on every read of a JS kvvfs key with a one-time alloc and a long paragraph explaining why we have to leak it. (check-in: 66fb9978f0 ... user: stephan tags: kvvfs-v2) | 
| 10:55 |  | New test case for bSplitColumn in QRF. (check-in: 6d01f9f49e ... user: drh tags: trunk) | 
| 00:45 |  | In QRF in bSplitColumn mode, increase the spacing between columns to be as wide as five, for improved legibility, if screen width allows. (check-in: 673648cc81 ... user: drh tags: trunk) | 
| 00:10 |  | Use the new "split" mode to simplify and improve the implementation of the ".tables" and ".indexes" commands in the CLI. Fix a bug in QRF that this new implementation found. (check-in: b73df612b6 ... user: drh tags: trunk) | 
| 2025-11-22 |  |  | 
| 23:34 |  | Add "split" mode in the CLI. Fix CLI commands given on the command line so that they honor --once in prior commands. (check-in: 2530f04d1b ... user: drh tags: trunk) | 
| 21:47 |  | Add the --no-limits option to the .mode command in the CLI. (check-in: 3cee168e88 ... user: drh tags: trunk) | 
| 21:34 |  | In QRF, change the name of the bWrapSnglCol flag to bSplitColumn. Similar changes to the TCL interface. (check-in: 76512b3b80 ... user: drh tags: trunk) | 
| 19:29 |  | Cherrypick [0b53be562f1e] into trunk. (check-in: e86e5723d7 ... user: stephan tags: trunk) | 
| 19:21 |  | This combination of kvvfs callbacks seems to work okay for both persistent and transient storage, but it's unclear why/how, as xAccess() does not appear to be doing anything useful (and misbehaves when told to report that the storage object in question exists). These tests fail in 64-bit builds for as-yet-unknown reasons (other tests continue to pass in 64-bit). (check-in: 69e591d005 ... user: stephan tags: kvvfs-v2) | 
| 17:27 |  | Fix a misinteraction of CREATE/READONLY flags in the OPFS VFS. (check-in: 0b53be562f ... user: stephan tags: kvvfs-v2) | 
| 16:43 |  | Ignore semicolons at the end of dot-commands, if the compatibility date is 20251115 or later. Forum thread 2025-10-09T21:12:32z (check-in: 7a9e753e97 ... user: drh tags: trunk) | 
| 16:29 |  | Fix storage-size counting of non-local/non-session kvvfs storage. Rename the 'global' buildin kvvfs storage object to 'localThread'. (check-in: 61606be2ae ... user: stephan tags: kvvfs-v2) | 
| 15:49 |  | Cherrypick [e0b33b51229a] for the latest wasmutil/jaccwabyt - all tests are happy. Fix a make deps bug which can break parallel builds involving fiddle. (check-in: 0be777bd6c ... user: stephan tags: trunk) | 
| 07:35 |  | Incremental work on kvvfs v2. (check-in: 60d61cf383 ... user: stephan tags: kvvfs-v2) | 
| 05:37 |  | Factor KVVfsFile::zName back out. Remove the extraneous part of the storage keys for non-local/non-session storage. (check-in: d2bf96d68c ... user: stephan tags: kvvfs-v2) | 
| 02:43 |  | Demonstrate completely transient and a semi-transient (until page reload) kvvfs instances. (check-in: 3f9ff98733 ... user: stephan tags: kvvfs-v2) | 
| 02:23 |  | Latest side-stream jaccwabyt/wasmutil. (check-in: e0b33b5122 ... user: stephan tags: kvvfs-v2) | 
| 00:38 |  | Implementation of bWrapSnglCol in QRF. (check-in: 4bbd3f7ead ... user: drh tags: trunk) | 
| 2025-11-21 |  |  | 
| 22:13 |  | Get initial JS overrides of the kvvfs sqlite3_vfs and sqlite3_io_methods in place. It now tracks a distinct Storage-ish object per sqlite3_file instance. (check-in: 19a3349a20 ... user: stephan tags: kvvfs-v2) | 
| 20:10 |  | Add the bWrapSnglCol flag to the QRF spec, though it is not yet documented and does not yet work. Fix column output so that it omits trailing space. (check-in: a858027fc9 ... user: drh tags: trunk) | 
| 19:47 |  | Need a double-underscore prefix on the cplusplus macro (check-in: 04394387e6 ... user: drh tags: trunk) | 
| 19:01 |  | Fix the qrf.h header so that it is usable from C++ (check-in: 1105b71093 ... user: drh tags: trunk) | 
| 18:55 |  | More work towards using JS generic Storage objects as db page storage via kvvfs. (check-in: 90a33941c6 ... user: stephan tags: kvvfs-v2) | 
| 18:49 |  | Remove kvvfs-specific filename validation from the oo1.DB ctor. (check-in: 2062752922 ... user: stephan tags: kvvfs-v2) | 
| 17:30 |  | Fix typo in the implementation of the ".schema" command of the CLI for the case when there are ATTACH-ed databases. (check-in: 8a230e4da2 ... user: drh tags: trunk) | 
| 15:23 |  | Initial restructuring of kvvfs working towards generic support of JS's Strorage interface as storage rather than hard-coding it to just window.localStorage and window.sessionStorage. The eventual goal of that is to have a storage solution which works without the POSIX I/O APIs, getting us one step closer to a wasi-sdk build. It would be transient, like Emscripten's virtual filesystem, and could hypothetically replace our dependency on that particular feature. Rename the kvstorage pieces to kvrec... (check-in: 190ef4a940 ... user: stephan tags: kvvfs-v2) | 
| 15:03 |  | Add new file missing from [3c4061428544]. (check-in: 41f94eca01 ... user: stephan tags: trunk) | 
| 14:15 |  | Enhance ALTER TABLE to support adding and removing NOT NULL and CHECK constraints. (check-in: 895498e443 ... user: drh tags: trunk) | 
| 11:00 |  | Fix the QRF so that it omits the closing text from styles "json" and "jobject" when the result set it empty. (check-in: e1ba175124 ... user: drh tags: trunk) | 
| 10:49 |  | Move the JS pieces of kvvfs into their own file to facilitate pending feature experimentation. (check-in: 3c40614285 ... user: stephan tags: trunk) | 
| 2025-11-20 |  |  | 
| 22:46 |  | Test cases for the fix in the prior check-in. (check-in: 4d41bee75e ... user: drh tags: trunk) | 
| 21:08 |  | Preserve auto-explain mode across mode changes, as has always been the case before. (check-in: 8896185ae0 ... user: drh tags: trunk) | 
| 12:41 |  | Fix hwtime.h (broken by [305bb022368bac9f]) so that unamalgamated windows builds work again. (check-in: 1245319257 ... user: drh tags: trunk) | 
| 12:14 |  | Remove an extraneous loop level from the JS bootstrapping bits. (check-in: 86817b4e9f ... user: stephan tags: trunk) | 
| 12:12 |  | Enable the SQLITE_EXPERIMENTAL_PRAGMA_20251114 in the WASM builds for the sake of VFS experimentors. The links in the comments describe what that does. (check-in: aaa55a3ebf ... user: stephan tags: trunk) | 
| 11:06 |  | Use sqlite3MPrintf() instead of sqlite3_mprintf() internally in alter.c, for improved OOM reporting. (closed check-in: 57555d75da ... user: drh tags: alter-table-constraints) | 
| 02:03 |  | Additional checks for OOM conditions. (check-in: 8fafd4988b ... user: drh tags: alter-table-constraints) | 
| 00:56 |  | Mark an unreachable branch with NEVER(). (check-in: df2259a32f ... user: drh tags: alter-table-constraints) | 
| 2025-11-19 |  |  | 
| 23:11 |  | Call sqlite3OomFault() rather than setting db->mallocFailed directly. (check-in: 4fd5dfa215 ... user: drh tags: alter-table-constraints) | 
| 19:37 |  | Fix three harmless declaration-after-statement warnings in the new CLI code. (check-in: b65b29656c ... user: drh tags: trunk) | 
| 15:25 |  | Improved comments in code. Omit unneeded wrapper functions. (check-in: 3485c7c81b ... user: drh tags: alter-table-constraints) | 
| 11:31 |  | Strip unterminated "--" comments from the end of constraints added using ALTER TABLE ADD CONSTRAINT or ADD NOT NULL. (check-in: c73ab47b02 ... user: dan tags: alter-table-constraints) | 
| 11:28 |  | Clarification to the header comment on dropConstraintFunc(). No changes to code. (check-in: 4bd53fbb20 ... user: drh tags: alter-table-constraints) | 
| 10:26 |  | Trivial simplification to the ALTER TABLE ADD CONSTRAINT code. (check-in: 383965b3ba ... user: drh tags: alter-table-constraints) | 
| 2025-11-18 |  |  | 
| 19:58 |  | Fix another problem that could follow an OOM fault on this branch. (check-in: c4cc80eb28 ... user: dan tags: temp-trigger-refs) | 
| 19:48 |  | Avoid a segfault that could follow an OOM in an ALTER TABLE statement. (check-in: 0e4c2b889e ... user: dan tags: temp-trigger-refs) | 
| 19:36 |  | Merge the latest trunk changes into the alter-table-constraints branch. (check-in: 31d7aeeaef ... user: drh tags: alter-table-constraints) | 
| 19:34 |  | Merge the latest trunk enhancements into the temp-trigger-refs branch. (check-in: 0957f4c9c4 ... user: drh tags: temp-trigger-refs) | 
| 19:11 |  | Do not attempt to make the generated file "shell.c" read-only. This turns out to be annoying in general, and especially annoying on macs. (check-in: 6ab18d036a ... user: drh tags: trunk) | 
| 17:49 |  | New subcomponent, the Query Result Formatter (QRF), that formats query results for display to humans on a fixed-width font terminal. Rework the CLI to make use of the QRF. Renovate the .mode command of the CLI. Also incorporate the QRF into the TCL interface as the "format" method. (check-in: 7e460ffa5a ... user: drh tags: trunk) | 
| 17:27 |  | Add support for SQLITE_LIMIT_PARSER_DEPTH to limit the size of the stack used by the parser. This can help prevent deeply nested parse trees that then cause problems on machines with smaller CPU stacks. Modify the %realloc and %free directives of Lemon and add the new %stack_size_limit directive in support of this capability. (check-in: 52ba0c731d ... user: drh tags: trunk) | 
| 15:40 |  | Modify the "%realloc" and "%free" commands in Lemon so that the functions they specify take an extra parameter at the end, the %extra_context pointer. This allows the implementation to distinguish between OOM errors and failures to increase the stack size because of the stack size limit. (closed check-in: 9862c945d9 ... user: drh tags: parser-stack-size) | 
| 15:20 |  | Lower the default stack size for the parse to 50. (check-in: 41fe19ab05 ... user: drh tags: parser-stack-size) | 
| 14:48 |  | Add the SQLITE_LIMIT_PARSER_DEPTH value for sqlite3_limit(). This isn't something that many applications will need, but it is useful for testing. (check-in: 8f0b07f361 ... user: drh tags: parser-stack-size) | 
| 13:03 |  | Yet another attempt at controlling the parser stack size. (check-in: cb19986dc6 ... user: drh tags: parser-stack-size) | 
| 10:38 |  | Fix incorrect "#line" generation in Lemon. (check-in: 5c0214df2c ... user: drh tags: trunk) | 
| 2025-11-17 |  |  | 
| 23:55 |  | Add api.oo1=0 flag to ext/wasm/GNUmakefile to strip out the sqlite3.oo1 pieces from the build. Part of the ongoing response to forum post 4b7d45433731d2e0. (check-in: ea48567ac5 ... user: stephan tags: trunk) | 
| 19:46 |  | If SQL is entered as additional command-line arguments to CLI, that counts the same as getting input from a pipe or file for the purpose of determining the default format mode. (closed check-in: 3d55ec15a9 ... user: drh tags: qrf) | 
| 19:19 |  | Fix a harmless compiler warning. (check-in: 6621737cc0 ... user: drh tags: trunk) | 
| 18:51 |  | The -textjsonb option is on by default in the standard non-batch mode of the CLI. New fast screening test for JSONB. (check-in: fd09f934d6 ... user: drh tags: qrf) | 
| 16:44 |  | CLI comes up in legacy "list" mode if neither stdin or stdout are a tty, or if "--compat 20251114" or earlier is specified, or if "--batch" is used. New modes "batch" and "tty" select either legacy "list" mode or the newer "qbox" mode with limits. Make CVS output compatible with legacy. Break out ".import" into a separate subroutine in anticipation of forthcoming improvements. General code cleanup. (check-in: f6bfcea9a0 ... user: drh tags: qrf) | 
| 00:15 |  | Fix test cases impacted by the use of ~/.sqliterc (check-in: 8fc05faef9 ... user: drh tags: qrf) | 
| 2025-11-16 |  |  | 
| 23:06 |  | Fix a bug in ".show". Adjust test cases for the new default mode. (check-in: 2aebd7bfec ... user: drh tags: qrf) | 
| 17:27 |  | Default .mode is now qbox with limits. New --compat YYYYMMDD command-line option, and new -DCOMPATIBILITY_DATE=YYYYMMDD compile-time options can override. Command ".mode YYYYMMDD" sets the mode to the default for the date given. (check-in: 52e0223759 ... user: drh tags: qrf) | 
| 10:49 |  | Fix a long-standing bug in .schema when the argument contains a ".". (check-in: 404576214f ... user: drh tags: qrf) | 
| 02:32 |  | Fix a problem with the new command-line parsing in the CLI. (check-in: 3e02804c38 ... user: drh tags: qrf) | 
| 01:34 |  | Improved --safe mode defenses. Fix a test case error. (check-in: eacc9db80d ... user: drh tags: qrf) | 
| 01:03 |  | Improved error detection in the argument to .mode --widths. (check-in: 4d774ee495 ... user: drh tags: qrf) | 
| 00:56 |  | Fix formatting problem in .schema and .fullschema (check-in: 4035fb15ff ... user: drh tags: qrf) | 
| 2025-11-15 |  |  | 
| 20:10 |  | Help text improvements in the CLI. (check-in: dcaabd97d5 ... user: drh tags: qrf) | 
| 16:06 |  | Remove an unused struct. (check-in: 93dd0e62b7 ... user: stephan tags: trunk) | 
| 15:30 |  | Bug fix to the --lineline option in the CLI. Other minor CLI tweaks. (check-in: 40f81a9f9f ... user: drh tags: qrf) | 
| 15:30 |  | JNI: when validing the eTextRep argument in sqlite3_create_function(), only validate the lower four bits (the high bits are for other flags). Add flags to test functions to ensure this case is triggered. (check-in: aab640be7b ... user: stephan tags: trunk) | 
| 15:18 |  | Columnar modes respond to nScreenWidth. (check-in: 60d26ebb4d ... user: drh tags: qrf) | 
| 15:09 |  | Replace the JNI binding's internal use of sqlite3ErrorWithMsg() with sqlite3_set_errmsg() and have it handle OOM in a way consistent with the rest of the JNI bindings. (check-in: 0899a9eb3a ... user: stephan tags: trunk) | 
| 13:30 |  | Random JS build cleanups and parallel build mkdir race fixes. Reinstate the recently-removed [a65bd978cbc646ec] after finding a reformulation which works on Emscripten 4.0.19 (and saves about 85kb on the JS deliverables). (check-in: acb1525a49 ... user: stephan tags: trunk) | 
| 12:09 |  | Columnar formats respond to --screenwidth restrictions by removing cell padding. (check-in: cbe233ca13 ... user: drh tags: qrf) | 
| 11:53 |  | Reorder initialization of sqlite3ApiBootstrap()'s bootstrapped config object so that it's possible to inject the wasm memory and exports via that, to simplify plugging-in of sqlite3-api.js in other builds. Previously the build-provided wasm exports/memory overrode any which a client might want to use. (check-in: 2bd0addb60 ... user: stephan tags: trunk) | 
| 11:30 |  | Generic internal JS cleanups towards improving portability of sqlite3-api.js to other build systems. (check-in: 5bc37e5c2f ... user: stephan tags: trunk) | 
| 11:28 |  | Data structure improvements on columnar layout. Prep work for getting columnar layouts to respond to nScreenWidth. (check-in: 777eeb2ed2 ... user: drh tags: qrf) | 
| 09:19 |  | Move sqlite3-api-cleanup.js into post-js-footer.js to remove the final direct Emscripten dependency from the intermediary build product sqlite3-api.js (the whole library, waiting to be bootstrapped). This is partly in response to forum post 4b7d45433731d2e0, which demonstrates a potential use case for a standalone sqlite3-api.js. This is a build/doc change, not a functional one. (check-in: 2fcbd8e17d ... user: stephan tags: trunk) | 
| 08:05 |  | API doc typo fix. (check-in: c1e9791fbf ... user: stephan tags: trunk) | 
| 00:23 |  | Improve columnar layout in QRF so that it correctly deals with control characters, and especially tabs. (check-in: 0650e2b831 ... user: drh tags: qrf) | 
| 2025-11-14 |  |  | 
| 21:49 |  | Make use of the new sqlite3_str_free() interface in the CLI. (check-in: 2e07bc29ab ... user: drh tags: qrf) | 
| 21:40 |  | Enforce the --charlimit constraint in QRF. (check-in: c25f8fdedd ... user: drh tags: qrf) | 
| 20:11 |  | Merge the latest trunk enhancements into the qrf branch. (check-in: 9ee892b4f9 ... user: drh tags: qrf) | 
| 20:09 |  | New interfaces sqlite3_str_truncate() and sqlite3_str_free(). Version number increases to 3.52.0. (check-in: fa85534ed9 ... user: drh tags: trunk) | 
| 19:04 |  | Fix harmless display bug in previous check-in. Add the -noinit command-line option to bypass reading the ~/.sqliterc file. (check-in: 229650a01d ... user: drh tags: qrf) | 
| 18:41 |  | Add the --tag and --list options to the ".mode" command. (check-in: 9daf98c2f4 ... user: drh tags: qrf) | 
| 17:27 |  | If SQLITE_EXPERIMENTAL_PRAGMA_20251114 is defined at build-time, send an experimental pragma file-control to the VFS if a call to take a SHARED lock is to be immediately followed by one to take a RESERVED. (check-in: e2b3f1a948 ... user: dan tags: trunk) | 
| 17:23 |  | Slight restructure of the previous checkin. (check-in: 8ff98747c0 ... user: stephan tags: trunk) | 
| 17:11 |  | Add the --once option to the .mode command. Improvements to help text. (check-in: 253980122a ... user: drh tags: qrf) | 
| 16:56 |  | In sqlite3-api-cleanup.js, if no Emscripten module is detected then simply return, rather than throw. This should enable the generated sqliet3-api.js (normally an intermediary file) to be used as-is, with the caveat that the caller has to bootstrap the library themselves. (check-in: d64c9cd4c7 ... user: stephan tags: trunk) | 
| 15:03 |  | Remove a harmless duplicated makefile var assignment from mkwasmbuilds.c. (check-in: 4b4a6fbe20 ... user: stephan tags: trunk) | 
| 14:59 |  | Fix harmless compiler warnings. Incorporate "USAGE:" comments on dot-command implementations into the CLI ".help" command output. (check-in: 046bfab4a0 ... user: drh tags: qrf) | 
| 14:42 |  | Fix a makefile ordering bug which caused certain builds to fail unless others had already been built. Remove the superfluous sqlite3-node.wasm from the final deliverables dir (sqlite3-node.mjs uses sqlite3.wasm instead). (check-in: fd70088284 ... user: stephan tags: trunk) | 
| 13:07 |  | Fix various bugs and compiler warnings. All tests now passing on linux, mac, and windows. More testing needed, though. (check-in: 2220cb70c2 ... user: drh tags: qrf) | 
| 11:06 |  | Merge compiler-warning fix from trunk into the qrf branch. (check-in: 6ffab43ca3 ... user: drh tags: qrf) | 
| 11:02 |  | Fix a harmless compiler warning in testing code. (check-in: 5252a2e629 ... user: drh tags: trunk) | 
| 10:42 |  | Fix a memory error in the .schema command. (check-in: 3300ed34b5 ... user: drh tags: qrf) | 
| 03:26 |  | Suppress a harmless compiler warning. (check-in: 89cf1de7b6 ... user: drh tags: qrf) | 
| 02:27 |  | Merge all the latest trunk enhancements into the qrf branch. (check-in: 97b06e58ed ... user: drh tags: qrf) | 
| 02:22 |  | Improvements to --titles handling in the .mode command. Fix shell1 test cases having to do with quoting of NULL values in "tcl" mode. (check-in: 0fef770bd6 ... user: drh tags: qrf) | 
| 2025-11-13 |  |  | 
| 20:37 |  | A better fix for the issue with RETURNING triggers on eponymous virtual tables and SQLITE_SCHEMA errors first addressed by [45d820ca22]. (check-in: bf399992cb ... user: dan tags: trunk) | 
| 18:02 |  | Add an OOM test case for the issue with dropping RETURNING triggers when the schema is reset while preparing a statement that uses an eponymous vtab. (check-in: dc569288dd ... user: dan tags: trunk) | 
| 17:42 |  | Add better test case for the issue with dropping RETURNING triggers when the schema is reset while preparing a statement. (check-in: 6a9fdde109 ... user: dan tags: trunk) | 
| 17:37 |  | Generic makefile cleanups and doc updates. (check-in: e8b34b4178 ... user: stephan tags: trunk) | 
| 16:38 |  | Internal doc fix (renamed file). (check-in: cb8fb01fe1 ... user: stephan tags: trunk) | 
| 15:33 |  | JS code reformatting and doc updates. No functional changes. (check-in: c566c653e4 ... user: stephan tags: trunk) | 
| 14:47 |  | Add the #include -raw flag to ext/wasm/c-pp-lite.c to support a pending feature. (check-in: 42c30c3149 ... user: stephan tags: trunk) | 
| 11:52 |  | Get --screenwidth auto working on linux and windows. (check-in: 48a91f2067 ... user: drh tags: qrf) | 
| 11:36 |  | Fix a problem in the EXISTS-to-JOIN optimization ([e33da6d5dc964db8]) so that it works with nested WHERE and EXISTS statements. Forum post 0704c3c41e4 (check-in: d1e901eddc ... user: drh tags: trunk) | 
| 09:03 |  | mkwasmbuilds.c doc updates. Merge in the parts of [8611cf643b] which are not contentious. (check-in: cb0f0e2224 ... user: stephan tags: trunk) | 
| 08:26 |  | Teach c-pp to export its argv as a #define list so that we can embed it in the generates files. (check-in: 42f95ea71e ... user: stephan tags: trunk) | 
| 07:07 |  | Doc typo fix reported in forum post 00e49e9aa1. (check-in: 36cd33f634 ... user: stephan tags: trunk) | 
| 06:49 |  | Minor doc correction and a JS error message tweak. (check-in: c4ae21c89e ... user: stephan tags: trunk) | 
| 2025-11-12 |  |  | 
| 20:50 |  | Implement the --titles option to the .mode command. (check-in: cc25643ebc ... user: drh tags: qrf) | 
| 19:03 |  | More details on the format of the QRF_STYLE_Json and QRF_STYLE_JOBject. (check-in: 2b22321df9 ... user: drh tags: qrf) | 
| 18:54 |  | Continuing code cleanup. Various minor bug fixes. All legacy tests now pass. All planned features for ".mode" are implemented, at least at the interface level, though some features are still no-ops. (check-in: e5a81711d0 ... user: drh tags: qrf) | 
| 17:25 |  | Clarify that 'pikchr' is not a typo. (check-in: d7bb2ee6ec ... user: stephan tags: trunk) | 
| 13:46 |  | Add a new direct CLI testing script in test/modeA.clitest to demonstration that can be done. Rig this this to be run by test/shellB.test during normal testing. (check-in: 3107ebb9e1 ... user: drh tags: qrf) | 
| 12:44 |  | Rename QRF_STYLE_JsonLine to QRF_STYLE_JObject. Add new convenience modes to the CLI and generally refactor and improve the ".mode" command. Some tests are failing, mostly due to changes in error message output and similar. (check-in: 4c8109682e ... user: drh tags: qrf) | 
| 2025-11-11 |  |  | 
| 18:07 |  | Minor refactoring of some field names in the sqlite3_qrf_spec object, to try to make the names more intuitive, memorable, and succinct. (check-in: 3450bc7eec ... user: drh tags: qrf) | 
| 17:06 |  | Merge trunk with this branch. (check-in: d7d8d08f84 ... user: dan tags: alter-table-constraints) | 
| 16:50 |  | Add further test cases for temp triggers to this branch. (check-in: c33899c09b ... user: dan tags: temp-trigger-refs) | 
| 14:57 |  | Merge latest trunk with this branch. (check-in: 68c8d04c39 ... user: dan tags: temp-trigger-refs) | 
| 14:08 |  | Move the ".output" command into a separate subroutine. Enhance .output so that it is able to capture output in a sqlite3_str object, then compare that captured output we patterns to accomplish tests. (check-in: ddd1670447 ... user: drh tags: qrf) | 
| 00:44 |  | Factor the code for ".mode" out of do_meta_command() and into its own subroutine, to show how the new infrastructure makes this easy. We need to do the same for all the dot-commands. At the same time, add the -textjsonb option to .mode. (check-in: b48aa054df ... user: drh tags: qrf) | 
| 00:21 |  | Bug fixes. All tests are now passing. (check-in: a0fc99a3bd ... user: drh tags: qrf) | 
| 2025-11-10 |  |  | 
| 23:40 |  | Revamp the internal data structures that the CLI uses for tracking and managing the display mode. This is an incremental check-in. The code compiles and runs, but not all tests are passing. (check-in: 8cc581e53c ... user: drh tags: qrf) | 
| 22:05 |  | Keep explicit NULL values in QRF_STYLE_Json output. (check-in: cb07f3d441 ... user: drh tags: qrf) | 
| 20:11 |  | Merge all the latest trunk improvements into the qrf branch. (check-in: 1fc2298edc ... user: drh tags: qrf) | 
| 20:01 |  | Improved concurrency for "make devtest" by splitting fuzzdata1.db and fuzzdata2.db into slices for asan and ubsan testing. (check-in: 2c31176607 ... user: drh tags: trunk) | 
| 19:11 |  | Avoid dropping RETURNING triggers when the schema is reset while preparing a statement. (check-in: 45d820ca22 ... user: dan tags: trunk) | 
| 18:39 |  | Improved argument parsing and error message infrastructure for dot-commands in the CLI. (check-in: 23d5d09db8 ... user: drh tags: qrf) | 
| 17:37 |  | Avoid dropping RETURNING triggers when the schema is reset while preparing a statement. (closed check-in: e26bcef752 ... user: dan tags: returning-fix) | 
| 15:56 |  | The ".www" command is now handled by QRF. So at this point, QRF handles all query result formatting in the CLI and the legacy formatter has been removed from the code. (check-in: 35d4c7151e ... user: drh tags: qrf) | 
| 15:01 |  | Rewrite the ".schema" and ".fullschema" commands in the CLI so as to eliminate the need for MODE_Pretty and MODE_Semi. (check-in: 0eb0410f72 ... user: drh tags: qrf) | 
| 12:32 |  | Add the bTextNull field to sqlite3_qrf_spec. Use QRF to implement "tcl" mode in the CLI. (check-in: 2ba92320db ... user: drh tags: qrf) | 
| 11:05 |  | Fix a missing va_end() call in the intckMprintf() function of the incremental integrity-check extension. (check-in: 62ad2350e3 ... user: drh tags: trunk) | 
| 11:00 |  | A more compact fix for the problem first fixed by [5f5a736f88]. (check-in: 7a644178c8 ... user: dan tags: trunk) | 
| 10:47 |  | Have sqlite3_set_errmsg() return SQLITE_MISUSE if a NULL db handle is passed to it, as documented. (check-in: 5f5a736f88 ... user: dan tags: trunk) | 
| 10:46 |  | Merge all the latest trunk fixes, and especially the fix for the ".www" command in the CLI, into the qrf branch. (check-in: 07a5523cf2 ... user: drh tags: qrf) | 
| 07:41 |  | Reworking of JS internals to support binding of nested C structs (like sqlite3_index_constraint and friends) and allow some of the automated JS/C conversions to be plugged in at the struct-binding level, simplifying how struct members, in particular function pointers, can be used from JS. (check-in: bb4fd5b789 ... user: stephan tags: trunk) | 
| 01:46 |  | Fix the ".www" command of the CLI so that it works on unix systems with newer web browsers that do not allow access to files in /tmp. (check-in: 2f918c14ba ... user: drh tags: trunk) | 
| 00:18 |  | Fixes to autoExplain (check-in: 38e4171697 ... user: drh tags: qrf) | 
| 2025-11-09 |  |  | 
| 17:25 |  | Enforce mxRowHeight in columnar layouts. Code clean-up. Minimum column with is 2 if there is wrapping, in case there are double-wide characters. (check-in: 0ee8fc00af ... user: drh tags: qrf) | 
| 07:11 |  | Minor doc typo fix reported in forum post 939d5864df. (check-in: a1f9c977b8 ... user: stephan tags: trunk) | 
| 06:51 |  | CLI shell: ensure that .dump propagates its error code on error. Problem reported in forum post a4f3873b0a. (check-in: 850b92b634 ... user: stephan tags: trunk) | 
| 06:32 |  | In sqlite3.wasm.allocFromTypedArray(), swap the order of the allocation and the access to the heap view object to work around an obscure issue when allocating huge blobs. The heap view object could previously become stale via the following allocation if the underlying WebAssembly.Memory object needed to grow. Essentially a realloc()-moves-the-pointer situation but it only comes up with huge allocations under the right circumstances and would trigger a JS exception (as opposed to corrupting the W... (check-in: 0f712b6b05 ... user: stephan tags: trunk) | 
| 00:37 |  | Fixes for harmless static analyzer warnings. (check-in: 37bd136ae8 ... user: drh tags: qrf) | 
| 00:01 |  | Remove several more hundred lines of dead code from shell.c.in. (check-in: 9de3e1d3fb ... user: drh tags: qrf) | 
| 2025-11-08 |  |  | 
| 23:35 |  | Remove about a thousand lines of dead code from shell.c.in. (check-in: efbcc7decd ... user: drh tags: qrf) | 
| 22:46 |  | Name changes. QRF_TEXT_Off becomes QRF_TEXT_Plain. mxTotalWidth becomes nScreenWidth. (check-in: 871567e377 ... user: drh tags: qrf) | 
| 18:31 |  | Improvements to QRF_STYLE_Line. Make it responsive to mxTotalWidth and mxRowHeight. Indent subsequent lines appropriately. Add corresponding test cases. (check-in: 23ddc38cf1 ... user: drh tags: qrf) | 
| 17:08 |  | Fix minor issues with alignment and quoting. All legacy tests are now passing. (check-in: 712b890531 ... user: drh tags: qrf) | 
| 15:25 |  | Miscellaneous fixes to the new alignment configurations. (check-in: 787fb4c2ac ... user: drh tags: qrf) | 
| 14:04 |  | Separate column width specification from cell alignment. (check-in: 5dadfd7f41 ... user: drh tags: qrf) | 
| 10:48 |  | Minor changes to fix a few legacy test cases. Snapshot prior to yet another refactor. (check-in: b429e7882f ... user: drh tags: qrf) | 
| 03:44 |  | Make sure columnar outputs always invoke qrfWrite(). (check-in: 2108404119 ... user: drh tags: qrf) | 
| 03:20 |  | Rewrite the table layout in QRF. Easier to maintain now. Still a few shell test failures, but much better after this rewrite. (check-in: 25838d282c ... user: drh tags: qrf) | 
| 2025-11-07 |  |  | 
| 19:29 |  | Update this branch with latest trunk changes. (check-in: 0b849623f8 ... user: dan tags: temp-trigger-refs) | 
| 19:23 |  | Merge latest trunk changes with this branch. (check-in: c0571f0010 ... user: dan tags: alter-table-constraints) | 
| 19:20 |  | Fix some problems. But there is a need to do major surgery on the table layout algorithm, so it seems good to snapshot before proceeding. (check-in: 9052991a89 ... user: drh tags: qrf) | 
| 17:48 |  | Incremental check-in. QRF is working, but there are many test failures in the shell test modules. (check-in: de7fc9afc4 ... user: drh tags: qrf) | 
| 14:31 |  | Begin the process of integrating QRF into the CLI. Adjust makefiles. Include the QRF code in shell.c. But QRF is not yet used by the CLI. (check-in: 7596fc32b3 ... user: drh tags: qrf) | 
| 13:23 |  | Update documentation to clarify that mxTotalWidth and mxLength are not yet implemented. (check-in: c25806fc54 ... user: drh tags: qrf) | 
| 13:11 |  | Rename "mxWidth" to "mxColWidth". Add "mxTotalWidth" and "mxLength" settings and hook them up to the TCL interface, but those settings are not yet implemented. (check-in: 3f327e8fbc ... user: drh tags: qrf) | 
| 12:15 |  | Fix an error-message test case broken by the previous check-in. (check-in: fb04035490 ... user: drh tags: qrf) | 
| 12:12 |  | Add the JsonLine output style. (check-in: 7dfba841cf ... user: drh tags: qrf) | 
| 11:46 |  | Add some UNUSED_PARAMATER()s for the sake of -pedantic -DSQLITE_OMIT_WAL. (check-in: 620434a4a2 ... user: stephan tags: trunk) | 
| 11:22 |  | Fix a harmless static-analyzer warning. (check-in: 9819fc5aa7 ... user: drh tags: trunk) | 
| 00:05 |  | Fix the size of an integer in the TCL interface. (check-in: 04156c34e4 ... user: drh tags: qrf) | 
| 2025-11-06 |  |  | 
| 23:34 |  | Put the style constants in alphabetic order. Improvements to documentation. (check-in: d5e891426f ... user: drh tags: qrf) | 
| 19:22 |  | Revamp so that Explain and Eqp styles force the statement into the the appropriate explain-mode for the rendering and restore it afterwards. Add the new scanstatus styles: Stats, StatsEst, and StatsVm. (check-in: 128f114907 ... user: drh tags: qrf) | 
| 17:28 |  | Changes to try to improve EXPLAIN capabilities. This is a snapshot prior to an attempt to make sweeping changes in that area of the interface. (check-in: e7367d8089 ... user: drh tags: qrf) | 
| 13:25 |  | Merge the latest trunk enhancements into the qrf branch. (check-in: 4e2ada6ad8 ... user: drh tags: qrf) | 
| 13:19 |  | Get the sqlite3Hwtime() function working on ARM. Enable scanstatus by default on Windows CLI builds. (check-in: 305bb02236 ... user: drh tags: trunk) | 
| 12:25 |  | Improvements to documentation for stmt_scanstatus interfaces. (check-in: 5f3e883d18 ... user: drh tags: trunk) | 
| 2025-11-05 |  |  | 
| 22:48 |  | More test cases and a bug fix. (check-in: 9a9c458077 ... user: drh tags: qrf) | 
| 22:33 |  | Further testing and bug fixing. (check-in: d6b1bac15a ... user: drh tags: qrf) | 
| 18:35 |  | The qrf-tester.c program was removed in the previous check-in, but the makefile rules to build it were not. This check-in fixes that oversight. (check-in: d154269640 ... user: drh tags: qrf) | 
| 18:27 |  | The TCL interface for QRF is now feature complete (at least until I think of new features to add). There is still a lot of testing, bug-fixing, and documentation to be done, however. (check-in: 633fe4fe58 ... user: drh tags: qrf) | 
| 16:29 |  | A few simple test cases added. (check-in: 7200372ed2 ... user: drh tags: qrf) | 
| 15:44 |  | New style setting QRF_STYLE_Auto which chooses box, explain, or EQP depending on the statement to be rendered. Improvements to memory management. Updates to the documentation. (check-in: 468ba188f0 ... user: drh tags: qrf) | 
| 14:07 |  | The new "format" method in the TCL interface is now partially functional. (check-in: ace1ebda08 ... user: drh tags: qrf) | 
| 12:37 |  | New build product "tclsqlite-ex.c" is the tclsqlite.c file with QRF added. This is now used whereever tclsqlite.c was used. Hence QRF is now in the testfixture and in the tclextension. The sqlite3 method is "format". That method is currently just a non-functional stub. (check-in: e08d21fe13 ... user: drh tags: qrf) | 
| 10:07 |  | JNI: doc tweaks. Add missing SQLITE_DBSTATUS_TEMPBUF_SPILL export. (check-in: 2c8938f43f ... user: stephan tags: trunk) | 
| 05:59 |  | TEA extension: add --tcl-stubs config flag, defaulting to zero, because there does not seem to be a universally-valid default for that. Addresses forum post cd6503fce2. (check-in: 4954b94763 ... user: stephan tags: trunk) | 
| 2025-11-04 |  |  | 
| 23:47 |  | Remove stale code from the fileio.c extension. (check-in: d26f115760 ... user: drh tags: trunk) | 
| 22:19 |  | Merge the latest trunk enhancements into the qrf branch (check-in: bed60e9be6 ... user: drh tags: qrf) | 
| 22:01 |  | Minor wasm build simplifications and extend the JS bindings of sqlite3_column/value_text() such that they include embeded NUL bytes in their result strings rather than truncate them at NUL. Consolidate the shell.c build flags from the canonical and autoconf makefiles into the configure script. (check-in: cee2e2576f ... user: stephan tags: trunk) | 
| 19:38 |  | Version 3.51.0 (check-in: fb2c931ae5 ... user: drh tags: trunk, release, major-release, version-3.51.0) | 
| 01:03 |  | Consolidate the three VFS-specific SEE tests into a shared routine. (closed check-in: 8ac0bf125d ... user: stephan tags: wasm-post-3.51) | 
| 00:15 |  | Move the hard-coded SHELL_OPT makefile values from the two main makefiles into their shared configure script. (check-in: 900885c4fb ... user: stephan tags: wasm-post-3.51) | 
| 00:10 |  | Merge trunk into wasm-post-3.51 to for some minor config script work. (check-in: bf55a4220a ... user: stephan tags: wasm-post-3.51) | 
| 2025-11-03 |  |  | 
| 21:38 |  | Add tests demonstrating [d0e803b90344]. (check-in: 7d22ee947d ... user: stephan tags: wasm-post-3.51) | 
| 21:26 |  | Add new file missing from [936990cf067a5ebb] (and subsequently deleted by fossil clean -x). Have speedtest1 wasm use the user-provided sqlite3.c, rather than forcing the canonical one, so that the wasm exports don't have to be re-generated for that special case. Remove a harmless duplicate -D... flag. (check-in: 7a592f5cd0 ... user: stephan tags: wasm-post-3.51) | 
| 15:34 |  | Use the same compile-time options for the CLI in the auto-setup tarball as are used in the canonical source tree. Forum post 8975130218. (check-in: ccabbe06d4 ... user: drh tags: trunk) | 
| 15:29 |  | Add new column "cwd" to the jobs table in testrunner.db. For recording the directory in which the test ran or is running. (check-in: 334305a469 ... user: dan tags: trunk) | 
| 12:36 |  | Adjust the dbstatus2.test module so that it works on 32-bit builds in addition to 64-bit builds. (check-in: dcaaa71e75 ... user: drh tags: trunk) | 
| 11:35 |  | Update test result output from test/dbstatus2.test module so that it reports the actual value of nTmpSpill when that value is out of range. (check-in: 382c8440d7 ... user: drh tags: trunk) | 
| 10:40 |  | Fix the test/speedtest.tcl script so that the CC= command-line option works. (check-in: 0de0ba878b ... user: drh tags: trunk) | 
| 10:14 |  | Fix a harmless memory leak in the CLI caused by the --cmd option generating an error message. (check-in: ab916bad18 ... user: drh tags: trunk) | 
| 2025-11-02 |  |  | 
| 22:57 |  | Add pedantic OOM detection logic to some disused diagnostic and test code. No changes to any deliverables. (check-in: abb6945fc6 ... user: drh tags: trunk) | 
| 2025-11-01 |  |  | 
| 20:14 |  | Update the test/walthread.test testing script so that it uses a random database name. (check-in: ce25a758d4 ... user: drh tags: trunk) | 
| 14:49 |  | Fix ext/wasm/mkdist.sh to use ./version-info instead of ../../version-info (a recent build change made to ensure that builds with a custom sqlite3.c embed that version info). (check-in: d25a92d889 ... user: stephan tags: trunk) | 
| 2025-10-31 |  |  | 
| 17:45 |  | Updates to the compile-for-windows.md document. No changes to code. (check-in: 652310a9f0 ... user: drh tags: trunk) | 
| 16:07 |  | Detect some cases of ambiguous column references in USING clauses that can come up with nested joins. (check-in: 38c993c8b7 ... user: dan tags: trunk) | 
| 15:18 |  | Change the name of a file-scope constant array in carray.c so that it does not conflict with other variables names in other files when carray.c is amalgamated. This is down to resolve a harmless compiler warning. (check-in: 5afd815ada ... user: drh tags: trunk) | 
| 13:44 |  | Update the main README.md file to use the robot-safe /rchvdwnld page on the server for downloads. (check-in: c1aa52361b ... user: drh tags: trunk) | 
| 11:23 |  | Fix the output of ".dump" in the CLI so that the sqlite_sequence table is created using correct syntax. (check-in: 4b9f2e1d44 ... user: drh tags: trunk) | 
| 2025-10-30 |  |  | 
| 22:10 |  | New assert() statements to help out static analyzers. (check-in: ba0c4bf072 ... user: drh tags: trunk) | 
| 21:11 |  | Add SQLITE_OMIT_WAL checks around some functions to avoid defining unused statics in such builds. Fixes OMIT_WAL builds on msys2 and passes tool/omittest-msvc.tcl. (check-in: f9b89535dc ... user: stephan tags: trunk) | 
| 20:18 |  | Simplify the EXPORTED_FUNCTIONS lists to use the preprocessor instead of per-list shell commands. (Unrelated) Remove part of the post-compilation JS filtering which breaks with emsdk 4.0.16+, which means the .js files are about 57kb bigger than they really ought to be. (check-in: 936990cf06 ... user: stephan tags: wasm-post-3.51) | 
| 19:32 |  | Merge trunk into the wasm-post-3.51 branch. (check-in: dda1735716 ... user: stephan tags: wasm-post-3.51) | 
| 18:52 |  | Further improvements the ".dump" command to handle corner cases. (check-in: cd0c77934f ... user: drh tags: trunk) | 
| 18:48 |  | Another (void), as per [dd6edfc09434]. (check-in: 7a822cfd38 ... user: stephan tags: trunk) | 
| 18:46 |  | Add (void) to two places for the sake of SQLITE_OMIT_WAL builds. Why these do not complain on tests in this tree, but do on a downstream tree (same machine, same compiler), is a mystery. (check-in: dd6edfc094 ... user: stephan tags: trunk) | 
| 18:39 |  | wasm: re-add SQLITE_OMIT_WAL to the c-pp-lite build flags. (check-in: 617f8ac40e ... user: stephan tags: trunk) | 
| 18:38 |  | Macro tweaks to get SQLITE_OMIT_WAL building. Remove some nearby stray EOL whitespace. (check-in: c31a944680 ... user: stephan tags: trunk) | 
| 18:33 |  | Fix a typo and improve wording in README.md (check-in: 34a662ce24 ... user: drh tags: trunk) | 
| 18:32 |  | Updates to the tool/warnings.sh script for better compiler warnings. (check-in: c9c64892c7 ... user: drh tags: trunk) | 
| 17:23 |  | Fix a harmless compiler warning associated with flexarrays. (check-in: f02cdf1d3e ... user: drh tags: trunk) | 
| 15:43 |  | Updates to the main README.md file. (check-in: ab10c4cd9f ... user: drh tags: trunk) | 
| 11:39 |  | Fix the CLI ".dump" command so that it does not make duplicate sqlite_sequence table entries as described in forum thread b1674b56ad. (check-in: c833f26a67 ... user: drh tags: trunk) | 
