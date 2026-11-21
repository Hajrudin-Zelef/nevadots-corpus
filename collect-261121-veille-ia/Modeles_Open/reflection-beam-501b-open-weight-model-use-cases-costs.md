---
id: collect-261121-veille-ia/modeles-open/reflection-beam-501b-open-weight-model-use-cases-costs
title: "Reflection Beam: 501B open-weight model, use cases, costs"
domain: modeles-open
role: reference
task: article
actors: []
dates: ["2026-10-16"]
keywords: ["beam", "reflection", "open-weight", "model", "weights", "artificial", "analysis", "index"]
source: https://builtbynpcs.com/stories/reflection-beam-501b-open-weight-model-use-cases-costs
source_anchor: ""
source_lines: [1, 72]
section: "Modeles_Open"
---
## TL;DR

Reflection released the weights of Beam, a 501B-parameter open-weight model with 23B active parameters, under Apache 2.0 on 16 October 2026. Artificial Analysis scores it 30 on its Intelligence Index, second among US open-weight models, and says K2 Horizon, one point ahead, uses about twice the output tokens.

## On this page

Reflection released the weights of Beam, a 501B-parameter open-weight model with 23B active parameters, under Apache 2.0 on 16 October 2026. Artificial Analysis scores it 30 on its Intelligence Index, second among US open-weight models, and says K2 Horizon, one point ahead, uses about twice the output tokens.

Beam is now available (Reflection)The launch post. Partners, deployment options, Mirror CLI and customization paths.reflection.ai â
## What was released

Beam is Reflection's first open-weight model. It has 501B total parameters and 23B active per token across 256 experts, takes text in and gives text out, and supports a 1M-token context, per its model card. Reflection released the instruction-tuned weights in BF16, FP8 and NVFP4, plus a speculative-decoding drafter, under Apache 2.0 on Hugging Face. It announced the model on 5 October, and the weights followed on 16 October.

The launch post lists more than 40 partners. Day-zero support covers Transformers, vLLM, SGLang, Unsloth, Axolotl and NVIDIA Dynamo. Hosted access comes from Reflection's own API, which is free to start in public beta, and from AWS, Azure, Google Cloud, Databricks, Baseten, Together AI, Fireworks, Modal, OpenRouter, Cloudflare, Vercel and others. Dell and HPE deliver it for on-premises and air-gapped sites. Reflection also open-sourced Mirror CLI, a terminal coding agent it trained Beam alongside, and says Beam works with Pi, OpenCode and Hermes.

## How it scores

Reflection's model card reports 81.6% on SWE-bench Verified, 80.1% on Terminal Bench v2.1, 66.1% on SWE Bench Pro v1 and 43.3% on DeepSWE v1.1 for the BF16 weights, with the FP8 and NVFP4 versions close behind on most rows. Those are Reflection's own runs.

Artificial Analysis scores Beam 30 on its Intelligence Index. That is second among US open-weight models, behind Meta's Muse Spark Open at 39, and one point below the UAE's K2 Horizon at 31. K2 Horizon uses about 52k output tokens per task against Beam's 26k. Artificial Analysis also puts Beam's Cyber Index at 34%, the best of the US open-weight models it has tested, and 28 on its Coding Agent Index when Beam runs in Mirror.

The two sets of numbers read differently. Beam's 43.3% on DeepSWE v1.1 sits well below the 61% to 74% that Paper Instruments reports for GLM-5.3 Flash, Claude Fable 5, GPT-5.6 Sol and others on the same benchmark's leaderboard, though harnesses and attempt counts differ. Our Ultramarine story covers that table.

## Where it could be used

- Coding agents inside a company's own network. Apache 2.0 carries no per-token fee, and Dell and HPE deliver Beam for on-premises and air-gapped sites. The weights alone are roughly 1 TB in BF16, 500 GB in FP8 and 250 GB in NVFP4, my arithmetic from 501B parameters, before memory for a long context.
- High-volume agent loops where output tokens drive the bill. The token efficiency Artificial Analysis measured matters most when an agent writes a lot. The cost per task still depends on the host.
- Tuning to a domain. Reflection's model card says it does not prohibit fine-tuning, distillation or training other models on Beam's outputs. Partners including NVIDIA, Hugging Face, Anyscale, RadixArk, Unsloth and Axolotl ship reinforcement learning and fine-tuning workflows. A self-distillation recipe, which generates traces on your tasks, trains a LoRA adapter on the verified ones and repeats, is promised in the coming days.
- Defensive security work. The usage terms allow defensive security research on systems you are authorized to test, and Beam leads the US open-weight models on Artificial Analysis's Cyber Index. Open weights keep sensitive logs on your own servers, the reason Hugging Face used an open-weight GLM model for forensics in our GLM-Legal story. Our XBOW story shows what agents can do on the attack side.
- Long-repository work. The weights support a 1M-token context, and OpenRouter and AI Gateway list the full 1,048,576 tokens. The free variants cap output at about 131,000 tokens.
- A daily coding agent. Mirror runs Beam from a terminal, with resumable conversations, parallel agents in separate worktrees, and connections to Slack, Linear and GitHub. On OpenRouter, the apps sending Beam the most tokens so far are the Pi coding agent (76.1M), Lemonade, an AI tool for Roblox games (43.1M), Claude Code (11M) and Hermes Agent (7.0M).

## What it costs, and where it runs

Self-hosting has no licence fee, so the cost is hardware and operations. Hosted prices vary widely. OpenRouter lists four providers for Beam, and its headline price of $0.25 per million input tokens and $1.00 output is the slowest host's rate.

| Host on OpenRouter | Input per 1M | Output per 1M | Cache read per 1M | Median latency | Throughput | Uptime, 3 days | 
|---|---|---|---|---|---|---|
| Nebius Token Factory | $0.25 | $1.00 | None | 23.95 s | 4 tokens/s | 83.40% | 
| Together | $0.40 | $1.20 | $0.04 | 10.94 s | 13 tokens/s | 89.34% | 
| CoreWeave | $1.00 | $3.00 | $0.10 | 0.53 s | 151 tokens/s | 86.82% | 
| Baseten | $1.30 | $4.30 | $0.13 | 0.25 s | 290 tokens/s | 100% | 

The fastest host costs about five times the cheapest on input and 4.3 times on output, so a price comparison needs the speed column beside it. OpenRouter reports 99.62% availability for Beam when it can route between hosts, against 90.34% without that failover. Beam is not offered through its EU in-region routing.

Vercel's AI Gateway has two Beam names. `reflection/beam` is free for a limited time when Reflection serves it, and the same name moves to paid pricing with no downtime when the launch period ends. `reflection/beam-free` stops serving at that point. The free variant caps output at 131,072 tokens, against 1,048,576 for the standard name. The gateway passes provider prices through with no markup and no platform fee, including on bring-your-own-key requests, and it lists Together's rates as $0.40 and $1.20. `npx vercel ai-gateway setup` points Claude Code, Codex or Cursor at it. OpenCode's Zen and Go plans also offer a free `beam-free` with about 1M context and no end date announced.

For scale, the Gemini 4 Argon and Muse Wonder comparison lists those closed models at $4 and $20 per million tokens.

## What to watch

- The spread between suites. An 81.6% on SWE-bench Verified beside an index score of 30 shows how differently benchmarks rate the same model.
- Safety numbers are Reflection's own. Its Iris suite shows Beam refusing or pivoting on 88.9% of harmful requests and complying with 81.5% of harmless ones in BF16.
- The end of free access. Vercel's launch period for `reflection/beam` has no stated end date, and the same name bills at provider rates afterward, so budget for the change.
- Pending pieces. The self-distillation recipes are still to come, and the model is text only.
- Rivals. Muse Spark Open scores 39 on the same index.

## Sources

1. Beam is now available (Reflection, 16 Oct) and Beam model card: specs, partners, releases, benchmarks, safety figures and usage terms.
2. Reflection's Beam on Artificial Analysis and model page: Intelligence Index, Cyber Index, Coding Agent Index and token use.
3. Beam on OpenRouter: per-host prices, latency, throughput, uptime, context and top apps.
4. Beam on Vercel AI Gateway, the free-launch changelog and the free variant: model names, free period, output limits and pricing policy.
5. OpenCode's Beam Free note: the free tier on Zen and Go.

## Continue reading

DeepInfer is DeepSeek's open-source inference engine (Apache-2.0, built mainly on vLLM). The release runs only DeepSeek-V4.1-Flash, offline, on NVIDIA SM100 GPUs or Huawei Ascend 950 NPUs. It has no KV cache system and no online serving. The $38 per GPU-hour figure is unverified.

Postiz, an AGPL-licensed social media scheduler, shows $290,273 MRR from 8,498 active subscriptions on TrustMRR, verified through Stripe. Anyone can self-host it free. The paid cloud sells approved social apps, AI quotas and no maintenance.

Ten inference providers charge $0.34 to $1.12 per million tokens for the reference open model used here (Qwen3.8-27B), as of 20 Oct 2026, and Cerebras is about ten times faster than a typical host. An RTX 5090 pays back against them in 8 to 44 months only at heavy use.
