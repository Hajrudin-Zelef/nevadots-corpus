---
id: collect-261121-veille-ia/kimi/moonshot-releases-kimi-k3-1-pro-2-8t-moe-1m-context
title: "Moonshot Releases Kimi K3.1 Pro: 2.8T MoE, 1M Context"
domain: kimi
role: reference
task: article
actors: ["Moonshot AI"]
dates: ["2026-11-04"]
keywords: ["kimi", "moonshot", "context", "window", "model", "coding", "releases", "token"]
source: https://emergent.sh/news/moonshot-releases-kimi-k3-1-pro-2-8t-moe-1m
source_anchor: ""
source_lines: [1, 31]
section: "Kimi"
---
# Moonshot Releases Kimi K3.1 Pro: 2.8T MoE, 1M Context

Moonshot AI officially launched on November 4, 2026 the open weights for Kimi K3.1 Pro, a 2.8 trillion-parameter mixture-of-experts model that activates 104 billion parameters per token and supports a 1,048,576-token context window. The model card on Hugging Face positions K3.1 Pro as Moonshot's most capable 3T-class model to date, with architecture optimizations for agentic coding and long-context tasks. The release immediately climbed to the top of Hugging Face trending models with more than 425 likes.

## Architecture and Training

Kimi K3.1 Pro uses a 93-layer architecture with 896 experts, selecting 16 per token plus 2 shared experts. The attention mechanism combines 69 Key-Distilled Attention (KDA) layers with 24 Gated DeepSeek Sparse Attention (Gated DSA) layers, replacing the full-attention Gated MLA layers from Kimi K3 to improve long-context efficiency. Weights are quantization-aware trained to MXFP4 precision with MXFP8 activations, reducing inference footprint while maintaining performance at the 3T parameter scale.

The model integrates a 401-million-parameter MoonViT-V2 vision encoder for native multimodal input. Moonshot reports that K3.1 Pro was post-trained from Kimi K3 using large-scale joint reinforcement learning across hundreds of downstream domains, targeting coding workflows and agent-based tasks.

## Benchmark Performance

Moonshot published self-reported scores across a range of coding, reasoning, and agent benchmarks. The model scored 65.7 on Terminal-Bench 4.0, 51.7 on SWE Marathon v1.1, 92.1 on BrowseComp, 70.0 on HLE-Full with tools, and 94.2 on GPQA-Diamond. Several internal benchmarks (Kimi Code Bench, DECK-Arena, Business Operation) appear in the model card but lack third-party validation.

The Terminal-Bench 4.0 and SWE Marathon scores place K3.1 Pro among the leading open-weight models for coding tasks, though independent evaluations have not yet been published. The 1,048,576-token context window positions the model for document-heavy agent workflows where long-context recall is critical.

## Release Details

The weights are available on Hugging Face under a bespoke Kimi K3.1 Pro License rather than an open-source license like Apache 2.0 or MIT. The custom license restricts certain commercial uses and derivative works, following the trend of restrictive licenses on large-scale open-weight releases.

API access is available through platform.kimi.ai with endpoints compatible with OpenAI and Anthropic SDKs. Moonshot has not published pricing for API inference or detailed hardware requirements for self-hosting the MXFP4 checkpoint. Several AI researchers shared the Hugging Face link within hours of release.

## Context Window Engineering

The 1M-token context window represents a 16-fold increase over the 65,536-token window in many production models. Moonshot reports that the Gated DSA layers provide more efficient attention computation at scale compared to full-attention mechanisms, enabling the extended window without proportional latency increases.

Use cases for the extended window include multi-file codebases, long-form document retrieval, and multi-turn agent workflows where conversation history must remain in context. The combination of sparse attention and quantization-aware training aims to make the 1M-token window practical for inference deployments.

## What This Means

Kimi K3.1 Pro extends the ceiling for open-weight mixture-of-experts models in coding and agentic tasks. The 1M-token context window and Terminal-Bench 4.0 score position it as a strong alternative to proprietary coding models, though the bespoke license limits redistribution and derivative fine-tuning. Self-hosting the MXFP4 checkpoint will require evaluation teams to confirm whether the reported benchmarks hold under third-party testing and whether the long-context performance remains stable across diverse inputs.
