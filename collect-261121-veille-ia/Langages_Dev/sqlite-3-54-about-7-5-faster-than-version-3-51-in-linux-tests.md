---
id: collect-261121-veille-ia/langages-dev/sqlite-3-54-about-7-5-faster-than-version-3-51-in-linux-tests
title: "SQLite 3.54: about 7,5% faster than version 3.51 in Linux tests"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["sqlite", "about", "version", "linux", "faster", "tests", "also", "windows"]
source: https://dev.to/hacksgr/sqlite-354-about-75-faster-than-version-351-in-linux-tests-2067
source_anchor: ""
source_lines: [1, 19]
section: "Langages_Dev"
---
SQLite 3.54 reports about 4% better CPU performance than the previous release in the project’s standardized Linux tests; compared with SQLite 3.51, the reported gain is about 7.5%.

These are benchmark results, not a guarantee for every application.

The release also improves some multi-source queries, avoids unnecessary work when indexed data is unchanged, and adds the.diskused command for inspecting database storage.

According to the project's measurements, in standard Linux tests, version 3.54 was about 4% faster in terms of processor usage. Compared to version 3.51, released a year earlier, the improvement is about 7,5%. This does not mean that every application that uses SQLite will become correspondingly faster; the result depends on how it uses the database.

Some searches that combine results from multiple sources can now complete faster and with less memory. SQLite also avoids unnecessary work when updating information used for quick searches if it hasn't changed.

Changes have also been made to the SQLite tool used by the terminal. Column results now display headers even when there are no matching records. The command has been added `.diskused`, which shows how much storage the database is using and replaces a separate tool that is now considered obsolete. The terminal tool also offers more control over how numbers are displayed, can optionally show how many rows a result contains, and allows colors to be disabled.

The new version adds options for generating well-formed JSON data, as well as new features for date and time calculations, such as calculating the day of the week and calendar period boundaries. On Linux and other systems that support it, it can also create temporary files without them being displayed with a name on the system.

SQLite 3.54 officially drops support for Windows XP, Windows CE, and earlier versions of Windows. The minimum supported version is now Windows Vista.

## Top comments (0)

Some comments may only be visible to logged-in visitors. Sign in to view all comments.
