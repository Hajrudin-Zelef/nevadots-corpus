---
id: collect-261121-veille-ia/gemini-google/google-cloud-nvidia-launch-a5x-vera-rubin-instances
title: "Google Cloud & NVIDIA Launch A5X Vera Rubin Instances"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["vera", "rubin", "cloud", "nvidia", "google", "instances", "nvidias", "gpus"]
source: https://dev.to/luckytaorem/google-cloud-nvidia-launch-a5x-vera-rubin-instances-a8h?ref=refetch.io
source_anchor: ""
source_lines: [1, 106]
section: "Gemini_Google"
---
Google Cloud and NVIDIA have moved from preview to **general availability** for the **A5X instances**, the first production‑grade servers built around NVIDIA’s Vera Rubin architecture. The announcement, slated for release next month, also outlines a roadmap that will eventually extend the service to European regions. 

At its core, the A5X combines:

- 
**NVIDIA Vera Rubin NVL72** CPUs and GPUs with a tightly coupled high‑bandwidth fabric.
- 
**NVIDIA Connect X‑9 Super NICs** for ultra‑low‑latency networking.
- 
**Google Cloud Virgo Network** that stitches the compute nodes into a unified AI hypercomputer.
- 
**Fourth‑generation advanced liquid‑cooling** that keeps power density high while staying energy‑efficient.

The result is a platform purpose‑built for **agentic inference** and **large‑scale reinforcement learning (RL)** workloads that require rapid feedback loops between experience generation and model training.

The NVL72 is a hybrid CPU‑GPU die that merges a high‑performance **Vera CPU** with **Rubin GPUs** on the same package. This integration eliminates the PCIe bottleneck that traditionally separates CPU and GPU memory domains. Key specs include:

- 
**Up to 96 Tensor Cores** per GPU, optimized for mixed‑precision matrix operations.
- 
**NVLink‑style bandwidth** exceeding 1 TB/s between the CPU and GPU, and between GPUs in the same rack.
- 
**Unified memory pool** that allows the RL agent to stream experience data directly into the training pipeline without staging copies.

The **Connect X‑9** is a 400 Gbps Ethernet NIC that leverages NVIDIA’s proprietary networking stack. When paired with Google’s **Virgo** fabric, the A5X can sustain:

- 
**Sub‑microsecond inter‑node latency** , crucial for synchronizing policy gradients across thousands of parallel agents.
- 
**Deterministic bandwidth allocation** , ensuring that data‑intensive replay buffers never become a choke point.

Heat removal has always been a limiting factor for dense AI clusters. Google’s latest liquid‑cooling loop:

- 
**Maintains chip temperatures below 70 °C** under sustained 90 % utilization.
- 
**Reduces PUE (Power Usage Effectiveness) to 1.08** , a notable improvement over traditional air‑cooled racks.
- 
**Supports dynamic coolant flow** , scaling cooling capacity in real time as workloads fluctuate.

Beyond raw performance, the A5X family inherits NVIDIA’s **Confidential Computing** capabilities. While the A5X itself is a public‑cloud offering, NVIDIA’s **G4 VMs**—powered by RTX PRO 6000 Blackwell GPUs—provide:

- 
**Encrypted memory regions** that protect model weights and inference data from insider threats.
- 
**Attestation mechanisms** that verify the integrity of the execution environment before workloads start.

These features are already being leveraged by customers such as **Vertebrae**, **Polaris Cloud**, and **Confidential Core AI** for privacy‑sensitive conversational agents and policy‑governed enterprise intelligence.

Reinforcement learning has historically been constrained by the “experience‑generation loop”: agents must collect data, send it to a trainer, receive updated policies, and repeat. The latency and bandwidth of that loop dictate how quickly an agent can improve.

- 
**Tight CPU‑GPU coupling** means the same silicon can both simulate environments (via the Vera CPU) and train policies (via the Rubin GPUs) without moving data across a PCIe bus.
- 
**High‑bandwidth networking** ensures that multi‑node clusters can share experience buffers in near‑real time, enabling**distributed RL at petaflop‑scale** .

David Silver of **Ineffable Intelligence**—the first A5X customer—states: *“We’re building superlearners that continually improve through their own experience… Vera Rubin’s combination of compute performance and high‑bandwidth connectivity… lets us push the scale and ambition of our reinforcement learning research further.”* 

With A5X, organizations no longer need to assemble bespoke on‑prem clusters; they can spin up a single instance that delivers the same throughput previously achievable only with dozens of nodes.

The integration of confidential computing means that **sensitive policy data** (e.g., financial trading strategies, medical decision models) can be trained in the same high‑performance environment without exposing raw data to the cloud provider. This opens doors for regulated industries to adopt RL

at scale while preserving data privacy.

Ineffable Intelligence has been the flagship early adopter, using a single A5X instance to train agents that learn to play complex strategy games and to optimize logistics networks. By co‑locating the simulation environment on the Vera CPU and the training loop on the Rubin GPUs, the team reports a **3.5× reduction in end‑to‑end training time** compared with their previous 32‑node GPU cluster.

WPP demonstrated a creative‑agent pipeline that generates, evaluates, and iterates on advertising concepts. The workload, which previously required a multi‑node G4‑based cluster to achieve acceptable latency, now runs on a single A5X instance, delivering **single‑instance throughput equivalent to a 12‑node deployment**. This translates into lower operational costs and faster time‑to‑market for campaign variants.

These customers are leveraging the G4 VMs with RTX PRO 6000 Blackwell GPUs for workloads that demand **memory encryption and attestation**. Use cases include:

- 
**Vertebrae** : Secure conversational agents that handle personally identifiable information (PII) for customer support.
- 
**Polaris Cloud** : Policy‑governed AI that enforces compliance rules during inference, ensuring that outputs stay within regulatory bounds.
- 
**Confidential Core AI** : End‑to‑end encrypted pipelines for training proprietary financial models, where even the cloud provider cannot view the raw data.

🔹 **--------**

  • Availability: --------------

  • Expected Launch: -----------------

🔹 **United States (us‑central1, us‑west2)**

  • Availability: GA (General Availability)

  • Expected Launch: November 2026

🔹 **Europe (europe‑west1, europe‑north1)**

  • Availability: GA – announced

  • Expected Launch: Q1 2027

🔹 **Asia‑Pacific (asia‑east1, australia‑south1)**

  • Availability: Preview

  • Expected Launch: Mid‑2027

Pricing is published on the Google Cloud Marketplace and follows a **pay‑as‑you‑go** model, with discounts for sustained‑use and committed‑use contracts.

*Read the full breakdown originally published at https://ltdeveloperblogs.github.io/posts/google-cloud-and-nvidia-announce-general-availability-of-nvidia-vera-rubin-infrastructure/*

## Top comments (0)
