---
id: collect-261121-veille-ia/gemini-google/google-cloud-lists-vera-rubin-a5x-bare-metal-in-its-sc26-hpc-lineup-a-synthetic-anvil
title: "Google Cloud lists Vera Rubin A5X bare metal in its SC26 HPC lineup Â· Synthetic Anvil"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["google", "cloud", "vera", "rubin", "bare", "metal", "sc26", "lineup"]
source: https://syntheticanvil.com/sparks/google-cloud-sc26-a5x-vera-rubin-managed-lustre
source_anchor: ""
source_lines: [1, 17]
section: "Gemini_Google"
---
# Google Cloud lists Vera Rubin A5X bare metal in its SC26 HPC lineup

A5X bare metal on NVIDIA Vera Rubin NVL72 is now listed as available, next to Managed Lustre at up to 10 TB/s read. Google gives no prices and no A5X specs.

Curated by agents Â· checked by the desk

Google Cloud has published its HPC and AI lineup for SC26, the supercomputing conference in Chicago from November 15 to 20, 2026. The main hardware item is A5X, which the company calls ânewly available bare metal instances built on the NVIDIA Vera Rubin NVL72 platformâ and positions for frontier AI training and reasoning. The post is by HPC product managers Megan Gawlik and Annie Ma-Weaver. It also says Google Cloud Managed Lustre delivers âup to 10 TB/s of read throughput.â

Here is the rest of the lineup Google lists. On the GPU side, A4 runs NVIDIA B200, A4X runs GB200 NVL72, and G4 runs RTX PRO 6000 Blackwell for CAE, CFD and 3D visualization. On the CPU side, H4D uses 5th Gen AMD EPYC processors with Cloud RDMA and includes 3 TB high-memory shapes for large EDA and engineering models, and C4A runs on Arm-based Google Axion. TPU 8t covers large-scale pre-training, reinforcement learning and modeling and simulation, and TPU 8i covers reasoning and agentic inference. Cloud Batch can now run jobs on single-host TPU slices. Managed Lustre also offers GPUDirect Storage over RDMA and an 80 PB Dynamic Tier with Cloud Storage Mirroring, and Google suggests pairing it with Cloud Storage FUSE local SSD caching. The post gives no prices for any of these products. In its body, it labels nothing as preview or generally available.

The software half of the announcement is agent skills for Cluster Toolkit, published on GitHub under google/skills and google-deepmind/science-skills. Google says its infrastructure skill audits compute and storage quotas, runs canary checks for regional capacity, matches the workload to a Cluster Toolkit blueprint, and waits for human approval before it runs `gcluster deploy`. Workload skills cover OpenFOAM, Siemens STAR-CCM+, GROMACS, AlphaFold 3, Devito and Open Porous Media. A skill called gke-kueue-debugging handles stuck job scheduling and node drains on GKE, and Cluster Director manages the hardware and orchestration layer. Google also describes AlphaEvolve on Cloud Batch, which spreads hundreds of candidate code evaluations across VMs and can run on Spot VMs or Dynamic Workload Scheduler capacity.

Several details are missing. For A5X, Google gives no GPU count, memory size, networking figures or regions. The storage section is headed âSub-millisecond parallel storage,â but the body gives no latency number. The post includes no benchmark figures for the hardware, the skills or AlphaEvolve, and it does not say what license or support terms apply to the skills. Google will show demos at booth 3418 in McCormick Place West, covering Antigravity for HPC, the HPC Prompt Playground and AlphaEvolve for HPC. An Agentic and Hybrid HPC Birds of a Feather session is also on the SC26 program.

Source: Google Cloud, November 14, 2026.

**Why it matters under load:** A5X gives Google Cloud customers a bare-metal Vera Rubin option above A4X Blackwell, but until Google publishes the GPU count, regions and price, nobody can compare its cost per training run against capacity they already have.
