---
id: collect-261121-veille-ia/nvidia-hardware/amd-zen-6-in-detail-epyc-venice-delivers-27-percent-more-ipc-and-up-to-256-cores
title: "AMD Zen 6 in Detail: EPYC Venice Delivers 27 Percent More IPC and Up to 256 Cores"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: []
keywords: ["percent", "cores", "more", "clock", "core", "cache", "epyc", "about"]
source: https://basic-tutorials.com/news/amd-zen-6-in-detail-epyc-venice-delivers-27-percent-more-ipc-and-up-to-256-cores/
source_anchor: ""
source_lines: [1, 87]
section: "NVIDIA_Hardware"
---
AMD fully disclosed the **AMD Zen 6** architecture on November 10, 2026, coinciding with the market launch of the EPYC 9006 “Venice” processors. According to AMD, the new core delivers an average of about 27 percent more instructions per clock cycle (IPC) than Zen 5. A Zen 6 chiplet now contains 12 cores instead of 8, and the high-density Zen 6c variant even has 32. This results in server CPUs with up to 256 cores, 1 GB of L3 cache, 16 DDR5 channels, and PCIe 6.0. However, the 27 percent figure comes from a test setup that makes Zen 6 look quite good. More on that**in a moment.**

Key Takeaways

- According to AMD, Zen 6 delivers about 27% more IPC than Zen 5, measured with Zen 6c at a fixed clock speed. For enterprise and cloud workloads, the increase is about 20%, and for AI workloads, about 31%.
- A Zen 6 CCD has up to 12 cores and 48 MB of L3 cache, while a Zen 6c CCD has up to 32 cores and 128 MB of L3 cache.
- The EPYC 9006 “Venice” for the SP7 socket has been available since November 10, 2026: up to 256 cores, DDR5-8000 or MRDIMM-12800, PCIe 6.0.
- Manufactured using TSMC’s 2-nm process, the top-of-the-line EPYC 9996 costs $14,904.
- SP8 will follow in the first half of 2027, with Venice-X featuring 3D V-Cache and Verano with LPDDR5X set to launch in the second half of 2027.

Zen 6 and Zen 6c share the same instruction set and microarchitecture. As with Zen 5, the difference lies in the physical design: Zen 6 is designed for high clock speeds, while Zen 6c is optimized for the highest possible core count per area and per watt.

The core has become wider and deeper. Dispatch, Rename, and Retire each operate 8-wide, plus there are 6 ALUs, 4 loads, and 2 stores per clock cycle, as well as a 512-bit-wide FP data path. The biggest improvements are in the buffers that hold instructions “in flight.” The reorder buffer grows by a good 40 percent, and the integer register file by nearly 50 percent. This allows the core to process more instructions ahead of time while waiting for data from memory. This is precisely the bottleneck in servers with 256 cores connected to a single memory system.

| Feature | Zen 5 | Zen 6 / Zen 6c | 
|---|---|---|
| Op-Cache | 6K instructions, 12 per clock cycle | 6.75K ops, 18 per clock cycle | 
| Return Address Stack | 52 entries | 64 entries | 
| Reorder buffer | 448 | 640 | 
| Integer register file | 240 | 352 | 
| Load Queue / Store Queue | 64 / 96 | 80 / 128 | 
| VNNI-INT8 Pipes | 2 | 4 | 
| L1 data cache | 48 KB | 48 KB (12x, 4-cycle latency) | 
| L2 cache per core | 1 MB | 1 MB | 
| L3 cache per core | 4 MB (Zen 5) / 2 MB (Zen 5c) | 4 MB (both variants) | 

In terms of the instruction set, AMD is stepping up its game, particularly for AI. Zen 6 now fully supports AVX-512 with FP16, along with AVX-VNNI-INT8, AVX-NE-CONVERT, and new Bit Matrix Multiply (BMM) instructions. For operating systems and virtualization, there’s FRED and improvements to SEV-SNP. According to AMD, it is the first x86 manufacturer to use the open-source Caliptra 1.2 as its Root of Trust.

The headline figure is impressive, but the test setup behind it is unique. AMD compared 16 Zen 5 cores, spread across two CCDs, against 16 Zen 6c cores on a single CCD, both running at the same clock speed. The Zen-6c cores therefore share an L3 cache and do not need to communicate with a second chiplet via Infinity Fabric. Part of the 27 percent gain is thus attributable to the topology and not solely to the core itself.

Breaking it down, AMD cites a roughly 20 percent increase for enterprise and cloud applications and about 31 percent for AI workloads. The latter comes as little surprise, given that AMD has doubled the number of INT8 units. In its launch article, ServeTheHome also mentions an IPC increase of about 20 percent compared to Turin. I consider this 20 percent to be the more realistic figure.

There’s also a boost when it comes to clock speed: According to ServeTheHome, the final silicon turned out better than expected. The EPYC 9686F with 96 cores now boosts to 5.3 GHz instead of the 5.0 GHz announced in July. The dense Zen 6c models reach up to 4.1 GHz—400 MHz more than Turin Dense.

The most significant change involves the chiplets. Previously, a Zen 5 CCD contained eight cores. Zen 6 increases this to twelve cores with 48 MB of L3 cache, while Zen 6c doubles the core count from 16 to 32 with 128 MB of L3 cache. Both variants feature a uniform 4 MB of L3 cache per core. The CCDs are connected to two I/O dies via the fifth generation of Infinity Fabric and so-called Elevated Fan-Out Bridges.

| Configuration | CCDs | Cores per CCD | Cores / Threads | Total L3 Cache | Max. Boost | 
|---|---|---|---|---|---|
| SP7, Zen 6 | 8 | 12 | 96 / 192 | 384 MB | 5.3 GHz | 
| SP7, Zen 6c | 8 | 32 | 256 / 512 | 1,024 MB | 4.1 GHz | 
| SP8, Zen 6c | 4 | 32 | 128 / 256 | 512 MB | N/A | 

With Venice, AMD is switching from the SP5 socket to the SP7 socket. Each of the two I/O dies connects eight DDR5 channels, resulting in 16 channels per socket instead of the previous 12. With standard RDIMMs, speeds of up to DDR5-8000 are possible; with second-generation MRDIMMs, speeds of up to 12,800 MT/s are achievable with one module per channel. AMD specifies the memory bandwidth at up to 1.6 TB/s per socket and the capacity at up to 8 TB.

| Platform | EPYC 9005 “Turin” (SP5) | EPYC 9006 “Venice” (SP7) | 
|---|---|---|
| Max. Cores | 192 (Zen 5c) | 256 (Zen 6c) | 
| Manufacturing (CCD) | TSMC 3 nm / 4 nm | TSMC 2 nm | 
| Memory | 12 × DDR5-6400, up to 6 TB | 16 × DDR5-8000 or MRDIMM-12800, up to 8 TB | 
| PCIe (1P) | 128 PCIe 5.0 lanes | 96 lanes of PCIe 6.0 + 8 lanes of PCIe 4.0 | 
| CXL | 2.0 | 3.1 | 
| Max. Power Consumption | 500 W | 600 W | 

Noteworthy: In single-socket mode, there are fewer lanes than on Turin, but each lane delivers twice the bandwidth with PCIe 6.0. For AI servers with GPUs, this is the more important metric. Additional memory can be connected via CXL 3.1. The more compact SP8 platform features eight memory channels and consumes up to 400 watts.

Instead of the traditional TDP, AMD now refers to a “Default CPU Power” starting with this generation, which combines the power consumption of the compute and I/O dies at a defined performance target. This makes comparisons with older EPYC datasheets somewhat more difficult.

In total, the family comprises 31 models for SP7 and SP8, priced between $700 and $14,904. At the top of the lineup is the EPYC 9996 with 256 Zen-6c cores.

| Model | Cores / Threads | Base / Boost | L3 Cache | Default CPU Power | Price (1,000 units) | 
|---|---|---|---|---|---|
| EPYC 9996 | 256 / 512 | 2.55 / 4.1 GHz | 1,024 MB | 600 W | $14,904 | 
| EPYC 9966 | 192 / 384 | 2.9 / 4.0 GHz | 768 MB | 600 W | $14,079 | 
| EPYC 9846 | 168 / 336 | 2.85 / 3.7 GHz | 768 MB | 500 W | $13,114 | 
| EPYC 9756 | 128 / 256 | 3.15 / 4.0 GHz | 512 MB | 500 W | $12,498 | 
| EPYC 9686F | 96 / 192 | 3.4 / 5.3 GHz | 384 MB | 500 W | N/A | 

At the same time, AMD is launching the EPYC Embedded 9006 with up to 256 cores for test systems, storage arrays, and firewalls. The embedded line recently received an update for compact edge systems with the Zen 5-based EPYC Embedded 2005.

AMD explicitly positions Venice for agent-based AI—that is, for servers on which many AI agents execute code in parallel, call tools, and query databases. The benchmark figures are correspondingly aggressive. Compared to Intel’s Xeon 6980P with 128 cores, AMD cites an average of 2.2 times the performance in HPC applications; compared to Nvidia’s Arm CPU Vera in the SPECrate Integer test, it cites 2.24 times the platform performance. According to AMD, the lead over Vera remains at around 20 percent per core.

These are manufacturer claims, and in some cases even estimates. PC Games Hardware has re-calculated AMD’s white paper: In the four SPEC subtests that Nvidia itself highlights as key for agentic AI, Venice leads by only 1 to 12 percent per core. The large gap is primarily due to the sheer number of cores—256 versus 88 for Vera. Independent benchmarks are just getting underway.

AMD unveiled Venice on July 23, 2026, at the “Advancing AI 2026” event. Mass production at TSMC in Taiwan has been underway since May, and the TSMC plant in Arizona is set to begin manufacturing later. Venice is also featured in AMD’s Helios rack system, which Microsoft, among others, plans to use for AI inference on Azure. The 2-gigawatt deal between AMD and Anthropic also centers on this generation of data center hardware.

- **EPYC 9006 (SP7):** Available since November 10, 2026
- **EPYC 9006 (SP8):** first half of 2027
- **EPYC 9006X “Venice-X”:** second half of 2027, up to 96 Zen-6 cores with 1,152 MB L3 thanks to 3D V-Cache
- **EPYC 9006 LP “Verano”:** second half of 2027, with LPDDR5X for AI host systems

For desktop users, the 12-core CCD is particularly exciting. If AMD sticks with this chiplet for Ryzen as well, two CCDs could enable 24 cores in the mainstream socket, and games running on a single CCD would have access to 12 cores sharing a common L3 cache. None of this has been confirmed: AMD made no mention of Zen 6-based desktop or notebook processors at the Venice launch.

However, the IPC improvements to the core itself will definitely carry over. It’s difficult to gauge how much of this will translate to games based on server benchmarks with 256 cores. In any case, AMD has momentum: In the second quarter of 2026, its market share for x86 client CPUs exceeded 30 percent for the first time.

On paper, AMD Zen 6 represents one of the biggest generational leaps since Zen 3. Still, I wouldn’t take the 27 percent IPC figure as a benchmark. The test setup, which compares one CCD to two, clearly favors Zen 6c; a more realistic figure is likely the 20 percent that AMD itself cites for enterprise and cloud workloads. That’s still a lot.

In the data center, the overall package matters more than the individual core anyway: 256 cores, 16 memory channels with up to 1.6 TB/s, PCIe 6.0, and 2-nm manufacturing. Intel’s direct P-core competitor, Diamond Rapids, is reportedly not expected until 2027, while NVIDIA’s Vera takes a completely different approach with 88 cores. The first independent tests are now showing just how well the manufacturers’ figures hold up. And anyone actually waiting for a new Ryzen should keep an eye on the 12-core CCD. For me, that’s the most exciting news from this launch.
