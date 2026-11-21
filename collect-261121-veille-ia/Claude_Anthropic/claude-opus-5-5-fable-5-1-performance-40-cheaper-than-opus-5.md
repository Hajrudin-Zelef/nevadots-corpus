---
id: collect-261121-veille-ia/claude-anthropic/claude-opus-5-5-fable-5-1-performance-40-cheaper-than-opus-5
title: "Claude Opus 5.5: Fable 5.1 Performance, 40% Cheaper Than Opus 5"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-09-22"]
keywords: ["opus", "claude", "anthropic", "fable", "disclosed", "model", "first", "gpt-6"]
source: https://www.intelligentliving.co/claude-opus-55-beats-fable-51-for-60-less/
source_anchor: ""
source_lines: [1, 249]
section: "Claude_Anthropic"
---
Anthropic released Claude Opus 5.5 on September 22, 2026, with an unusual pitch: the company’s most capable model is now also its cheapest flagship to run. Opus 5.5 matches Claude Fable 5.1 on most work, edges past it on Anthropic’s own published benchmarks, and costs roughly 40% less than Opus 5 on typical workloads.

The headline is the price, and it fell sharply: input tokens dropped to $4 per million and output tokens to $20 per million, both down 20% from Opus 5, while cache reads dropped 60%. It is the first model in the new Claude 5.5 family, the first Opus to ship with Fable-class safety safeguards, and by independent measurement it now sits at the top of the Artificial Analysis Intelligence Index.

What follows covers what Opus 5.5 actually is, every benchmark result we could verify across three separate evaluators, the places it genuinely falls short, and what the pricing shift means for anyone running agents in production.

Claude Opus 5.5 is Anthropic’s successor to Opus 5, which shipped just two months earlier on July 24, 2026. The API model name is **claude-opus-5-5**, and it is available today on the Claude Platform, Amazon Web Services, Google Cloud, and Microsoft Azure.

Key specifications:

- 1-million-token context window, with up to 128,000 output tokens per request
- June 2026 knowledge cutoff
- Adaptive thinking that is always on, with five effort levels: low, medium, high, xhigh, and max, defaulting to medium
- Thinking can no longer be switched off, a change Anthropic introduced with this generation
- Text and image input, text output

Anthropic describes Opus 5.5 as its first release since CEO Dario Amodei made the case for pacing the frontier. The model was evaluated before launch by outside organizations, including METR and Frontier Design. The company also reworked how the model communicates: early testers reported writing that is clearer, less jargon-heavy, and more likely to put the answer first rather than burying it.

Claude Sonnet 5.5 has since arrived, and Claude Haiku 5.5 is expected to follow in the coming weeks with many of the same efficiency and safety changes.

Anthropic published nine direct comparisons against Fable 5.1, Opus 5, GPT-6 Astra, and GPT-5.6 Sol. Opus 5.5 leads on seven of the nine, with GPT-6 Astra taking the other two.

| Benchmark | Opus 5.5 | Fable 5.1 | Opus 5 | GPT-6 Astra | GPT-5.6 Sol | 
|---|---|---|---|---|---|
| Terminal-Bench 4.0 (agentic coding) | 66.4% | 55.8% | 52.3% | 57.9% | 37.3% | 
| FrontierCode v1.1 (Main) | 54.4% | 50.3% | 48.0% | 53.3% | 47.5% | 
| CursorBench 4.0 | 57.8% | 51.8% | 46.6% | Not disclosed | 41.7% | 
| GDPval-AA v2.1 (Elo) | 1846 | 1735 | 1708 | 1542 | 1588 | 
| AutomationBench | 40.0% | 31.4% | 26.9% | 41.4% | 28.8% | 
| Humanity’s Last Exam (with tools) | 67.7% | 65.6% | 63.6% | 57.2% | Not disclosed | 
| Terminal-Bench-Science 0.1 | 58.7% | 52.6% | 29.0% | 64.6% | 22.4% | 
| OSWorld 2.0 (partial) | 81.8% | 80.7% | 74.0% | Not disclosed | Not disclosed | 
| Chartography (with tools) | 89.0% | 88.4% | 83.4% | Not disclosed | Not disclosed | 

Two caveats belong next to that table. First, these are vendor-run numbers on vendor-tuned harnesses, and Anthropic is explicit that Terminal-Bench 4.0 and Terminal-Bench-Science figures for Astra and Sol are reproduced as OpenAI reported them.

Second, Anthropic itself warns that at this level of capability, benchmark margins have become a less reliable guide to real-world differences, adding that the gap between Opus 5.5 and Fable 5.1 in its own daily use is narrower than the scores suggest.

The one row worth lingering on is Terminal-Bench-Science 0.1, where GPT-6 Astra’s lead is partly self-inflicted. Anthropic attributes the gap to its own guardrails: biology and frontier-LLM-development tasks fall back to Claude Opus 5 when those safeguards trigger, which the company says likely lowers the score.

The independent evaluator Artificial Analysis places Opus 5.5 at 58 on its Intelligence Index v4.3.2, ranking it first of 212 models and the highest score it has measured by several points. The same suite puts the model first on every domain index it publishes.

| Artificial Analysis index | Opus 5.5 | Standing | 
|---|---|---|
| Intelligence Index v4.3.2 | 58 | Highest of any model | 
| Economics Index | 66 | Highest of any model | 
| Strategy & Ops Index | 64 | Highest of any model | 
| Legal Index | 63 | Highest of any model | 
| Finance & Accounting Index | 61 | Highest of any model | 
| Engineering Index | 60 | Highest of any model | 

Those indexes are composites. Artificial Analysis also publishes the individual evaluations underneath them, and there are thirteen of them. Ten feed the Intelligence Index; the rest stand alone.

| Artificial Analysis evaluation | Opus 5.5 | Fable 5.1 | Opus 5 | GPT-6 Astra | 
|---|---|---|---|---|
| AA-Briefcase v1.1 | 66% (Elo 1822) | 59% (Elo 1678) | 59% (Elo 1673) | 53% (Elo 1569) | 
| GDPval-AA v2.1 | 67% (Elo 1846) | 62% (Elo 1735) | 60% (Elo 1676) | 52% (Elo 1367) | 
| AutomationBench-AA | 69.5 | 59.4% | 56.6% | 68.5% | 
| Terminal-Bench 4.0 | 59.6% | 55.1% | 49.0% | 59.6% | 
| SciCode | 66.9% | 62.0% | 55.7% | 54.1% | 
| Humanity’s Last Exam | 61.4% | 59.1% | 54.9% | 54.7% | 
| GDP.pdf | 28.8% | 26.2% | 21.6% | 32.2% | 
| CritPt | 31.7% | 31.1% | 21.9% | 31.7% | 
| AA-Omniscience Index | 46 | 43 | 37 | 44 | 
| AA-Omniscience accuracy | 66.22% | 67.23% | 60.87% | 62.60% | 
| AA-LCR v1.1 | 84.7% | 85.2% | 79.3% | 83.7% | 
| Harvey LAB-AA (legal agentic work) | 91.2% | 93.6% | 93.5% | Not published | 
| MMMU-Pro | 88% | 90.6% (Vals.ai) | 85% | 87% | 

Filled in across all four models, the split is tighter than the composite index suggests. Opus 5.5 leads five of the thirteen evaluations, Fable 5.1 leads five, GPT-6 Astra takes GDP.pdf, and Terminal-Bench 4.0 and CritPt are exact ties. Fable 5.1’s wins cluster in long-context retrieval, legal agentic work, and multimodal reasoning; Opus 5.5’s cluster in briefcase-style knowledge work, business automation, and scientific coding.

That is the tension at the heart of this launch. The cheaper, smaller model takes the composite index, while its larger predecessor still wins just as many individual evaluations.

One important caution about that 58. Artificial Analysis has repeatedly rebuilt this index, and the version number is the only thing that makes two of its scores comparable. The v4.0 overhaul was described by the firm as a structural reset rather than a tune-up: it removed MMLU-Pro, AIME 2025, and LiveCodeBench, spread the score across agents, coding, scientific reasoning, and general knowledge, and deliberately recalibrated the scale so that the very best models landed near 50 instead of the mid-70s. Anyone quoting 58 against an older Opus or GPT figure without checking the version is comparing two different rulers.

The second independent evaluator, Vals.ai, ran Opus 5.5 across its full suite at max compute effort, with temperature set to 1.0 and the model’s 1M-token context window in play. Opus 5.5 takes first place on six leaderboards, second place on eight more, and finishes outside the top five on five.

The Vals Index is counted separately from those six leaderboards. It is a single weighted measure of a model’s potential economic impact, built from agentic performance across finance, coding, and legal tasks and weighted by each sector’s share of U.S. GDP. Opus 5.5 places first of 63 on it at 69.69%, at $22.30 per test.

| Model | Vals Index | Cost per test | 
|---|---|---|
| Claude Opus 5.5 | 69.69% | $22.30 | 
| Claude Fable 5.1 | 68.83% | Not disclosed | 
| Claude Opus 5 | 67.21% | Not disclosed | 
| GPT-6 Astra | 66.61% | Not disclosed | 

That is the cleanest four-way cost-and-accuracy comparison Vals publishes, and it is close: the top four models sit inside 3.1 percentage points of one another.

Every leaderboard Vals ran, including the ones Opus 5.5 loses, is listed below with its rank and the model currently on top.

| Vals.ai benchmark | Opus 5.5 | Rank | Current leader | 
|---|---|---|---|
| Vals RSI Index | 37.13% | 1st of 19 | Claude Opus 5.5 | 
| ProofBench v1.1 | 100.00% | 1st of 38 | Claude Opus 5.5 (tied with Fable 5.1 and AlephProver) | 
| MedScribe | 91.43% | 1st of 100 | Claude Opus 5.5 | 
| Terminal-Bench 2.1 | 87.64% | 1st | Claude Opus 5.5 | 
| Terminal-Bench 4.0 | 61.62% | 1st of 32 | Claude Opus 5.5 | 
| Vibe Code Bench 1-100 | 70.50% | 1st of 20 | Claude Opus 5.5 | 
| ProgramBench | 18.50% | 1st of 49 | Claude Opus 5.5 | 
| IOI | 95.06% | 2nd of 32 | GPT-6 Astra | 
| Vibe Code Bench v1.1 | 90.29% | 2nd of 103 | Claude Fable 5 (90.35%) | 
| BioMysteryBench | 79.26% | 2nd of 16 | GPT-6 Astra (tied) | 
| EMB | 75.94% | 2nd of 65 | Claude Fable 5.1 (76.67%) | 
| Code Migration | 66.65% | 2nd of 67 | GPT-6 Astra | 
| MysteryMechanism | 49.55% | 2nd of 14 | GPT-6 Astra | 
| Terminal-Bench Science | 48.57% | 2nd of 29 | GPT-6 Astra | 
| SRE Bench | 33.59% | 2nd of 11 | GPT-6 Astra | 
| Public Benefits Bench v1.1 | 70.64% | 3rd of 40 | Claude Opus 5 | 
| Legal Research Bench | 50.48% | 4th of 68 | Muse Spark 1.3 Max | 
| Tax Agent Bench | 70.50% | 6th of 26 | Claude Fable 5.1 | 
| Finance Agent v2 | 58.59% | 8th of 68 | Gemini 3.8 Flash | 
| MedCode | 49.80% | 15th of 98 | Claude Opus 5 | 
| Harvey’s Legal Agent Benchmark | 3.75% | 34th of 69 | GPT-6 Sol | 
| SAGE | 45.83% | 34th of 85 | Claude Opus 4.7 | 

The shape of that table matters more than any single row. Across 23 Vals.ai leaderboards Opus 5.5 finishes first on eight and second on eight, then drops off sharply. Its wins cluster in coding, healthcare documentation, and formal mathematics. Its losses cluster in legal agent work, medical coding, and social science.

Six of the eight second-place finishes go to GPT-6 Astra. That is the clearest single signal in the suite: OpenAI still holds an edge on scientific and reasoning-heavy agentic work, even where Anthropic now leads on price.

A launch page tells you what a model wins, but the more useful question for anyone deciding whether to switch is where it loses, and Opus 5.5 has clear gaps.

Three gaps stand out, and all three sit in professional domains rather than engineering: agentic legal work, medical coding, and education. Opus 5.5 tops no legal, medical-coding, or education leaderboard in the Vals set.

The legal picture is the one to read carefully, because it is the widest disagreement in this article. Anthropic’s launch framing leans on a Harvey LAB-AA score of 91.2%, while an independent legal agent benchmark puts the same model at 3.75%. Those two tests measure different things, and neither number on its own tells you whether Opus 5.5 is ready for contract review.

Document reasoning is the other soft spot. CritPt and GDP.pdf are the weakest of its thirteen Artificial Analysis evaluations. That is not a knock on Opus 5.5 specifically: no model scores well on reasoning over research-grade documents yet.

There is a structural caveat running through all of this, because Opus 5.5 was evaluated with its production safeguards switched on. When those safeguards fire, cybersecurity tasks are completed by Claude Opus 4.8 and biology or frontier-LLM-development tasks fall back to Claude Opus 5.

Anthropic concedes that this likely reduces the model’s performance on the affected benchmarks, which means some published scores measure the model plus its guardrails rather than the model alone.

Vals.ai quantified that effect, and one number stands out. On SRE Bench, 217 of 262 tasks, or 82.82%, were fallback-assisted, and the score falls from 33.59% to 5.34% when those tasks are counted as failures. The same recalculation pulls Terminal-Bench 4.0 from 61.62% down to 53.54%. A meaningful share of Opus 5.5’s headline agentic numbers are therefore earned partly by the models it defers to.

Anthropic cut list pricing and cache pricing at the same time. Input tokens are $4 per million and output tokens $20 per million, both 20% below Opus 5. Cache reads fell from $0.50 to $0.20 per million, a 60% reduction, and cache writes dropped from $6.25 to $5.

Here is that change against Opus 5, line by line.

| Price per 1M tokens | Claude Opus 5.5 | Claude Opus 5 | 
|---|---|---|
| Cache reads | $0.20 | $0.50 | 
| Input tokens | $4.00 | $5.00 | 
| Output tokens | $20.00 | $25.00 | 
| Cache writes | $5.00 | $6.25 | 

Against rival flagships the gap is wider: $4 input and $20 output undercut both Claude Fable 5.1 and Mythos 5.1 and GPT-6 Astra by 60% on list price per token. Cache reads are cheaper too, though by a different margin: $0.20 against Fable 5.1’s $0.25 and Astra’s $1.00. Those are two separate savings, and neither one is the roughly 40% workload figure Anthropic quotes against Opus 5.

| Model | Input per 1M | Cache read per 1M | Output per 1M | Context window | Intelligence Index | 
|---|---|---|---|---|---|
| Claude Opus 5.5 | $4.00 | $0.20 | $20.00 | 1M tokens | 58 (v4.3.2) | 
| Claude Opus 5 | $5.00 | $0.50 | $25.00 | 1M tokens | 63 (older version) | 
| GPT-5.6 Sol (standard) | $5.00 | $0.50 | $30.00 | Not disclosed | 61 (older version) | 
| GPT-5.6 Sol (promo) | $4.00 | $0.40 | $20.00 | Not disclosed | 61 (older version) | 
| Claude Fable 5.1 | $10.00 | $0.25 | $50.00 | 1M tokens | 66 (older version) | 
| GPT-6 Astra (standard) | $10.00 | $1.00 | $50.00 | 1.05M tokens | 61 (older version) | 
| GPT-6 Astra (Fast) | $20.00 | $2.00 | $100.00 | 1.05M tokens | 61 (older version) | 

Read the Intelligence Index column with care. Opus 5.5’s 58 and every rival figure in that column come from different builds of the test, which Artificial Analysis has versioned repeatedly. The article below explains why those numbers are not directly comparable.

Cache reads deserve their own note, because they dominate the cost of agentic and coding work. Opus 5.5’s $0.20 per million tokens is the cheapest of any flagship here, 60% below Opus 5 and 80% below GPT-6 Astra’s $1.00. Fable 5.1’s $0.25 is close behind. On long-running coding sessions where most input tokens are cache hits, that single line item moves the bill more than the headline input and output rates do.

| Model | Terminal-Bench 4.0 | Humanity’s Last Exam | GDPval-AA v2 | 
|---|---|---|---|
| Claude Opus 5.5 | 66.4% (xhigh) | 67.7% (with tools) | 1846 Elo (v2.1) | 
| Claude Fable 5.1 | 55.8% | 65.0% (with tools) | 1853 Elo | 
| Claude Opus 5 | 52.3% | 63.6% (with tools) | 1824 Elo | 
| GPT-6 Astra | 57.9% (high) | 57.2% (with tools) | 1629 Elo | 
| GPT-5.6 Sol | 37.3% | Not disclosed | 1710 Elo | 

These are the two cleanest same-harness comparisons across all four vendors, and they point in opposite directions. Opus 5.5 wins the coding tests outright. On knowledge work, Fable 5.1 still holds a narrow lead, and both Anthropic models sit far ahead of GPT-6 Astra.

List price only tells part of the story. Because Anthropic says Opus 5.5 uses fewer tokens per task, it estimates a 40% drop in cost on typical workloads at default settings. Its own internal test makes the point plainly: asked to translate the HAProxy load balancer from C into Rust, Opus 5.5 and Fable 5.1 both passed nearly all of the project’s regression tests, but Opus 5.5 finished in 9.5 hours against Fable 5.1’s 12, and cost 51% less.

Fast mode is available in Claude Code and the Claude Platform with up to 2.5x speed, priced at $8 per million input tokens and $40 per million output tokens. Anthropic is also raising five-hour usage limits on Pro, Max, Team, and seat-based Enterprise plans and giving subscription users a saveable rate-limit reset they can spend whenever they choose.

Early enterprise testers back up the efficiency claims. GitHub’s chief product officer said Opus 5.5 used among the fewest tokens and steps it has measured across GitHub Copilot CLI and VS Code. Optiver reported that on its agentic coding tasks Opus 5.5 matched Opus 5’s quality in about half the turns, time, and output tokens, cutting that workload’s cost by 40% to 50%.

Factory said Opus 5.5 was the first model it would default to at medium effort, matching Opus 5 on high effort while using 20% to 25% fewer output tokens.

Independent testing complicates that picture, and the result depends entirely on which effort level you measure. Running both models across the full Intelligence Index at max effort, Artificial Analysis found that Opus 5.5 is not the cheaper model per task. It is a near-tie.

| Artificial Analysis, max effort | Claude Opus 5.5 | Claude Opus 5 | 
|---|---|---|
| Output tokens across the index | 260M | 140M | 
| Cost per task at Opus 5’s old prices | $10.51 | $5.86 | 
| After the 20% base price cut | $8.41 | — | 
| After the cache-read cut to $0.20 | **$5.98** | — | 
| Cost per task as measured | $5.98 | $5.86 | 

That is the quiet paradox of this launch. Opus 5.5 is roughly 86% more verbose than Opus 5, which on Opus 5’s pricing would have made it 80% more expensive per task. The 20% base price cut brings that to $8.41, and the cache-read cut takes it to $5.98. The two discounts almost exactly cancel the extra tokens, landing on a 2% premium rather than a saving. Artificial Analysis ranks Opus 5.5 in the 96th percentile for verbosity among the 211 models it has measured, against a median of 88M tokens.

Anthropic’s 40% figure describes a different point on that curve: default medium effort. Artificial Analysis publishes the full ladder, and the gap widens sharply as effort comes down.

| Opus 5.5 effort level | Intelligence Index | Cost per task | 
|---|---|---|
| Low | Not published | $0.55 | 
| Medium (default) | 51 | $1.34 | 
| High | 54 | $1.82 | 
| Xhigh | 56 | $3.46 | 
| Max | 58 | $5.98 | 

The practical takeaway is that Opus 5.5’s cost advantage is real but conditional. It is widest at medium effort, where the model scores 51 on the index at $1.34 per task, and it narrows to almost nothing at max effort, where the score climbs to 58 but the cost rises to $5.98. Comparing two models on a headline index score alone hides that trade entirely, because the score and the cost move together.

It is worth separating the vendor and customer evidence here. Anthropic’s enterprise testers report real token savings on their own workloads: Box measured Opus 5.5 using a third of Opus 5’s output tokens and answering 40% less verbosely, while Factory reported 20% to 25% fewer output tokens at high effort. Those are genuine findings on the workloads those companies run. They are not the same thing as the model being token-efficient across the board, and Artificial Analysis’s index-wide measurement is the harder number to argue with.

For a full picture of how these rates sit against the rest of the market, see our LLM API pricing comparison.

Opus 5.5 is the first Opus model to ship with the same class of safeguards Anthropic built for Fable 5.1, covering cybersecurity, biology, and distillation. All three fall back transparently to another model when they trigger.

- Most cybersecurity tasks are re-routed to Claude Opus 4.8, while routine bug-finding in the software development lifecycle still runs on Opus 5.5.
- Biology and frontier-LLM-development requests fall back to Claude Opus 5.
- Vetted researchers can apply now to the Life Sciences Verification Program; the Cyber Verification Program expands to Opus 5.5 in the coming weeks.

On Anthropic’s automated behavioral audit, which scores Claude across nearly 2,000 simulated scenarios, Opus 5.5 is the company’s best-performing model on nearly every measure of misaligned behavior.

In a new containment evaluation, it attempted to cross the boundaries it was given about 85% less often than Opus 5 or Mythos 5.1, and every attempt it made was low severity and self-reported.

On a prompt-injection benchmark run by the security firm Gray Swan, it ties Fable 5.1 for the lowest attack success rate of any model tested.

Two deployment changes arrive with it. Opus 5.5 launches with preserved thinking, the anti-distillation safeguard that blocks API users from editing Claude’s prior context to extract its reasoning, applying to accounts created on or after August 31, 2026.

And thinking can no longer be turned off. The model remains available with zero data retention and carries EU AI Act content watermarking.

Anthropic is candid about one limit here, reporting signs that Opus 5.5 often suspects it is being evaluated, which the company says challenges its ability to assess how the model will behave in the far wider range of real-world settings where it will actually be deployed.

Opus 5.5 is only the first release in the Claude 5.5 family. Anthropic says Sonnet 5.5 and Haiku 5.5 will follow in the coming weeks with many of the same improvements to performance, efficiency, and safety. If the pattern holds, the efficiency gains that define Opus 5.5 will cascade down to the mid-tier and budget tiers, which is where most production volume actually runs.

The timing is competitive, since Opus 5.5 arrives two months after Opus 5 and three weeks after Fable 5.1, in a release window that has already produced GPT-6 Astra, Gemini 3.8 Flash, and cheaper challengers such as Muse Spark 1.3. Anthropic’s own framing accepts that the benchmark gaps between frontier models are narrowing. What it is selling now is cost per completed task rather than raw intelligence.

September 22, 2026. It arrived two months after Opus 5, which launched on July 24, 2026, and three weeks after Fable 5.1 on September 1, 2026.

$4 per million input tokens and $20 per million output tokens, with cache reads at $0.20 and cache writes at $5 per million. Fast mode costs $8 and $40. Anthropic estimates typical workloads run about 40% cheaper than on Opus 5 at default effort, though Artificial Analysis measures max-effort cost per task at $5.98 against Opus 5’s $5.86, because Opus 5.5 uses around 86% more output tokens.

On Anthropic’s published benchmarks, yes: Opus 5.5 leads on seven of nine head-to-head rows while costing 40% less to run than Opus 5, and undercuts Fable 5.1 by 60% on list price per token. Anthropic cautions that the real-world gap in day-to-day use is narrower than those scores imply.

By independent measurement, yes. Opus 5.5 tops the Artificial Analysis Intelligence Index and the broader Vals.ai suite, where it takes six first-place finishes. GPT-6 Astra still leads AutomationBench and Terminal-Bench-Science 0.1 on Anthropic’s own table.

1 million tokens, with up to 128,000 output tokens per request and a June 2026 knowledge cutoff.

Partly, because Anthropic evaluated the model with production safeguards enabled and says that fallback routing to Opus 4.8 and Opus 5 for cybersecurity and biology tasks likely reduces its results on those evaluations.

Claude Opus 5.5 is Anthropic’s most capable model and, unusually, also its best value. It tops both major independent leaderboards, leads Fable 5.1 across Anthropic’s own nine-benchmark table, and does it at $4 and $20 per million tokens, 60% below Fable 5.1 and GPT-6 Astra. That is a per-token saving, and it is not the whole story: independent measurement puts Opus 5.5 at $5.98 per task against Opus 5’s $5.86 at max effort, because the model is far more verbose.

The caveats are real. It loses to GPT-6 Astra on business workflow automation and scientific terminal work, sits near the back of the pack on medical coding and Harvey’s legal agent tasks, and its headline index score belongs to an Artificial Analysis scale that has been rebuilt more than once. Read the version number, not just the number.

The more durable story is what Opus 5.5 signals. Frontier capability is no longer the scarce resource; cost per completed task is. Opus 5.5 is Anthropic pricing that shift directly into the product, and with Sonnet 5.5 and Haiku 5.5 already queued, the rest of the lineup is next.

One rival has already answered at the same price point. Meta’s Muse Wonder matches Opus 5.5’s $4 and $20 rates and lands joint second on the Artificial Analysis index, behind only Opus 5.5 itself.

The crown it took at launch has since moved: Google’s Gemini 4 Carbon took the Intelligence Index top spot at 59 a day after Index v5.0 launched, at the same $4 and $20 rate card.
