---
id: collect-261121-veille-ia/langages-dev/sqlite-3-54-is-available-with-optimizations-cli-enhancements-and-discontinuation-of-support-for-wind
title: "🥇SQLite 3.54 is available with optimizations, CLI enhancements, and discontinuation of support for Windows XP | ProHoster"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["sqlite", "been", "windows", "support", "optimizations", "command", "version", "mode"]
source: https://prohoster.info/en/blog/news/sqlite-3-54-released-with-optimizations-cli-enhancements-and-end-of-windows-xp-support
source_anchor: ""
source_lines: [1, 16]
section: "Langages_Dev"
---
SQLite 3.54 has been released, a lightweight database management system presented as a dynamic library. SQLite code is released into the public domain, meaning it can be used freely and without restrictions for any purpose. Financial support for SQLite developers is provided by a specially created consortium.

Key Changes:

- Optimizations have been implemented that allow speedtest1 to run approximately 4% faster compared to the previous version and 7.5% faster than the 3.51 release from a year ago. Results may vary depending on the type of load, build options, CPU, compiler, and operating system.
- The sqlite utility now supports escape sequences for displaying additional status information in the command-line prompt, such as "\/x" for details on open transactions, "\/r" for databases in read-only mode, "\/m" for memory-only databases, "\/f" for inserting the database file name, "\/v" for displaying the SQLite version, "\/:" and "\/;" for constructing conditional expressions, and "\/u" and "\/h" for inserting the username and hostname. The command prompt template can be configured through the SQLITE_PS1 and SQLITE_PS2 environment variables.
- In the sqlite utility, the ".mode" command has been updated with the options "—ifmt", "—rowcount on", and "—titles always" to adjust the format of floating-point numbers and control the display of row counts and headers. The ".diskused" command handler has been replaced. By default, the header with column names remains if the query returns no rows, and cleaning of zero bytes when outputting blobs in Unix systems has been disabled.
- In the query planner, UPDATE operations have been expedited in situations where indexed expressions exist, but the indexed value does not change. The execution of "expr OR TRUE" and "expr OR FALSE" has been optimized. The DISTINCT clause in SELECT subqueries on the right side of the IN operator is now ignored. Processing has been accelerated, and memory consumption reduced when specifying "LIMIT 1" in UNION queries.
- In the ALTER TABLE command, an error is thrown for any attempts to add, delete, or rename columns with reserved names "ROWID", "_ROWID_", and "OID".
- A new SQL function diskused() has been added to assess the space that tables and indexes occupy on the storage.
- The SQL functions printf() and format() have added substitutions "%J" and "%j" for JSON format output.
- Modifiers "weekday -N" and "end of" have been added to date and time functions.
- On systems that support the O_TMPFILE flag in the open() function, this mode is used when creating temporary files.
- Support for older versions of Windows, including Windows CE and Windows XP, has been discontinued. Windows Vista is now the minimum version supported.

Source: opennet.ru
