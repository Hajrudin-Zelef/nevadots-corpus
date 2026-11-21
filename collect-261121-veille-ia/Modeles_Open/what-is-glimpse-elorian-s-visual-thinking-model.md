---
id: collect-261121-veille-ia/modeles-open/what-is-glimpse-elorian-s-visual-thinking-model
title: "What Is Glimpse? Elorian's Visual Thinking Model"
domain: modeles-open
role: reference
task: article
actors: []
dates: ["2026-10-16"]
keywords: ["glimpse", "elorian", "model", "reasoning", "visual", "video", "image", "thinking"]
source: https://www.eigent.ai/blog/elorian-glimpse-visual-thinking-model
source_anchor: ""
source_lines: [1, 67]
section: "Modeles_Open"
---
Elorian launched **Glimpse** on October 16, 2026 — its first model, and a different kind of multimodal system. Instead of tokenizing an image once and reasoning from that single snapshot, Glimpse is a "visual thinking" model that can go back to the image or video mid-thought, crop into a region, or replay a short clip before it answers. This guide covers what Glimpse is, how it benchmarks against frontier models, what it costs, and where it fits.

Glimpse is a multimodal visual reasoning model from Elorian, built for understanding and reasoning about the physical world from what it sees. Rather than converting an image to text first, it reasons directly over images and video — so it can find small details in dense scenes, track events across frames, and keep its answers grounded in what is actually on screen (Elorian, OpenRouter).

The core idea comes from co-founder and CEO Andrew Dai: when a model is unsure what an image shows, it should look again instead of reasoning from a lossy first impression. Dai spent 12 years at Google, where he led the Gemini data area, before founding Elorian (Dealroom).

Elorian is shipping Glimpse as a research preview with API access and a chat interface. On OpenRouter it has a 262K-token context window, accepts text, images, and video, returns text, and supports tool calling and structured outputs.

Most vision-language models read an image into tokens once and never revisit the pixels. Glimpse is built around repeated inspection — what TechCrunch described as a "visual scratch space" (reported via WOWTALE).

In practice, as it reasons Glimpse can:

- **Zoom into a region** to check a detail it is unsure about.
- **Return to a specific moment** in a video and re-watch it.
- **Replay a short interval at a denser frame rate** — for example, moving from roughly 8 fps to 14 fps over the one ambiguous second that decides whether a shot is a pan or a cut.
- **Track one feature across time** to judge motion, like following a window edge to tell whether a train is decelerating.

According to Elorian, Glimpse is also trained end-to-end as an agent: it can write code, transform images, watch videos, and spawn subagents that exchange text, visual artifacts, and internal representations to solve a shared problem.

Elorian publishes a benchmark table comparing Glimpse (at max thinking effort) against GPT-6 Astra, GPT-6.1 Sol, Claude Opus 5.5, and Gemini 3.8 Flash. These are **company-reported** results, so treat them as Elorian's own evaluation rather than independently verified numbers.

By Elorian's account, Glimpse exceeds Astra on four evaluations — scientific chart reasoning, open-vocabulary counting, short-form video captioning, and manipulation-event understanding — and matches it on embodied reasoning. Selected figures from Elorian's table:

| Capability | Evaluation | Glimpse | GPT-6 Astra | Opus 5.5 | Gemini 3.8 Flash | 
|---|---|---|---|---|---|
| Robotics | WGO Bench (manipulation events) | 42.70 | 39.42 | 12.34 | 33.99 | 
| Robotics | ERQA (embodied spatial) | 83.75 | 83.75 | 82.20 | 81.20 | 
| Scientific | CharXiv-R (chart reasoning) | 92.90 | 92.10 | 93.40 | 91.80 | 
| Counting | FSC-147 (open-vocabulary) | 83.36 | 82.02 | 73.78 | 67.06 | 
| Video captioning | UGC-VideoCap (short-form) | 82.66 | 80.32 | 76.35 | 78.33 | 

The table also shows where Glimpse trails. On Elorian's own numbers it loses to Astra on BabyVision fine-grained perception, HSS scene understanding, DREAM-1K dense captioning, TOMATO temporal reasoning, Video-MME v2, and CountQA product counting. Elorian has been upfront that Glimpse is a substantially smaller model than the systems it benchmarks against, which makes the mix of wins and losses worth reading closely.

One widely shared demo claim: Glimpse solved 5 of 5 mazes of increasing difficulty while Gemini 3.8 Flash solved 1 of 5 (reported by TestingCatalog). That is a demo result, not a standardized benchmark.

Glimpse is priced at **$0.60 per million input tokens and $3.00 per million output tokens** (OpenRouter). Elorian says that is:

- 94% lower than GPT-6 Astra
- 85% lower than Claude Opus 5.5
- 70% lower than GPT-6.1 Sol
- 20% lower than Gemini 3.8 Flash's introductory rate

The practical gap can be larger per task because of how Glimpse works. In Elorian's robot-planning example, Glimpse planned a vision-language-action task for $0.035 while GPT-6 Astra cost $0.249 for the same episode. Elorian notes that dense re-inspection uses more tokens, and says token efficiency is its next priority.

Elorian says a small team trained Glimpse on only a few hundred GPUs by post-training an open-weights base model — far below the compute budgets frontier labs spend on flagship multimodal systems (Startup Fortune).

The company raised a $55M seed round at a $300M valuation before shipping a product, with Nvidia and Google chief scientist Jeff Dean among the participants (Dealroom).

Elorian targets physical-world and media use cases: robotics (synthetic data labeling and high-level planning), construction and manufacturing footage review, dense video captioning for media archives, and precise counting on job sites (OpenRouter). The pitch is that for visual workloads at scale, frontier general models are often either not accurate enough or too expensive — and a cheaper specialist that keeps looking can fill that gap.

The open question, raised even by supportive observers, is whether specialized models for specific capabilities are the right long-term shape, or whether general frontier models absorb these skills. For now, Glimpse is a concrete bet that visual reasoning deserves a model built around it.

Glimpse is a reminder that the useful unit isn't one giant model — it's the right model wired into a real workflow. Eigent is an open-source, local **Cowork** desktop app: a multi-agent AI workforce you can point at your own tools and data, mixing models for the tasks they're best at. If your work involves reviewing footage, documents, or dashboards, see how a multi-agent workflow handles it in one-person-company AI agents, then download Eigent to build your own.

## Recent Posts

### Eigent Release Notes v1.0.8: A New Home, Global Search & Clearer Conversations

Eigent v1.0.8 introduces a redesigned Home, search across Spaces, a new composer, and a default conversation timeline that makes agent work easier to follow.

### GPT-6.2 Astra and Sol: What's New, Pricing, and Benchmarks

GPT-6.2 Astra and Sol are OpenAI's newest models for ChatGPT Work, Codex, and the API. Here's what changed, the pricing, the benchmarks, and which to use.

### What Is NVIDIA Nemotron for Science? Open Models for Research Agents

NVIDIA Nemotron for Science explained: the open model, LitmusBench v0.2 chemistry dataset, and post-training recipes for building AI agents for scientific research.
