---
id: collect-261121-veille-ia/langages-dev/postgres-19-proves-it-ai-finds-bugs-faster-than-we-fix-them
title: "Postgres 19 Proves It: AI Finds Bugs Faster Than We Fix Them"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["code", "bugs", "postgresql", "october", "features", "human", "them", "review"]
source: https://dev.to/max_quimby/postgres-19-proves-it-ai-finds-bugs-faster-than-we-fix-them-3d6e
source_anchor: ""
source_lines: [1, 101]
section: "Langages_Dev"
---
PostgreSQL 19 was supposed to ship in October. Instead, the release slipped to November 19, three major features were reverted, and the project's back-patch commit rate jumped 74% year over year. The cause? LLM-assisted code review surfaced bugs in the decades-old codebase faster than the project's human committers could triage and fix them. As PostgreSQL committer Robert Haas wrote in his October blog post, the humans reviewing this code have not gotten any smarter between 2025 and 2026.

This is not a story about AI breaking things. It is the clearest evidence yet that the bottleneck in software engineering has shifted — from writing code to verifying it.

The story hit the first Hacker News thread on October 16, via The Register's coverage. Two weeks later, Haas's own blog post landed a second thread with 785 points — one of the highest-engagement programming stories of the month.

The PostgreSQL Global Development Group froze features for version 19 on April 8, 2026. What followed was supposed to be a routine beta-to-release cycle. Instead, between August 27 and September 15, three significant features were pulled: `UPDATE/DELETE FOR PORTION OF`, SQL Property Graph Queries (SQL/PGQ), and `ALTER TABLE MERGE/SPLIT PARTITION`. In total, roughly eight to ten major features were reverted during the beta window.

The numbers tell the story. Between feature freeze and September 25:

- Master branch commits rose from 1,037 (2025) to 1,443 (2026)
- Back-patches to REL_19_STABLE hit 1,169, up from 724 the prior year
- Patches to older release branches jumped from 248 to 433 — a **74% increase**

Haas argues the codebase did not get buggier. Committers were just as careful in 2026 as in prior years. What changed is that LLM-assisted review made it far easier for people outside the core team to find and report bugs — many of them lurking in code written years before these tools existed. Neon's detailed analysis of the commit statistics confirms the pattern: master-only commits actually declined slightly, while the explosion was entirely in back-patches — fixes to bugs that already existed in shipped code.

ℹ️ PostgreSQL 19 RC2 is scheduled for November 12, with GA planned for November 19, 2026. The same day, PostgreSQL 14 reaches end of life. The official schedule update was posted on the pgsql-hackers mailing list on October 10.


Here is the core tension: LLMs can surface defects at machine speed, but fixing those defects still requires human judgment. Each bug report needs a committer to understand the context, evaluate the severity, decide whether to fix or revert, write or review a patch, and verify it does not break something else. LLMs can draft fixes, but as Haas notes, those fixes are not always correct or compliant with PostgreSQL standards.

This is the **verification bottleneck**. Generation — whether of code, bug reports, or patches — has become cheap and fast. Verification — the process of confirming that something is correct, safe, and ready to ship — remains expensive and slow, because it depends on human expertise that does not scale linearly.

The Postgres case is the purest example because the AI contribution was not code generation. Nobody vibe-coded SQL/PGQ into existence. These features were designed, implemented, and reviewed by experienced human committers using traditional methods. The LLMs came in as reviewers, not authors. They found the bugs that decades of human review had missed. And that flood of bug reports is what broke the release schedule.


The verification bottleneck is hitting every layer of the software ecosystem.

**Open-source projects are drowning.** A Linux Foundation survey of 234 maintainers (October 2026) found that on projects with more than 50 contributors, maintainers estimate close to a third of incoming code changes are low quality or AI-generated. Burnout has overtaken code quality as the top concern for large-project maintainers. About a quarter of projects have no checks in place for AI-assisted code at all.

GitHub shipped repository-level PR controls in February 2026 after the volume became unmanageable. Maintainers can now disable pull requests entirely, limit them to existing collaborators, or cap how many external PRs stay open. Several high-profile projects have gone further: tldraw auto-closes all external PRs, Ghostty allows AI-generated PRs only for accepted issues, and Sindre Sorhus disabled external PRs across all his repositories.

**Enterprise teams face the same wall.** Qodo's 2026 State of AI Code Quality report — surveying 500 developers and 300 engineering leaders — found that both groups named reviewing and validating AI-generated code as their **top delivery bottleneck**, at 26% each. That beat trust in AI output, security and compliance, and cost. Meanwhile, 36% of developers say reviews take the same time they always have, but demand more cognitive effort. AI-authored PRs look polished — clean diffs, proper formatting — but the reviewer cannot tell which alternatives were considered or which assumptions carried through.

As one Signadot analysis put it: the cost of submitting a pull request has basically hit zero. The cost of reviewing one has not moved at all.


⚠️ **Contrarian Corner:** The "AI found too many bugs" framing flatters the tools. Many of these bugs would have been caught by better static analysis or more reviewer time years ago. The real constraint has always been that PostgreSQL — like most large open-source projects — has far fewer committers than its codebase demands. AI did not create a new bottleneck; it made the existing staffing gap undeniable. The question is whether projects will respond by investing in more committer capacity or by building verification automation that does not need it.


The data is converging from multiple directions:

**CircleCI's 2026 State of Software Delivery** identified a growing disconnect: teams produce more code but ship less of it. The pipeline is wider at the input, narrower at the output.

**Qodo's survey** found that 89% of organizations have experienced an AI-related production incident, and only 3.7% of engineering leaders say their current processes are sufficient for quality and governance. Fewer than half have centralized AI coding standards or visibility into AI-related code quality trends.

**The Linux Foundation study** found that AI users report coding time cut roughly in half and review time cut by more than 40% — but that productivity gain accrues to individuals, not to the project. The maintainers still face the same queue, now fed by more and faster contributors.

The Aviator engineering blog captured the paradox: teams ship more code than ever, yet review queues grow and cycle times slow down.

ℹ️ **The Haas Statistic:** Master-only commits on the PostgreSQL 19 branch actually declined slightly year over year. The explosion was in back-patches — fixes to bugs that already existed in shipped code. AI review did not find new bugs in new code. It found old bugs in old code, at a rate humans never could.


The Hacker News thread on Haas's post hit 785 points and 463 comments — putting it on the front page for most of a day. The community reaction was notably split.

One camp sees the delay as genuinely positive. The top comment argues that repaying technical debt is always a great thing. Several practitioners shared similar experiences — using AI review on their own codebases and finding bugs that had survived years of manual review.

The skeptical camp questions whether AI review is surfacing real, priority bugs or creating a flood of low-severity findings that consume committer time without proportional benefit. One commenter noted that the bugs found are not the kind you can just press a button on the LLM to fix — they require the reviewer to deeply consider design tradeoffs.

This split maps exactly onto the bottleneck thesis. Both camps agree that more bugs are being found. They disagree on whether the human verification step can scale to handle them.


If you run an engineering team, the Postgres 19 story has three immediate implications:

**1. Stop measuring AI productivity by generation output.** Lines of code, PRs opened, and tickets closed are input metrics. They tell you how much work entered the pipeline, not how much left it verified and deployed. Start tracking review queue depth, time-to-merge, and escaped defect rate. If your team generates 2x the PRs but review time doubles, you are running faster on a treadmill.

**2. Invest in verification automation, not more reviewers.** The Postgres team's dilemma is instructive: you cannot hire your way out of a bottleneck that grows faster than your headcount. The fix is shifting verification from humans reading diffs to machines checking invariants:

- Property-based testing that generates edge cases automatically
- Formal specifications where the cost is justified (Dave Farley's BDD-style executable specs approach)
- Shadow traffic that catches behavioral regressions before review
- AI reviewers as a first pass — not to replace human review, but to reduce what humans need to look at

**3. Accept that the review bar has permanently risen.** Every feature your team writes will face AI scrutiny, whether from your own tools or from users running LLM analysis on your codebase. As Haas put it: every feature that we write from now until the end of our careers will be scrutinized by AI. The bugs that used to stay hidden for years will surface in hours. Your review process needs to be ready for that volume.

💡 **The practical test:** Run an LLM-assisted review on your own codebase today. If it surfaces more bugs than your team can triage in a sprint, you are already in the verification bottleneck. Better to discover that on your terms than when a customer or a contributor does it for you.


PostgreSQL 19 is a microcosm of a shift happening across all of software engineering. For fifty years, the scarce resource was code generation — getting working software written at all. Tools evolved to make writing code faster: IDEs, frameworks, libraries, and now AI coding assistants.

But the constraint was never really about writing speed. It was about the speed of the entire pipeline, and for most of that history, writing was the slowest stage. Remove that bottleneck, and the constraint moves downstream — to review, testing, integration, and deployment.

This is the Theory of Constraints applied to software delivery. Improving the non-bottleneck stage (writing) does not improve throughput. It just moves inventory — unreviewed PRs, untested patches, undeployed features — into the queue ahead of the actual bottleneck.

The teams that will thrive in this environment are the ones that recognize the shift and invest accordingly. Not in more code generation tools, but in verification infrastructure: automated testing, formal methods, continuous deployment pipelines, and AI-assisted review that reduces what humans need to evaluate.

The Postgres project, to its credit, chose quality over schedule. That is the right call. But the long-term question Haas raises is harder: the project has decades of accumulated bugs that LLMs can now surface, and it is unclear how the project will both catch up on that backlog and keep innovating.

That question does not have a tool-shaped answer. It has an organizational one.

*The PostgreSQL 19 general availability release is planned for November 19, 2026. Robert Haas's full analysis is at rhaas.blogspot.com. The HN discussion has 785 points and 463 comments.*

**Related reading:**

- Anthropic's Agents Broke Git at 30K Merges a Day — what happens when agent velocity overwhelms existing infrastructure
- How a 5-Person Startup Beats Teams of 25 With AI Agents — the productivity upside, and the hidden review cost
- Anthropic's Doom Warning Is Really a Tooling Gap — why capability outpacing infrastructure is the real risk

*Originally published at ComputeLeap*

## Top comments (0)
