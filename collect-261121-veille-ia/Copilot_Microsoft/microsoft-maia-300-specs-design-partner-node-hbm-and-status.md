---
id: collect-261121-veille-ia/copilot-microsoft/microsoft-maia-300-specs-design-partner-node-hbm-and-status
title: "Microsoft Maia 300: Specs, Design Partner, Node, HBM and Status"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["microsoft", "maia", "chip", "memory", "owner", "node", "azure", "partner"]
source: https://customsiliconlist.com/microsoft-maia-300/
source_anchor: ""
source_lines: [1, 37]
section: "Copilot_Microsoft"
---
## Every published figure

Source per value
- Owner The company that specified the chip and runs it in its own cloud, data centers or products.
- Microsoft 1
- Role What the owner says the chip is for: training, inference, both, or a general-purpose server CPU.
- Inference 1
- Foundry The fab that manufactures the die (TSMC, Samsung, SMIC, Intel).
- TSMC 1
- Node Process node as the owner or foundry states it, for example 'TSMC N3' or '7 nm'.
- TSMC N2P 1
- Memory On-package memory type (HBM2e, HBM3, HBM3E, LPDDR5) or, for CPUs, the memory the socket drives (DDR5).
- HBM4 1
- Memory capacity GB On-package memory per chip in GB. Blank for CPUs, whose memory sits off-package.
- 288 2
- Memory bandwidth TB/s Peak memory bandwidth per chip in TB/s, as published.
- 18 1
- Precision note What the throughput figures mean: dense or sparse, which precision, per chip or per package, and any figure the owner states only in another unit.
- Maia 300 supports enhanced MXFP4 and sub-3-bit storage formats. A 96-accelerator rack delivers up to 7 times the rack-scale token throughput of Maia 200. 2
- Power W Chip or package power in watts as published (TDP, TBP or typical power; the source says which).
- 950 1
- Largest pod chips The largest scale-up domain the owner states: chips per pod, UltraServer, supernode or rack joined by the chip-to-chip interconnect.
- 96 1
- Status Announced: public roadmap without verified deployment. Sampling: first silicon or limited internal use. Preview: customers can request early access, without a GA claim. Deployed: stated production use or general availability. Discontinued: program stopped.
- Announced 1
- Announced Year the owner first announced the chip publicly.
- 2,026 1
- Deployment scale The largest deployment the owner or a named report states: chip counts, clusters, gigawatts or regions.
- Production deployments begin in 2027 as part of a multi-gigawatt Azure Maia rollout (Microsoft) 1

Not verified in the sources reviewed: design partner, partner basis, bf16, fp16, mxfp8, mxfp4, mx8, mx4, fp8, int8, fp4, how to get it.

## Sources

1. Microsoft: Microsoft to expand Azure Maia system to multi-gigawatt deployments accessed 2026-11-11
2. Microsoft Azure: Deep dive into Azure Maia 300 architecture, October 2026 accessed 2026-11-19
3. Microsoft: Azure Maia multi-gigawatt deployments, October 12, 2026 accessed 2026-11-19
