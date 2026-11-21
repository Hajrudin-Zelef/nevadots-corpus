---
id: collect-261121-veille-ia/gemini-google/google-cloud-8th-generation-tpu-family-splits-training-and-inference
title: "Google Cloud: 8th-Generation TPU Family Splits Training and Inference"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["google", "training", "inference", "chips", "generation", "on-chip", "cloud", "family"]
source: https://nand-research.com/google-cloud-8th-generation-tpu-family-splits-training-and-inference/
source_anchor: ""
source_lines: [1, 88]
section: "Gemini_Google"
---
At **Google** Cloud Next 2026, Google unveiled its 8<sup>th</sup>-generation Tensor Processing Unit (TPU) family, splitting its custom silicon lineup into two purpose-built architectures for the first time.

The TPU 8t targets large-scale model training, emphasizing compute throughput and scale-up bandwidth, while the TPU 8i addresses inference and reasoning workloads, emphasizing memory bandwidth and low-latency communication.

Both chips are co-designed with Google DeepMind and run on Google’s Axion **Arm**-based CPU hosts.

Google says that the TPU 8t delivers approximately 2.7x better training price-performance over its 7<sup>th</sup>-generation Ironwood TPU, while TPU 8i offers 80% better inference price-performance. Both chips target TSMC 2nm fabrication and general availability in late 2027, with **Broadcom** designing the TPU 8t (codenamed “Sunfish”) and **MediaTek** designing the TPU 8i (codenamed “Zebrafish”).

# Technical Details

The 8<sup>th</sup>-generation TPU introduces distinct silicon, interconnect topologies, and specialized on-chip accelerators for training and inference. Both chips share the Axion Arm-based CPU host platform, native support for JAX, PyTorch, vLLM, and SGLang, and bare-metal access, but they differ substantially in their memory subsystems, network fabrics, and on-chip specialization.

## TPU 8t: Built for Training

TPU 8t retains and scales the proven 3D torus interconnect topology, packing 9,600 chips into a single superpod that delivers 121 ExaFLOPs of FP4 compute and 2 petabytes of shared high-bandwidth memory. Key advances include:

- **SparseCore accelerator:** A dedicated unit that offloads irregular memory access patterns from embedding lookups and data-dependent all-gather operations, preventing bottlenecks that commonly affect general-purpose accelerators in recommendation and embedding-heavy workloads.
- **Native FP4 compute:** Introduces 4-bit floating-point support at the hardware level, doubling Matrix Multiply Unit (MXU) throughput while reducing energy costs from data movement. This allows larger model layers to fit within local hardware buffers, improving compute utilization.
- **Virgo Network fabric:** A new scale-out networking architecture built on high-radix switches in a flat, two-layer, non-blocking topology. Virgo delivers up to 4x higher data center network bandwidth than the previous generation and connects up to 134,000 TPU 8t chips with 47 petabits per second of non-blocking bisectional bandwidth in a single fabric. Combined with JAX and Pathways, the system scales to over one million TPU chips in a single logical training cluster.
- **TPUDirect RDMA and TPUDirect Storage** enable direct data transfers between TPU HBM and network interface cards, bypassing the host CPU. When paired with Managed Lustre 10T storage, Google claims 10x faster storage access than Ironwood, keeping MXUs saturated during large multimodal dataset ingestion.
- **Reliability, Availability, and Serviceability (RAS):** TPU 8t targets over 97% goodput through real-time telemetry, automatic detection and rerouting around faulty inter-chip interconnect links, and Optical Circuit Switching that reconfigures hardware in response to failures without human intervention.

## TPU 8i: Inference Accelerator

TPU 8i abandons the 3D torus topology in favor of a new Boardfly interconnect, inspired by Dragonfly topology principles and optimized for the all-to-all communication patterns common in Mixture-of-Experts (MoE) models and multi-agent reasoning workloads. The architecture centers on several targeted innovations:

- **Expanded on-chip SRAM:** 384 MB of on-chip SRAM, 3x the previous generation, sized specifically for the KV cache footprint of reasoning models at production scale. This keeps the active working set on-chip and reduces idle time for cores during long-context decoding.
- **Collectives Acceleration Engine (CAE):** Replaces the four SparseCores in Ironwood with a dedicated chiplet-based CAE that accelerates reduction and synchronization during autoregressive decoding. Google claims 5x lower on-chip latency for collective operations, directly improving throughput for concurrent agent workloads.
- **Boardfly ICI topology:** Connects up to 1,152 chips in a hierarchical structure: four-chip rings serve as building blocks, eight boards form fully connected groups via copper cabling, and 36 groups link through Optical Circuit Switches into a pod. The maximum network diameter drops from 16 hops (in a comparable 3D torus) to seven hops, a 56% reduction that lowers tail latency for all-to-all communication.
- **Higher memory bandwidth:** 288 GB of HBM at 8,601 GB/s, approximately 1.3x the bandwidth of TPU 8t (6,528 GB/s), reflecting the memory-bandwidth-intensive nature of inference workloads where feeding the decoder at low latency is the primary constraint.

The following table summarizes key specifications across both architectures:

| **Feature** | **TPU 8t** | **TPU 8i** | 
| Primary Workload | Large-scale pre-training | Inference, serving, reasoning | 
| Design Partner | Broadcom (Sunfish) | MediaTek (Zebrafish) | 
| Fabrication | TSMC 2nm | TSMC 2nm | 
| Network Topology | 3D torus | Boardfly (hierarchical) | 
| Specialized Accelerator | SparseCore | CAE (Collectives Acceleration) | 
| HBM Capacity | 216 GB | 288 GB | 
| On-Chip SRAM | 128 MB | 384 MB | 
| Peak FP4 PFLOPs | 12.6 | 10.1 | 
| HBM Bandwidth | 6,528 GB/s | 8,601 GB/s | 
| ICI Bandwidth | 2x prior gen | 19.2 Tb/s (2x prior gen) | 
| CPU Host | Arm Axion | Arm Axion | 
| Superpod Scale | 9,600 chips | Up to 1,152 chips | 
| Target Availability | Late 2027 | Late 2027 | 

# Analysis

For ML engineers and infrastructure teams, Google’s specialization approach simplifies hardware selection but adds a new dimension to capacity planning. Organizations running both training and inference at scale must now provision and manage two distinct chip families, each with its own topology, performance characteristics, and optimization profile.

The upside is that workloads run on silicon tuned to their specific bottlenecks rather than on a compromise architecture.

Additionally:

- The software portability story is a strength. Both chips support JAX, PyTorch (via TorchTPU, now in preview), vLLM, and SGLang. XLA handles the translation between Boardfly and torus topologies behind the scenes, reducing the burden on application developers. Bare-metal access is a new offering that gives performance engineers direct hardware control without the overhead of virtualization.
- The Pallas custom kernel language provides hardware-aware kernel development in Python, enabling teams to optimize for CAE on TPU 8i or SparseCore on TPU 8t. This is analogous to NKI on AWS Trainium, but the depth of the Pallas ecosystem and third-party tooling remains narrower than CUDA.
- The 97% goodput target on TPU 8t, if achieved in production, is a meaningful differentiator. At frontier training scale, each percentage point of lost productive compute translates to days of wall-clock time. The automatic failure detection and rerouting via Optical Circuit Switching reduces the operational burden on site reliability teams managing multi-thousand-chip clusters.

## Competitive Landscape

The TPU 8 family enters a market where custom silicon from hyperscalers is gaining traction, but NVIDIA retains overwhelming ecosystem advantages. The competitive dynamics vary significantly between the training and inference segments that Google is now addressing separately.

| **Dimension** | **Google TPU 8t** | **Google TPU 8i** | **NVIDIA Vera Rubin NVL72** | **AWS Trainium3** | **Assessment** | 
| Architecture | Dedicated training ASIC; 3D torus topology at 9,600-chip superpod scale | Dedicated inference ASIC; Boardfly topology optimized for MoE all-to-all | Unified GPU platform; 72 Rubin GPUs per rack with NVLink interconnect | Unified ASIC; single chip design with all-to-all switched topology for training and inference | Google’s bifurcation is the most aggressive specialization bet; NVIDIA and AWS favor flexible single-architecture approaches | 
| Per-Chip Compute | 12.6 PFLOPs FP4 | 10.1 PFLOPs FP4 | ~5 PFLOPs FP8 per GPU (est.); ~3.6 EFLOPs per rack | 2.52 PFLOPs FP8; 362 PFLOPs per 144-chip UltraServer | Direct FP4-to-FP8 comparisons are misleading; effective throughput depends on workload precision requirements | 
| Memory per Chip | 216 GB HBM at 6,528 GB/s | 288 GB HBM at 8,601 GB/s | HBM3e per GPU; ~20 TB aggregate per NVL72 rack | 144 GB HBM3e at 4.9 TB/s; 20.7 TB per UltraServer | TPU 8i leads on per-chip memory bandwidth; Trainium3 and NVIDIA compete at system-level aggregate | 
| Scale-Out Capability | 134K chips in single Virgo fabric; 1M+ via Pathways across sites | Up to 1,152 chips per pod | Up to 80K GPUs in single DC via Virgo; 960K across sites | 144 chips per UltraServer; 1M+ chips via UltraClusters 3.0 | Google’s Pathways/Virgo multi-site scaling is unique; AWS matches chip count but with different orchestration | 
| Software Ecosystem | JAX, PyTorch (TorchTPU preview), Pallas kernels, vLLM, SGLang | Same as TPU 8t; CAE optimized via Pallas | CUDA, cuDNN, TensorRT, NGC; broadest third-party support | Neuron SDK, NKI for custom kernels, PyTorch/JAX native, vLLM | NVIDIA’s CUDA ecosystem remains the widest moat; Google and AWS are narrowing the gap through framework-native support | 
| Availability | Late 2027 (external customers) | Late 2027 (external customers) | Expected 2H 2026 on Google Cloud (A5X); broader rollout TBD | Generally available (Dec 2025); Trainium4 previewed with NVLink Fusion | Trainium3 has a significant time-to-market lead; NVIDIA Vera Rubin also expected ahead of TPU 8 | 
| Key Differentiator | Co-designed with DeepMind; 97% goodput target; Virgo + Pathways multi-site training | Boardfly topology cuts network diameter 56%; CAE reduces collective latency 5x | CUDA ecosystem depth; universal framework support; GPU versatility | Already shipping at scale; Anthropic as anchor customer; NVLink Fusion roadmap bridges to NVIDIA | Google bets on workload-specific efficiency; NVIDIA bets on ecosystem; AWS bets on time-to-market and cost | 

## Final Thoughts

The 8<sup>th</sup>-generation TPU family is the most architecturally ambitious custom silicon announcement from any hyperscaler to date. By splitting training and inference into dedicated chip lines with distinct interconnect topologies, on-chip accelerators, and memory subsystems, Google is betting that specialization delivers efficiency gains large enough to offset the added complexity of managing two hardware platforms.

The performance improvements over Ironwood are substantial: 2.7x training price-performance, 80% better inference price-performance, and 2x better performance-per-watt.

Open questions remain:

- The late 2027 availability timeline for external customers means Trainium3 and NVIDIA Vera Rubin will have established production deployments before TPU 8t and 8i reach general availability.
- The multi-partner fabrication strategy with Broadcom, MediaTek, and TSMC 2nm introduces supply chain complexity alongside its diversification benefits.
- The fundamental challenge of competing with CUDA’s ecosystem depth persists, even as Google improves PyTorch support and provides bare-metal access.

The key takeaway from this announcement is the validation of workload-specific silicon as the next phase in AI infrastructure evolution. Google’s decision to split its TPU line, backed by its co-design relationship with DeepMind and a diversified supply chain, sets the template for how hyperscalers will architect AI accelerators going forward.

Organizations planning large-scale AI infrastructure for 2027 and beyond need to weigh the efficiency gains of purpose-built silicon against the ecosystem maturity and portability advantages of NVIDIA’s platform. Google has made that evaluation substantially more compelling in this generation.
