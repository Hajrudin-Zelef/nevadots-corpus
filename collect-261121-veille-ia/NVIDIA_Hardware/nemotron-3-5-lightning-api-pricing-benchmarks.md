---
id: collect-261121-veille-ia/nvidia-hardware/nemotron-3-5-lightning-api-pricing-benchmarks
title: "Nemotron 3.5 Lightning - API Pricing & Benchmarks"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: []
keywords: ["effective", "nemotron", "lightning", "nvidia", "model", "token", "provider", "artificial"]
source: https://openrouter.ai/nvidia/nemotron-3.5-lightning
source_anchor: ""
source_lines: [1, 151]
section: "NVIDIA_Hardware"
---
For the free endpoint, please do not upload any confidential information or personal data (such as voices or faces of people). Your use is logged for security purposes and to improve NVIDIA products and services. The logged session data for improvement purposes is not linked to your identity or any persistent identifier. For more information about NVIDIA's data processing practices, see Privacy PolicyOpens in new tab. By using this free endpoint, you consent to NVIDIA's collection, recording, and use of such information and the NVIDIA API Trial Terms of ServiceOpens in new tab

NVIDIA Nemotron 3.5 Lightning is an open mixture-of-experts model from NVIDIA, with 3B active parameters out of 30B total. It is suited for high-throughput agentic workloads and specialized tasks that benefit from domain-specific customization.

## Providers

Different companies host the same model. OpenRouter routes your request to one of them based on the routing mode you pick: Balanced (price + speed), Nitro (fastest), or Floor (cheapest).

|  | $0.05 | $0.200 | $0.01 | 0.28s | 318 tok/s |  | 
|  | $0.07 | $0.200 | $0.04 | 0.12s | 290 tok/s |  | 
|  | $0.06 | $0.160 | $0.03 | 0.25s | 180 tok/s |  | 
|  | $0.07 | $0.200 | $0.04 | 0.34s | 139 tok/s |  | 
|  | $0.02 | $0.096 | $0.01 | 1.29s | 94 tok/s |  | 

  - $0.05
  - $0.20
  - $0.01
  - 0.28s
  - 318 tok/s
  - 100.00%
  - $0.07
  - $0.20
  - $0.04
  - 0.12s
  - 290 tok/s
  - 100.00%
  - $0.06
  - $0.16
  - $0.03
  - 0.25s
  - 180 tok/s
  - 99.87%
  - $0.07
  - $0.20
  - $0.04
  - 0.34s
  - 139 tok/s
  - 100.00%
  - $0.02
  - $0.096
  - $0.01
  - 1.29s
  - 94 tok/s
  - 100.00%

## Pricing

The average price customers actually pay for this model, next to the prices providers post. Caching and discounts mean the price actually paid is often well below the listed one.

### Weighted average

### Nemotron 3.5 Lightning — Price history

|  |  | $0.0133 | $0.0958 | 66.8% | 1.4% | 
|---|---|---|---|---|---|
|  |  | $0.0206 | $0.1998 | 73.5% | 15.1% | 
|  |  | $0.0383 | $0.1597 | 72.4% | 10.5% | 
|  |  | $0.0442 | $0.1995 | 85.8% | 12.4% | 
|  |  | $0.0526 | $0.1999 | 57.9% | 60.5% | 

- Effective in $0.0133 / Effective out $0.0958Token share1.4%
- Effective in $0.0206 / Effective out $0.1998Token share15.1%
- Effective in $0.0383 / Effective out $0.1597Token share10.5%
- Effective in $0.0442 / Effective out $0.1995Token share12.4%
- Effective in $0.0526 / Effective out $0.1999Token share60.5%

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

| Benchmark score summary for NVIDIA: Nemotron 3.5 Lightning (Artificial Analysis) |  |  | 
|---|---|---|
| Source | Benchmark | Score | 
|---|---|---|
| Artificial Analysis | Nemotron 3.5 Lightning Intelligence Index | 5.5 | 
| Artificial Analysis | Nemotron 3.5 Lightning Finance & Accounting Index | 14.9 | 
| Artificial Analysis | Nemotron 3.5 Lightning Strategy & Ops Index | 7.1 | 
| Artificial Analysis | Nemotron 3.5 Lightning Legal Index | 14.2 | 
| Artificial Analysis | Nemotron 3.5 Lightning Healthcare & Medical Index | 9.0 | 
| Artificial Analysis | Nemotron 3.5 Lightning Engineering Index | 15.5 | 
| Artificial Analysis | Nemotron 3.5 Lightning Economics Index | 14.8 | 
| Artificial Analysis | Nemotron 3.5 Lightning Openness Index | 83.3 | 
| Artificial Analysis | Nemotron 3.5 Lightning AutomationBench-AA | 0.8% | 
| Artificial Analysis | Nemotron 3.5 Lightning GDP.pdf | 3.0% | 
| Artificial Analysis | Nemotron 3.5 Lightning EnterpriseOps-Gym-AA | 18.4% | 
| Artificial Analysis | Nemotron 3.5 Lightning GPQA Diamond | 74.3% | 
| Artificial Analysis | Nemotron 3.5 Lightning HLE | 10.6% | 
| Artificial Analysis | Nemotron 3.5 Lightning AA-LCR | 60.3% | 
| Artificial Analysis | Nemotron 3.5 Lightning τ³-Banking | 8.9% | 
| Artificial Analysis | Nemotron 3.5 Lightning GDPval-AA | 7.3% | 
| Artificial Analysis | Nemotron 3.5 Lightning CritPt | 0.0% | 
| Artificial Analysis | Nemotron 3.5 Lightning HLE-Diamond | 4.5% | 
| Artificial Analysis | Nemotron 3.5 Lightning SciCode | 32.1% | 
| Artificial Analysis | Nemotron 3.5 Lightning Terminal-Bench 4.0 | 0.5% | 
| Artificial Analysis | Nemotron 3.5 Lightning Terminal-Bench 2.1 | 24.3% | 
| Artificial Analysis | Nemotron 3.5 Lightning FrontierCode-AA 2.0 | 0.0% | 
| Artificial Analysis | Nemotron 3.5 Lightning Terminal-Bench-Science | 0.0% | 
| Artificial Analysis | Nemotron 3.5 Lightning CritPt v2 (Hard Subset) | 1.5% | 
| Artificial Analysis | Nemotron 3.5 Lightning CritPt v2 | 6.3% | 
| Artificial Analysis | Nemotron 3.5 Lightning AA-Omniscience Accuracy | 14.7% | 
| Artificial Analysis | Nemotron 3.5 Lightning AA-Omniscience Non-Hallucination Rate | 64.3% | 
| Artificial Analysis | Nemotron 3.5 Lightning MLCR-AA | 2.2% | 

## Apps

Public apps that send the most traffic to this model. Good signal for what real production workloads look like — and a hint at which use cases this model is best suited for.

## Activity

Token volume and request traffic to this model over time.

## API

Drop-in code to call this model. OpenRouter's API is OpenAI-compatible — most SDKs work by just swapping the base URL. The only thing that changes between models is the model slug below.

Not currently available through EU in-region routing. In-region routing docs

## Documentation

Guides, cookbooks, blog posts, and tools for building with NVIDIA: Nemotron 3.5 Lightning on OpenRouter.

## Frequently asked questions

NVIDIA Nemotron 3.5 Lightning is an open mixture-of-experts model from NVIDIA, with 3B active parameters out of 30B total. It is suited for high-throughput agentic workloads and specialized tasks that benefit from domain-specific customization.

The Nemotron 3.5 Lightning documentation on OpenRouter includes the What Is Nemotron 3.5 Lightning blog post. The quickstart covers API keys and your first request, and the API reference lists every parameter.

Nemotron 3.5 Lightning costs $0.02/M input tokens and $0.096/M output tokens, with separate rates for Cache Read at $0.01/M tokens.

Nemotron 3.5 Lightning has a 1,048,576 token context window. It supports up to 65,536 completion tokens.

Yes. Nemotron 3.5 Lightning accepts `tools` and `tool_choice` for function calling. It also supports structured outputs via a JSON schema in `response_format`.

Nemotron 3.5 Lightning is served by 5 providers on OpenRouter: Darkbloom, Fireworks, DeepInfra, ModelRun [by Modular], and CoreWeave. Requests are routed to the best available provider, with automatic failover to the others, and you can pin or exclude providers with provider routing.

Nemotron 3.5 Lightning was released on August 11, 2026.
