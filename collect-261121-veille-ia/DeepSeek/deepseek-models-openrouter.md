---
id: collect-261121-veille-ia/deepseek/deepseek-models-openrouter
title: "DeepSeek Models | OpenRouter"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: []
keywords: ["deepseek", "flash", "model", "models", "openrouter", "architecture", "completion", "october"]
source: https://openrouter.ai/provider/deepseek
source_anchor: ""
source_lines: [1, 1]
section: "DeepSeek"
---
DeepSeek V4.1 Flash 1017 is the October 17, 2026 snapshot of DeepSeek V4.1 Flash, served by DeepSeek under the `deepseek-flash` API model name. V4.1 Flash is a sparse mixture-of-experts model from DeepSeek, and the first built on the company's Causal Encoder-Decoder (CED) architecture. It activates 8B parameters on input and 16B on output from a 552B-parameter backbone, an asymmetric split that keeps per-token compute low relative to the model's total size. Image understanding is native to the architecture, with visual and text embeddings trained jointly from the start of pre-training rather than added afterward as in the earlier experimental V4 Flash Vision Exp.  It is suited for coding, terminal, and computer-use agents, along with long-horizon tasks that must run to completion across many steps and long-context analysis. Compressed KV caching cuts cache memory to roughly a quarter of the previous Flash generation, significantly reducing costs on agentic workloads. DeepSeek positions it as the cost-efficient tier of the V4.1 family and reports that it exceeds V4 Pro on performance, speed, and task completion time.
