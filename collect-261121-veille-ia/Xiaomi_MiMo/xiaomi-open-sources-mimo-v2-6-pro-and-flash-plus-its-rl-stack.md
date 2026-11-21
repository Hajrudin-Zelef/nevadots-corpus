---
id: collect-261121-veille-ia/xiaomi-mimo/xiaomi-open-sources-mimo-v2-6-pro-and-flash-plus-its-rl-stack
title: "Xiaomi Open-Sources MiMo-V2.6 Pro and Flash, Plus Its RL Stack"
domain: xiaomi-mimo
role: reference
task: article
actors: ["Xiaomi"]
dates: ["2026-09-21"]
keywords: ["flash", "model", "xiaomi", "mimo-v2", "active", "parameters", "runs", "both"]
source: https://rits.shanghai.nyu.edu/ai/xiaomi-open-sources-mimo-v2-6-pro-and-flash-plus-its-rl-stack/
source_anchor: ""
source_lines: [1, 42]
section: "Xiaomi_MiMo"
---
**Xiaomi has released the weights of MiMo-V2.6-Pro and MiMo-V2.6-Flash under the MIT licence**, one week after it began streaming their reinforcement learning (RL) runs on a public dashboard. The two checkpoints went up on Hugging Face on September 21, 2026 (UTC), and Xiaomi’s official announcement is dated September 22. Pro is a 1.02-trillion-parameter Mixture-of-Experts (MoE) model with 42 billion active parameters. Flash has 309 billion parameters, 15 billion of them active. Xiaomi also published a 9B model distilled from both into Qwen3.5-9B, a technical report, its RL code and more than 7,000 training environments.

## Two Checkpoints, One Architecture

According to the model cards, both models use the same design. A sparse MoE backbone mixes sliding-window attention (SWA) layers with a smaller number of global-attention (GA) layers. The models are natively multimodal: a 681M-parameter vision encoder and two audio encoders (308M and 127M parameters) feed the backbone, so text, image, video and audio all go into one model. Both models support a 1-million-token context.

- **MiMo-V2.6-Pro:** 1.02T total / 42B active parameters. It has 70 layers (60 SWA, 10 GA), a hidden size of 6144, and 384 routed experts, 8 of which are active per token.
- **MiMo-V2.6-Flash:** 309B total / 15B active parameters. It has 48 layers (39 SWA, 9 GA) and a hidden size of 4096. Its 5-layer multi-token-prediction drafter proposes up to 7 tokens per pass for speculative decoding. The FP8 weights come to 172.9 GB, according to OrcaRouter.

The “-RL” suffix in the repository names is not an adapter. It marks these as the full checkpoints produced by the RL runs Xiaomi streamed live. For serving, Xiaomi recommends SGLang with speculative decoding. The model card’s vLLM example uses tensor parallelism of 8 for Pro and 4 for Flash.

## Benchmarks

Xiaomi’s own tables put Pro close to the leading closed models on agentic coding and computer use. The table below compares Pro and Flash with Claude Opus 5 and GPT-5.6 Sol:

| Benchmark | V2.6-Flash | V2.6-Pro | Claude Opus 5 | GPT-5.6 Sol | 
|---|---|---|---|---|
| DeepSWE v1.1 | 67.9 | 71.9 | 74.0 | 73.0 | 
| Toolathlon-Verified | 73.6 | 76.9 | 80.6 | 74.9 | 
| AutomationBench | 52.3 | 53.1 | 50.3 | 45.8 | 
| ProgramBench | 26.0 | 26.5 | 37.0 | 25.0 | 
| OSWorld-Verified | â | 82.0 | 83.4 | 83.0 | 

The jump over the previous generation is large. MiMo-V2.5-Pro scored 19.0 on DeepSWE v1.1 and 49.1 on Toolathlon-Verified. On cybersecurity, Flash (95.1) edges out Pro (94.0) on CyberGym. On ExploitGym, however, Pro’s 17.8 trails Claude Opus 5 (22.1) and GPT-5.6 Sol (30.3).

Third-party testing tells a similar story. Pro scores 46 on Artificial Analysis‘s Intelligence Index v4.3, which VentureBeat reports as the highest score of any open-weight model. That is up from 26 for V2.5-Pro, and ahead of DeepSeek V4.1 Flash (39).

## The RL Run, Now Reproducible

The model cards describe Group Relative Policy Optimization (GRPO) at very large batch sizes: 1,568 prompts with 16 rollouts each per step. Two further techniques shape the rewards: Groupwise Reward Synthesis and Groupwise Advantage Redistribution. After RL, a multi-teacher on-policy distillation stage (MOPD2) follows. VentureBeat reports that both runs finished after 30 steps and about 750,000 trajectories. The final compute bill was .62 million for Pro and ,000 for Flash, split roughly evenly between training (43.5%) and rollout generation (43.8%), with grading taking the remaining 12.7%. VentureBeat quotes Xiaomi’s Fuli Luo calling it “likely one of the largest single reinforcement-learning runs undertaken by an open-source model team.”

The released environments cover four kinds of agent task: software engineering, vulnerability reproduction, knowledge-intensive work, and web design and development. According to TestingCatalog, the technical report also documents how graders were built and how Xiaomi defended against reward hacking.

## A 9B Model for Smaller Hardware

MiMo-V2.6-Distill-Qwen-9B takes a different route. It is Qwen3.5-9B fine-tuned on 77.4 billion tokens of MiMo-generated data (27.2 billion of them loss-bearing), split across code, cybersecurity, general and visual tasks. Against the base Qwen model, the largest gains come on agentic tasks. SWE-bench Pro rises from 32.0 to 44.6, AutomationBench from 5.0 to 30.3, and Terminal-Bench 2.1 from 27.0 to 37.1. SWE-bench Verified barely moves (60.0 to 61.1).

## What This Means

Xiaomi’s openness goes well beyond releasing weights. The V2.6 RL runs were watchable while they happened, and now the code and environments behind them are public, so outside groups can inspect or rerun the post-training that produced the gains. On price, VentureBeat lists API rates of $0.435 input / $0.87 output per million tokens for Pro and $0.14 / $0.28 for Flash. Artificial Analysis’s cost-versus-intelligence chart places Pro in its “most attractive quadrant”, at roughly a tenth of the cost per task of closed models with similar scores. Serving either checkpoint locally is still a multi-GPU job, though. For readers with a single consumer GPU, the distilled 9B model is the practical way in.

*This post was drafted with AI assistance and reviewed by RITS staff.*
