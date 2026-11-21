---
id: collect-261121-veille-ia/nvidia-hardware/nvidia-unveils-rubin-hpc-gpu-at-sc26-with-over-50-000-cuda-cores-and-3x-blackwell-performance
title: "NVIDIA Unveils Rubin HPC GPU at SC26 With Over 50,000 CUDA Cores and 3x Blackwell Performance"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: []
keywords: ["nvidia", "rubin", "cuda", "cores", "data", "vera", "sc26", "performance"]
source: https://technosports.co.in/rubin-hpc-nvidia-unveils-gpu/
source_anchor: ""
source_lines: [1, 50]
section: "NVIDIA_Hardware"
---
NVIDIA has officially expanded its next-generation Vera Rubin supercomputing architecture with the unveiling of **NVIDIA Rubin HPC**, a purpose-built data center GPU engineered to dramatically accelerate scientific simulations, engineering models, and large-scale analytics workflows. Announced during the **Supercomputing 2026 (SC26)** conference in Chicago and detailed on the official NVIDIA Blog, the new accelerator packs **more than 50,000 CUDA cores**—a record high for any single NVIDIA GPU—and delivers up to **3x the performance of NVIDIA Blackwell** for demanding computational science.

While standard AI accelerators focus primarily on dense matrix math for large language model training and inference, Rubin HPC is tailored specifically for vector processing, sparse data movement, and agentic scientific research. The announcement underscores NVIDIA’s continued push to dominate both artificial intelligence and traditional high-performance computing—a strategy we have closely followed across our coverage of NVIDIA’s next-generation data center GPUs and NVIDIA’s supercomputing hardware milestones.

## Over 50,000 CUDA Cores Built for Simulation and Agentic Discovery

Unlike generative AI workloads that rely heavily on Tensor Cores, traditional high-performance computing tasks—such as computational fluid dynamics (CFD), climate forecasting, astrophysics, genomics, and molecular dynamics—depend on massive parallel vector throughput.

To eliminate bottlenecks in these fields, NVIDIA has **more than doubled the CUDA core count** compared to the standard Rubin server GPU (28,672 CUDA cores), pushing Rubin HPC past the **50,000-core mark**—nearly 2.5 times the core density of a consumer flagship like the GeForce RTX 5090.

- **Hardware Sparse Data Acceleration:** Scientific datasets are frequently irregular and scattered across memory. Rubin HPC introduces dedicated sparse data acceleration that feeds scattered elements directly to its 50,000+ CUDA cores, minimizing memory stalls during stencil computations, reductions, and sparse-matrix solvers.
- **Seventh-Generation NVLink:** Upgrading from sixth-generation NVLink on standard Rubin chips, Rubin HPC integrates**7th-gen NVLink** to speed up GPU-to-GPU communication across multi-rack supercomputing clusters.
- **Agentic HPC Acceleration:** Paired with expanded**NVIDIA CUDA-X** libraries, Rubin HPC allows AI research agents to rapidly run physics-grounded simulations, test hypotheses, and narrow down experimental candidates before physical lab testing begins.

## NVIDIA Rubin HPC Preliminary Specifications

NVIDIA is holding back full memory bandwidth, double-precision (FP64) TFLOPS, thermal design power (TDP), and system pricing details until closer to launch, but the core architectural highlights confirmed at SC26 include:

| **Architectural Feature** | **NVIDIA Rubin HPC Specification** | 
| **Platform Architecture** | NVIDIA Vera Rubin (HPC-Dedicated Silicon) | 
| **CUDA Core Count** | **Over 50,000 CUDA cores** (NVIDIA single-GPU record) | 
| **Relative Performance** | Up to **3x NVIDIA Blackwell** in simulation and data processing | 
| **Interconnect Fabric** | **7th-Generation NVIDIA NVLink** | 
| **Specialized Engine** | Hardware Sparse Data Acceleration (stencils, sorting, reductions) | 
| **Target Workloads** | Scientific simulation, CFD, weather modeling, drug discovery, financial risk | 
| **Commercial Availability** | Scheduled for **2028** | 

## Vera CPU Racks and CUDA-X Updates Arrive Ahead of 2028 Rollout

Alongside the Rubin HPC teaser, NVIDIA confirmed that **liquid-cooled Vera CPU HPC racks**—housing 192 custom Olympus-core Vera processors linked via Quantum-X800 InfiniBand—will begin shipping from server partners including Dell Technologies, HPE, Lenovo, and Supermicro in **early 2027**. Meanwhile, new **CUDA-X** software releases are already slashing real-world research timelines, cutting Event Horizon Telescope black-hole data correlation from 30 days down to just 8 minutes.

With **NVIDIA Rubin HPC scheduled for commercial availability in 2028**, NVIDIA is signaling a clear architectural split: specialized Tensor-heavy silicon for AI factories, and ultra-dense, 50,000-core vector engines for national laboratories and industrial R&D centers. For deeper insights into how custom accelerators are reshaping global data centers, check out our analysis of Google’s expanding TPU roadmap against NVIDIA.

## FAQs

## What is NVIDIA Rubin HPC designed for?

Rubin HPC is a data center GPU built to accelerate scientific simulations, engineering models and large-scale analytics. Its target workloads include computational fluid dynamics, weather modeling, drug discovery and financial risk.

## How many CUDA cores does Rubin HPC have?

Rubin HPC has more than 50,000 CUDA cores, a record for a single NVIDIA GPU, according to the text.

## How much faster is Rubin HPC than NVIDIA Blackwell?

NVIDIA says Rubin HPC delivers up to 3x the performance of NVIDIA Blackwell for simulation and data processing.

## When is Rubin HPC expected to be commercially available?

Commercial availability is scheduled for 2028.
