---
id: collect-261121-veille-ia/nvidia-hardware/nvidia-releases-all-in-one-adaptable-nemotron-3-5-audio-transcription-and-translation-model-silicona
title: "Nvidia releases all-in-one adaptable Nemotron 3.5 audio transcription and translation model - SiliconANGLE"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: ["2026-10-28"]
keywords: ["model", "nvidia", "transcription", "audio", "transcribe", "siliconangle", "nemotron", "translation"]
source: https://siliconangle.com/2026/10/28/nvidia-releases-all-in-one-adaptable-nemotron-3-5-audio-transcription-and-translation-model/
source_anchor: ""
source_lines: [1, 39]
section: "NVIDIA_Hardware"
---
### Nvidia releases all-in-one adaptable Nemotron 3.5 audio transcription and translation model

Nvidia Corp. today released a leaderboard-winning open artificial intelligence model for multilingual audio transcription and translation, along with an open data pipeline for fine-tuning Nemotron 3.5 Transcribe.

Traditional transcription AI models follow a workflow that detects speech, transcribes it, identifies and labels different speakers, then answers questions about the transcription. Transcribe does all of this in a single model pass and is capable of digesting and answering questions about 100 hours of audio, in 40 languages and can label up to eight speakers.

It’s an open-weight, 30-billion parameter mixture-of-experts model with 4 billion active parameters. That means it can fit on reasonably small hardware, though more powerful hardware lets it transcribe faster.

For its size, Transcribe punches way above its weight and it can run anywhere from the edge to the data center depending on compression. The model runs on a single Nvidia graphics processing unit, from a Jetson AGX Thor, an edge computer robot brain, or DGX Spark, a desktop AI supercomputer, all the way to a Blackwell data-center GPU. Nvidia said a Blackwell Server Edition GPU with NVFP4 precision and one-quarter model compression processes 100 hours of audio in under three minutes.

According to the company, Transcribe hits a high accuracy benchmark for getting words right, with 30.8% error rates out of the box, which can be reduced to 20.8% with careful prompting. Fine-tuned for specialized work, for example, on the Eka Care Indian medical speech benchmark, the model hit 11.8% word error rate, a substantial reduction.

Users can guide transcription analysis or ask questions with prompts. The same checkpoint can turn a recording into different outputs with separate prompts.

For example, a user could instruct the model with, “Transcribe the recording and label each speaker,” or “Translate this audio into English,” or “What decisions were made in this meeting?” or some combination. Users can also provide the model with important jargon or dialogue to help it better understand the conversation before transcribing, improving accuracy.

To build the model, Nvidia extended Granary v1 into Granary v2, the company’s massive open-source public multilingual speech AI dataset. Built with Nvidia NeMo Curator, v2 expands significantly on the previous generation of Granary. The public manifest covers 463,062 hours across 48 languages and 16 source corpora, up from 25 languages in Granary v1. These figures describe the dataset release, not Transcribe’s full training mixture or its sampling weights.

Granary v2 generates three transcripts for each recording in clean verbatim styles that can be requested from Transcribe with a prompt. One provides everything, including “ums” and “ahs.” Another reduces full wording to figures, such as spelling out numbers as symbols. A third eliminates filler words.

All that lets developers adapt transcription from different languages using the pipeline data into readable formats and fine-tune model outputs.

Transcribe is available as a Hugging Face demo, where developers can download a checkpoint and adapt it for domain expertise with fine-tuning.

##### Image: SiliconANGLE/Microsoft Designer

# A message from John Furrier, co-founder of SiliconANGLE:

Support our mission to keep content open and free by engaging with theCUBE community. **Join theCUBE’s Alumni Trust Network**, where technology leaders connect, share intelligence and create opportunities.

- **15M+ viewers of theCUBE videos** , powering conversations across AI, cloud, cybersecurity and more
- **11.4k+ theCUBE alumni** — Connect with more than 11,400 tech and business leaders shaping the future through a unique trusted-based network

### Are you an AWS customer? Support SiliconANGLE financially by buying your AWS services from our Marketplace portal page and links: https://siliconangle.com/aws-marketplace/


##### **About SiliconANGLE Media**

Founded by tech visionaries John Furrier and Dave Vellante, SiliconANGLE Media has built a dynamic ecosystem of industry-leading digital media brands that reach 15+ million elite tech professionals. Our new proprietary theCUBE AI Video Cloud is breaking ground in audience interaction, leveraging theCUBEai.com neural network to help technology companies make data-driven decisions and stay at the forefront of industry conversations.
