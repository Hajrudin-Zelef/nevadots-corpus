---
id: collect-261121-veille-ia/langages-dev/postgresql-19-what-changed-what-matters-and-what-to-test-now
title: "PostgreSQL 19: What Changed, What Matters, and What to Test Now"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["postgresql", "release", "changed", "test", "review", "matters", "maintenance", "table"]
source: https://mktgsite.enterprisedb.com/blog/postgresql-19-what-changed-what-matters-and-what-test-now
source_anchor: ""
source_lines: [1, 105]
section: "Langages_Dev"
---
# PostgreSQL 19: What Changed, What Matters, and What to Test Now

## With the first release candidate available, PostgreSQL 19 brings practical advances in database maintenance, query planning, and replication.

Reclaiming disk space on a busy table. Understanding why a query’s execution plan changed. Keeping maintenance ahead of a growing workload. Making sure sequence values are ready when an application moves to a subscriber.


These are the kinds of operational problems PostgreSQL 19 addresses. Its current feature set includes concurrent table repacking, query plan advice, improvements to autovacuum, and sequence synchronization for logical replication. It also adds `ON CONFLICT DO SELECT`, so upserts can return existing rows, and `IGNORE NULLS` in window functions. For teams running Postgres<sup>®</sup>, there is plenty worth evaluating beyond the features that generated the earliest headlines. 

The first release candidate arrived on October 15. The community now plans a second release candidate for November 12 and general availability for November 19, subject to any issues requiring further changes. The additional time will allow fixes from the November maintenance release to be incorporated before PostgreSQL 19 becomes generally available. 


That makes this a good moment to reset expectations: Understand what changed, look closely at the capabilities moving toward release, and test them against the work your database actually does. If you run Postgres in production today, PostgreSQL 18 remains your production target until 19 reaches general availability. Test 19 in non-production now, so you are ready.

## 

What changed, and what to learn from it 

As described in the PostgreSQL 19 Beta 4 release notes, a significant number of major features were reverted from previous Beta releases. They include SQL property graph queries, partition merging and splitting, and the ability to enable or disable data checksums while the server is running. Others reported in the community’s tracking include& `GROUP BY ALL` and `UPDATE/DELETE ... FOR PORTION OF`. The community’s explanation is explicit: Reliability comes first, alongside a commitment to a predictable release schedule. Some features needed more development and review than this cycle could accommodate. 


It would be too simple, however, to treat every reversion as either a failure or an unqualified success.

 

In his recent reflection, EnterpriseDB’s (EDB’s) Peter Eisentraut describes several overlapping pressures: architectural problems in some features, AI-assisted review uncovering more edge cases, and senior developers spending substantial time on security fixes that would otherwise have been available for beta review. His account also distinguishes faster AI-assisted discovery from the assumption that the affected code was written by AI.


The same shift is visible in security. The August 2026 patch release for PostgreSQL 18 addressed 28 CVEs, where a release once averaged a couple. AI-assisted review is finding more, faster, across the project.


While scrutiny can be reduced, the real lesson here is that finding problems faster must be matched by the capacity to investigate, fix, and review them.

## 

REPACK: A new option for maintaining busy tables 

The new **REPACK** command rewrites a table to reclaim unused disk space and can reorganize its rows using an index. With **REPACK CONCURRENTLY**, reads and writes can continue through the bulk of that work. Concurrent changes, occurring while the table is being organized, are captured using logical decoding and applied before the operation completes.



This feature will relieve a significant pain point for applications that are update intensive. DBAs will not have to choose between tables being in accessible to the application and doing important maintenance operations to benefit overall performance. But *concurrent* does not mean *lock-free*. The final table-and-index swap still requires an **ACCESS EXCLUSIVE** lock. The operation also needs temporary disk space, and the documentation warns about visibility behavior for transactions using snapshots taken before repacking began.


The development effort includes EDB’s Álvaro Herrera, working alongside Antonin Houska and Mihail Nikalayeu, a useful example of the cross-company engineering behind a community release.

**What to test:** Choose a representative table with frequent updates and deletes. Run the operation under realistic write activity and long-running transactions. Measure reclaimed space, temporary storage, application latency, and the final lock, not just completion time. Given the scrutiny this feature has received, keep a rollback plan.

## 

Plan advice: More control, with a reason to use it carefully

PostgreSQL 19 introduces **pg_plan_advice**, which lets users describe, reproduce, and constrain specific planner decisions, including join order and scan methods. Once the module is loaded, **EXPLAIN (PLAN_ADVICE)** can generate an advice string describing those choices. The companion **pg_stash_advice** extension can store advice by query identifier and apply it automatically. This enables users to recover from query plan changes that introduce performance regressions or other problems.



This gives database teams another way to investigate a regression or stabilize selected planning decisions. Robert Haas implemented **pg_plan_advice** during his time at EDB. EDB will be adding layered features to this to enable customers to quickly recover from plan instability issues.


It is not a universal performance switch. Nor does it replace the planner or guarantee an identical execution plan in every circumstance. Plan advice constrains the choices the planner considers. As data changes, a restriction that once helped can prevent the planner from choosing a better approach. The documentation explicitly cautions against overriding it without weighing that risk.

**What to test:** Start with queries whose plans have previously caused problems. Compare the planner’s normal choices with narrowly targeted advice, using representative data and parameter values. Keep the intervention as small as possible, and check whether it remains useful as the workload changes.

## 

Autovacuum: Better prioritization and more parallel maintenance 

PostgreSQL 19 allows **autovacuum** to use parallel workers for index vacuuming and cleanup. It also introduces a scoring system for prioritizing vacuum and analyze work, with the **pg_stat_autovacuum_scores** view exposing those priorities. This is targeted parallelism, not a claim that every stage of vacuuming now runs in parallel.


For an operator, the interesting question is not simply how many workers can be launched. It is whether maintenance keeps up with the tables that need it most, within the resources available.

**What to test:** Use a mix of busy, frequently updated tables and larger, less active ones. Inspect the scores, observe which work is prioritized, and compare maintenance duration with application latency and resource consumption. The objective should be a healthier workload, not merely a busier maintenance subsystem.

 

## Logical replication: Sequence synchronization, with important boundaries

PostgreSQL 19 adds the ability to synchronize sequence values from a publisher to a subscriber. Initial synchronization can happen when a subscription is created, and **ALTER SUBSCRIPTION ... REFRESH SEQUENCES** can resynchronize known sequences later.


Two details matter when planning around this capability. First, the publisher must be running PostgreSQL 19 or later; upgrading only the subscriber does not enable it for an older publisher. Second, sequence values can become out of sync again as the publisher advances them. This is not continuous synchronization that removes the need to think about cutover procedures.


**What to test:** Rehearse the sequence-handling portion of your migration or cutover procedure. Check matching sequence definitions, synchronization completion, and the values the application will generate afterward. Test the workflow, not just whether the command succeeds.

## 

Test the release, not the early preview

These four areas are a starting point, not a complete upgrade checklist. Compatibility and default changes deserve attention too. For example, PostgreSQL 19 disables JIT compilation by default, so teams whose analytical workloads benefit from it should explicitly evaluate that setting rather than assume existing behavior carries forward.

It’s important to recognize that EDB has funded testing packages for the community for over 15 years and will continue to fund and support extensive testing for the community for the foreseeable future. This support of RPM packages and Beta 4 packages for the PostgreSQL Global Development Group will be an increasingly important part of how we contribute to the community.


Build a non-production environment that reflects your actual deployment: application queries, extensions, drivers, data distributions, and maintenance routines. Include upgrade and recovery rehearsals, not just feature demonstrations.


A release candidate is still a testing release, not a production recommendation. PostgreSQL’s testing guidance emphasizes using representative workloads to find regressions before general availability. Useful reports from users are part of how the community determines that a release is ready.


As the community gathers in Valencia for PGConf.EU during October 20–23, there is a productive conversation to have beyond which features made the cut. Which operational problems can PostgreSQL 19 help us solve? Where do the new capabilities need careful configuration? What does our testing reveal that someone else might otherwise discover in production?


The next contribution does not have to be a new feature. It can be a well-designed test, a reproducible bug report, or a clearer understanding of how PostgreSQL 19 behaves under a real workload.
