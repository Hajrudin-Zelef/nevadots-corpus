---
id: collect-261121-veille-ia/datacenters-infra/sovereign-ai-compute-in-sweden-scaleway-and-sferical-one
title: "Sovereign AI Compute in Sweden: Scaleway and Sferical One"
domain: datacenters-infra
role: reference
task: article
actors: []
dates: []
keywords: ["sferical", "scaleway", "sovereign", "release", "machine", "models", "compute", "sweden"]
source: https://www.supercomputing.news/ai/sweden-sovereign-ai-compute-scaleway-sferical-one
source_anchor: ""
source_lines: [1, 61]
section: "Datacenters_Infra"
---
On October 13, 2026, French cloud provider Scaleway told its ai-PULSE conference in Paris that it will build an AI supercomputer in Sweden on AMD Instinct MI355X GPUs, its first outside France, and rent out the capacity to startups, AI companies, and enterprises across Europe, according to Scaleway's release and Reuters. Two days later in Stockholm, Sferical AI formally launched Sferical One, an NVIDIA DGX SuperPOD built on 16 DGX GB300 systems and reserved for its backers: AstraZeneca, Ericsson, Saab, SEB, and Wallenberg Investments, with bearing maker SKF as a strategic partner.

Sferical One has been running since late summer 2026, Sferical says, and the consortium behind it was first announced in May 2025. Scaleway's machine is at an earlier stage. Its release describes the hardware in the future tense, and Scaleway has not given a GPU count, power rating, site, or opening date.

Both companies call their systems sovereign, and they mean different things by the word. Sferical applies it to a machine its founders invested in together, share among themselves, and plan to run with open-weight models they can inspect. Scaleway, the cloud and AI arm of France's iliad Group, applies it to public cloud capacity run by a European operator under European jurisdiction, with customers free to choose their chip architecture. Side by side, the two systems show how the people paying for sovereign AI compute are defining it: as ownership and control of data in one case, and as the operator's legal home and the buyer's choice of hardware in the other.

## Sferical One: built for the companies that use it

Wallenberg Investments and NVIDIA made that first announcement on May 24, 2025. AstraZeneca, Ericsson, Saab, and SEB would build the system in partnership with Wallenberg Investments, they said, and a joint company would operate it to give the industrial partners secure, sovereign compute. That company, Sferical AI, was formed in August 2025 with Jenny Nordlöw as chief executive and Anders Ynnerman as executive chairman, and its formation release said the system was intended to be located in Linköping. Sferical's April 2026 announcement of SKF as a strategic partner places the infrastructure in that city, and CIO Analytics described the machine as Linköping-based in its launch-day report.

The machine is built on 16 DGX GB300 systems, NVIDIA's rack-scale design in which 72 Blackwell Ultra GPUs and 36 Grace CPUs share one liquid-cooled rack. That makes 1,152 GPUs, the figure Sferical gives on its website. Sferical's release gives no investment figure; the Swedish business daily Dagens industri reported more than SEK 1 billion.

Sferical operates the system, and its members are now starting to put it to work. The release lists drug discovery at AstraZeneca, foundation models for telecommunications at Ericsson, defense applications at Saab, financial services at SEB, and predictive maintenance, quality control, and R&D at SKF. Ericsson says its own work there so far centers on AI-based network operations, and its chief technology officer, Erik Ekudden, said the cooperation gives the company "access to sovereign infrastructure for model training and refinement."

On the software side, Sferical plans services built on NVIDIA's Nemotron family of open multimodal models. Because the weights are open, the release says, members can inspect them, fine-tune the models for their own uses, and run them in environments that meet their security requirements. Nordlöw said Sferical One lets the companies "train and run AI models at a larger scale on a sovereign platform."

## Scaleway in Stockholm: capacity for rent

Scaleway's release presents the machine, which will pair the MI355X GPUs with AMD's ROCm open software, as a way to widen the range of hardware European customers can rent, and names the partners assembling it. Bull, the French supercomputer maker, will integrate and deploy the cluster, from racks and cooling integration through installation in the data center, and will maintain the hardware afterward. Giga Computing supplies GIGABYTE G4L3-ZX1-LAT4 servers built for direct liquid cooling, each with eight MI355X GPUs and two AMD EPYC 9005 processors, in a multi-rack deployment. HPE Networking provides the Ethernet fabric, VAST Data the data platform, and Motul the cooling fluid.

Tele2 is Scaleway's route into the Swedish market. In May 2026, the operator began offering Scaleway's public cloud alongside its own private cloud services as a hybrid package for Swedish organizations, and Scaleway's release says the Stockholm machine builds on that partnership. Stefan Trampus, who runs Tele2's business-to-business unit, said in Scaleway's release that Tele2 sees growing demand among its customers for European cloud and AI services. Scaleway said it picked Sweden for its AI ecosystem and its largely low-carbon electricity.

The release's subhead calls the Stockholm system "Europe's first commercial AMD AI supercomputer." In reporting that claim, Reuters noted that AMD chips already power European research supercomputers such as LUMI in Finland, and that US cloud companies including Oracle and Vultr rent AMD chips to European customers.

Scaleway's broader expansion announcement the same day words it differently, describing the Stockholm cluster as carrying "the ambition of delivering one of the first computing infrastructures of its kind from a European cloud provider." The same document announces a separate NVIDIA B300 cluster of several thousand GPUs in Copenhagen, and its tens-of-thousands-of-GPUs total covers all three GPU Zones, in Paris, Copenhagen, and Stockholm, with no size for the Swedish machine.

Other AI clouds are expanding in the same region. An NVIDIA blog post dated October 14 projects 1 gigawatt of AI cloud capacity across the Nordics by the end of 2027 and names seven of the cloud providers behind it, among them Nscale, which SCN has covered as it plans a UK AI supercomputer at Loughton.

## How the two are built

The designs scale up from different building blocks. In a GB300 NVL72 rack, all 72 GPUs share one NVLink domain with 130 TB/s of total NVLink bandwidth and 20 TB of HBM3E memory, according to NVIDIA's specifications, so GPUs within a rack exchange data without going out to the wider network. Sferical has 16 such racks.

Scaleway's building block is the eight-GPU server. Each MI355X carries 288 GB of HBM3E at 8 TB/s and is rated at 1,400 W of total board power, and each has seven Infinity Fabric links at 153 GB/s for reaching the other GPUs in its server, per AMD's specifications. Past the server, traffic crosses HPE's Ethernet. Both systems are liquid cooled.

The chips also differ on double precision, the 64-bit arithmetic that much of classical simulation relies on. AMD lists 78.6 teraflops of FP64 per MI355X, and NVIDIA lists 100 teraflops of FP64 for an entire 72-GPU GB300 NVL72 rack. For the AI training and inference both companies describe, lower-precision formats do most of the work.

Operations are split differently as well. Sferical runs its own machine, according to its release and Dagens industri. Scaleway has handed integration and ongoing hardware maintenance to Bull, which is also building LUMI-AI in Finland on AMD's MI430X GPUs for EuroHPC and won the EuroHPC contract for Sweden's new AI Factory supercomputer in Linköping.

## What "sovereign" means in each model

Sferical's release explains its model in terms of scale and control. Pooling investment, computing capacity, and expertise across industries, it says, spares each company from building the same infrastructure on its own and gives the members both the scale demanding AI work needs and "greater control over critical infrastructure." When the Swedish business outlet Realtid profiled the company in January 2026, it reported that Sweden already had supercomputers for academic research but no comparable infrastructure for industrial work with sensitive, business-critical data.

Scaleway's version starts with jurisdiction. When Tele2 announced the partnership in May, it described Scaleway's platform as "designed, built and operated entirely in Europe to ensure immunity from extraterritorial laws." Datacenter Forum, reporting Scaleway's Copenhagen plans, says all of Scaleway's GPU Zone sites are in the European Union, in line with its commitment to keep its operations under European jurisdiction.

The second element is choice. Scaleway's expansion announcement defines technological autonomy as "the ability to choose, develop and operate technologies from Europe." It backs that with AMD in Stockholm and NVIDIA in Copenhagen, and with a European inference offering based on VSORA's Jotunn8 processor, which Scaleway is helping VSORA and ZML develop. Reuters, in its report, set AMD's open-source ROCm against NVIDIA's proprietary CUDA, and SCN has reported that getting AMD's inference optimizations to work well together remains an engineering gap of its own. Brazil has turned the same principle into a procurement rule, writing anti-lock-in terms into its supercomputer tender.

In SCN's reading, the two models put the point of control in different places. For Sferical, sovereignty means ownership: a drugmaker, a telecom equipment maker, a defense company, and a bank share one machine without renting from anyone, and the open-weight model choice carries that control up into the software. For Scaleway, sovereignty means the legal system that governs the operator and the buyer's freedom to pick hardware, and the product is access, sold to any European company that wants it. Model weights have become a sovereignty question elsewhere too; Korea's national model program enforced a provenance standard on them.

Both systems run on US-designed processors. Of the announcements SCN read, only Scaleway's raises chip origin, in its work with VSORA, which it says aims to support new European chip architectures. Other national programs have weighed the silicon directly; Malaysia, for one, reportedly evaluated Huawei accelerators for a sovereign AI project.

## Four ways onto the hardware

Sweden's public systems sit in the same city as Sferical's. Arrhenius, which NAISS (the National Academic Infrastructure for Supercomputing in Sweden) installed with the EuroHPC Joint Undertaking and HPE delivered, entered production in May 2026 at NAISS in Linköping. Its GPU partition has 382 nodes, each with four NVIDIA Grace Hopper superchips, and scored 66.8 petaflops on the HPL benchmark, 42nd on the June 2026 TOP500 list. The system also has a partition dedicated to sensitive data.

Berzelius, the AI cluster at Linköping's National Supercomputer Centre, serves Swedish academic research groups and has a Wallenberg connection of its own. The Knut and Alice Wallenberg Foundation donated it in 2020 and paid for its expansions in 2023 and 2025. That foundation is one of the largest Wallenberg foundations, whose assets Wallenberg Investments manages, according to the 2025 consortium announcement.

The publicly funded route for companies is Sweden AI Factory, known as Mimer until August 2026, which NAISS hosts with the research institute RISE as part of EuroHPC's AI Factories network. It has partnered with more than 200 organizations since April 2025, and its dedicated supercomputer, the machine Bull is supplying, is due to be installed in Linköping later in 2026.

In SCN's reading, Sweden now has four ways onto large-scale AI hardware: academic allocation on Arrhenius and Berzelius; a publicly funded service for companies and startups through Sweden AI Factory; membership in Sferical, which added SKF as a strategic partner in April 2026; and, once Scaleway's system is built, capacity rented from a European operator.

Scaleway's next scheduled step in Sweden is three cloud Availability Zones in Stockholm, separate from the GPU Zone, which it plans to open in 2027.
