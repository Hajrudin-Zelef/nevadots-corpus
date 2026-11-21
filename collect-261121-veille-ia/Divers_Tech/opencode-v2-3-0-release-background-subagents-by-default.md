---
id: collect-261121-veille-ia/divers-tech/opencode-v2-3-0-release-background-subagents-by-default
title: "OpenCode v2.3.0 Release: Background Subagents by Default"
domain: divers-tech
role: reference
task: article
actors: []
dates: ["2026-11-13"]
keywords: ["background", "default", "subagents", "session", "opencode", "parent", "release", "work"]
source: https://enghyu.com/en/ax/3985-opencode-v2-3-0-background-subagents/
source_anchor: ""
source_lines: [1, 72]
section: "Divers_Tech"
---
# OpenCode v2.3.0 Release: Background Subagents by Default

OpenCode released **v2.3.0 on November 13, 2026**, and the headline change is a default, not an option. Subagents now **return immediately, continue in the background, and report back whenever they become idle**. The parent session does not stop and wait. It keeps working while multiple subagents run concurrently, according to the official changelog.

That default matters because waiting is the visible cost of parallel agent work. A coding session that delegates a long task and then blocks has not gained much from delegation. In v2.3.0 the delegation call hands back a session ID at once, so the parent can issue the next instruction, start another subagent, or review other work while the first job runs. The concurrency is only useful if the reporting is trustworthy, and the release spends a specific fix on exactly that point.

**November 13, 2026, v2.3.0**

**Background subagents by default**

**12 improvements, 12 fixes**

**226,834, observed November 15, 2026**

## Background subagents by default

The changelog entry for the core area describes the new flow in three parts. A subagent call returns right away with a session ID. The subagent continues in the background. When it becomes idle, it reports back to the parent session. The parent session stays usable in between, and several subagents can be in flight at once.

The context from earlier releases helps explain why this landed as a default now. OpenCode 2.0 turned the agent into a background service that terminal, web, and desktop clients attach to. In v2.2.6, `opencode run` was tidied so it waits for completion of background work it started itself. Version 2.3.0 carries that background model into the interactive session, where subagents are now background by default rather than by request. The changelog alone does not fully settle where interactive sessions and `opencode run` still differ in waiting behavior, and that point is listed as unverified below.

## The idle report rule

The most consequential detail is a reporting rule, not the background execution itself. When a subagent goes idle while its own background work is still running, its report to the parent now **says it is still running**. The parent waits for the final report instead of treating the partial answer as done.

That rule targets the failure mode that background execution creates. A parent that receives an early, partial answer and builds on it can produce work based on an unfinished result, and the error only surfaces later. Stating still running in the report is a small protocol change, but it is the part that makes the default safe to use at scale. Without it, making every subagent background by default would multiply the number of partial answers a parent has to judge.

## Experimental flags and shell changes

Two additions in v2.3.0 are experimental and stay off unless a flag is set. MCP events arrive behind the **mcp_events** flag. Moving a shell command to the background when its timeout expires arrives behind the **shell_timeout_background** flag, and with that flag on, an explicit timeout on a background command stops the command.

Shell behavior changes outside the flags as well. Shell commands now survive server handoffs and keep their output when interrupted. Local Windows shell commands now run through the process daemon. Those changes fit the same theme as the subagent default: long running work should not lose its state or its output when the session around it moves or stops.

## Other core changes in the release

The core list for v2.3.0 contains **12 improvements and 12 fixes**. Beyond the headline items, the changelog records support for freeform tool input on compatible models, LM Studio decision model support, and a notice to agents when their session is a fork. Tool output truncation now keeps the beginning of the output and preserves a usable preview if the full output cannot be saved, and tail reads no longer load an entire file into memory.

Providers can present a client certificate for gateways that require mutual TLS, configured through settings.tls with a certificate, a key, and an optional passphrase. Code Mode receives simpler structured results from file and skill tools. On the fix side, the release covers case only file renames in patches on case insensitive filesystems, directly driven child sessions not resuming after a restart, MCP OAuth refreshes and localhost redirects, and retryable provider streams losing completed work when interrupted. Local model discovery for Ollama, LM Studio, and vLLM now waits up to 15 seconds per request instead of 1 second, so remote or tunnelled endpoints can list their models.

## Context from neighboring agent tools

OpenCode is moving in the same direction as other coding agent harnesses. Claude Code v2.1.319 moved MCP tasks into the background, a change covered in issue #3850 of this series. The pattern is consistent across tools: work that used to block a session is being pushed into the background, and the design problem shifts from starting work to reporting its state correctly.

For anyone choosing an agent harness, that shift changes the comparison criteria. Raw parallelism is now common. The differences that matter are whether a parent can tell a partial result from a final one, whether interrupted work keeps its output, and whether background jobs survive the session that started them. Version 2.3.0 gives OpenCode explicit answers on the first two points, and a partial answer on the third through shell commands surviving server handoffs.

## Limits and unverified points

Three limits should stay attached to this release. First, the changelog does not state a cap on concurrent subagents or any resource limits for background execution, so no limit can be quoted here. Second, **mcp_events** and **shell_timeout_background** are experimental flags, so neither behavior applies in a default install. Third, where the waiting behavior of an interactive session and `opencode run` diverge after this change is not settled by the changelog alone, and it is unverified here.

The verified core is still a clear step. Subagents are background by default, the parent keeps working, and the reporting rule names the exact mistake background execution invites and prevents it in the protocol. Teams that delegate long tasks inside a coding session get the time back that blocking subagents used to consume, provided their work can tolerate results arriving out of order.

## Related articles

- Claude Code 2.1.319: MCP Tasks Go Background, Release Notes
- Claude Code Projects Update: Proactive Coordinator Guide

## FAQ

### What changed for subagents in OpenCode v2.3.0?

Subagents now return a session ID immediately and continue working in the background by default. They report back to the parent session when they become idle, and multiple subagents can run at the same time while the parent keeps working.

### What happens when a subagent goes idle but its background work is still running?

Its report to the parent says it is still running. The parent then waits for the final report instead of treating a partial answer as the finished result.

### Are MCP events and shell timeout backgrounding on by default?

No. Both are experimental in v2.3.0. MCP events sit behind the mcp_events flag, and moving a shell command to the background when its timeout expires sits behind the shell_timeout_background flag.

### How large is OpenCode on GitHub?

The repository anomalyco/opencode had 226,834 GitHub stars when observed on November 15, 2026, according to the GitHub API figure recorded in the source issue.
