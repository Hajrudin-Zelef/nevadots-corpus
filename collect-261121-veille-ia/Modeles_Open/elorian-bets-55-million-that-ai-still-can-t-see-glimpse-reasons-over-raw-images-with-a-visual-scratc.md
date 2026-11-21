---
id: collect-261121-veille-ia/modeles-open/elorian-bets-55-million-that-ai-still-can-t-see-glimpse-reasons-over-raw-images-with-a-visual-scratc
title: "Elorian Bets $55 Million That AI Still Can't See: Glimpse Reasons Over Raw Images With a Visual Scratch Space"
domain: modeles-open
role: reference
task: article
actors: []
dates: []
keywords: ["elorian", "visual", "model", "glimpse", "million", "reasoning", "models", "company"]
source: https://www.aireport.net/Gn_0561738b0dc3
source_anchor: ""
source_lines: [1, 20]
section: "Modeles_Open"
---
A Palo Alto startup called Elorian has launched its first AI model with an unusually blunt diagnosis of the field: today's multimodal systems, for all their skill with language and code, still cannot really see. The company released Glimpse on Friday as a research preview with API access, while its chat interface remains waitlist-only. TechCrunch reported the launch, and Chinese tech media picked it up within hours — notable coverage for a company that raised a $55 million seed at a $300 million valuation before shipping a single product.

The thesis comes from CEO Andrew Dai, a former Google DeepMind researcher. Last year he photographed a board game he had just finished with friends and asked a frontier model who was winning. It could not answer, even though the board's symbols were obvious to any adult at the table. "Board games each have their own visual language, each have their own iconography," Dai told TechCrunch. "Humans can look at IKEA diagrams, with no text, and assemble IKEA furniture, right? Why can't AI models?"

His answer is architectural. Most multimodal models look at an image once, convert it into tokens, and then reason about that text representation — describing a photo from memory, in effect. Glimpse instead returns to the raw source throughout the reasoning process: it can crop into a specific region of an image, pull particular frames out of a video, or run code to measure and compare what it is looking at. Elorian calls the mechanism a "visual scratch space," and compares it to a person pinching to zoom on a phone photo to double-check what they thought they saw.

The approach is not entirely without precedent — OpenAI added crop-and-zoom during reasoning to its o-series models in 2025 — but Elorian's pitch is that it built the entire model around re-inspection rather than bolting it on, and did so cheaply. The company says a small team trained Glimpse on a few hundred GPUs, and that the model is substantially smaller than the frontier systems it benchmarks against. It has not disclosed the parameter count, context window or training cutoff.

Pricing sits between tiers: $0.60 per million input tokens and $3 per million output tokens, cheaper than mid-level models like Anthropic's Sonnet and OpenAI's Sol, pricier than lightweight models like Haiku and Luna. As for capability, Elorian's own evaluations claim Glimpse outperforms GPT-6 Astra on scientific chart reasoning and short-form video captioning, with competitive results on other visual reasoning tasks. Those benchmarks are self-reported — the company published no independent evaluations, and the comparisons were run against models Elorian chose itself.

The backers suggest where this is heading. The seed round was co-led by Striker Venture Partners, Menlo Ventures and Altimeter Capital, with participation from NVIDIA and Google chief scientist Jeff Dean. Elorian came out of stealth six months ago, and its first target customers are companies sitting on mountains of video: robotics firms, scientific labs, and media and creative companies.

Dai's longer-term plan goes beyond perception. The company wants to move from seeing to doing — building a model of a scene and using it to choose actions, including directly controlling robots — and aims to build what it calls "the world's best visual reasoning model" next year, one that tops every visual reasoning test. That is a tall order for a model whose benchmark evidence is so far entirely self-generated. But if re-inspection really is the missing ingredient in machine vision, the industry's habit of converting images into tokens and reasoning from memory may start to look like the expensive shortcut it always was.

## Comments (0)

Log in to join the discussion

Log In
No comments yet
