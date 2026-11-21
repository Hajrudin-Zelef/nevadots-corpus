---
id: collect-261121-veille-ia/nvidia-hardware/nvidia-nemotron-3-5-super-and-lightning
title: "NVIDIA Nemotron 3.5 Super and Lightning"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: []
keywords: ["nemotron", "super", "lightning", "long-context", "agentic", "billion-token", "nvidia", "training"]
source: https://research.nvidia.com/labs/nemotron/Nemotron-3.5-Super-and-Lightning/
source_anchor: ""
source_lines: [1, 74]
section: "NVIDIA_Hardware"
---
We introduce **NVIDIA Nemotron 3.5 Super** and **NVIDIA Nemotron 3.5 Lightning**, open and efficient Mixture-of-Experts Hybrid Mamba-Transformer models designed for reasoning, long-context understanding, and agentic workloads.

Nemotron 3.5 Super is a **120-billion-parameter, 12-billion-active multimodal model**, while Nemotron 3.5 Lightning is a smaller **30-billion-parameter, 3-billion-active text model**. Both are designed for efficient deployment and combine large-scale distillation, long-context training, native speculative decoding, and efficiency-focused quantization. Super further supports reasoning across text, images, video, and interactive tool environments.

### Key Features

1. **Efficient hybrid architecture:** Mixture-of-Experts Hybrid Mamba-Transformer models activate only a fraction of their total parameters for each token.
2. **Native speculative decoding:** Multi-Token Prediction (MTP) heads accelerate generation, with dedicated post-training that improves acceptance at deeper draft positions.
3. **Long-context support:** Both models are trained for context lengths up to**1 million tokens** .
4. **Agentic and multimodal capabilities:** Super supports text, images, video, software-engineering agents, visual tool use, and long-running interactive workloads.
5. **Reasoning control:** Super supports reasoning-off, regular, and medium-effort modes, as well as inference-time reasoning-budget control.
6. **Efficient deployment:** Released BF16 and NVFP4 checkpoints are accompanied by optimized inference recipes and standalone speculative drafters.

### Training Nemotron 3.5

Both models begin with a dedicated **100-billion-token MTP warm-up**, followed by **1 trillion tokens of distillation** from the next-larger Nemotron 3 model. Nemotron 3.5 Super is distilled from Nemotron 3 Ultra Base, while Lightning is distilled from Nemotron 3 Super Base. Following distillation, Lightning undergoes a 21-billion-token long-context phase that extends it to a 1-million-token context length. Super proceeds through the additional continued-pretraining and mid-training stages below.

Nemotron 3.5 Super then undergoes additional training targeted at multilingual, long-context, and agentic use cases:

- **Indic continued pretraining:** A 100-billion-token stage includes synthetic and natural Indic data spanning 22 Indian languages and 34 language-script configurations. It improves all five evaluated Indic benchmarks, with gains of up to**9.82 percentage points** .
- **Long-context training:** A 35-billion-token continued-pretraining stage extends Super to a**1-million-token context length** .
- **Agentic software-engineering mid-training:** A 65.4-billion-token stage at 256K context uses repositories, issueâpull-request pairs, commit histories, build and CI logs, and agent trajectories. It improves long-horizon software engineering and robustness across agent harnesses.
- **Long-context recovery:** Because Superâs 256K mid-training stage reduced its RULER-1M score, a final 9-billion-token recovery stage at 1-million-token context restores the score from 33.5 to 81.0.

Superâs post-training pipeline combines supervised fine-tuning, Reinforcement Learning from Verifiable Rewards (RLVR), Reinforcement Learning from Human Feedback (RLHF), specialized teacher training, and Multi-Teacher On-Policy Distillation (MOPD). This process transforms the text-only base model into a multimodal agent while consolidating capabilities from reasoning, agentic, alignment, video, and vision-tool-use teachers. Lightning follows a lighter text-only pipeline using supervised fine-tuning and RLVR.

### Key Highlights

- Nemotron 3.5 Super achieves **51.78% on SWE-bench Pro** , surpassing Nemotron 3 Ultra while using approximately**4.6Ã fewer active parameters** .
- Agentic mid-training improves DeepSWE pass@1 from **6.4% to 10.4%** and improves robustness across multiple software-engineering agent harnesses.
- Parallel agent execution followed by self-verification improves Terminal-Bench 2.1 from **62.7% to 71.9%** , DeepSWE from**24.6% to 34.5%** , and BrowseComp from**71.5% to 79.8%** on the evaluated setups.
- MTP boosting improves average speculative-decoding acceptance length from 4.287 to 4.718 under greedy decoding and from 3.787 to 4.612 under probabilistic draft sampling.
- The released Nemotron 3.5 Super NVFP4 checkpoint achieves **99.62% median accuracy recovery** relative to BF16 across 36 benchmarks.
- On a single NVIDIA GB200 GPU, Nemotron 3.5 Super reaches up to **4,856 output tokens/s/GPU** while sustaining at least 50 tokens/s/user on the evaluated 20-turn agentic workload.
- Under the evaluated interactive serving conditions, NVFP4 delivers **2.43Ã the throughput of BF16** . Nemotron 3.5 Super delivers**2.34Ã and 1.90Ã the throughput** of Qwen3.5 122B-A10B and Qwen3.8 Flash-Next, respectively.

### Efficient Inference

Nemotron 3.5 is optimized for interactive, long-context agentic workloads. Hybrid-state caching, prefix reuse, Mamba checkpointing, GPU-kernel selection, and speculative decoding are jointly tuned to sustain responsive per-user generation rates at high aggregate throughput.

Post-training MTP boosting improves native speculative decoding, while standalone parallel drafters target low-concurrency, memory-bound deployments:

- **LiLiCorr** for Nemotron 3.5 Super
- **DFlash** and**DSpark** for Nemotron 3.5 Lightning

Nemotron 3.5 Super is validated across NVIDIA A100, H100, B200, GB200, and GB300 platforms, as well as DGX Spark, DGX Station, and RTX-class GPUs. Its quantization and speculative-decoding configurations can be tuned to the target platform and workload.

### Open Source

We are releasing base, post-trained BF16, and NVFP4-quantized checkpoints, together with speculative drafters, datasets, model cards, cookbooks, evaluation recipes, and training recipes.

#### Checkpoints

- **Nemotron 3.5 Super VL 120B-A12B Base BF16** â base model
- **Nemotron 3.5 Super VL 120B-A12B BF16** â post-trained model
- **Nemotron 3.5 Super VL 120B-A12B NVFP4** â post-trained and NVFP4-quantized model
- **Nemotron 3.5 Super VL 120B-A12B NVFP4 LiLiCorr** â standalone LiLiCorr drafter
- **Nemotron 3.5 Lightning 30B-A3B Base BF16** â base model
- **Nemotron 3.5 Lightning 30B-A3B BF16** â post-trained model
- **Nemotron 3.5 Lightning 30B-A3B NVFP4** â post-trained and NVFP4-quantized model
- **Nemotron 3.5 Lightning 30B-A3B NVFP4 DFlash** â standalone DFlash drafter
- **Nemotron 3.5 Lightning 30B-A3B NVFP4 DSpark** â standalone DSpark drafter

#### Data and Recipes

- **Nemotron-Pretraining-Indic-v1:** approximately 2.83 trillion synthetic output tokens across 22 Indian languages and 34 language-script configurations.
- **Post-training datasets and environments:** resources for supervised fine-tuning, reinforcement learning, reasoning, coding, software engineering, tool use, and multilingual capabilities.
- **Nemotron 3.5 Super evaluation recipes**
- **Nemotron 3.5 Lightning evaluation recipes**
- **NVIDIA Nemotron Developer Repository**

#### Technical Report

The **Nemotron 3.5 Super and Lightning Technical Report** provides full details on pretraining, post-training, quantization, inference optimization, evaluation methodology, and benchmark results.
