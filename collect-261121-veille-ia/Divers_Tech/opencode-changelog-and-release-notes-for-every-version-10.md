---
id: collect-261121-veille-ia/divers-tech/opencode-changelog-and-release-notes-for-every-version-10
title: "OpenCode | Changelog and release notes for every version"
domain: divers-tech
role: reference
task: article
actors: []
dates: []
keywords: ["opencode", "every", "claude", "mode", "prompt", "core", "background", "instead"]
source: https://opencode.ai/changelog?area=core,sdk,extensions
source_anchor: ""
source_lines: [1, 191]
section: "Divers_Tech"
---
# Changelog

New updates and improvements to OpenCode

1. GPT-6.2 models don't cache a system prompt sent in the Responses `instructions` field, so every step re-billed it as uncached input. OpenCode now sends it as a leading developer message on every Responses route, including OpenAI (API key and ChatGPT sign-in), GitHub Copilot, Zen, Azure and OpenRouter, so it is cached from the second step on.#64542#64544### Fixes
  - Core`webfetch` structured output no longer repeats the`url` and`format` it was called with.#64534
2. Core
3. A shell command still running when its timeout expires now moves to the background instead of being killed, so long builds and test runs finish and report back. A foreground `timeout: 0` now means the default 120 seconds.#63699As a session nears automatic compaction, the summary is now written in the background, so your next prompt doesn't pause for it. Set `compaction.background: false` to turn it off; a background summary that ends up unused still costs tokens.#64290GPT 6.2 Sol and GPT 6.2 Astra, plus their fast variants, now show up in the model picker when you sign in with ChatGPT.#64508 ### Improvements 12
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
  - ExtensionsEach tool call inside a Code Mode program now has its own call ID in plugin hooks and traces. Its permission requests still show on the `execute` call's row.#64324
  - ExtensionsPlugin `SessionIdleBackground` items can now be`{ kind: "codemode", sessionID, jobID }` , and GUI plugin`BridgeLayout` accepts an optional`clip` rectangle.#64343#63819
 ### Fixes 23
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
  - SDKApps embedding the SDK with an in-memory database no longer import the user's legacy `auth.json` credentials. Only a database file opened by path imports them.#63716
  - ExtensionsThe browser extension's URL bar action buttons are now spaced 6px apart.#64214
4. CoreOpenCode again reads 
5. Sessions with many images no longer send oversized requests. When the images in a session's history pass 25 MiB, the oldest are replaced with a placeholder. ### Fixes
  - CoreA provider error for a request body that is too large now starts compaction and a retry instead of failing the turn.
6. Subagents now return immediately, continue in the background, and report back whenever they become idle. You can keep working in the parent session while multiple subagents run concurrently.#63382 See all highlights in v2.3.0### Improvements 20
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
  - SDKEvent subscriptions can now receive `experimental.mcp.event` events from MCP event subscriptions.
  - SDKModel capability schemas now advertise supported freeform tool-input formats.
  - SDKSession throughput is now measured from the first streamed token.
  - SDKSession creation documentation now clarifies the `parentID` field.
  - ExtensionsPromise-based tools can cooperatively stop through `ToolContext.signal` .
  - ExtensionsAdded the experimental `session.fork` hook.
  - ExtensionsPromise plugin cleanup failures no longer prevent subsequent plugin replacements.
  - Extensions`@opencode/plugin/effect` now exports`Tool` , so an`execute.before` hook can reject a call with`Tool.Error` without importing`@opencode/schema/tool` .#63698
 ### Fixes 12
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
7. CoreAdded experimental support for MCP events, behind the 
 See all highlights in v2.2.7**MCP Logins Stay Signed In** : Remote MCP servers that rotate refresh tokens no longer sign you out when several clients refresh at once or the token endpoint fails for a moment.#62827#63012### Breaking changesOn a Location that is still loading plugins, #62796#63040`agent.list` returns the agents loaded so far, and`integration.list` ,`usage.limits` ,`vcs.init` ,`rpc.call` and`artifact.*` no longer wait up to 5 seconds. Entries and actions that a plugin provides appear when it finishes loading, so refresh on the matching`*.updated` event. Model, provider,`generate.*` ,`voice.*` and`websearch.query` endpoints still wait.### Improvements 8
  - Core`read` accepts a negative`offset` to read from the end of a file, such as`-50` for the last 50 lines. An offset past the end returns the last lines and the file's line count instead of failing.#62147
  - CoreShell results end with `Exit code: N` . Commands killed by a signal show the signal name, and timeouts say how long they waited and suggest a larger timeout.#62141
  - CorePrompt cache warming times its pings from the model's `cache.ttl` by default (`interval: "auto"` ), or every 4 minutes without a TTL. Models whose cache outlasts the warming`duration` are no longer pinged.#62831
  - CoreWhen MCP servers connect, disconnect or change tools, Code Mode sends the model a short list of what changed instead of the whole tool catalog.#62176
  - SDK`session.generate` returns`tokens` and`cost` along with`text` .#62835
  - SDK`credential.updated` includes the`integrationID` and`credentialID` of the login that changed.#62762
  - SDKPredicate questions in `client.experimental.generate.decisions` accept optional`criteria: { true?, false? }` .#62819
  - ExtensionsACP clients can redirect a running turn with the `_session/steering` extension, advertised as`_meta.steering.supported` in`initialize` .#54394@smagnuso
 ### Fixes 24
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
9. Core
10. Turn on pooling for an MCP server to share one connection across projects with the same setup, so you run fewer processes and start faster.#61185 GUI plugins can now add color themes, render or add actions to code blocks, and use native settings and menu components.#61485#61533#61527#61509 Run `opencode run -w` to start the session in a new git worktree, or`--worktree=<name>` to name it.#60712Add `--archived` to`opencode run` to keep scripted and CI sessions out of your inbox and session lists.#61270### Improvements 7
  - CoreAdded provider- and model-specific session warming settings, including an `interval: "auto"` mode based on cache TTL.#60025#60969
  - CoreAdded the `websearch.use` policy action and switches for enabling all feature flags with`OPENCODE_FLAGS=all` or`experimental.all_flags` .#61557#61826
  - CoreAdded an experimental bytecode VM for Code Mode. It is off by default; turn it on with `experimental.codemode_vm: true` or`OPENCODE_FLAGS=codemode_vm` .#62358
  - CoreAdded support for Microsoft Store installations of PowerShell 7 on Windows.#61952
  - CoreAdded Azure Messages support for Foundry Claude models.#60934
  - SDKAdded `client.experimental.job.list()` and its Effect equivalent for listing background shell, subagent, and image jobs, with session, descendant, kind, and status filters.#60698
  - SDKServer embedders can provide a custom `fetch` implementation for OpenCode HTTP, model, web, and remote MCP traffic.#61582
 ### Fixes 13
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
  - ExtensionsIsolated plugin dialogs, Markdown renderers, terminal views, lazy chunks, and artifact viewers so failures do not take down their host surface.#62110#62123#61993
11. CoreAdded provider- and model-specific session warming settings, including an 
12. Set See all highlights in v2.2.5`"compaction": { "buffer": "20%" }` in`opencode.json` to keep a share of each model's context window free, so one setting fits models with small and large windows. Token counts still work.#60647### Improvements
  - CoreWhen `read` misses because a folder is missing, the error names that folder and the nearest one that exists.#60688
  - CoreWhen `edit` can't find`oldString` , the error shows the file's closest lines, or says the change is already there.#60689
  - CoreCalls to a tool named `bash` now run as`shell` instead of failing, with the same permissions. A plugin tool actually named`bash` still takes precedence.#60687
 ### Fixes
  - CoreMCP servers that send stdio messages larger than 10 MiB no longer get disconnected, and large messages are read much faster.#60694
  - CoreIdle locations no longer restart their MCP servers right after shutting down.#60668
  - CoreCompaction now caps its prompt at the model's context window when the input limit is larger.#60660
  - CoreLiteLLM context window errors now trigger compaction instead of stopping the session.#60692
  - CoreReasoning is no longer turned into plain text when you switch models or resume an interrupted response, which caused request errors on some providers.#60498
13. CoreWhen 
14. Enable See all highlights in v2.2.4`"experimental": { "artifacts": true }` in`opencode.json` to let agents publish, update, and serve HTML or static artifacts through the built-in local provider or plugin-provided storage.#60351### Improvements
  - CoreMade enabled schedules run under `opencode serve` and coordinated multiple server processes so a schedule fires only once. Schedules remain opt-in through`experimental.schedules` .#60478
  - CoreRaised OpenCode Go limits for DeepSeek V4.1 Flash.#60625
  - SDKExtended `client.experimental.persistentPty.update` to rename terminals or clear a user title, and exposed`userTitle` plus`persistent-pty.updated` events.#60329
 ### Fixes
  - CoreAllowed user, project, and remote configuration to override Console-supplied provider names, limits, variants, settings, and headers.#60597
  - CoreKept local OpenCode service requests out of environment-configured proxies.#60441
  - CoreReplaced Claude Codeâs misleading `/login` instruction with`claude auth login` .#60592@Tarquinen
15. CoreMade enabled schedules run under 
16. Install Claude Code, run See all highlights in v2.2.3`claude auth login` , then select**Claude Agent** in`/connect` to use your Claude Code login and models.#58782### Breaking changes
 #60315`integration.use` now controls which integration accounts can be used, and the new`mcp.use` ,`skill.use` , and`plugin.use` actions control MCP servers, skills, and plugins. Rules with the old`mcp:` ,`skill:` , and`plugin:` resource prefixes are converted automatically, but a bare`integration.use` rule no longer applies to MCP servers, skills, or plugins.### Improvements 7
  - CoreAdded Code Mode JavaScript checks to experimental schedules, including statically authorized tool calls, persistent state, and custom wake messages.#60140
  - CoreAdded separate managed-policy actions for connecting to integrations and using integrations, MCP servers, skills, and plugins.#60315
  - Terminal, SDKAdded lazy loading for stored image references when `experimental.media_blob` is enabled. Images are downloaded only for previews, while undo, revert, fork, and prompt recall restore referenced attachments safely.#60116
  - SDKPreserved partial `LLMResponse` output on`AIError.partial` when generation, object generation, or a tool loop fails.#60477
  - SDKUnified Effect and Promise tool declarations around `tool(...)` , with optional output schemas and executable or declaration-only tools.#60407
  - SDKAdded typed OpenRouter `debug` ,`models` ,`plugins` ,`provider` , and`user` options to the Responses route.#60044
  - ExtensionsAdded persistent-terminal renaming to the process daemon, including capability discovery and user titles that override shell-provided titles until cleared.#60324
 ### Fixes 11
  - CorePreserved plugin-provided model variants and delayed dynamic provider fallback until external SDK hooks have run.#60097#60380
  - CorePreserved permission reasons and feedback in MCP failures, and made Code Mode failures match direct tool-call failures.#60102#60109
  - CoreFixed invalid Code Mode schedule checks so creation fails immediately and existing invalid checks pause without repeated execution.#60349
  - CoreRemoved a sessionâs persistent terminals when the session is deleted.#60322
  - CoreKept nested tool progress from replacing the parent `execute` call rows.#60480
  - SDKMatched the Promise AI client more closely to the Effect API and accepted typed tools in loops.#60465#60390
  - SDKKept tools whose `execute` handler can throw available to the AI client.#60431
  - SDKContinued polling queued media generations when providers return unknown intermediate statuses.#59988
  - SDKStopped sending an incompatible `filename` alongside`file_url` on OpenAI Responses requests.#60382
  - ExtensionsPreserved `null` values in Code Mode text encoding.#57639@xiajiahang
  - ExtensionsTidied nested Extensions menus.#59468
17. When a connected provider supports image models, agents can now use `experimental_image_generate` without additional configuration. Set its preferred model through`experimental.image_generate.model` .#60258Timeline quotes, terminal quotes, and browser annotations now share a compact optional-comment editor, while source highlights remain attached to local drafts.#60164 ### Improvements
  - CoreRecorded the start time of each sessionâs latest execution.
  - SDKAdded `if` and`until` inputs for`client.experimental.schedule.create` , plus schedule check results and output fields.#60137
  - SDKExposed `SessionInfo.time.busy` for the latest executionâs start time.
  - SDKAdded optional source metadata to prompt file attachments.
 ### Fixes
  - CoreRejected persistent terminal requests whose working directory no longer exists.
  - CorePreserved media when sessions are forked or imported.
  - CoreAssociated image-generation notices with the correct background job.
  - ExtensionsCorrected visible flowchart edges connected through Merman subgraphs.
