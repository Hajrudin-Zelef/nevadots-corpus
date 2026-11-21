---
id: collect-261121-veille-ia/muse/muse-spark-open-2
title: "Muse Spark Open"
domain: muse
role: reference
task: article
actors: ["Meta"]
dates: []
keywords: ["muse", "model", "spark", "open", "meta", "reasoning", "parameters", "context"]
source: https://lmstudio.ai/models/muse-spark-open
source_anchor: ""
source_lines: [1, 34]
section: "Muse"
---
Meta's 816B Mixture-of-Experts model for coding, reasoning, and long-horizon agents, with 42B active parameters, native multimodal understanding, and a 1M-token context window.

Muse Spark Open is Meta Superintelligence Labs' open-weight model for coding, tool use, and long-horizon agentic work. It uses a Mixture-of-Experts architecture with 816B total parameters and 42B active parameters, and supports a 1M-token context window.

*Results reported by Meta in its launch post. Evaluation settings and harnesses vary; see the official model card for methodology and the full results.*

Muse Spark Open is available through LM Studio Cloud in Bionic, with up to 1,048,576 tokens of context, image understanding, and tool calling. This page lists the Cloud model only; it does not include local downloads.

Choose low, medium, high, or xhigh reasoning effort. Reasoning is always enabled: an off setting maps to low. Meta recommends high or xhigh for complex problem solving, coding, and agentic tasks, and low or medium for faster, shorter responses.

Muse Spark Open is designed to work through extended tasks rather than just produce a single answer. Its training emphasizes writing and debugging code, resolving software-engineering tasks, invoking tools across multiple turns, and recovering from tool errors.

The 1M-token context window accommodates large repositories, documentation, and long-running task histories. Its sparse MoE architecture activates only a subset of its parameters for each token, while a hybrid local/global attention design reduces the memory needed to serve long contexts.

The underlying model accepts text, images, video, and audio and produces text. It can reason over screenshots, charts, documents, and recordings alongside a conversation. The current LM Studio Cloud listing exposes text and vision support; the model's broader audio and video capabilities should not be read as additional input modes available in Bionic.

Meta reports the following results for Muse Spark Open at xhigh reasoning effort, using temperature 1.0, top_p 1.0, and top_k disabled:

| Benchmark | Muse Spark Open | 
|---|---|
| JobBench | 61.8 | 
| MCP-Atlas | 89.2 | 
| HLE-Diamond (no tools) | 26.1 | 
| GPQA Diamond | 89.0 | 
| Terminal-Bench 2.1 (Terminus 2 agent) | 76.4 | 
| DeepSWE 1.1 (preferred harness) | 60 | 
| OSWorld Verified | 80.4 | 
| ZeroBench (with tools) | 50.0 | 

These are publisher-reported evaluations of the released model, not measurements of LM Studio Cloud. Benchmark scores depend on the harness, reasoning settings, tool access, and other evaluation conditions.

Released on October 15, 2026, Muse Spark Open is available under the custom Muse Model License, which includes commercial terms and a usage policy. It is an open-weight release, not a claim of unrestricted use.

See Meta's launch post and official model card for architecture, intended uses, evaluation methodology, and safety considerations.
