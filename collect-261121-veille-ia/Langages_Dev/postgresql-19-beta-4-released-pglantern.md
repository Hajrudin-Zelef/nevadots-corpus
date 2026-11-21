---
id: collect-261121-veille-ia/langages-dev/postgresql-19-beta-4-released-pglantern
title: "PostgreSQL 19 Beta 4 Released! · pgLantern"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-09-24"]
keywords: ["postgresql", "beta", "release", "several", "fixes", "community", "revert", "docs"]
source: http://pglantern.com/messages/179025483822.2108891.11599274854803065062%40wrigleys.postgresql.org
source_anchor: ""
source_lines: [1, 114]
section: "Langages_Dev"
---
To: PostgreSQL Announce <pgsql-announce@lists.postgresql.org>

    
    
      Date: 2026-09-24 13:00 UTC
    

    
      Message-ID:
      179025483822.2108891.11599274854803065062@wrigleys.postgresql.org
      
    

    
      Lists: pgsql-announce
    

  The PostgreSQL Global Development Group announces that the fourth beta release of
PostgreSQL 19 is now [available for download](https://www.postgresql.org/download/).
This release contains PostgreSQL 19 feature previews ahead of general
availability, though some details of the release can change during the beta
period.
You can find information about all of the PostgreSQL 19 features and changes in
the [release notes](https://www.postgresql.org/docs/19/release-19.html):
[https://www.postgresql.org/docs/19/release-19.html](https://www.postgresql.org/docs/19/release-19.html)
The next planned release of PostgreSQL 19 is the release candidate, which should
occur in early October. Based on testing and evaluation, this means that the
PostgreSQL 19 GA may also occur in October. More details will be provided as
we move closer to these releases.
This beta release reverted several features that were originally planned for
PostgreSQL 19. The PostgreSQL community strongly believes that, first and
foremost, PostgreSQL must be reliable. Additionally, the community strives for a
[predictable release schedule](https://www.postgresql.org/developer/roadmap/).
Because of this, the community chose to remove some features from the
PostgreSQL 19 release so they can be potentially be included in a later major
release after more time working through the community development and review
process.
Your testing and feedback help the community ensure that PostgreSQL 19
upholds our standards of delivering a stable, reliable release of the
world's most advanced open source relational database. Please read more about
our [beta testing process](https://www.postgresql.org/developer/beta/) and how
you can contribute:
[https://www.postgresql.org/developer/beta/](https://www.postgresql.org/developer/beta/)
Upgrading to PostgreSQL 19 Beta 4
---------------------------------
To upgrade to PostgreSQL 19 Beta 4 from an earlier version of PostgreSQL,
you will need to use a strategy similar to upgrading between major versions of
PostgreSQL (e.g. `pg_upgrade` or `pg_dump` / `pg_restore`). For more
information, please visit the documentation section on
[upgrading](https://www.postgresql.org/docs/19/static/upgrading.html).
Changes Since Beta 3
--------------------
Fixes and changes in PostgreSQL 19 Beta 4 include:
* Revert SQL/PGQ (property graph query) support.
* Revert online enabling and disabling of data checksums.
* Revert support for temporal updates/deletes provided by the `FOR PORTION OF`
clause.
* Revert `ALTER TABLE ... MERGE PARTITIONS` and
`ALTER TABLE ... SPLIT PARTITIONS`.
* Remove the `pg_get_role_ddl()`, `pg_get_tablespace_ddl()`, and
`pg_get_database_ddl()` functions.
* Revert a change that forced `LC_COLLATE` to `C` in the postmaster process.
* Several fixes to the performance improvement feature for the foreign key
constraint checks.
* Several fixes for the new `REPACK` command, including crashes, incorrect
behavior with invalid indexes and materialized views, and several permission
and error-reporting corrections.
* Several fixes for the new `WAIT FOR` command, including a deadlock and clearer
reporting of isolation-level errors.
* Fix validation of an empty `FOREIGN_JOIN` list in `pg_plan_advice`.
* Several fixes for `CREATE PUBLICATION ... EXCEPT`.
* Fix logical replication initial table synchronization when replicating from
an older PostgreSQL version to v19.
* Several fixes for logical replication conflict detection.
* Several fixes for the new autovacuum scoring system, including for TOAST
tables.
* Fix for the `COPY FROM` SIMD optimization.
* Fix a crash when accessing a partition whose concurrent detach did not
complete.
* Clarify that failed transactions are reported separately when using
`pgbench --continue-on-error`.
Please see the [release notes](https://www.postgresql.org/docs/19/release-19.html)
for a complete list of new and changed features:
[https://www.postgresql.org/docs/19/release-19.html](https://www.postgresql.org/docs/19/release-19.html)
Testing for Bugs & Compatibility
--------------------------------
The stability of each PostgreSQL release greatly depends on you, the community,
to test the upcoming version with your workloads and testing tools to find bugs
and regressions before the general availability of PostgreSQL 19. As this is a
Beta, minor changes to database behaviors, feature details, and APIs are still
possible. Your feedback and testing will help determine the final tweaks on the
new features, so please test in the near future. The quality of user testing
helps determine when we can make a final release.
A list of [open issues](https://wiki.postgresql.org/wiki/PostgreSQL_19_Open_Items)
is publicly available in the PostgreSQL wiki.  You can
[report bugs](https://www.postgresql.org/account/submitbug/) using this form on
the PostgreSQL website:
[https://www.postgresql.org/account/submitbug/](https://www.postgresql.org/account/submitbug/)
Beta Schedule
-------------
This is the fourth beta release of version 19. The PostgreSQL Project is
planning to move to the release candidate portion of the release cycle, and
will continue to issue release candidates until PostgreSQL 19 is made
generally available. For further information please see the
[Beta Testing](https://www.postgresql.org/developer/beta/) page.
Links
-----
* [Download](https://www.postgresql.org/download/)
* [Beta Testing Information](https://www.postgresql.org/developer/beta/)
* [PostgreSQL 19 Beta Release Notes](https://www.postgresql.org/docs/19/release-19.html)
* [PostgreSQL 19 Open Issues](https://wiki.postgresql.org/wiki/PostgreSQL_19_Open_Items)
* [Submit a Bug](https://www.postgresql.org/account/submitbug/)
* [Donate](https://www.postgresql.org/about/donate/)
If you have corrections or suggestions for this release announcement, please send them to the _pgsql-www@lists.postgresql.org_ public [mailing list](https://www.postgresql.org/list/).
