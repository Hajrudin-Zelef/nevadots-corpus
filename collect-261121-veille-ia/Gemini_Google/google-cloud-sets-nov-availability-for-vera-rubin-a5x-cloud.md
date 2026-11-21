---
id: collect-261121-veille-ia/gemini-google/google-cloud-sets-nov-availability-for-vera-rubin-a5x-cloud
title: "Google Cloud Sets Nov Availability for Vera Rubin A5X - Cloud"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["google", "storage", "cloud", "rubin", "availability", "vera", "nvidia", "networking"]
source: https://www.c114pro.com/cloud/206401.html
source_anchor: ""
source_lines: [1, 24]
section: "Gemini_Google"
---
Google Cloud’s A5X bare-metal instances, powered by NVIDIA Vera Rubin NVL72 systems, are scheduled for general availability in November 2026, moving the platform from early customer access toward broader commercial deployment. The October 21 announcement also outlines expansion into Europe. NVIDIA identified London-based Ineffable Intelligence, founded by David Silver, as the first A5X customer, using the infrastructure for large-scale reinforcement learning. A5X combines Vera Rubin compute, ConnectX-9 SuperNICs and Google’s fourth-generation liquid cooling within AI Hypercomputer.

Networking connects that compute to a larger system. Google’s Virgo fabric provides Scale Out connectivity within and between buildings, while Google and NVIDIA have also been co-engineering Falcon transport concepts for A5X. Google previously disclosed architectural support for up to 80,000 Rubin GPUs at one site and 960,000 across multiple sites in April. Those figures describe the platform’s intended scale; they do not establish how many GPUs will be available in November.

Google says Managed Lustre will support NVIDIA GPUDirect Storage on A5X, providing an RDMA path into GPU memory that bypasses the CPU and system memory. Planned KV-caching support will extend context capacity beyond GPU high-bandwidth memory by offloading and reusing context across storage tiers. GKE integration adds Managed DRANET for RDMA resource management and Kueue for topology-aware scheduling. Together, these capabilities address data delivery, context reuse and workload placement alongside GPU processing.

| Google Cloud A5X: Key Details | 
| • Availability: November 2026 general availability; European expansion planned. | 
| • Platform: Vera Rubin NVL72, ConnectX-9 SuperNICs and liquid cooling. | 
| • Networking: Virgo Scale Out; 960,000-GPU multi-site scale previously disclosed. | 
| • Storage: Planned GPUDirect Storage support with Managed Lustre. | 
| • Memory: Planned tiered KV-cache offload beyond GPU HBM. | 

“We’re building superlearners that continually improve through their own experience,” said David Silver, founder and CEO of Ineffable Intelligence.

**Analysis**

The engineering significance is the coordinated data path. Google’s storage guidance already identifies checkpointing, reinforcement-learning weight distribution and KV-cache offload as distinct demands on parallel storage. For A5X, coupling storage access with context reuse could reduce time spent moving or recomputing data. Actual gains will depend on the workload; the announcement does not quantify A5X KV-cache performance.

| Company Profile: Google Cloud | 
| Google’s cloud services portfolio spans computing, AI, data analytics, networking, storage and security. | 
| AI infrastructure: AI Hypercomputer integrates accelerators, networking, storage and orchestration. | 
| A5X platform: NVIDIA Vera Rubin NVL72 with Google Virgo networking. | 
| Availability: General availability scheduled for November 2026. |
