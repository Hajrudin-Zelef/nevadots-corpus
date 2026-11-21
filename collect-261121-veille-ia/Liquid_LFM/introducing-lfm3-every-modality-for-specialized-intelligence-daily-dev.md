---
id: collect-261121-veille-ia/liquid-lfm/introducing-lfm3-every-modality-for-specialized-intelligence-daily-dev
title: "Introducing LFM3: Every Modality for Specialized Intelligence | daily.dev"
domain: liquid-lfm
role: reference
task: article
actors: ["Liquid AI"]
dates: []
keywords: ["models", "model", "daily", "lfm3", "d1-omni-2b", "questions", "liquid", "vision"]
source: https://daily.dev/posts/introducing-lfm3-every-modality-for-specialized-intelligence-y3er0v1ft
source_anchor: ""
source_lines: [1, 26]
section: "Liquid_LFM"
---
Liquid AI released LFM3, a new family of open-weight 2B-class models spanning text, vision, audio, omni and decision variants on one shared hybrid backbone. The models are pre-trained on 22T tokens and use a tri-hybrid layer mix of short convolutions, sliding-window attention and global attention. LFM3-1.7B supports a 256K-token context window and decodes 1.8 to 2.8 times faster than competing models on-device. The vision model LFM3-VL-2B beats same-size rivals by at least 7.4 points across 62 benchmarks and nearly matches a 12B Gemma model. New decision models, d1-2B and d1-Omni-2B, answer multiple questions about an input in a single forward pass with calibrated probabilities. The base and post-trained models are available on Hugging Face, the Playground and the Liquid AI API for local fine-tuning and deployment.

## Table of contents

HighlightsNew architecture for more expressivityA multimodal family for every needMade for specialized intelligenceGet StartedLaunch PartnersCitation
## Community take

44

comments

The release is met with strong interest, especially for local/on-device agentic and classification use cases, with the team actively answering technical questions; a few commenters report tooling friction (MLX loading errors, quantization quality issues) and question whether tiny models are suited for coding tasks.

Questions this post answers

## What is Liquid AI's LFM3-1.7B model and how does its context window compare to LFM2?

LFM3-1.7B is a 2B-class text model for local agents with a 256K-token context window, up from the smaller context in LFM2. It was pre-trained on 22T tokens using a tri-hybrid architecture of gated short convolutions, sliding-window attention and global attention, and it decodes 1.8 to 2.8 times faster than comparable models on edge devices. Developers picking a compact agentic model can track LFM3's benchmarks and context-window specs on daily.dev.

## How does the LFM3-VL-2B vision model perform compared to larger vision-language models like Gemma?

LFM3-VL-2B scores at least 7.4 points ahead of all similarly sized models across 62 vision benchmarks and matches Gemma 4 12B, a model over six times larger. It also surpasses Gemini 3.5 Flash Lite and sits within 2 points of GPT-5.6 Luna, while running on-device with the lowest memory footprint in its class. Teams choosing a small vision-language model for edge deployment can compare these results on daily.dev.

## What are Liquid AI's d1-2B and d1-Omni-2B decision models used for?

d1-2B and d1-Omni-2B are 2B-class models built on LFM3-VL-2B and LFM3-Omni-2B that answer multiple yes/no, choice, score, pointing, reading, estimating, or picking questions about an input in a single forward pass with calibrated probabilities. d1-Omni-2B also accepts spoken input and keeps nearly all its accuracy versus typed questions, scoring 83.4 on DecisionBench by voice versus 83.6 by text. Developers building structured-decision pipelines can follow d1-2B and d1-Omni-2B updates on daily.dev.
