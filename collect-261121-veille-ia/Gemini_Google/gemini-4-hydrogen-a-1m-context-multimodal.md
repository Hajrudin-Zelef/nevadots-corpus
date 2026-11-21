---
id: collect-261121-veille-ia/unifi-ubiquiti/gemini-4-hydrogen-a-1m-context-multimodal
title: "Gemini 4 Hydrogen â 1M context, multimodal"
domain: unifi-ubiquiti
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["gemini", "gateway", "hydrogen", "cursorbench", "task", "tokens", "cursor", "google"]
source: https://www.llmreference.com/model/gemini-4-hydrogen
source_anchor: ""
source_lines: [1, 73]
section: "Gemini_Google"
---
# Gemini 4 Hydrogen

Cheapest of 4 routes Â· Google AI Studio Â· cache read $0.075

## About

Gemini 4 Hydrogen is Google DeepMind's fast, lower-cost Gemini 4 model, released generally available on November 12, 2026 in the Gemini API, Google AI Studio and Vertex AI (Gemini Enterprise Agent Platform). Google positions it for everyday coding, agentic workflows and multimodal work, alongside Gemini 4 Carbon for the hardest problems; its model card says it is adapted from Gemini 4 Carbon. It takes text, image, video, audio and PDF input and returns text, with a unified 1,048,576-token context window and long decode up to 1,048,576 output tokens (65,536 by default).

Compare API pricing across 4 tracked provider routes for input and output tokens, batch, and cached reads when available.

| Provider | Input / 1M | Output / 1M | Batch in / out | Cache | Route | 
|---|---|---|---|---|---|
| Google AI Studio | $1.50 | $7.50 | $0.750 / $3.75 | read $0.075 | Serverless | 
| GCP Vertex AI | $1.50 | $7.50 | $0.750 / $3.75 | read $0.075 | Serverless | 
| OpenRouter | $1.50 | $7.50 | - | read $0.075 | Serverless | 
| Vercel AI Gateway | $1.50 | $7.50 | - | read $0.075 | Serverless | 

## Available via routers & gateways(13)

### LiteLLM

Gateway
Open-source Python SDK and proxy server that unifies 100+ LLM APIs behind a single OpenAI-compatible interface, with load balancing, cost tracking, and configurable failover.

### OpenRouter

Hybrid
Unified hybrid gateway to 500+ models from 80+ providers via a single OpenAI-compatible API, with an optional Auto Router that selects a model per prompt from market spend data.

### Portkey

Gateway
Production AI gateway routing to 1,600+ LLMs with failover, load balancing, caching, and guardrails; MIT-licensed open-source core is self-hostable. Now part of Palo Alto Networks as Prisma AIRS AI Gateway.

### AIRouter

Router
Commercial LLM router (branded "AI Router" on its site) that picks the best model per request via a Decision API or an OpenAI-compatible Chat Completions endpoint, with a private selection mode that routes on anonymized patterns; sold as BYOK subscription plans priced by routed tokens.

### Helicone

Gateway
Observability-first AI gateway with routing, caching, rate limiting, and request tracing; Apache-2.0 open-source platform with a managed hosted tier for logging and analytics.

### Kong AI Gateway

Gateway
Multi-LLM AI gateway built on Kong Gateway 3.x, adding semantic routing, load balancing, guardrails, and MCP traffic analytics as plugins over Kong's existing API management platform.

## Capabilities

## Benchmark peer barsfor Coding

No task-mapped benchmark peers are available for this model yet.

## Benchmark scores(6)

| Benchmark | Score | Version | Evaluation | Source | 
|---|---|---|---|---|
|  | 57.8 | CursorBench 4.0Observed 2026-11-16 | Configuration: Gemini 4 Hydrogen Extra High Harness: CursorBench 4.0 productized Cursor-agent workflow Evaluator: Cursor Confidence: confirmed Cost/task: $5.86 Tokens/task: 164,610 Steps/task: 88 Notes: Cursor vendor-reported CursorBench 4.0 configuration result; verified on https://cursor.com/cursorbench table row 'Gemini 4 Hydrogen Extra High' (retrieved 2026-11-16): 57.8%, $5.86 avg cost per task, 164610 tokens, 88 steps. Not independently reproducible. Results are subject to variance, and small score differences may not be statistically meaningful. Timestamp is retrieval date (page carries no per-row date). |  | 
|  | 54.3 | CursorBench 4.0Observed 2026-11-16 | Configuration: Gemini 4 Hydrogen High Harness: CursorBench 4.0 productized Cursor-agent workflow Evaluator: Cursor Confidence: confirmed Cost/task: $5.59 Tokens/task: 88,682 Steps/task: 59 Notes: Cursor vendor-reported CursorBench 4.0 configuration result; verified on https://cursor.com/cursorbench table row 'Gemini 4 Hydrogen High' (retrieved 2026-11-16): 54.3%, $5.59 avg cost per task, 88682 tokens, 59 steps. Not independently reproducible. Results are subject to variance, and small score differences may not be statistically meaningful. Timestamp is retrieval date (page carries no per-row date). |  | 
|  | 46.7 | CursorBench 4.0Observed 2026-11-16 | Configuration: Gemini 4 Hydrogen Medium Harness: CursorBench 4.0 productized Cursor-agent workflow Evaluator: Cursor Confidence: confirmed Cost/task: $1.76 Tokens/task: 36,781 Steps/task: 38 Notes: Cursor vendor-reported CursorBench 4.0 configuration result; verified on https://cursor.com/cursorbench table row 'Gemini 4 Hydrogen Medium' (retrieved 2026-11-16): 46.7%, $1.76 avg cost per task, 36781 tokens, 38 steps. Not independently reproducible. Results are subject to variance, and small score differences may not be statistically meaningful. Timestamp is retrieval date (page carries no per-row date). |  | 
|  | 27.9 | CursorBench 4.0Observed 2026-11-16 | Configuration: Gemini 4 Hydrogen Low Harness: CursorBench 4.0 productized Cursor-agent workflow Evaluator: Cursor Confidence: confirmed Cost/task: $0.28 Tokens/task: 8,975 Steps/task: 19 Notes: Cursor vendor-reported CursorBench 4.0 configuration result; verified on https://cursor.com/cursorbench table row 'Gemini 4 Hydrogen Low' (retrieved 2026-11-16): 27.9%, $0.28 avg cost per task, 8975 tokens, 19 steps. Not independently reproducible. Results are subject to variance, and small score differences may not be statistically meaningful. Timestamp is retrieval date (page carries no per-row date). |  | 
|  | 66.6 | Chartography â No ToolsObserved 2026-11-16 | Configuration: Gemini 4 Hydrogen; self-computed without tools Harness: Official Surge public leaderboard protocol, without tools Evaluator: Google DeepMind Confidence: self-reported Notes: Google DeepMind self-reported (lab-published). Model card results table + methodology PDF, retrieved 2026-11-16. Self-computed without tools following the official Surge public leaderboard. Run count and grader are not stated on the methodology page. Does not move /best rankings (self-reported). Source https://deepmind.google/models/evals-methodology/gemini-4-hydrogen; evidenceUrl is the model card table. |  | 
|  | 51.4 | Terminal-Bench-Science 0.1 (Average Reward)Observed 2026-11-16 | Configuration: Gemini 4 Hydrogen; mini-swe-agent vision; average of 5 attempts; Average Reward Harness: mini-swe-agent vision harness, averaged over 5 attempts per task Evaluator: Google DeepMind Confidence: self-reported Notes: Google DeepMind self-reported (lab-published). Model card results table + methodology PDF, retrieved 2026-11-16. Self-computed with a mini-swe-agent vision harness, averaged over 5 attempts per task, reporting Average Reward. Does not move /best rankings (self-reported). Source https://deepmind.google/models/evals-methodology/gemini-4-hydrogen; evidenceUrl is the model card table. |  | 

## Migration checks

No linked migration route is available for this model yet.

## API versions

`gemini-4-hydrogen``google/gemini-4-hydrogen`
