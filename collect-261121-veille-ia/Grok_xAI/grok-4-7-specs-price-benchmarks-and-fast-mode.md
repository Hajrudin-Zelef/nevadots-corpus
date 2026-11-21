---
id: collect-261121-veille-ia/grok-xai/grok-4-7-specs-price-benchmarks-and-fast-mode
title: "Grok 4.7: Specs, Price, Benchmarks and Fast Mode"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: []
keywords: ["grok", "spacexsi", "effort", "tokens", "artificial", "analysis", "price", "specs"]
source: https://grokmag.com/grok-models/grok-4-7/
source_anchor: ""
source_lines: [1, 95]
section: "Grok_xAI"
---
# Grok 4.7: what the flagship can do, what it costs and how it really scores

Specs, effort levels and API prices for Grok 4.7, with SpaceXSI benchmark claims set against independent results.

**Checked**October 18, 2026

**By**GrokMag editorsIndependent · not affiliated with SpaceXSI, xAI or X

Grok 4.7 was released on **September 21, 2026**. It has a **500k-token context window**, four effort levels and costs **$2 per million input tokens and $6 per million output tokens**. Artificial Analysis scores it **46** on its Intelligence Index, and it ranks **#89 on LMArena** text as of October 15, 2026.

## Release and specs

SpaceXSI (formerly xAI) released Grok 4.7 on September 21, 2026. It replaced Grok 4.6, which came out on August 12, as the company’s flagship. SpaceXSI says Grok 4.7 is built on a larger new base model, with longer reinforcement learning focused on tasks that run for several hours.

- **Context window:** 500k tokens on the API.
- **Input:** text and images.
- **Knowledge cutoff:** May 2026.
- **Output cap:** none for text, according to the docs.
- **Size:** Musk has described it as about 2.1T parameters. SpaceXSI has not published an official figure.

## Effort levels and API settings

Grok 4.7 has four reasoning effort levels, set with the `reasoning_effort` parameter: **low**, **medium**, **high** (the default) and **xhigh**. Higher effort spends more tokens thinking before it answers. Most published scores, including SpaceXSI’s own, use xhigh.

Two API details matter for cost. Calls through the Responses API always return encrypted reasoning. SpaceXSI also strongly recommends setting `prompt_cache_key` or the `x-grok-conv-id` header. Without it, a request can land on a server with a cold cache and pay full input price instead of the cached rate.

## Price and the Fast variant

On the API, Grok 4.7 costs the same as Grok 4.6. Long-context rates apply once a prompt passes 200k tokens and then cover the whole request.

| Option | Input | Cached input | Output | 
|---|---|---|---|
| Standard (under 200k) | $2.00 | $0.50 | $6.00 | 
| Long context (200k+) | $4.00 | $1.00 | $12.00 | 
| US regional endpoint | $2.20 | $0.55 | $6.60 | 

Prices are USD per million tokens. The US endpoint keeps inference in the US at 1.1x global rates.

**Grok 4.7 Fast** is the same model on faster infrastructure. SpaceXSI describes it as about twice the output speed for twice the price, or 1.5x for long-context requests. It is only available in Cursor and Grok Build. It is not on the public API and not in the Grok Build free tier.

## What SpaceXSI claims

SpaceXSI published these scores for Grok 4.7 at xhigh effort. They are vendor claims, not independent tests.

| Benchmark | Grok 4.7 score | 
|---|---|
| CursorBench 4.0 | 46.3% | 
| DeepSWE v1.1 | 71.0% | 
| Terminal-Bench 4.0 | 37.6% | 
| EEBench | 64.0% | 
| HealthBench Professional | 56.7% | 
| Harvey Legal Agent | 19.6% | 
| GDPval Elo | 1695 | 
| LatchBio biosafety | 62.4% | 

SpaceXSI calls the LatchBio result the top score. Its own charts are candid about the rest: Claude Fable 5.1 Max leads on CursorBench, Terminal-Bench and GDPval, and GPT-5.6 Sol Max leads on DeepSWE.

## What independent tests found

Outside testers paint a more modest picture.

- **Artificial Analysis Intelligence Index:** 46, two points above Grok 4.6. Artificial Analysis says this puts SpaceXSI among the top four labs. The model itself ranks around 25th to 30th of 217 models, depending on how effort variants are counted (as of October 18, 2026).
- **Coding Agent Index:** 56 when run inside Grok Build, fourth behind Claude Fable 5.1, GPT-6 Astra and Claude Opus 5, according to Artificial Analysis.
- **Hallucination rate:** 29%, down from 34% for Grok 4.6, per Artificial Analysis. A hallucination is a confident answer that is false.
- **LMArena text leaderboard:** #89 with a score of 1444, as of October 15, 2026. That is well outside the top 30.
- **Token use:** Artificial Analysis found it token-hungry, at about 81k output tokens per task versus 38k for Grok 4.6. Its model page also notes a very slow time to first token.

**Comparing index numbers:**SpaceXSI’s Grok 4.6 materials quoted an Artificial Analysis score of 61 for 4.6. Artificial Analysis appears to have re-based its index since, so older and newer scores are not directly comparable.

For how Grok stacks up against OpenAI’s models in everyday use, see Grok vs ChatGPT.

## Where you can use Grok 4.7

- **API:** as`grok-4.7` , on the global endpoint and the US regional endpoint.
- **Grok Build:** Grok 4.7 now powers the terminal coding agent.
- **Cursor:** standard and Fast versions, drawing on the shared weekly pool.
- **Grok apps:** the X Subscription Premium plan ($30) lists “latest models” among its features. SpaceXSI does not publish a model-by-plan table for the apps.

Grok 4.8 has not shipped as of November 19, 2026. On November 17, Musk said a limited test with some Grok Bot users has started and that general release is still a week or two away (our story). See all Grok models for the full history.

## Questions people ask

## How much does Grok 4.7 cost?

On the API, $2 per million input tokens, $0.50 cached and $6 per million output tokens. Prompts over 200k tokens cost double. Grok 4.7 Fast costs twice the standard rate.

## What is the context window of Grok 4.7?

## Is Grok 4.7 better than Grok 4.6?

Slightly, by independent measures. Artificial Analysis scores it 46, two points above 4.6, with a lower hallucination rate (29% vs 34%). It also uses about twice as many output tokens per task.

## Can I use Grok 4.7 Fast on the API?

No. Grok 4.7 Fast is only offered in Cursor and Grok Build, and not in the Grok Build free tier.
