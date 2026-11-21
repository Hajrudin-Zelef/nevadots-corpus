---
id: collect-261121-veille-ia/gemini-google/antigravity-run-workflow-agent-teams-as-python-scripts-a-origotech
title: "Antigravity /run-workflow: agent teams as Python scripts Â· OrigoTech"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["antigravity", "agent", "copilot", "python", "script", "github", "workflows", "run-workflow"]
source: https://origotech.dev/articles/antigravity-dynamic-workflows-run-workflow-copilot-model-deprecations-codex-0-169/
source_anchor: ""
source_lines: [1, 88]
section: "Gemini_Google"
---
Agents

# Antigravity Dynamic Workflows: /run-workflow turns agent teams into Python scripts; Copilot drops six models on December 14, Codex CLI 0.169.0

Antigravity 2.32.0 (Nov 13) adds Dynamic Workflows, where the agent writes a short Python script that runs many subagents in parallel or in sequence, and you approve the script before it runs. GitHub put Gemini 4 Hydrogen in Copilot and will retire six Copilot models on December 14. Also: Codex CLI 0.169.0, Claude Code 2.1.325, Zed 1.28 and AirTrunk's 1.8 GW plan in India.

Google shipped **Antigravity 2.32.0** on November 13, 2026, and its main addition is **Dynamic Workflows**: instead of coordinating subagents turn by turn in a long conversation, the agent writes a short Python script that does the coordinating, shows it to you, and runs it in the background once you approve it (Antigravity changelog).

## How /run-workflow works

You type `/run-workflow` followed by a task, or ask for a workflow in your own words. According to the Dynamic Workflows documentation:

1. the agent reads a built-in `run-workflow` skill and writes a Python script for the task;
2. Antigravity shows the script and asks for approval, unless your permission preset already allows it (for example Turbo); an approval covers that exact script;
3. the workflow runs in the background, so the conversation stays usable, and each phase, log line and agent is shown live;
4. when it finishes, the main agent gets the logs and output and summarizes them.

The script runs agents concurrently with `parallel()` or one after another with `pipeline()`, grouped into named phases. Each agent works in its own conversation and must return a result that matches a JSON Schema, so file reads and test logs stay out of the main context. A workflow can keep one agent across turns, so the reviewer that found a bug can later check the fix. Agents use the conversation's model by default; the script can ask for a lighter tier (`flash_lite`, `flash`) for high-volume checks or `pro` for harder reasoning.

Scripts run on a Python runtime bundled with Antigravity, so no local install is needed; the `ANTIGRAVITY_PYTHON` variable points to another interpreter (Python 3.8 or later). A script that works can be saved in the project and run again. Availability: **Google AI Pro and Ultra** plans, in the Antigravity App and the Antigravity CLI. Google's docs now present Dynamic Workflows as the new workflow system and give a guide for turning old Markdown workflows into skills (migration guide).

Why it matters: Claude Code (Projects, subagents), Codex (side chats) and Antigravity itself (`/swarm`, added in 2.31.0 on November 9) all push toward many agents at once. Dynamic Workflows is the first of these where the plan is a file you can read, diff and re-run, rather than something the model improvises. We have not tested it.

Antigravity 2.32.0 also adds Everforest themes, keyboard shortcuts to jump between prompts, and a security change for the agent's browser: cross-site frames now load signed out and stay hidden from the agent's screenshots and page snapshots unless explicitly allowed (changelog).

## GitHub Copilot: Hydrogen in, six models out on December 14

**Gemini 4 Hydrogen** is generally available in GitHub Copilot since November 13 for Copilot Pro, Pro+, Max, Business and Enterprise, in VS Code, Visual Studio, Copilot CLI, the coding agent, the Copilot app, github.com, GitHub Mobile, JetBrains IDEs, Xcode, Eclipse and the Copilot SDK. It is billed at the provider's list price under usage-based billing, and the rollout is gradual (GitHub changelog).

The same day, GitHub announced that six models leave every Copilot experience on **December 14, 2026** (GitHub changelog):

| GitHub Copilot: Hydrogen in, six models out on December 14 |  | 
|---|---|
| Retired model | Suggested replacement | 
|---|---|
| Claude Fable 5 | Claude Fable 5.2 | 
| Claude Haiku 4.5 | Claude Haiku 5.5 | 
| Gemini 3.8 Flash | Gemini 4 Hydrogen | 
| Gemini 4 Argon | Gemini 4 Carbon | 
| Grok 4.6 | Grok 4.7 | 
| Kimi K3 | to be announced | 

Claude Haiku 4.5 stays available to individual subscribers on annual plans. For Business and Enterprise, the replacements are enabled automatically unless an administrator has turned off the global default. Teams that pin a model in scripts, Copilot CLI or the SDK have four weeks to switch.

## Codex CLI 0.169.0

OpenAI released **Codex CLI 0.169.0** on November 13 (08:37 UTC) (release notes):

- the opt-in terminal workspace now supports several side-chat tabs, which can be reopened after a restart;
- older tasks can be searched from the agent command center by name, prompt or folder;
- a **Collapse turn details** option folds finished activity under each prompt;
- custom model providers can be added or edited from the terminal, with device-code sign-in for some gateways over SSH;
- security fix: `codex exec` keeps project configuration and MCP servers disabled in untrusted folders, even with`--sandbox workspace-write` ;`--trust` saves project trust explicitly.

## Claude Code 2.1.325

Anthropic released **Claude Code v2.1.325** on November 13 (20:40 UTC). It is mostly fixes: a security fix in zstd response decoding, several permission fixes (a gap where some compound Bash commands could run without asking, a Bash allow rule that could run a different program than the one it names, relative deny rules such as `Read(./.env)` no longer covering the project after a `cd`), and fixes for Code Mode, which arrived in 2.1.323. In VS Code, the extension gains prompt suggestions accepted with Tab (release notes).

## Zed 1.28

Zed's stable channel moved to **1.28.1** on November 11. The 1.28 release improves compatibility with the latest Agent Client Protocol (the protocol Zed uses to run external agents such as Claude Code or Codex), adds a setting to hide the edit prediction button and truncates lines over 2,000 characters in the agent's `read_file` tool. On the Git side: optional automatic fetching (`git.auto_fetch.enabled`, off by default) and a resizable Refs column in the Git Graph (release notes).

## Infrastructure: AirTrunk plans 1.8 GW in Andhra Pradesh

AirTrunk, backed by Blackstone, signed a memorandum of understanding with the government of Andhra Pradesh, India, for two proposed data center campuses in Anakapalli district: **400 MW at Rambilli and 1,400 MW at Tootipala** (AirTrunk, Nov 16, 2026). Business Today puts the investment at about â¹1.03 lakh crore and stresses that it still needs approvals (Business Today). An MoU is a statement of intent, not a firm investment. AirTrunk says it favors low-water, air-cooled designs and would use industrial rather than municipal water.

## What we updated

- Claude Code page: latest version v2.1.325.
- Codex page: Codex CLI 0.169.0.
- GitHub Copilot page: Gemini 4 Hydrogen and the December 14 deprecations.
- Zed page: v1.28.1.
- New counter on the home page: AirTrunk's 1,800 MW planned in Andhra Pradesh.

## Sources

- Google Antigravity, changelog (v2.32.0, Nov 13, 2026)
- Google Antigravity, Dynamic Workflows (/run-workflow)
- Google Antigravity, Workflows to skills migration
- GitHub, Gemini 4 Hydrogen in GitHub Copilot (Nov 13, 2026)
- GitHub, Upcoming deprecation of selected GitHub Copilot models in December (Nov 13, 2026)
- OpenAI, Codex CLI 0.169.0 release notes (Nov 13, 2026)
- Anthropic, Claude Code v2.1.325 release notes (Nov 13, 2026)
- Zed, v1.28.1 release notes (Nov 11, 2026)
- AirTrunk, AirTrunk signs MoU for proposed data centre campuses in Andhra Pradesh (Nov 16, 2026)
- Business Today, AirTrunk plans â¹1.03 lakh crore investment in Andhra Pradesh (Nov 14, 2026)

Spotted an error in this article? Report it Â· Our corrections policy
