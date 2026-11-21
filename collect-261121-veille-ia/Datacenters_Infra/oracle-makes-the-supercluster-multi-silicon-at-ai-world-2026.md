---
id: collect-261121-veille-ia/datacenters-infra/oracle-makes-the-supercluster-multi-silicon-at-ai-world-2026
title: "Oracle Makes the Supercluster Multi-Silicon at AI World 2026"
domain: datacenters-infra
role: reference
task: article
actors: []
dates: ["2026-10-27"]
keywords: ["oracle", "cloud", "inference", "silicon", "world", "google", "nvidia", "first"]
source: https://futurumgroup.com/insights/oracle-makes-the-supercluster-multi-silicon-at-ai-world-2026/
source_anchor: ""
source_lines: [1, 63]
section: "Datacenters_Infra"
---
Oracle used AI World 2026 to sign compute deals across five silicon suppliers in one day, expanding NVIDIA Vera Rubin to multi-gigawatt scale while becoming the first hyperscaler outside Google to offer TPUs and adding Cerebras, SambaNova, and Intel Diamond Rapids capacity. Futurum assesses what a five-architecture OCI means for the design of AI Superclusters.

## What Is Covered in This Article:

- NVIDIA Vera Rubin NVL72 general availability on OCI and a planned multi-gigawatt expansion
- Google TPU 8t and 8i on OCI in calendar 2027, the first hyperscale cloud outside Google to offer TPUs
- Cerebras wafer-scale inference and SambaNova SN50 decode capacity coming to OCI
- A multiyear Intel collaboration putting Diamond Rapids in planned OCI X14 shapes
- Oracle Acceleron networking replacing some NVIDIA networking silicon in OCI systems

**The News:** Oracle announced compute agreements at AI World in Las Vegas on October 27, 2026 that place five external silicon architectures inside Oracle Cloud Infrastructure. NVIDIA Vera Rubin NVL72 racks and Vera CPUs reached general availability on OCI, and the two companies plan to expand OCI Superclusters to multi-gigawatt scale. Google’s eighth-generation TPUs, the 8t for training and the 8i for inference, will come to OCI in calendar 2027, the first TPU offering on a hyperscale cloud outside Google per Oracle. Cerebras plans to bring wafer-scale inference to OCI in calendar 2027, and SambaNova will deploy its fifth-generation SN50 Reconfigurable Dataflow Units for decode-stage inference. Intel and Oracle announced a multiyear Xeon collaboration, including planned OCI X14 offerings built on Diamond Rapids. Below the silicon, Oracle’s Acceleron networking will link TPU clusters, NVIDIA systems, and the rest of the OCI data center.

“Bringing Google TPUs to OCI gives customers access to multiple leading AI compute architectures in one cloud,” said Mahesh Thiagarajan, President of Oracle Cloud Infrastructure. The companies disclosed no deal values.

## Oracle Makes the Supercluster Multi-Silicon at AI World 2026

**Analyst Take:** Oracle has turned OCI into the first hyperscale cloud designed around silicon plurality. The announcements convert the posture signaled by Oracle’s reported $300 billion OpenAI contract and 50,000-unit AMD MI450 commitment into an operating model in which Oracle owns the fabric and the data center shell while training, prefill, decode, and agent orchestration run on different vendors’ silicon. Futurum assesses that this is the clearest public blueprint yet for the multi-silicon AI cloud and that its economics turn on inference demand that no vendor has yet proven at this scale.

## Inference Demand Breaks the Single Vendor Procurement Model

The deal stack maps onto the disaggregation of inference into stages with different silicon requirements. SambaNova’s pitch states the disaggregation thesis plainly. GPUs run prefill while the SN50 runs decode, the stage that sets agent response speed, with vendor claims of up to 5x the speed of competitive chips and 3x lower total cost of ownership in a 20 kW air-cooled rack. Cerebras specializes in the same decode stage on a wafer-scale part that keeps compute and memory on one die. Futurum’s 1H 2026 Data Center Semiconductors Market Sizing & Five-Year Forecast projects inference at 73% of data center AI accelerator demand in a market reaching $1,212.7 billion in CY2030, with agent and reasoning-first workloads at $546.0 billion. The implication is that a cloud selling tokens rather than GPU-hours can arbitrage architecture against workload. Oracle is the first hyperscaler to build that arbitrage into its public catalog.

## Acceleron Moves Oracle’s Value from Reselling Chips to Owning the Fabric

Acceleron is where Oracle’s margin argument lives. Its custom SmartNIC lets Google TPUs keep their native ICI and optical circuit switch interconnects inside the pod while joining OCI storage and databases outside it, and making Oracle’s own fabric the common layer beneath every vendor’s silicon is the sharpest competitive signal of the day. NVIDIA Spectrum-X Ethernet, with Spectrum-6 switches and BlueField-4 DPUs, runs alongside Acceleron, and the multi-gigawatt Vera Rubin expansion keeps NVIDIA the anchor tenant of the buildout. AWS, Microsoft, and Google each pair merchant accelerators with first-party accelerators, while Oracle’s first-party silicon is the network. The open question is whether fabric ownership captures enough value to justify buying everyone else’s compute at market prices.

## Agentic Workloads Return the CPU to the AI Factory Floor

Vera CPUs reached general availability with OpenAI bringing them into production for tool calls and code execution, and the planned Rubin HGX NVL8 pairs eight Rubin GPUs with two 64-core Intel Xeon 6 processors for x86-compatible AI serving. Intel’s multiyear agreement reads as roadmap and capacity coordination for this layer, with Diamond Rapids positioned for embedding generation, reranking, and the application services agents invoke. Futurum’s forecast places Intel at 45.3% of data center CPU value share in Q4 CY2025, with AMD at 29.7% and the Arm cohort at 25.0%, and agentic serving is where those shares will be contested. Agents spend most of their wall-clock time in tool execution, which returns the CPU to the center of AI infrastructure, and Oracle now sources CPUs from four suppliers at once: NVIDIA’s Vera, Intel’s Diamond Rapids, the AMD EPYC shapes OCI already runs, and Ampere’s Arm-based line.

## Financing and Software Fragmentation Are the Costs of Plurality

The bear case is operational. Oracle has not said how it will finance the capacity, and NVIDIA CEO Jensen Huang has said Vera Rubin systems can generate up to $40 billion in revenue per gigawatt for NVIDIA, which prices the multi-gigawatt commitment in the tens of billions. Five silicon platforms mean separate demand forecasts and software stacks, and stranded capacity if agentic demand grows slower than the 73% inference share implies. CUDA, XLA, Cerebras’ SDK, and SambaNova’s dataflow stack do not share a developer, and making heterogeneity invisible falls on OCI’s scheduling and Kubernetes layers. The reported addition of OpenAI’s Broadcom-designed Jalapeño inference processor to OCI would add a sixth architecture. Oracle’s investor day this week should show whether revenue commitments attach to this capacity as they did with the OpenAI contract, because backlogs are not revenue.

## What to Watch:

- Whether Oracle discloses TPU, Cerebras, or SambaNova capacity commitments at its investor day this week
- Whether Acceleron becomes the default fabric across the multi-gigawatt Vera Rubin expansion
- Whether Cerebras and SambaNova publish OCI-validated tokens-per-second and cost benchmarks in 2027
- Whether OCI X14 Diamond Rapids shapes reach general availability with AMX FP8 support in 2027
- Whether Oracle RPO growth in fiscal Q2 2027 shows customers prepaying for non-NVIDIA capacity

The complete information is available on Oracle’s website.

### Sources

###### *Declaration of generative AI and AI-assisted technologies in the writing process: This content has been generated with the support of artificial intelligence technologies. Due to the fast pace of content creation and the continuous evolution of data and information, The Futurum Group and its analysts strive to ensure the accuracy and factual integrity of the information presented. However, the opinions and interpretations expressed in this content reflect those of the individual author/analyst. The Futurum Group makes no guarantees regarding the completeness, accuracy, or reliability of any information contained herein. Readers are encouraged to verify facts independently and consult relevant sources for further clarification.*

###### *Disclosure: Futurum is a research and advisory firm that engages or has engaged in research, analysis, and advisory services with many technology companies, including those mentioned in this article. The author does not hold any equity positions with any company mentioned in this article.*

###### *Analysis and opinions expressed herein are specific to the analyst individually and data and other information that might have been provided for validation, not those of Futurum as a whole.*

###### *Read the full Futurum Group Disclosure.*

### Other Insights From Futurum:

### Author Information

Brendan is Research Director, Semiconductors, Supply Chain, and Emerging Tech. He advises clients on strategic initiatives and leads the Futurum Semiconductors Practice. He is an experienced tech industry analyst who has guided tech leaders in identifying market opportunities spanning edge processors, generative AI applications, and hyperscale data centers.

Before joining Futurum, Brendan consulted with global AI leaders and served as a Senior Analyst in Emerging Technology Research at PitchBook. At PitchBook, he developed market intelligence tools for AI, highlighted by one of the industry’s most comprehensive AI semiconductor market landscapes encompassing both public and private companies. He has advised Fortune 100 tech giants, growth-stage innovators, global investors, and leading market research firms. Before PitchBook, he led research teams in tech investment banking and market research.

Brendan is based in Seattle, Washington. He has a Bachelor of Arts Degree from Amherst College.
