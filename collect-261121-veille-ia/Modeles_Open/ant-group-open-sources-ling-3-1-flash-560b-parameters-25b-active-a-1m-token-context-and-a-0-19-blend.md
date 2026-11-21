---
id: collect-261121-veille-ia/modeles-open/ant-group-open-sources-ling-3-1-flash-560b-parameters-25b-active-a-1m-token-context-and-a-0-19-blend
title: "Ant Group Open-Sources Ling-3.1-Flash: 560B Parameters, 25B Active, a 1M-Token Context and a $0.19 Blended Price"
domain: modeles-open
role: reference
task: article
actors: []
dates: []
keywords: ["model", "ling-3", "flash", "team", "about", "context", "price", "group"]
source: https://www.aireport.net/Gn_6e612db17f1d
source_anchor: ""
source_lines: [1, 20]
section: "Modeles_Open"
---
Ant Group's BaiLing team open-sourced Ling-3.1-flash on Thursday, publishing the full weights on Hugging Face and ModelScope so teams can inspect and run the model inside their own compute environments. The model was announced on September 30 and pitched around a single idea: real-world work, not leaderboard trivia. Now the weights are out, and the deployment story is the headline.

Ling-3.1-flash carries roughly 560 billion total parameters with about 25 billion activated per token, and supports context windows up to 1 million tokens. The architecture is a hybrid design that combines linear attention with a sparse mixture of experts: within each eight-layer block, seven layers use KDA linear attention and one uses Gated MLA. The expert stack has 512 routed experts with eight selected per token, plus one shared expert.

The quantization work is what makes it practical. Routed experts are stored in MXFP4 while dense layers keep block-FP8 precision, which shrinks the weight footprint from roughly 534GB to about 292GB. In the team's Hopper GPU test configuration, serving drops from eight cards to four, with the usual caveat that real requirements vary with context length, concurrency and service configuration. The release also includes long-context parallel inference schemes (PCP and DCP) and enhanced JSON Schema support for structured output.

On the numbers: Artificial Analysis lists an intelligence index of 41, output speed of about 210 tokens per second and a blended price of roughly $0.19 per million tokens. The team reports an ELO of 1673 on the GDPVal-AA V2.1 office benchmark, a score of 65.35 on HealthBench Professional, and results on the FrontierSWE software-engineering and Terminal-Bench 4.0 terminal benchmarks. In an internal Lua native compiler development task, the team says the model worked for about 17 hours and passed 97.8% of independent tests. Those figures are the company's own unless marked otherwise, and in Design Arena's open-weights category Ling-3.1-flash ranks third in data visualization, React Native apps, 3D design and game development.

The intended workload is unglamorous and, for buyers, more relevant than benchmark peaks: turning scattered documents into a report, structuring a patient's fragmented symptoms into a coherent history, reading an unfamiliar legacy codebase and writing new features against tests until they pass. The team says the model can generate Excel files, presentations and working code, and that it already runs inside Ant's internal desktop collaboration agent, Xiaohu.

The limitations are stated unusually plainly. The model can lose focus when fed very large amounts of long text at once, one-shot generation has boundaries, and there is no native visual understanding, so a separate vision model is required for multimodal work.

What is worth watching is the tier, not the rank. An index of 41 at about $0.19 per million tokens is the flash-class price band where Chinese open-weight labs are now competing directly, and the differentiation has shifted from peak scores to cost per completed workflow: how few GPUs it takes to serve, how long a task the model can hold, and how cheaply it can be called at volume. On those terms, a 560B model that fits in 292GB and four cards is a more practical argument than any leaderboard position.

## Comments (0)

Log in to join the discussion

Log In
No comments yet
