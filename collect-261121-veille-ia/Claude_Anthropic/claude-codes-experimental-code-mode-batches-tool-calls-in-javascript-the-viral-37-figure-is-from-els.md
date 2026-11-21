---
id: collect-261121-veille-ia/claude-anthropic/claude-codes-experimental-code-mode-batches-tool-calls-in-javascript-the-viral-37-figure-is-from-els
title: "Claude Code’s Experimental Code Mode Batches Tool Calls in JavaScript. The Viral 37% Figure Is From Elsewhere"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["code", "claude", "mode", "tool", "javascript", "calls", "experimental", "anthropic"]
source: https://i10x.ai/news/claude-code-code-mode-javascript-tool-calls
source_anchor: ""
source_lines: [1, 118]
section: "Claude_Anthropic"
---
AI coding agents are chatty. They read a file, think, run a command, think, edit a file, think again, and every round trip resends the conversation. Anthropic is now testing a fix in Claude Code: let the model write a small program that does all of that in one go. The Claude Code changelog for version 2.1.323, dated November 12, 2026, lists “Code Mode, an experimental opt-in in which Claude writes JavaScript to call its tools.” By the weekend, developers on X were calling it the end of chatty agents, and a “37% fewer tokens” figure was everywhere. The feature is real. That number, as far as we can tell, is not about this feature.

## Quick Take

1. **What shipped:** Code Mode, an experimental, off-by-default option in Claude Code v2.1.323 and later. Instead of calling tools like Bash, Read or Edit one at a time, Claude writes JavaScript that calls them, and Claude Code runs it in the session.
2. **How to try it:** Start Claude Code with the environment variable`CLAUDE_CODE_EXPERIMENTAL_CODE_MODE=1` and confirm the prompt. Organizations can block it via managed settings.
3. **Limits:** Interactive terminal sessions only, Anthropic API only (no Bedrock, Google Cloud, Foundry or gateways), supported models Opus 5.5, Sonnet 5.5, Haiku 5.5 and Fable 5.2. Remote Control, Claude in Chrome and computer use do not work while it is on.
4. **The savings:** Anthropic’s Code Mode documentation gives no savings figure. The widely shared “37%” comes from Anthropic’s November 2025 article on Programmatic Tool Calling in the API. Independent tests show real but varying savings.
5. **Why it matters:** Tool overhead is a big part of what burns through Claude Code’s usage limits. Batching tool calls in code attacks that overhead directly.

## In plain English

When an AI coding assistant works on your project, it normally does one small step at a time and reports back after each one, which uses up a lot of the computing allowance you pay for. Anthropic is testing a new mode where the assistant instead writes a short set of instructions that does many steps at once and only reports the result. Think of it like sending one shopping list instead of making ten trips to the store. It is switched off by default, is still an experiment, and only works in the text-based version of the tool.

## What happened

Anthropic added Code Mode quietly: one line in the changelog and a new documentation page titled “Run tool calls from code with Code Mode.” The docs explain the change in two sentences: “Without Code Mode, Claude asks for tool calls such as Bash, Read, or Edit directly and reads each result before it decides what to do next. With Code Mode, Claude writes a piece of JavaScript that makes the tool calls, and Claude Code runs that code in your session.”

Anthropic is careful to label it as a test: “Code Mode is experimental and off by default. It may change or be removed in any release.” Developer posts on X, including one by Daniel San (@dani_avila7), note it is also available in the newer 2.1.325 build.

## How it works

In a Code Mode session, you still describe the task in plain words. Claude then writes JavaScript that can run commands in parallel, feed one result into the next call and return only the part of the output it needs to read. The documentation gives this example for raising a retry limit:

One script finds the file, edits it and runs the tests. In the chat you still see the individual tool calls, such as the diff of the edit. The code itself sits in the transcript viewer (Ctrl+O). Turning it on takes one line in the terminal:

Under the hood, a hands-on test by wmedia.es found that Code Mode gives the model just two tools, `REPL` (runs the JavaScript) and `Poll` (collects results), while the other tools are described inside `REPL` as JavaScript functions in a shorter form. Developer Rhys (@RhysSullivan) looked at the implementation and wrote that Claude Code uses “node:vm running in process in Bun,” while “Codex does their code mode in v8, OpenCode does theirs in a custom interpreter, Pi uses quickjs.”

Anthropic recommends it for tasks with many related tool calls: a change that repeats across many files, a search whose results need filtering, or several independent commands that can run at once.

## The fine print

The documentation lists a long set of conditions:

- **Surface:** an interactive`claude` session in a terminal, native build. It does not work with`-p` , the Agent SDK, the VS Code panel, the Desktop app or cloud sessions.
- **Account:** a Claude subscription or Console account. Not through a gateway, Amazon Bedrock, Claude Platform on AWS, Google Cloud’s Agent Platform or Microsoft Foundry. Not for organizations using the HIPAA configuration or zero data retention.
- **Conflicts:** it cannot start with an API key helper, agent teams, OpenTelemetry export or`DISABLE_TELEMETRY` .
- **Resuming:** a Code Mode session can only be resumed in the terminal with Code Mode on.
- **Time limits:** code that Claude waits for stops after ten minutes. Code running in the background (auto or bypassPermissions mode) can run for up to two hours.

Security also changes shape. Every tool call made by the code still passes through the user’s deny and ask rules and PreToolUse hooks. But in auto mode, “the classifier reviews each piece of code before it runs, not each tool call,” and it “doesn’t see values computed at run time, such as a path taken from search output.” Anthropic’s advice: keep rules or hooks for calls you must control.

## Background: where the 37% comes from

Calling tools from code is not a new idea. Anthropic’s own November 24, 2025 engineering post, Introducing advanced tool use, launched Programmatic Tool Calling for the API and credited inspiration from, among others, “Cloudflare’s Code Mode.” That article contains the figure now attached to Claude Code: “Average usage dropped from 43,588 to 27,297 tokens, a 37% reduction on complex research tasks.” It measured an API feature in which Claude wrote Python in a sandbox, not the new Claude Code mode.

Several X users spotted the mix-up. @lzuren2001 wrote (translated from Chinese) that the 37% matches “an API feature in another Anthropic article from a year ago,” and that “the official Code Mode documentation doesn’t contain a single token-saving number.” i10x also could not find an Anthropic source for the “1.6x longer sessions” claim circulating in summaries.

What independent tests show:

- **wmedia.es** ran the same task (find the top three customers across 40 JSON files) in two Opus 5.5 sessions. Total input tokens, cache included, fell from 150,364 to 94,659. But the run took 37 seconds instead of 15. The author notes that one run each “is not a benchmark.”
- **@kairofeed** reported eight runs on Sonnet 5.5: “about 40% fewer tokens” on short tasks and “55% fewer, 486K → 218K” on a 30-file refactor.

The wmedia test also explains where the savings come from: not necessarily fewer turns, but a much smaller fixed tool context that rides along on every request (System tools at 11.1k tokens with Code Mode versus 24.6k without, in that setup).

## Pros and cons

### For users

- **Pro:** Smaller context per request can stretch Claude Code’s five-hour and weekly limits on multi-step jobs, and less tool chatter in the conversation.
- **Con:** Opt-in, experimental, terminal only. In at least one test it was slower, and sessions started in Code Mode cannot be resumed without it.

### For developers

- **Pro:** Loops, filters and parallel commands in one step, with only the relevant output returned. Hooks can inspect the generated JavaScript via`tool_input.code` on the`REPL` tool.
- **Con:** Debugging shifts from reading tool calls to reading a script, and permission design gets trickier because the auto-mode classifier does not see runtime values.

### For companies

- **Pro:** Admins can block it organization-wide with one managed setting (`CLAUDE_CODE_EXPERIMENTAL_CODE_MODE` set to`0` ), and existing deny rules still apply to every call.
- **Con:** Not available through cloud providers, gateways, HIPAA or zero-data-retention setups, which rules out many enterprise deployments for now.

## What X is saying

The simplest explanation came from developer Pedro G (@pggalaviz):

Claude Code shipped an experimental Code Mode.

Instead of calling tools one at a time and reading each result, Claude writes a bit of JavaScript that runs the calls in parallel and hands back only the output it needs.


@RhysSullivan compared how different agents implement the same idea:

How Code Mode is implemented in Claude Code – they use node:vm running in process in Bun

Codex does their code mode in v8, OpenCode does theirs in a custom interpreter, Pi uses quickjs

Interesting how much variance there is in the implementations


Not everyone is convinced. “Why would code mode output be dramatically better than the absurdly long shell commands that Claude code already writes nowadays,” asked @Santosh74038967, adding: “I’ve seen code mode in Codex and didn’t find the results to be better.” Another developer, @tangmian, warned (translated from Chinese) that when tool calls turn into one block of script, “if something goes wrong you first have to understand the script it wrote” before you can trace which step failed.

## Grok’s take

Asked how to use it, Grok replied with the steps from the docs: update with `claude update`, start with `CLAUDE_CODE_EXPERIMENTAL_CODE_MODE=1 claude` and confirm “Yes, turn on Code Mode” when prompted. Grok did not attach a savings figure.

## Critical analysis

Code Mode is a good idea with a bad press release that Anthropic never wrote. The engineering logic is sound and well documented elsewhere: models are better at writing a loop than at “eyeballing” twenty tool results in a row, and every avoided round trip saves context. Anthropic’s own 2025 data on the API version supports that. But a feature that cuts tokens in an API benchmark does not automatically stretch a subscription’s five-hour window by the same amount, and Anthropic has not claimed it does.

The independent numbers so far point the same direction, with big caveats: single runs, different models, different tasks, and in one case a slower result. The more interesting trade-off is security and oversight. Code Mode moves the unit of review from a tool call to a program. Anthropic is upfront that the auto-mode classifier cannot see what a script computes at run time. For teams that rely on command-text allow rules, that is a reason to look at sandboxing and hooks before switching it on.

Code Mode also fits a pattern i10x has tracked: the harness around a model matters almost as much as the model. See our coverage of Claude Code vs Codex usage in T3 Code and the Reason coding harness cost claims.

## What’s still open

- Whether Anthropic will publish its own measurements for Code Mode in Claude Code.
- When it will work in the Desktop app, VS Code, the Agent SDK and cloud sessions, and whether it becomes the default.
- When agent teams and OpenTelemetry export will be compatible.
- Whether it will ever be available through Amazon Bedrock, Google Cloud or Microsoft Foundry.

## About the analysis

This independent i10x analysis is based on Anthropic’s Claude Code changelog and Code Mode documentation, Anthropic’s 2025 engineering post on advanced tool use, a hands-on test by wmedia.es and public posts on X. i10x did not run its own benchmark.

## i10x Perspective

The fastest way to make an agent cheaper is not always a cheaper model. Sometimes it is fewer, smarter calls. Code Mode turns Claude Code from a narrator (“now I will read the file”) into a scripter (“here is the result”), and that is where every major coding agent is heading. The hype got the number wrong, but the direction right. We would treat it like any experiment: try it on repetitive, multi-file jobs, measure your own token use with `/context`, keep your guardrails, and wait for Anthropic’s own data before telling the finance team about savings.
