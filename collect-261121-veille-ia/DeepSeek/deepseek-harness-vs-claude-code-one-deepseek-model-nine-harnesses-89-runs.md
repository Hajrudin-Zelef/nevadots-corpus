---
id: collect-261121-veille-ia/deepseek/deepseek-harness-vs-claude-code-one-deepseek-model-nine-harnesses-89-runs
title: "DeepSeek Harness vs Claude Code: one DeepSeek model, nine harnesses, 89 runs"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: []
keywords: ["deepseek", "code", "minutes", "runs", "harness", "claude", "model", "opencode"]
source: https://www.productcompass.pm/p/deepseek-harness-vs-claude-code
source_anchor: ""
source_lines: [1, 169]
section: "DeepSeek"
---
DeepSeek Harness scored a little higher than Claude Code on the same DeepSeek model, but not by enough to call a winner. On my Bug Hunt Bench, DeepSeek V4.1 Flash 1017 at max effort fixed 42.7 of 105 planted bugs per run in DeepSeek Harness and 39.1 in Claude Code, seven runs each, at $1.84 against $1.01 a run. Pi, a small open-source harness, scored 42.2 for $0.83.

**The short answer:** the gap is inside run-to-run noise, and on the same model DeepSeek Harness cost about 1.8 times as much per run.

Below: the results for all nine harnesses on that one model, including OpenCode, Codex CLI and Hermes Agent; how Claude Code was pointed at DeepSeek; and the two agents feature by feature, from the vendors' own docs. I build AFK Pilot, a VS Code extension that runs DeepSeek Harness and Claude Code side by side, and it gets one section at the end. DeepSeek and Anthropic are not affiliated with it.

Last updated: November 5, 2026.

## The Bench: One Model, Nine Harnesses

Bug Hunt Bench hides 105 real bugs in two production codebases, 45 in one and 60 in the other. An agent gets one round per codebase to find and fix what it can, a judge grades every diff blind against an answer key the agent never sees, and the score counts planted bugs only. The board, the method and the per-run receipts are public at bughunt.productcompass.pm.

For this comparison the harness was the variable. The model was DeepSeek V4.1 Flash 1017 (DeepSeek's `deepseek-flash`), on DeepSeek's own API and one account, on the same two repositories, with web search and fetch off in every harness. Claude Code and DeepSeek Harness both called DeepSeek's Anthropic-compatible endpoint. Pi, OpenCode, OpenCode 2.0, Hermes Agent, Kilo Code and Grok Build CLI used its OpenAI-compatible endpoint, and Codex CLI its Responses endpoint. I read the effort on the wire for every request in eight of the nine harnesses. For Claude Code, an earlier capture shows its `--effort` flag becomes the request's effort field, and DeepSeek's endpoint validated the tier before every run. Grok Build CLI, for one, sends no effort to a custom model, so DeepSeek's default, high, applied.

Five runs were voided and rerun because they looked for the answers outside their own folder: two Claude Code, one DeepSeek Harness and two Grok Build CLI. The 89 runs and every mean here count clean runs only. An integrity warning was added to the task after the first three of those; Claude Code carried it on only 4 of 7 runs at max and 3 of 5 at high and at low, and its runs with and without it differ in no consistent direction.

### At max effort

- **DeepSeek Harness:** 42.7 of 105 over 7 runs (worst 39, best 47), 62 minutes and $1.84 a run.
- **Pi:** 42.2 over 5 runs (41 to 44), 54 minutes, $0.83.
- **Claude Code:** 39.1 over 7 runs (34 to 46), 58 minutes, $1.01.
- **OpenCode:** 37.6 over 5 runs (35 to 40), 56 minutes, $0.96.

### At high effort

- **Pi:** 40.4 (38 to 44), 48 minutes, $0.64.
- **Grok Build CLI:** 40.4 (34 to 44), 93 minutes, $1.36.
- **DeepSeek Harness:** 39.2 (32 to 45), 53 minutes, $1.27.
- **OpenCode 2.0:** 38.6 (35 to 43), 50 minutes, $1.04.
- **OpenCode:** 36.6 (28 to 43), 42 minutes, $0.79.
- **Claude Code:** 36.4 (30 to 42), 54 minutes, $0.82.
- **Codex CLI:** 33.2 (30 to 35), 232 minutes, $3.97. Four of its five runs hit the bench's three-hour limit on one repository and were scored as they stood, so its score is a floor and its minutes are mostly that limit.
- **Hermes Agent:** 32.0 (24 to 38), 70 minutes, $0.97.
- **Kilo Code:** 31.8 (24 to 35), 92 minutes, $1.07.

### At low effort

- **DeepSeek Harness:** 34.8 (34 to 36), 34 minutes, $1.06.
- **Claude Code:** 34.4 (32 to 40), 44 minutes, $0.71.
- **Pi:** 33.8 (25 to 38), 33 minutes, $0.46.
- **OpenCode:** 32.8 (27 to 38), 41 minutes, $0.66.

Five runs per row at high and low. Versions: dsh 0.2.1-rc.2, Claude Code 2.1.302, Pi (pi-coding-agent) 1.3.0, OpenCode 1.19.3, OpenCode 2.0.32, Codex CLI 0.159.0, Hermes Agent 0.22.2, Kilo Code 7.8.12, Grok Build CLI 1.0.62. Minutes are wall clock on a shared machine, where runs often ran side by side. Costs are per run at DeepSeek's off-peak rates. For the four harnesses on the public board (DeepSeek Harness, Claude Code, Pi and OpenCode 1.19.3), the means, minutes and costs are the board's own figures, so you can check them against every run in its runs.csv. For the other five, cost is DeepSeek's list price on DeepSeek's own token counts, which matched the account balance within about 1%; they ran in the same wave and are in my October 20 write-up. At DeepSeek's peak hours (01:00-04:00 and 06:00-10:00 UTC on weekdays) the same runs cost twice as much.

## Is the Gap Real?

Not on its own. At max effort the gap is 3.6 points, and the runs overlap. Claude Code's seven scored 34, 37, 45, 46, 35, 39 and 38. DeepSeek Harness's scored 42, 41, 47, 47, 40, 43 and 39. Two Claude Code runs beat DeepSeek Harness's average.

**The test:** shuffle those 14 scores between the two labels, and a gap of 3.6 or more, either way, comes up 14% of the time (an exact permutation test, p = 0.14). A bootstrap puts the gap's 95% range at −0.4 to +7.3 points. Seven runs a side cannot separate them.

What held is the direction. DeepSeek Harness led Claude Code at all three effort levels, by 0.4 points at low, 2.8 at high and 3.6 at max, and it was steadier at max (a standard deviation of 3.2 points against 4.7). Its worst max run, 39, is level with Claude Code's max average of 39.1. Pooled across the three effort levels, with a permutation test within each level, the edge is borderline (p = 0.09). That is the fair reading of my October 18 post's "2-3 points on average": a consistent lean, not a proven gap.

The clearest separations sit further down. At max, DeepSeek Harness and Pi both beat OpenCode outside the noise (p = 0.02 and p = 0.01), and every Pi run, 41 to 44, scored above OpenCode's best, 40.

One setup difference to weigh. Claude Code ran with its subagent tool switched off, the bench's setting for every Claude Code row, while DeepSeek Harness ran with subagents on as shipped and launched 35 of them across its seven max runs. Pi has no subagents at all and scored 42.2, so subagents alone do not explain the gap.

## What It Costs to Run the Same Model

At max effort DeepSeek Harness cost $1.84 a run and Claude Code $1.01: 1.8 times as much for 3.6 more planted bugs. It read and wrote more. Per run it sent 2.55 million uncached input tokens against Claude Code's 1.04 million and 316.5 million cached against 207.5 million, and got back 0.85 million output tokens against 0.41 million. Both served over 99% of their input from DeepSeek's cache.

It also fixed about twice as many real bugs nobody planted: 44 a run against 23 at max. The bench shows those as extras and never adds them to the score.

Pi was the cheapest harness at every effort level, at $0.83, $0.64 and $0.46 a run. Codex CLI was the outlier the other way: $3.97 a run at high, with 94.1% of its input cached against 98.7% or more for every other harness, and 232 minutes a run, mostly the three-hour limit it hit on four of five runs.

This measures harnesses on one model, not the vendors' own models. For scale, on the same board and at API list price, Claude Code running Anthropic's Sonnet 5.5 at max effort scored 51.3 at $154.03 a run, and Codex CLI with OpenAI's GPT-6 Astra at max scored 45 at $33.03.

## How Claude Code Ran on a DeepSeek Model

Claude Code works with any server that speaks Anthropic's Messages API, and DeepSeek runs one at `https://api.deepseek.com/anthropic`. DeepSeek's own Claude Code guide (read November 5, 2026) sets these variables, then starts `claude` in your project, on macOS or Linux:

```
export ANTHROPIC_BASE_URL=https://api.deepseek.com/anthropic
export ANTHROPIC_AUTH_TOKEN=<your DeepSeek API key>
export ANTHROPIC_MODEL=deepseek-flash[1m]
export ANTHROPIC_DEFAULT_OPUS_MODEL=deepseek-flash[1m]
export ANTHROPIC_DEFAULT_SONNET_MODEL=deepseek-flash[1m]
export ANTHROPIC_DEFAULT_HAIKU_MODEL=deepseek-flash
export CLAUDE_CODE_SUBAGENT_MODEL=deepseek-flash
export CLAUDE_CODE_EFFORT_LEVEL=max
export CLAUDE_CODE_AUTO_COMPACT_WINDOW=786432
claude
```
On Windows, set the same variables in PowerShell, as `$env:ANTHROPIC_MODEL="deepseek-flash[1m]"` and so on. Three things to know:

- **Name the model.** DeepSeek maps any claude-opus model name to DeepSeek V4 Pro, billed at V4 Pro's price, and claude-sonnet and claude-haiku names to deepseek-flash. Setting every tier to deepseek-flash, as above, keeps the session on V4.1 Flash.
- **You pay DeepSeek.** Usage bills to your DeepSeek account per token, at the prices further down, not to a Claude plan.
- **Remote Control is off.** Anthropic's Remote Control needs a claude.ai subscription sign-in and does not work when ANTHROPIC_BASE_URL points at a host other than api.anthropic.com (Remote Control docs, read November 5, 2026).

The bench used this endpoint with `--model deepseek-flash[1m]` and `--effort` set per row.

## DeepSeek Harness vs Claude Code, Feature by Feature

From each vendor's own pages, read November 5, 2026.

- **Made by.** DeepSeek Harness (dsh): DeepSeek. Claude Code: Anthropic.
- **License.** DeepSeek Harness is open source under the MIT license (GitHub). Claude Code is proprietary: its repository's license reads "© Anthropic PBC. All rights reserved."
- **Status.** DeepSeek Harness is in developer preview. All 37 of its GitHub releases are prereleases, the latest 0.2.2-rc.1 on November 4, 2026, and its README warns: "THERE WILL BE COMPATIBILITY-BREAKING CHANGES." Claude Code is a released product whose native installs update themselves.
- **Where it runs.** DeepSeek Harness: a CLI with headless and SDK profiles, a local Web UI (`npx @deepseek-ai/dsh web` , on port 3080) and a desktop app for macOS on Apple silicon and Windows x64 (product page). It has no official VS Code or JetBrains extension and no phone app for the harness (DeepSeek's iPhone and Android app is its chat assistant); the Web UI improved its phone and tablet support in 0.2.2-rc.1 and can be published behind your own reverse proxy. Claude Code: the terminal, an extension for VS Code and Cursor, a JetBrains plugin, a desktop app for macOS and Windows with a Linux beta, the web at claude.ai/code, the Claude app for iOS and Android, and Remote Control (overview).
- **Models.** DeepSeek Harness: DeepSeek's models first, plus built-in providers such as Anthropic, OpenAI, Kimi and GLM, and any custom endpoint (providers guide). Claude Code: Anthropic's Claude models; its CLI, VS Code and JetBrains surfaces also take third-party providers and compatible endpoints such as DeepSeek's.
- **Pricing.** DeepSeek Harness is free; you pay the model provider. On DeepSeek's API that is pay-as-you-go: deepseek-flash costs $0.15 per million input tokens on a cache miss, $0.003 on a cache hit and $0.60 per million output tokens off-peak, twice that at peak (DeepSeek pricing). Claude Code is included in Claude Pro ($20 a month, or $17 billed annually), Max ($100) and Ultra ($200), not in Free (Claude pricing), or runs on an API key billed per token, with Opus 5.5 at $4 in and $20 out per million.
- **Extending it.** DeepSeek Harness: "Everything is a plugin." Plugins, installed or created through chat in Creator mode, extend its tools, skills and interface. Claude Code: plugins that bundle skills, subagents, hooks and MCP servers from a marketplace (`/plugin` ), and each of those also works on its own (plugins docs).
- **Subagents.** DeepSeek Harness: built in, and it can hand tasks to Claude Code or Codex as subagents (installable bundles since 0.1.0-rc.8; a preview subagent_oar plugin for local Codex and Claude CLIs in 0.2.2-rc.1). Claude Code: subagents coordinated by a lead agent.

**They are not either-or:** DeepSeek Harness can run Claude Code inside it, and Claude Code can run DeepSeek's models.

## OpenCode, Codex CLI, Hermes and Pi on the Same Model

DeepSeek's API docs list nine agent tools under Agent Integrations, each with a setup guide: DeepSeek Harness, Claude Code, Codex, OpenCode, OpenClaw, Hermes, Reasonix, WorkBuddy/CodeBuddy and Qoder. Here is how the others on the bench did with the same model.

- **OpenCode (Anomaly, MIT):** 37.6 at max, 36.6 at high and 32.8 at low: below DeepSeek Harness at every effort level and level with Claude Code at high. At max its 5.1-point gap to DeepSeek Harness is outside the noise. Anomaly's rebuilt OpenCode 2.0 scored 38.6 at high.
- **Codex CLI (OpenAI, Apache-2.0):** 33.2 at high, and that is a floor: four of its five runs hit the bench's three-hour limit on one repository and were scored as they stood, which is also why it averaged 232 minutes. At $3.97 a run it was the most expensive harness in the set. It reached DeepSeek through DeepSeek's Responses endpoint, the route DeepSeek documents for Codex.
- **Hermes Agent (Nous Research, MIT):** 32.0 at high (24 to 38), 70 minutes and $0.97 a run, through its built-in DeepSeek provider.
- **Pi (earendil-works, MIT):** a minimal agent with four tools (read, bash, edit, write) and no subagents. 42.2 at max for $0.83: 0.5 points behind DeepSeek Harness at under half the cost, and the cheapest at every effort level.
- **Grok Build CLI and Kilo Code:** 40.4 and 31.8 at high.

**If score per dollar on this model is the question, Pi led.** If you want DeepSeek's own stack, DeepSeek Harness scored at least level with Claude Code on this model, at a higher cost per run.

## Run DeepSeek Harness and Claude Code Side by Side

The fair test of two harnesses is your own repository. AFK Pilot, the free extension I build, runs DeepSeek Harness and Claude Code in one VS Code or Cursor sidebar, each in its own conversation, next to Grok Build, Muse Code, Codex and Antigravity. Grok Build Desktop (Community) runs the same agents as a desktop app.

1. Install AFK Pilot: search Extensions for AFK Pilot or the ID `grok-vscode-phuryn` . It works in VS Code 1.106+ and Cursor.
2. Turn on DeepSeek in Settings → Advanced (it's off by default), then connect it in Settings → Providers with a DeepSeek API key. The app installs a pinned copy of DeepSeek's own harness with its own Node, and the key stays in the app's secure storage.
3. Connect Claude Code in Settings → Providers. It signs in through its own CLI.
4. In a DeepSeek conversation, pick the harness in the context popup before the first message: Default, dsh's standard agent, which is built to ask before the actions dsh flags (in my tests on Windows it never asked, and its shell commands there run without dsh's sandbox), or Minimal, a lean tool set with full access and no approval prompts. Then pick DeepSeek V4.1 Flash 1017 and the effort in the model chip.

From your phone: link the computer once at afkpilot.com, then open either conversation from any browser. Turning DeepSeek on, installing and connecting it happen at the computer; after that a linked phone can chat with it and choose its harness. DeepSeek runs on your own computer only, not on AFK Pilot's Cloud machines, where Claude Code does run. The extension is free; Remote Free covers 100 remote messages a week on one device, and Remote Max is $5 a month.

What it doesn't do for DeepSeek yet: no plan mode, no manual compaction and no image attachments, and a DeepSeek conversation reopened after a restart keeps its context but doesn't show its earlier messages. It is not a replacement for DeepSeek's own Web UI, and community extensions also embed that Web UI in VS Code.

Setup in detail: DeepSeek Harness in VS Code. Everything else the extension does: the AFK Pilot extension. From the phone: AFK Pilot's remote control.

## FAQ

**Is DeepSeek Harness better than Claude Code?**

With the same model, slightly, and within noise. DeepSeek V4.1 Flash 1017 fixed 42.7 of 105 planted bugs per run in DeepSeek Harness and 39.1 in Claude Code at max effort, seven runs each on Bug Hunt Bench in October 2026, with Claude Code's subagents off, as the bench runs it. The gap is not significant at that sample size, though DeepSeek Harness led at all three effort levels. DeepSeek Harness is open source (MIT) and in developer preview; Claude Code is proprietary and runs on more surfaces.

**Can Claude Code use DeepSeek models?**

Yes. DeepSeek runs an Anthropic-compatible endpoint: set ANTHROPIC_BASE_URL to https://api.deepseek.com/anthropic, ANTHROPIC_AUTH_TOKEN to a DeepSeek API key, and the model variables to deepseek-flash, as DeepSeek's Claude Code guide shows. Usage then bills to DeepSeek. Anthropic's Remote Control does not work with a custom base URL.

**Which is cheaper, DeepSeek Harness or Claude Code?**

On the same DeepSeek model, Claude Code: $1.01 a run against $1.84 at max effort on Bug Hunt Bench, because DeepSeek Harness read about 1.5 times as many tokens and wrote about twice as many. Both harnesses are free to run on a DeepSeek API key; you pay DeepSeek per token, at half price off-peak. Claude Code with Anthropic's models needs a Claude plan, from $17 a month billed annually ($20 monthly), or an Anthropic API key. Pi was cheaper than both, at $0.83 a run.

**DeepSeek Harness vs OpenCode: which scores higher?**

DeepSeek Harness, at every effort level on the same model: 42.7 against 37.6 at max, 39.2 against 36.6 at high, and 34.8 against 32.8 at low. The max gap is outside run-to-run noise. OpenCode costs less per run ($0.96 against $1.84 at max), and both are open source under MIT.

**DeepSeek Harness vs Codex: which did better?**

With DeepSeek V4.1 Flash 1017 at high effort, DeepSeek Harness fixed 39.2 of 105 planted bugs and Codex CLI 33.2, at $1.27 and $3.97 a run. Four of Codex's five runs hit the bench's three-hour limit on one repository and were scored as they stood, so its score is a floor and its 232 minutes a run are mostly that limit; DeepSeek Harness averaged 53. That is Codex CLI running a DeepSeek model through DeepSeek's Responses endpoint, not Codex on OpenAI's own models.

**DeepSeek Harness vs Hermes Agent?**

At high effort on the same model, DeepSeek Harness scored 39.2 of 105 and Hermes Agent 32.0, five runs each, at $1.27 and $0.97 a run.

**Does DeepSeek Harness have a GUI?**

Yes. It ships a local Web UI (`npx @deepseek-ai/dsh web`) and a desktop app for macOS on Apple silicon and Windows x64. DeepSeek has no official VS Code extension for it, and no phone app for the harness (DeepSeek's iPhone and Android app is its chat assistant).

**Can I run DeepSeek Harness and Claude Code in one editor?**

Yes. The AFK Pilot extension runs both in one VS Code or Cursor sidebar, each in its own conversation, and lets you reach either from a phone browser once the computer is linked. DeepSeek is off by default: turn it on in Settings → Advanced, then connect it in Settings → Providers with a DeepSeek API key.

Bug Hunt Bench adds models and harnesses as they ship. Subscribe for each new result and what it changes for people who build with agents.

AFK Pilot and Grok Build Desktop (Community) are unofficial community projects, not affiliated with or endorsed by SpaceXAI (formerly xAI), Meta, DeepSeek, OpenAI, Anthropic or Google. DeepSeek is a trademark of DeepSeek. Claude and Claude Code are trademarks of Anthropic. Codex is a trademark of OpenAI. Muse Code is a trademark of Meta. Antigravity is a trademark of Google. Grok, Grok Build, and xAI are trademarks of xAI.
