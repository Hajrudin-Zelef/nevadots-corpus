---
id: collect-261121-veille-ia/langages-dev/sqlite-3-54-s-diskused-command-needs-a-build-flag-sqlite3-analyzer-never-required
title: "SQLite 3.54's diskused command needs a build flag sqlite3_analyzer never required"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["diskused", "sqlite3", "command", "analyzer", "sqlite", "build", "shell", "table"]
source: https://dev.to/alexgeorgiev17/sqlite-354s-diskused-command-needs-a-build-flag-sqlite3analyzer-never-required-4na8
source_anchor: ""
source_lines: [1, 107]
section: "Langages_Dev"
---
SQLite 3.54.0 shipped on 9 October, and the change list has one line that's easy to skim past: the `sqlite3_analyzer` utility is now deprecated, replaced by a `diskused()` SQL function built straight into the command-line shell as a new `.diskused` dot-command. I wanted to know what that actually buys you, and what it costs, so I built both versions from source and ran them against the same database.

`sqlite3_analyzer` has been the way to answer "which table is eating my disk space" for as long as I can remember using SQLite seriously. It's a separate program you download or build on top of the library, and it opens your database file cold, walks every b-tree page, and prints a report. `.diskused` does the identical analysis, but it runs inside the shell you already have open, using a virtual table (`dbstat`) the engine already exposes internally.

I built a 20MB test database — four tables, one index, 350,000 rows — and ran both tools against copies of the same file, five times each:


```
=== .diskused (new, 3.54) ===
0.028s  0.023s  0.022s  0.024s  0.029s
=== sqlite3_analyzer (old, deprecated) ===
0.056s  0.061s  0.058s  0.055s  0.076s
```
|  | fastest | mean of 5 | 
|---|---|---|
| `.diskused` | 0.022s | 0.025s | 
| `sqlite3_analyzer` | 0.055s | 0.061s | 

That's roughly 2.4 times faster on the mean, and the gap holds at the fastest runs too. Part of that is architectural rather than algorithmic: `sqlite3_analyzer` is a 3.3MB standalone binary that has to start its own process and open its own connection to the file from scratch, where `.diskused` runs a SQL function against a connection the shell already has open.

The two reports agree on every number I checked — page counts per table, bytes of payload, unused bytes per page, b-tree depth — which is exactly what you'd want from a replacement. But `.diskused`'s output is 242 lines and 13KB; `sqlite3_analyzer`'s is 432 lines and 21KB for the same database. The new tool drops some of the older report's redundant subsections (separate "all indexes" and "all tables" rollups that `.diskused` folds into fewer blocks) without losing any of the figures I actually used.

`diskused()` depends on `dbstat`, a virtual table that SQLite only exposes when it's compiled with `SQLITE_ENABLE_DBSTAT_VTAB`. The official binaries on sqlite.org have that flag set, so if you download the prebuilt shell, none of this applies to you. But a lot of people don't use the prebuilt shell — they drop `sqlite3.c` into their own project and compile it with their own flags, which is the whole point of the amalgamation. I did exactly that, with no extra flags beyond `-O2`, and ran the new command against my test database:


```
$ ./sqlite3_nodbstat test.db ".diskused"
$ echo $?
0
```
No output. No error. Just a clean exit. My first assumption was that I'd broken something in my own minimal build — I went back and double-checked the compile line, assuming I'd missed a header. I hadn't. The command had run, found nothing it could use, and said nothing about it.

Calling the underlying SQL function directly, instead of the dot-command wrapper, finally showed the real cause:


```
$ ./sqlite3_nodbstat test.db "SELECT diskused('main');"
Error in 2nd command line argument: SQL parse error: no such table: dbstat
```
`dbstat` isn't there, because I didn't ask for it at compile time. For comparison, running `.diskused` against 3.53.2, the version before this command existed, gives you an actual error:


```
$ ./sqlite3-3.53.2 test.db ".diskused"
Error: unknown command or invalid arguments:  "diskused". Enter ".help" for help
```
So an unsupported command on an old version tells you plainly that it doesn't exist. A supported command on a new version, missing one dependency, tells you nothing at all and exits zero. If you build your own shell from the amalgamation and rely on `.diskused` in a script to catch a bloated table before it causes problems, that script will quietly do nothing on a build missing one flag, and report success while doing it.

The release notes claim 3.54 is "about 4% faster" than 3.53 and "about 7.5% faster" than 3.51, measured as speedtest1 CPU-cycle counts under valgrind. I tried to check that first with ordinary wall-clock timing: a small C program against each version's `sqlite3.c`, inserting 300,000 rows, building an index, then running 100,000 indexed point lookups, three runs per version.


```
3.53.2: total 0.554s, 0.586s, 0.602s
3.54.0: total 0.563s, 0.600s, 0.635s
```
That's not 4% faster. If anything it points the other way. Before writing that down as a real finding, I distrusted the harness rather than the claim: a 4% difference on a workload with real disk I/O, page cache behaviour, and OS scheduling noise is well within the run-to-run jitter I was already seeing between repeats of the *same* binary. Wall-clock timing on a live filesystem was never going to resolve a few percent.

So I ran the identical workload under `valgrind --tool=callgrind`, which counts CPU instructions retired rather than elapsed time — the same kind of measurement the release notes describe:


```
3.53.2: 2,557,436,360 instructions (confirmed on a second run: 2,557,436,359)
3.54.0: 2,504,239,038 instructions (confirmed on a second run: 2,504,239,042)
```
That's a reduction of about 2.1%, consistent across repeats. The direction matches the documented claim; the size doesn't. My workload is dominated by writes and an index build, not the query-heavy mix speedtest1 actually runs, and the release notes say as much themselves ("results may vary depending on workload"). I can confirm 3.54 does less CPU work on a real workload. I can't confirm it does 4% less, because that number was never about my workload to begin with.

I ran `.diskused` against a database with an open write transaction in another connection, once in WAL mode and once in the default rollback-journal mode, expecting the reader to either block or get `SQLITE_BUSY` in the second case. Neither happened. `.diskused` only needs a `SHARED` lock, and `BEGIN IMMEDIATE` on the writer only takes a `RESERVED` lock, which is compatible with any number of concurrent readers under SQLite's normal locking rules. This isn't a new behaviour `diskused()` introduced; it's SQLite doing what SQLite has always done. Worth checking, nothing to report.

I spent longer than I'd like to admit convinced that the silent, empty output from my no-`dbstat` build was a bug in how I'd compiled the shell, not a property of the tool. The fact that it exited `0` was the tell I ignored for too long — a crash doesn't give you a clean exit code. Once I stopped assuming my build was broken and started asking the SQL function directly what it thought was wrong, the actual cause (`no such table: dbstat`) came back in one line.

This needs a C compiler and the usual build tools. It downloads and builds two SQLite versions from source, so expect it to take a few minutes.


```
mkdir sqlite-diskused-test && cd sqlite-diskused-test
curl -sS -o 354.tar.gz https://sqlite.org/2026/sqlite-autoconf-3540000.tar.gz
tar xzf 354.tar.gz && cd sqlite-autoconf-3540000
./configure CFLAGS="-O2 -DSQLITE_ENABLE_DBSTAT_VTAB=1"
make -j4
cd ..
# a build with no extra flags at all, to see the silent failure
cc -O2 -o sqlite3_nodbstat sqlite-autoconf-3540000/shell.c sqlite-autoconf-3540000/sqlite3.c -lpthread -ldl -lm
./sqlite-autoconf-3540000/sqlite3 test.db <<'EOF'
CREATE TABLE t(id INTEGER PRIMARY KEY, a INTEGER, b TEXT);
WITH RECURSIVE seq(x) AS (SELECT 1 UNION ALL SELECT x+1 FROM seq WHERE x<300000)
INSERT INTO t(a,b) SELECT x%997, 'padding' FROM seq;
EOF
./sqlite-autoconf-3540000/sqlite3 test.db ".diskused"      # full report
./sqlite3_nodbstat test.db ".diskused"; echo "exit: $?"    # silent, exit 0
./sqlite3_nodbstat test.db "SELECT diskused('main');"      # the real error
```
If you ship your own SQLite build compiled from the amalgamation, check now whether `SQLITE_ENABLE_DBSTAT_VTAB` is in your flags, rather than finding out the day a disk-space script reports "nothing to analyse" on a database that's actually full of bloat. If you're still shelling out to a separate `sqlite3_analyzer` binary in a build pipeline or cron job, `.diskused` on 3.54 does the same job from inside a shell you already have running, in under half the time, and you can drop the extra binary from whatever you're currently downloading to get it.

## Top comments (2)

The concurrency test only covers a writer that has an open transaction, and there

`EXCLUSIVE`, which waits for every`BEGIN IMMEDIATE; INSERT; COMMIT`with a 0.2 s busy timeout fails with "database is locked" at the COMMIT, not at the BEGIN.
On your 20 MB file the scan takes about 25 ms so you'd never notice, but on a multi-GB database

Nope, only the open-transaction case, in both modes. Commit-during-scan never got tested, and you're right that it's the one that actually matters. RESERVED next to SHARED was always gonna be fine, I basically tested the case that couldn't fail.

So I just ran it, same shape as yours but with the reader holding an open cursor on dbstat itself, since that's what .diskused walks. 0.2s busy timeout on the writer:

Heads up, that was on the Python sqlite3 module, which is 3.53.4 here, not 3.54. The locking rules didn't change between them and it's the same dbstat table .diskused reads, so I'd expect 3.54 to behave the same, but I haven't run the actual .diskused command against it.

So yeah, on a big database in rollback mode, running .diskused next to a live writer can hold up that writer's commits for as long as the report runs, or fail them outright if the busy timeout is shorter than that. "Nothing changed" in that section is too strong, I'll add the commit case and the WAL vs rollback split. Thanks for actually reproducing it instead of just guessing.
