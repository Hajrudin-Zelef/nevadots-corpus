---
id: collect-261121-veille-ia/xiaomi-mimo/mimo-v2-6-pro-api-pricing-benchmarks
title: "MiMo-V2.6-Pro - API Pricing & Benchmarks"
domain: xiaomi-mimo
role: reference
task: article
actors: ["Xiaomi"]
dates: ["2026-09-21"]
keywords: ["mimo-v2", "effective", "artificial", "analysis", "index", "model", "token", "provider"]
source: https://openrouter.ai/xiaomi/mimo-v2.6-pro
source_anchor: ""
source_lines: [1, 189]
section: "Xiaomi_MiMo"
---
MiMo-V2.6-Pro is the flagship foundation model developed by Xiaomi. Built at a scale of over 1T parameters, it is designed to push the ceiling of capability for the most demanding workloads. The model features a 1M-token context window and native multimodal capabilities. Optimized for agentic workflows, it delivers top-tier performance across coding, visual, general, and research scenarios, excelling at complex, long-horizon tasks with robust generalization across a diverse range of agent harnesses.

## Providers

Different companies host the same model. OpenRouter routes your request to one of them based on the routing mode you pick: Balanced (price + speed), Nitro (fastest), or Floor (cheapest).

|  | $0.4350 | $0.8700 | $0.00360 | 3.42s | 28 tok/s |  | 
| 10% off | $0.43$0.3870 | $0.87$0.7830 | $0.0036$0.00324 | 1.13s | 26 tok/s |  | 
|  | $2.6400 | $5.2800 | $0.02400 | 1.36s | 165 tok/s |  | 
|  | $0.4350 | $0.8700 | $0.00360 | 2.36s | 32 tok/s |  | 
| 25% off | $0.435$0.3263 | $0.87$0.6525 | $0.004$0.00300 | 4.59s | 21 tok/s |  | 

  - $0.435
  - $0.87
  - $0.0036
  - 3.42s
  - 28 tok/s
  - 99.97%
  - $0.387
  - $0.783
  - $0.00324
  - 1.13s
  - 26 tok/s
  - 100.00%
  - $2.64
  - $5.28
  - $0.024
  - 1.36s
  - 165 tok/s
  - 98.70%
  - $0.435
  - $0.87
  - $0.0036
  - 2.36s
  - 32 tok/s
  - 97.58%
  - $0.3263
  - $0.6525
  - $0.003
  - 4.59s
  - 21 tok/s
  - 96.36%

## Pricing

The average price customers actually pay for this model, next to the prices providers post. Caching and discounts mean the price actually paid is often well below the listed one.

### Weighted average

### MiMo-V2.6-Pro — Price history

|  |  | $0.0537 | $0.870 | 88.4% | 72.7% | 
|---|---|---|---|---|---|
|  |  | $0.0631 | $0.869 | 86.2% | 2.3% | 
|  |  | $0.0956 | $0.783 | 75.9% | 17.2% | 
|  |  | $0.1095 | $0.652 | 67.0% | 1.6% | 
|  |  | $0.1705 | $5.280 | 94.4% | 6.2% | 

- Effective in $0.0537 / Effective out $0.870Token share72.7%
- Effective in $0.0631 / Effective out $0.869Token share2.3%
- Effective in $0.0956 / Effective out $0.783Token share17.2%
- Effective in $0.1095 / Effective out $0.652Token share1.6%
- Effective in $0.1705 / Effective out $5.280Token share6.2%

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

| Benchmark score summary for Xiaomi: MiMo-V2.6-Pro (Artificial Analysis, Design Arena and Vals AI) |  |  | 
|---|---|---|
| Source | Benchmark | Score | 
|---|---|---|
| Artificial Analysis | MiMo-V2.6-Pro Intelligence Index | 29.9 | 
| Artificial Analysis | MiMo-V2.6-Pro Finance & Accounting Index | 38.9 | 
| Artificial Analysis | MiMo-V2.6-Pro Strategy & Ops Index | 46.9 | 
| Artificial Analysis | MiMo-V2.6-Pro Legal Index | 35.0 | 
| Artificial Analysis | MiMo-V2.6-Pro Healthcare & Medical Index | 35.8 | 
| Artificial Analysis | MiMo-V2.6-Pro Engineering Index | 34.3 | 
| Artificial Analysis | MiMo-V2.6-Pro Economics Index | 36.1 | 
| Artificial Analysis | MiMo-V2.6-Pro Cyber Index | 56.1 | 
| Artificial Analysis | MiMo-V2.6-Pro Openness Index | 52.8 | 
| Artificial Analysis | MiMo-V2.6-Pro AutomationBench-AA | 58.6% | 
| Artificial Analysis | MiMo-V2.6-Pro GDP.pdf | 19.2% | 
| Artificial Analysis | MiMo-V2.6-Pro AA-AnalystAgent | 37.5% | 
| Artificial Analysis | MiMo-V2.6-Pro HLE | 49.4% | 
| Artificial Analysis | MiMo-V2.6-Pro AA-LCR | 86.3% | 
| Artificial Analysis | MiMo-V2.6-Pro GDPval-AA | 59.6% | 
| Artificial Analysis | MiMo-V2.6-Pro CritPt | 26.6% | 
| Artificial Analysis | MiMo-V2.6-Pro HLE-Diamond | 15.2% | 
| Artificial Analysis | MiMo-V2.6-Pro SciCode | 60.9% | 
| Artificial Analysis | MiMo-V2.6-Pro Terminal-Bench 4.0 | 34.8% | 
| Artificial Analysis | MiMo-V2.6-Pro FrontierCode-AA 2.0 | 24.0% | 
| Artificial Analysis | MiMo-V2.6-Pro FrontierCode 1.1 | 45.4% | 
| Artificial Analysis | MiMo-V2.6-Pro Terminal-Bench-Science | 5.7% | 
| Artificial Analysis | MiMo-V2.6-Pro CritPt v2 (Hard Subset) | 34.0% | 
| Artificial Analysis | MiMo-V2.6-Pro CritPt v2 | 56.9% | 
| Artificial Analysis | MiMo-V2.6-Pro AA-Omniscience Accuracy | 34.8% | 
| Artificial Analysis | MiMo-V2.6-Pro AA-Omniscience Non-Hallucination Rate | 39.8% | 
| Artificial Analysis | MiMo-V2.6-Pro CWE-Bench-AA | 63.3% | 
| Artificial Analysis | MiMo-V2.6-Pro DeepsecBench-AA | 26.2% | 
| Artificial Analysis | MiMo-V2.6-Pro CyberGym-E2E-AA | 78.6% | 
| Artificial Analysis | MiMo-V2.6-Pro MLCR-AA | 12.8% | 
| Artificial Analysis | MiMo-V2.6-Pro VIALS-AA | 43.5% | 
| Design Arena | MiMo-V2.6-Pro Agents Arena Webapps Elo | 1265 | 
| Design Arena | MiMo-V2.6-Pro Models Arena 3D Elo | 1308 | 
| Design Arena | MiMo-V2.6-Pro Models Arena Code Categories Elo | 1309 | 
| Design Arena | MiMo-V2.6-Pro Models Arena Data Visualization Elo | 1335 | 
| Design Arena | MiMo-V2.6-Pro Models Arena Game Development Elo | 1302 | 
| Design Arena | MiMo-V2.6-Pro Models Arena SVG Elo | 1282 | 
| Design Arena | MiMo-V2.6-Pro Models Arena UI Component Elo | 1329 | 
| Design Arena | MiMo-V2.6-Pro Models Arena Website Elo | 1306 | 
| Vals AI | MiMo V2.6 Pro Access to Justice Benchmark | 70.0% | 
| Vals AI | MiMo V2.6 Pro Code Migration v1.1 | 48.8% | 
| Vals AI | MiMo V2.6 Pro CyberBench v1.1 | 72.9% | 
| Vals AI | MiMo V2.6 Pro EMB v1.1 | 67.3% | 
| Vals AI | MiMo V2.6 Pro Finance Agent v2.1 | 64.3% | 
| Vals AI | MiMo V2.6 Pro Vals Finance Index | 57.8% | 
| Vals AI | MiMo V2.6 Pro Financial Coaching Bench | 75.4% | 
| Vals AI | MiMo V2.6 Pro HLAB-Vals-1.1 | 13.8% | 
| Vals AI | MiMo V2.6 Pro Immigration | 67.5% | 
| Vals AI | MiMo V2.6 Pro IOI | 39.3% | 
| Vals AI | MiMo V2.6 Pro Vals Legal Index | 72.1% | 
| Vals AI | MiMo V2.6 Pro Legal Research Bench | 47.1% | 
| Vals AI | MiMo V2.6 Pro MedCode | 45.0% | 
| Vals AI | MiMo V2.6 Pro MedScribe | 88.3% | 
| Vals AI | MiMo V2.6 Pro MysteryMechanism | 15.3% | 
| Vals AI | MiMo V2.6 Pro ProgramBench | 0.5% | 
| Vals AI | MiMo V2.6 Pro ProofBench v1.1 | 70.0% | 
| Vals AI | MiMo V2.6 Pro Public Benefits Bench v1.1 | 68.9% | 
| Vals AI | MiMo V2.6 Pro SAGE | 84.4% | 
| Vals AI | MiMo V2.6 Pro SRE Bench | 3.1% | 
| Vals AI | MiMo V2.6 Pro SWE-bench Multimodal | 41.3% | 
| Vals AI | MiMo V2.6 Pro Tax Agent Bench | 32.4% | 
| Vals AI | MiMo V2.6 Pro Terminal-Bench 4.0 | 31.3% | 
| Vals AI | MiMo V2.6 Pro Terminal-Bench-Science | 2.9% | 
| Vals AI | MiMo V2.6 Pro Vals Index | 57.8% | 
| Vals AI | MiMo V2.6 Pro Vibe Code Bench 1-100 v1.1 | 26.4% | 
| Vals AI | MiMo V2.6 Pro Vibe Code Bench v1.1 | 85.2% | 

## Apps

Public apps that send the most traffic to this model. Good signal for what real production workloads look like — and a hint at which use cases this model is best suited for.

## Activity

Token volume and request traffic to this model over time.

## API

Drop-in code to call this model. OpenRouter's API is OpenAI-compatible — most SDKs work by just swapping the base URL. The only thing that changes between models is the model slug below.

Not currently available through EU in-region routing. In-region routing docs

## Documentation

Guides, cookbooks, blog posts, and tools for building with Xiaomi: MiMo-V2.6-Pro on OpenRouter.

## Frequently asked questions

MiMo-V2.6-Pro is the flagship foundation model developed by Xiaomi. Built at a scale of over 1T parameters, it is designed to push the ceiling of capability for the most demanding workloads. The model features a 1M-token context window and native multimodal capabilities.

The MiMo-V2.6-Pro documentation on OpenRouter includes the Open-Weight vs Closed-Weight Models: What's the Difference? blog post. The quickstart covers API keys and your first request, and the API reference lists every parameter.

MiMo-V2.6-Pro costs $0.3263/M input tokens and $0.6525/M output tokens, with separate rates for Cache Read at $0.003/M tokens.

MiMo-V2.6-Pro has a 1,050,000 token context window.

Yes. MiMo-V2.6-Pro accepts `tools` and `tool_choice` for function calling. It also supports structured outputs via a JSON schema in `response_format`.

MiMo-V2.6-Pro accepts text, images, video, and audio as input and returns text.

MiMo-V2.6-Pro is served by 5 providers on OpenRouter: GMICloud, DeepInfra, StreamLake, Xiaomi, and PrimaLabs. Requests are routed to the best available provider, with automatic failover to the others, and you can pin or exclude providers with provider routing.

MiMo-V2.6-Pro was released on September 21, 2026.
