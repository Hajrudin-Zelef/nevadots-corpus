---
id: collect-261121-veille-ia/claude-anthropic/claude-codeas-code-mode-has-claude-write-javascript-to-call-its-tools-how-to-try-it-a-enjoy-agentic-
title: "Claude Codeâs Code Mode has Claude write JavaScript to call its tools. How to try it â Enjoy Agentic Industries"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-11-13"]
keywords: ["claude", "code", "mode", "call", "codeas", "javascript", "november", "tools"]
source: https://enjoy.dev/blog/claude-code-code-mode/
source_anchor: ""
source_lines: [1, 116]
section: "Claude_Anthropic"
---
- To
- Everyone running Claude Code
- From
- Mo, Chief Executive
- Date
- November 13, 2026, 9:08 PM CST
- Re
- Claude Codeâs Code Mode

# Claude Codeâs Code Mode has Claude write JavaScript to call its tools. How to try it

Claude Code 2.1.323, released November 12, 2026, adds Code Mode, an experimental opt-in in which Claude writes JavaScript that makes its tool calls. Start claude in a terminal with CLAUDE_CODE_EXPERIMENTAL_CODE_MODE=1 on a Claude subscription or Console account. It doesnât work with -p, the Desktop app, VS Codeâs panel or cloud sessions, and in auto mode the classifier reviews the code, not each tool call.

## What Anthropic released

Claude Code 2.1.323 reached npm on November 12, 2026, and its changelog opens with it: âAdded Code Mode, an experimental opt-in in which Claude writes JavaScript to call its tools.â A new docs page explains it. The next release with a changelog, 2.1.325 on November 13, fixed three Code Mode bugs, among them scripts âignoring a worktree entered with `EnterWorktree`â and âwriting `~` paths into the project.â

Without Code Mode, Claude asks for one tool call at a time, such as Bash, Read or Edit, and reads each result before it decides what to do next. With it, the docs say, âClaude writes a piece of JavaScript that makes the tool calls, and Claude Code runs that code in your session.â One piece of code âcan run commands in parallel, use one result in the next call, and return the part of the output that Claude needs to read.â

The docsâ example raises a retry limit in one step: it finds the file, edits it, runs the tests, and hands back the fileâs path and the test output.

```
const files = await $`rg -l "function retryPolicy" src`.text();
const target = files.trim().split("\n")[0];
await edit(target, "maxAttempts: 3", "maxAttempts: 5");
({ edited: target, tests: $`npm test -- retryPolicy`.nothrow() });
```
In the conversation you still see the tool calls, such as the editâs diff, but not the code. `Ctrl+O` opens the transcript viewer, where each piece of code is a collapsed **Code** row. Code Mode is off by default, and the docs warn that it âmay change or be removed in any release.â

## What you need

The requirements, as the docs list them:

| Requirement | What Code Mode needs | 
|---|---|
| Version | Claude Code 2.1.323 or later, the native build | 
| Account | A Claude subscription or a Console account | 
| Not through | A gateway, Amazon Bedrock, Claude Platform on AWS, Google Cloudâs Agent Platform or Microsoft Foundry | 
| Model | Claude Opus 5.5, Sonnet 5.5, Haiku 5.5 or Fable 5.2 | 
| Where | An interactive `claude` session in a terminal | 
| Not in | `-p` , the Agent SDK, VS Codeâs panel, the Desktop app or cloud sessions | 
| Organization | Not on the HIPAA configuration or zero data retention | 
| Windows | Git for Windows installed | 

Some settings keep it from starting too: an API key helper, agent teams, OpenTelemetry export, or `DISABLE_TELEMETRY`, which stops Claude Code downloading the settings Code Mode needs.

Two more catches. The stable release channel is still on 2.1.311, by npmâs tags on November 13, so it doesnât have Code Mode yet. And even on 2.1.323, the session may say Code Mode âisnât open to your account yet.â The docsâ advice is to check your connection, start again, and âIf the reason stays, try again another day.â Anthropic hasnât said how itâs rolling out.

## How to turn it on

1. **Check your version.** Run`claude --version` , and`claude update` if itâs older than 2.1.323. On the stable channel, wait for it to catch up or switch to the latest channel, as Anthropicâs setup docs describe.
2. **Start Claude Code with the variable.** On macOS, Linux or WSL, run`CLAUDE_CODE_EXPERIMENTAL_CODE_MODE=1 claude` . In PowerShell, run`$env:CLAUDE_CODE_EXPERIMENTAL_CODE_MODE = "1"` , then`claude` . For every new session, put it in the`env` block of`~/.claude/settings.json` . Claude Code âignores this variable in a projectâs settings files, so a repository you clone canât turn on Code Mode for you.â
3. **Confirm.** Select**Yes, turn on Code Mode** . Claude Code restarts and shows âCode Mode (experimental) is on Â· this session can be resumed only with Code Mode onâ near the top. If it says Code Mode âisnât availableâ instead, the reason follows on the same line, and`/status` shows it too.
4. **Give it a task with many tool calls.** The docs suggest: âRead every Markdown file in this project and list the links that point to local files that donât exist.â Then press`Ctrl+O` and expand a**Code** row to see what Claude wrote.

To turn it off, remove the variable or set it to `0`. A `0` in your shell or your user settings keeps it off even with a `1` in the other. An organization can block it by setting the variable to `0` in managed settings; it canât turn Code Mode on for its people.

## What changes for permissions

Read this part before you try it. Each tool call that Claudeâs code makes still goes through your deny and ask rules and your `PreToolUse` hooks, unchanged. What changes is auto mode, where a classifier reviews actions instead of asking you. In Code Mode, âthe classifier reviews each piece of code before it runs, not each tool call. It doesnât see values computed at run time, such as a path taken from search output, so keep rules or hooks for calls you must control.â

In the other permission modes, the code starts without a review, and youâre asked about each tool call that would have asked you anyway. To read the code itself before it runs, a `PreToolUse` hook can match `REPL`, the tool that runs Claudeâs code, and read the JavaScript from `tool_input.code`.

A `PreToolUse` hook that canât start or times out on a call from Claudeâs code refuses that call, âeven when `onFailure` is unsetâ; setting `"onFailure": "continue"` on the hook lets such calls go ahead. And the docs repeat a warning that holds with or without Code Mode: Bash rules match command text, so Claude can run a denied command in a form the rule doesnât match. For limits that donât depend on wording, they point to sandboxing.

## What it canât do yet

- **Resume anywhere else.** A Code Mode session resumes only in the terminal with Code Mode on: âWith Code Mode off, Claude Code refuses to resume the session.â
- **Run for long while you wait.** Code that Claude waits on stops after ten minutes, time at permission prompts included. Started in auto or`bypassPermissions` mode, code can run in the background for up to two hours while you keep talking to Claude.
- **Use some features.** Remote Control, Claude in Chrome and computer use donât work in Code Mode, and the session canât move to the Desktop app or become a background session.
- **Run outside an interactive terminal.** Developers Digest ran`claude -p` with the variable set on 2.1.325 and got âCode Mode (experimental) isnât available: for now it only works in the interactive terminal,â then a normal answer with the ordinary tools. By that test, scripts that inherit the variable keep working, without Code Mode.

## How Codex and OpenCode compare

The idea isnât new. Cloudflare named it Code Mode in September 2025: âConvert the MCP tools into a TypeScript API, and then ask an LLM to write code that calls that API.â The Claude API has a cousin, programmatic tool calling, which Anthropic says âreduces latency for multi-tool workflows and decreases token consumption.â Two other coding agents have their own versions:

| Agent | How itâs turned on | Status, by its docs | 
|---|---|---|
| Claude Code | `CLAUDE_CODE_EXPERIMENTAL_CODE_MODE=1` , on 2.1.323 or later | Experimental, off by default | 
| Codex | `features.code_mode.enabled` in`config.toml` | âUnder development and off by defaultâ | 
| OpenCode 2 | Its `execute` tool | On for every MCP server by default | 

Codexâs configuration reference lists the setting and two others for it, but doesnât describe the feature. OpenCodeâs tools docs say `execute` âruns JavaScript in Code Mode so the agent can call and combine tools from the catalog,â that its runtime âhas no direct filesystem access, imports, or timers,â and that âDenying `execute` with resource `*` removes Code Mode.â Its 2.0 notes say âAll MCP servers in 2.0 are opted into code mode by default.â

## Should you try it?

1. **Try it on the work itâs for.** The docs name three: a change that repeats across many files, a search whose results need filtering before Claude reads them, and several independent commands that can run at once.
2. **Leave it off if you depend on what it lacks:** the Desktop app, VS Codeâs panel, Remote Control, Claude in Chrome, a gateway or cloud provider, agent teams, or OpenTelemetry export.
3. **Make your guardrails explicit.** If auto mode is your main protection, add deny rules or`PreToolUse` hooks for the calls that matter, and use the sandbox for limits that donât depend on how a command is written.
4. **Keep the variable set while you have Code Mode sessions to come back to,** or Claude Code wonât resume them.

## What Anthropic hasnât said

- **Whether it saves usage.** It has published no token, speed or usage-limit numbers for Code Mode. Its claims for programmatic tool calling are about the API.
- **Who has it.** âIt isnât open to your account yetâ suggests a gradual rollout, without a schedule.
- **Whatâs next.** Whether it will leave experimental, become the default, or come to the Desktop app, VS Code,`-p` or cloud sessions.
- **Why.** Beyond the changelog and the docs page, we found no announcement from Anthropic explaining it.

## Code Mode in apps that run Claude Code

The Desktop app and VS Codeâs panel donât offer Code Mode, and neither do other apps that drive Claude Code through `-p` or the Agent SDK. Enjoy is one of those: it runs the copy of Claude Code on your computer, on the Claude plan you already have, through `-p`âs streaming mode. So Claude Code conversations in Enjoy use the ordinary tools, whether or not the variable is set. To try Code Mode, run `claude` in a terminal, and resume Code Mode sessions there.

## 10.Questions.

- ### What is Code Mode in Claude Code?
- Itâs an experimental opt-in, added in Claude Code 2.1.323 on November 12, 2026, in which Claude writes JavaScript that makes its tool calls and Claude Code runs that code in your session. One piece of code can run commands in parallel, use one result in the next call, and return only the part of the output Claude needs to read.
- ### How do I turn on Code Mode?
- Update to 2.1.323 or later, then start claude in a terminal with CLAUDE_CODE_EXPERIMENTAL_CODE_MODE set to 1, or put it in the env block of ~/.claude/settings.json. Claude Code asks you to confirm before the first session. A projectâs settings file canât turn it on, so a repository you clone canât switch it on for you.
- ### Why does Claude Code say Code Mode âisnât open to your account yetâ?
- Your version may be older than 2.1.323, so update first. If it isnât, the message means what it says, and Anthropicâs docs say to check your connection, start again, and âIf the reason stays, try again another day.â The stable release channel is still on 2.1.311, so stable users need to switch to latest or wait.
- ### Does Code Mode work in the Claude Desktop app or VS Code?
- No. It works only in an interactive claude session in a terminal, not with -p, the Agent SDK, VS Codeâs panel, the Desktop app or cloud sessions, and not through a gateway, Amazon Bedrock, Claude Platform on AWS, Google Cloudâs Agent Platform or Microsoft Foundry.
- ### Is Code Mode safe to use with auto mode?
- It changes what gets checked. In auto mode, the classifier reviews each piece of code before it runs rather than each tool call, and it doesnât see values worked out while the code runs. Your deny and ask rules and PreToolUse hooks still apply to every tool call, so keep rules or hooks for the calls you must control.
- ### Does Code Mode use fewer tokens?
- Anthropic hasnât said. It has published no numbers for Code Mode. Its docs say a similar feature on the Claude API, programmatic tool calling, cuts round trips and âdecreases token consumption,â but thatâs a different product.
