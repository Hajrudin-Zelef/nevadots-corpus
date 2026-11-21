---
id: collect-261121-veille-ia/nvidia-hardware/nvidia-rubin-architecture-everything-you-must-know-november-2026
title: "Nvidia Rubin Architecture: Everything You Must Know (November 2026)"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: []
keywords: ["rubin", "cloud", "nvidia", "vera", "first", "gpus", "public", "rack"]
source: https://www.thundercompute.com/blog/nvidia-rubin-architecture
source_anchor: ""
source_lines: [1, 207]
section: "NVIDIA_Hardware"
---
The NVIDIA Vera Rubin platform is a seven-chip, rack-scale AI factory that delivers 3,600 PFLOPS of NVFP4 inference per rack. NVIDIA claims 5x the throughput of Blackwell at 10x lower cost per token.

Vera Rubin entered production in January 2026, began volume shipments in August, and is now live, although scarce, at Oracle, CoreWeave, Crusoe and Google Cloud.

First announced at Computex 2024, Vera Rubin co-designs GPUs, CPUs, networking, storage, and security silicon into one coherent system rather than a single chip.

**Takeaways**

- **Rubin GPU delivers 50 PFLOPS FP4 and 22 TB/s HBM4 bandwidth** : about 5x B200 and 3.3x B300 on FP4.
- **The Vera Rubin NVL72 rack** combines 72 Rubin GPUs and 36 Vera CPUs for 3,600 PFLOPS and 20.7 TB of HBM4.
- **No cloud sells Rubin on demand yet** : Oracle, CoreWeave, Crusoe and Google Cloud offer it only through contracts
- **The Groq 3 LPX rack** handles low-latency decode, with NVIDIA claiming up to 35x higher throughput/megawatt than Blackwell.
- **Rubin Ultra's Kyber rack may not ship until 2028** according to SemiAnalysis, though NVIDIA says its "roadmap is intact."

On-demand GPUs

Starting at $0.35/hr, billed per minute.

## Everything We Know So Far

NVIDIA confirmed Rubin entered full production at CES 2026. Jensen Huang used the keynote to detail all six co-designed chips and announce H2 2026 partner availability. GTC 2026 in March added the seventh chip, the Groq 3 LPU (language processing unit), and detailed the full Vera Rubin POD.

Rubin succeeds Blackwell and targets two core demands of agentic AI:

- Long-context inference and low latency at scale
- Trillion-parameter mixture-of-experts (MoE) models

Compared to Blackwell, NVIDIA claims:

- 5x rack-level inference performance
- 10x lower inference token cost
- 4x fewer GPUs for MoE training

Vera Rubin NVL72 cloud availability is live but limited: Oracle, CoreWeave, Crusoe and Google Cloud offer it today, but only through reserved contractsThe provider-by-provider rollout covers each cloud's status.

Read next
AI GPU rental market trends â How GPU rental supply is shifting through 2026.


## Where Can You Rent Rubin? Cloud Rollout by Provider

No cloud provider sells Rubin GPUs on demand as of November 2026, and none lists a public hourly rate. Every live deployment is a reserved NVL72 cluster sold through contracts. The first capacity went to frontier labs such as OpenAI and Cognition.

NVIDIA says Vera Rubin NVL72 racks are running at CoreWeave, Google Cloud, Microsoft Azure, Oracle Cloud Infrastructure, and Nebius. Each provider's Rubin status as of November 2026:

| Provider | Rubin status | Date | How to get access | 
|---|---|---|---|
| Oracle (OCI) | Generally available | Oct 27, 2026 | Through sales; not on OCI's public GPU price list. HGX Rubin NVL8 (8-GPU server) planned | 
| CoreWeave | Live in production; Cognition is the first customer | Sep 30, 2026 (first AI-cloud bring-up Jun 2026) | Reserved contracts | 
| Google Cloud (A5X) | GA rolling out | Nov 2026 (first customer instance Jul 2026) | A5X bare-metal instances; no public price yet. Europe expansion planned | 
| Microsoft Azure | Racks running, no public instance | No GA date (lab power-on Mar 2026) | Not offered to customers yet | 
| Nebius | Racks running; first Groq 3 LPX adopter | H2 2026 | No public listing yet | 
| Lambda | Announced | H2 2026 | Bare-metal Superclusters via sales; no self-serve | 
| Crusoe | Listed on pricing page (only provider with a public listing) | Not stated | "Contact sales," no hourly rate | 
| AWS | Not available | 2027-2028 | Not announced | 

### What This Means If You Need GPUs Now

Rubin will likely stay a reservation product into 2027. Supply is constrained, the minimum unit is effectively a 72-GPU rack, and the first allocations go to labs signing multi-year contracts. Smaller teams are unlikely to get on-demand Rubin access for several quarters.

On-demand H100 and A100 instances remain a practical, and accessible choice for fine-tuning, inference, and prototyping today. Most models under 70B parameters fit comfortably on them. Teams can move to Rubin once self-serve capacity appears.

## The NVIDIA Rubin Announced Product Line

The Rubin platform ships as three headline products: the Vera Rubin NVL72 rack, the Vera Rubin Superchip, and the Rubin GPU.

| Specification | Vera Rubin NVL72 | Vera Rubin Superchip | Rubin GPU | 
|---|---|---|---|
| Configuration | 72 Rubin GPUs + 36 Vera CPUs | 2 Rubin GPUs + 1 Vera CPU | 1 Rubin GPU | 
| Inference (NVFP4) | 3,600 PFLOPS | 100 PFLOPS | 50 PFLOPS | 
| Training (NVFP4) | 2,520 PFLOPS | 70 PFLOPS | 35 PFLOPS | 
| GPU Memory | 20.7 TB HBM4 | 576 GB HBM4 | 288 GB HBM4 | 
| Memory Bandwidth | 1,580 TB/s | 44 TB/s | 22 TB/s | 
| NVLink Bandwidth | 260 TB/s | 7.2 TB/s | 3.6 TB/s | 
| NVLink-C2C Bandwidth | 65 TB/s | 1.8 TB/s | - | 
| CPU Cores | 3,168 Olympus cores | 88 Olympus cores | - | 
| CPU Memory | 54 TB LPDDR5X | 1.5 TB LPDDR5X | - | 
| Cooling | Full liquid |  |  | 

### NVIDIA Vera Rubin NVL72: The Rack-Scale AI Powerhouse

The Vera Rubin NVL72 is NVIDIA's flagship rack-scale system and the direct successor to the GB200 NVL72. It houses 72 Rubin GPUs and 36 Vera CPUs in a single liquid-cooled rack, connected by NVLink 6.

### Vera Rubin Superchip: One Package to Run the AI Era

The Vera Rubin Superchip is the base compute unit of the platform. It pairs one Vera CPU and two Rubin GPUs in a single package. Applications can treat LPDDR5X and HBM4 as a unified memory pool, reducing data movement overhead.

The Rubin GPU uses TSMC's 3nm process with a dual-die design and 336B transistors, a 1.6x increase over Blackwell. The Vera CPU adds 227B transistors and 88 Arm Olympus cores; Spatial Multi-Threading brings the effective thread count to 176.

### Rubin GPU: The Accelerator at the Heart of Rubin

The Rubin GPU is the inference and training accelerator at the center of every Vera Rubin system. Each Rubin GPU carries 288 GB of HBM4. The Rubin GPU is optimized for FP4 and FP6 workloads, long-context inference, and multi-modal generative tasks.

### VR200, R100, R200, H300: What the Names Mean

NVIDIA has published no official alphanumeric SKU for the Rubin GPU, so press and supply-chain coverage use several unofficial names:

- **VR200 is the platform-level designation** (Vera CPU + Rubin GPU), following the GB200 naming convention; press also uses it loosely for the Rubin GPU itself.
- **R100 was early supply-chain shorthand** that still appears in some coverage.
- **R200 is the more accurate press shorthand** for the two-die GPU package.

To date, NVIDIA's own product pages use "Rubin GPU" exclusively.

## The NVIDIA Vera Rubin Platform: A Full-Stack AI Revolution

The Vera Rubin platform is a complete AI factory blueprint, not a single chip. NVIDIA describes it as a seven-chip, five-rack architecture where every component is co-designed to eliminate the bottlenecks of off-the-shelf parts.

The seven chips cover the full lifecycle of agentic AI workloads:

- **Rubin GPU** and**Vera CPU** for compute
- **NVLink 6 Switch ASIC** for rack-scale scale-up networking
- **ConnectX-9 SuperNIC** and**Spectrum-6 Ethernet** for scale-out connectivity
- **BlueField-4 DPU** for storage and infrastructure security
- **Groq 3 LPU** for high-throughput decode inference

## Rubin GPU vs Blackwell vs Hopper: Full Spec Comparison

The Rubin GPU's biggest gains over Blackwell are in memory bandwidth and FP4 compute. HBM capacity stays flat from B300 to Rubin at 288 GB per GPU. Rubin gains 2.8x in bandwidth and 3.3-5x in FP4 throughput, per NVIDIA's ratings.

| Specification | Rubin GPU (VR200) | Blackwell B200 | Hopper H100 SXM | 
|---|---|---|---|
| Architecture | Rubin | Blackwell | Hopper | 
| Process node | TSMC 3nm | TSMC 4nm | TSMC 4nm | 
| Transistors | 336B | 208B | 80B | 
| GPU memory | 288 GB HBM4 | 180 GB HBM3e | 80 GB HBM3 | 
| Memory bandwidth | 22 TB/s | 8 TB/s | 3.35 TB/s | 
| FP4 inference | 50 PFLOPS | 9 PFLOPS | N/A | 
| FP8 throughput | 17.5 PFLOPS | 4.5 PFLOPS | 1.979 PFLOPS | 
| NVLink bandwidth | 3.6 TB/s (NVLink 6) | 1.8 TB/s (NVLink 5) | 900 GB/s (NVLink 4) | 
| TDP | ~2,300 W (Max-P, reported) | 1,000 W | 700 W | 
| Cloud availability | Contract or quote-only (live at OCI, CoreWeave, Google Cloud, Crusoe) | Available now | Available now | 

## The Engineering Breakthroughs Inside the Rubin Architecture

### NVIDIA Vera CPU: The Brain of the Rubin Platform

The Vera CPU replaces Grace (NVIDIA's previous CPU generation) and delivers 2x the data processing performance. It is built on TSMC's 3nm process with 227B transistors and 88 custom Arm Olympus cores. Spatial Multi-Threading doubles the thread count to 176 by physically partitioning each core's resources between two threads.

The Vera CPU is purpose-built for agentic AI workloads: managing context memory, routing tokens, and keeping inference pipelines saturated. Vera uses NVIDIA-designed Olympus cores, replacing the licensed Arm Neoverse cores in Grace. Each Vera CPU supports up to 1.2 TB/s of LPDDR5X bandwidth.

### NVIDIA Groq 3 LPU: Redefining Inference Speed

The Groq 3 LPU handles deterministic, low-latency token generation (decode) in a companion LPX rack alongside the Vera Rubin NVL72. NVIDIA integrated the LPU into the Rubin platform after a roughly $20B licensing deal with Groq. The LPU serves as the decode-optimized engine for trillion-parameter and high-interactivity agentic models.

The LPX rack houses 256 LPUs, each with 500 MB of on-chip SRAM. The full LPX rack totals 128 GB of SRAM, 40 PB/s of aggregate memory bandwidth, and 640 TB/s of scale-up bandwidth. Paired with the Vera Rubin NVL72, NVIDIA claims up to 35x inference throughput/megawatt for trillion-parameter models.

The LPX rack is liquid-cooled and entered full production in August 2026.

### HBM4: The Memory Leap That Makes Rubin Possible

HBM4 gives each Rubin GPU 22 TB/s of memory bandwidth, a 2.8x improvement over Blackwell's HBM3e at 8 TB/s. This gain comes from doubling the interface bus width per stack to 2,048 bits, running at 10.8 GT/s per pin.

At the NVL72 rack level, total HBM4 capacity reaches 20.7 TB at 1.6 PB/s of aggregate bandwidth. This headroom lets larger models and longer KV caches fit in fewer GPUs. SK Hynix, Samsung, and Micron all confirmed volume HBM4 production for Vera Rubin at GTC Taipei in June 2026.

### NVLink 6: The Fastest GPU Interconnect

NVLink 6 doubles GPU-to-GPU bandwidth over Blackwell's NVLink 5: 3.6 TB/s of scale-up bandwidth per GPU, versus 1.8 TB/s. At the NVL72 rack level, total fabric bandwidth reaches 260 TB/s.

NVLink 6's doubled bandwidth cuts gradient sync time for communication-bound training and reduces latency for tensor-parallel inference. NVLink 6 enables all 72 GPUs in the NVL72 rack to act as a single coherent compute unit.

## NVIDIA GPU Roadmap: Rubin Ultra and Feynman

Rubin Ultra (VR300 in press and supply-chain coverage) is NVIDIA's next planned architecture, originally targeting H2 2027. Jensen Huang confirmed the roadmap through 2028 at GTC 2025. SemiAnalysis reported in July 2026 that Rubin Ultra's Kyber rack (the 144-GPU rack built for Rubin Ultra) has slipped to 2028 and that the 4-die version was cancelled. NVIDIA says its roadmap is intact.

At GTC 2025, NVIDIA said Rubin Ultra would double the compute die count from two to four per package. That plan raised FP4 inference to 100 PFLOPS per GPU package and moved to HBM4e with 1 TB per package. The planned rack-scale system is the NVL576. It houses 576 GPU dies and draws about 600 kW, roughly 3x the power of the Vera Rubin NVL72.

Rubin Ultra's expanded fabric is expected to use a next-generation NVLink, reported in press coverage as NVLink 7 but not yet officially named by NVIDIA.

The Feynman architecture will follow in 2028. NVIDIA has disclosed that Feynman pairs with a new CPU called Rosa, replacing Vera, and uses stacked GPU dies; no compute or memory specs have been published.

| Generation | Product name | Target date | FP4 per GPU package | HBM per GPU package | Rack power (NVL system) | 
|---|---|---|---|---|---|
| Blackwell Ultra | GB300 NVL72 | 2025 (shipping) | ~15 PFLOPS | 288 GB HBM3e | ~140 kW | 
| Rubin | VR200 NVL72 | H2 2026 | 50 PFLOPS | 288 GB HBM4 | ~190-230 kW (reported) | 
| Rubin Ultra | VR300 NVL576 | 2027-2028 (reported delay) | 100 PFLOPS (if 4-die ships) | 1 TB HBM4e (planned) | ~600 kW | 
| Feynman | TBD | 2028 | TBD | TBD | TBD | 

## Last Thoughts on NVIDIA Rubin Architecture

The NVIDIA Vera Rubin platform is a generational reset: the Rubin GPU, Vera CPU, Groq 3 LPU, HBM4, and NVLink 6 are co-designed as one system. Cloud availability is ramping through H2 2026, with broad neocloud access expected in 2027.

## FAQ

## ### What is the NVIDIA Rubin architecture?


NVIDIA Rubin is an AI factory platform that combines GPUs, CPUs, networking, storage, and security silicon. It succeeds Blackwell and targets agentic AI, long-context inference, and trillion-parameter MoE models. It entered production in January 2026 and began volume shipments in August 2026.

## ### How does HBM4 improve performance in the Rubin GPU?


HBM4 gives each Rubin GPU 22 TB/s of memory bandwidth, 2.8x Blackwell's 8 TB/s with HBM3e. The gain comes from doubling the interface width per stack to 2,048 bits, running at 10.8 GT/s per pin.

## ### How does the Rubin GPU compare to Blackwell B200 and B300?


The Rubin GPU delivers 22 TB/s of memory bandwidth (2.8x B200 and B300), 50 PFLOPS FP4 (about 5x B200 and 3.3x B300), and 3.6 TB/s of NVLink 6 bandwidth (2x B200). HBM capacity matches B300 at 288 GB.

## ### Where can I rent NVIDIA Rubin GPUs?


Vera Rubin NVL72 is live at Oracle Cloud Infrastructure, CoreWeave, Crusoe and Google Cloud as of November 2026, with racks also running at Microsoft Azure and Nebius. No provider sells Rubin on demand; access is through contracts or sales.

## ### How much power does a Vera Rubin NVL72 rack use?


A Vera Rubin NVL72 rack draws about 190-230 kW, according to analyst estimates. NVIDIA has not published an official figure. Each Rubin GPU reportedly draws up to about 2,300 W in its Max-P power profile.
