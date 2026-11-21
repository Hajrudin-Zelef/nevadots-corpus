---
id: collect-261121-veille-ia/liquid-lfm/orcarouter-lfm3-1-7b-abliterated
title: "orcarouter/LFM3-1.7B-abliterated"
domain: liquid-lfm
role: reference
task: article
actors: ["Liquid AI"]
dates: []
keywords: ["lfm3-1", "orcarouter", "b-abliterated", "ollama", "abliterated", "model", "context", "mix-8bit"]
source: https://ollama.com/orcarouter/LFM3-1.7B-abliterated:latest
source_anchor: ""
source_lines: [1, 161]
section: "Liquid_LFM"
---
ollama run orcarouter/LFM3-1.7B-abliterated

Updated 1 week ago

1 week ago

c159a329ba17 · 1.8GB

**LFM3-1.7B-abliterated**
*Abliterated (refusal-removed) LiquidAI LFM3-1.7B for Ollama — thinking · 262K context · runs on a laptop*

`LFM Open License v1.0` · `3 tags · 1.1–3.5 GB` · `Mix-8bit · Mix-4bit · BF16` · `thinking`

Website · Model Catalog · Hugging Face · GitHub · Discord · X

🛡️ **New from Orca — OrcaCyber Zero 1.5**

Post-trained for cybersecurity: vulnerability analysis, vulnerability reproduction, red teaming & offensive-security research. **98.01% pass@1 on CyberGym Level 1** · 1M context · function calling · OpenAI-compatible API.

Skip the download: OrcaRouter serves its cybersecurity and abliterated models behind one OpenAI-compatible API key.

🔌 **New from Orca — Orca Connect**

Running this in Ollama on your own machine? **Orca Connect** puts the rig under your desk behind your usual OrcaRouter API key, next to 200+ hosted models, reachable from a laptop, a phone or a cloud agent. Outbound only: no port forwarding, no tunnel, no firewall rule. **Requests your own hardware serves cost $0**, and `orca-connect claude` points Claude Code at your rig for one session.


These are Ollama builds of `orcarouter/LFM3-1.7B-abliterated`,
the **abliterated** (refusal-removed) `LiquidAI/LFM3-1.7B`.
LFM3-1.7B is a hybrid model: 24 layers, of which 13 are gated short-convolution blocks and 11
are grouped-query attention blocks (6 full, 5 sliding-window). Context is 262K tokens.

One refusal direction was removed from the BF16 weights before quantization. Harmful-prompt refusal drops from 87% to about 3%, and GSM8K and CMMLU are unchanged.

The two quantized builds are **mixed precision**. Each group of tensors got its own type
from **measured** KL divergence, at a fixed file size. They use stock llama.cpp types, so no
custom kernels are needed.

**At the same file size, both beat the stock llama.cpp types** on held-out text the search
never saw. Mix-8bit has 24% less KLD than Q8_0, and Mix-4bit has 16% less than Q4_K_M.

This model is **abliterated**: its **safety alignment has been substantially reduced**. It will
**comply with many harmful, unethical, offensive, or illegal requests** that the original
LFM3-1.7B would refuse.

| Tag | Size | bpw | KLD ↓ | Top-1 ↑ | JBB harmful refusal | 
|---|---|---|---|---|---|
| **`bf16`** | **3.47 GB** | 16.04 | (reference) | (reference) | 0% | 
| **`mix-8bit` · `latest`** | **1.84 GB** | 8.51 | 0.0025 | 97.8% | 0% | 
| **`mix-4bit`** | **1.10 GB** | 5.09 | 0.0509 | 89.2% | 0% | 

The KV cache is small. Only the 6 full-attention layers keep the whole context, at about 12 KB per token in F16. The 5 sliding-window layers keep 256 tokens each, and the 13 conv layers keep a fixed state. So 32K tokens of context take about 0.4 GB, and 128K tokens take about 1.6 GB.

Requires **Ollama v0.40.16 or later**.

```
ollama run orcarouter/LFM3-1.7B-abliterated               # Mix-8bit
ollama run orcarouter/LFM3-1.7B-abliterated:mix-4bit      # tight memory
ollama run --think=false orcarouter/LFM3-1.7B-abliterated # answer directly
```
**cURL**

```
curl http://localhost:11434/api/chat -d '{
  "model": "orcarouter/LFM3-1.7B-abliterated",
  "messages": [{"role": "user", "content": "Hello!"}]
}'
```
**Python**

```
from ollama import chat
resp = chat(
    model="orcarouter/LFM3-1.7B-abliterated",
    messages=[{"role": "user", "content": "Hello!"}],
)
print(resp["message"]["content"])
```
`message.thinking` and the answer
in `message.content`. Pass `"think": false` in the API, or `--think=false` on the command line,
to answer directly.
All builds are scored against the **abliterated BF16 GGUF** they were quantized from, using
llama.cpp’s full-vocabulary KLD (`llama-perplexity --kl-divergence`, real quantized kernels).
The teacher is the abliterated model, not the original, so the abliteration itself is not
counted as quantization error.

The text is the **whole wikitext-2 test split**: 143 windows of 2048 tokens. The precision search
used the validation split and the importance matrix used the train split, so neither saw this
text.

Each mixed build is compared with the stock llama.cpp types, quantized from the same BF16 with the same importance matrix and the same llama.cpp build.

|  | File | bpw | KLD ↓ | KLD p99 ↓ | Top-1 ↑ | PPL vs. BF16 | 
|---|---|---|---|---|---|---|
| **Mix-8bit** (ours) | **1.840 GB** | 8.51 | **0.0025** | **0.015** | **97.77%** | +0.3% | 
| `Q8_0` | 1.846 GB | 8.54 | 0.0033 | 0.021 | 97.68% | +1.3% | 
| `Q6_K` | 1.428 GB | 6.61 | 0.0194 | 0.177 | 94.22% | −4.2% | 
| `Q5_K_M` | 1.259 GB | 5.82 | 0.0239 | 0.177 | 93.24% | −1.7% | 
| **Mix-4bit** (ours) | **1.099 GB** | 5.09 | **0.0509** | **0.396** | **89.20%** | +3.3% | 
| `Q4_K_M` | 1.100 GB | 5.09 | 0.0605 | 0.464 | 88.51% | +2.2% | 
| `IQ4_XS` | 1.004 GB | 4.65 | 0.1085 | 0.926 | 85.18% | −4.5% | 

**Why not perplexity.** On this model, wikitext perplexity does not track quality. Q6_K,
Q5_K_M and IQ4_XS all score *lower* perplexity than the BF16 they were made from, while their
KLD rises as expected. A per-tensor ablation traced it to FFN quantization noise, which flattens
the output distribution and happens to suit wikitext. So the builds are ranked by KLD, which
measures distance from the BF16 directly.

**Method note.** These are full-vocabulary KLD and next-token agreement at 2K context, not a
downstream benchmark.


These numbers were measured on the abliterated BF16 in Hugging Face Transformers before conversion, against the original LFM3-1.7B, with identical prompts, splits and seed. Decoding was greedy with thinking disabled. Refusal is judged by a rule-based classifier, not an LLM judge, so treat the numbers as indicative.

**Harmful-prompt refusal (lower = more uncensored).** Average **2.8%**, against **87.3%** for the
original.

| Benchmark | n | Original | **Abliterated** | 
|---|---|---|---|
| AdvBench | 100 | 98.0% | **2.0%** | 
| JailbreakBench (harmful) | 100 | 93.0% | **2.0%** | 
| StrongREJECT | 150 | 82.0% | **1.3%** | 
| HarmBench (standard) | 150 | 81.3% | **4.0%** | 
| MaliciousInstruct | 100 | 95.0% | **0.0%** | 
| SimpleSafetyTests | 50 | 94.0% | **8.0%** | 
| ForbiddenQuestions | 150 | 68.0% | **2.0%** | 

**Over-refusal on benign prompts (lower = better).**

| Benchmark | n | Original | **Abliterated** | 
|---|---|---|---|
| XSTest-safe | 250 | 15.6% | **1.2%** | 
| JailbreakBench (benign) | 100 | 17.0% | **1.0%** | 

**Capability.** GSM8K and CMMLU are unchanged. MMLU drops 3.3 pp.

| Benchmark | n | Original | **Abliterated** | 
|---|---|---|---|
| MMLU | 300 | 49.0% | **45.7%** | 
| MMLU-Pro (CoT) ⚠ | 250 | 40.0% | **33.2%** | 
| GSM8K (CoT) | 150 | 86.0% | **86.0%** | 
| CMMLU | 500 | 35.0% | **34.6%** | 

⚠ **MMLU-Pro is a floor, not a measurement.** A third of the replies could not be parsed,
because a 1.7B model is often still reasoning when the 1024-token budget runs out.


| **Base model** | `LiquidAI/LFM3-1.7B` , abliterated as`orcarouter/LFM3-1.7B-abliterated` | 
| **Architecture** | `lfm3` in llama.cpp. 24 layers:**13 gated short-conv + 11 GQA attention** (6 full, 5 sliding-window 256); hidden 2048, SwiGLU MLP (intermediate 7680); 16 query / 4 KV heads × 128; tied embeddings | 
| **Weights** | 1.73B, of which 0.26B is the tied embedding / output head | 
| **Abliteration** | One refusal direction (diff-of-means, hidden-state layer 11) orthogonalized out of 49 residual-stream writers: attention `o_proj` , conv`out_proj` , MLP`w2` and the token embedding, in BF16 | 
| **Quantization** | Per-group mixed precision found by measured-KLD search at a fixed file size, from the abliterated BF16. Stock llama.cpp types, with an importance matrix | 
| **Context** | 262,144 tokens (inherited; evaluated at 2K) · **Vocabulary** 128,000 | 
| **Runtime** | llama.cpp master (PR #31252 or later) or Ollama v0.40.16+; text in, text out | 

`LiquidAI/LFM3-1.7B`.
The **LFM Open License v1.0**, inherited from the base model
`LiquidAI/LFM3-1.7B`. Abliteration and
quantization do not change the license obligations. See the license for its terms and
prohibited uses.

**Route Smarter · Ship Safer · Spend Less**
