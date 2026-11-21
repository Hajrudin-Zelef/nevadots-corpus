---
id: collect-261121-veille-ia/gemini-google/gemini-3-7-flash-api-pricing-benchmarks
title: "Gemini 3.7 Flash - API Pricing & Benchmarks"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["effective", "used", "routing", "gemini", "flash", "endpoints", "provider", "model"]
source: https://openrouter.ai/google/gemini-3.7-flash?view=api
source_anchor: ""
source_lines: [1, 233]
section: "Gemini_Google"
---
Gemini 3.7 Flash is a multimodal model from Google for fast agentic workflows, coding, and complex multi-step reasoning. It is designed for tasks that require responsive performance and reliable multi-step problem solving.

## Providers

Different companies host the same model. OpenRouter routes your request to one of them based on the routing mode you pick: Balanced (price + speed), Nitro (fastest), or Floor (cheapest).

| 50% off | $1.50$0.750 | $7.50$3.750 | $0.15$0.0750 | $0.15$0.0750 | 2.20s | 97 tok/s |  | 
| 50% off | $1.50$0.750 | $7.50$3.750 | $0.15$0.0750 | $0.15$0.0750 | 2.69s | 73 tok/s |  | 
| Not used in Standard routing:Why these endpoints are not used |  |  |  |  |  |  |  | 
| Flex50% off | $0.75$0.375 | $3.75$1.875 | $0.075$0.0375 | $0.075$0.0375 | 2.44s | 65 tok/s |  | 
| 50% off | $1.65$0.825 | $8.25$4.125 | $0.165$0.0825 | $0.165$0.0825 | 1.97s | 94 tok/s |  | 
| Priority50% off | $2.70$1.350 | $13.50$6.750 | $0.27$0.1350 | $0.27$0.1350 | 2.03s | 119 tok/s |  | 
| Priority50% off | $2.70$1.350 | $13.50$6.750 | $0.27$0.1350 | $0.27$0.1350 | 2.10s | 61 tok/s |  | 
| Flex50% off | $0.75$0.375 | $3.75$1.875 | $0.075$0.0375 | $0.075$0.0375 | 12.09s | 41 tok/s |  | 

- 50% off
  - $0.75
  - $3.75
  - $0.075
  - $0.075
  - 2.20s
  - 97 tok/s
  - 100.00%
- 50% off
  - $0.75
  - $3.75
  - $0.075
  - $0.075
  - 2.69s
  - 73 tok/s
  - 100.00%
- Not used in Standard routing:Why these endpoints are not used
- 50% offFlex
  - $0.375
  - $1.875
  - $0.0375
  - $0.0375
  - 2.44s
  - 65 tok/s
  - 100.00%
- 50% off
  - $0.825
  - $4.125
  - $0.0825
  - $0.0825
  - 1.97s
  - 94 tok/s
  - 100.00%
- 50% offPriority
  - $1.35
  - $6.75
  - $0.135
  - $0.135
  - 2.03s
  - 119 tok/s
  - 99.92%
- 50% offPriority
  - $1.35
  - $6.75
  - $0.135
  - $0.135
  - 2.10s
  - 61 tok/s
  - 100.00%
- 50% offFlex
  - $0.375
  - $1.875
  - $0.0375
  - $0.0375
  - 12.09s
  - 41 tok/s
  - 55.17%

## Pricing

The average price customers actually pay for this model, next to the prices providers post. Caching and discounts mean the price actually paid is often well below the listed one.

### Weighted average

### Gemini 3.7 Flash — Price history

|  |  | $0.417 | $4.10 | 56.3% | 14.1% | 
|---|---|---|---|---|---|
|  |  | $0.465 | $3.98 | 46.3% | 72.9% | 
| Not used in Standard routing:Why these endpoints are not used |  |  |  |  |  | 
|  | Flex | $0.180 | $1.87 | 59.0% | 10.1% | 
|  | Flex | $0.219 | $1.90 | 48.4% | 2.0% | 
|  | Priority | $0.416 | $9.28 | 78.5% | 0.1% | 
|  |  | $1.005 | $8.19 | 44.3% | 0.2% | 
|  | Priority | $1.075 | $8.79 | 31.9% | 0.7% | 

- Effective in $0.417 / Effective out $4.10Token share14.1%
- Effective in $0.465 / Effective out $3.98Token share72.9%
- Not used in Standard routing:Why these endpoints are not used
- Effective in $0.180 / Effective out $1.87FlexToken share10.1%
- Effective in $0.219 / Effective out $1.90FlexToken share2.0%
- Effective in $0.416 / Effective out $9.28PriorityToken share0.1%
- Effective in $1.005 / Effective out $8.19Token share0.2%
- Effective in $1.075 / Effective out $8.79PriorityToken share0.7%

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

| Benchmark score summary for Google: Gemini 3.7 Flash (Artificial Analysis, Design Arena and Vals AI) |  |  | 
|---|---|---|
| Source | Benchmark | Score | 
|---|---|---|
| Artificial Analysis | Gemini 3.7 Flash (High) AutomationBench-AA | 62.0% | 
| Artificial Analysis | Gemini 3.7 Flash (High) GDP.pdf | 23.6% | 
| Artificial Analysis | Gemini 3.7 Flash (High) AA-AnalystAgent | 60.0% | 
| Artificial Analysis | Gemini 3.7 Flash (High) GPQA Diamond | 94.5% | 
| Artificial Analysis | Gemini 3.7 Flash (High) HLE | 47.9% | 
| Artificial Analysis | Gemini 3.7 Flash (High) AA-LCR | 81.7% | 
| Artificial Analysis | Gemini 3.7 Flash (High) τ³-Banking | 32.8% | 
| Artificial Analysis | Gemini 3.7 Flash (High) GDPval-AA | 45.2% | 
| Artificial Analysis | Gemini 3.7 Flash (High) CritPt | 14.3% | 
| Artificial Analysis | Gemini 3.7 Flash (High) SciCode | 57.2% | 
| Artificial Analysis | Gemini 3.7 Flash (High) Terminal-Bench 4.0 | 13.6% | 
| Artificial Analysis | Gemini 3.7 Flash (High) Terminal-Bench 2.1 | 85.8% | 
| Artificial Analysis | Gemini 3.7 Flash (High) MLCR-AA | 12.2% | 
| Artificial Analysis | Gemini 3.7 Flash (High) MMMU-Pro | 85.5% | 
| Artificial Analysis | Gemini 3.7 Flash (Low) GPQA Diamond | 90.1% | 
| Artificial Analysis | Gemini 3.7 Flash (Low) HLE | 35.1% | 
| Artificial Analysis | Gemini 3.7 Flash (Low) AA-LCR | 78.7% | 
| Artificial Analysis | Gemini 3.7 Flash (Low) τ³-Banking | 29.5% | 
| Artificial Analysis | Gemini 3.7 Flash (Low) GDPval-AA | 41.4% | 
| Artificial Analysis | Gemini 3.7 Flash (Low) CritPt | 5.7% | 
| Artificial Analysis | Gemini 3.7 Flash (Low) SciCode | 55.7% | 
| Artificial Analysis | Gemini 3.7 Flash (Low) Terminal-Bench 2.1 | 79.8% | 
| Artificial Analysis | Gemini 3.7 Flash (Low) MMMU-Pro | 84.9% | 
| Artificial Analysis | Gemini 3.7 Flash (Medium) EnterpriseOps-Gym-AA | 50.4% | 
| Artificial Analysis | Gemini 3.7 Flash (Medium) GPQA Diamond | 92.1% | 
| Artificial Analysis | Gemini 3.7 Flash (Medium) HLE | 39.0% | 
| Artificial Analysis | Gemini 3.7 Flash (Medium) AA-LCR | 83.0% | 
| Artificial Analysis | Gemini 3.7 Flash (Medium) τ³-Banking | 35.5% | 
| Artificial Analysis | Gemini 3.7 Flash (Medium) GDPval-AA | 43.3% | 
| Artificial Analysis | Gemini 3.7 Flash (Medium) CritPt | 9.4% | 
| Artificial Analysis | Gemini 3.7 Flash (Medium) SciCode | 59.8% | 
| Artificial Analysis | Gemini 3.7 Flash (Medium) Terminal-Bench 2.1 | 78.3% | 
| Artificial Analysis | Gemini 3.7 Flash (Medium) MMMU-Pro | 84.7% | 
| Design Arena | Gemini 3.7 Flash Agents Arena Agenticgamedev Elo | 1181 | 
| Design Arena | Gemini 3.7 Flash Agents Arena Androidnative Elo | 1231 | 
| Design Arena | Gemini 3.7 Flash Agents Arena Blender3d Elo | 1112 | 
| Design Arena | Gemini 3.7 Flash Agents Arena Codetovideo Elo | 1188 | 
| Design Arena | Gemini 3.7 Flash Agents Arena Full Stack Elo | 1176 | 
| Design Arena | Gemini 3.7 Flash Agents Arena Godotgamedev Elo | 1226 | 
| Design Arena | Gemini 3.7 Flash Agents Arena Mobile Apps Elo | 1224 | 
| Design Arena | Gemini 3.7 Flash Agents Arena Pdf Elo | 1150 | 
| Design Arena | Gemini 3.7 Flash Agents Arena Python-Pptxslides Elo | 1055 | 
| Design Arena | Gemini 3.7 Flash Agents Arena Spreadsheet Elo | 1020 | 
| Design Arena | Gemini 3.7 Flash Agents Arena Webapps Elo | 1202 | 
| Design Arena | Gemini 3.7 Flash Models Arena 3D Elo | 1275 | 
| Design Arena | Gemini 3.7 Flash Models Arena Asciiart Elo | 1258 | 
| Design Arena | Gemini 3.7 Flash Models Arena Code Categories Elo | 1292 | 
| Design Arena | Gemini 3.7 Flash Models Arena Data Visualization Elo | 1306 | 
| Design Arena | Gemini 3.7 Flash Models Arena Game Development Elo | 1301 | 
| Design Arena | Gemini 3.7 Flash Models Arena SVG Elo | 1271 | 
| Design Arena | Gemini 3.7 Flash Models Arena UI Component Elo | 1282 | 
| Design Arena | Gemini 3.7 Flash Models Arena Website Elo | 1291 | 
| Vals AI | Gemini 3.7 Flash Code Migration | 34.8% | 
| Vals AI | Gemini 3.7 Flash CyberBench v1.1 | 43.8% | 
| Vals AI | Gemini 3.7 Flash EMB | 71.3% | 
| Vals AI | Gemini 3.7 Flash Finance Agent (v2) | 59.0% | 
| Vals AI | Gemini 3.7 Flash GPQA Diamond | 93.9% | 
| Vals AI | Gemini 3.7 Flash Harvey's Legal Agent Benchmark v1 | 8.8% | 
| Vals AI | Gemini 3.7 Flash IOI | 67.8% | 
| Vals AI | Gemini 3.7 Flash LiveCodeBench | 88.7% | 
| Vals AI | Gemini 3.7 Flash LegalBench | 87.3% | 
| Vals AI | Gemini 3.7 Flash Legal Research Bench | 34.6% | 
| Vals AI | Gemini 3.7 Flash MedCode | 53.4% | 
| Vals AI | Gemini 3.7 Flash MedScribe | 83.9% | 
| Vals AI | Gemini 3.7 Flash MMLU Pro | 90.1% | 
| Vals AI | Gemini 3.7 Flash MMMU Pro | 89.0% | 
| Vals AI | Gemini 3.7 Flash MortgageTax | 66.7% | 
| Vals AI | Gemini 3.7 Flash ProgramBench | 0.0% | 
| Vals AI | Gemini 3.7 Flash ProofBench v1.1 | 58.0% | 
| Vals AI | Gemini 3.7 Flash Vals RSI Index | 18.3% | 
| Vals AI | Gemini 3.7 Flash SAGE v1 | 49.2% | 
| Vals AI | Gemini 3.7 Flash SkillsBench | 65.9% | 
| Vals AI | Gemini 3.7 Flash SRE Bench | 4.6% | 
| Vals AI | Gemini 3.7 Flash SWE-bench | 80.8% | 
| Vals AI | Gemini 3.7 Flash Tax Agent Bench | 22.8% | 
| Vals AI | Gemini 3.7 Flash TaxEval v2 | 74.7% | 
| Vals AI | Gemini 3.7 Flash Terminal-Bench 4.0 | 12.1% | 
| Vals AI | Gemini 3.7 Flash Terminal-Bench-Science | 7.1% | 
| Vals AI | Gemini 3.7 Flash Vals Index | 51.3% | 
| Vals AI | Gemini 3.7 Flash Vibe Code Bench v1.1 | 70.4% | 

## Apps

Public apps that send the most traffic to this model. Good signal for what real production workloads look like — and a hint at which use cases this model is best suited for.

## Activity

Token volume and request traffic to this model over time.

## API

Drop-in code to call this model. OpenRouter's API is OpenAI-compatible — most SDKs work by just swapping the base URL. The only thing that changes between models is the model slug below.

Not currently available through EU in-region routing. In-region routing docs

## Frequently asked questions

Gemini 3.7 Flash is a multimodal model from Google for fast agentic workflows, coding, and complex multi-step reasoning. It is designed for tasks that require responsive performance and reliable multi-step problem solving.

Gemini 3.7 Flash costs $0.75/M input tokens and $3.75/M output tokens, with separate rates for Cache Read at $0.075/M tokens, Cache Write at $0.04167/M tokens, Image Input at $0.75/M tokens, Input Audio at $0.75/M tokens, Input Audio Cache at $0.075/M tokens, and Web Search at $14.00/1K calls.

Gemini 3.7 Flash has a 1,048,576 token context window. It supports up to 65,536 completion tokens.

Yes. Gemini 3.7 Flash accepts `tools` and `tool_choice` for function calling. It also supports structured outputs via a JSON schema in `response_format`.

Gemini 3.7 Flash accepts text, images, video, files such as PDFs, and audio as input and returns text.

Gemini 3.7 Flash is served by 2 providers on OpenRouter: Google Vertex and Google AI Studio. Requests are routed to the best available provider, with automatic failover to the others, and you can pin or exclude providers with provider routing.

Gemini 3.7 Flash was released on August 13, 2026.
