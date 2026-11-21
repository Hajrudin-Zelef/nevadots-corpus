---
id: collect-261121-veille-ia/cursor-ide/cursor-s-agent-sdk-is-ga-your-agent-is-a-folder-of-markdown-and-typescript
title: "Cursor's Agent SDK is GA: your agent is a folder of Markdown and TypeScript"
domain: cursor-ide
role: reference
task: article
actors: ["Anysphere"]
dates: []
keywords: ["agent", "cursor", "agent-sdk", "tools", "typescript", "folder", "deploy", "instruction"]
source: https://dev.to/piekwerk/cursors-agent-sdk-is-ga-your-agent-is-a-folder-of-markdown-and-typescript-1jh8
source_anchor: ""
source_lines: [1, 46]
section: "Cursor_IDE"
---
Cursor's Agent SDK went generally available on October 20. The changelog entry carries the one-line summary that matters: you define an agent as a folder of code, test it with evals, and deploy it to Cursor or your own infrastructure.

I spend most of my time on instruction files, CLAUDE.md, .cursorrules, copilot-instructions.md. So the interesting part of this release is what it does to that genre. An instruction file stops being IDE configuration and becomes the entry point of a deployable program, with tests and a deploy target. Here is the contract, piece by piece.

The Agent SDK is the framework Cursor uses for its own managed agents: Bugbot, Security Review, Approval Bot and Rollouts all run on this harness. Now it is a package you can install:


```
npx @cursor/agent-sdk init ./my-bot
cd my-bot
agent-sdk dev
```
`agent-sdk dev` starts the bot locally with a playground. Pass `--template` and you start from a security reviewer, an owners-policy approver, a PR autofixer, or a Linear or Jira triager. Those templates are worth reading even if you never deploy one, because they show how Cursor structures a working agent.

An agent is a small project with a fixed layout:


```
my-bot/
  instructions.md      # the prompt
  tools/               # one typed action per file, TypeScript
  skills/              # procedures the model loads on demand
  mcp-connections/     # tools from MCP servers
  channels/            # where the bot receives and answers messages
```
Prompts are Markdown. Tools are TypeScript functions. `agent-sdk validate` checks the project and `agent-sdk info` prints what was discovered, which is a nice touch: the layout is machine-checked, not just convention.

Everything that applies to a CLAUDE.md or a rules file applies here, with one difference: this prompt rides along on every event the bot handles. A reviewer bot that wakes on every pull request pays for its instruction file on every wake. The 300-line instruction budget logic transfers directly, and the discipline of keeping the always-loaded prompt small matters more, not less, when the agent runs unattended.

The layout encodes a split we have been recommending by hand for a while. `skills/` holds procedures the model loads on demand, which is the Agent Skills shape, bulky reference material that only loads when relevant. `mcp-connections/` is for tools that live in external servers. Stable behavior goes in instructions.md, on-demand knowledge goes in skills, external systems go through MCP. Having all three as first-class directories in one project removes the guesswork about where a given piece of config belongs.

This is the part I would call the real news. An eval sends a fixed message to the bot and asserts on what it did: which tools ran, with what input, and the shape of the reply. Not "did the answer sound right", but "did it call the right tool with the right arguments". You run `agent-sdk eval` locally or in CI.

There is also replay: `agent-sdk github replay <pr-url>` turns a real pull request into signed webhook payloads and sends them to the bot, so a GitHub-driven bot can be tested without waiting for a push. Add `--dry-run --fixtures-dir fixtures/` and the payloads become eval fixtures. That closes the loop that instruction-file work has always been missing: a prompt change lands in the same PR as the eval that proves behavior did not drift.

`agent-sdk deploy` builds the agent, gives it a URL, and starts it on Cursor's infrastructure, with secrets and integrations managed from a dashboard. Or you run it yourself: each agent is a single Node process that can be packaged in a container. That is the same shape as Cursor's self-hosted runners, the managed path is convenient and the self-hosted path is a real escape hatch, not a stub.

Agents wake on pull request, comment and CI events from GitHub, GitLab and Bitbucket, on Slack mentions and DMs, on webhooks from any provider, and on cron schedules. GitHub events pull through your Cursor account, so no public webhook URL is required. Each conversation keeps its own session and a complete record of every message and tool call, and follow-ups on a PR or Slack thread resume the same session. For a review bot that means context survives across the life of a pull request.

The instruction file genre just graduated. Prompts, tools, evals and channels now live in one repository and go through the same review process as the rest of your software, which is the argument we made for versioning agent configs applied to a whole agent. Two things to keep an eye on: the model is pinned in code (the launch post shows a `defineAgent` call with `composer-2.5` and effort parameters, so model routing decisions become reviewable diffs), and the harness is Cursor's, so the agent folder is portable but the surrounding ecosystem is theirs.

If you want a first project, port one existing rules file into an `instructions.md`, write one eval for the behavior you actually care about, and see what breaks. That exercise teaches more about agent config than any amount of template reading.

Sources: the launch post and the changelog entry, both from October 20, and the Agent SDK docs.

## Top comments (0)
