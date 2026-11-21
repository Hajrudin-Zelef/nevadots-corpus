---
id: collect-261121-veille-ia/qwen/qwen-3-8-benchmark-scores-every-number-explained
title: "Qwen 3.8 Benchmark Scores: Every Number Explained"
domain: qwen
role: reference
task: article
actors: ["Alibaba"]
dates: []
keywords: ["qwen", "qwen3", "model", "benchmark", "alibaba", "published", "scores", "claude"]
source: https://emergent.sh/learn/qwen-3-8-benchmarks
source_anchor: ""
source_lines: [1, 144]
section: "Qwen"
---
#### TL;DR

- Qwen 3.8 refers to Qwen3.8-Max, Alibaba's 2.4-trillion-parameter mixture-of-experts multimodal model (95 billion active), released August 3, 2026. It is not a small 3.8-billion-parameter model, despite what some pages claim.
- Published wins: PaperBench 93.0, IFBench 82.8, Terminal Bench 2.1 at 86.6, and a broad multimodal sweep led by MathVision 95.2 and OSWorld-Verified 86.1.
- Published losses: HLE at 43.6 (last among the four flagships) and SWE-bench Pro at 67.7 (12 points behind Fable 5).
- Every number is vendor-run, and most coding rows used Anthropic's Claude Code harness. Several benchmarks are Qwen's own in-house evals.
- Pricing is $2 input and $6 output per million tokens, roughly a third of Claude Opus 5 and a quarter of GPT-5.6 Sol.
- As of publication, no independent evaluator (Artificial Analysis, community leaderboards) had scored the model. Treat the table as a strong claim, not a measurement.


On August 3, 2026, Alibaba released Qwen3.8-Max and, for the first time in the model's rollout, published a full benchmark table to back its claims. The numbers tell a mixed story: Qwen3.8-Max wins on several agentic and multimodal tests, trails Claude Fable 5 on core coding, and arrives wrapped in methodology choices that shape how each row should be read. 

This article breaks down the published Qwen 3.8 benchmark scores in full, showing where the model genuinely leads, where it falls behind, how it stacks up against Kimi K3, Fable 5, and GPT-5.6 Sol, and what the fine print behind the table means before you trust any single figure.

## Qwen 3.8 is the 2.4T flagship, not a small model

Qwen 3.8 is shorthand for Qwen3.8-Max, a 2.4-trillion-parameter mixture-of-experts model with 95 billion active parameters. It is multimodal, carries a 1-million-token context window, and targets autonomous coding and long-horizon work. Alibaba released it on August 3, 2026.

A common mix-up is worth clearing up front. Qwen 3.8 is sometimes described as a compact 3.8-billion-parameter model for consumer GPUs, complete with VRAM tables and GSM8K scores. That description confuses the flagship with an unrelated small model and, in places, invents specifications. Qwen3.8-Max is a data-center-scale model, not something that runs on an 8 GB consumer graphics card.

A second point of confusion worth clearing: Qwen 3.8-Max is a different model from the older Qwen3 8B, a genuinely small 8-billion-parameter release from April 2025. The names look similar. The models are not.

Alibaba also promised open weights, including a smaller Qwen3.8-27B, to follow the launch. The license had not been disclosed at release, which is the detail enterprise buyers should track most closely.

## The published Qwen 3.8 benchmark scores

Here are the headline text-model rows exactly as Alibaba published them. Higher is better on every row.

| **Benchmark** | **Qwen3.8-Max** | **Claude Opus 4.8** | **Claude Fable 5** | **GPT-5.6 Sol** | **Qwen3.7-Max** | 
|---|---|---|---|---|---|
| Terminal Bench 2.1 | 86.6 | 84.6 | 84.6 | 88.8 | 74.5 | 
| SWE-bench Pro | 67.7 | 69.2 | 80.0 | 64.6 | 60.6 | 
| PaperBench | 93.0 | 80.3 | 88.8 | 90.5 | 64.8 | 
| GPQA Diamond | 92.6 | 92.0 | 92.6 | 94.1 | 92.4 | 
| IFBench | 82.8 | 62.2 | 63.5 | 72.7 | 79.1 | 
| HLE | 43.6 | 45.7 | 53.3 | 47.2 | 41.4 | 

*Qwen 3.8 benchmark scores as published by Alibaba, August 2026 - source:* *Qwen official release*

Two things are true at once here. Qwen3.8-Max beats both Claude flagships on Terminal Bench, and it trails Fable 5 by 12 points on SWE-bench Pro. The first result is the one Alibaba leads with; the second is the one that shapes whether the model fits deep engineering work.

## Where Qwen 3.8 leads: research, instruction following, and multimodal

Qwen3.8-Max is strongest on research reproduction, instruction following, and vision.

| **Benchmark** | **What it tests** | **Qwen3.8-Max** | **Best rival score** | 
|---|---|---|---|
| PaperBench | Research paper reproduction | 93.0 | GPT-5.6 Sol 90.5 | 
| IFBench | Instruction following | 82.8 | GPT-5.6 Sol 72.7 | 
| MathVision | Multimodal math reasoning | 95.2 | Leads its table | 
| LogicVista | Visual logic reasoning | 91.9 | Leads its table | 
| OSWorld-Verified | Computer-use agents | 86.1 | Fable 5 85.0 | 

*Benchmarks where Qwen3.8-Max leads, from Alibaba's published tables, August 2026*

Its clearest single result is PaperBench at 93.0, ahead of GPT-5.6 Sol (90.5), Fable 5 (88.8), and Opus 4.8 (80.3). That is a 28-point jump over Qwen3.7-Max, a generational swing large enough that it warrants independent confirmation.

PaperBench, built by OpenAI, tests whether a model can reproduce the results of a scientific paper from its experimental description. A high score points to long-horizon research ability, which lines up with Alibaba's demos of multi-day autonomous work.

On IFBench, which measures instruction following, Qwen3.8-Max posts 82.8. The nearest non-Qwen model in the table is GPT-5.6 Sol at 72.7, with both Claude flagships in the low 60s. Instruction following was already a Qwen3.7-Max strength at 79.1, so this reads as a durable trait rather than a one-off.

The multimodal table is where the model looks most dominant. Alibaba reports MathVision at 95.2, LogicVista at 91.9, and OSWorld-Verified at 86.1, the last of which edges Fable 5 (85.0) and GPT-5.6 Sol (83.2) on computer-use agents. Neither Claude flagship competes across most of the vision rows, which is part of why that table reads as a sweep.

## Where Qwen 3.8 loses: broad knowledge and hard coding

Alibaba left its losses in the table, which is worth some credit. Two rows stand out.

| **Benchmark** | **What it tests** | **Qwen3.8-Max** | **Category leader** | 
|---|---|---|---|
| HLE | Broad-knowledge frontier exam | 43.6 (last of four flagships) | Fable 5 53.3 | 
| SWE-bench Pro | Hardest software engineering | 67.7 | Fable 5 80.0 | 

*Benchmarks where Qwen3.8-Max trails the frontier, from Alibaba's published table, August 2026*

On HLE, the broad-knowledge frontier exam, Qwen3.8-Max scores 43.6. That is last among the four flagships: Fable 5 leads at 53.3, GPT-5.6 Sol sits at 47.2, and Opus 4.8 at 45.7. HLE resists benchmark-specific tuning better than most evals, so this row is worth weighting heavily. The model barely improved on Qwen3.7-Max's 41.4 here.

On SWE-bench Pro, the harder software-engineering benchmark, it lands mid-pack at 67.7: ahead of GPT-5.6 Sol (64.6), just behind Opus 4.8 (69.2), and a full 12 points behind Fable 5 (80.0). The generational gain over Qwen3.7-Max (60.6) is real, but the deepest coding work is not where this model wins.

The pattern across the coding section is consistent. Qwen3.8-Max is competitive on terminal-driven agentic tasks and behind on the hardest software-engineering evaluations. If your work is deep, careful engineering rather than agentic task completion, the Claude flagships still lead on Alibaba's own numbers.

## Every Qwen 3.8 benchmark number is vendor-run

Four details from Alibaba's own publication change how each row should be read.

### 1. Every score is vendor-run

Alibaba evaluated its own model and its competitors' models. That is standard for launch tables, and it is also the standard reason launch tables get revised later. The vendor picks the benchmark versions, the prompts, and the sampling settings.

### 2. Most coding rows used the Claude Code harness

Alibaba ran most coding benchmarks through Anthropic's own agent harness, pointed at Qwen3.8-Max via its Anthropic-compatible API. Running every model inside the same tool is fair. It also means Qwen's coding scores are really Qwen-inside-Anthropic's-harness scores, and harness choice can swing agentic results by several points.

### 3. Several benchmarks are Qwen's own

QwenSWEBench, QwenQoderBench, CoWorkBench, and RecreationBench were built by the Qwen team. In-house evals are not worthless, but a strong score on the maker's own benchmark is a different kind of evidence than a strong score on SWE-bench Pro. The table presents both without visual distinction.

### 4. The Fable 5 footnote does real work

Alibaba's table carries a note reading "Fable5 results may involve fallbacks." That is a quiet admission that at least one competitor's numbers may not reflect the model running cleanly, and Fable 5 is the column Qwen3.8-Max most often trails.

None of this is unique to Alibaba. OpenAI, Anthropic, and Google all publish self-run tables with their own choices. The point is not that anyone cheated. A vendor table is a claim, not a measurement.

## How Qwen 3.8 compares to Kimi K3, Fable 5, and GPT-5.6 Sol

Against Claude Fable 5, GPT-5.6 Sol, and Kimi K3, Qwen3.8-Max trades wins rather than sweeping any single rival.

| **Matchup** | **Where Qwen3.8-Max leads** | **Where the rival leads** | **Evidence status** | 
|---|---|---|---|
| vs Claude Fable 5 | PaperBench, IFBench, Terminal Bench 2.1 | HLE, SWE-bench Pro (hardest coding and broad knowledge) | Fable 5 column carries a "may involve fallbacks" footnote | 
| vs GPT-5.6 Sol | PaperBench, instruction following | GPQA Diamond (94.1), Terminal Bench 2.1 (88.8) | All scores vendor-run by Alibaba | 
| vs Kimi K3 | Multimodal breadth (Kimi K3 is text-only) | Edged the Qwen 3.8 preview 83 to 80 on one independent architecture test | Kimi K3 has third-party rankings and pricing; Qwen3.8-Max had neither at publication | 

*How Qwen 3.8 compares to Fable 5, GPT-5.6 Sol, and Kimi K3 - flagship scores from Alibaba's vendor-run table, August 2026*

Against Claude Fable 5, the wins split cleanly. Qwen3.8-Max leads on PaperBench, IFBench, and Terminal Bench, and trails on HLE and SWE-bench Pro. Fable 5 remains the stronger pick for the hardest coding and broad-knowledge work, with the fallback caveat noted above.

Against GPT-5.6 Sol, the picture is similar. GPT-5.6 Sol keeps the top spot on GPQA Diamond (94.1) and Terminal Bench (88.8), while Qwen3.8-Max leads on PaperBench and instruction following.

The sharpest contrast is with Kimi K3, the other big open-weight release of the summer. Kimi K3 is text-only, so Qwen3.8-Max's multimodal breadth is a clean differentiator. In one independent architecture test published before Alibaba's table, Kimi K3 edged the Qwen 3.8 preview 83 to 80. The important gap is evidence, not score: Kimi K3 already carries third-party index rankings and published pricing, while Qwen3.8-Max had neither independent ranking at publication. For a full breakdown of that matchup, see our Qwen 3.8-Max vs Kimi K3 comparison.

## Pricing tells you more than the benchmark right now

Qwen3.8-Max launches at $2 per million input tokens and $6 per million output, with cached input at $0.25. That undercuts the U.S. proprietary leaders by a wide margin: less than a third of Claude Opus 5's combined rate and under a quarter of GPT-5.6 Sol's standard-mode rate.

| **Model** | **Input ($/1M)** | **Output ($/1M)** | 
|---|---|---|
| Qwen3.8-Max | 2.00 | 6.00 | 
| Qwen3.7-Max | 2.50 | 7.50 | 
| Kimi K3 | 3.00 | 15.00 | 
| Claude Opus 5 | 5.00 | 25.00 | 
| GPT-5.6 Sol (standard) | 5.00 | 30.00 | 
| Claude Fable 5 | 10.00 | 50.00 | 

*API pricing per million tokens, as on August 2026 - source: VentureBeat launch coverage and Qwen official release. Verify current rates before budgeting.*

The number that actually matters is cost per solved task, and that requires a verified benchmark to compute. Agentic systems burn far more tokens than chatbots, so a multi-hour autonomous run can generate millions of tokens in a single task. A cheaper token compounds fast at that scale, but only if the quality holds. Qwen3.8-Max's $6 output rate undercuts Kimi K3's $15, though our Kimi K3 pricing breakdown shows why the cheaper rate only wins when the output quality holds. Until an independent evaluator confirms the scores, a cheaper token is only a cheaper token.

For family context, Qwen3.7-Max runs $2.50 input and $7.50 output per million tokens, so the new flagship is both stronger on paper and cheaper than the model it replaces.

## Beyond the benchmark table

Benchmark tables answer a narrow question: how does a model score on a fixed set of tests, under the vendor's chosen conditions? That is useful for picking a model. It says nothing about whether you can turn that model into working software. A high PaperBench score does not deploy an app, connect a database, or take a payment.

That is the gap Emergent closes. Emergent is an AI app building platform that turns a plain-English description into a full-stack, production-ready application, built by a multi-agent architecture, with a real backend, real integrations, and code you own. It runs on frontier models from Anthropic, OpenAI, and Google through the Universal LLM Key, so you build on proven model quality without wiring up any of it yourself. If the reason you are weighing benchmark scores is to build something real, start building on Emergent and go straight from idea to a working app.
