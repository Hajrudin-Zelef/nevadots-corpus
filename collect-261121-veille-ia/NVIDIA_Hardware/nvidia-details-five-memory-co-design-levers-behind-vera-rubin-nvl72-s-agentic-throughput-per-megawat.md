---
id: collect-261121-veille-ia/nvidia-hardware/nvidia-details-five-memory-co-design-levers-behind-vera-rubin-nvl72-s-agentic-throughput-per-megawat
title: "NVIDIA details five memory co-design levers behind Vera Rubin NVL72's agentic throughput-per-megawatt claim"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: []
keywords: ["rubin", "vera", "nvidia", "memory", "nvl72", "agentic", "levers", "about"]
source: https://www.at-pulse.com/stories/5778/
source_anchor: ""
source_lines: [1, 116]
section: "NVIDIA_Hardware"
---
AI-assisted, not human-edited. Methodology Corrections Who owns this site

## **Rising**How it’s measured

YouTube video · 1.8K views · 43 likes

Reach blends public engagement (views, likes, comments, shares, stars) from the platforms where the story or its original source lives: YouTube, Hacker News, GitHub, Substack and similar. A video and the article written from its transcript share one score, taken from the video. Platforms with no data are left out rather than counted as zero, and the age of a story never lowers its score.

NVFP4 stores values as 4-bit elements, and each block of 16 shares an FP8 scale. That gives finer control than MXFP4, which uses larger blocks. NVIDIA reports weights about 3.5x smaller than FP16 and a KV cache up to half the size of FP8. Its own pretraining paper finds accuracy close to FP8, with coding tasks slightly behind. [1] [2] [7]

The headline figure is up to 30x more agentic throughput per megawatt than GB300 NVL72. NVIDIA measured it itself on its AgentX benchmark at 160 tokens/s per user. SemiAnalysis's separate tests found up to 7x per megawatt. Its cost-of-ownership advantage fell to 1.4–3x at the 60–100 tokens/s most providers serve. [3] [5]

Several levers aim to reuse computed context instead of recomputing it. Dynamo routing, Vera host memory over NVLink-C2C and a new CMX flash tier between local SSD and shared storage all serve that goal. Reuse only pays when restoring a prefix is faster than recomputing it and the cache state is compatible. [1] [2] [4]

Rubin's advantage over GB300 varies by more than an order of magnitude depending on tokens/s per user. Implication: buyers should benchmark at their own interactivity target.

## Read the full assessment

They should also measure prefix-reuse rates before paying for new storage tiers.

## Executive brief

NVIDIA says Vera Rubin NVL72 delivers up to 30x the agentic throughput per megawatt of GB300 NVL72, even though each GPU has the same 288 GB of HBM. The Nov 5 NVIDIA post attributes the gain to five memory "levers": NVFP4, wide expert parallelism over NVLink, Dynamo KV-cache reuse, the Vera CPU, and CMX pod-level flash. NVIDIA ran the 30x measurement itself, and SemiAnalysis's review is still listed as pending. SemiAnalysis's own published tests report up to 7x per megawatt. At the 60–100 tokens/s that most providers serve, it found only 1.4–3x per TCO.

## What changed and event timeline

1. ### Context-memory tier announcedNVIDIA introduced what is now CMX at CES. It was then called the Inference Context Memory Storage platform and was later renamed, which created some confusion ().
2. ### GTC unveils Vera Rubin and CMX/STXHuang's GTC chart showed about 3x performance per MW for Vera Rubin (). More than 20 storage vendors pledged STX support, with partner platforms due in the second half of 2026 ().

## Capabilities and access

- **Rubin GPU:** up to 288 GB HBM4, 19.2 TB/s of memory bandwidth and 3.0 TB/s of NVLink 6.
- **Vera CPU:** 88 Olympus cores and up to 1.5 TB of LPDDR5X.
- **Software:** Wide-EP runs in TensorRT-LLM. Dynamo provides KV-aware routing, disaggregated serving and the KV Cache Runner (KVCR), which moves data over NIXL.

## Read the full section

All figures below come from the primary post.

- **Rubin GPU:** up to 288 GB HBM4, 19.2 TB/s of memory bandwidth and 3.0 TB/s of NVLink 6. That comes to about 20.7 TB of HBM4 per NVL72 rack.
- **Vera CPU:** 88 Olympus cores and up to 1.5 TB of LPDDR5X. It connects to Rubin over NVLink-C2C at 1.8 TB/s, about 7x a PCIe Gen 6 x16 link.
- **Software:** Wide-EP runs in TensorRT-LLM. Dynamo provides KV-aware routing, disaggregated serving and the KV Cache Runner (KVCR), which moves data over NIXL.
- **CMX:** built on BlueField-4 STX, Spectrum-X Ethernet and DOCA Memos. It currently applies only to Rubin racks (Blocks & Files).

## Technical analysis for researchers and developers

- **NVFP4:** each value is a 4-bit E2M1 element. MXFP4 uses larger groups and coarser scales (video 3:30).
- **Wide-EP:** DeepSeek-R1 at EP64 puts 4 experts per layer on each GPU. That is 232 of the model's 14,848 expert blocks.
- **KV tiering:** the stack runs from G1 (HBM) through G2 (host memory), G3 (local SSD), the new G3.5 (CMX) and G4 (shared storage).

## Read the full section

- **NVFP4:** each value is a 4-bit E2M1 element. Every 16 values share an FP8 E4M3 scale, and a per-tensor FP32 scale sits on top. MXFP4 uses larger groups and coarser scales (video 3:30).
- **Wide-EP:** DeepSeek-R1 at EP64 puts 4 experts per layer on each GPU. That is 232 of the model's 14,848 expert blocks. EPLB rebalances hot and cold experts between forward passes. The payoff depends on whether weight reuse outweighs all-to-all communication cost (video 6:01).
- **KV tiering:** the stack runs from G1 (HBM) through G2 (host memory), G3 (local SSD), the new G3.5 (CMX) and G4 (shared storage). Offloading only pays when restoring a prefix is faster than recomputing it. Reuse also requires a compatible cache state and permission to use it (video 11:13).
- **Benchmark:** AgentX replays recorded Claude Code sessions, preserving tool-call gaps and growing context (NVIDIA Developer).

## Claims and evidence

Unless noted, the claims below are NVIDIA's own figures.

## Read the full section

Unless noted, the claims below are NVIDIA's own figures.

- **NVFP4 weights:** about 3.5x smaller than FP16 and 1.8x smaller than FP8, with up to 4x FP8 throughput on DeepSeek R1 running on GB300 (primary). NVIDIA's own pretraining paper reports NVFP4 accuracy close to FP8, with coding tasks slightly behind (arXiv).
- **NVFP4 KV cache:** up to 50% smaller than FP8, scoring above 99% of the BF16 baseline across 10 DeepSeek-v4-Pro benchmarks (primary).
- **Storage feeding the GPU:** VAST and WEKA, both storage vendors, reported 35 GB/s to one H100 and 270 GB/s aggregate across eight H100s, respectively (primary).
- **CMX:** up to 5x more tokens/s and up to 5x better power efficiency than traditional storage (video 11:30).
- **30x per MW:** NVIDIA measured this, and SemiAnalysis's review was pending (NVIDIA Developer).

## Context and prior work

- The approach builds on disaggregated prefill/decode and prefix caching. VAST's KV-reuse test used vLLM with LMCache (primary).
- An independent NVFP4 training study at ICML 2026 compares training recipes up to 8B dense and 30B-A3B MoE models (ICML).
- Rival KV-cache extenders exist, for example MinIO's MemKV (HPCwire).

## Limitations, safety and contested findings

- **How big the gain is depends on the benchmark setup.** NVIDIA reports 30x per MW at 160 TPS.
- **The AgentX results leave out Vera's tool-calling performance.**
- **CMX is unproven in practice.** Blocks & Files notes thin explanations, limited availability and unclear data flow between tiers (Blocks & Files).

## Read the full section

- **How big the gain is depends on the benchmark setup.** NVIDIA reports 30x per MW at 160 TPS. SemiAnalysis reports up to 7x per MW. Its 67x per-TCO figure applies at 170 TPS, where GB300 is near its limit, and shrinks to 1.4–3x at typical serving speeds (SemiAnalysis).
- **The AgentX results leave out Vera's tool-calling performance.** The part of the five levers credited to the CPU is therefore not reflected in them (NVIDIA Blog).
- **CMX is unproven in practice.** Blocks & Files notes thin explanations, limited availability and unclear data flow between tiers (Blocks & Files).
- **The post never measures each lever separately.** No figure shows how much of the 30x each lever accounts for.

## Business and practitioner implications

- Rubin's advantage over GB300 varies by more than an order of magnitude depending on tokens/s per user (SemiAnalysis).
- Software levers like NVFP4, Dynamo routing and KV offload can help on current hardware. Measure your prefix-reuse rates before buying storage tiers.
- CMX ties KV storage to Rubin and BlueField. Some storage vendors were missing from the launch partner list (Blocks & Files).

## Read the full section

- **Compare at your own interactivity target.** Rubin's advantage over GB300 varies by more than an order of magnitude depending on tokens/s per user (SemiAnalysis).
- **Measure first, buy second.** Software levers like NVFP4, Dynamo routing and KV offload can help on current hardware. Measure your prefix-reuse rates before buying storage tiers.
- **Watch for lock-in.** CMX ties KV storage to Rubin and BlueField. Some storage vendors were missing from the launch partner list (Blocks & Files).

## The source trail.

## Sources (9)

### Five Ways NVIDIA Co-Design Maximizes the Value of Every Byte in the AI Factory

Article text retrieved; extracted text may omit tables or interactive elements. A transcript of the video linked from this page is supplied (linked video auto captions; https://www.youtube.com/watch?v=Ud3gdKrbdgw); automatic text may contain errors.

developer.nvidia.com
### Linked video: Five Ways NVIDIA Co-Design Maximizes the Value of Every Byte in the AI Factory

linked video auto captions

www.youtube.com
