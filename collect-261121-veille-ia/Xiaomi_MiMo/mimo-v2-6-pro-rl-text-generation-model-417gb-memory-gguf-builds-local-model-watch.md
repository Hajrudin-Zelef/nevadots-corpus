---
id: collect-261121-veille-ia/xiaomi-mimo/mimo-v2-6-pro-rl-text-generation-model-417gb-memory-gguf-builds-local-model-watch
title: "MiMo-V2.6-Pro-RL Text Generation Model: ~417GB Memory, GGUF Builds | Local Model Watch"
domain: xiaomi-mimo
role: reference
task: article
actors: ["Xiaomi"]
dates: ["2026-09-27"]
keywords: ["model", "mimo-v2", "text", "tokens", "layers", "pro-rl", "experts", "flash"]
source: https://localmodelwatch.tsuchitsuchi.com/en/2026/09/27/mimo-v2-6-pro-rl-released/
source_anchor: ""
source_lines: [1, 215]
section: "Xiaomi_MiMo"
---
## At a Glance

| Item | Value | 
|---|---|
| Repository | XiaomiMiMo/MiMo-V2.6-Pro-RL | 
| Family guide | MiMo-V2.6 guide (4 articles) | 
| Publisher guide | Xiaomi (MiMo): models and licenses | 
| Published | 2026-09-22 | 
| License | mit | 
| Formats | safetensors | 
| Source type | Primary source (the publisher itself) | 

*Values determined by this site’s code when the information was collected. Dates are JST.*

**What we checked ourselves**

- The same Japanese text takes about as many tokens as with the Qwen3 tokenizer.

Details and conditions are in “Our Own Measurements” below.

## Overview

Xiaomi’s MiMo team has released the flagship model of its new-generation **MiMo-V2.6** series, **MiMo-V2.6-Pro-RL**, on Hugging Face. Featuring 1.02T total parameters with 42B active during inference via a Mixture of Experts (MoE) architecture, it handles text, images, video, and audio in a single model with a 1M token context window. The model is released under the MIT license, allowing the weights to be used freely, including for commercial purposes.

The core theme of the series is “scaling reinforcement learning (RL) to create a system where the model continues to improve on its own." Rather than training coding, general agents, visual work, and cybersecurity separately, **they are trained together in a single mixed RL stage** (referred to in the model card as “You Only RL Once"). The mid-sized model in the same series, MiMo-V2.6-Flash-RL (309B total / 15B active), was released simultaneously, and our site has already covered its GGUF version in MiMo-V2.6-Flash-RL-GGUF Multimodal MoE Model: ~141GB Memory. This article covers the official Pro weights that sit above it.

## Specifications

Based on the “Model Summary" and architecture table in the model card.

- Parameter count: 1.02T total / 42B active (Sparse MoE)
- Layer configuration: 70 layers. Of these, 60 layers use Sliding Window Attention (SWA with a 128 token window) and 10 layers use global attention (GA). The very first layer uses GA and a dense FFN, while the rest alternate between SWA and GA, both using MoE FFNs without shared experts.
- Experts: 8 out of 384 experts used
- Hidden layer dimension: 6,144. Attention heads use Q 128 / KV 8 for both SWA and GA, with head dimensions of QK 192 / V 128.
- Context length: Up to 1M tokens
- Inputs: Text, images, video, audio
- Visual encoder: 681M parameter MiMo ViT (28 layers, of which 24 are SWA and 4 are GA)
- Audio encoder: 308M AudioTokenizer (20-stage RVQ codebook) and 127M audio patch encoder (combining 4 frames into 1 patch, reducing 25Hz to 6.25Hz)
- Speculative decoding: Built-in MTP (Multi-Token Prediction) drafter built with 5 SWA layers, predicting the subsequent 7 tokens in a single forward pass for parallel verification.

Compared to the mid-sized Flash, Pro scales up across every axis: layers increase from 48 to 70, hidden dimensions from 4,096 to 6,144, and experts from 256 to 384. On the other hand, Flash was trained on more pre-training tokens. According to the technical report, Flash was trained on 48T tokens (26T text, 22T multimodal), while Pro was trained on 30T tokens (27T text, 3T multimodal).

## Performance

Reproduced directly from the evaluation table in the model card. Comparison targets include the Flash model from the same series, the previous-generation MiMo-V2.5 Pro, and three closed commercial models (Claude Opus 5, GPT-5.6 Sol, and Claude Fable 5), with all figures published by their respective creators (“-" indicates no result available).

→ Scroll horizontally to see all columns

| Benchmark | MiMo-V2.6 Pro | MiMo-V2.6 Flash | MiMo-V2.5 Pro | Claude Opus 5 | GPT-5.6 Sol | Claude Fable 5 | 
|---|---|---|---|---|---|---|
| **Code Agent** |  |  |  |  |  |  | 
| DeepSWE v1.1 | 71.9 | 67.9 | 19.0 | 74.0 | 73.0 | 70.0 | 
| ProgramBench | 26.5 | 26.0 | 12.5 | 37.0 | 25.0 | 33.0 | 
| MiMo Code Bench | 63.2 | 61.2 | 40.4 | 68.6 | 59.3 | – | 
| **General Agent** |  |  |  |  |  |  | 
| AutomationBench v1.0.6 | 53.1 | 52.3 | 16.0 | 50.3 | 45.8 | 46.2 | 
| Toolathlon-Verified | 76.9 | 73.6 | 49.1 | 80.6 | 74.9 | 77.9 | 
| GDPval-AA 2.1 | 1673 | – | 1107 | 1708 | 1588 | 1595 | 
| Agents’ Last Exam | 31.6 | 27.6 | 13.2 | 31.6 | 30.8 | 25.7 | 
| Terminal Bench 4.0 | 34.9 | 28.8 | 1.5 | 49.0 | 39.9 | 42.4 | 
| Terminal Bench 2.1 | 89.9 | 87.6 | 65.2 | 89.1 | 88.8 | 84.3 | 
| OSWorld-Verified | 82.0 | 80.8 | – | 83.4 | 83.0 | 86.0 | 
| JobBench | 62.0 | 61.2 | 25.0 | 65.7 | 45.4 | 57.4 | 
| **Cybersecurity** |  |  |  |  |  |  | 
| CyberGym | 94.0 | 95.1 | 40.0 | – | – | – | 
| MiMo Cyber Bench | 80.2 | 77.2 | 0.0 | – | – | – | 
| ExploitGym | 17.8 | 6.0 | 0.2 | 22.1 | 30.3 | 28.4 | 
| ExploitBench | 47.9 | 25.3 | 16.6 | 70.0 | 78.5 | 78.0 | 
| SEC Bench Pro | 66.3 | 47.5 | 17.7 | – | 79.1 | – | 
| **Visual Agent** |  |  |  |  |  |  | 
| MiMo VisualCoding | 72.3 | 71.5 | – | 70.0 | 73.4 | 69.1 | 

**Gains over the previous generation are orders of magnitude larger.** DeepSWE v1.1, which measures the ability to carry out long development tasks, rose from 19.0 to 71.9; AutomationBench, which navigates REST APIs to build business workflows, rose from 16.0 to 53.1; and the newer terminal task benchmark Terminal Bench 4.0 increased from 1.5 to 34.9. All of these are tasks that require “taking multiple steps to see a job through to completion," directly reflecting the impact of large-scale agent-oriented RL.

**When compared to top commercial models, “matching areas" and “areas falling short" are clearly distinct.** Terminal Bench 2.1 (89.9) for terminal operations and AutomationBench (53.1) are the highest in the table, while Agents’ Last Exam (31.6), which gathers difficult problems, ties with Claude Opus 5. DeepSWE (71.9) is only 2.1 points behind Opus 5. On the other hand, ProgramBench, which reconstructs programs from compiled binaries and documents, scores 26.5, falling more than 10 points behind Opus 5’s 37.0, and Terminal Bench 4.0 lags significantly at 34.9 versus 49.0. ExploitBench (47.9) and ExploitGym (17.8), which proceed to actual vulnerability exploitation, fall below the three commercial models in the comparison (70.0–78.5, 22.1–30.3). OSWorld-Verified (82.0), which operates screens, also trails all three commercial models (83.0–86.0), showing a 4-point gap with Claude Fable 5. The table indicates that **while the capability for “diligently executing tasks with prescribed steps" matches top tiers, a gap remains in “analyzing unknown targets to create or break things anew."**

The gap between Pro and Flash is small across most items. DeepSWE is 71.9 vs 67.9, Terminal Bench 2.1 is 89.9 vs 87.6, OSWorld-Verified is 82.0 vs 80.8, and JobBench is 62.0 vs 61.2, with differences within 4 points. Flash scores higher on CyberGym (95.1 vs 94.0). Substantial gaps open up in security attack-side items: ExploitBench is 47.9 vs 25.3, ExploitGym is 17.8 vs 6.0, and SEC Bench Pro is 66.3 vs 47.5, resulting in about a 3x difference in ExploitGym and roughly 1.9x in ExploitBench. Despite having over 3 times the total parameters, Flash delivers most of the performance needed for general coding and business agent use cases.

Comparison conditions require caution. MiMo Code Bench, MiMo Cyber Bench, and MiMo VisualCoding are Xiaomi’s internal benchmarks and cannot be reproduced under identical conditions by third parties. According to technical report notes, CyberGym values were measured after the publisher corrected flaws in the evaluation environment.

## Strengths and Use Cases

The target indicated by the model card and technical report is to serve as **the core for agent workflows involving long sequences of steps**. The model card explains that the 1M token context is intended to feed entire large repositories, lengthy tool execution logs, and multi-step agent work histories all at once.

The training methodology also incorporates design choices tailored to this purpose:

- **Single Mixed RL** : Coding, general agent, visual, and cybersecurity tasks, alongside multiple agent frameworks (harnesses), are mixed into the same training batch. The model card states this reinforces capabilities mutually and transfers strategies to harnesses not seen during training.
- **Large-Scale RL** : Uses fully asynchronous GRPO, running 1,568 problems × 16 attempts per step. According to the technical report, a single step consumes 2.7B to 3.7B tokens.
- **Group Relative Scoring (GRS / GAR)** : Since binary pass/fail does not establish relative quality among successful answers, multiple answers to the same problem are compared to establish grading criteria, allocating rewards to superior answers. The model card states this steers the model toward solving tasks with shorter steps and fewer tokens.
- **Reward Hacking Countermeasures** : Starting from data where the model reflects on and rewrites its own erroneous utterances, reward hacking is suppressed throughout RL via environment hardening, adversarial selection, and cross-checking verifiers.
- **MOPD2** : Following mixed RL, distillation from multiple teacher models expands capabilities onto tasks where correct answers are difficult to judge mechanically.

The technical report notes that training the MoE router (the part deciding which experts to route to) during RL causes load to skew toward specific experts and destabilizes training. Consequently, MiMo-V2.6 freezes the router during RL. Publishing such failure logs is valuable for researchers attempting replication.

## How It Differs from Similar Models

The MiMo-V2.6 series includes three models with different use cases:

- **MiMo-V2.6-Pro-RL** (this article): 1.02T total / 42B active. The series flagship, significantly outperforming Flash particularly on security attack-side tasks.
- **MiMo-V2.6-Flash-RL** : 309B total / 15B active. The model card positions this as “a checkpoint striking a balance with efficiency," and as shown in the table above, the difference from Pro remains within a few points across most items. GGUF version coverage on our site: MiMo-V2.6-Flash-RL-GGUF Multimodal MoE Model: ~141GB Memory
- **MiMo-V2.6-Distill-Qwen-9B** : A smaller model obtained by further training Qwen3.5-9B on data generated by MiMo (total 77.4B tokens). The technical report describes releasing this as “a common starting point for the community to further experiment with RL." SWE-bench Pro increases from the original Qwen3.5-9B’s 32.0 to 44.6, reaching 47.6 when further trained in the published RL environment. GGUF version coverage on our site: MiMo-V2.6-Distill-Qwen-9B-GGUF: Our Test Answers, 12GB+ VRAM

Xiaomi has published not only the weights but also the RL environments used for training (approx. 3,000 coding problems, 1,000 cyber problems, 1,000 knowledge work problems, 2,000 web development problems, plus approx. 1,000 composition tasks), datasets (XiaomiMiMo/MiMo-V2.6-RL-oss), and the training framework (a fork of verl). Providing the materials to reproduce “how it was trained," rather than merely releasing open weights, is the primary distinguishing feature of this series.

## Hardware Requirements

**Estimated requirements (calculated by Local Model Watch)**

| Your VRAM | Quantization | File size | Est. memory needed | 
|---|---|---|---|
| More than 417GB of VRAM (multi-GPU or CPU offload required) | Q2_K | 347.8GB | 417.4GB | 

**Inference engine support** (architecture name matched against each project’s own model registry in its source code, checked 2026-11-20): llama.cpp: registered, vLLM: registered, MLX (mlx-lm): not registered. “Not registered" means the name is absent from that registry today, not that the model cannot run.

*Memory estimates add a 20% runtime overhead (KV cache, etc.) to the actual size of the distributed files. Actual usage varies with context length, batch size and inference engine. These figures are computed by this site from file sizes, not published by the model’s authors. File sizes are measured from the converted build DevQuasar/XiaomiMiMo.MiMo-V2.6-Pro-RL-GGUF. Compare with other models in our VRAM quick reference. What the quantization names mean: glossary.*

## Can You Run It Locally?

**Runs in Ollama, LM Studio and llama.cpp via a converted build.**

The publisher ships safetensors, but DevQuasar/XiaomiMiMo.MiMo-V2.6-Pro-RL-GGUF provides a GGUF build you can use.

**License — `mit` (Commercial use allowed):** Permits commercial use, modification and redistribution, provided the copyright notice and license text are retained.

*Compiled by this site’s code from the published formats, converted builds we have found, and each engine’s own model registry. “Not found" means we have not seen such a build, not that none exists. License summaries are not legal advice — check the publisher’s original terms before relying on them.*

## Our Own Measurements

Values we measured ourselves on our server (no GPU) by actually reading and running this model’s files — not figures copied from the model card. How we measure, and the results for every model: Our Measurements.

### Japanese Token Efficiency

| Tokenizer | Tokens per 1,000 Japanese characters | Ratio to the same text in English | 
|---|---|---|
| **This model** | **688** | **1.26×** | 
| Qwen3 | 688 | 1.26× | 
| Llama 3.2 | 744 | 1.36× | 
| Gemma 3 | 564 | 1.03× | 
| gpt-oss | 795 | 1.45× | 
| LLM-jp-3 | 497 | 0.85× | 

The same Japanese text takes about as many tokens as with the Qwen3 tokenizer.

Counted with the `tokenizer.json` of XiaomiMiMo/MiMo-V2.6-Pro-RL on a fixed text we wrote ourselves (876 Japanese characters across news, conversation, technical docs, a formal email, travel writing and a recipe) and its English translation. Fewer tokens mean more Japanese fits in the context window.

### Inside the GGUF File

File: Q4_K_M/XiaomiMiMo.MiMo-V2.6-Pro-RL.f16.gguf.Q4_K_M.gguf-00001-of-00048.gguf (575.43GB, `Q4_K_M`). We read only the header (metadata) of the file, not the weights.

| Item | Value | 
|---|---|
| Architecture (as named in the GGUF) | `mimo2` | 
| Maximum trained context length | 1,048,576 tokens | 
| Layers | 70 | 
| Experts | 8 active out of 384 | 
| Vocabulary size | 152,576 | 
| Chat template | Included (mentions tool calls, has a thinking switch) | 
| imatrix | Not recorded in the file | 
| Weight types (share of parameters) | Q4_K 83.7% / Q6_K 16.3% / F32 0.0% | 
| Average bits per weight | 4.84 bits | 
| Embedding / output layer type | Q4_K / Q6_K | 

The quant name in the file name describes the file as a whole; in practice layers mix several types. The average bits per weight is the measured file data divided by the number of weights.

## How to Get It

- Distribution format: Transformers format (safetensors) weights are hosted on Hugging Face at `XiaomiMiMo/MiMo-V2.6-Pro-RL` . Downloading does not require agreeing to terms of service. It is also distributed on ModelScope.
- Example download command: `huggingface-cli download XiaomiMiMo/MiMo-V2.6-Pro-RL`
- Supported engines: The model card lists SGLang and vLLM. The SGLang example uses tensor parallelism 16 across a 2-node configuration (`--tp 16 --nnodes 2` ), enabling MTP-based speculative decoding (EAGLE). The vLLM example uses tensor parallelism 8 (`--tensor-parallel-size 8` ) and a Docker image prepared for MiMo-V2.5 (`vllm/vllm-openai:mimov25-cu129` ).
- Recommended sampling settings: `temperature=1.0` ,`top_p=0.95`
- Running on local hardware: As indicated by the multi-GPU and multi-node startup examples, this is not a scale for individual hardware. For personal environments, Flash-RL’s GGUF version or Distill-Qwen-9B are realistic choices. The Pro model card does not mention llama.cpp, Ollama, or LM Studio.
- Using as an API: According to the model card, it is also available via the Xiaomi MiMo Open Platform API and OpenRouter.

## Quantized and Converted Variants

→ Scroll horizontally to see all columns

| Added | Publisher | Format | Repository | Smallest VRAM tier (build, est. memory) | 
|---|---|---|---|---|
| 2026-09-27 | mlx-community | MXFP4 | mlx-community/MiMo-V2.6-Pro-RL-mxfp4-q8 | MLX 4bit 619.0GB (does not fit a single consumer GPU) | 
| 2026-09-29 (2 days after publication) | DevQuasar | GGUF | DevQuasar/XiaomiMiMo.MiMo-V2.6-Pro-RL-GGUF | Q2_K 417.4GB (does not fit a single consumer GPU) | 
| 2026-10-02 (5 days after publication) | ggml-org | GGUF | ggml-org/MiMo-V2.6-Pro-RL-GGUF | Q2_K 468.0GB (does not fit a single consumer GPU) | 

File sizes of each build:

- Available builds in mlx-community/MiMo-V2.6-Pro-RL-mxfp4-q8: MLX 4bit 515.8GB
- Available builds in DevQuasar/XiaomiMiMo.MiMo-V2.6-Pro-RL-GGUF: Q2_K 347.8GB / Q5_K_M 455.7GB / MXFP4 516.1GB / Q4_K_M 575.4GB
- Available builds in ggml-org/MiMo-V2.6-Pro-RL-GGUF: Q2_K 390.0GB / MXFP4 516.1GB

In addition, 6 converted build(s) from other uploaders exist on Hugging Face; this site lists only builds from the model’s publisher or established quantization maintainers.

*This section is appended automatically by Local Model Watch when a converted build of this model appears after publication. Memory figures are estimated from the size of the distributed files. What the quantization names mean: glossary.*

## Related Articles

## What to Read Next

- **Find models by VRAM** (This model does not fit even in 80GB; its smallest build needs about 417GB) → VRAM quick reference, including models over 80GB
- **Explore the same model family** → MiMo-V2.6 family overview (4 articles, 4 converted builds)
- **Engines that run this model** → llama.cpp / Ollama / vLLM
- **What Q2_K mean and where to get this model** → GGUF format guide and models / NVFP4 / MXFP4 format guide and models / Quantization and model-format glossary
- **Learn about the publisher** → Xiaomi (MiMo): models, licenses and articles

## Sources

## Update History

- 2026-09-27: Added converted builds to “Quantized and Converted Variants”: mlx-community/MiMo-V2.6-Pro-RL-mxfp4-q8
- 2026-09-28: Added our own measurements: Japanese token efficiency.
- 2026-09-29: Added converted builds to “Quantized and Converted Variants”: DevQuasar/XiaomiMiMo.MiMo-V2.6-Pro-RL-GGUF
- 2026-09-29: Updated the hardware requirements table with the actual file sizes of DevQuasar/XiaomiMiMo.MiMo-V2.6-Pro-RL-GGUF.
- 2026-09-29: Changed the title to show what the article covers (VRAM requirements, file list, etc.).
- 2026-09-30: Changed the title to show what the article covers (VRAM requirements, file list, etc.).
- 2026-10-03: Added converted builds to “Quantized and Converted Variants”: ggml-org/MiMo-V2.6-Pro-RL-GGUF
- 2026-10-06: Added our own measurements: what is inside the GGUF file.
