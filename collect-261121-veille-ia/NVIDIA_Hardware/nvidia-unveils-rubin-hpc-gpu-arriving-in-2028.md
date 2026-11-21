---
id: collect-261121-veille-ia/nvidia-hardware/nvidia-unveils-rubin-hpc-gpu-arriving-in-2028
title: "Nvidia unveils Rubin HPC GPU, arriving in 2028"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: ["2026-11-16"]
keywords: ["nvidia", "rubin", "says", "siliconangle", "chip", "hardware", "cores", "data"]
source: https://www.ai-news.media/posts/nvidia-unveils-rubin-hpc-gpu-for-science-agents-due-in-2028
source_anchor: ""
source_lines: [1, 48]
section: "NVIDIA_Hardware"
---
## TL;DR

Nvidia announced Rubin HPC at SC26, a new GPU in its Vera Rubin platform built to speed up simulation and data processing. The company says the chip will carry more than 50,000 CUDA cores. Nvidia expects Rubin HPC to be available in 2028.

- **Number:** The new GPU will have more than 50,000 CUDA cores, according to Nvidia.
- **Scale:** SiliconANGLE says Nvidia's current flagship Rubin GPU has over 28,000 CUDA cores.
- **Timeframe:** SiliconANGLE reports Nvidia plans a 2028 shipping date for Rubin HPC.
- **Impact:** Nvidia pitches the chip at researchers who use agents to run simulations.
- **Limit:** Tom's Hardware says Nvidia gave no details on HBM capacity or bandwidth for the chip.

## Nvidia unveils Rubin HPC at SC26

Nvidia says Rubin HPC is meant to speed up simulation work for scientists, engineers and analysts. SiliconANGLE describes Rubin HPC as an upcoming graphics card with far more cores than Nvidia's current flagship.

The debut took place at the SC26 conference in Chicago, SiliconANGLE reports. âFor agents tackling science and engineering, faster simulation means faster discovery,â said Ian Buck of Nvidia in the NVIDIA Blog.

## Core counts, NVLink and sparse data

Nvidia says Rubin HPC will have more than 50,000 CUDA cores, SiliconANGLE reports. SiliconANGLE says the current flagship Rubin GPU has over 28,000 CUDA cores. Tom's Hardware lists today's Rubin GPU at 28,672 CUDA cores and says reaching the higher count would require more dies on a package.

Nvidia describes a balanced design that grows data movement together with compute instead of adding compute alone. Nvidia says the design is meant to keep the cores busy computing instead of idling while they wait for data.

SiliconANGLE explains that sparse data means datasets with many zeros, which projects in biology and astrophysics often generate. Tom's Hardware reports that Rubin HPC GPUs will exchange data over NVLink 7.

Nvidia says its CUDA-X libraries offer agents optimized versions of complex algorithms for simulation and analytics. Nvidia says cuPhoton.xBeam can finish in eight minutes data analysis that historically took a month, SiliconANGLE reports.

## What it means for researchers and engineers

Nvidia says scientists and engineers simulate complex systems first, ahead of committing to experiments or physical prototypes. With agents in the loop, the company says, teams can look at more candidate materials, molecules and designs. Nvidia says that raises demand for GPU accelerated simulations to evaluate the ideas.

SiliconANGLE notes that customers can already speed up scientific workloads with the software tools launched alongside the chip. Nemotron for Science, an AI bundle from Nvidia, pairs a language model with packaged post training workflows, SiliconANGLE says.

Readers planning research budgets can treat the software as the near term story and the chip as a later one.

## What Nvidia has not said about Rubin HPC

Tom's Hardware says Nvidia gave no details on HBM capacity or bandwidth for the chip. Tom's Hardware adds that Nvidia did not say whether the chip shifts compute toward native FP64, which AMD and Intel are promoting this week. HPCwire headlines its coverage with a recommitment to FP64 for the new GPU, which the Tom's Hardware report does not confirm.

SiliconANGLE puts the current NVLink bandwidth at 3.6 terabits per second per graphics card. Tom's Hardware instead cites 3.6 TB/s per GPU for NVLink 6, which past disclosures indicate the newer link retains. The two outlets use different units for the bandwidth figure, and the coverage does not settle which one is correct.

Tom's Hardware says the chip might use a layout like that of Rubin Ultra, a four die GPU. Nvidia did not reveal more about the chip's physical layout with the announcement, Tom's Hardware says. Beyond a 2028 availability window, none of the coverage gives pricing or named buyers for Rubin HPC.

## Sources

- SiliconANGLE, At SC26, Nvidia debuts new Rubin HPC chip, scientific research libraries - SiliconANGLE (2026-11-17): https://siliconangle.com/2026/11/16/nvidia-debuts-new-rubin-hpc-chip-scientific-research-libraries/
- NVIDIA Blog, NVIDIA Announces Rubin HPC , A New GPU to Accelerate Agents for Science, Engineering and Analytics (2026-11-16): https://blogs.nvidia.com/blog/rubin-hpc/
- Tom's Hardware, Nvidia announces Rubin HPC GPU with 'record number of CUDA cores' at Supercomputing 2026 , new chip accelerate (2026-11-16): https://www.tomshardware.com/pc-components/gpus/nvidia-announces-rubin-hpc-gpu-with-record-number-of-cuda-cores-at-supercomputing-2026-new-chip-accelerates-agentic-hpc-workloads-arrives-in-2028
- HPCwire, NVIDIA Announces Rubin HPC, a New GPU to Accelerate Agents for Science, Engineering and Analytics - HPCwire (2026-11-16): https://www.hpcwire.com/off-the-wire/nvidia-announces-rubin-hpc-a-new-gpu-to-accelerate-agents-for-science-engineering-and-analytics/
