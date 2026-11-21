---
id: collect-261121-veille-ia/divers-tech/opencode-2-3-0-makes-background-subagents-the-only-mode-automater-news
title: "OpenCode 2.3.0 Makes Background Subagents the Only Mode | Automater News"
domain: divers-tech
role: reference
task: article
actors: []
dates: []
keywords: ["background", "report", "subagents", "subagent", "parent", "opencode", "tool", "session"]
source: https://automater.ai/news/opencode-2-3-0-background-subagents-default/
source_anchor: ""
source_lines: [1, 44]
section: "Divers_Tech"
---
OpenCode 2.3.0 shipped November 13 with a change that alters how every fan-out workflow behaves, not just its changelog surface: subagents now always run in the background. The subagent tool call returns a session ID immediately, the child keeps working in its own session, and the parent receives a report each time the child goes idle. You can keep prompting the parent while multiple children run concurrently â and if you had automation that read a subagentâs answer inline, it breaks on upgrade. It is the biggest default flip since OpenCode 2.0 made background subagents an option.

## What changed

The v2.3.0 release notes list the change as the releaseâs first highlight: subagents ânow always run in the background, return their session ID immediately, and report results to their parent whenever they become idleâ (#63382). There is no flag. PR #63382, merged November 11, deletes the foreground path outright: the `background` option is removed from the subagent tool, `jobs.block` is gone, and there is no inline answer and no Ctrl+B handoff for subagents anymore.

Before, the parent blocked until the child finished; parallel work was opt-in via `background: true`. Now there is one mode:

```
- subagent({ background?: boolean })  // default: block, return answer inline
+ subagent({ ... })                   // always: sessionID now, report on every idle
```
A follow-up to a running child joins its current run; a follow-up to an idle child starts a new background job. The release also closes a partial-answer trap: when a subagent goes idle while its own background work is still running, its report now says it is still running, âso the parent waits for the final report instead of treating the partial answer as doneâ (#63675).

## The operational catches

The PR discussion documents three consequences operators should plan for:

- **Stopping the parent no longer stops the children.** With`jobs.block` gone, pressing Escape on the parent leaves its running children going; they finish and report later. The only per-child stop is`composer.subagent.interrupt` . Ctrl+B,`Session.background` and`jobs.backgroundAll` still exist â for foreground shells only.
- **`opencode run` output changes.** Noninteractive runs now end their first turn early, print âwaiting for N background job(s)â, and the turn triggered by the report decides the exit code. Scripts that parse the final text or exit code will see different output.
- **The tool contract is not backwards compatible.** Subagent tool output status drops from`"completed" | "running"` to`"running"` only. Integrations that read the answer inline, or the`<subagent state="completed">` tool content, must get it from the report message instead. Per the bot review on #63382, Effect Schema ignores unknown keys by default, so old calls that still pass`background` should be ignored by schema decoding and likely wonât error â theyâll just silently behave differently. Downstream projects noticed immediately: the openkit project landed a PR titled âdrop background-subagents and require OpenCode 2.3.0â the same day.

For interactive users the change is mostly upside: the prompt stays usable while explorers run. The risk the PR reviewers flagged is real, though â a childâs report can wake a parent the user just interrupted, and thereâs a known follow-up issue about agents hallucinating answers when told to âwork in the backgroundâ with no independent work to do. If you are designing a safe fan-out configuration on top of this model, our read-only explorers, one writer guide for background subagents covers the permission rules.

## Quieter transcripts by default

A second default change pairs with it: TUI transcript verbosity now defaults to low, folding each run of tool calls and thoughts into one collapsible activity row (#63207). If you audit what a child actually read, set Verbosity to medium or high in settings, or set `session.verbosity` in your TUI config â an existing setting is kept. Session IDs found in messages can now be opened directly, which matters when youâre chasing a background child.

## Smaller items worth knowing

- **Jump to an earlier prompt** in the desktop app: command palette or Cmd+Alt+G / Ctrl+Alt+G, searching prompts across the current session (#63585). An experimental Recent sessions sidebar limits itself to the last seven days and 30 rows.
- **Experimental MCP events** behind the`mcp_events` flag, plus SDK event subscriptions for`experimental.mcp.event` .
- **Shell hardening** : commands survive server handoffs and keep output when interrupted; an experimental`shell_timeout_background` flag moves a command to the background when its timeout expires.
- **Mutual TLS for gateways** : providers can present a client certificate via`settings.tls` with cert, key and optional passphrase, accepting`{file:}` and`{env:}` values (#61832).
- **Local discovery patience** : Ollama, LM Studio and vLLM endpoints get up to 15 seconds per request instead of 1, so tunnelled model lists actually appear (#63700).

## What to do before upgrading

1. Grep your automation for code that reads a subagent callâs inline output or its `completed` status; move it to report-message handling.
2. If you run `opencode run` in CI, pin the version and re-test exit-code assumptions before rolling out.
3. Accept that Escape is no longer a kill switch for delegated work â decide whether your runbook needs per-child interrupts.
4. Set `session.verbosity` now if your team relies on full transcripts for review.

OpenCode remains MIT-licensed with roughly 227k stars and 30.7k forks, per the repository page, and v2.3.0 is a normal upgrade via npm, brew or its install script. If you are still on V1-era config, now is a good moment to review the OpenCode V2 setup guide so permission rules match the new always-background model. The background-subagent flip is the item that changes behavior you didnât opt into â treat it as a contract change, not a feature.
