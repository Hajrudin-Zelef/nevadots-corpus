---
id: collect-261121-veille-ia/nvidia-hardware/nemotron-3-5-lightning-free-api-pricing-benchmarks
title: "Nemotron 3.5 Lightning (free) - API Pricing & Benchmarks"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: []
keywords: ["nemotron", "lightning", "artificial", "analysis", "free", "index", "nvidia", "provider"]
source: https://openrouter.ai/nvidia/nemotron-3.5-lightning%3Afree?view=api
source_anchor: ""
source_lines: [1, 99]
section: "NVIDIA_Hardware"
---
For the free endpoint, please do not upload any confidential information or personal data (such as voices or faces of people). Your use is logged for security purposes and to improve NVIDIA products and services. The logged session data for improvement purposes is not linked to your identity or any persistent identifier. For more information about NVIDIA's data processing practices, see Privacy PolicyOpens in new tab. By using this free endpoint, you consent to NVIDIA's collection, recording, and use of such information and the NVIDIA API Trial Terms of ServiceOpens in new tab

NVIDIA Nemotron 3.5 Lightning is an open mixture-of-experts model from NVIDIA, with 3B active parameters out of 30B total. It is suited for high-throughput agentic workloads and specialized tasks that benefit from domain-specific customization.

## Providers

Different companies host the same model. OpenRouter routes your request to one of them based on the routing mode you pick: Balanced (price + speed), Nitro (fastest), or Floor (cheapest).

|  | Free | Free | 0.45s | 132 tok/s |  | 

  - Free
  - Free
  - 0.45s
  - 132 tok/s
  - 99.22%

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

| Benchmark score summary for NVIDIA: Nemotron 3.5 Lightning (free) (Artificial Analysis) |  |  | 
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

Guides, cookbooks, blog posts, and tools for building with NVIDIA: Nemotron 3.5 Lightning (free) on OpenRouter.

## Frequently asked questions

NVIDIA Nemotron 3.5 Lightning is an open mixture-of-experts model from NVIDIA, with 3B active parameters out of 30B total. It is suited for high-throughput agentic workloads and specialized tasks that benefit from domain-specific customization.

The Nemotron 3.5 Lightning (free) documentation on OpenRouter includes the What Is Nemotron 3.5 Lightning blog post. The quickstart covers API keys and your first request, and the API reference lists every parameter.

Nemotron 3.5 Lightning (free) has a 1,000,000 token context window. It supports up to 65,536 completion tokens.

Yes. Nemotron 3.5 Lightning (free) accepts `tools` and `tool_choice` for function calling. It does not support `response_format`, so JSON output is not enforced.

Nemotron 3.5 Lightning (free) is served by 1 provider on OpenRouter: NVIDIA.

Nemotron 3.5 Lightning (free) was released on August 11, 2026.
