---
id: collect-261121-veille-ia/nvidia-hardware/nvidia-opens-nemotron-3-5-transcribe-for-40-languages-a-mamafero
title: "Nvidia opens Nemotron 3.5 Transcribe for 40 languages Â· MamÃfero"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: []
keywords: ["nvidia", "model", "transcribe", "languages", "voice", "open", "nemotron", "october"]
source: https://www.mamiferolab.com/notes/nemotron-transcribe-open-voice
source_anchor: ""
source_lines: [1, 44]
section: "NVIDIA_Hardware"
---
## What happened

- On Wednesday, October 28, 2026, Nvidia released Nemotron 3.5 Transcribe, a free, open-weight speech recognition model that transcribes audio in up to 40 languages.
- According to the company, the model translates 31 languages to and from English, handles recordings up to an hour long and tells apart up to eight people speaking in the same conversation.
- Alongside the model, Nvidia released version 2 of Granary, its open speech dataset, and pitched it as a base for tuning apps to each industryâs jargon.
- The Hugging Face card names it NVIDIA-Nemotron-3.5-Transcribe-30B-A4B and offers quantized FP8 and NVFP4 variants.

## Why it matters

- Meeting transcription and medical transcription are two of the longest-standing uses of AI inside companies. An open model of this size can be installed on in-house servers, without sending recordings to a third party.
- Nvidia says the model runs on a single chip: from the Jetson AGX Thor robotics controller and the DGX Spark in new laptops to Blackwell data center processors. If that holds in practice, a hospital or a notary office could transcribe on a desktop machine.
- Speaker separation, up to eight voices, is what makes a meeting transcript useful: without it the text is one continuous block with no authorship.
- There is a stated limit: Transcribe works with recorded audio, not live streams, so it is not suited for simultaneous captions.

## The number

**40 languages.** That is the transcription coverage Nvidia claims for Nemotron 3.5 Transcribe, with translation in 31 of them to and from English.

## Context

The launch lands in a busy month for voice. TechCrunch notes that Mistral released Voxtral 3 in early October, ElevenLabs launched Realtime for instant voice agents and Salesforce unveiled Launa-Omni, its own voice model.

Nvidia already had open voice models, such as the 1.1-billion-parameter Parakeet. Transcribe is almost thirty times larger and targets a different job: long recordings, with several people and several languages. The company says it beats rivals on benchmarks, including Hugging Faceâs Open ASR Leaderboard, although that claim comes from Nvidia itself and not from an independent evaluation released with the model.

Granary plays a different role. It is the material a team can use to retrain or tune the system so it recognizes the terms of a field, such as drug names or legal vocabulary, in each language. Nvidia pairs it with NeMo Speech, its toolkit for customizing voice models.

For Spanish, the relevant figure is not only the number of languages but accuracy with regional accents, which the public card does not break down by country.

## Whatâs next

- The FP8 and NVFP4 variants cut the memory needed and make tests on modest hardware easier.
- Independent word-error measurements for Spanish as spoken in Chile, Argentina or Mexico are still missing, and they will decide its use in the region.

## Bottom line

Competition in open voice is speeding up: Mistral launched Voxtral 3 with prices from US$ 0.0035 per minute and Block unveiled Dessa, a model trained only on audio. The push to run models on in-house hardware also shows up in the inference engine DeepSeek opened for Nvidia chips and in Flint, a coding model that runs on a laptop.

## Sources

- Hugging Face â NVIDIA-Nemotron-3.5-Transcribe-30B-A4B (model card, October 28, 2026)
- TechCrunch â Voice AI in vogue, Nvidia releases new open transcription model (October 28, 2026)
- Cover image: Justin De La Ornellas from China Town, Hawaii, CC BY 2.0, via Wikimedia Commons

Edited by Rodrigo Cornejo. How we select and verify: who writes these notes.
