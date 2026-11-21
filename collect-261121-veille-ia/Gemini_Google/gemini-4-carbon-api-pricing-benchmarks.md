---
id: collect-261121-veille-ia/gemini-google/gemini-4-carbon-api-pricing-benchmarks
title: "Gemini 4 Carbon - API Pricing & Benchmarks"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-11-10"]
keywords: ["effective", "used", "routing", "gemini", "carbon", "endpoints", "provider", "model"]
source: https://openrouter.ai/google/gemini-4-carbon-20261110
source_anchor: ""
source_lines: [1, 248]
section: "Gemini_Google"
---
Gemini 4 Carbon is a frontier Gemini 4 model from Google for coding, agentic and complex reasoning work. It takes text, image, audio, video and file input, supports long decode of up to 1M output tokens through continuation, and controls thinking depth with low, medium and high reasoning effort.

## Providers

Different companies host the same model. OpenRouter routes your request to one of them based on the routing mode you pick: Balanced (price + speed), Nitro (fastest), or Floor (cheapest).

| 50% off | $4.00$2.00 | $20.00$10.00 | $0.20$0.10 | $0.20$0.10 | 4.83s | 64 tok/s |  | 
| 50% off | $4.00$2.00 | $20.00$10.00 | $0.20$0.10 | $0.20$0.10 | 8.94s | 35 tok/s |  | 
| Not used in Standard routing:Why these endpoints are not used |  |  |  |  |  |  |  | 
| Flex50% off | $2.00$1.00 | $10.00$5.00 | $0.10$0.05 | $0.10$0.05 | 6.62s | 37 tok/s |  | 
| 50% off | $4.40$2.20 | $22.00$11.00 | $0.22$0.11 | $0.22$0.11 | 5.16s | 76 tok/s |  | 
| 50% off | $4.40$2.20 | $22.00$11.00 | $0.22$0.11 | $0.22$0.11 | 4.92s | 28 tok/s |  | 
| Priority50% off | $7.20$3.60 | $36.00$18.00 | $0.36$0.18 | $0.36$0.18 | 7.92s | 32 tok/s |  | 
| Priority50% off | $7.20$3.60 | $36.00$18.00 | $0.36$0.18 | $0.36$0.18 | 5.62s | 52 tok/s |  | 

- 50% off
  - $2.00
  - $10.00
  - $0.10
  - $0.10
  - 4.83s
  - 64 tok/s
  - 98.09%
- 50% off
  - $2.00
  - $10.00
  - $0.10
  - $0.10
  - 8.94s
  - 35 tok/s
  - 60.83%
- Not used in Standard routing:Why these endpoints are not used
- 50% offFlex
  - $1.00
  - $5.00
  - $0.05
  - $0.05
  - 6.62s
  - 37 tok/s
  - 100.00%
- 50% off
  - $2.20
  - $11.00
  - $0.11
  - $0.11
  - 5.16s
  - 76 tok/s
  - 88.80%
- 50% off
  - $2.20
  - $11.00
  - $0.11
  - $0.11
  - 4.92s
  - 28 tok/s
  - 92.33%
- 50% offPriority
  - $3.60
  - $18.00
  - $0.18
  - $0.18
  - 7.92s
  - 32 tok/s
  - 79.52%
- 50% offPriority
  - $3.60
  - $18.00
  - $0.18
  - $0.18
  - 5.62s
  - 52 tok/s
  - 85.03%

## Pricing

The average price customers actually pay for this model, next to the prices providers post. Caching and discounts mean the price actually paid is often well below the listed one.

### Weighted average

### Gemini 4 Carbon — Price history

|  |  | $0.338 | $10.14 | 87.9% | 91.1% | 
|---|---|---|---|---|---|
|  |  | $0.427 | $10.58 | 84.8% | 3.3% | 
| Not used in Standard routing:Why these endpoints are not used |  |  |  |  |  | 
|  | Flex | $0.319 | $5.02 | 72.1% | 2.6% | 
|  | Priority | $0.773 | $20.02 | 86.0% | 0.3% | 
|  |  | $0.885 | $11.03 | 64.3% | 0.1% | 
|  | Priority | $0.912 | $31.65 | 90.4% | 2.5% | 
|  |  | $1.047 | $12.44 | 75.5% | 0.2% | 

- Effective in $0.338 / Effective out $10.14Token share91.1%
- Effective in $0.427 / Effective out $10.58Token share3.3%
- Not used in Standard routing:Why these endpoints are not used
- Effective in $0.319 / Effective out $5.02FlexToken share2.6%
- Effective in $0.773 / Effective out $20.02PriorityToken share0.3%
- Effective in $0.885 / Effective out $11.03Token share0.1%
- Effective in $0.912 / Effective out $31.65PriorityToken share2.5%
- Effective in $1.047 / Effective out $12.44Token share0.2%

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

| Benchmark score summary for Google: Gemini 4 Carbon (Artificial Analysis, Design Arena and Vals AI) |  |  | 
|---|---|---|
| Source | Benchmark | Score | 
|---|---|---|
| Artificial Analysis | Gemini 4 Carbon (High) Intelligence Index | 60.0 | 
| Artificial Analysis | Gemini 4 Carbon (High) Finance & Accounting Index | 62.6 | 
| Artificial Analysis | Gemini 4 Carbon (High) Strategy & Ops Index | 60.9 | 
| Artificial Analysis | Gemini 4 Carbon (High) Legal Index | 62.0 | 
| Artificial Analysis | Gemini 4 Carbon (High) Healthcare & Medical Index | 56.8 | 
| Artificial Analysis | Gemini 4 Carbon (High) Engineering Index | 65.3 | 
| Artificial Analysis | Gemini 4 Carbon (High) Economics Index | 59.6 | 
| Artificial Analysis | Gemini 4 Carbon (High) AutomationBench-AA | 77.8% | 
| Artificial Analysis | Gemini 4 Carbon (High) GDP.pdf | 28.4% | 
| Artificial Analysis | Gemini 4 Carbon (High) AA-LCR | 79.3% | 
| Artificial Analysis | Gemini 4 Carbon (High) GDPval-AA | 61.6% | 
| Artificial Analysis | Gemini 4 Carbon (High) HLE-Diamond | 69.1% | 
| Artificial Analysis | Gemini 4 Carbon (High) Terminal-Bench 4.0 | 65.2% | 
| Artificial Analysis | Gemini 4 Carbon (High) FrontierCode-AA 2.0 | 40.6% | 
| Artificial Analysis | Gemini 4 Carbon (High) Terminal-Bench-Science | 64.8% | 
| Artificial Analysis | Gemini 4 Carbon (High) CritPt v2 (Hard Subset) | 69.0% | 
| Artificial Analysis | Gemini 4 Carbon (High) CritPt v2 | 82.3% | 
| Artificial Analysis | Gemini 4 Carbon (High) AA-Omniscience Accuracy | 58.3% | 
| Artificial Analysis | Gemini 4 Carbon (High) AA-Omniscience Non-Hallucination Rate | 88.6% | 
| Artificial Analysis | Gemini 4 Carbon (High) MLCR-AA | 19.4% | 
| Artificial Analysis | Gemini 4 Carbon (High) VIALS-AA | 64.0% | 
| Artificial Analysis | Gemini 4 Carbon (Low) Intelligence Index | 54.2 | 
| Artificial Analysis | Gemini 4 Carbon (Low) Finance & Accounting Index | 56.0 | 
| Artificial Analysis | Gemini 4 Carbon (Low) Strategy & Ops Index | 53.0 | 
| Artificial Analysis | Gemini 4 Carbon (Low) Legal Index | 54.0 | 
| Artificial Analysis | Gemini 4 Carbon (Low) Healthcare & Medical Index | 51.7 | 
| Artificial Analysis | Gemini 4 Carbon (Low) Engineering Index | 60.2 | 
| Artificial Analysis | Gemini 4 Carbon (Low) Economics Index | 55.2 | 
| Artificial Analysis | Gemini 4 Carbon (Low) AutomationBench-AA | 63.5% | 
| Artificial Analysis | Gemini 4 Carbon (Low) GDP.pdf | 28.6% | 
| Artificial Analysis | Gemini 4 Carbon (Low) HLE-Diamond | 61.9% | 
| Artificial Analysis | Gemini 4 Carbon (Low) Terminal-Bench 4.0 | 62.6% | 
| Artificial Analysis | Gemini 4 Carbon (Low) FrontierCode-AA 2.0 | 29.6% | 
| Artificial Analysis | Gemini 4 Carbon (Low) Terminal-Bench-Science | 61.9% | 
| Artificial Analysis | Gemini 4 Carbon (Low) CritPt v2 (Hard Subset) | 65.0% | 
| Artificial Analysis | Gemini 4 Carbon (Low) CritPt v2 | 79.7% | 
| Artificial Analysis | Gemini 4 Carbon (Low) AA-Omniscience Accuracy | 49.2% | 
| Artificial Analysis | Gemini 4 Carbon (Low) AA-Omniscience Non-Hallucination Rate | 80.2% | 
| Artificial Analysis | Gemini 4 Carbon (Low) MLCR-AA | 18.9% | 
| Artificial Analysis | Gemini 4 Carbon (Medium) Intelligence Index | 58.3 | 
| Artificial Analysis | Gemini 4 Carbon (Medium) Finance & Accounting Index | 59.6 | 
| Artificial Analysis | Gemini 4 Carbon (Medium) Strategy & Ops Index | 57.8 | 
| Artificial Analysis | Gemini 4 Carbon (Medium) Legal Index | 59.5 | 
| Artificial Analysis | Gemini 4 Carbon (Medium) Healthcare & Medical Index | 54.8 | 
| Artificial Analysis | Gemini 4 Carbon (Medium) Engineering Index | 63.9 | 
| Artificial Analysis | Gemini 4 Carbon (Medium) Economics Index | 57.5 | 
| Artificial Analysis | Gemini 4 Carbon (Medium) AutomationBench-AA | 77.6% | 
| Artificial Analysis | Gemini 4 Carbon (Medium) GDP.pdf | 29.6% | 
| Artificial Analysis | Gemini 4 Carbon (Medium) HLE-Diamond | 67.4% | 
| Artificial Analysis | Gemini 4 Carbon (Medium) Terminal-Bench 4.0 | 60.6% | 
| Artificial Analysis | Gemini 4 Carbon (Medium) FrontierCode-AA 2.0 | 39.5% | 
| Artificial Analysis | Gemini 4 Carbon (Medium) Terminal-Bench-Science | 62.4% | 
| Artificial Analysis | Gemini 4 Carbon (Medium) CritPt v2 (Hard Subset) | 69.0% | 
| Artificial Analysis | Gemini 4 Carbon (Medium) CritPt v2 | 82.3% | 
| Artificial Analysis | Gemini 4 Carbon (Medium) AA-Omniscience Accuracy | 48.9% | 
| Artificial Analysis | Gemini 4 Carbon (Medium) AA-Omniscience Non-Hallucination Rate | 95.8% | 
| Artificial Analysis | Gemini 4 Carbon (Medium) MLCR-AA | 18.3% | 
| Artificial Analysis | Gemini 4 Carbon (Medium) MMMU-Pro | 89.2% | 
| Design Arena | Gemini 4 Carbon Agents Arena Codetovideo Elo | 1269 | 
| Design Arena | Gemini 4 Carbon Agents Arena Mobile Apps Elo | 1293 | 
| Design Arena | Gemini 4 Carbon Agents Arena Pdf Elo | 1311 | 
| Design Arena | Gemini 4 Carbon Agents Arena Spreadsheet Elo | 1220 | 
| Design Arena | Gemini 4 Carbon Models Arena 3D Elo | 1412 | 
| Design Arena | Gemini 4 Carbon Models Arena Code Categories Elo | 1335 | 
| Design Arena | Gemini 4 Carbon Models Arena Data Visualization Elo | 1339 | 
| Design Arena | Gemini 4 Carbon Models Arena Game Development Elo | 1325 | 
| Design Arena | Gemini 4 Carbon Models Arena SVG Elo | 1395 | 
| Design Arena | Gemini 4 Carbon Models Arena UI Component Elo | 1342 | 
| Design Arena | Gemini 4 Carbon Models Arena Website Elo | 1305 | 
| Vals AI | Gemini 4 Carbon Autoform-Hard | 71.4% | 
| Vals AI | Gemini 4 Carbon EMB v1.1 | 76.5% | 
| Vals AI | Gemini 4 Carbon Finance Agent v2.1 | 67.2% | 
| Vals AI | Gemini 4 Carbon Vals Finance Index | 67.0% | 
| Vals AI | Gemini 4 Carbon HLAB-Vals-1.1 | 17.5% | 
| Vals AI | Gemini 4 Carbon Vals Legal Index | 75.9% | 
| Vals AI | Gemini 4 Carbon Legal Research Bench | 55.8% | 
| Vals AI | Gemini 4 Carbon Vals RSI Index | 27.7% | 
| Vals AI | Gemini 4 Carbon Tax Agent Bench | 51.8% | 
| Vals AI | Gemini 4 Carbon Terminal-Bench 4.0 | 63.1% | 
| Vals AI | Gemini 4 Carbon Vals Index | 70.5% | 
| Vals AI | Gemini 4 Carbon Vibe Code Bench 1-100 v1.1 | 49.7% | 
| Vals AI | Gemini 4 Carbon Vibe Code Bench v1.1 | 89.1% | 

## Apps

Public apps that send the most traffic to this model. Good signal for what real production workloads look like — and a hint at which use cases this model is best suited for.

## Activity

Token volume and request traffic to this model over time.

## API

Drop-in code to call this model. OpenRouter's API is OpenAI-compatible — most SDKs work by just swapping the base URL. The only thing that changes between models is the model slug below.

## Available through EU in-region routing

Google: Gemini 4 Carbon is available through EU in-region routing. Requests sent to eu.openrouter.ai are decrypted in the EU and routed only to shared provider endpoints in Europe (the EU/EEA, the UK, and Switzerland).

- Providers in Europe
- Google Vertex
- Prices in Europe
  - Google Vertex: $2.20/M tokens input, $11/M tokens output
- Base URL
- `https://eu.openrouter.ai/api/v1`
- Plan
- Requires Business or Enterprise. Self-serve on Business: no contract, no monthly minimum. OpenRouter Business for EU in-region routing

Fails closed: if no shared provider endpoint in Europe is available, the request errors instead of leaving the region. Requests served by your own BYOK key go to your deployment’s region, which you control. In-region routing docs

## Frequently asked questions

Gemini 4 Carbon is a frontier Gemini 4 model from Google for coding, agentic and complex reasoning work. It takes text, image, audio, video and file input, supports long decode of up to 1M output tokens through continuation, and controls thinking depth with low, medium and high reasoning effort.

Gemini 4 Carbon costs $2.00/M input tokens and $10.00/M output tokens, with separate rates for Cache Read at $0.10/M tokens, Cache Write at $0.1875/M tokens, Image Input at $2.00/M tokens, Input Audio at $2.00/M tokens, Input Audio Cache at $0.10/M tokens, and Web Search at $14.00/1K calls.

Gemini 4 Carbon has a 1,048,576 token context window. It supports up to 1,048,576 completion tokens.

Yes. Gemini 4 Carbon accepts `tools` and `tool_choice` for function calling. It also supports structured outputs via a JSON schema in `response_format`.

Gemini 4 Carbon accepts audio, files such as PDFs, images, text, and video as input and returns text.

Gemini 4 Carbon is served by 2 providers on OpenRouter: Google AI Studio and Google Vertex. Requests are routed to the best available provider, with automatic failover to the others, and you can pin or exclude providers with provider routing.

Gemini 4 Carbon was released on November 10, 2026.
