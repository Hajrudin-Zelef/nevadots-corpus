---
id: collect-261121-veille-ia/nvidia-hardware/nvidia-nemotron-3-5-lightning-30b-a3b
title: "NVIDIA Nemotron 3.5 Lightning 30B-A3B"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: ["2026-08-11"]
keywords: ["nemotron", "nvidia", "lightning", "b-a3b", "model", "ultra", "tokens", "accuracy"]
source: https://awesomeagents.ai/models/nvidia-nemotron-3-5-lightning-30b-a3b/
source_anchor: ""
source_lines: [1, 135]
section: "NVIDIA_Hardware"
---
NVIDIA released Nemotron 3.5 Lightning on August 11, 2026 - a 30 billion parameter Mixture-of-Experts model that activates only 3 billion parameters per token. It's not built to out-think anything. It's built to run the same agent loop thousands of times a day without falling over on cost or latency.

**TL;DR**

- 30B MoE with 3B active parameters, distilled from Nemotron 3 Ultra for high-volume, low-latency agent execution
- 86% PinchBench accuracy, up to 4x output speed of comparable open models, $0.05/$0.20 per million tokens on OpenRouter
- Trades peak intelligence for throughput: ties gpt-oss-120b at 24 on the Artificial Analysis Intelligence Index while running circles around it on speed

## Overview

Lightning replaces Nemotron 3 Nano 30B-A3B as the small, fast option in NVIDIA's Nemotron 3 lineup. Architecturally it's the same idea as the rest of the family: interleaved Mamba-2 and MoE layers plus a handful of standard Attention layers, so long sequences stay cheap while attention handles precise recall. What's new is where the weights came from - NVIDIA distilled Lightning down from Nemotron 3 Ultra, the 550B-parameter flagship, rather than training a small model from scratch, so it inherits some of Ultra's judgment at a fraction of the compute.

The positioning is explicit: Ultra handles orchestration and complex multi-step planning, Lightning executes the repetitive, well-defined steps underneath it - tool calls, form-filling, routine coding edits, the bulk of an agent's turns that don't need frontier reasoning. NVIDIA shipped a companion open-source router, NeMo Switchyard, to automate that handoff inside existing agent harnesses.

On paper, Lightning competes with Qwen 3.6-35B-A3B, Muse Glimmer, and OpenAI's gpt-oss-120b in the small open-MoE class. It doesn't win on raw benchmark accuracy against any of them. It wins on accuracy-per-dollar and accuracy-per-second, which matters for pipelines that call a model thousands of times per task rather than once.

*Nemotron 3.5 Lightning scores 24 on the Artificial Analysis Intelligence Index (v4.1.1), tied with gpt-oss-120b (high) and just behind Nemotron 3 Super (26). Nemotron 3 Ultra, its teacher model, tops the open-weight field at 38.*
Source: blogs.nvidia.com

## Key Specifications

| Specification | Details | 
|---|---|
| Provider | NVIDIA | 
| Model Family | Nemotron 3.5 | 
| Parameters | 30B total / 3B active | 
| Architecture | Hybrid Mamba-2 + MoE + select Attention layers | 
| Context Window | 256K tokens (single H100, BF16); up to 1M tokens (NVFP4 on Blackwell) | 
| Input Price | $0.05/M tokens (OpenRouter) | 
| Output Price | $0.20/M tokens (OpenRouter) | 
| Release Date | August 11, 2026 | 
| License | OpenMDW-1.1 (permissive, commercial use allowed) | 
| Training Data | 20+ trillion tokens; distilled from Nemotron 3 Ultra | 
| Knowledge Cutoff | September 2025 (pretraining); May 2026 (post-training) | 
| Languages | English, Spanish, French, German, Italian, Japanese; 43 programming languages | 
| Min. Hardware | 1x H100 80GB or A100 80GB | 

## Benchmark Performance

| Benchmark | Nemotron 3.5 Lightning | Nemotron 3 Nano 30B-A3B | Qwen 3.6-35B-A3B | 
|---|---|---|---|
| MMLU-Pro | 81.94 | 78.3 | 85.2 | 
| GPQA Diamond | 75.44 | 73.0 | 86.0 | 
| SWE-bench Verified | 51.56 | 38.8 | 73.4 | 
| AA-LCR (long context) | 52.00 | N/P | N/P | 
| PinchBench (agentic accuracy) | 86% | N/P | ~86% (per NVIDIA, "similar accuracy") | 
| AA Intelligence Index (v4.1.1) | 24 | N/P | N/P | 

N/P = not published for that model.

The single-turn benchmarks tell an unflattering story in isolation: Lightning trails Qwen 3.6-35B-A3B by 3 to 11 points on MMLU-Pro, GPQA Diamond, and SWE-bench Verified. NVIDIA built Lightning to win on a different axis. On PinchBench, NVIDIA's own 10,000-task agentic benchmark, Lightning matches Qwen 3.6-35B-A3B's accuracy while finishing the same task set in roughly 30% fewer H100 GPU-hours. NVIDIA frames this as the "efficiency frontier," and the chart backs the claim: similar accuracy, meaningfully less compute.

*NVIDIA's PinchBench positioning chart: Nemotron 3.5 Lightning reaches similar accuracy to Qwen 3.6-35B in fewer H100 GPU-hours, and clears Gemma 4 26B on both axes.*
Source: blogs.nvidia.com

The Artificial Analysis Intelligence Index tells a more sober version of the same story: Lightning scores 24, tied with gpt-oss-120b (high) and 14 points behind Nemotron 3 Ultra's 38. It's a genuine improvement over the Nemotron 3 Nano generation it replaces, but nobody should mistake this for a frontier-reasoning model. It's a specialist built for the execution layer, not the planning layer, and the numbers reflect that tradeoff honestly.

## Key Capabilities

**Speculative decoding baked into pretraining.** Lightning was trained with multi-token prediction from the start rather than bolting speculative decoding on afterward, and ships with two purpose-built draft models: DSpark, a semi-autoregressive drafter that proposes a full block of candidate tokens in one forward pass (recommended for DGX Spark and low-concurrency deployments), and DFlash, a lightweight block-diffusion drafter for the same job. Combined with native MTP, this is what gets NVIDIA to its up-to-4x throughput claim over similarly-sized open models.

**Built for agent harnesses, not benchmarks.** NVIDIA tuned Lightning against real production workloads during a private preview with CrowdStrike (security alert triage), CodeRabbit (coding review router), Harvey with Trajectory (legal task completion), and Lila Sciences (physical-science simulation). NVIDIA's own numbers show large jumps over off-the-shelf checkpoints on these tasks - +45 points on CrowdStrike's benign-recall test, +34 on CodeRabbit's coding router versus GPT-5.4 Nano. These are vendor-reported, single-customer numbers, not independent benchmarks, so treat them as directional.

**Quantization and deployment flexibility.** Lightning ships as BF16 for full-precision customization and NVFP4 for inference, using the same NVFP4 kernels NVIDIA built for Ultra, across Blackwell, Hopper, and Ampere GPUs. It runs on a single H100 or A100, and the NVFP4 checkpoint is what unlocks the full 1M token context - standard BF16 on one H100 caps at 256K, the same split Ultra uses.

## Pricing and Availability

OpenRouter lists Lightning at $0.05 per million input tokens and $0.20 per million output tokens, with Balanced, Nitro (fastest), and Exacto (highest tool-calling accuracy) routing modes across multiple hosting providers. That undercuts Nemotron 3 Nano's $0.06/$0.24 pricing on DeepInfra despite Lightning being the more capable model.

Weights are open on Hugging Face in BF16, NVFP4, NVFP4-DFlash, and NVFP4-DSpark variants, plus a Base-BF16 checkpoint with no post-training applied for teams building their own SFT/RL pipeline. It's also on ModelScope, as an NVIDIA NIM microservice on build.nvidia.com, and through Amazon SageMaker JumpStart, DeepInfra, Baseten, and GMI Cloud. Inference framework support spans vLLM, SGLang, TensorRT-LLM, llama.cpp, Ollama, LM Studio, and Unsloth.

For customization, NVIDIA points teams at NeMo Automodel for LoRA or full SFT, NeMo Megatron Bridge for larger training runs, and NeMo RL plus NeMo Gym for reinforcement learning. It also open-sourced the Nemotron-RL Agentic Terminal Pivot dataset - training data for RL on terminal-based agentic tasks - alongside the release.

Lightning doesn't try to out-reason Qwen 3.6-35B-A3B. It tries to finish the same 10,000 tasks for less GPU time - and NVIDIA's own chart shows it actually doing that.

## Strengths and Weaknesses

### Strengths

- Distilled from Nemotron 3 Ultra rather than trained from scratch, inheriting some frontier-model judgment at 3B active parameters
- Native multi-token prediction plus two purpose-built draft models (DSpark, DFlash) for up to 4x output throughput
- Runs on a single H100 or A100 - no multi-GPU cluster required
- Aggressive OpenRouter pricing: $0.05/$0.20 per million tokens, cheaper than its own predecessor
- OpenMDW-1.1 license permits unrestricted commercial use
- Broad day-one inference framework support (vLLM, SGLang, TensorRT-LLM, llama.cpp, Ollama, LM Studio, Unsloth)
- Open-sourced Nemotron-RL Agentic Terminal Pivot dataset for teams building agentic RL pipelines

### Weaknesses

- Trails Qwen 3.6-35B-A3B by 3-11 points on MMLU-Pro, GPQA Diamond, and SWE-bench Verified - not a reasoning upgrade
- AA Intelligence Index of 24 sits 14 points behind its own teacher model, Nemotron 3 Ultra (38)
- Full 1M token context needs the NVFP4 checkpoint on Blackwell; standard BF16 single-H100 caps at 256K
- Strongest capability numbers (CrowdStrike, CodeRabbit, Harvey, Lila Sciences) are NVIDIA's own private-preview partner results, not independent evaluation
- Optimized for agent-harness execution; less compelling as a general-purpose chat or knowledge model

## Related Coverage

- NVIDIA Nemotron 3 Ultra 550B-A55B - the 550B teacher model Lightning was distilled from
- NVIDIA Ships Nemotron 3 Ultra - 550B Open-Weight MoE - our coverage of the parent model's release
- Nemotron 3 Nano 30B-A3B - the model Lightning replaces in the family lineup
- Nemotron 3 Super Review: Best Open Model for Agents - our hands-on review of the mid-size Nemotron 3 model
- Qwen 3.6-35B-A3B - closest same-class competitor on raw benchmark accuracy
- Kimi K2.5 vs Nemotron 3 Nano 30B-A3B - our head-to-head on the predecessor model
- Agentic AI benchmarks leaderboard - broader agentic model comparisons
- SWE-Bench coding agent leaderboard - full SWE-Bench rankings across all models

## FAQ

### What is Nemotron 3.5 Lightning best at?

High-volume, low-latency agent execution - tool calling, coding edits, and routine multi-turn tasks where speed and cost per call matter more than peak reasoning depth. It's designed to run underneath a larger orchestrator model, not replace one.

### Was Nemotron 3.5 Lightning trained from scratch?

No. NVIDIA distilled it from Nemotron 3 Ultra, its 550B-parameter flagship, rather than pretraining a new small model independently.

### How does it compare to Qwen 3.6-35B-A3B?

Qwen 3.6-35B-A3B scores higher on MMLU-Pro, GPQA Diamond, and SWE-bench Verified. Lightning matches it on PinchBench agentic accuracy while using roughly 30% less GPU-hour compute to finish the same 10,000 tasks, and it runs at up to 4x the output speed thanks to native speculative decoding.

### Can I run Nemotron 3.5 Lightning locally?

Yes. It fits on a single H100 80GB or A100 80GB GPU in BF16, and the NVFP4 quantized checkpoint runs even leaner on Blackwell, Hopper, or Ampere hardware. It's supported out of the box in Ollama, LM Studio, and llama.cpp.

### Is Nemotron 3.5 Lightning commercially licensed?

Yes. It ships under the OpenMDW-1.1 license, which permits commercial use without geographic restriction, the same license NVIDIA used for the rest of the Nemotron 3 family.

### What's NeMo Switchyard?

An open-source router NVIDIA shipped with Lightning. It hands routine agent-loop steps to Lightning while keeping complex planning on a frontier model, which NVIDIA says cuts task cost to roughly a third of running the frontier model alone.

**Sources:**

✓ Last verified August 12, 2026
