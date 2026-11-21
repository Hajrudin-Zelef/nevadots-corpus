---
id: collect-261121-veille-ia/glm-zhipu/glm-5-3-flashx-api-pricing-and-benchmarks
title: "GLM 5.3 FlashX API: Pricing and Benchmarks"
domain: glm-zhipu
role: reference
task: article
actors: ["Zhipu AI"]
dates: ["2026-09-18"]
keywords: ["flashx", "tokens", "glm-5", "output", "input", "zhipu", "model", "flash"]
source: https://aimlapi.com/models/glm-5-3-flashx
source_anchor: ""
source_lines: [1, 210]
section: "GLM_Zhipu"
---
## GLM 5.3 FlashX overview

### GLM 5.3 FlashX by Zhipu AI

GLM-5.3 FlashX is the high-speed variant of Zhipu AI GLM-5.3 Flash, a native multimodal model reaching up to 200 tokens/s on the same hybrid sparse and linear attention architecture, with a 1M-token context window and text, image and video understanding.

### GLM 5.3 FlashX specs

- Model ID
- `z-ai/glm-5.3-flashx`
- Developer
- Zhipu AI
- Context window
- 1,048,576 tokens
- Max output
- 131,072 tokens
- Input
- Image, Text
- Output
- Text
- API
- OpenAI-compatible, base URL `https://api.aimlapi.com/v1`

### What Is GLM 5.3 FlashX API?

GLM-5.3 FlashX is the high-speed variant of Zhipu AI's GLM-5.3 Flash. It keeps the same hybrid sparse-plus-linear attention architecture and the same 1,048,576-token context window, but is tuned for throughput, Zhipu reports up to 200 tokens per second.

It sits in the GLM-5.3 family alongside GLM-5.3 and GLM-5.3-Flash, trading some of the flagship's depth for speed and a lower price.

### Technical Reference

- **Aliases:** glm-5.3-flashx, zhipu/glm-5.3-flashx
- **Release date:** September 18, 2026
- **Context window:** 1,048,576 tokens
- **Max output tokens:** 131,072 tokens
- **Modalities:** Image, Text → Text
- **Capabilities:** reasoning, parallel tool calls, structured output, streaming, vision, file input
- **Endpoint:** POST /v1/chat/completions

### What GLM 5.3 FlashX Brings to the Table

**Throughput first**

Up to 200 tokens per second, vendor-reported, on the same architecture as GLM-5.3 Flash. Built for paths where latency is the constraint.

**A million tokens of context**

1,048,576 tokens in, up to 131,072 back, enough for whole repositories or long transcripts without chunking.

**Agentic plumbing**

Parallel tool calling, structured (JSON schema) output and streaming, which is what agent loops need to plan, call tools and iterate.

### Where GLM 5.3 FlashX Makes the Most Sense

**Latency-bound chat**

Interactive assistants where time-to-answer matters more than the last increment of quality.

**High-volume pipelines**

Classification, extraction and summarisation run continuously rather than occasionally.

**Long-context work**

The 1M window removes most of the need for retrieval scaffolding.

**Agents with tools**

Parallel tool calling and structured output for multi-step workflows.

### Use cases

- Latency-sensitive chat and assistants
- High-volume classification, extraction and summarisation
- Long-context analysis over 1M-token inputs
- Agentic workflows with parallel tool calling and structured output
- Document and screenshot understanding via vision and file input

### Capabilities

- Reasoning
- Streaming
- Structured outputs
- Function calling
- Vision
- File input

## GLM 5.3 FlashX API pricing

| GLM 5.3 FlashX pricing in US dollars |  |  | 
|---|---|---|
| Usage | Price | Unit | 
|---|---|---|
| Input | $0.37 | 1M tokens | 
| Output | $1.25 | 1M tokens | 
| Cached input | $0.075 | 1M tokens | 

Prices from the AI/ML API price list, checked on Nov 4, 2026.

### GLM 5.3 FlashX vs other models: token prices

AI/ML API input and output rates for 5 models, per million tokens.

| AI/ML API model prices in USD per million tokens |  |  | 
|---|---|---|
| Model |  |  | 
|---|---|---|
| GLM 5.3 FlashX | $0.37 | $1.25 | 
| GLM-5.3-Flash | $0.12 | $0.4 | 
| GLM-5.3 | $1.4 | $4.4 | 
| GLM 5.2 | $1.4 | $4.4 | 
| GLM 5.2 Fast Preview | $3.5 | $11 | 

## How to use the GLM 5.3 FlashX API

### Build with your agent

Give your agent the model details, code and documentation.

`export AIMLAPI_KEY="8f2b4c7d9a1e6b3f…"`
### 2Make your first request

```
curl 'https://api.aimlapi.com/v1/chat/completions' \
  -H "Authorization: Bearer $AIMLAPI_KEY" \
  -H 'Content-Type: application/json' \
  -d '{
  "model": "z-ai/glm-5.3-flashx",
  "messages": [
    {
      "role": "user",
      "content": "Write a haiku about the ocean."
    }
  ]
}'
```
```
import os
import requests
headers = {
    "Authorization": f"Bearer {os.environ['AIMLAPI_KEY']}",
    "Content-Type": "application/json",
}
payload = {
    "model": "z-ai/glm-5.3-flashx",
    "messages": [
        {
            "role": "user",
            "content": "Write a haiku about the ocean."
        }
    ]
}
response = requests.post("https://api.aimlapi.com/v1/chat/completions", headers=headers, json=payload, timeout=120)
response.raise_for_status()
print(response.json())
```
```
const response = await fetch("https://api.aimlapi.com/v1/chat/completions", {
  method: "POST",
  headers: {
    Authorization: `Bearer ${process.env.AIMLAPI_KEY}`,
    "Content-Type": "application/json",
  },
  body: JSON.stringify({
    "model": "z-ai/glm-5.3-flashx",
    "messages": [
      {
        "role": "user",
        "content": "Write a haiku about the ocean."
      }
    ]
  }),
});
if (!response.ok) throw new Error(`HTTP ${response.status}`);
console.log(await response.json());
```
## GLM 5.3 FlashX FAQ

## What is the context window of GLM 5.3 FlashX?

GLM 5.3 FlashX has a 1,048,576 token context window and can return up to 131,072 tokens.

## How is GLM 5.3 FlashX different from GLM-5.3 Flash?

FlashX is the high-speed variant. It runs on the same hybrid sparse-plus-linear attention architecture and the same 1M-token context window as GLM-5.3 Flash, but is tuned for throughput, Zhipu reports up to 200 tokens per second.

## How much does GLM 5.3 FlashX cost?

GLM 5.3 FlashX costs $0.37 input and $1.25 output per 1M tokens on AI/ML API, billed as you go with no subscription.

## What inputs and outputs does GLM 5.3 FlashX support?

It accepts images and text as input and returns text. File input is supported as well.

## How fast is GLM 5.3 FlashX?

Zhipu AI reports up to 200 tokens per second. That figure is vendor-reported and has not been independently reproduced.

## Who created GLM 5.3 FlashX?

GLM 5.3 FlashX was built by Zhipu AI as part of the GLM-5.3 family, alongside GLM-5.3 and GLM-5.3 Flash. It became available on September 18, 2026.

## How do I call GLM 5.3 FlashX through AI/ML API?

It is served on the OpenAI-compatible chat completions endpoint. Point any OpenAI SDK at https://api.aimlapi.com/v1 and set the model to `z-ai/glm-5.3-flashx`. The aliases `glm-5.3-flashx` and `zhipu/glm-5.3-flashx` also resolve to it.

## What is the GLM 5.3 FlashX model ID?

Use `z-ai/glm-5.3-flashx` as the model parameter. Requests go to the OpenAI-compatible endpoint with your AI/ML API key.
