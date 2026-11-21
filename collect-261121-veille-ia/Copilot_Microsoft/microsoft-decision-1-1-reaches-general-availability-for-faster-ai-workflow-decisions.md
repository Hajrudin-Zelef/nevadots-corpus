---
id: collect-261121-veille-ia/copilot-microsoft/microsoft-decision-1-1-reaches-general-availability-for-faster-ai-workflow-decisions
title: "Microsoft Decision 1.1 Reaches General Availability for Faster AI Workflow Decisions"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["microsoft", "decision", "model", "workflow", "decisions", "should", "index", "disclosed"]
source: https://dev.to/alifar/microsoft-decision-11-reaches-general-availability-for-faster-ai-workflow-decisions-5gj7
source_anchor: ""
source_lines: [1, 63]
section: "Copilot_Microsoft"
---
Microsoft has made **Microsoft-Decision-1.1 generally available**, giving developers a decision-oriented AI model intended for use inside applications and automated workflows. The model is available through Microsoft Foundry and can also be accessed through OpenRouter and Vercel. Rather than positioning it as a general-purpose assistant, Microsoft is targeting tasks where software needs a fast, calibrated choice, such as routing, classification, prioritization, verification and workflow control.

The release matters because many AI-enabled processes do not need a long-form answer. They need a reliable next step: determine which queue should receive a request, whether a document meets a condition, which model should handle a task, or whether a workflow should continue. In its official general-availability announcement for Microsoft Decision 1.1, Microsoft says the model is designed to reduce latency and cost compared with using a general-purpose large language model for these decision tasks.

Decision 1.1 is a model for embedding decision capabilities directly into software. Microsoft describes a calibrated decision-making approach and support for **multimodal inputs, including images**. That combination broadens the kinds of workflow inputs the model can evaluate, although the appropriate use case still depends on the application and the decision being made.

The core deployment paths cited by Microsoft are significant for developers and teams building with multiple AI services:

- 
**Microsoft Foundry** provides a Microsoft deployment route for the model.
- 
**OpenRouter and Vercel access** make the model available through additional developer ecosystems.
- 
**Copilot and Foundry Evaluations integrations** point to ways the model can be used and assessed alongside other AI capabilities.
- 
**Token-based pricing with free output tokens** gives developers a disclosed pricing structure for model use.

Microsoft says thousands of customers adopted Decision 1.1 during its public preview. The company also says it tested the model across 102 benchmarks containing 385,000 questions. Those figures describe Microsoft's testing and adoption claims, not a guarantee of performance for every business process. A routing task in customer support, for example, can have very different data quality, error tolerance and business consequences from an internal benchmark.

Microsoft highlights a score of **68 on the Artificial Analysis Decision Index version 2.1**, stating that Decision 1.1 outranks Jev 1.14, Google Gemini Decision 1 and OpenAI GPT-6 Luna (Decisions) on that index. This is a useful signal for buyers evaluating decision-model options, but it should not be treated as a universal ranking for every AI workload.

A decision index measures a particular evaluation target. Businesses should still test models against their own inputs, edge cases and success criteria. A model that performs well in a broad benchmark may need different prompts, thresholds or review steps when used for financial checks, customer eligibility, content handling or operational triage.

| Model | Artificial Analysis Decision Index v2.1 result cited by Microsoft | Relationship stated in Microsoft's announcement | 
|---|---|---|
| Microsoft-Decision-1.1 | 68 | Microsoft's reported score | 
| Jev 1.14 | Not disclosed in the supplied research | Ranked below Decision 1.1, according to Microsoft | 
| Google Gemini Decision 1 | Not disclosed in the supplied research | Ranked below Decision 1.1, according to Microsoft | 
| OpenAI GPT-6 Luna (Decisions) | Not disclosed in the supplied research | Ranked below Decision 1.1, according to Microsoft | 

The comparison is specifically Microsoft’s performance framing around the AA Index. It does not establish relative quality for writing, coding, customer conversations or other general AI tasks. The stronger takeaway is that Microsoft is defining Decision 1.1 as a specialized option for a class of application decisions that are often currently handled by broader models.

For companies building AI-assisted operations, a specialized decision model can be useful as a control point in a larger system. A workflow may first receive text or an image, then use a decision model to classify the request, choose a route or check a condition before sending the work to another system or model. This can make the workflow more deliberate than sending every task to a single general-purpose model.

Potential applications supported by Microsoft's stated use cases include routing incoming work, classifying records, prioritizing tasks, verifying conditions and controlling workflow steps. The model's multimodal support could also matter where a workflow needs to consider images as well as text. The business value in these cases is not simply adding AI. It is reducing manual sorting and creating faster, more consistent paths for routine decisions.

Pricing is another relevant part of the release. Microsoft discloses token-based pricing and says output tokens are free. That may simplify cost planning for workloads that produce short decision outputs, but total cost will still depend on input volume, surrounding tools, engineering work and any human review required. Teams using non-Microsoft stacks should also assess the real-world cost of ownership across their chosen providers and integrations.

**Scalevise CTA:** Decision models are most useful when they are connected to real processes, clear rules and the systems where work already happens. Scalevise can help identify high-value routing, classification and verification tasks, then design AI workflow automation for practical business processes that reduces repetitive manual work without obscuring accountability. Start by discussing an AI automation project with Scalevise.

**What is Microsoft Decision 1.1?**

Microsoft Decision 1.1 is a generally available, decision-oriented AI model designed for application tasks such as routing, classification, prioritization, verification and workflow control.

**Where is Microsoft Decision 1.1 available?**

Microsoft says Decision 1.1 is available in Microsoft Foundry and can be accessed through OpenRouter and Vercel.

**Does Microsoft Decision 1.1 support images?**

Yes. Microsoft states that Decision 1.1 supports multimodal inputs, including images.

**How is Microsoft Decision 1.1 priced?**

Microsoft discloses token-based pricing for Decision 1.1 and says output tokens are free. The supplied research does not provide specific price figures.

**Did Microsoft Decision 1.1 outperform other decision models?**

Microsoft reports that Decision 1.1 scored 68 on the Artificial Analysis Decision Index version 2.1 and outranked Jev 1.14, Google Gemini Decision 1 and OpenAI GPT-6 Luna (Decisions) on that index.

Microsoft Decision 1.1 formalizes a specialized approach to AI-assisted workflow decisions. Its general availability, multimodal inputs, ecosystem access and disclosed token-based pricing give developers several routes to test it in real applications. The reported benchmark lead is notable, but the practical measure will be how well the model performs against each organization’s own data, exceptions and operational requirements.

## Top comments (0)
