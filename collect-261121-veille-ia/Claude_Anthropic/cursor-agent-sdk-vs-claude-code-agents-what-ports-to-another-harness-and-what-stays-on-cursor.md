---
id: collect-261121-veille-ia/claude-anthropic/cursor-agent-sdk-vs-claude-code-agents-what-ports-to-another-harness-and-what-stays-on-cursor
title: "Cursor Agent SDK vs Claude Code agents: what ports to another harness, and what stays on Cursor"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-10-20"]
keywords: ["cursor", "agent", "claude", "code", "agents", "anthropic", "team", "model"]
source: https://stackness.dev/blog/cursor-agent-sdk-vs-claude-code-agents-what-ports-and-what-locks-in
source_anchor: ""
source_lines: [1, 97]
section: "Claude_Anthropic"
---
Cursor made its Agent SDK generally available on 20 October 2026: agents written in TypeScript and Markdown, woken by pull requests, Slack messages, webhooks or cron, and deployed to Cursor with one command. My recommendation for a team already on Claude Code, as of 1 November 2026: keep the agent's instructions, skills and MCP servers in plain files, and run the service on the Claude Agent SDK or the Claude Code GitHub Action. It depends on one condition. Choose the Cursor Agent SDK only if the team already pays for Cursor and wants Cursor to host the service, because every model call goes through Cursor, even when you host it yourself, and the hosting price is not published.

## Cursor Agent SDK vs Claude Code agents: what the decision covers

The Cursor Agent SDK and Claude Code's agents fill different slots, so the decision is less about which writes better code than about where an always-on agent lives. A Claude Code subagent is a Markdown file under `.claude/agents/` that runs inside someone's session. The Cursor Agent SDK builds a long-running service with its own URL, triggers, secrets and state.

On the Claude side, the equivalent of that service is assembled from parts: the Claude Agent SDK, a library you run in your own container; Claude Managed Agents, Anthropic-hosted and in beta; scheduled tasks in Claude Code, a research preview tied to one person's account; and the GitHub Action, MIT-licensed, on your runners. The question for a tech lead is which of these a team can own for a year without rewriting it.

The Agent SDK is a new product, not the Cursor SDK that went into public beta on 29 April 2026 for driving Cursor's agent from code. Most comparisons that rank for the query, including MindStudio's and SpotDev's, cover that April SDK.

## What each option commits the team to

Each way of running a team agent commits the team to a different owner for four things: the code that defines the agent, the runtime it runs on, the model calls and the bill. The table lists them for the five options a Claude Code team would weigh, from Cursor's and Anthropic's documentation as of 1 November 2026.

| Option | Status | Where it runs | Model calls go to | What you pay | 
|---|---|---|---|---|
| Cursor Agent SDK | GA, 20 Oct 2026 | Cursor hosting or your own Node 22.13+ process | Cursor, with `CURSOR_API_KEY` , default`grok-4.5` | Tokens through Cursor; hosting price not published | 
| Claude Agent SDK | Released, no preview label | Your container | Anthropic, Bedrock, Vertex or Foundry | Tokens, plus a container Anthropic puts at about $0.05 an hour | 
| Claude Managed Agents | Beta | Anthropic | Anthropic | Tokens plus $0.08 per session-hour | 
| Claude Code GitHub Action | GA | Your GitHub runners | Anthropic or a cloud provider | Actions minutes plus tokens, or a subscription token | 
| Claude Code scheduled tasks | Research preview | Anthropic | Anthropic | Counts against one person's plan | 

## The risks behind each Cursor Agent SDK commitment

### Every model call goes through Cursor, even self-hosted

The Cursor Agent SDK's self-hosting moves the process, not the inference. Cursor's docs say "You write the instructions and tools, and Cursor runs the model loop that calls them", and the self-host example starts with `export CURSOR_API_KEY`. The package licence reads "© Anysphere Inc. All rights reserved", although its README calls it "the open-source version of the engine behind Bugbot".

Model choice follows the same line. The default is `grok-4.5` with high effort and fast mode; Cursor joined SpaceX on 14 August 2026, and the package ships as `@grok/agent-sdk`. Third-party models are billed through Cursor at list price, with a $0.25 per million token "Cursor Token Rate" on Teams and Enterprise plans. I found no documented path to bring your own Anthropic or OpenAI key to an Agent SDK agent. The Claude Agent SDK has the mirror-image limit: it is not open source either, and it calls only Claude models.

### The Cursor Agent SDK hosting price and plan are not published

As of 1 November 2026, neither Cursor's pricing page nor its docs give a price for a deployed agent's hosting, or say which plan is required to deploy one. What is documented: each turn reports its token cost, some teams are capped at 20 deployments and 100 deploys a day, a schedule's run must finish within 26 minutes on Cursor hosting, and "Privacy Mode (Legacy)" refuses to deploy at all.

The Anthropic options publish both halves. Managed Agents bills $0.08 per session-hour on top of tokens; Anthropic's worked example puts a one-hour Opus 5.5 session at $0.58. A self-hosted Claude Agent SDK container is your own compute, which Anthropic estimates at "roughly $0.05 per hour" for a minimal one, "while a single long agent session can spend dollars in tokens".

### Three package names in four months

The Cursor Agent SDK is 12 days old in its current form and has moved quickly. The same code is published under three package names: `@cursor/july`, created in July, `@cursor/bdk`, the "Bot Development Kit", created in September, and `@grok/agent-sdk`, created on 22 October and the one the docs use. Cursor's changelog tells readers to run `npx @cursor/agent-sdk init`, a name that returns 404 on npm.

Version 0.5.0 arrived on 28 October, two minor versions after the 0.3.0 release the docs label GA, which removed the `persistent` option. Cursor's migration guide for projects deployed during the early alpha warns that a cutover under the same name "deletes v1's hosted sessions, KV, files, and eval runs". Cursor's own products run on it: Bugbot, Security Reviewer and Rollouts, per the launch post.

## What ports between harnesses, and what does not

The parts of an agent written in plain text port between Cursor, Claude Code and other harnesses; the parts written against one vendor's runtime do not. Instructions are Markdown, skills follow the SKILL.md convention both sides read, and MCP server configs are URLs and headers. Triggers, hosting, state and evals are written against `@grok/agent-sdk` and stay with it.

| Part of the agent | Ports? | Note | 
|---|---|---|
| Instructions ( `instructions.md` ,`CLAUDE.md` ,`AGENTS.md` ) | Yes | Plain Markdown | 
| Skills | Yes | SKILL.md on both sides | 
| MCP servers | Yes | Same URL and headers | 
| Tool logic | The function body | The `defineTool` wrapper and`ctx` do not | 
| Per-agent tool limits and model choice | Partly | wshobson/agents drops `tools:` for Cursor and rewrites`model: opus` to`inherit` | 
| Hooks | No | wshobson/agents lists none for Cursor or Codex subagents | 
| Triggers (GitHub, Slack, webhooks, cron) | No | Ride on the Cursor account | 
| State ( `ctx.host.kv` , files, sessions) | No | Stored on Cursor hosting | 
| Evals and `github replay` | Partly | The suites are SDK code; replay fixtures are plain JSON | 

The import runs one way. The Agent SDK's `cursorPlugin()` mounts a Claude Code plugin, turning its skills, commands, agents and MCP servers into Agent SDK parts, though "Plugin rules and hooks aren't mounted". Nothing turns an Agent SDK project back into a Claude Code plugin. wshobson/agents, 42,066 stars, keeps one source in the Claude Code plugin format and ships it to Claude Code, Codex, Cursor, OpenCode, Copilot, Antigravity and Pi. It covers editor subagents and plugins, not Agent SDK services.

## A hosted review bot is cheaper than an agent you own

A hosted code review bot beats a code-defined agent whenever a pull request comment is the whole job. SpotDev's test for when a Cursor automation is enough applies here too: "a pull request opens and a comment comes back". The bots have published per-review prices and need no service, secrets or evals to maintain. All quality figures below are the vendors' own.

| Bot | Price as published | Vendor's own quality claim | 
|---|---|---|
| Bugbot | Average $1.00-$1.50 per run, usage-based since May 2026; was $40 per seat | 80% of findings resolved by merge time | 
| GitHub Copilot code review | About $0.25, $1.50 or $10 per review by effort level | Not given | 
| CodeRabbit | $24, $48 or $72 per user a month, billed annually | Not given | 
| Claude Code Review | Token-billed; "generally average $15–25" at its March launch | PRs with substantive comments rose from 16% to 54% | 
| Claude Code GitHub Action | Actions minutes plus tokens | Anthropic calls Code Review "more thorough (and more expensive)" | 

Cursor's own docs draw the same line: Automations fit when "The job fits a schedule or event trigger", and the Agent SDK when "You want code around model judgment that your team can version control, review, test and hillclimb". The launch post's example is kb-reviewer, which flags pull requests that contradict a recorded team lesson, a rule no stock bot knows.

## What would change the recommendation

Three published facts would move this recommendation toward the Cursor Agent SDK. A hosting price and a named plan would make the cost comparable with Managed Agents. A documented bring-your-own-key path would end the rule that every model call goes through Cursor. A stable package name for a few months would make the churn risk smaller. Each is a docs change, not a rewrite.

Two facts would move it further away: a hosted-agent price above Managed Agents' $0.08 per session-hour, or another rename. On Stackness, as of 1 November 2026, Cursor is on 7 real public stacks and Claude Code on 8, and 4 profiles list both (how I count). Teams that use both harnesses, not one, are the norm among the AI coding tools developers list.

## What I would do

- If the team already pays for Cursor Teams and wants a PR or Slack agent hosted with no infrastructure work, I would try the Cursor Agent SDK on one non-critical agent, keep `instructions.md` , skills and MCP configs in a folder that Claude Code also reads, and avoid storing state in`ctx.host.kv` .
- If the team runs on Claude Code and owns its CI, I would start with the Claude Code GitHub Action, which is MIT-licensed and runs on its own runners, and move to the Claude Agent SDK in a container when the agent needs Slack or webhooks.
- If no one on the team will own a TypeScript service, its evals and its secrets, I would buy a review bot: Bugbot at about $1.00-$1.50 a run or Copilot code review at about $0.25 to $10 a review.
- If agent definitions have to run in several harnesses, I would keep one source in the Claude Code plugin format, as wshobson/agents does, and accept that Cursor drops per-agent tool limits.
- If Cursor publishes a hosting price at or below Managed Agents' rate and a bring-your-own-key path, I would run this decision again.

## New posts by email

About one email a week with what we published. No account needed.

## Which of these do you use?

Nobody on Stackness runs Cursor Agent SDK yet. Tick what you use and be the first.

For AI agents and assistants: a plain-text index of this site is at /llms.txt, the full text at /llms-full.txt, and Stackness has an MCP server for reading stacks and tools (/docs/mcp).
