---
id: collect-261121-veille-ia/nvidia-hardware/nvidia-vera-rubin-release-date-specs-and-oracle-deal-pandromeda
title: "Nvidia Vera Rubin Release Date, Specs and Oracle Deal | Pandromeda"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: []
keywords: ["nvidia", "vera", "rubin", "oracle", "blackwell", "cloud", "infrastructure", "multi-gigawatt"]
source: https://pandromeda.com/hardware/nvidia-vera-rubin-release-specs-and-the-oracle-deal
source_anchor: ""
source_lines: [1, 155]
section: "NVIDIA_Hardware"
---
# Nvidia Vera Rubin: Release, Specs and the Oracle Deal

On October 27, 2026, Nvidia and Oracle confirmed Vera Rubin is now generally available on Oracle Cloud Infrastructure, with plans to expand to multi-gigawatt scale.

Nvidia and Oracle confirmed on October 27, 2026 that Nvidia Vera Rubin — Nvidia's next-generation AI computing platform — is now generally available on Oracle Cloud Infrastructure (OCI), and that OCI's Vera Rubin NVL72 superclusters will expand to "multi-gigawatt scale." If you came here looking for a new GeForce gaming card, this isn't it: Vera Rubin is a datacenter and enterprise AI platform, built for training and running large AI models in the cloud, not for playing games on your desktop.

**Key facts**

- **What happened:** On October 27, 2026, Nvidia and Oracle jointly announced that Vera Rubin NVL72 systems and Vera CPUs are now generally available on Oracle Cloud Infrastructure, with plans to scale OCI's Vera Rubin superclusters to multi-gigawatt levels.
- **What Vera Rubin is:** Nvidia's successor platform to Grace Blackwell (including Blackwell Ultra/GB300), pairing a new "Vera" CPU with new "Rubin" GPUs.
- **Who it's for:** Cloud providers, AI labs and enterprises running large-scale AI training, inference and "agentic" AI workloads — not consumer gamers.
- **Confirmed performance detail:** Nvidia has disclosed that Rubin GPUs deliver 50 sparse petaflops of FP4 compute, versus 20 sparse petaflops on Blackwell.
- **Named customer:** OpenAI, which has early access to Vera CPUs and already trains and serves models on an OCI supercluster using more than 100,000 Nvidia Blackwell GPUs.

## What Nvidia and Oracle Just Confirmed (October 27, 2026)

The news is a joint announcement from Nvidia's newsroom, mirrored on Oracle's own news page, titled "NVIDIA and Oracle Expand NVIDIA Vera Rubin to Multi-Gigawatt Scale and Bring NVIDIA Vera CPU to Oracle Cloud Infrastructure." Two things are now officially confirmed rather than rumored:

- **General availability.** Nvidia Vera Rubin NVL72 rack-scale systems and standalone Nvidia Vera CPUs are now available on Oracle Cloud Infrastructure.
- **A multi-gigawatt expansion plan.** Nvidia and Oracle say OCI's "Superclusters" — which already run Grace Blackwell GB200 and GB300 NVL72 systems — will scale with Vera Rubin NVL72 hardware to multi-gigawatt levels of compute capacity, to meet what the companies describe as growing demand for AI training, inference and agentic applications.

The release describes these OCI Superclusters as combining Nvidia NVLink-connected NVL72 racks (72 GPUs linked per rack), Oracle's own Acceleron networking, Nvidia Spectrum-X Ethernet built on Spectrum-6 switches and BlueField-4 DPUs, and "gigawatt-class" power and cooling infrastructure. Nvidia says OCI's multiplanar network architecture is designed to scale to hundreds of thousands of GPUs. The two companies frame this as a continuation of the existing GB200/GB300 buildout rather than a brand-new cluster design — Vera Rubin effectively slots into infrastructure Oracle was already building for Blackwell.

## Vera Rubin Is a Datacenter Platform, Not a GeForce Card

It's worth stating plainly, because the name sounds like it could be a graphics card: Vera Rubin has nothing to do with Nvidia's consumer GeForce lineup. If you're shopping for a gaming GPU, Nvidia's current consumer flagships are covered separately — see Pandromeda's RTX 5080 vs RTX 4090 comparison for that side of Nvidia's business. Vera Rubin, by contrast, is built for hyperscale cloud datacenters: it's the hardware that trains and runs large language models and "agentic" AI systems for companies like OpenAI, not something you'll install in a desktop PC.

## What Is Nvidia Vera Rubin, Exactly?

Vera Rubin is Nvidia's codename for its next AI compute platform after Grace Blackwell (the Blackwell and Blackwell Ultra/GB300 generation). Rather than a single chip, it's a combination of two new, purpose-built processors, as documented on Wikipedia's Rubin (microarchitecture) page, which draws on Nvidia's own disclosures:

- **Vera** — Nvidia's next-generation Arm-based CPU, designed to work alongside Rubin GPUs and to take over CPU-heavy work (like running agentic AI software) so the GPUs stay focused on AI computation.
- **Rubin** — Nvidia's next-generation GPU, succeeding Blackwell as Nvidia's flagship AI accelerator.

The platform is named after astronomer Vera Rubin, continuing Nvidia's tradition of naming GPU architectures after scientists (Ada Lovelace, Hopper, Blackwell). Rubin's successors are reportedly already planned: an enhanced "Rubin Ultra" and a further-out architecture codenamed "Feynman."

## Vera CPU and Rubin GPU: What's Officially Known

Nvidia has not published a single consolidated spec sheet for Vera Rubin the way it does for a consumer GPU launch, and much of what's circulating online — exact HBM4 capacity, memory bandwidth figures, transistor counts and per-GPU power draw — comes from industry analysts and trade press rather than Nvidia itself. Based only on what Nvidia has disclosed (via its own keynotes and newsroom posts, as reflected in Wikipedia's sourcing), here's what's confirmed:

| Attribute | Confirmed detail | Source | 
|---|---|---|
| Platform components | Vera CPU + Rubin GPU, successor to Grace + Blackwell | Nvidia / Wikipedia | 
| Manufacturing process | TSMC, 3-nanometer-class node | Wikipedia (Nvidia-sourced) | 
| GPU memory type | HBM4 | Wikipedia (Nvidia-sourced) | 
| Rubin GPU FP4 compute | 50 sparse petaflops (vs. 20 on Blackwell) | Nvidia GTC disclosure, via Wikipedia | 
| Rack-scale system | Vera Rubin NVL72 — 72 GPUs linked per rack via NVLink | Nvidia Newsroom, Oct. 27, 2026 | 
| Planned x86 option | HGX Rubin NVL8 — 8 Rubin GPUs, two 64-core Intel Xeon 6 CPUs, 4TB DDR5, 6.4 Tbps RDMA networking | Nvidia Newsroom, Oct. 27, 2026 | 
| Full production | Nvidia says Vera Rubin entered full production in May 2026 | Nvidia Newsroom, via Wikipedia | 
| Deployment | Nvidia says Vera Rubin entered deployment in September 2026 | Nvidia Blog, via Wikipedia | 

Note the HGX Rubin NVL8 system described in the October 27 release is explicitly a planned addition to OCI, intended for training, fine-tuning and inference workloads that benefit from x86 compatibility — Nvidia and Oracle have not given it a specific ship date.

## Nvidia Vera Rubin Release Date and Timeline

For readers searching "Vera Rubin Nvidia release date," there isn't one single launch day — like Blackwell before it, Vera Rubin has rolled out in stages over roughly two and a half years:

| When | Milestone | 
|---|---|
| Computex 2024 | Nvidia CEO Jensen Huang first announces the Rubin platform as Blackwell's successor, as later recapped in Nvidia's own post, "NVIDIA Kicks Off the Next Generation of AI With Rubin." | 
| GTC 2025 | Nvidia says the follow-up "Rubin Ultra" chip is expected in 2027, with roughly double Rubin's FP4 throughput. | 
| May 2026 | Nvidia says Vera Rubin enters full production. | 
| September 2026 | Nvidia says Vera Rubin enters deployment with cloud partners. | 
| October 27, 2026 | Nvidia and Oracle confirm general availability on Oracle Cloud Infrastructure and a multi-gigawatt expansion plan — the news covered in this article. | 

In short: Vera Rubin isn't a future product anymore. As of this announcement, it's shipping hardware that a named hyperscaler (Oracle) and a named customer (OpenAI) are actually running.

## Nvidia Vera Rubin vs. GB300 (Blackwell Ultra): What Changes

Readers searching "Vera Rubin Nvidia vs GB300" are really asking how the new platform relates to Nvidia's current-generation Blackwell Ultra systems (branded GB300), which already run on Oracle Cloud Infrastructure and other hyperscalers today. Here's what's confirmed, generation to generation:

|  | Blackwell / Blackwell Ultra (GB200/GB300) | Vera Rubin | 
|---|---|---|
| CPU | Grace | Vera | 
| GPU | Blackwell / Blackwell Ultra | Rubin | 
| GPUs per NVL72 rack | 72 | 72 | 
| Peak sparse FP4 compute (per Nvidia disclosure) | 20 petaflops | 50 petaflops | 
| GPU memory | HBM3e-generation | HBM4 | 
| OCI status (as of Oct. 27, 2026) | Already deployed at scale | Now generally available; expanding to multi-gigawatt scale | 

Nvidia's and Oracle's framing is explicitly continuity, not replacement: the October 27 release describes Vera Rubin as building on the existing GB200/GB300 footprint, and says Nvidia's CUDA software stack is designed to run the same AI models and tooling across both generations. For context on Nvidia's broader business calendar, the company's next quarterly results — where executives are likely to field investor questions about Vera Rubin's ramp — are covered in Pandromeda's report on the Nvidia Q3 FY2027 earnings date.

## Inside the Oracle Deal: Multi-Gigawatt Superclusters and Vera CPUs

Two distinct pieces make up the Oracle announcement. The first is scale: Oracle Cloud Infrastructure Superclusters, which already combine thousands of GPUs with Nvidia's networking gear, are being expanded with Vera Rubin NVL72 racks to reach "multi-gigawatt" total capacity — a figure that describes aggregate datacenter power draw across many racks and sites, not the power consumption of a single chip or server.

The second is the Vera CPU specifically. Nvidia and Oracle pitch Vera as solving a problem that gets worse as AI shifts toward "agentic" workloads — AI systems that call external tools, execute code and retrieve data as part of answering a request. That kind of CPU-bound work can leave expensive GPUs sitting idle. Nvidia says Vera is built for high single-thread performance and high memory bandwidth to keep that work off the critical GPU path. On the infrastructure side, Oracle says its Kubernetes Engine lets customers scale CPU and GPU resources independently, and that Oracle Acceleron networking provides hardware-based isolation for running untrusted, AI-generated code safely.

Nvidia vice president of hyperscale and high-performance computing Ian Buck said in the release that "AI factories must be productive, durable and fungible," while Oracle Cloud Infrastructure president Mahesh Thiagarajan said the deal gives customers "the performance, security and scale to advance frontier AI."

## OpenAI and Early Customers

OpenAI is named as an early-access customer for Vera CPUs on OCI. The release points to an existing example of the kind of scale involved: OpenAI has trained and served its "GPT-6 Astra" model on an OCI Supercluster in Abilene, Texas, using more than 100,000 Nvidia Blackwell GPUs. Two OpenAI executives are quoted — Sreekanth Pothanis, an engineering leader for compute infrastructure, said "as our frontier models advance, we need infrastructure that can keep pace," while Uday Ruddarraju, OpenAI's chief technology officer of compute, credited Vera CPUs on OCI with delivering the performance needed to execute the kind of code-heavy, loaded workloads agentic AI systems require.

## What Nvidia Hasn't Confirmed Yet

A lot of specific numbers are circulating in tech-industry coverage of Vera Rubin — including claims of roughly 288GB of HBM4 memory per GPU, memory bandwidth around 22 terabytes per second, transistor counts near 336 billion, an 88-core Vera CPU, and per-GPU power draws above 2,000 watts. None of those figures appear in Nvidia's own newsroom posts, blog posts or the Wikipedia page cited in this article, which means they trace back to industry analysts and trade publications rather than Nvidia's own disclosures. Readers should treat them as reported estimates, not confirmed specifications, until Nvidia publishes them directly.

## Frequently Asked Questions

**Is Nvidia Vera Rubin a gaming graphics card?**

No. Vera Rubin is a datacenter AI computing platform combining a new Vera CPU with new Rubin GPUs. It has no connection to Nvidia's consumer GeForce RTX lineup.

**What is Nvidia Vera Rubin's release date?**

There's no single launch date. Nvidia says the platform entered full production in May 2026 and deployment in September 2026, and on October 27, 2026, Nvidia and Oracle confirmed it is now generally available on Oracle Cloud Infrastructure.

**What are Vera Rubin's confirmed specs?**

Nvidia has confirmed Rubin GPUs use HBM4 memory on a TSMC 3-nanometer-class process and deliver 50 sparse petaflops of FP4 compute (versus 20 on Blackwell), arranged 72 GPUs per Vera Rubin NVL72 rack. Many other circulating specs are unconfirmed third-party estimates.

**How does Vera Rubin compare to GB300 (Blackwell Ultra)?**

Vera Rubin is the direct successor to the Grace Blackwell/GB300 generation, swapping in a Vera CPU for Grace and a Rubin GPU for Blackwell, with Nvidia citing 2.5x the FP4 compute per GPU. Nvidia and Oracle describe it as extending, not replacing, existing GB200/GB300 infrastructure.

**Is Vera Rubin available to buy or rent now?**

As of October 27, 2026, Vera Rubin NVL72 systems and Vera CPUs are generally available on Oracle Cloud Infrastructure. A planned x86-based system called HGX Rubin NVL8 has been announced but has no confirmed availability date yet.

## What to Watch Next

Three things to watch from here: whether Nvidia publishes a formal spec sheet for Rubin GPUs and Vera CPUs that confirms the memory, bandwidth and power figures currently circulating only as industry estimates; whether other hyperscalers (beyond Oracle) announce their own Vera Rubin general-availability timelines; and how Nvidia's leadership discusses the Vera Rubin ramp on its next earnings call. Pandromeda will update this story as Nvidia and its cloud partners share more.

## Frequently asked questions

### Is Nvidia Vera Rubin a gaming graphics card?

No. Vera Rubin is a datacenter AI computing platform combining a new Vera CPU with new Rubin GPUs. It has no connection to Nvidia's consumer GeForce RTX lineup.

### What is Nvidia Vera Rubin's release date?

There's no single launch date. Nvidia says the platform entered full production in May 2026 and deployment in September 2026, and on October 27, 2026, Nvidia and Oracle confirmed it is now generally available on Oracle Cloud Infrastructure.

### What are Vera Rubin's confirmed specs?

Nvidia has confirmed Rubin GPUs use HBM4 memory on a TSMC 3-nanometer-class process and deliver 50 sparse petaflops of FP4 compute (versus 20 on Blackwell), arranged 72 GPUs per Vera Rubin NVL72 rack. Many other circulating specs are unconfirmed third-party estimates.

### How does Vera Rubin compare to GB300 (Blackwell Ultra)?

Vera Rubin is the direct successor to the Grace Blackwell/GB300 generation, swapping in a Vera CPU for Grace and a Rubin GPU for Blackwell, with Nvidia citing 2.5x the FP4 compute per GPU. Nvidia and Oracle describe it as extending, not replacing, existing GB200/GB300 infrastructure.

### Is Vera Rubin available to buy or rent now?

As of October 27, 2026, Vera Rubin NVL72 systems and Vera CPUs are generally available on Oracle Cloud Infrastructure. A planned x86-based system called HGX Rubin NVL8 has been announced but has no confirmed availability date yet.

## Sources

- NVIDIA Newsroom: NVIDIA and Oracle Expand NVIDIA Vera Rubin to Multi-Gigawatt Scale and Bring NVIDIA Vera CPU to Oracle Cloud Infrastructure (Oct. 27, 2026)nvidianews.nvidia.com
- Oracle News: AI World — NVIDIA and Oracle Expand NVIDIA Vera Rubin to Multi-Gigawatt Scaleoracle.com
- NVIDIA Newsroom: NVIDIA Vera Rubin Ramps Into Full Productionnvidianews.nvidia.com
- NVIDIA Newsroom: NVIDIA Kicks Off the Next Generation of AI With Rubinnvidianews.nvidia.com
- Wikipedia: Rubin (microarchitecture)en.wikipedia.org

Mara Lindqvist edits the hardware desk. She covers graphics cards, processors, memory and storage, the foundries and chip designers behind them, and what the numbers on a spec sheet mean for people choosing a PC. Specifications in her stories come from manufacturer spec pages and datasheets.
