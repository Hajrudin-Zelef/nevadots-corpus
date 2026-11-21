---
id: collect-261121-veille-ia/copilot-microsoft/microsoft-ships-decision-1-1-for-fast-calibrated-choices-in-software
title: "Microsoft ships Decision-1.1 for fast, calibrated choices in software"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-11-17"]
keywords: ["microsoft", "model", "decision-1", "calibrated", "choices", "ships", "fast", "software"]
source: https://nownessai.com/article/microsoft-decision-1-1-now-generally-available
source_anchor: ""
source_lines: [1, 16]
section: "Copilot_Microsoft"
---
NownessAI · Agent · Agents & tool use · 2026-11-17

# Microsoft ships Decision-1.1 for fast, calibrated choices in software

A specialized decision model now generally available in Foundry, OpenRouter and Vercel routes, classifies and prioritizes at a fraction of LLM cost and latency, with confidence scores that let apps defer when uncertain.

Microsoft announced the general availability of Microsoft-Decision-1.1 on November 17, 2026. The model is accessible in Microsoft Foundry, OpenRouter and Vercel, and has already been used by more than 4,000 customers during a five-week public preview. Early adopters include Epic, Equinor, Atos Group and Raiffeisen Bank. The release adds multimodal support so the model can accept photos and screenshots as part of a request, and it is priced at $0.042 per million input tokens in the Global Standard zone with output tokens free.

Microsoft-Decision-1.1 is a mid-sized mixture-of-experts model based on Gemma 4 26B A4B, trained from first principles for structured decision-making rather than text generation. The team combined supervised fine-tuning and reinforcement learning on hundreds of synthetic and licensed datasets covering tens of thousands of task types across industries, teaching the model to discriminate between close choices, recognize uncertainty and abstain when information is incomplete. A custom inference engine optimized for decision workloads routes requests across a flexible GPU fleet (A100, H100/200, GB200/300), delivering more than twice the speed of the first Decision model at the same cost. The model outputs calibrated confidence scores, splits confidence when multiple answers are valid, and lets developers set their own thresholds for acting, flagging or escalating.

- **68** Artificial Analysis Decision Index v2.1 score
- **6** point improvement over Decision-1
- **2.2x** speedup vs LLM for Copilot routing
- **4.4x** cost reduction vs LLM for Copilot routing

Source: commandline.microsoft.com
