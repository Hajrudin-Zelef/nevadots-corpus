---
id: collect-261121-veille-ia/langages-dev/postgresql-19-release-candidate-what-shipped-what-was-cut-how-to-upgrade
title: "PostgreSQL 19 Release Candidate: What Shipped, What Was Cut, How to Upgrade"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-10-15"]
keywords: ["release", "postgresql", "candidate", "beta", "shipped", "november", "features", "before"]
source: http://dev.to/muhammadmoiz269/postgresql-19-release-candidate-what-shipped-what-was-cut-how-to-upgrade-5foi
source_anchor: ""
source_lines: [1, 148]
section: "Langages_Dev"
---
*Originally published at muhammadmoiz.dev.*

PostgreSQL 19 is not out yet. The first release candidate shipped on October 15, 2026, and the project now plans general availability for November 19, 2026, later than many teams expected. Several headline features announced in the June beta were pulled before the RC, so if you planned an upgrade around them, this guide covers what actually made the cut, what changes under your feet, and how to get ready.

The PostgreSQL 19 timeline this year has been longer than usual. Here are the dates that matter, all from the project's own announcements:

| Milestone | Date | 
|---|---|
| Beta 1 | June 4, 2026 | 
| Beta 4 (features removed) | September 24, 2026 | 
| Release Candidate 1 | October 15, 2026 | 
| RC 2 and minor updates for supported versions | November 12, 2026 (planned) | 
| PostgreSQL 14 end of life | November 12, 2026 | 
| PostgreSQL 19 general availability | November 19, 2026 (planned) | 

The RC 1 announcement explains that the release team chose to fold a large batch of fixes into version 19 before calling it final, which is why a second release candidate is on the calendar. The GA date can still move if a serious issue turns up.

Why so many late fixes? According to a DEV Community write-up that collects the public discussion, long-time committer Peter Eisentraut wrote on October 9 that LLM-assisted code review was at least partly responsible for the wave of reverts, because it surfaced defects in already-committed features much faster than before. The same write-up notes that some community members pushed back, saying a large share of AI-reported bugs turned out not to be real. Either way, the practical result for you is the same: a smaller, more conservative release.

If you read the Beta 1 announcement and got excited, check this list before you plan anything. The official Beta 4 announcement says these were removed so they can be reconsidered for a later major version:

- 
**SQL/PGQ property graph queries** (graph-style queries in standard SQL)
- **Turning data checksums on or off while the cluster is online**
- **Temporal `UPDATE` and `DELETE` using `FOR PORTION OF`**
- **`ALTER TABLE ... MERGE PARTITIONS` and `SPLIT PARTITIONS`**
- **`pg_get_role_ddl()`, `pg_get_tablespace_ddl()` and `pg_get_database_ddl()`**

`GROUP BY ALL`, which appeared in the Beta 1 highlights, is also missing from the current release notes. So if a blog post or tutorial shows any of these as "new in Postgres 19", treat it as outdated.

The good news is that a lot of useful work survived. Based on the RC-era release notes, these are the changes most app developers will notice:

*Photo: Domaintechnik on Unsplash*

- 
**`INSERT ... ON CONFLICT DO SELECT ... RETURNING`.** An upsert can now hand back the row that already existed instead of silently skipping it. That removes a common "insert, and if nothing came back, run a second SELECT" round trip.
- 
**Faster foreign key checks.** The notes list a general performance improvement for foreign key constraint checks.
- 
**A `WAIT` command for replicas.** A session on a standby can wait until a given WAL position has been written, flushed or replayed. That gives you read-your-writes behaviour when you send writes to the primary and reads to a replica.
- 
**More `jsonpath` string methods** , including`lower()` ,`upper()` ,`replace()` ,`split_part()` and the trim family.
- 
**`COPY TO` can output JSON** , with a`FORCE_ARRAY` option to produce a single JSON array.
- 
**Smarter `NOTIFY`** , which now wakes only the backends listening on the relevant channel. Apps that use`LISTEN` /`NOTIFY` for real-time features should see less overhead.

Here is the kind of upsert that gets simpler. This is an illustration, not code from the docs:


```
-- Create a tag if it doesn't exist, and always get its row back
INSERT INTO tags (name)
VALUES ('billing')
ON CONFLICT (name) DO SELECT
RETURNING id, name;
```
- 
**`REPACK`** , a built-in way to rebuild bloated tables, with a`CONCURRENTLY` option that avoids blocking reads and writes. It shipped with a narrower scope than first planned, and it received many crash and correctness fixes during the betas, so test it on a copy first.
- 
**Parallel autovacuum** via`autovacuum_max_parallel_workers` , plus a new scoring system for deciding which tables to vacuum first.
- 
**Automatic I/O worker scaling** between the new`io_min_workers` and`io_max_workers` settings.
- 
**`pg_plan_advice` and `pg_stash_advice`** , new modules for steering and pinning planner decisions.
- 
**Logical replication of sequences** , which makes replication-based online upgrades less fiddly.
- 
**TLS SNI support** through a new`pg_hosts.conf` file, so one server can present different certificates per hostname.

This is the section to read twice. These items come from the migration section and other behaviour changes in the release notes:

1. 
**JIT is off by default.** If you have analytical queries that benefited from JIT compilation, benchmark them and turn it back on explicitly if needed.
2. 
**RADIUS authentication is gone.** The notes describe the UDP-only implementation as unfixably insecure. Move to another method before upgrading.
3. 
**MD5 password logins now trigger a warning** after successful authentication (controlled by`md5_password_warnings` ). It's a nudge to switch to SCRAM.
4. 
**`standard_conforming_strings` is always on.** Dumps taken from older versions with this setting off will not load correctly into 19.
5. 
**`max_locks_per_transaction` defaults to 128 instead of 64** , and lock sizing changed. If you tuned this value, the notes say you effectively need to double it to keep the same capacity.
6. 
**`json_array()` returns an empty array instead of NULL** when there are no rows. Check any code that tests for NULL here.
7. 
**Default TOAST compression is now `lz4`** rather than`pglz` .
8. 
**`vacuumdb --analyze-only` now includes partitioned tables** when no targets are given.

A quick way to spot settings that matter on your current server:


```
SELECT name, setting
FROM pg_settings
WHERE name IN (
  'jit',
  'standard_conforming_strings',
  'max_locks_per_transaction',
  'password_encryption',
  'default_toast_compression'
);
```
And to find roles still using MD5 password hashes (run as a superuser):


```
SELECT rolname
FROM pg_authid
WHERE rolpassword LIKE 'md5%';
```
**Don't run an RC in production:** A release candidate is for testing. Wait for the final 19.0 release, and for most teams, for the first minor release after it, before moving production data.


1. 
**Know your current version.** Run`SELECT version();` . If you're on PostgreSQL 14, that matters more than 19: version 14 stops getting fixes on November 12, 2026. Plan to move to 17 or 18 now, or straight to 19 once it's stable.
2. 
**Check your provider.** Managed services (RDS, Cloud SQL, Supabase, Neon and others) adopt new majors on their own schedules. Look up when they will offer 19 rather than assuming it will be there on day one.
3. 
**Spin up the RC in a container** and restore a recent copy of your database into it. Run your test suite and your slowest real queries.
4. 
**Work through the breaking-change list above** , especially MD5, RADIUS and`standard_conforming_strings` .
5. 
**Pick an upgrade method.** The project recommends a major-version approach such as`pg_upgrade` (fast, in place) or`pg_dump` /`pg_restore` (slower, cleaner). Logical replication is another option if you need minimal downtime.
6. 
**Run `pg_upgrade --check` first** against a copy. It reports incompatibilities without changing anything.
7. 
**Update drivers and ORMs** (node-postgres, Prisma, Drizzle, psycopg and so on) in staging, then confirm migrations still run.
8. 
**Have a rollback plan** : a verified backup and a tested restore, not just a backup.

If you run a SaaS product, an e-commerce store or an internal tool on Postgres, PostgreSQL 19 is not urgent. Nothing in it is a security fix you must rush to apply, and the most eye-catching features were deferred. The more pressing date is November 12: if your database is on version 14, it will stop receiving security patches that day.

A sensible plan for most teams looks like this: get off 14 this quarter, test 19 in staging once it hits GA, and schedule the production upgrade for early 2027 after the first point release. The features that did ship, like `ON CONFLICT DO SELECT`, replica `WAIT` and faster foreign key checks, are real quality-of-life wins worth adopting once you're there.

If your app was built quickly, perhaps with an AI builder, and nobody is sure which Postgres version it runs, which auth method it uses, or whether backups restore, that's worth finding out before any upgrade. A production audit covers exactly those questions, and if you need help planning a database move as part of backend work, get in touch.

- PostgreSQL 19 RC 1 Released (postgresql.org)
- PostgreSQL 19 Beta 4 Released (postgresql.org)
- PostgreSQL 19 Beta 1 Released (postgresql.org)
- PostgreSQL 19 release notes (RC documentation)
- PostgreSQL versioning policy
- PostgreSQL 19 Release Date Moved to Nov 19 After AI Review Found Too Many Bugs (DEV Community)
- Cover photo: Taylor Vick on Unsplash

I'm Muhammad Moiz, a full-stack developer who builds web apps, SaaS and POS systems, and takes AI-built apps (Lovable, Bolt, Replit) to production. Get in touch.

## Top comments (0)
