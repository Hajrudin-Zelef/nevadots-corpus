---
id: collect-261121-veille-ia/nvidia-hardware/nvidia-nemotron-3-5-transcribe-open-model-released-with-granary-v2
title: "NVIDIA Nemotron 3.5 Transcribe Open Model Released with Granary v2"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: []
keywords: ["model", "nvidia", "nemotron", "transcribe", "languages", "audio", "granary", "open"]
source: https://www.brocker.org/nvidia-nemotron-3-5-transcribe-granary-v2-open-model
source_anchor: ""
source_lines: [1, 38]
section: "NVIDIA_Hardware"
---
NVIDIA has released Nemotron 3.5 Transcribe, an open-weight speech model that combines transcription, speaker attribution, translation, and audio question answering in a single checkpoint. The model is available now on Hugging Face and ModelScope, accompanied by the Granary v2 multilingual data pipeline and NeMo Speech fine-tuning tools that let developers adapt the model to domain-specific vocabulary and accents.

Nemotron 3.5 can transcribe 40 languages and translate 31 either into or out of English; it also identifies speakers across 13 languages. It handles recordings up to one hour long and can label up to eight speakers. Developers guide output through natural-language prompts — for example, requesting speaker-labeled transcription, translation, or answers to questions about the audio content. The model runs on a single NVIDIA GPU, from Jetson AGX Thor and DGX Spark up to Blackwell data-center GPUs.

Among open-weight models for English, Nemotron 3.5 Transcribe leads the Hugging Face Open ASR Leaderboard with a 5.33% average word error rate (WER). It also posts the lowest mean WER of the open-weight models in NVIDIA’s model-card comparison, both on the multilingual track (4.62% across seven languages) and the long-form track (10.98%). The model achieves a 2.8% AA-WER on Artificial Analysis's Speech-to-Text Leaderboard, sharing the top position for open-weight models and placing it on the accuracy–active-parameter Pareto frontier. NVIDIA’s blog says about 4 billion of its 30 billion parameters are active per token; the model card counts 34 billion in total, 4.3 billion active, including the audio encoder. For speaker attribution, it achieves the lowest diarization error rate (18.26%) on David AI's 21-language benchmark and leads VoiceArena's 14-system comparison with a 32.4% concatenated minimum-permutation WER.

The Granary v2 dataset release expands to 463,062 hours of curated multilingual speech covering 48 languages and 16 source corpora, compared with 25 languages in Granary v1. For each recording, its reusable NeMo Curator pipeline generates three transcript formats: verbatim, inverse-text-normalized with disfluencies, and inverse-text-normalized without disfluencies. It also provides translations with quality scores, contextual ASR biasing terms, speech-grounded Q&A pairs, code-switching restoration, and audio captions. A medical ASR case study on Eka Care's Indian medical speech benchmark shows what prompting and fine-tuning each add. A prompt that specified English and Latin script lowered full-benchmark WER from 30.8% to 23.8%. Fine-tuning with synthetic and real medical speech then cut WER on held-out speakers from 22.44% to 12.17%, a 45.8% relative reduction from the best-prompt baseline. On the same held-out test set, the adapted model beat an open medical-specialist model (13.79% WER), as well as a widely used general-purpose ASR model (14.92% WER).

Nemotron 3.5 Transcribe is built for recorded audio, not live streaming. A single NVIDIA RTX PRO 6000 Blackwell Server Edition GPU running at NVFP4 precision processes 100 hours of audio in under three minutes, based on 30-second clips at high concurrency. The checkpoint can be served with vLLM or run locally with NeMo-Speech.cpp. Hosted endpoints are available from Baseten, DeepInfra, and Simplismart. The Granary v2 manifests, fine-tuning tutorials, and a coding-agent skill for automated workflow execution are all publicly accessible.

## Confirmed

- Nemotron 3.5 Transcribe open-weight checkpoint released on Hugging Face and ModelScope (model ID: nvidia/NVIDIA-Nemotron-3.5-Transcribe-30B-A4B)
- Single model handles transcription (40 languages), translation (31 languages to/from English), speaker attribution (13 languages), and audio Q&A (English)
- Supports recordings up to 1 hour, up to 8 speakers, word-level timestamps
- Runs on single NVIDIA GPU from Jetson AGX Thor / DGX Spark to Blackwell data-center GPUs
- Hugging Face Open ASR Leaderboard: 1st open-weight English (5.33% WER), 1st open-weight multilingual (4.62% mean WER, 7 languages), 1st open-weight long-form (10.98% mean WER)
- Artificial Analysis: ties 1st open-weight at 2.8% AA-WER (about 4B active parameters; the model card lists 34B total, 4.3B active, including the audio encoder); 1.8% WER on AA-AgentTalk held-out set
- David AI DAI-ASR-I18N: lowest diarization error rate (18.26%) across 21 languages among 14 systems tested
- VoiceArena Diarization-Bench: 1st of 14 systems at 32.4% cpWER; lowest deletions (8.32% vs ~31% median)
- Granary v2 dataset: 463,062 hours, 48 languages, 16 corpora; three transcript forms (tn_raw, itn_raw, itn_clean) plus captions, Q&A pairs, biasing terms, translations
- Medical case study: prompt cut WER 30.8% → 23.8%; fine-tuning brought held-out WER to 12.17% (45.8% relative reduction vs best prompt), beating medical-specialist (13.79%) and general-purpose (14.92%) models
- Fine-tuning recipe: ~14 GPU-hours on one NVIDIA GB300; LoRA on language model, trains encoder and projection; checkpoint averaging
- Hosted endpoints: Baseten, DeepInfra, Simplismart; local serving via vLLM or NeMo-Speech.cpp
- License: OpenMDW-1.1 per the model card, which says the model is ready for commercial or non-commercial use; Granary v2 source audio keeps each corpus’s original license

## Unknown

- Full training mixture and sampling weights for Nemotron 3.5 Transcribe (Granary v2 figures describe the dataset release, not the complete training data)
- Independent replication of leaderboard scores by third parties
- Performance on languages and dialects beyond the evaluated benchmarks
- Long-term maintenance and update cadence for the open model and dataset

## Our take

NVIDIA is shipping the model together with the data pipeline behind its training and the fine-tuning workflow, which is what teams need to adapt open ASR to their own vocabulary. The medical case study shows the order that works: a better prompt first, then fine-tuning, which cut held-out WER a further 45.8% relative to the best prompt. The model card lists the OpenMDW-1.1 license and allows commercial use.

*Series:* 1. NVIDIA Releases NemotronLabs VoiceChat 11B, an Open Full-Duplex Speech Model with Live Tool Calling · **2. NVIDIA releases Nemotron 3.5 Transcribe open model with Granary v2 data pipeline** · NVIDIA Nemotron & NeMo
