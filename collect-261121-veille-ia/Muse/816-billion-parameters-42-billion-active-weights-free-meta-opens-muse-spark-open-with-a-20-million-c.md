---
id: collect-261121-veille-ia/muse/816-billion-parameters-42-billion-active-weights-free-meta-opens-muse-spark-open-with-a-20-million-c
title: "816 Billion Parameters, 42 Billion Active, Weights Free: Meta Opens Muse Spark Open — With a $20 Million Clause"
domain: muse
role: reference
task: article
actors: ["Meta"]
dates: []
keywords: ["meta", "billion", "model", "weights", "parameters", "muse", "spark", "open"]
source: https://www.gurujicorp.org/blog/meta-muse-spark-open.html
source_anchor: ""
source_lines: [1, 27]
section: "Muse"
---
Meta released the weights of an 816-billion-parameter model that activates only 42 billion parameters per token, while independent scoring puts it at 39 on the Artificial Analysis Intelligence Index — behind Xiaomi's open-weight MiMo-V2.6-Pro at 46.

Eight hundred and sixteen billion parameters, forty-two billion of them switched on for any single token, and the weights available to download from yesterday. Meta's release of Muse Spark Open is being described by the company itself as the most capable open-weight model to come out of a US lab (Meta AI Research). The independent number sits next to it: 39 on the Artificial Analysis Intelligence Index, ranked ninth of 118 models (Artificial Analysis), while the top open-weight score belongs to Xiaomi's MiMo-V2.6-Pro at 46 (open-weight comparison). Both statements are true. The US record fell; the world record did not move, and that gap is the most useful thing to hold on to before anyone re-platforms on launch-day excitement.

**The release.** On 15 October Meta published the weights for Muse Spark Open: a natively multimodal mixture-of-experts model with 816 billion total parameters, 42 billion active per token and a one-million-token context window, aimed squarely at coding and long-running agent work (Meta AI Research). Text, images, audio and video go in; text comes out, so a single model can read a screenshot, a voice note or a screen recording in the same conversation (Hugging Face technical post).

**The architecture, and why the headline number misleads.** Each layer routes to 256 experts and fires eight of them per token, with roughly 97% of the model's parameters living in those experts rather than in attention (Hugging Face technical post). Only 42 billion parameters do work on any token, but all 256 experts must stay resident: the BF16 checkpoint is about 1.5 TB, a full one-million-token context costs roughly 266 GB of KV cache in BF16, and Meta's own guidance runs the BF16 weights across four nodes of eight H200 GPUs while the 4-bit NVFP4 build fits a single B200 node (same source).

**Where you can actually get it.** API access went live at Baseten, Fireworks AI, Groq, Modal, Nebius, OpenRouter and Together AI, with weights on Hugging Face, Amazon SageMaker AI, Google Cloud Model Garden and Microsoft Foundry, and support for SGLang, vLLM, TensorRT-LLM, Transformers, Apple's MLX, llama.cpp and Unsloth across AMD, Apple, Intel and NVIDIA silicon (Meta AI Research). Artificial Analysis currently lists four serving providers at $1.25 per million input tokens and $4.25 per million output tokens, running at 103 output tokens per second (Artificial Analysis).

**The desktop story.** Meta says this is the first model of its class to run on a desktop, using two Apple Mac Studio machines with M5 Ultra chips — 512 GB of unified memory each, 1 TB between them — through the latest MLX release, and reports driving agent workflows on-device, from shipping iOS changes in an open-source client to reviewing footage frame by frame in DaVinci Resolve (same source). It is a genuine engineering result and a hardware showcase at once: a single-machine deployment is not what 1.5 TB of weights looks like.

**The licence is the part buyers should read.** The Muse Model License, dated 15 October, permits use, copying, modification, fine-tuning, derivative works, distillation, publication and sale, and Meta claims no ownership over generated outputs (model card and licence). Two conditions carry weight: products built on it must display "Built with Muse Spark Open" in the interface or documentation, and any business offering the model to third parties as a service must notify Meta and sign a separate agreement once aggregate gross revenue passes US$20 million in any consecutive twelve-month period (same source). Its usage policy also prohibits use by people under 18 and bars military, warfare, nuclear and espionage applications as well as the operation of critical infrastructure, transportation technologies or heavy machinery (same source).

**What the model card does not say.** Meta added safety post-training so the open weights hold up without the guardrails Meta's own products wrap around them, and published custom Helion GPU kernels that it says deliver 1.03–3.95× kernel-level speedups on NVIDIA and AMD GPUs (PyTorch blog). Reasoning cannot be switched off — the lowest effort setting is the floor, which sets a floor on token spend too (Hugging Face technical post).

**Also this week, from India.** NVIDIA and IIT Madras's AI4Bharat lab described Nemotron-pretraining-Indic-v1: roughly 2.8 trillion tokens across the 22 languages listed in India's Constitution — the first languages of 97% of Indians, close to 1.2 billion citizens at the 2011 Census — with native-script data for all 22 languages, romanised text for twelve, and an open, GPU-accelerated pipeline for translation, quality assessment and repair that is releasing alongside it (Hugging Face blog).

**Buy the tokens, not the hardware.** A 1.5 TB checkpoint plus 266 GB of cache for a full context makes self-hosting a datacentre decision with capex attached, not a workstation experiment. The realistic entry point is one of the hosted providers at $1.25/$4.25 per million tokens — a real number to put against your current flagship bill before anything migrates.

**Read the licence against your revenue and your use case.** The $20 million clause only bites when you resell model access, but the "Built with Muse Spark Open" label applies from day one, and the prohibition on operating critical infrastructure matters for anyone in manufacturing, logistics or OT-adjacent workflows. Neither is a problem to discover after the feature ships.

**Benchmark the claim, not the geography.** "Most capable US open-weight model" is a geographically scoped sentence, and on the independent index this model sits at 39 while Xiaomi's open-weight entry scores 46 (Artificial Analysis). Where the model genuinely earns attention is multimodal input in an agentic loop — screenshots, voice notes and recordings through one interface — which is exactly what support, training and document workflows contain.

We run that comparison on your own files rather than on a launch blog: candidate models against your actual volume, measured as cost per finished task, with the licence screened against your revenue and your deployment so the label and revenue clauses are settled before launch. When the next model lands, the numbers are re-run rather than assumed.

Contact us for a model evaluation on your own workloads.
