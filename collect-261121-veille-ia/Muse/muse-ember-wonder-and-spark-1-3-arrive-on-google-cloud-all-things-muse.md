---
id: collect-261121-veille-ia/muse/muse-ember-wonder-and-spark-1-3-arrive-on-google-cloud-all-things-muse
title: "Muse Ember, Wonder, and Spark 1.3 Arrive on Google Cloud | All Things Muse"
domain: muse
role: reference
task: article
actors: ["Meta"]
dates: []
keywords: ["muse", "cloud", "ember", "spark", "google", "wonder", "model", "oracle"]
source: https://www.allthingsmuse.com/articles/muse-models-google-cloud/
source_anchor: ""
source_lines: [1, 9]
section: "Muse"
---
Meta’s Muse models are now available on Google Cloud and Oracle Cloud. Alexandr Wang, Meta’s chief AI officer, announced the move on X today: “we’re also bringing muse ember to @googlecloud today along with muse wonder and muse spark 1.3!”

Meta’s research post on Ember says: “Muse Ember, Muse Spark and Muse Wonder are now available on Google Cloud and Oracle Cloud.” Google’s own documentation lists Muse Spark 1.3 on its Gemini Enterprise Agent Platform as model `meta/muse-spark-1.3`, in preview, with a global endpoint and a 1M-token context window.

The timing puts all three models where developers already run agent workloads. Ember launched this morning on the Meta Model API and Muse Code as `muse-ember-1.0`, priced at $0.10 per million input tokens and $0.50 per million output tokens, and it is also listed on OpenRouter.

Ember is the model most likely to benefit. It is built for the repetitive steps agents run hundreds of times in a session, tool calls, routing, extraction, and coding subtasks, and its whole pitch is cost: $0.14 per task on Artificial Analysis’s Intelligence Index, against $2.39 for Spark 1.3 at nearly the same score. Putting it on Google Cloud and Oracle Cloud lets teams use it inside the cloud they already pay for, instead of adding another API vendor. Spark 1.3 covers coding and agentic workflows, and Wonder handles the hardest problems.

Vercel’s AI Gateway added Ember today as well, under the name `meta/muse-ember`, at provider pricing with no markup. Between the Meta Model API, OpenRouter, Google Cloud, Oracle Cloud, and Vercel, the model is now reachable wherever developers already call models.
