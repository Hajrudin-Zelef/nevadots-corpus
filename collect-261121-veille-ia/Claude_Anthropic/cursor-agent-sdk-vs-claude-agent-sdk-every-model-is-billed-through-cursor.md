---
id: collect-261121-veille-ia/claude-anthropic/cursor-agent-sdk-vs-claude-agent-sdk-every-model-is-billed-through-cursor
title: "Cursor Agent SDK vs Claude Agent SDK: Every Model Is Billed Through Cursor"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["cursor", "agent", "claude", "model", "agent-sdk", "install", "through", "docs"]
source: https://dev.to/alanwest/cursor-agent-sdk-vs-claude-agent-sdk-every-model-is-billed-through-cursor-gal
source_anchor: ""
source_lines: [1, 83]
section: "Claude_Anthropic"
---
The npm package for Cursor's new Agent SDK isn't `@cursor/anything`. According to the docs, you install it with:


```
npm install --global @grok/agent-sdk
```
That package name breaks the usual framing of the Cursor Agent SDK vs Claude Agent SDK question, which goes "editor company with a model-agnostic harness vs model company pushing its own model." SpaceX closed its $60B acquisition of Cursor on August 14 and folded it into SpaceXAI, and Cursor's own pricing page now lists Grok 4.x as a "Cursor model". So both of these SDKs come from a company that trains models. And whichever model you pick in Cursor's SDK, Claude and GPT included, the inference runs through Cursor and so does the bill.

I went through the docs, changelogs and the HN thread (141 points, 132 comments) for Cursor's Agent SDK, Anthropic's Claude Agent SDK and OpenAI's Agents SDK. I haven't run any of them, so everything below is what the vendors published, with links.

Cursor announced the Agent SDK on October 20. The changelog marks 0.3.0 (2026-10-23) as the GA release. It's the framework Cursor uses internally for Bugbot, Security Reviewer and Rollouts.

An agent is a folder of Markdown and TypeScript. Prompts live in `.md` (or `.ts`) files, you write the tools, and Cursor's harness runs the model loop. The CLI needs Node 22.13 or newer:


```
agent-sdk login
agent-sdk init ./my-bot
agent-sdk dev        # local playground
agent-sdk validate --dir .
agent-sdk deploy     # Cursor-managed hosting
```
The CLI also covers evals, logs, deployment secrets, Slack setup and webhook replay for GitHub, GitLab, Bitbucket and PagerDuty. Subagents are defined in the project itself. If you don't want Cursor hosting it, the docs say you can run it on your own infrastructure. One team at Cursor built its internal `kb-reviewer` PR bot "in about a day."

There's some naming mess. The announcement's code sample imports `defineAgent` from `@cursor/agent-sdk`. The docs install `@grok/agent-sdk` and note it used to be `@cursor/bdk`. On top of that there's a separate, older `@cursor/sdk` (April 2026) for calling Cursor's agent from your own code, and the Agent SDK docs say it runs on that same Cursor SDK harness. Pin your versions.

|  | Cursor Agent SDK | Claude Agent SDK | OpenAI Agents SDK | 
|---|---|---|---|
| Install | `npm i -g @grok/agent-sdk` | `npm install @anthropic-ai/claude-agent-sdk` /`pip install claude-agent-sdk` | `pip install openai-agents` /`npm install @openai/agents zod` | 
| Languages | TypeScript (+ Markdown prompts) | TypeScript, Python | Python, TypeScript | 
| Runtime minimum | Node 22.13 | Node 18.14 / Python 3.10 | Python 3.10 / Node 22 | 
| Models | Cursor's catalog: Composer 2.5, Grok 4.x, Claude, GPT, Gemini, DeepSeek, GLM, Kimi | Claude only | OpenAI by default; others via Chat Completions, LiteLLM (beta), Any-LLM (beta) | 
| Where inference goes | Through Cursor | Anthropic API, Bedrock, Google Cloud's Agent Platform, Microsoft Foundry, Claude Platform on AWS | Wherever you point it | 
| MCP | Yes | Yes | Yes | 
| Built-in coding tools | Cursor's harness | Read/Edit/Bash/Glob/Grep/WebSearch etc. | You bring them (plus sandbox agents) | 
| Hosting | Cursor-managed or self-hosted | Self-host; Managed Agents if you want Anthropic to run it | Self-host | 
| Sandboxing | `local.sandbox` , off by default | Permissions + hooks; container isolation is on you | Sandbox agents: Unix-local, Docker, hosted | 
| Pricing | Token-based, same pools as the IDE | API token pricing | Whatever your model provider charges | 
| License | Proprietary (Cursor Terms of Service) | Anthropic Commercial Terms; bundles the closed Claude Code binary | MIT | 

Every row is sourced in the list at the end. A few of them need more room than a table cell.

Model choice is Cursor's main selling point. On the Cursor TypeScript SDK the docs say it plainly: "All inference goes through Cursor's hosted models in both modes." You can switch from `composer-2.5` to a Claude or GPT model by changing one field, but the bill always comes from Cursor.

Under the current plans, third-party models are billed at API list price on Premium ($30/mo) and up. On Teams and Enterprise there's also a Cursor Token Rate of $0.25 per million tokens on eligible third-party models. SDK runs "follow the same pricing, request pools, and Privacy Mode rules as runs from the IDE." The Agent SDK docs don't list a separate price for hosted deployments, so you can't budget hosted agents from the docs alone yet.

So you can swap models all day, but leaving Cursor means a rewrite. The lock-in is the Cursor account, and picking a different model in the config does nothing about it.

The Claude Agent SDK gives you "the same tools, agent loop, and context management that power Claude Code" in Python and TypeScript. Under the hood it spawns a `claude` CLI subprocess per session. It runs Claude models only, but you can pay for them through Anthropic's API, Bedrock, Claude Platform on AWS, Google Cloud's Agent Platform or Microsoft Foundry:


```
export CLAUDE_CODE_USE_BEDROCK=1   # or CLAUDE_CODE_USE_VERTEX=1, CLAUDE_CODE_USE_FOUNDRY=1
```
So you depend on one model family but not on one vendor account. For a company that already has an AWS or Azure commit, that's a lot easier to get through procurement than another SaaS bill.

The hosting docs are refreshingly concrete. Start at 1 GiB RAM, 5 GiB disk and 1 CPU per agent. A minimal container costs "roughly $0.05 per hour," and token cost "typically dominates container infrastructure cost by an order of magnitude or more."

The part people argue about is subscriptions. The docs say: "Unless previously approved, Anthropic does not allow third party developers to offer claude.ai login or rate limits for their products." Plan on API billing. A long stretch of the HN thread was people disagreeing about whether `claude -p` and the SDK draw from Max limits, which tells you how unclear that line is in practice.

This was the best exchange on HN. One commenter argued "there's reason to believe that Claude itself works better with Claude Code than with other harnesses," and another replied that labs generally tune models alongside a specific harness. If that's true, a model-agnostic harness is always running someone else's model a bit off-tune.

I think it's true in part. Cursor's argument works the same way, just for Composer and Grok: those are the models its harness is tuned for. With Claude in Cursor's harness, you get Cursor's tool definitions and prompts, not Anthropic's. Neither vendor publishes a cross-harness benchmark, so take any "better" claim as marketing until one shows up.

The OpenAI Agents SDK is MIT-licensed and the most provider-agnostic of the three. The caveats are in its own docs. LiteLLM support is "best-effort, beta." Some providers "don't have support for structured outputs." Tracing uploads fail with a 401 if you don't have an OpenAI key. It also ships a much thinner coding harness. You get sandbox agents with Unix-local and Docker clients, but not a ready-made Claude Code or Cursor agent.

- For a PR bot or code reviewer you want shipped this week, I'd pick the Cursor Agent SDK. `agent-sdk deploy` , PR triggers and evals out of the box beat wiring all that up yourself. Accept that you're a Cursor customer for as long as it runs.
- For an agent inside your own product, especially a multi-tenant one, I'd pick the Claude Agent SDK. It has the best-documented production story of the three, and your inference bill can sit with your cloud provider.
- If exit cost is what you care about most, go with the OpenAI Agents SDK and write the coding tools yourself.

1. Put your tools behind MCP servers. All three SDKs speak MCP, so your tools are the part you can move to another SDK. Prompts in Markdown move easily too. Harness-specific hooks don't.
2. Running Cursor agents locally? Set `local.sandbox` in`defineAgent` . It's off by default, which means shell commands auto-approve and inherit the host's credentials. With it on, shell commands run without network access and writes outside the session workspace are refused.
3. Running the Claude SDK for multiple tenants? Pass `settingSources: []` , set`CLAUDE_CODE_DISABLE_AUTO_MEMORY=1` , and give each tenant its own`cwd` and`CLAUDE_CONFIG_DIR` . Otherwise one tenant's`CLAUDE.md` can leak into another's session.
4. Set `maxTurns` . Claude's docs say outright that there's no top-level session timeout.

So: would you build your agent on an editor company's SDK or a model company's, now that Cursor is technically both? I think the harness matters less than whose invoice it's on, and I'd like to hear from anyone who has shipped on one of these and disagrees.

- Cursor: Agent SDK announcement, Agent SDK docs, CLI reference, changelog, TypeScript SDK docs, Cursor SDK post, models and pricing
- Anthropic: Agent SDK overview, quickstart, hosting
- OpenAI: Agents SDK (Python), models, sandbox agents, openai-agents-js
- Discussion: Hacker News thread; acquisition: The Next Web (deal terms), Yahoo Finance (close)

## Top comments (0)
