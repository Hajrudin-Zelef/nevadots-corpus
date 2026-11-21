---
id: collect-261121-veille-ia/actu-business/sambanova-a-ai-stack-current
title: "SambaNova â AI Stack Current"
domain: actu-business
role: reference
task: article
actors: []
dates: ["2026-07-08"]
keywords: ["sambanova", "inference", "current", "model", "infrastructure", "stack", "hardware", "private"]
source: https://www.aistackcurrent.com/companies/sambanova/
source_anchor: ""
source_lines: [1, 80]
section: "Actu_Business"
---
**SambaNova** is a private AI infrastructure company that designs its own **Reconfigurable Dataflow Unit (RDU)** accelerators and sells a vertically integrated inference stack spanning rack-scale hardware, serving software, hosted APIs, dedicated enterprise systems, and managed infrastructure for data-center operators. Its current strategy is centered on inference rather than training: use purpose-built dataflow hardware to serve large and agent-oriented models with low latency and high throughput, while supporting deployment in public, private, on-premises, and sovereign environments.

SambaNova was founded in **2017** by **Rodrigo Liang**, **Kunle Olukotun**, and **Christopher RÃ©**. Liang is co-founder and Chief Executive Officer; Olukotun is co-founder and Chief Technologist. The companyâs current board page identifies **Lip-Bu Tan**, who is also CEO of Intel, as SambaNovaâs chairman.

**Corporate boundary:** SambaNova remains a private company. Intel Capital is an investor, Intel is a strategic technology partner, and Intelâs CEO chairs SambaNovaâs board, but those facts do not establish that Intel owns or controls SambaNova. SoftBank, GV, BlackRock, General Atlantic, Vista and other investors or customers likewise should not be treated as owners beyond the interests actually disclosed. SambaNova does not publish a complete current capitalization or voting-right structure.


## SambaNova now spans chips, systems, software, and hosted inference

The company should not be reduced to an AI-chip vendor. Its commercial surface is a stack in which the RDU is only the execution layer.

| Layer | Current SambaNova surface | Role | Important boundary | 
|---|---|---|---|
| Accelerator | **SN40 / SN50 RDU** | Purpose-built dataflow processors for AI inference | Hardware capability does not establish model quality or application reliability | 
| Rack system | **SambaRack** | Integrated RDU systems for data centers | Rated hardware capability is not the same as delivered customer capacity | 
| Dedicated platform | **SambaStack** | Hardware and software for private or dedicated inference | Customer deployment and operations remain separate from SambaNova model ownership | 
| Hosted service | **SambaCloud** | Developer and enterprise model-serving APIs | Many served models are developed by third parties | 
| Managed infrastructure | **SambaManaged** | Managed inference service deployed inside operator or customer infrastructure | Managed operation does not transfer ownership of the customerâs data center | 
| Control plane | **SambaOrchestrator** | Model deployment, scaling, load balancing, monitoring, and server management | Orchestration is distinct from the model and from application authorization | 
| Developer API | OpenAI-compatible Chat Completions and **Responses API** | Application and agent integration | API compatibility does not imply identical provider behavior or feature support | 

This structure lets SambaNova sell into several markets at once. A developer can call hosted models through SambaCloud; an enterprise can deploy SambaStack on premises; a data-center operator can use SambaManaged to offer an inference service; and a sovereign provider can keep infrastructure inside its own jurisdiction.

For the engineering layer beneath these products, see LLM inference serving and capacity engineering. Model serving performance depends on workload shape, prefill and decode behavior, KV-cache pressure, batching, topology, quantization, compiler state, and service-level objectives rather than accelerator peak numbers alone.

## SN50 is the fifth-generation RDU, but availability claims need a maturity boundary

SambaNova introduced **SN50** on **February 24, 2026** as its fifth-generation RDU and the successor to SN40. The company positions it for large-scale and agentic inference, where repeated model calls make decode latency, memory movement, cache behavior, and concurrency economically important.

SambaNovaâs current hardware documentation describes SN50 as a 5 nm TSMC design with a three-tier memory hierarchy spanning on-chip SRAM, HBM2E, and DDR5. A SambaRack SN50 combines 16 chips and can scale across multiple racks, with the architecture designed to keep model data close to compute and reduce repeated movement through external memory.

The companyâs published performance and cost comparisons should be read as **SambaNova-reported or benchmark-specific results**, not as universal workload rankings. Results depend on the exact model, precision, prompt and output lengths, concurrency, batching, serving stack, hardware count, and whether prefill and decode run on the same or different systems.

Availability also needs to be stated precisely. At launch, SambaNova said SN50 would begin shipping in the **second half of 2026**. By July, the company was publicly demonstrating SN50 and said JPMorganChase had selected deployments of both SN40 and SN50. Its current product pages market SambaRack SN50 as the active fifth-generation platform. That establishes a real product and customer deployment path, but public material does not provide a complete shipment count or enough evidence to equate announced deployments with broad installed-base volume.

## The Intel relationship is a heterogeneous-inference design, not an acquisition

SambaNova and Intel moved from a planned collaboration in February to a **signed agreement announced April 8, 2026**. The joint blueprint separates inference work across different processors:

**GPU prefill â SambaNova RDU decode â Intel Xeon 6 host and action CPU**

The design reflects an important systems distinction. Prefill and decode have different compute and memory behavior, while an agent can also require CPU work for orchestration, tool execution, networking, and application logic. Intel and SambaNova said the jointly engineered solution was expected to become available to enterprise, cloud, and sovereign deployments in the second half of 2026.

The agreement does not make SN50 an Intel accelerator, and it does not make SambaNova an Intel subsidiary. Intel supplies CPU, networking, storage, GPU and ecosystem components around the design; SambaNova supplies its RDU systems and inference stack. Intel Capitalâs investment is another relationship layer and should not be conflated with product ownership or corporate control.

## The developer surface is increasingly designed for agents

SambaNova exposes inference through OpenAI-compatible client interfaces. Its documentation supports Chat Completions and a `POST /v1/responses` endpoint compatible with the OpenAI Responses API shape. The Responses endpoint supports the structured interaction patterns needed by coding and tool-using agents, including tool calls, tool results and streaming workflows.

That compatibility is useful but not absolute. SambaNova documents provider-specific feature differences and unsupported OpenAI parameters. Applications still need compatibility tests for the exact model, endpoint, tool schema, streaming behavior, structured output, retry policy, rate limits, and error handling they depend on.

There is also an important model-ownership boundary. SambaCloud serves models from developers including **MiniMax, DeepSeek, Meta, OpenAI, Alibaba and others**. Running those checkpoints on SambaNova infrastructure does not make them SambaNova-developed models. SambaNovaâs differentiated asset is primarily the compute, serving, software, and deployment stack around the model.

## Enterprise and sovereign deployments show a broader route to market

On **July 8, 2026**, SambaNova said **JPMorganChase** had selected the company as an inference infrastructure partner and was deploying SN40 and SN50 systems for secure on-premises AI inference. This is stronger evidence than a generic partnership announcement because it identifies an infrastructure role and deployment target. It still does not disclose fleet size, spend, utilization, or production workload volume.

**SoftBank Corp.** was named in February as the first customer expected to deploy SN50 in next-generation AI data centers in Japan. SambaNova said the systems would support low-latency domestic inference services and extend an existing relationship in which SoftBank hosts SambaCloud capacity in the region. The announcement establishes a customer and deployment commitment; it should not be converted into a completed-fleet number unless later evidence confirms delivery and operation.

SambaNova also uses infrastructure partners to pursue sovereign inference. Its current materials identify deployments or provider relationships with **SouthernCrossAI in Australia, Argyll in the United Kingdom, Infercom in Europe, and OVHcloud in Europe**. These arrangements emphasize domestic infrastructure, data residency, and access to open-weight models. Their commercial maturity differs by provider, so a named sovereign relationship should not automatically be read as a fully utilized national-scale production cloud.

In July 2026 SambaNova also joined the U.S. Department of Energyâs **Genesis Mission Consortium**, contributing RDU hardware, software, and inference expertise. Consortium membership is evidence of government-facing participation, not by itself a procurement award or proof that SambaNova systems operate every Genesis workload.

## Security posture is tied to where inference runs

SambaNova markets private, on-premises, sovereign, and air-gapped deployment modes for customers that need stronger control over data location and infrastructure. Its public-sector materials state that the company holds **SOC 2 Type 2** and **ISO/IEC 27001:2022** certifications.

Those controls matter, but they should not be generalized into a claim that every deployment or model output is secure. Certification scope, customer network design, identity and access management, software supply chain, model provenance, logging, tenant isolation, endpoint exposure, and operational procedures remain separate parts of the security case. A private deployment can reduce external data exposure while still being misconfigured or granted excessive application authority.

## Financing has accelerated alongside the inference push

SambaNova raised more than **$350 million in Series E financing in February 2026**, alongside the SN50 and Intel announcements. The company said the proceeds would expand SN50 production, SambaCloud capacity, and enterprise software integration.

On **July 8, 2026**, SambaNova announced the **first close of $1 billion in Series F financing at an $11 billion post-money valuation**. General Atlantic led the round, with significant investment from Seligman Ventures and T. Rowe Price Associates and participation from other new and existing investors.

The wording matters. This was a **first close** of the Series F, not necessarily the final size of the round. The $11 billion figure is a company-announced financing valuation, not a public-market capitalization. Investor participation also does not reveal exact voting control without a complete cap table and governance rights.

SambaNovaâs current position is therefore broader than âan NVIDIA alternative.â It is building an inference company around proprietary dataflow silicon, rack systems, model-serving software, OpenAI-compatible APIs, private and managed deployment, and sovereign infrastructure. The durable reader boundary is to distinguish **SambaNova-owned compute and software from third-party model ownership; benchmark performance from workload-specific production economics; announced deployments from installed fleet size; customer and investor relationships from corporate control; and security certifications from the complete security posture of a deployed AI system**.

Evidence
