---
id: collect-261121-veille-ia/modeles-open/elorian-s-glimpse-thinks-with-its-eyes-the-model-that-zooms-back-in-mid-thought
title: "Elorian's Glimpse Thinks With Its Eyes: The Model That Zooms Back In Mid-Thought"
domain: modeles-open
role: reference
task: article
actors: []
dates: []
keywords: ["model", "elorian", "glimpse", "million", "back", "image", "chart", "frames"]
source: https://dev.to/paullovelace/elorians-glimpse-thinks-with-its-eyes-the-model-that-zooms-back-in-mid-thought-3f7m
source_anchor: ""
source_lines: [1, 31]
section: "Modeles_Open"
---
Elorian's Glimpse is the first AI model I'd describe as having a working memory for images — and at $0.60 per million input tokens, it's priced like the company actually wants you to test that claim.

On October 16, 2026, TechCrunch broke the story: Elorian, a brand-new Palo Alto lab founded by Andrew Dai — the ex-CEO of Google DeepMind — has released its first model as a research preview, with an API and a chat interface. The funding tells you the smart money is paying attention: $55 million seed at a $300 million valuation, backed by Striker, Menlo Ventures, Altimeter, Nvidia, and Jeff Dean. That's a heavyweight cap table for a lab that didn't exist in public conversation a week ago.

But the money isn't the story. The architecture is.

Every vision model you've used works the same way: it glances at your image once, flattens it into tokens, and then does all of its thinking in text. Ask it about a dense scientific chart and it's reasoning from a compressed memory of the image — squinting at its own snapshot. Glimpse does something different. Mid-reasoning, it goes back and looks again. It re-crops regions of the image, pulls frames from video, or even runs code to measure what it sees. Elorian calls it a "visual scratch space."

Make it concrete. Hand a standard multimodal model a chart with twelve overlaid series and a legend printed in 6-point type. It takes one look and starts guessing. Glimpse, per the announced design, crops the legend and reads it properly, crops the axis labels, measures the pixel distances between data points, and runs code on those measurements to compute its answer. It's not a better guess. It's a different procedure — look, measure, compute, look again.

Same idea for short-form video captioning, the other task Elorian claims leadership on. Instead of sampling a fixed grid of frames upfront — the industry default — the model pulls the frames it needs as its questions emerge. The eyes stay open during the thinking.

It's the difference between describing a room from memory and walking back in to check the bookshelf.

There's a pattern here worth naming. Tool use made language models better at math: they stopped hallucinating arithmetic and started running code instead. Glimpse applies the same lesson to perception — stop hallucinating the chart, go measure it. We've spent two years watching models get better at reasoning over a frozen snapshot. The next two years will be about models that refuse the snapshot.

The training story is nearly as interesting as the architecture. Per the TechCrunch report, a small team trained Glimpse on a few hundred GPUs. Frontier systems cost orders of magnitude more. If the claimed results survive independent testing, that's a real data point for the unfashionable view that architecture still beats brute force. Emphasis on *if*. But $300M valuations don't get assembled on vapor alone — Striker and Nvidia both got to look at the model before writing checks.

The pricing is aggressive on purpose: $0.60 per million input tokens, $3 per million output. That lands between the Sonnet/Sol-class heavyweights and the Haiku/Luna-class budget tier — cheap enough that you can run real experiments instead of toy demos. One thing the announcement doesn't spell out: how image and video frames are tokenized and billed. For a visual model, that detail is the real price tag. Don't wire it into anything until you see it.

Now the brakes — because there should always be brakes on launch day.

Every performance claim here is Elorian's own. "Outperforms GPT-6 Astra" covers two specific tasks — scientific chart reasoning and short-form video captioning — and it comes from the lab's own announcements, not independent evals. No third-party benchmarks exist yet. This is a research preview, which in practice means breaking API changes, rate limits, and rough edges. There's no mention of downloadable weights, so plan on API-only. Latency is an open question: a model that re-crops images and runs code mid-reasoning is doing more work per answer, and more work costs time. And chart-reasoning benchmarks are famously cherry-pickable — a test suite built around models that crop their own images will flatter the model built to crop its own images, unless the evaluation is carefully blinded.

Still, the idea deserves your attention even if this specific model doesn't earn it. The visual scratch space is a technique, and techniques get copied. Three things I'd actually try first: screenshot debugging — hand it your broken UI and ask it to find the misalignment; chart extraction from PDFs — the "read this graph into a table" job that current models botch weekly; and video QA, letting it pull its own frames from a screen recording instead of pre-sampling them yourself. If that last one works, it changes how you build video agents.

Bottom line: Glimpse may not be the model you end up using. But keeping the eyes open during the thinking is almost certainly how every vision model will work within two years. Try the API. Keep your receipts.

*Source: TechCrunch, October 16, 2026*

## Top comments (0)
