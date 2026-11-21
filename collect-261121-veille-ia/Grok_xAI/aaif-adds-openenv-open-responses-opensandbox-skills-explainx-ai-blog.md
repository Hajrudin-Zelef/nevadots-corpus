---
id: collect-261121-veille-ia/grok-xai/aaif-adds-openenv-open-responses-opensandbox-skills-explainx-ai-blog
title: "AAIF Adds OpenEnv, Open Responses, OpenSandbox, Skills | explainx.ai Blog"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: ["2026-10-22"]
keywords: ["open", "skills", "agent", "agents", "responses", "openenv", "aaif", "opensandbox"]
source: https://explainx.ai/blog/aaif-10-projects-openenv-open-responses-opensandbox-agent-skills-agntcon-2026
source_anchor: ""
source_lines: [1, 102]
section: "Grok_xAI"
---
The **Agentic AI Foundation (AAIF)**, the Linux Foundation project that started with three agent standards in December 2025, now hosts **10**. At AGNTCon + MCPCon North America in San Jose on October 22, 2026, it announced four additions: **Agent Skills** from Anthropic, **Open Responses** from OpenAI, **OpenEnv** from Hugging Face and **OpenSandbox** from Alibaba. Four companies, four layers of the stack, one neutral home. This post explains what each project does, what the governance change means, and what builders should do about it.

We covered the Anthropic side in Anthropic donates Agent Skills to the AAIF. This post covers the full set, with emphasis on the three that are less covered.

## TL;DR

| Project | Contributor | Layer | What it is | 
|---|---|---|---|
| Agent Skills | Anthropic | Capabilities | Open format for packaging instructions, scripts and resources that agents load on demand | 
| Open Responses | OpenAI | Interoperability | Open spec for multi-provider LLM interfaces, based on the Responses API | 
| OpenEnv | Hugging Face | Training and evaluation | Open framework and hub for RL environments | 
| OpenSandbox | Alibaba | Safe execution | General-purpose sandbox platform with SDKs and Docker and Kubernetes runtimes | 

According to the Linux Foundation press release, they join the Agent2Agent (A2A) protocol, Agent Router, agentgateway, AGENTS.md, goose and the Model Context Protocol (MCP). AAIF CTO and co-founder Manik Surtani said the foundation launched a year ago "with three projects and a bet that agentic AI needed a neutral home to thrive."

Agents that act on your behalf need rules. See how explainx helps secure your agents →

## The four layers

The release frames the new projects as addressing "how agents gain new skills, how applications talk to models across providers, how agents are trained and evaluated, and where agents run safely." That is a useful way to map your own stack.

### Agent Skills: how agents get capabilities

Skills package instructions, scripts and resources so an agent can load them when needed, making specialized know-how portable across agent products. Anthropic's David Soria Parra, co-creator and lead maintainer of MCP, said that "without the right context, even the most intelligent agents fall short," and that Anthropic is donating Agent Skills so anyone building with skills can help shape their future. If you build or use skills, see our complete guide to agent skills and the donation explainer.

### Open Responses: how apps talk to models

Open Responses is described as an open specification for multi-provider, interoperable LLM interfaces, based on the OpenAI Responses API. It defines a shared schema, consistent streaming events and extensible tooling so developers can "describe requests and outputs once and run them across model providers with minimal translation." OpenAI's Steve Coffey said Open Responses is "thrilled to be joining the AAIF." The practical promise is less provider lock-in in your client code. The open question is how closely other providers will follow the spec, which no press release can answer.

### OpenEnv: how agents are trained

OpenEnv is the one most relevant to anyone doing reinforcement learning. The release describes an open framework and community hub for RL environments that makes them "interoperable, plug-and-play, easy to build, easy to share, and scalable," through a simple containerized service exposing an agent interface, MCP and the Gym training protocol. Hugging Face's Ben Burtenshaw said "democratizing AI requires a fully open stack," and the release lists OpenEnv committee representatives from: Hugging Face, Microsoft, Reflection, Meta-PyTorch, Fleet, Mercor, RadixArk, Modal, Prime Intellect and more. Background: we explained the project's launch in Hugging Face puts RL environments on the Hub.

### OpenSandbox: where agents run

Alibaba's OpenSandbox offers multi-language SDKs, unified sandbox APIs and Docker and Kubernetes runtimes for coding agents, GUI agents, agent evaluation, code execution and RL training, plus a sandbox protocol defining lifecycle and execution APIs so teams can plug in custom runtimes. Xiaobin Xu of Alibaba said the company does not want the project's future "defined by a single company." For comparison with other approaches to agent isolation, see NVIDIA OpenShell and Sentry and our roundup of agent safety coverage.

## The foundation's first year, in a timeline

| When | What happened | 
|---|---|
| December 2025 | AAIF launches under the Linux Foundation with three projects: MCP, goose and AGENTS.md | 
| Between December 2025 and October 2026 | Agent2Agent (A2A) protocol, Agent Router and agentgateway are among the projects hosted (the release does not give join dates) | 
| October 22, 2026 | Agent Skills, Open Responses, OpenEnv and OpenSandbox are announced at AGNTCon + MCPCon North America, taking the count to 10 | 
| October 22-23, 2026 | The conference runs at the San Jose McEnery Convention Center on building, securing and operating agents | 

The pattern is that every few months another layer of the agent stack gets a neutral home. First came the tool-calling protocol (MCP) and the project instruction file (AGENTS.md), then inter-agent communication and routing, and now capabilities, model interfaces, training environments and sandboxes.

## A map of how the pieces fit

If you are building an agent product, the 10 projects roughly stack like this, based on the descriptions in the release.

1. **The model call.** Open Responses describes how your application sends requests and receives streaming events from any provider.
2. **The tools the model can use.** MCP connects agents to tools and data. AGENTS.md gives coding agents project-level instructions.
3. **Reusable know-how.** Agent Skills package instructions and scripts that load on demand.
4. **Agents talking to agents.** The A2A protocol covers communication between agents, and Agent Router and agentgateway deal with routing and gateway concerns.
5. **Where code runs.** OpenSandbox gives agent-generated code a contained runtime.
6. **How the agent got good.** OpenEnv standardizes the environments used to train and evaluate agents with reinforcement learning.

Read that way, the foundation is assembling most of an open agent platform from interchangeable parts, which is why it matters even if you never touch the code of any single project.

## Questions people are asking

- **Does this affect MCP?** Not directly. MCP was already an AAIF project and stays one.
- **Is Open Responses the same as the OpenAI Responses API?** No. It is an open specification based on that API. The point is that other providers can implement the same schema and streaming events without depending on one vendor's product.
- **Will OpenEnv environments run with my training library?** The release says environments expose the Gym training protocol that training libraries use, so the intent is yes, but check the specific library and version.
- **Is OpenSandbox a security guarantee?** No project name is. A sandbox is only as strong as its configuration, and you should test escape paths and egress rules yourself.
- **Can my company contribute?** The foundation says it wants one place for shared standards. Check the AAIF site for the process of joining or proposing projects.

## What neutral governance changes, and what it does not

Neutral governance means no single company controls the roadmap, trademarks and releases. It does not mean:

- **The code changes license.** The release says nothing about licenses, so check each repository's license file.
- **Contributors must join the foundation.** The release does not say membership is required to use the code; check each project's contribution rules.
- **Vendors stop competing.** Each company still ships its own products on top. OpenAI's Responses API, Anthropic's Claude features and Alibaba's cloud remain commercial.
- **Adoption is guaranteed.** A foundation can host a standard, but only builders and rival vendors make it a standard.

What it does change: a donated project can no longer be quietly redirected by its origin company, and other firms can invest with less fear of being locked out. MCP's move to a neutral home a year ago is the template.

## Why four contributors from four rivals matters

Anthropic, OpenAI, Hugging Face and Alibaba rarely appear on the same press release. Putting their projects under one umbrella signals that the plumbing of agents, skills, transport, sandboxes and training environments, is being treated as shared infrastructure while competition moves to models and products. It also puts a Chinese lab's project (OpenSandbox) alongside US ones, at a time when export controls and policy are a constant background issue for cross-border open source. We have followed that tension in coverage of Alibaba's OpenGCUA stack.

## What builders should do now

1. **Skills authors:** keep using the open format; watch the AAIF repositories for governance docs and contribution rules.
2. **Teams calling multiple model APIs:** evaluate Open Responses as an abstraction layer, but test each provider's actual behavior, since a spec is only as good as its implementations.
3. **RL and eval teams:** try OpenEnv environments through the Hub, and consider packaging your own environments in its containerized format so others can reuse them.
4. **Agent platform teams:** compare OpenSandbox with your current isolation approach, including network egress, file system scope and credential handling, before swapping anything in production.
5. **Everyone:** treat standards news as a reason to reduce coupling, not as a reason to rewrite working systems.

## What we do not know yet

- Technical steering structures and release cadence for each new project inside AAIF.
- Whether other major vendors, such as Google and Microsoft, will contribute their own standards beyond the A2A protocol already hosted.
- How quickly implementations of Open Responses appear outside OpenAI.
- Any security audits of OpenSandbox's isolation guarantees.

## Related reading

*Details are as announced on October 22, 2026 by the Linux Foundation and AAIF.*
