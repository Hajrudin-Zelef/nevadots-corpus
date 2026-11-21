---
id: collect-261121-veille-ia/nvidia-hardware/nemotron-3-5-transcribe-nvidia-s-promptable-open-asr
title: "Nemotron 3.5 Transcribe: NVIDIA's promptable open ASR"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: []
keywords: ["nvidia", "audio", "model", "languages", "english", "open", "nemotron", "transcribe"]
source: https://www.neuraldaily.news/en/a/nemotron-3-5-transcribe-granary-v2-nvidia-open-asr
source_anchor: ""
source_lines: [1, 33]
section: "NVIDIA_Hardware"
---
## What launched?

On October 28 NVIDIA published Nemotron 3.5 Transcribe on Hugging Face and ModelScope (blog post). A single checkpoint handles transcription, speaker attribution (diarization), translation and audio question answering.

- Transcribes 40 languages, translates 31 of them to or from English and attributes speakers in 13.
- Handles recordings up to an hour long with up to eight speakers, plus word-level timestamps.
- 30B parameters with about 4B active per token: a speech encoder feeds a hybrid Mamba-Transformer mixture-of-experts language model.
- Runs on a single NVIDIA GPU, from Jetson AGX Thor and DGX Spark to Blackwell servers. Serve it with vLLM or locally with NeMo-Speech.cpp.
- Built for recorded audio, not live streaming.

Output is steered with natural-language prompts: the same meeting recording can yield a speaker-labeled transcript, an English translation or a list of decisions. Prompts can also carry domain term lists, such as medical vocabulary, with an instruction to use them only when the audio supports them. The license is OpenMDW 1.1 (model card).

## What the numbers show

According to NVIDIA, the model ranks first among open-weight models on the English Hugging Face Open ASR Leaderboard at 5.33% average word error rate (WER). It posts 4.62% on the multilingual track and 10.98% on long-form audio.

On Artificial Analysis's independent ranking it ties for first among open-weight models at 2.8% AA-WER, using about 4B active parameters versus 24B for the model it ties. For diarization it has the lowest error rate (18.26% DER) on David AI's 21-language leaderboard and ranks first in VoiceArena's 14-system comparison.

Source: Artificial Analysis, via NVIDIA blog. Word error rate versus active parameters for open-weight models. Lower is more accurate.

Throughput: on one RTX PRO 6000 Blackwell Server Edition GPU with NVFP4, it processes 100 hours of audio in under three minutes, measured on 30-second clips at high concurrency.

## Granary v2 and fine-tuning

Granary v2 is the public part of the training data pipeline, built on NeMo Curator. The released manifest covers 463,062 hours across 48 languages from 16 source corpora, up from 25 languages in v1 (dataset). Each recording comes with three transcript forms (verbatim, normalized with and without disfluencies), plus translations, quality flags and Q&A pairs. Source audio has to be downloaded separately under each corpus's license.

NVIDIA's medical case study shows what adaptation buys. On recordings of Indian doctors, a better prompt cut WER from 30.8% to 23.8%. After fine-tuning, WER on held-out speakers dropped from 22.44% to 12.17%, a 45.8% relative reduction. The full adaptation took about 14 GPU-hours on one GB300, while general English WER rose slightly from 1.54% to 1.69%.

## Why it matters

Transcription, speaker labeling and translation usually mean stitching several models together. Here one open model you can self-host does all of it, controlled by a prompt. That also matters for privacy, since audio does not have to pass through an external API.

Practical tip: test it on your own recordings before going to production, especially for smaller languages. The benchmarks focus on English and major European languages, and names and speaker changes still need a spot check with any ASR model.
