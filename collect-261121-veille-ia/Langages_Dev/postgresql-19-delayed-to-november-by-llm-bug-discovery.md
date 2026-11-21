---
id: collect-261121-veille-ia/langages-dev/postgresql-19-delayed-to-november-by-llm-bug-discovery
title: "PostgreSQL 19 Delayed to November by LLM Bug Discovery"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-11-19"]
keywords: ["october", "postgresql", "release", "november", "year", "major", "september", "beta"]
source: https://afengblog.com/en/blog/postgresql-19-release-delay-llm-bugs.html
source_anchor: ""
source_lines: [1, 166]
section: "Langages_Dev"
---
From PostgreSQL 9.6 in 2016 through PostgreSQL 18 last year, the world's most trusted open-source relational database shipped 10 consecutive major versions without once missing its mid-September to mid-October autumn release window. This year, PostgreSQL 19 broke that streak. After adding two rare extra milestonesâBeta 4 and RC 2âthe project pushed its planned general availability (GA) date to **November 19, 2026**, and reverted 8 to 10 already-merged major features late in the testing cycle.

What slowed the release down is not what most outsiders first guessedâthat developers flooded the codebase with sloppy AI-generated patches. Quite the opposite: security researchers and core maintainers wielding LLMs are finding real bugs far faster than humans can fix them. With verified annual CVEs across supported branches jumping more than fivefold and cross-release back-patches to prior production branches surging **74.6%** in a single year, PostgreSQL's **31** active committers worldwide have collided head-on with the throughput ceiling of human code review and regression verification.

## Breaking a Ten-Year Autumn Streak and Two Rare Extra Rounds

### Three Schedule Retreats Across Autumn

If you run PostgreSQL clusters in production, you have probably grown used to its Swiss-watch annual cadence. Check the official versioning policy table and you will see that all 10 major releases over the past decade landed inside a narrow 35-day windowâas early as September 14 (PG 16) and never later than October 18 (PG 11). Even on the static official developer roadmap, the target under major releases still read "October 2026" well into late October.

The first cracks actually showed up in early summer. Like PG 18 a year earlier, PostgreSQL 19 hit its final CommitFest feature freeze right on time on **April 8 at 12:00 UTC**. Yet while PG 18 shipped Beta 1 on May 8, this year's Beta 1 did not arrive until **June 4**ânearly a full month behind last year's pace from day one.

Once September arrived, release coordinator and Core Team member Jonathan S. Katz posted three scheduling updates to `pgsql-hackers` that trace how the community tried to defend its ten-year autumn window before ultimately yielding to stability:

- **September 3 (Adding Beta 4)** : Katz announced an atypical fourth beta release for September 24 (a normal cycle needs only three betas). He explained frankly that the extra milestone was driven by "handling the influx of issues that have been reported that have taken additional time to handle," citing the`18.6` release notesâwhich patched 28 CVEs in one shotâdirectly in his footnote. At that stage, the team still aimed to reach GA before the end of October.
- **September 28 (Aiming for Late October)** : Four days after Beta 4 shipped, the team outlined a tight sprint scheduleâRC 1 on October 15, no RC 2, and a target GA on October 29 to preserve the usual two-week buffer ahead of the scheduled November minor releases.
- **October 10 (Adding RC 2 and Moving GA to November)** : Just five days before RC 1, the release team stopped trying to force the October deadline and announced a second release candidate, RC 2, for November 12âthe same day as the Q4 scheduled minor releasesâso the large batch of fixes accumulated on the back branches could sit in public testing for a week before shipping PostgreSQL 19 GA on**November 19** . The official RC 1 announcement on October 15 formalized that timeline while keeping the standard caveat: "Unless an issue is discovered that requires a delay or an additional release candidate, PostgreSQL 19 should be made generally available on November 19, 2026."

### Unpacking the 61.5% and 74.6% Git Commit Ledgers

Why did bug fixes on older maintenance branches pile up fast enough to push a major release out of October?

On October 21, veteran committer Robert Haas (now at Databricks) published a blunt postmortem on the Neon blog titled *Postgres 19 is late, and it's AI's fault* (which sparked a second wave of industry debate after he cross-posted it to his personal blog on October 30). Haas pulled commit counts directly from the official Git repository across identical calendar windows in 2025 and 2026, running from the April 8 feature freeze through September 25.

Plenty of secondary coverage blurred "digging up decades-old bugs" and "a 60% jump in back-patches" into a single figure. When I break apart Haas's raw branch numbers side by side, the underlying shift is even starker:

| Metric (April 8 â September 25 Window) | 2025 (PG 18 Cycle) | 2026 (PG 19 Cycle) | YoY Change | Engineering Meaning | 
|---|---|---|---|---|
| **Total commits on `master`** | 1,037 | 1,443 | **+39.15%** | Overall code-change throughput across the repository | 
| **Master-only commits (new development)** | 313 | 274 | **-12.46%** | Feature work targeting the next major cycle actually dipped slightly | 
| **Commits back-patched to in-progress release branch** | 724 ( `REL_18_STABLE` ) | 1,169 ( `REL_19_STABLE` ) | **+61.46%** | Includes v19 post-freeze fixes/reverts plus shared fixes ("more than 60%" metric) | 
| **Commits back-patched to prior production branches** | 248 (into `REL_17_STABLE` ) | 433 (into `REL_18_STABLE` ) | **+74.60%** | True measure of historical bugs affecting live releases (up nearly three-quarters in one year) | 

The ledger leaves little room for ambiguity. Committers wrote 12.5% *fewer* master-only commits for new features this year. What blew out the repository was the 1,169 commits back-patched from `master` into the in-progress stabilization branch (**+61.46%**)âand above all, fixes for pre-existing defects serious enough to require back-patching to at least one already-shipped production release, which jumped **74.60%** in a single year.

### Why Vibe Coding Is Not the Culprit

When engineers hear "AI delayed a database release" alongside "a wave of reverted features," the reflex assumption is obvious: did contributors start "vibe coding" half-baked patches with LLMs and sneaking them into core?

Fellow Core Team member and committer Peter Eisentraut, VP at EDB (and himself the author or committer of several features reverted this cycle), shut down that theory on October 9 in *The reverts will continue until morale improves*.

Most of the reverted features, Eisentraut noted, were written long before LLM-assisted code review became genuinely useful, and committers applied the same review standards at merge time as in previous years ("this situation is not due to 'vibe coding'"). What shifted dramatically was the scrutiny applied afterward: **"the effective quality bar has been raised after the time the feature was written and committed."**

Historically, subtle edge-case interactions in a complex database feature would trickle out over the subsequent **five years** as unusual production workloads hit them in the wild, allowing maintainers to fix them bit by bit. This year, researchers and developers pointed frontier LLMs at the source tree for code review and fuzzingâcompressing five years of long-tail defect discovery into a single summer.

## The 2026 Vulnpocalypse and 20-Year-Old Code Blind Spots

### A Flood of Verified High-Severity CVEs Rather Than AI Slop

Open-source maintainers have spent much of 2026 fighting off "AI slop." Late in January, cURL shut down its HackerOne bug bounty program after AI-fabricated vulnerability reports drove valid submissions below 5%. In late August, a single account dumped 132 unverified AI-generated UBSan reports onto QEMU's bug tracker in under 10 minutes, prompting veteran Red Hat developer Daniel BerrangÃ© to call out the wave as a "DoS attack" against maintainer attention.

Understandably, plenty of commenters on Hacker News initially assumed PostgreSQL was likewise drowning in hallucinated reports. One look at the official PGDG Security Information ledger, however, tells a completely different story. PostgreSQL was not stalled by bogus spam; it was hit by a high-signal "Vulnpocalypse."

While PGDG does not publish how many invalid reports are filtered out behind the private `security@postgresql.org` inbox, the count of real vulnerabilitiesâstrictly reproduced by the Security Team, confirmed against supported branches (PG 14â18), and assigned official CVEsâhas gone vertical:

- **Full year 2023** : 7 CVEs
- **Full year 2024** : 7 CVEs
- **Full year 2025** : 7 CVEs
- **First three scheduled updates of 2026 (through August)** :**44 CVEs** (plus over 235 regular bug fixes, not yet counting the fourth quarterly batch due on November 12)

Three straight years of flat, predictable "7 CVEs per year" exploded to **44** across just the first three quarters of 2026âa **528.6%** jump (6.29Ã the prior annual average). Breaking down the three quarterly batches shows the pressure compounding wave after wave: February 12 (`18.2` batch) fixed 5 CVEs; May 14 (`18.4` batch) climbed to 11; and on August 13 (the `18.6` batch), PGDG shipped fixes for a record **28 CVEs** alongside more than 110 regular bugs in a single day. Roughly half of those August advisories were CVSS 8.8 high-severity flaws involving heap buffer overflows, integer overflows, type confusion, or arbitrary code execution.

### AI Security Squads in the Release Credits and 11-Way Bug Collisions

Reading the acknowledgments across this year's three security advisories and the official `18.6` release notes feels like scanning a live leaderboard of frontier models and autonomous auditing engines:

- **Claude & Anthropic Research** : Credited alongside Calif.io in May for an`hstore` binary-input stack overflow (`CVE-2026-6479` ), then credited three more times in August with researcher Ben Morris for high-severity flaws including an`ltree` heap buffer overflow (`CVE-2026-16239` ), superuser SQL injection via`EXTRACT()` expression deparsing (`CVE-2026-15741` ), and role-list deparse SQL injection (`CVE-2026-15742` ).
- **OpenAI Codex Security** : Amy Burnett and the OpenAI Security Research Team earned back-to-back credits in`18.6` for a`tsquery` out-of-bounds read and stack overflow (`CVE-2026-14669` ), a`btree_gist` heap buffer overflow (`CVE-2026-16238` ), and arbitrary memory reads during planner statistics computation (`CVE-2026-14680` ).
- **Specialized Autonomous Auditing Engines Worldwide** : Automated code-auditing team**Xint Code** (5 CVEs across February and May), cybersecurity startup**AISLE** (whose autonomous system earlier swept all 12 of 12 CVEs in a single OpenSSL release),**Tencent Xuanwu Lab's XlabAI Team with the Atuin Automated Vulnerability Discovery Engine** (`CVE-2026-6475` ), Ant Group's**AntAISecurityLab** (with Fudan University), and**TrendAI ZDI** all appear in the official credits.

Pointing top-tier models from security teams around the globe at the same 30-to-40-year-old C codebase produced two unmistakable signatures of this era.

The first is **unearthing 20-year-old dormant bugs**. Take `CVE-2026-2005` (CVSS 8.8, allowing a ciphertext provider to execute arbitrary code as the database OS user), discovered via AI-assisted auditing by Xint Code and patched in February. Tracing the Git and CVS history reveals that the vulnerable heap buffer overflow entered PostgreSQL back in **2005** with early `pgcrypto` C code. It sat silently in production deployments for more than two decadesâsurviving generations of manual audits and traditional fuzzersâuntil an LLM spotted it (during the same period, Xint Code also uncovered `CVE-2026-21576` in MariaDB, a stack buffer overflow introduced in 2004 that had lurked for 22 years).

The second is **double-digit researcher collisions on the exact same bug**. In traditional vulnerability research, two independent teams reporting the same deep flaw within a single month is already a notable coincidence. Yet the May advisory for `CVE-2026-6473` (arbitrary code execution via type confusion in `jsonb_populate_recordset`) credited an extraordinary **10 independent reporting groups**; by August, `CVE-2026-14669` pushed that record to **11 co-credited reporters**, including both OpenAI Codex Security and AntAISecurityLab. Frontier models have not only lowered the cost of bug discoveryâthey are causing independent teams following similar reasoning chains to converge en masse on the exact same historical blind spots.

## Thirty-One Human Gatekeepers at Capacity and Three Minor-Release Regressions

### Why AI Cannot Simply Patch the Bugs AI Finds

If LLMs are this effective at finding deep C bugs, why not let them generate the patches and run code review too?

The answer lies in the wide gulf between probabilistic patch generation and commit-level accountability in database internals. According to PGDG's official committers roster, the core PostgreSQL repository powering millions of enterprise databases worldwide has just **31 active committers** with direct Git push access.

Their hats overlap heavily. **Twenty** of those 31 committers also sit on the PostgreSQL Security Team, quietly verifying private vulnerability reports under embargo while hand-crafting and adapting back-patches across five supported major branches (PG 14, 15, 16, 17, and 18). Meanwhile, the Release Management Team (RMT) coordinating PG 19 quality consists of just **three people**âNathan Bossart, Heikki Linnakangas, and Melanie Plagemanâwith the first two simultaneously on the front lines writing fixes for multiple high-severity CVEs this year.

Robert Haas summed up the bottleneck with uncomfortable honesty: **"LLMs can draft fixes, but those fixes arenât always correct or compliant with PostgreSQL standards. So, they still need to be reviewed and committed by humans, and those humans have not gotten any smarter between 2025 and 2026."**

PGX principal consultant Christophe Pettus documented a textbook example in *19th Nervous Breakdown*. During beta testing, veteran contributor Thom Brown used Claude to pinpoint a genuine cluster-crashing defect in the new `REPACK` feature under logical decoding, and Claude helpfully suggested two one-line fixes. When patch author Antonin Houska reviewed them, one of the AI's two one-liners turned out to be wrongâmerging it directly would have corrupted underlying state.

Even when a fix genuinely takes only a handful of lines, the engineering "fixed overhead" refuses to shrink. As Peter Eisentraut calculated, once "youâre staring at a list of like forty defect reports," even if each averages just a **three-line fix**, reproducing the minimal test case, debating boundary effects on `pgsql-hackers`, writing regression tests, manually resolving back-patch conflicts across five or six branches, and watching cross-platform Buildfarm runs still burns irreplaceable cognitive bandwidth from the same 31 people.

### Three Close Calls in Six Months Under Heavy Back-Patching

Worse still, pushing hundreds of high-pressure security and bug fixes across five live production branches took a visible toll on PostgreSQL's famously rock-solid minor releases during the first eight months of 2026:

1. **February's Out-of-Cycle Emergency Release (`18.3`)** : Just four days after shipping the February 12`18.2` update (with 5 CVE fixes), PGDG had to announce an unscheduled emergency release because a security patch introduced a severe`substring()` regression on multibyte text, rushing`18.3 / 17.9 / 16.13 / 15.17 / 14.22` out the door on February 26.
2. **May's Standby WAL Replay Self-Deadlock** : The May 14 minor releases (`16.14 / 15.18 / 14.23` , carrying 11 CVE fixes) inadvertently introduced a self-deadlock regression when PG 14â16 standbys replayed WAL from older primariesâan issue that lingered until the August update.
3. **August's Rare Abandonment of `18.5`** : In mid-August, after wrapping`18.5` packages but just before public release, maintainers caught yet another regression at the eleventh hour, forcing PGDG to scrap`18.5` outright and skip straight to`18.6` on August 13.

Connect those three scares across the first half of the year and the release team's October 10 decision makes complete engineering sense. With another heavy batch of back-branch fixes slated to land on November 12 alongside the Q4 minor updates, breaking a ten-year autumn streak to bake those changes in an extra RC 2 for a week was vastly preferable to letting unvetted low-level churn slip straight into PostgreSQL 19.0 GA.

## The Claude-Driven Scary Patch Contest and Late-Stage Feature Triage

### The Mailing List Post That Sparked the Revert Wave

The April-to-August Vulnpocalypse did more than stress old branches; it drained the prime summer months senior committers normally spend shaking out new PostgreSQL 19 features. By the time maintainers caught their breath after the 28-CVE `18.6` release in mid-August and turned back to PG 19 ahead of the release-candidate window, almost no runway remained.

Fittingly, core maintainers picked up LLMs as a scalpel themselves. On August 25, Robert Haas kicked off a `pgsql-hackers` thread titled `scary patch contest`. He asked Anthropic's Claude to audit every v19 feature based on the count and severity of post-freeze bug fixesâjoking that he shared the output "with a few particularly cutting remarks from the LLM edited out"âand spotlighted six high-risk patch sets:

- **`REPACK / REPACK CONCURRENTLY`** : 28 post-freeze fixes, including data-loss hazards;
- **Online enabling/disabling of data checksums** : ~25 substantive fixes, mostly state-machine holes, plus false positives where "A corruption-detection feature producing false positives is exactly the wrong failure mode";
- **`UPDATE / DELETE ... FOR PORTION OF` temporal DML** : 17 fixes, three of them security flaws bypassing row-level security (RLS) or permission checks;
- **`SQL/PGQ` property graph queries** : 17 fixes;
- **RI fast-path foreign-key checks and batching** : ~16 fixes, including an out-of-bounds write and five distinct classes of broken constraint enforcement;
- **`postgres_fdw` statistics import** : committed right on freeze day, yet a single-file contrib change still required 7 follow-up fixes.

At the same time, committer Noah Misch was scanning `master` with Claude Opus 5, and longtime core veteran Tom Lane openly noted in an August 28 query-planner commit message that its initial draft came from Claude Opus 4.8âeven while exercising characteristic restraint by rejecting external bulk AI-generated test suites whose permanent runtime cost outweighed their bug-catching value.

### Cutting Eight to Ten Major Features and What Survived in RC 1

In a typical PostgreSQL cycle, perhaps one feature gets reverted after Beta begins. Over three intense weeks from late August through mid-September, however, committers carried out an unprecedented late-stage triage, pulling **8 to 10 major features** depending on how you group them (according to Snowflake's engineering team, the branch accumulated 53 revert commits between the start of Beta and mid-September):

- **`SQL/PGQ` Property Graph Queries (reverted September 7)** : The cycle's largest headline feature; a single revert commit deleted**124 files and roughly 16,000 lines of code** . Its fatal flaw was missing locking around dependent object drops, and fixing it properly required changing system catalogs (`catversion` ). Tom Lane remarked during the discussion that he was willing to bet dinner that forcing it out in v19 would leave unfixable catalog bugs until v20.
- **Online Data Checksums (reverted September 16)** : Claude-assisted review uncovered that orphaned relation files left behind by historical cluster crashes caused`pg_basebackup` checksum verification failures with no safe short-term fix, taking 30 related commits out with it.
- **`UPDATE / DELETE ... FOR PORTION OF` (reverted September 15)** : Pulled across 23 commits due to concurrent lost-update executor bugs under`READ COMMITTED` isolation alongside 14 unresolved open items (PG 18's existing`WITHOUT OVERLAPS` constraints remain unaffected).
- **Other Reverted or Scoped-Back Features** : These included`ALTER TABLE ... MERGE / SPLIT PARTITIONS` , cluster-level DDL generation functions like`pg_get_role_ddl()` (reverting 12 follow-up commits),`GROUP BY ALL` (which clashed with non-default equality semantics), non-plain-text`pg_dumpall` output formats, collation provider-specific casefolding, and`CREATE SCHEMA` subcommand extensions. One common community misconception is worth clearing up:**faster foreign-key checks were not reverted across the board** . On September 10, committer Amit Langote removed only the RI batching layerâwhich interacted poorly with subtransactionsâwhile keeping the**per-row fast path** intact in RC 1.

In fairness, external LLMs do not bear 100% of the blame for this revert wave. After auditing Git timestamps, Christophe Pettus and Layerbase highlighted the other side of the coin: during the final five weeks before the April 8 "freeze cliff" (March 10 to April 8), committers sprinted to merge eight large features deeply entangled with low-level internalsâ`REPACK (CONCURRENTLY)` even landed 40 hours before freeze carrying three known design loose ends in its commit message. Put simply, **the pre-freeze merge rush loaded high architectural complexity onto the branch, while the LLM vulnerability storm simultaneously drained early-summer review bandwidth externally and accelerated design-flaw discovery internallyâshattering a summer beta calendar calibrated for an older era.**

The silver lining is that after this ruthless surgical triage, the features surviving in the official PostgreSQL 19 RC 1 release notes remain remarkably strong:

- **Built-in `REPACK` and Non-Blocking `REPACK (CONCURRENTLY)`** : Survived scrutiny by deliberately narrowing its blast radiusârestricting execution to one backend cluster-wide at a time, heap tables only, and disallowing`ANALYZE` inside transaction blocksâgiving DBAs a clear native path beyond the third-party`pg_repack` extension;
- **Official Plan-Control Extensions `pg_plan_advice` and `pg_stash_advice`** : Fine-grained control over join orders, scan methods, and parallelism strategies without rewriting application SQL;
- **Replication and High-Availability Upgrades** : Logical replication of sequences, enabling logical replication under`wal_level = replica` without a server restart, and a native`WAIT` command on standbys for read-your-writes consistency;
- **Operations and Performance Gains** : Parallel autovacuum paired with a new priority-scoring scheduler, quicker per-row foreign-key checks,`INSERT ... ON CONFLICT DO SELECT` ,`COPY TO (FORMAT json)` , and two pragmatic default configuration changesâturning`jit` to`off` by default and switching`default_toast_compression` to`lz4` whenever compiled with LZ4 support.

## Redefining Ready to Ship When AI Can Always Find Another Bug

PostgreSQL 19's three-week slip is almost certainly just the opening thunderclap for large systems software entering the AI-auditing era.

Inside the PostgreSQL community, several concrete process reforms are already on the table to avoid a repeat next year. RMT member Nathan Bossart proposed piloting **6 to 8 scheduled minor releases per year** (every 6â8 weeks instead of quarterly) starting in 2027, spreading back-patch risk across smaller batches rather than absorbing dozens of CVEs at once. Peter Eisentraut called for moving **LLM-assisted fuzzing upstream before features are committed**âbecause PostgreSQL prizes an "everything works with everything" orthogonality (domains inside composite types, out-of-order partition attachment stacked on security-definer triggers) whose combinatorial explosion no human reviewer can exhaustively traceâwhile exploring an in-tree **"experimental status"** tier so complex features no longer face an all-or-nothing five-year compatibility bet at the April 8 freeze cliff.

Looking across open-source infrastructure, defensive AI engineering is already showing early promise in easing maintainer bottlenecks. As Phoronix reported in early October, **Sashiko**, an AI code-review system built by Google engineers for the Linux kernel community, has reviewed over **191,000 kernel patches** and been cited across fixes associated with **463 Linux kernel CVEs**, cutting initial patch-review time by 60% to 70% for some subsystem maintainers.

Harder than any tooling or calendar tweak, though, is the question Robert Haas left hanging at the end of his post: **"Given that we have forty years of bugs that humans were unable to find but LLMs are quite capable of discovering, how are we ever going to catch up, and how will PostgreSQL continue to innovate? Right now, Iâm not sure."**

For decades, shipping systems software on schedule relied on a natural equilibrium: once a release had zero known showstopper defects, it was "good enough to ship," and remaining long-tail edge cases were left to surface gradually over years of production use. Once machines gain near-infinite compute to probe every corner branch of a forty-year codebase, defect discovery decouples permanently from human repair bandwidth. Across Hacker News and engineering circles, systems architects are already warning about **"release paralysis"**âwhen an AI auditor can pull ten more genuine, never-triggered edge-case bugs out of legacy code every single day, who makes the call that it is safe to stop patching and cut the release?

There is no tidy formula for that dilemma yet. Still, in the autumn of 2026, those 31 human database gatekeepers who "have not gotten any smarter between 2025 and 2026" made the plainest and most reassuring call they could: refuse to ship half-baked features, refuse to worship a ten-year calendar streak for its own sake, take the extra rounds, and protect the integrity of user data first.

## References and Primary Sources

1. PostgreSQL Global Development Group â *PostgreSQL 19 RC 1 Released!* (Confirming Nov 12 RC 2 and Nov 19 planned GA, 2026-10-15)
2. Jonathan S. Katz (`pgsql-hackers` ) â*Updated PostgreSQL RC / GA dates* (Mailing list archive of schedule updates, 2026-10-10)
3. Robert Haas (Neon Blog) â *Postgres 19 is late, and it's AI's fault â LLMs are finding bugs faster than committers can fix them* (Original >60% / >74% Git branch commit analysis, 2026-10-21)
4. Peter Eisentraut â *The reverts will continue until morale improves* (Analysis of 8â10 feature reverts and the post-commit raised quality bar, 2026-10-09)
5. Robert Haas (`pgsql-hackers` ) â*scary patch contest* (Original email auditing the six highest-risk post-freeze patch sets with Claude, 2026-08-25)
6. PostgreSQL Global Development Group â *PostgreSQL 18.6, 17.11, 16.15, 15.19, 14.24 and 19 Beta 3 Released!* (28 CVEs patched in a single release and skipped 18.5 note, 2026-08-13)
7. PostgreSQL Documentation â *Appendix E.1. Release 18.6* &*Security Information* (Official security ledger and AI security team acknowledgments)
8. PostgreSQL Security Advisory â *CVE-2026-2005 â pgcrypto heap buffer overflow* & CSO Online coverage (Verification of the 20-year-old 2005 vulnerability)
9. PostgreSQL Global Development Group â *PostgreSQL 19 Beta 4 Released!* &*Release 19 Documentation* (Official list of feature reverts and retained RC 1 features)
10. Christophe Pettus (The Build) â *19th Nervous Breakdown* (Feature-freeze cliff postmortem and 2027 minor-release cadence proposal, 2026-09-15)
11. Layerbase â *When Will Postgres 19 Be Released?* (Upstream revert commit hashes and release timeline verification, 2026-10-15)
12. LWN.net â *PostgreSQL 19's "scary patch contest"* &*Haas: Postgres 19 is late, and it's AI's fault* (2026-09-10 / 2026-10-30)
13. Phoronix â *AI Code Review System Has Reviewed 191k+ Linux Patches, Cited On 463 Kernel CVEs* &*AI-Generated Bug Reports "DoS Attack"* (2026-08-25 / 2026-10-08)
