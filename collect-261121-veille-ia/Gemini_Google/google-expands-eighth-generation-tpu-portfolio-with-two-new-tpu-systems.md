---
id: collect-261121-veille-ia/gemini-google/google-expands-eighth-generation-tpu-portfolio-with-two-new-tpu-systems
title: "Google expands eighth-generation TPU portfolio with two new “TPU systems”"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["google", "systems", "data", "eighth-generation", "tpu", "portfolio", "models", "said"]
source: https://www.datacenterdynamics.com/en/news/google-expands-eighth-generation-tpu-portfolio-with-two-new-tpu-systems/
source_anchor: ""
source_lines: [1, 33]
section: "Gemini_Google"
---
Google has expanded its eighth-generation TPU portfolio with two additional offerings: TPU 8tx and TPU 8i Max.

Described by Google as “TPU systems” rather than chips, the company said that this means it has reconfigured the compute, networking, storage, and packaging around its TPU 8t (for training) and TPU 8i (for inference), first unveiled in April 2026.

“As enterprises bring autonomous agents into production, we have created two new configurations of these foundational systems to solve two urgent new challenges: hardware-level data security and million-token context scale,” Amin Vahdat, chief technologist and SVP for AI and Infrastructure at Google, wrote in a blog post detailing the new systems.

## The confidential computing TPU

Built on the same silicon as TPU 8t and paired with custom AMD CPU hosts, Google said the TPU 8tx has been optimized for “secure and confidential inference” but can also support small-scale training workloads, making it best suited for regulated enterprises and governments running frontier or proprietary models and autonomous agents alongside sensitive data.

Whereas encrypted data traditionally has to be unlocked in memory when stored or sent over a network in order to be processed, the TPU 8tx “builds security directly into the hardware layer,” keeping sensitive data and model weights protected and isolated even while in active use.

The system also makes use of Google’s Boardfly network, created for real-time reasoning, thus allowing the TPU 8tx to “keep multi-step agents moving,” with the company claiming it can deliver up to 2x lower latency compared to TPU 8t with a near-zero performance penalty.

Finally, the TPU 8tx will allow organizations to simultaneously run and customize their models on a single system, enabling teams to fine-tune models using their private data, distill large models into smaller ones, and train their agents through reinforcement learning, all without needing to migrate sensitive data between separate clusters.

## The context and density TPU

Meanwhile, the TPU 8i Max has been designed to tackle an issue known as the “memory wall” – the gap between processor speed and memory bandwidth.

By combining two TPU 8i chips onto a single package, Google said the TPU 8i Max “scales compute and memory in lockstep,” delivering double the compute, double the SRAM, and double the HBM bandwidth of TPU 8i, allowing processors to both remain equipped with essential data while also ensuring large workloads don’t stall.

Additionally, Google claimed that because TPU 8i Max fits twice the silicon within the existing rack footprint, data centers can deploy significantly more computing power per square foot and per watt.

In his blog post, Vahdat said that together, the four offerings that make up Google’s eighth-generation TPU portfolio give customers “a purpose-built accelerator for every stage of the agentic lifecycle.”

“While flexible across workloads, together the four systems in our eighth-generation TPUs give customers a purpose-built accelerator for every stage of the agentic lifecycle. TPU 8t for scale-up training, TPU 8i for low-latency reasoning, TPU 8tx for confidential computing, and TPU 8i Max for massive context windows and the highest compute density per rack."

He added: “With computing moving from human to agent scale, businesses are asking for accelerators that can reason across AI models’ massive context windows. In addition, some highly regulated customers are asking for specialized encryption capabilities for unique workloads.

“The new TPU systems we are announcing today address this new reality.”

## Comments
