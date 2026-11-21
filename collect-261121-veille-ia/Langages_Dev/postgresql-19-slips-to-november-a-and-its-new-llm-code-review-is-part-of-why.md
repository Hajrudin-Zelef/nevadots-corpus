---
id: collect-261121-veille-ia/langages-dev/postgresql-19-slips-to-november-a-and-its-new-llm-code-review-is-part-of-why
title: "PostgreSQL 19 slips to November â and its new LLM code review is part of why"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["postgresql", "review", "november", "code", "part", "slips", "beta", "october"]
source: https://stacksignal.fdhcl.com/postgresql-19-delay-llm-code-review/
source_anchor: ""
source_lines: [1, 17]
section: "Langages_Dev"
---
# PostgreSQL 19 slips to November â and its new LLM code review is part of why

PostgreSQL 19 was supposed to ship in late September or early October. Instead, the famously cautious project is still in its release-candidate tunnel: the first RC landed on October 15, a second is planned for November 12, and general availability is now expected around November 19.

The delay has a distinctly 2026 flavor. To protect the schedule, the community cut features â an unusually large number of them. Snowflake's engineering team counted 53 reverts since the beta period began, versus about 44 across PostgreSQL 18's entire cycle. Several casualties were headline features.

The biggest is SQL/PGQ, the property-graph syntax for exploring nodes and edges that became part of the SQL standard in 2023. Also pulled: toggling data checksums online, temporal UPDATE/DELETE via FOR PORTION OF, and ALTER TABLE ... MERGE/SPLIT PARTITIONS. GROUP BY ALL went too, after a post-commit review found it could return wrong results when ORDER BY used non-default equality semantics. The revert message was blunt: try again in v20.

The Beta 4 announcement, on September 24, explained the project's thinking in two sentences: first and foremost, PostgreSQL must be reliable â and the community strives for a predictable release schedule. Longtime contributor Tom Lane was blunter about SQL/PGQ: "At this point I'd be willing to bet dinner that if we ship it in v19 there will be post-release bug discoveries that are unfixable until v20."

The fullest account of why so many features missed this cycle comes from core team member Peter Eisentraut, in an October 9 blog post titled "The reverts will continue until morale improves." A few reverts were architectural, he writes â defects too big to fix "quickly and during beta." But the long tail is new: LLM-assisted code review now surfaces the edge cases that used to trickle out as bug fixes across the five years after a release. "You're staring at a list of like forty defect reports. And even if each of them requires only a three-line fix on average, there is a fixed overhead to dealing with each of these, and then you run out of time." LLM-assisted fuzzing added a pile of vulnerability reports that ate the beta window. And he is explicit about what this is not: the reverted code predates the LLM era. Not vibe coding.

None of this makes 19 a thin release. REPACK â and its CONCURRENTLY variant, which keeps reads and writes flowing through most of the rewrite â is the built-in answer to table bloat. pg_plan_advice and its companion pg_stash_advice let you pin a good plan instead of chasing regressions. Parallel autovacuum with a priority-scoring system, sequence values over logical replication, ON CONFLICT DO SELECT, and IGNORE NULLS in window functions all survived.

The community's upgrade advice is boring and good: keep production on 18 until 19 goes GA around mid-November, and grep your migrations for the reverted syntax â GRAPH_TABLE, FOR PORTION OF, MERGE/SPLIT PARTITION, bare GROUP BY ALL â before planning the jump.

The genuinely interesting part is the quality bar. In Eisentraut's phrase, LLM-assisted review has "raised the effective quality bar after the time the feature was written and committed." PostgreSQL treated that as a reason to slow down, not speed up. The tools that find problems can arrive faster than the process that fixes them â and this time, the most cautious project in the industry chose the process.
