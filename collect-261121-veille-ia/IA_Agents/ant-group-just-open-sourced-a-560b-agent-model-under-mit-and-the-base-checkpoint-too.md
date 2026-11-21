---
id: collect-261121-veille-ia/ia-agents/ant-group-just-open-sourced-a-560b-agent-model-under-mit-and-the-base-checkpoint-too
title: "Ant Group just open-sourced a 560B agent model under MIT, and the base checkpoint too 🧩"
domain: ia-agents
role: reference
task: article
actors: []
dates: []
keywords: ["model", "base", "under", "checkpoint", "group", "agent", "just", "open-sourced"]
source: https://medium.com/@victor.grosser/ant-group-just-open-sourced-a-560b-agent-model-under-mit-and-the-base-checkpoint-too-a88b99fe7b5d
source_anchor: ""
source_lines: [1, 21]
section: "IA_Agents"
---
# Ant Group just open-sourced a 560B agent model under MIT, and the base checkpoint too 🧩

On 27 October, inclusionAI, the AI lab of Ant Group, published **Ling-3.1-flash-1019** on Hugging Face. It’s an upgraded version of Ling-3.1-flash with extra post-training aimed at reasoning and real-world agent tasks. What made me stop scrolling wasn’t only the size. It was the licence: MIT, one of the most permissive licences there is, for a frontier-scale model. And two days earlier, the team also released **Ling-3.1-flash-base**, the raw pretrained checkpoint, under the same licence.

## A big model that thinks small per token

Ling-3.1-flash-1019 has 560B parameters in total, but only about 25B are active for each token. It is a mixture-of-experts model with 512 routed experts, of which 8 (plus one shared expert) fire per token. The attention stack is the part I find most interesting: 63 layers of KDA, a linear-attention variant, combined with 9 Gated MLA layers, in a 7:1 ratio. Linear attention keeps the cost of long inputs under control, while the few full-attention layers help the model still find the exact detail it needs. The model card advertises a 1M-token context window.

## Why I find this exciting

According to the model card, the extra post-training lifted its AA-Briefcase score from about 1400 to 1524 Elo and AutomationBench-AA from 62% to 68%, with gains on science tasks too (CritPt 18% to 26%, SciCode 54% to 57%). Overall it scores 44 on the Artificial Analysis Intelligence Index, up from 41 for its predecessor. Those are exactly the multi-step, tool-using workloads that agents live on.

But the part I value most is the base model. Releasing a base checkpoint means researchers and companies can do their own continued pretraining, domain adaptation and post-training, instead of only fine-tuning someone else’s finished chat model. For anyone interested in distillation, a large MIT-licensed teacher with a matching base model is a very useful starting point. The team even ships day-0 support: a dedicated SGLang image with speculative decoding, plus vLLM instructions with tool-calling and reasoning parsers.

## The honest caveats

This is not a homelab model. The reference SGLang setup runs on 8 H200 GPUs (or 4 GB300s), so most of us will meet it through an API or OpenRouter. The benchmark gains are the developer’s own figures; Neural Brew notes that only the overall index was visible on Artificial Analysis when they checked. And while the card advertises 1M tokens of context, the shipped config.json sets max_position_embeddings to 262,144, so check your serving setup before throwing a whole codebase at it.

## Sources

Open weights, an open base model and a permissive licence: that’s the kind of release that lets the whole community build further. I’m curious what people will distil from it.
