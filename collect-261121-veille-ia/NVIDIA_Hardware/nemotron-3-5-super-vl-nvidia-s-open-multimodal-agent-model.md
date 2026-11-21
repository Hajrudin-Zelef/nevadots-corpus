---
id: collect-261121-veille-ia/nvidia-hardware/nemotron-3-5-super-vl-nvidia-s-open-multimodal-agent-model
title: "Nemotron 3.5 Super VL: NVIDIA's Open Multimodal Agent Model"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: ["2026-10-15"]
keywords: ["nvidia", "nemotron", "model", "super", "open", "multimodal", "tasks", "reports"]
source: https://thehype.news/nemotron-3-5-super-vl/
source_anchor: ""
source_lines: [1, 36]
section: "NVIDIA_Hardware"
---
Nvidia has released Nemotron 3.5 Super VL, an open-weight multimodal model built for visual reasoning and agentic tasks. It has 120 billion parameters, with 12 billion active per token. The model handles text, images, documents and video, and can use tools to complete tasks.

It combines Nvidia's Nemotron-H hybrid decoder with the RADIO vision encoder and a dedicated video temporal embedder. Nvidia distilled the model from Nemotron 3 Ultra using several teacher models to evaluate its text and vision outputs.

## Nemotron 3.5 Super VL benchmarks

Nvidia reports results in four areas:

- Faster inference: Nvidia reports up to 5× higher throughput than comparable open models on a single GPU for agentic tasks. The model exceeds 600 tokens per second on the Baseten endpoint. On 10,000 SWE-bench Pro tasks on B200 GPUs, it finished 4× faster than the next-fastest open model.
- Visual reasoning: Nemotron 3.5 Super VL scores 80.8% on CharXiv, increasing to 84% with a Python interpreter. Nvidia also reports strong results on OCRBench v2.
- Agentic tasks: the model scores 63.44% on Hermes Bench, up from 56.33% for Nemotron 3 Ultra. On LangChain evals, it reaches 87.05%, compared with 84.3% for its predecessor.
- Indian languages: Nvidia added 10 billion training tokens for Indian languages and released a dataset covering 22 languages, developed with IIT Madras and AI4Bharat.

All of these figures are reported by Nvidia.

## How Nvidia made it faster

Nvidia has also released lilicorr, an open speculative decoding method that evaluates draft tokens together instead of one at a time. The company reports generation speedups of up to 2.3× on DGX Spark. Other optimizations include NVFP4 quantization, multi-token prediction and caching for both attention and recurrent Mamba states.

The model is designed to run on Nvidia hardware, including DGX Spark with 128 GB of memory. According to Nvidia, its Dynamo inference platform can serve eight times as many users on 16 B200 GPUs at 50 tokens per second per user, while cutting time to first token by 85%.

## Early results from partners

Several companies have shared early results. Scale AI expects inference costs for its work with Skydance to drop by 40×, alongside a 5× reduction in latency. Ramp expects up to 50% higher throughput and 10× lower latency when converting receipts into structured JSON. AI21 reports a 40% accuracy improvement in its Alice threat intelligence system. These figures come from the companies themselves and have not necessarily been independently verified in production.

## Where to get Nemotron 3.5 Super VL

Nemotron 3.5 Super VL is available on Hugging Face, ModelScope and OpenRouter. It supports vLLM, SGLang, Ollama and LM Studio, among other tools. Nvidia has also published base and post-trained checkpoints, an NVFP4 version, training recipes, datasets and reinforcement learning environments.

The release pairs open model weights with tools and optimizations for running multimodal agents more efficiently, with a particular focus on Nvidia hardware.

## Sources

- Nemotron 3.5 Super VL announcement, Nvidia technical blog – Oct 15, 2026; benchmarks, lilicorr, Dynamo, partner results
- Nemotron 3.5 Super VL model card, Hugging Face – architecture, training, indic data, license
- Nemotron 3.5 Super VL on OpenRouter – hosted access
