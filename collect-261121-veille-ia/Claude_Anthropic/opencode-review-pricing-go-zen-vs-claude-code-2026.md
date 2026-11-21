---
id: collect-261121-veille-ia/claude-anthropic/opencode-review-pricing-go-zen-vs-claude-code-2026
title: "OpenCode Review: Pricing, Go & Zen, vs Claude Code (2026)"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-11-13"]
keywords: ["opencode", "code", "anomaly", "terminal", "november", "coding", "agent", "files"]
source: https://stratoai.pro/en/ai-directory/opencode/
source_anchor: ""
source_lines: [1, 242]
section: "Claude_Anthropic"
---
**OpenCode is an open-source coding agent**: it reads your project, edits files and runs commands with whatever AI model you choose, and you can use it from a desktop app, the terminal, your phone or your own code. It’s made by **Anomaly**, it’s MIT-licensed, the current release is **v2.3.0 (November 13, 2026)** and it’s free; if you want more models, the **OpenCode Go** subscription starts at **$10 a month**. I use it in my own coding workflow, I set up coding agents and I train engineering teams. Here’s what it is today, what it really costs and when I’d point you to Claude Code instead.

## What OpenCode is today

The official site puts it this way: “OpenCode is an open source coding agent. Use it in the desktop app, in the terminal, on your phone or from your own code with the SDK.” If you thought of it as a terminal tool, **OpenCode 2** is an agent with four ways in, plus a web interface:

| Where | What it is | Platforms | 
|---|---|---|
| **Desktop** | App with sessions, tabs, diffs, a built-in browser and extensions | macOS, Windows and Linux (x64 and ARM64) | 
| **Terminal** | Text interface that works in any shell, editor or remote server | macOS, Windows and Linux (glibc and musl) | 
| **Mobile** | Start, steer and approve sessions from your phone | iPhone (through TestFlight, Apple’s beta channel) and Android (Google Play) | 
| **SDK** | Drive OpenCode from your own scripts, CI jobs and tools | Your own code | 
| **Web** | Web interface you open with the `opencode pair` command | Browser | 

What it does, according to its site and its repository:

- **Two built-in agents** you switch between with the Tab key:**build** , with full access for development work, and**plan** , a read-only agent that by default doesn’t edit files and asks permission before running commands. There’s also a**general** subagent for searches and multistep tasks, which you call with`@general` .
- **Any model** : “200+ LLM providers through Models.dev, including local models.”
- **Multiple sessions** : run several agents in parallel on the same project.
- **Customization** : plugins, local and remote MCP servers, agents with their own prompt, model and tools, skills in`SKILL.md` files, custom commands, and permissions that decide what runs automatically and what needs your approval.
- **Undo** :`/undo` rolls back recent changes to your files and the conversation.

**The latest release (v2.3.0).** Subagents now run in the background by default. The changelog says: “Subagents now return immediately, continue in the background, and report back whenever they become idle,” so you can keep working in the main session while several subagents run at once. The same release collapses each answer’s intermediate steps into a single row in the terminal and adds a search in the desktop app to jump back to an earlier message.

## Who makes it: Anomaly (and what happened to “OpenCode SST”)

If you knew it as “SST’s OpenCode,” you’ve got the right tool: the repository used to live at `sst/opencode`, and that address now **redirects to github.com/anomalyco/opencode**. The site is signed “©2026 Anomaly,” and the privacy policy spells it out: “OpenCode is provided by Anomaly Innovations, Inc.” The homepage reads: “MIT licensed. Built in the open by Anomaly and 1,092 contributors.”

The repository’s numbers, pulled from the GitHub API on November 16, 2026:

| Item | Value | 
|---|---|
| License | MIT | 
| Stars | 227,059 | 
| Forks | 30,759 | 
| Created | April 30, 2025 | 
| Current release | v2.3.0 (November 13, 2026); before that, v2.2.7 on November 10 and v2.2.6 on November 6 | 

The site also claims it’s used by “over 22M developers every month.” That’s the company’s own number, and I can’t audit it. I also couldn’t verify who its founders are or the legal relationship between SST and Anomaly: all I know for sure is that the repository moved and that everything official is signed by Anomaly.

## How to install it (and the Windows catch)

The official docs list these options for the terminal:

```
# Official installer
curl -fsSL https://opencode.ai/v2/install | bash
# npm (bun, pnpm, yarn or vp also work)
npm install -g @opencode/cli
# Homebrew
brew install anomalyco/tap/opencode-v2
# Arch Linux (AUR)
paru -S opencode-beta
```
Three details that will save you some time:

- **The npm package was renamed.** The version 2 package is`@opencode/cli` . The old one,`opencode-ai` , stopped at 1.19.5 (October 28, 2026). If you install with mise, you need version 2026.10.9 or later; older versions install OpenCode 1.
- **Windows.** The docs are blunt: “Windows package managers are not supported.” On Windows you download the x64 or ARM64 binary, or the desktop app.
- **Where it lands.** The installer puts it in`$HOME/.opencode/bin` . There’s also a Docker image (`ghcr.io/anomalyco/opencode` ) and a desktop app for all three operating systems.

**With your editor.** According to the docs, it integrates with VS Code, Cursor or any editor with a terminal, and there’s an extension for VS Code, Cursor, Windsurf and VSCodium. That docs page still belongs to version 1, so check it before assuming it works the same way in version 2.

Once it’s installed, `/connect` hooks up a model provider and `/models` picks the model. If you don’t connect anything, you can start with the free models: the site says “OpenCode comes with a set of free models. No account needed.”

## OpenCode pricing: free, OpenCode Go and Console (formerly Zen)

OpenCode itself is free. What you pay for is the model, and there are four ways to do that. Official prices from opencode.ai, the Go page and the Console docs, checked on November 16, 2026. **They’re in dollars**: the site doesn’t publish prices in euros.

| Option | Price | What you get | 
|---|---|---|
| **OpenCode** (desktop, terminal, mobile and SDK) | $0, MIT license | The full agent | 
| **Free models** | $0, no account needed | A lineup of free models for a limited time, with a daily request limit | 
| **Your own subscription or key** | Whatever you already pay your provider | Sign in with ChatGPT Plus or Pro or with GitHub Copilot, or connect your provider | 
| **OpenCode Go** | **$10/mo** | Curated open-weight models, with per-model limits | 
| **OpenCode Go Plus** | $40/mo | Everything in Go, with higher limits | 
| **OpenCode Go Teams** | $20 per member/mo | Each member gets their own limits, $5/month higher than Go on every model | 
| **Console** (formerly “Zen”) | Pay as you go, per million tokens | Paid models (Claude, GPT, Gemini, Kimi, DeepSeek, Qwen…) with a 4.6% + $0.31 fee on each credit top-up | 
| **Enterprise** | No public price | Single sign-on (SSO), budgets and policies for teams | 

### OpenCode Go: what’s included and how the limits work

Go is the most-searched part of OpenCode after the name itself: “opencode go” gets 4,400 searches a month in Spain, according to DataForSEO’s Google Ads data from November 16, 2026. Its page describes it as “a subscription that gives you and your team access to the best open-weight models with generous usage limits at a low-cost fixed monthly price.” It currently includes 32 models (Kimi K3.1 Pro, DeepSeek V4 Pro, GLM-5.3, Qwen3.8 Max, MiniMax M3, GPT 6 Luna, Claude Haiku 5.5, Gemini 4 Argon, Grok 4.7 and others), and the docs warn: “The list of models may change.” For a sense of real-world usage, the OpenCode Data page says that as of November 16, 2026, the most-used model on OpenCode over the previous 7 days was DeepSeek V4.1 Flash, with 157 trillion tokens, followed by MiniMax-M3.1-Flash (9.1 trillion) and GLM-5.3-Flash (7.8 trillion).

**How the limits are counted.** Each model has a monthly limit expressed in dollars of usage. You can spend at most 20% of that limit in any 5-hour window and 50% in a week. A few examples from the Go docs:

| Model | Input / output (per million tokens) | Monthly limit on Go | On Go Plus | 
|---|---|---|---|
| Kimi K3.1 Pro | $2.00 / $10.00 | $15 | $60 | 
| DeepSeek V4 Pro (off-peak) | $0.66 / $1.98 | $15 | $60 | 
| DeepSeek V4.1 Flash (off-peak) | $0.15 / $0.60 | $100 | $200 | 
| Qwen3.8 Flash | $0.15 / $0.47 | $30 | $90 | 
| GLM-5.3-Flash | $0.15 / $0.50 | $60 | $180 | 
| GPT 6 Luna (up to 272K tokens) | $0.10 / $0.50 | $15 | $60 | 
| Claude Haiku 5.5 (up to 100K tokens) | $0.10 / $0.50 | $15 | $60 | 

**What to know before you subscribe:**

- **Hitting the limit doesn’t stop you.** You can keep going on the free models or turn on “Extra Usage” so requests draw on your credit balance.
- **It works outside OpenCode.** The Go page says “Works in OpenCode, Claude Code, Codex and Pi,” and its docs also list Hermes among the validated clients. If you use Claude Code or Hermes Agent, you can give them Go’s models.
- **Go and Go Plus are for a single person.** For a workspace with several members there’s Go Teams, at $20 per member.
- **You can cancel any time** , and the plan stays active until the end of the period you paid for. You may qualify for a refund if you ask within 14 days of the charge and have barely used that period.
- **DeepSeek peak hours.** From 01:00 to 04:00 and from 06:00 to 10:00 UTC, Monday through Friday, its prices double.

### Console: what used to be called Zen

If you’re searching for “OpenCode Zen,” it now lives in Console: `opencode.ai/zen` redirects to `opencode.ai/console/models`. It’s a pay-as-you-go gateway, priced per million tokens. Some examples from the docs:

| Model | Input | Output | 
|---|---|---|
| Claude Opus 5.5 | $4.00 | $20.00 | 
| Claude Sonnet 5.5 | $2.00 | $10.00 | 
| Claude Haiku 5.5 (up to 100K tokens) | $0.10 | $0.50 | 
| GPT 5.5 (up to 272K tokens) | $5.00 | $30.00 | 
| Kimi K3 | $3.00 | $15.00 | 
| Qwen3.8 Max | $2.00 | $6.00 | 
| DeepSeek V4.1 Flash | $0.30 | $1.20 | 
| GLM 5.3 Flash | $0.15 | $0.50 | 

The docs are upfront about the business model: “A processing fee of 4.6% + $0.31 is added to each credit purchase to cover payment costs; we don’t charge anything beyond that.” Among its goals, it lists “selling at cost, with markup only to cover processing fees.” One detail that surprises people on the bill: OpenCode uses cheap models (Haiku, Nano or Flash) to title your sessions, so you’ll see them in your usage history.

**◇ Pricing in euros and VAT: not published.** OpenCode Go and Console list their prices in dollars, and I couldn’t confirm how they apply VAT to a Spanish business. Before you budget, check the final amount in your account.

## Privacy: where your code goes

Here’s what the privacy policy and the docs say, and what I couldn’t verify:

- **Your prompts go to the model provider.** The policy says: “Prompts are passed through to the upstream model provider to provide the services and are not stored, except by hosted agents,” and hosted agents keep sessions until 30 days without activity.
- **Console models.** “All our models are hosted in the US and EU. Our providers follow a zero-retention policy and do not use your data for model training,” with exceptions: several free models (Big Pickle, Iceman Free, Inkling 2 Small Free, Flint 40B Free, MiMo-V2.6-Flash Free) may use your data to improve the model or keep it; NVIDIA’s models (Nemotron Free) are for trial use only and ask you not to submit personal or confidential data; the OpenAI and Anthropic APIs retain requests for 30 days; and**LongCat 2.5 Free is hosted in China** , according to the same page.
- **OpenCode Go.** By default it blocks paid endpoints that train on your requests and allows the Europe and United States regions; note that workspaces without saved privacy settings also allow the Global region for requests from China. An admin can change that in the privacy settings; some models (the Muse Spark Contributor ones) can only be turned on if you allow them to train on your data.
- **The OpenCode website.** Since November 2026 it runs its own analytics: “We collect these records ourselves and don’t use third-party analytics services on opencode.ai.” You can turn it off, but it still records page views and clicks, just without your identifiers.
- **The app itself: not verified.** The policy covers the website, Console and hosted agents. I didn’t find a section in the version 2 docs on telemetry from the app you install.
- **Watch your logs.** Local logs live in`~/.local/share/opencode/log/` , and the docs warn that memory snapshots can contain “prompts, file contents, and credentials. Do not share it publicly.”

If you work with client code or in a regulated industry, my advice is simple: use zero-retention models, save Go’s privacy settings with the Europe and United States regions and keep confidential code away from the free models.

## OpenCode vs Claude Code

It’s one of the most-searched comparisons: “opencode vs claude code” gets 480 searches a month in Spain and 4,400 in the United States, according to DataForSEO’s Google Ads data from November 16, 2026. Here’s the short version, with official data from November 16, 2026:

|  | OpenCode | Claude Code | 
|---|---|---|
| **Made by** | Anomaly | Anthropic | 
| **Code** | Open source, MIT license | The public repository doesn’t declare an open-source license (the GitHub API detects none) | 
| **GitHub stars** | 227,059 | 155,796 | 
| **How you pay** | Free; you pay for the model (free models, your subscription, Go from $10/mo or Console pay-as-you-go) | Included in Claude Pro ($17/mo billed annually or $20 monthly), Max ($100) and Ultra ($200) | 
| **Models** | 200+ providers through Models.dev; you can also sign in with ChatGPT or GitHub Copilot | See its directory entry | 

**How I decide.** Claude Code and OpenCode both live in my shop, and it isn’t a cage match. If your team works with Claude models and already pays for Claude Pro or Max, Claude Code is included and it’s the path with the fewest moving parts. OpenCode wins when you want to pick the model for each task, make use of a ChatGPT or Copilot subscription you already pay for, code at a flat price with open-weight models (Go) or have a tool you can audit. And since Go also works inside Claude Code, it doesn’t have to be one or the other.

On Google you’ll find comparisons with numbers on speed, tokens or tests passed. I haven’t validated them, so I won’t repeat them: if performance is the deciding factor, try both on a real task in your own repository.

## OpenCode alternatives

Entry prices from each tool’s official pricing page, in dollars, checked on November 16, 2026.

| Alternative | Entry price | What it is | Heads-up | 
|---|---|---|---|
| **Claude Code** (Anthropic) | Included in Claude Pro: $17/mo billed annually or $20 monthly; Max $100; Ultra $200 | Anthropic’s coding agent | Its repository doesn’t declare an open-source license | 
| **Codex** (OpenAI) | Plus $20/mo (includes the CLI); Free $0 and Go $8 with GPT-6 Luna in the desktop app (subject to rollout); pay as you go with an API key | OpenAI’s coding agent | `openai/codex` repository under Apache-2.0 | 
| **Cursor** (Anysphere) | Free $0; Plus $8; Premium $30; Super $100; Ultra $200; Teams $30 per user | AI code editor | Its core product is the editor; its site also offers a CLI | 
| **Kilo Code** | Free and open source (VS Code, JetBrains and CLI), usage billed separately; Teams $15 per user/mo | AI coding extensions and CLI | MIT license; the OpenCode Go docs list its CLI among the validated clients | 
| **Aider** | Free (Apache-2.0); you pay for the model | AI pair programming in the terminal | Its latest release on PyPI is 0.86.2, from February 2026 | 

If what you’re after is the model rather than the tool, the directory has entries for Kimi, DeepSeek, Qwen and MiniMax, whose models are on OpenCode Go, plus OpenRouter, a gateway with lots of models behind a single key. To connect OpenCode to your tools, see the MCP servers entry. And if you’re interested in running models on your own machine, the OpenCode homepage mentions “local models”; I haven’t checked in this review how each local app connects in version 2, but the LM Studio entry explains how that kind of tool works.

## My operator’s verdict

OpenCode has 227,059 GitHub stars, more than Claude Code, Codex, Kilo Code or Aider, and it’s no longer “just a CLI”: desktop, terminal, mobile and SDK, background subagents and any model behind it. I use it in my coding workflow with OpenCode Go. My rule: I don’t merge anything an agent wrote without reviewing it first.

What I won’t hide from you: the free models are only free for a limited time and several of them keep your data, the app’s telemetry isn’t documented in anything I’ve read, everything is billed in dollars and there’s no package manager on Windows. My recommendation: start free on a repository that isn’t confidential, measure a real week of usage and then decide between your own subscription, Go or Console.

## I’ll set it up and train your team

I set up coding agents like OpenCode for engineering teams and train them to work with them without depending on me: picking the model and plan, configuring permissions and agents, connecting the MCP servers you need and setting review rules before anything reaches production.

- **If you want AI agents set up in your company:** AI agent implementation.
- **If you want me to teach you and your team to code with agents:** AI Mentoring.
- **If you’re not sure where to start:** tell me about your situation and I’ll tell you whether OpenCode is a fit or whether something else makes more sense.

## OpenCode FAQ

### What is OpenCode?

An open-source coding agent (MIT license) that reads your project, edits files and runs commands with the AI model you choose. You use it from a desktop app, the terminal, your phone or your own code through its SDK. The current release is v2.3.0, from November 13, 2026.

### Who is the founder of OpenCode?

OpenCode is made by Anomaly (Anomaly Innovations, Inc.). Its privacy policy says “OpenCode is provided by Anomaly Innovations, Inc.,” and the repository lives at `anomalyco/opencode`. I couldn’t verify the names of its founders.

### Is “OpenCode SST” the same as OpenCode?

Yes. The repository used to live at `sst/opencode` and now redirects to `anomalyco/opencode`; the website and the privacy policy are signed by Anomaly. I couldn’t verify the legal relationship between SST and Anomaly.

### Is OpenCode really free?

The app is: it’s open source under the MIT license, and installing it costs nothing. What costs money is the model. You can start with the free models, no account needed, which are free for a limited time and have a daily request limit; use a subscription you already pay for (ChatGPT Plus or Pro, GitHub Copilot) or your own key; or pay for OpenCode Go (from $10 a month) or Console (pay as you go).

### How much does OpenCode Go cost per month?

$10 a month for Go, $40 a month for Go Plus or $20 per member a month for Go Teams. It’s Anomaly’s subscription to curated open-weight models for coding. Each model has a monthly limit in dollars of usage, capped at 20% every 5 hours and 50% per week. You can cancel any time.

### What’s the difference between OpenCode Go and Zen?

Go is a flat-price subscription with per-model limits. Zen, now called Console, is pay as you go: you buy credit and pay for each million tokens at the published price, with a 4.6% + $0.31 fee per top-up. Go makes sense if you code every day with open-weight models; Console, if you want models like Claude Opus or GPT, or your usage is irregular.

### Is OpenCode worth it?

For me, yes: it’s free, open source and lets you choose both the model and how you pay. If your team works with Claude models and already pays for Claude Pro or Max, Claude Code is included in that plan and you probably don’t need another tool. I walk through it in the comparison above.

### OpenCode or Claude Code: which should I pick?

Claude Code, if you work with Claude models and already pay for Claude Pro ($17/mo billed annually or $20 monthly) or Max. OpenCode, if you want to choose the model, make use of ChatGPT or GitHub Copilot, code at a flat price with OpenCode Go or be able to audit the code. OpenCode Go also works inside Claude Code.

### How do I install OpenCode?

With the official installer (`curl -fsSL https://opencode.ai/v2/install | bash`), with npm (`npm install -g @opencode/cli`) or with Homebrew (`brew install anomalyco/tap/opencode-v2`). There’s also a desktop app for macOS, Windows and Linux, and a mobile app for iPhone and Android.

### How to run OpenCode on Windows?

Download the Windows binary (x64 or ARM64) or the desktop app from the official docs. Windows package managers aren’t supported; the docs say so word for word: “Windows package managers are not supported.”

### Does OpenCode send my code to third parties?

Your prompts, including any code in them, go to the model provider you choose. Anomaly says it doesn’t store them, except for hosted agents. On Console, providers follow a zero-retention policy with exceptions: several free models may keep your data or use it for training, the OpenAI and Anthropic APIs retain requests for 30 days, and LongCat 2.5 Free is hosted in China. I couldn’t verify what telemetry the app itself sends.

### Does OpenCode work with local models?

The site says it connects to “200+ LLM providers through Models.dev, including local models.” I haven’t checked the step-by-step for each local app in version 2 in this review, so test it on your machine before you plan a project around it.

### Can I use my ChatGPT or GitHub Copilot subscription?

Yes. The site puts it like this: “Log in with ChatGPT Plus or Pro or GitHub Copilot.” That way you use what you already pay for without buying OpenCode Go.

### How can I learn to code AI?

The official docs recommend Boot.dev’s “AI Coding with OpenCode” course, built in consultation with the OpenCode team: all of its content is free to read and watch, though some interactive features may require payment. If you’d rather learn on your own code with your own team, that’s exactly what I work on in AI Mentoring.

### Can you help me roll out OpenCode on my team?

Yes. I set up coding agents and train the engineering teams who’ll use them. You can see how I approach AI agents, look at AI Mentoring or message me directly.

**Official sources checked on November 16, 2026:** opencode.ai · Docs · Changelog · anomalyco/opencode repository · OpenCode Go and its docs · Console models and their docs · Enterprise · OpenCode Data · Privacy policy · Editor integration. Alternative pricing: Claude · Codex · Cursor · Kilo Code · Aider and its PyPI package.
