---
id: collect-261121-veille-ia/gpt-openai/mai-code-2-claims-98-of-gpt-6-1-sol-at-40-of-the-cost-what-the-may-17-2027-price-doubling-does-to-th
title: "MAI Code 2 claims 98% of GPT-6.1 Sol at 40% of the cost: what the May 17, 2027 price doubling does to that math"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["code", "flash", "cost", "gpt-6", "model", "microsoft", "price", "rate"]
source: https://dev.to/ai_maya_063fc568e157562fd/mai-code-2-claims-98-of-gpt-61-sol-at-40-of-the-cost-what-the-may-17-2027-price-doubling-does-170h
source_anchor: ""
source_lines: [1, 168]
section: "GPT_OpenAI"
---
**TL;DR**

- Microsoft announced MAI Code 2 and MAI Code 2 Flash on November 17, 2026, with the headline "98% of GPT-6.1 Sol's performance at 40% of its cost" and "96% of Claude Opus 5.5's performance at 5% of its cost."
- The cost side of that claim was computed at a 50% promotional price that runs until May 17, 2027. Every per-token rate then exactly doubles, so the same token trace costs exactly twice as much.
- My arithmetic on Microsoft's own per-task figures: at list price MAI Code 2 lands near 81% of GPT-6.1 Sol's cost and near 10% of Claude Opus 5.5's. Still cheaper, but a very different sentence from "40%" and "5%".
- The small model flips more sharply. MAI Code 2 Flash at $0.052 per task beats GPT-6 Luna's $0.080 today, but the doubled figure of $0.104 sits about 30% above it, assuming the rival's cost does not move.
- Everything below is vendor-reported on Microsoft's own harness. I did not run any of it, and the sources say as much about how to read the comparison.

The model card is dated November 17, 2026, and describes two hosted agentic coding models that share a training recipe and differ mainly in size. MAI Code 2 is the larger model for long-horizon work in real repositories. MAI Code 2 Flash is the smaller, cheaper one for everyday edits, explanations and routine fixes.

What the card states directly:

| Item | MAI Code 2 | MAI Code 2 Flash | 
|---|---|---|
| Context window | 1,050,000 tokens | 1,050,000 tokens | 
| Input / output limit | 922,000 / 128,000 | 922,000 / 128,000 | 
| Inputs | text and images | text and images | 
| Reasoning effort | low, medium, high, xhigh, max | low, medium, high, xhigh, max | 
| Weights | not distributed, hosted on Azure | not distributed, hosted on Azure | 
| Channels listed | GitHub Copilot | GitHub Copilot, Microsoft Foundry API | 

Both models are decoder-only sparse Mixture-of-Experts transformers with an integrated vision encoder, trained between August and November 2026. Pricing is per million tokens, as reported by InfoWorld and others:

| Rate (USD per 1M tokens) | Code 2 promo | Code 2 list | Flash promo | Flash list | 
|---|---|---|---|---|
| Input | 0.50 | 1.00 | 0.05 | 0.10 | 
| Cached input | 0.025 | 0.05 | 0.005 | 0.01 | 
| Output | 2.50 | 5.00 | 0.25 | 0.50 | 

The promotional rates run until May 17, 2027. Look at the table row by row: every list rate is exactly twice its promo rate. That detail matters, because it removes any ambiguity about how the doubling plays out.

The card says all coding results were produced on MSBench, Microsoft's agentic evaluation platform, using GitHub Copilot CLI version 1.0.102 as the harness with subagents disabled. Every model in a comparison ran in the same harness with the same settings. Results use max reasoning effort and are the mean of three independent runs per task (mean single-attempt pass rate, not pass@k), with infrastructure errors counted as failures.

Those are good methodological choices. Counting infrastructure errors as failures and averaging three runs removes two common ways to flatter a number. It is also a single-vendor setup: the harness, the platform and the model all belong to the same ecosystem, and one of the four benchmarks in the headline average (FrontierCode v1.1) is reported to have run outside MSBench.

The model card table for the three MSBench benchmarks:

| Benchmark | MAI Code 2 Flash | MAI Code 2 | 
|---|---|---|
| DeepSWE 1.1 | 59.9% | 73.2% | 
| Terminal-Bench 2.1 | 78.3% | 84.6% | 
| SWE Atlas | 44.8% | 43.1% | 

Two sanity checks I ran on the reported averages. First, the Flash average of 61.0% is exactly the mean of those three cells ((59.9 + 78.3 + 44.8) / 3 = 61.0), so the Flash comparison is a three-benchmark average, not four. Second, the 63.2% headline for MAI Code 2 is a four-benchmark average. The three cells above average 67.0%, so the implied FrontierCode v1.1 score is about 51.9% (my derivation from rounded numbers, treat it as approximate). The two averages therefore cover different benchmark sets and should not be compared with each other directly.

Against that bar chart, the reported four-benchmark averages are 63.2% for MAI Code 2, 64.1% for GPT-6.1 Sol and 65.3% for Claude Opus 5.5. The ratios 63.2/64.1 = 98.6% and 63.2/65.3 = 96.8% are where the "98%" and "96%" come from. The arithmetic checks out.

The reported average cost per task, at the 50% promotional rate:

- MAI Code 2: $0.309
- GPT-6.1 Sol: $0.761
- Claude Opus 5.5: $6.306

$0.309 / $0.761 = 40.6%, and $0.309 / $6.306 = 4.9%. That reproduces the "40%" and "5%" headline.

Now apply the list price. Because all three per-token rates exactly double, a fixed token trace costs exactly twice as much, whatever the mix of input, cached input and output. That gives about $0.618 per task for MAI Code 2.

- $0.618 / $0.761 = 81.2%
- $0.618 / $6.306 = 9.8%

InfoWorld quotes an analyst, Sanchit Vir Gogia, making the related point that the cost in the comparison is "the spend of a benchmark run, not a published price per token." Another write-up reaches the same roughly 80% and roughly 10% figures after May 17.

Two assumptions sit behind my second set of numbers, and both are worth stating plainly:

1. The rivals' per-task figures stay where Microsoft reported them. I do not know whether those were computed at the rivals' own list prices, and the reports do not say.
2. MAI Code 2 uses the same number of tokens per task after the promo as during it. A model that behaves the same under the same harness would, but cost per task is a product of price and tokens, and only the price is guaranteed to change.

Even with those caveats, the direction is not in doubt. A price that doubles on a known date turns "40% of the cost" into "about 81% of the cost", and the promo window is the period the headline was priced in.

Cost per percentage point of pass rate shows the same shift. Using the reported figures: MAI Code 2 at promo is 0.489 cents per point, at list about 0.978 cents per point; GPT-6.1 Sol is 1.187 cents per point; Claude Opus 5.5 is 9.657 cents per point. On that measure MAI Code 2 stays ahead of both at list price, by about 18% against Sol and by roughly a factor of ten against Opus. So the value case survives. It is a smaller case than the announcement implies.

The Flash comparison is where the doubling bites hardest. Reported averages over three benchmarks:

- MAI Code 2 Flash: 61.0% pass rate, $0.052 per task
- GPT-6 Luna: 55.1%, $0.080 per task
- Claude Haiku 5.5: 53.9%, $0.319 per task

At the promo rate Flash is 35% cheaper than Luna per task and about 6 points higher on pass rate. At list price the same trace costs about $0.104, which is 30% more than Luna's $0.080, still with the 6-point accuracy lead. Against Haiku 5.5 Flash remains cheap either way (16% of Haiku's cost at promo, about 33% at list).

Cost per pass-point on the Flash tier: Flash 0.085 cents at promo, about 0.170 cents at list; Luna 0.145 cents. So from May 17, 2027 a team optimizing for the cheapest successful task, with Luna's cost unchanged, would have a reason to look at Luna again, while a team that values the higher pass rate may still prefer Flash. Whether the rival prices change in six months is unknown, which is exactly why the break-even is worth computing rather than reading off a launch chart.

The launch figures use Microsoft's task mix. Yours is different. The function below uses the published rates and lets you plug in a measured token trace from your own agent runs. The token counts are illustrative, not measured.


```
# USD per 1M tokens: (uncached input, cached input, output)
RATES = {
    "code2_promo": (0.50, 0.025, 2.50),
    "code2_list":  (1.00, 0.05,  5.00),
    "flash_promo": (0.05, 0.005, 0.25),
    "flash_list":  (0.10, 0.01,  0.50),
}
def task_cost(name, uncached_m, cached_m, output_m):
    """Token counts in millions. Returns USD for one task."""
    r_in, r_cached, r_out = RATES[name]
    return uncached_m * r_in + cached_m * r_cached + output_m * r_out
# Illustrative trace: 120k uncached input, 600k cached input, 60k output
trace = (0.12, 0.60, 0.06)
for name in RATES:
    print(name, round(task_cost(name, *trace), 4))
# code2_promo 0.225
# code2_list  0.45
# flash_promo 0.024
# flash_list  0.048
# Output-heavy trace (200k output): reasoning effort "max" can look like this
heavy = (0.12, 0.60, 0.20)
print(round(task_cost("code2_list", *heavy), 4))   # 1.15
```
Two things to measure on your own repositories before trusting any vendor chart:

- 
**Tokens per solved task, not per attempt.** If a cheaper model needs more retries or a higher reasoning effort, the saving shrinks. The card itself notes that higher reasoning effort increases latency and token usage and does not always improve results.
- 
**Cost at the price you will actually pay.** If your finance team approves a pilot at promo rates, add a row for the list rate now.

The card's known-limitations list is unusually concrete for a launch document. Items worth a platform engineer's attention:

- In long agentic sessions the models can repeat tool calls, under-use the existing code base, drift from instructions or stop before fully verifying their work, and quality may degrade as context approaches its maximum.
- At release the models are optimized for English code production only, and performance on less common languages and frameworks has not been extensively evaluated.
- Knowledge ends at the August 2026 pre-training cutoff.
- Code should be reviewed, tested and security-scanned before it reaches production, and agents should run in sandboxes with least-privilege access.

One inconsistency is worth flagging for anyone sizing infrastructure or comparing architectures. The summary table at the top of the card lists MAI Code 2 as "50B-100B (80B active)" and Flash as "5B-15B (12B active)". The model overview section later says MAI Code 2 has 77 billion active and approximately 2.8 trillion total parameters, and Flash has 12 billion active and approximately 398 billion total. Those two descriptions cannot both describe total size. Since the weights are not distributed and the models are served only by Microsoft, the discrepancy does not change what you can run, but it does mean parameter counts from this card should be cited with care until Microsoft reconciles them.

On safety, the card publishes a table of agentic evaluations for both models, including AgentDojo attack success rates of 0.02% (Flash) and 0.00% (Code 2), and says a two-tier real-time classifier system restricts cyber and bio dual-use content. Those are vendor-run internal and public evaluations; treat them as a starting point for your own red-team pass.

1. 
**What is the cost per merged change at list price?** Re-run the arithmetic above with your own trace and the doubled rates.
2. 
**Is your language mix English-centered?** The card says that is where the model was optimized.
3. 
**Does the model need to run outside Microsoft's platforms?** Weights are not distributed. The card lists GitHub Copilot for both models and a Foundry API for Flash.
4. 
**What happens on default enablement?** One report on Copilot administration says new models are enabled automatically unless an administrator has turned off the global default, so check your organization's policy before developers see a new model in the picker.
5. 
**What is your fallback if pricing or availability changes?** A cost gate and a routing layer that can swap models is cheaper than a migration.

**Is MAI Code 2 really 98% as good as GPT-6.1 Sol?**

On Microsoft's four-benchmark average, 63.2% versus 64.1% is 98.6%. That is a vendor-reported number on Microsoft's harness, so it is a fair starting hypothesis rather than a settled fact.

**Is it really 40% of the cost?**

It is 40.6% at the 50% promotional rate. At list price the same arithmetic gives about 81%, assuming the rival's cost is unchanged and token usage is the same.

**When does the pricing change?**

Reports put the end of the promotional period at May 17, 2027, after which every per-token rate doubles.

**Can I self-host MAI Code 2 or Flash?**

No. The card states that weights are not distributed and that the models are hosted on Azure GPU infrastructure.

**Which tier is cheaper, Flash or GPT-6 Luna?**

At promo, Flash: $0.052 versus $0.080 per task. At list price, with Luna unchanged, Luna: about $0.080 versus about $0.104, though Flash reports the higher pass rate (61.0% versus 55.1%).

**Should I trust the benchmark numbers?**

Use them to pick candidates, then measure on your own repositories. Microsoft itself cautions that changing the workload, model configuration, reasoning effort or pricing assumptions can change the comparisons.

- Microsoft AI, MAI Code 2 and MAI Code 2 Flash model card (PDF, last updated 17 November 2026): https://microsoft.ai/pdf/MAI-Code-2-And-MAI-Code-2-Flash-Model-Card.pdf
- InfoWorld, Microsoft launches MAI Code 2 models to challenge OpenAI and Anthropic in AI coding: https://www.infoworld.com/article/4250152/microsoft-launches-mai-code-2-models-to-challenge-openai-and-anthropic-in-ai-coding.html
- Axios, Microsoft aims to challenge OpenAI, Anthropic with cheaper, specialized AI models: https://www.axios.com/2026/11/17/microsoft-ai-models-openai-anthropic-ignite
- Unite.AI, Microsoft AI releases MAI Code 2 and MAI Code 2 Flash for enterprises: https://www.unite.ai/microsoft-ai-releases-mai-code-2-and-mai-code-2-flash-for-enterprises/
- Beri, MAI Code 2 prices double in GitHub Copilot on May 17, 2027: https://www.beri.net/article/mai-code-2-flash-github-copilot-pricing-promo-ends-may-2027-msbench-vs-gpt-6-1-sol-opus-5-5-model-policy-mai-code-1-1-flash-deprecation

## Top comments (0)
