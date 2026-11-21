---
id: collect-261121-veille-ia/gemini-google/antigravity-changelog-november-2026
title: "Antigravity Changelog (November 2026)"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["conversation", "remote", "control", "view", "gemini", "afixed", "such", "antigravity"]
source: https://www.gradually.ai/en/changelogs/antigravity/
source_anchor: ""
source_lines: [1, 600]
section: "Gemini_Google"
---
## Latest Releases (135)

### Antigravity CLI: Remote Control split view, Gemini 4 Neon API key support, and faster conversation forking.

November 19, 2026

- â¢Added split view to the Remote Control browser view, so you can keep several conversations open side by side: choose Split Right or Split Down from the Split submenu of a conversation's menu in the sidebar or of the â® menu in its title bar (not available on phones).
- â¢Added Gemini 4 Neon (Low, Medium, High, and XHigh) to /model and --model when you connect with a GEMINI_API_KEY.
- â¢Added a scroll position indicator to the full screen (altscreen) rendering mode: while you're scrolled up, the line above the prompt shows how much of the conversation is below what you're reading, such as â 25 lines Â· 2 turns.
- â¢Improved Markdown headings in agent responses and artifacts, which now show only their text, set apart by bold, italic, or underline, instead of starting with ## to ###### markers that looked like Markdown that failed to render.
- â¢Improved /fork for conversations whose artifact folder has grown large, for example after the agent cloned a repository or kept build output in scratch/: the fork now leaves those files with the original conversation and points the forked agent to them instead of copying every file, so forking takes seconds instead of up to an hour, and deleting the original conversation also deletes those files.
- â¢Improved scrolling back through a long conversation in the Remote Control browser view: Loading older messagesâ¦ now shows while earlier messages load, and if they fail to load, Couldn't load older messages Â· Retry lets you try again instead of the view retrying silently every time you scroll up.
- â¢Improved Auto mode's review of commands that would leave the terminal sandbox so it decides faster, times out less often, and asks you to approve safe commands less often: when the review runs on Gemini 4 Hydrogen, it now uses medium instead of high thinking.
- â¢Fixed agent turns stopping with an error after a brief connection problem, such as a connection the server reset, a model response that broke off partway through, or the network not being ready yet right after your computer wakes from sleep; the CLI now retries these requests automatically.

## Show more (6)Show less

- â¢Fixed Remote Control, --remote-agent, and voice dictation being offered when you connect with a GEMINI_API_KEY, where they could only fail, for example with a Remote Control link that never connected: --remote-control and --remote-agent now exit with code 2 at launch, and /remote-control, /voice, and /record are left out of autocomplete and /help and explain that they aren't available with a Gemini API key.
- â¢Fixed the status line showing a red AI: Out of credits when you connect with a GEMINI_API_KEY and AI credits are turned on in your settings; API keys don't use AI credits, so that status and the Use AI Credits setting in /config no longer appear.
- â¢Fixed copying a long prompt with the mouse in the full screen (altscreen) rendering mode putting a line break wherever the prompt wrapped on screen; a prompt you sent, or the text in the prompt box, now copies as you typed it.
- â¢Fixed bold text next to Chinese, Japanese, or Korean text showing its  **markers when the bold part starts or ends with punctuation, such as** æ³¨æï¼**è¿é.
- â¢Fixed the Remote Control browser view crashing on a conversation that contains @ mentions pasted back from a Markdown export, such as Google Docs' Copy as Markdown; such mentions now paste as plain text, and affected conversations open normally again.
- â¢Fixed pressing Enter in the Remote Control browser view sending your message while the send button was disabled, such as before an attachment finished uploading or while the conversation was still loading; Enter now waits until you can send, and your draft stays in the message box.

### Antigravity Eclipse extension: In-IDE issue reporting and retry actions, improved mid-session crash recovery, and sign-in flow reliability improvements

November 19, 2026

- â¢In-IDE Feedback Reporting: Added direct issue reporting and retry actions to error views across chat, settings, and artifact panels.
- â¢Service Crash Recovery: Fixed an issue where unexpected service terminations were not detected and displayed across open tool views.
- â¢Sign-In Experience: Fixed an issue where completing sign-in prompted to launch an external desktop application instead of returning to the editor.

### Antigravity IDE: Background update checks, instant startup, remote sign-in port forwarding, and Windows recovery fixes

November 19, 2026

- â¢Increased the default backend startup readiness timeout from 30 to 60 seconds to improve startup reliability on Windows.
- â¢Added automatic port forwarding and authorization code fallback for sign-in across remote development environments.
- â¢Enabled immediate startup on installed backend binaries without blocking editor activation on release checks.
- â¢Added recurring background release checks and a notification prompt to restart when an update is ready.
- â¢Added keyboard shortcuts to search within conversations when the Agent panel is focused.
- â¢Added visually hidden semantic headings to conversation messages to improve turn-by-turn navigation for screen readers.
- â¢Added support for custom root certificate authorities and proxy configurations during sign-in.
- â¢Fixed background recovery loops attempting doomed restarts during Windows desktop logoff or system shutdown.

## Show more (4)Show less

- â¢Fixed an issue where opening multiple editor windows on shared release channels could cause unintended backend binary downgrades.
- â¢Fixed an issue where transient recovery timeouts could wipe local metadata and trigger unnecessary binary re-downloads.
- â¢Fixed task completion notifications on Windows focusing the incorrect window across multi-window workspaces.
- â¢Addressed other minor user interface issues.

November 18, 2026

- â¢Added dragging a subagent's tab from the side panel into the message box in the Remote Control browser view, which adds an @ mention of that subagent's conversation to your message.
- â¢Improved undo in Vim mode to work like Vim: u now undoes one command at a time instead of grouping edits made close together, a change that enters Insert mode, such as cw or o, undoes together with everything you typed before Esc, and undoing or redoing an operator such as dd or diw puts the cursor at the start of the changed text.
- â¢Improved agy plugin validate so you can check a plugin before you install it: besides a local folder, it now accepts a plugin .zip file, an https:// link to one, or a GitHub repository URL, using github.com///tree/ for a plugin in a subfolder.
- â¢Changed file and artifact tabs in the side panel of the Remote Control browser view to open with their sidebar closed, so the file or artifact gets the panel's full width; previously the file tree or artifact list opened on its own whenever the panel was wide enough, and you can still open it with Toggle sidebar in the tab's header.
- â¢Fixed /usage, /quota, and /credits loading forever or showing an empty quota when you connect with a GEMINI_API_KEY, which has no account quota to show; they are now left out of autocomplete and /help, running one explains why and points you to the key's usage in AI Studio, and in -p runs they exit with code 2.
- â¢Fixed plugins whose plugin.json starts with a UTF-8 byte order mark (BOM), as some Windows editors save it, not loading even though agy plugin install reported success, and agy plugin validate rejecting them.
- â¢Fixed files larger than a few megabytes, such as photos and videos, failing to upload when you attach them in the Remote Control browser view; attachments up to 20 MB now upload.
- â¢Fixed clicking a quote in one of your messages in the Remote Control browser view doing nothing when the quoted passage was too far back in a long conversation to have loaded yet; the view now loads that part of the conversation and scrolls to the passage.

## Show more (1)Show less

- â¢Fixed installing the CLI printing routine progress messages as lines starting with ERROR: logging before google.Init, which made a successful install look like it had failed.

November 17, 2026

- â¢You can now connect the Antigravity App to a remote 64-bit Linux machine over SSH from the sidebar, the project menu (SSH Host...), or the command palette (Connect to SSH Host...). Antigravity installs its remote server automatically over OpenSSH using your ~/.ssh/config so projects, Git worktrees, agents, and terminals run on the host and keep working even after you close the window.
- â¢GitHub pull request links in chat and URL artifact cards now render as live status pills with hover cards, alongside a new Pull Requests section in the Overview panel and an optional conversation sidebar indicator.
- â¢Finished /btw side-question answers now include an Insert into main conversation action, and you can start a side question from selected subagent text or promote a subagent tab to Full View when running /btw.
- â¢You can now search within pages in the built-in browser with Cmd/Ctrl+F from either the browser toolbar or the page itself, while web apps with their own search keep handling the shortcut.
- â¢You can now choose the Nord dark theme and matching syntax palette in Settings > Appearance.
- â¢Added syntax highlighting for R and R Markdown (.R, .Rmd, .Rprofile, .Renviron, and NAMESPACE), including YAML frontmatter and inline or fenced R code chunks.
- â¢Business and enterprise accounts can now use /swarm and open UI plugin panels from the side panel, Overview, New Tab, and title bar when allowed by organization admin settings.
- â¢Added Cloudflare, Grafana Cloud, SonarQube, incident.io, Better Stack, Tavily, and Levels.fyi to the partner plugin marketplace, and updated the Gemini API plugin and 15 partner plugins.

## Show more (29)Show less

- â¢You can now open and play audio and video files up to 200 MB in the side panel instead of hitting a 20 MB limit, and attach .amr voice recordings in Gemini conversations.
- â¢Text-heavy and dense PDFs now open and scroll faster in the side panel by deferring text-layer rendering, with a toolbar toggle to turn text selection on for very large documents.
- â¢Settings > Shortcuts now lists additional navigation, conversation, and layout shortcuts for customization and stops showing false conflict warnings for inactive bindings.
- â¢Messages sent immediately while the agent is working now appear as sent without pulsing and keep the turn above them steady on delivery. Agent response actions are now left-aligned below the status line, and a single running subagent opens from the status line in one step.
- â¢Improved performance when switching conversations, typing in long chats, scrolling large file diffs in Review, reverting turns, and playing the welcome intro.
- â¢Selected conversations now have stronger contrast in light themes, disabled Download and Export actions stay visible with a reason when a file cannot load, and shell commands run in the first listed project folder.
- â¢Device management policies in admin_settings.json can now lock sign-in and project settings, enforce permission ceilings, disable remote control, and restrict plugins or marketplace access.
- â¢If you use remote agents, editing env-setup.sh now shows Saved setup routine with an Open in settings link, project setup scripts are labeled Environment, and multiple open windows share a single cloud polling loop.
- â¢Fixed an issue where the Continue button on the Terms of Service dialog could be pushed off screen in shorter windows and where the Creating a Project tip overlapped the welcome dialog on first launch.
- â¢Fixed an issue where opening Antigravity offline or hitting a temporary network failure signed you out instead of keeping your saved session.
- â¢Fixed an issue where macOS could repeatedly show Keychain Not Found dialogs every hour after a keychain write failed.
- â¢Fixed an issue where Workforce Identity Federation (SSO) accounts had to re-enter their sign-in configuration after token expiry or an app restart.
- â¢Fixed an issue where refreshing models or restarting after an enterprise license was revoked left the model selector empty instead of reopening subscription selection, and where top-tier Google AI plans still showed Upgrade links.
- â¢Fixed an issue where, on Windows, timed-out Git status checks could leave background git.exe processes running and consuming gigabytes of memory.
- â¢Fixed an issue where, on Windows, custom agent, rule, and hooks.json files saved with a UTF-8 byte order mark were ignored, command hooks with quoted paths failed, and multi-line scheduled automation prompts were truncated to the first line.
- â¢Fixed an issue where attaching an empty text, Markdown, JSON, or HTML file caused that turn and every later message in the conversation to fail.
- â¢Fixed an issue where sent voice memos and audio attachments showed 0:00 without playing, and where removing an audio attachment in the composer transferred its playback state to the next recording.
- â¢Fixed an issue where dragging a file onto the edge padding of the message box failed with an editor error, and where moving up to another choice in a single-select question card erased your custom Other answer.
- â¢Fixed an issue where @ and / menus in a split-view new-conversation pane were cut off at the top edge, and where pressing Ctrl+D on Windows or Linux split the pane instead of canceling an active turn or prompt.
- â¢Fixed an issue where switching between View File and View Diff could leave the diff pane blank, where files over 500 KB showed a false trailing change, and where resizing the side panel briefly shifted Markdown scroll position.
- â¢Fixed an issue where custom file viewers from plugins and MCP apps still showed non-working Preview, Raw, Word Wrap, and Export as PDF controls in the side panel.
- â¢Fixed an issue where built-in browser approval prompts from the browser agent timed out after 60 seconds while you were away, where Comment and Quote missed text in same-origin iframes, and where the desktop app opened a local debug port on startup.
- â¢Fixed an issue where remote MCP servers using OAuth signed you out after an hour when tokens refreshed, where configured MCP headers were sent across cross-origin redirects, and where OAuth sign-in callbacks failed on IPv6-only machines.
- â¢Fixed an issue where empty internal plugin projects appeared in project pickers, where Create with Prompt on the Automations page could target a remote project instead of This Computer, and where /learn appended past rule file size limits.
- â¢Fixed an issue where Mark all as read failed if an older conversation file was missing on disk, where files opened in the side panel were missing from Recent Files, and where renaming a conversation in the sidebar lost keyboard focus.
- â¢Fixed an issue where long reasoning turns timed out after five minutes of thinking, where stale pooled network connections failed retries, and where a corrupt queued message file could block later messages in a conversation.
- â¢Fixed an issue where unanswered microphone permission prompts in Voice mode waited indefinitely instead of timing out with a retryable error.
- â¢Fixed an issue where organization file-access policies reset Always Proceed terminal settings on admin-managed accounts, and where hooks or wildcard rules could bypass enforced command or outside-project file review.
- â¢Fixed an issue where, in remote conversations, post-clone setup scripts left the status stuck on Cloning repositories or Copying files instead of showing Setting up environment.

### Antigravity CLI: Remote agent accept-edits mode, Orchestrator for Enterprise/ADC, and Remote Control updates

November 17, 2026

- â¢Added accept-edits mode to --remote-agent sessions: Shift+Tab now switches between the default mode and accept-edits instead of doing nothing. Sessions still start in the default mode, where the remote agent now asks before each file edit in its workspace, so you may see edit approval prompts you didn't get before; switch to accept-edits to let edits go ahead without asking.
- â¢Added the Orchestrator model to /model for Gemini Enterprise accounts and sign-ins with Application Default Credentials (AGY_ADC_AUTH): it plans with Gemini 4 Carbon (High) and hands the plan's execution to Gemini 4 Hydrogen (High), and it appears when you can select both of those models.
- â¢Added a conversation navigator to the Remote Control browser view: a rail with a mark for each of your prompts previews the prompt when you hover over it and jumps to it when you click, and [ and ] jump to the previous or next prompt.
- â¢Added a Quote button to the Remote Control browser view: select text in an agent response and click Quote to add it to your next message as a quote.
- â¢Added clickable links to the message box and your sent messages in the Remote Control browser view: a web address you type or paste opens with Cmd+click (Ctrl+click on Windows and Linux) or a middle-click, while a plain click still places the cursor.
- â¢Added the time left to timers the agent sets, in the Remote Control browser view: each running timer in Background Tasks shows a countdown such as 59m left, and when a timer is all that's running, the status line reads, for example, Waiting for 1h timer Â· 59m left.
- â¢Improved rate-limit handling: when the model API asks the CLI to wait before retrying, the CLI now waits and retries for delays of up to 4 minutes instead of stopping the turn when the delay is longer than 30 seconds, so per-minute rate and capacity limits no longer end your turn, while spend-cap, prepaid-balance, and weekly quota errors now stop the turn right away.
- â¢Improved /model when your plan offers more than one Orchestrator planner: the separate Orchestrator (Carbon) and Orchestrator (Argon) rows are now a single Orchestrator row with a Planner choice that you change with â/â or Tab, while --model and saved settings keep using the full names.

## Show more (20)Show less

- â¢Improved switching to a conversation that's already open in long sessions: /resume back to it and the switch after /fork no longer load the conversation's whole history first, so they no longer pause for a second or more, and /btw and /help side questions start faster too.
- â¢Improved memory use in long sessions: the CLI now keeps only recently used conversation steps in memory and reloads older ones from disk when it needs them, instead of holding every step for as long as the conversation is open, and it no longer loads every earlier subagent's full history at the start of each turn, which could take several GiB in sessions that ran many subagents.
- â¢Improved click-to-expand in the full screen (altscreen) rendering mode: items you can click, such as (ctrl+o to expand) hints, thoughts, expanded tool groups, and the pinned prompt, now highlight when you hover over them.
- â¢Improved the side panel in the Remote Control browser view: files and reviews open in tabs that come back when you reload the page, and a new Overview Panel, opened from the title bar or with Cmd+Option+O (Ctrl+Alt+O on Windows and Linux), lists the conversation's artifacts, modified files, subagents, and background tasks.
- â¢Changed the terminal sandbox to block sandboxed commands from reading ~/.config (or your XDG_CONFIG_HOME) and, on Windows, AppData\Roaming by default, since other programs keep settings and tokens there; previously they were readable. The settings of common developer tools such as Git, Go, npm, and Python stay readable, ~/.git-credentials is now blocked, and you can still allow other paths explicitly in your sandbox rules.
- â¢Fixed Auto mode asking for approval to read files outside the workspace even when Non-Workspace Access is turned on in /config; Auto mode now honors that setting.
- â¢Fixed long, high-effort model responses being canceled and restarted from scratch when the model went more than 5 minutes without sending anything while it was still thinking; the CLI now waits up to 10 minutes before it treats a silent response as stalled.
- â¢Fixed turns failing with model output must contain either output text or tool calls, or the conversation compacting far earlier than needed, after the agent read very large files, such as multi-megabyte PDFs, before the conversation's first compaction.
- â¢Fixed a rare race that could leave a new conversation unusable, with every message failing with trajectory not found even after a restart, when the conversation was created while the CLI was listing saved conversations, such as at startup or when an editor connected through agy acp refreshed its session list.
- â¢Fixed agy acp holding back what the agent did after a turn ended: when a background task or subagent finishes and wakes the agent, its replies, tool calls, and permission requests now reach the editor right away instead of at your next prompt, so the agent no longer waits on a permission request the editor never showed.
- â¢Fixed agy acp leaving a session's subagents and background tasks, such as dev servers, running after the editor canceled a prompt or closed the session, where they could keep working and wake the conversation again; canceling or closing now stops them, and closing does so even when no prompt is running.
- â¢Fixed signing in to a plugin right after installing it from /plugin, where pasting the code from its sign-in page could fail with "The sign-in link changed. Open the new one above, then paste its code."
- â¢Fixed plugins running while your organization turns them off with pluginsDisabled in managed admin settings: plugin MCP servers and other plugin processes no longer start before sign-in finishes, and they now stop as soon as an administrator turns plugins off during a session.
- â¢Fixed skills whose SKILL.md has no name in its frontmatter: they were all named SKILL, so only the first one loaded and the rest were silently dropped; each is now named after its folder, and agy plugin validate reports that name.
- â¢Fixed the agent writing LF line endings when it appends to a file that uses CRLF line endings, as is common on Windows, which left the file with mixed line endings and made later multi-line edits to it fail with target content not found; appended text now uses the file's own line endings.
- â¢Fixed OSC 7501 terminal status showing the session as done while the agent was paused waiting for background subagents or tasks to finish; the terminal's tab or window now shows it as working until that work ends, and daemon tasks such as dev servers don't count.
- â¢Fixed typing in the Remote Control browser view: with Chrome on Android, Backspace on the on-screen keyboard could stop deleting characters and later text could land in the middle of a word, and after typing with an input method or a phone keyboard's suggestions, Enter could add a new line instead of sending your message.
- â¢Fixed scrolling up through a long agent turn in the Remote Control browser view, where the view could jump back down instead of reaching your prompt.
- â¢Fixed the permission prompt in the Remote Control browser view letting the number or arrow keys select and submit an unavailable "always allow" option when one of your ask rules or a hook requires you to confirm each time; the keyboard now skips those options, as clicking already did.
- â¢Fixed the model selector in the Remote Control browser view switching to No Model Selected after a brief connection or server error; it now keeps your account and model list until the next successful refresh.

November 16, 2026

- â¢Side questions (/btw) now open in a dedicated Side Question tab in the side panel where you can ask multi-turn follow-up questions and let the agent read files, search your workspace, and run read-only commands without interrupting the main conversation.
- â¢Fixed an issue that could truncate diagnostic logs.

### Antigravity VS Code extension: Keybinding scheme selector, standardized shortcuts, voice dictation, file attachments, and inline diff fixes

November 16, 2026

- â¢Keybinding Scheme Selector: Added a Shortcuts settings page with VS Code Default and Antigravity (AGY) Style schemes and a native shortcuts link.
- â¢Standardized Commands & Shortcuts: Added keyboard shortcuts for conversation search, settings, model selection, history navigation, and canceling generation under consistent command titles.
- â¢Voice Dictation Support: Added microphone capture support in the chat input for dictating prompts directly inside the extension.
- â¢Context Menu Attachment: Added Antigravity: Add to Antigravity Chat to Explorer menus, editor tabs, and Ctrl+L / Cmd+L without a selection.
- â¢Proxy Certificate Fallback: Added fallback support for standard Node.js trusted certificate environment variables during extension sign-in and proxy connections.
- â¢Turn Navigation Shortcuts: Added keyboard shortcuts (Ctrl+Alt+[ / Ctrl+Alt+] or Cmd+Option+[ / Cmd+Option+]) to jump between conversation prompts.
- â¢Skill Pill Navigation: Updated skill pills so clicking a pill opens its definition file directly.
- â¢Chat File Drag-and-Drop: Fixed dragging and dropping local files, folders, and remote workspace items into chat.

## Show more (17)Show less

- â¢Image & Diagram Downloads: Fixed downloading agent-generated images and diagrams from the chat preview modal.
- â¢Editor Find Shortcuts: Fixed Alt+Enter and Option+Enter shortcuts in the Find widget and terminal being intercepted during active chats.
- â¢Startup Connection Reliability: Fixed an issue where premature startup failure screens could appear while loading.
- â¢External File Reverts: Fixed inline diff reviews staying active and causing save conflicts after reverting files externally.
- â¢Background Inline Diffs: Fixed files with deletions opening as unsaved tabs when auto-opening edited files is disabled.
- â¢Auto-Open Edited Files: Fixed newly edited files not opening automatically when the auto-open files setting is enabled.
- â¢Integration Sign-In Links: Fixed authentication links for extensions and MCP servers failing to open in the system browser.
- â¢Subscription Selection Recovery: Fixed an issue where refreshing models could get stuck instead of reopening subscription selection.
- â¢Offline Sign-In Persistence: Fixed an issue where transient offline startups could sign out saved sessions.
- â¢Follow-Up Message Delivery: Fixed follow-up messages in new workspace conversations queuing instead of sending immediately.
- â¢Narrow Sidebar Layout: Fixed header controls, greeting text, and the chat input clipping in narrow sidebars.
- â¢Mention Menu Positioning: Fixed @ and / suggestion menus clipping off-screen in top-docked new-conversation panels.
- â¢Question Input Drafts: Fixed custom text entered in single-select prompts clearing when switching between options.
- â¢Voice Permission Timeouts: Fixed voice capture hanging indefinitely when microphone permission prompts remain pending.
- â¢Voice Shortcut Focus: Fixed the voice dictation shortcut targeting the wrong input when comment boxes or side panes are open.
- â¢Screen Reader Accessibility: Fixed UI freezes on Markdown dividers and restored native link announcements for inline entities.
- â¢Addressed other minor user interface issues.

### Antigravity Visual Studio extension: Voice dictation support in the chat interface, and improved reliability when reviewing inline diffs.

November 16, 2026

- â¢Voice Dictation Support: Added microphone support for voice dictation in the chat interface.
- â¢Inline Diff Review: Fixed an issue where rejecting suggested code changes in the editor could fail.

### Antigravity SDK: Default model gemini-4-hydrogen, LiteRT vision and prefix caching, and cancellation cleanup.

November 16, 2026

- â¢Default Model (gemini-4-hydrogen) & ThinkingLevel.EXTRA_HIGH Mapping: Updated the SDK default model (DEFAULT_MODEL) from "gemini-3.8-flash" to "gemini-4-hydrogen" and mapped ThinkingLevel.EXTRA_HIGH to "xhigh" across Gemini API, Vertex AI, and Interactions API surfaces (normalizing per-request "extra_high" overrides in localharness). Override the default anytime via model or ModelTarget on LocalAgentConfig.from google.antigravity import (
- â¢On-Device Vision Support (LiteRTAgentConfig): Added automatic vision capability and hardware backend detection ("gpu" / "cpu") via litert_lm.ModelInfo (with optional vision_backend override) and OpenAI-to-LiteRT multimodal image translation (from_file, Image) for vision-capable .litertlm checkpoints such as Gemma 4 26B (requires litert-lm >= 0.19.1).from google.antigravity import Agent, LiteRTAgentConfig, from_file
- â¢LiteRT Multi-Turn Prefix Caching: Reuses the active LiteRT-LM conversation KV cache across sequential loopback turns when message prefixes and tool schemas match, prefilling only newly added prompts and tool responses.
- â¢LocalOpenAIConnection Usage, Latency, and Retry Meta Populates token usage (Usage) and latency metadata on streaming SSE chunks for OpenAI Chat Completions and Responses APIs, and parses HTTP Retry-After headers to support automatic retries on transient 429 and 5xx responses.
- â¢Tool Media Return Values (struct_converter): Added support for returning SDK media primitives (Image, Audio, Video, Document) and Pydantic models directly from custom tools, serializing them as Content protos while summarizing inline binary media cleanly in JSON step content.
- â¢Callable Tool Wrapping (ToolRunner): Preserved underlying function names, docstrings, and deferred stringified type hints (from __future__ import annotations) when wrapping functools.partial callables and callable class instances as tools.
- â¢Reliable Subprocess and Transport Cleanup on Cancellation: Session startup, cancellation via asyncio.CancelledError, and disconnection now reliably terminate and reap local runtime subprocesses and close stdio pipes and WebSocket or loopback transports without blocking on non-UTF-8 stderr or child file descriptors. Non-zero runtime exits during teardown now surface AntigravityExecutionError directly (resolving GitHub issue #279).import asyncio
- â¢LiteRT Thought Channel Streaming: Resolved an issue where model thought tokens were omitted from LiteRT responses by emitting the thought channel as reasoning and reasoning_content fields in streaming SSE deltas and synchronous payloads (GitHub issue #267).

## Show more (5)Show less

- â¢LocalOpenAIConnection Error Classification: Non-200 HTTP responses now surface as AntigravityConnectionError (consistent with Gemini local connections) rather than generic execution errors, marking transient 429 and 5xx errors as retryable.
- â¢ToolCall.canonical_path Path Traversal Normalization: Wire path normalization now resolves . and .. segments for file:// URIs and plain paths so traversal paths cannot bypass prefix-based workspace checks in user hooks.
- â¢Conversation Compaction Tracking: Conversation.compaction_indices now records only root-level (depth == 0) StepType.COMPACTION steps once they reach StepStatus.DONE, ignoring in-progress ACTIVE updates and subagent compactions (depth > 0).
- â¢Multi-Turn ChatResponse & Conversation State Isolation: Isolated per-turn response state by snapshotting structured_output(), usage_metadata, and stop_reason on turn completion, scoping Conversation.get_last_structured_output() to root (depth == 0) steps of the current turn, re-raising stream errors on repeat structured_output() calls, and draining any active unconsumed ChatResponse before starting a new turn.
- â¢Documentation & Examples: Fixed the types.AntigravityCancelledError import in the cancellation guide, updated prioritized inference documentation to pass GeminiAPIEndpoint via ModelTarget, clarified default tool permissions (confirm_run_command gating run_command) in README.md, and updated Gemini documentation URLs to aistudio.google.com.

### Antigravity CLI: Added run_workflow tool, admin pluginControls, faster Auto mode reviews, and MCP security fixes

November 14, 2026

- â¢Added the run_workflow tool for everyone: the agent can run a Python workflow script that fans work out to many subagents in parallel or in stages and then combines their results. It uses the Python bundled with the CLI, so you don't need Python installed.
- â¢Added a pluginControls section to managed admin settings, so organizations that don't use Gemini Enterprise can control plugins too: pluginsDisabled stops all plugins from loading, including built-in ones, and blocks installing or enabling them, while marketplaceDisabled turns off the plugin marketplace but keeps installed plugins working. Gemini Enterprise users keep following their organization's Cloud Console policy.
- â¢Improved Auto mode's review of commands that would leave the terminal sandbox, so it decides faster and times out less often: the reviewer can now search the conversation, list directories, and search files directly instead of starting a sandboxed command for each check.
- â¢Improved /resume: the conversation picker no longer fetches the full history of every open conversation, so it no longer stalls on "Loading conversations..." in long sessions, and /resume now accepts any prefix that matches exactly one conversation, such as the short ID shown in the status line, and reports conversation not found for an unknown ID instead of silently opening the picker.
- â¢Improved /browser subagent session recordings, which are now .mp4 files made with Chrome's built-in screen recording instead of .webm.
- â¢Fixed a security issue where custom headers configured for an HTTP MCP server, such as API keys, could be forwarded to a different host when the server responded with a redirect; they are now sent only to the configured server's own origin.
- â¢Fixed the terminal UI accepting malformed or implausible window-size reports, such as negative or extremely large dimensions from stray escape sequences, which could make the CLI freeze or use excessive memory; these reports are now ignored.
- â¢Fixed several agy acp issues: a session no longer fails with "Authentication required" after agy acp --login when settings.json names an auth method that has no credential (such as gemini-api-key without GEMINI_API_KEY), because the CLI now falls back to your saved login; prompts no longer fail when the ACP client passes an MCP server whose name contains spaces or other special characters; a failed turn now reports its specific error, such as a model or quota error, instead of "Agent execution terminated due to error."; and the diffs sent for completed edits no longer carry an extra trailing newline in oldText and newText.

## Show more (11)Show less

- â¢Fixed more cases of files saved with a UTF-8 byte order mark (BOM) being mishandled: "modelProvider" in settings.json is now honored on the first run, agy acp now loads your settings and authentication options instead of falling back to defaults, and agy plugin validate now runs all of its checks on such a hooks.json and reports the real syntax error and line instead of blaming the BOM.
- â¢Fixed -i/--prompt-interactive together with --remote-agent and --conversation or --continue: the prompt is now sent to the resumed conversation after its history loads, instead of starting a new conversation, and it is handled like typed input, so it shows right away, waits behind a running turn, and reports a failed send.
- â¢Fixed ! shell commands on Windows: PowerShell no longer drops double quotes, so commands like ! git commit -m "two words" run exactly as typed, and quitting the CLI or killing the task in the tasks panel now also ends the programs the command started instead of leaving them running and the task marked as running.
- â¢Fixed /copy and other clipboard actions on Windows over SSH: the copy now reaches your local clipboard, and the copied text is no longer written to the CLI log file.
- â¢Fixed scheduled automations on Windows receiving only the first line of a multi-line prompt; each scheduled or manually triggered run now gets the full prompt.
- â¢Fixed the macOS "Keychain Not Found" dialog reappearing about once an hour: after a keychain write fails, the CLI now saves your sign-in token to its file-based storage for the rest of the session instead of retrying the keychain on every token refresh.
- â¢Fixed a race where reloading MCP servers while several subagents were starting could launch duplicate MCP server processes, leaving some subagents with broken MCP tools or failed sign-ins.
- â¢Fixed MCP server and plugin OAuth sign-in on machines without an IPv4 loopback address, such as IPv6-only VMs; the sign-in callback now falls back to [::1] when 127.0.0.1 is unavailable instead of failing silently.
- â¢Fixed signing in to a plugin from /plugin sometimes reporting that the sign-in didn't finish when it had just succeeded.
- â¢Fixed tool groups jumping around when expanded with Ctrl+O: the collapse hint now stays on the same line as the expand hint, and in low verbosity the token summary line no longer disappears from the expanded view.
- â¢Fixed the model selector in the Remote Control browser view on phones, where the list could not be scrolled by touch past the first screen, so models near the end of the list could not be selected.

November 13, 2026

November 13, 2026

- â¢You can now use Dynamic Workflows to orchestrate parallel subagents and multi-step tasks in Python using the Python runtime bundled with the app.
- â¢You can now choose Everforest Light and Everforest Dark in theme settings for warm, lower-contrast reading and editing.
- â¢You can now jump between previous and next prompts in a conversation using keyboard shortcuts from the composer or reading view, or customize them in Settings.
- â¢You can now bind an optional Submit Prompt shortcut in Settings > Shortcuts to send dictated or drafted prompts without pressing Enter.
- â¢You can now copy a sign-in link or paste a one-time authorization code during sign-in when an automatic browser redirect does not complete.
- â¢The model selector menu now expands to fit available screen height and displays a Refresh button whenever models fail to load.
- â¢Agent-controlled browser tabs now notify the agent and show a blocked status in the download drawer when a download is rejected for its file type.
- â¢Cross-site frames inside agent-controlled browser tabs now load signed out, and stay hidden from the agent's screenshots and page snapshots unless explicitly allowed and targeted.

## Show more (16)Show less

- â¢Installed plugin cards and detail headers now display MCP server startup errors when a connection fails to start.
- â¢Permission menus now explain when Default and Auto modes are hidden because terminal sandboxing is unavailable on the host.
- â¢If you use remote agents, you can now clone Cloud GitHub repositories locally from the workspace picker and sign in to pull request links directly from the app.
- â¢Fixed an issue where some websites blocked the built-in browser because its user agent exposed an Electron token.
- â¢Fixed an issue where quitting the desktop app could leave background processes running and where temporary file locks could prevent settings from saving.
- â¢Fixed an issue where full-page plugin views turned blank or broke open tabs after restarting or updating a plugin.
- â¢Fixed an issue where selecting the Sign in to Google Drive mention suggestion did not start sign-in.
- â¢Fixed an issue where the Pin Conversation keyboard shortcut conflicted with Chrome tab groups on Linux and could not be rebound in Settings.
- â¢Fixed an issue where filtering by Automations in the sidebar could hide plugin conversations.
- â¢Fixed an issue where editing a queued message, reverting a conversation step, or restoring a draft flattened lists and dropped link pills.
- â¢Fixed an issue where scrolling up through long conversations could stall or lose your place by loading smaller history pages earlier.
- â¢Fixed an issue where bold Chinese, Japanese, and Korean text rendered at regular weight under the default font.
- â¢Fixed an issue where opening a file from the Open File menu opened as a temporary preview tab that got replaced by the next file.
- â¢Fixed an issue where reverting a conversation caused the last response toolbar and cards to briefly flicker away.
- â¢Fixed an issue where long lines of hyphens in text could freeze accessibility announcements.
- â¢Fixed an issue where the Saved confirmation for project shell setup scripts disappeared immediately after saving.

### Antigravity CLI: Gemini 4 Hydrogen & Carbon models, click-to-expand in full screen mode, and security fixes.

November 13, 2026

- â¢Added Gemini 4 Hydrogen (Low, Medium, High, and XHigh) and Gemini 4 Carbon (Low, Medium, and High) to /model and --model when you connect with a GEMINI_API_KEY, added Gemini 4 Hydrogen (Low, Medium, High, and XHigh) when you sign in with Application Default Credentials (AGY_ADC_AUTH), and added support for Gemini 4 Hydrogen through an enterprise model gateway.
- â¢Added click-to-expand in the full screen (altscreen) rendering mode: clicking a tool group, a thought, a prompt's attached images, or multi-line command output now expands or collapses just that item, so you no longer have to use Ctrl+O. The default Rendering Mode is unchanged.
- â¢Added context window usage updates to agy acp sessions, so editors such as JetBrains AI Chat can show how much of the model's context window the conversation is using; the value is sent when a session starts or is reloaded, after each model call, and after a model or thinking level change.
- â¢Added support for two admin settings: gateway.routeWebFetch stops the agent's web page reads from going around an enterprise model gateway by disabling URL reading unless a proxy.address is configured, and featureControls.remoteControlEnabled lets an organization turn off Remote Control. With an enterprise model gateway, web pages are now fetched directly from your machine (through proxy.address when set) instead of through Google's fetch service.
- â¢Improved the first message of a conversation in --remote-agent sessions, which no longer waits for its remote environment to start or resume: the CLI now prepares a new conversation's environment as soon as the session knows its remote project, and again after /new or switching projects, and starts a resumed conversation's idle environment in the background as soon as it opens with --continue or --c.
- â¢Fixed a security issue in the terminal sandbox's network proxy where a command could get around path-based network rules, such as those limiting where credentials are sent or which Git and API paths are allowed, by using percent-encoded slashes (%2F), backslashes, or .. segments in a URL path; every way of reading the path is now checked against the rules, and the cleaned-up path is the one sent upstream.
- â¢Fixed always-allow command rules on Windows so they can no longer be bypassed when the shell is cmd, cmd.exe, powershell.exe, pwsh.exe, or a full path to one of them. Commands run in PowerShell or Command Prompt now always need an exact match, because their quoting rules differ from POSIX shells. A command with hidden extra statements, such as git status '&& del secret.txt', could otherwise be approved by a rule meant only for git status.
- â¢Fixed permission rules for MCP tools, such as an mcp(server/tool) deny or ask rule, being ignored for MCP tools that are loaded up front (marked eager in mcp_config.json) and for the /browser subagent's Chrome DevTools tools; these tools now ask for approval or are blocked according to your rules, like other MCP tools.

## Show more (19)Show less

- â¢Fixed /plugin, agy plugin, and headless -p runs ignoring your organization's Gemini Enterprise admin controls for plugins: when an administrator disables plugins, installing, importing, or enabling one now fails with plugins are disabled by your organization, and when the plugin marketplace is disabled, marketplace catalogs are not fetched and marketplace installs fail; listing, disabling, and uninstalling plugins still work.
- â¢Fixed hook commands on Windows that contain double quotes, such as type "C:\my dir\deny.json", failing and being silently skipped, which let a PreInvocation or PreToolUse hook that should block the agent go unenforced; hook commands are now passed to cmd.exe exactly as written.
- â¢Fixed files saved with a UTF-8 byte order mark (BOM), as Windows PowerShell, Visual Studio, and Notepad's "UTF-8 with BOM" option do, being silently ignored: settings.json and files passed to --settings are no longer rejected as malformed, a hooks.json no longer leaves /hooks empty with none of its hooks running, and custom agent and rule files no longer go missing from /agents and the agent picker or lose their frontmatter trigger.
- â¢Fixed opening your external editor on Windows when EDITOR or the Editor setting is a normal Windows path such as C:\Tools\ed.cmd or "C:\Program Files\Vim\vim.exe"; backslashes were dropped, so Ctrl+G, /open, /keybindings, and artifact review failed with an editor ... not found in PATH error.
- â¢Fixed ! shell commands hanging forever with no output when the command wrote a lot to stderr, which was easy to hit on Windows with build tools that print progress or warnings to stderr; such commands now finish and show their stdout and stderr lines in the order they were written.
- â¢Fixed agent turns failing when a network connection silently went stale or the network dropped a shared HTTP/2 connection, for example with http2: client connection force closed via ClientConn.Close or after a socket write error; model requests now retry on a fresh connection and, if retries run out, show the CLI's network error message. Slow model responses also no longer time out after 60 seconds when an organization's admin settings configure a proxy or custom CA certificate.
- â¢Fixed the CLI sometimes never exiting after a session, and background requests such as generating a conversation title, worktree name, or commit message hanging, when the model answered one of those requests with an unexpectedly long reply.
- â¢Fixed a single empty or corrupted queued message file, for example one left behind by an unclean shutdown, blocking every later message to the agent, so background task completions, timers, and subagent replies no longer silently stop reaching it; the unreadable file is set aside and the remaining messages are delivered.
- â¢Fixed remote MCP servers that sign in with OAuth without a client secret signing you out as soon as the access token expired, typically after an hour; token refreshes now send the client ID the way the server requires, so you stay signed in, including with sign-ins saved by earlier versions.
- â¢Fixed the agent telling you to click a Sign in button or use an Accounts tab, which the CLI doesn't have, when a plugin or plugin MCP server needs you to sign in; it now tells you to sign in with /plugin and then send a message to continue, and /mcp shows the same /plugin guidance for a server that is waiting on a sign-in.
- â¢Fixed switching a conversation from a Gemini model to a custom or admin-managed non-Gemini model, such as Claude, failing with an Invalid signature in thinking block error.
- â¢Fixed /btw and /help side questions failing with stream timed out after 5m0s while the side question was still working: they now run until they finish or you press Esc, which the Thinking... line and the "already in progress" message now mention, and a side question that needs an approval the card can't show stops right away and tells you to ask in the main conversation instead.
- â¢Fixed /learn repeatedly appending to rule files such as GEMINI.md or AGENTS.md until they grew past the rule size limit, after which their endings were silently cut off on every turn. Before updating a rule, /learn now checks the documented size limits, and when an update would get close to or exceed them, it proposes condensing existing entries, splitting them into a scoped rule, or moving step-by-step guidance into a skill.
- â¢Fixed follow-up turns in a --json-schema conversation resumed with --conversation ending on a plain-text reply with no structured result once an earlier turn had produced one; each new message now gets its own reminders to return the structured output, up to the usual limit per message.
- â¢Fixed /rewind failing with a step_index out of bounds error when you pressed Esc while a turn was still starting and then rewound to remove that message.
- â¢Fixed /diff showing a made-up change at the end of a turn's diff for files larger than 500 KB when an earlier edit changed the file's length; the two versions are now cut off at the same line, so the preview shows only real edits.
- â¢Fixed / search in the artifact viewer missing text split across different styles, such as a sentence containing a number or inline code, and leaving the rest of a styled line unstyled after a highlighted match; the current match is also now easier to pick out from the other matches in every color scheme.
- â¢Fixed status messages such as "Copied 3 lines (42 characters)" after selecting text, settings save errors, and permission reload notices never appearing when you use a custom status line; they now briefly replace your custom status line, which comes back once the message clears, without shifting the layout.
- â¢Fixed the in-progress spinner line freezing until you restarted the CLI after you logged out while the agent was working and then signed back in; it now keeps animating.

### Antigravity CLI: Default Agent Mode set to Auto, UI freeze fixes, and improved permission enforcement.

November 12, 2026

- â¢Changed the default Agent Mode from Default to Auto: sessions with no saved mode, including headless -p runs, now start in Auto wherever Auto mode is available and in Default elsewhere; to keep the previous behavior, choose Default for Agent Mode in /config, which saves "agentMode": "" to settings.json. A Tool Permission setting of strict now also takes precedence over Auto and accept-edits modes, so file edits still ask for approval under strict.
- â¢Fixed multi-second UI freezes, which also stalled streaming and window resizing, when a command printed a very long line without spaces; line wrapping now takes time proportional to the line's length.
- â¢Fixed agent replies stalling for 20 seconds or more while a group of tool calls with long output was still animating its token counts; streamed steps now keep appearing while the group re-renders.
- â¢Fixed the agent not noticing that a command it moved to the background was waiting for input, such as a password or y/n prompt printed before the command went to the background; the agent is now notified once that output settles instead of waiting silently.
- â¢Fixed browser sign-in on Windows sometimes failing to complete because the CLI's local sign-in callback listened on only IPv4 or IPv6 while the browser redirected over the other; the callback now listens on both.
- â¢Fixed Gemini 4 Carbon requests through an enterprise model gateway failing with an HTTP 400 Thinking budget is not supported for this model error; the CLI now sends Gemini 4 Carbon a thinking level and no longer leaves the Low, Medium, or High suffix in the model ID it sends to the gateway.
- â¢Fixed a PreToolUse hook returning auto_approve, or a wildcard command(*) allow rule, bypassing an organization's admin-enforced review: commands that would leave the terminal sandbox, commands the admin policy holds for review, and file reads or writes outside the workspace under an "always ask" file-access policy now always ask for your approval.
- â¢Fixed saved command allow and deny rules being ignored, so that every command asked for approval, when an organization's admin policy requires commands to run in the terminal sandbox but the machine has no OS sandbox; your saved rules now apply and unlisted commands still ask before running.

## Show more (2)Show less

- â¢Fixed background task details from manage_task losing their formatting when the task's recent output, or input sent to the task, contained Markdown code blocks or runs of backticks.
- â¢Fixed Remote Control showing a link that could not be opened at the end of some agent replies, such as the swarm coordinator's link to the swarm UI; Remote Control now leaves these links out.

### Antigravity IDE: Clickable terminal context pills, quick-access menus, clearer diff review commands, and multi-window sign-in sync

November 12, 2026

- â¢Added clickable terminal context pills: send a terminal selection or output to the chat (Ctrl+L on Windows and Linux, Cmd+L on macOS), see a snippet preview in the composer, and click the pill to jump back to the terminal it came from.
- â¢Added quick-access menus for Antigravity actions in the status bar, in the editor right-click menu, and in the Agent panel title bar (New Conversation, Provide Feedback, and More Actions...).
- â¢Renamed diff review commands to show what they apply to (Focused Change, Current File, or All Files) and hid commands that don't apply to the current editor.
- â¢Fixed diff review tracking when you queue a follow-up prompt while the agent is still editing files.
- â¢Fixed false "The content of the file is newer" save conflicts and stuck unsaved tab indicators when reviewing inline diffs with Auto Save or manual save.
- â¢Fixed inline diff reviews on new files stripping trailing newlines or changing CRLF line endings.
- â¢Fixed side-by-side diff reviews and the Files With Changes bar showing +0 -0 for replaced lines.
- â¢Fixed signing in, signing out, or switching accounts in one window leaving other windows showing the old account until reload.

## Show more (1)Show less

- â¢Fixed startup failures on Windows when the chosen port fell in a Hyper-V, WSL2, or system-reserved port range.

November 11, 2026

November 11, 2026

- â¢Fixed an issue where, on Windows, signing in to Gemini Enterprise with a Workforce Identity Federation account could drop the license you selected, switch your plan to Agent Platform, and make requests fail with permission errors.

### Antigravity CLI: Swarm board visuals, smarter Auto mode sandbox checks, Stop hook continuation, and gateway fixes

November 11, 2026

- â¢Added the swarm's queen bee to the /swarm board header and a ð beside each live member in the Roster; in terminals with Kitty graphics, such as Kitty and Ghostty, each member gets its own colored bee card that flaps while that member is working, and other terminals, including tmux, show the queen as pixel art when the window is at least 60Ã35.
- â¢Added a continuation_active field to the JSON input of Stop hooks, which is true while the agent is already working on a continuation an earlier Stop hook requested, so a hook such as "run the tests before finishing" can tell it has already asked and avoid looping.
- â¢Added a supportsThoughtCirculation option to admin-managed model entries (models.list[] in admin settings), so organizations can let non-Gemini models behind an enterprise model gateway reuse their reasoning from earlier turns; when it is unset, the model's default applies.
- â¢Improved Auto mode's review of commands that would leave the terminal sandbox so it asks less often on safe reads: GitHub API reads, fetches into an existing clone, and single-file downloads such as raw.githubusercontent.com URLs no longer get the repository popularity, age, and release checks that cloning a repository still gets, host names that start with a digit are no longer mistaken for IP addresses, and reading ~/.ssh/config, known_hosts, authorized_keys, or public keys (*.pub) is no longer treated as reading secrets, while private keys still are and sending any .ssh file elsewhere is still blocked.
- â¢Improved agy plugin validate to warn when a plugin hook command uses ${PLUGIN_ROOT} or $PLUGIN_ROOT, which older CLI versions do not set, and to suggest a relative path instead, since hooks already run in the plugin's folder; the warning fails validation only with --strict.
- â¢Changed pay-as-you-go Google Cloud accounts that sign in with Application Default Credentials (AGY_ADC_AUTH) to use the same permission settings as other pay-as-you-go accounts, which makes Auto mode available to them; Gemini Enterprise accounts keep following their organization's admin-controlled permission settings, including their unsandboxed(...) deny and ask rules, and are not offered Auto mode.
- â¢Fixed undo and redo in the prompt, including u in Vim mode, moving the cursor to the end of the input; the cursor now returns to where the undone or redone edit was made.
- â¢Fixed agy plugin install ignoring the skill folders that a Claude Code plugin lists in the "skills" field of its manifest; those skills are now installed and loaded, and the install summary warns about listed paths that are missing, contain no skill, or point outside the plugin.

## Show more (7)Show less

- â¢Fixed agy plugin install and agy plugin import leaving a half-copied plugin that still loaded as enabled when an install failed partway; installs are now swapped in all at once and the previous version is restored on failure, plugins containing symlinks that point outside the plugin, FIFOs, or device files are refused, and a command whose file name cannot be used as a skill folder name is skipped with a warning instead of failing the import.
- â¢Fixed pressing esc in a swarm member's conversation opened from /swarm board with o dropping you back at the prompt; it now returns to the board with the same selection, filter, focused pane, and zoom.
- â¢Fixed a swarm coordinator sometimes ending its turn with a one-line filler reply instead of its real answer after being sent back to re-read board posts it had already read, fixed a coordinator offering a sidecar:// board link that the CLI cannot open instead of /swarm board on later turns of a swarm, and fixed agy -p runs that start a swarm pointing you to /swarm board, which headless runs cannot open.
- â¢Fixed some models served through an enterprise model gateway being sent requests without native tool declarations, which kept the agent from calling tools reliably; every model in the gateway's catalog, including models listed in AGY_LLM_GATEWAY_MODELS, now gets native tool calls.
- â¢Fixed custom Gemini models served through an enterprise model gateway or an admin model list, such as tuned models, cutting off long edits and file writes at 8,192 output tokens; they now default to the same 65,535-token output limit as built-in Gemini models, and an explicit admin maxOutputTokens value still takes precedence.
- â¢Fixed duplicate approval prompts and sandbox errors for organizations that enforce sandbox mode on machines without an OS sandbox, such as some Windows hosts or Linux with user namespaces disabled; commands now ask for review once and then run.
- â¢Fixed editing project rules with /permissions clearing the marker that records the project's permission rules as already migrated, which made the Antigravity app migrate them again and could drop command(...) allow rules when the sandbox is on.

### Antigravity CLI: Added agent swarms preview, /swarm board, Gemini 4 Carbon support, and plugin import fixes.

November 10, 2026

- â¢Added swarms as a preview: start one with /swarm and the agent becomes a coordinator that adds worker agents, each in its own conversation, which work on the task in parallel and collaborate through a shared message board; workers can be forked from an existing member's conversation, run on any model in the /model picker, and pause instead of retrying when their model runs out of quota, and a monitor worker alerts the coordinator when the whole swarm stalls. /swarm is labeled (Preview & Quota Heavy) in the / menu because every worker is a separate agent that uses your quota.
- â¢Added /swarm board, a full-screen live view of the current swarm that also opens with alt+s (rebindable as swarm.board), with a Roster of members showing each one's state and model, a Posts feed, and a Message pane that renders the selected post as Markdown; tab and shift+tab move between panes, z zooms the focused pane, enter shows or hides the message, [ and ] step through posts, p filters the feed to one member's posts and the posts that mention them, o opens a member's subagent details, f follows new posts, and esc backs out.
- â¢Added a â Swarm() row to the active-items tray below the prompt while a swarm runs, showing the latest post and the post count and then Done when the swarm finishes; press Enter on the row to open /swarm board, or alt+x (swarm.dismiss) to dismiss it.
- â¢Added the ANTIGRAVITY_DISABLE_EMBEDDED_RIPGREP environment variable, which you can set to true to stop the CLI from extracting and running its bundled ripgrep, for example when endpoint security software blocks it, so searches use a ripgrep on your PATH or the built-in search instead.
- â¢Added Gemini 4 Carbon (Low, Medium, and High) to /model and --model when you sign in with Application Default Credentials (AGY_ADC_AUTH).
- â¢Changed enterprise model gateway URLs to require https:// unless the gateway runs on the same machine (localhost, 127.0.0.0/8, or ::1); a remote gateway configured with a plain http:// URL is now rejected with an error instead of receiving your prompts unencrypted.
- â¢Fixed agy plugin import turning Claude Code plugin commands into skills that did not load or were all named SKILL; each command now becomes a skill named after its folder, including commands with no frontmatter or with Claude Code-style values such as argument-hint: [base branch] [focus], and agy plugin validate no longer warns about the leftover commands/ folder.
- â¢Fixed agy plugin import copying certain plugin's hooks/hooks.json unchanged, which caused the plugin's hooks to be skipped; PreToolUse, PostToolUse, Stop, and startup SessionStart hooks are now converted, with tool names in matchers mapped to built-in tools (for example Bash to run_command) and a default 60-second timeout, and hooks with no equivalent are listed as warnings in the import summary.

## Show more (5)Show less

- â¢Fixed agy -p headless runs sometimes never exiting after printing their result when a background task, such as conversation title generation, got stuck; the CLI now finishes shutting down within 30 seconds.
- â¢Fixed /teamwork writing run state into .agents/teamwork/ in your working directory, where two runs in the same workspace could overwrite each other; each run now keeps its state in its own folder under the CLI's app data directory.
- â¢Fixed links to files in agent responses showing only the file name, which made the file's location impossible to tell in terminals that cannot open clickable links, such as Terminal.app, screen, or over SSH; labels now show the path relative to your working directory, relative to ~/, or the absolute path.
- â¢Fixed enterprise model gateways that validate the request format rejecting every request, because the CLI sent a non-standard sessionId field in Gemini-format requests.
- â¢Fixed subagents and swarm workers that get their own copy of a Git repository starting from the last commit and losing your uncommitted work; their worktree now includes your staged, unstaged, and untracked changes, and agents outside version control can give a subagent or worker an isolated copy of the workspace folder, for folders up to 100,000 entries and 4 GiB.

### Antigravity CLI: Auto mode for ACP sessions, enriched hook JSON input, faster sandbox reviews, and stability fixes

November 10, 2026

- â¢Added Auto mode to agy acp sessions, so editors can switch a session to auto wherever the CLI itself offers Auto mode; it approves edits inside the workspace and runs commands in the terminal sandbox, and still asks before commands that would leave the sandbox unless Auto mode's review clears them.
- â¢Added hookEventName, cwd, and toolCall.id to the JSON input that hooks receive, so a single hook command can handle several events, knows the agent's working directory, and can match a PostToolUse call to its PreToolUse call.
- â¢Improved Auto mode's review of commands that would leave the terminal sandbox so it decides faster and times out less often, by using a lighter-thinking Gemini Flash model for the review.
- â¢Fixed agy acp sessions dropping back to the default mode when an editor reloaded or resumed an open session, so a session in accept-edits no longer starts asking for permission on file writes again.
- â¢Fixed agy acp turns hanging when a tool call needed a second permission approval, such as a command that asks again after its first approval; the editor now receives a new permission request instead of the turn waiting forever.
- â¢Fixed crashes and freezes with subagents: the CLI no longer crashes when a subagent's question or approval prompt is drawn in a very narrow terminal or before the terminal size is known, a subagent's question no longer closes by itself while you are answering it, and opening a subagent's view no longer occasionally freezes the CLI.
- â¢Fixed PgUp and PgDn in the conversation skipping text: paging down could jump past an entire message when a page ended exactly on a message boundary, and in full screen (altscreen) rendering mode the lines hidden under the pinned prompt header were scrolled past without ever being shown.
- â¢Fixed a thought that follows a tool's result line, such as Read 22 lines, being indented as if it were part of that tool call in the expanded view (Ctrl+O).

## Show more (3)Show less

- â¢Fixed the remote-control background service staying disconnected for good when it started before the network was up, such as right after a reboot, and stopped it from printing a sign-in link it could not complete when checking the saved sign-in only hit a network error.
- â¢Fixed the selected Gemini Enterprise plan being lost, and requests failing with permission errors, when the stored sign-in grew too large for the Windows Credential Manager or macOS Keychain; sign-ins that were already affected are repaired automatically the next time they load.
- â¢Fixed the Sign-in update needed warning, which asks you to run /logout and sign in again when your sign-in is missing a newly required permission, not appearing in --remote-agent sessions, where only a cloud conversations are not enabled error was shown.

### Antigravity JetBrains extension: Multi-root workspace support, per-turn token breakdowns, expanded model options, and MCP tool and sign-in fixes

November 10, 2026

- â¢Multi-Root Workspace Support: Added support for secondary project folders in multi-root workspaces across file tools, project skills, and session hooks.
- â¢Per-Turn Token Breakdowns: Added end-of-turn token usage summaries covering input, output, thinking, and cached tokens.
- â¢Context Window Reporting: Enabled immediate context window capacity updates when starting a session or switching models and thinking levels.
- â¢Expanded Model Options: Expanded available model options across supported clients.
- â¢MCP Tool Attribution: Fixed MCP tool calls missing server and tool names in permission prompts, live streaming, and session history.
- â¢Linux Sign-In Reliability: Fixed an issue on Linux where opening the browser during sign-in or running terminal commands could fail.
- â¢IPv4 Environment Startup: Fixed an issue where the agent service failed to start in IPv4-only container and workstation environments.
- â¢Sign-In Error Clarity: Fixed an issue where sign-in error messages displayed blank details when no verification link was needed.

## Show more (1)Show less

- â¢Session Cleanup Reliability: Fixed background subprocesses remaining open when session startup failed or disconnection was canceled.

### Antigravity Zed extension: Multi-root workspace support, per-turn token breakdowns, expanded model options, and MCP tool and sign-in fixes

November 10, 2026

- â¢Multi-Root Workspace Support: Added support for secondary project folders in multi-root workspaces across file tools, project skills, and session hooks.
- â¢Per-Turn Token Breakdowns: Added end-of-turn token usage summaries covering input, output, thinking, and cached tokens.
- â¢Context Window Reporting: Enabled immediate context window capacity updates when starting a session or switching models and thinking levels.
- â¢Expanded Model Options: Expanded available model options across supported clients.
- â¢MCP Tool Attribution: Fixed MCP tool calls missing server and tool names in permission prompts, live streaming, and session history.
- â¢Linux Sign-In Reliability: Fixed an issue on Linux where opening the browser during sign-in or running terminal commands could fail.
- â¢IPv4 Environment Startup: Fixed an issue where the agent service failed to start in IPv4-only container and workstation environments.
- â¢Sign-In Error Clarity: Fixed an issue where sign-in error messages displayed blank details when no verification link was needed.

## Show more (1)Show less

- â¢Session Cleanup Reliability: Fixed background subprocesses remaining open when session startup failed or disconnection was canceled.

### Antigravity Xcode extension: Multi-root workspace support, per-turn token breakdowns, expanded model options, and MCP tool and sign-in fixes

November 10, 2026

- â¢Multi-Root Workspace Support: Added support for secondary project folders in multi-root workspaces across file tools, project skills, and session hooks.
- â¢Per-Turn Token Breakdowns: Added end-of-turn token usage summaries covering input, output, thinking, and cached tokens.
- â¢Context Window Reporting: Enabled immediate context window capacity updates when starting a session or switching models and thinking levels.
- â¢Expanded Model Options: Expanded available model options across supported clients.
- â¢MCP Tool Attribution: Fixed MCP tool calls missing server and tool names in permission prompts, live streaming, and session history.
- â¢Linux Sign-In Reliability: Fixed an issue on Linux where opening the browser during sign-in or running terminal commands could fail.
- â¢IPv4 Environment Startup: Fixed an issue where the agent service failed to start in IPv4-only container and workstation environments.
- â¢Sign-In Error Clarity: Fixed an issue where sign-in error messages displayed blank details when no verification link was needed.

## Show more (1)Show less

- â¢Session Cleanup Reliability: Fixed background subprocesses remaining open when session startup failed or disconnection was canceled.

### Antigravity Eclipse extension: Terminal context in chat, changelog viewer menu, external link handling, and editor persistence fixes

November 10, 2026

- â¢Terminal Context: Added support for referencing terminal and console output in chat conversations.
- â¢Changelog Navigation: Added a View Changelog option to the Antigravity menu to view extension release notes.
- â¢External Link Handling: Added support for opening web links and email addresses from extension views in the system browser.
- â¢Theme Synchronization: Fixed an issue where switching themes did not reliably update across all open extension views.
- â¢Artifact Tab Restoration: Fixed an issue where open artifact tabs could restore as empty tabs following an unexpected restart.
- â¢Feedback Options: Fixed an issue where the feedback menu option was incorrectly displayed.
- â¢Addressed other minor user interface issues.

November 9, 2026

- â¢You can now use /swarm to coordinate multiple subagents over a message board to complete long-horizon tasks.
- â¢You can now change the interface font size to 12, 14, or 16 pixels in Settings > Appearance. The code preview in each theme card is now collapsible.
- â¢Full-page plugin views can now share a split view with a conversation, so opening a conversation from a plugin no longer navigates away from it. The window title bar, including the update button, now stays visible on full-page plugin views.
- â¢When the browser agent tries to download a file, the download now pauses until you approve it.
- â¢Pages controlled by the browser agent can no longer lock your mouse pointer or keyboard.
- â¢When a voice call ends, its transcript card now shows a short summary of the call. Answers you speak during a multi-question prompt now move the question card forward, and answering on the card tells voice mode to skip to the next question.
- â¢Pay-As-You-Go setup and license switching in Settings now suggest your Google Cloud projects as you type instead of requiring you to enter the project ID by hand.
- â¢Installing a plugin or skill from a plugin card in the agent's latest reply now resumes the conversation automatically.

## Show more (20)Show less

- â¢If you use remote agents, creating a project now offers a This Computer or Remote choice in one wizard instead of a separate New Remote Project item.
- â¢You can now scroll the conversation and side panels line by line with the Up and Down arrow keys.
- â¢Fixed an issue where sign-in could get stuck on Signing in... for some personal accounts.
- â¢Fixed an issue where conversations from a project whose settings file was missing disappeared from the sidebar when it was grouped by project.
- â¢Fixed an issue where content loading above the visible area made your reading position jump while you were scrolled up in a conversation.
- â¢Fixed an issue where, with Always Proceed set for artifact reviews, an artifact that asked for feedback hid its Proceed button and left the agent waiting.
- â¢Fixed an issue where voice mode kept failing for up to two minutes after one failed connection attempt.
- â¢Fixed an issue where file and artifact previews in the side panel could show stale contents after the file changed.
- â¢Fixed an issue where switching tabs in the side panel lost each tab's scroll position.
- â¢Fixed an issue where plugin panels showed blank in pinned side panel tabs, covered the edges of the side panel, or stayed on screen over a maximized side panel.
- â¢Fixed an issue where a plugin's context providers were missing after you added it from its full-page details screen.
- â¢Fixed an issue where a plugin that declared extra fields in its manifest lost all of its context providers.
- â¢Fixed an issue where plugin panels could not play sound until you interacted with them.
- â¢Fixed an issue where conversation titles sometimes guessed what an abbreviation in your first message stood for instead of keeping it as written.
- â¢Fixed an issue where opening a PDF larger than the display limit showed a second, raw error under the message saying the file is too large.
- â¢Fixed an issue where network error messages showed an internal service address instead of a plain explanation.
- â¢Fixed an issue where requests failed when a thinking model took more than a minute to start responding.
- â¢Fixed an issue where a model your Google Cloud project cannot access returned a raw error instead of a message explaining the access problem.
- â¢Fixed an issue where a custom model gateway failed when its base URL ended in /v1beta, or reported an empty response when a thinking model returned only reasoning before its answer.
- â¢Fixed an issue where messages displayed as chat bubbles were cut off sharply at the top of the conversation instead of fading out.

November 9, 2026

- â¢You can now switch to a simpler Knowledge Work layout for research, writing, and everyday tasks in Settings > General > Interface Layout, except on business and enterprise accounts. It hides code changes and developer tools, folds the agent's progress updates into one line, and describes commands in plain language.
- â¢When you first sign in, Antigravity now asks what best describes your work, such as Software Engineering, Knowledge Work, Data & Analytics, Science, or Design, and sets up a matching layout. Home then suggests ways to start, and selecting one adds any plugin it needs and fills in the prompt.
- â¢Voice mode has finished rolling out and is now available to everyone except business and enterprise accounts.
- â¢Remote agents have finished rolling out to Google AI Ultra subscribers and to Google AI Pro subscribers on a paid plan. The GitHub repository picker for cloud projects also has a new Configure on GitHub link for giving Antigravity access to more of your repositories.
- â¢Antigravity can now bring over your MCP servers, skills, custom commands, subagents, and instruction files from coding agents such as Claude Code, Codex, and Cursor into a plugin you own. Ask the agent to import your setup to get started.
- â¢The sidebar and title bar now share one background, and your conversation and side panel sit on it as a card with rounded corners, including in split view.
- â¢Your messages now appear as chat bubbles on the right side of the conversation.
- â¢You can now write bulleted and numbered lists in the message box by starting a line with a dash or "1." and a space, using Shift+Enter for the next item and Tab to indent. Lists keep their formatting when you copy and paste them.

## Show more (44)Show less

- â¢Tables in responses now fit the width of the conversation instead of scrolling sideways.
- â¢The permission preset you pick in the message box now applies only to that conversation, so changing it no longer affects your other conversations. This doesn't apply to business and enterprise accounts.
- â¢The Auto permission preset is no longer marked Preview, and business accounts without admin controls now get it too, along with the updated permissions system that other accounts already use and a notice that explains the change. Accounts with admin controls keep their current permissions.
- â¢When links open in the built-in browser, holding Cmd (Ctrl on Windows and Linux) while selecting a link, or using the middle mouse button, now opens it in a background tab. Hold Shift to open it in your default browser.
- â¢The built-in browser now warns you with a "Your connection is not private" page when a site's security certificate has a problem, and you can clear a site's cookies and data from the site information menu in the address bar.
- â¢Websites in the built-in browser can now connect to USB, serial, and other hardware devices. When a site asks, pick a device from the list under the address bar and select Connect.
- â¢Built-in browser settings now have their own Browser page in Settings.
- â¢Comment and Quote for text you select in the built-in browser are now in the page's right-click menu, so a button no longer pops up over the page each time you select text. They also work on more websites.
- â¢When Voice mode can't use your microphone, it now tells you why, such as no microphone found or access turned off in your system settings, and on macOS and Windows it links to the privacy settings. If your microphone goes silent during a call, it tries to reconnect and offers a Reconnect button.
- â¢Notices that a background task finished or that a subagent sent a message now appear inside the turn's Worked for section, so conversations stay compact.
- â¢When two files open in the side panel share a name, their tabs now also show the folder, and background task tabs show a short summary instead of the raw command.
- â¢Files, artifacts, and folders open in the side panel now have a Refresh button that reloads their latest contents.
- â¢Added and removed lines in code changes use new colors that keep syntax highlighting readable, and fully added or removed lines now stand out from lines that changed only in part.
- â¢Plugins you mention with @ in a message now show their logo, and you can hover over one to see its description.
- â¢A plugin's Accounts tab now appears sooner and shows why signing in failed, for example when you denied access in your browser. Plugin accounts aren't available for business and enterprise accounts.
- â¢If an MCP server can't start because its command isn't installed, Antigravity now tells you which command is missing and how to fix it.
- â¢When you type or paste the exact path of a file or folder in your project after @, it now appears first in the Files list.
- â¢The terms you see when you first sign in have new wording and now let you sign up for news and tips by email. Business and enterprise accounts see the same terms as before.
- â¢Improved performance of the conversation sidebar and of the app when you have many conversations. Switching to a conversation with many artifacts is faster, and Antigravity finishes starting up sooner.
- â¢Fixed an issue where signing in to a Google account in the built-in browser with a passkey on your phone, by scanning a QR code, failed with a message about Bluetooth.
- â¢Fixed an issue where a built-in browser tab that failed to load while in the background showed a blank page instead of an error page when you switched back to it.
- â¢Fixed an issue where the built-in browser's Download Location setting didn't show your computer's actual Downloads folder, for example on Windows, when you hadn't picked one.
- â¢Fixed an issue where built-in browser tabs showed a generic globe instead of the site's icon for some sites, such as python.org, and for pages on your local network.
- â¢Fixed an issue where Voice mode looked unresponsive while your microphone started, and replayed its start sound each time a call reconnected.
- â¢Fixed an issue where voice dictation could drop the last words you said when you stopped recording, and where Chinese and Japanese dictation briefly showed spaces between characters.
- â¢Fixed an issue where, in cloud conversations, scrolling up in a long conversation stopped before the first message, and the first message of a new conversation briefly disappeared.
- â¢Fixed an issue where the conversation navigator highlighted the wrong turn near the end of a conversation, or disappeared when Conversation Width was set to Fill.
- â¢Fixed an issue where zooming in or using a narrow window could push the side panel or its tabs past the edge of the window, and where the find bar slid under the Overview panel in wide windows.
- â¢Fixed an issue where the Review tab showed "Diff contents are no longer available" for changes made by subagents.
- â¢Fixed an issue where some SVG files showed up blank when opened in the file viewer, and where selected text in the file viewer was nearly invisible in dark mode.
- â¢Fixed an issue where the agent sometimes put a stray backslash before dollar amounts, and where code in a response could gain extra angle brackets when square brackets were followed by parentheses.
- â¢Fixed an issue where a queued follow-up message was sent right away, starting a new turn, while subagents or background tasks were still running.
- â¢Fixed an issue where Python and PowerShell commands that the agent ran on Windows could fail or show garbled text when their output contained emoji or non-English characters.
- â¢Fixed an issue where sign-in stayed on the account eligibility screen for several minutes after you verified your account in the browser.
- â¢Fixed an issue where a project in the sidebar said "No conversations yet" when all of its conversations were pinned.
- â¢Fixed an issue where View > Reset Zoom in the menu bar did nothing on Windows and Linux.
- â¢Fixed an issue where the Legacy Light and Legacy Dark themes didn't use their original colors.
- â¢Fixed an issue where asking a side question with /btw in a subagent's conversation could fail with an error.
- â¢Fixed an issue where signing in to an MCP server could fail if a new conversation turn started before you finished.
- â¢Fixed an issue where automatic updates sometimes left a Marketplace plugin on its old version, and where MCP servers on Windows left helper processes running that could make plugin updates fail.
- â¢Fixed an issue where selecting Sign in on a plugin's card in a conversation showed "No plugin found" for a plugin you added yourself.
- â¢Fixed an issue where business and enterprise accounts that reached a usage limit saw personal plan upgrade prompts, and where accounts with AI credits were told to upgrade instead of being offered AI credit overages.
- â¢Fixed an issue where screen readers didn't announce the names of some switches in Settings, such as Enable Sandbox Mode.
- â¢Fixed an issue where business and enterprise accounts with an invalid cloud project ID couldn't see or change it in Settings > Account.

### Antigravity VS Code extension: Background update checks, instant startup, remote sign-in port forwarding, and Windows recovery fixes

November 9, 2026

- â¢Startup Readiness Budget: Increased the default backend startup readiness timeout from 30 to 60 seconds to improve startup reliability on Windows.
- â¢Remote Workspace Sign-In: Added automatic port forwarding and authorization code fallback for sign-in across remote development environments.
- â¢Fast Background Startup: Enabled immediate startup on installed backend binaries without blocking editor activation on release checks.
- â¢Background Update Checks: Added recurring background release checks and a notification prompt to restart when an update is ready.
- â¢Conversation Search Shortcuts: Added keyboard shortcuts to search within conversations when the Antigravity chat panel is focused.
- â¢Screen Reader Navigation: Added visually hidden semantic headings to conversation messages to improve turn-by-turn navigation for screen readers.
- â¢Enterprise Proxy Authentication: Added support for custom root certificate authorities and proxy configurations during extension sign-in.
- â¢Windows Recovery Handling: Fixed background recovery loops attempting doomed restarts during Windows desktop logoff or system shutdown.

## Show more (4)Show less

- â¢Backend Binary Management: Fixed an issue where opening multiple editor windows on shared release channels could cause unintended backend binary downgrades.
- â¢Recovery Re-Download Prevention: Fixed an issue where transient recovery timeouts could wipe local metadata and trigger unnecessary binary re-downloads.
- â¢Notification Window Routing: Fixed task completion notifications on Windows focusing the incorrect window across multi-window workspaces.
- â¢Addressed other minor user interface issues.

### Antigravity Visual Studio extension: Voice-to-text input support, smoother sign-in flow, and update and gateway reliability improvements.

November 9, 2026

- â¢Voice Input: Enabled microphone access to support voice-to-text dictation within the chat tool window.
- â¢Sign-In Flow: Fixed an issue where completing sign-in prompted to open an external application instead of returning to the editor.
- â¢Update Reliability: Fixed an issue where installed background components could be downgraded when checking for updates.
- â¢Gateway Error Handling: Fixed an issue where reasoning responses from custom gateways could trigger an unexpected empty output error.

November 7, 2026

- â¢Fixed an issue where voice mode showed "Couldn't connect" when your sign-in needed an update. It now asks you to sign in again.
- â¢Fixed an issue where voice mode couldn't see links and @ mentions in messages you typed, and skipped messages that contained only a link.
- â¢Fixed an issue where voice mode could attach something you said earlier in a call to your latest words, which sometimes kept it from answering.
- â¢Fixed an issue where, after a brief drop in a voice call, the transcript could show parts of a reply you never heard.
