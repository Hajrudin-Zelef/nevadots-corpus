---
id: collect-261121-veille-ia/modeles-open/reflection-releases-beam-a-501b-open-model-under-apache-2-0
title: "Reflection releases Beam, a 501B open model under Apache 2.0"
domain: modeles-open
role: reference
task: article
actors: []
dates: ["2026-10-16"]
keywords: ["model", "beam", "reflection", "hugging", "face", "under", "apache", "token"]
source: https://dev.to/mikefluff/reflection-releases-beam-a-501b-open-model-under-apache-20-4o14
source_anchor: ""
source_lines: [1, 19]
section: "Modeles_Open"
---
Reflection, a new US lab, has released its first model, Beam, and published it on Hugging Face. Hugging Face's launch post, dated October 16, 2026, describes it as a text-only model aimed at coding and agentic use. It is released under the Apache 2.0 license.

Beam is a mixture-of-experts model with 501B parameters in total, of which 23B are active for each token. It has 256 experts per MoE layer and routes each token to 8 of them. The context window is 1M tokens. It supports five reasoning levels (low, medium, high, xhigh and max), with medium as the default, and it has a dedicated grammar for tool calling. The model ships with its own open-source harness written in Rust.

The post explains that memory needs follow the full 501B size, while compute per token follows the 23B active parameters. Most layers use a 512-token sliding window, and only 9 of 52 layers attend to the whole context. At the full 1M context, the key-value cache is about 36 GiB per sequence, against 208 GiB if every layer were global.

Three checkpoint formats are published: BF16 (about 1 TB), FP8 and NVFP4. NVFP4 brings the weights down to about 280 GB but runs natively only on NVIDIA Blackwell GPUs. FP8 is the option for older GPUs such as the H200. The authors suggest four B200s as a practical setup, which leaves room for long contexts and concurrent requests. Other options they list are an 8xB200 node, two B300s, or 8xRTX Pro 6000.

Transformers, vLLM and SGLang support it from day one. Without Blackwell hardware, Beam is also available through Hugging Face Inference Providers, powered by Baseten, Fireworks and Together, and as a one-click Inference Endpoints recipe in FP8 or NVFP4.

The figures below come from the Reflection team, not from independent testing. For the BF16 checkpoint they include 81.6% on SWE Bench Verified, 80.1% on Terminal Bench v2.1, 90.6% on GPQA Diamond and 80.8% on MCP Atlas. Results for FP8 and NVFP4 are within a few points of BF16 on most tests.

Hugging Face also ran five informal tasks at low and high reasoning effort, which it says are not benchmarks. At high effort all five passed. At low effort, one long-context code task and one math task fell short. In the code task the answer did not match the parser's format, and in the math task the requested justification was missing.

The post covers fine-tuning with TRL and reinforcement learning with Miles. LoRA supervised fine-tuning fits on a single node of 8 B300 GPUs. After one epoch on a multilingual reasoning dataset, Beam reasoned in the requested language on 16 of 16 held-out prompts, against 3 of 16 for the base model. Full-parameter GRPO training needs 96 H200 GPUs in the reference setup, and LoRA GRPO needs 32.

Reflection is also offering $10,000 in Hub credits for the best projects built on Beam and shared on the Hub.

## Top comments (0)
