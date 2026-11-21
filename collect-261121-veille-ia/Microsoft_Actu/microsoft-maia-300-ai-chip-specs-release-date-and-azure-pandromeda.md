---
id: collect-261121-veille-ia/microsoft-actu/microsoft-maia-300-ai-chip-specs-release-date-and-azure-pandromeda
title: "Microsoft Maia 300: AI Chip Specs, Release Date and Azure | Pandromeda"
domain: microsoft-actu
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-10-12"]
keywords: ["microsoft", "maia", "azure", "chip", "date", "specs", "release", "october"]
source: https://pandromeda.com/ai/microsoft-maia-300-release-date-specs-and-what-it-means-for-azure
source_anchor: ""
source_lines: [1, 120]
section: "Microsoft_Actu"
---
# Microsoft Maia 300: Release Date, Specs and What It Means for Azure

Microsoft unveiled its third-generation in-house AI chip on October 12, with multi-gigawatt Azure deployment planned for 2027.

Microsoft Maia 300 is the company's third-generation, in-house AI accelerator chip, unveiled on October 12, 2026, and built specifically to run AI inference — the day-to-day work of answering prompts and generating tokens — inside Azure data centers. Microsoft says the chip will deliver up to 40% more tokens per megawatt than leading GPU-based systems and, in a full 96-accelerator rack, up to seven times the token throughput of its predecessor, Maia 200. Production deployment is planned for 2027, when Microsoft intends to scale the Azure Maia family to "multi-gigawatt" capacity, making it the primary inference engine behind Copilot, Microsoft 365, and other first-party AI products — while Nvidia and AMD GPUs continue to handle the bulk of Azure's AI workloads in the meantime.

**Maia 300 at a glance**

- **Announced:** October 12, 2026, in a blog post from Scott Guthrie, EVP of Microsoft's Cloud + AI group
- **Purpose:** AI inference only (not training), optimized for long-context, memory-heavy workloads
- **Memory:** Eight HBM4 stacks per chip — 288GB capacity, 18TB/s peak bandwidth, within a 950W TDP
- **Process:** Compute chiplets built on TSMC's N2P node, in a multi-chiplet design
- **Rack scale:** 96 accelerators, 167kW, 27.6TB of HBM, 1.7PB/s aggregate bandwidth
- **Deployment:** 2027, at multi-gigawatt scale across Azure's data centers

## What is Microsoft Maia 300?

Maia 300 is Microsoft's third custom AI chip, following Maia 100 (unveiled in 2023) and Maia 200 (introduced in January 2026 and now running in production). Unlike a general-purpose GPU, Maia 300 is purpose-built for inference: taking an already-trained AI model and using it to generate answers, summaries, or code at scale. Microsoft's own description, published in its Azure engineering blog, calls it an "inference-first architecture" tuned for long-context, memory-intensive, and decode-heavy AI workloads — the kind of sustained, multi-step reasoning that products like Copilot increasingly rely on.

The chip uses a multi-chiplet design that separates compute, memory-controller, and input/output functions onto different dies, rather than cramming everything onto one piece of silicon. The compute chiplets are manufactured on TSMC's N2P process node. Microsoft says this split improves yield and lets each chiplet be optimized for its own job — bandwidth and connectivity for the memory and I/O dies, raw throughput for the compute dies.

## When was Maia 300 announced?

Microsoft announced Maia 300 on October 12, 2026, through a post on the official Microsoft Blog titled "Microsoft to expand Azure Maia system to multi-gigawatt deployments," alongside a companion feature on Microsoft's news site and a technical deep dive on the Azure Blog, co-authored by Rani Borkar (President, Azure Hardware Systems and Infrastructure) and Saurabh Dighe (CVP, Azure Hardware Systems Planning and Architecture). The announcement did not include a precise retail or enterprise shipping date for Maia 300 itself; Microsoft instead framed 2027 as the year production deployment begins, with more detail promised at Microsoft Ignite 2026 the following month.

The timing follows months of pre-announcement reporting. Industry outlets, citing The Information, reported as early as August 2026 that Microsoft was negotiating with TSMC for capacity to produce hundreds of thousands of Maia 300 chips for 2027 delivery, with a longer-term goal of more than a million units. Those production-volume figures come from analyst and press reporting rather than Microsoft's own disclosures — Microsoft's Azure Maia general manager has previously declined to confirm specific chip counts.

## Key specs and performance claims

According to Microsoft's own architecture post, each Maia 300 accelerator carries eight HBM4 memory stacks, totaling 288GB of capacity and 18TB/s of peak memory bandwidth, inside a 950W thermal envelope. That's a meaningful jump over Maia 200: Microsoft says Maia 300 offers 2.5 times the memory bandwidth, about a third more capacity, and roughly double the HBM bandwidth per watt compared with its predecessor. Compute throughput (measured in FLOPs) is up 36% generation over generation, Microsoft says.

Microsoft's headline efficiency claim is that Maia 300 will deliver up to 40% more tokens per megawatt than "leading GPU-based systems" — a comparison to competing hardware rather than a specific named product. At the rack level, a full configuration of 96 accelerators is rated for up to seven times the token throughput of an equivalent Maia 200 rack, with more than 2.5 times the performance per watt. All of these figures are Microsoft's own projections; they have not been independently benchmarked by a third party, and Microsoft's post does not disclose how the comparison GPU systems were configured.

Maia 300 also adds an enhanced MXFP4 low-precision numeric format and hardware-accelerated data-type conversion, intended to shrink the memory and bandwidth footprint of both model weights and the key-value cache used during long conversations — one of the main costs of running large, context-heavy AI sessions at scale.

| Spec | Maia 200 | Maia 300 | 
|---|---|---|
| Role | Inference | Inference | 
| Memory per accelerator | Lower capacity/bandwidth (baseline) | 288GB HBM4, 18TB/s bandwidth | 
| Memory bandwidth vs. prior gen | Baseline | ~2.5x Maia 200 | 
| Accelerators per rack | Half of Maia 300's density | 96, in a 167kW rack | 
| Rack HBM / bandwidth | Lower (baseline) | 27.6TB capacity / 1.7PB/s aggregate | 
| Rack-scale token throughput | Baseline | Up to 7x Maia 200 | 
| Status (as of Oct. 2026) | In production, serving Copilot | Deploying from 2027 | 

## How Maia 300 compares with Maia 200

Maia 200, which Microsoft introduced in January 2026, is already running in production across data centers in the United States, Europe, and Australia. Microsoft says it currently delivers up to 1.7 times more tokens per watt than leading GPU infrastructure and already powers built-in AI features in Microsoft Teams, Copilot, and Microsoft Cowork, with GitHub Copilot and Microsoft Foundry expected to move onto it by the end of 2026. Maia 300 is designed to take over as the primary inference chip once it reaches volume production, doubling the number of accelerators packed into each rack and roughly doubling performance per watt on top of that — the combination Microsoft is using to justify the "seven times the throughput" rack-level claim.

## Why Microsoft is building its own AI chips

Azure currently runs the overwhelming majority of its AI inference and training on third-party Nvidia and AMD GPUs, and Microsoft's own post is explicit that this will not change overnight — Maia 300 is additive capacity, not a GPU replacement, in the near term. The strategic logic is the same one pushing Amazon (Trainium, Inferentia) and Google (TPUs) toward custom silicon: owning the chip design lets a cloud provider tune hardware specifically for its own model stack, reduce dependence on a GPU supply chain with its own constraints and pricing power, and potentially lower the cost of running inference at the scale Copilot and Azure AI Foundry now require. Unlike AWS and Google Cloud, Microsoft has not made Maia available to outside Azure customers as a selectable instance type — it remains internal infrastructure for Microsoft's own AI products, at least for now.

That contrasts with how rivals are opening up their own custom accelerators; Google, for instance, has begun offering Nvidia's latest Vera Rubin silicon directly as Google Cloud instances, a reminder that the custom-versus-merchant-silicon split in cloud AI infrastructure is being drawn differently by every major provider.

The underlying pressure is the same across the industry: AI models keep getting larger and more context-hungry, and every major lab is racing to ship bigger, more capable systems, from Google's Gemini 4 Argon to Microsoft's own in-house MAI models. Running those models profitably at Copilot's scale is what makes Microsoft willing to spend years and billions of dollars designing silicon rather than simply buying more GPUs.

## What it means for Azure AI pricing

Microsoft's announcement does not mention Azure AI pricing, and it makes no promise that Maia 300 will lower what customers pay for inference. The efficiency gains Microsoft describes — more tokens generated per megawatt of power — are an internal cost metric for Microsoft's own infrastructure, not a published discount. If Maia 300 does reduce Microsoft's own cost of serving tokens through Copilot and Azure AI Foundry, any of that saving reaching customer-facing prices would be a separate business decision Microsoft has not announced. Models already running on Azure, including OpenAI's own API products, continue to be billed under their existing pricing structures for now.

In short: expect any pricing impact, if there is one, to show up gradually and indirectly — through Microsoft's own margins and capacity availability — rather than as a line-item cut tied to this announcement.

## Release and deployment timeline

Microsoft's own timeline, as laid out in its blog post, is deliberately broad rather than date-specific:

- **October 12, 2026:** Maia 300 announced; Maia 200 already in production.
- **End of 2026:** GitHub Copilot and Microsoft Foundry expected to move onto Maia 200.
- **November 2026:** More detail expected at Microsoft Ignite.
- **2027:** Maia 300 enters production deployment; Azure Maia scales to "multi-gigawatt" capacity.

Microsoft has not given a specific month in 2027 for Maia 300's production rollout, nor has it confirmed exact chip volumes. The widely cited figures of 300,000-plus units for 2027 delivery, and a longer-term ambition north of a million chips, trace back to reporting from The Information rather than to Microsoft's own disclosures, and should be treated as industry estimates rather than confirmed targets.

## The TSMC production question

Maia 300's compute chiplets are built on TSMC's N2P process, one of TSMC's advanced nodes, putting Microsoft's silicon in competition for the same leading-edge capacity sought by Nvidia, AMD, Apple, and other TSMC customers. Pre-announcement reports describing Microsoft's capacity negotiations with TSMC were not confirmed by Microsoft at the time of the October 12 announcement, and Microsoft's own materials do not disclose committed wafer volumes, yield rates, or a firm production-start date. That leaves genuine uncertainty about how quickly Maia 300 can scale in 2027 relative to Microsoft's own stated ambitions — a gap analysts have flagged as the main execution risk for the program.

## How Maia 300 stacks up against Nvidia and AMD

Microsoft's "40% more tokens per megawatt than leading GPU-based systems" claim is a comparison against unnamed competing hardware, not a specific, named Nvidia or AMD part, and it is Microsoft's own figure rather than an independently reproduced benchmark. For context, Azure continues to deploy large fleets of Nvidia and AMD GPUs for both training and inference, and Microsoft's post reiterates that GPUs will remain central to Azure's AI capacity even as Maia scales up. The practical takeaway for now is that Maia 300 is best read as Microsoft narrowing its dependence on merchant GPU silicon over time, rather than replacing it in 2027.

## What's next

The next concrete marker to watch is Microsoft Ignite 2026 in November, where Microsoft has said it will share more detail on Maia 300 and the broader Azure Maia roadmap. Beyond that, the real tests will be whether Microsoft can secure the TSMC N2P capacity its 2027 plans imply, whether Maia 300 actually ships to data centers on schedule, and whether any of its efficiency gains show up as lower Azure AI pricing rather than staying an internal cost advantage. Until Maia 300 hardware is running real production traffic, every performance figure in this piece remains a Microsoft-stated projection rather than a verified, independent result.

## Frequently asked questions

### What is Microsoft Maia 300?

Maia 300 is Microsoft's third-generation, in-house AI accelerator chip, designed for AI inference inside Azure data centers rather than for training models.

### When was Maia 300 announced?

Microsoft announced Maia 300 on October 12, 2026, via posts on the Microsoft Blog, news.microsoft.com, and the Azure Blog.

### When will Maia 300 be deployed?

Microsoft says production deployment begins in 2027, when it plans to scale the Azure Maia family to multi-gigawatt capacity. It has not given a specific month.

### How is Maia 300 different from Maia 200?

Maia 300 roughly doubles the accelerators per rack and adds 2.5x the memory bandwidth of Maia 200, for up to 7x the rack-scale token throughput, according to Microsoft.

### Will Maia 300 replace Nvidia and AMD GPUs in Azure?

No. Microsoft says Maia 300 adds inference capacity alongside existing Nvidia and AMD GPUs, which will continue to handle the bulk of Azure's AI workloads.

### Will Maia 300 lower Azure AI pricing?

Microsoft's announcement does not mention pricing or promise lower costs for customers; any efficiency gains are described as internal infrastructure metrics, not a published discount.

## Sources

- Microsoft Blog: Microsoft to expand Azure Maia system to multi-gigawatt deploymentsblogs.microsoft.com
- Microsoft News: Introducing Microsoft's latest custom chip: Maia 300news.microsoft.com
- Azure Blog: Deep dive into Azure Maia 300 architectureazure.microsoft.com
- Microsoft News: Introducing Maia 200, the next generation of AI technologynews.microsoft.com

Theo Park runs the AI desk at Pandromeda. He follows model launches from the frontier labs and the open-weight community, tracks the assistants and developer tools built on them, and explains what each release changes on pricing, capability and safety. His reporting leans on primary sources: model cards, technical reports, API documentation and the companies' own announcements.
