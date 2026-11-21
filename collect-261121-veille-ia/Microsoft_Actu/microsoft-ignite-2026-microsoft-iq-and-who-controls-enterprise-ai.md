---
id: collect-261121-veille-ia/microsoft-actu/microsoft-ignite-2026-microsoft-iq-and-who-controls-enterprise-ai
title: "Microsoft Ignite 2026: Microsoft IQ and who controls enterprise AI"
domain: microsoft-actu
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["microsoft", "work", "copilot", "ignite", "business", "change", "salesforce", "enterprise"]
source: https://www.thisisfrontier.ai/news/microsoft-ignite-2026-who-runs-the-agentic-enterprise
source_anchor: ""
source_lines: [1, 63]
section: "Microsoft_Actu"
---
## What changed

Perhaps the most revealing thing Microsoft showed at Ignite was a way to change a Salesforce record: Put a Teams conversation and an Outlook thread next to a live opportunity, give Copilot permission to take a supported action, and suddenly an AI interface can change the state of a business it has been watching. That is certainly a different proposition from helping draft the follow-up email. While Salesforce still governs which CRM actions the user can take, Microsoft is moving to govern the work context on its side. But if that record changes incorrectly, the CIO will need clear evidence from both.<sup>1</sup>

Satya Nadella opened Ignite in San Francisco on November 17 in a far-reaching talk by acknowledging the work Microsoft still has to do on resilience. He then dubbed Copilot an operating system for work and made an unusually direct argument about the limits of trusting AI models: enterprises must “separate the supply of intelligence from the authority over it.” Taken together, those remarks define this year’s event. Microsoft wants to give agents more work, give enterprises a way to constrain them, and supply much of the machinery that does both.<sup>2</sup><sup>3</sup>

*This is Frontier Research’s anchor article for Microsoft Ignite 2026. Come back as we expand it as new announcements materially change the picture for CIOs and collect our reporting in the coverage block below.*

### From a conversation to a standing assignment

Microsoft is bringing Chat, Cowork, Code and Autopilot into Copilot. Workspaces gives teams a shared place to track evolving work. Autopilot goes further. It can carry a project forward or run recurring work with its own identity, memory and access to tools. Both are rolling out through Microsoft’s Frontier program. The Salesforce plugin is in public preview, and the agreement includes two-way Zero Copy integrations between Salesforce Data 360 and Microsoft Fabric. Copilot can become the point of interaction while the underlying business applications still hold the records and enforce their own rules.<sup>4</sup><sup>1</sup>

The shift shows up in a mundane decision: who checks the work tomorrow? Once an assignment persists after the employee closes the app, approval design, exception handling and accountability become part of the workflow. Copilot Code takes the same change in another direction. An employee can create an internal app in natural language, and the generally available Copilot Managed Runtime gives supported apps a common hosting and governance path. At scale, that means the application inventory is now a question for every business unit with permission to build.<sup>4</sup>

## Why it matters

### Microsoft IQ is the announcement to watch

Nadella then spent much of the keynote explaining what sits underneath Copilot. The important move was to take that system and sell its capabilities as an enterprise platform. Microsoft IQ brings together context from Microsoft 365, business data (from Graph and business APIs), and metrics from Fabric, unstructured knowledge from Foundry, and information from the web and third-party systems. It packages context, skills and tools for reuse. The core platform is generally available without a new IQ subscription. Microsoft’s new hierarchical business plugins, which let teams inherit and extend guidance for specific scenarios, are in its Frontier program.<sup>2</sup><sup>4</sup><sup>5</sup>

Think about what a company actually knows. A contract has an approval policy. The legal team also knows which clause triggers escalation in practice, which documents settle a dispute, and which exceptions require a call to the general counsel. That second kind of knowledge usually lives in people, scattered documents and the history of past decisions. If it can be captured as governed guidance and tested against actual work, a new agent can start with what the firm has learned. Proof points already exist: Publicis is already testing custom skills that combine its expertise and data with IQ against real client scenarios, according to Microsoft.<sup>5</sup><sup>6</sup>

Foundry is the second half of that bet. Its managed agent harness and continuous-improvement tools give customers a way to define private evaluations, observe production behavior and use the results to refine configurations or train supported models. The upside is compounding learning. A better evaluation or a well-understood exception can improve multiple workflows. The commercial implication is just as important. Microsoft can offer a wide choice of models while making its context, evaluation and execution layers increasingly valuable to keep.<sup>2</sup><sup>5</sup>

That is where CIOs should press the ownership claim. A connector lets another agent reach a service. Portability means being able to carry the source mappings, skills, policy decisions, evaluations and records of past actions into another environment, with enough fidelity to keep operating. Microsoft’s support for access through MCP is useful. A working exit or continuity test is stronger evidence.<sup>4</sup><sup>5</sup>

### The data and compute choices follow the work

Nadella called the convergence of operational and analytical data a goal he had worked toward for roughly 25 years. Azure HorizonDB, Microsoft’s distributed managed PostgreSQL service, is generally available. His Fabric vision is an agent that can reason over live transactions and years of history together. Consider an order agent that sees stock move while it is making a commitment to a customer. Freshness suddenly matters to the correctness of the action, and the agent still needs an authorized transaction path and rules for what happens when the stock position changes midway through the job.<sup>2</sup><sup>4</sup>

The same logic explains why Microsoft emphasized both a small decision model and a new general-purpose CPU. The generally available Microsoft-Decision-1.1 targets routine decisions such as routing, classification and verification. Cobalt 200 VMs are also generally available, and Microsoft points out that agents spend much of their time outside the model, running tools and spinning up workspaces. Model choice, tool execution, latency and review costs all feed into the cost of a completed task. Azure Catalyst, now generally available, also belongs in the conversation because surfacing business logic in older applications is part of making it usable by agents. Hybrid and isolation options, including preview capabilities around Kubernetes and confidential computing, give CIOs more placement choices where data, resilience or policy warrants them.<sup>4</sup><sup>6</sup>

## Frontier take

### The platform that grants authority needs scrutiny of its own

Microsoft is making a serious attempt to govern agents as actors. Agent 365 extends oversight to agents, Entra supplies identity, and Windows and cloud isolation add boundaries around execution. Perception, now generally available, applies agentic workflows to security operations. Its ability to coordinate detection and remediation brings the same governance question back inside the security team. Which response can run immediately, which needs an approver, and who can reverse it? Microsoft’s AI Transparency Initiative, including expanded collaboration with NIST’s Agent Standards Initiative, makes independent verification an explicit part of the agenda.<sup>2</sup><sup>4</sup><sup>3</sup>

There is an economic version of the same question. Agent 365 Business Analytics, in the Frontier program, brings spend and measures like reported success and assisted hours into view. Useful numbers, especially for a fleet of agents. But a business owner has to decide whether the workflow actually improved. An agent that closes work quickly and creates a second queue for people to clean up has shifted the cost. Count review, retries, rework and recovery alongside the usage bill. Put the resulting cost per acceptable outcome next to the human baseline.<sup>4</sup>

Microsoft is offering CIOs a credible way to wire agents into the operating life of a company. It is also asking them to trust a tightly integrated supplier with more of the context and machinery needed to keep that company running. Nadella’s opening acknowledgment of resilience gives the story a practical test. If Copilot or its control layer is unavailable, can people still see which orders, cases and approvals are in flight? Can they safely resume the work? Can another system recreate the record? The best proof of enterprise control will come on the day a CIO has to exercise it.<sup>2</sup>

### More Frontier coverage from Microsoft Ignite 2026

This section will grow as Frontier publishes its Ignite reporting.

 **Microsoft Ignite 2026 Moves Copilot From Application Feature to Enterprise Work Layer**
Copilot is becoming a shared layer for enterprise work. Why workspaces, embedded applications and new infrastructure change how CIOs govern execution across business systems.

 **Microsoft Brings Perception to Production. Now Comes the Multivendor Agent Reality Test.**
One defense for an enterprise full of agents? Where Perception strengthens Microsoft’s multivendor security case—and where CIOs still need proof of control.

 **Microsoft’s Windows Server 2028 Preview Makes Legacy Compatibility the Exception**
A safer server estate starts with harder choices. Why hardened defaults could justify the compatibility work—and what CIOs should test before adoption.

 **Microsoft and Salesforce’s Copilotforce Pushes Enterprise AI Across System Boundaries—Before the Portfolio Is Fully Ready**
Agents are crossing the Microsoft–Salesforce boundary. What that means for enterprise workflows, permissions and control while the portfolio is still taking shape.

## Three moves for CIOs to validate these developments

1. **Pilot one cross-vendor workflow with a reversible write.** Give the agent a named identity and a tightly scoped action. Capture the inputs, policy decision and downstream result in evidence your team can inspect. Run the failure drill before expanding the permissions.
2. **Treat company knowledge and evaluations as enterprise assets.** Assign owners for skills and policy, version the evaluations, and test whether a second model or runtime can use the same knowledge and meet the same standard. Ask for a practical export and recovery demonstration before making IQ the sole home of critical process knowledge.
3. **Set an operating and economic threshold for autonomy.** Compare cost per correctly completed workflow with the current baseline, including human supervision and rework. Require a named business owner, a tested way to stop and resume work, and explicit approval for actions with material financial, operational or customer impact.
