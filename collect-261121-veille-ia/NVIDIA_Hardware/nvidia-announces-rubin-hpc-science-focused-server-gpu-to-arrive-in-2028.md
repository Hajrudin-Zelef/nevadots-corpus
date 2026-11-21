---
id: collect-261121-veille-ia/nvidia-hardware/nvidia-announces-rubin-hpc-science-focused-server-gpu-to-arrive-in-2028
title: "NVIDIA Announces Rubin HPC: Science-Focused Server GPU to Arrive in 2028"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: []
keywords: ["rubin", "chip", "nvidias", "server", "nvidia", "cores", "announcement", "market"]
source: https://www.servethehome.com/nvidia-announces-rubin-hpc-science-focused-server-gpu-to-arrive-in-2028/
source_anchor: ""
source_lines: [1, 54]
section: "NVIDIA_Hardware"
---
Adding to a busy SC26 conference, NVIDIA has announced that they are developing a new GPU that will be targeted specifically at high-performance computing workloads. Aptly named the Rubin HPC, the future chip will be an HPC optimized version of the company’s Rubin GPU architecture. It would mark NVIDIA’s first significant HPC-class part since the Hopper generation almost 4 years ago.

This week’s announcement comes amidst a broader degree of soul searching that has been taking place over the part couple of years about the future of classic HPC workloads and the hardware that will run them. With commercial GPU server demand shifting over to AI-related tasks and low precision math over the last half-decade, server GPU designs have been favoring large arrays of tensor cores and other design choices that play better to AI needs. This has left traditional science and engineering users with fewer options than they have previously enjoyed, as the bulk of the prior decade saw server GPU designs treat HPC as a first-class citizen, if not a top customer.

Consequently, NVIDIA’s announcement of a dedicated HPC chip is a significant one. After the company all but sat out the HPC market with its Blackwell generation of parts, there was a question whether the leader of the AI revolution would even bother with the smaller HPC market. The answer to that turns out to be an unambiguous “yes,” though not until 2028 when the Rubin HPC chip is released. Nonetheless, it is a clear signal that NVIDIA does not intend to permanently secede the HPC (or supercomputer) market to rivals like AMD and Intel, who have been splitting up the market in recent years.


## Rubin HPC: 50K+ CUDA Cores, Coming In 2028

With NVIDIA’s Rubin HPC chip not slated to arrive until 2028, this week’s announcement from the company (and a very visible VP of Hyperscale and HPC, Ian Buck) is in effect a teaser with more details to come. The company only had a few key details to share about the chip for this week’s announcement.

The headline news here is that NVIDIA’s future HPC chip will over more than 50,000 CUDA cores – NVIDIA’s ALU arrays for classic vector/SIMD processing. This is around twice as many CUDA cores as NVIDIA’s current flagship Rubin SoC, and it is of great importance to the HPC market as few HPC workloads can make heavy use of matrix multiplication and NVIDIA’s tensor cores. An ideal HPC processor needs to be a great vector processing machine and not (just) a tensor processing machine, so the announcement of a large number of CUDA cores underscores just what kind of GPU this will be.

Painting broader strokes about the design of the rest of the GPU at this time, NVIDIA is not sharing any more details about expected performance or the memory subsystem for the HPC chip. Instead, the company is touting a “balanced design” that emphasizes growing compute and data movement capabilities together, rather than jump ramping up compute throughput on its own.

This is (presumably) a reference to the chip’s other marquee feature, which is NVLink 7 support. NVIDIA’s current Rubin server GPU is on NVLink 6; in fact, it is the first chip to offer it. Which means that Rubin HPC’s scale-up networking technology will be a generation beyond that when it launches in 2028.

Reading between the lines a bit, the high-level specifications for Rubin HPC seem to be more closely tied to NVIDIA’s mid-generation refresh part for the Rubin lineup, Rubin Ultra. With it not set to be available for another year, there is only a paucity of information available on that chip as well, but most notably we already know that it is going to be the first chip to ship with NVLink 7 support. Which lends credit to the idea that the 2028 Rubin HPC chip following it will be closer to a Rubin Ultra derivative than it will be of the original Rubin server chip.

That 2028 timeframe also lines up with giving Rubin HPC access to newer HBM4e memory, rather than the HBM4 used on the current Rubin server chip.

Past that, NVIDIA has also included a render of the chip in their SC26 presentation as well as on their newsroom article, which showcases a chip that looks very similar to their current Rubin GPU renders. NVIDIA chip renders are often more aspirational or illustrative than technical this far out, but if it is at least physically accurate then we are looking at 8 stacks of HBM spread out across multiple dies, the same as the current Rubin server chip.

Finally, for this week’s announcement NVIDIA has very briefly touted what they are calling a “sparse data acceleration” capability to keep the GPU cores fed, though with no other details on how this might work. The company’s tensor cores have long supported sparse matrices, though this would seem to be something beyond that, especially with the emphasis on vector math and CUDA cores.

| **NVIDIA HPC Accelerator Specification Comparison** |  |  |  |  |  | 
|  | **Rubin HPC** | **B200** | **H200** |  |  | 
| **CUDA Cores** | >50000 | 18944 | 16896 |  |  | 
| **Tensor Cores** | Fewer? | 592 | 528 |  |  | 
| **Memory Clock** | HBM4E? | 8Gbps HBM3E | 6.4Gbps HBM3 |  |  | 
| **Memory Bus Width** | 2x 8192-bit? | 2x 4096-bit | 6144-bit |  |  | 
| **Memory Bandwidth** | TBD | 8TB/sec | 4.8TB/sec |  |  | 
| **VRAM** | TBD | 192GB | 141GB |  |  | 
| **FP32 Vector** | TBD | 75 TFLOPS | 67 TFLOPS |  |  | 
| **FP64 Vector** | A Whole Lot | 37 TFLOPS | 34 TFLOPS |  |  | 
| **FP64 Tensor** | TBD | 37 TFLOPS | 67 TFLOPS |  |  | 
| **INT8/FP8 Tensor** | TBD | 9 P(FL)OPS | 3.96 P(FL)OPS |  |  | 
| **Interconnect** | NVLink 7 | NVLink 5 18 Links (1800GB/sec) | NVLink 4 18 Links (900GB/sec) |  |  | 
| **GPU** | Rubin HPC | B200 | GH100 |  |  | 
| **Transistor Count** | ? | 208B (2x104B) | 80B |  |  | 
| **Manufacturing Process** | TSMC 3nm? | TSMC 4NP | TSMC 4N |  |  | 
| **Architecture** | Rubin | Blackwell | Hopper |  |  | 

In lieu of specific performance figures, NVIDIA is at least emphasizing that Rubin HPC will be applicable to a wide range of precisions, from NVFP4 all the way up to FP64. While FP64 (and to a lesser extent, FP32) are the biggest focuses for classic HPC workloads, more recent software efforts have looked at lower precision math as well, if only to account for how GPU hardware has changed over the years. While it was always extremely unlikely that NVIDIA would remove low precision math format support entirely, if only for binary compatibility reasons, the emphasis on supporting the full range of precisions implies that we should not expect to see NVIDIA too heavily compromise them, either.

Still, the top line figure to watch here is going to be FP64 performance, both for vector and tensor operations. NVIDIA has essentially plateaued at a bit under 40 TFLOPS for FP64 since Hopper (with Blackwell Ultra being especially poor here at less than 2 TFLOPS), so a fresh chip with a much larger number of CUDA cores would give NVIDIA the means to significantly ramp up their FP64 performance, depending in large part on just how many FP64 CUDA cores they bake into the architecture. And for that matter, how much die space they take away from tensor cores to free up the space for that FP64 hardware.

## Final Words

Wrapping things up, the announcement of their Rubin HPC GPU means that NVIDIA is going to be firmly putting its hat back into the ring for HPC after something of an absence over the last few years. With the company so heavily focused on AI performance with the Blackwell generation, the Rubin generation marks a chance to reset their position in the market, not to mention to take advantage of the technologies they have developed for it. NVIDIA will not be without competition, however, as AMD is going to be launching their HPC-focused Instinct MI430X accelerator in 2027, and Intel is preparing their PCIe-based GPU D-Series. Which means that for NVIDIA, they will have a fight on their hands if they want to retake a large portion of the HPC market or its close cousin, the classic supercomputer market.

nvidia can just fk right off imo

i hope they get torn apart and broken up at this point; they’re a disgusting company.
