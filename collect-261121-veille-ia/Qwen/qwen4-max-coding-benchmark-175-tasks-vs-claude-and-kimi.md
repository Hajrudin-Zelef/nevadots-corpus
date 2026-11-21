---
id: collect-261121-veille-ia/qwen/qwen4-max-coding-benchmark-175-tasks-vs-claude-and-kimi
title: "Qwen4-Max coding benchmark: 175 tasks vs Claude and Kimi"
domain: qwen
role: reference
task: article
actors: ["Alibaba"]
dates: []
keywords: ["tasks", "qwen4-max", "claude", "output", "model", "benchmark", "retry", "sonnet"]
source: https://www.coderouter.io/blog/qwen4-max-coding-benchmark-2026
source_anchor: ""
source_lines: [1, 45]
section: "Qwen"
---
**Disclosure:** we build CodeRouter, a router for coding agents. We called Qwen4-Max through OpenRouter, where Alibaba is its only provider. The other models' numbers are the ones already on our benchmark page, measured on November 3.

Qwen4-Max went live on OpenRouter on November 16 (UTC): $1.50 per million input tokens, $4.50 per million output, a 1M-token context, reasoning on by default, and no open weights. A cheaper frontier-class model is exactly what a router looks for, so we ran it the same day on the same tasks as every model on our benchmark page.

**Qwen4-Max solved 96.0% of the tasks with one retry, 83.4% on the first try, for $1.47. That's what Claude Sonnet 5.5 cost, and Sonnet solved 100%.**

## The results

Same 175 single-file Python tasks (Exercism plus Aider's hard set). The model sees the instructions and the stub file and returns the whole file; we run the tests, and on a failure it gets one retry with the failing output.

| Model | First try | With one retry | Cost of all 175 tasks | Median time per task | 
|---|---|---|---|---|
| Claude Opus 5.5 | 98.3% | 100% | $2.61 | 4.8 s | 
| Claude Sonnet 5.5 | 93.7% | 100% | $1.48 | 4.0 s | 
| **Qwen4-Max** | **83.4%** | **96.0%** | **$1.47** | **11.3 s** | 
| Kimi K2.6 | 86.9% | 94.9% | $0.57 | 9.4 s | 
| Claude Haiku 5.5 | 72.0% | 93.7% | $0.15 | 5.5 s | 

Costs are list prices applied to the tokens each request reported, retries included.

It missed 7 tasks: paasio and tree-building in both task sets, plus dot-dsl, go-counting and pov from Aider's hard set. It never hit our 32,768-token output limit and returned no errors.

## Why a cheaper price list didn't mean a cheaper run

Per token, Qwen4-Max is cheaper than Sonnet 5.5: 25% less for input ($1.50 vs $2) and 55% less for output ($4.50 vs $10). But it wrote 272,000 output tokens on these tasks against Sonnet's 109,000, and 72% of them were reasoning. The extra output ate the whole discount.

The reasoning scales with the task. Half of its first answers reasoned for under 200 tokens, one in ten for over 2,600, and the longest for 14,200. It also makes Qwen4-Max slower: a median of 11.3 seconds per task against 4 to 5.5 for the Claude models.

## What it means for routing

Against what CodeRouter already uses, Qwen4-Max doesn't earn a slot on this suite:

- Against Sonnet 5.5: same cost, 4 points lower with a retry, 10 points lower on the first try.
- Against Kimi K2.6: 1 point higher with a retry, 3.5 points lower on the first try, at 2.6× the cost.

So we're not adding it to routing now. Single-file tasks aren't long agent sessions, and a model that reasons more can look different there; that would take an agent run to find out.

## Limits

- 175 single-file Python tasks, one run. Public tasks a model may have seen in training.
- Qwen4-Max ran on November 16 through OpenRouter (provider: Alibaba), with reasoning at its default. The other models ran on November 3 through CodeRouter's production path. Same tasks, prompt and grading; different days and call paths.
- Median times include the retry and depend on the provider's load on the day.
- We publish pass/fail, tokens, cost and time per task, not the model's output.

Per-task data: coderouter-qwen4max-175-2026-11.json. The other models' data is on the benchmark page.
