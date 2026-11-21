---
id: collect-261121-veille-ia/outils-agents/releases-ollama-ollama
title: "Releases · ollama/ollama"
domain: outils-agents
role: reference
task: article
actors: []
dates: []
keywords: ["ollama", "model", "models", "launch", "lfm3", "claude", "changed", "dhiltgen"]
source: https://github.com/ollama/ollama/releases
source_anchor: ""
source_lines: [1, 165]
section: "Outils_Agents"
---
## v0.40.17

## What's Changed

- Improve ChatGPT CLI restore and graceful restart by @hoyyeva in #19583
- cmd/launch: use thinking metadata for OpenCode settings by @hoyyeva in #19409
- llm: use mmap for Vulkan integrated GPUs by @dhiltgen in #19640
- rpreslar4765/Ollama by @rpreslar4765 in #17163
- mlx: support concurrent generation by @dhiltgen in #19197
- llama.cpp: version bump b12257 by @dhiltgen in #19646
- MLX, MLX-C: version bump by @dhiltgen in #19647

## New Contributors

- @rpreslar4765 made their first contribution in #17163

**Full Changelog**: `v0.40.16...v0.40.17-rc0`

## v0.40.16

## Ripple Preview

Ripple Preview from Ricursive Intelligence is a 65B dense model built for agentic coding.

`ollama run ripple-preview`
## What's Changed

- Qwen3.8 Flash Next uses about 27 GiB less memory on Apple silicon
- `ollama launch claude` always routes Claude Code to the selected model
- `ollama launch` explains when a cloud model has been retired
- `/v1/responses` respects`truncation` and no longer silently truncates by default
- `/v1/models` returns an empty list instead of`null` when no models are installed
- CUDA 13.5 support for Windows on ARM
- Updated llama.cpp

**Full Changelog**: `v0.40.15...v0.40.16`

## v0.40.15

## MiMo V2.6

Xiaomi MiMo's mimo-v2.6, a 9B model built for code and agent workflows, is now supported in Ollama.

`ollama run mimo-v2.6`
## AutoTrust decision models

AutoTrust decision models read text and return choices, yes or no probabilities, and ratings for the questions you supply. The family includes 9b, 35b, and 27b-vl sizes — the 35b and 27b-vl sizes can also read images.

`ollama run autotrust-jev "A customer says their card was charged twice." --ask "Is this a billing issue?"`
Note: autotrust-jev is supported on Apple silicon in this release.

## What's Changed

- ollama launch now supports Kimi Code CLI — the successor to kimi-cli — and offers to migrate existing kimi-cli setups
- ollama launch now fills in Pi's model list automatically so models can be picked inside Pi, and Claude Desktop's model
picker shows Ollama's actual model names
- Recurrent and vision GGUF models can now serve more than one request at a time
- Qwen3 and Qwen3.5 models using long-context (YaRN) scaling
- Running ollama with no arguments opens the welcome launcher

**Full Changelog**: `v0.40.14...v0.40.15`

## v0.40.14

## What's Changed

- `ollama launch claude` now applies the model's output token limit for Claude Code, so cloud models no longer run into the 128k default cap
- Cloud web search responses on `/api/chat` are now decoded correctly, including compressed responses
- OpenAI-compatible endpoints no longer override a model's packaged sampler defaults — clients that send no `temperature` /`top_p` get the model's own values instead of 1.0, matching`/api/chat`
- `/v1/responses` now preserves readable input content, including prior assistant refusals, instead of flattening it
- Ollama's app can now retry ChatGPT disconnect failures instead of leaving the connection in a broken state
- `ollama launch codex` verifies ChatGPT routing before configuring, and reports a clear error when routing is unavailable
- Faster top-k sampling on MLX for large vocabularies

**Full Changelog**: `v0.40.13...v0.40.14`

## v0.40.13

## v0.40.12

## Liquid AI LFM3

LFM3 is Liquid AI's new family of small models built for local agents, tool calling, and structured output, on Liquid's new tri-hybrid architecture. The family includes:

- lfm3: a 1.7B text model with a 256K-token context window
- lfm3-vl: adds image understanding
- lfm3-audio: speech recognition, translation, and text-to-speech in 18 languages
- lfm3-omni: text, images, and audio in one model

`ollama run lfm3`
## d1 decision models

Liquid AI's d1 models now come in a 2B size, built on LFM3. d1 makes decisions about text and images, and d1-omni adds audio.

`ollama run d1:2b "I was charged twice for order 4417." --ask "Is this a billing issue?"`
Note: lfm3 and d1 2b are supported on Apple hardware in this release. Support for Linux/Windows coming soon.

## What's Changed

- Faster prompt processing on Apple silicon for Gemma 4, Flint and Laguna models
- Speculative decoding now supports DFlash2 draft models on MLX
- Gemma 4 keeps more detail in small images
- `ollama launch` now works with OpenCode v2
- Fixed tool-only assistant turns from Kimi CLI
- Updated MLX and llama.cpp

**Full Changelog**: `v0.40.11...v0.40.12`

## v0.40.11

## Speech

Ollama can now generate speech on macOS. `ollama run` plays text or a chat model's reply aloud as the audio is generated, and apps can stream audio through the OpenAI-compatible `/v1/audio/speech` endpoint.

`ollama run phonon "Hello from Ollama."`
## d3 decision models

d3 brings vLLM Semantic Router's decision models to Ollama. Give a model text, structured state, or an image, and ask it to classify, answer yes or no, or score options with probabilities. The family includes 2B, 4B, 9B, and 27B sizes.

`ollama run d3 "A customer says their card was charged twice." --ask "Is this a billing issue?"`
## New Contributors

- @SynapShift made their first contribution in #19173

**Full Changelog**: `v0.40.10...v0.40.11`

## v0.40.10

## NVIDIA Nemotron 3.5 Bolt E4B

Nemotron 3.5 Bolt E4B is NVIDIA’s smaller, Gemma 4-based vision-language model for fast local conversations, image understanding, and tool use. It supports a 128K-token context window.

The model includes a DFlash draft for speculative decoding.

```
ollama run nemotron-3.5-bolt:e4b
```
**Full Changelog**: `v0.40.9...v0.40.10`

## v0.40.9

## Flint on Linux and Windows

Flint now runs on Linux and Windows as well as macOS.

## Other improvements

- Streaming OpenAI Responses and Anthropic Messages requests now report generation failures instead of ending silently after partial output.
- `ollama launch` now passes cloud models' context lengths and capabilities to coding tools, fixing incorrect context and compaction settings.
- Anthropic-compatible web search now uses the model's supported thinking setting in follow-up requests, preventing avoidable failures.

**Full Changelog**: `v0.40.8...v0.40.9`

## v0.40.8

## Recursive Flint

Flint brings Recursive's coding model to Ollama. It is designed for the work coding agents do across a repository: understanding unfamiliar code, tracing bugs, implementing features, and adding tests. Reasoning, multi-turn tool use, and a 262K-token context window support longer tasks across files and tool interactions.

Flint is an Apache 2.0-licensed, 40B-parameter mixture-of-experts model with approximately 2.5B parameters active per token. In its release evaluations, Recursive reports 54.9% on Terminal-Bench 2.1 and 74.2% on SWE-bench Verified.

`ollama run flint`
Note: Flint is supported on MacOS. Other GPU support will be enabled in a subsequent release.

**Full Changelog**: `v0.40.7...v0.40.8`
