---
id: collect-261121-veille-ia/gpt-openai/gpt-5-6-pricing-what-sol-terra-and-luna-cost-and-how-to-keep-the-bill-down
title: "GPT-5.6 pricing: what Sol, Terra, and Luna cost and how to keep the bill down"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["gpt-5", "terra", "luna", "model", "cost", "input", "bill", "flagship"]
source: https://apidog.com/blog/gpt-5-6-pricing/
source_anchor: ""
source_lines: [1, 142]
section: "GPT_OpenAI"
---
OpenAI shipped GPT-5.6 to general availability on July 9, 2026, and for the first time a flagship GPT release arrived as three models with three separate prices. Sol handles the hardest reasoning at $5 per million input tokens and $30 per million output. Terra takes the middle lane at $2.50 and $15. Luna covers high-volume work at $1 and $6. That is a 5x spread from top to bottom, which makes model choice the single biggest lever on your API bill this year.

There is a trap hiding in the naming. The bare alias `gpt-5.6` routes to Sol, the flagship reasoning tier. Copy a quickstart, leave the model string alone, and every production request pays flagship rates for work Luna could handle at a fifth of the cost. The default is the expensive one, and nothing in the response will warn you.

This guide lays out the full rate card, runs worked cost examples on each tier, walks through the caching math, explains how reasoning effort and ultra mode change spend, and closes with routing patterns that keep the bill flat without giving up quality where it counts.

## TL;DR

- GPT-5.6 pricing per 1M tokens: Sol $5 input / $30 output, Terra $2.50 / $15, Luna $1 / $6.
- The bare `gpt-5.6` alias routes to Sol. Pin`gpt-5.6-terra` or`gpt-5.6-luna` unless you want flagship rates by default.
- OpenAI positions Terra as competitive with GPT-5.5 at roughly half the price, which makes it the natural migration target.
- Prompt caching: writes bill at 1.25x the input rate, reads get a 90% discount, and the minimum cache life is 30 minutes. Caching pays for itself from the second request.
- Higher reasoning effort means more output tokens; ultra mode runs four agents in parallel and multiplies token spend deliberately.
- ChatGPT Free and Go plans get Terra; Plus and above pick between all three.
- Compare tiers side by side and watch per-request token usage in Apidog before you commit a model ID to production.

## The GPT-5.6 rate card

Here is what each model costs per million tokens, including the derived caching rates (reads at 10% of the input price, writes at 125%):

| Model | Input / 1M | Output / 1M | Cached input read / 1M | Cache write / 1M | 
|---|---|---|---|---|
| `gpt-5.6-sol` (alias:`gpt-5.6` ) | $5.00 | $30.00 | $0.50 | $6.25 | 
| `gpt-5.6-terra` | $2.50 | $15.00 | $0.25 | $3.13 | 
| `gpt-5.6-luna` | $1.00 | $6.00 | $0.10 | $1.25 | 

The model IDs are confirmed in OpenAI’s developer docs, and API access is self-serve for any API account. There is no plan gating on the API side; the June preview restriction ended before GA and is history.

One more time, because it will bite someone on your team: `gpt-5.6` without a suffix is Sol. If your config, SDK default, or copied example uses the bare alias, you are paying $5/$30 rates whether or not the task needs them. Pin the full model ID everywhere and make the tier choice explicit in code review.

## What the tiering means

The number is the generation; Sol, Terra, and Luna are durable capability tiers that will advance on their own cadence. Knowing what each tier is for is most of the cost decision.

**Terra is the price-performance headline.** OpenAI positions it as competitive with GPT-5.5 at roughly 2x cheaper. If you run GPT-5.5 in production today, Terra is the drop-in candidate that cuts your rate card in half; check your current spend against the GPT-5.5 pricing breakdown to size the saving before you migrate. Treat the quality claim as OpenAI’s until your own evals confirm it, but the price cut is unconditional.

**Luna is the volume tier.** At $1/$6 it is built for classification, extraction, routing, and first-pass drafting: the high-frequency, latency-sensitive work where per-request cost dominates every other concern.

**Sol is for the problems that fail elsewhere.** Deepest reasoning, highest rates. Simon Willison’s launch-day review is worth reading for a practitioner’s view of where the flagship earns its premium and where it does not.

All three reportedly share a 1M-token context window and 128K max output, per early documentation coverage, so you are not trading capacity when you route down a tier. You are trading reasoning depth for price.

## What requests cost in practice

Per-million rates are abstract until you price a real request. Take a typical RAG-style call: 10,000 input tokens (system prompt, retrieved context, user question) and 1,000 output tokens.

| Model | Input cost | Output cost | Total per request | 
|---|---|---|---|
| Sol | $0.050 | $0.030 | $0.080 | 
| Terra | $0.025 | $0.015 | $0.040 | 
| Luna | $0.010 | $0.006 | $0.016 | 

One request looks cheap everywhere. Volume is where the tiers separate. Price a classification workload: 1 million requests per month, 500 input tokens and 50 output tokens each. That is 500M input tokens and 50M output tokens monthly.

| Model | Input | Output | Monthly total | 
|---|---|---|---|
| Luna | $500 | $300 | $800 | 
| Terra | $1,250 | $750 | $2,000 | 
| Sol | $2,500 | $1,500 | $4,000 | 

Run that workload on the bare `gpt-5.6` alias and you pay $4,000 a month for a job Luna does for $800. The alias default is a $3,200-per-month typo, every month, until someone reads the invoice.

## Caching economics, with real math

GPT-5.6 uses explicit cache breakpoints: you opt in with `prompt_cache_options.mode: "explicit"` and a `ttl` field, instead of relying on automatic prefix detection. Three numbers govern the economics. Cache writes bill at 1.25x the uncached input rate. Cache reads keep the 90% discount. Minimum cache life is 30 minutes.

```
{
  "model": "gpt-5.6-terra",
  "input": [
    { "role": "system", "content": "You are a support triage assistant. Classify each ticket..." },
    { "role": "user", "content": "Ticket #4821: webhook retries firing twice after 502s" }
  ],
  "prompt_cache_options": { "mode": "explicit", "ttl": "30m" }
}
```
Now the worked example. Say you have a 5,000-token system prompt reused across 100 requests within the cache window, running on Sol.

- Uncached: 100 requests x 5,000 tokens = 500,000 prefix tokens at $5 per 1M = **$2.50**
- Cached: one write (5,000 tokens at $6.25 per 1M = $0.031) plus 99 reads (495,000 tokens at $0.50 per 1M = $0.248) = **about $0.28**

That is roughly 89% off the prefix portion of the bill. The breakeven point sits at the second request: the write premium costs 25% of one uncached pass, while every read saves 90% of one. A prefix used twice is already cheaper cached.

The 30-minute floor cuts both ways. A chat assistant or support pipeline that fires requests every few minutes keeps the cache hot for free. A batch job that runs once a night gains nothing and pays the 1.25x write premium on every cold start; leave caching off there. Cache the stable prefix (system prompt, tool definitions, few-shot examples) and keep the volatile part (user input, retrieved documents that change per request) after the breakpoint.

## Reasoning effort, pro mode, and ultra

GPT-5.6 exposes six reasoning effort levels: `none`, `low`, `medium`, `high`, `xhigh`, and `max`. Effort is a cost dial, not only a quality dial. Higher settings produce more output-side tokens per request, and output is the expensive direction: $30 per million on Sol, five times the input rate. Two requests with identical prompts can differ several-fold in cost purely on the effort setting.

OpenAI’s own migration guidance says to treat the move as a tuning pass, not only a model-slug change: benchmark your current effort level and one level lower on representative tasks. Many workloads hold quality at the lower setting, and the saving compounds on every request.

Pro mode (`reasoning.mode: "pro"`) is a setting available on all three models, not a separate model with its own rate line. You pay the same per-token prices; the model spends more tokens thinking, so budget for heavier output on quality-first workloads.

Ultra is the deliberate splurge. It runs four agents in parallel by default, multiplying token spend on purpose in exchange for faster wall-clock results and a measurable quality lift: per OpenAI, it raises Sol’s Terminal-Bench 2.1 score from 88.8% to 91.9%. As a first approximation, budget around four times the tokens of a single-agent run and reserve it for work where time-to-answer matters more than cost per answer. The full breakdown of when the parallel spend is worth it lives in our GPT-5.6 ultra mode explainer. Ultra ships in ChatGPT Work on Pro and Enterprise plans, and in Codex from Plus upward.

## What ChatGPT plans get

If your GPT-5.6 usage is conversational rather than programmatic, a subscription may beat the API on cost. Here is how model access maps to plans:

| Plan | GPT-5.6 access | 
|---|---|
| Free / Go | Terra | 
| Plus | Sol, Terra, Luna; per-model effort control (Sol at medium effort and up) | 
| Pro / Business / Enterprise | All three, plus Sol Pro | 
| ChatGPT Work (Pro / Enterprise) | Adds ultra | 

Free and Go users landing on Terra is a strong default; it is the tier OpenAI benchmarks against GPT-5.5. Plus is where per-model effort control appears, which matters if you lean on Sol for hard problems inside ChatGPT. Coding teams weighing Codex seats against raw API spend should note that ultra comes with Codex from Plus upward; the Codex pricing breakdown covers how those seats compare to paying per token.

## Patterns that keep the bill down

The rate card rewards routing discipline more than prompt golf. The patterns that move the number:

- **Route by task, not by habit.** Luna for classification, extraction, and routing; Terra as the default for everything else; Sol only for the problems Terra demonstrably fails. Most teams find the Sol-worthy slice is small.
- **Pin full model IDs.** Ban the bare`gpt-5.6` alias in code review. Every model string in your codebase should say which tier it chose and why.
- **Cache long prefixes.** Any stable prefix over a few thousand tokens that recurs inside 30 minutes should sit behind an explicit breakpoint. The math above says it pays from the second request.
- **Tune effort down one level.** Benchmark current effort against one level lower before you ship. This is OpenAI’s own advice, and it is free money when quality holds.
- **Let short answers stay short.** GPT-5.6 writes notably shorter responses with fewer generic intros than earlier generations. Strip the “be concise” boilerplate from old prompts; redundant instructions add input tokens to fix a problem that no longer exists.
- **Measure before you commit.** Save`gpt-5.6-sol` ,`gpt-5.6-terra` , and`gpt-5.6-luna` as environment variables in Apidog, fire the same request at each tier, and read the token usage fields in the responses side by side. Real counts from your own prompts beat any estimate in this article, including the tables above.

## FAQ

### Is GPT-5.6 cheaper than GPT-5.5?

The comparison point is Terra, which OpenAI positions as competitive with GPT-5.5 at roughly 2x cheaper, at $2.50 input and $15 output per million tokens. Sol costs more than Terra but buys deeper reasoning. Run your own evals before migrating quality-sensitive workloads, but on price alone Terra halves the GPT-5.5 rate card.

### What does the bare gpt-5.6 model ID cost?

The alias `gpt-5.6` routes to Sol, so you pay the top rates: $5 per million input tokens and $30 per million output. That default catches teams who copy quickstart code unchanged. Pin `gpt-5.6-terra` or `gpt-5.6-luna` explicitly when the task does not need flagship reasoning.

### Do reasoning tokens count against output pricing?

Yes. Higher effort settings generate more output-side tokens, billed at the output rate, which is $30 per million on Sol and $6 on Luna. The effort dial is one of the largest cost levers in GPT-5.6, so benchmark your workload at the current level and one lower before locking it in.

### What is the cheapest way to start testing GPT-5.6?

Point your first requests at `gpt-5.6-luna`: a 10K-input, 1K-output request costs about $0.016, so a full afternoon of experimentation runs well under a dollar. Our guide to using the GPT-5.6 API walks through authentication, the Responses API call shape, and tier selection step by step.

## Where this leaves you

Make Terra your default, route volume work down to Luna, and hold Sol for the problems that earn $30-per-million output. Add explicit caching to any prefix that recurs inside 30 minutes, and test one effort level below wherever you run today. Those four moves routinely cut a GPT-5.6 bill by more than half without touching quality.

Before any of that reaches production, get real numbers from your own prompts. Download Apidog, save the three model IDs as environment variables, send the same request through each tier, and compare the token usage fields in the responses. Ten minutes of side-by-side testing tells you more about your bill than any rate card, including this one.
