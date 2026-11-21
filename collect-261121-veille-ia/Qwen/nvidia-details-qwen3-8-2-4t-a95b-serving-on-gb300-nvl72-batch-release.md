---
id: collect-261121-veille-ia/qwen/nvidia-details-qwen3-8-2-4t-a95b-serving-on-gb300-nvl72-batch-release
title: "NVIDIA Details Qwen3.8-2.4T-A95B Serving on GB300 NVL72 | Batch & Release"
domain: qwen
role: reference
task: article
actors: ["Alibaba"]
dates: ["2026-10-15"]
keywords: ["nvidia", "model", "qwen3", "serving", "gb300", "nvl72", "t-a95b", "reasoning"]
source: https://batchandrelease.com/nvidia-details-qwen3-8-2-4t-a95b-serving-on-gb300-nvl72
source_anchor: ""
source_lines: [1, 21]
section: "Qwen"
---
Alibaba released the open weights for Qwen3.8-2.4T-A95B, a 2.4 trillion parameter mixture-of-experts model with 95 billion activated parameters per token. The model uses a hybrid of full and linear attention, supports a context window of up to one million tokens, and an output length of up to 128K. NVIDIA published serving details for the model on the GB300 NVL72, a rack-scale platform with 72 Blackwell Ultra GPUs connected by NVLink at 130 TB/s.

## Performance and architecture

Without additional tuning, the model achieves over 4,000 tokens per second per GPU and over 350 tokens per second per user on GB300 NVL72 in FP8 precision on Day 0. NVIDIA expects further gains from NVFP4 precision optimizations. The fine-grained MoE design spreads capacity across many smaller experts, and a learned router activates only the experts needed per token, so serving costs track the 95B active parameters rather than the full 2.4T.

The hybrid attention architecture alternates between full-attention layers, where every token attends to every other, and linear-attention layers that replace the growing KV cache with a bounded recurrent state. This keeps compute and memory bounded as context scales to one million tokens, which matters for agentic workloads that accumulate system instructions, tool outputs, retrieved documents, code, logs, and multi-step reasoning traces.

Built-in reasoning controls (low, high, xhigh) let developers configure inference depth per request, trading compute for reasoning quality. Developers can dial up for complex multi-step reasoning or dial down for high-throughput document processing.

## Serving and post-training options

NVIDIA supports multiple inference stacks. SGLang, vLLM, and NVIDIA Dynamo provide open-source inference recipes for developers who want control over performance on NVIDIA hardware. The model is also available as a model-free NVIDIA NIM container, which serves any supported model from downloaded weights and can scale to production.

For domain-specific use cases, NVIDIA NeMo AutoModel supports post-training directly on Hugging Face checkpoints without model conversion. It offers full supervised fine-tuning or memory-efficient LoRA fine-tuning.

Hosted APIs from DeepInfra, DigitalOcean, Fireworks AI, Modal, and OpenRouter provide immediate access. Weights are downloadable from Hugging Face and ModelScope, and deployment via NVIDIA NIM from NGC is available. The model is also referred to as Qwen3.8-Max.

## Sources & references

NVIDIA: Serve Qwen3.8-2.4T-A95B, a 2.4T-Parameter Model, with Configurable Reasoning on NVIDIA GB300 NVL72 ↗Source consulted: 2026-10-15
