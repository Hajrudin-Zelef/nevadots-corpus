---
id: collect-261121-veille-ia/liquid-lfm/liquid-ai-releases-open-weight-lfm3-family-of-2b-class-models
title: "Liquid AI releases open-weight LFM3 family of 2B-class models"
domain: liquid-lfm
role: reference
task: article
actors: ["Liquid AI"]
dates: []
keywords: ["text", "liquid", "models", "model", "audio", "lfm3", "post", "family"]
source: https://dev.to/mikefluff/liquid-ai-releases-open-weight-lfm3-family-of-2b-class-models-12c1
source_anchor: ""
source_lines: [1, 30]
section: "Liquid_LFM"
---
Liquid AI has released LFM3, a new generation of its foundation models, in a post on Hugging Face. The weights are open and available now. The company describes the family as a starting point for specialized models rather than one large general-purpose model.

Six models share one backbone, LFM3-1.7B-Base, and all sit in the 2B-parameter range:

- 
**LFM3-1.7B** takes text in and gives text out. It is aimed at local agents, tool calling and structured outputs.
- 
**LFM3-VL-2B** takes text, images and video. It is the first LFM vision model to accept video.
- 
**LFM3-Audio-2B** takes text and audio and answers in text, speech or both, across 18 languages.
- 
**LFM3-Omni-2B** takes text, images, video and audio and answers in text or speech. Liquid AI says it can replace a separate speech-recognition model, vision-language model, LLM and speech synthesizer.
- 
**d1-2B and d1-omni-2B** return calibrated answers rather than free text. d1-2B handles text, images and video, and d1-omni-2B adds audio.

All figures below are Liquid AI's own. The company says LFM3-1.7B beats its earlier LFM2.5-2.6B with 35% fewer parameters. On WebArena-Verified it reports 38.42 against 3.02. The text model fits in about 1 GB at 4-bit precision and has a 256K-token context window.

LFM3-VL-2B raises the average across 62 vision benchmarks from 57.6 to 71.4 compared with LFM2.5-VL-3B. Liquid AI says that matches Gemma 4 12B. For audio, it reports a speech-quality score of 3.83 against 3.22 for the previous audio model.

The d1 models are the most directly relevant to business processes. You give them a situation, such as a support ticket, an invoice, an app screen, a photo or a voice note, plus a list of questions. They answer every question in one forward pass, each with a calibrated probability.

Question types include yes/no, single or multiple choice, scores, pointing at objects, reading values from documents, and estimating numbers with a range. The sample in the post routes a ticket to a team, flags a refund request, lists topics and rates urgency. d1-2B replaces d1-3B with a third fewer parameters. Liquid AI reports 75.5 against 64.0 on ImajevBench.

Every model can be fine-tuned on a single GPU. Liquid AI published Colab notebooks for tasks that include extracting structured JSON from messy documents, answering from retrieved documents or declining, calling tools in a 108-tool phone app, and reading receipt photos into JSON.

The post also shows LFM3-VL-2B adapted into three robotics models. The manipulation model reaches 96.2% average success across the four LIBERO suites. That is a demonstration of adaptability rather than a business workflow.

The post is the vendor's own announcement. The comparisons have not been independently checked here, and the source gives no pricing or licensing terms beyond calling the models open-weight.

## Top comments (0)
