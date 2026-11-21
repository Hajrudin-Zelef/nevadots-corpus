---
id: collect-261121-veille-ia/claude-anthropic/codex-vs-claude-code-nov-2026-prices-limits-benchmarks
title: "Codex vs Claude Code (Nov 2026): Prices, Limits, Benchmarks"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-11-16"]
keywords: ["claude", "codex", "code", "gpt-6", "benchmarks", "default", "want", "released"]
source: https://openchamber.dev/blog/codex-vs-claude-code/
source_anchor: ""
source_lines: [1, 252]
section: "Claude_Anthropic"
---
**Claude Code with Opus 5.5 solves more tasks on the public agent benchmarks: 65.8% against 60.9% for Codex with GPT-6.1 Sol on Terminal-Bench 4.1. Codex finished that run for about one seventh of the cost, has more npm and Homebrew installs, sandboxes by default and lets third-party apps use your ChatGPT plan. Neither vendor publishes hard usage limits, so their own pages cannot tell you which $20 plan lasts longer. If results on hard tasks matter most, the benchmarks favour Claude Code; if the cost of each task matters most, they favour Codex.**

Both products changed a lot since the start of September. Anthropic shipped Opus 5.5, Sonnet 5.5, Haiku 5.5 and Fable 5.2, and renamed Max 20x to Ultra. OpenAI shipped GPT-6 Astra, GPT-6 Sol, GPT-6.1 Sol and GPT-6.1 Terra, retired GPT-5.5 from Codex on 14 October, doubled the Work and Codex allowance on two of its three Pro plans and announced that ChatGPT and Codex will share one usage allowance from December. Most comparison pages in today’s search results still describe the products as they were in the summer.

**Where we stand.** We build OpenChamber, a desktop, web and mobile app for OpenCode that can also run Claude Code. Both products are described and checked on the same terms, and Claude Code comes first in the tables only because of the alphabet. We did not run our own tests for this article: quality numbers come from public benchmark boards, prices from the vendors’ own pages, install counts from the npm, Homebrew, VS Code Marketplace and GitHub APIs, and the Reddit numbers from our own labelled dataset of AI coding threads. Prices, limits, models, features and install counts were re-checked on 16 November 2026; benchmark, incident and Reddit windows keep their own dates below.

## The short answer

| Choose | If you | 
|---|---|
| Claude Code | want the highest scores on agent benchmarks, want a 1-million-token context window by default, or rely on hooks, `/rewind` or agent teams | 
| Codex | want the cheapest finished task, want to try it without paying, work on native Windows, want a sandbox switched on from the first run, want to use your plan inside other apps, or need a $500 tier with Ultrafast mode | 
| Both | review one agent’s work with the other. OpenAI ships an official plugin that runs Codex inside Claude Code | 

## At a glance

|  | Claude Code | Codex | 
|---|---|---|
| Made by | Anthropic | OpenAI | 
| Default model | Opus 5.5, since 22 September | GPT-6.1 Sol on Plus, Pro, Business, Enterprise and Edu, since 29 September; GPT-6 Luna in the desktop app on Free and Go | 
| Context window | 1 million tokens on Opus 5.5, Sonnet 5.5, Haiku 5.5 and Fable 5.2 | 500,000 tokens for GPT-6.1 Sol by default, according to OpenAI’s docs; 1,050,000 through the API | 
| API price of the default model, per million tokens | $4 input, $20 output, with no surcharge for long prompts | $2 input, $10 output; prompts over 272,000 input tokens cost twice as much for input and 1.5 times for output | 
| Cheapest plan that includes it | Claude Pro, $20 a month or $17 billed yearly | ChatGPT Free: GPT-6 Luna in the desktop app for quick coding tasks, subject to rollout | 
| Sandbox on a fresh install | off | on | 

Sources: Claude Code model configuration, Claude API pricing, Codex models, GPT-6.1 Sol.

## What changed since September

| Date | Claude Code | Codex | 
|---|---|---|
| 3 Sep | — | GPT-6 Astra released | 
| 18 Sep | Claude Code reads `AGENTS.md` from v2.1.277 when a project has no`CLAUDE.md` | — | 
| 22 Sep | Opus 5.5 released and made the default model | GPT-6 Sol and Luna released | 
| 25 Sep | v2.1.283 starts interactive sessions in auto mode | — | 
| 28 Sep | Sonnet 5.5 released | — | 
| 29 Sep | — | GPT-6.1 Sol released; a cut to the Pro $200 allowance announced by email, later cancelled | 
| 7 Oct | Haiku 5.5 released; Max, Ultra and Team plans get monthly API credits | — | 
| 14 Oct | — | GPT-5.5 retired from Codex for ChatGPT sign-in | 
| 26 Oct | — | GPT-6.1 Terra released | 
| late Oct | Max 20x renamed to Ultra: same $200, 10 times Pro’s weekly usage, no five-hour limit for verified accounts | Twice the Work and Codex allowance on Pro 100 and Pro 500, and the higher of its two allowances for every Pro 200; ChatGPT and Codex allowance to be shared from December | 
| 9 Nov | Fable 5.2 released | — | 
| 12–13 Nov | v2.1.323, v2.1.325 | Codex 0.168.0 and 0.169.0 stable | 

Sources: Opus 5.5, Sonnet 5.5, GPT-6 Astra, GPT-6 Sol and Luna, GPT-6.1 Sol, GPT-6.1 Terra, the Ultra plan, ChatGPT Pro tiers, shared ChatGPT and Codex limits.

One thing to watch: OpenAI’s own page at chatgpt.com/codex/pricing still listed GPT-5.5 on 16 November, five weeks after it left Codex. The current plan page is learn.chatgpt.com/docs/pricing.

## Prices and usage limits

Both products come with a chat subscription. Claude Code is in every paid Claude plan; Codex is in every ChatGPT plan, including Free.

| Per month | Claude | ChatGPT with Codex | 
|---|---|---|
| $0 | Free, no Claude Code | Free, “quick coding tasks” in the desktop app | 
| $8 | — | Go, “lightweight coding tasks” | 
| $20 | Pro, or $17 a month billed yearly | Plus, with GPT-6.1 Sol | 
| $100 | Max: 5 times Pro per five hours and per week, plus $100 in API credits | Pro 100, plus $100 in API credits | 
| $200 | Ultra: 10 times Pro per week, no five-hour limit for verified accounts, plus $200 in API credits | Pro 200, plus $200 in API credits | 
| $500 | — | Pro 500, the only plan with Ultrafast mode, plus $500 in API credits | 
| Teams | Team Standard $20 or $25 a seat, Premium $100 or $125 | Business $20 or $25 a seat; the Premium seat price is not published | 

Sources: claude.com/pricing, learn.chatgpt.com/docs/pricing, ChatGPT Pro tiers.

The API credits on the higher plans do not pay for the coding tools themselves. Anthropic’s credits do not cover interactive Claude Code, and OpenAI’s cannot be spent on Codex through an API key.

In late October OpenAI doubled the Work and Codex allowance on Pro 100 and Pro 500. Pro 200 had come with two different allowances; the lower one doubled to match the higher, and Plus did not change. OpenAI’s help page on Pro tiers adds that “a larger allowance does not guarantee twice as many completed tasks”.

Neither vendor publishes hard numbers for its limits. Anthropic states every plan as a multiple of Pro. OpenAI gives estimates for Plus and Business as local messages per five hours: 15 to 160 with GPT-6.1 Sol, 35 to 285 with GPT-6.1 Terra, 5 to 45 with GPT-6 Astra, and 350 to 3,000 with GPT-6 Luna. It says Pro “currently” has no five-hour limit and that weekly limits “may also apply”. From December the Plus, Pro and Business allowance will be shared between ChatGPT and Codex and measured in tokens. Instant chat will not draw from it; chat with the other models will draw on the same allowance as coding.

Ultra has no five-hour limit for verified accounts, and OpenAI’s Pro plans have none for now. Neither vendor publishes a weekly number, so Anthropic’s multiples of Pro and OpenAI’s estimates for Plus and Business are the only official figures.

## Which one writes better code

No public test runs the same model inside both products. Claude Code runs Anthropic’s models and Codex runs OpenAI’s, so every “Claude Code vs Codex” number is a model and an agent measured together.

The fairest public source is the official Terminal-Bench 4.1 leaderboard, where each row names the agent and the model and each entry ran 330 trials:

| Agent and model | Tasks solved | Cost of the full run | Tokens | 
|---|---|---|---|
| Claude Code, Opus 5.5 at max effort | 65.8% | $4,719 | 8.03 billion | 
| Claude Code, Sonnet 5.5 at max effort | 63.9% | $7,334 | 19.41 billion | 
| Codex, GPT-6.1 Sol at max effort | 60.9% | $634 | 1.46 billion | 
| Codex, GPT-6 Astra at max effort | 60.3% | $3,267 | 1.53 billion | 

The confidence interval is about 3 points either way, so the gap between the best rows is real but small. The cost gap is not small: the Codex run cost about one seventh as much.

Two other public sources point the same way. Artificial Analysis scores Claude Code with Opus 5.5 at 62 on its Coding Agent Index, Codex with GPT-6 Astra at 55 and Codex with GPT-6.1 Sol at 54. Vals AI runs the models through the same neutral agent, mini-SWE-agent, on Terminal-Bench 4.0: Opus 5.5 solved 65.2% and GPT-6.1 Sol 55.1%. That gap comes from the models alone.

Pages that still compare the two on SWE-bench Verified are out of date. Neither vendor publishes Verified scores for its current models, and OpenAI wrote in July that about 30 percent of its tasks are broken.

## Cost and speed of one task

The same sources record what each run cost at API prices, and there the gap is much wider than in the scores. On Terminal-Bench 4.1 the Codex run with GPT-6.1 Sol cost $634 against $4,719 for Claude Code with Opus 5.5, and the leaderboard’s data puts an average trial at 53 minutes against 80. Artificial Analysis measured $11.42, about an hour and 14.5 million tokens per task for Claude Code with Opus 5.5, against $7.04, 25 minutes and 3.2 million tokens for Codex with GPT-6 Astra, and $1.50 per task for Codex with GPT-6.1 Sol. On Vals AI’s neutral agent a test cost $12.04 with Opus 5.5 and $1.72 with GPT-6.1 Sol.

These are API prices. On a subscription you pay a flat fee, and neither vendor says how many such tasks a plan covers.

Both products can also run subagents on a cheaper model than the main session, which can lower the cost of a task. Our guide to model routing for coding agents covers when that pays off and when a change of model costs more than it saves.

Both also sell faster modes at a higher price:

|  | Claude Code | Codex | 
|---|---|---|
| Fast mode | up to 2.5 times faster at twice the price; on a subscription it is paid from usage credits only | uses your included limits 2.5 times faster, or credits at twice the rate | 
| Ultrafast | — | up to 8 times faster; 8 times the included usage, 6 times in credits; Pro 500 and Enterprise only | 

Sources: Claude Code fast mode, Codex pricing.

## Features that actually differ

Many features now exist in both: `/goal`, worktrees, cloud tasks, code review on GitHub pull requests, skills, MCP servers, plugins, and controlling a local session from your phone. These are the ones that still differ.

|  | Claude Code | Codex | 
|---|---|---|
| Sandbox on a fresh install | off; sessions start in auto mode, where a classifier reviews actions | on; no network, writes limited to the project folder | 
| Native Windows | commands run without a sandbox; WSL 2 is recommended | its own Windows sandbox | 
| Hooks | 33 events, 5 handler types | 14 events | 
| Undo file changes | `/rewind` restores checkpoints | no file undo; `/fork` and`/side` branch the chat | 
| Agent teams that message each other | yes, experimental | not documented | 
| Subagents | start on their own from the agent’s description and can have their own permissions | start on request and inherit the parent’s sandbox | 
| Instructions file | `CLAUDE.md` , and`AGENTS.md` when there is no`CLAUDE.md` | `AGENTS.md` ;`CLAUDE.md` only if you add it to`project_doc_fallback_filenames` | 
| Source code | not open source | CLI, SDK and app server under Apache-2.0; the IDE extension and the cloud are closed | 

Sources: Claude Code sandboxing, permission modes, hooks, Codex Windows sandbox, Codex AGENTS.md, Codex open source, Claude Code license.

The sandbox default matters most in practice. A fresh Codex install cannot reach the network or write outside your project until you allow it. A fresh Claude Code install relies on a classifier to judge each action, and you turn the OS sandbox on with `/sandbox`.

## Where each one runs

|  | Claude Code | Codex | 
|---|---|---|
| Terminal | macOS, Linux, Windows; native installer, Homebrew, WinGet, apt, dnf, apk | macOS, Linux, Windows; installer, npm, Homebrew | 
| VS Code | 30.31 million installs, rated 3.70 by 855 people | 16.81 million installs, rated 2.64 by 941 people | 
| Desktop | Code tab in the Claude app: macOS, Windows, Linux in beta | Codex mode in the ChatGPT desktop app since 9 July: macOS, Windows, Linux in preview since 11 August | 
| Web and cloud | claude.ai/code on Anthropic’s VMs; self-hosted runners in beta for Team and Enterprise | Codex Cloud, ChatGPT sign-in only | 
| Phone | Code tab in the Claude app | Codex in the ChatGPT app | 
| Slack | Claude Tag, Team and Enterprise | `@ChatGPT` in Slack and Microsoft Teams | 

Sources: Claude Code setup, Claude Code on the web, Codex CLI, VS Code Marketplace API on 16 November.

## Which one more people use

| Counter, 16 November 2026 | Claude Code | Codex | 
|---|---|---|
| npm downloads, last 28 days | 56.8 million | 103.2 million | 
| Homebrew installs, last 30 days | 57,665 | 467,139 | 
| Homebrew installs, last 365 days | 1,163,659 | 1,349,753 | 
| VS Code Marketplace installs, all time | 30,307,585 | 16,811,690 | 
| GitHub stars | 155,840 | 135,486 | 
| GitHub stars added since 15 September | +7% | +78% | 

Sources: npm @openai/codex, npm @anthropic-ai/claude-code, Homebrew codex, Homebrew claude-code, openai/codex, anthropics/claude-code, checked 16 November.

Codex has had more monthly npm downloads than Claude Code since June. npm also counts CI runs, and Claude Code ships a native installer that skips npm, so these counts favour Codex. We left out May: Codex npm downloads hit 46 million on 2 May alone and dropped back to the usual level within a week. That spike was not people installing Codex.

The GitHub issue trackers show another difference. In the 90 days to 11 November Claude Code got 22,570 new issues and closed 21,341. Codex got 17,684 and closed 2,893. On 16 November Codex has 27,631 open and Claude Code 17,000.

## Outages in the last 90 days

| 13 August to 11 November | Claude Code | Codex | 
|---|---|---|
| Status page incidents on the product’s components | 32, 2 of them critical | 15, one full outage of 56 minutes on 25 September | 
| Published uptime | not shown in the status API | 99.94% for CLI, VS Code, web and API; 99.91% for the desktop app | 
| Versions released | 79; v2.1.323 and v2.1.325 followed on 12–13 November | 36 stable and 259 alpha; 0.168.0 and 0.169.0 stable followed on 13 November | 

Sources: status.claude.com, status.openai.com.

Read the incident counts with care. Anthropic attaches nearly every model incident to Claude Code, and OpenAI attaches fewer model incidents to Codex, so part of the gap is how each vendor labels its status page. Anthropic published one postmortem on Claude Code quality this year, on 23 April. OpenAI has not published a formal one for Codex in 2026.

## What developers say on Reddit

We label Reddit threads about AI coding every day since 18 August, and the same data feeds our AI coding dashboard. Between 18 August and 11 November, 43,444 on-topic threads mentioned Codex or Claude Code. Reddit posters are a small and vocal part of either tool’s users, so this section shows what people argue about, not how many use which tool.

Usage limits come up on both sides. “The amount of work that can be done is at least 2/3 less than the previous week,” a Claude Max 20x subscriber wrote in r/ClaudeCode on 27 October, after using up the weekly allowance in less than 2.5 days. “Ok, I’m a bit fed up with Astra today and I’m now out of usage again,” a Codex user wrote in r/codex on 6 October.

Since late October refusals have become a theme of their own: safeguards around reverse engineering a TV in r/ClaudeAI, and a new rule on games that a board game publisher says “shut down my small business overnight”.

The share of negative threads stayed lower for Claude Code all along: 13 to 30 percent of threads about it each week, against 39 to 61 percent for Codex. Communities like r/codex and r/ClaudeCode attract complaints, so these shares describe the conversation in those places more than the products.

For a longer view, our count of 66,320 Reddit complaint posts about AI coding agents found that by September 2026 the bill for a run had become the most common complaint about Claude Code, and token consumption made up a quarter of complaint posts about Codex, the highest share for any tool we track.

## Using both

OpenAI publishes an official Codex plugin for Claude Code, released on 30 March and starred about 35,000 times on GitHub. It adds `/codex:review`, `/codex:adversarial-review`, `/codex:rescue` and a few more commands. It calls your local Codex CLI and spends your Codex limits; any ChatGPT plan works, including Free.

```
/plugin marketplace add openai/codex-plugin-cc
/plugin install codex@openai-codex
```
The other direction exists too. Codex has an `/import` command that copies instructions, settings, skills, plugins, projects and recent chats from Claude Code, and it accepts Claude Code plugin manifests. To share one instructions file, keep `AGENTS.md`: Claude Code reads it when there is no `CLAUDE.md`. Skills use the same `SKILL.md` format but live in different folders, `.claude/skills` and `.agents/skills`. MCP settings do not carry over, because Claude Code uses JSON in `.mcp.json` and Codex uses TOML in `config.toml`.

Several workspace apps run both agents side by side; our guide to 36 AI coding agents describes them.

## Using your plan outside the vendor’s app

The two vendors take opposite positions here.

OpenAI lets other apps use a ChatGPT plan through Sign in with ChatGPT, and lists open-source agents such as OpenCode among the supported integrations.

Anthropic’s terms for Claude Code say it “does not permit third-party developers to offer Claude.ai login into their own applications, or to route requests through Free, Pro, Max, or Ultra plan credentials on behalf of their users.” The same page allows “an end user from signing in to the unmodified Claude Code binary with their own Claude subscription”, including when another platform hosts it. Anthropic’s support page confirms that the Agent SDK and `claude -p` draw from your subscription limits when you sign in with your plan.

This is how OpenChamber’s Claude Code integration works: it runs the Claude Code you installed, signed in with your own account.

## FAQ

### Is Codex better than Claude Code?

On public benchmarks Claude Code with Opus 5.5 solves slightly more tasks: 65.8 percent against 60.9 percent for Codex with GPT-6.1 Sol on Terminal-Bench 4.1. Codex completes the same run for about one seventh of the cost.

### What is the difference between Claude Code and Codex?

Claude Code is Anthropic’s coding agent and runs Anthropic’s models, Opus 5.5 by default. Codex is OpenAI’s coding agent and runs OpenAI’s models, GPT-6.1 Sol by default on Plus and above. Both work in the terminal, in VS Code, in a desktop app, on the web and on a phone. Codex sandboxes by default, has an open-source CLI and lets other apps use your ChatGPT plan. Claude Code has more hook events, file checkpoints with `/rewind`, experimental agent teams and a 1-million-token context window by default.

### Which is better on a $20 plan, Codex Plus or Claude Pro?

Neither vendor publishes hard limits. OpenAI estimates 15 to 160 messages per five hours with GPT-6.1 Sol on Plus; Anthropic publishes no numbers for Pro. In Artificial Analysis’s runs Codex with GPT-6 Astra used 3.2 million tokens per task against 14.5 million for Claude Code with Opus 5.5, but neither vendor states its allowance in tokens, so that does not convert into tasks per plan. From December, Codex on Plus shares its allowance with ChatGPT chat, except Instant chat.

### Codex Pro or Claude Max: which $100 or $200 plan gives more?

Neither vendor puts a number on these plans. Anthropic’s Max gives 5 times Pro’s usage and Ultra 10 times Pro’s weekly usage, with no five-hour limit for verified Ultra accounts. OpenAI’s Pro plans have no five-hour limit for now; in late October OpenAI doubled the Pro 100 allowance and brought every Pro 200 subscription up to the higher of its two allowances. All four plans include monthly API credits equal to their price, which do not pay for interactive Claude Code or for Codex used with an API key.

### Which has the larger context window?

Claude Code: 1 million tokens on Opus 5.5, Sonnet 5.5, Haiku 5.5 and Fable 5.2. Codex: 500,000 tokens for GPT-6.1 Sol by default, according to OpenAI’s docs; the same model takes 1,050,000 tokens through the API.

### Is Codex or Claude Code more popular?

Codex has more npm and Homebrew installs: 103.2 million npm downloads in the last 28 days against 56.8 million. Claude Code has more VS Code installs, 30.3 million against 16.8 million, and more GitHub stars.

### Is Codex outdated?

No. Codex is OpenAI’s current coding agent: GPT-6.1 Sol, released on 29 September, is its default model on Plus and above, and Codex shipped 36 stable versions between 13 August and 11 November, with 0.168.0 and 0.169.0 following on 13 November. On Terminal-Bench 4.1 it trails Claude Code by about 5 points and finishes the same run for about one seventh of the cost.

### Does Claude Code read AGENTS.md?

Yes, since version 2.1.277 on 18 September 2026, when the project has no `CLAUDE.md`. You can change this in `/config`.

### Can I use Codex inside Claude Code?

Yes. Install OpenAI’s official `codex-plugin-cc` plugin. It runs your local Codex CLI and uses your Codex limits.

### Is Codex free?

Codex is included in ChatGPT Free for quick coding tasks and in Go for lightweight coding tasks, in both cases in the desktop app with GPT-6 Luna, subject to rollout. Claude Code needs a paid Claude plan or an API key.

## How we checked

- Prices, limits, features and terms come from the vendors’ own documentation and help pages, re-checked on 16 November 2026.
- Benchmark numbers come from the Terminal-Bench 4.1 leaderboard page, Artificial Analysis and Vals AI as published on 11 November 2026. We did not run either product ourselves.
- Install counts come from the npm, Homebrew, VS Code Marketplace and GitHub APIs on 16 November 2026.
- Incident counts come from both status pages for 13 August to 11 November 2026.
- Reddit figures come from our daily labelling of AI coding threads between 18 August and 11 November 2026. Weekly shares of negative threads count only threads labelled by gpt-5.4-mini, so the labelling stays the same across the whole window. Quotes are copied from the threads they link to.
