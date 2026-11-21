---
id: collect-261121-veille-ia/modeles-open/elorian-launches-glimpse-visual-scratch-space-model-by-ex-deepmind-team-nextvector
title: "Elorian Launches Glimpse: Visual Scratch Space Model by Ex-DeepMind Team | NextVector"
domain: modeles-open
role: reference
task: article
actors: []
dates: []
keywords: ["glimpse", "elorian", "visual", "model", "million", "across", "scratch", "space"]
source: https://nextvector.rhasan.online/article/elorian-releases-glimpse-visual-scratch-space-model
source_anchor: ""
source_lines: [1, 9]
section: "Modeles_Open"
---
According to official technical disclosures published by Elorian and API telemetry confirmed via OpenRouter on October 17, 2026, the Palo Alto-based artificial intelligence startup has released Glimpse, its first foundation model engineered around an interactive visual reasoning architecture. Founded by former Google DeepMind director and Gemini data lead Andrew Dai alongside former Apple and Google researchers Yinfei Yang and Dustin Tran, Elorian previously secured $55 million in seed funding at a $300 million valuation from Menlo Ventures, Altimeter Capital, NVIDIA, and Google chief scientist Jeff Dean.

Glimpse introduces an architectural departure from standard vision-language models. In conventional multimodal architectures, high-resolution images or video clips are processed through a vision encoder once at the start of inference, generating a fixed sequence of token embeddings from which the language backbone must reason without further access to original pixel data. Elorian's engineers argue that this static tokenization forces models to answer visual queries from degraded memory, leading to persistent failures on dense spatial tasks such as reading text-free assembly diagrams, parsing electronic schematics, or tracking game pieces across cluttered physical environments.

To circumvent this bottleneck, Glimpse implements an active visual scratch space. During the chain-of-thought generation phase, the model can emit intermediate visual tool calls that re-interrogate the raw source image or video feed. It can dynamically request crops of fine-grained regions to resolve small features, adjust temporal sampling rates across video intervals to track rapid motion, or execute lightweight code routines to compute exact geometric coordinates and pixel distances before outputting a terminal response.

Trained on an open-weight base architecture across an internal cluster of several hundred GPUs, Glimpse was optimized for compute efficiency rather than brute-force parameter scaling. In early evaluations across multi-image spatial reasoning and dense object counting, the model demonstrated accuracy parity with commercial frontier systems while operating at substantially lower serving latency.

Elorian has opened public access to Glimpse through a waitlisted web chat interface and an API gateway, alongside immediate listing on OpenRouter under the identifier elorian/glimpse. The endpoint is priced at $0.60 per million input tokens, $3.00 per million output tokens, and $0.06 per million prompt cache read tokens across its native 262,144-token context window. Initial deployment partnerships focus on robotic teleoperation data curation, automated electronics inspection, and scientific laboratory microscopy.
