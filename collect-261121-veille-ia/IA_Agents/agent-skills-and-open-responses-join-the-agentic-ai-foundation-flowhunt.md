---
id: collect-261121-veille-ia/ia-agents/agent-skills-and-open-responses-join-the-agentic-ai-foundation-flowhunt
title: "Agent Skills and Open Responses Join the Agentic AI Foundation | FlowHunt"
domain: ia-agents
role: reference
task: article
actors: []
dates: ["2026-08-20"]
keywords: ["agent", "skills", "foundation", "responses", "open", "agents", "agentic", "anthropic"]
source: https://www.flowhunt.io/blog/agentic-ai-foundation-agent-skills-open-responses/
source_anchor: ""
source_lines: [1, 56]
section: "IA_Agents"
---
### AI Agents Just Got a Shared Standard: A2A Joins MCP at the Linux Foundation

On August 20, 2026, Google donated its Agent2Agent protocol to the Linux Foundation's Agentic AI Foundation, placing it alongside Anthropic's MCP. Here's what c...

Agent Skills, Open Responses, OpenEnv and OpenSandbox joined the Agentic AI Foundation on Oct 22, 2026. What each does and what it means for agent builders.

Two months ago, this blog covered how **A2A joined MCP at the Linux Foundation**, putting Googleâs agent-to-agent protocol under the same neutral roof as Anthropicâs **Model Context Protocol**. On October 22, 2026, the Agentic AI Foundation (AAIF) took a much bigger step. At AGNTCon + MCPCon North America in San Jose, four more projects moved under its governance in a single announcement: Agent Skills from Anthropic, Open Responses from OpenAI, OpenEnv from Hugging Face, and OpenSandbox from Alibaba. The foundation launched with three projects in December 2025. It now hosts 10, and between them they cover nearly every layer an AI agent runs on, from the instructions it follows to the sandbox its code executes in.

Each of the four donations fills a gap the foundation didnât cover before:

| Project | Contributed by | What it standardizes | 
|---|---|---|
| Agent Skills | Anthropic | Packaged instructions, scripts, and resources that agents load on demand | 
| Open Responses | OpenAI | One request and streaming format for calling models from different providers | 
| OpenEnv | Hugging Face | Containerized reinforcement learning environments with an agent interface | 
| OpenSandbox | Alibaba | Isolated runtimes for code that agents generate and execute | 

They join the six projects already hosted: MCP, goose, AGENTS.md, A2A, Agent Router, and agentgateway. âA year ago, AAIF launched with three projects and a bet that agentic AI needed a neutral home to thrive,â said Manik Surtani, the foundationâs CTO and co-founder. âToday we host 10.â

Agent Skills is the donation most agent builders will notice first. A skill is a folder built around a SKILL.md file: a short description the agent always sees, plus fuller instructions, scripts, and reference files it loads only when a task calls for them. That progressive loading is the point. An agent can carry hundreds of skills without stuffing all of them into its context window, a trade-off we dug into when comparing **how AI agents load skills into their context** across 11 frameworks.

Anthropic didnât just hand over the spec. It donated the Agent Skills name, the GitHub organization, and the agentskills.io domain. âToday, weâre donating Agent Skills to the AAIF to ensure that anyone building with skills can shape whatâs next,â said David Soria Parra, co-creator and lead maintainer of MCP at Anthropic.

Skills now sit next to AGENTS.md, the plain-text convention for repo-level agent instructions that OpenAI contributed at the foundationâs launch. Together they cover the instruction layer: AGENTS.md for always-on context, skills for expertise loaded on demand. Neither is a guarantee of quality, though. When we **A/B tested a coding agentâs instructions**, a 33 KB instruction file bought nothing a short one didnât. A shared format makes skills portable, but it doesnât make them good.

Open Responses targets a quieter source of lock-in: the shape of the API call itself. Every model provider has its own request format, its own streaming events, and its own way of describing tools. Switching models usually means rewriting the code around them.

Open Responses is based on OpenAIâs Responses API and defines a shared schema, consistent streaming events, and extensible tooling, so one request format can run across providers. âAAIF has an amazing track record of boosting important projects across the industry,â said Steve Coffey of OpenAIâs technical staff. For teams that already route different tasks to different models, a neutral interface is the difference between switching a config value and rewriting an integration.

The other two projects deal with what happens around the agent rather than inside it.

**OpenSandbox** is Alibabaâs general-purpose sandbox platform for AI applications. It ships multi-language SDKs, unified sandbox APIs, and Docker and Kubernetes runtimes, plus a protocol for lifecycle and execution APIs that lets teams plug in custom runtimes. âWe donât want its future to be defined by a single company,â said Xiaobin Xu, head of development infrastructure at Alibaba. The timing is pointed. This summer brought reports of **AI agents breaking out of their test sandboxes** at OpenAI and Anthropic, and isolation is no longer something each team can afford to improvise.

**OpenEnv** is Hugging Faceâs framework and community hub for **reinforcement learning** environments. It packages each environment as a containerized service that exposes an agent interface, MCP, and the Gym training protocol. That means the same environment an agent trains in can speak the same protocol it uses in production. âWe believe, now more than ever, that democratizing AI requires a fully open stack,â said Ben Burtenshaw, machine learning engineer at Hugging Face.

FlowHunt's visual builder keeps your agents independent of any single model provider, and connects them to your systems through MCP.

The project count tells one part of the story. Adoption tells the other. According to figures the foundation published when it welcomed Agent Skills, the format is supported by more than 70 agent platforms, over 1 million skills have been published on the skills.sh registry, and skills have been installed nearly 280 million times. For comparison, MCP had passed 10,000 published servers and AGENTS.md had been adopted by more than 60,000 open source projects when the foundation launched.

Those numbers explain why the labs are donating at all. Once a format has that much adoption, keeping it proprietary carries more risk than benefit. A competitor can fork it, and customers start asking who controls the roadmap. Neutral governance answers that question before it slows anyone down.

A shared home doesnât make the projects work together automatically. Open Responses is a specification that providers still have to implement, and a common request format doesnât erase real differences in how models reason, call tools, or handle long context. Skills are portable across platforms that support them, but a skill written for one agentâs tool set may assume capabilities another agent doesnât have. And OpenSandbox gives teams a common way to isolate agent code, not a guarantee that any given configuration is safe.

There is also overlap to work out. MCP, A2A, Agent Skills, and Open Responses all touch how agents describe and call tools. Having them under one foundation makes it easier to align them, but it doesnât mean theyâre aligned yet.

For teams building agents today, the practical message is to treat the open formats as the default:

A **no-code AI agent platform** like FlowHunt sits on top of these layers. You pick the model per task, connect tools through MCP, and orchestrate agents on one canvas, so the workflows you build donât depend on any single vendorâs format winning.

In ten months, the Agentic AI Foundation went from three projects to 10, and the labs that compete hardest on models are now sharing governance of the plumbing underneath them. Anthropic gave up control of Agent Skills, OpenAI opened up the shape of its API, Hugging Face contributed its training environments, and Alibaba its sandboxes. None of that makes agents smarter on its own. What it does is make the agent stack less dependent on any one company, which is the condition every team building on it has been waiting for.

FlowHunt lets you swap models, connect tools through MCP, and orchestrate agents on one canvas, so your workflows keep running as the open agent stack settles.

In July 2026, OpenAI and Anthropic each disclosed that their own AI models broke out of isolated test environments and reached real organizations' systems. Here...

Explore the top AI agent builders in 2026, from no-code platforms to enterprise-grade frameworks. Discover which tools are best for your use case and how FlowHu...
