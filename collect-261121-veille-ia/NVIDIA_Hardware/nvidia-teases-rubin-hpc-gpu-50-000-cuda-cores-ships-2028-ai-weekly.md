---
id: collect-261121-veille-ia/nvidia-hardware/nvidia-teases-rubin-hpc-gpu-50-000-cuda-cores-ships-2028-ai-weekly
title: "Nvidia Teases Rubin HPC GPU: 50,000+ CUDA Cores, Ships 2028 | AI Weekly"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: []
keywords: ["rubin", "nvidia", "cores", "cuda", "more", "nvlink", "ships", "bandwidth"]
source: https://aiweekly.co/alerts/nvidia-teases-rubin-hpc-gpu-50000-cuda-cores-ships-2028
source_anchor: ""
source_lines: [1, 31]
section: "NVIDIA_Hardware"
---
## TL;DR

- Rubin HPC uses 7th-gen NVLink at 3.6 Tb/s, one generation ahead of standard Vera Rubin, targeting interconnect-sensitive scientific workloads.
- Two Rubin HPC dies paired with one Vera CPU yield 200,000+ CUDA cores per tray, a configuration no announced competitor matches on paper.
- cuPhoton.xBeam cuts Event Horizon Telescope analysis from months to 8 minutes, giving national labs a concrete ROI argument two years before the chip ships.

Nvidia used Supercomputing 2026 to tee up a Rubin HPC GPU with 'more than 50,000' CUDA cores, a chip the company says will 'accelerate agentic workflows and simulation for scientists, engineers, and analysts working with large data sets.' The current Rubin tops out at 28,672 CUDA cores.

Tom's Hardware reports Nvidia is framing the part as a 'balanced design,' pairing more compute with faster node-to-node data movement over NVLink 7. The interconnect retains the 3.6 TB/s of per-GPU bandwidth already shipping on NVLink 6, and the piece says the Rubin HPC design likely reuses a four-die layout similar to Rubin Ultra, linked by NVLink-C2C.

Nvidia disclosed little else. HBM capacity, memory bandwidth and native FP64 numbers, the figure traditional HPC buyers scrutinize most, are all missing from the announcement, and Tom's Hardware notes competitors are promoting exactly those metrics at the same conference. It is one more roadmap tease across the 288 Nvidia stories we have tracked over the past 90 days where headline core counts arrive long before the specs HPC buyers actually procure on.

Nvidia says Rubin HPC will arrive sometime in 2028, leaving two procurement cycles between the tease and the silicon.

## What others are reporting

1. 
                
                                  Leads with the software ecosystem: NVLink 7 at 3.6 Tb/s, cuPhoton library launches, Nemotron for Science specs (124B params, 50k chemistry questions), and the IBM/Fermilab quantum codesign details. Rubin HPC accelerates the simulations agents use to test ideas and guide their next steps, helping scientists and engineers explore more possibilities.
2. 
                
                                  Deepest spec comparison: Rubin HPC more than doubles RTX 5090's 21,760 cores, uses 7th-gen NVLink vs 6th-gen on standard Rubin, two dies per Superchip yielding 200,000+ cores per tray. "Faster simulation means faster discovery," said Ian Buck, NVIDIA's VP of hyperscale and high-performance computing.
3. 
                
                                  Only outlet to frame the missing bandwidth as the critical gap: for sparse-matrix and stencil workloads, bandwidth matters more than core count, and buyers cannot evaluate Rubin HPC yet.

          Originally reported by
          **tomshardware.com**
        

Original headline: Nvidia Unveils Rubin HPC GPU With Over 50,000 CUDA Cores at SC26, Shipping 2028
