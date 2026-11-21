---
id: collect-261121-veille-ia/claude-anthropic/claude-opus-5-5-takes-the-top-spot-on-artificial-analysis-at-58
title: "Claude Opus 5.5 Takes the Top Spot on Artificial Analysis at 58"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-09-22"]
keywords: ["opus", "artificial", "analysis", "claude", "index", "tokens", "model", "points"]
source: https://rits.shanghai.nyu.edu/ai/claude-opus-5-5-takes-the-top-spot-on-artificial-analysis-at-58/
source_anchor: ""
source_lines: [1, 28]
section: "Claude_Anthropic"
---
**Anthropic released Claude Opus 5.5 on September 22, 2026, and independent benchmarker Artificial Analysis now ranks it first on its Intelligence Index with a score of 58** â five points above GPT-6 Astra and Claude Fable 5.1, which are tied at 53. Anthropic also cut Opus pricing by 20%. The result puts the gap between the top proprietary model and the best open-weights model, Xiaomi’s MiMo-V2.6-Pro at 46, at 12 points.

## What Artificial Analysis Measured

The Artificial Analysis Intelligence Index v4.3 combines ten evaluations: AA-Briefcase, GDPval-AA v2, AutomationBench-AA, Terminal-Bench 4.0, SciCode, Humanity’s Last Exam, GDP.pdf, CritPt, AA-Omniscience and AA-LCR v1.1. Tested at its maximum effort setting with fallback enabled, Opus 5.5 scored 58, which Artificial Analysis called “the highest score we have measured by several points.” The previous Claude Opus 5 scored 51.

According to Artificial Analysis, Opus 5.5 leads on six of the ten evaluations, including Humanity’s Last Exam (61.4%), SciCode (66.9%), GDPval-AA, AA-Briefcase, AA-Omniscience and AutomationBench-AA. On Terminal-Bench 4.0 it ties GPT-6 Astra at 59.6%. Anthropic’s own announcement reports 66.4% on Terminal-Bench 4.0. The two figures come from different test harnesses and should not be compared directly.

The largest margin is on AA-Briefcase, Artificial Analysis’s agentic knowledge-work benchmark. There Opus 5.5 (max) reaches 1,822 Elo against 1,678 for Fable 5.1. The higher scores come with more tokens: Opus 5.5 (max) uses about 119k output tokens per index task, against about 73k for Opus 5 (max). Because of the price cut, Artificial Analysis says its cost per task stays level with Opus 5 despite using 1.6 times the tokens. Four of its five effort levels (max, xhigh, high and medium) sit on the benchmarker’s intelligence-versus-cost Pareto frontier.

## Pricing and Availability

Opus 5.5 is available through the Claude Platform as `claude-opus-5-5`, and on AWS, Google Cloud and Microsoft Azure. Pricing is:

- **Input / output:** $4 / $20 per million tokens (Opus 5: $5 / $25)
- **Cache reads:** $0.20 per million tokens (down from $0.50)
- **Cache writes:** $5 per million tokens (down from $6.25)
- **Fast mode:** $8 / $40 per million tokens

Anthropic says Opus 5.5 “performs at the level of Claude Fable 5.1 on most work.” It also says the model costs 40% less to run than Opus 5 on typical workloads and generates output more than 30% faster. The company also said it worked on writing style, saying the model “puts the most important information up front”. The Decoder linked this to long-running criticism of “Claudish” prose. External evaluators including METR tested the model before release, and Anthropic says Claude Sonnet 5.5 and Haiku 5.5 will follow in the coming weeks.

## What This Means

The 5-point lead is uneven across the index. Most of it comes from agentic and knowledge-work tasks, not from coding, where GPT-6 Astra scores the same. Readers choosing a model for terminal-heavy agent work will see a smaller difference than the headline score suggests.

The gap to open weights is now wider. On the same v4.3 index, the highest open-weights scores are MiMo-V2.6-Pro at 46, GLM-5.3 (max) at 45 and Kimi K3 (max) at 44. That makes the gap between the top open and closed models 12 points, up from 7 when the proprietary leaders stood at 53. Cost changes the comparison, though. On Artificial Analysis’s cost chart, MiMo-V2.6-Pro costs roughly $0.13 per index task, while Opus 5.5 at maximum effort costs around $6. That is about one-fiftieth of the cost for a score 12 points lower, a trade-off that will matter for high-volume research and teaching workloads. Readers should also note that Artificial Analysis revises its index between versions, so scores from earlier versions (including those in some of our past coverage) cannot be compared with v4.3 figures.

*This post was drafted with AI assistance and reviewed by RITS staff.*
