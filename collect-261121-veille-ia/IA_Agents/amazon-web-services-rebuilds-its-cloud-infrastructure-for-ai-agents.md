---
id: collect-261121-veille-ia/ia-agents/amazon-web-services-rebuilds-its-cloud-infrastructure-for-ai-agents
title: "Amazon Web Services Rebuilds Its Cloud Infrastructure for AI Agents"
domain: ia-agents
role: reference
task: article
actors: []
dates: []
keywords: ["agents", "agent", "infrastructure", "memory", "openai", "customer", "runtime", "september"]
source: https://pro.edgex.exchange/en-US/news/article/aws-reworks-cloud-to-host-ai-agents
source_anchor: ""
source_lines: [1, 37]
section: "IA_Agents"
---
## Key Takeaways

- â¢AWS released a reworked Bedrock AgentCore Runtime around September 18, 2026, adding memory reclamation and maintaining a consistent cold-start latency of approximately two seconds.
- â¢AWS and OpenAI launched Bedrock Managed Agents in limited preview on September 29, 2026, allowing OpenAI's API-based agents to run entirely within AWS while customer data remains in the chosen jurisdiction.
- â¢AWS announced the public preview of the Well-Architected Agent on October 1, 2026, a tool that analyzes customer infrastructure and returns prioritized security and performance recommendations.
- â¢On October 7, 2026, AWS open-sourced Strands Box, an agent sandbox with OS-level isolation and history-aware policy controls that developers can inspect and run on their own infrastructure.
- â¢AWS's agent push targets three core needsâisolated compute, persistent memory, and permission controlsâthrough AgentCore components including Runtime, Memory, Identity, and Policy Evaluations.

Amazon Web Services is reworking core parts of its cloud platform so it can host AI agentsânot only the human developers who build them. The company is rolling out a set of new building blocks, including fast databases, compute sandboxes, and improved permission systems, reflecting the fact that agents require a different kind of hosting environment.

Across several weeks in September and October 2026, AWS shipped a series of agent-focused updates: a reworked runtime, a managed agent service developed with OpenAI, an automated architecture reviewer, and an open-source sandbox. The common thread is less about smarter models and more about where agents live, what they remember, and what they are allowed to touch.

## What AWS Shipped

The center of the effort is Amazon Bedrock AgentCore, which AWS has been rearchitecting with production deployment in mind. Its components include Runtime, Memory, Identity, and Policy Evaluations.

On or around September 18, 2026, AWS released a reworked AgentCore Runtime. The update adds memory reclamation and keeps cold-start latency consistent at approximately two seconds. A cold start is the delay that occurs when a dormant service must spin up before it can respond. A predictable two-second wake-up matters because agent workloads tend to be either long-running or bursty. Memory reclamation addresses the long-running side, preventing agents from hoarding resources over extended periods.

On September 29, 2026, AWS and OpenAI launched Bedrock Managed Agents in limited preview. The service allows OpenAI's API-based agents to run entirely within AWS, with customer data intended to remain secure and inside the customer's chosen jurisdiction rather than traveling between providers. That data-residency detail is more than a technicality: companies in regulated industries often operate under rules that require certain information to stay within specific regions, so where data physically lives can shape whether a given deployment is permissible at all.

On October 1, 2026, AWS announced the public preview of the AWS Well-Architected Agent, a tool that analyzes a customer's infrastructure and returns prioritized recommendations on security and performance. Unlike the restricted access to the OpenAI partnership, a public preview is open to any AWS customer willing to test unreleased softwareâa standard stage for gathering feedback before general availability.

On October 7, 2026, AWS open-sourced Strands Box, a sandbox for agents featuring OS-level isolation and policy controls, including history-aware action controls. Rather than judging each action in isolation, the system can weigh what the agent has already done before approving the next move. Open-sourcing means developers can inspect the code and run the sandbox on infrastructure they control, rather than using it only as a managed AWS serviceâwhich matters for teams that want to verify for themselves how their agents are isolated.

## Why Agents Need Different Infrastructure

Traditional cloud services were designed around a straightforward model: a person or application sends a request, the server responds, and the interaction ends. Agents break that pattern. They take sequences of actions, call tools, store context between steps, and operate with some degree of autonomy.

That shift creates three practical problems that AWS's updates line up against directly. The first is compute: agents need environments where they can run code and take actions without endangering the rest of a company's systemsâa need targeted by sandboxes like Strands Box. The second is context: persistent memory lets agents carry state across long tasks. The third is permissions: Identity and Policy Evaluations address what software acting a company's behalf is actually allowed to do.

## What This Means for AWS and Its Customers

The most strategically notable piece is the OpenAI partnership. By letting OpenAI's agents run entirely inside AWS, Amazon positions its cloud as neutral ground where customers can use a leading outside model without moving their data elsewhere. The limited-preview status is worth keeping in mind, however: access is restricted, and the terms of broader availability have not yet been settled.

Several of these launches are previews, meaning features, pricing, and performance can still change before general release. A consistent cold start of approximately two seconds is a useful benchmark, but real-world results will depend on how customers build and scale their agents.

Key developments to watch include whether Bedrock Managed Agents moves beyond limited preview, how quickly developers adopt the open-source Strands Box, and whether the Well-Architected Agent becomes a standard part of how AWS customers review their setups.
