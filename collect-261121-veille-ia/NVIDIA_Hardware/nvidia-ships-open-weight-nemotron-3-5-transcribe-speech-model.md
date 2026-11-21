---
id: collect-261121-veille-ia/nvidia-hardware/nvidia-ships-open-weight-nemotron-3-5-transcribe-speech-model
title: "NVIDIA ships open-weight Nemotron 3.5 Transcribe speech model"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: []
keywords: ["model", "nvidia", "open-weight", "nemotron", "speech", "about", "error", "rate"]
source: https://www.deai.org/news/nvidia-nemotron-35-transcribe-30b-open-weights
source_anchor: ""
source_lines: [1, 67]
section: "NVIDIA_Hardware"
---
NVIDIA released Nemotron 3.5 Transcribe, an open-weight speech model that turns a recording into a transcript, a translation, or a speaker-labeled dialogue depending on the prompt you give it. Speech is the one modality most self-hosted stacks still send to a hosted API, and this release is NVIDIA's bid to close that gap: one checkpoint handles 40 transcription languages with up to 8 speakers, and it runs on a single GPU.

## Key facts

- Nemotron 3.5 Transcribe has 34B total parameters with about 4.3B active per token, a hybrid Mamba-2–Transformer mixture-of-experts architecture, and ships in BF16, FP8, and NVFP4 under the OpenMDW-1.1 license.
- NVIDIA reports 5.33% average word error rate, first among open-weight models on the Hugging Face Open ASR Leaderboard for English, where API-only models are excluded.
- Artificial Analysis independently measures 2.81% AA-WER for the model, tying the best open-weight competitor, Mistral's Voxtral Small, while using roughly one-sixth of its active parameters (about 4B versus 24B).
- On David AI's DAI-ASR-I18N diarization track the model posts the lowest error rate of 14 systems tested, 18.26% across 21 languages, ahead of NVIDIA's own dedicated Nemotron 3 Diarization model at 18.38%.
- A published medical fine-tuning case study cut held-out word error rate from 22.44% to 12.17% in about 14 GPU-hours on one NVIDIA GB300, using mostly synthetic training speech.
- The Granary v2 dataset release covers 463,062 hours across 48 languages in its public manifests, up from 25 languages in Granary v1.

## What happened

NVIDIA published the checkpoint, the dataset, and the fine-tuning tooling on Hugging Face and ModelScope on October 28, 2026. The model card describes a Speech-Augmented Language Model built on the company's Nemotron 3.5 Lightning 30B text model: a Mamba-2–Transformer hybrid mixture-of-experts with a parallel expert encoder that fuses acoustic features and speaker cues into the language model. Because only about 4.3B of the 34B parameters activate per token, the model card lists a single H100 80 GB as the deployment target for full-precision serving.

The promptable design is the part that changes how the model gets used. One instruction turns the same meeting recording into a verbatim transcript with speaker labels, an English translation, or a summary of the decisions made, and prompts can carry domain terms such as drug names that the model is told to use only when the audio supports them. Recordings run up to one hour, word-level timestamps are supported, and audio question answering answers questions about the content of a clip. The model card is explicit about limits: audio question answering was trained in English, features vary by language, and the model is built for recorded audio, not live streaming.

The benchmark results NVIDIA cites span three independent evaluators. On the Hugging Face Open ASR Leaderboard, the model ranks first among open-weight entries for English at 5.33% average word error rate, with API-only models excluded from that track. Artificial Analysis, which runs its own speech-to-text index, places the model at 2.81% AA-WER, on the open-weight accuracy-versus-size Pareto frontier. For speaker attribution, the model posts the lowest diarization error rate on David AI's 21-language leaderboard and the lowest concatenated word error rate in VoiceArena's 14-system comparison, where it drops 8.32% of reference words against a median of roughly 31% across systems. NVIDIA's claim that several widely used commercial speech APIs trail it on Artificial Analysis's all-model chart is the company's own framing and has not been independently verified by us.

Throughput numbers come with a caveat worth reading closely. NVIDIA benchmarked one RTX PRO 6000 Blackwell Server Edition GPU at up to 2,238 seconds of audio transcribed per second with the forthcoming NVFP4 quantized build, and says a 30-second clip returns in 0.13 seconds. Those serving optimizations are not in the open-source release yet; the vLLM integration that shipped on day one is, by NVIDIA's own note, roughly two times slower and is intended for development. The quantized versions and a NeMo-Speech.cpp local runtime are listed as coming soon, targeting RTX Spark, DGX Spark, and Jetson AGX Thor class devices.

## Why it matters

Audio is the modality where "run it yourself" has lagged furthest behind. Text models reached credible open-weight parity years ago, image models are close, but most teams that need transcription, diarization, and translation have still pointed a hosted API at their recordings, which means their conversations leave their infrastructure. A checkpoint that combines all three tasks, runs on one GPU, and carries a permissive license gives self-hosters and privacy-sensitive deployments a first-party alternative to evaluate rather than settle for.

The open dataset matters as much as the model. Granary v2 ships the curation pipeline along with the corpus, so a team with domain-specific audio, legal depositions, call centers, clinical dictation, can reproduce the pipeline on its own recordings. The medical case study shows the payoff curve: naming the language and alphabet in the prompt cut word error rate from 30.8% to 23.8% before any training, and fine-tuning on about 153 hours of synthetic speech plus public corpora brought held-out speakers down to 12.17%. A 14-GPU-hour adaptation budget is small enough for a single team to budget.

This release also continues a visible pattern in NVIDIA's speech portfolio this year. The company shipped Nemotron 3 Diarization, a dedicated 100M-parameter speaker model, in September, and Nemotron 3.5 Transcribe now absorbs that job along with translation and audio understanding. Competitors are choosing different axes: Mistral's Voxtral line, the closest open-weight peer on accuracy, is pushing real-time voice agents with Voxtral 3, while Nemotron 3.5 Transcribe deliberately does not support streaming at all.

## Background

Open-weight speech models have been improving quickly, but the practical stack has stayed fragmented: one model for transcription, another for diarization, a translation service for the rest. DeAI has tracked that consolidation pressure across the beat. Fish Audio's Drama 3 shipped editable speech generation with license limits that shape who can self-host it, Maya Research's Maya2 targeted native open-weight text-to-speech in our PULSE coverage, and Liquid AI's LFM2.5-24B showed small multimodal models running local agent workloads on consumer hardware. Transcribe extends the same arc into audio understanding, where the hosted-API default has been strongest.

The license choice also matters for the self-hosting decision. OpenMDW-1.1 permits commercial use, which puts the checkpoint in a different practical category from speech models shipped under restrictive community licenses. The trade-off to weigh is the inverse of the usual one: this model is optimized for recorded audio and batch transcription, so teams that need live captioning or streaming agents are not the audience, and NVIDIA says so directly in the model card.

## What's next

Watch three things. First, the quantized releases: NVFP4 and FP8 checkpoints are the difference between "runs on an H100" and "runs on a desk," and the local runtime timeline will determine whether the device story materializes. Second, the leaderboard numbers are already public and independently checkable, so the meaningful test is whether third-party replications of the diarization results hold up on real overlapping-speech audio rather than vendor-selected clips. Third, whether hosted endpoints (Baseten, DeepInfra, and Simplismart listed at launch) price transcription aggressively enough to compress the API market, or whether the model's real effect is moving audio workloads off hosted APIs entirely.

Questions

- What can NVIDIA Nemotron 3.5 Transcribe do?
- It is one open-weight checkpoint that transcribes speech in 40 languages, translates 31 of them to or from English, attributes up to 8 speakers (diarization in 13 languages), answers questions about a recording in English, and accepts natural-language prompts to control output format.
- Can you run Nemotron 3.5 Transcribe locally?
- Yes. The model card lists a single H100 80 GB for full-precision serving, and NVIDIA says quantized versions are coming for RTX Spark, DGX Spark, and Jetson AGX Thor. It is built for recorded audio only, with no real-time streaming support.
- What license does Nemotron 3.5 Transcribe use?
- OpenMDW-1.1, a permissive model license. The model card states the checkpoint is ready for commercial and non-commercial use. Developers should still check third-party license terms for any components they redistribute.
- How accurate is Nemotron 3.5 Transcribe?
- NVIDIA reports 5.33% average word error rate, first among open-weight models on the Hugging Face Open ASR Leaderboard for English. Artificial Analysis independently measures 2.81% AA-WER, tying the best open-weight model while using about one-sixth of its active parameters.
- What is the Granary v2 dataset?
- NVIDIA's open speech-training dataset and curation pipeline, released alongside the model. Its public manifests cover 463,062 hours of audio across 48 languages, with three transcript styles per recording and reusable curation code for developers' own corpora.

Sources

1. NVIDIA Nemotron 3.5 Transcribe and Granary v2: Intelligent Multilingual Transcription You Can Prompt and Adapt — NVIDIA on the Hugging Face blog
2. NVIDIA-Nemotron-3.5-Transcribe-30B-A4B model card — NVIDIA (model card)
3. Nvidia's open Nemotron 3.5 Transcribe combines transcription, translation, and speaker recognition — The Decoder
4. Hugging Face Open ASR Leaderboard — Hugging Face
5. Artificial Analysis speech-to-text leaderboard (non-streaming) — Artificial Analysis

About DeAI

DeAI is an independent publication covering open-weight AI models, private inference, and decentralized infrastructure — the tools for running AI you actually control. We test providers on price, privacy, and refusal behavior and publish the numbers, not the vibes. DeAI is powered by Morpheus (mor.org), a decentralized inference marketplace, and covers it on the same terms as every other provider.

Powered by Morpheus and StrandCMS

Morpheus is a decentralized inference marketplace, covered on the same terms as every other provider — we rank it wherever the data lands. StrandCMS is the open-source, agent-first framework this site is built on.
