---
id: collect-261121-veille-ia/claude-anthropic/claude-code-changelog-what-s-new-in-november-2026
title: "Claude Code Changelog: What's New in November 2026"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-10-30"]
keywords: ["claude", "code", "mode", "tool", "permission", "november", "month", "improved"]
source: https://agentgid.com/agents/claude-code/changelog/
source_anchor: ""
source_lines: [1, 250]
section: "Claude_Anthropic"
---
# Claude Code changelog: what's new each month

Every stable Claude Code release summarised by month: the highlights, new features, improvements, fixes and anything you need to act on. 5 months covered; the current month updates daily.

## November 2026 · so far

11 releases: 2.1.317 → 2.1.329

November brought expanded auto mode support across cloud platforms, new Code Mode for experimental JavaScript-based tool calling, and improved security with enhanced permission checks and MCP OAuth fixes. The month also introduced Claude Fable 5.2 as the default model and numerous quality-of-life improvements for both users and administrators.

### Highlights

- Code Mode now available as an experimental opt-in feature where Claude writes JavaScript to call its tools.
- Auto mode support added for Claude Opus 4.6 and Sonnet 4.6 on Amazon Bedrock, Google Cloud's Agent Platform, Microsoft Foundry and Claude apps gateway.
- Claude Fable 5.2 introduced as the new default model with 1M context and improved pricing.
- Enhanced permission system with new denial-instead-of-prompting option and improved security checks for Bash commands and file operations.
- MCP task background execution added to move tool calls to the background automatically in terminal sessions.

### New

- Code Mode: experimental feature where Claude writes JavaScript to call tools instead of native tool calling.
- MCP task background execution: automatically moves MCP server tool calls to background in terminal sessions.
- Permissions.denyInsteadOfPrompting setting to deny tool calls instead of waiting on permission prompts.
- /claude-api latency-optimize command to identify and optimize slow sections of Claude API code.
- Claude Fable 5.2 model with 1M context window and cache read pricing.
- assume_role support on mantle upstreams for cross-account AWS access and per-developer cost attribution.
- Code Mode type checking in plugin validation and plugin version range enforcement via engines["claude-code"] in plugin.json.
- .docx support for Artifact tool asset uploads.

### Improved

- Auto mode now checks server-side for calls behind LLM gateways even with experimental features disabled.
- Permission prompts enhanced with better checks for Bash commands, file writes, and copy/move operations into directories.
- MCP OAuth requests no longer send secrets to redirected hosts.
- Session memory management improved for macOS with many concurrent Bash commands.
- Permission prompts no longer answered by keys typed immediately after sending messages.
- Better handling of model fallbacks when primary model is blocked before replying.
- Enhanced /status output showing session's scheduled tasks, loop wakeups, goals and conversation forks.
- Progress heartbeat added every 30 seconds during long hook execution to prevent timeout misinterpretation.

### Fixed

- Security issue in zstd-compressed response decoding.
- /continue sometimes losing earlier conversation after malformed tool call retries.
- MCP OAuth sending secrets to redirected hosts.
- Read-only Bash commands running outside sandbox without permission prompt.
- Bash permission checks judging file writes from wrong folder.
- Deny rules and protected-file prompts not applying to files written by cp or mv into directories.

## October 2026

26 releases: 2.1.287 → 2.1.316

October brought major plugin and customization improvements to Claude Code, including the new Claude Mods system for deeper behavior modifications and expanded AI capabilities. The month focused on reliability, security, and better integration with various cloud platforms.

**Action needed**

- Deprecated fast mode for Opus 4.8 effective October 30, 2026; will be removed November 30, 2026.
- MCP servers on protocol 2026-11-25 must add "bareElicitationCapability": true to config if they stopped connecting after the update.

### Highlights

- Claude Mods now let plugins modify deeper Claude behavior beyond basic tool use.
- Added Claude Haiku 5.5 as the default Haiku model with 1M context and lower pricing.
- New /auto-mode-setup guided configuration to teach auto mode about your project environment.
- Idle compaction now automatically summarizes long conversations before the prompt cache expires.
- Expanded platform support for Bedrock, Vertex, Foundry and other cloud upstreams.

### New

- Claude Mods system allowing plugins to hook into and modify Claude's deeper behavior.
- Built-in 'You should know' mod that flags potential issues during sessions.
- Session filtering by name with n:<text> in the agents view.
- MCP server URL prompts support for authentication flows.
- $.ui.selection() and $.ui.view() APIs for mods to read user selections and switch agent views.
- Program Status Protocol (OSC 7501) showing Claude's current state in compatible terminals.
- Idle compaction feature that automatically summarizes conversations during inactivity.
- /auto-mode-setup guided configuration wizard for environment-specific auto mode tuning.

### Improved

- Better recovery for prompts cleared with Ctrl+C by pressing Up to restore drafts.
- Faster opening of large files in plugin code panes through optimized layout.
- Improved performance of prompt hooks written as instructions for more accurate execution.
- Enhanced permission rule handling for compound shell commands.
- Better error messages for disk space issues in Bash commands.
- Faster authentication for MCP servers by handling keychain locking more gracefully.
- More accurate retention of conversation context during automatic compaction.
- Improved effort level fallback when organization caps limit a model's capabilities.

### Fixed

- Fixed fast mode staying off in remote sessions with agent-owned accounts.
- Fixed mid-response API timeouts now allowing continuation from partial responses.
- Fixed terminal freezing on short code blocks with unclosed tags or nested substitutions.
- Fixed message loss when moving a session to the background with the left arrow key.
- Fixed memory leak in HTTP MCP connections retaining all sent requests.
- Fixed requests failing when switching between Vertex or Bedrock mid-session.

## September 2026

27 releases: 2.1.257 → 2.1.286

September brought three new default models with improved capabilities and pricing, plus major usability improvements for permissions, file handling, and multi-session workflows. The month focused on reliability, safety controls, and better integration with enterprise deployments.

**Action needed**

- Auto mode for Claude API and Enterprise users now defaults to server-side classifier instead of local classifier.
- availableModels managed setting with "exact" now only allows exact model versions, blocking new releases until explicitly listed.

### Highlights

- Claude Opus 5.5, Sonnet 5.5, and Fable 5.1 are now available as default models with 1M context windows and improved cache pricing.
- New Containment Escape rule in auto mode prevents unauthorized credential fetches and cross-tenant access unless marked expected.
- Diff panel in fullscreen mode shows uncommitted changes alongside the conversation as Claude edits files.
- Fast mode now available in Claude Code Remote sessions with permission controls.
- Fixed critical issues with concurrent sessions losing changes, prompt cache invalidation, and multi-model subagent handling.

### New

- Claude Fable 5.1, Claude Opus 5.5, and Claude Sonnet 5.5 as default models with 1M context and improved cache reads.
- Time format and timezone settings for 12-hour, 24-hour, UTC, or custom strftime patterns in timestamps.
- Diff panel alongside conversation in fullscreen mode showing uncommitted file changes made by Claude.
- AGENTS.md support as an alternative to CLAUDE.md for project instructions.
- Fast mode in Claude Code Remote sessions (cloud and self-hosted runners) with organization permission controls.
- Mouse support in fullscreen mode for settings panels, skill lists, and various scrollable lists.
- Plugin eval command to run plugin test suites and get reproducible scored results with JSON and HTML reports.
- Effort level slider with keybinding actions to adjust effort without typing commands.

### Improved

- Auto mode now defaults to server-side classifier for Claude API, Enterprise, Bedrock, Vertex, and gateway users, removing classifier overhead charges.
- Permission rules with path patterns now properly handle files given as option values and compound commands in Bash.
- Read permission behavior improved with confirmation prompt before first file read outside working directories, with option to block.
- Prompt cache miss causes now shown in /cost and status line (e.g. tool definitions changed, idle past TTL).
- Concurrent sessions no longer silently revert each other's workspace trust and MCP state changes.
- Bash tool results now include a diff of changed files when the Bash tool handles file edits.
- MCP server disconnection now shows notification pointing to /mcp; automatic reconnection backed off when retrying.
- Better error messages for 401/403 errors naming which credential to refresh or pointing to gateway administrator.

### Fixed

- Claude Code no longer fails to launch on macOS 12 (Monterey).
- Concurrent sessions no longer lose changes to ~/.claude.json, workspace trust, and MCP/project state.
- Conversations with rejected thinking no longer get rejected again on every later turn.
- Permission rules with parentheses in paths no longer dropped as invalid by Bash sandbox.
- Read-only git commands no longer unexpectedly ask for permission after session has been running.
- Resumed foreground-spawned subagents no longer change their tool list and system prompt, breaking prompt-cache reuse.

## August 2026

27 releases: 2.1.221 → 2.1.252

August brought major infrastructure improvements, security hardening, and new collaboration features to Claude Code. The month focused on self-hosted environments, cross-session messaging, and enhanced sandboxing controls.

**Action needed**

- Subagent forking is now on by default: subagent type "fork" now inherits full conversation and prompt cache.
- Interactive sessions on one machine keep unique names: conflicts receive name-word-word variants instead of reusing names.

### Highlights

- Self-hosted runners let you run Claude Code sessions on your own machines or containers for Team and Enterprise plans.
- Sessions can now message each other directly using SendMessage and discover others with ListAgents, enabling collaboration across machines.
- Subagent forking is now on by default, giving spawned agents access to your full conversation and prompt cache.
- Focus view hides tool activity details behind an expandable summary for cleaner chat interaction.
- New Concise output style lets Claude skip preamble and get straight to results.

### New

- Self-hosted runners: run Claude Code on your own infrastructure with server-supplied hook support and graceful shutdown options.
- Cross-session messaging: sessions on one machine can send messages to each other and discover connected peers.
- Archive plugin source: install plugins from a ZIP file over HTTPS with optional SHA-256 pinning.
- Plugin marketplace command sources: resolve plugin directories via a local command applied each session without restart.
- Spellcheck setting: underlines misspelled words in prompt input using installed aspell, hunspell, or ispell.
- SendFeedback tool: Claude can draft feedback reports for you to review when issues occur.
- GitLab support: plugin marketplaces now support GitLab URLs and merge request tracking.
- Concise output style: new built-in option that leads with results and skips narration.

### Improved

- Focus view toggle (Ctrl+Alt+F) hides tool activity behind an expandable per-turn summary with a live running-tool indicator.
- Sandbox credential masking now supports JWT with maskClaims, structured env extraction, and AWS SigV4 re-signing.
- Slash-command menu improved: blue marks selected row, matched characters bold, emoji and accents preserved.
- MCP OAuth servers on macOS no longer intermittently fail with 401 errors after keychain timeouts.
- Permission check warnings added for Bash allow rules with wildcards before subcommands.
- Auto mode now properly releases subagent tool results to prevent unbounded memory growth in long sessions.
- Session names kept unique on one machine: name conflicts get name-word-word variants.
- Prompt caching now works for sessions using an LLM gateway or custom base URL.

### Fixed

- Bash permission bypass where zsh could execute hidden commands in regex conditionals is now blocked.
- PowerShell permission checks mishandling paths with quote characters on Windows.
- Worktree-isolated sessions were able to run destructive git commands against main checkout; isolation now applies to all session types.
- PreToolUse auto-allow hooks bypassing tool restrictions in background agent tasks.
- Long responses partly disappearing while streaming and being printed twice in terminal.
- File tools following a symlink swapped after permission check, allowing reads or writes outside approved locations.

## July 2026

17 releases: 2.1.203 → 2.1.220

July brought Claude Code significant stability improvements, new accessibility features, and enhanced security controls. The month focused on fixing regressions, improving session reliability, and expanding auto mode availability.

**Action needed**

- /verify and /code-review no longer run automatically—invoke them explicitly when needed.
- /fork now creates a background session instead of launching an in-session subagent; use /subtask for in-session delegation.

### Highlights

- Claude Opus 5 is now the default model with 1M context window and fast mode support.
- Auto mode is now available by default on Bedrock, Vertex AI, and Foundry without opt-in.
- Screen reader mode added with plain-text rendering for accessibility.
- Multiple critical fixes for background session stability and permission handling.
- New controls for runaway loops: limits on web searches and subagent spawning per session.

### New

- Screen reader mode (--ax-screen-reader) for accessible plain-text rendering.
- Vim insert mode remaps setting to map key sequences like jj to Escape.
- CLAUDECODEPROCESSWRAPPER support for corporate launcher requirements.
- Live elapsed-time counter for long-running tool calls.
- /fork command to copy conversations into new background sessions.
- Session-wide web search limit (default 200) and subagent spawn cap (default 200).
- /resume picker to browse and resume past sessions including deleted ones.
- EndConversation tool for Claude to end conversations with abusive users.

### Improved

- Background agents now auto-upgrade in the background after Claude Code updates instead of on attach.
- /cd now provides directory path suggestions matching /add-dir behavior.
- /commit-push-pr auto-allows push to the configured push remote in addition to origin.
- Gateway /login now supports Anthropic-operated public gateway endpoints.
- MCP tool calls over 2 minutes now move to background automatically.
- EnterWorktree asks for confirmation before entering worktrees outside .claude/worktrees/.
- /code-review now runs as a background subagent to keep conversation cleaner.
- Emoji shortcode autocomplete in prompt input (type :heart: for ❤️).

### Fixed

- Fixed background sessions becoming permanently unresponsive when daemon session token went stale—sessions now recover automatically.
- Fixed returning to claude agents silently stopping running subagents—their work now carries over.
- Fixed context-usage indicator re-analyzing entire transcript after every turn, causing memory and CPU regression.
- Fixed macOS background session stalling for 15-20 seconds due to false low-memory detection.
- Fixed Windows worktree removal deleting files outside the worktree when NTFS junctions existed inside.
- Fixed terminal freezing while streaming very long lists, tables, or code blocks.

Summaries are written automatically from the official release notes (full changelog ↗); check the original notes before relying on a detail. Claude Code: pricing, features and alternatives · All changelogs
