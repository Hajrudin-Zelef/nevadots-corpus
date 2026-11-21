---
id: collect-261121-veille-ia/gemini-google/a-practitioners-cheat-sheet-to-multi-agent-workflows-in-google-antigravity
title: "A Practitioner’s Cheat Sheet to Multi-Agent Workflows in Google Antigravity"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["orchestrator", "gemini", "antigravity", "workflows", "google", "hydrogen", "cheat", "sheet"]
source: https://medium.com/google-cloud/a-practitioners-cheat-sheet-to-multi-agent-workflows-in-google-antigravity-93520180bfea
source_anchor: ""
source_lines: [1, 192]
section: "Gemini_Google"
---
# A Practitioner’s Cheat Sheet to Multi-Agent Workflows in Google Antigravity

When to use Orchestrator, Subagents, Custom Agents, Dynamic Workflows, /boost, /swarm, /teamwork, /goal, and the Python SDK.

In the span of a single week in November 2026, Google Antigravity shipped **Gemini 4 Carbon** alongside `/swarm` (Nov 9-10), **Gemini 4 Hydrogen** with **Orchestrator** (Nov 12), and **Dynamic Workflows** (`/run-workflow`, Nov 13) — building on top of **Subagents**, **Custom Agents**, `/boost`, `/teamwork`, `/goal`, and the **Antigravity Python SDK**.

Suddenly, developers opening the Antigravity App or CLI (`agy`) face a new kind of decision paralysis: *which multi-model or multi-agent primitive should I reach for on this task?*

A recent community post by developer Bee (`@thtbee_`) on X captured a crisp five-line heuristic that immediately resonated across the ecosystem:

*"Same thing on repeat = workflows. Many files = swarm. One bug = boost. Many days = teamwork. Everything else = Orchestrator.¨*


That mental model gets you 80% of the way there. When you cross-check it against the official Antigravity documentation and engineering blog, though, a few architectural details — around **how agents share state**, **who pays the token bill**, **how workspaces are isolated**, and **where verification happens —** make all the difference in day-to-day practice.

This field guide fact-checks the full lineup and gives you a practical decision framework for choosing the right orchestration mode.

## The At-a-Glance Cheat Sheet

## 30-Second Decision Tree

- **Everyday feature work & debugging** → Pick**Orchestrator** in the model menu.
- **One background lookup, browser test, or repeatable role** → Use**Subagents** or define a**Custom Agent** .
- **Deterministic multi-step fan-out or recurring check** → Use**Dynamic Workflows (****/run-workflow**
**)** .
- **One stubborn race condition, deadlock, or algorithm** → Run`/boost` .
- **Broad work across independent files where agents should talk to each other** → Run`/swarm`*(watch your quota)* .
- **Multi-milestone migration, systems simulator, or formal research** → Run`/teamwork` .
- **Overnight single-agent grind or automated CI pipeline** → Pair with`/goal` or build directly on the**Antigravity Python SDK** .

## Deep Dive: How Each Option Actually Works

## 1. Orchestrator: Bidirectional Multi-Model Collaboration (Not a Static Router)

If you use one setting as your daily driver, make it **Orchestrator**. Listed directly in the model selector as **Orchestrator (Carbon)** (Google AI Ultra) and **Orchestrator (Argon)** (paid Google AI Pro, eligible device trials, and Ultra), Orchestrator pairs a larger Gemini 4 planner with fast **Gemini 4 Hydrogen** inside a single conversation. *(Note: On Gemini Enterprise plans, Gemini 4 Carbon, Gemini 4 Argon, and Gemini 4 Hydrogen are selected individually rather than via the Orchestrator menu entries.)*

A common misconception is that multi-model modes act like static upfront routers that guess task difficulty from your first prompt. According to the Antigravity models documentation and the Gemini 4 Hydrogen launch post, Orchestrator works as a **dynamic, bidirectional loop within every turn**:

- **Read-only planning:** Gemini 4 Carbon or Argon inspects the codebase, resolves ambiguity, and writes a concrete plan — including explicit re-planning triggers. (If you only asked a question, the planner answers directly.)
- **Fast execution:** Gemini 4 Hydrogen takes over to edit files, run builds, and execute tests.
- **Automatic escalation & review:** If a verification step fails twice, an API behaves unexpectedly, a checkpoint comes due, or you send a message mid-run, control returns to the planner to repair the plan before handing back to Hydrogen.

In Google’s published **Terminal-Bench 4.1** evaluations, Orchestrator matched near-Carbon task success rates while consuming **24% less quota** and completing **19% faster** on average. One practical caveat: Orchestrator runs a fixed reasoning configuration, so thinking-level overrides (`/effort`) do not apply. When you need manual reasoning depth—including Hydrogen’s `Extra` (`xhigh`) budget—switch directly to standalone Gemini 4 Hydrogen, Argon, or Carbon.

## 2. Subagents & Custom Agents: Clean Contexts and Execution Symmetry

**Subagents** are available across **all plan tiers** and handle focused side tasks — codebase exploration (`research`), live UI/network inspection (`browser`), or cloned worker threads (`self`)—asynchronously in the background. Each subagent starts with a **clean context window** so verbose grep output and test logs never pollute your main thread, and can run in your active workspace (`inherit`), an isolated Git worktree (`branch`), or shared repo storage (`share`). In the CLI, `/agents` opens the Agent Manager, `Alt+J` jumps to pending approvals, `Ctrl+K` approves inline, and `K` terminates a looping subagent.

**Custom Agents** take this further by letting you commit specialized roles as Markdown files with YAML frontmatter in `.agents/agents/<name>.md` (repository-scoped for your whole team), `~/.gemini/config/agents/<name>.md` (global), or inside plugins.

What makes Antigravity’s custom agents distinctive — detailed in Introducing Custom Agents and Custom Agents in Google Plugins — is **execution symmetry**: setting both `mainAgent: true` and `subagent: true` lets you either launch a conversation *directly* as that agent (`agy --agent release-auditor`) or let a coordinator delegate to it dynamically. You can also bind a scoped `tools` subset, curated `skills`, `mcpServers`, and a granular `commandExecutionPolicy` (`sandbox`, `auto`, `eager`, or `off`):

```
<!-- Licensed under the Apache License, Version 2.0 -->
---
name: release-auditor
description: Read-only pre-release auditor for manifests and tests.
model: flash
mainAgent: true
subagent: true
permissionMode: plan
commandExecutionPolicy: sandbox
tools:
  - view_file
  - grep_search
  - run_command
skills:
  - skills/release-checklist
---
# Release Gate Guidelines
Inspect manifests, lockfiles, and test reports without edits.
Flag regressions with file paths and remediation steps.
```
*Tip from the* *subagents docs**: double-check exact tool identifiers (such as* *view_file* *and* *run_command**) in your* *tools* *list—a misspelled tool name can cause a custom subagent to hang.*

## 3. Dynamic Workflows (/run-workflow): Scripted Fan-Out, Not Agent Chat

Shipped in **v2.32.0** (Nov 13, 2026) for **Google AI Pro and Ultra**, Dynamic Workflows lets the agent write and run a short Python orchestration script using a Python runtime bundled with Antigravity (overrideable via `ANTIGRAVITY_PYTHON` for Python 3.8+).

`@thtbee_` nailed the core workflow value proposition—*“same thing on repeat”*—and there are **three helpful fact-checks** from the official documentation:

- **You can trigger it with** **/run-workflow**
 **or plain language:** Asking*“Run a workflow that audits each package in parallel”* invokes the built-in`run-workflow` skill just like`/run-workflow ...` . Unless your permission preset (`Turbo` ) already permits it, Antigravity displays the exact Python script for your approval before running, and you can save vetted scripts in your repository to re-run later (`/run-workflow Run scripts/review-changes.py` ).
- **Workflow agents don’t default to a fixed Carbon/Hydrogen pairing:** Inside a workflow script,`agent(...)` calls default to`model="inherit"` (your current conversation’s model), or accept explicit model tiers such as`flash_lite` ,`flash` , or`pro` , plus`type_name` (e.g.`research` or a custom agent) and`workspace="branch"` for isolated parallel edits.
- **Workflow agents never talk to each other:** Unlike`/swarm` , spawned workflow agents cannot see the Python script or message peer agents. They execute in independent conversations and pass typed results back to the script via a`finish` tool validated against a**JSON Schema** .

```
# Licensed under the Apache License, Version 2.0
# Dynamic Workflow: discover changed files, then audit in parallel.
```
phase("Discover")

changed_files = await agent(

"List Python files changed vs main (paths only).",

schema={"type": "array", "items": {"type": "string"}},

)phase("Parallel Audit")

verdicts = await parallel(

changed_files,

lambda path: agent(

f"Audit {path} for unhandled exceptions (1-line verdict).",

schema={"type": "string"},

role=f"Audit {path}",

type_name="research",

model="flash",

),

)for verdict in verdicts:

log(verdict)

Use `parallel()` for concurrent fan-out (note: if one item fails, peer branches cancel), `pipeline()` for sequential stages, `a = agent(...); await a(...)` when the same reviewer needs multiple turns to verify fixes, and `await trajectory(...)` to inspect prior steps. If agents need to negotiate shared interfaces mid-flight, switch to `/swarm`.

## 4. /boost: Depth-First Reasoning for Stubborn Bugs

Available on **paid plans**, `/boost` is built for problems that are **hard rather than broad**—race conditions, subtle deadlocks, tight algorithmic optimizations, or gnarly coupled refactors where you do not want a prolonged scoping interview.

Under the hood, `/boost` launches a three-phase pipeline:

- **Strategy routing:** The primary orchestrator classifies the task as implementation (`DeepCoder` ) or root-cause analysis (`DeepInvestigator` ) and hands your verbatim prompt and context to a pipeline coordinator.
- **Challenge rounds:** An initial worker writes the candidate patch or root-cause trace and runs tests. Follow-up**improvement workers** then review your original requirements independently, actively try to break the prior attempt, fix flaws, and re-run checks.
- **Independent spot-check:** Before reporting back, the primary orchestrator verifies diffs and test logs against every constraint in your prompt — triggering another round if anything fails.

## 5. /swarm (Preview): Parallel Teams on a Shared Message Board

Available in preview on **Google AI Pro and Ultra**, `/swarm` turns your main conversation into a non-coding **coordinator** managing a live team:

- **Named workers & pools:** The coordinator assigns non-overlapping file or package boundaries to named workers (`@parser` ,`@api-tests` ) or numbered groups (`@fix-1` ..`@fix-4` ).
- **Built-in verification & stall monitoring:** It typically adds one`@verifier` per two to four workers to inspect diffs and run test suites, plus a`@monitor` that stays silent unless the entire board stalls.
- **Append-only shared board:** Every brief, diff summary, and handoff happens in public using`@name` ,`@group` ,`@#channel` , or`@all` mentions—visible live in the App’s**Swarm pane** (Board, Members, and Graph replay) or in the CLI via`/swarm board` (`Alt+S` ).
- **Zero-token idle sleep & mixed models:** When a worker finishes or waits for input, it parks in`Waiting` state at**zero token cost** until mentioned. You can pin specific roles to different models (for example, running`@verifier` on Gemini 4 Carbon while lighter models write unit tests) and even hot-swap a waiting worker’s model mid-run.

`@thtbee_`’s warning holds true: **swarms burn quota fast**. Because every active worker runs its own full agent loop, save `/swarm` for tasks that cleanly parallelize across files or investigation tracks—and ask for isolated per-worker working copies whenever multiple workers edit or run builds concurrently. If any member hits a quota limit, it pauses cleanly instead of spinning in a retry loop until you switch its model or nudge it.

## 6. /teamwork: Multi-Milestone Campaigns with Anti-Mocking Gates

Available on **paid plans**, `/teamwork` is Antigravity’s heavy-duty framework for work spanning **hours or days**—large migrations, systems simulators, formal proofs, and multi-angle design reviews.

Instead of jumping straight into code, `/teamwork` runs a **two-phase workflow**:

- **Phase 1 (Scoping interview):** Antigravity interviews you (*“specify what, not how”* ) to lock down objectives, objective verification checks, an isolated project directory (`~/teamwork_projects/{PROJECT_NAME}` ), and an**Integrity Mode** (`development` ,`demo` , or`benchmark` ) defining what shortcuts are forbidden.
- **Phase 2 (Milestone execution & audit gates):** Once you approve the prompt artifact,**Sentinel** hands off to a**Project Orchestrator** (which rotates to a fresh successor between milestones to avoid context degradation),**Explorers** , and**Workers** with exclusive file ownership. Candidate code must clear four independent gates:**Critic** (design/correctness),**Challenger** (adversarial stress tests),**Auditor** (checks real command output against the selected Integrity Mode so nothing is faked or mocked), and**Success Auditor** (end-to-end sign-off).

Google’s Teamwork research post documents adaptive Teamwork patterns (`Distributed coding`, `Iterative coding`, `Document Review`, `Long Proof`, and `Self-Verification`) producing real-world artifacts including an out-of-order RISC-V CPU simulator that boots `xv6` with 0.71% average cycle error against BOOM, upstreamed optimizations in `Eigen` and `ParlayHash`, and Lean-verified proofs for simplified cases of Knuth’s Cycles Conjecture.

## Three More Primitives Worth Keeping in Your Toolkit

**/goal**
 **(Goal Mode):** When a single agent—and any background subagents it spawns—just needs to keep grinding until every test or build check is green,`/goal` installs a stop-hook self-audit loop. Every time the agent tries to finish early, Antigravity blocks the exit and prompts it to verify every deliverable against real output evidence. Pair`/plan` →`/goal` for overnight migrations, or`/schedule` for recurring progress check-ins.
- **Antigravity Python SDK (****pip install google-antigravity**
**):** If you want deterministic multi-agent orchestration*outside* interactive chat—such as the automated issue-triage system in the Hydrogen launch post where Hydrogen handles high-volume triage and Carbon handles heavy refactors—the Apache 2.0 Antigravity SDK exposes`SubagentConfig` ,`max_subagent_depth` ,`allowed_subagents` , structured Pydantic outputs, and nine lifecycle hooks (`@post_tool_call` , etc.), supporting hosted Gemini models, Vertex AI, and local on-device models via`LiteRTAgentConfig` .
- **Remote Agents vs. Remote Control:** Need long-running tasks to continue after you close your laptop? Remote Agents (`agy --remote-agent` or`antigravity.google.com` , rolling out to personal Google AI Pro & Ultra accounts) run inside isolated Google-managed remote environments connected to GitHub or Google Drive, whereas**Remote Control** lets you steer your own powered-on workstation and local Git worktrees from anywhere.

## Quick-Reference Command Cheat Sheet

`# Licensed under the Apache License, Version 2.0`
# Interactive multi-agent modes (Antigravity App & CLI)

/boost <hard bug or race condition>

/swarm <parallel multi-file feature or investigation>

/teamwork <multi-day migration, simulator, or formal proof>

/run-workflow <repeatable fan-out or staged Python flow>

/goal <objective with verifiable build/test evidence># Inspect & steer live execution in the CLI

/swarm board # Open Swarm Board (or press Alt+S)

/agents # Switch custom agents or manage subagents

# Keybindings:

# Alt+J (pending approval) | Ctrl+K (approve) | Ctrl+O (tree)

## Wrapping Up

You do not need `/teamwork` for a routine feature PR, and you do not want a bare single-model prompt for a 40-file migration. Start with **Orchestrator** for daily flow, keep **Subagents / Custom Agents** handy for clean side tasks, reach for **Dynamic Workflows** when steps repeat deterministically, switch to `/boost` when depth is the bottleneck, spin up `/swarm` when independent tracks benefit from live peer discussion, and reserve `/teamwork` when strict milestone integrity gates matter most.

*Special thanks to* *@thtbee_* *for sparking the original comparison matrix.*
