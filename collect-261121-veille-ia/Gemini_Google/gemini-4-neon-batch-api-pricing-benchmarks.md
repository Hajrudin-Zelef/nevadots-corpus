---
id: collect-261121-veille-ia/gemini-google/gemini-4-neon-batch-api-pricing-benchmarks
title: "Gemini 4 Neon (batch) - API Pricing & Benchmarks"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["gemini", "neon", "xhigh", "artificial", "analysis", "arena", "index", "model"]
source: https://openrouter.ai/google/gemini-4-neon:batch
source_anchor: ""
source_lines: [1, 102]
section: "Gemini_Google"
---
Gemini 4 Neon is a fast, mid-priced model in the Gemini 4 family from Google, built for coding and agentic work. It is suited for repository and terminal coding, web builds, multi-turn tool use and multimodal STEM problems, and is positioned as a step up from Gemini 4 Hydrogen that targets Gemini 4 Carbon quality at higher speed and lower cost. It supports long decode of up to 1M output tokens through continuation, and controls thinking depth with low, medium, high and xhigh reasoning effort.

## Providers

Different companies host the same model. OpenRouter routes your request to one of them based on the routing mode you pick: Balanced (price + speed), Nitro (fastest), or Floor (cheapest).

|  | $1.50 | $7.50 | $0.075 | $0.075 | -- | -- |  | 

  - $1.50
  - $7.50
  - $0.075
  - $0.075
  - --
  - --
  - --

## Pricing

The average price customers actually pay for this model, next to the prices providers post. Caching and discounts mean the price actually paid is often well below the listed one.

### Gemini 4 Neon (batch) — Price history

|  |  | $1.50 | $7.50 | $0.075 | $0.083333 | — | 
|---|---|---|---|---|---|---|

- Input $1.50 / Output $7.50

## Performance

Throughput is how fast the model writes (tokens per second — higher is better). Latency is total round-trip time (lower is better). TTFT is time-to-first-token — how long before you see anything appear (lower is better).

## Uptime

Uptime is the percentage of the past 3 days that at least one provider was responding to requests. Availability is the percentage of time that inference was successfully served. OpenRouter continuously monitors and uses the next-best provider when one returns an error.

Not enough uptime data to display yet.

When an error occurs in an upstream provider, we can recover by routing to another healthy provider, if your request filters allow it. You can access per-provider uptime data programmatically through the Endpoints API. Learn more about our load balancing and provider routing options.

## Benchmarks

Scores on standardized evaluations and this model’s own outputs on shared benchmark prompts. Higher percentages are better — and each rank shows where this model lands among the models evaluated on that source.

| Benchmark score summary for Google: Gemini 4 Neon (batch) (Artificial Analysis and Design Arena) |  |  | 
|---|---|---|
| Source | Benchmark | Score | 
|---|---|---|
| Artificial Analysis | Gemini 4 Neon (Xhigh) Intelligence Index | 61.4 | 
| Artificial Analysis | Gemini 4 Neon (Xhigh) Finance & Accounting Index | 65.1 | 
| Artificial Analysis | Gemini 4 Neon (Xhigh) Strategy & Ops Index | 63.8 | 
| Artificial Analysis | Gemini 4 Neon (Xhigh) Legal Index | 63.8 | 
| Artificial Analysis | Gemini 4 Neon (Xhigh) Healthcare & Medical Index | 60.1 | 
| Artificial Analysis | Gemini 4 Neon (Xhigh) Engineering Index | 66.8 | 
| Artificial Analysis | Gemini 4 Neon (Xhigh) Economics Index | 61.9 | 
| Artificial Analysis | Gemini 4 Neon (Xhigh) AutomationBench-AA | 79.3% | 
| Artificial Analysis | Gemini 4 Neon (Xhigh) GDP.pdf | 28.0% | 
| Artificial Analysis | Gemini 4 Neon (Xhigh) HLE-Diamond | 71.3% | 
| Artificial Analysis | Gemini 4 Neon (Xhigh) Terminal-Bench 4.0 | 63.6% | 
| Artificial Analysis | Gemini 4 Neon (Xhigh) FrontierCode-AA 2.0 | 44.4% | 
| Artificial Analysis | Gemini 4 Neon (Xhigh) Terminal-Bench-Science | 70.0% | 
| Artificial Analysis | Gemini 4 Neon (Xhigh) CritPt v2 (Hard Subset) | 60.5% | 
| Artificial Analysis | Gemini 4 Neon (Xhigh) CritPt v2 | 77.4% | 
| Artificial Analysis | Gemini 4 Neon (Xhigh) AA-Omniscience Accuracy | 60.5% | 
| Artificial Analysis | Gemini 4 Neon (Xhigh) AA-Omniscience Non-Hallucination Rate | 85.6% | 
| Artificial Analysis | Gemini 4 Neon (Xhigh) MLCR-AA | 31.7% | 
| Design Arena | Gemini 4 Neon Models Arena 3D Elo | 1502 | 
| Design Arena | Gemini 4 Neon Models Arena Code Categories Elo | 1393 | 
| Design Arena | Gemini 4 Neon Models Arena Game Development Elo | 1396 | 
| Design Arena | Gemini 4 Neon Models Arena UI Component Elo | 1358 | 
| Design Arena | Gemini 4 Neon Models Arena Website Elo | 1347 | 

## Apps

Public apps that send the most traffic to this model. Good signal for what real production workloads look like — and a hint at which use cases this model is best suited for.

Not enough data for apps using this model yet.

## Activity

Token volume and request traffic to this model over time.

## API

Drop-in code to call this model. OpenRouter's API is OpenAI-compatible — most SDKs work by just swapping the base URL. The only thing that changes between models is the model slug below.

Not currently available through EU in-region routing. In-region routing docs

## Frequently asked questions

Gemini 4 Neon is a fast, mid-priced model in the Gemini 4 family from Google, built for coding and agentic work. It is suited for repository and terminal coding, web builds, multi-turn tool use and multimodal STEM problems, and is positioned as a step up from Gemini 4 Hydrogen that targets Gemini 4 Carbon quality at higher speed and lower cost.

Gemini 4 Neon (batch) costs $1.50/M input tokens and $7.50/M output tokens, with separate rates for Cache Read at $0.075/M tokens, Cache Write at $0.08333/M tokens, Image Input at $1.50/M tokens, Input Audio at $1.50/M tokens, Input Audio Cache at $0.075/M tokens, and Web Search at $14.00/1K calls.

Gemini 4 Neon (batch) has a 1,048,576 token context window. It supports up to 1,048,576 completion tokens.

Yes. Gemini 4 Neon (batch) accepts `tools` and `tool_choice` for function calling. It also supports structured outputs via a JSON schema in `response_format`.

Gemini 4 Neon (batch) accepts audio, files such as PDFs, images, text, and video as input and returns text.

Gemini 4 Neon (batch) is served by 1 provider on OpenRouter: Google Vertex.

Gemini 4 Neon (batch) was released on November 18, 2026.
