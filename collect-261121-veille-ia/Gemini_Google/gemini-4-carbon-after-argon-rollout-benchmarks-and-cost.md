---
id: collect-261121-veille-ia/gemini-google/gemini-4-carbon-after-argon-rollout-benchmarks-and-cost
title: "Gemini 4 Carbon after Argon: rollout, benchmarks and cost"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-07-01"]
keywords: ["carbon", "argon", "google", "gemini", "model", "index", "opus", "october"]
source: https://blog.invidelabs.com/gemini-4-carbon-after-argon/
source_anchor: ""
source_lines: [1, 68]
section: "Gemini_Google"
---
Google announced Gemini 4 Carbon on October 28, with access starting this week for a subset of cyber defenders and no date for broader developer access. Argon, released to paid API users on October 13, is the Gemini 4 model available to integrate today.

Carbon belongs in the next model evaluation. Artificial Analysis places Carbon first on its v5.0 Intelligence Index at 59, one point above Claude Opus 5.5 and nine above Argon. Its cost per Index task is close to Argon's when priced at the announced post-introductory rates. Google's claimed gains target long debugging runs and security work. Build the evaluation on Argon and Claude Opus 5.5 now, then rerun the same cases when Carbon becomes available.

## Argon's rollout explains where Carbon stands

Google introduced Argon in two stages. Its September 30 announcement opened access to trusted cyber defenders through the Fairwind Program. The broader launch followed on October 13, including the paid API for developers and the Gemini app and AI Mode in Search for Google AI Pro and Ultra subscribers. Carbon arrived 15 days after that broader launch.

Google presented Argon as a model for sustained work on large codebases and complex professional tasks. At the October launch, Google reported a rise of more than 35% in agentic code submissions and nearly a third fewer rollbacks among its own teams using Antigravity.

Carbon repeats the cyber-defender-first approach. Fairwind is a vetted program that prioritizes governments, critical infrastructure operators and core technology platforms. Google directs applicants to an interest form and vets their security history and record of ethical operations.

Argon took 13 days to go from its Fairwind announcement to the paid API. Google has given Carbon no broader release date. As of October 29, the Gemini API model catalog lists `gemini-4-argon` as stable and has no Carbon entry.

## What the benchmarks show

Artificial Analysis's v5.0 Index page names Carbon as its highest-scoring model, followed by Anthropic's Claude Opus 5.5. On October 29, its model pages reported:

| Artificial Analysis measure | Gemini 4 Argon | Gemini 4 Carbon | Claude Opus 5.5 | 
|---|---|---|---|
| Evaluated reasoning setting | High | High | Max effort, default fallback | 
| Intelligence Index v5.0 score | 50 | 59 | 58 | 
| Output tokens generated during Index evaluation | 110 million | 200 million | 220 million | 
| Weighted cost per Index task at $4 input / $20 output per million tokens | $7.68 | $7.76 | $10.04 | 

Sources: Artificial Analysis model pages for Argon, Carbon and Opus 5.5. The Gemini prices used here are the announced post-introductory rates; Artificial Analysis also lists Opus at $4 per million input tokens and $20 per million output tokens. "Default fallback" is Artificial Analysis's configuration label; its model and methodology pages do not explain the routing or report which requests used a fallback.

The Index combines nine evaluations, including software engineering, terminal tasks and professional document reasoning. Carbon leads Opus by one displayed point on that composite score. The models' reasoning settings differ, and individual engineering tasks need their own comparison.

Google's Carbon announcement emphasizes persistence through difficult debugging work. It reports internal runs lasting days and more than twice Argon's success rate on an internal secure-code-generation benchmark. Google also reports approximately halving unsupported claims compared with Argon in internal testing. The claimed gains target failures such as abandoning difficult fixes and inventing evidence of completion.

The reported cyber results cover different steps in investigating and repairing vulnerabilities:

| Evaluation | Task | Google-reported Carbon result | 
|---|---|---|
| SEC-bench Pro | Finding vulnerabilities | 88.2% | 
| ExploitGym | Validating exploitation | 54.8% | 
| CWE-bench | Patching vulnerabilities | 71.0% | 

Google evaluated these Carbon results without guardrails and says it will keep iterating on guardrails before making Carbon available to developers. Its Argon launch post offers Fairwind partners Argon without cyber guardrails, and describes a multi-layer defense stack combining jailbreak resistance, monitoring and enforcement. Evaluate the version and controls actually provided, using authorized targets and patches reviewed against the project's tests.

## The same token prices leave task costs open

Carbon's announced introductory prices match Argon's: $2 per million input tokens and $10 per million output tokens, with a 95% discount on cached input. The announced later rates are $4 and $20 respectively.

Argon's developer guide specifies that its introductory period ends November 30, with higher rates starting December 1. Google has not said when Carbon's introductory rates end. Through November 30, Argon's input and output prices are half Opus 5.5's current $4/$20 list rates.

Argon is unavailable on the API's free tier. Its output billing includes thinking tokens. Explicit context caching is unsupported. Implicit caching is enabled for prompts of at least 4,096 tokens, with the discount applying on cache hits. For hard prompts, Google recommends background execution or HTTP timeouts of at least 15 minutes.

At the $4 input and $20 output rates per million tokens used by Artificial Analysis, Carbon costs $7.76 per weighted Index task, compared with $10.04 for Opus 5.5.

The output-token totals count tokens across the evaluations for each model. Cost per Index task is a weighted average. Each benchmark's cost is divided by its task count, then weighted by its share of the Index. The raw token totals and weighted task costs therefore measure different aggregates.

For an engineering budget, record the full attempt through acceptance. A patch that needs another agent run and an hour of reviewer repair costs more than the first response. Conversely, higher model spending can be justified if a difficult fix passes review with less human work.

## What to evaluate before changing the team's default

Run Argon and Opus 5.5 on a small set of completed engineering tasks whose results the team can judge. Include a bug that required tracing behavior across modules and a security patch with a regression test. Keep the starting commit, instructions and available tools consistent across model runs. Record whether the fix survives review, how much intervention it needs and whether the agent accurately reports unfinished work.

Argon's default `thinking_level` is `"medium"`, while its Artificial Analysis result above uses high reasoning. Set `thinking_level: "high"` for a comparison at that setting, or test `low`, `medium` and `high` as Google's developer guide recommends. Record each model's setting alongside the result.

Opus 5.5 also defaults to medium effort. Anthropic's effort guide uses `output_config={"effort": "max"}` to select max effort. To enable Anthropic's beta default fallback, set `fallbacks="default"` with the `server-side-fallback-2026-07-01` beta header. That option can retry a request declined by a safety classifier on another recommended model. Check the returned `model` field and `fallback_message` entries in `usage.iterations` to record which model answered. These are documented API settings for the local baseline; the checked Artificial Analysis pages do not confirm that this beta option produced their labelled result.

Give the evaluation a spending limit and a stopping condition. A multiday Carbon run needs to end in a reviewable patch or a specific account of what blocked it. Measure reviewer time alongside model and tool costs, and inspect any changes to tests or permissions before accepting the result.

Keep a record of both baselines' failures, including premature stopping, unsupported completion claims and unsuccessful patches. When Carbon reaches the API, rerun those cases with the same starting commits and tests, then compare accepted fixes, review time and total spending.

## Member discussion
