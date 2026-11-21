---
id: collect-261121-veille-ia/claude-anthropic/claude-fable-5-1-vs-opus-5-vs-sonnet-5-reading-anthropic-s-september-2026-lineup-claude-reports
title: "Claude Fable 5.1 vs Opus 5 vs Sonnet 5: Reading Anthropic's September 2026 Lineup | Claude Reports"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["fable", "claude", "opus", "anthropic", "model", "sonnet", "september", "announcement"]
source: https://www.claudereports.com/reports/claude-fable-5-1-vs-opus-5-vs-sonnet-5/
source_anchor: ""
source_lines: [1, 77]
section: "Claude_Anthropic"
---
Citable baseline

## Key facts

**Sep 1, 2026**Claude Fable 5.1 and Mythos 5.1 launch date. Fable 5.1 is Anthropic's most capable generally available model.Primary source: Anthropic

"Selecting the optimal Claude model involves balancing three key considerations: capabilities, speed, and cost."

## Anthropic's published comparison from the Fable 5.1 announcement

| Eval | Fable 5.1 | Fable 5 | Opus 5 | 
|---|---|---|---|
| **Terminal-Bench-Science 0.1** | 52.6% | 24.7% | 29.0% | 
| **Terminal-Bench 4.0** | 55.8% (Mythos 5.1: 60.9%) | 42.0% | 52.3% | 
| **GDPval-AA v2** | 1853 | 1723 | 1824 | 
| **OSWorld 2.0 (partial / strict)** | 77.9% / 41.7% | 72.9% / 36.1% | 75.4% / 39.6% | 
| **HLE (no tools / with tools)** | 60.9% / 65.0% | 57.8% / 63.8% | 56.6% / 63.6% | 
| **AutomationBench** | 31.4% | 17.1% | 26.9% | 
| **CursorBench 3.2.0** | 73.4% | 70.5% | 70.0% | 

Reproduced from Anthropic's launch announcement. These are provider-run numbers. The announcement does not publish the agent scaffold, effort setting, attempt budget, or confidence interval for each row, and it contains no GPT, Gemini, or open-model column. Do not read it as a cross-vendor bake-off.

## The lineup as of September 19, 2026

Three claims that were accurate in July are now wrong. Claude Opus 4.8 is not the current Opus; Opus 5 launched on July 24, 2026. Claude Fable 5 is not the current flagship; Fable 5.1 launched on September 1, 2026. And Sonnet 5 did not step up to $3/$15 on September 1, which we cover separately in the Sonnet 5 pricing report.

The generally available set is now Claude Fable 5.1 (`claude-fable-5-1`) at $10/$50 per million tokens, Claude Opus 5 (`claude-opus-5`) at $5/$25, Claude Sonnet 5 (`claude-sonnet-5`) at $2/$10, and Claude Haiku 4.5 (`claude-haiku-4-5`) at $1/$5. The first three carry a 1M-token context window and 128K max output with adaptive thinking; Haiku 4.5 is listed at 200K context and 64K output.

Anthropic's own selection guidance is unusually direct for a vendor: start with Opus 5 for most workloads, move to Fable 5.1 for demanding reasoning and long-horizon agentic work or when evaluations on Opus 5 at higher effort still fall short, use Sonnet 5 for the speed and intelligence balance, and use Haiku 4.5 when speed and price dominate. That is a routing ladder, not a leaderboard, and it is the correct shape for the decision most teams are actually making.

Mythos 5.1 appears in the announcement table and is worth naming precisely: it is the same underlying model as Fable 5.1 with more permissive safeguards, available only through trusted access. It is not a model most readers can buy, so its 60.9% Terminal-Bench 4.0 row should not enter a procurement comparison.

## What the announcement table shows

Fable 5.1 leads Opus 5 on every row Anthropic published, but the size of the lead varies enormously by task family. On Terminal-Bench-Science 0.1 the gap is 52.6% against 29.0%, which is close to a doubling. On CursorBench 3.2.0 it is 73.4% against 70.0%, which is small enough that harness noise and run-to-run variance could plausibly account for much of it without an interval to check against.

The generational comparison is more informative than the tier comparison. Fable 5 scores 24.7% on Terminal-Bench-Science 0.1 and 17.1% on AutomationBench, while Opus 5 — a cheaper, lower-tier model released seven weeks earlier — scores 29.0% and 26.9%. On several agentic rows the newer mid-tier model beats the older top-tier model. That is a strong argument against treating "flagship" as a durable capability label, and a strong argument for re-running your evals on release rather than inheriting last quarter's tier ordering.

GDPval-AA v2 is reported as an Elo-style rating rather than a percentage: 1853 for Fable 5.1, 1824 for Opus 5, 1723 for Fable 5. Ratings and accuracies do not combine, so this row cannot be averaged into a single headline with the others. Anthropic does not attempt that, and neither should a secondary report.

## Why this table is not a bake-off

The most important caveat is structural: there is no competitor column. Anthropic's table compares three Anthropic models. It makes no claim about GPT-5.6 Sol, GPT-5.5, or any non-Anthropic system, and no cross-vendor conclusion can be extracted from it. Any article that places a number from this table beside an OpenAI-reported number and declares a winner has manufactured a comparison that neither provider published.

The second caveat is harness. Terminal-Bench, OSWorld, and AutomationBench are agent benchmarks: the score reflects a model plus a scaffold plus tool access plus a stopping policy. The Terminal-Bench authors state plainly that agent and model performance are hard to decouple. The announcement does not publish the scaffold used for each row, so the numbers describe a system Anthropic built and did not fully specify.

The third caveat is effort. Fable 5.1 uses adaptive thinking that is always on, with a default effort of high, but the platform docs note the default differs by surface — high in Claude Code, medium on Claude.ai and Cowork. A benchmark row and a production deployment can therefore run the same model at different operating points. Comparing a high-effort benchmark score against a medium-effort production experience is a common and expensive error.

The fourth caveat is versioning. Terminal-Bench 4.0 is not Terminal-Bench 2.0, and OSWorld 2.0 is not OSWorld-Verified. Scores across benchmark versions are not comparable even when the names rhyme. We expand on this in the Claude vs GPT coding report.

## How to route between the three

Treat Opus 5 as the default and make Fable 5.1 earn its 2x price on your tasks. The escalation trigger Anthropic itself names — Opus 5 at higher effort still falling short of your evaluation bar — is a better rule than "use the biggest model," because it forces you to have an evaluation bar first.

Cost may pull harder than capability here. Anthropic prices Fable 5.1 cache reads at 2.5% of base input, or $0.25 per million tokens, against the 10% cache-read rate on most other models. The announcement estimates roughly 25% savings on typical workloads and up to about 45% on highly agentic ones. For a long-running agent that resends a large stable prefix on every turn, the effective gap between Fable 5.1 and Opus 5 is much narrower than the headline $10 against $5 suggests.

Migration is not free. The Fable 5.1 platform overview lists breaking changes against Fable 5: forced tool use now raises errors, earlier models cannot read its thinking blocks, and editing an earlier turn invalidates thinking. If you are running a multi-model pipeline where one model reads another's trace, test that path explicitly before switching.

For high-volume production routing, Sonnet 5 at $2/$10 deserves its own evaluation slot rather than being treated as a degraded Opus. It shares the 1M context window and 128K output ceiling with both larger models, and at a quarter of Fable 5.1's input price it changes what is affordable to attempt at all.

## FAQ

### Which Claude model is current in September 2026?

Claude Fable 5.1 is the most capable generally available model as of its September 1, 2026 launch. Claude Opus 5, released July 24, 2026, is the model Anthropic recommends starting with for most workloads. Sonnet 5 and Haiku 4.5 complete the generally available lineup. Opus 4.8 and Fable 5 are legacy.

### Does Anthropic's Fable 5.1 table compare Claude with GPT-5.6 Sol?

No. The published table has three columns — Fable 5.1, Fable 5, and Opus 5 — and no OpenAI, Google, or open-model row. It supports no cross-vendor claim of any kind.

### Is Fable 5.1 worth twice the price of Opus 5?

Only your evals can answer that. Anthropic's table shows a wide lead on science and automation tasks and a narrow one on some coding rows. Because Fable 5.1 cache reads are priced at 2.5% of base input rather than 10%, cache-heavy agent workloads narrow the real cost gap considerably.

## Cite this page

`Claude Reports. "Claude Fable 5.1 vs Opus 5 vs Sonnet 5: Reading Anthropic's September 2026 Lineup." Updated 2026-09-19. https://claudereports.com/reports/claude-fable-5-1-vs-opus-5-vs-sonnet-5/`
