---
id: collect-261121-veille-ia/outils-agents/agent-skills-moves-to-the-agentic-ai-foundation-what-developers-need-to-know
title: "Agent Skills Moves to the Agentic AI Foundation: What Developers Need to Know"
domain: outils-agents
role: reference
task: article
actors: []
dates: ["2026-10-22"]
keywords: ["agent", "skills", "foundation", "anthropic", "skill", "agentic", "specification", "developers"]
source: https://dev.to/tokely/agent-skills-moves-to-the-agentic-ai-foundation-what-developers-need-to-know-1g90
source_anchor: ""
source_lines: [1, 94]
section: "Outils_Agents"
---
**Short answer:** Anthropic donated the Agent Skills standard to the Agentic AI Foundation on October 22, 2026. This transfer places the skill specification under neutral Linux Foundation governance alongside the Model Context Protocol. Developers can build skills using simple local directories, Markdown instructions, and executable scripts, then run them across dozens of supported agent clients and model backends without encountering platform lock-in.

On October 22, 2026, Anthropic transferred the Agent Skills standard and all related assets to the Agentic AI Foundation. The donation includes the core specification, the Agent Skills name and logo, the official GitHub organization, and the agentskills.io domain. Anthropic stated that it is committed to keeping Agent Skills open, community-driven, and vendor-neutral.

The Agentic AI Foundation operates as a project under the Linux Foundation to host vendor-neutral agent standards. Anthropic, Block, and OpenAI co-founded the foundation in 2025 to create a shared home for open agent infrastructure. The foundation already hosts the Model Context Protocol, which Anthropic donated in an earlier governance transfer.

Anthropic first launched Agent Skills inside its Claude products in October 2025. Two months later, in December 2025, Anthropic published the underlying format as an open standard for the developer community. Donating the assets to the Agentic AI Foundation completes the transition from a single-vendor feature to a neutral industry specification.

Two Lead Maintainers, David Soria Parra and Mahesh Murag, now guide the Agent Skills project. Individual software engineers from Anthropic, Google, OpenAI, and other technology companies maintain the specification on their own behalf rather than through corporate representation.

Development teams do not need to join the Agentic AI Foundation to use the standard or contribute code to the repository.

Anyone who wants to suggest changes to the specification can send feedback through the project's Discord server.

More than 70 agent platforms have integrated the Agent Skills specification across their production tools. That adoption footprint includes major coding agents and developer environments such as ChatGPT, Cursor, GitHub Copilot, and VS Code.

According to Anthropic, developers have published 1 million skills and end users have installed nearly 280 million skills across those integrated platforms. These are Anthropic's own figures and are not independently audited.

For skill authors, that means one package format across competing platforms. Developers who author a skill can deploy that package directly to multiple editors and command-line agents without rewriting instructions for each client.

A skill is a directory containing at minimum a SKILL.md file with YAML frontmatter followed by standard Markdown instructions. The directory can also house optional subdirectories by convention, including scripts for executable code, references for documentation loaded on demand, and assets for templates, images, or data files.


```
api-cost-check/
├── SKILL.md
├── scripts/
│   └── estimate.py
└── references/
    └── prices.md
```
File references inside SKILL.md should be relative paths from the skill root, kept one level deep. The specification advises against long chains of nested references.

The YAML frontmatter defines two required fields: name and description. The name field takes a string between 1 and 64 characters containing only lowercase letters, digits, and single hyphens. It must match the parent directory name exactly, with no leading hyphens, trailing hyphens, or double hyphens.

The description field accepts 1 to 1,024 characters explaining what the skill accomplishes and when the agent should select it. Authors should write clear descriptions with relevant keywords, because the agent runtime inspects this text to decide which skill fits a given user prompt.

Frontmatter also supports four optional fields. The license field names the distribution license, while compatibility accepts up to 500 characters outlining environment requirements. Authors can provide arbitrary string key-value pairs through the metadata map, or specify an experimental allowed-tools list containing space-separated pre-approved tools.


```
---
name: api-cost-check
description: "Estimates the API cost of a prompt or batch job before it runs. Use when the user asks what a request, batch or agent run will cost on a given model."
license: Apache-2.0
metadata:
  author: example-org
  version: "1.0"
---
# API cost check
1. Count input tokens and estimate output tokens.
2. Read per-1M-token prices from references/prices.md.
3. Run scripts/estimate.py and report the cost per request and per 1,000 requests.
```
Developers can verify their directory structure and schema formatting before distributing a package. Running skills-ref validate ./my-skill through the skills-ref reference library checks that the frontmatter is valid and follows the naming rules.

Roughly 100 tokens of metadata per skill enter the context window when an agent session initializes. The runtime loads only the name and description fields at startup, allowing an agent to index dozens of capabilities without consuming substantial context space.

The agent defers reading the rest of the package until the user prompt triggers that specific skill. The full body of SKILL.md loads into the active prompt only upon activation, and the specification recommends keeping that body under 5,000 tokens and under 500 lines of text.

Files located inside the scripts, references, and assets subdirectories remain unloaded until the activated instructions specifically call for them. This phased approach prevents large documentation files and scripts from bloating context during unrelated prompts.

Fifty installed skills add about 5,000 tokens of metadata at session start. At an input price of $2 per 1M tokens before prompt caching, loading metadata for all 50 skills costs roughly $0.01 per session.

The Model Context Protocol connects an agent to external tools and live data stores through client-server connections. In contrast, an Agent Skill packages procedural instructions, executable scripts, and static reference files that an agent runtime reads directly from disk.

MCP relies on active server processes that expose tool definitions, resources, and prompt templates over standard communication protocols. A skill functions as a static file bundle that informs the model how to complete a specific task using tools already available on the host.

Both standards now operate under the Agentic AI Foundation as complementary building blocks. A skill can direct an agent on when and how to call an MCP server, while the MCP server handles the protocol communication with remote systems.

The proposed starting roadmap for Agent Skills focuses on three areas: versioned releases, a formal specification proposal process, and security guidance for authors and users. A structured release cycle will help tool builders maintain compatibility as the format matures under foundation governance.

Security guidance represents an immediate priority because a skill can include arbitrary code in its scripts directory. When a skill's instructions tell the agent to run those scripts, they run with whatever access the agent has on that machine.

Installing a skill therefore means trusting the author who packaged it. Malicious or poorly tested scripts can modify local files, execute dangerous shell commands, or expose sensitive project data. Developers should always review the contents of the scripts directory before installing third-party skills into their environments.

Claude Code reads personal skills from ~/.claude/skills//SKILL.md and project-level skills from .claude/skills//SKILL.md inside the root of a repository.

Skills are plain text files evaluated by the agent client rather than the upstream model provider. Because the client parses the frontmatter and passes the instructions to the model, the same skill files work whichever model the client calls, although results still depend on the model.

Developers can run Claude Code through the Tokely API gateway by setting two environment variables. The skill directories stay exactly as they are.


```
skills-ref validate ./api-cost-check
ANTHROPIC_BASE_URL=https://api.tokely.me ANTHROPIC_AUTH_TOKEN='<YOUR_TOKELY_API_KEY>' claude
```
A single Tokely API key and unified balance grants access to Claude, GPT, Gemini, and Kimi models, including Kimi K3.1 Pro. The gateway exposes an Anthropic-compatible endpoint for Claude Code and an OpenAI-compatible endpoint for other agent clients, so a team can test the same skill on several models from one account. You can view all supported options on the Tokely models page or read our guide on using Claude Code with a cheaper API.

Read the announcement details at Anthropic's announcement and explore technical definitions in the Agent Skills specification. Further context appears in Simon Willison's note and our setup tutorial for Claude Code with a cheaper API.

*Originally published at tokely.me.*

## Top comments (0)
