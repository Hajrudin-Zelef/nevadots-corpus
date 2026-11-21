---
id: collect-261121-veille-ia/modeles-open/reflection-releases-open-weights-for-beam-a-501b-parameter-mixture-of-experts-model-under-apache-2-0
title: "Reflection releases open weights for Beam, a 501B-parameter mixture-of-experts model, under Apache 2.0"
domain: modeles-open
role: reference
task: article
actors: []
dates: []
keywords: ["beam", "reflection", "model", "index", "open", "about", "weights", "scored"]
source: https://www.at-pulse.com/stories/3054/
source_anchor: ""
source_lines: [1, 111]
section: "Modeles_Open"
---
AI-assisted, not human-edited. Methodology Corrections Who owns this site

## **Rising**How it’s measured

Hacker News · 19 likes · 2 comments

Reach blends public engagement (views, likes, comments, shares, stars) from the platforms where the story or its original source lives: YouTube, Hacker News, GitHub, Substack and similar. A video and the article written from its transcript share one score, taken from the video. Platforms with no data are left out rather than counted as zero, and the age of a story never lowers its score.

Beam is now downloadable in BF16, FP8 and NVFP4 checkpoints, with day-0 support in transformers, vLLM and SGLang and published fine-tuning and RL recipes. Its license permits commercial use, fine-tuning and distillation on a team's own hardware. [1] [7]

On Artificial Analysis's Intelligence Index, Beam scored 30, second among US open-weight models behind Muse Spark Open at 39. It used about half the output tokens of K2 Horizon, which scored only one point higher. The same testing measured a 47% hallucination rate on AA-Omniscience. [2]

Reflection reports 81.6% on SWE-bench Verified and 3–4× less inference compute than GLM-5.2. The compute estimate counts only generated tokens and ignores prefill, attention and serving costs. Independent coding results are weaker: a Coding Agent Index of 28 and 3.0% on Terminal-Bench 4.0. [1] [2] [3] [5]

Measured results show Beam scoring 30 on the Intelligence Index while using about half the output tokens of a close rival.

## Read the full assessment

That suggests lower per-task cost for self-hosting teams, but the 47% hallucination rate argues for safeguards in reliability-critical uses.

## Executive brief

Reflection, a US lab, has released the weights of its first model, Beam (501B total parameters, 23B active per token), under Apache 2.0. It is a frontier-scale open model built for coding and agentic work. In Artificial Analysis's independent testing, Beam scored 30 on the Intelligence Index, second among US open-weight models behind Meta's Muse Spark Open (39). It used about half the output tokens of K2 Horizon, which scored one point higher. The same testing found a 47% hallucination rate on AA-Omniscience and a 3.0% score on Terminal-Bench 4.0 (Artificial Analysis). Reflection says it was trained from scratch (Reflection).

## What changed and event timeline

1. ### Beam announced, weights held backReflection says Beam matches GLM-5.2 on reasoning with 3–4× less inference compute. Access started as an early-access program, with weights promised later in October (;).
2. ### Early skepticismDataCamp pointed out that every score came from Reflection and that the model wasn't yet on Hugging Face, OpenRouter or Ollama. It also found Beam behind Kimi K3 on DeepSWE and BrowseComp ().
3. ### Nvidia acquisition talks reportedTechTimes, citing unnamed sources, reported early talks about Nvidia acquiring, acqui-hiring or investing further in Reflection. Nvidia had already backed the company with about $800M. The report says the talks could collapse ().
4. ### First independent benchmarkArtificial Analysis scored Beam 30 on its Intelligence Index, using about 26k output tokens per task. It led US open-weight models on the Cyber Index at 34% ().

## Capabilities and access

- **Model:**`reflection/Beam-1.0-501B-A23B` , plus`-FP8` and`-NVFP4` variants. Text in, text out, 1M-token context, 152k vocabulary, knowledge cutoff June 30, 2026 (model card).
- **Reasoning effort:** five levels, from low to max.
- **Hosted access:** HF Inference Providers (Baseten, Fireworks, Together) and one-click Inference Endpoints.

## Read the full section

- **Model:**`reflection/Beam-1.0-501B-A23B` , plus`-FP8` and`-NVFP4` variants. Text in, text out, 1M-token context, 152k vocabulary, knowledge cutoff June 30, 2026 (model card).
- **Reasoning effort:** five levels, from low to max. Reasoning can be kept across turns, tool calls use a dedicated XML-style grammar, and a DFlash2 speculative-decoding drafter is available (HF blog).
- **Hosted access:** HF Inference Providers (Baseten, Fireworks, Together) and one-click Inference Endpoints.
- **Hardware:** the NVFP4 checkpoint is about 280 GB, and four B200s are the suggested setup. The BF16 checkpoint is about 1 TB.
- **Language:** built mainly for English (model card).

## Technical analysis for researchers and developers

- **Layers:** 52 in total, 3 dense and 49 MoE. Experts hold 98.4% of all parameters.
- **Attention:** a hybrid of 43 sliding-window layers (512 tokens, RoPE) and 9 global layers.
- **Residual stream:** sublayer outputs are added with weights of 1/√k, which keeps the stream at a constant scale with depth.

## Read the full section

Details from the HF blog:

- **Layers:** 52 in total, 3 dense and 49 MoE. Each MoE layer has 256 experts, and 8 are routed per token. Routing follows DeepSeek-V3's group-limited scheme but has no shared expert. Experts hold 98.4% of all parameters.
- **Attention:** a hybrid of 43 sliding-window layers (512 tokens, RoPE) and 9 global layers. The global layers use no positional encoding and instead apply a "scalable-softmax" scaling to queries. As a result, the KV cache grows by about 36 KiB per token instead of 208 KiB, or roughly 36 GiB at 1M tokens.
- **Residual stream:** sublayer outputs are added with weights of 1/√k, which keeps the stream at a constant scale with depth. Logits are capped with tanh at ±30.
- **Post-training recipes:** LoRA SFT runs on one 8×B300 node. Full-parameter GRPO in Miles needs 96 H200s and 2 TB of NVMe scratch per trainer node.

## Claims and evidence

- **Efficiency vs. GLM-5.2 (3–4×):** this is Reflection's own estimate, calculated as 2 × active parameters × generated tokens.
- **Coding scores (SWE-bench Verified 81.6%, Terminal Bench v2.1 80.1%):** reported by Reflection (HF blog).
- **Coding in independent tests:** Artificial Analysis gives Beam a Coding Agent Index of 28, 15 points behind Claude Code running Qwen3.8 Max (AA).

## Read the full section

- **Efficiency vs. GLM-5.2 (3–4×):** this is Reflection's own estimate, calculated as 2 × active parameters × generated tokens. It excludes prefill, attention cost and serving overhead (Reflection). Artificial Analysis independently supports the narrower point that Beam uses few tokens (AA).
- **Coding scores (SWE-bench Verified 81.6%, Terminal Bench v2.1 80.1%):** reported by Reflection (HF blog). Figures published at the Oct 5 announcement were slightly different, for example 80.9% on SWE-bench Verified (DataCamp).
- **Coding in independent tests:** Artificial Analysis gives Beam a Coding Agent Index of 28, 15 points behind Claude Code running Qwen3.8 Max (AA).
- **Hugging Face's own tests:** what HF calls "vibe evals" were five tasks run once each, with traces published, and HF explicitly says they are not benchmarks.

## Context and prior work

- **Chinese labs:** many of the strongest open models have come from Chinese companies, and Beam is pitched as a US alternative (SiliconANGLE).
- **Training run:** 23.8T tokens of pretraining on 6,144 GB300s in about 4 weeks.
- **Funding:** SiliconANGLE reports a $25B valuation and a $6.3B compute deal with SpaceX.

## Read the full section

- **Chinese labs:** many of the strongest open models have come from Chinese companies, and Beam is pitched as a US alternative (SiliconANGLE).
- **Training run:** 23.8T tokens of pretraining on 6,144 GB300s in about 4 weeks. RL then ran on about 10.5K GB300s and produced more than 100M rollouts (Reflection).
- **Funding:** SiliconANGLE reports a $25B valuation and a $6.3B compute deal with SpaceX.
- **Architecture lineage:** the router is borrowed from DeepSeek-V3.

## Limitations, safety and contested findings

- **Hallucination:** Artificial Analysis found a 47% hallucination rate on AA-Omniscience and summarized Beam as "knows more, guesses more" (AA).
- **Terminal tasks:** the 3.0% on Terminal-Bench 4.0 sits alongside Reflection's 80.1% on v2.1.
- **Safety (vendor-reported):** on Reflection's in-house Iris suite, Beam refused 88.9% of harmful requests and complied with 81.5% of harmless ones (model card).

## Read the full section

- **Hallucination:** Artificial Analysis found a 47% hallucination rate on AA-Omniscience and summarized Beam as "knows more, guesses more" (AA).
- **Terminal tasks:** the 3.0% on Terminal-Bench 4.0 sits alongside Reflection's 80.1% on v2.1. These are different benchmark versions, so the numbers can't be compared directly.
- **Safety (vendor-reported):** on Reflection's in-house Iris suite, Beam refused 88.9% of harmful requests and complied with 81.5% of harmless ones (model card).
- **Comparisons with Kimi K3:** DataCamp found Beam behind on agentic search and multi-step coding (DataCamp).

## Business and practitioner implications

- **Ownership:** Apache 2.0 allows commercial use, fine-tuning and distillation, so teams can run a frontier-scale model on their own hardware.
- **Memory vs. speed:** you need memory for 501B parameters, but per-token compute is that of a 23B model.
- **Cost:** low token use points to cheaper cost per task. Reliability-critical uses need extra safeguards because of the hallucination rate.

## Read the full section

- **Ownership:** Apache 2.0 allows commercial use, fine-tuning and distillation, so teams can run a frontier-scale model on their own hardware.
- **Memory vs. speed:** you need memory for 501B parameters, but per-token compute is that of a 23B model. In practice that means Blackwell hardware for NVFP4, or 8–16 H200s for FP8/BF16.
- **Cost:** low token use points to cheaper cost per task. Reliability-critical uses need extra safeguards because of the hallucination rate.
- **Licensing risk:** if Nvidia acquires Reflection, Nvidia would decide the license for future versions (TechTimes).
