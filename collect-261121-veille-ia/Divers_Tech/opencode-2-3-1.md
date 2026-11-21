---
id: collect-261121-veille-ia/divers-tech/opencode-2-3-1
title: "OpenCode 2.3.1"
domain: divers-tech
role: reference
task: article
actors: []
dates: []
keywords: ["background", "code", "instead", "compaction", "opencode", "still", "timeout", "session"]
source: https://diffharness.com/changes/opencode-v2-3-1
source_anchor: ""
source_lines: [1, 128]
section: "Divers_Tech"
---
Official release notes from github.com, copied in full and unedited on Nov 18, 2026. No person approved this entry; a versioned contract checked that the copy is complete.

## Official release notes

**Highlights**

- Timed-Out Shell Commands Keep Running: A shell command still running when its timeout expires now moves to the background instead of being killed, so long builds and test runs finish and report back. A foreground `timeout: 0` now means the default 120 seconds. (#63699)
- Compaction Without the Wait: As a session nears automatic compaction, the summary is now written in the background, so your next prompt doesn't pause for it. Set `compaction.background: false` to turn it off; a background summary that ends up unused still costs tokens. (#64290)
- GPT 6.2 with ChatGPT Sign-In: GPT 6.2 Sol and GPT 6.2 Astra, plus their fast variants, now show up in the model picker when you sign in with ChatGPT. (#64508)
- Update Plugins from Settings: Open Settings › Extensions, click Check for updates, then Update on any plugin with a new version. (#64511)
- Comment on Selected Code: Selecting code in diff and file viewers now just selects it, so you can copy it, and an Add comment toolbar appears beside the selection. (#64410)

**Core**

**Improvements**

- Shell commands still running when their timeout expires now move to the background instead of being killed, in every build. The `shell_timeout_background` flag is gone; configs that still set it keep loading. A foreground`timeout: 0` now uses the default 120 second timeout instead of waiting until the command exits. (#63699)
- Background compaction is now on by default: as a session nears automatic compaction, the summary is written in the background so the next prompt doesn't wait for it. Turn it off with `compaction.background: false` ; a background summary that is never used still costs tokens. The`experimental.background_compaction` setting is removed. (#64290)
- OpenCode again reads `CLAUDE.md` in any directory that has no`AGENTS.md` , and falls back to`~/.claude/CLAUDE.md` when the global`AGENTS.md` is missing. This also applies to instructions found when the agent reads nested files. Set`OPENCODE_DISABLE_CLAUDE_CODE` or`OPENCODE_DISABLE_CLAUDE_CODE_PROMPT` to skip every`CLAUDE.md` . (#64474, #64533)
- Added GPT 6.2 Sol and GPT 6.2 Astra, plus their fast variants, to the ChatGPT model list. (#64508)
- `websearch` now returns a flat list of results, each with its`url` ,`title` ,`content` and`published` date. (#64471)
- Code Mode and plugins get a simpler structured result from `read` :`text` (with`offset` ,`lineCount` and`next` ),`directory` (entry names) or`media` (a URI instead of base64). What the model sees is unchanged. (#64361)
- Code Mode programs can use Node's `Buffer` for encoding and decoding bytes. (#64365)
- In Code Mode, `search("issues")` now works as shorthand for`search({ query: "issues" })` . (#64347)
- Code Mode tool summaries in the catalog can now be up to 500 characters instead of 120, so the model sees more of each tool's description. (#64341)
- The `glob` and`grep` tools now report a malformed filename glob (for example`[` ) as an error that names the parameter to fix, instead of returning no results. (#63960)
- Server requests that crash now return a 500 with a readable JSON `UnknownError` body instead of an empty one. Storage problems name the cause (for example "Storage is full" or "Storage is read-only"); other crashes include a`ref` you can find in the server log. (#64022)
- The shell tool description now tells the model that its session ID is in the `OPENCODE_SESSION_ID` environment variable. (#63718)
- Experimental schedules are now automations, behind the `automations` flag (the old`schedules` flag still works). An automation runs on a time, an interval, a cron or a pushed MCP event, and either sends a prompt (to the creating session, another session or a new subagent) or runs a Code Mode program that decides whether to send one. Use`/automation` or the`automation_*` tools. Active or paused plain prompt schedules carry over; schedules with a condition or run limit, and finished ones, are dropped. (#64160)

**Bug fixes**

- Fixed finished shell commands leaving zombie processes under `opencode-pty` . The process daemon now cleans each command up when it exits. Background processes started by a successful command (`cmd &` ) keep running, and a failed command's leftovers are stopped. (#64266, #64251)
- Fixed ChatGPT and OpenAI subscription logins losing their sharing-limit safeguards and context caps when a token refresh failed. (#64470)
- Fixed Claude Agent models falling back to a 200K context window when Claude Code adds or renames a model. Thanks @Tarquinen. (#64107)
- Fixed requests being rejected after switching models mid-session on OpenAI-compatible providers. Models that declare a different reasoning field (such as `reasoning_content` ) no longer receive`reasoning_details` from earlier turns. (#64243)
- Rejecting a permission request now rejects only the other pending requests from the same step, not every pending request in the session. (#64343)
- Fixed a finished subagent being reported to its parent as still running when its answer quoted the "Background work still running" heading. (#64026)
- Fixed ACP clients missing turns that a background shell woke after the original turn ended. (#64262)
- Reading a small text file now applies the default line and byte limits, so a short file with very many lines is paged instead of returned whole. (#63810)
- Paged reads that select a single blank line now return that line instead of an empty result. (#63809)
- `read` now rejects`limit: 0` with "Limit must be a positive integer". (#63808)
- Text files that start with `BM` are no longer mistaken for BMP images, and only real`GIF87a` /`GIF89a` headers count as GIFs, so these files read as text again. (#63822, #63811)
- Fixed shell output pages, grep previews, generated titles, read lines and shell output tails splitting emoji and other multi-byte characters, which showed up as `�` . (#63984, #63967)
- Shell commands using unknown ANSI-C escapes such as `$'\q'` are now read the same way Bash and Zsh read them, so permission rules can match them. (#64263)
- Session title search is now case-insensitive for non-ASCII letters, such as Cyrillic and accented text. (#64259)
- Fixed `grep` results being dropped when a matching line or path is not valid UTF-8. Thanks @Tarquinen. (#62249)
- The `@` file picker now finds files in hidden folders such as`.github/` and dotfiles such as`.env.example` when file search uses ripgrep (the default on Windows and outside git repos).`.git` and gitignored files stay excluded. Thanks @Tarquinen. (#62577)
- An unreachable well-known integration source is now skipped instead of dropping every well-known source. Thanks @Tarquinen. (#62578)
- A corrupt cached project ID is now ignored instead of being used. Thanks @Tarquinen. (#62576)
- Plugin tool renames from `tool.execute.before` now apply to calls inside Code Mode programs, and a renamed call returns the new tool's structured result. (#63633, #64333)
- In Code Mode, calling a tool that doesn't exist now returns a rejected promise, so `.catch` and`Promise.allSettled` can handle it instead of the whole script failing. (#64510)
- A Code Mode program that only logs no longer shows a stray `null` line before its logs. (#64344)

**TUI**

**Improvements**

- Permission prompts from a subagent, and from that subagent's own subagents, now appear while you view the subagent's session. Prompts also show in front of an open composer. (#64343)
- The export dialog under the new designs can save to the system temp directory as well as the project folder. Thanks @Tarquinen. (#62490)
- Removed time to first token (ttft) from the assistant message footer. Tokens per second still shows when `session.tps` is on. (#63759)

**Bug fixes**

- When another TUI sharing your tabs closes the session you're viewing, closing it here now moves to the neighbouring tab. (#64261)
- Fixed broken characters at the edges of the shell output viewer and the inline shell display when output is long. (#63967, #63984)

**Desktop**

**Improvements**

- You can check for and install plugin package updates from Settings › Extensions. Use Check for updates to look for new versions, then click Update on any plugin that has one. (#64511, #64525)
- Selecting code in diff and file viewers now just selects text, so you can copy it. An Add comment toolbar appears next to the selection to comment on those lines; line numbers and the gutter `+` still start a comment directly. (#64410)
- Cmd+F (Ctrl+F) now finds text in Markdown, Mermaid, CSV and HTML previews in the side panel, using the same find bar as the code view. (#63663)
- Disconnecting a provider or removing an account in Settings → Providers now asks for confirmation first. Disconnecting OpenCode Console says how many providers will stop working. (#64212)
- The model picker's variant flyout now follows the keyboard: it opens beside the highlighted row, `→` enters it to pick a variant or toggle Favorite, and`←` or`Escape` goes back. A model's Console budget and reset time now show in the flyout. (#64409, #64423)
- When New worktree is selected for a new session, the composer shows the base branch (`from dev ▾` ). Click it to pick another base; the choice applies immediately, and Use main checkout switches back. (#64228)
- Redesigned the folder picker for remote servers, with a path bar, up to three recent-project pills plus a More projects menu, and a search box that shows the selected folder. (#64241)
- `` ` ```latex` and ````math` code fences now render as display math once complete. (#64380)
- The new icon set is now the default, and the New icons experiment has been removed. (#64466, #64451)
- The copy button under assistant messages is now always visible. Message metadata still appears on hover. (#64478)
- Text files opened in side-panel tabs now have the same copy and download actions as other files. (#64463)
- Manage models now shows every provider as a card with a `⋯` menu, whether or not you're signed in to OpenCode Console. (#64235)
- The connect provider dialog has a cleaner layout for every sign-in step: breadcrumbs, method icons, full-width device codes, and a "Signing in from another device?" row for pasting a redirect URL. (#63829)
- Collapse long messages is no longer experimental. It is now a setting under Timeline preferences. (#64441)
- Settings now opens on your client preferences, which are listed first, pairing is grouped with Servers, and there's a Help & feedback link. (#64440, #64220)
- Automatic compactions now always show as a single quiet divider; manual and failed compactions still show the full record. (#64290)
- Pairing QR codes can carry a server name, which becomes the paired server's default name. (#64139)
- Image attachment thumbnails are larger, with a 3:2 crop, and a failed image no longer moves the message below it when retried. (#64215)
- The Hex voice tip no longer appears when Hex is already installed, its copy now says what Hex is, and tips wait an hour after first launch before the first one shows. (#64130, #64131, #63974, #63760)
- Added missing translations for every non-English language, about 920 strings each. (#64205)

**Bug fixes**

- Fixed the desktop app failing to start when `drafts.sqlite` was a symlink to or copy of the CLI's`opencode.db` . The desktop now moves that file aside and creates its own, leaving the CLI database untouched. (#64203)
- Switching session tabs with a keyboard shortcut now focuses the composer. (#64418)
- Long prompts now stay folded while a new session is being created. (#64452)
- On macOS, the browser pane stays live while you resize panels instead of showing a still image. (#64443)
- Submenus now open one at a time and swap without blinking, and stay open while you move the pointer across other items to reach them. (#63739)
- Pressing a button now dismisses its tooltip until you hover it again, and copy buttons show "Copied" for two seconds, including in the `/btw` panel. (#63834)
- Dialogs now use consistent sizes, corner radius and row styles across the command palette, session search, model picker, location picker and other lists. (#63432, #64216, #64206)
- Sessions on OpenCode Go no longer show a $0.00 cost. (#63744)
- Use main checkout now stays on one line in the branch picker, and wrapped question descriptions have better spacing. (#64450, #64446)
- Restyled the Timeline detail slider and visibility buttons in settings so they read as clickable. (#64186)
- Polished small details: animated counts no longer look faded at rest, the sidebar resize pill is hidden, the sidebar peek insets match, subagent breadcrumbs show a hover background, and screen readers name the redirect URL field after its label only. (#64155, #64219, #64192, #63742, #63885)

**SDK**

**Improvements**

- Unhandled server errors now return a JSON `UnknownError` body (`message` and`ref` ) with status 500 instead of an empty body. (#64022)
- Experimental automation endpoints replace the schedule endpoints: `/api/experimental/automation` and`client.experimental.automation.*` (`create` ,`list` ,`get` ,`pause` ,`resume` ,`delete` ).`/api/experimental/schedule` and`client.experimental.schedule.*` are removed. (#64160)
- Config adds `compaction.background` ;`experimental.background_compaction` and the`shell_timeout_background` flag are removed. (#64290, #63699)

**Bug fixes**

- Apps embedding the SDK with an in-memory database no longer import the user's legacy `auth.json` credentials. Only a database file opened by path imports them. (#63716)

**Extensions**

**Improvements**

- Each tool call inside a Code Mode program now has its own call ID in plugin hooks and traces. Its permission requests still show on the `execute` call's row. (#64324)
- Plugin `SessionIdleBackground` items can now be`{ kind: "codemode", sessionID, jobID }` , and GUI plugin`BridgeLayout` accepts an optional`clip` rectangle. (#64343, #63819)

**Bug fixes**

- The browser extension's URL bar action buttons are now spaced 6px apart. (#64214)
