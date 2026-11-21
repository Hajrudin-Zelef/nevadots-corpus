---
id: collect-261121-veille-ia/gemini-google/google-cloud-opens-a5x-vera-rubin-ai-instances-with-1-15-tb-gpu-memory
title: "Google Cloud Opens A5X Vera Rubin AI Instances With 1.15 TB GPU Memory"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-11-07"]
keywords: ["google", "rubin", "cloud", "instances", "vera", "memory", "gpus", "nvidia"]
source: https://bitcoinversus.tech/2026/11/07/google-cloud-a5x-vera-rubin-1152gb-hbm4-bare-metal-ga-november-2026/
source_anchor: ""
source_lines: [1, 35]
section: "Gemini_Google"
---
Google Cloud made its A5X accelerator instances generally available on **November 6, 2026**, introducing NVIDIA Vera Rubin NVL72 systems into its AI Hypercomputer platform. The Google Cloud announcement, updated Friday, confirms the launch after an October preview. The instances target large-scale training, reasoning-model inference and reinforcement learning. General availability, however, does not mean customers can rent the machines instantly: access requires contacting Google to arrange a future capacity reservation.

## Four Rubin GPUs per bare-metal server

Each documented A5X bare-metal instance contains four Rubin GPUs with 288 GB of HBM4 apiece, or **1,152 GB of GPU memory** per machine. Two NVIDIA Vera processors provide 176 physical CPU cores, alongside 1,440 GB of system memory and 48,000 GiB of local NVMe SSD capacity. Bare-metal access eliminates the usual virtual-machine hypervisor layer while leaving customers responsible for configuring appropriate software environments.

An NVL72 domain connects up to 18 A5X instances and 72 Rubin GPUs over NVLink 6, enabling fast exchanges between model partitions. The architecture extends earlier open hardware work examined by BitcoinVersus.Tech in NVIDIA’s Vera Rubin rack designs. A full rack is not a single four-GPU A5X instance; the distinction matters when comparing capacity and memory figures.

## Networking claims need careful units

Google lists a maximum aggregate instance networking figure of 7,200 Gbps: 6,400 Gbps across eight ConnectX-9 GPU-facing SuperNICs plus 800 Gbps across two Titanium host-network interfaces. The figure is a combined interface ceiling, not a promise of 7.2-Tbps public-internet downloads. GPU links use RDMA over converged Ethernet, while Google’s Virgo fabric connects accelerators across racks and buildings.

Google describes a network architecture designed to scale to 80,000 Rubin GPUs at one site and as many as 960,000 across multiple sites. Those values are **architectural scale targets**, not verified counts of active GPUs. The networking emphasis parallels BitcoinVersus.Tech’s earlier comparison of leading AI accelerators and their throughput characteristics. Peak hardware specifications alone cannot establish application-level performance.

## Early users and the next inference bottleneck

NVIDIA identified Ineffable Intelligence, founded by AlphaGo researcher David Silver, as an early A5X customer. The company is using the hardware for reinforcement-learning research. NVIDIA also reported initial WPP creative-agent demonstrations. These are vendor-reported workloads, not independently audited benchmarks or proof of universal reductions in inference cost. NVIDIA’s partnership announcement attributes the system to joint work on cooling, networking and accelerator orchestration.

Google plans to link A5X with Managed Lustre storage and GPU-direct data movement while expanding distributed key-value caching. Several capabilities are presented as future integrations rather than already delivered functionality. BitcoinVersus.Tech previously explained Google’s new C++ inference-serving software and the importance of inference KV-cache placement. Faster processors can still stall when storage and network data paths lag.

## Availability does not remove deployment risk

Google’s official limitations state that A5X is available only in selected regions through future capacity-block reservations; standard on-demand, Spot and Flex-start purchasing are not supported. Windows images and conventional Persistent Disk are unsupported, and the standard Compute Engine service-level agreement does not apply. Prospective customers must examine region availability, reservation commitments, reliability terms, workload compatibility and the cost of additional network or storage resources.

The launch confirms a commercial cloud offering rather than merely a prototype rack. Its true economics will be established by repeatable customer measurements of training completion time, token throughput, utilization, power consumption and total operating expense—not by theoretical FLOPS or the maximum cluster size alone.

**Editor’s Note:** The November 6 general-availability date was verified against Google Cloud’s updated GTC Berlin announcement and its official product updates. Hardware, memory, networking, purchasing restrictions and lack of standard SLA were checked against Google’s Compute Engine documentation and GPU machine-type specification. Early-customer claims were attributed to NVIDIA’s announcement. The featured image is an original conceptual editorial illustration; the three in-story images are official manufacturer or cloud-provider visuals. No unverified video ID or social-media post was added. Network bandwidth ceilings and projected multi-site scale are not observed application benchmarks.

*To donate 5,000 sats, open the article on a separate display and scan the original QR code with a compatible Bitcoin wallet. On mobile, use another screen or your wallet’s supported image-scanning feature. Verify the destination **3C9o19EH5HSiwEPyCTmEKzxhNCbo2X6TTb** before sending.*

*We volunteer daily to ensure the information on this platform is Verifiably True. If you would like to support the research, please scan QR code.*

*BitcoinVersus.tech is not a financial advisor. Technical and cloud-infrastructure coverage is informational and should be checked against current vendor service terms.*

## Leave a Reply
