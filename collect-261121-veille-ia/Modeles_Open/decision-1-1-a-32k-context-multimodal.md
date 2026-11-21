---
id: collect-261121-veille-ia/modeles-open/decision-1-1-a-32k-context-multimodal
title: "Decision 1.1 â 32K context, multimodal"
domain: modeles-open
role: reference
task: article
actors: []
dates: []
keywords: ["gateway", "model", "foundry", "decision", "microsoft", "available", "routing", "routes"]
source: https://www.llmreference.com/model/microsoft-decision-1.1
source_anchor: ""
source_lines: [1, 54]
section: "Modeles_Open"
---
# Decision 1.1

Cheapest of 3 routes Â· Microsoft Foundry

Cheapest of 3 routes Â· Microsoft Foundry

## About

Decision 1.1 is Microsoftâs decision-scoring model, generally available in Foundry. It evaluates text, JSON and up to eight images against typed questions, returning numerical decisions rather than generated explanations. It supports classification, routing, ranking, grading and yes/no judgments. Microsoft post-trained Gemma 4 26B-A4B for the model. Inputs can contain up to 32K tokens.

Compare API pricing across 3 tracked provider routes for input and output tokens, batch, and cached reads when available.

| Provider | Input / 1M | Output / 1M | Route | 
|---|---|---|---|
| Microsoft Foundry | $0.042 | Free | Serverless | 
| OpenRouter | $0.042 | Free | Serverless | 
| Vercel AI Gateway | $0.042 | Free | Serverless | 

## Available via routers & gateways(5)

### LiteLLM

Gateway
Open-source Python SDK and proxy server that unifies 100+ LLM APIs behind a single OpenAI-compatible interface, with load balancing, cost tracking, and configurable failover.

### Portkey

Gateway
Production AI gateway routing to 1,600+ LLMs with failover, load balancing, caching, and guardrails; MIT-licensed open-source core is self-hostable. Now part of Palo Alto Networks as Prisma AIRS AI Gateway.

### Azure AI Foundry Model Router

Router
Microsoft Azure AI Foundry's native model router that uses a trained ML model to route each prompt in real time to the optimal Azure-hosted model, with Balanced/Cost/Quality mode selection and automatic failover.

### Helicone

Gateway
Observability-first AI gateway with routing, caching, rate limiting, and request tracing; Apache-2.0 open-source platform with a managed hosted tier for logging and analytics.

### Kong AI Gateway

Gateway
Multi-LLM AI gateway built on Kong Gateway 3.x, adding semantic routing, load balancing, guardrails, and MCP traffic analytics as plugins over Kong's existing API management platform.

## Capabilities

## Benchmark peer barsfor Agents

No task-mapped benchmark peers are available for this model yet.

## Migration checks

No linked migration route is available for this model yet.
