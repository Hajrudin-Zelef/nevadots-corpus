---
id: collect-261121-veille-ia/microsoft-actu/microsoft-ignite-2026-day-1-announcements
title: "Microsoft Ignite 2026 Day 1 Announcements"
domain: microsoft-actu
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-11-17"]
keywords: ["microsoft", "copilot", "generally", "available", "agents", "frontier", "spend", "preview"]
source: https://www.epcgroup.net/insights/microsoft-ignite-2026-day-1-announcements
source_anchor: ""
source_lines: [1, 170]
section: "Microsoft_Actu"
---
By Errin O'Connor — Founder & Chief AI Architect, EPC Group · Microsoft Solutions Partner across all six designations · G2 Leader in BI consulting

## What did Microsoft announce on Day 1 of Ignite 2026?

Satya Nadella opened Ignite on Tuesday, November 17, with a keynote framed around what Microsoft calls a frontier ecosystem: people and agents working together, organizations keeping control of their own intelligence, and trust in the technology. <sup>[3]</sup><sup>[4]</sup> The announcements below are the ones an enterprise running Microsoft 365, Azure, Power BI or Fabric will be asked about this week. The status column uses Microsoft's own words; "Frontier" means Microsoft's early-access program, not general availability.

| Announcement | What it is | Status | Decision it changes | 
|---|---|---|---|
| Copilot Home | Chat and Cowork in one place | Generally available | 3. Metered spend | 
| Office in Copilot | Word, Excel and PowerPoint inside Copilot | Generally available | None; included in the license | 
| Copilot Workspaces | Shared space for a team, its work and Copilot | Frontier program | 2. Agents and owners | 
| Code and Autopilot | Plain language to working apps; a persistent, proactive agent | Rolling out to Frontier customers | 5. Who publishes what agents build | 
| Copilot Managed Runtime | Hosting for what people build in Copilot | Generally available | 5. Who publishes what agents build | 
| Today (Brief and Feed) | Morning briefing and a refreshing priority feed | Private preview (Frontier) | 3. Metered spend (the Feed uses credits) | 
| Microsoft-Decision-1.1 | Small model for routing and classification | Generally available in Foundry | 3. Metered spend | 
| MAI Code 2 and Code 2 Flash | Microsoft's agentic coding models | Generally available in GitHub; Code 2 Flash in private preview in Foundry | 5. Who publishes what agents build | 
| MAI Cyber 2 Flash | Microsoft's security model | Preview, in Codename MDASH only | 6. Evidence | 
| GPT-6.2 Astra and Sol | OpenAI's latest models in Foundry | Generally available in Foundry | 3. Metered spend | 
| Agent 365 Business Analytics | AI spend, adoption and outcomes for business leaders | Frontier program | 7. What is measured | 
| Agent 365 Cost Management | Spend drivers, policies, limits and alerts for IT | Preview | 3. Metered spend | 
| Microsoft IQ | Enterprise context for every agent | Generally available, no new SKU | 4. Numbers AI answers from | 
| Microsoft Perception | Blue, red and green security agents run through playbooks | Generally available | 6. Evidence | 
| AI Transparency Initiative | Open approach to AI transparency with more than 40 organizations | Launched | 6. Evidence | 
| Azure HorizonDB | Distributed, managed PostgreSQL | Generally available; in Fabric, public preview | 4. Numbers AI answers from | 
| Salesforce in Copilot | Salesforce context and actions inside Copilot | Public preview (beta) | 2. Agents and owners | 
| Azure Cobalt 200 VMs | Microsoft's next Arm-based virtual machines | Generally available | None of the seven | 

Sources for the table: <sup>[2]</sup><sup>[5]</sup><sup>[7]</sup><sup>[8]</sup><sup>[10]</sup><sup>[11]</sup><sup>[12]</sup><sup>[13]</sup><sup>[14]</sup><sup>[15]</sup><sup>[16]</sup><sup>[19]</sup><sup>[20]</sup>. The decision column maps each item to the Seven-Decision Filter from EPC Group's pre-event brief.

## What changed in Microsoft Copilot?

Microsoft described the new Copilot as one connected system for people and agents. Two parts are generally available now: **Home**, where Chat and Cowork come together, and **Office in Copilot**, which brings Word, Excel and PowerPoint inside Copilot. Microsoft says Office in Copilot is already included in a Copilot subscription license. <sup>[5]</sup>

**Copilot Workspaces** gives a team, its work and Copilot one shared space in the Microsoft Copilot app. Microsoft calls it the evolution of Copilot Notebooks, offers it through the Frontier program, and lists bringing a personal Autopilot into a workspace as coming soon. **Code**, which turns plain language into working apps, and **Autopilot**, a persistent and proactive agent, are rolling out to Frontier customers; the Book of News dates Code's general availability to December. **Copilot Managed Runtime**, which hosts what people build, is generally available. <sup>[2]</sup><sup>[3]</sup>

**Today** enters private preview in Outlook and the Copilot app through Frontier. Its licensing is worth reading closely: the Today Brief is included with a Microsoft 365 Copilot license, while the Today Feed draws on Copilot credits. Microsoft also says Auto now recommends moving from Chat to Cowork for complex tasks. <sup>[2]</sup> Both details push more work onto the metered side of Copilot, which is why EPC Group maps them to decision 3. The controls for that side are covered in Copilot Cowork: access, cost and controls and the usage-based billing playbook.

## Which new models did Microsoft announce?

Microsoft's model message was about matching each task to the model with the best economics, rather than sending everything to the largest model. <sup>[4]</sup>

- **Microsoft-Decision-1.1** is generally available in Microsoft Foundry. It handles the small decisions inside an agent system: routing, classification, prioritization, verification and workflow control. Input tokens are listed from $0.042 per million (Global Standard).<sup>[7]</sup>
- **MAI Code 2 and MAI Code 2 Flash** are generally available in GitHub; Code 2 Flash is in private preview in Foundry.<sup>[2]</sup><sup>[19]</sup>
- **MAI Cyber 2 Flash** is Microsoft's security model. It is in preview inside Codename MDASH only.<sup>[2]</sup><sup>[20]</sup>
- **GPT-6.2 Astra and GPT-6.2 Sol** from OpenAI are generally available in Foundry. Microsoft lists GPT-6.2 Sol at $2.00 per million input tokens and $10.00 per million output tokens, and GPT-6.2 Astra at $8.00 and $40.00 (Global Standard, short context).<sup>[8]</sup>
- **Meta's Muse Wonder, Muse Spark 1.3 and Muse Ember** are coming soon to Foundry, which Microsoft says already offers more than 11,000 models.<sup>[9]</sup><sup>[3]</sup>
- **MAI Licensed Weights** , which Microsoft frames as model weights kept under the customer's control, is coming soon through Microsoft's Frontier Company engagements.<sup>[17]</sup>

Microsoft published its own cost comparisons for these models, and they use different baselines from one announcement to the next. Treat them as a reason to test, not as a forecast: run two or three of your real tasks through the router and price the result. Customer examples Microsoft published include Alaska Airlines, whose custom MAI model raised maintenance-report classification accuracy from 81% to 91%, and Epic, which uses Microsoft-Decision-1.1 across patient experience, clinical care and back-office apps. <sup>[17]</sup><sup>[21]</sup>

## How does Agent 365 change AI spend management?

Microsoft presented Agent 365 as FinOps for AI and split it in two. <sup>[3]</sup>

- **Agent 365 Business Analytics** is for business leaders. It brings AI spend, adoption and outcomes together, with measures such as success rate, user satisfaction and assisted hours, plus agentic Q&A, custom reports and export into existing reporting tools.<sup>[3]</sup> It is in the Frontier program, with features dated November and December 2026. Microsoft says it needs at least one Microsoft 365 Copilot license for Microsoft services; non-Microsoft services need Agent 365 or ME7 licensing.<sup>[10]</sup>
- **Agent 365 Cost Management** is for IT: what is driving spend, spending policies, limits and alerts. The core is in preview; forecasting, Q&A and recommendations are rolling out through Frontier.<sup>[2]</sup>

Satya Nadella said there are now more than 50 million agents in Agent 365. <sup>[4]</sup> That number is why EPC Group puts Agent 365 against decisions 3 and 7: a cost dashboard does not set a limit, and an analytics view does not choose the three measures a CFO will accept. Decide those first, then let the tools report against them. EPC Group's AI ROI assessment sets that baseline, and the CFO AI governance conversation covers how to present it.

## What is Microsoft IQ?

Microsoft IQ is Microsoft's enterprise context layer: a governed understanding of an organization that every agent can draw on, so nobody has to explain how the business works each time an agent is built. It brings together Work IQ, Fabric IQ, Foundry IQ and Web IQ with more than 350 third-party connectors, and connects the build tools in GitHub, Foundry and Fabric. <sup>[13]</sup>

The Book of News lists Microsoft IQ as generally available with no new SKU or subscription; business plugins are in the Frontier program, and Web IQ starts at $1 per 1,000 calls. <sup>[2]</sup><sup>[13]</sup> Microsoft also named Publicis Groupe as building custom AI skills on it. <sup>[18]</sup>

For decision 4, Microsoft IQ raises the stakes rather than settling them. Context is only as trustworthy as the numbers behind it, so the semantic models an agent reads still need an owner and a certification date. See Fabric IQ ontology vs. semantic model and semantic model certification.

## What changed in security and AI trust?

**Microsoft Perception** is generally available. It runs teams of blue, red and green agents through playbooks in a closed loop; security teams set the guardrails and approve high-impact actions, and automated triggers start a playbook when an incident meets the criteria they set. <sup>[11]</sup><sup>[3]</sup>

The **AI Transparency Initiative** launched with more than 40 organizations to build a common, open approach to AI transparency, security and governance across models and providers. <sup>[12]</sup> Microsoft's security announcements also made Defender runtime protection for local AI agents generally available for Defender for Endpoint Plan 2, and put Purview grounding protection for Copilot on a January preview. <sup>[11]</sup><sup>[2]</sup>

These map to decision 6: the record a company can produce when an insurer, a regulator or its own board asks what its AI systems did. See TRAIGA compliance for Copilot and AI agents and Microsoft Purview consulting.

## What changed in data, Fabric and Azure?

**Azure HorizonDB**, a distributed, managed PostgreSQL database, is generally available; HorizonDB in Fabric is in public preview. Microsoft says it delivers up to 8x faster OLTP performance than self-managed PostgreSQL, based on its own HammerDB benchmark, and scales to 128 TB. <sup>[14]</sup> Bringing it into Fabric lets agents reason over live transactions next to historical data. <sup>[3]</sup>

In Fabric and Power BI, Microsoft put Power BI Pro data apps and Fabric IQ connectors for ChatGPT and Claude into preview. <sup>[2]</sup> Agents that read live transactional data and build data apps both land on decision 5: who may publish what an agent builds. See apps in Power BI and Fabric Apps governance.

On Azure, Microsoft described rearchitected compute, storage and networking primitives for agents, and made **Azure Cobalt 200** virtual machines generally available: up to 50% better performance than Cobalt 100, in 14 regions today and expanding to 23 by the end of 2026, according to the Azure blog (the Book of News says 24). <sup>[16]</sup><sup>[2]</sup>

## What did Microsoft and Salesforce announce?

Microsoft and Salesforce announced Copilotforce, which brings Salesforce customer context and actions into Microsoft's AI tools. **Salesforce in Copilot** is in public preview (beta) with more than 38 prebuilt sales skills; sellers retrieve customer information and take supported actions within their existing Salesforce permissions, through authenticated Salesforce MCP access. Sales Cloud in Teams is in a pilot, OneLake data in Salesforce Data 360 is generally available, and Data 360 to OneLake is in public preview. Accenture is the first customer. <sup>[15]</sup>

For decision 2, a plugin that can take actions in a CRM is an agent with an owner question attached: who approves which actions, and who reviews the log.

## Day 1 through the Seven-Decision Filter

EPC Group's pre-event brief argued that every announcement either changes one of seven decisions or can wait. Here is Day 1 against that filter.

| Decision | What Day 1 changed | Decide now | 
|---|---|---|
| 1. Who owns AI | Nothing; no announcement names an owner | One named executive with budget authority | 
| 2. Agents and owners | Workspaces, Autopilot and Salesforce in Copilot add agents that act | An owner for each agent before it is enabled | 
| 3. Metered spend | Cost Management preview; the Today Feed uses credits; Auto points complex tasks to Cowork | Who can spend, the per-user limit and the monthly review | 
| 4. Numbers AI answers from | Microsoft IQ is generally available; HorizonDB reaches Fabric | Which semantic models are certified, and by when | 
| 5. What agents build | Code (Frontier), Managed Runtime and Power BI Pro data apps | The publishing lanes and who signs off | 
| 6. Evidence in 60 days | Perception, Defender runtime protection, the Transparency Initiative | Who assembles the evidence pack and where it lives | 
| 7. What is measured | Agent 365 Business Analytics | The three measures the CFO accepts, and a report date | 

The pattern is the same one the brief predicted. Microsoft shipped better tools for decisions 3, 6 and 7, and more ways for agents to act under decisions 2 and 5. None of it makes the decisions for you.

## What should an enterprise do this week?

1. **Sort by status.** Plan rollouts only around what is generally available in your cloud. Treat Frontier and preview items as pilots with an end date.
2. **Check the meters.** For each item you plan to use, find whether it sits in the seat price, on Copilot credits or on Azure consumption. The Today Feed and Cowork are on credits.
3. **Name owners before enabling agents.** Workspaces, Autopilot and CRM plugins act on people's behalf. Each needs an owner and an approval rule.
4. **Set the measures, then turn on the analytics.** Agree the baseline and the three measures with finance before Agent 365 Business Analytics starts reporting.
5. **Price your own tasks.** Run a few real workloads through Foundry's router and the new models before trusting any vendor comparison.

## Questions for your Microsoft account team

1. Which Day 1 items are generally available today in the cloud our tenant runs in, and which are Frontier only?
2. Which of them bill on Copilot credits, and what limit applies by default?
3. What does Agent 365 Business Analytics need from us in licensing to report on our non-Microsoft agents?
4. Does enabling Microsoft IQ change which data our existing agents can reach?
5. When does Office in Copilot, Workspaces or Code reach our government cloud, if we run in one?

## Where EPC Group fits

EPC Group has delivered Microsoft consulting since 1997 and holds all six Microsoft Solutions Partner designations, with more than 60 Copilot rollouts and more than 240 readiness assessments since August 2025. A one-hour call with a senior architect sorts your Ignite list against the Seven-Decision Filter for your own tenant. Related work: Microsoft Agent 365 consulting, Copilot Studio agent development, Microsoft Fabric consulting and Azure cloud services.

## The data behind this (sources and verification)

Third-party figures above are attributed to their named sources as of the Last verified date. EPC Group audit figures are directional findings from client engagements.

## Frequently asked questions

### When and where is Microsoft Ignite 2026?

November 17 to 20, 2026, at Moscone Center in San Francisco and online. Satya Nadella gave the opening keynote on Tuesday, November 17.

### What is Copilot Workspaces?

A shared space in the Microsoft Copilot app where a team, its work and Copilot sit together. Microsoft calls it the evolution of Copilot Notebooks and offers it through the Frontier program; bringing a personal Autopilot into a workspace is listed as coming soon.

### Does Office in Copilot cost extra?

Microsoft says Office in Copilot is already included in a Copilot subscription license. The new Today Feed is different: it draws on Copilot credits, while the Today Brief is included with a Microsoft 365 Copilot license.

### What is Microsoft-Decision-1.1?

A small, fast model for the routine choices inside agent systems: routing, classification, prioritization, verification and workflow control. It is generally available in Microsoft Foundry, with input tokens listed from $0.042 per million (Global Standard).

### Is GPT-6.2 available in Microsoft Foundry?

Yes. Microsoft announced GPT-6.2 Astra and GPT-6.2 Sol as generally available in Foundry on November 17, 2026, with published per-token prices.

### What does Agent 365 Business Analytics measure?

AI spend, adoption and business outcomes, using measures such as success rate, user satisfaction and assisted hours. It is in the Frontier program and needs at least one Microsoft 365 Copilot license for Microsoft services.

### Does Microsoft IQ need a new license?

Microsoft's Book of News lists Microsoft IQ as generally available with no new SKU or subscription. Business plugins are in the Frontier program, and Web IQ is billed per call.

### What is Microsoft Perception?

A generally available security service that runs teams of blue, red and green agents through playbooks. Security teams set the guardrails and approve high-impact actions.

## Ready to act on this?

Start with the practice most relevant to your estate, or reach out directly for a senior-architect conversation.

📧 contact@epcgroup.net · 📞 (888) 381-9725

Multiple models. One truth.
