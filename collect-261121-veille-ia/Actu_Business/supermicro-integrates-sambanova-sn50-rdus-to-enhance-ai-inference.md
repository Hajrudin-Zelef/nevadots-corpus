---
id: collect-261121-veille-ia/actu-business/supermicro-integrates-sambanova-sn50-rdus-to-enhance-ai-inference
title: "Supermicro Integrates SambaNova SN50 RDUs to Enhance AI Inference"
domain: actu-business
role: reference
task: article
actors: []
dates: []
keywords: ["sambanova", "sn50", "inference", "rdus", "supermicro", "architecture", "disaggregated", "model"]
source: https://www.thenextgentechinsider.com/pulse/supermicro-integrates-sambanova-sn50-rdus-to-enhance-ai-inference
source_anchor: ""
source_lines: [1, 28]
section: "Actu_Business"
---
## Supermicro to Integrate SambaNova SN50 RDUs for Disaggregated AI Inference

Supermicro will integrate, validate, and support new disaggregated inference systems that pair NVIDIA GPUs with SambaNova SN50 Reconfigurable Dataflow Units (RDUs), according to SambaNova. This collaboration aims to optimize the inference pipeline for agentic AI workloads, which require high-speed, efficient processing for frequent, high-latency large language model (LLM) calls.

## Technical Implementation

The SN50 RDU is a fifth-generation AI chip designed specifically to handle the memory-bound phases of model inference. Key specifications include:

- **Process Technology** : 5-nanometer TSMC process SambaNova
- **Memory Hierarchy** : A three-tier architecture comprising 432 MB of on-chip SRAM, 64 GB of HBM2E, and DDR5 for prompt caches and larger model pools SambaNova
- **Networking** : 4x more network bandwidth than the previous generation SambaNova
- **Scale** : Support for up to 256 accelerators linked via a multi-terabyte-per-second interconnect, capable of running ten-trillion-parameter models SambaNova

## How It Works

The architecture utilizes a disaggregated approach to split the LLM inference process into two distinct stages. NVIDIA GPUs are utilized for the **prefill phase**, which is compute-bound and processes the initial prompt. The SambaNova SN50 RDUs handle the **decode phase**, which is memory-bound and generates responses one token at a time. By chaining operations within a single kernel, the RDU architecture reduces data movement and latency compared to traditional GPU flows SambaNova.

## Performance Results

The SN50 architecture has been benchmarked using SemiAnalysis validation on the MiniMax M2.7, where it reportedly achieved record-breaking speeds SambaNova. The hardware is designed to provide high-performance inference with a lower total cost of ownership and higher energy efficiency per token compared to standard GPU architectures.

## Availability

The rollout will begin with an initial batch of 10,000 SN50 chips, distributed first to NeoClouds and sovereign clouds before expanding to enterprise data centers SambaNova. The systems are air-cooled and designed for a power density of approximately 20 kW per rack, allowing for deployment in existing data center environments without liquid cooling requirements SambaNova.

## Discussion & Insights

## Comments (0)
