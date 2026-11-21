---
id: collect-261121-veille-ia/muse/meta-s-muse-ember-haiku-level-intelligence-at-a-third-of-the-cost-what-it-means-for-agent-builders
title: "Meta's Muse Ember: Haiku-Level Intelligence at a Third of the Cost — What It Means for Agent Builders"
domain: muse
role: reference
task: article
actors: ["Meta"]
dates: ["2026-11-09"]
keywords: ["ember", "meta", "muse", "model", "agent", "intelligence", "tokens", "haiku"]
source: https://dev.to/tin_thnhbnh_6d6c9102d/metas-muse-ember-haiku-level-intelligence-at-a-third-of-the-cost-what-it-means-for-agent-3he9
source_anchor: ""
source_lines: [1, 67]
section: "Muse"
---
On November 9, 2026, Meta introduced Muse Ember, the smallest and most efficient model in the Muse family. It is not pitched as a frontier model. It is pitched as the model that does the work agents repeat all day: tool calls, coding subtasks, routing, extraction, and long document parsing.

That framing matters. Most of an agent's token spend is not one brilliant reasoning leap. It is hundreds of small, structured steps. Ember is built for exactly that layer — and Meta's headline claim is that it delivers more intelligence per dollar than Claude Haiku 5.5 or GPT-6 Luna at any reasoning setting.

Ember completes a division of labor Meta has been building toward:

- 
**Muse Wonder** — for the hardest problems.
- 
**Muse Spark** — for coding and agentic workflows.
- 
**Muse Ember** — for repeatable coding and agentic tasks at scale.

The line Meta itself uses: "Muse Wonder plans, Muse Ember handles the volume." Ember is also the first model trained using Muse Wonder Full, Meta's largest model — the teacher-student pipeline is now part of the product story, not just a training detail.

|  | Muse Ember | 
|---|---|
| Input | $0.10 per million tokens | 
| Cached input | $0.004 per million tokens | 
| Output | $0.50 per million tokens | 
| Context window | 1M tokens | 
| Max output | 512K tokens | 
| Model ID | `muse-ember-1.0` | 
| API | Responses API, Meta Model API, Muse Code | 

For comparison, Meta lists Muse Spark 1.3 at $1.25 per million input tokens and $4.25 per million output tokens. On Artificial Analysis's cost-per-task measure, Ember comes in at about $0.14 per task versus $2.39 for Spark 1.3 — roughly a 17x gap for a one-point difference on the Intelligence Index (36 vs 37).

Ember is natively multimodal: text, images, audio, and video in, so agents can read screenshots, charts, and scanned documents, or work from voice memos and meeting recordings without a separate model.

All figures below are at maximum reasoning effort, from Meta's official announcement, using Artificial Analysis's November 9, 2026 leaderboard and Meta's own internal runs:

| Benchmark | Muse Ember | Claude Haiku 5.5 | GPT-6 Luna | 
|---|---|---|---|
| Artificial Analysis Intelligence Index | 36 | 36 | 28 | 
| AutomationBench-AA (business workflows) | 37.8 | 13.9 | 20.2 | 
| Agents' Last Exam (end-to-end agent tasks) | 61.6 | 57.0 | 57.1 | 
| Terminal-Bench 4.0 (agentic terminal coding) | 40.9 | 39.2 | 16.4 | 
| Coding Agent Index-AA (real agent harnesses) | 46 | 37 | 31 | 
| OSWorld 2.1 (computer use, partial credit) | 69.5 | 68.9 | 50.5 | 
| PerceptionBench with tools (visual perception) | 80.6 | 78.6 | 62.8 | 
| GDP.pdf-AA (professional long documents) | 22.8 | 20.8 | 22.8 | 

The headline is the Intelligence Index tie: Ember matches Haiku 5.5's best score at about a third of the cost per task, and beats every Luna setting. Meta also claims Ember sits on the efficiency frontier of the full Artificial Analysis leaderboard — no model scores higher for less per task.

The agent numbers are the more interesting part. AutomationBench-AA, which runs end-to-end business workflows, is not close: 37.8 against 13.9 for Haiku. That is the benchmark that most resembles what Ember is actually sold for — pulling records, calling tools in sequence, updating systems across apps.

The same table shows the limits, and Meta published them rather than hiding them. On FrontierCode 2.0-AA — real engineering tasks judged on mergeability — Ember scores 27.2, behind Haiku 5.5's 34.9. On long-document work (GDP.pdf-AA) it ties Luna at 22.8 and only narrowly beats Haiku.

The pattern is consistent: Ember is strong at executing structured, repeatable agent steps and weaker at the hardest open-ended engineering. That is not a flaw in the pitch; it is the pitch. You do not route your hardest merge to the cheapest model. You route the hundred routine steps around it.

Before release, Meta evaluated Ember under its Meta Superintelligence Scaling Framework, covering chemical and biological, cybersecurity, and loss-of-control risks. Across the categories assessed, the model remained below the framework's high-risk thresholds, and it received the same core safety training and testing as the larger models. Meta also tested how it behaves when acting on a user's behalf — whether it respects user intent, handles sensitive information appropriately, resists instructions hidden in web pages, documents, or tool outputs, and seeks human input before high-impact actions.

Ember is available today on Muse Code and the Meta Model API as `muse-ember-1.0`, and on Google Cloud and Oracle Cloud. Meta says more efficient models are already in training.

Three practical takeaways:

**1. Route by step type, not by model loyalty.** The economical architecture is now explicit in Meta's own lineup: Wonder plans, Spark handles the hard coding workflows, Ember runs the volume. If your agent spends most of its tokens on file reads, test runs, targeted fixes, and extraction, those steps belong on the cheapest model that clears the quality bar — and Ember's benchmark profile says it clears it for exactly those steps.

**2. The cost math changes what you can afford to retry.** At $0.14 per Artificial Analysis task versus $2.39 for Spark, verification loops, self-correction passes, and multi-sample checks stop being luxuries. A small model that is cheap enough to run three times can beat a big model run once — provided the task is the kind Ember is good at.

**3. Check the harness before you trust the number.** Meta is transparent that Terminal-Bench 4.0 ran each model in its best published harness (mini-swe-agent for Ember, Claude Code for Haiku, Codex for Luna), and that some results are Meta's own internal evaluations. The direction of the results is consistent across independent and internal benchmarks, but the exact margins depend on setup. Pilot it on your own workload before migrating volume.

The bigger signal is structural: the frontier labs are now competing on the cost of the *repeated* step, not just the hardest one. Ember is Meta's bet that the agent economy will be won at the bottom of the stack — and at $0.10 per million input tokens, it is a bet other builders can now test for the price of a coffee.

*Source: Meta AI Research, "Introducing Our Smallest, Most Efficient Model: Muse Ember," November 9, 2026 — https://research.meta.ai/blog/muse-ember*

## Top comments (0)
