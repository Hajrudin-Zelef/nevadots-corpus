---
id: collect-261121-veille-ia/langages-dev/postgresql-19-ships-on-19-november-and-postgresql-14-leaves-support-2
title: "PostgreSQL 19 Ships on 19 November and PostgreSQL 14 Leaves Support"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["postgresql", "november", "version", "release", "support", "leaves", "project", "minor"]
source: https://www.veltrix.co.ke/news/postgresql-ships-november-postgresql-leaves-suppor-8a7f
source_anchor: ""
source_lines: [1, 69]
section: "Langages_Dev"
---
# PostgreSQL 19 Ships on 19 November and PostgreSQL 14 Leaves Support

## TL;DR

- PostgreSQL is an open-source relational database that teams install on their own servers, and the project supports each major version for five years before a final minor release ends the branch.
- The PostgreSQL project's RC 1 announcement, published on 15 October, schedules PostgreSQL 19 general availability for 19 November 2026.
- A second candidate, PostgreSQL 19 RC 2, arrives on 12 November, the same day as the quarterly update release for supported versions.
- PostgreSQL 14 leaves support on that same 12 November with version 14.24, so a cluster on 14 stops receiving fixes that day.
- PostgreSQL 19 brings REPACK, a single command that replaces VACUUM FULL and CLUSTER, and REPACK (CONCURRENTLY) rebuilds a table without an access-exclusive lock.
- The same release adds pg_plan_advice and pg_stash_advice for planner decisions, logical replication of sequence values, a start for logical replication without a restart, and a WAIT command that lets a standby wait on a log position.
- Defaults change under an upgraded cluster: JIT ships disabled, max_locks_per_transaction moves to 128, standard_conforming_strings is always on, RADIUS authentication is gone, and inet and cidr indexes move to GiST.
- The path off version 14 runs through pg_upgrade or a dump and restore, and pg_upgrade refuses clusters that carry btree_gist indexes on inet or cidr.
- The planning point: the version a team runs is a support date, and the date sits on the project's published calendar.

This release-notes briefing draws its dates from two PostgreSQL project documents and focuses on clearing PostgreSQL 19's documented incompatibilities before version 14 support ends. The 19 release notes carry an overview that names each headline feature, and the 19 RC 1 announcement sets the schedule plainly: RC 2 on 12 November 2026, general availability on 19 November 2026.

Netics visual, 11 November 2026: what a minor release is and what installing one actually involves.

## The project supports each major version for five years

The versioning policy gives a major version five years of bug and security fixes counted from its first release, and the last of those arrives as a final minor release. After that the branch sits unsupported. Minor releases land at least once every three months, targeted at the second Thursday of February, May, August and November.

That calendar is the practical part. A team planning a migration can read the final-release column and see the wall before it arrives, which is a rare luxury in infrastructure planning.

The project's own support table, read on 11 November 2026: version 14 lists 12 November 2026 in the final-release column, version 18 lists November 2030. Page from https://www.postgresql.org/support/versioning/.

## PostgreSQL 14 leaves support on 12 November

Version 14 first shipped on 30 September 2021, and the support table lists 14.24 as its current minor release with a final release date of 12 November 2026. After that final minor release, version 14 becomes unsupported.

The release notes provide an inventory of incompatibilities. An operational decision also needs an inventory of the extensions and drivers used by the application. Rehearse the chosen migration route on a production-like copy and run the critical queries. Verify backups, the rollback procedure and the maintenance-window duration before approving the migration.

The announcement as published on 15 October 2026, naming RC 2 on 12 November and general availability on 19 November. Page from https://www.postgresql.org/about/news/postgresql-19-rc-1-released-3409/.

## PostgreSQL 19 arrives on 19 November with REPACK

The release notes describe REPACK as the command that replaces VACUUM FULL and CLUSTER. The old commands remain for compatibility. REPACK (CONCURRENTLY) adds the version most operators want: rebuilding a table without taking an access-exclusive lock on it.

Planner control arrives as two extensions. pg_plan_advice stabilizes and controls planner decisions, and pg_stash_advice applies stored advice automatically based on a query. Autovacuum also gains parallel index processing and a scoring system that orders which tables it should reach first, on top of I/O worker processes that scale through io_min_workers and io_max_workers.

The overview section of the release notes, where the feature list and the migration requirements sit at the top of the page. Page from https://www.postgresql.org/docs/release/19.0/.

Replication gets the two changes that matter most on a busy cluster. Logical replication can be switched on with wal_level set to replica without restarting the server, and sequence values now replicate. A WAIT command lets a standby wait until a log position has been written, flushed or replayed.

## Defaults change under an upgraded cluster

Five defaults shift under the cluster a team restores into 19:

- JIT is disabled by default, because the optimizer cost that used to trigger it proved unreliable. Sites running large analytical queries need to enable it deliberately.
- max_locks_per_transaction moves from 64 to 128, and the release notes say the lock size allocation changed so settings must double to hold their previous capacity.
- standard_conforming_strings is forced on in the server. A dump taken with a pre-19 pg_dump while the setting was off will not load into 19.
- RADIUS authentication is removed, on the grounds that support existed only over UDP and that transport is unfixably insecure.
- inet and cidr indexes default to GiST, and pg_upgrade refuses clusters carrying btree_gist opclasses on those types because the old opclasses could exclude rows that belong in the result.

Netics visual, 11 November 2026: the setting-level differences between a 14 cluster and a 19 cluster, drawn from the release notes.

## pg_upgrade decides the path off version 14

The release notes accept three migration routes: a dump and restore with pg_dumpall, pg_upgrade, or logical replication. For planning purposes, treat pg_upgrade as a route that needs an explicit preflight check.

Two pg_upgrade refusal conditions are worth checking before a maintenance window: clusters with btree_gist indexes on inet or cidr, and clusters that use carriage returns or line feeds in database, role or tablespace names. Separately, dumps produced by pre-19 tools while standard_conforming_strings was off will not properly load into PostgreSQL 19. Each is a configuration fact that can be found in advance, and each one turns a migration into a debugging session when it surfaces mid-run.

There is also a spare SQL. MULE_INTERNAL encoding is gone, so a database that uses it needs a dump and restore into a different encoding.

The mark the PostgreSQL project publishes for press use. Image from https://www.postgresql.org/media/img/about/press/elephant.png.

Application compatibility deserves priority during upgrade planning, including dependencies introduced through custom web and mobile development. The five-year calendar supplies a deadline; migration safety depends on testing the chosen route and clearing PostgreSQL 19's documented incompatibilities and pg_upgrade refusals before the maintenance window.

*Originally published on the Netics blog.*
