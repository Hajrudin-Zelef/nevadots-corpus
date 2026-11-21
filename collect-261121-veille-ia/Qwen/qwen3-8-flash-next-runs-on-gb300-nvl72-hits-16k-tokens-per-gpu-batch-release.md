---
id: collect-261121-veille-ia/qwen/qwen3-8-flash-next-runs-on-gb300-nvl72-hits-16k-tokens-per-gpu-batch-release
title: "Qwen3.8-Flash-Next Runs on GB300 NVL72, Hits 16K Tokens per GPU | Batch & Release"
domain: qwen
role: reference
task: article
actors: ["Alibaba"]
dates: ["2026-10-15"]
keywords: ["nvidia", "gb300", "nvl72", "model", "tokens", "qwen3", "flash-next", "agentic"]
source: https://batchandrelease.com/qwen3-8-flash-next-runs-on-gb300-nvl72-hits-16k-tokens-per
source_anchor: ""
source_lines: [1, 17]
section: "Qwen"
---
NVIDIA published an experiment running Alibaba's Qwen3.8-Flash-Next on the GB300 NVL72 platform for agentic coding workloads. The model, a multimodal mixture-of-experts design with 125 billion total parameters and 6 billion activated per token, also carries an additional 51 billion N-gram embedding parameters. Its native context window is 262,144 tokens, extendable to 1 million using YaRN. Alibaba positions the model as a preview of the upcoming Qwen4 architecture.

## Architecture and performance

The model alternates three Gated DeltaNet layers with one Qwen Sparse Attention layer. Gated DeltaNet layers compress historical context into a fixed-size recurrent state, which prevents KV-cache growth. Qwen Sparse Attention aggregates tokens into micro-blocks and selects the most relevant blocks, reducing attention compute. Alibaba benchmarks report up to 7.6 times prefill speedup and 4.9 times decoding speedup versus full attention for 1-million-token workloads. In a cache-heavy online test with a 90 percent prefix-cache hit rate, the model achieved 8.6 times the prefill throughput of Qwen3.7-Plus.

On the GB300 NVL72, NVIDIA reports over 16,000 tokens per second per GPU and over 200 tokens per second per user for agentic coding workloads. The GB300 NVL72 integrates 72 Blackwell Ultra GPUs with a 72-GPU NVLink domain providing 130 TB/s all-to-all bandwidth. The model can also run on local hardware such as DGX Station, DGX Spark clusters, and workstations with four RTX PRO 6000 Blackwell Max-Q GPUs.

## Deployment and fine-tuning

Fine-tuning is supported via NVIDIA NeMo AutoModel with full SFT or LoRA memory-efficient methods. Reinforcement learning fine-tuning can be performed using NVIDIA NeMo RL recipes. Inference can be deployed with open-source stacks SGLang, vLLM, or TokenSpeed. Day-0 functional support for the model is provided by NVIDIA across SGLang, vLLM, and TensorRT LLM.

Model weights are available for download from Hugging Face and ModelScope. A hosted version is accessible through QwenCloud for immediate experimentation. For inference engineers, the key takeaway is that the hybrid recurrent-sparse architecture changes how KV cache is managed, which directly impacts memory footprint and throughput on long-context agentic workloads. The GB300 NVL72 numbers from NVIDIA show what the platform can do with this model, but local hardware options also exist for smaller deployments.

## Sources & references

NVIDIA: Experiment with Qwen3.8-Flash-Next on NVIDIA GB300 NVL72 for Agentic Coding ↗Source consulted: 2026-10-15
