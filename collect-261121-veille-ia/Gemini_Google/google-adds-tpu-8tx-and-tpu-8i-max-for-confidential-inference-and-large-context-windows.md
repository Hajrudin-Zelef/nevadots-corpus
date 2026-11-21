---
id: collect-261121-veille-ia/gemini-google/google-adds-tpu-8tx-and-tpu-8i-max-for-confidential-inference-and-large-context-windows
title: "Google adds TPU 8tx and TPU 8i Max for confidential inference and large context windows"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["google", "inference", "context", "tpu", "data", "confidential", "windows", "blog"]
source: https://dev.to/delimitter/google-adds-tpu-8tx-and-tpu-8i-max-for-confidential-inference-and-large-context-windows-5c1i
source_anchor: ""
source_lines: [1, 27]
section: "Gemini_Google"
---
📅 November 9, 2026 • Google Cloud blog

Google expanded its eighth-generation TPU portfolio with **TPU 8tx and TPU 8i Max**. In a blog post, Amin Vahdat, chief technologist and SVP for AI and Infrastructure at Google, described hardware-level data security and million-token context as urgent challenges for enterprises bringing autonomous agents into production. Google calls the additions **systems rather than chips**: they reconfigure compute, networking, storage and packaging around the TPU 8t and TPU 8i chips unveiled in **April 2026**. Google says this is the first time it has shipped **four TPU systems within a single year**.

**TPU 8tx** uses the same silicon as TPU 8t, paired with custom AMD CPU hosts, and is optimized for secure and confidential inference. Data and model weights remain protected and isolated during active use, addressing the point at which data traditionally has to be decrypted in memory for processing. The system uses Boardfly, a network developed for real-time inference. Google claims **up to 2x lower latency** than TPU 8t with a near-zero performance penalty. TPU 8tx also supports small-scale training: teams can fine-tune models on private data, distill larger models into smaller ones and train agents through reinforcement learning on the same system, without migrating sensitive data between clusters.

**TPU 8i Max** places two TPU 8i chips on a single package. Compared with TPU 8i, it **doubles compute, SRAM and HBM bandwidth**. Google designed it to address the memory wall, putting twice the silicon within the existing rack footprint to provide more computing per square foot and per watt. Vahdat positions TPU 8t for scale-up training, TPU 8i for low-latency reasoning, TPU 8tx for confidential computing and TPU 8i Max for massive context windows and the highest compute density per rack. The distinctions concern how each system serves a stage of the agentic lifecycle, rather than a single configuration intended for every workload.

For automation and infrastructure teams, the practical distinction is between keeping sensitive agent work inside a **protected computing environment** and supplying the memory resources needed for large context windows. TPU 8tx brings private-data adaptation and inference onto the same system, while TPU 8i Max changes the compute and memory resources available within a rack. Software compatibility also matters when evaluating these paths. According to The Decoder, Google now counts **nearly 100 external TPU customers**, has released TorchTPU as open source so PyTorch code can run with minimal changes, and supports the vLLM and SGLang inference frameworks. These details give engineering teams a basis for examining both workload fit and the software changes involved.

Sources:

blog.google

datacenterdynamics.com

the-decoder.com

**Sources:**

- https://blog.google/innovation-and-ai/infrastructure-and-cloud/google-cloud/eighth-generation-tpu-agentic-era/
- https://www.datacenterdynamics.com/en/news/google-expands-eighth-generation-tpu-portfolio-with-two-new-tpu-systems/
- https://the-decoder.com/google-ships-four-tpu-variants-in-one-year-as-it-ramps-up-competition-with-nvidia/

**Continue the conversation:** https://t.me/AIAutomationArchitecture · Bluesky

## Top comments (0)
