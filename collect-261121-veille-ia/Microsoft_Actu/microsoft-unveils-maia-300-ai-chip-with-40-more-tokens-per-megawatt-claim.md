---
id: collect-261121-veille-ia/microsoft-actu/microsoft-unveils-maia-300-ai-chip-with-40-more-tokens-per-megawatt-claim
title: "Microsoft unveils Maia 300 AI chip with 40% more tokens per megawatt claim"
domain: microsoft-actu
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["microsoft", "maia", "more", "tokens", "chip", "megawatt", "systems", "claim"]
source: https://thenyjournals.com/microsoft-unveils-maia-300-ai-chip/
source_anchor: ""
source_lines: [1, 47]
section: "Microsoft_Actu"
---
# Microsoft unveils Maia 300 AI chip with 40% more tokens per megawatt claim

Microsoft unveiled its third-generation Maia 300 AI chip on October 12, 2026, claiming 40% more inference tokens per megawatt and planning a gigawatt-scale rollout from 2027.

Microsoft unveiled Maia 300, the third generation of its in-house AI accelerator, on October 12, 2026, at the OCP Global Summit in San Jose. The chip is designed for inference, the operation of AI models after training. Microsoft plans to make the Maia family its primary inference system for its products and services from 2027.

Microsoft says Maia 300 is expected to deliver 40% more inference tokens per megawatt than leading GPU-based systems, and up to seven times the rack-scale token throughput of Maia 200. The 40% figure is a company projection. Scott Guthrie, Microsoft’s executive vice president of Cloud + AI, is credited with it in one report, and the company has not named the systems it compared against.

## Hardware Specifications and Rack Design

Microsoft’s architecture deep dive, published October 12 under the names of Rani Borkar, president of Azure hardware systems and infrastructure, and Saurabh Dighe, corporate vice president of systems planning and architecture, describes compute dies built on TSMC’s N2P process. Each accelerator has eight HBM4 memory stacks, with 288 GB of capacity and 18 TB/s of bandwidth. Capacity is 33% higher than Maia 200, and bandwidth is 2.5 times higher.

A full rack holds 96 accelerators within a 167 kW envelope, double the accelerator density of Maia 200. Liquid cooling handles more than 80% of the rack’s thermal load. Up to 12,288 accelerators can be linked across 128 racks in one superpod.

Borkar told Reuters the chip is meant to deliver more intelligence for every watt, dollar and byte of memory, TechBriefly reported. The company says the design aims to maximize useful tokens per watt and useful tokens per dollar, aimed at long-context reasoning, conversational AI and multistep agents.

**The 40% claim**

Microsoft says Maia 300 is expected to deliver 40% more inference tokens per megawatt than leading GPU-based systems. The company has not named the systems it compared against, so the figure is a projection.


## Why Cloud Providers Are Building Their Own Chips

Microsoft presents the chips as a way to cut the cost of running services such as Copilot on in-house silicon instead of Nvidia and AMD processors. Maia 200 already runs in Microsoft data centers, serving Copilot, OpenAI models, Microsoft’s MAI models and open-source models.

GitHub Copilot and Microsoft Foundry are scheduled to move to Maia 200 by the end of 2026. Maia 200 is in production across the United States, Europe and Australia, according to Technology.org.

Amazon, Alphabet, Meta, OpenAI and Anthropic are also developing their own chips, according to TechBriefly. Analysts cautioned that major cloud companies are unlikely to stop buying Nvidia chips entirely, since customers need different processors for different workloads.

## What It Means for Nvidia, AMD, TSMC and Customers

Reuters noted that a Nvidia GB300 rack holds 72 GPUs and draws about 142 kW, a comparison TechBriefly reported. Nvidia regularly receives more than half of its quarterly revenue from hyperscalers, according to Yahoo Finance as cited by TechBriefly. On the day of the announcement, Microsoft’s shares rose while Nvidia’s and AMD’s fell, TechBriefly reported.

Microsoft says the compute dies use TSMC’s N2P process. The Information reported in August that Microsoft was negotiating with TSMC for capacity to make more than 300,000 Maia 300 chips for 2027 delivery. Microsoft has not confirmed that figure.

For AI customers, Microsoft says it wants outside AI developers to use the chip. The coverage gives no details of how that would work.

The 40% figure is a company projection, and Microsoft has not named the GPU systems it compared against.


## Unconfirmed Figures and Next Steps

Reports differ on volume. Reuters reported that more than one million Maia 300 chips could be deployed in 2027, while The Information’s August figure for TSMC capacity was more than 300,000 chips for 2027. Microsoft has not disclosed chip volumes, costs or what multi-gigawatt deployment means in practice.

The 40% comparison has no named baseline, and the figure does not appear in the architecture deep dive published the same day. As of October 13, 2026, the chip is slated to enter production in 2027.

Microsoft plans to share more about its AI strategy at Ignite, scheduled for November 17 to 20 in San Francisco.
