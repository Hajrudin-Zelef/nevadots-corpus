---
id: collect-261121-veille-ia/langages-dev/postgresql-19-is-out-patch-the-52-cves-first
title: "PostgreSQL 19 is out: patch the 52 CVEs first"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["november", "release", "postgresql", "released", "page", "fixed", "security", "cves"]
source: https://ecorpit.com/postgresql-19-general-availability-19-november-2026-52-cve-update/
source_anchor: ""
source_lines: [1, 155]
section: "Langages_Dev"
---
## On this page Â· 9 sections

**Summary.** PostgreSQL 19 is generally available: the PostgreSQL Global Development Group released it on 19 November 2026, a week after Release Candidate 2, and will support it until 19 November 2031. The same 12 November update released 18.7, 17.12, 16.16, 15.20 and 14.25, fixed 52 security vulnerabilities and more than 140 bugs, and made 14.25 the final release of PostgreSQL 14. By our count of the project's security page, 32 of the 52 carry a CVSS 3.1 score of 8.8, all 52 are fixed in 18.7, and 40 also affect 14. None of them was in the CISA Known Exploited Vulnerabilities feed published on 18 November. PostgreSQL 19 brings the REPACK command, the pg_plan_advice extension, a WAIT command for read-your-writes on standbys, self-scaling I/O workers and parallel autovacuum, plus new defaults for JIT, TOAST compression and lock memory. Our verdict: install the 12 November minor release on every server now, and treat 19 as a test target for 2027, not a same-week upgrade.

**About this article.** This is for database administrators, platform engineers and engineering leads in the US, UK and India who run PostgreSQL themselves or on a managed service. On 18 November 2026 we checked every version, date, CVE count and feature against postgresql.org (the 12 November release announcement, the roadmap, the versioning policy, the security page, the 18.7 and 19 release notes and the PostgreSQL 19 documentation), the NVD record for one CVE, the CISA KEV feed and section 8(5) of India's Digital Personal Data Protection Act, 2023. We re-checked the figures on 20 November 2026, after the project announced the 19.0 release on 19 November. This is general technical guidance, not advice for a specific system, and no vendor paid for inclusion. Researched with AI assistance and checked against the primary sources listed in the references.

## What was released, and when

Two things happened on 12 November 2026 and a third followed on 19 November. It helps to keep them apart.

| Item | Status on 20 November 2026 | Date | 
|---|---|---|
| Minor releases 18.7, 17.12, 16.16, 15.20, 14.25 | Released | 12 November 2026 | 
| 52 security fixes in those releases | Released | 12 November 2026 | 
| PostgreSQL 14 end of life (14.25 is final) | Confirmed | 12 November 2026 | 
| PostgreSQL 19 Release Candidate 2 | Released | 12 November 2026 | 
| PostgreSQL 19 general availability | Released | 19 November 2026 | 
| Next scheduled minor releases | Planned | 11 February 2027 | 

The minor release dates come from the 12 November announcement, the 19.0 date from the project's 19 November announcement and versioning page, and the 2027 minor release dates from the roadmap. The release went out on the planned date.

The project describes a release candidate as mostly identical to the final release, with some fixes still possible before general availability. It also says plainly that betas and release candidates are not meant for production systems. Now that 19.0 is out, replace any RC 2 test installs with 19.0.

## The 12 November update: what it fixed

The update covers every supported branch. It fixes 52 CVEs and more than 140 other bugs, several of which need extra steps after you install it. We tallied the project's security page on 18 November to see how the 52 spread across branches.

| Branch | Fixed in | CVEs fixed in this release | 
|---|---|---|
| 18 | 18.7 | 52 | 
| 17 | 17.12 | 47 | 
| 16 | 16.16 | 43 | 
| 15 | 15.20 | 41 | 
| 14 | 14.25 (final) | 40 | 

The counts are ours, from the affected and fixed columns of the security page; 40 CVEs affect all five branches, and 5 affect only 18. By component, 43 are in the core server, 6 in client programs and 3 in contrib modules. The score spread is wide: 32 rate 8.8, two rate 8.1, one rates 8.0, three rate 7.2, and the rest score between 3.1 and 6.5.

Most of the 8.8 findings share a pattern. The announcement describes them as type confusion, use-after-free or buffer overflow flaws that can let an authenticated user run arbitrary code on the server. Several sit in procedural languages and modules: plperl appears in five of the titles, and pltcl and dblink each have a use-after-free fix.

Client tools matter too. One example is CVE-2026-116318, an SQL injection in pg_dump. A user who can create objects can craft a not-null constraint name on an inherited column so that whoever restores the dump runs that user's SQL. It affects 18 before 18.7 only and scores 8.8. NVD published the record on 12 November and still showed it as awaiting analysis on 18 November, with the 8.8 score supplied by the PostgreSQL project. The update also fixes a pg_restore flaw and a reindexdb quoting flaw, so upgrade client packages on admin hosts and backup servers, not only the database servers.

On exploitation, the announcement does not say that any of the 52 is being exploited, and the CISA KEV feed dated 18 November 2026 lists no PostgreSQL entry. That can change. Most of these flaws need a valid login, so your exposure depends on who can connect and create objects.

### The post-update steps

A minor update needs no dump or pg_upgrade: stop the server, install the new binaries and restart. The announcement then lists work that the binaries alone do not do.

- **Reindex GIN indexes you suspect.** Several GIN bugs could corrupt indexes so that queries miss rows or return unrelated ones.

- **Reindex pg_trgm GiST indexes.** Any GiST index using the`gist_trgm_ops` operator class should be rebuilt.

- **Reindex B-tree indexes on bpchar columns.** This means columns declared as`bpchar` without a length;`char(n)` columns are not affected.

- **Reindex tables that report missing chunk number errors.** Reindexing the table also rebuilds its TOAST index.

- **Take a new full backup if you use incremental backups.** The 18.7 release notes say existing incremental backup files should not be trusted, and base on a fresh full backup from now on.

- **Check partitioned tables for NOT NULL NO INHERIT constraints.** These are now refused on partitioned tables because they could let a partition hold NULLs.

The announcement gives this query for the last item:

Drop the constraints you do not want, and repair the ones you keep with `ALTER TABLE ... ALTER CONSTRAINT ... INHERIT`, which fails if a partition already holds NULLs in that column. If you skipped earlier minor releases, read their release notes too, because their post-update steps still apply.

PostgreSQL 14 users get these fixes in 14.25, but it is the last 14 release: any flaw found from now on is fixed only in 15 and later. Our PostgreSQL 14.25 and upgrade plan covers that move in detail.

## What is new in PostgreSQL 19

The final release notes, dated 19 November 2026, list these headline features. The 19.0 announcement leads with the same features.

| Feature | What it does | Who should care | 
|---|---|---|
| REPACK and REPACK CONCURRENTLY | Rewrites a table to return dead space to the operating system; the concurrent form does not block reads and writes | Teams that run VACUUM FULL or pg_repack today | 
| pg_plan_advice and pg_stash_advice | Describe, pin or alter planner decisions, and apply advice per query automatically | Teams fighting plan regressions | 
| WAIT command | A session on a standby waits until a given WAL position is written, flushed or replayed | Apps that read from replicas after writing to the primary | 
| Self-scaling I/O workers | io_workers is replaced by io_min_workers, io_max_workers and two timing settings | Anyone who tuned asynchronous I/O in 18 | 
| Parallel autovacuum and scoring | Autovacuum can vacuum a table's indexes in parallel and prioritises tables by score | Large, write-heavy tables | 
| Logical replication without restart | With wal_level set to replica, logical replication can be enabled when needed | Teams adding change data capture | 
| Sequence replication | Logical replication now carries sequence values | Logical-replication upgrades and failovers | 

### REPACK replaces VACUUM FULL and CLUSTER

REPACK unifies the two older commands, which are kept for compatibility. Without the CONCURRENTLY option it holds an ACCESS EXCLUSIVE lock for the whole run, just as VACUUM FULL does; with it, other transactions can keep using the table, and the exclusive lock is taken only for the final swap of files. The command can also reorder rows by an index and run ANALYZE afterwards. For many teams this replaces an external tool for online table rebuilds, but test it on a copy of your largest table first: REPACK builds a temporary copy of the table and its indexes, so it needs free disk space at least equal to their combined size.

### Plan advice for unstable query plans

pg_plan_advice lets you describe, reproduce and change key planner decisions with a small advice language, so you can lock in a plan you know is good or try one the planner would not choose. pg_stash_advice applies advice by query ID. If you have used hint extensions on other platforms, check whether your provider will offer these modules: our note on the RDS PostgreSQL 19 preview covers how one managed service handled them during the betas.

### Read-your-writes on standbys

WAIT blocks until a given log sequence number is written, flushed or replayed, with an optional timeout; a NO_THROW option returns a status instead of an error. An application can take the LSN after a commit on the primary, then run WAIT on the replica before reading, so the read sees its own write.

### Smaller features worth knowing

The notes also add `INSERT ... ON CONFLICT DO SELECT ... RETURNING`, IGNORE NULLS for window functions, JSON output from COPY TO, COPY TO on partitioned tables, an EXPLAIN ANALYZE option that reports asynchronous I/O, server-side SNI with a new `pg_hosts.conf` file, and casts between bytea and uuid. The beta 1 announcement claimed up to 2 times faster inserts when foreign-key checks are present; the release notes list faster foreign-key checks without a figure.

### Two beta features that did not make 19.0

The beta 1 announcement in June listed SQL/PGQ property graph queries and switching data checksums on or off without a restart. Neither appears in the final 19.0 release notes, and the version 19 SQL command reference has no CREATE PROPERTY GRAPH entry. Do not plan work around either in 19.

## What 19 changes that can break things

The migration section of the notes lists the incompatibilities. These are the ones most likely to show up in a typical estate.

| Change | Effect | What to do | 
|---|---|---|
| JIT off by default | Large analytical queries may slow down | Turn JIT on for reporting workloads that gained from it | 
| TOAST compression defaults to lz4 | New large values compress with lz4 instead of pglz | Check that your builds include lz4 support | 
| max_locks_per_transaction default 64 to 128 | Lock memory sizing changed | Double any custom value to keep the same capacity | 
| io_workers removed | The old setting no longer exists | Move to io_min_workers and io_max_workers | 
| RADIUS authentication removed | RADIUS logins stop working | Move to another method before upgrading | 
| standard_conforming_strings always on | Old dumps made with it off will not load | Dump with the 19 pg_dump | 
| MD5 logins warn | Each MD5 login raises a warning | Move roles to SCRAM | 
| Parallel vacuum shares the cost limit | Parallel VACUUM may run slower | Review vacuum_cost_limit | 
| Full text search and pg_trgm use the database locale provider | Results can change on ICU or builtin databases | Reindex those indexes after pg_upgrade | 

The lz4 default comes from the notes' list of changes rather than the migration section. pg_upgrade also refuses clusters with btree_gist indexes on inet or cidr columns, or with carriage returns or line feeds in database, role or tablespace names, and the MULE_INTERNAL encoding is gone. RC 2 reverted one RC 1 change: if you built expression indexes, generated columns or partition keys on JSON_ARRAY() or JSON_OBJECT() over arrays, composite or range types while testing, drop or redefine them, because pg_dump, pg_restore and pg_upgrade cannot recreate them.

## When to move to 19

Version 19 is supported for five years from its first release on 19 November 2026; the versioning page lists its final release date as 19 November 2031. Version 18 is supported until 14 November 2030 and 17 until 8 November 2029.

| Situation | Our suggestion | 
|---|---|
| On 18 today | Apply 18.7 now; test 19 in staging and plan the move after the first minor releases | 
| On 17, 16 or 15 | Apply the 12 November minor release; choose 18 or 19 for your next major upgrade based on extension support | 
| On 14 | Apply 14.25 now; upgrade to 17 or 18 this winter rather than waiting on a brand-new 19 | 
| New project starting in 2027 | Start on 19 once your managed service and extensions support it | 

The project's scheduled minor releases after general availability are 11 February and 13 May 2027. Waiting for at least the first of those gives early adopters time to find .0 problems. Moving to 19 still needs a major version upgrade by pg_upgrade, dump and restore, or logical replication. If you are on 18, our PostgreSQL 18 tuning guide covers the asynchronous I/O settings that 19 changes.

## A one-week checklist

1. Inventory every PostgreSQL server and client host, with its major and minor version.

1. Install 18.7, 17.12, 16.16, 15.20 or 14.25 as applicable, starting with servers that untrusted or many users can log in to.

1. Upgrade client packages (pg_dump, pg_restore, reindexdb, psql) on admin and backup hosts.

1. Run the reindex steps that apply, and run the NOT NULL NO INHERIT query on partitioned tables.

1. Take a new full backup if you use incremental backups.

1. On managed services, confirm each instance reports the new minor version before you mark it done.

1. Stand up a 19.0 test instance and run your application tests against it.

## For teams in India, the UK and the US

The patching work is the same everywhere. Teams in India running systems that hold personal data should record when they patched: section 8(5) of the Digital Personal Data Protection Act, 2023 obliges data fiduciaries to take reasonable security safeguards against personal data breaches, and our DPDP Rules checklist covers when those duties apply. UK and EU teams facing security audits should be ready to show the minor version of every instance and the date of the update. US teams with change freezes over the holiday season should schedule the minor update before the freeze, not after it. For older branches, our PostgreSQL 15 end of life guide gives the next deadline: 11 November 2027.

## How eCorpIT can help

eCorpIT is a Gurugram-based technology organisation, founded in 2021, assessed at CMMI Level 5 and MSME certified, with senior-led engineering teams working across AWS, Microsoft and Google platforms. Our PostgreSQL upgrade and migration service can roll out the 12 November minor releases with their reindex steps, then test and plan your move to PostgreSQL 18 or 19 on RDS, Azure, Cloud SQL or your own servers. Talk to us at /contact-us/.

Last updated: 20 November 2026.
