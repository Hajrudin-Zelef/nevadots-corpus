---
id: collect-261121-veille-ia/claude-anthropic/claude-fable-5-2-deep-analysis-capabilities-benchmarks-pricing-and-model-comparison
title: "Claude Fable 5.2 Deep Analysis: Capabilities, Benchmarks, Pricing and Model Comparison"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["fable", "claude", "model", "output", "tokens", "anthropic", "input", "benchmarks"]
source: https://dev.to/modelflare_dev/claude-fable-52-deep-analysis-capabilities-benchmarks-pricing-and-model-comparison-2cl4
source_anchor: ""
source_lines: [1, 243]
section: "Claude_Anthropic"
---
A deep analysis of Claude Fable 5.2: 1M context, $8/$40 pricing, published benchmarks against Fable 5.1, Opus 5.5, GPT-6 Astra and Gemini 4 Carbon, cost per task, API breaking changes and Modelflare routes.

Claude Fable 5.2 is Anthropic's most capable widely released model, launched on **November 9, 2026** for demanding reasoning and long-horizon agentic work. It keeps the 1M-token context window, 128K maximum output and always-on adaptive thinking of Claude Fable 5.1. It costs 20% less per token, at **$8 per million input tokens and $40 per million output tokens**. Anthropic reports large gains in agentic coding, science, robotics, search and chart reading, and outputs up to 30% faster than Fable 5.1.

**Short answer:** Fable 5.2 is the strongest general-purpose Claude model you can call through the API without a verification program. On Anthropic's published table it beats Fable 5.1, Claude Opus 5.5, GPT-6 Astra and Gemini 4 Carbon on 10 of the 12 benchmarks where Anthropic reports a Fable 5.2 score. Gemini 4 Carbon leads on AutomationBench, and Opus 5.5 has a slightly higher AA-Briefcase Elo. The per-token price is still twice that of Opus 5.5, so the real question is cost per finished task. On several published tasks, Fable 5.2 at a lower effort level matches Opus 5.5's best score for half the cost or less.

On Modelflare, the exact model ID `claude-fable-5-2` is live on three Claude routes, from **$2 / $10 per 1M tokens**. This analysis separates four kinds of evidence: Anthropic's documented specifications, provider-reported benchmarks, Modelflare's live catalog checked on November 11, 2026, and our own engineering interpretation. Modelflare has not run its own benchmark of the model, and nothing below should be read as one.

| Property | Claude Fable 5.2 | 
|---|---|
| Developer | Anthropic | 
| Release date | November 9, 2026 | 
| API model ID | `claude-fable-5-2` | 
| Sibling model | Claude Mythos 5.2 ( `claude-mythos-5-2` ), same model and price, verified organizations only | 
| Context window | 1M tokens (default and maximum) | 
| Maximum output | 128K tokens (synchronous Messages API) | 
| Input and output | Text and image input, text output | 
| Thinking | Adaptive thinking, always on; the raw chain of thought is never returned | 
| Effort levels | `low` ,`medium` ,`high` (default),`xhigh` ,`max` | 
| Reliable knowledge cutoff | June 2026 | 
| Comparative latency | Moderate (Anthropic's lineup rating) | 
| Official price | $8 input / $40 output per 1M tokens | 
| Prompt cache | $10 (5-minute write), $16 (1-hour write), $0.20 (read) per 1M tokens | 
| Batch API | $4 input / $20 output per 1M tokens | 
| Fast mode | $16 input / $80 output per 1M tokens, up to 2.5x faster output | 
| Data retention | 30-day retention required; Covered Model | 
| Retirement | Not sooner than November 9, 2027 | 

Fable 5.2 and Mythos 5.2 are the same underlying model. Fable runs stronger cybersecurity and biology safeguards, which is why it can be available to every Claude API customer. Mythos 5.2 serves organizations admitted to Anthropic's Cyber Verification Program or Life Sciences Verification Program. Several headline security and protein-design results in the launch materials are Mythos 5.2 results. A Fable 5.2 deployment should not expect them, because Fable's safeguards block most cyber tasks.

Fable 5.2 is a successor in the same line, not a new family. Capacity, tokenizer and thinking mode carry over. Price, speed, safeguards and several API contracts change.

| Dimension | Fable 5.2 | Fable 5.1 | Practical consequence | 
|---|---|---|---|
| Input / output price | $8 / $40 | $10 / $50 | 20% lower base cost for the same tokens | 
| Cache reads | $0.20 / 1M | $0.25 / 1M | Same 0.025x multiplier on a lower base | 
| Output speed | Up to 30% faster (Anthropic) | Baseline | Shorter wall-clock time for long outputs | 
| Context / output | 1M / 128K | 1M / 128K | No capacity migration | 
| Safeguard blocks | About 55% fewer overall; about 60% fewer in cyber, about 40% fewer in bio | Baseline | Fewer false positives on legitimate work | 
| Prefix check on thinking blocks | Enforced on every account | Enforced only on accounts created on or after August 31, 2026 | Code that edits history can start failing | 
| Who reads its thinking blocks | Only Fable 5.2 and Mythos 5.2 | Wider set | Reasoning is lost when a router moves to any other model | 
| `computer_20251124` tool | 400 error except on Amazon Bedrock | Accepted with beta header | Move to `computer_toolset_20260801` | 
| Advisor tool | Advisor must be Fable 5.2 or Mythos 5.2 | Looser pairing | Update the advisor `model` with the executor | 

The speed and safeguard figures are Anthropic's own measurements. The four API changes are documented breaking changes and need a code review before a switch. They are covered in detail below.

The table reproduces Anthropic's launch comparison. Scores for Fable 5.2, Fable 5.1 and Opus 5.5 include production safeguards unless noted, and use each model's highest available effort unless noted. These are provider-reported results under Anthropic's harnesses, not independent measurements.

| Benchmark | Fable 5.2 | Fable 5.1 | Opus 5.5 | GPT-6 Astra | Gemini 4 Carbon | 
|---|---|---|---|---|---|
| FrontierCode 2.0 (agentic coding) | **49.1%** | 32.4% | 45.5% | 42.8% | 41.1% | 
| FrontierSWE v2 (agentic coding) | **69.9%** | 56.3% | 62.3% | 65.5% | 61.2% | 
| AutomationBench (business workflows) | 51.4% | 31.4% | 42.5% | 41.4% | **53.9%** | 
| Terminal-Bench-Science 0.1 (agentic science) | **74.6%** | 52.6% | 58.7% | 68.1% | 64.8% | 
| WANDR, relaxed (agentic search) | **82.1%** | 68.7% | 72.3% | — | — | 
| OSWorld 2.1, offline partial (computer use) | **89.3%** | 83.5% | 87.2% | 79.5% | 81.5% | 
| RoboDojo Sim (robotics, internal) | **64.9%** | 27.6% | 39.4% | 29.8% | — | 
| AA-Briefcase v1.1, rubric (knowledge work) | **66.1%** | 61.5% | 61.2% | 52.0% | 61.8% | 
| AA-Briefcase v1.1, Elo (knowledge work) | 1785 | 1669 | **1800** | 1569 | 1611 | 
| Remote Labor Index (knowledge work) | **41.3%** | 17.9% | 21.3% | 20.8% | — | 
| Humanity's Last Exam, with tools | **74.2%** | 65.6% | 67.7% | 57.2% | — | 
| Chartography, no tools (visual charts) | **73.4%** | 44.8% | 64.4% | 71.0% | 72.5% | 

Anthropic also published three results where the Fable column is Mythos 5.2 and Opus 5.5 ran without safeguards: CyScenarioBench 76.3% (Mythos 5.1: 61.7%, Opus 5.5: 67.6%), CVE Rediscovery 89.6% (78.3%, 81.3%) and 24-hour de novo binder design 85.1% (79.1%, 82.6%). CyScenarioBench tasks differ by lab, so its cross-lab column is not comparable. We keep these rows out of the main table because a Fable 5.2 caller will not see Mythos behavior on cyber and biology work.

**The largest jumps are in long, open-ended agentic work.** Against Fable 5.1, Remote Labor Index more than doubles (17.9% to 41.3%), and RoboDojo Sim rises from 27.6% to 64.9%. Terminal-Bench-Science gains 22.0 points, AutomationBench 20.0 points and FrontierCode 2.0 16.7 points. These benchmarks reward a model that keeps a plan coherent over many steps, recovers from failed tool calls and finishes a deliverable. That matches Anthropic's qualitative claims about multi-day sessions.

**Visual reasoning is the surprise.** Chartography rises 28.6 points over Fable 5.1, to 73.4%. That narrowly passes Gemini 4 Carbon (72.5%) and GPT-6 Astra (71.0%), two models with strong vision records. For document-heavy pipelines with dense charts and tables, this is the single most consequential change in the table.

**Computer use is near its ceiling on this benchmark.** OSWorld 2.1 offline partial goes from 83.5% to 89.3%, but Opus 5.5 already scores 87.2%. A 2.1-point lead over Opus 5.5 is unlikely to justify a doubled per-token price for routine browser automation.

**Fable 5.2 does not lead everywhere.** Gemini 4 Carbon leads AutomationBench by 2.5 points. On AA-Briefcase, Fable 5.2 wins the rubric score (66.1% against 61.2%) but trails Opus 5.5 on Elo (1785 against 1800). In other words, graders' rubrics favored Fable's deliverables, while pairwise preference slightly favored Opus 5.5. For business documents judged by human taste, the two models are close, and Opus 5.5 costs half as much per token.

Anthropic published several matched-score comparisons that change the pricing conversation:

| Task | Fable 5.2 setting | Result against Opus 5.5 | 
|---|---|---|
| FrontierCode 2.0 | `high` effort: 45.3% | Matches Opus 5.5 at `xhigh` (45.5%) at about half the cost per task | 
| WANDR | `medium` effort | Matches Opus 5.5's top score at about 40% of the cost | 
| Humanity's Last Exam | `low` effort | Matches Opus 5.5's top score at under one-tenth of the cost | 
| Chartography | `medium` effort: 66.4% | Beats Opus 5.5 at `max` (64.4%) | 
| BenchCAD 2.1 | `max` effort: 83.5 | Opus 5.5 65.3, Fable 5.1 44.4 | 

Fable 5.2 costs twice as much per token as Opus 5.5, so "half the cost per task" means it reached the same score with roughly a quarter of the tokens. This is the strongest argument in the release. When a smaller model needs more turns, longer reasoning and more retries, the cheaper per-token price does not make it the cheaper task. Treat these figures as a hypothesis to test on your own workload, not a guarantee. A fair test records successful-task cost, wall-clock time, retries and reviewer corrections (see our guide to cost per successful task).

Anthropic reports that over 90% of changes merged into its core codebase are now written by Claude under human guidance. An earlier Fable 5.2 build migrated a production service from Python to Rust that now starts about 26 times faster. In another internal run, one engineer coordinated over 1,000 subagents for eight days and produced a prototype operating system that boots on a laptop. Launch partners describe the same pattern. Uber ran a 10 to 12 hour security audit in which the model sent out about 170 reviewer agents and returned a write-up, fix pull requests and a policy proposal. Datadog compares it to an incident commander that sends out faster models for quick signals.

The engineering takeaway is that Fable 5.2 works best as a **planner and orchestrator** with clear boundaries, not as a chat model that writes snippets. Anthropic also observed it using subagents to review its own work at `xhigh` and `max`, which Fable 5.1 did not do on the same tasks. Delegation multiplies spend, so check which model your harness assigns to subagents. A Fable 5.2 planner that delegates to Opus 5.5 or Sonnet 5.5 workers is often a better cost structure than Fable 5.2 everywhere.

Anthropic's prompting guide adds a counterintuitive detail about **code ownership**. When the prompt says nothing about ownership, the model tends to add to existing structure and report nearby problems instead of fixing them. When the prompt says the codebase is the model's own, it merged repeated code in about half of Anthropic's small-task trials, against under a tenth without that sentence. It also used about 1.5 times the output tokens and made more unrequested changes. Choose the framing on purpose, and review the diff rather than only the model's summary.

The launch materials contain the most ambitious claims Anthropic has published for a generally available model:

- 
**Mathematics:** a team of 1,000 Fable 5.2 agents formalized Fermat's Last Theorem in 30 hours with minimal human intervention. The first formalization took 11 days. Opus 5.5 in the same setup finished 26% in 18 hours and barely progressed after 30.
- 
**Physics:** Fable 5.2 solved two open problems on black holes using holographic duality.
- 
**Hardware:** with Diode, it designed three working electronic devices, a voice-assistant hub, a USB speakerphone and a handheld virtual pet. All worked without electrical modifications, and each board was manufacturing-ready in under a day.
- 
**Scientific computing:** Discovered Materials reports that it made Quantum ESPRESSO's phonon code about 36 times faster on average, 3 to 4 times more than Fable 5.1's speedup.

These are case studies, not controlled benchmarks. Their value is directional: the model can sustain very long, verifiable work when a checker exists, such as a proof assistant, a circuit simulator or a test suite. The common factor is an external verifier. Build that verifier into your own long tasks before you trust long unattended runs.

The largest knowledge-work gain is on Remote Labor Index, which is built from real remote freelance projects. Fable 5.2 scores 41.3% at `high` effort against 21.3% for Opus 5.5. Partner reports point the same way. Fundamental Research Labs reported 91% on its evaluation against 86% for the next-best model, and Jump Trading reported 10% lower error than the next-best model on an internal benchmark. Harvey says that in an I-9 compliance review, Fable 5.2 gave clearer, more actionable legal direction than Fable 5.1. Balyasny and Cognition both mention restraint: the model admits uncertainty instead of overstating evidence.

For research pipelines, the practical risk remains evidence drift over long chains. Require source links, dates and a short "what would change this conclusion" section. Anthropic's guide also recommends prompt text for self-contained final reports, because readers of a long agent run have not seen its tool calls.

Chartography and OSWorld 2.1 both improve. The useful applications are dense financial statements, scientific figures, dashboards, design review and browser workflows. Two cautions apply. First, the OSWorld lead over Opus 5.5 is small. Second, computer use now requires the `computer_toolset_20260801` toolset on every platform except Amazon Bedrock. Keep irreversible actions behind application-level authorization and a human stop point.

| Dimension | Claude Fable 5.2 | Claude Opus 5.5 | Claude Sonnet 5.5 | Claude Haiku 5.5 | GPT-6 Astra | 
|---|---|---|---|---|---|
| Positioning | Hardest reasoning and long-horizon agents | Default for most agentic coding and knowledge work | Best speed and capability balance | High-volume, latency-sensitive tasks | OpenAI frontier model | 
| Official input / output per 1M | $8 / $40 | $4 / $20 | $2 / $10 | From $0.10 / $0.50 | $10 / $50 up to 272K input; $20 / $75 above | 
| Cache read per 1M | $0.20 | $0.20 | $0.10 | $0.01 up to 100K | $1 (standard tier) | 
| Context / max output | 1M / 128K | 1M / 128K | 1M / 128K | 1M / 128K | 1.05M / 128K | 
| Default effort | `high` | `medium` | `high` | `medium` | Provider-specific | 
| Latency rating | Moderate | Moderate | Fast | Fastest | — | 
| Thinking can be turned off | No | No | Only via `between_tools` | Yes, at `high` or lower | Provider-specific | 

Claude prices are Anthropic's first-party list prices. GPT-6 Astra prices are the official tiers recorded in Modelflare's live catalog on November 11, 2026; above 272K input tokens the higher rates apply to the whole request. Gemini 4 Carbon appears in Anthropic's benchmark table but is not on Modelflare, so we compare it on benchmarks only.

Three observations follow from the table:

1. 
**Fable 5.2 against Opus 5.5 is the main decision.** Anthropic itself recommends starting with Opus 5.5 and moving to Fable 5.2 only when evals at higher Opus effort still fall short. Opus 5.5 costs half as much per token and matches or nearly matches Fable 5.2 on computer use and human-preference knowledge work. Fable 5.2 pulls ahead on long, hard, verifiable tasks, and the matched-score results show it can be cheaper per task there.
2. 
**Against GPT-6 Astra, Fable 5.2 is both stronger on the published table and cheaper per token.** The table shows Fable 5.2 ahead of GPT-6 Astra on every benchmark where both appear. Its list price is 20% lower for input and output up to 272K tokens, and its 1M context is billed at one rate. Cache reads are a fifth of Astra's standard rate. The benchmark claims come from Anthropic, so validate them on your own tasks.
3. 
**Sonnet 5.5 and Haiku 5.5 are partners rather than competitors.** A common production pattern is Fable 5.2 for planning and review, Opus 5.5 or Sonnet 5.5 for implementation, and Haiku 5.5 for classification, extraction and routing. Anthropic's lineup is built for this kind of division of labor (see our analysis of Haiku 5.5).

Fable 5.2's prompt cache is unusually cheap relative to its base price. A cache read costs 0.025 times the base input price, compared with 0.05 on Opus 5.5 and Sonnet 5.5 and 0.1 on most other models. Thinking tokens are billed as output tokens at every `display` setting, and Fable 5.2 thinks before almost every reply at `high` effort and above.

Modelflare serves `claude-fable-5-2` on the Anthropic-compatible and OpenAI-compatible endpoints through three public routes. The prices below are user-facing Modelflare prices from the live catalog on November 11, 2026, in USD per 1M tokens. The pricing page is the source of truth.

| Route | Multiplier | Input | Output | Cache read | Positioning | 
|---|---|---|---|---|---|
| `claude-premium` | 0.5x | $4.00 | $20.00 | $0.10 | Continuity and request success first | 
| `claude-stable` | 0.315x | $2.52 | $12.60 | $0.063 | Everyday development and production | 
| `claude-award` | 0.25x | $2.00 | $10.00 | $0.05 | Lowest price; retryable development and testing | 

On the same routes, Opus 5.5 costs half these amounts and Fable 5.1 costs 25% more.

Take 100K uncached input tokens and 10K output tokens, including thinking, and leave out cache, retries and tool fees:

| Option | Calculation | Cost | 
|---|---|---|
| Fable 5.2, official | 0.1 × $8 + 0.01 × $40 | $1.20 | 
| Fable 5.1, official | 0.1 × $10 + 0.01 × $50 | $1.50 | 
| Opus 5.5, official | 0.1 × $4 + 0.01 × $20 | $0.60 | 
| Fable 5.2, `claude-premium` | 0.1 × $4 + 0.01 × $20 | $0.60 | 
| Fable 5.2, `claude-stable` | 0.1 × $2.52 + 0.01 × $12.60 | $0.378 | 
| Fable 5.2, `claude-award` | 0.1 × $2 + 0.01 × $10 | $0.30 | 

Assume a 200K-token prefix (system prompt, tools and repository context) that is written to the cache once, for example by a pre-warm request, and then read on each of 30 turns. Each turn adds 5K new input tokens and 3K output tokens. For simplicity, the prefix does not grow. At official prices:

| Component | Calculation | Cost | 
|---|---|---|
| One 5-minute cache write | 0.2 × $10 | $2.00 | 
| 30 cache reads | 30 × 0.2 × $0.20 | $1.20 | 
| New input | 30 × 0.005 × $8 | $1.20 | 
| Output | 30 × 0.003 × $40 | $3.60 | 
| **Total with caching** |  | **$8.00** | 
| Same session, no cache hits | 30 × 0.205 × $8 + $3.60 | $52.80 | 

On `claude-stable`, the cached session costs about $2.52. Output becomes the largest cost once the cache works, so effort level and verbosity matter more than input size. A cache that silently misses multiplies the bill about 6.6 times in this example. Our guides to prompt caching cost and agent cache hit rates explain how to measure it.

Anthropic lists four changes that can break code moving from Fable 5.1:

1. 
**The prefix check now applies to every account.** If anything before a Fable 5.2 thinking block changes, such as the`system` prompt, the`tools` list or an earlier message, a request that sends that block back fails with a 400 error. Keep the history append-only, return each assistant turn exactly as received, and use mid-conversation system messages or compaction on demand instead of editing earlier turns. The`drop_block` setting under the`thinking-binding-controls-2026-08-01` beta avoids the error, but the model then thinks again and output tokens rise.
2. 
**Only Fable 5.2 and Mythos 5.2 read Fable 5.2 thinking blocks.** A router or fallback that moves a conversation to Opus 5.5, Fable 5.1 or anything else loses the earlier reasoning for those turns. Blocks are also bound to the account that produced them.
3. 
**`computer_20251124` returns a 400 error** everywhere except Amazon Bedrock. Declare`computer_toolset_20260801` instead.
4. 
**A Fable 5.2 executor needs a Fable 5.2 or Mythos 5.2 advisor** when you use the advisor tool. Change the advisor`model` only in new conversations, because it is an edit to`tools` .

The rules shared with Fable 5.1 still apply. Adaptive thinking cannot be disabled, and `budget_tokens` returns a 400 error. Forced tool use (`tool_choice` of `any` or `tool`) is rejected, so use `auto` with `strict: true` or structured outputs. Assistant prefill is not supported. `temperature`, `top_p` and `top_k` are rejected.

Handle refusals explicitly. A declined request returns HTTP 200 with `stop_reason: "refusal"` and a `stop_details.category` of `cyber`, `bio`, `frontier_llm`, `reasoning_extraction` or `general_harms`. Anthropic's permitted fallback targets for Fable 5.2 are Claude Opus 4.8 and Claude Opus 5. Blocked cyber queries fall back to Opus 4.8, and blocked bio queries to Opus 5. Your billing and retry code must tell a refusal apart from a transport error.

```
export MODELFLARE_API_KEY='YOUR_MODELFLARE_API_KEY'
curl -sS https://modelflare.dev/v1/messages \
  -H "x-api-key: ${MODELFLARE_API_KEY}" \
  -H "anthropic-version: 2023-06-01" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "claude-fable-5-2",
    "max_tokens": 16000,
    "messages": [{"role": "user", "content": "Review this migration plan and list the three riskiest steps."}]
  }'
~~~{% endraw %}
Long agent loops that depend on preserved thinking are safest on the Anthropic-compatible {% raw %}`/v1/messages`{% endraw %} endpoint. The OpenAI Chat Completions format has no native field for signed thinking blocks. Before you move production traffic, replay a real multi-turn conversation with tools, streaming, a refusal and a cancellation, and confirm that every turn returns unchanged. Endpoint support in the catalog does not guarantee feature parity for every parameter (see [connecting Claude Code to an Anthropic API gateway](https://modelflare.dev/articles/claude-code-anthropic-api-gateway)).
## Prompting changes that affect cost and quality
Anthropic's prompting guide for Fable 5.2 documents several behavior shifts that appear without any code change:
- **Effort matters, but less above {% raw %}`high`.** Start at `high`. Use `low` for quick, well-specified and high-volume work; Anthropic reports that Fable 5.2 at `low` scores close to Fable 5.1 at higher effort. Use `medium` for interactive coding and chat. Reserve `xhigh` for measured gains on the hardest long tasks, and `max` for ceiling tests. Effort barely changes reply length, so control length in the prompt.
- **Instructions to think less no longer work.** A line such as "answer directly without deliberating" did not stop thinking at `medium` and above. Lower the effort level instead.
- **Turns can end early** with a list of remaining work, especially at `low` and `medium`. Anthropic's suggested "finishing work" prompt halved such endings in a replay, but raised output tokens by about 30%.
- **Long tool runs can be silent.** Progress updates arrive as {% raw %}`thinking`{% endraw %} blocks that are empty at the default display. Set {% raw %}`display: "updates"`{% endraw %} (beta) and render them if users watch the work live.
- **Accumulated caution can stall agents.** The model follows rules in memory files, notes and reviewer messages closely. Remove stale "never" rules and name the steps that need no approval.
- **Old style rules can backfire.** Hard sentence-length limits make prose choppy, and a token budget written into the prompt can raise spend.
## When to choose Fable 5.2, and when not to
**Choose Fable 5.2 when:**
- the task is long, hard and verifiable, such as repository-scale refactors, migrations, formal proofs, scientific computing or multi-source research with a checkable deliverable;
- you are orchestrating other models and need the strongest planner and reviewer;
- your Opus 5.5 evals at {% raw %}`high`{% endraw %} or {% raw %}`xhigh`{% endraw %} still miss the quality bar;
- your pipeline reads dense charts, statements and figures;
- you are on Fable 5.1 today, because Fable 5.2 is cheaper per token and stronger on every published comparison.
**Prefer another model when:**
- the work is routine chat, extraction, classification or short coding, where Opus 5.5, Sonnet 5.5 or Haiku 5.5 hold quality at a fraction of the price;
- latency matters more than depth, because Fable 5.2 thinks before almost every reply at {% raw %}`high`{% endraw %};
- your harness edits conversation history and cannot become append-only yet;
- the workload is cybersecurity or biology research, where Fable's safeguards may block legitimate requests and Mythos 5.2 requires verification;
- you need zero data retention, which Fable 5.2 does not offer without Anthropic's express authorization.
On Modelflare, use {% raw %}`claude-stable`{% endraw %} as the balanced default, {% raw %}`claude-premium`{% endraw %} when a failed long run is expensive, and {% raw %}`claude-award`{% endraw %} for development, evaluation sweeps and retryable batch work.
## Safety, limits and data handling
Anthropic rates high-stakes misalignment risk as low. On an automated behavioral audit of about 1,900 scenarios (lower is better), Mythos 5.2 scored 2.67, against 2.79 for Opus 5.5 and 3.28 for Mythos 5.1. In an adversarial sandbox-escape test, Mythos 5.2 used outside credentials or files in 3.1% of scenarios, compared with 21% for Mythos 5.1. These tests used Mythos 5.2 with reduced safeguards, and Anthropic expects Fable 5.2 to behave similarly. External testers included the UK AI Security Institute and METR.
Anthropic also limited capability gains in accelerator kernel optimization and LLM architecture improvement during training, where performance is about Opus 5.5 level. Anti-distillation measures reduced the data harvested by distillers by 90% to 99%.
Operationally, Fable 5.2 requires 30-day data retention and is a Covered Model. It does not support Priority Tier. Structured outputs are unavailable in organizations that use customer-managed encryption keys. A 1M-token window does not make every position equally retrievable or cheap. The current tokenizer fits about 555K English words in 1M tokens, so budget from usage records, not character counts.
## FAQ
**When was Claude Fable 5.2 released?** Anthropic announced Claude Fable 5.2 and Claude Mythos 5.2 on November 9, 2026. The API model ID is {% raw %}`claude-fable-5-2`{% endraw %}.
**How much does Claude Fable 5.2 cost?** The official price is $8 per million input tokens and $40 per million output tokens. Cache reads cost $0.20, and fast mode costs $16 / $80. On Modelflare, it starts at $2 / $10 per million tokens on {% raw %}`claude-award`{% endraw %}.
**Is Fable 5.2 better than Claude Opus 5.5?** On Anthropic's published table it scores higher on every benchmark except AA-Briefcase Elo, where Opus 5.5 leads 1800 to 1785. Opus 5.5 costs half as much per token and is Anthropic's recommended starting point. Move up to Fable 5.2 when your Opus 5.5 evals fall short.
**Is Fable 5.2 better than GPT-6 Astra and Gemini 4 Carbon?** In Anthropic's comparison, Fable 5.2 leads GPT-6 Astra on every benchmark where both appear. It leads Gemini 4 Carbon everywhere except AutomationBench, where Gemini scores 53.9% against 51.4%. These are provider-reported results.
**What is the difference between Fable 5.2 and Mythos 5.2?** They are the same underlying model with the same price. Fable 5.2 runs stronger cyber and biology safeguards and is available to all API customers. Mythos 5.2 is limited to organizations in Anthropic's verification programs.
**Can I swap {% raw %}`claude-fable-5-1`{% endraw %} for {% raw %}`claude-fable-5-2`{% endraw %} without other changes?** Often, but not always. Check whether your code edits conversation history, routes conversations between models, uses {% raw %}`computer_20251124`{% endraw %}, or uses the advisor tool. Then rerun your evals with an effort sweep that starts at {% raw %}`high`{% endraw %}.
**Does a lower per-token price make Fable 5.2 cheaper than Fable 5.1 for every task?** For the same token counts, yes, by 20%. Real task cost also depends on effort, thinking length, cache hit rate and retries, so measure complete tasks.
## Sources and update record
- [Anthropic: Claude Fable 5.2 and Claude Mythos 5.2 announcement](https://www.anthropic.com/claude-fable-and-mythos-5-2)
- [What's new in Claude Fable 5.2](https://platform.claude.com/docs/en/models/fable-5-2/whats-new-fable-5-2)
- [Claude models overview](https://platform.claude.com/docs/en/models/overview)
- [Prompting Claude Fable 5.2](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-fable-5-2)
- [Claude Fable 5.2 migration guide](https://platform.claude.com/docs/en/models/fable-5-2/migration-guide)
- [Modelflare live pricing](https://modelflare.dev/pricing)
- Related Modelflare analysis: [Claude Fable 5.1 deep dive](https://modelflare.dev/articles/claude-fable-5-1-review-pricing), [Claude Opus 5.5](https://modelflare.dev/articles/claude-opus-5-5-review-pricing), [Claude Sonnet 5.5](https://modelflare.dev/articles/claude-sonnet-5-5-review-pricing), [GPT-6 Astra vs Claude Fable 5.1](https://modelflare.dev/articles/gpt-6-astra-vs-claude-fable-5-1)
**Update record:** November 11, 2026. Initial publication. Specifications, prices, benchmark values, partner reports and safety figures were checked against the linked Anthropic pages. Modelflare availability and route prices were checked against the live catalog. Modelflare did not run its own benchmark of Fable 5.2 or call any model for the article text. The cover illustration was generated with GPT Image 2.5.
---
*Originally published at [modelflare.dev](https://modelflare.dev/articles/claude-fable-5-2-deep-analysis?utm_source=devto&utm_medium=syndication&utm_campaign=blog).*
*Disclosure: I'm part of the Modelflare team, which builds the AI API gateway mentioned in this article.*
```
## Top comments (0)
