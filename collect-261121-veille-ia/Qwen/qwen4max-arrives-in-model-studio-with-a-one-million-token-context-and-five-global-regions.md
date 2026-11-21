---
id: collect-261121-veille-ia/qwen/qwen4max-arrives-in-model-studio-with-a-one-million-token-context-and-five-global-regions
title: "Qwen4‑Max arrives in Model Studio with a one-million-token context and five global regions"
domain: qwen
role: reference
task: article
actors: ["Alibaba"]
dates: ["2026-11-16"]
keywords: ["model", "studio", "global", "reasoning", "context", "regions", "qwen4max", "text"]
source: https://technoyasno.com/en/read/alibaba-qwen4-max-model-studio-2026-11-16/
source_anchor: ""
source_lines: [1, 15]
section: "Qwen"
---
## One model ID for text, images and video

In Alibaba Cloud Model Studio, the model is invoked as qwen4-max. It accepts text, images and video, returns text, and supports deep reasoning, parallel function calls, web search and structured JSON. Integration options include OpenAI Chat Completions and Responses, Anthropic Messages and DashScope, letting an existing client switch without a separate multimodal API.

The context window is one million tokens. Without reasoning, maximum output is 131,072 tokens; with reasoning enabled, the combined budget for reasoning and the final answer reaches 262,144 tokens. These are technical limits rather than a quality guarantee across the whole window: long-context use still requires testing retrieval accuracy and invocation cost.

## Availability depends on region

The Beijing region serves mainland China, Singapore serves an international scope, and Frankfurt, Virginia, Tokyo and Hong Kong are listed as global. Batch inference is supported in Beijing but is not promised for the listed international and global regions. Streaming, search, tools and structured output are available in every documented location.

The new entry uses the stable qwen4-max ID and is distinct from qwen4-max-preview, which was limited to Token Plan from November 11 through 25. Model Studio does not currently support fine-tuning Qwen4‑Max or deploying it as a dedicated model instance. The release suits developers who want a managed API, but it does not replace a self-hosted weights-and-infrastructure option.

## Sources

- Alibaba Cloud Model Studio — Qwen4-Max · 16 November 2026
