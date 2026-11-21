---
id: collect-261121-veille-ia/modeles-open/thinkingmachines-inkling-2-mini-hugging-face
title: "thinkingmachines/Inkling-2-Mini · Hugging Face"
domain: modeles-open
role: reference
task: article
actors: []
dates: ["2026-11-20"]
keywords: ["model", "text", "data", "inkling", "mini", "image", "other", "hugging"]
source: https://huggingface.co/thinkingmachines/Inkling-2-Mini
source_anchor: ""
source_lines: [1, 152]
section: "Modeles_Open"
---
Inkling 2 Mini is a general-purpose multimodal model that accepts text and image inputs and generates text outputs. It is intended for use in English and other languages, and across multiple coding languages. The model is designed to be used by developers building AI-powered applications, including agentic and tool-use systems, coding assistants, chatbots, and retrieval-augmented generation systems, and is suitable for general-purpose conversational use, instruction-following, and other natural language and visual tasks. It is released with open weights to support research, fine-tuning and integration into third-party products by downstream developers.

**Languages:** English, general multilingual support

**Model provider:** Thinking Machines Lab, Inc.

**Date of release:** November 20, 2026

**License:** Apache 2.0

The weights are available for download through Hugging Face here.

Developers can also access the model using the Tinker fine-tuning platform, which provides API access.

- For accessing our model via Tinker, you can get started by referring to the Tinker Cookbook here, and installing the tml-renderers package here.
- Tinker playground also provides inference-only access to the model here.

The model is distributed as a single checkpoint of about 20GB, in mixed NVFP4 and MXFP8 precisions.

Given its small size, it is possible to run this checkpoint on local AI compute for inference. We support:

- 1x NVIDIA DGX Spark
- Apple silicon devices with 32GB or more memory (MacBooks, Mac minis and Studios)
- Other GPUs and platforms are possible, with 32GB or more of VRAM recommended

Inkling 2 Mini supports local inference using the following frameworks, along with their dependency libraries:

- Hugging Face
- SGLang
- vLLM
- TokenSpeed
- NVIDIA Dynamo
- Unsloth, including quantized GGUF versions
- llama.cpp, for quantized GGUF versions
- Llama App
- Ollama and Exo Labs, on Apple silicon

Inkling 2 Mini can also be post-trained with:

Multimodal autoregressive looped transformer

A 24-layer decoder-only transformer with sparse Mixture-of-Experts (MoE) feed-forward layers. The middle 12 layers run three times with shared weights, giving 48 layer applications per token from 24 layers of weights. In each layer, each token is routed to 8 of 128 experts. Attention is a hybrid of local and global layers. The model is natively multimodal – images are encoded via a vision transformer – with image and text inputs projected into a shared hidden space and processed jointly by the decoder.

26.4B, 3.4B active

Note: each extra pass through the looped layers adds about 1.0B effective active parameters. Since the model runs with three passes, there are 5.4B effective active parameters per token.

NVFP4 and MXFP8

- Text: UTF-8 encoded text
- Image: Any pixel-based image input. For optimal performance, each image dimension should be between 40px to 4096px.

Inkling 2 Mini supports a context window of up to 1M tokens.

Text

Training data includes a broad variety of content types, including text, images, video. Training data for the model was drawn from publicly available sources, acquired from third-parties, generated internally, or synthetically generated or augmented. Publicly available data includes content from the public internet and publicly accessible repositories.

Data curation includes cleaning, processing, and modifying datasets. These processing steps, which vary by data type, may include deduplication and filtering to remove junk or other low-quality data, or to advance safety or other objectives.

|  | Inkling 2 Mini | Gemma 4 31B | Muse Glimmer | Qwen3.8-27B | Nemotron 3.5 Super VL | 
|---|---|---|---|---|---|
| Model Info |  |  |  |  |  | 
| Params (B) activated / total | 5/26 | 31/31 | 30/30 | 27/27 | 12/125 | 
| Third Party Index |  |  |  |  |  | 
| AA Index (v5.0) |  | 4.4 | 6.3 |  | 13.6 | 
| Vals Index (v2.2) |  | – | – | – |  | 
| Coding and Autoresearch |  |  |  |  |  | 
| FrontierCode v2.0 stirrup v0.2.1 |  | 0.0% | 1.5% |  | 6.1% | 
| DeepSWE v1.1 mini-swe-agent v2.4.6 |  | 0.3% | 4.4% |  | 24.7% | 
| Terminal Bench v4.0 mini-swe-agent v2.4.6 |  | 0.0% | 0.5% |  | 0.0% | 
| Terminal Bench v2.1 Inkling 2 Mini in pi v0.83.0 |  | 43.4% | 51.7% |  | 60.7% | 
| SWEBench Pro v2 (Hard) mini-swe-agent v2.4.6 |  | 14.2% | 44.3% |  | 44.1% | 
| SWEBench Multilingual Inkling 2 Mini with bash only |  | 43.0% | 66.9% |  | 67.7% | 
| SWEBench Verified bash only |  | 46.1% | 76.0% |  | 71.1% | 
| Vibe Code Bench v1.1 openhands (vals) |  | – | – |  | 3.5% | 
| PMPP-Hard (AUC, 0–10h) pi v0.83.0 |  | – | – |  | – | 
| Agentic Search, Tool Use, and Knowledge Work |  |  |  |  |  | 
| BrowseComp Inkling 2 Mini with compaction |  | 37.0% | 63.9% | 61.6% |  | 
| DeepSearchQA Inkling 2 Mini with compaction |  | 61.7% | 74.6% |  | 77.7% | 
| MCP Atlas Public |  | 54.2% | 75.5% |  | 67.7% | 
| AA-Briefcase v1.1 |  | 369 | 478 |  | 999 | 
| GDPVal-AA v2.1 |  | 627 | 792 |  | 1162 | 
| JobBench |  | 8.2% | 16.1% | 33.4% |  | 
| AutomationBench-AA v1.0.6 |  | 4.9% | 6.8% |  | 14.8% | 
| EnterpriseOps-Gym |  | 28.4% | 34.7% |  | 28.1% | 
| Vals Finance Index |  | – | – | – |  | 
| AA-AnalystAgent |  | – |  | – | – | 
| Diligence Bench Best harness |  | – | 40.9% |  | 45.0% | 
| Vision Interpretation and Creation |  |  |  |  |  | 
| GDP.pdf |  | 6.0% | 11.8% |  | 8.0% | 
| PerceptionBench |  | 40.7% | 42.8% |  | 36.8% | 
| MMMU Pro Standard 10 |  | 73.4% | 74.3% |  | 72.3% | 
| OmniDocBench v1.6 |  | 82.5% | 90.4% |  | 77.9% | 
| ScreenSpot Pro original / with Python |  | 5.9%/– |  | 57.6/78.9% | 43.5%/– | 
| Chartography original / with Python |  | 12.0/11.8% |  | 12.2/52.1% | 9.7/29.3% | 
| ZeroBench (main) original / with Python |  | 9.0/12.0% | 9.0/17.0% |  | 11.0/12.0% | 
| MuirBench |  | 72.3% |  | 65.9% | 72.7% | 
| BVB |  | 47.0% | 50.5% |  | 52.1% | 
| Reasoning, Knowledge, and Epistemics |  |  |  |  |  | 
| HLE-Diamond |  | 7.2% | 9.1% |  | 8.8% | 
| CritPt v2 Full / Hard |  | –/4.0% | –/7.0% | 10.6/4.0% |  | 
| Apex 2025 |  | 5.2% | 4.2% | 18.8% |  | 
| ARC-AGI v1 / v2 |  | – | – |  | 22.7/1.5% | 
| LongBench v2 |  | 52.9% | 33.1% | 52.3% |  | 
| GraphWalks BFS 256K |  | – | – | 50.4% |  | 
| MRCR v2 8-needle 256K–512K |  | – | – |  | 5.7% | 
| AA-Omniscience v1.1 score | -8.6 | -48.5 | -32.2 |  |  | 
| AA-Omniscience v1.1 non-hallucination rate | 63.4% | 14.5% | 16.4% |  |  | 
| ComplexConstraints v1.1 openhands v1.45.0 |  | 31.6% |  | 30.1% | 30.1% | 
| Safety and Alignment |  |  |  |  |  | 
| FORTRESS (adversarial) Public |  | 67.8% |  | 78.8% | 79.6% | 
| FORTRESS (benign) Public | 96.4% |  | 89.1% |  | 92.7% | 
| StrongREJECT (none) | 97.8% | 98.1% |  |  |  | 
| Misbehavior (Internal) ↓ |  |  | 46.4% | 57.7% | 52.8% | 
| Covert misbehavior (Internal) ↓ |  |  | 45.6% | 33.2% | 43.4% | 
| Destructive actions (Internal) ↓ |  | 75.8% | 68.8% |  | 65.6% | 

in each row (lowest where lower is better); equal displayed scores tie in each row; none when first place is tied

We conducted safety evaluations ahead of release, spanning both everyday human-AI interaction and dangerous-capability testing. Because Inkling 2 Mini is multimodal, we paid attention to whether safety behavior held consistently across text and image inputs. We applied mitigations to reduce risks before release, including an extra phase of alignment training using on-policy distillation, similarly to Inkling 2 Small.

For everyday use, we evaluated sycophancy and how the model handles requests involving suicide, self-harm and eating disorders, including whether it encourages harm or affirms false beliefs. We also assessed whether the model refuses genuinely harmful requests without over-refusing benign ones. For CBRN and cyber, we assessed knowledge and procedural uplift through internal evaluations and refusal-suppressed variants intended to estimate latent capability with safeguards removed. For loss of control, we evaluated agentic capability, strategic deception, and sabotage potential, benchmarked against public frontier models, and found the model materially below frontier capabilities.

Inkling 2 Mini is the first looped transformer we have released, so we also studied how looping affects monitorability: whether we can still oversee the model by reading its chain of thought. Looping lets the model do more computation without reasoning in visible tokens. Inkling 2 Mini has very little controllability over its chain of thought, and was unable to perform covert side tasks in realistic agentic environments. Further experiments on the effect of looping on monitorability are described in our blog post.

Across all areas, we concluded that Inkling 2 Mini does not give material uplift beyond what is already available from open-weights models.

Inkling 2 Mini may exhibit general limitations common to foundation models, including hallucination (generating plausible but factually incorrect or unsupported content), occasional failures to follow instructions precisely, and degraded performance in long multi-turn conversations. As with other models trained on web-derived and synthetic data, it may reflect biases present in its training data, including demographic, cultural or linguistic biases, and may perform unevenly across languages, dialects or subject domains that were less represented during training.

Inkling 2 Mini’s knowledge is limited to information available as of its training cutoff, and it may not reflect events or changes that occurred afterward.

Inkling 2 Mini is a small model built to run on local hardware, and some limitations are specific to it:

- Knowledge and factual accuracy. It is less capable than Inkling 2 Small across most of our evaluations, with the largest gaps on factual recall and knowledge-heavy questions. It is more likely than larger models to state incorrect information with confidence.
- Repetitive reasoning. In internal testing the model sometimes became stuck in long reasoning loops, or repeatedly re-checked its own work without making progress, particularly when debugging code. We recommend setting limits on output tokens and agent turns.
- Agentic reliability. When using tools, it is less reliable than larger models at following detailed instructions and project conventions. It may take destructive actions, such as deleting or overwriting files, without confirmation, and may not report the outcome of its work accurately.
- Languages other than English. Output quality can be lower outside English. In testing in Chinese we observed clipped phrasing and mixed languages.
- Sensitive topics. The model can respond with more caution than the situation calls for when a conversation touches on mental health.
- Reasoning and compute. Setting reasoning effort to its minimum does not fully disable reasoning. Because the middle layers run three times, compute per token is higher than the active-parameter count alone suggests.
- Local performance. Support for looped models in local runtimes is new, and speed varies by runtime and hardware.

We recommend that downstream developers and deployers:

- Apply appropriate human oversight and review for outputs used in high-stakes or safety-critical contexts, rather than relying on Inkling 2 Mini’s outputs without verification.
- Run agentic workloads in a sandbox, and require confirmation before destructive or irreversible actions.
- Conduct their own evaluation of Inkling 2 Mini’s performance, safety and fairness for their specific use case, language and population prior to deployment, particularly for applications involving vulnerable groups.
- Implement additional safeguards, such as content filtering, rate limiting and monitoring, at the application layer, especially for open deployment contexts where Inkling 2 Mini’s built-in mitigations may not be sufficient on their own.
- Avoid deploying Inkling 2 Mini in domains such as medical, legal or safety-critical decision-making without additional fine-tuning, domain-specific validation and human oversight.

- Downloads last month
- 7,944
