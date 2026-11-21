---
id: collect-261121-veille-ia/modeles-open/ling-releases-the-open-weights-of-ling-3-1-flash-1019-and-a-base-model-ai-tracker
title: "Ling releases the open weights of Ling-3.1-flash-1019 and a base model · AI Tracker"
domain: modeles-open
role: reference
task: article
actors: []
dates: ["2026-10-27"]
keywords: ["model", "ling-3", "open", "flash-1019", "ling", "weights", "releases", "base"]
source: https://ai-tracker.rootstudio.io/news/2026-10-27-ling-3-1-flash-1019-open-weights
source_anchor: ""
source_lines: [1, 15]
section: "Modeles_Open"
---
# Ling releases the open weights of Ling-3.1-flash-1019 and a base model

The Ling team says Ling-3.1-flash-1019 is now open source, with weights on Hugging Face and ModelScope. It also released Ling-3.1-flash-base, which it describes as an open foundation for further training. The team says the new model had further post-training for reasoning and real-world agent workflows.

The figures below are the team's own claims, based on Artificial Analysis benchmarks, comparing Ling-3.1-flash with Ling-3.1-flash-1019:

- **Intelligence Index (v4.3.2):** 41 to 44.
- **AA-Briefcase:** about 1400 to 1524 Elo.
- **AutomationBench-AA:** 62% to 68%.
- **CritPt:** 18% to 26%.
- **SciCode:** 54% to 57%.

The team also links an online page for trying the model.

**Artificial Analysis** (a benchmark site, its own measurements) confirms the gains and describes the model as a reasoning model with 560B total and 25B active parameters, a 1M-token context window and an MIT license. It scores 44 on its Intelligence Index v4.3, up from 41, with 43% on Humanity's Last Exam (39% before), 26% on CritPt (18%), an Elo of 1521 on AA-Briefcase v1.1 (1397) and 1653 on GDPval-AA v2.1 (1617), and 68% on AutomationBench-AA (62%). It notes the model uses about 145k output tokens per Index task, over 40% more than its predecessor's 103k, so the cost per task is $1.49, higher than before but below Mistral Large 4 Preview and GLM 5.3. It lists first-party API pricing of $0.30 per million input tokens and $0.90 per million output tokens, with cached input at $0.06.
