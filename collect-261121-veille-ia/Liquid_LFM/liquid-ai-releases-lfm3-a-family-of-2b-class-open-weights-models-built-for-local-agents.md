---
id: collect-261121-veille-ia/liquid-lfm/liquid-ai-releases-lfm3-a-family-of-2b-class-open-weights-models-built-for-local-agents
title: "Liquid AI releases LFM3, a family of 2B-class open-weights models built for local agents"
domain: liquid-lfm
role: reference
task: article
actors: ["Liquid AI"]
dates: []
keywords: ["lfm3", "ollama", "against", "liquid", "llama", "model", "models", "lfm3-1"]
source: https://geekbyte.com/post/liquid-ai-lfm3-local-models/
source_anchor: ""
source_lines: [1, 100]
section: "Liquid_LFM"
---
Liquid AI released LFM3 on 27 October 2026, a family of small open-weights models for agents and tool calling on laptops and phones. By 28 October, you could run it with llama.cpp or Ollama on macOS, Linux and Windows.

The text model, LFM3-1.7B, has 1.73 billion parameters and a 256K-token context window. Liquid says its weights fit in about 1 GB at 4-bit. It covers 16 languages.

The launch post describes six 2B-class models branched from LFM3-1.7B-Base: text, vision, audio and omni, plus two d1 decision models that score answers instead of generating text.

## Liquid AI’s benchmarks

Liquid compares LFM3-1.7B with Qwen3.5 9B, the largest model in the model card’s table. The results are mixed. LFM3-1.7B leads on:

- BFCL V4 (tool calling): 71.39 against 62.83.
- IFEval (instruction following): 96.66 against 92.08.
- LiveCodeBench v6 (coding): 73.46 against 69.86.
- Full-table average: 49.37 against 47.24.

It trails on:

- MCP-Atlas (tool use): 32.57 against 42.67.
- SWE-bench Verified (agentic coding, OpenCode agent): 43.40 against 51.20.
- GPQA Diamond (knowledge): 63.37 against 80.40.
- RULER 128K (long context): 65.56 against 87.11.

Liquid ran these benchmarks on vLLM with BF16 (bfloat16, a 16-bit number format) weights. A 4-bit GGUF, the file format llama.cpp and Ollama load, may score lower. The card itself says the model “is not recommended for agentic coding or complex multi-step reasoning due to its small size.”

## Speed and memory

Liquid says a full 128K-token context needs 1.6 GB of key-value (KV) cache, the memory that holds the context being processed, against 4.3 GB for Qwen3.5-4B. That sits on top of the roughly 1 GB of weights.

In the card’s edge inference tests (Q4_0, 32K context) on AMD, Apple and iPhone hardware, LFM3-1.7B was at least 2.5x faster at prefill (processing the prompt) and at least 1.8x faster at decode (generating tokens) than every compared model. LFM3 ran on an unnumbered llama.cpp build, the others on release b11744.

## Running it locally

We checked these commands against the Ollama v0.40.12 and v0.40.13 release notes, the llama.cpp b11938 release, the llama.cpp server README and the model card on 7 November 2026. We have not run them ourselves.

We recommend Ollama for a first try, because it needs no flags. You need v0.40.12 (27 October) or later on Apple hardware, or v0.40.13 (28 October) or later on Linux and Windows.

This downloads `lfm3` and shows what it supports:

```
ollama pull lfm3
ollama show lfm3
```
The capabilities should list tools and thinking. This opens an interactive chat with the model:

`ollama run lfm3`
Type `/bye` to leave the chat and get back to your shell.

For llama.cpp, text and vision support shipped in b11938 (PR #31252). This starts a server with the card’s sampling settings and an 8K context:

`llama-server -hf LiquidAI/LFM3-1.7B-GGUF --ctx-size 8192 --temp 0.1 --top-k 50 --repeat-penalty 1.1 --cors-origins localhost --api-key <key>`
Two flags matter here:

- `--ctx-size 8192` keeps the KV cache small. Without it, llama.cpp starts from the 256K maximum and sizes unset settings to device memory (server README).
- `--cors-origins localhost` stops other sites’ pages reading responses, but the server still processes their simple requests. That is why we add`--api-key` .

Native builds listen on 127.0.0.1:9931 since PR #30159 (9 October).

Reasoning is on by default. The card says turning it off (`enable_thinking=False`, or `--think=false` in Ollama) “can be inconsistent.”

If tool calls come back as plain text, re-download the GGUF or pass the repo’s `chat_template.jinja` with `--chat-template-file`.

## Security notes

- Pull only from the `LiquidAI` Hugging Face organisation or the official`lfm3` Ollama entries.
- The Transformers path needs `trust_remote_code=True` , which runs the repository’s Python. Pin`revision` to a commit you have reviewed.
- Ollama binds to 127.0.0.1:11434 by default (FAQ) and its local API needs no authentication. Keep it on localhost.
- Small models mis-call tools. Validate every call against your schema and confirm destructive actions.

## Licence

LFM3 ships under the LFM Open License v1.0, which reads like Apache 2.0 apart from section 5. That section ties commercial use to a US$10 million annual revenue threshold, counting affiliates under common control.

The exact boundary is ambiguous. The threshold is defined as $10 million “or more”, while section 5 licenses entities “not exceeding” it and excludes those that “exceed” it. If your company is near that figure, ask Liquid AI.

In our reading, non-commercial use stays licensed at any size.

## What it means for local setups

In our view, LFM3-1.7B fits best as the always-loaded layer of a local agent for routing, tool calls and extraction. Hand coding and knowledge-heavy work to a larger model.

Test it against your own tool schemas before you wire it into anything that writes data.

## Sources

1. Liquid AIIntroducing LFM3: Every Modality for Specialized Intelligence
2. Liquid AI (Hugging Face)LiquidAI/LFM3-1.7B model card
3. Liquid AI (Hugging Face)LiquidAI/LFM3-1.7B model card: Edge inference
4. Liquid AILFM Open License v1.0 (LFM3-1.7B repository)
5. ggml-orgllama.cpp PR #31252: add LiquidAI/LFM3-1.7B and LiquidAI/LFM3-VL-2B
6. ggml-orgllama.cpp release b11938
7. ggml-orgllama.cpp PR #30159: server: use port 9931 by default
8. ggml-orgllama.cpp server README
9. ggml-orgllama.cpp server-http.cpp (CORS handling)
10. OllamaOllama v0.40.12 release notes
11. OllamaOllama v0.40.13 release notes
12. Ollamalfm3 in the Ollama library
13. OllamaOllama API: Authentication
14. OllamaOllama FAQ

Written with AI assistance from the sources above, and fact-checked by a second AI model before publication. How we use AI
