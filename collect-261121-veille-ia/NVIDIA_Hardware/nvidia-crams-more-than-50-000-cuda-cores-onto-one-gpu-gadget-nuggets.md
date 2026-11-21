---
id: collect-261121-veille-ia/nvidia-hardware/nvidia-crams-more-than-50-000-cuda-cores-onto-one-gpu-gadget-nuggets
title: "Nvidia crams more than 50,000 CUDA cores onto one GPU - Gadget Nuggets"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: ["2026-11-16"]
keywords: ["nvidia", "cores", "rubin", "more", "card", "memory", "cuda", "simulation"]
source: https://gadgetnuggets.com/nvidia-rubin-hpc-gpu-50000-cuda-cores-2028/
source_anchor: ""
source_lines: [1, 31]
section: "NVIDIA_Hardware"
---
Nvidia introduced Rubin HPC on Monday, November 16, 2026, at the Supercomputing 2026 conference in Chicago. The card belongs to the Vera Rubin platform and carries more than 50,000 CUDA cores on a single GPU. Nvidia calls that a company record. It aims at simulation and data work in science, engineering and analytics. Availability is set for 2028, and that year is the only promise Nvidia gives. There is no launch month and no price.

## More than double the cores of today’s Rubin

Today’s Rubin GPU stops at 224 streaming multiprocessors and 28,672 CUDA cores. Rubin HPC roughly doubles that number, on a platform where memory bandwidth has already been cut. Nvidia frames the new part as a science and simulation card rather than a training card, as covered in Nvidia’s announcement of Rubin HPC on HPCwire.

The target workloads eat data. Nvidia names sparse matrix operations, stencil computations, sorting and reduction as the patterns it designed for. Nvidia’s approach is to scale data flow and compute together instead of piling on compute alone, plus a sparse data acceleration path meant to keep the cores fed.

Ian Buck, vice president for hyperscale and HPC at Nvidia, puts the case plainly: “For agents tackling science and engineering, faster simulation means faster discovery. Rubin HPC accelerates the simulations agents use to test ideas and guide their next steps”.

The software half of the pitch is CUDA-X. Nvidia says its libraries hand agents ready-made optimized kernels, and that the cycle of hypothesis, simulation and refinement gets shorter once agents get their answers faster.

## Where the cores come from stays unconfirmed

Nvidia published no memory capacity, no memory bandwidth, no clock speed and no power figure. It also gave no FP64 performance number. Seventh generation NVLink is on the card, but Nvidia added no new bandwidth detail. Earlier figures for NVLink 6 sat at 3.6 TB/s per GPU, and GPU memory prices have already pushed one consumer card out of the market.

The packaging is the open question. Nvidia showed no image of the package and gave no die count. Tom’s Hardware’s report on the core count reads the jump from 28,672 to more than 50,000 cores as a sign that more dies are needed in the package, and points to Rubin Ultra, which links its four dies with NVLink-C2C. Note the split here. The die layout is Tom’s Hardware’s own reading of the numbers. Nvidia confirmed no layout at all.

The FP64 question stayed open on stage as well. Tom’s Hardware reported that Nvidia’s own reporter at SC26 got no answer when asking whether the new chip moves resources toward double precision. AMD and Intel market that strength on their cards the same week.

## Still unknown, and what it means for your next build

- HBM capacity: not given
- Memory bandwidth: not given
- Clock speed and power draw: not given
- FP64 throughput: no figure
- Price and launch date: Nvidia commits to 2028 and nothing more, while memory prices already cost one consumer card its launch

For anyone planning an HPC or AI machine, the honest conclusion is that there is nothing to buy. A card that lists 50,000 cores could still turn out to be two dies, and agentic simulation performance is not for sale before 2028. If your codes lean on double precision, the FP64 silence matters more than the core count. Judge Rubin HPC when Nvidia puts memory, bandwidth and FP64 numbers on the table, and plan today’s builds on hardware that ships now.

## Leave a comment
