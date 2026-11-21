---
id: collect-261121-veille-ia/gpt-openai/anthropic-openai-hugging-face-and-alibaba-hand-agent-skills-open-responses-openenv-and-opensandbox-t
title: "Anthropic, OpenAI, Hugging Face and Alibaba Hand Agent Skills, Open Responses, OpenEnv and OpenSandbox to a Neutral Foundation | Writeble"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: ["2026-10-23"]
keywords: ["agent", "open", "skills", "anthropic", "responses", "hugging", "face", "openenv"]
source: https://www.writeble.com/2026/10/23/agentic-ai-foundation-four-new-projects/
source_anchor: ""
source_lines: [1, 39]
section: "GPT_OpenAI"
---
**Four companies that usually compete on agent tooling have each handed one of their projects to the same neutral home.** On October 22, 2026, the Agentic AI Foundation (AAIF) announced that it now hosts Agent Skills from Anthropic, Open Responses from OpenAI, OpenEnv from Hugging Face and OpenSandbox from Alibaba. The news came on the first day of the foundationâs North American conference at the San Jose McEnery Convention Center.

The additions take AAIF from the three projects it launched with in December 2025 to 10. Each of the four covers a different layer of how agents are built: how they pick up new skills, how applications talk to models from different providers, how agents are trained and evaluated, and where they run code safely.

Agent Skills is an open format for giving an agent new capabilities and domain knowledge. A skill bundles instructions, scripts and resources into a reusable package that an agent loads only when it needs it. The aim is that specialist know-how written once can travel between agent products and platforms instead of being rewritten for each one.

Anthropic launched the format about a year ago. David Soria Parra, who spoke for Anthropic in the announcement, said the company was donating it âto ensure that anyone building with skills can shape whatâs next.â The format already has users outside Anthropic: TanStack AI, for example, now loads `SKILL.md` skills into any tool-calling model.

Open Responses is an open specification for calling language models through a single interface. It is based on OpenAIâs Responses API and defines a shared schema, consistent streaming events and extensible tooling. Developers describe a request and its output once and run it against different providers with little translation.

Steve Coffey of OpenAIâs technical staff said the projectâs mission is âhelping developers build great products on todayâs language models through simple, clean, and unified interfaces.â AWS said on the same day that it employs a core maintainer on the project and that Amazon Bedrock already supports the Responses API across dozens of models.

OpenEnv, from Hugging Face, is a framework and community hub for reinforcement-learning environments, the simulated settings where agents practise tasks and get scored. Each environment runs as a containerized service that exposes a familiar agent-facing interface on one side and Gym, the interface training libraries already use, on the other. The goal is environments that are easy to build and share and that plug into different training stacks without custom glue.

Ben Burtenshaw, a machine learning engineer at Hugging Face, said the committee behind OpenEnv includes people from Hugging Face, Microsoft, Reflection, Metaâs PyTorch team, Fleet, Mercor, RadixArk, Modal and Prime Intellect. âDemocratizing AI requires a fully open stack,â he said, calling reinforcement-learning environments âa fundamental part of that stack.â

OpenSandbox is a general-purpose sandbox platform from Alibaba for running agent workloads away from the host machine. Coding agents, GUI agents, agent evaluations, AI-generated code and reinforcement-learning training runs are among the use cases it lists. It ships SDKs in several languages and supports Docker and Kubernetes runtimes. The project is Apache-2.0 licensed.

At its centre is a sandbox protocol: a set of OpenAPI specifications with a lifecycle API to create, inspect, pause, resume, renew, snapshot and delete a sandbox, and an execution API to run commands and code, handle files, open sessions and collect resource measurements. An agent asks for a sandbox with a given configuration, and the configured runtime turns that into a container or a Kubernetes workload. Teams can add their own runtimes behind the same interface.

âWhile OpenSandbox was created at Alibaba, we donât want its future to be defined by a single company,â said Xiaobin Xu, Alibabaâs head of development infrastructure.

The four newcomers join six existing AAIF projects: the Agent2Agent (A2A) protocol, Agent Router, agentgateway, AGENTS.md, goose and the Model Context Protocol. All of them run under the same open, vendor-neutral governance.

âFour different companies chose to contribute them to neutral governance,â said Manik Surtani, AAIFâs CTO and co-founder. He described the foundationâs job as giving the community âone place to build shared standards, protocols, and infrastructure, with no single company controlling the outcome.â

For developers, the practical change is about who decides what comes next. The specifications and code stay where they are, but future changes to the skill format, the cross-provider API, the environment interface and the sandbox protocol now go through the foundationâs process rather than through a single vendorâs roadmap.

- #Agentic AI Foundation
- #Agent Skills
- #Open Responses
- #OpenEnv
- #OpenSandbox

Author

Contributor

AI Systems Architect is a seasoned technology leader with over 15 years of experience in the IT industry working with Fortune 500 companies. With a solid foundation in multi-agent systems, open-source LLM infrastructure, and enterprise deployment, he excels at building scalable production-grade AI platforms.
