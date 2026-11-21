---
id: collect-261121-veille-ia/gpt-openai/what-is-gpt-6-luna-model-id-0-10-0-50-pricing-and-a-1m-token-context-window
title: "What Is GPT-6 Luna? Model ID, $0.10/$0.50 Pricing, and a 1M Token Context Window"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["luna", "gpt-6", "model", "window", "gpt-5", "million", "tokens", "token"]
source: https://apidog.com/blog/what-is-gpt-6-luna/
source_anchor: ""
source_lines: [1, 104]
section: "GPT_OpenAI"
---
The cheapest tier in a model family used to be the tier you did not trust. You sent classification, summarization and retry traffic there, kept anything agentic on the flagship, and accepted that the budget model would fall over the moment a task needed more than one step.

GPT-6 Luna, announced on September 22, 2026, is priced like that tier and does not behave like it. It costs $0.10 per million input tokens and $0.50 per million output tokens, it carries a 1 million token context window, and OpenAI’s published numbers put it inside the range of frontier models on agentic coding benchmarks at a small fraction of their cost per task.

One thing to settle first: this is not the GPT-5.6 Luna you may already have wired into a fallback chain. Same tier name, new model, new price, new context window. Assuming otherwise is an easy way to ship a silent behavior change and only find out from a support ticket.

## GPT-6 Luna at a glance

| Item | Value | 
|---|---|
| API model ID | `gpt-6-luna` | 
| Input price | $0.10 per million tokens | 
| Output price | $0.50 per million tokens | 
| Cached input reads | 90% discount | 
| Context window | 1,000,000 tokens | 
| Artificial Analysis Intelligence Index | 37 | 
| Announced | September 22, 2026, alongside GPT-6 Sol | 
| Available in | ChatGPT Work and Codex, the desktop app for Free and Go users, plus the API | 
| Not available in | Chat, as of launch | 

Inside the same family, GPT-6 Sol is $2/$10 with an 872,000 token window and an index of 48, and GPT-6 Astra is $10/$50. OpenAI states that Astra “continues to be our best model across the board,” so Luna is not being sold as the smartest model. It is being sold as the one where the cost per task collapsed.

## It is a new model, not a renamed tier

The GPT-5.6 family was Sol, Terra and Luna. The GPT-6 family is Astra, Sol and Luna. Two names carried over, Terra did not, and Astra is new. OpenAI describes Sol and Luna as trained with similar methods to GPT-6 Astra, which is to say they are new models sharing a naming convention with the old ones.

There is no GPT-6 Terra. If you built a routing ladder around the GPT-5.6 naming scheme, the middle rung no longer exists, and our earlier comparison of Sol against Terra and Luna now reads as history rather than current guidance.

The price lineage shows how far the tier moved:

| Tier | GPT-5.6 list | GPT-5.6 promotional | GPT-6 | 
|---|---|---|---|
| Sol | $5 / $30 | $4 / $20 | $2 / $10 | 
| Terra | $2.50 / $15 | $2 / $12 | no GPT-6 Terra | 
| Luna | $1 / $6 | $0.20 / $1.20 | **$0.10 / $0.50** | 

All figures are per million input tokens and per million output tokens. The GPT-5.6 rows come from our coverage at the time, GPT-5.6 pricing and the GPT-5.6 price cut.

OpenAI describes Sol and Luna as 50% cheaper than GPT-5.6 **promotional** pricing. The word promotional is OpenAI’s own and it changes the meaning of the claim: the comparison runs against a discounted rate, not the rate GPT-5.6 launched at. Measured against the GPT-5.6 Luna list price of $1/$6, GPT-6 Luna at $0.10/$0.50 is a 90% cut on input and a 92% cut on output. The real reduction is larger than the headline, and the headline is measured from a discount. Put the second number in your budget model, not the first.

## The cheap model has the bigger context window

This is the detail most launch coverage skipped. Luna ships with a 1,000,000 token context window. Sol, at twenty times the price, ships with 872,000.

That inverts an assumption a lot of routing code encodes: that you promote a request to a more expensive model when the payload grows. With GPT-6, the largest window in the family sits at the bottom of the price list. A 900,000 token document does not fit in Sol and does fit in Luna.

The arithmetic follows from the two prices. Filling Luna’s full window once costs $0.10 in input tokens. Filling Sol’s 872,000 token window once costs roughly $1.74. Filling Claude Opus 5.5’s 1M window at $4 per million costs $4.00. Same class of payload, a fortyfold spread in what you pay to read it.

Layer OpenAI’s caching work on top and the gap widens again. GPT-6 applies a 90% discount to cached input reads, so a 1M token prefix that stays cached across calls costs about $0.01 to re-read instead of $0.10. OpenAI also says GPT-6 gets higher cache hit rates by default, that changing reasoning effort or tool availability no longer invalidates the cache, and that explicit breakpoints let you control where a cached prefix ends. GitHub reports more than 50% fewer prompt tokens needing fresh processing across billions of requests.

## What OpenAI published

Every number below is from OpenAI’s launch material. Reasoning effort settings appear in parentheses, because these models score very differently at different effort levels and a benchmark row without its effort setting is not a usable number.

| Benchmark | GPT-6 Luna | Comparison point | 
|---|---|---|
| DeepSWE 1.1 | 66.6% (max) | Comparable to Claude Opus 5 and Claude Fable 5 at medium, 93% cheaper per task than Opus 5 and 96% cheaper than Fable 5 | 
| AutomationBench 1.0.6 | Beats its predecessor by 5.4 points (high) | At 58% lower cost per task | 
| OSWorld 2.0 offline | Beats GPT-5.6 Sol (medium) at max effort | At roughly a tenth the cost | 
| Factuality | Matches GPT-5.6 Sol at higher effort | At roughly a hundredth the cost | 

The DeepSWE row is the one to sit with. A model at $0.10 per million input tokens scoring 66.6% on an agentic software engineering benchmark, in the same territory as two frontier models at medium effort, for 93% less per task than one of them, is a different proposition from a budget tier. It does not mean Luna replaces a frontier model on your hardest work. It does mean the boundary between “route this to the cheap model” and “this needs the expensive one” has moved, and wherever you drew that line before September 2026 is now drawn in the wrong place.

The factuality result is the one most likely to matter for ordinary product work. Matching the previous generation’s mid tier on factual accuracy, at about a hundredth of its cost, is what makes Luna viable for user-facing summarization and extraction rather than just internal batch jobs.

## Every comparison here is against Claude Opus 5

Note which Claude model those rows benchmark against. Claude Opus 5.

Anthropic shipped Claude Opus 5.5 on the same day, at $4/$20 against Opus 5’s $5/$25, and it took the top slot on the Artificial Analysis Intelligence Index at 58, ranked first of 212 models. OpenAI’s launch post was written before Opus 5.5 existed, so every cost-per-task comparison against Opus 5 is measured against a model that was superseded within hours of publication.

That does not make OpenAI’s figures wrong. They are accurate for the comparison that was run. It does mean you should not carry “93% cheaper than Opus 5” into a procurement document as the current state of the market without saying which Opus it refers to. No vendor has published a Luna against Opus 5.5 head to head, and the comparisons circulating on social media come from individual testers rather than either lab. Our price war pillar tracks all three launches against each other in one table.

## Latency: the cheap model is not the fast model

Third party measurements from Artificial Analysis put GPT-6 Luna at 153.9 output tokens per second with a time to first token of 124.23 seconds. Two caveats apply and both are load bearing. These are third party figures, not vendor published. And they are measured on the max reasoning variant, the slowest and most expensive configuration available.

Even with both caveats, the direction is worth absorbing: Luna measures slower to first token than Sol, which measures at 102.15 seconds. Price and latency are not correlated in this family. A two minute wait before the first token arrives will break a default HTTP client timeout, idle out a load balancer, and exceed the execution ceiling on most serverless runtimes. If you are routing a synchronous endpoint to Luna at high effort, the integration work lives in your timeout and streaming layer, not in your prompt.

## Test it against your own workload before you route to it

Benchmark tables are aggregate. Your prompt is not. The useful question is whether Luna holds up on your traffic at an effort level you can afford, and that takes about ten minutes to answer properly.

Set up one request per model, parameterize the model ID, and run the same payload across the tier:

```
curl https://api.openai.com/v1/responses \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "gpt-6-luna",
    "input": "Extract every endpoint, method and required parameter from the OpenAPI document below as JSON.",
    "reasoning": { "effort": "high" }
  }'
```
Confirm the endpoint and parameter names against OpenAI’s current model reference before you build on this shape. The model ID `gpt-6-luna` is the part that is confirmed from the launch material.

In Apidog, the same request becomes a saved endpoint with the model ID as an environment variable, so one collection covers Luna, Sol and Astra without duplicating anything. Run it as a test scenario and you get response time and token usage per run, plus assertions on the response shape, which is what catches the real failure mode of a cheaper model: not a wrong answer, but a response that stops conforming to the JSON schema your parser expects. Run the same scenario at three effort levels and you have a cost, latency and reliability table for your own prompts rather than for someone else’s benchmark suite.

## When Luna is the right call

Route to Luna when the payload is large, the task is well specified, and you can verify the output programmatically. Document extraction, spec parsing, log triage, test generation, high volume classification and any batch job where a 1M window saves you a chunking pipeline all qualify, and the caching discount compounds when a long prefix stays stable across calls.

Keep the expensive tier for work where you cannot check the answer cheaply, and for anything latency sensitive until you have measured first token time on your own traffic. Luna moved the line. It did not erase it.
