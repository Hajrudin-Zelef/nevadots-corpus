---
id: collect-261121-veille-ia/claude-anthropic/claude-code-changelog-claude-code-docs-3
title: "Claude Code changelog - Claude Code Docs"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["claude", "tool", "code", "added", "mode", "instead", "json", "vscode"]
source: https://code.claude.com/docs/en/changelog
source_anchor: ""
source_lines: [3001, 4500]
section: "Claude_Anthropic"
---
- [VSCode] Voice dictation now respects the `accessibility.voice.speechLanguage` setting when no Claude Code language is configured
- [VSCode] `/context` now opens a native token usage dialog
- Reduced peak file descriptor usage during `find` in the Bash tool on large directory trees

**Added**

- Added `claude ultrareview [target]` subcommand to run`/ultrareview` non-interactively from CI or scripts — prints findings to stdout (`--json` for raw output) and exits 0 on completion or 1 on failure

**Changed**

- Windows: Git for Windows (Git Bash) is no longer required — when absent, Claude Code uses PowerShell as the shell tool
- Skills can now reference the current effort level with `${CLAUDE_EFFORT}` in their content
- Set `AI_AGENT` environment variable for subprocesses so`gh` can attribute traffic to Claude Code
- Spinner tips that recommend installing the desktop app or creating skills/agents are now hidden when you already have them
- Show a “use PgUp/PgDn to scroll” hint when the terminal sends arrow keys instead of scroll events
- Faster session start when you have many claude.ai connectors configured but not authorized
- The auto mode denial message now links to the configuration docs
- `claude plugin validate` now accepts`$schema` ,`version` , and`description` at the top level of`marketplace.json` and`$schema` in`plugin.json`
- Auto-compact in auto mode now displays `auto` (lowercase, no token count) instead of a misleading token value
- [VSCode] `/usage` now opens the native Account & Usage dialog instead of returning plain-text session cost
- [VSCode] Voice dictation now respects the `language` setting in`~/.claude/settings.json`

**Added**

- Added `prUrlTemplate` setting to point the footer PR badge at a custom code-review URL instead of github.com
- Added `CLAUDE_CODE_HIDE_CWD` environment variable to hide the working directory in the startup logo

**Changed**

- `/config` settings (theme, editor mode, verbose, etc.) now persist to`~/.claude/settings.json` and participate in project/local/policy override precedence
- `--from-pr` now accepts GitLab merge-request, Bitbucket pull-request, and GitHub Enterprise PR URLs
- `--print` mode now honors the agent’s`tools:` and`disallowedTools:` frontmatter, matching interactive-mode behavior
- `--agent <name>` now honors the agent definition’s`permissionMode` for built-in agents
- PowerShell tool commands can now be auto-approved in permission mode, matching Bash behavior
- Hooks: `PostToolUse` and`PostToolUseFailure` hook inputs now include`duration_ms` (tool execution time, excluding permission prompts and PreToolUse hooks)
- Subagent and SDK MCP server reconfiguration now connects servers in parallel instead of serially
- Plugins pinned by another plugin’s version constraint now auto-update to the highest satisfying git tag
- Vim mode: Esc in INSERT no longer pulls a queued message back into the input; press Esc again to interrupt
- Slash command suggestions now highlight the characters that matched your query
- Slash command picker now wraps long descriptions onto a second line instead of truncating
- `owner/repo#N` shorthand links in output now use your git remote’s host instead of always pointing at github.com
- Security: `blockedMarketplaces` now correctly enforces`hostPattern` and`pathPattern` entries
- OpenTelemetry: `tool_result` and`tool_decision` events now include`tool_use_id` ;`tool_result` also includes`tool_input_size_bytes`
- Status line: stdin JSON now includes `effort.level` and`thinking.enabled`
- Tool search is now disabled by default on Vertex AI to avoid an unsupported beta header error (opt in with `ENABLE_TOOL_SEARCH` )
- Windows: removed false-positive “Windows requires ‘cmd /c’ wrapper” MCP config warning

**Added**

- Added vim visual mode (`v` ) and visual-line mode (`V` ) with selection, operators, and visual feedback
- Added `DISABLE_UPDATES` env var to completely block all update paths including manual`claude update` — stricter than`DISABLE_AUTOUPDATER`
- Added a “Don’t ask again” option to the auto mode opt-in prompt
- Added `claude plugin tag` to create release git tags for plugins with version validation

**Changed**

- Merged `/cost` and`/stats` into`/usage` — both remain as typing shortcuts that open the relevant tab
- Create and switch between named custom themes from `/theme` , or hand-edit JSON files in`~/.claude/themes/` ; plugins can also ship themes via a`themes/` directory
- Hooks can now invoke MCP tools directly via `type: "mcp_tool"`
- WSL on Windows can now inherit Windows-side managed settings via the `wslInheritsWindowsSettings` policy key
- Auto mode: include `"$defaults"` in`autoMode.allow` ,`autoMode.soft_deny` , or`autoMode.environment` to add custom rules alongside the built-in list instead of replacing it
- `--continue` /`--resume` now find sessions that added the current directory via`/add-dir`
- `/color` now syncs the session accent color to claude.ai/code when Remote Control is connected
- The `/model` picker now honors`ANTHROPIC_DEFAULT_*_MODEL_NAME` /`_DESCRIPTION` overrides when using a custom`ANTHROPIC_BASE_URL` gateway
- When auto-update skips a plugin due to another plugin’s version constraint, the skip now appears in `/doctor` and the`/plugin` Errors tab

**Changed**

- Forked subagents can now be enabled on external builds by setting `CLAUDE_CODE_FORK_SUBAGENT=1`
- Agent frontmatter `mcpServers` are now loaded for main-thread agent sessions via`--agent`
- The `/resume` command now offers to summarize stale, large sessions before re-reading them, matching the existing`--resume` behavior
- Faster startup when both local and claude.ai MCP servers are configured (concurrent connect now default)
- `plugin install` on an already-installed plugin now installs any missing dependencies instead of stopping at “already installed”
- Plugin dependency errors now say “not installed” with an install hint, and `claude plugin marketplace add` now auto-resolves missing dependencies from configured marketplaces
- Managed-settings `blockedMarketplaces` and`strictKnownMarketplaces` are now enforced on plugin install, update, refresh, and autoupdate
- Advisor Tool (experimental): dialog now carries an “experimental” label, learn-more link, and startup notification when enabled; sessions no longer get stuck with “Advisor tool result content could not be processed” errors on every prompt and `/compact`
- The `cleanupPeriodDays` retention sweep now also covers`~/.claude/tasks/` ,`~/.claude/shell-snapshots/` , and`~/.claude/backups/`
- OpenTelemetry: `user_prompt` events now include`command_name` and`command_source` for slash commands;`cost.usage` ,`token.usage` ,`api_request` , and`api_error` now include an`effort` attribute when the model supports effort levels. Custom/MCP command names are redacted unless`OTEL_LOG_TOOL_DETAILS=1` is set
- Native builds on macOS and Linux: the `Glob` and`Grep` tools are replaced by embedded`bfs` and`ugrep` available through the Bash tool — faster searches without a separate tool round-trip (Windows and npm-installed builds unchanged)
- Windows: cached `where.exe` executable lookups per process for faster subprocess launches
- Default effort for Pro/Max subscribers on Opus 4.6 and Sonnet 4.6 is now `high` (was`medium` )

**Improved**

- Improved `/model` : selections now persist across restarts even when the project pins a different model, and the startup header shows when the active model comes from a project or managed-settings pin

**Changed**

- `/resume` on large sessions is significantly faster (up to 67% on 40MB+ sessions) and handles sessions with many dead-fork entries more efficiently
- Faster MCP startup when multiple stdio servers are configured; `resources/templates/list` is now deferred to first`@` -mention
- Smoother fullscreen scrolling in VS Code, Cursor, and Windsurf terminals — `/terminal-setup` now configures the editor’s scroll sensitivity
- Thinking spinner now shows progress inline (“still thinking”, “thinking more”, “almost done thinking”), replacing the separate hint row
- `/config` search now matches option values (e.g. searching “vim” finds the Editor mode setting)
- `/doctor` can now be opened while Claude is responding, without waiting for the current turn to finish
- `/reload-plugins` and background plugin auto-update now auto-install missing plugin dependencies from marketplaces you’ve already added
- Bash tool now surfaces a hint when `gh` commands hit GitHub’s API rate limit, so agents can back off instead of retrying
- The Usage tab in Settings now shows your 5-hour and weekly usage immediately and no longer fails when the usage endpoint is rate-limited
- Agent frontmatter `hooks:` now fire when running as a main-thread agent via`--agent`
- Slash command menu now shows “No commands match” when your filter has zero results, instead of disappearing
- Security: sandbox auto-allow no longer bypasses the dangerous-path safety check for `rm` /`rmdir` targeting`/` ,`$HOME` , or other critical system directories
- Claude Code and installer now use `https://downloads.claude.ai/claude-code-releases` instead of`https://storage.googleapis.com/claude-code-dist-86c565f3-f756-42ad-8dfa-d59b1c096819/claude-code-releases`

- Fixed a crash in the permission dialog when an agent teams teammate requested tool permission

**Added**

- Added `sandbox.network.deniedDomains` setting to block specific domains even when a broader`allowedDomains` wildcard would otherwise permit them

**Changed**

- Changed the CLI to spawn a native Claude Code binary (via a per-platform optional dependency) instead of bundled JavaScript
- Fullscreen mode: Shift+↑/↓ now scrolls the viewport when extending a selection past the visible edge
- `Ctrl+A` and`Ctrl+E` now move to the start/end of the current logical line in multiline input, matching readline behavior
- Windows: `Ctrl+Backspace` now deletes the previous word
- Long URLs in responses and bash output stay clickable when they wrap across lines (in terminals with OSC 8 hyperlinks)
- `/extra-usage` now works from Remote Control (mobile/web) clients
- Remote Control clients can now query `@` -file autocomplete suggestions
- Subagents that stall mid-stream now fail with a clear error after 10 minutes instead of hanging silently
- Bash tool: multi-line commands whose first line is a comment now show the full command in the transcript, closing a UI-spoofing vector
- Running `cd <current-directory> && git …` no longer triggers a permission prompt when the`cd` is a no-op
- Security: on macOS, `/private/{etc,var,tmp,home}` paths are now treated as dangerous removal targets under`Bash(rm:*)` allow rules
- Security: Bash deny rules now match commands wrapped in `env` /`sudo` /`watch` /`ionice` /`setsid` and similar exec wrappers
- Security: `Bash(find:*)` allow rules no longer auto-approve`find -exec` /`-delete`

**Improved**

- Improved `/loop` : pressing Esc now cancels pending wakeups, and wakeups display as “Claude resuming /loop wakeup” for clarity
- Improved `/ultrareview` : faster launch with parallelized checks, diffstat in the launch dialog, and animated launching state

**Added**

- Added `xhigh` effort level for Opus 4.7, sitting between`high` and`max` . Available via`/effort` ,`--effort` , and the model picker; other models fall back to`high`
- Added “Auto (match terminal)” theme option that matches your terminal’s dark/light mode — select it from `/theme`
- Added `/less-permission-prompts` skill — scans transcripts for common read-only Bash and MCP tool calls and proposes a prioritized allowlist for`.claude/settings.json`
- Added `/ultrareview` for running comprehensive code review in the cloud using parallel multi-agent analysis and critique — invoke with no arguments to review your current branch, or`/ultrareview <PR#>` to fetch and review a specific GitHub PR
- Added `OTEL_LOG_RAW_API_BODIES` environment variable to emit full API request and response bodies as OpenTelemetry log events for debugging

**Changed**

- Claude Opus 4.7 xhigh is now available! Use /effort to tune speed vs. intelligence
- Auto mode is now available for Max subscribers when using Opus 4.7
- `/effort` now opens an interactive slider when called without arguments, with arrow-key navigation between levels and Enter to confirm
- Auto mode no longer requires `--enable-auto-mode`
- Windows: PowerShell tool is progressively rolling out. Opt in or out with `CLAUDE_CODE_USE_POWERSHELL_TOOL` . On Linux and macOS, enable with`CLAUDE_CODE_USE_POWERSHELL_TOOL=1` (requires`pwsh` on PATH)
- Read-only bash commands with glob patterns (e.g. `ls *.ts` ) and commands starting with`cd <project-dir> &&` no longer trigger a permission prompt
- Suggest the closest matching subcommand when `claude <word>` is invoked with a near-miss typo (e.g.`claude udpate` → “Did you mean`claude update` ?”)
- Plan files are now named after your prompt (e.g. `fix-auth-race-snug-otter.md` ) instead of purely random words
- `/skills` menu now supports sorting by estimated token count — press`t` to toggle
- `Ctrl+U` now clears the entire input buffer (previously: delete to start of line); press`Ctrl+Y` to restore
- `Ctrl+L` now forces a full screen redraw in addition to clearing the prompt input
- Transcript view footer now shows `[` (dump to scrollback) and`v` (open in editor) shortcuts
- The “+N lines” marker for truncated long pastes is now a full-width rule for easier scanning
- Headless `--output-format stream-json` now includes`plugin_errors` on the init event when plugins are demoted for unsatisfied dependencies
- Suppressed spurious decompression, network, and transient error messages that could appear in the TUI during normal operation
- Reverted the v2.1.110 cap on non-streaming fallback retries — it traded long waits for more outright failures during API overload
- Windows: `CLAUDE_ENV_FILE` and SessionStart hook environment files now apply (previously a no-op)
- Windows: permission rules with drive-letter paths are now correctly root-anchored, and paths differing only by drive-letter case are recognized as the same path

**Improved**

- Improved `/setup-vertex` and`/setup-bedrock` to show the actual`settings.json` path when`CLAUDE_CONFIG_DIR` is set, seed model candidates from existing pins on re-run, and offer a “with 1M context” option for supported models
- Improved plugin error handling: dependency errors now distinguish conflicting, invalid, and overly complex version requirements; fixed stale resolved versions after `plugin update` ;`plugin install` now recovers from interrupted prior installs

**Added**

- Added `/tui` command and`tui` setting — run`/tui fullscreen` to switch to flicker-free rendering in the same conversation
- Added push notification tool — Claude can send mobile push notifications when Remote Control and “Push when Claude decides” config are enabled
- Added `autoScrollEnabled` config to disable conversation auto-scroll in fullscreen mode
- Added option to show Claude’s last response as commented context in the `Ctrl+G` external editor (enable via`/config` )

**Changed**

- Changed `Ctrl+O` to toggle between normal and verbose transcript only; focus view is now toggled separately with the new`/focus` command
- `--resume` /`--continue` now resurrects unexpired scheduled tasks
- `/context` ,`/exit` , and`/reload-plugins` now work from Remote Control (mobile/web) clients
- Write tool now informs the model when you edit the proposed content in the IDE diff before accepting
- Bash tool now enforces the documented maximum timeout instead of accepting arbitrarily large values
- SDK/headless sessions now read `TRACEPARENT` /`TRACESTATE` from the environment for distributed trace linking
- Session recap is now enabled for users with telemetry disabled (Bedrock, Vertex, Foundry, `DISABLE_TELEMETRY` ). Opt out via`/config` or`CLAUDE_CODE_ENABLE_AWAY_SUMMARY=0` .
- Hardened “Open in editor” actions against command injection from untrusted filenames

**Improved**

- Improved `/plugin` Installed tab — items needing attention and favorites appear at the top, disabled items are hidden behind a fold, and`f` favorites the selected item
- Improved `/doctor` to warn when an MCP server is defined in multiple config scopes with different endpoints

**Added**

- Added `ENABLE_PROMPT_CACHING_1H` env var to opt into 1-hour prompt cache TTL on API key, Bedrock, Vertex, and Foundry (`ENABLE_PROMPT_CACHING_1H_BEDROCK` is deprecated but still honored), and`FORCE_PROMPT_CACHING_5M` to force 5-minute TTL
- Added recap feature to provide context when returning to a session, configurable in `/config` and manually invocable with`/recap` ; force with`CLAUDE_CODE_ENABLE_AWAY_SUMMARY` if telemetry disabled.
- Added “verbose” indicator when viewing the detailed transcript (`Ctrl+O` )
- Added a warning at startup when prompt caching is disabled via `DISABLE_PROMPT_CACHING*` environment variables

**Changed**

- The model can now discover and invoke built-in slash commands like `/init` ,`/review` , and`/security-review` via the Skill tool
- `/undo` is now an alias for`/rewind`
- Reduced memory footprint for file reads, edits, and syntax highlighting by loading language grammars on demand

**Improved**

- Improved `/model` to warn before switching models mid-conversation, since the next response re-reads the full history uncached
- Improved `/resume` picker to default to sessions from the current directory; press`Ctrl+A` to show all projects
- Improved error messages: server rate limits are now distinguished from plan usage limits; 5xx/529 errors show a link to status.claude.com; unknown slash commands suggest the closest match

**Added**

- Added `path` parameter to the`EnterWorktree` tool to switch into an existing worktree of the current repository
- Added PreCompact hook support: hooks can now block compaction by exiting with code 2 or returning `{"decision":"block"}`
- Added background monitor support for plugins via a top-level `monitors` manifest key that auto-arms at session start or on skill invoke

**Changed**

- `/proactive` is now an alias for`/loop`

**Added**

- Added `/team-onboarding` command to generate a teammate ramp-up guide from your local Claude Code usage
- Added OS CA certificate store trust by default, so enterprise TLS proxies work without extra setup (set `CLAUDE_CODE_CERT_STORE=bundled` to use only bundled CAs)

**Changed**

- `/ultraplan` and other remote-session features now auto-create a default cloud environment instead of requiring web setup first

**Added**

- Added interactive Google Vertex AI setup wizard accessible from the login screen when selecting “3rd-party platform”, guiding you through GCP authentication, project and region configuration, credential verification, and model pinning
- Added `CLAUDE_CODE_PERFORCE_MODE` env var: when set, Edit/Write/NotebookEdit fail on read-only files with a`p4 edit` hint instead of silently overwriting them
- Added Monitor tool for streaming events from background scripts
- Added subprocess sandboxing with PID namespace isolation on Linux when `CLAUDE_CODE_SUBPROCESS_ENV_SCRUB` is set, and`CLAUDE_CODE_SCRIPT_CAPS` env var to limit per-session script invocations
- Added `--exclude-dynamic-system-prompt-sections` flag to print mode for improved cross-user prompt caching
- Added `workspace.git_worktree` to the status line JSON input, set whenever the current directory is inside a linked git worktree
- Added W3C `TRACEPARENT` env var to Bash tool subprocesses when OTEL tracing is enabled, so child-process spans correctly parent to Claude Code’s trace tree

**Changed**

- LSP: Claude Code now identifies itself to language servers via `clientInfo` in the initialize request
- Updated the `/claude-api` skill to cover Managed Agents alongside Claude API
- Dropped `/compact` hints when`DISABLE_COMPACT` is set.

**Added**

- Added focus view toggle (`Ctrl+O` ) in`NO_FLICKER` mode showing prompt, one-line tool summary with edit diffstats, and final response
- Added `refreshInterval` status line setting to re-run the status line command every N seconds
- Added `workspace.git_worktree` to the status line JSON input, set when the current directory is inside a linked git worktree
- Added `● N running` indicator in`/agents` next to agent types with live subagent instances
- Added syntax highlighting for Cedar policy files (`.cedar` ,`.cedarpolicy` )

**Changed**

- Updated `/claude-api` skill to cover Managed Agents alongside the Claude API

- Fixed Bedrock requests failing with `403 "Authorization header is missing"` when using`AWS_BEARER_TOKEN_BEDROCK` or`CLAUDE_CODE_SKIP_BEDROCK_AUTH` (regression in 2.1.94)

**Added**

- Added support for Amazon Bedrock powered by Mantle, set `CLAUDE_CODE_USE_MANTLE=1`
- Added compact `Slacked #channel` header with a clickable channel link for Slack MCP send-message tool calls
- Added `keep-coding-instructions` frontmatter field support for plugin output styles
- Added `hookSpecificOutput.sessionTitle` to`UserPromptSubmit` hooks for setting the session title
- [VSCode] Added a warning banner when `settings.json` files fail to parse, so users know their permission rules are not being applied

**Changed**

- Changed default effort level from medium to high for API-key, Bedrock/Vertex/Foundry, Team, and Enterprise users (control this with `/effort` )
- Plugin skills declared via `"skills": ["./"]` now use the skill’s frontmatter`name` for the invocation name instead of the directory basename, giving a stable name across install methods
- [VSCode] Reduced cold-open subprocess work on starting a session

**Improved**

- Improved `--resume` to resume sessions from other worktrees of the same repo directly instead of printing a`cd` command

**Added**

- Added `forceRemoteSettingsRefresh` policy setting: when set, the CLI blocks startup until remote managed settings are freshly fetched, and exits if the fetch fails (fail-closed)
- Added interactive Bedrock setup wizard accessible from the login screen when selecting “3rd-party platform” — guides you through AWS authentication, region configuration, credential verification, and model pinning
- Added per-model and cache-hit breakdown to `/cost` for subscription users

**Changed**

- `/release-notes` is now an interactive version picker
- Remote Control session names now use your hostname as the default prefix (e.g. `myhost-graceful-unicorn` ), overridable with`--remote-control-session-name-prefix`
- Pro users now see a footer hint when returning to a session after the prompt cache has expired, showing roughly how many tokens the next turn will send uncached
- Removed `/tag` command
- Removed `/vim` command (toggle vim mode via`/config` → Editor mode)
- Linux sandbox now ships the `apply-seccomp` helper in both npm and native builds, restoring unix-socket blocking for sandboxed commands

**Improved**

- Improved Write tool diff computation speed for large files (60% faster on files with tabs/`&` /`$` )

**Added**

- Added MCP tool result persistence override via `_meta["anthropic/maxResultSizeChars"]` annotation (up to 500K), allowing larger results like DB schemas to pass through without truncation
- Added `disableSkillShellExecution` setting to disable inline shell execution in skills, custom slash commands, and plugin commands
- Added support for multi-line prompts in `claude-cli://open?q=` deep links (encoded newlines`%0A` no longer rejected)

**Changed**

- Plugins can now ship executables under `bin/` and invoke them as bare commands from the Bash tool
- `/feedback` now explains why it’s unavailable instead of disappearing from the slash menu
- Edit tool now uses shorter `old_string` anchors, reducing output tokens

**Improved**

- Improved `/claude-api` skill guidance for agent design patterns including tool surface decisions, context management, and caching strategy
- Improved performance: faster `stripAnsi` on Bun by routing through`Bun.stripANSI`

**Added**

- Added `/powerup` — interactive lessons teaching Claude Code features with animated demos
- Added `CLAUDE_CODE_PLUGIN_KEEP_MARKETPLACE_ON_FAILURE` env var to keep the existing marketplace cache when`git pull` fails, useful in offline environments
- Added `.husky` to protected directories (acceptEdits mode)

**Changed**

- Hardened PowerShell tool permission checks: fixed trailing `&` background job bypass,`-ErrorAction Break` debugger hang, archive-extraction TOCTOU, and parse-fail fallback deny-rule degradation
- Changed `--resume` picker to no longer show sessions created by`claude -p` or SDK invocations
- Removed `Get-DnsClientCache` and`ipconfig /displaydns` from auto-allow (DNS cache privacy)

**Improved**

- Improved performance: eliminated per-turn JSON.stringify of MCP tool schemas on cache-key lookup
- Improved performance: SSE transport now handles large streamed frames in linear time (was quadratic)
- Improved performance: SDK sessions with long conversations no longer slow down quadratically on transcript writes
- Improved `/resume` all-projects view to load project sessions in parallel, improving load times for users with many projects

**Added**

- Added `"defer"` permission decision to`PreToolUse` hooks — headless sessions can pause at a tool call and resume with`-p --resume` to have the hook re-evaluate
- Added `CLAUDE_CODE_NO_FLICKER=1` environment variable to opt into flicker-free alt-screen rendering with virtualized scrollback
- Added `PermissionDenied` hook that fires after auto mode classifier denials — return`{retry: true}` to tell the model it can retry
- Added named subagents to `@` mention typeahead suggestions
- Added `MCP_CONNECTION_NONBLOCKING=true` for`-p` mode to skip the MCP connection wait entirely, and bounded`--mcp-config` server connections at 5s instead of blocking on the slowest server

**Changed**

- Auto mode: denied commands now show a notification and appear in `/permissions` → Recent tab where you can retry with`r`
- Changed `Edit` to work on files viewed via`Bash` with`sed -n` or`cat` , without requiring a separate`Read` call first
- Changed hook output over 10,000 characters to be saved to disk with a file path + a 2,000-character preview instead of being injected directly into context
- Changed `cleanupPeriodDays: 0` in settings.json to be rejected with a validation error — it previously silently disabled transcript persistence
- Changed thinking summaries to no longer be generated by default in interactive sessions — set `showThinkingSummaries: true` in settings.json to restore
- Documented `TaskCreated` hook event and its blocking behavior
- Preserved task notifications when backgrounding a running command with Ctrl+B
- PowerShell tool on Windows: external-command arguments containing both a double-quote and whitespace now prompt instead of auto-allowing (PS 5.1 argument-splitting hardening)
- `/env` now applies to PowerShell tool commands (previously only affected Bash)
- `/usage` now hides redundant “Current week (Sonnet only)” bar for Pro and Enterprise plans
- Image paste no longer inserts a trailing space
- Pasting `!command` into an empty prompt now enters bash mode, matching typed`!` behavior
- `/buddy` is here for April 1st — hatch a small creature that watches you code

**Improved**

- Improved collapsed tool summary to show “Listed N directories” for `ls` /`tree` /`du` instead of “Read N files”
- Improved Bash tool to warn when a formatter/linter command modifies files you have previously read, preventing stale-edit errors
- Improved `@` -mention typeahead to rank source files above MCP resources with similar names
- Improved PowerShell tool prompt with version-appropriate syntax guidance (5.1 vs 7+)

**Added**

- Added `X-Claude-Code-Session-Id` header to API requests so proxies can aggregate requests by session without parsing the body
- Added `.jj` and`.sl` to VCS directory exclusion lists so Grep and file autocomplete don’t descend into Jujutsu or Sapling metadata

**Changed**

- Reduced startup event-loop stalls when many claude.ai MCP connectors are configured (macOS keychain cache extended from 5s to 30s)
- Reduced token overhead when mentioning files with `@` — raw string content no longer JSON-escaped
- Memory filenames in the “Saved N memories” notice now highlight on hover and open on click
- Skill descriptions in the `/skills` listing are now capped at 250 characters to reduce context usage
- Changed `/skills` menu to sort alphabetically for easier scanning
- Auto mode now shows “unavailable for your plan” when disabled by plan restrictions (was “temporarily unavailable”)
- Read tool now uses compact line-number format and deduplicates unchanged re-reads, reducing token usage

**Improved**

- Improved prompt cache hit rate for Bedrock, Vertex, and Foundry users by removing dynamic content from tool descriptions

**Added**

- Added `CLAUDE_CODE_MCP_SERVER_NAME` and`CLAUDE_CODE_MCP_SERVER_URL` environment variables to MCP`headersHelper` scripts, allowing one helper to serve multiple servers
- Added conditional `if` field for hooks using permission rule syntax (e.g.,`Bash(git *)` ) to filter when they run, reducing process spawning overhead
- Added timestamp markers in transcripts when scheduled tasks (`/loop` ,`CronCreate` ) fire
- Added trailing space after `[Image #N]` placeholder when pasting images

**Changed**

- Deep link queries (`claude-cli://open?q=…` ) now support up to 5,000 characters, with a “scroll to review” warning for long pre-filled prompts
- MCP OAuth now follows RFC 9728 Protected Resource Metadata discovery to find the authorization server
- Plugins blocked by organization policy (`managed-settings.json` ) can no longer be installed or enabled, and are hidden from marketplace views
- PreToolUse hooks can now satisfy `AskUserQuestion` by returning`updatedInput` alongside`permissionDecision: "allow"` , enabling headless integrations that collect answers via their own UI
- `tool_parameters` in OpenTelemetry tool_result events are now gated behind`OTEL_LOG_TOOL_DETAILS=1`
- Reduced UI stutter when compaction triggers on large sessions

**Improved**

- Improved @-mention file autocomplete performance on large repositories
- Improved PowerShell dangerous command detection
- Improved scroll performance with large transcripts by replacing WASM yoga-layout with a pure TypeScript implementation

**Added**

- Added PowerShell tool for Windows as an opt-in preview. Learn more at https://code.claude.com/docs/en/tools-reference#powershell-tool
- Added `ANTHROPIC_DEFAULT_{OPUS,SONNET,HAIKU}_MODEL_SUPPORTS` env vars to override effort/thinking capability detection for pinned default models for 3p (Bedrock, Vertex, Foundry), and`_MODEL_NAME` /`_DESCRIPTION` to customize the`/model` picker label
- Added `CLAUDE_STREAM_IDLE_TIMEOUT_MS` env var to configure the streaming idle watchdog threshold (default 90s)
- Added `TaskCreated` hook that fires when a task is created via`TaskCreate`
- Added `WorktreeCreate` hook support for`type: "http"` — return the created worktree path via`hookSpecificOutput.worktreePath` in the response JSON
- Added `allowedChannelPlugins` managed setting for team/enterprise admins to define a channel plugin allowlist
- Added `x-client-request-id` header to API requests for debugging timeouts
- Added idle-return prompt that nudges users returning after 75+ minutes to `/clear` , reducing unnecessary token re-caching on stale sessions
- [VSCode] Added rate limit warning banner with usage percentage and reset time

**Changed**

- Deep links (`claude-cli://` ) now open in your preferred terminal instead of whichever terminal happens to be first in the detection list
- Rules and skills `paths:` frontmatter now accepts a YAML list of globs
- MCP tool descriptions and server instructions are now capped at 2KB to prevent OpenAPI-generated servers from bloating context
- MCP servers configured both locally and via claude.ai connectors are now deduplicated — the local config wins
- Background bash tasks that appear stuck on an interactive prompt now surface a notification after ~45 seconds
- Token counts ≥1M now display as “1.5m” instead of “1512.6k”
- Global system-prompt caching now works when `ToolSearch` is enabled, including for users with MCP tools configured
- Reduced scroll-to-top resets in long sessions by making the message window immune to compaction and grouping changes
- Reduced terminal flickering when animated tool progress scrolls above the viewport
- Changed issue/PR references to only become clickable links when written as `owner/repo#123` — bare`#123` is no longer auto-linked
- Slash commands unavailable for the current auth setup (`/voice` ,`/mobile` ,`/chrome` ,`/upgrade` , etc.) are now hidden instead of shown
- Stats screenshot (Ctrl+S in /stats) now works in all builds and is 16× faster

**Added**

- Added `managed-settings.d/` drop-in directory alongside`managed-settings.json` , letting separate teams deploy independent policy fragments that merge alphabetically
- Added `CwdChanged` and`FileChanged` hook events for reactive environment management (e.g., direnv)
- Added `sandbox.failIfUnavailable` setting to exit with an error when sandbox is enabled but cannot start, instead of running unsandboxed
- Added `disableDeepLinkRegistration` setting to prevent`claude-cli://` protocol handler registration
- Added `CLAUDE_CODE_SUBPROCESS_ENV_SCRUB=1` to strip Anthropic and cloud provider credentials from subprocess environments (Bash tool, hooks, MCP stdio servers)
- Added transcript search — press `/` in transcript mode (`Ctrl+O` ) to search,`n` /`N` to step through matches
- Added `Ctrl+X Ctrl+E` as an alias for opening the external editor (readline-native binding;`Ctrl+G` still works)
- Added `CLAUDE_CODE_DISABLE_NONSTREAMING_FALLBACK` env var to disable the non-streaming fallback when streaming fails
- [VSCode] Added Esc-twice (or `/rewind` ) to open a keyboard-navigable rewind picker

**Changed**

- Pasted images now insert an `[Image #N]` chip at the cursor so you can reference them positionally in your prompt
- Agents can now declare `initialPrompt` in frontmatter to auto-submit a first turn
- `chat:killAgents` and`chat:fastMode` are now rebindable via`~/.claude/keybindings.json`
- Reduced `WebFetch` peak memory usage for large pages
- Reduced scrollback resets in long sessions from once per turn to once per ~50 messages
- Faster `claude -p` startup with unauthenticated HTTP/SSE MCP servers (~600ms saved)
- Bash ghost-text suggestions now include just-submitted commands immediately
- Increased non-streaming fallback token cap (21k → 64k) and timeout (120s → 300s local) so fallback requests are less likely to be truncated
- Interrupting a prompt before any response now automatically restores your input so you can edit and resubmit
- `/status` now works while Claude is responding, instead of being queued until the turn finishes
- Plugin MCP servers that duplicate an org-managed connector are now suppressed instead of running a second connection
- Linux: respect `XDG_DATA_HOME` when registering the`claude-cli://` protocol handler
- Changed “stop all background agents” keybinding from `Ctrl+F` to`Ctrl+X Ctrl+K` to stop shadowing readline forward-char
- Deprecated `TaskOutput` tool in favor of using`Read` on the background task’s output file path
- Plugin options (`manifest.userConfig` ) now available externally — plugins can prompt for configuration at enable time, with`sensitive: true` values stored in keychain (macOS) or protected credentials file (other platforms)
- Claude can now reference the on-disk path of clipboard-pasted images for file operations
- `Ctrl+L` now clears the screen and forces a full redraw — use this to recover when Cmd+K leaves the UI partially blank. Use`Ctrl+U` or double-Esc to clear prompt input.
- `--bare -p` (SDK pattern) is ~14% faster to the API request
- Memory: `MEMORY.md` index now truncates at 25KB as well as 200 lines
- Disabled `AskUserQuestion` and plan-mode tools when`--channels` is active
- [VSCode] Spinner now turns red with “Not responding” when the backend hasn’t responded for 60 seconds

**Added**

- Added `--bare` flag for scripted`-p` calls — skips hooks, LSP, plugin sync, and skill directory walks; requires`ANTHROPIC_API_KEY` or an`apiKeyHelper` via`--settings` (OAuth and keychain auth disabled); auto-memory fully disabled
- Added `--channels` permission relay — channel servers that declare the permission capability can forward tool approval prompts to your phone

**Changed**

- Resuming a session that was in a worktree now switches back to that worktree
- Updated MCP OAuth to support Client ID Metadata Document (CIMD / SEP-991) for servers without Dynamic Client Registration
- Changed plan mode to hide the “clear context” option by default (restore with `"showClearContextOnPlanAccept": true` )
- Disabled line-by-line response streaming on Windows (including WSL in Windows Terminal) due to rendering issues

**Improved**

- Improved MCP read/search tool calls to collapse into a single “Queried `{server}` ” line (expand with Ctrl+O)
- Improved `!` bash mode discoverability — Claude now suggests it when you need to run an interactive command
- Improved plugin freshness — ref-tracked plugins now re-clone on every load to pick up upstream changes
- Improved Remote Control session titles to refresh after your third message

**Added**

- Added `rate_limits` field to statusline scripts for displaying Claude.ai rate limit usage (5-hour and 7-day windows with`used_percentage` and`resets_at` )
- Added `source: 'settings'` plugin marketplace source — declare plugin entries inline in settings.json
- Added CLI tool usage detection to plugin tips, in addition to file pattern matching
- Added `effort` frontmatter support for skills and slash commands to override the model effort level when invoked
- Added `--channels` (research preview) — allow MCP servers to push messages into your session

**Changed**

- Simplified plugin install tips to use a single `/plugin install` command instead of a two-step flow
- Reduced memory usage on startup in large repositories (~80 MB saved on 250k-file repos)

**Improved**

- Improved responsiveness of `@` file autocomplete in large git repositories
- Improved `/effort` to show what auto currently resolves to, matching the status bar indicator
- Improved `/permissions` — Tab and arrow keys now switch tabs from within a list
- Improved background tasks panel — left arrow now closes from the list view

**Added**

- Added `--console` flag to`claude auth login` for Anthropic Console (API billing) authentication
- Added “Show turn duration” toggle to the `/config` menu
- [VSCode] Added `/remote-control` — bridge your session to claude.ai/code to continue from a browser or phone

**Changed**

- `CLAUDE_CODE_PLUGIN_SEED_DIR` now supports multiple seed directories separated by the platform path delimiter (`:` on Unix,`;` on Windows)
- [VSCode] Session tabs now get AI-generated titles based on your first message

**Improved**

- Improved startup memory usage by ~18MB across all scenarios
- Improved non-streaming API fallback with a 2-minute per-attempt timeout, preventing sessions from hanging indefinitely

**Added**

- Added `StopFailure` hook event that fires when the turn ends due to an API error (rate limit, auth failure, etc.)
- Added `${CLAUDE_PLUGIN_DATA}` variable for plugin persistent state that survives plugin updates;`/plugin uninstall` prompts before deleting it
- Added `effort` ,`maxTurns` , and`disallowedTools` frontmatter support for plugin-shipped agents
- Added `ANTHROPIC_CUSTOM_MODEL_OPTION` env var to add a custom entry to the`/model` picker, with optional`_NAME` and`_DESCRIPTION` suffixed vars for display

**Changed**

- Terminal notifications (iTerm2/Kitty/Ghostty popups, progress bar) now reach the outer terminal when running inside tmux with `set -g allow-passthrough on`
- Response text now streams line-by-line as it’s generated

**Improved**

- Improved memory usage and startup time when resuming large sessions

**Added**

- Added `allowRead` sandbox filesystem setting to re-allow read access within`denyRead` regions

**Changed**

- Increased default maximum output token limits for Claude Opus 4.6 to 64k tokens, and the upper bound for Opus 4.6 and Sonnet 4.6 models to 128k tokens
- `/copy` now accepts an optional index:`/copy N` copies the Nth-latest assistant response
- Faster startup on macOS (~60ms) by reading keychain credentials in parallel with module loading
- Faster `--resume` on fork-heavy and very large sessions — up to 45% faster loading and ~100-150MB less peak memory
- Background bash tasks are now killed if output exceeds 5GB, preventing runaway processes from filling disk
- Sessions are now auto-named from plan content when you accept a plan
- Show a notice when `apiKeyHelper` takes longer than 10s, preventing it from blocking the main loop
- The Agent tool no longer accepts a `resume` parameter — use`SendMessage({to: agentId})` to continue a previously spawned agent
- `SendMessage` now auto-resumes stopped agents in the background instead of returning an error
- Renamed `/fork` to`/branch` (`/fork` still works as an alias)
- [VSCode] When option+click doesn’t trigger native selection on macOS, the footer now points to the `macOptionClickForcesSelection` setting

**Improved**

- Improved Esc to abort in-flight non-streaming API requests
- Improved `claude plugin validate` to check skill, agent, and command frontmatter plus`hooks/hooks.json` , catching YAML parse errors and schema violations
- Improved headless mode plugin installation to compose correctly with `CLAUDE_CODE_PLUGIN_SEED_DIR`
- [VSCode] Improved plan preview tab titles to use the plan’s heading instead of “Claude’s Plan”

**Added**

- Added MCP elicitation support — MCP servers can now request structured input mid-task via an interactive dialog (form fields or browser URL)
- Added new `Elicitation` and`ElicitationResult` hooks to intercept and override responses before they’re sent back
- Added `-n` /`--name <name>` CLI flag to set a display name for the session at startup
- Added `worktree.sparsePaths` setting for`claude --worktree` in large monorepos to check out only the directories you need via git sparse-checkout
- Added `PostCompact` hook that fires after compaction completes
- Added `/effort` slash command to set model effort level
- Added session quality survey — enterprise admins can configure the sample rate via the `feedbackSurveyRate` setting

**Changed**

- Updated `--plugin-dir` to only accept one path to support subcommands — use repeated`--plugin-dir` for multiple directories

**Added**

- Added 1M context window for Opus 4.6 by default for Max, Team, and Enterprise plans (previously required extra usage)
- Added `/color` command for all users to set a prompt-bar color for your session
- Added session name display on the prompt bar when using `/rename`
- Added last-modified timestamps to memory files, helping Claude reason about which memories are fresh vs. stale
- Added hook source display (settings/plugin/skill) in permission prompts when a hook requires confirmation

**Changed**

- Suppressed async hook completion messages by default (visible with `--verbose` or transcript mode)
- Breaking change: Removed deprecated Windows managed settings fallback at `C:\ProgramData\ClaudeCode\managed-settings.json` — use`C:\Program Files\ClaudeCode\managed-settings.json`

**Improved**

- Improved startup performance on macOS non-MDM machines by skipping unnecessary subprocess spawns

**Added**

- Added actionable suggestions to `/context` command — identifies context-heavy tools, memory bloat, and capacity warnings with specific optimization tips
- Added `autoMemoryDirectory` setting to configure a custom directory for auto-memory storage

**Changed**

- Changed `--plugin-dir` so local dev copies now override installed marketplace plugins with the same name (unless that plugin is force-enabled by managed settings)

**Improved**

- [VSCode] Improved scroll wheel responsiveness in the integrated terminal with terminal-aware acceleration

**Added**

- Added `modelOverrides` setting to map model picker entries to custom provider model IDs (e.g. Bedrock inference profile ARNs)
- Added actionable guidance when OAuth login or connectivity checks fail due to SSL certificate errors (corporate proxies, `NODE_EXTRA_CA_CERTS` )

**Changed**

- Changed default Opus model on Bedrock, Vertex, and Microsoft Foundry to Opus 4.6 (was Opus 4.1)
- Deprecated `/output-style` command — use`/config` instead. Output style is now fixed at session start for better prompt caching

**Added**

- Added `w` key in`/copy` to write the focused selection directly to a file, bypassing the clipboard (useful over SSH)
- Added optional description argument to `/plan` (e.g.,`/plan fix the auth bug` ) that enters plan mode and immediately starts
- Added `ExitWorktree` tool to leave an`EnterWorktree` session
- Added `CLAUDE_CODE_DISABLE_CRON` environment variable to immediately stop scheduled cron jobs mid-session
- Added `lsof` ,`pgrep` ,`tput` ,`ss` ,`fd` , and`fdfind` to the bash auto-approval allowlist, reducing permission prompts for common read-only operations
- Added support for marketplace git URLs without `.git` suffix (Azure DevOps, AWS CodeCommit)
- VSCode: Added effort level indicator on the input border
- VSCode: Added `vscode://anthropic.claude-code/open` URI handler to open a new Claude Code tab programmatically, with optional`prompt` and`session` query parameters

**Changed**

- Restored the `model` parameter on the Agent tool for per-invocation model overrides
- Simplified effort levels to low/medium/high (removed max) with new symbols (○ ◐ ●) and a brief notification instead of a persistent icon. Use `/effort auto` to reset to default
- Reduced bundle size by ~510 KB
- Changed CLAUDE.md HTML comments (`<!-- ... -->` ) to be hidden from Claude when auto-injected. Comments remain visible when read with the Read tool

**Added**

- Added `/loop` command to run a prompt or slash command on a recurring interval (e.g.`/loop 5m check the deploy` )
- Added cron scheduling tools for recurring prompts within a session
- Added `voice:pushToTalk` keybinding to make the voice activation key rebindable in`keybindings.json` (default: space) — modifier+letter combos like`meta+k` have zero typing interference
- Added `fmt` ,`comm` ,`cmp` ,`numfmt` ,`expr` ,`test` ,`printf` ,`getconf` ,`seq` ,`tsort` , and`pr` to the bash auto-approval allowlist

**Changed**

- Updated `/debug` to toggle debug logging on mid-session, since debug logs are no longer written by default
- Removed startup notification noise for unauthenticated org-registered claude.ai connectors

**Improved**

- Improved startup time by deferring native image processor loading to first use
- Improved bridge session reconnection to complete within seconds after laptop wake from sleep, instead of waiting up to 10 minutes
- Improved `/plugin uninstall` to disable project-scoped plugins in`.claude/settings.local.json` instead of modifying`.claude/settings.json` , so changes don’t affect teammates
- Improved plugin-provided MCP server deduplication — servers that duplicate a manually-configured server (same command/URL) are now skipped, preventing duplicate connections and tool sets. Suppressions are shown in the `/plugin` menu.

**Added**

- [VSCode] Added spark icon in VS Code activity bar that lists all Claude Code sessions, with sessions opening as full editors
- [VSCode] Added full markdown document view for plans in VS Code, with support for adding comments to provide feedback
- [VSCode] Added native MCP server management dialog — use `/mcp` in the chat panel to enable/disable servers, reconnect, and manage OAuth authentication without switching to the terminal

**Changed**

- Reduced prompt input re-renders during turns by ~74%
- Reduced startup memory by ~426KB for users without custom CA certificates
- Reduced Remote Control `/poll` rate to once per 10 minutes while connected (was 1–2s), cutting server load ~300×. Reconnection is unaffected — transport loss immediately wakes fast polling.

**Improved**

- Improved error message when microphone captures silence to distinguish from “no speech detected”
- Improved compaction to preserve images in the summarizer request, allowing prompt cache reuse for faster and cheaper compaction
- Improved `/rename` to work while Claude is processing, instead of being silently queued

**Added**

- Added the `/claude-api` skill for building applications with the Claude API and Anthropic SDK
- Added Ctrl+U on an empty bash prompt (`!` ) to exit bash mode, matching`escape` and`backspace`
- Added numeric keypad support for selecting options in Claude’s interview questions (previously only the number row above QWERTY worked)
- Added optional name argument to `/remote-control` and`claude remote-control` (`/remote-control My Project` or`--name "My Project"` ) to set a custom session title visible in claude.ai/code
- Added Voice STT support for 10 new languages (20 total) — Russian, Polish, Turkish, Dutch, Ukrainian, Greek, Czech, Danish, Swedish, Norwegian
- Added effort level display (e.g., “with low effort”) to the logo and spinner, making it easier to see which effort setting is active
- Added agent name display in terminal title when using `claude --agent`
- Added `sandbox.enableWeakerNetworkIsolation` setting (macOS only) to allow Go programs like`gh` ,`gcloud` , and`terraform` to verify TLS certificates when using a custom MITM proxy with`httpProxyPort`
- Added `includeGitInstructions` setting (and`CLAUDE_CODE_DISABLE_GIT_INSTRUCTIONS` env var) to remove built-in commit and PR workflow instructions from Claude’s system prompt
- Added `/reload-plugins` command to activate pending plugin changes without restarting
- Added a one-time startup prompt suggesting Claude Code Desktop on macOS and Windows (max 3 showings, dismissible)
- Added `${CLAUDE_SKILL_DIR}` variable for skills to reference their own directory in SKILL.md content
- Added `InstructionsLoaded` hook event that fires when CLAUDE.md or`.claude/rules/*.md` files are loaded into context
- Added `agent_id` (for subagents) and`agent_type` (for subagents and`--agent` ) to hook events
- Added `worktree` field to status line hook commands with name, path, branch, and original repo directory when running in a`--worktree` session
- Added `pluginTrustMessage` in managed settings to append organization-specific context to the plugin trust warning shown before installation
- Added policy limit fetching (e.g., remote control restrictions) for Team plan OAuth users, not just Enterprise
- Added `pathPattern` to`strictKnownMarketplaces` for regex-matching file/directory marketplace sources alongside`hostPattern` restrictions
- Added plugin source type `git-subdir` to point to a subdirectory within a git repo
- Added `oauth.authServerMetadataUrl` config option for MCP servers to specify a custom OAuth metadata discovery URL when standard discovery fails
- [VSCode] Added compaction display as a collapsible “Compacted chat” card with the summary inside

**Changed**

- Reduced baseline memory by ~16MB by deferring Yoga WASM preloading
- Reduced memory footprint for SDK and CCR sessions using stream-json output
- Reduced memory usage when resuming large sessions (including compacted history)
- Reduced token usage on multi-agent tasks with more concise subagent final reports
- Changed Sonnet 4.5 users on Pro/Max/Team Premium to be automatically migrated to Sonnet 4.6
- Changed the `/resume` picker to show your most recent prompt instead of the first one. This also resolves some titles appearing as`(session)` .
- Changed claude.ai MCP connector failures to show a notification instead of silently disappearing from the tool list
- Changed example command suggestions to be generated deterministically instead of calling Haiku
- Changed resuming after compaction to no longer produce a preamble recap before continuing
- [SDK] Changed task creation to no longer require the `activeForm` field — the spinner falls back to the task subject
- [VSCode] The permission mode picker now respects `permissions.disableBypassPermissionsMode` from your effective Claude Code settings (including managed/policy settings) — when set to`disable` , bypass permissions mode is hidden from the picker

- Opus 4.6 now defaults to medium effort for Max and Team subscribers. Medium effort works well for most tasks — it’s the sweet spot between speed and thoroughness. You can change this anytime with `/model`
- Re-introduced the “ultrathink” keyword to enable high effort for the next turn
- Removed Opus 4 and 4.1 from Claude Code on the first-party API — users with these models pinned are automatically moved to Opus 4.6

**Added**

- Added `/simplify` and`/batch` bundled slash commands
- Added `ENABLE_CLAUDEAI_MCP_SERVERS=false` env var to opt out from making claude.ai MCP servers available
- Added HTTP hooks, which can POST JSON to a URL and receive JSON instead of running a shell command
- Added manual URL paste fallback during MCP OAuth authentication. If the automatic localhost redirect doesn’t work, you can paste the callback URL to complete authentication.
- Added “Always copy full response” option to the `/copy` picker. When selected, future`/copy` commands will skip the code block picker and copy the full response directly.
- VSCode: Added session rename and remove actions to the sessions list

**Changed**

- Project configs & auto memory now shared across git worktrees of the same repository

**Improved**

- Improved `/model` command to show the currently active model in the slash command menu
- Improved memory usage in long sessions with subagents by stripping heavy progress message payloads during context compaction

- Claude automatically saves useful context to auto-memory. Manage with /memory
- Added `/copy` command to show an interactive picker when code blocks are present, allowing selection of individual code blocks or the full response.
- Improved “always allow” prefix suggestions for compound bash commands (e.g. `cd /tmp && git fetch && git push` ) to compute smarter per-subcommand prefixes instead of treating the whole command as one
- Improved ordering of short task lists
- Improved memory usage in multi-agent sessions by releasing completed subagent task state
- Fixed MCP OAuth token refresh race condition when running multiple Claude Code instances simultaneously
- Fixed shell commands not showing a clear error message when the working directory has been deleted
- Fixed config file corruption that could wipe authentication when multiple Claude Code instances ran simultaneously

- VS Code: Fixed another cause of “command ‘claude-vscode.editor.openLast’ not found” crashes

- Fixed a UI flicker where user input would briefly disappear after submission before the message rendered
- Fixed bulk agent kill (ctrl+f) to send a single aggregate notification instead of one per agent, and to properly clear the command queue
- Fixed graceful shutdown sometimes leaving stale sessions when using Remote Control by parallelizing teardown network calls
- Fixed `--worktree` sometimes being ignored on first launch
- Fixed a panic (“switch on corrupted value”) on Windows
- Fixed a crash that could occur when spawning many processes on Windows
- Fixed a crash in the WebAssembly interpreter on Linux x64 & Windows x64
- Fixed a crash that sometimes occurred after 2 minutes on Windows ARM64

- VS Code: Fixed extension crash on Windows (“command ‘claude-vscode.editor.openLast’ not found”)

**Added**

- Added `claude remote-control` subcommand for external builds, enabling local environment serving for all users.
- Added support for custom npm registries and specific version pinning when installing plugins from npm sources
- Added `CLAUDE_CODE_ACCOUNT_UUID` ,`CLAUDE_CODE_USER_EMAIL` , and`CLAUDE_CODE_ORGANIZATION_UUID` environment variables for SDK callers to provide account info synchronously, eliminating a race condition where early telemetry events lacked account metadata.

**Changed**

- Updated plugin marketplace default git timeout from 30s to 120s and added `CLAUDE_CODE_PLUGIN_GIT_TIMEOUT_MS` to configure.
- BashTool now skips login shell (`-l` flag) by default when a shell snapshot is available, improving command execution performance. Previously this required setting`CLAUDE_BASH_NO_LOGIN=true` .
- Tool results larger than 50K characters are now persisted to disk (previously 100K). This reduces context window usage and improves conversation longevity.
- The `/model` picker now shows human-readable labels (e.g., “Sonnet 4.5”) instead of raw model IDs for pinned model versions, with an upgrade hint when a newer version is available.
- Managed settings can now be set via macOS plist or Windows Registry. Learn more at https://code.claude.com/docs/en/settings#settings-files

**Fixed**

- Fixed a security issue where `statusLine` and`fileSuggestion` hook commands could execute without workspace trust acceptance in interactive mode.
- Fixed a bug where duplicate `control_response` messages (e.g. from WebSocket reconnects) could cause API 400 errors by pushing duplicate assistant messages into the conversation.
- Fixed slash command autocomplete crashing when a plugin’s SKILL.md description is a YAML array or other non-string type

**Added**

- Added support for `startupTimeout` configuration for LSP servers
- Added `WorktreeCreate` and`WorktreeRemove` hook events, enabling custom VCS setup and teardown when agent worktree isolation creates or removes worktrees.
- Added support for `isolation: worktree` in agent definitions, allowing agents to declaratively run in isolated git worktrees.
- Added `claude agents` CLI command to list all configured agents
- Added `CLAUDE_CODE_DISABLE_1M_CONTEXT` environment variable to disable 1M context window support
- VSCode: Added `/extra-usage` command support in VS Code sessions

**Changed**

- `CLAUDE_CODE_SIMPLE` mode now also disables MCP tools, attachments, hooks, and CLAUDE.md file loading for a fully minimal experience.
- Opus 4.6 (fast mode) now includes the full 1M context window

**Improved**

- Improved memory usage during long sessions by clearing internal caches after compaction
- Improved memory usage during long sessions by clearing large tool results after they have been processed
- Improved startup performance for headless mode (`-p` flag) by deferring Yoga WASM and UI component imports

**Added**

- Added `--worktree` (`-w` ) flag to start Claude in an isolated git worktree
- Added Ctrl+F keybinding to kill background agents (two-press confirmation)
- Added `ConfigChange` hook event that fires when configuration files change during a session, enabling enterprise security auditing and optional blocking of settings changes.

**Changed**

- Subagents support `isolation: "worktree"` for working in a temporary git worktree
- Agent definitions support `background: true` to always run as a background task
- Plugins can ship `settings.json` for default configuration
- Simple mode (`CLAUDE_CODE_SIMPLE` ) now includes the file edit tool in addition to the Bash tool, allowing direct file editing in simple mode.
- Permission suggestions are now populated when safety checks trigger an ask response, enabling SDK consumers to display permission options
- Sonnet 4.5 with 1M context is being removed from the Max plan in favor of our frontier Sonnet 4.6 model, which now has 1M context. Please switch in /model.
- SDK model info now includes `supportsEffort` ,`supportedEffortLevels` , and`supportsAdaptiveThinking` fields so consumers can discover model capabilities.

**Added**

- Added `last_assistant_message` field to Stop and SubagentStop hook inputs, providing the final assistant response text so hooks can access it without parsing transcript files.
- Added `chat:newline` keybinding action for configurable multi-line input (anthropics/claude-code#26075)
- Added `added_dirs` to the statusline JSON`workspace` section, exposing directories added via`/add-dir` to external scripts (anthropics/claude-code#26096)

**Changed**

- Search patterns in collapsed tool results are now displayed in quotes for clarity
- Use `ctrl+f` to kill all background agents instead of double-pressing ESC. Background agents now continue running when you press ESC to cancel the main thread, giving you more control over agent lifecycle.
- Simplified teammate navigation to use only Shift+Down (with wrapping) instead of both Shift+Up and Shift+Down.
- Increased initial session count in resume picker from 10 to 50 for faster session discovery (anthropics/claude-code#26123)
- Moved config backup files from home directory root to `~/.claude/backups/` to reduce home directory clutter (anthropics/claude-code#26130)
- The `/rename` command now updates the terminal tab title by default (anthropics/claude-code#25789)

- Fixed orphaned CC processes after terminal disconnect on macOS
- Added support for using claude.ai MCP connectors in Claude Code

**Added**

- Added support for Claude Sonnet 4.6
- Added support for reading `enabledPlugins` and`extraKnownMarketplaces` from`--add-dir` directories
- Added `spinnerTipsOverride` setting to customize spinner tips — configure`tips` with an array of custom tip strings, and optionally set`excludeDefault: true` to show only your custom tips instead of the built-in ones
- Added `SDKRateLimitInfo` and`SDKRateLimitEvent` types to the SDK, enabling consumers to receive rate limit status updates including utilization, reset times, and overage information

**Improved**

- Improved startup performance by removing eager loading of session history for stats caching
- Improved memory usage for shell commands that produce large output — RSS no longer grows unboundedly with command output size
- Improved collapsed read/search groups to show the current file or search pattern being processed beneath the summary line while active
- [VSCode] Improved permission destination choice (project/user/session) to persist across sessions

- Fixed ENAMETOOLONG errors for deeply-nested directory paths
- Fixed auth refresh errors

- Fixed AWS auth refresh hanging indefinitely by adding a 3-minute timeout
- Fixed spurious warnings for non-agent markdown files in `.claude/agents/` directory
- Fixed structured-outputs beta header being sent unconditionally on Vertex/Bedrock

- Improved startup performance by deferring Zod schema construction
- Improved prompt cache hit rates by moving date out of system prompt
- Added one-time Opus 4.6 effort callout for eligible users
- Fixed /resume showing interrupt messages as session titles
- Fixed image dimension limit errors to suggest /compact

**Added**

- Added guard against launching Claude Code inside another Claude Code session
- Added `speed` attribute to OTel events and trace spans for fast mode visibility
- Added `claude auth login` ,`claude auth status` , and`claude auth logout` CLI subcommands
- Added Windows ARM64 (win32-arm64) native binary support

**Improved**

- Improved model error messages for Bedrock/Vertex/Foundry users with fallback suggestions
- Improved `/rename` to auto-generate session name from conversation context when called without arguments
- Improved narrow terminal layout for prompt footer

- Improved terminal rendering performance
- Fixed fatal errors being swallowed instead of displayed
- Fixed process hanging after session close
- Fixed character loss at terminal screen boundary
- Fixed blank lines in verbose transcript view

- Fixed VS Code terminal scroll-to-top regression introduced in 2.1.37
- Fixed Tab key queueing slash commands instead of autocompleting
- Fixed bash permission matching for commands using environment variable wrappers
- Fixed text between tool uses disappearing when not using streaming
- Fixed duplicate sessions when resuming in VS Code extension
- Improved heredoc delimiter parsing to prevent command smuggling
- Blocked writes to `.claude/skills` directory in sandbox mode

- Fixed an issue where /fast was not immediately available after enabling /extra-usage

- Fixed a crash when agent teams setting changed between renders
- Fixed a bug where commands excluded from sandboxing (via `sandbox.excludedCommands` or`dangerouslyDisableSandbox` ) could bypass the Bash ask permission rule when`autoAllowBashIfSandboxed` was enabled

**Added**

- Added `TeammateIdle` and`TaskCompleted` hook events for multi-agent workflows
- Added support for restricting which sub-agents can be spawned via `Task(agent_type)` syntax in agent “tools” frontmatter
- Added `memory` frontmatter field support for agents, enabling persistent memory with`user` ,`project` , or`local` scope
- Added plugin name to skill descriptions and `/skills` menu for better discoverability
- VSCode: Added support for remote sessions, allowing OAuth users to browse and resume sessions from claude.ai
- VSCode: Added git branch and message count to the session picker, with support for searching by branch name

**Changed**

- Errors from invalid managed settings are now surfaced

**Improved**

- Improved error messages for API connection failures — now shows specific cause (e.g., ECONNREFUSED, SSL errors) instead of generic “Connection error”

**Added**

- Added research preview agent teams feature for multi-agent collaboration (token-intensive feature, requires setting CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1)
- Added “Summarize from here” to the message selector, allowing partial conversation summarization.
- VSCode: Added spinner when loading past conversations list

**Changed**

- Claude Opus 4.6 is now available!
- Claude now automatically records and recalls memories as it works
- Skills defined in `.claude/skills/` within additional directories (`--add-dir` ) are now loaded automatically.
- Updated —resume to re-use —agent value specified in previous conversation by default.
- Skill character budget now scales with context window (2% of context), so users with larger context windows can see more skill descriptions without truncation

**Fixed**

- Fixed `@` file completion showing incorrect relative paths when running from a subdirectory
- Fixed: Bash tool no longer throws “Bad substitution” errors when heredocs contain JavaScript template literals like `${index + 1}` , which previously interrupted tool execution
- Fixed Thai/Lao spacing vowels (สระ า, ำ) not rendering correctly in the input field
- VSCode: Fixed slash commands incorrectly being executed when pressing Enter with preceding text in the input field

**Added**

- Added session resume hint on exit, showing how to continue your conversation later
- Added support for full-width (zenkaku) space input from Japanese IME in checkbox selection

**Changed**

- Reduced layout jitter in the terminal when the spinner appears and disappears during streaming
- Removed misleading Anthropic API pricing from model selector for third-party provider (Bedrock, Vertex, Foundry) users

**Improved**

- Improved system prompts to more clearly guide the model toward using dedicated tools (Read, Edit, Glob, Grep) instead of bash equivalents (`cat` ,`sed` ,`grep` ,`find` ), reducing unnecessary bash command usage
- Improved PDF and request size error messages to show actual limits (100 pages, 20MB)

**Added**

- Added `pages` parameter to the Read tool for PDFs, allowing specific page ranges to be read (e.g.,`pages: "1-5"` ). Large PDFs (>10 pages) now return a lightweight reference when`@` mentioned instead of being inlined into context.
- Added pre-configured OAuth client credentials for MCP servers that don’t support Dynamic Client Registration (e.g., Slack). Use `--client-id` and`--client-secret` with`claude mcp add` .
- Added `/debug` for Claude to help troubleshoot the current session
- Added support for additional `git log` and`git show` flags in read-only mode (e.g.,`--topo-order` ,`--cherry-pick` ,`--format` ,`--raw` )
- Added token count, tool uses, and duration metrics to Task tool results
- Added reduced motion mode to the config
- [VSCode] Added multiline input support to the “Other” text input in question dialogs (use Shift+Enter for new lines)

**Changed**

- Changed `/model` to execute immediately instead of being queued

**Improved**

- Improved memory usage for `--resume` (68% reduction for users with many sessions) by replacing the session index with lightweight stat-based loading and progressive enrichment
- Improved `TaskStop` tool to display the stopped command/task description in the result line instead of a generic “Task stopped” message

- Fixed startup performance issues when resuming sessions that have `saved_hook_context`

- Added tool call failures and denials to debug logs
- Fixed context management validation error for gateway users, ensuring `CLAUDE_CODE_DISABLE_EXPERIMENTAL_BETAS=1` avoids the error
- Added `--from-pr` flag to resume sessions linked to a specific GitHub PR number or URL
- Sessions are now automatically linked to PRs when created via `gh pr create`
- Fixed /context command not displaying colored output
- Fixed status bar duplicating background task indicator when PR status was shown
- Windows: Fixed bash command execution failing for users with `.bashrc` files
- Windows: Fixed console windows flashing when spawning child processes
- VSCode: Fixed OAuth token expiration causing 401 errors after extended sessions

- Fixed beta header validation error for gateway users on Bedrock and Vertex, ensuring `CLAUDE_CODE_DISABLE_EXPERIMENTAL_BETAS=1` avoids the error

**Added**

- Added customizable spinner verbs setting (`spinnerVerbs` )

**Changed**

- Changed Bash commands to show timeout duration alongside elapsed time
- Changed merged pull requests to show a purple status indicator in the prompt footer

**Improved**

- Improved terminal rendering performance with optimized screen data layout

**Added**

- Added support for full-width (zenkaku) number input from Japanese IME in option selection prompts
- [VSCode] Added automatic Python virtual environment activation, ensuring `python` and`pip` commands use the correct interpreter (configurable via`claudeCode.usePythonEnvironment` setting)

**Improved**

- Improved read/search progress indicators to show “Reading…” while in progress and “Read” when complete
- Improved Claude to prefer file operation tools (Read, Edit, Write) over bash equivalents (cat, sed, awk)

**Added**

- Added arrow key history navigation in vim normal mode when cursor cannot move further
- Added external editor shortcut (Ctrl+G) to the help menu for better discoverability
- Added PR review status indicator to the prompt footer, showing the current branch’s PR state (approved, changes requested, pending, or draft) as a colored dot with a clickable link
- Added support for loading `CLAUDE.md` files from additional directories specified via`--add-dir` flag (requires setting`CLAUDE_CODE_ADDITIONAL_DIRECTORIES_CLAUDE_MD=1` )
- Added ability to delete tasks via the `TaskUpdate` tool

**Changed**

- Changed collapsed read/search groups to show present tense (“Reading”, “Searching for”) while in progress, and past tense (“Read”, “Searched for”) when complete
- Changed `ToolSearch` results to appear as a brief notification instead of inline in the conversation
- Changed the `/commit-push-pr` skill to automatically post PR URLs to Slack channels when configured via MCP tools
- Changed the `/copy` command to be available to all users
- Changed background agents to prompt for tool permissions before launching
- Changed permission rules like `Bash(*)` to be accepted and treated as equivalent to`Bash`
- Changed config backups to be timestamped and rotated (keeping 5 most recent) to prevent data loss

**Improved**

- Improved `/sandbox` command UI to show dependency status with installation instructions when dependencies are missing
- Improved thinking status text with a subtle shimmer animation
- Improved task list to dynamically adjust visible items based on terminal height
- Improved fork conversation hint to show how to resume the original session

**Added**

- Added env var `CLAUDE_CODE_ENABLE_TASKS` , set to`false` to keep the old system temporarily
- Added shorthand `$0` ,`$1` , etc. for accessing individual arguments in custom commands
- [SDK] Added replay of `queued_command` attachment messages as`SDKUserMessageReplay` events when`replayUserMessages` is enabled

**Changed**

- Changed skills without additional permissions or hooks to be allowed without requiring approval
- Changed indexed argument syntax from `$ARGUMENTS.0` to`$ARGUMENTS[0]` (bracket syntax)
- [VSCode] Enabled session forking and rewind functionality for all users

- Added customizable keyboard shortcuts. Configure keybindings per context, create chord sequences, and personalize your workflow. Run `/keybindings` to get started. Learn more at https://code.claude.com/docs/en/keybindings

- Added new task management system, including new capabilities like dependency tracking
- [VSCode] Added native plugin management support
- [VSCode] Added ability for OAuth users to browse and resume remote Claude sessions from the Sessions dialog
- Fixed out-of-memory crashes when resuming sessions with heavy subagent usage
- Fixed an issue where the “context remaining” warning was not hidden after running `/compact`
- Fixed session titles on the resume screen not respecting the user’s language setting
- [IDE] Fixed a race condition on Windows where the Claude Code sidebar view container would not appear on start

- Added deprecation notification for npm installations - run `claude install` or see https://code.claude.com/docs/en/setup for more options
- Improved UI rendering performance with React Compiler
- Fixed the “Context left until auto-compact” warning not disappearing after running `/compact`
- Fixed MCP stdio server timeout not killing child process, which could cause UI freezes

**Added**

- Added history-based autocomplete in bash mode (`!` ) - type a partial command and press Tab to complete from your bash command history
- Added search to installed plugins list - type to filter by name or description
- Added support for pinning plugins to specific git commit SHAs, allowing marketplace entries to install exact versions
- [VSCode] Added `/usage` command to display current plan usage

**Improved**

- Improved backspace to delete pasted text as a single token instead of one character at a time

- Added new `Setup` hook event that can be triggered via`--init` ,`--init-only` , or`--maintenance` CLI flags for repository setup and maintenance operations
- Added keyboard shortcut ‘c’ to copy OAuth URL when browser doesn’t open automatically during login
- Fixed a crash when running bash commands containing heredocs with JavaScript template literals like `${index + 1}`
- Improved startup to capture keystrokes typed before the REPL is fully ready
- Improved file suggestions to show as removable attachments instead of inserting text when accepted
- [VSCode] Added install count display to plugin listings
- [VSCode] Added trust warning when installing plugins

- Added `auto:N` syntax for configuring the MCP tool search auto-enable threshold, where N is the context window percentage (0-100)
- Added `plansDirectory` setting to customize where plan files are stored
- Added external editor support (Ctrl+G) in AskUserQuestion “Other” input field
- Added session URL attribution to commits and PRs created from web sessions
- Added support for `PreToolUse` hooks to return`additionalContext` to the model
- Added `${CLAUDE_SESSION_ID}` string substitution for skills to access the current session ID
- Fixed long sessions with parallel tool calls failing with an API error about orphan tool_result blocks
- Fixed MCP server reconnection hanging when cached connection promise never resolves
- Fixed Ctrl+Z suspend not working in terminals using Kitty keyboard protocol (Ghostty, iTerm2, kitty, WezTerm)

**Added**

- Added `showTurnDuration` setting to hide turn duration messages (e.g., “Cooked for 1m 6s”)
- Added ability to provide feedback when accepting permission prompts
- Added inline display of agent’s final response in task notifications, making it easier to see results without reading the full transcript file

**Changed**

- Enabled MCP tool search auto mode by default for all users. When MCP tool descriptions exceed 10% of the context window, they are automatically deferred and discovered via the MCPSearch tool instead of being loaded upfront. This reduces context usage for users with many MCP tools configured. Users can disable this by adding `MCPSearch` to`disallowedTools` in their settings.
- Changed OAuth and API Console URLs from console.anthropic.com to platform.claude.com

**Improved**

- Improved typing responsiveness by reducing memory allocation overhead in terminal rendering

**Added**

- Added search functionality to `/config` command for quickly filtering settings
- Added Updates section to `/doctor` showing auto-update channel and available npm versions (stable/latest)
- Added date range filtering to `/stats` command - press`r` to cycle between Last 7 days, Last 30 days, and All time
- Added automatic discovery of skills from nested `.claude/skills` directories when working with files in subdirectories
- Added `context_window.used_percentage` and`context_window.remaining_percentage` fields to status line input for easier context window display
- Added an error display when the editor fails during Ctrl+G

**Changed**

- Updated “Help improve Claude” setting fetch to refresh OAuth and retry when it fails due to a stale OAuth token
- Changed task notification display to cap at 3 lines with overflow summary when multiple background tasks complete simultaneously
- Changed terminal title to “Claude Code” on startup for better window identification
- Removed ability to @-mention MCP servers to enable/disable - use `/mcp enable <name>` instead

**Improved**

- Improved the external CLAUDE.md imports approval dialog to show which files are being imported and from where
- Improved the `/tasks` dialog to go directly to task details when there’s only one background task running
- Improved @ autocomplete with icons for different suggestion types and single-line formatting

- Added `CLAUDE_CODE_TMPDIR` environment variable to override the temp directory used for internal temp files, useful for environments with custom temp directory requirements

- Added `CLAUDE_CODE_DISABLE_BACKGROUND_TASKS` environment variable to disable all background task functionality including auto-backgrounding and the Ctrl+B shortcut
- Fixed “Help improve Claude” setting fetch to refresh OAuth and retry when it fails due to a stale OAuth token

**Added**

- Added release channel (`stable` or`latest` ) toggle to`/config`
- Added detection and warnings for unreachable permission rules, with warnings in `/doctor` and after saving rules that include the source of each rule and actionable fix guidance
- [VSCode] Added clickable destination selector for permission requests, allowing you to choose where settings are saved (this project, all projects, shared with team, or session only)

**Changed**

- Merged slash commands and skills, simplifying the mental model with no change in behavior
- Changed tool hook execution timeout from 60 seconds to 10 minutes

**Improved**

- Improved terminal rendering stability by preventing uncontrolled writes from corrupting cursor state
- Improved slash command suggestion readability by truncating long descriptions to 2 lines

**Added**

- Added source path metadata to images dragged onto the terminal, helping Claude understand where images originated
- Added clickable hyperlinks for file paths in tool output in terminals that support OSC 8 (like iTerm)
- Added support for Windows Package Manager (winget) installations with automatic detection and update instructions
- Added Shift+Tab keyboard shortcut in plan mode to quickly select “auto-accept edits” option
- Added `FORCE_AUTOUPDATE_PLUGINS` environment variable to allow plugin autoupdate even when the main auto-updater is disabled
- Added `agent_type` to SessionStart hook input, populated if`--agent` is specified

**Changed**

- Changed large bash command outputs to be saved to disk instead of truncated, allowing Claude to read the full content
- Changed large tool outputs to be persisted to disk instead of truncated, providing full output access via file references
- Changed `/plugins` installed tab to unify plugins and MCPs with scope-based grouping
- Deprecated Windows managed settings path `C:\ProgramData\ClaudeCode\managed-settings.json` - administrators should migrate to`C:\Program Files\ClaudeCode\managed-settings.json`
- [SDK] Changed minimum zod peer dependency to ^4.0.0

**Improved**

- Improved Option-as-Meta hint on macOS to show terminal-specific instructions for native CSIu terminals like iTerm2, Kitty, and WezTerm
- Improved error message when pasting images over SSH to suggest using `scp` instead of the unhelpful clipboard shortcut hint
- Improved permission explainer to not flag routine dev workflows (git fetch/rebase, npm install, tests, PRs) as medium risk

**Added**

- Added automatic skill hot-reload - skills created or modified in `~/.claude/skills` or`.claude/skills` are now immediately available without restarting the session
- Added support for running skills and slash commands in a forked sub-agent context using `context: fork` in skill frontmatter
- Added support for `agent` field in skills to specify agent type for execution
- Added `language` setting to configure Claude’s response language (e.g., language: “japanese”)
- Added `respectGitignore` support in`settings.json` for per-project control over @-mention file picker behavior
- Added `IS_DEMO` environment variable to hide email and organization from the UI, useful for streaming or recording sessions
- Added wildcard pattern matching for Bash tool permissions using `*` at any position in rules (e.g.,`Bash(npm *)` ,`Bash(* install)` ,`Bash(git * main)` )
- Added unified Ctrl+B backgrounding for both bash commands and agents - pressing Ctrl+B now backgrounds all running foreground tasks simultaneously
- Added support for MCP `list_changed` notifications, allowing MCP servers to dynamically update their available tools, prompts, and resources without requiring reconnection
- Added `/teleport` and`/remote-env` slash commands for claude.ai subscribers, allowing them to resume and configure remote sessions
- Added support for disabling specific agents using `Task(AgentName)` syntax in settings.json permissions or the`--disallowedTools` CLI flag
- Added hooks support to agent frontmatter, allowing agents to define PreToolUse, PostToolUse, and Stop hooks scoped to the agent’s lifecycle
- Added hooks support for skill and slash command frontmatter
- Added new Vim motions: `;` and`,` to repeat f/F/t/T motions,`y` operator for yank with`yy` /`Y` ,`p` /`P` for paste, text objects (`iw` ,`aw` ,`iW` ,`aW` ,`i"` ,`a"` ,`i'` ,`a'` ,`i(` ,`a(` ,`i[` ,`a[` ,`i{` ,`a{` ),`>>` and`<<` for indent/dedent, and`J` to join lines
- Added `/plan` command shortcut to enable plan mode directly from the prompt
- Added slash command autocomplete support when `/` appears anywhere in input, not just at the beginning
- Added `--tools` flag support in interactive mode to restrict which built-in tools Claude can use during interactive sessions
- Added `CLAUDE_CODE_FILE_READ_MAX_OUTPUT_TOKENS` environment variable to override the default file read token limit
- Added support for `once: true` config for hooks
- Added support for YAML-style lists in frontmatter `allowed-tools` field for cleaner skill declarations
- Added support for prompt and agent hook types from plugins (previously only command hooks were supported)
- Added Cmd+V support for image paste in iTerm2 (maps to Ctrl+V)
- Added left/right arrow key navigation for cycling through tabs in dialogs
- Added real-time thinking block display in Ctrl+O transcript mode
- Added filepath to full output in background bash task details dialog
- Added Skills as a separate category in the context visualization
- [VSCode] Added currently selected model name to the context menu
- [VSCode] Added descriptive labels on auto-accept permission button (e.g., “Yes, allow npm for this project” instead of “Yes, and don’t ask again”)

**Changed**

- Changed Shift+Enter to work out of the box in iTerm2, WezTerm, Ghostty, and Kitty without modifying terminal configs
- Reduced permission prompts for complex bash commands
- Multiple optimizations to improve startup performance
- Updated Atlassian MCP integration to use a more reliable default configuration (streamable HTTP)
- Changed “Interrupted” message color from red to grey for a less alarming appearance
- Removed permission prompt when entering plan mode - users can now enter plan mode without approval
- Removed underline styling from image reference links
- [SDK] Changed minimum zod peer dependency to ^4.0.0

**Added**

- Added LSP (Language Server Protocol) tool for code intelligence features like go-to-definition, find references, and hover documentation
- Added `/terminal-setup` support for Kitty, Alacritty, Zed, and Warp terminals
- Added ctrl+t shortcut in `/theme` to toggle syntax highlighting on/off
- Added syntax highlighting info to theme picker
- Added guidance for macOS users when Alt shortcuts fail due to terminal configuration
- [VSCode] Added gift tag pictogram for year-end promotion message

**Improved**

- Improved `/context` command visualization with grouped skills and agents by source, slash commands, and sorted token count

- Added clickable `[Image #N]` links that open attached images in the default viewer
- Added alt-y yank-pop to cycle through kill ring history after ctrl-y yank
- Added search filtering to the plugin discover screen (type to filter by name, description, or marketplace)
- Added support for custom session IDs when forking sessions with `--session-id` combined with`--resume` or`--continue` and`--fork-session`
- Fixed slow input history cycling and race condition that could overwrite text after message submission
- Improved `/theme` command to open theme picker directly
- Improved theme picker UI
- Improved search UX across resume session, permissions, and plugins screens with a unified SearchBox component
- [VSCode] Added tab icon badges showing pending permissions (blue) and unread completions (orange)

**Added**

- Added Claude in Chrome (Beta) feature that works with the Chrome extension (https://claude.ai/chrome) to let you control your browser directly from Claude Code
- Added scannable QR code to mobile app tip for quick app downloads
- Added loading indicator when resuming conversations for better feedback

**Changed**

- Reduced terminal flickering
- Changed thinking toggle from Tab to Alt+T to avoid accidental triggers

**Improved**

- Improved @ mention file suggestion speed (~3× faster in git repositories)
- Improved file suggestion performance in repos with `.ignore` or`.rgignore` files
- Improved settings validation errors to be more prominent

**Fixed**

- Fixed `/context` command not respecting custom system prompts in non-interactive mode
- Fixed order of consecutive Ctrl+K lines when pasting with Ctrl+Y

- Added /config toggle to enable/disable prompt suggestions
- Added `/settings` as an alias for the`/config` command
- Fixed @ file reference suggestions incorrectly triggering when cursor is in the middle of a path
- Fixed MCP servers from `.mcp.json` not loading when using`--dangerously-skip-permissions`
- Fixed permission rules incorrectly rejecting valid bash commands containing shell glob patterns (e.g., `ls *.txt` ,`for f in *.png` )
- Bedrock: Environment variable `ANTHROPIC_BEDROCK_BASE_URL` is now respected for token counting and inference profile listing
- New syntax highlighting engine for native build

**Added**

- Added Enter key to accept and submit prompt suggestions immediately (tab still accepts for editing)
- Added wildcard syntax `mcp__server__*` for MCP tool permissions to allow or deny all tools from a server
- Added auto-update toggle for plugin marketplaces, allowing per-marketplace control over automatic updates
- Added `current_usage` field to status line input, enabling accurate context window percentage calculations

**Changed**

- Removed # shortcut for quick memory entry (tell Claude to edit your CLAUDE.md instead)

**Improved**

- Improved memory usage by 3x for large conversations
- Improved resolution of stats screenshots copied to clipboard (Ctrl+S) for crisper images
- Improve UI for file creation permission dialog

**Fixed**

- Fixed input being cleared when processing queued commands while the user was typing
- Fixed prompt suggestions replacing typed input when pressing Tab
- Fixed diff view not updating when terminal is resized
- Fix thinking mode toggle in /config not persisting correctly

- Fixed IME (Input Method Editor) support for languages like Chinese, Japanese, and Korean by correctly positioning the composition window at the cursor
- Fixed a bug where disallowed MCP tools were visible to the model
- Fixed an issue where steering messages could be lost while a subagent is working
- Fixed Option+Arrow word navigation treating entire CJK (Chinese, Japanese, Korean) text sequences as a single word instead of navigating by word boundaries
- Improved plan mode exit UX: show simplified yes/no dialog when exiting with empty or missing plan instead of throwing an error
- Add support for enterprise managed settings. Contact your Anthropic account team to enable this feature.

**Added**

- Added search functionality to `/permissions` command with`/` keyboard shortcut for filtering rules by tool name

**Changed**

- Thinking mode is now enabled by default for Opus 4.5
- Thinking mode configuration has moved to /config
- Show reason why autoupdater is disabled in `/doctor`

- Added ability to switch models while writing a prompt using alt+p (linux, windows), option+p (macos).
- Added context window information to status line input
- Added `fileSuggestion` setting for custom`@` file search commands
- Added `CLAUDE_CODE_SHELL` environment variable to override automatic shell detection (useful when login shell differs from actual working shell)
- Fixed prompt not being saved to history when aborting a query with Escape
- Fixed Read tool image handling to identify format from bytes instead of file extension

**Added**

- Added named session support: use `/rename` to name sessions,`/resume <name>` in REPL or`claude --resume <name>` from the terminal to resume them
- Added support for .claude/rules/`. See https://code.claude.com/docs/en/memory for details.
- Added image dimension metadata when images are resized, enabling accurate coordinate mappings for large images
- VSCode: Added copy-to-clipboard button on code blocks and bash tool inputs
- Bedrock: Add support for `aws login` AWS Management Console credentials

**Changed**

- Made auto-compacting instant
- Agents and bash commands can run asynchronously and send messages to wake up the main agent
- /stats now provides users with interesting CC stats, such as favorite model, usage graph, usage streak
- Unshipped AgentOutputTool and BashOutputTool, in favor of a new unified TaskOutputTool

**Improved**

- Improved `/resume` screen with grouped forked sessions and keyboard shortcuts for preview (P) and rename (R)
- Bedrock: Improve efficiency of token counting

**Fixed**

- Fixed auto-loading .env when using native installer
- Fixed `--system-prompt` being ignored when using`--continue` or`--resume` flags
- VSCode: Fixed extension not working on Windows ARM64 by falling back to x64 binary via emulation

- Added “(Recommended)” indicator for multiple-choice questions, with the recommended option moved to the top of the list
- Added `attribution` setting to customize commit and PR bylines (deprecates`includeCoAuthoredBy` )
- Fixed duplicate slash commands appearing when ~/.claude is symlinked to a project directory
- Fixed slash command selection not working when multiple commands share the same name
- Fixed an issue where skill files inside symlinked skill directories could become circular symlinks
- Fixed running versions getting removed because lock file incorrectly going stale
- Fixed IDE diff tab not closing when rejecting file changes

- Reverted VSCode support for multiple terminal clients due to responsiveness issues.

- Added background agent support. Agents run in the background while you work
- Added —disable-slash-commands CLI flag to disable all slash commands
- Added model name to “Co-Authored-By” commit messages
- Enabled “/mcp enable [server-name]” or “/mcp disable [server-name]” to quickly toggle all servers
- Updated Fetch to skip summarization for pre-approved websites
- VSCode: Added support for multiple terminal clients connecting to the IDE server simultaneously

- Added —agent CLI flag to override the agent setting for the current session
- Added `agent` setting to configure main thread with a specific agent’s system prompt, tool restrictions, and model
- VS Code: Fixed .claude.json config file being read from incorrect location

- Pro users now have access to Opus 4.5 as part of their subscription!
- Fixed timer duration showing “11m 60s” instead of “12m 0s”
- Windows: Managed settings now prefer `C:\Program Files\ClaudeCode` if it exists. Support for`C:\ProgramData\ClaudeCode` will be removed in a future version.

- Added feedback input when rejecting plans, allowing users to tell Claude what to change
- VSCode: Added streaming message support for real-time response display

- Added setting to enable/disable terminal progress bar (OSC 9;4)
- VSCode Extension: Added support for VS Code’s secondary sidebar (VS Code 1.97+), allowing Claude Code to be displayed in the right sidebar while keeping the file explorer on the left. Requires setting sidebar as Preferred Location in the config.

- Fixed proxy DNS resolution being forced on by default. Now opt-in via `CLAUDE_CODE_PROXY_RESOLVES_HOSTS=true` environment variable
- Fixed keyboard navigation becoming unresponsive when holding down arrow keys in memory location selector
- Improved AskUserQuestion tool to auto-submit single-select questions on the last question, eliminating the extra review screen for simple question flows
- Improved fuzzy matching for `@` file suggestions with faster, more accurate results

- Hooks: Enable PermissionRequest hooks to process ‘always allow’ suggestions and apply permission updates
- Fix issue with excessive iTerm notifications

- Fixed duplicate message display when starting Claude with a command line argument
- Fixed `/usage` command progress bars to fill up as usage increases (instead of showing remaining percentage)
- Fixed image pasting not working on Linux systems running Wayland (now falls back to wl-paste when xclip is unavailable)
- Permit some uses of `$!` in bash commands

- Added Opus 4.5! https://www.anthropic.com/news/claude-opus-4-5
- Introducing Claude Code for Desktop: https://claude.com/download
- To give you room to try out our new model, we’ve updated usage limits for Claude Code users. See the Claude Opus 4.5 blog for full details
- Pro users can now purchase extra usage for access to Opus 4.5 in Claude Code
- Plan Mode now builds more precise plans and executes more thoroughly
- Usage limit notifications now easier to understand
- Switched `/usage` back to ”% used”
- Fixed handling of thinking errors
- Fixed performance regression

- Fixed bug preventing calling MCP tools that have nested references in their input schemas
- Silenced a noisy but harmless error during upgrades
- Improved ultrathink text display
- Improved clarity of 5-hour session limit warning message

- Added readline-style ctrl-y for pasting deleted text
- Improved clarity of usage limit warning message
- Fixed handling of subagent permissions

- Improved error messages and validation for `claude --teleport`
- Improved error handling in `/usage`
- Fixed race condition with history entry not getting logged at exit
- Fixed Vertex AI configuration not being applied from `settings.json`

- Fixed image files being reported with incorrect media type when format cannot be detected from metadata

- Added support for Microsoft Foundry! See https://code.claude.com/docs/en/azure-ai-foundry
- Added `PermissionRequest` hook to automatically approve or deny tool permission requests with custom logic
- Send background tasks to Claude Code on the web by starting a message with `&`

- Added `permissionMode` field for custom agents
- Added `tool_use_id` field to`PreToolUseHookInput` and`PostToolUseHookInput` types
- Added skills frontmatter field to declare skills to auto-load for subagents
- Added the `SubagentStart` hook event
- Fixed nested `CLAUDE.md` files not loading when @-mentioning files
- Fixed duplicate rendering of some messages in the UI
- Fixed some visual flickers
- Fixed NotebookEdit tool inserting cells at incorrect positions when cell IDs matched the pattern `cell-N`

**Added**

- Added `model` parameter to prompt-based stop hooks, allowing users to specify a custom model for hook evaluation
- Plugins: Added support for sharing and installing output styles

**Changed**

- ctrl-r history search landing on a slash command no longer cancels the search
- SDK: Support custom timeouts for hooks
- Allow more safe git commands to run without approval
- Teleporting a session from web will automatically set the upstream branch

- Fixed how idleness is computed for notifications
- Hooks: Added matcher values for Notification hook events
- Output Styles: Added `keep-coding-instructions` option to frontmatter

- Fixed: DISABLE_AUTOUPDATER environment variable now properly disables package manager update notifications
- Fixed queued messages being incorrectly executed as bash commands
- Fixed input being lost when typing while a queued message is processed

- Improve fuzzy search results when searching commands
- Improved VS Code extension to respect `chat.fontSize` and`chat.fontFamily` settings throughout the entire UI, and apply font changes immediately without requiring reload
- Added `CLAUDE_CODE_EXIT_AFTER_STOP_DELAY` environment variable to automatically exit SDK mode after a specified idle duration, useful for automated workflows and scripts
- Migrated `ignorePatterns` from project config to deny permissions in the localSettings.
- Fixed menu navigation getting stuck on items with empty string or other falsy values (e.g., in the `/hooks` menu)

- VSCode Extension: Added setting to configure the initial permission mode for new conversations
- Improved file path suggestion performance with native Rust-based fuzzy finder
- Fixed infinite token refresh loop that caused MCP servers with OAuth (e.g., Slack) to hang during connection
- Fixed memory crash when reading or writing large files (especially base64-encoded images)

- Native binary installs now launch quicker.
- Fixed `claude doctor` incorrectly detecting Homebrew vs npm-global installations by properly resolving symlinks
- Fixed `claude mcp serve` exposing tools with incompatible outputSchemas

- Un-deprecate output styles based on community feedback
- Added `companyAnnouncements` setting for displaying announcements on startup
- Fixed hook progress messages not updating correctly during PostToolUse hook execution

- Windows: native installation uses shift+tab as shortcut for mode switching, instead of alt+m
- Vertex: add support for Web Search on supported models
- VSCode: Adding the respectGitIgnore configuration to include .gitignored files in file searches (defaults to true)
- Fixed a bug with subagents and MCP servers related to “Tool names must be unique” error
- Fixed issue causing `/compact` to fail with`prompt_too_long` by making it respect existing compact boundaries
- Fixed plugin uninstall not removing plugins

**Added**

- Added helpful hint to run `security unlock-keychain` when encountering API key errors on macOS with locked keychain
- Added `allowUnsandboxedCommands` sandbox setting to disable the dangerouslyDisableSandbox escape hatch at policy level
- Added `disallowedTools` field to custom agent definitions for explicit tool blocking
- Added prompt-based stop hooks
- VSCode: Added respectGitIgnore configuration to include .gitignored files in file searches (defaults to true)

**Changed**

- Enabled SSE MCP servers on native build
- Deprecated output styles. Review options in `/output-style` and use —system-prompt-file, —system-prompt, —append-system-prompt, CLAUDE.md, or plugins instead
- Removed support for custom ripgrep configuration, resolving an issue where Search returns no results and config discovery fails
- VSCode: Restored selection indicator in input footer showing current file or code selection status

- Plan mode: introduced new Plan subagent
- Subagents: claude can now choose to resume subagents
- Subagents: claude can dynamically choose the model used by its subagents
- SDK: added —max-budget-usd flag
- Discovery of custom slash commands, subagents, and output styles no longer respects .gitignore
- Stop `/terminal-setup` from adding backslash to`Shift + Enter` in VS Code
- Add branch and tag support for git-based plugins and marketplaces using fragment syntax (e.g., `owner/repo#branch` )
- Fixed a bug where macOS permission prompts would show up upon initial launch when launching from home directory
- Various other bug fixes

- New UI for permission prompts
- Added current branch filtering and search to session resume screen for easier navigation
- Fixed directory @-mention causing “No assistant message found” error
- VSCode Extension: Add config setting to include .gitignored files in file searches
- VSCode Extension: Bug fixes for unrelated ‘Warmup’ conversations, and configuration/settings occasionally being reset to defaults

- Fixed a bug where project-level skills were not loading when —setting-sources ‘project’ was specified
- Claude Code Web: Support for Web -> CLI teleport
- Sandbox: Releasing a sandbox mode for the BashTool on Linux & Mac
- Bedrock: Display awsAuthRefresh output when auth is required

- Fixed content layout shift when scrolling through slash commands
- IDE: Add toggle to enable/disable thinking.
- Fix bug causing duplicate permission prompts with parallel tool calls
- Add support for enterprise managed MCP allowlist and denylist

- Support MCP `structuredContent` field in tool responses
- Added an interactive question tool
- Claude will now ask you questions more often in plan mode
- Added Haiku 4.5 as a model option for Pro users
- Fixed an issue where queued commands don’t have access to previous messages’ output

- Auto-background long-running bash commands instead of killing them. Customize with BASH_DEFAULT_TIMEOUT_MS
- Fixed a bug where Haiku was unnecessarily called in print mode
