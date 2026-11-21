---
id: collect-261121-veille-ia/liquid-lfm/orcarouter-qwen3-8-flash-next-uncensored-gguf-hugging-face
title: "orcarouter/Qwen3.8-Flash-Next-Uncensored-GGUF · Hugging Face"
domain: liquid-lfm
role: reference
task: article
actors: ["Liquid AI"]
dates: []
keywords: ["llama", "build", "qwen3", "orcarouter", "model", "files", "flash-next-uncensored-gguf", "gguf"]
source: https://huggingface.co/orcarouter/Qwen3.8-Flash-Next-Uncensored-GGUF
source_anchor: ""
source_lines: [1, 160]
section: "Liquid_LFM"
---
# Qwen3.8-Flash-Next-Uncensored-GGUF

*GGUF quants (2-bit → 8-bit) of the abliterated (refusal-removed) Qwen3.8-Flash-Next — for llama.cpp*

|  | 🛡️ **New from Orca — OrcaCyber Zero 1.5** Post-trained for cybersecurity: vulnerability analysis, vulnerability reproduction, red teaming & offensive-security research. **98.01% pass@1 on CyberGym Level 1** · 1M context · function calling · OpenAI-compatible API.  | 
| 🔌 **New from Orca — Orca Connect** Running this GGUF in llama.cpp? **Orca Connect** puts the rig under your desk behind your usual OrcaRouter API key, next to 200+ hosted models, reachable from a laptop, a phone or a cloud agent. Outbound only: no port forwarding, no tunnel, no firewall rule.**Requests your own hardware serves cost $0** , and`orca-connect claude` points Claude Code at your rig for one session.  |  | 

**One Gateway. Every Model.** — Route Smarter · Ship Safer · Spend Less.

**GGUF conversions** of the **abliterated** (refusal-removed) build of Qwen's
`Qwen3.8-Flash-Next` — a large **Mixture-of-Experts**
(512 experts, 10 routed + 1 shared active) preview of the **Qwen4 architecture** (`qwen4_exp`):
**Gated DeltaNet** linear attention + **Qwen Sparse Attention (QSA)** at the micro-block level,
**HyperConnections** in place of layer norms, **PLE n-gram** hash embeddings, native
**vision-language**, reasoning, and tool-calling. These files run in **llama.cpp** (CPU / CUDA /
Metal / ROCm), quantized from **2-bit to 8-bit**, with a separate **mmproj** file that
restores **vision**. Browse all models in the OrcaRouter Model Catalog.


This model has had its **safety alignment substantially removed** via *abliteration* (orthogonalizing
the refusal direction out of the residual stream). It will **comply with harmful, unethical, or illegal
requests** the original `Qwen3.8-Flash-Next` would refuse. Released **strictly for legitimate research** —
interpretability, AI-safety / refusal-mechanism study, red-teaming, and robustness evaluation. **You
assume full responsibility** for how you use it and everything it generates; add your own safety and
moderation layers before any deployment. Use must comply with the
Apache 2.0 License inherited from the base model and all
applicable law. The authors accept **no liability** for misuse.

The `qwen4_exp` architecture (Gated DeltaNet + QSA + HyperConnections + PLE n-gram) was **merged into
mainline llama.cpp on 2026-08-27** (PR #27742,
`model: add Qwen3.8-Flash-Next (qwen4exp)`, plus follow-up #27880). Any llama.cpp built from `master`
at/after that date loads these files. **Older or bundled runtimes that predate it will not** — e.g.
LM Studio's current build stops at `qwen35moe` and fails with `unknown architecture 'qwen4_exp'`; those
will work once they bump their bundled llama.cpp. Build a current one yourself:

```
git clone https://github.com/ggml-org/llama.cpp/
cmake llama.cpp -B llama.cpp/build -DBUILD_SHARED_LIBS=OFF -DGGML_CUDA=ON
cmake --build llama.cpp/build --config Release -j --clean-first \
  --target llama-cli llama-mtmd-cli llama-server llama-gguf-split
```
(Drop `-DGGML_CUDA=ON` for a CPU-only build.)

**MTP speculative decoding (new).** As of **PR #30592**
(stacked on #27836), llama.cpp's `qwen4exp` backend has a
real multi-token-prediction (`mtp`) inference path. **All quants here — including `Q8_0` — are plain
target-only files that load on any recent mainline llama.cpp / LM Studio.** To add speculative decoding, pair any
quant with the small **`…-MTP-draft.gguf`** (~4 GB, in Files) via `-md --spec-type draft-mtp` on a #30592 build
(see Usage). A separate **`…-Q8_0-MTP`** build (the MTP draft head embedded in the same file, so it self-speculates
without `-md`) is also provided for #30592 users — it needs that build and will not load on mainline. Apple Silicon
users can also use the MLX build `orcarouter/Qwen3.8-Flash-Next-Uncensored-MLX`,
which ships the `mtp/` drafter.


Files larger than ~48 GB are **split** with `llama-gguf-split` into multiple parts — download the whole
set for that quant and point llama.cpp at the `…-00001-of-000NN.gguf` part; it loads the rest
automatically. Sizes are approximate.

| File | Bits | Size | Notes / recommendation | 
|---|---|---|---|
| `…-Q2_K` | 2-bit | ~74 GB | Smallest K-quant; noticeable quality drop — low-VRAM only | 
| `…-Q3_K_S` | 3-bit | ~78 GB |  | 
| `…-Q3_K_M` | 3-bit | ~87 GB | Good small option | 
| `…-Q3_K_L` | 3-bit | ~93 GB |  | 
| `…-Q4_K_S` | 4-bit | ~103 GB |  | 
| **`…-Q4_K_M`** | 4-bit | ~110 GB | **Recommended default** — best quality/size balance | 
| `…-Q5_K_S` | 5-bit | ~117 GB | High quality | 
| `…-Q5_K_M` | 5-bit | ~125 GB | Near-max quality; every shard <50 GB (no Xet needed) | 
| **`…-Q6_K`** | 6-bit | ~168 GB | **High fidelity.** The PLE table falls back to Q8_0 → one ~54 GB shard, so it needs a Xet-capable downloader (default`hf` / recent`llama.cpp` ) | 
| **`…-Q8_0`** | 8-bit | ~188 GB | **Highest fidelity.** Plain target-only build; loads on mainline llama.cpp / LM Studio like the other quants. PLE shard ~54 GB → Xet-capable downloader required | 
| `…-Q8_0-MTP` | 8-bit **+ MTP** | ~191 GB | Same weights as `Q8_0`**plus the embedded MTP draft head** for single-file self-speculative decoding (`--spec-type draft-mtp` ).**Requires a PR #30592 build** — will not load on mainline yet. PLE shard ~54 GB → Xet-capable downloader required | 

**Q6_K and higher.** The model's **n-gram (PLE) embedding table** is a single **51.2B**-parameter tensor
(`per_layer_token_embd`, shape `[160, 320001536]`; 102.4 GB in BF16). Its quantized dimension (160) is not
divisible by 256, so it cannot be a K-quant — at Q6_K it falls back to **Q8_0 (~54.4 GB)** and occupies its own
shard. That shard is above CloudFront's **50 GB single-request *download*** limit (a delivery constraint, not a
storage limit), so it is served via **Xet / ranged requests**, which the default `hf` client and recent
`llama.cpp` downloaders handle transparently — the same approach upstream Unsloth uses for its Q6_K/Q8_0/BF16
builds. **Q6_K and Q8_0 are provided above** (a separate `Q8_0-MTP` variant embeds the MTP draft head); BF16 is omitted as impractically
large — for full precision use the BF16 safetensors in `orcarouter/Qwen3.8-Flash-Next-Uncensored`.


Lower-bit quants built with an **importance matrix** (computed on English + Chinese + code calibration
text) — better quality-per-bit than plain K-quants at the low end, especially IQ3/IQ2.

| File | Bits | Size | Notes / recommendation | 
|---|---|---|---|
| **`…-IQ4_XS`** | ~4.25-bit | ~97 GB | **Best low-bit pick** — approx Q4_K_S quality at smaller size | 
| `…-IQ4_XS-MTP` | ~4.25-bit **+ MTP** | ~100 GB | Same weights as `IQ4_XS`**plus the embedded MTP draft head** for single-file self-speculative decoding (`--spec-type draft-mtp` , ~0.77 draft acceptance, no`-md` needed).**Requires a PR #30592 build** — will not load on mainline yet. | 
| `…-IQ3_M` | ~3.7-bit | ~82 GB | Solid 3-bit | 
| `…-IQ3_XXS` | ~3.1-bit | ~73 GB | Smaller 3-bit | 
| `…-IQ2_M` | ~2.7-bit | ~63 GB | Runs in lower VRAM; some quality loss | 
| `…-IQ2_XXS` | ~2.1-bit | ~52 GB | Smallest runnable; most degraded | 

| File | Size | Notes | 
|---|---|---|
| **`mmproj-…-F16.gguf`** | ~0.9 GB | **Vision projector — download this too for image input** | 

| File | Size | Notes | 
|---|---|---|
| **`…-MTP-draft.gguf`** | ~4 GB | Multi-token-prediction draft head. Pair with **any** quant above via`-md` for faster decode (measured ~1.3–2x; ~67% draft acceptance) on a PR #30592 build. Self-contained (carries its own embeddings), so it loads as a normal`-md` draft. The separate`Q8_0-MTP` build already embeds this head and can self-speculate without it. | 

```
hf download orcarouter/Qwen3.8-Flash-Next-Uncensored-GGUF \
  --include "Qwen3.8-Flash-Next-Uncensored-Q4_K_M*" "mmproj-*" \
  --local-dir ./qwen-flashnext-uncensored
```
```
./llama-cli -m Qwen3.8-Flash-Next-Uncensored-Q4_K_M-00001-of-00003.gguf --jinja -c 8192 \
  --temp 1.0 --top-p 0.95 --top-k 20 --min-p 0.0 -p "Hello!"
```
```
./llama-server -m Qwen3.8-Flash-Next-Uncensored-Q4_K_M-00001-of-00003.gguf \
  --mmproj mmproj-Qwen3.8-Flash-Next-Uncensored-F16.gguf \
  --host 0.0.0.0 --port 8000 -c 8192 --jinja
```
- **Recommended sampling:**`--temp 1.0 --top-p 0.95 --top-k 20 --min-p 0.0` .
- **Vision:** pass`--mmproj …` , then send OpenAI`image_url` content parts (base64 data-URI or URL).
- **Tool calling:**`--jinja` enables the Qwen tool template; use standard OpenAI`tools` +`tool_calls` .
- **Reasoning (thinking):** thinking is on by default; toggle per request via`chat_template_kwargs.enable_thinking` . The reasoning trace is returned in`reasoning_content` —
give`max_tokens` room (e.g. >= 2048) so the final answer isn't truncated by the thinking budget.

The **`Q8_0-MTP`** build embeds the model's multi-token-prediction draft head. On a llama.cpp built from
PR #30592, turn on **single-file self-speculative
decoding** — no separate draft model required:

```
./llama-cli -m Qwen3.8-Flash-Next-Uncensored-Q8_0-MTP-00001-of-00005.gguf --jinja -c 8192 --spec-type draft-mtp --spec-draft-n-max 3 --temp 1.0 --top-p 0.95 --top-k 20 --min-p 0.0 -p "Hello!"
```
Keep `--spec-draft-n-max` small (2–3) for best acceptance. This path exists only on the #30592 build;
the plain `Q8_0` (and every other quant) loads on mainline llama.cpp as usual. For MTP on mainline-compatible
files, use any quant with the standalone `-md` draft below.

The same single-file self-speculation also works with the much smaller **`IQ4_XS-MTP`** (~100 GB, the IQ4_XS weights with the MTP head embedded, ~0.77 draft acceptance):

```
./llama-cli -m Qwen3.8-Flash-Next-Uncensored-IQ4_XS-MTP-00001-of-00003.gguf --jinja -c 8192 --spec-type draft-mtp --spec-draft-n-max 3 --temp 1.0 --top-p 0.95 --top-k 20 --min-p 0.0 -p "Hello!"
```
For **any other quant** (Q2_K–Q6_K, IQ*), pair it with the standalone MTP draft via `-md`:

```
./llama-cli -m Qwen3.8-Flash-Next-Uncensored-Q4_K_M-00001-of-00003.gguf -md Qwen3.8-Flash-Next-Uncensored-MTP-draft.gguf --jinja -c 8192 --spec-type draft-mtp --spec-draft-n-max 3 --temp 1.0 --top-p 0.95 --top-k 20 --min-p 0.0 -p "Hello!"
```
Abliteration was measured on this build (served with vLLM) vs the official `Qwen/Qwen3.8-Flash-Next`,
using the same scripts. Harmful-prompt refusal collapses from **64–100%** (base) to **~0–3.3%**;
benign over-refusal stays near 0%; capability stays within **±2 pts** of the base across MMLU-Pro /
GSM8K / CMMLU-style checks; vision (image + OCR via `mmproj`) and multi-turn tool calling verified
working. GGUF quants are deterministic derivations and inherit these behaviours; lower bits trade some
quality (most visible at Q2_K / IQ2).

Runs on CPU, CUDA, Metal, or ROCm via llama.cpp. As a large MoE, only ~10 of 512 experts are active per
token, so decode is far faster than the total parameter count suggests, but **the full weights must fit
in RAM/VRAM (or be memory-mapped)**: budget ≈ the file size + KV cache + (for vision) the ~0.9 GB
mmproj. Multi-GPU (`-ngl` split) and CPU+GPU offload both work.

**Apache 2.0**, inherited from `Qwen/Qwen3.8-Flash-Next`.
Abliteration and quantization do not change the underlying license obligations.

- Downloads last month
- 673,135
