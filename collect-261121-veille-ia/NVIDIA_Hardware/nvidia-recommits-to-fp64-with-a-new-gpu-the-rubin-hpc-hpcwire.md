---
id: collect-261121-veille-ia/nvidia-hardware/nvidia-recommits-to-fp64-with-a-new-gpu-the-rubin-hpc-hpcwire
title: "Nvidia Recommits to FP64 with a New GPU, the Rubin HPC - HPCwire"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: ["2026-11-16"]
keywords: ["fp64", "nvidia", "teraflops", "rubin", "cores", "tensor", "gpus", "hpcwire"]
source: https://www.hpcwire.com/2026/11/16/nvidia-recommits-to-fp64-with-a-new-gpu-the-rubin-hpc/
source_anchor: ""
source_lines: [1, 17]
section: "NVIDIA_Hardware"
---
Nvidia today unveiled the Rubin HPC, a new GPU that features 3X the vector cores of Blackwell. The chip shows a recommitment to FP64 on Nvidia’s part after abandoning increases in double-precision 64-bit floating point math in its last two generation of data center GPUs.

The Rubin HPC will feature more than 50,000 CUDA cores, which Nvidia says is a record.

It’s all about having the right tool for the job, said Dion Harris, senior director of HPC and AI hyperscale infrastructure solutions at Nvidia.

“We’ve been on this, this journey of looking at how you can leverage this mix of tools,” Harris told *HPCwire* in an interview at SC26 today. “You might need a very specific GPU that’s going to do great at running simulations, FP4, FP32, a broad range of precisions.”

While Nvidia has said that it’s not abandoning 64-bit computing, its most recent GPUs have been nearly devoid of FP64 capability. The company’s Hopper H100 and the H200 follow-on both featured 34 teraflops of FP64 and 67 teraflops of FP64 Tensor Core performance. (Tensor Cores are specialized engines on the GPU that accelerate matrix multiplication and support mixed-precision workloads.) That was a big increase from the 9.7 teraflops of FP64 and 19.5 teraflops of FP64 Tensor Core performance that Nvidia delivered with the Ampere A100 GPU in 2020.

When Nvidia unveiled the Blackwell architecture in 2024, 64-bit computing was lower, with just 30 teraflops of FP64 and FP64 Tensor Core performance in the B100. The B200 still didn’t match the H200 in overall FP64 Tensor Core performance, making the older (and cheaper) H100s and H200s a superior choice for traditional HPC workloads. The new Rubin GPUs deliver 33 teraflops of native FP64.

Instead of adding more FP64 and FP32 to its chips, Nvidia has been increasing the lower precision capabilities that are needed in AI. It also has talked about how Ozaki emulation can be used to get FP64-like precision from lower-precision cores.

AMD capitalized on its competitor’s move by showcasing a new line of GPUs with significantly more native FP64, which the company said its customers demanded. While Ozaki scheme may work in some situations, it can’t be used in all cases, and can also give the wrong answer in some situations, the company warned. The MI430X, which is due out in 2027, will feature 288 teraflops of FP64.

Nvidia isn’t saying how much FP64 the Rubin HPC will have when it ships in 2028. If it scales linearly with cores, then it would likely be around 100 teraflops.
