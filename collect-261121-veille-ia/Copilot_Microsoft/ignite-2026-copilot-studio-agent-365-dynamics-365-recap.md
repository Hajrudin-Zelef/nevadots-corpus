---
id: collect-261121-veille-ia/copilot-microsoft/ignite-2026-copilot-studio-agent-365-dynamics-365-recap
title: "Ignite 2026: Copilot Studio, Agent 365 & Dynamics 365 Recap"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["copilot", "dynamics", "agent", "studio", "work", "microsoft", "available", "agents"]
source: https://www.jondoesflow.com/post/microsoft-ignite-2026-power-platform-dynamics-365
source_anchor: ""
source_lines: [1, 153]
section: "Copilot_Microsoft"
---
Microsoft Ignite 2026 kicked off in San Francisco this week, and the Book of News is long. If you work with Power Platform, Copilot Studio or Dynamics 365, most of it isn't for you. Six announcements are, and they all point the same way.

The interface is moving into Copilot. The governance work is moving up the agenda. Both mean more work on the foundations, not less.

Here's what was announced, what's actually available today, and what I'd do about each one. Availability is quoted from Microsoft's own wording, because "announced" and "available" are not the same thing.

The short version

Dynamics 365 Sales and ERP apps plugins for Copilot: generally available

Copilot Studio agents built locally and published with a CLI, via Copilot Managed Runtime: runtime GA, agents and workflows piece has no stated status yet

Work IQ read and act permissions, action policies and tenant-wide limits: expanded controls in preview

Agent 365 Cost Management: spend visibility and controls in preview

Agent 365 governance (action centre, Agent Map, Dynamic Agent Risk): mostly preview, management insights GA

Microsoft Foundry Agent Optimizer and multi-turn evaluation: generally available

1. Dynamics 365 in Copilot: Sales and ERP plugins

Two Dynamics 365 plugins for Microsoft Copilot are now generally available.

The Dynamics 365 Sales plugin lets sellers build account plans, prepare for customer meetings and prioritise their pipeline from inside Copilot. Deal insights track progress against the quarterly forecast and flag deals at risk, and it can build a PowerPoint deck.

The Dynamics 365 ERP apps plugin brings ERP context and actions into Copilot Cowork and Excel through business skills. Microsoft's example is a sourcing lead working through purchase requisitions, requests for quotes and purchase orders in Source-to-Pay.

Microsoft's own framing is that Copilot is the new front door to Dynamics 365. I think that's right, and it changes three things on a project:

Data quality matters more, not less. If close dates are fiction and stages are a guess, Copilot will flag the wrong deals with complete confidence.

Your security model reaches further. Check roles and business units properly before people start asking Copilot about accounts.

Adoption metrics need a rethink. Logins to the Sales app stop meaning much when the work happens somewhere else.

What to do: switch it on for one sales team and log what Copilot gets wrong. That list is your data quality backlog.

2. Copilot Studio ALM with Copilot Managed Runtime

Copilot Managed Runtime is generally available. It's Microsoft-managed hosting for things built in Copilot Code, Copilot Cowork, Copilot Studio or compatible third-party tools, with an optional Fabric data backend.

The part ALM people will care about is the first iteration of Agents and Workflows in the runtime. You can build Copilot Studio agents and workflows locally, change them in your dev tool of choice, publish them back to Copilot Studio with the CLI, and keep them under version control in platforms like GitHub.

The Book of News doesn't give an availability status for that agents and workflows piece, so check before you plan a project around it.

Why it matters: agent instructions are effectively code now. A one-line change to an agent's instructions can change its behaviour more than a week of flow changes. That deserves a diff, a pull request and a second pair of eyes.

What I don't know yet is how this sits alongside solutions, environment strategy and Power Platform pipelines. Don't rip up your pipelines this week. Try the local build and CLI publish route on one low-risk agent first.

3. Work IQ: separating read from act

Work IQ is generally available in Microsoft Foundry and in Copilot Studio. It's the layer that connects Copilot and agents to your organisation's knowledge, business data and tools, while honouring existing permissions. Business Applications in Work IQ brings Dynamics 365 CRM and ERP and Power Platform data together with Microsoft 365 context, with ERP in preview.

The governance detail is the part I'd want every Copilot Studio maker to read:

WorkIQ.Read and WorkIQ.Act permissions separate reading from acting across Microsoft 365, business applications and connected sources

Action policies can allow an action, block it, or require a person to approve first

A tenant-wide upper limit on agent actions, which individual agent policies can narrow but never exceed

A policy workflow in Agent 365 where makers propose, admins approve or deny, and Work IQ checks each call against the agent's identity

Prompt-injection protection, using an isolated model with no tool access to clean content before agents see it

The Book of News lists the expanded enterprise controls, including granular policies and human approval options, as preview.

This is least privilege, applied to agents. It's the same principle as Dataverse security roles. Start every agent on read, prove it's useful, then grant act for specific actions, with an approval step in front of the ones that matter.

What to do: for each agent you're building, write down which actions it genuinely needs to take and which of those a person should approve. When the controls land, the policies become a five-minute job.

4. Agent 365 Cost Management

Sooner or later every agent project hits the same question: what's this going to cost each month?

Agent 365 Cost Management is a central view in the Microsoft admin centre of usage-based AI. It covers Copilot Cowork, Copilot Code, GitHub Copilot CLI, Copilot Studio, Copilot Managed Runtime, Autopilot and the Work IQ APIs. Admins can see spend drivers, set policies, limits and alerts, and extend analysis through Microsoft Fabric for departmental chargeback.

Status, as Microsoft states it:

Spend visibility and controls are in preview

Forecasting and optimisation recommendations are in the Frontier programme

Value insights across Microsoft and non-Microsoft AI services, and delegation to business leaders, expand in December

Insights for Microsoft services need at least one Microsoft 365 Copilot licence in the tenant; non-Microsoft insights need an Agent 365 licence

Two things to settle before you switch on limits. First, a limit is a design decision: agree with the business owner what happens to someone mid-task when the cap is hit. Second, chargeback forces the ownership conversation. If spend lands on a department, someone in that department has to own the agent.

5. Agent 365 governance: finding ownerless agents

Microsoft is building a screen specifically to surface ownerless agents. That tells you how common they are.

Action centre: brings governance tasks into one view, starting with inactive and ownerless agents for review and cleanup. Expected in private preview in December.

Agent Map classification: helps admins see what agents do and spot agents doing similar work, to inform keep or consolidate decisions. In preview.

Management insights for Microsoft agents: active users, sessions, activity, exception rate and run time, regardless of Agent 365 licensing. Generally available. Full management, security and governance still need an Agent 365 or ME7 licence.

Dynamic Agent Risk: combines runtime signals, posture and blast radius into a low, medium or high risk rating per agent. In preview.

Power Platform teams know this problem well. Give people a low-code tool and you get a long tail of things built for a good reason, used for a month, then forgotten. The maker moves teams. The agent keeps running.

What to do today: use the GA management insights to pull your agent list, sort by active users, and find everything nobody uses. Each one needs a named owner or a retirement date.

6. Microsoft Foundry: Agent Optimizer and continuous improvement

Microsoft Foundry now has a learning loop driven by production signals:

Multi-turn evaluations assess whole conversations rather than single responses

Insights group recurring failures and rank the most frequent issues and fixes

Agent Optimizer proposes candidate changes and tests them against your team's evaluations, so you can compare results and choose what to deploy

Agent Optimizer, multi-turn evaluation and insights are generally available. Human annotation, Managed Training and Programmed Training are in preview.

The catch: everything runs against your evaluations. If your evaluation set is five prompts someone typed the afternoon before go-live, the optimiser will get very good at those five prompts. I do like that a person still chooses what to deploy.

What to do: pull twenty real conversations from your agent's logs, including the awkward ones, and turn them into your first proper evaluation set. That applies whether you build in Foundry or Copilot Studio.

Your Ignite 2026 checklist

Pilot the Dynamics 365 Sales plugin with one team and log what it gets wrong

Trial the Copilot Studio CLI route on one low-risk agent

Map read and act needs for every agent you're building

Put a named owner next to every agent in your tenant

Agree with business owners what happens when a spend limit is hit

Build an evaluation set from real conversations

FAQ

Is the Dynamics 365 Sales plugin for Copilot generally available?

Yes. Microsoft lists both the Dynamics 365 Sales plugin and the Dynamics 365 ERP apps plugin as generally available.

Can I put Copilot Studio agents under version control in GitHub?

Microsoft has announced a first iteration of Agents and Workflows in Copilot Managed Runtime that lets you build Copilot Studio agents locally, publish with the CLI and use version control through platforms like GitHub. The Book of News doesn't state its availability yet.

Is Agent 365 Cost Management available now?

Spend visibility and controls are in preview. Forecasting and recommendations are in the Frontier programme, with more capabilities expanding in December.

Is Work IQ available in Copilot Studio?

Yes. Microsoft says Work IQ is generally available in Foundry and in Copilot Studio. Some of the newer controls, including the listen capability and expanded enterprise controls, are in preview.
