---
id: collect-261121-veille-ia/modeles-open/beam-1-0-501b-a23b-api-pricing-benchmarks
title: "Beam 1.0 501B A23B - API Pricing & Benchmarks"
domain: modeles-open
role: reference
task: article
actors: []
dates: []
keywords: ["beam", "artificial", "effective", "analysis", "index", "token", "model", "a23b"]
source: https://openrouter.ai/reflection/beam-1.0-501b-a23b
source_anchor: ""
source_lines: [1, 170]
section: "Modeles_Open"
---
Beam 1.0 is Reflection's open-weight reasoning model for coding and long-horizon agentic workflows. It uses a sparse mixture-of-experts architecture with 501B total and 23B active parameters per token, and a 1M-token context window.

Beam targets software engineering, multi-step tool use, and mathematical and scientific reasoning, with an emphasis on token efficiency. It takes text input and produces text output, supports configurable reasoning effort, function calling and structured outputs, and is released under Apache 2.0.

## Providers

Different companies host the same model. OpenRouter routes your request to one of them based on the routing mode you pick: Balanced (price + speed), Nitro (fastest), or Floor (cheapest).

|  | $0.30 | $1.00 | $0.04 | 0.28s | 472 tok/s |  | 
|  | $0.40 | $1.20 | $0.04 | 0.20s | 252 tok/s |  | 
|  | $0.25 | $1.00 | $0.05 | 0.74s | 189 tok/s |  | 
|  | $0.40 | $1.20 | $0.04 | 0.34s | 138 tok/s |  | 
|  | $0.40 | $1.20 | $0.04 | 0.44s | 221 tok/s |  | 

  - $0.30
  - $1.00
  - $0.04
  - 0.28s
  - 472 tok/s
  - 98.47%
  - $0.40
  - $1.20
  - $0.04
  - 0.20s
  - 252 tok/s
  - 99.33%
  - $0.25
  - $1.00
  - $0.05
  - 0.74s
  - 189 tok/s
  - 99.52%
  - $0.40
  - $1.20
  - $0.04
  - 0.34s
  - 138 tok/s
  - 99.79%
  - $0.40
  - $1.20
  - $0.04
  - 0.44s
  - 221 tok/s
  - 100.00%

## Pricing

The average price customers actually pay for this model, next to the prices providers post. Caching and discounts mean the price actually paid is often well below the listed one.

### Weighted average

### Beam 1.0 501B A23B — Price history

|  |  | $0.0486 | $1.00 | 96.7% | 60.0% | 
|---|---|---|---|---|---|
|  |  | $0.0526 | $1.00 | 98.7% | 22.4% | 
|  |  | $0.0552 | $1.20 | 95.8% | 16.2% | 
|  |  | $0.0997 | $1.20 | 83.4% | 1.0% | 
|  |  | $0.1030 | $1.20 | 82.5% | 0.5% | 

- Effective in $0.0486 / Effective out $1.00Token share60.0%
- Effective in $0.0526 / Effective out $1.00Token share22.4%
- Effective in $0.0552 / Effective out $1.20Token share16.2%
- Effective in $0.0997 / Effective out $1.20Token share1.0%
- Effective in $0.1030 / Effective out $1.20Token share0.5%

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

| Benchmark score summary for Reflection: Beam 1.0 501B A23B (Artificial Analysis, Design Arena and Vals AI) |  |  | 
|---|---|---|
| Source | Benchmark | Score | 
|---|---|---|
| Artificial Analysis | Beam 1.0 Intelligence Index | 17.7 | 
| Artificial Analysis | Beam 1.0 Finance & Accounting Index | 30.2 | 
| Artificial Analysis | Beam 1.0 Strategy & Ops Index | 34.5 | 
| Artificial Analysis | Beam 1.0 Legal Index | 28.1 | 
| Artificial Analysis | Beam 1.0 Healthcare & Medical Index | 30.7 | 
| Artificial Analysis | Beam 1.0 Engineering Index | 25.2 | 
| Artificial Analysis | Beam 1.0 Economics Index | 27.1 | 
| Artificial Analysis | Beam 1.0 Cyber Index | 33.7 | 
| Artificial Analysis | Beam 1.0 Openness Index | 38.9 | 
| Artificial Analysis | Beam 1.0 AutomationBench-AA | 48.5% | 
| Artificial Analysis | Beam 1.0 GDP.pdf | 11.6% | 
| Artificial Analysis | Beam 1.0 HLE | 34.7% | 
| Artificial Analysis | Beam 1.0 AA-LCR | 78.0% | 
| Artificial Analysis | Beam 1.0 GDPval-AA | 33.9% | 
| Artificial Analysis | Beam 1.0 CritPt | 13.1% | 
| Artificial Analysis | Beam 1.0 HLE-Diamond | 12.8% | 
| Artificial Analysis | Beam 1.0 SciCode | 48.5% | 
| Artificial Analysis | Beam 1.0 Terminal-Bench 4.0 | 3.0% | 
| Artificial Analysis | Beam 1.0 FrontierCode-AA 2.0 | 5.5% | 
| Artificial Analysis | Beam 1.0 FrontierCode 1.1 | 24.3% | 
| Artificial Analysis | Beam 1.0 Terminal-Bench-Science | 0.5% | 
| Artificial Analysis | Beam 1.0 CritPt v2 (Hard Subset) | 26.5% | 
| Artificial Analysis | Beam 1.0 CritPt v2 | 40.0% | 
| Artificial Analysis | Beam 1.0 AA-Omniscience Accuracy | 30.7% | 
| Artificial Analysis | Beam 1.0 AA-Omniscience Non-Hallucination Rate | 59.0% | 
| Artificial Analysis | Beam 1.0 CWE-Bench-AA | 49.2% | 
| Artificial Analysis | Beam 1.0 DeepsecBench-AA | 6.9% | 
| Artificial Analysis | Beam 1.0 CyberGym-E2E-AA | 45.0% | 
| Artificial Analysis | Beam 1.0 MLCR-AA | 17.8% | 
| Design Arena | Beam 1.0 (max) Agents Arena Pdf Elo | 1162 | 
| Design Arena | Beam 1.0 (max) Models Arena 3D Elo | 1108 | 
| Design Arena | Beam 1.0 (max) Models Arena Code Categories Elo | 1215 | 
| Design Arena | Beam 1.0 (max) Models Arena Game Development Elo | 1174 | 
| Design Arena | Beam 1.0 (max) Models Arena UI Component Elo | 1268 | 
| Design Arena | Beam 1.0 (max) Models Arena Website Elo | 1231 | 
| Vals AI | Beam 1.0 Autoform-Hard | 0.0% | 
| Vals AI | Beam 1.0 BioMysteryBench | 42.6% | 
| Vals AI | Beam 1.0 Code Migration v1.1 | 22.8% | 
| Vals AI | Beam 1.0 EMB v1.1 | 64.7% | 
| Vals AI | Beam 1.0 Finance Agent v2.1 | 51.8% | 
| Vals AI | Beam 1.0 Vals Finance Index | 46.5% | 
| Vals AI | Beam 1.0 HLAB-Vals-1.1 | 4.6% | 
| Vals AI | Beam 1.0 IOI | 22.8% | 
| Vals AI | Beam 1.0 Vals Legal Index | 61.8% | 
| Vals AI | Beam 1.0 Legal Research Bench | 28.8% | 
| Vals AI | Beam 1.0 MysteryMechanism | 22.1% | 
| Vals AI | Beam 1.0 ProofBench v1.1 | 18.0% | 
| Vals AI | Beam 1.0 Vals Science Index | 22.0% | 
| Vals AI | Beam 1.0 Tax Agent Bench | 8.8% | 
| Vals AI | Beam 1.0 Terminal-Bench 4.0 | 4.0% | 
| Vals AI | Beam 1.0 Terminal-Bench-Science | 1.4% | 
| Vals AI | Beam 1.0 Vals Index | 43.4% | 
| Vals AI | Beam 1.0 Vibe Code Bench v1.1 | 66.4% | 

## Apps

Public apps that send the most traffic to this model. Good signal for what real production workloads look like — and a hint at which use cases this model is best suited for.

## Activity

Token volume and request traffic to this model over time.

## API

Drop-in code to call this model. OpenRouter's API is OpenAI-compatible — most SDKs work by just swapping the base URL. The only thing that changes between models is the model slug below.

Not currently available through EU in-region routing. In-region routing docs

## Frequently asked questions

Beam 1.0 is Reflection's open-weight reasoning model for coding and long-horizon agentic workflows. It uses a sparse mixture-of-experts architecture with 501B total and 23B active parameters per token, and a 1M-token context window. Beam targets software engineering, multi-step tool use, and mathematical and scientific reasoning, with an emphasis on token efficiency.

Beam 1.0 501B A23B costs $0.25/M input tokens and $1.00/M output tokens, with separate rates for Cache Read at $0.05/M tokens.

Beam 1.0 501B A23B has a 1,048,576 token context window. It supports up to 131,072 completion tokens.

Yes. Beam 1.0 501B A23B accepts `tools` and `tool_choice` for function calling on 4 of the 5 providers serving it, and requests that send tools are routed to those providers. It also supports structured outputs via a JSON schema in `response_format`.

Beam 1.0 501B A23B is served by 5 providers on OpenRouter: Nebius Token Factory, CoreWeave, DigitalOcean, Baseten, and Together. Requests are routed to the best available provider, with automatic failover to the others, and you can pin or exclude providers with provider routing.

Beam 1.0 501B A23B was released on October 16, 2026. Its knowledge cutoff is June 30, 2026.
