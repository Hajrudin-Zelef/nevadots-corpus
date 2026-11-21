---
id: collect-261121-veille-ia/divers-tech/opencode-review-coding-and-cli-agents-pricing-stackpick
title: "OpenCode review: Coding and CLI agents, pricing · StackPick"
domain: divers-tech
role: reference
task: article
actors: []
dates: []
keywords: ["opencode", "coding", "code", "agents", "claude", "agent", "terminal", "background"]
source: https://stackpick.app/tool/opencode
source_anchor: ""
source_lines: [1, 89]
section: "Divers_Tech"
---
# OpenCode

Open-source coding agent you run in the terminal.

Last reviewed

Last updated

OpenCode is #5 of 7 in Coding and CLI agents. Editorial score 7.4: Ease 6, Power 7, Price 9, Vibe-coder fit 8. Initial editorial estimate. Not a measured benchmark.

Who it is for A local open-source terminal agent.Coding and CLI agents: An agent that edits the repo.

OpenCode is an open-source coding agent for the terminal. You run it against a repo and connect a model. Use it when you want an alternative to Aider or Claude Code that stays local and open, and you are willing to check the docs for the current provider list.

## Best use cases

- Agent edits without a hosted IDE
- Bringing your own model key
- A CLI you can swap in beside Aider

## Pros

- + Open source
- + Terminal-first and repo-local
- + Not locked to one model vendor

## Who should skip it

Skip OpenCode when one of these listed limits is the constraint.

- − Younger and less of a default than Claude Code or Aider
- − Provider support and prices for models: check site

All comparisonsAll best-of lists

## Pricing

OpenCode is listed as Free. Open source and free. Model fees are the provider's. Rates: check site.

## Alternatives to OpenCode

Listed alternatives: Aider, Cline, Claude Code. Aider is listed for “Git-native pair programming from the terminal”. Cline is listed for “An agent inside stock VS Code” (also Claude Dev). Claude Code is listed for “Repo-sized edits from the terminal”.

Also in Coding and CLI agents: Codex CLI, Gemini CLI, Devin.

Coding and CLI agents

7.4

Open-source pair programming in the terminal. Git commits included.

Best for Git-native pair programming from the terminal

Coding and CLI agents

7.6

Open-source coding agent inside VS Code. Bring your own model.

Best for An agent inside stock VS Code

Coding and CLI agents

8.3

Anthropic's coding agent for the terminal and supported editors.

Best for Repo-sized edits from the terminal

1. ### OpenCode 2.3.2Released November 18, 2026. GPT-6.2 models do not cache a system prompt sent in the Responses instructions field, so every step rebilled it as uncached input. OpenCode now sends that prompt as a leading developer message on every Responses route, including OpenAI with an API key or ChatGPT sign-in, GitHub Copilot, Zen, Azure, and OpenRouter, so it is cached from the second step on. On GitHub Copilot gpt-6.2-sol, uncached input per step dropped from about 14,000 tokens to 3. Compaction through /responses/compact reads the same cached prefix. webfetch structured output no longer repeats the url and format it was called with. Source for OpenCode 2.3.2 (opens in a new tab)
2. ### OpenCode 2.3.1Released November 18, 2026. A shell command still running when its timeout expires moves to the background instead of being killed. The shell_timeout_background flag is gone, and a foreground timeout of 0 now means the default 120 seconds. Background compaction is on by default: as a session nears automatic compaction, the summary is written in the background so the next prompt does not wait. Set compaction.background to false to turn it off. A background summary that is never used still costs tokens, and experimental.background_compaction is removed. GPT 6.2 Sol and GPT 6.2 Astra, plus their fast variants, show up in the model picker when you sign in with ChatGPT. Settings, Extensions can check for plugin updates and install one. Selecting code in diff and file viewers selects text so you can copy it, and an Add comment toolbar appears beside the selection. OpenCode again reads CLAUDE.md in any directory that has no AGENTS.md, and falls back to ~/.claude/CLAUDE.md when the global AGENTS.md is missing. OPENCODE_DISABLE_CLAUDE_CODE or OPENCODE_DISABLE_CLAUDE_CODE_PROMPT skips every CLAUDE.md. websearch returns a flat list of results, each with url, title, content, and published date. Code Mode and plugins get a structured read result, Code Mode programs can use Node's Buffer, search("issues") is shorthand for a query object, and tool summaries in the catalog can be up to 500 characters. A malformed filename glob is reported as an error instead of returning no results. Server crashes return a 500 with a JSON UnknownError body. Experimental schedules are now automations, behind the automations flag. The schedules flag still works. An automation runs on a time, an interval, a cron, or a pushed MCP event. Active or paused plain prompt schedules carry over. Schedules with a condition or a run limit, and finished ones, are dropped. The schedule HTTP and SDK endpoints are removed. The new icon set is the default. Collapse long messages is a Timeline setting. The desktop app no longer fails to start when drafts.sqlite is a symlink to or a copy of the CLI database: it moves that file aside and creates its own. An app that embeds the SDK with an in-memory database no longer imports legacy auth.json credentials. The release also includes a large fix sweep for the core, the terminal, the desktop app, the SDK, and plugins. Source for OpenCode 2.3.1 (opens in a new tab)
3. ### OpenCode 1.19.6Released November 17, 2026. OpenCode 1 offers an upgrade to OpenCode 2.0. OpenCode 2 downloads in the background and installs only after you choose Restart to upgrade, Upgrade on quit, or Later. Sessions, projects, and settings are kept. Plugins made for OpenCode 1 need an update. Sessions with many images no longer send oversized requests: when the images in a session's history pass 25 MiB, the oldest are replaced with a placeholder. A provider error for a request body that is too large starts compaction and a retry instead of failing the turn. Source for OpenCode 1.19.6 (opens in a new tab)
4. ### OpenCode 2.3.0Released November 13, 2026. Subagents now always run in the background. They return their session ID immediately and report results to the parent when they become idle, so the parent session can keep working while several run at once. If a subagent goes idle while its own background work is still running, the report says it is still running and the parent waits for the final report. The terminal summarizes each run of tool calls and thoughts into one collapsible activity row. Verbosity defaults to low. Set it to medium or high in settings, or set session.verbosity, to see every step; a setting already saved is kept. In the desktop app, Jump to message searches prompts in the current session, including prompts not yet loaded, and jumps to one. The shortcut is Cmd+Alt+G, or Ctrl+Alt+G on Windows and Linux. Experimental MCP events are behind the mcp_events flag. An experimental option can move a shell command to the background when its timeout expires, behind shell_timeout_background; with that flag on, an explicit timeout on a background command stops it. Local Windows shell commands run through the process daemon. Providers can present a client certificate for gateways that require mutual TLS, using settings.tls with cert, key, and an optional passphrase. Local model discovery waits up to 15 seconds per request instead of 1 second. The desktop app adds an experimental Recent sessions sidebar, limited to the last seven days and 30 rows, and an experimental customizable sessions sidebar. The release also includes a large fix sweep for the core, the terminal, the desktop app, and plugins. Source for OpenCode 2.3.0 (opens in a new tab)
5. ### OpenCode 2.2.7Released November 10, 2026. When a Copilot account has an Auto pool, Auto in the model picker lets Copilot's router choose the model for each session at the Auto discount. If routing fails, OpenCode falls back to the account's default chat model. Right-click a side panel tab to close tabs to the right. Command-W closes side panel tabs wherever focus is, Option-Command-W or Ctrl+Alt+W always closes the session tab, and Command-Shift-T reopens the last closed tab of either kind where it was. Mark as unread on a tab, Home row, project row, or inbox card keeps its dot until the next visit. Code that imports @opencode/ai directly reads response.images[0], because ImageResponse.image is removed. On a Location that is still loading plugins, agent.list returns the agents loaded so far, and integration.list, usage.limits, vcs.init, rpc.call, and artifact endpoints no longer wait up to 5 seconds. Model, provider, generate, voice, and websearch.query endpoints still wait. read accepts a negative offset, such as -50 for the last 50 lines. Remote MCP servers that rotate refresh tokens stay signed in when several clients refresh at once or the token endpoint fails for a moment. The release also includes a large fix sweep for MCP, the TUI, desktop, and the SDK. Source for OpenCode 2.2.7 (opens in a new tab)
6. ### OpenCode 2.2.6Released November 6, 2026. pool: true shares one MCP connection across projects with the same setup. GUI plugins can add color themes, render or add actions to code blocks, and use native settings and menu components. On phones and tablets, composer actions sit in a touch dock above the keyboard. opencode run -w starts the session in a new git worktree, and --worktree=<name> names it. That flag cannot be combined with --continue or --session. opencode run --archived creates the session already archived, so scripted and CI runs stay out of the inbox, home, and session lists, and --continue skips them. opencode run waits for the background shell commands, subagents, and image jobs its prompt started, prints that it is waiting, and lets the model respond to their results before it exits. A job that never exits, such as a dev server, keeps the run open until Ctrl+C or SIGTERM. SIGTERM cancels the session and its background jobs and exits 143. Session warming can be set per provider and model, including interval: auto based on cache TTL. websearch.use is a policy action. OPENCODE_FLAGS=all or experimental.all_flags enables every feature flag. An experimental Code Mode bytecode VM is off by default and turns on with experimental.codemode_vm: true or OPENCODE_FLAGS=codemode_vm. Foundry Claude models can use Azure Messages. File reads and media from the server API are served as sandboxed downloads, and cross-site browser navigations and requests from disallowed origins to /api/* are refused. Webfetch and Code Mode fetch reject URLs that contain credentials, ignore the fragment when matching permission rules, and Code Mode fetch checks every redirect against webfetch rules. The TUI adds Scroll on send. Keep, session.submit_scroll set to keep, retains the reading position after a prompt is sent. The SDK adds client.experimental.job.list() for background shell, subagent, and image jobs. The release also includes a large fix sweep for the TUI, desktop, MCP, and plugins. Source for OpenCode 2.2.6 (opens in a new tab)
7. ### OpenCode 2.2.5Released October 30, 2026. Command-Option-O on macOS, or Ctrl+Alt+O on Windows and Linux, chooses a worktree, and Command-Option-B or Ctrl+Alt+B chooses its base branch. Command-Shift-X or Ctrl+Shift+X toggles shell mode in the composer. Holding Command or Ctrl in the quote popup, or pressing Command-Enter or Ctrl+Enter, sends that quote and comment and leaves the rest of the composer untouched. compaction.buffer accepts a percentage of the model's context window, such as 20%, in addition to a token count. A tool call named bash runs as shell, with the same permissions, unless a plugin tool is actually named bash. MCP servers that send stdio messages larger than 10 MiB stay connected, and those messages are read much faster. Source for OpenCode 2.2.5 (opens in a new tab)
8. ### OpenCode 2.2.4Released October 30, 2026. Desktop terminals now belong to the session, survive server restarts with their scrollback, and stay synchronized with the TUI and other windows. Renamed terminals keep their names across restarts. With experimental.artifacts enabled, agents can publish, update, and serve HTML or static artifacts through the built-in local provider or plugin-provided storage. Selecting Claude Agent from the provider picker copies claude auth login, can install Claude Code, and checks the connection before model selection. While creating a worktree, Choose base branch is available from the command palette and Settings, Shortcuts, bound to Command-Option-Period on macOS or Ctrl+Alt+Period on Windows and Linux. Enabled schedules run under opencode serve, and multiple server processes coordinate so a schedule fires only once. Schedules stay opt-in through experimental.schedules. Source for OpenCode 2.2.4 (opens in a new tab)
9. ### OpenCode 2.2.3Released October 30, 2026. Install Claude Code, run claude auth login, and select Claude Agent in /connect to use that login and its models. OPENCODE_CLAUDE_PATH can override the Claude executable. Desktop streaming and long conversations do less rendering work, and the release fixes side-panel tabs, repeated "Session not found" toasts, and Open file over HTTP. Experimental schedules can run Code Mode JavaScript checks, including statically authorized tool calls, persistent state, and custom wake messages. Breaking changes for code that imports @opencode/ai directly do not change the CLI, Desktop, or HTTP API: speech, transcription, and video move from @opencode/ai/effect to @opencode/ai/experimental, including ai.experimental.speech, ai.experimental.transcription, and ai.experimental.video on the Promise client. TestSpeech, TestTranscription, and TestVideo move to @opencode/ai/experimental/testing. Both clients build tools with tool(...), which replaces AI.tool and Tool.make, and testing.tool is now testing.toolCall. Separately, @opencode/plugin now requires @opentui/core and @opentui/solid 0.8.0 or later. In OpenTUI 0.8, a ScrollBox's wrapper, viewport, content, and scrollbars are facade objects instead of renderables, and a custom RenderContext must implement clock and onAnimationTick. Managed policy integration.use now controls integration accounts only. The new mcp.use, skill.use, and plugin.use actions control MCP servers, skills, and plugins. Rules with the old mcp:, skill:, and plugin: resource prefixes are converted automatically, but a bare integration.use rule no longer applies to those resources. Source for OpenCode 2.2.3 (opens in a new tab)
10. ### OpenCode 2.2.2Released October 29, 2026. When a connected provider supports image models, agents can use experimental_image_generate without extra configuration. Set a preferred model with experimental.image_generate.model. On the Desktop new-session screen, clicking the current Git branch opens that project's "Run session in" worktree picker. Timeline quotes, terminal quotes, and browser annotations share a compact optional-comment editor, and source highlights stay attached to local drafts. Classic question cards are the default again. Settings, Experimental, New question UI turns on the redesigned keyboard-oriented dock. The SDK adds if and until inputs for client.experimental.schedule.create, plus schedule check results and output fields. Source for OpenCode 2.2.2 (opens in a new tab)
11. ### OpenCode 2.2.1Released October 29, 2026. Run /goal with an objective in the TUI or Desktop and the session continues until a reviewer confirms the objective is met. /goal pause, /goal resume, and /goal clear control it. Enable experimental.image_generate so agents can create or edit images with the image_generate tool, and optionally set experimental.image_generate.model. Opt-in scheduled prompts are available through /schedule, agent tools, and experimental.schedules. In Desktop, selecting a running shell in the activity pill shows its live output and can stop it, and a redesigned model picker that searches favorites, recent models, providers, and variants is now the default. Codex sign-in can offer GPT Image 2, and MiniMax and Meta Muse image generation are supported. OpenRouter, DigitalOcean, and Venice gain Responses API support, including freeform tools. Breaking changes affect code that imports @opencode/ai directly, not the CLI, Desktop, or HTTP API: the fal, Black Forest Labs, Replicate, and Stability providers and their entry points are removed; five legacy provider entry points are removed in favor of the nested paths 2.2.0 already exports; and each provider's configure() input type is named Config. Source for OpenCode 2.2.1 (opens in a new tab)
12. ### OpenCode 2.2.0Released October 27, 2026. Experimental Desktop plugins can be created, installed, and shared, and managed from Settings, Extensions, Plugins. Plugins can attach and update keyed session instructions with ctx.session.instructions.entry.put, list, and remove; each change is added as a small update at the next step instead of editing the system prompt, and the entries survive compaction and reverts without starting an extra model turn. Experimental image and decision generation APIs are available to plugins and the SDK. Prompt-cache TTLs can be set per provider and model, and the Todo tool is restored in the desktop app as an opt-in. Breaking changes: the @opencode/ai package root exports the Promise API and the Effect API moves to @opencode/ai/effect, @opencode/ai/promise is removed, the OpenCode GitHub Action is no longer published, and TUI plugins require OpenTUI 0.7.0. Sign in with ChatGPT adds GPT-6.1 Terra and GPT-6.1 Terra Fast. The release also refreshes the desktop app. Source for OpenCode 2.2.0 (opens in a new tab)
13. ### OpenCode 2.1.4Released October 23, 2026. When an MCP server asks for input during a tool call, the form shows up instead of the call waiting until it times out. Desktop and web can show these forms too, including text, number, choice, multiselect, boolean, and URL requests. Set experimental.background_compaction to true to prepare a summary in the background as a session nears its compaction threshold, so automatic compaction does not wait for it. Summaries that are never used still cost tokens. ChatGPT sign-in context caps are a documented built-in plugin; add "-opencode.provider.openai-context-caps" to plugins to turn them off. An experimental TUI tools option lets agents open and type into the session terminal through tui_open_terminal and tui_write_terminal without taking keyboard focus. SDK clients can call connectClientMcp() to host MCP tools over the experimental client MCP WebSocket transport. Source for OpenCode 2.1.4 (opens in a new tab)
14. ### OpenCode 1.19.4Released October 22, 2026. Upgrades the bundled Bun runtime to 1.4.4. OpenCode says that runtime includes security fixes for Brotli, TLS, HTTP/1.1, HTTP/2, HTTP/3, WebSocket, and fetch, and updates root certificates to NSS 3.131.1. Source for OpenCode 1.19.4 (opens in a new tab)

Newsletter

## Get the weekly AI tool shortlist

One email when rankings change or a tool ships something that matters. No spam, unsubscribe anytime.
