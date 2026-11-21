---
id: collect-261121-veille-ia/gemini-google/oracle-cloud-infrastructure-to-offer-googles-tpu-accelerators
title: "Oracle Cloud Infrastructure to Offer Google’s TPU Accelerators"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-10-27"]
keywords: ["google", "tpus", "cloud", "oracle", "said", "infrastructure", "training", "access"]
source: https://www.unite.ai/oracle-cloud-infrastructure-to-offer-googles-tpu-accelerators/
source_anchor: ""
source_lines: [1, 39]
section: "Gemini_Google"
---
## Key PointsOracle becomes the first hyperscale cloud outside Google to host Google TPUs.

View more

- Oracle becomes the first hyperscale cloud outside Google to host Google TPUs.
- Google splits its newest TPU line into separate training and inference architectures.
- Customers gain access to competing AI accelerator designs within a single cloud platform.

Oracle and Google announced on October 27, 2026, at Oracle AI World in Las Vegas that Google’s Tensor Processing Units (TPUs) will be available through Oracle Cloud Infrastructure (OCI). Oracle’s announcement describes OCI as the first hyperscale cloud platform outside Google to offer Google TPUs, with availability planned for calendar year 2027.

## Oracle’s Infrastructure Role

OCI will provide the power, cooling, and datacenter networking to deploy Google TPUs at scale within Oracle’s infrastructure. Oracle and Google said the TPUs can support large-model training and post-training, including dense and mixture-of-experts (MoE) models, as well as production inference for reasoning, agentic, and long-context applications. Customers will be able to choose Google TPUs within OCI optimized for their workloads, performance requirements, and cost objectives.

“Different AI workloads have different compute needs,” said Mahesh Thiagarajan, president of Oracle Cloud Infrastructure. “Bringing Google TPUs to OCI gives customers access to multiple leading AI compute architectures in one cloud.”

Amin Vahdat, senior vice president and chief technologist for AI and infrastructure at Google, said the next generation of AI will require a diverse range of compute. He said bringing Google TPUs to OCI will give more organizations access to the technology and greater flexibility in how they build and run their most complex AI applications.

The companies described Google TPUs as custom accelerators co-designed with open software to power the full AI lifecycle, from frontier training and large-scale inference to the multi-step reasoning demands of agentic AI. They said the TPU portfolio draws on more than a decade of custom AI silicon development, and described the architecture as designed to maximize AI processing from each watt of electricity, with fast access to model data and rapid communication between chips.

## Eighth-Generation TPU Hardware

Oracle and Google described the eighth-generation TPU as two purpose-built architectures, TPU 8t for training and TPU 8i for inference, and said TPU 8t delivers nearly 3x the compute performance per pod over the previous generation while cutting frontier model development cycles from months to weeks. TPU 8i is designed to handle the iterative work of many specialized agents.

Google detailed the eighth-generation design in a technical deep dive published April 22, 2026. According to that post, TPU 8t uses a 3D torus network topology at a scale of 9,600 chips in a single superpod and adds a SparseCore, a specialized accelerator that handles the irregular memory access patterns of embedding lookups while the matrix multiply unit handles matrix math. The chip introduces native 4-bit floating point (FP4) processing, which Google said doubles matrix multiply unit throughput, and carries twice the scale-up bandwidth on its inter-chip interconnect compared with the previous generation.

On networking and storage, Google said its Virgo Network, a scale-out fabric built on high-radix switches, provides up to a 4x increase in data center network bandwidth for TPU 8t training. New TPUDirect RDMA and TPUDirect Storage capabilities move data between TPU memory, network interface cards, and managed storage while bypassing host CPUs, which Google said delivers 10x faster storage access than training on seventh-generation Ironwood TPUs.

TPU 8i, optimized for post-training and high-concurrency reasoning, carries 3x more on-chip SRAM than the previous generation, allowing a larger KV cache to sit entirely on silicon during long-context decoding, Google said. A Collectives Acceleration Engine reduces the on-chip latency of collective operations by 5x, and a serving-optimized topology called Boardfly connects up to 1,152 chips through fully connected boards and groups, with pods scaling to 36 groups linked by optical circuit switches. For a 1,024-chip pod, Google said Boardfly reduces the network diameter from 16 hops in a comparable 3D torus to seven, delivering up to a 50% latency improvement for communication-intensive workloads.

Google’s published specifications list TPU 8t at 216 GB of high-bandwidth memory (HBM), 128 MB of on-chip SRAM, 12.6 peak FP4 petaflops, and 6,528 GB/s of memory bandwidth, with TPU 8i listed at 288 GB of HBM, 384 MB of SRAM, 10.1 peak FP4 petaflops, and 8,601 GB/s of bandwidth. Both systems integrate Arm-based Axion CPU headers, which Google said remove the host bottleneck caused by data-preparation latency.

Google claims TPU 8t delivers up to a 2.7x performance-per-dollar improvement over Ironwood for large-scale training and that TPU 8i delivers up to an 80% performance-per-dollar improvement at low-latency targets for large MoE models, with both chips delivering up to 2x better performance per watt. The company also said that with its JAX framework and Pathways software it can scale distributed training to more than 1 million TPU chips in a single cluster, and that the Virgo Network can link more than 134,000 TPU 8t chips with up to 47 petabits per second of non-blocking bisectional bandwidth.

## Current Lineup and Availability

Google Cloud’s TPU product page lists both TPU 8t and TPU 8i as coming soon. The seventh-generation Ironwood TPU, built with 9,216 liquid-cooled chips per pod and rated at 42.5 exaflops, is listed as generally available alongside the sixth-generation Trillium. Google describes TPUs as custom-designed application-specific integrated circuits that power its Gemini models and flagship products including Search, Photos, and Maps, with native software support for PyTorch and JAX and the vLLM engine for inference.

Google TPUs on OCI will be available in calendar year 2027, ramping over a multi-year period to meet customer needs, the companies said.
