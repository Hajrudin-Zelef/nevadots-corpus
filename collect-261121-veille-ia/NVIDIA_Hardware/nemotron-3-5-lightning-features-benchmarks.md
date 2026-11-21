---
id: collect-261121-veille-ia/nvidia-hardware/nemotron-3-5-lightning-features-benchmarks
title: "Nemotron 3.5 Lightning: Features & Benchmarks"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: ["2026-08-11"]
keywords: ["lightning", "nemotron", "model", "nvidia", "frontier", "models", "open", "parameters"]
source: https://www.datacamp.com/blog/nemotron-3-5-lightning
source_anchor: ""
source_lines: [1, 288]
section: "NVIDIA_Hardware"
---
Most conversation about agentic AI fixates on the frontier reasoning models doing the planning.

NVIDIA's argument with Nemotron 3.5 Lightning is that the frontier model is the wrong tool for most of what an agent actually does. L

ong-running agents burn most of their token budget on tool calls, validating outputs, and delegating to subagents, and running a frontier model for every one of those steps is expensive and slow.

Nemotron 3.5 Lightning, released August 11, 2026, is an open 30B mixture-of-experts (MoE) model with 3B active parameters aimed squarely at that execution layer.

NVIDIA claims it wins the accuracy-versus-speed Pareto frontier on the Artificial Analysis Intelligence Index, and reports up to 4x output speed of similar-sized models. It ships under the permissive OpenMDW-1.1 license with weights, training data, and recipes released.

In this article, I'll cover everything new with Nemotron 3.5 Lightning, looking at its features and exploring the benchmarks. If you want to see how a similar-sized MoE model behaves in practice, our tutorial on fine-tuning Qwen3.6 on a medical Q&A dataset walks through the workflow end to end.

## Nemotron 3.5 Lightning in a Nutshell

- Nemotron 3.5 Lightning is an open 30B MoE model with 3B active parameters, released August 11, 2026, built for the high-volume execution layer of always-on agents.
- NVIDIA claims up to 4x output speed of similar-sized models and 10,000 tasks completed 30% faster than Qwen3.6-35B at similar accuracy.
- Key benchmarks (BF16): SWE-bench Verified 51.56, PinchBench 85.37, GPQA Diamond 75.44, MMLU Pro 81.94.
- It ships under OpenMDW-1.1 with weights, data, and recipes, and runs on hardware from Jetson to DGX Spark to data centers.
- NeMo Switchyard routes execution work to Lightning and planning to frontier models like Nemotron 3 Ultra.

## What Is Nemotron 3.5 Lightning?

Nemotron 3.5 Lightning is the smallest member of NVIDIA's Nemotron 3 open model family, built specifically for high-volume, low-latency execution in autonomous agents.

It uses a Mamba-2 + MoE + attention hybrid architecture with 30B total parameters and 3B active per token, and supports a context length of up to 1M tokens.

The MoE design is what makes it fast.

A router sends each token to only a few of its many experts, so a fraction of the parameters run per token.

You get the capacity of a larger dense model at the compute cost of a small one, which is the same trade-off Qwen3.6-35B-A3B makes with its 3B active parameters.

The big claim from NVIDIA is positional rather than a single score: on the Artificial Analysis Intelligence Index, which combines 9 evaluations across agentic tasks, coding, scientific reasoning, and general intelligence, Lightning sits on the accuracy-speed Pareto frontier for small open models.

In plain terms, nothing in its size class is both more accurate and faster at the same time.

## What's New With Nemotron 3.5 Lightning?

The features here are all in service of one goal: running the routine work of an agent quickly without the model falling over on accuracy.

Here is what you can actually do with it.

### Run capable agents on local hardware

Because the model is small enough to deploy on an NVIDIA DGX Spark, GeForce RTX 5090, or Jetson, you can run agentic workloads on a desktop without a data center.

NVIDIA worked with EXO Labs to profile Lightning on DGX Spark, and reports it sits on the Pareto frontier for small open models on the EXO Labs local.ai leaderboard.

For practitioners, this means an always-on agent handling tool calls and result validation can run on hardware you already own.

You can also serve it through LM Studio, llama.cpp, Ollama, and Unsloth, so the local tooling story is already mature at launch.

### Route execution work down and planning up with NeMo Switchyard

NVIDIA is releasing NeMo Switchyard alongside the model, a library that routes each request to the most efficient model that can handle it.

Switchyard exposes Nemotron 3.5 Lightning as a routing target next to your open and closed models, so planning routes up to a frontier model like Nemotron 3 Ultra while execution routes down to Lightning.

The practical payoff is token efficiency.

Instead of paying frontier-model rates to run a `git pull`, validate a tool output, or format a result, those calls land on the cheap model, and the expensive model is reserved for the hard planning steps that dominate quality but not volume.

### Customize it out of the box

Small models fine-tune faster, cheaper, and on more modest hardware than large ones, and Lightning is built to be adapted to a specific job.

NVIDIA released the weights, training data, and recipes under OpenMDW-1.1, and you can fine-tune with LoRA or full SFT using NeMo Automodel and NeMo Megatron Bridge, or run reinforcement learning with NeMo RL and NeMo Gym.

The release also includes Nemotron-RL Agentic Terminal Pivot, an open agentic RL dataset used to train some of the coding-agent behavior.

One community-facing claim I think is worth flagging here.

MindStudio reports partner fine-tuning done in under 3 hours for roughly $100, which is the kind of adaptation cost that only makes sense with a model this size.

### Speculative decoding for higher throughput

Lightning generates multiple tokens per step through speculative decoding, where a draft model proposes tokens that are then efficiently reviewed.

It underwent a dedicated pretraining stage to bake multi-token prediction (MTP) into the model, followed by an MTP-boosting phase, the same approach used in Nemotron 3 Super and Ultra.

Beyond MTP, NVIDIA ships two draft models.

DSpark is recommended for DGX Spark and low-concurrency data center workloads, while MTP suits medium-to-high concurrency, and a DFlash draft model is also provided so you can benchmark which performs best for your serving pattern.

## Nemotron 3.5 Lightning Benchmarks

NVIDIA reports scores for both the BF16 and NVFP4 checkpoints, and the two track closely across most tasks, which matters because NVFP4 is the quantized checkpoint you would actually deploy. The numbers below are from NVIDIA's own model card and launch blog. No independent third-party reproduction was available at the time of writing, so treat these as vendor-published.

### SWE-bench Verified

Lightning scores 51.56 (BF16) and 52.80 (NVFP4) on SWE-bench Verified, a benchmark that measures whether a model can resolve real GitHub issues by producing a working code patch. For a 30B MoE with 3B active parameters, breaking 50% on this is a solid result and directly supports the coding-agent use case.

SWE-bench Multilingual comes in lower at 39.33 (BF16), which is expected given the harder multilingual codebase spread.

### PinchBench and task efficiency

On PinchBench, Lightning reaches 85.37 (BF16) and 83.43 (NVFP4), and OpenRouter reports it completes 10,000 tasks 30% faster than Qwen3.6-35B at similar accuracy. 

This is the benchmark that best captures the model's actual pitch, since agent efficiency comes down to how quickly a model finishes useful work, not raw token generation speed.

The Qwen3.6 comparison is worth dwelling on. When we reviewed Qwen3.6-35B-A3B, we noted its 3B active parameters and strong SWE-bench and Terminal-Bench performance, so NVIDIA picking it as the reference point is a like-for-like fight in the same size class.

### GPQA Diamond and reasoning

Lightning scores 75.44 (BF16) on GPQA Diamond with no tools, a set of graduate-level science questions written to be hard for non-experts even with web access.

That is a strong number for a small model, though the model card is explicit that Lightning is not positioned as a reasoning engine.

Humanity's Last Exam (text-only, no tools) tells the other side of the story at 11.72 (BF16), and SciCode lands at 32.60.

These lower scores are consistent with NVIDIA's framing: this is an execution model, and the hard open-ended reasoning is meant to be routed to a frontier model instead.

### Agentic and instruction-following

Terminal-Bench 2.1 comes in at 24.58 (BF16) and BrowseComp at 36.97, while IFBench (loose) reaches 71.88, showing the model follows structured instructions reliably.

General knowledge holds up too, with MMLU Pro at 81.94 (BF16) and 81.62 (NVFP4), a negligible gap that confirms the quantized checkpoint is safe to deploy.

## Nemotron 3.5 Lightning vs Qwen3.6-35B-A3B

Both are MoE models with 3B active parameters aimed at coding and agentic workloads, so a side-by-side is the cleanest way to see where Lightning fits.

| Attribute | Nemotron 3.5 Lightning | Qwen3.6-35B-A3B | 
|---|---|---|
| Total / active params | 30B / 3B | 35B / 3B | 
| Context window | Up to 1M tokens | 262K tokens | 
| SWE-bench Verified | 51.56 (BF16) | Strong (per our review) | 
| License | OpenMDW-1.1 (open) | Open | 
| Positioning | Execution layer of agents | Coding, reasoning, long context | 

## Nemotron 3.5 Lightning Pricing and Availability

You can try Nemotron 3.5 Lightning on build.nvidia.com or through OpenRouter, and download the weights from Hugging Face and ModelScope. The model exposes OpenAI-compatible endpoints via NVIDIA NIM, including `/v1/chat/completions`, `/v1/completions`, and `/v1/responses`, so it drops into existing agentic pipelines without rewrites.

Hosted pricing varies by provider:

- **DeepInfra:** $0.05 per 1M input tokens and $0.20 per 1M output tokens, available day zero with no GPU provisioning.
- **OpenRouter:** a free tier listed at $0 input and $0 output, but note that while the route supports the 1M token context window, it enforces a 65,536 max-token cap on the output generation.
- **Self-hosted:** free under OpenMDW-1.1, deployable via vLLM, SGLang, and TensorRT-LLM.

That OpenRouter cap is worth noting. The 1M context length is a model capability, but the practical limit you get depends on the provider and route, so check the endpoint before assuming you have the full window.

## Final Thoughts

Nemotron 3.5 Lightning is NVIDIA making a specific bet: that the future of production agents is a system of models, where a cheap execution model does the volume and a frontier model does the planning. Releasing NeMo Switchyard in the same launch is the tell, because the model only makes sense if you can route work to it intelligently.

What I find most useful is the honesty about scope. NVIDIA is not pretending this is a reasoning model, and the benchmarks back that up: strong on SWE-bench Verified (51.56) and PinchBench (85.37), modest on Humanity's Last Exam (11.72). If your workload is tool calls, validation, and formatting on always-on agents, that trade is exactly right.

The open release under OpenMDW-1.1 with weights, data, and recipes, plus a checkpoint small enough to run on a DGX Spark, makes this genuinely useful to teams who want to fine-tune and self-host rather than pay per token. The main caveat is that all current benchmark numbers are vendor-published, so I would want independent reproduction before treating the 4x speed and 30% faster-completion claims as settled.

If you want to build the fine-tuning skills to adapt a model like this to your own workload, our tutorial on fine-tuning Qwen3.6 on a medical Q&A dataset covers the QLoRA workflow on a comparable MoE model step by step.

Most conversation about agentic AI fixates on the frontier reasoning models doing the planning.

NVIDIA's argument with Nemotron 3.5 Lightning is that the frontier model is the wrong tool for most of what an agent actually does. L

ong-running agents burn most of their token budget on tool calls, validating outputs, and delegating to subagents, and running a frontier model for every one of those steps is expensive and slow.

Nemotron 3.5 Lightning, released August 11, 2026, is an open 30B mixture-of-experts (MoE) model with 3B active parameters aimed squarely at that execution layer.

NVIDIA claims it wins the accuracy-versus-speed Pareto frontier on the Artificial Analysis Intelligence Index, and reports up to 4x output speed of similar-sized models. It ships under the permissive OpenMDW-1.1 license with weights, training data, and recipes released.

In this article, I'll cover everything new with Nemotron 3.5 Lightning, looking at its features and exploring the benchmarks. If you want to see how a similar-sized MoE model behaves in practice, our tutorial on fine-tuning Qwen3.6 on a medical Q&A dataset walks through the workflow end to end.

## Nemotron 3.5 Lightning in a Nutshell

## What Is Nemotron 3.5 Lightning?

Nemotron 3.5 Lightning is the smallest member of NVIDIA's Nemotron 3 open model family, built specifically for high-volume, low-latency execution in autonomous agents.

It uses a Mamba-2 + MoE + attention hybrid architecture with 30B total parameters and 3B active per token, and supports a context length of up to 1M tokens.

The MoE design is what makes it fast.

A router sends each token to only a few of its many experts, so a fraction of the parameters run per token.

You get the capacity of a larger dense model at the compute cost of a small one, which is the same trade-off Qwen3.6-35B-A3B makes with its 3B active parameters.

The big claim from NVIDIA is positional rather than a single score: on the Artificial Analysis Intelligence Index, which combines 9 evaluations across agentic tasks, coding, scientific reasoning, and general intelligence, Lightning sits on the accuracy-speed Pareto frontier for small open models.

In plain terms, nothing in its size class is both more accurate and faster at the same time.

## What's New With Nemotron 3.5 Lightning?

The features here are all in service of one goal: running the routine work of an agent quickly without the model falling over on accuracy.

Here is what you can actually do with it.

## Run capable agents on local hardware

Because the model is small enough to deploy on an NVIDIA DGX Spark, GeForce RTX 5090, or Jetson, you can run agentic workloads on a desktop without a data center.

NVIDIA worked with EXO Labs to profile Lightning on DGX Spark, and reports it sits on the Pareto frontier for small open models on the EXO Labs local.ai leaderboard.

For practitioners, this means an always-on agent handling tool calls and result validation can run on hardware you already own.

You can also serve it through LM Studio, llama.cpp, Ollama, and Unsloth, so the local tooling story is already mature at launch.

## Route execution work down and planning up with NeMo Switchyard

NVIDIA is releasing NeMo Switchyard alongside the model, a library that routes each request to the most efficient model that can handle it.

Switchyard exposes Nemotron 3.5 Lightning as a routing target next to your open and closed models, so planning routes up to a frontier model like Nemotron 3 Ultra while execution routes down to Lightning.

The practical payoff is token efficiency.

Instead of paying frontier-model rates to run a

`git pull`, validate a tool output, or format a result, those calls land on the cheap model, and the expensive model is reserved for the hard planning steps that dominate quality but not volume.
## Customize it out of the box

Small models fine-tune faster, cheaper, and on more modest hardware than large ones, and Lightning is built to be adapted to a specific job.

NVIDIA released the weights, training data, and recipes under OpenMDW-1.1, and you can fine-tune with LoRA or full SFT using NeMo Automodel and NeMo Megatron Bridge, or run reinforcement learning with NeMo RL and NeMo Gym.

The release also includes Nemotron-RL Agentic Terminal Pivot, an open agentic RL dataset used to train some of the coding-agent behavior.

One community-facing claim I think is worth flagging here.

MindStudio reports partner fine-tuning done in under 3 hours for roughly $100, which is the kind of adaptation cost that only makes sense with a model this size.

## Speculative decoding for higher throughput

Lightning generates multiple tokens per step through speculative decoding, where a draft model proposes tokens that are then efficiently reviewed.

It underwent a dedicated pretraining stage to bake multi-token prediction (MTP) into the model, followed by an MTP-boosting phase, the same approach used in Nemotron 3 Super and Ultra.

Beyond MTP, NVIDIA ships two draft models.

DSpark is recommended for DGX Spark and low-concurrency data center workloads, while MTP suits medium-to-high concurrency, and a DFlash draft model is also provided so you can benchmark which performs best for your serving pattern.

## Nemotron 3.5 Lightning Benchmarks

NVIDIA reports scores for both the BF16 and NVFP4 checkpoints, and the two track closely across most tasks, which matters because NVFP4 is the quantized checkpoint you would actually deploy. The numbers below are from NVIDIA's own model card and launch blog. No independent third-party reproduction was available at the time of writing, so treat these as vendor-published.

## SWE-bench Verified

Lightning scores 51.56 (BF16) and 52.80 (NVFP4) on SWE-bench Verified, a benchmark that measures whether a model can resolve real GitHub issues by producing a working code patch. For a 30B MoE with 3B active parameters, breaking 50% on this is a solid result and directly supports the coding-agent use case.

SWE-bench Multilingual comes in lower at 39.33 (BF16), which is expected given the harder multilingual codebase spread.

## PinchBench and task efficiency

On PinchBench, Lightning reaches 85.37 (BF16) and 83.43 (NVFP4), and OpenRouter reports it completes 10,000 tasks 30% faster than Qwen3.6-35B at similar accuracy.

This is the benchmark that best captures the model's actual pitch, since agent efficiency comes down to how quickly a model finishes useful work, not raw token generation speed.

The Qwen3.6 comparison is worth dwelling on. When we reviewed Qwen3.6-35B-A3B, we noted its 3B active parameters and strong SWE-bench and Terminal-Bench performance, so NVIDIA picking it as the reference point is a like-for-like fight in the same size class.

## GPQA Diamond and reasoning

Lightning scores 75.44 (BF16) on GPQA Diamond with no tools, a set of graduate-level science questions written to be hard for non-experts even with web access.

That is a strong number for a small model, though the model card is explicit that Lightning is not positioned as a reasoning engine.

Humanity's Last Exam (text-only, no tools) tells the other side of the story at 11.72 (BF16), and SciCode lands at 32.60.

These lower scores are consistent with NVIDIA's framing: this is an execution model, and the hard open-ended reasoning is meant to be routed to a frontier model instead.

## Agentic and instruction-following

Terminal-Bench 2.1 comes in at 24.58 (BF16) and BrowseComp at 36.97, while IFBench (loose) reaches 71.88, showing the model follows structured instructions reliably.

General knowledge holds up too, with MMLU Pro at 81.94 (BF16) and 81.62 (NVFP4), a negligible gap that confirms the quantized checkpoint is safe to deploy.

## Nemotron 3.5 Lightning vs Qwen3.6-35B-A3B

Both are MoE models with 3B active parameters aimed at coding and agentic workloads, so a side-by-side is the cleanest way to see where Lightning fits.

## Nemotron 3.5 Lightning Pricing and Availability

You can try Nemotron 3.5 Lightning on build.nvidia.com or through OpenRouter, and download the weights from Hugging Face and ModelScope. The model exposes OpenAI-compatible endpoints via NVIDIA NIM, including

`/v1/chat/completions`,`/v1/completions`, and`/v1/responses`, so it drops into existing agentic pipelines without rewrites.
Hosted pricing varies by provider:

DeepInfra:$0.05 per 1M input tokens and $0.20 per 1M output tokens, available day zero with no GPU provisioning.OpenRouter:a free tier listed at $0 input and $0 output, but note that while the route supports the 1M token context window, it enforces a 65,536 max-token cap on the output generation.Self-hosted:free under OpenMDW-1.1, deployable via vLLM, SGLang, and TensorRT-LLM.
That OpenRouter cap is worth noting. The 1M context length is a model capability, but the practical limit you get depends on the provider and route, so check the endpoint before assuming you have the full window.

## Final Thoughts

Nemotron 3.5 Lightning is NVIDIA making a specific bet: that the future of production agents is a system of models, where a cheap execution model does the volume and a frontier model does the planning. Releasing NeMo Switchyard in the same launch is the tell, because the model only makes sense if you can route work to it intelligently.

What I find most useful is the honesty about scope. NVIDIA is not pretending this is a reasoning model, and the benchmarks back that up: strong on SWE-bench Verified (51.56) and PinchBench (85.37), modest on Humanity's Last Exam (11.72). If your workload is tool calls, validation, and formatting on always-on agents, that trade is exactly right.

The open release under OpenMDW-1.1 with weights, data, and recipes, plus a checkpoint small enough to run on a DGX Spark, makes this genuinely useful to teams who want to fine-tune and self-host rather than pay per token. The main caveat is that all current benchmark numbers are vendor-published, so I would want independent reproduction before treating the 4x speed and 30% faster-completion claims as settled.

If you want to build the fine-tuning skills to adapt a model like this to your own workload, our tutorial on fine-tuning Qwen3.6 on a medical Q&A dataset covers the QLoRA workflow on a comparable MoE model step by step.
