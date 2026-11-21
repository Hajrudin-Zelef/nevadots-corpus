---
id: collect-261121-veille-ia/xiaomi-mimo/xiaomi-mimo-v2-6-pro-ties-grok-4-7-atop-open-weights-index-ai-weekly
title: "Xiaomi MiMo-V2.6-Pro ties Grok 4.7 atop open-weights index | AI Weekly"
domain: xiaomi-mimo
role: reference
task: article
actors: ["Xiaomi"]
dates: []
keywords: ["xiaomi", "mimo-v2", "index", "grok", "open-weights", "intelligence", "roughly", "under"]
source: https://aiweekly.co/alerts/xiaomi-mimo-v26-pro-ties-grok-47-atop-open-weights-index
source_anchor: ""
source_lines: [1, 29]
section: "Xiaomi_MiMo"
---
## TL;DR

- Xiaomi's MiMo-V2.6-Pro scored 46 on Artificial Analysis' Intelligence Index, tying Grok 4.7 and taking the top open-weights slot.
- The 1.02-trillion-parameter Pro completed 30 RL steps across roughly 750,000 trajectories in under six days for about $2.62 million.
- Xiaomi released weights under MIT license alongside a 9B Qwen3.5 distill, more than 7,000 RL environments, and its training code.

Xiaomi's MiMo-V2.6-Pro, a 1.02-trillion-parameter mixture-of-experts model with roughly 42 billion parameters active per token, debuted at the top of Artificial Analysis' Intelligence Index among open-weights systems with a score of 46, tying Grok 4.7 and surpassing DeepSeek V4.1 Pro at 36. Xiaomi released weights, code, and a technical report under an MIT license on September 21, 2026.

The reported bill for getting there sits in the low millions. According to Xiaomi's write-up as summarized by VentureBeat, the Pro model completed "30 large RL steps covering roughly 750,000 trajectories in under six days" at approximately $2.62 million. A smaller Flash variant with 309 billion total parameters and about 15 billion active per token ran through the same pipeline for around $850,000.

Xiaomi "assigned several dozen people" to the reinforcement learning effort, said Fuli Luo, the former DeepSeek researcher who now heads Xiaomi's MiMo team, VentureBeat reported. Rather than train each capability in isolation, the team ran a single unified pass across coding, general agents, visual, and cybersecurity tasks, an approach Xiaomi terms "You Only RL Once."

The benchmark card is mixed against closed frontier competitors. Pro hit 71.9 on DeepSWE v1.1 versus Claude Opus 5's 74.0 and GPT-5.6 Sol's 73.0, 89.9 on Terminal Bench 2.1, and 94.0 on CyberGym, but only 47.9 on ExploitBench against Sol's 78.5. On the composite Intelligence Index, Pro's 46 still trails Claude Fable 5.1 and GPT-6 Astra at 53 each, Claude Opus 5 at 51, and Meta's Muse Spark 1.3 at 48, per OfficeChai's leaderboard summary.

Pricing is where the pressure lands. Xiaomi lists Pro at $0.435 per million input tokens and $0.87 per million output, and Flash at $0.14 and $0.28. OfficeChai reports a cost per Intelligence Index task of $0.13, roughly "one-twentieth to one-sixtieth" of leading international models.

Alongside the two frontier variants, Xiaomi released a 9-billion-parameter distilled model built on Qwen3.5, plus more than 7,000 RL environments and its training code.

## Shared on Bluesky by 1 AI expert

- 
        
                  huggingface.co/XiaomiMiMo/M... MiMo-V2.6 is out! and, more importantly to me, the tech report. this is the most batteries-included tech report for a modern frontier model ever. really cool stuff. View on Bluesky →

          Originally reported by
          **huggingface.co**
        

Original headline: MiMo_V2_6_technical_report.pdf · XiaomiMiMo/MiMo-V2.6-Pro-RL at main
