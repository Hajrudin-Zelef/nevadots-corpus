---
id: collect-261121-veille-ia/divers-tech/opencode-2-3-sends-every-subagent-to-the-background-and-folds-tool-noise-into-one-row-writeble
title: "OpenCode 2.3 Sends Every Subagent to the Background and Folds Tool Noise Into One Row | Writeble"
domain: divers-tech
role: reference
task: article
actors: []
dates: ["2026-11-15"]
keywords: ["background", "session", "subagent", "tool", "opencode", "work", "returns", "parent"]
source: https://www.writeble.com/2026/11/15/opencode-v2-3-0-background-subagents-default-quieter-transcripts-mcp-events/
source_anchor: ""
source_lines: [1, 39]
section: "Divers_Tech"
---
**OpenCode, the open-source terminal coding agent, has changed how it delegates work.** Version **2.3.0**, published on GitHub on **November 13, 2026**, makes **background subagents the default**. When the main agent hands a task to a subagent, the call now returns immediately, the subagent keeps working on its own, and it reports to the parent session when it goes idle.

The project, hosted at `anomalyco/opencode` under the **MIT licence**, had about **227,000 GitHub stars** when the release went out.

The 2.3.0 notes describe the new default plainly: subagents ânow return immediately, continue in the background, and report back whenever they become idle,â and the user âcan keep working in the parent session while multiple subagents run concurrently.â

The core changelog adds detail. A subagent now returns its **session ID** right away, so the parent, or the person driving it, can open that session and follow it. A related fix handles a subtle failure: when a subagent goes idle while its own background work is still running, its report now says it is still running. The notes say this lets the parent wait for the final report âinstead of treating the partial answer as done.â

In practice, one agent can split a job (for example, investigate a failing test, update docs and scan for a deprecated call) and keep talking to the user while the pieces finish. It also means more work happens out of sight, and that is where the releaseâs second change comes in.

The terminal UI now **summarises each run of tool calls and thoughts into one collapsible activity row**, and transcript verbosity defaults to `low`. Users who want every step back can set **Verbosity** to `medium` or `high` in settings, or set `session.verbosity` in the TUI config. A value someone has already set is kept.

The desktop app gets a matching navigation feature: **Jump to message**, opened from the command palette or with `Cmd+Alt+G` (`Ctrl+Alt+G` on Windows and Linux), searches prompts across the current session, including ones not yet loaded in the timeline. An experimental **Recent** sessions sidebar works across servers and is limited to the last seven days and 30 rows.

Several smaller changes point the same way, toward agents that run longer and across machines:

- **MCP events** , experimental and behind the`mcp_events` flag. SDK event subscriptions can now receive`experimental.mcp.event` messages from MCP servers.
- **Shell commands survive server handoffs** and keep their output when interrupted. Behind a separate`shell_timeout_background` flag, a shell command can move to the background when its timeout expires instead of being killed.
- **Mutual TLS for providers.** A provider entry can present a client certificate to gateways that require it, using`settings.tls` with`cert` ,`key` and an optional`passphrase` . Values accept the same`{file:}` and`{env:}` references as`apiKey` .
- **Fork awareness.** Agents are now told when their session is a fork, and extensions get an experimental`session.fork` hook.
- **Local model discovery** for Ollama, LM Studio and vLLM now waits up to 15 seconds per request instead of one, so remote or tunnelled endpoints can list their models, and a slow endpoint no longer holds up the others. LM Studio decision-type models are now accepted.

Code Mode gets simpler structured results. `write` and `edit` return a confirmation string, `patch` returns an empty result, and `skill` returns the skillâs directory and content. Calling a namespace as if it were a tool now suggests up to three real tools under it.

The bug-fix list covers problems that cost long sessions real work: retryable provider streams no longer lose completed work when interrupted, and exhausted rate limits are no longer treated as fatal errors. MCP OAuth refreshes are fixed for servers that rewrite `127.0.0.1` redirects, and child sessions driven directly now resume after a restart. Tool output truncation now keeps the start of the output and a usable preview if the full output cannot be saved. Tail reads no longer load a whole file into memory.

The release is a direct upgrade with no breaking-change warnings in its notes. The main adjustment for existing users is a transcript that shows less by default, and a parent agent that no longer waits for its subagents to finish.

- #OpenCode
- #Coding Agents
- #Open Source
- #Subagents
- #MCP

Author

Contributor

AI Systems Architect is a seasoned technology leader with over 15 years of experience in the IT industry working with Fortune 500 companies. With a solid foundation in multi-agent systems, open-source LLM infrastructure, and enterprise deployment, he excels at building scalable production-grade AI platforms.
