---
id: collect-261121-veille-ia/gemini-google/gemini-4-hydrogen-batch-api-pricing-benchmarks
title: "Gemini 4 Hydrogen (batch) - API Pricing & Benchmarks"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["gemini", "hydrogen", "artificial", "analysis", "high", "index", "model", "batch"]
source: https://openrouter.ai/google/gemini-4-hydrogen:batch
source_anchor: ""
source_lines: [1, 170]
section: "Gemini_Google"
---
Gemini 4 Hydrogen is the high-efficiency workhorse model of the Gemini 4 family from Google, built for high-throughput agentic workflows, coding and multi-model routing at lower cost and latency than Gemini 4 Carbon. It takes text, image, audio, video and file input, supports long decode of up to 1M output tokens through continuation, and controls thinking depth with low, medium, high and xhigh reasoning effort.

## Providers

Different companies host the same model. OpenRouter routes your request to one of them based on the routing mode you pick: Balanced (price + speed), Nitro (fastest), or Floor (cheapest).

|  | $0.75 | $3.75 | $0.0375 | $0.0375 | -- | -- |  | 

  - $0.75
  - $3.75
  - $0.0375
  - $0.0375
  - --
  - --
  - --

## Pricing

The average price customers actually pay for this model, next to the prices providers post. Caching and discounts mean the price actually paid is often well below the listed one.

### Gemini 4 Hydrogen (batch) — Price history

|  |  | $0.75 | $3.75 | $0.0375 | $0.083333 | — | 
|---|---|---|---|---|---|---|

- Input $0.75 / Output $3.75

## Performance

Throughput is how fast the model writes (tokens per second — higher is better). Latency is total round-trip time (lower is better). TTFT is time-to-first-token — how long before you see anything appear (lower is better).

## Uptime

Uptime is the percentage of the past 3 days that at least one provider was responding to requests. Availability is the percentage of time that inference was successfully served. OpenRouter continuously monitors and uses the next-best provider when one returns an error.

Not enough uptime data to display yet.

When an error occurs in an upstream provider, we can recover by routing to another healthy provider, if your request filters allow it. You can access per-provider uptime data programmatically through the Endpoints API. Learn more about our load balancing and provider routing options.

## Benchmarks

Scores on standardized evaluations and this model’s own outputs on shared benchmark prompts. Higher percentages are better — and each rank shows where this model lands among the models evaluated on that source.

| Benchmark score summary for Google: Gemini 4 Hydrogen (batch) (Artificial Analysis, Design Arena and Vals AI) |  |  | 
|---|---|---|
| Source | Benchmark | Score | 
|---|---|---|
| Artificial Analysis | Gemini 4 Hydrogen (High) Intelligence Index | 50.8 | 
| Artificial Analysis | Gemini 4 Hydrogen (High) Finance & Accounting Index | 56.1 | 
| Artificial Analysis | Gemini 4 Hydrogen (High) Strategy & Ops Index | 55.4 | 
| Artificial Analysis | Gemini 4 Hydrogen (High) Legal Index | 54.8 | 
| Artificial Analysis | Gemini 4 Hydrogen (High) Healthcare & Medical Index | 52.4 | 
| Artificial Analysis | Gemini 4 Hydrogen (High) Engineering Index | 57.0 | 
| Artificial Analysis | Gemini 4 Hydrogen (High) Economics Index | 53.0 | 
| Artificial Analysis | Gemini 4 Hydrogen (High) AutomationBench-AA | 74.4% | 
| Artificial Analysis | Gemini 4 Hydrogen (High) GDP.pdf | 30.0% | 
| Artificial Analysis | Gemini 4 Hydrogen (High) HLE-Diamond | 54.9% | 
| Artificial Analysis | Gemini 4 Hydrogen (High) Terminal-Bench 4.0 | 54.0% | 
| Artificial Analysis | Gemini 4 Hydrogen (High) FrontierCode-AA 2.0 | 36.3% | 
| Artificial Analysis | Gemini 4 Hydrogen (High) Terminal-Bench-Science | 41.4% | 
| Artificial Analysis | Gemini 4 Hydrogen (High) CritPt v2 (Hard Subset) | 61.5% | 
| Artificial Analysis | Gemini 4 Hydrogen (High) CritPt v2 | 78.0% | 
| Artificial Analysis | Gemini 4 Hydrogen (High) AA-Omniscience Accuracy | 48.2% | 
| Artificial Analysis | Gemini 4 Hydrogen (High) AA-Omniscience Non-Hallucination Rate | 91.3% | 
| Artificial Analysis | Gemini 4 Hydrogen (High) MLCR-AA | 27.2% | 
| Artificial Analysis | Gemini 4 Hydrogen (Low) Intelligence Index | 26.9 | 
| Artificial Analysis | Gemini 4 Hydrogen (Low) Finance & Accounting Index | 41.2 | 
| Artificial Analysis | Gemini 4 Hydrogen (Low) Strategy & Ops Index | 41.7 | 
| Artificial Analysis | Gemini 4 Hydrogen (Low) Legal Index | 42.4 | 
| Artificial Analysis | Gemini 4 Hydrogen (Low) Healthcare & Medical Index | 39.0 | 
| Artificial Analysis | Gemini 4 Hydrogen (Low) Engineering Index | 36.3 | 
| Artificial Analysis | Gemini 4 Hydrogen (Low) Economics Index | 37.9 | 
| Artificial Analysis | Gemini 4 Hydrogen (Low) AutomationBench-AA | 60.5% | 
| Artificial Analysis | Gemini 4 Hydrogen (Low) GDP.pdf | 28.2% | 
| Artificial Analysis | Gemini 4 Hydrogen (Low) HLE-Diamond | 32.2% | 
| Artificial Analysis | Gemini 4 Hydrogen (Low) Terminal-Bench 4.0 | 6.6% | 
| Artificial Analysis | Gemini 4 Hydrogen (Low) FrontierCode-AA 2.0 | 9.3% | 
| Artificial Analysis | Gemini 4 Hydrogen (Low) Terminal-Bench-Science | 2.4% | 
| Artificial Analysis | Gemini 4 Hydrogen (Low) CritPt v2 (Hard Subset) | 48.0% | 
| Artificial Analysis | Gemini 4 Hydrogen (Low) CritPt v2 | 66.0% | 
| Artificial Analysis | Gemini 4 Hydrogen (Low) AA-Omniscience Accuracy | 46.2% | 
| Artificial Analysis | Gemini 4 Hydrogen (Low) AA-Omniscience Non-Hallucination Rate | 81.2% | 
| Artificial Analysis | Gemini 4 Hydrogen (Low) MLCR-AA | 22.2% | 
| Artificial Analysis | Gemini 4 Hydrogen (Medium) Intelligence Index | 41.2 | 
| Artificial Analysis | Gemini 4 Hydrogen (Medium) Finance & Accounting Index | 51.4 | 
| Artificial Analysis | Gemini 4 Hydrogen (Medium) Strategy & Ops Index | 51.8 | 
| Artificial Analysis | Gemini 4 Hydrogen (Medium) Legal Index | 51.4 | 
| Artificial Analysis | Gemini 4 Hydrogen (Medium) Healthcare & Medical Index | 47.2 | 
| Artificial Analysis | Gemini 4 Hydrogen (Medium) Engineering Index | 49.1 | 
| Artificial Analysis | Gemini 4 Hydrogen (Medium) Economics Index | 48.4 | 
| Artificial Analysis | Gemini 4 Hydrogen (Medium) AutomationBench-AA | 72.0% | 
| Artificial Analysis | Gemini 4 Hydrogen (Medium) GDP.pdf | 32.8% | 
| Artificial Analysis | Gemini 4 Hydrogen (Medium) HLE-Diamond | 47.7% | 
| Artificial Analysis | Gemini 4 Hydrogen (Medium) Terminal-Bench 4.0 | 29.3% | 
| Artificial Analysis | Gemini 4 Hydrogen (Medium) FrontierCode-AA 2.0 | 31.5% | 
| Artificial Analysis | Gemini 4 Hydrogen (Medium) Terminal-Bench-Science | 15.2% | 
| Artificial Analysis | Gemini 4 Hydrogen (Medium) CritPt v2 (Hard Subset) | 61.0% | 
| Artificial Analysis | Gemini 4 Hydrogen (Medium) CritPt v2 | 77.7% | 
| Artificial Analysis | Gemini 4 Hydrogen (Medium) AA-Omniscience Accuracy | 48.3% | 
| Artificial Analysis | Gemini 4 Hydrogen (Medium) AA-Omniscience Non-Hallucination Rate | 87.5% | 
| Artificial Analysis | Gemini 4 Hydrogen (Medium) MLCR-AA | 20.0% | 
| Artificial Analysis | Gemini 4 Hydrogen (Xhigh) Intelligence Index | 53.9 | 
| Artificial Analysis | Gemini 4 Hydrogen (Xhigh) Finance & Accounting Index | 58.1 | 
| Artificial Analysis | Gemini 4 Hydrogen (Xhigh) Strategy & Ops Index | 57.7 | 
| Artificial Analysis | Gemini 4 Hydrogen (Xhigh) Legal Index | 56.7 | 
| Artificial Analysis | Gemini 4 Hydrogen (Xhigh) Healthcare & Medical Index | 53.3 | 
| Artificial Analysis | Gemini 4 Hydrogen (Xhigh) Engineering Index | 60.1 | 
| Artificial Analysis | Gemini 4 Hydrogen (Xhigh) Economics Index | 55.0 | 
| Artificial Analysis | Gemini 4 Hydrogen (Xhigh) AutomationBench-AA | 76.1% | 
| Artificial Analysis | Gemini 4 Hydrogen (Xhigh) GDP.pdf | 27.6% | 
| Artificial Analysis | Gemini 4 Hydrogen (Xhigh) HLE-Diamond | 58.8% | 
| Artificial Analysis | Gemini 4 Hydrogen (Xhigh) Terminal-Bench 4.0 | 53.5% | 
| Artificial Analysis | Gemini 4 Hydrogen (Xhigh) FrontierCode-AA 2.0 | 39.0% | 
| Artificial Analysis | Gemini 4 Hydrogen (Xhigh) Terminal-Bench-Science | 49.5% | 
| Artificial Analysis | Gemini 4 Hydrogen (Xhigh) CritPt v2 (Hard Subset) | 66.0% | 
| Artificial Analysis | Gemini 4 Hydrogen (Xhigh) CritPt v2 | 80.6% | 
| Artificial Analysis | Gemini 4 Hydrogen (Xhigh) AA-Omniscience Accuracy | 50.5% | 
| Artificial Analysis | Gemini 4 Hydrogen (Xhigh) AA-Omniscience Non-Hallucination Rate | 91.5% | 
| Artificial Analysis | Gemini 4 Hydrogen (Xhigh) MLCR-AA | 21.1% | 
| Design Arena | Gemini 4 Hydrogen Agents Arena Pdf Elo | 1216 | 
| Design Arena | Gemini 4 Hydrogen Models Arena 3D Elo | 1394 | 
| Design Arena | Gemini 4 Hydrogen Models Arena Asciiart Elo | 1456 | 
| Design Arena | Gemini 4 Hydrogen Models Arena Code Categories Elo | 1335 | 
| Design Arena | Gemini 4 Hydrogen Models Arena Data Visualization Elo | 1307 | 
| Design Arena | Gemini 4 Hydrogen Models Arena Game Development Elo | 1340 | 
| Design Arena | Gemini 4 Hydrogen Models Arena UI Component Elo | 1338 | 
| Design Arena | Gemini 4 Hydrogen Models Arena Website Elo | 1330 | 
| Vals AI | Gemini 4 Hydrogen Autoform-Hard | 64.3% | 
| Vals AI | Gemini 4 Hydrogen EMB v1.1 | 75.8% | 
| Vals AI | Gemini 4 Hydrogen Finance Agent v2.1 | 65.1% | 
| Vals AI | Gemini 4 Hydrogen Vals Finance Index | 66.1% | 
| Vals AI | Gemini 4 Hydrogen HLAB-Vals-1.1 | 11.3% | 
| Vals AI | Gemini 4 Hydrogen Vals Legal Index | 73.6% | 
| Vals AI | Gemini 4 Hydrogen Legal Research Bench | 53.8% | 
| Vals AI | Gemini 4 Hydrogen Tax Agent Bench | 52.3% | 
| Vals AI | Gemini 4 Hydrogen Terminal-Bench 4.0 | 54.0% | 
| Vals AI | Gemini 4 Hydrogen Vals Index | 68.5% | 
| Vals AI | Gemini 4 Hydrogen Vibe Code Bench v1.1 | 87.8% | 

## Apps

Public apps that send the most traffic to this model. Good signal for what real production workloads look like — and a hint at which use cases this model is best suited for.

Not enough data for apps using this model yet.

## Activity

Token volume and request traffic to this model over time.

## API

Drop-in code to call this model. OpenRouter's API is OpenAI-compatible — most SDKs work by just swapping the base URL. The only thing that changes between models is the model slug below.

Not currently available through EU in-region routing. In-region routing docs

## Frequently asked questions

Gemini 4 Hydrogen is the high-efficiency workhorse model of the Gemini 4 family from Google, built for high-throughput agentic workflows, coding and multi-model routing at lower cost and latency than Gemini 4 Carbon.

Gemini 4 Hydrogen (batch) costs $0.75/M input tokens and $3.75/M output tokens, with separate rates for Cache Read at $0.0375/M tokens, Cache Write at $0.08333/M tokens, Image Input at $0.75/M tokens, Input Audio at $0.75/M tokens, Input Audio Cache at $0.0375/M tokens, and Web Search at $14.00/1K calls.

Gemini 4 Hydrogen (batch) has a 1,048,576 token context window. It supports up to 1,048,576 completion tokens.

Yes. Gemini 4 Hydrogen (batch) accepts `tools` and `tool_choice` for function calling. It also supports structured outputs via a JSON schema in `response_format`.

Gemini 4 Hydrogen (batch) accepts audio, files such as PDFs, images, text, and video as input and returns text.

Gemini 4 Hydrogen (batch) is served by 1 provider on OpenRouter: Google Vertex.

Gemini 4 Hydrogen (batch) was released on November 12, 2026.
