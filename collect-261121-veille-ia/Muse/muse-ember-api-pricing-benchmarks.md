---
id: collect-261121-veille-ia/muse/muse-ember-api-pricing-benchmarks
title: "Muse Ember - API Pricing & Benchmarks"
domain: muse
role: reference
task: article
actors: ["Meta"]
dates: []
keywords: ["muse", "ember", "artificial", "analysis", "index", "provider", "model", "routing"]
source: https://openrouter.ai/meta/muse-ember-20261109:nitro
source_anchor: ""
source_lines: [1, 144]
section: "Muse"
---
Muse Ember is the smallest and most efficient model from Meta that’s designed for complex reasoning execution and the work agents repeat all day: tool calls, coding agent subtasks, routing, extraction, and long document parsing.

Muse Ember accepts text, images, video, audio, and PDF documents and returns text, with a 1M-token context window.

## Providers

Different companies host the same model. OpenRouter routes your request to one of them based on the routing mode you pick: Balanced (price + speed), Nitro (fastest), or Floor (cheapest).

| Provider | Input /M | Output /M | Cache read /M | Latency | Throughput | Uptime | 
|---|---|---|---|---|---|---|
|  | $0.10 | $0.50 | $0.004 | 2.03s | 270 tok/s |  | 
|  | $0.10 | $0.50 | $0.004 | 1.90s | 266 tok/s |  | 

  - $0.10
  - $0.50
  - $0.004
  - 2.03s
  - 270 tok/s
  - 100.00%
  - $0.10
  - $0.50
  - $0.004
  - 1.90s
  - 266 tok/s
  - 100.00%

## Pricing

The average price customers actually pay for this model, next to the prices providers post. Caching and discounts mean the price actually paid is often well below the listed one.

### Weighted average

### Muse Ember — Price history

|  |  | $0.0141 | $0.500 | 89.5% | 49.3% | 
|---|---|---|---|---|---|
|  |  | $0.0274 | $0.500 | 75.6% | 50.7% | 

- Effective in $0.0141 / Effective out $0.500Token share49.3%
- Effective in $0.0274 / Effective out $0.500Token share50.7%

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

| Benchmark score summary for Meta: Muse Ember (Artificial Analysis, Design Arena and Vals AI) |  |  | 
|---|---|---|
| Source | Benchmark | Score | 
|---|---|---|
| Artificial Analysis | Muse Ember (Max) Intelligence Index | 36.0 | 
| Artificial Analysis | Muse Ember (Max) Finance & Accounting Index | 44.6 | 
| Artificial Analysis | Muse Ember (Max) Strategy & Ops Index | 48.7 | 
| Artificial Analysis | Muse Ember (Max) Legal Index | 38.5 | 
| Artificial Analysis | Muse Ember (Max) Healthcare & Medical Index | 41.3 | 
| Artificial Analysis | Muse Ember (Max) Engineering Index | 41.8 | 
| Artificial Analysis | Muse Ember (Max) Economics Index | 41.0 | 
| Artificial Analysis | Muse Ember (Max) AutomationBench-AA | 68.1% | 
| Artificial Analysis | Muse Ember (Max) GDP.pdf | 22.8% | 
| Artificial Analysis | Muse Ember (Max) HLE-Diamond | 25.1% | 
| Artificial Analysis | Muse Ember (Max) Terminal-Bench 4.0 | 40.9% | 
| Artificial Analysis | Muse Ember (Max) FrontierCode-AA 2.0 | 27.2% | 
| Artificial Analysis | Muse Ember (Max) Terminal-Bench-Science | 20.0% | 
| Artificial Analysis | Muse Ember (Max) CritPt v2 (Hard Subset) | 39.0% | 
| Artificial Analysis | Muse Ember (Max) CritPt v2 | 62.3% | 
| Artificial Analysis | Muse Ember (Max) AA-Omniscience Accuracy | 21.1% | 
| Artificial Analysis | Muse Ember (Max) AA-Omniscience Non-Hallucination Rate | 82.6% | 
| Artificial Analysis | Muse Ember (Max) MLCR-AA | 18.3% | 
| Artificial Analysis | Muse Ember (Xhigh) Intelligence Index | 34.1 | 
| Artificial Analysis | Muse Ember (Xhigh) Finance & Accounting Index | 43.6 | 
| Artificial Analysis | Muse Ember (Xhigh) Strategy & Ops Index | 48.5 | 
| Artificial Analysis | Muse Ember (Xhigh) Legal Index | 37.8 | 
| Artificial Analysis | Muse Ember (Xhigh) Engineering Index | 40.9 | 
| Artificial Analysis | Muse Ember (Xhigh) Economics Index | 40.4 | 
| Artificial Analysis | Muse Ember (Xhigh) AutomationBench-AA | 66.7% | 
| Artificial Analysis | Muse Ember (Xhigh) GDP.pdf | 22.4% | 
| Artificial Analysis | Muse Ember (Xhigh) HLE-Diamond | 23.6% | 
| Artificial Analysis | Muse Ember (Xhigh) Terminal-Bench 4.0 | 35.4% | 
| Artificial Analysis | Muse Ember (Xhigh) FrontierCode-AA 2.0 | 22.0% | 
| Artificial Analysis | Muse Ember (Xhigh) Terminal-Bench-Science | 17.6% | 
| Artificial Analysis | Muse Ember (Xhigh) CritPt v2 (Hard Subset) | 38.5% | 
| Artificial Analysis | Muse Ember (Xhigh) CritPt v2 | 62.0% | 
| Artificial Analysis | Muse Ember (Xhigh) AA-Omniscience Accuracy | 21.4% | 
| Artificial Analysis | Muse Ember (Xhigh) AA-Omniscience Non-Hallucination Rate | 82.7% | 
| Design Arena | Muse Ember Models Arena 3D Elo | 1357 | 
| Design Arena | Muse Ember Models Arena Code Categories Elo | 1341 | 
| Design Arena | Muse Ember Models Arena Data Visualization Elo | 1314 | 
| Design Arena | Muse Ember Models Arena Game Development Elo | 1321 | 
| Design Arena | Muse Ember Models Arena UI Component Elo | 1306 | 
| Design Arena | Muse Ember Models Arena Website Elo | 1326 | 
| Vals AI | Muse Ember EMB v1.1 | 72.2% | 
| Vals AI | Muse Ember Finance Agent v2.1 | 63.8% | 
| Vals AI | Muse Ember Vals Finance Index | 60.9% | 
| Vals AI | Muse Ember HLAB-Vals-1.1 | 25.0% | 
| Vals AI | Muse Ember Vals Legal Index | 75.7% | 
| Vals AI | Muse Ember Legal Research Bench | 49.0% | 
| Vals AI | Muse Ember Tax Agent Bench | 38.2% | 
| Vals AI | Muse Ember Terminal-Bench 4.0 | 39.4% | 
| Vals AI | Muse Ember Vals Index | 60.5% | 
| Vals AI | Muse Ember Vibe Code Bench v1.1 | 83.7% | 

## Apps

Public apps that send the most traffic to this model. Good signal for what real production workloads look like — and a hint at which use cases this model is best suited for.

## Activity

Token volume and request traffic to this model over time.

## API

Drop-in code to call this model. OpenRouter's API is OpenAI-compatible — most SDKs work by just swapping the base URL. The only thing that changes between models is the model slug below.

Not currently available through EU in-region routing. In-region routing docs

## Frequently asked questions

Muse Ember is the smallest and most efficient model from Meta that’s designed for complex reasoning execution and the work agents repeat all day: tool calls, coding agent subtasks, routing, extraction, and long document parsing. Muse Ember accepts text, images, video, audio, and PDF documents and returns text, with a 1M-token context window.

Muse Ember costs $0.10/M input tokens and $0.50/M output tokens, with separate rates for Cache Read at $0.004/M tokens.

Muse Ember has a 1,048,576 token context window. It supports up to 524,288 completion tokens.

Yes. Muse Ember accepts `tools` and `tool_choice` for function calling. It also supports structured outputs via a JSON schema in `response_format`.

Muse Ember accepts text, images, video, files such as PDFs, and audio as input and returns text.

Muse Ember is served by 2 providers on OpenRouter: Google Vertex and Meta. Requests are routed to the best available provider, with automatic failover to the others, and you can pin or exclude providers with provider routing.

Muse Ember was released on November 9, 2026.
