---
id: collect-261121-veille-ia/claude-anthropic/claude-code-update-adds-monitor-tool-refines-compaction-and-expands-enterprise-controls-lockdraft
title: "Claude Code Update Adds Monitor Tool, Refines Compaction, and Expands Enterprise Controls - LockDraft"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["code", "claude", "tool", "compaction", "fixed", "monitor", "enterprise", "controls"]
source: https://www.lockdraft.com/posts/claude-code-update-adds-monitor-tool-refines-compaction-and-expands-enterprise-c/
source_anchor: ""
source_lines: [1, 31]
section: "Claude_Anthropic"
---
# Claude Code Update Adds Monitor Tool, Refines Compaction, and Expands Enterprise Controls

- **Monitoring and Telemetry** : Added the Monitor tool and plugin monitors for Amazon Bedrock, Google Cloud Vertex AI, and Palantir Foundry. Monitors also activate when telemetry or nonessential traffic is disabled, and long foreground sleep calls are now blocked in these environments.
- **Gateway and Retry Fallbacks** : Resolved request failures occurring behind reverse proxies and compatible API servers that reject the`effort-2025-11-24` beta parameter or mid-conversation system messages. Claude Code now automatically retries failing requests without those elements.
- **Compaction and Memory Optimization** : Fixed an issue where context compaction following a “Prompt is too long” error retained excessive dense text, such as base64 or SVG data, leading to repeated request failures. Non-interactive sessions run via the Agent SDK or the`-p` flag now consume significantly less memory during compaction when handling large transcript files.
- **File Management and Permissions** : Corrected background cleanup deleting recently added files, such as pasted images, when`cleanupPeriodDays` was configured with a large value. File read permission dialogs now explicitly display the destination paths of symbolic links. In auto mode, initial prompts for reads outside working directories now specify that blocking those reads forces subsequent shell commands to prompt for manual permission.
- **OS and Sandbox Controls** :
  - **Linux** : Updated`/sandbox` and startup warnings to identify Ubuntu AppArmor restrictions on unprivileged user namespaces, offering direct steps to lift the restriction when bubblewrap fails to initialize.
  - **Windows** : Fixed a security issue where`PreToolUse` and`PermissionRequest` hooks allowed tool calls to execute unvetted if Git Bash was absent and the hook command consisted solely of quoted strings.
  - **Terminal Rendering** : Corrected rendering for mod`Image` elements inside tmux with`allow-passthrough` and`focus-events` enabled. Images now render properly on Kitty, Ghostty, and iTerm2 (version 3.5+) in fullscreen mode instead of displaying only alt text.
- **MCP Tool Handling** : Non-interactive Model Context Protocol (MCP) tool call timeouts during unanswered server elicitations now pass an explicit error informing Claude that no response was received, instructing it not to repeat the request. Additionally, the external editor auto-continue timeout no longer triggers immediately upon closing after an extended editing session.
- **VS Code Extension and Cloud Sessions** :
  - Fixed an issue where pressing Enter on the plan review card’s “Send feedback and keep planning” action approved the plan instead of sending comments back.
  - Fixed a “Something went wrong” panel error when choosing “Switch branch & continue” on cloud sessions opened from the Web tab in Session history.
  - Added Claude Code cloud sessions and Remote Control permissions to Enterprise custom roles, allowing Owners to enforce fine-grained access control. Admin toggles for these features now require explicit confirmation and show affected custom roles.
  - Reorganized the cloud environment dialog into row-based views for network access, setup scripts, and environment variables. Added a “Choose file” button for setup scripts and a scrollable log box showing setup script output during session setup checks.
- **Claude Tag and Code Review Admin** : Fixed`!` commands (such as`!restart` and`!help` ) being treated as plain text when formatted in bold, italic, or strikethrough. Fixed workspace load errors during failed background bundle updates and false “no matching channels” notices on spend limit filters. Improved the Code Review admin UI to retry initial repository list loads automatically.

Automated workflows executing behind corporate firewalls and custom API proxies often encounter strict header filtering. Automatic retries that omit unsupported beta parameters like `effort-2025-11-24` ensure sustained session stability without manual session resets. Memory reductions during compaction for large transcript files complement ongoing stability enhancements previously covered in Claude Code updates on terminal layout controls and telemetry diagnostics.

Properly dropping dense text such as base64 or SVG blocks during context reduction prevents token limit failure loops during long agent sessions. This refined context pruning builds on earlier updates introduced for command safeguards and MCP protocol standards. Meanwhile, enterprise administrators gain control over cloud environments by gating Remote Control features behind specific custom roles.

- **Enterprise Admins** : Check custom role configurations in administrative settings to verify that Cloud session and Remote Control permissions are assigned to authorized roles.
- **Linux Developers** : Run`/sandbox` after upgrading on Ubuntu hosts to verify whether AppArmor user namespace rules interfere with bubblewrap isolation.
- **Windows Users** : Ensure Git Bash is correctly installed in your system path if you rely on`PreToolUse` or`PermissionRequest` hooks configured with quoted string paths.

While fallback retries allow execution behind strict gateways that strip beta headers or mid-conversation system messages, sessions running through these proxies may perform differently if expected system prompt directives are omitted during retries.

### Share Article

If this article helped you, please share it with others!
