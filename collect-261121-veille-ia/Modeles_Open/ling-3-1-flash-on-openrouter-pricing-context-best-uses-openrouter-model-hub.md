---
id: collect-261121-veille-ia/modeles-open/ling-3-1-flash-on-openrouter-pricing-context-best-uses-openrouter-model-hub
title: "Ling 3.1 Flash on OpenRouter: Pricing, Context & Best Uses | OpenRouter Model Hub"
domain: modeles-open
role: reference
task: article
actors: []
dates: ["2026-11-13"]
keywords: ["openrouter", "output", "window", "free", "ling", "flash", "pricing", "context"]
source: https://www.hunteralphahub.com/openrouter-models/ling-3.1-flash
source_anchor: ""
source_lines: [1, 55]
section: "Modeles_Open"
---
These examples use the 2026-11-13 input and output prices. Cache discounts, provider surcharges and
tool fees are excluded.

Workload

Input

Output

Estimated cost

Quick test

20,000

5,000

$0.002

Product chat month

500,000

125,000

$0.039

Document batch

5,000,000

1,000,000

$0.360

Strengths

â 1,000,000-token window at $0.045 / $0.135 per million after the 2026-11-09 cut â cheap for the window, though no longer free

â 25B active parameters out of 560B in a hybrid reasoning mixture-of-experts

â A text row with a window big enough for real documents rather than a demo

Limitations

! Text only â no images, files or audio

! It was free when we added it on 2026-10-10, cost $0.075 / $0.225 on 2026-10-14 and then fell to $0.045 / $0.135 on 2026-11-09: the free listing lasted days, not months

! 32,768-token output ceiling, the smallest here after Nano Banana

OpenRouter API quickstart

Run these examples from your server or edge function. Keep the OpenRouter API key private and do not
expose it in browser code.
