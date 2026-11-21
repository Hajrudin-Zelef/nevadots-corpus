---
id: collect-261121-veille-ia/claude-anthropic/codex-cli-0-162-0-vs-claude-code-2-1-296-what-changed
title: "Codex CLI 0.162.0 vs Claude Code 2.1.296: What Changed"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-10-08"]
keywords: ["claude", "model", "workflow", "codex", "release", "code", "agents", "subagent"]
source: https://tech-insider.org/codex-cli-0-162-vs-claude-code-2-1-296-2026/
source_anchor: ""
source_lines: [1, 71]
section: "Claude_Anthropic"
---
Codex CLI 0.162.0 and Claude Code 2.1.296 address different parts of coordinating coding agents. The Codex CLI release notes introduce managed Git worktrees and shared task pinning, while the Claude Code release notes add a model override for workflow agents and earlier compaction for individual subagents. The practical choice is between improvements to organizing project work and controls over how a workflow executes.

## The release changes side by side

OpenAI dated Codex CLI 0.162.0 October 8, 2026; Anthropic dated Claude Code 2.1.296 October 9, 2026. These are the historical releases compared here. Their release notes identify different control surfaces: projects and tasks on the Codex side, workflow agents and subagent context on the Claude side.

| Release | Project or agent control | Important scope | 
|---|---|---|
| Codex CLI 0.162.0 | Create and list managed Git worktrees; pin Command Center tasks | Worktrees require a trusted local project and the worktrees feature enabled. Shared pinning requires server support. | 
| Claude Code 2.1.296 | Select a common workflow-agent model; set subagent autoCompactWindow | The model override applies to workflow agents. Other subagents retain their own models. Compaction can occur before the main conversation’s window fills. | 

## Codex puts project and task controls in the terminal

OpenAI’s managed-worktree addition provides creation and listing operations for trusted local projects. Because the release explicitly requires the worktrees feature to be enabled, installing the version is only part of making that capability available. Check that condition before treating a missing worktree action as an installation failure.

Task pinning has a separate condition. OpenAI specifies the `p` key in the agent Command Center and a shared Pinned group when the server supports it. For someone juggling related tasks, the useful distinction is that worktree management concerns project work, while pinning concerns which tasks stay easy to find. Neither setting chooses the model used by a Claude workflow.

The same OpenAI release adds `/copy` for navigating and copying transcript blocks, `Ctrl+Insert` for copying selections, and `tui.mouse_scroll_speed` for wheel scrolling. Those controls are relevant when handing a specific agent response or command result to a colleague: the transcript becomes easier to navigate and extract without making the whole session the handoff.

The model change belongs to OpenAI’s preceding release: Codex CLI 0.161.0 made GPT-6.1 Sol the default in its bundled and Amazon Bedrock catalogs and added `/mcp login <name>` for MCP-server sign-in from an active terminal session. In 0.162.0, OpenAI separately fixed new terminal threads to honor server model and reasoning-summary defaults while keeping explicit launch overrides. It also added live-web-access and remote-compaction capability settings for custom Responses-compatible providers.

## Claude separates workflow model policy from other subagents

Anthropic’s `CLAUDE_CODE_WORKFLOW_SUBAGENT_MODEL` override gives the agents inside a workflow a common model while leaving other subagents’ selections intact. That scope matters when a session combines a scripted workflow with separately delegated tasks: choosing a workflow model is not a promise that every agent in the session has changed model.

The release also adds `autoCompactWindow` to subagent frontmatter and `--agents` definitions. Anthropic describes it as allowing a subagent to compact earlier than the main conversation. This is a separate decision from model selection: it changes when that subagent compacts its context, rather than which model handles its work.

For operating a workflow, Anthropic’s current workflow documentation describes a JavaScript script that Claude writes and a runtime executes in the background. The documented interactive procedure is:

1. Describe the task and explicitly ask Claude to use a workflow. A direct request in ordinary language is supported.
2. When the permission mode presents a launch prompt, inspect the planned work. The prompt offers a raw-script view before you choose whether to run it.
3. Open `/workflows` , select the run and inspect its progress. Anthropic documents per-agent token usage in this view, alongside the ability to stop the run.
4. To reuse the orchestration, select the run in `/workflows` and press`s` . Choose the project’s`.claude/workflows/` location for repository sharing or the personal`~/.claude/workflows/` location for your own projects.

This makes the script itself a useful review point. Tech Insider recommends checking which stages need the chosen model before applying a common override, then inspecting the resulting work and token use on a limited task. Uniform model selection simplifies the choice across agents; the quality of their combined result still needs evaluation.

## Line endings and denied calls get concrete fixes

File handling is a practical reason to care about these point releases. OpenAI’s patch notes say `apply_patch` now preserves existing CRLF line endings without an opt-in. Anthropic’s release fixes the Workflow tool rejecting scripts with CRLF endings. The fixes touch different operations: preserving an edited file’s format in Codex and accepting a workflow script in Claude Code.

Anthropic’s fixes also cover Edit and NotebookEdit operations on files with invalid UTF-8 encoding: those edits are refused instead of replacing non-ASCII characters. For a repository containing older text encodings, refusal makes the affected file an explicit handling task rather than silently changing its characters.

The policy fixes deserve equal attention. Anthropic repaired managed `PreToolUse` hooks that denied a call with `"continue": false`, and managed prompt hooks that blocked a call, without ending the turn. It also fixed headless sessions starting folder or plugin MCP servers that had been disabled for that folder after a directory change or plugin reload. These are specific enforcement repairs worth including in an organization’s upgrade checks.

OpenAI’s notes also list Linux sandbox-startup fixes involving denied files and sandbox executables, plus protection against ripgrep configuration weakening deny-glob masks. On Windows 10, the release restores drive-letter access and aligns sandbox temporary-directory permissions with the child process environment. Those repairs concern permission handling separately from the new worktree controls.

## Model rates put the cost controls in context

For API billing, OpenAI’s model release record lists absolute GPT-6.1 Sol rates, and Anthropic’s model pricing table supplies the Claude rates below. Input, output and cache reads have separate prices, so a model override needs to be evaluated against the token mix of the workflow it will run.

| Model and prompt condition | Input | Output | Cached input | 
|---|---|---|---|
| GPT-6.1 Sol, up to 272K input tokens | $2 | $10 | $0.10 | 
| Claude Sonnet 5.5 | $2 | $10 | $0.10 | 
| Claude Opus 5.5 | $4 | $20 | $0.20 | 
| Claude Haiku 5.5, up to 100,000 prompt tokens | $0.10 | $0.50 | $0.01 | 
| Claude Haiku 5.5, over 100,000 prompt tokens | $0.50 | $2.50 | $0.05 | 

At these rates, GPT-6.1 Sol and Claude Sonnet 5.5 have matching input, output and cache-read prices. Tech Insider would therefore compare their completed work and actual token use before expecting either model to save money on the same assignment. Anthropic’s Haiku threshold also makes prompt size part of the model choice: its pricing documentation counts all input tokens, including cache reads and writes, toward the threshold, and applies the higher rates to an over-threshold request.

Anthropic’s October API release notes date the Sonnet cache-read reduction to October 7, 2026. Claude Code 2.1.296 then updated `/cost`, its status line, `--max-budget-usd` and SDK cost figures to use $0.10 per million cache-read tokens instead of $0.20. That is a correction to the cost accounting for an already changed rate.

The table describes API usage. OpenAI’s Codex project documentation separately supports ChatGPT-plan sign-in and API-key setup, while Anthropic’s workflow documentation says runs count toward plan usage and rate limits. Treat the billing route as part of the comparison before turning token prices into an expected subscription bill.

## Choose around the work you need to coordinate

Our assessment is that Codex’s additions are most directly useful when the difficulty lies in organizing project work and keeping tasks accessible. Claude’s override and compaction settings are more directly relevant when the difficulty lies inside a workflow: deciding which model handles its agents and when individual subagents compact. The value comes from matching the control to that problem.

For a practical evaluation, use a representative repository task and keep the review focused. On Codex, examine whether the managed-worktree and task-pinning controls improve the way you organize that task. On Claude, inspect the workflow’s stages, their results and the token usage shown in the progress view. Anthropic explicitly says a workflow can consume more tokens than doing the same task in conversation, so a lower-priced model should be assessed across the whole run.

Judge the outcome by accepted changes and review effort. The question of coding-agent output and software delivery belongs alongside the release comparison: a useful evaluation should show whether these controls help you finish the intended work, not merely generate more activity.
