---
id: collect-261121-veille-ia/gemini-google/antigravity-cli-changelog-what-s-new-in-november-2026
title: "Antigravity CLI Changelog: What's New in November 2026"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["agent", "mode", "instead", "antigravity", "month", "november", "model", "remote"]
source: https://agentgid.com/agents/antigravity-cli/changelog/
source_anchor: ""
source_lines: [1, 290]
section: "Gemini_Google"
---
# Antigravity CLI changelog: what's new each month

Every stable Antigravity CLI release summarised by month: the highlights, new features, improvements, fixes and anything you need to act on. 6 months covered; the current month updates daily.

## November 2026 · so far

14 releases: 1.5.0 → 1.5.13

November brought major new capabilities including swarms for parallel agent work, support for new Gemini 4 models, and security improvements across multiple systems. The month focused on expanding the agent's abilities while refining performance and fixing various bugs.

**Action needed**

- --dangerously-skip-permissions now blocks commands on the system denylist instead of approving them automatically.

### Highlights

- Introduced swarms as a preview feature, allowing multiple worker agents to collaborate in parallel on tasks.
- Added new Gemini 4 models (Hydrogen, Carbon, and Neon) across multiple authentication methods.
- Implemented Agent Client Protocol (ACP) for editor integration with full model access and permission handling.
- Added remote agent sessions to run conversations on cloud infrastructure instead of locally.
- Launched Remote Control browser view for web-based conversation management and monitoring.

### New

- Swarms feature with /swarm command to coordinate parallel worker agents and /swarm board for live monitoring.
- Orchestrator model that plans with Gemini 4 Carbon and delegates work to other models.
- Agent Client Protocol (ACP) mode for editors to use the CLI as a language model server.
- Local projects management in /projects with ability to list, create, and switch projects.
- run_workflow tool for Python scripts that orchestrate parallel or staged subagent work.
- Split view in Remote Control browser to keep multiple conversations open side by side.
- Conversation navigator in Remote Control with preview marks for each prompt.
- Click-to-expand functionality in full screen rendering mode for tools, thoughts, and output.

### Improved

- Agent Mode now defaults to Auto in sessions with no saved mode and in headless runs where available.
- Auto mode review faster and asks less often for safe reads like GitHub API and existing clones.
- Subagent rows now show which tool they are blocked waiting on instead of generic processing status.
- Plugin OAuth credentials better protected in terminal sandbox by replacing tokens only in Authorization headers.
- Agent moves on faster after terminal commands exit, reduced from 5 seconds to about 1 second.
- Rate-limit handling now retries for delays up to 4 minutes instead of stopping the turn.
- Undo in Vim mode now works like Vim, undoing one command at a time instead of grouping edits.
- File and artifact tabs in Remote Control sidebar now open with sidebar closed for full width.

### Fixed

- File edits on Windows now handle CRLF line endings correctly.
- Agent-run commands on Windows no longer fail or return garbled text with non-ASCII output.
- Multi-second UI freezes when commands printed very long lines without spaces.
- Gemini 4 Carbon requests through enterprise model gateway failing with thinking budget error.
- Security issue where custom headers for HTTP MCP servers could be forwarded to different hosts on redirects.
- Terminal UI accepting malformed window-size reports that could freeze the CLI.

## October 2026

23 releases: 1.2.15 → 1.4.12

October brought major improvements to the CLI's responsiveness, permissions model, and diff viewing experience, along with significant new features for plugins, Auto mode, and conversation management. The month saw consistent refinements across performance, reliability, and user workflow efficiency.

**Action needed**

- Changed default Verbosity setting from high to medium; conversations now show concise summaries of related tool calls and thoughts.
- Changed j and k keys in /diff file view to move line cursor like arrow keys instead of jumping between files, matching file list and Vim behavior.
- Changed --dangerously-skip-permissions to skip only tool permission prompts, not the implementation plan review in /plan mode.
- Changed execute_url permissions so approving Google domains no longer grants access to sensitive Google apps like mail, docs, or sheets.

### Highlights

- Added Auto mode to approve routine file edits and safe commands without prompts while still requesting approval for irreversible actions.
- Introduced the /plugin interactive manager to browse, install, and configure plugins from the official marketplace with improved reliability on all platforms.
- Added /compact command to summarize conversations and free up context window on demand, with automatic compaction improvements throughout the month.
- Redesigned /diff viewer with syntax highlighting, hunk folding, word-level change highlighting, and improved navigation across multiple file views.
- Dramatically improved CLI responsiveness by reducing memory allocation on updates, optimizing rendering performance, and pausing refreshes during idle periods.

### New

- Prebuilt native Android binaries for Termux without requiring proot-distro environment.
- Announcement cards showing model launches, deprecations, and service notices.
- Pinned prompt header in no-flicker mode that stays at the top while scrolling through conversations.
- Terminal status reporting over OSC 7501 showing whether the CLI is working, waiting, done, or failed.
- Image pasting through terminal paste shortcuts and dragging image files into the prompt.
- Alt+m shortcut to send messages using the opposite of your Queued Messages setting.
- Billing model choice (Gemini subscription or Pay As You Go) after Google Cloud sign-in.
- Support for MCP servers to mark tools as always loaded via _meta field.

### Improved

- Dramatically reduced memory allocation on every update and redraw for better responsiveness while typing, scrolling, and streaming.
- Terminal sandbox on Windows no longer requires administrator rights and includes Python, Git, and npm out of the box.
- Always-allow suggestions for repo, go, and other commands now limited to specific subcommands instead of all variants.
- Permission prompts now apply across main agent and all subagents instead of asking separately.
- Agent now respects denied permission requests instead of attempting to work around them.
- Improved Auto mode review of commands leaving sandbox with faster performance and better understanding of repository context.
- Improved Remote Control with better support for tablets, foldables, and Business sign-in displays.
- Improved plugin compatibility including remote MCP servers with OAuth and plugins using relative paths.

### Fixed

- Conversation view freezing after quick terminal resizes in long conversations.
- Terminal garbled text left behind in macOS Terminal.app after exiting over SSH.
- Trackpad and mouse-wheel scrolling jumping back and forth, especially over SSH or in tmux.
- Files and folders with spaces, #, %, or other special characters being mishandled in file mentions and search results.
- Markdown tables leaving misaligned borders and repeated text in scrollback during streaming replies.
- Questions from subagents and workflow agents being lost, leaving subagents waiting forever.

## September 2026

21 releases: 1.1.23 → 1.2.14

September brought substantial improvements to reliability, responsiveness, and user experience across the CLI. The month featured expanded remote control capabilities, better error handling, and refinements to core workflows.

**Action needed**

- Default agent and subagent toolset retired find_by_name, grep_search, and list_dir tools from the baseline; custom agents can still access them.
- Changed how directory entries in skills.json, rules.json, agents.json, and plugins.json are scanned to load only items directly inside the directory.
- Plugins placed directly in ~/.gemini/config/plugins with MCP servers requiring configuration variables now start disabled until enabled.
- --print-timeout default behavior changed: when it expires mid-turn, the CLI returns partial output and exits successfully with a warning instead of failing with a timeout error.

### Highlights

- Added remote-control start and stop subcommands to run the CLI as a background service registered with your operating system.
- Introduced @<subagent> syntax to send messages directly to subagent conversations with autocomplete support.
- Expanded Vim mode with numeric count multipliers and improved word-wise navigation in the prompt editor.
- Added Kitty graphics rendering for LaTeX equations and Mermaid diagrams in the artifact viewer.
- Improved headless runs to exit promptly, report failures clearly, and default to unlimited timeout instead of 5 minutes.

### New

- Remote Control feature to connect companion UIs to active sessions for following and controlling the CLI.
- Half-page scrolling with Ctrl+D and Ctrl+U across all views.
- Workspace-grouped view toggle in /resume conversation picker with Ctrl+F.
- /model <name> <prompt> command to consult a different model mid-conversation without switching sessions.
- /copy btw subcommand to copy side-question responses to clipboard.
- /skills reload subcommand to reload discovered skills without restarting the session.
- Automatic subagent guidance in Markdown custom agents and improved agent Markdown frontmatter with agents list.
- Queued Messages option in /config to control whether follow-up messages interrupt or queue until the current turn ends.

### Improved

- Model API error resilience with longer retry windows and exponential backoff for transient failures.
- Sign-in and startup speed by reading stored credentials instead of making network requests.
- Headless runs to exit promptly, deliver partial output on timeout, and report errors with stable markers on stderr.
- Tool approval prompts to show exactly what is being approved with appropriate verbs.
- MCP panel navigation with arrow keys and improved action cycling.
- /settings panel to explain Verbosity options as you highlight them.
- Context compaction to spread user-request budget and size summaries from the model's full context window.
- Scrolling performance by coalescing mouse-wheel events before redrawing and improving rendering efficiency overall.

### Fixed

- Subagent streaming overhead and overall latency by optimizing metadata transmission and startup processes.
- Duplicate agent entries in the /agents picker when agents have the same name across configuration sources.
- MCP configuration parsing to accept single-line and multi-line comments and trailing commas in mcp_config.json.
- Tool invocations omitting tool-call IDs for Gemini models when reconstructing request history.
- Headless runs leaving daemon background processes that hung script output until end-of-file.
- Artifact viewer text selection capturing line-number gutter and fixing markdown re-wrapping after terminal resize.

## August 2026

13 releases: 1.1.10 → 1.1.22

August brought major authentication and usability improvements to Antigravity CLI, with enterprise sign-in options, Vim editing support, and numerous tool and interface enhancements. The month saw consistent refinement across interactive and print modes, with fixes for credential handling and terminal compatibility.

### Highlights

- Added Vim editing mode with modal support and Insert First toggle for seamless prompt submission without leaving edit mode.
- Enterprise users can now sign in with Business accounts, Workforce Identity Federation, or Application Default Credentials.
- Added voice dictation through /voice command to transcribe speech directly into prompts.
- Improved file path completion with typo tolerance and better ranking across large workspaces.
- Added MCP server management subcommands to configure servers without hand-editing configuration files.

### New

- Voice dictation via /voice command with microphone forwarding support through mic-serve subcommand.
- Heading outline for artifact viewer opened with t key for quick navigation of long markdown documents.
- Vim editing mode with Normal, Insert, Visual and Visual Line modes across prompt and comment editors.
- Non-interactive slash command answers in print mode for usage, quota, credits, model, effort, skills, permissions, hooks, help, changelog and config.
- MCP subcommands (add, remove, list, enable, disable) for managing MCP servers without hand-editing.
- Support for GEMINI_API_KEY environment variable to run against Gemini API without signing in.
- Markdown alert callout rendering ([!NOTE], [!TIP], [!IMPORTANT], [!WARNING], [!CAUTION]) and carousel blocks.
- Disable-slash-command flag for skill frontmatter to hide skills from the / menu while keeping them discoverable.

### Improved

- Terminal hyperlink support now works over SSH and in tmux, with word-wrapped URLs remaining clickable.
- Hook ordering improved so hooks in hooks.json run before built-in termination checks.
- Tool call headers now show a short summary of what the tool is doing instead of bare names.
- Schedule tool accepts DurationSeconds and MaxIterations as bare JSON numbers instead of requiring strings.
- Model retries honor server-supplied retry delays instead of client backoff.
- Artifact detail view wraps long lines in code files instead of clipping them.
- File path completion runs through bundled ripgrep for better performance in large workspaces.
- Terminal sandbox now grants read-only access to .git directory instead of writable access.

### Fixed

- Fixed --model and --effort flags being ignored in interactive sessions and headless runs.
- Fixed CLI exiting silently when language server failed at startup or during execution.
- Fixed Enter key being swallowed while path-suggestion popup was still loading.
- Fixed corrupted edits to files containing non-ASCII text due to inexact match splicing.
- Fixed print mode treating benign tool execution errors and permission denials as fatal failures.
- Fixed credentials and billing project being lost when CLI restarted and restored from keyring.

## July 2026

12 releases: 1.0.15 → 1.1.9

July brought major execution-mode improvements, structured output for headless runs, and comprehensive stability fixes across interactive and print modes. The month focused on usability refinements, permission handling, and support for custom agent definitions in Markdown.

**Action needed**

- Removed /fast and /planning slash commands; use /plan mode and shift+tab cycling instead.
- Default execution mode changed to request-review; set Agent Mode in /settings to change behavior.
- Changed default new project name from "Untitled" to "CLI Project" for clearer workspace identification.

### Highlights

- Added request-review execution mode with line-level diff previews for file operations, and mode cycling via shift+tab.
- Introduced structured JSON and streaming JSON output formats for headless runs, enabling machine-readable results in CI and scripts.
- Added /codesearch command with regex and literal matching to search code interactively across your workspace.
- Launched Markdown-based custom agent definitions with YAML frontmatter as an alternative to JSON.
- Added /effort command to adjust model reasoning effort on a timeline gauge for trading latency against depth.

### New

- Interactive status indicator below the input box showing active subagents and background tasks in real-time.
- Agent mode selection via --agent flag and agent/agents subcommand to choose custom agents at launch.
- In-file keyword search (/) and jump navigation (n/N) in the artifact detail viewer.
- /plan mode replacing the legacy /planning command, with consolidated mode switching.
- Full-screen diff view (f shortcut) for create-file confirmations matching file-edit experience.
- Copy-on-select in no-flickering mode to auto-copy ANSI-stripped text when mouse selection is released.
- Stacking multiple leading slash commands in a single prompt (e.g., /plan /grill-me <prompt>).
- Support for nested subagents (grandchild and deeper) with recursive subtrajectory updates.

### Improved

- /tasks detail panel now automatically scrolls to bottom as background task logs stream in.
- Model generation resilience improved with automatic client-side retries on transient errors.
- File-edit diff preview with accurate line-level diffs, 3-line context, and hunk separators.
- Interactive startup latency reduced by loading skills and MCP servers asynchronously.
- Responsiveness on large conversations (5000+ steps) by optimizing line-count methods in no-flickering mode.
- Permission prompts for compound shell commands now show the full command when any part needs approval.
- /codesearch renders results progressively with live count while loading.
- Customization discovery now sorts rules and paths deterministically to prevent prompt-cache misses.

### Fixed

- Windows print mode no longer silently discards output in non-TTY environments like pipes or subprocesses.
- Windows editor fallback now uses "edit" or "notepad" instead of attempting "vim" when auto-configured.
- Dynamically defined subagents now work by transitioning from JSON to Markdown format.
- Crash when executing background tasks or terminal commands producing empty outputs.
- Shutdown resource leaks including goroutine and database connection leaks on CLI exit.
- Print mode now hard-fails with non-zero exit when --model cannot be resolved, instead of silently downgrading.

## June 2026

2 releases: 1.0.13 → 1.0.14

June brought stability improvements and usability enhancements to the Antigravity CLI. The month focused on fixing synchronization issues, improving permission handling, and resolving bugs that affected core workflows.

### Highlights

- Goals can now run indefinitely without a length limit.
- Command permission rules are now strict by default, with optional regex matching for advanced users.
- Fixed critical synchronization bugs during UI transitions that could cause lockups.
- Subagents can now auto-approve artifacts to prevent hanging when the parent is blocked.
- Image pasting from clipboard now works in local tmux sessions.

### New

- Image pasting from clipboard in local tmux sessions.
- Auto-approve mode for subagents to handle artifacts without parent approval.
- Explicit opt-in for regex matching in command permission rules via prepended syntax.

### Improved

- Command permission security made strict by default instead of using regex matching.
- Redirection checks relaxed to allow safe commands with output redirection (e.g., tool > file).
- Exit output simplified by removing the "Resume in the same project" hint line.
- Goal command now runs indefinitely until completed or cancelled instead of timing out.

### Fixed

- Skill commands temporarily displayed without slash prefix during optimistic updates.
- UI transition lockups caused by missing synchronization mechanism.
- Undo and redo history stacks desynchronized during rapid mutations in the prompt editor.
- Browser-related prompt sections missing from the agent's prompt registry.
- Plugin import logic stripping non-skill directories like shared/.
- MCP configuration path mismatch between CLI and permission manager.

Summaries are written automatically from the official release notes (full changelog ↗); check the original notes before relying on a detail. Antigravity CLI: pricing, features and alternatives · All changelogs
