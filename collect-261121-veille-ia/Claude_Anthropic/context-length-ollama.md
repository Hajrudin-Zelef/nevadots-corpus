---
id: collect-261121-veille-ia/claude-anthropic/context-length-ollama
title: "Context length - Ollama"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["context", "length", "ollama", "vram", "model", "setting", "increase", "maximum"]
source: https://docs.ollama.com/context-length
source_anchor: ""
source_lines: [1, 18]
section: "Claude_Anthropic"
---
Ollama defaults to the following context lengths based on VRAM:

- < 24 GiB VRAM: 4k context
- 24-48 GiB VRAM: 32k context
- >= 48 GiB VRAM: 256k context

## Setting context length

Setting a larger context length will increase the amount of memory required to run a model. Ensure you have enough VRAM available to increase the context length. Cloud models are set to their maximum context length by default.
### App

Change the slider in the Ollama app under settings to your desired context length.
### CLI

If editing the context length for Ollama is not possible, the context length can also be updated when serving Ollama.
### Check allocated context length and model offloading

For best performance, use the maximum context length for a model, and avoid offloading the model to CPU. Verify the split under`PROCESSOR` using `ollama ps`.
