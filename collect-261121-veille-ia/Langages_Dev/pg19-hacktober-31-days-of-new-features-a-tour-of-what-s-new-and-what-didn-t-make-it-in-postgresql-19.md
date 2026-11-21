---
id: collect-261121-veille-ia/langages-dev/pg19-hacktober-31-days-of-new-features-a-tour-of-what-s-new-and-what-didn-t-make-it-in-postgresql-19
title: "PG19 Hacktober: 31 Days of New Features : A Tour of What's New (and What Didn't Make It) in PostgreSQL 19 Beta 4 - OpenSourceDB"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["beta", "postgresql", "fixes", "features", "several", "reverted", "new", "pg19"]
source: https://opensource-db.com/pg19-hacktober-31-days-of-new-features-a-tour-of-whats-new-and-what-didnt-make-it-in-postgresql-19-beta-4/
source_anchor: ""
source_lines: [1, 92]
section: "Langages_Dev"
---
Welcome to Day 1 of **PG19 Hacktober**, following last year’s PG18 Hacktober. Every day this October we’ll take a new feature or change in PostgreSQL 19 and look at it with a real example, a before/after demo, and an honest take on whether you should care.

We start with the most useful post to read first: **what’s in Beta 4, and what didn’t make it.**

## Where PostgreSQL 19 stands today

**PostgreSQL 19 Beta 4 was released on September 24, 2026.**

- The release notes still show the release date as “??”, so details can still change.
- The project has signaled that a **release candidate (RC) is expected in early October** .
- **GA may follow later in October** , but that isn’t guaranteed.

Beta 4 is a stabilization release: mostly bug fixes, plus a handful of larger reverts. If you’re planning around PG19, the reverts matter as much as the fixes.

### What to expect from this series

- **Same layout every time:** a short intro, a real example, a demo, and caveats, so you can finish each post in about 10 minutes.
- **One post a day, all of October:** 31 posts, each on a single feature or change.
- **Hands-on demos:** before and after behavior, with video where it helps.
- **Honesty about what’s missing:** some planned features were pulled in Beta 4. You’ll know what*not* to build plans around.

## What’s new ?

Beta 4 is mostly fixes to features that are new in 19, so the headline features are being hardened rather than removed.

- **Foreign key checks:** several fixes to the performance improvements for foreign key constraint checks.
- **REPACK:** several fixes for the new command, including crashes, incorrect behavior with invalid indexes and materialized views, and permission and error-reporting corrections.
- **WAIT FOR:** several fixes for the new command, including a deadlock and clearer reporting of isolation-level errors.
- **pg_plan_advice:** fixed validation of an empty`FOREIGN_JOIN` list.
- **CREATE PUBLICATION … EXCEPT:** several fixes.
- **Logical replication initial sync:** fixed table synchronization when replicating from an older PostgreSQL version to v19.
- **Logical replication conflict detection:** several fixes.
- **Autovacuum scoring:** several fixes to the new scoring system, including for TOAST tables.
- **COPY FROM:** fix for the SIMD optimization.
- **Partitioning:** fixed a crash when accessing a partition whose concurrent detach did not complete.
- **pgbench –continue-on-error:** clarified that failed transactions are reported separately.

**Why it matters:** `REPACK`, `WAIT FOR`, autovacuum scoring, `pg_plan_advice` and `EXCEPT` publications are all new in 19, and Beta 4 is where early testers’ crash and edge-case reports are landing. If you tested any of them on an earlier beta, re-run those tests on Beta 4.

## What didn’t make it in PostgreSQL beta4

This is the section to read before you plan your upgrade.

**Reverted**

- **SQL/PGQ (property graph queries):** support reverted. Stay with recursive CTEs or a dedicated graph tool.
- **Online enabling and disabling of data checksums:** reverted. Plan a maintenance window with`pg_checksums` , or enable checksums at`initdb` .
- **FOR PORTION OF:** support for temporal updates/deletes reverted. Keep your application-level or trigger-based approach.
- **ALTER TABLE … MERGE PARTITIONS and ALTER TABLE … SPLIT PARTITIONS:** both reverted. Merging and splitting partitions stays a manual create/attach/detach job.
- **LC_COLLATE in the postmaster:** the change that forced it to`C` was reverted.

**Removed**

- **pg_get_role_ddl():** removed.
- **pg_get_tablespace_ddl():** removed.
- **pg_get_database_ddl():** removed.

A revert in a beta doesn’t mean a feature is dead, and some may return in PostgreSQL 20. Just don’t build a roadmap on them.

## How to test Beta 4 safely

- **Use a throwaway environment.** Never put a beta near production data.
- **Treat beta clusters as disposable.** Catalog changes between betas and the final release can force a dump and restore.
- **Confirm your build:**

```
postgres=# SELECT version();
                                                   version                                                    
--------------------------------------------------------------------------------------------------------------
 PostgreSQL 19beta4 on x86_64-pc-linux-gnu, compiled by gcc (GCC) 11.5.0 20240719 (Red Hat 11.5.0-14), 64-bit
(1 row)
postgres=# show server_version;
 server_version 
----------------
 19beta4
(1 row)
```
- **Re-test what you tested before:**`REPACK` ,`WAIT FOR` , autovacuum, and`EXCEPT` publications.
- **Report problems** to the community. Beta testing only works if people send reports.

### Where we go from here

Over the next 30 days we’ll cover the features that survived, moving through performance, replication, operations, and developer-facing changes. If a feature changes between now and GA, I’ll update the relevant post.

**Tomorrow (Day 2): PostgreSQL I/O Performance Improvements: Smarter Read-Ahead & Visibility Map Updates**

We’ll look at two changes that affect how PostgreSQL 19 reads data:

- **Improved async I/O read-ahead scheduling:** how PostgreSQL decides what to read ahead of time, and what that means for scan performance.
- **Table scans update the visibility map:** how regular table scans now help keep the visibility map current, and why that matters for index-only scans and vacuum.

Follow along, and tell me which PG19 features you most want covered.
