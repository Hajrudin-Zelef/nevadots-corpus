---
id: collect-261121-veille-ia/gemini-google/google-tpu-arrives-on-oracle-cloud-new-split-architecture-transforms-enterprise-ai-compute
title: "Google TPU Arrives on Oracle Cloud: New Split Architecture Transforms Enterprise AI Compute"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["google", "compute", "oracle", "training", "cloud", "chip", "enterprise", "inference"]
source: https://www.techtimes.com/articles/329577/20261028/google-tpu-arrives-oracle-cloud-new-split-architecture-transforms-enterprise-ai-compute.htm
source_anchor: ""
source_lines: [1, 89]
section: "Gemini_Google"
---
Google and Oracle announced on Tuesday that Google's 8th-generation Tensor Processing Units (TPUs) — the custom AI silicon powering every major Google model, from Gemini to its latest frontier releases — will be available on Oracle Cloud Infrastructure (OCI) starting in calendar year 2027, marking the first time in the chip's 11-year history that any enterprise can access Google's purpose-built AI accelerators without adopting Google Cloud.

The announcement, made at Oracle AI World 2026 at the Venetian Convention and Expo Center in Las Vegas, carries an implication larger than the product news itself: hardware exclusivity is shifting away from being Google's primary competitive moat. The real lock-in for AI platforms is moving toward software ecosystems and data gravity — toward teams already fluent in JAX or TorchTPU, and toward enterprises whose training data already lives inside Oracle's databases.

### Training and Inference Are Different Problems, So Google Built Different Chips

Google's 8th-generation TPU portfolio divides into two architectures, each engineered for a distinct phase of the AI lifecycle. That split — the TPU 8t for training and the TPU 8i for inference — is the design decision most likely to shape how enterprise buyers deploy the chips on OCI, because the two options involve meaningfully different tradeoffs in compute density, memory, and interconnect topology.

The TPU 8t is built for large-scale model training, including both dense and mixture-of-experts (MoE) architectures. A 9,600-chip superpod delivers 121 ExaFLOPs of peak FP4 compute and 2 PB (petabytes) of shared high-bandwidth memory across the pod. Each chip contributes 12.6 FP4 PFLOPs (petaflops) of compute, 216 GB of HBM3e memory at 6,528 GB/s bandwidth, and 19.2 Tbps of bidirectional scale-up bandwidth. Google claims the 8t delivers 2.7× training price-performance versus Ironwood — a figure that, as with all vendor benchmark claims, should be treated as unverified until independently audited.

The TPU 8i is optimized for production inference — specifically the multi-step reasoning, agentic frameworks, and long-context applications that define how most enterprises use finished AI models. Each chip delivers 10.1 FP4 PFLOPs of compute and 288 GB of HBM3e at 8,601 GB/s bandwidth, with a 1,152-chip pod configuration and 384 MB of on-chip SRAM for fast activation storage. Google claims 80% better performance per dollar for low-latency inference versus Ironwood — again, a vendor figure that merits independent verification before informing procurement decisions.

### SparseCore Targets the Embedding Bottleneck That Limits Mixture-of-Experts Models

The TPU 8t introduces a dedicated on-chip component called SparseCore that addresses one of the most persistent bottlenecks in training large-scale recommendation and MoE models: the embedding table lookup.

Embedding tables — the data structures that map tokens or user behaviors to numerical vectors — are accessed in irregular, non-sequential patterns that resist the dense matrix operations TPUs normally optimize for. In MoE architectures, a routing mechanism selects which subset of the model's "expert" layers handles each input, creating similarly irregular memory-access patterns that standard HBM memory handles slowly.

SparseCore handles irregular embedding lookups on dedicated hardware rather than routing sparse lookups through the same systolic array that handles dense matrix math, leaving the main compute fabric available for the tensor operations where it performs best. For enterprises training large recommendation systems, advertising models, or frontier MoE architectures, the presence of SparseCore on the 8t is a meaningful hardware capability — not a footnote.

### Boardfly Topology Halves Network Hops to Serve Agentic AI at Low Latency

The TPU 8i's defining architectural feature is its interconnect topology, which Google calls Boardfly. Where the TPU 8t uses a three-dimensional torus topology — connecting chips in a mesh that spans length, width, and height — the TPU 8i uses a low-diameter design cutting hops by 50% compared to a standard 3D torus at the same scale.

The practical consequence is latency. In a 3D torus with 1,024 chips, a collective communication operation — the all-reduce or all-gather that synchronizes data between chips during inference — may require 16 network hops. Boardfly cuts that to seven hops in a 1,024-chip configuration, reducing the synchronization latency that limits throughput on multi-step inference workloads.

That matters for agentic AI. Single-query inference — a user sends a question, the model responds — tolerates relatively high per-token latency. Agentic inference — where a model reasons through multiple steps, calls tools, evaluates intermediate results, and continues — is far more sensitive to cumulative latency. A topology that cuts per-hop synchronization cost translates directly into throughput for the multi-step reasoning pipelines enterprises are building now.

The 8i also introduces the Collectives Acceleration Engine (CAE), a dedicated block reducing synchronization latency fivefold compared to software-handled collectives. Together, Boardfly and CAE give the 8i a fundamentally different latency profile from the 8t — which is why the two chips belong in different pods, and why an enterprise OCI deployment may run both simultaneously for different workloads within the same organization.

### Optical Circuit Switching Keeps 9,600-Chip Training Pods Running Through Hardware Failures

A 9,600-chip training pod is large enough that hardware failure is not an edge case — it is a scheduled event. Any chip, cable, optical transceiver, or switch in the fabric can fail during a training run that may last weeks. Without active fault recovery, a single failed component can stall the entire job.

Google addresses this with Optical Circuit Switching (OCS), a MEMS-based technology that dynamically reroutes interconnect traffic around failures in real time without requiring a job restart. OCS covers 144 racks and 8,960 TPUs within a single pod, and the switching fabric is reconfigured multiple times per day during production training runs. Google reports that OCS-equipped pods sustain 97% goodput on training runs — meaning more than 97% of available compute time is spent on actual model training rather than recovery operations.

Chip-to-chip communication between TPUs travels over Google's Inter-Chip Interconnect (ICI), a proprietary protocol delivering switch-scale bandwidth per chip while eliminating the overhead of standard network stacks. ICI operates within the pod; Google's Virgo datacenter fabric — a flat two-layer non-blocking network — handles communication between pods, supporting 134,000-plus TPUs in a single routing domain with more than 47 Pb/s of bisection bandwidth. JAX and Google's Pathways orchestration system can extend coordinated training runs to more than 1 million TPUs across this fabric.

### Oracle Supplies Power, Cooling, and Zero Trust Networking: Google Brings the Silicon

Under the OCI arrangement, Oracle provides the physical infrastructure layer: power, cooling, and datacenter networking. For customers, that means Oracle takes on the engineering challenge of deploying chips that require direct liquid cooling at rack density — a significant capital commitment.

"This is going to be a cloud gigawatt-scale investment from Oracle over time," said Karan Batta, Senior Vice President of Oracle Cloud Infrastructure, speaking at Oracle AI World 2026. "When Google launches a new TPU, we will have it on day one," Batta added, characterizing the agreement as a multi-year, multi-gigawatt partnership.

Oracle's high-performance networking platform, Acceleron, connects TPU clusters to the rest of OCI's compute and storage fabric. Acceleron uses AMD Pensando DPUs, hardware-level tenant isolation, Zero Trust Packet Routing (ZPR), and a High Performance Fabric supporting 400G and 800G links via LPO and LRO optical protocols. The fabric is RoCEv2-based, enabling remote direct memory access for high-bandwidth workloads.

The proximity to Oracle's database infrastructure matters for enterprise buyers. Organizations running Oracle as their system of record can run fine-tuning or inference workloads against their existing data without incurring cross-cloud data transfer costs — a real but often underquoted expense in multi-cloud AI deployments.

OCI's existing infrastructure scale provides relevant context: the company's OCI Zettascale10 supports 800,000 NVIDIA GPUs and 16 ZettaFLOPs of peak compute, underpinning the Stargate Oracle-OpenAI supercluster. Adding Google TPUs gives OCI customers access to purpose-built AI silicon within the same security perimeter and billing relationship they use for GPU workloads.

### Is the TPU Software Ecosystem Ready for Teams Not Already on Google Cloud?

The TPU software ecosystem has expanded substantially since the chips first became available on Google Cloud in 2018. The 8th-generation TPUs support JAX, vLLM, SGLang, and Ray, as well as Hugging Face Transformers, TorchTitan, and LiteRT.

The PyTorch integration warrants specific attention. Google's TorchTPU stack — which provides a PyTorch-native path to TPU execution — offers three eager execution modes: Debug (for development), Strict (for validated production runs), and Fused Eager, which provides 50 to 100 percent or better performance improvement over the base eager modes. The standard compilation path routes through torch.compile, then XLA, then StableHLO, preserving compatibility with existing PyTorch model code.

Importantly, TorchTPU is now publicly available on GitHub, having been open-sourced at PyTorch Conference 2026 on October 20, 2026. Teams evaluating TPU compatibility with their PyTorch codebases can now access the repository directly at github.com/google-pytorch/torch_tpu and contribute to an Apache 2.0-licensed codebase.

The remaining barrier for teams not already on Google Cloud is practical rather than technical: JAX proficiency. Enterprises whose ML teams are fluent in PyTorch or TensorFlow can reach TPUs through the framework bridges noted above, but the full performance envelope of the TPU architecture — including SparseCore, the ICI fabric, and Pathways orchestration — is most accessible through JAX. Organizations planning substantial TPU investments should factor in the cost of developing that expertise.

### Placing TPUs on a Rival Cloud Signals a Shift from Hardware Moat to Software Moat

The competitive context of Tuesday's announcement matters. Amazon has Trainium and Inferentia. Microsoft has Maia 200. Meta has its MTIA accelerators. All three represent internal-only custom silicon programs: the chips run internal workloads, but no enterprise outside those companies can access them. Neither has made custom silicon externally available, making Google's OCI arrangement the first of its kind among major hyperscalers.

Google's decision to place its 8th-generation TPUs on Oracle Cloud Infrastructure is therefore without direct precedent among major hyperscalers. The question this decision raises is the more interesting one for the AI infrastructure market: what does it signal about where the competitive moat in AI cloud is moving?

Public statements from Google Cloud leadership indicate the company views TPU access as a potential revenue stream beyond its own cloud. Morgan Stanley analysts have estimated that external TPU deployments could represent a meaningful revenue expansion for Google — though those figures remain speculative and unverified by Google's own disclosures.

The deeper signal is architectural rather than commercial. For a decade, the TPU was Google's hardware moat: enterprises that wanted the chip had to adopt Google Cloud, and that dependency created gravity toward Google's data, tooling, and services. Placing the chip on Oracle Cloud does not eliminate that gravity — JAX, Pathways, and the rest of the software stack still require Google expertise — but it decouples the chip from the cloud. If the moat is now the software ecosystem and the teams who have invested in it, rather than the physical hardware, then Google's strategy is to expand adoption through any available distribution channel, knowing that software familiarity is the harder thing to replicate.

Amin Vahdat, Senior Vice President and Chief Technologist for AI and Infrastructure at Google, holds ACM Fellow and NAE membership. His infrastructure work — including contributions to Google's B4 software-defined networking architecture and co-authorship of the P4 programming language for programmable network data planes — directly underlies some of the ICI and Virgo networking technology deployed in these pods. "The next generation of AI will require a diverse range of compute," Vahdat said in Tuesday's announcement. "Bringing Google TPUs to OCI will give more organizations access to this technology and greater flexibility in how they build and run their most complex AI applications."

## Frequently Asked Questions

### When will Google TPUs be available on Oracle Cloud Infrastructure?

Google TPUs on OCI will be available starting in calendar year 2027, according to Oracle's announcement at Oracle AI World 2026. Oracle's Karan Batta described a multi-year, multi-gigawatt commitment and stated that Oracle will have future TPU generations available on day one of their release. Initial deployment capacity has not been publicly specified, though Batta's "multi-gigawatt" characterization indicates a commitment well beyond a pilot program.

### Do I need to rewrite my existing PyTorch or TensorFlow code to run on Google TPUs?

Not necessarily. Google's TorchTPU stack provides a PyTorch-native path to TPU execution, including a Fused Eager mode that delivers 50 to 100 percent or better performance improvement over standard eager execution. Hugging Face Transformers, vLLM, SGLang, and Ray are also supported frameworks. TorchTPU was open-sourced on October 20, 2026 at PyTorch Conference 2026 and is now publicly available on GitHub. The highest-performance workflows — particularly those using SparseCore, the ICI fabric, and multi-pod orchestration — are most accessible through JAX, and teams planning significant TPU investments should factor in developing JAX expertise.

### What makes the TPU 8i different from the TPU 8t, and how do I know which one my workload needs?

The 8t is for training: it prioritizes raw compute throughput (9,600 chips per superpod, 121 FP4 ExaFLOPs), large shared memory (2 PB per superpod), and the SparseCore accelerator for MoE and recommendation model training. The 8i is for inference: it prioritizes low latency through Boardfly topology (which reduces network hops by more than 50%) and the Collectives Acceleration Engine (which reduces synchronization latency 5 times), in a 1,152-chip pod suited for production serving. If your workload is iterative model development and training, you want the 8t. If your workload is serving a finished model at scale — especially an agentic or multi-step reasoning system — you want the 8i.

### If Google's competitive moat is now software rather than hardware, does placing TPUs on Oracle Cloud put my organization in a better or worse negotiating position?

It depends on how deep your Google stack runs. Enterprises accessing TPUs through OCI will still depend on Google for the ICI/Virgo fabric, JAX, Pathways, and TorchTPU — the software and interconnect layer that actually makes TPUs perform at scale. Oracle provides the physical infrastructure and the security perimeter. This arrangement gives buyers more supply-chain options (Oracle's multi-cloud contracts versus Google Cloud contracts) without eliminating Google's technical dependencies. Teams that build deep JAX fluency in exchange for TPU performance gains are effectively trading one form of lock-in for another. That tradeoff may be worth it for workloads where TPU performance advantages are large and durable — but it is a tradeoff to enter with full awareness, not just a procurement diversification.
