---
id: collect-261121-veille-ia/nvidia-hardware/nvidia-unveils-rubin-hpc-a-50-000-core-simulation-gpu-due-in-2028-fp64-unstated
title: "Nvidia Unveils Rubin HPC, a 50,000-Core Simulation GPU Due in 2028, FP64 Unstated"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: []
keywords: ["nvidia", "rubin", "simulation", "fp64", "hpcwire", "agents", "cores", "core"]
source: https://timesof.ai/2026/11/nvidia-rubin-hpc-50000-cuda-cores-simulation-gpu-2028
source_anchor: ""
source_lines: [1, 21]
section: "NVIDIA_Hardware"
---
Nvidia has spent two GPU generations tilting its silicon toward low-precision AI math. Rubin HPC is its sign that classic scientific simulation still gets its own chip, now pitched as tooling for AI agents rather than for human researchers alone. Announced at the SC26 supercomputing conference on November 16, it is a GPU in the Vera Rubin platform built for simulation and data processing, and it is not expected until 2028. The number HPC buyers most want, double-precision throughput, is the one Nvidia did not give.

## What Nvidia announced#

According to Nvidia, Rubin HPC has "a record number of CUDA cores", more than **50,000** on a single GPU, a sparse-data acceleration capability meant to keep those cores supplied, and seventh-generation NVLink for GPU-to-GPU connections. Nvidia says the design scales data movement and compute together, targeting patterns common to simulation such as sparse matrix operations, stencil computations, sorting and reductions.

The framing is agentic. "For agents tackling science and engineering, faster simulation means faster discovery," said Ian Buck, Nvidia's vice president of hyperscale and HPC. Nvidia says its CUDA-X libraries give agents optimised implementations to call.

## The FP64 question#

HPCwire reads the chip as a recommitment to FP64, the 64-bit math that traditional HPC codes rely on, after Nvidia's last two data centre generations offered little increase in it. It reports Rubin HPC has three times the vector cores of Blackwell and that Nvidia "isn't saying" how much FP64 it will have; HPCwire's own estimate of around 100 teraflops assumes linear scaling with cores and is not an Nvidia figure. For comparison, HPCwire says AMD's MI430X, due in 2027, is rated at 288 teraflops of FP64.

This sits alongside Nvidia's mainstream Vera Rubin parts, whose published memory and NVLink bandwidth Nvidia lowered earlier this month as it moved from targets to production figures.

## Limits#

- Rubin HPC is an announcement of a 2028 product. Core count and features are Nvidia's; no performance, price, power or customer figures were given.
- HPCwire's FP64 estimate is its own extrapolation, and the "three times Blackwell" vector-core figure is as HPCwire reports it.
- Nvidia's claims that faster simulation speeds AI agents' discovery are untested for this chip.

## Sources#
