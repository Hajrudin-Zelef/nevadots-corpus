---
id: collect-261121-veille-ia/gpt-openai/codex-cli-changelog-november-2026
title: "Codex CLI Changelog (November 2026)"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: ["2026-11-18"]
keywords: ["aadded", "november", "codex", "afixed", "terminal", "permission", "show", "model"]
source: https://www.gradually.ai/en/changelogs/codex-cli/
source_anchor: ""
source_lines: [1, 572]
section: "GPT_OpenAI"
---
## Latest Releases (148)

November 20, 2026

- â¢Added GPT-6.2 Sol and Astra to model catalogs, with Sol as the default catalog selection. (#62106)
- â¢Enabled the TUI workspace by default, with searchable document previews, image previews, and subagent transcripts alongside the main conversation. (#61352, #61888, #61889, #61919, #62157)
- â¢Added configurable undo and redo shortcuts to the non-Vim prompt composer, including restoration of attachments and large pastes. (#61954)
- â¢Added `codex exec --remote <ADDR>` to start or resume noninteractive sessions on an existing app server. (#61707)
- â¢Added `/rc` and`/remote` to manage local remote control and display device pairing codes from the TUI. (#62121)
- â¢Added custom MCP status line selection and authoring through `/statusline` when`tui_extensions_statusline` is enabled. (#61979, #61988, #62037)
- â¢Preserved drafts, queued follow-ups, and interrupted input with their original threads when switching or resuming sessions. (#61216)
- â¢Kept permission profiles intact when changing working directories. (#61195)

## Show more (6)Show less

- â¢Prevented late completion events from cancelling approvals for a newer turn. (#61801)
- â¢Preserved earlier edits in aggregate diffs when steering an active turn. (#61820)
- â¢Fixed scrolling that inserted stray characters, composer caret jumps, and redraws using stale terminal dimensions. (#62139, #62174, #62175)
- â¢Fixed enterprise MCP sign-in for identity providers that issue ID tokens without refresh tokens. (#62129)
- â¢Updated source builds and release tooling to Rust 1.99.1. (#61205)
- â¢Updated XML dependencies and removed obsolete security advisory exceptions. (#62068)

November 18, 2026

- â¢Added WebRTC negotiation for realtime voice calls in the TUI. (#61172)
- â¢Added automatically refreshed authentication headers for OpenTelemetry HTTP exporters. (#61028)
- â¢Added experimental support for using identically named MCP servers across environments. (#60905)
- â¢Oversized WebSocket requests now fall back to HTTP without unnecessary retries. (#60840, #60841)
- â¢Fixed MCP OAuth credential races during cancellation, login, and logout. (#60849, #60981)
- â¢Improved automatic reconnection for Cloud conversations and preserved active recovery turns when clients reconnect. (#60969, #61014)
- â¢Fixed Windows file replacement, sandbox permission preservation, and runtime registration recovery. (#60872, #60943, #60944)
- â¢Fixed permission updates and child threads losing existing sandbox restrictions. (#60936, #61134)

## Show more (2)Show less

- â¢Removed duplicate file search results and fixed thread search snippets for queries containing whitespace. (#60845, #60846)
- â¢Updated sandbox documentation to explain permission profiles and filesystem rules. (#61133)

November 13, 2026

- â¢The opt-in terminal workspace supports multiple side-chat tabs. Switch between them with the keyboard or mouse, and reopen saved local side chats after restarting Codex. (#59784, #60152, #60181)
- â¢Search older tasks from the agent command center by name, prompt preview, or working directory. (#60286)
- â¢Turn on **Collapse turn details** in the full-screen transcript to fold completed activity beneath each prompt while keeping answers visible and tool output searchable. (#60554, #60555, #60556, #60557)
- â¢Add or edit custom model providers from the terminal. With custom gateway onboarding enabled, you can create a provider during sign-in and use a device code for supported gateways over SSH or without a local browser. (#59779, #60009, #60186, #60486, #60662)
- â¢Press `Ctrl+S` in the terminal report editor to save a diagnostic ZIP locally, even when uploads are disabled. (#60542)
- â¢With the opt-in Cloud connection on macOS and Linux, send images, choose a model and reasoning effort, and archive or restore Cloud conversations from the terminal. (#59828, #60061, #60635, #60651)
- â¢`codex exec` keeps project configuration and MCP servers disabled in untrusted directories, even with`--sandbox workspace-write` . Use`--trust` with`codex exec` or`codex review` to explicitly save project trust. (#60041)
- â¢**Yes, clear context and implement** now hands the full approved plan to the fresh session and keeps the current session open if the handoff fails. (#60763)

## Show more (7)Show less

- â¢Fixed Windows sandbox handling for quoted `cmd.exe` paths and for PowerShell sessions whose working directory has an unreadable parent. (#60624, #60647)
- â¢Pasted images on macOS retain their intended orientation and use native PNG or JPEG clipboard data when available. (#60455)
- â¢The terminal no longer stays busy waiting for MCP servers that a shared app server did not report. (#60280, #60622)
- â¢Interrupted turns now close cleanly, and delayed aborts or file approval cancellations no longer interrupt a later turn. (#59910, #60700)
- â¢Updated the `openai-docs` skill to focus on Codex and ChatGPT Work questions and first-time API setup, with dedicated guidance for configuring Codex across a team. (#59944, #59949)
- â¢Documented how app-server clients can list models or authorize a gateway for a configured provider without changing the default provider. (#59776, #59819)
- â¢Updated bundled SQLite to 3.54 to pick up write-ahead log fixes. (#60432)

November 13, 2026

- â¢Browse model providers under Sessions â Model providers and choose the default for new chats. (#59541, #59722)
- â¢Enable `features.tui_workspace` to view a side conversation beside the main chat. Drag the divider to resize the panes or press`leader z` to zoom the focused pane. (#58992, #59527, #59595)
- â¢Attach recordings when using an audio-capable model. Codex skips confirmed rejected recordings on later turns and uses clip duration to keep more short recordings in context. (#59234, #59268)
- â¢Enable `features.image_generation_animation` to generate or edit animated GIFs with optional frame count and timing controls. (#59073)
- â¢Fullscreen Codex displays image previews and pets in tmux on Ghostty and Kitty with `allow-passthrough all` . (#59579, #59580)
- â¢Set `tui.notification_method = "osc777"` for terminal notifications. Approval notifications show longer command previews and distinguish terminal input from network access requests. (#59256)
- â¢Local TUI history and resume pickers filter newly created threads by the signed-in ChatGPT account. (#58918)
- â¢Open and resumed chats use their saved model provider after you change the default for new chats. (#59503)

## Show more (7)Show less

- â¢The TUI renders replies interrupted by tool calls as a single Markdown message and restores reasoning summaries when replaying a conversation. (#59698, #59700)
- â¢Steering messages reach running turns when sent during the final transcript save or after switching back to a long-running chat. (#58877, #58998)
- â¢Fuzzy file search gives filename matches more weight so matching parent directories are less likely to crowd them out. (#59709)
- â¢Idle local MCP servers on Windows leave filesystem I/O available to the rest of Codex. (#59726)
- â¢The app-server reference covers listing model providers, creating custom gateways, and updating gateway, Bedrock, and GCP settings. (#58845, #58876)
- â¢Codex warns when `features.multi_agent` is explicitly configured and points to`agents.enabled` for disabling subagent tools. (#59479)
- â¢Contributors can run Bazel CI jobs locally without BuildBuddy credentials. (#59055)

November 6, 2026

- â¢Open the agent command center with `codex agents --no-daemon` . The command center and session picker now mark unread results (#58238, #58239, #58263, #58264).
- â¢Switch between connected backends from the terminal session picker and keep your drafts when switching (#58280, #58457).
- â¢Preview local image attachments while composing on supported terminals, and open viewed local images from clickable transcript paths (#58261, #58446).
- â¢Find API key and cloud provider options under âSign in another way.â Enable `gcp_setup_wizard` to set up Google Cloud Vertex AI with Application Default Credentials or a credential file (#57857, #58427).
- â¢Opt into macOS or Windows system proxy settings from `/experimental` (#58157).
- â¢Expand patch details directly in the transcript while an approval prompt is open (#58281).
- â¢Saved chats stay visible after you switch model providers. If a chat canât resume, you can read its transcript and prepare an editable prompt to continue in a new chat (#58202).
- â¢`Ctrl+L` clears the terminal display while keeping earlier messages available through`Ctrl+T` (#58266).

## Show more (7)Show less

- â¢Deleting text that exposes a leading `!` keeps an ordinary draft in chat mode (#58412).
- â¢Network approval prompts close when their requests end. Concurrent requests to the same endpoint prompt one at a time (#58350, #58351).
- â¢Long realtime V3 conversations save completed speech to chat history as the transcript grows (#58221).
- â¢Automatic context compaction now accounts for retained output from failed responses and responses with missing token usage data (#58404).
- â¢The app-server README covers managed network constraints under `[permissions.network]` and`prompt-user` command rules that require fresh human approval (#58045, #58182).
- â¢The `openai-docs` skill recommends the documented replacement when migrating away from a deprecated model (#58134).
- â¢DotSlash manifests now reference the Cargo packages shipped with the release (#58717, #58757).

October 29, 2026

- â¢Added keyboard-driven transcript selection with word, line, and page motions, integrated search, and configurable shortcuts. (#57221, #57222, #57223, #57224)
- â¢Rendered Markdown consistently in async questions, choices, answers, and transcript replies. (#57259, #57292)
- â¢Added GPT-6.1 Terra to the bundled, Vertex AI, and Amazon Bedrock model catalogs. (#57293)
- â¢Added `codex queue --resume` to resume unloaded threads after queueing a message. (#57232)
- â¢Added clearer safety-block explanations and selectable safer follow-up prompts in the TUI. (#57363, #57365)
- â¢Added `codex doctor --network-only` for focused configuration, connectivity, and WebSocket diagnostics. (#57606)
- â¢Preserved acknowledged approval and permission settings when resuming or forking threads. (#57227)
- â¢Preserved local image attachments when restoring killed composer text. (#57502)

## Show more (7)Show less

- â¢Fixed standalone release cleanup on macOS when installations are in use or paths contain non-ASCII characters. (#57503)
- â¢Restored spawned descendant threads when unarchiving their parent. (#57635)
- â¢Fixed app-server thread startup and shutdown races and released completed runtimes promptly. (#57787, #57788)
- â¢Improved TUI reconnection recovery, including offline `/quit` and`/exit` handling. (#57820)
- â¢Preserved reasoning-token accounting across resume and fork to avoid premature compaction. (#57839)
- â¢Switched Bazel to source-built V8 with target-matched Rust bindings and native Windows MSVC archive support. (#57164, #57269, #57644, #57832)
- â¢Shipped the standalone executor in canonical CLI and app-server release packages. (#57837)

October 27, 2026

- â¢Added a built-in Google Cloud Vertex AI provider, configured with `model_provider = "gcp"` . (#56265)
- â¢Added GPT-6.1 Terra to the model catalogs. (#57337)
- â¢Added a repository filter to the session picker and directory grouping to the agent command center, making it easier to navigate sessions across linked worktrees. (#56308)
- â¢Added experimental attached thread forks to the app-server API. (#56351)
- â¢Enabled reasoning effort changes during a conversation by default for supported models. (#56163)
- â¢Enabled memory v2 by default for fresh profiles and added support for importing external-agent memories into it. (#56211, #56414)
- â¢Fixed fresh TUI sessions to honor the serverâs default approval reviewer when no explicit override is provided. (#56090)
- â¢Kept TUI preferences editable and immediately applicable when execution settings are invalid. (#56091)

## Show more (4)Show less

- â¢Cancelled remote MCP tool requests when their local response futures are dropped. (#56132)
- â¢Prevented queued agent messages from restarting work after an interrupt or a switch to Plan mode. (#56330)
- â¢Stopped retrying when a provider reports that a model was not found, preserving its error message. (#56445)
- â¢Closed a marketplace URL validation gap that could allow Git to connect to a different host than the one checked against allowed patterns. (#56263)

October 23, 2026

- â¢Save reusable prompts and slash commands in ten numbered `/presets` slots, with a keyboard menu for quick access. (#54380, #54381)
- â¢Set model, reasoning effort, speed, and permission profiles directly with inline `/model` and`/permissions` arguments. (#54378, #54383)
- â¢Preview viewed and generated images in the fullscreen transcript on terminals supporting Kitty or Sixel graphics. (#54216)
- â¢Authenticate enterprise MCP connections using eligible Codex account grants or dedicated sign-in, including OAuth clients requiring secrets. (#55334, #55859, #56001)
- â¢Mask credentials during supported remote Bash commands and Zsh snapshot sessions, including credentials from custom providers. (#54309, #54432, #56049)
- â¢The Amazon Bedrock setup wizard is now enabled by default, with API key entry grouped alongside other AWS sign-in methods when credentials are detected. (#56033, #56023)
- â¢Failed compaction no longer leaves partial summaries in conversation history, and background compaction failures fall back without panicking. (#53932, #55581)
- â¢Resuming sessions now finds compressed histories and restores saved thread settings instead of replacing them with global defaults. (#54008, #55923)

## Show more (6)Show less

- â¢Windows HTTPS and WebSocket connections now handle proxy authentication challenges using the signed-in userâs Negotiate or NTLM credentials. (#55994, #56000)
- â¢CLI configuration overrides are preserved when `-c` or`--config` appears at multiple subcommand levels. (#56022)
- â¢Queued prompts now wait for pending permission selections to resolve before continuing. (#55682)
- â¢Hardened sandbox checks for case-variant metadata paths on macOS and replaceable symlinks used to locate executables. (#54273, #55384)
- â¢Reduced disk usage by pruning unused installations, cleaning abandoned marketplace checkouts, and bounding daemon logs. (#53827, #55149, #55689, #55690, #55856)
- â¢Updated the embedded JavaScript runtime with safety checks for optimized `Array.sort` operations. (#54519)

October 15, 2026

- â¢Open `/config` to search preferences across categories, change supported settings immediately, and see which changes require a restart. (#52990, #53429)
- â¢Send a follow-up while Codex is responding to steer it immediately. Interrupting a turn also stops active Code Mode cells and their nested tool calls. (#51812, #51835)
- â¢Configure a leader key in `/keymap` . The default is`Ctrl+X` , shortcut hints stay open until you choose or cancel, and voice mute moves to`F9` . (#52273)
- â¢Restart a managed background server from `/daemon` . Codex warns about work the restart could interrupt, then reconnects automatically. (#53173)
- â¢Codex can run independent read-only tools in parallel, including skill, memory, and conversation-history lookups. (#52245)
- â¢Daybreak is enabled by default for eligible accounts. `codex exec` can also select it with an OpenAI API key when the server offers access. (#53701, #53717)
- â¢Resuming a saved thread restores its permissions and network approvals, while respecting explicit overrides and current managed policy. (#53150, #53419, #53424)
- â¢The network proxy now checks permission before a DNS lookup, rejects duplicate `Host` headers, and prevents credential aliases from bypassing inspection hooks. (#51650, #52661, #53506)

## Show more (7)Show less

- â¢Return to an existing side conversation with `/side` after switching threads. Answers to pending prompts also go back to the thread that asked. (#52184, #53436)
- â¢On Windows, `Ctrl+V` and`Shift+Insert` paste text as well as images. On WSL, repeated pastes reuse the clipboard reader, and copy attempts time out instead of hanging. (#53224, #52967, #53453)
- â¢Local commands finish promptly when a child keeps an output pipe open. Remote commands preserve late-arriving output so the final poll can still collect it. (#52652, #53331, #53622)
- â¢`codex exec` no longer prints the same file diff again after an unrelated command. (#53444)
- â¢Updated the bundled OpenAI Docs guidance for migrations to the GPT-6 family, including model-specific reasoning, tool use, and data residency considerations. (#51768)
- â¢Clarified how the Linux sandbox uses bubblewrap for filesystem isolation and seccomp for syscall filtering. (#52610)
- â¢Codex now uses its standard shell executor and no longer bundles a patched `zsh` . Configurations using the retired shell flags receive a migration notice. (#52160, #52213)

October 9, 2026

- â¢Fixed a TUI crash when asynchronous questions contain multiple lines, preserving line breaks and complete hyperlink destinations. (#51866)
- â¢Fixed startup failures caused by differences between a running background server's feature settings and CLI defaults. Compatibility checks now apply only to explicit command-line feature overrides. (#52648)

October 8, 2026

- â¢Add tools for creating and listing managed Git worktrees from trusted local projects when the worktrees feature is enabled. (#50148)
- â¢Pin tasks in the agent Command Center with `p` and keep them in a shared Pinned group when supported by the server. (#51500)
- â¢Navigate and copy transcript blocks with `/copy` , use`Ctrl+Insert` to copy selections, and tune mouse-wheel scrolling with`tui.mouse_scroll_speed` . (#50434, #50215, #50209)
- â¢Make URLs clickable in approval headers, questions, MCP prompts, warnings, banners, and verification prompts, including when links wrap across lines. (#51439, #51449, #51450, #51451, #51452, #51458)
- â¢Configure live web access and remote compaction capabilities for custom Responses-compatible model providers. (#50459)
- â¢Add JavaScript helpers for streaming promise results as they settle, plus opt-in ranked tool search in Code Mode. (#51126, #51209)
- â¢Respect server model and reasoning-summary defaults for new TUI threads while retaining explicit launch overrides. (#50013, #50811, #50913)
- â¢Preserve existing CRLF line endings in `apply_patch` updates without requiring an opt-in. (#51203)

## Show more (6)Show less

- â¢Fix Linux sandbox startup with multiple denied files, reject writable sandbox-construction executables, and keep ripgrep configuration from weakening deny-glob masks. (#50059, #51211, #51407, #51527)
- â¢Restore ordinary drive-letter file access on Windows 10 and match Windows sandbox temp permissions to the child process environment. (#51511, #51512)
- â¢Honor server `Retry-After` advice for retryable Responses and WebSocket failures. (#50418, #51440)
- â¢Fix archive checksum verification when installing through Windows PowerShell with PowerShell 7 module paths present. (#51257)
- â¢Publish a signed PowerShell installer with Windows releases. (#51158)
- â¢Keep older stable releases and prereleases from replacing newer stable download targets or installer aliases. (#51186, #51425)

October 7, 2026

- â¢GPT-6.1 Sol is now the default model in the bundled and Amazon Bedrock catalogs. (#49318, #49339)
- â¢Amazon Bedrock supports multi-agent V2 and Ultra reasoning on compatible models; Bedrock Mantle also accepts AWS GovCloud regions. (#49345, #49813)
- â¢Sign in to MCP servers from an active terminal session with `/mcp login <name>` . (#49290)
- â¢Choose your microphone, speaker, and microphone input channels for voice conversations, with preferences saved locally. (#49437, #49836)
- â¢Daybreak is opt-in through `--enable cli_daybreak` or`features.cli_daybreak=true` ;`daybreak=true` alone is insufficient. By default, controls and indicators are hidden,`/daybreak` is unavailable, and automatic Cyber routing is omittedâeven for saved Daybreak threads. Saved preferences remain intact. Opt-in routing requires eligible ChatGPT sign-in, the OpenAI provider, and advertised model/program support. (#49856, #49858, #49859, #49861, #51207)
- â¢Select a Cyber access program per turn with `codex exec --cyber-access-program` or the TypeScript SDKâs`cyberAccessProgram` option. The explicit exec override remains available with`cli_daybreak` disabled and leaves the saved choice unchanged. (#49939, #51207)
- â¢Approved filesystem escalation can now grant broader write access while preserving denied reads and network restrictions. Background tasks retain their originating turnâs permissions. (#49353, #49880)
- â¢Explicit launch permissions survive terminal reconnects and new sessions, while implicit client settings no longer overwrite server or saved-thread web-search settings. (#49809, #49799)

## Show more (6)Show less

- â¢Elevated Windows terminal sessions can start using an embedded server, and sandboxed PowerShell preserves relative paths beneath protected user profiles. (#49855, #49690)
- â¢Enter correctly submits buffered input after paste detection expires, including in Vim insert mode. (#49810)
- â¢Thread resume includes the latest committed history. Startup detects recoverable SQLite corruption earlier and preserves damaged databases as backups. (#49599, #49701)
- â¢Responses retries and WebSocket-to-HTTP fallback honor server retry guidance, reducing premature failures during overload. (#49441)
- â¢Authentication guidance now accounts for keyring storage instead of implying credentials always reside in `auth.json` . (#49361)
- â¢Publishing an older alpha or hotfix no longer moves npm alpha tags backward. (#49704)

October 5, 2026

- â¢Preserve `SYSTEMROOT` ,`TEMP` , and`TMP` when launching remote stdio MCP servers with explicitly configured remote environment variables, allowing Unix hosts to retain the Windows executor's startup environment.
- â¢#51121: Backport Windows remote MCP environment preservation to 0.160.

October 1, 2026

- â¢Browse older tasks in the agent command center with a keyboard-accessible âShow moreâ action. (#49106)
- â¢Select transcript text and paste with middle-click in fullscreen mode on supported local Linux X11 terminals. (#49112)
- â¢Start sessions outside a project with workspace defaults when policy permits, and restore saved permissions when resuming. (#49160)
- â¢Added opt-in Guardian review capabilities to retrieve earlier user instructions and include context from agent handoffs. (#49036, #49057)
- â¢Unsent queued messages now resume after reconnection once uncertain submissions are resolved, avoiding duplicate sends. (#49105)
- â¢The terminal UI now preserves server provider, reasoning-summary, and verbosity settings and shows the correct sessions in resume and fork history. (#49144, #49161, #49171)
- â¢Fixed Windows sandbox PowerShell fallbacks and long-path permission repairs, and suppressed unwanted console windows from background helpers. (#49019, #49058, #49098, #49164, #49386)
- â¢Subagents now retain environments that are still starting and receive their configuration or preparation failure. (#49075)

## Show more (5)Show less

- â¢Prevented SQLite stalls during connection setup and logging, and surfaced initialization errors instead of masking them as timeouts. (#49032, #49102)
- â¢Explicit provider model catalogs no longer include unsupported bundled models or reuse stale entries after refresh failures. (#49135)
- â¢Clarified how provider credentials use the configured storage backend and how `env_key` identifies the API-key environment variable. (#49118)
- â¢Reduced repeated plugin-loading work by caching parsed manifests and reusing HTTP connections for remote plugin requests. (#49099, #49100)
- â¢Added background reclamation of unused log database space to reduce disk usage. (#49069)

September 29, 2026

- â¢Opt-in `instant_interrupt` lets new input steer Codex during model responses or long-running code-mode calls. (#48135, #48141)
- â¢New sessions get a compact welcome screen and consistent headers, with occasional tips during and after turns. (#48513, #48562, #48352)
- â¢The warnings viewer dismisses reviewed warnings when closed; press `k` to keep one for later. (#48205, #48206)
- â¢You can scroll the transcript while deciding whether to implement a plan. (#48805)
- â¢Native Mermaid rendering supports more flowchart edges, labels, and node groups. (#48814, #48895)
- â¢App-server clients can paginate thread history from a specific item. (#48151)
- â¢Windows launches avoid stray console windows for MCP servers, code-mode hosts, and piped commands; restrictive launchers can fall back to embedded mode. (#48138, #48238, #48483, #48491)
- â¢Copying transcript selections preserves Markdown tables, formatting, and significant whitespace. More terminals now copy automatically on selection. (#48548, #48549, #48469)

## Show more (6)Show less

- â¢Blank sessions retain drafts when switching tasks, and threads can be archived and listed before their first turn. (#48628, #48828, #48199)
- â¢Local ChatGPT sign-in opens the browser reliably; onboarding also provides a shortcut to copy the login link. (#48502, #48544)
- â¢Approved commands retain explicit filesystem denials, and `.aws` directories are protected by default under writable roots. (#48155, #48176)
- â¢Fixed macOS TLS access in network-enabled sandboxes and remote environments that require proxy access. (#48565, #48198)
- â¢Removed automatic follow-up prompt suggestions and the `tui.prompt_suggestions` setting. (#48621)
- â¢Removed the bundled `plugin-creator` skill. (#48604)

September 28, 2026

New Features

- Configure copy-on-select and right-click paste in the fullscreen TUI. Copied transcript selections now preserve Markdown formatting. (#47639, #47896, #48118)
- Connect to MCP servers that require pre-registered OAuth client secrets, including through `codex mcp add --oauth-client-secret` . (#47891)
- Secure direct exec-server WebSocket connections with bearer tokens, including connections configured through app-server. (#47601, #47648)
- Image generation and editing can explicitly request transparent backgrounds, and edits now accept file-backed conversation images. (#47484, #47956)

## Show moreShow less

- Terminal input approval is enabled by default for commands running with elevated permissions; runtime-only grants no longer cause unnecessary reviews. (#47799, #48073)

Bug Fixes

- Fixed Windows sandbox failures involving ordinary Windows 10 paths, rejected stored credentials, and large permission policies. (#47672, #47695, #47919)
- Fixed Linux sandbox startup with nested writable roots and preserved Git metadata protections across writable roots on Linux and macOS. (#47623, #47974)
- macOS patch operations now recognize system path aliases covered by existing permissions, avoiding unnecessary approval prompts. (#47879)
- Approval reviews now retry when new user input arrives, so a status question does not automatically abort a pending action. (#47819)
- Mermaid flowcharts now render quoted labels and ampersands; unsupported diagrams explain why they fall back to source. (#47572, #47678)
- Command completion events now include early output and report process-launch failures to clients. (#47529, #47665)

Changelog

Full Changelog: github.com/openai/codex/compare/rust-v0.157.0...rust-v0.158.0

- #47441 Use secondary text styling for the transcript footer shortcut hint @etraut-openai
- #47447 Extract WebSocket authentication into `codex-websocket-auth` @euroelessar
- #47458 Parallelize instruction refresh and tool preparation @hlevy-openai
- #47484 Add explicit background control to image generation @alicec-oai
- #47520 Route session agent operations through `AgentControl` @jif-oai
- #47529 Emit command lifecycle events for unified exec launch failures @steipete-oai
- #47536 Route agent lookups and V2 context through `AgentControl` @jif-oai
- #47539 Test interrupted one-shot command launch failure persistence @jif-oai
- #47540 Add an option to disable multi-agent v2 direct messaging @jif-oai
- #47565 Classify rollout read failures by reason and progress @jif-oai
- #47568 Record sandbox backends in command execution analytics @iceweasel-oai
- #47571 Wait for idle before injecting remote compaction test history @felixxia-oai
- #47572 Explain Mermaid rendering fallbacks in the TUI @etraut-openai
- #47582 Retain assistant context for Guardian authorization reviews @felixxia-oai
- #47584 Preserve streamed assistant message order in retained context @felixxia-oai
- #47585 Preserve delivered assistant messages in Guardian retained context @felixxia-oai
- #47589 Keep unfinished link destinations out of rich streaming previews @etraut-openai
- #47590 Serialize numeric custom reasoning effort as JSON numbers @dylan-hurd-oai
- #47591 Stream daemon executable hashing off the async runtime @etraut-openai
- #47596 Refresh realtime context for each model request @reia-oai
- #47597 Guard prerelease channel and canary updates against older versions @imac-oai
- #47601 Add opt-in WebSocket authentication to exec-server @euroelessar
- #47603 Add a reap-only drop policy for child processes @charliemarsh-oai
- #47604 Extend child commands with session and descriptor controls @charliemarsh-oai
- #47605 Route pipe processes through the shared child launcher @charliemarsh-oai
- #47610 Use native POSIX spawning for command hooks @charliemarsh-oai
- #47611 Use the shared process launcher for Unix shell snapshots @charliemarsh-oai
- #47612 Launch Linux pipe processes through a fresh setup helper @freeqaz-openai
- #47613 Expand Linux spawn-helper lifecycle test coverage @charliemarsh-oai
- #47617 Route Linux PTY launches through the process setup helper @charliemarsh-oai
- #47618 Prefer Shift-arrow hints for queued messages and questions @imac-oai
- #47619 Add bounded buffering for global operation metrics @celia-oai
- #47620 Add portable project trust lookup APIs @seanh-oai
- #47623 Fix read-only metadata mount ordering for nested writable roots @jif-oai
- #47624 Recognize `user_message` tools in Guardian authorization context @ankushg
- #47625 Allow history and notes without experimental context capability @pmccrary-oai
- #47629 Route V2 child loading through `AgentControl` @jif-oai
- #47630 Bind Guardian reviews to the action's target environment @jif-oai
- #47633 Route message board agent resolution through the selected controller @jif-oai
- #47635 Overlap startup WebSocket preconnect with tool discovery @bromano-oai
- #47638 Classify retryable exec-server preparation errors by type @mtsui-oai
- #47639 Add configurable copy-on-select for transcript selections @fcoury-oai
- #47641 Honor Retry-After and preserve server retry deadlines @anp-oai
- #47642 Add model-specific prefixes to indirect tool descriptions @rhan-oai
- #47647 Apply Guardian computer-use review to the Browser connector @johnl-oai
- #47648 Support bearer tokens for app-server executor connections @euroelessar
- #47649 Add opt-in OTLP logging for final agent responses @xli-oai
- #47653 Attach inherited rollout history to diagnostic reports @dkovalenko-oai
- #47654 Make Linux descriptor cleanup fork-safe @yuzhu-oai
- #47655 Bump the exec-server stable compatibility test to Codex 0.156.1 @imac-oai
- #47657 Restrict the default Bedrock GovCloud model catalog @jackz100
- #47662 Expose tool dispatch and timing observations to extensions @euroelessar
- #47663 Preserve managed network policy in route-aware transports @jackz100
- #47665 Preserve early unified exec output in completion events @sdcoffey
- #47670 Support model-specific descriptions for agent message board tools @eknight-oai
- #47672 Fix no-reparse directory opens on Windows 10 @zm-oai
- #47673 Clarify registered Windows sandbox setup errors @zm-oai
- #47677 Support model catalog overrides for MCP resource tool specs @rhan-oai
- #47678 Support quoted labels and ampersands in Mermaid flowcharts @etraut-openai
- #47679 Add extension hooks for model requests and response streams @euroelessar
- #47680 Add exec-server RPC timing and process startup tracing @anp-oai
- #47683 Add executor capability discovery V2 infrastructure @TAFOYA-OAI
- #47686 Make thread-owned Guardian context always enabled @felixxia-oai
- #47688 Remove legacy Guardian authorization evidence paths @felixxia-oai
- #47689 Make Guardian thread context capture unconditional @felixxia-oai
- #47690 Remove obsolete Guardian context capture mode branches @felixxia-oai
- #47691 Materialize rollout persistence for pending inter-agent messages @dermanyang-oai
- #47693 Configure curl retries for DotSlash installation in CI @anp-oai
- #47695 Repair rejected Windows sandbox credentials during provisioning @zm-oai
- #47696 Avoid the shutdown timeout in the lagged-event test @jgershen-oai
- #47698 Allow WebSocket test server shutdown while waiting for requests @jgershen-oai
- #47701 Allow idle threads to prewarm and repair WebSocket connections @vkg-oai
- #47703 Preserve account network policy for ChatGPT backend requests @jackz100
- #47704 Fix spawn flag typing and isolate project configuration tests @seanh-oai
- #47709 Route resume prewarm through the cached WebSocket session @vkg-oai
- #47712 Update unified exec output buffers atomically @sdcoffey
- #47713 Reduce dependency coupling in shared configuration crates @aibrahim-oai
- #47714 Preserve tool result metadata more selectively under size limits @ningyi-oai
- #47717 Avoid recursive TUI event dispatch for model picker selections @etraut-openai
- #47741 Attribute tool telemetry to the invoking turn's product SKU @rennie-openai
- #47742 Honor network policy in history notes and image generation extensions @jackz100
- #47745 Skip startup prewarm preparation when the WebSocket is ready @vkg-oai
- #47748 Align Cargo and Bazel Rust debug information defaults @aibrahim-oai
- #47751 Reduce generic code duplication in RPC and Markdown rendering @aibrahim-oai
- #47755 Centralize typed app-server response decoding @aibrahim-oai
- #47757 Refactor tool telemetry product SKU matching to use an allowlist @rennie-openai
- #47758 Preserve more tool metadata within outgoing message budgets @ningyi-oai
- #47773 Honor catalog schemas for asynchronous user input @rhan-oai
- #47797 Support close-on-exec attachments without changing PTY semantics @jif-oai
- #47799 Enable terminal input approval by default @jif-oai
- #47808 Allow hosts to provide agent controllers through ThreadManager @jif-oai
- #47811 Preserve explicit user goal updates in Guardian authorization @felixxia-oai
- #47813 Fix sleep interruption test event handling and fixture lifetime @felixxia-oai
- #47814 Fix a lost wakeup in the unified exec termination test @felixxia-oai
- #47817 Stabilize thread resume and memory dual-write tests @jif-oai
- #47819 Retry Guardian reviews when authorization changes @teddywyly-oai
- #47820 Add integration coverage for host agent controllers @jif-oai
- #47824 Allow four concurrent threads in the multi-agent resume test @felixxia-oai
- #47828 Wait for login completion in recommended plugin tests @felixxia-oai
- #47830 Bind Guardian async scores to target environment permissions @jif-oai
- #47832 Stabilize Rosetta test timing and retry delay telemetry @jif-oai
- #47847 Keep the TUI responsive during clipboard copies @fcoury-oai
- #47851 Preserve human overrides across repeated heartbeat instructions @felixxia-oai
- #47852 Avoid blocking async proxy resolution on the system proxy cache @jif-oai
- #47856 Initialize media estimates outside the global cache lock @jif-oai
- #47858 Validate loaded plugins outside the cache lock @jif-oai
- #47861 Avoid nested read locking when rendering browser sign-in @jif-oai
- #47867 Render remote permission paths using executor context @iceweasel-oai
- #47870 Add opt-in OTLP logging for Guardian assessments @jif-oai
- #47871 Fix PID reservation test race and update guardian heartbeat snapshot @jif-oai
- #47873 Measure deferred tool namespace fragments before and after truncation @mzeng-openai
- #47879 Fix macOS system-alias matching in patch permission checks @felixxia-oai
- #47881 Use Tokio's clock for TUI paste timing @charliemarsh-oai
- #47886 Preserve diagnostic logs when SQLite logging fails @dkovalenko-oai
- #47887 Warn users when SQLite diagnostic log writes fail @dkovalenko-oai
- #47889 Include TUI client logs in diagnostic uploads @etraut-openai
- #47891 Support client secrets for pre-registered MCP OAuth clients @willwang-openai
- #47894 Clean up temporary Codex homes after TUI tests @fcoury-oai
- #47896 Preserve Markdown formatting when copying transcript selections @fcoury-oai
- #47898 Preserve local-binding inheritance in environment network policies @seanh-oai
- #47899 Add diagnostic reasons to MCP attribution errors @peilin-openai
- #47900 Default local threads to paginated history @owenlin0
- #47901 Add bounded credential-storage telemetry helpers @celia-oai
- #47902 Avoid repeated table clones during config merging @imac-oai
- #47903 Move config key alias normalization ahead of merging @imac-oai
- #47904 Support nested canonical paths in config key aliases @imac-oai
- #47908 Alias `tui.whimsy` to`tui.effects.starfield` @imac-oai
- #47912 Fix attestation routing during thread startup @charliemarsh-oai
- #47913 Add an opt-in flag to defer mailbox preemption @jif-oai
- #47915 Reuse verified V8 checksum manifests from the artifact cache @aibrahim-oai
- #47918 Parameterize the turn-start originator header test @aibrahim-oai
- #47919 Transport large Windows sandbox launch payloads through the environment @malsamiri-oai
- #47920 Allow directory moves under global Seatbelt basename denies @chess-oai
- #47922 Allow full-access Windows setup to provision through registered Core @zm-oai
- #47924 Make project trust lookup paths explicit and defer root resolution @seanh-oai
- #47926 Retry file blob uploads on HTTP 502 and 504 @mtsui-oai
- #47927 Use `127.0.0.1` for local login redirects @willwang-openai
- #47932 Remove GPT-5.4 from bundled catalogs and preserve migration prompts @andrewgu-oai
- #47934 Apply the unchanged-model compaction shortcut to all session sources @hlevy-openai
- #47935 Allow cached catalogs to satisfy MCP startup readiness @hlevy-openai
- #47936 Make MCP and Code Mode input schema budgets configurable @vivi
- #47937 Add direct replies for thread settings updates @sayan-oai
- #47939 Separate selected plugin identities from MCP contributions @sayan-oai
- #47943 Remove unused Windows world-writable audit code @iceweasel-oai
- #47945 Parameterize the thread initialization analytics test by originator @eddie-openai
- #47946 Add an in-memory agent message board for ephemeral sessions @jif-oai
- #47947 Expand root authorization context to 16 messages @felixxia-oai
- #47951 Use prebuilt V8 archives for Bazel on macOS and GNU Linux @aibrahim-oai
- #47952 Prune expired in-memory message board registry entries @jif-oai
- #47954 Move fullscreen startup tips into the transcript @etraut-openai
- #47956 Support file references in image edit requests @kchainani-oai
- #47957 Bound tool-call observations to the outgoing Responses message budget @ningyi-oai
- #47962 Request transparent huge pages for Cargo and eligible Bazel rustc jobs @aibrahim-oai
- #47964 Preserve the client-agent header for Amazon Bedrock Runtime @celia-oai
- #47967 Surface Flex capacity failures as a distinct terminal error @sdcoffey
- #47968 Handle Btrfs device mismatches when masking daemon sockets @etraut-openai
- #47970 Expose current environment selections for a running turn @sayan-oai
- #47971 Add Pro Max plan support and update Pro display names @etraut-openai
- #47974 Preserve Git directory protections across writable roots @aionescu-oai
- #47975 Prevent stale voice answers from reappearing during speech recovery @etraut-openai
- #47981 Prepare MCP calls directly from advertised tool identities @hlevy-openai
- #47984 Add multi-agent spawn latency and failure metrics @owenlin0
- #47988 Reuse MCP handlers across equivalent bindings @hlevy-openai
- #47989 Add startup-only PID namespace inheritance to exec-server @open-matt
- #48004 Respect configured authentication in the thread manager sample @celia-oai
- #48015 Validate tool suggestion install URLs before showing the app link @aionescu-oai
- #48017 Test same-cell permission grants and strict review in code mode @anp-oai
- #48035 Remove plugin extension metadata from discovery and summaries @victor-openai
- #48060 Deduplicate retained instructions across Guardian reviews @felixxia-oai
- #48069 Handle early command yields in the Guardian network approval test @jif-oai
- #48072 Skip message-board notification previews when there are no recipients @jif-oai
- #48073 Avoid stdin approval for runtime-only permission grants @jif-oai
- #48077 Add Serde support to agent message board request types @jif-oai
- #48078 Replay exec-server shell snapshots through unnamed files @jif-oai
- #48098 Preserve recent authorization context for Guardian reviews @felixxia-oai
- #48099 Honor shell environment policy in legacy snapshots @jif-oai
- #48100 Add an HTTP client for remote agent message boards @jif-oai
- #48101 Show multiline command previews in `/ps` @etraut-openai
- #48109 Deduplicate retained instructions against Guardian transcripts @felixxia-oai
- #48110 Deduplicate retained instructions in async Guardian context @felixxia-oai
- #48115 Preserve user text parts during local compaction @felixxia-oai
- #48116 Allow reasoning shortcuts to reach Max @etraut-openai
- #48118 Add configurable right-click paste to the fullscreen TUI @fcoury-oai
- #48119 Prevent worker completion races in the guardian authorization test @felixxia-oai
- #48121 Keep startup drafts visible during command center session handoff @etraut-openai
- #48123 Add early yielding for code-mode observations @pakrym-oai
- #48130 Use request notifications in the cloud config loader lifetime test @felixxia-oai
- #48132 Allow Left to open the command center from read-only conversations @etraut-openai

September 25, 2026

- â¢Added GPT-6 Sol and Luna, including Amazon Bedrock support and migration prompts for older models. (#47332, #47347)
- â¢Enabled fullscreen transcripts by default and added Shift-click to extend text selections. (#47178, #47414)
- â¢Enabled automatic background-server startup for eligible interactive sessions, with recovery choices when server settings are incompatible. (#47179, #47318)
- â¢Added an `f` shortcut to fork conversations open in another app, preserving drafts and queued prompts. (#47185)
- â¢Made `/import` available in remote sessions and local background-server sessions. (#47317)
- â¢Improved terminal rendering with Unicode bullets, checkboxes, aligned equations, and optimization notation. (#47191, #47322)
- â¢Preserved active voice conversations when switching threads. (#47381)
- â¢Recovered unsent question answers into the composer when turns end, without disrupting active history searches. (#47422, #47423)

## Show more (4)Show less

- â¢Respected tmux mouse settings and restored native scrollback for Terminal.app over SSH in automatic screen mode. (#47399, #47417)
- â¢Fixed configured proxy routing for realtime connections and standalone web search, including search redirects. (#47101, #47142, #47204)
- â¢Added retries for transient file-upload failures and increased the upload timeout to five minutes. (#47122, #47393)
- â¢Enforced network restrictions across redirects and ongoing HTTP and WebSocket traffic, including cancellation when policy changes revoke access. (#47389, #47407)

September 22, 2026

- â¢Choose an optional fullscreen UI with `/tui` for your next launch, including transcript search, mouse selection, and right-click copying. (#46732, #46734, #46883, #46895)
- â¢Voice conversations are enabled by default, with an F8 toggle, a `/voice settings` picker, and bundled audio runtimes for Linux and Windows. (#44921, #46071, #44622, #44714, #44922)
- â¢Explore account usage, token totals, and plugin and skill activity through the `/usage` analytics dashboard. (#45764, #45769)
- â¢Filter tasks by status and create worktree sessions from the agent command center; worktree support is now enabled by default. (#46839, #45276, #44870)
- â¢Customize the terminal with six new themes and view supported Mermaid diagrams and display equations directly in responses. (#46504, #46054, #45612)
- â¢Update the local background server through `/daemon` , or bypass it with`--no-daemon` . (#45854, #46088)
- â¢Preserve streamed answers and plans when turns fail, are interrupted, or receive subagent completion events. (#45549, #46867)
- â¢Fix clipboard forwarding in tmux and SSH sessions and preserve tab indentation when terminals send pasted text as individual keystrokes. (#45457, #45454)

## Show more (7)Show less

- â¢Restore Plan mode when resuming sessions and preserve thread identity and settings when editing earlier prompts. (#45519, #45845)
- â¢Recover login through system proxies and refresh MCP credentials when OAuth discovery returns a 503 error. (#46562, #44636)
- â¢Prevent speech from being dropped during playback pauses or bursts of incoming audio. (#46880)
- â¢Close sandbox isolation gaps involving inbound Windows connections, privileged Linux/macOS sockets, and writes through read-only macOS file handles. (#44639, #45984, #46500)
- â¢Clarify that deprecated `friendly` and`pragmatic` personality settings no longer select response styles. (#45809)
- â¢Explain how `?` matches a single character in network proxy allow and deny patterns. (#46027)
- â¢Update bundled TLS dependencies, including OpenSSL 3.6.4 for Linux musl builds. (#45149, #45489)

September 18, 2026

- â¢Added experimental `/voice` conversations with live transcripts and microphone controls on supported builds, enabled through`/experimental` . (#43581, #43651, #44331)
- â¢The TUI now shows live reasoning summaries in the status row and completion timestamps after successful turns. (#43558, #43921)
- â¢Added task hiding, archiving, and deletion in the agents overview, plus worktree ownership details and confirmed deletion of clean managed worktrees. (#43942, #44424, #44433)
- â¢Added Touch ID verification for MCP requests in local TUI sessions on supported Macs. (#43624, #43712, #43715)
- â¢Added configurable daemon update schedules and `codex app-server daemon update` ; saved threads and active goals can recover after daemon restarts. (#43542, #43562, #44314)
- â¢Amazon Bedrock can now obtain AWS credentials from configured commands, with caching, expiration-based refresh, and authentication recovery. (#44028)
- â¢Accepted prompts are now saved even when compaction fails before a turn starts. (#44487)
- â¢Fixed missed tmux resizes, transcript viewport restoration, and stale history appearing after switching threads. (#43603, #43889, #43994)

## Show more (5)Show less

- â¢MCP servers now report expired OAuth credentials accurately and provide reconnect guidance when token refresh fails. (#43947, #44359)
- â¢Automatic approval reviews now preserve complete actions and authorization evidence more reliably, retry transient failures, and distinguish review failures from unsafe-action findings. (#44482, #44569, #44570)
- â¢Switching accounts now invalidates remote-control sessions, cached WebSocket state, and model catalogs belonging to the previous identity. (#43906, #44341, #44489)
- â¢Blocked Windows-process escapes from restricted WSL sandboxes and hardened brokered shell snapshots against credential exposure. (#44286, #43909, #44040)
- â¢Aligned Python SDK and runtime publishing with stable CLI releases, using matching versions and verifying runtime assets before SDK publication. (#44067)

September 10, 2026

- â¢Add `max` and`ultra` reasoning-effort values. #39662
- â¢Add `ExternalMessage` to synchronous and asynchronous`run()` and`turn()` calls. External content can start a turn or join an active regular turn with tool-level authority; it does not grant user authorization. Consumers receive independent event streams. #44086
- â¢Add `include_turns` on resume/fork,`turn_service_tier` for one newly started turn, and`source` metadata. History selection changes the returned response, not model context. Existing defaults are preserved when these options are omitted. #44084
- â¢Refresh generated protocol models and notifications, and preserve completion events that arrive before a turn-start response. #44032, #44400
- â¢`HookMetadata` wraps its handler in`.root` . Replace accesses such as`hook.command` with`hook.root.command` , checking`hook.root.handler_type` before reading handler-specific fields.
- â¢Some previously unknown notifications now have typed payloads. Read named fields instead of `.params` ; unknown or invalid payloads still use`UnknownNotification` .
- â¢Manually constructed or late-joining turn handles receive events from their attachment point. Earlier output is not replayed, so collected results can be partial; attaching after completion can raise `TransportClosedError` . Use`thread.read(include_turns=True)` for saved history. Handles returned directly by`thread.turn(...)` retain events from when their request is sent.

September 10, 2026

- â¢GPT-6-Astra is now available in the model picker and Amazon Bedrock catalogs. (#42879, #42619)
- â¢Experimental worktree support lets you create isolated checkouts for new or forked sessions using `--worktree` or`/worktree` , then browse and resume them. (#42652, #43069, #43120, #43286)
- â¢Answer questions inline while Codex continues working, using suggested choices or custom text without losing your main draft. (#42891, #42894, #42897)
- â¢Windows sessions can now share a background Codex server, with daemon lifecycle commands and managed updates. (#42405, #42392)
- â¢Vim editing gains `R` replace mode with undo and dot-repeat, plus more reliable Escape handling in legacy terminals. (#42194, #42584)
- â¢Copying responses preserves formatting in rich-text apps, and `/copy` can copy status output or individual session fields. (#42847, #43055)
- â¢Existing sessions pick up newly installed plugin tools and refresh skills and hooks after external plugin upgrades or rollbacks. (#42284, #42593, #42990)
- â¢MCP connections coordinate OAuth token refreshes and surface login challenges when refresh fails, without automatically replaying rejected tool calls. (#42413, #42552)

## Show more (6)Show less

- â¢Startup avoids running workspace-controlled helpers before trust is established, and the macOS sandbox blocks terminal input injection. (#42324, #42590)
- â¢Remote resume and fork operations preserve saved permissions; fresh sessions and forks respect server model defaults unless explicitly overridden. (#43330, #43177, #43355)
- â¢Resuming a conversation open in another app now shows a read-only transcript with a retry option while preserving your draft. (#43253)
- â¢Automatic approval reviews better preserve authorization context through compaction and reject approvals invalidated by new user instructions or answers. (#42844, #42852, #43442)
- â¢Updated the bundled OpenAI Docs skill with GPT-6-Astra migration, compatibility, and prompting guidance. (#42931)
- â¢The deprecated `codex mcp-server` entry point is no longer available. (#42993)
