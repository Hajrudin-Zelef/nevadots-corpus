---
id: collect-261121-veille-ia/nvidia-hardware/amd-puts-6th-gen-epyc-venice-on-general-availability-256-core-zen-6-cpus-for-the-agentic-ai-era
title: "AMD puts 6th Gen EPYC Venice on general availability: 256-core Zen 6 CPUs for the agentic AI era"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: ["2026-11-10"]
keywords: ["epyc", "agentic", "socket", "availability", "cores", "general", "servethehome", "venice"]
source: https://dev.to/delimitter/amd-puts-6th-gen-epyc-venice-on-general-availability-256-core-zen-6-cpus-for-the-agentic-ai-era-1njf
source_anchor: ""
source_lines: [1, 20]
section: "NVIDIA_Hardware"
---
**📅 November 10, 2026 • AMD**

Per AMD, on November 10, 2026 the company announced general availability of 6th Gen EPYC server processors, the SP7 line built on the new Zen 6 architecture. SP7-based systems are shipping from leading server OEMs and ODMs, and cloud instances from Microsoft Azure and OCI are coming soon — recent Azure and OCI announcements showcase agent orchestration and concurrent agent sandboxes as target workloads. The portfolio spans agentic AI, enterprise, cloud computing, HPC and general-purpose computing. Dan McNamara, SVP and GM of Compute and Enterprise AI at AMD, said AI is significantly expanding the role of the CPU across every layer of the data center.

Per ServeTheHome launch coverage, the lineup goes up to 256 cores and 512 threads per socket. The flagship EPYC 9996 has 256 cores, 4.1 GHz max boost, 1024 MB of L3 cache and a 600W TDP, while the EPYC 9686F ships at 5.3 GHz max boost, up from the 5.0 GHz originally announced for the SKU stack. Memory is 16 channels of DDR5-8000 or MRDIMM-12800, up to 8 TB per socket, with PCIe 6.0 on the new SP7 socket. Per ServeTheHome and TechPowerUp, the chips are made on a 2nm-class TSMC node, the first GAAFET-based node, and are the first x86 processors with PCIe Gen 6. Per TechPowerUp, the Zen 6c CCD packs up to 32 cores with 128 MB of L3, and the classic Zen 6 CCD up to 12 cores with 48 MB of L3.

Per Tom's Hardware first tests, a dual EPYC 9996 configuration with 1024 threads delivered up to 93 percent generational performance gains. Market context comes from The Next Platform, citing analyst forecasts: the agentic AI market is forecast to grow almost 43 percent annually through 2034, and GPU-to-CPU ratios in data centers, currently running 1 to 4 up to 1 to 8, are expected to move toward 1 to 1 in the agentic era. Per Phoronix and ServeTheHome, the roadmap continues beyond Venice: the SP8 platform launches early next year, Venice-X with 3D V-Cache debuts in 2027, and the LPDDR5X Verano variants are expected in the second half of 2027.

**How it works in practice.** For infrastructure planners the sizing math changes on density and memory: 256 cores per socket, 16 memory channels and up to 8 TB per socket set how many concurrent agent sandboxes, embedding jobs and CPU-side inference tasks a single node can hold. The Tom's Hardware figure of up to 93 percent generational gain in a dual socket configuration is the reference number for refresh decisions against 5th Gen EPYC. Availability today is through OEMs and on-premise builds, while Azure and OCI instances arrive later, so cloud-based agentic workloads have a wait ahead.

**Sources:**

- https://newsroom.amd.com/news/6th-gen-epyc-lead-ai-enterprise-computing
- https://www.servethehome.com/amd-epyc-9006-results-are-here-and-amd-delivers-a-knockout-part-deux/
- https://www.nextplatform.com/compute/2026/11/10/amd-tunes-up-cpus-for-agentic-ai-with-venice-epyc-processors/5308850
- https://www.tomshardware.com/pc-components/cpus/amd-zen-6-epyc-venice-deep-dive-and-benchmarking-dual-epyc-9996-with-1-024-threads-delivers-up-to-93-percent-performance-gain-gen-on-gen-9686f-outclasses-nvidias-vera

**Continue the conversation:** https://t.me/AIAutomationArchitecture · Bluesky

## Top comments (0)
