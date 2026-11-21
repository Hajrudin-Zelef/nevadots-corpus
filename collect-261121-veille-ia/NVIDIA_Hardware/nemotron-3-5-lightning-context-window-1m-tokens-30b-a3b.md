---
id: collect-261121-veille-ia/nvidia-hardware/nemotron-3-5-lightning-context-window-1m-tokens-30b-a3b
title: "Nemotron 3.5 Lightning Context Window: 1M Tokens, 30B-A3B"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: []
keywords: ["nemotron", "lightning", "model", "nvidia", "layers", "context", "token", "window"]
source: https://www.layer3labs.io/guides/nemotron-3-5-lightning-explained
source_anchor: ""
source_lines: [1, 142]
section: "NVIDIA_Hardware"
---
# Nemotron 3.5 Lightning

NVIDIA's open-weight agent model, explained in plain language.

Nemotron 3.5 Lightning is NVIDIA's open-weight model built for fast, accurate task execution in long-running agents. NVIDIA released it on August 11, 2026.

The model has 30 billion total parameters but only activates 3 billion per token. This design is called an A3B Mixture-of-Experts. You get big-model quality at a small-model speed.

This guide explains what the model is, how it differs from Nemotron 3, who should use it, and how to get access. It sticks to NVIDIA's published facts, and points you to the model card to verify numbers.

## Context Window and Benchmark Performance

Nemotron 3.5 Lightning supports a context window of up to 1 million tokens. The hybrid architecture combines Mamba-2 layers, Mixture-of-Experts (MoE) layers, and a few Attention layers to keep long-context processing practical at that 1 million token scale.

On general capability, Artificial Analysis reports a score of 24 on its Intelligence Index, level with gpt-oss-120b at about a quarter of the total parameters. NVIDIA also states that Lightning finishes 10,000 tasks about 30 percent faster than Qwen3.6-35B at similar accuracy.

There is no head-to-head benchmark of Nemotron 3.5 Lightning against Claude, so neither model can be called better. Task mix, hardware, and speed needs decide the pick. Nemotron 3.5 Lightning vs Claude covers deployment details, and Nemotron 3.5 Lightning alternatives covers other options.

## What Is Nemotron 3.5 Lightning?

Nemotron 3.5 Lightning is an open-weight language model from NVIDIA. It is tuned for agentic and coding work that runs for a long time.

The full model id is NVIDIA-Nemotron-3.5-Lightning-30B-A3B. The A3B tag means 30 billion total parameters with 3 billion active per token.

A Mixture-of-Experts model holds many small expert networks. It routes each token to only a few experts. That keeps compute low while total knowledge stays high.

NVIDIA positions it for specialized task execution inside long-running agents. Think coding steps, tool calls, and multi-step jobs, not open-ended chat alone.

The name signals the goal. Lightning stands for speed, and the whole design serves fast, repeatable work at scale.

NVIDIA pre-trained the model on more than 20 trillion tokens. That large training set is why a small active model still knows a lot.

## How the Architecture Works

The model uses a hybrid architecture. It interleaves Mamba-2 layers, MoE layers, and a few Attention layers.

Mamba-2 layers are a state-space design. They process long sequences fast without the heavy cost of full attention at every layer.

The MoE layers add capacity without adding runtime cost, because only a few experts fire per token. The select Attention layers keep sharp, precise recall where it matters most.

NVIDIA pre-trained the model on more than 20 trillion tokens. Weights ship in two checkpoints: NVFP4, a 4-bit format, and BF16, a 16-bit format.

The hybrid mix is the whole trick. Each layer type does what it is best at, so the model stays both fast and accurate.

Pure attention models slow down as input grows. By leaning on Mamba-2 layers, this design keeps long-context work practical at the 1 million token scale.

## The 1 Million Token Context Window

Nemotron 3.5 Lightning supports a context window of up to 1 million tokens. That is a large working memory for a single request.

A big context lets an agent hold whole codebases, long logs, or many documents at once. It reduces the need to chunk and re-fetch data mid-task.

This pairs well with long-running agents. The model can keep track of a task across many steps without losing earlier detail.

## Who Makes It and What Shipped with It

NVIDIA makes Nemotron 3.5 Lightning. It is part of the company's Nemotron open-model family.

The license is OpenMDW-1.1. NVIDIA released the open weights, and also published the training data and training recipes.

NVIDIA shipped a separate product alongside the model: NeMo Switchyard, a model router. The router picks the right model for a request. It is not the same thing as the model itself. In October 2026 the same team shipped a larger sibling, Nemotron 3.5 Super VL, NVIDIA's open vision-language model for agents; the launch post was bylined by Kari Briski, Nvidia's VP of generative AI software for enterprise.

## How It Differs from Nemotron 3

Nemotron 3.5 Lightning is a newer, speed-focused release than the Nemotron 3 family from December 2025. Both use a hybrid Mamba-Transformer MoE design and a 1 million token context.

Nemotron 3 shipped as a size range: Nano at 3B, Super at 12B, and Ultra at 55B active parameters. Nemotron 3.5 Lightning is one 30B-A3B model with 3B active.

The license also changed. Nemotron 3 used the NVIDIA Open Model License. Nemotron 3.5 Lightning uses OpenMDW-1.1, with data and recipes released too.

The headline shift is speed and agent focus. NVIDIA tuned Lightning for fast, repeated task execution rather than a broad size ladder.

## Who It Is For

Nemotron 3.5 Lightning fits teams building agents that run many steps and call many tools. Coding agents are a core use case.

It suits builders who want open weights they can host and control. You can run it on your own hardware and keep data in house.

In our work evaluating open-weight models for clients, the recurring failure mode is picking a model for a benchmark score, then finding it too slow or costly at real agent volume. A fast MoE like this one is aimed straight at that gap.

It is a weaker fit if you only need occasional short chats. A smaller hosted model may cost less for light, simple use.

Researchers and regulated teams also benefit. Open weights, released data, and published recipes let them audit and adapt the model with confidence.

## How It Compares to Other Open Models

Nemotron 3.5 Lightning competes with other open models built for coding and agents. NVIDIA and Artificial Analysis name two direct comparators.

The first is gpt-oss-120b, OpenAI's open model. Artificial Analysis reports Lightning scoring 24 on its Intelligence Index, level with gpt-oss-120b, at about a quarter of the total parameters.

The second is Qwen, from Alibaba. NVIDIA cites Qwen3.6-35B and claims Lightning finishes 10,000 tasks about 30 percent faster at similar accuracy.

Other open options include DeepSeek and Llama-class models. The right choice depends on your task mix, your hardware, and your speed needs, so test rather than assume.

## NVFP4 and BF16 Checkpoints

NVIDIA ships the model in two checkpoints, and the difference is precision. NVFP4 is a 4-bit format, and BF16 is a 16-bit format.

The BF16 checkpoint keeps full precision and is the one behind the published benchmark scores. It needs more GPU memory to run.

The NVFP4 checkpoint uses 4-bit math to shrink memory use. That lets the model run on smaller or cheaper hardware with little quality loss.

Pick BF16 when you want top accuracy and have the memory. Pick NVFP4 when you want to run leaner or on tighter hardware.

## How to Access Nemotron 3.5 Lightning

You can access the model in two main ways: a hosted endpoint or a local install. Pick based on control, cost, and scale.

For a hosted API, use build.nvidia.com. You call the endpoint and pay per token, with no hardware to manage.

To self-host, download the weights from Hugging Face. You can run it on RTX or DGX hardware, or through open serving stacks like vLLM.

The open license makes both paths easy to try. You can prototype on the hosted endpoint, then move the same weights in house once volume grows.

Always confirm current specs, benchmarks, and access terms on the official model card before you build on them.

## What you need to run Nemotron 3.5 Lightning yourself

Nemotron 3.5 Lightning needs real memory, but it is within reach of a high-end workstation or a couple of professional GPUs — and many teams simply rent instead of buying. Match the path below to whether you want to own the hardware or pay by the hour.

| Path | What it is | Best for | Get started | 
|---|---|---|---|
| Call the hosted API | Use Nemotron 3.5 Lightning as a pay-per-token API — zero hardware | Most teams; getting started | OpenRouter | 
| Rent GPUs by the hour | Spin up an H100 / A100 for a few dollars an hour | Flexible self-hosting without buying cards | RunPod | 
| Local on unified memory | One Mac with enough unified memory to hold a 4-bit quant | A single quiet on-prem box | Apple Mac Studio (M4 Max, 128GB) | 
| Local on a workstation GPU | One 48GB pro card, or two 24GB consumer cards | Power users who want hardware they own | NVIDIA RTX 6000 Ada (48GB) | 

To put Nemotron 3.5 Lightning to work once it is live, connect a coding client like Cursor (via OpenRouter) or a local runner such as Ollama.

## Frequently Asked Questions

- Nemotron 3.5 Lightning is NVIDIA's open-weight model for long-running agents, released August 11, 2026. It has 30 billion total parameters with 3 billion active per token, and supports up to a 1 million token context.
- The model ships with open weights under the OpenMDW-1.1 license. NVIDIA also released the training data and training recipes, and the weights are available on Hugging Face.
- Nemotron 3.5 Lightning is a single 30B-A3B model tuned for speed and agent tasks, while Nemotron 3 shipped as Nano, Super, and Ultra sizes. Lightning also uses the OpenMDW-1.1 license instead of the NVIDIA Open Model License.
- A3B means the model has about 3 billion active parameters per token, even though it holds 30 billion total. This Mixture-of-Experts design routes each token to a few experts, so it runs faster than a dense model of the same total size.
- You can use the hosted API on build.nvidia.com or self-host the weights from Hugging Face. Self-hosting runs on RTX or DGX hardware and open serving stacks like vLLM.

### Put Nemotron 3.5 Lightning to work

Not sure if an open-weight agent model fits your stack? At Layer3Labs, we help teams test and deploy models like Nemotron 3.5 Lightning without the guesswork. Book a call to map the right path.

Book a Call
