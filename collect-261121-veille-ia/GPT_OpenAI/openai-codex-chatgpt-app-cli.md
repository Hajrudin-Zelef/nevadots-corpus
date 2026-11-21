---
id: collect-261121-veille-ia/gpt-openai/openai-codex-chatgpt-app-cli
title: "OpenAI Codex (ChatGPT App & CLI)"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["codex", "chatgpt", "openai", "install", "desktop", "plans", "lesson", "sandbox"]
source: https://learn.intellimetrics.net/modules/06-cli-tools-setup/04-codex-cli/
source_anchor: ""
source_lines: [1, 134]
section: "GPT_OpenAI"
---
# OpenAI Codex (ChatGPT App & CLI)

Last reviewed

### What you'll learn

~15 min
- Know when the ChatGPT desktop app (ChatGPT Work or Codex) is the easier way in
- Install and sign in to Codex CLI (skip if you did it in Lesson 1.2)
- Understand the approval policies and sandbox modes
- Run the build loop (plan, approve, build, check, keep) with /plan and /review

This is the in-depth lesson for OpenAI Codex. If Codex is the tool you installed in Lesson 1.2, you are set up already: use this page for its modes, approvals, memory files and `/review`, then mark it complete at the bottom to finish Module 6.

**Codex Cloud** (open it from your ChatGPT account) runs async coding tasks in a sandboxed cloud environment and opens PRs when done, with no local install. Local CLI work, cloud tasks and the desktop app share one usage pool. See the Cloud Sandbox Cheat Sheet for pricing, or set up your sandbox.

## Desktop app or CLI

The **ChatGPT desktop app** runs the same Codex agent with no terminal: download it from OpenAI for macOS or Windows (the Linux app is in preview). It is also the free way in: **Free ($0) and Go include GPT-6 Luna in the desktop app**, while OpenAI’s docs do not confirm CLI access on those two plans.

You choose a local folder, and ChatGPT can read and change the files in it. A switcher at the top picks who the agent talks to. **ChatGPT Work** is for non-developers: it plans the task, runs it on your folder, keeps Git and shell commands out of view, and hands back documents, spreadsheets, or a shareable site. **Codex** is for builders: it shows diffs and review views, uses Git worktrees, and reads the same `AGENTS.md`, skills, and `~/.codex/config.toml` as the CLI.

On the app, pick **Codex** for builders. Everything in this lesson about plan mode, `/review` and checkpoints applies to it.

## What is Codex CLI?

Codex CLI is OpenAI’s open-source command-line coding agent, written in Rust. You assign it a task and it plans, implements, tests and iterates, but outputs still need your review. OpenAI’s featured models are **GPT-6.2 Astra** (the flagship), **GPT-6.2 Sol** (the everyday workhorse) and **GPT-6 Luna** (the fastest, and the model on Free and Go in the desktop app). `/model` shows what your plan offers and sets the model and its reasoning effort.

## Install & sign in (skip if you already set this up in Lesson 1.2)

Codex runs natively on Windows in PowerShell with its own sandbox (`mxc` is the recommended one), and in WSL 2 (not WSL 1). On Linux and WSL, install `bubblewrap` with your package manager first so the sandbox works.

### Install Codex CLI

**Option A: Installer script** (recommended; no Node.js needed):

**Windows native:** open a new PowerShell window and run:

**WSL:**

See OpenAI’s Codex CLI page for the current install options.

**Option B: npm** (if you already have Node.js):

**Option C: Homebrew:**

Verify (any install method):

Later, `codex update` upgrades in place and `codex doctor` diagnoses install, config, and auth problems.

### Sign in

The easiest way is your **ChatGPT account**; no API key needed:

On first launch, Codex opens a browser sign-in. Sign in with your ChatGPT account and you’re done. Plans differ in limits and models; Free and Go get GPT-6 Luna in the desktop app, and OpenAI’s docs do not confirm CLI access on those two plans (see the plan table below).

On a remote server over SSH, use the device-code flow (beta) instead. Turn it on first in your ChatGPT security settings (a workspace admin does this for business accounts), then run:

## 🔍Alternative: API keyclick to expand

An OpenAI API key bills per token to your OpenAI Platform account (create one at platform.openai.com under **API Keys**). Never commit a key to git or share it; revoke it from the dashboard if exposed. Hand it to Codex:

Plus and Business share a **five-hour window** plus **weekly limits**; Pro plans currently have no five-hour limit. `/status` or `/usage` shows where you stand (pricing page; current prices for every tool are in the Cloud Sandbox Cheat Sheet).

| Plan | Price | What it gets you | 
|---|---|---|
| **Free** | $0 | Limited Codex access; GPT-6 Luna in the desktop app | 
| **Go** | $8/mo | Lightweight coding tasks; GPT-6 Luna in the desktop app | 
| **Plus** | $20/mo | Codex in the CLI, IDE and desktop app with the larger models; the practical “I use Codex regularly” tier | 
| **Pro** | $100, $200, or $500/mo | Higher included usage by tier | 
| **Business** | $20/user/mo (annual; $25 monthly) | Plus-level limits per seat, then shared workspace credits and admin controls | 

## Your first build

Already built `welcome.html` in Lesson 1.2? You’re set. If you are trying Codex as a second tool, run Lesson 1.2’s Step 2 in your `ai-class` folder.

## The build loop and modes

The rhythm is **plan, approve, build, check, keep**: open `codex` in your project folder, switch to plan mode (`/plan` or `Shift+Tab`), describe the goal in one or two sentences, approve the plan, let it build, check the result with `/review`, then ask Codex to save a checkpoint (Module 7).

### Approval and sandbox

Codex controls autonomy with two independent policies. **Approval** (`-a`) is when it asks: `on-request` lets the model decide and asks before stepping outside the sandbox; `never` never asks, and anything the sandbox disallows simply fails. **Sandbox** (`-s`) is what it can touch: `read-only`, `workspace-write` (your project directory) or `danger-full-access`.

Combine them: `codex -a on-request -s workspace-write` is the **Auto** preset, the everyday setting and what a git repository starts in (a folder that is not a git repository starts `read-only`). Switch mid-session with `/permissions`. The old `--full-auto` shortcut and the `untrusted` policy have been removed (approvals and security).

This course starts practice sessions with approval prompts off: `codex --dangerously-bypass-approvals-and-sandbox` skips every confirmation prompt and also turns off the sandbox, so commands run without approval and outside the project folder’s limits. OpenAI documents that flag only for environments sandboxed or hardened from outside, and its best practices suggest newcomers start with the default permissions (what that does and does not protect). To be asked more often, start with `codex` (the Auto preset above). The four tools side by side are in the tool modes table.

### Plan first

Plan mode is a session state where Codex reads your code, asks clarifying questions, and proposes a plan before it builds. Enter it with `/plan` (with or without a prompt) or `Shift+Tab`. It is an in-session control with no launch flag, so start `codex` first and switch inside. Read the plan, toggle plan mode off the same way, and say which steps to build: `Go ahead with steps 1 and 2, then stop.` Plan mode is not a sandbox; when nothing may be written at all, as in an audit, start with `codex --sandbox read-only` instead (developer commands).

### Write the brief: `/init`

`/init` scaffolds an `AGENTS.md`, the brief file Codex reads every session: layout, build and test commands, conventions. Edit it down, and add a rule when Codex repeats a mistake. Module 9 covers what to keep in it.

### Review your diff: `/review`

`/review` asks what to review: the diff against a base branch, your uncommitted changes, one commit, or custom instructions. Pick uncommitted changes before you commit, evaluate the findings, fix the justified ones, and rerun your tests. From the shell, `codex review --uncommitted` does the same.

### No `/rewind`: save a checkpoint first

Codex has no `/rewind`. OpenAI’s CLI guide says to create Git checkpoints before and after a task. Ask Codex: “Save a checkpoint called ‘before CSV export’.” If the task goes wrong, ask it to put the files back to that checkpoint. Module 7 teaches the rest of git.

## Power features

## Power features: sandbox, models, effort, resume, headless

### Sandbox

`codex exec` runs read-only unless you pass `--sandbox workspace-write`. The sandbox **reduces blast radius** but does not replace review: read the diff before accepting changes.

### Models and effort

Think in tiers: a **flagship** (GPT-6.2 Astra today), an **everyday workhorse** (GPT-6.2 Sol) and a **fast, light** model (GPT-6 Luna); `/model` shows what your plan offers and sets how hard it thinks (`low` up to `max`, plus `ultra`, which delegates to parallel subagents and uses your window faster).

### Resume, tidy sessions, headless

`codex resume --last` reopens your most recent session; `/resume` picks an earlier one, `/fork` copies one, `/clear` starts fresh and `/compact` condenses. `codex exec "your prompt"` runs one task non-interactively, the building block for CI (Module 19).

## Verify it works

`codex --version` should print a version number. If you get “command not found,” revisit the install step. Smoke test: `codex exec --skip-git-repo-check "What is 2 + 2? Reply with just the number."` (`--skip-git-repo-check` is fine for this one-line test).

### When Things Go Wrong

Use the **Symptom → Evidence → Request** pattern: describe what you see, paste the error, then ask for a fix.

### Key Takeaways

- **Two ways in** : the**ChatGPT desktop app** (pick**Codex** for builders or**ChatGPT Work** for everyday knowledge work), or the CLI via the installer script (`curl -fsSL https://chatgpt.com/codex/install.sh | sh` , or the PowerShell one-liner on Windows),`npm install -g @openai/codex` , or`brew install --cask codex`
- **Sign in with ChatGPT** ;`codex login --device-auth` for SSH; an API key still works for CI
- **Two policy axes** : approval controls when Codex asks, the sandbox controls what it can touch
- **The build loop** : plan, approve, build, check, keep;`/plan` before a non-trivial task,`/review` before you keep it, and a checkpoint as your undo, because Codex has no`/rewind`
