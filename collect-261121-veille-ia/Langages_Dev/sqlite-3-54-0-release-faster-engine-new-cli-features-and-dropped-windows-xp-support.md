---
id: collect-261121-veille-ia/langages-dev/sqlite-3-54-0-release-faster-engine-new-cli-features-and-dropped-windows-xp-support
title: "SQLite 3.54.0 Release: Faster Engine, New CLI Features, and Dropped Windows XP Support"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["sqlite", "faster", "windows", "database", "release", "engine", "support", "file"]
source: https://www.linuxcompatible.org/story/sqlite-3540-release-faster-engine-new-cli-features-and-dropped-windows-xp-support/
source_anchor: ""
source_lines: [1, 45]
section: "Langages_Dev"
---
### SQLite 3.54.0 Ships Faster and Leaves Windows XP Behind

**SQLite is the database engine you have never heard about, and that is precisely the point. On October 9th, the SQLite Project released version 3.54.0, the latest stable build of what its developers call "the most widely deployed database in the world." It also drops Windows XP support, a policy change with real teeth for legacy users.**

Here is the thing about SQLite: you almost certainly use it every single day. It lives inside web browsers, phones, and desktop apps instead of running as a separate server. The entire database, with its tables, indexes, and triggers, fits inside one ordinary file. Because it sits in the public domain, anyone can embed it without paying a licensing fee. The project has promised to keep supporting the file format through 2050. The Library of Congress even recommends the format as a storage standard.

#### More speed, if you are counting

The headline claim is that 3.54.0 runs about 4% faster than its predecessor. That figure comes from the speedtest1 benchmark, counted in CPU cycles with valgrind on Linux x64. Against a year ago — version 3.51.0 — the gain climbs to roughly 7.5%.

Keep in mind that 4% in a single benchmark will not jump out in daily use. But SQLite has long punched above its weight, sometimes reading and writing faster than a raw filesystem call. Multiply small gains by billions of deployments and the math starts to matter. The release notes carry their usual caveat, noting that results depend on workload, build options, CPU, compiler, and operating system. The team still appears hungry.

#### The shell gets sharper

Most of the visible changes land in the command-line shell, the tool developers reach for when they want to poke directly at a database file. The prompt is now customizable through the SQLITE_PS1 and SQLITE_PS2 environment variables, and it honors NO_COLOR if you prefer a drab terminal.

For people scripting around the CLI, columnar output modes pick up a few quality-of-life tweaks. Headers now show even when a query returns nothing. The .mode command gains --titles always as its new default, plus --rowcount, --ifmt, and --fpfmt for formatting control. On Unix, empty BLOB output no longer gets mangled.

There is one small change that will mostly go unnoticed. The old CLI accepted "go" and "/" as command terminators, a concession to SQL Server and Oracle users. That convenience is retired now, unless you recompile with a specific legacy flag. It was a bridge the developers built long ago. Gone.

One genuinely useful addition rides along. diskused() is a new SQL function that analyzes how your database file actually fills up across tables and indexes, replacing the legacy sqlite3_analyzer utility, which the project now deprecates. Run sqlite3 demo.db .diskused and you get a human-readable breakdown, optionally fed into a space_used table for deeper digging.

It is an extension, not part of the core library. The lightweight amalgamation stays lean, and you can compile it in yourself if you want it.

#### Behind the query planner

The query planner decides how a SQL statement actually runs, and this release teaches it a few tricks. Expression indexes no longer churn when nothing behind them changes. Queries with expr OR TRUE or expr OR FALSE get cleaner plans. A DISTINCT hiding on the right side of an IN gets ignored, since deduplication there is wasted effort. Union queries capped at LIMIT 1 also run leaner.

There are new SQL conveniences too. The %J and %j format codes now render JSON string literals, two date modifiers let you compute weekday-relative dates and snap to the end of a time unit, and ALTER TABLE will refuse to add columns that shadow SQLite's internal ROWID names. It also now uses O_TMPFILE for temporary files where the OS allows it, cutting down on name collisions.

For those embedding via the C API, three interfaces show up as well. sqlite3_result_str() and sqlite3_incomplete() simplify internal plumbing, while SQLITE_LIMIT_SCHEMA and SQLITE_LIMIT_TRIGGER_STEPS give apps finer control over limits. It is the kind of change that quietly keeps the tool extensible.

#### Windows XP no longer welcome

The change with the most teeth is the one that only bites Windows users. SQLite has dropped support for Windows XP and earlier, including Windows CE. To run it today, you need Vista or newer.

The code underneath has been modernized to match. It now uses Windows Slim Reader/Writer Locks for synchronization, which handle busy but reader-light workloads better than the older CriticalSection approach. A new -DSQLITE_UWP=1 flag also lets you build for UWP and WebUI apps.

This matters only on Windows. Linux, macOS, BSD, and the rest of the Unix family are untouched. Given how much of SQLite lives outside Microsoft's ecosystem, that distinction is worth hammering home.

#### Still the same rock-solid database

For all the churn, 3.54.0 keeps SQLite's core promise. The API, SQL syntax, and file format stay backward compatible, so no migration is required. The test suite still churns through hundreds of millions of individual SQL statements and claims full branch coverage, with transactions staying ACID even after crashes or power failures.

It is available now on the SQLite download page. If you embed SQLite, it is a low-risk upgrade worth the swap.
