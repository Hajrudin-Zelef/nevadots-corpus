---
id: collect-261121-veille-ia/divers-tech/opencode-changelog-and-release-notes-for-every-version-4
title: "OpenCode | Changelog and release notes for every version"
domain: divers-tech
role: reference
task: article
actors: []
dates: []
keywords: ["opencode", "every", "session", "claude", "prompt", "core", "instead", "code"]
source: https://dev.opencode.ai/changelog?area=core,sdk,desktop,terminal
source_anchor: ""
source_lines: [1, 344]
section: "Divers_Tech"
---
# Changelog

New updates and improvements to OpenCode

1. GPT-6.2 models don't cache a system prompt sent in the Responses `instructions` field, so every step re-billed it as uncached input. OpenCode now sends it as a leading developer message on every Responses route, including OpenAI (API key and ChatGPT sign-in), GitHub Copilot, Zen, Azure and OpenRouter, so it is cached from the second step on.#64542#64544### Fixes
  - Core`webfetch` structured output no longer repeats the`url` and`format` it was called with.#64534
2. Core
3. A shell command still running when its timeout expires now moves to the background instead of being killed, so long builds and test runs finish and report back. A foreground `timeout: 0` now means the default 120 seconds.#63699As a session nears automatic compaction, the summary is now written in the background, so your next prompt doesn't pause for it. Set `compaction.background: false` to turn it off; a background summary that ends up unused still costs tokens.#64290GPT 6.2 Sol and GPT 6.2 Astra, plus their fast variants, now show up in the model picker when you sign in with ChatGPT.#64508 Open **Settings âº Extensions** , click**Check for updates** , then**Update** on any plugin with a new version.#64511Selecting code in diff and file viewers now just selects it, so you can copy it, and an **Add comment** toolbar appears beside the selection.#64410### Improvements 31
  - CoreOpenCode again reads `CLAUDE.md` in any directory that has no`AGENTS.md` , and falls back to`~/.claude/CLAUDE.md` when the global`AGENTS.md` is missing. This also applies to instructions found when the agent reads nested files. Set`OPENCODE_DISABLE_CLAUDE_CODE` or`OPENCODE_DISABLE_CLAUDE_CODE_PROMPT` to skip every`CLAUDE.md` .#64474#64533
  - Core`websearch` now returns a flat list of results, each with its`url` ,`title` ,`content` and`published` date.#64471
  - CoreCode Mode and plugins get a simpler structured result from `read` :`text` (with`offset` ,`lineCount` and`next` ),`directory` (entry names) or`media` (a URI instead of base64). What the model sees is unchanged.#64361
  - CoreCode Mode programs can use Node's `Buffer` for encoding and decoding bytes.#64365
  - CoreIn Code Mode, `search("issues")` now works as shorthand for`search({ query: "issues" })` .#64347
  - CoreCode Mode tool summaries in the catalog can now be up to 500 characters instead of 120, so the model sees more of each tool's description.#64341
  - CoreThe `glob` and`grep` tools now report a malformed filename glob (for example`[` ) as an error that names the parameter to fix, instead of returning no results.#63960
  - Core, SDKServer requests that crash now return a 500 with a readable JSON `UnknownError` body instead of an empty one. Storage problems name the cause (for example "Storage is full" or "Storage is read-only"); other crashes include a`ref` you can find in the server log.#64022
  - CoreThe shell tool description now tells the model that its session ID is in the `OPENCODE_SESSION_ID` environment variable.#63718
  - Core, SDKExperimental schedules are now automations, behind the `automations` flag (the old`schedules` flag still works). An automation runs on a time, an interval, a cron or a pushed MCP event, and either sends a prompt (to the creating session, another session or a new subagent) or runs a Code Mode program that decides whether to send one. Use`/automation` or the`automation_*` tools. Active or paused plain prompt schedules carry over; schedules with a condition or run limit, and finished ones, are dropped.#64160
  - TerminalPermission prompts from a subagent, and from that subagent's own subagents, now appear while you view the subagent's session. Prompts also show in front of an open composer.#64343
  - TerminalThe export dialog under the new designs can save to the system temp directory as well as the project folder. Thanks @Tarquinen.#62490
  - TerminalRemoved time to first token (ttft) from the assistant message footer. Tokens per second still shows when `session.tps` is on.#63759
  - DesktopYou can check for and install plugin package updates from **Settings âº Extensions** . Use**Check for updates** to look for new versions, then click**Update** on any plugin that has one.#64511#64525
  - DesktopCmd+F (Ctrl+F) now finds text in Markdown, Mermaid, CSV and HTML previews in the side panel, using the same find bar as the code view.#63663
  - DesktopDisconnecting a provider or removing an account in **Settings â Providers** now asks for confirmation first. Disconnecting OpenCode Console says how many providers will stop working.#64212
  - DesktopThe model picker's variant flyout now follows the keyboard: it opens beside the highlighted row, `â` enters it to pick a variant or toggle Favorite, and`â` or`Escape` goes back. A model's Console budget and reset time now show in the flyout.#64409#64423
  - DesktopWhen **New worktree** is selected for a new session, the composer shows the base branch (`from dev â¾` ). Click it to pick another base; the choice applies immediately, and**Use main checkout** switches back.#64228
  - DesktopRedesigned the folder picker for remote servers, with a path bar, up to three recent-project pills plus a **More projects** menu, and a search box that shows the selected folder.#64241
  - Desktop ``latex` and ``math` code fences now render as display math once complete.#64380
  - DesktopThe new icon set is now the default, and the **New icons** experiment has been removed.#64466#64451
  - DesktopThe copy button under assistant messages is now always visible. Message metadata still appears on hover.#64478
  - DesktopText files opened in side-panel tabs now have the same copy and download actions as other files.#64463
  - Desktop**Manage models** now shows every provider as a card with a`â¯` menu, whether or not you're signed in to OpenCode Console.#64235
  - DesktopThe connect provider dialog has a cleaner layout for every sign-in step: breadcrumbs, method icons, full-width device codes, and a "Signing in from another device?" row for pasting a redirect URL.#63829
  - Desktop**Collapse long messages** is no longer experimental. It is now a setting under**Timeline** preferences.#64441
  - DesktopSettings now opens on your client preferences, which are listed first, pairing is grouped with Servers, and there's a **Help & feedback** link.#64440#64220
  - DesktopPairing QR codes can carry a server name, which becomes the paired server's default name.#64139
  - DesktopImage attachment thumbnails are larger, with a 3:2 crop, and a failed image no longer moves the message below it when retried.#64215
  - DesktopThe Hex voice tip no longer appears when Hex is already installed, its copy now says what Hex is, and tips wait an hour after first launch before the first one shows.#64130#64131#63974#63760
  - DesktopAdded missing translations for every non-English language, about 920 strings each.#64205
 ### Fixes 35
  - CoreFixed finished shell commands leaving zombie processes under `opencode-pty` . The process daemon now cleans each command up when it exits. Background processes started by a successful command (`cmd &` ) keep running, and a failed command's leftovers are stopped.#64266#64251
  - CoreFixed ChatGPT and OpenAI subscription logins losing their sharing-limit safeguards and context caps when a token refresh failed.#64470
  - CoreFixed Claude Agent models falling back to a 200K context window when Claude Code adds or renames a model. Thanks @Tarquinen.#64107
  - CoreFixed requests being rejected after switching models mid-session on OpenAI-compatible providers. Models that declare a different reasoning field (such as `reasoning_content` ) no longer receive`reasoning_details` from earlier turns.#64243
  - CoreRejecting a permission request now rejects only the other pending requests from the same step, not every pending request in the session.#64343
  - CoreFixed a finished subagent being reported to its parent as still running when its answer quoted the "Background work still running" heading.#64026
  - CoreFixed ACP clients missing turns that a background shell woke after the original turn ended.#64262
  - CoreReading a small text file now applies the default line and byte limits, so a short file with very many lines is paged instead of returned whole.#63810
  - CorePaged reads that select a single blank line now return that line instead of an empty result.#63809
  - Core`read` now rejects`limit: 0` with "Limit must be a positive integer".#63808
  - CoreText files that start with `BM` are no longer mistaken for BMP images, and only real`GIF87a` /`GIF89a` headers count as GIFs, so these files read as text again.#63822#63811
  - CoreFixed shell output pages, grep previews, generated titles, read lines and shell output tails splitting emoji and other multi-byte characters, which showed up as `ï¿½` .#63984#63967
  - CoreShell commands using unknown ANSI-C escapes such as `$'\q'` are now read the same way Bash and Zsh read them, so permission rules can match them.#64263
  - CoreSession title search is now case-insensitive for non-ASCII letters, such as Cyrillic and accented text.#64259
  - CoreFixed `grep` results being dropped when a matching line or path is not valid UTF-8. Thanks @Tarquinen.#62249
  - CoreThe `@` file picker now finds files in hidden folders such as`.github/` and dotfiles such as`.env.example` when file search uses ripgrep (the default on Windows and outside git repos).`.git` and gitignored files stay excluded. Thanks @Tarquinen.#62577
  - CoreAn unreachable well-known integration source is now skipped instead of dropping every well-known source. Thanks @Tarquinen.#62578
  - CoreA corrupt cached project ID is now ignored instead of being used. Thanks @Tarquinen.#62576
  - CorePlugin tool renames from `tool.execute.before` now apply to calls inside Code Mode programs, and a renamed call returns the new tool's structured result.#63633#64333
  - CoreIn Code Mode, calling a tool that doesn't exist now returns a rejected promise, so `.catch` and`Promise.allSettled` can handle it instead of the whole script failing.#64510
  - CoreA Code Mode program that only logs no longer shows a stray `null` line before its logs.#64344
  - TerminalWhen another TUI sharing your tabs closes the session you're viewing, closing it here now moves to the neighbouring tab.#64261
  - TerminalFixed broken characters at the edges of the shell output viewer and the inline shell display when output is long.#63967#63984
  - DesktopFixed the desktop app failing to start when `drafts.sqlite` was a symlink to or copy of the CLI's`opencode.db` . The desktop now moves that file aside and creates its own, leaving the CLI database untouched.#64203
  - DesktopSwitching session tabs with a keyboard shortcut now focuses the composer.#64418
  - DesktopLong prompts now stay folded while a new session is being created.#64452
  - DesktopOn macOS, the browser pane stays live while you resize panels instead of showing a still image.#64443
  - DesktopSubmenus now open one at a time and swap without blinking, and stay open while you move the pointer across other items to reach them.#63739
  - DesktopPressing a button now dismisses its tooltip until you hover it again, and copy buttons show "Copied" for two seconds, including in the `/btw` panel.#63834
  - DesktopDialogs now use consistent sizes, corner radius and row styles across the command palette, session search, model picker, location picker and other lists.#63432#64216#64206
  - DesktopSessions on OpenCode Go no longer show a $0.00 cost.#63744
  - Desktop**Use main checkout** now stays on one line in the branch picker, and wrapped question descriptions have better spacing.#64450#64446
  - DesktopRestyled the **Timeline detail** slider and visibility buttons in settings so they read as clickable.#64186
  - DesktopPolished small details: animated counts no longer look faded at rest, the sidebar resize pill is hidden, the sidebar peek insets match, subagent breadcrumbs show a hover background, and screen readers name the redirect URL field after its label only.#64155#64219#64192#63742#63885
  - SDKApps embedding the SDK with an in-memory database no longer import the user's legacy `auth.json` credentials. Only a database file opened by path imports them.#63716
4. CoreOpenCode again reads 
5. OpenCode 1 now offers the upgrade to OpenCode 2.0. OpenCode 2 downloads in the background and installs only when you choose: Restart to upgrade, Upgrade on quit, or Later. Your sessions, projects, and settings are kept. Plugins made for OpenCode 1 need an update. ### Fixes
  - CoreSessions with many images no longer send oversized requests. When the images in a session's history pass 25 MiB, the oldest are replaced with a placeholder.
  - CoreA provider error for a request body that is too large now starts compaction and a retry instead of failing the turn.
6. Subagents now return immediately, continue in the background, and report back whenever they become idle. You can keep working in the parent session while multiple subagents run concurrently.#63382 The TUI now summarizes each run of tool calls and thoughts into one collapsible activity row, so the answer is easier to follow. To see every step again, set **Verbosity** to`medium` or`high` in settings, or set`session.verbosity` in your TUI config; an existing setting is kept.#63207In the desktop app, open **Jump to message** from the command palette or press`Cmd+Alt+G` (`Ctrl+Alt+G` on Windows and Linux) to search prompts across the current session and jump directly to one.#63585### Improvements 28
  - CoreAdded experimental support for MCP events, behind the `mcp_events` flag.#63254#63261#63269
  - CoreShell commands now survive server handoffs and keep their output when interrupted.
  - CoreAdded experimental support for moving a shell command to the background when its timeout expires, behind the `shell_timeout_background` flag. With the flag on, an explicit timeout on a background command stops it.#63024#63703
  - CoreLocal Windows shell commands now run through the process daemon.
  - CoreAdded support for freeform tool input on compatible models.
  - CoreAdded LM Studio decision-model support.
  - CoreAgents are now told when their session is a fork. Thanks @hasta84.#63235
  - CoreTool output truncation now keeps the beginning of the output and preserves a usable preview if the full output cannot be saved.
  - CoreTail reads no longer load the entire file into memory.
  - CoreProviders can present a client certificate for gateways that require mutual TLS. Set `settings.tls` with`cert` ,`key` and an optional`passphrase` on the provider; values accept`{file:}` and`{env:}` like`apiKey` .#61832
  - CoreWhen a subagent goes idle while its own background work is still running, its report to the parent now says it is still running, so the parent waits for the final report instead of treating the partial answer as done.#63675
  - CoreCode Mode gets simpler structured results from file and skill tools: `write` and`edit` return their confirmation as a string,`patch` returns an empty result, and`skill` returns the skill's directory and content.#63616#63635#63618#63623
  - TerminalSession IDs found in messages can now be opened directly.
  - TerminalActivity tabs are clickable and open the correct view.
  - TerminalShell mode has its own prompt color under the new designs.
  - DesktopAdded the experimental **Recent** sessions sidebar, with cross-server recency, local pins, reordering, and standard session-row actions.
  - DesktopRecent sessions are limited to the last seven days and 30 rows, and can be hidden or muted.
  - DesktopAdded an experimental customizable sessions sidebar.
  - DesktopMermaid diagrams can now be maximized for zooming and panning.#59250
  - DesktopAdded `Alt+Enter` to place a follow-up first in the queue and`Mod+Enter` to steer the next queued prompt.
  - DesktopHolding the modifier key now shows tab shortcut numbers.
  - DesktopEach serverâs terminal shell can be selected from its menu.
  - DesktopAdded a free-limit panel in place of the composer when applicable.
  - DesktopSession IDs are easier to find and copy.
  - SDKEvent subscriptions can now receive `experimental.mcp.event` events from MCP event subscriptions.
  - SDKModel capability schemas now advertise supported freeform tool-input formats.
  - SDKSession throughput is now measured from the first streamed token.
  - SDKSession creation documentation now clarifies the `parentID` field.
 ### Fixes 34
  - CoreFixed case-only file renames in patches on case-insensitive filesystems.
  - CoreFixed stale thinking being retained after Claude Agent prompts or tools change.
  - CoreFixed directly driven child sessions not resuming after a restart.
  - CoreFixed plugin replacement stopping when cleanup of an earlier plugin fails.
  - CoreFixed MCP OAuth refreshes and localhost redirects when a server rewrites `127.0.0.1` .
  - CoreFixed saved API keys being omitted from local-model discovery.
  - CoreLocal model discovery (Ollama, LM Studio, vLLM) waits up to 15 seconds per request instead of 1, so remote or tunnelled endpoints list their models, and a slow endpoint no longer delays other endpoints or config reloads.#63700
  - CoreIn Code Mode, calling a namespace as if it were a tool now suggests up to three real tools under it.#63697
  - CoreFixed retryable provider streams losing completed work when interrupted.
  - CoreFixed GitHub Copilot requests using the wrong endpoint before models finish loading.
  - CoreFixed exhausted request rate limits being treated as terminal. Thanks @Kyle-j-bush.#59816
  - CoreFixed LM Studio models with a null context length and accepted decision-type models. Thanks @tomzhu1024.#56214
  - TerminalRestored pasted text when undo brings back its placeholder.
  - TerminalRestored the live draft after browsing prompt history.
  - TerminalFixed copying a paste from within the prompt selection.
  - TerminalPreserved the Mini composer caret when closing the command palette.
  - TerminalPrevented image bytes from being retained in transcript storage when image previews are disabled.
  - TerminalFixed paste-chip contrast across light and dark themes.
  - TerminalFixed plugin-provided themes updating live.
  - TerminalSession lists now open at the top rather than at the current session.
  - TerminalMini scrollback now shows the subagent card instead of obsolete launch instructions.
  - DesktopRecent sessions now keep stable recency ordering through list reloads, renames, and completed runs.
  - DesktopFixed browser panes not retrying after server plugins finish loading.
  - DesktopFixed mention suggestions being recreated while typing.
  - DesktopFixed SSH prompts failing to advance or close after connection.
  - DesktopFixed pasted provider-auth redirect URLs being rejected.
  - DesktopRestored message footers in the New timeline.
  - DesktopFixed links whose labels contain inline code.
  - DesktopFixed the desktop main process freezing while probing the login-shell environment.
  - DesktopFixed the drag region after changing layout direction.
  - DesktopFixed vertical-tab previews snapping to the wrong tab.
  - DesktopFixed the file-tree sidebar opening animation.
  - DesktopFixed pairing QR-code contrast in light themes. Thanks @ssskay.#63220
  - DesktopFixed activity-tab navigation and click handling. Thanks @OpeOginni.#60168
7. CoreAdded experimental support for MCP events, behind the 
8. Right-click a side panel tab and choose **Close tabs to the right** .`âW` now closes side panel tabs wherever focus is,`â¥âW` or`Ctrl+Alt+W` always closes the session tab, and`ââ§T` reopens the last closed tab of either kind where it was.#62922Open the menu on a tab, Home row, project row or inbox card and choose **Mark as unread** to keep its dot until you next visit.#62889### Breaking changesOn a Location that is still loading plugins, #62796#63040`agent.list` returns the agents loaded so far, and`integration.list` ,`usage.limits` ,`vcs.init` ,`rpc.call` and`artifact.*` no longer wait up to 5 seconds. Entries and actions that a plugin provides appear when it finishes loading, so refresh on the matching`*.updated` event. Model, provider,`generate.*` ,`voice.*` and`websearch.query` endpoints still wait.### Improvements 12
  - Core**MCP Logins Stay Signed In** : Remote MCP servers that rotate refresh tokens no longer sign you out when several clients refresh at once or the token endpoint fails for a moment.#62827#63012
  - Core`read` accepts a negative`offset` to read from the end of a file, such as`-50` for the last 50 lines. An offset past the end returns the last lines and the file's line count instead of failing.#62147
  - CoreShell results end with `Exit code: N` . Commands killed by a signal show the signal name, and timeouts say how long they waited and suggest a larger timeout.#62141
  - CorePrompt cache warming times its pings from the model's `cache.ttl` by default (`interval: "auto"` ), or every 4 minutes without a TTL. Models whose cache outlasts the warming`duration` are no longer pinged.#62831
  - CoreWhen MCP servers connect, disconnect or change tools, Code Mode sends the model a short list of what changed instead of the whole tool catalog.#62176
  - TerminalThe update and confirm dialogs have buttons you can move between with `â` /`â` and`Tab` /`Shift+Tab` .#62906
  - TerminalThe model picker no longer has a **Usage** action; use`/usage` instead.#62927
  - DesktopThe answer you pick in the question dock shows as a raised card.#62896
  - DesktopInline rename and custom-answer fields no longer show a focus border.#62919
  - SDK`session.generate` returns`tokens` and`cost` along with`text` .#62835
  - SDK`credential.updated` includes the`integrationID` and`credentialID` of the login that changed.#62762
  - SDKPredicate questions in `client.experimental.generate.decisions` accept optional`criteria: { true?, false? }` .#62819
 ### Fixes 34
  - CoreGitHub Copilot session costs use the amount Copilot charged instead of the catalog estimate, and content-filter errors name the filter that blocked the response.#62034
  - CoreGitHub Copilot picks up a new API endpoint after a plan change on its next model refresh, so you no longer have to sign in again.#62171
  - CoreRemote MCP servers that rotate refresh tokens no longer log you out when the TUI, desktop app, background service or several projects refresh the same login at once.#62827
  - CoreA temporary failure while refreshing an MCP OAuth token keeps your login for the next try instead of marking the server `needs_auth` .#63012
  - CoreMCP tool calls no longer fail when the server's result doesn't match the tool's own output schema, so completed actions aren't reported as failed.#61254
  - CoreMCP prompts stay available when refreshing a server's prompt list fails.#62266
  - CoreWith `background_compaction` on, a background summary no longer stops a running Claude Agent session mid tool call.#63025
  - CoreAfter `opencode auth logout` on a`.well-known` integration, its remote providers and policies stop applying until you log back in.#62760
  - CoreOpenCode Console workspace policies still apply when Console's instructions or skills fail to load.#62702
  - CoreCloudflare AI Gateway providers set up with only `gatewayApiKey` now send that key.#62574
  - CoreA v1 model override that sets only some capabilities no longer resets the others, so catalog capabilities like PDF input are kept.#62573
  - CoreA v1 provider config that sets both `api` and`options.baseURL` keeps`options.baseURL` .#62570
  - CoreLegacy model references with an inline variant, such as `"openai/gpt-5#high"` , keep the variant.#62254
  - CoreA fresh install imports saved provider credentials from the legacy `auth.json` , and skips an invalid one with a warning instead of stopping setup.#62568
  - CoreCache warming stops for a session when a ping rebuilds the cache instead of reading it, counts its interval from when a request was sent, and no longer pings during compaction or after the session is deleted.#62926#62941#62972
  - CoreTokens and cost from `session.generate` requests, including cache warming pings, count toward the session's totals.#63006
  - CoreUnder zsh, a glob that matches nothing, like `rm *.tmp` , is passed through as bash does instead of stopping the command.#62141
  - CoreOn Windows, multi-line commands no longer lose everything after the first line with PowerShell 7 from the Microsoft Store.#62522
  - Core`read` no longer cuts an emoji in half when it shortens a long line.#62575
  - CoreGenerated session titles no longer include the model's commentary.#62267@shanebishop1
  - CoreMarkdown agents whose `description` starts with a bracketed tag, like`[Beta] Reviews code` , now load.#62250
  - CoreMarkdown agent and command files that fail to load log a warning with the file path and frontmatter error.#62464#62251
  - CoreMarkdown agent discovery skips `node_modules` .#62571
  - CoreFailed plugin installs from git show git's own error instead of "An unknown git error occurred".#62261
  - TerminalSession exports are saved in the current working directory instead of the system temp folder.#62253
  - TerminalTyping a child session's exact ID in the open-session menu lists that session.#62262
  - TerminalThe TUI follows the terminal's current light or dark mode instead of a cached palette.#62248
  - DesktopOn Windows and Linux, `Ctrl+Shift+R` toggles the review panel again instead of reloading the window.#62567
  - DesktopClosing the last side panel tab slides the panel shut.#62758
  - DesktopSelects, menus, tooltips and toasts opened from a dialog are reachable by screen readers.#62913
  - DesktopAn error appears when a path can't be opened in its default app.#62572
  - DesktopCut, Copy, Paste and Select All in the right-click menu follow the app language.#62252
  - DesktopUkrainian tool titles use nouns.#62263
  - DesktopScrolling titlebar search results no longer jumps back when late results arrive, the inbox no longer announces already-running sessions as working on load, and the quote toolbar comes back after `Escape` .#62597
9. Core
10. Turn on pooling for an MCP server to share one connection across projects with the same setup, so you run fewer processes and start faster.#61185 On phones and tablets, composer actions now sit in a touch dock above the keyboard with larger buttons.#61773 Run `opencode run -w` to start the session in a new git worktree, or`--worktree=<name>` to name it.#60712Add `--archived` to`opencode run` to keep scripted and CI sessions out of your inbox and session lists.#61270### Improvements 14
  - CoreAdded provider- and model-specific session warming settings, including an `interval: "auto"` mode based on cache TTL.#60025#60969
  - CoreAdded the `websearch.use` policy action and switches for enabling all feature flags with`OPENCODE_FLAGS=all` or`experimental.all_flags` .#61557#61826
  - CoreAdded an experimental bytecode VM for Code Mode. It is off by default; turn it on with `experimental.codemode_vm: true` or`OPENCODE_FLAGS=codemode_vm` .#62358
  - CoreAdded support for Microsoft Store installations of PowerShell 7 on Windows.#61952
  - CoreAdded Azure Messages support for Foundry Claude models.#60934
  - TerminalAdded **Scroll on send** in Settings. Choose**Keep** (`session.submit_scroll: "keep"` ) to retain your reading position after submitting a prompt.#61460
  - DesktopAdded close-other-tabs and close-tabs-after actions to the tab menu.#61631
  - DesktopAdded unread-session grouping in Inbox and failed-run indicators on tabs.#61691#62077
  - DesktopMCP sign-in now accepts a pasted redirect URL when automatic redirection is unavailable.#61706
  - DesktopMarkdown file previews now display frontmatter as a properties list.#60333
  - DesktopThe command palette shows a date instead of "Xd ago" for sessions 7 or more days old. Hover the time to see the full date and time.#62314
  - DesktopPrompt history now keeps up to 200 prompts. Past the newest 100, only prompts without attachments are kept.#62313
  - SDKAdded `client.experimental.job.list()` and its Effect equivalent for listing background shell, subagent, and image jobs, with session, descendant, kind, and status filters.#60698
  - SDKServer embedders can provide a custom `fetch` implementation for OpenCode HTTP, model, web, and remote MCP traffic.#61582
 ### Fixes 32
  - Core`opencode run` now waits for the background shell commands, subagents, and image jobs that its prompt started, then lets the model respond to their results before it exits. While it waits, it prints`waiting for N background job(s)` (a`background_wait` event with`--format json` ). There is no time limit: a job that never exits, such as a dev server or file watcher, keeps the run open until you press Ctrl+C or send SIGTERM, so set a timeout on CI jobs.#60699#61911
  - CoreReworked noninteractive `opencode run` execution so arguments, cancellation, compiled WebSocket upgrades, and service shutdown complete reliably. SIGTERM now cancels the session and its background jobs and exits with code 143.#61911#61721#61518#61564
  - CoreServe file reads and media from the server API as sandboxed downloads, and refuse cross-site browser navigations and requests from disallowed origins to `/api/*` , so a file in your project cannot run scripts on the server's origin.#62422
  - CoreWebfetch and Code Mode `fetch` now reject URLs that contain credentials and ignore the`#fragment` when matching permission rules, and Code Mode`fetch` checks every redirect against your webfetch rules.#60502#61701#62373
  - CoreAsk for `external_directory` permission before`session_move` leaves the current project.#62204
  - CoreWait up to 2 seconds for MCP servers that are still connecting before a model call, so tools from servers that connect quickly reach the first prompt and `opencode run` . Also expand`~` in local MCP working directories, handle legacy stdio servers, and send`logo_uri` during dynamic registration.#60490#61496#61817#59031
  - CoreTime out OpenCode Console config requests after 5 seconds, so startup no longer hangs when the Console accepts the connection but never answers.#62404
  - CoreRetry provider requests while their host is unreachable and recover prematurely ended Cohere streams.#61880#61184
  - CoreKeep legacy custom providers whose models set only part of `limit` , such as only`limit.context` , instead of dropping the whole provider without an error. Thanks @Tarquinen.#62240
  - Core`opencode acp` now removes a session that a client creates with`session/new` and closes without prompting, so IDE integrations no longer leave empty sessions behind.#62189
  - CorePrevent repeated zsh `q` flags from freezing shell permission scanning. Thanks @yangguan-xue.#60791
  - CoreAllow npm upgrade install scripts again. Thanks @renatomoselli.#51554
  - TerminalAllow Ctrl+Z to suspend OpenCode while a form or dialog is open.#62118
  - TerminalKeep focus on an open dialog when a question arrives and contain plugin dialogs that throw during rendering.#60365#62110
  - TerminalShow an error when Ctrl+V cannot read the clipboard. Thanks @jimmyallinai.#61094
  - TerminalPrevent session titles containing newlines from crashing the TUI. Thanks @vishalshekhar.#57523
  - TerminalImproved message spacing and prevented older history from flashing while scrolling.#62104#59236
  - TerminalJump to the first or last message with Home, End, and Ctrl+G in subagent sessions. Thanks @ricky9408.#60817
  - TerminalKeypad Home, End, Page Up, and Page Down now navigate the transcript like the main keys in terminals that use the kitty keyboard protocol. Thanks @argszero.#59770
  - Terminal`opencode mini` now uses the agent colors from your config. Thanks @ferdinandyb.#62067
  - TerminalReturn to the `/mcp` list after signing in to an MCP server, with that server still selected. Thanks @Tarquinen.#62245
  - TerminalAllow favoriting models with Ctrl+F in `/models` when they come from a provider declared in config or an add-on instead of a saved connection. Thanks @Tarquinen.#62244
  - DesktopImproved prompt submission and timeline behavior during loading, reconnects, retries, scrolling, compaction, and optimistic delivery.#62025#62037#61932#62151
  - DesktopSurface loading and offline failures in review previews, file search, details, Word pages, and review diffs instead of showing empty or clean results.#62114#61862
  - DesktopFixed provider connection, MCP sign-in, settings, and plugin loading flows that could become stuck after failures.#62102#61917#61580
  - DesktopPreserve drafts, focus, caret position, queued prompts, and selected terminal tabs across failed or concurrent transitions.#61579#61969#61852
  - DesktopRestart plugins after failed update handoffs and recover the renderer, storage, event stream, and background-service connection from transient failures.#61649#61908#61804#61748
  - DesktopCorrected V2 file labels and mention syntax. Thanks @velmet and @cheapredwine.#55677#60737
  - DesktopAttach only the `@mentions` in a line comment that match an existing file or folder, so a comment such as`Check @here` no longer fails the send. Render partial diffs in the review pane when it shows all lines, instead of a blank pane. Thanks @Tarquinen.#62242#62241
  - DesktopKeep pasted images sendable after the clipboard tool deletes its temporary file. Thanks @lfscqk67.#62057
  - DesktopWhile an input method is composing text, Escape, Enter, and Tab go to the input method instead of stopping the agent or picking a suggestion.#62318
  - DesktopFixed padding in the WSL distribution picker, the position of worktree and branch labels in Home search results, and the selected ring on the Interface layout setting. Thanks @inuyume.#53676#62317#62309
11. CoreAdded provider- and model-specific session warming settings, including an 
12. Press `ââ¥O` on macOS or`Ctrl+Alt+O` on Windows and Linux to choose a worktree, and`ââ¥B` or`Ctrl+Alt+B` to choose its base branch.`ââ§X` or`Ctrl+Shift+X` now toggles shell mode in the composer.#60713Hold `â` or`Ctrl` in the quote popup to turn its checkmark into send, or press`âEnter` or`Ctrl+Enter` , to send just that quote and comment while leaving the rest of the composer untouched.#60640Set `"compaction": { "buffer": "20%" }` in`opencode.json` to keep a share of each model's context window free, so one setting fits models with small and large windows. Token counts still work.#60647### Improvements
  - CoreWhen `read` misses because a folder is missing, the error names that folder and the nearest one that exists.#60688
  - CoreWhen `edit` can't find`oldString` , the error shows the file's closest lines, or says the change is already there.#60689
  - CoreCalls to a tool named `bash` now run as`shell` instead of failing, with the same permissions. A plugin tool actually named`bash` still takes precedence.#60687
  - DesktopAdded flat input surfaces and a compact input size.#60598
 ### Fixes 7
  - CoreMCP servers that send stdio messages larger than 10 MiB no longer get disconnected, and large messages are read much faster.#60694
  - CoreIdle locations no longer restart their MCP servers right after shutting down.#60668
  - CoreCompaction now caps its prompt at the model's context window when the input limit is larger.#60660
  - CoreLiteLLM context window errors now trigger compaction instead of stopping the session.#60692
  - CoreReasoning is no longer turned into plain text when you switch models or resume an interrupted response, which caused request errors on some providers.#60498
  - Terminal, DesktopThe connect menu no longer lists MCP servers, which are managed in their own place.#60675
  - DesktopAdded the missing top inset to the search field in the recently closed menu.#60652
13. CoreWhen 
14. Desktop terminals now belong to the session, survive server restarts with their scrollback, and stay synchronized with the TUI and other windows. Renamed terminals also keep their names across restarts.#60346 Enable `"experimental": { "artifacts": true }` in`opencode.json` to let agents publish, update, and serve HTML or static artifacts through the built-in local provider or plugin-provided storage.#60351Select **Claude Agent** from the provider picker to copy`claude auth login` , install Claude Code, and verify the connection before continuing to model selection.#60603While creating a worktree, open **Choose base branch** with`ââ¥.` on macOS or`Ctrl+Alt+.` on Windows and Linux. The command is also available in the command palette and Settings âº Shortcuts.#60579### Improvements
  - CoreMade enabled schedules run under `opencode serve` and coordinated multiple server processes so a schedule fires only once. Schedules remain opt-in through`experimental.schedules` .#60478
  - CoreRaised OpenCode Go limits for DeepSeek V4.1 Flash.#60625
  - DesktopSimplified the session location popover and made long paths easier to read.#60492
  - SDKExtended `client.experimental.persistentPty.update` to rename terminals or clear a user title, and exposed`userTitle` plus`persistent-pty.updated` events.#60329
 ### Fixes 10
  - CoreAllowed user, project, and remote configuration to override Console-supplied provider names, limits, variants, settings, and headers.#60597
  - CoreKept local OpenCode service requests out of environment-configured proxies.#60441
  - CoreReplaced Claude Codeâs misleading `/login` instruction with`claude auth login` .#60592@Tarquinen
  - TerminalPreserved prompt cursor styles applied by plugins when themes or unrelated prompt state change.#60599
  - TerminalAdded missing right padding to edit and patch diffs.#60614
  - TerminalExtended the vertical session-tab activity pulse across the full tab width.#60338
  - DesktopKept terminal tab clicks and rename fields attached to the intended tab.#60645
  - DesktopListed matching commands before sessions in command-palette search.#60616
  - DesktopRestored copying user messages from an open actions menu.#60576
  - DesktopReduced repeated work and allocations while streaming Markdown, especially in long sessions.#60549#60562#60577
15. CoreMade enabled schedules run under 
16. Install Claude Code, run `claude auth login` , then select**Claude Agent** in`/connect` to use your Claude Code login and models.#58782Streaming replies and long conversations now do much less rendering work.#60489#60521#60513#60481 Fixes side-panel tabs, repeated "Session not found" toasts, and **Open file** over HTTP.#60430#60436#60459#60462### Breaking changes
 #60385`@opencode/plugin` now requires`@opentui/core` and`@opentui/solid` 0.8.0 or later. In OpenTUI 0.8, a ScrollBox's`wrapper` ,`viewport` ,`content` , and scrollbars are facade objects instead of renderables, and a custom`RenderContext` must implement`clock` and`onAnimationTick` . See the OpenTUI 0.8.0 release notes.
 #60315`integration.use` now controls which integration accounts can be used, and the new`mcp.use` ,`skill.use` , and`plugin.use` actions control MCP servers, skills, and plugins. Rules with the old`mcp:` ,`skill:` , and`plugin:` resource prefixes are converted automatically, but a bare`integration.use` rule no longer applies to MCP servers, skills, or plugins.### Improvements 15
  - CoreAdded Code Mode JavaScript checks to experimental schedules, including statically authorized tool calls, persistent state, and custom wake messages.#60140
  - CoreAdded separate managed-policy actions for connecting to integrations and using integrations, MCP servers, skills, and plugins.#60315
  - Terminal, SDKAdded lazy loading for stored image references when `experimental.media_blob` is enabled. Images are downloaded only for previews, while undo, revert, fork, and prompt recall restore referenced attachments safely.#60116
  - TerminalAdded a distinct marker for the selected composer suggestion in the new designs.#60393
  - DesktopAdded a Desktop spellcheck toggle under **Settings â General** and system-language dictionary selection on Windows and Linux.#59129
  - DesktopAdded `PageUp` and`PageDown` timeline paging from the composer without moving the draft cursor or focus.#59426
  - DesktopAdded independently configurable question notifications and sounds under **Settings â Notifications â Questions** .#60358@YuriNek0
  - DesktopAdded separate project and worktree actions to the new-session control.#60486
  - DesktopAdded one-click version copying in **Settings â About** .#52356
  - DesktopGrouped added projects in workspace settings.#60221
  - DesktopMade question-answer text selectable.#60372
  - DesktopAdapted the session header to narrow timeline widths.#59512
  - SDKPreserved partial `LLMResponse` output on`AIError.partial` when generation, object generation, or a tool loop fails.#60477
  - SDKUnified Effect and Promise tool declarations around `tool(...)` , with optional output schemas and executable or declaration-only tools.#60407
  - SDKAdded typed OpenRouter `debug` ,`models` ,`plugins` ,`provider` , and`user` options to the Responses route.#60044
 ### Fixes 24
  - CorePreserved plugin-provided model variants and delayed dynamic provider fallback until external SDK hooks have run.#60097#60380
  - CorePreserved permission reasons and feedback in MCP failures, and made Code Mode failures match direct tool-call failures.#60102#60109
  - CoreFixed invalid Code Mode schedule checks so creation fails immediately and existing invalid checks pause without repeated execution.#60349
  - CoreRemoved a sessionâs persistent terminals when the session is deleted.#60322
  - CoreKept nested tool progress from replacing the parent `execute` call rows.#60480
  - TerminalFixed PHP syntax highlighting for code blocks without an opening `<?php` tag.#60359
  - TerminalPreserved `#` in file-search paths unless it introduces a valid line range.#60336
  - TerminalKept the configured default cursor style after config reloads.#60302
  - TerminalMatched the search cursor to the highlighted row when a selection dialog has no query.#60310
  - DesktopKept question options and submit controls reachable on mobile.#60335
  - DesktopPreserved simultaneous request notifications and inbox scroll position when opening sessions.#60378#60356
  - DesktopFixed consecutive dropped files so every file mention is inserted into the composer.#60463
  - DesktopKept the current `@` and command-menu selection stable while results update under a stationary pointer.#59434#60368
  - DesktopFixed `âW` side-panel closing and focus restoration when no closable tabs remain.#59002#60435
  - DesktopKept the mobile session drawer open while a session loads.#59427
  - DesktopRestored maximized windows on their saved display and retained renderer permissions across shared-session windows.#60482#60458
  - DesktopFixed timeline growth, zoom-end scrolling, highlighting, and DOM-observer performance issues in long sessions.#60489#60481#60488#60513#60521
  - DesktopKept models with unknown release dates visible.#60496@joe-ieta
  - DesktopFixed reasoning-capability detection for models that expose reasoning through variants or compatibility metadata.#60457
  - DesktopFixed the macOS application icon and added the required folder-access description to the app bundle.#60020#60503
  - SDKMatched the Promise AI client more closely to the Effect API and accepted typed tools in loops.#60465#60390
  - SDKKept tools whose `execute` handler can throw available to the AI client.#60431
  - SDKContinued polling queued media generations when providers return unknown intermediate statuses.#59988
  - SDKStopped sending an incompatible `filename` alongside`file_url` on OpenAI Responses requests.#60382
17. When a connected provider supports image models, agents can now use `experimental_image_generate` without additional configuration. Set its preferred model through`experimental.image_generate.model` .#60258On the new-session screen, click the current Git branch to open that projectâs âRun session inâ worktree picker directly.#60263 Timeline quotes, terminal quotes, and browser annotations now share a compact optional-comment editor, while source highlights remain attached to local drafts.#60164 ### Improvements 9
  - CoreRecorded the start time of each sessionâs latest execution.
  - DesktopRestored classic question cards by default and added **Settings â Experimental â New question UI** for the redesigned keyboard-oriented dock.#59723
  - DesktopAdded a side-panel tab menu and kept close buttons visible when tabs overflow.
  - DesktopMoved user-message actions into a compact menu.
  - DesktopRedesigned the active todo pill to show the current task and concurrent work more clearly.
  - DesktopAdded consistent styling for searchable dialogs and composer pills.
  - SDKAdded `if` and`until` inputs for`client.experimental.schedule.create` , plus schedule check results and output fields.#60137
  - SDKExposed `SessionInfo.time.busy` for the latest executionâs start time.
  - SDKAdded optional source metadata to prompt file attachments.
 ### Fixes 17
  - CoreRejected persistent terminal requests whose working directory no longer exists.
  - CorePreserved media when sessions are forked or imported.
  - CoreAssociated image-generation notices with the correct background job.
  - TerminalPreserved captured attachments and their original source paths.#60222@OpeOginni
  - TerminalPrevented middle-truncated paths from overflowing at very narrow terminal widths.
  - TerminalImproved Go usage-limit messages with the reset time and account page link.
  - TerminalCorrectly typed and displayed Go and ChatGPT usage-limit errors.
  - DesktopStopped queued prompts from dropping frames as their content grows.
  - DesktopCounted queued quotes and browser annotations as attachments.
  - DesktopReported failures to persist composer attachments through the normal error UI.
  - DesktopMade Markdown fragment links scroll to the referenced heading.
  - DesktopKept each tabâs timeline state when switching directories.
  - DesktopOrdered the inboxâs **Working** shelf by execution start time.
  - DesktopFixed side-panel launchers and automatic closing when the final tab is removed.
  - DesktopFocused the matching navigation arrow while reviewing changes.
  - DesktopPreserved pending model labels only when a queued prompt actually changes models.
  - DesktopAdded reset details, an account link, and a composer tray for Go usage limits.
