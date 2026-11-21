---
id: collect-261121-veille-ia/copilot-microsoft/microsoft-s-maia-300-publishes-rack-numbers-for-its-own-inference-silicon
title: "Microsoft's Maia 300 Publishes Rack Numbers for Its Own Inference Silicon"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-10-12"]
keywords: ["microsoft", "maia", "rack", "bandwidth", "inference", "capacity", "azure", "inside"]
source: https://dev.to/echakiri/microsofts-maia-300-publishes-rack-numbers-for-its-own-inference-silicon-41mg
source_anchor: ""
source_lines: [1, 61]
section: "Copilot_Microsoft"
---
- Microsoft Azure Maia 300 is an AI inference accelerator, the third generation of Microsoft's own chip family for running large models inside its datacenters, announced on 12 October 2026.
- The chip is a multi-chiplet package with compute chiplets on TSMC's N2P process, memory-controller and I/O functions on separate dies, eight HBM4 stacks providing 288 GB and 18.0 TB/s of peak bandwidth inside a 950 W TDP, and support for enhanced MXFP4 and sub-3-bit storage formats.
- The rack holds 96 accelerators inside a 167-kilowatt envelope with 27.6 TB of HBM in the announcement (27.7 TB in the deep dive's rack table) and 1.7 PB/s of aggregate memory bandwidth, which doubles the accelerator density of Maia 200 per Microsoft's published rack table.
- A two-tier Ethernet scale-up fabric joins up to 128 racks into a superpod of 12,288 accelerators, using electrical links inside the rack, optical links between racks, a bandwidth split that job packing can move and hot-spare capacity that schedulers take from another rack.
- Microsoft expects 40% more tokens per megawatt than leading GPU-based systems and up to 7x the rack-scale token throughput of Maia 200, so the unnamed baseline sits on the cross-vendor claim while the Maia 200 ratios stay checkable.
- Capacity planning changes shape around these figures: more tokens per watt, twice the accelerators per rack, a liquid-cooling loop that carries more than 80% of the rack's thermal load, and a power envelope that decides where capacity can land.
- Maia 200 already serves Microsoft production workloads including Microsoft Copilot and is deploying across the United States, Europe and Australia, with GitHub Copilot and Microsoft Foundry following by the end of 2026.

The announcement of the third Azure Maia generation arrived on 12 October 2026 with a system-level frame. Scott Guthrie, executive vice president for Cloud + AI, described Maia 300 as "a complete intelligent system designed for the new types of AI workloads emerging today: Long-context reasoning, latency-sensitive conversational experiences and sustained, multi-step agentic tasks", and set the deployment plan alongside it. Microsoft will scale the Azure Maia family to multi-gigawatt deployments starting in 2027 and make it the primary inference system for Microsoft products and services, with Maia 300 production deployments beginning that year.

That ordering carries information. A chip announcement that leads with a power envelope and a rack count is a statement about datacenter capacity.

The Maia 300 accelerator as published in Microsoft's official announcement on the Official Microsoft Blog on 12 October 2026: the packaged chip in its carrier, photographed for the launch.

The generational story matters more than the launch itself. Maia 200 is already serving production workloads including Microsoft Copilot, and Microsoft says GitHub Copilot and Microsoft Foundry follow by the end of 2026, with OpenAI models, Microsoft's own MAI models and leading open-source model families all running on that silicon across the United States, Europe and Australia.

Inside the package, Microsoft separates functions across dies. Compute sits on chiplets built on TSMC's N2P process, while memory-controller and I/O functions occupy dies chosen for bandwidth, connectivity and cost, an arrangement Microsoft's Azure hardware team describes as allowing each function to use the technology best suited to its role.

The memory side is where the arithmetic for inference gets decided. Eight HBM4 stacks give the accelerator 288 GB of capacity and 18 TB/s of bandwidth, which Azure's architecture post reports as 2.5 times the memory bandwidth, 33% more capacity and roughly twice the HBM bandwidth per watt of Maia 200. Alongside the memory change, Microsoft moved the package from a uniform memory organisation to a locality-aware one across chiplets, so data placement follows the compute that consumes it.

SiliconANGLE's reading of the same package is worth keeping in the margin: the 288 GB matches the capacity of NVIDIA's Rubin accelerator, while memory bandwidth lands about 20% lower. Two vendors converging on the same capacity and diverging on bandwidth per accelerator describes the design space a buyer is actually choosing inside.

The rack is the unit Microsoft chose to publish. Maia 300 integrates 96 accelerators across 24 four-accelerator trays inside a 167-kilowatt envelope, with a host processor, memory, storage and cooling in each tray. Folding the host into the accelerator tray is the detail that doubles accelerator density relative to Maia 200, because the separate host trays disappear from the floor plan.

Read the HBM bandwidth and rack power rows of that table together. Rack HBM bandwidth grows by a factor of 5.1 while rack power grows by 2.6, which the table expresses as 1.95 times the HBM bandwidth per watt, close to the ratio Microsoft states per accelerator in its deep dive: approximately 2 times the HBM bandwidth per watt of Maia 200. A team that reasons in tokens per watt finds that number more useful than any peak figure, because the power envelope is what a datacenter bills and what a facility has to cool.

Cooling is the second half of that envelope. Liquid cooling carries more than 80% of the rack's thermal load from accelerators, voltage regulators and switch ASICs, and the design connects either to facility water or through a heat-exchanger unit for air-cooled halls. Those two options decide which buildings can host the capacity at all.

Scale-up networking is where Maia 300 departs furthest from a conventional GPU cluster. Microsoft's AI network controller runs custom AI transport over Ethernet, and the topology is two-tier: a half-switched arrangement inside the rack where each accelerator divides its connectivity between direct links to a partner in its tray and switched paths to the other 94, all of it electrical, and a second optical tier of switches that connects up to 128 racks into a superpod.

Microsoft's official rack architecture figure and comparison table for Maia 300, published in the Azure deep dive of 12 October 2026. The table gives pod size 48 to 96, HBM bandwidth 341 to 1,728 TB/s, HBM bandwidth per watt 5.3 to 10.6 GB/s/W, HBM capacity 10.4 to 27.7 TB, FP4 performance 485 to 1,324 peta-operations per second, and rack power 64 kW to 167 kW.

The engineering choice worth naming is that the bandwidth split between in-rack and cross-rack traffic stays open after the system is designed. It becomes a parameter set by job packing, and an accelerator can drive up to 750 GB/s to a peer elsewhere in the superpod while only a subset of accelerators travel cross-rack. Tensor parallelism can stay inside one rack while expert and pipeline parallelism span racks, and schedulers can absorb a failed tray by reaching into hot-spare capacity in a neighbouring rack.

Our reading of that arrangement is straightforward: this is a scheduling capability expressed as a network design. Capacity that used to sit stranded inside one rack boundary becomes usable by a job packed elsewhere, which raises the effective utilization of the same silicon.

Microsoft's efficiency headline reads as follows: Maia 300 should deliver 40% more tokens per megawatt compared with leading GPU-based systems, and up to 7x the rack-scale token throughput of Maia 200 in a 96-accelerator configuration. Maia 200, meanwhile, is credited with up to 1.7x more tokens per watt than leading GPU infrastructure for built-in AI in Teams, Copilot and Cowork.

The two halves of that sentence carry different weights. The 7x rack-scale figure and the 2.5x performance-per-watt figure are comparisons inside Microsoft's own product line. The 40% figure crosses a vendor boundary, and Microsoft's post names no counterpart system, no model, no sequence length and no batch configuration for it. The Verge reported the same figure on the same day with the same absence.

We treat cross-vendor numbers as the starting point for a question. A useful question asks which baseline the tokens-per-megawatt comparison used, on which model, at which context length, and whether the serving stack was tuned for both systems. The answer decides whether the number transfers to your workload. This is the same discipline we apply to our own sovereign infrastructure and cloud exit work for European teams, where a published efficiency ratio has to survive contact with a real power contract and a real rack count.

Microsoft's official two-tier scale-up network figure, published in the Azure deep dive on 12 October 2026: tier-2 switches linking 128 racks into a superpod of 12,288 accelerators, with electrical links inside the rack and optics between racks.

For anyone buying inference, the announcement is useful as a planning template even when the silicon never reaches your estate. Capacity now arrives denominated in racks and megawatts, and the arithmetic runs in this order: tokens per second required, divided by tokens per watt at the context length your workload uses, giving a power figure, which then meets a cooling limit and a facility limit.

Two of Microsoft's own numbers change how that arithmetic behaves. Fewer racks carry the same load when each rack holds twice the accelerators, and the bandwidth-per-watt improvement moves more of the bottleneck onto cooling and power delivery. The rack as a power contract is the frame we use for that shift, and hyperscale rack designs keep confirming it.

Liquid cooling deserves a line of its own in any plan. A rack that removes more than 80% of its thermal load through liquid needs either facility water or a heat exchanger.

The installed generation tells a reader what Microsoft's inference roadmap looks like in practice. Maia 200 runs Copilot workloads today, with GitHub Copilot and Microsoft Foundry following by the end of 2026, and the same silicon hosts models from OpenAI, from Microsoft's MAI team and from open-source families.

Running one accelerator family across three model provenances is the less visible achievement in this announcement. For an operations team, a fleet that serves several model lineages on one serving path reduces the number of hardware-specific tuning tracks it maintains, which is the same portability question we answer in self-hosted platform work, where the goal is keeping the model layer swappable while the serving layer stays stable.

Netics editorial diagram: the four published Maia 300 figures that come from Microsoft's own rack table, set beside the cross-vendor efficiency claim whose underlying systems the announcement leaves unnamed.

Microsoft says more detail follows at Ignite 2026 in November. Until then, the published rack table and the two architecture figures are the parts of this announcement that a technical reader can check.

*Originally published on the Netics blog.*

## Top comments (0)
