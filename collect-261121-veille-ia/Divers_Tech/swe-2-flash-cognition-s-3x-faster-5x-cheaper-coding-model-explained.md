---
id: collect-261121-veille-ia/divers-tech/swe-2-flash-cognition-s-3x-faster-5x-cheaper-coding-model-explained
title: "SWE-2 Flash: Cognition's 3x-Faster, 5x-Cheaper Coding Model, Explained"
domain: divers-tech
role: reference
task: article
actors: []
dates: []
keywords: ["swe-2", "cognition", "flash", "model", "coding", "devin", "work", "tokens"]
source: http://dev.to/neuraitool/swe-2-flash-cognitions-3x-faster-5x-cheaper-coding-model-explained-2ka9
source_anchor: ""
source_lines: [1, 28]
section: "Divers_Tech"
---
Cognition, the company behind the Devin coding agent, just shipped **SWE-2 Flash** — a smaller sibling of its flagship SWE-2 model that runs at roughly **3x the speed for about one-fifth of the price**. Announced November 4, 2026, it is available now in Devin Desktop and the Devin CLI, and it is **free to use through November 30, 2026**.

The launch is interesting for a specific reason: it is one of the most honest model launches of the year. Cognition is not claiming frontier dominance. It is claiming that most daily coding work does not need the frontier — and pricing accordingly.

SWE-2 Flash is a 276-billion-parameter mixture-of-experts model with 12 billion parameters active per token, adapted from Thinking Machines Lab's Inkling-Small base model (weights released July 30, 2026). That MoE structure is the whole trick: sparse activation keeps inference fast and cheap while the total parameter count stays large enough to handle real coding work.

The intended workloads, per Cognition: automations, code review, feature development, and documentation updates. The high-volume, low-drama work that eats most engineering budgets.

Cognition's reported evaluations show Flash delivering big gains over its Inkling-Small base. Against SWE-2 itself, the picture is mixed:

- 
**FrontierCode 2.0:** Flash edges out SWE-2 — notable for a model at a fifth of the cost.
- 
**Terminal-Bench 4.0:** SWE-2 scores higher. Long, complex terminal tasks still belong to the flagship.
- 
**DeepSWE 1.1:** SWE-2 scores higher here too.

One honest caveat to carry with you: Cognition's own speed illustration is a **simulation** — each model's Terminal-Bench run replayed at 200 vs. 60 tokens per second. It is marketing visualization, not a measured benchmark. Treat all vendor-reported numbers as best-case until independent evaluations land.

Cognition hasn't published a separate per-token table for Flash — pricing is positioned strictly relative to SWE-2. SWE-2's Devin list price is $3.00 per million input tokens and $15.00 per million output tokens, so one-fifth works out to roughly **$0.60 input / $3.00 output per million tokens**.

The catches: Flash runs **only inside Devin** (Desktop and CLI) — no standalone API, no public weights. Cognition says it is building a secure sharing program for trusted partners and research labs instead. If you want to self-host, look at open-weight models instead.

The thesis here is the same efficiency story reshaping the whole industry: a mid-size MoE can handle the daily grind. If your agent spend is dominated by routine work, the free window through November 30 is a genuinely free way to test whether the cheap-fast model covers your workflow. Just measure before the December billing reality sets in.

*This is a condensed developer explainer. The full breakdown — what changed versus SWE-2, the complete benchmark picture, who should and shouldn't use it, and the try-it-free steps — is on NeurAITool: SWE-2 Flash: Cognition's Cheap, Fast AI Coder (2026).*

## Top comments (0)
