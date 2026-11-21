---
id: collect-261121-veille-ia/langages-dev/recent-sqlite-news
title: "Recent SQLite News"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-04-09"]
keywords: ["release", "version", "sqlite", "patch", "fixes", "bugs", "enhancements", "interface"]
source: https://www3.sqlite.org/news.html
source_anchor: ""
source_lines: [1, 732]
section: "Langages_Dev"
---
Version 3.54.1 is a patch release against version 3.54.0
that fixes a few problems in that release.  Most notably,
it was possible to cause a heap buffer overflow in 3.54.0
on some 32-bit builds.  This heap buffer overflow does
not affect 64-bit machines.

Version 3.54.0 includes significant enhancements to the
command-line interface (CLI) program ("sqlite3" or "sqlite3.exe")
as well as query planner improvements, new feature,
performance improvements, and countless fixes for
AI-discovered bugs.
Beginning with this release, SQLite will not compile on Windows version prior to Windows Vista (circa 2007), due to a switch to using Slim Reader/Writer Locks (SRWLs) which are faster but which are not available on WindowsXP and earlier.

This is the last release that will include the "sqlite3_analyzer" utility program. The equivalent functionality is now available in the new .diskused command of the CLI and so the stand-alone sqlite3_analyzer program is deprecated.


SQLite version 3.53.4 is a maintenance patch release
for 3.53, with fixes for (mostly) AI-discovered bugs.
There was a huge rush of AI-aided bug reporting shortly after the 3.53.0 release (2026-04-09), but lately the rate of bug reports has declined noticably, and the bugs that are being reported are increasingly insignificant. Could it be that the AI-bug avalanche is coming to an end, and that all of the bugs that AIs are able to find have now nearly all been found and fixed? Watch this news log for updates. Perhaps this will be the last patch release for 3.53 and that the next release will be 3.54.0, with enhancements and new features.


SQLite version 3.53.3 is a maintenance patch release
for 3.53.

SQLite version 3.53.2 is a maintenance patch release
for 3.53.

SQLite version 3.53.1 is a maintenance patch release
for 3.53.

SQLite version 3.53.0 is a re-release of version 3.52.0 with
additional enhancements to address the problem of
stale expression indexes.
The WAL-reset bug is fixed in this release. This is the first major release (if you don't count 3.52.0) that contains a fix for that bug, so upgrading is recommended.

See the version 3.53.0 change log for a list of the many other enhancements included in 3.53.0.


SQLite version 3.52.0 has been withdrawn because some of
the new features found in 3.52.0 are not 100% compatible with
prior releases.  Those new features and their associated
APIs need to be reworked before 3.52 is made available.
In place of 3.52.0, patch release 3.51.3 is now available. Patch release 3.51.3 fixes the WAL-reset bug as well as other minor problems that have come to light since the 3.51.2 release.

Version 3.52.0 works fine and is fully backwards compatible as long as the database does not contain indexes on expressions or indexes on VIRTUAL computed columns where the expression evaluates to a floating-point number that is derived from text or JSONB inputs. If indexes of that type are present, then version 3.52.0 might not interoperate correct with prior releases, in rare cases.


SQLite version 3.52.0 is a major release with many important
changes, the two most important of which (in my opinion) are:

- A 15-year-old database corruption bug was discovered just a few days ago, and is now fixed.
- The command-line interface utility has been significantly enhanced, especially in how it formats query results.
See the change log for a more complete list.

The previous release was about four months ago.
We are *striving* to slow the release cadence of
SQLite to about once every six months.  This is not a promise, only
a goal. We'll see how it goes.


Patch release 3.51.2 fixes a potential (but unlikely) deadlock
in the OS interface for unix that can arise due to changes in 3.51.0
that were designed to detect broken Posix Advisory Locks cause by
the application mistakenly directly opening and closing a file 
descriptor to the database file and not going through the SQLite
interface.  The patch also fixes some other obscure bugs that are
of little importance, but still ought to be addressed.  Changes from
3.51.0 and 3.51.1 are minimal.

Patch release 3.51.1 fixes two cases of incorrect answers to
corner-case queries caused by new optimizations in version 3.51.0.

Version 3.51.0 is the first major update to SQLite since the
25th anniversary release on 2025-05-29. Version 3.51.0 
includes new features, new interfaces, and new optimizations.
See the 3.51.0 change log for more detail.

Version 3.50.4 is a patch release to fix minor problems reported
since the previous release.

Version 3.50.3 is a patch release to fix minor problems reported
since the previous release.

Version 3.50.2 is a patch release to fix minor problems reported
since the previous release.

Version 3.50.1 is a small patch to fix minor problems in the 3.50.0
release.  Upgrading is recommended.

Version 3.50.0 is the twenty-fifth anniversary release of SQLite.

Patch release 3.49.2 fixes various bugs.  The most important problem is
a mistake in the NOT NULL optimization that was added to version 3.40.0
(see item 3c in the version 3.40.0 change log) that if
abused by an attacker with SQL injection capabilities can lead to read
off the end of a static array, resulting in a segfault and possible
denial of service.  This patch release is mostly to address that one
mistake.  The other fixes could have waited for the next major release.
The 3.50.0 release is still scheduled for 2025-05-29, which is only about three weeks away. All the same bug fixes will be in the 3.50.0 release, so you might want to wait for that release to come up before upgrading.


Patch release 3.49.1 fixes a problem in the concat_ws() SQL function.
The problem has existed since version 3.44.0 (2023-11-01) but was
only discovered two days ago.  The problem is a potential vulnerability.
An attacker who can control the separator string and can make that
string very large (hundreds of megabytes) could cause a memory error.
Your system is not vulnerable if you are following the 
recommended application security practices, especially recommendation
number 2, or if your system does not use the concat_ws() function and
is impervious to SQL injections.  Regardless of whether or not your
system is subject to this problem, you will do well to upgrade to version
3.49.1 or later.
The 3.49.1 patch release also makes multiple improvements to the portability of configure scripts and makefiles. These improvements involve a lot of change, but only to makefiles and configuration scripts. The core SQLite library is unaffected by those enhancements.


There were a lot of minor patches and bug fixes that needed to be added
to the 3.48.0 release.  We were originally planning to do a patch release
3.48.1 to publish those changes.  But we noticed that the patches formed
a significant fraction of all changes on the main development trunk, and
the trunk was stable, so we decided instead just to
do an early release of version 3.49.0.
Hence, the 3.49.0 release is being published less than one month after the 3.48.0 release. Version 3.49.0 contains both patches for minor issues in the 3.48.0 release and new enhancements. See the change log for details.

We expect that the next major release (3.50.0) will resume the usual 5-to-7 month cadence.


Version 3.48.0 refactors the "./configure" system of SQLite to fix bugs,
simplify usage, improve performance, and make it easier to maintain moving
forward.  Though the internal changes are extensive, the user interface
should remain the same.  Unless you were doing something bizarre and
undocumented with ./configure, you should not notice the difference.
**NB:** This change applies to the configure system in the
canonical source code only.  The amalgamation tarball
(the sqlite-autoconf-xxxxxx.tar.gz download) is unaffected.

The ./configure implementation has changed to use Autosetup instead of GNU Autoconf. One benefit of this is that TCL is no longer necessary to build SQLite from canonical sources, thus simplifying the build process. Just download the canonical source code, unpack it, and run "./configure && make". No third-party software packages (other than the C compiler and related tools like "make") are needed in most cases. The Windows makefile ("Makefile.msc") also now runs without TCL. Note, however, that TCL is still required to run tests or to build tools such as sqlite3_analyzer that link against libtcl.

There are other minor enhancements in the 3.48.0 release, but the refactor of the ./configure system is by far the biggest and most important change. See the change log for more detail.


Versions 3.47.0 and 3.47.1 contain a bug in the text-to-floating-point
conversion routines such that numbers where the 16
most significant digits are "1844674407370955" might be converted to
floating point incorrectly on x64 and i386 hardware.  This is not an
issue on ARM or PPC (the only other CPUs that the SQLite devs have
access to), only x64/i386.
Version 3.47.2 addresses the problems.  You should upgrade to
3.47.2 or later if you are using any prior 3.47.x release.

Version 3.47.1 is a patch release with minimal changes from
version 3.47.0.  The purpose of the patch is to fix various
issues in the 3.47.0 release.

Version 3.47.0 is an enhancement release with many
feature improvements and performance enhancements.  Of particular note:

- The sqlite3_rsync command-line utility is now available for doing bandwidth-efficient live backups over SSH.
- Integration with TCL9.
See the change log for details.


Version 3.46.1 is a patch release that fixes several minor
problems in version 3.46.0.  There are minimal changes from
the previous release.

Version 3.46.0 is an enhancement release.  Enhancements include:

- Improvements to the PRAGMA optimize command
- Enhancements to the date and time functions
- The ability to put "_" between digits in numeric literals for readability
- Added the json_pretty() function
- Improvements to the query planner, especially a new faster implementation for VALUES clauses with many terms.
None of these enhancements are critical. Update at your convenience.


Version 3.45.3 is a patch release that fixes a few obscure problems,
including:
None of the problems are emergencies. Upgrade at your convenience.

- The "old.*" values in an UPDATE trigger might be incorrect if the trigger fires in response to an UPSERT.
- The sum() function might return NULL in some cases where Infinity would be a better answer.

Version 3.45.2 is a patch against SQLite versions 3.45.0 and 3.45.1.
The primary reason for this patch is to fix two three-year-old bugs identified by Forum posts 919c6579c8 and 440f2a2f17. These problems could result in incorrect query results or corrupt indexes. See the associated forum threads for details.

Other trifling fixes are also included in the patch.


Version 3.45.1 is a patch against SQLite version 3.45.0.
The main focus of this patch release is to restore certain undocumented legacy behavior in the JSON SQL functions that the developers were unaware of but which some applications had come to depend on. This undocumented behavior was "fixed" in the 3.45.0, resulting in breakage for applications that were using it. So it has now been restored and documented.

Other obscure issues that have come up since the 3.45.0 release, some related to the release itself and some going back years, are also fixed.


Version 3.45.0 is an enhancement release of SQLite.
The most impactful change is likely enhancements to the JSON SQL functions such that they are now able to store their internal JSON parse trees in the database as BLOB values. This can significantly increase the performance of applications that process large JSON strings as it omits the need to translate JSON text to and from the internal binary format used by SQLite.

A second noteworthy change is that any application-defined SQL functions that make use of the sqlite3_result_subtype() interface must now include the SQLITE_RESULT_SUBTYPE attribute when the function is registered using sqlite3_create_function() or similar. Failure to include the SQLITE_RESULT_SUBTYPE attribute on functions that use sqlite3_result_subtype() might result in incorrect answers.

See the change log for additional enhancements that are part of the 3.45.0 release.


The CLI fix in version 3.44.1 introduced a new bug, which is
fixed by patch release 3.44.2.  Version 3.44.2 also fixes an
FTS5 problem that was found by a fuzzer just minutes after the
3.44.1 release.

Version 3.44.1 is a patch release that fixes various obscure
bugs.  There is no need to upgrade, unless you are having
problems with a prior release.

Version 3.44.0 is a new enhancement release of SQLite.
It has only been 69 days since the previous major release (3.43.0). The original plan was for version 3.44.0 to occur at a spacing of approximately 120 days from the prior release. However, the code accumulated so many important enhancements that it seemed better to accelerate the release of 3.44.0, thus getting those enhancements into circulation. This means that some enhancements that were originally planned to be in 3.44.0 have been deferred until subsequent releases.


Version 3.43.2 is a patch release that fixes a few small inaccuracies in
version 3.43.0 and 3.43.1.

Version 3.43.1 is a patch release that fixes a few small inaccuracies that were
discovered in the the 3.43.0 and/or 3.42.0 releases after the 3.43.0 release was
published.

Version 3.43.0 is a routine enhancement release of SQLite.
Key enhancements in this release include added support for
Contentless-Delete FTS5 Indexes, 
and performance improvements in JSON processing.
See the change log for
details.

Version 3.42.0 is a routine enhancement release of SQLite.
Key enhancements in this release are added support for
JSON5 and the FTS5 secure-delete command.  See the
change log for a
summary of all enhancements in this release.

Version 3.41.2 is a patch release that fixes multiple
fuzzer-found problems in prior releases.  The worst problems
include reads (not writes) past the end of a buffer.  Upgrading
is recommended.

Version 3.41.1 is a patch release that fixes various obscure
problems found in 3.41.0 and reported by users.  Upgrading
is optional.

Version 3.41.0 is a routine enhancement release.

Version 3.40.1 is a patch release that fixes some obscure problems
in version 3.40.0.  The problems fixed have no impact on most applications.
Upgrading is only necessary if you encounter problems.
The two most important fixes are these:


Fix the safe command-line option on the CLI so that it
correctly disallows functions with side-effects.  This is a bug
in the CLI — *not* a bug in the
SQLite library — and it only affects the --safe command-line
option, making that option less than fully "safe".  As the number
of systems that use the --safe command-line option in the CLI is
approximately zero, this is not considered an important bug.  However,
a third-party wrote a CVE against it which caused considerable angst
among maintainers, so it seems good to get the fix into circulation
sooner rather than wait on the next major release.

The optional memsys5 memory allocator picked up a bug that might put it into an infinite loop for very large (500MiB) allocations. Almost all systems use their native memory allocator, not memsys5. Memsys5 is only used if SQLite is compiled using SQLITE_ENABLE_MEMSYS5 and then initialized using sqlite3_config(SQLITE_CONFIG_HEAP). Very, very few systems do this, and so the problem is not considered to be serious.

See the branch-3.40 timeline for a complete listing of changes that have gone into this patch release.


Version 3.40.0 is a new feature release of SQLite.  See the
change log for details.
Key enhancements in this release include:

Official support for compiling SQLite to WASM and running it in a web browser.

New and improved recovery extension for extracting data from corrupted database files.

This release also includes performance enhancements and improvements to the query planner.

A psychological milestone: The performance benchmark that the SQLite developers have used for many years has for the first time dropped below 1 billion CPU cycles (measured using cachegrind) when run in WAL mode. This is less than half the number of CPU cycles used as recently as 8 years ago. (The graph below shows SQLite using a rollback journal which uses fewer CPU cycles at the expense of extra I/O. The benchmark passed through the 1 billion cycle barrier for rollback journals for the previous release.)



Version 3.39.4 is a security release that addresses a single
long-standing problem in the FTS3 extension.  An attacker
who is able to execute arbitrary SQL statements and who can create
a corrupt database that is 2GB or larger in size might be able to trick
FTS3 into overflowing an integer used to size a memory allocation,
causing the allocation to be too small and ultimately resulting in
a buffer overrun.  The release also includes fixes for other
obscure weaknesses, as described in the release notes.

Version 3.39.3 is a patch release that fixes a few obscure problems
in the 3.39.0 release.  See the release notes for details.

Version 3.39.2 is a security release that addresses multiple long-standing
issues in SQLite.  The most severe problem is identified by CVE-2022-35737.
That issue is associated with the auxiliary C-language APIs
of SQLite and cannot be reached using SQL or database inputs, and hence is
unlikely to affect your application.  Nevertheless, upgrading is recommended.
The other issues are comparatively minor.
This release also fixes a performance regression that appeared in version
3.39.0 that might affect some multi-way joins that use LEFT JOIN.

Version 3.39.1 is a patch release that fixes a few minor problems in version
3.39.0.  Upgrading is optional.

Version 3.39.0 is a regular maintenance release of SQLite.  The key enhancement
in this release is added support for RIGHT and FULL JOIN.  There are other
language and performance enhancements as well — see the
release notes for details.

The 3.38.4 patch release included a minor change to the CLI source code
that did not work.  The release manager only ran a subset of the normal
release tests, and hence did not catch the problem. As a result, the CLI
will segfault when using columnar output modes in version 3.38.4.  This
blunder did not affect the core SQLite library.  It only affected the CLI.
Take-away lesson:  **Always** run **all** of your tests prior to
a release - even a trival patch release.  **Always**.

The 3.38.5 patch release fixes the 3.38.4 blunder.


Another user-discovered problem in the new Bloom filter optimization
is fixed in this patch release.  Without the fix, it is possible for
a multi-way join that uses Bloom filters for two or more tables in
the join to enter an infinite loop if the key constraint on one of those
tables contains a NULL value.

Version 3.38.3 fixes a bug in the automatic-index and Bloom filter
construction logic that might cause SQLite to be overly aggressive
in the use of ON clause constraints, resulting in an incorrect
automatic-index or Bloom filter that excludes some valid rows from
output.  The bug was introduced in version 3.38.0.  Other minor
changes were tossed in to complete the patch.

Version 3.38.2 fixes another bug in the new Bloom filter
optimization that might cause incorrect answers for a
LEFT JOIN that has an IS NULL constraint on the right-hand
table.

Version 3.38.1 fixes a pair of bugs in the Bloom filter
optimization that was introduced in version 3.38.0.  These
bugs might cause incorrect answers for some obscure queries.
Various other minor problems and documentation typos were
fixed at the same time.

Version 3.38.0 is a routine maintenance release of
SQLite.  There are various minor enhancements and
about a 0.5% reduction in the number of CPU cycles
used. See the
release notes for
more detail.

Version 3.37.2 fixes a
database corruption bug.  You are
encouraged to upgrade, especially if you are using SAVEPOINT.
The problem first appeared in version 3.35.0 (2021-03-12) and affects all subsequent releases through 3.37.1. If temporary files are stored in memory (which is not the default behavior, but is sometimes selected by applications using either -DSQLITE_TEMP_STORE or PRAGMA temp_store) and if a SAVEPOINT is rolled back and then subsequent changes within the same transaction are committed, the database file might (with low but non-zero probability) go corrupt.


Version 3.37.1 fixes a bug in the UPSERT logic, introduced by
the UPSERT enhancements of version 3.35.0, that can cause
incorrect byte-code to be generated in some cases, resulting
in an infinite loop in the byte code, or a NULL-pointer dereference.
This patch release also fixes some other minor problems with
assert() statements and in the CLI.

Version 3.37.0 is a routine maintenance release of SQLite.
The biggest new feature in this release is support for
STRICT tables.  Other enhancements are described in
the release notes.

Version 3.36.0 is a routine maintenance release of SQLite.
There are no new major features, only incremental improvements
to existing features and small performance improvements.

The new ALTER TABLE DROP COLUMN capability that was added
in the 3.35.0 release contained a bug that might cause the
table content to go corrupt when the table was rewritten
to remove the dropped column.  Fixed by this patch.

Version 3.35.4 is yet another patch release to fix
obscure problems in features associated with the 3.35.0 release.

Version 3.35.3 contains patches for a handful of minor
problems discovered in prior releases.

Version 3.35.2 is a small patch release to fix some minor problems
that were discovered shortly after the 3.35.1 release.

A user discovered an issue with the new DROP COLUMN capability
in version 3.35.0, and so version 3.35.1 was created to fix it.
No need to upgrade if you are not using DROP COLUMN.

SQLite version 3.35.0 is a routine maintenance release.  This
release adds a number of new language features, including
support for ALTER TABLE DROP COLUMN, built-in math functions,
generalized UPSERT, and the MATERIALIZED hint on common table
expressions.  There are also query planner optimizations and
incremental CLI improvements.

SQLite version 3.34.1 is a patch release that fixes a possible
use-after-free bug that can be provoked by malicious SQL.  Other
minor issues in extensions and documentation are also fixed.

SQLite version 3.34.0 is a routine maintenance release.  This
release adds incremental improvements to performance and features,
including enhancements to the query planner, multiple recursive
SELECTS in recursive common table expressions, and better error messages
from CHECK constraint failures.  See the change log for details.

SQLite version 3.33.0 is a routine maintenance release.  This
release features added support for "UPDATE FROM" following the
PostgreSQL syntax, and a doubling of the maximum database size
to 281 TB, as well as many other improvements.  See the change
log for details.

The 3.32.3 release is a patch release that contains fixes for
various issues discovered by fuzzers.  None of the issues fixed
are likely to be encountered by applications that use SQLite in
ordinary ways, though upgrading never hurts.
Map of all changes since the 3.32.0 release: https://sqlite.org/src/timeline?p=version-3.32.3&bt=version-3.32.0


The 3.32.2 release is a one-line change relative to 3.32.1
that fixes a long-standing bug in the COMMIT command.  Since
version 3.17.0, if you were to retry a COMMIT command over
and over after it returns SQLITE_BUSY, it might eventually
report success, even though it was still blocked.  This patch
fixes the problem.

Grey-hats published
information about two SQLite bugs approximately 24 hours after
the release of version 3.32.0.  These bugs enable maliciously
crafted SQL to crash the process that is running SQLite.  Both
bugs are long-standing problems that affect releases prior to
3.32.0. The 3.32.1 release fixes both problems.

Version 3.32.0 is an ordinary maintenance release of SQLite.
This release features the ability to run an
approximate ANALYZE to gather database statistics for
use by the query planner, without having to scan every row
of every index.
See the change log for additional enhancements
and improvements.

Applications that use SQLite should only interface with SQLite
through the officially published APIs.  Applications should not
depend upon or use the internal data structures of SQLite as those
structures might change from one release to another.  However, there
is a popular application that does depend on the details of the
internal layout of data in an internal SQLite data structure, and
those details changed in version 3.31.0, breaking the application.
This is, technically, a bug in the application, not in SQLite.
But it is within the power of SQLite to fix it, by reverting the
internal data structure change, and so that is what we have done
for the 3.31.1 release.

Version 3.31.0 is an ordinary maintenance release of SQLite.
This release features the ability to define
generated columns for tables as well as many other enhancements.
See the change log for additional information.

Version 3.30.1 is a bug-fix release that addresses a problem
that can occur when an aggregate function in a nested query
makes use of the new FILTER clause capability.  Some additional
patches for various obscure issues are also included, for
completeness.

Version 3.30.0 is a regularly scheduled maintenance release
of SQLite containing miscellaneous performance and feature
enhancements.  This release adds support fo the NULLS FIRST
and NULLS LAST clauses on ORDER BY statements and the 
addition of FILTER clauses on all aggregate functions.
See the change log for details.

Version 3.29.0 is a regularly scheduled maintenance release
of SQLite containing miscellaneous performance and feature
enhancements.  See the change log for
details.
Beginning with this release, the double-quoted string literal misfeature is deprecated. The misfeature is still enabled by default, for legacy compatibility, however developers are encouraged to disable it at compile-time using the -DSQLITE_DQS=0 option, or at run-time using the SQLITE_DBCONFIG_DQS_DML and SQLITE_DBCONFIG_DQS_DDL actions to the sqlite3_db_config() interface. This is especially true for double-quoted string literals in CREATE TABLE and CREATE INDEX statements, as those elements can cause unexpected problems following an ALTER TABLE. See ticket 9b78184be266fd70 for an example.


Version 3.28.0 is a regularly scheduled maintenance release
of SQLite containing miscellaneous performance and feature
enhancements.  See the change log for
details.
This release fixes an obscure security issue. Applications using older versions of SQLite may be vulnerable if

We are not aware of any applications that are vulnerable to this problem. On the other hand, we do not know about every application that uses SQLite. If your application allows unauthenticated users on the internet (and hence potential attackers) to run arbitrary SQL and if you build SQLite with any of the optional extensions enabled, then you should take this upgrade at your earliest opportunity.

- SQLite is compiled with certain optional extensions enabled, and
- the attacker is able to inject arbitrary SQL.
For further information about improving SQLite's robustness in internet-facing applications, see the our security recommendations.


Version 3.27.2 is a patch release that
fixes two bugs and various documentation
errors found in the version 3.27.1.  The
changes from version 3.27.1 and 3.27.0 are
minimal.

After release 3.27.0 was tagged, but before the build could
be completed and uploaded, a 
long-standing bug
in the query optimizer was reported via System.Data.SQLite.
Since people will be upgrading anyhow, we decided to
publish the fix for this newly discovered problem right
away.  Hence, 3.27.1 was released less than 24 hours after
3.27.0.
It would have been better if the query optimizer bug had come to our attention one day earlier, so that we could have incorporated a fix into 3.27.0, but sometimes that's the way things go.


SQLite version 3.27.0 is a routine maintenance release with various
performance and feature enhancements.  See the
release notes for details.

SQLite version 3.26.0 is a routine maintenance release with various
performance and feature enhancements.  See the
release notes for details.

SQLite version 3.25.3 is a third patch against 3.25.0 that fixes various
problems that have come to light and which seem serious enough to 
justify a patch.

SQLite version 3.25.2 is another patch against 3.25.0 that fixes still
more problems associated with the new window function feature and the
ALTER TABLE enhancements.  Of particular note is the new
PRAGMA legacy_alter_table=ON command, which causes the ALTER TABLE RENAME
command to behave in the same goofy way that it did before the enhancements
found in version 3.25.0 → references to renamed tables that are inside
the bodies of triggers and views are not updated.  The legacy behavior is
arguably a bug, but some programs depend on the older buggy behavior.  The
3.25.2 release also contains a fix to window function processing for
VIEWs.  There also a slew of other minor fixes that affect obscure
compile-time options.  See the
Fossil Timeline for
details.

SQLite version 3.25.1 is a patch against version 3.25.0 that contains
two one-line fixes for bug that were introduced in version 3.25.0.
See the change log for details.  Upgrading from 3.25.0 is recommended.

SQLite version 3.25.0 is a regularly scheduled maintenance release.
Two big enhancements in this release:

Support for window functions was added, using the PostgreSQL documentation as the baseline.

The ALTER TABLE command was enhanced to support renaming of columns, and so that column and table renames are propagated into trigger bodies and views.

In addition, there are various performance enhancements and minor fixes.

One bug of note is ticket 9936b2fa443fec which describes a hard-to-reach condition where the ORDER BY LIMIT optimization might cause an infinite loop during query evaluation. This ticket raised a lot of concern on HackerNews and Reddit, probably due to my choice of the ticket title. "Infinite Loop" sounds scary. But I argue that the bug isn't really all that bad in that it is very difficult to reach, will show up during testing (rather than magically appearing after a product is deployed), does not cause any data loss, and does not return an incorrect result. It was an important error, but not nearly as dire as many people interpreted it to be. And, in any event, it is fixed now.


SQLite version 3.24.0 is a regularly scheduled maintenance release.
Highlights of this release include support for PostgreSQL-style
UPSERT and improved performance, especially for ORDER BY LIMIT queries.

The version 3.23.1 release fixes a bug in the new
LEFT JOIN strength reduction optimization added to version 3.23.0.
A few other minor and obscure fixes were also inserted, as well as
a small performance optimization.  Code changes relative to
version 3.23.0 are minimal.

The version 3.23.0 release is a regularly scheduled maintenance release.
See the change log for a list of enhancements and bug
fixes.

The version 3.22.0 release is a regularly scheduled maintenance release.
There are many minor, though interesting, enhancements in this release.
See the change log for details.

The version 3.21.0 release is a regularly scheduled maintenance release.
There are lots of enhancements in this release.
See the change log for details.

The version 3.20.1 patch release changes two lines of code in
the sqlite3_result_pointer() interface in order to fix a rare
memory leak.  There are no other changes relative to version 3.20.0.

SQLite version 3.20.0 is a regularly scheduled maintenance release
of SQLite.
This release contains many minor enhancements, including:


- Several new extensions
- Enhancements to the "sqlite3.exe" command-line shell
- Query planner enhancements
- Miscellaneous code optimizations for improved performance
- Fixes for some obscure bugs
See the release notes for more information.


SQLite version 3.18.2 is another backport of a bug fix found
in SQLite version 3.19.0, specifically the fix for
ticket 61fe9745.  Changes
against version 3.18.0 are minimal.

SQLite version 3.18.1 is a bug-fix release against version 3.18.0
that fixes the auto_vacuum corruption bug described in ticket
fda22108.  This release was
created for users who need that bug fix but do not yet want to upgrade 
to version 3.19.3.  

Version 3.19.3 is an emergency patch release to fix a 
bug in 
auto_vacuum logic that can lead to database corruption.
The bug was introduced in version 3.16.0 
(2017-01-02).  Though the bug is obscure and rarely
encountered, upgrading is recommended for all users, and
especially for users who turn on auto_vacuum.

Still more problems have been found in the LEFT JOIN
flattening optimization
that was added in the 3.19.0 release.  This patch release fixes all known
issues with that optimization and adds new test cases.  Hopefully this
will be the last patch.

One of the new query planner optimizations in the 3.19.0 release contained
bugs.  The 3.19.1 patch release fixes them.
Beginning with 3.19.0, subqueries and views on the right-hand side of a LEFT JOIN operator could sometimes be flattened into the main query. The new optimization worked well for all of the test cases that the developers devised, and for millions of legacy test cases, but once 3.19.0 was released, users found some other cases where the optimization failed. Ticket cad1ab4cb7b0fc344 contains examples.

These problems exist only in 3.19.0. Users of SQLite 3.19.0 should upgrade, but users of all prior versions of SQLite are safe.


SQLite version 3.19.0 is a regularly scheduled maintenance release.
The emphasis on this release is improvements to the query planner. There are also some obscure bug fixes. There is no reason to upgrade unless you are having problems with a prior release.


SQLite version 3.18.0 is a regularly scheduled maintenance release.
This release features an initial implementation the "PRAGMA optimize" command. This command can now be used to cause ANALYZE to be run on an as-needed basis. Applications should invoke "PRAGMA optimize" just before closing the database connection. The "PRAGMA optimize" statement will likely be enhanced to do other kinds of automated database maintenance in future releases.

The Fossil version control system that is used to manage the SQLite project has been upgraded to use SHA3-256 hashes instead of SHA1. Therefore, the version identifications for SQLite now show a 64-hex-digit SHA3-256 hash rather than the 40-hex-digit SHA1 hash.

See the change log for other enhancements and optimizations in this release.


SQLite version 3.17.0 is a regularly scheduled maintenance release.
Most of the changes in this release are performance optimizations. Optimizations to the R-Tree extension are especially noticeable.

In this release, the default size of the lookaside buffer allocated for each database connection is increased from 64,000 to 120,000 bytes. This provides improved performance on many common workloads in exchange for a small increase in memory usage. Applications that value a small memory footprint over raw speed can change the lookaside buffer size back to its old value (or to zero) using the SQLITE_DEFAULT_LOOKASIDE compile-time option, or the sqlite3_config(SQLITE_CONFIG_LOOKASIDE) start-time setting, or the sqlite3_db_config(SQLITE_DBCONFIG_LOOKASIDE) run-time setting.


One of the performance optimizations added in 3.16.0 caused triggers
and foreign keys to malfunction for the REPLACE statement on
WITHOUT ROWID tables that lack secondary indexes.  This patch
release fixes the problem.  See ticket 
30027b613b4 for details.

SQLite version 3.16.1 fixes a bug in the row-value logic for UPDATE
statements inside of triggers.  The bug has been there since row-values
were added by release 3.15.0, but was not discovered until just a few
minutes after the 3.16.0 release was published, and so it was not fixed
by 3.16.0.  This patch release is version 3.16.0 with the row-value bug fix.

SQLite version 3.16.0 is a regularly scheduled maintenance release.
This release includes many microoptimizations that collectively reduce the CPU cycle count by about 9%, add there have been important enhancements to the command-line shell.

Support for PRAGMA functions is added, so that many pragma statements can be used as part of a larger SQL query. This is considered an experimental feature. We do not anticipate any changes to the PRAGMA function interface, but will keep continuing to call this interface "experimental" for a few release cycles in case unforeseen issues arise.

See the change log for other enhancements.


SQLite version 3.15.2 is a bug-fix patch release that fixes several minor
issues in the 3.15.0 and 3.15.1 releases.

SQLite version 3.15.1 is a bug-fix patch release that fixes some minor
issues in the 3.15.0 release.

SQLite version 3.15.0 is a regularly scheduled maintenance release.
The key feature in this release is the added support for
row values.  There are also other enhancements and
fixes for a number of obscure bugs.
The 3.15.0 release uses about 7% fewer CPU cycles than 3.14.2. Most of the improvement in this release is in the SQL parser, query planner, and byte-code generator (the front-end) corresponding to the sqlite3_prepare_v2() interface. Overall, version 3.15.0 uses about half as much CPU time as version 3.8.1 (2013-10-17). These performance measurements are made using the "speedtest1.c" workload on x64 compiled with gcc and -Os. Performance improvements may vary with different platforms and workloads.


SQLite version 3.14.2 fixes several obscure bugs and adds 
improved support for building SQLite using the STDCALL calling
convention on 32-bit windows systems.  Upgrading from versions
3.14 and 3.14.1 is optional.

SQLite version 3.14.1 adds a small patch to improve the performance
of the pcache1TruncateUnsafe() routine for cases when the only a few
pages on the end of the cache are being removed.  This causes COMMITs
to run faster when there is a very large page cache.  Upgrading from
version 3.14 is optional.

SQLite version 3.14 (the "π" release)
is a regularly scheduled maintenance
release containing performance enhancements, new features, and fixes for
obscure bugs.

SQLite version 3.13.0 is a regularly scheduled maintenance release containing
performance enhancements and fixes for obscure bugs.

Yikes!  The 3.12.0 and 3.12.1 releases contain a backwards compatibility bug!
   Tables that declare a column with type `"INTEGER" PRIMARY KEY`
   (where the datatype name INTEGER is quoted) generate an incompatible
   database file.  The mistake came about because the developers have never
   thought to put a typename in quotes before, and so there was no documentation 
   of that capability nor any tests.  (There are tests now, though, of course.)
   Instances of quoting the datatype name are probably infrequent in the wild,
   so we do not expect the impact of this bug to be too severe.
   Upgrading is still strongly recommended.

Fixes for three other minor issues were included in this patch release. The other issues would have normally been deferred until the next scheduled release, but since a patch release is being issued anyhow, they might as well be included.


SQLite version 3.12.1 is an emergency patch release to address a crash bug that snuck into version 3.12.0. Upgrading from version 3.12.0 is highly recommended.

Another minor problem involving datatypes on view columns, and a query planner deficiency are fixed at the same time. These two issues did not justify a new release on their own, but since a release is being issued to deal with the crash bug, we included these other fixes for good measure.


SQLite version 3.12.0 is a regularly scheduled maintenance release. A notable change in this release is an increase in the default page size for newly created database files. There are also various performance improvements. See the change log for details.


SQLite version 3.11.1 is a patch release that fixes problems in the new FTS5 extension and increases a default setting in the spellfix1 extension, and implements enhancements to some of the Windows makefiles. The SQLite core is unchanged from 3.11.0. Upgrading is optional.


SQLite version 3.11.0 is a regularly scheduled maintenance release.


Yikes! An optimization attempt gone bad resulted in a bug in the LIKE operator which is fixed by this patch release. Three other minor but low-risk fixes are also included in the patch.


SQLite version 3.10.1 is a bug-fix release primarily targeting the fix for the query planner bug cb3aa0641d9a4 discovered by Mapscape. Also included is a minor API enhancement requested by the Firefox developers at Mozilla. The differences from version 3.10.0 are minimal.


SQLite version 3.10.0 is a regularly scheduled maintenance release.


*This page was last updated on 2026-10-25 00:51:55Z*
