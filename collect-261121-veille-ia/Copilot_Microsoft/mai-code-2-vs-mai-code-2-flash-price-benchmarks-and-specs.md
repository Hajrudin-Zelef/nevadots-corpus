---
id: collect-261121-veille-ia/copilot-microsoft/mai-code-2-vs-mai-code-2-flash-price-benchmarks-and-specs
title: "MAI Code 2 vs MAI Code 2 Flash: Price, Benchmarks and Specs"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["code", "flash", "tokens", "microsoft", "better", "each", "price", "input"]
source: https://vandatateam.com/ai-model-benchmarks/compare/mai-code-2-vs-mai-code-2-flash
source_anchor: ""
source_lines: [1, 150]
section: "Copilot_Microsoft"
---
Side by side

## MAI Code 2 and MAI Code 2 Flash, line by line

Prices in effect today, compared at the same tier wherever both list one. The better value in each row is highlighted.

| Fact |  |  | 
|---|---|---|
| Price |  |  | 
| Price per 1M input tokens | $0.50 | $0.05 (better) | 
| Price per 1M output tokens | $2.50 | $0.25 (better) | 
| Specs |  |  | 
| Provider | Microsoft AI | Microsoft AI | 
| Release date | November 17, 2026 | November 17, 2026 | 
| Access | limited access | limited access | 
| Context window | 1,050,000 tokens | 1,050,000 tokens | 
| Max output | 128,000 tokens | 128,000 tokens | 
| Input | text, image | text, image | 
| Output | text | text | 

Provider results

## What the labs report

Results tables a lab published with one of its models, where it lists both MAI Code 2 and MAI Code 2 Flash. These are the lab's own figures, not independent measurements, and each table is read on its own: labs run benchmarks with different harnesses and budgets.

Microsoft AI, MAI Code 2 and MAI Code 2 Flash launch charts, November 17, 2026Microsoft AI · 2026-11-18

| Benchmark | MAI Code 2 | MAI Code 2 Flash | 
|---|---|---|
| DeepSWE 1.1 | 73.2% (better) | 59.9% | 
| Terminal-Bench 2.1 | 84.6% (better) | 78.3% | 
| SWE Atlas | 43.1% | 44.8% (better) | 

Microsoft's two charts from the MAI Code 2 announcement: MAI Code 2 against GPT-6.1 Sol and Claude Opus 5.5, and MAI Code 2 Flash against GPT-6 Luna and Claude Haiku 5.5; the model card gives the same scores for the two MAI models. Microsoft ran every model at max reasoning effort on MSBench, its evaluation platform, in the GitHub Copilot CLI harness without subagents. Left out here: FrontierCode v1.1, whose scores Microsoft takes from Artificial Analysis, the charts' averages, and the blind front-end preference test.

Charts

## Against every current text model

Where MAI Code 2 and MAI Code 2 Flash land among the current models of their task, on every independent benchmark, on price and on context.

### Where each stands

No independent benchmark has scored either model yet, so the tiles rank their prices among the current models of their task.

Cost

## What each costs

Example workloads at each provider's listed rates, then every other rate either one lists. The cheaper figure in each row is highlighted.

### The same work, priced

For 10M input and 2M output tokens, MAI Code 2 costs 10 times as much as MAI Code 2 Flash, at each provider's listed rates.

| Workload or rate | MAI Code 2 | MAI Code 2 Flash | 
|---|---|---|
| Example workloads |  |  | 
| 10M input and 2M output tokens | $10 | $1 (better) | 
| The same, with 9M of the input tokens cached | $5.73 | $0.60 (better) | 
| Other listed rates |  |  | 
| 1M input tokens, cached | $0.025 | $0.005 (better) | 

Your own bill will differ. These figures use list prices and example workloads. The AI Model Cost Audit reviews your real usage, shortlists models that score as well for each task for less, and estimates the savings.

Launch price $100, regular price $150

Verdict

## When to choose each

Where it leads

No measured lead over MAI Code 2 Flash in this data.

Best for

- Everyday enterprise coding in GitHub Copilot: debugging, multi-file changes, refactoring and front-end work
- Copilot teams that want a cheaper token bill than GPT-6.1 Sol's or Claude Opus 5.5's for routine agent work

Watch out for

- Limited access: only in GitHub Copilot, on the Pro, Pro+, Max, Business and Enterprise plans, with no public API yet
- Microsoft says it is not yet tuned for fundamental science or very long agentic work, and Claude Opus 5.5 leads it on SWE Atlas in Microsoft's own runs (57.5% against 43.1%)
- Microsoft's model card rates its alignment close to GPT-5.6 Sol's and below GPT-6's, and no independent benchmark has scored it yet

Where it leads

- Lower input and output prices, $0.05 and $0.25 vs $0.50 and $2.50 per 1M tokens
- Cheaper cached input, $0.005 vs $0.025 per 1M tokens

Best for

- High-volume coding help in Copilot: explanations, targeted edits, routine fixes and fast iteration
- Teams on Copilot Free or Student plans that want a current coding model

Watch out for

- Limited access: in GitHub Copilot on every plan, Free and Student included, and in a limited-access preview in Microsoft Foundry, with no public API yet
- The Copilot rates double on May 17, 2027, to $0.10 input and $0.50 output per 1M tokens, the same as GPT-6 Luna and Claude Haiku 5.5
- Its DeepSWE lead over GPT-6 Luna is 0.3 points in Microsoft's own runs, and no independent benchmark has scored it yet
- Replaces MAI-Code-1.1-Flash, which GitHub retires from Copilot on December 18, 2026

From benchmark to production

## Building on MAI Code 2 or MAI Code 2 Flash?

Public scores rarely hold on your own data. We test both models on your real tasks, then build the agent or pipeline around the one that wins.

AI Model Cost AuditLaunch price $100, regular price $150

FAQ

## MAI Code 2 vs MAI Code 2 Flash FAQ

### Which is cheaper, MAI Code 2 or MAI Code 2 Flash?

MAI Code 2 Flash is 90% cheaper per 1M input tokens than MAI Code 2, $0.05 vs $0.50. For 10M input and 2M output tokens, MAI Code 2 costs $10 and MAI Code 2 Flash $1 at standard rates.

### Which is cheaper with prompt caching, MAI Code 2 or MAI Code 2 Flash?

MAI Code 2 charges $0.025 per 1M cached input tokens, 95% below its standard input rate, and MAI Code 2 Flash $0.005, 90% below. With 9M of 10M input tokens cached and 2M output tokens, MAI Code 2 costs $5.73 and MAI Code 2 Flash $0.60.

### How do MAI Code 2 and MAI Code 2 Flash compare in Microsoft AI's own results?

In the MAI Code 2 and MAI Code 2 Flash launch charts (November 17, 2026), MAI Code 2 leads on DeepSWE 1.1, 73.2% to 59.9%; MAI Code 2 leads on Terminal-Bench 2.1, 84.6% to 78.3%; MAI Code 2 Flash leads on SWE Atlas, 44.8% to 43.1%. These are Microsoft AI's figures, not independent measurements.

### Which has the larger context window, MAI Code 2 or MAI Code 2 Flash?

Both accept 1,050,000 tokens of context. Both return up to 128,000 tokens per response.

AI Model Brief

## The AI models worth knowing, once a week

What shipped, how it scores on independent benchmarks, what it costs per million tokens and what was deprecated, from the data behind our AI model benchmarks.

Further reading

## More on MAI Code 2 and MAI Code 2 Flash

Production evaluation

## Choosing between MAI Code 2 and MAI Code 2 Flash?

Public benchmarks narrow the shortlist. We run both on your own tasks, data and cost limits, then build the routing and review controls to run the winner in production.

- A head-to-head evaluation on your real workload
- Routing between models by cost and difficulty
