---
id: collect-261121-veille-ia/microsoft-actu/microsoft-boosts-hbm-bandwidth-by-2-5x-with-maia-300-accelerator-hpcwire
title: "Microsoft Boosts HBM Bandwidth by 2.5x with Maia 300 Accelerator - HPCwire"
domain: microsoft-actu
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-10-12"]
keywords: ["maia", "microsoft", "accelerators", "power", "memory", "data", "bandwidth", "delivers"]
source: https://www.hpcwire.com/2026/10/12/microsoft-boosts-hbm-bandwidth-by-2-5x-with-maia-300-accelerator/
source_anchor: ""
source_lines: [1, 29]
section: "Microsoft_Actu"
---
Microsoft is goosing its high-bandwidth memory (HBM) bandwidth from 7.1 TB per second with Maia 200 to 18 TBps with the new Maia 300, delivering a significant upgrade to the chip’s capability to move data for AI inference workloads while lowering per-token energy cost.

Microsoft’s Scott Guthrie, the executive vice president of cloud and AI, announced the new chip today in a blog post in which he also announced that the Maia 200 chip has hit full production. Maia 200 is running significant AI workloads in Azure today, including OpenAI models and its own MAI models, which power Microsoft Copilot.

While Maia 200, which Microsoft unveiled in January 2026, delivers a big boost to its AI inference capabilities, Maia 300 will deliver even more AI inference computing power to Azure.

In addition to goosing the HBM from 216 GB of HBM3 in Maia 200 to 288 GB of HBM4 in Maia 300 (and increasing the HBM bandwidth by 2.5x as previously noted), the new chip delivers 13.8 FP4 peta-ops (POPS), versus 10.1 FP4 POPS with Maia 200, and 6.9 FP8 POPS, versus 5.1 FP8 POPS previously. It also gets enhanced MXFP4 support.

Microsoft has evolved Maia’s architecture pretty significantly. Instead of the previous monolithic system on a chip (SOC) design utilizing TSMCs’ 3 nanometer process node, Microsoft has adopted a modular chiplet architecture using NSMC’s N2P process node with the Maia 300.

Moving to a chiplet architecture allows Microsoft to separate compute, memory-controller, and I/O functions, which it says improves efficiency, scalability and cost structure. It has replaced the uniform memory access (UMA ) with non-uniform memory access (NUMA) across chiplets, delivering higher data locality and lowering the need to move data.

“Each compute region has nearby memory, and SDLA’s explicit control over DMA engines, local SRAM, HBM and the network-on-chip places data next to the compute that uses it,” Microsoft writes in its technical brief on the Maia 300. “Data travels shorter distances, which reduces the energy spent moving it.”

The network has also evolved to support bigger clusters. Specifically, Microsoft adopted a two-tier scale-up network with Maia that effectively doubles the size of Maia 300 clusters.

The company uses direct links between accelerators within a tray and a switched fabric to connect all 96 accelerators within a single rack. An optical second tier switch, meanwhile, enables Microsoft to connect up to 128 racks to build scale-up “superpod” equipped with more than 12,000 Maia 300 accelerators, which is twice the size of the biggest scale-up cluster with Maia 200.

The in-rack switched fabric relies on six high-radix switch ASICs that are integrated into three liquid-cooled switch trays. providing the rack-local connectivity required by all 96 accelerators while minimizing the space and power consumed by switching, according to Microsoft.

“Maia 300 treats networking as a seamless extension of the software-defined dataflow architecture,” Guthrie writes. “Rather than moving data through separate communication layers, dataflow operations span compute, memory and network resources as a single system. High-bandwidth interconnects between Maia accelerators are optimized for tensor parallelism, while a custom AI transport delivers performant and reliable scale-up communication across accelerators and racks.”

Microsoft is using dual power feeds, high-capacity power shelves, and a 50 V busbar architecture to distribute power across Maia 300 accelerator and switch trays. It uses a combination of liquid cooling for Maia 300 accelerators, voltage regulators, and switch ASICs and air-cooling for memory, storage, and other components.

All told, a full rack of 96 Maia 300 accelerators delivers up to seven times greater token throughput than a full rack of 48 Maia 200 accelerators. On a per-watt basis, Maia 300 delivers 2.5X the performance.

“Maia 300 rack systems bring together 96 accelerators within a 167-kilowatt power envelope, delivering 27.6 TB of HBM capacity and 1.7 PB/s of aggregate HBM bandwidth across a high-speed scale-up domain,” Guthrie writes. “That density allows Microsoft to serve more simultaneous AI interactions and larger workloads from each rack, improving datacenter capacity and the economics of expanding AI services.”

Maia 300 is expected to begin production deployments in 2027.
