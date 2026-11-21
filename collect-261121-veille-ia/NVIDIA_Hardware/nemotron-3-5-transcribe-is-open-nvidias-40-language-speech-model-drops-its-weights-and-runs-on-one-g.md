---
id: collect-261121-veille-ia/nvidia-hardware/nemotron-3-5-transcribe-is-open-nvidias-40-language-speech-model-drops-its-weights-and-runs-on-one-g
title: "Nemotron 3.5 Transcribe is open: NVIDIA’s 40-language speech model drops its weights — and runs on one GPU | GPUJournal"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: []
keywords: ["model", "card", "nvidia", "speech", "open", "weights", "language", "about"]
source: https://gpujournal.ai/story/nemotron-3-5-transcribe-open-weights
source_anchor: ""
source_lines: [1, 21]
section: "NVIDIA_Hardware"
---
NEWS → · Models · NVIDIA

# Nemotron 3.5 Transcribe is open: NVIDIA’s 40-language speech model drops its weights — and runs on one GPU

Oct 28, 2026

Released by NVIDIA on Oct 28, 2026: an open-weight speech model that transcribes 40 languages, labels up to eight speakers, translates 31 languages to or from English and answers questions about a recording — 34 billion parameters in total, about 4.3 billion active, under the OpenMDW-1.1 licence, in BF16, FP8 and NVFP4 builds.

Image: NVIDIA

What shipped, from the model’s own Hugging Face card (nvidia/NVIDIA-Nemotron-3.5-Transcribe-30B-A4B), read Oct 28, 2026: the release date is Oct 28, 2026 across Hugging Face, GitHub (NVIDIA NeMo Speech), NGC and Build.Nvidia.com. The checkpoint pairs a Nemotron-3.5 Lightning 30B-A3B hybrid Mamba-2–Transformer mixture-of-experts language backbone with a 600M-parameter speech encoder and a roughly 99M-parameter speaker encoder — 34B parameters in total including the audio encoder, about 4.3B active per token. Context is 65,536 tokens and recordings can run up to one hour. Output is steered by plain-language prompts: verbatim or cleaned-up transcripts, speaker tags, word-level timestamps, translation, or questions about the audio. NVIDIA released Granary v2 alongside it — the open data pipeline behind the model, whose public manifest covers 463,062 hours across 48 languages and 16 source corpora, up from 25 languages in Granary v1.

The scores, all NVIDIA-reported on the card and its Hugging Face announcement, read Oct 28, 2026: first among open-weight models on the Hugging Face Open ASR Leaderboard for English at 5.33% average word error rate, the lowest multilingual result in its comparison at 4.62% mean WER across seven languages, and the lowest long-form result at 10.98%. On Artificial Analysis’s speech-to-text index it ties for first among open-weight models at 2.8% AA-WER while activating about 4B parameters, against 24B for the model it ties. For speaker attribution it posts the lowest diarization error rate of the 14 systems on David AI’s 21-language track, at 18.26%. These are the maker’s published figures, not our measurements — independent reproductions are the thing to watch.

The licence is part of the news. The weights are released under OpenMDW-1.1, and the card states plainly that the model is ready for commercial or non-commercial use — no revenue gate, no separate commercial licence to negotiate. FP8 and NVFP4 builds ship alongside the BF16 reference weights, and hosted endpoints are offered by Baseten, DeepInfra and Simplismart for anyone who would rather not self-host. One honest limit, in NVIDIA’s own framing: this is an offline model for recorded audio — internal chunking does not make it a live-streaming transcription service.

The renter maths is unusually friendly for a model this capable. The card’s own deployment line is a single H100 80GB, and the arithmetic agrees: 34B parameters at BF16 is roughly 68GB of weights, which fits one 80GB card; the FP8 build is roughly 34GB, inside a 48GB card with headroom; the NVFP4 build is roughly 17GB, inside a 24GB card. NVIDIA reports that with its optimised NIM implementation, one RTX PRO 6000 Blackwell Server Edition GPU transcribes up to 2,238 seconds of audio per second using NVFP4 with its DFlash2 speculative-decoding draft model — while noting, on the same card, that those serving optimisations are not yet part of the open-source release, and the shipped vLLM integration is expected to run about two times slower. In its medical fine-tuning case study, adaptation cut word error rate on held-out speakers by 45.8% relative to the best prompt-only result.

### What it means if you rent GPUs

Transcription is one of the steadiest GPU workloads there is, and it just got a strong open option that fits on a single rented card — at 4-bit, on a 24GB card. If you run speech pipelines, the build-versus-buy question now has a real open-weight candidate with a commercial-use licence and published numbers to test against. Test them: every score above is NVIDIA’s own until someone reproduces it.
