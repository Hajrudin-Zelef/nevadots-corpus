---
id: collect-261121-veille-ia/copilot-microsoft/microsoft-maia-300-chip-bets-on-power-and-memory-not-speed-memeburn
title: "Microsoft Maia 300 Chip Bets on Power and Memory, Not Speed - Memeburn"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["maia", "microsoft", "memory", "chip", "tokens", "more", "megawatt", "power"]
source: https://memeburn.com/microsoft-maia-300-chip-bets-on-power-and-memory-not-speed/
source_anchor: ""
source_lines: [1, 67]
section: "Copilot_Microsoft"
---
TL;DR

- Maia 300 pairs four TSMC 2nm compute chiplets with 288GB of HBM4 and ships from 2027.
- Microsoft claims 40% more tokens per megawatt than unnamed "leading GPU-based systems" and 7x Maia 200 rack throughput.
- Maia 300 matches Nvidia Rubin's 288GB of memory with about 20% less bandwidth.
- In agent workloads, CPU tool processing can take up to 90.6% of latency, so an inference chip speeds only part of the loop.

Microsoft unveiled Maia 300, its third-generation in-house AI chip, on Oct. 12, and the number it led with was not a measure of raw compute but of efficiency, with the company claiming 40% more tokens per megawatt than “leading GPU-based systems”. For anyone tracking whether hyperscalers can loosen Nvidia’s grip on AI spending, that choice of headline says more than the chip itself, because almost every design decision Microsoft described targets memory, networking or power rather than processing speed.

Memeburn’s reading is that Maia 300 is a map of where the cost of AI has moved, from the model and the GPU toward the layers around them, and that Microsoft is trying to own those layers for its own products. The caveat is that the 40% figure rests on an unnamed baseline, and the previous Maia generation has so far run mostly inside Microsoft’s own services.

## **What Microsoft Announced With Maia 300**

Maia 300 combines four compute chiplets built on TSMC’s 2-nanometer process with eight HBM4 memory stacks totaling 288GB, plus a device control chiplet and two chiplets that move data in and out of the accelerator, according to SiliconANGLE. The chips ship in trays of four, sit 96 to a liquid-cooled rack and group into superpods of 12,288 accelerators, and Microsoft expects a Maia 300 rack to deliver up to seven times the token throughput of a Maia 200 system.

Microsoft says production deployment starts in 2027, that Maia 300 will become “the primary AI inference system for Microsoft products and services”, and that it is aimed at long-context reasoning, conversational experiences and “sustained, multi-step agentic tasks” across Copilot, Teams, Dragon Copilot, GitHub and Microsoft Foundry, according to the company’s announcement carried by HPCwire.

|  | **Maia 100** | **Maia 200** | **Maia 300** | 
| Announced | Ignite, November 2023 | January 2026 | October 2026 | 
| TSMC process | 5nm | 3nm | 2nm compute chiplets | 
| High-bandwidth memory | 64GB HBM2E, 1.8TB/s | 216GB HBM3e, 7TB/s | 288GB HBM4, about 20% less bandwidth than Nvidia Rubin | 
| Largest cluster | Not disclosed | 6,144 accelerators | 12,288-accelerator superpods | 
| Status | Internal Azure workloads | Live in two US regions | Production deployment from 2027 | 

Memeburn compiled the table from Microsoft’s disclosures and SiliconANGLE’s reporting. Memory capacity has grown 4.5 times across three generations, while the largest connected cluster has doubled from Maia 200 to Maia 300.

## **Why Microsoft Leads With Tokens per Megawatt**

Microsoft’s emphasis on tokens per megawatt follows a framework that Rani Borkar, who runs Azure’s hardware systems, laid out at SEMICON Taiwan in September, arguing that AI infrastructure should be judged by “useful yield” measured in tokens per dollar and tokens per watt, and that “the toughest constraints are often solved beyond the layer where they first appear”, according to Microsoft. Data centers are increasingly limited by how much power they can secure, which Memeburn’s AI electricity statistics track, so a chip that produces more answers from the same megawatt is worth more to Microsoft than one that simply runs faster.

The memory design follows the same logic, because Maia 300 matches the 288GB of Nvidia’s Rubin but has about 20% less memory bandwidth, and Microsoft compensates by keeping data in on-chip SRAM where possible, reading from the nearest HBM stack and compressing data into narrow formats such as MXFP4, which Borkar and Microsoft’s Saurabh Dighe said “reduce memory footprint and data movement, allowing the system to support larger models”. The network is designed to shift bandwidth between traffic inside and between racks so that “jobs can use capacity across rack boundaries, reducing stranded accelerators”, which is a fix for wasted hardware rather than a speed gain.

## **What the 40% Claim Doesn’t Tell You**

Microsoft has not named the “leading GPU-based systems” behind its 40% figure, nor the model, precision or workload it measured, so the claim cannot yet be checked against Nvidia’s Blackwell or Rubin systems. The previous generation followed a similar pattern, since Maia 200’s 30% performance-per-dollar advantage was measured against “the latest generation hardware in our fleet” rather than against a named competitor, according to Microsoft’s Maia 200 announcement in January.

Maia 200 itself has had a limited footprint, launching in January in Microsoft’s US Central region in Iowa with US West 3 in Arizona named as next, and serving OpenAI’s GPT-5.2 models through Foundry and Microsoft 365 Copilot rather than being rented broadly to Azure customers. CNBC reported in May that Anthropic was negotiating to rent Maia 200 capacity for Claude, and Memeburn found no announced deal since. Reports on Maia 300 volume also vary widely, from TSMC capacity talks for more than 300,000 chips in 2027, according to The Next Web, to Reuters’ report of potentially more than one million, and Microsoft has not confirmed either figure.

That leaves Maia 300 as a cost tool for Microsoft’s own services first. Google and Amazon already rent their TPU and Trainium chips to outside customers, Anthropic among them, while Microsoft remains a major Nvidia buyer and has not said when, or whether, Azure customers will be able to rent Maia 300 directly.

## **Why AI Agents Make the Memory and Network Bet Matter More**

Microsoft is pitching Maia 300 for multi-step agentic work, but an agent spends much of its time outside the accelerator. Each step in an agent’s loop can involve a tool call, a web search, a retrieval from a database or a block of code to run, and those steps run on CPUs and across the network rather than on an inference chip. A study by Georgia Tech and Intel researchers found that tool processing on CPUs took up to 90.6% of total latency in some agent workloads, and that code execution accounted for 44% to 79% of a coding agent’s latency, according to the paper on arXiv.

A faster inference chip therefore speeds only part of an agent’s work, which is why Borkar’s framework also credits Azure’s Cobalt CPU with finer power control that lets “significantly more servers” run from the same megawatts. Memeburn’s reading is that agentic AI spreads the bottleneck across CPUs, memory, networking and power at once, and Maia 300 is one piece of Microsoft’s attempt to design all of them together instead of buying each from a different supplier.

## **Where Maia 300 Leaves the Value in AI**

The case for building chips rather than buying them gets stronger as models get cheaper. Investor Chamath Palihapitiya’s six-layer map of the AI stack, from infrastructure and chips up through data, models, execution and applications, notes that “the price of running a model has dropped 1,500x in six years, and intelligence is becoming free”, while describing infrastructure as “the most concentrated layer in the stack”, according to Social Capital’s research. If the model layer keeps commoditizing, the money moves to whoever serves tokens most cheaply and owns the product people pay for.

Microsoft sits on both ends of that chain, with data centers and now three generations of silicon at the bottom, and Copilot, GitHub and Teams at the top, while it licenses frontier models from OpenAI and builds its own as part of its MAI push. OpenAI is pursuing the same logic with its own Jalapeño chip. For Nvidia, the near-term risk is limited because Microsoft is still buying its GPUs at scale, but every Copilot query that moves to Maia is inference spending that no longer passes through Nvidia.

The next markers are Maia 300’s first production racks in 2027, whether Microsoft publishes an independent benchmark that names its GPU baseline, whether an outside customer such as Anthropic signs on, and whether Microsoft can secure enough HBM4 while AI demand keeps memory scarce.

## **FAQs**

### **What is Microsoft Maia 300?** 

Maia 300 is Microsoft’s third-generation in-house AI accelerator, announced on Oct. 12, 2026, and designed for inference. It combines four compute chiplets on TSMC’s 2-nanometer process with 288GB of HBM4 memory, and Microsoft says it delivers 40% more tokens per megawatt than leading GPU-based systems.

### **When will Maia 300 be available?** 

Microsoft plans to begin production deployment of Maia 300 in 2027 and says it will become the primary AI inference system for its own products, including Copilot, Teams, GitHub and Microsoft Foundry. It has not said whether Azure customers will be able to rent it directly.

### **Is Maia 300 faster than Nvidia’s chips?** 

Microsoft has not claimed that. Its main claim is efficiency, at 40% more tokens per megawatt than unnamed leading GPU-based systems, and Maia 300 matches Nvidia Rubin’s 288GB of memory with about 20% less memory bandwidth, according to SiliconANGLE.
