---
id: collect-261121-veille-ia/langages-dev/sqlite-release-3-54-0-on-2026-10-09-2
title: "SQLite Release 3.54.0 On 2026-10-09"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-10-09"]
keywords: ["sqlite", "default", "windows", "prompt", "command", "using", "table", "titles"]
source: https://sqlite.org/releaselog/3_54_0.html
source_anchor: ""
source_lines: [1, 84]
section: "Langages_Dev"
---
CLI prompt strings may contain escape sequences that
expand to show state information. The initial CLI
prompt is now be initialized the values of the
SQLITE_PS1 and SQLITE_PS2 environment variables, if
those variables exist.

The default CLI prompt has been enhanced to take
advantage of the new escape sequences described in
the previous.

For columnar output modes, the CLI now shows the column name header
by default even if the query returns no rows.

Use the new diskused() extension to implement the
".diskused" dot-command. The replaces the separate sqlite3_analyzer
utility program, which is now deprecated.

On unix, the CLI preserves zero bytes when outputing
unformatted BLOB values.

Enhance the .mode command with new options
"--rowcount on"
and "--titles always".
The "--titles always" is now the default behavior
(see item 3a above). Legacy behavior can be restored
using "-titles on".

The -csv command-line option automatically disables
display limits, for legacy compatibility.

The default CLI prompt honors the NO_COLOR environment variable.

The ability to end an SQL command using the line that contains just
"go" or "/" (for compatibility with SQL Server and Oracle, respectively)
is discontinued, unless the CLI is compiled with
option -DSQLITE_SHELL_LEGACY_COMMAND_TERMINATOR.

Query planner improvements:

When doing an UPDATE on a table that has expression indexes,
do not change the expression index if the value being indexed
is unchanged.

Improved optimization of "expr OR TRUE" and
"expr OR FALSE".

Ignore DISTINCT in the SELECT statement on the right-hand
side of an IN operator.

UNION queries with a LIMIT of exactly 1 are now handled faster
and with less memory.

The ALTER TABLE command raises an error on any attempt to add, drop, or rename columns
with the special names "ROWID", "_ROWID_", or "OID".

Use O_TMPFILE when generating temporary files, on systems that
support that feature.

Drop support for WindowsXP and earlier Windows versions,
including WindowsCE. SQLite for Windows now requires
Windows Vista (circa 2007) or later.

Improve performance by using
Slim Reader/Writer Locks
(SRWLs) to implement non-recursive mutexes, instead of
CriticalSection,
since all supported Windows platforms now provide SRWLs.

Use the -DSQLITE_UWP=1 compile-time option for UWP and WebUI applications.

To be clear: This changes above only affects SQLite on Windows.
Builds for Linux, Mac, BSD, and other unix variants are unaffected.

The authorizer callback is now invoked for SQL functions found in
the DEFAULT clauses of CREATE TABLE statements. This happens both when
the CREATE TABLE statement is first issued and when the DEFAULT value is
actually used, but not when reading the schema while opening a database.

About 4% faster, as measured by counting CPU cycles in the
speedtest1
preformance benchmark using valgrind on Linux x64. About 7.5%
faster compared to one year ago (version 3.51.0). Your mileage
may vary depending on workload, build options, CPU, compiler,
and operating system.
