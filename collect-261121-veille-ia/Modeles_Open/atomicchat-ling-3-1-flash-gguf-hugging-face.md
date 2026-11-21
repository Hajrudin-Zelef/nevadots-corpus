---
id: collect-261121-veille-ia/modeles-open/atomicchat-ling-3-1-flash-gguf-hugging-face
title: "AtomicChat/Ling-3.1-flash-GGUF · Hugging Face"
domain: modeles-open
role: reference
task: article
actors: []
dates: []
keywords: ["file", "ling-3", "ad-q4", "model", "atomicchat", "flash-gguf", "bf16", "experts"]
source: https://huggingface.co/AtomicChat/Ling-3.1-flash-GGUF
source_anchor: ""
source_lines: [1, 89]
section: "Modeles_Open"
---
*GGUF builds of inclusionAI's Ling 3.1 flash with our own importance matrix. The calibration corpora behind our builds are public.*

- Built by the team behind Atomic Chat, the app that runs open-weight models locally and offline.
- Runs in llama.cpp build b11571 or newer.

| File | Size on disk | KL divergence | top-1 | 
|---|---|---|---|
| `AD-Q8_0` | 600.8 GB | 0.00503 | 97.88% | 
| `AD-Q6_K` | 478.0 GB | 0.00557 | 97.72% | 
| `AD-Q5_K_M` | 400.2 GB | 0.00766 | 97.34% | 
| `AD-Q4_K_M` | 354.4 GB | 0.01114 | 96.86% | 
| `AD-IQ4_XS` | 337.8 GB | 0.01212 | 96.64% | 
| `AD-IQ4_XXS` | 311.4 GB | 0.01391 | 96.46% | 
| `AD-IQ3_M` | 277.1 GB | 0.02564 | 95.27% | 
| `AD-IQ3_XXS` | 256.4 GB | 0.03436 | 94.48% | 
| `AD-IQ2_M` | 218.1 GB | 0.05927 | 92.72% | 
| `AD-IQ2_XS` | 198.2 GB | 0.07972 | 91.64% | 
| `AD-IQ2_XXS` | 174.1 GB | 0.11133 | 90.10% | 
| `AD-IQ1_M` | 160.9 GB | 0.16597 | 87.86% | 
| `AD-IQ1_S` | 141.8 GB | 0.20448 | 86.41% | 

`KL divergence` is how far the quantized model's predictions drift from the
BF16 model's. Lower is better, and zero means identical. `top-1` is how often it
picks the same next token the BF16 model would have picked. Both are measured
on held-out text; how is at the bottom.

The two 1-bit files are experimental: they matched the BF16 model's token choice 86 to 88% of the time here, so check them on your own tasks first.

`AD-` marks an **Atomic Dynamic** layout: the routed experts (97% of the weights)
take the low-bit type, while attention, the shared expert, the router and the
dense layers stay at q8_0 or higher. The file name is the type of the experts'
gate and up projections; their down projections, and all experts of the last
five blocks, get a step more.

The file has to fit, plus a little for the context. Context is cheap on this model: 63 of its 72 layers use linear attention with a fixed-size state, and only 9 keep a compressed attention cache. That is about 0.5 GB per conversation plus about 10 KB per token.

| Total memory (VRAM or unified) | File | 
|---|---|
| 192 GB | `AD-IQ1_M` | 
| 256 GB | `AD-IQ2_M` | 
| 384 GB | `AD-Q4_K_M` | 
| 512 GB | `AD-Q5_K_M` | 
| 640 GB | `AD-Q8_0` | 

On a Mac, raise the GPU memory limit first, for example
`sudo sysctl iogpu.wired_limit_mb=240000` on a 256 GB machine.

```
llama-server -hf AtomicChat/Ling-3.1-flash-GGUF:AD-Q4_K_M -ngl 99 -c 32768 --jinja
```
Each file sits in a folder of its name, split into shards of up to 45 GB; point llama.cpp at the first shard and it loads the rest:

```
hf download AtomicChat/Ling-3.1-flash-GGUF --include "AD-Q4_K_M/*" --local-dir .
llama-server -m AD-Q4_K_M/Ling-3.1-flash-AD-Q4_K_M-00001-of-00009.gguf -ngl 99 -c 32768 --jinja
```
The chat template is inside every file. Thinking is on by default. The template
switches it with `enable_thinking`, so a system prompt alone does not turn it
off; start the server with

```
llama-server -hf AtomicChat/Ling-3.1-flash-GGUF:AD-Q4_K_M -ngl 99 -c 32768 --jinja --chat-template-kwargs '{"enable_thinking": false}'
```
or send `"chat_template_kwargs": {"enable_thinking": false}` with a request.

Reference: BF16 weights converted from inclusionAI's FP8 release and run unquantized (perplexity 2.3590 on the held-out set). Converting FP8 to BF16 rounds each weight once, far below what Q8_0 adds.

Held-out text: `eval_neutral` from our
calibration dataset,
never used for calibration, 144 chunks of 4096 tokens. Metric: per-token KL
divergence of each file's predictions against the reference, plus top-1
agreement. Hardware: 8x A100-SXM4-80GB, llama.cpp 097cf33d9 for every file.

The importance matrix is the one we computed on 5.5M tokens of the
`ling3-moe` corpus (thinking on and off, tool calls, 30 languages, code, long
documents up to 32k tokens), rendered through this model's own chat template.

Mixture of experts: 512 routed experts in each of 68 layers (the first 4 layers are dense), 8 routed plus 1 shared expert per token, about 24B parameters active of 564B. Hybrid attention: 63 Kimi Delta Attention (linear) layers and 9 multi-head latent attention layers. Hidden size 4096, vocabulary 157,184, context 262,144. The multi-token-prediction head of the release is not included.

Built from inclusionAI's FP8 release (revision `b426279f`), converted to BF16
and quantized with llama.cpp b11571 (097cf33d9); needs llama.cpp with
`bailingmoe3` support.

MIT, as the original. Base model **Ling 3.1 flash** by
inclusionAI. Quantization and evaluation
by **AtomicChat**.

- Downloads last month
- 1,831
