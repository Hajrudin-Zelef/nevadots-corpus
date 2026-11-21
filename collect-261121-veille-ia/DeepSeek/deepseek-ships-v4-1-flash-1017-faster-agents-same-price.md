---
id: collect-261121-veille-ia/deepseek/deepseek-ships-v4-1-flash-1017-faster-agents-same-price
title: "DeepSeek Ships V4.1-Flash-1017: Faster Agents, Same Price"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: ["2026-10-17"]
keywords: ["deepseek", "checkpoint", "same", "flash-1017", "table", "still", "open-weight", "ships"]
source: https://rits.shanghai.nyu.edu/ai/deepseek-ships-v4-1-flash-1017-faster-agents-same-price/
source_anchor: ""
source_lines: [1, 27]
section: "DeepSeek"
---
**DeepSeek replaced V4.1-Flash with DeepSeek-V4.1-Flash-1017 on October 17, 2026**, shipping the new checkpoint to its website, mobile app and API the same day and publishing MIT-licensed weights on Hugging Face and ModelScope. DeepSeek calls it a minor update; it improves all eleven benchmarks in the company’s comparison table, shortens outputs, and leaves API pricing unchanged.

## What Changed Since September

The architecture is unchanged. September’s release introduced the Causal Encoder-Decoder design â a 552-billion-parameter backbone that activates 8B parameters per token during prefill and 16B during decode â along with the compressed-sparse-attention scheme and FP4 KV caching that hold a one-million-token context in 890 bytes per token. The 1017 checkpoint moves only the post-training: DeepSeek says it used reinforcement learning with self-feedback (RLSF), in which the model’s own self-critiques supply the reward signal instead of human raters.

Agentic coding is where that shows. DeepSWE v1.1 rises to 77.0 Pass@1 from 74.2, ahead of Claude Opus 5 (74.0) and GPT-5.6 Sol (73.0) in the same table. Terminal-Bench 4.0 jumps from 31.2 to 42.3, ProgramBench from 20.3 to 29.1, Automation-Bench v1.0.6 from 54.8 to 60.5 and Apex Bench v1.1 from 64.6 to 72.1. Humanity’s Last Exam, without tools, goes from 36.8 to 42.6.

The table also shows what still loses to Opus 5: Humanity’s Last Exam (56.3), Terminal-Bench 4.0 (51.8), ProgramBench (37.0) and NL2Repo-Bench (75.3). On the published DeepSWE v1.1 curve, 1017 reaches 77.0% at under $0.20 per task and still clears 72% at roughly $0.10 in its low reasoning setting; its predecessor scored 74.2% at about $0.50, while GPT-6 Astra, Gemini 3.8 Flash and Opus 5 sit between $2 and more than $10. Shorter outputs produced that saving, not cheaper tokens â per-token pricing ($0.15 per million input, $0.60 per million output off-peak, doubling at peak) did not move.

## Where the “Open-Weight Leader” Claim Comes From

The framing that travelled with the release â that this is now the strongest open-weight model â does not come from Artificial Analysis. It comes from the Agentic Coding Index published by ai-leaderboard.dev, a third-party composite that blends DeepSWE v1.1, Terminal-Bench 4.0, FrontierCode 1.1, SWE-bench Pro and CodeArena WebDev Elo into one weighted score. It is an aggregation of other people’s numbers on a single axis.

The better-known independent suites are narrower. Vals AI placed the September checkpoint first among open-weight models on the Vals Index at 57.86% â 15th of 56 overall, 0.05 points ahead of Kimi K3 at 57.81%, and $0.30 per test against Kimi’s $6.47. Artificial Analysis still carries only that release: 39 on the Intelligence Index, 9th of 120 and well above the median of 19 for comparable open-weight systems, but last of 46 on verbosity. No independent suite has published a 1017 score yet. On BenchLM’s aggregate the 1017 checkpoint is the top DeepSeek model at 65.6 of 100 â 35th of 224 overall.

## Running the 1017 Weights Locally

Quantised builds appeared within two days. Unsloth has a Dynamic GGUF set (UD-Q4_K_XL at 501 GB, UD-Q8_K_XL at 508 GB, still flagged work in progress), and 59 quantised repositories now derive from the checkpoint. The route for a single machine is the ds4/DwarfStar build: a 2.36-bits-per-weight quant with 8-bit attention projections and Engram tables copied byte for byte, giving 151.77 GiB of main weights plus 188.83 GiB of FP8 Engram tables â 340.60 GiB in one file â alongside a 0.90 GiB vision encoder. Because ds4 streams Engram rows from disk instead of loading the whole table, that runs on a 128 GB machine and reaches up to 20 tokens per second with expert caching. The build pins checkpoint revision `1bae0e10` and ships SHA-256 checksums.

## What This Means

A checkpoint that gains on eleven benchmarks at unchanged per-token prices is a reminder that agentic cost is set by how many tokens a model emits, not by the price of each one. Cutting output length while raising accuracy is the cheapest gain available, and DeepSeek keeps taking it: V4-Pro traffic moved to V4.1-Flash in September at Flash prices, and 1017 makes that substitution look better.

The caveats stay visible. The comparison table is self-reported, no independent suite has scored the 1017 weights, and the ranking language rests on a composite of other published results rather than one controlled evaluation. The licence and the downloads are real; the ordering at the top of the open-weight field is the part still in flux.

*This post was drafted with AI assistance and reviewed by RITS staff.*
