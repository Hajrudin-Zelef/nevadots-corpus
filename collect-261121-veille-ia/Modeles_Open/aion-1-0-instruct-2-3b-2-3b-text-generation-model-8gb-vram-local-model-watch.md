---
id: collect-261121-veille-ia/modeles-open/aion-1-0-instruct-2-3b-2-3b-text-generation-model-8gb-vram-local-model-watch
title: "Aion-1.0-Instruct-2.3B 2.3B Text Generation Model: 8GB+ VRAM | Local Model Watch"
domain: modeles-open
role: reference
task: article
actors: []
dates: ["2026-11-19"]
keywords: ["model", "knowledge", "aion-1", "text", "instruct-2", "layers", "size", "math"]
source: https://localmodelwatch.tsuchitsuchi.com/en/2026/11/19/microsoft-aion-1-0-instruct-2-3b/
source_anchor: ""
source_lines: [1, 188]
section: "Modeles_Open"
---
## At a Glance

| Item | Value | 
|---|---|
| Repository | microsoft/Aion-1.0-Instruct-2.3B | 
| Family guide | Aion-1.0 guide (2 articles) | 
| Publisher guide | Microsoft: models and licenses | 
| Published | 2026-11-19 | 
| License | mit | 
| Formats | safetensors | 
| Source type | Primary source (the publisher itself) | 

*Values determined by this site’s code when the information was collected. Dates are JST.*

**What we checked ourselves**

- It needs about 16% more tokens than the Qwen3 tokenizer for the same Japanese text, so only about 0.87× as much Japanese fits in the same context length.

Details and conditions are in “Our Own Measurements” below.

## Overview

Microsoft’s Windows Applied Sciences Group has released “Aion-1.0-Instruct-2.3B", a small language model (SLM) designed for on-device environments.

This model is positioned as a foundational SLM within the Windows ecosystem, optimized specifically for use cases such as text processing and OS integration that respond safely and quickly on the device without requiring constant cloud connectivity. An optimized version combined with Microsoft’s content moderation model is scheduled to be introduced as a Windows OS SLM by the end of January 2027, and this release on Hugging Face aims to ensure transparency and gather user feedback ahead of the upcoming feature rollout. Initially, the unquantized version of the model has been published, with quantized versions planned for the future.

## Specifications

The specifications listed in the model card are as follows:

- Number of parameters: Approx. 2.29 billion (~2.29B, tied input/output embeddings)
- Architecture: Causal language model (instruction-tuned)
- Number of layers: 28
- Hidden size: 2048
- MLP intermediate size: 8192 (SiLU)
- Attention: GQA (24 query heads, 8 key/value heads, head dim 128)
- Attention pattern: Hybrid configuration (21 sliding-window layers [window 2048] + 7 full-attention layers)
- Position encoding: RoPE (θ = 32000), YaRN scaling (factor 16) in full-attention layers
- Context length: 32,768 tokens
- Vocabulary size: 200,029
- Precision: bfloat16

## Performance

The evaluation scores measured by Microsoft itself and published in the model card are as follows. EleutherAI’s lm-evaluation-harness was primarily used for benchmark measurements, and GPT-4o was used as the judge model for tasks requiring LLM-based evaluation.

| Category | Benchmark | Score | Notes | 
|---|---|---|---|
| Knowledge | MMLU | 55.15 |  | 
| Knowledge | MMLU-Pro | 33.35 |  | 
| Knowledge | MMMLU | 45.51 | Multilingual MMLU | 
| Knowledge | GPQA | 28.18 |  | 
| Math | GSM8K | 74.91 | Loose matching | 
| Math | GSMPlus | 49.56 |  | 
| Math | MATH-500 | 48.00 |  | 
| Instruction following | IFEval | 83.90 | Mean of prompt-level strict and instruction-level strict | 
| Instruction following | IFBench | 32.00 | Prompt-level loose | 
| Instruction following | Multi-IF | 64.70 |  | 
| Translation | chrF++ | 43.18 |  | 

These measurement results show good scores in instruction-following capabilities that adhere to formal constraints and basic calculation processing. In particular, it recorded a high score of 83.90 on IFEval, which measures whether it can follow formatting instructions such as “within 300 words" or “in JSON format", possessing practical accuracy for daily text processing and instruction execution. It also recorded 74.91 on GSM8K, which measures the ability to solve elementary school-level math word problems step-by-step.

On the other hand, scores lag behind in areas requiring knowledge or advanced reasoning. It stays at 55.15 on MMLU covering a broad range of knowledge, and remains at low levels such as 33.35 on MMLU-Pro, a more difficult reasoning-focused version, and 28.18 on GPQA, which handles PhD-level difficult science questions. As the publisher itself mentions, due to its small size of 2.3 billion parameters, there are limitations in retaining vast factual knowledge and complex multi-stage reasoning, making its performance tendency more suited for text processing and summarization tasks based on clear instructions rather than uses covering general world knowledge.

## Strengths and Use Cases

According to the model card, Aion Instruct is trained with a focus on general-purpose text use cases such as document reading, content analysis, question answering, and natural language understanding. The tags include summarization, rewriting, and chat, with summarization, rewriting, and dialogue designated as expected use cases. The context length is 32K tokens, making it capable of processing multi-page documents, long email threads, and complex technical specifications entirely on-device.

Supported languages include 8 languages: English, German, Spanish, Portuguese, Italian, French, Chinese, and Japanese. However, multilingual data accounts for only about 6% of the total training data, and the publisher states that output quality may vary depending on the language.

Tool calling is also supported. Passing tool definitions via `apply_chat_template(..., tools=tools)` causes the model to output calls in the `<|tool_call|>` format.

There are also points to note. The card explicitly states that this model has not been trained to excel in world knowledge. Because it is a small model with 2.3 billion parameters, details may be missed, erroneous descriptions may occur, and complex multi-stage reasoning may fail. It may also be susceptible to adversarial attacks, with XPIA, DIC, and ECI cited as key safety metrics to be improved in the future. Users are asked not to use it for automated decision-making in critical scenarios such as medical diagnosis or legal judgment.

## How It Differs from Similar Models

Our site’s “Aion-1.0-Instruct-2.3B-GGUF" 2.3B Text Generation Model: Required VRAM 4GB+ covers a version where the same model is reported to have been converted into GGUF format by AtomicChat. In that article, the source is unverified and it is treated as an unofficial converted version. This time, it is the unquantized safetensors version released by Microsoft itself, based on official primary information. Converted versions serve as an option when you want to run things easily in a lighter format. However, the quality of quantized versions needs to be verified separately from the official scores in this article.

## Hardware Requirements

**Estimated requirements (calculated by Local Model Watch)** — 2.3B parameters

| Your VRAM | Quantization | File size | Est. memory needed | 
|---|---|---|---|
| 8GB (RTX 4060 / 3060 Ti, etc.) | BF16 | 4.3GB | 5.1GB | 

**Inference engine support** (architecture name matched against each project’s own model registry in its source code, checked 2026-11-21): llama.cpp: not registered, vLLM: not registered, MLX (mlx-lm): not registered. “Not registered" means the name is absent from that registry today, not that the model cannot run.

*Memory estimates add a 20% runtime overhead (KV cache, etc.) to the actual size of the distributed files. Actual usage varies with context length, batch size and inference engine. These figures are computed by this site from file sizes, not published by the model’s authors. Compare with other models in our VRAM quick reference. What the quantization names mean: glossary.*

## Can You Run It Locally?

**Not usable in Ollama, LM Studio and llama.cpp yet.**

The publisher ships safetensors only, and llama.cpp’s registry does not list this architecture. llama.cpp would need to add support before these tools can run it. 2 converted build(s) from other uploaders exist. Today it can be run with transformers, using the memory figures in the table above.

**License — `mit` (Commercial use allowed):** Permits commercial use, modification and redistribution, provided the copyright notice and license text are retained.

*Compiled by this site’s code from the published formats, converted builds we have found, and each engine’s own model registry. “Not found" means we have not seen such a build, not that none exists. License summaries are not legal advice — check the publisher’s original terms before relying on them.*

## Our Own Measurements

Values we measured ourselves on our server (no GPU) by actually reading and running this model’s files — not figures copied from the model card. How we measure, and the results for every model: Our Measurements.

### Japanese Token Efficiency

| Tokenizer | Tokens per 1,000 Japanese characters | Ratio to the same text in English | 
|---|---|---|
| **This model** | **795** | **1.45×** | 
| Qwen3 | 688 | 1.26× | 
| Llama 3.2 | 744 | 1.36× | 
| Gemma 3 | 564 | 1.03× | 
| gpt-oss | 795 | 1.45× | 
| LLM-jp-3 | 497 | 0.85× | 

It needs about 16% more tokens than the Qwen3 tokenizer for the same Japanese text, so only about 0.87× as much Japanese fits in the same context length.

Counted with the `tokenizer.json` of microsoft/Aion-1.0-Instruct-2.3B on a fixed text we wrote ourselves (876 Japanese characters across news, conversation, technical docs, a formal email, travel writing and a recipe) and its English translation. Fewer tokens mean more Japanese fits in the context window.

## Recent Models in the Same Size Class

*Models with* *up to 4B* *parameters that Local Model Watch covered recently, listed by code from our article log for comparison. VRAM tiers are this site’s estimates; licenses are as stated on the model cards.*

→ Scroll horizontally to see all columns

| Model | Parameters | Smallest VRAM tier | License | Our article | 
|---|---|---|---|---|
| ggml-org/LFM3-1.7B-GGUF | 1.7B | 4GB | lfm1.0 | LFM3-1.7B-GGUF Text Generation Model: Our Test Answers, 4GB+ VRAM (2026-11-18) | 
| openbmb/MiniCPM-V-4.7-35B-A3B-DSpark-gguf | 389M | 4GB | apache-2.0 | MiniCPM-V-4.7-35B-A3B-DSpark-gguf Draft Model for Speedup: 4GB+ VRAM (2026-11-17) | 
| openbmb/MiniCPM-V-4.7-1B-DSpark-gguf | 214M | 4GB | apache-2.0 | MiniCPM-V-4.7-1B-DSpark-gguf Text Generation Model: 4GB+ VRAM (2026-11-17) | 
| IFM/Jais-3-0.9B-GGUF | 1.1B | 4GB | apache-2.0 | Jais-3-0.9B-GGUF Bilingual Dense Model: Our Test Answers, 4GB+ VRAM (2026-11-17) | 
| nvidia/NVIDIA-Nemotron-Labs-Dory1.0-4B-BF16 | 4.0B | 4GB | openmdw-1.1 | NVIDIA Releases Dory1.0-4B: A Recurrent 4B LLM with Dynamic Loops (2026-11-17) | 

## How to Get It

The distribution format is safetensors (bfloat16), and the license is MIT. Gates (consent to terms of use) are not set, and it can be downloaded without consent. The card lists PyTorch, ONNX, and GGUF as library formats, but Microsoft states it has released the unquantized version first and plans to release quantized versions later. What is currently in the repository is safetensors.

Because it uses custom modeling code, `trust_remote_code=True` is required. It is written for `transformers` 5.x.

```
from transformers import AutoModelForCausalLM, AutoTokenizer
model_id = "microsoft/Aion-1.0-Instruct-2.3B"
tokenizer = AutoTokenizer.from_pretrained(model_id, trust_remote_code=True)
model = AutoModelForCausalLM.from_pretrained(
    model_id,
    dtype="auto",
    device_map="auto",
    trust_remote_code=True,
)
```
The chat template uses `<|system|>`, `<|user|>`, and `<|assistant|>`, and each turn ends with `<|end|>`. `<|end|>` is also the EOS token. The card contains no description regarding operation in llama.cpp, Ollama, or vLLM. Therefore, compatibility with these engines is unverified. microsoft/Aion-Instruct-Preview-Sample is listed in the card as a related GitHub repository.

## Quantized and Converted Variants

→ Scroll horizontally to see all columns

| Added | Publisher | Format | Repository | Smallest VRAM tier (build, est. memory) | 
|---|---|---|---|---|
| 2026-11-19 | microsoft | INT4 | microsoft/Aion-1.0-Instruct-2.3B-ONNX-WebGPU | – | 

In addition, 2 converted build(s) from other uploaders exist on Hugging Face; this site lists only builds from the model’s publisher or established quantization maintainers.

*This section is appended automatically by Local Model Watch when a converted build of this model appears after publication. Memory figures are estimated from the size of the distributed files. What the quantization names mean: glossary.*

## Related Articles

- Aion-1.0-Instruct-2.3B-GGUF 2.3B Text Generation Model: 4GB+ VRAM
- Mage-VL-Cowork Vision-Language Model: 12GB+ VRAM, GGUF Builds
- UserRepGen-4B Text Generation Model: Our Test Answers, 4GB+ VRAM
- SingVL_S1_4B Multimodal Decision Model with Image Support: 4GB+ VRAM

## What to Read Next

- **Find models by VRAM** (This model runs from the 8GB tier) → Other models that run on a 8GB GPU
- **Explore the same model family** → Aion-1.0 family overview (2 articles, 1 converted builds)
- **Formats this model is available in** → Safetensors format guide and models
- **Learn about the publisher** → Microsoft: models, licenses and articles
- **Other models for the same task** → Other text generation models

## Sources

- microsoft/Aion-1.0-Instruct-2.3B (Hugging Face)
- “Aion-1.0-Instruct-2.3B-GGUF" 2.3B Text Generation Model: Required VRAM 4GB+ (Local Model Watch)

## Update History

- 2026-11-19: Added converted builds to “Quantized and Converted Variants”: microsoft/Aion-1.0-Instruct-2.3B-ONNX-WebGPU
- 2026-11-20: Added our own measurements: Japanese token efficiency.
