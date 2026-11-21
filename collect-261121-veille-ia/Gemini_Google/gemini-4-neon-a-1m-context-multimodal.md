---
id: collect-261121-veille-ia/unifi-ubiquiti/gemini-4-neon-a-1m-context-multimodal
title: "Gemini 4 Neon â 1M context, multimodal"
domain: unifi-ubiquiti
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["gateway", "gemini", "google", "neon", "model", "read", "serverless", "available"]
source: https://www.llmreference.com/model/gemini-4-neon
source_anchor: ""
source_lines: [1, 74]
section: "Gemini_Google"
---
# Gemini 4 Neon

Cheapest of 5 routes Â· Google AI Studio Â· cache read $0.150

## About

Gemini 4 Neon is Google DeepMind's Gemini 4 model, released generally available on November 18, 2026 in the Gemini API, Google AI Studio, and Vertex AI (Gemini Enterprise Agent Platform). Google positions it for complex software engineering, knowledge work, and science workflows. The model card says it is adapted from Gemini 4 Carbon. It takes text, image, video, audio and PDF input and returns text, with a unified 1,048,576-token context window and long decode up to 1,048,576 output tokens (65,536 by default).

Compare API pricing across 5 tracked provider routes for input and output tokens, batch, and cached reads when available.

| Provider | Input / 1M | Output / 1M | Batch in / out | Cache | Route | 
|---|---|---|---|---|---|
| Google AI Studio | $3.00 | $15.00 | $1.50 / $7.50 | read $0.150 | Serverless | 
| GCP Vertex AI | $3.00 | $15.00 | $1.50 / $7.50 | read $0.150 | Serverless | 
| OpenRouter | $3.00 | $15.00 | - | read $0.150 | Serverless | 
| Vercel AI Gateway | $3.00 | $15.00 | - | read $0.150 | Serverless | 
| Venice AI | $3.75 | $18.75 | - | read $0.1875 | Serverless | 

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
|  | 46.2 | FrontierCode 2.0 Main scoreObserved 2026-11-18 | Configuration: gemini-4-neon (high) Harness: Official public leaderboard; Google's evals methodology says Gemini 4 Neon results use high thinking Evaluator: Cognition Confidence: self-reported Notes: Google DeepMind model card results table (as of November 2026): FrontierCode 2.0 Main score 46.2%. Evals methodology: sourced from the official public leaderboard; Neon uses high thinking. The Cognition board was not independently recaptured in this packet. Timestamp is retrieval date. |  | 
|  | 56.3 | AutomationBenchObserved 2026-11-18 | Configuration: Gemini 4 Neon Harness: Official public leaderboard (Zapier), as stated in Google's evals methodology Evaluator: Zapier Confidence: self-reported Notes: Google DeepMind model card results table (as of November 2026): AutomationBench 56.3%. Evals methodology: sourced from the official public leaderboard. The owner board was not independently recaptured in this packet. Timestamp is retrieval date. |  | 
|  | 75.8 | Agents' Last Exam v1.1Observed 2026-11-18 | Configuration: Agents' Last Exam v1.1; Antigravity-CLI 1.4.8; 122-task; 5-hour window; safety filters enabled; averaged rubric score Harness: Self-computed on the 122-task v1.1 benchmark using the Antigravity-CLI version 1.4.8 harness through official integration and a 5-hour window, with safety filters enabled; averaged rubric score Evaluator: Google DeepMind Confidence: self-reported Notes: Google DeepMind model card results table (as of November 2026): Agents' Last Exam v1.1 75.8%. Evals methodology records the self-computed harness above. Timestamp is retrieval date. |  | 
|  | 70.5 | Terminal-Bench-Science 0.1Observed 2026-11-18 | Configuration: Gemini 4 Neon; mini-swe-agent vision; average of 5 attempts; Average Reward Harness: Self-computed using a mini-swe-agent vision harness, averaged over 5 attempts per task (Average Reward) Evaluator: Google DeepMind Confidence: self-reported Notes: Google DeepMind model card results table (as of November 2026): Terminal-Bench-Science 0.1 70.5%. Evals methodology matches this harness. Timestamp is retrieval date. |  | 
|  | 90.5 | SEC-bench ProObserved 2026-11-18 | Configuration: Pass@1; 183-task May 2026 JavaScript engine split; average of 3 independent attempts; Antigravity harness Harness: Self-computed using the open source evaluation setup; Pass@1 on the 183-task May 2026 JavaScript engine split, averaged over 3 independent attempts per task, using the Antigravity harness Evaluator: Google DeepMind Confidence: self-reported Notes: Google DeepMind model card results table (as of November 2026): SEC-bench Pro 90.5%. Evals methodology records the self-computed protocol above. Timestamp is retrieval date. |  | 
|  | 73.5 | ChartographyObserved 2026-11-18 | Configuration: Gemini 4 Neon; self-computed without tools Harness: Self-computed without tools following the official Surge public leaderboard Evaluator: Google DeepMind Confidence: self-reported Notes: Google DeepMind model card results table (as of November 2026): Chartography 73.5%. Evals methodology: Neon results are self-computed without tools. Timestamp is retrieval date. |  | 

## Migration checks

No linked migration route is available for this model yet.

## API versions

`gemini-4-neon``google/gemini-4-neon`
