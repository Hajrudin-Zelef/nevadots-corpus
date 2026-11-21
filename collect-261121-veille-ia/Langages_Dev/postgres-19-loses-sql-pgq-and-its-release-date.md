---
id: collect-261121-veille-ia/langages-dev/postgres-19-loses-sql-pgq-and-its-release-date
title: "Postgres 19 loses SQL/PGQ (and its release date)"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-09-09"]
keywords: ["postgres", "data", "release", "date", "table", "thread", "more", "paradedb"]
source: https://postgresweekly.com/issues/664
source_anchor: ""
source_lines: [1, 39]
section: "Langages_Dev"
---
| #664 — September 9, 2026 | Web Version | 

|  | 

| Together with |  | 

| Postgres Weekly | 

| The *Scary* Postgres 19 Patch Contest — It’s not even Halloween yet, but in this fascinating thread on the`pgsql-hackers` list Robert Haas set out with Claude to*“evaluate which v19 patches were the scariest”* . One of them, SQL/PGQ property graphs, has since been dropped from v19, but five remain… Robert Haas et al. | 

| 💡 There's some more background to SQL/PGQ's removal in a separate thread on `pgsql-hackers` too. | 

| Coming Soon: ParadeDB Cloud — We're building a fully managed ParadeDB: one Postgres for your application data, full-text search, vector retrieval, and aggregations. Our early access waitlist is open now. ParadeDB | 

| **POSTGRES 19: A FOURTH BETA AND SOME HARD CHOICES:** Originally expected later this month, Postgres 19's release date is now officially TBD, with a fourth beta expected on September 24. But the 'scary patch' thread *(above)* shows that what's going to be in it remains an open question:  | 

| How to Optimize When You Can’t Do Anything — Hettie had a production query on a 750GB, 16 billion row table that suddenly ran slow. Creating the right index wasn’t an option (it’d take over a day) but a close look at the underlying data led to an ingenious fix. Henrietta Dombrovskaya | 

| **IN BRIEF:**  | 

| Sixteen Locks Ought to Be Enough for Anybody — Did you know that, when planning a query, Postgres takes a (weak) lock on every index on the table whether it uses them or not? This is usually fine, but *until Postgres 18* this could sting once a query touched more than 16 tables and indexes combined. Christophe Pettus | 

| 📄 Inserting State Transitions in Postgres – An append-only status table gives you full history, but two concurrent transitions can both insert. Here's how to prevent the conflicts. Thiago Araújo Silva (thoughtbot) 📄 From the Trenches: My Path Through Postgres – Twenty years of Postgres war stories, from a one-time MySQL advocate. Shaun Thomas 📄 Fix Circular Role Dependencies Before Upgrading RDS and Aurora Postgres Ravi Teja Adabala (Amazon) 📄 Postgres Calculations and the Ambiguity of `NULL` Christopher Winslett (Crunchy Data) | 

|  | 

| 📰 Classifieds | 

| 🧊 Paying SSD prices for data nobody reads? ColdFront tiers old Postgres rows to S3 automatically, the open source way. 🎟️ Join us at Supabase Select on October 2. Get first-hand information about what's coming soon in Supabase. Apply to attend with 25% off: SUPAWEEKLY. | 

| **RELEASES AND CODE:** | 

| Kafgres: A Kafka-Compatible Broker Embedded Inside Postgres — A pgrx-powered extension that serves the Kafka wire protocol from a Postgres background worker, so standard Kafka clients can produce, consume, and rebalance consumer groups against topics stored in your database. Raynor Elgie | 

| chdb: An Extension for Fast Imports from S3, GCS and Azure — ClickHouse’s chDB engine, wrapped as an extension for bulk imports and exports. `COPY` (or`CREATE TABLE` ) can pull Parquet, Avro, ORC, Arrow and more from S3, GCS, Azure or over HTTP. Early days, but promising. David Wheeler (ClickHouse) | 

|  | 

| **Got a new email?** You can change your address here. **Bored of us?** Click here to stop getting Postgres Weekly. **Got a link for us?** Reply and tell us. We can't include everything but we'll look at anything you send.*Thanks!* Published by Cooper Press Ltd. Fairfield Enterprise Centre, Louth, LN11 0LS, United Kingdom Postgres, PostgreSQL and the Slonik Logo are trademarks or registered trademarks of the PostgreSQL Community Association of Canada. Stop getting this newsletter. |
