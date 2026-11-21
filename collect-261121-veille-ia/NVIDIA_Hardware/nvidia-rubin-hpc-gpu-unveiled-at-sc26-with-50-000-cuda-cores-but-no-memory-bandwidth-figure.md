---
id: collect-261121-veille-ia/nvidia-hardware/nvidia-rubin-hpc-gpu-unveiled-at-sc26-with-50-000-cuda-cores-but-no-memory-bandwidth-figure
title: "Nvidia Rubin HPC GPU Unveiled at SC26 With 50,000+ CUDA Cores but No Memory Bandwidth Figure"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: []
keywords: ["nvidia", "rubin", "memory", "sparse", "bandwidth", "hpcg", "linpack", "speed"]
source: https://www.techtimes.com/articles/330881/20261117/nvidia-rubin-hpc-gpu-unveiled-sc26-50000-cuda-cores-no-memory-bandwidth-figure.htm
source_anchor: ""
source_lines: [1, 93]
section: "NVIDIA_Hardware"
---
Nvidia announced Rubin HPC on Monday at the SC26 supercomputing conference in Chicago: a new GPU in its Vera Rubin family with more than 50,000 CUDA cores, hardware for sparse data acceleration and seventh-generation NVLink, due in 2028.

Nvidia has not published an FP64 rate, a memory bandwidth figure or a power figure, and bandwidth is the limit on memory-bound kernels, the category that includes the sparse matrix, stencil, sorting and reduction work Rubin HPC targets according to Nvidia.

That leaves engineers who plan simulation budgets for 2028 without the numbers that decide speed. A core count does not show how fast a code will run, and neither does an FP64 peak when the code waits on memory. On TOP500's June 2026 list, El Capitan ran the memory-bound HPCG benchmark at about 1% of its LINPACK speed, by TechTimes' calculation.

### Sparse Data Acceleration Targets Memory Waits in Four Kernel Types Nvidia Named

Nvidia says Rubin HPC scales data movement together with compute instead of adding compute alone, so CUDA cores spend their time computing rather than waiting for data. The company names four recurring patterns it aims to speed up: sparse matrix operations, stencil computations, sorting and reductions. Nvidia has not explained how the sparse data acceleration works.

A sparse matrix holds mostly zeros, so software stores only the nonzero entries. The common compressed sparse row format keeps three arrays, holding the values, their column indices and row pointers. Multiplying such a matrix by a vector means reading those index arrays and then fetching vector entries, and irregular matrix structures cause execution and memory divergence on GPUs. Iterative solvers such as conjugate gradient and GMRES depend on fast sparse matrix-vector products.

Stencil codes update each cell of a grid from its neighbors, and most are memory bound, meaning they make many memory accesses for each calculation. The roofline model introduced in 2009 by Samuel Williams, Andrew Waterman and David Patterson explains why that matters: a kernel's arithmetic intensity, the operations it performs per byte moved, decides whether memory bandwidth or peak compute caps its speed. Below the point where the two limits meet, memory bandwidth is the cap.

GPU sparse math is not new in software. Nvidia's Michael Garland and Google's Nathan Bell were named winners of the SC26 Test of Time Award for a 2009 paper that mapped sparse matrix-vector multiplication onto GPUs and reached high memory bandwidth utilization on a GeForce GTX 285. Nvidia has not said what Rubin HPC's hardware adds beyond that kind of software work.

### El Capitan Reaches About 1% of LINPACK Speed on HPCG While Fugaku Reaches About 3.6%

TOP500 publishes the High-Performance Conjugate Gradient benchmark, HPCG, next to its LINPACK ranking, and says HPCG reflects data-intensive application patterns rather than raw floating-point throughput. On the June 2026 list, El Capitan posted 1.809 exaflops on LINPACK and 17.41 petaflops on HPCG, while Fugaku posted 442 petaflops and 16.00 petaflops. By TechTimes' calculation, that is about 1% of LINPACK speed for El Capitan and about 3.6% for Fugaku.

The rankings reorder: Fugaku ranks ninth on LINPACK but third on HPCG. The gap shows how much arithmetic capacity the leading machines leave idle on memory-bound work, which is the waiting Nvidia says Rubin HPC is built to cut.

HPCG is one benchmark, not any specific application, and its score and the LINPACK score are measured separately, so the ratio is a rough gauge rather than a rule. Nvidia has not cited HPCG or any other benchmark for Rubin HPC.

### Does Rubin HPC Have Native FP64? Nvidia Has Not Said

Nvidia's announcement gives no FP32 or FP64 performance, so no theoretical double-precision peak exists for Rubin HPC. Double precision stores numbers in 64 bits, about 15 to 17 significant decimal digits, and Nvidia says its standard Vera Rubin platform provides native FP64 capabilities for simulations that need the highest accuracy. Hardwareluxx lists standard Rubin at about 33 TFLOPS of native FP64 (preliminary) and about 200 TFLOPS through Tensor Core emulation that is not native, the B300 at about 1.25 TFLOPS and the B200 at about 37 TFLOPS. AMD's upcoming Instinct MI430X is listed at a preliminary 288 TFLOPS.

Early reports point in different directions. HPCwire's headline frames Rubin HPC as Nvidia recommitting to FP64. Tom's Hardware coverage reports that Nvidia did not say whether the part shifts compute toward native FP64, and Nvidia's own announcement gives no rate.

Researchers disagree about how much native FP64 matters. Satoshi Matsuoka's arXiv paper argues that native FP64 hardware is not essential for the matrix-dominated kernel classes he surveyed, because FP8 tensor-core operations combined with the Ozaki Scheme II can deliver FP64-grade accuracy, though the paper describes FP8 as a candidate approach and keeps a native-FP64 fallback. He reports that, at the engineered bound of his model, nearly every kernel class reaches the memory roof, meaning bandwidth rather than arithmetic sets its speed, with large dense matrix multiplication and the 3-D FFT as exceptions. Nvidia's counterargument, as TechTimes reported from HPCwire, is that more native FP64 would not speed up applications bottlenecked by registers, caches and HBM.

That is why FP64 alone cannot settle whether Rubin HPC speeds up a given simulation. If a code sits on the memory roof, bandwidth sets its speed, and Nvidia has published no bandwidth figure.

### How Many CUDA Cores Does Rubin HPC Have Compared With B300 and Standard Rubin?

| GPU | CUDA cores | Source | 
|---|---|---|
| RTX 5090 | 21,760 | Notebookcheck RTX 5090 count | 
| B300 (Blackwell Ultra) | 20,480 | Hardwareluxx B300 specifications | 
| Standard Rubin | 28,672 | Techstrong standard Rubin figure | 
| **Rubin HPC** | **More than 50,000** | Nvidia Rubin HPC announcement | 

At the 50,000 floor, Rubin HPC has at least 2.44 times the B300's core count and 1.74 times standard Rubin's, by TechTimes' arithmetic. Because Nvidia gave only a floor, those ratios are minimums.

Notebookcheck's headline compares Rubin HPC with the RTX 5090 at nearly 2.5 times the cores. At the floor the ratio is 2.30, and 2.5 would require about 54,400 cores.

Core counts also compare poorly across generations, and Wikipedia's CUDA entry notes that the term "CUDA core" is deprecated in favor of the thread. Nvidia has not said how the cores are organized or how many dies the GPU uses. Standard Rubin already combines two reticle-limited dies and 224 streaming multiprocessors. TechTimes reported in July that SemiAnalysis said Nvidia canceled a four-die Rubin Ultra, citing manufacturing constraints, and Nvidia had not commented at the time. How one GPU reaches more than 50,000 cores in 2028 is therefore an open engineering question.

### Seventh-Generation NVLink Arrives With No Bandwidth Figure, Against 3.6 TB/s on NVLink 6

Nvidia's announcement names seventh-generation NVLink for GPU-to-GPU connectivity and gives no bandwidth figure, and press coverage shortens the name to NVLink 7.

The baseline is steep. Per-GPU NVLink bandwidth rose from 900 GB/s on NVLink 4 in Hopper to 1,800 GB/s on NVLink 5 in Blackwell to 3.6 TB/s bidirectional on NVLink 6 in Vera Rubin NVL72, where nine NVLink 6 switches provide 260 TB/s across the rack. Whether NVLink 7 continues that doubling is unpublished. Multi-GPU simulation codes that exchange boundary data between GPUs depend on that link, and larger GPU counts need a switch fabric rather than direct links.

### Vera CPU Racks From Four OEMs Are Due in Early 2027, Roughly One Year Before Rubin HPC

Nvidia paired the GPU with CPU news: a liquid-cooled rack with 192 Vera CPUs, about 300 TB of memory, 1 PB of storage and over 200 TB/s of aggregate peak memory bandwidth, due from Dell, HPE, Lenovo and Supermicro starting in early 2027. In an OpenFOAM test, Nvidia reported 1.9 times the performance and 2.4 times the performance per watt of x86 systems, vendor-reported figures.

Nvidia also introduced Nemotron for Science resources and expanded CUDA-X libraries. The company ties Rubin HPC to agentic AI in research, where software agents propose candidate materials, molecules or designs and simulations check them. Ian Buck, Nvidia's vice president of hyperscale and high-performance computing, said, "For agents tackling science and engineering, faster simulation means faster discovery."

### Four Missing Specs Decide Whether Rubin HPC's Core Count Becomes Simulation Speed

The standard Vera Rubin platform is already in production: Jensen Huang confirmed full production on June 1, and CoreWeave announced production availability on September 30. Rubin HPC is due roughly two years after that generation, so its specifications can still change.

Four numbers will show whether the core count converts to simulation speed:

- **FP64 and FP32 rates:** whether any of the cores handle double precision natively, and at what speed; Hardwareluxx found no figures.
- **Memory capacity and bandwidth:** Notebookcheck warns that standard Rubin's 288 GB of HBM4 and up to 22 TB/s should not be assumed to apply to Rubin HPC.
- **NVLink bandwidth:** the seventh-generation figure that sets multi-GPU scaling, which Nvidia's announcement omits.
- **Power draw:** Nvidia has not disclosed power consumption.

Everything so far comes from Nvidia's announcement and coverage of it, and no third-party measurements of Rubin HPC exist.

Rubin HPC reads as a bet that moving scattered data efficiently matters more than adding arithmetic for climate, engineering, genomics and risk codes. TOP500's HPCG results show why the bet is plausible, and the FP64 gaps in Hardwareluxx's comparison show why precision is still a question. Until Nvidia publishes bandwidth, FP64 rates and power, and independent groups measure real kernels, more than 50,000 cores is a count rather than a forecast, so buyers should ask for bandwidth in writing and test their own codes.

## Frequently Asked Questions

### Is Rubin HPC the same GPU as standard Rubin?

No. Nvidia describes Rubin HPC as a new Vera Rubin platform GPU for simulation and data processing in science, engineering and analytics. Notebookcheck says it targets general-purpose GPU workloads such as sparse matrix calculations, stencils, sorting and reductions rather than mainly AI training and inference.

### Will higher FP64 speed make my simulation faster?

Only if arithmetic limits the code. In the roofline model, a kernel in the bandwidth-limited region is capped by memory bandwidth, so extra arithmetic rate does not raise its speed. Measure your code's arithmetic intensity, the operations per byte moved, and ask vendors for memory bandwidth alongside FP64 rates.

### When can labs buy Rubin HPC?

Nvidia expects availability in 2028 and has published no pricing or system configurations.

### How does Rubin HPC compare with AMD's Instinct MI430X?

The comparison is not possible yet. AMD's MI430X is listed at a preliminary 288 TFLOPS of FP64, and TechTimes reported 432 GB of HBM4 and 23.3 TB/s of memory bandwidth for the chip, with LUMI-AI deployment scheduled for the second half of 2027. Nvidia has published none of those figures for Rubin HPC.
