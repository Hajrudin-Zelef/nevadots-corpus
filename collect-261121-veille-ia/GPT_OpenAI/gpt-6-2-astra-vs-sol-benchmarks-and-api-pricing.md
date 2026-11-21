---
id: collect-261121-veille-ia/gpt-openai/gpt-6-2-astra-vs-sol-benchmarks-and-api-pricing
title: "GPT-6.2 Astra vs. Sol: Benchmarks and API Pricing"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["astra", "model", "gpt-6", "gpt6", "reasoning", "openai", "tokens", "text"]
source: https://dev.to/modelflare_dev/gpt-62-astra-vs-sol-benchmarks-and-api-pricing-2n8p
source_anchor: ""
source_lines: [1, 108]
section: "GPT_OpenAI"
---
Explore GPT-6.2 Astra and Sol through coding and reasoning benchmarks, long-context and cache costs, and a breakdown of Modelflare prices by group.

This syndication copy follows the Modelflare source article. Benchmark scores, harness notes and OpenAI list prices are attributed to the cited publishers (OpenAI, Cognition, Artificial Analysis, Epoch AI and ARC Prize). Modelflare prices are a dated public-catalog snapshot. Modelflare did not run its own independent benchmark of GPT-6.2 Astra or Sol for this article, and the workload examples use illustrative token counts and acceptance rates.

OpenAI released GPT‑6.2 Astra and GPT‑6.2 Sol on November 17, 2026. Both have a 1.05 million token context window, but what the extra spend buys depends on the work. On Cognition’s FrontierCode 2.0, both pass **48.2%** of tasks. Sol costs **$1.29** per run against Astra’s **$5.97**. In the standard ARC‑AGI‑3 harness, which asks models to explore interactive environments and build on previous turns, Astra leads by **15.4 percentage points**. The decision for a development team is where to reserve the premium model and which tasks Sol can handle reliably. OpenAI launch · Cognition FrontierCode · ARC Prize Astra results · ARC Prize Sol results

| Specification | GPT‑6.2 Astra | GPT‑6.2 Sol | 
|---|---|---|
| API model ID | `gpt-6.2-astra` | `gpt-6.2-sol` | 
| Workload | Difficult reasoning, multistep professional work, complex code changes | Everyday coding, document work, agents at scale | 
| Context window | 1,050,000 tokens | 1,050,000 tokens | 
| Maximum input / output | 922,000 / 128,000 tokens | 922,000 / 128,000 tokens | 
| Modalities | Text and image in, text out | Text and image in, text out | 
| Knowledge cutoff | May 18, 2026 | June 1, 2026 | 
| API endpoints | Responses, Chat Completions, Batch | Responses, Chat Completions, Batch | 
| Reasoning effort | `low` ,`medium` ,`high` ,`xhigh` ,`max` | `low` ,`medium` ,`high` ,`xhigh` ,`max` | 

Both models support streaming and structured outputs. Tool calling in the official API requires Responses. Image input lets the model interpret a screenshot or chart, and image creation is available as a separate Responses tool. OpenAI suggests starting at `medium` for new integrations or at `low` when moving from `none` or `minimal`. Budget enough output for reasoning tokens as well as the final text, especially for long reports, patches or structured results. Astra model documentation · Sol model documentation · GPT‑6.2 migration guide · Reasoning token documentation

Sol suits changes with a clear brief, a known boundary and a way to validate the result. Astra warrants a closer look for projects with dependencies across components, conflicting requirements or research that requires multiple passes through the evidence. Adding a well-defined metric to an existing report is a reasonable Sol task. Migrating the entire reporting definition and tracking implicit dependencies on the old metric is a better case for measuring whether Astra can save rework.

A 922,000-token input limit gives you room to assemble a substantial record. To measure how well the model uses it, ask for primary citations, include conflicting versions of a document, and check whether the model accounts for effective dates and exceptions. For a code agent, inspect which entry points it read, which existing behaviors it preserved and whether its tests exercise the relevant outcome.

Cognition’s FrontierCode 2.0 evaluates changes to real repositories. Its leaderboard reports both a weighted rubric score and a pass rate, plus the average API cost of one rollout. The harness and effort settings are part of each result. Cognition leaderboard and methodology

| Model | Harness / effort | Weighted score | Pass rate | Cost per rollout | 
|---|---|---|---|---|
| Claude Fable 5.2 | Claude Code / xhigh | 50.2% | 54.7% | $17.73 | 
| Claude Opus 5.5 | Claude Code / max | 47.3% | 51.8% | $12.82 | 
| GPT‑6.2 Astra | Codex / max | 44.2% | 48.2% | $5.97 | 
| GPT‑6 Astra | Codex / max | 44.0% | 47.6% | $6.68 | 
| GPT‑6.2 Sol | Codex / max | 43.8% | 48.2% | $1.29 | 
| GPT‑6.1 Sol | Codex / max | 39.6% | 43.0% | $1.03 | 

In the same Codex setup, Sol 6.2 gains **4.2 percentage points** over Sol 6.1 while cost per run rises from $1.03 to $1.29. Equal list prices can yield different bills when the amount and composition of input, cached and output tokens change. Sol costs about **22%** as much as Astra 6.2 on this benchmark. That makes it a sensible first candidate for bounded implementation work with automated acceptance checks. The Claude rows measure a different model and harness together, so portability to another agent requires its own measurement. Cognition FrontierCode

Artificial Analysis reported launch Intelligence Index scores of **62** for Astra and **57** for Sol, up 5 and 2 points from their predecessors. Its separate Coding Agent Index, using Codex at max effort, had Astra flat and Sol **3 points lower** than 6.1 Sol. A **14-point** drop on SWE‑Atlas‑QnA‑AA drove Sol’s result. Artificial Analysis observed that the newer model was more likely to omit key facts in repository questions. Track implementation and repository comprehension separately if your team uses one agent to do both. Artificial Analysis launch evaluation

OpenAI reports that Astra gains **5.2 percentage points** over the original GPT‑6 Astra on Agents’ Last Exam, a professional-task benchmark, and about **9 points** on Terminal‑Bench‑Science 0.1. OpenAI reports less than one-third of the previous generation’s cost on each benchmark. Interpret the comparisons with the original configurations attached. For example, OpenAI’s footnote identifies different execution harnesses for reported Claude Fable 5.2 results on Terminal‑Bench‑Science. OpenAI launch evaluation

On OpenAI’s internal GDPval v2 knowledge-work benchmark, Astra gains **6.2 percentage points** over its predecessor with **22% fewer output tokens**. Sol gains **5.3 points** over 6.1. Artificial Analysis’s AA‑3D instead has judges try two blinded interactive experiences generated from the same prompt and vote for one. Astra scores **1,318 Elo**, Fable 5.2 **1,271**, and Sol **1,259**. These tests give different information about whether a deliverable meets its brief and what it feels like to use. For a financial workbook, for example, you can require both correct answers about the data and formulas that reference the right cells. OpenAI GDPval v2 · Artificial Analysis AA‑3D

Epoch AI gives Astra 6.2 and Claude Fable 5.2 an Epoch Capabilities Index estimate of **174**, with intervals of 170–179 and 170–177 respectively. In a separate small, manually graded evaluation of work inside Epoch, Astra averaged **65%**, **17 percentage points** below Fable. Graders assessed the choice of research question, quality of evidence, iteration and whether readers would understand the findings. For market research or internal decision documents, those criteria can go directly into the acceptance rubric, along with human editing time in the cost calculation. Epoch Capabilities Index · Epoch evaluation of real work

| Verified ARC Prize evaluation | GPT‑6.2 Astra | GPT‑6.2 Sol | 
|---|---|---|
| ARC‑AGI‑2, semi-private set, max | 95.8% | 85.8% | 
| ARC‑AGI‑3, Standard harness, max | 70.3% | 54.9% | 
| ARC‑AGI‑3, Provider Adapter, max | 99.8% | 98.2% | 
| ARC‑AGI‑3, Provider Adapter, xhigh | 100.0% | 97.4% | 

ARC‑AGI‑2 measures rule induction from examples. Version 3 adds interactive environments. The Standard harness carries notes the model chooses to keep. The Provider Adapter also preserves opaque reasoning state and compacts longer conversations. Astra’s **100%** is its `xhigh` result with the adapter. ARC Prize reports incomplete task or game coverage on some Sol runs and uses the full dataset denominators for the published scores. In your own multi-turn agent, include tool-result replay, interruption recovery and context compaction in the evaluation. Those can all change when you change client or server-side state handling. ARC Prize Astra results · ARC Prize Sol results and coverage notes

These are **USD per million tokens** at the official Standard rate. Cache writes are for 30-minute retention.

| Model | Request input | Uncached input | Cache read | Cache write | Output | 
|---|---|---|---|---|---|
| GPT‑6.2 Astra | ≤272,000 | $8 | $0.40 | $10 | $40 | 
| GPT‑6.2 Astra | >272,000 | $16 | $0.80 | $20 | $60 | 
| GPT‑6.2 Sol | ≤272,000 | $2 | $0.10 | $2.50 | $10 | 
| GPT‑6.2 Sol | >272,000 | $4 | $0.20 | $5 | $15 | 

Astra costs **20% less** than the original Astra for uncached input, 30-minute cache writes and output. Cache reads fall from $1 to $0.40, a **60%** reduction. Sol inherits the corresponding list prices of 6.1 Sol, and Astra’s 6.2 token rates are **four times** Sol’s within either context tier. For short context, the 12-hour cache write rate is $20 for Astra and $5 for Sol. OpenAI also offers Batch at 0.5× Standard, Fast at 2× and Ultrafast at 3× through February 28, 2027, rising to 6× on March 1 Pacific time. Two-hour and eight-hour background completion windows in the Responses public beta have model token rates 30% and 40% below Standard respectively. Those terms apply to OpenAI’s direct API. Astra prices · Sol prices · Original Astra prices · OpenAI API changelog

**272,000 is a threshold for the full request.** From 272,001 input tokens, input and cache rates double and output costs 1.5 times the short-context rate. With no cache activity and 8,000 billable output tokens, Astra goes from **$2.496** at 272,000 input tokens to about **$4.832** one token later. An agent that appends every tool log can cross this boundary early in a workflow. Track the distribution of input length, the share of requests that cross the boundary and the volume of cache reads. Astra pricing rules · Sol pricing rules

Consider 100,000 input tokens and 8,000 billable output tokens. Astra costs **$1.12** and Sol **$0.28** without cache activity. If 80,000 input tokens are cache reads and 20,000 are uncached, the totals fall to **$0.512** and **$0.128**. If those 80,000 tokens are written to a 30-minute cache on the first call, the totals become **$1.28** and **$0.32**. For a full workflow budget, include the initial write, reuse of the prefix, cache hit rate and billable reasoning output. Looking only at the final warm-cache call understates costs across the workflow.

Modelflare’s public pricing API listed both `gpt-6.2-astra` and `gpt-6.2-sol` on November 18, 2026. Both have short and long context tiers and use the rates of the selected group. Each cell shows **uncached input / cache read / cache write / output**, in **USD per million tokens**. Modelflare Astra pricing · Modelflare Sol pricing · Public pricing API

| Group | Astra ≤272K | Astra >272K | Sol ≤272K | Sol >272K | 
|---|---|---|---|---|
| `openai-award` | 0.24 / 0.012 / 0.30 / 1.20 | 0.48 / 0.024 / 0.60 / 1.80 | 0.06 / 0.003 / 0.075 / 0.30 | 0.12 / 0.006 / 0.15 / 0.45 | 
| `openai-stable` | 0.64 / 0.032 / 0.80 / 3.20 | 1.28 / 0.064 / 1.60 / 4.80 | 0.16 / 0.008 / 0.20 / 0.80 | 0.32 / 0.016 / 0.40 / 1.20 | 
| `openai-premium` | 1.04 / 0.052 / 1.30 / 5.20 | 2.08 / 0.104 / 2.60 / 7.80 | 0.26 / 0.013 / 0.325 / 1.30 | 0.52 / 0.026 / 0.65 / 1.95 | 
| `openai-enterprise` | 4.00 / 0.20 / 5.00 / 20.00 | 8.00 / 0.40 / 10.00 / 30.00 | 1.00 / 0.05 / 1.25 / 5.00 | 2.00 / 0.10 / 2.50 / 7.50 | 
| `openai-first-topup` | 0.12 / 0.006 / 0.15 / 0.60 | 0.24 / 0.012 / 0.30 / 0.90 | 0.03 / 0.0015 / 0.0375 / 0.15 | 0.06 / 0.003 / 0.075 / 0.225 | 

Award, Stable and Premium apply 3%, 8% and 13% of catalog base rates respectively. Enterprise is available to authorized customers at 50%. The top-up group applies a 1.5% rate and a 10 RPM limit. Its public eligibility rule calls for a single qualifying top-up of at least $20, and account eligibility determines access. The published rules for these two model entries list no request-level surcharge. Check the live catalog and the model and group shown in your usage record for the applicable rate. Modelflare model and group pricing · Public pricing API

Here is the same snapshot for other GPT generations at the short-context `openai-stable` rate, also in USD per million tokens.

| Model | Uncached input | Cache read | Cache write | Output | 
|---|---|---|---|---|
| `gpt-6-astra` | 0.80 | 0.08 | 1.00 | 4.00 | 
| `gpt-6.2-astra` | 0.64 | 0.032 | 0.80 | 3.20 | 
| `gpt-6.1-sol` | 0.16 | 0.008 | 0.20 | 0.80 | 
| `gpt-6.2-sol` | 0.16 | 0.008 | 0.20 | 0.80 | 
| `gpt-6.1-terra` | 0.08 | 0.004 | 0.10 | 0.40 | 
| `gpt-6-luna` | 0.02 | 0.002 | 0.025 | 0.10 | 

Existing Sol users can compare success rates, elapsed time and actual tokens because 6.1 and 6.2 have the same short and long unit prices within a group. Terra and Luna give teams cheaper baselines for classification, extraction and predictable templates. Under Stable pricing, the 80,000-token cache hit example above comes to **$0.04096 for Astra** and **$0.01024 for Sol** in model token charges. Confirm service tier, background-window and tool charges separately for the endpoint you use. Modelflare public pricing API

Split evaluation samples into mergeable patches, repository questions, long-document work and multistep deliverables. Keep the input version, tool permissions and stopping rules consistent. Start with Sol 6.2 at `medium`, then compare Sol at `high`, Astra at `medium` or your current Sol version. Measure recall of important facts, human corrections, full task latency and whether interrupted runs can still deliver. For tool-heavy workflows, capture time to the first visible token, first tool action and final deliverable separately.

Divide total spend by the tasks that pass acceptance. Include failed runs, escalation, tool charges and reviewer time. In a hypothetical example, 100 initial Sol runs cost $0.10 each and 80 pass. The remaining 20 go to Astra at $0.40 each and 16 pass. That is **$18 / 96 = $0.1875** per accepted task. Sending all 100 to Astra, assuming 95 pass, gives **$40 / 95 ≈ $0.421**. Estimate the second model’s success on the actual subset that failed. Models can struggle on the same cases, and the aggregate leaderboard cannot tell you how often an escalation will recover one.

There is also a concrete caching change to account for when migrating. GPT‑6.2’s Responses API places top-level `instructions` after `input`. OpenAI recommends putting stable instructions in a `developer` message at the start of `input` so they can be part of the reusable prefix. Keep request-specific guidance in the top-level field. For tool workflows, use Responses and check old sampling or logprob fields, output budgets and incomplete-response handling. Use the resulting usage data to confirm the cache actually behaves as expected. OpenAI GPT‑6.2 migration guide

Sol 6.2 is a strong first candidate for routine coding and high-volume agents. For difficult work with a small number of expensive failures, add Astra to the comparison and judge its premium by results that pass review. This article uses published evaluations and dated price observations. The workload examples use illustrative token counts and acceptance rates.

**Update record:** November 18, 2026 source publication on modelflare.dev. Syndication prepared November 19, 2026 (Asia/Shanghai) from the live article. Values above are as published on the linked pages and Modelflare catalog snapshot cited in the source; re-check before production routing.

*Originally published at modelflare.dev.*

*Disclosure: I'm part of the Modelflare team, which builds the AI API gateway mentioned in this article.*

## Top comments (0)
