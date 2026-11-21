---
id: collect-261121-veille-ia/gemini-google/google-expands-tpu-lineup-with-8tx-and-8i-max-systems
title: "Google expands TPU lineup with 8tx and 8i Max systems"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["google", "said", "systems", "silicon", "memory", "data", "training", "lineup"]
source: https://www.siliconreport.com/google-expands-tpu-lineup-with-8tx-and-8i-max-systems
source_anchor: ""
source_lines: [1, 43]
section: "Gemini_Google"
---
Google Cloud said on Nov. 9 that it is expanding its eighth-generation custom artificial intelligence accelerator portfolio with the TPU 8tx for confidential computing and the dual-chip TPU 8i Max for massive context windows.

The release builds on the rollout of the TPU 8t for large-scale training and the TPU 8i for real-time reasoning, which Google introduced in April. The additions bring Google's eighth-generation lineup to four systems within a single calendar year, marking the first time the company has shipped that many TPU systems in one year across more than a decade of developing in-house silicon.

## Custom silicon for confidential agents and memory walls

Google designed both new systems around existing eighth-generation processors rather than fabricating brand-new silicon dies. The company said this modular approach focuses on arranging packaging, high-speed memory, host processors and optical networking around proven accelerator blocks to deliver specialized hardware within months instead of multi-year chip fabrication cycles.

The TPU 8tx is built on the same silicon die as the TPU 8t training chip but packages it specifically for secure and confidential inference alongside small-scale training. To isolate sensitive enterprise workloads, Google paired the accelerator with fifth-generation AMD EPYC central processing unit hosts and integrated hardware-level memory encryption directly into the platform.

Standard cloud computing encrypts data at rest and across networks, but information is traditionally decrypted while active in system memory during processing. The 8tx keeps model weights and customer datasets isolated and encrypted during computation, closing that security gap for regulated enterprises and government agencies.

To manage latency across agent workflows, the TPU 8tx incorporates the Boardfly optical interconnect network originally created for the 8i inference system. Google said this configuration cuts latency by up to half compared with the baseline TPU 8t while imposing a near-zero performance penalty.

By combining training-grade compute with fast inference networking, Google said organizations can run and customize models on a single system. Enterprises can fine-tune models on private data, distill larger architectures into smaller ones and train agents through reinforcement learning without moving sensitive data between separate clusters.

The second system, the TPU 8i Max, addresses the memory bandwidth bottlenecks that emerge when models process multi-hour audio, large codebases or long video streams. Google connected two TPU 8i chips onto a single physical package, doubling the available compute density, static RAM and high-bandwidth memory capacity and bandwidth relative to a standalone TPU 8i.

Because the dual-chip package fits into existing data center rack footprints, Google said it doubles the silicon deployed per rack to raise compute density per square foot and per watt. The setup keeps processors supplied with data so massive agentic tasks do not stall.

## Shifting silicon strategy toward external enterprise demand

Google first deployed TPUs internally more than a decade ago to handle voice recognition and language translation queries, later using successive generations to train and serve its flagship Gemini models. Through its seventh generation, Google delivered a single unified chip architecture per cycle. With the eighth generation, Google divided the line into specialized roles.

According to Google, the baseline 8t delivered up to 2.7 times the training performance of prior chips, while the 8i yielded an 80% improvement in inference price-performance. The company co-designed the chips alongside Google DeepMind, using internal reinforcement learning tools such as AlphaChip to generate physical layouts in hours rather than weeks.

External adoption has expanded alongside that architectural specialization. Google said its cloud TPU business now serves nearly 100 external customers, including commercial retailers, financial trading firms, frontier research labs and universities. Customers can access the chips through Google Cloud infrastructure or deploy TPU hardware directly inside their own data centers.

Silicon Report reported on Oct. 27 that Oracle announced Google's Tensor Processing Units will be available through Oracle Cloud Infrastructure, marking the first time Google's custom artificial intelligence silicon will be offered on a competing hyperscale cloud platform.

Mark Lohmeyer, vice president of AI and computing infrastructure at Google Cloud, said the broader industry is moving away from simple single-turn query responses toward multi-step autonomous agents. Lohmeyer said those compound workflows create infrastructure challenges that hinge on data transfer speeds, memory efficiency, latency reduction and operational costs.

## Software stack updates for developer access

To make its proprietary architectures easier to adopt, Google paired the hardware release with several software updates intended to lower migration friction from industry-standard graphics processing units. In October, Google open-sourced TorchTPU, allowing developers to execute native PyTorch code and Hugging Face models on TPUs with minimal code modifications.

The hardware also supports open-source serving engines including vLLM and SGLang. Alongside TorchTPU, Google previewed an open-source TPU agent suite that connects with developer tools such as Claude Code, Cursor and Google Antigravity to automate onboarding and autotune custom kernels using natural language prompts.

For runtime efficiency, Google introduced the Agent Compute Engine, software that automatically suspends idle agent sessions and shifts working memory off the chip in milliseconds when an agent pauses to await database queries or human review. The company said this cuts operational infrastructure costs during stateful workflow pauses.

Google also open-sourced TPU Sync, a model-state coordination layer for TPU clusters. The software treats chip memory, host memory, shared storage and network fabric as a unified hierarchy, automatically tiering massive key-value caches off the chip in milliseconds and synchronizing reinforcement learning weights to cut transfer latency and time-to-first-token by more than 90%.

On Google Kubernetes Engine, Google made Cloud Pathways and dynamic optical circuit slicing generally available. The dynamic slicing update decouples physical hardware provisioning from logical slice formation using optical circuit switches, which Google said slashes job startup times by up to five times and accelerates fault recovery by up to 4.5 times without manual node rebuilding.
