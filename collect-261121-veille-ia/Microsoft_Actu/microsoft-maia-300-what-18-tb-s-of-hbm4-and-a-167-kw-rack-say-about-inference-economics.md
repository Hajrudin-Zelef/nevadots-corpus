---
id: collect-261121-veille-ia/microsoft-actu/microsoft-maia-300-what-18-tb-s-of-hbm4-and-a-167-kw-rack-say-about-inference-economics
title: "Microsoft Maia 300: What 18 TB/s of HBM4 and a 167 kW Rack Say About Inference Economics"
domain: microsoft-actu
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-10-12"]
keywords: ["maia", "microsoft", "accelerator", "rack", "about", "inference", "leading", "hbm4"]
source: https://www.metirai.com/blog/microsoft-maia-300-inference-chip-multi-gigawatt-2027-2026
source_anchor: ""
source_lines: [1, 71]
section: "Microsoft_Actu"
---
On October 12, 2026, Microsoft said its Maia 200 accelerator is serving production workloads and unveiled Maia 300, a third-generation, inference-first chip due for production deployment in 2027. Microsoft also said it plans to scale the Azure Maia family to multi-gigawatt deployments from 2027 and to make it the primary inference system for its own products and services. The announcement came in a blog post by Scott Guthrie, Microsoft's executive vice president of cloud and AI.

## What Microsoft announced

According to IT Brief UK, Maia 200 is serving production workloads including Microsoft Copilot. It runs OpenAI models, Microsoft's own MAI models and open-source model families, and is deployed in the United States, Europe and Australia. GitHub Copilot and Microsoft Foundry are expected to follow by the end of 2026.

Microsoft said Maia 200 delivers "up to 1.7 times more tokens per watt" for built-in AI features in Teams, Copilot and Cowork, compared with "leading GPU infrastructure". For Maia 300, it claimed 40% more tokens per megawatt than leading GPU-based systems, and up to seven times the rack-scale token throughput of Maia 200 in a full 96-accelerator configuration.

## The Maia 300 hardware, as reported

The reported figures for Maia 300 are:

- Compute chiplets on TSMC's N2P process node.
- Eight HBM4 stacks with 18.0 TB/s of peak memory bandwidth.
- A 950 W thermal design power per accelerator.
- A rack of 96 accelerators in a 167 kW envelope, with 27.6 TB of HBM capacity and 1.7 PB/s of aggregate HBM bandwidth.
- A software-defined dataflow design with on-die SRAM used as a software-managed scratchpad, and a custom transport over Ethernet-based infrastructure.

Microsoft describes the architecture as optimized for long-context, memory-intensive and decode-heavy workloads, including low-latency conversational services and multi-step agent tasks. The specifications are Microsoft's own, and no independent measurements have been published.

## Reading the claims side by side

### Microsoft's two efficiency claims use different yardsticks

Microsoft-stated efficiency relative to "leading GPU" systems, indexed to 1.0. The Maia 200 figure is per watt for built-in Microsoft features; the Maia 300 figure is per megawatt of facility power.

Source: Microsoft announcement of October 12, 2026 (via IT Brief UK). Vendor claims, not independently verified. Maia 300 "40% more" is shown as 1.4x.

The two efficiency numbers look like a step down (1.7x to 1.4x), but they should not be compared. The Maia 200 claim is per watt, for specific built-in Microsoft features. The Maia 300 claim is per megawatt, which is a facility-level measure that includes cooling and power-delivery losses. Each also compares against an unnamed "leading GPU" system that may differ between the two statements. A like-for-like generational comparison is not possible from the public material, and the seven-times rack figure is the only direct Maia 200 to Maia 300 comparison.

## What the numbers imply: a memory-bound view of decode

Some arithmetic from the reported specifications (our own derivation, not a Microsoft figure):

| Quantity | Derivation | Result | 
|---|---|---|
| HBM per accelerator | 27.6 TB / 96 | about 288 GB | 
| Time to read all HBM once | 288 GB / 18.0 TB/s | about 16 ms | 
| Rack power per accelerator | 167 kW / 96 | about 1.74 kW | 
| Accelerator share of rack power | (96 x 950 W) / 167 kW | about 55% | 

Autoregressive decoding is typically limited by how fast weights and the key-value cache can be streamed from memory, not by raw arithmetic. If a model's active weights and cache filled an entire accelerator's HBM and had to be read once per generated token, the ceiling would be roughly 60 tokens per second per sequence. Mixture-of-experts models read only active parameters, and batching amortizes weight reads across many users, so real throughput differs widely. The point is directional: memory bandwidth and capacity, rather than peak compute, set the shape of the design, which matches Microsoft's "decode-heavy" framing.

The power split is also informative. Roughly 55% of the rack envelope is accounted for by accelerator thermal design power. The remainder covers networking, host systems, cooling and conversion losses. A tokens-per-megawatt claim therefore depends as much on that overhead as on the chip.

## Why custom silicon matters to a hyperscaler

Microsoft still relies heavily on Nvidia and AMD GPUs, according to Data Center Knowledge. Unlike AWS with Trainium and Inferentia, or Google with TPUs, Microsoft has not made Maia available for customer workloads on Azure and did not say whether that might change. Matt Kimball of Moor Insights and Strategy said that because Maia serves mainly internal workloads, competition works differently than in a typical hyperscaler chip race, describing Maia 100 as "a proof of concept", Maia 200 as fixing its issues, and Maia 300 as a push to "go big".

This framing suggests the main comparison is not Maia against a GPU on a benchmark, but Microsoft's cost per token on its own services with and without a captive chip. Related reading on the supply side includes Microsoft's Azure compute rationing, its 38 gigawatt data center expansion and its MAI Base 2 pretraining run.

## What to watch

- **Third-party measurements.** Independent token-throughput and latency numbers for Maia 200 on shared models would anchor the efficiency claims.
- **Customer access.** Whether Maia-backed capacity ever appears as an Azure customer option, as TPUs and Trainium have.
- **The 2027 ramp.** Microsoft gave no precise production date for Maia 300, and a multi-gigawatt target depends on N2P capacity and HBM4 supply.
- **GPU spending.** Microsoft's upcoming results will show whether captive silicon changes its capital mix. Its fiscal Q4 results are the latest baseline.

## What it means for teams building on models

For application builders, the practical effect of custom inference silicon is indirect: it changes the cost and availability of the model endpoints they call, not the code they write. That is one reason model-agnostic tooling, such as the multi-model chat in Metir, keeps teams insulated from which accelerator ultimately serves a request.

## Sources:

- Microsoft unveils Maia 300 for 2027 AI deployments (IT Brief UK, October 12, 2026)
- Microsoft Pushes Custom AI Chips with Azure Maia 300 (Data Center Knowledge, October 13, 2026)
- Microsoft Azure Maia 200 enters production, Maia 300 set to arrive in 2027 (MySmartPrice)

## Image credits

- Hero and in-body photo: "Wenatchee washington azure datacenter aerial 1.jpg" by Tedder, licensed CC BY 4.0, via Wikimedia Commons.
