---
id: collect-261121-veille-ia/atria-dawn/run-atria-dawn-preview-locally-atomic-chat
title: "Run Atria Dawn Preview Locally | Atomic Chat"
domain: atria-dawn
role: reference
task: article
actors: []
dates: ["2026-09-15"]
keywords: ["preview", "atria", "dawn", "model", "atomic", "chat", "card", "license"]
source: https://atomic.chat/models/atria-dawn-preview
source_anchor: ""
source_lines: [1, 52]
section: "Atria_Dawn"
---
## What is Atria Dawn Preview?

Atria Dawn Preview is Shanghai AI Laboratory's text-only model for research and engineering agents. It builds on GLM-5.2 and is designed to turn a task into a result you can check: code that passes tests, an experiment with saved outputs, or a report backed by sources. The preview has downloadable BF16 and FP8 weights, but its size puts self-hosting in multi-accelerator territory.

| Specification | Atria Dawn Preview | 
|---|---|
| Parameters | 744B foundation; 753.33B stored parameters | 
| Architecture | GLM MoE with sparse attention | 
| Layers | 78 | 
| Context window | 256K advertised | 
| Modalities | Text input and output | 
| Release status | Preview | 
| License | MIT, per model card | 

The release describes its foundation as 744B parameters; the uploaded tensor inventory is larger. That distinction matters when you budget storage. The model card advertises 256K context even though config.json sets a 1,048,576-position ceiling. We use the advertised limit, not that configuration value as a promise of a tested million-token deployment. For the underlying model, see GLM-5.2.

## Atria Dawn Preview benchmarks

Shanghai AI Laboratory reports these results in its launch card. These are vendor scores, not Atomic Chat measurements, and the comparison does not establish equal tool budgets or serving conditions across all models. A dash means no result was provided. Source: official model card.

| Benchmark | Atria Dawn Preview | DeepSeek V4 Pro 0813 | KIMI K3 | Qwen 3.8 Max | GLM 5.3 | GPT 5.6 sol | Claude Opus 5 | 
|---|---|---|---|---|---|---|---|
| BrowseComp Web research | 92.5 | 83.4 | 91.2 | - | - | 92.2 | 90.8 | 
| DeepResearch Bench II Deep research | 51.1 | 46.6 | 51.3 | 49.2 | 52.7 | 50.7 | 54.1 | 
| MLE-bench Lite ML engineering | 86.2 | 86.8 | 85.8 | 81.3 | 80.8 | 88.9 | 88.0 | 
| SWE-bench Pro Harder engineering | 59.6 | 58.3 | 61.6 | 65.1 | 60.3 | 61.4 | 74.7 | 
| Terminal-Bench 2.1 Terminal agents | 78.3 | 78.7 | - | 89.3 | 85.4 | 85.1 | 90.2 | 
| BFCL v4 Function calling | 77.0 | 71.4 | 69.1 | - | 74.1 | - | - | 
| AutomationBench Office automation | 53.8 | 41.7 | 45.9 | 49.7 | 49.2 | 45.7 | 49.4 | 

The published comparison puts Atria first on BrowseComp, BFCL v4 and AutomationBench among the listed models. It trails Claude Opus 5 on SWE-bench Pro and Terminal-Bench 2.1. That supports evaluating it for search and tool-driven work, not treating it as the best model for every coding task.

## Atria Dawn Preview hardware requirements

The official repositories contain 353 BF16 shards and 177 FP8 shards. The totals below sum the actual weight-file sizes, including their file overhead. FP8 reduces disk usage substantially, but 755.63 GB of weight files still does not describe a laptop workload.

| Precision | Source | Size on disk | 
|---|---|---|
| BF16 Safetensors | internlm/Atria-Dawn-Preview | 1,506.67 GB | 
| FP8 Safetensors | internlm/Atria-Dawn-Preview-FP8 | 755.63 GB | 

RAM or VRAM must also cover runtime buffers and the attention cache. The required headroom depends on context, concurrency and the serving engine; we have not measured a minimum configuration. The developer lists SGLang 0.5.13.post1 or newer and vLLM 0.23.0 or newer, with GLM-5.2 deployment recipes.

## How to run Atria Dawn Preview in Atomic Chat

In-app execution has not been verified for this preview. The checked official repositories provide Safetensors, not an Atomic Chat GGUF build. Do not download either full checkpoint expecting the standard desktop flow to work. For self-hosting, the publisher's deployment section lists supported server frameworks; that is separate from confirmed Atomic Chat support. Browse the model catalog for other checkpoints and their documented download options.

## Atria Dawn Preview license

The official model card labels the preview code and weights MIT. Its LICENSE link currently returns 404, so obtain the actual license text from the publisher before relying on redistribution terms. Hosted API access is a separate service from downloading the weights.

Sources and file inventories checked September 15, 2026.
