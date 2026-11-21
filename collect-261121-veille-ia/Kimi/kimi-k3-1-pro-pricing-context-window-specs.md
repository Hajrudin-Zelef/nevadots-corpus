---
id: collect-261121-veille-ia/kimi/kimi-k3-1-pro-pricing-context-window-specs
title: "Kimi K3.1 Pro: pricing, context window & specs"
domain: kimi
role: reference
task: article
actors: ["Moonshot AI"]
dates: []
keywords: ["models", "kimi-k3", "kimi", "moonshotai", "context", "tokens", "window", "output"]
source: https://www.aimodel.directory/models/kimi-k3-1-pro
source_anchor: ""
source_lines: [1, 125]
section: "Kimi"
---
## Summary

Kimi K3.1 Pro is an open-weight language model from Moonshot AI, released on 23 Oct 2026. It accepts text, images and video and generates text, with a 1M-token context window and up to 1M output tokens per response. As of 20 Nov 2026, the lowest listed API price is $0.25 per million input tokens and $8.80 per million output tokens (OpenRouter), across 11 providers we track.

Multimodal Kimi model with a 1M-token context window and always-on reasoning, for coding, 3D creation and financial research

Last verified 20 Nov 2026. Every figure on this page links to its source.

## Specifications

| Developer | Moonshot AI | 
|---|---|
| Release date | 23 Oct 2026 [models.dev] | 
| Status | Generally available [models.dev] | 
| Weights | Open weights [Hugging Face] | 
| License | kimi-k3.1-pro [Hugging Face] | 
| Parameters | 2.8T [Hugging Face] | 
| Context window | 1M (1,048,576 tokens) [models.dev] | 
| Max output | 1M tokens [models.dev] | 
| Input | Text, images and video [models.dev] | 
| Output | Text [models.dev] | 
| Reasoning mode | Yes [models.dev] | 
| Tool calling | Yes [models.dev] | 
| Structured output | Yes [models.dev] | 
| Hugging Face | moonshotai/Kimi-K3.1-Pro [Hugging Face] | 

## Kimi K3.1 Pro API pricing by provider

USD per million tokens. Sorted by input price.

| Provider | Input | Output | Cached input | Context | Source | 
|---|---|---|---|---|---|
| OpenRouter moonshotai/kimi-k3.1-pro | $0.25 | $8.80 | $0.18 | 1M | OpenRouter, 20 Nov 2026 | 
| Moonshot AI API kimi-k3.1-pro | $2 | $10 | $0.20 | 1M | models.dev, 20 Nov 2026 | 
| DeepInfra moonshotai/Kimi-K3.1-Pro | $2 | $10 | $0.20 | 1M | models.dev, 20 Nov 2026 | 
| Fireworks AI accounts/fireworks/models/kimi-k3p1-pro | $2 | $10 | $0.20 | 1M | models.dev, 20 Nov 2026 | 
| Hugging Face Inference Providers moonshotai/Kimi-K3.1-Pro | $2 | $10 | – | 1M | models.dev, 20 Nov 2026 | 
| Together AI moonshotai/Kimi-K3.1-Pro | $2 | $10 | $0.20 | 1M | models.dev, 20 Nov 2026 | 
| Nebius AI Studio moonshotai/Kimi-K3.1-Pro | $2 | $10 | $0.20 | 1M | models.dev, 20 Nov 2026 | 
| Novita AI moonshotai/kimi-k3.1-pro | $2 | $10 | $0.20 | 1M | models.dev, 20 Nov 2026 | 
| Baseten moonshotai/Kimi-K3.1-Pro | $2 | $10 | $0.20 | 1M | models.dev, 20 Nov 2026 | 
| Vercel AI Gateway moonshotai/kimi-k3.1-pro | $2 | $10 | $0.20 | 1M | models.dev, 20 Nov 2026 | 
| Moonshot AI API kimi-k3.1-pro-highspeed | $4 | $20 | $0.20 | 1M | models.dev, 20 Nov 2026 | 
| Amazon Bedrock global.moonshotai.kimi-k3.1 | $4 | $20 | $0.20 | 1M | models.dev, 20 Nov 2026 | 
| Novita AI moonshotai/kimi-k3.1-pro-highspeed | $4 | $20 | $0.20 | 1M | models.dev, 20 Nov 2026 | 
| Amazon Bedrock us.moonshotai.kimi-k3.1 | $4.40 | $22 | $0.22 | 1M | models.dev, 20 Nov 2026 | 

## Running Kimi K3.1 Pro locally

Estimated memory, from the parameter count and the architecture in config.json on Hugging Face. Includes about 0.8 GB of runtime overhead.

| Quantization | Weights | Total, 8K context | Total, 32K context | 
|---|---|---|---|
| Q4_K_M (4-bit) | 1685 GB | 1687 GB | 1690 GB | 
| Q8_0 (8-bit) | 2954 GB | 2956 GB | 2958 GB | 
| FP16 / BF16 (16-bit) | 5560 GB | 5562 GB | 5565 GB | 

At 4-bit with an 8K conversation, Kimi K3.1 Pro needs about 1687 GB, more than any single consumer graphics card; it needs a unified-memory machine, a multi-GPU setup or a split between graphics card and system RAM. Check your own hardware, or see Best computer for local AI on our sister site AIPCs.

## Frequently asked questions

### What is the context window of Kimi K3.1 Pro?

Kimi K3.1 Pro has a context window of 1,048,576 tokens (1M) and can generate up to 1,048,576 tokens in a single response, according to models.dev.

### How much does the Kimi K3.1 Pro API cost?

Through Moonshot AI API, Kimi K3.1 Pro costs $2 per million input tokens and $10 per million output tokens. The lowest listed price is $0.25 input / $8.80 output through OpenRouter. Prices last verified 20 Nov 2026.

### Is Kimi K3.1 Pro open source?

Kimi K3.1 Pro is an open-weight model: its weights are published on Hugging Face as moonshotai/Kimi-K3.1-Pro under the kimi-k3.1-pro license. Check the license terms before commercial use.

### When was Kimi K3.1 Pro released?

Moonshot AI released Kimi K3.1 Pro on 23 Oct 2026, according to models.dev.

### Which providers offer Kimi K3.1 Pro?

We track Kimi K3.1 Pro on 11 providers: OpenRouter, Moonshot AI API, DeepInfra, Fireworks AI, Hugging Face Inference Providers, Together AI, Nebius AI Studio, Novita AI, Baseten, Vercel AI Gateway, Amazon Bedrock.

### How much VRAM does Kimi K3.1 Pro need?

About 1687 GB at 4-bit (Q4_K_M) with an 8K-token conversation, and about 2956 GB at 8-bit. That is more than any single consumer graphics card holds. These are estimates from the model's published parameter count and architecture.

## Similar models from other labs

Comparable on output type, weights, release date, price and context window. Prices are the lowest listed, USD per million tokens.

| Model | Lab | Released | Context | Input $/1M | Output $/1M | Weights | 
|---|---|---|---|---|---|---|
| GPT-6.1 Terra Pro | OpenAI | 26 Oct 2026 | 1.1M | $1 | $5 | – | 
| GPT-6.1 Sol Pro | OpenAI | 29 Sept 2026 | 1.1M | $2 | $10 | – | 
| GLM 5.3 Prime | Zhipu AI (Z.ai) | 23 Sept 2026 | 1M | $1.68 | $5.28 | – | 
| Muse Spark Open | Meta | 15 Oct 2026 | 1M | $1.25 | $4.25 | Open | 
| MiMo-V2.6-Pro-UltraSpeed | Xiaomi (MiMo) | 21 Sept 2026 | 1M | $4.35 | $8.70 | Open | 
| ERNIE-Light-DT-1.0 | Baidu | 10 Nov 2026 | 1M | $1.13 | $3.94 | – | 

## Change history

- OpenRouter price changed: input $0.24 → $0.25, output $8.40 → $8.80 per 1M tokens
- Name changed from Kimi K3.1 Pro Highspeed to Kimi K3.1 Pro
- Name changed from Kimi K3.1 Pro to Kimi K3.1 Pro Highspeed
- OpenRouter price changed: input $2 → $0.24, output $10 → $8.40 per 1M tokens
- Now listed on Hugging Face Inference Providers
- Now listed on Nebius AI Studio
- Name changed from Kimi K3.1 Pro Highspeed to Kimi K3.1 Pro
- OpenRouter price changed: input $0.24 → $2, output $8.79 → $10 per 1M tokens
- Name changed from Kimi K3.1 Pro to Kimi K3.1 Pro Highspeed
- OpenRouter price changed: input $2 → $0.24, output $10 → $8.79 per 1M tokens
- Name changed from Kimi K3.1 Pro Highspeed to Kimi K3.1 Pro
- Name changed from Kimi K3.1 Pro to Kimi K3.1 Pro Highspeed
- Now listed on Amazon Bedrock
- Now listed on Amazon Bedrock
- Now listed on DeepInfra
- Name changed from Kimi K3.1 Pro Highspeed to Kimi K3.1 Pro
- Now listed on Fireworks AI
- Name changed from Kimi K3.1 Pro to Kimi K3.1 Pro Highspeed
- Name changed from Kimi K3.1 Pro Highspeed to Kimi K3.1 Pro
- Open weights changed from no to yes
- Parameter count changed from none to 2.8T
- License changed from none to kimi-k3.1-pro
- Now listed on Baseten
- Now listed on Together AI
- Name changed from Kimi K3.1 Pro to Kimi K3.1 Pro Highspeed
