---
id: collect-261121-veille-ia/copilot-microsoft/github-copilot-added-model-policy-controls-while-open-source-assistants-reorganized-their-runtimes-a
title: "GitHub Copilot added model-policy controls while open-source assistants reorganized their runtimes. — AI Assistant Software Trends, Week of Oct 26, 2026"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-10-26"]
keywords: ["github", "copilot", "week", "added", "model", "controls", "agent", "lobechat"]
source: https://sparkpulse.io/briefs/ai-assistants/2026-10-26-github-copilot-added-model-policy-controls-while-open-source
source_anchor: ""
source_lines: [1, 29]
section: "Copilot_Microsoft"
---
## The week in ai-assistants

Enterprise control and agent orchestration were the two active fronts. **GitHub Copilot** shipped a REST API for model policies and moved agentic autofix for code scanning alerts to general availability, while **LobeChat** added goal batch rollouts and server-queued follow-ups. The assistants are splitting between governance for large organizations and workspaces for agent work.

Underneath, the open-source runtimes were reorganizing. **opencode** promoted its V2 codebase to the main dev branch, and **Ollama** moved to v0.40.7 candidates with packaging cleanup.

## Leaders

**GitHub Copilot** had the strongest week. On October 19 GitHub added REST endpoints to read and set Copilot model policies for enterprises, teams, apps and organizations, and deprecated a batch of older models across Chat, agent mode and completions. Agentic autofix for code scanning alerts reached general availability on October 15, and GitHub in Slack and Teams gained multi-repository support.

**LobeChat**, now shipping as LobeHub, released v2.2.20 stable with device model relay, so server-side agent runs can use models that exist only on a user's own browser or device, plus goal batch rollouts with an owner review gate. The v2.2.21 canaries added server-queued steer follow-ups, scheduled tasks from their own tab and clearer MCP stdio errors.

**opencode** retired its separate beta repository. The structural moves came the week before, when the V2 tree was promoted to the dev branch and the VS Code extension reached 1.0 by launching the V2 CLI.

## Wildcards

**Ollama** is in packaging work: CUDA libraries shared by the GGML and MLX backends now ship once, in the CUDA payload. Earlier candidates fixed model pulls behind proxies and added Nemotron 3.5 Super on MLX.

**Goose** appears only as automated GDK v0.1.0 alpha tags with no notes, the latest on October 24. The feed shows cadence but not content.

## Themes that compounded

- Policy and review gates appeared in **GitHub Copilot** (model policies) and**LobeChat** (owner review for batch rollouts).
- Deprecations accompanied launches at **GitHub Copilot** , which retired older models the same day it shipped controls.
- Runtime restructuring continued at **opencode** and**Ollama** .

## Watch this week

**GitHub Copilot** ships on a weekly-or-faster cadence and paired new controls with deprecations, so more model changes are likely. **LobeChat** has v2.2.21 canaries in flight, making a stable v2.2.21 the next step. For **Ollama**, a final v0.40.7 would follow the release candidates.
