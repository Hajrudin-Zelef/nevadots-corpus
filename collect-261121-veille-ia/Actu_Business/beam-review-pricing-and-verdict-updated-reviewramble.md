---
id: collect-261121-veille-ia/actu-business/beam-review-pricing-and-verdict-updated-reviewramble
title: "Beam review: pricing and verdict (updated) | ReviewRamble"
domain: actu-business
role: reference
task: article
actors: []
dates: []
keywords: ["model", "beam", "reflection", "index", "tokens", "output", "scores", "models"]
source: https://www.reviewramble.com/models/reflection-beam
source_anchor: ""
source_lines: [1, 77]
section: "Actu_Business"
---
## What is Beam?

Beam is the first open-weight model from Reflection, the New York lab founded in 2024 by former Google DeepMind researchers Misha Laskin and Ioannis Antonoglou. Reflection announced it on 5 October 2026 and published the weights on Hugging Face on 16 October under the Apache 2.0 licence, in BF16, FP8 and NVFP4, with a 1.93B-parameter DFlash2 draft model for speculative decoding. The full model ID is Beam-1.0-501B-A23B.

It is a text-only mixture-of-experts model with 501.3 billion parameters, of which 23.4 billion are active per token (8 of 256 experts), a 1M-token context and a knowledge cutoff of 30 June 2026. Reasoning is always on, with five effort levels from low to max. It uses a modified version of OpenAI's harmony chat format and was developed mainly for English.

Reflection's own API is a free public beta with daily usage limits and no published post-beta price. Hosted endpoints run from $0.24 per million input tokens and $0.98 per million output on OpenRouter, and Together AI and Baseten both charge $0.40 / $1.20.

## Is Beam worth it?

Beam's case rests on efficiency rather than raw ability. Artificial Analysis scores it 30 on its Intelligence Index, second among US open-weight models behind Meta's Muse Spark Open (39) and ahead of NVIDIA's Nemotron 3.5 Super VL (26), but only tenth among all open models. The Chinese leaders are a tier above: MiMo-V2.6-Pro scores 46, GLM-5.3 45 and Kimi K3 44 on the same index. Reflection's own launch post concedes that Kimi K3 is ahead on raw capability.

What it does better is finish a task in fewer words. Artificial Analysis counts about 26,000 output tokens per index task, half the 52,000 of the UAE's K2 Horizon, which scores one point higher, and it measured 364.3 output tokens a second, second of 78 models in its class. Running the whole index cost $0.23 per task. Reflection claims scores comparable to GLM-5.2 for three to four times less inference compute, but that figure is its own estimate from active parameters and generated tokens, not a measurement.

The self-reported numbers on the model card are strong: 81.6% on SWE-bench Verified, 80.1% on Terminal Bench v2.1 and 90.6% on GPQA Diamond. Independent testing is less kind on agentic coding. Artificial Analysis records 3.0% on Terminal-Bench 4.0, behind Qwen3.8 27B (5.6%), and in Reflection's own Mirror harness Beam scores 28 on the Coding Agent Index, 15 points behind the next setup and 40 behind the leaders. Its hallucination rate on AA-Omniscience is 47%, against 30% for Qwen3.8 27B, even though it answers more questions correctly (33%).

The licence is the strongest argument for it. Apache 2.0 carries no revenue cap or attribution rule, where Muse Spark Open requires attribution in each product and a separate deal for large model-as-a-service providers, and the model card explicitly permits distillation and training on its outputs. Artificial Analysis also puts it top of US open models on its Cyber Index at 34%. Reflection's documentation shows the most practical use of that freedom: teaching a small model such as Gemma 4 E4B to reason briefly before calling tools, which cut that model's output tokens by 45% in Reflection's example.

Self-hosting is not small. The NVFP4 checkpoint alone is 273 GiB, and Reflection's serving guides assume four NVIDIA B300 GPUs on NVLink. For a team that needs an American, Apache-licensed reasoning model larger than gpt-oss-120b, Beam is the obvious candidate; for a team that only needs the best open model per dollar, GLM-5.3 and MiMo-V2.6-Pro still score about 15 points higher.

## Where it earns its place

- An ML team at a US software company whose legal department blocks Chinese-origin models ### Distil a large reasoning model into a small one that runs on cheap hardwareApache 2.0 and the model card's explicit permission for output training make Beam a clean teacher. Reflection's TRL recipe fine-tunes Gemma 4 E4B on tool-calling conversations whose reasoning Beam wrote, reporting 45% fewer output tokens and 3.7 more points on BFCL v4 in a 14 to 19 minute run on one node.
- A platform team running research and automation agents through a hosted API ### Keep the output-token bill down on long reasoning workloadsArtificial Analysis puts Beam at $0.23 per index task, helped by about 26,000 output tokens a task. A month of 1 billion input and 100 million output tokens costs about $520 on Together AI at $0.40 / $1.20, or about $350 through Nebius on OpenRouter at $0.25 / $1.00, before cache hits.
- A developer evaluating open models for a coding agent ### Decide whether Beam is good enough to replace a paid coding modelStart on Reflection's free beta API or in Mirror CLI, then compare with your current model on your own repositories. Expect a gap: Artificial Analysis scores Beam 28 on its Coding Agent Index and 3.0% on Terminal-Bench 4.0, well short of Reflection's self-reported 80.1% on Terminal Bench v2.1.

## Before you commit

- Setup
- Reflection's OpenAI-compatible API at api.reflection.ai/openai/v1 with model Beam-1.0-501B-A23B, or Mirror CLI for terminal coding. Self-hosting has documented SGLang, vLLM and TokenSpeed recipes on four B300 GPUs, plus a Helm chart for Kubernetes.
- Integrations
- Tool calling, structured outputs and reasoning effort on Reflection's API and OpenRouter. Hosts include Together AI, Baseten, Nebius, CoreWeave, DigitalOcean and Io Net on OpenRouter, AWS SageMaker and Google Cloud Model Garden; Azure and Oracle are listed as coming soon.
- Data and privacy
- Apache 2.0, with Reflection's Usage Policy recommended alongside it. On Reflection's API, each response's reflection-data-retention header reports zero, standard or contributor retention; only contributor terms, which an organisation must accept, allow training on your requests.
- Cost at scale
- 1 billion input and 100 million output tokens a month cost about $520 on Together AI or Baseten and about $350 on Nebius. Self-hosting needs 273 GiB for NVFP4, 468.72 GiB for FP8 or 933.81 GiB for BF16 before any KV cache.
- Limits
- Text only. 1M-token context; 131,072 output tokens on the Reflection API, where the default budget is 32,768 tokens including reasoning and effort defaults to max. Beta daily allowances are not published, and a 47% hallucination rate means answers need checking.

## Whatâs good

- Apache 2.0, distillation and output training explicitly allowed
- About 26k output tokens per Artificial Analysis task, half K2 Horizon's
- 364 tokens a second median on Artificial Analysis
- Hosted from $0.24 / $0.98 per 1M tokens; Reflection API free in beta

## Whatâs not

- Scores 30 on Artificial Analysis, about 15 below GLM-5.3 and MiMo-V2.6-Pro
- 47% hallucination rate on AA-Omniscience
- 3.0% on Terminal-Bench 4.0 and 28 on the Coding Agent Index
- Text only; NVFP4 build still needs four B300s

## Best for

Teams that want a large reasoning model from a US lab under Apache 2.0, especially to distil into smaller models or to run cost-sensitive agents where output tokens dominate the bill.

## Beam alternatives

## Beam: common questions

- Is Beam worth it?
- Beam is the strongest American open model with a genuinely open licence, and its thrift with tokens is real, but the Chinese open models it is pitched against still score well ahead of it. We rate it 3.8 out of 5.
- How much does Beam cost?
- Free Apache 2.0 weights. Reflection API free in public beta with daily limits. Hosted: Together AI and Baseten $0.40 in / $0.04 cached / $1.20 out per 1M tokens; OpenRouter from $0.24 / $0.98.
- What are the downsides of Beam?
- Scores 30 on Artificial Analysis, about 15 below GLM-5.3 and MiMo-V2.6-Pro; 47% hallucination rate on AA-Omniscience; 3.0% on Terminal-Bench 4.0 and 28 on the Coding Agent Index; text only; NVFP4 build still needs four B300s.
- Who is Beam best for?
- Teams that want a large reasoning model from a US lab under Apache 2.0, especially to distil into smaller models or to run cost-sensitive agents where output tokens dominate the bill.
- How much does Beam cost for a team or in production?
- 1 billion input and 100 million output tokens a month cost about $520 on Together AI or Baseten and about $350 on Nebius. Self-hosting needs 273 GiB for NVFP4, 468.72 GiB for FP8 or 933.81 GiB for BF16 before any KV cache.
- Is Beam safe for company data?
- Apache 2.0, with Reflection's Usage Policy recommended alongside it. On Reflection's API, each response's reflection-data-retention header reports zero, standard or contributor retention; only contributor terms, which an organisation must accept, allow training on your requests.
- What are the best Beam alternatives?
- The closest alternatives we have reviewed are gpt-oss-120b, DeepSeek-V4.1-Flash, Muse Spark Open, Nemotron 3.5 Super VL, GLM-5.3, MiMo-V2.6-Pro, Kimi K3, MiniMax-M3 and Qwen3.8-27B.

Answers checked Oct 26, 2026.
