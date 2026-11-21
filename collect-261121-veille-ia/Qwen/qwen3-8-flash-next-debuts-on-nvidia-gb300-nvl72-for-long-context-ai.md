---
id: collect-261121-veille-ia/qwen/qwen3-8-flash-next-debuts-on-nvidia-gb300-nvl72-for-long-context-ai
title: "Qwen3.8-Flash-Next Debuts on NVIDIA GB300 NVL72 for Long-Context AI"
domain: qwen
role: reference
task: article
actors: ["Alibaba"]
dates: []
keywords: ["qwen3", "flash-next", "nvidia", "gb300", "nvl72", "model", "long-context", "debuts"]
source: https://blockchain.news/postamp?id=qwen3-8-flash-next-nvidia-gb300-nvl72
source_anchor: ""
source_lines: [1, 27]
section: "Qwen"
---
# Qwen3.8-Flash-Next Debuts on NVIDIA GB300 NVL72 for Long-Context AI

Alibaba has released the model weights for **Qwen3.8-Flash-Next**, an experimental multimodal Mixture-of-Experts (MoE) AI model designed to push the boundaries of long-context processing. Positioned as a preview of the upcoming Qwen4 architecture, the model boasts cutting-edge innovations aimed at high-efficiency, high-context applications. Developers can now run Qwen3.8-Flash-Next on NVIDIA’s powerful **GB300 NVL72** platform, achieving unprecedented throughput and scalability for agentic coding, document processing, and more.

The **Qwen3.8-Flash-Next** model features a 125-billion parameter base, augmented by 51 billion N-gram embeddings, and a native 262,144-token context window extensible to 1 million tokens using the YaRN framework. This makes it particularly suited for tasks requiring extensive sequential data processing, such as legal document analysis or tool-driven automation workflows.

## Technical Innovations: GDN and QSA

At the core of Qwen3.8-Flash-Next are two architectural breakthroughs: **Gated DeltaNet (GDN)** and **Qwen Sparse Attention (QSA)**. GDN eliminates the memory burden of growing key-value (KV) caches by compressing historical context into a fixed-size recurrent state. This ensures consistent performance regardless of sequence length. Meanwhile, QSA addresses computational inefficiencies in long-context attention by aggregating sequences into micro-blocks and retrieving only the most relevant regions, significantly reducing overhead.

Benchmarks published by Alibaba validate these innovations. At a 1 million-token context length, the QSA attention kernel reportedly delivered **7.6x speedups in prefill** and **4.9x in decoding** compared to traditional full-attention mechanisms. In a real-world serving test with a 90% prefix-cache hit rate, Qwen3.8-Flash-Next achieved **8.6x prefill throughput** over its predecessor, Qwen3.7-Plus.

## Performance on NVIDIA GB300 NVL72

The NVIDIA GB300 NVL72 platform, featuring 72 Blackwell Ultra GPUs and a 130 TB/s NVLink communication domain, is the backbone for Qwen3.8-Flash-Next’s deployment. This setup enables the model to process over **16,000 tokens per second per GPU**, equating to more than 200 tokens per second per user. Such performance highlights its potential for high-throughput, low-latency applications.

For developers, the scalability of Qwen3.8-Flash-Next is noteworthy. While large-scale deployments can leverage the GB300 NVL72, smaller setups can use NVIDIA DGX Station or workstations equipped with RTX PRO 6000 GPUs for prototyping.

## Fine-Tuning and Deployment

Developers aiming to customize the model for specific use cases can take advantage of NVIDIA NeMo tools. The NeMo AutoModel library supports fine-tuning directly on the provided checkpoints, either via full supervised fine-tuning (SFT) or memory-efficient low-rank adaptation (LoRA). For reinforcement learning, NVIDIA’s NeMo RL recipes offer additional optimization pathways.

Inference options are flexible, with supported stacks including SGLang, vLLM, and TokenSpeed. Open-weight availability on Hugging Face and ModelScope ensures accessible experimentation.

## Looking Ahead

Qwen3.8-Flash-Next is a stepping stone toward the Qwen4 model family, with Alibaba using it as a testbed for next-generation architectural concepts. Its innovations in attention mechanisms and memory efficiency suggest transformative potential for long-context AI applications. Developers now have the opportunity to experiment with this cutting-edge model and prepare for the Qwen4 release.
