---
id: collect-261121-veille-ia/gpt-openai/gpt-6-astra-vs-gpt-6-1-sol-vs-luna-pricing-benchmarks
title: "GPT-6 Astra vs GPT-6.1 Sol vs Luna: Pricing, Benchmarks"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["gpt-6", "astra", "index", "luna", "effort", "task", "terra", "openai"]
source: https://dervity.com/blog/gpt-6-astra-sol-luna-guide
source_anchor: ""
source_lines: [1, 279]
section: "GPT_OpenAI"
---
GPT-6 Astra is OpenAI's top model at $10/$50 per million tokens and scores 56.9 on the Artificial Analysis Intelligence Index at max effort. GPT-6.1 Sol scores 54.6 at $2/$10. That's 2.3 points behind Astra for $1.27 per benchmark task against Astra's $6.24. GPT-6.1 Terra, new on October 26, scores 51.3 at $1/$5. GPT-6 Luna costs $0.10/$0.50, scores 27.9 and costs about 15 cents a task.

Use GPT-6.1 Sol for almost everything. Terra is the cheaper tier for background jobs, and Astra is worth its price for quick answers from a top model and for long-form writing. Luna has been overtaken at its own price: Claude Haiku 5.5 and Muse Ember also cost $0.10/$0.50 and score 36.5 and 36.0.

**Update (November 12, 2026):** Artificial Analysis replaced its Intelligence Index v4.3.2 with v5.0.1 and its Coding Agent Index v1.5 with v2.0. Every score in this guide is new and can't be compared with the September version. OpenAI also added GPT-6.1 Terra, so there's a Terra tier again. Outside OpenAI, Claude Fable 5.2 now leads the Intelligence Index at 64.6.

Scores come from Artificial Analysis (AA): Intelligence Index v5.0.1 and Coding Agent Index v2.0. We compare models at max effort, the setting AA and OpenAI report, and show the cost per task and answer time at that setting. The tables below cover every effort level, including the fastest ones. Per-effort data was pulled on November 12, 2026.

## What's the difference between GPT-6 Astra, GPT-6.1 Sol, Terra and Luna?

The four tiers share a 1.05M-token context window and 128K max output, and all take text and images. They differ in price, score and how they spend reasoning effort. OpenAI's docs pitch GPT-6.1 Sol as "near-Astra performance" for coding, computer use and professional work, and Luna at focused, high-volume tasks.

|  | GPT-6 Astra | GPT-6.1 Sol | GPT-6.1 Terra | GPT-6 Luna | 
|---|---|---|---|---|
| Released | Sep 3, 2026 | Sep 29, 2026 | Oct 26, 2026 | Sep 22, 2026 | 
| Input / output per 1M tokens | $10 / $50 | $2 / $10 | $1 / $5 | $0.10 / $0.50 | 
| Context window | 1.05M | 1.05M | 1.05M | 1.05M | 
| Max output | 128K | 128K | 128K | 128K | 
| Reasoning efforts tested | low to max | low to max | none to max | none to max | 
| Intelligence Index, max | 56.9 | 54.6 | 51.3 | 27.9 | 
| Cost per task, max | $6.24 | $1.27 | $0.96 | $0.15 | 
| Full answer, max | 358 s | 419 s | 412 s | 127 s | 
| Best score under 45 s | 51.6 (medium, 22 s) | 46.3 (medium, 18 s) | 42.4 (high, 37 s) | 21.4 (high, 38 s) | 
| Coding Agent Index v2.0 | 55 | 54 | Not published | Not published | 

At max effort, GPT-6.1 Sol costs a fifth of Astra per token and 20% as much per task. Terra costs a quarter less than Sol per task at max. Max effort is slow on every tier except Luna, at six to seven minutes per answer, so the "under 45 s" row is the one to read for anything a person waits on.

Cached input costs $1.00 per million on Astra, $0.10 on GPT-6.1 Sol and $0.01 on Luna, per OpenAI's docs. API names are `gpt-6-astra`, `gpt-6.1-sol` and `gpt-6-luna`.

## GPT-6.1 Sol: what changed a week after GPT-6 Sol

GPT-6.1 Sol replaced GPT-6 Sol on September 29 at the same $2/$10. On the new index the gap is 10.4 points at max effort.

|  | GPT-6 Sol | GPT-6.1 Sol | 
|---|---|---|
| Released | Sep 22, 2026 | Sep 29, 2026 | 
| Input / output per 1M tokens | $2 / $10 | $2 / $10 | 
| Cached input per 1M | $0.20 | $0.10 | 
| Intelligence Index v5.0.1, max | 44.2 | 54.6 | 
| Cost per task, max | $2.47 | $1.27 | 
| Full answer, max | 154 s | 419 s | 
| Reasoning efforts | none to max | low to max | 
| Knowledge cutoff | Apr 20, 2026 | Apr 30, 2026 | 

The gains are broad. In its September write-up, AA measured 12 more points on Terminal-Bench 4.0 (55% against 43%), more accuracy on AA-Omniscience and a lower hallucination rate there (54% against 60%).

Two things got worse. AA found GPT-6.1 Sol writes 10 to 30% more output tokens than GPT-6 Sol at each effort level, and max effort now takes about seven minutes per answer instead of two and a half. It still costs about half as much per task at max, and the cache discount rises from 90% to 95%.

If you're moving code over, check two details in OpenAI's model page. The `none` reasoning effort is gone, so the fastest setting is now `low`. Tool calling needs the Responses API, because Chat Completions runs GPT-6.1 Sol without tools.

## GPT-6.1 Terra: a Terra tier again

When GPT-6 launched in September there was no Terra. OpenAI added GPT-6.1 Terra on October 26 at $1/$5, half of Sol's per-token price. It scores 51.3 at max, 3.3 points below Sol, for $0.96 per task against Sol's $1.27.

At interactive settings Sol is the better buy. GPT-6.1 Sol at medium scores 46.3 in 18 seconds for $0.36. Terra's best setting under 45 seconds is high, which scores 42.4 in 37 seconds for $0.26. That's 10 cents less for 3.9 fewer points and twice the wait.

Terra makes sense for background work at extra high or max, where it saves about a quarter per task against Sol, and at its `none` setting, which scores 30.5 in about 9 seconds. AA hasn't published a Coding Agent score for it, and EQ-Bench hasn't tested it.

## GPT-6 benchmarks: Sol stays close to Astra

At max effort, Astra scores 56.9, GPT-6.1 Sol 54.6, Terra 51.3 and Luna 27.9. Under a 45-second answer limit the order holds at 51.6, 46.3, 42.4 and 21.4. On the Coding Agent Index v2.0, which runs each model inside Codex, Astra scores 55 and GPT-6.1 Sol 54. AA hasn't published Terra or Luna.

AA's September write-ups on Astra, Sol and Luna and GPT-6.1 Sol broke out a few of the evaluations behind the scores:

| AA evaluation (September 2026) | GPT-6 Astra | GPT-6.1 Sol | GPT-6 Luna | 
|---|---|---|---|
| Terminal-Bench 4.0 | 59% | 55% | 13% | 
| Hallucination rate, AA-Omniscience (lower is better) | 51% | 54% | 77% | 

Astra keeps a small lead on agentic terminal work and hallucinates a little less. Luna completed 13% of Terminal-Bench tasks, so it's the wrong model to run as a coding agent even though it writes decent code in a chat.

## What each reasoning effort costs and how long it takes

Reasoning effort changes the bill more than the tier does. Each table lists every setting AA tested, with the score, the cost of one Intelligence Index task including thinking tokens, the median time to a full answer, and the wait before the first answer token.

### GPT-6 Astra by effort

| Effort | Intelligence Index | Cost per task | Full answer | First token | 
|---|---|---|---|---|
| Low | 43.3 | $1.42 | 16.7 s | 4.1 s | 
| Medium | 51.6 | $2.73 | 22.0 s | 9.8 s | 
| High | 54.8 | $3.24 | 186.9 s | 175.7 s | 
| Extra high | 55.3 | $3.66 | 334.8 s | 325.2 s | 
| Max | 56.9 | $6.24 | 358.4 s | 347.4 s | 

Medium is Astra's sweet spot for anything interactive: 51.6 in 22 seconds. High adds 3.2 points for 19% more money but about eight times the wait. Max nearly doubles high's cost for 2.1 more points and takes six minutes, so keep it for background jobs.

### GPT-6.1 Sol by effort

| Effort | Intelligence Index | Cost per task | Full answer | First token | 
|---|---|---|---|---|
| Low | 34.8 | $0.16 | 14.5 s | 3.9 s | 
| Medium (default) | 46.3 | $0.36 | 18.2 s | 8.1 s | 
| High | 50.3 | $0.59 | 113.3 s | 104.0 s | 
| Extra high | 52.5 | $0.76 | 232.6 s | 224.8 s | 
| Max | 54.6 | $1.27 | 419.0 s | 410.8 s | 

The default is the standout. Medium scores 46.3 in 18 seconds for $0.36, 5.3 points below Astra at medium for 13% of the cost. High adds 4 points but takes nearly two minutes. Max adds 2.1 points over extra high for 67% more money and about seven minutes per answer.

### GPT-6.1 Terra by effort

| Effort | Intelligence Index | Cost per task | Full answer | First token | 
|---|---|---|---|---|
| None | 30.5 | $0.23 | 8.9 s | 2.0 s | 
| Low | 29.4 | $0.07 | 9.8 s | 3.0 s | 
| Medium | 38.2 | $0.17 | 14.5 s | 7.8 s | 
| High | 42.4 | $0.26 | 37.3 s | 31.0 s | 
| Extra high | 45.3 | $0.40 | 107.3 s | 101.2 s | 
| Max | 51.3 | $0.96 | 411.9 s | 406.1 s | 

Terra's jump from extra high to max is unusually large, 6 points, so if you run it in the background, run it at max. Low effort costs 7 cents a task and answers in under 10 seconds.

### GPT-6 Luna by effort

| Effort | Intelligence Index | Cost per task | Full answer | First token | 
|---|---|---|---|---|
| None | 9.4 | $0.071 | 6.0 s | 1.7 s | 
| Low | 9.7 | $0.005 | 7.5 s | 3.2 s | 
| Medium | 18.5 | $0.042 | 17.4 s | 13.1 s | 
| High | 21.4 | $0.073 | 38.0 s | 33.9 s | 
| Extra high | 23.5 | $0.10 | 55.3 s | 51.4 s | 
| Max | 27.9 | $0.15 | 126.6 s | 123.0 s | 

Luna scores much lower on the new index than it did in September, and the budget models that arrived since then beat it. Muse Ember scores 36.0 in about 11 seconds for $0.14 per task, ahead of Luna at max on score, speed and cost. Luna's one remaining niche is low effort, at half a cent per task, for routing and tagging you can check in code.

## GPT-6 vs Claude Fable 5.2, Opus 5.5, Sonnet 5.5, Gemini 4 Carbon and the budget models

Three models now score higher than any GPT-6 tier at max: Claude Fable 5.2, Gemini 4 Carbon and Claude Opus 5.5. Claude Sonnet 5.5 ties GPT-6.1 Sol. At max, the GPT-6 models are still the cheap ones per task.

| Model | Price per 1M (in / out) | Max score | Cost per task, max | Full answer, max | Best under 45 s | Coding Agent Index | 
|---|---|---|---|---|---|---|
| Claude Fable 5.2 | $8 / $40 | 64.6 | $26.54 | 247 s | 61.8 (high, 36 s) | – | 
| Gemini 4 Carbon | $2 / $10 (intro) | 60.0 (high) | $7.76 | 130 s | 58.3 (medium, 33 s) | 59 | 
| Claude Opus 5.5 | $4 / $20 | 59.1 | $10.04 | 788 s | 48.2 (medium, 32 s) | 62 | 
| GPT-6 Astra | $10 / $50 | 56.9 | $6.24 | 358 s | 51.6 (medium, 22 s) | 55 | 
| GPT-6.1 Sol | $2 / $10 | 54.6 | $1.27 | 419 s | 46.3 (medium, 18 s) | 54 | 
| Claude Sonnet 5.5 | $2 / $10 | 54.5 | $10.02 | 500 s | 41.5 (high, 19 s) | 59 | 
| GPT-6.1 Terra | $1 / $5 | 51.3 | $0.96 | 412 s | 42.4 (high, 37 s) | – | 
| Claude Haiku 5.5 | $0.10 / $0.50 | 36.5 | $0.41 | 371 s | 29.9 (high, 33 s) | – | 
| Muse Ember | $0.10 / $0.50 | 36.0 | $0.14 | 11 s | 36.0 (max, 11 s) | – | 
| GPT-6 Luna | $0.10 / $0.50 | 27.9 | $0.15 | 127 s | 21.4 (high, 38 s) | – | 

GPT-6.1 Sol costs an eighth of Opus 5.5 per task at max and about a twenty-first of Fable 5.2, for 4.5 and 10 fewer points. Per-task cost doesn't follow list price either. Sonnet 5.5 shares Sol's $2/$10 price and costs about eight times as much per task at max, because it thinks far longer. Gemini 4 Carbon shares the price too and costs six times as much at its best setting, for 5.4 more points.

### GPT-6 Astra vs Claude Opus 5.5

Opus 5.5 is the stronger model: 2.2 points ahead at max and 62 against 55 on the Coding Agent Index. At max it costs $10.04 per task against Astra's $6.24 and takes about 13 minutes per answer against six. Under a 45-second limit Astra wins: at medium it scores 51.6 in 22 seconds for $2.73, and Opus 5.5 at medium scores 48.2 in 32 seconds for $2.45. Pick Opus 5.5 for coding, and Astra when you need fast answers inside ChatGPT or Codex, or when the job is writing.

### GPT-6.1 Sol vs Claude Sonnet 5.5

Both cost $2/$10 per million tokens and they tie at max, 54.6 against 54.5. Sonnet 5.5 spends $10.02 per task and more than eight minutes per answer to get there. Sol does it for $1.27. Under 45 seconds, Sol at medium scores 46.3 in 18 seconds for $0.36, and Sonnet 5.5 at high scores 41.5 in 19 seconds for $1.64. Sol's cache reads also cost half as much ($0.10 against $0.20).

Sonnet 5.5's edge is coding: 59 on the Coding Agent Index against Sol's 54. Pick Sonnet 5.5 for coding in Claude Code. Pick GPT-6.1 Sol for agents, analysis and anything that runs thousands of times a day.

### GPT-6 Astra vs Claude Fable 5.2

Fable 5.2 costs less per token ($8/$40 against $10/$50) and scores 7.7 points higher at max. At max it's expensive, $26.54 per task against Astra's $6.24. At its faster settings it competes on cost too: at low effort Fable 5.2 scores 53.9 in about 21 seconds for $3.10, above Astra at medium (51.6, 22 seconds, $2.73) for 37 cents more. Fable 5.2 is the better model. Astra is cheaper at max and still leads EQ-Bench, which hasn't tested Fable 5.2.

### GPT-6.1 Sol vs Gemini 4 Carbon

Both cost $2/$10, though Carbon's price is a 50% introductory offer and its list price is $4/$20. Carbon scores higher at every setting: 60.0 at high against Sol's 54.6 at max, and 58.3 against 46.3 at medium. It also leads on coding, 59 to 54. The cost is per task: at medium AA spent $6.59 on Carbon and $0.36 on Sol. Pick Carbon for the best answer per call at this price, and Sol for volume.

### GPT-6 Luna vs Claude Haiku 5.5 and Muse Ember

All three cost $0.10/$0.50. Muse Ember scores 36.0 at max in about 11 seconds for $0.14 per task. Claude Haiku 5.5 scores 36.5 at max but takes about six minutes, or 29.9 at high in 33 seconds for $0.16. Luna's best is 27.9 at max in about two minutes. For new budget work, start with Ember.

## Creative writing: Astra ranks first on EQ-Bench

GPT-6 Astra has the highest Elo on EQ-Bench Creative Writing v3, at 2176.9. GPT-6 Sol scored 2126.0, above Claude Opus 5.5. EQ-Bench hasn't scored GPT-6.1 Sol, GPT-6.1 Terra or GPT-6 Luna yet.

| Model | EQ-Bench Elo | Slop score (lower is better) | 
|---|---|---|
| GPT-6 Astra | 2176.9 | 8.41 | 
| Kimi K3.1 Pro | 2172.6 | 9.64 | 
| Claude Fable 5.1 | 2155.3 | 8.16 | 
| GPT-6 Sol | 2126.0 | 8.97 | 
| Claude Opus 5.5 | 2050.9 | 10.43 | 
| GPT-5.6 Luna | 1831.5 | 11.80 | 
| Gemini 3.8 Flash | 1744.4 | 22.60 | 

The slop score counts how often a model leans on stock AI phrases. Both GPT-6 models EQ-Bench has tested score under 9, so their drafts need less cleanup than Opus 5.5's and far less than Gemini 3.8 Flash's.

## Should you upgrade from GPT-6 Sol or GPT-5.6?

Yes. GPT-6.1 Sol scores 10.4 points more than GPT-6 Sol at max for about half the cost per task. GPT-6 Luna scores 4.1 points more than GPT-5.6 Luna for less than half the cost per task. AA hasn't scored GPT-5.6 Sol or Terra on the new index.

|  | GPT-6 Sol | GPT-6.1 Sol | GPT-6.1 Terra | GPT-5.6 Luna | GPT-6 Luna | 
|---|---|---|---|---|---|
| Price per 1M (in / out) | $2 / $10 | $2 / $10 | $1 / $5 | $0.20 / $1.20 | $0.10 / $0.50 | 
| Max score | 44.2 | 54.6 | 51.3 | 23.8 | 27.9 | 
| Cost per task, max | $2.47 | $1.27 | $0.96 | $0.33 | $0.15 | 
| Full answer, max | 154 s | 419 s | 412 s | 160 s | 127 s | 
| EQ-Bench Elo | 2126.0 | Not tested | Not tested | 1831.5 | Not tested | 

The regressions are about time and settings. GPT-6.1 Sol at max takes about four and a half minutes longer per answer than GPT-6 Sol, and it drops the `none` effort. None of it changes the verdict.

If you're on GPT-5.6 Terra, move to GPT-6.1 Terra. It lists at $1/$5 against GPT-5.6 Terra's $2/$12. If you're on either Luna, test Claude Haiku 5.5 or Muse Ember before you settle on GPT-6 Luna. Our GPT-5.6 Sol vs Terra vs Luna guide has the full history of the old tiers.

## Which GPT-6 model to use for coding, agents, writing and bulk work

Inside the GPT-6 family, here's how we'd split the work. Our best-model-for pages rank every model across labs.

- Hard reasoning and code review56.9 at max and the top Coding Agent score in the familyAstra
- Daily coding in Codex54 on the Coding Agent Index, a point behind Astra, at a fifth of the per-token priceGPT-6.1 Sol
- Quick answers from a top model51.6 at medium in about 22 sAstra
- Long agent runs54.6 at max for $1.27 per taskGPT-6.1 Sol
- Background batch jobs51.3 at max for $0.96 per taskGPT-6.1 Terra
- Creative writing2176.9 on EQ-Bench; 6.1 Sol and Terra not tested yetAstra
- Routing and taggingHalf a cent per task at low effortLuna
- Categories wonAstra 3GPT-6.1 Sol 2GPT-6.1 Terra 1Luna 1

| Category | Astra | GPT-6.1 Sol | GPT-6.1 Terra | Luna | 
|---|---|---|---|---|
| Hard reasoning and code review56.9 at max and the top Coding Agent score in the family | Wins |  |  |  | 
| Daily coding in Codex54 on the Coding Agent Index, a point behind Astra, at a fifth of the per-token price |  | Wins |  |  | 
| Quick answers from a top model51.6 at medium in about 22 s | Wins |  |  |  | 
| Long agent runs54.6 at max for $1.27 per task |  | Wins |  |  | 
| Background batch jobs51.3 at max for $0.96 per task |  |  | Wins |  | 
| Creative writing2176.9 on EQ-Bench; 6.1 Sol and Terra not tested yet | Wins |  |  |  | 
| Routing and taggingHalf a cent per task at low effort |  |  |  | Wins | 
| Categories won | 3 | 2 | 1 | 1 | 

### Coding: GPT-6.1 Sol

If you work in Codex, run GPT-6.1 Sol. It scores a point below Astra on the Coding Agent Index for a fifth of the per-token price. Outside OpenAI, Claude Opus 5.5 (62) and Claude Sonnet 5.5 (59) score higher, so our best model for coding page is the place to compare across labs. Keep Luna away from coding agents: its 13% on Terminal-Bench 4.0 in AA's September tests says it can't finish multi-step terminal work.

### Agents: GPT-6.1 Sol to plan, Terra to execute

Sol sits within 2.3 points of Astra at max for a fifth of the cost per task, so it can handle planning and review. Terra can run the long, simple steps at a quarter less per task. Keep Astra for runs where a person is waiting on each turn, since it answers in 22 seconds at medium with a higher score than Sol's medium.

### Writing: Astra

GPT-6 Astra tops EQ-Bench at 2176.9. GPT-6 Sol scored 2126.0, ahead of Claude Opus 5.5, but EQ-Bench hasn't tested GPT-6.1 Sol yet. Until it does, Astra is the safe pick for fiction and long-form writing from OpenAI. Kimi K3.1 Pro scores within five points of Astra at a fifth of the per-token price if you can leave OpenAI.

### Cheap bulk work: look past Luna

At low effort Luna costs half a cent per task, which is fine for tagging and routing you can validate in code. For anything that needs judgment at the $0.10/$0.50 price, Muse Ember and Claude Haiku 5.5 score far higher.

## Where GPT-6 falls short

Max effort is slow across the family. Astra takes six minutes per answer at max, and GPT-6.1 Sol and Terra about seven, so max is for background jobs only. Astra's best interactive setting (medium, 22 seconds) scores 5.3 points below its max.

Claude Fable 5.2, Gemini 4 Carbon and Claude Opus 5.5 all outscore every GPT-6 tier at max. OpenAI's edge is cost: GPT-6.1 Sol gets within 4.5 points of Opus 5.5 for an eighth of the price per task.

Luna's max score of 27.9 trails the newer budget models at the same price. It's a cheap router, not a general assistant.

Coding Agent Index scores mix the model with its tool. GPT-6 runs in Codex and Claude runs in Claude Code, so a gap of a few points can come from the harness as much as from the model. You can compare the raw numbers on our benchmark dashboard.

## FAQ

### What is the difference between GPT-6 Astra, GPT-6.1 Sol and Luna?

Astra is the flagship at $10/$50 per million tokens and scores 56.9 on the Artificial Analysis Intelligence Index at max effort. GPT-6.1 Sol costs $2/$10 and scores 54.6. GPT-6.1 Terra costs $1/$5 and scores 51.3. Luna costs $0.10/$0.50 and scores 27.9. At max effort they cost about $6.24, $1.27, $0.96 and $0.15 per benchmark task. Sol handles coding and agent work, Terra background jobs, and Luna routing and tagging.

### Is GPT-6.1 Sol better than GPT-6 Sol?

Yes. On the Artificial Analysis Intelligence Index v5.0.1, GPT-6.1 Sol scores 54.6 at max effort against GPT-6 Sol's 44.2, at the same $2/$10 price and about half the cost per task ($1.27 against $2.47). In AA's September tests it also gained 12 points on Terminal-Bench 4.0 and cut the hallucination rate from 60% to 54%, and it halves the cached input price to $0.10. It's slower at max, about 419 seconds per answer against 154, and drops the `none` reasoning effort.

### Is there a GPT-6 Terra?

Yes, as GPT-6.1 Terra. OpenAI released it on October 26, 2026 at $1/$5 per million tokens, half of GPT-6.1 Sol's price. It scores 51.3 on the Artificial Analysis Intelligence Index at max effort, 3.3 points below Sol, and costs $0.96 per benchmark task against Sol's $1.27. The September GPT-6 launches had no Terra tier, which is why older guides say there isn't one.

### How much does GPT-6 cost per task?

At max effort, Artificial Analysis measured $6.24 per Intelligence Index task for GPT-6 Astra, $1.27 for GPT-6.1 Sol, $0.96 for GPT-6.1 Terra and $0.15 for Luna, thinking tokens included. Faster settings cost less: Astra at medium costs $2.73 and answers in 22 seconds, and GPT-6.1 Sol at medium costs $0.36 and answers in 18 seconds.

### Is GPT-6 Astra better than Claude Opus 5.5?

No. Claude Opus 5.5 scores 59.1 on the Artificial Analysis Intelligence Index at max effort against Astra's 56.9, and 62 against 55 on the Coding Agent Index v2.0. Astra is cheaper at max ($6.24 per task against $10.04), and at medium it scores 51.6 in 22 seconds, above Opus 5.5 at medium (48.2 in 32 seconds). It also ranks first for creative writing on EQ-Bench. Claude Fable 5.2 and Gemini 4 Carbon score above both.

### GPT-6.1 Sol or Claude Sonnet 5.5?

Both cost $2/$10 per million tokens and they tie at max effort (54.6 against 54.5), but Sonnet 5.5 costs $10.02 per task there against $1.27 for GPT-6.1 Sol. Under 45 seconds, Sol at medium scores 46.3 for $0.36 per task and Sonnet 5.5 at high scores 41.5 for $1.64. Sonnet 5.5 leads on the Coding Agent Index, 59 to 54. Pick Sonnet 5.5 for coding in Claude Code and Sol for volume.

### Should I upgrade from GPT-5.6 to GPT-6?

Yes. Artificial Analysis hasn't scored GPT-5.6 Sol or Terra on Intelligence Index v5.0.1, but GPT-6.1 Sol (54.6) and GPT-6.1 Terra (51.3) cost the same or less per token than both. GPT-6 Luna scores 27.9 against GPT-5.6 Luna's 23.8 for less than half the cost per task. For new budget work, Claude Haiku 5.5 and Muse Ember score higher than either Luna at $0.10/$0.50.

*Scores from Artificial Analysis Intelligence Index v5.0.1 and Coding Agent Index v2.0, with per-effort costs and times measured on November 12, 2026. Terminal-Bench 4.0 and hallucination figures from AA's September 2026 GPT-6 Astra, GPT-6 Sol and Luna and GPT-6.1 Sol articles. Prices, context windows and effort levels from OpenAI's model docs for Astra, GPT-6.1 Sol, GPT-6 Sol and Luna, and from Artificial Analysis for GPT-6.1 Terra. Creative writing scores from EQ-Bench Creative Writing v3, checked November 12, 2026. For live rankings of every model, see the LLM Leaderboard.*
