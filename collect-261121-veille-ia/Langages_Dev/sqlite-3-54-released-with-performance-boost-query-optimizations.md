---
id: collect-261121-veille-ia/langages-dev/sqlite-3-54-released-with-performance-boost-query-optimizations
title: "SQLite 3.54 Released with Performance Boost, Query Optimizations"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["sqlite", "performance", "windows", "database", "released", "more", "query", "used"]
source: https://linuxiac.com/sqlite-3-54-released-with-performance-boost-query-optimizations/
source_anchor: ""
source_lines: [1, 21]
section: "Langages_Dev"
---
SQLite, the widely used open-source lightweight database engine found in desktop applications, web browsers, mobile devices, and embedded systems, has released version 3.54.

One of the biggest highlights is improved performance. According to the project’s benchmarks, SQLite 3.54 is approximately 4% faster than the previous release, measured by CPU usage during standardized tests on Linux. Compared with version 3.51 from a year ago, the improvement reaches around 7.5%.

Of course, these numbers don’t mean every application using SQLite will suddenly run 4% faster. The actual difference depends on how the database is used, but the results show that developers continue to squeeze more performance out of an already highly optimized engine.

Moreover, SQLite has become better at deciding how to retrieve and update information efficiently. For example, certain queries combining results from multiple sources can now execute faster while using less memory. Plus, the database engine also avoids unnecessary work when updating indexed information that hasn’t changed.

SQLite 3.54 also brings several improvements to its CLI. The command prompt is now more informative and customizable, and Query results displayed in columns now show headers even when no matching records are found, avoiding confusion when checking empty tables or running searches.

On top of that, there is a new `.diskused` command for examining how database storage space is used. It replaces the separate `sqlite3_analyzer` utility, which is now deprecated.

Other terminal improvements include more control over number display, optional row counts, and support for the standard `NO_COLOR` environment variable, which disables colored output when requested.

SQLite 3.54 also expands its built-in SQL functionality. New JSON formatting options make it easier to produce properly formatted JSON strings, while additional date and time capabilities simplify calculations involving weekdays and the ends of calendar periods.

On Linux and other supported systems, SQLite can now use the `O_TMPFILE` feature when creating temporary files, which allows temporary files to be created without visible filenames.

Finally, SQLite 3.54 officially drops support for Windows XP, Windows CE, and earlier Windows versions, making Windows Vista the minimum supported Windows release.

For more information, refer to the official SQLite 3.54 release notes.
