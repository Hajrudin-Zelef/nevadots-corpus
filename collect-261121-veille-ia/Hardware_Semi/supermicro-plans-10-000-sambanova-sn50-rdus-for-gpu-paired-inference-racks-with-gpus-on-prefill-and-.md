---
id: collect-261121-veille-ia/hardware-semi/supermicro-plans-10-000-sambanova-sn50-rdus-for-gpu-paired-inference-racks-with-gpus-on-prefill-and-
title: "Supermicro Plans 10,000 SambaNova SN50 RDUs for GPU-Paired Inference Racks, With GPUs on Prefill and RDUs on Decode"
domain: hardware-semi
role: reference
task: article
actors: []
dates: []
keywords: ["supermicro", "sn50", "inference", "sambanova", "rdus", "gpus", "racks", "decode"]
source: https://www.storagereview.com/news/supermicro-sambanova-sn50-rdu-inference-racks-10000-chips?amp
source_anchor: ""
source_lines: [1, 27]
section: "Hardware_Semi"
---
SambaNova and Supermicro will build and sell disaggregated inference systems that pair NVIDIA GPUs with SambaNova’s SN50 Reconfigurable Dataflow Units (RDUs), with Supermicro initially planning for 10,000 SN50 chips. The racks join Supermicro’s Data Center Building Block Solutions (DCBBS) catalog, and Supermicro will integrate, test, deliver, and support them. Both companies describe the 10,000 chips as a planning figure for a phased rollout; neither has disclosed a purchase commitment, pricing, or a ship date.

The split follows the two phases of LLM inference. Prefill processes the prompt and is compute-bound, which SambaNova assigns to the NVIDIA GPUs; decode generates the response one token at a time and is memory-bound, which it assigns to the SN50. SambaNova’s case for the pairing is that agentic workloads spend most of their time in decode, because an agent makes thousands of model calls and each response gates its next action, so a faster decode stage shortens the whole loop.

SN50 is SambaNova’s fifth-generation RDU, and the company says it serves the largest models at high throughput without extreme rack power density, so operators can add inference capacity in existing facilities and keep GPUs they already own on prefill duty. SambaNova claims 5x the compute per accelerator and 4x the network bandwidth of its previous generation for the SambaRack SN50, which is air-cooled and scales to 256 accelerators.

SambaRack SN50 carries 16 RDUs per rack, so the initial 10,000-chip plan works out to about 625 racks if Supermicro keeps that density. SambaNova has not published throughput, latency, or power figures for the combined GPU and RDU systems, and Supermicro has not specified the server configurations, GPU models, or interconnects, or how many GPUs and RDUs each system holds. Supermicro’s DCBBS catalog already includes the NVIDIA Vera Rubin NVL72 racks and the Vera CPU rack it positions for agent sandboxes.

Supermicro takes system integration, testing, delivery, and customer support, including on-site service, while SambaNova engineers bring models onto SN50, tune them for the hardware, and assist Supermicro with deployments. For buyers, the change is in the channel: SN50 RDUs can be ordered alongside GPUs from Supermicro in the same way as the rest of a rack, with one vendor responsible for the system.

“Agentic AI is the next wave of inference, and it has to run everywhere,” said Rodrigo Liang, co-founder and CEO of SambaNova. “Supermicro builds AI infrastructure for data centers globally. Now customers can order SN50 RDUs alongside GPUs, from a world-leading partner they trust. That’s how fast, efficient inference gets to scale.”

“Our collaboration with SambaNova expands our broad DCBBS portfolio by enabling heterogeneous architectures that integrate GPU computing with specialized inference acceleration, helping customers deploy efficient, scalable AI solutions,” said Charles Liang, president and CEO of Supermicro.

The companies will start with NeoClouds and sovereign clouds, then expand to enterprises running AI on premises. SambaNova expects the partnership to put substantially more high-performance inference capacity into service in 2027 than it could alone, and it has not named customers or given a schedule for the 10,000 chips. The deal follows SambaNova’s July first close of a $1 billion financing at an $11 billion valuation, which came with JPMorganChase deploying SN40 and SN50 systems for on-prem inference, and its heterogeneous inference collaboration with Intel.

Dell Technologies says it is the first OEM to deliver a fully validated, direct liquid-cooled networking rack, and that the…

Graid Technology plans to bring VROC, the CPU-based NVMe RAID stack it took over from Intel, to AMD EPYC servers.…

HighPoint Technologies, best known for its NVMe RAID and switch adapters, has moved into Ethernet with the Rocket 9500A Series,…

AMD has released ROCm 10.2.0, a stability-focused update to its open GPU software stack that refreshes the serving frameworks, libraries,…

SC26 is under way at McCormick Place in Chicago, and behind the headline launches we have already covered, a second…

Hammerspace has turned its software-defined Data Platform into the Hammerspace Data System and, for the first time, will sell it…
