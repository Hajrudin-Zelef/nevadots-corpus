---
id: collect-261121-veille-ia/langages-dev/sqlite-1
title: "SQLite"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-11-21"]
keywords: ["trunk", "check-in", "user", "tags", "sqlite", "test", "optimization", "shell"]
source: https://www.sqlite.org/src/timeline?bt=version-3.47.0&p=trunk&advm=1&udc=1
source_anchor: ""
source_lines: [1, 1500]
section: "Langages_Dev"
---
## 4420 ancestors of trunk back to version-3.47.0

| 2026-11-21 |  |  | 
| 13:17 |  | Slightly simplify how the shell's --quote flag handling is done to address bug report 2026-11-21T02:47:14Z. New test cases. (leaf check-in: 3b86db209d ... user: stephan tags: trunk) | 
| 12:30 |  | Do not allow PRAGMA temp_store changes during a backup. Bug 2026-11-21T08:23:41Z (check-in: d5b66ee77d ... user: drh tags: trunk) | 
| 11:47 |  | Do not convert -0.0 to +0.0 when storing in a REAL column of a table. Requires the signbit() macro from <math.h> which is C99, but seems to be readily available on all platforms. Bug 2026-11-20T11:09:29Z (check-in: 7344f4d1d9 ... user: drh tags: trunk) | 
| 09:45 |  | Fix an early return out of a va_start/va_end block in the shell's pickStr(). (check-in: 64f067d529 ... user: stephan tags: trunk) | 
| 01:18 |  | Properly escape the content of the sqlite_statN tables when doing .fullschema in the CLI. Bug 2026-11-21T01:05:21Z (check-in: f33b248fce ... user: drh tags: trunk) | 
| 2026-11-20 |  |  | 
| 20:37 |  | Fix signed integer overflow issues in FTS3. Bugs 2026-11-20T13:17:45Z and 2026-11-20T13:17:08Z. (check-in: 41fb800ec9 ... user: drh tags: trunk) | 
| 20:35 |  | Add a test case for the previous check-in. (check-in: 8b56ce8206 ... user: drh tags: trunk) | 
| 20:13 |  | Enhance the sqlite3changegroup_change_blob() API so that it returns an error if given an out-of-bounds length parameter. Enhancement request 2026-11-20T13:15:17Z (check-in: 8d96fe8c9f ... user: drh tags: trunk) | 
| 19:37 |  | Prevent excess recursion on a ridiculous USING clause. Bug 2026-11-20T13:10:07Z (check-in: 8bfa34eefb ... user: drh tags: trunk) | 
| 18:33 |  | Fix a typo in [5eb1662fbb3af945]. (check-in: 709506be42 ... user: drh tags: trunk) | 
| 17:08 |  | Do not allow incrblob writes to a column that appears in the WHERE clause of a partial index. Bug 2026-11-20T00:58:36Z (check-in: d8e9029696 ... user: drh tags: trunk) | 
| 15:42 |  | Disable the SQLITE_DBCONFIG_FP_DIGITS settings, as it causes far more problems than it solves. (check-in: 90ba17da84 ... user: drh tags: trunk) | 
| 15:23 |  | Drop support for the dbdump.c extension, as it is a magnet for AI-generated bug reports. Remove all dbdump.c code and references from the repository. (check-in: 607bb0bf04 ... user: drh tags: trunk) | 
| 12:26 |  | As demonstrated by multiple bug reports, the SQLITE_DBCONFIG_FP_DIGITS setting from [3f16985dcc47a366] is unsafe if misused, and so it is here changed into a no-op. (closed check-in: 5b588b3e0f ... user: drh tags: noop-fp-digits) | 
| 10:07 |  | Add the -v option to the CLI, which if present causes each successful .check command to display its identify with "ok" on standard output. (check-in: b36768c467 ... user: drh tags: trunk) | 
| 10:04 |  | Fix the URI parsing built into the CLI so that it does the same thing with %-escapes as the SQLite core does. Bug 2026-11-20T03:13:17Z (check-in: 102d7b9adb ... user: drh tags: trunk) | 
| 02:20 |  | Fix an issue with the new test/backupnpage.test script so that it works on all configurations. (check-in: cd666df36d ... user: drh tags: trunk) | 
| 02:11 |  | Prevent a flush-loop in secure-delete of FTS5 which could overflow the CPU stack. (check-in: fa323051b3 ... user: drh tags: trunk) | 
| 02:08 |  | Bypass tests in test/misc03.sql that require virtual tables on builds that lack virtual table support. (check-in: f0c1b7bf8e ... user: drh tags: trunk) | 
| 2026-11-19 |  |  | 
| 23:58 |  | Fix a subtle problem with the transitive constraint optimization reported in bug 2026-11-19T21:38:21Z. (check-in: 284441d4d8 ... user: drh tags: trunk) | 
| 23:39 |  | Fix a jump destination resolution bug in the code generator that results from the findNoMatchContinuation() optimization that was added as part of the tree-tracker optimization that landed on trunk at [e24a8fe9867370bf]. Bug 2026-11-19T21:24:04Z (check-in: 2c5a10583c ... user: drh tags: trunk) | 
| 23:15 |  | Add a regression test for bug 2026-11-19T14:10:05Z even though the problem is already fixed. (check-in: 754c41cd06 ... user: drh tags: trunk) | 
| 23:03 |  | Harden fts5DoSecureDelete() against problems caused by index corruption. Bug 2026-11-19T13:05:58Z (check-in: 0fdb03e35d ... user: drh tags: trunk) | 
| 20:37 |  | Fix a harmless compiler warning. (check-in: e7d8e88d10 ... user: drh tags: trunk) | 
| 18:57 |  | Enhance both the CLI and the ext/misc/dbdump.c library so that it is more rebust against injection attacks. Inspired by bug report 2026-11-19T15:45:09Z but going well beyond that report. (check-in: 1e84c333e8 ... user: drh tags: trunk) | 
| 18:54 |  | In sqlite3_deserialize(), when built with SQLITE_ENABLE_API_ARMOR return SQLITE_MISUSE if the db is larger than the target buffer. No effect in other builds. Report 2026-11-19T00:48:08Z. (check-in: 9bb6f7807e ... user: stephan tags: trunk) | 
| 18:44 |  | Fix an infinite loop in QRF with .mode box --wrap 1 and double-width characters. Bug report 2026-11-19T15:40:01Z. (check-in: bc34f1a1c2 ... user: stephan tags: trunk) | 
| 16:25 |  | Do not push-down WHERE clause terms into DISTINCT subqueries or into compound subqueries other than UNION ALL unless those terms apply across all equivalence classes. Bug 2026-11-19T08:58:28Z. This is similar to the having-to-where optimization fix of [096fafb8e19ece22]. (check-in: 3a2cffd44b ... user: drh tags: trunk) | 
| 14:32 |  | Improvements to sqlite3_backup so that it does not corrupt the destination database if the application breaks the rules and has active transactions going on the destination at the time of the backup. Bug 2026-11-16T17:39:05Z (check-in: 99d8f6a9ac ... user: drh tags: trunk) | 
| 14:07 |  | Use autosetup's msg-result instead of user-notice in two places, as the latter goes to stderr and breaks Tcl's exec. (check-in: 88fe388f1a ... user: stephan tags: trunk) | 
| 2026-11-18 |  |  | 
| 23:58 |  | Fix test-case problems on Windows introduced by [f377150d867f7333] - forcedelete trying to delete a database file out from under an active connection, which is not allowed on Windows. (check-in: a234179ac9 ... user: drh tags: trunk) | 
| 23:42 |  | Fix issues on UPSERT with a UNIQUE index with duplicate columns. Bug 2026-11-16T13:32:44Z (check-in: 809f8c1528 ... user: drh tags: trunk) | 
| 19:58 |  | Fix deletion of scratch dbs in several tests. Test scripts only, no code changes. Belated Bug 3 from the bugs-ac23256beb report. (check-in: f153195f03 ... user: stephan tags: trunk) | 
| 19:50 |  | Fix the modeof= query parameter to URI filenames so that misuse does not cause an assertion fault or memory leak. Bug 2026-11-18T19:07:34Z (check-in: c2b40f017b ... user: drh tags: trunk) | 
| 19:15 |  | Merge fixes from the bugs-ac23256beb. This completes everything needed to fix all of the issues enumerated by bug report 2026-11-18T13:50:26Z. (check-in: b087f898c9 ... user: drh tags: trunk) | 
| 19:01 |  | Fix possible btree corruption caused by a multirow INSERT that deletes conflicting rows using ON CONFLICT. Bugs 2026-11-04T20:22:42Z (check-in: 22f08258b6 ... user: drh tags: trunk) | 
| 18:30 |  | Disable the before-trigger-read optimization if the trigger uses a generated column. Bug 2026-11-04T20:22:42Z (check-in: 1d6070c287 ... user: drh tags: trunk) | 
| 17:35 |  | CLI shell: Fix a NULL deref in .read heredoc parsing which could happen on OOM or an empty heredoc. Add an empty-heredoc test. (check-in: edcd4c4667 ... user: stephan tags: trunk) | 
| 17:31 |  | CLI shell: do not impose the db-configured size limits for error messages unrelated to the current db connection. Add tests ensuring that that truncation doesn't happen to the affected messages. (check-in: 4b68e72715 ... user: stephan tags: trunk) | 
| 16:47 |  | Fix unintended test exclusions. (Bug 4) (closed check-in: ccff4f4693 ... user: stephan tags: bugs-ac23256beb) | 
| 16:46 |  | Have session tests clear their log callbacks. (Bug 2) (check-in: 6279345378 ... user: stephan tags: bugs-ac23256beb) | 
| 16:44 |  | Correct a miscalculation in unixShmPurge(). (Bug 1) (check-in: 1a9ca48f4f ... user: stephan tags: bugs-ac23256beb) | 
| 16:42 |  | In fts5, avoid dropping the error code if an error occurs while loading the "averages" record. Report 2026-11-18T15:49:15Z. (check-in: cdfee84529 ... user: dan tags: trunk) | 
| 16:23 |  | Fix processing of window function RANGE frames that end at "0.0 PRECEDING". (check-in: 7320819bff ... user: dan tags: trunk) | 
| 16:22 |  | Improved parsing of options to the .recover command of the CLI. Bug 2026-11-18T16:10:01Z (check-in: acc4a0c499 ... user: drh tags: trunk) | 
| 16:06 |  | Fix the "excluded.*" columns of UPSERT so that they have the same affinity and collating sequence as the columns they represent. Bug 2026-11-18T08:08:53Z (check-in: 7cf52a64dd ... user: drh tags: trunk) | 
| 15:34 |  | Improved quoting of PATH results coming out of json_tree() and json_each(). Bug 2026-11-06T00:28:19Z (check-in: 5c10a8e4d5 ... user: drh tags: trunk) | 
| 14:35 |  | Improved cleanup before and after test cases. Partial solution to Bug 3 of 2026-11-18T13:50:26Z. (check-in: f377150d86 ... user: drh tags: trunk) | 
| 11:27 |  | If a generated column has a different datatype from the expression that defines it, then do not use that expression from a covering index. Recompute the expression as needed instead, so that the correct datatype comes out. Bug 2026-11-18T08:15:23Z. (check-in: ec0ffcae0b ... user: drh tags: trunk) | 
| 10:27 |  | Do not accept U+d7ff as a prefix character for the LIKE optimization since on UTF16be databases it would increment to a surrogate and become invalid UTF. Bug 2026-11-18T08:16:43Z. (check-in: 5b19e611d5 ... user: drh tags: trunk) | 
| 09:52 |  | Fix two dbstat bugs: NOCASE name lookups and pgsize/pgoffset for non-default schemas. Bugs 2026-11-04T00:50:04Z and 2026-11-04T00:50:51Z, respectively. (check-in: 4b3591c790 ... user: drh tags: trunk) | 
| 2026-11-17 |  |  | 
| 20:37 |  | Fix a minor configuration issue with the ext/expert/expert1.test script. (check-in: 0093f70ade ... user: drh tags: trunk) | 
| 20:21 |  | Minor fix to the dbdata.tcl script: make sure the database is not in auto_vacuum mode. (check-in: 3bef86371c ... user: drh tags: trunk) | 
| 17:53 |  | Make it an error to configure secure-delete mode on a contentless_delete=1 fts5 table. Prior to this it was not an error, but the setting had no effect at all. (check-in: 3497206012 ... user: dan tags: trunk) | 
| 17:33 |  | When a term is completely deleted from an fts5 index in secure-delete mode, ensure any prefix of it is also removed from the tree index. Forum post 2026-11-15T07:31:18Z. (check-in: 7a9c6378a2 ... user: dan tags: trunk) | 
| 17:33 |  | Remove a harmless code redundancy and compiler warning left over from [0c67cf34dc85f28e] (check-in: d19604f264 ... user: drh tags: trunk) | 
| 17:28 |  | Enhance PRAGMA integrity check to find surplus entries in a partial index. Enhancement request 2026-11-05T22:19:16Z. (check-in: ce9a666193 ... user: drh tags: trunk) | 
| 16:53 |  | Further details on commenting style in the AGENTS.md file. (check-in: e10d0410ef ... user: drh tags: trunk) | 
| 16:45 |  | Merge all recent trunk changes into this branch. (closed check-in: cbd0980c41 ... user: drh tags: integ-ck-partial-idx) | 
| 16:36 |  | Remove excess commentary. (check-in: 7560077775 ... user: drh tags: integ-ck-partial-idx) | 
| 16:21 |  | Prevent the "approximate_match" virtual table from reading its vocabulary from a table in any database other than temp and the database containing the virtual table. Issue F4 reported at 2026-11-17T15:14:07Z. (check-in: af1f29265b ... user: dan tags: trunk) | 
| 16:09 |  | Update AGENTS.md to encourage agents to use fewer words. (check-in: afa0a2e682 ... user: drh tags: trunk) | 
| 15:34 |  | Have the session module more consistently reject any changeset/patchset that contains a mix of changeset and patchset headers as corrupt. Issue F8 reported at 2026-11-17T15:14:07Z. (check-in: d6eb7a06c4 ... user: dan tags: trunk) | 
| 15:21 |  | Merge fixes for issues F3 (expert extension), F5 (dbdata extension) and F6 (vec1 extension) reported at 2026-11-17T15:14:07Z. (check-in: fbc5e92bdd ... user: dan tags: trunk) | 
| 15:05 |  | Fix the format() function (and its related printf() routines) so that the alternate-form flag ("#") on hexadecimal and octal outputs includes the 0x, 0X, or 0 prefix as part of the total width. Bug 2026-11-05T22:14:37Z (check-in: 0c67cf34dc ... user: drh tags: trunk) | 
| 14:06 |  | Use a statement journal if any trigger program or FK action recursively calls itself. Also tighten up the assert() used to check a statement journal is used when required. Report 2026-11-04T20:32:45Z. (check-in: 6c54ed03c1 ... user: dan tags: trunk) | 
| 11:33 |  | In sqlite3FpDecode() with a low rounding precision, add a couple of extra digits to the end of the conversion prior to the rounding process for improved rounding accuracy. Bug reports 2026-11-04T00:53:10Z and 2026-11-01T12:02:55Z. (check-in: ac5e7ddf77 ... user: drh tags: trunk) | 
| 10:25 |  | Fix a bug in the test/temptrigfault.test script that causes it to malfunction if SQLite is configured to use an in-memory journal. (check-in: ab67ba405e ... user: drh tags: trunk) | 
| 10:11 |  | Fix temptrigfault.test expectations to not rely on values which vary across by builds. Fix an array mis-read in the test_carray_bind() handling of blobs. Clear the test_carray_bind() isMalloc flag when the input size is 0 to avoid passing a string literal to free(). (check-in: 8682901664 ... user: stephan tags: trunk) | 
| 09:26 |  | Remove the invalid assert() which [2e834294b65] modified. (check-in: 2607cfcf9d ... user: stephan tags: trunk) | 
| 08:45 |  | Correct an always-true assert() spotted by gcc 15.2. (check-in: 2e834294b6 ... user: stephan tags: trunk) | 
| 07:26 |  | JS: more consistently handle binding of bigints which lie outside the range of integers. Forum report 2026-11-17T01:54:40Z. (check-in: ae7c9b80d6 ... user: stephan tags: trunk) | 
| 07:23 |  | CLI shell: disable sqlite3_completion_init() in fiddle builds, where it's unavailable. (check-in: fa9964fe4f ... user: stephan tags: trunk) | 
| 03:16 |  | Fix an extreme corner case OOM problem in PRAGMA integrity_check. (check-in: 785291512e ... user: drh tags: trunk) | 
| 2026-11-16 |  |  | 
| 23:50 |  | Restore tab-completion in the CLI, accidentally deleted by [d58866448b23b996]. Bug 2026-11-16T23:43:44Z. (check-in: f8c1113c37 ... user: drh tags: trunk) | 
| 21:14 |  | Enhance the pragma vtab so that it returns the schema hidden column. Bug 2026-11-04T20:26:34Z (check-in: 23a7c4d154 ... user: drh tags: trunk) | 
| 21:09 |  | Remove all test references to an error string which can no longer be emitted, as demonstrated in the proof posted alongside [af33c2b22c]. Rename a misnamed test file which caused it to be skipped in *.test lists, and which now has two test failures pending resolution. (check-in: 935a693d3a ... user: stephan tags: trunk) | 
| 20:09 |  | Fix a stray comma in the amatch extension which causes a variadic call to do the wrong thing. (check-in: 3430eb99c8 ... user: stephan tags: trunk) | 
| 19:35 |  | Remove dead code from [83fc83e36c906ad4]. See proof sketch in the associated wiki. (check-in: af33c2b22c ... user: drh tags: trunk) | 
| 18:58 |  | Put ALWAYS() around an always-true conditional added by [485b7480fcb80f62]. (check-in: 780b76ef22 ... user: drh tags: trunk) | 
| 18:38 |  | Tests intended for the previous check-in. (check-in: cb262c966d ... user: stephan tags: trunk) | 
| 18:37 |  | When opening a TEMP db fails due to OOM, report it as OOM instead of as a failed open. (check-in: 83fc83e36c ... user: stephan tags: trunk) | 
| 18:35 |  | Fix a potential int overflow in vec1TransformInputVector() in the vec1 extension. (check-in: 8b0edc3031 ... user: stephan tags: trunk) | 
| 18:29 |  | In JSON5, allow an unquoted label to be a JSON keyword or special constant like "Infinity". Enhancement request 2026-11-04T00:54:43Z (check-in: ccd4e7aa97 ... user: drh tags: trunk) | 
| 17:35 |  | Mark some internal-only routines as static in the expert extension. No functional changes. (check-in: 37a7918715 ... user: stephan tags: trunk) | 
| 17:08 |  | OOB write fix in vec1 (PandaSt0rm F6). (check-in: a07c04da57 ... user: stephan tags: pandast0rm-fixes) | 
| 17:08 |  | Ensure that the recovery extension's dbdata vtab is DIRECTONLY so that it may not indirectly, via use in views or triggers, cross boundaries of ATTACHed dbs (PandaSt0rm F5) (check-in: 4c400f5122 ... user: stephan tags: pandast0rm-fixes) | 
| 17:07 |  | Ensure that the expert extension only ever runs a single leading CREATE TABLE statement from its inputs (PandaSt0rm F3). (check-in: 7e6825df99 ... user: stephan tags: pandast0rm-fixes) | 
| 16:58 |  | Fix bugs in "^" and "$" handling of the regexp extension. Also enhance the regexp extension to support "{m,}" (without the "n") as some AIs consider its prior absence to be a bug. Reports 2026-11-06T18:46:42Z and 2026-11-06T18:47:29Z, respectively. (check-in: 2f5b194d9e ... user: drh tags: trunk) | 
| 16:53 |  | Ensure that the collations used by an ORDER BY attached to a compound SELECT are determined correctly in the case where the ORDER BY contains a mix of terms with and without explicit collation sequences. Report 2026-11-04T01:00:55Z. (check-in: 485b7480fc ... user: dan tags: trunk) | 
| 14:42 |  | Have vec1 reject a model with residual=1 and nbucket=0. (check-in: 0fb2d74fe6 ... user: dan tags: trunk) | 
| 12:18 |  | Enhancements to the ".testctrl tune" dot-command in the CLI. (check-in: 447a681116 ... user: drh tags: trunk) | 
| 12:06 |  | New debugging interface, callable from gdb or similar: sqlite3ShowSelectAsSql(). Similar to sqlite3ShowSelect() except that the output is reconstructed SQL text rather than the AST. (check-in: 7919b2227b ... user: drh tags: trunk) | 
| 11:26 |  | Make most tunable parameters in the query planner adjustable using SQLITE_TESTCTRL_TUNE on builds with SQLITE_DEBUG or SQLITE_ENABLE_TUNING. Add the tool/tuningverify.tcl script to verify alignment between the tunable parameter array and Tuning() macros and have makefiles run that script and fail if something is amiss. (check-in: 7c66b6f510 ... user: drh tags: trunk) | 
| 2026-11-15 |  |  | 
| 23:30 |  | Fix documentation that describes what happens when you access a floating-point query result using sqlite3_column_blob(). No changes to code. Bug 2026-11-14T13:16:09Z (check-in: e4704675c8 ... user: drh tags: trunk) | 
| 23:14 |  | Ensure that EXPLAIN and EXPLAIN QUERY PLAN statements are expired after the schema changes out from under them. Forum post 2026-11-15T21:05:06Z (check-in: 81341fb74f ... user: drh tags: trunk) | 
| 00:55 |  | Improve robustness of RBU against SQL injection attackes coming from
 hostile databases.  (Because of the way RBU is intended to be used,
 both databases *should* be trusted, and so this is not a realistic
 attack, but it does not hurt to fix it.)
 Bug fd4e7c9c641d738e item F1  (check-in: ad4ee20509 ...  user: drh tags: trunk) | 
| 00:48 |  | Disable test/fuzz4.test between 00:00Z and 01:59Z, as it gets wrong answer then, and isn't really a very important test. (check-in: 427bf208e8 ... user: drh tags: trunk) | 
| 2026-11-14 |  |  | 
| 23:44 |  | JS preprocessor: consolidate some of the argument interpolation and lay the groundwork for first-class list- and object-type values (JSON). #query bind support was extended significantly. (check-in: cb7b3e3d1a ... user: stephan tags: trunk) | 
| 21:02 |  | Remove an unnecessary branch added by [116a220245]. (check-in: bf5d2f9f26 ... user: dan tags: trunk) | 
| 19:59 |  | Fix a harmless compiler warning added by [116a220245803639]. (check-in: 361598e2d3 ... user: drh tags: trunk) | 
| 19:54 |  | Enhance the .check command of the CLI so that the PATTERN is the concatenation with space separator of all non-option arguments. Also fix a bug that prevented the CLI from compiling with SQLITE_OMIT_LOAD_EXTENSION. (check-in: cf2c3c8f12 ... user: drh tags: trunk) | 
| 19:16 |  | Multiple fixes to the zipfile extension. Bug fd4e7c9c641d738e items F7, F9 (check-in: c383654fd1 ... user: drh tags: trunk) | 
| 18:45 |  | Include the tryeval extension in the CLI, but disable it in safe mode. Also disable extension loading in safe mode, as an added safety. (check-in: d58866448b ... user: drh tags: trunk) | 
| 15:58 |  | Extend the capable(X) function again with capable('unsafe') which is true as long as the CLI was not launched with --safe and the .safe command has not been run. (check-in: 7f9fec6724 ... user: drh tags: trunk) | 
| 15:23 |  | Enhance the capable(X) function in the CLI so that it also looks at available VFSes. So now you can do ".if capable('unix')" or similar in script. (check-in: dad81fcfd6 ... user: drh tags: trunk) | 
| 15:15 |  | When comparing expressions, require that a bound value be the same type and bit-for-bit identical as the value is being compared to for the comparison to succeed, just as is done for literal values. Report 2026-10-28T03:40:05Z. (check-in: 116a220245 ... user: dan tags: trunk) | 
| 14:31 |  | Improved shell escaping on "ssh" commands run by sqlite3_rsync. Note that "~" is now escaped and so it no longer works to specify "~" as part of the pathname for the remote-side database. The same goes for "=", though that has less impact. These are security improvements. Bug fd4e7c9c641d738e item F2 (check-in: 8427482870 ... user: drh tags: trunk) | 
| 13:28 |  | Replace the JS preprocessor's two eval engines with a consolidated one. This changes how non-trivial expressions parse but makes them more intuitive and breaks none of our existing inputs. (check-in: f150cefd0a ... user: stephan tags: trunk) | 
| 12:07 |  | Fix a date/time test involving timezones that was not working on Windows. (check-in: 62e99c1336 ... user: drh tags: trunk) | 
| 11:50 |  | Fix recent test cases so that they run on all configurations. Fix a very minor memory leak in the CLI. (check-in: 6911bdb236 ... user: drh tags: trunk) | 
| 02:31 |  | Fix the dbstat virtual table so that it shows the correct number of unused bytes on a 65536-page-size database with one table that has no content. Bug 2026-11-04T00:51:37Z (check-in: 20a518c187 ... user: drh tags: trunk) | 
| 02:10 |  | Fix the json_error_position() interface so that it gives the correct offset to surplus text after valid JSON. Bug 2026-11-04T00:57:03Z (check-in: 5584a141c4 ... user: drh tags: trunk) | 
| 02:00 |  | The 'utc' time modifier should not cancel 'subsec'. Bug 2026-11-05T22:11:32Z (check-in: 2d7042c18c ... user: drh tags: trunk) | 
| 01:47 |  | Correctly issue an error in an attempt to array-insert against PATH '$', which is never an array element. Bug 2026-11-05T22:16:10Z (check-in: b6f9eb930c ... user: drh tags: trunk) | 
| 2026-11-13 |  |  | 
| 23:59 |  | Improved parsing of unquoted PATH elements in JSON. Bug 2026-11-05T22:16:57Z. (check-in: 23012aa58d ... user: drh tags: trunk) | 
| 21:19 |  | Fix up some corner cases in json_valid(...,8). Bug 2026-11-05T22:17:43Z (check-in: dab2f8735e ... user: drh tags: trunk) | 
| 20:43 |  | Internal cleanups and bug fixes in the JS preprocessor. (check-in: b3ff11d405 ... user: stephan tags: trunk) | 
| 20:14 |  | Avoid applying RealAffinity to a USING column of a FULL JOIN subquery if it is materialized. Report 2026-10-26T12:56:39Z. (check-in: e3336dbb46 ... user: dan tags: trunk) | 
| 20:02 |  | The pragma virtual table mechanism preserves datatype on the pragma argument. Enhancement 2026-11-05T22:20:02Z. (check-in: 88a08083d7 ... user: drh tags: trunk) | 
| 18:53 |  | Make the dbpage virtual table strict about requiring an integer for the page number, not something that can be converted into an integer by looking at the first few characters. Enhancement request 2026-11-05T22:24:41Z. Test case in TH3. (check-in: acba5999eb ... user: drh tags: trunk) | 
| 18:45 |  | Fix the IN-early-out optimization for row-value cases when the NULL bypass skips a required affinity on the LHS. This extends the fix in [eb40248ce6]. Report 2026-11-04T00:58:35Z. (check-in: b3cf602a90 ... user: dan tags: trunk) | 
| 18:06 |  | Minor simplification to the previous in support of testing. (check-in: 159b247964 ... user: drh tags: trunk) | 
| 17:01 |  | When adding a column to a table using ALTER TABLE, do a full scan of the table only if either (a) there is a new CHECK constraint, (b) the new column is NOT NULL and a generated column, or (c) the table is STRICT, the new column is a generated column and the datatype is not ANY. Report 2026-11-12T06:19:49Z. (check-in: ebcb9bb98f ... user: dan tags: trunk) | 
| 16:42 |  | Fix handling of whitespace when decoding base64 in the base64 extension. Bug 2026-11-05T22:30:27Z. (check-in: df5ed9a17f ... user: drh tags: trunk) | 
| 14:49 |  | For a row-value inequality in a WHERE clause (e.g. WHERE (a, b) > (...)), do not incorrectly use a collation sequence attached to the second or subsequent vector element when comparing the first elements together. Report 2026-11-05T22:22:22Z. (check-in: 1aa6415b8e ... user: dan tags: trunk) | 
| 13:49 |  | Fix a problem allowing a SELECT to reuse the materialization of a view even when it was materialized with only a subset of columns populated. Fix for report 2026-11-06T20:21:37Z. (check-in: 5f0a6953de ... user: dan tags: trunk) | 
| 13:39 |  | In the CLI, ".open --deserialize" with no arguments should open an empty in-memory database and not call fopen() with a NULL filename. bug 2026-11-13T12:52:21Z. (check-in: 83388772bd ... user: drh tags: trunk) | 
| 11:37 |  | Fix processing of "x IS TRUE" and similar expressions within DEFAULT clauses. Report 2026-11-05T22:15:24Z. (check-in: 7111503e9c ... user: dan tags: trunk) | 
| 11:19 |  | Fix long-standing problems with the having-to-where optimization identified by reports 2026-10-26T12:58:59Z and 2026-10-27T07:19:03Z. (check-in: 096fafb8e1 ... user: drh tags: trunk) | 
| 08:57 |  | Change the althttpd port number used by ext/wasm/GNUmakefile because it's colliding with local fossil servers. (check-in: b7dbdff6fe ... user: stephan tags: trunk) | 
| 2026-11-12 |  |  | 
| 20:50 |  | Fix a branch that became always true due to [c82b41a9c6623630]. Change it into an assert(). (check-in: 05ab86b918 ... user: drh tags: trunk) | 
| 20:06 |  | Updates to the doc/writing-test-cases.md document. (check-in: 7172271350 ... user: drh tags: trunk) | 
| 19:53 |  | Apply a signed int overflow fix from upstream/downstream Fossil check-in e196b5f8dd4 to the fossildelta extension. (check-in: 6f32df658d ... user: stephan tags: trunk) | 
| 19:18 |  | Fix integrity-check problem causing an invalid corruption report on a table with (a) a VIRTUAL column, (b) an IPK to the right of the VIRTUAL column and (c) a DEFAULT value attached to the right most column. Report 2026-11-05T22:21:35Z. (check-in: c82b41a9c6 ... user: dan tags: trunk) | 
| 18:56 |  | Provide an error message when a ROLLBACK changes content out from under an active incrblob and changes that content into something an incrblob cannot read. (check-in: 48fa9ae1ad ... user: drh tags: trunk) | 
| 16:27 |  | Improved incrblob recovery when the content of the row it is reading is change by ROLLBACK. (check-in: dad5736971 ... user: drh tags: trunk) | 
| 16:22 |  | Avoid integer overflows when calculating DESC RANGE windows. Report 2026-11-04T20:35:05Z. (check-in: 78c82f18e3 ... user: dan tags: trunk) | 
| 15:39 |  | Avoid a potential FP divide-by-zero condition in vec1 training. Report 2026-11-12T08:53:13Z. (check-in: 60cf9357e8 ... user: dan tags: trunk) | 
| 15:19 |  | When processing a "DEFAULT CAST(const AS type)" clause, apply the cast to "type" before the affinity of the column, not after. Report 2026-11-04T20:33:32Z. (check-in: 30bf896242 ... user: dan tags: trunk) | 
| 14:59 |  | Update to AGENTS.md to remind agents not to run two or more testfixtures in the same directory at the same time. (check-in: a1f417e5b5 ... user: drh tags: trunk) | 
| 08:41 |  | Do not use the xfer optimization if there is an INDEXED BY clause attached to the source table. Report 2026-11-04T20:25:01Z. (check-in: 668c6503e8 ... user: dan tags: trunk) | 
| 06:57 |  | Update tests in misc01.sql to account for the stricter ".check" formatting required by [1a00748caf]. (check-in: 255b612e42 ... user: dan tags: trunk) | 
| 06:50 |  | Do not use the xfer optimization if FKs are enabled and the target table is the parent table in an FK constraint. Report 2026-11-04T20:24:15Z. (check-in: 0be7c6e810 ... user: dan tags: trunk) | 
| 02:22 |  | When reading from an incrblob, make sure the underlying row has not changed size and resulted in the offset+amt being too large for the row. (check-in: ba3d401155 ... user: drh tags: trunk) | 
| 00:10 |  | Do not misidentify dot-commands as database files on the command-line parsing of the CLI. Also fix a help typo. (check-in: 3c48444aff ... user: drh tags: trunk) | 
| 2026-11-11 |  |  | 
| 23:27 |  | Add the "posting rules for agents" section to AGENTS.md (check-in: 5fed53b458 ... user: drh tags: trunk) | 
| 23:05 |  | Improvements to the AGENTS.md file. (check-in: d27e488899 ... user: drh tags: trunk) | 
| 20:51 |  | Fix bugs in and improve the interface to the ".check" command of the CLI. (check-in: 1a00748caf ... user: drh tags: trunk) | 
| 19:39 |  | Ensure a statement that uses REPLACE conflict handling uses a statement journal if a later index is on an expression or is a partial index, in case evaluating that expression or WHERE clause raises an error after the conflicting row has been deleted. Report 2026-11-04T20:23:28Z. (check-in: 75757ed3a5 ... user: dan tags: trunk) | 
| 19:11 |  | Ensure a statement is marked as may-abort if it uses an aggregate or window function, or if it reads from a virtual table. Reports 2026-11-04T20:27:20Z and 2026-11-04T20:31:13Z. (check-in: c9c3554918 ... user: dan tags: trunk) | 
| 15:30 |  | Take out ".mode test" added as part of [85e50de017d28abc] since it is really just an alias for ".mode quote". Update recent test cases to use "quote" instead of "test". (check-in: 509c350654 ... user: drh tags: trunk) | 
| 14:58 |  | If an error during a batch-atomic-write (F2FS) commit causes SQLite to fall back to a regular rollback mode commit, ensure that the db file is synced to disk before any data is written to disk. Report 2026-11-10T07:50:48Z. (check-in: 75ad951b1b ... user: dan tags: trunk) | 
| 14:43 |  | Minor typo fix. (check-in: 1e2c11c32d ... user: drh tags: trunk) | 
| 14:38 |  | Add the doc/writing-test-cases.md document. Improvements to AGENTS.md. (check-in: ea4c56756f ... user: drh tags: trunk) | 
| 13:17 |  | Still more hardening against smuggled shell command attacks. (check-in: 8981033053 ... user: drh tags: trunk) | 
| 13:09 |  | A 64-bit WASM fix for [1b7b50a6d601c]. (check-in: d380b494c9 ... user: stephan tags: trunk) | 
| 13:04 |  | Simplify the WASM on-dispose notifications by eliminating the intermediary struct. It turns out that we can overwrite a static function pointer from JS without it. (check-in: 1b7b50a6d6 ... user: stephan tags: trunk) | 
| 12:32 |  | Further strengthening of the new ".safe" command in the CLI. (check-in: d486e99790 ... user: drh tags: trunk) | 
| 11:39 |  | Add the ".safe" command to the CLI and enhance the ".recover" command so that the script it generates includes the ".safe" command near the top. Prior check-in [d684ccc9ec4e41d8] is sufficient to block the attack described by bug 2026-11-06T04:16:54Z and CVE-2026-119586. This check-in simply adds an additional layer of defense. (check-in: f2be7c2a98 ... user: drh tags: trunk) | 
| 03:00 |  | Further minor tweaks to AGENTS.md (check-in: 27f322ba50 ... user: drh tags: trunk) | 
| 02:57 |  | Updates to AGENTS.md based on recent experience. (check-in: e80beb913f ... user: drh tags: trunk) | 
| 01:04 |  | Fix CAST(TO NUMERIC) so that it correctly understand the entire prefix in all circumstances. Bug 2026-11-04T20:34:18Z (check-in: e390985460 ... user: drh tags: trunk) | 
| 00:43 |  | Fix a boundary case in substr(). Bug 2026-11-04T20:19:36Z (check-in: 4ddf1761c0 ... user: drh tags: trunk) | 
| 2026-11-10 |  |  | 
| 23:49 |  | Fix corner-case issues in date/time functions as described by bugs 2026-11-05T22:10:45Z, 2026-11-04T00:49:18Z, and 2026-11-04T00:48:32Z. (check-in: 1cbe4599b0 ... user: drh tags: trunk) | 
| 22:58 |  | Do not allow timezones on a HH:MM:SS offset modifier. (closed check-in: 4b7348afb4 ... user: drh tags: date-time-bugs) | 
| 22:00 |  | Clean up excess commentary in the initial prototype fix. (check-in: 80e3541043 ... user: drh tags: date-time-bugs) | 
| 21:42 |  | In the CLI and the ".mode" command, clamp various parameters at the limit of their range and do not allow integer truncation, as robots seem to not like integer truncation behavior in user interfaces. Bug 2026-11-05T22:26:14Z (check-in: a6250888ec ... user: drh tags: trunk) | 
| 21:10 |  | Enhance QRF to allow columnar modes to wrap at a control character. Bug 2026-11-05T22:27:00Z (check-in: 6982393888 ... user: drh tags: trunk) | 
| 20:46 |  | Fix NULL outputs for the "html" output mode in the CLI. Bug 2026-11-05T22:27:46Z (check-in: f827be82ea ... user: drh tags: trunk) | 
| 20:42 |  | Doc-only updates in the JS/WASM pieces. (check-in: 363e7a9f71 ... user: stephan tags: trunk) | 
| 20:31 |  | When processing immediate FK constraints, do not assume a CASCADE or SET NULL action resolves all constraints if there are BEFORE triggers that will fire on the child table, as a RAISE(IGNORE) may intervene. Report 2026-11-04T20:21:09Z. (check-in: 0c42b4821a ... user: dan tags: trunk) | 
| 19:16 |  | Put an ALWAYS() on an always-true condition in [c41e45677d75bf49]. (check-in: 885008f7ff ... user: drh tags: trunk) | 
| 19:10 |  | Process all immediate FK constraints before all deferred ones. Otherwise, if there is no statement journal and a deferred FK constraint is violated before an immediate one is, the database handle is left in an inconsistent state. Report 2026-11-04T20:20:23Z. (check-in: 43c40c7961 ... user: dan tags: trunk) | 
| 18:38 |  | Within an EXISTS(...) subquery, do not ignore DISTINCT if there is an OFFSET clause. Report 2026-11-04T01:01:41Z. (check-in: c41e45677d ... user: dan tags: trunk) | 
| 17:16 |  | Do not propagate a constant gleaned from a no affinity WHERE clause term to the RHS of another WHERE clause comparison that uses TEXT affinity, just as is already done for the LHS. Report 2026-11-04T01:00:08Z. (check-in: 42fbc72bcf ... user: dan tags: trunk) | 
| 15:54 |  | Ensure that expressions like "x IS (y IN ())" are equivalent to "x IS 0", not "x IS FALSE". Reports 2026-11-10T13:17:50Z and 2026-11-05T22:18:29Z. (check-in: 3b28941b47 ... user: dan tags: trunk) | 
| 15:51 |  | Enhance QRF to do a better job of restricting the title width when there are accented and double-width characters, with and without ellipsis. Bug 2026-11-05T22:29:41Z. (check-in: a3738b9e3d ... user: drh tags: trunk) | 
| 15:09 |  | If the RHS of a vector IN() expression is a sub-query with a LIMIT clause, do not allow the sub-query to be split into multiple separate queries each returning a single column, as such queries might return different rows than the overall query. Report 2026-11-04T00:59:22Z. (check-in: 177d350eb8 ... user: dan tags: trunk) | 
| 14:11 |  | Remove a stray, obsolete bit of Big Endian test code. Forum report 2026-11-05T12:25:12Z. (check-in: b674507ef7 ... user: stephan tags: trunk) | 
| 13:19 |  | Fix another adverse interaction between outer join strength reduction and query flattening, as reported by bug 2026-11-08T01:01:03Z. Also add more test cases for the previous strength-reduction-flattening fix at [8d213d8a1dc2a1b6]. (check-in: 89066ca37c ... user: drh tags: trunk) | 
| 12:52 |  | Fix harmless compiler warning in [ab6f3665f960aa12]. (check-in: 84b0327ef2 ... user: drh tags: trunk) | 
| 12:29 |  | Avoid unnecessary scanning of subqueries in the previous check-in. Move the sqlite3SelectWalkFail() routine from expr.c to walker.c for aesthetics. (check-in: ff612271ea ... user: drh tags: trunk) | 
| 12:21 |  | Fix an adverse interaction between outer-join strength reduction and query flattening. Bug 2026-11-08T07:03:28Z (check-in: 8d213d8a1d ... user: drh tags: trunk) | 
| 11:18 |  | Only intepret "WHERE x=y" as allowing the substitution of "x" for "y" in other parts of the WHERE clause if the collation sequence used for "x=y" is either BINARY or else matches the collation sequence used where the substitution takes place. Report 2026-11-04T00:57:49Z. (check-in: 11076b8c60 ... user: dan tags: trunk) | 
| 10:38 |  | Enhance the TEXT→REAL converter so that it works even for ridiculous AI-generated floating point literals. Forum thread 2026-11-09T04:18:04Z. (check-in: a10f9df37b ... user: drh tags: trunk) | 
| 01:13 |  | Omit some test cases that require virtual tables when virtual tables are not available. (check-in: 2b0e0e3d59 ... user: drh tags: trunk) | 
| 2026-11-09 |  |  | 
| 23:49 |  | Fix duplicate test numbers in a test script. No code changes. (check-in: d84818861d ... user: drh tags: trunk) | 
| 21:20 |  | More commentary on the operation of the capable() function that is built into the CLI. (check-in: 7d156a5fd3 ... user: drh tags: trunk) | 
| 20:49 |  | Avoid truncating declared column types that end in "always". Report 2026-11-04T00:47:45Z. (check-in: ab6f3665f9 ... user: dan tags: trunk) | 
| 18:44 |  | Have the rtree code catch SQLITE_TOOBIG and other errors in sqlite3_bind_blob(). Report 2026-11-03T18:57:10Z. (check-in: 24525ea163 ... user: dan tags: trunk) | 
| 18:13 |  | Ensure that affinities are applied to the excluded.* values in a DO UPDATE clause associated with an IPK confict. Report 2026-11-03T02:17:21Z. (check-in: ed9e465405 ... user: dan tags: trunk) | 
| 17:42 |  | Update README.md and AGENTS.md to try to make it clearer that bug reports should go to the bugs forum and not to the main forum. (check-in: 192e4ee8f8 ... user: drh tags: trunk) | 
| 17:36 |  | When a VALUES clause is part of a compound SELECT, do not ignore collation sequences attached to rows or SELECTs to the right of the first VALUES row. Report 2026-11-03T02:16:35Z. (check-in: 4d8d52d654 ... user: dan tags: trunk) | 
| 01:13 |  | Report surplus entries in a partial index by comparing the number of rows that satisfy the index WHERE clause with the entry count from the b-tree walk. (check-in: 2223f42b79 ... user: lehenbauer tags: integ-ck-partial-idx) | 
| 00:59 |  | Updates to testrunner.tcl so that it runs each test/*.sql script as a separate job. (check-in: 1fea2d9c18 ... user: drh tags: trunk) | 
| 2026-11-08 |  |  | 
| 23:31 |  | On Windows, provide globbing of scripts names to arguments of --runtests. (check-in: b90856289c ... user: drh tags: trunk) | 
| 23:03 |  | Get the --runtests feature of the CLI working on Windows. Also fix several bugs introduced over the previous few CLI check-ins. (check-in: fcef53baca ... user: drh tags: trunk) | 
| 22:38 |  | Add the --runtests option to the CLI. (check-in: 7e4d87f9a6 ... user: drh tags: trunk) | 
| 20:46 |  | A printf format misuse fix for the previous check-in. (check-in: fcfe6f0e08 ... user: stephan tags: trunk) | 
| 20:36 |  | Latest upstream preprocessor to fix missing file/line error info in some contexts. (check-in: 7daccc404e ... user: stephan tags: trunk) | 
| 19:41 |  | JS: internal cleanups and micro-optimizations in the C/JS struct glue. (check-in: 5391095f1c ... user: stephan tags: trunk) | 
| 16:51 |  | Rename CLI tests that require special options from *.sql to *.clitest. Adjust all remaining *.sql tests so that they can all be run together using "sqlite3 test/*.sql". (check-in: 5b9cef877b ... user: drh tags: trunk) | 
| 16:04 |  | Improvements to the CLI to improve testing ability. (1) Add the "test" output mode that works like "list" but uses "," as the column separator and has "-quote on". (2) Add "--any", "--not", and "--regexp" to the ".check" command. (3) Default match algorithm for ".check" treats all whitespace characters as equal to one another. (check-in: 85e50de017 ... user: drh tags: trunk) | 
| 11:19 |  | JS: (re)init each individual WASM heap view on demand, rather than (re)initing them all at the same time. (check-in: be14fa6627 ... user: stephan tags: trunk) | 
| 10:21 |  | Fix an assertion fault in FTS5 error handling. Discovered by dbsqlfuzz. (check-in: e3db01e75a ... user: drh tags: trunk) | 
| 2026-11-07 |  |  | 
| 21:02 |  | Adjustments to test results in one session test. (check-in: eb48769d49 ... user: drh tags: trunk) | 
| 20:33 |  | Prototype fixes for bug reports 7757ac1132, 3cc3a0207e, e0fe679e7c. (check-in: 82ba76b277 ... user: robot-swarm tags: date-time-bugs) | 
| 20:13 |  | Add NEVER() to an unreachable branch. (check-in: 3ff20c6b2d ... user: drh tags: trunk) | 
| 19:56 |  | Have the substr() function correctly handle zero-length blobs. Report 2026-11-04T00:45:36Z. (check-in: dbc2df5e50 ... user: dan tags: trunk) | 
| 19:20 |  | Avoid factoring out constant expressions when replacing a column from the RHS of a LEFT JOIN with a value from a partial index WHERE clause, as the value may sometimes be NULL. Report 2026-11-03T02:14:16Z. (check-in: 668fd87454 ... user: dan tags: trunk) | 
| 18:57 |  | Fix sqlite3ExprAnalyzeAggregate() so that it does not translate expression in subqueries, as a fix for Bug 2026-11-07T18:33:11Z. (check-in: a07a6ca1db ... user: drh tags: trunk) | 
| 18:49 |  | Avoiding using an index expression to evaluate a constant expression, even if the cursor on the index appears to be open and pointing at a non-null row. Report 2026-11-03T02:12:43Z. See also [fee60d93e9]. (check-in: c9cb6e19a8 ... user: dan tags: trunk) | 
| 17:47 |  | Take care not to take the negative of -9223372036854775808 in json decoding, which though it is harmless, is still considered UB. (check-in: 0963ac979e ... user: drh tags: trunk) | 
| 17:17 |  | Bypass recently added test cases involving GLOB comparisons of BLOB values when using -DSQLITE_LIKE_DOESNT_MATCH_BLOBS. (check-in: 0fe4cb1f34 ... user: drh tags: trunk) | 
| 17:01 |  | Test cases for Bug 2026-11-05T22:09:59Z. (check-in: 05a6ebd42e ... user: drh tags: trunk) | 
| 16:28 |  | Ensure trigger programs that use views do not end up with duplicate select-ids in the parse-tree. Report 2026-11-03T02:11:56Z. (check-in: 6240e0daa7 ... user: dan tags: trunk) | 
| 14:45 |  | In aggregate queries, if the RHS of a LEFT JOIN uses an index on an expression, do not assume that the value of that expression is NULL just because the cursor current points to the null-row. Report 2026-11-03T02:11:10Z. (check-in: eb1fc11ad6 ... user: dan tags: trunk) | 
| 13:48 |  | Subtypes should pass through a CASE expression. Forum post 2026-11-07T10:10:46Z. (check-in: bb091549d4 ... user: drh tags: trunk) | 
| 11:41 |  | In the implementation of the CLI, break out the ".eqp" command into a separate dotCmdEqp() function so that it can have better help text. And add the ability to redirect output to a file. (check-in: 707fc799ad ... user: drh tags: trunk) | 
| 09:51 |  | Fix an inaccuracy in the LIKE optimization for the corner case of a UTF-16be database and last character of the pattern before the wildcard being U+07ff. Bug 2026-11-04T20:30:26Z (check-in: de742404bc ... user: drh tags: trunk) | 
| 2026-11-06 |  |  | 
| 22:55 |  | Change the comment on the SQLITE_JSON_MAX_DEPTH compile-time option to make it clear that the limit is approximate. Bug 2026-11-06T20:20:51Z (check-in: 9a42b58170 ... user: drh tags: trunk) | 
| 22:43 |  | Fix an off-by-one error in the documentation for the lower bound of SQLITE_DBCONFIG_FP_DIGITS. No changes to code. Bug 2026-11-06T20:19:18Z (check-in: 58a2965acc ... user: drh tags: trunk) | 
| 21:35 |  | Merge the fix for bug 2026-11-05T22:23:54Z onto turnk. (check-in: 4f6350ccdf ... user: drh tags: trunk) | 
| 21:32 |  | Extra test cases. (closed check-in: 20e640bb63 ... user: drh tags: experimental) | 
| 20:55 |  | Do not substitute a value read from an index on a generated column for an identical expression within the query if the affinities do not match within an aggregate query, just as is already done for a non-aggregate. Report 2026-11-03T01:55:11Z. (check-in: dcaab21165 ... user: dan tags: trunk) | 
| 20:49 |  | Fix a harmless compiler warning in the CLI. (check-in: 3eb5d98627 ... user: drh tags: trunk) | 
| 19:51 |  | In vec1, avoid fixed buffers and 32-bit arithmatic when loading training vectors. Report 2026-11-06T17:58:29Z. (check-in: f38a1d0a51 ... user: dan tags: trunk) | 
| 19:05 |  | Remove some dead code from vec1.c. (check-in: ee94b1c151 ... user: dan tags: trunk) | 
| 18:46 |  | Candidate fix for missing rows from a join against an ordered UNION ALL subquery. Avoid flattening if another FROM term contains a parser-generated coroutine. Add regression coverage for reversed FROM order and nested wrappers. Report 1b06e712af3a3d18. (check-in: f393be41ca ... user: lehenbauer tags: experimental) | 
| 18:40 |  | Handle sqlite3_bind_blob() returning SQLITE_TOOBIG in fts5. This should only be possible with a very small SQLITE_LIMIT_LENGTH configuration. Report 2026-11-03T19:00:39Z. (check-in: 99ffa39b1f ... user: dan tags: trunk) | 
| 18:01 |  | Avoid silently omitting to configure the fts3/4 compress= or uncompress= functions following an OOM error when loading the schema for such an fts3/4 table. Report 2026-10-28T11:05:06Z. (check-in: cf4e4a7de2 ... user: dan tags: trunk) | 
| 17:51 |  | Fix the generate_series() table-valued function such that it works correctly with an ORDER BY rowid DESC LIMIT ← "rowid" instead of "value". Bug 2026-11-05T22:31:13Z (check-in: b05d02e27a ... user: drh tags: trunk) | 
| 17:21 |  | Avoid silently dropping index entries following an OOM within an fts3 'merge' command. Report 2026-10-28T11:00:38Z. (check-in: 89ace0292a ... user: dan tags: trunk) | 
| 16:53 |  | Ensure that OOM conditions in calls to sqlite3changegroup_begin() or sqlite3changegroup_add_xxx() do not leave the changegroup object in a corrupt state. Report 2026-10-28T10:49:09Z. (check-in: 9f8bf750d9 ... user: dan tags: trunk) | 
| 16:27 |  | Do not perform the optimization of combining disjuncts if either side comes from the LIKE optimization, as doing so messes up the LIKE optimization. Bug 2026-11-06T00:25:13Z (check-in: f2ecc3000c ... user: drh tags: trunk) | 
| 16:20 |  | Make fts3/4 more resiliant in the face of errors while loading the extension, whether it is built-in or loaded from a shared object. Report 2026-10-27T14:14:22Z. (check-in: 37afb1e91d ... user: dan tags: trunk) | 
| 14:54 |  | Early out of the sqlite3ProcessJoin() procedure if there is nothing for it to do. Fix for the (harmless) UB noticed in Bug 2026-11-06T14:12:17Z. (check-in: 96bb1b6da5 ... user: drh tags: trunk) | 
| 14:26 |  | Reduce the amount of testing devoted to Device-One to "veryquick" from "all". Omit test/misc01.test if not capable of virtual tables. (check-in: 02d3b2614e ... user: drh tags: trunk) | 
| 13:41 |  | Set the type of the fts5 table-name column to BLOB. This prevents it from begin used by the constant-propagation optimization when a full-text query is posed using "WHERE table-name = ?". Report 2026-10-20T18:15:13Z. (check-in: f21236e74d ... user: dan tags: trunk) | 
| 13:29 |  | Fix a recent change to test/intreal.test so that it works on all test configurations. (check-in: 76faefddf9 ... user: drh tags: trunk) | 
| 13:22 |  | The .recover command now omits extraneous text found at the end of the sqlite_schema.sql field. Bug 2026-11-06T04:16:54Z (check-in: d684ccc9ec ... user: drh tags: trunk) | 
| 12:01 |  | Change an assert() into a testcase(). New test case added to TH3 for coverage. Bug 2026-11-06T00:24:19Z. (check-in: b92950bde7 ... user: drh tags: trunk) | 
| 12:00 |  | JS: numerous internal cleanups and doc/test tweaks. (check-in: 5a62f2b816 ... user: stephan tags: trunk) | 
| 09:30 |  | JS: a couple of hot-path optimizations in the WASM glue and teach wasm.isPtr32() to recognize a full 4gb range instead of 2gb. (check-in: 1eb8bc17c9 ... user: stephan tags: trunk) | 
| 2026-11-05 |  |  | 
| 23:26 |  | Fix overly aggressive recursion depth limitations in json_pretty(). Bug 2026-11-04T00:55:30Z (check-in: 6107f5affe ... user: drh tags: trunk) | 
| 23:19 |  | JS: have wasm.cstrlen() return null, instead of 0, for non-NUL-terminated input and specifically document that behavior as undefined (noting that the whole rest of the heap would have to be non-0 for that to happen). (check-in: 35a22d469f ... user: stephan tags: trunk) | 
| 23:02 |  | JS: speed up wasm.ptrAdd() by eliminating the rest-args and specializing it for the pointer type at bootstrap-time and the common case of 2 args. (check-in: 1c31ff6ace ... user: stephan tags: trunk) | 
| 22:52 |  | JS: speed up automated argument/result conversions by (1) replacing some unnecessary hot-path "rest args" with explicit parameters and (2) calculating which result converter to use when the function is wrapped rather than on each subsequent call. (check-in: e5696e597e ... user: stephan tags: trunk) | 
| 22:24 |  | JS: harden wasm.cstrlen() against a non-NUL-terminated address. Speed up the cleanup phase of scoped allocs by reducing the number of calls into WebAssembly.Table.get(). (check-in: 6f5c9687f6 ... user: stephan tags: trunk) | 
| 21:31 |  | JS: speed up wasm.cstrlen() by way of Uint8Array.indexOf(). (check-in: 62cf9fb51a ... user: stephan tags: trunk) | 
| 21:20 |  | Make the fts4aux virtual table WITHOUT ROWID, so that RIGHT JOIN and OR queries work properly with it. Report 2026-10-26T16:30:43Z. (check-in: ed4535f2a8 ... user: dan tags: trunk) | 
| 20:38 |  | Fix cases where a dual-representation sqlite3_value can cause problems. Bug 2026-11-05T18:08:21Z. (check-in: 83454d6aa8 ... user: drh tags: trunk) | 
| 20:20 |  | Fix a problem in fts4aux where a BLOB upper bound on the term column was being treated as TEXT. Report 2026-10-26T16:29:13Z. (check-in: cc625b9c6a ... user: dan tags: trunk) | 
| 20:10 |  | Add the custom SQL function "dualrep(X)" to the CLI. This function returns its argument X. If the argument is numeric, it returns an sqlite3_value object that contains both the numeric and the text representation. This is useful for testing. (check-in: 17f27736c5 ... user: drh tags: trunk) | 
| 19:35 |  | Prevent fts5 from being used to bypass the trusted_schema=0 setting using the SQLITE_PREPARE_FROM_DDL flag. Report 2026-10-30T23:21:00Z. (check-in: 651828d0e8 ... user: dan tags: trunk) | 
| 19:28 |  | Fix the OP_SeekRowid opcode so that it works even in a debugging build if the key value has both a text and a numeric representation. Bug 2026-11-05T18:09:41Z (check-in: 088c80f5e3 ... user: drh tags: trunk) | 
| 18:49 |  | Fix some integer overflow problems in fts3/4. Report 2026-10-29T14:48:44Z. (check-in: 93579adb8b ... user: dan tags: trunk) | 
| 18:31 |  | Merge the fix for the silly "-0" bug in the decimal extension. (check-in: 824f2d9136 ... user: drh tags: trunk) | 
| 18:23 |  | This time apply the fix to the actual source file instead of to an intermediate build product. (leaf check-in: 5c31d37464 ... user: drh tags: missed-file) | 
| 18:22 |  | Fix a typo in [b5fb67f6f2e61e92].  Tried to fix an obscure UBSAN violation that only affected 32-bit builds and ended up branch fossildelta.c for *all* builds!  (check-in: f1c68c6cb3 ...  user: drh tags: trunk) | 
| 18:03 |  | Fix the decimal extension so that it does not return negative zero. Bug 2026-11-04T20:36:37Z. Oops. I made the edits to "shell.c" instead of "ext/misc/decimal.c". (check-in: 84efb0eece ... user: drh tags: missed-file) | 
| 16:17 |  | Remove the undocumented and unused "vtshim.c" extension module, so as not to confuse LLM robots. (check-in: 75c3621a00 ... user: drh tags: trunk) | 
| 16:10 |  | Fix an OOB write in fts5 that could occur when processing a corrupt database. Report 2026-10-29T14:47:00Z. (check-in: 02e598c973 ... user: dan tags: trunk) | 
| 16:04 |  | Redact hostile column names from comments in the output of the ".lint" command of the CLI. Bug 2026-10-29T14:50:08Z (check-in: 0f19654cf2 ... user: drh tags: trunk) | 
| 15:40 |  | Change how pointer differences are computed in the (experimental) "delta_parse" table-valued function so that it is more robust and portable. Bug 2026-10-29T14:52:00Z (check-in: b5fb67f6f2 ... user: drh tags: trunk) | 
| 15:31 |  | Remove a large chunk of #if'd-out tests of the failed auxdata dtor binding experiment. (check-in: 1b1f7cec2e ... user: stephan tags: trunk) | 
| 15:13 |  | Revise a test case so that it works even on systems with very large filesystem page sizes. Forum thread 2026-10-11T08:31:48Z (check-in: 4c87f040f9 ... user: drh tags: trunk) | 
| 15:12 |  | JS: tester1 pass/fail reporting tweak to reduce eyeball strain during the typical four-tabs-in-parallel test runs. (check-in: 32f5e8f06e ... user: stephan tags: trunk) | 
| 15:05 |  | Use %lld to print a 64-bit quantity in the .import command of the CLI. Forum post 2026-11-05T14:36:54Z. (check-in: 873591876f ... user: drh tags: trunk) | 
| 14:49 |  | JS: internal docs and cleanups. No functional changes. (check-in: 0ab0ac3e7d ... user: stephan tags: trunk) | 
| 14:38 |  | Add test cases for writing to fts3/4 docid and rowid columns. (check-in: 701617b200 ... user: dan tags: trunk) | 
| 13:00 |  | Include the sNameToken field of Parse in the reset-after-recursion section in order to avoid a harmless UBSAN warning. (check-in: 42c751c001 ... user: drh tags: trunk) | 
| 11:26 |  | Abort any fts5 scans underway when a transaction is explicitly rolled back by the same connection, just as we already do if a savepoint is folled back. Report 2026-11-05T06:49:24Z. (check-in: 6818f1bc8a ... user: dan tags: trunk) | 
| 11:23 |  | Merge all branch-3.54 fixes into trunk. (check-in: 132faed5eb ... user: drh tags: trunk) | 
| 11:22 |  | Fix an off-by-one error in an assert(). Harmless. Bug 2026-11-04T20:21:55Z (check-in: f1d2bab29a ... user: drh tags: trunk) | 
| 11:20 |  | Binding-API doc correction reported in post 2026-11-05T10:06:51Z. (check-in: 282be17434 ... user: stephan tags: branch-3.54) | 
| 11:13 |  | Fix the json_each() and json_tree() table-valued functions to respond better to malformed JSONB. Bug 2026-11-04T20:25:48Z (check-in: 2ac472602f ... user: drh tags: trunk) | 
| 11:00 |  | The ".mode --reset" command in the CLI should not change the ".echo" mode. (check-in: 5caff00888 ... user: drh tags: trunk) | 
| 10:43 |  | Do not run the WHERE_IN_SEEKSCAN optimization and the WHERE_BIGNULL_SORT optimization at the same time. We don't know that this causes problems, but neither do we know for certain that it is safe, and the need for both is exceedingly rare, so why risk it. Report 2026-11-04T20:28:07Z. (check-in: 56a5b1b9ae ... user: drh tags: trunk) | 
| 10:06 |  | Set SQLITE_ENABLE_BYTECODE_VTAB for CLI builds always, even when using --dev and building sqlite3d. (check-in: 91f4b150f2 ... user: drh tags: trunk) | 
| 00:01 |  | Fix a harmless assertion fault caused by [f43294a5582b540a]. Bug 2026-11-04T20:28:53Z (check-in: 8a675a4b2b ... user: drh tags: trunk) | 
| 2026-11-04 |  |  | 
| 21:37 |  | Enhance the JSON interface so that it will accept integer literals larger than what will fit in a 64-bit signed integer. When asked to return their value, it returns the nearest floating-point equivalent. Enhancement 2026-11-04T00:53:57Z and 2026-11-04T00:56:16Z. (check-in: 13abbb1d72 ... user: drh tags: trunk) | 
| 21:13 |  | Change the way demo file test_delete.c uses the SQLITE_OS_WIN macro so that it can be more easily included in the amalgamation. (check-in: 7451725df4 ... user: dan tags: trunk) | 
| 21:06 |  | Fix .fullschema --indent being ignored. Bug report 2026-11-04T20:35:51Z. (check-in: ad609aa905 ... user: stephan tags: trunk) | 
| 19:23 |  | Put ALWAYS() around two recently added conditionals that cannot be FALSE as far as we can tell. (check-in: 7a751f226c ... user: drh tags: trunk) | 
| 19:11 |  | Do not allow ALTER TABLE to add columns with CHECK constraints that use RAISE() expressions. Report 2026-11-04T00:52:24Z. (check-in: 3cbb575c6e ... user: dan tags: trunk) | 
| 17:54 |  | Reintroduce the patch from [e1b09510df4860d3] that got clobbered by the merge at [65091cea03b9a664]. (check-in: 394290b7ce ... user: drh tags: trunk) | 
| 17:49 |  | JS internal doc tweaks. No code changes. (check-in: 72928416b2 ... user: stephan tags: trunk) | 
| 17:13 |  | Do not allow ALTER TABLE to add a NOT NULL column with a default expression that is not a literal NULL but evaluates to NULL to a non-empty table. Report 2026-11-04T00:44:49Z. (check-in: ef7231eb4d ... user: dan tags: trunk) | 
| 17:06 |  | JS: minor Jaccwabyt internal tweaks. Rewrite the Jaccwabyt docs entirely. (check-in: 34dee6fdac ... user: stephan tags: trunk) | 
| 16:24 |  | Fix a problem with ALTER TABLE in cases where an identifier in a view or trigger's ORDER BY clause matches a column being renamed and an alias generated by expanding a "*" within the query. Report 2026-11-03T01:27:10Z. (check-in: 0c0b346b86 ... user: dan tags: trunk) | 
| 14:14 |  | When multiple *.sql files are specific on the CLI command-line, do not stop if one of them fails, unless the --bail flag is used. This facilities running a whole batch of test scripts all in one command. (check-in: c28afd7acd ... user: drh tags: trunk) | 
| 14:03 |  | Add an int cast to a variadic argument in the shell. bug report 2026-11-04T13:36:13Z. (check-in: bcdafc082f ... user: stephan tags: trunk) | 
| 13:45 |  | Use box-drawing characters to highlight the "Expected" and "Got" output from failed ".check" lines in the CLI, for improved debug support. (check-in: 7f29d38362 ... user: drh tags: trunk) | 
| 13:22 |  | Reverse the order of two conditions in a single "if()" to help make it more easily testable. No change to functionality. (check-in: 5cdecca19c ... user: drh tags: trunk) | 
| 11:04 |  | Fix some cases in self-referencing FK processing. Report 2026-11-03T02:15:48Z. (check-in: 8cddae53c7 ... user: dan tags: trunk) | 
| 10:59 |  | A large integer that has no exact floating point representation ought not be converted to floating point when comparing aainst a REAL value. Bug 2026-11-03T02:13:29Z (check-in: d0770f6f56 ... user: drh tags: trunk) | 
| 2026-11-03 |  |  | 
| 20:30 |  | Merge branch-3.54 fixes into trunk. (check-in: 78cf22906e ... user: dan tags: trunk) | 
| 20:27 |  | Fix foreign key processing for the case where a row is its own parent and the parent key uses a non-binary collation. Report 2026-11-03T02:15:02Z. (check-in: 44727d6b79 ... user: dan tags: trunk) | 
| 19:30 |  | Fix corruption of a file-backed temporary database caused by committing a transaction that modifies zero pages when the cache contains dirty pages from a previous transaction. Report 2026-11-03T02:18:54Z. (check-in: d6730aed3d ... user: dan tags: trunk) | 
| 17:47 |  | Fix a problem with renaming a table using ALTER TABLE when that table is part of an "expr IN tbl" expression within the database schema. Report 2026-11-03T02:20:27Z. (check-in: 0c085216b3 ... user: dan tags: trunk) | 
| 17:10 |  | Ensure that ALTER TABLE may be used with a schema that includes one or more UPDATE or DELETE trigger steps that attach an alias to the table being modified. Report 2026-11-03T02:19:41Z. (check-in: d048dd07af ... user: dan tags: trunk) | 
| 16:42 |  | Fix a problem with automatic indexes on WITHOUT ROWID tables where at least one field of the PRIMARY KEY is its 63rd or later column. Report 2026-11-03T02:21:13Z. (check-in: 07b5596901 ... user: dan tags: trunk) | 
| 15:24 |  | Ensure that all automatic-indexes constructed on WITHOUT ROWID tables always include all PK columns, sorted using either BINARY or the same collation sequence as used by the table's PRIMARY KEY index. Report 2026-11-03T02:22:00Z. (check-in: 6b81ddfd06 ... user: dan tags: trunk) | 
| 13:09 |  | Shell: cast literal zeroes in int-pointer variadic arg slots to int-pointer for arg type compliance. Forum post 2026-11-03T12:59:51Z. (check-in: b0b3ca1ca9 ... user: stephan tags: trunk) | 
| 11:56 |  | Fix ORDER BY handling for the case where the FROM clause of the SELECT contains a single subquery that is sorted by the same expressions as the outer SELECT, but using different collation sequences. Report 2026-11-03T02:22:46Z. (check-in: 2d7171370a ... user: dan tags: branch-3.54) | 
| 11:12 |  | Always cast the second argument of SQLITE_CONFIG_LOG to (void*) to avoid harmless UB warnings. Bug 2026-11-03T09:43:08Z (check-in: 7b0be56e72 ... user: drh tags: trunk) | 
| 2026-11-02 |  |  | 
| 20:14 |  | Return SQLITE_LOCKED if an ALTER TABLE DROP COLUMN statement is attempted when there are other statements belonging to the same connection reading the database. Report 2026-10-18T20:41:32Z. (check-in: efbbc90f18 ... user: dan tags: trunk) | 
| 20:11 |  | Return SQLITE_LOCKED if an ALTER TABLE DROP COLUMN statement is attempted when there are other statements belonging to the same connection reading the database. Report 2026-10-18T20:41:32Z. (closed check-in: cb74c4c2c6 ... user: dan tags: drop-column-lock) | 
| 19:10 |  | Minor JS simplification. (check-in: 486ed05e1d ... user: stephan tags: trunk) | 
| 17:57 |  | On Haiku OS have testrunner.tcl behave as it does for Linux. Tests fail because the --with-tclsh needed to build it there is not propagated into the sub-configures, but that's a separate problem to solve. (check-in: ad29055783 ... user: stephan tags: trunk) | 
| 17:43 |  | Minor changes to the abs() and round() SQL functions to better deal with negative zeros. Bug 2026-11-02T08:17:41Z (check-in: 439cec35a8 ... user: drh tags: trunk) | 
| 16:05 |  | Do not allow sqlite3-wasm.c to build against an sqlite3.c older than 3.55.0 because it would silently be missing the new cleanup hooks so would leak like a sieve. (check-in: 290b12a97e ... user: stephan tags: trunk) | 
| 15:34 |  | Remove superfluous 'utf-8' argument from TextEncoder() constructor calls. (check-in: 193386b8a2 ... user: stephan tags: trunk) | 
| 15:21 |  | JS doc updates. Fix a minor doc-vs-impl descrepancy. (check-in: 76ba9c48e1 ... user: stephan tags: trunk) | 
| 14:55 |  | JS API doc updates. (check-in: 576e270963 ... user: stephan tags: trunk) | 
| 13:24 |  | Internal tweaks to the oo1 db/stmt disposal and add tests demonstrating that shared-access db/stmt handles behave well when their rugs are pulled out from under them. (check-in: b964875134 ... user: stephan tags: trunk) | 
| 12:58 |  | Minor test code additions. (check-in: 7e08e4bb7b ... user: stephan tags: trunk) | 
| 12:12 |  | Strip the sqlite3_context support from SQLITE_DISPOSAL_NOTIFIER because those pointers are not stable enough to be of any use for this purpose. They are, by happy accident, in two cases but are not guaranteed to be and are not so in other cases. (check-in: ead354645d ... user: stephan tags: trunk) | 
| 11:52 |  | Avoid errors that sometimes occur when running ALTER TABLE or VACUUM when there are tables in the TEMP database with the same names as tables in the database being altered or vacuumed. Report 2026-10-29T11:55:37Z. (check-in: c4b02a964b ... user: dan tags: trunk) | 
| 10:29 |  | Add a few sqlite3_js_on_pointer_dispose() tests. Re-indent a large block of test code. (check-in: 4fc4469e44 ... user: stephan tags: trunk) | 
| 10:09 |  | Doc typos and cosmetic code cleanups. Remove some dead code. Replace some stray console.error() calls with sqlite3.config.error(). Update some doc links in module-symbols.html. (check-in: e2daa79790 ... user: stephan tags: trunk) | 
| 09:44 |  | Teach sqlite3_js_on_pointer_dispose() to accept a WASM function pointer for a callback. (check-in: 8c5908f428 ... user: stephan tags: trunk) | 
| 01:20 |  | Fix sqlite3_carray_bind() with SQLITE_TRANSIENT so that it correctly differentiates between a NULL and a zero-length BLOB. Bug 2026-11-01T12:04:38Z (check-in: d6ba38e5fe ... user: drh tags: trunk) | 
| 2026-11-01 |  |  | 
| 17:49 |  | Enhance the tables_used() table-valued function so that reports tables and indexes deleted by DROP TABLE and DROP INDEX and clear using the truncate optimization. Bug 2026-11-01T11:57:59Z. (check-in: ceca12f86f ... user: drh tags: trunk) | 
| 16:01 |  | Improved handling of empty filenames in the writefile() function that is part of the fileio.c extension. (check-in: 24b4184a57 ... user: drh tags: trunk) | 
| 11:38 |  | Ensure that the database schema has been read prior to generating a hex dump of a database file using ".dbtotxt" in the CLI. (check-in: 7bed0b36e4 ... user: drh tags: trunk) | 
| 10:46 |  | Fix a slightly incorrect assert() in [afa137dbeb020fae]. (check-in: 4cac619f00 ... user: drh tags: trunk) | 
| 10:24 |  | Restructure some test code. (check-in: 27f1ba7eaa ... user: stephan tags: trunk) | 
| 10:13 |  | Doc tweaks and test improvements. (check-in: 3c0f79c2e8 ... user: stephan tags: trunk) | 
| 09:44 |  | Get sqlite3_js_on_pointer_dispose() working and tested. (check-in: 4088abcb25 ... user: stephan tags: trunk) | 
| 00:42 |  | Add the experimental and as-yet untested sqlite3_js_on_pointer_dispose() as a way for JS clients to clean up data mapped to native pointers when the underlying native objects are destroyed by the library. (check-in: ca551ede9c ... user: stephan tags: trunk) | 
| 2026-10-31 |  |  | 
| 23:17 |  | Test doc touchups. (check-in: bda1217086 ... user: stephan tags: trunk) | 
| 22:44 |  | Add another sqlite3_set_auxdata() conversion test. Document why that option is (and should remain) disabled and leave #if'd out as food for thought later. (check-in: ec117fe0fd ... user: stephan tags: trunk) | 
| 21:57 |  | Add SQLITE_DISPOSAL_NOTIFIER call for sqlite3_context. Experimentally use that to provide sane lifetime mappings for sqlite3_set_auxdata() dtor conversions. That seems to work fine, with predictable cost, but is currently disabled because it needs much more testing. (check-in: 4139b9f0c4 ... user: stephan tags: trunk) | 
| 20:26 |  | Add a the SQLITE_DISPOSAL_NOTIFIER internal-use hook to alert callers when certain objects are finalized. This allows the WASM builds to better clean up associated JS-side state. e.g. passing an oo1.DB instance to sqlite3_close_v2() is now well-behaved, disconnecting any oo1.DB instances which share that pointer and any DB.Stmt objects still opened by those. (check-in: 055fdd4709 ... user: stephan tags: trunk) | 
| 20:14 |  | Flesh out the documentation for the SQLITE_DISPOSAL_NOTIFIER (internal) interface. (closed check-in: cd96471ae0 ... user: stephan tags: disposal-notification) | 
| 20:12 |  | Fix an ALTER TABLE problem with renaming a temp table when there is a temp trigger defined on a main db table of the same name. Report 2026-10-27T20:00:22Z. (check-in: f7a9970ec1 ... user: dan tags: trunk) | 
| 20:08 |  | Fix the display of current optimizations in the CLI. (check-in: 8d891830dd ... user: drh tags: trunk) | 
| 19:47 |  | Consolidate some duplicate code. (check-in: 0a13364732 ... user: stephan tags: disposal-notification) | 
| 19:30 |  | Fix a typo in the previous checkin which caused a corner case to not clean up properly. (check-in: d58903a30e ... user: stephan tags: disposal-notification) | 
| 19:26 |  | Fix deregistration of shared-handle DB/Stmt instances. Add tests demonstrating that closing the prime handle disassociates other oo1-level objects which share that handle. Simplify the by-typeId dispatching of being-disposed pointers. (check-in: c15dc8027f ... user: stephan tags: disposal-notification) | 
| 18:36 |  | Hook oo1.DB and DB.Stmt into SQLITE_DISPOSAL_NOTIFIER, such that passing an instance to sqlite3_close_v2() resp. sqlite3_finalize() will cleanly unplug the oo1 instance from its native. (check-in: 6c8982f153 ... user: stephan tags: disposal-notification) | 
| 17:55 |  | Expand the optimization disable mask from 32 to 64 bits. Add a new bit to disable the having-to-where optimization, for testing purposes. (check-in: 89ea662676 ... user: drh tags: trunk) | 
| 17:48 |  | When renaming a non-temp table using ALTER TABLE RENAME TO, do not also modify references within temp triggers to a temp table of the same name. Report 2026-10-27T19:51:14Z. (check-in: 6488dbedd1 ... user: dan tags: trunk) | 
| 16:47 |  | JS: remove the hand-written sqlite3_close_v2() binding, moving its cleanup work into the new disposal notification. This makes it safe and easy to map arbitrary JS state to dbs and stmts with the certainty that any mappings can be deleted regardless of how the C counterparts are disposed. (check-in: 6e21a7b4cd ... user: stephan tags: disposal-notification) | 
| 16:26 |  | Fix ALTER TABLE RENAME COLUMN statements to correctly rename column references that appear in ON clauses that are part of triggers and views. Report 2026-10-27T19:54:24Z. (check-in: 9138604750 ... user: dan tags: trunk) | 
| 15:12 |  | Make the "-DevTest" optimization flag available for testing purposes. Don't actually use it for anything permanent. Just have it available for use during debugging builds, in enable/disable features for comparison. (check-in: 439a702e5c ... user: drh tags: trunk) | 
| 14:25 |  | Generic cleanups. Add a missing finalizer call in the new test code. (check-in: 727d1886f6 ... user: stephan tags: disposal-notification) | 
| 14:08 |  | Remove a couple of unnecessary levels of indirection. Add some comments explaining what's going on JS-side. (check-in: 724b7da264 ... user: stephan tags: disposal-notification) | 
| 13:27 |  | A proof-of-concept for a compile-time-provided hook to notify when the library disposes of certain memory. The use case is providing the JS bindings with a way to unmap native-to-JS bindings when a db, statement, etc. are finalized in some way other than the JS-side bindings which do this work. (check-in: 50785fb6cb ... user: stephan tags: disposal-notification) | 
| 13:06 |  | Back out part of [a7f314264fc98f50] which turns out to have been ill-advised. (check-in: c80f9edb34 ... user: stephan tags: trunk) | 
| 10:45 |  | Preprocessor-strip approximately 4k of JS we don't need client-side. (check-in: 7da6c2681e ... user: stephan tags: trunk) | 
| 10:31 |  | JS: add some docs explaining the problem the Module.instantiateWasm() override solves. (check-in: a7f314264f ... user: stephan tags: trunk) | 
| 07:43 |  | Merge branch-3.54 into trunk and resolve merge conflicts in the JS pieces. (check-in: 65091cea03 ... user: stephan tags: trunk) | 
| 07:35 |  | Handle ALTER TABLE ADD COLUMN statements when the new column definition ends with a comment. Report 2026-10-27T19:49:19Z. (check-in: 7ae2d4d1e4 ... user: dan tags: trunk) | 
| 06:38 |  | Have ALTER TABLE ADD CHECK handle the case where input ends with an unterminated C-style comment. e.g. "ALTER TABLE t1 ADD CHECK (x!=0) /* unterminated". Report 2026-10-27T19:57:50Z. (check-in: 64f9e76132 ... user: dan tags: trunk) | 
| 02:36 |  | Fix RCEs so that if the self-reference table in the recursive section is on the nullable size of an outer join, it gets the correct answer. Test cases are in TH3. Bug 2026-10-26T12:56:29Z. Other systems simple do not allow this. Claude tells me that the SQL standard does not allow it. I'm not sure why anyone would want to do it. But in any event, I believe it is now working as of this patch. (check-in: 36b78a7509 ... user: drh tags: trunk) | 
| 00:22 |  | Fix the REGEXP extension so that "\|" has higher precedence than "^". Bug 2026-10-27T07:20:52Z (check-in: c6399317f0 ... user: drh tags: trunk) | 
| 00:01 |  | Improved formatting of quirky column names in ".mode line" in QRF. Bug 2026-10-27T07:26:52Z (check-in: 164b284fab ... user: drh tags: trunk) | 
| 2026-10-30 |  |  | 
| 23:32 |  | In the ext/misc/csv.c extension, if the last row of CSV input is incomplete, fill it out with empty strings rather than NULLs. Bug 2026-10-28T05:07:03Z (check-in: fc7692c39b ... user: drh tags: trunk) | 
| 23:08 |  | When the zipfile extension writes a file with a timestamp before 1980-01-01T00:00:01, set the DOS timestamp to that minimum value so that can be displayed on Windows11, which appears to ignore the 0x5455 extended timestamp. Bug 2026-10-27T07:28:08Z (check-in: 8c3db88586 ... user: drh tags: trunk) | 
| 20:32 |  | Fix ALTER TABLE so that it works when a trigger contains an UPSERT with more than one ON CONFLICT clause. Report 2026-10-27T19:46:34Z. (check-in: 3c038169f7 ... user: dan tags: trunk) | 
| 19:22 |  | Simplify an ESM export construct. (check-in: 78d9fd1175 ... user: stephan tags: trunk) | 
| 19:14 |  | Update JS README.md to more closely reflect current reality. (check-in: 244db4ef9b ... user: stephan tags: trunk) | 
| 18:56 |  | JS internal docs and filtering out of state not needed in bundler-friendly builds. (check-in: 02a05ed7f0 ... user: stephan tags: trunk) | 
| 18:35 |  | Update RBU to correctly handle short-rows with non-NULL default values. Report 2026-10-20T03:22:29Z. (check-in: 2deabdd391 ... user: dan tags: trunk) | 
| 18:15 |  | JS internal doc modernization and removal of unnecessary globalThis references. (check-in: 984d7f0e57 ... user: stephan tags: trunk) | 
| 17:42 |  | Rename one of the JS files for consistency. (check-in: 47ccd1b823 ... user: stephan tags: trunk) | 
| 16:36 |  | Small performance optimization and size reduction to covering index detection. (check-in: 7b45acf8ca ... user: drh tags: trunk) | 
| 16:31 |  | JS: remove another superfluous globalThis reference. Internal doc tweaks. (check-in: 66dfdcd648 ... user: stephan tags: trunk) | 
| 16:21 |  | Improvements to the detection of partial indexes working as covering indexes. Enhancement request 2026-10-30T13:03:10Z (check-in: 2c20240ad2 ... user: drh tags: trunk) | 
| 15:51 |  | Add a missing preprocessor define which caused some JS pieces to inject into the global scope instead of file scope. (check-in: aa7f7af405 ... user: stephan tags: trunk) | 
| 15:46 |  | Rework sqlite3Worker1Promiser() to not need the globalThis binding. We still add that binding in non-ESM builds because docs have historically referred to it, but ESM builds have never liked that. (check-in: e0c59098f5 ... user: stephan tags: trunk) | 
| 15:09 |  | JS: rework sqlite3APIBootstrap() to avoid attaching it to the globalThis object. (check-in: acc2425a6c ... user: stephan tags: trunk) | 
| 11:13 |  | Optimization: Do not parse or record trigger definitions on a read-only database. (check-in: afa137dbeb ... user: drh tags: trunk) | 
| 2026-10-29 |  |  | 
| 23:25 |  | Enhance the recover extension so that it uses alternative names for the rowid if its first choices are used for ordinary columns. Bug 2026-10-20T02:57:35Z (check-in: ee05abbb91 ... user: drh tags: trunk) | 
| 23:20 |  | A micro-optimization in the JS 'string:static' arg converter and improve associated tests. (check-in: 1688100029 ... user: stephan tags: trunk) | 
| 22:55 |  | Fix the recover extension so that it understands that INTEGER PRIMARY KEY DESC is not the same as the rowid. Bug 2026-10-20T02:57:07Z (check-in: d8c729e0f5 ... user: drh tags: trunk) | 
| 20:33 |  | JS: remove an outdated test warning and refine the sqlite3_set_auxdata() tests. (check-in: e6c2b84f26 ... user: stephan tags: trunk) | 
| 20:20 |  | In the Expert extension, correctly quote names that begin with a digit. Bug 2026-10-28T05:10:06Z (check-in: 5b2cb9fd21 ... user: drh tags: trunk) | 
| 19:56 |  | Fix a possible infinite loop associated with the nocase collation in the (disused) spellfix extension. Bug 2026-10-29T11:59:57Z (check-in: 5b1155422b ... user: drh tags: trunk) | 
| 19:34 |  | Do not let a TEMP table or an uninitialized schema prevent unauthorized writes to a shadow table. Bug 2026-10-29T15:45:39Z (check-in: ffec289af9 ... user: drh tags: trunk) | 
| 15:30 |  | CSS tweaks to the main JS test apps. (check-in: fd28dabb03 ... user: stephan tags: trunk) | 
| 15:24 |  | Fix RENAME COLUMN on an IPK column that is also part of a UNIQUE constraint. Report 2026-10-23T03:08:04Z. (check-in: b8be307fea ... user: dan tags: trunk) | 
| 14:04 |  | Correct two JS export syntax errors which are not hit in current builds but would eventually have been. (check-in: 661501bbc6 ... user: stephan tags: trunk) | 
| 13:32 |  | In the CLI, do parameter binding before running the EXPLAIN specified by the ".eqp on" setting, not afterwards, as binding can change the bytecode. Also, if vdbe_trace is engaged, print a line for each parameter bound. (check-in: a63cb048d4 ... user: drh tags: trunk) | 
| 13:23 |  | JS: rework how the low-level glue pieces are integrated so that they (A) do not need to temporarily pollute the global namespace during bootstrapping and (B) should no longer upset bundler tools. (check-in: 376184c7b0 ... user: stephan tags: trunk) | 
| 12:44 |  | JS: replace the generated versions of the upstream jaccwabyt/whwasmutil pieces with their original preprocessor-ready forms so that they can integrate cleanly here with vanilla-vs-esm. (check-in: 213d76c009 ... user: stephan tags: trunk) | 
| 12:02 |  | Omit code in generateOutputSubroutine() that is dead due to [659805b27cef273d] when not using SQLITE_ENABLE_UPDATE_DELETE_LIMIT. (check-in: 25ff0ad78e ... user: drh tags: trunk) | 
| 11:19 |  | Add the SQLITE_DBCONFIG_VDBE_TRACE as an alternative to "PRAGMA vdbe_trace" and use it to enable/disable tracing in the CLI, for a smoother debugging experience. (check-in: 345c0ddb81 ... user: drh tags: trunk) | 
| 11:09 |  | Ensure that all operations in a compound SELECT use the correct collation sequence, even when the rightmost operator is a UNION ALL. Report 2026-10-21T19:20:14Z. (check-in: 659805b27c ... user: dan tags: trunk) | 
| 10:04 |  | Apply the treatment from [8e1c88aa41] to another JS file. (check-in: 467b9f5715 ... user: stephan tags: branch-3.54) | 
| 08:51 |  | JS: remove all references to globalThis from the ESM builds of worker1-promiser, as they confuse bundler tools and are only necessary in the non-ESM builds. Per request from npm package maintainer Jure Rotar. (check-in: 8e1c88aa41 ... user: stephan tags: branch-3.54) | 
| 01:36 |  | Fixes to the LIKE optimization to catch new corner cases where it cannot be applied if the LHS operand does not have TEXT affinity. Bug 2026-10-28T05:04:51Z (check-in: d62cdafdcb ... user: drh tags: trunk) | 
| 2026-10-28 |  |  | 
| 19:23 |  | Add commented-out code that *would* support +NaN and -NaN literals in JSON were we to choose to support that syntax. (check-in: 170bf1b4f2 ... user: drh tags: trunk) | 
| 19:10 |  | Fix RBU so that it works with ANY columns in STRICT tables. Report 2026-10-18T20:39:51Z. (check-in: 2fbe79d1f1 ... user: dan tags: trunk) | 
| 00:20 |  | Fix the log(B,N) function so that it works for 0.0<B<1.0. Bug 2026-10-26T16:32:25Z (check-in: 994f05ccd1 ... user: drh tags: trunk) | 
| 00:00 |  | When aggregate content passes through the sorter to implement GROUP BY it loses its subtype. So do not pass JSON values from an index on an expression through the sorter and into a JSON aggregate, as that optimization will not work. Bug 2026-10-27T20:02:01Z (check-in: 9887cc5234 ... user: drh tags: trunk) | 
| 2026-10-27 |  |  | 
| 18:46 |  | Improve generate_series so that it handles constraints that compare the "value" output against a string or blob. Bug 2026-10-26T12:55:22Z (check-in: 6b66b52ab2 ... user: drh tags: trunk) | 
| 18:26 |  | Fix a problem introduced by [08e979525ff1eef8] and reported by a follow-up comment to the original bug report. (check-in: 6be81afa66 ... user: drh tags: trunk) | 
| 16:24 |  | Remove an ALWAYS() in the query planner that can be false for a truly ridiculous 350K+ character input query. Bug 2026-10-26T18:22:15Z. (check-in: c5f98a311e ... user: drh tags: trunk) | 
| 15:19 |  | Fix a problem with vector NOT IN expressions that use an index with the leftmost column declared DESC. Report 2026-10-26T12:54:00Z. (check-in: e1b09510df ... user: dan tags: branch-3.54) | 
| 15:11 |  | Merge branch-3.54 fixes into trunk (check-in: 1193264901 ... user: drh tags: trunk) | 
| 14:58 |  | Prevent application-defined functions that invoke DDL statement or perform other mischief from deleting the schema out from under the statement that called the function. Bug 2026-10-25T19:26:00Z (check-in: 5410f7696b ... user: drh tags: trunk) | 
| 14:37 |  | Fix a typo in sqlite3expert.c that could lead to a heap buffer overwrite in some cases. Report 2026-10-27T14:08:33Z. (check-in: 0273cbefd4 ... user: dan tags: branch-3.54) | 
| 14:28 |  | Remove DIRECTONLY markings from the tryeval() and expire() functions. (closed check-in: f4efdfa6ff ... user: drh tags: schema-reset-appfunc) | 
| 12:18 |  | Add the SQLITE_FILECTRL_EXPIRE_STMTS file-control to facilitate testing. (check-in: fc75988f75 ... user: drh tags: schema-reset-appfunc) | 
| 11:08 |  | Disable the "<expr> IN ('const')" to "expr = 'const'" optimization when 'const' has a collation sequence. Bug 2026-10-26T04:25:22Z. (check-in: 46da9e0212 ... user: dan tags: branch-3.54) | 
| 10:07 |  | Expire every prepared statement that uses a schema when that schema is cleared. In the VDBE, do not access p4.pTab or p4.pIdx fields if the prepared statement is expired. (check-in: 6ca068cdce ... user: drh tags: schema-reset-appfunc) | 
| 09:55 |  | Revert the part of [341525ecfdd4] which throws for a short digest read, and simply treat that as a digest mismatch as we historically have. (check-in: a4951df233 ... user: stephan tags: branch-3.54) | 
| 09:49 |  | Rename an internal JS API for clarity. Replace a == from the previous checkin with ===. (check-in: 77debe0314 ... user: stephan tags: branch-3.54) | 
| 09:41 |  | JS: teach the SAHPool VFS to be able to recover the data part of a handle when the JS context is killed between the two writes of its header part. Bug report 2026-10-27T08:42:50Z. (check-in: 341525ecfd ... user: stephan tags: branch-3.54) | 
| 2026-10-26 |  |  | 
| 23:21 |  | New test cases for schema resets. (check-in: b694f96bf8 ... user: drh tags: schema-reset-appfunc) | 
| 22:53 |  | Fix missing prototype for sqlite3DbMaskUnion() (check-in: 7c2ea69839 ... user: drh tags: schema-reset-appfunc) | 
| 21:07 |  | A more accurate approach to resetting schemas on active statements. (check-in: 031a3911c3 ... user: drh tags: schema-reset-appfunc) | 
| 20:07 |  | Be very permissive for schema resets following a schema cookie mismatch. (check-in: bfb2c110a9 ... user: drh tags: schema-reset-appfunc) | 
| 20:05 |  | Fix test script error causing failures in sessionfault2.test for SQLITE_DEBUG builds. (check-in: 5146cf4b3e ... user: dan tags: branch-3.54) | 
| 19:51 |  | More precision in controlling when to reset schemas. (check-in: 717b1b90c5 ... user: drh tags: schema-reset-appfunc) | 
| 19:43 |  | Avoid leaving a transaction open in the session module when a changeset applied in no-savepoint mode fails because an UPDATE hits a constraint. Report 2026-10-21T02:23:36Z. (check-in: 46d3df1361 ... user: dan tags: trunk) | 
| 17:49 |  | Have vec1 discard its in-memory model cache when a savepoint is rolled back. Report 2026-10-26T11:36:53Z. (check-in: 46fb620071 ... user: dan tags: branch-3.54) | 
| 17:21 |  | Where possible delete the table root page prior to updating the schema cookie, and that avoids the need to early-out the reprepare loop. (check-in: 258891bee0 ... user: drh tags: schema-reset-appfunc) | 
| 17:21 |  | Fix another fts5 problem with mixing writes in secure-delete and non-secure-delete mode on a single table. Report 2026-10-22T16:23:31Z. (check-in: 0b486e53a4 ... user: dan tags: branch-3.54) | 
| 15:44 |  | Do not be as aggressive at blocking automatic reprepares due to a pending schema reset. (check-in: c1a6ef0daa ... user: drh tags: schema-reset-appfunc) | 
| 15:26 |  | Update to include the latest trunk enhancements. (check-in: 95da88756c ... user: drh tags: schema-reset-appfunc) | 
| 15:18 |  | Test case tweaks to account for different configurations. (check-in: 46476df942 ... user: drh tags: trunk) | 
| 14:57 |  | Fix an fts5 problem with deleting a key in secure-delete mode on a detail=none table when the key being deleted is the last on its page and has a delete-marker attached to it. Report 2026-10-21T17:14:08Z. (check-in: 2d7df34b62 ... user: dan tags: branch-3.54) | 
| 14:43 |  | Merge corrections from branch-3.54 into trunk (check-in: c2c5f34f0f ... user: drh tags: trunk) | 
| 14:13 |  | Disable (at least temporarily) a test case associated with .recover that shows different corruption recoveries based on compile-time options. (check-in: 16dd7e5415 ... user: drh tags: branch-3.54) | 
| 13:55 |  | Fix the ext/recover/recovercorrupt4.test script so that works even if cell_size_check is turned on by default. (check-in: a2b7dcba6b ... user: drh tags: branch-3.54) | 
| 13:21 |  | Test case changes to account for various configurations. (check-in: a207774a85 ... user: drh tags: trunk) | 
| 12:46 |  | Reorganize OP_MakeRecord slightly to avoid signed 32-bit integer overflow prior to discovering an oversize error. (check-in: 8575e97c30 ... user: drh tags: branch-3.54) | 
| 12:37 |  | Back out the tSerial type definition change. It was a mistake. (check-in: fc56735f1c ... user: drh tags: branch-3.54) | 
| 11:46 |  | Improvements to handling of failed DDL statements from within an application-defined function. (check-in: 35cf7c1e81 ... user: drh tags: schema-reset-appfunc) | 
| 08:23 |  | Improve detection of corrupt changesets in session module function sqlite3changeset_next(). (check-in: 75cb77338b ... user: dan tags: trunk) | 
| 06:39 |  | Avoid a signed integer overflow in fts5 when processing a corrupt database. (check-in: 888adfb93a ... user: dan tags: trunk) | 
| 01:20 |  | Experimental fix to Bug 2026-10-25T19:26:00Z (check-in: efdff276d2 ... user: drh tags: schema-reset-appfunc) | 
| 2026-10-25 |  |  | 
| 22:40 |  | Merge all recent branch-3.54 fixes into trunk (check-in: 4fee54f1d6 ... user: drh tags: trunk) | 
| 22:12 |  | Prevent a NULL pointer dereference in dbdump after an OOM (check-in: 53da8bd2c3 ... user: drh tags: branch-3.54) | 
| 22:05 |  | Fix the sqlite3_db_dump() interface to the dbdump.c extension so that it works even on an empty database with PRAGMA empty_result_callbacks engaged. (check-in: 8026e49181 ... user: drh tags: branch-3.54) | 
| 22:00 |  | Make sure memory allocations do not overflow in spellfix on i386 and similar 32-bit platforms. Later written up as bug 2026-11-20T13:11:10Z. (check-in: 65c545ab8d ... user: drh tags: branch-3.54) | 
| 21:51 |  | Avoid an integer overflow in the spellfix extension when setting a ridiculous values for the top= parameter. Later reported as a bug at 2026-11-20T13:14:51Z. (check-in: 3dfc7fa4c2 ... user: drh tags: branch-3.54) | 
| 20:42 |  | Use the new sqlite3_result_str() function to implement SQL functions geopoly_json() and geopoly_svg() as part of the geopoly extension. (check-in: 4fbce3be94 ... user: drh tags: branch-3.54) | 
| 20:29 |  | Increase the size of the Fts3Table.nPendingData field to 64 bits to avoid any chance of ridiculous inputs causing a signed integer overflow. (check-in: 56f261a676 ... user: drh tags: branch-3.54) | 
| 20:15 |  | Fix another problem with OS_VXWORKS (check-in: 5eb1662fbb ... user: drh tags: branch-3.54) | 
| 19:29 |  | Avoid a small memory leak that might occurs if SQLite is compiled using -DOS_VXWORKS=1 and a call to sem_open() in the unix-namedsem VFS fails. (check-in: f0a3729e38 ... user: drh tags: branch-3.54) | 
| 18:43 |  | Better detection of database corruption in the clearCellOverflow() routine. (check-in: b6c072240d ... user: drh tags: branch-3.54) | 
| 18:13 |  | Fix two instances in test cases where there is an attempt to delete a file that is still open, which does not work on windows. (check-in: c6e22bceae ... user: drh tags: branch-3.54) | 
| 17:45 |  | Use a 64-bit integer to measure expression chain size in dupedExprSize(), in case of crazy expressions. (check-in: ed78f5d2bf ... user: drh tags: branch-3.54) | 
| 17:38 |  | Use a new typedef "tSerial" to determine the datatype for serial-type value. It is 32 bits for reasonable SQLITE_MAX_LENGTH but can be 64 bits if necessary. (check-in: 8435c6f0b1 ... user: drh tags: branch-3.54) | 
| 16:57 |  | Reorder a sequence point violation. Bug report 2026-10-25T16:05:55Z. (check-in: 78ad029d8f ... user: stephan tags: branch-3.54) | 
| 16:54 |  | Increase the size of unixShmNode.nRegion to 32 bits to avoid the possibility of integer overflow. (check-in: 033b603ced ... user: drh tags: branch-3.54) | 
| 15:59 |  | Teach SQLite's internal sqlite3AtoF() routine to understand "Inf" and in so doing fix Bug 2026-10-22T05:11:21Z. (check-in: 8b1fa7497f ... user: drh tags: trunk) | 
| 13:52 |  | Merge the improved .recover OOB write fix from branch-3.54 (check-in: 5c07d633ca ... user: drh tags: trunk) | 
| 13:46 |  | An even better fix to the same problem as the previous two check-ins. (check-in: 8f694d2a7d ... user: drh tags: branch-3.54) | 
| 13:36 |  | A better fix for the recover OOB write problem of the [37963c3869640f98] (check-in: fbe2d9fb82 ... user: drh tags: branch-3.54) | 
| 13:29 |  | Merge recent branch-3.54 fixes into trunk. (check-in: 00a52a3da1 ... user: drh tags: trunk) | 
| 13:07 |  | Fix a possible OOB write in the recover extension given a corrupt schema. Bug 2026-10-25T12:08:19Z. (check-in: 37963c3869 ... user: drh tags: branch-3.54) | 
| 12:39 |  | Fix a bug in the whereOrInsert() routine of the query planner that caused a sub-optimal query plan to be selected. Bug 2026-10-24T05:24:02Z The speedtest1.c program is updated to include the test case. (check-in: acda734dc8 ... user: drh tags: trunk) | 
| 12:30 |  | The test case for this bug exercises code paths through the query planner and bytecode engine that are not otherwise followed in performance testing, so it seems reasonable to add the test case to speedtest1.c. (closed check-in: eff0498638 ... user: drh tags: where-or-insert) | 
| 11:23 |  | Fix performance problem according to bugs-forum reply 2026-10-25T01:57:04Z. (check-in: 040d465c8a ... user: drh tags: where-or-insert) | 
| 2026-10-24 |  |  | 
| 22:25 |  | Rework the classic seconds-only sleep computations in [458351f9725a423b] to avoid any chance of integer overflow. (check-in: d08cabd778 ... user: drh tags: trunk) | 
| 19:50 |  | Fix an fts5 problem causing auxiliary functions to see an incorrect list of matches in some cases when the query contains a NEAR expression. Report 2026-10-22T16:41:04Z. (check-in: fcddc5ab9a ... user: dan tags: trunk) | 
| 19:30 |  | Fix some cases where the list of matches made available to auxilary fts5 functions was not correct. Report 2026-10-23T02:22:13Z. (check-in: 16ba33e6df ... user: dan tags: trunk) | 
| 18:56 |  | The recover extension no longer drops the last column of a table with a generated column, and other fixes. Bug 2026-10-20T02:56:33Z (check-in: 0ce909f57a ... user: drh tags: trunk) | 
| 18:26 |  | Make the fts5_vocab virtual table WITHOUT ROWID, so that RIGHT JOIN and OR queries work properly with it. Report 2026-10-23T03:07:25Z. (check-in: 25d3a17413 ... user: dan tags: trunk) | 
| 18:13 |  | Pages that fail the cell-size-check should be marked as uninitialized so they are not reused without being checked again. Bug 2026-10-24T05:20:01Z (check-in: 0f540888bc ... user: drh tags: trunk) | 
| 17:25 |  | This is the change suggested by Bug 2026-10-24T05:24:02Z. But it makes SQLite run slower, at least on the speedtest1.c benchmark. Need to investigate further. (check-in: 1d40327bcf ... user: drh tags: where-or-insert) | 
| 17:08 |  | Always report back the output flags from the xOpen method of the unix VFS. Bug 2026-10-24T04:49:19Z (check-in: 7bd441063b ... user: drh tags: trunk) | 
| 16:49 |  | Fix various minor problems in the CLI. Bug 2026-10-24T04:43:44Z (check-in: a7fd475c0d ... user: drh tags: trunk) | 
| 16:40 |  | Upate AGENTS.md to encourage robots to generate PoC scripts that run in the CLI. (check-in: d91738256e ... user: drh tags: trunk) | 
| 16:35 |  | Have vec1 check the vector size field is within range when loading a configuration. Report 2026-10-24T09:06:20Z. (check-in: d3325353c8 ... user: dan tags: branch-3.54) | 
| 16:18 |  | Enforce the upper limit on vector size in vec1_train() as well as in the vec1 virtual table. This fixes integer overflow report 2026-10-23T11:24:33Z. (check-in: 25a81a8650 ... user: dan tags: branch-3.54) | 
| 15:10 |  | For the "relaxed" quoting mode of QRF, tighten the restrictions on what must be quoted to help prevent strings that actually look like numbers from going unquoted. Bug 2026-10-24T04:41:27Z (check-in: 7c8c79a443 ... user: drh tags: trunk) | 
| 14:54 |  | Use 64-bit math to avoid integer overflow in unixSleep() on the rare systems that lack both usleep() and nanosleep(). Bug 2026-10-24T09:43:25Z (check-in: 458351f972 ... user: drh tags: trunk) | 
| 14:37 |  | Change the order of fields in an internal structure to avoid the possibility of leaking heap address information when a corrupt database is queried. (check-in: 3f9a0ba4f4 ... user: dan tags: branch-3.54) | 
| 14:29 |  | Change the order of fields in an internal structure to avoid the possibility of leaking heap address information when a corrupt database is queried. Problem identified and reported by Jimmy Vassenden. (check-in: a5261fd580 ... user: dan tags: trunk) | 
| 14:05 |  | New CLI commands: .if, .elseif, .else, and .endif. New CLI function: capable(X). (check-in: 7ba9c1f65f ... user: drh tags: trunk) | 
| 13:57 |  | Improvement to testing using the new CLI commands. (leaf check-in: 1209b11fda ... user: drh tags: if-else) | 
| 12:34 |  | Further improvements and bug fixes to .if. Add the if.sql test script. (check-in: 8d352db18c ... user: drh tags: if-else) | 
| 11:59 |  | Enhancements and bug fixes to .if. Added a heredoc option to .read for testing purposes. (check-in: 680551e713 ... user: drh tags: if-else) | 
| 08:19 |  | Add the "capable(X)" function to the CLI, with the idea of using it for things like ".if capable('fts5')". (check-in: cdef947cd0 ... user: drh tags: if-else) | 
| 03:27 |  | Initial prototype implementation of ".if", ".elseif", ".else" and ".endif" for the CLI. (check-in: 52f3525656 ... user: drh tags: if-else) | 
| 01:23 |  | Fix a test case for geopoly that should have been inside of "ifcapable geopoly". (check-in: 0172c575ef ... user: drh tags: trunk) | 
| 00:40 |  | Merge WAL race condition fixes from branch-3.54 (check-in: 6322b44f13 ... user: drh tags: trunk) | 
| 2026-10-23 |  |  | 
| 18:39 |  | FTS5: INSERT into a locale=1, contentless_unindexed=1 table with an UNINDEXED column now works. Bug 2026-10-21T02:49:37Z (check-in: 055f90829c ... user: drh tags: trunk) | 
| 18:14 |  | Fix a race condition with a writer that could cause a connection with a read-only *-shm file to return a spurious SQLITE_READONLY_RECOVERY error. (check-in: e2e7c8eb02 ... user: dan tags: branch-3.54) | 
| 17:24 |  | Fix a race condition with a writer that could cause a connection with a read-only *-shm file to return a spurious SQLITE_READONLY_CANTINIT error. (check-in: 30a301835b ... user: dan tags: branch-3.54) | 
| 16:01 |  | Merge the FTS5 one-byte overread fix from branch-3.54 (check-in: b527190046 ... user: drh tags: trunk) | 
| 15:56 |  | Merge BOM handling fixes. (check-in: 5b0c5f6341 ... user: drh tags: trunk) | 
| 15:52 |  | The .recover command should not remove BOMs that are already inside the database. Bug 2026-10-18T20:41:57Z (closed check-in: 69f4d423fa ... user: drh tags: bom-handling) | 
| 15:30 |  | Fix the sqlite3_bind_value() interface so that it does not remove a byte-order mark on the value being bound. (check-in: 56e268312d ... user: drh tags: bom-handling) | 
| 13:41 |  | FTS5 trigram: LIKE and GLOB on UNINDEXED column. Bug 2026-10-23T03:09:50Z (check-in: 887264cd9a ... user: drh tags: trunk) | 
| 13:22 |  | Fix a one-byte buffer overread in FTS5 associated with LIKE processing. Bug 2026-10-23T12:49:10Z. (check-in: 60222d023e ... user: drh tags: branch-3.54) | 
| 13:06 |  | Follow documented behavior when casting a zeroblob() to text on a UTF-16 database. bug 2026-10-23T12:45:59Z (check-in: 8a0296aa4d ... user: drh tags: trunk) | 
| 12:51 |  | Merge the FTS5 fix from branch-3.54 (check-in: 320cb1f513 ... user: drh tags: trunk) | 
| 12:23 |  | Fix a UAF in FTS5 on a query that uses a prefix query and a ordinary term inside of a transaction. Bug 2026-10-23T03:10:17Z. (check-in: 599f3345b9 ... user: drh tags: branch-3.54) | 
| 12:14 |  | Fix query planner error in the case of a multi-index OR on a WITHOUT ROWID table where the OR-term uses the seek-scan optimization. Bug 2026-10-23T03:09:01Z. (check-in: b33ff35e62 ... user: drh tags: trunk) | 
| 11:55 |  | Fix a problem in QRF when the screen width is narrower than a column. Bug 2026-10-23T03:50:28Z (check-in: 8de43901c7 ... user: drh tags: trunk) | 
| 11:15 |  | Enhancements to the geopoly extension to catch some invalid polygons that it was letting through. Bug 2026-10-23T03:54:22Z (check-in: a327ab93a4 ... user: drh tags: trunk) | 
| 11:09 |  | Enhance the ieee754(X,Y) function so that it returns 0.0 even if Y very large. Bug 2026-10-23T03:55:14Z (check-in: 7d8cc4d253 ... user: drh tags: trunk) | 
| 10:57 |  | Cap the sleep time for sqlite3_sleep() at 2147483 milliseconds to avoid overflowing the microsecond-resolution parameter to the xSleep method of the VFS. Test cases in TH3. Bug 2026-10-23T04:02:08Z. (check-in: 58efac8396 ... user: drh tags: trunk) | 
| 10:39 |  | Merge the latest branch-3.54 fixes into trunk (check-in: 47da4393af ... user: drh tags: trunk) | 
| 09:48 |  | In FTS5, fix a potential read past the string terminator while parsing a malformed RANK input. Private email from Mirchev. (check-in: d7e1c85c17 ... user: drh tags: branch-3.54) | 
| 09:38 |  | Fix a heap-buffer overread in FTS5 due to a corrupt inverted index holding an empty doclist. Private email from Mirchev. (check-in: d0716cc48b ... user: drh tags: branch-3.54) | 
| 09:34 |  | Ensure that all content that is copied into the -shm file during recovery has been initialized, even in the case of no valid WAL frames. (check-in: f2255af823 ... user: drh tags: branch-3.54) | 
| 09:08 |  | Ensure that all content that is copied into the -shm file during recovery has been initialized, even in the case of no valid WAL frames. Private email from Mirchev. (check-in: 64c93bc7e6 ... user: drh tags: trunk) | 
| 00:49 |  | Fix two minor issues with the geopoly extension. Bugs bugs:/info/2026-10-21T02:53:43Z and bugs:/info/2026-10-21T05:30:42Z. (check-in: d6f1df8c0b ... user: drh tags: trunk) | 
| 2026-10-22 |  |  | 
| 20:03 |  | Performance improvements in PRAGMA integrity_check and in materialized CTEs. (check-in: 52d0c0b3d1 ... user: drh tags: trunk) | 
| 19:55 |  | Improve ALTER TABLE DROP CONSTRAINT so that it is better able to deal with some corner case in the constraint syntax. Bugs 2026-10-22T16:25:18Z and 2026-10-22T16:30:48Z. (check-in: 14d924c33f ... user: drh tags: trunk) | 
| 14:03 |  | Remove unused per-index counters from integrity checking. (closed check-in: d0b5bc53a9 ... user: jeffchen tags: performance) | 
| 14:03 |  | Avoid repeated setup when reusing a materialized CTE. (check-in: 18bec552a0 ... user: jeffchen tags: performance) | 
| 11:41 |  | Merge branch-3.54 fixes into trunk (check-in: fdf339d965 ... user: drh tags: trunk) | 
| 11:35 |  | Make the session rebaser more robust the face of column count mismatches. (check-in: 12285a3f4e ... user: drh tags: branch-3.54) | 
| 11:19 |  | Add a rootpage number lower bounds check in the "recover" extension. Test case in TH3. (check-in: 4a1fd244b5 ... user: drh tags: branch-3.54) | 
| 11:03 |  | Increased safety margin in sqlite3_preupdate_old(). Test case in TH3. (check-in: 31b8415796 ... user: drh tags: branch-3.54) | 
| 10:40 |  | Extra safety margin added to sqlite3VdbeMemFromBtree(). Change suggested by Martin Mirchev. Test cases in TH3. (check-in: 952ae2c63e ... user: drh tags: branch-3.54) | 
| 2026-10-21 |  |  | 
| 23:49 |  | Index lookup of an integer column using value -9223372036854775808.0 now works. Bug 2026-10-21T17:54:28Z (check-in: 03f699544e ... user: drh tags: trunk) | 
| 20:53 |  | Make the NOT INDEXED key phrase work for WITHOUT ROWID tables. Bug 2026-10-21T17:47:47Z (check-in: 9eafe6158d ... user: drh tags: trunk) | 
| 17:36 |  | Mark the AND optimization as "not-a-real-0-or-1" so as not to confuse ORDER BY and GROUP BY clauses. Bug 2026-10-21T15:33:18Z (check-in: 16cf625360 ... user: drh tags: trunk) | 
| 17:23 |  | Do not synthesize position-lists for UNINDEXED fts5 columns in detail=none mode. Report 2026-10-20T20:40:52Z. (check-in: 64e37575ca ... user: dan tags: trunk) | 
| 16:56 |  | Avoid orphaning an fts5 %_content table when dropping or renaming an fts5 table that uses contentless_unindexed=1. Report 2026-10-21T02:51:01Z. (check-in: fa0247a431 ... user: dan tags: trunk) | 
| 16:48 |  | Improve usage of fts5 doclist-indexes in tokendata=1 tables. (check-in: 3e4c42ea20 ... user: dan tags: trunk) | 
| 16:29 |  | New CLI-based test cases for foreign keys. (check-in: f4f25cd2a8 ... user: drh tags: trunk) | 
| 16:05 |  | When querying an fts5 table in tokendata mode, avoid using the "has-doclist" flag from the previous leaf page when querying for a non-existent term larger than all terms on that page. Report 2026-10-21T02:52:01Z. (check-in: 3769f60be3 ... user: dan tags: trunk) | 
| 15:25 |  | Avoid returning extra rows when the RHS of a "rowid = " constraint on an fts5 table is not an integer value. Report 2026-10-20T20:31:49Z. (check-in: 5a09fc8073 ... user: dan tags: branch-3.54) | 
| 15:15 |  | Improve RTREE to be more robust against abuses of the "rowid" column. Bug 2026-10-21T05:31:28Z (check-in: 8d5a59d56e ... user: drh tags: trunk) | 
| 14:41 |  | Allow the foreign key code to use child indexes even when there are affinity mismatches between child and parent columns. (check-in: ee1424130e ... user: dan tags: trunk) | 
| 14:37 |  | Allow composite FKs to use indexes with mismatched affinities as well. (closed check-in: 2e0883964e ... user: dan tags: experimental-fk-opt) | 
| 14:35 |  | Allow the origin counter in FTS5 to grow beyond 32 bits. Bug 2026-10-21T10:56:15Z (check-in: a0fed3f26e ... user: drh tags: trunk) | 
| 14:28 |  | Fix the fsdir() table-valued function to better cope with unreasonable LEVEL constraints. Bug 2026-10-21T10:49:41Z (check-in: 3e8932ebf2 ... user: drh tags: trunk) | 
| 12:57 |  | The IN pre-filter is now only added when the child key can actually use an index lookup. (check-in: a3d46e1f8f ... user: drh tags: experimental-fk-opt) | 
| 12:24 |  | An attempt to get the new foreign key code to use indexes on type mismatch between parent and child. (check-in: c0523245e2 ... user: drh tags: experimental-fk-opt) | 
| 11:52 |  | Ensure that all (parent_key = child_key) comparisons performed as part of foreign key processing are done using the affinity of the parent key. Report 2026-10-20T02:56:10Z. (check-in: feb6d3a301 ... user: dan tags: trunk) | 
| 11:51 |  | Cap the number of entires in a ZIP archive processed by the zipfile extension at 65535. Bug 2026-10-21T10:52:39Z (check-in: decd49e656 ... user: drh tags: trunk) | 
| 11:25 |  | If a reprepare fails in sqlite3_stmt_explain(), do not leave the prepared statement in an invalid state. Bug 2026-10-21T09:18:32Z. Test cases in TH3. (check-in: 0ffda38c5f ... user: drh tags: trunk) | 
| 10:31 |  | Improvements to vtab handling of SAVEPOINT. (check-in: 1d886c6abf ... user: drh tags: trunk) | 
| 10:27 |  | Test cases added (closed check-in: 2ec6c0ff49 ... user: drh tags: vtab-savepoint) | 
| 00:34 |  | If a virtual table is created within a SAVEPOINT, it needs to be undo by the corresponding ROLLBACK TO. This resolves an issue going back to before version 3.7.8 circa 2011. Bug 2026-10-20T20:37:14Z (check-in: 33aeb419d5 ... user: drh tags: vtab-savepoint) | 
| 2026-10-20 |  |  | 
| 20:17 |  | Change the sqlite3_serialize() interface to return a NULL pointer if the database is file-based and an I/O error occurs while reading. Enhancement request 2026-10-18T10:04:32Z. (check-in: aa36cfe5b0 ... user: drh tags: trunk) | 
| 19:30 |  | Simplify the ".clone" command so that it just makes a copy of the main database. Move the implementation into a separate dotCmdClone() function and provide better help text explaining its history. (check-in: 8b5c87a5f4 ... user: drh tags: trunk) | 
| 19:21 |  | Fix the shellB.test module so that it does not try to run dump.sql without virtual tables available. (check-in: 3f8b4c7760 ... user: drh tags: trunk) | 
| 18:25 |  | Be sure to use the correct collating sequence when doing a balanced merge of a four-way or larger compound select. Error goes back to version 3.38.0 (2021). Bug 2026-10-18T23:46:25Z (check-in: 7ee2a48bb3 ... user: drh tags: trunk) | 
| 17:53 |  | Merge branch-3.54 fixes into trunk (check-in: 8efb0acad8 ... user: drh tags: trunk) | 
| 17:46 |  | Enhance the readfile() function in the fileio.c extension so that it is able to read from pipes. Enhancement request 2026-10-20T14:02:37Z (check-in: 49d185f2ac ... user: drh tags: trunk) | 
| 17:42 |  | Remove a debugging ".mode -v" in the dump.sql test script that was accidentally left in the previous check-in. (check-in: 632943da2d ... user: drh tags: trunk) | 
| 17:27 |  | When a b-tree cell is deleted, zero the cursor's cell-info cache as the cell it describes no longer exists, even if the cursor position is being preserved. Report 2026-10-18T16:11:58Z. (check-in: 07ed30558e ... user: dan tags: branch-3.54) | 
| 17:24 |  | Fix the --preserve-rowids option on the ".dump" command of the CLI so that it works for tables that have a non-IPK column named "rowid". Bug 2026-10-20T03:22:48Z (check-in: ecd172bb44 ... user: drh tags: trunk) | 
| 17:05 |  | Fix the sqlite3_carray_bind() interface so that it works with SQLITE_TRANSIENT and an array length of zero. Enhancement request 2026-10-20T13:53:58Z (check-in: 47228787b1 ... user: drh tags: trunk) | 
| 16:40 |  | Provide auto-init for the sqlite3_cancel_auto_extension(). Enhancement request 2026-10-20T13:51:06Z. (check-in: 08962c885f ... user: drh tags: trunk) | 
| 16:11 |  | When a real value is specified as a default for a text column, avoid various inconsistencies by ensuring that it is always converted to an IEEE double and then back to text before use, as is already done for integer values. Report 2026-10-05T16:22:00Z. (check-in: d140ada648 ... user: dan tags: branch-3.54) | 
| 15:42 |  | Raise an error if a REGEXP pattern ends with "\\". Bug 2026-10-20T13:44:52Z (check-in: 480d6e1a39 ... user: drh tags: branch-3.54) | 
| 15:19 |  | Fix handling of GROUP BY and HAVING clauses attached to the rightmost SELECT of a compound SELECT with one or more COLLATE clauses attached to ORDER BY expressions. Report 2026-10-16T22:06:45Z. (check-in: bd873f7ca9 ... user: dan tags: branch-3.54) | 
| 14:36 |  | Fix QRF so that it correctly handles zero-length output appended to the sqlite3_qrf_spec.pzOutput result. /info/2026-10-20T13:22:11Z (check-in: ee344fc063 ... user: drh tags: branch-3.54) | 
| 14:07 |  | Correctly handle aggregate functions that aggregate over the rows of a window function query, even if they are nested within sub-selects. Report 2026-10-16T08:07:09Z. (check-in: 7522512ff9 ... user: dan tags: branch-3.54) | 
| 13:45 |  | Fix the CLI to correctly handle a ".exit" immediately following a ".output memory". Bug 2026-10-20T13:13:50Z (check-in: 2fdd752dc9 ... user: drh tags: branch-3.54) | 
| 13:10 |  | Test case changes to handle non-standard configurations. (check-in: 185ffd2706 ... user: drh tags: trunk) | 
| 12:42 |  | Add an extra sqlite3MemoryBarrier() call in sqlite3_initialize() to avoid a race condition that only happens on non-X64 CPUs. Bug 2026-10-20T07:33:27Z (check-in: 37b83ad2f5 ... user: drh tags: trunk) | 
| 12:07 |  | Do not allow a simplified constant expression (example: "123 ISNULL" → "1") to be used as the ordinal column number in an ORDER BY or GROUP BY clause. Bug 2026-10-16T12:58:09Z (check-in: c40e620b04 ... user: drh tags: trunk) | 
| 11:13 |  | Fix a problem causing queries on secure-delete fts5 databases to occasionally return extra, non-matching, rows. Report 2026-10-18T20:40:47Z. (check-in: 10b8f4166d ... user: dan tags: branch-3.54) | 
| 09:49 |  | Fix ALTER TABLE RENAME so that it works on triggers that contain UPSERT. Bug 2026-10-20T03:23:05Z (check-in: be17b0b030 ... user: drh tags: trunk) | 
| 09:24 |  | Minor fix to ALTER TABLE RENAME COLUMN so that it works on TEMP tables. Bug 2026-10-19T01:55:31Z (check-in: 6d998905f8 ... user: drh tags: trunk) | 
| 09:00 |  | Remove an ALWAYS() that can sometimes be true when using sqlite3_collation_needed() and dropping collations. Bug 2026-10-18T18:09:53Z (check-in: 8717ff8d73 ... user: drh tags: trunk) | 
| 00:17 |  | Merge the latest branch-3.54 fixes into trunk (check-in: b3606f7019 ... user: drh tags: trunk) | 
| 00:14 |  | Fix an adverse interaction between triggers changing a table that uses ON DELETE SET NULL constraints and the optimization at [8c5dd6cc259e0cda], almost 12 years ago. Bug 2026-10-19T06:36:47Z. (check-in: 15424da529 ... user: drh tags: trunk) | 
| 2026-10-19 |  |  | 
| 23:49 |  | Fix a harmless compiler warning in lemon.c. No changes to the core SQLite. (check-in: fd341a1952 ... user: drh tags: trunk) | 
| 23:22 |  | Fix errors in the decimal_cmp() comparison routine of the decimal extension, and the corresponding "decimal" collating sequence, as reported by Bug 2026-10-19T06:35:26Z. This fix requires running "REINDEX decimal" if the decimal collating sequence was used to build any indexes. (check-in: bf74b51f7e ... user: drh tags: trunk) | 
| 21:07 |  | Correct two bugs, dating back to [2022-12-23], which caused the JS wrappers of sqlite3_bind_text/blob() to severely misbehave. Add missing tests. Bug report 2026-10-19T19:03:55Z. (check-in: d45943cbaf ... user: stephan tags: branch-3.54) | 
| 20:21 |  | Avoid dropping error codes from the streaming input callback in sqlite3rebaser_rebase_strm(). Report 2026-10-18T05:31:08Z. (check-in: 90f953b42a ... user: dan tags: branch-3.54) | 
| 19:40 |  | Fix a problem in ALTER TABLE DROP COLUMN that could result in a stale index. Bug 2026-10-19T06:36:17Z (check-in: 4ea5d16c21 ... user: drh tags: branch-3.54) | 
| 19:15 |  | Fix a regression in the CLI's handling of control characters in CSV output, caused by [d0fe92e0ce8662b9]. Bug 2026-10-18T20:38:57Z. (check-in: 2146eab561 ... user: drh tags: branch-3.54) | 
| 18:57 |  | Fix a problem in the sessions rebaser occuring when a DELETE/REPLACE rebase record is followed by another rebase record on the same row. Report 2026-10-18T05:00:41Z. (check-in: c2d2991242 ... user: dan tags: branch-3.54) | 
| 18:33 |  | Wasm build: move wasm-strip to after wasm-opt, at recommendation from an Emscripten ticket. Remove several more uses of -sWASM_BIGINT=1, as described for [cc9457981e2a]. (check-in: c2fc091401 ... user: stephan tags: branch-3.54) | 
| 18:26 |  | Merge the latest branch-3.54 fixes onto trunk (check-in: 79ba7ba1a7 ... user: drh tags: trunk) | 
| 18:20 |  | Correct the handling of SQLITE_IGNORE returns from authorizer calls for UPDATE on the rowid or INTEGER PRIMARY KEY. See the test cases in TH3 for details. (check-in: 606005419f ... user: drh tags: branch-3.54) | 
| 18:05 |  | JS: resolve a build regression triggered by Emscripten 6.0.5+. Remove the -sWASM_BIGINT flag from the build because (A) it's been Emscripten's default for a long time and (B) Emscripten now warns loudly if it's used. Remove a stray build flag from fiddle, inadvertently introduced in [c32cf3e355], which caused it to build in 64-bit mode. (check-in: cc9457981e ... user: stephan tags: branch-3.54) | 
| 17:44 |  | Avoid crashing if an RBU vacuum is attempted with a VFS that does not support RBU. Report 2026-10-19T12:21:34Z. (check-in: 5024b528dc ... user: dan tags: branch-3.54) | 
| 17:24 |  | Fix JSON parsing to properly recognize a backslash-continuation at the end of a string. Bug 2026-10-18T04:42:37Z (check-in: d8c93a5f2c ... user: drh tags: branch-3.54) | 
| 16:25 |  | In the CLI, do not read the ~/.sqliterc file or an alternative specified by the --init command-line option if the --safe option is used. Bug 2026-10-19T12:32:27Z (check-in: f1e94b6042 ... user: drh tags: branch-3.54) | 
| 16:06 |  | Update sqldiff so that the script it outputs preserves the full precision of floating point values. This fixes the first of two issues reported by Bug 2026-10-18T20:38:30Z (check-in: 2a439593d0 ... user: drh tags: branch-3.54) | 
| 15:14 |  | Correctly handle compound SELECTs with explicit COLLATE clauses on ORDER BY expressions, even if they appear in a CTE or VIEW definition. Report 2026-10-19T08:22:22Z. (check-in: 36803e2660 ... user: dan tags: branch-3.54) | 
| 15:06 |  | Apply [5080eb6d7a7a] to a similar case in the JS test suite. (check-in: 336154a23a ... user: stephan tags: branch-3.54) | 
| 14:50 |  | JS: reject URL arguments which may influence the load path for sqlite3.js and sqlite3.wasm if those values look like they might be remote URLs. sqlite3.mjs (ESM) is unaffected by this. This addresses bug report 2026-10-19T12:25:33Z but is a preliminary measure, with follow-ups expected. (check-in: 5080eb6d7a ... user: stephan tags: branch-3.54) | 
| 14:34 |  | The default floating point format for JSON and INSERT output modes should be "%!0.17" to preserve full binary→text→binary round-trips with no loss of precision. Bug 2026-10-18T20:37:23Z. (check-in: c31221e954 ... user: drh tags: branch-3.54) | 
| 12:02 |  | Fix the ".dump" command in the CLI so that it is not affected by mode settings like --fpfmt, --ifmt, or --textjsonb. /info/2026-10-18T20:39:22Z (check-in: a9d8123e27 ... user: drh tags: branch-3.54) | 
| 11:06 |  | Fix a vec1 problem with querying a table within the same transaction as writing it in some configurations. (check-in: e2af533c28 ... user: dan tags: trunk) | 
| 08:05 |  | Fix a vec1 problem with issuing a 'rebuild' command within a transaction that has already written to the table. Report 2026-10-18T20:40:18Z. (check-in: 2448fc46ca ... user: dan tags: trunk) | 
| 05:43 |  | Fix a vec1 problem with deleting rows that share rowids with tombstone values within transactions. Report 2026-10-18T20:41:06Z. (check-in: b8fc7a1398 ... user: dan tags: trunk) | 
| 2026-10-18 |  |  | 
| 23:51 |  | Merge branch-3.54 fixes into trunk (check-in: d987e46422 ... user: drh tags: trunk) | 
| 23:45 |  | Do not run the OR optimization on the left operand of a LEFT JOIN. Bug 2026-10-18T16:10:55Z. (check-in: b497412752 ... user: drh tags: branch-3.54) | 
| 22:48 |  | If terms on the SELECT on the RHS of a vector IN are removed, then the subquery cannot be reused for other things, since it is now different. Bug 2026-10-16T22:07:15Z (check-in: 080338a91b ... user: drh tags: trunk) | 
| 21:36 |  | Fix harmless compiler warnings. (check-in: a68c9f3e98 ... user: drh tags: trunk) | 
| 19:42 |  | Fix the decimal extension to correctly deal with NULL parameters. Bug 2026-10-18T14:25:03Z (check-in: 716b63c934 ... user: drh tags: trunk) | 
| 19:10 |  | Add the SQLITE_LIMIT_CTES option to sqlite3_limit(). The default is 50K. Setting to zero disables the limit. The limit is approximate - it does not actually count CTEs but rather simply refuses to process a CTE with cursor number that exceeds this limit. This is a re-do of the fix for Bug 2026-10-04T05:35:14Z that also avoids the regression reported by Forum post 2026-10-16T20:30:17Z. (check-in: b32709ea06 ... user: drh tags: trunk) | 
| 18:30 |  | Limit the size of the second argument of the (testing use only) SQL function fpdecode() to avoid a (harmless) signed integer overflow. Bug 2026-10-18T03:43:20Z (check-in: 72187f18fe ... user: drh tags: trunk) | 
| 16:31 |  | Merge recent branch-3.54 fixes into trunk (check-in: f9e0ce3382 ... user: drh tags: trunk) | 
| 16:17 |  | Back out parts of [e68aa6e1293a2c7a] for the 3.54.x patch series, as they cause a regression reported by Forum post 2026-10-16T20:30:17Z. The "bug" fixed was not important. The plan is to add a new limit in 3.55.0 to address this problem. (check-in: 845190e3f2 ... user: drh tags: branch-3.54) | 
| 16:04 |  | Remove a NEVER() that can in fact be true. A test case to demonstrate has been separately added to TH3. Bug 2026-10-18T07:33:23Z. (check-in: 70bb130523 ... user: drh tags: branch-3.54) | 
| 13:54 |  | Ensure that vec1 normalizes vectors before, not after, applying the WHT transform. Report 2026-10-18T10:46:11Z. (check-in: b5a53fbede ... user: dan tags: trunk) | 
| 10:43 |  | Correct a var name typo in the session extension. Bug report 2026-10-18T10:40:12Z. (check-in: 9826ef1c52 ... user: stephan tags: branch-3.54) | 
| 10:39 |  | Minor doc-only updates to ext/wasm/README.md and GNUmakefile. (check-in: 7f6793fe94 ... user: stephan tags: branch-3.54) | 
| 10:14 |  | Check the datatype of the second (optional) argument to the log() SQL function and return NULL if it is non-numeric as the documentation says it should. Bug 2026-10-18T05:05:18Z (check-in: a99e577b95 ... user: drh tags: branch-3.54) | 
| 2026-10-17 |  |  | 
| 18:29 |  | Use box-drawing characters to highlight the "Expected" and "Got" output from failed ".check" lines in the CLI, for improved debug support. (leaf check-in: 09e1f16661 ... user: drh tags: experimental) | 
| 18:25 |  | Fix uses of C99 and other minor issues in ext/vec1/vec1.c. Report 2026-10-17T09:17:48Z. (check-in: 8fd4b73592 ... user: dan tags: trunk) | 
| 13:23 |  | Fix 24:00:00 rounding of dates with negative years. Forum post 2026-10-17T12:57:32Z (check-in: 91515aa82e ... user: drh tags: trunk) | 
| 12:10 |  | Merge recent branch-3.54 fixes into trunk. (check-in: c75eff906c ... user: drh tags: trunk) | 
| 12:03 |  | Fix an incorrect assert() in generate_series(). No impact on non-debug builds. Bug 2026-10-17T09:24:09Z (check-in: c5d67762df ... user: drh tags: branch-3.54) | 
| 11:34 |  | Enhance ".expert" in the CLI so that when it is generating unique index names, the probability of being unable to create a unique names becomes vanishingly small, essentially zero. And even if that does happen, be sure to free the last candidate name before returning an error. Bug 2026-10-17T09:25:38Z (check-in: d43611f653 ... user: drh tags: branch-3.54) | 
| 11:13 |  | Enhance QRF to deal with zero-length column names. Also make use of the new sqlite3_str_free() interface. Bug 2026-10-17T09:26:21Z (check-in: d3e004fe3d ... user: drh tags: branch-3.54) | 
| 10:10 |  | Add a byte to an alloc made during .mode -widths parsing. Bug report 2026-10-17T09:23:52Z. (check-in: 9373a2e3be ... user: stephan tags: branch-3.54) | 
| 03:06 |  | Merge the latest fixes from branch-3.54 onto trunk. (check-in: e30e6624c4 ... user: drh tags: trunk) | 
| 01:52 |  | Update the sqlite3_clear_bindings() interface so that it avoids forcing a reprepare if a binding that is not 0 or 1 is cleared. Forum post 2026-10-16T19:10:10Z. (check-in: 535a77cf5a ... user: drh tags: branch-3.54) | 
| 2026-10-16 |  |  | 
| 20:31 |  | Add two new assert()s to document invariants within selectExpander(). (check-in: 69d51c3542 ... user: drh tags: trunk) | 
| 18:11 |  | Fix a problem with reading large UTF16 strings with a BOM out of the VDBE column cache. Bug 2026-10-13T01:16:49Z (check-in: 5bd093e261 ... user: drh tags: trunk) | 
| 17:58 |  | Fix a harmless C90 compiler warning in the decimal extension. (check-in: a7f6aa448f ... user: drh tags: trunk) | 
| 17:42 |  | Fix a problem handling zero-length strings in some cases in group_concat() when it is used as a window function. Report 2026-10-16T15:51:51Z. (check-in: 2f794cf18a ... user: dan tags: trunk) | 
| 16:23 |  | Increase the version number to 3.54.2 to begin the next patch cycle. (check-in: ffce7c6272 ... user: drh tags: branch-3.54) | 
| 14:45 |  | Version 3.54.1 (check-in: 555e07ef33 ... user: drh tags: release, branch-3.54, version-3.54.1) | 
| 14:31 |  | In test file test1.c, use a static SQLite mutex to protect the hash table instead of a Tcl mutex. A Tcl mutex is a no-op on unix unless TCL_THREADS is defined at runtime, and SQLite test configurations do not define this, which was causing errors in multi-threaded tests like notify2.test. (check-in: 668541c592 ... user: dan tags: trunk) | 
| 13:14 |  | Avoid unnecessary zero-length calls to memmove(). (check-in: 155b341bb4 ... user: drh tags: trunk) | 
| 10:00 |  | New optimizations to GLOB/LIKE and to mutex locking on shared-cache. (check-in: a787e60de2 ... user: drh tags: trunk) | 
| 08:43 |  | Avoid zero-length memmove() calls when appending B-tree cells. (closed check-in: d75a9fd3cf ... user: jeffchen tags: opt-btree-insertcell-skip-empty-memmove) | 
| 2026-10-15 |  |  | 
| 23:22 |  | Enable the noSharedCache fast path in SQLITE_THREADSAFE=0 builds. (closed check-in: 3122863990 ... user: jeffchen tags: opt-btree-enterall-nosharedcache) | 
| 23:15 |  | Avoid unnecessary recursive calls in LIKE and GLOB matching by checking the next ASCII literal before recursing. (closed check-in: f88d33b190 ... user: jeffchen tags: opt-like-glob-prefilter) | 
| 21:46 |  | Deal with harmless 32-bit integer overflow on parameters to some extension SQL functions. Bug 2026-10-15T04:34:08Z (check-in: ca52702835 ... user: drh tags: trunk) | 
| 19:59 |  | Avoid a crash processing "ORDER BY rowid NULLS LAST" introduced by the previous commit. (check-in: 90cab4acea ... user: dan tags: trunk) | 
| 19:07 |  | Avoid a spurious corruption report in some cases when processing an ORDER BY .. NULLS LAST query against a WITHOUT ROWID table that has had columns added to it using ALTER TABLE. Report 2026-10-15T03:54:19Z. (check-in: a2a20c54a2 ... user: dan tags: trunk) | 
| 17:36 |  | Fix DEFAULT so that BLOB values cast to text use the database encoding. Bug 2026-10-14T17:33:12Z. (check-in: 178677e141 ... user: drh tags: trunk) | 
| 17:09 |  | Fix a missing term on an assert() added by the previous check-in. (check-in: 59dec753df ... user: drh tags: trunk) | 
| 15:33 |  | When a single loop uses an IN(...) expression and a LIKE/GLOB expression with an index, nest the 2-iteration loop that scans ranges of text and blob values using the LIKE/GLOB pattern inside the IN(...) loop in order to ensure that results are scanned in index order. Report 2026-10-13T18:54:28Z. (check-in: 08e979525f ... user: dan tags: trunk) | 
| 13:55 |  | Remove code that is no longer reachable after [bbdd13370de93035]. Add an assert() to verify that the code is unreachable. (check-in: a0f9daf75b ... user: drh tags: trunk) | 
| 13:33 |  | Merge the 24:00:00 fix from branch-3.54 into trunk (check-in: 3ce64081ca ... user: drh tags: trunk) | 
| 13:19 |  | Fixes to the handling of time 24:00:00. Forum post 2026-10-15T10:26:06Z (check-in: 69071e25ca ... user: drh tags: branch-3.54) | 
| 2026-10-14 |  |  | 
| 20:07 |  | Ensure the correct collation sequence is used for expressions like "a IN (SELECT b COLLATE ...)" when using an index on "a". Bug 2026-10-14T18:00:55Z. (check-in: bbdd13370d ... user: dan tags: trunk) | 
| 18:39 |  | Fix indexes on typeless generated columns that pull from REAL columns so that they correctly return REAL values. Bug 2026-10-14T17:34:30Z. (check-in: 7408f90ecc ... user: drh tags: trunk) | 
| 17:52 |  | Fix json_tree() when it is given a root that ends with an array index so that it returns the correct "key" for the first root. This was broken by check-in [858b76a00e8ff552] and in version 3.45.0. Problem reported by Forum post 2026-10-14T08:42:26Z. (check-in: eaf646e376 ... user: drh tags: trunk) | 
| 17:52 |  | Have testrunner.tcl unset environment variable DEBUGINFOD_URLS. Leaving it set slows down some sanitizer release tests. (check-in: b7ebfea1c1 ... user: dan tags: trunk) | 
| 16:11 |  | Improve the error messages emitted by fts5 in some cases. Report 2026-10-13T21:57:58Z. (check-in: dabff5c1cd ... user: dan tags: trunk) | 
| 15:02 |  | Ensure the internal DBFLAG_SchemaKnownOk flag is always cleared after a database schema is reset. (check-in: 96a56e7cf9 ... user: drh tags: trunk) | 
| 13:39 |  | Ensure the internal DBFLAG_SchemaKnownOk flag is always cleared after a database schema is reset. Report 2026-10-14T01:25:35Z. (check-in: e6b9a40732 ... user: dan tags: branch-3.54) | 
| 12:48 |  | Merge bug fix from branch-3.54 into trunk. (check-in: c2df85dd6e ... user: drh tags: trunk) | 
| 12:43 |  | Fix harmless compiler warnings from [a65c63c6a632d60c]. (check-in: 57efbeb73e ... user: drh tags: trunk) | 
| 12:36 |  | Reduce per-cell bookkeeping in balance_nonroot. Use an advancing destination pointer in while loop. (check-in: fd68b587e9 ... user: drh tags: trunk) | 
| 12:23 |  | Reduce per-cell bookkeeping in balance_nonroot. Use an advancing destination pointer in while loop. (leaf check-in: a422babef7 ... user: jeffchen tags: opt-balance-nonroot-cell-pointer) | 
| 11:32 |  | The previous check-in was not quite correct. Both rLhsOrig and rLhs are vectors and so neither should be freed using sqlite3ReleaseTempReg(). Indeed, neither should be freed at all since both vectors remain in use. (check-in: fc7b959f45 ... user: drh tags: branch-3.54) | 
| 11:22 |  | In check-in [8800c13deca3717c], fix a double-free of a byte-code register, resulting in the same register being on the reusable register list twice. Bug 2026-10-14T05:52:10Z. (check-in: 930df67b44 ... user: drh tags: branch-3.54) | 
| 2026-10-13 |  |  | 
| 19:04 |  | Avoid incorrectly marking an index used by an OR-scan as a covering index leading to an "internal query planner error" error. Report 2026-10-11T19:31:18Z. (check-in: 3e41413d32 ... user: dan tags: trunk) | 
| 18:36 |  | Merge all recent branch-3.54 fixes into trunk (check-in: 74a6182a0c ... user: drh tags: trunk) | 
| 18:19 |  | Add test cases for CLI command parsing (cliparse.sql). (check-in: a65c63c6a6 ... user: drh tags: trunk) | 
| 17:08 |  | Avoid an assert() failure by clearing Vdbe.smimask when Vdbe.expmask is cleared. (check-in: 6399cc43b5 ... user: dan tags: branch-3.54) | 
| 15:51 |  | Restore 3.53.0 handling of codepoints u+fffe and u+ffff (they are separator characters) in fts3 and fts5. See forum post 2026-10-13T08:37:16Z. (check-in: f5ed485017 ... user: dan tags: branch-3.54) | 
| 14:59 |  | Merge fixes to vec1 made on trunk into this branch. (check-in: 2ca1a93f01 ... user: dan tags: branch-3.54) | 
| 14:00 |  | Enhancements to the partial-index constraint reuse logic of check-in [8d4160910d651246] to take into account column affinity and STRICT tables. Bug 2026-10-11T20:18:09Z. (check-in: cb7816d72c ... user: drh tags: trunk) | 
| 11:05 |  | Merge the latest fixes on branch-3.54 into trunk (check-in: c57525b36a ... user: drh tags: trunk) | 
| 10:51 |  | Enhance OP_MustBeInt so that it is able to handle -9223372036854775808.0. Bug 2026-10-11T20:33:51Z. See also [987ba5a3dc5f2a22]. (check-in: 5e488c8da3 ... user: drh tags: branch-3.54) | 
| 06:45 |  | Merge wasm-post-3.54 into branch-3.54: wasm build updates for emcc changes and an OPFS fix for an as-yet-hypothetical failure case. (check-in: c32cf3e355 ... user: stephan tags: branch-3.54) | 
| 02:43 |  | Fix the implementation of Tree-Tracker. Bug 2026-10-13T02:17:32Z. (check-in: bfe473ab60 ... user: drh tags: trunk) | 
| 02:14 |  | Fix "csv" output mode in the CLI so that it responds to --colsep and --rowsep as it should. Forum post 2026-10-12T17:59:34Z. (check-in: d0fe92e0ce ... user: drh tags: branch-3.54) | 
| 2026-10-12 |  |  | 
| 23:38 |  | Speed up substr() for the common case of ASCII text. (check-in: f69fd5f5c3 ... user: drh tags: trunk) | 
| 22:42 |  | Fix compiler warnings, mostly associated with the new ext/misc/optname.c extension. (check-in: 9d1bc870de ... user: drh tags: trunk) | 
| 22:38 |  | Speed up substr() start scanning for ASCII text. Skip bounded groups of eight nonzero ASCII bytes while advancing the start position. Keep the existing scalar handling for NUL, non-ASCII bytes, and short tails. Improves speedtest1 CTE with -Os. (leaf check-in: 5749b0489c ... user: jeffchen tags: opt-substr-ascii8) | 
| 22:05 |  | Merge fixes from branch-3.54 onto trunk. (check-in: 5a05aded7f ... user: drh tags: trunk) | 
| 21:33 |  | Prevent scalar subqueries from ever returning a value with a non-zero subtype. Report 2026-10-12T13:52:22Z. Re-added after the addition of a necessary prerequisite in the previous check-in. (check-in: 8ab55e0185 ... user: drh tags: branch-3.54) | 
| 21:24 |  | An unknown function can be seen in exprNodeCanReturnSubtype(), but only if SQLite is compiled with SQLITE_ENABLE_UNKNOWN_SQL_FUNCTIONS. Adjust asserts accordingly. (check-in: dcf20611cd ... user: drh tags: branch-3.54) | 
| 20:12 |  | Failed to increase the memory allocation size as required in the previous check-in. Fixed here. (check-in: 7b827c44ab ... user: drh tags: branch-3.54) | 
| 20:09 |  | Improvements to [313ab6c4e517e599] to address the concern raised by Bug comment 2026-10-12T17:03:33Z. (check-in: 51f69864e7 ... user: drh tags: branch-3.54) | 
| 18:20 |  | Avoid leaking pages when remapping a database file on unix. Forum post 3cfba36128. (check-in: 90d3c595af ... user: dan tags: branch-3.54) | 
| 17:52 |  | Temporarily back out the penultimate check-in on this branch in order to prevent an assertion fault. Need to reconsider this approach in light of Bug 2026-05-18T14:04:01Z. That other bug might be fixed incorrectly, and might also need to be backed out. Both bugs reopened until we can complete the analysis. (check-in: 163413d8e1 ... user: drh tags: branch-3.54) | 
| 16:29 |  | Fix a problem causing "aggregate( t1.col1 ORDER BY t2.col2 )" where col1 and col2 are in the same positions in their tables to be treated as "aggregate( t2.col2 ORDER BY t2.col2 )". Report 2026-10-12T13:31:40Z. (check-in: ffe3f056f1 ... user: dan tags: branch-3.54) | 
| 16:08 |  | Prevent scalar subqueries from ever returning a value with a non-zero subtype. Report 2026-10-12T13:52:22Z. (check-in: af3c900e2b ... user: dan tags: branch-3.54) | 
| 15:11 |  | Fix a harmless compiler warning in [7f154192db5a3a79]. (check-in: b3fcf7aecf ... user: drh tags: branch-3.54) | 
| 14:05 |  | Avoid ever repreparing a statement if a call to sqlite3_bind_int64() does not actually change the bound value. (check-in: 4708cfe5f7 ... user: dan tags: branch-3.54) | 
| 13:44 |  | Add test case for 2026-10-11T16:09:34Z. Bug was already fixed by [7f154192db]. (check-in: 085dca9d30 ... user: dan tags: branch-3.54) | 
| 13:36 |  | Avoid excessive reprepares when there is an unindexed "col=?" constraint on a join query. This should only be reprepared when the bound value toggles between integer values 0 or 1 and any other value. Report 2026-10-11T16:06:46Z. (check-in: 7f154192db ... user: dan tags: branch-3.54) | 
| 12:51 |  | Merge recent branch-3.54 fixes into trunk. (check-in: a9d8cddccc ... user: drh tags: trunk) | 
| 12:48 |  | When configuring with --editline, be sure to define HAVE_EDITLINE. Addresses forum post 2026-10-12T12:33:03Z. (check-in: af570c8fc2 ... user: stephan tags: branch-3.54) | 
| 12:44 |  | When configuring with --editline, be sure to define HAVE_EDITLINE. Addresses forum post 2026-10-12T12:33:03Z. (check-in: 85adb10e22 ... user: stephan tags: trunk) | 
| 11:42 |  | Improved detection of complete input in the CLI. Bug 2026-10-12T09:57:26Z (check-in: 313ab6c4e5 ... user: drh tags: branch-3.54) | 
| 10:24 |  | Do not use an index expression to evaluate a constant expression, as that uses unnecessary disk access and might actually give incorrect results if the expression was moved outside of the indexed loop. The error condition arose in check-in [44200596aa943963] for version 3.41.0 and was first reported 3.5 years later by Bug 2026-10-11T19:26:40Z. (check-in: fee60d93e9 ... user: drh tags: branch-3.54) | 
| 09:49 |  | Minor hardening of the fossil delta extension against malformed deltas. Based on suggestions from Dan Mestas. (check-in: 4528f00d5a ... user: stephan tags: trunk) | 
| 09:36 |  | Update test/fossildelta.sql for output expectation changes. (check-in: 41e0466cb8 ... user: stephan tags: trunk) | 
| 07:54 |  | Fix a minor typo in a comment. (check-in: 6c343ae1fc ... user: drh tags: branch-3.54) | 
| 07:37 |  | Merge fixes from branch-3.54 (check-in: 7b86d92cf5 ... user: drh tags: trunk) | 
| 2026-10-11 |  |  | 
| 23:06 |  | Do not allow the json_valid() routine to use a cached (and possibly edited) copy of the JSON. It needs to parse the JSON for itself. Bug 2026-10-11T18:32:23Z. (check-in: ba74c33b6a ... user: drh tags: branch-3.54) | 
| 18:38 |  | Another fix to the order-by-subquery optimization of check-in [7a0cdc7edb704a88]. Trouble reported by Bug 2026-10-11T17:00:56Z. (check-in: 1b574df8c7 ... user: drh tags: branch-3.54) | 
| 17:44 |  | Fix a problem in the query flattener associated with RIGHT JOIN, as reported by Bug 2026-10-11T16:49:00Z. This problem surfaced after the fix to Forum post 2025-08-21T14:14:03Z at check-in [e0beda7ffe92abf6]. Before that, the query would not parse. (check-in: e6e27e37eb ... user: drh tags: branch-3.54) | 
| 2026-10-10 |  |  | 
| 19:19 |  | Merge branch-3.54 fixes into trunk. (check-in: dc388e83b0 ... user: drh tags: trunk) | 
| 19:11 |  | Back out a minor performance optimization associated with sqlite3VdbeMakeReady() as it has been found to not work correctly on some 32-bit builds. Bug 2026-10-10T18:05:46Z (check-in: 6ebd1e6948 ... user: drh tags: branch-3.54) | 
| 18:40 |  | In the CLI, disallow any use of the fileio extension in --safe mode. Bug 2026-10-10T13:15:05Z (check-in: e3e5ef4e17 ... user: drh tags: branch-3.54) | 
| 18:06 |  | Fix a vec1 problem where rounding errors when assigning vectors to centroids could cause delete operations to fail. (check-in: aab150f4b3 ... user: dan tags: trunk) | 
| 17:40 |  | New extension ext/misc/optname.c provides the canonical mapping between optimization bitmaps and optimization names. This gives a single point of edit when there are changes. Used by the CLI, by testfixture, and by TH3. (check-in: 135ed9e2cc ... user: drh tags: trunk) | 
| 16:05 |  | Update vec1 integrity-check method to tolerate small rounding errors when checking that a vector has been assigned to the correct centroid. (check-in: 54e6ae3ae6 ... user: dan tags: trunk) | 
| 12:14 |  | Minor refinements to the TreeTracker (aka "JoinMiss") optimization. (check-in: c1ad888eec ... user: drh tags: trunk) | 
| 00:11 |  | Add comments to help remind developers to keep the table of optimization disable bit values in sync with the CLI and with TH3. (check-in: 0e707e7dba ... user: drh tags: trunk) | 
| 2026-10-09 |  |  | 
| 23:18 |  | The code for sqlite3_analyzer is still in the tree, and you can still build it using "make sqlite3_analyzer" but it does not get built as part of "make tool-zip" and there is no mention of it in the README files and similar. (check-in: 77278079d9 ... user: drh tags: trunk) | 
| 21:05 |  | Add the tree-tracker optimization. (check-in: e24a8fe986 ... user: drh tags: trunk) | 
| 19:12 |  | Fix some problems with handling corrupt meta-value arrays in vec1. (check-in: fa41e62016 ... user: dan tags: trunk) | 
| 18:44 |  | Bring the tree-tracker branch up-to-date with trunk. (closed check-in: e5d0f76059 ... user: drh tags: tree-tracker) | 
| 18:34 |  | Improved error handling in the ".import" command of the CLI. Forum post 2026-10-09T17:57:07Z. (check-in: 48fedacea7 ... user: drh tags: branch-3.54) | 
| 18:11 |  | Bump the version number to 3.55 to begin the next development cycle. (check-in: 0dc7d0aaf5 ... user: drh tags: trunk) | 
| 18:11 |  | Start a new branch to receive patches for the 3.54 release. Put all bug fixes that will go into a patch release on this branch. The plan it to periodically merge the branch into trunk, rather than cherrypicking from trunk to branch as has been done before. (check-in: 6b286a1bb0 ... user: drh tags: branch-3.54) | 
| 15:46 |  | Version 3.54.0 (check-in: be8d059e9a ... user: drh tags: trunk, release, major-release, version-3.54.0) | 
| 14:40 |  | Fix a typo in a comment. No changes to code. (check-in: 7b468f6f1d ... user: drh tags: trunk) | 
| 11:47 |  | In the CLI, add comments to internal SQL functions making it clear that those functions are subject to change and removal at any time, lest people come to depend on them. No changes to code. (check-in: a3332bd623 ... user: drh tags: trunk) | 
| 11:22 |  | Fix a build script problem causing generated file sqlite3-all.c to #include "sqlite-10.c" instead of "sqlite-A.c". (check-in: 962b207123 ... user: dan tags: trunk) | 
| 2026-10-08 |  |  | 
| 23:59 |  | Remove the OP_IfNotOpen opcode. The OP_IfNullRow opcode is sufficient to get the job done. This is a follow-up to [e71b0465b9fa13db]. (check-in: 797081cb70 ... user: drh tags: trunk) | 
| 19:34 |  | Ensure the join flags on the parent are set correctly when flattening UNION ALL sub-queries that include branches with RIGHT joins. Report 2026-10-08T15:27:33Z. (check-in: cfa69ea48a ... user: dan tags: trunk) | 
| 19:06 |  | Disable the "expr IN (const)" to "expr=const" optimization of "const" has a COLLATE clause, as using the collating on "=" would give the wrong answer. Bugs 2026-10-08T12:36:41Z (check-in: 53a73aa03d ... user: drh tags: trunk) | 
| 18:34 |  | Improvements to the display of DDL SQL statements in the CLI and in the sqldiff command-line tool in order to close comments and similar in the case of a corrupt schema, and avoid possible SQL injection attacks. ANT-2026-9VFH4DZS. (check-in: 028a92f9c3 ... user: drh tags: trunk) | 
| 17:57 |  | Avoid clearing unrelated join related expression flags when processing window function queries. Report 2026-10-08T15:38:26Z. (check-in: c50d7f682c ... user: dan tags: trunk) | 
| 16:17 |  | Fix test script errors causing problems on windows. (check-in: 379c017e8f ... user: dan tags: trunk) | 
| 13:26 |  | Make the sqlite3_incomplete() SQL function available in debugging builds of the CLI, for testing. (check-in: 07095b76c1 ... user: drh tags: trunk) | 
| 12:09 |  | Disable legacy "go" and "/" command terminator options in the CLI unless compiled with -DSQLITE_SHELL_LEGACY_COMMAND_TERMINATOR. ANT-2026-FE9980M1 and ANT-2026-G9MMWYGM. (check-in: ac6ac0c22d ... user: drh tags: trunk) | 
| 10:55 |  | Sanitize "*/" in tables names of the output of the .diskused command in the CLI, and in sqlite3_analyzer, so that it doesn't prematurely close the comment. ANT-2026-XQM1QD48 (check-in: bcde8f991a ... user: drh tags: trunk) | 
| 09:57 |  | Update check-in [e71b0465b9fa13db] so that it does not try to to generate an OP_NullRow opcode for cursors of an IN operator that do not actually exist. (check-in: ea315175a8 ... user: drh tags: trunk) | 
| 03:28 |  | Assign the correct encoding to the BLOB values read from an overflow page. Bug 2026-10-07T15:44:40Z (check-in: a8895cb047 ... user: drh tags: trunk) | 
| 02:55 |  | Fix processing of LEFT JOIN when the right operand is controlled by an IN loop. Bug 2026-10-07T23:22:09Z. (check-in: e71b0465b9 ... user: drh tags: trunk) | 
| 2026-10-07 |  |  | 
| 23:07 |  | Improved escaping of filenames with unusual characters sent over the SSH link by the sqlite3_rsync utility program. ANT-2026-31X9HVRR. (check-in: eaec2f7f37 ... user: drh tags: trunk) | 
| 21:12 |  | Mark vec1 training function vec1_train() as SQLITE_DIRECTONLY, on the grounds that it sometimes prepares and executes SQL statements based on user-supplied arguments. (check-in: 38c5972e91 ... user: dan tags: trunk) | 
| 17:05 |  | Include a work-around of the Microsoft "WorstFit" bug on Windows builds of the sqlite3_rsync utility program. ANT-2026-H5TXKR5J. (check-in: 2662a68f0d ... user: drh tags: trunk) | 
| 15:45 |  | Doc-only tweaks in main.mk. No build changes. (check-in: 2195709cd5 ... user: stephan tags: trunk) | 
| 12:50 |  | When calculating TCLLIBDIR from the configure script, filter out //zipfs: paths. Tcl9's (file isdir) returns true for those but they are not usable for our purposes, causing (make install DESTDIR...) to install tclsqlite3 under /zipfs. main.mk's counterpart to this piece already does that filtering by virtue of the shell's (test -d) behaving more conventionally. (check-in: bb109cfa9d ... user: stephan tags: trunk) | 
| 10:41 |  | Fix the expansion of "TABLE.*" so that it works the same as PostgreSQL when TABLE is the left operand of a RIGHT JOIN USING. Bug 2026-10-07T02:56:24Z (check-in: d232720b79 ... user: drh tags: trunk) | 
| 10:07 |  | In the treeview display for Expr, show TK_ASTERISK nodes. Debugging logic only - not used in production builds. (check-in: 5cf4df16eb ... user: drh tags: trunk) | 
| 2026-10-06 |  |  | 
| 18:52 |  | Fix the implementation of O_TMPFILE so that it actually works. (check-in: a435d786fe ... user: drh tags: trunk) | 
| 14:34 |  | In the CLI, further improvements to delayed temp-file deletion so that the test/output-redirect-test.txt script works even better. (check-in: adacd1423f ... user: drh tags: trunk) | 
| 11:49 |  | Fix to the time-delay temporary-file deletion logic in the CLI, added by [4e9ee211ca661fe8] but never fully debugged. Now working with the test/output-redir-test.txt test script. (check-in: 9dd1e3a1c7 ... user: drh tags: trunk) | 
| 11:13 |  | Increase version number returned by vec1_info() to "0.8". Also fix some warnings in vec1.c. (check-in: 97014dfa82 ... user: dan tags: trunk) | 
| 10:30 |  | Fix a case causing infinitely recursive calls to vec1.xSavepoint. (check-in: a6f0843000 ... user: dan tags: trunk) | 
| 09:30 |  | Fix the build for SQLITE_OMIT_WSD. (check-in: 4bf968095f ... user: drh tags: trunk) | 
| 2026-10-05 |  |  | 
| 22:16 |  | When a 3-byte cell on the leaf of an index page, which has a reported size of 4, is moved up to an intermediate page due to a DELETE, the 4-byte page number is added. The cell should should become 7, not 8. If it becomes 8, then the fragment count on the page header goes off by one. This is technically corrupt, but is easily fixed with VACUUM. Bug 2026-10-05T19:18:18Z. This issue has been in the code for at least 12.5 years. (check-in: a61987887a ... user: drh tags: trunk) | 
| 20:26 |  | OPFS SAHPool VFS: fix an improbable corner case in which the VFS's in-memory state and its on-disk state could differ with regards to availability of a given SAH if a write failure happens at one very specific point during de-registration of the SAH. Addresses bug report 2026-10-05T17:51:35Z. (closed check-in: 837fee6e45 ... user: stephan tags: wasm-post-3.54) | 
| 20:04 |  | Merge trunk into branch wasm-post-3.54. (check-in: de71a91870 ... user: stephan tags: wasm-post-3.54) | 
| 18:07 |  | Handle temp triggers attached to databases clobbered by sqlite3_deserialize() in the same way as those attached to DETACHed databases. (check-in: 84c773fcc2 ... user: dan tags: trunk) | 
| 17:51 |  | Ensure that deserializing into a database expires existing prepared statements. (check-in: de0361b1ca ... user: dan tags: trunk) | 
| 17:36 |  | Merge the latest trunk fixes into the tree-tracker branch, especially to seal in the cherrypick made necessary by me committing a trunk change to this branch earlier. (check-in: b06cac1d93 ... user: drh tags: tree-tracker) | 
| 17:32 |  | Fixes to sqlite3_incomplete() so that it does not appear to presume a trailing newline if there is none. Forum 2026-10-05T16:22:33Z (check-in: adc1113cfd ... user: drh tags: trunk) | 
| 17:27 |  | Fixes to sqlite3_incomplete() so that it does not appear to presume a trailing newline if there is none. Forum 2026-10-05T16:22:33Z (check-in: bc54517c86 ... user: drh tags: tree-tracker) | 
| 17:25 |  | Do not run new virtual table tests in memdb1.test for SQLITE_OMIT_VIRTUALTABLE builds. (check-in: eced240b2a ... user: dan tags: trunk) | 
| 17:11 |  | Allow windows clients to open and recover a wal mode database even if an old client still has the *-shm file mapped. Forum post 2026-10-05T14:07:31Z. (check-in: 7c103a0b7d ... user: dan tags: trunk) | 
| 15:20 |  | Omit the -r command-line option from Lemon. That does not affect SQLite. Forum 2026-10-05T15:11:11Z. (check-in: bc72b1d95c ... user: drh tags: trunk) | 
| 14:26 |  | Merge the latest trunk enhancements into the tree-tracker branch. (check-in: 5fa8b22cfb ... user: drh tags: tree-tracker) | 
| 14:22 |  | Update test cases so that they work for configuration that use -DSQLITE_MAX_EXPR_DEPTH=0. (check-in: 4bfc6e53a9 ... user: drh tags: trunk) | 
| 14:03 |  | Ensure that eponymous virtual table objects are not left with dangling schema pointers if the main database is overwritten using sqlite3_serialize(). Bug 2026-10-03T19:21:58Z. (check-in: 18d2646f6f ... user: dan tags: trunk) | 
| 13:27 |  | Correctly rollback "OR REPLACE" changes following a "datatype mismatch" error. Bug 2026-10-05T07:15:40Z. (check-in: 028f3f96a0 ... user: drh tags: trunk) | 
| 12:18 |  | Prevent the expression generated by the count-of-view optimization from getting too deep and causing a CPU stack overflow. Bug 2026-10-04T05:34:47Z (check-in: ab621a3e11 ... user: drh tags: trunk) | 
| 11:29 |  | Ensure that when a value that may be stored without data loss as an integer is stored in a column with REAL affinity, the values passed to the preupdate-hook are of type SQLITE_FLOAT, not SQLITE_INTEGER. Report 2026-10-04T16:18:21Z. (check-in: 9f7490065e ... user: dan tags: trunk) | 
| 10:54 |  | Ensure that it is not a constraint violation if a CHECK constraint that evaluates to NULL for one or more existing rows is added to a table using ALTER TABLE. Report 2026-10-05T07:36:38Z. (check-in: f940c67b21 ... user: dan tags: trunk) | 
| 10:41 |  | Raise a "nested too deep" error if the number of cursors consumed when resolving a CTE name exceeds SQLITE_LIMIT_EXPR_DEPTH. Bug 2026-10-04T05:35:14Z (check-in: e68aa6e129 ... user: drh tags: trunk) | 
| 09:32 |  | Limit the depth of recursion for CTEs using SQLITE_LIMIT_EXPR_DEPTH. Bug 2026-10-04T04:58:58Z. (check-in: f547308713 ... user: drh tags: trunk) | 
| 2026-10-04 |  |  | 
| 21:54 |  | Fix expression tree depth calculations for COLLATE. Bug 2026-10-04T04:50:56Z (check-in: 252c0b7bf5 ... user: drh tags: trunk) | 
| 19:51 |  | Improve support for --with-tcl in cross-compilation environments, as discussed in forum thread 2026-08-15T18:21:17Z. (check-in: d4330c79a4 ... user: stephan tags: trunk) | 
| 19:47 |  | When using --with-tcl and friends during cross-compilation, emit a warning that tclConfig.sh will not be used. Addresses forum post 2026-10-04T17:25:09Z. Normalize TCL to Tcl in nearby docs and help text because that's how tcl-lang.org types it. (closed check-in: 41f095e349 ... user: stephan tags: cygwin-msys-tclsh-forum-fe9e99eb) | 
| 18:24 |  | Merge trunk into this branch. (check-in: 40d58aa98d ... user: stephan tags: cygwin-msys-tclsh-forum-fe9e99eb) | 
| 16:23 |  | In QRF, add the zero-pad flag to the default floating-point format in output modes that render JSON or INSERT, so that infinities are shown as 9e999 or similar instead of as "Inf". Bug 2026-10-04T05:35:43Z (check-in: f63139ba68 ... user: drh tags: trunk) | 
| 15:41 |  | Avoid truncation of a 64-bit value into 32-bits when it is the N parameter to the geopoly_regular() function. (check-in: 13d8921bca ... user: drh tags: trunk) | 
| 15:23 |  | In the geopoly extension, any polygon with 16,777,216 or more vertexes is considered malformed. So the number of edges must be between 4 and and 16,777,215. Bug 2026-10-04T04:58:23Z. (check-in: e8b7f75e65 ... user: drh tags: trunk) | 
| 12:10 |  | Adapt wasm build for emcc's deprecation of-sMEMORY64=1 and -sWASM_BIGINT=1. Enable emcc -sASSERTIONS for fiddle at the suggestion of an emcc OOM error, as that flag makes such errors more informative. (check-in: 5ca28b4ba3 ... user: stephan tags: wasm-post-3.54) | 
| 10:49 |  | Make no changes to the busy timeout if the argument to "PRAGMA busy_timeout" is too large. Bug 2026-10-04T00:05:36Z. (check-in: e025d8fd33 ... user: drh tags: trunk) | 
| 2026-10-03 |  |  | 
| 15:54 |  | Fix some formatting issues in test program vec1bench.tcl. (check-in: da8069b877 ... user: dan tags: trunk) | 
| 15:29 |  | Fix a test case in rbuvacuum.test that was assuming SQLite ignored trailing SQL stored in a db schema. As of [e21c5bc59a] this is not true. (check-in: d47bc6e0d2 ... user: dan tags: trunk) | 
| 10:45 |  | Fix test cases added by [e21c5bc59ae66173] so that they work with in-memory configurations. (check-in: 85207e5253 ... user: drh tags: trunk) | 
| 09:53 |  | Enhance the sqlite3_incomplete() interface so that it returns SQLITE_EMPTY if the input consists purely of whitespace and comments. Fix bugs in the continuation prompt mechanism of the CLI discovered while making this enhancement. (check-in: 086e6fb47c ... user: drh tags: trunk) | 
| 2026-10-02 |  |  | 
| 20:18 |  | Update the CLI and the sqldiff utility program to use "COLLATE nocase" when comparing table names in sqlite_schema. Add the --unsafe flag to sqldiff so that it can attempt to process corrupt database files. Changes inspired by ANT-2026-YKVW6RS4. (check-in: 65ec11f05a ... user: drh tags: trunk) | 
| 19:56 |  | Do not allow surplus SQL text to come after the CREATE statement in the sqlite_schema.sql field. Any such text is flagged as an error. However, the schema will still work if PRAGMA writable_schema is engaged. (check-in: e21c5bc59a ... user: drh tags: trunk) | 
| 18:47 |  | Fix the "sqldiff" utility so that when using the --primarykey option on a table that has a PRIMARY KEY with redundant columns, it does not overflow the array of PRIMARY KEY column names. Bug discovered by Anthropic as ANT-2026-Y36XS4M7. (check-in: 8ffd0be079 ... user: drh tags: trunk) | 
| 14:33 |  | Fix integrity-check fails that would occur if the database contained a contentless fts4 table. Report 2026-10-02T12:22:03Z. (check-in: ca8de9a304 ... user: dan tags: trunk) | 
| 2026-10-01 |  |  | 
| 16:48 |  | Change the way the session module applies changesets containing two or more UPDATE changes that form a dependency loop in order to avoid triggering ON DELETE CASCADE actions. (check-in: 99aab2c992 ... user: dan tags: trunk) | 
| 11:51 |  | Merge the latest trunk enhancements into the tree-tracker branch (check-in: 38d22cae43 ... user: drh tags: tree-tracker) | 
| 11:21 |  | Import code for vec1 into ext/vec1/. (check-in: 92709f54d4 ... user: dan tags: trunk) | 
| 08:13 |  | SAHPool VFS tweaks contributed off-list by Jure Rotar: hardening against malicious inputs and I/O errors. Truncate to the shorter length when overwriting a db file with a shorter one via importDb(). Correct xOpen() to honor the combination of CREATE/EXCLUSIVE flags. (check-in: 7af745868b ... user: stephan tags: trunk) | 
| 2026-09-30 |  |  | 
| 20:59 |  | Remove an extraneous nearly-full-file level of indentation in ext/wasm/tester1.c-pp.js. (check-in: b838769b4b ... user: stephan tags: trunk) | 
| 19:29 |  | Improve locking support in the JS kvvfs. (check-in: bc5b3e391f ... user: stephan tags: trunk) | 
| 19:26 |  | Add tests demonstrating kvvfs locking errors across multiple connections to the same storage. Add sqlite3.oo1.Stmt.resetNoThrow() for reasons described in its API docs. (closed check-in: 0fed9bcbe2 ... user: stephan tags: kvvfs-locking) | 
| 18:40 |  | Rework the JS kvvfs xLock(), xUnlock(), xCheckReserved(), and xSleep() analog to the recent opfs-sahpool changes to make it more robust when a given kvvfs store is accessed via multiple handles (all necessarily from the same thread). This does not break anything but also does not prove anything - tests are pending. (check-in: d0e27fa55b ... user: stephan tags: kvvfs-locking) | 
| 17:50 |  | Fix a typo in the VERTICAL_SUM_GENERIC() macro in vec1.c. (closed check-in: b55faf3253 ... user: dan tags: vec1) | 
| 17:38 |  | For msvc, __AVX2__ implies __FMA__. (check-in: 2196a779fb ... user: dan tags: vec1) | 
| 17:07 |  | Fix a faulty assert() that was failing when running vec1 OOM tests. (check-in: aae6615269 ... user: dan tags: vec1) | 
| 16:41 |  | Fix problems in vec1rollback.test preventing it from running on windows. (check-in: d15cf38902 ... user: dan tags: vec1) | 
| 16:38 |  | Fix definition of VEC1_META_4BYTEMIN so that it works with msvc. (check-in: c9b4c9c0aa ... user: dan tags: vec1) | 
| 15:46 |  | [fa44643f65d6] inadvertently removed its circular-cleanup protection by fixing the finalizer ordering. This check-in fixes that. Spotted in forum post 2026-09-30T14:59:13Z. (check-in: 771faec269 ... user: stephan tags: trunk) | 
| 15:45 |  | [fa44643f65d6] inadvertently removed its circular-cleanup protection by fixing the finalizer ordering. This check-in fixes that. Spotted in forum post 2026-09-30T14:59:13Z. (check-in: fc3b85a1b7 ... user: stephan tags: branch-3.53) | 
| 14:49 |  | Import vec1 sources into directory ext/vec1/ of this project. (check-in: 3a0876d973 ... user: dan tags: vec1) | 
| 13:23 |  | Do not inherit the OPFS SAHPool's xSleep method from the default VFS, as doing so can cause the thread to freeze. This VFS only deals with contention originating in the same thread, so xSleep() needs to be a no-op. Addresses bug report 2026-09-30T12:52:54Z. (check-in: c9dd4d88e4 ... user: stephan tags: trunk) | 
| 13:18 |  | JS: when disposing of a C struct instance, do not unmap the 'pointer' property interceptor until after all client-added finalizers have been called. (check-in: fa44643f65 ... user: stephan tags: trunk) | 
| 11:48 |  | Fix the OPFS SAHPool VFS's xLock/xUnlock to properly deal with multiple handles to the same underlying file within the same JS thread. Addresses bug report 2026-09-30T10:49:15Z. Also (unrelated but code-adjacent) make sure that the VFS releases all of its internal mappings when it's paused. (check-in: 9e2caaa382 ... user: stephan tags: trunk) | 
| 11:05 |  | Incremental improvements. (check-in: aae085c5ac ... user: drh tags: tree-tracker) | 
| 10:36 |  | Minor JS doc correction. (check-in: 6b470917b2 ... user: stephan tags: trunk) | 
| 09:53 |  | Correct the xCheckReserveLock() impl of the OPFS SAHPool VFS to fix a corruption case involving multiple handles in the same JS thread to the same filename when one leaves a journal behind. Addresses bug report 2026-09-29T18:25:28Z. (check-in: 9168a6f1be ... user: stephan tags: trunk) | 
| 09:17 |  | Generic internal JS cleanups. No functional changes. Update the preprocessor for the ability to use C-style comments in directive arguments. (check-in: b2699eb967 ... user: stephan tags: trunk) | 
| 2026-09-29 |  |  | 
| 19:42 |  | Change the way the session module applies changesets containing two or more UPDATE changes that form a dependency loop in order to avoid triggering ON DELETE CASCADE actions. (closed check-in: 05acfd4dc9 ... user: dan tags: session-update-to-update) | 
| 18:49 |  | An early attempt to implement the tree-tracker optimization. (check-in: ead82958b3 ... user: drh tags: tree-tracker) | 
| 14:25 |  | Add the preserveOnInitFailure option to the opfs-sahpool VFS. (check-in: f2d3432320 ... user: stephan tags: trunk) | 
| 12:02 |  | Fix a harmless compiler warning in fts3ChecksumIndex(). (check-in: 44e0d3e2ad ... user: drh tags: trunk) | 
| 11:52 |  | Reset the nullRow flag on the pseudo-cursor on each iteration of the loop when the RHS of a LEFT JOIN is a multi-row VALUES clause. Fix for Bug 2026-09-29T00:41:09Z. Problem introduced by check-in [a120c9235f125e05] with a missed opportunity to fix the problem at check-in [8c0f69e0e4ae0a44]. (check-in: 8e59d3c0a4 ... user: drh tags: trunk) | 
| 08:31 |  | OPFS SAHPool: more cleanly handle an init failure. Add an init option which tells it not to wipe the storage if initialization fails. Based on the discussion and patch in forum post 5664cd4b. Minor doc updates in unrelated code. (closed check-in: 0560c3cc20 ... user: stephan tags: opfs-sahpool-forum-5664cd4b) | 
| 2026-09-28 |  |  | 
| 16:30 |  | Fix a duplicate test case identifier label in speedtest1.c. (check-in: 6f7db1d731 ... user: drh tags: trunk) | 
| 2026-09-27 |  |  | 
| 15:54 |  | Enable the --column-metadata configuration flag in the 'autoconf' build. It was previously disabled with a comment that it changed how sqlite3.c was generated, but that's not the case. Addresses bug report 2026-09-27T13:25:48Z. (check-in: 03bb7acfe8 ... user: stephan tags: trunk) | 
| 2026-09-26 |  |  | 
| 11:21 |  | Improvements to detection of invalid JSONB in the json_pretty() function. This is an enhancement, not a bug fix, as json_pretty() does not guarantee detection of invalid JSONB, as long as the invalid JSONB does not cause a memory error. Bug 2026-09-25T15:59:04Z. Test cases in TH3. (check-in: c1027e6aee ... user: drh tags: trunk) | 
| 11:01 |  | New assert()s added to demonstrate that calls to sqlite3_aggregate_context() within an xInverse() callback never return NULL. (check-in: d28a4b4167 ... user: drh tags: trunk) | 
| 09:51 |  | Never let the SQLITE_FP_PRECISION_LIMIT exceed 100 million. Bug 2026-09-26T07:06:12Z (check-in: ba6f5fb7b8 ... user: drh tags: trunk) | 
| 06:23 |  | Update some long-dormant JS test code for refactoring-/renaming breakage induced since it was last used. Remove a separate long-dormant JS demo which cannot work with the current code. (check-in: fc4e246dc2 ... user: stephan tags: trunk) | 
| 2026-09-25 |  |  | 
| 17:42 |  | Avoid dropping an error code in fts5 contentless-delete mode after parsing an %_docize record. Report 2026-09-25T15:52:10Z. (check-in: 52f84cfcdb ... user: dan tags: trunk) | 
| 16:35 |  | Have RBU refuse to update any table with GENERATED columns. Bug report 2026-09-25T10:43:02Z. (check-in: 2cde8fd0a2 ... user: dan tags: trunk) | 
| 15:12 |  | Fix 8 byte buffer overwrite that could occur when applying RBU updates to databases containing indexes with more than 16 columns. Report 2026-09-25T11:18:49Z. (check-in: 7b4fa1d8cd ... user: dan tags: trunk) | 
| 14:55 |  | Fix some cases where fts3 integrity-check was not detecting corrupt databases. Report 2026-09-25T11:18:11Z. (check-in: 9b23f99212 ... user: dan tags: trunk) | 
| 2026-09-24 |  |  | 
| 20:14 |  | Avoid a use-after-free problem in rtree triggered by an attacker manipulating the shadow table schema to ensure sqlite3_last_insert_rowid() returns 0 after certain INSERTs. Problem reported by Claude and Ada Logics. (check-in: 6a9e141565 ... user: dan tags: trunk) | 
| 17:09 |  | Avoid marking little-known extension function next_char() as SQLITE_INNOCUOUS by default. (check-in: 481a1df638 ... user: dan tags: trunk) | 
| 16:32 |  | Avoid potential buffer overwrite in debugging tool showwal when dealing with a wal file containing records with a very large number of fields. (check-in: bc7641d1f1 ... user: dan tags: trunk) | 
| 15:53 |  | Avoid leaving an fts3 porter tokenizer cursor in an inconsistent state following an OOM error. At the moment, this does not cause problems because fts3 always frees a tokenizer cursor following an OOM. But it might if some other code used the porter tokenizer implementation. (check-in: 75c1ee9de6 ... user: dan tags: trunk) | 
| 15:34 |  | Fix an off-by-one error in sqlite3GetUInt32(). The only user-visible effect of fixing this problem is that the error message emitted for a corrupt database has changed in one obscure case. (check-in: 7f0149c969 ... user: dan tags: trunk) | 
| 10:55 |  | Work around quirks in MSVC associated with [d046d5f4c6855]. (check-in: 18b6a43eb8 ... user: drh tags: trunk) | 
| 10:37 |  | Fix test case inaccuracy introduced by [4f4ca57309c82d9e]. (check-in: 1b097b30ac ... user: drh tags: trunk) | 
| 10:04 |  | In accordance with Unicode Corrigendum #9, allow characters U+fffe and U+ffff to pass through unchanged. Bug 2026-09-23T02:39:21Z. (check-in: 0f5ddd0041 ... user: drh tags: trunk) | 
| 09:34 |  | Avoid truncating the argument of flag pragmas to 8 bits. Bug 2026-09-23T02:42:23Z. (check-in: 9400baf192 ... user: drh tags: trunk) | 
| 00:18 |  | In date/time functions, the hour can be 24 as long as the minutes and seconds are zero. Bug 2026-09-23T02:44:48Z (check-in: 52eec02592 ... user: drh tags: trunk) | 
| 2026-09-23 |  |  | 
| 21:04 |  | The --escape mode for CSV should default to "off". Bug 2026-09-23T16:25:07Z (check-in: c356641fad ... user: drh tags: trunk) | 
| 14:00 |  | Fix a small potential buffer overwrite in the JS impl of kvvfs to resolve bug report 2026-09-23T10:50:46Z. Remove some adjacent dead code. (check-in: c43287d885 ... user: stephan tags: trunk) | 
| 12:27 |  | Minor adjustment to [987ba5a3dc5f2a22] to deal with a corner case involving the maximum integer. Bug 2026-09-23T03:07:00Z. (check-in: 4a66e328a0 ... user: drh tags: trunk) | 
| 12:15 |  | Improved comment on the previous check-in. (leaf check-in: 4ee6d72487 ... user: drh tags: large-realint-keys) | 
| 11:37 |  | An attempt to improve the handling of rowid lookups using very large floating point values. (check-in: 881da860d6 ... user: drh tags: large-realint-keys) | 
| 11:31 |  | Avoid a potential crash by checking the arguments to sqlite_expert_rem() (part of the "expert" extension). This function was normally only called internally, but a sufficiently determined attacker could also find a way. Report 2026-09-23T10:50:15Z. (check-in: 3529f81f53 ... user: dan tags: trunk) | 
| 11:12 |  | Do not allow the arguments of table-valued-functions to the left of a LEFT JOIN to refer to tables to the right of the LEFT JOIN. Fix for 2026-09-20T09:57:48Z. (check-in: e9e341013f ... user: dan tags: trunk) | 
| 11:06 |  | Handle correctly very large (or small) out-of-range column indexes passed to various fts5 auxiliary functions. Report 2026-09-23T02:40:15Z. (check-in: 55c634d96d ... user: dan tags: trunk) | 
| 10:06 |  | Preprocessor: remove more not-relevant-to-this-copy pieces and add support for C-style comments in directive arguments. (check-in: 5c321483e0 ... user: stephan tags: trunk) | 
| 09:34 |  | Change the behavior of rtree_i32 such that coordinate inputs that are outside the range of signed 32-bit integers are converted into the nearest representable signed 32-bit integer rather than just being truncated to 32-bits. The prior behavior of truncating is what programmers expected years ago, but apparently the new generation of programmers expects the values to be capped at the nearest representable value. We could argue about which is more "correct", but it seems easier to just do what th... (check-in: d046d5f4c6 ... user: drh tags: trunk) | 
| 01:29 |  | Fix a possible double-free in the (unused) sqlite3_intck_test_sql() test interface to the intck extension. Bug 2026-09-22T22:05:25Z (check-in: 3b1e5538da ... user: drh tags: trunk) | 
| 2026-09-22 |  |  | 
| 19:39 |  | Improved output when errors occur during ".dump" in the CLI. (check-in: c49c3890bb ... user: drh tags: trunk) | 
| 19:11 |  | Fix goofy condition in the previous check-in (check-in: 03e14c5002 ... user: drh tags: trunk) | 
| 17:12 |  | Improved capture and reporting of errors by the CLI. Bug 2026-09-21T02:55:55Z (check-in: 6698a7e187 ... user: drh tags: trunk) | 
| 10:30 |  | Fix calculation of the virtual table dbstat.pgoffset field for overflow pages. Report 2026-09-22T01:43:42Z. (check-in: 4f4ca57309 ... user: dan tags: trunk) | 
| 2026-09-21 |  |  | 
| 20:18 |  | Latest preprocessor for various cleanups and fixes. Duplicate the tester1 concurrency warning in the worker-based copy. (check-in: c9b4695ffa ... user: stephan tags: trunk) | 
| 17:12 |  | Fix potential buffer overreads in demo program ext/fts3/tool/fts3view.c. Bug 2026-09-21T13:50:52Z. (check-in: 90891e96c4 ... user: dan tags: trunk) | 
| 15:52 |  | Update sessionChangesetFindTable() so that it honors the contract defined in its header comment and always returns non-zero if the table is not found. (check-in: e6fc087f5a ... user: dan tags: trunk) | 
| 15:45 |  | Do not allow the arguments of table-valued-functions to the left of a LEFT JOIN to refer to tables to the right of the LEFT JOIN. Fix for 2026-09-20T09:57:48Z. (closed check-in: c6b035ceff ... user: dan tags: tvf-left-join-fix) | 
| 13:08 |  | Teach 'make libtcl' to honor the --disable-rpath configure flag. Addresses forum post 2026-09-21T10:24:56Z. (check-in: e911d84b39 ... user: stephan tags: trunk) | 
| 11:14 |  | Fix truncation of 64-bit integer values passed to the fts3 snippet() function. Report 2026-09-21T02:41:49Z. (check-in: b0919c9687 ... user: dan tags: trunk) | 
| 07:29 |  | Add new test cases for the sqlite3.oo1.Stmt.get() overloads which take both a client-specified type ID and an array or object target. (check-in: 24bddf2aff ... user: stephan tags: trunk) | 
| 2026-09-20 |  |  | 
| 20:17 |  | Adjustments to the substr() function so that it can accommodate ridiculous values for the second argument. Bug 2026-09-20T18:42:27Z (check-in: aadf42e021 ... user: drh tags: trunk) | 
| 2026-09-19 |  |  | 
| 17:20 |  | Add test cases to sessionchange2.test. (closed check-in: e90a05f56f ... user: dan tags: changeset-missing-table) | 
| 13:35 |  | Fix test cases due to improved error messages from this branch. (check-in: 3a2f7fc4c6 ... user: drh tags: changeset-missing-table) | 
| 11:10 |  | Merge test cases fixes from trunk (check-in: 02b47eb9b7 ... user: drh tags: changeset-missing-table) | 
| 11:07 |  | Fix test cases added by [f847298c8d424071] so that they work on all configurations. (check-in: 977e0faac7 ... user: drh tags: trunk) | 
| 10:58 |  | Fix new test cases added by [c77ff6089de242d8] so that they work on all configurations. (check-in: 78d3370d36 ... user: drh tags: trunk) | 
| 09:52 |  | Update sessionChangesetFindTable() so that it honors the contract defined in its header comment and always returns non-zero if the table is not found. (check-in: 72005af24d ... user: drh tags: changeset-missing-table) | 
| 09:24 |  | Ensure that the comments generated by the ".schema" command in the CLI are not terminated early by rogue SQL. (check-in: e6eef2cb8f ... user: drh tags: trunk) | 
| 08:28 |  | When binding blobs in tclsqlite3, use sqlite3_bind_blob64() because Tcl 9 allows byte arrays larger than 32-bits and the size argument being passed there is of type Tcl_Size. Discrepancy caught and patched by Jan Nijtmans. (check-in: edb5545003 ... user: stephan tags: trunk) | 
| 08:13 |  | Use newer, more compact JS syntax in a couple of places. No functional changes. (check-in: 0d747adadf ... user: stephan tags: trunk) | 
| 07:59 |  | Simplification to the 32-bit float rounding routines from the previous check-in. Also fix a test case from that check-in. (check-in: 78871391d5 ... user: drh tags: trunk) | 
| 01:54 |  | Fix 32-bit floating point (float) rounding in rtree so that it works correctly for subnormal values and for values near infinity. Bug 2026-09-19T00:36:32Z. (check-in: 3d5e101324 ... user: drh tags: trunk) | 
| 2026-09-18 |  |  | 
| 16:50 |  | Fix issues in support for STDCALL on 32-bit Windows builds. Bug 2026-09-15T09:23:20Z and Bug 2026-09-15T09:24:06Z. (check-in: 7e09ee42af ... user: drh tags: trunk) | 
| 16:30 |  | When constructing sqlite3.h from sqlite.h.in using the -useapicall option, make sure to tag all callback arguments with SQLITE_CALLBACK. Also omit the SQLITE_CALLBACK macro in comments. (closed check-in: 5f8621d2f6 ... user: drh tags: windows-stdcall) | 
| 15:52 |  | In the sqlite3ext.h file, the MSVC compiler is unable to figure out for itself that a varargs function must be CDECL, so we have to give it a hint. (check-in: 19f963a3b1 ... user: drh tags: windows-stdcall) | 
| 15:38 |  | Improved support for stdcall on 32-bit windows, for legacy. (check-in: e638307a51 ... user: drh tags: windows-stdcall) | 
| 08:00 |  | Minor internal API doc update. (check-in: 346c56e02c ... user: stephan tags: trunk) | 
| 07:56 |  | Performance improvements in some of the JS sqlite3.oo1 APIs, based on analysis and suggestions from Jure Rotar. (check-in: 8c6c586c3e ... user: stephan tags: trunk) | 
| 07:55 |  | Add a note to ext/wasm/tester1 about running tests in concurrent tabs. (check-in: 8ce64bed3b ... user: stephan tags: trunk) | 
| 2026-09-17 |  |  | 
| 13:30 |  | Portability improvements for Cygwin contributed by Jan Nijtmans. (check-in: 8dc536e845 ... user: stephan tags: trunk) | 
| 2026-09-16 |  |  | 
| 13:58 |  | Fix a problem causing fts5 auxiliary function API methods xRowCount() and xColumnTotalSize() to return inaccurate values after rows had been deleted from a contentless_delete=1 table. Forum post 5e6f15753a. (check-in: 39ca0ac365 ... user: dan tags: trunk) | 
| 08:31 |  | Typo fix in JS test code. (check-in: 4630ee80dc ... user: stephan tags: trunk) | 
| 2026-09-15 |  |  | 
| 18:11 |  | Add a JavaScript switch/case break which does not actually change any behavior but has the potential to (A) confuse someone into thinking it's broken or (B) become broken via future code changes. Spotted in a PR in the sqlite github repository. (check-in: a6351d0413 ... user: stephan tags: trunk) | 
| 10:52 |  | Resolve a 'Tcl_SetIntObj called with shared object' crash in the tcl db copy binding. Reported in the bugs forum on 2026-09-15 but the post was rejected, apparently by the OP, before it was published. (check-in: bcce90fdc1 ... user: stephan tags: trunk) | 
| 10:20 |  | Minor internal doc addition explaining a build flag for jimsh. (check-in: 8eff1b5cda ... user: stephan tags: cygwin-msys-tclsh-forum-fe9e99eb) | 
| 08:37 |  | Cygwin portability fixes contributed by Jan Nijtmans, including the un-exporting of some non-public-APIs which need to be reviewed closely. One test (delete_db.test) is failing but that specific failure has been a recurring theme in this environment. Edit: re. un-exporting of several win32 APIs, Jan points out that, as the maintainer of the Cygwin SQLite package, he's not heard any complaints about their removal, which suggests that this change will not break any clients on that platform. (Any w... (closed check-in: fe484c8dbc ... user: stephan tags: cygwin-portability-202609) | 
| 2026-09-12 |  |  | 
| 11:12 |  | When looking up a text key in a temporary index, use the fast memcmp-based comparison if the index uses the built-in BINARY collation. Indexes in the schema already got this fast path because they record BINARY as a NULL collation pointer. (check-in: f065790f59 ... user: drh tags: trunk) | 
| 10:43 |  | Configure-script patches from forum post fe9e99eb27c8 to address cygwin/msys cross-compilation issues related to --with-tcl and --with-tclsh. Stashing in a branch pending testing on those platorms. It does not adversely affect any native-build systems tested so far. (check-in: 4470bed69b ... user: stephan tags: cygwin-msys-tclsh-forum-fe9e99eb) | 
| 2026-09-11 |  |  | 
| 22:11 |  | When looking up a text key in a temporary index, use the fast memcmp-based comparison if the index uses the built-in BINARY collation. Indexes in the schema already got this fast path because they record BINARY as a NULL collation pointer. (closed check-in: ebb4534bc8 ... user: jeffchen tags: binary-collseq-fastpath) | 
| 2026-09-10 |  |  | 
| 23:54 |  | Do not discard INDEXED BY or NOT INDEXED from a parenthesized term of the FROM clause. Bug 2026-09-10T20:38:13Z. (check-in: e308b0fce4 ... user: drh tags: trunk) | 
| 20:04 |  | In PRAGMA integrity_check, double the space available for checking that every byte of a page is used only once as a safety margin to deal with unforeseen kinds of database corruption. (check-in: 0c173d54b1 ... user: drh tags: trunk) | 
| 10:20 |  | The value returned by json_each.root should be SQLITE_TRANSIENT, not SQLITE_STATIC. (check-in: 1d9bd6c8a2 ... user: drh tags: trunk) | 
| 2026-09-09 |  |  | 
| 13:49 |  | Fix minor issues with the delta_parse() table-valued function in the fossildelta.c extension. Add test cases for the fossildelta.c extension. Bug 2026-09-09T10:29:35Z and Bug 2026-09-09T10:55:54Z. (check-in: 737ab94632 ... user: drh tags: trunk) | 
| 2026-09-04 |  |  | 
| 14:20 |  | For "make test" on Windows, the --stop-on-error option is used for testrunner.tcl, which causes processing to do an orderly shutdown if any individual test module fails. (check-in: 4021369bc9 ... user: drh tags: trunk) | 
| 14:02 |  | Improved orderly-shutdown in testrunner. Windows lets running jobs complete. Unix just invokes exit and lets the OS deal with cleaning up the mess. (closed check-in: ce822e8dc1 ... user: drh tags: windows-testing) | 
| 13:43 |  | Remove an intentional error used for testing that was accidently left in the previous check-in. (check-in: 87f9d70d9e ... user: drh tags: windows-testing) | 
| 13:37 |  | The testrunner.tcl script is enhanced so that --stop-on-error does not exit immediately, but instead marks all pending tasks as "halt" so that no new tasks are scheduled, but allows "running" tasks to continue on to completion, as an immediate halt can cause hung processes on Windows. Also, in Makefile.msc, the "test" target now includes --stop-on-error. (check-in: 0fba9b52b4 ... user: drh tags: windows-testing) | 
| 12:39 |  | Fix the GetDiskTypeW() call in the previous check-in so that it works on x32. (check-in: ae1650228d ... user: drh tags: trunk) | 
| 2026-09-03 |  |  | 
| 19:56 |  | On Windows, use the GetDriveTypeW() interface to detect network filesystems and always use a single shared file handle for the -shm files on such filesystems. (check-in: 52ed3b490a ... user: drh tags: trunk) | 
| 18:37 |  | Enhance the WAL checkpoint algorithm to run faster and use less memory if the WAL file is larger than the database file. (check-in: 630ab4b6bd ... user: drh tags: trunk) | 
| 18:26 |  | Improve the documentation of the WalIterator object and the new pagemap algorithm. Remove one unnecessary conditional. (closed check-in: f690f5b148 ... user: drh tags: pagemap-checkpoint) | 
| 16:37 |  | Fix a bug in fts5 test code function fts5TestUtf8(). Report 2026-09-03T14:51:50Z. (check-in: ac094ec69b ... user: dan tags: trunk) | 
| 16:32 |  | Omit a minro optimization that fails in two ways: (1) it does not actually make the code run any faster, and (2) it allows an incorrect type coercion on a GROUP BY query where there is a subquery as one of the result set columns. Bug 2026-09-03T02:29:56Z. (check-in: 5d9e5cddec ... user: drh tags: trunk) | 
| 14:45 |  | When checkpointing, use a mapping from pgno to frame instead of the sorted list of frame/pgno pairs in cases where this will use less memory. (check-in: ec3d33931b ... user: dan tags: pagemap-checkpoint) | 
| 2026-09-02 |  |  | 
| 12:39 |  | When -DSQLITE_DEBUG or -DSQLITE_ENABLE_WALSTAT is used, provide the sqlite_walstat() SQL function that provides state information about the WAL subsystem. Used for debugging only. Function subject to change or removal. (check-in: 789d84e691 ... user: drh tags: trunk) | 
| 2026-09-01 |  |  | 
| 17:50 |  | Cygwin portability patches from Jan Nijtmans. (check-in: c76e64373c ... user: stephan tags: trunk) | 
| 17:44 |  | Within a checkpoint, avoid allocating memory for *-shm pages that will not be processed as they have already been completely checkpointed. (check-in: 53a9312cc9 ... user: dan tags: trunk) | 
| 16:26 |  | Cygwin portability patches sent off-list from from Jan Nijtmans. (closed check-in: f8306cd54d ... user: stephan tags: cygwin-portability) | 
| 12:36 |  | Make sure the extra sorting cursor used by SQLITE_ENABLE_SORTER_REFERENCES is initialized even if the corresponding table is empty. Bug 2026-09-01T01:34:02Z (check-in: a418005ba5 ... user: drh tags: trunk) | 
| 2026-08-31 |  |  | 
| 20:43 |  | Update the authorizer action code documentation to make it clear that string arguments to the authorizer callback can be in an arbitrary case. (check-in: 6f73383647 ... user: drh tags: trunk) | 
| 18:15 |  | Fix a false-positive SQLITE_CORRUPT error that could occur with an incremental vacuum database if many tables were created and dropped within a single wal-mode transaction. (check-in: f847298c8d ... user: dan tags: trunk) | 
| 15:18 |  | Fix a harmless compiler warning. (check-in: 40530cf21d ... user: drh tags: trunk) | 
| 15:02 |  | Return an error if the arguments of a table-valued function that is on the left of a RIGHT JOIN depend on tables to the right of it in the FROM clause. Report 2026-08-31T08:07:08Z. (check-in: 43c8d7c41c ... user: dan tags: trunk) | 
| 11:04 |  | Fix a bad interaction between the skip-ahead DISTINCT optimization and "ORDER BY ... NULLS FIRST/LAST" clauses. Report 2026-08-30T23:22:05Z. (check-in: 7bfd3d6f80 ... user: dan tags: trunk) | 
| 09:48 |  | In Lemon, remove the option to omit compression of the action table. (check-in: 9b75f0c346 ... user: drh tags: trunk) | 
| 2026-08-29 |  |  | 
| 20:27 |  | Follow-up to check-in [2cb349e5b02aa549] to help indexes on negative DEFAULT integer values get stored more efficiently. (check-in: c77ff6089d ... user: drh tags: trunk) | 
| 19:51 |  | Remove a redundant OP_RealAffinity inserted by recent changes to sqlite3ColumnDefault(). (check-in: c7a987a4f7 ... user: drh tags: trunk) | 
| 18:39 |  | Fix the CLI so that it does not capture ".echo" output as part of the result of a ".testcase"...".check". (check-in: 33322913a8 ... user: drh tags: trunk) | 
| 16:26 |  | Ensure that ALTER TABLE ADD COLUMN has zero return columns, even when altering a STRICT table. Test case in TH3. (check-in: 778e2bb9a6 ... user: drh tags: trunk) | 
| 13:42 |  | Fix harmless comment typo. (check-in: 66f608ceca ... user: drh tags: trunk) | 
| 2026-08-27 |  |  | 
| 18:07 |  | Fix a case in the fts5 matchinfo() demo function causing it to return the wrong error code. (check-in: 5251f4d707 ... user: dan tags: trunk) | 
| 14:26 |  | Built-in SQL function oom() invokes sqlite3OomFault() and then returns NULL, but only if compiled with SQLITE_DEBUG or SQLITE_BUILTIN_OOM_FUNCTION. (check-in: 90dfac7f27 ... user: drh tags: trunk) | 
| 10:58 |  | Fix a C90-compliance issue in the CLI. (check-in: 555f31c64d ... user: drh tags: trunk) | 
| 10:31 |  | Enhance the authorizer so that it invokes the callback for the use of SQL functions in the DEFAULT clause of a CREATE TABLE. It does this both with the CREATE TABLE statement is first used, and whenever the DEFAULT expression is used, but not when the schema is being reparsed. (check-in: 4b30d1d7e2 ... user: drh tags: trunk) | 
| 10:16 |  | Update a stale comment. Fix minor #ifdef issues. (closed check-in: 2959ec249b ... user: drh tags: authorize-default) | 
| 10:01 |  | Fix the ext/misc/compress.c extension such that it compiles independently of the CLI. (check-in: 4cd5ecc5b3 ... user: drh tags: trunk) | 
| 09:44 |  | Minor enhancements to the built-in ".mode" documentation in the CLI. (check-in: b3a034a82d ... user: drh tags: trunk) | 
| 01:42 |  | Performance optimizations and code simplifications. (check-in: cbf2e796e0 ... user: drh tags: authorize-default) | 
| 2026-08-26 |  |  | 
| 21:04 |  | Split the sqlite3ColumnExpr() routine out into two routines, one that works as before and the second sqlite3ColumnExprAuth() to deal with the authenticator in places where that is needed. (check-in: 51a1149bdc ... user: drh tags: authorize-default) | 
| 19:37 |  | An attempt to make the authorizer effective at preventing the use of some functions within the DEFAULT clause of a table. (check-in: 321e41b091 ... user: drh tags: authorize-default) | 
| 18:21 |  | Add the --deny and --ignore options to the ".auth" command in the CLI. (check-in: da1e7d73c6 ... user: drh tags: trunk) | 
| 15:23 |  | The sqlite3ExprListIsConstant() routine probably should not return true if the list contains a function like random(). Fixed here. Simplify the logic used to test if expressions are constant. (check-in: 753f42a092 ... user: drh tags: trunk) | 
| 12:31 |  | Enhance the tmstmpvfs.c extension so that it tags zero pages in the log file. (check-in: e9d649b77b ... user: drh tags: trunk) | 
| 2026-08-25 |  |  | 
| 18:40 |  | Improved handling of OOM conditions in the sessions extension. Bug 2026-08-25T17:04:14Z. (check-in: ce2aa9f849 ... user: drh tags: trunk) | 
| 18:28 |  | Add assert() statements and comments to explain why the return value of sqlite3_column_name() does not need to be checked in fts3.c. Repor 2026-08-25T17:08:22Z. (check-in: 08b34b1656 ... user: dan tags: trunk) | 
| 18:26 |  | Avoid a possible NULL pointer dereference in the QRF sublibrary when compiled with SQLITE_ENABLE_STMT_SCANSTATUS. (check-in: 58014b2a2e ... user: drh tags: trunk) | 
| 18:16 |  | Improved handling of OOM errors in the Windows implementation of the sqlite3_stdio.c extension. Bug 2026-08-25T16:55:53Z. (check-in: 555489e4c2 ... user: drh tags: trunk) | 
| 17:59 |  | Prevent some potential NULL pointer deferences following misuse of the non-standard amatch extension. Bug 2026-08-25T16:47:32Z. (check-in: 99b69371a1 ... user: drh tags: trunk) | 
| 17:12 |  | New optimizations for improved performance of UNION with LIMIT 1, with or without ORDER BY. (check-in: b5b8ede613 ... user: drh tags: trunk) | 
| 17:08 |  | Fix two test cases that do not work when optimizations are disabled. (closed check-in: e78b72fadf ... user: drh tags: union-to-union-all) | 
| 16:29 |  | Make the optimization of UNION with ORDER BY and LIMIT sound by restricing it to LIMIT 1. (check-in: 7e6e665d05 ... user: drh tags: union-to-union-all) | 
| 16:15 |  | Fix OOM handling in little-used extension file "expert.c". Report 2026-08-25T16:02:31Z. (check-in: c89f4f7967 ... user: dan tags: trunk) | 
| 15:49 |  | Merge the latest trunk enhancements into the union-to-union-all branch. (check-in: c58631b26a ... user: drh tags: union-to-union-all) | 
| 14:27 |  | Update the Lemon documentation with clarifications regarding the %fallback and %wildcard directives. (check-in: f3ad1b4394 ... user: drh tags: trunk) | 
| 14:26 |  | When an expression that may have a subtype from the WHERE clause of a partial index is substituted for a column value, clear the subtype before the value is used. Report 2026-08-25T05:36:33Z. (check-in: e6ae6fbd5c ... user: dan tags: trunk) | 
| 11:50 |  | Fix test file default.test so that it works with SQLITE_OMIT_ALTERTABLE builds. (check-in: ff0e3dd094 ... user: dan tags: trunk) | 
| 11:42 |  | Avoid coding WHERE constraints too early when using the OR-optimization with OUTER join queries. Report 2026-08-25T02:08:22Z. (check-in: a3d15e4937 ... user: dan tags: trunk) | 
| 10:39 |  | Change error message text in the previous check-in to not have initial caps, for consistency with other error messages. (check-in: c4bd2ffcb7 ... user: drh tags: trunk) | 
| 2026-08-24 |  |  | 
| 21:20 |  | Prohibit using ALTER TABLE to add or drop, or rename to or from a rowid alias, unless the table is a WITHOUT ROWID table. Report 2026-08-23T15:59:23Z. (check-in: dfcff5be53 ... user: dan tags: trunk) | 
| 18:55 |  | Avoid a technically undefined signed integer overflow in fts5. Report 2026-08-23T16:25:44Z. (check-in: cde23480be ... user: dan tags: trunk) | 
| 18:24 |  | Fix some fts5 cases where all columns are excluded by a column filter (so that no rows match). Report 2026-08-23T16:25:44Z. (check-in: 7c9b711fc8 ... user: dan tags: trunk) | 
| 17:29 |  | Ensure that the fts5_vocab.term column behaves as if it has blob affinity for comparisons. Report 2026-08-23T16:25:44Z. (check-in: a832bc1ef2 ... user: dan tags: trunk) | 
| 17:08 |  | Fix a problem with combining index=none and secure-delete modes causing full-text index entries to be sometimes lost when inserts and deletes on the same token are combined in the same transaction. Report 2026-08-23T16:25:44Z. (check-in: e1f939f473 ... user: dan tags: trunk) | 
| 16:19 |  | Fix harmless compiler warning. (check-in: 0c5f5b37ce ... user: drh tags: trunk) | 
| 14:40 |  | Ensure that when a large integer value (too large to be losslessly converted to double) is specified as a default value for a column with REAL affinity, the real version of the value is stored in indexes. Report 2026-08-23T15:39:59Z. (check-in: 2cb349e5b0 ... user: dan tags: trunk) | 
| 14:30 |  | When generating code to evaluate a vector IN constraint where the RHS of the IN operator is a subquery, if the removeUnindexableInClauseTerms() subroutine changes the order of terms in the IN operator, then do not allow the modified IN operator to reuse a previously generated evaluation of the subquery. Bug 2026-08-23T06:58:36Z. (check-in: 8ee92456eb ... user: drh tags: trunk) | 
| 14:22 |  | Ensure that when a large integer value (too large to be losslessly converted to double) is specified as a default value for a column with REAL affinity, the real version of the value is stored in indexes. Report 2026-08-23T15:39:59Z. (closed check-in: c494b123f5 ... user: dan tags: bigint-defaults) | 
| 09:56 |  | Remove a harmless unused variadic argument in sqlite3_rsync. Bug report 2026-08-24T01:22:51Z. (check-in: 5231ac428d ... user: stephan tags: trunk) | 
| 00:08 |  | The optimization of disabling an ORDER BY term if it is constrained by an equality comparison with a column from an outer query does not work if there is an affinity mismatch between the two columns. This is a follow-on to check-in [16aed5d0c63dcdc2] and a fix for Bug 2026-08-23T01:02:14Z. (check-in: 383e42bc10 ... user: drh tags: trunk) | 
| 2026-08-23 |  |  | 
| 21:41 |  | Correctly enforce a UNIQUE constraint on a non-PRIMARY KEY column of a table that has an INTEGER PRIMARY KEY with ON CONFLICT REPLACE and that has a AFTER DELETE trigger that fires when the REPLACE happens and that trigger tries to INSERT a new row that violates uniqueness. Bug 2026-08-22T23:55:15Z. (check-in: 521c9f5f28 ... user: drh tags: trunk) | 
| 2026-08-22 |  |  | 
| 19:27 |  | Enhance sqlite3_bind_int64() so that it never triggers a reprepare if the value does not actually change. (check-in: db0cb462aa ... user: drh tags: trunk) | 
| 17:56 |  | Fix incorrect termination of varargs argument to pickStr() in check-in [852972861636d75e] from yesterday. (check-in: ad7d15323b ... user: drh tags: trunk) | 
| 16:33 |  | Remove some unnecessary sqlite3_test_control() optimization control flags for a small performance increase and size reduction. (check-in: 2a9cb1e12a ... user: drh tags: trunk) | 
| 15:56 |  | Improved use of LIMIT in UNION operators. (check-in: 7304e5aeaf ... user: drh tags: union-to-union-all) | 
| 14:41 |  | New optimization bit SQLITE_UnionLimit. (check-in: 5890dbe54d ... user: drh tags: union-to-union-all) | 
| 11:10 |  | Improved testability. (check-in: e8e1ce6dae ... user: drh tags: union-to-union-all) | 
| 10:39 |  | Add an optimization that converts UNION into UNION ALL if there is a LIMIT 1 and no ORDER BY. (check-in: fb83742dd1 ... user: drh tags: union-to-union-all) | 
| 09:33 |  | Fix check-in [468e730036edac22] so that it works with DESC indexes. Bug 2026-08-22T07:01:45Z. (check-in: d1f4fa43f3 ... user: drh tags: trunk) | 
| 01:21 |  | A couple of fixes, one of them important, for the previous check-in. (check-in: 3a01099c45 ... user: drh tags: trunk) | 
| 2026-08-21 |  |  | 
| 20:32 |  | Faster implementation of sqlite3_bind_int64(). Reduce the need to reprepare when replacing one integer binding with a different integer value is many cases. (check-in: 98edd4ac22 ... user: drh tags: trunk) | 
| 20:21 |  | Refactor. 2M cycles faster on x64. (closed check-in: 2b3e73200b ... user: drh tags: reduce-reprepares) | 
| 14:04 |  | In the CLI, finer-grain control of what status information is displayed via the ".stats" command. Add the ability to reset peak measurements after each display. (check-in: 8529728616 ... user: drh tags: trunk) | 
| 00:31 |  | Fix a harmless compiler warning in the non-deliverable normalize.c extension. Bug 2026-08-20T23:33:57Z. (check-in: 11a73d18be ... user: drh tags: trunk) | 
| 2026-08-20 |  |  | 
| 16:47 |  | Fix a faulty assert() in lemon, as reported at Forum post 2026-08-20T14:18:08Z. (check-in: 0672f6f074 ... user: drh tags: trunk) | 
| 15:59 |  | Remove a literal 0x0d character from a sessions test script, using char(0x0d) in its place, to avoid false-positive mixed-line-ending warnings from Fossil. (check-in: d0e4714aff ... user: drh tags: trunk) | 
| 14:05 |  | Merge the latest trunk enhancements into the reduce-reprepares branch. (check-in: 62ad88fe1c ... user: drh tags: reduce-reprepares) | 
| 13:25 |  | Performance enhancements (inlining and partial inlining) in the jsonbPayloadSize() routine of the JSON logic saves a little more than 10 million cycles (0.6%) on the standard benchmark (gcc 13.3, -Os, mint linux) while using less than 700 extra bytes of code space. (check-in: 3bd5456e50 ... user: drh tags: trunk) | 
| 13:12 |  | Use testcase() macros to prove coverage in jsonbPayloadSizeWide() (closed check-in: cff777eeb9 ... user: drh tags: opt-inline-jsonbPayloadSize) | 
| 11:50 |  | Latest upstream libcmpp.c for #eval and #include -resolve, and removal of the clumsy @'string' literals. (check-in: dc176e4335 ... user: stephan tags: trunk) | 
| 11:12 |  | Internal doc typo fix. No code changes. (check-in: 1c19bc27cb ... user: stephan tags: trunk) | 
| 10:24 |  | Fix typo in comment. No code changes. (check-in: da3cb2b23e ... user: drh tags: opt-inline-jsonbPayloadSize) | 
| 10:08 |  | Revise the OP_Int64, OP_Real, and OP_ColumnsUsed opcodes to split their 64-bit argument across two ordinary 32-bit arguments rather than storing the 64-bit value as a separate memory allocation. This gives a small performance boost and size reduction, and makes the code a little easier to maintain. (check-in: 83e72cc862 ... user: drh tags: trunk) | 
| 10:03 |  | Fix a comment typo and add back a missing assert(). (closed check-in: 63de2f92ab ... user: drh tags: split-64bit-opcodes) | 
| 09:41 |  | Add a comment on the OP_ColumnsUsed operator. (check-in: 8b3a559994 ... user: drh tags: split-64bit-opcodes) | 
| 09:26 |  | Fix the order of 32-bit works on an OP_ColumnsUsed operator. (check-in: 0433f33e02 ... user: drh tags: split-64bit-opcodes) | 
| 01:27 |  | Fix a harmless compiler warning. (check-in: 4278ee631a ... user: drh tags: trunk) | 
| 01:22 |  | Modify the OP_Int64 and OP_Real opcodes so that their 64-bit immediate value is stored split across to arguments, P1 and P3, rather than being stored as a separate memory allocation pointed to by P4. Slightly smaller and slightly faster, but perhaps also easier to maintain. (check-in: c13b4c2c41 ... user: drh tags: split-64bit-opcodes) | 
| 2026-08-19 |  |  | 
| 21:01 |  | Disable three unimportant scanstatus tests on Windows to work around platform incompatibilities. Bug 2026-08-19T20:22:44Z. (check-in: 3f2b5dc6cd ... user: drh tags: trunk) | 
| 17:25 |  | Merge recent trunk enhancements into the reduce-reprepares branch. (check-in: f3adcd561a ... user: drh tags: reduce-reprepares) | 
| 15:29 |  | Fix the zipvfs virtual table so that it works with UPDATE OR REPLACE. Bug 2026-08-19T01:52:09Z. (check-in: 1f71a577ad ... user: drh tags: trunk) | 
| 15:06 |  | In the (test and debug use only) qpvtab extension, terminate each string at the first 0x00 character. Bug 2026-08-19T02:29:00Z. (check-in: 15192e7a4f ... user: drh tags: trunk) | 
| 14:27 |  | Detect and report FTS5 index corruption early enough to prevent CPU stack overflow. Bug 2026-08-19T01:42:02Z. (check-in: 663aa876b5 ... user: drh tags: trunk) | 
| 13:01 |  | Fix the (test and debug use only) vtablog.c extension so that it avoids NULL pointer dereferences following an OOM fault. Bug 2026-08-19T02:38:14Z. (check-in: 33296ed1d6 ... user: drh tags: trunk) | 
| 11:59 |  | Limit the depth of recursion in the internal clearDatabasePage() routine. Raise a database corruption error if it goes too deep. Bug 2026-08-19T02:05:18Z. (check-in: 4a7b12914e ... user: drh tags: trunk) | 
| 11:15 |  | In Lemon, do not invoke qsort() with a NULL array pointer even if the number of elements is zero. That is harmless in practice, but is technically undefined behavior. This change does not affect SQLite. Bug 2026-08-19T05:56:54Z. (check-in: fc3c471a7e ... user: drh tags: trunk) | 
| 10:38 |  | Fix a minor error in [d287358b8d356d4f] that was causing an assertion fault. (check-in: 1357ac105a ... user: drh tags: trunk) | 
| 2026-08-18 |  |  | 
| 23:04 |  | Merge the latest trunk enhancements into the opt-inline-jsonbPayloadSize branch. (check-in: 88fbeafc8d ... user: drh tags: opt-inline-jsonbPayloadSize) | 
| 21:12 |  | Refactor the new code on this branch. (check-in: 76a03b5314 ... user: dan tags: reduce-reprepares) | 
| 20:22 |  | Ensure that Vdbe.smimask is correctly set after a reprepare. (check-in: 56cdd2d432 ... user: dan tags: reduce-reprepares) | 
| 18:22 |  | Experimental attempt to reduce reprepares caused by bound variables and "=" constraints in WHERE clauses. (check-in: 1cfdde74f1 ... user: dan tags: reduce-reprepares) | 
| 15:13 |  | Correctly process "x IS TRUE" and similar expressions that appear in the second or later rows of a VALUES() clause. Report 2026-08-18T05:06:40Z. (check-in: d287358b8d ... user: dan tags: trunk) | 
| 15:06 |  | Fix a buffer overread in the fts5vocab virtual table when processing "term < 123" constraints. Problem recently introduced by [db05b203f4]. (check-in: 08e5a730d4 ... user: dan tags: trunk) | 
| 14:40 |  | Also correctly process "x IS NOT TRUE" expressions that appear in the second or later rows of a VALUES() clause. (closed check-in: 0de50d5685 ... user: dan tags: values-is-fix) | 
| 14:21 |  | Correctly process an "x IS TRUE" expression that appears in the second or later rows of a VALUES() clause. Possible fix for report bugs:/info/2026-08-18T05:06:40Z 2026-08-18T05:06:40Z. (check-in: 1be96edbd5 ... user: dan tags: values-is-fix) | 
| 14:15 |  | The geopoly_regular() SQL function now returns NULL if the radius argument is too large (+1.0e+300 or larger). Bug 2026-08-18T09:12:02Z (check-in: 15da75f104 ... user: drh tags: trunk) | 
| 14:00 |  | Filter the rtree tests out of speedtest1's default configuration when built without SQLITE_ENABLE_RTREE. Resolves forum post 2026-08-18T11:15:33Z. (check-in: 198c9678a2 ... user: stephan tags: trunk) | 
| 13:07 |  | Prevent a corrupt database from causing a CPU stack overflow in PRAGMA integrity_check. Report the excess btree depth instead. Bug 2026-08-18T10:33:10Z. (check-in: afd9f401c4 ... user: drh tags: trunk) | 
| 10:33 |  | Use a typecast to fix a possible integer overflow in the (unsupported) ext/misc/compress.c extension. Bug 2026-08-18T09:33:57Z. (check-in: d0f1f683b9 ... user: drh tags: trunk) | 
| 10:12 |  | Limit the number of nested RIGHT JOINs (or FULL JOINs) that can occur in a single query. Report 2026-08-18T07:03:05Z. (check-in: 003ed951d8 ... user: drh tags: trunk) | 
| 09:17 |  | Add SQLITE_LIMIT_TRIGGER_STEPS and -DSQLITE_MAX_TRIGGER_STEPS=n. Default maximum is 65000. (check-in: 6fbed57b14 ... user: drh tags: trunk) | 
| 07:51 |  | Plaform portability improvements for autosetup/find_tclconfig.tcl, from Dan Shearer at forum post 2026-08-13T20:55:18Z. (check-in: 2df4fe72f7 ... user: stephan tags: trunk) | 
| 00:36 |  | Fix the include guard on pcache.h, as requested by Report 2026-08-17T20:09:10Z. (check-in: e53c5fd75f ... user: drh tags: trunk) | 
| 00:17 |  | Further optimizations to jsonbPayloadSize(). The code is now faster than the first check-in on this branch, but less than 700 bytes larger than trunk (on x64 with gcc -Os). (check-in: 870313fea0 ... user: drh tags: opt-inline-jsonbPayloadSize) | 
| 2026-08-17 |  |  | 
| 21:26 |  | Declare fts5 structure records corrupt if they contain a page number greater than 0x7FFFFF00. Report 2026-08-17T20:07:33Z. (check-in: c97a940ceb ... user: dan tags: trunk) | 
| 15:31 |  | New -pgo option to test/speedtest.tcl compiles using gcc-style PGO. (check-in: b460488d05 ... user: drh tags: trunk) | 
| 13:55 |  | Avoid a problem causing a use-after-free in the fts3 snippet() function. Report 2026-08-17T09:18:24Z. (check-in: e4635c4153 ... user: dan tags: trunk) | 
| 11:52 |  | Have the fts5term and fts3aux virtual table modules only process constraints on the term column that use binary collation. Report 2026-08-17T10:17:15Z. (check-in: f26099b7fa ... user: dan tags: trunk) | 
| 11:31 |  | Avoid a buffer overrun when processing a strategically corrupted changeset. Report 2026-08-16T10:15:25Z. (check-in: 3f57024aec ... user: dan tags: trunk) | 
| 2026-08-16 |  |  | 
| 23:05 |  | Partial in-lining of jsonbPayloadSize(), happens in only 5 performance critical places instead of everywhere. This gives 96% of the performance increase with only 18% of the object code size increase. (check-in: e934c4a7aa ... user: drh tags: opt-inline-jsonbPayloadSize) | 
| 19:54 |  | Add the "Variable Number Bitmap Cache" to speed up parsing of queries that have a large number of "?NNN" style variables. (check-in: 2955ac4264 ... user: drh tags: trunk) | 
| 19:50 |  | Change the variable name for the variable-number bitmap cache. Make the size of the cache configurable at compile-time. (closed check-in: 35c9c6e635 ... user: drh tags: opt-add-bitmap-scan-VList) | 
| 12:04 |  | Add a small bitmap to the Parse object recording which variable numbers below 128 are already in Parse.pVList, so that repeated or out-of-order ?NNN parameters avoid a linear scan of the VList on each occurrence. Optimize particularly well on ORM testset. (check-in: 306f8f3ccb ... user: jeffchen tags: opt-add-bitmap-scan-VList) | 
| 11:52 |  | Mark jsonbPayloadSize() with SQLITE_OPT_INLINE, so that each JSONB node-header decode site absorbs a specialized copy in amalgamation builds. Reduces JSON testset time at a cost of about 9-10KB of code space in speed builds (check-in: 6d8e8474c5 ... user: jeffchen tags: opt-inline-jsonbPayloadSize) | 
| 10:49 |  | When reading a frame from a WAL file where the WAL pagesize and the database pagesize disagree, be sure to fully initialize the page that is read. Report 2026-08-17T15:54:41Z. (check-in: f9b4ee3695 ... user: drh tags: trunk) | 
| 05:19 |  | Add -DBUILD_sqlite to the T.compile.tcl makefile definition for the sake of Unix-on-Windows environments as a partial resolution to forum post fe9e99eb27. (check-in: aff3e43ed5 ... user: stephan tags: trunk) | 
| 2026-08-15 |  |  | 
| 14:24 |  | Fix the fts5vocab virtual table so that columns always behave as if they have blob affinity. Report 2026-08-15T07:03:55Z. (check-in: db05b203f4 ... user: dan tags: trunk) | 
| 12:35 |  | In the CLI, fix the internal modeDup() routine so that it correctly handles zIFmt and zFpFmt. (check-in: aa5a515490 ... user: drh tags: trunk) | 
| 12:22 |  | Fix typos. Turn off -fpfmt and -ifmt using "auto" instead of "default". (check-in: 530b55da56 ... user: drh tags: trunk) | 
| 10:10 |  | Refactor "realfmt"→"fpfmt" and "intfmt"→"ifmt". (check-in: 17277813d1 ... user: drh tags: trunk) | 
| 2026-08-14 |  |  | 
| 18:49 |  | Remove redundant offsetof definition from qrf.c. (check-in: 5d230cafc6 ... user: drh tags: trunk) | 
| 17:31 |  | Enhance QRF with options to supply format strings used to render integers and floating point numbers. Make those options accessible in the shell using the ".mode --intfmt" and ".mode --realfmt" respectively. Also make those options available to the "format" method of the sqlite3 object in the TCL interface. (check-in: 61b0157686 ... user: drh tags: trunk) | 
| 17:22 |  | Add support for integer and float-point format strings to the "format" method of the sqlite3 object in the TCL interface. (closed check-in: 49778c334a ... user: drh tags: qrf-num-format) | 
| 16:58 |  | We don't actually need to increment the version number of QRF as version 2 has not yet been released. Updates to the QRF README.md file. (check-in: 3ccc25a06f ... user: drh tags: qrf-num-format) | 
| 16:34 |  | Fix an off-by-one bug in --intfmt. Add some test cases. (check-in: 4e86fc1922 ... user: drh tags: qrf-num-format) | 
| 15:57 |  | Initial code for an enhancement to QRF that allows for app-defined format strings used to render integer and floating-point values. (check-in: 067083e973 ... user: drh tags: qrf-num-format) | 
| 15:17 |  | Build portability improvements for buildtclext.tcl, from bug forum post 2026-08-13T19:40:39Z. (check-in: 2530b5ada6 ... user: stephan tags: trunk) | 
| 14:03 |  | Adjust the bytecode generated for RIGHT JOIN so that it checks for duplicate RHS rows when the LHS is NULL. Bug 2026-08-14T04:56:02Z (check-in: f4f57ca539 ... user: drh tags: trunk) | 
| 13:07 |  | Adjust the bytecode so that in a RIGHT OUTER JOIN, whenever a row from the RHS table is added to the output set, the "match" index is updated to avoid duplicate rows. (closed check-in: 9d39b0973b ... user: drh tags: 2026-08-14T04:56:02Z) | 
| 10:57 |  | Fix the fts4aux virtual table so that it consistently behaves as if its columns have blob affinity. Report 2026-08-14T07:27:28Z. (check-in: d5b57f76b6 ... user: dan tags: trunk) | 
| 2026-08-13 |  |  | 
| 18:55 |  | Try to make test/testrunner.tcl work on other BSD variants. This change is untested as I don't have access to any of those BSDs. Bug 2026-08-13T17:55:42Z. (check-in: c406ce3f1e ... user: drh tags: trunk) | 
| 18:50 |  | Disable the seekscan optimization for IN(...) expressions when the RHS is a SELECT that returns more than one column. This was previously only possible if the sqlite_stat1 table was populated with inconsistent information, and was malfunctioning in some circumstances. Report 2026-08-13T08:45:47Z. (check-in: 601615fd00 ... user: dan tags: trunk) | 
| 18:37 |  | Disable the seekscan optimization for IN(...) expressions when the RHS is a SELECT that returns more than one column. This was previously only possible if the sqlite_stat1 table was populated with inconsistent information, and was malfunctioning in some circumstances. Report 2026-08-13T08:45:47Z. (closed check-in: 9544d46a6d ... user: dan tags: seekscan-fix) | 
| 17:49 |  | Disable misuse testing unless the ENABLE_MISUSE environment variable is set. Bug 2026-08-13T13:27:50Z (check-in: 273e1bcc2c ... user: drh tags: trunk) | 
| 16:10 |  | The OP_Affinity opcode with argument E should not try to preserve IntReal but should cast the value to a pure Real. Bug 2026-08-13T08:12:13Z. (check-in: 5a4e6920af ... user: drh tags: trunk) | 
| 15:16 |  | Ensure that if a seek-scan is used for an expression like (x, y) IN (SELECT a, b FROM ...) then the SELECT delivers rows in order of (a, b). (check-in: c9661d928e ... user: dan tags: seekscan-fix) | 
| 11:29 |  | Fix a problem in the fts4aux virtual table causing it to sometimes return spurious SQLITE_CORRUPT errors for fts4 tables with more than 1 column. Report 2026-08-13T05:56:36Z. (check-in: 06f3f9f93b ... user: dan tags: trunk) | 
| 00:21 |  | Disable recently added test cases for immutable=1 when running the inmemory_journal permutation. (check-in: 198e602053 ... user: drh tags: trunk) | 
| 2026-08-12 |  |  | 
| 20:26 |  | Fix an MSVC compiler warning in the amatch.c extension. (check-in: 43b23132a1 ... user: drh tags: trunk) | 
| 20:17 |  | Simplification and size reduction for sqlite3GetVarint(). Add and use a new routine sqlite3VarintValue() that works similarly but returns the value rather than the number of bytes consumed, for a performance improvement. Improvements inspired by [48132f815508b6e1]. (check-in: 2d7ac2095e ... user: drh tags: trunk) | 
| 20:04 |  | Simplify the implementation of sqlite3GetVarint(). (closed check-in: d3dfaeb8b9 ... user: drh tags: varint-decode) | 
| 19:42 |  | Create a new varint decoder that does not need to return the size of the varint, and use that in sqlite3BtreeTableMoveto(). Omit the pointless getVarint and putVarint macros and use the actual function names instead. (check-in: f845ede54c ... user: drh tags: varint-decode) | 
| 13:41 |  | Fix tests recently added to nolock.test so that they run on windows. (check-in: bc57a14ab9 ... user: dan tags: trunk) | 
| 13:23 |  | Do not use the propagate-constant optimization on a constant that is really a function that might return a subtype. Bug 2026-08-12T09:26:04Z (check-in: a5bf606f3b ... user: drh tags: trunk) | 
| 00:51 |  | Enhance the CARRAY interface so that it raises an SQLITE_TOOBIG error if the iov_len value of a BLOB is bigger than a 32-bit signed integer can hold. Forum 2026-08-12T00:07:44Z. (check-in: 39ae2b7c59 ... user: drh tags: trunk) | 
| 2026-08-11 |  |  | 
| 23:40 |  | Small performance enhancement and size reduction in the page cache. (check-in: 0910cf8ef4 ... user: drh tags: trunk) | 
| 21:48 |  | Avoid integer divides in the remaining pcache1 hash-bucket computations, extending check-in [750c37d4044c0957] from the lookup fast path to the other eight sites. Small size reduction. (closed check-in: 0f814ec505 ... user: jeffchen tags: opt-pcache1-hash-mask) | 
| 18:22 |  | Ensure that databases opened with immutable=1 specified are opened read-only at the OS level. Report 2026-08-11T17:21:54Z (check-in: 702cdeb78c ... user: dan tags: trunk) | 
| 14:16 |  | Fix a problem in fts5 secure-delete code that could lead to a buffer overrun when dealing with a strategically corrupted database. Problem reported by YEONBA. (check-in: 9cd4f9e920 ... user: dan tags: trunk) | 
| 10:46 |  | Update preprocessor logic in test file fts5_tcl.c to use intptr_t when available. (check-in: fee71cd6f7 ... user: dan tags: trunk) | 
| 2026-08-10 |  |  | 
| 20:17 |  | Performance improvement and size reduction in the vdbeRecordCompareInt() routine. (check-in: 15f719dde4 ... user: drh tags: trunk) | 
| 20:05 |  | Performance improvement and size reduction in the OP_MakeRecord opcode. (check-in: 8b1f492374 ... user: drh tags: trunk) | 
| 19:49 |  | Add "include <stdint.h>" to test file fts5_tcl.c. (check-in: 9cf3f0079a ... user: dan tags: trunk) | 
| 19:38 |  | Add an assert() to demonstrate that an arithmetic right-shift is being used where it is needed. (closed check-in: cce760dc6b ... user: drh tags: branchless-int-compare) | 
| 18:07 |  | Fix the logic for big-endian CPUs. Add comments on how to test various limbs of the logic. (closed check-in: cf49f4fc96 ... user: drh tags: optimized-makerecord) | 
| 16:27 |  | Fix the logic to prevent buffer overruns. (check-in: 82a8332ed9 ... user: drh tags: optimized-makerecord) | 
| 15:57 |  | When comparing row values that are sub-selects, do not consider the collation sequences of the expressions returned by the sub-select when determining the collation sequences to use for the comparisons. i.e. in "(a, b)=(SELECT c, d)", do not consider the collation sequences of "c" or "d". Fix for problem 2026-08-09T09:33:49Z. (check-in: 88580851be ... user: dan tags: trunk) | 
| 15:36 |  | Experimental changes to improve performance of the OP_MakeRecord opcode, and especially the code the writes integer content into the record. This change is not yet 100% correct. It is a work in progress. Parking on a branch because I need to stop and work on other things. (check-in: fa8dd2af6d ... user: drh tags: optimized-makerecord) | 
| 12:56 |  | Make this optimization available to MSVC in addition to GCC. (check-in: 648ab3f7ae ... user: drh tags: branchless-int-compare) | 
| 12:45 |  | Remove some stray debug output and obsolete bug-hunting comments. No functional changes. (check-in: 41443d5ffe ... user: stephan tags: trunk) | 
| 12:41 |  | Merge the sqlite3Get8byte() enhancement from trunk (check-in: 661d0c7104 ... user: drh tags: branchless-int-compare) | 
| 12:41 |  | Expose the sqlite3_backup APIs to JavaScript. (check-in: fa17d89f76 ... user: stephan tags: trunk) | 
| 12:35 |  | Disable Module.instantiateWasm() in node builds to match branch-3.53's behavior. Fix a sqlite3_backup test (duplicate finalize). The aforementioned world-breaking bug was caused by my browser having gotten into a funky state and needing a restart. (closed check-in: 7d77bb0886 ... user: stephan tags: js-backup-bindings) | 
| 12:29 |  | Replace the EIGHT_BYTE_U64 macro with the in-line function sqlite3Get8byte() which is more efficient cross-platform. (check-in: 9492183f2f ... user: drh tags: trunk) | 
| 12:18 |  | Add tests for the sqlite3_backup JS bindings but they are disabled while tracking down an unrelated problem which prevents those tests from working (most of the sqlite3.capi methods are being mis-mapped to the sqlite3InitModule function for reasons unimaginable). (check-in: cb610a8a87 ... user: stephan tags: js-backup-bindings) | 
| 11:51 |  | Fix a circular dep generated by mkwasmbuilds.c (check-in: f2514ace13 ... user: stephan tags: js-backup-bindings) | 
| 11:23 |  | Merge trunk into the js-backup-bindings branch. (check-in: 692c4e8ad8 ... user: stephan tags: js-backup-bindings) | 
| 2026-08-09 |  |  | 
| 19:03 |  | Decode fixed-width integer serial types in vdbeRecordCompareInt() with a single eight-byte read and an arithmetic shift, replacing a switch that predicts poorly on mixed-width integer keys. (check-in: fad1a762e1 ... user: jeffchen tags: branchless-int-compare) | 
| 2026-08-08 |  |  | 
| 20:29 |  | Remove unused variable from pragma.c. (check-in: 7e3ad0dcde ... user: dan tags: trunk) | 
| 19:47 |  | Change the way fts5 test code casts between integers and pointers to avoid a ubsan error in some versions of gcc. Report 2026-08-08T15:54:04Z. (check-in: d275eb5b63 ... user: dan tags: trunk) | 
| 09:58 |  | Improve the performance of the OP_Column opcode of the bytecode engine by in-lining the call to sqlite3VdbeSerialGet(). (check-in: d2dd7b3b13 ... user: drh tags: trunk) | 
| 2026-08-07 |  |  | 
| 19:11 |  | Fix a comment typo. Remove an unused function prototype. (closed check-in: b08d213daf ... user: drh tags: opcol-inline-ints) | 
| 18:41 |  | Fix a harmless UBSAN warning. (check-in: eb492208fc ... user: drh tags: opcol-inline-ints) | 
| 18:18 |  | Change the order of two operations in the REAL decoded in OP_Column for a small performance boost. (check-in: d3ab3da024 ... user: drh tags: opcol-inline-ints) | 
| 15:09 |  | Code simplifications. This check-in actually gives up a few CPU cycles relative to the previous, according to speedtest.tcl, but not many. And in exchange, the binary is smaller. The branch has a whole is still much faster than trunk. (check-in: 20f50ea9c6 ... user: drh tags: opcol-inline-ints) | 
| 13:33 |  | Fix a problem introduced by [32c762bbb1] causing sessions module conflict handling to malfunction when used with a database handle configured with sqlite3_extended_result_codes(). Also ensure the "session_eec" permutation, which would have detected this, is run as part of release testing. (check-in: 8b3da5d6ce ... user: dan tags: trunk) | 
| 11:14 |  | Further optimization to OP_Column along the lines of the prior check-in. (check-in: a4b3021d1f ... user: drh tags: opcol-inline-ints) | 
| 05:31 |  | Lower the range of inputs accepted by the pstack allocator to address /bugs/4dbd096fe335f974. (check-in: fafefb5948 ... user: stephan tags: trunk) | 
| 00:00 |  | Performance optimization in OP_Column: decode serial types 0 through 6, 8, and 9 (NULL and all integer widths) inline in a switch, avoiding the call and dispatch overhead of sqlite3VdbeSerialGet(). The wide-integer cases 5 and 6 add bulk, but omitting them would leave those types paying the switch miss plus the original call. Other serial types are decoded as before. (check-in: cf829cc21b ... user: jeffchen tags: opcol-inline-ints) | 
| 2026-08-06 |  |  | 
| 17:20 |  | Fix a hyperlink typo in the Lemon documentation (check-in: ee8ef5cd4e ... user: drh tags: trunk) | 
| 2026-08-05 |  |  | 
| 15:40 |  | Merge in the errant wasm-updates branch, caused by developer PEBKAC. (check-in: 75d3343fa3 ... user: stephan tags: trunk) | 
| 15:30 |  | Fix a compiler warning that comes up in gcc 4.0.1 for target powerpc-apple-darwin9 (check-in: 90fe33d9ed ... user: drh tags: trunk) | 
| 15:00 |  | Strip out 42kb of ext/wasm/libcmpp.c (build tool) by using a destilled build shorn of the capabilities we don't use in this tree, like #pipe and loadable modules. (closed check-in: 3ec42b829e ... user: stephan tags: wasm-updates) | 
| 14:00 |  | Small performance improvement and size reduction in the page cache. (check-in: 2c3ae5b8e1 ... user: drh tags: trunk) | 
| 13:29 |  | Avoid an integer divide on the page-cache lookup fast path. The hash table size in pcache1 is always a power of two, so the bucket index in pcache1FetchNoMutex() can be computed with a mask instead of a modulo. An assert() verifies the power-of-two invariant in debug builds. (closed check-in: 750c37d404 ... user: jeffchen tags: jeffchen-opt-page-cache-lookup) | 
| 2026-08-04 |  |  | 
| 20:40 |  | The sorter enhancement at [bdc841de10fef65b] contains a bug where it might sort 64-bit integers in the wrong order due to the limited precision of double. Fixed here. (check-in: 6bdfff7ddb ... user: drh tags: trunk) | 
| 14:55 |  | Improved performance in the sorter by adding a special case comparison function for when the sorting by floating-point numbers. (check-in: bdc841de10 ... user: drh tags: trunk) | 
| 13:44 |  | Fix a single byte buffer overrun in the approximate_match extension. (check-in: 7f380be91b ... user: dan tags: trunk) | 
| 11:04 |  | In fts5, account for the possibility of negative docids when allocating a buffer to store the results of merging detail=none prefix lists. Fix for report 2026-08-04T08:49:27Z. (check-in: a7f8c92f62 ... user: dan tags: trunk) | 
| 2026-08-03 |  |  | 
| 19:59 |  | Improved comparison function for the sorter, using the idea demonstrated by the patch in Bug 2026-08-02T13:41:35Z. (closed check-in: 6cda98ee3c ... user: drh tags: vdbesort-opt) | 
| 15:05 |  | When processing a vector IN(...) term in a WHERE clause, do not add individual terms for each column if the RHS of the IN(...) is a query that is expected to do the special min-max aggregate behaviour. Fix for 2026-08-03T09:05:06Z. (check-in: 0f873f5651 ... user: dan tags: trunk) | 
| 14:10 |  | When processing a vector IN(...) term in a WHERE clause, do not add individual terms for each column if the RHS of the IN(...) is a query that is expected to do the special min-max aggregate behaviour. Possible fix for 2026-08-03T09:05:06Z. (closed check-in: b5cee626e1 ... user: dan tags: bug-2026-08-03T09:05:06Z) | 
| 13:22 |  | Set O_BINARY for Windows console output in the CLI. Candidate fix for bug 2026-08-03T08:52:42Z. (check-in: af136dc1d1 ... user: drh tags: trunk) | 
| 2026-08-02 |  |  | 
| 22:51 |  | Fix harmless compiler warnings caused by the enhancement at [a67bff640c9e4ffd] (check-in: e0ab12ec72 ... user: drh tags: trunk) | 
| 22:45 |  | Make the test-use-only SQL function fts3_exprtest() (available only if compiled with SQLITE_TEST) more resistant to out-of-bounds inputs. Bug 2026-08-02T12:28:28Z and bug 2026-08-02T12:31:40Z. (check-in: 30a6134707 ... user: drh tags: trunk) | 
| 21:59 |  | Cap constraint values on constraints to the amatch virtual table so that they fit into a 32-bit signed integer. Bug 2026-08-02T13:01:25Z (check-in: 092b87164a ... user: drh tags: trunk) | 
| 20:17 |  | Update the header comment on the zipfile extension to state clearly that writes to the ZIP archive are non-transaction. No code changes. Bug 2026-08-02T14:03:49Z. (check-in: 1b623a6064 ... user: drh tags: trunk) | 
| 19:40 |  | Fix incorrect header comment in the templatevtab.c demo extension. Bug 2026-08-02T13:49:50Z (check-in: 1c95c741ca ... user: drh tags: trunk) | 
| 19:35 |  | Fix typo in column name generation logic of the (debugging use only) vtablog.c extension. Bug 2026-08-02T13:56:56Z (check-in: 42bde30535 ... user: drh tags: trunk) | 
| 18:27 |  | Take care to avoid trying to compute 0.0/0.0 in the diskused extension. If such a value needs to be displayed, show the result as 0.0. Bug 2026-08-02T12:48:58Z (check-in: a455f347c5 ... user: drh tags: trunk) | 
| 17:22 |  | Reduce the size of test cases for the closure extension so that they run faster. The closure extension is not important for anything anymore and is retained mostly for historical reference. There is no point in burning CPU cycles testing it with large cases. (check-in: 73350e1281 ... user: drh tags: trunk) | 
| 17:12 |  | Enhance the (unsupported) closure extension to support 64-bit "depth" values. Bug 2026-08-02T13:11:13Z. (check-in: fccfd5c410 ... user: drh tags: trunk) | 
| 16:32 |  | Do not allow the (obsolete and unsupported) closure extension to use itself as its own reference table. Bug 2026-08-02T13:07:40Z. (check-in: 5e347561c3 ... user: drh tags: trunk) | 
| 2026-08-01 |  |  | 
| 15:08 |  | Updates to build instructions, especially on Windows. Fix a harmless compiler working in debug builds on Windows. (check-in: be245e136a ... user: drh tags: trunk) | 
| 2026-07-31 |  |  | 
| 22:45 |  | The use of sqlite3_set_clientdata() to limit recursion in [4687e04d948b787c] is incorrect. Fixed here. (check-in: bf44a08adc ... user: drh tags: trunk) | 
| 2026-07-30 |  |  | 
| 12:17 |  | Update ext/wasm/libcmpp.c for build portability improvements and doc updates. (check-in: 48fdf51b53 ... user: stephan tags: js-backup-bindings) | 
| 2026-07-29 |  |  | 
| 23:47 |  | Enforce the 1GiB size limit on appendvfs files. Bug 2026-07-29T06:29:52Z (check-in: 180e75e076 ... user: drh tags: trunk) | 
| 23:28 |  | Fix the detection ON clauses using tables to the right so that it does not depend on the order of cursor allocation, which is inconsistent with recursive CTEs. Problem introduced by [d96271db6a3a44e5]. Reported by Bug 2026-07-29T17:50:40Z. (check-in: 5a22150bf8 ... user: drh tags: trunk) | 
| 21:09 |  | Improve comments. Move a variable outside of a loop. (closed check-in: f26831158e ... user: drh tags: bug-2026-07-29T17:50:40Z) | 
| 19:17 |  | Avoid assuming that cursor numbers were allocated in order for all SrcList objects when detecting ON clauses that refer to tables to their right. Possible fix for 2026-07-29T17:50:40Z. (check-in: ebfea8cfb0 ... user: dan tags: bug-2026-07-29T17:50:40Z) | 
| 18:03 |  | In the experimental and unsupported amatch.c extension, raise an error if a cost value is out of range even if the rule that the cost applies to will never be used. Bug 2026-07-29T06:30:13Z (check-in: 86b7a62c5c ... user: drh tags: trunk) | 
| 17:36 |  | Do not allow recursion in extension functions sha1_query() or sha3_query(). Bug 2026-07-29T06:29:33Z. (check-in: 4687e04d94 ... user: drh tags: trunk) | 
| 17:19 |  | Turn redundant loads of the experimental vfsstat.c extension from a stack overflow into a harmless no-op. Bug 2026-07-29T06:28:47Z. (check-in: add89fba2f ... user: drh tags: trunk) | 
| 17:08 |  | Turn redundant registrations of the cksumvfs extension from a stack overflow into a harmless no-op. Bug 2026-07-29T06:28:31Z (check-in: d6453bc80e ... user: drh tags: trunk) | 
| 16:55 |  | Return an empty string if an fts3 snippet() is requested for an out-of-range column index. Bug 2026-07-29T06:30:35Z. (check-in: 5c645d5e24 ... user: dan tags: trunk) | 
| 15:57 |  | Always take a copy of a value used to drive an index as part of a RIGHT JOIN loop. Otherwise, the RIGHT JOIN loop and the body of the loop it jumps back into may interfere with each other. Fix for 2026-07-29T05:33:50Z. (check-in: 8d626b31ae ... user: dan tags: trunk) | 
| 15:49 |  | Always take a copy of a value used to drive an index as part of a RIGHT JOIN loop. Otherwise, the RIGHT JOIN loop and the body of the loop it jumps back into may interfere with each other. Possible fix for 2026-07-29T05:33:50Z. (closed check-in: d7ad2d0a91 ... user: dan tags: bug-2026-07-29T05:33:50Z) | 
| 15:13 |  | Avoid apply an affinity to a register that might be the result of a TK_AGG_COLUMN expression, as it might be reused. Fix for bug 2026-07-29T05:51:06Z. (check-in: b9ecd9caf7 ... user: dan tags: trunk) | 
| 14:23 |  | Avoid apply an affinity to a register that might be the result of a TK_AGG_COLUMN expression, as it might be reused. Possible fix for bug 2026-07-29T05:51:06Z. (closed check-in: e5cce8a002 ... user: dan tags: bug-2026-07-29T05:51:06) | 
| 13:00 |  | Block deep recursion in test extension functions test_eval() and eval(). Does not affect deliverable code. Bug 2026-07-29T06:29:11Z (check-in: 3e99c27fe3 ... user: drh tags: trunk) | 
| 12:51 |  | Add long-missing sqlite3_backup bindings to JS/WASM, in response to forum thread dc96ed27b3. This does not yet include tests. (check-in: 90e5997574 ... user: stephan tags: js-backup-bindings) | 
| 11:28 |  | Improved comments clarifying operation of the ext/misc/noop.c testing extension. Bug 2026-07-29T06:31:22Z. (check-in: c9bb594059 ... user: drh tags: trunk) | 
| 10:28 |  | Fix the sha1() extension function so that it interprets zero-length BLOBs as BLOBs and not as NULL. Also add a few test cases for that extension function, which was previously completely untested. Bug 2026-07-29T06:31:41Z. (check-in: 978d12de38 ... user: drh tags: trunk) | 
| 2026-07-28 |  |  | 
| 14:16 |  | Further fixes for running [make devtest] with Fil-C. (check-in: 1ed5e5bed2 ... user: dan tags: trunk) | 
| 2026-07-27 |  |  | 
| 14:22 |  | Remove a stray reference to "sqlite3_checker.exe" from Makefile.msc. sqlite3_checker was removed from the tree here [213b1c6608]. (check-in: 86bc7c8648 ... user: dan tags: trunk) | 
| 11:19 |  | The MREMAP_FIXED macro (added by [4d60798b20a560be]) is not available on all platforms. (check-in: 8afa7a1d40 ... user: drh tags: trunk) | 
| 10:19 |  | Use snprintf() rather than sprintf() in the dtostr() test function of the CLI. Forum 2026-07-27T09:44:11Z. (check-in: c0681a6cb7 ... user: drh tags: trunk) | 
| 2026-07-26 |  |  | 
| 18:45 |  | Add reference counting to the EditDist3Config object in the spellfix extension. This should prevent possible UAF following OOM as described by Bug 2026-07-26T07:55:13Z. (check-in: 37364eaecc ... user: drh tags: trunk) | 
| 16:05 |  | Add a JS test for [d5d8ee76d3] resp. [012550c089]. (check-in: f28494eeaf ... user: stephan tags: trunk) | 
| 15:56 |  | Factor an extranous local var out of the previous check-in. (check-in: 422b4303da ... user: stephan tags: trunk) | 
| 15:47 |  | Remove some extraneous memory copying in kvvfs. Reported off-list by Jure Rotar. (check-in: ce09bfe406 ... user: stephan tags: trunk) | 
| 15:39 |  | Fix a JS container API misuse in sqlite3.oo1.DB.close() which caused its fallback cleanup of still-open statements to not actually do so. Reported off-list by Jure Rotar. (check-in: d5d8ee76d3 ... user: stephan tags: trunk) | 
| 2026-07-25 |  |  | 
| 18:56 |  | Improved optimization of "expr OR TRUE" and "expr OR FALSE". Bug 2026-07-25T08:36:25Z. (check-in: 2f0bde4bc8 ... user: drh tags: trunk) | 
| 17:13 |  | Avoid opening cursors only required for reads that are optimized away by the exists-to-join optimization. Addresses report 2026-07-25T08:39:23Z. (check-in: c81ec73a37 ... user: dan tags: trunk) | 
| 14:49 |  | Ignore any DISTINCT qualifier on a SELECT statement on the RHS of an IN(...) expression. Forum post 2026-07-25T08:40:13Z. (check-in: c54858eab4 ... user: dan tags: trunk) | 
| 2026-07-24 |  |  | 
| 16:28 |  | Fix an incompatibility with Tcl 9 in scanstatus2.test. (check-in: 2f1f4f7353 ... user: dan tags: trunk) | 
| 14:53 |  | Fix a bug in the check-in [bbd69ce274620008] causing that bug-fix check-in to fail to completely fix the original bug. This fix is in response to the follow-up report at Bug 2026-07-24T02:52:08Z. (check-in: 2cb2287768 ... user: drh tags: trunk) | 
| 14:10 |  | Fix a problem in test1.c causing test script win32lock.test to fail. (check-in: 8f6d25b3f0 ... user: dan tags: trunk) | 
| 13:36 |  | Fix to the interaction between indexes-on-expressions, subtypes, and the unary "+" operator. This is a continuation of the prior fix from check-in [d75c08c8416bde3f]. This enhanced fix is prompted by Bug 2026-07-24T08:42:47Z. (check-in: 5398c6a2b6 ... user: drh tags: trunk) | 
| 10:38 |  | In test_syscall.c, assume SQLite always calls the 4 (not 5) argument form of mremap(). This fixes a problem with Fil-C and mmap2.test. (check-in: 4d60798b20 ... user: dan tags: trunk) | 
| 00:39 |  | Fix some test cases on Windows. (check-in: d2d6363f31 ... user: drh tags: trunk) | 
| 2026-07-23 |  |  | 
| 21:34 |  | Enhance Bloom-filter support so that it works with blob values and with text values as long as the collating sequence is binary. (check-in: a67bff640c ... user: drh tags: trunk) | 
| 20:23 |  | New test cases for Bloom filter use on text keys as long as the collating sequence is binary. (closed check-in: 98ced971bf ... user: drh tags: bloom-filter-on-text) | 
| 20:04 |  | Change the way pointers are passed between C code and Tcl test scripts. This allows tests to be run with Fil-C builds. (check-in: ba919ea1d4 ... user: dan tags: trunk) | 
| 18:55 |  | Improved Bloom-filter invariant checking. (check-in: f38f416711 ... user: drh tags: bloom-filter-on-text) | 
| 17:41 |  | Fix the new filterHash() function so that it works correctly for zeroblobs. (check-in: 3013d6ab4a ... user: drh tags: bloom-filter-on-text) | 
| 16:00 |  | Add the ability for Bloom filters to work on text values, as long as the collating sequence is BINARY. (check-in: 2577edc796 ... user: drh tags: bloom-filter-on-text) | 
| 12:12 |  | CLI improvement: New command-line option "--cmdline-edit BOOLEAN" to enable/disable command-line editing. Command-line editing is disabled by default if either stdin or stdout is not a tty. (check-in: 675ed9a486 ... user: drh tags: trunk) | 
| 2026-07-22 |  |  | 
| 14:16 |  | Fix an error handling case in #base64. (check-in: f8c408f662 ... user: stephan tags: trunk) | 
| 14:06 |  | Add (#base64 -f filename) support to the preprocessor and slightly simplify the wasm embedding with it. Fix a bug in the preprocessor's @token@ call expansion when the length of the configured @ is greater than 1 byte. (check-in: 1e90bedec9 ... user: stephan tags: trunk) | 
| 12:55 |  | Do not cache the results of getenv() in the temporary filename generation logic as that might result in a UAF if an environment variable value is modified using setenv() or similar. (check-in: cefd5807d8 ... user: drh tags: trunk) | 
| 11:24 |  | Improvements to the unix temporary filename generator so that it continues
 to work even after SQLITE_TMPDIR or TMPDIR environment variables are
 changed by the application. ~~Similar changes are needed for Windows.~~ Actually, os_win.c is ok.  The getenv() calls only happen on cygwin and the
 results from getenv() are not persisted.  (closed check-in: 82e522ff14 ...  user: drh tags: temp-filenames) | 
| 2026-07-21 |  |  | 
| 23:54 |  | Remove an unnecessary munmap() from the unix interface. (check-in: 5fc1ede8b2 ... user: drh tags: trunk) | 
| 20:56 |  | Random makefile doc updates. No functional changes. (check-in: 014ef252c7 ... user: stephan tags: trunk) | 
| 20:18 |  | Random minor doc and formatting cleanups in the wasm makefile generator. (check-in: a24064919b ... user: stephan tags: trunk) | 
| 19:12 |  | Restructure an errant goto which only accidentally never broke anything. Add some debug output to mkwasmbuilds.c. (check-in: 0ca4e748e1 ... user: stephan tags: trunk) | 
| 16:58 |  | The base64.wasm=1 flag to ext/wasm/GNUmakefile tells it to embed each resulting WASM file directly in its counterpart JS file base64-encoded. This is roughly 20% larger and saves a network request but is overall less efficient so will not be the new default. It may be interesting for custom builds, in particular for server-side node.js. (check-in: 7a06190b71 ... user: stephan tags: trunk) | 
| 16:43 |  | Document the origin of the input size limit for #base64. No functional changes. (closed check-in: 696988aac3 ... user: stephan tags: wasm-base64) | 
| 16:29 |  | Simplify the previous checkin by removing an extraneous local var. (check-in: 6382d8336e ... user: stephan tags: wasm-base64) | 
| 16:23 |  | Fix the corner case of feeding #base64 empty input. (check-in: 999d201971 ... user: stephan tags: wasm-base64) | 
| 16:12 |  | Consolidate a duplicated code block and ensure that the new filtering of the final JS files makes them read/write so that rebuilds without a clean between them work. (check-in: 8b013184e4 ... user: stephan tags: wasm-base64) | 
| 16:04 |  | Add support for (make base64.wasm=1), which embeds sqlite3.wasm into its corresponding JS file as base64. A very expensive calculator which was fed the resulting file sizes assures me that the two-file approach is, in the aggregate, more efficient both in terms of overall network bandwidth (+~20% bigger for base64) and client-side memory usage (streaming vs non-streaming wasm compilation), so we'll shelve this as an interesting experiment or maybe for use by custom builds. (check-in: a167aee751 ... user: stephan tags: wasm-base64) | 
| 13:28 |  | This embeds sqlite3.wasm into sqlite3.js/mjs as base64 but is applied globally - it still needs to be made configurable. (check-in: 0467a0f826 ... user: stephan tags: wasm-base64) | 
| 12:57 |  | An initial attempt at embedding sqlite3.wasm into sqlite3.js using base64. This produces a chicken-egg situation in the build because the input file where the blob belongs is injested by emscripten (which generates the wasm file from which that blob is to be generated), so it does not work as-is but is being set aside for later. We'll need to add a second preprocessing step which uses a different #directive delimiter to keep it hidden inside the first pass, then squeeze the wasm file in through ... (check-in: 538199cf62 ... user: stephan tags: wasm-base64) | 
| 10:44 |  | Latest upstream ext/wasm/libcmpp.c, primarily to make it filcc-legal (only the final chunk in the diff - the rest is minor changes accumulated since the last update). (check-in: 06f3bbc5bd ... user: stephan tags: trunk) | 
| 2026-07-20 |  |  | 
| 21:03 |  | Improve performance and robustness of internal routine jsonBlobExpand(). (check-in: 099af17c6f ... user: drh tags: trunk) | 
| 20:23 |  | Add tests to verify that the problem reported by forum post 2026-07-20T18:27:00Z is only a problem on branch-3.53, not on this branch. (check-in: 6384b4b872 ... user: dan tags: trunk) | 
| 20:03 |  | Improved detection and reporting of deliberate misinformation in the sqlite_schema table. (check-in: 760af4e455 ... user: drh tags: trunk) | 
| 19:17 |  | Remove an incorrect ALWAYS() macro from the implementation of the internal exprNodeCanReturnSubtype() routine so that it works with zero-argument deterministic functions like pi(). (check-in: a2511c6223 ... user: drh tags: trunk) | 
| 17:53 |  | Ensure that a write with REPLACE conflict resolution on an integer primary key column is handled after any IGNORE or FAIL conflicts, even if there is also an explicit "ON CONFLICT" clause unrelated to the IPK. Forum post 2026-07--17T09:30:42Z. Also fix a problem whereby a trigger fired by REPLACE conflict resolution might cause a UNIQUE conflict violation to be committed to the database. Forum post 2026-07-18T09:45:24Z. (check-in: 8f0ad8f33f ... user: dan tags: trunk) | 
| 16:45 |  | Additional test cases. (closed check-in: 3ae4f953de ... user: drh tags: do-nothing-replace-bug) | 
| 14:37 |  | Move the new test cases for this branch into a new *.test file. (check-in: 7bdce4ec3b ... user: drh tags: do-nothing-replace-bug) | 
| 11:50 |  | Performance improvement in the sqlite3_initialize() interface. (check-in: 4ad701b085 ... user: drh tags: trunk) | 
| 2026-07-19 |  |  | 
| 19:49 |  | Fix boundary cases of rowid lookup using very large floating point values - values near the minimum and maximum 64-bit signed integer. Forum 2026-07-19T07:31:46Z. (check-in: 987ba5a3dc ... user: drh tags: trunk) | 
| 12:56 |  | Latest upstream jimsh0.c for minor portability improvements and cleanups. (check-in: 436cf63e94 ... user: stephan tags: trunk) | 
| 2026-07-18 |  |  | 
| 19:49 |  | Test case fixes due to the previous. (check-in: 9236658232 ... user: drh tags: do-nothing-replace-bug) | 
| 19:31 |  | After a DELETE trigger is fired by REPLACE conflict handling, retest all UNIQUE constraints to ensure that the row being inserted or updated does not conflict with one written by the DELETE trigger. Abort the statement if it does. Possible fix for forum post 2026-07-18T03:35:48Z. (check-in: e2ab286bb7 ... user: dan tags: do-nothing-replace-bug) | 
| 17:47 |  | Disqualify the count-of-view optimization if the view is really a correlated subquery. Forum post 2026-07-18T10:23:43Z. (check-in: bb65c3adcd ... user: drh tags: trunk) | 
| 13:34 |  | Update tcl test command "optimization_control" to support the "count-of-view" optimization. (check-in: b14c3f8fec ... user: dan tags: trunk) | 
| 09:04 |  | Enhancements to new test cases, for improved readability. (check-in: 053e0d1f01 ... user: drh tags: do-nothing-replace-bug) | 
| 08:46 |  | Add test cases from Forum Post 2026-07-18T03:35:48Z. New test cases are currently failing, as new or alternative fixes are needed for the original problem. (check-in: 49b67b7da6 ... user: drh tags: do-nothing-replace-bug) | 
| 2026-07-17 |  |  | 
| 23:25 |  | Additional test cases (check-in: da16fe363e ... user: drh tags: do-nothing-replace-bug) | 
| 16:26 |  | Ensure that a write with REPLACE conflict resolution on an integer primary key column is handled after any IGNORE or FAIL conflicts, even if there is also an explicit "ON CONFLICT" clause unrelated to the IPK. Forum post 2026-07-17T09:30:42Z. (check-in: 1875249ab3 ... user: dan tags: do-nothing-replace-bug) | 
| 2026-07-16 |  |  | 
| 23:55 |  | Add the SQLITE_SHELL_EDITION compile-time option, which if set to a date number of the form YYYYMMDD will try to configure the defaults to be similar to the latest release at or prior to that date. (check-in: 6924895d2c ... user: drh tags: trunk) | 
| 13:17 |  | Relax the restrictions on reordering tables in vtab queries that occur to the right of a LEFT or CROSS JOIN but are not actually the RHS of said join. (check-in: 9062c79fc2 ... user: dan tags: trunk) | 
| 2026-07-15 |  |  | 
| 19:19 |  | Relax the restrictions on reordering tables in vtab queries that occur to the right of a LEFT or CROSS JOIN but are not actually the RHS of said join. (closed check-in: 7df34b446e ... user: dan tags: vtab-outer-joins) | 
| 2026-07-14 |  |  | 
| 17:02 |  | Adjust QRF to generate lower-case HTML output with the SQLITE_STYLE_Html style setting. (check-in: 4be8630e05 ... user: drh tags: trunk) | 
| 14:52 |  | Remove harmless "breakpoint" command inserted by the previous check-in (check-in: 5a4b0b81de ... user: drh tags: trunk) | 
| 11:46 |  | Update some test cases in fts5corrupt3.test and fts5corruptB.test to account for recent updates to database and record corruption detection. (check-in: a48f4cf312 ... user: dan tags: trunk) | 
| 2026-07-13 |  |  | 
| 14:52 |  | Add assert()s and a testcase() to and fix and improve comments in the percentSort() routine. (check-in: be673351eb ... user: drh tags: trunk) | 
| 2026-07-12 |  |  | 
| 07:42 |  | Improve documentation for sqlite3_result_zeroblob64(). No changes to code. (check-in: c3d307d136 ... user: drh tags: trunk) | 
| 2026-07-11 |  |  | 
| 19:49 |  | Improvements to the implementation of sqlite3_result_str() so that it has 100% mutation coverage. (check-in: 121f54c532 ... user: drh tags: trunk) | 
| 16:14 |  | Minor change to the "weekday N" modifier in the date/time functions to facilitate 100% mutation test coverage. (check-in: 76002a6f5d ... user: drh tags: trunk) | 
| 2026-07-10 |  |  | 
| 15:06 |  | New compile-time option SQLITE_MAX_SCHEMA and runtime limit option SQLITE_LIMIT_SCHEMA that put an upper bound on the number of objects in a database schema. Current default is 10 million. (check-in: 4ff362a4c2 ... user: drh tags: trunk) | 
| 2026-07-09 |  |  | 
| 20:56 |  | Ensure that the "leaf-size" field of an fts5 record is only read from records that are actually leaves and are large enough to contain it. Bug 2026-07-09T20:15:55Z. (check-in: fe52900694 ... user: dan tags: trunk) | 
| 16:34 |  | In the sqlite3_rsync utility program, fix three defects identified by Bug e2dfadf51209f12b. (check-in: 805f3acd2b ... user: drh tags: trunk) | 
| 2026-07-08 |  |  | 
| 20:30 |  | Fix an incorrect free() call in the CLI on Windows when using the -A command-line option. (check-in: 1ce57ba94b ... user: drh tags: trunk) | 
| 17:54 |  | Allow "ALTER TABLE ... DROP CONSTRAINT" to drop foreign key constraints. (check-in: 4b0882cdb5 ... user: dan tags: trunk) | 
| 17:40 |  | Allow "ALTER TABLE ... DROP CONSTRAINT" to drop foreign key constraints. (closed check-in: 17c0896381 ... user: dan tags: drop-constraint-fk) | 
| 15:43 |  | For display modes in the CLI for which column headers are enabled, make the mode "always" instead of "on. Improved comments and test cases. (check-in: c8aa208bf5 ... user: drh tags: trunk) | 
| 15:34 |  | Make the (legacy, deprecated) ".header" command in the CLI responsive to the "always" title mode. Clean up title mode and define its operation with improved comments in the TCL interface and in the CLI. Add the "-title auto" option to the CLI. (closed check-in: f94259fc07 ... user: drh tags: title-always-default-on) | 
| 2026-07-07 |  |  | 
| 14:53 |  | Change the CLI so that the default "--title" is "always" instead of "on" for some display modes. This check-in also include a fix to "--title always" for the "markdown" display mode. (check-in: c3b0fff1b7 ... user: drh tags: title-always-default-on) | 
| 13:53 |  | Add the "--rowcount on" and "--title always" features to QRF, the CLI, and the TCL interface. Make them on by default for "--style psql". (check-in: 4edba31af4 ... user: drh tags: trunk) | 
| 13:24 |  | Corrections to the mode matrix in the CLI (closed check-in: 0bca306165 ... user: drh tags: psql-mode) | 
| 13:16 |  | Fix a problem with --rowcount found by Claude in code review. (check-in: bc5fb05aa6 ... user: drh tags: psql-mode) | 
| 12:24 |  | Improved title separator in QRF when using "-title always" on an empty row. Improved comments on the mode matrix in the CLI code. Additional test cases. (check-in: a102d28fca ... user: drh tags: psql-mode) | 
| 11:00 |  | Show --rowcount for "--style insert". New test cases. (check-in: 386c40e3a9 ... user: drh tags: psql-mode) | 
| 00:23 |  | Continuing work on --rowcount and --title always. (check-in: adeb4fbb86 ... user: drh tags: psql-mode) | 
| 2026-07-06 |  |  | 
| 19:24 |  | Add new option "bRowCount" to QRF. Add QRF_Always as a possibility for bTitles. Use these feature to enhance ".mode psql" to be a better approximation of what psql actually does. (check-in: 600c6b4640 ... user: drh tags: psql-mode) | 
| 16:26 |  | Reduce divergence between ext/misc/fossildelta.c, the delta code in ext/rbu/sqlite3rbu.c, and Fossil itself. (check-in: 7f49a7a90e ... user: drh tags: trunk) | 
| 16:04 |  | Fix an OOB read caused by passing an out-of-range phrase number to fts5 auxiliary API xPhraseFirstColumn() in detail=columns mode. (check-in: aeb85fdfca ... user: dan tags: trunk) | 
| 15:13 |  | Fix a problem in the fossil-delta implementation within RBU causing it to reject some well-formed delta records. Bug 2026-07-06T13:01:22Z. This fixes a problem introduced by check-in [67271c31292bc1bd] and published in version 3.53.3. (check-in: 0bfba3824f ... user: dan tags: trunk) | 
| 14:37 |  | Fix another buffer overread in RBU that could occur when processing an RBU database with corrupt rbu_control fields. Bug 2026-07-06T13:01:41Z. (check-in: 46c2476b10 ... user: dan tags: trunk) | 
| 14:10 |  | Fix an OOB read that could occur in RBU when using a state db with corrupt contents. Bug 2026-07-06T13:02:00Z. (check-in: 9e54159f9a ... user: dan tags: trunk) | 
| 13:43 |  | Avoid a (harmless) 1-byte OOB read due to malformed inputs in the normalize.c extension. Bug 2026-07-06T13:02:37Z (check-in: b1d7123bc6 ... user: drh tags: trunk) | 
| 11:58 |  | Return an error if a patchset is passed to sqlite3rebaser_configure(). This was causing a buffer overread. Bug reported by Owais Lone (thesecguy). (check-in: 887a29a837 ... user: dan tags: trunk) | 
| 11:34 |  | In the strdup.c expension, do not invoke memcpy() with a NULL pointer even if the count is zero. (check-in: c56da4bba1 ... user: drh tags: trunk) | 
| 08:36 |  | Avoid a buffer overread in the session module when an UPDATE change is rebased against a baseline that records fewer columns than the UPDATE. Bug 2026-07-05T13:57:01Z. (check-in: ea26bf3cd7 ... user: dan tags: trunk) | 
| 2026-07-05 |  |  | 
| 19:18 |  | Properly handle errors from the readlink() system call in the fileio.c extension. Bug 2026-07-05T12:06:32Z (check-in: 85effd2ad3 ... user: drh tags: trunk) | 
| 18:28 |  | Fix the parse_create_index() SQL function created by the incremental integrity_check extension so that it works for widely correct CREATE INDEX statements. Bug 2026-07-05T12:00:57Z. Also fix the extension so that it removes the parse_create_index() function. Add the sqlite3_intck_register() API function so that the parse_create_index() function can be more easily tested. (check-in: bd28985523 ... user: drh tags: trunk) | 
| 16:06 |  | Be sure to invoke the authorizer callback on all terms of the USING clause in a RIGHT JOIN. Bug 2026-07-05T08:15:45Z (check-in: bbd69ce274 ... user: drh tags: trunk) | 
| 2026-07-04 |  |  | 
| 15:09 |  | Add the strdup() SQL function extension for use in testing. (check-in: 521a7cf844 ... user: drh tags: trunk) | 
| 12:12 |  | The jsonSkipLabel() routine, part of json_each() and json_next(), should never return an index outside of the range of bytes of the input JSONB. Bug 2026-07-04T04:58:54Z (check-in: 96072f5f87 ... user: drh tags: trunk) | 
| 09:22 |  | Fix possible one-byte OOB read in the fossildelta.c extension. Bug 2026-07-04T04:53:59Z (check-in: 8001a373d1 ... user: drh tags: trunk) | 
| 08:48 |  | Fix possible 1-byte OOB read when decoding a malformed FLOAT5 element in JSONB. Bug 2026-07-04T04:59:19Z (check-in: 6ed20ab693 ... user: drh tags: trunk) | 
| 2026-07-03 |  |  | 
| 20:26 |  | Improved data validation in fts5SecureDeleteOverflow(). Bug 2026-07-03T19:16:18Z (check-in: bb1588f04a ... user: drh tags: trunk) | 
| 19:13 |  | Avoid an integer overflow in the string allocator for the snippet() function of FTS3/4, when SQLITE_MAX_LENGTH is unusually large. Bug 2026-07-03T18:34:04Z (check-in: b2816b4d6a ... user: drh tags: trunk) | 
| 2026-07-02 |  |  | 
| 09:52 |  | In ext/wasm, add the ability to omit specific JS APIs and VFSes at build-time. (check-in: 0d70a15c3b ... user: stephan tags: trunk) | 
| 2026-07-01 |  |  | 
| 12:11 |  | Enhancements to date/time functions: The "weekday -N" modifier moves the date backward in time. The "end of day/month/year" modifiers added. (check-in: 88901fffcd ... user: drh tags: trunk) | 
| 11:57 |  | Add "end of day/month/year" modifiers to date/time functions. (closed check-in: 2c4cb3223c ... user: drh tags: prev-weekday) | 
| 03:02 |  | Enhance the "weekday N" modifier for date/time functions such that if the argument is negative, it move the date backwards in time to the matching weekday, rather than forwards. (check-in: e31bea6ce1 ... user: drh tags: prev-weekday) | 
| 2026-06-29 |  |  | 
| 15:11 |  | Add new assert()s to help static analyzers realize that the code is correct. (check-in: 007f049661 ... user: drh tags: trunk) | 
| 11:59 |  | Rename omit-api=all to omit-api=ALL to defend against breaking on a substring match of 'all' in as-yet-hypothetical future feature names. (closed check-in: 225dd27e14 ... user: stephan tags: js-omit-pieces) | 
| 11:36 |  | Add worker1 to the list of omit-api=X values, per downstream request. Add omit-api=all shorthand. (check-in: 97ab248b4d ... user: stephan tags: js-omit-pieces) | 
| 11:22 |  | Merge in trunk for continued work on this branch. (check-in: 14bdec7494 ... user: stephan tags: js-omit-pieces) | 
| 2026-06-28 |  |  | 
| 19:05 |  | Mark shadow tables when registering a new virtual table that already exists after the schema has loaded. Test cases in TH3. Bug 2026-06-26T12:45:51Z. (check-in: 3e80958a70 ... user: drh tags: trunk) | 
| 16:40 |  | Fix incorrect header comment on the sqlite3ShadowTableName() function. Comment chagne only. No changes to code. (check-in: b5af1a5c7f ... user: drh tags: trunk) | 
| 00:21 |  | Check for oversized iTermOff in fs5SegIterNextInit() and trigger a corruption error if detected. Bug 2026-06-27T18:46:54Z (check-in: 130d4d10da ... user: drh tags: trunk) | 
| 2026-06-27 |  |  | 
| 23:04 |  | Do not allow an integer overflow to bypass a leaf-term bound check in the integrity-check function of FTS5. Bug 2026-06-27T18:47:28Z (check-in: 74ba971bfa ... user: drh tags: trunk) | 
| 21:52 |  | Make sure local functions in the series.c extension have only file scope. Forum 2026-06-27T20:38:26Z. (check-in: d8c59f264b ... user: drh tags: trunk) | 
| 20:30 |  | In the amatch.c and fuzzer.c extensions (both of which are experimental, for testing purposes only, and are not part of any deliverable) treat NULL inputs as empty strings. Bug 2026-06-27T19:32:29Z. (check-in: 7bcd606c8a ... user: drh tags: trunk) | 
| 20:18 |  | Put reasonable input limits on the (non-deliverable and obsolete) amatch.c extension. Bug 2026-06-27T19:15:11Z (check-in: 2fe1366e64 ... user: drh tags: trunk) | 
| 16:07 |  | Improved detection of CTEs being misused as table-valued functions. Remove an incorrect assert() from check-in [716782abe939083b]. (check-in: ca197caec7 ... user: drh tags: trunk) | 
| 15:25 |  | Add a new sqlite3FaultSim() to make it easier to test the changes in check-in [942498724f5bb83b]. (check-in: a4472a17e8 ... user: drh tags: trunk) | 
| 2026-06-26 |  |  | 
| 19:31 |  | Fix a harmless "jump or move depends on uninitialised value" warning that could occur when using a virtual table as the RHS of an outer join within a trigger. (check-in: 716782abe9 ... user: dan tags: trunk) | 
| 15:27 |  | Enhance the documentation on sqlite3_set_authorizer() to make it clear that the authorizer callback is not invoked for elements of an expression in a generated column. Bugs 2026-06-26T12:42:48Z. (check-in: 45213c1c39 ... user: drh tags: trunk) | 
| 15:04 |  | Fix both a buffer overread and overwrite in the sessions rebase module. Bug 2026-06-26T12:16:48Z. (check-in: cc36f17413 ... user: dan tags: trunk) | 
| 14:36 |  | Fix parsing of unterminated "[" tokens in the incremental integrity check extension. Bug 2026-06-26T10:05:53Z. (check-in: c8f143bd61 ... user: drh tags: trunk) | 
| 14:20 |  | Avoid 32-bit integer overflow in the zipfile extension given ridiculous arguments. Bug 2026-06-26T11:40:02Z (check-in: 7eedf458ca ... user: drh tags: trunk) | 
| 14:08 |  | Fix a buffer overread that could occur in fts5 within a memcmp() when doing an integrity-check on corrupted records. Bug 2026-06-26T08:30:13Z. (check-in: 062597f10a ... user: dan tags: trunk) | 
| 13:51 |  | Fix parsing of quoted instantiation arguments in the spellfix extension. Bug 2026-06-26T10:52:36Z (check-in: c2e963ad94 ... user: drh tags: trunk) | 
| 13:41 |  | Fix unbounded whitespace skip in the decimal extension. Bugs 2026-06-26T10:06:54Z (check-in: 71d4cfe5a3 ... user: drh tags: trunk) | 
| 13:21 |  | Extra margin to prevent queue overflow in the RTree extension. Bug 2026-06-26T12:15:33Z (check-in: de40bfb6b3 ... user: drh tags: trunk) | 
| 13:00 |  | Fix the (non-deliverable, testing-use-only) closure extension so that it parses quoted instantiation arguments correctly. Bug 2026-06-26T10:06:27Z (check-in: 11ffa3bfba ... user: drh tags: trunk) | 
| 12:34 |  | Mark the diskused() extension SQL function as direct-only. Bug 2026-06-26T10:52:15Z (check-in: b66292820a ... user: drh tags: trunk) | 
| 11:36 |  | Fix the size of an allocation in fts3 to take into account that when merging a phrase doclist, each entry may grow by up to 1 byte if the rhs position of the merged lists is stored. Bug 2026-06-26T08:08:39Z. (check-in: df5728d79f ... user: dan tags: trunk) | 
| 09:55 |  | Improved detection of corrupt freelist chains on btree pages. Plus, add assert()s to verify that the min-heap does not overflow inside of PRAGMA integrity_check. (check-in: 942498724f ... user: drh tags: trunk) | 
| 2026-06-25 |  |  | 
| 20:26 |  | Check super-journal names for validity earlier, as this simplifies verification that the logic is correct. (check-in: 6ba9e19430 ... user: drh tags: trunk) | 
| 17:07 |  | Have API functions sqlite3_serialize, sqlite3_limit, sqlite3_busy_timeout, sqlite3_error_offset, sqlite3_db_name, sqlite3_get_autocommit, sqlite3_errcode, sqlite3_total_changes64, sqlite3_changes64, sqlite3_last_insert_rowid and sqlite3_expired take and hold the database mutex while running. (check-in: 7bfd5c810c ... user: dan tags: trunk) | 
| 16:24 |  | Also have functions sqlite3_db_name(), sqlite3_get_autocommit(), sqlite3_extended_errcode(), sqlite3_errcode(), sqlite3_total_changes64(), sqlite3_changese64(), sqlite3_last_insert_rowid() and sqlite3_expired() take the database mutex. (closed check-in: 46509e48d3 ... user: dan tags: db-mutex-fixes) | 
| 15:47 |  | Have API functions sqlite3_serialize(), sqlite3_limit(), sqlite3_busy_timeout() and sqlite3_error_offset() take the database mutex. (check-in: fff2c37b9f ... user: dan tags: db-mutex-fixes) | 
| 15:17 |  | Change the undocumented and unsupported sqlite_log() SQL function so that it is marked as unsafe and direct-only. Bug 2026-06-25T10:19:43Z. (check-in: c30ea3cefe ... user: drh tags: trunk) | 
| 14:48 |  | Improve the sqlite3_str_vappendf() routine for additional robustness in the face of over-sized inputs. Bug 2026-06-25T11:29:23Z. (check-in: d7e4b2a85f ... user: drh tags: trunk) | 
| 14:25 |  | Have sqlite3_drop_modules() take the database mutex. Bug 2026-06-25T12:11:37Z . (check-in: 533e59b40c ... user: dan tags: trunk) | 
| 10:36 |  | Add test case to backup5.test to verify that detaching the destination database from its handle is handled correctly. (check-in: 50cce47583 ... user: dan tags: trunk) | 
| 00:15 |  | Fix a harmless false-positive compiler warning on Windows. (check-in: dae7575469 ... user: drh tags: trunk) | 
| 00:09 |  | Alternative to [15a6482300bb2804] that is testable. (check-in: 847e6a6574 ... user: drh tags: trunk) | 
| 2026-06-24 |  |  | 
| 20:20 |  | Check for cells extending too far past the end of a page while search an index. Bug 2026-06-24T14:55:57Z (check-in: 6d0696706a ... user: drh tags: trunk) | 
| 20:17 |  | Remove ALWAYS() and NEVER() macros used for developmemt testing on this branch. (closed check-in: f3532fc55a ... user: drh tags: index-cell-overflow) | 
| 19:28 |  | When rolling back a journal that contains a super-journal pointer, only attempt to unlink the super-journal if (a) the filename looks like one that SQLite might have generated, and (b) the super-journal contains the name of the journal being rolled back. This is to limit the extent to which SQLite can be caused to delete arbitrary files by supplying it with a crafted hot-journal. Report 2026-06-24T14:18:00Z. (check-in: 6d8da8861c ... user: dan tags: trunk) | 
| 19:07 |  | Update mjournal.test so that it works on windows. (closed check-in: 6007b5c2d3 ... user: dan tags: del-super-filter) | 
| 18:51 |  | Only delete a super-journal if it contains a pointer to the current database's journal. Test cases in this commit might be broken on some platforms. (check-in: c768be03d4 ... user: dan tags: del-super-filter) | 
| 18:50 |  | Check for cells that extend off the end of a page when searching indexes. This is a work-in-progress. This check-in includes four NEVER() and ALWAYS() macros for which simple test cases are needed. (check-in: 27ffb3eefe ... user: drh tags: index-cell-overflow) | 
| 18:03 |  | Do not delete super-journal files named "xyz.9hh" unless the library is built with SQLITE_ENABLE_8_3_FILENAMES. (check-in: 3973e37a1e ... user: dan tags: del-super-filter) | 
| 17:14 |  | When rolling back a journal that contains a super-journal pointer, only attempt to unlink the super-journal if the filename looks like one that SQLite might have generated. This is to limit the extent to which SQLite can be caused to delete arbitrary files by supplying it with a crafted hot-journal. Bug 2026-06-24T14:18:00Z. (check-in: 0cdde67558 ... user: dan tags: del-super-filter) | 
| 14:17 |  | Avoid trying to delete a database file while it is still open in backup5.test. (check-in: 395cbed103 ... user: dan tags: trunk) | 
| 13:46 |  | Fix a comment in os_kv.c so that it does not match "###" and thus does not cause false-positives when searching the output of gcov. No changes to code. (check-in: 5482548b6b ... user: drh tags: trunk) | 
| 13:45 |  | Fix the sqlite3ParseUri() for improved defenses against ridiculously long URI inputs causing integer overflow problems. Bug 2026-06-24T11:46:39Z. (check-in: 140a693ee0 ... user: drh tags: trunk) | 
| 13:42 |  | Revert the fixes to backup in [1f940357f7] and [e5db80350c] and instead fix the problem reported by forum post 15d82885e2 (that a call to sqlite3_deserialize() after sqlite3_backup_init() but before the first call to sqlite3_backup_step() on the destination db of a backup could cause a crash) by deferring caching the pointer to the destination Btree until after it is locked. This addresses bug 2026-06-24T08:41:13Z. (check-in: 18da575959 ... user: dan tags: trunk) | 
| 13:37 |  | Remove a stray C++ style comment left in build.c by the previous commit on this branch. (closed check-in: bfce01a4b0 ... user: dan tags: attach-backup-fix) | 
| 12:40 |  | Defend against integer overflow on oversized string inputs to sqlite3_mprintf() and similar C-language interfaces when using the "%!.*s" conversion. The problem is not reachable from SQL due to string length restrictions in SQL. C-code is required. Bug 2026-06-24T11:57:36Z. (check-in: 15a6482300 ... user: drh tags: trunk) | 
| 12:23 |  | Back out [23936786e6] because [9725b513c0] obviates it. (check-in: ef5e415303 ... user: stephan tags: trunk) | 
| 12:22 |  | Update documentation to explain that a odd nByte parameter to sqlite3_prepare16() is undefined behavior. In addition, always round down the nByte parameter to an even number so that it in fact harmless to give it an odd nByte value. Bug 2026-06-24T11:25:24Z. (check-in: 7af7c501b6 ... user: drh tags: trunk) | 
| 11:51 |  | Fixes for SQLITE_OMIT_FLOATING_POINT. (check-in: 9725b513c0 ... user: drh tags: trunk) | 
| 11:50 |  | Revert the fixes to backup in [1f940357f7] and [e5db80350c] and instead fix the problem reported by forum post 15d82885e2 (that a call to sqlite3_deserialize() after sqlite3_backup_init() but before the first call to sqlite3_backup_step() on the destination db of a backup could cause a crash) by deferring caching the pointer to the destination Btree until after it is locked. This addresses bug 2026-06-24T08:41:13Z. (check-in: 44a0f396da ... user: dan tags: attach-backup-fix) | 
| 10:22 |  | Swap two lines to get the amalgamation building again with SQLITE_OMIT_FLOATING_POINT, noting that neither the shell nor the canonical library build with that flag. Reported in forum post 2026-06-24T07:21:05Z. (check-in: 23936786e6 ... user: stephan tags: trunk) | 
| 09:32 |  | Add vtab to the list of supported omit-api=X options and teach tester1 to gracefully handle that. (check-in: e4a350e684 ... user: stephan tags: js-omit-pieces) | 
| 09:19 |  | Have the npm target forward the omit-api flag to its sub-make. Doc typo fixes. (check-in: c65153b28f ... user: stephan tags: js-omit-pieces) | 
| 09:09 |  | Teach the JS build to filter out various APIs/VFSes when passed omit-api='api names...'. Fully trimmed this saves just shy of 300kb from the JS. Make loud=1 mode distinctly louder when preprocessing. (check-in: 2603c85579 ... user: stephan tags: js-omit-pieces) | 
| 2026-06-23 |  |  | 
| 20:42 |  | Add preprocessor guards to various JS files working towards the ability to build, e.g., with only one of various VFSes included. The makefile does not yet integrate these - this is an incremental step towards addressing the discussion in the downstream npm project's PR#168. (check-in: f6618a693b ... user: stephan tags: js-omit-pieces) | 
| 20:12 |  | Merge the fixes for the use-after-free problem caused by executing an ATTACH when a backup operation was active, and for the OOB read in the unused spellfix1 extension. (check-in: e458ed9cc9 ... user: dan tags: trunk) | 
| 19:49 |  | Add preprocessor guards to various JS files working towards the ability to build, e.g., with only one of various VFSes included. The makefile does not yet integrate these - this is an incremental step towards addressing the discussion in the downstream npm project's PR#168. (Edit: closing - failed to notice branch switch after an update. Will cherrypick elsewhere.) (closed check-in: 9b9b0b733a ... user: stephan tags: backup-fix) | 
| 19:46 |  | Fix sqlite3_backup_init() to correctly handle the case where opening the temp database fails. (check-in: c08bb97f39 ... user: dan tags: backup-fix) | 
| 19:28 |  | Fix an OOB read on the unused spellfix1 extension. The OOB read is one byte prior to the start of an allocation, which is almost always harmless. Bug 2026-06-23T18:48:40Z. (check-in: e58d5efc7b ... user: drh tags: backup-fix) | 
| 19:14 |  | Avoid a use-after-free problem that could occur if ATTACH statements are executed while a backup operation is active. Bug 2026-06-23T15:46:48Z. (check-in: 9454592878 ... user: dan tags: backup-fix) | 
| 19:08 |  | Do not allow internal-use-only functions to be coded, anywhere, unless inside of a nested parse (which is the case for ALTER TABLE) or if the appropriate test-control is activated. Bug 2026-06-23T15:49:27Z. (check-in: a145307e4d ... user: drh tags: trunk) | 
| 15:35 |  | Fix a typo in a comment. No code changes. (check-in: 8a535a36a9 ... user: drh tags: trunk) | 
| 14:11 |  | Makefile doc tweaks. No functional changes. (check-in: d85204bbd9 ... user: stephan tags: trunk) | 
| 13:26 |  | Improve robustness against corrupt kvvfs journal files. Addresses bug report 2026-06-23T06:51:34Z and the first follow-up report in that thread. (check-in: bd24ea4d76 ... user: stephan tags: trunk) | 
| 11:14 |  | Fix a NULL pointer dereference that could occur in the unionvtab virtual table if the configuration SQL returned an SQL NULL value in place of a table name. Bug 2026-06-23T05:32:49Z. (check-in: 3a2163f969 ... user: dan tags: trunk) | 
| 11:09 |  | Fix a NULL pointer dereference that can occur when misusing a extension virtual table that was created for testing purposes. Bug 2026-06-23T05:32:49Z (check-in: 256d5cd478 ... user: drh tags: trunk) | 
| 10:51 |  | Fix an OOB read in the next_char() function of the "spellfix" extension. The spellfix extension is not a deliverable. Bug 2026-06-23T05:41:00Z (check-in: 6ede22e13f ... user: drh tags: trunk) | 
| 10:23 |  | Fix dequoting error on the instantiation parameters of the non-deliverable extension virtual table "fuzzer. Bug 2026-06-23T05:51:36Z. (check-in: 410045796b ... user: drh tags: trunk) | 
| 10:07 |  | Fix rounding behavior for all-9s decimals in the decimal extension. Bug 2026-06-23T05:07:48Z (check-in: 21b2692c83 ... user: drh tags: trunk) | 
| 2026-06-22 |  |  | 
| 11:02 |  | Add the BTREE_HINT_TABLECURSOR cursor-hint. This is not currently used by stock SQLite. (check-in: 35b3ff4007 ... user: dan tags: trunk) | 
| 2026-06-21 |  |  | 
| 11:38 |  | Correct test from [96a48a15760a] - that journal was indeed corrupt, but not in the intended manner. Add a verification that the bad journal gets replaced after a write. (check-in: c917122116 ... user: stephan tags: trunk) | 
| 11:10 |  | Extend the test from the previous check-in a bit. (check-in: bd6bbd0e84 ... user: stephan tags: trunk) | 
| 10:55 |  | Add a JS test demonstrating that [7c4db1cd2d06] recovers gracefully from journal corruption. (check-in: 96a48a1576 ... user: stephan tags: trunk) | 
| 08:38 |  | Fix a result value semantics bug which broke application of a journal in kvvfs. Fix a memory-reset bug in hypothetical non-WASM builds of kvvfs. Bug report 2026-06-20T18:22:59Z, and this resolves the journaling issue mentioned in the parent post. (check-in: 7c4db1cd2d ... user: stephan tags: trunk) | 
| 08:13 |  | Fix a potential integer overflow when decoding a corrupt kvvfs block. Bug report 2026-06-20T18:35:54Z. (check-in: c36fc5df62 ... user: stephan tags: trunk) | 
| 2026-06-20 |  |  | 
| 21:54 |  | Fix an incorrect assert() in check-in [984c9b181801c1de] (check-in: 1d41c93b36 ... user: drh tags: trunk) | 
| 11:07 |  | Correct misuse of a loop control variable in writing of a kvvfs journal. Bug report 2026-06-20T08:49:41Z. Remove a snippet of dead code in the JS kvvfs side. (check-in: a396d7c54c ... user: stephan tags: trunk) | 
| 2026-06-19 |  |  | 
| 23:53 |  | Add the sqlite3_result_str() interface and use it to simplify the code. (check-in: 8f8361edf8 ... user: drh tags: trunk) | 
| 22:57 |  | Use SQLITE_FINISH in a few places in the core, to help ensure that path is well tested. (closed check-in: 7356bf3a81 ... user: drh tags: sqlite3-result-str) | 
| 22:17 |  | Put the sqlite3OomStr singleton in "const" space to ensure that it is never modified. Doing so identified a long-standing bug. (check-in: 2fe53e6d68 ... user: drh tags: sqlite3-result-str) | 
| 21:43 |  | Additional changes based on Claude analysis. (check-in: d4691a0db1 ... user: drh tags: sqlite3-result-str) | 
| 20:01 |  | Further refinements and uses for sqlite3_result_str(). (check-in: 974253a605 ... user: drh tags: sqlite3-result-str) | 
| 17:03 |  | Prototype implementation for a new sqlite3_result_str() API. (check-in: cdbc62c36a ... user: drh tags: sqlite3-result-str) | 
| 16:55 |  | Fix the format() SQL function so that it returns an empty string not a NULL if the first argument is an empty string. Bug 2026-06-19T16:40:04Z (check-in: ece7af98a8 ... user: drh tags: trunk) | 
| 11:46 |  | Fix a cast in fts3_write so that it is applied as intended. Bug 2026-06-19T07:56:23Z. (check-in: d6a3272e55 ... user: dan tags: trunk) | 
| 11:14 |  | Fix an fts5 problem causing attempts to create locale=1 tables with more than 115 columns to fail. Bug 2026-06-19T05:19:08Z. (check-in: 979229f720 ... user: dan tags: trunk) | 
| 2026-06-18 |  |  | 
| 17:09 |  | Fix a memory size computation error on 32-bit platforms caused by check-in [9a2a230464e43140]. (check-in: dc4763b14c ... user: drh tags: trunk) | 
| 2026-06-17 |  |  | 
| 20:11 |  | Small parsing performance improvement by reducing the amount of the Parse object that needs to be initialized. (check-in: 0901b7c2df ... user: drh tags: trunk) | 
| 19:28 |  | Remove the unnecessary Parse.szOpAlloc field, for small size reduction and performance improvement. (check-in: 9a2a230464 ... user: drh tags: trunk) | 
| 18:39 |  | Small performance boost in the sqlite3VdbeResolveLabel() routine of the code generator. (check-in: f786f3643e ... user: drh tags: trunk) | 
| 16:57 |  | Safer conversion from double to 64-bit integer in two places. (check-in: 17c4443829 ... user: drh tags: trunk) | 
| 14:53 |  | Additional refinements to the large floating-point handling in generate_series(). Follow-up to [5aab8c2cde63f9db]. (check-in: 4247c5d9af ... user: drh tags: trunk) | 
| 13:52 |  | Fix CLI CSV test cases so that they suppress the CR on windows and thus work cross-platform. (check-in: 7a2c96405e ... user: drh tags: trunk) | 
| 13:31 |  | Fix the OR-to-IN optimization so that it works in cases where there are conflicting collation sequences on the equality constraints within the OR. Bug 2026-06-17T05:04:48Z. (check-in: 984c9b1818 ... user: drh tags: trunk) | 
| 2026-06-16 |  |  | 
| 23:59 |  | Typo fix in a comment used to generate documentation. No changes to code. Forum 2026-06-16T23:18:50Z. (check-in: 7386faef61 ... user: drh tags: trunk) | 
| 18:06 |  | After opening a non-covering index cursor, pass the corresponding table cursor to the implementation via sqlite3BtreeCursorHint(). (closed check-in: d48c6d2dcd ... user: dan tags: cursor-hint-tblcsr) | 
| 13:43 |  | Limit VIEW recursion depth to SQLITE_LIMIT_EXPR_DEPTH to prevent static overflow in malicious schemas with tens of thousands of levels of recursive views. Bug 2026-06-16T04:21:51Z (check-in: 3f3fb9b638 ... user: drh tags: trunk) | 
| 11:53 |  | Fix instances of potential buffer overflow caused by loading the contents of a stat4 table with an absurd number of samples in a 32-bit build. Bug 2026-06-16T11:30:52Z. (check-in: 0b72246732 ... user: dan tags: trunk) | 
| 09:53 |  | Fix a possible call to memcpy() with a NULL source pointer when the size parameter is zero (which is technically undefined behavior but in practice is completely harmless) in the sha1_query() extension function. Bug 2026-06-16T07:42:23Z. (check-in: 2063926473 ... user: drh tags: trunk) | 
| 2026-06-15 |  |  | 
| 17:39 |  | Consistent error messages for excess trigger nesting. (check-in: 4fb9f2ffb4 ... user: drh tags: trunk) | 
| 17:27 |  | Speed up SQL aggregate functions percentile() and median() by using quickselect to find the required values instead of fully sorting the array of values with quicksort. (check-in: 0796d337f6 ... user: dan tags: trunk) | 
| 17:14 |  | New Windows makefile targets for common EXEs that omit the ".exe" suffix, so that when muscle memory kicks in and we type "make sqlite3" on windows, it still works. (check-in: 1152463a66 ... user: drh tags: trunk) | 
| 17:13 |  | Improve comments in percentSort(). (closed check-in: 9b43500f51 ... user: dan tags: pecentile-partial-sort) | 
| 16:45 |  | Enhance SQLITE_LIMIT_TRIGGER_DEPTH so that it also limits a chain of distinct triggers during code generation. Bug 2026-06-15T06:44:07Z. (check-in: 0b5378678e ... user: drh tags: trunk) | 
| 15:48 |  | Change the PACKAGE_BUGREPORT config option point to /bugs instead of /forum. (check-in: ca15fedede ... user: stephan tags: trunk) | 
| 15:38 |  | Fix a problem sometimes causing an UPSERT to incorrectly convert an excluded.* column from type REAL to INT in cases where doing so does not cause data loss. Bug 2026-06-15T09:08:12Z. (check-in: 71334f7ec9 ... user: dan tags: trunk) | 
| 15:27 |  | Improvements to handling of inequality constraints on generate_series().value against very large floating-point values. Bug 2026-06-15T04:37:48Z. (check-in: 5aab8c2cde ... user: drh tags: trunk) | 
| 15:24 |  | Additional tweaks and test cases for generate_series() constraints using very large floating-point numbers. (closed check-in: 50d611404a ... user: drh tags: series-boundry-cases) | 
| 14:16 |  | Better behavior for generate_series() in cases where constraints involve floating point number that are near the minimum and maximum 64-bit integer values. (check-in: 236ef21789 ... user: drh tags: series-boundry-cases) | 
| 13:39 |  | Avoid a possible buffer overrun in 32-bit builds of fts5 that could occur when processing corrupt records. Bug 2026-06-14T12:21:15Z. (check-in: 0fa3345fe4 ... user: dan tags: trunk) | 
| 12:36 |  | Revert the native impl of kvvfs's xOpen() (as distinct from the JS impl which the wasm build uses) to the historical db name restrictions of 'local' or 'session', failing with SQLITE_CANTOPEN if passed another name. [ec866b04d088e53b] overhauled support for kvvfs db names in JS but it turns out that the native impl still relies on those names in order to match journals to databases. Correct a related too-lenient assert() in the WASM pieces. (check-in: 4b3ec30c63 ... user: stephan tags: trunk) | 
| 11:47 |  | Fix a problem that could cause fts5 to return incorrect answers when performing prefix queries on tokens with synonyms. Bug 2026-06-14T11:11:06Z. (check-in: 323d87541a ... user: dan tags: trunk) | 
| 11:03 |  | Regardless of whether the platform is 32 or 64-bits, Use 64-bit arithmetic to calculate buffer sizes in the fts3_snippet(). Bug 2026-06-14T09:19:52Z. (check-in: 79dc1949b2 ... user: dan tags: trunk) | 
| 10:13 |  | Fix rounding behavior in the decimal extension when all leading digits are 9 and there is not fractional component. Bug 2026-06-15T03:49:42Z. (check-in: c6a3b18ee9 ... user: drh tags: trunk) | 
| 01:24 |  | Prevent array overflow when decoding a corrupted value from the underlying key/value store. Bug 2026-06-14T21:58:12Z (check-in: 732c8f81b5 ... user: drh tags: trunk) | 
| 2026-06-14 |  |  | 
| 22:01 |  | Remove the unused, untested, experiemental "blobio.c" extension, to avoid confusing AIs. Bug 2026-06-14T15:12:50Z (check-in: 45918d5e97 ... user: drh tags: trunk) | 
