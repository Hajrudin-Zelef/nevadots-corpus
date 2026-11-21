---
id: collect-261121-veille-ia/nvidia-hardware/nvidia-nemotron-3-5-super-vl-key-specs-deployment-paths-and-selection-guide
title: "NVIDIA Nemotron 3.5 Super VL: Key Specs, Deployment Paths, and Selection Guide"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: []
keywords: ["nvidia", "nemotron", "super", "model", "multimodal", "text", "license", "deployment"]
source: https://www.namoclaw.ai/blog/nvidia-nemotron-3-5-super-vl-guide
source_anchor: ""
source_lines: [1, 157]
section: "NVIDIA_Hardware"
---
This article breaks down NVIDIA Nemotron 3.5 Super VL's core capabilities, context window, deployment options, and typical use cases so developers can quickly decide if it fits their multimodal workloads.

**TL;DR:** NVIDIA Nemotron 3.5 Super VL is a multimodal LLM (text + vision) built on Nemotron 3 Ultra, offering 128K context, native function calling, and an open license for commercial use. It runs on your own GPUs via NVIDIA NIM containers — no per-token fees — making it a strong fit for enterprises with H100/A100 clusters that need data privacy and high-throughput vision-language inference.


## #What Is NVIDIA Nemotron 3.5 Super VL?

Nemotron 3.5 Super VL is NVIDIA's **multimodal large language model** (a model that understands both images and text). It extends the Nemotron 3 Ultra backbone with a vision encoder, enabling direct processing of screenshots, charts, diagrams, and PDF pages while retaining strong reasoning and code generation capabilities.

- **Parameters** : ~70B (exact figure not publicly disclosed as of writing; check NGC page)
- **Context window** : 128K tokens
- **Input modalities** : Text + images (single or multiple)
- **Output modality** : Text
- **License** : NVIDIA Open Model License (commercial use, modification, and redistribution permitted with attribution)

We position it as the go-to multimodal foundation for teams running their own GPU clusters — unlike closed APIs, you pay for hardware, not tokens.

## #Core Capability Comparison: Nemotron 3.5 Super VL vs. Mainstream Multimodal Models

| Dimension | Nemotron 3.5 Super VL | GPT-4o (OpenAI) | Claude 3.5 Sonnet | LLaVA-Next-72B (Open) | 
|---|---|---|---|---|
| Deployment | Self-hosted (NIM/NGC/bare container) | Cloud API only | Cloud API only | Self-hosted | 
| Billing | Hardware capex/opex | Per-token | Per-token | Hardware capex/opex | 
| Context window | 128K | 128K | 200K | 128K | 
| Max visual resolution | 1024×1024 per image | Multi-image, high-res | Multi-image, high-res | Depends on vision encoder | 
| Function calling | Native | Native | Native | Requires fine-tuning | 
| Reasoning/code base | Nemotron 3 Ultra (strong code) | GPT-4 base | Claude 3.5 base | Llama-3 base | 
| License | NVIDIA Open Model License | Proprietary | Proprietary | Llama 3 Community License | 

**How to read this table**: If you already own H100/A100 clusters, need data residency, and want native function calling, Nemotron 3.5 Super VL minimizes integration effort. If you lack GPU infra or are just prototyping, closed APIs get you started faster.

## #Run Your First Inference in 10 Minutes

NVIDIA packages TensorRT-LLM optimizations into **NIM (NVIDIA Inference Microservices)** containers with single-/multi-GPU tensor parallelism. Minimal steps:

### #1. Prerequisites

- One or more NVIDIA GPUs (recommended: H100 80GB × 1 or A100 80GB × 2; VRAM ≥ quantized model weight size)
- Docker + NVIDIA Container Toolkit installed
- NGC API Key (generate on NGC website)

### #2. Pull and Start the NIM Container

```
docker run -d --name nemotron-vl \
  --gpus all \
  -p 8000:8000 \
  -e NGC_API_KEY=$NGC_API_KEY \
  nvcr.io/nim/microsoft/nemotron-3-5-super-vl:latest
```
Container starts in 2–5 minutes (first pull downloads ~40 GB weights). Ready when logs show `Uvicorn running on http://0.0.0.0:8000`.


### #3. Send Your First Multimodal Request

```
curl -X POST http://localhost:8000/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "nemotron-3-5-super-vl",
    "messages": [
      {
        "role": "user",
        "content": [
          {"type": "text", "text": "Summarize the key components in this architecture diagram"},
          {"type": "image_url", "image_url": {"url": "https://example.com/arch.png"}}
        ]
      }
    ],
    "max_tokens": 512,
    "temperature": 0.2
  }'
```
The response's `choices[0].message.content` holds the model output.

## #Typical Production Scenarios & Prompt Templates

### #Scenario 1: Technical Diagram / Architecture Auto-Interpretation

**Teams**: DevOps, SRE, Architects**Prompt skeleton**:

```
You are a senior system architect. Analyze the microservice topology in the image and output:
1. Core service inventory
2. Data flows (use arrows)
3. Single-point-of-failure risks
4. Scaling recommendations
```
### #Scenario 2: Financial Report / Research Chart Extraction

**Teams**: Investment research, Finance automation**Prompt skeleton**:

```
Extract all tabular data from the image(s) as Markdown tables.
Separate multiple charts with ## headings.
```
### #Scenario 3: UI Screenshot to Code Generation

**Teams**: Frontend / Full-stack developers**Prompt skeleton**:

```
Generate a React + Tailwind component from this UI screenshot. Requirements:
- TypeScript
- PascalCase component name
- Responsive breakpoints: sm/md/lg
- Single-file output only
```
## #VRAM Planning & Quantization Guidance

| Quantization | Single-GPU VRAM | TP=2 (2 GPUs) | Latency vs. FP16 | Accuracy Risk | 
|---|---|---|---|---|
| FP16 / BF16 | ~140 GB | 2×80 GB (A100/H100) | Baseline | Low | 
| INT8 (AWQ/GPTQ) | ~75 GB | 2×48 GB | ~0.7× | Medium | 
| INT4 (AWQ/GPTQ) | ~40 GB | 2×24 GB | ~0.5× | High (vision tasks sensitive) | 

**Our take**: Vision-language tasks degrade faster under quantization. Start with INT8; only drop to INT4 if VRAM forces it, and always run your own eval set (≥50 samples) to verify quality.

## #What This Means for Developers

1. **Data stays in your VPC** — meets finance, healthcare, gov compliance.
2. **Predictable cost** — one-time GPU capex/rental; marginal inference cost ≈ electricity. Ideal for high throughput (millions of vision-language requests/day).
3. **Low vendor lock-in** — NIM exposes an OpenAI-compatible API; switching to vLLM/TGI only requires changing the base URL.
4. **You own ops** — driver updates, model refreshes, multi-GPU scheduling, monitoring/alerting are your responsibility.

If your team lacks GPU ops experience, try NVIDIA's managed NIM endpoints on-demand before committing to self-hosted.

## #Quick Fit Checklist (5-Minute Self-Assessment)

- Access to H100/A100 with ≥80 GB VRAM (single or multi-GPU)?
- Business handles sensitive images (ID docs, medical scans, industrial drawings) that cannot leave your network?
- Daily inference volume >10k requests or peak concurrency >50?
- Team can maintain Docker/K8s + NVIDIA driver stack?
- Need native function calling to internal systems?

**All yes → Deploy Nemotron 3.5 Super VL + NIM**;**Only first two → Prototype with closed APIs first, evaluate migration later**;**None → Start with quantized LLaVA-Next-72B on consumer GPUs to validate workflow**.

## #FAQ

### #Does Nemotron 3.5 Super VL support video input?

No. Current release accepts static images only (single or multiple). For video, sample frames yourself and feed as multi-image input.

### #Can it run on a consumer GPU like RTX 4090 24GB?

Only INT4 quantized with CPU offloading (e.g., llama.cpp `--split-mode layer`). Expect 5–10 sec/turn latency — not production viable.

### #Does the NIM container require internet to download weights?

First start pulls weights (~40 GB). After that, use `docker save/load` or a private registry for fully offline deployments.

### #How does it differ from Nemotron 3 Ultra (text-only)?

Super VL freezes the Ultra LLM backbone, adds a vision encoder, and undergoes multimodal alignment training. Text reasoning stays comparable; vision understanding and image-text reasoning are new.

### #How do I monitor NIM health in production?

The container exposes a `/metrics` endpoint (Prometheus format) with `request_latency_seconds`, `gpu_utilization`, `queue_size`, etc. — plug directly into Grafana.
