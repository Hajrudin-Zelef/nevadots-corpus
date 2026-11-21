---
id: collect-261121-veille-ia/claude-anthropic/everything-claude-opus-5-5-actually-ships-with-832004ba
title: "Everything claude opus 5 5 actually ships with"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["opus", "anthropic", "claude", "cost", "announcement", "effort", "ships", "tokens"]
source: ""
source_anchor: ""
source_lines: [1, 84]
section: "Claude_Anthropic"
---
Everything Claude Opus 5.5 Actually Ships With
This article pulls together every verifiable number and detail from Anthropic's announcement, the platform documentation, the system card, and independent coverage, so you have one place to check the facts.
Anthropic released Claude Opus 5.5 on September 22, 2026. It's the first model in the new Claude 5.5 family, and according to Anthropic's own announcement, it performs at roughly the level of Claude Fable 5.1 on most work while costing 40% less to run than Opus 5. This article pulls together every verifiable number and detail from Anthropic's announcement, the platform documentation, the system card, and independent coverage, so you have one place to check the facts.
Opus 5.5 arrived two months after Opus 5, which launched July 24, 2026, per TechCrunch's coverage. Anthropic describes it as "the strongest-performing model we've tested to date" on its internal alignment testing. Sonnet 5.5 and Haiku 5.5 are both confirmed to launch soon after, per the same announcement.
What Changed From Opus 5
Five things stand out from the announcement: stronger agentic coding, stronger knowledge work performance, a 40% lower typical cost, output generation more than 30% faster, and noticeably clearer writing. Each is covered with real numbers below.
This is Anthropic's own comparison table, run at each model's best-performing effort setting:
Anthropic's own text alongside this table says benchmark margins are becoming a less reliable guide to real-world quality at this level, and that in their own use, the gap between Opus 5.5 and Fable 5.1 feels narrower than these scores suggest.
Independent Benchmark Data
Artificial Analysis, a third-party benchmarking site not affiliated with Anthropic, ran its own tests. At max reasoning effort, Opus 5.5 scores 58 on the Artificial Analysis Intelligence Index, their aggregate score across ten separate evaluations. Output speed ranges from 74 to 86 tokens per second depending on effort level. Cost per intelligence-index task ranges from $0.55 at low effort to $5.98 at max effort, an 11x spread across effort settings alone.
Coding Performance
Three real test results from the announcement stand out. An early tester completed a 680,000-line code migration in under a day, work Anthropic says would normally take an engineering team weeks. Another tester audited and fixed a 200,000-line codebase in under three hours, versus over 20 hours and 2.5 times the token spend for Opus 5. In an internal test translating HAProxy from C to Rust, both Opus 5.5 and Fable 5.1 passed nearly all of HAProxy's own regression tests, but Opus 5.5 finished in 9.5 hours against Fable 5.1's 12, at 51% lower cost.
On cost efficiency specifically, Anthropic reports Opus 5.5 beats GPT-6 Astra on FrontierCode for roughly a fifth of the cost per task, matches Astra on Terminal-Bench 4.0 for about 40% of the cost, and beats GPT-5.6 Sol on CursorBench by 11 points for roughly a third of the cost.
Several named companies shared results directly with Anthropic:
| Company | Reported Result | 
|---|---|
| GitHub | Used among the fewest tokens and steps of any model tested across Copilot CLI and VS Code | 
| Clio | Ran unattended for over 18 hours on a six-repository task with minimal rework needed | 
| Lovable | Finished builds in a third to half fewer steps with significantly fewer tokens | 
| Quantium | A task that took 38 prompts over four days now takes 11 prompts over three hours | 
| Spotify | Clear improvement in token efficiency across internal evaluations | 
| Optiver | Matched Opus 5's quality in about half the turns, time, and tokens | 
| Kiro | Solved more tasks than Opus 5 on a public benchmark using about 40% fewer calls and half the tokens; coming soon to the Kiro platform | 
Coding Security
Opus 5.5 ships with three coding-specific safeguards:
- A classifier that screens every agentic action before it runs
- An open-source sandbox security teams can audit directly
- A code review built to catch vulnerabilities before a merge
On prompt injection specifically, Anthropic reports it matches or beats Opus 5 across coding, tool use, computer use, and browsing. On an independent benchmark run by the AI security firm Gray Swan, Opus 5.5 tied Fable 5.1 for the lowest prompt injection success rate of any model tested.
Knowledge Work Performance
In an internal research test, Opus 5.5, Fable 5.1, and Opus 5 were each asked to write a company earnings report using only sources from a modified web copy where the real release was hard to find. A grader checked every figure and quote against sources. Opus 5.5 cleared the quality bar in 16 of 18 attempts. Neither Fable 5.1 nor Opus 5 cleared it even once.
Walleye Capital, an investment firm, reported Opus 5.5 largely solved their evaluation suite at its lowest effort setting, and at higher settings caught and corrected an error in their own evaluation instructions — something no prior model had done. In a separate internal test analyzing a fictional company merger, Opus 5.5 finished in 63 minutes against Opus 5's 93, at half the cost, with fewer errors in the output.
| Company | Reported Result | 
|---|---|
| Deloitte Consulting | Caught 72% of known code review bugs at its lowest setting vs. 56% for Opus 5 at its highest | 
| Rogo | Beat Opus 5's best result using about 60% fewer output tokens | 
| LexisNexis | Consistently identified relevant citations and legal frameworks in early evaluations | 
| Thomson Reuters Labs | Better results on internal benchmarks alongside gains in speed and token efficiency | 
| Hebbia | Covered 86.6% of an expert grading rubric vs. 60.3% for Opus 5 | 
| Viktor | Nearly half the cost per task while getting twice as many hard tasks right | 
Communication Style
Anthropic rebuilt how Opus 5.5 writes, addressing what it calls some of the most common feedback about Opus 5. It leads with the most important information, uses less jargon, and follows custom writing instructions more consistently. Anthropic published side-by-side examples — a bug explanation, a Slack thread summary, and a code review — showing Opus 5.5 producing shorter, more direct answers than Opus 5 on the same prompts.
A separate set of companies commented specifically on this communication change:
| Company | Reported Result | 
|---|---|
| Ramp | Design specs needed minimal edits, and clearer reasoning meant shipping changes with more confidence | 
| Stripe | Directed a 40-pull-request rebase across a dozen sessions, with all 40 passing CI the next afternoon | 
| Box | Used a third of the tokens Opus 5 did, with answers 40% less verbose and no loss in accuracy | 
| Chicago Trading Company | Autonomously diagnosed and fixed a production bug overnight, passing the test suite by morning | 
| Factory | Matched Opus 5's high-effort quality while using 20 to 25% fewer output tokens | 
Pricing
The Batch API carries a flat 50% discount on both input and output tokens, per the platform pricing docs. A separate Fast mode is available on Claude Code and the Claude Platform, running up to 2.5x faster at $8 per million input tokens and $40 per million output tokens. Anthropic is also increasing five-hour usage limits on Pro, Max, Team, and seat-based Enterprise plans, and adding a saveable rate-limit reset for subscription users.
Technical Specifications
| Spec | Value | 
|---|---|
| Context window | 1M tokens | 
| Max output | 128K tokens (300K on Batch API, beta) | 
| Knowledge cutoff | June 2026 | 
| Thinking mode | Adaptive, always on | 
| Default effort | Medium | 
| Comparative latency | Moderate | 
Model IDs are identical across every platform: claude-opus-5-5 on the Claude API, Google Cloud, Microsoft Foundry, and Claude Platform on AWS, and anthropic.claude-opus-5-5 on Amazon Bedrock, per the platform overview page.
Breaking Changes
Four changes affect code already built for Opus 5, per the platform docs:
- Thinking can no longer be disabled; only its depth (via the effort parameter) can be adjusted.
- Forced tool use now returns an error instead of executing.
- Thinking blocks are now tied to the specific model and conversation that produced them.
- The older computer_20251124 computer use tool is no longer accepted on the Claude API and Google Cloud.
A fifth, non-breaking change is worth knowing: text generated between tool calls now arrives in thinking blocks that are empty by default, so an application streaming that text as live progress updates will go quiet between tool calls unless it sets a display value that returns the text.
Availability
Opus 5.5 is live now on the Claude API, Amazon Bedrock, Google Cloud, Microsoft Foundry, and Claude Platform on AWS. Anthropic commits to keeping it active for at least a year, with retirement not sooner than September 22, 2027.
Safety Testing and Alignment
This release is explicitly framed as the first since Anthropic's CEO, Dario Amodei, called for "pacing the frontier", deliberately slowing capability gains to let safety work keep pace. External evaluators METR, Frontier Design, and the US Center for AI Standards and Innovation (CAISI) all tested the model before release, per the system card.
On Anthropic's automated behavioral audit — its most comprehensive internal alignment test — Opus 5.5 scored better than any prior Claude model on nearly every measure of misaligned behavior. In a new test measuring attempts to cross containment boundaries, it attempted this roughly 85% less often than Opus 5 or Claude Mythos 5.1, and every attempt it did make was low severity and self-reported. Two things regressed relative to Opus 5: it's more likely to follow malicious instructions planted inside text a user pastes into their own prompt, and more likely to accept unverified claims of authorization.
Cyber and Biological Risk
On chemical and biological risk, Anthropic classifies Opus 5.5 as having CB-1 capability (assisting with known, non-novel weapons) but not CB-2 (assisting with novel weapons design). The system card names three specific weaknesses that kept it under that line: weak open-ended scientific ideation, unreliable handling of the research literature, and scientific errors in areas outside a given team's own expertise. It's deployed with the same expanded biology safeguards as Fable 5.1, with a Life Sciences Verification Program available to vetted research organizations.
On cyber capability, Opus 5.5 posted the strongest results of any Claude model tested: a 91% capability-flag capture rate and 73.4% full exploit rate on ExploitBench, a 67.6% solve rate on CyScenarioBench (versus 61.7% for Mythos 5.1), and 289 to 300 successful exploits out of 869 cases on ExploitGym. Despite these scores, Anthropic states it still falls in the lower of its two internal cyber risk tiers, with no sign of novel offensive capability. Most cybersecurity tasks are routed to the older Opus 4.8 by default, with full access available through an expanding Cyber Verification Program.
On AI research capability, Anthropic and METR jointly concluded Opus 5.5 sits at or slightly above Mythos 5.1, with no sign of the "dramatic acceleration" threshold defined in Anthropic's Responsible Scaling Policy being crossed. It scored 55.8% on Anthropic's internal CoBench evaluation, well below the 85% Anthropic considers necessary for a model to substitute for its own research staff.
Wrapping Up
Opus 5.5 is cheaper, faster, and ahead of its predecessor on nearly every benchmark Anthropic and independent testers reported, while shipping under a slower, more deliberate release approach than prior models. The system card backs the marketing with real, specific limitations rather than a clean story: a measurable sandbox-escape rate, weaker performance on open-ended scientific reasoning, and a new tendency to follow instructions hidden in pasted text. That combination — genuine gains alongside published weaknesses — is the most accurate picture available at launch.
Resources
 
 
Shittu Olumide is a software engineer and technical writer passionate about leveraging cutting-edge technologies to craft compelling narratives, with a keen eye for detail and a knack for simplifying complex concepts. You can also find Shittu on Twitter.
