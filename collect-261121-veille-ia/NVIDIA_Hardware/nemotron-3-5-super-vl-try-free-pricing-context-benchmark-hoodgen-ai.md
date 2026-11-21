---
id: collect-261121-veille-ia/nvidia-hardware/nemotron-3-5-super-vl-try-free-pricing-context-benchmark-hoodgen-ai
title: "Nemotron 3.5 Super VL (Try Free) - Pricing, Context & Benchmark | Hoodgen Ai"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: ["2026-10-15"]
keywords: ["pricing", "nemotron", "super", "input", "context", "output", "text", "standard"]
source: https://hoodgen.com/model/nemotron-3-5-super-vl
source_anchor: ""
source_lines: [1, 157]
section: "NVIDIA_Hardware"
---
# Nemotron 3.5 Super VL pricing, context and benchmark

NVIDIA Nemotron 3.5 Super VL is an open multimodal mixture-of-experts model from NVIDIA, with 12B active parameters out of 120B total.

## Everything that matters, in one table

Context, pricing, cache, modalities and tooling, from the model registry and Artificial Analysis.

| Model | Nemotron 3.5 Super VL | 
| Model ID | nvidia/nemotron-3.5-super-vl | 
| Provider | NVIDIA | 
| Release date | 15 October 2026 | 
| Status | Generally available | 
| Input context | 1,048,576 tokens | 
| Output context | 943,718 tokens | 
| Official input pricing | $0.06 per 1M tokens | 
| Official output pricing | $0.35 per 1M tokens | 
| Cache read pricing | $0.049 per 1M tokens (71% off input) | 
| Market input pricing | $0.06 per 1M (cheapest route) | 
| Market output pricing | $0.35 per 1M (cheapest route) | 
| Highest route pricing | $0.15 in, $0.5 out per 1M | 
| Input modalities | text · image · video | 
| Output modalities | text | 
| Reasoning control | Not exposed | 
| Tool support | Text generation only | 
| Tokenizer | Other | 
| Provider routes | 8 endpoints | 
| Artificial Analysis index | 14 (#1 of 4) | 
| Speed | 257 tokens/s, 14.43s to first token | 
| Knowledge cutoff | Not publicly stated | 

### Input and output support

| Modality | Input | Output | Billed at | 
|---|---|---|---|
| Text | ✓ yes | ✓ yes | $0.06 in | 
| Image | ✓ yes | no | $0.06 in | 
| Audio | no | no | not supported | 
| Video | ✓ yes | no | $0.06 in | 
| File (PDF) | no | no | not supported | 

## What is Nemotron 3.5 Super VL?

NVIDIA Nemotron 3.5 Super VL is an open multimodal mixture-of-experts model from NVIDIA, with 12B active parameters out of 120B total.

It takes text, images, documents, GUI screenshots, and video...

Nemotron 3.5 Super VL carries a 1.05M token window, uses the Other tokenizer and is served by NVIDIA. On Hoodgen it sits in the Multimodal tier of the model menu next to its siblings, so you can switch mid conversation without losing the thread.

## Release and what changed from the previous version

| Property | Nemotron 3.5 Lightning | Nemotron 3.5 Super VL | Change | 
|---|---|---|---|
| Released | 2026 | Oct 2026 | Newer release | 
| Context | 262K | 1.05M | Changed | 
| Max output | 236K | 944K | Changed | 
| Input price | $0.025 | $0.06 | Higher | 
| Output price | $0.12 | $0.35 | Higher | 
| Input modalities | text | text, image, video | Unchanged | 
| AA index | 6 | 14 | Newer benchmark run | 

## Features and capabilities

## Benchmark results and performance

Artificial Analysis Intelligence Index v5.0.1 blends nine evaluations. Nemotron 3.5 Super VL scores **14** , ranking **#1 of 4**.

## What can you do with Nemotron 3.5 Super VL?

## Prompt caching and how to cut the bill

Cached input bills at $0.049 per million instead of $0.06, a 71% discount. Keep the system prompt and documents first and the changing question last, so every call hits the cache.

## Nemotron 3.5 Super VL pricing

### Official pricing

- $0.049 cached input
- 1.05M context
- text, image, video input

- 8 routes on OpenRouter
- What Hoodgen routes to first
- Same model, lower bill

- Priority or premium tier
- Best under load
- Use for latency critical calls

### Market pricing, every route

| Provider | Tier | Input | Output | Cache read | Uptime 24h | 
|---|---|---|---|---|---|
| Darkbloom | standard | $0.06 | $0.35 | $0.01 | 100.0% | 
| Io Net | standard | $0.09 | $0.3 | $0.012 | 100.0% | 
| Crusoe | standard | $0.1 | $0.4 | $0.08 | 99.0% | 
| CoreWeave | standard | $0.15 | $0.47 | $0.02 | 97.7% | 
| PrimeIntellect | standard | $0.15 | $0.5 | $0.02 | 100.0% | 
| Together | standard | $0.15 | $0.5 | $0.02 | 98.6% | 
| DeepInfra | standard | $0.15 | $0.5 | $0.02 | 99.8% | 
| Venice | standard | $0.2 | $0.5 | $0.04 | 99.6% | 

### API pricing and cost calculator

| Rate | Per call | Total | Credits | 
|---|---|---|---|
| Cheapest route | $0.0011 | $0.56 | 535 | 
| List price | $0.0016 | $0.80 | 759 | 
| Highest route | $0.0037 | $1.88 | 1782 | 

## API and developer information

| Model ID | nvidia/nemotron-3.5-super-vl | 
| Endpoint | https://openrouter.ai/api/v1/chat/completions | 
| Parameters | prompt and max_tokens | 
| Streaming | Yes, server sent events | 
| Modality string | text+image+video->text | 
| Max prompt tokens | 1,048,576 | 
| Max completion tokens | 943,718 | 

```
curl https://openrouter.ai/api/v1/chat/completions \
  -H "Authorization: Bearer $OPENROUTER_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "nvidia/nemotron-3.5-super-vl",
    "messages": [{ "role": "user", "content": "Summarise this document." }],
    "max_tokens": 1024
  }'
```
## Model limitations

| Text only output | It cannot generate images, audio or video. Pair it with an image model on Hoodgen when you need visuals. | 
| Output cap | 943,718 tokens per request. Long deliverables have to be chunked. | 
| Knowledge cutoff | The provider does not state a fixed cutoff for every release. Verify very recent events against search. | 

## Nemotron 3.5 Super VL vs other NVIDIA models

| Model | Context | Input /1M | Output /1M | AA index | Released | 
|---|---|---|---|---|---|
| Nemotron 3.5 Super VL | 1.05M | $0.06 | $0.35 | 14 | Oct 2026 | 
| Nemotron 3 Nano 30B A3B | 262K | $0.06 | $0.24 | n/a | 2025 | 
| Nemotron 3.5 Lightning | 262K | $0.025 | $0.12 | 6 | 2026 | 
| Nemotron 3.5 Super VL (free) | 1M | Free | Free | n/a | Oct 2026 | 
| Nemotron 3 Nano Omni (free) | 256K | Free | Free | n/a | 2026 | 

## Privacy and data policy

- On Hoodgen, prompts and attachments answer that request and stay in your conversation history so you can reopen it. They are not used to train models.
- Requests route through OpenRouter to NVIDIA and are covered by that provider's API terms.
- Do not paste secrets or personal data you would not send to any hosted model.

## Nemotron 3.5 Super VL questions, answered

## Start chatting with Nemotron 3.5 Super VL

Free to try on Hoodgen AI, with a live credit estimate before every send. No API key, no setup.
