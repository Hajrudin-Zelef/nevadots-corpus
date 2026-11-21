---
id: collect-261121-veille-ia/liquid-lfm/liquidai-lfm3-1-7b-gguf-hugging-face
title: "LiquidAI/LFM3-1.7B-GGUF · Hugging Face"
domain: liquid-lfm
role: reference
task: article
actors: ["Liquid AI"]
dates: []
keywords: ["lfm3-1", "llama", "liquidai", "b-gguf", "gguf", "model", "newer", "temp"]
source: https://huggingface.co/LiquidAI/LFM3-1.7B-GGUF
source_anchor: ""
source_lines: [1, 45]
section: "Liquid_LFM"
---
GGUF version of LFM3-1.7B for local inference on CPUs and GPUs.

Find architecture and usage details in the original model card.

Recommended sampling: temperature 0.1, top_k 50, repeat penalty 1.1. For math and science: temperature 1.0, top_p 0.95, top_k 20.

- Llama.app (**Use this model > Llama** in the menu), for macOS and Windows. Requires llama.cpp version v0.8.0 or newer.
- LM Studio (**Use this model > LM Studio** in the menu), for macOS, Windows and Linux. Requires LM Studio with llama.cpp engine 2.65.1 or newer.
- Ollama (`ollama run lfm3` ), for macOS. Requires Ollama version v0.40.12 or newer.

Add a quantization tag to pick a file.

```
llama-cli -hf LiquidAI/LFM3-1.7B-GGUF:Q4_K_M \
  --temp 0.1 --top-k 50 --repeat-penalty 1.1
```
For math and science, use `--temp 1.0 --top-p 0.95 --top-k 20` instead. More options in the llama.cpp guide.

Unlike the other files in this repository, which are converted from LFM3-1.7B,
`LFM3-1.7B-NVFP4.gguf` is converted from a separate repository,
LiquidAI/LFM3-1.7B-NVFP4, a checkpoint quantized to NVFP4 with NVIDIA Model Optimizer.
The feed-forward weights keep their NVFP4 quantization from that checkpoint, all other weights are Q8_0. On NVIDIA Blackwell GPUs (RTX 50 series, DGX Spark) the NVFP4 layers run on FP4 tensor cores.

Supported in llama.cpp b12088 and newer (#31608). Older builds load the file but produce incorrect output.


Example usage with llama.cpp:

```
llama-cli -hf LiquidAI/LFM3-1.7B-GGUF:NVFP4 \
  --temp 0.1 --top-k 50 --repeat-penalty 1.1
```
The Q4_0 build in this repository is the Quantization-Aware Distillation (QAD) checkpoint,
`LFM3-1.7B-QAD-Q4_0.gguf`.
It was trained to recover the accuracy lost to Q4_0 quantization.

Example usage with llama.cpp:

```
llama-cli -hf LiquidAI/LFM3-1.7B-GGUF \
  --hf-file LFM3-1.7B-QAD-Q4_0.gguf \
  --temp 0.1 --top-k 50 --repeat-penalty 1.1
```
- Downloads last month
- 430,243
