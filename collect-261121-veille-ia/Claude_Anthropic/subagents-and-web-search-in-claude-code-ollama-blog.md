---
id: collect-261121-veille-ia/claude-anthropic/subagents-and-web-search-in-claude-code-ollama-blog
title: "Subagents and web search in Claude Code · Ollama Blog"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["subagents", "search", "ollama", "claude", "code", "cloud", "research", "model"]
source: https://ollama.com/blog/web-search-subagents-claude-code
source_anchor: ""
source_lines: [1, 50]
section: "Claude_Anthropic"
---
# Subagents and web search in Claude Code

## February 16, 2026

Ollama now supports subagents and web search in Claude Code. No MCP servers or API keys required.

## Get started

```
ollama launch claude --model minimax-m2.5:cloud
```
It works with any model on Ollama’s cloud.

## Subagents

Subagents can run tasks in parallel, such as file search, code exploration, and research, each in their own context.

Longer coding sessions stay productive. Side tasks don’t fill the context with noise.

Some models will naturally trigger subagents when needed (minimax-m2.5, glm-5, kimi-k2.5), but you can force triggering subagents by telling the model to “use/spawn/create subagents”

Example prompts:

```
> spawn subagents to explore the auth flow, payment integration, and notification system
> audit security issues, find performance bottlenecks, and check accessibility in parallel with subagents
> create subagents to map the database queries, trace the API routes, and catalog error handling patterns
```
## Web search

Ollama’s web search is now built into the Anthropic compatibility layer. When a model needs current information, Ollama handles the search and returns results directly without any additional configuration.

Subagents can leverage web search to research topics in parallel and come back with actionable results.

```
> research the postgres 18 release notes, audit our queries for deprecated patterns, and create migration tasks
> create 3 research agents to research how our top 3 competitors price their API tiers, compare against our current pricing, and draft recommendations
> study how top open source projects handle their release process, review our CI/CD pipeline, and draft improvements
```
## Recommended cloud models

- `minimax-m2.5:cloud`
- `glm-5:cloud`
- `kimi-k2.5:cloud`

## Learn more

- ollama launch for more integrations
- Claude Code with Ollama for basic setup
- Web search API for standalone usage
