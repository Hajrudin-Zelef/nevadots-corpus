---
id: collect-261121-veille-ia/qwen/qwen4-max-api-pricing-benchmarks
title: "Qwen4 Max - API Pricing & Benchmarks"
domain: qwen
role: reference
task: article
actors: ["Alibaba"]
dates: ["2026-11-16"]
keywords: ["qwen4", "artificial", "analysis", "index", "vals", "provider", "qwen", "model"]
source: https://openrouter.ai/qwen/qwen4-max
source_anchor: ""
source_lines: [1, 119]
section: "Qwen"
---
Qwen4 Max is the flagship model in Alibaba's Qwen4 series. It accepts text, image, and video input and returns text, with a 1M-token context window and reasoning enabled by default.

Reasoning effort can be set to low, high, or xhigh (default high), or reasoning can be turned off. Tool calling and structured outputs are supported.

## Providers

This model is hosted by one provider. OpenRouter forwards every request without routing.

| 5% off | $1.50$1.425 | $4.50$4.275 | $0.075$0.07125 | $1.875$1.781 | $0.075$0.07125 | 2.39s | 46 tok/s |  | 

- 5% off
  - $1.425
  - $4.275
  - $0.07125
  - $1.781
  - $0.07125
  - 2.39s
  - 46 tok/s
  - 100.00%

## Pricing

The average price customers actually pay for this model, next to the prices providers post. Caching and discounts mean the price actually paid is often well below the listed one.

### Weighted average

### Qwen4 Max — Price history

|  |  | $0.235 | $4.45 | 90.4% | 100.0% | 
|---|---|---|---|---|---|

- Effective in $0.235 / Effective out $4.45Token share100.0%

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

| Benchmark score summary for Qwen: Qwen4 Max (Artificial Analysis and Vals AI) |  |  | 
|---|---|---|
| Source | Benchmark | Score | 
|---|---|---|
| Artificial Analysis | Qwen4 Max Intelligence Index | 42.9 | 
| Artificial Analysis | Qwen4 Max Finance & Accounting Index | 54.1 | 
| Artificial Analysis | Qwen4 Max Strategy & Ops Index | 56.9 | 
| Artificial Analysis | Qwen4 Max Legal Index | 49.6 | 
| Artificial Analysis | Qwen4 Max Healthcare & Medical Index | 48.2 | 
| Artificial Analysis | Qwen4 Max Engineering Index | 50.1 | 
| Artificial Analysis | Qwen4 Max Economics Index | 51.3 | 
| Artificial Analysis | Qwen4 Max AutomationBench-AA | 72.6% | 
| Artificial Analysis | Qwen4 Max GDP.pdf | 26.2% | 
| Artificial Analysis | Qwen4 Max APEX-Agents-AA | 59.1% | 
| Artificial Analysis | Qwen4 Max AA-AnalystAgent | 46.3% | 
| Artificial Analysis | Qwen4 Max EnterpriseOps-Gym-AA | 48.5% | 
| Artificial Analysis | Qwen4 Max Harvey LAB-AA | 9.4% | 
| Artificial Analysis | Qwen4 Max GDPval-AA | 65.4% | 
| Artificial Analysis | Qwen4 Max HLE-Diamond | 44.9% | 
| Artificial Analysis | Qwen4 Max Terminal-Bench 4.0 | 48.5% | 
| Artificial Analysis | Qwen4 Max FrontierCode-AA 2.0 | 26.5% | 
| Artificial Analysis | Qwen4 Max Terminal-Bench-Science | 23.3% | 
| Artificial Analysis | Qwen4 Max CritPt v2 (Hard Subset) | 43.0% | 
| Artificial Analysis | Qwen4 Max CritPt v2 | 66.0% | 
| Artificial Analysis | Qwen4 Max AA-Omniscience Accuracy | 43.7% | 
| Artificial Analysis | Qwen4 Max AA-Omniscience Non-Hallucination Rate | 71.6% | 
| Artificial Analysis | Qwen4 Max MLCR-AA | 19.4% | 
| Vals AI | Qwen 4 Max EMB v1.1 | 70.8% | 
| Vals AI | Qwen 4 Max Finance Agent v2.1 | 66.7% | 
| Vals AI | Qwen 4 Max Vals Finance Index | 62.0% | 
| Vals AI | Qwen 4 Max HLAB-Vals-1.1 | 21.3% | 
| Vals AI | Qwen 4 Max Vals Legal Index | 70.7% | 
| Vals AI | Qwen 4 Max Legal Research Bench | 41.3% | 
| Vals AI | Qwen 4 Max Tax Agent Bench | 40.4% | 
| Vals AI | Qwen 4 Max Terminal-Bench 4.0 | 48.5% | 
| Vals AI | Qwen 4 Max Vals Index | 63.2% | 
| Vals AI | Qwen 4 Max Vibe Code Bench v1.1 | 86.7% | 

## Apps

Public apps that send the most traffic to this model. Good signal for what real production workloads look like — and a hint at which use cases this model is best suited for.

## Activity

Token volume and request traffic to this model over time.

## API

Drop-in code to call this model. OpenRouter's API is OpenAI-compatible — most SDKs work by just swapping the base URL. The only thing that changes between models is the model slug below.

Not currently available through EU in-region routing. In-region routing docs

## Frequently asked questions

Qwen4 Max is the flagship model in Alibaba's Qwen4 series. It accepts text, image, and video input and returns text, with a 1M-token context window and reasoning enabled by default. Reasoning effort can be set to low, high, or xhigh (default high), or reasoning can be turned off. Tool calling and structured outputs are supported.

Qwen4 Max costs $1.425/M input tokens and $4.275/M output tokens, with separate rates for Cache Read at $0.07125/M tokens, Cache Creation (5min) at $1.781/M tokens, Cache Creation (1h) at $2.85/M tokens, and Cache Read (5min) at $0.07125/M tokens.

Qwen4 Max has a 1,000,000 token context window. It supports up to 131,072 completion tokens.

Yes. Qwen4 Max accepts `tools` and `tool_choice` for function calling. It also supports structured outputs via a JSON schema in `response_format`.

Qwen4 Max accepts text, images, and video as input and returns text.

Qwen4 Max is served by 1 provider on OpenRouter: Alibaba Cloud Int..

Qwen4 Max was released on November 16, 2026.
