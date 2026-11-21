---
id: collect-261121-veille-ia/unifi-ubiquiti/build-with-muse-spark-open
title: "Build with Muse Spark Open"
domain: unifi-ubiquiti
role: reference
task: article
actors: ["Meta"]
dates: []
keywords: ["muse", "spark", "open", "model", "sglang", "vllm", "nvidia", "nvfp4"]
source: https://dev.meta.ai/resources/blog/build-with-muse-spark-open?team_id=1074055538425878&project_id=1366748018914493
source_anchor: ""
source_lines: [1, 175]
section: "Muse"
---
# Build with Muse Spark Open

Today we're releasing Muse Spark Open, the open weights version of Muse Spark, an 816B-parameter sparse mixture-of-experts model that activates roughly 42 billion parameters per token. It takes text, image, video and audio input and holds a million tokens of context. It's trained for agentic work, so you can build agents that can read a large codebase, plan a change, call tools, check the result and correct themselves when a step fails.

For more details on the model and the research foundations, check out the Meta research blog.

## TL;DR

- **Inference runtimes:** Runs on SGLang, vLLM, TensorRT-LLM, Hugging Face Transformers, MLX, llama.cpp and Unsloth at launch, and verified on NVIDIA, AMD, Intel and Apple silicon.
- **Hosted inference:** Baseten, Fireworks AI, Groq, Modal, Nebius, OpenRouter and Together AI.
- **Precisions:** Includes BF16 and NVFP4 support from Meta, (FP8, MXFP4) from AMD, (INT4, MXFP4) optimized for Crescent Island from Intel and GGUF quants from Unsloth.
- **Post Training:** Fine-tuning with TorchTitan, SGLang + Miles, NVIDIA NeMo AutoModel, Hugging Face TRL and managed post-training APIs from Fireworks AI and Together AI are all live at release.
- **Decoding Model:** Draft models for faster token generation, built by SGLang (*DFlash V2* ) and vLLM (*DSpark* ).
- **Model Catalogs:** Amazon SageMaker AI, Google Cloud Model Garden, Microsoft Foundry Models
- **Download from Hugging Face:** Link
- **Key documentation**
  - Get the model – Download
  - Run the model – Deploy
  - Get the most out of Muse Spark Open – Prompting guide
  - Post Training – Fine-tuning and Reinforcement learning

Muse Spark Open launches with day-one support in major inference and training frameworks. It can be served with SGLang, vLLM, TensorRT-LLM, Transformers, llama.cpp, MLX or Unsloth on NVIDIA, AMD, Intel or Apple hardware. Choose a quantized checkpoint that fits your hardware, speed up generation with a draft model, and fine-tune or train it with reinforcement learning.

## Serving it yourself

Run Muse Spark Open on the inference runtime you already use. We worked with RadixArk, Inferact, NVIDIA, Apple and Hugging Face so that SGLang, vLLM, TensorRT-LLM, MLX, Transformers and llama.cpp all support it at launch:

### SGLang

SGLang supports BF16 and NVFP4 and ships the DFlash2 speculative decoding path for faster token generation.

**Running it on SGLang**

shell

### vLLM

vLLM supports BF16, NVFP4 and MXFP4, verified on both NVIDIA and AMD GPUs, with DSpark speculative decoding on CUDA and ROCm.

**Running it on vLLM**

shell

### Mac Studio with MLX

MLX now supports inference using the NVFP4 checkpoint. Serve the model on either two Mac Studios, with M5 Ultra and 512GB of unified memory; or four Mac Studios with 256GB of unified memory, for a total of 1TB of unified memory, enough to hold a NVFP4 quantized version of Muse Spark Open. Connect your Mac Studios directly using the built-in support for Thunderbolt 5 and RDMA to ensure high-bandwidth, low-latency communication.

Muse Spark Open's mixture-of-experts architecture activates only 42B parameters per token, and is complemented with MLX tensor parallelism. This splits every layer of the model across both machines, so each machine holds half the weights and does half the computation on every token. The weights never move. Only small intermediate results travel between the two machines in microseconds enabling the two Mac Studios to work as one. The result is the most capable US open-weight model running on the desktop. MLX Adapter.

See the Muse Spark Open on Mac cookbook for details.

### NVIDIA TensorRT-LLM and Dynamo

Serve the NVFP4 checkpoint on a single four-GPU Blackwell node with TensorRT-LLM, which splits attention four ways and spreads the 256 experts across the GPUs. Start from the deployment guide and the tuned serving configurations in the feat/musespark-open branch:

shell

To scale beyond one node, deploy with NVIDIA Dynamo. It separates prefill and decode onto different workers and routes each request to the worker that already holds its cached context. Follow the Muse Spark Open Dynamo recipe for launch commands and settings.

Technical blog: Link

### Hugging Face Transformers and llama.cpp

Hugging Face hosts the BF16 and NVFP4 checkpoints and added Transformers support for the architecture. llama.cpp support means the GGUF quants run on CPU, mixed CPU and GPU, or full GPU.

**Running it on HF**

python

**Running it on llama.cpp**

shell

### Unsloth

Run Muse Spark Open locally with Unsloth Desktop or serve it via the Unsloth API using Dynamic GGUFs for optimized memory usage and fast local inference.

**Running it on Unsloth**

Run Muse Spark Open on a single workstation instead of a GPU cluster. Unsloth's Dynamic GGUFs compress the model to as little as 2-bit, so the smallest version fits in about 300 GB of combined system RAM and VRAM, or unified memory on Apple silicon. Unsloth Desktop runs on macOS, Windows and Linux, uses llama.cpp for CPU and GPU inference, and offloads layers to system RAM when VRAM runs short. Download and chat with the model in the app, or serve it through OpenAI and Anthropic compatible endpoints for your applications and coding agents.

shell

## Hosted inference

Launch-day serverless and dedicated endpoints, so you can evaluate the model before you commit any hardware to it.

**Model catalogs:** for teams who need to utilize the model in their own private instance, in addition to the option of the HuggingFace repository, we have also partnered with the leading cloud and data providers to ensure easy access via their online model catalogs. This includes: AWS SageMaker, Google Cloud Model Garden, Microsoft Foundry Models.

### What early users are seeing

Early users evaluated Muse Spark Open in production-oriented agentic workflows ahead of launch.

- "*Muse Spark Open consistently selected the right next tool and kept its responses compact, exactly the decisiveness production agents need.* " Zimu Li, Member of Technical Staff, Factory
- "*This model performs particularly well on tool calling and long context, including multi-step and parallel calls with clean structured outputs.* " Dong Chen, Engineering, Ollama

## Quantized Models

Pick the format that fits your hardware. Meta released BF16 and NVFP4 checkpoints. AMD, Intel and Unsloth built formats for their own hardware and for local setups.

## Speculative decoding

Muse Spark Open ships with two draft models, one from each open-source runtime. The draft model proposes several tokens at once and Muse Spark Open verifies them in a single pass, so every step yields multiple tokens instead of one.

### DFlash V2 for SGLang

**Model:** RadixArk/Muse-Spark-Open-DFlash2

- **Built by RadixArk** on DFlash V2, which proposes a whole block of tokens in one pass and uses a lightweight selector to pick the best candidates.
- **Runs on NVIDIA and AMD GPUs.** SGLang drafts and selects inside a single CUDA graph, and NVIDIA's fused MoE kernels make each verification pass cheaper.
- **Works with SGLang's serving features** , including split prefill and decode workers.
- **3.7× throughput for a single request on AMD MI355X** , and 2.9× at 16 concurrent requests, with accuracy unchanged. On average, 6.4 drafted tokens are accepted per step.
- **Enable it** with:

shell

### DSpark for vLLM

**Model:** Inferact/Muse-Spark-Open-DSpark

- **Trained by Inferact** on DSpark, a method NVIDIA and the vLLM team developed together. It proposes up to seven tokens in one pass and uses a lightweight head to account for how those tokens depend on each other.
- **Runs on NVIDIA, AMD and TPU.**
- **Stays cheap at long context** by using the sliding-window attention in Muse Spark Open, even at 1M tokens.
- **Keeps its speedup as batches grow.** Adaptive verification checks fewer drafted tokens when the server is busy, where speculative decoding usually stops helping.
- **Enable it** with:

shell

## Post Training

**Fine-tune and train Muse Spark Open from launch day.** Run supervised fine-tuning, LoRA or reinforcement learning on your own data with open-source frameworks, or hand the training to a managed API.

### PyTorch TorchTitan

TorchTitan supports Muse Spark Open natively, with pre-training, multi-turn SFT, LoRA and reinforcement-learning support. The model also ships with Helion kernels, so the PyTorch path is fast without hand-written CUDA.

- PyTorch blog — Muse Spark Open weights, featuring Helion and TorchTitan: Blog
- TorchTitan reference model and recipes: Pre-training & Lora · RL Recipe

### SGLang Miles

### NVIDIA NeMo AutoModel

Developers can further post-train the model using the NVIDIA NeMo AutoModel with high-throughput efficiency, which is a post-training library based on PyTorch DTensor. It provides native Hugging Face checkpoint support with no model conversion requirements. AutoModel enables full SFT and LoRA fine-tuning out of the box, optimized for rapid experimentation on NVIDIA GPUs. Link

### Managed post-training APIs

- **Fireworks AI** — Run serverless LoRA training on the NVFP4 checkpoint, with no GPUs to provision. Write the training loop in Python, and Fireworks runs it on its GPUs. Supervised fine-tuning, DPO and RL work out of the box, with sequences up to 262,144 tokens. Serverless training guide · Cookbook
- **Together AI** — Run SFT or GRPO reinforcement learning on Together AI's post-training service. You control the training loop, and Together AI runs the infrastructure. Then deploy the result for inference, including several LoRA adapters on one deployment. SFT quickstart · GRPO quickstart · Cookbook

### Hugging Face Trainer

Fine-tune Muse Spark Open with Hugging Face's SFTTrainer. It trains a LoRA adapter on top of the released checkpoint, and Transformers shards the model across your GPUs as it loads. Load the adapter with PEFT or share it on the Hugging Face Hub. Recipe · SFTTrainer docs

python

## Start Building Today

## Acknowledgements

Day-zero support across this many runtimes, precisions, accelerators and training stacks is not something a model release gets for free. It is months of work by teams outside Meta who agreed to build against an unreleased checkpoint, and by teams inside Meta who made that checkpoint something others could build against.

**Runtimes and serving:** SGLang, vLLM, NVIDIA, Hugging Face, llama.cpp, Apple.

**Silicon:** AMD, Intel, NVIDIA, Apple.

**Quantization:** AMD, Unsloth, Intel.

**Post-training:** PyTorch, NVIDIA, Miles, Fireworks, Hugging Face, Together, Unsloth.

**Hosted inference:** Baseten, Fireworks AI, Groq, Modal, Nebius, OpenRouter, Together AI.

**Model catalogs:** Amazon SageMaker AI, Google Cloud Model Garden, Microsoft Foundry Models.

We are looking forward to seeing what you build.
