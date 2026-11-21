---
id: collect-261121-veille-ia/muse/muse-spark-open
title: "Muse Spark Open"
domain: muse
role: reference
task: article
actors: ["Meta"]
dates: []
keywords: ["muse", "spark", "open", "model", "weights", "nvidia", "bf16", "open-weight"]
source: https://dev.meta.ai/docs/muse-spark-open
source_anchor: ""
source_lines: [1, 28]
section: "Muse"
---
Muse Spark Open is the open-weight release of Muse Spark, Meta's agentic and coding model. Run it two ways:

| Model | Parameters | Architecture | Context window | 
|---|---|---|---|
| Muse Spark Open | 816B total, 42B active | Sparse mixture of experts, multimodal (text, image, audio, and video input; text output) | 1,048,576 tokens | 

Muse Spark Open is a sparse mixture-of-experts transformer with built-in perception encoders. Each token routes to a small subset of experts, so per-token compute tracks the 42B active parameters. Serving still loads the full 816B weights into VRAM, which sets the hardware footprint.

Muse Spark Open was trained on data from more than 100 languages.

Muse Spark Open ships in two checkpoint formats:

- **BF16:** the full-precision reference, about 1.65 TB of weights. Serves on eight GPUs, such as 8 × B300.
- **NVFP4:** stores most weights in 4-bit and keeps some layers in BF16, about 537 GB of weights. Serves on a single 4-GPU NVIDIA Blackwell node.

Serving needs memory beyond the weights for the key-value cache, activations, and runtime overhead: budget roughly 2 TB of VRAM for BF16. Get the model has the download, and Hardware platforms covers the NVIDIA, AMD, and Intel configurations, with sample commands.

Muse Spark Open is released under the Muse Model License(opens in new tab). Use is also subject to the Acceptable Use Policy(opens in new tab).

The Muse Spark Open launch ecosystem includes:

- **Model catalogs:** Amazon SageMaker AI, Google Cloud Model Garden, and Microsoft Foundry Model Catalog
- **Inference platforms:** Baseten, Fireworks AI, Groq, Modal, Nebius, and Together AI
- **Tooling:** Hugging Face, llama.cpp, MLX, NVIDIA TensorRT-LLM, SGLang and RadixArk, Unsloth, and vLLM and Inferact
- **Silicon:** AMD, Apple, Intel, and NVIDIA
- **Gateway:** OpenRouter

Now that you know what the open-weight release offers, pick a checkpoint and download the weights. When you're ready to serve, choose a runtime, then tune throughput and output quality with the recommended settings.
