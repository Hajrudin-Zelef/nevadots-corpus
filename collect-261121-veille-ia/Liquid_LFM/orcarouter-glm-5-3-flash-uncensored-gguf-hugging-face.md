---
id: collect-261121-veille-ia/liquid-lfm/orcarouter-glm-5-3-flash-uncensored-gguf-hugging-face
title: "orcarouter/GLM-5.3-Flash-Uncensored-GGUF · Hugging Face"
domain: liquid-lfm
role: reference
task: article
actors: ["Liquid AI"]
dates: []
keywords: ["glm-5", "llama", "model", "build", "orcarouter", "flash-uncensored-gguf", "flash", "orca"]
source: https://huggingface.co/orcarouter/GLM-5.3-Flash-Uncensored-GGUF
source_anchor: ""
source_lines: [1, 126]
section: "Liquid_LFM"
---
# GLM-5.3-Flash-Uncensored-GGUF

*GGUF quants (2-bit → 6-bit) of the abliterated (refusal-removed) GLM-5.3-Flash — for llama.cpp*

|  | 🛡️ **New from Orca — OrcaCyber Zero 1.5** Post-trained for cybersecurity: vulnerability analysis, vulnerability reproduction, red teaming & offensive-security research. **98.01% pass@1 on CyberGym Level 1** · 1M context · function calling · OpenAI-compatible API.  | 
| 🔌 **New from Orca — Orca Connect** Running this GGUF in llama.cpp? **Orca Connect** puts the rig under your desk behind your usual OrcaRouter API key, next to 200+ hosted models, reachable from a laptop, a phone or a cloud agent. Outbound only: no port forwarding, no tunnel, no firewall rule.**Requests your own hardware serves cost $0** , and`orca-connect claude` points Claude Code at your rig for one session.  |  | 

**One Gateway. Every Model.** — Route Smarter · Ship Safer · Spend Less.

Website · Model Catalog · GitHub · Discord · X

**GGUF conversions** of the **abliterated** (refusal-removed) build of Z.ai's
`GLM-5.3-Flash` — a **320B / 18B-active**
Mixture-of-Experts model (`glm5_next`, 288 routed experts top-8 + 1 shared) with hybrid linear +
sparse attention, 4-wide **Manifold-Constrained Hyper-Connections**, a native **vision + video**
tower, and a **1M-token** context. These files run in **llama.cpp** (CPU / CUDA / Metal / ROCm),
quantized from **2-bit to 6-bit**, with a separate **mmproj** file that restores **vision**. Browse
all models in the OrcaRouter Model Catalog.

**Derived releases:**  •  `GLM-5.3-Flash-Uncensored-FP8` (block-FP8, mirrors official)
 •  `GLM-5.3-Flash-Uncensored-NVFP4` (experts-only NVFP4, for vLLM)
 •  `GLM-5.3-Flash-Uncensored-MLX` (2/3/4/6-bit, Apple Silicon)
 •  `GLM-5.3-Flash-Uncensored-GGUF` (this repo).


This model has had its **safety alignment substantially removed** via *abliteration* (orthogonalizing the
refusal direction out of the residual stream). As a direct consequence:

- **It will comply with harmful, unethical, offensive, or illegal requests** that the original`GLM-5.3-Flash` would refuse. It has no meaningful built-in guardrails.
- It is released **strictly for legitimate research** — interpretability, AI-safety and refusal-mechanism
study, red-teaming, robustness evaluation, and controlled experiments.
- **You assume full responsibility and liability** for how you use it and for everything it generates. Do
not deploy it to end users or in production without adding your own safety, moderation, and
abuse-prevention layers.
- Use must comply with the **MIT License** inherited from the base
model, and all laws and regulations that apply to you.
- The authors and uploaders **accept no liability** for any misuse or harm. Its outputs do**not** reflect
the views of the uploaders or of Z.ai / Zhipu AI.

By downloading or using this model you acknowledge and accept the above.

The `glm5_next` (GLM-5.3-Flash) architecture is **new** and not yet in mainline llama.cpp — support is in
PR #27754 (*model: add GLM-5-Next (GLM-5.3-Flash)*).
Until it merges, build from the PR branch; a bundled/older runtime will fail with
`unknown architecture 'glm5next'`.

```
git clone -b glm5next/upstream https://github.com/unslothai/llama.cpp
cmake llama.cpp -B llama.cpp/build -DGGML_CUDA=ON
cmake --build llama.cpp/build --config Release -j --clean-first \
  --target llama-cli llama-server llama-mtmd-cli llama-gguf-split
```
(Drop `-DGGML_CUDA=ON` for a CPU-only build.) `transformers>=5.16` is needed only if you convert from the
HF weights yourself.

**Note — no MTP head.** These GGUFs **do not include** the multi-token-prediction (`mtp`) speculative
draft head (layer 45); the llama.cpp `glm5next` backend has no MTP inference path, so the converter
drops it. All text + vision capability is unaffected. For speculative decoding, use the MLX build
`orcarouter/GLM-5.3-Flash-Uncensored-MLX`.


Files larger than ~48 GB are **split** with `llama-gguf-split` into parts — download the whole set for a
quant and point llama.cpp at the `…-00001-of-000NN.gguf` part; it loads the rest automatically. Sizes are
approximate.

| File | Bits | Size | Notes / recommendation | 
|---|---|---|---|
| `…-Q2_K` | 2-bit | ~117 GB | Smallest; noticeable quality drop — low-VRAM only | 
| `…-Q3_K_M` | 3-bit | ~153 GB | Good small option | 
| **`…-Q4_K_M`** | 4-bit | ~193 GB | **Recommended default** — best quality/size balance | 
| **`…-Q6_K`** | 6-bit | ~263 GB | **Highest fidelity offered** — within noise of the source | 
| **`mmproj-…-F16.gguf`** | — | ~1 GB | **Vision projector — download this too for image input** | 

For full precision, use the block-FP8 safetensors in
`orcarouter/GLM-5.3-Flash-Uncensored-FP8`.

```
hf download orcarouter/GLM-5.3-Flash-Uncensored-GGUF \
  --include "Q4_K_M/*" "mmproj-*" --local-dir ./glm53-flash-uncensored
```
```
./llama-cli -m Q4_K_M/GLM-5.3-Flash-Uncensored-Q4_K_M-00001-of-00005.gguf --jinja -c 8192 -p "Hello!"
```
```
./llama-server -m Q4_K_M/GLM-5.3-Flash-Uncensored-Q4_K_M-00001-of-00005.gguf \
  --mmproj mmproj-GLM-5.3-Flash-Uncensored-F16.gguf \
  --host 0.0.0.0 --port 8000 -c 8192 --jinja
```
- **Vision:** pass`--mmproj …` , then send OpenAI`image_url` content parts (base64 data-URI or URL).
- **Reasoning:** GLM-5.3-Flash has**no `enable_thinking` toggle** — its chat template always opens a`<think>` block. Give generation enough budget to reach`</think>` or the final answer is truncated
inside the scratchpad.

Perplexity, KL-divergence and top-1 token agreement of each quant against the near-lossless **Q8_0**
reference (the source dequantized to 8-bit and run through the identical llama.cpp forward, so the
quantization is the only variable). Measured on wikitext-2 (~91k tokens, `n_ctx=512`). Lower KLD and
higher Top-1 = closer to the source.

| Build | Size | PPL | ΔPPL | Mean KLD | Top-1 agreement | 
|---|---|---|---|---|---|
| **Q8_0 (ref)** | 341 GB | 3.394 | — | — | 100 % | 
| **Q6_K** | 263 GB | 3.406 | +0.3 % | **0.026** | **94.3 %** | 
| **Q4_K_M** | 193 GB | 3.513 | +3.5 % | 0.087 | 89.8 % | 
| **Q3_K_M** | 153 GB | 3.677 | +8.4 % | 0.176 | 85.6 % | 
| **Q2_K** | 117 GB | 4.916 | +44.9 % | 0.486 | 75.5 % | 

All three signals agree on the ordering and on where the cliff is: **Q6_K is within noise of the source**,
**Q4_K_M is the sweet spot**, Q3_K_M degrades gently, and **Q2_K costs a lot** — pick it for fit, not
quality.

Abliteration is baked into the weights and inherited by every quant. On the held-out JailbreakBench test
split, harmful-prompt refusal drops from **~89 %** on the base `GLM-5.3-Flash` to the low-teens, with
benign over-refusal at 0 % (see the base
`GLM-5.3-Flash-Uncensored-FP8` card for
the full safety numbers). Refusal is **reduced, not removed** — a few extreme content categories the
single direction does not reach still refuse.

Runs on CPU, CUDA, Metal, or ROCm via llama.cpp. As a large MoE, only 8 of 288 routed experts are active
per token, so decode is far faster than the total parameter count suggests, but **the full weights must
fit in RAM/VRAM (or be memory-mapped)**: budget ≈ the file size + KV cache + (for vision) the ~1 GB
mmproj. Multi-GPU (`-ngl` split) and CPU+GPU offload both work.

**MIT**, inherited from `zai-org/GLM-5.3-Flash`.
Abliteration and quantization do not change the underlying license obligations.

- Downloads last month
- 12,684
