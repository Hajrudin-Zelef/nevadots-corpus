---
id: collect-261121-veille-ia/deepseek/deepseek-harness-guide-2026
title: "DeepSeek Harness Guide (2026)"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: ["2026-08-13"]
keywords: ["deepseek", "harness", "agent", "plugin", "model", "code", "cordis", "tool"]
source: https://rpabotsworld.com/deepseek-harness-open-source-agent-framework-guide/
source_anchor: ""
source_lines: [1, 219]
section: "DeepSeek"
---
## What Is DeepSeek Harness and Why Does It Matter?

On August 13, 2026, DeepSeek AI quietly published a GitHub repository labeled “developer preview.” Within 48 hours, it had crossed 95,000 stars — breaking GitHub’s record for fastest-starred project. By early September 2026, it sits at over 210,000 stars with 24,500 forks. The project is DeepSeek Harness (`dsh`), and its tagline is deceptively simple: **“Everything is a Plugin.”**

But DeepSeek Harness is not just another coding agent or CLI wrapper. It is a fully open-source, model-agnostic **agent runtime** — a harness that treats every component of the agent stack, from the model adapter to the tool registry to the agent loop itself, as a swappable plugin. Where Claude Code and OpenAI Codex ship as polished, vendor-locked products, DeepSeek Harness ships a composable *frame* and expects you to assemble the walls.

For Agentic AI Architects, RPA engineers exploring AI-augmented automation, and platform teams evaluating open-source agent infrastructure, this distinction matters. The harness — the runtime layer that manages context, tool calls, retries, sandboxing, and the agent control loop — is emerging as a critical differentiator in agent performance. Recent benchmarks from Composio show that the same model routed through different harnesses can produce wildly different task-completion rates, latencies, and costs. Choosing your harness now matters almost as much as choosing your model.

This guide breaks down DeepSeek Harness’s architecture, explains what the “everything-is-a-plugin” philosophy means in practice, compares it head-to-head against Claude Code and OpenAI Codex, walks through getting started, and assesses where it fits in enterprise and RPA-adjacent automation stacks.

## The Architecture: Cordis and Spatiotemporal Composability

Most agent frameworks bolt capabilities onto a monolithic core. DeepSeek Harness inverts this: **there is no privileged core**. The entire system is built on Cordis, an open-source JavaScript meta-framework whose design is formalized in the academic paper *A Programming Paradigm for Spatiotemporal Composability*.

Cordis did not originate with DeepSeek Harness. It has four years of production history powering the Koishi chatbot framework. What DeepSeek did was recognize that its plugin composition model — where components can be added, removed, or replaced at runtime without teardown — maps perfectly onto the agent problem.

### What “Everything Is a Plugin” Actually Means

In DeepSeek Harness, each of the following is a discrete, independently replaceable plugin:

- **Model adapter** — the inference layer that talks to an LLM provider (Anthropic, OpenAI, DeepSeek, AWS Bedrock, Azure, Google)
- **Tool registry** — how tools are discovered, validated, and called
- **Session log** — conversation state, history, and context windowing
- **Agent loop** — the ReAct/reflection/planning cycle itself
- **Execution sandbox** — where code runs (Docker, local shell, remote VM)
- **Storage backend** — where sessions and artifacts persist
- **Web UI** — the frontend (ships a default, but it’s a plugin too)
- **MCP bridges** — Model Context Protocol connectors for external tool servers

Plugins contribute typed services, events, and reversible effects to a shared Cordis context. A developer can swap the model adapter from DeepSeek V4-Pro to Claude Fable 5.1, or replace the default agent loop with a custom multi-step planner, purely through configuration — without forking the source code.

This is architecturally different from, say, LangChain (which uses a chain-of-invocations pattern) or CrewAI (which models agents as role-based collaborators). DeepSeek Harness’s composition unit is the *plugin*, not the chain or the agent persona.

### The Plugin Ecosystem

As of September 2026, the ecosystem includes 110+ curated Cordis plugins spanning core orchestration, AST-based code analysis, Docker sandboxes, Kafka streaming pipelines, and MCP bridges. Community plugins are discoverable via the `dsh-plugin` GitHub topic.

For RPA teams, the MCP bridge plugins are particularly interesting. They allow DeepSeek Harness to connect to any MCP-compatible tool server — the same protocol that powers tool integration in Claude, Cursor, and other AI-native IDEs. This means a single DeepSeek Harness instance can orchestrate browser automation tools, database connectors, API gateways, and A2A-protocol agents through a unified plugin interface.

## DeepSeek Harness vs Claude Code vs OpenAI Codex: The Harness Matters

The 2026 coding-agent market has consolidated around three primary harnesses: Claude Code (Anthropic), OpenAI Codex (OpenAI), and now DeepSeek Harness (open-source). Each represents a fundamentally different philosophy.

| Dimension | DeepSeek Harness ( `dsh` ) | Claude Code | OpenAI Codex | 
|---|---|---|---|
| **License** | MIT (fully open-source) | Proprietary | Proprietary | 
| **Model lock-in** | Model-agnostic (any provider) | Anthropic models only | OpenAI models only | 
| **Architecture** | Plugin-first, everything swappable | Fixed runtime, closed internals | Fixed runtime, closed internals | 
| **Self-hosting** | Yes (local, on-prem, cloud) | No (Anthropic-hosted) | No (OpenAI-hosted) | 
| **Language** | TypeScript / Node.js | TypeScript (internal) | Python/TypeScript (internal) | 
| **MCP support** | Native via bridge plugins | Native | Native | 
| **Sub-agent delegation** | Can call Claude Code or Codex as sub-agents | No external sub-agents | No external sub-agents | 
| **Status (Sep 2026)** | Developer preview (breaking changes expected) | Generally available | Generally available | 

### Benchmark Reality Check

When Composio ran DeepSeek V4-Pro through several harnesses on 30 standardized agentic tasks, the results revealed that the harness — not the model — drove significant performance variation:

- **Pi harness** : 21/30 tasks completed
- **DeepSeek Harness** : 20/30 tasks
- **Claude Code** (same model): 19/30 tasks

However, Claude Code finished in roughly **half the median time** and used 90 fewer tool calls. Codex took longer and made more calls but cost around **58% less**. The takeaway for architects: raw task completion, latency, and cost per task are three separate optimization axes, and the harness mediates all three.

When each harness runs its *native* model (Claude Fable 5.1 on Claude Code, DeepSeek V4-Pro on dsh), performance gaps shift again. The point is not which wins on a leaderboard — it’s that **harness selection is now a first-class architectural decision**, on par with model selection.

### The Sub-Agent Trick

One of DeepSeek Harness’s most distinctive features is its **sub-agent provider system**. It ships plugins that can delegate tasks directly to Claude Code and OpenAI Codex by resolving their binaries from the system PATH. This means you can run dsh as the orchestration layer on a server while using Claude Code as the interactive agent in your IDE — or call Codex for specific tasks where GPT excels (e.g., rapid prototyping) while routing complex reasoning tasks to Anthropic’s models.

No other harness currently offers this level of cross-vendor delegation.

## Getting Started: From Zero to Agent in Five Minutes

DeepSeek Harness requires Node.js. Installation is a single command:

`npx @deepseek-ai/dsh web`
This starts the Web UI at `http://127.0.0.1:3080` and opens it in your default browser. The `web` command launches DeepSeek Harness in its full-featured web mode; for terminal-only workflows, replace `web` with `cli`.

### Running from Source

```
git clone https://github.com/deepseek-ai/deepseek-harness.git
cd deepseek-harness
pnpm install
pnpm run build
pnpm dsh web
```
### Configuring a Non-Default Model Provider

To route through Anthropic instead of DeepSeek’s own API, configure a provider plugin in your `dsh` config:

```
{
  "providers": {
    "anthropic": {
      "apiKey": "sk-ant-...",
      "model": "claude-fable-5-1"
    }
  }
}
```
The same pattern works for OpenAI, AWS Bedrock, Azure, and Google Vertex AI. The model adapter plugin handles provider-specific request formatting, authentication, and streaming behavior — you configure, it adapts.

### Installing Plugins

Plugins follow a declarative install pattern. From the dsh CLI:

```
dsh plugin install @dsh/docker-sandbox
dsh plugin install @dsh/mcp-bridge
dsh plugin install @dsh/ast-analyzer
```
Each plugin registers its services (tools, events, lifecycle hooks) into the Cordis context automatically. No code wiring required.

## Why This Matters for RPA and Automation Teams

If you work in enterprise RPA — building bots with UiPath, Automation Anywhere, or Blue Prism — DeepSeek Harness might seem like a different world. It is not. The convergence between RPA and agentic AI is accelerating, and understanding the harness layer is becoming essential for several reasons:

### 1. The Harness Is the New Orchestrator

Traditional RPA uses an orchestrator (UiPath Orchestrator, AA Control Room) to schedule, monitor, and manage bot execution. In the agentic world, the *harness* fills this role — managing the agent loop, tool access, context persistence, error recovery, and sandbox isolation. DeepSeek Harness’s plugin architecture means these orchestration concerns are modular and replaceable, much like how UiPath’s Autopilot is evolving from a bot assistant into a coding agent.

### 2. MCP Bridges Enable Hybrid Architectures

The MCP bridge plugins in DeepSeek Harness allow it to connect to any MCP-compatible tool server. For RPA teams, this opens a practical integration path: expose your existing RPA workflows as MCP tools, and an agentic harness can orchestrate them alongside API calls, browser automation, and LLM reasoning. This is the same integration model that the Agentic AI Foundation is standardizing with MCP and A2A.

### 3. Self-Hosting Is an Enterprise Requirement

Many RPA deployments operate under strict data residency, compliance, and air-gapped network requirements. Claude Code and Codex are cloud-hosted services. DeepSeek Harness can run entirely on-premises — local Node.js process, Docker sandbox, self-hosted model endpoint — with no data leaving the network boundary. For regulated industries (banking, healthcare, government), this is not a nice-to-have; it is a deployment prerequisite.

### 4. Model Agnosticism Reduces Vendor Risk

Enterprise automation teams learned painful lessons about platform lock-in during the first RPA wave. DeepSeek Harness’s model-agnostic architecture lets you swap providers without rewriting agent logic. Start with DeepSeek V4-Pro for cost efficiency, switch to Claude Fable 5.1 for complex reasoning tasks, or route to a fine-tuned open-weight model for domain-specific automation — all through configuration, not code changes.

## Production Considerations: What the Developer Preview Gets Right (and Wrong)

DeepSeek Harness is explicitly labeled as a **developer preview**. The README states in bold: “THERE WILL BE COMPATIBILITY-BREAKING CHANGES.” Teams evaluating it for production should understand what this means.

### What Works Today

- **Plugin composition** — the core Cordis runtime is mature (4 years in production on the Koishi platform) and handles plugin lifecycle management reliably.
- **Multi-provider routing** — switching between model providers is genuinely configuration-only, and the sub-agent delegation to Claude Code/Codex works.
- **Web UI** — the default web interface is functional and sufficient for development and light team use.
- **MCP integration** — bridge plugins connect to the growing MCP ecosystem with reasonable stability.
- **Community velocity** — 210k stars and 24.5k forks in three weeks means a large contributor base is forming, plugin development is active, and bugs surface (and get fixed) quickly.

### What Does Not Work Yet

- **API stability** — plugin interfaces, configuration schemas, and CLI commands are actively changing between versions. Pinning to a specific commit is the only safe strategy for anything beyond experimentation.
- **Enterprise features** — audit logging, role-based access control, multi-tenant isolation, and compliance tooling are either missing or community-contributed and unvetted.
- **Documentation depth** — the architecture docs and README are solid but the plugin authoring guide is still sparse. Expect to read source code.
- **Windows support** — primary development targets Linux and macOS; Windows support exists but edge cases surface regularly in GitHub issues.

### When to Adopt

| Use Case | Readiness | 
|---|---|
| Personal coding agent / developer productivity | Ready now | 
| Internal team experiments and hackathons | Ready now | 
| Evaluating harness architecture for future platform decisions | Ready now | 
| Custom agent for a specific internal workflow (non-critical) | Cautious yes — pin your version | 
| Customer-facing production agent | Wait for stable API | 
| Regulated-industry production deployment | Wait for enterprise features | 

## The Bigger Picture: Why the Harness Layer Is Emerging

DeepSeek Harness is not an isolated phenomenon. It arrives in a month when Anthropic released Claude Fable 5.1 with 75% cheaper cached context, explicitly pricing for long-running agents. Microsoft shipped Agent Framework 1.0 and Foundry Agent Service. The Agentic AI Foundation admitted A2A alongside MCP as co-equal protocols.

All of these developments point to the same architectural insight: **the model is becoming a commodity; the orchestration layer is becoming the product**. Models compete on intelligence and price. Harnesses compete on how efficiently they put that intelligence to work — managing context windows, orchestrating tool calls, handling failures, enforcing security boundaries, and persisting state across sessions.

For the RPA and automation community, this mirrors a pattern we lived through a decade ago. In 2015, the “robot” (the bot runtime) was the product. By 2020, the orchestrator and the platform around it — process mining, test suites, governance dashboards — determined enterprise value. The same shift is happening in agentic AI, but compressed into months rather than years.

DeepSeek Harness’s contribution is proving that this orchestration layer can be **open-source, composable, and model-agnostic** without sacrificing capability. Whether dsh itself becomes the standard or simply accelerates the category, it has already changed how architects think about agent infrastructure.

## Frequently Asked Questions

### Is DeepSeek Harness free to use commercially?

Yes. DeepSeek Harness is released under the MIT license, which permits commercial use, modification, and distribution without restriction. You still need to pay for inference from whichever model provider you configure (DeepSeek, Anthropic, OpenAI, etc.), but the harness runtime itself is completely free.

### Can I use DeepSeek Harness with Claude or GPT models instead of DeepSeek?

Yes. The model adapter is a plugin, so you can configure Anthropic (Claude Fable 5.1, Opus 5, Sonnet 5), OpenAI (GPT-5.6 Sol, GPT-5.4), AWS Bedrock, Azure, or Google Vertex AI as your inference provider. You can even use the sub-agent providers to delegate specific tasks to Claude Code or OpenAI Codex directly.

### How does DeepSeek Harness compare to LangChain or CrewAI?

LangChain is a chain-of-invocations framework focused on composing LLM calls with tools. CrewAI models agents as role-based collaborators. DeepSeek Harness is a full agent *runtime* — it manages the complete lifecycle including the agent loop, sandboxing, session persistence, and UI, with every component swappable via plugins. It operates at a lower infrastructure level than LangChain or CrewAI, closer to what Claude Code and Codex provide but fully open-source.

### Is DeepSeek Harness production-ready?

Not for customer-facing or regulated workloads. It is in developer preview with expected breaking changes. The underlying Cordis runtime is mature (4 years of production use), but the harness-specific plugin APIs and configuration schemas are still stabilizing. It is suitable for personal use, team experiments, and internal non-critical workflows today.

### Can RPA bots be integrated with DeepSeek Harness?

Yes, through MCP bridge plugins. If your RPA platform exposes workflows as MCP-compatible tool servers (or you wrap them in an MCP adapter), DeepSeek Harness can orchestrate them alongside other tools. This is the same integration pattern being standardized by the Agentic AI Foundation’s MCP and A2A protocols.

## Key Takeaways

- **DeepSeek Harness** (`dsh` ) is an open-source, MIT-licensed agent runtime that has crossed 210,000 GitHub stars in three weeks, breaking the platform’s fastest-star record.
- Its **“everything-is-a-plugin” architecture** , built on the Cordis meta-framework, lets you swap the model adapter, agent loop, tool registry, sandbox, and even the UI without changing source code.
- It is **model-agnostic** — supports Anthropic, OpenAI, DeepSeek, AWS Bedrock, Azure, and Google out of the box, and can delegate sub-tasks to Claude Code or Codex.
- **Benchmarks show the harness layer matters** — the same model routed through different harnesses produces different task-completion rates, latencies, and costs.
- For **RPA and automation teams** , DeepSeek Harness demonstrates that the orchestration layer (not the model) is becoming the critical architectural decision — mirroring the orchestrator-over-bot shift that defined enterprise RPA maturity.
- It is in **developer preview** — suitable for experiments and internal tools now, but enterprise production deployments should wait for API stabilization and governance features.
- **110+ curated plugins** and native MCP bridge support make it a strong candidate for hybrid agentic-AI + RPA architectures.

## References

1. DeepSeek AI. “DeepSeek Harness: Everything is a Plugin.” GitHub Repository, August 2026. https://github.com/deepseek-ai/deepseek-harness
2. Lardinois, Frederic. “DeepSeek open sources an agent harness where everything is a plugin.” *The New Stack* , August 13, 2026. https://thenewstack.io/deepseek-harness-open-source-plugins/
3. Pillitteri, Pasquale. “DeepSeek Harness Breaks GitHub’s Fastest Star Record.” August 2026. https://pasqualepillitteri.it/en/news/11573/deepseek-harness-fastest-github-stars-record
4. Composio. “DeepSeek Harness vs Claude Code in 2026: Open Runtime vs the Deepest Closed Harness.” https://composio.dev/content/deepseek-harness-vs-claude-code
5. MindStudio. “DeepSeek Harness vs Claude Code vs Codex: Which Coding Agent Wins?” https://www.mindstudio.ai/blog/deepseek-harness-vs-claude-code-codex
6. Habr. “Inside DeepSeek Harness: Cordis, Session Events, Tool Pipelines, and Permission Boundaries.” https://habr.com/en/articles/1070958/
7. Franzen, Carl. “Anthropic’s Claude Fable 5.1 and Mythos 5.1 arrive with a 75% cost reduction for Fable cache reads.” *VentureBeat* , September 1, 2026. https://venturebeat.com/technology/anthropics-claude-fable-5-1-and-mythos-5-1-arrive-with-a-75-cost-reduction-for-fable-cache-reads
8. Cordiverse. “Cordis: A Programming Paradigm for Spatiotemporal Composability.” arXiv:2608.25512. https://arxiv.org/abs/2608.25512
9. Flowtivity. “DeepSeek Harness: Why 95,000 GitHub Stars in 2 Days Matters.” https://flowtivity.ai/blog/deepseek-harness-open-source-agent-explained/
10. AlphaMatch AI. “DeepSeek Harness: Revolutionizing AI Agent Development with ‘Everything is a Plugin.'” https://www.alphamatch.ai/blog/deepseek-harness-everything-is-a-plugin-2026
