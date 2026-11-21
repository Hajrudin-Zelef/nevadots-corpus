---
id: collect-261121-veille-ia/qwen/qwen3-8-max-benchmarks-pricing-the-model-gap
title: "Qwen3.8-Max benchmarks & pricing · The Model Gap"
domain: qwen
role: reference
task: article
actors: ["Alibaba"]
dates: ["2026-08-03"]
keywords: ["qwen3", "noise", "pricing", "benchmark", "model", "scores", "tracked", "terminal-bench"]
source: https://themodelgap.com/models/qwen3-8-max
source_anchor: ""
source_lines: [1, 105]
section: "Qwen"
---
AlibabaPrevious version · Qwen4-Max

# Qwen3.8-Max

Ten of the twelve benchmark scores tracked here, including the core coding results (SWE-bench Verified, LiveCodeBench, DeepSWE, Terminal-Bench 2.1), are independently confirmed — only the Toolathlon-Verified number behind Alibaba's agentic claim, and HLE with tools, are still self-reported. The hosted endpoint has served the Qwen3.8-Max-0902 snapshot since 2026-09-05; rows observed before that were earned by the 0803 build.

Qwen3.8-Max benchmarks and pricing, every number sourced: 12 tracked Qwen3.8-Max benchmark scores (10 independently run, 2 still resting on a vendor’s own claim), priced at $2.00 per million input tokens and $6.00 per million output.

- Released
- 2026-08-03
- License
- proprietary
- Context window
- 1M tokens
- Knowledge cutoff
- Not disclosed
- Verified
- sources checked 2026-08-13–2026-10-08
- Parameters
- Not disclosed
- Architecture
- Not disclosed

## Qwen3.8-Max’s verified record

Against the 303 head-to-head comparisons Qwen3.8-Max shares with other tracked models:

A gap counts for Qwen3.8-Max only where both sides were run independently and the benchmark still separates models. Losses are listed alongside wins on purpose — Qwen3.8-Max trails on 44 of them.

## Qwen3.8-Max API pricing

$2.00 in / $6.00 out per 1M tokens — official pricing source

### What Qwen3.8-Max costs per job

| Qwen3.8-Max cost for three reference workloads, computed from its list rates |  |  | 
|---|---|---|
| Workload | Tokens in / out | Cost | 
|---|---|---|
| One long chat turn | 100K / 10K | $0.260 | 
| A codebase review | 1,000K / 100K | $2.60 | 
| A day of agent work | 10,000K / 1,000K | $26.00 | 

Computed from Qwen3.8-Max’s list rates above — cache discounts and batch tiers are not applied.

Qwen3.8-Max is one of 3 Alibaba models tracked on this site, at these official list prices.

| Alibaba model pricing, official list rates |  |  | 
|---|---|---|
| Model | In / 1M | Out / 1M | 
|---|---|---|
| Qwen4-Max | $1.50 | $4.50 | 
| Qwen3.8-Flash-Next | — | — | 
| Qwen3.8-Max(previous version) | $2.00 | $6.00 | 

## Qwen3.8-Max benchmark scores

| Qwen3.8-Max benchmark scores, provenance, and source links |  | 
|---|---|
| Benchmark | Score | 
|---|---|
| HLE(no tools) <sup>[1]</sup>Reasoning · ±2 is noise |  | 
| HLE(with tools) Reasoning · ±2 is noise |  | 
| GPQA Diamondsaturated <sup>[2]</sup>Expert science Q&A — not ranked at any gap size |  | 
| Terminal-Bench 2.1 <sup>[3]</sup>Terminal ops · ±10.6 is noise |  | 
| DeepSWE <sup>[4]</sup>Long-horizon coding · ±9.5 is noise |  | 
| Toolathlon-Verified <sup>[5]</sup>Multi-tool chores · ±9.7 is noise |  | 
| Agents' Last Exam <sup>[6]</sup>Professional work · ±3.2 is noise |  | 
| SWE-bench Verifiedsaturated <sup>[7]</sup>Bug fixing — not ranked at any gap size |  | 
| LiveCodeBenchsaturated <sup>[8]</sup>Contest coding — not ranked at any gap size |  | 
| LiveBench <sup>[9]</sup>Composite score across 7 domains · ±2.7 is noise |  | 
| AnalystAgent <sup>[10]</sup>Spreadsheet & document analysis · ±11.2 is noise |  | 
| Terminal-Bench 4.0 <sup>[11]</sup>Terminal ops · ±12.4 is noise |  | 

Who ran these numbers: 10 of 12 independent — vals.ai (4), artificialanalysis.ai (3), deepswe.datacurve.ai (1), snorkel.ai (1), livebench.ai (1); vendor self-reported (2).

## Notes on the record

## Compare with

## FAQ

### Has Qwen3.8-Max been independently benchmarked?

Of the twelve scores tracked for Qwen3.8-Max on this site, ten come from independent evaluators — Artificial Analysis (HLE no tools, Terminal-Bench 2.1, and AA-AnalystAgent, all on the 0902 snapshot), Vals.ai (GPQA Diamond, SWE-bench Verified, LiveCodeBench, and Terminal-Bench 4.0, added 2026-10-08), the DeepSWE leaderboard, Snorkel AI's Agents' Last Exam, and LiveBench's own board — while two are self-reported by Alibaba, sourced to the Qwen3.8-2.4T-A95B model card on Hugging Face: HLE with tools and Toolathlon-Verified. The core software-engineering benchmarks (SWE-bench Verified, LiveCodeBench, DeepSWE) all fall on the independent side, and Terminal-Bench 2.1 joined them when Artificial Analysis ran the 0902 build; Toolathlon-Verified is the one agentic number still vendor-reported.

### Is Qwen3.8-Max open source or a proprietary model?

The Qwen3.8-Max tracked on this page is the pay-per-token model on Alibaba Cloud Model Studio — proprietary, not open weight. Alibaba separately published the underlying architecture on Hugging Face as Qwen/Qwen3.8-2.4T-A95B, but under a custom "qwen3.8-max" license rather than Apache 2.0 (unlike the smaller Qwen3.8-27B, which is Apache 2.0 — source: huggingface.co/Qwen/Qwen3.8-27B), and that release drops vision/video input, forces thinking mode on, and caps native context at 262,144 tokens versus the hosted API's 1,000,000-token default. Source: huggingface.co/Qwen/Qwen3.8-2.4T-A95B.

### Does Qwen3.8-Max cost the same price in every region?

No. The $2 in / $6 out per-million-token price on this page is Alibaba Cloud's Singapore endpoint rate. Its Beijing endpoint bills $1.65 in / $4.951 out, and — per the same Alibaba Cloud pricing page, checked 2026-08-20 — the Frankfurt, Virginia (US), Tokyo, and Hong Kong endpoints bill at that same lower rate. Singapore is the outlier, not the norm: of Qwen3.8-Max's six live regions, it's the only one billing at the higher rate.

### What is Qwen3.8-Max's knowledge cutoff date?

Alibaba has not published one. Neither the Model Studio documentation for the hosted Qwen3.8-Max API nor the Hugging Face card for the related open-weight Qwen3.8-2.4T-A95B checkpoint states a training-data cutoff, even though both list parameter counts, context limits, and benchmark tables in detail.

### Why is Qwen3.8-Max also listed as Qwen3.8-2.4T-A95B on Hugging Face?

"2.4T-A95B" is Alibaba's shorthand for the mixture-of-experts design behind Qwen3.8-Max: 2.4 trillion parameters total, with 95 billion active per token. It's the repository name for the open-weight release of that architecture; the hosted, billed API tracked on this page is versioned separately as "Qwen3.8-Max." Source: huggingface.co/Qwen/Qwen3.8-2.4T-A95B.

### Which of Qwen3.8-Max's scores actually count for ranking?

Not the headline ones. Its three biggest numbers — GPQA Diamond 93.69, LiveCodeBench 87.85 and SWE-bench Verified 85.6 — all sit on boards retired as saturated. The strongest still-ranked independent result is Terminal-Bench 2.1 at 88.76 (2026-09-29).
