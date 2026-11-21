---
id: collect-261121-veille-ia/hardware-semi/tesla-s-ai-chip-roadmap-revealed-sixth-gen-si6-compute-to-hit-80x-current-model-dojo-3-shifts-to-hbm
title: "Tesla's AI Chip Roadmap Revealed: Sixth-Gen SI6 Compute to Hit 80x Current Model, Dojo 3 Shifts to HBM — BigGo Finance"
domain: hardware-semi
role: reference
task: article
actors: []
dates: []
keywords: ["chip", "tesla", "compute", "dojo", "roadmap", "inference", "memory", "process"]
source: https://finance.biggo.com/news/1c38ae54-8ac1-40b0-9a77-99e9be6dd830
source_anchor: ""
source_lines: [1, 36]
section: "Hardware_Semi"
---
# Tesla's AI Chip Roadmap Revealed: Sixth-Gen SI6 Compute to Hit 80x Current Model, Dojo 3 Shifts to HBM

Tesla recently disclosed a revised AI accelerator roadmap, offering for the first time approximate performance targets for its sixth-generation inference chip, the SI6, and confirming that its in-house Dojo training accelerator will return with an entirely new architecture. The roadmap also shows that Tesla has switched its AI chip naming convention entirely from the "AI" prefix to "SI."

Under the plan, the SI6 will deliver 80x the compute of the fourth-generation inference chip, the SI4, currently in volume production and deployed in vehicles, with 9x its memory capacity. Using the SI4's estimated 216 to 576 TOPS as a baseline, the SI6's compute range works out to roughly 17,280 to 46,080 TOPS, or 17.28 to 46.08 POPS. Tesla did not disclose which data formats the SI6 supports, so these figures are estimates.

On the training side, the third-generation Dojo (D3) abandons the wafer-scale integration and distributed on-chip SRAM architecture that the first-generation Dojo relied on, shifting instead to a chiplet-based design combined with 2.5D and 3D packaging and stacked HBM. The original Dojo D1 interconnected 25 processors with 11GB of SRAM through wafer-level packaging, but the memory demands of modern AI models — in parameters, activations and intermediate data — have made SRAM unattractive at the capacity level.

It remains unclear, however, whether Tesla has completely abandoned wafer-scale integration. In theory, custom HBM modules could be stacked atop logic dies connected through wafer-level integration, but that path would magnify design complexity, thermal and manufacturing challenges, and would lower yields while driving up costs. The company said only that the D3 will be optimized for "space, training and scaling," and the reference to extraterrestrial deployment implies that at least some SKUs will require radiation hardening. Compute, memory capacity, process node and timeline for the D3 were not disclosed.

On process and packaging, the division of labor across generations is now fairly clear:

| Chip | Positioning | Process | Packaging/Memory | Compute gain vs. SI4 | 
|---|---|---|---|---|
| SI4 (formerly HW4/AI4) | Current volume-production inference chip | Samsung 7nm-class | FCBGA, ~16GB GDDR6 | Baseline | 
| SI5 (formerly HW5/AI5) | Next-gen inference chip | TSMC 3nm-class, Samsung 2nm-class | Multi-chip module (MCM), 96GB | ~40x | 
| SI6 | Sixth-gen inference chip | Samsung 2nm-class | MCM continued, ~144GB | 80x | 
| Dojo D3 | Training accelerator | Undisclosed | Chiplet + 2.5D/3D packaging + stacked HBM | Undisclosed | 

Note: The SI5 and SI6 compute multiples come from Tesla presentation materials; absolute compute figures are estimates derived from the SI4. The SI4 process is taken as Samsung 7nm-class; the "5nm-class" wording in the presentation slides appears to be an error.

The SI5 is expected to draw between 250W and 800W depending on the application, with 6x the memory of its predecessor at 96GB, and memory bandwidth ranging from 768GB/s to 1,536GB/s depending on whether GDDR6 or GDDR7 is used. Musk has explained that the SI5's 40x gain over the SI4 comprises two parts: an 8x compute increase from a denser process and more execution units, plus a further 5x gain from silicon-level optimizations.

On manufacturing, Tesla has signed an agreement with Samsung Foundry to build the sixth-generation inference accelerator through 2033. Combined with the 2029-2030 launch window and Samsung's updated foundry roadmap, the SI6 could use the automotive-grade SF2A, the performance-enhanced SF2P, or even the SF2Z with backside power delivery. Notably, Tesla does not envision producing the chip on Intel's 14A process at Terafab in the 2030s.

As a cross-reference, Nvidia's Rubin GPU offers 50 NVFP4 PFLOPS of inference performance and 35 NVFP4 PFLOPS of training performance, plus 17.5 FP8/FP6 PFLOPS, and has already begun shipping to customers; the SI6 will not arrive until 2029-2030, so the two are not competitors in the same time window.

The naming change also merits attention. Tesla's previous chip generations were named AI4, AI5 and AI6, but the latest presentation has standardized them as SI4, SI5 and SI6. The shift is tied to U.S. President Donald Trump's push to replace "artificial intelligence" (AI) with "super intelligence" (SI); Musk recently confirmed that SpaceXAI will be renamed SpaceXSI for the same reason.

In terms of industry impact, the Dojo project has had a checkered history: Musk canceled the entire wafer-scale Dojo project in August 2025, then said in August 2026 that SpaceX and xAI would use Nvidia hardware exclusively in their ground and space data centers. The D3's return in the form of a conventional accelerator built on chiplets plus HBM means Tesla is shifting from architectural risk-taking to engineering feasibility on training chips, and that its in-house silicon and the Nvidia ecosystem will coexist for some time to come.

For the supply chain, the SI5 being split between TSMC and Samsung and the SI6 being locked to Samsung's 2nm-class process through 2033 lengthens order visibility, but yields and costs for 2nm-class automotive chips remain variables. For investors, the roadmap offers direction rather than financial guidance: the SI6's 80x gain rests on an estimated baseline, all key D3 parameters are missing, and the real validation point will not come until around 2029.

Once added, BigGo Finance appears first in Google Search Top Stories, so you get the broadest, most up-to-the-minute, and most comprehensive global financial news first.

Add preferred source
