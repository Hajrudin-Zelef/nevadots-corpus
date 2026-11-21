---
id: collect-261121-veille-ia/nvidia-hardware/nemotron-3-5-transcribe-fasttoolhub
title: "Nemotron 3.5 Transcribe · FastToolHub"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: ["2026-10-28"]
keywords: ["model", "audio", "transcription", "nemotron", "nvidia", "language", "transcribe", "speech"]
source: https://fasttoolhub.com/en/terms/nemotron-3-5-transcribe/
source_anchor: ""
source_lines: [1, 36]
section: "NVIDIA_Hardware"
---
## Intro

**Nemotron 3.5 Transcribe** (Hugging Face repo `nvidia/NVIDIA-Nemotron-3.5-Transcribe-30B-A4B`) is an **open-weight, offline speech and audio model** that **NVIDIA** released on **2026-10-28** across Hugging Face, NGC, build.nvidia.com and NeMo Speech on GitHub. It takes a recording plus a text instruction and returns text: transcription in **40 languages**, speaker-attributed transcription, speech translation, word timestamps, language identification and audio question answering, for recordings up to **one hour** with a **65,536-token** context.

Model card: huggingface.co/nvidia/NVIDIA-Nemotron-3.5-Transcribe-30B-A4B · Live demo: nvidia/nemotron-transcribe · Companion dataset: Granary v2 · License: **OpenMDW-1.1** (commercial use allowed).

Advertisement

## Explain

## Key points

1. **Architecture** : a Speech-Augmented Language Model (SALM). The language model is a Mamba-2–Transformer hybrid Mixture of Experts with a Parallel Expert Encoder (Transformer ASR encoder + speaker encoder) in front.**34B** total parameters including the audio encoder, about**4.3B** active; base model is Nemotron 3.5 Lightning 30B-A3B.
2. **Precision and deployment** : BF16 reference weights plus FP8 and NVFP4 variants; the official example serves it with vLLM on a single**H100 80 GB** . Native Multi-Token Prediction speculative decoding, with an optional DFlash2 draft model.
3. **One promptable model, many tasks** : verbatim or written-style transcription, contextual (hot-word) transcription, multi-speaker transcription, rich transcription, speech translation, language ID and audio QA; a reasoning mode can be toggled via the chat template (`enable_thinking` ).
4. **Published results (model card, measured 2026-10-12)** : English Open ASR Leaderboard mean WER**4.71** across nine sets (next best Qwen3-ASR-1.7B at 4.94); long-form WER**10.98** ; multilingual 7-language mean WER**4.62** ; full-session multi-speaker mean cpWER**15.80** .
5. **Third-party evaluation** : on launch day Artificial Analysis said it**ties for #1** among open-weights speech-to-text models at**2.8% WER** , using only about 4B active parameters.

## Why it's tracked

A brand-new named model from a major lab in this scan window, with a full model card and independent benchmarks. It is a separate speech-focused line from the previously tracked Nemotron 3.5 Bolt and Super VL.

## Usage

## Quick start

1. Get one H100 80 GB (or check the hardware compatibility table on the model card), install vLLM and the latest NeMo Speech, and download the weights.
2. Set `VLLM_PLUGINS=nemo_speechlm` and the other environment variables, then run`vllm serve ./NVIDIA-Nemotron-3.5-Transcribe-30B-A4B --trust-remote-code ...` (full flags in the model card's Quick Start).
3. Call the OpenAI-compatible endpoint: put a text instruction (e.g. "Transcribe the following audio.") and a base64 `audio_url` in`messages` .
4. For transcription, translation and language ID use temperature 0 with reasoning off; for audio understanding use temperature 0.6, top_p 0.95 with reasoning on.

## Tips

- To try it without hosting, use the Hugging Face demo Space or build.nvidia.com.
- Speaker labels only separate speakers within one recording; they do not identify real people.
- If GPU memory is tight, start with the FP8 or NVFP4 checkpoints.
