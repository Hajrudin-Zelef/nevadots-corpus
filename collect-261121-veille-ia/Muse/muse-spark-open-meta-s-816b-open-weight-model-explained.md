---
id: collect-261121-veille-ia/muse/muse-spark-open-meta-s-816b-open-weight-model-explained
title: "Muse Spark Open: Meta's 816B Open-Weight Model Explained"
domain: muse
role: reference
task: article
actors: ["Meta"]
dates: []
keywords: ["muse", "spark", "open", "model", "kimi", "meta", "card", "qwen3"]
source: https://x9m8.com/muse-spark-open-meta-816b-open-weights-license-hardware/
source_anchor: ""
source_lines: [1, 70]
section: "Muse"
---
Meta published the weights of Muse Spark Open on Hugging Face on October 15, 2026: a mixture-of-experts model with 816 billion total parameters, 42 billion active on each token and a one-million-token context window. Meta’s own release post calls it “the most capable US open-weight model to date.”

*Video: “Meta’s New Models & Agent,” from Sam Witteveen on YouTube. The specifications in this article come from the written sources listed below.*

## What Meta released on October 15

The package is a checkpoint, a license and a distribution plan. Weights ship in two formats, BF16 and NVFP4, a four-bit floating-point format. Beyond Hugging Face, Meta lists downloads on Amazon SageMaker AI, Google Cloud Model Garden and Microsoft Foundry Models, and hosted access through Baseten, Fireworks AI, Groq, Modal, Nebius, OpenRouter and Together AI. Supported inference engines are SGLang, vLLM, TensorRT-LLM, Hugging Face Transformers, Apple MLX, llama.cpp and Unsloth.

The PyTorch Foundation announcement adds lineage. It says the model shares a base with Muse Spark 1.3, follows the August Muse Spark release, and is tuned for agentic coding: code generation, debugging, codebase understanding and tool calling. That post gives no coding benchmark scores, so the coding claims rest on the numbers Meta placed on the model card.

Third parties moved within hours. According to a report on the launch, compressed community builds appeared on Hugging Face the same day, including MLX 3-bit and 4-bit conversions from mlx-community and GGUF files for llama.cpp from Unsloth and ggml-org. Those builds matter because the official files are far too large for ordinary hardware.

## Architecture on the model card

The model card describes a Mixture-of-Experts Transformer paired with a Perception Encoder. The language model has 62 layers, a hidden dimension of 8,192, 128 query heads sharing 16 key-value heads through grouped-query attention, and a vocabulary of 202,048 tokens. Each MoE layer routes among 256 experts and activates 8 per token, which is how an 816-billion-parameter model spends the compute of a 42-billion-parameter one. The Hugging Face metadata counts 822 billion parameters, a discrepancy the card does not explain; the encoder, listed at 1.8 billion parameters with 50 layers, may account for part of the difference, though the card does not say so.

Input can be text, image, video or audio; output is text only. Training data covers more than 100 languages, and the card cautions that the model has not been evaluated on all of them. Reasoning effort has four settings, low, medium, high and xhigh, with xhigh as the default. The recommended sampling values are temperature 1.0 and top-p 1.0, with top-k disabled.

Active parameters set the cost of each token. Total parameters set the cost of keeping the model in memory at all. That split explains most of what follows.

## Benchmarks against Chinese open models

The card compares Muse Spark Open at xhigh effort with Kimi K3 and GLM 5.3 at max effort, Qwen3.8-2.4T-A95B at xhigh, and Inkling. Every figure is reported by Meta; the card is the only source located for these exact rows. Selected results, with the three Chinese models listed in that order (Qwen3.8, GLM 5.3, Kimi K3):

- MCP-Atlas, a tool-use benchmark: Muse Spark Open 89.2; Qwen3.8 84.5, GLM 5.3 84.2, Kimi K3 82.3.
- JobBench: Muse Spark Open 61.8; Qwen3.8 52.7, GLM 5.3 61.4, Kimi K3 54.3.
- HLE-Diamond without tools: Muse Spark Open 26.1; Qwen3.8 17.3, GLM 5.3 16.4, Kimi K3 22.2.
- ZeroBench with tools: Muse Spark Open 50.0; Kimi K3 41.0.
- OSWorld Verified: Muse Spark Open 80.4; Kimi K3 84.8.
- Terminal-Bench 2.1: Muse Spark Open 76.4; Qwen3.8 82.0, GLM 5.3 83.9, Kimi K3 85.0.
- DeepSWE 1.1: Muse Spark Open 60; Qwen3.8 57, GLM 5.3 67, Kimi K3 68.
- GPQA Diamond: Muse Spark Open 89.0; Qwen3.8 93.5, GLM 5.3 91.7, Kimi K3 93.5.

The pattern is uneven. Muse Spark Open leads on tool orchestration and on the multimodal and no-tools reasoning rows listed above, and trails on terminal work, repository-level software engineering and graduate-level science questions. On the Artificial Analysis Intelligence Index, the card lists it at 39, against 40 for Qwen3.8, 44 for Kimi K3 and 45 for GLM 5.3. A comparison table from Computing for Geeks, checked on October 22, 2026, puts the same index at 45 for DeepSeek V4.1-Flash, so the model sits below the open-weight leaders on that composite.

A vendor-chosen comparison set shapes any such table. The rows come from Meta, the models were run under Meta’s settings, and independent reproductions had not appeared in the sources reviewed.

## The license and the access gate

Open weights do not mean an open license. Meta describes the Muse Model License as carrying limited commercial terms that let many developers use the model for free. The card spells those terms out. Products, documentation or settings must display “Built with Muse Spark Open.” A company whose combined gross revenue with its affiliates exceeds $20 million in any 12-month period must notify Meta and sign a separate agreement before offering the model as a service. An attached usage policy restricts illegal activity, weapons and deceptive content, and the model is not intended for users under 18.

Coverage in The New Stack made the same point in its own terms: the weights are downloadable, but the license is more restrictive than the open label suggests.

Access has its own friction. The card requires a login and a form asking for a full legal name, date of birth and the full name of the user’s organization with corporate identifiers. According to the card, submissions cannot be edited afterward, and the information is handled under the Meta Privacy Policy. The Let’s Data Science analysis flagged the form as a privacy cost of getting the weights.

The contrast with Meta’s earlier release is sharp. Muse Glimmer, a 30-billion-parameter model published on August 10, uses Apache 2.0, which Artificial Analysis notes is the most permissive license Meta has applied to its own open models.

## What it takes to run it

The card puts the NVFP4 checkpoint at no less than 700 GB of GPU memory and the BF16 checkpoint at about 2 TB. Those figures place self-hosting in the range of multi-node GPU clusters. The same memory economics drive the supply story covered in x9m8’s earlier look at the memory supercycle and HBM4, and the cost questions raised in its piece on Mistral Large 4’s one-trillion-parameter preview apply here in the same form.

Meta’s answer for desktop users is Apple hardware. Its post says the model runs on two Apple M5 Ultra Mac Studios using MLX, each with up to 512 GB of unified memory and 1.2 TB/s of memory bandwidth. Two machines together hold about 1 TB, which fits the NVFP4 build with room left for context.

On data-center GPUs, the PyTorch post describes Helion kernels for operations such as RoPE, RMSNorm and quantization. It reports speedups of 1.03 to 3.95 times over custom CUDA and CuTe DSL kernels on NVIDIA GB300. On AMD MI350 it gives no kernel figure, only a 3 percent end-to-end throughput gain. Fine-tuning is covered too: TorchTitan gained a model definition for Muse Spark Open with multi-turn supervised fine-tuning and LoRA, and reinforcement learning runs through TitanRL.

## Safety claims and what remains unverified

Meta applied extra safety post-training because open weights leave no provider-side filter in place, and says it evaluated the model under its Meta Superintelligence Scaling Framework and met the release thresholds. The card reports a StrongREJECT v2 score of 0.5, where lower is better, against 62.9 for GLM 5.3, 65.6 for Qwen3.8, 66.6 for Kimi K3 and 86.4 for Inkling. It also lists a 99.2 percent cybersecurity jailbreak refusal rate on an internal benchmark, 94.8 percent across three public benchmarks, a 98.1 percent honesty rate on MASK and a 0 percent misbehavior rate on an agentic misalignment test.

The gap between 0.5 and the 60s is large enough to deserve scrutiny. Meta ran these evaluations itself, and its post says the full evaluation report was still to come. The card itself recommends deploying the model with system-level guardrails. Safety post-training on weights that anyone can download is also a different proposition from safety on a hosted API, since a user with the weights can fine-tune them; the card does not report results for fine-tuned variants.

## Where it sits in Meta’s lineup and the open field

Muse Spark Open follows Muse Glimmer, which Artificial Analysis described as Meta’s first open-weights release since Llama 4 and scored 35 on its index. Meta also kept a closed tier: according to Let’s Data Science, Muse Wonder Full arrived on October 12, three days before the open release, and is sold only inside paid coding subscriptions.

Scale context comes from Hugging Face’s mid-year report on open models, summarized by TUN. It found that in 2026 the monthly ceiling for Chinese open-weight model size ran between 754 billion and 2.78 trillion parameters. At 816 billion, Muse Spark Open lands near the bottom of that range in raw size, while Meta’s pitch rests on active-parameter efficiency, multimodal input, a US origin and the safety numbers above.

What decides adoption now is practical. A 700 GB floor, a revenue-gated license and a personal-data form narrow the field of users. The community quantizations and the Mac Studio path widen it. Independent benchmark runs, which were pending at the time of the release, will show whether the scores on the card hold.

[…] update landed two days after Meta published its Muse Spark Open weights. x9m8’s piece on Muse Spark Open covers that release, and the two make a useful pair for […]
