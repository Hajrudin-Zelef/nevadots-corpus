---
id: collect-261121-veille-ia/cursor-ide/agents-get-a-home-of-their-own-cursor-s-agent-sdk-entire-s-git-forge-and-smarter-permission-prompts
title: "Agents get a home of their own: Cursor's Agent SDK, Entire's Git forge, and smarter permission prompts"
domain: cursor-ide
role: reference
task: article
actors: ["Anysphere"]
dates: []
keywords: ["agent", "agents", "cursor", "entire", "github", "october", "code", "every"]
source: https://vibekitweekly.com/issues/issue-003.html
source_anchor: ""
source_lines: [1, 115]
section: "Cursor_IDE"
---
**How to read this issue:** every news item links to its source, mostly official changelogs and company announcements. Dates are as published by each vendor for the week of October 20 to 27, 2026. Version numbers and model names change fast, so click through before you change anything. Nothing here is sponsored.


## The short version

- **Agents became projects you can ship.** Cursor's new Agent SDK turns an agent into a folder of code you can test with evals and deploy. Entire, the startup led by former GitHub CEO Thomas Dohmke, launched a Git forge where agent-scored "Trails" replace pull requests.
- **Permission prompts got smarter, and you still own the risk.** GitHub Copilot's assisted permissions are now generally available: a model lets clearly safe requests through and asks you about the rest.
- **Update Claude Code.** Version 2.1.309 (October 22) fixed several ways sandboxed commands could step outside the sandbox.
- **A quiet model swap.** Lovable retired the Gemini 2.5 models for app AI features on October 20 and now routes those requests to newer models automatically. Your app still works, but cost and answers can change.

## This week's news: what changed, what breaks, what to do

### 1. Cursor Agent SDK: an agent is now a folder of code

**What changed.** The Cursor changelog announced the Agent SDK as generally available on October 20. You start with `npx @cursor/agent-sdk init ./my-bot` and run it locally with a playground using `agent-sdk dev`. In a project, `instructions.md` is the prompt, each file in `tools/` is one typed action, `skills/` holds procedures the model loads on demand, `mcp-connections/` adds tools from MCP servers, and `channels/` says where the bot listens. Agents can wake on GitHub, GitLab and Bitbucket pull request, comment and CI events, on Slack mentions and DMs, on webhooks, or on a cron schedule. `agent-sdk eval` runs tests that check which tools ran and what the reply looked like, and `agent-sdk deploy` puts the bot on Cursor's infrastructure with a URL. Templates include a security reviewer, a PR autofixer and a Linear or Jira triager.

**What breaks.** Nothing breaks, but the bar moves. "My agent works" now means "my agent passes its evals", and a bot that wakes on every pull request can spend money and post comments while you sleep.

**What to do.** If you've been gluing prompts and scripts together, try rebuilding one small bot as a project (this week's project walks you through it). Write two or three evals before you add a channel, and start with a cron schedule rather than "every pull request".

### 2. Entire: a GitHub alternative where agents score the code

**What changed.** On October 26, Entire launched a Git forge "built for agents and humans to work together". Pull requests are replaced by Trails, which show evals scored by agents on every push, available on the web and through the open-source Entire CLI. It also adds Continuous Memory, which syncs agent sessions into the repository so later agents can search them, and private repo hosting in the US, EU, India and Australia. Entire claims pushes up to 89x faster than GitHub, based on its own benchmark. GeekWire notes teams can still require a person's sign-off or passing tests before anything merges, and that you can mirror from GitHub instead of moving. The company raised a $60 million seed round in February.

**What breaks.** Habits, mostly. "Merge without reading every line" only works if the evals are good. A weak eval plus a confident score is how bad code ships faster.

**What to do.** No need to move your repos. Do steal the idea: for your next agent-written change, write down what "done" means as a short checklist before the agent starts, and check the result against it instead of skimming the diff.

### 3. GitHub Copilot: assisted permissions and a cheaper everyday model

**What changed.** Assisted permissions became generally available on October 26 in Copilot CLI, the Copilot app and VS Code. When Copilot asks to run a command, touch a file path or open a URL, a language model evaluates the request: clearly safe ones proceed, everything else needs your approval. In the CLI, run `/permissions` and pick assisted. The same day, GPT-6.1 Terra arrived in Copilot, pitched for everyday agentic coding and terminal work at a lower cost than GPT-6.1 Sol, rolling out gradually to Pro, Pro+, Max, Business and Enterprise. OpenAI's changelog also lists Terra in ChatGPT Work and Codex on paid plans from October 26.

**What breaks.** Your attention, if you let it. Fewer prompts means you read each one less carefully. "Clearly safe" is the model's judgment, not a guarantee.

**What to do.** Assisted is a good middle ground between approving everything by hand and "Allow All". Keep the prompts that do appear slow and deliberate, especially for deletes, network calls and anything touching `.env`. Try Terra for routine tasks, and keep a stronger model for tricky ones.

### 4. Claude Code: sandbox holes closed, steady fixes

**What changed.** Claude Code v2.1.309 (October 22) fixed sandboxed Bash commands on macOS and Linux running the shell's startup files outside the sandbox, sandboxed commands and auto-accepted edits being able to modify the MCP config file passed with `--mcp-config`, and some compound commands that skipped a permission prompt. v2.1.307 (October 21) stopped HTTP MCP tool calls from being sent twice when a connection dropped; only read-only or idempotent tools are retried now. The newest, v2.1.311 (October 26), fixed standard-context sessions stopping with "Usage credits required for 1M context" past 200K tokens: Claude Code now compacts and continues.

**What breaks.** On older versions, the sandbox protects less than you think, and a flaky connection could run an MCP action, like sending a message or creating a ticket, twice.

**What to do.** Update. If you rely on the sandbox to keep an agent away from files, re-test that one rule after updating instead of assuming.

### 5. Lovable: Gemini 2.5 retired for app AI features

**What changed.** The Lovable changelog says Gemini 2.5 Pro, Flash and Flash Lite retired for your app's AI features on October 20. Requests for Pro and Flash now go to Gemini 3.8 Flash, and Flash Lite requests go to Gemini 3.1 Flash Lite. Lovable also added an option to have it create a Supabase project for an app with no backend (October 20). On October 22, Privacy & security settings got clearer names, with confirmation dialogs for wide-reaching changes.

**What breaks.** Nothing crashes, which is the trap. Lovable says the replacement can cost a different amount per request and respond differently, so prompts tuned for the old model may drift.

**What to do.** If your app named a Gemini 2.5 model, ask Lovable to switch it to a current one on purpose and re-test your main AI feature. Last week's Model Retirement Radar would have flagged it.

### 6. Also worth a click

- **Replit** shipped a Monitoring Agent (October 23) that investigates production issues from your app's logs and metrics and proposes fixes; auto-publishing those fixes is an experimental opt-in. It also lets you save workflows as reusable skills, and Replit warns that shared chats include development URLs that open live servers.
- **OpenAI Codex CLI 0.164.0** (October 23, per the changelog) adds ten saved`/presets` slots for prompts and commands, and masks credentials during supported remote Bash sessions.
- **GitHub** public monitoring for leaked secrets added validity checks and alert triage (October 23), and Copilot usage metrics now name the custom skills, agents and MCP servers your organization uses (October 22).
- **In case you missed it:** Google's Gemini 4 Argon (October 13) launched at an introductory API price of $2 per million input tokens and $10 per million output tokens until November 30, 2026, after which it rises to $4 and $20. It's in the Antigravity desktop app and CLI for paid plans. If you're building on it, budget for December now.

## Project of the week: your first agent as code

**Level:** beginner to intermediate · **Time:** an afternoon · **Category:** agents

Two weeks ago you built a Changelog Translator, and last week a Model Retirement Radar. This week, turn them into a real agent: a small bot that wakes up every Monday morning, reads the changelogs you care about, and posts a plain-English "what changed, what breaks, what to do" summary to one place, with evals so you know it still works next month.

The prompt below targets Cursor's Agent SDK because its project layout matches the idea neatly, but the same plan works in any agent framework. Commands and folder names come from Cursor's announcement; check the SDK docs for exact syntax.

**What it's for, in one sentence:** every Monday, tell me in plain words which tool changes affect me, with links, without me opening ten changelogs.

### Starter prompt

```
Help me build a small agent called monday-changelog with the Cursor Agent SDK.
Start from `npx @cursor/agent-sdk init ./monday-changelog` and explain each
generated file before changing it.
1. instructions.md: the agent reads a list of changelog URLs from sources.json,
   keeps only entries from the last 7 days, and writes a summary with three
   parts per item: what changed, what breaks, what to do. Every item must link
   to its source. If a page can't be read, say so instead of guessing.
2. tools/: one tool that fetches a URL and returns its text, and one that posts
   the summary to a single destination I choose (start with writing a local
   Markdown file; add Slack later).
3. channels/: a cron schedule for Monday 8:00 in my time zone. No pull request
   or comment triggers.
4. evals: at least three. (a) Given a saved sample changelog, the reply links
   every item to its source. (b) Given an unreadable URL, it reports the failure
   and invents nothing. (c) It never calls the post tool more than once per run.
5. Keep secrets in the dashboard or environment, never in files I commit.
Make one change at a time, put the file name above every code block, and run
`agent-sdk validate` and `agent-sdk eval` after each step.
```
### Checklist

- `sources.json` lists 3 to 5 changelogs you actually use
- You can explain what each folder (`tools/` ,`skills/` ,`channels/` ) does
- All three evals pass locally before you add the schedule
- The bot writes to a local file first; Slack or email comes later
- No API keys or tokens in the repository
- You read the first real Monday summary and checked two links by hand

**Stretch goals:** add a skill that checks the summary against last week's Model Retirement Radar list, run `agent-sdk eval` in CI, or deploy with `agent-sdk deploy` once a week of local runs looks right.

## Builder tips from the week

*(Collected from public posts by other builders. Treat them as ideas to try, not facts.)*

1. **Better references beat better prompts.** A widely discussed post argued that AI-built apps look generic because people describe a design instead of showing one. Collect three screenshots of sites you like and attach them before you ask for a redesign.
2. **The hard part is knowing what to build.** Several builders said the same thing this week (one, two): the tools make building easy, picking a worthwhile idea is the bottleneck. Our project generator is one way to start; another is to write down three annoyances from your own week.
3. **An idea worth building beats a fast build.** Another builder put it plainly: having an app idea that's actually worth building is the main thing, and making it is the easier bit. Before you prompt, describe in one sentence who will use it next Tuesday.

## Next week

We'll keep tracking the November 16 Copilot model retirements, Lovable's November 19 Gemini 3.6 Flash cutoff, and Gemini 4 Argon's price change after November 30. Tell us which tools you use, and we'll prioritise their changes.

**Sources & transparency:** every news item above links to its source, mostly vendor changelogs, release notes and company announcements, checked on October 27, 2026. Performance claims (such as Entire's push speed) are the vendors' own. Builder tips come from public posts and are opinions, not verified facts. This issue has no sponsor. VibeKit Weekly is not affiliated with any tool vendor mentioned.
