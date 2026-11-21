---
id: collect-261121-veille-ia/gemini-google/gemini-4-neon-api-pricing-benchmarks
title: "Gemini 4 Neon - API Pricing & Benchmarks"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-11-18"]
keywords: ["effective", "gemini", "used", "routing", "priority", "neon", "endpoints", "provider"]
source: https://openrouter.ai/google/gemini-4-neon
source_anchor: ""
source_lines: [1, 185]
section: "Gemini_Google"
---
Gemini 4 Neon is a fast, mid-priced model in the Gemini 4 family from Google, built for coding and agentic work. It is suited for repository and terminal coding, web builds, multi-turn tool use and multimodal STEM problems, and is positioned as a step up from Gemini 4 Hydrogen that targets Gemini 4 Carbon quality at higher speed and lower cost. It supports long decode of up to 1M output tokens through continuation, and controls thinking depth with low, medium, high and xhigh reasoning effort.

## Providers

Different companies host the same model. OpenRouter routes your request to one of them based on the routing mode you pick: Balanced (price + speed), Nitro (fastest), or Floor (cheapest).

|  | $3.00 | $15.00 | $0.150 | $0.150 | 3.22s | 83 tok/s |  | 
|  | $3.00 | $15.00 | $0.150 | $0.150 | 3.60s | 86 tok/s |  | 
| Not used in Standard routing:Why these endpoints are not used |  |  |  |  |  |  |  | 
| Flex | $1.50 | $7.50 | $0.075 | $0.075 | 4.52s | 80 tok/s |  | 
|  | $3.30 | $16.50 | $0.165 | $0.165 | 3.18s | 47 tok/s |  | 
|  | $3.30 | $16.50 | $0.165 | $0.165 | 2.41s | 75 tok/s |  | 
| Priority | $5.40 | $27.00 | $0.270 | $0.270 | 1.25s | 73 tok/s |  | 
| Priority | $5.40 | $27.00 | $0.270 | $0.270 | 4.45s | 63 tok/s |  | 

  - $3.00
  - $15.00
  - $0.15
  - $0.15
  - 3.22s
  - 83 tok/s
  - 97.84%
  - $3.00
  - $15.00
  - $0.15
  - $0.15
  - 3.60s
  - 86 tok/s
  - 95.86%
- Not used in Standard routing:Why these endpoints are not used
- Flex
  - $1.50
  - $7.50
  - $0.075
  - $0.075
  - 4.52s
  - 80 tok/s
  - 99.74%
  - $3.30
  - $16.50
  - $0.165
  - $0.165
  - 3.18s
  - 47 tok/s
  - 100.00%
  - $3.30
  - $16.50
  - $0.165
  - $0.165
  - 2.41s
  - 75 tok/s
  - 100.00%
- Priority
  - $5.40
  - $27.00
  - $0.27
  - $0.27
  - 1.25s
  - 73 tok/s
  - 100.00%
- Priority
  - $5.40
  - $27.00
  - $0.27
  - $0.27
  - 4.45s
  - 63 tok/s
  - 100.00%

## Pricing

The average price customers actually pay for this model, next to the prices providers post. Caching and discounts mean the price actually paid is often well below the listed one.

### Weighted average

### Gemini 4 Neon — Price history

|  |  | $0.515 | $15.00 | 87.2% | 80.2% | 
|---|---|---|---|---|---|
|  |  | $0.680 | $15.00 | 81.4% | 4.4% | 
| Not used in Standard routing:Why these endpoints are not used |  |  |  |  |  | 
|  | Flex | $0.291 | $7.50 | 84.8% | 11.1% | 
|  |  | $0.469 | $16.50 | 90.3% | 3.3% | 
|  |  | $0.532 | $16.50 | 88.3% | 0.0% | 
|  | Priority | $1.024 | $27.00 | 85.3% | 0.7% | 
|  | Priority | $1.271 | $27.00 | 80.5% | 0.3% | 

- Effective in $0.515 / Effective out $15.00Token share80.2%
- Effective in $0.680 / Effective out $15.00Token share4.4%
- Not used in Standard routing:Why these endpoints are not used
- Effective in $0.291 / Effective out $7.50FlexToken share11.1%
- Effective in $0.469 / Effective out $16.50Token share3.3%
- Effective in $0.532 / Effective out $16.50Token share0.0%
- Effective in $1.024 / Effective out $27.00PriorityToken share0.7%
- Effective in $1.271 / Effective out $27.00PriorityToken share0.3%

## Performance

Throughput is how fast the model writes (tokens per second — higher is better). Latency is total round-trip time (lower is better). TTFT is time-to-first-token — how long before you see anything appear (lower is better).

## Uptime

Uptime is the percentage of the past 3 days that at least one provider was responding to requests. Availability is the percentage of time that inference was successfully served. OpenRouter continuously monitors and uses the next-best provider when one returns an error.

### Availability over the last 3 days

Last 72 hours
### Availability over the last 24 hours

When an error occurs in an upstream provider, we can recover by routing to another healthy provider, if your request filters allow it. You can access per-provider uptime data programmatically through the Endpoints API. Learn more about our load balancing and provider routing options.

## Benchmarks

Scores on standardized evaluations and this model’s own outputs on shared benchmark prompts. Higher percentages are better — and each rank shows where this model lands among the models evaluated on that source.

| Benchmark score summary for Google: Gemini 4 Neon (Artificial Analysis and Design Arena) |  |  | 
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

## Activity

Token volume and request traffic to this model over time.

## API

Drop-in code to call this model. OpenRouter's API is OpenAI-compatible — most SDKs work by just swapping the base URL. The only thing that changes between models is the model slug below.

## Available through EU in-region routing

Google: Gemini 4 Neon is available through EU in-region routing. Requests sent to eu.openrouter.ai are decrypted in the EU and routed only to shared provider endpoints in Europe (the EU/EEA, the UK, and Switzerland).

- Providers in Europe
- Google Vertex
- Prices in Europe
  - Google Vertex: $3.30/M tokens input, $16.50/M tokens output
- Base URL
- `https://eu.openrouter.ai/api/v1`
- Plan
- Requires Business or Enterprise. Self-serve on Business: no contract, no monthly minimum. OpenRouter Business for EU in-region routing

Fails closed: if no shared provider endpoint in Europe is available, the request errors instead of leaving the region. Requests served by your own BYOK key go to your deployment’s region, which you control. In-region routing docs

## Frequently asked questions

Gemini 4 Neon is a fast, mid-priced model in the Gemini 4 family from Google, built for coding and agentic work. It is suited for repository and terminal coding, web builds, multi-turn tool use and multimodal STEM problems, and is positioned as a step up from Gemini 4 Hydrogen that targets Gemini 4 Carbon quality at higher speed and lower cost.

Gemini 4 Neon costs $3.00/M input tokens and $15.00/M output tokens, with separate rates for Cache Read at $0.15/M tokens, Cache Write at $0.08333/M tokens, Image Input at $3.00/M tokens, Input Audio at $3.00/M tokens, Input Audio Cache at $0.15/M tokens, and Web Search at $14.00/1K calls.

Gemini 4 Neon has a 1,048,576 token context window. It supports up to 65,536 completion tokens.

Yes. Gemini 4 Neon accepts `tools` and `tool_choice` for function calling. It also supports structured outputs via a JSON schema in `response_format`.

Gemini 4 Neon accepts audio, files such as PDFs, images, text, and video as input and returns text.

Gemini 4 Neon is served by 2 providers on OpenRouter: Google AI Studio and Google Vertex. Requests are routed to the best available provider, with automatic failover to the others, and you can pin or exclude providers with provider routing.

Gemini 4 Neon was released on November 18, 2026.
