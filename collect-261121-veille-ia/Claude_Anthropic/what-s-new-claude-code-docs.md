---
id: collect-261121-veille-ia/claude-anthropic/what-s-new-claude-code-docs
title: "What's new - Claude Code Docs"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["claude", "week", "code", "desktop", "also", "read", "digest", "session"]
source: https://code.claude.com/docs/en/whats-new
source_anchor: ""
source_lines: [1, 206]
section: "Claude_Anthropic"
---
**: run your plugin against a suite of test cases, score the results, and compare against a no-plugin baseline.**

`claude plugin eval``claude plugin eval init` drafts the cases and graders for you.Also this week: pop any **Claude Code Desktop pane**out into its own window and dock it back later; the

**setting caps the effort level on every provider; and a page that**

`maxEffortLevel`
**WebFetch**hasn’t finished downloading within five minutes fails instead of hanging.Read the Week 37 digest →

**Claude Fable 5.1**: available in Claude Code with a 1M-token context window.Also this week: on Pro and Max plans,

**computer use in the Desktop app**works in the background on macOS while you keep working; in fullscreen rendering,

**opens a live panel beside the conversation that refreshes as Claude edits; and**

`/diff`
**shows what each of your skills costs in context and how often it gets used.Read the Week 36 digest →**

`/skill-doctor`
**Resume terminal sessions in the Desktop app**: type

`/resume` in the Claude Code Desktop prompt box to pick up any session you started from the CLI, with the full conversation and context intact.Also this week: **Claude-drafted feedback**has Claude write up a feedback report when something goes wrong in a session, which you review and send from

`/feedback`; **starts a session without the command-running tools or your user and project settings, for evaluation harnesses on shared machines; and the**

`--restricted`
**setting controls which models the**

`modelPicker``/model` picker lists.Read the Week 35 digest →
**: a research preview that brings Claude Design’s artboard workflow into the CLI and Claude Code Desktop, built on artifacts, so Claude drafts editable artboards for your UI and implements the one you pick.Also this week: the built-in**

`/design`
**Concise output style**makes Claude lead with the result and skip preamble; any machine running

`claude remote-control` shows up as a **device card**on your phone so you can start a session on it from the Code tab; and

**sets the model new sessions start on.Read the Week 34 digest →**

`ANTHROPIC_DEFAULT_MODEL`
**Auto-continue after a usage limit on Desktop**: when you hit your session limit in Claude Code Desktop, check

**Auto-continue when limits reset**on the limit card and the app retries the interrupted turn once the limit resets.Also this week:

**fork mode**is on by default in interactive sessions, so Claude can delegate a side task to a subagent that inherits the full conversation;

**GitLab**merge request URLs work with

`--worktree` and the `claude agents` view, and marketplaces clone bare `gitlab.com` URLs; and typing **in the prompt mentions another Claude session by name.Read the Week 33 digest →**

`@`
**Cross-session messaging**: on macOS and Linux, your Claude Code sessions can now message each other, so Claude passes a finding or a decision from one session to another instead of you re-explaining it.Also this week:

**self-hosted environments**run Claude Code cloud sessions on infrastructure your organization operates, in public beta on Team and Enterprise plans;

**auto mode**becomes the default permission mode for new sessions on Pro, Max, and Team plans starting August 14; and the

**VS Code extension**gets Focus view.Read the Week 32 digest →

**Claude Opus 5**: the new default Opus model in Claude Code, with a 1M-token context window and fast mode at $10/$50 per MTok.Also this week:

**Claude Code Desktop**opens an iOS Simulator pane in public beta so Claude can run your app and tap through it while you watch; the

**Claude Security plugin**runs a multi-agent vulnerability scan of your codebase and turns the findings you pick into patches you apply yourself; and

**runs as a background subagent.Read the Week 30 digest →**

`/code-review`
**Artifacts call your MCP connectors**: a published artifact can pull live data and take actions through each viewer’s own MCP connectors when they open the page, and this week also adds public sharing links, editor roles on Team and Enterprise, and artifacts created from Claude Tag sessions.Also this week:

**screen reader mode**replaces the visual terminal interface with plain, linear text for screen readers such as VoiceOver and NVDA;

**copies your conversation into a new background session while you keep working; and**

`/fork`
**auto mode**no longer needs an opt-in variable on Amazon Bedrock, Google Cloud’s Agent Platform, and Microsoft Foundry.Read the Week 29 digest →

**In-app browser on Desktop**: Claude Code on desktop gets a built-in browser, so Claude can pull up docs, designs, or any other site and interact with pages the same way it does with your local dev server previews.Also this week:

**is a full setup checkup that diagnoses issues and can fix them, with**

`/doctor``/checkup` as its alias; **auto mode**blocks transcript tampering and asks before

`rm -rf` on unresolved variables; and **agent view rows**show a colored state word and a classifier-written headline.Read the Week 28 digest →

**Claude Sonnet 5**: the new default model for Pro, Team Standard, and Enterprise subscription seats, with top-tier coding and tool use at Sonnet pricing, a native 1M-token context window, and adaptive thinking on by default.Also this week:

**Claude in Chrome**is generally available on all direct Anthropic plans;

**subagents run in the background by default**so Claude keeps working while they run;

**Claude Desktop on Linux**lands in beta on Ubuntu and Debian; and

**tunes into Claude FM lo-fi radio.Read the Week 27 digest →**

`/radio`
**: authenticate a configured MCP server from your shell instead of the interactive**

`claude mcp login``/mcp` menu, and clear its stored credentials later with `claude mcp logout`.Also this week: **shell mode responds to command output**(

`! npm test` gets an explanation without a second prompt); **can resume a conversation from before**

`/rewind``/clear` was run; and **background subagents**now surface permission prompts in the main session instead of auto-denying.Read the Week 26 digest →

**Artifacts**: turn a session’s output into a live, shareable page on claude.ai that updates in place as the session works, now in beta on Team and Enterprise plans.Also this week:

**deny and ask rules match tool parameters**with

`Tool(param:value)`, for example `Agent(model:opus)`; **sets any setting from the prompt, in**

`/config key=value``-p` mode, and from Remote Control; and **auto mode blocks destructive git commands**when you didn’t ask to discard local work.Read the Week 25 digest →

**: move the current session to a new working directory mid-conversation without rebuilding the prompt cache.Also this week:**

`/cd`
**sub-agents can spawn their own sub-agents**(background chains are capped at five levels deep);

**starts Claude Code with all customizations disabled for troubleshooting; and**

`--safe-mode`
**configures up to three fallback models tried in order.Read the Week 24 digest →**

`fallbackModel`
**Auto mode on Amazon Bedrock, Google Cloud’s Agent Platform, and Microsoft Foundry**: auto mode is now available on third-party providers for Opus 4.7 and Opus 4.8, replacing permission prompts with background safety checks.Also this week:

**safer automatic edits**prompt before writing files that can run code in

`acceptEdits` mode; **prints your installed plugins inline; and**

`/plugin list`
**version requirements**let managed deployments require an approved Claude Code version range.Read the Week 23 digest →

**Claude Opus 4.8**: the new default model for Max, Team Premium, Enterprise pay-as-you-go, and Anthropic API accounts, with high effort by default and

`/effort xhigh` for the hardest tasks.Also this week: **dynamic workflows**orchestrate dozens to hundreds of subagents from a script Claude writes; the

**security-guidance plugin**reviews Claude’s changes for vulnerabilities as it works; and

**fast mode**runs on Opus 4.8 at $10/$50 per MTok.Read the Week 22 digest →

**Auto mode on the Pro plan**: auto mode now runs on Pro accounts and supports Sonnet 4.6 alongside Opus, replacing permission prompts with background safety checks.Also this week:

**breaks down what drives your plan limits by skill, subagent, plugin, and MCP server; the new**

`/usage`
**command reports correctness bugs; and**

`/code-review`
**background sessions**appear in

`/resume` and stay alive when pinned.Read the Week 21 digest →
**Agent view**:

`claude agents` opens one screen for every Claude Code session, showing what’s running, what’s blocked on you, and what’s done.Also this week: **keeps Claude working across turns until a completion condition holds;**

`/goal`
**fast mode**now runs on Opus 4.7 by default; and the

**Rewind menu**can compress earlier context with “Summarize up to here”.Read the Week 20 digest →

**Plugins load from**:

`.zip` archives and URLs`--plugin-dir` now accepts `.zip` files, and `--plugin-url` fetches a plugin archive for the current session.Also this week: **chooses whether new worktrees branch from the remote default or local**

`worktree.baseRef``HEAD`; **auto mode hard deny rules**block actions unconditionally regardless of allow exceptions; and

**hooks see the active effort level**via

`effort.level` and `$CLAUDE_EFFORT`.Read the Week 19 digest →
**Windows without Git Bash**: Git for Windows is no longer required, and Claude Code uses PowerShell as the shell tool when Bash is absent.Also this week:

**brings cloud code review to CI and scripts;**

`claude ultrareview`
**cleans up local state for a project; and pasting a**

`claude project purge`
**PR URL into**finds the session that created it.Read the Week 18 digest →

`/resume`
**opens as a public research preview: a fleet of bug-hunting agents runs in the cloud and findings land back in your CLI or Desktop automatically.Also this week:**

`/ultrareview`
**session recap**shows you what happened while a terminal was unfocused;

**custom themes**let you build and ship color palettes from

`/theme` or a plugin; and **Claude Code on the web**gets a redesign with a new sessions sidebar and drag-and-drop layout.Read the Week 17 digest →

**Claude Opus 4.7**lands as the new default on Max and Team Premium, with a new

`xhigh` effort level that’s the recommended setting for most coding work and an interactive `/effort` slider to dial it in.Also this week: **Routines**on Claude Code on the web fire templated cloud agents from a schedule, GitHub event, or API call;

**mobile push notifications**ping your phone when a long task finishes or Claude needs you;

`/usage` shows what’s driving your limits; and the CLI moves to native binaries.Read the Week 16 digest →
**Ultraplan**enters early preview: draft a plan in the cloud from your CLI, review and comment on it in a web editor, then run it remotely or pull it back local. The first run now auto-creates a cloud environment for you.Also this week: the

**Monitor**tool streams background events into the conversation so Claude can tail logs and react live,

`/loop` self-paces when you omit the interval, `/team-onboarding` packages your setup into a replayable guide, and `/autofix-pr` turns on PR auto-fix from your terminal.Read the Week 15 digest →
**Computer use**comes to the CLI in research preview: Claude can open native apps, click through UI, and verify changes from your terminal. Best for closing the loop on things only a GUI can verify.Also this week:

`/powerup` interactive lessons, flicker-free alt-screen rendering, a per-tool MCP result-size override up to 500K, and plugin executables on the Bash tool’s `PATH`.Read the Week 14 digest →
**Auto mode**lands in research preview: a classifier handles your permission prompts so safe actions run without interruption and risky ones get blocked. The middle ground between approving everything and

`--dangerously-skip-permissions`.Also this week: computer use in the Desktop app, PR auto-fix on Web, transcript search with `/`, a native PowerShell tool for Windows, and conditional `if` hooks.Read the Week 13 digest →
