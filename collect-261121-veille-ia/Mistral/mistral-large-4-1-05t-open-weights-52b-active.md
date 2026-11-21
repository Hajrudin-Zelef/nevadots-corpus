---
id: collect-261121-veille-ia/mistral/mistral-large-4-1-05t-open-weights-52b-active
title: "Mistral Large 4: 1.05T open weights, 52B active"
domain: mistral
role: reference
task: article
actors: ["Mistral AI"]
dates: ["2026-10-29"]
keywords: ["mistral", "open", "large", "weights", "active", "license", "tokens", "model"]
source: https://aihubhq.com/news/mistral-large-4-launch/
source_anchor: ""
source_lines: [1, 152]
section: "Mistral"
---
# Mistral Large 4: 1.05T open weights, 52B active

Mistral’s biggest bet of the year is now a file you can download. On
**October 29, 2026**, Mistral released the open weights of
**Mistral Large 4** — a **1.05-trillion-parameter mixture of experts** with
**49B active parameters per token** (52B counting the embedding and output
layers, hence the “A52B” in the repo name), a **1-million-token context
window**, and a license that lets you run, fine-tune and resell the
weights. The API, previewed since October 6, launched at
**$0.70 per million input tokens**. Independent measurement from
Artificial Analysis put the model at **26 on Intelligence Index v5** —
the strongest open-weights model developed outside China, and six points
behind the Chinese 1T+ leaders.

## What it is

| Parameter | Value | 
|---|---|
| Total / active parameters | 1.05T / 49B per token (52B incl. embeddings + output head) | 
| Architecture | MoE: 128 routed experts, 4 active + 1 shared per token; 72 layers; hidden 6,144; 64 heads | 
| Attention | MLA with a DSA indexer | 
| Vision | ~1.6B Pixtral-lineage encoder; text + image in, text out | 
| Context | 1,000,000 tokens | 
| Languages | 160+ | 
| Reasoning | three effort levels (low / medium / high; default high) | 
| License | Mistral Ecosystem Open-Weight License (MEOW), MIT-derived | 
| Checkpoints | FP8 (~1.1 TB), NVFP4 (~610 GB), GGUF Q4 (~605–635 GB) | 
| Training | ~3,800 NVIDIA Grace Blackwell GPUs in EU datacenters (~10 MW), ~2 months, then ~3 weeks of RL | 

The naming is the first trap. The model card is
`mistralai/Mistral-Large-4-1T-A52B`: 49B parameters do the work on each
token, and the “52” in the name adds the input-embedding and output-head
parameters. Mistral trained the model from scratch on roughly 3,800
NVIDIA Grace Blackwell GPUs in its EU datacenters — about 10 MW of
compute — over about two months, then ran roughly three extra weeks of
reinforcement learning with customer and security feedback after the
October 6 preview. The license, Mistral Ecosystem Open-Weight License
(MEOW), is an MIT derivative: use, modify, fine-tune, distribute and sell
the weights are all free. There is exactly one exception — if you want to
sell access to the model as an inference service, you need a separate
commercial license from Mistral.

## Benchmarks

### What Mistral reports

| Benchmark | Mistral Large 4 | Nearest rivals (vendor table) | 
|---|---|---|
| FrontierCode 2.0 (Cognition, private) | 19.7% | DeepSeek V4.1 Flash 19.3 · GLM-5.3 17.8 · Kimi K3 17.3 | 
| Terminal-Bench 4.0 | 41% | GLM-5.3 40 · DeepSeek V4.1 Flash 31 · Kimi K3 21 · Muse Spark Open 6 | 
| AA Cyber Index | 51% | — | 
| Agent-Computer Use (AA CAI) | 48 | GLM-5.3 49 — “tied, 47% fewer tokens per task”; Claude Code + Opus 5.5 tops at 62 | 
| BrowseComp | 84.5% | 75% at the SFT stage | 
| Refusal on malicious cyber requests | 96.6% | “the highest we measure among open models” | 

### What Artificial Analysis measured

| Metric | Large 4 (Oct 29) | Preview (Oct 6) | 
|---|---|---|
| Intelligence Index v5 | 26 (high) / 23 (medium) | 25 | 
| Terminal-Bench 4.0 | 25% | 28.3% (vendor) | 
| CyberGym-E2E-AA | 79% | 82% | 
| Hallucination | 28% | 41% | 
| Output tokens per II task | 82K | 135K | 
| Cost per II task | $2.06 | $5.54 | 

The vendor/independent gap is real and worth internalizing. On the same Terminal-Bench 4.0, Mistral reports 41% and Artificial Analysis measures 25%; on FrontierCode 2.0 the split is 19.7% vs 15%. One label needs a correction: the “82%” that circulates from the Hugging Face blog is the preview-stage CyberGym-E2E-AA subtest. At release that subtest sits at 79% (tied with MiMo-V2.6-Pro, DeepSeek V4.1 Flash at 76%), and the full Cyber Index is 51 — third among open models, behind DeepSeek V4.1 Flash (57) and MiMo-V2.6-Pro (56).

The preview-to-release delta is where the extra RL weeks show up: hallucination fell from 41% to 28%, output tokens per Intelligence Index task dropped 39%, and the cost per task fell 63% — from $5.54 to $2.06. As of October 2026, the independent verdict is “top open-weights model developed outside China” and fifth overall among open models.

## How it compares

| Model | Lab | Total / active | AA II v5 | Cyber Index | Cost per II task | 
|---|---|---|---|---|---|
| **Mistral Large 4** (high) | Mistral (FR) | 1.05T / 49B | **26** | 51 | $2.06 | 
| DeepSeek V4.1 Flash 1017 | DeepSeek (CN) | — | 32 | 57 | $0.60 | 
| GLM-5.3 | Zhipu (CN) | — | 32 | — | — | 
| Meta Muse Spark Open | Meta (US) | 816B / 42B | 25 | — | $2.02 | 
| MiMo-V2.6-Pro | Xiaomi (CN) | — | — | 56 | $0.26 | 

Large 4 is the first 1-trillion-parameter open-weights release from a Western lab. The largest US open model before it was Meta’s Muse Spark Open — 816B total / 42B active, released October 15, which already needed 700+ GB of VRAM for its NVFP4 checkpoint and two Mac Studio M5 Ultra machines for a practical MLX path. Artificial Analysis calls Large 4 the top open model developed outside China; VentureBeat and the Hugging Face blog tell the same story.

The honest read has two sides. On agentic coding, Large 4 is in the same breath as the Chinese leaders — 19.7% on FrontierCode against GLM-5.3’s 17.8%, and 41% on Terminal-Bench against 40% (vendor harness). But on the independent index it trails GLM-5.3 and DeepSeek V4.1 Flash 1017 by six points, and the price gap is wider than the quality gap: $2.06 per Intelligence Index task vs $0.60 for DeepSeek and $0.26 for MiMo. The token-efficiency line is the strongest independent argument for Large 4: its CAI score of 48 came in at 8.5M output tokens, against 15.9M for GLM-5.3 and 16.7M for DeepSeek V4.1 Flash — the same job at roughly half the tokens.

## What it means for our readers

- **Switch (API users):** if you are paying frontier-API prices for
agentic coding work and care about EU data residency, Large 4 is the
first Western open-weights option worth a real trial. $0.70/M in and
$2.10/M out at launch — roughly half the October 6 preview prices, with
the discount expected to last about two weeks. If your current stack
runs on GLM-5.3 or DeepSeek V4.1 Flash and the 6-point index gap does
not matter to your workload, the per-task economics still favor those
models.
- **Wait:** if you need the technical report — Mistral promised one after
the open release — or if you want a second wave of community numbers
on the vendor benchmarks, hold a week or two.
- **Buy (hardware):** only if you already have a private-inference or
data-residency requirement. The practical self-hosted path is the
official NVFP4 checkpoint on a 4× B200 or B300 node with vLLM or SGLang;
Dell ships a one-liner for its own B300 platform.
- **Skip (local builders):** there is no consumer path. The lowest
quantization is still a 256 GB floor. Use the API.

## Local AI: the honest math

| Checkpoint | Size | Fits | 
|---|---|---|
| NVFP4 (official) | ~610 GB | four Nvidia B200 or B300 GPUs | 
| FP8 (official) | ~1.1 TB | 4-GPU Blackwell/Hopper-class node | 
| GGUF Q4_K_M (ggml-org) | ~635 GB | 4-GPU Blackwell-class node | 
| GGUF UD-Q4_K_XL (Unsloth) | ~605 GB | 4-GPU Blackwell-class node | 
| 1-bit (Unsloth) | ~223–233 GB | 256 GB RAM machine, Mac Studio, 2× DGX Spark | 
| Q8 (Unsloth) | ~1.1 TB | 1.2 TB+ of memory | 

The vision projector (mmproj-BF16) adds about 3.4 GB. Unsloth also ships
an embedded DSpark draft head (~2.25 GB) that the repo claims doubles
decode speed at batch 1 — a built-in speculative-decoding path in llama.cpp
via `--spec-type draft-dspark`.

Every realistic self-hosted deployment is a cluster. Even the 1-bit Unsloth build, which fits a 256 GB-RAM machine, a Mac Studio or two DGX Spark units, runs at CPU-class throughput. The 4-bit GGUF files at 605–635 GB are a four-GPU Blackwell exercise, not a workstation one. If you want the local-vs-cloud arithmetic for a model this size, start with the VRAM by model and quant guide and the local vs cloud API prices guide, then run your exact break-even through the local vs cloud finder. This is also the moment to update the local LLMs overview in your head: the frontier of “downloadable” has moved to a 1T scale that Western labs can finally match, and the hardware floor moved with it.

## Community reaction

The Hacker News thread (1,685 points, 435 comments as of October 31) split into four camps. License skeptics debated what “open” means here: “if you host it, you can run it… but if you want to sell access as an inference provider, you need a license.” EU-sovereignty optimists: “I’m moving a lot of our stuff to Mistral. I’m much happier keeping our data in Europe” and “It’s great that Europe finally has a good open-weight model.” Hardware realists: “They were given day zero access to release their quantizations. The lowest quant will still require 256GB VRAM” — and the “$25k custom build with 8 video cards” joke. A recurring technical take is that Mistral, with access to roughly 4,000 GPUs — five to ten times fewer than Meta — produced a model that is “heavily undertrained”; that is community speculation, not a measured fact. On r/LocalLLaMA, the thread opened with “godsend, non-Chinese player” and settled on “not an upgrade for most people here.” The preview thread from October 6 already drew 1,216 comments — this release landed on a large, opinionated audience.

What comes next: Mistral promised a technical report after the open release, and its VP Pierre Stock said on the mattturck podcast that Large 5 is already in pretraining — a single-source claim, but consistent with the cadence. This follows the same pattern we tracked in open weights close the gap: the Western open-weights class keeps climbing, and the gap to the Chinese frontier is now measured in single index points instead of tiers.

## Sources

- Mistral — “Mistral Large 4: open weights” announcement — official launch post (key claims via search excerpts; page not fetchable from our network).
- HF model card — mistralai/Mistral-Large-4-1T-A52B — architecture, license, training details.
- HF blog — “Welcome Le Chonk” — vendor benchmarks, 82% subtest label, availability.
- Mistral docs — mistral-large-4-0 — API names, pricing.
- HF — mistralai/Mistral-Large-4-1T-A52B-NVFP4 — 610 GB NVFP4 checkpoint, 32 safetensors (size API-verified).
- HF — ggml-org/Mistral-Large-4-1T-A52B-GGUF — Q4_K_M, 13 shards (~635 GB, API-verified).
- HF — unsloth/Mistral-Large-4-1T-A52B-GGUF — UD-Q4_K_XL (~605 GB, API-verified), 1-bit/2-bit/Q8 ladder, DSpark draft head.
- Mistral launch X post — official announcement.
- Artificial Analysis — measured results (X post, 2026-10-29) — II v5 26, Cyber Index 51, cost per task, preview delta.
- Artificial Analysis — model release page — speed (161.3 tok/s, TTFT 13.82 s), cost.
- Artificial Analysis — preview article (2026-10-06) — preview numbers, CyberGym-E2E-AA baseline.
- The Decoder — “Mistral Large 4 is said to be the most powerful open AI model from Europe and the US” — vendor benchmarks, license.
- The Decoder (DE) — Mistral gibt “Le Chonk” frei — same, German.
- TheNextWeb — open-weights release — 4× B200/B300, NVFP4, license.
- TheNextWeb — API preview — preview timeline, ~4,000 GPUs.
- VentureBeat — “Mistral Large 4 … top open model outside China, 63% lower cost per task” — AA results, positioning, subtest breakdown.
- Unsloth docs — Mistral Large 4 — quant ladder, DSpark usage, 256 GB floor.
- mattturck podcast — Pierre Stock (VP) — training scale, Large 5 in pretraining (single source).
- HF — meta-models/Muse-Spark-Open-816B-A42B + Meta Muse Spark Open blog — 816B/42B, 700 GB NVFP4, 2× Mac Studio M5 Ultra.
- Hacker News — release thread — community reaction (1,685 pts, 435 comments).
- r/LocalLLaMA — launch thread — local-hardware reaction.
- Hacker News — preview thread — 1,216 comments on the October 6 preview.
