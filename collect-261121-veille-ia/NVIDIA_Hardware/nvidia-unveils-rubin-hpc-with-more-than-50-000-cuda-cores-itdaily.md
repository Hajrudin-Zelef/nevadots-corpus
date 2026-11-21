---
id: collect-261121-veille-ia/nvidia-hardware/nvidia-unveils-rubin-hpc-with-more-than-50-000-cuda-cores-itdaily
title: "Nvidia unveils Rubin HPC with more than 50,000 CUDA cores - ITdaily"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: []
keywords: ["nvidia", "rubin", "cuda", "cores", "also", "more", "chip", "researchers"]
source: https://itdaily.com/news/datacenter/nvidia-rubin-hpc-cuda-cores/
source_anchor: ""
source_lines: [1, 33]
section: "NVIDIA_Hardware"
---
**At SC26 in Chicago, Nvidia unveiled the Rubin HPC, a GPU for scientific simulations with more than 50,000 CUDA cores. The chip arrives on the market in 2028, together with new software libraries and an AI package for researchers.**

Nvidia introduced Rubin HPC at the SC26 supercomputing conference. The chip is part of the Vera Rubin platform and is meant to speed up simulations and data processing for science, engineering and analytics. The standard Rubin GPU has 28,672 CUDA cores, so the new variant has almost twice as many.

We note that Nvidia has said nothing yet about the physical design. The core count suggests that the chip, like the Rubin Ultra, consists of four dies, but that has not been confirmed.

## What Rubin HPC is meant to do

Rubin HPC gets a new generation of the NVLink interconnect, the seventh. The current version moves 3.6 terabits per second per GPU. There will also be a feature for sparse data, datasets with many zeros. These are common in biology and astrophysics, for example when a telescope uses zeros to represent parts of space without visible stars.

Ian Buck, vice president of hyperscale and high-performance computing at Nvidia, says: “For AI agents tackling scientific and engineering problems, faster simulation means faster discoveries.” According to him, Rubin HPC speeds up the simulations that agents use to test ideas and decide their next steps.

Nvidia is staying quiet for now on memory capacity and bandwidth. FP64 performance, which matters for classic scientific calculations, also remains unknown. HPCwire points out that in recent generations Nvidia added little extra FP64 and mainly strengthened lower precisions. AMD is responding with its MI430X, which arrives in 2027 with 288 teraflops of FP64.

Dion Harris, senior director of HPC and AI hyperscale infrastructure at Nvidia, says in an interview that it is about the right tool for each task.

## Software for astronomy and biology

The chip comes with new libraries in CUDA-X. With cuPhoton.xBeam, astrophysicists analyse data from the Event Horizon Telescope. According to Nvidia, an analysis that used to take a month now takes eight minutes. The sister library cuPhoton.xLens simulates the plasma in fusion reactors.

For biology, Nvidia is making cuSpa open source. The library speeds up calculations with cell polygons, which researchers use to study the structure of cells and how they interact. At Memorial Sloan Kettering Cancer Center, this reportedly runs 900 times faster than on CPUs. The existing rapids-singlecell is now also available as a container.

Nemotron for Science bundles the open model Nemotron 3.5 Super VL with ready-made workflows for post-training. The model has 124 billion parameters and activates roughly a tenth of them at a time. A dataset with nearly 50,000 chemistry questions is used to train and evaluate models.

## Vera racks and quantum platform

For CPU-heavy workloads, there will be a rack with 192 Vera CPUs, about 300 TB of memory and 1 PB of storage. Dell, HPE, Lenovo and Supermicro will offer these racks from early 2027. The German Climate Computing Centre DKRZ chose Vera for its HLRE-5 supercomputer, which is due to enter service in early 2028.

Nvidia also announced a partnership with IBM and the American Fermilab for a modular quantum platform. It combines DGX B200 systems with IBM quantum hardware, so that researchers can develop algorithms for qubit error correction. CUDA-Q also received new building blocks and an algorithm that reduces qubit errors.

## Availability

Rubin HPC arrives in 2028. There are no list prices or memory and FP64 specifications yet. Until then, researchers can get started with the software libraries and Nemotron for Science.
