---
id: collect-261121-veille-ia/hardware-semi/sambanova-eyes-10-trillion-parameter-models-for-agentic-ai-with-new-chip-aiwire
title: "SambaNova Eyes 10-Trillion Parameter Models for Agentic AI with New Chip - AIwire"
domain: hardware-semi
role: reference
task: article
actors: []
dates: ["2026-02-25"]
keywords: ["sambanova", "models", "sn50", "company", "says", "agentic", "data", "memory"]
source: https://www.hpcwire.com/aiwire/2026/02/25/sambanova-eyes-10-trillion-parameter-models-for-agentic-ai-with-new-chip/
source_anchor: ""
source_lines: [1, 23]
section: "Hardware_Semi"
---
Your company’s AI agents can recommend a financial transaction, update a customer record, or initiate...

## Off the Wire Press Releases

SambaNova has unveiled its latest chip, the SN50, which it says is five times faster than Nvidia Blackwell and offers 3X the throughput, enough oomph to run agentic AI models exceeding 10 trillion parameters. It also announced the deployment of SN50s into Japan’s SoftBank, a new partnership with Intel, and a $350 million fundraising round.

SambaNova is one of the new chipmakers looking to capitalize on the AI boom and the insatiable demand for data processing that it has unleashed. The company developed its Reconfigurable Data Unit (RDU) architecture, which implements custom processing pipelines where data flows through the complete computation graph, to address the inefficiencies in data movement experienced by instruction set architecture (ISA) used by traditional CPUs and GPUs.

Like the SN40, the SN50 features a tiered memory architecture that combines 64GB of high‑bandwidth memory (HBM), 432 MB of static random-access memory (SRAM), and 256 GB to 2 TB of DDR5. SambaNova says this memory architecture allows it to host the largest AI models, including models with up to 10 trillion parameters. “Models residing in HBM and SRAM can be hot swapped in milliseconds, a capability that is essential for agentic workloads switching frequently between multiple models,” the company writes in a blog post.

SambaNova says the SN50 delivers five times more compute per accelerator and four times more network bandwidth than the SN40. It says that internal benchmarks show that, compared to Nvidia’s Blackwell B200 GPU, the SN50 delivers 5X the maximum speed and more than 3X the throughput for agentic inference workloads running on models like Meta’s Llama 3.3 70B.

SambaNova sells its chips in preconfigured racks, called SambaRacks, which can contain up to 16 individual SN50. The company supports the capability to scale its SambaRacks outward to support a cluster with up to 256 SN50s connected across a multi‑terabyte‑per‑second interconnect. Each SambaRack consumes an average of 20 kW of power, which allows it to use air cooling rather than liquid cooling.

AI inference workloads are the target for SambaNova and its chips, and that story hasn’t changed with the SN50. The company says that its capability to cache input tokens in memory reduces the time-to-first-token (TTFT) relative to mainstream GPU architectures. It can also keep multiple AI models in memory and swap them in a fraction of the time that it takes Nvidia GPUs, the company claims.

SoftBank will be the first company to deploy the SN50, SambaNova said. The Japanese company will deploy SN50 in its next-generation AI data center, SambaNova said.

The company also announced a new collaboration with Intel, which reportedly tried to buy SambaNova in January for $1.6 billion. Instead, Intel is a participant in SambaNova’s Series E round of financing worth $350 million, which it says it will use to expand manufacturing and cloud capacity.

“AI is no longer a contest to build the biggest model,” Rodrigo Liang, co‑founder and CEO of SambaNova, said in a press release. “With the SN50 and our deep collaboration with Intel, the real race is about who can light up entire data centers with AI agents that answer instantly, never stall, and do it at a cost that turns AI from an experiment into the most profitable engine in the cloud.”

*This article first appeared on* HPCwire*.*
