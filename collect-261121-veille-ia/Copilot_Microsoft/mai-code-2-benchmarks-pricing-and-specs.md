---
id: collect-261121-veille-ia/copilot-microsoft/mai-code-2-benchmarks-pricing-and-specs
title: "MAI Code 2 Benchmarks, Pricing and Specs"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["code", "microsoft", "gpt-6", "model", "claude", "current", "benchmarks", "copilot"]
source: https://vandatateam.com/ai-model-benchmarks/mai-code-2
source_anchor: ""
source_lines: [1, 168]
section: "Copilot_Microsoft"
---
Specs

## Key facts

- Provider
- Microsoft AI
- Model family
- MAI Code
- Release date
- November 17, 2026
- Status
- Current
- Access
- limited access
- API model id
- Not reported
- Context window
- 1,050,000 tokens
- Max output
- 128,000 tokens
- Input
- text, image
- Output
- text
- Task families
- text, multimodal
- Declared capabilities
- reasoning, coding, agentic coding, image understanding

Charts

## Against every current text model

Where MAI Code 2 lands among the current models of its task, on every independent benchmark, on price and on context.

### Where it stands

21 of 64 list a lower price per 1M input tokens. Each bar is the share of current models it beats.

Price

## MAI Code 2 pricing

It is the #22 cheapest of 64 current text models priced per 1M input tokens.

Benchmarks

## Independent benchmark results

Scores come from independent evaluators that ran the model themselves, pinned to a dated snapshot. Rank is among current models in the same snapshot. Microsoft AI's own launch figures are in the next section, because they are not measured the same way.

No independent benchmark has published a result for MAI Code 2 yet. It is still compared below on price, context and capabilities.

Provider results

## Reported by Microsoft AI

Microsoft AI's own results for MAI Code 2, beside the rivals it chose, as its table lists them. Labs pick the benchmarks and run them their own way, so compare within one table only. Internal benchmarks and other evaluators' indexes are left out.

MAI Code 2 and MAI Code 2 Flash launch charts, November 17, 2026Microsoft AI · 2026-11-18

| Benchmark | MAI Code 2 | MAI Code 2 Flash | GPT-6.1 Sol | Claude Opus 5.5 | GPT-6 Luna | Claude Haiku 5.5 | 
|---|---|---|---|---|---|---|
| DeepSWE 1.1 | 73.2% | 59.9% | 74.6% | 69.0% | 59.6% | 46.0% | 
| Terminal-Bench 2.1 | 84.6% | 78.3% | 85.0% | 78.7% | 69.3% | 73.4% | 
| SWE Atlas | 43.1% | 44.8% | 47.4% | 57.5% | 36.4% | 42.1% | 

Microsoft's two charts from the MAI Code 2 announcement: MAI Code 2 against GPT-6.1 Sol and Claude Opus 5.5, and MAI Code 2 Flash against GPT-6 Luna and Claude Haiku 5.5; the model card gives the same scores for the two MAI models. Microsoft ran every model at max reasoning effort on MSBench, its evaluation platform, in the GitHub Copilot CLI harness without subagents. Left out here: FrontierCode v1.1, whose scores Microsoft takes from Artificial Analysis, the charts' averages, and the blind front-end preference test.

Fit

## Strengths, cautions and best uses

- Strengths
  - Microsoft's larger coding model (November 17, 2026), trained from scratch for debugging, multi-file changes and repository-wide work: a sparse mixture-of-experts with 77B active and about 2.8T total parameters, a 1,050,000-token context and a 128,000-token output
  - In GitHub Copilot it costs $0.50 input, $0.025 cached input and $2.50 output per 1M tokens until May 17, 2027, then $1, $0.05 and $5: a quarter of GPT-6.1 Sol's rates during the promotion and half after it
  - In Microsoft's runs (GitHub Copilot CLI, max effort) it scores 84.6% on Terminal-Bench 2.1 and 73.2% on DeepSWE 1.1, against 85.0% and 74.6% for GPT-6.1 Sol and 78.7% and 69.0% for Claude Opus 5.5
  - Reviewers preferred its front-end apps to GPT-6 Sol's 64% of the time in Microsoft's blind test with 258 annotators
- Cautions
  - Limited access: only in GitHub Copilot, on the Pro, Pro+, Max, Business and Enterprise plans, with no public API yet
  - Microsoft says it is not yet tuned for fundamental science or very long agentic work, and Claude Opus 5.5 leads it on SWE Atlas in Microsoft's own runs (57.5% against 43.1%)
  - Microsoft's model card rates its alignment close to GPT-5.6 Sol's and below GPT-6's, and no independent benchmark has scored it yet
- Best for
  - Everyday enterprise coding in GitHub Copilot: debugging, multi-file changes, refactoring and front-end work
  - Copilot teams that want a cheaper token bill than GPT-6.1 Sol's or Claude Opus 5.5's for routine agent work

From benchmark to production

## Building on MAI Code 2?

Public scores rarely hold on your own data. We test it against the alternatives on your real tasks, then build the agent or pipeline around the model that wins.

AI Model Cost AuditLaunch price $100, regular price $150

Alternatives

## Alternatives to MAI Code 2

The closest current text models by price. The cheapest model that scores as well or better is listed first; failing that, the cheapest one within 3% of the benchmark's scale.

### On score and price

MAI Code 2 beside the 4 alternatives listed below, on Arena Text and on blended price per 1M tokens.

  - Same price per 1M input tokens, $0.50
  - From Google
  - 13% cheaper per 1M input tokens, $0.435 vs $0.50
  - From Xiaomi MiMo
  - 20% more per 1M input tokens, $0.60 vs $0.50
  - From Elorian
  - 20% cheaper per 1M input tokens, $0.40 vs $0.50
  - From OpenAI

FAQ

## MAI Code 2 FAQ

### How much does MAI Code 2 cost?

MAI Code 2 costs $0.50 per 1M input tokens and $2.50 per 1M output tokens at the standard tier. It is the #22 cheapest of 64 current text models priced per 1M input tokens. Source: GitHub, verified November 18, 2026.

### What is the context window of MAI Code 2?

MAI Code 2 accepts up to 1,050,000 tokens of context. It can return up to 128,000 tokens in one response. That ranks #1 of 75 current text models by context length.

### What benchmark results has Microsoft AI published for MAI Code 2?

In the MAI Code 2 and MAI Code 2 Flash launch charts (November 17, 2026), Microsoft AI reports 73.2% on DeepSWE 1.1, 84.6% on Terminal-Bench 2.1 and 43.1% on SWE Atlas. These are Microsoft AI's own figures, beside the rivals it chose, not independent measurements.

### Is MAI Code 2 open source or API only?

Neither. MAI Code 2 has no public API and no published weights; Microsoft AI offers it only through the limited access described in this page's cautions.

### What are the best alternatives to MAI Code 2?

The closest current alternatives by price and measured performance are Gemini 3 Flash Preview (same price per 1M input tokens, $0.50), MiMo-V2.6-Pro (13% cheaper per 1M input tokens, $0.435 vs $0.50), Glimpse (20% more per 1M input tokens, $0.60 vs $0.50) and GPT-4.1 mini (20% cheaper per 1M input tokens, $0.40 vs $0.50).

AI Model Brief

## The AI models worth knowing, once a week

What shipped, how it scores on independent benchmarks, what it costs per million tokens and what was deprecated, from the data behind our AI model benchmarks.

Sources

## Sources

- MAI Code 2 and Code 2 Flash: Everyday coding at scalable prices · Microsoft AI · official · verified November 18, 2026
- MAI Code 2 and MAI Code 2 Flash model card · Microsoft AI · official · verified November 18, 2026
- MAI Code 2 and MAI Code 2 Flash in GitHub Copilot · GitHub · official · verified November 18, 2026
- Models and pricing for GitHub Copilot · GitHub · official · verified November 18, 2026

All 300 sources behind the benchmarks, with their versions, licenses and notes, are in the source catalog.

### Cite this page

Van Data Team. "MAI Code 2 Benchmarks, Pricing and Specs." AI Model Benchmark Dataset 2026-11-18. https://vandatateam.com/ai-model-benchmarks/mai-code-2

Reuse the data: JSON · CSV · or explore every model in the AI model benchmark comparison.

Production evaluation

## Deciding whether MAI Code 2 fits your workload?

Public benchmarks narrow the shortlist. We test the finalists on your own tasks, data and cost limits, then build the routing and review controls to run the winner in production.

- Evaluation cases and acceptance thresholds tied to real work
- Model routing, fallbacks and cost controls
