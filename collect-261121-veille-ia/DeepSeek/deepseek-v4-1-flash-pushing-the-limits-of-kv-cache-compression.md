---
id: collect-261121-veille-ia/deepseek/deepseek-v4-1-flash-pushing-the-limits-of-kv-cache-compression
title: "DeepSeek-V4.1-Flash: Pushing the Limits of KV Cache Compression"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: []
keywords: ["cache", "deepseek-v4", "flash", "deepseek-v4-flash", "model", "causal", "prefill", "bytes"]
source: https://arxiviq.substack.com/p/deepseek-v41-flash-pushing-the-limits
source_anchor: ""
source_lines: [1, 23]
section: "DeepSeek"
---
**Authors:** *~500 authors***Affiliations:** *DeepSeek-AI***Paper:** https://arxiv.org/abs/2609.19969**Code:** N/A**Model:** https://huggingface.co/deepseek-ai/DeepSeek-V4.1-Flash

# TL;DR

**WHAT was done?** DeepSeek-V4.1-Flash is a 552B-backbone (plus 196B Engram lookup) multimodal Mixture-of-Experts (MoE) model for 1M-token contexts. It cuts serving costs via a Causal Encoder-Decoder (CED) architecture (activating 8B parameters per token in prefill and 16B in decode), cross-layer cache reuse in Compressed Sparse Attention 2 (CSA2) with FP4 quantization, and Sliding-Window Attention (SWA) Bounded Replay to avoid storing local window caches on disk.

**WHAT was found?** The model shrinks runtime global KV cache in HBM to 890 bytes per token (vs 3,514 bytes for DeepSeek-V4-Flash) and cuts persistent SSD KV storage to roughly 1/8 of DeepSeek-V4-Flash. On DeepSWE v1.1, it reaches 74.2% resolved rate (vs 54.4% for DeepSeek-V4-Flash and 74.0% for Opus-5), and on Terminal-Bench 2.1 it achieves 90.6% Pass@1 (vs 82.7% for DeepSeek-V4-Flash and 89.1% for Opus-5).

**WHY it matters?** Serving million-token agent sessions becomes bounded by compute rather than HBM or SSD capacity, though the paper reports no component ablations for its eight efficiency approximations, leaves multi-turn SWA Bounded Replay drift untested, and still trails larger closed systems on expert science tasks like Terminal-Bench 4.0 (31.2% vs 51.8% for Opus-5).

# Details

### Why long-horizon agent serving is now a storage and prefill problem

Sparse attention has largely solved the quadratic compute cost of long sequences, leaving HBM capacity, SSD bandwidth, and prefill compute on cache misses as the primary limits on serving million-token agents. Building directly on the authors’ earlier DeepSeek-V4, this paper introduces DeepSeek-V4.1-Flash, a 552B-backbone multimodal Mixture-of-Experts (MoE) model plus 196B Engram lookup parameters (748B total) that compresses runtime and persistent key-value (KV) caches while halving prefill compute.

The headline numbers in **Figure 1** show how far this compression goes. Global KV cache per token in HBM drops from 389,120 bytes in DeepSeek-V1 and 3,514 bytes in DeepSeek-V4-Flash down to 890 bytes in DeepSeek-V4.1-Flash. At the same time, single-token decode FLOPs stay nearly flat across context lengths (**Figure 2**): scaling context 256-fold from 4K to 1M tokens raises decode FLOPs by only 25%.

What stands out to me in **Figure 1** and **Figure 2** is how the co-design trades static weight capacity for per-token cache compression: total parameters grow from 284B in DeepSeek-V4-Flash to 748B (552B backbone plus 196B Engram), while runtime KV memory drops by 4x and persistent SSD cache drops by 8x. Instead of treating model architecture, quantization, and disk eviction as separate layers of the stack, the authors co-design all three around a single observation: local sliding-window states die within minutes, whereas compressed global states live for days.

### Halving prefill compute: The return of Encoder-Decoder via YOCO

In The Transformer Zoo Revisited, I asked whether pure decoder-only models are truly the end of history, or if encoder-decoder architectures would stage a comeback. Earlier, in my coverage of Microsoft’s You Only Cache Once (YOCO) (paper), we saw how a “decoder-decoder” layout could capture the main systems advantage of an encoder-decoder—cheap prompt encoding decoupled from generation—without giving up the unified, causal next-token pre-training objective of decoder-only LLMs. DeepSeek-V4.1-Flash brings this exact layout to frontier scale under the name Causal Encoder-Decoder (CED), shown in **Figure 3**. Rather than reverting to T5-style bidirectional encoders and cross-attention, CED keeps causal attention across all 40 layers (d = 5120), splitting the backbone into a 20-layer causal encoder and a 20-layer decoder. Every layer contains a DeepSeekMoE feed-forward block (1 shared expert and 384 routed experts of intermediate dimension 2304, activating 6 routed experts per token) and a local Sliding-Window Attention (SWA) branch with window size n<sub>win</sub> = 128, while all layers except the first two also include a global sparse attention branch.
