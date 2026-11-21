---
id: collect-261121-veille-ia/nvidia-hardware/nemotron-3-5-super-vl-explained-specs-and-benchmarks
title: "Nemotron 3.5 Super VL Explained: Specs and Benchmarks"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: ["2026-10-15"]
keywords: ["nvidia", "model", "nemotron", "super", "tokens", "text", "billion", "parameters"]
source: https://www.layer3labs.io/guides/nemotron-3-5-super-vl-explained
source_anchor: ""
source_lines: [1, 110]
section: "NVIDIA_Hardware"
---
# Nemotron 3.5 Super VL

What NVIDIA published for Super VL on October 15, 2026, plus OpenRouter prices and the 24GB Bolt sibling.

Nemotron 3.5 Super VL is NVIDIA's open-weight 120-billion-parameter vision-language model for agents, released on October 15, 2026. The model reads text, images, and short video while activating 12 billion parameters per token.

At Layer3Labs, we build custom agents and document automation for business workflows, so we read a launch like this for what it changes in model cost and document handling.

NVIDIA states that this architecture performs close to its largest reasoning model at roughly one-fifth the parameter size.

## What Nemotron 3.5 Super VL Is

Nemotron 3.5 Super VL is an open-weight vision-language model with 120 billion total parameters and 12 billion active parameters per token, released by NVIDIA on October 15, 2026. Kari Briski announced the release in an NVIDIA blog post as an agentic system designed to process text, image, and video data. NVIDIA reports that the architecture performs close to its largest reasoning model at roughly one-fifth the parameter count.

The official Specifications section on build.nvidia.com lists exactly 123,611,012,096 parameters. Hugging Face lists model version v1.0, published on October 15, 2026. Nemotron 3.5 Super VL supports function calling and structured output, so an agent built on it can call tools. Reasoning is configurable through the `enable_thinking` parameter in the chat template.

- Release date: October 15, 2026 on build.nvidia.com and Hugging Face.
- Model parameters: 120 billion total parameters with 12 billion active per token in an A12B configuration.
- Supported modalities: Text, JPEG and PNG images, and MP4 video up to two minutes as inputs, with text-only outputs.
- Context length: Up to 1 million tokens, though many hosts serve 262,144 tokens.
- Licensing terms: OpenMDW License Agreement version 1.1 (OpenMDW-1.1); NVIDIA also releases the training datasets and recipes.
- Distribution points: NVIDIA hosted trial Application Programming Interface (API), Hugging Face repositories, OpenRouter endpoints, and NVIDIA NIM containers.

## Architecture and Multimodal Inputs

The Nemotron 3.5 Super VL architecture uses a hybrid Mixture-of-Experts (MoE) design that interleaves Mamba-2 state-space layers, Transformer attention layers, and routed feed-forward blocks. Each MoE layer contains 512 routed experts. A shared Multi-Token Prediction (MTP) head provides native speculative decoding during generation.

Visual input processing relies on a C-RADIO vision encoder coupled with a specialized vision-language connector. The model accepts text prompts, Joint Photographic Experts Group (JPEG) and Portable Network Graphics (PNG) images, and Moving Picture Experts Group-4 (MP4) video files up to two minutes in duration. Document pages enter the system as page images accompanied by text, while the model generates text output exclusively.

NVIDIA notes that the model processes Optical Character Recognition (OCR) tasks, data tables, charts, full web pages, and Portable Document Format (PDF) files. The context window scales up to 1 million tokens across text, visual frames, chat history, and tool execution outputs. Long-context pretraining reached 1 million tokens through a 35-billion-token extension stage and a 9-billion-token recovery stage. Many hosts serve a 262,144-token window, while CoreWeave on OpenRouter serves the full 1,048,576 tokens.

Pretraining and continued pretraining covered roughly 26 trillion tokens from foundation datasets collected between 2013 and December 2025. This foundation included 100 billion tokens of Indic language continued pretraining and 67 billion tokens of software engineering data. Post-training added about 3.8 million samples plus Reinforcement Learning (RL) across dozens of environments. Post-training covers English, French, German, Italian, Japanese, Spanish, and Chinese.

## Benchmark Results for Nemotron 3.5 Super VL

NVIDIA published benchmark evaluations comparing Nemotron 3.5 Super VL across 16-bit Brain Floating Point (BF16) and 4-bit Floating Point (NVFP4) checkpoints. All scores in the table are vendor-reported and come from the Hugging Face model card. Evaluation harnesses differ from earlier releases, meaning scores cannot be compared directly against the Nemotron 3 Super model card without accounting for benchmark version changes.

NVIDIA claims the model delivers up to 5x faster throughput than the next best open agentic model in its class. NVIDIA also reports that serving the model through NVIDIA Dynamo supports up to 8x more users with 85 percent lower latency. Independent third-party data from Artificial Analysis reported by Let's Data Science logged an Intelligence Index score of 26.5 and roughly 630 output tokens per second on 100,000-token prompts, measured on a pre-release endpoint.

## OpenMDW License and Checkpoint Weights

Nemotron 3.5 Super VL is licensed under the OpenMDW License Agreement version 1.1, and NVIDIA releases the weights alongside open training datasets and recipes. This marks a shift from Nemotron 3 Super, which NVIDIA released under the NVIDIA Nemotron Open Model License. Users can review the full legal terms on the OpenMDW-1.1 license page.

NVIDIA hosts weights across several Hugging Face repositories. The BF16 base checkpoint is stored under nvidia/NVIDIA-Nemotron-3.5-Super-VL-120B-A12B-BF16, which lists 20 quantized variants. The NVFP4 repository is nvidia/NVIDIA-Nemotron-3.5-Super-VL-120B-A12B-NVFP4, alongside a speculative drafter repository named nvidia/NVIDIA-Nemotron-3.5-Super-VL-120B-A12B-NVFP4-LiLiCorr for DGX Spark systems.

NVIDIA created the NVFP4 checkpoint using NVIDIA Model Optimizer, recovering accuracy through Quantization-Aware Distillation (QAD) from the BF16 base. In this 4-bit format, the vision encoder, connector, Mamba convolutions, and input embeddings stay in BF16 precision, while the MoE router operates in 32-bit Floating Point (FP32). Inference runs at W4A4 precision on Blackwell hardware and W4A16 precision on non-Blackwell hardware.

## Where to Run Nemotron 3.5 Super VL and What It Costs

Developers can run Nemotron 3.5 Super VL through cloud Application Programming Interface endpoints or self-hosted enterprise infrastructure. Hosted API trial access is available on build.nvidia.com, subject to the NVIDIA API Trial Terms of Service. Commercial API endpoints are available on OpenRouter under the model identifier nvidia/nemotron-3.5-super-vl, with list pricing from $0.10 per million input tokens and $0.40 per million output tokens as of October 2026.

OpenRouter provider rates vary by backend configuration as of October 2026. Crusoe lists BF16 endpoints at $0.10 per million input and $0.40 per million output tokens. DeepInfra charges $0.10/$0.40 for FP4, while Together charges $0.15/$0.50. CoreWeave serves NVFP4 at $0.15/$0.50 with a full 1,048,576 token context window, and Venice lists FP4 endpoints at $0.25/$1.25. OpenRouter also lists a free variant under nvidia/nemotron-3.5-super-vl:free.

For self-hosting, supported runtimes include vLLM, SGLang, TensorRT-LLM, NVIDIA NIM, NVIDIA Dynamo, llama.cpp, Unsloth, and Docker Model Runner. Baseten also provides managed Hugging Face inference deployments. NVIDIA lists hardware configurations including single DGX Spark, single H200, dual H100, or single GB200 systems, plus RTX Spark and RTX Pro 6000 machines for local use.

No official Video Random Access Memory (VRAM) capacity requirement is published for Nemotron 3.5 Super VL. Measure memory use on your target hardware before sizing a production cluster; our guide to running Nemotron locally covers local setup.

## Nemotron 3.5 Bolt as a Compact Sibling

Nemotron 3.5 Bolt is a 74-billion-parameter multimodal model packaged to operate within a 24-gigabyte memory footprint on consumer workstations. NVIDIA announced the model alongside Super VL on October 15, 2026, and published files on Hugging Face. Digital Today called the release Volt, but NVIDIA's blog and Hugging Face card use the name Bolt.

NVIDIA compressed Bolt from the Super VL BF16 base using Star Elastic, which cuts it to 74 billion total and 9 billion active parameters per token. Expert routing top-K drops from 22 to 9. The model pairs a Mamba-2 Transformer hybrid LatentMoE with a C-RADIO v4-H vision encoder through a Multilayer Perceptron (MLP) projector. It accepts text, images, and MP4 video up to two minutes, outputs text, and supports a context window of up to 256K tokens.

The 24GB deployment package relies on 2-bit quantization, running routed experts at IQ2-XXS precision (2.0625 bits per element), Mamba layers at NVFP4, and embedding layers at Q8_0. Testing hardware includes GeForce RTX 5090 cards, DGX Spark systems, and RTX Spark laptops. Supported execution engines include llama.cpp, Ollama, LM Studio, Unsloth, and vLLM. As of October 2026, Bolt is not accessible on OpenRouter.

On vendor-reported benchmarks, Bolt scores 87.82 on PinchBench and 66.75 on BrowseComp. NVIDIA's blog calls it the top open model in its class on PinchBench, though the model card shows Qwen3.6-35B-A3B BF16 ahead at 88.07. Sibling variant Nemotron 3.5 Bolt E4B, a 4GB model designed for Jetson edge devices and conversational agents, is scheduled to release on October 23, 2026.

## Enterprise Integrations and Partner Adoption

NVIDIA's launch post names the software platforms and fine-tuning partners working with Nemotron 3.5 Super VL. Atlassian deploys the model for agentic search across Jira and Confluence repositories. CrowdStrike uses the architecture for agentic blue-team security operations, while Dassault Systèmes integrates it into engineering simulation and design workflows.

Perplexity incorporates the model for search execution, Salesforce uses it in Missionforce Operations for public sector workloads, and Synopsys applies it to semiconductor verification. Fine-tuning partners include Scale AI, which developed finance agents for Skydance vendor credit and Ernst & Young mergers and acquisitions due diligence. AfterQuery uses the model for Hebbia credit agreement analysis, and ModelScope supports ms-swift fine-tuning recipes.

NVIDIA also names Ramp, which uses the model for financial documents. Dream applies it to cybersecurity research, Lila Sciences deploys it for scientific discovery, R1 automates healthcare revenue cycles, and TrainLoop processes dermatology datasets from Nolla.

## NVIDIA Open Model Release Cadence

Kari Briski bylined the Super VL launch post. She said NVIDIA "will continue to release newer and better open models" (Digital Today, October 17, 2026).

In March 2026, NVIDIA released Nemotron 3 Super as a 120-billion-parameter text-only model under the NVIDIA Nemotron Open Model License, as detailed in our guide to Nemotron 3 Super. In August 2026, NVIDIA launched Nemotron 3.5 Lightning, a text-only model with 30 billion total and 3 billion active parameters under OpenMDW-1.1.

The October 2026 launch of Nemotron 3.5 Super VL and Bolt expands this progression into multimodal vision-language agents.

## When to Switch from Nemotron 3 Super

Upgrading from Nemotron 3 Super to Nemotron 3.5 Super VL is warranted when your workflows require visual document understanding or OpenMDW-1.1 licensing. Teams running text-only workloads with established prompt templates may find little operational benefit in replacing Nemotron 3 Super if their current inference pipeline satisfies throughput goals. However, if your applications process visual data like PDFs, charts, or video clips, Super VL adds native multimodal comprehension that Nemotron 3 Super cannot provide. For an in-depth breakdown of architectural and benchmark differences, read our comparison of Nemotron 3.5 Super VL vs Nemotron 3 Super.

Our recommendation would flip if your hardware cannot hold Super VL in memory: NVIDIA publishes no VRAM figure, so measure before migrating. Before committing engineering resources to a migration, test your document parsing pipelines on build.nvidia.com to see how Nemotron 3.5 Super VL handles your own documents.

## What you need to run Nemotron 3.5 Super VL yourself

Nemotron 3.5 Super VL needs real memory, but it is within reach of a high-end workstation or a couple of professional GPUs — and many teams simply rent instead of buying. Match the path below to whether you want to own the hardware or pay by the hour.

| Path | What it is | Best for | Get started | 
|---|---|---|---|
| Call the hosted API | Use Nemotron 3.5 Super VL as a pay-per-token API — zero hardware | Most teams; getting started | OpenRouter | 
| Rent GPUs by the hour | Spin up an H100 / A100 for a few dollars an hour | Flexible self-hosting without buying cards | RunPod | 
| Local on unified memory | One Mac with enough unified memory to hold a 4-bit quant | A single quiet on-prem box | Apple Mac Studio (M4 Max, 128GB) | 
| Local on a workstation GPU | One 48GB pro card, or two 24GB consumer cards | Power users who want hardware they own | NVIDIA RTX 6000 Ada (48GB) | 

To put Nemotron 3.5 Super VL to work once it is live, connect a coding client like Cursor (via OpenRouter) or a local runner such as Ollama.

## Frequently Asked Questions

- Nemotron 3.5 Super VL is an open-weight 120-billion-parameter vision-language model released by NVIDIA on October 15, 2026. It uses a hybrid Mixture-of-Experts architecture with 12 billion active parameters per token to process text, image, and video inputs for autonomous agents.
- Nemotron 3.5 Super VL is released under the OpenMDW License Agreement version 1.1 (OpenMDW-1.1), and NVIDIA publishes open weights, open datasets, and training recipes. Hosted Application Programming Interface trial access is subject to separate NVIDIA trial terms.
- Nemotron 3.5 Super VL contains 120 billion total parameters, with 12 billion parameters active per token during inference. The specifications section on build.nvidia.com lists an exact parameter count of 123,611,012,096.
- The model accepts text prompts, Joint Photographic Experts Group (JPEG) and Portable Network Graphics (PNG) images, and Moving Picture Experts Group-4 (MP4) video files up to two minutes in duration. Document pages are submitted as page images with text, while outputs remain text only.
- Nemotron 3.5 Bolt is a compact sibling model with 74 billion total parameters and 9 billion active parameters, compressed from Super VL to fit into 24 gigabytes of memory. It runs on consumer hardware like GeForce RTX graphics cards and supports an up to 256K-token context window.
- Paid endpoints on OpenRouter start at $0.10 per million input tokens and $0.40 per million output tokens as of October 2026, and OpenRouter also lists a free variant. For self-hosting, NVIDIA lists one DGX Spark, one H200, two H100s, or one GB200, but publishes no official Video Random Access Memory (VRAM) requirement.
