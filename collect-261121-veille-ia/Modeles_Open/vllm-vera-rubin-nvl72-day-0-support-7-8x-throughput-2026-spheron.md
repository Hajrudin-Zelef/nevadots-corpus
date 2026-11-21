---
id: collect-261121-veille-ia/modeles-open/vllm-vera-rubin-nvl72-day-0-support-7-8x-throughput-2026-spheron
title: "vLLM Vera Rubin NVL72: Day-0 Support, 7.8x Throughput (2026) | Spheron"
domain: modeles-open
role: reference
task: article
actors: []
dates: ["2026-10-09"]
keywords: ["rubin", "vllm", "nvl72", "vera", "support", "throughput", "agentx", "minimax"]
source: https://www.spheron.network/blog/vllm-on-vera-rubin-nvl72-day-0-support-and-7-8x-throughput-2/
source_anchor: ""
source_lines: [1, 69]
section: "Modeles_Open"
---
vLLM shipped preview support for NVIDIA Vera Rubin NVL72 on October 9, 2026, in a nightly container built months before a single cloud provider sells Rubin capacity by the hour. The vLLM project's own post puts a number on it: 7.8x the throughput of GB200 NVL72 on an agentic coding benchmark called AgentX, running MiniMax M3. NVIDIA's own MLPerf Inference v6.1 preview submission already ran two different models on two different engines on the same Rubin rack: TensorRT-LLM for DeepSeek-R1, vLLM with NVIDIA Dynamo for Qwen3-VL. That split is the first concrete, hardware-validated answer to "which engine, for which workload, on next-gen silicon" anyone has published. This post covers what changed under the hood, what the 7.8x number actually measures, what the MLPerf split tells you about engine choice, and how to plan around a GPU you can't rent by the hour yet.

## TL;DR: Does vLLM Support Vera Rubin NVL72, and How Fast Is It?

Yes: vLLM added Vera Rubin NVL72 support on 9 Oct 2026, hitting 7.84x GB200's per-GPU throughput on MiniMax M3 via AgentX.

- **Headline number:** 7.84x GB200's per-GPU throughput on MiniMax M3 (AgentX); 5.18x under a 150 TPS latency cap.
- **MLPerf split:** NVIDIA ran DeepSeek-R1 on TensorRT-LLM (2.5x GB300) and Qwen3-VL on vLLM plus Dynamo (3.7x GB300), same rack.
- **Hardware:** Rubin NVL72 carries 5x the NVFP4 FLOPS and 2.4x the HBM bandwidth of GB200 NVL72, per GPU.
- **Renting it:** Spheron lists Vera Rubin as reservation/pre-order for H2 2026; H100, H200, B200 and B300 are live now. Reserve Vera Rubin →

## What Changed Day-0: Rubin-Tuned FlashInfer Kernels and MoE Locality Domains

Day-0 support for a brand-new GPU generation is rare, and it's rare for a structural reason: a serving engine's fast paths are usually written and tuned against the hardware already in front of the team, not hardware nobody can touch yet. vLLM got there this time because Rubin isn't a clean-sheet redesign at the kernel level. It shares Blackwell's kernel family, so the attention, GEMM and MoE kernels vLLM already ships for B200 and B300 carried over to Rubin largely unchanged, which is why DeepSeek, Kimi (Moonshot AI), GLM (Z.ai), and MiniMax models worked on Rubin from the first nightly build rather than needing a dedicated bring-up cycle.

What's new isn't the kernel family, it's the tuning inside it. FlashInfer 0.7.0 adds Rubin-tuned attention, GEMM and MoE kernels, and the vLLM team separately hand-tuned the MiniMax Sparse Attention (MSA) prefill kernel specifically for Rubin rather than inheriting Blackwell's tuning as-is. The other change is routing-level, not kernel-level: locality-domain-aware MoE routing, which keeps an expert's traffic inside the part of the rack with the fastest path to it instead of letting requests hop across the full NVLink fabric indiscriminately. NVIDIA, Inferact, Red Hat, and the vLLM community are credited with the Rubin bring-up.

One spec explains why locality domains matter here specifically: Rubin NVL72 carries 212 total streaming multiprocessors (SMs) per GPU, with 200 active under the default locality-domain mode. That's a wide enough compute surface that where a request's experts physically sit on the rack starts to matter as much as how fast any single SM runs, which is a newer problem than earlier NVL72 generations posed at this scale.

## The 7.8x Number: What AgentX on MiniMax M3 Actually Measures

The headline figure, repeated by the vLLM project on X: "vLLM now supports NVIDIA Vera Rubin. The early results show more than 7.8x the throughput of GB200 on MiniMax M3 on AgentX." The number is real, but it's worth knowing what AgentX is actually stressing before you apply it to your own workload.

AgentX is an open-source, multi-turn agentic coding-inference benchmark built from opt-in coding-agent traces, specifically designed to measure long sessions, shared prefixes, tool-call pauses and parallel subagents at context lengths up to 1 million tokens, according to SemiAnalysis's InferenceX glossary. That's a meaningfully different shape of load than a single-turn chatbot benchmark: a coding agent holds a session open across dozens of tool calls, reuses a large shared prefix (the codebase context) across every turn, and often runs several subagents in parallel against the same KV cache. Prefix reuse and locality-aware MoE routing both pay off disproportionately on exactly this kind of workload, which is part of why the Rubin number is as large as it is.

Here's how that 5.18x figure should actually change a capacity plan, not just a slide. Take a coding-agent product holding MiniMax M3 at a 150 TPS-per-user floor on four GB200 NVL72 GPUs during business hours, a footprint sized the way most teams size for a fixed peak-hour concurrency target rather than average load. Apply the 5.18x constant-latency gain at a similar batch size and context mix, and the same floor needs roughly one Rubin GPU instead of four. Run that same arithmetic against your own concurrency numbers, batch size and context mix before you commit to a reservation: the multiplier is measured on AgentX's specific traffic shape, and your production mix of session length, prefix reuse and tool-call frequency will move the real number up or down from there.

## vLLM vs TensorRT-LLM on Rubin: NVIDIA's Own MLPerf Submission Used Both

Every prior vLLM-vs-TensorRT-LLM comparison, including our own H100 benchmark, was written against hardware that's already shipping and already has a year or more of tuning behind both engines. NVIDIA's MLPerf Inference v6.1 preview submission for Rubin NVL72 is the first time the "which engine, for which workload" question has a hardware-validated answer on silicon nobody can rent yet, and NVIDIA's own choice is the most useful data point in it: the company ran two different model families on two different engines on the same rack, rather than standardizing on one. That same v6.1 round also introduced agentic-inference benchmarking and a trillion-parameter open-division submission; our MLPerf Inference v6.1 Vera Rubin benchmark breakdown covers the full round and what the preview-category numbers do and don't say about real cost per token.

For DeepSeek-R1, NVIDIA's submission used TensorRT-LLM, reaching up to 2.5x the throughput of GB300 NVL72. For Qwen3-VL, it used vLLM paired with the NVIDIA Dynamo serving framework, reaching up to 3.7x GB300 NVL72 throughput across three operating points (1.83x offline, 1.92x server, 3.74x interactive). Both submissions used disaggregated prefill/decode serving and large-scale expert parallelism across the MoE layers, so the architecture pattern was shared even though the engine wasn't. NVIDIA frames the overall result plainly: "Each Vera Rubin NVL72 rack delivers significantly more tokens, serves more users and generates more revenue than a GB300 NVL72 rack, while lowering cost per token," as NVIDIA's own blog post on the submission puts it.

Read plainly, that split is NVIDIA picking the engine the workload favors rather than picking a side: TensorRT-LLM's compiled, fixed-graph approach for a dense reasoning model where the graph shape is stable and worth compiling once, vLLM plus Dynamo's disaggregated flexibility for a vision-language model where request shapes and modalities vary more. That's consistent with the general-purpose split our H100 numbers already showed on current-gen hardware. If you also need SGLang in the comparison, our three-way vLLM vs TensorRT-LLM vs SGLang benchmark covers where RadixAttention's prefix-sharing advantage shows up and where it disappears.

It's also worth separating two names that get conflated in search results: TensorRT is NVIDIA's general-purpose deep learning inference compiler, used across vision models, recommenders and any neural network you want optimized for a specific GPU. TensorRT-LLM is a separate, LLM-specific library built on top of TensorRT's compiler core, adding the transformer-specific machinery, in-flight batching, paged KV cache, tensor and pipeline parallelism, that a generic TensorRT graph doesn't handle on its own. When people say "TensorRT vs TensorRT-LLM" they usually mean "should I write a custom TensorRT graph or just use the LLM-specific library," and for almost every production LLM serving case, the answer is TensorRT-LLM.

## The Hardware Behind the Number: NVFP4 FLOPS, HBM Bandwidth, NVLink, SM Count

None of the throughput gains above happen on software alone. Per vLLM's own preview post, Rubin NVL72 carries 5x the NVFP4 inference FLOPS, 2.4x the HBM bandwidth and 1.7x the bidirectional NVLink bandwidth of GB200 NVL72, plus 2 to 4x faster exponential computation for softmax, which matters directly for attention throughput at every decode step. For the full rack-level spec table (HBM4 capacity, NVLink 6 fabric bandwidth, per-GPU FLOPS at each precision), see our Vera Rubin NVL72 architecture guide rather than us repeating it here.

NVFP4 is the precision format doing most of the compute-side work in these numbers. If you haven't worked with 4-bit inference yet, our FP4 quantization on Blackwell guide covers the accuracy tradeoff and the roughly 2x TFLOPS jump over FP8 on Blackwell-family tensor cores, the same jump Rubin inherits and extends.

The pattern matches what the AgentX results already showed: the top-line multiplier is real, but it compresses hard once you anchor it to a latency bar an actual product has to hold.

## Day-0 Model Coverage: DeepSeek, Kimi, GLM, MiniMax

Because Rubin inherited Blackwell's kernel family instead of requiring new ones, the practical day-0 model list reads like a current B200/B300 deployment checklist rather than a fresh bring-up queue: DeepSeek's reasoning and MoE lines, Moonshot AI's Kimi, Z.ai's GLM, and MiniMax, including the M3 model AgentX was run against, all served on day one. Qwen3-VL also ran on Rubin, but under vLLM paired with NVIDIA Dynamo inside NVIDIA's separate MLPerf submission, not as part of vLLM's own day-0 announcement, a distinction worth keeping straight since the two results get cited together. That's a materially lower migration risk than most GPU generation launches, where the first few months are spent waiting for engine support to catch up to the hardware model by model.

That migration risk shrinks further once you separate two decisions that are easy to conflate: which engine to run, and how to split capacity across the rack. A single-node deployment and a full NVL72 rack behave differently under the same model, so settle the form factor question on its own terms before you size a reservation around it. The same separation applies if your bottleneck is context length rather than MoE routing: the AgentX and MLPerf numbers above were both measured on agentic, multi-turn and vision-language traffic, not on long-context throughput, so a workload that lives or dies on context window size needs its own evaluation rather than borrowing these figures.

## What This Means for Planning a Rubin Rental Once Spheron Capacity Opens

None of this is rentable by the hour anywhere yet, which is the gap this whole post sits inside: the engine support and the benchmark numbers exist months before the hardware access does. Spheron's pricing page lists the Rubin GPU, the chip inside Vera Rubin NVL72, as a reservation and pre-order SKU shipping H2 2026, not an on-demand hourly rate, so don't expect to spin one up on demand today. First-cohort access for genuinely new NVIDIA silicon typically goes to hyperscalers and the largest neoclouds before it reaches GPU marketplaces; our Vera Rubin NVL72 cloud availability tracker covers who has racks live right now if your timeline can't wait for a marketplace listing.

For everyone else, the practical move is to stop waiting on the engine and start on the hardware you can already rent. vLLM's Rubin kernels are extensions of its existing B200 and B300 kernels, not a separate codebase, so a workload already running MiniMax, DeepSeek, Kimi or GLM on Blackwell today should carry over to Rubin with minimal engine-side changes once capacity opens. That makes them a reasonable bridge, not a consolation prize: the precision format and much of the kernel tuning you'd use on day-one Rubin capacity is the same format and tuning available on Blackwell today.

Planning ahead for Vera Rubin NVL72 means bridging on current-gen hardware while rack capacity opens up.


## Frequently *Asked Questions*

Yes. vLLM shipped preview support for Vera Rubin NVL72 on October 9, 2026, in a nightly container (vllm/vllm-openai:cu134-nightly) built on CUDA 13.4 and PyTorch 2.15. Because Rubin shares Blackwell's kernel family, vLLM's existing B200 and B300 kernels carried over on day one, giving immediate serving support for DeepSeek, Kimi, GLM and MiniMax on Rubin hardware. NVIDIA's separate MLPerf submission also ran Qwen3-VL on vLLM with Rubin, but that was a distinct benchmark result, not part of vLLM's own day-0 model list.

vLLM uses PagedAttention and needs no compile step, so it serves a new model within minutes and runs on NVIDIA, AMD, Intel and TPU hardware. SGLang's RadixAttention is strongest when requests share a prefix, like chatbots with long system prompts. TensorRT-LLM compiles a fixed graph per model and per GPU, which costs 25 to 40 minutes upfront but typically wins on raw throughput once that engine is built. See our [full three-way benchmark](/blog/vllm-vs-tensorrt-llm-vs-sglang-benchmarks/) for the H100 numbers behind this.

TensorRT is NVIDIA's general-purpose compiler for optimizing any neural network, vision models and recommenders included, for a specific GPU. TensorRT-LLM builds on that compiler core but adds the pieces a transformer actually needs to serve well: in-flight batching, a paged KV cache, and tensor/pipeline parallelism. If you're serving an LLM in production, TensorRT-LLM is almost always the right layer, not a hand-built TensorRT graph.

Spheron lists the Rubin GPU (the chip inside Vera Rubin NVL72, also aliased as R100) as a reservation and pre-order SKU on its pricing page, shipping H2 2026, rather than an on-demand hourly rate. First-cohort access typically goes to hyperscalers and large neoclouds before marketplace platforms. Readers who need Rubin capacity immediately should check Spheron's Vera Rubin cloud availability tracker for which providers have racks live; readers who can wait can reserve Vera Rubin ahead of general availability and bridge current workloads on B200 or B300, both of which have native FP4 support and are rentable today.
