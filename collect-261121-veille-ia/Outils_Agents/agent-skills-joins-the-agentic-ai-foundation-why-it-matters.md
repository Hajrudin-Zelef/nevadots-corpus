---
id: collect-261121-veille-ia/outils-agents/agent-skills-joins-the-agentic-ai-foundation-why-it-matters
title: "Agent Skills Joins the Agentic AI Foundation: Why It Matters"
domain: outils-agents
role: reference
task: article
actors: []
dates: []
keywords: ["agent", "foundation", "skills", "anthropic", "standard", "agentic", "open", "instructions"]
source: https://tech-stribe.com/anthropic-donates-agent-skills-ai-foundation/
source_anchor: ""
source_lines: [1, 135]
section: "Outils_Agents"
---
What if the instructions you spent months teaching your AI assistant only worked in one app?

That is the lock-in problem **Agent Skills** was built to avoid. A skill is a folder of instructions that teaches an AI agent how to do a task, and the format is designed to work across tools. On October 22, 2026, Anthropic handed the standard to a neutral home: the **Agentic AI Foundation**, part of the Linux Foundation.

This guide explains what changed, why **open standards** matter, and the limits of what a foundation donation can actually fix—combining the announced governance changes with independent security evidence.

## The News in 30 Seconds

| Question | Answer | 
|---|---|
| **What happened?** | Anthropic donated the Agent Skills standard to the Agentic AI Foundation (AAIF) under the Linux Foundation | 
| **When?** | October 22, 2026, announced at AGNTCon and MCPCon in San Jose | 
| **What moved?** | The name and logo, the GitHub organization, the specification, and the `agentskills.io` domain | 
| **Who runs it now?** | Maintainers from Anthropic, Google, OpenAI, and others, with David Soria Parra and Mahesh Murag as lead maintainers | 
| **Does it stay free?** | Yes. It remains open, and using or contributing does not require joining the foundation | 
| **Same day** | Three other projects joined AAIF: Open Responses (OpenAI), OpenEnv (Hugging Face), and OpenSandbox (Alibaba) | 

## What Are Agent Skills?

An agent skill is a folder that gives an AI agent specialized knowledge for a task. The agent reads a short description and loads the full instructions into its active context window only when it needs them.

| Piece | What it does | 
|---|---|
| **SKILL.md** | The main file, with a description and step-by-step instructions | 
| **Scripts** | Optional code the agent can run for the task | 
| **Resources** | Templates, reference files, and examples the skill can use | 

Think of it as a recipe card the agent pulls from the shelf only when a matching dish is ordered.

## How We Got Here

| Date | Milestone | 
|---|---|
| **October 2025** | Anthropic launches skills in Claude | 
| **December 9, 2025** | The Agentic AI Foundation is formed, and MCP is donated to it | 
| **December 2025** | Agent Skills is published as an open standard | 
| **October 22, 2026** | Agent Skills is donated to AAIF | 

Anthropic says more than 70 agent platforms now support the standard, with 1 million skills published and nearly 280 million installs. Those are Anthropic’s reported figures.

## Who Runs It Now?

- **The foundation.** AAIF is a Linux Foundation body that Anthropic co-founded with Block and OpenAI. After the October additions it hosts ten projects, including Model Context Protocol (MCP).
- **The maintainers.** Engineers from Anthropic, Google, OpenAI, and others maintain the standard as individuals, not as company delegates.
- **The roadmap.** The proposed plan starts with versioned releases, a formal proposal process, and security guidance for people who write and install skills.
- **The home for changes.** Issues and proposals now go to the foundation’s GitHub, according to developer documentation covering the transition.

## Why Open Standards Matter

| Dimension | Without a shared standard | With one | 
|---|---|---|
| **Portability** | Your instructions work in one product | The same skills run across many tools | 
| **Vendor risk** | One company can change terms | No single vendor controls the rules | 
| **Buying decisions** | Hard to compare tools | Ask: do you read the format, and which version? | 
| **Effort** | Every tool invents its own format | Builders support one format | 
| **Trust** | Rules set privately | Public proposals and shared maintainers | 

The Linux Foundation is the nonprofit that runs Linux and hundreds of open projects shared among competing companies, which is the model AAIF follows. MCP took the same path in December 2025, so reusable workflows and standardized tool connections now share a neutral home.

Analysts also read the move as a bid for de facto standardization. Neutral governance makes it easier for rivals to adopt the format without feeling they are following a competitor.

### Open Standard vs Open Source

The two terms get mixed up. An open standard is a shared specification that anyone may implement. Open source means the code itself is public. Agent Skills is mainly the first: a format that many products can read, with reference code and tools alongside it.

### A Familiar Pattern

Shared standards have a long record of making whole industries easier to build on. Web pages work in any browser because of common rules, and Kubernetes grew under the Cloud Native Computing Foundation. The AI agent world is following the same playbook. The open question is whether a standard born at one company can stay truly shared.

## What Does Not Change

- **It is still free to read and implement.** The specification remains at`agentskills.io` and the code stays on GitHub.
- **You do not have to join the foundation** to use or contribute.
- **Existing skills should keep working.** Developer guidance describes the spec as backward compatible, so check the release notes for your tools.

## What a Foundation Does Not Fix

A neutral home answers the question of who controls the format. It does not answer whether the skills you install are safe.

| Source | Date | Finding | Note | 
|---|---|---|---|
| **Snyk** | Feb 5, 2026 | Of 3,984 skills scanned, 13.4% had a critical security issue, 36.8% had any issue, and 76 had confirmed malicious payloads | Security vendor study of public marketplaces | 
| **Koi Security** | Feb 2026 | About 900 of roughly 4,500 skills in one marketplace were weaponized in the ClawHavoc campaign | As summarized by Forkast | 
| **Unit 42** | Jun 2026 | 49,943 skills crawled. Most had behavior mismatches, and 18.9% of those showed adversarial intent | As summarized by Forkast. Most mismatches were oversights | 

**An important distinction:** These studies measured third-party skill marketplaces, not the standard itself. They show why the foundation’s planned security guidance matters, but they do not show the specification is unsafe.

Skills run with the permissions of the agent that loads them. Snyk compares the situation to the early days of npm and PyPI, where installing a package was itself an attack route—a risk pattern teams already mitigate when locking down MCP servers and AI tool connections.

## What Teams Should Do

Practical operational checklist for teams deploying agent skills today (independent engineering guidance rather than official foundation policy):

| Step | Action | 
|---|---|
| **1** | Inventory the skills your agents use, and trace where each one came from. | 
| **2** | Review any skill that touches credentials, files, the network, or a shell before installing it. | 
| **3** | Prefer skills you wrote yourself or got from sources you trust, and keep them in version control. | 
| **4** | Give agents the fewest permissions they need. | 
| **5** | When evaluating vendors, ask which version of the Agent Skills format they support. | 

Related Reading & Deeper Context

- Agentic AI Explained: How Autonomous Systems Work — how autonomous agent loops combine skills, tools, and orchestration.
- How to Secure MCP Servers and AI Tool Connections — practical permission isolation and vetting controls for third-party agent tools.

## Frequently Asked Questions

## What are Agent Skills?

Agent Skills is an open standard for packaging instructions, scripts, and resources in a folder, centered on a SKILL.md file, so an AI agent can load them when a task needs them.

## Who owns Agent Skills now?

Since October 22, 2026, the Agentic AI Foundation, a Linux Foundation body. Anthropic donated the name, logo, GitHub organization, specification, and agentskills.io domain.

## What is the Agentic AI Foundation?

It is a Linux Foundation group, launched in December 2025 and co-founded by Anthropic, Block, and OpenAI, that hosts open projects for AI agents, including MCP.

## Is Agent Skills still free to use?

Yes. It remains open, and you do not need to join the foundation to use it or contribute.

## Which AI tools support Agent Skills?

Anthropic says more than 70 agent platforms support it, including ChatGPT and Cursor, according to coverage of the announcement.

## Are agent skills safe to install?

Not automatically. Security studies of public skill marketplaces found significant issues, so review skills that access credentials, files, or the network before installing.

## Why do open standards matter for AI agents?

They make skills portable across tools, reduce lock-in, and give buyers a simple question to ask vendors: which version of the standard do you support?
