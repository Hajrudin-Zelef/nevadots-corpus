---
id: collect-261121-veille-ia/gpt-openai/claude-fable-5-2-codex-0-169-and-cursor-breach-recap
title: "Claude Fable 5.2, Codex 0.169 and Cursor Breach Recap"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["claude", "fable", "code", "github", "changelog", "anthropic", "model", "mode"]
source: https://oday-bakkour.com/blog/ai-coding-tools-roundup-week-of-november-9-2026
source_anchor: ""
source_lines: [1, 125]
section: "GPT_OpenAI"
---
## Executive Summary

- **Claude Fable 5.2** launched on November 9 at $8/$40 per million tokens, cheaper and up to 30% faster than Fable 5.1. It became the default Fable model in Claude Code 2.1.321 and reached GitHub Copilot the same day. (Anthropic, Claude Code changelog, GitHub changelog)
- **Claude Code 2.1.321 to 2.1.325** add an experimental Code Mode and plugin version ranges, and fix several permission and security bugs, including an MCP OAuth token leak on redirects. (Claude Code changelog)
- **Codex CLI 0.168.0 and 0.169.0** (both November 13) add a side-by-side terminal workspace, audio input and animated GIF generation, and stop`codex exec` from loading project config in untrusted folders. (ChatGPT and Codex changelog)
- **GitHub Copilot** added Gemini 4 Carbon and Gemini 4 Hydrogen, made GitHub Enterprise Server support in the Copilot app generally available, and will retire six models on**December 14** . (GitHub changelog)
- **Cursor** published its investigation of a security incident that included a malicious copy of the Claude Code extension on its marketplace cache. (Cursor)
- **OpenCode 2.3.0** makes subagents run in the background by default. (OpenCode)

## Claude Fable 5.2 is the release of the week

Anthropic released Claude Fable 5.2 and Claude Mythos 5.2 on November 9. Anthropic says they are the same underlying model. Fable is available to everyone and has stronger cybersecurity and biology safeguards. Mythos is limited to organizations in Anthropic's Cyber and Life Sciences Verification Programs.

For coding, the main facts are:

- **Price:** $8 input and $40 output per million tokens, down from $10 and $50 for Fable 5.1. Fast mode costs $16 and $80 and generates tokens up to 2.5x faster.
- **Benchmarks (Anthropic's own numbers):** 69.9% on FrontierSWE v2, against 62.3% for Opus 5.5 and 56.3% for Fable 5.1. On FrontierCode 2.0 at High effort it roughly matches Opus 5.5's best score at about half the cost per task.
- **Positioning:** Anthropic still calls Opus 5.5 the daily driver for most coding. It suggests Fable for the hardest open-ended work, or as an orchestrator that hands simpler tasks to smaller models.
- **Where:** Claude apps, the API as`claude-fable-5-2` , AWS, Google Cloud and Microsoft Azure.

Coverage from MacRumors and TestingCatalog adds context. Note that Anthropic amended the post on November 11 to correct a sentence in its math formalization section.

### It reached the tools on day one

- **Claude Code 2.1.321** (November 9) made Fable 5.2 the default Fable model with a 1M context window and $0.20 per million cache reads. Saved Fable 5.1 selections move to`fable` , and`/model claude-fable-5-1` switches back. (changelog)
- **GitHub Copilot** made it generally available the same day on Pro+, Max, Business and Enterprise, across VS Code, Visual Studio, JetBrains, Xcode, Eclipse, the CLI, the coding agent and github.com. Business and Enterprise admins must turn on its policy, which is off by default and requires data retention. (GitHub changelog)

## Claude Code 2.1.321 to 2.1.325

Four releases this week. There is no 2.1.320 or 2.1.324 in the official changelog.

### New features

- **Code Mode (2.1.323, experimental):** an opt-in mode in which Claude writes JavaScript to call its tools instead of calling them one at a time. See the Code Mode docs.
- **Plugin version ranges (2.1.323):**`engines["claude-code"]` in`plugin.json` sets the Claude Code versions a plugin supports. Plugins outside the range are not installed, updated or loaded, and`claude plugin validate` now type-checks TypeScript hook modules.
- **1M context for unknown model IDs (2.1.323):** if you point Claude Code at a gateway model it doesn't recognize, it now assumes 1M tokens instead of 200K. Set`CLAUDE_CODE_DISABLE_1M_CONTEXT=1` to keep the old limit.
- **Fast mode on Vertex (2.1.322):** available in the global, US and EU regions, with a fallback to standard speed.

```
{
  "name": "my-plugin",
  "engines": { "claude-code": ">=2.1.323" }
}
```
### Security and permission fixes

These are the reason to update now:

- MCP OAuth could send secrets such as refresh tokens to another host after a server redirect (2.1.321).
- A read-only Bash command could run outside the sandbox without a prompt, and deny rules missed files written by `cp` or`mv` into a directory (2.1.321).
- A security issue in decoding zstd-compressed responses (2.1.325).
- Sandboxed Bash commands now go through the normal permission flow by default. Set `sandbox.autoAllowBashIfSandboxed: true` if you relied on the old auto-allow (2.1.325).
- A start-up warning now flags Bash allow rules with `*` inside a variable, such as`Bash(PORT=* npm run dev)` (2.1.323).

2.1.325 also fixes sessions on third-party gateways and Foundry failing with a context length error instead of compacting, and restores turns lost after an unclean exit. A weekly digest on DEV lists the full set.

## OpenAI Codex CLI 0.168.0 and 0.169.0

Both shipped on November 13, according to the ChatGPT and Codex changelog.

### 0.168.0: workspace, audio and GIFs

- `features.tui_workspace` shows a side conversation next to the main chat, with resizable panes and`leader z` to zoom.
- Audio recordings can be attached when the model supports audio.
- `features.image_generation_animation` generates or edits animated GIFs.
- Model providers can be browsed under Sessions → Model providers, with a default for new chats.
- `tui.notification_method = "osc777"` turns on terminal notifications with longer approval previews.
- Setting `features.multi_agent` now warns you to use`agents.enabled` instead.

```
[features]
tui_workspace = true
[tui]
notification_method = "osc777"
```
### 0.169.0: tabs and stricter trust

- **Breaking for CI:**`codex exec` keeps project config and MCP servers disabled in untrusted folders, even with`--sandbox workspace-write` . Pass`--trust` to`codex exec` or`codex review` to save trust.
- The terminal workspace supports multiple side-chat tabs, and saved side chats reopen after a restart.
- The agent command center can search older tasks by name, prompt or working directory.
- Custom model providers can be added from the terminal, with device-code sign-in for supported gateways.
- `Ctrl+S` in the report editor saves a diagnostic ZIP locally.

Earlier in the week, 0.167.2 began sending tool updates incrementally on Vertex AI by default. (DEV digest)

## GitHub Copilot: new models and a December cutoff

- **New models:** Gemini 4 Carbon (November 10, Pro+ and up) and Gemini 4 Hydrogen (November 13, Pro and up). Both are turned on automatically unless an admin has disabled default model enablement. Google describes Carbon as aimed at cybersecurity and long-horizon agent work in its developer post.
- **Retirements on December 14:** Claude Fable 5, Claude Haiku 4.5, Gemini 3.8 Flash, Gemini 4 Argon, Grok 4.6 and Kimi K3. Suggested replacements are Fable 5.2, Haiku 5.5, Gemini 4 Hydrogen, Gemini 4 Carbon and Grok 4.7. (GitHub changelog)
- **Enterprise:** GitHub Enterprise Server support in the Copilot app is generally available from app 1.2.0, without cloud sessions. Enterprises can also manage Copilot seats through a GitHub App.
- **C++:** the C++ language server now auto-configures MSBuild builds.

If your team pins any of the retiring models in agent configs or custom instructions, change them before December 14.

## Cursor: security incident update and multiplayer Projects

On November 13 Cursor published its findings on a security incident first disclosed on October 29. An attacker used an access key Cursor had issued to its inference vendor Fireworks, which Fireworks exposed, to copy coding agent traces. Credentials in some traces then led to SaaS tools, archived non-production repositories and some support data. The attacker also put a malicious copy of the Claude Code extension, versions 2.1.312 and 2.1.313, on the cache behind Cursor's VS Code extension marketplace for about 11 hours. Cursor says Anthropic did not publish it and that this was not a flaw in Anthropic's product.

What to do:

- If you installed `Anthropic.claude-code` 2.1.312 or 2.1.313 from the Cursor marketplace in that window, follow Cursor's notice. It says uninstalling alone may not be enough.
- Rotate any keys Cursor told you were exposed. Cursor says users who got no notification were not in the exposed traces.

On the product side, Cursor Projects is now multiplayer in beta. Teammates share specs and a task board, and their agents can spot overlapping work.

## OpenCode 2.2.7 and 2.3.0

From the OpenCode release list:

- **2.2.7** (November 10): GitHub Copilot Auto support, side panel tab shortcuts, and marking sessions read or unread.
- **2.3.0** (November 13): subagents run in the background by default, transcripts are quieter by default, and you can jump back to an earlier prompt.

## Gemini CLI v0.68.0

Gemini CLI added an extension validation command, a `SubagentStop` hook event, rootless Podman sandbox support and a visible cool-down after 429 rate limits. (DEV digest)

## My take

Fable 5.2 is the headline, but the useful lesson is the Cursor incident. Agent traces hold secrets, and extension caches are part of your supply chain. This week Claude Code and Codex both tightened trust and permission defaults, and that is the right direction. Before you update, check two things: CI jobs that call `codex exec` in untrusted folders, and Claude Code setups that depended on sandboxed Bash being auto-approved. Also replace any Copilot models retiring on December 14.

## Sources

## Comments

Share your thoughts and join the conversation
