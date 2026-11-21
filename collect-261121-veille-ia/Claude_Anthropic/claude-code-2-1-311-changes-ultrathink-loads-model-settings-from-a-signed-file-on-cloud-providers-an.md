---
id: collect-261121-veille-ia/claude-anthropic/claude-code-2-1-311-changes-ultrathink-loads-model-settings-from-a-signed-file-on-cloud-providers-an
title: "Claude Code 2.1.311 changes ultrathink, loads model settings from a signed file on cloud providers, and fixes the 1M-context error · AI Tracker"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-10-26"]
keywords: ["claude", "code", "model", "ultrathink", "settings", "file", "cloud", "loads"]
source: https://ai-tracker.rootstudio.io/news/2026-10-26-claude-code-2-1-311
source_anchor: ""
source_lines: [1, 20]
section: "Claude_Anthropic"
---
# Claude Code 2.1.311 changes ultrathink, loads model settings from a signed file on cloud providers, and fixes the 1M-context error

Claude Code v2.1.311, released October 26, 2026, per its release notes:

- **Ultrathink:** in the interactive terminal,`ultrathink` now runs its turn at xhigh effort on Opus 5.5, Sonnet 5.5, Haiku 5.5 and Fable 5.1 through Anthropic's API; typed while Claude is working, it raises the rest of that turn. The rainbow highlight now appears only where the keyword can raise effort, not when the word is quoted or part of a file path.
- **Cloud providers:** on Bedrock, Mantle, Vertex and Foundry, Claude Code now loads model settings from a signed file on downloads.claude.ai. Set`CLAUDE_CODE_MODEL_CATALOG=off` to turn this off.
- **Compaction:**`DISABLE_COMPACT` in settings files can now only turn compaction off, not back on; agent teammates and the rewind menu respect it. Standard-context sessions that ran over 200K tokens used to stop with "Usage credits required for 1M context"; Claude Code now compacts the conversation and continues. When the model's reply has no summary, it usually asks once more before replacing the conversation.
- **Self-hosted runner:** new`--hold-idle-file` and`--max-hold-min` options keep an idle session while the host reports processes it left running.
- **Model names:** model names and IDs drop the`[1m]` suffix where it has no effect; the suffix is still accepted.
- **JSON output:**`--output-format json` and`stream-json` now write DEL, C1 control and Unicode format characters inside strings as`\uXXXX` escapes; parsed values are unchanged.
- **Retries:** Claude Code no longer retries requests the API marked not to retry, which could hold a turn for minutes or, in unattended retry mode, indefinitely.
- **Fixes:** long freezes with a very long list item in`CLAUDE.md` ; interrupts not taking effect while an MCP server had not answered; subagents unable to use MCP tools from servers that connect late;`SubagentStop` hooks running about twice a minute; stale`MEMORY.md` and`CLAUDE.md` content after compaction; and several`/mcp` ,`/rewind` ,`/ide` , Remote Control and Cowork cloud session problems.
- **VS Code:** code blocks in replies get syntax coloring, and sessions start with`DISABLE_TELEMETRY` set while VS Code's telemetry setting is off.
- **Claude Tag and Code Review:** Claude Tag analytics gains a Skills view; Code Review retries once automatically when a review session fails to start from a transient server fault.

Daisy Hollman of the Claude Code team wrote on X that `--dangerously-skip-permissions` still blocks some obvious commands such as `rm -rf ~`, and that this release adds an environment variable, `CLAUDE_CODE_EVEN_MORE_DANGEROUSLY_SKIP_PERMISSIONS=1`, that turns that block off. The post had about 3,900 likes and 198 replies by October 29.

## Sources

- v2.1.311claude
