---
id: collect-261121-veille-ia/nvidia-hardware/nvidia-sweeps-mlperf-training-and-endpoints-as-vera-rubin-debuts
title: "NVIDIA Sweeps MLPerf Training and Endpoints as Vera Rubin Debuts"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: ["2026-11-16"]
keywords: ["mlperf", "nvidia", "vera", "rubin", "training", "across", "endpoints", "agentic"]
source: https://www.techbytes.app/posts/nvidia-sweeps-mlperf-training-and-endpoints-as-vera-rubin-debuts/
source_anchor: ""
source_lines: [1, 100]
section: "NVIDIA_Hardware"
---
# NVIDIA Sweeps MLPerf Training and Endpoints as Vera Rubin Debuts

NVIDIA debuted its Vera Rubin NVL72 platform by sweeping MLPerf Training v6.1 and recording a 10x throughput-per-watt efficiency gain on agentic AI workloads.

By Dillip Chowdary â¢ Nov 16, 2026 â¢ Source: NVIDIA Blog

At a glance

- Product
- NVIDIA Vera Rubin NVL72
- Benchmark suite
- MLPerf Training v6.1 and Endpoints v1.0
- Efficiency gain
- >10x throughput per watt on agentic AI
- Scaling efficiency
- ~97% scaling across 256 to 1,024 GPUs
- Announcement date
- November 16, 2026
- Partner submissions
- 20 partner organizations

In this post

NVIDIA debuted its Vera Rubin platform in the latest industry benchmarks, sweeping every MLPerf Training v6.1 test and posting leading scores across post-training and agentic inference according to NVIDIA Blog's report. In its first submission to MLPerf Endpoints, the platform demonstrated more than 10x generational throughput-per-watt gains for agentic artificial intelligence workloads compared to previous hardware.

This breakdown examines the benchmark scores, architectural scaling metrics, and agentic inference data announced during the SC26 conference. It is designed for infrastructure engineers, high-performance computing operators, and enterprise technology leaders evaluating hardware roadmaps for large language model pretraining and agent deployment.

## 01Clean sweep across MLPerf Training v6.1

NVIDIA finished as the only platform submitting verified results across every single benchmark category in MLPerf Training v6.1. The testing suite evaluated large language models, image generation networks, recommender systems, and reinforcement learning post-training pipelines. In this debut run, Vera Rubin NVL72 delivered up to 2x more performance per GPU than the preceding NVIDIA GB300 NVL72 configuration.

The benchmark suite tracks the precise time required for computing infrastructure to train diverse architectures to defined accuracy targets. Performance optimizations continue to rely on the CUDA software stack, allowing systems to register performance gains across identical hardware configurations over extended development cycles.

**Note:** Vera Rubin results in the MLPerf Endpoints division remain provisional and subject to standard peer review verification by the MLCommons Association.

## 02Distributed scaling across thousands of accelerators

Testing on the DeepSeek-V3 benchmark evaluated how Vera Rubin NVL72 scales across clustered configurations within high-density environments. The platform scaled from 256 GPUs up to 1,024 GPUs with approximately 97 percent scaling efficiency. Quadrupling the cluster accelerator count reduced total model training time from sixteen minutes down to four minutes.

Scale testing also included massive GB300 deployments containing 10,240 GPUs, representing the largest single cluster submitted in the v6.1 evaluation round. Microsoft Azure matched this deployment volume in its own testing submission, deploying 10,240 NVIDIA GPUs across GB200 systems.

Interconnect hardware coordinated the distributed cluster across multiple networking tiers. Scale-up communication within rack frames operated across NVLink, while inter-rack data center communication utilized Quantum-X800 InfiniBand alongside Spectrum-X Ethernet networking.

## 03Agentic inference efficiency on MLPerf Endpoints

The MLPerf Endpoints v1.0 evaluation measured system throughput against multi-turn enterprise agentic coding and business workflows. Under the standardized testing division, Vera Rubin NVL72 demonstrated more than 10x throughput per watt compared to GB300 NVL72 systems running the open Kimi K3 agentic model at comparable interactive response latencies.

Advertisement

Tech Pulse Daily

### Get tomorrow's pulse first

Join engineers who read Tech Pulse before stand-up. Free, weekday mornings.

| Platform System | Accelerators | Concurrency | System TPS | Power Draw | E2E Interactivity | 
|---|---|---|---|---|---|
| Vera Rubin NVL72 | 32 | 1,024 queries | 41,414.2 | 100.89 kW | 50.4 tokens/sec/user | 
| GB300 NVL72 | 48 | 48 queries | 2,210.4 | 60.00 kW | 56.2 tokens/sec/user | 

Vera Rubin achieved 41,414.2 tokens per second while sustaining an average end-to-end interactivity of 50.4 tokens per second across 1,024 concurrent queries. By contrast, the GB300 NVL72 test configuration logged 2,210.4 tokens per second with 48 concurrent queries at 56.2 tokens per second interactivity.

## 04Ecosystem partner deployments and independent evaluations

The MLPerf Training round included hardware and software submissions from twenty external partner organizations. Validated implementations came from ASUS, Azure, Cisco, CoreWeave, Crusoe, Dell Technologies, Firmus, Fujitsu, Giga Computing, Google Cloud, HPE, Inventec, Krai, Lambda, Nebius, Oracle, Quanta Cloud Technology, Supermicro, TTA, and the University of Florida.

Independent evaluation suites confirmed the performance margins outside the MLCommons ecosystem. Vera Rubin placed first in evaluations published by SemiAnalysis AgentX and Artificial Analysis AgentPerf. These suites benchmarked full infrastructure utilization covering pretraining, post-training, and tool-calling agentic execution.

Outside machine learning workloads, the architecture addresses general high-performance computing tasks presented at the SC26 conference. The programmable platform interfaces directly with existing open-source libraries and frameworks across commercial and research deployments.

## 05Infrastructure management and software configuration

Cluster administrators manage Rubin nodes and CUDA driver libraries using standard NVIDIA platform utilities. Software stacks configure networking fabrics and runtime environments across automated deployment toolchains.

`nvidia-smi`
System administrators set software profiles and verify accelerator health across installed racks using unified control commands.

`nvlink-audit --status`
Environment scripts and container runtimes pin model execution flags to designated accelerator topologies across connected fabrics.

`export CUDA_VISIBLE_DEVICES=0,1,2,3`
## Frequently asked questions

### What did NVIDIA achieve in MLPerf Training v6.1?

NVIDIA swept every benchmark category in the MLPerf Training v6.1 round, with Vera Rubin NVL72 delivering up to 2x more performance per GPU than GB300 NVL72 systems.

### How did Vera Rubin perform on agentic inference?

In MLPerf Endpoints v1.0 running the Kimi K3 model, Vera Rubin NVL72 delivered over 10x the throughput per watt of GB300 NVL72 at matching interactivity levels.

### How efficiently does the Vera Rubin platform scale across nodes?

On the DeepSeek-V3 benchmark, Vera Rubin scaled from 256 to 1,024 GPUs with roughly 97 percent efficiency, cutting training time from sixteen minutes to four.

## Sources

## Related on Tech Bytes

Advertisement
