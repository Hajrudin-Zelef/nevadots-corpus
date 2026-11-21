---
id: collect-261121-veille-ia/nvidia-hardware/nemotron-3-5-super-vl-nvidia-s-open-120b-vlm-ai-tldr
title: "Nemotron 3.5 Super VL: NVIDIA's Open 120B VLM | AI/TLDR"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: ["2026-10-15"]
keywords: ["nvidia", "nemotron", "open", "super", "qwen3", "model", "tokens", "pre-training"]
source: https://ai-tldr.dev/models/nemotron-3-5-super-vl/
source_anchor: ""
source_lines: [1, 116]
section: "NVIDIA_Hardware"
---
## Overview

Nemotron 3.5 Super VL is the larger of the two models NVIDIA added to its Nemotron 3.5 generation on 15 October 2026, published alongside the laptop-sized Nemotron 3.5 Bolt 74B-A9B. It is a single open model that handles reasoning, instruction following and tool use together with image, document and video understanding, so one checkpoint can sit behind an agent that reads charts, scanned forms and screen recordings as well as text. NVIDIA built it by distilling from its larger Nemotron 3 Ultra and describes its accuracy as comparable with that model at roughly one-fifth of the parameter count.

The architecture follows the rest of the Nemotron 3 and 3.5 families: Mamba-2 state-space layers interleaved with attention layers over a Mixture-of-Experts stack, here with 120 billion total parameters, 12 billion active per token and 512 routed experts per MoE layer. A C-RADIO vision encoder and a vision-language connector feed images (JPEG, PNG) and MP4 video up to two minutes long into the language model, and built-in Multi-Token Prediction supports speculative decoding. The context window runs to one million tokens. NVIDIA's announcement puts training at about 26 trillion tokens across pre-training and continued pre-training, followed by roughly 3.8 million post-training samples and reinforcement learning across dozens of environments, including 10 billion tokens of continued pre-training on Indic languages.

As with Lightning, the release is open in the full sense NVIDIA uses for Nemotron: base, post-trained and NVFP4-quantised checkpoints under the OpenMDW-1.1 license, plus training recipes, datasets (including 40 vision-language datasets) and RL environments. The card lists 4Ã H100 or 2Ã GB200 for the BF16 checkpoint. Weights are on Hugging Face and ModelScope, hosted access runs through NVIDIA NIM on build.nvidia.com and OpenRouter, and NVIDIA names vLLM, SGLang, Ollama, LM Studio and Unsloth among supported runtimes.

NVIDIA's own comparison table is candid. Super VL scores 89.52 on GPQA Diamond, 71.10 on SWE-bench Verified, 60.67 on Terminal-Bench 2.1 and 80.82 on CharXiv reasoning, which puts it ahead of Qwen3.5-122B-A10B on most rows but behind Qwen3.8-Flash-Next and GLM-5.3-Flash on most coding and agent benchmarks â Terminal-Bench 2.1 is 85.80 for Qwen3.8-Flash-Next and 82.70 for GLM-5.3-Flash. NVIDIA's case is efficiency: it reports up to 5Ã higher agentic throughput on a single GPU than other open models in its size class and over 600 output tokens per second on Artificial Analysis agentic tasks on a Baseten endpoint.

| Released | 2026-10-15 | 
|---|---|
| License | OpenMDW-1.1 | 
| Weights | Open weights | 
| Parameters | 120B total Â· 12B active (Mixture-of-Experts, 512 routed experts per MoE layer) | 
| Context | 1M (deployment recipes may use less) | 
| Architecture | Hybrid Mamba-2 + Mixture-of-Experts + attention, with a C-RADIO vision encoder and native Multi-Token Prediction | 
| Knowledge cutoff | December 2025 (end of the pre-training data collection period) | 
| Modalities | Text, Image, Video | 
| Status | Generally available (v1.0 GA on the model card) â open weights on Hugging Face and ModelScope, hosted through NVIDIA NIM and OpenRouter | 

## Benchmarks

NVIDIA's published comparison for the BF16 checkpoint (15 October 2026). N/A in the source (text-only models on vision rows) and blank cells are shown as dashes. Scores are as reported by NVIDIA, collected through NeMo Gym.

| Benchmark | Nemotron 3.5 Super VL (BF16) | Qwen3.5-122B-A10B | Qwen3.8-Flash-Next | DeepSeek-V4-Flash-0731 | Nemotron 3 Ultra | Inkling-Small | GLM-5.3-Flash | 
|---|---|---|---|---|---|---|---|
| GPQA Diamond | 89.52 | 86.7 | 92.7 | 89.6 | 87 | 88.6 | 91.2 | 
| Humanity's Last Exam (text-only) | 31.79 | 24.2 | 39.6 | 35.8 | 29.2 | 33.3 | 36.47 | 
| MMLU-ProX | 76.91 | 83.2 | 83 | 85.5 | 83.1 | 84 | 83 | 
| LiveCodeBench v6 | 88.22 | 76.7 | 94.1 | 94.6 | 89.2 | 88.3 | 80.6 | 
| SciCode (subtask) | 47.13 | 39.8 | 46.6 | 49.3 | 42 | 48.3 | 50.05 | 
| AA-LCR v1.1 | 68.62 | 75.1 | 78.4 | 79.7 | 75.2 | 77.3 | 79.5 | 
| IFBench | 70.86 | 67.6 | 75.6 | 76.2 | 82.9 | 79.3 | 57.7 | 
| ÏÂ²-bench (Retail) | 69.24 | 55.4 | 71.6 | 73.8 | 73.7 | 71.8 | 78.2 | 
| ÏÂ²-bench (Airline) | 76.5 | 63.5 | 80.8 | 83.5 | 79.8 | 62.7 | 81.8 | 
| ÏÂ²-bench (Telecom) | 73.19 | 87.2 | 90.4 | 98.2 | 83 | 90.1 | 98.5 | 
| ÏÂ³-bench (Banking) | 18.88 | 14.2 | 44.5 | 38.6 | 18.4 | â | 34.8 | 
| BrowseComp | 78 | 58.1 | 89 | 84.2 | 67.85 | 74.1 | 86.6 | 
| SWE-bench Verified | 71.1 | 71.9 | 78.7 | 75.4 | 73.9 | 75.27 | 79.1 | 
| SWE-bench Multilingual | 67.72 | 62 | 77.1 | 72 | 68.8 | 71.4 | 78.3 | 
| SWE-bench Pro | 51.78 | 44.9 | 60.37 | 56.91 | 49.66 | 56.54 | 59.7 | 
| Terminal-Bench 2.1 | 60.67 | 48.7 | 85.8 | 76.8 | 54.8 | 53.4 | 82.7 | 
| MathVision | 78.16 | 83.3 | 87.5 | â | â | 78.8 | 82.5 | 
| CharXiv (reasoning) | 80.82 | 83.5 | 85.8 | â | â | 77.8 | 81.9 | 
| OCRBench v2 (English) | 68.12 | 68.3 | 67.9 | â | â | 54.2 | 61.8 | 
| MMLongBench-Doc | 62.82 | 63.7 | 67.9 | â | â | 49.06 | 65.8 | 
| OSWorld-Verified | 51.14 | 51.5 | 55.8 | â | â | â | 14.78 | 

### This model's scores

1. GPQA Diamond89.52%
2. LiveCodeBench v688.22%
3. CharXiv (reasoning)80.82%
4. MathVision78.16%
5. BrowseComp78%
6. SWE-bench Verified71.1%
7. IFBench70.86%
8. AA-LCR v1.168.62%
9. OCRBench v2 (English)68.12%
10. Terminal-Bench 2.160.67%
11. SWE-bench Pro51.78%
12. OSWorld-Verified51.14%
13. Humanity's Last Exam (text-only)31.79%

Scores on a 0â100 scale (25-point gridlines); higher is better. Each benchmark links to its published source.

## Strengths

- One open checkpoint covers text reasoning, tool use, OCR, charts, documents and up to two minutes of video
- 12B active parameters out of 120B, with NVIDIA reporting up to 5Ã the single-GPU agentic throughput of open models in its class
- Permissive OpenMDW-1.1 license with base, post-trained and NVFP4 checkpoints, training data, recipes and RL environments published
- 1M-token context window and native Multi-Token Prediction for speculative decoding
- 80.82 on CharXiv reasoning and 89.52 on GPQA Diamond in NVIDIA's published evaluations

## Best for

- Document-heavy agents â invoices, receipts, contracts, charts and tables â where text and vision need to live in one model
- Self-hosted multimodal agents on DGX, RTX PRO or cloud GPUs where a permissive license and published training data matter
- Post-training and domain customisation from the open BF16 and base checkpoints with NeMo AutoModel, NeMo RL and NeMo Gym
- Indic-language workloads, drawing on the 10B tokens of Indic continued pre-training NVIDIA describes

## How to access

## Nemotron (open-weight) â every version

The full lineage of the Nemotron (open-weight) line, newest first. Every version has its own page â click any to compare specs, benchmarks and pricing.

| Version | Released | Context | License | 
|---|---|---|---|
| Nemotron 3.5 Bolt E4Bcurrent | 2026-10-23 | 64K | OpenMDW-1.1 | 
| **Nemotron 3.5 Super VL** | 2026-10-15 | 1M | OpenMDW-1.1 | 
| Nemotron 3.5 Bolt 74B-A9B | 2026-10-15 | 256K | OpenMDW-1.1 | 
| Nemotron 3.5 Lightning | 2026-08-11 | 1M | OpenMDW-1.1 | 

## FAQ

## What can Nemotron 3.5 Super VL take as input?

Text, images (JPEG and PNG) and MP4 video up to two minutes long, with text as output. NVIDIA positions it for OCR, tables, charts, documents and video alongside ordinary reasoning, coding and tool use, so a single model can back a multimodal agent.

## Is it open source?

The weights are published under the OpenMDW-1.1 license, which the model card says is ready for commercial or non-commercial use. NVIDIA released base, post-trained and NVFP4-quantised checkpoints together with training recipes, datasets and RL environments.

## How does it compare with Qwen3.8-Flash-Next and GLM-5.3-Flash?

On NVIDIA's own table it trails both on most coding and agent rows â SWE-bench Verified 71.10 against 78.7 and 79.10, Terminal-Bench 2.1 60.67 against 85.80 and 82.70 â while leading on GraphWalks (89.71) and staying close on GPQA Diamond (89.52 against 92.7 and 91.2). NVIDIA's argument is throughput rather than peak accuracy.

## What hardware does it need?

The model card lists 4Ã H100 or 2Ã GB200 for the BF16 checkpoint, with an NVFP4 release for lighter deployments. NVIDIA's announcement lists DGX Spark, DGX Station, RTX PRO and RTX systems, Jetson and data-centre GPUs among supported targets.

## How is it related to Nemotron 3.5 Bolt?

Bolt 74B-A9B was compressed from Super VL with NVIDIA's Star Elastic method and then quantised to about 2 bits so it fits in 24GB of memory for local agents. Both were released on 15 October 2026.
