---
id: collect-261121-veille-ia/nvidia-hardware/nemotron-3-5-transcribe-launches-40-language-ai-audio
title: "Nemotron 3.5 Transcribe Launches: 40-Language AI Audio"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: []
keywords: ["transcribe", "audio", "nemotron", "nvidia", "model", "also", "open-weight", "speech"]
source: https://saeedai.com/nemotron-3-5-transcribe-launch/
source_anchor: ""
source_lines: [1, 51]
section: "NVIDIA_Hardware"
---
NVIDIA has released Nemotron 3.5 Transcribe, a free open-weight speech model that turns recorded audio into accurate text in up to 40 languages. Announced on October 28, 2026, it folds transcription, speaker labelling, translation and audio question answering into one promptable checkpoint, replacing the need to chain several separate tools. NVIDIA also shipped Granary v2, an open speech dataset and pipeline for fine-tuning the model on specialist vocabulary.

The launch lands in a busy month for voice AI. NVIDIA’s pitch differs: Transcribe is downloadable, so teams can run it on their own hardware and adapt it with their own data.

## What Was Announced?

Nemotron 3.5 Transcribe is a promptable model: hand it a recording and a plain-language instruction, and that instruction decides the output. Ask for a verbatim transcript and you get one; ask for speaker labels, a translation, or answers about the recording, and the same checkpoint delivers that instead — no multi-step pipeline required.

Under the hood, it is a 30-billion-parameter mixture-of-experts system that activates roughly 4 billion parameters per token. That sparse design keeps it practical on a single NVIDIA GPU. NVIDIA says it leads open-weight rivals on transcription accuracy, including first place on the Hugging Face Open ASR Leaderboard for English.

Granary v2 is the companion release: an open dataset and curation pipeline covering 463,062 hours of audio across 48 languages, which developers can use with NVIDIA’s NeMo Speech tools to teach Transcribe industry jargon.

## Key Details

The headline numbers are strong. Transcribe handles 40 languages, translates 31 of them to and from English, labels up to eight speakers (with speaker attribution in 13 languages), and processes recordings up to an hour long with word-level timestamps. Users can also supply names, acronyms and domain terms so specialist vocabulary is spelled correctly.

Speed is another selling point: NVIDIA says one RTX PRO 6000 Blackwell Server Edition GPU at NVFP4 precision can process 100 hours of audio in under three minutes. On accuracy, Artificial Analysis places it at 2.8% AA-WER, tied with Mistral’s Voxtral Small for the top open-weight spot — despite activating far fewer parameters per token.

One limitation matters: Transcribe targets recorded audio, not live streams, so it is not built for real-time captions. NVIDIA’s smaller Nemotron 3.5 ASR model covers streaming use cases.

## Why It Matters

Transcription is one of the most established workplace uses of AI, and meetings and medical dictation are NVIDIA’s first targets. One model that transcribes, labels speakers, translates and answers questions removes a lot of plumbing for meeting assistants, call analytics and clinical documentation tools.

The open-weight release matters as much as the accuracy claims. Teams that run Transcribe themselves keep control over where audio is processed — crucial for healthcare, legal and financial work, where recordings often cannot leave a company’s own systems. They can also fine-tune it on their own vocabulary instead of waiting for a vendor to support a niche accent.

The launch also extends the Nemotron 3.5 family. Earlier this month NVIDIA released Nemotron 3.5 Super VL, a multimodal model for always-on enterprise agents; Transcribe adds the speech input those agents increasingly need.

## Availability and What’s Next

Transcribe is available now on Hugging Face and ModelScope under the OpenMDW-1.1 licence, for commercial and non-commercial use. NVIDIA also offers it through NGC and Build, hosted endpoints are available from Baseten and DeepInfra, and there is an interactive demo on Hugging Face Spaces.

Quantized versions are coming soon, which should make local runs on RTX Spark, DGX Spark and Jetson AGX Thor systems more practical. Next, watch for independent real-world testing: noisy rooms, overlapping speakers and heavy accents are where transcription models truly prove themselves.

The speech field is moving fast, from Suno Speech to Grok Voice 3.0. For full technical details, see NVIDIA’s official Transcribe and Granary v2 announcement.

## FAQs

### What is Nemotron 3.5 Transcribe?

Nemotron 3.5 Transcribe is NVIDIA’s open-weight speech model, released on October 28, 2026. It converts recorded audio into text in up to 40 languages, and can also label speakers, translate audio, and answer questions about a recording, all from a single model run on your own hardware.

### How many languages does Nemotron 3.5 Transcribe support?

The model transcribes 40 languages, translates 31 of them to and from English, and attributes speakers in 13 languages. It labels up to eight speakers in one recording and handles audio files up to an hour long, with word-level timestamps in the output.

### Is Nemotron 3.5 Transcribe free to use?

Yes, the weights are free to download from Hugging Face and ModelScope under the OpenMDW-1.1 licence, covering commercial and non-commercial use. You still need NVIDIA GPU hardware to run it, or you can use a hosted endpoint from a third-party provider.

Follow Saeed AI for daily AI news.
