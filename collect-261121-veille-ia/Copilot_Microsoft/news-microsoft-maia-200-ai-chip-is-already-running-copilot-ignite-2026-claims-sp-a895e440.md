---
id: collect-261121-veille-ia/copilot-microsoft/news-microsoft-maia-200-ai-chip-is-already-running-copilot-ignite-2026-claims-sp-a895e440
title: "News microsoft maia 200 ai chip is already running copilot ignite 2026 claims sp"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["microsoft", "maia", "said", "ignite", "copilot", "post", "production", "chip"]
source: ""
source_anchor: ""
source_lines: [1, 36]
section: "Copilot_Microsoft"
---
What Microsoft said, and when
Maia 200 has been rolled out in stages, and the Ignite message is the latest step.
- January 26, 2026: Scott Guthrie introduced Maia 200 as an inference accelerator. Microsoft says it is built to improve the economics of AI token generation. That post said Maia 200 was deployed in the US Central region near Des Moines, Iowa, with US West 3 near Phoenix, Arizona, coming next.
- October 12, 2026: Guthrie said Maia 200 was serving Microsoft production workloads, including Microsoft Copilot, and supporting the MAI team's model development. He said GitHub Copilot and Microsoft Foundry would follow by the end of the year.
- November 17, 2026 (Ignite): Microsoft AI repeated that Maia 200 is in production and quoted the 1.7x tokens-per-watt figure. That post did not describe production as starting at Ignite.
I'd treat "scaled production at Ignite" as loose shorthand. The more accurate reading is that Ignite confirmed a deployment already underway.
The hardware
Microsoft's January technical post gives these specifications:
- Process: TSMC's 3nm node, with more than 140 billion transistors per chip.
- Compute: Native FP4 and FP8 tensor cores. Each chip delivers over 10 petaFLOPS at FP4 and over 5 petaFLOPS at FP8, within a 750W SoC power envelope.
- Memory: 216GB of HBM3e at 7 TB/s, plus 272MB of on-chip SRAM.
- Networking: A two-tier scale-up network built on standard Ethernet. Each accelerator exposes 2.8 TB/s of bidirectional scale-up bandwidth, and collective operations are supported across clusters of up to 6,144 accelerators. Within a tray, four accelerators are directly connected.
The 6,144 figure is the design ceiling for a cluster. It doesn't mean Microsoft has built clusters that large.
Where it runs
In October, Microsoft said Maia 200 was deploying across the United States, Europe and Australia. It said the chip runs OpenAI models, Microsoft's own MAI models and leading open-source model families.
Microsoft named Copilot as a live workload. It named GitHub Copilot and Foundry as coming next. For Foundry customers, any Maia-backed capacity is still on the roadmap, not in production.
The 1.7x claim
Microsoft's October post says Maia 200 delivers up to 1.7x more tokens per watt for built-in AI features in Teams, Copilot and Cowork, compared with "leading GPU infrastructure." Before you repeat that number, note three limits:
- It's Microsoft's own figure. The Ignite post repeats it without a test setup, a named comparison GPU or a workload mix. Other coverage has also noted that the comparison is with unnamed GPUs.
- It measures efficiency, not response time. Tokens per watt compares throughput to power draw. Some write-ups say the gain comes "without slowing response times," but I found no Microsoft statement saying that. I've left it out.
- It is a different metric from the January claim. In January, Microsoft claimed 30% better performance per dollar than the latest hardware in its fleet. That is a cost comparison against Microsoft's own fleet, not a power comparison against GPUs.
What comes next
The October post said Microsoft will scale Azure Maia to multi-gigawatt deployments starting in 2027. The stated goal is to make Maia the primary inference system for Microsoft products and services. That is a plan. Microsoft hasn't said this capacity is installed, and it hasn't said Maia will replace GPUs across Azure.
Maia 300 is a separate future generation. Microsoft says it will begin production deployments in 2027. It projects 40% more tokens per megawatt than leading GPU-based systems, and up to 7x Maia 200's rack-scale token throughput in a 96-accelerator configuration. Those are Maia 300 projections, not Maia 200 results.
Industry coverage frames custom silicon like this as complementing Nvidia and AMD accelerators, not immediately replacing them. That fits Microsoft's own description of Maia as part of a heterogeneous infrastructure.
What it means for Windows and Microsoft 365 admins
- Nothing changes in your tenant. The announcement doesn't introduce new licensing, admin controls or user-facing settings. Maia is back-end infrastructure.
- You can't pick the hardware. Microsoft hasn't said Copilot customers can choose which hardware serves their requests, or that they'll see a Maia indicator.
- Developers have a preview option. Microsoft previewed a Maia SDK in January. It includes PyTorch integration, a Triton compiler, an optimized kernel library, low-level programming in NPL and a simulator with a cost calculator.
- Watch for pricing. The efficiency gains matter to customers only if they show up as price, capacity or latency. Microsoft hasn't tied the 1.7x figure to customer pricing.
Bottom line
Maia 200 is a real, deployed inference chip that Microsoft says is running production Copilot workloads in three regions. Ignite repeated that status; it didn't launch it. The headline efficiency number is a Microsoft claim with no published methodology. GitHub Copilot, Foundry and the multi-gigawatt build-out are all still plans.
References
- Microsoft Moves Maia 200 AI Accelerator Into Scaled Production - Technetbook Technetbook · 2026-11-18T23:10:00+00:00
- Maia 200: The AI accelerator built for inference - The Official Microsoft Blog blogs.microsoft.com
- Microsoft to expand Azure Maia system to multi-gigawatt deployments - The Official Microsoft Blog blogs.microsoft.com
