---
id: collect-261121-veille-ia/gemini-google/granary-v2-scalable-multilingual-speech-data-curation-for-predictable-and-controllable-model-outputs
title: "Granary-v2: Scalable Multilingual Speech Data Curation for Predictable and Controllable Model Outputs"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["granary-v2", "data", "speech", "curation", "training", "multilingual", "model", "models"]
source: https://research.nvidia.com/labs/nemotron/Granary-v2/
source_anchor: ""
source_lines: [1, 7]
section: "Gemini_Google"
---
#### Paper Â Â Dataset ð¤

#### Nithin Rao Koluguri, Meline Mkrtchyan, Nune Tadevosyan, Aaftab V, Sushmitha Deva, Fan Qian, Davit Karamyan, Kunal Dhawan, Taejin Park, Piotr Å»elasko, Arushi Dhingra, Viraj Karandikar, Shuoyang Ding, Slyne Deng, Onur Yilmaz, Praateek Mahajan, Utkarsh Vaidya, Nikolay Karpov, Jagadeesh Balam, Boris Ginsburg

## Overview

Speech-language models that follow instructions need training data that connects the same recording to different output forms and tasks. We present Granary-v2, a multilingual speech dataset and reusable curation system that provides a data foundation for these models. Granary-v2 extends Granary-v1 with aligned transcript variants and instructionâtarget pairs for multi-task speech-language training. Language coverage increases from 25 to 48 languages. Built with NeMo Curator, the system combines language-dependent ASR labeling, transcript improvement, and text label generation. It produces aligned targets for speech-faithful transcription, written forms with and without disfluencies, and code-switched transcription. Additional targets include transcript-derived captions and questionâanswer pairs, with context and translations for selected records. The full processed collection contains approximately 843,600 unique audio hours, with translations covering about 142,800 hours into English and about 55k hours of English audio into 31 target languages. We evaluate label-quality filtering with two English Transformer-TDT models under the same training-step budget. Filtering reduces the unweighted mean word error rate across nine benchmarks from 4.91% to 4.67%, with improvements on six benchmarks. We also report curation throughput and downstream ASR and speech translation results from Nemotron-3.5-Transcribe, which uses Granary-v2 as part of its training data. We release the Granary-v2 pipeline, built with NeMo Curator, and model training recipes as open source. We release a public subset of the generated labels and instructionâtarget pairs through Hugging Face.
