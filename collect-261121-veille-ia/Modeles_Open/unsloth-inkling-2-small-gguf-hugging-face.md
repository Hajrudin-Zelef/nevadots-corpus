---
id: collect-261121-veille-ia/modeles-open/unsloth-inkling-2-small-gguf-hugging-face
title: "unsloth/Inkling-2-Small-GGUF · Hugging Face"
domain: modeles-open
role: reference
task: article
actors: []
dates: []
keywords: ["llama", "unsloth", "gguf", "inkling", "inkling-2-small-gguf", "ud-q8", "original", "nvfp4"]
source: https://huggingface.co/unsloth/Inkling-2-Small-GGUF
source_anchor: ""
source_lines: [1, 242]
section: "Modeles_Open"
---
# Run Inkling-2-Small via the Unsloth Desktop or read our Inkling 2 guide

- `UD-*` folders: Unsloth Dynamic quants, imatrix-calibrated (`imatrix_unsloth.gguf_file` ).
- `UD-Q8_K_XL` : lossless conversion: the original NVFP4 experts kept bit-exact (layers 0-39), every other weight BF16.
- `Q8_0` : the original NVFP4 experts kept bit-exact (layers 0-39), every other weight 8-bit.
- `mmproj-BF16.gguf` /`mmproj-F16.gguf` /`mmproj-F32.gguf` : vision projector, pass with`--mmproj` for image inputs.
- The chat template is embedded in every GGUF; use `--jinja` .

| Quant | Size (GB) | Mean KLD vs UD-Q8_K_XL | 99% KLD | Same top token | 
|---|---|---|---|---|
| UD-IQ1_S | 171.7 | 0.0922 | 1.143 | 89.7% | 
| UD-IQ1_M | 178.5 | 0.0829 | 1.041 | 90.2% | 
| UD-IQ2_XXS | 190.6 | 0.0577 | 0.730 | 91.8% | 
| UD-Q2_K_XL | 205.0 | 0.0450 | 0.559 | 92.7% | 
| UD-IQ3_XXS | 226.0 | 0.0364 | 0.430 | 93.3% | 
| UD-Q3_K_XL | 285.1 | 0.0176 | 0.209 | 95.3% | 
| UD-Q4_K_XL | 354.2 | 0.0029 | 0.031 | 98.0% | 
| Q8_0 | 386.5 | 0.0027 | 0.029 | 98.1% | 
| UD-Q8_K_XL | 484.4 | reference |  |  | 

KLD and top-token agreement are measured against the `UD-Q8_K_XL` GGUF (the original NVFP4 experts kept as is, every other
weight BF16) on 64 x 4096 tokens of wikitext-2, with llama.cpp's default F16 KV cache on both sides.

Inkling 2 Small uses a new `inkling2` architecture, so it needs a llama.cpp build with Inkling 2 support. Builds that
only support the original Inkling will not load it.

**1. Install llama.cpp with Inkling 2 support**

Build llama.cpp from `master` (any commit that includes ggml-org/llama.cpp#32217)
by following our Inkling 2 guide, or use the Unsloth Desktop app.

- `UD-IQ1_S` ,`UD-IQ1_M` ,`UD-IQ2_XXS` ,`UD-Q2_K_XL` and`UD-IQ3_XXS` run on llama.cpp`master` .
- `UD-Q3_K_XL` ,`UD-Q4_K_XL` ,`Q8_0` and`UD-Q8_K_XL` keep the original NVFP4 experts with their per-expert scales. These
need per-expert scale support from ggml-org/llama.cpp#32657: use a
llama.cpp`master` build that includes it, or Unsloth's prebuilt llama.cpp
(`b12540-mix-c4017f9` or newer). Without it, llama.cpp stops with`wrong number of tensors` .
- Builds made before #32217 (the ggml-org/llama.cpp#32187 layout) need the previous files, kept on the
`ggml-org-32187-layout` branch: add`--revision ggml-org-32187-layout` to the`hf download` command below.

**2. Download a quant and the vision projector**

```
pip install -U huggingface_hub
hf download unsloth/Inkling-2-Small-GGUF \
    --include "UD-IQ2_XXS/*" "mmproj-F16.gguf" \
    --local-dir Inkling-2-Small-GGUF
```
Keep all shards in the same folder and pass the first one to llama.cpp; the rest load automatically.

**3. Run**

```
./llama.cpp/llama-server \
    -m Inkling-2-Small-GGUF/UD-IQ2_XXS/Inkling-2-Small-UD-IQ2_XXS-00001-of-00005.gguf \
    --mmproj Inkling-2-Small-GGUF/mmproj-F16.gguf \
    --jinja -ngl 99 -c 32768 \
    --temp 1.0 --top-p 1.0 --top-k 0 --min-p 0.0 \
    --chat-template-kwargs '{"reasoning_effort":"high"}'
```
Open http://localhost:8080 to chat and upload images. Lower `-ngl` if the model does not fit in VRAM, or use `-ngl 0`
for CPU inference. For text-only use, omit `--mmproj`. `reasoning_effort` accepts `none`, `minimal`, `low`, `medium`,
`high` (default), `xhigh` and `max` (same as `xhigh`).

NVFP4 | MXFP8 | Playground | Tinker Cookbook | Acceptable Use

Inkling 2 Small is a general-purpose multimodal model that accepts text and image inputs and generates text outputs. It is intended for use in English and other languages, and across multiple coding languages. The model is designed to be used by developers building AI-powered applications, including agentic and tool-use systems, coding assistants, chatbots, and retrieval-augmented generation systems, and is suitable for general-purpose conversational use, instruction-following, and other natural language and visual tasks. It is released with open weights to support research, fine-tuning and integration into third-party products by downstream developers.

**Languages:** English, with general multilingual capabilities across other languages.

Try Inkling 2 Small on the Tinker Playground or access via API using the Tinker Cookbook. To fine-tune Inkling 2 Small with Tinker, also install the tml-renderers package.

Inkling 2 Small supports local inference using:

Inkling 2 Small can also be post-trained with:

API access is also available through third party inference providers, including:

- NVIDIA
- Fireworks
- Together
- Baseten
- Nebius
- Modal
- Cloudflare

The model is distributed in two checkpoint formats.

The main NVFP4 checkpoint requires a GPU cluster with at least 500GB of aggregated VRAM. This can be met with either of the following example configurations:

- 2x NVIDIA B300 GPUs
- 4x NVIDIA H200 GPUs

The alternative MXFP8 checkpoint requires a GPU cluster with at least 800GB of aggregated VRAM. This can be met with either of the following example configurations:

- 4x NVIDIA B300 GPUs
- 8x NVIDIA H200 GPUs

The model was pretrained, midtrained and post-trained in NVFP4, so this is the native precision for the model. We recommend the NVFP4 checkpoint for all use-cases, and we're confident that it's the best option. The MXFP8 has a very marginal 1–2 percentage points gain over the NVFP4 checkpoint over evals, but we do not think it is meaningful for any practical use-cases.

Running the model directly on GPU hardware requires an inference deployment framework — either SGLang, vLLM, TokenSpeed, Unsloth, or Hugging Face, along with all of their respective dependency libraries.

Multimodal autoregressive transformer

A 48-layer decoder-only transformer with a sparse Mixture-of-Experts (MoE) feed-forward backbone: each token is routed to 8 of 384 experts. Attention is a hybrid of local and global layers. The model is natively multimodal — images are encoded via a vision transformer — with image and text inputs projected into a shared hidden space and processed jointly by the decoder.

594B total, 17B active

MXFP8 and NVFP4

Inkling 2 Small accepts text input in UTF-8 encoding and image input in any pixel-based format (with each dimension ideally between 40px and 4096px for optimal performance). It supports a context window of up to 1M tokens.

Inkling 2 Small generates output as UTF-8 encoded text.

Training data includes a broad variety of content types, including text, images, video. Training data for the model was drawn from publicly available sources, acquired from third-parties, generated internally, or synthetically generated or augmented. Publicly available data includes content from the public internet and publicly accessible repositories.

The training data curation process includes cleaning, processing, and modifying datasets. These processing steps, which vary by data type, may include deduplication and filtering to remove junk or other low-quality data, or to advance safety or other objectives.

|  | Open Weights Flash |  |  |  |  | Open Weights Pro |  |  |  |  | Closed Weights Flash |  |  | 
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|  | Inkling 2 Small | GLM-5.3-Flash | MiMo V2.6 Flash | MiniMax-M3.1-Flash | DeepSeek-V4.1-Flash-1017 | Muse Spark Open | GLM 5.3 | MiMo V2.6 Pro | Mistral Large 4 | Kimi K3 | GPT-6 Luna | Gemini 3.8 Flash | Claude Haiku 5.5 | 
| Model Info |  |  |  |  |  |  |  |  |  |  |  |  |  | 
| Params (B) activated / total | 17/594 | 18/320 | 15/309 | 23/428 | 16/552 | 42/816 | 40/753 | 42/1020 | 49/1050 | 104/2800 | – | – | – | 
| Third Party Index |  |  |  |  |  |  |  |  |  |  |  |  |  | 
| AA Index (v5.0) | 30.6 | 27.8 | 26.7 | 29.1 |  | 24.9 | 31.7 | 29.9 | 26.1 | – | 27.9 | 30.8 |  | 
| Vals Index (v2.2) | 56.1% | 50.9% | 54.8% | 53.8% |  | 48.6% |  | 57.8% | 56.5% | 53.0% | 52.3% | 56.0% | 58.7% | 
| Coding |  |  |  |  |  |  |  |  |  |  |  |  |  | 
| FrontierCode v2.0 stirrup v0.2.1 | 25.4% | 17.6% | 21.6% | 20.8% |  | 7.6% | 21.6% | 24.0% | 15.0% | 17.3% | 25.2% | 18.7% |  | 
| DeepSWE v1.1 mini-swe-agent v2.4.6 | 72.1% | 63.4% | 67.9% | 73.7% |  | 60.0% | 69.0% | 71.9% | 61.7% | 68.5% | 66.6% |  | 59.9% | 
| Terminal Bench Science v0.1 mini-swe-agent v2.4.6 | 5.7% | 4.8% | 4.3% | 3.3% |  | 1.9% | 9.5% | 5.7% | 4.3% | 8.1% | 8.6% | 10.0% |  | 
| Vibe Code Bench v1.1 openhands (vals) | 81.6% | 30.8% | 79.0% | 73.9% |  | 76.2% | 78.1% | 85.2% | 81.6% | 85.0% | 81.7% | 78.7% |  | 
| SWE-Atlas v1.1 mini-swe-agent v2.4.6 |  | – | – | – |  | 42.3% | – | – | – | 61.0% | 50.8% | 57.3% | 59.4% | 
| SWEBench Pro v2 (Hard) mini-swe-agent v2.4.6 | 86.3% | 82.8% |  | 84.3% |  | 82.4% | 84.3% | 89.2% | 83.8% | 88.2% | 88.7% | 58.8% | 52.9% | 
| Sudo L7 |  | – | – | – | – | – | 9.4% | – |  | 12.2% | – | 10.6% | – | 
| Autoresearch |  |  |  |  |  |  |  |  |  |  |  |  |  | 
| PostTrainBench v1.2 Inkling 2 Small in Claude Code | 31.9% |  | – | – | 30.5% | – |  | – | – | 31.5% | – | – | – | 
| PMPP-Hard (AUC, 0–10h) pi v0.83.0 |  | – | – | – |  | – | 430.96 | – | 478.39 | 453.58 | – | – | – | 
| BioMysteryBench terminus v2 | 70.0% | – | 69.3% | – | – | – | – | – | – |  | 61.5% | 62.2% |  | 
| Search and Tools |  |  |  |  |  |  |  |  |  |  |  |  |  | 
| BrowseComp Inkling 2 Small with compaction |  | 76.0% | 78.1% | 69.5% | 87.8% | 68.7% | 75.1% | – | 84.5% | 88.4% | 83.6% |  | 80.2% | 
| DeepSearchQA Inkling 2 Small with compaction |  | 84.3% | 86.0% | 82.7% | 84.5% | 80.4% | 84.7% | 90.0% | 84.4% |  | 83.5% | 90.1% | 83.2% | 
| MCP Atlas Full / Public |  | –/82.0% | –/62.9% | –/80.4% | –/83.2% |  | 84.2/83.6% | –/84.4% | –/84.5% | 82.3/84.2% | –/77.8% | –/87.5% | –/81.0% | 
| Knowledge Work |  |  |  |  |  |  |  |  |  |  |  |  |  | 
| AA-Briefcase v1.1 | 1527 | 1451 | 1487 | 1476 |  | 1279 | 1503 | 1512 | 1348 | 1497 | 1337 | 1203 |  | 
| GDPVal-AA v2.1 | 1630 | 1649 | 1614 | 1465 |  | 1401 | 1657 |  | 1384 | 1543 | 1444 | 1445 | 1631 | 
| JobBench |  | 60.7% | 61.2% | 67.5% |  | 61.8% | 61.4% | 62.0% | 55.2% | 54.3% | 49.0% | 63.4% | 39.2% | 
| DAYJOB (Healthcare) |  | 1.2% | – | – |  | – | 4.4% | – | 3.6% | 4.0% | 1.2% | 2.0% | – | 
| Enterprise Operations |  |  |  |  |  |  |  |  |  |  |  |  |  | 
| EnterpriseOps-Gym |  | 33.2% | 29.0% |  | 43.6% | 43.1% | 36.4% | 38.0% | 44.9% | 45.3% | 24.9% | 45.7% | 25.1% | 
| EnterpriseBench-CoreCraft |  | 46.7% | – | – |  | – | 51.8% | – | 47.7% | 43.1% | 43.1% |  | – | 
| Finance Work |  |  |  |  |  |  |  |  |  |  |  |  |  | 
| Finance Agent v2.1 |  | 60.6% | 63.3% | 58.8% | 62.9% | 60.7% | 65.0% | 64.3% | 61.7% | 60.0% | 56.8% |  | 65.4% | 
| Excel Modeling Benchmark v1.1 | 69.5% | 66.3% | 68.9% | 67.8% | 69.2% | 62.0% | 69.1% | 67.4% | 68.7% |  | 70.0% |  | 65.5% | 
| Tax Agent Bench | 65.8% | 62.0% | 59.9% | 62.7% |  | 52.1% |  | 64.9% | 67.5% | 68.7% | 58.9% | 66.8% | 62.2% | 
| Apex-Accounting v1.1 pass@1 | 14.5% | 12.3% | – | 13.5% | 12.5% | 13.8% | 13.9% | – | 12.0% | 9.5% | 10.8% |  |  | 
| DAYJOB (Finance) |  | 7.3% | – | – |  | – | 11.0% | – | 4.8% | 5.5% | 6.8% | 4.5% | – | 
| DeepFinance |  | – | – | – | – | – | 27.5% | – | 21.5% | – | 26.4% | – |  | 
| Diligence Bench Best harness |  | 48.3% | – | 51.6% | 50.4% |  | 49.4% | 51.8% | 51.5% | 50.9% | 42.1% | 51.1% | 40.8% | 
| AA-AnalystAgent |  | 25.0% | – | – | – | – | 38.8% | 37.5% | – | 38.8% | 43.8% | 46.3% |  | 
| Tau 3 Banking v1.0.1 |  |  | – | – | – | – | – | – | – | 33.4% | – | – | – | 
| ATLAS-Finance v1.1 |  | – | – | – | – | – | 3.0% | – | – |  | – |  | – | 
| Legal Work |  |  |  |  |  |  |  |  |  |  |  |  |  | 
| Harvey LAB v1.1 |  | 10.4% | 13.3% | 7.5% | 14.2% |  | 14.6% | 13.8% | 15.8% | 12.9% | 4.2% | 12.1% | 4.2% | 
| Legal Research Bench | 46.6% | 45.2% | 38.0% | 34.1% | 45.2% | 38.5% |  |  | 35.6% | 46.2% | 30.3% | 38.9% | 43.8% | 
| Visual Creation |  |  |  |  |  |  |  |  |  |  |  |  |  | 
| DesignArena Web Apps |  | – | – | – | – | – | 1271 | 1274 | – |  | 1160 | 1224 | – | 
| DesignArena Website |  | 1266 | – | 1275 | 1296 | 1300 | 1285 | 1303 | 1253 |  | 1265 | 1295 | 1257 | 
| DesignArena Game Dev |  | 1270 | – | 1307 | 1261 | 1254 | 1292 | 1309 | 1218 |  | 1254 | 1298 | 1314 | 
| DesignArena 3D Design |  | 1300 | – | 1319 | 1344 | 1255 | 1304 | 1322 | 1239 |  | 1199 | 1279 | 1329 | 
| Code Arena (WebDev) | 1653 | 1607 | 1637 | 1582 |  | – | 1623 | 1625 | 1544 |  | 1579 | 1583 | 1589 | 
| AA-3D |  | 937 | – | – | – | – | 782 | 977 | – | 982 | – | 965 |  | 
| Code2Video-AA | 1020 | 975 |  | – | – | – | 1034 |  | – | 1041 | 1030 | 899 |  | 
| BVB |  | 67.9% | – | – | 70.0% | – | – | – | – | – |  | 65.9% | 68.4% | 
| Visual Perception |  |  |  |  |  |  |  |  |  |  |  |  |  | 
| PerceptionBench |  | 49.6% | 39.1% | 47.1% | 47.7% | 44.1% | – | 38.9% | 46.5% | 58.5% | 56.2% |  | 60.5% | 
| Humanity’s Sixth Sense |  | 26.8% | 25.4% | – | 30.1% | 29.6% | – | 25.2% | – | 25.5% | 21.0% |  | – | 
| BabyVision original / with Python |  | 53.4/84.0% | 16.9/68.4% | 56.1/91.0% | 44.9/92.5% | 44.3/80.2% | – | 19.0/73.8% | 42.3/71.0% | 53.4/85.7% | 55.3/79.6% |  | 48.3/93.9% | 
| ScreenSpot Pro original / with Python |  | –/85.3% | 35.4/72.5% | 48.5/76.8% | – | – | – | 56.6/79.7% | 42.3/43.4% | 73.8/86.1% | 81.1/87.7% | 65.7/93.7% |  | 
| OmniDocBench v1.6 |  | 92.3% | 87.4% | 92.0% | 91.9% | 93.8% | – | 88.4% | 90.2% |  | 92.3% | 89.7% | 92.6% | 
| SimpleVQA with search and tools |  | 67.1% | – | – | – | 63.6% | – | – | 57.8% |  | – | – | – | 
| Visual Reasoning |  |  |  |  |  |  |  |  |  |  |  |  |  | 
| GDP.pdf |  | 15.4% | 9.0% | 20.8% | 17.2% | 18.6% | 11.2% | 19.2% | 17.6% | 22.0% |  | 21.0% | 20.8% | 
| MMMU Pro Standard 10 |  | 79.0% | 67.8% | 79.1% | 80.6% | 80.9% | – | 74.1% | 78.0% | 80.5% | 79.7% |  | 79.1% | 
| Charxiv RQ original / with Python |  | 80.2/89.4% | 66.2/88.4% | 82.5/87.6% | 77.0/89.6% | 85.4/89.5% | – | 74.2/86.7% | 83.2/87.4% | 84.8/91.3% | 81.8/87.6% |  | 84.4/86.3% | 
| Chartography original / with Python | 36.7/82.1% | 16.3/78.0% | 7.5/73.9% | 12.6%/– | 17.9/83.0% | 35.7/65.8% | – | 10.8/69.0% | 21.2/55.2% | 26.6/66.6% | 32.0/66.5% |  |  | 
| VIALS original / with stirrup v0.2.1 |  | –/53.0% | – | –/49.7% | –/53.2% | – | – | –/58.4% | –/42.0% |  | –/44.1% | –/52.0% | –/60.5% | 
| SAGE v1.1 | 88.6% |  | 83.3% | 85.3% | – | 85.8% | – | 84.4% | 74.2% | 86.8% | 83.4% | 88.2% |  | 
| MuirBench |  | 71.6% | 63.2% | 69.4% | 75.7% | 74.5% | – | 72.6% | 67.5% | 77.0% | 73.9% |  | 74.0% | 
| ZeroBench (main) original / with Python |  | 13.0/32.0% | 7.0%/– | 21.0%/– |  | 14.0/50.0% | – | 8.0%/– | 15.0/23.0% |  | 16.0%/– |  |  | 
| STEM Reasoning |  |  |  |  |  |  |  |  |  |  |  |  |  | 
| CritPt v2 Full / Hard | 55.7/35.0% | –/24.5% | –/29.5% | 51.4/32.5% | 50.6/25.0% | 52.6/29.5% | 57.4/33.5% | 56.9/34.0% | 50.3/27.0% | –/40.5% |  |  | 64.0/43.5% | 
| HLE-Diamond | 28.7% | 18.2% | 13.7% | 28.1% | 25.3% | 26.3% | 17.9% | 15.2% | 17.3% | 26.9% | 22.2% |  |  | 
| HLE (text only) | 44.1% | 39.9% | 35.1% |  | 43.3% | 43.9% | 42.3% |  | 36.4% | 46.9% | 38.5% | 47.8% | 44.4% | 
| ArxivMath 10/2026 |  | – | – | – |  | 19.2% | – | – | 18.1% | – | – | – | – | 
| BrokenArxiv 10/2026 |  | – | – | – |  | 33.3% | – | – | 30.6% | – | – | – | – | 
| ARC-AGI-1 |  | 91.0% | – | – | 93.5% | 89.0% | – | – | – | 94.5% | 86.7% |  | 89.0% | 
| ARC-AGI-2 |  | 65.8% | – | – | 78.1% | 57.5% | – | – | – | 60.4% | 59.3% |  | 61.3% | 
| MysteryMechanism | 25.2% | – | 21.6% | – | – | – | 23.0% | 15.3% | – | – | 19.4% |  |  | 
| Long Context |  |  |  |  |  |  |  |  |  |  |  |  |  | 
| AA-LCR v1.1 | 83.3% | 80.0% | 74.3% | 79.7% | 82.7% | 78.3% | 79.7% |  | 78.0% |  | 83.3% | 81.3% | 82.7% | 
| LongBench v2 |  | 68.2% | 60.7% | 60.9% | 69.0% | 68.7% | 68.4% | 68.0% | 65.7% | 69.0% | 65.0% |  | 65.3% | 
| Factuality |  |  |  |  |  |  |  |  |  |  |  |  |  | 
| AA-Omniscience v1.1 score | 16.1 | 0.7 | -10.8 | -6.4 | 0.9 |  | 6.0 | -4.5 | 5.1 | – | -8.3 |  | 12.3 | 
| AA-Omniscience v1.1 non-hallucination rate |  | 61.7% | 47.8% | 6.3% | 35.9% |  | 61.1% | 39.8% | 71.8% | – | 20.7% | 44.9% | 65.8% | 
| Instruction Following |  |  |  |  |  |  |  |  |  |  |  |  |  | 
| ComplexConstraints v1.0 | 44.5% | 27.6% | 22.1% | 23.2% |  | 41.3% | 30.5% | 32.3% | 36.5% | 37.9% | 27.3% |  | 36.8% | 
| Multilinguality |  |  |  |  |  |  |  |  |  |  |  |  |  | 
| Global-MMLU-Lite | 91.0% | 89.0% | 83.9% | 78.5% | 90.4% |  | 89.8% | 88.0% | 88.0% | 90.6% | 90.2% |  | 87.8% | 
| MDPBench | 81.4% | 70.9% | 54.7% | 69.2% | 68.8% |  | – | 67.0% | 64.3% |  | 71.2% | 81.5% | 78.4% | 
| Safety |  |  |  |  |  |  |  |  |  |  |  |  |  | 
| HoneyBench v0.2 (↓) |  | – | – | – |  | 85.6% | 76.8% | – | 83.6% | 78.5% | – | 80.5% | – | 
| DistressBench |  | 72.7% | – | – | – |  | 72.5% | – | 77.0% | 73.5% | 66.7% | 70.7% | 71.7% | 
| FORTRESS (adversarial) | 83.8% | 73.3% | 64.7%* | 47.9% | 72.8% |  | 71.8% | 69.9%* | 76.0% | 73.4% | 89.2% | 79.5%* |  | 
| FORTRESS (benign) | 79.7% | 96.4% |  | 96.8% | 93.1% | 84.0% | 96.8% | 97.1%* | 91.8% |  | 92.4% | 94.8%* | 91.0% | 
| StrongREJECT (none) | 97.1% | 97.8% | 97.8% | 97.4% |  | 97.1% | 97.5% | 99.0% | 99.0% | 99.0% |  | 93.0% | 97.8% | 
| Cybersecurity |  |  |  |  |  |  |  |  |  |  |  |  |  | 
| CyberBench v1.1 | 72.7% | 67.9% | 75.4% | – | – | – | 72.1% | 72.9% | – | 75.2% |  | 43.8% |  | 
| PatchEval | 79.6% | 74.8% | 77.0% | 76.5% |  | 77.8% | 69.6% | 80.0% | 76.1% | 77.0% | 76.5% | 77.8% |  | 

in each row (lowest where lower is better); equal displayed scores tie in each row; none when first place is tied

* On public split only.

For ScreenSpot Pro evaluation, images whose long side exceed 4,096 pixels are downsampled to that limit while preserving the aspect ratio; the ground truth answers were rescaled accordingly. Smaller images remain unchanged.

We conducted safety evaluations ahead of release, spanning both everyday human-AI interaction and dangerous-capability testing. Because Inkling 2 Small is multimodal, we paid attention to whether safety behavior held consistently across text and image inputs. We applied mitigations to reduce risks before release.

For everyday interaction, we evaluated sycophancy, harmful manipulation, and psychological-harm patterns like parasocial dependency and validation of delusional reasoning, including through multi-turn, open-ended external red-teaming designed to surface issues that only emerge over longer conversations. We also assessed whether the model refuses genuinely harmful requests without over-refusing benign ones. For CBRN and cyber, we assessed knowledge and procedural uplift through internal evaluations, external testing, and refusal-suppressed variants intended to estimate latent capability with safeguards removed. For loss of control, we evaluated agentic capability, strategic deception, and sabotage potential, benchmarked against public frontier models, and found the model materially below frontier capabilities.

Across all areas, we concluded that Inkling 2 Small did not present risk of material uplift beyond what's already available in the open-weight ecosystem.

Inkling 2 Small may exhibit general limitations common to foundation models, including hallucination (generating plausible but factually incorrect or unsupported content), occasional failures to follow instructions precisely, and degraded performance in long multi-turn conversations. As with other large-scale models trained on web-derived and synthetic data, Inkling 2 Small may reflect biases present in its training data, including demographic, cultural, or linguistic biases, and may perform unevenly across languages, dialects, or subject domains that were less represented during training.

Inkling 2 Small's knowledge is limited to information available as of its training cutoff, and it may not reflect events, developments, or changes that occurred afterward.

We recommend that downstream developers and deployers apply appropriate human oversight and review for outputs used in high-stakes or safety-critical contexts, rather than relying on Inkling 2 Small's outputs without verification.

- Conduct their own evaluation of Inkling 2 Small's performance, safety, and fairness for their specific use case, language, and population prior to deployment, particularly for applications involving vulnerable groups.
- Implement additional safeguards — such as content filtering, rate limiting, and monitoring — at the application layer, especially for open deployment contexts where Inkling 2 Small's built-in mitigations may not be sufficient on their own.
- Avoid deploying Inkling 2 Small in domains such as medical, legal, or safety-critical decision-making without additional fine-tuning, domain-specific validation, and human oversight.

- Downloads last month
- 9,515
