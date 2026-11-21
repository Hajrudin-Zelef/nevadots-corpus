---
id: collect-261121-veille-ia/mistral/orcarouter-mistral-large-4-1t-a52b
title: "orcarouter/Mistral-Large-4-1T-A52B"
domain: mistral
role: reference
task: article
actors: ["Mistral AI"]
dates: []
keywords: ["ollama", "mistral-large-4-1t-a52b", "orcarouter", "reasoning", "model", "chat", "vision", "orca"]
source: https://ollama.com/orcarouter/Mistral-Large-4-1T-A52B
source_anchor: ""
source_lines: [1, 125]
section: "Mistral"
---
ollama run orcarouter/Mistral-Large-4-1T-A52B

**Mistral-Large-4-1T-A52B**
*The 1-trillion-parameter Mistral Large 4 for Ollama — 52B active · MLA + sparse attention · vision · 1M context*

`Mistral Ecosystem Open-Weight License` · `2 tags · 236 / 496 GB` · `256 GB or 512 GB machine` · `vision bundled` · `reasoning`

Website · Model Catalog · Hugging Face · GitHub · Discord · X

🛡️ **New from Orca — OrcaCyber Zero 1.5**

Post-trained for cybersecurity: vulnerability analysis, vulnerability reproduction, red teaming & offensive-security research. **98.01% pass@1 on CyberGym Level 1** · 1M context · function calling · OpenAI-compatible API.

🔌 **New from Orca — Orca Connect**

Running this in Ollama on your own machine? **Orca Connect** puts the rig under your desk behind your usual OrcaRouter API key, next to 200+ hosted models, reachable from a laptop, a phone or a cloud agent. Outbound only: no port forwarding, no tunnel, no firewall rule. **Requests your own hardware serves cost $0**, and `orca-connect claude` points Claude Code at your rig for one session.


These are Ollama builds of `Mistral-Large-4-1T-A52B`.
It is a 1T-parameter MoE with 128 routed experts (top-4) and one shared expert, 52B active per
token. It uses MLA attention, DeepSeek-style sparse attention (top-2048) and a 1M-token context.

Routed experts are 98% of the weights, so that is where the bits were spent. Each group of
expert layers got its own precision from **measured** KL divergence, not from a hand-set rule.

**At the same file size, both builds sit closer to the original than unsloth’s UD-IQ1_S and
UD-Q3_K_XL:** 20% lower KLD at 233 GB, 13% lower at 492 GB, on held-out text the search never saw.

**Vision included.** Every tag carries the vision projector for image input. Mistral’s DSpark
speculative-decoding drafter is in the GGUF repo for llama.cpp; Ollama does not run it.

| Tag | In memory | Download (Ollama) | Experts | KLD ↓ | Top-1 ↑ | PPL vs. original | 
|---|---|---|---|---|---|---|
| **`mix-256g` · `latest`** | **232.9 GB** | 236.4 GB | 1.72 bpw | 0.0975 | 91.6% | +8.7% | 
| **`mix-512g`** | **492.2 GB** | 495.6 GB | 3.72 bpw | 0.0129 | 96.9% | +0.75% | 

Use Mix-512G if you have the memory. Use Mix-256G to run the full model on half of it.

Requires **Ollama v0.40.16 or later**.

```
ollama run orcarouter/Mistral-Large-4-1T-A52B            # Mix-256G
ollama run orcarouter/Mistral-Large-4-1T-A52B:mix-512g
```
**cURL**

```
curl http://localhost:11434/api/chat -d '{
  "model": "orcarouter/Mistral-Large-4-1T-A52B",
  "messages": [{"role": "user", "content": "Hello!"}]
}'
```
**Python**

```
from ollama import chat
resp = chat(
    model="orcarouter/Mistral-Large-4-1T-A52B",
    messages=[{"role": "user", "content": "Hello!"}],
)
print(resp["message"]["content"])
```
**Image input**

```
ollama run orcarouter/Mistral-Large-4-1T-A52B "What is in this image? ./photo.png"
```
`"think": false` does not turn it off. The reasoning comes back in
`message.thinking`, the answer in `message.content`.`images` field of a message, or as a file path in the prompt with `ollama run`.
All builds are scored against a **Q8_0 GGUF of the official FP8 checkpoint**, using llama.cpp’s
full-vocabulary KLD.

The text is **Mistral Large 4’s own answers**. Prompts come from four sources:
- English chat (UltraChat 200k)
- non-English chat (WildChat)
- code (Magicoder OSS-Instruct)
- math reasoning (OpenR1-Math)

The Q8_0 model answered each one, sampled as the model card recommends (temperature 1.0, top_p
0.95), with reasoning effort drawn per prompt from low / medium / high. Each conversation was
rendered through the chat template with its reasoning as a `[THINK]` block. KLD is counted on the
**assistant tokens**, reasoning and answer, over 64 windows of 2048 tokens. Truncated answers were
dropped. The precision search used a separate draw and never saw this text.

Each build is compared with **unsloth’s dynamic GGUF of the same size**, from
unsloth/Mistral-Large-4-1T-A52B-GGUF.
Their published files were downloaded and scored with the same text, teacher and llama.cpp build.

|  | In memory | KLD ↓ | KLD p99 ↓ | Top-1 ↑ | PPL vs. original | 
|---|---|---|---|---|---|
| **Mix-256G** (ours) | **232.9 GB** | **0.0975** | **1.41** | **91.61%** | **+8.7%** | 
| unsloth `UD-IQ1_S` | 233.0 GB | 0.1220 | 1.82 | 90.84% | +10.9% | 
| **Mix-512G** (ours) | **492.2 GB** | **0.01285** | **0.193** | **96.93%** | **+0.75%** | 
| unsloth `UD-Q3_K_XL` | 492.8 GB | 0.01482 | 0.219 | 96.81% | +0.81% | 

**At the same size**, Mix-256G has **20% less KL divergence** than UD-IQ1_S, and Mix-512G has
**13% less** than UD-Q3_K_XL. Both are also a little smaller, and both win in the tail (KLD p99).

**Why the model’s own text.** Mistral Large 4 always reasons before it answers. On other
models’ answers (no reasoning block, someone else’s style) it makes confident misses on ~3% of
tokens even at Q8_0, and which of those misses a quant happens to flip dominated the KLD. The
same applies to the user turns, which are human-written. So the score covers what the model
itself writes.

**Method note.** These are full-vocabulary KLD and next-token agreement at 2k context, not a
downstream benchmark.


| **Base model** | `mistralai/Mistral-Large-4-1T-A52B` | 
| **Architecture** | `mistral-large4` in llama.cpp. 72 layers, hidden 6144, 64 heads,**MLA** (kv rank 512),**sparse attention** with a lightning indexer (top-2048), NoPE on every 4th layer, KV sharing in the later layers, 1 leading dense layer,**128 routed experts top-4** + 1 shared expert | 
| **Weights in these files** | 1.05T: 1.03T routed experts, 20B attention / shared expert / dense / embeddings / head. 4 of 128 experts active per token | 
| **Quantization** | Per-group mixed precision found by measured-KLD search, from a Q8_0 conversion of the official FP8 weights. Experts are IQ1_S–IQ2_XS (Mix-256G) or IQ3_S–IQ4_XS (Mix-512G). Uses an importance matrix. | 
| **Context** | 1,048,576 tokens (inherited; evaluated at 2k) · **Vocabulary** 204,800 | 
| **Runtime** | llama.cpp master (PR #31376 or later), or Ollama v0.40.16+; text and images in, text out; DSpark speculative decoding in llama.cpp only | 

`Mistral-Large-4-1T-A52B`.`78feec0` or later). Tools built on older
llama.cpp, such as older Ollama and LM Studio releases, will not load these files. Ollama v0.40.16
and later do.
The **Mistral Ecosystem Open-Weight License**, inherited from the base model
`mistralai/Mistral-Large-4-1T-A52B`. It is
a modified MIT license that allows commercial and non-commercial use. Operating a
Model-as-a-Service business with the model requires a separate commercial license from Mistral. See
the license for the terms.

**Route Smarter · Ship Safer · Spend Less**
