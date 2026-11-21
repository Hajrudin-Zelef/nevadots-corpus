---
id: collect-261121-veille-ia/modeles-open/bertholomus-beam-1-0-501b-a23b-exl3-3bpw-2xgb10-hugging-face
title: "bertholomus/Beam-1.0-501B-A23B-EXL3-3bpw-2xGB10 · Hugging Face"
domain: modeles-open
role: reference
task: article
actors: []
dates: []
keywords: ["prompt", "decode", "code", "token", "stream", "prose", "reflection", "beam-1"]
source: https://huggingface.co/bertholomus/Beam-1.0-501B-A23B-EXL3-3bpw-2xGB10
source_anchor: ""
source_lines: [1, 86]
section: "Modeles_Open"
---
**EXL3 quantization of reflection/Beam-1.0-501B-A23B, made by
Bertholomus AI. Model and weights © Reflection, under the Apache License 2.0** (the `LICENSE` file here is Reflection's,
unchanged). This repository holds quantized weights only. The original model card, benchmarks and prompt format are on
Reflection's model page. The chat template and tokenizer files are
Reflection's, unchanged.

Beam is an open-weight 501B mixture-of-experts model (23B active) from Reflection, a US lab. This quant runs it on your own hardware: two NVIDIA DGX Spark (GB10, 128 GB unified memory each), tensor-parallel 2 over one 200 Gb/s RoCE port pair, with the DFlash2 drafter, on TensorFold (the default since recipe v0.2.0: an 868,352-token context window) or vLLM (a 655,360-token window). Nothing leaves the two machines.

| Source | `reflection/Beam-1.0-501B-A23B` (BF16) | 
| Converter | diffbot's `requant_seq.py` (MIT) on exllamav3 v1.6.5 (`cdc785d` ) by turboderp, with 128 calibration sequences | 
| Routed experts (52 layers × 256) | 3 bits, all experts | 
| Attention ( `qkv_gate_proj` ,`o_proj` ) and dense MLPs | 6 bits | 
| Embedding, norms, router, lm_head | BF16 ( `nonexpert-00001.safetensors` ) | 
| Size | ~194 GB of tensors ( `dense-L*` ,`experts-L*` ,`nonexpert-*` ) | 

KL divergence against the BF16 model's logits, AtomicChat's `eval_neutral` set, all 133 chunks × 4,096 tokens
(272,251 positions), top-128 logprobs through each engine's API (a lower bound of the full KL):

| Engine | KL mean | KL median | KL q99 | Top-1 agreement | Perplexity ratio vs BF16 | 
|---|---|---|---|---|---|
| TensorFold | 0.0121 | 0.00085 | 0.166 | 96.87 % | 1.0114 | 
| vLLM | 0.0132 | 0.00098 | 0.177 | 96.66 % | 1.0125 | 

Our TensorFold fork, branch `beam-1.0-tp2` @ `d00201b4`, with the recipe's settings. Medians of 3 runs, the recipe's
`gb10/rows.py` with `SEED=4242000`, thinking off; the vLLM column is the recipe's vLLM lane, measured the same days
with the same prompts (the vLLM table below holds the v0.1.0 runs).

| Row | TensorFold | vLLM | 
|---|---|---|
| Decode, 1 stream, code, 2K prompt | 45.5 tok/s | 33.4 tok/s | 
| Decode, 1 stream, prose, 2K prompt | 30.6 tok/s | 28.3 tok/s | 
| Decode, 1 stream, code, 46K prompt | 28.6 tok/s | 29.7 tok/s | 
| Decode, 1 stream, prose, 46K prompt | 31.2 tok/s | 27.7 tok/s | 
| Decode, 4 streams, code, 2K prompt (aggregate) | 64.7 tok/s | 61.0 tok/s | 
| Decode, 4 streams, code / prose, 46K prompt (aggregate) | 63.9 / 60.7 tok/s | 53.0 / 53.1 tok/s | 
| Prefill, 8K prompt, prose / code | 1,409 / 1,390 tok/s | 1,394 / 1,257 tok/s | 
| Prefill, 46K prompt, prose / code | 1,390 / 1,401 tok/s | 1,362 / 1,307 tok/s | 
| Short-chat time to first token, idle / behind a cold 46K prompt | 0.39 s / 2.2 s | 0.44 s / 32.9 s | 
| Context window | 868,352 tokens | 655,360 tokens | 

Medians of 3 runs, range in brackets.

| Row | Result | 
|---|---|
| Decode, 1 stream, code, 2K prompt | 34.2 tok/s [32.4–35.0] | 
| Decode, 1 stream, prose, 2K prompt | 28.9 tok/s [28.1–30.2] | 
| Decode, 1 stream, code, 36K prompt | 32.3 tok/s [29.9–32.8] | 
| Decode, 1 stream, prose, 46K prompt | 29.6 tok/s [26.8–30.3] | 
| Decode, 4 streams, code, 2K prompt (aggregate) | 60.3 tok/s [58.4–65.0] | 
| Decode, 4 streams, code / prose, 46K prompt (aggregate) | 55.0 / 54.6 tok/s | 
| Prefill, 8K prompt | 1,306–1,411 tok/s (time to first token 5.5–5.7 s) | 
| Prefill, 46K prompt | 1,371 tok/s (time to first token 33.5 s) | 
| Short-chat time to first token (idle server) | 0.37 s [0.37–0.47] | 
| Longest prompt run | 632,185 tokens + 8 new tokens in 755.7 s (837 tok/s) | 
| DFlash2 mean acceptance length | 2.76–2.96 | 

Decode and prefill rows were measured with a 400,000-token window; the long-prompt row with the published 655,360 window. The window size changes only the KV pool, not the per-token work.

The weights take ~91 GiB per node; the DFlash2 drafter adds ~1.6 GiB of weights per node. A 5.9 GiB KV pool per node holds one 655,360-token request.
We keep at least 9 GiB of free system memory on each node under load; the lowest seen was about 9.3 GiB. On a node image with less
free memory, lower `KV_MEM` and `MAX_MODEL_LEN` together (about 1 GiB per 100K tokens).
TensorFold sizes its KV pool at each start from what the same 9 GiB floor leaves: 868,352 positions per node in our served
start, at 9,216 bytes a position (fp8); one stream can use all of it.

Recipe and launch scripts: the recipe repository bertholomus/beam-1.0-2x-dgx-spark,
v0.2.0.
Default serving stack (`tf/`): our TensorFold fork (bertholomus/TensorFold,
branch `beam-1.0-tp2`) in NVIDIA's `nvcr.io/nvidia/pytorch:26.07-py3` container. Alternative (`gb10/`, with its image build
for aarch64 / sm_121 and kernels): Reflection's vLLM Beam image (`vllm/vllm-openai:reflection-beam-arm64-cu130`), the vllm-exl3 plugin with
diffbot's Beam patch, and ExLlamaV3. The drafter is
reflection/Beam-1.0-501B-A23B-DFlash2 (Apache-2.0); download
it from Reflection, it is not included here.

- **Reflection** — Beam-1.0-501B-A23B weights, the DFlash2 drafter and the vLLM Beam image (Apache-2.0).
- **ashhart and the TensorFold contributors** — TensorFold (Apache-2.0), the engine our fork builds on.
- **diffbot** — the Beam EXL3 recipe this build ports to GB10:`requant_seq.py` , kernels and the vllm-exl3 Beam patch (MIT;
the patch is AGPL-3.0-only).
- **turboderp** — ExLlamaV3 (MIT).
- **vcruz305** — vllm-exl3 (AGPL-3.0).
- **vLLM project** — vLLM (Apache-2.0).
- **AtomicChat** — the BF16 reference logits used for the KL measurement.

Bertholomus AI is not affiliated with Reflection, diffbot, NVIDIA or any project named above.

- Downloads last month
- 310
