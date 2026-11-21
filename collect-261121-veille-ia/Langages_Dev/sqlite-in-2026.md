---
id: collect-261121-veille-ia/langages-dev/sqlite-in-2026
title: "SQLite in 2026"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-01-09"]
keywords: ["release", "patch", "sqlite", "bugs", "mostly", "feature", "three", "database"]
source: https://www.bubelov.com/blog/2026/sqlite/
source_anchor: ""
source_lines: [1, 125]
section: "Langages_Dev"
---
## Table of Contents

## Preface

I upgraded to 3.54.1 three weeks ago, and the first thing I noticed was the new prompt:

```
SQLite-3.54 memory->
```
Showing the version and the data source by default, in different colors, is one of those things you never knew you wanted, but once you see this setup, you never want to go back.

With three shells open, the filename and the color tell you which database you are in and whether a transaction is still open, without typing a command. Cheap observability, always on, nothing to configure. I like this approach.

## Why I Catch Up With All Release Notes

Everything I run for BTC Map and other projects keeps its data in SQLite. For example, BTC Map API has three databases: `main.db` for places, areas, users, events, and invoices, `log.db` for every HTTP request (30 days retention, auto pruned), and `image.db` for OpenGraph previews and merchant images.

When SQLite is a foundational part of your tech stack, keeping a finger on its pulse is mandatory. My Rust code is mostly migrations, queries, and glue around the database, and it needs to adapt to any changes in the underlying layer.

SQLite grows slowly and in one direction: your application should do less. Expression indexes replaced filtering JSON in Rust. `STRICT` tables replaced a pile of validation. Generated columns replaced trigger-maintained copies. Every release I don’t keep up with adds unnecessary complexity to my code.

## Releases

I went through the official release history and the news log. This is everything the project published in 2026.

| Date | Version | What it was | 
|---|---|---|
| 2026-01-09 | 3.51.2 | Patch. A possible deadlock in the new broken-lock detection, plus mistakes in the EXISTS-to-JOIN optimization | 
| 2026-03-06 | 3.52.0 | Feature release. Withdrawn seven days later | 
| 2026-03-13 | 3.51.3 | Patch. Database corruption fix | 
| 2026-04-09 | 3.53.0 | Feature release. Shell overhaul, ALTER TABLE constraints, 17-digit floats | 
| 2026-05-05 | 3.53.1 | Patch. User-reported problems in 3.53.0 | 
| 2026-06-03 | 3.53.2 | Patch. More of the same | 
| 2026-06-26 | 3.53.3 | Patch, mostly bugs reported by AI | 
| 2026-07-24 | 3.53.4 | Patch, mostly bugs reported by AI | 
| 2026-10-09 | 3.54.0 | Feature release. Prompts, `%J` ,`.diskused` , 4% faster | 
| 2026-10-16 | 3.54.1 | Patch. A heap buffer overflow on 32-bit builds | 

## Summer of AI

Four patch releases in four months, and the release notes are pretty blunt about why.

Fixes for problems in 3.53.0 (and 3.53.1, 3.53.2, and 3.53.3) mostly coming from AIs.


Then, in the news log for 3.53.4:

There was a huge rush of AI-aided bug reporting shortly after the 3.53.0 release (2026-04-09), but lately the rate of bug reports has declined noticably [sic], and the bugs that are being reported are increasingly insignificant. Could it be that the AI-bug avalanche is coming to an end, and that all of the bugs that AIs are able to find have now nearly all been found and fixed?


And in the 3.54.0 announcement, after the feature list, one line: “countless fixes for AI-discovered bugs”.

Finding bugs became cheap. That is good for users in the long run. A project that tests more aggressively than most still spent a summer shipping patches, yikes.

## The Shell Got Good

The most visible change in 3.53.0 and 3.54.0 is the command-line shell, and it’s a pleasure to use now.

Results are formatted by a new library called QRF, the Query Result Formatter. Interactive sessions default to Unicode boxes. This is real output from my machine:

```
╭────┬────────┬─────╮
│ id │  name  │ qty │
╞════╪════════╪═════╡
│  1 │ bolt   │  10 │
│  2 │ nut    │  20 │
│  3 │ washer │   5 │
╰────┴────────┴─────╯
```
Numbers are right-justified. Column headers appear even when a query returns zero rows. Batch mode keeps the old format so scripts do not break. `.mode` grew `--rowcount`, `--titles always`, and two formatting knobs: `--ifmt` and `--fpfmt`, which take a printf-style format string for integers and floats.

```
.mode box --ifmt %06d
SELECT 42 AS n;
```
```
╭────────╮
│   n    │
╞════════╡
│ 000042 │
╰────────╯
```
The prompt is its own small language now. `/f` is the database file name, `/v` is the version, `/x` tests whether a transaction is open, and `/` is the escape character because a backslash would collide with your shell. The prompt documentation has the full table. `NO_COLOR` is honored, so the color-averse can opt out without patching anything.

Other things I found handy:

- `.diskused` prints a storage report, using the new`diskused()` function. It replaces`sqlite3_analyzer` , which is deprecated as of 3.54.0.
- `.indexes` now matches the pattern against index names instead of table names, which is what everyone assumed it did.`--expr` shows only expression indexes.
- `.progress --timeout 5` interrupts a statement after five seconds. For the queries you write once and immediately regret.
- `.timer once` times only the next statement instead of everything that follows.

All that stuff is CLI-exclusive. None of the shell work reaches your code, and you get the engine improvements and nothing else.

## ALTER TABLE Improvements

This is the main code-affecting change this year, at least for my code.

Before 3.53.0, adding `NOT NULL` to a column meant building a temporary table, copying the data in, and recreating every index. That was tedious, and it made migrations harder.

Now it is one statement.

```
ALTER TABLE users ALTER COLUMN email SET NOT NULL;
ALTER TABLE users ADD CONSTRAINT age_ok CHECK (age >= 0);
ALTER TABLE users DROP CONSTRAINT age_ok;
```
The schema comes back the way you would write it by hand:

```
CREATE TABLE users(id integer primary key, email text not null, age int);
```
Both operations validate existing rows:

```
Error near line 3: constraint failed
```
The table is left untouched and the constraint is not added. That is the correct behavior. A constraint that only applies to future rows is worse than no constraint.

The limit is that this is still not a full-scale `ALTER TABLE`. You cannot change a column type, add a `UNIQUE` constraint, or add a foreign key. For those, you still rebuild the table by hand. But the two most common cases in application development, “this field should never have been nullable” and “add a sanity check”, are now one line each.

## Conclusion

As always, SQLite got significantly faster and easier to use this year, without breaking backward compatibility. I use the CLI often, so the new prompt, the boxed output, and the small dot-command fixes are not cosmetic to me.

For my code, the new `ALTER TABLE` improvements and yearly performance bump is what really matters. My future migrations will be shorter, clearer, and less error-prone.
