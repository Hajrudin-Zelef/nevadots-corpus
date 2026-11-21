---
id: collect-261121-veille-ia/deepseek/deepseek-v4-1-flash-1017-benchmarks-vs-opus-5-pricing-switch
title: "DeepSeek V4.1-Flash-1017: Benchmarks vs Opus 5, Pricing, Switch?"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: ["2026-10-17"]
keywords: ["deepseek", "flash-1017", "benchmarks", "model", "opus", "flash", "terminal-bench", "deepseek-flash"]
source: https://www.developersdigest.tech/blog/deepseek-v4-1-flash-1017-release-guide-2026
source_anchor: ""
source_lines: [1, 189]
section: "DeepSeek"
---
TL;DR

DeepSeek V4.1-Flash-1017 jumps 11 points on Terminal-Bench 4.0 at the same deepseek-flash price. Benchmarks vs Opus 5, K3, GLM-5.3 and when to switch.

**Last updated:** October 17, 2026

DeepSeek V4.1-Flash-1017 is a same-price checkpoint update to DeepSeek's `deepseek-flash` API model, released October 17, 2026 with MIT-licensed weights. By DeepSeek's own numbers it gains 11.1 points on Terminal-Bench 4.0 and 8.8 on ProgramBench over the September V4.1-Flash, and it beats Opus 5 on DeepSWE v1.1 (77.0 vs 74.0). If you already run `deepseek-flash`, you are on it now; the question is whether it earns more of your agent work.

## Official Sources#

| Resource | What it confirms | 
|---|---|
| DeepSeek release note | what shipped, harness partners, open-source release | 
| DeepSeek X announcement | the launch thread, October 17, 2026 | 
| Models & Pricing | model id `deepseek-flash` , version string, rates, context, peak hours | 
| Hugging Face model card | benchmark table, sampling settings, license, prompt encoding | 

## What Shipped#

DeepSeek's launch post on X calls 1017 "a Pareto improvement in performance and cost": stronger benchmarks, faster task completion and shorter outputs than the previous version. It is live on DeepSeek's website, app and API, and DeepSeek says it is tuned for DeepSeek Harness (v0.2.1-rc.1 or later), Tencent CodeBuddy and WorkBuddy. Our first look at DeepSeek Harness covers that client.

The facts that matter for a config file, all from the pricing page:

- **Model id:**`deepseek-flash` . The version listed behind it is now`DeepSeek-V4.1-Flash-1017` .
- **Legacy names:**`deepseek-v4-flash` and`deepseek-v4-flash-vision-exp` are still accepted, served by 1017 and billed at the Flash price.
- **Context and output:** 1M context, 384K maximum output.
- **Features:** thinking mode on by default (non-thinking available), tool calls, JSON output, Responses API, an Anthropic-format endpoint at`https://api.deepseek.com/anthropic` , and vision on Flash.
- **Weights:** MIT license, on Hugging Face and ModelScope.

The architecture is the one DeepSeek described for V4.1-Flash in September: a 552B-parameter MoE with a causal encoder-decoder layout that activates 8B parameters for input and 16B for output. DeepSeek's model card calls 1017 "a minor update" to that model.

DeepSeek also says the gains come from "reinforcement learning with self-feedback" aimed at end-to-end project development, and shows demos ranging from a web racing game to a Godot recreation of Portal 2. Treat those as vendor demos, not measurements.

## Benchmarks: 1017 vs the September V4.1-Flash#

All numbers below are vendor-reported, from the model card. No independent lab has published 1017 results yet.

| Benchmark | V4.1-Flash-1017 | V4.1-Flash | Change | 
|---|---|---|---|
| HLE (no tools) | 42.6 | 36.8 | +5.8 | 
| Terminal-Bench 4.0 | 42.3 | 31.2 | +11.1 | 
| DeepSWE v1.1 | 77.0 | 74.2 | +2.8 | 
| ProgramBench | 29.1 | 20.3 | +8.8 | 
| NL2Repo-Bench | 68.2 | 65.4 | +2.8 | 
| Automation-Bench v1.0.6 | 60.5 | 54.8 | +5.7 | 
| Agents' Last Exam | 35.4 | 31.8 | +3.6 | 
| Apex Bench v1.1 | 72.1 | 64.6 | +7.5 | 
| Chartography (with tools) | 83.0 | 78.9 | +4.1 | 
| BabyVision (with tools) | 92.5 | 89.6 | +2.9 | 
| ZeroBench-main (with tools) | 51.0 | 49.0 | +2.0 | 

The pattern is clear: the largest jumps are on long-horizon agent work (Terminal-Bench, ProgramBench, Apex), which lines up with the "faster task completion" claim. The model card says Terminal-Bench 4.0 was run in the Claude Code harness and DeepSWE v1.1 in the Mini-SWE-V2 harness.

## Benchmarks: 1017 vs K3, GLM-5.3, GPT-5.6 Sol and Opus 5#

These cross-vendor numbers are also DeepSeek-reported, from the same table. Each vendor runs benchmarks its own way, so read them as DeepSeek's claim, not a neutral leaderboard.

| Benchmark | V4.1-Flash-1017 | Kimi K3 | GLM-5.3 | GPT-5.6 Sol | Opus 5 | 
|---|---|---|---|---|---|
| HLE (no tools) | 42.6 | 43.5 | 42.0 (text-only) | 44.5 | **56.3** | 
| Terminal-Bench 4.0 | 42.3 | 12.6 | 37.9 | 39.9 | **51.8** | 
| DeepSWE v1.1 | **77.0** | 67.5 | 66.9 | 73.0 | 74.0 | 
| ProgramBench | 29.1 | 17.5 | 19.0 | 23.0 | **37.0** | 
| NL2Repo-Bench | 68.2 | 58.0 | 58.0 | 56.8 | **75.3** | 
| Automation-Bench v1.0.6 | **60.5** | 46.7 | 48.8 | 45.8 | 50.3 | 
| Agents' Last Exam | **35.4** | 27.6 | 28.5 | 26.7 | 28.6 | 
| Apex Bench v1.1 | **72.1** | 47.8 | 56.6 | 54.3 | 65.8 | 
| Chartography (with tools) | 83.0 | 68.1 | n/a | 79.9 | **84.0** | 
| BabyVision (with tools) | 92.5 | 85.7 | n/a | 88.9 | **94.1** | 
| ZeroBench-main (with tools) | 51.0 | 41.0 | n/a | **53.0** | 52.0 | 

**Where 1017 leads:** DeepSWE v1.1, Automation-Bench, Agents' Last Exam and Apex Bench. It also beats K3 and GLM-5.3 on every row where both have a score, except HLE, where K3 edges it 43.5 to 42.6.

**Where Opus 5 still leads:** HLE by almost 14 points, Terminal-Bench 4.0, ProgramBench, NL2Repo-Bench, Chartography and BabyVision. GPT-5.6 Sol takes ZeroBench.

**What the table leaves out:** the comparison stops at Opus 5 and GPT-5.6 Sol. It does not include Claude Opus 5.5 or the GPT-6 generation, which is what most teams paying frontier prices run today. "Beats Opus 5" is not the same as "beats the current Anthropic flagship".

## Pricing and Context#

The `deepseek-flash` rates on the pricing page are unchanged from the ones we recorded on October 9 in our daily-coding cost breakdown. Per million tokens:

| `deepseek-flash` | Peak | Off-peak | 
|---|---|---|
| Input, cache hit | $0.006 | $0.003 | 
| Input, cache miss | $0.30 | $0.15 | 
| Output | $1.20 | $0.60 | 

Off-peak is half of peak. Peak hours are 01:00-04:00 and 06:00-10:00 UTC, Monday through Friday, excluding Chinese public holidays; weekends and holidays are off-peak all day. The concurrency limit for `deepseek-flash` is 2,500.

For scale, list prices we have verified in earlier posts, input / output per million tokens:

| Model | Input | Output | Where we checked | 
|---|---|---|---|
| DeepSeek `deepseek-flash` (peak) | $0.30 | $1.20 | this post | 
| GLM-5.3 (Z.ai first-party) | $1.40 | $4.40 | GLM-5.3 access guide | 
| Kimi K3 (Moonshot) | $3 | $15 | Kimi K3 access guide | 
| Claude Opus 5 | $5 | $25 | Opus 5 comparison | 
| GPT-5.6 Sol | $4 | $20 | promo rate through at least November 21 (list $5/$30), per the September price war breakdown | 
| Claude Opus 5.5 (not in DeepSeek's table) | $4 | $20 | Opus 5.5 release guide | 
| GPT-6.1 Sol (not in DeepSeek's table) | $2 | $10 | September price war breakdown | 

Opus 5's output price is about 21 times Flash's peak output price, and about 42 times its off-peak price. Even GPT-6.1 Sol, the cheapest current frontier option above, charges about 8 times Flash's peak output rate. Benchmark rows where 1017 trails by a few points are the ones to weigh against that gap.

## Run It#

Because 1017 sits behind the existing model id, anything already pointed at `deepseek-flash` picked it up with no change. To start fresh in OpenCode, the OpenCode providers docs give this flow: create a key in the DeepSeek console, run `/connect` and pick DeepSeek, paste the key, then run `/models` to choose the model.

```
/connect
/models
```
Against the API directly, use the OpenAI-format base URL `https://api.deepseek.com` (or `https://api.deepseek.com/anthropic` for Anthropic-format clients) with `deepseek-flash` as the model name. The model card recommends `temperature` 1.0, `top_p` 1.0, and leaving `max_tokens` at 256K or more so thinking output is not cut off.

If you self-host, note that the release ships no Jinja chat template. The encoding folder holds a Python reference encoder, and DeepSeek's deepseek-recipe Rust libraries (with Python bindings) convert Messages, Chat Completions and Responses API requests into the V4.1 prompt format. Budget for a lot of hardware: on Hacker News, commenters debated whether even a pair of DGX Sparks is enough without quantization.

## Which One to Pick#

| If you need | Pick | Why | 
|---|---|---|
| A cheap default for routine coding agents | `deepseek-flash` (1017) | Leads DeepSWE v1.1 in DeepSeek's table at $1.20 peak output | 
| Long agent runs that you can schedule | `deepseek-flash` off-peak | Same model at half price outside the UTC peak windows | 
| Hard reasoning or hard debugging | Opus 5 or newer | Opus 5 leads HLE by about 14 points and Terminal-Bench 4.0 by 9.5 | 
| Repo-scale generation from a spec | Opus 5 or newer | Opus 5 leads NL2Repo-Bench and ProgramBench | 
| An open-weights alternative to compare | GLM-5.3 or Kimi K3 | 1017 out-scores both on most rows, but test your own workload | 

The practical pattern that keeps coming up is routing: a frontier model plans and reviews, a cheap model like Flash does the volume. Our budget models comparison works through that split.

## What People Are Actually Saying#

- **The benchmarks drew skepticism first.** On the Hacker News thread, one of the first replies questioned whether the numbers could be real, and two commenters pointed out the contradiction of calling a release "minor" while showing double-digit gains. Others replied that small changes can compound.
- **Calibration against newer models.** One commenter placed it close to Opus 5 but well behind Sol 6.1 and Opus 5.5. Another said they would wait for Artificial Analysis to confirm the numbers. That matches the gap in DeepSeek's own table.
- **The routing crowd is happy.** One practitioner described using Opus for planning, DeepSeek or GLM Flash for the bulk of the work, and a Sol model for review, with complex tasks capped around $10.
- **The counter-case.** Another commenter argued that cheaper models handle routine edits fine but stall on hard debugging, where a stronger model solved their problem first try. The weights release has its own r/LocalLLaMA thread for anyone planning to self-host.

## FAQ#

### What is the API model id for DeepSeek V4.1-Flash-1017?#

Use `deepseek-flash`. DeepSeek's pricing page lists DeepSeek-V4.1-Flash-1017 as the version behind that name. The legacy names `deepseek-v4-flash` and `deepseek-v4-flash-vision-exp` also route to 1017 and are billed at the Flash price.

### How much does DeepSeek V4.1-Flash-1017 cost?#

The same as V4.1-Flash before it: $0.30 per million cache-miss input tokens, $0.006 per million cache-hit input tokens and $1.20 per million output tokens at peak. Off-peak rates are half that. Peak is 01:00-04:00 and 06:00-10:00 UTC on weekdays, excluding Chinese public holidays.

### Is DeepSeek V4.1-Flash-1017 better than Claude Opus 5?#

On DeepSeek's own table it beats Opus 5 on DeepSWE v1.1, Automation-Bench, Agents' Last Exam and Apex Bench. Opus 5 still leads on HLE, Terminal-Bench 4.0, ProgramBench, NL2Repo-Bench, Chartography and BabyVision. All of these figures are vendor-reported, and the table does not include Opus 5.5.

### Are the DeepSeek V4.1-Flash-1017 weights open?#

Yes. The weights are on Hugging Face and ModelScope under the MIT license. The release has no Jinja chat template, so use the encoding folder's reference implementation or the deepseek-recipe libraries to build prompts.

### What context window does DeepSeek V4.1-Flash-1017 support?#

1M tokens of context and up to 384K output tokens on the API. The model card recommends leaving `max_tokens` at 256K or more.

## Sources#

## Continue Reading#

- DeepSeek 4.1 Flash as a Daily Coding Model: Cache, Peak Hours and Limits - per-session cost math and the peak-hour windows in detail
- Where to Run DeepSeek V4.1 Flash Free and Cheap - every route to the model and what each charges
- GLM-5.3 Free and Cheap: Every Route and Price (October 2026) - the open-weights rival 1017 is benchmarked against
- Where to Access Kimi K3: Every Provider and Price Compared (2026) - Moonshot's model and its host prices
- Claude Opus 5 vs Opus 4.8 vs Fable 5: Benchmark Comparison (July 2026) - the frontier baseline in DeepSeek's table
- DeepInfer: What DeepSeek Open-Sourced and What You Can Run - the inference engine that serves this model

## Get the next comparison like this in your inbox

One email a week on DeepSeek and the rest of the AI dev stack. Free.

Read next on local and open-weight models

### DeepSeek 4.1 Flash as a Daily Coding Model: Cache, Peak Hours and Limits

DeepSeek 4.1 Flash as a daily coding model: what a session costs, how peak hours double the bill, why caching matters, and where the subsidy argument holds.

8 min read
### Where to Run DeepSeek V4.1 Flash Free and Cheap

DeepSeek V4.1 Flash replaced V4 Flash. Official API: $0.15 input, $0.60 output off-peak. OpenRouter hosts start near $0.05. MIT weights are on Hugging Face.

6 min read
### GLM-5.3 Free and Cheap: Every Route and Price (October 2026)

GLM-5.3 has no free hosted tier. Cheapest on October 9: Wafer via OpenRouter at $0.049/$3.39, Flash from $0.075/$0.25. Z.ai's promos ended October 7.

13 min read
Technical content at the intersection of AI and development. Building with AI agents, Claude Code, and modern dev tools - then showing you exactly how it works.
