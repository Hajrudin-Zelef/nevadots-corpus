---
id: collect-261121-veille-ia/xiaomi-mimo/mimo-v2-6-pro-2
title: "MiMo-V2.6-Pro"
domain: xiaomi-mimo
role: reference
task: article
actors: ["Xiaomi"]
dates: ["2026-09-24"]
keywords: ["score", "does", "arena", "mimo-v2", "chat", "context", "models", "model"]
source: https://freellmarena.com/model/mimo-v2-6-pro
source_anchor: ""
source_lines: [1, 97]
section: "Xiaomi_MiMo"
---
All models › xiaomi

# MiMo-V2.6-Pro

xiaomi · MIT license · on Arena since 24 Sept 2026

**1481**#33 of 426 in Chat, paid models included

**1024.2 B**43.9 B active per token (mixture of experts)

**621 GB**weights only, Q4_K_M

**1024K**tokens, from the model config

## Where to use it for free

- **Run it in one command** with whichllm (free, open source):`uvx whichllm@latest plan "MiMo-V2.6-Pro-RL"` shows the GPU you need,`uvx whichllm@latest run "MiMo-V2.6-Pro-RL gguf"` downloads it and starts a chat
- **Download and run it at home** : official weights · GGUF files for Ollama and LM Studio

## Arena scores

| Arena | Score | Rank | Votes | 
|---|---|---|---|
| Chat | 1481 | #33 of 426 | 4K | 
| Coding | 1622 | #35 of 161 | 10K | 
| Vision | 1247 | #58 of 168 | 2K | 
| Agents | 1.4 agent score | #29 of 68 | 53K sessions | 

Rank among all models, paid ones included. Source: Arena leaderboard dataset. The agent score is on a different scale (×100, from real agent sessions) and cannot be compared with the other scores.

## Speed on popular hardware

| Hardware | GPU memory | Fits? | Speed | 
|---|---|---|---|
| RTX 3060 12 GB | 12 GB | Does not fit | — | 
| RTX 4060 Ti 16 GB | 16 GB | Does not fit | — | 
| RTX 3090 | 24 GB | Does not fit | — | 
| RTX 4090 | 24 GB | Does not fit | — | 
| RTX 5090 | 32 GB | Does not fit | — | 
| RX 7900 XTX | 24 GB | Does not fit | — | 
| 2 × RTX 3090 | 48 GB | Does not fit | — | 
| 4 × RTX 3090 | 96 GB | Does not fit | — | 
| Mac M4 Max 128 GB | 96 GB | Does not fit | — | 
| Ryzen AI Max+ 395 128 GB | 96 GB | Does not fit | — | 
| 8 × H100 SXM | 640 GB | Fits in GPU | 60 tok/s | 

Estimate at 4-bit with 32K context, 64 GB of DDR5 for offload. Speed ≈ memory bandwidth × 78% ÷ data read per token, plus a fixed cost per token, calibrated on 44 real llama.cpp tests; typical error about ±10%. Compare all GPUs.

## GPU memory needed

| Level | 8K context | 32K context | 128K context | Quality vs full | 
|---|---|---|---|---|
| Q3_K_M | 513 GB | 515 GB | 521 GB | +4.7% noticeable loss | 
| Q4_K_M | 622 GB | 623 GB | 629 GB | +1.2% very small loss | 
| Q5_K_M | 729 GB | 731 GB | 737 GB | +0.4% near-lossless | 
| Q6_K | 841 GB | 843 GB | 849 GB | +0.2% near-lossless | 
| Q8_0 | 1089 GB | 1091 GB | 1097 GB | ≈ 0% same as full | 
| F16 | 2050 GB | 2051 GB | 2057 GB | ≈ 0% same as full | 

Weights + context cache (from the model's architecture) + runtime. Quality: increase in perplexity against the full model, from llama.cpp tests, scaled by model size. Simulate your own setup.

## Chat score over time

**1481**

Arena score from 27 May 2026 to 19 Nov 2026.

## Coding score over time

**1622**

Arena score from 25 May 2026 to 19 Nov 2026.

## Vision score over time

**1247**

Arena score from 3 Jun 2026 to 19 Nov 2026.

## Agents score over time

**1.4**

Arena score from 4 Jun 2026 to 18 Nov 2026.

## Similar free models in Chat

- GLM-5.3 (Max) 1482 · 753.3 B
- Kimi K3.1 Pro (Max) 1482 · 2780.1 B
- Gemini 3.6 Flash (High) 1483
- Qwen3.8-Max 1484
- GLM-5.3-Flash 1475 · 321.3 B
- GLM-5.2 (Max) 1475 · 753.3 B

Share this page:XLinkedInRedditWhatsApp

Spotted a wrong number or a broken link? Report a mistake
