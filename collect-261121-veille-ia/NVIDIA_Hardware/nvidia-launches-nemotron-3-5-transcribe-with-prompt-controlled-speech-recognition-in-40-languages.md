---
id: collect-261121-veille-ia/nvidia-hardware/nvidia-launches-nemotron-3-5-transcribe-with-prompt-controlled-speech-recognition-in-40-languages
title: "NVIDIA Launches Nemotron 3.5 Transcribe With Prompt-Controlled Speech Recognition in 40 Languages"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: []
keywords: ["nvidia", "speech", "model", "nemotron", "transcribe", "audio", "transcript", "languages"]
source: https://www.tao.media/nvidia-launches-nemotron-3-5-transcribe-with-prompt-controlled-speech-recognition-in-40-languages/
source_anchor: ""
source_lines: [1, 65]
section: "NVIDIA_Hardware"
---
## Table of Contents

NVIDIA has released Nemotron 3.5 Transcribe, an open-weight speech model that lets developers specify how a recording should be transcribed through natural-language instructions. The same checkpoint can produce a verbatim transcript, remove fillers, label speakers, translate speech, or answer questions about the audio.

The model transcribes 40 languages and processes recordings up to an hour long, with labels for up to eight speakers. Translation to or from English covers 31 languages, while speaker-attributed transcription supports 13. Audio question answering is trained in English.

NVIDIA also released Granary v2, a multilingual speech-data release and reusable curation pipeline, alongside fine-tuning recipes in NeMo Speech. Together, they give developers control over both the output requested at inference time and the training examples used to adapt recognition to particular accents, terminology, and writing conventions.

The launch follows Nemotron 3.5 Super VL, NVIDIA's open-weight model for agents working with documents, images, video, and tools. Transcribe focuses on recorded speech rather than live streaming.

## How Prompts Control the Transcript

A transcript for legal review may need every hesitation and repetition, while subtitles may need cleaner wording and conventional punctuation. Nemotron 3.5 Transcribe lets an application choose between those outputs for each request, without retraining the model.

Instructions can also specify whether numbers appear as spoken words or as digits and symbols. A list of expected names, products, or medical terms supplies vocabulary context. In NVIDIA's ContextASR-Bench evaluation, providing expected entities reduced named-entity errors sixfold.

For meetings, the model can generate speaker tags alongside the words. Diarization identifies who spoke when within a recording, allowing an application to separate turns without establishing a speaker's real-world identity. Word-level timestamps can then connect individual words to playback. Agora used that capability in a classroom demonstration that displays a transcript beside a recorded lesson.

Underneath those controls is a speech-augmented language model. An audio encoder converts sound into representations that a language model uses to generate text. NVIDIA combines a Transformer speech encoder with its frozen Nemotron 3 Diarization model and the Nemotron 3.5 Lightning 30B-A3B language backbone, a hybrid Mamba–Transformer mixture-of-experts system.

The Parallel Expert Encoder adds speaker-activity information to the speech representations before they enter the language model. That gives the decoder speaker cues while it writes the transcript, rather than leaving all speaker assignment to a later processing step. A lightweight connectionist temporal classification, or CTC, adapter aligns the generated words with the audio afterward to produce timestamps.

The full system has 34 billion parameters, including its audio components, with approximately 4.3 billion active. Mixture-of-experts routing uses only part of the model's parameter pool for each token. Its context window is 65,536 tokens, accommodating the instructions, audio representations, and generated response.

## Transcription and Speaker-Attribution Results

NVIDIA reports leading open-weight results across English, multilingual, and long-form speech recognition. Word error rate, or WER, counts substitutions, deletions, and insertions against a reference transcript; lower scores indicate fewer errors.

The launch article on Hugging Face shows 5.33% average WER for the English Open ASR leaderboard. NVIDIA's research evaluation separately reports 4.71% across nine public English test sets, alongside 4.62% for the seven-language multilingual comparison and 10.98% for long-form audio.

On Artificial Analysis, NVIDIA reports that Transcribe ties for first among open-weight models at 2.8% AA-WER, its speech-recognition error index. The comparison places Transcribe at about 4 billion active parameters, versus 24 billion for the model it ties. On AA-AgentTalk, a held-out voice-agent speech set that contributes half of the index, Transcribe scores 1.8% WER, compared with 2.3% for the next-best open-weight model.

Speaker evaluations measure a different part of the task. On David AI's diarization benchmark across 21 languages, Transcribe records an 18.26% diarization error rate, the lowest among 14 tested systems. That metric measures errors in assigning speaker time, rather than transcription accuracy alone.

On VoiceArena's English Diarization-Bench, which covers full sessions with overlapping speech and two to eight speakers, Transcribe ranks first among 14 systems at 32.4% concatenated minimum-permutation WER. This metric counts word errors after matching predicted speaker labels to reference speakers. NVIDIA also reports an 8.32% word-deletion rate, versus a median of about 31% across the comparison.

## Granary v2 and Medical Speech Adaptation

Granary v2 provides a public manifest covering 463,062 hours across 48 languages and 16 source corpora, expanding from 25 languages in Granary v1. Those figures describe the dataset release, not the model's entire training mixture.

Built with NeMo Curator, the pipeline produces three aligned transcript forms for a recording: spoken-form text with fillers, written-form text with fillers, and written-form text without them. An utterance containing "twenty-five euros," for example, can retain those words or become "€25," depending on the target convention.

The pipeline also generates translations, audio captions, speech-grounded question-and-answer pairs, and contextual vocabulary hints. Its records include provenance and quality information, making the relationship between an instruction and the expected text explicit during training.

Developers can reuse the curation workflow on their own recordings or authorized public corpora. Source audio must be downloaded separately under each corpus's original license.

NVIDIA's medical adaptation study shows how prompt changes and training address different errors. On Eka Care's benchmark of Indian medical speech, specifying English and the Latin alphabet reduced full-set WER from 30.8% to 23.8%. Fine-tuning brought it to 11.8%, approximately 62% below the out-of-the-box result.

That full set includes clips used to select prompts and checkpoints. On the separate held-out speakers, fine-tuning reduced WER from 22.44% with the best prompt to 12.17%, a 45.8% relative reduction.

The adaptation combined approximately 153 hours of synthetic speech with 29 hours of accented English and 63 hours of medical recordings. The recipe trained the speech encoder and projection while adapting the language model through low-rank adaptation, or LoRA. The full workflow, from text generation through evaluation, took about 14 GPU-hours on one NVIDIA GB300.

Separately, NVIDIA's technical report describes adaptation across 17 tasks, typically requiring 0.5 to 2.3 GPU-hours per task on a single GB300. The longer medical workflow includes data generation and evaluation as well as model training.

## Deployment and Available Weights

The main checkpoint on Hugging Face is released under OpenMDW-1.1 for commercial and noncommercial use, with distribution also available through ModelScope. NVIDIA provides a vLLM deployment example using one H100 with 80 GB of memory.

The current research page lists BF16, FP8, NVFP4, and GGUF checkpoints, plus an optional DFlash2 draft model. The launch blog still describes quantized versions and local deployment through NeMo-Speech.cpp as forthcoming.

For faster decoding, Transcribe includes native multi-token prediction, which proposes additional tokens for verification, or can use the separate DFlash2 model. NVIDIA reports throughput of up to 2,238 seconds of audio per second on one RTX PRO 6000 Blackwell Server Edition GPU using NVFP4, DFlash2, and the optimized Nemotron Transcribe NIM. The launch benchmark used 30-second clips at high concurrency.

Those NIM serving optimizations are not yet included in the open-source implementation. NVIDIA expects the release-day NeMo Speech vLLM integration, intended for model development, to run roughly twice as slowly.

Developers can use the released NeMo Speech fine-tuning recipes to adapt the model to new languages, domains, and code-switched speech, then export the adapted model for deployment.
