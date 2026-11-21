---
id: collect-261121-veille-ia/claude-anthropic/anthropic-gives-agent-skills-to-the-agentic-ai-foundation
title: "Anthropic gives Agent Skills to the Agentic AI Foundation"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["agent", "skills", "foundation", "anthropic", "agentic", "skill", "instructions", "gives"]
source: https://searchenginewatch.com/anthropic-agent-skills-agentic-ai-foundation/
source_anchor: ""
source_lines: [1, 25]
section: "Claude_Anthropic"
---
Anthropic is donating Agent Skills to the Linux Foundation’s Agentic AI Foundation. The October 22 transfer covers the format’s branding, GitHub organization and website.

A skill packages reusable instructions in a `SKILL.md` file, with optional scripts and reference files.

Using or contributing to the standard remains open without foundation membership. David Soria Parra and Mahesh Murag will serve as lead maintainers.

## Skills load instructions when a task needs them

The format’s documentation describes a staged loading process: an agent initially reads a skill’s name and description, opens its full instructions when the task matches, then retrieves additional files as needed.

Skills can describe research procedures using SEO MCP servers, which connect an agent to external tools and data.

## Software requirements travel with the workflow

The Agent Skills specification allows a `compatibility` field for requirements such as installed software and network access. A skill containing a Python script therefore needs an agent environment that can execute that script.

Tool permissions vary: the experimental `allowed-tools` field has different support across implementations. A procedure that runs in one product can therefore require different permissions in another.

Anthropic’s security guidance recommends auditing unfamiliar scripts, dependencies and network connections before installing a skill, because malicious code or instructions can expose data.

The proposed foundation roadmap includes versioned releases, a formal proposal process and security guidance.

Source: Anthropic

## Start the conversation by posting the first comment
