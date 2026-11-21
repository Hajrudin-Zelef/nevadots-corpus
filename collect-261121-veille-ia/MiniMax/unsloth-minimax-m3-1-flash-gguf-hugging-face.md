---
id: collect-261121-veille-ia/minimax/unsloth-minimax-m3-1-flash-gguf-hugging-face
title: "unsloth/MiniMax-M3.1-Flash-GGUF · Hugging Face"
domain: minimax
role: reference
task: article
actors: ["MiniMax"]
dates: []
keywords: ["context", "attention", "official", "minimax-m3", "flash", "bench", "sparse", "claude"]
source: https://huggingface.co/unsloth/MiniMax-M3.1-Flash-GGUF
source_anchor: ""
source_lines: [1, 92]
section: "MiniMax"
---
# Read our How to Run MiniMax-M3.1-Flash Guide!

**MiniMax-M3.1-Flash** is a native multimodal mixture-of-experts model for coding, agentic workflows, computer use, and cybersecurity. It supports text, image, and video inputs with a context window of up to **1M tokens**.

Building on MiniMax-M3, M3.1-Flash improves task performance and inference efficiency with sparse attention throughout the model, quantization-aware training, and DSpark speculative decoding.

**Highlights:**

- **Agentic performance:** A generational leap in agentic tasks, coding, and cybersecurity — 76.2 on BankerToolBench, 73.7% on DeepSWE v1.1, 61.7 on OSWorld V2, 29.6 on Agents' Last Exam, and 82.2 on CyberGym.
- **Efficient by design:** Sparse attention throughout, quantization-aware training, and DSpark speculative decoding improve inference efficiency.

- **Architecture:** Native multimodal mixture-of-experts (MoE)
- **Parameters:** Approximately 428B total; 23B activated per token
- **Inputs / output:** Text, image, and video inputs; text output
- **Context window:** Up to 1,000,000 tokens
- **Attention:** MiniMax Sparse Attention (MSA) throughout the model
- **Speculative decoding:** DSpark draft head included

| Benchmark | MiniMax M3.1 Flash | DeepSeek V4.1 Flash-1017 | DeepSeek V4.1 Flash | Kimi K3 | GLM-5.3 | GPT-5.6 Sol | Opus 5 | 
|---|---|---|---|---|---|---|---|
| Automation-Bench v1.0.6 | **62.8** | 60.5 | 54.8 | 46.7 | 48.8 | 45.8 | 50.3 | 
| BankerToolBench | **76.2** | — | 73.7 | 69.1 | 74.5 | 74.6 | **76.2** | 
| GDP.pdf AA | 20.8 | — | 12.8 | 22.0 | 11.2 | **27.2** | 21.6 | 
| Terminal Bench 4.0 | 38.1 | 42.3 | 31.2 | 12.6 | 37.9 | 39.9 | **51.8** | 
| LOCA-Bench 256K | **60.3** | — | — | 49.7 | 50.3 | 60.0 | 53.7 | 
| Agents’ Last Exam | 29.6 | — | — | 28.3 | — | 30.6 | **32.2** | 
| OSWorld 2.0 | 61.7 | — | 54.3 | 58.3 | — | 65.7 | **70.6** | 
| PostTrain Bench v1.1 | 39.3 | — | 30.5 | 36.6 | 37.5 | 36.2 | **40.4** | 
| DeepSWE v1.1 | 73.7 | **77.0** | 74.2 | 67.5 | 66.9 | 73.0 | 74.0 | 
| NL2Repo-Bench | 61.2 | 68.2 | 65.4 | 58.0 | 58.0 | 56.8 | **75.3** | 
| SWE Refactor Bench | 23.0 | — | 11.8 | 19.5 | 10.4 | 28.5 | **31.0** | 
| CyberGym | 82.2 | — | — | 80.0 | **84.5** | 83.6 | — | 
| BabyVision (with tools) | 91.0 | 92.5 | 89.6 | 85.7 | — | 88.9 | **94.1** | 
| HLE (without tools) | 43.4 | 42.6 | 39.2 | 43.5 | 42.3 | 49.5 | **55.6** | 
| CharXiv RQ | 82.5 | — | 69.0 | 84.8 | — | **85.2** | 83.7 | 

## **Evaluation Notes**

- **Automation-Bench v1.0.6:** 600 publicly released tasks.
- **BankerToolBench:** OpenCode 1.17.18; MiniMax-M2.7 judge.
- **GDP.pdf AA:** Taken directly from Artificial Analysis.
- **Terminal Bench 4.0:** Codex 0.149.1.
- **LOCA-Bench 256K:** Official implementation.
- **Agents’ Last Exam:** Official Overall setting; full-set results from the official ALE leaderboard only. Blank = no official result (CLI-only excluded), not zero.
- **OSWorld 2.0:** 0808 Full release; highest reported per model at release, except DeepSeek V4.1 (in-house reproduction).
- **PostTrain Bench v1.1:** Claude Code 2.1.251, 512K context; otherwise official setup.
- **DeepSWE v1.1:** mini-SWE-agent 2.4.5, 512K context.
- **NL2Repo-Bench:** Claude Code 2.1.233, 512K context; reward hacking = 0; two inconsistent tasks corrected (scores use the corrected set).
- **SWE Refactor Bench:** Claude Code 2.1.220, 200K context; otherwise official setup.
- **CyberGym:** Claude Code 2.1.263, 1M context.
- **BabyVision (with tools):** Claude Code 2.1.285; min image side 1,500 px.
- **HLE (without tools):** Text-only subset (2,158 questions).
- **CharXiv RQ:** GPT-5.5 judge.

Results are for the official weights; third-party quantizations may differ.

M3.1-Flash retains M3’s core mixture-of-experts architecture, with three main changes:

- **Sparse attention throughout:** the first three full-attention layers are replaced with sparse attention.
- **Quantization-aware training:** the routed MoE experts and the attention paths are quantized during training, reducing memory footprint and improving serving efficiency.
- **DSpark speculative decoding:** DSpark replaces the earlier EAGLE-like MTP approach. The release includes the draft head for use with compatible serving implementations.

`dspark-MiniMax-M3.1-Flash-Q8_0.gguf` is MiniMax's official DSpark draft head for MiniMax-M3.1-Flash (the `dspark/`
folder of the original repo), converted to GGUF at Q8_0 (about 2.1 GB). It reuses the main model's embeddings and
output head and reads its hidden states, so it only works together with a MiniMax-M3.1-Flash GGUF from this repo.
In our greedy tests it had the same acceptance rate as a BF16 conversion of the draft.

It needs a llama.cpp build with MiniMax-M3.1 support (including the `draft-dspark` speculative type). Example:

```
./llama-server \
    -m MiniMax-M3.1-Flash-UD-Q4_K_XL-00001-of-00007.gguf \
    -md dspark-MiniMax-M3.1-Flash-Q8_0.gguf \
    --spec-type draft-dspark --spec-draft-n-max 1 --spec-draft-p-min 0 \
    -ngl 99 -ngld 99 -np 1 --jinja
```
- Use `-np 1` (a single server slot); this is the configuration we tested.
- `--spec-draft-n-max 1` gave the best speed in our tests (about 1.5x on code and math, smaller gains on chat).
Larger values raise the tokens drafted per step but lower acceptance, and can be slower on open-ended chat.

FlagOS — see Multi-Chip Support and Downloads.

Recommended sampling settings: `temperature=1.0` and `top_p=0.95`.

For reasoning, set `reasoning_effort` in the chat template: `low`, `medium`, `high`, `xhigh`, or `max`.

The model supports a context window of up to 1M tokens. Usable context length and throughput depend on GPU memory and the serving configuration.

Contact us at model@minimax.io.

- Downloads last month
- 37,648
