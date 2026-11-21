---
id: collect-261121-veille-ia/langages-dev/edb-announces-general-availability-of-postgresql-19-for-sovereign-ai-data-systems-celebrating-30-yea
title: "EDB Announces General Availability of PostgreSQL 19 for Sovereign AI Data Systems: Celebrating 30 Years of Community Innovation"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-11-19"]
keywords: ["postgresql", "data", "postgres", "community", "plan", "open", "source", "maintenance"]
source: https://www.prnewswire.com/news-releases/edb-announces-general-availability-of-postgresql-19-for-sovereign-ai-data-systems-celebrating-30-years-of-community-innovation-302945001.html
source_anchor: ""
source_lines: [1, 80]
section: "Langages_Dev"
---
*Marking three decades of open source excellence alongside PostgresÂ® creator Michael Stonebraker, PostgreSQL 19 delivers breakthrough online maintenance and query plan stability as the foundation of EDB Postgres AI.* 

WILMINGTON, Del., Nov. 19, 2026 /PRNewswire/ -- EDB, the leading sovereign AI data system company, today welcomed the general availability of PostgreSQL 19, released in the 30th year of the PostgreSQL open source project. This year's release delivers long-requested advances in online maintenance and query plan stability. It also reflects a deliberate choice by the PostgreSQL community to ship only the features that earned production trust, at the moment Postgres is becoming the de facto data foundation for AI and agentic applications.

The community's history of always prioritizing reliability is especially important this year, because AI has raised the stakes. Enterprises are putting billions of agents to work directly on live operational data, and globally 58% would consider Postgres for their next data layer decision (Thinker Media Group, October 2026). Agents that read and write production records around the clock leave no room for error in the database beneath them.

"AI has made it faster than ever to write software and faster than ever to find what is wrong with the code and what it produces," said **Tom Kincaid, SVP of Databases Servers and Tools at EDB**. "With PostgreSQL 19, the Postgres community did what it has done for 30 years: It took the time to evaluate all feedback and make the right decision for the database that enterprises trust with their mission-critical data in the age of agents."

**PostgreSQL 19 release highlights**

For organizations that run on Postgres, PostgreSQL 19 means fewer maintenance windows and fewer surprise slowdowns. Two of its headline capabilities ship with PostgreSQL and replace work that used to require off-hours outages, third-party tools, or emergency SQL rewrites:

- **Table cleanup while applications keep running:** The new REPACK command rewrites a table to reclaim the wasted space that builds up as records change. With REPACK (CONCURRENTLY), reads and writes continue through the bulk of that work, so teams no longer choose between taking a busy table offline and letting bloat slow it down. EDB's Ãlvaro Herrera worked on the feature alongside Antonin Houska and Mihail Nikalayeu.
- **Steady plans for critical queries:** pg_plan_advice lets database teams describe, reproduce, and constrain specific planner decisions, and the companion pg_stash_advice extension can store that advice and apply it automatically. A critical query can keep a proven plan through upgrades and data growth, with no application code changes. Robert Haas implemented pg_plan_advice during his time at EDB.

The release also includes:

- **Autovacuum that keeps up:** Autovacuum can use parallel workers for index vacuuming and cleanup, and a new scoring system directs maintenance to the tables that need it most.
- **Smoother migrations and cutovers.** Logical replication can now synchronize sequence values from publisher to subscriber.
- **OAuth refinements:** Building on the OAuth support introduced in PostgreSQL 18, administrators can apply different validator settings to different groups of users in pg_hba.conf, and client debugging options are more granular to reduce the risk of exposing tokens. EDB's Jacob Champion describes the changes in OAuth in Postgres 19 (and beyond).
- **Developer conveniences:** ON CONFLICT DO SELECT lets upserts return existing rows, and window functions now support IGNORE NULLS.

The complete list is in the PostgreSQL 19 release notes.

**Continued open source commitments, now across two communities**

EDB remains one of the largest commercial contributors to PostgreSQL, and over the past two decades no company has contributed more to the open source project. EDB engineers serve the community as committers, major contributors, and core team members. EDB Postgres AI (PG AI) is defined as one AI data system with four layersâmulti-model data, governance at the data layer, agentic runtime, and a sovereign control planeâacross four deployment modes: bring-your-own-cloud (BYOC), developer cloud, self-hosted/on-premises, and physical far edge.

Beyond code, EDB has funded building, testing, and producing publicly available packages for the community for more than 15 years, a level of support that proved its value across PostgreSQL 19's four betas and two release candidates. CloudNativePGâ¢, the Kubernetes operator for Postgres that EDB created and open sourced, became a Cloud Native Computing Foundation Incubating project on November 10. EDB also donated Klio to the CloudNativePG community for backup and recovery with near-zero data loss.

That commitment now extends to where Postgres users are already going. More than half (55%) already run or are evaluating ClickHouseÂ® software alongside Postgres, because agents need both: Postgres holds the live operational record, and ClickHouse software analyzes billions of events in real time. With Altinity, which EDB acquired this month, EDB's engineers now contribute to that open source community too. Altinity is the No. 2 independent contributor to ClickHouse software and maintains the Altinity Kubernetes Operator for ClickHouse, which has been pulled more than 108 million times from Docker Hub. Under one governance model, the pair lets agents work from both the current state of the business and the patterns across its history, and EDB will keep investing in both open source communities.

*"Open source is the foundation of EDB Postgres AI, our multi-model data and AI system," said Kevin Dallas, CEO of EDB. "Postgres, ClickHouse software, WarehousePG, and vector search are increasingly what enterprises choose to put AI and agents into production. We will keep backing the community developers who advance these projects, along with Kubernetes and Iceberg, and we look forward to even more innovation still to come."*

**In conversation with Stonebraker and the builders of Postgres**

To mark PostgreSQL 19's release and commemorate Postgres's 30th anniversary, EDB today launched a two-part series on its *AI & Data Horizons* podcast, opening with Michael Stonebraker, the creator of Postgres and recipient of the 2014 ACM A.M. Turing Award.

Few people have shaped the database industry more. Stonebraker built Postgres around one idea, that users should be able to extend the database themselves, and that idea is why it has absorbed every major shift in data since, from geospatial and JSON to the vectors that power AI. A design decision made at UC Berkeley in 1986 is the reason a database older than the web is the one enterprises are choosing for the age of agents.

The two episodes:

- **Michael Stonebraker (****available today****):** Why Postgres is "the intergalactic answer to open source," controlled by no single company, and why data-heavy agents belong in the data layer
- **Bruce Momjian, PostgreSQL core team member:** Why software developed in the open keeps outlasting software that is only distributed as open source

Episodes are available at enterprisedb.com/ai-data-horizons and on Spotify, Apple Podcasts, and Amazon Music.

**An award-winning playbook for building on Postgres in the AI era**

For leaders turning this foundation into strategy, EDB's O'Reilly book, *Building a Data and AI Platform with PostgreSQL*, explains how to treat data and AI as a platform instead of a set of pilots; ground generative and agentic systems in trusted transactional data; and build sovereign, compliant architectures on Postgres. The book is available as a free download.

The book is part of EDB's "Intelligence Moves to the Data" thought leadership program, which won Silver in the Go to Market: Thought Leadership category at 2026 The Drum Awards for B2B.

**Availability**

PostgreSQL 19 is available now from postgresql.org. EDB begins supporting PostgreSQL 19 today, and PG AI's enterprise-grade Postgres servers will ship with Version 19 in February 2027.

**About EDB**

EDB Postgres<sup>Â®</sup> AI (PG AI) is the sovereign AI data system for the agentic enterprise. Built on Postgres, the world's leading open source database, PG AI unifies transactional, analytical, and AI workloads in a single architecture, eliminating the data movement, ETL, and operational fragmentation that slow enterprises down. With governance enforced at the data layer and the flexibility to deploy on-premises, in hybrid environments, or across clouds, enterprises operationalize their data and AI on infrastructure they own and controlâreaching production-ready sovereign AI in weeks, not months. As one of the most active contributors to the PostgreSQL project, EDB is deeply invested in the vitality of the global open source community. To learn more, visit www.enterprisedb.com.

**About Altinity**

Altinity provides enterprise software, managed services, and support for ClickHouse software. Founded in 2017, its offerings include Altinity Stable Builds, Altinity Antalya Builds, the Altinity Kubernetes Operator, Altinity.Cloud, and 24/7 enterprise support for high-performance analytics. Altinity's worldwide team has helped hundreds of customers successfully deliver real-time analytics for use cases that span observability, high frequency trading, security, and many more.

*EnterpriseDB and EDB are registered trademarks of EnterpriseDB Corporation. Postgres and PostgreSQL are registered trademarks of the PostgreSQL Community Association of Canada and used with their permission.* 

*The software and services described in this announcement are provided by EDB and Altinity, not ClickHouse, Inc. References to ClickHouse software do not imply sponsorship or endorsement by ClickHouse, Inc. Altinity is now an EDB company and is not affiliated with ClickHouse, Inc. ClickHouse is a registered trademark of ClickHouse, Inc.* 

*All other trademarks are owned by their respective owners.* 

**Media Contact**Anwar Ahmad 

[email protected]

(347) 206-5651

SOURCE EnterpriseDB

Share this article
