---
id: collect-261121-veille-ia/grok-xai/grok-4-7-free-on-felo-search-benchmarks-pricing-specs
title: "Grok 4.7 Free on Felo Search: Benchmarks, Pricing & Specs"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: ["2026-09-21"]
keywords: ["grok", "spacexai", "model", "tokens", "same", "felo", "search", "pricing"]
source: https://felo.ai/tools/grok-4-7
source_anchor: ""
source_lines: [1, 362]
section: "Grok_xAI"
---
# Grok 4.7on Felo Search

Grok 4.7 is SpaceXAI's newest flagship for coding, agentic tasks and knowledge work. It uses a new, larger base model, was trained with a longer reinforcement-learning run, and checks its own work more carefully than Grok 4.6.

Opens Felo Search with search_model=grok-4.7 selected. Model pricing shown on this page is SpaceXAI API pricing, not a Felo plan price.

Model card

## Grok 4.7 at a glance

- Model name
- grok-4.7
- Context window
- 500,000 tokens
- Knowledge cutoff
- May 2026
- Input / output price
- $2 / $6 per 1M tokens
- Reasoning levels
- low / medium / high / xhigh

Sep 21

2026 release

SpaceXAI announced Grok 4.7 on September 21, 2026, alongside API availability.

$2 / $6

API price per 1M tokens

Same input and output rate as Grok 4.6, despite a larger base model.

500k

Context window

Enough for long documents and multi-turn agent sessions.

3.3%

Risky prompt pass-through

SpaceXAI's HackerBench v0.3 figure for its new safeguard stack.

## What is actually new in Grok 4.7

SpaceXAI lists four changes over Grok 4.6. They are the reason this is a new model and not a renamed Grok 4.5.

### A new, larger base model

Grok 4.7 does not reuse the Grok 4.6 base. SpaceXAI trained a new and larger foundation model for this release.

### A longer RL run

Training used a longer reinforcement-learning run on a harder task mix, weighted toward problems that take many hours to finish.

### Better self-verification

The model is trained to check its own work more carefully and to manage longer context across a session.

### Native Grok Bot harness

Grok 4.7 was trained to understand the Grok Bot harness natively, which SpaceXAI says improves conversational and knowledge work.

## Where Grok 4.7 stands out

These are the four claims SpaceXAI leads with. Each one is labelled with the source so you can separate the vendor's framing from independent testing.

### Built for long coding tasks

SpaceXAI positions Grok 4.7 for coding and agentic work, and reports a jump on CursorBench 4.0, which stresses longer-running coding tasks.

Source: SpaceXAI launch post

### Same price as Grok 4.6

Grok 4.7 keeps the $2 per 1M input and $6 per 1M output rate of Grok 4.6, even though it uses a larger base model and a longer training run.

Source: SpaceXAI model card and pricing

### A new safeguard stack

Grok 4.7 ships with an entirely new safety stack. SpaceXAI says it is the strongest model it has tested on refusals and jailbreak resistance.

Source: SpaceXAI launch post

### Price-performance framing

SpaceXAI frames Grok 4.7 as frontier price-performance: it costs far less per million tokens than Fable 5.1 Max or GPT-5.6 Sol Max.

Source: SpaceXAI launch post

## SpaceXAI's published scores, with the gaps left in

Grok 4.7 improves on Grok 4.6 on every score below, but it does not lead every column. Fable 5.1 Max tops four of the seven, and GPT-5.6 Sol Max still holds the best DeepSWE v1.1 result.

Benchmark

Grok 4.7 (xhigh)

Grok 4.6 (high)

GPT-5.6 Sol Max

Fable 5.1 Max

CursorBench 4.0

46.3%

40.4%

41.7%

51.8%

DeepSWE v1.1

71.0%*

65.2%

72.7%

70.0%

EEBench

64.0%

53.0%

39.4%

56.4%

AA Briefcase v1.1

1,657

1,546

1,487

1,678

Terminal-Bench 4.0

37.6%

20.3%

37.3%

57.9%

Harvey Legal Agent

19.6%

15.8%

2.5%

6.7%

HealthBench Professional

56.7%

48.5%

60.5%

62.1%

All seven scores are SpaceXAI's own published figures. An asterisk marks a high-effort Grok 4.7 result rather than the xhigh setting. Self-reported benchmarks are a starting point, not a verdict.

## Same price per token is not the same cost per task

Grok 4.7 costs the same per token as Grok 4.6, but independent testing found it can generate far more tokens to finish the same job.

One independent measurement reported Grok 4.7 producing roughly 2.5x the output tokens of Grok 4.6. If a task takes more tokens, the same per-token rate can still mean a higher bill. Evaluate cost per useful answer, not just the headline rate.

### What to check before you standardise on it

Run the same prompt at the same reasoning level on Grok 4.7 and Grok 4.6, then compare both answer quality and total tokens used. A lower rate only wins if the token count stays close.

Independent measurement, September 2026

## Official SpaceXAI API pricing

These are the rates SpaceXAI publishes for grok-4.7. They are model API prices, not a Felo subscription price, and the provider can change them.

Tier

Input / 1M

Cached input / 1M

Output / 1M

Standard (under 200k context)

The headline rate, unchanged from Grok 4.6.

$2.00

$0.50

$6.00

Long context (200k and above)

Long-context rates bill all tokens in the request once the prompt reaches 200k.

$4.00

$1.00

$12.00

Grok 4.7 Fast

Twice the output speed at twice the price. Cursor and Grok Build only; not on the public xAI API.

$4.00

—

$12.00

US regional endpoint

Keeps inference inside the United States at a 10% token premium.

+10%

+10%

+10%

Felo Search access and SpaceXAI API pricing are separate. The table above is SpaceXAI's published model pricing; it is not a Felo billing table.

## A new safety stack, in SpaceXAI's own words

SpaceXAI says Grok 4.7 is its strongest model yet on refusals and jailbreak resistance. These are vendor-reported figures.

### Best-calibrated safeguards to date

SpaceXAI describes Grok 4.7 as built with an entirely new safeguard stack and as the strongest model it has tested on refusals and jailbreak resistance.

### Dual-use domains

In cybersecurity and biological work, SpaceXAI says Grok 4.7 leads on both utility for benign tasks and safe refusal on dangerous ones.

### HackerBench v0.3

On SpaceXAI's own risky-cyber benchmark, Grok 4.7 allowed only 3.3% of risky dual-use prompts through while rarely blocking legitimate security work.

## Official grok-4.7 specification

Pulled from the SpaceXAI model card. Where the provider has published no figure, this page does not invent one.

### Context window

500,000 tokens. SpaceXAI recommends context compaction for long agent loops.

### Knowledge cutoff

May 2026. Anything after that date needs live retrieval, which is where Felo Search adds web context.

### Modalities

Text and image input, text output. SpaceXAI lists no text output limit.

### Reasoning levels

low, medium, high (default) or xhigh. Higher settings spend more tokens for harder problems.

### APIs

Supported on both the Responses API and Chat Completions. Encrypted reasoning is always returned on Responses.

### Tools

Function calling, web search, X search and code execution are all available to the model.

## Where Grok 4.7 runs

SpaceXAI lists these surfaces at launch. Grok 4.7 is not yet announced for grok.com, the mobile apps or SuperGrok.

### xAI API

Available today with the model name grok-4.7. Create a key in the SpaceXAI console.

### Cursor

Available on all Cursor plans. Grok 4.7 Fast is also served here.

### Grok Build

Grok 4.7 is the default model of the Grok Build coding agent. Fast is not included in the free tier.

### Model gateways

Available through OpenRouter, Vercel and Cloudflare model routers.

### US regional endpoint

Keeps inference inside the United States at a 10% token premium.

### Felo Search

Open Felo Search with search_model=grok-4.7 to run the model with live web context and citations.

## What to test Grok 4.7 on

Grok 4.7 is aimed at long, multi-step work. These are the tasks where its new base model and longer training run should show up first.

### Long coding sessions

Multi-file refactors, migration planning and debugging where the model has to hold a lot of context across many steps.

### Agentic task decomposition

Turn an open-ended brief into a plan, a source checklist and a sequence of verifiable actions.

### Document and deck drafting

SpaceXAI says Grok 4.7 improved at creating documents and presentations, so test it on real deliverables.

### Security and dual-use review

Use it for defensive security work where the new safeguard stack is meant to allow legitimate use.

### Cost-per-answer evaluation

Compare Grok 4.7 against Grok 4.6 and other models on both answer quality and total tokens used.

### Knowledge-work benchmarks

Legal, clinical and financial analysis tasks, where SpaceXAI reports gains on Harvey and HealthBench.

## Comparing Grok generations?

Grok 4.5 was a July 2026 launch with a Felo Pro free-trial campaign. Grok 4.7 is a September 2026 release with a new base model and no Felo trial campaign.

See the Grok 4.5 page
## Frequently Asked Questions

Grok 4.7 is SpaceXAI's flagship model for coding, agentic tasks and knowledge work, released on September 21, 2026. SpaceXAI describes it as its most capable model for those tasks, built on a new and larger base model than Grok 4.6.

## Try Grok 4.7 on Felo Search

Run SpaceXAI's newest flagship with live web context and citations, and compare it against Grok 4.6 and other frontier models on your own tasks.

Open Grok 4.7 in Felo Search
Opens Felo Search with Grok 4.7 selected where available

Model specifications, pricing, benchmark scores and safety figures on this page are SpaceXAI's own published numbers from the Grok 4.7 launch post, model card and pricing page. The token-use caveat is an independent measurement from September 2026. Self-reported benchmarks are labelled as vendor claims throughout.
