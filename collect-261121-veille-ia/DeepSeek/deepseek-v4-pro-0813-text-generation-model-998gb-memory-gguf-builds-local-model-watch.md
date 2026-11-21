---
id: collect-261121-veille-ia/deepseek/deepseek-v4-pro-0813-text-generation-model-998gb-memory-gguf-builds-local-model-watch
title: "DeepSeek-V4-Pro-0813 Text Generation Model: ~998GB Memory, GGUF Builds | Local Model Watch"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: ["2026-09-26"]
keywords: ["model", "preview", "version", "deepseek-v4-pro-0813", "text", "guide", "same", "tokens"]
source: https://localmodelwatch.tsuchitsuchi.com/en/2026/09/26/deepseek-v4-pro-0813-released/
source_anchor: ""
source_lines: [1, 199]
section: "DeepSeek"
---
## At a Glance

| Item | Value | 
|---|---|
| Repository | deepseek-ai/DeepSeek-V4-Pro-0813 | 
| Family guide | DeepSeek-V4 guide (2 articles) | 
| Publisher guide | DeepSeek: models and licenses | 
| Published | 2026-08-13 | 
| License | mit | 
| Formats | safetensors | 
| Paper | arXiv:2606.19348 | 
| Source type | Primary source (the publisher itself) | 

*Values determined by this site’s code when the information was collected. Dates are JST.*

**What we checked ourselves**

- The same Japanese text takes about as many tokens as with the Qwen3 tokenizer.

Details and conditions are in “Our Own Measurements” below.

## Overview

DeepSeek has released the official version of its large-scale MoE model, **DeepSeek-V4-Pro-0813**, on Hugging Face. According to the model card, it replaces the previous preview version (DeepSeek-V4-Pro (Preview)) with significantly enhanced agent capabilities, bringing noticeable performance improvements especially in production environments. The structure remains the same as the preview version, with the addition of the **DSpark** module for speculative decoding.

According to the summary of the technical report for the DeepSeek-V4 series, the series is designed to be “a highly efficient model capable of routinely handling 1-million-token contexts." At a 1-million-token context, it is reported to require 27% of the inference compute (FLOPs) per token and 10% of the KV cache compared to DeepSeek-V3.2.

## Specifications

- Parameter Count: According to the technical report summary, DeepSeek-V4-Pro (Preview) is a MoE model with 1.6T total parameters and 49B active parameters. The official 0813 version uses this exact structure.
- Architecture: MoE. To efficiently handle long contexts, it uses a hybrid attention combining Compressed Sparse Attention (CSA) and Heavily Compressed Attention (HCA). Residual connections are enhanced with Manifold-Constrained Hyper-Connections (mHC), and training uses the Muon optimizer.
- Training Data: Pre-trained on 32T+ tokens, followed by extensive post-training (from the preview version technical report summary).
- Context Length: 1 million tokens.
- Speculative Decoding: The DSpark module is bundled into the same checkpoint, eliminating the need for a separate draft model.
- Reasoning Depth: Three levels can be selected via `reasoning_effort` :`low` ,`high` , and`max` .
- Weight Precision: FP8 (from Hugging Face tags).

## Performance

ursery The model card provides a comparison table featuring the official version, the concurrent DeepSeek-V4-Flash-0731, both preview versions, and four models from other companies. The figures are measurements by the publishers and have not been independently verified.

→ Scroll horizontally to see all columns

| Benchmark | DeepSeek-V4-Pro-0813 | DeepSeek-V4-Flash-0731 | DeepSeek-V4-Pro (Preview) | DeepSeek-V4-Flash (Preview) | GLM-5.2 | Kimi K3 | Opus-4.8 | Fable-5 (w/ fallback) | 
|---|---|---|---|---|---|---|---|---|
| HLE (wo / w tools) | 42.7 / 60.0 | 37.8 / 51.5 | 37.7 / 48.2 | 34.8 / 45.1 | 40.5 / 54.7 | 43.5 / 56.0 | 49.8 / 57.9 | 53.3 / 63.0 | 
| Terminal Bench 2.1 | 87.9 | 82.7 | 72.1 | 61.8 | 81.0 | 88.3 | 85.0 | 88.0 | 
| NL2Repo | 61.5 | 54.2 | 38.5 | 39.4 | 48.9 | – | 69.7 | – | 
| Cybergym | 83.3 | 76.7 | 52.7 | 38.7 | – | 80.0 | 78.3 | 83.1 | 
| DeepSWE | 62.7 | 54.4 | 12.8 | 7.3 | 46.2 | 67.5 | 58.0 | 70.0 | 
| Toolathlon-Verified | 74.1 | 70.3 | 55.9 | 49.7 | 59.9 | 76.5 | 76.2 | 77.9 | 
| Agents’ Last Exam | 25.7 | 25.2 | 16.5 | 15.8 | 23.8 | 27.6 | 25.7 | – | 
| AutomationBench (Public) | 31.8 | 25.1 | 12.8 | 10.8 | 12.9 | 30.8 | 27.2 | 29.1 | 
| DSBench-FullStack † | 71.1 | 68.7 | 41.8 | 37.0 | 61.8 | 73.7 | 71.6 | 77.2 | 
| DSBench-Hard † | 67.2 | 59.6 | 31.1 | 25.8 | 54.5 | 63.0 | 71.7 | 68.3 | 

**Gains from the preview version are substantial across all rows.** The gap widens particularly in agent-type tasks, with DeepSWE rising from 12.8 to 62.7, AutomationBench (Public) from 12.8 to 31.8, and internal DSBench-Hard from 31.1 to 67.2. The model card’s description that “agent capabilities have been significantly enhanced" is borne out by this table.

**It trades blows with top models from other companies depending on the metric.** Terminal Bench 2.1, which measures the ability to complete tasks in a terminal, scores 87.9—a close margin within 0.4 points of Kimi K3 (88.3) and Fable-5 (88.0), and outperforming Opus-4.8 (85.0). Cybergym (83.3) and AutomationBench (Public) (31.8) are the highest in the table. On the other hand, HLE, a collection of extremely difficult questions created by domain experts, stops at 42.7 without tools, showing a 7.1 to 10.6 point gap behind Fable-5 (53.3) and Opus-4.8 (49.8) (though with tools it reaches 60.0, ranking second behind Fable-5’s 63.0). NL2Repo falls short of Opus-4.8 by 8.2 points, DeepSWE falls short of Fable-5 by 7.3 points and Kimi K3 by 4.8 points. Against Kimi K3, it scores lower on 6 out of the 10 numeric metrics where both have values. It is fair to read this as **“it ranks among the very top in many agent-type tasks, but a gap remains in knowledge-intensive difficult questions and some coding tasks."**

Attention must also be paid to the comparison conditions. According to the notes in the model card, the code agent tasks in the table are measured using DeepSeek’s own agent infrastructure (DeepSeek Harness in minimal mode) with a reasoning depth of `max`, `temperature = 1.0`, and `top_p = 0.95`. DSBench-FullStack and DSBench-Hard (marked with †) are DeepSeek’s internal test sets and are not evaluations that third parties can verify under the same conditions. The Fable-5 column includes a “w/ fallback" condition.

## Strengths and Use Cases

The model card emphasizes agent capabilities, and the tasks showing the largest gains in the table are those requiring “hands-on execution through to completion," such as terminal operations, repository generation, software patching, workflow automation, and tool usage. The efficiency of “routinely handling 1-million-token contexts" highlighted in the technical report summary is said to make multi-step workflows and heavy token usage for reasoning (test-time scaling) practical.

Reasoning depth can be selected from three levels of `reasoning_effort`. The model card recommends maximum output lengths of up to **384K tokens** for `high` and `max`, presupposing such long outputs for deep reasoning.

On the other hand, it is not suited for running on a reader’s local hardware. The vLLM example in the model card is for execution on **a single node equipped with four GB300s**, and the SGLang example also assumes tensor parallelism of 4. This is a model meant for data-center-grade environments or provider-operated APIs, rather than something to be tested on consumer GPUs.

## How It Differs from Similar Models

Our site previously covered the quantized text generation model DeepSeek-V4-Pro-0813-nvfp4-DSpark: ~1005GB Memory quantized by NVIDIA. That version quantized everything down to NVFP4, including the DSpark draft portion, so it could run on a single checkpoint targeting NVIDIA environments. Today’s 0813 release is DeepSeek’s own original weights upon which that was based.

## Hardware Requirements

**Estimated requirements (calculated by Local Model Watch)** — 1650.5B parameters

| Your VRAM | Quantization | File size | Est. memory needed | 
|---|---|---|---|
| More than 998GB of VRAM (multi-GPU or CPU offload required) | FP8 | 831.4GB | 997.7GB | 

**Inference engine support** (architecture name matched against each project’s own model registry in its source code, checked 2026-11-20): llama.cpp: registered, vLLM: registered, MLX (mlx-lm): not registered. “Not registered" means the name is absent from that registry today, not that the model cannot run.

*Memory estimates add a 20% runtime overhead (KV cache, etc.) to the actual size of the distributed files. Actual usage varies with context length, batch size and inference engine. These figures are computed by this site from file sizes, not published by the model’s authors. Compare with other models in our VRAM quick reference. What the quantization names mean: glossary.*

## Can You Run It Locally?

**Runs in Ollama, LM Studio and llama.cpp via a converted build.**

The publisher ships safetensors, but unsloth/DeepSeek-V4-Pro-0813-GGUF provides a GGUF build you can use.

**License — `mit` (Commercial use allowed):** Permits commercial use, modification and redistribution, provided the copyright notice and license text are retained.

*Compiled by this site’s code from the published formats, converted builds we have found, and each engine’s own model registry. “Not found" means we have not seen such a build, not that none exists. License summaries are not legal advice — check the publisher’s original terms before relying on them.*

## Our Own Measurements

Values we measured ourselves on our server (no GPU) by actually reading and running this model’s files — not figures copied from the model card. How we measure, and the results for every model: Our Measurements.

### Japanese Token Efficiency

| Tokenizer | Tokens per 1,000 Japanese characters | Ratio to the same text in English | 
|---|---|---|
| **This model** | **705** | **1.29×** | 
| Qwen3 | 688 | 1.26× | 
| Llama 3.2 | 744 | 1.36× | 
| Gemma 3 | 564 | 1.03× | 
| gpt-oss | 795 | 1.45× | 
| LLM-jp-3 | 497 | 0.85× | 

The same Japanese text takes about as many tokens as with the Qwen3 tokenizer.

Counted with the `tokenizer.json` of deepseek-ai/DeepSeek-V4-Pro-0813 on a fixed text we wrote ourselves (876 Japanese characters across news, conversation, technical docs, a formal email, travel writing and a recipe) and its English translation. Fewer tokens mean more Japanese fits in the context window.

### Inside the GGUF File

File: Q4_K_M/deepseek-ai.DeepSeek-V4-Pro-0813.Q4_K_M-00001-of-00076.gguf (885.58GB, `Q4_K_M`). We read only the header (metadata) of the file, not the weights.

| Item | Value | 
|---|---|
| Architecture (as named in the GGUF) | `deepseek4` | 
| Maximum trained context length | 1,048,576 tokens | 
| Layers | 61 | 
| Experts | 6 active out of 384 | 
| Vocabulary size | 129,280 | 
| Chat template | Included (mentions tool calls, has a thinking switch) | 
| imatrix | Not recorded in the file | 
| Weight types (share of parameters) | Q4_K 83.8% / Q6_K 16.2% / BF16 0.0% / I32 0.0% / other 0.0% | 
| Average bits per weight | 4.84 bits | 
| Embedding / output layer type | Q4_K / Q6_K | 

The quant name in the file name describes the file as a whole; in practice layers mix several types. The average bits per weight is the measured file data divided by the number of weights.

## Recent Models in the Same Size Class

*Models with* *over 40B* *parameters that Local Model Watch covered recently, listed by code from our article log for comparison. VRAM tiers are this site’s estimates; licenses are as stated on the model cards.*

→ Scroll horizontally to see all columns

| Model | Parameters | Smallest VRAM tier | License | Our article | 
|---|---|---|---|---|
| inclusionAI/Ling-3.1-flash | 572.1B | — | mit | Ling-3.1-flash Hybrid Reasoning MoE Model: ~641GB Memory, GGUF Builds (2026-11-17) | 
| inclusionAI/Ling-3.1-flash-1019 | 572.1B | — | mit | Ling-3.1-flash-1019 Text Generation Model: ~641GB Memory, GGUF Builds (2026-11-17) | 
| inclusionAI/Ling-3.1-flash-base | 572.1B | — | mit | Ling-3.1-flash-base Released: 560B Hybrid Linear MoE Model (2026-11-17) | 
| inclusionAI/Ling-3.0-flash-Sante-fp8 | 127.5B | — | mit | Ling-3.0-flash-Sante-fp8 Text Generation Model: ~144GB Memory (2026-11-17) | 
| nex-agi/Nex-N2.5-Pro | 396.8B | — | apache-2.0 | Nex-N2.5-Pro Long-Horizon Agent Model: ~98GB Memory, GGUF Builds (2026-09-09) | 

## How to Get It

- Distribution Format: Safetensors weights (FP8) are available at Hugging Face’s `deepseek-ai/DeepSeek-V4-Pro-0813` . Downloading does not require agreeing to a terms of use agreement.
- Example Download Command: `huggingface-cli download deepseek-ai/DeepSeek-V4-Pro-0813`
- Supported Engines: The model card guides users toward vLLM and SGLang. Both can enable speculative decoding via DSpark. In vLLM, add `--speculative-config '{"method":"dspark","num_speculative_tokens":7,"draft_sample_method":"greedy"}'` to the launch command, and in SGLang, specify`--speculative-algorithm DSPARK` . SGLang reads the draft weights from the same checkpoint, so`--speculative-draft-model-path` should not be specified.
- Chat Template: This release **does not include** the Jinja-format chat template used by many tools. Instead, the repository’s`encoding` folder contains Python scripts and test cases to convert OpenAI-compatible messages into input strings for the model and parse the outputs. Users must check individual engine compatibility before proceeding.
- Running Locally: The repository’s `inference` folder contains procedures for weight conversion and interactive demos. The model card recommends`temperature = 1.0` , with`top_p` set to 0.95 for agent use cases and 1.0 otherwise.
- GGUF versions or similar formats for running on consumer hardware are not mentioned in the model card.

## Quantized and Converted Variants

→ Scroll horizontally to see all columns

| Added | Publisher | Format | Repository | Smallest VRAM tier (build, est. memory) | 
|---|---|---|---|---|
| 2026-09-26 | unsloth | GGUF | unsloth/DeepSeek-V4-Pro-0813-GGUF | Q4_K_XL 949.6GB (does not fit a single consumer GPU) | 
| 2026-09-26 | unsloth | FP8 | unsloth/DeepSeek-V4-Pro-0813 | FP8 997.7GB (does not fit a single consumer GPU) | 
| 2026-09-26 | DevQuasar | GGUF | DevQuasar/deepseek-ai.DeepSeek-V4-Pro-0813-GGUF | Q2_K 636.4GB (does not fit a single consumer GPU) | 

File sizes of each build:

- Available builds in unsloth/DeepSeek-V4-Pro-0813-GGUF: Q4_K_XL 791.3GB / Q8_K_XL 813.5GB
- Available builds in unsloth/DeepSeek-V4-Pro-0813: FP8 831.4GB
- Available builds in DevQuasar/deepseek-ai.DeepSeek-V4-Pro-0813-GGUF: Q2_K 530.3GB / Q3_K_M 697.0GB / Q4_K_M 885.6GB

In addition, 9 converted build(s) from other uploaders exist on Hugging Face; this site lists only builds from the model’s publisher or established quantization maintainers.

*This section is appended automatically by Local Model Watch when a converted build of this model appears after publication. Memory figures are estimated from the size of the distributed files. What the quantization names mean: glossary.*

## Related Articles

- DeepSeek-V4-Pro-0813-nvfp4-DSpark: ~1005GB Memory
- DeepSeek-V4.1-Flash-1017 Vision-Language Model: ~570GB Memory
- DeepSeek-V4.1-Flash 552B Multimodal MoE Model: ~570GB Memory
- inclusionAI Releases SingProbe Models for Real-Time LLM Guardrails

## What to Read Next

- **Find models by VRAM** (This model does not fit even in 80GB; its smallest build needs about 998GB) → VRAM quick reference, including models over 80GB
- **Explore the same model family** → DeepSeek-V4 family overview (2 articles, 3 converted builds)
- **Engines that run this model** → llama.cpp / Ollama / vLLM
- **What FP8 mean and where to get this model** → GGUF format guide and models / FP8 format guide and models / Quantization and model-format glossary
- **Learn about the publisher** → DeepSeek: models, licenses and articles

## Sources

## Update History

- 2026-09-26: Added converted builds to “Quantized and Converted Variants”: unsloth/DeepSeek-V4-Pro-0813-GGUF, unsloth/DeepSeek-V4-Pro-0813, DevQuasar/deepseek-ai.DeepSeek-V4-Pro-0813-GGUF
- 2026-09-26: Changed the title to show what the article covers (VRAM requirements, file list, etc.).
- 2026-09-28: Added our own measurements: Japanese token efficiency, what is inside the GGUF file.
