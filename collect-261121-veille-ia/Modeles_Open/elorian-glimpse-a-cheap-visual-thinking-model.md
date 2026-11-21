---
id: collect-261121-veille-ia/modeles-open/elorian-glimpse-a-cheap-visual-thinking-model
title: "Elorian Glimpse: a cheap visual thinking model"
domain: modeles-open
role: reference
task: article
actors: []
dates: []
keywords: ["glimpse", "elorian", "visual", "video", "says", "model", "below", "gpt-6"]
source: https://www.neuraldaily.news/en/a/elorian-glimpse-visual-thinking-model
source_anchor: ""
source_lines: [1, 23]
section: "Modeles_Open"
---
## What Glimpse is

Glimpse works with visual representations during reasoning: it zooms into regions, rewatches short video intervals at a denser frame rate and tracks features over time. As an agent it writes code, transforms images, watches videos and can spawn subagents that exchange text, visual artefacts and internal representations (Elorian).

Elorian says a small team built it with a few hundred GPUs by post-training an open-weights base model. Glimpse is a preview, available through the company's API and chat UI, as well as on Together AI (Together AI).

## Pricing and results

Glimpse costs $0.60 per million input tokens and $3.00 per million output tokens. Elorian says that is 94% below GPT-6 Astra, 85% below Claude Opus 5.5, 70% below GPT-6.1 Sol and 20% below Gemini 3.8 Flash's introductory rates.

In Elorian's own results, Glimpse beats GPT-6 Astra on four evaluations, ties on one and trails on several others:

- Ahead: WGO Bench (manipulation events) 42.70 vs 39.42; CharXiv-R (scientific charts) 92.90 vs 92.10, though Opus 5.5 leads at 93.40; UGC-VideoCap (short-form captioning) 82.66 vs 80.32; FSC-147 (counting) 83.36 vs 82.02.
- Tied: ERQA (embodied spatial reasoning) 83.75.
- Behind: Video-MME v2 56.68 vs 64.06; TOMATO 93.60 vs 96.02; HSS 46.36 vs 53.6; BabyVision 95.36 vs 97.68; DREAM-1K 76.04 vs 79.05; CountQA 82.66 vs 86.42.

These numbers are vendor-reported and not yet independently verified. Elorian also notes that looking closely costs time and tokens, and says token efficiency is its next focus.

## What it's for

Elorian's examples include labelling and segmenting robot teleoperation data, dense video captioning and counting objects in video. In a LIBERO task, Glimpse and GPT-6 Astra each acted as planner for the same π0.5 policy from Physical Intelligence. Both completed the task; Glimpse's planning cost $0.035 and Astra's $0.249.

Glimpse doesn't control robots directly yet: Elorian says calibrated reconstruction and robot control are work ahead. Teams running high-volume image or video analysis, such as quality inspection or data labelling, may want to test it on their own data given the price.
