---
id: collect-261121-veille-ia/liquid-lfm/lfm3-1-7b-free-api-pricing-benchmarks
title: "LFM3-1.7B (free) - API Pricing & Benchmarks"
domain: liquid-lfm
role: reference
task: article
actors: ["Liquid AI"]
dates: ["2026-11-06"]
keywords: ["lfm3-1", "artificial", "analysis", "free", "model", "provider", "index", "outputs"]
source: https://openrouter.ai/liquid/lfm-3-1.7b:free
source_anchor: ""
source_lines: [1, 80]
section: "Liquid_LFM"
---
A compact 1.7B-parameter text model with a 256K-token context, post-trained for agents, tool calling, and structured outputs. It reasons before it answers. Prompts and outputs may be retained and used to train Liquid models.

## Providers

This model is hosted by one provider. OpenRouter forwards every request without routing.

|  | Free | Free | 0.55s | 142 tok/s |  | 

  - Free
  - Free
  - 0.55s
  - 142 tok/s
  - 59.06%

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

| Benchmark score summary for LiquidAI: LFM3-1.7B (free) (Artificial Analysis) |  |  | 
|---|---|---|
| Source | Benchmark | Score | 
|---|---|---|
| Artificial Analysis | LFM3-1.7B Intelligence Index | 6.0 | 
| Artificial Analysis | LFM3-1.7B Finance & Accounting Index | 16.8 | 
| Artificial Analysis | LFM3-1.7B Strategy & Ops Index | 8.7 | 
| Artificial Analysis | LFM3-1.7B Legal Index | 16.1 | 
| Artificial Analysis | LFM3-1.7B Engineering Index | 16.3 | 
| Artificial Analysis | LFM3-1.7B Economics Index | 16.0 | 
| Artificial Analysis | LFM3-1.7B Openness Index | 38.9 | 
| Artificial Analysis | LFM3-1.7B AutomationBench-AA | 7.0% | 
| Artificial Analysis | LFM3-1.7B GDP.pdf | 0.4% | 
| Artificial Analysis | LFM3-1.7B HLE-Diamond | 3.8% | 
| Artificial Analysis | LFM3-1.7B Terminal-Bench 4.0 | 0.0% | 
| Artificial Analysis | LFM3-1.7B FrontierCode-AA 2.0 | 0.0% | 
| Artificial Analysis | LFM3-1.7B Terminal-Bench-Science | 0.0% | 
| Artificial Analysis | LFM3-1.7B CritPt v2 (Hard Subset) | 0.0% | 
| Artificial Analysis | LFM3-1.7B CritPt v2 | 0.6% | 
| Artificial Analysis | LFM3-1.7B AA-Omniscience Accuracy | 6.6% | 
| Artificial Analysis | LFM3-1.7B AA-Omniscience Non-Hallucination Rate | 91.6% | 

## Apps

Public apps that send the most traffic to this model. Good signal for what real production workloads look like — and a hint at which use cases this model is best suited for.

## Activity

Token volume and request traffic to this model over time.

## API

Drop-in code to call this model. OpenRouter's API is OpenAI-compatible — most SDKs work by just swapping the base URL. The only thing that changes between models is the model slug below.

Not currently available through EU in-region routing. In-region routing docs

## Frequently asked questions

A compact 1.7B-parameter text model with a 256K-token context, post-trained for agents, tool calling, and structured outputs. It reasons before it answers. Prompts and outputs may be retained and used to train Liquid models.

LFM3-1.7B (free) has a 262,144 token context window. It supports up to 8,192 completion tokens.

Yes. LFM3-1.7B (free) accepts `tools` and `tool_choice` for function calling. It also supports structured outputs via a JSON schema in `response_format`.

LFM3-1.7B (free) is served by 1 provider on OpenRouter: Liquid.

LFM3-1.7B (free) was released on November 6, 2026.
