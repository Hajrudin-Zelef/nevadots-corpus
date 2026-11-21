---
id: collect-261121-veille-ia/claude-anthropic/2026-11-17-anthropic-claude-code-code-mode-javascript-tool-calls-849b3b0e
title: "Anthropic claude code code mode javascript tool calls"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-11-17"]
keywords: ["code", "claude", "mode", "tool", "anthropic", "calls", "javascript", "session"]
source: ""
source_anchor: ""
source_lines: [1, 66]
section: "Claude_Anthropic"
---
Anthropic has added Code Mode to Claude Code as an experimental, opt-in feature. Claude Code Code Mode is a mode in which Claude writes JavaScript that makes several tool calls in 1 step. It shipped in Claude Code v2.1.323 last week. For developers, it changes how the agent spends round-trips on multi-step work. You can check the deployment options and specs here through our GPULeaderboard.com chrome extension.
TL;DR
- Works with: It works with Claude Opus 5.5, Sonnet 5.5, Haiku 5.5 and Fable 5.2.
- Runs on: An interactive terminal session on the native build, v2.1.323 or later. Windows needs Git for Windows. Anthropic API or Claude subscription only.
- Performance: Anthropic publishes no benchmark or token-saving figure for Code Mode.
- Best: Background code can run for up to 2 hours while you keep talking to Claude.
- Worst: Code that Claude waits on stops after 10 minutes. No Bedrock, Google Cloud, Foundry, -p , Agent SDK, VS Code or Desktop.
- Bottom line: Best: related tool calls run as 1 program. Worst: narrow availability and coarser auto-mode review.
What is Code Mode in Claude Code?
Without Code Mode, Claude requests each Bash, Read or Edit call directly. It reads every result before choosing the next step. With Code Mode, Claude writes a piece of JavaScript, and Claude Code runs it in your session.
▲ AI DEV SIGNALS · FREE NEWSLETTER
Get the AI releases that matter, before they trend.
70,000+ AI builders · 1-click unsubscribe
According to the docs, one piece of code can run commands in parallel. It can feed one result into the next call. It can also return only the output Claude needs to read.
Anthropic’s example raises a retry limit. The code finds the file, edits it and runs the tests:
const files = await $`rg -l "function retryPolicy" src`.text();
const target = files.trim().split("\n")[0];
await edit(target, "maxAttempts: 3", "maxAttempts: 5");
({ edited: target, tests: $`npm test -- retryPolicy`.nothrow() });
In the conversation, you see the tool calls, such as the diff. You do not see the code by default. Press Ctrl+O to open the transcript viewer and expand a collapsed Code row.
How do you turn on Code Mode?
Set CLAUDE_CODE_EXPERIMENTAL_CODE_MODE=1 in your shell, or in the env block of ~/.claude/settings.json. Before the first session, Claude Code asks you to confirm. Select “Yes, turn on Code Mode”.
Claude Code then restarts and shows ‘Code Mode (experimental) is on’.
A Code Mode session resumes only with the variable still set to 1. Setting 0 anywhere turns it off, even if 1 is set elsewhere. Admins can block it org-wide with a 0 in managed settings. They cannot force it on.
When should developers use Code Mode?
Anthropic recommends it for tasks with many related tool calls:
- A change that repeats across many files
- A search whose results need filtering before Claude reads them
- Several independent commands that can run at the same time
How does Code Mode handle permissions and long-running code?
Each tool call inside the code still passes your deny and ask rules. It also passes PreToolUse hooks under the tool’s usual name, such as Bash.
Review differs by permission mode:
- Auto mode: a classifier reviews each piece of code, not each tool call. It cannot see values computed at run time, such as a path from search output.
- Other modes: code starts without review, and you are prompted per tool call as usual.
Hook authors should check each part of a command. A git push can arrive as cd 'app' && git push. To inspect the code itself, match the REPL tool and read tool_input.code.
If Claude waits, the code stops after 10 minutes, including time at permission prompts. In the background, it can run for up to 2 hours while you keep chatting. Background runs need a session started in auto or bypassPermissions mode.
What are the requirements and limitations?
- Version: v2.1.323 or later; v2.1.326 or later with agent teams
- Account: Claude subscription or Console account. Not available through gateways, Amazon Bedrock, Claude Platform on AWS, Google Cloud, or Microsoft Foundry
- Organization: no HIPAA configuration or zero data retention
- Surface: interactive terminal only. No -p , Agent SDK, VS Code panel, Desktop app or cloud sessions
- Blocked settings: API key helper, OpenTelemetry export or DISABLE_TELEMETRY
- Unavailable features: Remote Control, Claude in Chrome and computer use
How does Code Mode compare with other code-first tool calling?
Cloudflare coined ‘Code Mode’ on September 26, 2025. Its Agents SDK turns MCP schemas into a TypeScript API that runs in V8 isolates.
Anthropic then published code execution with MCP. In one example, token use fell from 150,000 to 2,000, a 98.7% saving. Its API feature, Programmatic Tool Calling, uses Python. It cut average usage from 43,588 to 27,297 tokens on complex research tasks.
Claude Code’s version applies the pattern to the CLI’s own tools. The docs give no token figure. One independent test reported system-tool context of 11.1k tokens with Code Mode versus 24.6k without. Treat that as a single third-party measurement.
| Feature | Claude Code Code Mode | Claude API Programmatic Tool Calling | Cloudflare Code Mode | Standard tool calling | 
|---|---|---|---|---|
| Where it runs | Claude Code CLI, your session (docs) | Code execution tool, Claude API (Anthropic) | Agents SDK, V8 isolates (Cloudflare) | Any tool-use API | 
| Language Claude writes | JavaScript | Python | TypeScript | None (JSON calls) | 
| Tools it orchestrates | Built-in tools (Bash, Read, Edit) | Developer-defined tools | MCP servers as TypeScript APIs | Any | 
| Status | Experimental, off by default | Launched Nov 2025 | Worker Loader API in closed beta | GA | 
| Published token figure | Not disclosed | 43,588 to 27,297 avg (37%) | Not disclosed in launch post | Baseline | 
| Max run time | 10 min waiting, 2 hr background | Not disclosed | Not disclosed | Per call | 
| Cloud providers | Anthropic API only | Not on Bedrock or Google Cloud (docs) | Cloudflare Workers | Broad | 
Key Takeaways
- Claude writes JavaScript that chains Bash, Read and Edit calls in 1 step.
- Opt in with CLAUDE_CODE_EXPERIMENTAL_CODE_MODE=1 on v2.1.323 or later.
- Background code runs up to 2 hours; waiting code stops at 10 minutes.
- Auto mode reviews the code, not each tool call.
- Terminal and Anthropic API only: no Bedrock, Google Cloud, SDK or IDE panel.
Check out the Code Mode documentation and the Claude Code changelog. You can check the deployment options and specs here through our GPULeaderboard.com chrome extension.
Also, feel free to follow us on Twitter and don’t forget to join our 150k+ML SubReddit and Subscribe to our Newsletter. Wait! are you on telegram? now you can join us on telegram as well.
Need to partner with us for promoting your GitHub Repo OR Hugging Face Page OR Product Release OR Webinar etc.? Connect with us
Michal Sutter is a data science professional with a Master of Science in Data Science from the University of Padova. With a solid foundation in statistical analysis, machine learning, and data engineering, Michal excels at transforming complex datasets into actionable insights.
