---
id: collect-261121-veille-ia/modeles-open/elorian-launches-glimpse-a-vision-model-that-keeps-returning-to-the-image-mid-reasoning
title: "Elorian launches Glimpse, a vision model that keeps returning to the image mid-reasoning"
domain: modeles-open
role: reference
task: article
actors: []
dates: ["2026-10-17"]
keywords: ["elorian", "glimpse", "model", "astra", "image", "visual", "reasoning", "vision"]
source: https://pondero.ai/news/2026-10-17-elorian-glimpse/
source_anchor: ""
source_lines: [1, 24]
section: "Modeles_Open"
---
# Elorian launches Glimpse, a vision model that keeps returning to the image mid-reasoning

Most multimodal models look at a picture once, convert it into tokens, and reason about that description from memory, the way someone might describe a photo they glanced at yesterday. Elorian AI's first model, Glimpse, released October 16, 2026 as a research preview, keeps going back to the actual pixels while it thinks: cropping into a region, pulling a specific frame from a video, or running code to measure what it is looking at.

## What happened

Elorian, a Palo Alto startup founded by former Google Brain and DeepMind researcher Andrew Dai, opened Glimpse through an API and a chat interface at $0.60 per million input tokens and $3 per million output tokens per Elorian's launch post. Elorian calls the mechanism a "visual scratch space": rather than tokenizing an image once, Glimpse can revisit and refine its internal visual representations as it reasons, spawn subagents that trade visual artifacts back and forth, and write code against what it sees. On Elorian's own benchmark table, Glimpse scores 92.90 against GPT-6 Astra's 92.10 on the CharXiv-R chart-reasoning test and 82.66 against Astra's 80.32 on short-form video captioning (UGC-VideoCap), while running at roughly a quarter of Astra's output price per Elorian's launch post. Astra still leads Glimpse on several other benchmarks in the same table, including fine-grained visual reasoning and dense video captioning.

Dai told TechCrunch the company trained Glimpse with a small team on only a few hundred GPUs, built on top of an open-weights base model, and that Elorian disclosed a $55 million seed round at a $300 million valuation when it came out of stealth six months earlier, co-led by Striker Venture Partners, Menlo Ventures, and Altimeter Capital with participation from Nvidia and Google chief scientist Jeff Dean per TechCrunch.

## A cheaper option that only wins on some tasks, not every one

Buyers evaluating Glimpse against frontier vision models get a real tradeoff rather than a clean win. Elorian's own numbers show Glimpse beating GPT-6 Astra specifically on chart reasoning, short-clip captioning, open-vocabulary counting, and a robotics manipulation benchmark, while losing to Astra on fine-grained visual reasoning, scene understanding, and two of three temporal-reasoning tests. Combined with pricing that undercuts Astra by 94 percent on both input and output tokens per Elorian, that makes Glimpse a plausible fit for teams whose workload matches its specific strengths, not a blanket replacement for a general-purpose multimodal model. Robotics firms, scientific labs, and media companies sitting on large volumes of video or image data are the buyers Elorian is targeting first, and those are also the use cases its benchmark picks favor.

The crop-and-revisit approach itself is not new; OpenAI added similar crop-and-zoom reasoning to its o-series models in 2025. Elorian's bet is that building a dedicated model around that mechanism from the start, rather than retrofitting it onto a general-purpose system, lets a much smaller team compete on specific tasks at a fraction of the cost.

## What to watch next

Elorian's benchmark claims are the company's own evaluations, not an independent reproduction, so third-party testing on the CharXiv-R and UGC-VideoCap results is the first thing to watch. Dai has said Elorian's next goal is building what he calls the world's best visual reasoning model within a year; whether Glimpse's narrower, cheaper approach can close the remaining gaps with Astra on scene understanding and temporal reasoning, rather than only winning on the tasks it already leads, will determine whether that goal is realistic.

## Sources

- Introducing Glimpse: Elorian, October 16, 2026 (primary)
- Elorian's first AI model wants to reason with images, not just about them: TechCrunch, October 16, 2026 (secondary)
