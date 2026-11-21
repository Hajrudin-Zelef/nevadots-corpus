---
id: collect-261121-veille-ia/divers-tech/opencode-changelog-november-2026
title: "OpenCode Changelog (November 2026)"
domain: divers-tech
role: reference
task: article
actors: []
dates: ["2026-11-18"]
keywords: ["background", "compaction", "november", "prompt", "opencode", "timeout", "instead", "claude"]
source: https://www.gradually.ai/en/changelogs/opencode/
source_anchor: ""
source_lines: [1, 397]
section: "Divers_Tech"
---
## Latest Releases (179)

November 18, 2026

- â¢**GPT-6.2 Prompt Caching** : GPT-6.2 models don't cache a system prompt sent in the Responses`instructions` field, so every step re-billed it as uncached input. OpenCode now sends it as a leading developer message on every Responses route, including OpenAI (API key and ChatGPT sign-in), GitHub Copilot, Zen, Azure and OpenRouter, so it is cached from the second step on. (#64542, #64544)
- â¢Fixed GPT-6.2 models re-billing the system prompt as uncached input on every step. On GitHub Copilot `gpt-6.2-sol` , uncached input per step dropped from about 14,000 tokens to 3. Compaction through`/responses/compact` now reads the same cached prefix. (#64542, #64544)
- â¢`webfetch` structured output no longer repeats the`url` and`format` it was called with. (#64534)

November 18, 2026

- â¢**Timed-Out Shell Commands Keep Running** : A shell command still running when its timeout expires now moves to the background instead of being killed, so long builds and test runs finish and report back. A foreground`timeout: 0` now means the default 120 seconds. (#63699)
- â¢**Compaction Without the Wait** : As a session nears automatic compaction, the summary is now written in the background, so your next prompt doesn't pause for it. Set`compaction.background: false` to turn it off; a background summary that ends up unused still costs tokens. (#64290)
- â¢**GPT 6.2 with ChatGPT Sign-In** : GPT 6.2 Sol and GPT 6.2 Astra, plus their fast variants, now show up in the model picker when you sign in with ChatGPT. (#64508)
- â¢**Update Plugins from Settings** : Open**Settings âº Extensions** , click**Check for updates** , then**Update** on any plugin with a new version. (#64511)
- â¢**Comment on Selected Code** : Selecting code in diff and file viewers now just selects it, so you can copy it, and an**Add comment** toolbar appears beside the selection. (#64410)
- â¢Shell commands still running when their timeout expires now move to the background instead of being killed, in every build. The `shell_timeout_background` flag is gone; configs that still set it keep loading. A foreground`timeout: 0` now uses the default 120 second timeout instead of waiting until the command exits. (#63699)
- â¢Background compaction is now on by default: as a session nears automatic compaction, the summary is written in the background so the next prompt doesn't wait for it. Turn it off with `compaction.background: false` ; a background summary that is never used still costs tokens. The`experimental.background_compaction` setting is removed. (#64290)
- â¢OpenCode again reads `CLAUDE.md` in any directory that has no`AGENTS.md` , and falls back to`~/.claude/CLAUDE.md` when the global`AGENTS.md` is missing. This also applies to instructions found when the agent reads nested files. Set`OPENCODE_DISABLE_CLAUDE_CODE` or`OPENCODE_DISABLE_CLAUDE_CODE_PROMPT` to skip every`CLAUDE.md` . (#64474, #64533)

## Show more (7)Show less

- â¢Added GPT 6.2 Sol and GPT 6.2 Astra, plus their fast variants, to the ChatGPT model list. (#64508)
- â¢`websearch` now returns a flat list of results, each with its`url` ,`title` ,`content` and`published` date. (#64471)
- â¢Code Mode and plugins get a simpler structured result from `read` :`text` (with`offset` ,`lineCount` and`next` ),`directory` (entry names) or`media` (a URI instead of base64). What the model sees is unchanged. (#64361)
- â¢Code Mode programs can use Node's `Buffer` for encoding and decoding bytes. (#64365)
- â¢In Code Mode, `search("issues")` now works as shorthand for`search({ query: "issues" })` . (#64347)
- â¢Code Mode tool summaries in the catalog can now be up to 500 characters instead of 120, so the model sees more of each tool's description. (#64341)
- â¢The `glob` and`grep` tools now report a malformed filename glob (for example`[` ) as an error that names the parameter to fix, instead of returning no results. (#63960)

November 17, 2026

- â¢OpenCode 1 now offers the upgrade to OpenCode 2.0. OpenCode 2 downloads in the background and installs only when you choose: Restart to upgrade, Upgrade on quit, or Later. Your sessions, projects, and settings are kept. Plugins made for OpenCode 1 need an update.
- â¢Sessions with many images no longer send oversized requests. When the images in a session's history pass 25 MiB, the oldest are replaced with a placeholder.
- â¢A provider error for a request body that is too large now starts compaction and a retry instead of failing the turn.

November 13, 2026

- â¢**Background Subagents by Default** : Subagents now return immediately, continue in the background, and report back whenever they become idle. You can keep working in the parent session while multiple subagents run concurrently. (#63382)
- â¢**Quieter Transcripts by Default** : The TUI now summarizes each run of tool calls and thoughts into one collapsible activity row, so the answer is easier to follow. To see every step again, set**Verbosity** to`medium` or`high` in settings, or set`session.verbosity` in your TUI config; an existing setting is kept. (#63207)
- â¢**Jump to an Earlier Prompt** : In the desktop app, open**Jump to message** from the command palette or press`Cmd+Alt+G` (`Ctrl+Alt+G` on Windows and Linux) to search prompts across the current session and jump directly to one. (#63585)
- â¢Subagents now always run in the background, return their session ID immediately, and report results to their parent whenever they become idle. (#63382)
- â¢Added experimental support for MCP events, behind the `mcp_events` flag. (#63254, #63261, #63269)
- â¢Shell commands now survive server handoffs and keep their output when interrupted.
- â¢Added experimental support for moving a shell command to the background when its timeout expires, behind the `shell_timeout_background` flag. With the flag on, an explicit timeout on a background command stops it. (#63024, #63703)
- â¢Local Windows shell commands now run through the process daemon.

## Show more (7)Show less

- â¢Added support for freeform tool input on compatible models.
- â¢Added LM Studio decision-model support.
- â¢Agents are now told when their session is a fork. Thanks @hasta84. (#63235)
- â¢Tool output truncation now keeps the beginning of the output and preserves a usable preview if the full output cannot be saved.
- â¢Tail reads no longer load the entire file into memory.
- â¢Providers can present a client certificate for gateways that require mutual TLS. Set `settings.tls` with`cert` ,`key` and an optional`passphrase` on the provider; values accept`{file:}` and`{env:}` like`apiKey` . (#61832)
- â¢When a subagent goes idle while its own background work is still running, its report to the parent now says it is still running, so the parent waits for the final report instead of treating the partial answer as done. (#63675)

November 10, 2026

- â¢**GitHub Copilot Auto** : Pick**Auto** in the model picker when your Copilot account has an Auto pool, and Copilot's router chooses the model for each session at the Auto discount. If routing fails, OpenCode falls back to the account's default chat model. (#62997)
- â¢**Side Panel Tab Shortcuts** : Right-click a side panel tab and choose**Close tabs to the right** .`âW` now closes side panel tabs wherever focus is,`â¥âW` or`Ctrl+Alt+W` always closes the session tab, and`ââ§T` reopens the last closed tab of either kind where it was. (#62922)
- â¢**Mark Sessions Read or Unread** : Open the menu on a tab, Home row, project row or inbox card and choose**Mark as unread** to keep its dot until you next visit. (#62889)
- â¢**MCP Logins Stay Signed In** : Remote MCP servers that rotate refresh tokens no longer sign you out when several clients refresh at once or the token endpoint fails for a moment. (#62827, #63012)
- â¢`read` accepts a negative`offset` to read from the end of a file, such as`-50` for the last 50 lines. An offset past the end returns the last lines and the file's line count instead of failing. (#62147)
- â¢When `read` can't find a file, it suggests close names: different capitalization, another extension, a small typo, or the matching path inside the project. (#62147)
- â¢Shell results end with `Exit code: N` . Commands killed by a signal show the signal name, and timeouts say how long they waited and suggest a larger timeout. (#62141)
- â¢Prompt cache warming times its pings from the model's `cache.ttl` by default (`interval: "auto"` ), or every 4 minutes without a TTL. Models whose cache outlasts the warming`duration` are no longer pinged. (#62831)

## Show more (7)Show less

- â¢When MCP servers connect, disconnect or change tools, Code Mode sends the model a short list of what changed instead of the whole tool catalog. (#62176)
- â¢GitHub Copilot session costs use the amount Copilot charged instead of the catalog estimate, and content-filter errors name the filter that blocked the response. (#62034)
- â¢GitHub Copilot picks up a new API endpoint after a plan change on its next model refresh, so you no longer have to sign in again. (#62171)
- â¢Remote MCP servers that rotate refresh tokens no longer log you out when the TUI, desktop app, background service or several projects refresh the same login at once. (#62827)
- â¢A temporary failure while refreshing an MCP OAuth token keeps your login for the next try instead of marking the server `needs_auth` . (#63012)
- â¢MCP tool calls no longer fail when the server's result doesn't match the tool's own output schema, so completed actions aren't reported as failed. (#61254)
- â¢MCP prompts stay available when refreshing a server's prompt list fails. (#62266)

November 7, 2026

- â¢**Shared MCP Connections** : Turn on pooling for an MCP server to share one connection across projects with the same setup, so you run fewer processes and start faster. (#61185)
- â¢**GUI Plugin Themes and New Slots** : GUI plugins can now add color themes, render or add actions to code blocks, and use native settings and menu components. (#61485, #61533, #61527, #61509)
- â¢**Mobile Composer Dock** : On phones and tablets, composer actions now sit in a touch dock above the keyboard with larger buttons. (#61773)
- â¢**Scripted Runs in Worktrees** : Run`opencode run -w` to start the session in a new git worktree, or`--worktree=<name>` to name it. (#60712)
- â¢**Automatically Archived Runs** : Add`--archived` to`opencode run` to keep scripted and CI sessions out of your inbox and session lists. (#61270)
- â¢Added opt-in MCP connection sharing through `pool: true` . (#61185)
- â¢Added `opencode run --archived` , which creates the session already archived so scripted and CI runs stay out of the inbox, home, and session lists. Archived sessions are also hidden from the TUI session list and skipped by`--continue` . (#61270)
- â¢Added `opencode run --worktree` (`-w` ), which starts the session in a new git worktree. Use`--worktree=<name>` to choose the worktree name. It cannot be combined with`--continue` or`--session` . (#60712)

## Show more (7)Show less

- â¢Added provider- and model-specific session warming settings, including an `interval: "auto"` mode based on cache TTL. (#60025, #60969)
- â¢Added the `websearch.use` policy action and switches for enabling all feature flags with`OPENCODE_FLAGS=all` or`experimental.all_flags` . (#61557, #61826)
- â¢Added an experimental bytecode VM for Code Mode. It is off by default; turn it on with `experimental.codemode_vm: true` or`OPENCODE_FLAGS=codemode_vm` . (#62358)
- â¢Added support for Microsoft Store installations of PowerShell 7 on Windows. (#61952)
- â¢Added Azure Messages support for Foundry Claude models. (#60934)
- â¢`opencode run` now waits for the background shell commands, subagents, and image jobs that its prompt started, then lets the model respond to their results before it exits. While it waits, it prints`waiting for N background job(s)` (a`background_wait` event with`--format json` ). There is no time limit: a job that never exits, such as a dev server or file watcher, keeps the run open until you press Ctrl+C or send SIGTERM, so set a timeout on CI jobs. (#60699, #61911)
- â¢Reworked noninteractive `opencode run` execution so arguments, cancellation, compiled WebSocket upgrades, and service shutdown complete reliably. SIGTERM now cancels the session and its background jobs and exits with code 143. (#61911, #61721, #61518, #61564)

October 31, 2026

- â¢**Worktree and Shell Mode Shortcuts** : Press`ââ¥O` on macOS or`Ctrl+Alt+O` on Windows and Linux to choose a worktree, and`ââ¥B` or`Ctrl+Alt+B` to choose its base branch.`ââ§X` or`Ctrl+Shift+X` now toggles shell mode in the composer. (#60713)
- â¢**Send Quotes From the Comment Popup** : Hold`â` or`Ctrl` in the quote popup to turn its checkmark into send, or press`âEnter` or`Ctrl+Enter` , to send just that quote and comment while leaving the rest of the composer untouched. (#60640)
- â¢**Percentage Compaction Buffer** : Set`"compaction": { "buffer": "20%" }` in`opencode.json` to keep a share of each model's context window free, so one setting fits models with small and large windows. Token counts still work. (#60647)
- â¢`compaction.buffer` accepts a percentage of the model's context window, such as`"20%"` , in addition to a token count. (#60647)
- â¢When `read` misses because a folder is missing, the error names that folder and the nearest one that exists. (#60688)
- â¢When `edit` can't find`oldString` , the error shows the file's closest lines, or says the change is already there. (#60689)
- â¢Calls to a tool named `bash` now run as`shell` instead of failing, with the same permissions. A plugin tool actually named`bash` still takes precedence. (#60687)
- â¢MCP servers that send stdio messages larger than 10 MiB no longer get disconnected, and large messages are read much faster. (#60694)

## Show more (7)Show less

- â¢Idle locations no longer restart their MCP servers right after shutting down. (#60668)
- â¢Compaction now caps its prompt at the model's context window when the input limit is larger. (#60660)
- â¢LiteLLM context window errors now trigger compaction instead of stopping the session. (#60692)
- â¢Reasoning is no longer turned into plain text when you switch models or resume an interrupted response, which caused request errors on some providers. (#60498)
- â¢The connect menu no longer lists MCP servers, which are managed in their own place. (#60675)
- â¢Added keyboard shortcuts to choose a worktree and its base branch, shown in tooltips, and a shortcut to toggle shell mode. The worktree path can be selected to copy the full directory. (#60713)
- â¢Hold `â` or`Ctrl` in the quote popup to send a quote and its comment directly. (#60640)

October 30, 2026

- â¢**Persistent Desktop Terminals** : Desktop terminals now belong to the session, survive server restarts with their scrollback, and stay synchronized with the TUI and other windows. Renamed terminals also keep their names across restarts. (#60346)
- â¢**Publish Interactive Artifacts** : Enable`"experimental": { "artifacts": true }` in`opencode.json` to let agents publish, update, and serve HTML or static artifacts through the built-in local provider or plugin-provided storage. (#60351)
- â¢**Guided Claude Agent Setup** : Select**Claude Agent** from the provider picker to copy`claude auth login` , install Claude Code, and verify the connection before continuing to model selection. (#60603)
- â¢**Choose a Base Branch Quickly** : While creating a worktree, open**Choose base branch** with`ââ¥.` on macOS or`Ctrl+Alt+.` on Windows and Linux. The command is also available in the command palette and Settings âº Shortcuts. (#60579)
- â¢Added opt-in artifacts with `artifact_create` ,`artifact_read` ,`artifact_update` , and`artifact_delete` , including a built-in local static-file provider. (#60351)
- â¢Made enabled schedules run under `opencode serve` and coordinated multiple server processes so a schedule fires only once. Schedules remain opt-in through`experimental.schedules` . (#60478)
- â¢Raised OpenCode Go limits for DeepSeek V4.1 Flash. (#60625)
- â¢Allowed user, project, and remote configuration to override Console-supplied provider names, limits, variants, settings, and headers. (#60597)

## Show more (7)Show less

- â¢Kept local OpenCode service requests out of environment-configured proxies. (#60441)
- â¢Replaced Claude Codeâs misleading `/login` instruction with`claude auth login` . Thanks @Tarquinen. (#60592)
- â¢Preserved prompt cursor styles applied by plugins when themes or unrelated prompt state change. (#60599)
- â¢Added missing right padding to edit and patch diffs. (#60614)
- â¢Extended the vertical session-tab activity pulse across the full tab width. (#60338)
- â¢Moved terminals to persistent session PTYs shared with the TUI and other windows, with restored scrollback, server-side renaming, and restart persistence. (#60346)
- â¢Added guided Claude Agent installation, sign-in, connection checking, and recovery from **Needs sign-in** . (#60603)

October 30, 2026

Highlights

- **Use** **Claude Code** **in OpenCode** : Install Claude Code, run`claude auth login` , then select**Claude Agent** in`/connect` to use your Claude Code login and models. (#58782)
- **Faster Long Sessions in Desktop** : Streaming replies and long conversations now do much less rendering work. (#60489, #60521, #60513, #60481)

## Show moreShow less

- **Desktop Stability Fixes** : Fixes side-panel tabs, repeated "Session not found" toasts, and**Open file** over HTTP. (#60430, #60436, #60459, #60462)

TUI plugins require OpenTUI 0.8

`@opencode/plugin` now requires `@opentui/core` and `@opentui/solid` 0.8.0 or later. In OpenTUI 0.8, a ScrollBox's `wrapper`, `viewport`, `content`, and scrollbars are facade objects instead of renderables, and a custom `RenderContext` must implement `clock` and `onAnimationTick`. See the OpenTUI 0.8.0 release notes. (#60385)

Managed policy: `integration.use` covers integrations only

`integration.use` now controls which integration accounts can be used, and the new `mcp.use`, `skill.use`, and `plugin.use` actions control MCP servers, skills, and plugins. Rules with the old `mcp:`, `skill:`, and `plugin:` resource prefixes are converted automatically, but a bare `integration.use` rule no longer applies to MCP servers, skills, or plugins. (#60315)

Improvements

- Added the built-in **Claude Agent** provider. Install Claude Code, run`claude auth login` , and select**Claude Agent** in`/connect` ;`OPENCODE_CLAUDE_PATH` can override the Claude executable. Thanks @Tarquinen. (#58782)
- Added Code Mode JavaScript checks to experimental schedules, including statically authorized tool calls, persistent state, and custom wake messages. (#60140)
- Added separate managed-policy actions for connecting to integrations and using integrations, MCP servers, skills, and plugins. (#60315)

Bug fixes

- Preserved plugin-provided model variants and delayed dynamic provider fallback until external SDK hooks have run. (#60097, #60380)
- Preserved permission reasons and feedback in MCP failures, and made Code Mode failures match direct tool-call failures. (#60102, #60109)
- Fixed invalid Code Mode schedule checks so creation fails immediately and existing invalid checks pause without repeated execution. (#60349)
- Removed a sessionâs persistent terminals when the session is deleted. (#60322)
- Kept nested tool progress from replacing the parent `execute` call rows. (#60480)

Improvements

- Added lazy loading for stored image references when `experimental.media_blob` is enabled. Images are downloaded only for previews, while undo, revert, fork, and prompt recall restore referenced attachments safely. (#60116)
- Added a distinct marker for the selected composer suggestion in the new designs. (#60393)

Bug fixes

- Fixed PHP syntax highlighting for code blocks without an opening `<?php` tag. (#60359)
- Preserved `#` in file-search paths unless it introduces a valid line range. (#60336)
- Kept the configured default cursor style after config reloads. (#60302)
- Matched the search cursor to the highlighted row when a selection dialog has no query. (#60310)

Improvements

- Added a Desktop spellcheck toggle under **Settings â General** and system-language dictionary selection on Windows and Linux. (#59129)
- Added `PageUp` and`PageDown` timeline paging from the composer without moving the draft cursor or focus. (#59426)
- Added independently configurable question notifications and sounds under **Settings â Notifications â Questions** . Thanks @YuriNek0. (#60358)
- Added separate project and worktree actions to the new-session control. (#60486)
- Added one-click version copying in **Settings â About** . (#52356)
- Grouped added projects in workspace settings. (#60221)
- Made question-answer text selectable. (#60372)
- Adapted the session header to narrow timeline widths. (#59512)

Bug fixes

- Side-panel tabs close again and New tab launchers respond to clicks, fixing a 2.2.2 regression. (#60430, #60436)
- **Open file** in the side panel works again in the web app over HTTP. (#60462)
- Kept question options and submit controls reachable on mobile. (#60335)
- Preserved simultaneous request notifications and inbox scroll position when opening sessions. (#60378, #60356)
- Fixed consecutive dropped files so every file mention is inserted into the composer. (#60463)
- Kept the current `@` and command-menu selection stable while results update under a stationary pointer. (#59434, #60368)
- Fixed `âW` side-panel closing and focus restoration when no closable tabs remain. (#59002, #60435)
- Kept the mobile session drawer open while a session loads. (#59427)
- Restored maximized windows on their saved display and retained renderer permissions across shared-session windows. (#60482, #60458)
- Fixed timeline growth, zoom-end scrolling, highlighting, and DOM-observer performance issues in long sessions. (#60489, #60481, #60488, #60513, #60521)
- Kept models with unknown release dates visible. Thanks @joe-ieta. (#60496)
- Fixed reasoning-capability detection for models that expose reasoning through variants or compatibility metadata. (#60457)
- Evicted deleted sessions after a `404` instead of retrying or showing repeated errors. (#60459)
- Fixed the macOS application icon and added the required folder-access description to the app bundle. (#60020, #60503)

Improvements

- Preserved partial `LLMResponse` output on`AIError.partial` when generation, object generation, or a tool loop fails. (#60477)
- Unified Effect and Promise tool declarations around `tool(...)` , with optional output schemas and executable or declaration-only tools. (#60407)
- Added typed OpenRouter `debug` ,`models` ,`plugins` ,`provider` , and`user` options to the Responses route. (#60044)
- Added `withMediaRefs` ,`mediaHashFromUri` , and`MediaRequestOptions` to the Promise client for experimental stored-media references. (#60116)

Bug fixes

- Matched the Promise AI client more closely to the Effect API and accepted typed tools in loops. (#60465, #60390)
- Kept tools whose `execute` handler can throw available to the AI client. (#60431)
- Continued polling queued media generations when providers return unknown intermediate statuses. (#59988)
- Stopped sending an incompatible `filename` alongside`file_url` on OpenAI Responses requests. (#60382)

Improvements

- Added persistent-terminal renaming to the process daemon, including capability discovery and user titles that override shell-provided titles until cleared. (#60324)

Bug fixes

- Preserved `null` values in Code Mode text encoding. Thanks @xiajiahang. (#57639)
- Tidied nested Extensions menus. (#59468)

October 29, 2026

- â¢**Image Generation by Default** : When a connected provider supports image models, agents can now use`experimental_image_generate` without additional configuration. Set its preferred model through`experimental.image_generate.model` . (#60258)
- â¢**Faster Worktree Selection** : On the new-session screen, click the current Git branch to open that projectâs âRun session inâ worktree picker directly. (#60263)
- â¢**Unified Quotes and Annotations** : Timeline quotes, terminal quotes, and browser annotations now share a compact optional-comment editor, while source highlights remain attached to local drafts. (#60164)
- â¢Enabled `experimental_image_generate` by default when an image-capable provider is connected. (#60258)
- â¢Recorded the start time of each sessionâs latest execution.
- â¢Rejected persistent terminal requests whose working directory no longer exists.
- â¢Preserved media when sessions are forked or imported.
- â¢Associated image-generation notices with the correct background job.

## Show more (7)Show less

- â¢Preserved captured attachments and their original source paths. Thanks @OpeOginni. (#60222)
- â¢Prevented middle-truncated paths from overflowing at very narrow terminal widths.
- â¢Improved Go usage-limit messages with the reset time and account page link.
- â¢Correctly typed and displayed Go and ChatGPT usage-limit errors.
- â¢Made the Git branch on the new-session screen open the current projectâs worktree picker directly. (#60263)
- â¢Unified timeline quotes, terminal quotes, and browser annotations around one optional-comment workflow. (#60164)
- â¢Restored classic question cards by default and added **Settings â Experimental â New question UI** for the redesigned keyboard-oriented dock. (#59723)

October 29, 2026

- â¢**Keep Working Toward a Goal** : Run`/goal <objective>` in the TUI or Desktop to let the session continue until a reviewer confirms the objective is met. Use`/goal pause` ,`/goal resume` , or`/goal clear` to control it. (#59437)
- â¢**Generate Images** : Enable`experimental.image_generate` to let agents create or edit images with the new`image_generate` tool. You can optionally set its default model with`experimental.image_generate.model` . (#59939, #60096)
- â¢**Inspect Background Shells** : Click a running shell in the Desktop activity pill to see its live output and stop it without leaving the menu. (#59496)
- â¢**New Default Model Picker** : Search favorites, recent models, providers, and variants from the redesigned model picker, which is now the default in Desktop. (#59997)
- â¢The fal, Black Forest Labs, Replicate, and Stability providers are removed, along with their `./providers/fal` ,`./providers/black-forest-labs` ,`./providers/replicate` , and`./providers/stability` entry points. (#59977)
- â¢Five legacy entry points are removed. Import the nested paths instead, which 2.2.0 already exports: `./providers/amazon-bedrock-mantle` â`./providers/amazon-bedrock/mantle` , and`./providers/google-vertex-chat` ,`-interactions` ,`-mistral` , and`-responses` â`./providers/google-vertex/chat` ,`/interactions` ,`/mistral` , and`/responses` . (#59506)
- â¢Each provider's `configure()` input type is now named`Config` , for example`Groq.LanguageModelOptions` â`Groq.Config` , and`model()` settings are optional when every setting is optional. (#59765)
- â¢Add `/goal <objective>` with pause, resume, and clear controls, plus automatic progress review until the objective is met. (#59437)

## Show more (7)Show less

- â¢Add opt-in scheduled prompts through `/schedule` , agent tools, and`experimental.schedules` . (#59401, #59457)
- â¢Add the opt-in `image_generate` tool for background image creation and editing, with automatic or configured model selection. (#59939, #60096)
- â¢Offer GPT Image 2 after Codex sign-in and add MiniMax and Meta Muse image generation support. (#60085, #59929, #60081)
- â¢Add Responses API support for OpenRouter, DigitalOcean, and Venice, including freeform tools. (#59991, #59963)
- â¢Improve snapshot performance by coalescing captures and avoiding repeated ignore-file work. (#59419)
- â¢Preserve more context headroom when fitting tool output into the context window. (#59934)
- â¢Retry provider throttling correctly instead of reporting it as context overflow. (#59649)

October 27, 2026

- â¢Create, install and share Desktop plugins (experimental) (#55246, #59469, #59489)
- â¢Refreshed the desktop app experience (#56898, #58865, #55838, #57962)
- â¢Add experimental image and decision generation APIs to plugins and sdk, allowing image generation or decision model (think typesafe, clef, etc) workflows. (#59190, #59380, #58758)
- â¢Plugins can now attach and update keyed session instructions with ctx.session.instructions.entry.put/list/remove. Instead of editing the system prompt and busting the prompt cache, each change is added as a small update at the next step. The entries also survive compaction and reverts, and they don't start an extra model turn. (#56550)
- â¢Restored the Todo tool in desktop via opt-in (#59092)
- â¢Add configurable prompt cache TTLs per provider/model (#59157)
- â¢Add experimental image and decision generation API endpoints. (#59190)
- â¢Add image and decision entrypoints for catalog providers. (#59380)

## Show more (7)Show less

- â¢Improve integration rebuild performance. (#59402)
- â¢Let custom model configuration set a model type. (#59388)
- â¢Let model-list API requests filter by type. (#59355)
- â¢Make external-directory permission checks use the project's filesystem. (#59089)
- â¢Reduce memory use when code search finds very large lines. (#59205)
- â¢Let plugins filter event subscriptions by type. (#58864)
- â¢Add configurable prompt-cache TTLs. (#59157)

October 22, 2026

October 16, 2026

- â¢OpenCode 1 now stays on the 1.x release line: update checks use only the npm `opencode-ai` latest version, so 1.x installs are never offered a 2.x build as a regular update.
- â¢Upgrading to OpenCode 2 remains a separate, explicit choice through the 2.0 offer in `/update` or`opencode upgrade --major` .
- â¢Fix upgrades for installs managed by yarn.
- â¢Skip 1.x updates for Homebrew Core installs, which now track OpenCode 2, and check tap and Chocolatey availability before offering an update.
- â¢The install script refuses to install 2.x versions.

October 11, 2026

- â¢Run `opencode upgrade --major` to upgrade to OpenCode 2.
- â¢Allow the OpenAI `ultrafast` service tier.
- â¢Validate PTY connection origins and tickets before checking whether a PTY exists.
- â¢Use `/update` to install OpenCode updates or upgrade to OpenCode 2; updates are no longer installed automatically.
- â¢docs(web): add Exo Free to Zen docs (#53622)
- â¢fix(stats): attribute exo usage to an unknown provider (#53621)
- â¢docs(web): remove Fledge Alpha Free from Zen docs (#53984)

October 6, 2026

- â¢Added canonical redirects and JSON and Markdown data formats for agent-readable stats.
- â¢xAI tool results now include supported images, while unsupported image formats are skipped. (@Jaaneek)
- â¢docs(web): add Fledge Alpha Free to Zen docs (#52895)
- â¢@jm0ney337:
- â¢docs(ecosystem): add opencode-supabase to ecosystem plugins (#49848)
- â¢fix(opencode): bump @ai-sdk/xai to 3.0.139 so tool-result images reach xAI (#53549)

October 1, 2026

- â¢Send namespaced session and parent-session identity headers with model requests.
- â¢Re-sign locally compiled macOS binaries so they run reliably on macOS 27+. (@ryangamerdev)
- â¢Sign macOS CLI release binaries with a Developer ID.
- â¢docs(web): correct GPT 6.1 Sol cache pricing (#52176)
- â¢@metal-huang:
- â¢fix(tui): use path.sep for plugin name extraction in /status dialog (#52328)
- â¢@ryangamerdev:
- â¢fix(opencode): ad-hoc re-sign darwin binaries after local compile (#52183)

September 28, 2026

Bugfixes

- Cloudflare AI Gateway models now honor provider response and stream timeouts. (@danlapid)
- MCP browser launch failures are now reported when the launcher exits immediately.
- Debug configuration output now redacts credentials and sensitive headers.
- Gemini thinking defaults and effort options now match the supported controls across model generations. (@markmcd)

**Thank you to 5 community contributors:**

- @dc85:
- docs(web): add Claude Opus 5.5, GPT 6 Sol, and GPT 6 Luna to Zen (#50708)
- @vglafirov:
- maint(gitlab): bump gitlab-ai-provider to 6.16.0 (#50742)
- @markmcd:
- fix(gemini): switch thinking default logic (#50841)
- @remorses:
- docs(web): link kimaki to product website (#49735)
- @danlapid:
- fix(opencode): apply provider timeouts to Cloudflare AI Gateway models (#51549)

September 22, 2026

- â¢Fixed Bedrock image attachments so they are only hoisted for Claude, Nova, and Llama 4 models.
- â¢Fixed Together AI streaming usage reporting.
- â¢docs: add DeepSeek V4.1 Flash to Zen (#49897)
- â¢feat: add Grok 4.7 to Zen and Go (#50288)

September 14, 2026

- â¢Restored ACP session model, effort, mode, and reasoning chunk boundaries when loading, resuming, or forking sessions. (@JacobNWolf)
- â¢Show remote config authentication errors during startup and exit with a failure status.
- â¢Request summarized adaptive thinking for GitHub Copilot models.
- â¢@heimoshuiyu:
- â¢feat(console): add batch workspace block endpoints (#48491)
- â¢@JacobNWolf:
- â¢fix(acp): restore session options and reasoning boundaries (#48225)
- â¢@MatthewFeroz:

## Show more (1)Show less

- â¢fix(ui): add Merge Gateway logo (#42015)

September 9, 2026

- â¢Added the Astra system prompt for GPT-6 models.
- â¢Preserved Bedrock DeepSeek model IDs, including ARN-based IDs, so they resolve correctly. (@YeEmrick)
- â¢Updated the Azure provider SDK to pick up compatibility fixes.
- â¢Updated the OpenAI provider SDK to pick up compatibility fixes.
- â¢Added reasoning effort variants for supported GitLab GPT and Claude models. (@far-ouq)
- â¢@heimoshuiyu:
- â¢fix(console): openai usage normalization and tier threshold config (#47342)
- â¢fix(opencode): add GitLab reasoning variants (#47306)

## Show more (1)Show less

- â¢fix: preserve Bedrock DeepSeek model ids (#34441)
