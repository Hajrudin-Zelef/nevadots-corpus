---
id: collect-261121-veille-ia/claude-anthropic/claude-code-changelog-claude-code-docs-2
title: "Claude Code changelog - Claude Code Docs"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["claude", "model", "added", "changed", "code", "session", "gateway", "fable"]
source: https://code.claude.com/docs/en/changelog
source_anchor: ""
source_lines: [1501, 3000]
section: "Claude_Anthropic"
---
**Added**

- Added Claude Fable 5.1 (`claude-fable-5-1` ), now the default Fable model — 1M context, $10/$50 per Mtok with $0.25/Mtok cache reads
- Added “Time format” (`timeFormat` ) and`timeZone` settings: 12-hour, 24-hour, 24-hour UTC, or a strftime pattern for the turn-end clock and transcript-view timestamps
- Added a Containment Escape rule to auto mode so cloud metadata-credential fetches, egress evasion, and cross-tenant reach are no longer auto-approved unless your environment marks them expected
- Added `CLAUDE_CODE_SUBAGENT_MODEL_FORCE` to apply`CLAUDE_CODE_SUBAGENT_MODEL` (or the main model) to every subagent, ignoring per-spawn and agent-definition model overrides
- Added `s` in`/effort` to change effort for the current session only, matching`/model`
- Added a `/doctor` warning for stale sandbox mask files left by a killed session
- Added a one-time prompt in auto mode before the first file read outside the working directories, with the option to block such reads (`permissions.blockReadsOutsideWorkingDirectories` )
- Added support for a gateway-supplied `description` on discovered`/model` picker entries (`CLAUDE_CODE_ENABLE_GATEWAY_MODEL_DISCOVERY` ); entries without one still read “From gateway”
- [VSCode] Added collapsible ACCOUNT & USAGE and SESSION MANAGER section headers to the session list panel, with the account email, the usage meter, and a View details link opening the usage dialog
- [VSCode] Added a model pill to the input footer that shows the current model and opens the model picker, with an Effort row and a “More models” page
- [VSCode] Added a collapse toggle to the Ungrouped section of the session list
- [VSCode] Added output style selection to the command menu, including custom styles

**Changed**

- Changed `--effort` to lift a new model’s default-effort hold for that session only rather than permanently; an effort picked on claude.ai for a Remote Control session now applies during the hold
- Changed a `policyHelper` in MDM or`managed-settings.json` shadowed at launch by cached server-managed settings to run (or exit) as soon as the fetch reports them removed, not at the next launch
- Changed `managedSourcesBehavior: "merge"` to take`sandbox.credentials.awsPairs` and`sandbox.ripgrep` whole from the highest managed source that sets them instead of combining the sources’ values
- Changed gateway model discovery (`CLAUDE_CODE_ENABLE_GATEWAY_MODEL_DISCOVERY=1` ) to run even when`CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC` is set, since it only queries your gateway
- Changed `claude --resume <session-id> --bg` to continue that session under its own ID when nothing is running it, instead of silently starting a copy; a copy is now announced
- Changed `/btw` history browsing from`←` /`→` to`Shift+←` /`Shift+→` (or`[` /`]` ), stepping through your recent side questions and back to the live answer
- Changed `defaultMode: "bypassPermissions"` in`.claude/settings.json` or`.claude/settings.local.json` to be ignored, like`"auto"` ; set it in user or managed settings, or pass`--permission-mode`
- Changed `fable` and`best` in Claude apps gateway sessions to keep resolving to Fable 5 for now, since gateways not yet configured for Fable 5.1 reject it; pick Fable 5.1 in`/model` to use it
- Changed `--add-dir` ,`/add-dir` , and`additionalDirectories` to refuse network paths (UNC shares,`/net/<host>` automounts) with a message before touching them; on Windows use a mapped drive letter
- Changed Claude apps gateway sign-in and token refresh requests to verify the gateway’s pinned TLS certificate, as the managed settings fetch already does
- Changed Cowork and claude.ai cloud sessions: reading an artifact that isn’t yours now always asks you first, even in auto mode
- Removed the Ctrl+E command explanation on Bash and PowerShell permission prompts
- [VSCode] Changed the action menu to list slash commands in a filterable “Slash commands” dialog instead of inline; picking one runs it; the MCP servers dialog gained the same filter box
- [VSCode] Changed “Delete session” to “Archive session”: archived sessions move to a collapsible “Archived sessions” group at the bottom of the list with an Unarchive action

- Fixed Bash commands failing with “task output swap refused (tasks dir moved or linked)” on some Macs
- Fixed “always allow” not saving in a project that has no .claude/settings.local.json yet
- Fixed Remote Control sessions hosted by Claude Desktop or VS Code stalling for minutes after a tool finished when the connection to claude.ai was degraded
- Fixed background task notifications with very large failure output (for example git errors on a full disk) making the conversation exceed the API request size limit

**Added**

- Added `PreModelSwitch` and`PostModelSwitch` hook events (block, confirm, or annotate a model switch);`SessionStart` resume hooks now receive session staleness and the estimated re-cache cost
- Added live streaming of a foreground subagent’s tool calls and results to Remote Control clients (background subagents, the default, still show status only)
- Added a Spend limit bar to `/usage` and a`rate_limits.spend_limit` status line field for developers behind a Claude apps gateway with spend limits
- Added a per-session prompt-cache line to `/cost` (hit ratio, misses, tokens re-cached, warm/cold) and a matching`prompt_cache` object for status line scripts
- Added `attach` ,`logs` ,`stop` ,`respawn` , and`rm` to`claude --help` ; the`--resume` message for a running background session now names the exact`claude attach <id>` command

**Changed**

- Changed `/radio` to be available on Bedrock, Vertex AI, Foundry, and Claude Platform on AWS, and when telemetry is disabled
- Changed Claude in Chrome so browser actions always go through Claude Code’s permission checks, including in sessions with telemetry disabled, which previously used the Chrome extension’s own prompts
- Changed `CLAUDE_CODE_SUBAGENT_MODEL` to set the default subagent model rather than override everything: an agent definition’s`model:` and an explicit per-spawn model now take precedence over it
- Changed the default commit trailer to `Co-Authored-By: Claude Code` when the active model isn’t a recognized Claude model (e.g. third-party models behind a custom`ANTHROPIC_BASE_URL` )
- Changed the default model for seat-based Enterprise subscriptions to Opus 5, matching other premium plans
- Changed `/effort` to save your default effort level per model, so each model keeps its own setting when you switch
- Changed analytics to no longer turn off before sign-in solely because managed settings force gateway login (or cannot be read); they stay off once signed in to the gateway or via `DISABLE_TELEMETRY`
- Changed the footer PR badge on Bedrock, Vertex, and Foundry, and when telemetry is off, to call the GitHub API directly (via `gh auth token` ,`GH_TOKEN` , or`GITHUB_TOKEN` ) instead of`gh pr view`
- Changed how Bash command output files are created and read back when commands run in the sandbox, so a sandboxed command cannot redirect or replace them
- Changed plugin/LSP install suggestions and the auto-mode default offer to wait until you’ve sent or cleared what you’re typing, so the Enter that sends your prompt can’t answer them
- Changed server-managed settings that terminate sandbox TLS, route sandbox traffic through your own proxy, inject credentials, or weaken sandbox isolation to require approval before they apply
- Changed `ANTHROPIC_CUSTOM_HEADERS` from managed or project settings to require approval when it sets a credential, org/tenant, routing, or API-behavior header (e.g.`Authorization` ,`Host` )
- Changed project-level `.claude/settings.json``env` to no longer set`CLAUDE_CONFIG_DIR` ,`CLAUDE_CODE_TMPDIR` , or`TMPDIR` /`TMP` /`TEMP` ; set them in your shell, user, or managed settings instead
- Removed syntax highlighting for six rarely used languages (1c, gml, isbl, mathematica, maxima, sqf); the binary is 2.5 MB smaller
- [VSCode] Changed the Remote Control banner to a footer pill (shown while Remote Control is on or has failed) that opens the session on claude.ai/code; turn it on or off with `/remote-control`

**Added**

- Added `--restricted` (or`CLAUDE_CODE_RESTRICTED=1` ): removes the built-in tools that run commands or code and`WebFetch` (unless named in`--tools` ), keeps file tools inside the working directory, refuses`bypassPermissions` , and ignores user, project and local settings files
- Added `experimental.cacheTtl` (`"5m"` or`"1h"` ) to agent frontmatter: a per-agent prompt cache TTL used when no subagent TTL setting is configured
- Added `claude self-hosted-runner --client-label <label>` (or`SELF_HOSTED_RUNNER_CLIENT_LABEL` ) to override the label the runner registers with (default: hostname)
- Added server-managed settings diagnostics: a startup warning when the settings fail to load, and a `/doctor` and`/status` line explaining a load failure or why they weren’t fetched (Bedrock/Vertex/third-party provider, custom`ANTHROPIC_BASE_URL` )
- Added a warning in `/web-setup` when the GitHub CLI token lacks the`workflow` scope, since pushes to very large repositories can be rejected without it
- Added `/usage-credits` for Enterprise organizations billed through AWS Marketplace, self-serve Enterprise, and Enterprise trials, so members can request a higher usage limit from their admin
- Added cross-session messaging (`SendMessage` /`ListAgents` ) between sessions on the same machine on Bedrock, Vertex, and Foundry, and when telemetry is disabled

**Changed**

- Changed shift+enter in the agent view dispatch input to insert a newline (matching the prompt); ctrl+enter now dispatches and attaches
- Changed `/loop` : self-paced dynamic mode and the no-prompt autonomous default are now always available, including on Bedrock/Vertex/Foundry
- Changed Anthropic telemetry export failures to log at debug level as `[Anthropic telemetry]` instead of`[3P telemetry] OTEL diag error` , so they are not mistaken for your OTel collector failing
- Changed cross-session messaging in Linux user namespaces: root-equivalent trust for unmapped owners is limited to canonical system directories
- Changed `SendMessage` from a subagent to another session: the result now notes that any reply is delivered to the parent session’s conversation, not to the subagent

**Added**

- Added the `SendFeedback` tool: when something goes wrong in a session, Claude can draft a feedback report for you to review and send from`/feedback` (turn off with the`feedbackDrafts` setting)
- Added `{id, text, cooldownSessions, priority}` entries,`tipsFile` , and`label` to`spinnerTipsOverride` , so organizations can rotate their own tips alongside the built-in ones
- Added a tip on Bash permission prompts pointing to auto mode, with a one-keystroke “Yes, and switch to auto mode” option
- Added `/claude-api cost-optimize` to profile an existing project’s Claude API spend and work through cost levers (caching, token hygiene, batch, effort, model choice) one measured change at a time

**Changed**

- Updated the `/claude-api` skill with Admin API coverage (organization members, invites, workspaces, API keys, rate limit reports, workload identity federation, CMEK)
- Changed Sonnet 5’s default auto-compact window to its full 1M context, so sessions on the 1M window now auto-compact at about 967K tokens instead of about 934K
- Changed cross-session peer messages to collapse by default to a one-line `Message from @<sender>: <first line>` preview; Ctrl+O expands the full body
- Changed terminal hyperlinks in rendered markdown: link targets that point at a network or automounter path, contain a control character, or lead with an invisible character now render as plain text
- Changed the prompt-footer PR badge to skip its GitHub re-check on terminal refocus when the last check is under a minute old
- Changed analytics to stay off from startup, not only after login, when managed settings force gateway login or a custom OAuth deployment is configured
- Changed Claude apps gateway sign-in requests to identify Claude Code (a `surface=claude_code` device-authorization parameter and a`claude-code/<version>` User-Agent)
- Changed organization sign-in enforcement to exit at start when the administrator’s managed settings cannot be read, even if host-supplied or per-user Windows registry settings exist

**Improved**

- Improved plugin marketplace hardening: names containing control or invisible characters are rejected, and marketplace-supplied text in `/plugin` and`claude plugin` output is escape-safe
- Improved Bedrock, Vertex, and Foundry sessions (and any with telemetry disabled): Claude is now told when a configured MCP server failed to connect, instead of concluding its tools don’t exist

**Added**

- Added a startup warning for Bash allow rules with a wildcard before the subcommand (e.g. `Bash(git * main)` ), since they also match options inserted before the subcommand
- Added an Auto mode tab to `/permissions` for viewing and editing auto mode classifier rules
- Added the turn’s completion time to the end-of-turn duration line, e.g. `✻ Sautéed for 23s · done 6:05 PM`

**Changed**

- Changed `/code-review` so Claude can also start it on its own on Bedrock, Vertex AI, and Foundry, through the Claude apps gateway, and when telemetry or non-essential traffic is disabled
- `/goal` : Changed idle sessions to start at most three check-ins on long-running background work per goal; your next message allows three more
- Changed `claude install` and`claude update` to defer a pending managed-settings consent prompt to the next interactive session instead of prompting mid-command
- Changed OpenTelemetry plugin events for plugins synced from claude.ai: `plugin_id_hash` now reflects the plugin’s real marketplace, and`enabled_via` is`admin-install` for admin-installed plugins

- Fixed a crash on startup on Linux distributions that ship glibc 2.44 (for example Arch Linux, CachyOS and Fedora Rawhide)

**Added**

- Added a Loops breakdown to `/usage` : per-loop run count, total tokens, tokens per run, and last run, so runaway or chatty`/loop` tasks are easy to spot
- Added `modelPicker` setting: curate the`/model` picker with an ordered, labeled list of models (any id spelling, including Vertex/Bedrock ids), appended to or replacing the built-in lineup
- Added `promptCacheTtl` and`subagentPromptCacheTtl` settings so API-key and cloud-provider users can keep a 1-hour prompt cache on the main conversation while subagents stay at 5 minutes
- Added `modelPricing` managed setting so an organization’s contracted per-model rates and discount multiplier are used for`/cost` , the status line, and telemetry cost figures instead of list price
- Added a keyless sign-in under `/login` → Anthropic Console: “Sign in with your Console account” (recommended) alongside creating an API key, so organizations that don’t allow API keys can sign in
- Added a `Skipped sources` line to`/status` that lists managed settings sources (for example`managed-settings.json` ) present but not applied because a higher-precedence managed source is active
- Added a `managed` marker in`/mcp` and`/plugins` on claude.ai connectors whose authentication is managed by your organization
- Added a tip pointing claude.ai users who haven’t connected GitHub for Claude Code on the web to `/web-setup`
- Added a `/status` line showing whether GitHub is connected for Claude Code on the web (Pro/Max), pointing to`/web-setup` when it isn’t
- Added the model (and effort level) each subagent ran on to `/tasks` and the agent detail dialogs

**Changed**

- Changed the sandboxed Bash tool prompt to no longer list allowed network hosts, so Claude attempts requests (and you can approve new hosts) instead of assuming unlisted hosts are blocked
- Updated the `/model` picker and the bundled`claude-api` skill to show Sonnet 5’s $2/$10 per Mtok pricing as its standard list price rather than a limited-time promo
- Changed computer use on macOS so clicking the desktop, Dock, or a Finder window requires granting Finder via the access dialog, like any other app
- Changed `/model` ,`/fast` , and`/effort` to also run immediately instead of queueing until the turn ends on Bedrock, Vertex, and Foundry and when telemetry is disabled
- Changed the cross-session messaging inbox socket to close connections that send no complete line within 30 seconds; scripts posting to it should connect once their data is ready

**Added**

- Added the one-time fullscreen renderer offer on Bedrock, Vertex, Foundry and other previously excluded setups; new installs there now start in fullscreen
- Added `/claude-api upgrade` to migrate Python projects from`anthropic` 0.x to 1.x, and updated the skill’s Python reference for 1.x (timeouts use`anthropic.Timeout` , not`httpx.Timeout` )

**Changed**

- Cost estimates (`/cost` , status line,`--max-budget-usd` ) now include the 1.1× US-only-inference premium for data-residency workspaces
- Cloud sessions: plugins synced from claude.ai now show as `name@synced` , work with`claude plugin enable/disable <name>@synced` , and never override a same-named plugin you installed
- Alpine/musl builds: native image paste, clipboard, and audio-capture add-ons now load (musl-built binaries instead of glibc ones refused by the runtime)
- The usage-limit message shown when your monthly spend limit is already used up now also says when your session or weekly limit resets
- Long file paths on tool-use rows now truncate in the middle to stay on one line
- Remote sessions keep sending keep-alives while a long `SessionStart` or`Setup` hook runs, so the container is not idle-reaped mid-hook
- `/goal` : repeat check-ins on long-running background work now back off (30 min, then 1 h, then every 2 h) instead of repeating every 30 minutes
- `/goal` : resuming a session from the`claude --resume` picker now restores its active goal
- `ListAgents` now tells a session its own name (the one peers use to message it), and`SendMessage` to your own name says so instead of “no agent named …”
- `ListAgents` and`/list-agents` now list your live teammates (previously only subagents and other sessions appeared, so a reachable teammate looked absent)
- `keybindingFlavor: "readline"` now also matches Bash for word keys: Alt+F and Ctrl/Option+→ stop at the end of the word, Alt+D deletes to it (Ctrl+Y pastes it back), and punctuation separates words
- Persistent retry mode (`CLAUDE_CODE_RETRY_WATCHDOG` ) now fails immediately on organization spend-limit and out-of-credits errors instead of waiting indefinitely for a reset
- Claude in Chrome: `/clear` now closes the session’s Chrome tab group, and empty groups are closed on`/resume` and when Claude Code exits
- Remote sessions: images uploaded from mobile now include their saved file path, so Claude can copy them into files it creates
- Claude Code on the web: requests from Bash and other tools to non-API anthropic.com hosts (e.g. www, docs) now go through the session’s network proxy, so your environment’s allowed domains apply
- Remote Control: clearer message and `claude doctor` wording when Remote Control isn’t enabled for your account
- Windows: cross-session messaging is now available, so Claude Code sessions across your machines can message each other with `SendMessage` and find each other with`ListAgents` , as on macOS and Linux
- [VSCode] “View usage” in the usage-limit banner now sits inline with the warning text instead of floating mid-banner

**Improved**

- Improved the reminder shown after compaction so a skill’s original arguments are not re-run as a new request

**Added**

- Added a `keybindingFlavor` setting: set it to`"readline"` to make Ctrl+W in the prompt delete back to the previous whitespace, as in Bash; the default (`"classic"` ) is unchanged
- Added `claude self-hosted-runner --defer-shutdown-max-min <minutes>` : on SIGTERM, keep serving attached sessions, park what is left after that many minutes, then exit
- Added `claude self-hosted-runner --proxy-authorization-command` /`--proxy-authorization-file` for egress proxies that require a freshly issued`Proxy-Authorization` header on every connection

**Changed**

- Plugin marketplaces: `headersHelper` on a url marketplace or a catalog entry runs a command that mints HTTP headers (e.g. a short-lived token) for catalog and same-origin archive fetches
- A catalog entry’s `headersHelper` runs only when you install or update that plugin, after its command is shown;`claude plugin install/update` ask`[y/N]` (or pass`-y` )
- Cross-session messaging: sending to a session on this machine that refuses inbound messages (e.g. `crossSessionInbound: "refuse"` ) now reports “refused” to the sender instead of a silent success
- Cross-session messaging: a session whose inbox drops your messages (rate limit or full queue) now tells your session, instead of the messages vanishing silently
- Updated the bundled `claude-api` skill for the Managed Agents Aug 19 release: web search/fetch domain settings and memory stores on self-hosted sandboxes
- Changed Ctrl+L and Cmd+K in fullscreen to always just repaint — the double-press `/clear` shortcut was removed, and 1-row nvim terminals no longer trigger automatic`/clear` loops
- Changed `claude mcp list` and`claude mcp get` to show disabled servers as`⊘ Disabled` instead of connecting to them for a health check
- MCP `headersHelper` in a project`.mcp.json` , and inline MCP servers in project or`--add-dir` agent files, now require that folder’s trust dialog to have been accepted (also under`claude -p` )
- MCP `headersHelper` from a project`.mcp.json` , plugin, or agent file runs without inherited credential env vars; user, managed and claude.ai-scope helpers now run from the Claude config dir

**Improved**

- Improved startup: bare `claude` starts sooner on macOS
- Improved Bash tool permission checking for zsh-specific syntax in shell conditionals
- Improved Remote Control connection resilience: brief HTTP 403 refusals from a network edge, VPN, or proxy are now tolerated for up to 3 minutes, with the refusing party named when a block persists
- Improved startup responsiveness: the automatic update check now runs about 10 seconds after launch instead of competing with startup for CPU

- Fixed prompt caching for sessions using an LLM gateway or custom base URL
- Added a built-in “Concise” output style: Claude leads with results and skips preamble and narration, while doing the work just as thoroughly. Select it under Output style in /config.

**Added**

- Added `ANTHROPIC_DEFAULT_MODEL` environment variable: sets the model new sessions start on, while a`/model` pick still overrides it and persists across restarts (unlike`ANTHROPIC_MODEL` )
- Added `notify_when_idle` to cross-session`SendMessage` : ask another Claude Code session on this machine to send one notice when it next goes idle — opt-in, one-shot, no polling (macOS and Linux)
- [VSCode] Added screen reader support for the transcript: live announcements for replies, permission requests, errors, and status changes, plus per-turn heading navigation

**Changed**

- Sandbox: on macOS, wildcard read-deny rules (e.g. `**/.env` ) now take precedence inside allowed read regions, cover matched directories’ contents, and can’t be bypassed by renaming the denied file
- Changed the `/model` picker to highlight only the newest model’s name, so the highlight marks the new release rather than an arbitrary subset of the list
- `/goal` : an idle session whose goal is parked behind long-running background work now checks in automatically after 30 minutes (then 1h, 2h) instead of waiting for you to return
- `/usage` now shows the usage-credits spend row for Team and Enterprise members, and shows a capped row at 0% before anything is spent
- SIGTERM in print/SDK mode no longer records an interrupted turn or synthetic tool denials before exiting; running commands are still terminated and the process still exits with code 143
- Pressing Enter on a slash-command typo or a command unavailable in this session now reports it instead of running the closest fuzzy match; prefixes and aliases still run
- Remote Control now marks a session offline within seconds when the CLI exits or its terminal closes
- `SendMessage` now refuses further messages to a session up front once a rapid burst would exceed what that session’s inbox accepts, instead of reporting them sent while they were dropped
- Aligned the session title chip on the prompt border with the footer’s right edge
- Right-aligned footer items (goal indicator, session state, background agent status) and truncated notices now share a consistent right margin with the rest of the prompt area

**Improved**

- Improved startup performance: the session counter is now written in the background
- Improved auto mode: `Monitor` allow rules are now set aside while auto mode is active, so Monitor commands are reviewed the same way Bash commands are
- Improved auto mode on Bedrock, Vertex AI, and Foundry, and when telemetry is disabled: the classifier now uses the same defaults as on the Claude API, including severity-scored classification
- Improved auto mode: the git status check can no longer be fooled by a repo’s `status.showUntrackedFiles=no` setting into reporting a clean tree

**Added**

- Added an optional `spellcheck` setting that underlines misspelled words in the prompt input as you type, using your installed`aspell` ,`hunspell` , or`ispell`

**Changed**

- Vim mode: NORMAL mode and cursor position are now preserved when toggling the detailed transcript (ctrl+o) or closing a panel
- Dialogs: arrow keys and Enter pressed in quick succession now select the option you navigated to instead of the previously highlighted one
- `SendMessage` now refuses messages too large for cross-session delivery up front instead of silently dropping them
- Remote Control: `claude rc` now applies the same enterprise-gateway availability check as interactive startup

**Improved**

- Improved memory and CPU usage while cloud sessions such as `/ultrareview` or`/autofix-pr` run in the background — their event streams are no longer re-scanned and re-rendered on every update
- Improved permission dialogs: display text and “don’t ask again” options now always match what a grant would cover, and “don’t ask again” is withheld when contents cannot be fully displayed
- Improved the embedded `grep` in native macOS/Linux builds: pathological patterns now fail fast instead of exhausting memory, and`-m N` with`-A/-C` prints correct context
- Improved the context-limit error to say when auto-compact is off and point to `/config` to re-enable it

**Added**

- Added the optional `CLAUDE_CODE_PROJECT_DIR_NAME` environment variable: hosts that give each session its own config directory can choose a short name for the per-project transcript directory
- Added the `selection:clear` keybinding action, so a key can be bound to clear an in-app text selection; also works in the agents view
- Added a GitLab merge request badge to the footer and statusline: repos with a GitLab remote and an authenticated glab CLI show MR !N with draft/pending/green states

**Changed**

- Claude Code now continues your session automatically when a claude.ai usage limit resets; turn it off in `/config` (“Continue automatically at usage limit”)
- Claude is now told to use your account email only to identify you, and not to send it to unrelated services unless you ask
- Security: remote file reads, session restore, CLAUDE.md includes, workflow scripts and file uploads now reject Windows NT-namespace (`\??\` ) paths, hardening the remaining pre-approval file accesses against the NTLM credential-leak vector
- Remote Control: signing this computer in to a different claude.ai account or organization now stops the running session within seconds and says why, instead of a misleading HTTP 404 hours later
- Remote Control sessions started from Claude Code Desktop or VS Code now keep phones and claude.ai/code updated on the session’s permission mode (and claude.ai/code on the model) as they change
- Remote Control: effort picks made on a phone or on claude.ai/code now apply to terminal- and Desktop/VS Code-hosted sessions, and the session publishes its effort level to connected clients
- `SendMessage` and`ListAgents` now say when your account’s session list was too long to check completely, instead of treating unseen sessions as absent
- Expired Anthropic profile credential now points you at `/login` when a claude.ai login would take precedence
- Reduced the context cost of loading the built-in `claude-api` skill from ~200k+ tokens to ~25k by loading reference docs on demand
- `/permissions` can now be opened while Claude is working — rule changes apply to the rest of the current turn
- `/add-dir <path>` can now be used while Claude is working;`/add-dir` ,`/autocompact` ,`/theme` ,`/help` ,`/config` and`/advisor` dialogs open mid-turn in the fullscreen TUI
- `/goal` now clears itself with a notice when a turn dies on an unrecoverable error (e.g. revoked auth, an exhausted credit balance, or a context overflow) instead of staying armed
- `/goal` : when background tasks keep a goal waiting for 30+ minutes, Claude now checks in on them instead of waiting indefinitely (set`CLAUDE_CODE_GOAL_CHECKIN_MINUTES=0` to opt out)
- `claude setup-token` now rejects unexpected extra arguments instead of silently ignoring them
- Changed Esc in fullscreen mode to no longer clear a mouse text selection: it interrupts or dismisses as usual and the selection stays highlighted
- Removed the redundant “Allowed by auto mode classifier” line that auto mode showed under every Agent tool call
- Removed the “Default teammate model” setting from `/config` ; agent-team teammates now use the leader’s model unless the spawn names one
- Dimmed the elapsed-time counter on the running tool header so it no longer competes with the bold counts
- Background task notifications delivered between turns are now sent to the model inside `<system-reminder>` tags, matching mid-turn delivery
- Mantle: skip the admin-pin availability probe at startup when a main-loop model is already picked
- Windows: startup no longer stalls on repeated rename retries when `~/.claude.json` is read-only

**Improved**

- Improved the transcript: your own prompts now render markdown (highlighted code blocks, inline code, lists) the same way replies do
- Improved the “API returned an empty or malformed response” error to say what came back (content type, body kind, size, request ID) and why the original streaming request failed
- Improved auto-generated session titles to read as short, specific names (e.g. “Login button bug”) rather than sentences restating your request (e.g. “Fix the login button on mobile”)

**Added**

- Added GitLab merge request URL support to the `--worktree` flag and the`claude agents` view (where MRs display as`!N` )
- Added an opt-in `forward_user_identity` apps gateway setting on Anthropic upstreams that sends the signed-in user’s identity as headers, so a proxy behind the gateway can attribute spend per user
- Added opt-in memory cgroup support for Bash tool commands on Linux (`CLAUDE_CODE_TOOL_MEMORY_LIMIT` ) so a runaway build can’t stall the session
- Added `CLAUDE_CODE_WEBFETCH_CACHE_TTL_MS` environment variable to configure the WebFetch session URL cache TTL (default unchanged: 15 minutes)

**Changed**

- Changed the GitHub app setup tip to no longer appear in repositories whose origin remote is on gitlab.com or bitbucket.org; the enterprise marketplace tip now covers non-GitHub internal git hosts
- Todo/task-tracking tools (TaskCreate/Get/Update/List, TodoWrite) are no longer available on Opus 4.8, Sonnet 5, Fable 5, Mythos 5, and newer models; set `CLAUDE_CODE_ENABLE_TODO_TOOLS=1` to bring them back
- Reverted the 2.1.232 Bash permission changes for Cygwin-style symlinks on Windows and for input redirections (`< file` ); a narrower version will return in a later release

**Added**

- Added `/config` rows for “Dialog expiry” and “Messages from your other sessions” (cross-session inbound accept/hold/refuse)
- Added secret redaction for GitLab token families (`glrt-` ,`gloas-` ,`glptt-` ,`glagent-` ,`glimt-` ,`glsoat-` ,`glcbt-` ,`glft-` ,`glffct-` ) and full redaction of routable`glpat-` /`gldt-` tokens; the`glab` CLI config store gets the same sandbox and credential-path protection as`gh`
- Added GitLab support to plugin marketplaces: bare `gitlab.com` repo URLs (including nested subgroups) now clone like`github.com` URLs, and clone auth-failure hints name your actual git host

**Changed**

- Subagent forking is now on by default: a `subagent_type: "fork"` subagent inherits the full conversation and prompt cache, and non-teammate agent spawns in interactive sessions now run in the background by default
- Type `@` in the prompt to mention another Claude session by name; Claude then uses`SendMessage` to reach that session directly
- `SendMessage` now delivers to a bare name that exactly matches one live session, instead of asking to confirm with a ref first
- Interactive sessions on one machine now keep unique names: starting or renaming a session to a name another live session already uses gives it a `name-word-word` variant and tells you
- Settings: `additionalMarketplaces` and`allowedMarketplaces` are now accepted as friendlier aliases for`extraKnownMarketplaces` and`strictKnownMarketplaces`
- Enterprise policy: a url-typed `blockedMarketplaces` entry for a bare repo URL keeps blocking that URL when the CLI classifies it as a git clone
- Gateway: the `desktop:` overlay now accepts every released Desktop setting (was 11 hand-listed keys), validated at boot against Desktop’s own schema; unknown or invalid keys fail boot
- Gateway: empty `managed.policies[].match.groups` /`admin.admin_groups` entries and malformed`email_domain` values (empty, or containing`@` , whitespace, or commas) now fail at boot instead of silently matching no one or granting admin access
- Fable 5 is offered as an advisor in `/advisor` again for organizations with Fable access, with usage-credits consent set up through`/model fable`
- Remote Control: resuming a conversation whose session was deleted from claude.ai or the app now starts a replacement instead of failing with a message about your login (regressed in v2.1.227)
- `/feedback` and`/bug` now open immediately when invoked while Claude is responding, instead of waiting for the turn to finish
- `/plugin install plugin@marketplace` now refreshes the marketplace first, so newly published plugins install without a manual marketplace update
- `/code-review` at high, xhigh, and max effort now runs in a background agent like the other levels
- Pasted and clipboard images are read without blocking the event loop
- Remote Control now keeps reconnecting for about 30 minutes after a network blip and no longer drops after a few blips spread across an hour
- Remote Control: resuming a conversation no longer silently takes Remote Control away from another Claude Code on the same machine that still has it; run `/remote-control` there to move it
- Updated agent panel: completed subagents hide immediately with a `/tasks` footer hint, and the ”↓ N more” overflow indicator moved left for visibility
- Remote Control: the terminal now says whether a session was taken over by another device, ended from another app, or deleted, and stops suggesting a reconnect that would undo it
- Bash input redirections (`< file` ) are now permission-checked like their argument spellings on all platforms
- Shortened the message shown when resuming a completed background agent
- Cowork sessions no longer inline external @-imports from user-scope memory files
- Hardened the auto-generated cross-session messaging socket directory on shared `/tmp` : a pre-planted symlink or another user’s directory is now refused instead of used
- Hardened the Linux filesystem sandbox against a protected-path bypass
- Changed `sandbox.ripgrep` to be honored only from user, managed, and`--settings` settings; project settings can no longer override the sandbox’s ripgrep binary
- Removed the startup tip suggesting you create custom subagents, and the matching nudge in the `/powerup` tour

**Improved**

- Improved fullscreen streaming: long sessions stay responsive because the whole conversation is no longer re-normalized on every update
- Improved the managed settings approval dialog: shows endpoint URLs, uses clearer wording for telemetry-only changes, skips routine OpenTelemetry options, and requires approval for server-managed sandbox binary overrides (`sandbox.bwrapPath` ,`sandbox.socatPath` ,`sandbox.ripgrep` )

- Fixed MCP OAuth sign-in failing with a redirect URI mismatch for servers that use a pre-registered OAuth client, such as Slack

**Added**

- Added server-supplied Claude Code hook support for self-hosted runner sessions, matching managed-environment behavior
- Added SSE keepalive pings to gateway streaming responses during long thinking pauses, preventing idle-timeout disconnects on Vertex and Bedrock upstreams
- Added plugin marketplace `command` sources: a local command (e.g. an IDE) prints the plugin directory, which is re-resolved each session and applied without a restart;`mode: "link"` uses it in place
- [VSCode] Added session groups in the sidebar — right-click to create, rename, or delete; Cmd/Ctrl- or Shift-click to move several sessions at once

**Changed**

- Documented `claude remote-control --continue` for resuming the most recent Remote Control session
- `ListAgents` now marks disconnected Remote Control sessions as`offline` and labels your cloud sessions as`cloud`
- Updated `/login` to repeat the`CLAUDE_CODE_OAUTH_TOKEN` override warning after a successful login
- Changed `/commit-push-pr` so git/gh commands with dangerous flags (`--force` ,`--amend` ,`--no-verify` , etc.) are no longer auto-approved
- Changed self-hosted runner Windows startup to require an explicit `--base-dir` ; there is no default checkout directory on Windows
- [VSCode] “Report a problem” and `/bug` now open the built-in feedback dialog instead of a retired survey link
- [VSCode] Made the `/btw` side-question panel resizable by dragging its boundary, in both side-docked and stacked layouts

**Improved**

- Improved workflow fan-outs to stagger same-prefix sibling agents so subsequent agents read the cached prompt prefix instead of re-paying it (`CLAUDE_CODE_WORKFLOW_PREFIX_STAGGER_MS=0` disables)
- Improved “prompt is too long” errors to explain why automatic compaction could not recover instead of only suggesting `/compact`
- Improved sandbox: IPv6 literals in network domain lists are now bracketed (`[::1]:443` ), and ambiguous spellings are enforced fail-closed and flagged by`/doctor`

**Changed**

- Hardened skills synced from claude.ai: they no longer shadow local commands or MCP prompts, their descriptions are sanitized and labeled, and on your machine their bodies don’t run `!` commands or expand`@` files
- Updated terminal title busy-spinner glyphs to reduce tab-bar jitter on some terminals
- Changed the Write tool so newer models can overwrite an existing file they haven’t read this session, matching the Edit tool’s rules; older models still require the read first
- Removed the outdated note about auto mode sessions costing slightly more from the first-use notice for Pro, Max, and Team plans

**Improved**

- Improved cross-session messages: the sender and body now display inline instead of a collapsed line, and messages to Remote Control sessions on other machines show your Remote Control session name as the sender
- Improved Vertex AI credential handling: expired or missing Google Cloud credentials now fail within seconds instead of retrying for minutes
- Improved compaction progress: the retry countdown and stall hint now appear during compaction instead of only a progress bar

- Fixed feature flags being evaluated without the user’s subscription tier when a session started with an expired login token, which could wrongly prompt Max plan users to enable usage credits for Fable
- Fixed every Bash command failing under `claude-code-action` with`allowed_non_write_users` on GitHub-hosted runners
- Fixed `/tui` bringing back a conversation that had been rewound to before its first message
- Improved slash-command menu: blue now marks only the selected row, matched characters are bolded instead of recolored, and emoji or accented names keep their glyphs
- Improved performance: fewer event-loop stalls on file-not-found suggestions and at-mention size checks

**Added**

- Added gateway spend-limit support to Claude Code’s usage warning; the limit-reached message now names the cap, its reset time, and the operator’s message (requires the gateway on 2.1.225)
- Added a workspace trust prompt to `claude agents` for untrusted directories, matching the behavior of`claude`

**Changed**

- SendMessage can now start a conversation with your Remote Control sessions on other machines by name (`ListAgents` shows them as`name [ref]` ), instead of only replying after they message you first
- SendMessage: a Remote Control recipient you already confirmed is never swapped for a same-named session on this machine when its own list couldn’t be checked

**Improved**

- Improved Remote Control: photos attached from the Claude app are now shown to Claude directly instead of being read from disk with a separate tool call

**Added**

- Added self-hosted environments: `claude self-hosted-runner` turns your own machines or containers into a place Claude Code web, mobile, and desktop sessions can run, on Team and Enterprise plans
- Added `archive` plugin source: install plugins from a zip over HTTPS without git or npm, with optional SHA-256 pinning
- Added a cancel-and-confirm step when removing an unavailable paste changes a command’s text
- Added `ANTHROPIC_BEDROCK_REGION_PREFIX` env var for Bedrock to prefer a specific cross-region inference profile over the`AWS_REGION` -derived one
- Added `crossSessionInbound` and`dialogExpiry` settings: cross-session messages sent to a session running with bypassed permissions are held for your approval, and messages to other sessions auto-deliver
- Added sandbox credential-masking options: `extract` and`onExtractNoMatch` for structured env values,`decode: "jwt"` with`maskClaims` for JWT-aware masking, and`awsPairs` /`sigv4` for AWS SigV4 re-signing; these need`network.tlsTerminate` and are honored only from user, managed, or`--settings` settings
- Added cross-session `SendMessage` : Claude Code sessions can now message each other, on any of your machines, with`ListAgents` to discover them (macOS and Linux)

**Changed**

- Removed the 200-subagent-per-session spawn cap; long-running sessions no longer refuse new agents (concurrency and depth limits still apply)
- Changed managed settings: the approval prompt no longer re-appears after re-login or org switching when the organization’s settings are unchanged
- Changed the feedback-survey transcript share: with your consent it now also uploads the last request’s model settings — the system prompt (which includes your `CLAUDE.md` instructions), tool definitions, and model parameters. Secrets are redacted as before, and these fields are dropped first if the share is too large
- Changed the Bash tool description to always note that command output is displayed to the model, not reliably to the user
- Changed recalled paste placeholder numbers to renumber when accepted into the input
- Changed Remote Control to archive the stale server session instead of leaving a dead one listed when a fresh session is minted after compaction or `/resume`

**Improved**

- Improved fullscreen mode to keep the full pre-compaction history in scrollback across repeated compactions, instead of only the most recent interval
- Improved Remote Control: attached web and mobile clients now see compaction progress and the post-compaction boundary instead of a silent pause; `/clear` resets now propagate to attached clients
- Improved Remote Control: connection failures now show a persistent failure indicator with details and a reconnect shortcut, instead of only an 8-second toast

**Added**

- Added owner wildcard entries (`"owner/*"` ) to the`strictKnownMarketplaces` and`blockedMarketplaces` managed settings for allowing or blocking all marketplace repos under a GitHub org
- Added a warning when workflow agents, forked skills, slash commands, or resumed background agents’ requested subagent model is restricted and the parent model runs instead
- Added a `/teleport` hint in cloud sessions showing how to continue locally with`claude --teleport <session id>`

**Changed**

- Changed `CLAUDE_CODE_DISABLE_1M_CONTEXT` to hold every Claude model with a native 1M window to 200K via auto-compaction, not just a fixed list; a startup warning now appears when auto-compaction isn’t holding the session to 200K
- Changed auto-compact to keep sessions on unrecognized model IDs within the assumed context window instead of letting them grow past it; set `CLAUDE_CODE_DISABLE_UNKNOWN_MODEL_WINDOW_ENFORCEMENT=1` to restore the previous behavior
- Changed `/review` to be an alias of`/code-review` , which reviews the current diff or a PR (`/code-review <level> <pr#>` ); use`/code-review ultra` for a deep cloud review
- Changed `/code-review` with no effort level to reuse the level you typed last; type a level like`/code-review high` to change it

**Changed**

- Changed Remote Control auto-start so repo-local settings (`.claude/settings.json` or`.claude/settings.local.json` ) can no longer turn it on (they can still turn it off); enable it at user scope via`/config`
- Removed ultraplan feature

**Improved**

- Improved auto mode safety: messages sent to other agent sessions via `SendMessage` are now evaluated by the permission classifier before dispatch
- Improved the refusal when Claude tries to invoke a skill with `disable-model-invocation` : Claude is now told to ask you to run the skill instead of replicating its workflow
- Improved the `/diff` view, the Remote Control workspace diff, and file-edit diffs in Claude Code on the web sessions to use raw git blob content, ignoring workspace-configured diff drivers and textconv

**Added**

- [VSCode] Added Focus view: a chat-menu toggle that hides tool activity behind an expandable per-turn summary with a live running-tool indicator, toggled with `Ctrl+Alt+F` or the “Claude Code: Toggle Focus view” command
- Added `mode: "mask"` for sandbox credential files on Linux and WSL — sandboxed commands read a sentinel copy (the whole file, or just the spans captured by an`extract` regex) while the sandbox proxy substitutes the real value on egress; on macOS file masking falls back to`deny`
- Added warnings to `claude plugin validate` when a marketplace or plugin name would be rejected by Claude Desktop’s managed marketplace sync
- Added a `prompt-audit` subcommand to the`claude-api` skill for auditing prompts and tool descriptions for patterns written for older models

**Changed**

- Reduced prompt-cache costs for auto-mode permission checks by reusing the cached conversation prefix across decisions
- Changed background sessions to commit and push to preserve work, open a draft PR only when the task calls for one, follow your CLAUDE.md git instructions, and always end by reporting where the work lives
- Changed `/plugin install` to refresh a stale marketplace catalog and retry before reporting a plugin not found
- Changed plugins installed from `/plugin` to activate immediately when safe, instead of always requiring`/reload-plugins`
- Changed plugins to accept `"."` as a`skills` path, and the root-level`SKILL.md` validation error now suggests using the plugin root
- Changed `/status` to show the session kind:`interactive` , or a background job that is`attached` or`unattended`
- Changed emoji autocomplete to accept common alternate shortcodes like `:thumbsup:` ,`:thumbsdown:` , and`:love:`
- Changed sessions forked with `/fork` to create a new worktree of their own instead of working in the original session’s checkout
- Changed Claude in Chrome to close the browser tabs it opens once it no longer needs them
- Changed fast mode to report on the stream when usage credits run out mid-session, instead of failing silently
- Changed Monitor: a watch that exits without producing any output now says so instead of reporting “stream ended”
- Changed the Gateway `model` field validation: non-string values are rejected with a 400 instead of being forwarded
- Removed the repeated “Permission mode changed while the auto-mode classifier call was queued” notice from approval prompts

**Added**

- Added Claude Opus 5 (`claude-opus-5` ), now the default Opus model — 1M context, fast mode at $10/$50 per Mtok
- Added `sandbox.network.strictAllowlist` setting to deny non-allowlisted hosts for sandboxed commands without prompting
- Added `DirectoryAdded` hook that fires after`/add-dir` or the SDK`register_repo_root` control request registers a new working directory mid-session
- Added `mcp_server_errors` to the headless stream-json init event, listing`--mcp-config` entries skipped by config validation; terminal runs print a startup warning
- Added the `workflowSizeGuideline` settings key so the advisory Dynamic workflow size guideline can be set from any settings file; the`/config` row is hidden while one does
- Added nested subagent forwarding in stream-json: subagents spawned at depth-2+ now appear when `--forward-subagent-text` is set, keyed by their spawning Agent`tool_use` id
- Added HTTP status and error text to `claude mcp list` and`/mcp` when a server fails to connect, and a warning for MCP config values with hidden leading or trailing whitespace
- Added the current default workflow size to the running-workflow status line, with a pointer to `/config` for changing it

**Changed**

- Changed dynamic workflows to default to a medium size guideline (aim for fewer than 15 agents); pick another size or unrestricted with Dynamic workflow size in `/config`
- Changed managed MCP allowlist/denylist `${VAR}` entries to resolve from the startup environment and managed-settings env instead of settings-file env
- Changed the `/model` picker to highlight only the newest model’s name, so the highlight marks the new release rather than an arbitrary subset of the list
- Removed Opus 4.7 from fast mode; `/fast` now applies to Opus 5 and Opus 4.8
- Updated the claude-api skill to default to Claude Opus 5, with a migration path from Opus 4.8
- Subagents can now spawn nested subagents up to depth 3 by default (was 1); set CLAUDE_CODE_MAX_SUBAGENT_SPAWN_DEPTH=1 to disable nesting

**Improved**

- Improved the “Remote Control is only available via api.anthropic.com” error to name the specific setting that caused it
- Improved `claude --teleport` to show which repo your current checkout points at when it doesn’t match the session’s repo

**Added**

- Added screen-reader announcements of deleted text for word and line deletions (`Option+Delete` ,`Ctrl+W` ,`Cmd+Backspace` ,`Ctrl+U` ,`Ctrl+K` ) in`--ax-screen-reader` mode
- Added an announcement when fast mode changes as a result of switching models via `/config model=<x>` or Remote Control
- Added `yes` /`no` /`on` /`off` /`1` /`0` (case-insensitive) as accepted values for skill and plugin frontmatter booleans, alongside`true` /`false`

**Changed**

- Changed `/code-review` to run as a background subagent, so review work no longer fills your conversation and keeps stacked slash commands as its review target
- Changed `/deep-research` to start only when invoked manually; Claude no longer launches it on its own
- Changed plan mode with auto to no longer prompt for Bash commands the static analyzer can’t prove read-only; the auto-mode classifier judges them instead
- Changed server-managed settings so benign feature and cost toggles no longer trigger the settings-approval prompt
- Changed agent markdown files to reject agent names containing `:` , which is reserved for plugin namespacing
- Changed skills with `context: fork` to run in the background by default; opt out per skill with`background: false`

**Improved**

- Improved `/ultrareview` error feedback so Claude can correct an invalid argument instead of retrying it unchanged
- Improved auto mode: the dangerous-rm, background-`&` , and suspicious-Windows-path checks no longer open permission dialogs; the auto-mode classifier adjudicates them instead
- Improved sandbox command restrictions for IDE interactions
- Improved trust dialogs to name the repository root the grant covers

**Added**

- Added emoji shortcode autocomplete in the prompt input: type `:heart:` to insert ❤️, or`:hea` for suggestions — disable with the`emojiCompletionEnabled` setting
- Added warnings when transcript writes are failing (e.g. disk full) or when session saving is off due to an inherited environment variable, instead of losing transcripts silently
- Added a cap on concurrently-running subagents (default 20, override with `CLAUDE_CODE_MAX_CONCURRENT_SUBAGENTS` ) so one message can’t fan out unbounded background agents

**Changed**

- Changed the login-expiry warning to appear 3 days before expiry instead of 5
- Capped the frontend-design plugin suggestion tip at 3 lifetime impressions instead of repeating indefinitely
- Changed subagents to no longer spawn nested subagents by default; set `CLAUDE_CODE_MAX_SUBAGENT_SPAWN_DEPTH` to allow deeper nesting

**Improved**

- Improved footer PR badge links to be clickable hyperlinks even when terminal support can’t be detected (e.g. over ssh/tmux); set `FORCE_HYPERLINK=0` to opt out

**Added**

- Added `sandbox.filesystem.disabled` setting to skip filesystem isolation while keeping network egress control

**Changed**

- `/context` now shows an explicit warning when the conversation exceeds the context window, and a failed`/compact` displays as an error
- `/rewind` no longer restores or deletes files through symlinks or hard links at tracked paths and reports how many paths it skipped
- Background sessions: `/mcp` and`/install-github-app` now park a “needs input” request in the agent view when no client is attached
- Updated the bundled dataviz skill: reordered the default chart palette and fixed guidance that suggested direct labels for four-series charts

- Claude no longer runs the `/verify` and`/code-review` skills on its own; invoke them with`/verify` or`/code-review` when you want them

**Added**

- Added the EndConversation tool: Claude can end sessions with highly abusive users or jailbreak attempts, as on claude.ai since 2025 — see https://www.anthropic.com/research/end-subset-conversations
- Added a periodic progress heartbeat for long-running tool calls that previously went silent
- Added an ISO `modified` timestamp to memory file frontmatter
- Added `message.uuid` ,`client_request_id` , and`tool_source` attributes to OpenTelemetry log events for message-level correlation and tool provenance
- Added `CLAUDE_CODE_OTEL_CONTENT_MAX_LENGTH` to configure the 60 KB truncation limit on OpenTelemetry content attributes
- Added reasoning effort to the `subagentStatusLine` payload, so custom agent rows can render model and effort
- Added permission prompts for `docker` commands (including the Podman`docker` shim) carrying daemon-redirect flags (`--url` ,`--connection` ,`--identity` , and Podman’s remote mode) that previously ran without one

**Changed**

- Changed single-segment `dir/**` hook`if:` conditions to match only`<cwd>/dir` ; write`**/dir/**` for any-depth matching.`deny` /`ask` permission rules keep their any-depth match.
- Changed `file` commands using`-m` /`--magic-file` or`-f` /`--files-from` to require permission instead of being auto-allowed as read-only
- Changed keep-alive connection pooling to disable after a stale-connection error, so retries open a fresh socket
- Changed SessionStart hooks to report source `"fork"` when a session begins as a fork instead of`"resume"`

**Improved**

- Improved the `claude rc` workspace-trust error in the home directory to say trust there is never saved and to suggest running from a project directory

**Added**

- Added `claude auto-mode reset` to restore the default auto-mode configuration, with a confirmation prompt (pass`--yes` to skip)
- Added a session-wide limit on WebSearch tool calls (default 200, tunable via `CLAUDE_CODE_MAX_WEB_SEARCHES_PER_SESSION` ) to stop runaway search loops
- Added a per-session cap on subagent spawns (default 200, override with `CLAUDE_CODE_MAX_SUBAGENTS_PER_SESSION` ) to stop runaway delegation loops;`/clear` resets the budget

**Changed**

- `/fork` now copies your conversation into a new background session (its own row in`claude agents` ) while you keep working; the in-session subagent it used to launch is now`/subtask`
- MCP tool calls running longer than 2 minutes now move to the background automatically so the session stays usable; configure the threshold or disable with `CLAUDE_CODE_MCP_AUTO_BACKGROUND_MS`
- Typing `/resume` in the agent view now opens a picker of past sessions — including sessions deleted from the list — and resumes your pick as a background session
- Reduced token usage in inter-agent messaging: `SendMessage` bodies are no longer duplicated into replayed history and tool results
- Changed `/fork` to name the copy after your prompt when the session has no title, so the row is recognizable in the agent view
- Changed bare `/btw` to reopen the side-question panel on your most recent exchange so you can browse earlier answers
- Changed the `←` footer hint to pulse`N done` for a moment when a background agent finishes while nothing needs your input
- Deprecated the Task tool’s `mode` parameter (now ignored); subagents inherit the parent session’s permission mode by default
- Changed Enterprise `forceLoginMethod` to be enforced for VS Code extension, SDK,`setup-token` , and`install-github-app` logins, not just the terminal
- Changed session transcripts to record the reasoning effort level on each assistant message
- Changed headless/SDK sessions to apply a `set_model` control request mid-turn; the next model round-trip uses the new model instead of waiting for the next turn
- Changed agent view / `claude agents --json` : sessions waiting on a sandbox, MCP-input, or managed-settings prompt now show as “Needs input” instead of “Working”
- Updated the auth status panel title from “Cloud authentication” to “Authentication”
- Corrected an earlier release note (2.1.200): tmux through the 3.6 series lacks synchronized output; newer tmux with support is detected automatically

**Improved**

- Improved web search and web fetch reliability by retrying 529 errors and rate-limited requests with bounded backoff
- Improved prompt caching: the mid-conversation system block now works behind LLM gateways and custom base URLs (Bedrock, Vertex, 1P)
- Improved background agent attach: cold-attaching now instantly shows the formatted transcript while the session boots, instead of a blank wait

**Added**

- Added `--forward-subagent-text` flag and`CLAUDE_CODE_FORWARD_SUBAGENT_TEXT` environment variable to include subagent text and thinking in stream-json output

**Changed**

- Hardened synced skill/plugin directory naming on Windows and kept CCR web fetch/search proxies working after `/clear`
- Updated integer environment variables (timeouts, token budgets, retry counts) to accept scientific notation and digit-separator spellings like `1e6` and`64_000`
- Updated documentation links to the current docs sites
- Changed “always allow” permission rules to save at the repository root, so approvals granted in a git worktree persist across sessions and worktrees
- Changed `/usage-credits` to ask for confirmation before sending a request to organization admins
- Changed Vim mode `s` and`S` (substitute char/line) to work in NORMAL mode, matching vim behavior
- [VSCode] Updated the Remote Control banner to describe what it does
- Claude in Chrome: hardened file-upload path validation
- Claude in Chrome: `save_to_disk` on screenshot actions now writes the image to disk and returns the path; previously it did nothing

**Improved**

- Improved terminal layout and rendering performance
- Improved background agent result reporting — Claude now reports the status of still-running agents and waits for the real completion instead of fabricating results
- Improved the memory index over-limit warning to measure only loaded content, excluding frontmatter and HTML comments

**Added**

- Added a live elapsed-time counter to the collapsed tool summary line so long-running tool calls visibly tick instead of looking stuck
- Added a startup warning for `Write(path)` ,`NotebookEdit(path)` , and`Glob(path)` permission rules — use`Edit(path)` or`Read(path)` instead

**Changed**

- Hardened the Agent tool against indirect prompt injection via content a subagent read
- Memory writes that leave a MEMORY.md index over its read limit now produce an explicit error instead of silent truncation
- Screen reader mode now announces permission mode changes aloud when cycling modes with Shift+Tab
- The agents footer hint now shows how many background agents are waiting on your input, with a brief color emphasis when the count changes
- Agent view: the session you pressed ← from stays visibly marked even after mouse hover or arrow keys move the selection
- Fable temporarily shows as unavailable in the advisor picker while a server-side issue causing Fable advisor failures is fixed

**Improved**

- Improved the Bash/PowerShell tool message when a command hits its timeout and is auto-backgrounded, so the model can distinguish a hang from an explicit background request
- Improved auto mode: the permission classifier now defaults to Sonnet 5 for external sessions, validated on the session’s first request and pinned for the session
- Improved the bundled dataviz skill’s chart color validation with perceptual OKLab color difference and recalibrated color-blindness thresholds

- Fixed /model and other dialogs being blocked in `claude agents` background sessions (reverts an overly broad guard)

**Added**

- Added screen reader mode: opt-in plain-text rendering for screen reader users. Run `claude --ax-screen-reader` , set CLAUDE_AX_SCREEN_READER=1, or add “axScreenReader”: true to settings.
- Added `vimInsertModeRemaps` setting: map two-key insert-mode sequences like`jj` to Escape in vim mode
- Added `CLAUDE_CODE_PROCESS_WRAPPER` : agent view and the background service now honor a corporate launcher by running every Claude Code self-spawn through a required wrapper executable
- Added mouse-click support for multi-select menus and “Other” input rows in fullscreen mode

**Changed**

- Changed the Fable 5 usage-credits consent prompt to start with the decline option focused
- Reduced per-tool-call CPU overhead in print/SDK sessions with many MCP tools by caching tool-pool assembly (up to 7x faster tool rounds at high tool counts)
- Reduced memory usage by bounding the file edit read cache to 16 MB instead of pinning up to 1,000 full files
- Reduced session transcript size (up to 79x in edit-heavy sessions) and bounded checkpoint disk usage by pruning superseded file-history backups
- Reduced memory usage when resuming sessions with background agents or forks spawned from large conversations
- Completed background agents now stay listed in `/tasks` until cleanup instead of vanishing the moment they finish
- Attaching to a stopped background agent now shows its transcript immediately while the session warms up, instead of a blank “Session is starting” screen
- Background sessions: an older daemon no longer silently restarts workers spawned by a newer version onto the older binary
- Agent view: Ctrl+X now deletes renamed-branch worktrees, never destroys unpushed commits, keeps the session row when a worktree is kept, and reused worktree names reset to the current base
- Catastrophic removals (e.g. `rm -rf ~` ) in commands containing`$(…)` /backticks/`<(…)` now prompt in`--dangerously-skip-permissions` and auto mode, matching the plain form
- `/install-github-app` and the`/mcp` settings menu no longer open in background sessions
- MCP servers configured with an empty URL now show as “not configured” in `/mcp` instead of a config error
- `/usage` now shows your last-known usage bars with an “as of” note when the usage endpoint is rate-limited, instead of an error screen

**Improved**

- Improved input responsiveness while agent task lists update — task updates no longer re-render the entire UI

**Changed**

- Auto mode is now available without `CLAUDE_CODE_ENABLE_AUTO_MODE` opt-in on Bedrock, Vertex AI, and Foundry; disable via`disableAutoMode` in settings
- Changed Bedrock, Vertex, and Claude Platform on AWS to default to Claude Opus 4.8
- Changed auto mode to no longer read `autoMode` from`.claude/settings.local.json` (repo-resident); use`~/.claude/settings.json` instead
- Plugin hooks/monitors/MCP headersHelper: `${user_config.*}` in shell-form commands is now rejected (shell-injection fix). Hooks: use exec form (`args` array) or`$CLAUDE_PLUGIN_OPTION_<KEY>` ; monitors and headersHelper: read the value inside the script (config file or the server’s`env` block).
- Plugin option values (`pluginConfigs` ) are no longer read from project-level`.claude/settings.json` ; only user,`--settings` , and managed settings are honored

**Improved**

- Improved agent view: pasting the same text again now expands the collapsed `[Pasted text #N]` placeholder instead of adding a second one
- Improved agent view: blocked session peeks now lead with the question and show a worded staleness clock (`waiting 3m` ) instead of the same timestamp twice

**Added**

- Added directory path suggestions to `/cd` , matching`/add-dir` behavior
- Added a `/doctor` check that proposes trimming checked-in`CLAUDE.md` files by cutting content Claude could derive from the codebase

**Changed**

- `/commit-push-pr` now auto-allows`git push` to the repo’s configured push remote (`remote.pushDefault` , or the sole remote when only one is configured) in addition to`origin`
- Gateway: `/login` now supports Anthropic-operated public gateway endpoints
- `EnterWorktree` now asks for confirmation before entering a git worktree outside the project’s`.claude/worktrees/` directory
- Background agents now upgrade to a new version in the background right after a Claude Code update, instead of paying a slow stale-session upgrade when you attach
- Changed agents view: Ctrl+X now permanently removes a completed session, and sessions no longer render twice; deleted background jobs stay deleted

**Improved**

- Improved `/code-review` findings quality on claude-opus-4-8 across all effort levels
- Improved agents view: status column now uses full terminal width instead of truncating at 64 characters

**Added**

- Added an auto mode rule that blocks tampering with session transcript files

**Changed**

- Auto-update binary downloads now stream to disk instead of buffering in memory, cutting the updater’s peak memory usage by roughly 400 MB
- Background task notifications now explicitly state that no human input has occurred, preventing fabricated in-transcript approvals from being acted on
- `/doctor` is now a full setup checkup that can diagnose and fix issues;`/checkup` is its alias
- Reserved the “Claude Browser” MCP server name (alongside “Claude Preview”) ahead of the Claude Desktop pane rename; user-configured MCP servers can no longer register under either name

**Improved**

- Improved auto mode to ask before running `rm -rf` on a variable it can’t resolve from context
- Improved agent view: sessions that edit, merge, comment on, or push to an existing PR now link it in `claude agents`
- Improved agent view: rows now show a colored state word and a classifier-written headline instead of raw tool call text, and the peek opens with full status including the exact ask for blocked sessions

- Fixed hook events not streaming during SessionStart hooks in headless sessions, which could cause remote workers to be idle-reaped mid-hook

**Added**

- Added a warning when your login is about to expire, so you can re-authenticate before background sessions are interrupted
- Added a grey ⏸ badge to the footer when in manual permission mode, making the active mode always visible
- Added the session’s additional working directories to MCP `roots/list` , with`notifications/roots/list_changed` sent when the set changes
- [VSCode] Added a Settings toggle for “Enable Remote Control for all sessions”

**Changed**

- Reduced binary size by ~7 MB and startup memory by ~7 MB by loading a large bundled dependency lazily instead of inlining it
- Changed left arrow to no longer close the background tasks, diff, and workflow detail views — press Esc instead
- Changed the empty `claude agents` view to always show the organized sections (Needs input / Working / Completed) with descriptions
- Removed the startup “claude command missing or broken” warnings — they now appear in `/doctor` and`/status` instead
- Removed a redundant navigation hint from the `claude agents` footer

**Improved**

- Improved responsiveness while long responses stream: live-preview updates no longer re-render the whole screen
- Improved subagent behavior: agents are now less likely to re-delegate their entire task to another subagent

**Added**

- Added a “Dynamic workflow size” setting in `/config` for controlling how large Claude generally makes dynamic workflows (small/medium/large agent counts) — an advisory guideline, not an enforced cap
- Added `workflow.run_id` and`workflow.name` OpenTelemetry attributes to telemetry emitted by workflow-spawned agents, so a workflow run’s activity can be reconstructed from OTel data

**Changed**

- Changed `/review <pr>` back to a fast single-pass review; use`/code-review <level> <pr#>` for the multi-agent review at a chosen effort level

**Improved**

- Improved `/workflows` agent list layout: wider titles, a dedicated time column, shorter model names, and no per-row tool-call counts
- Improved MCP error messages: clearer error when a server config has `url` but no`type` , suggesting`"type": "http"` instead of the misleading “command: expected string”

- Claude Sonnet 5 sessions no longer use the mid-conversation system role for harness reminders

**Changed**

- Changed `AskUserQuestion` dialogs to no longer auto-continue by default; opt into an idle timeout via`/config`
- Changed the “default” permission mode to “Manual” across the CLI, `--help` , VS Code, and JetBrains;`--permission-mode manual` and`"defaultMode": "manual"` are accepted alongside`default`

**Improved**

- Improved screen-reader output: decorative glyphs are now hidden, transcript symbols read as short labels, and nested tables read as `Header: value.` lines
- Improved the install script to explain when installation is killed by the system running out of memory

**Changed**

- Stacked slash-skill invocations like `/skill-a /skill-b do XYZ` now load all leading skills (up to 5), not just the first
- Transient server rate-limit errors (429s unrelated to your usage limit) are now retried automatically with backoff for subscribers instead of failing the turn
- `CLAUDE_CODE_RETRY_WATCHDOG` now raises the default retry count for non-capacity transient errors to 300 and lifts the cap of 15 on`CLAUDE_CODE_MAX_RETRIES`
- `claude agents` session rows now show pull-request links as bare`#N` without the redundant “PR” label

**Added**

- Added background agent notifications in `claude agents` — sessions that need input or finish now fire the`Notification` hook (`agent_needs_input` /`agent_completed` )
- Added `/dataviz` skill for chart and dashboard design guidance with a runnable color-palette validator
- Gateway: added Claude Platform on AWS (anthropicAws) as an upstream provider; model-not-found responses now advance the failover chain

**Changed**

- Subagents now run in the background by default, so Claude keeps working while they run and is notified when they finish (previously a gradual rollout)
- Claude in Chrome is now generally available
- Background agents launched from `claude agents` now commit, push, and open a draft PR when they finish code work in a worktree, instead of stopping to ask
- The built-in Explore agent now inherits the main session’s model (capped at opus) instead of running on haiku
- Subagents and context compaction now inherit the session’s extended thinking configuration, improving output quality on delegated tasks
- Keyboard shortcut hints now show opt/cmd instead of alt/super when connected from a Mac over SSH
- `/login` now opens the sign-in dialog from the`claude agents` view instead of saying it isn’t available
- Subagents now treat messages from the agent that launched them as normal task direction; an agent’s message is still never treated as the user’s approval
- Removed the `/agents` wizard; ask Claude to create or manage subagents, or edit`.claude/agents/` directly

**Improved**

- Improved focus mode: subagents launched in a turn now appear in its activity summary, and completed background notifications fold into a single count
- Improved syntax highlighting accuracy in code blocks, diffs, and file previews by upgrading to highlight.js 11
- Improved API retry UX: the error reason is now shown after the second attempt, and a status page link replaces the spinner tip when the API is overloaded

- Introducing Claude Sonnet 5: now the default model in Claude Code, with a native 1M-token context window and promotional pricing of $2/$10 per Mtok through August 31. Update to version 2.1.197 for access. https://www.anthropic.com/news/claude-sonnet-5

**Added**

- Added support for organization default models — admins set it in the org console; it shows as “Org default” (or “Role default”) in `/model` when you haven’t picked one yourself
- Added readable default names for sessions at start, making them easier to identify and message
- Added clickable file attachments in chat — Cmd/Ctrl-click reveals the file in Finder/Explorer

**Changed**

- Security: `claude mcp list` /`get` no longer spawn`.mcp.json` servers that a repo self-approved via a committed`.claude/settings.json` ; untrusted workspaces show`⏸ Pending approval`
- Reduced per-frame rendering work in the terminal UI by skipping no-op subtree walks during streaming
- The streaming idle watchdog is now on by default for all providers — it aborts and retries when a response stream produces no events for 5 minutes. Set `CLAUDE_ENABLE_STREAM_WATCHDOG=0` to disable.
- Remote Control is now disabled when `ANTHROPIC_BASE_URL` points at a non-Anthropic host, matching the existing behavior under`CLAUDE_CODE_USE_BEDROCK` /`_VERTEX` /`_FOUNDRY`
- Changed opening the agents view from a foreground session to require a single `←` press instead of two, matching the behavior in background sessions

**Improved**

- Improved background session reliability: long-running commands and workflows now survive the session’s process being stopped, restarted, or updated — including on Windows, where background shells are handed off instead of being killed
- Improved background agents: workers killed by a daemon restart are now automatically resumed from where they left off the next time the agents view opens
- Improved `/code-review` workflow: merged five cleanup finders into one, cutting token usage by roughly 25%

**Added**

- Added `CLAUDE_CODE_DISABLE_MOUSE_CLICKS` to disable mouse click/drag/hover in fullscreen mode while keeping wheel scroll

**Improved**

- Improved voice mode on Linux: now distinguishes “no microphone” from “SoX not installed” when SoX is present but no audio capture device exists
- Improved `claude agents` completed list to fill available vertical space; on short terminals the header compacts so live sessions stay visible
- Improved Remote session startup with a provisioning checklist while the container starts

**Added**

- Added `autoMode.classifyAllShell` setting to route all Bash/PowerShell commands through the auto-mode classifier instead of only arbitrary-code-execution patterns
- Added auto-mode denial reasons to the transcript, the denial toast, and `/permissions` recent denials
- Added `claude_code.assistant_response` OpenTelemetry log event containing the model’s response text. Redacted unless`OTEL_LOG_ASSISTANT_RESPONSES=1` ; when that var is unset it follows`OTEL_LOG_USER_PROMPTS` , so deployments that already log prompt content will start receiving response content on upgrade — set`OTEL_LOG_ASSISTANT_RESPONSES=0` to keep prompts-only.
- Added live file path autocomplete to bash mode (`!` )
- Added a startup notice when MCP servers need authentication, pointing at `/mcp`
- Added automatic memory-pressure reaping for idle background shell commands (disable with `CLAUDE_CODE_DISABLE_BG_SHELL_PRESSURE_REAP=1` )

**Improved**

- Improved background agents: the launch result no longer instructs Claude to “end your response” — it keeps working on other tasks while the agent runs
- Improved MCP `headersHelper` auth: the helper now re-runs and reconnects automatically when a tool call returns 401/403
- Improved plugin auto-rename: marketplace `renames` maps are now followed automatically, updating your settings to the new name
- Improved `/add-dir` message when the directory is already a working directory

**Added**

- Added `/rewind` support for resuming a conversation from before`/clear` was run

**Changed**

- Reduced CPU usage during streaming responses by ~37% by coalescing text updates to 100ms
- Reduced long-session memory growth from terminal output cache

**Added**

- Added `sandbox.credentials` setting to block sandboxed commands from reading credential files and secret environment variables
- Added org-configured model restrictions to the model picker, `--model` ,`/model` , and`ANTHROPIC_MODEL` , with a “restricted by your organization’s settings” message when a restricted model is selected
- Added mouse click support to select menus (permission prompts, `/model` ,`/config` , etc.) in fullscreen mode

**Improved**

- Improved `/install-github-app` : GitHub Actions workflow setup is now optional — you can install just the GitHub App and skip the workflow/secret steps
- Improved `/btw` with ←/→ arrow navigation to step through earlier answers
- Improved `/plugin` to surface plugins you haven’t used recently so you can clean them up

**Added**

- Added `claude mcp login <name>` and`claude mcp logout <name>` to authenticate MCP servers from the CLI without opening the interactive`/mcp` menu, with`--no-browser` stdin redirect support for completing over SSH
- Added status filtering (press `f` ) to the`/workflows` agent detail view
- Added a “Skills” section to the `/plugin` Installed tab
- Added `teammateMode: "iterm2"` setting with a warning when auto mode cannot find the`it2` CLI
- Added “Claude Platform on AWS - refresh credentials” option to `/login` when`awsAuthRefresh` is configured

**Changed**

- `!` bash commands now trigger Claude to respond to the output automatically; set`"respondToBashCommands": false` in settings.json to keep the previous context-only behavior
- Changed `CLAUDE_CODE_MAX_RETRIES` to cap at 15; for unattended sessions, use`CLAUDE_CODE_RETRY_WATCHDOG` instead
- Changed background subagents to surface permission prompts in the main session instead of auto-denying; the dialog shows which agent is asking, and Esc denies just that tool
- Changed `/review <pr>` to use the same review engine as`/code-review medium`

**Improved**

- Improved `claude mcp get` and`claude mcp remove` to suggest the closest configured server name on a typo and truncate long server lists
- Improved memory: the agent is now reminded to compact its `MEMORY.md` index when nearing the size limit
- Improved skill frontmatter: `display-name` ,`default-enabled` ,`fallback` , and`metadata.*` keys now accept kebab-case, snake_case, and camelCase
- Improved malformed `SKILL.md` YAML frontmatter handling: loads the skill body with empty metadata instead of failing silently

- The stream-stall hint now reads “Waiting for API response · will retry in …” instead of “No response from API · Retrying in …”, and triggers after 20s of silence instead of 10s

**Added**

- Added a warning when the requested model is deprecated or automatically updated to a newer model, shown on stderr in print mode (`-p` ) and now also covering models set in agent frontmatter
- Added `attribution.sessionUrl` setting to omit the claude.ai session link from commits and PRs in web and Remote Control sessions
- Added `/config --help` to list all available shorthand keys for`/config key=value`

**Changed**

- Changed `/config` toggle behavior: Enter and Space both change the selected setting, and Esc now saves and closes instead of reverting
- Removed the startup “setup issues” line under the logo — run `/doctor` to see configuration issues or use`--debug`

**Improved**

- Improved auto mode safety: destructive git commands (`git reset --hard` ,`git checkout -- .` ,`git clean -fd` ,`git stash drop` ) are now blocked when you didn’t ask to discard local work,`git commit --amend` is blocked when the commit wasn’t made by the agent this session, and`terraform destroy` /`pulumi destroy` /`cdk destroy` are blocked unless you asked for the specific stack

**Added**

- Added `/config key=value` syntax to set any setting from the prompt (e.g.`/config thinking=false` ) — works in interactive,`-p` , and Remote Control
- Added `sandbox.allowAppleEvents` opt-in setting that lets sandboxed commands send Apple Events on macOS
- Added `CLAUDE_CLIENT_PRESENCE_FILE` environment variable: point it at a marker file to suppress mobile push notifications while you’re at the machine

**Changed**

- Upgraded the bundled Bun runtime to 1.4
- Changed fullscreen mode URL opening to require Cmd+click (macOS) / Ctrl+click, matching native terminal behavior
- Changed the `Improved N memories` line to no longer list individual files outside verbose mode

**Improved**

- Improved streaming of long paragraphs: text now appears line-by-line instead of waiting for the first line break
- Improved auto-retry: API connection drops mid-thinking now automatically retry instead of showing “Connection closed while thinking”
- Improved the subagent panel: idle subagents auto-hide after 30s, the list caps at 5 rows with scroll hints, and keyboard hints now show in the footer
- Improved the MCP OAuth browser page to match Claude Code’s visual style and auto-close on success

- Fixed mid-stream connection drops: partial responses are now preserved instead of showing a raw error, and the spinner no longer gets stuck at “running tool”
- Fixed mouse-wheel scrolling in WSL2 under Windows Terminal and VS Code (regression in 2.1.172)
- Fixed a sandbox `denyRead` /`allowRead` glob over a large directory tree making the Bash tool description enormous and the session unusable on Linux
- Fixed the feedback survey capturing a single-digit reply as a session rating immediately after a turn completes
- Fixed the welcome screen stacking multiple promotional banners — at most one promo now shows per session
- Fixed Ctrl+O not showing the subagent’s transcript when viewing a subagent
- Fixed clicking the prompt input not returning focus from the subagent/footer panel
- Fixed remote session background tasks appearing stuck as “still running” between turns
- Improved plugin loading performance in remote sessions

**Added**

- Added `Tool(param:value)` syntax for permission rules to match a tool’s input parameters (with`*` wildcard), e.g.`Agent(model:opus)` to block Opus subagents

**Changed**

- Agent teams: removed the `TeamCreate` and`TeamDelete` tools. With`CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1` set, every session now has one implicit team — spawn teammates directly with the Agent tool’s`name` parameter, no setup step needed. The`team_name` parameter on the Agent tool is still accepted but ignored.
- Skills in nested `.claude/skills` directories now load when working on files there; on a name clash, the nested skill appears as`<dir>:<name>` so both stay available
- Nested `.claude/` directories: the agent, workflow, and output-style closest to the working directory now wins when names collide; project-scope workflow saves now target the closest existing`.claude/workflows/`
- Changed the workflow prompt keyword to use a purple shimmer highlight and trigger only on explicit phrases like “run a workflow” or “workflow:”, not on any mention of the word
- `/bug` now requires a description before submitting, and no longer uses model-refusal text as the GitHub issue title

**Improved**

- Improved auto mode: subagent spawns are now evaluated by the classifier before launch, closing a gap where a subagent could request a blocked action without review
- Improved `/doctor` with consistent flat tree layout across all sections, clearer section status icons, and highlighted command names
- Improved the skill listing truncation warning to show how many skill descriptions are affected
- Improved Remote Control error messages: connection failures now show a persistent red “/rc failed” indicator in the footer, and the “not yet enabled” error now explains whether it’s a gate, a check failure, stale entitlement, or org policy

**Added**

- Added `footerLinksRegexes` setting for regex-matched link badges in the footer row, configurable via user or managed settings

**Changed**

- Session titles are now generated in the language of your conversation (set the `language` setting to pin a specific language)
- Background sessions now show clearer guidance when a window left open across an auto-update can’t submit a reply, and `claude daemon status` explains version-skew behavior

**Improved**

- Improved Bedrock credential caching: credentials from `awsCredentialExport` are now cached until their`Expiration` instead of a fixed 1 hour

- Added `enforceAvailableModels` managed setting — when enabled, the`availableModels` allowlist also constrains the Default model (a Default that would resolve to a disallowed model now falls back to the first allowed model), and user or project settings can no longer widen a managed`availableModels` list

**Added**

- Added `wheelScrollAccelerationEnabled` setting to disable mouse-wheel scroll acceleration in fullscreen mode
- [VSCode] Added usage attribution to the Account & usage dialog (`/usage` ) showing cache misses, long context, subagents, and per-skill/agent/plugin/MCP breakdowns over the last 24h or 7d

- Fixed Fable 5 model names with a `[1m]` suffix not being normalized — Fable 5 includes 1M context by default, so the suffix is now stripped automatically
- Fixed a spurious “sandbox dependencies missing” startup warning on Windows when sandbox was enabled in settings

**Added**

- Added a search bar when browsing a marketplace’s plugins in `/plugin`
- Added `model` attribute to the`claude_code.lines_of_code.count` OTEL metric

**Changed**

- Sub-agents can now spawn their own sub-agents (up to 5 levels deep)
- Amazon Bedrock now reads the AWS region from `~/.aws` config files when`AWS_REGION` isn’t set, matching AWS SDK precedence;`/status` shows where the region came from
- Disable mouse tracking on Windows consoles that don’t fully support it
- Reduced idle CPU usage: `/goal` status chip no longer re-renders the terminal at 5 Hz while idle, and fewer UI re-renders while subagents run in parallel
- `/code-review` now keeps the`ultra` option visible when you’re not signed in to claude.ai, with an explanation that the cloud review requires a claude.ai account
- Shortened the Remote Control footer indicator to “/rc active” and hid it on narrow terminals
- Stopped promoting `/loop` in remote sessions, where pending loops don’t keep the container alive

**Improved**

- Improved performance in long conversations by removing redundant message normalization and avoiding full message-history transforms when streaming tool-use state is unchanged
- Improved Claude in Chrome tool loading: browser tools now load in a single batched call instead of one per tool
- Improved the non-interactive Usage Policy refusal message to suggest starting a new session or changing your model

- Introducing Claude Fable 5: a Mythos-class model that we’ve made safe for general use. Fable’s capabilities exceed those of any model we’ve ever made generally available. Update to version 2.1.170 for access. https://www.anthropic.com/news/claude-fable-5-mythos-5
- Fixed sessions not saving transcripts (and not appearing in —resume) when launched from the VS Code integrated terminal or any shell that inherited Claude Code environment variables.

**Added**

- Added `--safe-mode` flag (and`CLAUDE_CODE_SAFE_MODE` ) to start Claude Code with all customizations (CLAUDE.md, plugins, skills, hooks, MCP servers) disabled for troubleshooting
- Added `/cd` command to move a session to a new working directory without breaking the prompt cache mid-session
- Added a `disableBundledSkills` setting and`CLAUDE_CODE_DISABLE_BUNDLED_SKILLS` environment variable to hide bundled skills, workflows, and built-in slash commands from the model
- Added a tip suggesting `claude agents` when running multiple concurrent sessions

**Changed**

- `/workflows` now opens immediately even while a turn is in progress
- Reduced CPU usage while responses stream and during spinner animations
- Restored a default 5-minute idle timeout on Vertex/Foundry so a stalled stream aborts instead of hanging indefinitely; set `API_FORCE_IDLE_TIMEOUT=0` to opt out
- Remote-managed settings with an invalid entry now apply their remaining valid policies and surface the validation error, instead of silently dropping the whole payload
- Background sessions now preserve `--ide` ,`--chrome` ,`--bare` ,`--remote-control` , and other flags across retire→wake, and respawn state validation was hardened
- Background sessions are now told that shared-checkout edits are blocked until they enter a worktree, avoiding a wasted rejected edit before `EnterWorktree`
- The “CLAUDE.md is too long” warning threshold now scales with the model’s context window
- Auto-updater on Windows now stops retrying within a session once `claude.exe` is held by another process
- Promo credit claims for Apple/Google-billed subscribers without a payment method now explain where to add one

**Improved**

- Improved `TaskCreate` reliability: malformed inputs are repaired automatically and validation errors for unloaded tools include the schema
- Improved the error message shown when your organization has disabled API key authentication, with guidance based on where the active API key comes from
- Improved color contrast for skill tags in the slash-command menu

**Added**

- Added `fallbackModel` setting to configure up to three fallback models tried in order when the primary model is overloaded or unavailable;`--fallback-model` now also applies to interactive sessions
- Added glob pattern support in deny rule tool-name position (`"*"` denies all tools); allow rules reject non-MCP globs, and unknown tool names in deny rules warn at startup

**Changed**

- Hardened cross-session messaging: messages relayed via `SendMessage` from other Claude sessions no longer carry user authority — receivers refuse relayed permission requests, and auto mode blocks them
- `MAX_THINKING_TOKENS=0` ,`--thinking disabled` , and the per-model thinking toggle now disable thinking on models that think by default via the Claude API (3P providers unchanged)
- Claude Code now retries a turn once on the fallback model when the API rejects an unexpected non-retryable error; auth, rate-limit, request-size, and transport errors still surface immediately
- `claude update` now announces the target version before downloading instead of going silent
- `claude agents` : typing a URL into the list now filters to the session whose first prompt contained it

**Added**

- Added `requiredMinimumVersion` and`requiredMaximumVersion` managed settings — Claude Code refuses to start if its version is outside the allowed range and directs the user to an approved version
- Added `/plugin list` command to list installed plugins, with`--enabled` /`--disabled` filters
- Added a “c to copy” shortcut to `/btw` that copies the raw markdown answer to the clipboard, preserving formatting when pasted elsewhere
- Skills: added `\$` escape syntax to include a literal`$` before a digit in command bodies

**Changed**

- Hooks: Stop and SubagentStop hooks can now return `hookSpecificOutput.additionalContext` to give Claude feedback and keep the turn going without being labeled a hook error
- stdio MCP servers now receive the same `CLAUDE_CODE_SESSION_ID` as hooks/Bash on`--resume`
- Background agent sessions now update to a new Claude Code version in the background, so opening a session after an update no longer waits on a cold restart
- Clearer descriptions for built-in commands and skills in the / menu
- The subscription-switch suggestion now shows in the startup announcement slot instead of a toast
- `claude agents` dispatching from the state-grouped view now starts the session in the directory the agent view was opened from

**Changed**

- `claude agents --json` now includes`waitingFor` showing what a waiting session is blocked on (e.g. permission prompt)
- `--tools` : explicitly listing Grep/Glob now provides the dedicated search tools on native builds with embedded search (previously these names were silently ignored)
- `/effort` now confirms when your chosen level will persist as the default for new sessions
- Clicking a slash command in the autocomplete menu now fills it into your prompt instead of running it immediately; press Enter to run
- Remote Control now shows as a persistent footer pill (with a link to the session) instead of a startup message
- Renamed Windsurf to Devin Desktop in the `/ide` menu,`/terminal-setup` , and`/scroll-speed` , following the editor’s rebrand
- Quieter startup: notices group by severity, and session info and announcements share a single line per launch
- Startup warnings rewritten to be shorter and clearer, each with a concrete fix
- Launch-prompt warnings (deep link/pre-filled prompt) now stay pinned below the input until you act instead of scrolling away
- Failed turns now show a compact warning line instead of a multi-line red error block
- Background dispatch spawn failures now report the error class name when no errno is available
- Removed the “Claude in Chrome enabled” and “marketplace installed” startup messages; model auto-updates and the team-onboarding tip now show as quiet notices under the logo

**Improved**

- Improved background service startup and `claude update` verification to wait out endpoint-security scanning of new binaries instead of failing after 5 seconds

**Added**

- [VSCode] Added a tip suggesting disabling terminal GPU acceleration (or running `/terminal-setup` ) to fix garbled glyphs

**Changed**

- `OTEL_RESOURCE_ATTRIBUTES` values are now included as labels on metric datapoints, so you can slice usage metrics by custom dimensions like team or repo
- `claude agents` rows now show`done/total` before the detail when work is fanned out; peek shows the longest-running item
- `/mcp` now collapses claude.ai connectors you’ve never signed in to behind a “Show unused connectors” row
- Parallel tool calls: a failed Bash command no longer cancels other calls in the same batch — each tool returns its own result independently
- Fullscreen mode: clipboard now uses `wl-copy` /`xclip` /`xsel` on Linux when available, copies to both the clipboard and PRIMARY selection for middle-click paste, and the “hold`{key}` for native selection” hint now shows the correct key per terminal

**Improved**

- Improved terminal rendering performance by stabilizing the layout engine’s JIT compilation profile
- Improved rendering performance for large file writes

**Added**

- Added a prompt before writing to shell startup files (`.zshenv` ,`.zlogin` ,`.bash_login` ) and`~/.config/git/` , which could otherwise lead to unintended command execution

**Changed**

- `acceptEdits` mode now prompts before writing build-tool config files that grant code execution (`.npmrc` ,`.yarnrc*` ,`bunfig.toml` ,`.bazelrc` ,`.pre-commit-config.yaml` ,`.devcontainer/` , etc.)
- Edit no longer requires a separate Read after viewing a file with `grep` : single-file`grep` /`egrep` /`fgrep` commands now satisfy the read-before-edit check
- Removed `CLAUDE_CODE_OPUS_4_6_FAST_MODE_OVERRIDE` ; the environment variable is now a no-op
- Removed the JetBrains plugin install suggestion from startup
- Renamed the dynamic-workflow trigger keyword from `workflow` to`ultracode` . The word “workflow” no longer triggers a run; asking for one in your own words still works. The trigger keyword is highlighted in violet in the prompt input

**Improved**

- Improved performance of opening recently-inactive background agent sessions in `claude agents`
- Improved auto mode classifier latency by reducing reasoning on routine actions, lowering the chance of “could not evaluate this action” blocks
- Improved background-session teardown (`claude rm` /`stop` , idle reap) to send SIGTERM to running shell subprocesses before SIGKILL, so cleanup handlers run

- Auto mode is now available on Bedrock, Vertex, and Foundry for Opus 4.7 and Opus 4.8. Opt in by setting `CLAUDE_CODE_ENABLE_AUTO_MODE=1`

**Added**

- Added `claude plugin init <name>` to scaffold a new plugin in`.claude/skills`
- Added autocomplete for `/plugin` arguments: subcommands, installed plugin names, and plugins from known marketplaces
- Added a “Workflow keyword trigger” setting in /config to stop the word “workflow” in a prompt from triggering a dynamic workflow

**Changed**

- Plugins in `.claude/skills` directories are now automatically loaded, no marketplace required
- `claude agents` : the`agent` field in`settings.json` is now honored for dispatched sessions, with`--agent <name>` to override it
- `EnterWorktree` can now switch between Claude-managed worktrees mid-session
- `tool_decision` telemetry events now include`tool_parameters` (bash commands, MCP/skill names) when`OTEL_LOG_TOOL_DETAILS=1`
- Worktrees managed by Claude are now left unlocked when the agent finishes, so `git worktree remove` /`prune` can clean them up
- `/terminal-setup` now disables GPU acceleration in VS Code/Cursor/Windsurf integrated terminals to prevent garbled-text rendering
- The Feature of the Week credit-claim status now appears as a notification in the status area instead of a line above the prompt
- `claude agents` : slash-command autocomplete in the dispatch input now matches substrings
- Removed the “bash commands will be sandboxed” startup banner — sandbox status still shows in `/status` and when a command is blocked
- Removed the “/ide for …” startup hint toast
- Pressing backspace right after a workflow trigger keyword now dismisses the workflow request (same as alt+w) instead of deleting a character

**Improved**

- Improved performance of long and resumed conversations by eliminating redundant message-rendering recomputations

- Fixed an issue when using Opus 4.8 where thinking blocks were modified, leading to API errors.

**Added**

- Introducing dynamic workflows: ask Claude to create a workflow and it orchestrates work across tens to hundreds of agents in the background, so you can take on larger, more complex tasks. Run `/workflows` to view your runs
- Added Claude Opus 4.8 support and 4.7 → 4.8 migration guidance to the `/claude-api` skill

**Changed**

- Opus 4.8 is here! Now defaults to high effort · /effort xhigh for your hardest tasks
- Fast mode on Opus 4.8 is now available at a fraction of its previous cost: 2x the standard rate for 2.5x the speed
- The lean system prompt is now the default for all models except Haiku, Sonnet, and Opus 4.7 and earlier
- Claude now reserves the multiple-choice question prompt for decisions it genuinely cannot make itself, instead of asking when it already has enough context to proceed
- `/simplify` now runs a cleanup-only review (reuse, simplification, efficiency, altitude) and applies the fixes, instead of running the full`/code-review --fix` bug-hunting review
- Renamed the `/effort` slider labels from “Speed”/“Intelligence” to “Faster”/“Smarter” for clarity
- `claude agents` : type`! <command>` to run a shell command as a background session you can attach to and detach from. Also available as`claude --bg --exec '<command>'`
- `claude agents` :`/logout` now signs you out instead of being sent to a background session
- `←←` to open the agents view now works on Bedrock, Vertex, Foundry, and with telemetry disabled
- Claude in Chrome: pick which connected browser to use via `/chrome` → “Select browser…”, or in-chat when a browser action runs with multiple connected
- Plugins can now declare `defaultEnabled: false` in`plugin.json` or a marketplace entry; enable them with`/plugin` or`claude plugin enable` . Dependencies of enabled plugins are still enabled automatically
- The `/plugin` Discover tab now pins plugins whose relevance signals match the current directory with a “suggested for this directory” annotation
- Streaming tool execution is now always enabled, including when telemetry is disabled or on Bedrock/Vertex/Foundry (previously behind a feature flag)
- Stdio MCP server subprocesses now receive `CLAUDE_CODE_SESSION_ID` and`CLAUDECODE=1` in their environment
- `claude mcp list` /`get` now show unapproved`.mcp.json` servers as`⏸ Pending approval` instead of auto-approving and connecting when output is piped
- `/remote-control` autocomplete now shows “Disconnect Remote Control” when Remote Control is already active
- Deprecated `CLAUDE_CODE_OPUS_4_6_FAST_MODE_OVERRIDE` (will be removed on 06/01). To use fast mode on Opus 4.6, switch with`/model claude-opus-4-6[1m]` and then`/fast on`
- Removed the stale ”& for background” hint from the shortcuts help panel
- [VSCode] Auto mode no longer requires the bypass-permissions setting to appear in the mode picker, and a dismissable notice on the new-session screen explains auto mode the first time it’s active

**Improved**

- Improved the auto-mode classifier’s detection of data exfiltration, particularly bulk transfers of repository contents

**Added**

- Added `skipLfs` option to`github` /`git` plugin marketplace sources to skip Git LFS downloads during clone and update

**Changed**

- Claude Code now shows a one-time notice when your npm global install can’t auto-update; `/doctor` lists the fixes
- Status line commands now receive `COLUMNS` and`LINES` environment variables so scripts can size output to the terminal width
- `claude agents` : autocomplete in the dispatch input now suggests native slash commands and bundled skills, not just project skills
- `claude agents` : PR column now shows`PR #N` for a single PR or`N PRs` for multiple
- `claude doctor` now shows the result of your last update attempt
- Combined the separate “needs authentication” startup notifications for MCP servers and connectors into a single message
- macOS: background agents now appear as “Claude Code” in Privacy & Security and keep their permission grants across upgrades
- `--strict-mcp-config` no longer strips inline`mcpServers` from explicitly-passed agent definitions (`--agents` / SDK`agents` ), and blocked subagent MCP servers now surface a visible warning
- `/bg` while Claude is responding now continues the response in the background session instead of dropping it
- `/model` now saves your selection as the default for new sessions (matching the IDE). Press`s` in the picker to switch models for the current session only.
- If you customized the `modelPicker:setAsDefault` keybinding, rename it to`modelPicker:thisSessionOnly` in keybindings.json (the`d` action was replaced by`s` )

**Added**

- Added `/reload-skills` command to re-scan skill directories without restarting the session
- Added a `MessageDisplay` hook event that lets hooks transform or hide assistant message text as it is displayed
- Added `pluginSuggestionMarketplaces` managed setting: admins can allowlist org marketplaces whose plugins may be suggested via context-aware tips
- Added the session entrypoint as an OpenTelemetry metric attribute (`app.entrypoint` , opt-in via`OTEL_METRICS_INCLUDE_ENTRYPOINT=true` )

**Changed**

- `/code-review --fix` now applies review findings to your working tree after the review, surfacing reuse, simplification, and efficiency suggestions;`/simplify` now invokes`/code-review --fix`
- Skills and slash commands can now set `disallowed-tools` in frontmatter to remove tools from the model while the skill is active
- `SessionStart` hooks can now return`reloadSkills: true` to re-scan skill directories, making skills installed by the hook available in the same session
- `SessionStart` hooks can now set the session title via`hookSpecificOutput.sessionTitle` on startup and resume
- `claude plugin marketplace remove` now accepts`--scope user|project|local` for symmetry with`marketplace add` ,`install` , and`uninstall`
- Claude Code now switches to your configured `--fallback-model` for the rest of the session when the primary model is not found, instead of failing every request
- Auto mode no longer requires opt-in consent
- Vim mode: `/` in NORMAL mode now opens reverse history search (like Ctrl+R), matching bash/zsh vi-mode
- The `/usage` breakdown now includes large session files; files are scanned with a streaming read so memory usage stays flat
- Thinking summaries in the collapsed group now stay readable for at least 3 seconds, render as markdown, and cap at 10 lines (`Ctrl+O` shows the full thinking)
- In fullscreen mode, the “Thinking for Ns” indicator now counts up live while the model is thinking, and keeps its value if you interrupt mid-thought
- Simplified the Workflow tool’s inline progress display — live agent counts now show only in the persistent workflow status row below the prompt
- The post-response timer now shows “Waiting for N background agents/workflows to finish” when backgrounded agents or workflows are still running, and reports the cumulative time once their results are processed

**Added**

- Enterprise: added the `allowAllClaudeAiMcps` managed setting to load claude.ai cloud MCP connectors alongside`managed-mcp.json`

**Changed**

- `/usage` now shows a per-category breakdown of what’s driving your limits usage — skills, subagents, plugins, and per-MCP-server cost
- `/diff` detail view can now be scrolled with the keyboard (arrows,`j` /`k` ,`PgUp` /`PgDn` ,`Space` ,`Home` /`End` )
- Markdown output now renders GFM task list checkboxes (`- [ ] todo` /`- [x] done` ) instead of plain bullets

**Improved**

- Improved `/feedback` reports to include the conversation that happened before context compaction, making issues from earlier in long sessions easier to triage

- Fixed the Bash tool returning exit code 127 on every command for some users (a regression introduced in 2.1.147)

**Changed**

- Pinned background sessions (`Ctrl+T` in`claude agents` ) now stay alive when idle, are restarted in place to apply Claude Code updates, and are shed under memory pressure only after non-pinned sessions
- Renamed `/simplify` to`/code-review` . It now reports correctness bugs at a chosen effort level (e.g.,`/code-review high` ); pass`--comment` to post findings as inline GitHub PR comments. The old cleanup-and-fix behavior has been removed
- Prompt history no longer records consecutive duplicate entries — recalling a prompt with arrow-up and submitting it again won’t add another copy

**Improved**

- Improved auto-updater: retries transient network failures, reports specific error categories and OS error codes on failure, and shows the current version when an update fails
- Improved diff rendering performance for large file edits

**Added**

- Added `claude agents --json` to list live Claude sessions as JSON for scripting (tmux-resurrect, status bars, session pickers)
- Added `agent_id` and`parent_agent_id` attributes to`claude_code.tool` OTEL spans, and fixed trace parenting so background subagent spans nest under the dispatching Agent tool span

**Changed**

- Status line JSON input now includes GitHub repo and PR information when detected
- `/plugin` Discover and Browse screens now show a plugin’s commands, agents, skills, hooks, and MCP/LSP servers before installation
- `claude agents` terminal tab title now shows the awaiting-input count so an alt-tabbed window tells you when an agent needs attention
- Slash command and @-mention suggestion list now supports mouse hover and click in fullscreen mode
- Stop and SubagentStop hook input now includes `background_tasks` and`session_crons` fields

**Improved**

- Improved the Read tool to return a truncated first page with a “PARTIAL view” notice instead of a hard error when a whole-file read exceeds the token limit

**Added**

- Added `/resume` support for background sessions — sessions started via`claude --bg` or agent view now appear alongside interactive ones, marked with`bg`
- Added elapsed duration to background subagent completion notifications (e.g. “Agent completed · 3h 2m 5s”)

**Changed**

- The `/plugin` browse and discover panes now show when a plugin was last updated
- `/model` now changes the model for the current session only; press`d` in the model picker to set a default for new sessions
- Renamed “extra usage” to “usage credits” across CLI copy; `/extra-usage` is now`/usage-credits` (old name still works)
- Reduced terminal rendering glitches in VS Code by reducing spinner animation color count
- Fewer spurious tool errors during search: `head` /`tail` file views now satisfy the read-before-edit check, and a “no matches” result (exit code 1) from`egrep` ,`fgrep` ,`git grep` , or`git diff` is no longer reported as a command failure
- Resumed sessions now keep the model they were using instead of picking up another session’s `/model` choice
- `/bg` and`←` -detach now preserve directories added via`/add-dir`
- `claude agents` /`--bg` rejection messages now name the specific gate (non-TTY, env var, or setting) instead of a generic message
- `claude --bg --name <label>` now echoes the name in the post-spawn confirmation
- `claude agents` : renaming a background session with Ctrl+R now updates the attached session’s banner immediately
- Background session worktree isolation guard now applies for non-git VCS users with `WorktreeCreate` hooks configured
- Plugin marketplace add/update now respects `CLAUDE_CODE_PLUGIN_PREFER_HTTPS`
- `/plugin` now returns to the Installed list after enabling, disabling, or uninstalling a plugin
- `/doctor` now shows an exec-form example when a command hook is missing the`command` field
- Skill-listing truncation is no longer shown as a startup notification — run `/doctor` for the full breakdown
- The post-survey follow-up hint now appears after every non-dismiss survey response with context-aware copy, making it easier to share more detail via /feedback.

**Improved**

- Improved recovery from rare pre-response stream stalls — now retries streaming once instead of falling back to a slower non-streaming request
- Improved SDK/headless MCP startup: pre-wait now overlaps startup instead of blocking before the first turn (up to 2s faster with slow MCP servers)

**Added**

- Added plugin dependency enforcement: `claude plugin disable` now refuses when another enabled plugin depends on the target (with a copy-pasteable disable-chain hint), and`claude plugin enable` force-enables transitive dependencies
- Added projected context cost (per-turn and per-invocation token estimates) to the `/plugin` marketplace browse pane
- Added `worktree.bgIsolation: "none"` setting to let background sessions edit the working copy directly without`EnterWorktree` , for repos where worktrees are impractical

**Changed**

- PowerShell tool now passes `-ExecutionPolicy Bypass` . Opt out with`CLAUDE_CODE_POWERSHELL_RESPECT_EXECUTION_POLICY=1`
- Background sessions now preserve the model and effort level you set after waking from idle
- Shift+Tab in attached agent sessions now includes auto mode in the cycle
- The PowerShell tool is now enabled by default on Windows for Bedrock, Vertex, and Foundry users. Opt out with `CLAUDE_CODE_USE_POWERSHELL_TOOL=0` .
- `claude agents` now accepts`--add-dir` ,`--settings` ,`--mcp-config` , and`--plugin-dir` and applies them to the dashboard and to background sessions dispatched from it
- `claude agents` accepts`--permission-mode` ,`--model` ,`--effort` , and`--dangerously-skip-permissions` to set defaults for sessions dispatched from the view
- `claude --bg --dangerously-skip-permissions` now persists across retire→wake
- Worktree cleanup no longer falls back to `rm -rf` when`git worktree remove` fails, preventing loss of gitignored or in-progress files
- `/bg` now preserves`--mcp-config` ,`--settings` ,`--add-dir` ,`--plugin-dir` , and`--strict-mcp-config` , so backgrounded sessions keep their MCP servers and settings across respawn.
- Background sessions launched from `claude agents` now honor`permissions.defaultMode` from settings.json (was previously overridden to auto mode)
- `/bg` and`←` -detach now preserve`--fallback-model` , so backgrounded workers degrade to the fallback model on overload instead of hard-failing.
- `/bg` and`←` -detach now preserve`--allow-dangerously-skip-permissions` , so the forked worker keeps bypass-permissions available in its Shift+Tab cycle.

**Added**

- Added new `claude agents` flags:`--add-dir` ,`--settings` ,`--mcp-config` ,`--plugin-dir` ,`--permission-mode` ,`--model` ,`--effort` , and`--dangerously-skip-permissions` to configure dispatched background sessions

**Changed**

- Fast mode now uses Opus 4.7 by default (previously Opus 4.6). Set `CLAUDE_CODE_OPUS_4_6_FAST_MODE_OVERRIDE=1` to pin fast mode to Opus 4.6
- Plugins with a root-level `SKILL.md` and no`skills/` subdirectory are now surfaced as a skill
- The `/plugin` details pane and`claude plugin details` now show LSP servers a plugin provides
- `/web-setup` warns before replacing an existing GitHub App connection
- Removed stale `/model claude-sonnet-4-20250514` suggestion from Usage Policy refusal messages

**Improved**

- Improved reactive compaction: the first summarize attempt now seeds from the original request’s overflow size, avoiding a wasted near-full-context retry
- Improved hook configuration error: configuring a prompt- or agent-type hook for `SessionStart` /`Setup` /`SubagentStart` now shows a clear “use a command-type hook instead” error

**Added**

- Added `terminalSequence` field to hook JSON output so hooks can emit desktop notifications, window titles, and bells without a controlling terminal
- Added `CLAUDE_CODE_PLUGIN_PREFER_HTTPS` to clone GitHub plugin sources over HTTPS instead of SSH, for environments without a GitHub SSH key
- Added `ANTHROPIC_WORKSPACE_ID` environment variable for workload identity federation — scopes the minted token to a specific workspace when the federation rule covers more than one
- Added `claude agents --cwd <path>` to scope the session list to a directory
- Rewind menu: added “Summarize up to here” to compress earlier context while keeping recent turns intact

**Changed**

- `/feedback` can now include recent sessions (last 24 hours or 7 days) for issues spanning more than the current session
- Auto mode permission dialog now explains when a `permissions.ask` rule caused the prompt
- Restored the “view diff in your IDE” option on file-edit permission prompts when an IDE is connected
- Background agents launched via `/bg` or`←←` now preserve the current permission mode instead of reverting to default
- `claude agents` : agents that finish work but leave a background shell running now move to Completed instead of staying under Working
- Bedrock: `awsCredentialExport` now always runs when configured instead of being skipped when ambient AWS credentials resolve, fixing auth for cross-account access
- [VSCode] Voice mode: the WSL error now suggests installing `sox libsox-fmt-pulse` for WSLg users
- `claude agents` : launching a session no longer fails when the pre-warmed background worker is unhealthy — now falls back to a fresh launch
- `claude agents` no longer shows empty placeholder sessions left over from backgrounding a fresh REPL, and shows onboarding text when entered via ← with no other agents
- Empty idle background sessions left over from `←` are now automatically retired by the daemon after 5 minutes

**Improved**

- Improved spinner feedback during long thinking periods — the spinner now warms to amber after 10 seconds to signal Claude is still working
- Improved plugin menu navigation: `→` /Tab switch tabs,`↑` moves to the tab strip, and tab headers and search box are clickable in fullscreen mode

**Changed**

- Updated agent color palette
- Plugins now warn when a default component folder (e.g. `commands/` ) is silently ignored because`plugin.json` sets the matching key. Shown in`/doctor` ,`claude plugin list` , and`/plugin` .

**Improved**

- Improved Agent tool `subagent_type` matching to accept case- and separator-insensitive values (e.g.`"Code Reviewer"` resolves to`code-reviewer` )

**Added**

- Added agent view (Research Preview): a single list of every Claude Code session — running, blocked on you, or done. Run `claude agents` to get started. See https://code.claude.com/docs/en/agent-view
- Added `/goal` command: set a completion condition and Claude keeps working across turns until it’s met. Works in interactive,`-p` , and Remote Control. Shows live elapsed/turns/tokens as an overlay panel
- Added `/scroll-speed` command to tune mouse wheel scroll speed with a live preview
- Added `claude plugin details <name>` to show a plugin’s component inventory and projected per-session token cost
- Added transcript view navigation: `?` for keyboard shortcuts,`{` /`}` to jump between user prompts,`v` to toggle shortcut panel
- Added hook `args: string[]` field (exec form) that spawns the command directly without a shell, so path placeholders never need quoting
- Added hook `continueOnBlock` config option for`PostToolUse` — set to`true` to feed the hook’s rejection reason back to Claude and continue the turn

**Changed**

- MCP stdio servers now receive `CLAUDE_PROJECT_DIR` in their environment, matching hooks. Plugin configs can reference`${CLAUDE_PROJECT_DIR}` in commands
- Compaction prompt now asks the model to preserve sensitive user instructions
- `/mcp` Reconnect now picks up`.mcp.json` edits without a restart, and shows the HTTP status and URL when reconnecting fails
- `/context all` per-skill token estimates now account for the model’s tokenizer and show rounded values
- `claude plugin install <name>@<marketplace>` now auto-refreshes the marketplace and retries before reporting a plugin as not found
- `/plugin` installed-plugin details now show hook event names and MCP server names cleanly
- `/context` now shows the providing plugin’s name for plugin-sourced skills
- Remote MCP server reconnect retry on transient failures is now enabled for all users
- API requests from subagents now carry `x-claude-code-agent-id` /`x-claude-code-parent-agent-id` headers, and`claude_code.llm_request` OTEL spans include`agent_id` /`parent_agent_id` attributes
- Remote Control, `/schedule` , claude.ai MCP connectors, and notification preferences are now disabled when`ANTHROPIC_API_KEY` /`apiKeyHelper` /`ANTHROPIC_AUTH_TOKEN` is set, even if a Claude.ai login also exists. Unset the API key to use these features
- [VSCode] Press Cmd/Ctrl+Shift+T to reopen the most recently closed session tab, configurable via `claudeCode.enableReopenClosedSessionShortcut`

**Added**

- Added `CLAUDE_CODE_ENABLE_FEEDBACK_SURVEY_FOR_OTEL` to re-enable the session quality survey for enterprises capturing responses through OpenTelemetry
- Added `settings.autoMode.hard_deny` for auto mode classifier rules that block unconditionally regardless of user intent or allow exceptions

**Changed**

- WSL2: image paste from Windows clipboard now works via a PowerShell fallback when xclip/wl-paste cannot read image data
- Changed plugin marketplace removal key to `d` (matching delete elsewhere) instead of`r` which collided with retry

**Improved**

- Improved visual consistency across slash command dialogs: standardized footer hints, dialog spacing, and arrow-key styling, and the dialog frame now appears immediately during loading instead of popping in after
- Improved error message when `--worktree` collides with an existing or stale worktree

**Added**

- Added `worktree.baseRef` setting (`fresh` |`head` ) to choose whether`--worktree` ,`EnterWorktree` , and agent-isolation worktrees branch from`origin/<default>` or local`HEAD` .**Note:** the default`fresh` changes`EnterWorktree` ’s base back to`origin/<default>` (it has been local`HEAD` since 2.1.128) — set`worktree.baseRef: "head"` to keep unpushed commits in new worktrees
- Added `sandbox.bwrapPath` and`sandbox.socatPath` managed settings (Linux/WSL) to specify custom bubblewrap and socat binary locations
- Added `parentSettingsBehavior` admin-tier key (`'first-wins' | 'merge'` ) to let admins opt SDK`managedSettings` (parent tier) into the policy merge

**Changed**

- Hooks now receive the active effort level via the `effort.level` JSON input field and the`$CLAUDE_EFFORT` environment variable, and Bash tool commands can read`$CLAUDE_EFFORT`
- `claude --help` now lists`--remote-control` alongside`--remote-control-session-name-prefix`

**Improved**

- Improved focus mode behavior
- Improved memory usage by releasing warm-spare background workers under memory pressure

**Added**

- Added `CLAUDE_CODE_SESSION_ID` environment variable to the Bash tool subprocess environment, matching the`session_id` passed to hooks
- Added `CLAUDE_CODE_DISABLE_ALTERNATE_SCREEN=1` env var to opt out of the fullscreen alternate-screen renderer and keep the conversation in the terminal’s native scrollback
- Added a “Pasting…” footer hint while a Ctrl+V image paste is being read from the clipboard

**Changed**

- Updated the `/tui fullscreen` startup banner to describe additional renderer benefits (lower memory usage, mouse support, auto-copy on select)

**Improved**

- Improved visual consistency in slash command dialogs and `/login` ,`/upgrade` ,`/extra-usage` dialog spacing

- Fixed VS Code extension failing to activate on Windows due to a hardcoded build path in the bundled SDK (`createRequire` polyfill bug)
- Fixed Mantle endpoint authentication failing with missing `x-api-key` header

**Added**

- Added `--plugin-url <url>` flag to fetch a plugin`.zip` archive from a URL for the current session
- Added `CLAUDE_CODE_FORCE_SYNC_OUTPUT=1` env var to force-enable synchronized output on terminals that auto-detection misses (e.g. Emacs`eat` )
- Added `CLAUDE_CODE_PACKAGE_MANAGER_AUTO_UPDATE` : when set on Homebrew or WinGet installations, Claude Code runs the upgrade command in the background and prompts to restart

**Changed**

- Plugin manifests: `themes` and`monitors` should now be declared under`"experimental": { ... }` . Top-level declarations still work but`claude plugin validate` will warn
- Gateway `/v1/models` discovery for the`/model` picker is now opt-in via`CLAUDE_CODE_ENABLE_GATEWAY_MODEL_DISCOVERY=1` (was automatic in 2.1.126–2.1.128)
- Ctrl+R history picker now defaults to searching all prompts across all projects, matching pre-2.1.124 behavior. Press Ctrl+S to narrow to the current project or session
- Third-party deployments (Bedrock, Vertex, Foundry, or `ANTHROPIC_BASE_URL` gateway) no longer see spinner tips pointing at first-party Anthropic surfaces
- `skillOverrides` setting now works:`off` hides from model and`/` ,`user-invocable-only` hides from model only,`name-only` collapses description
- The `claude_code.pull_request.count` OTel metric now counts PRs/MRs created via MCP tools, not just shell commands
- Policy refusal error messages now include the API Request ID for easier support debugging

**Changed**

- Bare `/color` (no args) now picks a random session color
- `/mcp` now shows the tool count for connected servers and flags servers that connected with 0 tools
- `--plugin-dir` now accepts`.zip` plugin archives in addition to directories
- `--channels` now works with console (API key) authentication — console orgs with managed settings must set`channelsEnabled: true` to enable
- Updated `/model` picker: collapsed duplicate Opus 4.7 entries, and current Opus now shows as “Opus” instead of “Opus 4.7”
- Subprocesses (Bash, hooks, MCP, LSP) no longer inherit `OTEL_*` environment variables, so OTEL-instrumented apps run via the Bash tool no longer pick up the CLI’s own OTLP endpoint
- MCP: `workspace` is now a reserved server name — existing servers with that name will be skipped with a warning
- Reconnecting MCP servers no longer flood the conversation with full tool-name lists on every reconnect — re-announced tools are summarized by server prefix
- SDK hosts now receive a persistent `localSettings` suggestion for Bash permission prompts, so “Always allow” writes to`.claude/settings.local.json`
- `EnterWorktree` now creates the new branch from local HEAD as documented, instead of`origin/<default-branch>` — unpushed commits are no longer dropped
- Auto mode: when the classifier can’t evaluate an action, the error now includes a hint (retry, `/compact` , or run with`--debug` )
- Headless `--output-format stream-json` :`init.plugin_errors` now includes`--plugin-dir` load failures in addition to dependency demotions

**Changed**

- The `/model` picker now lists models from your gateway’s`/v1/models` endpoint when`ANTHROPIC_BASE_URL` points at an Anthropic-compatible gateway
- 
  - Added `claude project purge [path]` to delete all Claude Code state for a project (transcripts, tasks, file history, config entry) — supports`--dry-run` ,`-y/--yes` ,`-i/--interactive` , and`--all`
- Added 
- `--dangerously-skip-permissions` now bypasses prompts for writes to`.claude/` ,`.git/` ,`.vscode/` , shell config files, and other previously-protected paths (catastrophic removal commands still prompt as a safety net)
- `claude auth login` now accepts the OAuth code pasted into the terminal when the browser callback can’t reach localhost (WSL2, SSH, containers)
- `claude_code.skill_activated` OpenTelemetry event now fires for user-typed slash commands and carries a new`invocation_trigger` attribute (`"user-slash"` ,`"claude-proactive"` , or`"nested-skill"` )
- Auto mode: the spinner now turns red when a permission check stalls, instead of looking like the tool is running
- Host-managed deployments (`CLAUDE_CODE_PROVIDER_MANAGED_BY_HOST` ) no longer auto-disable analytics on Bedrock/Vertex/Foundry
- Windows: PowerShell 7 installed via the Microsoft Store, MSI without PATH, or `.NET global tool` is now detected
- Windows: when the PowerShell tool is enabled, Claude now treats PowerShell as the primary shell instead of defaulting to Bash
- Read tool: removed the per-file malware-assessment reminder that could cause spurious refusals and “this is not malware” commentary on legacy models
- Bounded total size of file-modified reminders when a linter touches many files at once
- Windows: clipboard writes no longer expose copied content in process command-line arguments visible to EDR/SIEM telemetry; also fixes >22KB selections not reaching the clipboard
- PowerShell tool: bare `--` (e.g.`git diff -- file` ) is no longer mis-flagged as the`--%` stop-parsing token

- Fixed OAuth authentication failing with a 401 retry loop when `CLAUDE_CODE_DISABLE_EXPERIMENTAL_BETAS=1` is set

**Added**

- Added `ANTHROPIC_BEDROCK_SERVICE_TIER` environment variable to select a Bedrock service tier (`default` ,`flex` , or`priority` ), sent as the`X-Amzn-Bedrock-Service-Tier` header
- OpenTelemetry: added `claude_code.at_mention` log event for`@` -mention resolution

**Changed**

- Pasting a PR URL into the `/resume` search box now finds the session that created that PR (GitHub, GitHub Enterprise, GitLab, and Bitbucket)
- `/mcp` now shows claude.ai connectors hidden by a manually-added server with the same URL, with a hint to remove the duplicate
- Clarified the `/mcp` message shown when an MCP server is still unauthorized after the browser sign-in flow
- OpenTelemetry: numeric attributes on `api_request` /`api_error` log events are now emitted as numbers, not strings
- Voice mode: keybindings bound to Caps Lock now show an error since terminals don’t deliver Caps Lock as a key event

**Added**

- Added `alwaysLoad` option to MCP server config — when`true` , all tools from that server skip tool-search deferral and are always available
- Added `claude plugin prune` to remove orphaned auto-installed plugin dependencies;`plugin uninstall --prune` cascades
- Added a type-to-filter search box to `/skills` so you can find a skill in long lists without scrolling
- OpenTelemetry: added `stop_reason` ,`gen_ai.response.finish_reasons` , and`user_system_prompt` (gated behind`OTEL_LOG_USER_PROMPTS` ) to LLM request spans

**Changed**

- PostToolUse hooks can now replace tool output for all tools via `hookSpecificOutput.updatedToolOutput` (previously MCP-only)
- Fullscreen mode: typing into the prompt no longer jumps scroll back to the bottom after you’ve scrolled up to read earlier output
- Dialogs that overflow the terminal are now scrollable with arrow keys, PgUp/PgDn, home/end, and mouse wheel in both fullscreen and non-fullscreen modes
- Clicking any line of a long URL that wraps across rows in fullscreen mode now opens the full URL
- SDK and `claude -p` :`CLAUDE_CODE_FORK_SUBAGENT=1` now works in non-interactive sessions
- `--dangerously-skip-permissions` no longer prompts for writes to`.claude/skills/` ,`.claude/agents/` , and`.claude/commands/`
- `/terminal-setup` now enables iTerm2’s “Applications in terminal may access clipboard” setting so`/copy` works, including from tmux
- MCP servers that hit a transient error during startup now auto-retry up to 3 times instead of staying disconnected
- The terminal tab session title is now generated in your configured `language` setting
- Claude.ai connectors with the same upstream URL are now deduplicated instead of appearing as duplicates
- Vertex AI: support X.509 certificate-based Workload Identity Federation (mTLS ADC)
- Faster startup after upgrading: removed the Recent Activity panel from the release-notes splash
- LSP diagnostic summaries now expand on click/ctrl+o and show the expand hint
- SDK: `mcp_authenticate` now supports`redirectUri` for custom scheme completion and claude.ai connectors
