---
id: collect-261121-veille-ia/gpt-openai/what-is-gpt-6-sol-model-id-pricing-1-05m-context-and-benchmarks
title: "What Is GPT-6 Sol? Model ID, Pricing, 1.05M Context, and Benchmarks"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: ["2026-09-30"]
keywords: ["gpt-6", "model", "gpt-5", "price", "tokens", "context", "luna", "openai"]
source: https://apidog.com/blog/what-is-gpt-6-sol/
source_anchor: ""
source_lines: [1, 135]
section: "GPT_OpenAI"
---
If you saw `gpt-6-sol` land in a changelog on September 22, 2026 and assumed it was the GPT-5.6 Sol you already integrated, stop there. It is not the same model. OpenAI reused the tier name and shipped a new model underneath it, trained with similar methods to GPT-6 Astra, at a different price, with a different context window, and with a different benchmark profile.

That naming decision is going to cost teams money and debugging hours. This piece covers what GPT-6 Sol actually is: the model ID, what it costs, the numbers OpenAI published, the one measurement that is already out of date, and where you can call it today.

**Correction (September 30, 2026):** an earlier version of this article listed GPT-6 Sol's context window as 872,000 tokens. OpenAI's GPT-6 Sol model page lists a 1,050,000-token context window, with up to 922,000 input tokens and 128,000 output tokens. OpenAI has since released GPT-6.1 Sol at the same $2/$10 price.

## GPT-6 Sol at a glance

| Item | Value | 
|---|---|
| API model ID | `gpt-6-sol` | 
| Input price | $2 per million tokens | 
| Output price | $10 per million tokens | 
| Cached input reads | 90% discount | 
| Context window | 1,050,000 tokens (up to 922,000 input, 128,000 output) | 
| Artificial Analysis Intelligence Index | 48 | 
| Announced | September 22, 2026, alongside GPT-6 Luna | 
| Available in | ChatGPT Work and Codex, plus the API | 
| Not available in | Chat, as of launch | 

For comparison inside the same family: GPT-6 Astra is $10/$50, and GPT-6 Luna is $0.10/$0.50 with a 1M context window and an index of 37. OpenAI’s own line is that Astra “continues to be our best model across the board,” so Sol is not the flagship. It is the tier where cost per task moved.

## It is a new model, not a renamed tier

The GPT-5.6 family was Sol, Terra, and Luna. The GPT-6 family is Astra, Sol, and Luna. Two names carried over, one did not, and one is new.

There is no GPT-6 Terra. If you built a tier ladder around the GPT-5.6 naming scheme, the middle rung is gone and the rungs that remain sit at different heights. Our older comparison of Sol against Terra and Luna describes the previous generation and should be read as history.

The price lineage makes the break obvious:

| Tier | GPT-5.6 list price | GPT-5.6 promotional price | GPT-6 price | 
|---|---|---|---|
| Sol | $5 / $30 | $4 / $20 | **$2 / $10** | 
| Terra | $2.50 / $15 | $2 / $12 | no GPT-6 Terra | 
| Luna | $1 / $6 | $0.20 / $1.20 | **$0.10 / $0.50** | 

Prices are per million input tokens and per million output tokens. The GPT-5.6 list and promotional rows come from our own coverage at the time: GPT-5.6 pricing and the GPT-5.6 price cut.

## What “50% cheaper” actually means

OpenAI describes Sol and Luna as 50% cheaper than GPT-5.6 **promotional** pricing. The word promotional is OpenAI’s, and it matters. The comparison is against a discounted rate, not the list rate GPT-5.6 launched with.

Measured against the GPT-5.6 Sol list price of $5/$30, GPT-6 Sol at $2/$10 is a 60% cut on input and a 67% cut on output. The real reduction is larger than the headline, and the headline is measured from a discount. Both statements are true at once, and you want the second one in your budget model rather than the first.

One thing that has not changed is that a cheaper per-token rate does not automatically produce a cheaper bill. Reasoning models spend output tokens on thinking. A model that costs half as much per token and thinks three times as long is not a saving. Which is why OpenAI led its launch numbers with cost per task instead of cost per token.

## The published benchmarks

Every figure below is from OpenAI’s launch material. The reasoning effort setting is in parentheses because these models score very differently at different effort levels, and a benchmark row without its effort setting is not a usable number.

| Benchmark | GPT-6 Sol | Comparison point | 
|---|---|---|
| AutomationBench 1.0.6 | 33.2% (xhigh) at $0.27 per task | GPT-6 Astra (low) 30.3% at 3.9x Sol’s cost; Claude Opus 5 (max) 26.9% at 11.1x Sol | 
| Agents’ Last Exam V1 | 56.4% (max) | Above Claude Opus 5’s best score, at 60% lower cost per task | 
| DeepSWE 1.1 | 68.8% (max) | Within 1.1 points of Claude Fable 5 at xhigh (69.9%), roughly 80% cheaper per task | 
| OSWorld 2.0 offline | 60.5% (xhigh) | Claude Opus 5 (medium) 60.3%, roughly 80% cheaper per task | 

The pattern is consistent across all four: Sol lands at or slightly above the frontier models it is compared against, at a fraction of the cost per task. On AutomationBench, Sol at xhigh beats Claude Opus 5 at max for 9% of Opus 5’s cost per task. A Claude Fable 5.1 setup with an Opus 5 fallback scores 31.4%, below Sol, at more than 8.9x the cost.

OpenAI also states that Sol makes about half as many factuality mistakes as its predecessor.

## The comparison that expired on launch day

Read that table again and note which Claude model it benchmarks against. Claude Opus 5.

Anthropic shipped Claude Opus 5.5 on the same day, at $4/$20 against Opus 5’s $5/$25, and it took the number one slot of the 212 models ranked on the Artificial Analysis Intelligence Index, with a score of 58. OpenAI’s launch post was written before that model existed, so every “beats Opus 5 at 9% of the cost” line is measured against a model that was superseded within hours.

That does not make OpenAI’s numbers wrong. They are accurate for the comparison they ran. It does mean you should not carry them into a 2027 procurement deck as the current state of play. If you need a Sol against Opus 5.5 answer, nobody has published one yet, and the head-to-head figures circulating on social media are from individual testers rather than either vendor.

Our price war pillar tracks all three launches in one table.

## The cheap model is not the fast model

Third party measurements from Artificial Analysis put GPT-6 Sol at 115.2 output tokens per second with a **time to first token of 102.15 seconds** in its max reasoning configuration. GPT-6 Luna, the cheaper model, measures slower to first token at 124.23 seconds.

Two caveats before you plan around those numbers. They are third party, not vendor published. And they are for the max reasoning variants specifically, which is the most expensive setting to run and the slowest to respond. Lower effort levels will behave differently, and OpenAI has not published latency figures of its own.

Even with both caveats, the shape of the result is the thing to absorb. A 102 second wait before the first token arrives is not a gap your existing timeout configuration is likely to survive. Default HTTP client timeouts sit at 30 or 60 seconds. Load balancers idle out. Serverless functions hit their execution ceiling. If you are moving a synchronous endpoint from GPT-5.6 Sol to GPT-6 Sol at high effort, the integration work is in your timeout and streaming layer, not in the prompt.

This is worth measuring yourself rather than trusting either us or Artificial Analysis. In Apidog, you can point a request at the OpenAI endpoint, set the model ID as an environment variable so one collection covers every tier, and read the response time directly off each run. Build the same request three times at three effort levels, save them as a test scenario, and you have your own latency table for your own prompts in about ten minutes. That number is the one your timeout config should be built from.

## Prompt caching got a real release

Alongside the models, OpenAI shipped caching changes that matter more than the headline price for anyone running agents:

- **90% discount on cached input reads.** At $2 input, a cached read is $0.20 per million tokens.
- **Higher cache hit rates by default** , with no code change required.
- **Changing reasoning effort or tool availability no longer breaks the cache.** Previously, bumping effort mid-conversation invalidated the cached prefix.
- **Explicit breakpoints** , so you choose where a cached prefix ends instead of guessing.
- **A Prompt Caching Dashboard and a diagnostics tool** , so hit rate stops being invisible.

GitHub reports more than 50% fewer prompt tokens needing fresh processing across billions of requests. With up to 922,000 input tokens per request, the difference between a 20% and an 80% hit rate is the difference between a viable agent product and one you quietly shut down.

The effort and tool-availability change is the sleeper. Agent loops routinely escalate effort when a task gets hard and swap tool sets between planning and execution steps. Under the old behavior, both moves threw away the cache exactly when the context was longest.

## Where you can call it today

| Surface | GPT-6 Sol | 
|---|---|
| API | Yes, model ID `gpt-6-sol` | 
| ChatGPT Work | Yes, for Plus, Pro, Business, Enterprise, and Edu | 
| Codex | Yes, for Plus, Pro, Business, Enterprise, and Edu | 
| Chat | Not yet | 
| Free and Go tiers | GPT-6 Luna in the desktop app | 

The Chat gap is the one that trips people up. If you usually sanity check a prompt in ChatGPT’s consumer surface before wiring it into code, you will not find GPT-6 Sol there. Use Codex or go straight to the API.

A minimal call looks like this:

```
curl https://api.openai.com/v1/responses \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "gpt-6-sol",
    "input": "Summarize the breaking changes in this OpenAPI diff.",
    "reasoning": { "effort": "high" }
  }'
```
Confirm the request shape against OpenAI’s current model reference before you ship it. The model ID is the part we have verified.

## How much this is worth to you

One number from OpenAI’s launch gives a sense of who this pricing is aimed at: the median OpenAI researcher spends over $600 a day on coding agents, and the 90th percentile spends $7,000 a day. At that scale, an 80% cut in cost per task is a budget line item, not a rounding error.

For most teams the calculation is smaller and more specific:

| Coming from | The case for GPT-6 Sol | 
|---|---|
| GPT-5.6 Sol | 60% off input and 67% off output against list, and better factuality. Test first, the outputs will differ. | 
| GPT-6 Astra | $10/$50 down to $2/$10, and Sol at xhigh beat Astra at low on AutomationBench. But OpenAI still calls Astra its best model across the board, so hold Astra for the hardest work. | 
| Claude Opus 5 | OpenAI’s numbers favor Sol heavily. Those numbers predate Opus 5.5 at $4/$20, so rerun the comparison yourself. | 
| GPT-6 Luna | Luna is 20x cheaper at $0.10/$0.50 with a larger 1M window. If your workload is high volume and not frontier-hard, start there. | 

Whichever way you go, the switch is a behavior change, not a config change. Same prompt, new model, different outputs. Run your existing API test suite against the new model ID before you route production traffic to it, and assert on response schema and latency, not just on a 200 status code. That is exactly the gap between a model that benchmarks well and a model that works in your stack.
