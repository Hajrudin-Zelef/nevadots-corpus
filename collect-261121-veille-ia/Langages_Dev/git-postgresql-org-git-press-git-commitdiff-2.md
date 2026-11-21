---
id: collect-261121-veille-ia/langages-dev/git-postgresql-org-git-press-git-commitdiff-2
title: "git.postgresql.org Git - press.git/commitdiff"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["postgresql", "release", "fixes", "docs", "git", "beta", "candidate", "several"]
source: https://git.postgresql.org/gitweb/?p=press.git;a=commitdiff;h=f781f8aa1a38f8e9f0a8d64c7476729a7fcb4856
source_anchor: ""
source_lines: [1, 147]
section: "Langages_Dev"
---
--- /dev/null

+The PostgreSQL Global Development Group announces that the first release

+candidate of PostgreSQL 19 is now available for download. As a release

+candidate, PostgreSQL 19 RC 1 will be mostly identical to the initial release of

+PostgreSQL 19, though some more fixes may be applied prior to the general

+availability of PostgreSQL 19.

+

+The next [scheduled update release](https://www.postgresql.org/developer/roadmap/)

+for supported versions of PostgreSQL, on November 12, 2026, will include a large

+number of fixes. Because of this, the release team decided to include these

+fixes in PostgreSQL 19 before its general availability. This means a second

+release candidate, PostgreSQL 19 RC 2, will be available on the same day as the

+update release.

+

+The planned date for the general availability of PostgreSQL 19 is

+November 19, 2026. Please see the "Release Schedule" section for more details.

+

+Upgrading to PostgreSQL 19 RC 1

+-------------------------------

+

+To upgrade to PostgreSQL 19 RC 1 from earlier versions of PostgreSQL, you will

+need to use a major version upgrade strategy, e.g. `pg_upgrade` or

+`pg_dump` / `pg_restore`. For more information, please visit the documentation

+section on [upgrading](https://www.postgresql.org/docs/19/upgrading.html):

+

+[https://www.postgresql.org/docs/19/upgrading.html](https://www.postgresql.org/docs/19/upgrading.html)

+

+Changes Since 19 Beta 4

+-----------------------

+

+Several bug fixes were applied to PostgreSQL 19 during the Beta 4 period. These

+include:

+

+* Several fixes for [`REPACK (CONCURRENTLY)`](https://www.postgresql.org/docs/19/sql-repack.html).

+* Several fixes for [`pg_plan_advice`](https://www.postgresql.org/docs/19/pgplanadvice.html).

+* Several fixes for new query planner optimizations that rely on whether an

+expression can be `NULL`.

+* Several fixes for the [automatic enablement of logical replication](https://www.postgresql.org/docs/19/runtime-config-preset.html#GUC-EFFECTIVE-WAL-LEVEL)

+when `wal_level` is set to `replica`.

+* Fix [`postgres_fdw`](https://www.postgresql.org/docs/19/postgres-fdw.html)

+statistics import to run as the owner of the foreign table instead of the user

+running `ANALYZE`.

+* Several fixes for [parallel autovacuum](https://www.postgresql.org/docs/19/runtime-config-vacuum.html#GUC-AUTOVACUUM-MAX-PARALLEL-WORKERS).

+* Fix [`COPY (query) TO ... (FORMAT json)`](https://www.postgresql.org/docs/19/sql-copy.html)

+to use the query's column names, including aliases, as the JSON keys instead of

+the table's column names.

+

+For a detailed list of fixes, please visit the

+[open items](https://wiki.postgresql.org/wiki/PostgreSQL_19_Open_Items#resolved_before_19rc1)

+page.

+

+Release Schedule

+----------------

+

+This is the first release candidate for PostgreSQL 19. The release team plans to

+issue a second release candidate, PostgreSQL 19 RC 2, on November 12, 2026.

+Unless an issue is discovered that requires a delay or an additional release

+candidate, PostgreSQL 19 should be made generally available on

+November 19, 2026.

+

+For further information please see the

+[Beta Testing](https://www.postgresql.org/developer/beta/) page.

+

+Links

+-----

+

+* [Download](https://www.postgresql.org/download/)

+* [Beta Testing Information](https://www.postgresql.org/developer/beta/)

+* [PostgreSQL 19 Beta Release Notes](https://www.postgresql.org/docs/19/release-19.html)

+* [PostgreSQL 19 Open Issues](https://wiki.postgresql.org/wiki/PostgreSQL_19_Open_Items)

+* [Submit a Bug](https://www.postgresql.org/account/submitbug/)

+* [Donate](https://www.postgresql.org/about/donate/)

+

+If you have corrections or suggestions for this release announcement, please send them to the _pgsql-www@lists.postgresql.org_ public [mailing list](https://www.postgresql.org/list/).
