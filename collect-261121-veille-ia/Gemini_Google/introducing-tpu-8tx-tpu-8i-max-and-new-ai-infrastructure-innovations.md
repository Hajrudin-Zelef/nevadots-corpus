---
id: collect-261121-veille-ia/gemini-google/introducing-tpu-8tx-tpu-8i-max-and-new-ai-infrastructure-innovations
title: "Introducing TPU 8tx, TPU 8i Max, and new AI infrastructure innovations"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["tpu", "tpus", "memory", "data", "software", "models", "single", "introducing"]
source: https://www.linkedin.com/pulse/introducing-tpu-8tx-8i-max-new-ai-infrastructure-innovations-dkfde
source_anchor: ""
source_lines: [1, 35]
section: "Gemini_Google"
---
When we built our first TPU more than a decade ago, we knew AI software would evolve faster than silicon manufacturing lifecycles. By architecting our TPUs as modular building blocks within our AI Hypercomputer, our engineers can adjust packaging, high-speed memory, optical networking and hardware security around the chip to deliver specialized systems in months rather than years.

As computing moves from human scale to agent scale, organizations are asking for accelerators that can power reasoning workloads across massive context windows and protect sensitive data and model weights in active use. This is paired with an open software stack that makes TPUs accessible to every PyTorch and JAX developer. Here is a look at the hardware and software updates we announced this week:

## 1. Two new eighth-generation TPU systems: TPU 8tx and TPU 8i Max

Built for regulated enterprises and governments, TPU 8tx pairs TPU 8t silicon with custom AMD CPU hosts and our low-latency Boardfly network to provide hardware-level confidential computing, keeping sensitive data and model weights protected and isolated in memory while delivering up to 2x lower latency than TPU 8t with a near-zero performance penalty. Organizations can run inference, fine-tune on private data, distill large models into smaller ones and train agents through reinforcement learning on a single system without migrating sensitive data between separate clusters. Alongside it, TPU 8i Max combines two low-latency TPU 8i chips onto a single package within the existing rack footprint, delivering double the compute, double the SRAM and double the HBM bandwidth of TPU 8i so multi-agent workloads can reason across massive software codebases or hours of video without stalling on memory bandwidth.

## 2. Open-sourcing the developer and serving stack on TPUs

Building on our recently open-sourced TorchTPU stack, PyTorch code and Hugging Face models run natively on TPUs with out-of-the-box backends for vLLM and SGLang, achieving state-of-the-art price-performance on the industry-standard InferenceX and AgentX benchmarks. Today, we are introducing the open-source TPU agent suite (in preview), which plugs into Claude Code, Cursor and Google Antigravity to automate TPU onboarding in minutes, diagnose bottlenecks and autotune custom kernels using plain language. For production deployments, our new open-source XLA Serving Engine lets teams train models in Python and serve them directly in a low-latency C++ runtime across TPUs, GPUs and CPUs.

## 3. Breaking through the memory wall with open-source TPU Sync

As AI shifts from stateless single-turn prompts to million-token agent sessions, disaggregated prefill-and-decode serving and continuous reinforcement learning, orchestrating live model state has become a primary infrastructure bottleneck. Today we are open-sourcing TPU Sync, a native C++ library that unifies TPU HBM, pinned host DRAM, shared storage (local SSDs and Google Cloud Managed Lustre) and the data center network into a single memory hierarchy using layout-preserving direct DMA transfers overlapped with active computation. Integrated with vLLM-TorchTPU, SGLang-TorchTPU and llm-d 1.0 on GKE, TPU Sync cuts KV transfer latency by over 90% and reduces Time to First Token by an average of 90% on heavyweight open-weight MoE models (235B to 480B parameters) in prefix-caching and disaggregated-serving workloads, while providing experimental weight-transfer primitives for reinforcement learning.

## Recommandé par LinkedIn

## 4. Cloud Pathways and GKE dynamic slicing are now generally available

Originally engineered by Google DeepMind to train and serve Gemini, and used to process more than 130 million Cloud TPU chip-hours on GKE over the last 90 days, Cloud Pathways is now generally available on GKE (and in preview on Slurm clusters via Cluster Director). Its single-controller architecture lets a single JAX client orchestrate multiple TPU slices spanning thousands of chips as if they were connected locally, with colocated Python sidecars and host-memory snapshots that dynamically reshard training down to N-1 slices during hardware failures. Pairing with it, GKE dynamic slicing is also generally available today, using TPU optical circuit switching to stitch 16-VM TPU cubes into larger slice topologies on demand (super-slicing) or partition cubes into smaller isolated workloads (sub-slicing), reducing job startup times by up to 5x and accelerating fault recovery by up to 4.5x without recreating nodepools.

          

        

Really useful roadmap — the jump from calling an LLM to running something secure and reliable in production is where it gets real. The Responsible AI and production part stood out to me.

Congrats on the new TPU release, exciting

What caught my eye is how Google is addressing two very different barriers to scaling AI agents. The 8tx is designed for workflows that involve sensitive data, while the 8i Max is built for tasks that need to work through large amounts of information. I’d be curious to see how the benchmark gains translate into cost per successfully completed task in a real enterprise workflow. For example, what does it cost an agent to review a large set of documents and produce an answer that meets the required quality bar? That’s a number a business can build a case around.

หา

What stands out is the continued evolution of AI infrastructure as an integrated ecosystem. Advancements in TPU performance are important, but the real competitive advantage comes from combining specialized hardware, optimized software, and efficient orchestration. As AI workloads become more demanding, scalability, utilization, and performance per dollar will matter just as much as raw computing power. The companies that can deliver across the entire infrastructure stack will be well positioned for the next phase of AI growth.
