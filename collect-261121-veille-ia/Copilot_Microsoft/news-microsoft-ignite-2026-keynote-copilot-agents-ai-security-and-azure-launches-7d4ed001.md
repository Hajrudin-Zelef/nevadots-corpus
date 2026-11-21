---
id: collect-261121-veille-ia/copilot-microsoft/news-microsoft-ignite-2026-keynote-copilot-agents-ai-security-and-azure-launches-7d4ed001
title: "News microsoft ignite 2026 keynote copilot agents ai security and azure launches"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["copilot", "microsoft", "available", "news", "generally", "nadella", "agents", "code"]
source: ""
source_anchor: ""
source_lines: [1, 94]
section: "Copilot_Microsoft"
---
The framing: three pillars
Nadella said the goal is a stack built on human ambition, control of your organization's intelligence and learning, and trust. In practice, that means connecting AI assistants and agents to company data and workflows, then adding the governance, security and cost visibility IT will want. Nadella called Copilot the new OS for work, built around Chat, Cowork, Code and Autopilot. All figures below are Microsoft's own claims unless noted.
Copilot: what is actually available
Copilot Workspaces is the headline "multiplayer" feature, where people and their agents share context. Microsoft's Book of News describes it as the evolution of Copilot Notebooks, available in the Copilot app through the Frontier program. Personal Autopilot agents are "coming soon" to Workspaces rather than shipping today. Neowin reports Workspaces as Frontier Public.
Autopilot is a long-running agent for personal productivity. According to the Book of News it has its own identity, memory and tool access inside the tenant. Colleagues can @mention a person's Autopilot in Teams. Its actions are tied to its own agent ID and visible to admins. It is rolling out through the Frontier program, not general release.
Copilot Code lets people build dashboards, trackers and apps in natural language. It is in the Frontier program on Windows, web, iOS and Android, with general availability planned for December. Apps can be published through Copilot Managed Runtime, which the Book of News lists as generally available. Published apps appear in the Microsoft 365 admin center.
Home is rolling out worldwide as the starting point for Chat and Cowork. Auto, the model router, can recommend moving a complex task from Chat to Cowork without losing context. That recommendation feature is generally available. Office in Copilot (Word, Excel and PowerPoint inside the conversation) is also generally available.
Business plugins: the Dynamics 365 Sales plugin and the Dynamics 365 ERP apps plugin are both generally available. The ERP plugin works in Copilot Cowork and Excel. Salesforce in Copilot is a plugin in preview with more than 38 prebuilt sales skills.
Microsoft IQ: context as a product
Nadella described Microsoft IQ as the system under Copilot, now "externalized" as a platform. It pulls from Fabric, Foundry, Microsoft 365 and third-party sources. Nadella cited hundreds of new connectors, and a live blog from The Letter Two put the number of third-party IQ connectors at more than 350. That is a journalist's note, not a Microsoft figure.
Per the Book of News, it is built on products customers already use and doesn't require a new subscription. The pieces are:
- Work IQ, for work context.
- Fabric IQ, for business data and metrics.
- Foundry IQ, for unstructured content.
- Web IQ, for real-world information.
Reusable business plugins can be shared and inherited across the organization. Admins should check how licensing and data-access boundaries apply in their own tenant, because the keynote didn't spell those out.
Models: the real GA list
- Microsoft-Decision-1.1 is generally available. The Book of News says it is available in Foundry, OpenRouter and Vercel. It is built for structured decisions such as routing, classification, prioritization and verification. It returns calibrated confidence scores so apps can defer when uncertainty is high. It builds on Microsoft-Decision-1 from October, uses a Gemma 4 26B A4B base, and adds image input. Nadella said more than 4,000 customers have used it.
- Code 2 and Code 2 Flash are generally available in GitHub Copilot's model picker and Copilot Fusion. Code 2 Flash is also in Microsoft Foundry, but only as a limited-access preview, not generally available; Microsoft has announced no Foundry availability for Code 2. The "fraction of frontier cost" claim comes with no benchmark methodology in the material I reviewed.
- Cyber 2 Flash: Nadella called it Microsoft's most capable security model yet. But the Book of News says MAI-Cyber-2-Flash is available in preview only, in Codename MDASH. That is a notable gap between the keynote's tone and the product status.
- GPT 6.2 and Meta's latest models are coming to Foundry, with no dates given.
- MAI Licensed Weights will give "trusted customers" access to Microsoft AI model weights through Frontier Co. Access conditions weren't specified.
Foundry also adds a generally available evaluation and optimization loop (multi-turn evaluation, production insights and an agent optimizer). Human annotation and training features are in preview.
Agent 365 and the cost question
Nadella said Agent 365 already holds more than 50 million agents. He didn't say whether that counts deployed, registered or active agents, so treat the number as a scale signal rather than an adoption metric.
Agent 365 Business Analytics is the FinOps piece. The Book of News says preview and Frontier capabilities show spending drivers and support custom chargeback through Fabric. They also measure outcomes such as task success, satisfaction and assisted hours. In December, value insights will expand across Microsoft and non-Microsoft agents.
For IT, this matters because AI spend has been spreading outside IT budgets. Microsoft is betting that cost-to-value reporting will become a standard admin task. Whether the "value" numbers hold up is a question each finance team will have to test, since the outcome measures come from Microsoft's tooling.
Security: Perception and the Windows angle
Microsoft Perception is now generally available. It is an agentic security system with red agents that simulate attacks, blue agents that detect them and green agents that remediate. Playbooks compose and trigger these teams. Constellation Research reports that Perception covers Microsoft, AWS and Google Cloud, and that AI Readiness Initiatives arrive in preview in January.
The Windows-specific detail came in Microsoft's separate summary. Windows is expanding OS-level protection for agents, with more granular containment through Microsoft Execution Containers, built-in agent identity with Entra support and Agent 365 policies enforced on every device. Microsoft also says hybrid intelligence on Windows will let AI run on the device or in the cloud, starting with GitHub Copilot and Copilot for everyday work. No rollout timelines appear in the material I reviewed, so admins shouldn't assume those controls are available yet.
AI Transparency Initiative
Microsoft says it is working with more than 40 organizations, including Accenture, Adobe, KPMG, Mastercard and Volvo Cars. It has three parts: expanded work with NIST's Agent Standards Initiative, open standards across models and providers, and incorporating the results into Microsoft AI, Foundry and Copilot.
The problem Microsoft describes is an evidence gap. Organizations can see much of what surrounds a model but can't reconstruct what happened between request and outcome. A model's account of itself is not independent evidence. The "see, verify, control" framework asks for records of what the AI was authorized to do, which model and runtime it used, what data and tools it touched, and what it did. Those records should be checkable against independent sources such as identity and policy systems.
This is a standards effort, not a shipped feature. The first deliverables and timelines weren't given. Skeptics may note that a vendor-led initiative can still shape the standards in its favor. The multi-party and NIST involvement is the counterweight.
Azure: Cobalt 200 and Maia 300
Cobalt 200 VMs are generally available. They run on Microsoft's Arm-based processor and deliver up to 50% better CPU performance than Cobalt 100. Microsoft's reasoning is that agents do much of their work outside the model, on general-purpose CPUs.
The region numbers are inconsistent across Microsoft's own materials:
- The Book of News says 24 regions by year-end.
- Microsoft's Ignite summary and Azure's Cobalt blog say 23.
- Current availability is given as 14 regions on the Azure blog, while the Book of News lists 11 named existing deployments.
Check your target region in the Azure portal before planning a migration.
Other Cobalt details from the Book of News:
- Each VM can be set to a consistent-speed or a boost mode.
- Two new VM types are Mpsv4, for large in-memory databases, and Lpsv5, for storage-heavy jobs with up to 23 TB of built-in storage.
Azure's blog adds that Microsoft's silicon engineers saw 1.3 to 4.15 times the performance of Cobalt 100 on their own EDA workloads. That is an internal benchmark.
Because Cobalt is Arm-based, check that your workloads and container images have Arm builds before you migrate.
Maia 300 was cited at 40% more tokens per watt than leading GPU-based systems. That is Microsoft's claim with no independent benchmark attached.
What to do now
- Separate GA from Frontier. Generally available now: Microsoft IQ, Perception, Decision-1.1, Code 2 models in GitHub Copilot, Dynamics 365 Sales and ERP plugins, Copilot Managed Runtime, Office in Copilot and Cobalt 200. Frontier or preview: Workspaces, Autopilot, Copilot Code (GA planned for December), Salesforce in Copilot and Cyber 2 Flash.
- Plan agent governance early. Agent identity, Agent 365 policies and cost tracking are easier to set up before agents multiply.
- Pilot cautiously. Test Perception playbooks and Autopilot in a small scope. The keynote promises control, but your tenant's controls are what count.
- Test Cobalt for Arm compatibility before moving production workloads.
Bottom line
The keynote's strongest facts are the generally available launches and the named integrations. The most ambitious parts, such as multiplayer agent work, transparent AI and the full Windows agent-containment story, are still preview, Frontier or roadmap items. Cost, performance and adoption figures all come from Microsoft. Treat them as claims to validate in your own environment.
Correction (November 17, 2026): An earlier version of this article said Code 2 and Code 2 Flash were generally available in Microsoft Foundry. In fact, Microsoft lists Code 2 Flash in Foundry only as a limited-access preview, and has announced no Foundry availability for Code 2; both models are generally available in GitHub Copilot.
Update: Agentic Users and live Teams transcripts add new governance stakes (November 18, 2026)
UC Today reports that Microsoft also introduced Agentic Users in public preview. These let agents join Teams under their own identity, with an accountable owner and admin controls; users can add them to chats, channels and meetings and @mention them like coworkers.
The same report says Work IQ can expose real-time Teams transcripts to agents while a meeting is still in progress. That would extend the article’s earlier Teams Autopilot detail from post-hoc collaboration into live meeting assistance.
For Windows and Microsoft 365 administrators, the practical issue is no longer just whether an agent can act, but what meeting data it can access while people are speaking. Teams membership, agent ownership, transcript permissions, retention, sensitivity labels and audit trails will need review before enabling these previews broadly.
UC Today also noted that Nadella opened the keynote by acknowledging the Outlook and Teams outage from the prior week and pledged improved resilience. That makes service continuity a direct dependency for organizations planning to rely on Teams-hosted agents and live collaboration workflows.
Update: Autopilot’s separate Entra identity is tied to a named manager (November 18, 2026)
UC Today reports that each Microsoft Autopilot is assigned its own Microsoft Entra Agent ID and a named human manager. IT can scope the agent’s access, disable it, and review sign-ins and actions under the agent’s own identity rather than attributing them solely to the employee it assists.
That adds an explicit ownership layer to Microsoft’s earlier agent-governance description. For administrators, the useful control point is the named manager: offboarding, privilege reviews and incident response should account for both the agent’s Entra identity and the person accountable for it.
UC Today also reports that Microsoft has not published a general-availability date or pricing for Autopilot, and has not clarified whether it will require a Microsoft 365 Copilot license, consume Copilot Credits, or both. Organizations should therefore treat Frontier access as a governed pilot, not as a basis for production licensing forecasts.
Update: Foundry hosted agents gain GA egress controls and resilient execution (November 18, 2026)
Microsoft’s Foundry blog has added production-focused capabilities for hosted agents in Foundry Agent Service. Network egress controls are now generally available, letting teams apply ordered allow/deny rules, outbound headers or a gateway route outside the agent sandbox. Microsoft recommends starting in audit mode to identify real destinations before enforcing a default-deny policy.
Hosted agents also now support generally available resilient execution and configurable idle timeouts. Agents that opt in can recover long-running work after a crash, restart or redeployment, though developers must make handlers idempotent to avoid repeating actions during recovery. Idle timeout can be set from two minutes to four hours per agent version.
For network-constrained deployments, shared subnet IPs are generally available. Microsoft estimates that a /24 delegated subnet can support roughly 60,000 concurrent sessions, up from about 250 under the former per-session IP model; regional session quotas remain the likely operational limit.
Several related features remain previews: private CA trust, memory snapshots for faster cold starts, A2A passthrough, and Azure Policy governance for egress. Microsoft says the Azure Policy preview initially covers Canada Central, Spain Central and Korea Central. A bring-your-own AKS deployment option, which would run Foundry’s agent runtime on customer-operated Kubernetes clusters, is targeted for public preview in early December.
For IT teams, the change makes Foundry-hosted agents more viable inside private-network and compliance boundaries, but policy rollout, certificate handling and recovery behavior still need validation before broad production use.
Update: Foundry expands into voice, browser automation and Microsoft 365 agent identities (November 18, 2026)
Microsoft’s Foundry update adds several capabilities not covered in the earlier hosted-agent rollout. Voice agents are now generally available in Foundry Agent Service, including real-time conversational handling and native telephony connections. That gives organizations a supported path for voice-based customer service and employee assistants on the same managed platform as other Foundry agents.
Microsoft also says computer use and browser automation are generally available. The service uses isolated browser sessions and supports Live View, human takeover and recordings stored in customer-controlled storage. Access to apps behind private virtual networks remains in preview, so teams with internal web applications should not assume that scenario is production-ready yet.
On the integration side, Work IQ and Fabric IQ connections are in preview, while Web Search powered by Web IQ is generally available for prompt agents. Microsoft’s A2A tool for connecting Foundry agents with external agents is also generally available. These features broaden what agents can retrieve and act on, but increase the need to review data permissions and tool-scoping policies.
Microsoft also describes a Microsoft 365 publishing model for supported hosted agents: they can receive an Entra Agent ID, email address, Teams presence and an organizational-chart entry, with dedicated mailbox, calendar and OneDrive resources planned through Microsoft 365 Resources for Agents. For admins, that makes lifecycle controls, access reviews and retention policies more important than treating such agents as ordinary application identities.
Update: Ignite partners announce new Copilot and agent integrations (November 19, 2026)
Technology Record reports that CTERA has announced an integration connecting enterprise file data to Microsoft 365 Copilot and Teams Copilot. The company says it uses the Model Context Protocol and its Global File System to provide permission-aware access while preserving existing security and governance policies. The report does not specify an availability date.
Technology Record also reports that ServiceNow demonstrated deeper integration between its Super Intelligence Control Tower and Microsoft Agent 365. The proposed capabilities include sharing agents across the platforms, applying policies before actions and maintaining an audit trail of agent handoffs. The report describes a demonstration, not a general release.
Separately, Technology Record says IBM and Microsoft launched Imagine Enterprise, an agentic AI platform combining IBM Consulting’s prebuilt agents and industry assets with Microsoft’s AI, cloud and security technologies.
For Microsoft 365 and IT administrators, the CTERA integration could expand the enterprise content available to Copilot, making existing file permissions important to review. The ServiceNow demonstration points to potential cross-platform governance, but teams should verify availability and test policy enforcement and audit coverage before relying on it in production.
Update: Teams Phone gains certified partner voice agents (November 19, 2026)
UC Today reports that voice agents from certified partners can answer incoming Teams Phone calls and route callers to people, queues or departments with context. The outlet identifies NiCE’s Cognigy among the certified offerings and says Microsoft supports voice agents only from certified partners.
The report also names Teams Phone, Twilio and Genesys as telephony or contact-center connections for Foundry’s generally available voice agents. That adds specific integration options to Microsoft’s previously announced Foundry voice-agent availability. For IT teams, the distinction matters: partner voice agents and developer-built Foundry agents represent different deployment and support routes.
UC Today also reports an updated Foundry Live Interpreter API model that translates while a person is still speaking. Microsoft claims fewer translation errors and end-to-end translation in under three seconds. The outlet says Teams Interpreter supports meetings of up to 1,000 participants; those claims should be tested against an organization’s own meeting conditions.
Finally, UC Today says Microsoft’s figure of more than 50 million Agent 365 agents refers to registered agents since the service launched in May. That clarifies the keynote figure, though registration does not establish how many agents are actively used.
Update: Report says Autopilot will use Copilot Credits (November 20, 2026)
IDM.net.au reports that Autopilot will be billed according to usage through Copilot Credits, although final pricing has not been announced. That would narrow the licensing uncertainty noted in the earlier update, which said Microsoft had not clarified whether Autopilot would consume credits, require a Microsoft 365 Copilot license, or both. The report does not resolve the license requirement.
IDM.net.au also reports that a feature called Today is in private preview. It says Today provides a personalized morning brief and prepares work, including email drafts and schedule changes, for approval.
The outlet adds that Copilot Workspaces will come to OneNote over time, without specifying a date.
For IT and finance teams, the reported credit-based billing makes usage monitoring relevant to Autopilot pilots, but it is not yet a basis for a reliable production cost forecast. Administrators should verify the final pricing and licensing terms before expanding access.
References
- Ignite Day 2: Microsoft Puts Voice Agents in Teams, Copilot Everywhere - UC Today UC Today · 2026-11-19T17:32:16.953000+00:00
- Hosted agents in Foundry Agent Service at Ignite 2026: from pilot to production inside your enterprise boundaries Microsoft Foundry Blog · 2026-11-18T17:00:54+00:00
- Microsoft Ignite 2026: Agents Get a Seat in Teams - UC Today UC Today · 2026-11-18T01:52:13.098000+00:00
