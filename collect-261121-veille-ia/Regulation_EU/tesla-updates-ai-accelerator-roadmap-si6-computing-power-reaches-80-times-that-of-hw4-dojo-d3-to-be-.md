---
id: collect-261121-veille-ia/regulation-eu/tesla-updates-ai-accelerator-roadmap-si6-computing-power-reaches-80-times-that-of-hw4-dojo-d3-to-be-
title: "Tesla updates AI accelerator roadmap: SI6 computing power reaches 80 times that of HW4; Dojo D3 to be deployed for space missions"
domain: regulation-eu
role: reference
task: article
actors: []
dates: []
keywords: ["tesla", "dojo", "wafer-level", "accelerator", "roadmap", "computing", "power", "memory"]
source: https://www.528btc.com/en/news/1261691
source_anchor: ""
source_lines: [1, 79]
section: "Regulation_EU"
---
Tesla has announced an updated roadmap for its AI accelerators, further revealing the company's chip development direction for the coming years. According to this roadmap, the computing power of Tesla's sixth-generation inference accelerator is expected to reach 80 times that of the current fourth-generation chips; meanwhile, the Dojo D3 used for training tasks will abandon the architecture centered around SRAM and shift to a multi-chiplet design, incorporating stacked HBM high-bandwidth memory.

In addition, Tesla has also begun to replace the term "artificial intelligence" ( Artificial Intelligence , AI ) with "super intelligence" ( Super Intelligence , SI ), and has adopted the SI series of names for its AI accelerators. For the same reason, Musk has also recently confirmed that SpaceXAI will be renamed SpaceXSI .

**Tesla AI Accelerator Roadmap**

According to the disclosed information, Tesla's current chip product line and plans are as follows:

**Dojo D3: No longer adopting wafer-level integration?**

IT Home has noted that perhaps the most noteworthy change in this roadmap is that Tesla has restarted its independently developed Dojo training accelerator project.

In August 2025, Musk canceled the entire wafer-level Dojo project, and in August 2026, he stated that SpaceX and xAI would fully adopt NVIDIA hardware in their ground and space data centers. However, the third generation of Dojo D3 will be significantly different from the first two generations, Dojo D1 and Dojo D2.

Tesla describes the next generation of Dojo D3 as a training accelerator based on chiplets (Chiplet), utilizing 2.5D and 3D packaging technology, and is equipped with stacked HBM memory.

This is very different from the wafer-level system architecture initially adopted by Dojo D1 ( System-on-Wafer, SoW ). D1 mainly relies on distributed on-chip SRAM and, through wafer-level packaging technology, connects 25 D1 processors to provide a total of 11GB of SRAM.

The shift from D3 to HBM indicates that Tesla aims to significantly increase memory capacity. Modern AI models require a large amount of memory to store model parameters, activation values, and intermediate calculation data. From a capacity perspective, SRAM is no longer suitable to meet such demands.

Using HBM can make it easier to expand memory capacity, but it also comes with different trade-offs in terms of latency, bandwidth, packaging, and power consumption. It is not yet clear whether Tesla has completely abandoned wafer-level integration technology.

One possibility is that Tesla will still use wafer-level packaging technology to connect the logic chips, and stack custom HBM modules on top of them.

Another possibility is that Tesla believes that wafer-level integration is too costly and only holds significant value in specific architectures dominated by SRAM. If HBM continues to be stacked on logic chips that use wafer-level interconnections, it will further increase design complexity, heat dissipation challenges, and manufacturing difficulties, potentially reducing chip yield. This could also lead to long-term reliability issues and ultimately drive up costs.

Although most of these problems can be solved by adding redundant design, improving process technology, and other methods, such measures will increase costs. Therefore, it seems more likely that D3 will adopt a traditional die architecture rather than wafer-level packaging.

Currently, Tesla has not disclosed the specific computing power, memory capacity, manufacturing process, or development timeline of D3.

However, the company stated that Dojo D3 will be optimized for "space, training, and scalability." This means that the product not only needs to provide sufficient computing performance but also must have strong scalability. This further implies that D3 may adopt a more traditional accelerator architecture rather than a wafer-level packaging solution.

What is more noteworthy is that D3 will also be optimized for deployment in space environments. This may mean that at least some models need to have radiation resistance capabilities.

**SI6: In the next few years, computing power is expected to reach 80 times that of HW4.**

Another important piece of information in the roadmap is that Tesla has disclosed the approximate computing power level of its sixth-generation inference accelerator SI6. Its computing power is expected to reach 80 times that of HW4, and its memory capacity is 9 times that of HW4.

The specific values depend on how the performance of HW4 is measured. According to current estimates, the computing power of SI6 could reach 17,280 to 46,080 TOPS, which is 17.28 to 46.08 POPS. However, it is not yet clear what data formats SI6 supports, so there is still uncertainty around these figures, and they can only be used as a reference.

For comparison, the inference computing power of NVIDIA Rubin GPU is 50, the training computing power is 35, while the computing power of FP8 / FP6 is 17.5.

It should be noted that NVIDIA Rubin has already begun to be delivered to customers, while Tesla's SI6 is not expected to be launched until 2029 to 2030.

Currently, the fourth-generation inference processor used in Tesla cars was initially named HW4, later renamed to AI4, and most recently renamed again to SI4. This chip is manufactured by Samsung using a 7nm manufacturing process and is packaged using the traditional Ball Grid Array (BGA) technology. It is expected to come equipped with 16GB of GDDR6 memory.

It should be noted that there is a purported error in the relevant Tesla presentation, where the manufacturing process of this chip is labeled as 5nm, when it should actually be 7nm.

Tesla describes the SI4 as the performance benchmark for current fully autonomous driving (FSD) technology, and the performance of future generations of chips will be referenced against it. However, Tesla has not officially announced the absolute computing power, memory capacity, and power consumption of this chip, making it difficult to make accurate comparisons.

Elon Musk once stated that the performance of HW4 is 3 to 8 times that of HW3. The official computing power of HW3 is 72 TOPS, and based on this, the computing power of HW4 is estimated to be around 216 to 576 TOPS.

According to the roadmap, the next-generation fifth-generation inference accelerator SI5 (also known as HW5 or AI5) will adopt a multi-chip module MCM design, saying goodbye to the traditional packaging methods used in HW4.

This chip will be manufactured using TSMC's 3nm and Samsung's 2nm process technologies respectively.

Tesla expects that the computing power of SI5 will reach 40 times that of HW4. Musk previously explained that this improvement comes from two aspects: a 8-fold increase in computing power, mainly due to more advanced manufacturing processes and more execution units; and silicon chip optimization further brings a 5-fold performance boost.

Based on this estimation, the computing power of SI5 may reach 8640 to 23040 TOPS. The specific value depends on the actual computing power benchmark of HW4.

However, the expected power consumption of SI5 is also quite high, ranging from 250W to 800W depending on the application scenario. Therefore, there is still a significant degree of uncertainty in estimating its actual performance at this stage.

In terms of memory, the capacity of SI5 is expected to reach 6 times that of the previous generation, namely 96GB. Depending on whether GDDR6 or GDDR7 memory is ultimately chosen, its bandwidth may range between 768GB/s and 1.536TB/s.

As for the sixth-generation inference accelerator SI6, Tesla plans to further increase its computing power to 80 times that of the current model, and the memory capacity will reach 9 times that of the current generation of chips.

According to the above estimates, the computing power of SI6 could reach 17.28 to 46.08 POPS, and the memory capacity would be 144GB. For a AI inference accelerator, this represents a considerable amount of computing power and memory capacity.

Tesla has signed an agreement with Samsung's wafer foundry division, planning for the latter to produce its sixth-generation inference accelerators, with the cooperation lasting until 2033.

The latest roadmap indicates that SI6 will adopt a 2nm manufacturing process. Considering that this chip is expected to be launched between 2029 and 2030, as well as Samsung's recently updated foundry roadmap, SI6 may adopt automotive-grade technology, while SF2A and the performance-enhanced version SF2P are also possible, and even SF2Z which is equipped with backside power supply technology.

It is worth noting that Tesla does not seem to have plans to use Intel's 14A process at the Terafab factory to produce SI6 in the 2030s.

In terms of packaging, SI6 will continue with the multi-chip module (MCM) design introduced by AI5 / SI5 / HW5. This choice is quite reasonable, as Tesla will have accumulated experience in the research and development and production of fifth-generation inference accelerators by then, and it will also be possible to keep the packaging costs within a relatively reasonable range.

Currently, many automobile manufacturers are considering adopting multi-chiplet designs and HBM memory for the next generation of system-level packaging (SiP), which will also increase the manufacturing costs of the related chips.

As for when Tesla will introduce HBM into its inference accelerators, there is currently no clear answer, but it is likely that SI6 will not adopt this type of memory yet.
