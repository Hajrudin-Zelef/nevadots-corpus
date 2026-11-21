---
id: collect-261121-veille-ia/claude-anthropic/claude-code-november-2026-changelog-versions-2-1-317-2-1-326
title: "Claude Code November 2026 changelog: versions 2.1.317-2.1.326"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["fable", "claude", "november", "code", "model", "versions", "changelog", "through"]
source: https://claudexia.tech/blog/claude-code-novoe-v-noyabre-2026
source_anchor: ""
source_lines: [1, 161]
section: "Claude_Anthropic"
---
From November 3 to 16, 2026, eight versions, 2.1.317 through 2.1.326, made it into the Claude Code changelog. Four changes stand out at once. Fable 5.2 became the model the `fable` alias points to, a menu for switching to usage credits appeared, the experimental Code Mode shipped, and the sandbox and permission checks got stricter. Below are dates and version numbers from Anthropic's entries, then a breakdown of each item and what changes for people who connect through a gateway.

The overview covers releases through November 16 inclusive, the last being 2.1.326. The changelog already has an entry for 2.1.327 dated November 17, which the overview leaves out. To find your version, run `claude --version`.

## Versions and dates

Dates are as the changelog writes them. The page has no versions 2.1.320 and 2.1.324.

| Version | Date | What was added or changed | 
|---|---|---|
| 2.1.317 | November 3 | `/design-sync` warns up front that the design system has moved to Claude, and stops before building. Before a web or shell tool sends your account's email address over the network, Claude Code asks for permission. Subagents on a different model take the saved effort level of that model | 
| 2.1.318 | November 4 | `claude auth status` shows the expiry date of the claude.ai sign-in, and`claude -p` warns in stderr 10 days ahead. Fixed`/design revoke` , which reported success on a 404 response | 
| 2.1.319 | November 6 | MCP tasks in the terminal, the `permissions.denyInsteadOfPrompting` setting, the`/claude-api latency-optimize` command, the`CLAUDE_CODE_DISABLE_FEATURE_FLAGS` variable. Older models in`/model` are grouped under a More models line | 
| 2.1.321 | November 9 | Fable 5.2. A saved Fable 5.1 choice moves to `fable` .`claude remote-control` got a`--settings <file>` flag | 
| 2.1.322 | November 10 | Fast mode on Vertex in the global, us and eu regions, with a fallback to normal speed if Vertex refuses | 
| 2.1.323 | November 12 | Code Mode. The usage credits menu. An unknown model ID is now assumed to have a 1M window instead of 200K. Requests for organization settings and policies no longer go through a proxy set only by `ALL_PROXY` | 
| 2.1.325 | November 13 | `CLAUDE_CODE_MAX_CONTEXT_TOKENS` sets the window for unfamiliar IDs with the claude- prefix too.`/login` asks permission to change privacy settings. In VS Code: sign-in through Console without creating an API key, and next-prompt suggestions | 
| 2.1.326 | November 16 | Code Mode together with agent teams. In full-screen `/usage` , subscribers see the plan limit bars before the session cost. The sandbox and permission checks became stricter. In VS Code, the`claudeCode.promptSuggestions` setting | 

Versions 2.1.317 and 2.1.318 also touch Claude Design commands, a product we wrote about in Claude Design: what it is.

## Fable 5.2 for the fable alias

The changelog entry for 2.1.321 of November 9 reads: Claude Fable 5.2 added with ID `claude-fable-5-2`, now the default Fable model, 1M context, $8 and $40 per million tokens, cache read $0.20. The prices match Anthropic's pricing page. The word "default" is easy to misread, so here is what the documentation actually says.

- No Fable model is selected by default on any plan. You choose it explicitly: `/model fable` in a session or`claude --model fable` at launch.
- The `fable` alias points to Fable 5.2 everywhere except Claude apps gateway sessions. There it stays on Fable 5 until you set`ANTHROPIC_DEFAULT_FABLE_MODEL` , and in such sessions you type`/model claude-fable-5-2` .
- A saved `claude-fable-5-1` in user settings moves to`fable` . On Pro, Max and Ultra this happens on the first launch of version 2.1.321 or newer, and on Team and Enterprise after the organization allows it. Project, local and managed settings, Console accounts and third-party providers stay on Fable 5.1. You can go back with`/model claude-fable-5-1` .
- After the move, sessions start with the default effort level for Fable 5.2 until you choose your own.
- Fable 5.2 needs Claude Code 2.1.321 or newer, and Fable 5.1 needs 2.1.257 or newer.

Fable and usage credits are tied together. By the documentation, depending on the plan and seat type, Fable use may be charged to usage credits instead of included limits, and then the Fable line in `/model` carries the note "Requires usage credits". In an interactive session Claude Code may show a consent prompt: continue on Fable at the expense of usage credits, or return to the default model. After you agree, the prompt does not come back. In `-p` mode and in apps built on the Agent SDK there is no such prompt, consent is not asked, and the charge happens automatically. Prices, fast mode and subscription terms are covered in the article Claude Fable 5.2. As of November 17, 2026 there are no Fable models in Claudexia's price list, and the current list is on the models page.

## The usage credits switch menu

Version 2.1.323 brought a menu with three options: stop, switch to usage credits, or upgrade your plan. It appears when the selected model needs usage credits and they are turned off or used up. The same version fixed a few small things around Fable:

- the Fable limit message in `-p` mode, the SDK, IDEs and cloud sessions now names the reset time;
- the weekly Fable limit in the terminal shows the reset time and a menu of options;
- with Remote Control connected, the usage credits question is also visible in the Claude app;
- errors about the spend limit and the 1M context in cloud sessions and under Remote Control no longer give a web address.

One more fix landed in 2.1.326. `/usage-credits` reported "Request sent to your admin" when the request had been rejected, and people with billing access are now pointed to the usage settings page. Our article on Pro, Max and Claude Code limits explains what the Pro and Max limits are and how Claude Code counts them. Commands such as `/usage` and `/model` are gathered in our reference of commands and flags.

## Code Mode

Code Mode was added in 2.1.323 on November 12. Normally Claude asks to call a tool (Bash, Read, Edit), reads the result and decides what to do next. In Code Mode it writes a piece of JavaScript that makes several calls in one step: it runs commands in parallel, passes the result of one call to the next, and returns only the part of the output you need. In the conversation you see tool calls, not code. The code opens with `Ctrl+O` in the transcript view.

The documentation warns that the mode is experimental, off by default, and may be changed or removed in any release. Anthropic suggests trying it on tasks with many calls of the same kind: edits across many files, search with filtering of the results, several independent commands at once.

| Requirement | What the documentation says | 
|---|---|
| Version | Claude Code 2.1.323 or newer; with agent teams, 2.1.326 or newer | 
| Account | A Claude subscription or a Console account. Not available through a gateway, Amazon Bedrock, Claude Platform on AWS, Google Cloud Agent Platform, Microsoft Foundry or Claude Models on Google Cloud (Anthropic managed) | 
| Organization | No HIPAA configuration and no zero data retention | 
| Model | Claude Opus 5.5, Sonnet 5.5, Haiku 5.5 or Fable 5.2 | 
| Where | An interactive `claude` session in the terminal. Does not work with`-p` , the Agent SDK, the VS Code panel, the Desktop app or cloud sessions | 
| Install | The native build of Claude Code; on Windows, Git for Windows as well | 

Meeting the requirements is not enough, because the mode also fails to start with some settings. Among them are `apiKeyHelper`, `ANTHROPIC_AUTH_TOKEN`, `Authorization` or `X-Api-Key` headers in `ANTHROPIC_CUSTOM_HEADERS`, disabled loading of feature flags (for example through `DISABLE_TELEMETRY`), and OpenTelemetry export, which `CLAUDE_CODE_ENABLE_TELEMETRY` and `ENABLE_BETA_TRACING_DETAILED` turn on. If the mode did not start, the top of the session shows the line "Code Mode (experimental) isn't available" with the reason, and `/status` shows the same reason. The fix for each reason is described in the documentation section Code Mode doesn't start.

You turn the mode on with an environment variable:

```
claude --version
claude update
CLAUDE_CODE_EXPERIMENTAL_CODE_MODE=1 claude
```
Before the first session, Claude Code asks you to confirm and restarts. A session started this way can be resumed only in the terminal and only with the same variable. Without it Claude Code refuses to continue the session. Code that runs in the normal waiting mode stops after ten minutes, while in the background it can run for up to two hours. In Code Mode you cannot use Remote Control, Claude in Chrome or computer use. If you are on the `stable` channel and version 2.1.323 has not reached you, the documentation suggests waiting or switching to `latest`.

An organization closes the mode for everyone with the value `0` in managed settings:

```
{
  "env": {
    "CLAUDE_CODE_EXPERIMENTAL_CODE_MODE": "0"
  }
}
```
The ban does not cut off sessions that are already running. They keep working in Code Mode until you exit or restart Claude Code, and after that both starting the mode and resuming such sessions are blocked.

Version 2.1.326 added Code Mode support with agent teams enabled. By the documentation, team members work in Code Mode in the in-process display mode, and without it in split panes.

## Client versions for 5.5 models and Fable 5.2

When a request to a new model fails, look at the version first. The model-config documentation names the minimum versions, and the changelog confirms when each model appeared.

| Model | ID | Minimum version | In the changelog | Anthropic price per million tokens | 
|---|---|---|---|---|
| Opus 5.5 | `claude-opus-5-5` | 2.1.280 | September 22 | $4 input, $20 output | 
| Sonnet 5.5 | `claude-sonnet-5-5` | 2.1.284 | September 28 | $2 input, $10 output | 
| Haiku 5.5 | `claude-haiku-5-5` | 2.1.293 | October 7 | $0.10 input, $0.50 output; for requests longer than 100 thousand tokens, $0.50 and $2.50 | 
| Fable 5.2 | `claude-fable-5-2` | 2.1.321 | November 9 | $8 input, $40 output | 

The 1M window on these models opens by itself, and the `[1m]` suffix is not needed. The documentation lists Fable, Sonnet 5 and newer, Haiku 5.5, and Opus 4.7 and newer. If a model is newer than the client, the documentation advises running `claude update`: after the update the ID is usually recognized, and nothing needs configuring. The error is described on the errors page under the heading "Claude Code does not support this model". Version 2.1.319 now suggests fixing a typo in a model name, such as `opus-5-5`, instead of sending you to update the client.

## If Claude Code is connected through a gateway

Some of the November changes mention gateways and your own `ANTHROPIC_BASE_URL` directly. Claudexia is such a gateway: it accepts Claude Code requests at `https://api.claudexia.tech` without `/v1`, with a key of the form `sk_cdx_`. We checked only what Anthropic's documentation says. We did not run Claude Code at this version against Claudexia ourselves.

- Code Mode. The documentation gives the reason for the refusal, "only works directly with the Anthropic API", and asks you to remove settings that route requests through a cloud provider, a gateway or your own `ANTHROPIC_BASE_URL` . By the documentation, then, you cannot turn the mode on with a Claudexia key.
- Artifacts and `/design` . Claude Code's documentation says a session with an API key or a gateway token cannot publish an artifact: you need to sign in to a Claude account with`/login` .
- Context window. Since 2.1.323 a model with an unknown ID gets a 1M window instead of 200K, and 2.1.325 extended `CLAUDE_CODE_MAX_CONTEXT_TOKENS` to unfamiliar IDs with the claude- prefix. If the gateway rejects requests longer than 200 thousand tokens, the documentation advises setting`CLAUDE_CODE_AUTO_COMPACT_WINDOW=200000` . To keep a 200K window there is`CLAUDE_CODE_DISABLE_1M_CONTEXT=1` .
- Auto mode. Version 2.1.323 fixed this: behind a gateway, auto mode now refuses to work on a model with "haiku" in its name unless it is `claude-haiku-5-5` , just as it refuses on Haiku 4.5.
- Proxy. Since 2.1.323, requests for organization settings and policies do not use a proxy set only through `ALL_PROXY` . If your network requires a proxy, set`HTTPS_PROXY` .

Ordinary Claude Code use through a gateway is described in our guide. Below is a configuration with IDs from Claudexia's live price list, checked on November 18, 2026. Among the Claude models, Premium has `claude-haiku-5-5`, `claude-opus-5`, `claude-opus-5-5`, `claude-sonnet-5`, `claude-sonnet-5-5`, `aws-claude-haiku-5-5` (input / output $0.18 / $0.28 per million tokens) and `aws-claude-opus-5-5` ($4.00 / $6.40). Cheap has three: `claude-haiku-5-5`, `claude-opus-5-5` and `claude-sonnet-5-5`. For a Cheap-only key, the IDs from the last list are the ones that work. Prices change, and the full list is on the models page.

```
{
  "env": {
    "ANTHROPIC_BASE_URL": "https://api.claudexia.tech",
    "ANTHROPIC_AUTH_TOKEN": "sk_cdx_YOUR_KEY",
    "ANTHROPIC_DEFAULT_OPUS_MODEL": "claude-opus-5-5",
    "ANTHROPIC_DEFAULT_SONNET_MODEL": "claude-sonnet-5-5",
    "ANTHROPIC_DEFAULT_HAIKU_MODEL": "claude-haiku-5-5"
  }
}
```
The file `~/.claude/settings.json` lives in your home folder. Do not put the key into `.claude/settings.json` inside a project, because it will end up in the repository. Step-by-step installation, Windows and checking the connection are described in the Claude Code setup guide, and the editor extension is covered in the article Claude Code in VS Code. Registration on claudexia.tech works with a login and password or through Google, GitHub and Telegram, and the minimum top-up is $1.

## What else is in the November versions

- Sandbox and permission checks got stricter. In 2.1.326, sandboxed commands on macOS, Linux and WSL by default no longer write to some sensitive folders in the home directory, and file tools refuse to write to Claude Code sign-in files such as `.credentials.json` . In 2.1.323 the sandbox on macOS, Linux and WSL stopped counting write access that goes through a symbolic link pointing somewhere else. Some commands that used to pass now ask for permission or are rejected. If you have hooks and permissions configured, read the 2.1.323 and 2.1.326 entries in full.
- Cost changed. In 2.1.326, automatic context compaction in plan mode uses the model that answers you, for example Opus under `opusplan` , and so it may cost more.
- Fast mode for Opus 4.8. The 2.1.316 entry of October 31 says fast mode for this model has been deprecated since October 30, 2026 and will be removed on November 30, 2026. After that, requests will return an error and will not switch to regular Opus 4.8. For more speed, the changelog suggests moving to Opus 5.5.
- VS Code. Version 2.1.325 added next-prompt suggestions: `Tab` accepts a suggestion, and`promptSuggestionEnabled: false` in`settings.json` turns them off. Version 2.1.326 added the`claudeCode.promptSuggestions` setting to the extension.

All the entries listed here can be rechecked on the changelog page. It is assembled from `CHANGELOG.md` on GitHub, and long releases are grouped by type of change.

## Sources

Pages opened on November 17, 2026.

- Changelog: code.claude.com/docs/en/changelog.
- Models, versions and Fable: Model configuration.
- Code Mode: Run tool calls from code with Code Mode.
- Artifacts and `/design` : Share session output as artifacts.
- Model prices: Anthropic pricing.

## FAQ

### How do I find my version and update Claude Code?

Run `claude --version`. If the version is old, run `claude update`. On the `stable` channel the update may not arrive right away: the Code Mode documentation suggests waiting or switching to the `latest` channel.

### Is Fable 5.2 now the default?

No. The documentation says no Fable model is selected by default on any plan, and you choose it explicitly. Something else changed: the `fable` alias, and a saved Fable 5.1 choice on Pro, Max and Ultra, now lead to Fable 5.2.

### Is Code Mode on for everyone?

No. It is off by default and is turned on with the variable `CLAUDE_CODE_EXPERIMENTAL_CODE_MODE=1`. An organization can close it for everyone with the value `0` in managed settings.

### Does Code Mode work through Claudexia?

By Anthropic's documentation, the mode does not start with a custom `ANTHROPIC_BASE_URL` or through gateways. We did not run this combination.

### Which version do Sonnet 5.5 and Haiku 5.5 need?

Sonnet 5.5 needs Claude Code 2.1.284 or newer, and Haiku 5.5 needs 2.1.293 or newer. Opus 5.5 works from 2.1.280.
