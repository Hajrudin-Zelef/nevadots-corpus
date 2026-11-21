---
id: collect-261121-veille-ia/langages-dev/postgresql-most-widely-used-open-source-stackscape
title: "PostgreSQL: Most widely used open-source | Stackscape"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-11-15"]
keywords: ["data", "postgresql", "open-source", "architecture", "platform", "operational", "types", "stackscape"]
source: https://stackscape.org/en/p/postgresql/
source_anchor: ""
source_lines: [1, 238]
section: "Langages_Dev"
---
## Key points

- The leading open-source relational database, developed by the community for nearly 30 years since originating from UC Berkeley's POSTGRES
- MVCC transactions, rich data types and indexes, and an extension architecture have formed an ecosystem including pgvector, PostGIS, and TimescaleDB
- 2025-09: PostgreSQL 18 introduces asynchronous I/O, uuidv7, and OAuth authentication; 2026-09: 19 Beta 4 released; latest minor is 18.6
- #1 most-used DB in the Stack Overflow 2025 survey; AWS, Azure, and Google Cloud offer managed services; OpenAI runs it as a core DB
- In an enterprise data platform, a candidate store for catalog and orchestrator metadata, business operational DBs, and combined vector search

## Stackscape verdict

Core pickGeneral data platform view Â· October 2026 review

The permissive PostgreSQL License, decades of production validation, and abundant talent and managed services make it a fit as the standard metadata and operational DB for a data platform. Parts that need large-scale analytics and horizontal scaling should be split off to OLAP engines or distributed DBs.

Essential role: Operational RDB

## Overview

PostgreSQL is an open-source relational database developed not by a particular company but by the PostgreSQL Global Development Group (PGDG) community. With standard SQL compliance, ACID transactions, and diverse data types and indexes, it is used as the operational DB of business systems, and most data platform tools such as Airflow, DataHub, and Superset support PostgreSQL by default as a metadata store. The license is the PostgreSQL License, in the BSD and MIT family, with almost no restrictions on commercial use.

Its differentiator is the extension architecture. Users can add data types, functions, index methods, and foreign data wrappers (FDW), so pgvector handles vector search, PostGIS spatial data, and TimescaleDB time series in the same DB. JSONB handles semi-structured data, and logical replication can pass change data to CDC tools such as Debezium, so it also serves as a source connecting operational DBs and the data platform. On the other hand, its single-node-centered architecture means horizontal scaling requires extensions such as Citus or a distributed DB.

PostgreSQL 18, released in September 2025, added asynchronous I/O, skip scan for multicolumn B-tree indexes, uuidv7, virtual generated columns, and OAuth 2.0 authentication, and enabled data checksums by default on new clusters. 19 Beta 4 was released in September 2026 ahead of the next major release, and support for the 14 line ends in November 2026. Cloud managed services and commercial distributions such as EDB provide technical support, giving a wide choice of operating models.

## Architecture

PostgreSQL has a process-based architecture that spawns one backend process per client connection, and the processes write data pages together through shared buffers. Every change is first recorded in the WAL (Write-Ahead Log), which underpins crash recovery and streaming and logical replication. Concurrency is handled by MVCC so reads and writes do not block each other, and the old row versions left behind are cleaned up by autovacuum. For queries, a cost-based planner builds execution plans and parallel query splits large scans. From 18, asynchronous I/O lets sequential scans and vacuum issue multiple reads at once. Extensions add types, functions, and index access methods through CREATE EXTENSION. The 19 beta includes non-blocking table rebuilds (REPACK CONCURRENTLY), parallel autovacuum, read-your-writes guarantees on replicas (WAIT FOR LSN), and SQL/PGQ property graph queries.

- **Backend ProcessÂ·Shared Buffers** Per-connection processes and a shared-memory page cache
- **WALÂ·Replication** Log underlying crash recovery, streaming replication, and logical replication
- **MVCCÂ·Autovacuum** Multi-version concurrency control and automatic cleanup of old row versions
- **PlannerÂ·Executor** Cost-based execution planning and parallel query execution
- **ExtensionÂ·FDW** Type, function, and index extensions and external data connections

## Strengths Â· Limitations

### Strengths

- PostgreSQL License places almost no restrictions on commercial use, modification, or redistribution, with no vendor lock-in
- Extension architecture lets one DB handle vector, spatial, time-series, and full-text search
- Streaming and logical replication plus Debezium integration make CDC sources and high-availability setups easy
- Supported by default as a metadata store by most data platform tools
- AWS, Azure, and Google Cloud managed services and abundant skilled staff widen operating options
- Annual major release and a 5-year support policy make upgrade planning predictable
- Standard SQL extensions such as JSONB, full-text search, and window functions handle even semi-structured data in a single DB

### Limitations

- Single-node, write-centered architecture means horizontal scaling needs extensions such as Citus or a distributed DB
- Large-scale aggregation and analytics are slower than columnar OLAP engines, so roles must be separated from the analytics layer
- Process-per-connection architecture requires a connection pooler such as PgBouncer in high-connection environments
- Given MVCC, heavily updated tables require operational skill in vacuum and bloat management
- Community distributions lack transparent data encryption (TDE), so supplement with a commercial distribution or storage encryption

## Use cases

### Common: platform metadata store

Consolidates the metadata of tools such as Airflow, DataHub, Superset, and MLflow in a single PostgreSQL cluster to unify backup and high-availability standards. Databases and accounts are separated per tool, and a connection pooler manages connection counts.

### Financial services: transaction ledger CDC source

Streams transaction table changes from the business DB to Kafka via logical replication and Debezium for delivery to the lakehouse and real-time aggregation. To reduce load on the source, replication slots and WAL retention monitoring are designed in together.

### Public sector: RAG store for citizen-complaint documents

Stores regulation and manual document chunks and embeddings with pgvector and searches them together with department and permission metadata conditions. For vectors in the hundreds of millions or more, or high-throughput search, consider a dedicated vector DB such as Milvus.

### Retail: order and inventory operational DB

Processes orders, payments, and inventory deductions as transactions to guarantee consistency, and hands analytics data to an OLAP engine via CDC. Bulk aggregation queries are separated from the operational DB to prevent service latency. For products with heavy inventory-deduction contention, the design must narrow the row-lock scope.

## Adoption checklist

1. Finalize the standard major versions (17 and 18), the annual upgrade cycle, and the pg_upgrade procedure
2. Decide the operating model: self-managed or managed service (RDS, Cloud SQL, etc.)
3. Validate the high-availability setup (Patroni, etc.) and backup and PITR recovery time objectives
4. Establish an allowed extension list (pgvector, PostGIS, etc.) and version management criteria
5. Decide the scope of roles, row-level security (RLS), audit logging (pgAudit), and encryption
6. Plan for the end of support for the 14 line in 2026-11 and for validating extension compatibility after 19 is released

## Alternatives

| Alternative | How it differs | 
|---|---|
| TiDB | MySQL-compatible distributed HTAP that is strong in horizontal scaling; PostgreSQL leads in extension ecosystem and single-node efficiency | 
| DuckDB | Fast for embedded columnar analytics; PostgreSQL is better suited to the role of a concurrent transactional operational DB | 
| ClickHouse | High performance on large aggregation analytics; PostgreSQL is better suited to transaction- and consistency-centered business DBs | 
| OpenTenBase | PostgreSQL-family distributed DB focused on scalability; PostgreSQL has the larger community and extension compatibility | 

## Public adoption

- Microsoft maintains a team of committers and contributors working directly on upstream PostgreSQL, committed 345 changes to the latest release, and took part in PostgreSQL 18's asynchronous I/O, vacuum, and query-planning improvements. It offers this engine as managed services such as Azure Database for PostgreSQL. From commit to cloud: Powering whatâs next for PostgreSQL (Microsoft Azure Blog) â
- Amazon said it employs six PostgreSQL committers, including three on the security team, and that AWS engineers were counted among the major contributors to the PostgreSQL 16 release (2023). Behind the scenes on AWS contributions to open source databases (AWS Open Source Blog) â
- Baidu AI Cloud offers PostgreSQL engine instances (RDS for PostgreSQL) as managed instances in its relational database service RDS. Baidu AI Cloud RDS documentation: Log in to and Create an Instance â
- Tencent Cloud offers TencentDB for PostgreSQL, a managed service based on open-source PostgreSQL that operates installation, high-availability replication, and backups on the customer's behalf. TencentDB for PostgreSQL Overview â
- PolarDB for PostgreSQL, a cloud-native DB, is 100% compatible with PostgreSQL and highly compatible with Oracle syntax, offering centralized (Enterprise, Standard) and distributed deployments. Alibaba Cloud â PolarDB Product overview â
- Operates nearly 400 PostgreSQL clusters, set Aurora PostgreSQL as the standard relational DB, and automated migration from RDS. CDC connects to Data Mesh through logical replication slots. Netflix Tech Blog â Automating RDS Postgres to Aurora Postgres Migration â
- Agreed to acquire serverless Postgres company Neon in May 2025, and in June announced Lakebase, a fully managed Postgres service built on Neon technology, as an OLTP database for AI apps and agents. Databricks Launches Lakebase â
- Announced the acquisition of open-source Postgres company Crunchy Data in June 2025, and made Snowflake Postgres, which runs Postgres instances on dedicated Snowflake-managed VMs, generally available in February 2026. Feb 24, 2026: Snowflake Postgres (General availability) â
- The low-code platform of Siemens subsidiary Mendix stores the domain model data of apps deployed to Mendix Cloud in a PostgreSQL database. Mendix Docs â Data Storage (Mendix 10) â
- AlloyDB for PostgreSQL, generally available since December 2022, is a fully managed PostgreSQL-compatible DB that lets existing PostgreSQL applications move without changes. By Google's own tests, it says transactions run over 4x faster and analytic queries up to 100x faster than standard PostgreSQL. Announcing the general availability of AlloyDB for PostgreSQL (Google Cloud Blog) â
- Stored all user data in a single master PostgreSQL in London, handling changes for 75 million active users, then switched to Cassandra in May 2015. A service restart during the switch halted sign-ups and logins for a few seconds. Switching user database on a running system â
- Palantir stated that its software and internal tools are built around open-source databases such as Cassandra and Postgres. Palantir and Open-Source Software (Palantir Blog) â
- Named PostgreSQL, along with MariaDB, as the databases used for data analytics, and said open-source platforms and databases on commodity hardware, plus automation, cut operating costs by up to 80%. DBS Bank goes big on open source (Computer Weekly, group CIO interview) â

### Other public cases

- **OpenAI**Runs an Azure PostgreSQL primary server with read replicas worldwide as a core DB for ChatGPT and the API openai.com â
- **BMO Financial Group**Adopted EDB Postgres as the central DB for digital transformation and always-on services enterprisedb.com â
- **Large North American automaker**Migrated core business applications from Oracle to EDB Postgres enterprisedb.com â

## History

1. 1986Michael Stonebraker starts the POSTGRES project at UC Berkeley
2. 1994Postgres95 released with an added SQL interpreter
3. 1996Renamed PostgreSQL; community development model launched
4. 20058.0 released: native Windows support, PITR introduced
5. 20109.0 released: streaming replication and hot standby introduced
6. 20149.4 released: JSONB type and logical decoding introduced
7. 201710 released: logical replication and declarative partitioning introduced
8. 2025Sep: 18 released: asynchronous I/O, skip scan, uuidv7, OAuth authentication
9. 2026Jun: 19 Beta 1: REPACK, SQL/PGQ property graph queries, GROUP BY ALL released
10. 2026Aug: minor releases such as 18.6 fix 28 security vulnerabilities; Sep: 19 Beta 4 released

## Related papers

- A retrospective on the UC Berkeley Postgres project, which ran for about ten years from the mid-1980s.
- Summarizes how design ideas such as abstract data types, the rule system, and the no-overwrite storage manager were implemented and evaluated.
- Also explains how the student-centered development team and the open-source release led into the PostgreSQL community, and the lessons learned.

**Data platform takeaway** Understanding the extensibility philosophy of PostgreSQL and the origins of its open-source governance allows more accurate risk assessment when adopting it as a long-term operational DB standard.

- The original paper presenting the goals and initial design of the POSTGRES system, designed as the successor to INGRES.
- Proposes an extensible relational model as the core design, with user-defined data types, operators, and access methods, complex objects, and a rule system.
- This extensibility design became the foundation of today's PostgreSQL extension ecosystem (pgvector, TimescaleDB, and others).

**Data platform takeaway** The design that lets types, indexes, and functions be attached as extension modules makes possible a strategy of accommodating vector, time-series, and even geospatial data in one operational DB.

## Operating signals

### Development activity Active

- Commits (last 90 days)
- 955
- Active contributors (last 100 commits)
- 23
- Top contributor share
- 16%
- Contributing orgs (profile affiliation)
- https: 42% Â· enterprisedb 35% Â· microsoft 15%

### Releases & issues

- Stable releases (12 months)
- 0
- Open issues
- 0

No stable release in the last 12 months

### Security & license

- GitHub security advisories (12 months)
- 0 all-time 0
- OpenSSF Scorecard
- 6.1 / 10
- License
- â

- No license change since tracking began

Collected from GitHub every Sunday Â· 2026-11-15

## Verdict history

- 2026-10Initial evaluation Â· no verdict change since

## Often used together

Projects the same reference companies also run in production. The number is how many companies have production cases for both; it does not mean they are wired together.

- Apache Hadoop 3 companiesStorage & formats Â· DBS Bank, Netflix, Spotify
- Apache Spark 3 companiesProcessing engines Â· DBS Bank, Netflix, Spotify
- Apache Druid 2 companiesOLAP & query Â· DBS Bank, Netflix
- Apache Flink 2 companiesProcessing engines Â· Netflix, Spotify
- Apache Hive 2 companiesOLAP & query Â· Netflix, Spotify
- Apache Kafka 2 companiesIngestion & streaming Â· Netflix, Spotify
- Elasticsearch 2 companiesVector & search Â· Netflix, Spotify
- Ray 2 companiesProcessing engines Â· Netflix, Spotify

## Sources and links

- Official site postgresql.org
- Official documentation postgresql.org
- GitHub mirror github.com
- 18 release announcement postgresql.org
- postgresql.org postgresql.org
- survey.stackoverflow.co survey.stackoverflow.co
- openai.com openai.com

## Community mentions

Posts about this project in developer communities over the last 90 days

- Hacker News The Internals of PostgreSQL 356pts 11-04 Â· PostgreSQL
- Hacker News PostgreSQL 19 341pts 11-20 Â· PostgreSQL
- Hacker News PostgreSQL Internals [pdf] (2023) 77pts 11-04 Â· PostgreSQL
- Hacker News PostgreSQL 18.7, 17.12, 16.16, 15.20, 14.25, and 19 RC 2 Released 61pts 11-12 Â· PostgreSQL
- Hacker News PostgreSQL 19: A hands-on tour with runnable browser examples 36pts 11-21 Â· PostgreSQL
- Hacker News ZeBridge â PostgreSQL replicas on devices, synchronized over NATS 12pts 11-04 Â· PostgreSQL
- GeekNews PostgreSQL 19 ì£¼ì ë³ê²½ ì¬íê³¼ ì ê·¸ë ì´ë ì£¼ìì  11-20 Â· PostgreSQL
- GeekNews PostgreSQL 19 Released 11-20 Â· PostgreSQL

### Last test: MCP connection Connected

2026-11-18 Â· MCP Toolbox for Databases 1.17.0 (by Google)

29 tools Â· read query OK Â· write statement Ran Â· PostgreSQL 16.16

The write check sends CREATE TABLE to an empty test database. 'Ran' means writes are open with the default settings; it is not a defect. Use a read-only account or read-only setting when you connect an agent.

## **Follow this project** RSS Â· Slack Â· Teams Â· Discord

New releases, license and verdict changes, and high-severity security advisories for PostgreSQL appear in this feed.

- RSS reader
- Add the address above.
- Slack
- Add the RSS app, then send this command in the channel that should receive updates.`/feed subscribe https://stackscape.org/en/p/postgresql/feed.xml`
- Microsoft Teams
- In the Workflows app (Power Automate), create a flow that posts an RSS feed to a channel. The old RSS connector was retired in May 2026.
- Discord
- Connect a bot or automation service that forwards RSS to a channel webhook.

All projects: release news feed Â· change log feed

Verdicts are opinions based on public sources, not adoption advice. GitHub stars and releases update daily (last checked Email the admin.
