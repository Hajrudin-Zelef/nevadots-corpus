---
id: collect-261121-veille-ia/deepseek/deepseek-v4-1-flash-size-hardware-and-local-setup
title: "DeepSeek V4.1 Flash: Size, Hardware and Local Setup"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: []
keywords: ["deepseek", "flash", "token", "parameters", "billion", "weights", "engram", "active"]
source: https://vdf.ai/blog/deepseek-v4-1-flash-local/
source_anchor: ""
source_lines: [1, 195]
section: "DeepSeek"
---
DeepSeek V4.1 Flash is an MIT-licensed multimodal mixture-of-experts model with a 552-billion-parameter backbone, 196.6 billion parameters of Engram n-gram memory, and 8 billion active parameters per prompt token or 16 billion per generated token. Its 510 GB checkpoint fits one 8-GPU H200 node, or four of its GPUs once the 183 GiB of Engram tables move to host memory. Locally, a 512 GB Mac Studio holds a 3- or 4-bit build, while a single 128 GB machine needs SSD streaming.

DeepSeek calls V4.1 Flash the smallest model in its new architecture family, yet the download is three times the size of the V4 Flash it replaced. Searches for it mix three things: the open weights, the 1017 update and the API that serves both. This guide keeps them apart.

Model cards, configuration files, serving recipes and DeepSeekâs API pages were read on 4 November 2026, and memory figures follow the method of our LLM VRAM calculator. The checks a security team should run before a pilot are in our DeepSeek provenance and risk review.

## DeepSeek V4.1 Flash at a glance

|  | DeepSeek V4.1 Flash | 
|---|---|
| Versions | Launch release, 10 September 2026; 1017 update, 17 October 2026 | 
| Hugging Face | deepseek-ai/DeepSeek-V4.1-Flash and deepseek-ai/DeepSeek-V4.1-Flash-1017 | 
| Parameters | 552B backbone plus 196.6B of Engram memory; 763B in Hugging Faceâs count | 
| Active parameters | 8B per prompt token, 16B per generated token | 
| Architecture | 40 layers, a 20-layer causal encoder and a 20-layer decoder; 384 routed experts, 6 per token, plus 1 shared | 
| Context | 1,048,576 tokens | 
| Input | Text and images | 
| Global KV cache | 890 bytes per token | 
| Weights | 510.3 GB; routed experts in FP4, most other weights in FP8 | 
| Reasoning | Thinking on or off; effort is an integer from 1 to 100 | 
| Licence | MIT | 

The suffixes are dates. V4-Flash-0731 shipped on 31 July 2026, V4-Pro-0813 on 13 August and V4.1-Flash-1017 on 17 October. The 1017 checkpoint keeps the launch modelâs architecture, configuration, tokenizer and size, and only the weights changed. DeepSeek reports gains on its own tests, such as Terminal-Bench 4.0 rising from 31.2 to 42.3 and DeepSWE v1.1 from 74.2 to 77.0, along with shorter outputs. Its API, web chat and app moved to 1017 the same day, and vLLMâs recipe now defaults to it.

The technical report, DeepSeek_V41_Tech_Report.pdf, sits in the launch repository on Hugging Face. vLLM labels the launch weights â0909â, while DeepSeekâs change log dates the release 10 September.

## What changed from DeepSeek V4 Flash

|  | DeepSeek-V4-Flash-0731 | DeepSeek-V4.1-Flash | 
|---|---|---|
| Parameters | 284B, 13B active | 552B plus 196.6B Engram; 8B or 16B active | 
| Layers | 43 | 40, split into encoder and decoder | 
| Routed experts | 256, 6 per token | 384, 6 per token | 
| Attention | Compressed sparse attention, ratios 4 and 128 | CSA2 with ratios 1 and 2, latents shared across layers, two-stage indexer | 
| Input | Text | Text and images | 
| Reasoning effort | low, high, max | Integer from 1 to 100 | 
| Weights | 166.9 GB | 510.3 GB | 
| DeepSeek API, 4 November 2026 | Retired; old names now reach V4.1-Flash-1017 | deepseek-flash | 

Three changes decide the hardware. Engram adds two hash tables of about 384 million rows each, looked up by 2- to 4-gram hashes of the input: 196.6 billion parameters, or 183 GiB in FP8, read only a few rows at a time, so engines can keep them off the GPU. The encoder-decoder split means a prompt token passes through only the 20-layer encoder, hence 8 billion active parameters in prefill and 16 billion in decode. And compressed latents, stored in FP4 and shared between layers, bring the global KV cache to 890 bytes per token, about a quarter of V4 Flashâs, so a million-token prompt holds under 1 GB of it plus a 128-token sliding window per layer.

DeepSeek trained V4.1 Flash from scratch on 45 trillion multimodal tokens. One setting catches people out: the API maps low, high and max effort to 50, 75 and 100, while the open encoder that vLLM uses maps low, high, xhigh and max to 25, 50, 75 and 100. Send an integer to get the same behaviour everywhere.

## DeepSeek V4.1 Flash size: official and community builds

vLLMâs recipe breaks the official 510.3 GB down: routed and draft experts in MXFP4 take 259.5 GiB, the Engram tables 183.1 GiB, block scales 21.9 GiB, attention and dense layers 6.9 GiB, and the BF16 embedding and output head 3.9 GiB. Because DeepSeek already ships the experts in FP4, a 4-bit conversion saves little.

| Build | Publisher | Total | Without Engram tables | 
|---|---|---|---|
| As released, FP4 experts and FP8 | DeepSeek | 510.3 GB | 313.7 GB | 
| NVFP4 | NVIDIA | 429.0 GB | â | 
| MLX, original experts, 4-bit Engram tables | mlx-community | 432.0 GB | 309.1 GB | 
| GGUF UD-Q4_K_XL | Unsloth | 500.7 GB | 297.9 GB | 
| GGUF Q4_K_M | mrfakename | 444.7 GB | â | 
| GGUF UD-IQ3_XXS | Unsloth | 394.7 GB | 233.5 GB | 
| GGUF UD-Q2_K_XL | Unsloth | 334.8 GB | 199.6 GB | 
| GGUF UD-IQ2_XXS | Unsloth | 300.5 GB | 165.3 GB | 
| GGUF UD-IQ1_S | Unsloth | 233.0 GB | 122.4 GB | 
| DwarfStar q2 | antirez/ds4 | 341 GiB | 152 GiB | 
| DwarfStar q4 | antirez/ds4 | 483 GiB | 294 GiB | 

Unsloth measured top-1 agreement with the official model of 82.8% for UD-IQ1_S, 91.5% for UD-Q2_K_XL, 94.6% for UD-IQ3_XXS and 98.2% for UD-Q4_K_XL on its wiki test. Across six benchmarks, UD-Q2_K_XL averaged 87.49 against 87.68 for Unslothâs near-lossless build, and UD-IQ1_S 83.53.

### Where the Engram tables can live

- **vLLM** pins them in host memory with`--engram-config '{"cpu_offload":true}'` ; on H200 at four-way tensor parallelism that frees 23.6 GiB per GPU, with unchanged output.
- **SGLang** moves them to host memory with`--engram-config '{"placement": "cpu"}'` , one huge-page shard per rank.
- **llama.cpp** reads rows from the memory-mapped GGUF on demand with`--lazy-mode on` , so the table can stay on SSD; Unslothâs figures without Engram assume this.
- **DwarfStar** never loads the tables and reads rows from the file in every mode, so the GGUF belongs on a fast local SSD.

## DeepSeek V4.1 Flash hardware requirements

Weights decide the hardware, because the cache is small. Using the calculatorâs rule, 90% of GPU memory and 80% of unified memory count as usable, and each configuration must hold its resident weights plus a few gigabytes per GPU for cache, buffers and the vision encoder.

### GPU servers

| Configuration | Engram tables | vLLM recipe notes | 
|---|---|---|
| 8Ã H200, 141 GB each | Resident, or host memory at TP4 | The whole checkpoint fits one node; at TP4 with offload, 81.2 GiB of weights and 38.5 GiB of cache per GPU, about 20 million tokens | 
| 8Ã H100, 80 GB each | Host memory | TP8, with batched tokens capped at 4,096 | 
| B200, GB200, GB300 | Host memory | TP2 per replica; one 768 GB GB200 NVL4 tray holds it at TP4 | 
| AMD MI300X, 192 GB | Host memory | TP4 at about 81 GiB per rank; TP2 does not fit | 
| AMD MI325X, MI355X | Host memory | TP2 | 
| NVIDIA DGX Station GB300 | Host memory, plus 60 GB of routed experts | One GPU, the full 1,048,576-token window and 24 sequences | 

Engram offload needs about 183 GiB of pinned host memory on top of the operating system. Our Kimi K3 walkthrough applies the same per-GPU arithmetic to a model that genuinely needs several nodes.

### Workstations with 96 to 128 GB

No vLLM or SGLang configuration targets a single 96 GB card or a 128 GB desk-side system, which leaves two routes. A workstation with a 96 GB RTX PRO 6000 and plenty of system RAM can keep the routed experts in that RAM with llama.cppâs `--cpu-moe` and the Engram table on SSD; Unslothâs UD-Q2_K_XL then needs about 200 GB of RAM and VRAM together, and generation runs at the speed of system memory. Alternatively, DwarfStar, which supports a DGX Spark through CUDA, streams its 2-bit build from SSD on one 128 GB Spark, or splits it across two Sparks over RDMA with about 81 GiB of main weights on each.

By our 80% rule, no single 128 GB machine holds any buildâs main weights. Unsloth lists its 122.4 GB 1-bit set for 128 GB systems such as a MacBook or a DGX Spark, which leaves under 6 GB for the system, cache and runtime. Treat single-Spark V4.1 Flash as an experiment; our desk-side hardware comparison covers what each machine holds.

### Mac Studio

The M5 Ultra Mac Studio comes with 96, 256 or 512 GB at 1.2 TB/s. A 512 GB model offers about 410 GB by our rule: enough for Unslothâs UD-IQ3_XXS fully resident at 394.7 GB, or its UD-Q4_K_XL at 297.9 GB with the Engram tables left on SSD. DwarfStarâs README says its 4-bit build is fully resident on a 512 GB Mac, with Engram rows read from disk. A 256 GB model offers about 205 GB, enough for Unslothâs UD-Q2_K_XL or UD-IQ2_XXS without Engram, or for DwarfStarâs 2-bit build. The 432 GB MLX conversion needs the 512 GB model and still passes our 80% line. Our M5 Ultra Mac Studio guide covers the machine.

## Serving DeepSeek V4.1 Flash with vLLM, SGLang, llama.cpp and Ollama

### vLLM

Use vLLM 0.31.0 or newer. The architecture arrived in 0.30.0, and 0.31.0 added a KV-offload fix and the gate that lets AMD GPUs offload Engram; 0.32.0, released on 18 October, was current on 4 November. The recipeâs command for an 8-GPU H200 node uses four of its GPUs:

```
export VLLM_ENGINE_READY_TIMEOUT_S=3600
vllm serve deepseek-ai/DeepSeek-V4.1-Flash-1017 \
  --tokenizer-mode deepseek_v41 \
  --tensor-parallel-size 4 \
  --engram-config '{"cpu_offload":true}' \
  --tool-call-parser deepseek_v41 \
  --enable-auto-tool-choice \
  --reasoning-parser deepseek_v41 \
  --mm-encoder-tp-mode data
```
The first load is slow, hence the hour-long timeout. Add `--language-model-only` for text-only work. Speculative decoding uses DeepSeekâs own DSpark drafter, which adds about 14 billion parameters; V4.1 has no MTP module. With thinking settings left unset, vLLM thinks at effort 50, so a small `max_tokens` can return empty content.

### SGLang

SGLangâs cookbook covers V4.1 Flash on H200, B200, B300, GB300 and AMD MI355X. When we checked, it pointed NVIDIA users to the preview container `lmsysorg/sglang:dev-dsv41` rather than a pip release, although release 0.5.23 of 2 November lists further V4.1 work. Thinking is off by default in SGLang, the reverse of vLLM, and the `deepseekv41` tool-call parser is required.

### llama.cpp

llama.cpp merged V4.1 Flash support on 25 October 2026 in pull request #31086; the separate pull request that Unslothâs guide builds from, #30817, was closed without merging two days later. Early GGUF files have needed re-uploads: an issue opened on 29 October reported Unslothâs files failing on the main branch, and a 1 November comment says UD-Q2_K_XL loads after the update. Pin both the llama.cpp build and the GGUF revision. Unslothâs command keeps the Engram table out of GPU memory:

```
./llama.cpp/llama-cli \
  --model unsloth/DeepSeek-V4.1-Flash-1017-GGUF/UD-Q2_K_XL/DeepSeek-V4.1-Flash-1017-UD-Q2_K_XL-00001-of-00007.gguf \
  --temp 1.0 --top-p 1.0 --top-k 0 --min-p 0.0 \
  --chat-template-kwargs '{"reasoning_effort":75}' \
  --load-mode mmap --lazy-mode on \
  --override-tensor "per_layer_token_embd.weight=CPU"
```
### DwarfStar and Ollama

DwarfStar (`ds4`) is an open-source engine from the antirez/ds4 repository, built around a handful of large models and its own GGUF files. It runs V4.1 Flash text and images on Appleâs Metal, and text on CUDA, including a DGX Spark. Its README says it is developed with strong assistance from AI coding agents, so review it like any new dependency.

Ollamaâs library lists only `deepseek-v4.1-flash:cloud` and `deepseek-v4.1-flash:1017-cloud`, which run on Ollamaâs servers at $0.30 per million input tokens and $1.20 per million output tokens at peak, so prompts leave your machine. LM Studioâs catalogue had no V4.1 Flash entry on 4 November. Our vLLM, Ollama and llama.cpp comparison covers the engines more generally.

## DeepSeek V4.1 Flash API or your own hardware

DeepSeekâs API prices per million tokens, as of 4 November 2026:

|  | deepseek-flash (V4.1-Flash-1017) | deepseek-v4-pro (V4-Pro-0813) | 
|---|---|---|
| Input, cache hit | $0.006 peak, $0.003 off-peak | $0.044 peak, $0.022 off-peak | 
| Input, cache miss | $0.30 peak, $0.15 off-peak | $1.32 peak, $0.66 off-peak | 
| Output | $1.20 peak, $0.60 off-peak | $3.96 peak, $1.98 off-peak | 
| Context, maximum output | 1M, 384K | 1M, 384K | 
| Image input | Yes | No | 

Peak hours run from 01:00 to 04:00 and 06:00 to 10:00 UTC on weekdays, excluding Chinese public holidays. The legacy names `deepseek-v4-flash` and `deepseek-v4-flash-vision-exp` still work but now reach V4.1-Flash-1017.

Price is only part of the choice. API calls go to DeepSeek, whose privacy policy says that to provide its services it collects, processes and stores personal data in the Peopleâs Republic of China. Weights on your own servers, with no outbound route, send nothing anywhere, and they change only when you change them, whereas an API name can move to new weights overnight, as `deepseek-flash` did on 17 October. Our guide to whether DeepSeek is safe compares the app, the API and local weights.

## DeepSeek V4.1 Flash vs GLM-5.3-Flash, Qwen 3.8 and V4 Pro

|  | DeepSeek-V4.1-Flash | DeepSeek-V4-Pro-0813 | GLM-5.3-Flash | Qwen3.8-Flash-Next | Qwen3.8-27B | 
|---|---|---|---|---|---|
| Parameters | 552B plus 196.6B Engram; 8B or 16B active | 1.6T, 49B active | 320B, 18B active | 125B plus a 51B n-gram table; 6B active | 27.8B dense | 
| Context | 1M | 1M | 1M | 262K, 1M with YaRN | 262K, 1M with YaRN | 
| Input | Text, images | Text | Text, images | Text, images, video | Text, images, video | 
| Licence | MIT | MIT | MIT | Qwen Community License 1.0 | Apache 2.0 | 
| Published weights | 510.3 GB | 892.8 GB | 328.3 GB FP8 | 185.6 GB FP8 | 30.9 GB FP8 | 
| Smallest machine by our rule | 256 GB Mac, Engram on SSD | 8Ã H200 | 128 GB at 1-bit | 128 GB at 4-bit | 24 GB GPU at 4-bit | 

Against V4 Pro, DeepSeekâs own maximum-effort table has the newer Flash ahead on most agent tests, for example 74.2 against 62.7 on DeepSWE v1.1 and 90.6 against 87.9 on Terminal-Bench 2.1. Pro keeps an edge in knowledge: its base model scores 55.2 on SimpleQA-Verified against 42.3. It also reads text only, weighs almost 900 GB and costs three to four times as much per token on the API. DeepSeekâs launch post expected a V4.1 Pro; none was listed on 4 November 2026.

GLM-5.3-Flash and Qwen3.8 sit in lighter classes. GLM-5.3-Flashâs 4-bit builds fit 256 GB, as our GLM-5.3 sizing guide shows, and Qwen3.8 starts at a single 24 GB GPU, as our Qwen 3.8 hardware guide explains. For smaller cards, our picks by VRAM size apply.

## DeepSeek V4.1 Flash licence

Both V4.1 repositories state that the repository and the weights are licensed under the MIT License, and the LICENSE file is the standard MIT text with âCopyright (c) 2023 DeepSeekâ. The weights are free to download and use: the licence sets no fees, user thresholds, naming rules or acceptable-use policy, and asks only that you keep the notice with any copies you pass on. Conversions carry their own tags, and NVIDIAâs NVFP4 build, Unslothâs GGUFs and the MLX conversion are also marked MIT on Hugging Face.

## How VDF AI fits

Running V4.1 Flash on your own hardware settles where prompts go. It does not settle who may use the model, for which tasks and with which documents. VDF AI handles that on premises or in a private cloud, with policy-bound agents, multi-agent workflows in Networks, retrieval over company documents that keeps their permissions, audit logs and role-based access control on every plan.

VDF AI Router can register a vLLM endpoint serving V4.1 Flash beside other local models and any cloud models you approve, and in air-gap mode it keeps every request on local models. DeepSeek says it optimised the 1017 update for its own DeepSeek Harness, among other tools. VDF AI does not run or integrate with DeepSeek Harness; VDF Code is its own governed coding assistant for infrastructure you control. On-prem bundles can begin with a three-month platform trial.

## Sources

DeepSeek model cards, API pages, serving recipes and community builds checked 4 November 2026.

- DeepSeek on Hugging Face: DeepSeek-V4.1-Flash with its config.json, technical report and commit history; DeepSeek-V4.1-Flash-1017 with its MIT License; DeepSeek-V4-Flash-0731; DeepSeek-V4-Pro-0813
- DeepSeek: API change log, models and pricing, V4.1 Flash announcement and privacy policy
- vLLM recipe for DeepSeek-V4.1-Flash and vLLM 0.32.0 release notes
- SGLang DeepSeek-V4.1 cookbook and SGLang 0.5.23 release notes
- llama.cpp pull requests #31086 and #30817, and issue #31385
- Unsloth DeepSeek-V4.1-Flash guide and GGUF builds
- Other builds: NVIDIA NVFP4, mlx-community 4-bit, mrfakename GGUF, Unsloth V4-Flash-0731 GGUF and the DwarfStar repository
- Ollama deepseek-v4.1-flash and deepseek-v4-flash library pages
- Comparison models: GLM-5.3-Flash, Unsloth GLM-5.3-Flash GGUF, Qwen3.8-Flash-Next and Qwen3.8-27B
- Hardware: Apple Mac Studio specifications and NVIDIA DGX Spark

**Weighing DeepSeekâs API against your own servers?** See how VDF AI Router keeps local models under one governed endpoint, or book a demo.
