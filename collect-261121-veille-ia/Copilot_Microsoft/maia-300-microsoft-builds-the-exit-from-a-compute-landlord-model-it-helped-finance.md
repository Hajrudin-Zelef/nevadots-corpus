---
id: collect-261121-veille-ia/copilot-microsoft/maia-300-microsoft-builds-the-exit-from-a-compute-landlord-model-it-helped-finance
title: "Maia 300: Microsoft Builds the Exit From a Compute Landlord Model It Helped Finance"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-10-12"]
keywords: ["microsoft", "maia", "compute", "landlord", "finance", "financing", "nvidias", "dense"]
source: https://forkast.news/maia-300-microsoft-builds-the-exit-from-a-compute-landlord-model-it-helped-finance/
source_anchor: ""
source_lines: [1, 31]
section: "Copilot_Microsoft"
---
## The Shift From Borrower to Builder

Microsoft is no longer content to finance the dominance of the chipmakers it depends on. The company that paid Anthropic’s largest cloud bills and sat inside the Broadcom-orchestrated financing web that we traced through Post 131186 and Post 131806 just unveiled its own silicon at the OCP Global Summit. The Azure Maia 300, announced by Scott Guthrie, Microsoft’s EVP of Cloud+AI, via the company’s blog on October 12, 2026, is not a token effort. On vendor-reported specs, it outperforms NVIDIA’s Blackwell B200 on dense FP8 compute, matches the B300’s memory capacity, and delivers more than double the bandwidth at lower power.

## What the Specs Actually Say

The Maia 300 is built on TSMC’s N2P process, a 2nm-class node, with a multi-chiplet design and I/O chiplets on a separate 3nm process. According to vendor-reported figures shared through the Microsoft blog and confirmed via secondary reporting by CNBC, Notebookcheck, and TechPowerUp, the accelerator delivers 6.9 PFLOPS of FP8 performance and 13.8 PFLOPS of FP4. It carries 288GB of HBM4 memory across eight 36GB stacks with a reported 18 TB/s of bandwidth and a 9 Gbps pin speed. The thermal design power is 950 watts, and Microsoft is packing 96 accelerators per rack, with scale-up clusters targeting roughly 12,000 accelerators.

Microsoft claims the Maia 300 will deliver 40% more tokens per megawatt than leading GPU-based systems and up to seven times the rack-scale token throughput of the previous Maia 200 generation. Production is scheduled for 2027, with multi-gigawatt deployment targets. Reuters reports that Microsoft is negotiating with TSMC for more than 300,000 units to meet that delivery window. None of these benchmarks has been independently verified. The official Microsoft blog post sits behind a Cloudflare wall and was not machine-readable at the time of this analysis; the figures cited here are search-confirmed through multiple secondary sources.

## How It Stacks Against NVIDIA

The comparison matters because NVIDIA’s Blackwell architecture is the baseline every cloud provider measures against. The B200 delivers 4.5 PFLOPS of dense FP8 performance (9.0 sparse) with 192GB of HBM3e at 8 TB/s, operating at a TDP of 1,000 to 1,200 watts. The B300 reaches 7.0 PFLOPS dense (14.0 sparse) with 288GB of HBM3e at 8 TB/s, drawing up to 1,400 watts. On these vendor-reported figures, the Maia 300 beats the B200 on dense FP8 compute (6.9 versus 4.5), matches the B300’s memory capacity at 288GB, and offers a 2.25x bandwidth advantage (18 versus 8 TB/s) while drawing 950 watts versus the B300’s 1,400. It does not match the B300 on sparse FP8 (6.9 versus 14.0), and sparse performance is where NVIDIA’s tensor core advantage lives. But on the metrics that govern inference throughput per dollar of power, Maia’s numbers are not decorative.

## Part Eight: From Financing the Landlord to Building a Competing Property

The compute landlord thesis has tracked how chip suppliers moved from selling hardware to financing the purchases of that hardware, creating dependency loops where the vendor is simultaneously supplier, lender, and landlord. Broadcom’s $42 billion convertible note to Anthropic and its reported financing talks with OpenAI spanning $50 billion or more are the clearest expressions of that model. Microsoft has been inside this structure as a customer: Anthropic’s S-1 disclosed $31.4 billion in commitments to Microsoft for Azure compute.

Maia 300 is the first credible attempt by one of those customers to build its own exit. If the chip ships on claimed specs and at the scale Microsoft is targeting, the company would be the first hyperscaler to field a custom accelerator that competes directly with NVIDIA’s flagship in both compute and memory bandwidth. Evercore ISI estimates that Maia could represent approximately 10% of Microsoft’s total AI chip spending by the first quarter of 2027. That is not a replacement for NVIDIA, but it is a structural hedge: the more silicon Microsoft designs in-house, the less leverage any single supplier holds over its infrastructure costs.

Microsoft is also pitching Maia 300 to external customers, including Anthropic, though no confirmed deployments have been announced and no external parties have tested the chip. If Anthropic were to adopt Maia alongside its existing NVIDIA and Google TPU corridors, the compute landlord structure that binds it to Broadcom and NVIDIA would gain a third, internally sourced option with different incentive alignment.

## What Enterprise Deployers Should Watch

The market drew the line immediately: Microsoft shares rose 1.07% intraday while NVIDIA fell 1.22%, according to market data confirmed via Yahoo Finance. For teams building on these platforms, the question is not whether Maia 300 beats NVIDIA’s best on a spec sheet. The question is whether Microsoft can deliver these performance claims at multi-gigawatt production scale starting in 2027, and whether the economics of running inference on Maia are favorable enough to shift workloads away from the NVIDIA ecosystem that most enterprises have already optimized for.

The financing model is also worth watching. Microsoft is not asking private credit funds to underwrite this silicon the way Broadcom structured Anthropic’s compute commitments. It is building the chip and deploying it through its own cloud. That removes the lender-landlord dependency from the equation, but it concentrates a different risk: if Maia underperforms, Microsoft absorbs the cost on its own balance sheet rather than passing it through a financing structure to an external tenant.

The compute landlord thesis just gained its first credible escape route. Whether that route leads somewhere useful depends on execution, not announcements.

*Note: All technical specifications cited are vendor-reported figures from Microsoft’s blog post and secondary reporting by CNBC, TechPowerUp, and Notebookcheck, Oct. 12, 2026. The Microsoft blog post was behind Cloudflare at the time of analysis and was not directly machine-readable. NVIDIA specs are from NVIDIA datasheets and third-party analysis. Evercore ISI estimate confirmed via CNBC. No independent benchmarks of the Maia 300 exist at this time. Intraday stock data from Yahoo Finance, Oct. 12, 2026.*
