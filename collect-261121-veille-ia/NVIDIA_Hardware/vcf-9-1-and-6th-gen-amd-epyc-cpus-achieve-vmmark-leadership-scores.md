---
id: collect-261121-veille-ia/nvidia-hardware/vcf-9-1-and-6th-gen-amd-epyc-cpus-achieve-vmmark-leadership-scores
title: "VCF 9.1 and 6th Gen AMD EPYC CPUs Achieve VMmark Leadership Scores"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: ["2026-11-18"]
keywords: ["vmmark", "epyc", "server", "cpus", "poweredge", "performance", "generation", "socket"]
source: https://blogs.vmware.com/cloud-foundation/2026/11/18/vcf-9-1-and-6th-gen-amd-epyc-cpus-achieve-vmmark-leadership-scores/
source_anchor: ""
source_lines: [1, 22]
section: "NVIDIA_Hardware"
---
We’re excited to announce the first VMmark benchmark results using new AMD EPYC™ 9006 Series Server CPUs. Submitted by Dell Technologies across 1-socket PowerEdge R9815 and 2-socket PowerEdge R9825 configurations, these two server platforms set a number of records in both performance and power-performance: see the VMmark Results page for more information.

Here are some of the highlights:

- The PowerEdge R9815 and R9825 lead the VMmark 1-socket and 2-socket server performance categories, respectively.
- They also have the top-ranked VMmark Performance-Per-Kilowatt (PPKW) scores in both **Server Power-Performance** and**Server and Storage Power-Performance** categories, which were added in VMmark 4.1. For more information, see: Introducing VMmark 4.1: Enhanced Power Efficiency Benchmarking for Private Cloud Infrastructure
- These systems are powered by new 6th Generation AMD EPYC 9996 “Venice” CPUs featuring 256 cores, 512 threads, and 1024 MB L3 cache.
- By enabling the latest CPU architectures, Broadcom, through VMware Cloud Foundation, brings each new CPU generation to private cloud infrastructure with scale and efficiency you can measure.

Let’s compare a 5th Generation AMD EPYC “Turin” VMmark result to one on the new 6th Generation AMD EPYC “Venice” platform to see how performance scales across generations:

| **VCF Version** | **Server Description** | **Processor Model** | **Total Hosts** | **Total Cores** | **VMmark 4.1 Score** | 
| 9.1.2 | Dell PowerEdge R9815 | AMD EPYC 9996 | 2 | 512 | 5.42 @ 5.8 tiles | 
| 9.1.0 | Dell PowerEdge R7715 | AMD EPYC 9845 | 2 | 320 | 3.18 @ 3.8 tiles | 

VCF 9.1.2 enables support for these new CPUs, but performance across 9.1 maintenance releases remains virtually unchanged. The significant score jump is driven entirely by the new hardware generation.

A 60% increase in core count translates directly into a 70% higher VMmark score. By sustaining 5.8 tiles (121 VMs) compared to the R7715’s 3.8 tiles (77 VMs)—where each tile simulates a realistic mix of modern data center workloads—the R9815 drives a 57% increase in workload capacity. For private cloud environments, this means organizations can handle significantly higher compute demand while consolidating onto fewer physical servers during a hardware refresh.

Here are some resources to learn more:

These comparisons are based on all VMmark results published as of 11/10/2026. VMmark is a free benchmarking tool provided as-is by Broadcom and used by partners and customers to measure the performance, scalability, and power consumption of virtualization platforms.
