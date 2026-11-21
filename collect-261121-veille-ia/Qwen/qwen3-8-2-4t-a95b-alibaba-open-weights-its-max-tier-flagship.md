---
id: collect-261121-veille-ia/qwen/qwen3-8-2-4t-a95b-alibaba-open-weights-its-max-tier-flagship
title: "Qwen3.8-2.4T-A95B: Alibaba Open-Weights Its Max-Tier Flagship"
domain: qwen
role: reference
task: article
actors: ["Alibaba"]
dates: []
keywords: ["model", "qwen3", "alibaba", "weights", "t-a95b", "max-tier", "heads", "tokens"]
source: https://rits.shanghai.nyu.edu/ai/qwen3-8-2-4t-a95b-alibaba-open-weights-its-max-tier-flagship/
source_anchor: ""
source_lines: [1, 29]
section: "Qwen"
---
**Alibaba published the weights for Qwen3.8-2.4T-A95B on August 13, 2026** â the open-weight release of Qwen3.8-Max, the 2.4-trillion-parameter Mixture-of-Experts model it launched as an API product ten days earlier. It is the first time Alibaba has released a Max-tier model’s weights, and at 2.4T total parameters it is the largest open-weight language model published to date.

## Technical Details

The model is a fine-grained MoE with 2.4T total parameters and 95B activated per token â a sparsity ratio of roughly 25:1. It carries 512 experts with 11 active per token (10 routed plus one shared), each with an intermediate dimension of 2,048. The 92 layers follow a repeating hybrid-attention pattern: 23 blocks of three Gated DeltaNet layers followed by one full Gated Attention layer, each paired with an MoE block. Gated DeltaNet supplies linear attention across 128 value heads and 16 query-key heads; the full-attention layers use 64 query heads against 4 key-value heads at a head dimension of 256.

Context is 262,144 tokens natively, extensible to roughly 1.01 million. Reasoning is not optional â the model card states that thinking mode is required for all interactions, with `reasoning_effort` selectable across `low`, `medium`, and `xhigh` (the default). Maximum reasoning length is 262,144 tokens and maximum final output 131,072.

One capability did not survive the transition from API to open weights: the released checkpoint **does not support multimodal input**, while the hosted Qwen3.8-Max does. Vision and video benchmark numbers in Alibaba’s launch materials therefore describe the API model rather than the weights on Hugging Face.

Alibaba’s own numbers put the model ahead on research reproduction (PaperBench 93.0) and agentic computer use (OSWorld-Verified 86.1), and behind on software engineering (SWE-bench Pro 67.7 against Fable 5’s 80.0) and terminal agency (TerminalBench 2.1 86.6 against GPT-5.6 Sol’s 88.8). These are vendor-reported. Independent evaluation so far is narrower but broadly consistent: the Vals Index places Qwen3.8-Max second among open-weight models and tenth overall out of 43 at 66.1, and Frontend Code Arena has it fourth at 1,668 Elo, behind Claude Opus 5 and Kimi K3.

## What It Takes to Run

Sparsity cuts inference cost, not storage. NVIDIA reports serving the model in FP8 on a GB300 NVL72 rack â 72 Blackwell Ultra GPUs in a single NVLink domain â at over 4,000 tokens per second per GPU and over 350 tokens per second per user, with NVFP4 gains still to come.

Below that tier, the constraint is memory. Unsloth’s GGUF conversions run from 4.89 TB at BF16 down to 397 GB for a dynamic 1-bit quantization, with intermediate stops at 1.31 TB (4-bit), 657 GB (2-bit), and 956 GB (3-bit). Unsloth puts the practical floor at 410 GB of combined RAM and VRAM â a figure that admits well-provisioned workstations and small servers, and excludes essentially every consumer GPU configuration.

The weights also ship under a custom licence rather than the Apache 2.0 terms Alibaba has used for smaller Qwen releases. Products above 100 million monthly active users or $20 million in monthly revenue must display the model name prominently in their interface, and Model-as-a-Service or “AI work assistant” businesses earning more than $50 million a year need a separate commercial licence.

## What This Means

The gap between open and closed frontier models has usually been measured in months. What is unusual here is the gap between *released* and *runnable*: the weights are public, but the hardware needed to serve them at full precision is not something most institutions have, and the 1-bit quantization that fits a large workstation carries accuracy costs that independent evaluation has not yet measured.

For university research groups, the practical value is less about local deployment than about access to a frontier-scale checkpoint for study â interpretability work, routing analysis, and fine-tuning experiments that a hosted API cannot support. The hybrid linear-plus-full attention layout is also now inspectable at frontier scale rather than described in a paper, which matters for anyone tracking whether linear attention holds up outside small models.

Alibaba’s team called it “the most powerful model besides Fable 5” and demonstrated it running 16 days of continuous autonomous programming. Both claims await independent replication.

*This post was drafted with AI assistance and reviewed by RITS staff.*
