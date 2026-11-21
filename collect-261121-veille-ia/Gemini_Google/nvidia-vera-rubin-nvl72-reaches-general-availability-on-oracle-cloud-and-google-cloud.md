---
id: collect-261121-veille-ia/gemini-google/nvidia-vera-rubin-nvl72-reaches-general-availability-on-oracle-cloud-and-google-cloud
title: "NVIDIA Vera Rubin NVL72 reaches general availability on Oracle Cloud and Google Cloud"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["vera", "cloud", "oracle", "rubin", "google", "nvidia", "nvl72", "available"]
source: https://geekbyte.com/post/nvidia-vera-rubin-cloud-general-availability/
source_anchor: ""
source_lines: [1, 63]
section: "Gemini_Google"
---
NVIDIA’s Vera Rubin rack-scale systems are now generally available on two large clouds. Oracle Cloud Infrastructure (OCI) declared them available on 27 October 2026, and Google Cloud followed with its A5X bare-metal instances on 6 November.

Neither cloud has published prices, and both route capacity requests through account teams. Oracle has not named regions for its Vera CPU instances, and Google lists a single zone.

## The hardware

A Vera Rubin NVL72 is one rack that combines 72 Rubin GPUs with 36 Vera CPUs. Vera is an Arm CPU with 88 custom Olympus cores, which Google’s bare metal docs describe as Armv9.2.

NVIDIA’s spec page gives each Rubin GPU 50 petaflops of NVFP4 inference, footnoted as a sparse figure. Compare it only with Blackwell numbers at the same precision and sparsity.

The NVLink 6 bandwidth figures have changed since launch. NVIDIA’s NVLink page now lists 3 TB/s per GPU, marked preliminary. The January launch release said 3.6 TB/s, and Google’s A5X docs still list 3,600 GBps. We have not seen a measured figure on either cloud.

Against GB200 NVL72, NVIDIA claims one-tenth the cost per million tokens running Kimi-K2-Thinking with 32K input and 8K output tokens. That is a vendor number, so plan to test your own model.

## Oracle: generally available on 27 October

At Oracle AI World, NVIDIA and Oracle announced “the general availability of NVIDIA Vera Rubin NVL72 and NVIDIA Vera CPUs on OCI” (joint release). OCI Superclusters pair the racks with Oracle Acceleron networking, Spectrum-X Ethernet and BlueField-4 data processing units (DPUs). The release names no shapes.

The Vera CPU instances are aimed at agent work, such as tool calls and code execution. OpenAI is named as an early-access customer.

Read the GA label for the CPU instances carefully. At launch, Oracle’s Vera CPU announcement said they would reach select regions “this quarter”, and named none. Confirm your region and a delivery date before you plan around them.

Oracle also says Acceleron “provides hardware-based isolation for workloads running untrusted, AI-generated code.” Ask Oracle exactly what that covers before you rely on it.

## Google Cloud: A5X generally available on 6 November

Google introduced A5X at Google Cloud Next on 22 April 2026. Its GTC Berlin post now carries a note dated 6 November 2026: “A5X Instances are now generally available.”

A5X is bare metal, meaning you rent a whole physical server rather than a virtual machine. There is one shape, `a5x-highgpu-4g-metal`: 4 Rubin GPUs, 1,152 GB of HBM4, 176 vCPUs, 1,440 GB of memory and 48,000 GiB of Local SSD.

Eight ConnectX-9 SuperNICs carry up to 6,400 Gbps of GPU-to-GPU traffic. Two Titanium NICs add 800 Gbps for storage and management. The only listed zone is `us-west1-b` in The Dalles, Oregon.

The terms are strict:

- Capacity comes through future reservations for capacity blocks. Spot VMs, Flex-start VMs and sole-tenancy are not supported.
- “The Compute Engine Service Level Agreement (SLA) doesn’t apply to the A5X machine series.”
- Linux on Arm only, with OS images that support the IDPF network driver. The boot disk must be Hyperdisk Balanced, and Persistent Disk is not supported.
- You patch the OS image and GPU drivers, but you can’t update the machine firmware.

Treat a capacity block as a financial commitment before you request one. You pay from the start time “whether you use the capacity or not” (reservation guidance). Once Google approves a request and it moves to `PROVISIONING`, you can’t cancel, modify or delete it.

## What to do next

Ask your account team for the region, delivery date, price and support terms in writing. Before you reserve anything, check that your OS images, containers and native dependencies have working Arm64 builds.

Then agree a small trial block with a fixed length and total cost. Run your own model on it before either cloud goes into a migration plan.

## Sources

1. NVIDIA / OracleNVIDIA and Oracle Expand NVIDIA Vera Rubin to Multi-Gigawatt Scale and Bring NVIDIA Vera CPU to Oracle Cloud Infrastructure
2. OracleAnnouncing General Availability of OCI Compute with NVIDIA Vera CPUs
3. Google CloudGoogle Cloud and NVIDIA expand co-engineering collaboration to scale infrastructure for the agentic era
4. Google CloudAccelerator-optimized machine family (A5X machine series)
5. Google CloudGPU regions and zones
6. Google CloudBare metal instances
7. Google CloudReserve capacity (future reservations for capacity blocks)
8. Google CloudConsumption models
9. NVIDIANVIDIA and Google Cloud Collaborate to Advance Agentic and Physical AI
10. NVIDIANVIDIA Vera Rubin NVL72
11. NVIDIANVIDIA NVLink and NVLink Switch
12. NVIDIANVIDIA Kicks Off the Next Generation of AI With Rubin: Six New Chips, One Incredible AI Supercomputer

Written with AI assistance from the sources above, and fact-checked by a second AI model before publication. How we use AI
