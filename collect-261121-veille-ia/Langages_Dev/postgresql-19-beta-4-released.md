---
id: collect-261121-veille-ia/langages-dev/postgresql-19-beta-4-released
title: "PostgreSQL 19 Beta 4 Released!"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["postgresql", "release", "beta", "testing", "community", "please", "features", "more"]
source: https://www.postgresql.org/about/news/postgresql-19-beta-4-released-3386/
source_anchor: ""
source_lines: [1, 34]
section: "Langages_Dev"
---
The PostgreSQL Global Development Group announces that the fourth beta release of PostgreSQL 19 is now available for download. This release contains PostgreSQL 19 feature previews ahead of general availability, though some details of the release can change during the beta period.

You can find information about all of the PostgreSQL 19 features and changes in the release notes:

The next planned release of PostgreSQL 19 is the release candidate, which should occur in early October. Based on testing and evaluation, this means that the PostgreSQL 19 GA may also occur in October. More details will be provided as we move closer to these releases.

This beta release reverted several features that were originally planned for PostgreSQL 19. The PostgreSQL community strongly believes that, first and foremost, PostgreSQL must be reliable. Additionally, the community strives for a predictable release schedule. Because of this, the community chose to remove some features from the PostgreSQL 19 release so they can be potentially be included in a later major release after more time working through the community development and review process.

Your testing and feedback help the community ensure that PostgreSQL 19 upholds our standards of delivering a stable, reliable release of the world's most advanced open source relational database. Please read more about our beta testing process and how you can contribute:

To upgrade to PostgreSQL 19 Beta 4 from an earlier version of PostgreSQL,
you will need to use a strategy similar to upgrading between major versions of
PostgreSQL (e.g. `pg_upgrade` or `pg_dump` / `pg_restore`). For more
information, please visit the documentation section on
upgrading.

Fixes and changes in PostgreSQL 19 Beta 4 include:

`FOR PORTION OF`
clause.`ALTER TABLE ... MERGE PARTITIONS` and
`ALTER TABLE ... SPLIT PARTITIONS`.`pg_get_role_ddl()`, `pg_get_tablespace_ddl()`, and
`pg_get_database_ddl()` functions.`LC_COLLATE` to `C` in the postmaster process.`REPACK` command, including crashes, incorrect
behavior with invalid indexes and materialized views, and several permission
and error-reporting corrections.`WAIT FOR` command, including a deadlock and clearer
reporting of isolation-level errors.`FOREIGN_JOIN` list in `pg_plan_advice`.`CREATE PUBLICATION ... EXCEPT`.`COPY FROM` SIMD optimization.`pgbench --continue-on-error`.
Please see the release notes for a complete list of new and changed features:

The stability of each PostgreSQL release greatly depends on you, the community, to test the upcoming version with your workloads and testing tools to find bugs and regressions before the general availability of PostgreSQL 19. As this is a Beta, minor changes to database behaviors, feature details, and APIs are still possible. Your feedback and testing will help determine the final tweaks on the new features, so please test in the near future. The quality of user testing helps determine when we can make a final release.

A list of open issues is publicly available in the PostgreSQL wiki. You can report bugs using this form on the PostgreSQL website:

This is the fourth beta release of version 19. The PostgreSQL Project is planning to move to the release candidate portion of the release cycle, and will continue to issue release candidates until PostgreSQL 19 is made generally available. For further information please see the Beta Testing page.

If you have corrections or suggestions for this release announcement, please send them to the *pgsql-www@lists.postgresql.org* public mailing list.
