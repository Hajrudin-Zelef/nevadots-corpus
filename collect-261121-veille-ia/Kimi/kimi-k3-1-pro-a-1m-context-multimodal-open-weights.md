---
id: collect-261121-veille-ia/kimi/kimi-k3-1-pro-a-1m-context-multimodal-open-weights
title: "Kimi K3.1 Pro â 1M context, multimodal, open weights"
domain: kimi
role: reference
task: article
actors: ["Moonshot AI"]
dates: ["2026-10-23"]
keywords: ["reasoning", "effortobserved", "kimi", "context", "read", "weights", "serverless", "harness"]
source: https://www.llmreference.com/model/kimi-k3-1-pro
source_anchor: ""
source_lines: [1, 66]
section: "Kimi"
---
# Kimi K3.1 Pro

- Released
- 2026-10-23
- Context
- 1M
- Parameters
- 2800B total; 104B active
- Architecture
- Mixture of Experts
- Specialization
- general
- Openness
- Open weights
- Code
- Unknown

Cheapest of 6 routes Â· Moonshot AI Kimi Â· cache read $0.200

## About

Kimi K3.1 Pro is the first model in Moonshot AI's K3.1 series, a post-trained version of Kimi K3. It has a 1,048,576-token context window, always-on reasoning with low, high and max effort, image and video input, tool calling and strict JSON Schema structured output. Released 2026-10-23 on Kimi.ai, Kimi Work, Kimi Code and the Kimi API; Moonshot published the weights on Hugging Face under the Kimi K3.1 Pro License.

Compare API pricing across 6 tracked provider routes for input and output tokens, batch, and cached reads when available.

| Provider | Input / 1M | Output / 1M | Cache | Route | 
|---|---|---|---|---|
| Moonshot AI Kimi | $2.00 | $10.00 | read $0.200 / 5m $2.00 / 1h $4.00 | Serverless | 
| Fireworks AI | $2.00 | $10.00 | read $0.200 | Serverless | 
| OpenRouter | $2.00 | $10.00 | read $0.200 | Serverless | 
| Together AI | $2.00 | $10.00 | read $0.200 | Serverless | 
| Vercel AI Gateway | $2.00 | $10.00 | read $0.200 | Serverless | 
| DeepInfra | - | - | - | ServerlessPartial | 

## Available via routers & gateways(1)

## Capabilities

## Benchmark peer barsfor RAG

No task-mapped benchmark peers are available for this model yet.

## Benchmark scores(16)

| Benchmark | Score | Version | Evaluation | Source | 
|---|---|---|---|---|
|  | 65.7 | Terminal-Bench 4.0; Claude-Code harness; max reasoning effortObserved 2026-10-23 | â |  | 
|  | 75.5 | DeepSWE v1.1; Claude-Code harness; max reasoning effortObserved 2026-10-23 | â |  | 
|  | 77.1 | Kimi Code Bench 2.0 (internal); Claude-Code harness; max reasoning effortObserved 2026-10-23 | â |  | 
|  | 70.0 | HLE-Full with tools; max reasoning effortObserved 2026-10-23 | â |  | 
|  | 51.1 | HLE-Full without tools; max reasoning effortObserved 2026-10-23 | â |  | 
|  | 94.2 | diamond; max reasoning effortObserved 2026-10-23 | â |  | 
|  | 92.1 | BrowseComp; context compaction at 200K tokens; max 500 steps; max reasoning effortObserved 2026-10-23 | â |  | 
|  | 43.2 | Agents' Last Exam v1.1 (ALL, 122 tasks); max reasoning effortObserved 2026-10-23 | â |  | 
|  | 79.6 | OSWorld V2.1; Kimi Agent harness; max reasoning effortObserved 2026-10-23 | â |  | 
|  | 89.0 | CharXiv RQ; no tools; max reasoning effortObserved 2026-10-23 | â |  | 
|  | 84.1 | MMMU-Pro; official protocol; no tools; max reasoning effortObserved 2026-10-23 | â |  | 
|  | 34.6 | GDP.pdf vision, no tools; strict pass rate; five-run mean; max reasoning effortObserved 2026-10-23 | â |  | 
|  | 20.7 | ExploitGym; KimiCode harness; 6-hour budget; max reasoning effortObserved 2026-10-23 | â |  | 
|  | 95.4 | BabyVision w/ python; four-run mean; max reasoning effortObserved 2026-10-23 | â |  | 
|  | 90.2 | ChartoGraphy w/ python; five-run mean; max reasoning effortObserved 2026-10-23 | â |  | 
|  | 63.0 | ZeroBench-main w/ python Pass@5; max reasoning effortObserved 2026-10-23 | â |  | 

## Migration checks

No linked migration route is available for this model yet.
