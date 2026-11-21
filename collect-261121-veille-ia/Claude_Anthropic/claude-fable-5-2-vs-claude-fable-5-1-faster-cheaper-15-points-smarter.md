---
id: collect-261121-veille-ia/claude-anthropic/claude-fable-5-2-vs-claude-fable-5-1-faster-cheaper-15-points-smarter
title: "Claude Fable 5.2 vs Claude Fable 5.1: Faster, Cheaper, 15 Points Smarter"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-09-01"]
keywords: ["fable", "claude", "input", "output", "effort", "cheaper", "points", "anthropic"]
source: https://codingfleet.com/blog/claude-fable-5-2-vs-fable-5-1/
source_anchor: ""
source_lines: [1, 270]
section: "Claude_Anthropic"
---
Anthropic shipped **Claude Fable 5.2** on 9 November 2026, ten weeks after **Fable 5.1** (1 September 2026). It is the same Fable/Mythos model that ships in two safeguard tiers, and the headline numbers are unusual in one respect: it is *cheaper* per token than the model it replaces, yet it jumps 15 points on the independent Artificial Analysis Intelligence Index. That combination is rare enough to be worth checking line by line.

### TL;DR

- **More capable, 20% cheaper.** $8 / $40 per million input/output tokens, down from $10 / $50. Every cache line drops too.
- **Artificial Analysis: 65 vs 49 on Intelligence Index v5.** Fable 5.2 takes #1 overall and sets new bests on 5 of the 9 sub-evals, most of them science and knowledge.
- **Do not run it at max effort by default.** High effort scores 62 for $9.08 per AA task; max scores 65 for $26.54. You pay 2.9× for 3 points.
- **High effort beats Fable 5.1 at max on both axes:** 62 vs 49, at $9.08 vs $12.99 per task.
- **The biggest vendor gains are outside coding:** RoboDojo Sim +37.3 points, Chartography +28.6, Remote Labor Index +23.4, Terminal-Bench-Science +22.0.
- **Not everything moved.** On AA-Briefcase Elo and Terminal-Bench 4.0 it lands level with Opus 5.5, and Gemini 4 Carbon still wins AutomationBench.
- **Speed is the softest claim.** Anthropic says "up to 30% faster"; AA measures 98 tok/s vs 88, about 11%, and a longer wall-clock time per task at max effort.

Fable 5.1 → Fable 5.2 (max)

$8 / $40 per MTok

vs $12.99 for 5.1 at max

single vendor gain

## The short version: three things changed

Fable 5.1 was an incremental step up from Fable 5 (September 2026's "better at every coding and knowledge task" release, with a cache-read cut to $0.25). Fable 5.2 is a different shape of release. Anthropic pushed the gains into science, knowledge, vision and robotics, dropped the price, and changed the economics of the effort dial by making the middle settings far stronger.

| Specification | Claude Fable 5.1 | Claude Fable 5.2 | Change | 
|---|---|---|---|
| Released | 1 September 2026 | 9 November 2026 | Ten weeks later | 
| Status | Legacy | Active (latest) | Migration recommended | 
| API model ID | `claude-fable-5-1` | `claude-fable-5-2` | Bedrock: `anthropic.claude-fable-5-2` | 
| Context window | 1M | 1M | Unchanged | 
| Max output | 128K | 128K | Unchanged | 
| Thinking | Adaptive, always on | Adaptive, always on | Effort is still the only dial | 
| Effort levels | low → max | low → max | Five settings | 
| Default effort | `high` (API) | `high` (API),`medium` in apps | Apps and Claude Code default to medium | 
| Knowledge cutoff | June 2026 | June 2026 | Unchanged | 
| Fast mode | Not offered | 2.5× output, 2× price | New `speed` parameter | 
| Input / output price | $10 / $50 | $8 / $40 | −20% | 
| Retirement | Not announced | Not sooner than 9 Nov 2027 | — | 

**One structural change worth knowing.** Fable 5.2 requires 30-day data retention for safety monitoring. Zero data retention is possible only for eligible customers running Anthropic's Enterprise Frontier Safeguards, which keeps the monitoring inside your own infrastructure. If you send regulated data through the API, that is a procurement question, not a footnote.

## Price: cheaper on every line, not just the two headline numbers

The 20% cut applies across the whole rate card, including cache writes and batch. Only fast mode is new money.

| Per 1M tokens (USD) | Fable 5.1 | Fable 5.2 | Change | Note | 
|---|---|---|---|---|
| Input | $10.00 | $8.00 | −20% | Full 1M context at the same rate | 
| Output (incl. thinking) | $50.00 | $40.00 | −20% | Reasoning bills as output | 
| 5-minute cache write | $12.50 | $10.00 | −20% | 1.25× input | 
| 1-hour cache write | $20.00 | $16.00 | −20% | 2× input | 
| Cache read | $0.25 | $0.20 | −20% | 0.025× input on both | 
| Batch input / output | $5 / $25 | $4 / $20 | −20% | 50% batch discount still stacks | 
| Fast mode input / output | — | $16 / $80 | New | 2× standard, not available on Batch | 

**List price per million tokens, USD.**Fable 5.2 is 20% cheaper on all six standard lines. Fast mode is the only way to spend more than Fable 5.1 did.

**Monthly bill for 100,000 requests**at 2.5K input and 600 output tokens (250M input, 60M output). Excludes reasoning tokens and cache hits, so real bills will be higher for both models — but the ratio holds.

## Independent numbers: the Artificial Analysis effort ladder

Artificial Analysis re-ran the model, and its leaderboard is the cleanest like-for-like comparison because every effort setting is the same nine-evaluation harness (Intelligence Index v5.0.1). The result is a straight jump with a cost cliff at the top:

| Model & effort | AA Index v5 | Cost per task | vs 5.1 at max | Read | 
|---|---|---|---|---|
| Fable 5.1 (max) | 49 | $12.99 | — | The bar to clear | 
| Fable 5.2 (low) | 54 | $3.10 | +5 pts, 76% cheaper | Beats 5.1 max at a quarter of the cost | 
| Fable 5.2 (medium) | 56 | $3.80 | +7 pts, 71% cheaper | Default in Claude apps and Claude Code | 
| Fable 5.2 (high) | 62 | $9.08 | +13 pts, 30% cheaper | The sweet spot | 
| Fable 5.2 (xhigh) | 62 | $14.83 | +13 pts, 14% dearer | Same score as high, 63% more money | 
| Fable 5.2 (max) | 65 | $26.54 | +16 pts, 2.0× dearer | Best score on any model; pricey | 
| Reference: Opus 5.5 (max) | 59 | $10.04 | +10 pts | Cheaper than 5.2 max | 
| Reference: Gemini 4 Carbon (high) | 60 | $7.76 | +11 pts | Best value at the top end | 
| Reference: GPT-6 Astra (max) | 57 | $6.24 | +8 pts | Cheapest frontier point | 

Read the middle rows carefully, because they are the practical story. **Fable 5.2 at high effort scores 13 points above Fable 5.1 at max while costing 30% less per task.** The xhigh setting buys the same 62 for $14.83, and max buys 3 more points for $26.54 — nearly triple. If you are moving off Fable 5.1 and you leave the effort dial wherever it was, you may spend more than before and get less than you could.

**AA Intelligence Index v5 by effort setting.**Fable 5.1 is a single point (AA reports its max run at 49). The plateau between high and xhigh is the most useful detail on this chart.

**Cost per Intelligence Index task, USD.**The same ordering, inverted: at low effort Fable 5.2 costs $3.10, roughly a quarter of what Fable 5.1 cost per task.

**Score against cost per task (linear scale).**Each dot is one effort setting; Fable 5.2's five settings are joined by the line. The max setting sits far off the useful frontier: $26.54 buys three points more than the $9.08 high setting.

## Which evaluations actually improved

Fable 5.2 sets the highest score Artificial Analysis has recorded on five of the nine index evaluations, and every one of the five is a science, knowledge or coding-quality test rather than an agentic-workflow test:

| AA evaluation (max effort) | Fable 5.2 | Previous best | What it measures | 
|---|---|---|---|
| HLE-Diamond | 80.2% | 69% (Gemini 4 Carbon) | Hard knowledge and reasoning, tools allowed. Anthropic's own HLE-with-tools row is 74.2%. | 
| AA-Omniscience v1.1 | 67 | 54 (Gemini 4 Carbon) | Knowledge reliability: correct answers minus hallucinations, refusals unpenalised. | 
| Terminal-Bench-Science 0.1 | 73% | 65% (Gemini 4 Carbon) | Agentic scientific research inside a terminal. | 
| CritPt v2 (hard subset) | 76.5% | 72% (GPT-6 Astra) | Frontier physics reasoning. | 
| FrontierCode 2.0 | 48.8% | 46% (Claude Opus 5.5) | Whether a patch would actually be merged by maintainers. | 

And here is where it does *not* lead, which is just as important if you are choosing a model for agent work:

| AA evaluation (max effort) | Fable 5.2 | Best in class | Read | 
|---|---|---|---|
| Terminal-Bench 4.0 | 59.6% | 67% (Kimi K3.1 Pro) | Level with Opus 5.5 (59.6%). Gemini 4 Carbon 65%, Sonnet 5.5 64%. | 
| AA-Briefcase v1.1 (Elo) | 1785 | 1813 (Sonnet 5.5) | Inside the confidence interval of Opus 5.5 (1800). No premium paid for Fable here. | 
| GDP.pdf (all-pass) | 24.2% | 26.6% (Opus 5.5 xhigh) | Long professional-document reasoning. Slightly behind its cheaper sibling. | 
| AutomationBench-AA | 76.4% | 78% (Gemini 4 Carbon) | Ahead of Opus 5.5 (70%), narrowly behind Gemini. | 

**The pattern:** Fable 5.2 is a science and knowledge model that can code extremely well, not an agentic-workflow model that happens to know physics. Where the task rewards deep reasoning over a hard artefact — a proof, a kernel, a merged patch, a chart read correctly — it is clearly ahead. Where the task rewards long tool-driven loops with many cheap steps, the gap to Opus 5.5 and to cheaper rivals mostly disappears.

## Anthropic's own benchmark table, with the deltas

These are vendor-reported numbers from the launch post, with safeguards on and each model at its highest available effort unless noted. They have not been independently reproduced except where AA ran the same benchmark (FrontierCode 2.0, Terminal-Bench-Science, AutomationBench, AA-Briefcase, OSWorld).

| Benchmark | 5.1 | 5.2 | Δ | Opus 5.5 | GPT-6 Astra | Gemini 4 Carbon | 
|---|---|---|---|---|---|---|
| FrontierCode 2.0 agentic coding, mergeability | 32.4% | 49.1% | +16.7 | 45.5% | 42.8% | 41.1%medium effort | 
| FrontierSWE v2 repository-level SWE | 56.3% | 69.9% | +13.6 | 62.3% | 65.5% | 61.2% | 
| AutomationBench business workflows | 31.4% | 51.4% | +20.0 | 42.5% | 41.4% | 53.9% | 
| Terminal-Bench-Science 0.1 scientific research | 52.6% | 74.6% | +22.0 | 58.7% | 68.1% | 64.8% | 
| OSWorld 2.1 computer use, offline partial set | 83.5% | 89.3% | +5.8 | 87.2% | 79.5% | 81.5% | 
| RoboDojo Sim simulated two-arm robot, internal | 27.6% | 64.9% | +37.3 | 39.4% | 29.8% | — | 
| WANDR agentic search, relaxed score | 68.7% | 82.1% | +13.4 | 72.3% | — | — | 
| AA-Briefcase v1.1 knowledge work, rubric | 61.5% | 66.1% | +4.6 | 61.2% | 52.0% | 61.8% | 
| AA-Briefcase v1.1 Elo | 1669 | 1785 | +116 | 1800 | 1569 | 1611 | 
| Remote Labor Index freelance-style work | 17.9% | 41.3% | +23.4 | 21.3% | 20.8% | — | 
| Humanity's Last Exam with tools | 65.6% | 74.2% | +8.6 | 67.7% | 57.2% | — | 
| Chartography chart reading, no tools | 44.8% | 73.4% | +28.6 | 64.4% | 71.0% | 72.5% | 

**Vendor-reported capability radar, % scores.**Every axis is the same 0–100 scale, so the shape is directly comparable. The robotics and chart-reading axes are where the 5.1 outline collapses inward.

**Fable 5.1 → 5.2, percentage points.**Robotics, chart reading, knowledge work and science take the top four slots. FrontierCode 2.0 is the best coding gain at +16.7 points.

Two rows deserve a caveat. **RoboDojo Sim** is an internal simulation run — it is evidence of improvement, not of parity with purpose-built robot controllers on physical hardware, and Anthropic says as much. **OSWorld 2.1** is an offline, partial evaluation set, so 89.3% is not a universal desktop-task success rate.

**Coding and computer-use rows, grouped.**Fable 5.2 wins all four against Fable 5.1 and all four against Opus 5.5. Gemini 4 Carbon's numbers are quoted at medium effort, which flatters its AutomationBench result.

**Science, knowledge and research rows.**These are the rows that moved the AA index: Terminal-Bench-Science at 74.6% is 22 points above 5.1 and 6.5 above GPT-6 Astra.

## Speed: three claims, three different numbers

Anthropic says Fable 5.2 generates output "up to 30% faster" than Fable 5.1. Artificial Analysis measures output speed at about 98 tokens per second for Fable 5.2 (max) against 88 for Fable 5.1 (max) — roughly 11% faster at the top setting. Both can be true: the launch claim is a best case across Anthropic's own evaluations, and the AA figure is an end-to-end average including a long thinking phase.

What matters more in practice is wall-clock time per task, and there Fable 5.2 is *slower* at max effort: about 44 minutes per AA index task versus 22 minutes for Fable 5.1 at max. It thinks more, so it finishes later even while emitting tokens faster. This is the trade the effort dial makes explicit, and it is why the high setting is the interesting one.

| Speed signal | Fable 5.1 (max) | Fable 5.2 (max) | Fable 5.2 (fast mode) | 
|---|---|---|---|
| Output speed | 88 tok/s | 98 tok/s | Up to 2.5× standard | 
| Time per AA task | 22.2 min | 44.1 min | Not benchmarked by AA | 
| Price multiplier | 1× | 1× (20% cheaper than 5.1) | 2× standard ($16 / $80) | 

**Fast mode is not available on the Batch API** and it stacks with cache multipliers. For an interactive coding agent where a human is waiting, 2.5× is often worth 2× the tokens. For an overnight batch job it is money burned.

## The enterprise demos, and the capability they deliberately withheld

The launch post leans on four demos rather than benchmark rows, and they are worth understanding because they show the intended shape of the model. All are company-reported.

| Demo | What Fable 5.2 did | Why it is notable | 
|---|---|---|
| Operating system from scratch | Coordinated 1,000+ subagents over eight days from an empty folder; produced a bootable OS with drivers, filesystem, window manager and installer. | Long-horizon orchestration with self-directed planning, not single-session coding. | 
| Fermat's Last Theorem formalisation | 1,000 agents fully formalised the proof in 30 hours, with minimal human input, using one-third the action tokens of Anthropic's first attempt and producing a proof 7× shorter. | Opus 5.5 with the same setup completed 26% in 18 hours and then stalled. | 
| Two open problems in black-hole physics | Completed a 2003 calculation in small-universe holography and found the exact limit of a 15-year-old technique; two preprints released. | Largely autonomous mathematical research, with published artefacts you can check. | 
| Hardware design | Designed three working electronic devices end-to-end: four-layer boards routed, enclosures CAD-modelled, firmware written, all working without electrical fixes. | Cross-domain work in a single session, with Diode fabricating the boards. | 

Buried in the announcement is the more consequential admission: during training, Anthropic **deliberately limited** progress on accelerator kernel optimisation and on improving LLM architectures, so that in those specific areas Fable 5.2 performs at about Opus 5.5's level. Anthropic states the training changes did not touch agentic coding, hardware development or interpretability research, and that it does not cap these abilities at inference time. If your workload *is* kernel tuning or model-architecture search, read that paragraph before you buy an upgrade.

### Safety and safeguards at a glance

| Signal | Fable 5.1 / Mythos 5.1 | Fable 5.2 / Mythos 5.2 | 
|---|---|---|
| False-positive blocks (early testing) | Baseline | ~55% fewer (60% cyber, 40% biology) | 
| Sandbox-escape eval (credential use outside sandbox) | 21% (Mythos 5.1) | 3.1% (Mythos 5.2) | 
| Blocked cyber request fallback | Opus 4.8 / Opus 5 | Same routing | 
| Anti-distillation defences | Standard | Strongest yet; 90–99% less data harvested | 
| External pre-release testers | UK AISI, METR and others | UK AISI, Frontier Design, METR | 
| Cyber / bio benchmark lift | CyScenarioBench 61.7% (Mythos) | 76.3%; CVE Rediscovery 89.6% vs 78.3% | 

Those cyber and bio scores belong to Mythos 5.2, the same weights with lighter guardrails, available only to vetted organisations through Anthropic's Cyber and Life Sciences Verification Programs. Fable 5.2's classifiers will block most of that work, as they did on 5.1.

## Migrating: what actually changes in your code

The migration is mostly a model-ID change, but three details are easy to get wrong: the effort default, the cache-pricing line, and the retention requirement.

| Task | Change | 
|---|---|
| Model ID | `claude-fable-5-1` →`claude-fable-5-2` . Bedrock:`anthropic.claude-fable-5-2` . | 
| Effort | API default is `high` ; Claude apps and Claude Code default to`medium` . If you were relying on an old implicit setting, pin`output_config.effort` explicitly. | 
| Thinking | Adaptive and always on; there is no non-thinking mode and no budget-token parameter to tune. | 
| Cache economics | Cache read drops from $0.25 to $0.20 per MTok, writes from $12.50/$20 to $10/$16. Agents that resend a stable prefix get a larger saving than the headline 20%. | 
| Speed | Optional `speed="fast"` on the Messages API at $16/$80. On Microsoft Foundry fast mode is a separate deployment,`claude-fable-5-2-fast` . | 
| Data handling | 30-day retention required for safety monitoring. ZDR requires Enterprise Frontier Safeguards (private beta) or explicit authorisation. | 

```
import anthropic
client = anthropic.Anthropic()
resp = client.messages.create(
    model="claude-fable-5-2",
    max_tokens=64_000,
    output_config={"effort": "high"},   # low | medium | high | xhigh | max
    messages=[{"role": "user", "content": "Refactor this module to use dependency injection"}],
)
print(resp.usage)
```
## Routing: which setting to buy

| Your workload | Pick | Why | 
|---|---|---|
| Hard but routine coding, most tickets | Opus 5.5 | Anthropic recommends it as the daily driver. On AA it scores 59 at $10.04, and on Terminal-Bench 4.0 it matches Fable 5.2. | 
| Multi-repo refactors, migrations, merged-quality patches | Fable 5.2 at high | FrontierCode 2.0 45.3% at high, level with Opus 5.5 at max for roughly half the cost per task (Anthropic's testing). | 
| Maximum score on a hard artefact, cost secondary | Fable 5.2 at max or xhigh | Only setting that reaches 65 on AA. Budget ~$26 per index task and ~44 minutes of wall clock. | 
| Long agent loops with many cheap steps | Kimi K3.1 Pro, Gemini 4 Carbon or Sonnet 5.5 | Fable 5.2 is level with Opus 5.5 on Terminal-Bench 4.0 (59.6%) and behind all three on cost. | 
| Scientific research, physics, formal proofs | Fable 5.2 at xhigh | Terminal-Bench-Science 74.6%, HLE-Diamond 80.2%, CritPt v2 76.5% — new bests on all three. | 
| Charts, dashboards, figures, PDFs of tables | Fable 5.2 at medium | Chartography 66.4% at medium beats Opus 5.5 at max (64.4%) for under a tenth of the cost per task. | 
| High-volume classification or extraction | Haiku 5.5 / Sonnet 5.5 | Fable 5.2's faintest capability axis is still $8 per million input tokens. | 
| Interactive agent where a human is waiting | Fable 5.2 + fast mode | 2.5× output speed for 2× token cost. Cheaper than a developer idling. | 
| Overnight batch jobs | Fable 5.2 Batch at high | $4 / $20 per MTok is the cheapest Fable-class token ever sold, and batch is where the 20% cut compounds. | 
| Kernel optimisation or LLM architecture research | Opus 5.5 | Fable 5.2's training deliberately capped these at Opus 5.5 level. No reason to pay the premium. | 
| Offensive security, exploit analysis, research biology | Mythos 5.2 (vetted) | Fable 5.2's classifiers block most of this work. Access runs through verification programs. | 

### The short version

**Fable 5.2 is a real generational step, and it is cheaper — but only if you move the effort dial.** From Fable 5.1, the sensible migration is: change the model ID, drop effort from max to *high*, and expect a 13-point index gain for about 30% less per task. The max setting is a research tool, not a default: it costs three times high effort for three points and roughly doubles wall-clock latency.

Against the rest of the field, Fable 5.2 is now the clearest lead on scientific reasoning, hard knowledge and mergeable code quality, and no longer the most expensive token on the market. It has not taken the lead on long tool-driven agent loops, where Opus 5.5, Sonnet 5.5, Gemini 4 Carbon and Kimi K3.1 Pro all offer comparable or better results for less money.

The cheapest way to decide: run 200 of your own hard tasks at Fable 5.2 *high* and the same 200 at Fable 5.1 *max*, and compare total spend against accepted-output count. The list price will tell you 20% cheaper; the effort plateau will tell you the rest.

## FAQ

## Is Fable 5.2 actually cheaper than Fable 5.1?

Per token, yes: $8/$40 versus $10/$50, a 20% cut on input, output, both cache-write tiers, cache reads and batch. Per *task*, only if you control effort. At max effort AA measures $26.54 per index task against $12.99 for Fable 5.1 at max, because Fable 5.2 thinks for longer. At high effort the same task costs $9.08 — cheaper than the old model — and scores 13 points higher.

## Which effort level should I start with?

High. It scores 62 on the AA index for $9.08, and xhigh buys the identical 62 for $14.83 while max buys 65 for $26.54. Anthropic's own FrontierCode 2.0 testing found high effort level with Opus 5.5 at max for about half the cost per task. Use medium for volume work and xhigh only when a hard artefact needs one more pass.

## Does Fable 5.2 replace Opus 5.5?

Anthropic says no, and the independent numbers support that. On AA, Fable 5.2 lands level with Opus 5.5 on Terminal-Bench 4.0 (59.6%) and slightly behind on GDP.pdf (24.2% vs 26.2%) and AA-Briefcase Elo (1785 vs 1800 and 1813 for Sonnet 5.5). The Fable premium buys science, knowledge and vision: HLE-Diamond 80.2%, AA-Omniscience 67, Chartography 73.4%.

## Why is the AA index jump (49 to 65) larger than any single vendor benchmark gain?

Because the two indices weight different things. The vendor table tops out at +37 points on RoboDojo and +16.7 on FrontierCode, but HLE-Diamond, AA-Omniscience and CritPt v2 carry significant weight in AA's nine-eval index, and those are where Fable 5.2 set outright records. AA also notes the 15-point gap is against Fable 5.1's max run on the same v5 index, so the comparison is like for like.

## Is the "30% faster" claim accurate?

Partly. Anthropic's figure is a best case across its own evaluations. Artificial Analysis measures 98 tok/s versus 88 at max effort, about 11%, and a longer time per task (44 minutes versus 22) because the model spends more time thinking. The reliable speed lever is fast mode, at 2.5× output for 2× the token price.

## Can I use Fable 5.2 with zero data retention?

Not by default. Fable 5.2 requires 30-day retention so Anthropic can run safety monitoring across request windows. Eligible customers can keep ZDR by adopting Enterprise Frontier Safeguards, currently in private beta, which runs the monitoring inside the customer's own infrastructure. Check this before moving regulated workloads.

## Is Fable 5.1 being retired?

It is marked legacy and Anthropic recommends migrating, but no retirement date has been announced. Fable 5.2 itself is committed to availability until at least 9 November 2027. The two models share a 1M context window, 128K max output and a June 2026 knowledge cutoff, so the API surface is otherwise identical.

## Sources

- Anthropic — Introducing Claude Fable 5.2 and Claude Mythos 5.2: the vendor benchmark table, effort/efficiency claims, safety testing, the kernel-optimisation cap, fast mode, availability and the retention policy.
- Claude Platform docs — Fable 5.2 overview: 1M context, 128K output, adaptive thinking, default effort, June 2026 cutoff, pricing, retirement window.
- Claude Platform docs — Fable 5.1 (legacy): the previous generation's specs and price.
- Claude Platform docs — Pricing: full rate card for both models, cache multipliers, fast-mode and batch pricing.
- Artificial Analysis — Claude Fable 5.2: Intelligence Index v5.0.1 score, cost per task, output speed, token use.
- Artificial Analysis — Claude Fable 5.1: the 49 / $12.99 / 88 tok/s baseline figures.
- Artificial Analysis — launch thread: the five evaluations where Fable 5.2 set records, Terminal-Bench 4.0, GDP.pdf, AA-Briefcase Elo, AutomationBench and the 98k-vs-173k token note.
- R&D World — Fable 5.2 index score and cost per task: the full five-setting effort ladder with cost per task, read 9 November 2026.
- OfficeChai — Anthropic jumps to top of the AA Intelligence Index: per-evaluation scores and the FrontierCode high-effort note.
- Emergent — Fable 5.2 benchmarks: transcribed vendor tables, effort-level efficiency claims and the WANDR/Chartography effort notes.
- Eigent — Fable 5.2 and Mythos 5.2 explained: the two-tier safeguard split, the Anthropic guidance to start from Opus 5.5, and the 90%-of-merges context.
- The Decoder — Fable 5.2 and Mythos 5.2: the AA ranking summary and the deliberate capability-limiting decision.
- 9to5Mac — Anthropic upgrades Claude with Fable 5.2: the Opus-5.5-first guidance and the Python-to-Rust migration claim.

Vendor scores are Anthropic-reported, taken at each model's highest available effort with safeguards on unless labelled otherwise; Gemini 4 Carbon's FrontierCode row is at medium effort, Fable 5.2's Remote Labor Index row is at high effort and its FrontierCode row at xhigh. AA scores are independent runs in Intelligence Index v5.0.1. Prices are USD per million tokens, November 2026.
