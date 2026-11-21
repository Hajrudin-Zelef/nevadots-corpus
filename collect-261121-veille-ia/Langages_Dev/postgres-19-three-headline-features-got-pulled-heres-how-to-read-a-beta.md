---
id: collect-261121-veille-ia/langages-dev/postgres-19-three-headline-features-got-pulled-heres-how-to-read-a-beta
title: "Postgres 19: Three Headline Features Got Pulled. Here’s How to Read a Beta"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["beta", "read", "release", "revert", "postgres", "three", "features", "feature"]
source: https://medium.com/@tl77/postgres-19-three-headline-features-got-pulled-heres-how-to-read-a-beta-47db399432a9
source_anchor: ""
source_lines: [1, 107]
section: "Langages_Dev"
---
# Postgres 19: Three Headline Features Got Pulled. Here’s How to Read a Beta

SQL/PGQ, online checksums and FOR PORTION OF were reverted late. A way to plan database upgrades without trusting beta hype.

**A feature in a beta announcement isn’t a feature yet.** It’s a planning assumption with a nice blog post attached. PostgreSQL 19 just proved it: Beta 4, dated September 24, 2026, came with a list of reverts, and three of them were features the Beta 1 announcement had headlined back in June.

Property graph queries. Online checksum toggling. Temporal updates with `FOR PORTION OF`.

None of this is a scandal. The Beta 4 announcement says it contains “feature previews ahead of general availability” and warns that “some details of the release can change during the beta period.” The project told everyone. Plenty of people read the feature list and skipped the fine print.

If your roadmap has a line that starts with “once we’re on Postgres 19”, this one’s for you. The fix is small: read the reverts narrowly, read the dates as carefully as the features, treat your provider as a second release, and write down what your upgrade actually depends on.

## What actually got reverted (read it narrowly)

The Beta 4 announcement lists these three, word for word:

- “Revert SQL/PGQ (property graph query) support”
- “Revert online enabling and disabling of data checksums”
- “Revert support for temporal updates/deletes provided by the `FOR PORTION OF` clause"

Read those lines exactly as written. The checksum revert is about turning checksums on and off *online*. Postgres didn’t lose data checksums. The temporal revert is about one clause for updates and deletes. The excerpt doesn’t say every temporal feature is gone, so don’t tell your team it is.

And three is only the headline count. The same announcement records a revert of `ALTER TABLE MERGE/SPLIT PARTITIONS` and the removal of `pg_get_role_ddl`, `pg_get_tablespace_ddl` and `pg_get_database_ddl`. Beta 4 alone has five revert lines plus that removal, and earlier betas had their own changes. If you care about a specific feature, find its line in the official text instead of trusting someone's running total.

Why did they go? The release notes don’t say, so this part comes from commentary. Snowflake’s engineering blog attributes most of the big reverts to design flaws found in post-commit review. Dimitri Fontaine’s update says the `FOR PORTION OF` problem was about concurrency at `READ COMMITTED`, which is Postgres's default isolation level. That's the level your app is almost certainly running at right now.

Fontaine wrote this about the partition-merge revert, and I think it applies to all of them:

*“Reverting two years of work a month before a release is not a failure of the process.”*


A revert a month before release feels bad. A concurrency bug in the default isolation level a month *after* release feels worse, and it sticks around longer. I’ll take the project that pulls the feature.

## Read the dates like you read the features

Dates move too, usually without much notice.

The Beta 4 announcement expected a release candidate in early October and said general availability “may also occur in October.” Then RC 1 arrived on October 15, 2026, with a plan: RC 2 on November 12 and GA on November 19, unless an issue forces a delay or another release candidate. So the October possibility became a November plan. At the time of writing, GA is scheduled, not done. Check postgresql.org before you quote a date to anyone.

Snowflake published its own skeptical take on September 16, predicting delays and recommending PostgreSQL 18 as the upgrade target. You’ll see people call that prediction right or wrong. The honest version is that it’s dated commentary, and the final release will settle it. A blog post won’t (including this one).

The release notes are better evidence than any of that, with one catch. A snapshot of the PostgreSQL 19 release notes page carries an “AS OF” date of October 9, 2026, and a release date of `2026-??-??`. That snapshot has no SQL/PGQ or `FOR PORTION OF` entries. That's good corroboration, but it's still a snapshot of a page that keeps changing.

So the habit is boring and it works: write the evidence date next to every claim. “Postgres 19 supports X” is a rumor. “The release notes as of October 9 list X” is evidence. One of those survives a Beta 5.

There’s a second date you should look at, and it has nothing to do with new features. Upstream Postgres supports each major version for five years. The project’s versioning table lists PostgreSQL 14’s final release as November 12, 2026. After that, 14 gets no more fixes upstream. If you’re on 14, you have an upgrade to plan whether or not 19 ships anything you want.

Recheck that table before you act on it. Dates are only true on the day you read them.

## Your provider is a second release

Upstream GA isn’t your GA. If you run managed or serverless Postgres, your provider decides when a major version shows up, in which regions, with which extensions, and with which upgrade path. That’s a separate release with its own schedule.

AWS gives a concrete example. On June 8, 2026, it announced PostgreSQL 19 Beta 1 in the Amazon RDS Database Preview Environment. The conditions matter more than the headline:

- Preview instances are kept for at most 60 days, then deleted automatically.
- Snapshots restore only inside the preview environment.
- It’s for evaluating a prerelease.

That shapes your test plan. Results from a preview are disposable, so record them somewhere that outlives the instance. You can’t snapshot your way from preview into production. And a Beta 1 build from June predates the September reverts. If you tested SQL/PGQ there in July, your test passed on a build that no longer matches the release.

Fontaine’s advice fits here: check the branch you’ll actually run before you depend on a feature ahead of GA. At minimum, write the exact server version into your test notes:

`SELECT version();`
One line, and it stops a whole category of “but it worked in staging” arguments.

One more trap. If you can’t find an announcement from your provider about Postgres 19, that tells you nothing. It doesn’t mean they won’t support it, and it doesn’t mean they do. Check their docs and their version list, and write down the date you checked.

## Write the dependency down before you promise anything

This is the part worth putting in the repo. For every new database capability your plan depends on, keep a short record with five fields: what you need, the evidence and its date, the build you tested, the provider status, and the fallback if it slips.

For Postgres 19 right now, an illustrative version looks like this (a planning template, not a report from a real team):

- SQL/PGQ graph queries. Reverted in Beta 4. Keep delivery independent of it. Whatever joins or recursive queries you run today stay the plan until new evidence shows up.
- Online checksum enabling and disabling. Reverted in Beta 4. Don’t promise anyone an online checksum switch based on a summer preview.
- Temporal updates and deletes with `FOR PORTION OF` . Reverted in Beta 4. Keep your app-level temporal logic. If someone proposes a workaround, test it with concurrent transactions, since that's where the reported problem lived.
- Managed Postgres 19 testing. The June RDS preview had a 60-day limit and preview-only restores. Confirm which build is on offer today before you schedule work.

The routine around it is five steps:

1. Name the exact capability and link the announcement or doc that supports it. Beta wording already tells you it’s provisional.
2. Recheck it against later announcements, the release notes and the build you’ll run. Record the date every time.
3. Record the provider side separately: preview or GA, retention, restore limits, the upgrade path they support.
4. Rehearse the major-version upgrade in staging. The RC 1 announcement names `pg_upgrade` or`pg_dump` /`pg_restore` as routes. Your provider may have its own.
5. Decide: wait for the feature, pick another supported target, or go ahead without it.

Step 5 is where people blur two decisions. DevX recommends using the beta period to test compatibility in staging. Command Prompt and Snowflake lean toward PostgreSQL 18 for upgrade planning.

Those don’t conflict. **Testing 19 is one decision. Running 19 in production is another.**

My take: test 19 now, because finding a compatibility problem in staging costs you an afternoon. Plan production around what’s GA, on your provider, today. For a lot of teams that will point at 18 for a while, and that’s fine. Kinda boring. Boring is the point with databases.

## What to plan on

*A beta tells you what might ship. Release notes tell you what did. Your provider tells you when you get it. Plan on the last one.*


The Postgres project did its job here. It previewed features, found problems and pulled them before GA, and it said in writing that this could happen. The only way this goes badly for you is if your roadmap treated a preview like a promise. Write the dependency down, date the evidence, keep a fallback, and the next revert is a one-line update instead of a rewrite.

## 🐘 More Postgres opinions

None of these depend on a beta feature.

- 🗄️ Why Postgres Is Enough for 90% of Apps — the case for one boring database, before you upgrade it.
- ⚙️ Postgres + Go: Connection Pooling and Timeouts That Actually Work — settings worth rechecking after any major-version move.
- 🏗️ Skip Microservices for Your Mobile Backend — same instinct: don’t build on what you don’t need yet.
