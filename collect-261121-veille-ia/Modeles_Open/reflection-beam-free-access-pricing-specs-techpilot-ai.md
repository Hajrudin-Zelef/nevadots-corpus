---
id: collect-261121-veille-ia/modeles-open/reflection-beam-free-access-pricing-specs-techpilot-ai
title: "Reflection Beam: Free Access, Pricing & Specs | TechPilot.ai"
domain: modeles-open
role: reference
task: article
actors: []
dates: []
keywords: ["beam", "free", "reflection", "model", "weights", "opencode", "apache-2", "open"]
source: https://techpilot.ai/tools/reflection-beam/
source_anchor: ""
source_lines: [1, 173]
section: "Modeles_Open"
---
Reflection Beam is a big, genuinely open coding and agent model. Try it free on Ollama Cloud or OpenCode while the offer lasts.

## The AI news that actually mattered this week.

One short email every Friday: the biggest AI stories, what they mean for you, and the tools worth trying. A 3-minute read, no hype.

Free. Unsubscribe anytime.

 **You're in.**
Watch for the brief on Friday. Free. Unsubscribe anytime.

## Summary

## Pros and cons

### What we like

-  **Open licence** Apache-2.0 weights you can self-host, fine-tune and ship.
-  **Free to try** No-cost access on Ollama Cloud and OpenCode for a limited time.
-  **Built for agents** Strong coding, tool calling and 1M-token context.
-  **Cheap hosted option** $0.25 input and $1 output per 1M tokens on OpenRouter.
-  **Wide support** vLLM, SGLang, Transformers, Unsloth and 20+ hosting partners.

### What could be better

-  **Huge to self-host** Even NVFP4 weights are 273 GiB.
-  **Free access is temporary** No end date announced for Ollama or OpenCode.
-  **Text only** No image, audio or video input.
-  **No official API price yet** Reflection hasn't published per-token rates.

## Key features

- Open weights: BF16, FP8 and NVFP4 checkpoints on Hugging Face under Apache-2.0.
- 1M-token context: Work with large codebases, logs and long documents in one request.
- Coding agents: Runs in Claude Code, Codex, OpenCode, Hermes and Reflection’s Mirror CLI.
- Reasoning control: Five effort levels, from low to max, to trade speed for depth.
- Tool calling: Function calling, structured outputs and MCP-based tool use.
- Fine-tuning: Recipes for RL and LoRA customization with partner training stacks.

## Screenshots

## Full review

Independent and reader-supported. Some links may earn us a commission; it never changes our scores. Editorial policy

Reflection AI’s Beam is a free, open-weight 501B model for coding agents. See how to use Reflection Beam free on Ollama and OpenCode, specs and pricing.

Most people searching for Reflection AI this week want to know three things: what Beam is, whether they can use it for free, and how to get it running. Short answers: Beam is Reflection’s first open-weight model, the weights are free under Apache-2.0, and right now you can use the hosted version at no cost on Ollama Cloud and in OpenCode for a limited time. Everything below was checked on official pages on October 19, 2026.

## What is Reflection AI’s Beam?

Reflection AI is an AI lab that builds open models. On October 16, 2026 it released Beam’s weights on Hugging Face under the Apache-2.0 licence, after announcing the model on October 5. Beam is a text-in, text-out model built for software engineering, multi-step tool use and math and science reasoning. Here are the key specs from Reflection’s model card:

- **Size:** sparse Mixture-of-Experts with 501.3B total and 23.4B active parameters per token (256 experts, 52 layers).
- **Context:** 1M tokens, with up to 128K output tokens on Reflection’s API.
- **Formats:** BF16, FP8 and NVFP4 weights, plus a small DFlash2 drafter model for faster speculative decoding.
- **Reasoning:** five effort levels (low to max), tool calling and structured outputs.
- **Knowledge cutoff:** June 30, 2026. Mainly trained for English.
- **Licence:** Apache-2.0. Reflection says it doesn’t prohibit fine-tuning, quantization, merging or using outputs to train other models.

Reflection’s pitch is token efficiency: it says Beam reaches similar quality to leading open models with about 3x fewer reasoning tokens. On its own model card Beam scores 81.6% on SWE-bench Verified and 80.1% on Terminal-Bench 2.1 (BF16). Those are vendor numbers, so treat them as a starting point and test it on your own code.

## Is Reflection Beam free?

Yes, in three ways, though two of them are limited-time offers:

- **The weights are free for good.** You can download Beam from Hugging Face and run, fine-tune or ship it commercially under Apache-2.0. The catch is hardware, covered below.
- **Ollama Cloud is free for a limited time.** Ollama says Beam is free on Ollama for a limited time, runs it in the cloud so you don’t download anything, and serves it with zero data retention.
- **OpenCode has a free Beam model.** OpenCode lists “Beam Free” (model ID`beam-free` ) at $0 for input and output in its Zen model list, flagged as limited time.

Neither Ollama nor OpenCode has said when the free period ends, so don’t build a production workflow on it. Reflection’s own API is in public beta and “free to start”, according to its launch post, but Reflection hasn’t published per-token prices on its site.

## How to use Reflection Beam for free

### Option 1: Ollama Cloud (terminal, Claude Code, Codex, OpenCode)

Install the latest Ollama, sign in to your Ollama account, then run Beam from the cloud with one command. Ollama can also launch popular coding agents with Beam as the model:

```
ollama run beam:cloud
# or plug Beam into a coding agent
ollama launch claude --model beam:cloud
ollama launch codex --model beam:cloud
ollama launch opencode --model beam:cloud
```
The `:cloud` tag matters: it runs on Ollama’s servers, not your computer. Ollama’s free account allows one request at a time, so long agent runs may queue.

### Option 2: OpenCode

In OpenCode, pick **Beam Free** from the model list, or set the model to `opencode/beam-free`. It’s the same route early testers used: Reflection’s launch post shows a Snake game built with Beam in OpenCode in about half the time of OpenCode’s Big Pickle model. If you like free coding models in OpenCode, our guide to using DeepSeek V4.1 Flash free in OpenCode works the same way.

### Option 3: Reflection’s API and Mirror CLI

Sign up at platform.reflection.ai, create an API key and call the OpenAI-compatible endpoint with the model ID `Beam-1.0-501B-A23B`. Reflection also open-sourced Mirror CLI, its own terminal coding agent tuned for Beam, and says Beam works with Pi, OpenCode and Hermes Agent.

```
curl https://api.reflection.ai/openai/v1/chat/completions \
  -H "Authorization: Bearer $REFLECTION_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model": "Beam-1.0-501B-A23B", "messages": [{"role": "user", "content": "Hello"}]}'
```
## Reflection Beam Pricing (2026)

Beam itself costs nothing to download. What you pay depends on where it runs. Prices below were checked on October 19, 2026:

| Where you use Beam | Price | Notes | 
|---|---|---|
| **Open weights (Hugging Face)** | Free | Apache-2.0. You pay for your own GPUs. | 
| **Ollama Cloud** | Free for a limited time | Zero data retention. End date not announced. | 
| **OpenCode (Beam Free)** | Free for a limited time | Model ID beam-free. | 
| **OpenRouter** | $0.25 per 1M input tokens, $1 per 1M output tokens | 1.05M context, listed Oct 16, 2026. | 
| **Reflection API** | Free to start (public beta) | Per-token prices not published on reflection.ai. | 

The cheapest paid option we could verify is OpenRouter, at $0.25 input and $1 output per million tokens. Beam is also offered by AWS, Google Cloud, Together AI, Fireworks, Baseten and others, each with its own pricing.

## Can you run Reflection Beam locally?

Only with serious hardware. Reflection’s serving guide targets four NVIDIA B300 SXM GPUs connected by NVLink, and the downloads are huge:

| Weights | Download size | Hugging Face repo | 
|---|---|---|
| **NVFP4** | 273 GiB | reflection/Beam-1.0-501B-A23B-NVFP4 | 
| **FP8** | 468.72 GiB | reflection/Beam-1.0-501B-A23B-FP8 | 
| **BF16** | 933.81 GiB | reflection/Beam-1.0-501B-A23B | 

That rules out a normal laptop or a single gaming GPU. Our step-by-step guide on how to run Reflection Beam locally covers the hardware, downloads and serving setup in detail. If you want a model you can actually run at home, see our list of the best local AI models, or our walkthrough on how to run Muse Spark Open locally, which covers the same kind of multi-machine setup for a large open model.

## Who should try Beam?

- **Developers who use coding agents:** Beam is tuned for Claude Code-style workflows, and the free OpenCode and Ollama access makes it an easy test.
- **Teams that need an open licence:** Apache-2.0 is about as permissive as model licences get, which matters if you want to self-host or fine-tune.
- **Long-context work:** 1M tokens of context helps with large codebases, logs and documents.
- **Not for images or voice:** Beam is text only and mainly English.

## Verdict

Reflection Beam is one of the most interesting open models of October 2026: a 501B coding and agent model with a truly open licence and a 1M-token context. For most people the smart move is to try it free on Ollama Cloud or OpenCode while that lasts, then decide whether OpenRouter’s low token price or self-hosting makes sense. Just don’t expect to run it on your own PC.

## TechPilot.ai’s Verdict on Reflection Beam

Reflection Beam is a big, genuinely open coding and agent model. Try it free on Ollama Cloud or OpenCode while the offer lasts.

For ongoing use, OpenRouter’s $0.25/$1 per million tokens is the cheapest verified paid route. Self-hosting needs data-center GPUs.

## Reflection Beam FAQ

## Is Reflection Beam free?

Yes. The weights are free under Apache-2.0 on Hugging Face, and the hosted model is free for a limited time on Ollama Cloud (beam:cloud) and in OpenCode (Beam Free). Neither has announced when the free period ends.

•

## How much does Reflection Beam cost?

The model is free to download. On OpenRouter it costs $0.25 per 1M input tokens and $1 per 1M output tokens (October 19, 2026). Reflection’s own API is free to start in public beta, with no per-token prices published yet.

•

## What is Reflection AI?

Reflection AI is an AI lab that builds open models. Beam, released with open weights on October 16, 2026, is its first open-weight model: a 501B-parameter Mixture-of-Experts model with 23B active parameters and a 1M-token context.

•

## Can I run Reflection Beam on my own computer?

Not on a normal PC. The smallest official weights (NVFP4) are 273 GiB, and Reflection’s serving guide uses four NVIDIA B300 GPUs. Use Ollama Cloud, OpenCode or an API provider instead.

•

## How do I use Reflection Beam in Claude Code?

Install Ollama, sign in, and run ‘ollama launch claude –model beam:cloud’. You can also point Claude Code-style agents at Reflection’s OpenAI-compatible API with the model ID Beam-1.0-501B-A23B.
