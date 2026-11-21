---
id: collect-261121-veille-ia/langages-dev/postgresql-19-what-s-actually-shipping-what-got-cut
title: "PostgreSQL 19: What's Actually Shipping, What Got Cut"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["beta", "release", "postgresql", "snowflake", "notes", "parallel", "autovacuum", "project"]
source: https://dev.to/amareswer/postgresql-19-whats-actually-shipping-what-got-cut-4b1j
source_anchor: ""
source_lines: [1, 94]
section: "Langages_Dev"
---
Originally published on DevToolHub.


PostgreSQL 19 is late. The project released Beta 4 on September 24, 2026, reverted several headline features along the way, and now plans a release candidate in early October, with general availability possibly also in October. If you planned around SQL/PGQ graph queries, `GROUP BY ALL` or partition merge/split, those are out. `REPACK`, `pg_plan_advice` and parallel autovacuum are still in.

Here's what changed. It's taken from the PostgreSQL project's own Beta 4 announcement and release notes, plus Snowflake's engineering writeup of the revert cycle.

The project normally ships a major version every fall, usually in late September. This year, the Beta 4 announcement says the next step is a release candidate "in early October," and that GA "may also occur in October." The announcement explains the delay directly: the community "chose to remove some features from the PostgreSQL 19 release" so the release stays reliable and close to schedule.

Snowflake's post, written a week before Beta 4, counted 53 reverts since beta began, compared with about 44 for PostgreSQL 18. The difference this year is timing. Many reverts landed late and hit headline features, several after in-depth reviews that used AI tools to find bugs and build reproducible test cases.

The Beta 4 announcement lists these reverts since Beta 3:

- 
**SQL/PGQ** (property graph queries)
- **Online enabling and disabling of data checksums**
- **Temporal `UPDATE`/`DELETE ... FOR PORTION OF`**
- **`ALTER TABLE ... MERGE PARTITIONS` and `SPLIT PARTITIONS`**
- The new **`pg_get_role_ddl()`, `pg_get_tablespace_ddl()` and `pg_get_database_ddl()`** functions (removed)

Earlier in the cycle, per Snowflake's writeup, the project also reverted:

- 
**`GROUP BY ALL`** : a post-commit review found it missed special handling of`ORDER BY` entries with nondefault equality semantics, which produced wrong results. It is absent from the current release notes.
- **Non-text output formats for `pg_dumpall`**
- 
**`JSON_TABLE` `ON ERROR` cascading** to columns
- 
**Fast defaults for domains** with nonvolatile constraints
- **Logical replication database-specific snapshots**
- **Nested query tracking in `pg_stat_statements`**

Snowflake notes that property graphs, `GROUP BY ALL` and partition merge/split are now PostgreSQL 20 material.

⚠️ **Note on lz4:** Snowflake's post lists the default TOAST compression change to `lz4` as reverted, citing missing buildfarm support. But the official release notes, dated September 14, still list "Change the default TOAST compression method from pglz to the more efficient lz4." The two sources disagree, so check the final release notes before relying on the new default. You can always set `default_toast_compression` yourself.

These features are in the release notes and have survived the revert cycle so far:

| Feature | What it does | 
|---|---|
| `REPACK` | Reclaims disk space and reorganizes a table, combining `VACUUM FULL` and`CLUSTER` , with a concurrent option | 
| `pg_plan_advice` | New extension for stabilizing and controlling query planner decisions | 
| Parallel autovacuum | Autovacuum can use parallel workers, set with `autovacuum_max_parallel_workers` and a per-table`autovacuum_parallel_workers` | 
| `INSERT ... ON CONFLICT DO SELECT ... RETURNING` | Returns conflicting rows instead of doing nothing | 
| `IGNORE NULLS` /`RESPECT NULLS` | For `lead()` ,`lag()` ,`first_value()` ,`last_value()` and`nth_value()` | 

Snowflake's post, published before Beta 4, flagged `REPACK`, the fast-path foreign key checks and `postgres_fdw` statistics import as at risk. Beta 4 then shipped "several fixes for the new REPACK command" and "several fixes to the performance improvement feature for the foreign key constraint checks," which suggests both are still in. Nothing is final until GA, though.

Snowflake's writeup points to AI-assisted review as a real factor: more people are using AI tools to find bugs and produce reproducible test cases, and the fixes are large enough that pushing features to a later release made sense. It gives security patching as a parallel example: PostgreSQL used to average a couple of CVEs per release, while the August 2026 patch release for PostgreSQL 18 had 28. The same post notes the project had no official AI contribution policy at the time of writing.

The takeaway isn't that the release is in trouble. The process is working as designed: features that weren't ready came out, and the reasons are public on the hackers mailing list. If you're troubleshooting a current Postgres deployment in the meantime, our PostgreSQL troubleshooting guide covers the common problems.

1. 
**Stay on PostgreSQL 18 for production.** Snowflake's post calls it the safe upgrade target today, and 19 isn't at release candidate yet.
2. 
**Grep your code for reverted features.** Look for`GROUP BY ALL` ,`GRAPH_TABLE` / property graph syntax,`FOR PORTION OF` , and`MERGE PARTITIONS` /`SPLIT PARTITION` in migrations, ORM-generated SQL and internal tools.
3. 
**Test the features you plan to use on the beta.**`REPACK` and the foreign key changes were still getting fixes in Beta 4, so test them against your own workload.
4. 
**Check the final release notes at GA** before relying on anything flagged above, especially the`lz4` default.

For partitioning without the reverted merge/split commands, our PostgreSQL partitioning guide covers what works today. Also, if you run Postgres on Kubernetes, see PostgreSQL on Kubernetes with CloudNativePG for how major-version upgrades are handled there.

**Q: When will PostgreSQL 19 be released?**

A: The project's Beta 4 announcement (September 24, 2026) plans a release candidate in early October and says general availability may also occur in October. That's later than the usual late-September release.

**Q: Was GROUP BY ALL removed from PostgreSQL 19?**

A: Yes. It was reverted after a post-commit review found it mishandled `ORDER BY` entries with nondefault equality semantics, producing wrong results. It doesn't appear in the current PostgreSQL 19 release notes.

**Q: Is SQL/PGQ graph query support in PostgreSQL 19?**

A: No. The Beta 4 announcement lists "Revert SQL/PGQ (property graph query) support." Snowflake's writeup describes it as PostgreSQL 20 material.

**Q: Is REPACK still in PostgreSQL 19?**

A: As of Beta 4, yes. It's in the release notes, and Beta 4 shipped several fixes for it. It was flagged as at risk earlier, so confirm in the final release notes.

**Q: Should I upgrade to PostgreSQL 19 now?**

A: Not in production. It's still in beta. Use PostgreSQL 18 for production and test your workloads on the PostgreSQL 19 beta.

**Quick Summary:**

- PostgreSQL 19 Beta 4 shipped September 24, 2026; a release candidate is planned for early October and GA may follow in October
- Reverted: SQL/PGQ, `GROUP BY ALL` , partition merge/split,`FOR PORTION OF` , online checksum toggling, the new`pg_get_*_ddl()` functions and several smaller items
- Still shipping: `REPACK` ,`pg_plan_advice` , parallel autovacuum,`ON CONFLICT DO SELECT` ,`IGNORE NULLS` in window functions
- The `lz4` TOAST default is unclear: Snowflake reports it reverted, the September 14 release notes still list it
- Stay on PostgreSQL 18 for production and grep your code for the reverted syntax now

Check the PostgreSQL 19 release notes again at GA before you finalize a migration plan. They are the source that settles every open question above.

## Top comments (0)
