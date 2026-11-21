---
id: collect-261121-veille-ia/hardware-semi/sferical-one-goes-live-swedish-industry-coalition-shares-1-152-blackwell-ultra-gpus
title: "Sferical One Goes Live: Swedish Industry Coalition Shares 1,152 Blackwell Ultra GPUs"
domain: hardware-semi
role: reference
task: article
actors: []
dates: ["2026-10-15"]
keywords: ["sferical", "gpus", "nvl72", "nvidia", "memory", "blackwell", "ultra", "single"]
source: https://www.techtimes.com:443/articles/329133/20261016/sferical-one-goes-live-swedish-industry-coalition-shares-1152-blackwell-ultra-gpus.htm
source_anchor: ""
source_lines: [1, 103]
section: "Hardware_Semi"
---
Sweden's most powerful industrial companies solved a problem none could solve alone: how to run frontier-scale AI on sensitive proprietary data without routing it through a US-domiciled cloud.

The answer — a jointly owned supercomputer where AstraZeneca, Ericsson, Saab, SEB, and Wallenberg Investments share costs and compute that each company could not economically justify building privately — formally launched in Stockholm on October 15, 2026, with Jensen Huang attending the ceremony alongside Ericsson CEO Per Narvinger and Wallenberg Investments Chairman Marcus Wallenberg.

The machine is called Sferical One. It has been running since late summer 2026. And the model that built it may be the most replicable template yet for how European industry closes the sovereign AI compute gap.

### Sweden's Shared Supercomputer: What the Machine Actually Contains

Sferical One is built on two NVIDIA DGX SuperPODs, spanning 1,152 Blackwell Ultra GPUs organized across 16 NVIDIA DGX GB300 NVL72 systems. Each of those 16 racks is a DGX GB300 NVL72 — NVIDIA's latest rack-scale AI system, pairing 72 Blackwell Ultra GPUs with 36 Grace ARM CPUs in a single fully liquid-cooled enclosure, as detailed on the official GB300 NVL72 product page.

The aggregate numbers are substantial. Across all 16 NVL72 units, Sferical One delivers approximately 320 terabytes of HBM3E GPU memory, 9,216 terabytes per second of aggregate GPU memory bandwidth, and roughly 23,040 petaFLOPS of FP4 dense AI compute — approximately 23 exaFLOPS.

The system is already operational. Having come online in late summer 2026, Sferical One had accumulated several months of real-world workloads before its formal public launch — meaning the performance figures above reflect production, not a preview configuration.

### How 1,152 GPUs Actually Work Together

The 1,152-GPU headline figure deserves a closer look, because Sferical One's internal architecture shapes what the system can actually do.

Those 1,152 GPUs are not connected in a single flat fabric. They are organized as 16 independent NVLink 5.0 domains per rack — one per NVL72 — each containing 72 GPUs in a fully unified memory pool. Within each domain, all 72 GPUs share 20 terabytes of HBM3E memory addressable as a single continuous address space, connected at 130 terabytes per second of aggregate NVLink bandwidth. For any AI workload that fits within a single NVL72's memory capacity, this means near-frictionless parallelism: no gradient synchronization across network links, no tensor-slicing overhead.

Cross-rack communication — for models too large to fit within one NVL72's 20 terabytes — runs over ConnectX-8 SuperNIC fabric interconnect at 800 gigabits per second per GPU, interconnecting the 16 domains via InfiniBand. This is NVIDIA's DGX SuperPOD interconnect architecture: NVLink for intra-rack memory coherence, InfiniBand for inter-rack tensor parallelism. The entire cluster runs NVIDIA Base Command for orchestration and NVIDIA AI Enterprise for managed AI workflows.

The practical implication: Sferical One is well-suited for large language models and multimodal models at the 100-to-300-billion parameter scale within a single NVL72 domain. Models approaching or exceeding one trillion parameters — the outer edge of current frontier development — require coordinated inter-rack parallelism and carry the latency costs of InfiniBand communication at scale. That architectural constraint is not unique to Sferical One; it applies to any DGX SuperPOD deployment. But it sets the realistic ceiling for what the system's partners can train or run without additional cross-site infrastructure.

### Who Owns It and Why

Sferical AI was jointly launched by five partners — AstraZeneca, Ericsson, Saab, SEB, and Wallenberg Investments — as a jointly owned venture, with formal incorporation occurring in August 2025. The founding partners and their stated use cases:

**AstraZeneca**, the Anglo-Swedish pharmaceutical giant, plans to use Sferical One to accelerate drug discovery, train biomedical foundation models, and run multimodal clinical inference workloads where patient data confidentiality rules out third-party cloud processing.

**Ericsson**, the telecommunications equipment maker, aims to apply the platform to next-generation network AI, RAN optimization, and digital twin simulation. Ericsson CTO Erik Ekudden described a vision of a "6G/AI intelligent fabric for securely connecting AI systems," noting that developing and operating advanced AI for "robust, secure networks" requires the kind of in-house, sovereign compute that Sferical provides.

**Saab**, Sweden's aerospace and defense conglomerate, intends to integrate advanced AI into defense systems — the most sovereignty-sensitive use case in the consortium, where data residency is not a compliance preference but an operational security requirement.

**SEB**, one of Scandinavia's largest banks, is targeting fraud detection, risk modeling, and financial services automation, sectors where European Banking Authority guidelines and ECB supervisory requirements restrict data movement to external cloud providers.

**Wallenberg Investments**, the investment arm of the Wallenberg family whose industrial holdings span much of Swedish corporate life, is both a financial backer and an anchor institution. Marcus Wallenberg said at launch that Sferical One represents "a long-term investment in the skills Sweden will need," calling AI "a critical factor in the future competitiveness of Swedish industry."

In April 2026, **SKF** — the Gothenburg-based bearings and industrial technology company, founded in 1907 — joined as strategic partner, citing the importance of protecting its proprietary intellectual property within sovereign infrastructure.

### Why a Professor Who Ran Sweden's National Supercomputing Infrastructure Is Now Running Sferical

The company is led by CEO **Jenny Nordlöw**, a former advisor to the Wallenberg family office who has emphasized cross-industry collaboration as the structural center of Sferical's value proposition. Nordlöw said at the launch that "the value lies in several large Swedish companies sharing infrastructure, developing solutions together, and sharing knowledge across industries."

Executive Chairman Anders Ynnerman brings the deepest infrastructure credentials of any executive in the Swedish AI landscape. A physicist by training — PhD from the University of Gothenburg, postdoctoral work at Oxford and Vanderbilt — Ynnerman spent years leading the Swedish National Infrastructure for Computing (SNIC) and the National Supercomputer Centre (NSC) at Linköping University, Sweden's national HPC infrastructure bodies, per his Saab board profile. He now serves as Director of Strategic Research at the Knut and Alice Wallenberg Foundation (KAW), which funds WASP — the Wallenberg AI, Autonomous Systems and Software Program — Sweden's largest single investment in AI research. He also sits on the board of Saab.

In effect, the person who spent years building the public supercomputing infrastructure that Sweden's universities depend on is now applying the same logic at industrial scale. The institutional knowledge transfer is not incidental — it is the design.

### Open Models and the Compliance Architecture

On top of the hardware, Sferical AI has chosen the NVIDIA Nemotron open-weights models as its foundation model family — a suite of open-weights multimodal AI models that includes the Nemotron 3 Nano Omni (a 30-billion-total-parameter hybrid Mixture of Experts model with approximately 3 billion active parameters) and the larger Nemotron 3 Ultra (550 billion total parameters) and Nemotron 3 Super (120 billion total parameters). The open-weights licensing is deliberate and directly relevant to each partner's compliance situation.

For AstraZeneca, open weights mean clinical trial data never leaves the building to reach a proprietary inference endpoint. For Saab, it means defense application models can be fine-tuned and audited entirely within controlled facilities. For SEB, it means the bank's fraud models are not inference-as-a-service calls to a provider whose data handling practices the ECB may later scrutinize. Model internals are transparent. Fine-tuning is unrestricted. Deployment is on-premises.

The company also operates **Sferical Labs**, a collaborative R&D environment where the partner organizations and Sferical's own team work jointly on AI solutions. Plans are underway for a dedicated **NVIDIA AI Technology Centre** in Sweden, which would extend the consortium's ecosystem to companies beyond the founding members.

### Why European Companies Cannot Simply Use AWS or Azure

The sovereign AI argument is sometimes dismissed as regulatory theater — a preference, not a necessity. For the sectors Sferical serves, it is a necessity.

The core legal constraint is GDPR Article 46 transfer rules: personal data transferred outside the EU or EEA to a third country requires adequate protection guarantees. US cloud providers operating EU-region data centers argue they meet this requirement. But the EU-US Data Privacy Framework and the shadow of the Schrems II line of rulings have left residual uncertainty: the US CLOUD Act data provisions allow US government demands for data held by US-headquartered companies regardless of where the data physically sits. For Saab, processing defense-related sensor data on a US-incorporated cloud — even in a Stockholm data center — creates a structural exposure that in-country sovereign infrastructure eliminates.

The same logic applies, but differently, to each partner. AstraZeneca's clinical trial data is subject to EMA regulatory confidentiality requirements. SEB's transaction-level data falls under EBA Guidelines on Outsourcing Arrangements and ECB supervisory expectations. Ericsson's network topology data for commercial customers is subject to client confidentiality and national critical infrastructure protections. For all of them, "EU-region cloud" is not an equivalent substitute for "infrastructure we own and operate under Swedish law."

Sferical One adds an economic argument to this analysis. Each of these companies, independently, could not build a 16-rack, 1,152-GPU compute cluster and justify the capital expenditure against its own AI pipeline alone. Shared ownership—where Sferical AI operates the cluster and each partner buys compute capacity at per-GPU-hour rates—turns what would be an unjustifiable capital investment into a shared infrastructure service. That economic logic is the actual innovation.

### The Nordic AI Build-Out Around It

Sferical One is arriving inside a broader regional acceleration. The Nordic AI computing ecosystem is projected to reach one gigawatt of AI capacity deployed across the region by end of 2027, with providers including Bitdeer, CoreWeave, Crusoe, Hyperstack, Nscale, Verda, and Volta among those building capacity in the region.

Sweden's venture ecosystem has been a particular bright spot: the country raised 2.8 billion in venture funding in the first half of 2026 alone, with full-year projections reaching $5 billion. Sweden's unicorn density sits at 4.5 unicorns per million inhabitants, the second-highest in the Nordics.

Sferical One occupies a complementary layer in this stack: enterprise-grade sovereign compute for established industrial players who need data residency guarantees and audit trails, rather than the flexible startup-focused GPU cloud that characterizes most of the regional build-out.

Sweden is also home to the Sweden AI Factory, one of 19 EuroHPC-funded AI factories across Europe established under the EU AI Continent Action Plan. While the Sweden AI Factory is publicly funded and oriented toward research and SME access, Sferical One is privately funded and oriented toward production workloads in regulated sectors. The two complement rather than compete.

### What the Consortium Model Gets Right — and What Comes Next

Jensen Huang, who appeared on stage at the Stockholm ceremony, offered a pointed observation. Huang said at the ceremony: "You have done the hardest part... you have started."

That framing captures both the achievement and the open question. Getting five major corporations to co-own a supercomputer, agree on governance, align on model choices, and bring it online before any of them could be certain the others would follow — that is genuinely hard. The machine exists and is running. That is the proof of concept.

The questions that come next are scaling questions. Each partner's AI ambitions will grow. AstraZeneca is a company that processes genomic datasets at a scale that makes 20 terabytes per NVL72 look constrained. Ericsson's digital twin aspirations for 6G networks involve near-real-time inference across distributed infrastructure. Whether 1,152 GPUs — shared across six partners — scales with those ambitions, or whether the consortium will need to add racks, add partners, or add sites, is a question Sferical's next year of operations will begin to answer.

What is already clear is the model itself. An industrial consortium, jointly owning shared compute, organized as an independent legal entity, running open-weights models fine-tuned for sector-specific workloads, on infrastructure they control — is a pattern that the automotive, energy, and manufacturing sectors across Europe will study carefully. If it works as claimed at Sferical, it works everywhere the same regulatory and economic constraints apply. In Europe, that is most places.

## Frequently Asked Questions

### What is Sferical One and how many GPUs does it have?

Sferical One is an AI supercomputer based in Linköping, Sweden, built and jointly operated by a consortium of six Swedish companies: AstraZeneca, Ericsson, Saab, SEB, Wallenberg Investments, and SKF. The system contains 1,152 NVIDIA Blackwell Ultra GPUs organized across 16 DGX GB300 NVL72 racks in two DGX SuperPODs. It has been operational since late summer 2026 and formally launched on October 15, 2026.

### Why can't these companies just use Amazon Web Services or Microsoft Azure for their AI workloads?

For the sectors Sferical serves — defense, pharmaceuticals, banking, and telecom — routing sensitive data through US-domiciled cloud providers creates a structural legal exposure that EU-region data center locations don't fully eliminate. The US CLOUD Act allows the US government to compel US-headquartered cloud companies to produce data regardless of where it's physically stored. For Saab's defense systems data, AstraZeneca's clinical trial datasets, and SEB's financial transaction records, that residual risk is not acceptable under their respective regulatory frameworks. Sovereign infrastructure — hardware and software entirely under Swedish jurisdiction — removes the ambiguity.

### What does "open weights" mean for Nemotron, and why does it matter to regulated-sector companies?

Open weights means the full trained parameters of the AI model are publicly released, allowing any user to run, inspect, audit, and fine-tune the model on their own hardware without depending on the original developer's servers. For Sferical's partners, this is operationally essential: AstraZeneca's biomedical models can be fine-tuned on patient data that never leaves the company's own infrastructure, Saab's defense applications can be audited by Swedish government security reviewers, and SEB's fraud models are not black-box API calls to a vendor the ECB may scrutinize. Open weights converts a cloud-dependent AI service into software the partner companies actually own and control.

### Could other European industries replicate the Sferical consortium model?

The model is directly replicable anywhere several large companies in the same regulated sector face the same triple constraint: GDPR data residency requirements, EU AI Act compliance obligations for high-risk AI systems, and insufficient scale to justify building private GPU clusters alone. Automotive OEMs, European energy companies, healthcare networks, and defense primes across Germany, France, the Netherlands, and the Nordics face versions of the same problem. Sferical One is the first working industrial-consortium implementation at GPU-cluster scale, which makes it a reference design for the category — not just a Swedish story.
