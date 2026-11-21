---
id: collect-261121-veille-ia/muse/muse-research-muse2-125m-base-hugging-face
title: "Muse-research/Muse2-125M-Base · Hugging Face"
domain: muse
role: reference
task: article
actors: ["Meta"]
dates: ["2026-08-25"]
keywords: ["model", "import", "text", "tokens", "torch", "tokenizer", "muse", "muse2forcausallm"]
source: https://huggingface.co/Muse-research/Muse2-125M-Base
source_anchor: ""
source_lines: [1, 62]
section: "Muse"
---
Muse2-125M is the smaller sibling of Muse2-230M — the same from-scratch hybrid convolution/attention architecture, sized for tighter edge budgets. Both the base and instruction-tuned weights are included.

**Model Developer:** Muse Research

**Model Architecture:** Muse2 is an auto-regressive language model that uses a hybrid convolution/attention architecture implemented from scratch in PyTorch (`Muse2ForCausalLM`, no `transformers` code). The tuned version is aligned with supervised fine-tuning (SFT) with completion-only loss masking. No RLHF or DPO has been applied.

|  | Params | Input modalities | Output modalities | Context Length | GQA | Shared Embeddings | Knowledge cutoff | 
|---|---|---|---|---|---|---|---|
| Muse2-125M (Base) | 125M (122.9M) | English Text | English Text and code | 8k (128k max) | Yes | Yes | Early 2024 | 
| Muse2-125M (Instruct) | 125M (122.9M) | English Text | English Text and code | 8k (128k max) | Yes | Yes | Early 2024 | 

**Supported Languages:** English. The model also handles source code and mathematical text.

**Architecture Details:** 12 blocks interleaving 7 causal depthwise short-convolution blocks (kernel size 3) with 5 full-attention blocks; Grouped-Query Attention (12 query / 6 key-value heads); RoPE with theta 1e6; RMSNorm; parallel merged SwiGLU MLPs (hidden 768, ff 2304); tied input/output embeddings; vocabulary of 65,536 byte-level BPE tokens (pad=0, bos=1, eos=7, ChatML-style control tokens).

**Model Release Date:** Aug 25, 2026

**Status:** This is a static model. Later versions may be released that improve model capabilities.

**License:** Model weights are released under Apache-2.0.

**Intended Use Cases:** Muse2 is intended for research and education: studying compact hybrid architectures, edge/on-device inference, structured log-to-JSON extraction, formatting-disciplined output, and as a base for further fine-tuning. The Instruct version is intended for assistant-like chat in resource-constrained environments where a small footprint matters more than factual reliability.

**Out of Scope:** Production deployment without developer-side evaluation and guardrails. Any safety-critical, medical, legal, or factual-reliance use. Use in any manner that violates applicable laws or the upstream dataset licenses. The model has had **no safety alignment, red-teaming, or content filtering** applied.

This repository uses the from-scratch `muse` package (PyTorch + safetensors only).

**Base model** (text completion):

```
import torch
from tokenizers import Tokenizer
from muse import Muse2ForCausalLM
model = Muse2ForCausalLM.from_pretrained("./weights", dtype=torch.float32).eval()
tk = Tokenizer.from_file("./weights/tokenizer.json")
ids = [1] + tk.encode("The capital of France is").ids
out = model.generate(torch.tensor([ids]), max_new_tokens=40,
                     do_sample=False, eos_token_id=7)
print(tk.decode(out[0][len(ids):].tolist(), skip_special_tokens=True))
```
**Instruct model** (chat):

```
import torch
from tokenizers import Tokenizer
from muse import Muse2ForCausalLM
from muse.sft_data import apply_chat_template
model = Muse2ForCausalLM.from_pretrained("./Instruct", dtype=torch.float32).eval()
tk = Tokenizer.from_file("./Instruct/tokenizer.json")
ids = apply_chat_template(tk, [{"role": "user", "content": "What is 2+2?"}])
out = model.generate(torch.tensor([ids]), max_new_tokens=80,
                     temperature=0.2, top_k=50, eos_token_id=4)
print(tk.decode(out[0][len(ids):].tolist(), skip_special_tokens=True))
```
Muse2 is a research artifact released without safety fine-tuning, red-teaming, or content classification. It can produce inaccurate, biased, repetitive, or otherwise objectionable output, and it **hallucinates facts readily at this model scale**. Developers are solely responsible for deployment decisions and should add appropriate input/output safeguards for any application, and should not rely on the model for correctness.

**Values:** Muse2 is intended to make small-scale, from-scratch language-model development accessible — every component (tokenizer, architecture, inference stack) is open and reproducible.

**Testing:** Safety and capability testing to date is limited to qualitative inspection. It does not cover, nor could it cover, all scenarios. Before any deployment, developers should perform testing appropriate to their use case. Upstream dataset licenses and attribution requirements continue to apply to derivative uses.

- Downloads last month
- 213
