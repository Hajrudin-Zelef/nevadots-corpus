---
id: collect-261121-veille-ia/gpt-openai/codex-cli-guide-2026-setup-sandbox-agents-md-mcp-1
title: "Codex CLI Guide 2026: Setup, Sandbox, AGENTS.md & MCP"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["codex", "sandbox", "agents", "guide", "cloud", "tasks", "setup", "gpt-6"]
source: https://blakecrosley.com/guides/codex
source_anchor: ""
source_lines: [1, 1500]
section: "GPT_OpenAI"
---
# Codex CLI Guide 2026: Setup, Sandbox, AGENTS.md & MCP

# Complete Codex CLI reference: install and auth, models and pricing, sandbox and approval modes, AGENTS.md, MCP, skills, and cloud tasks. Updated for v0.167.1.

Complete Codex CLI reference: install and auth, models and pricing, sandbox and approval modes, AGENTS.md, MCP, skills, and cloud tasks. Updated for v0.167.1.

**TL;DR:** Codex is a multi-surface coding agent: it reads your codebase, runs commands in an OS-level sandbox, patches files, and delegates tasks to the cloud. Master five systems – `config.toml`, the sandbox/approval model, AGENTS.md, MCP, and skills – and it becomes a force multiplier. **Which model:** the CLI’s bundled default is **GPT-6.1 Sol** (`gpt-6.1-sol`, since v0.159.1), which OpenAI’s models docs describe as near-Astra performance at a lower cost and suggest for repeated, long-running work across code, apps, and documents. The same docs point the hardest end-to-end work at **GPT-6-Astra**, clear, repeatable tasks at **GPT-6 Luna**, and everyday writing and visual work at **GPT-6.1 Terra** (in Codex since October 26, 2026; in the bundled catalog since v0.165.0). Available options vary by plan, client, workspace settings, and rollout; GPT-6 Sol and the GPT-5.6 models stay in the picker, and GPT-5.5 retired from Codex for ChatGPT sign-in on October 14, 2026 (the API and API-key sessions keep it).<sup>179</sup><sup>202</sup><sup>205</sup><sup>233</sup> New here? Jump straight to Install and Quick Start. Tracking releases? The latest stable is **v0.167.1 (November 10, 2026)**, a three-commit patch on v0.167.0 (November 6), which added `codex agents --no-daemon`, backend switching in the session picker, local image previews in the composer, a “Sign in another way” screen with an opt-in Google Cloud setup wizard, and a system-proxy option in `/experimental`; Release Highlights opens with a current-state summary (what is on by default, what is still experimental) and then covers each version, and the Changelog lists every guide update.<sup>123</sup><sup>179</sup><sup>183</sup><sup>184</sup><sup>198</sup><sup>200</sup>


**Codex operates as a multi-surface coding agent**, not a chatbot that writes code. The CLI reads your codebase, executes commands in a sandbox, patches files, connects to external services via MCP, and delegates long-running tasks to the cloud. It runs locally but thinks globally; the same intelligence powers five distinct surfaces depending on how you work, including the browser extension (Chrome, Edge, Brave, Opera, and Vivaldi) that runs Codex in your browser without taking it over.<sup>90</sup><sup>136</sup>

**The difference between casual and effective Codex usage comes down to five core systems.** Master these and Codex becomes a force multiplier:


1. **Configuration system** : controls behavior via`config.toml`
2. **Sandbox & approval model** : gates what Codex can do
3. **AGENTS.md** : defines project-level operating contracts
4. **MCP protocol** : extends capabilities to external services
5. **Skills system** : packages reusable domain expertise

I spent months running Codex alongside Claude Code across production codebases, CI/CD pipelines, and team workflows. This guide distills that experience into the complete reference I wish existed when I started. Every feature includes actual syntax, real configuration examples, and the edge cases that trip up experienced users.

## Key Takeaways

- **Five surfaces, one brain** : CLI, desktop app, IDE extension, cloud tasks, and the browser extension (Chrome, Edge, Brave, Opera, and Vivaldi) all share the same model family, so pick the surface that fits your workflow.<sup>90</sup>
- **OS-level sandboxing** : Codex enforces filesystem and network restrictions with operating-system sandboxes (Seatbelt on macOS; bubblewrap + seccomp on Linux; MXC,`elevated` or`unelevated` on Windows), not inside containers.
- **AGENTS.md is cross-tool** : Your project instructions work in Codex, Cursor, Copilot, Amp, Jules, Gemini CLI, Windsurf, Cline, Aider, Zed, and 60,000+ open source projects. Write once, use everywhere.
- **Profiles save context-switching overhead** : Define named config presets (`fast` ,`careful` ,`auto` ) and switch between them with`--profile` .
- **Context management matters** : OpenAI’s models docs give GPT-6.1 Sol a 500K default context window in Codex, while the model catalog bundled with the CLI still lists 272K for it; the same catalog gives GPT-6 Astra, Sol, and Luna and the GPT-5.6 family 272K and GPT-6.1 Terra 500K (the API lists GPT-6.1 Sol, GPT-6.1 Terra, GPT-6 Astra, GPT-6 Sol, and Luna at 1.05M). Use`/compact` , focused prompts, and`@file` references to manage token budgets proactively.<sup>117</sup><sup>179</sup><sup>202</sup><sup>205</sup><sup>242</sup>

### How to Use This Guide

This is a reference of more than 4,000 lines; start where your experience level fits:

| Experience | Start Here | Then Explore | 
|---|---|---|
| **New to Codex** | Installation → Quick Start → Mental Model | Configuration, Sandbox | 
| **Daily user** | AGENTS.md, Skills, Plan Mode | MCP, Hooks | 
| **Team lead / enterprise** | Enterprise Deployment → Best Practices | Decision Frameworks, Workflow Recipes | 
| **Migrating from another tool** | Migration Guide | Decision Frameworks | 

The Quick Reference Card at the end provides a scannable summary of all major commands.

## Release Highlights: What Changed Recently

The version-by-version record, newest first, for readers tracking Codex’s release train. New here? Skip to Quick Start; nothing below is required to get productive. Source detail (compares, commit lists, docs quotes) lives in the footnotes.

### Current State

**Codex CLI as of November 11, 2026**


**Stable:** v0.167.1 (November 10, npm `latest`), a three-commit patch on v0.167.0 whose release notes are only a compare link; it adds one flag, `incremental_tools_legacy`, off and under development (incremental tool declarations for the Amazon Bedrock providers). v0.167.0 (November 6) was cut from the out-of-band 0.167.0-alpha.3.2 plus one release commit, 123 commits past v0.166.0: `codex agents --no-daemon` opens the agent command center without the shared background server, the command center and session picker mark unread results, the session picker switches between connected backends and keeps drafts, local image attachments preview in the composer, API-key and cloud-provider sign-in sit under “Sign in another way” with an opt-in Google Cloud setup wizard, `/experimental` gains a system-proxy option for macOS and Windows, and patch details expand in the transcript while an approval prompt is open. Before them: v0.166.0 (October 29; keyboard-driven transcript selection, GPT-6.1 Terra in the Vertex AI and Bedrock catalogs (bundled since v0.165.0), `codex queue --resume`, `codex doctor --network-only`) and v0.165.0 (October 27; the built-in Google Cloud Vertex AI provider, memory v2 for fresh profiles, a repository filter in the session picker).
**Alpha:** npm `alpha` is 0.170.0-alpha.1 (November 11, 20:15 UTC), after 0.169.0-alpha.11 (November 11, 10:20 UTC). The 0.167 line ran alpha.2 (November 1) to alpha.5 (November 4), with the out-of-band alpha.4.1 (November 9) and alpha.4.2 (November 11) published later from the alpha.4 base, but the stable was cut from the out-of-band 0.167.0-alpha.3.2, so three things visible at the alpha.4 tag did not ship in v0.167.0 or v0.167.1: `retain_client_tool_outputs` is still under development and off, `daemon_auto_start` is still a stable flag you can switch, and `cli_resume_account_check`, `tui_workspace`, and `agent_message_board_mcp` are absent. The 0.168 line produced alpha.1 (November 4) through alpha.7 (November 5) and no stable; the 0.169 line has been on npm since alpha.2 (November 6), and the 0.170 line since alpha.1 (November 11).
**On by default since v0.167.0:** deferred tool world state (`deferred_tool_world_state`, stable): deferred tool namespaces are listed in the developer `<tools>` context instead of the `tool_search` description (`deferred_tool_world_state = false` under `[features]` puts them back). Stable but off, so opt in under `[features]`: `gcp_setup_wizard` (Google Cloud Vertex AI setup during sign-in) and `suppress_model_change_announcements`. Experimental and off: `respect_system_proxy`, the “System proxy” entry in `/experimental`.
**On by default since v0.165.0 and v0.166.0:** reasoning-effort changes during a conversation (`reasoning_effort_override`, stable; `reasoning_effort_override = false` under `[features]` turns them off), memory v2 for profiles that have no v1 memories yet (`memories.version = "v1"` keeps v1), and, from v0.166.0, `prediction_preserve_parent_tools` and `plugin_discovery_prefetch` (both stable).
**Default model since v0.159.1:** GPT-6.1 Sol, replacing GPT-6-Astra, including on Amazon Bedrock. Set `model` in `config.toml` to keep another. The bundled catalog gained GPT-6.1 Terra in v0.165.0 (sixth in the catalog’s order, after GPT-5.6 Sol; 500K context) and is otherwise the v0.159.1 list; the catalog file is byte-identical from v0.165.0 through v0.167.1, and GPT-6.1 Sol stays first.
**On by default since v0.164.0:** the Amazon Bedrock setup wizard (`bedrock_setup_wizard`, stable; `bedrock_setup_wizard = false` under `[features]` turns it off), incremental tool updates (`incremental_tools`, stable), and MCP OAuth refresh coordination (`mcp_oauth_refresh_coordination`, stable). Managed worktrees and realtime conversation are permanent: their flags are marked removed with the features left on.
**On by default since v0.163.0:** Daybreak (`cli_daybreak`, now stable; `cli_daybreak = false` under `[features]` hides the controls) and code-mode interruption (`code_mode_interrupt`, stable), so a follow-up also stops a running Code Mode cell; API-key sessions get Cyber access programs by default (`api_key_cyber_access_programs`). Removed: `shell_zsh_fork` and `unified_exec_zsh_fork`, ignored with a migration notice.
**On by default since v0.162.0:** `instant_interrupt`, so new input preempts a model response (`instant_interrupt = false` under `[features]` turns it off); the flag stays marked under development.
**On by default since v0.161.0:** API-key model discovery (`api_key_model_discovery = false` under `[features]` turns it off).
**On by default since v0.158.0:** approval for input to elevated terminals (`write_stdin_approval = false` under `[features]` turns it off), plus copy-on-select and right-click paste in the fullscreen TUI (`copy_on_select = "never"`, `right_click_paste = "off"` under `[tui]`). Guardian thread context is always on; `thread_context` is ignored.
**On by default since v0.157.0:** the fullscreen transcript (`fullscreen_transcript = false` under `[tui]`) and background-server autostart (`daemon_auto_start = false` under `[features]`, or `--no-daemon`).
**On by default since v0.156.0:** `/voice`, managed worktrees, Guardian parent-compaction reuse.
**Opt-in or experimental:** Codex Cloud, code mode and its ranked tool search (`code_mode_tool_search`, v0.162.0), Guardian history retrieval (`guardian_conversation_history_tools`, v0.160.0), and the under-development flags that ship off at v0.164.0 (from v0.163.0: `async_compaction`, `credential_masking`, `output_token_replay`, `retain_client_tool_outputs`, `subagent_default_context_limits`, `code_mode_host_grpc`, `code_mode_tool_description_first`, `guardian_trust_orchestrator_connectors`; new at v0.164.0: `app_server_websocket_compression`, `executor_capability_discovery_v2`, `mcp_tool_list_changed`, `multi_env_copy_file`, `runtime_agent_instances`, `subagent_thread_environments`, `wake_on_agent_messages`; new and off at v0.165.0 and v0.166.0: `tui_multi_backend`, `untrusted_as_tool_output`, `critical_path_timing`, `mcp_connection_sharing`, `simplified_selection_hints`, `omit_skill_descriptions`, `skill_catalog_deltas`, `guardian_recorded_items`, `guardian_native_multi_agent_history`, `guardianv3`, and `external_agent_memory_import`; new and off at v0.167.0: `apps_early_resource_discovery`, `code_mode_host_output_truncation`, `incremental_exec_tools`, `interrupt_on_cancel`, and `tui_extensions_statusline`; new and off at v0.167.1: `incremental_tools_legacy`; `code_mode_tool_description_first` is removed). Experimental context management is not currently available.
**Models:** GPT-6.1 Sol (the bundled default) for complex coding and long-running work, GPT-6-Astra for the hardest work, GPT-6 Luna for repeatable tasks, GPT-6.1 Terra (in Codex since October 26, 2026) for everyday writing and visual work. GPT-6 Sol and Luna start on Fast (2.5x included usage); GPT-6.1 Sol does not. Ultrafast, for GPT-6.1 Sol and GPT-6 Astra, is limited to Pro $500 and eligible Enterprise and Edu plans. GPT-5.5 retired from Codex for ChatGPT sign-in on October 14, 2026 (API-key sessions keep it), and GPT-5.3-Codex-Spark retired on September 14, 2026.
**Pinning:** `npm install -g @openai/codex@0.167.1`, then `codex --version`.<sup>177</sup><sup>179</sup><sup>187</sup><sup>188</sup><sup>189</sup><sup>190</sup><sup>194</sup><sup>195</sup><sup>196</sup><sup>197</sup><sup>198</sup><sup>199</sup><sup>200</sup><sup>201</sup><sup>202</sup><sup>204</sup>

### v0.167.0 and v0.167.1 (November 6 and 10, 2026)

**One stable and a three-commit patch.** v0.167.0 was published on November 6 (01:40 UTC; npm `latest` at 01:46 UTC), cut from the out-of-band 0.167.0-alpha.3.2 plus one release commit; the in-line 0.167.0-alpha.5 (November 4) carried 132 commits the stable does not have, the 0.163 pattern again. The compare from v0.166.0 counts 123 commits. One flag flips on and goes stable: `deferred_tool_world_state`, which lists deferred tool namespaces in the developer `<tools>` context instead of the `tool_search` description (`deferred_tool_world_state = false` under `[features]` puts them back). Two flags arrive stable but off, `gcp_setup_wizard` and `suppress_model_change_announcements`; `respect_system_proxy` moves from under development to experimental, still off; and five arrive off and under development (`apps_early_resource_discovery`, `code_mode_host_output_truncation`, `incremental_exec_tools`, `interrupt_on_cancel`, `tui_extensions_statusline`). v0.167.1 followed on November 10 (21:32 UTC; npm 21:39 UTC) with three commits and release notes that are only a compare link. The bundled model catalog and the TUI’s slash command source file are byte-identical to v0.166.0’s.<sup>200</sup><sup>201</sup>

**Agents, sessions, and the composer (v0.167.0).** `codex agents --no-daemon` opens the agent command center in an embedded session, without the shared background server, and both the command center and the session picker mark unread results (#58238, #58239, #58263, #58264). The session picker switches between connected backends and keeps your draft when you switch (#58280, #58457). On supported terminals, local image attachments preview as thumbnails while you compose, and viewed local images open from clickable transcript paths (#58261, #58446). Patch details expand in the transcript while an approval prompt is open, so you can read the change before you answer (#58281). The CLI reference had not caught up on November 10: it still said `codex agents` does not support `--no-daemon`.<sup>200</sup><sup>201</sup>

**Sign-in and networking (v0.167.0).** Onboarding groups API-key and cloud-provider options under “Sign in another way.” The Google Cloud Vertex AI setup wizard is opt-in: with `gcp_setup_wizard = true` under `[features]`, that screen offers setup with Application Default Credentials or a credential file (#57857, #58427); without the flag, configure the `gcp` provider by hand as shown in Custom Model Providers. `/experimental` gains a “System proxy” entry that makes Codex requests use the macOS or Windows system proxy settings (`respect_system_proxy`, off by default; #58157). Provider OAuth sign-in waits ten minutes before timing out (#57859), and network approval prompts close when their requests end, with concurrent requests to the same endpoint prompting one at a time (#58350, #58351).<sup>200</sup><sup>201</sup>

**Fixes and housekeeping.** Saved chats stay visible after you switch model providers, and a chat that cannot resume still shows its transcript and offers an editable prompt to continue in a new chat (#58202). `Ctrl+L` clears the terminal display while keeping earlier messages available through `Ctrl+T` (#58266); deleting text that exposes a leading `!` keeps an ordinary draft in chat mode (#58412); long realtime V3 conversations save completed speech to chat history (#58221); and automatic compaction accounts for retained output from failed responses and responses with missing token usage (#58404). `PreToolUse` hooks run before direct Guardian review policy checks (#58368), MCP approval metadata includes tool names and annotation hints (#58161), and the default models for memory extraction and consolidation were updated (#58091). Linux musl builds use fewer allocator arenas, which cuts idle memory after conversations unload (#58399). v0.167.1 carries two changes besides its release commit: `incremental_tools_legacy`, an opt-in flag for incremental tool declarations on regular Responses requests (#59832), and a follow-up that limits it to the `amazon-bedrock` and `amazon-bedrock-runtime` providers (#60040). It ships off.<sup>200</sup><sup>201</sup>

What to do: upgrade. Nothing changes a default you would notice. Deferred tool namespaces now sit in the developer context (`deferred_tool_world_state = false` under `[features]` returns them to the `tool_search` description); if you are setting up Vertex AI, `gcp_setup_wizard = true` turns the wizard on; and if you work behind a macOS or Windows system proxy, try “System proxy” in `/experimental`.<sup>200</sup><sup>201</sup>

### v0.165.0 and v0.166.0 (October 27 and 29, 2026)

**Two stables in three days.** v0.165.0 was published on October 27 (20:09 UTC; npm `latest` at 20:14 UTC), cut from 0.165.0-alpha.1 plus four commits, 71 commits past v0.164.0; the out-of-band 0.165.0-alpha.1.1 and 1.2 (October 27) ran alongside. v0.166.0 followed on October 29 (19:47 UTC; npm 19:56 UTC), cut from 0.166.0-alpha.6 plus one release commit after a line that ran alpha.1 (October 27) to alpha.6 (October 29) with 0.166.0-alpha.5.1 out of band; the compare from v0.165.0 counts 265 commits. One flag flips on and goes stable in the window: `reasoning_effort_override`, so reasoning effort can change during a conversation on supported models (`reasoning_effort_override = false` under `[features]` restores the old behavior). Two new flags arrive stable and on, `prediction_preserve_parent_tools` (inherited tool declarations survive in MCP-disabled prediction forks) and `plugin_discovery_prefetch`; ten arrive off and under development (`tui_multi_backend`, `untrusted_as_tool_output`, `critical_path_timing`, `mcp_connection_sharing`, `simplified_selection_hints`, `omit_skill_descriptions`, `skill_catalog_deltas`, `guardian_recorded_items`, `guardian_native_multi_agent_history`, `guardianv3`); and `code_mode_tool_description_first` is removed. The TUI’s built-in slash command list is unchanged at 65 entries, plus `/fast` where the model catalog offers a Fast tier.<sup>198</sup><sup>199</sup>

**Models and providers (v0.165.0, v0.166.0).** GPT-6.1 Terra enters the bundled catalog in v0.165.0 (#57337) and the Vertex AI and Amazon Bedrock catalogs in v0.166.0 (#57293): `gpt-6.1-terra`, sixth in the catalog’s order (after GPT-5.6 Sol), 500K context in the catalog (872K maximum) against 1.05M on the API, default reasoning `low`, image input, and code mode only. OpenAI’s October 26 note makes it available in ChatGPT Work and Codex on Plus, Pro, Business, Enterprise, and Edu plans as “a fast, efficient model for professional work that combines strong image understanding with precise computer use”; see Which Model Should I Choose? for prices. A built-in **Google Cloud Vertex AI provider** ships in v0.165.0: `model_provider = "gcp"` sends requests to Vertex AI with Google Application Default Credentials, and the provider’s `project_id`, `location` (defaults: `GOOGLE_CLOUD_PROJECT`, `GOOGLE_CLOUD_LOCATION`, then `global`), and `credential_file` (overrides `GOOGLE_APPLICATION_CREDENTIALS` for the auth command) fill in what the environment does not (#56265, #56560, #57188); see Custom Model Providers. A provider that reports a model as not found is no longer retried, and its error message is kept (#56445).<sup>198</sup><sup>199</sup>

**Memory v2 (v0.165.0).** A fresh profile, one with no `~/.codex/memories/` directory yet, now starts on memory v2, whose artifacts live under `~/.codex/memories_v2/`; a profile that already has v1 memories keeps v1, an explicit `memories.version` (`v1` or `v2`) wins either way, and `memories.dual_write = false` opts a fresh profile out of the switch. Importing external-agent memories into v2 is behind `external_agent_memory_import`, off and under development at both tags. See Memory System.<sup>198</sup><sup>199</sup>

**New in the TUI and CLI.** Transcript selection is keyboard-driven, with word, line, and page motions, integrated search, and configurable shortcuts (#57221 to #57224); Markdown renders consistently in async questions, choices, answers, and transcript replies (#57259, #57292); a safety block explains itself and offers selectable safer follow-up prompts (#57363, #57365); and the session picker gains a repository filter while the agent command center groups by directory, for sessions across linked worktrees (#56308). `codex queue --resume` resumes an unloaded thread on the local daemon or the selected `--remote` server after queueing a message, warning rather than failing when the resume does not work (#57232). `codex doctor --network-only` runs only the configuration, connectivity, and WebSocket diagnostics (#57606). The app-server API gains experimental attached thread forks (#56351). Fresh TUI sessions honor the server’s default approval reviewer when nothing overrides it, TUI preferences stay editable when execution settings are invalid, a remote MCP tool request is cancelled when its local response future is dropped, and queued agent messages no longer restart work after an interrupt or a switch to Plan mode (#56090, #56091, #56132, #56330).<sup>198</sup><sup>199</sup>

**Fixes and housekeeping (v0.166.0).** Acknowledged approval and permission settings survive resume and fork, local image attachments survive a killed composer, spawned descendant threads come back when their parent is unarchived, app-server thread startup and shutdown races are fixed with completed runtimes released promptly, TUI reconnection recovers better including offline `/quit` and `/exit`, and reasoning-token accounting survives resume and fork so compaction no longer fires early (#57227, #57502, #57635, #57787, #57788, #57820, #57839). Standalone release cleanup on macOS works when an installation is in use or a path has non-ASCII characters (#57503). v0.165.0 closed a marketplace URL validation gap that could let Git connect to a different host than the one checked against the allowed patterns (#56263). The standalone executor now ships in the canonical CLI and app-server packages, Bazel builds V8 from source, and builds standardize on Node.js 24.22.1 and Rust 1.95.0 (#57837, #57164, #57168, #57373).<sup>198</sup>

What to do: upgrade. Nothing changes a default you would notice unless you relied on the reasoning effort staying fixed for a conversation (`reasoning_effort_override = false` under `[features]`) or you start a new profile, which now writes memory v2. GPT-6.1 Terra needs no configuration; pick it in `/model` or run `codex -m gpt-6.1-terra`.<sup>198</sup>

### v0.164.0 (October 23, 2026)

**The 0.164 line ships stable from its own alpha.** v0.164.0 was published on October 23 (04:48 UTC; npm `latest` at 04:52 UTC), cut from 0.164.0-alpha.11 plus one release commit; alpha.12 (October 22) ran nine commits ahead of that cut, and the out-of-band 0.164.0-alpha.11.1 (October 23, 01:58 UTC) added four commits on the alpha.11 base. The compare from v0.163.0 counts 330 commits. Three flags flip on and are marked stable: `bedrock_setup_wizard` (the Amazon Bedrock setup wizard, with API key entry grouped alongside the other AWS sign-in methods when credentials are detected), `incremental_tools`, and `mcp_oauth_refresh_coordination`. The `worktrees` and `realtime_conversation` flags are marked removed with their features left on, so managed worktrees and realtime conversation are no longer switchable; `stable_environment_tools` and `transcript_v2` are removed too. Seven under-development flags are new and off. The bundled model catalog is the blob v0.162.1 shipped.<sup>196</sup><sup>197</sup>

**New in the TUI.** `/presets` saves reusable prompts and slash commands in ten numbered slots with a keyboard menu; a preset can be made from composer text or from the current model, reasoning effort, and speed, and presets persist under `tui.presets` in the active user configuration (#54380, #54381). `/model [model] --reasoning <level> --speed <tier>` applies model, reasoning, and speed to the current session without changing saved defaults, with space-separated or `=` flag values; omitting the model applies the settings to the current one, and a bare `/model` still opens the picker (#54378). `/permissions <profile-id>` selects a built-in or custom permission profile inline, such as `/permissions :read-only`, with `:ask-for-approval` and `:approve-for-me` as aliases (#54383). On terminals that support Kitty or Sixel graphics, the fullscreen transcript previews viewed and generated images as bounded thumbnails; remote workspaces, unsupported terminals, and oversized images keep the text-only entry (#54216).<sup>196</sup>

**Connections and credentials.** Directly configured MCP servers can authenticate with account-scoped enterprise grants stored in the keyring or a dedicated sign-in, including OAuth clients that require secrets; the enterprise-managed authorization extension is negotiated with the `2026-07-28` protocol and the authorization is revoked when the account owner changes or an enterprise login replaces the grant (#55334, #55859, #56001). Credential masking reaches supported remote Bash commands and zsh snapshot sessions, including credentials from custom providers: the command sees placeholders and the executor-local proxy injects the real credentials for matching destinations, falling back to unmasked execution with a warning where masking is unavailable; the `credential_masking` flag still ships off at the tag (#54309, #54432, #56049).<sup>196</sup><sup>197</sup>

**Fixes and housekeeping.** A failed compaction no longer leaves partial summaries in the conversation history, and a background compaction failure falls back without panicking (#53932, #55581). Resuming a session finds compressed histories and restores the saved thread settings instead of replacing them with global defaults (#54008, #55923). On Windows, HTTPS and WebSocket connections answer proxy authentication challenges with the signed-in user’s Negotiate or NTLM credentials (#55994, #56000). `-c` or `--config` given at several subcommand levels keeps every override (#56022); queued prompts wait for a pending permission selection to resolve (#55682); and the sandbox checks case-variant metadata paths on macOS and replaceable symlinks used to locate executables (#54273, #55384). Disk use drops: unused installations are pruned (the current and previous daemon selections are kept), abandoned marketplace checkouts are cleaned, and daemon logs are bounded (#53827, #55149, #55689, #55690, #55856); the embedded JavaScript runtime gains safety checks for optimized `Array.sort` (#54519).<sup>196</sup>

What to do: upgrade. Nothing changes a default you would notice unless you onboard on Bedrock, where the wizard now runs by itself (`bedrock_setup_wizard = false` under `[features]` turns it off); if you had set `worktrees` or `realtime_conversation` under `[features]`, the keys are now ignored.<sup>196</sup><sup>197</sup>

### v0.163.0 (October 15, 2026)

**The 0.163 line ships stable, from the out-of-band branch.** v0.163.0 was published on October 15 (21:30 UTC; npm `latest` at 21:37 UTC), cut from 0.163.0-alpha.10.2, an out-of-band build on the alpha.10 base, plus one release commit; the in-line alpha.11 (October 14) carried 23 commits the stable does not have, the 0.158 pattern again. The compare from v0.162.1 counts 232 commits. Two feature flags flip on and both are marked stable: `cli_daybreak` (Daybreak controls, the `/daybreak` toggle, and automatic Cyber access-program routing for eligible OpenAI accounts; `codex exec` can select it with an OpenAI API key when the server offers access) and `code_mode_interrupt` (interrupting a turn also stops active Code Mode cells and their nested tool calls). `api_key_cyber_access_programs`, already stable, turns on. `instant_interrupt` stays on and still under development.<sup>194</sup><sup>195</sup>

**New in the TUI.** `/config` opens a searchable preferences panel grouped into Appearance, Notifications, Input, Sessions, and Text selection tabs; supported changes apply immediately, the panel says which need a restart, and edits persist to the selected local configuration file (#52990, #53429). `/keymap` configures a **leader key**: `tui.keymap.global.leader` defaults to `ctrl-x`, bindings such as `leader c` follow it, shortcut hints stay open until you choose or cancel, and the voice mute shortcut moves from `Ctrl+X` to `F9` (#52273). `/daemon` can restart a managed background server, warning about work the restart could interrupt and reconnecting afterwards (#53173). Codex runs independent read-only tools in parallel: skill, memory, and conversation-history lookups, plugin and agent listings, current time, remaining context, and `get_goal` (#52245). A follow-up sent while Codex is responding steers it immediately (#51812, #51835).<sup>194</sup>

**Fixes and platform.** Resuming a saved thread restores its permissions and network approvals, subject to explicit overrides and managed policy (#53150, #53419, #53424). The network proxy checks permission before a DNS lookup, rejects duplicate `Host` headers, and stops credential aliases from bypassing inspection hooks (#51650, #52661, #53506). `/side` returns to an existing side conversation after a thread switch, and answers to pending prompts go back to the thread that asked (#52184, #53436). On Windows, `Ctrl+V` and `Shift+Insert` paste text as well as images; on WSL, repeated pastes reuse the clipboard reader and copy attempts time out instead of hanging (#53224, #52967, #53453). Local commands finish promptly when a child keeps an output pipe open, remote commands keep late output for the final poll, and `codex exec` no longer repeats a file diff after an unrelated command (#52652, #53331, #53622, #53444).<sup>194</sup>

**The patched `zsh` is gone.** Codex uses its standard shell executor and no longer bundles a patched `zsh`; the `codex-execve-wrapper` and `shell-escalation` crate went with it, `shell_zsh_fork` and `unified_exec_zsh_fork` are ignored with a migration notice, and managed requirements that still name them draw a warning (#52160, #52213). Linux and macOS packages are smaller, with symbols stripped from the shipped binaries (#52178). The Linux sandbox crate’s names and README now say what the sandbox has been doing: bubblewrap for filesystem and namespace isolation, seccomp for syscall filtering, with the Landlock implementation retained as legacy; enforcement did not change (#52610). The bundled OpenAI Docs guidance covers migrations to the GPT-6 family (#51768).<sup>194</sup><sup>195</sup>

What to do: upgrade. If a `config.toml` or `requirements.toml` names `shell_zsh_fork` or `unified_exec_zsh_fork`, remove it; if you had turned Daybreak on by hand, the flag is no longer needed; and if you relied on `Ctrl+X` to mute `/voice`, it is `F9` now.<sup>194</sup>

### v0.162.1 (October 9, 2026)

**Two fixes, three commits on top of v0.162.0.** A TUI crash on asynchronous questions with multiple lines is fixed, with line breaks and complete hyperlink destinations preserved. And the startup failures some v0.162.0 users hit, caused by differences between a running background server’s feature settings and the CLI’s defaults, are gone: the compatibility check now applies only to explicit command-line feature overrides. No defaults, config keys, or models change.

What to do: upgrade, especially if v0.162.0 refused to start while a background server from an earlier version was running.<sup>189</sup>

### v0.162.0 (October 8, 2026)

**`instant_interrupt` turns on by default.** The flag v0.159.0 shipped as opt-in, which lets new input preempt a model response or yield a long-running code-mode call, is on for everyone from this release (PR #51812, merged October 7); it stays marked under development. Set `instant_interrupt = false` under `[features]` to restore the old behavior, where typed input waits for the response to finish.

New in the TUI: tools for creating and listing managed Git worktrees from trusted local projects, when the worktrees feature is on, so the model can open a worktree itself (#50148); pinning in the agent Command Center with `p`, which keeps pinned tasks in a shared Pinned group when the server supports it (#51500); `/copy` navigates transcript blocks, `Ctrl+Insert` copies a selection, and `tui.mouse_scroll_speed` (a finite positive number, default 1.0) tunes mouse-wheel scrolling (#50434, #50215, #50209); URLs are clickable in approval headers, questions, MCP prompts, warnings, and banners, including links that wrap. New TUI threads respect the server’s model and reasoning-summary defaults while keeping explicit launch overrides. `codex doctor` reports the configured TUI mode, and `/status` shows the account email again after an account update.

Providers and models: a custom Responses-compatible provider can declare `external_web_access` and `remote_compaction` under `[model_providers.<name>.capabilities]` (#50459). The `ultrafast_mode` feature ships on, so a model catalog that advertises an Ultra Fast tier (Amazon Bedrock’s Astra models, #50472) can offer it independently of Fast mode; the bundled catalog at this tag lists no such tier. Code mode gains JavaScript helpers for streaming promise results and an opt-in ranked tool search (`code_mode_tool_search`). `apply_patch` always preserves existing CRLF line endings, and the `apply_patch_preserve_line_endings` flag is removed. Retryable Responses and WebSocket failures honor the server’s `Retry-After` advice.

Sandbox and platform: Linux sandbox startup with multiple denied files is fixed, writable sandbox-construction executables are rejected, and ripgrep configuration can no longer weaken deny-glob masks; Windows 10 regains ordinary drive-letter file access, sandbox temp permissions match the child process, archive checksum verification works with PowerShell 7 module paths, and Windows releases ship a signed PowerShell installer.

What to do: upgrade; if you relied on typing ahead without interrupting the model, set `instant_interrupt = false`.<sup>188</sup><sup>190</sup>

### v0.161.0 (October 7, 2026)

Cut from 0.161.0-alpha.13.1 plus one release commit, 155 commits ahead of v0.160.1. Its release notes list GPT-6.1 Sol as the new default (#49318, #49339), but those are the catalog changes v0.159.1 already shipped: the bundled `models.json` at this tag is byte-identical to v0.160.1’s, so nothing changes for anyone on 0.159.1 or later.

New: **`/mcp login <name>`** signs in to an MCP server from a running terminal session (#49290). **Voice device selection:** choose your microphone, speaker, and microphone input channels for `/voice` conversations, with preferences saved locally (#49437, #49836). **API-key model discovery is on by default** (`api_key_model_discovery`, promoted to stable, #49807): signed in with an OpenAI API key against an endpoint that supports it, Codex refreshes the model catalog from the API instead of relying only on the bundled list; `api_key_model_discovery = false` under `[features]` turns it off, and the change takes effect in a new session. Amazon Bedrock supports multi-agent V2 and Ultra reasoning on compatible models, and Bedrock Mantle accepts AWS GovCloud regions (#49345, #49813).

**Daybreak (opt-in).** A new `cli_daybreak` flag, off by default and under development, adds Daybreak controls to the CLI: a `/daybreak` toggle, indicators, and automatic Cyber access-program routing, for ChatGPT sign-ins on the OpenAI provider whose account and model catalog advertise support. Turn it on with `--enable cli_daybreak` or `cli_daybreak = true` under `[features]`; a saved `daybreak = true` preference alone does nothing. Independently, `codex exec --cyber-access-program <standard|daybreak_blue|daybreak_red>` (and the TypeScript SDK’s `cyberAccessProgram`) requests a Cyber access program for one turn, with or without the flag, and leaves the saved choice unchanged (#49856 through #49861, #49939, #51207). The bundled catalog already carries hidden `gpt-daybreak-blue-latest` and `gpt-daybreak-red-latest` entries. On by default, and marked stable, since v0.163.0.<sup>195</sup>

Fixes: an approved filesystem escalation can grant broader write access while keeping denied reads and network restrictions, and background tasks keep their originating turn’s permissions; explicit launch permissions survive terminal reconnects; elevated Windows terminal sessions can start with an embedded server; Enter submits buffered input after paste detection expires, in Vim insert mode too; thread resume includes the latest committed history, and startup detects recoverable SQLite corruption earlier, keeping damaged databases as backups; Responses retries and the WebSocket-to-HTTP fallback honor server retry guidance. The authentication docs now say credentials may live in the keyring rather than always in `auth.json`.

What to do: upgrade. Nothing here changes a default you would notice unless you sign in with an API key, in which case check `/model` for the discovered catalog.<sup>187</sup><sup>190</sup>

### v0.160.1 (October 5, 2026)

**A fix for remote MCP servers on Windows.** When Codex on a macOS or Linux host launches a stdio MCP server on a remote Windows executor, and that server’s config sets explicit remote environment variables, Codex now passes `SYSTEMROOT`, `TEMP`, and `TMP` through as well, so the Windows process keeps the startup environment it needs. Variables you did not request, secrets included, stay excluded. The release is one backport on top of v0.160.0 and changes no defaults, config keys, or models.

What to do: upgrade if you run remote stdio MCP servers on a Windows executor from a Unix host.<sup>184</sup>

### v0.160.0 (October 1, 2026)

The 0.160 line’s stable release, cut from 0.160.0-alpha.6.1 plus one backport. **No default changes.** The bundled model catalog at the tag is byte-identical to v0.159.1 and v0.159.2, so GPT-6.1 Sol stays the default, and the two new feature flags ship off. The backport (#49763) brought the frozen 0.160 candidate, branched before v0.159.1, up to the 0.159 maintenance line: the GPT-6.1 Sol bundled and Amazon Bedrock catalogs and the account-security reminders from v0.159.3. Without it, 0.160 would have shipped without GPT-6.1 Sol.

Opt-in Guardian additions (both under development, off by default):

- **Guardian can read earlier user messages.** Set`guardian_conversation_history_tools = true` under`[features]` . With Apps enabled, the reviewer gets`user_message.search_messages` and`user_message.read_messages` through the parent session’s own Apps connection, so it can check earlier instructions, restrictions, and revoked permissions before approving an action with side effects. Each call rechecks the parent’s current app and tool policy. Responses are capped at an estimated 4,000 tokens by default (`auto_review.conversation_history_max_output_tokens` ), and`auto_review.experimental_conversation_history_prompt` replaces the built-in retrieval instructions.
- **Handoff-aware root context** (`guardian_root_handoff_context` ): when Guardian reviews a worker agent’s action, it selects the three root messages before each handoff to that worker or its ancestors, plus the three latest root messages, so a recent cancellation stays visible.

Always on: encrypted agent messages now appear in Guardian transcripts, in order, instead of being dropped.

TUI changes: in a folder with no project config and no saved trust decision, a new session can start with workspace-write permissions and granular approvals, but only when local execution, your config, and managed policy all allow it. Resumed sessions restore their saved permissions unless you override them, and `/cd` can ask for folder trust with a “Keep current directory” option. The agent command center gains a “Show more” row that loads older tasks ten at a time. On local Linux X11 terminals (not Wayland, SSH, tmux, WSL, or VS Code), fullscreen transcript selections go to the primary selection, and middle-click pastes into the composer. The fullscreen status line shows `Plan mode (shift+tab to cycle)` when there is room. The Pro tiers now read “Pro 100,” “Pro 200,” and “Pro 500” in `/status` and analytics. Startup tips point to the desktop app for your platform instead of plan or Fast-mode promotions, and queued messages that were never sent resume after a reconnect without duplicates.

Providers and connections: the TUI keeps an app server’s provider, reasoning-summary, and verbosity settings, shows the right sessions in resume and fork history, and hides reasoning-summary settings in `/status` for server connections. A provider with an explicit model catalog (`model_catalog_url`) now gets only that catalog: no bundled models are merged in, metadata matches the exact model ID, a failed refresh discards cached entries instead of reusing them, and a startup with no available model reports a configuration error unless `model` is set. When a content filter stops a response, the retry tells the model to explain the limitation, offer a permitted alternative, and continue other work.

Fixes: Windows sandbox PowerShell fallbacks, ACL repair for long runtime paths, and no console windows from background helpers; subagents keep environments that are still starting; SQLite no longer stalls at connection setup and reports initialization errors instead of timeouts. The log database reclaims unused space in the background, and plugin loading caches parsed manifests and reuses HTTP connections. The provider docs now say that `requires_openai_auth` credentials use the backend chosen by `cli_auth_credentials_store`, and that `env_key` names the environment variable holding the API key.

What to do: upgrade. If a custom provider sets `model_catalog_url`, make sure its catalog lists every model you select.<sup>183</sup>

### v0.159.3 (September 30, 2026)

**Optional account-security reminders.** Local TUI sessions signed in with ChatGPT on the OpenAI provider can show a dismissible banner asking you to finish account security setup, with a button that opens chatgpt.com. The server decides who sees it; the CLI fetches the notice in the background without blocking a turn, shows nothing when the state is unknown, skips remote workspaces and FedRAMP accounts, and remembers a dismissal across threads and reconnects. Usage banners take priority. It is the release’s only change, and v0.160.0 carries the same code.

What to do: nothing. The reminder is optional.<sup>185</sup>

### v0.159.2 (September 29, 2026)

**A Windows fix for flashing console windows.** On Windows, Codex no longer opens a console window each time it starts a background process. That covers shell-tool commands, hooks, Git and npm calls, Amazon Bedrock credential refreshes run without a terminal, doctor checks, sandbox setup refreshes, and commands in the legacy Windows sandbox. Commands that need your terminal still inherit its console. The release is one backport of two fixes from main, published about three and a half hours after v0.159.1. It changes no defaults, config keys, or models, and the new launch helper behaves as before on macOS and Linux.

What to do: on Windows, upgrade if console windows flash while Codex works, especially with the background server installed.<sup>181</sup>

### v0.159.1 (September 29, 2026)

**GPT-6.1 Sol becomes the default model.** When `config.toml` sets no model, the CLI now starts on GPT-6.1 Sol (`gpt-6.1-sol`) instead of GPT-6-Astra, and GPT-6.1 Sol replaces GPT-6 Sol as the default in the Amazon Bedrock Mantle and Runtime catalogs. The `/model` picker lists it first, as “Latest workhorse model for coding and everyday work,” followed by GPT-6-Astra, GPT-6 Sol (now “Previous generation workhorse model”), GPT-6 Luna, and the older models. Saved choices are not migrated: a config that names `gpt-6-sol` or `gpt-6-astra` keeps that model, and the migration prompts for older GPT-5 selections still point to GPT-6 Sol or GPT-6 Luna.

OpenAI’s docs added the model the same day and recommended it at launch for complex coding and agentic workflows; the models page now describes it as “Near-Astra performance for complex work at a lower cost than Astra” and suggests it for repeated, long-running work.<sup>233</sup> It costs 50 credits per 1M input tokens (2.5 cached, 250 output), and the Plus band is 15-160 messages.<sup>203</sup> Two catalog details differ from GPT-6 Sol: it has no default service tier, so a session starts at Standard speed rather than Fast, and its default reasoning effort is `low` rather than `medium`. The launch rollout covered Plus, Pro, Business, Enterprise, and Edu, with Free and Go not included; the pricing page still keeps the model off in Enterprise and Edu until an administrator enables it, and the Free and Go plan cards list only GPT-6 Luna.<sup>179</sup><sup>203</sup><sup>234</sup>

What to do: upgrade, then check the model in `/status`. If your account does not have GPT-6.1 Sol yet, pin one it does have with `model` in `config.toml` or `-m`.<sup>179</sup>

### v0.159.0 (September 29, 2026)

Cut from 0.159.0-alpha.13, about 27 hours after v0.158.0. Several items in its release notes shipped earlier, in v0.158.0, as backports: the Windows console-window and launcher fixes, Markdown-preserving copy, the welcome screen and turn tips, and the sign-in fixes. The `tui.prompt_suggestions` setting the notes list as removed never appeared in a stable release.

New in this release:

- **`instant_interrupt`** (opt-in, under development): new input preempts a model response or yields a long-running code-mode call. Set`instant_interrupt = true` under`[features]` to try it. On by default since v0.162.0.
- The warnings viewer dismisses the warnings you have seen when it closes; press `k` to keep one for later.
- You can scroll the transcript while deciding whether to implement a plan.
- Mermaid rendering handles more flowchart edges, labels, and node groups.
- Blank sessions keep their drafts when you switch tasks, and threads can be archived before their first turn.
- `/status` renders without borders and wraps long values.
- App-server clients can page thread history from a specific item.

Sandbox changes: approved commands keep explicit filesystem denials, `.aws` directories under writable roots are protected by default (explicit rules still override), network-enabled macOS sandboxes allow TLS trust evaluation, and execution environments that require a proxy now get one. The bundled `plugin-creator` skill is removed.

What to do: if a sandboxed task writes under `.aws`, expect it to be blocked unless you add an explicit rule.<sup>178</sup>

### v0.158.0 (September 28, 2026)

The 0.158 line’s stable release, cut from 0.158.0-alpha.15.4: alpha.15 plus sixteen backports, including the four Windows fixes from v0.157.1. It is 207 commits ahead of v0.157.0.

Defaults that change:

- **Terminal input approval is on.** When Codex sends input to a running command that has elevated permissions (extra permission grants, a sandbox bypass, or a policy that changed since the command started), the input needs approval first. Runtime-only grants, such as those plugins use for metrics, no longer trigger a review. To turn it off, set`write_stdin_approval = false` under`[features]` .
- **Copy-on-select** in the fullscreen transcript: mouse selections are copied when you release the button. The default,`auto` , skips direct terminals that forward their own copy shortcut (Ghostty 1.2+, Kitty on macOS, Windows Terminal, and VS Code on Windows). Copies keep Markdown formatting. Set`copy_on_select = "never"` (or`"always"` ) under`[tui]` .
- **Right-click paste** in the fullscreen TUI on Windows, WSL, and Linux. Set`right_click_paste = "off"` under`[tui]` , or`"on"` to add macOS.
- **Guardian thread context is always on.** The v0.157.0 off-switch is gone:`thread_context` under`[features.guardianv2]` is ignored, including in profiles, and Codex shows a notice asking you to remove it.

Models and plans: GPT-5.4 leaves the bundled and Bedrock catalogs, and saved GPT-5.4 selections still get the migration prompt to GPT-6 Sol. Codex recognizes the Pro Max plan and labels the Pro tiers “Pro,” “Pro (More),” and “Pro (Max).” Reasoning shortcuts can reach Max.

Also new: MCP servers that need a pre-registered OAuth client secret (`codex mcp add --oauth-client-secret`), bearer tokens for exec-server WebSocket connections, transparent backgrounds for generated images, and multiline command previews in `/ps`. Fixes cover Windows sandbox failures (ordinary Windows 10 paths, rejected stored credentials, large permission policies), Linux sandbox startup with nested writable roots, Git metadata protection across writable roots, macOS path aliases in patch approvals, and Guardian reviews that now retry instead of aborting when you send a message mid-review.

What to do: if automation sends keystrokes to elevated commands, expect approval prompts or turn the feature off, and remove `thread_context` from your config.<sup>177</sup>

### v0.157.1 (September 26, 2026)

A Windows patch of four fixes backported from main. Codex no longer opens a console window when it starts the code-mode host or a local stdio MCP server. The background server can launch when Windows leaves the process inside an outer job object, and a detached background server no longer holds the launcher’s output pipes open, which could leave the calling command waiting after the launcher exited. The GitHub release notes and the changelog entry say only that highlights “could not be determined”; the list above comes from the release’s commits.

What to do: upgrade if you run Codex on Windows. The four fixes are Windows-only.<sup>175</sup>

### v0.157.0 (September 25, 2026)

The release that flips three more defaults.

- **The fullscreen transcript is on by default.** The fullscreen UI that v0.156.0 offered through`/tui` is now what a session opens in, including when config has no`[tui]` section. To keep terminal scrollback, pick Scrollback in`/tui` or set`fullscreen_transcript = false` under`[tui]` ;`--no-alt-screen` and`tui.alternate_screen = "never"` still take precedence. Shift-click now extends a transcript selection.
- **The background server starts automatically** for eligible interactive launches, and the option leaves`/experimental` . Launches with`--no-daemon` ,`--oss` ,`--profile` , or most`-c` overrides do not use it. If a running server’s settings do not match the session, Codex offers to run without it, restart it with the required settings, or cancel (the default). To turn automatic startup off, set`daemon_auto_start = false` under`[features]` .
- **Guardian thread context is on by default** for synchronous and asynchronous reviews, independent of the Guardian v2`enabled` toggle. To turn it off, set`thread_context = false` under`[features.guardianv2]` .

Models: GPT-6 Sol and Luna, first shipped in v0.156.1, are in this release too, and the Amazon Bedrock catalogs now carry them, with GPT-6 Sol replacing GPT-5.6 Sol as the Bedrock default. GPT-5.6 Sol loses its `ultrafast` service tier, leaving Fast. The bundled catalog no longer marks GPT-6-Astra as supporting experimental context management (see Context Management).

Also new: an `f` shortcut forks a conversation that is open in another app, keeping drafts and queued prompts; `/import` works in remote and background-server sessions; and the terminal renders Unicode bullets, checkboxes, and aligned equations. Fixes keep voice conversations running when you switch threads, recover unsent answers when a turn ends, respect tmux mouse settings, route realtime connections and standalone web search through configured proxies, retry transient file uploads with a five-minute timeout, and enforce network restrictions across redirects and open HTTP and WebSocket connections. On Windows, sandbox object access is limited to the runner’s logon session.<sup>175</sup>

### v0.156.1 (September 23, 2026)

A one-change patch that brings **GPT-6 Sol and GPT-6 Luna** to the stable CLI. Both now appear in the `/model` picker; in this release they sat below GPT-6-Astra, which stayed first and remained the bundled default when `config.toml` sets no model (GPT-6.1 Sol replaced it in v0.159.1). The picker relabels the GPT-5.6 family as older models (in this release GPT-5.6 Sol read “Older coding model for complex work”) and GPT-5.5 as a “Legacy coding model.” When you approach a rate limit, the switch prompt now suggests `gpt-6-luna` instead of `gpt-5.6-luna`.

If your config names an older model, expect a one-time migration prompt: saved selections of `gpt-5.6-sol`, `gpt-5.6-terra`, `gpt-5.5`, and `gpt-5.4` point to GPT-6 Sol, and `gpt-5.6-luna` and `gpt-5.4-mini` point to GPT-6 Luna. The Amazon Bedrock catalogs do not change in this release: they carry no GPT-6 Sol or Luna, and GPT-5.6 Sol stays the Bedrock default.

One cost detail the docs do not mention: the bundled catalog sets Fast (`priority`) as the default service tier for GPT-6 Sol and Luna, so a TUI session with no `service_tier` set can start in Fast mode, which draws included ChatGPT plan limits at 2.5x the Standard rate (purchased credits at 2x). `/fast` toggles the Fast tier on and off (it takes no arguments); add the speed item with `/statusline` to see which tier is active, or set `service_tier` in `config.toml` to pin one.<sup>215</sup>

What to do: upgrade, then try `codex -m gpt-6-sol` on a familiar task. At launch, availability depended on your plan, workspace settings, and the rollout, and Enterprise administrators had to enable the new models first; the models page still says an administrator must enable Luna in Enterprise and Edu.<sup>172</sup><sup>231</sup><sup>229</sup>

### v0.156.0 (September 22, 2026)

The release that flips three defaults at once; the docs changelog announced the first two the same day.

- **Voice is on by default.**`/voice` starts a live conversation on supported builds without a visit to`/experimental` . New alongside it: a configurable F8 toggle, a`/voice settings` picker for the voice used in future conversations, bundled voice runtimes for Linux and Windows, and reduced-motion support.
- **Managed worktrees are on by default.**`--worktree` and`/worktree` no longer need a feature flag. Worktree sessions can attach to an existing local daemon and can be created from the agents overview.
- **Guardian parent-compaction reuse is on by default:** when Guardian restarts a review session, it can reuse the parent thread’s encrypted compaction.

How to turn each off is in the `/voice` and `/worktree` rows of the slash-command table and in the Guardian section.

Also new: an optional **fullscreen TUI**, chosen with `/tui`, with transcript search, mouse selection and copy, right-click copy, clickable links, and a warnings viewer; six bundled themes in the `/theme` picker; the `/usage` analytics dashboard; Mermaid diagrams and TeX math rendered in the terminal, each with its own toggle; a `/daemon` menu and a `--no-daemon` flag; and reasoning summaries off by default for new TUI threads. Security fixes close Windows inbound-traffic, privileged-socket, and read-only-file-handle sandbox gaps, and `codex doctor` no longer echoes config values in its errors. Bug fixes keep streamed answers after failed or interrupted turns, repair clipboard forwarding in tmux and SSH, restore Plan mode on resume, and let login recover through system proxies.<sup>170</sup>

### v0.155.1 (September 18, 2026)

A one-fix hotfix. On 0.155.0, a new local TUI thread with no `model_reasoning_summary` in config requested `detailed` summaries, and providers that do not support reasoning summaries rejected the request outright. 0.155.1 restores `none` as the fallback, so an unset key requests no summaries. Explicit `auto`, `concise`, and `detailed` settings were always honored and still are. The same one-line change also landed on main and shipped in 0.156.0.

What to do: if new TUI sessions on 0.155.0 failed against a non-OpenAI or self-hosted provider, upgrade. If you want summaries, set the key explicitly (see the config reference).<sup>167</sup>

### v0.155.0 (September 17, 2026)

Voice arrived in the stable TUI as an experimental, opt-in feature: `/voice` opened a live conversation with a streaming transcript and microphone controls, enabled through `/experimental` on supported builds, with a configurable mute shortcut and signed voice resources in the macOS packages. v0.156.0 turned it on by default five days later. Managed worktrees stayed off by default in this release.

Also in 0.155.0:

- **MCP user verification:** Touch ID verification of MCP requests, signed by the Secure Enclave, in local TUI sessions on supported Macs, with workspace-scoped identity restriction and native verification in MCP tool continuations.
- **The Windows `/sandbox-add-read-dir` slash command is removed.**
- **Worktree management:** hide, archive, and delete worktree sessions in the agents overview, see ownership details, and confirm before deleting a clean managed worktree.
- **Live reasoning summaries** stream in the TUI status row, with completion timestamps on finished work.
- **Daemon:** configurable update schedules, a`codex app-server daemon update` command, and thread recovery after daemon restarts.
- Amazon Bedrock AWS credential commands, and Python SDK publishing aligned with stable CLI releases.

Security fixes close WSL interop escapes and shell-snapshot credential exposure. Memory v2 lands as storage and consolidation groundwork only, not a usable feature.<sup>165</sup>

### v0.154.0 (September 9, 2026)

This release was cut at roughly the 0.154.0-alpha.6 boundary, so everything that first appeared in alpha.7 (live TUI voice, MCP user verification, Memory v2, daemon auto-update configuration, reasoning-effort pinning) waited for 0.155.

Removed: **`codex mcp-server`** and the standalone `codex-mcp-server` binary. Move integrations to the app-server protocol or the Codex plugin for Claude Code; `codex mcp`, which connects Codex to external MCP servers, is unaffected (see Running Codex AS an MCP Server). The legacy Guardian approval review paths are gone too, and approvals are now rejected when new user instructions invalidate them.

New:

- **Experimental managed worktrees** via`--worktree` or`/worktree` , with`codex exec` support and a TUI worktree browser.
- **Asynchronous user messages:** answer inline questions while Codex keeps working, without losing your draft.
- **Windows service support:** sandbox provisioning, the app-server daemon on Windows, and managed daemon updates.
- **Remote permission profiles** , discovered from the app server and selectable by name in the TUI.
- **Vim `R` replace mode** , live context-compaction status, and experimental context management gated by model capability at session startup.

Security: PATH helpers no longer run before workspace trust is established, and the macOS sandbox is hardened against terminal input injection. MCP connections coordinate OAuth token refreshes, remote resume and fork keep saved permissions, and resuming a conversation open in another app shows a read-only transcript. Voice shipped only as runtime infrastructure, and the first Daybreak code references landed with no public docs.<sup>161</sup><sup>162</sup>

### v0.153.3 and v0.153.4 (September 4, 2026)

Same-day patches that carried the GPT-6-Astra launch into the CLI. v0.153.3 added Astra to the Amazon Bedrock catalogs (GPT-5.6 Sol stayed the Bedrock default) and corrected Astra’s async-clarification guidance. v0.153.4 put Astra in the model picker, made it the **bundled default when no model is configured**, and qualified async-question guidance by tool availability.

What to do: Astra works with `-m gpt-6-astra` from v0.153.1 and appears in the picker from v0.153.4; no minimum CLI version is documented, so treat those as the floors. If your account is not entitled to the model the bundled default selects, set `model` explicitly in `config.toml` rather than relying on the default.<sup>154</sup><sup>155</sup><sup>156</sup><sup>157</sup>

### v0.153.1 and v0.153.2 (September 3, 2026)

v0.153.1 makes Guardian’s computer-use scoring honor model requirements: computer-use-only scoring and fast approval decisions run only when the active model’s metadata sets `node_repl_auto_review_required`, and a model switch that skips scoring invalidates prior or in-flight scores, so switching back cannot revive a stale approval. The release body lists only a backported GPT-6-Astra catalog entry, configurable through the API; the picker and default did not change until v0.153.4. v0.153.2 corrects the Astra Fast-tier description from “1.5x” to “2x speed, increased usage” (display text only).<sup>151</sup><sup>152</sup>

### v0.153.0 (September 3, 2026)

- **Vim undo and redo:**`u` and`Ctrl+R` restore complete composer drafts, including pasted content and image attachments;`vim_normal.redo` is a configurable keymap action.
- **Remote plugin marketplaces:** the plugin CLI can list, install, and remove remote plugins, and`codex plugin list` shows source, version, install policy, and authentication policy.
- **`tui.auto_recap = false`** turns off automatic recaps; manual`/recap` still works.
- **Earlier rate-limit warnings for Plus and Team** when less than half the allowance remains in a roughly five-hour window; other plans keep the 75/90/95% thresholds.
- **Guardian scope follows approval mode:** Full Access skips Guardian reviews for confirmation-only actions, and User approval mode skips background scoring and prewarming.
- **`features.context_management.experimental_mode`** (off by default) enables token-budget context, history notes, and the`new_context` tool for eligible Plus, Pro, and Pro Lite sessions on the Codex backend. The flag is still under development at v0.167.1, and the config reference says the feature “is not currently available.”<sup>247</sup>
- **`tui.disable_paste_burst`** replaces the top-level key, which still works as a fallback.

Fixes: TUI sessions reconnect after an app-server drop with drafts intact, Guardian review history survives compaction, restarts, and forks, and remembered MCP approvals are scoped to the selected account.<sup>149</sup>

### v0.152.0 and v0.152.1 (September 1, 2026)

`update_plan` became opt-in:`tools.update_plan.enabled` defaults to `false`, and while it is off the bundled `update_plan` guidance is removed from model, collaboration-mode, multi-agent, compaction, prewarm, and goal-continuation prompts (see Plan Mode & Collaboration). Also new: Vim search with `/` and `?` plus `n`/`N`, composable with operators, with fresh drafts starting in Insert mode; actionable rate-limit banners; credential-refresh progress, including Amazon Bedrock reauthentication; package-style MCP server names (`:`, `@`, `/`, and `.` are allowed); MCP `openai/elicitation` form requests; a per-tool MCP `output_token_limit`; cloud task credentials restricted to trusted origins; configurable `thread/shellCommand` timeouts; and token budgeting on by default through model metadata. v0.152.1 lets Guardian approval review source its Node REPL policy from model metadata.<sup>147</sup><sup>148</sup>

### v0.151.0 (August 29, 2026)

- **`/recap`** summarizes the conversation on demand, and Codex recaps eligible idle, unfocused conversations automatically.
- Code-mode hosts drop the WebSocket transport (stdio and gRPC only).
- Durable threads default to paginated history where the store supports it.
- The **`persistent` reasoning effort** arrives with clock tools and bundled proactivity guidance.
- **Guardian v2 defaults to computer-use reviews with images.**
- Vim gains `f` /`F` /`t` /`T` find and till motions plus`gg` /`G` .
- Input to escalated terminals can require fresh approval (`write_stdin_approval` , off by default).
- A configurable grace period for optional MCP servers (`mcp_optional_startup_grace_ms` , default 1,000 ms), and extensions can inspect or replace MCP tool results.

Fixes stop `/cd` from weakening sandbox restrictions and stop stale Guardian classifications from authorizing actions after permission changes. The release body lists only the last few PRs; the full change set is in the compare.<sup>144</sup><sup>145</sup>

### v0.150.0 and v0.150.1 (August 26-27, 2026)

Hooks gain an **`Interrupt` event** that runs when an active top-level turn is interrupted (1-second default timeout, 3-second cap, never for subagents). `/copy` opens a picker for the full response, individual code blocks, or blockquotes. Tasks become addressable with `@` mentions, agents can read, create, and message tasks, and `/rename` suggests an editable title. Markdown links render as clickable labels, shortcuts can cycle permission modes, Vim gains `.` repeat, and **untrusted projects no longer supply project-level AGENTS.md instructions**. `features.network_proxy.credential_broker` and Guardian v2’s `review_scope.computer_use_only` arrive as configurable, default-off surfaces. v0.150.1 counts retained images toward remote compaction’s token budget.<sup>141</sup><sup>142</sup>

### v0.149.0 and v0.149.1 (August 20-24, 2026)

The **`untrusted` approval policy is retired**; move configs that name it to `on-request`. New: an interactive `codex agents` dashboard, `/cd`, `/pwd`, and `/cwd`, `codex queue` for messaging existing local or remote sessions, more Vim change motions, a broader `codex doctor` (endpoint protection, network and proxy failures, desktop-app state, update connectivity), and `max`/`ultra` reasoning efforts in the SDKs. Resumed and forked threads restore their permission profile. v0.149.1 adds `codex exec --thread-source <SOURCE>` (`user`, `subagent`, `memory_consolidation`, or a feature name; `threadSource` in the TypeScript SDK).

Pricing in the same window: OpenAI cut GPT-5.6 Sol pricing by more than 20% for three months from August 21 (through at least November 21, 2026), and the Codex credit rate for Sol dropped from 125/12.5/750 to 100/10/500 credits per 1M input, cached, and output tokens.<sup>129</sup><sup>130</sup><sup>135</sup>

### v0.147.0 and v0.148.0 (August 7-18, 2026)

v0.147.0 **removes `codex exec --full-auto`** (deprecated since v0.128); use `--sandbox workspace-write`, or the `--approve-for-me` flag that the same release adds for automatically reviewed approvals. It also adds portable Agent Plugins with search across catalogs, the opt-in MCP 2026-07-28 protocol, and conversation sections. v0.148.0 adds `/export` to Markdown, `codex exec fork`, estimated credits or cost in `/status`, Amazon Bedrock Runtime as a built-in provider, asynchronous and MCP-invoking hooks, and sandbox restrictions that fail closed on denied or unreadable paths.<sup>123</sup><sup>127</sup>

### Earlier Releases (May-July 2026)

- **v0.146.0 and v0.146.1 (July 29, August 5):** configured proxies cover every transport; Agent Plugins manifests plus the Amazon Bedrock and Claude Code marketplaces; session naming with thread pinning; executor-provided skills; hardened automatic-review defaults for cyber-capable models.<sup>121</sup><sup>122</sup>
- **v0.145.0 (July 21):**`/import` from Cursor as well as Claude Code; voice returns as streaming realtime V3 conversations; the opt-in multi-agent v2 experience is stabilized; CSV-backed agent jobs (`spawn_agents_on_csv` ) are removed; experimental paginated thread history and Amazon Bedrock login; bundled GPT-5.4 selections migrate to GPT-5.6 Terra and Luna. v0.144.6 set the GPT-5.6 context windows to 272,000 tokens.<sup>116</sup><sup>117</sup><sup>241</sup>
- **v0.140.0 (June 15):**`/usage` ,`codex delete` and`/delete` ,`/import` from Claude Code, the unified`@` mentions menu, Amazon Bedrock API-key authentication, and encrypted local storage for CLI and MCP OAuth credentials; the old`/realtime` voice controls were removed.<sup>107</sup>
- **v0.139.0 (June 9):** code mode can call web search directly, tool schemas keep`oneOf` /`allOf` ,`codex doctor` reports editor and pager details, and marketplace sources appear in JSON output.<sup>106</sup>
- **v0.138.0 (June 8):**`/app` hands a CLI session to the desktop app, and parent-to-subagent task text is encrypted in multi-agent v2.<sup>105</sup><sup>113</sup>
- **v0.137.0 (June 4):** multi-agent v2, F13-F24 TUI keybindings, and a v1 skills extension.<sup>104</sup>
- **v0.132.0 through v0.135.0 (May 20-28):** local conversation history search,`--profile` as the primary profile selector, goals on by default, permission-profile inheritance and managed`requirements.toml` ,`codex remote-control` start and stop, richer`codex doctor` and`/status` , Vim text objects, named permission profiles in`/permissions` , Python SDK auth and`Sandbox` presets, and`codex exec resume --output-schema` . The May 21 app update added Appshots, Goal mode GA, and opt-in locked Computer Use.<sup>96</sup><sup>97</sup><sup>98</sup><sup>99</sup><sup>100</sup><sup>101</sup><sup>102</sup>

Legacy `--full-auto` is removed as of v0.147.0, and `js_repl` remains removed.<sup>86</sup><sup>87</sup><sup>89</sup><sup>91</sup><sup>123</sup>

**Stability note:** Stable, documented surfaces: the core CLI and TUI, sandboxing and approval policies, `config.toml`, AGENTS.md, skills, hooks, plugins, multi-agent tools, goals, permission profiles, `/voice` and managed worktrees (default since v0.156.0), the fullscreen transcript and automatic background-server startup (default since v0.157.0), terminal input approval, copy-on-select, and right-click paste (default since v0.158.0), instant interrupt (default since v0.162.0, still marked under development), Daybreak and code-mode interruption (`cli_daybreak`, `code_mode_interrupt`; stable and on since v0.163.0), and the Bedrock setup wizard, incremental tool updates, and MCP OAuth refresh coordination (stable and on since v0.164.0), mid-conversation reasoning-effort changes (`reasoning_effort_override`, stable and on since v0.165.0), memory v2 for fresh profiles (v0.165.0), and deferred tool world state (`deferred_tool_world_state`, stable and on since v0.167.0). Managed worktrees and realtime conversation are permanent as of v0.164.0: their flags are marked removed with the features left on. Browser, Computer Use, and Appshots are documented user-facing surfaces whose availability depends on platform and plan.

Experimental or under development: the `codex cloud` command, code mode and code mode tool search, external-agent memory import (`external_agent_memory_import`), the local agent message board, the Guardian history and handoff-context flags (v0.160.0), and anything this guide marks experimental or lists under `/experimental`; these can change between releases. Experimental context management is, per the config reference, “not currently available.”

Removed: spacebar voice transcription and custom prompts (v0.118.0), `js_repl` (v0.128.0), the top-level `profile` selector and `[profiles.*]` tables (v0.134.0), CSV agent jobs (`spawn_agents_on_csv`, v0.145.0), `--full-auto` (v0.147.0), the `untrusted` approval policy (v0.149.0), `codex mcp-server` (v0.154.0), the repository’s devcontainer profiles (v0.155.0), the Guardian `thread_context` opt-out (v0.158.0), and the `shell_zsh_fork` and `unified_exec_zsh_fork` flags with the patched `zsh` itself (v0.163.0).<sup>237</sup><sup>238</sup><sup>240</sup><sup>241</sup><sup>248</sup>

**Alpha channel:** alpha builds ship several times a day, release bodies on the alpha lines are stubs, and out-of-band builds cut from older bases appear regularly. At least twice the npm `alpha` tag pointed at one of them, so `@alpha` installed a build older than `latest`. If you track alphas, check `npm view @openai/codex dist-tags` and install an exact version rather than `@alpha`; on November 11 (20:15 UTC) the tag pointed at 0.170.0-alpha.1.<sup>177</sup><sup>180</sup><sup>182</sup><sup>186</sup><sup>201</sup> A dated history of how earlier alpha lines resolved into stable releases is in <sup>174</sup>. How the recent lines resolved:


**0.158 and 0.159:** the 0.158 line ran from 0.158.0-alpha.2 (September 23) to alpha.15 (September 25); its stable, v0.158.0 (September 28), was cut from the out-of-band build 0.158.0-alpha.15.4, alpha.15 plus sixteen backports, and was npm `latest` until v0.159.0 published 27 hours later. The 0.159 line ran from alpha.1 (September 25) to alpha.13 (September 28), shipped as v0.159.0 from alpha.13, and got one more patch, v0.159.3, on September 30.<sup>177</sup><sup>180</sup><sup>182</sup>
**0.160:** the line reached 0.160.0-alpha.6 (September 29) and froze there; 0.161.0-alpha.1 is alpha.6 with only the version number changed. On September 30, 0.160.0-alpha.6.1 added to alpha.6 the one v0.159.2 Windows console fix it lacked. The line shipped stable as v0.160.0 (October 1), cut from alpha.6.1 plus one backport, and v0.160.1 (October 5).<sup>182</sup><sup>183</sup><sup>184</sup>
**0.161 and 0.162:** v0.161.0 shipped on October 7 from 0.161.0-alpha.13.1 (one release commit on top of it); v0.162.0 followed on October 8 from 0.162.0-alpha.18.1, while 0.162.0-alpha.20 ran 52 commits ahead of that stable cut; v0.162.1 added three commits on October 9.<sup>186</sup><sup>187</sup><sup>188</sup><sup>189</sup>
**0.163:** `alpha` was 0.163.0-alpha.5 on October 10, moved through alpha.8, alpha.9, and alpha.10 on October 13, and was 0.163.0-alpha.11 on October 14. v0.163.0 was cut on October 15 from the out-of-band 0.163.0-alpha.10.2 (one release commit on top), while alpha.11 ran 23 commits ahead of that cut.<sup>190</sup><sup>192</sup><sup>193</sup><sup>195</sup>
**0.164:** the line opened on October 16 with alpha.1 and alpha.2 and reached alpha.12 by October 22, with the out-of-band builds 0.163.0-alpha.11.1 and 11.2 (October 21 and 22) and 0.164.0-alpha.11.1 (October 23) alongside. v0.164.0 shipped on October 23, cut from alpha.11 plus one release commit (alpha.12 ran nine commits ahead of that cut), and `alpha` moved to 0.165.0-alpha.1 the same day.<sup>195</sup><sup>197</sup>
**0.165 and 0.166:** v0.165.0 shipped on October 27, cut from 0.165.0-alpha.1 plus four commits, with the out-of-band 0.165.0-alpha.1.1 and 1.2 the same day; the 0.166 line ran alpha.1 (October 27) to alpha.6 (October 29) with 0.166.0-alpha.5.1 out of band and shipped as v0.166.0 on October 29 from alpha.6 plus one release commit.<sup>198</sup><sup>199</sup>
**0.167, 0.168, and 0.169:** the 0.167 line opened with alpha.2 and alpha.3 on November 1 (no alpha.1 was published) and reached alpha.5 on November 4; v0.167.0 shipped on November 6 from the out-of-band 0.167.0-alpha.3.2 plus one release commit, leaving alpha.5 with 132 commits the stable does not have, and v0.167.1 added three commits on November 10. Two more out-of-band builds followed from the alpha.4 base, alpha.4.1 on November 9 and alpha.4.2 on November 11. The 0.168 line produced alpha.1 (November 4) through alpha.7 (November 5) and no stable; it is not called skipped, because no higher version has shipped stable. The 0.169 line has been on npm since alpha.2 (November 6), and the 0.170 line opened with alpha.1 on November 11.<sup>200</sup><sup>201</sup>

## How Codex Works: The Mental Model

Before diving into features, understand how Codex’s architecture shapes everything you do with it. The system operates across five surfaces backed by a shared intelligence layer:

```
┌─────────────────────────────────────────────────────────┐
│                    CODEX SURFACES                       │
├─────────────────────────────────────────────────────────┤
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌────────┐   │
│  │   CLI    │  │ Desktop  │  │   IDE    │  │ Cloud  │   │
│  │ Terminal │  │   App    │  │Extension │  │  Tasks │   │
│  └──────────┘  └──────────┘  └──────────┘  └────────┘   │
│  Local exec     Multi-task    Editor-native  Async      │
│  + scripting    + worktrees   + inline edits detached   │
│  ┌──────────┐                                           │
│  │ Browser  │  Signed-in browser workflows              │
│  │Extension │  (launched May 2026)                      │
│  └──────────┘                                           │
├─────────────────────────────────────────────────────────┤
│  EXTENSION LAYER                                        │
│  ┌─────────┐  ┌─────────┐  ┌─────────┐  ┌─────────┐     │
│  │   MCP   │  │ Skills  │  │  Apps   │  │  Search │     │
│  └─────────┘  └─────────┘  └─────────┘  └─────────┘     │
│  External tools, reusable expertise, ChatGPT            │
│  connectors, web search (cached + live)                 │
├─────────────────────────────────────────────────────────┤
│  SECURITY LAYER                                         │
│  ┌─────────────────────────────────────────────────┐    │
│  │    Sandbox (Seatbelt / bubblewrap / seccomp)    │    │
│  │    + Approval Policy (on-request → never)       │    │
│  └─────────────────────────────────────────────────┘    │
│  OS-level filesystem + network restrictions             │
├─────────────────────────────────────────────────────────┤
│  CORE LAYER                                             │
│  ┌─────────────────────────────────────────────────┐    │
│  │        GPT-6 models (GPT-6.1 Sol default)       │    │
│  │   Tools: Shell, Patch, Read, Web Search         │    │
│  │   (legacy artifact, read_file, grep_files       │    │
│  │    removed in v0.117.0)                         │    │
│  └─────────────────────────────────────────────────┘    │
│  Shared model across all surfaces; costs tokens         │
└─────────────────────────────────────────────────────────┘
```
**Core Layer**: The GPT model family powers everything. The current generation is GPT-6: **`gpt-6.1-sol`** for complex coding and repeated, long-running work (the bundled CLI default when no model is configured, since v0.159.1), **`gpt-6-astra`** for the hardest end-to-end work, **`gpt-6-luna`** for focused, high-volume tasks, **`gpt-6.1-terra`** for everyday writing and visual work, and the previous-generation **`gpt-6-sol`**. The GPT-5.6 models (Sol, Terra, Luna) stay in the picker as older models. `gpt-5.5` retired from Codex for ChatGPT sign-in on October 14, 2026 and `gpt-5.4` on August 31, 2026; the API and API-key sessions are unaffected by either retirement.<sup>179</sup><sup>202</sup><sup>205</sup><sup>229</sup><sup>233</sup><sup>234</sup> It reads files, writes patches, executes shell commands, and reasons about your codebase. When context fills, Codex compacts the conversation to free space. This layer costs tokens.

**Security Layer**: Every command Codex runs passes through an OS-level sandbox. On macOS, Apple’s Seatbelt framework enforces kernel-level restrictions. On Linux, bubblewrap isolates the filesystem and namespaces and seccomp filters syscalls; the older Landlock implementation is retained as legacy. The sandbox operates at the kernel level, not inside containers. The approval policy then decides when to ask for human confirmation.

**Extension Layer**: MCP connects external services (GitHub, Figma, Sentry). Skills package reusable workflows that Codex loads on demand. Apps connect to ChatGPT connectors. Web search adds real-time context from the internet.

**Surface Layer**: CLI for terminal power users and automation. Desktop app for multi-threaded project management. IDE extension for edit-compile-test loops. Cloud for async tasks that run independently.

**The key insight**: Most users only use one surface. Power users use all five: Cloud for long-running tasks, CLI for deterministic repo operations, IDE extension for tight coding loops, the desktop app for planning and coordination, and the browser extension for signed-in browser workflows.

## Table of Contents

## How Do I Install Codex?

### Standalone Installer

The OpenAI docs list the standalone installer first. Run the same command again to update:<sup>223</sup>

```
# macOS and Linux
curl -fsSL https://chatgpt.com/codex/install.sh | sh
```
```
# Windows (from a new PowerShell window)
powershell -ExecutionPolicy ByPass -c "irm https://chatgpt.com/codex/install.ps1 | iex"
```
### Package Managers

```
# npm
npm install -g @openai/codex
# Homebrew (macOS)
brew install --cask codex
# winget (Windows)
winget install OpenAI.Codex
# Upgrade to latest
npm install -g @openai/codex@latest
```
### How Do I Update, Downgrade, or Uninstall the Codex CLI?

Codex ships a self-update command as of v0.128.0; everything else runs through whichever
package manager installed it.<sup>86</sup> As of v0.146.0, release artifacts, channel metadata, and installer aliases are served from **OpenAI-hosted release infrastructure, with GitHub as a fallback**.<sup>121</sup> If your network allowlists GitHub for Codex installs and updates, add the OpenAI release host too — the fallback means an allowlist miss degrades quietly rather than failing loudly.

```
# Check what you are running
codex --version                          # -> codex-cli 0.167.1
npm view @openai/codex version           # Latest published version
# Update
codex update                             # Built-in self-update (v0.128.0+)
npm install -g @openai/codex@latest      # Or update via npm
brew upgrade --cask codex                # Or via Homebrew
# Pin or downgrade to an exact version
npm install -g @openai/codex@0.167.1
# Uninstall
npm uninstall -g @openai/codex
brew uninstall --cask codex
winget uninstall OpenAI.Codex
```
**Pinning is the right move when a release breaks you.** Codex tracks a fast stable line, so
installing an exact version is the fastest rollback – file the issue, pin, and move on.
Verify the rollback took with `codex --version`, since a shadowed second install on your
`PATH` will silently keep serving the old binary (`codex doctor` flags exactly this under
**Environment**).

**Uninstalling the package does not remove your state.** Auth, config, and session history
live in `~/.codex/`. Leave it in place to preserve logins and history across a reinstall;
delete it only for a genuinely clean slate – `rm -rf ~/.codex` also discards `config.toml`,
`auth.json`, and every stored session.


### Direct Install Script (v0.106.0+)

Each GitHub release also carries the install script as an asset (`install.sh` for macOS and Linux, `install.ps1` for Windows), which is useful when you want to pin the source to GitHub:<sup>60</sup>

```
curl -fsSL https://github.com/openai/codex/releases/latest/download/install.sh | sh
```
The script auto-detects your platform and architecture, downloads the correct binary, and places it on your PATH.

### Binary Downloads

For environments without npm or Homebrew, download platform-specific binaries from GitHub Releases<sup>1</sup>:

| Platform | Binary | 
|---|---|
| macOS Apple Silicon | `codex-aarch64-apple-darwin.tar.gz` | 
| macOS x86_64 | `codex-x86_64-apple-darwin.tar.gz` | 
| Linux x86_64 | `codex-x86_64-unknown-linux-musl.tar.gz` | 
| Linux arm64 | `codex-aarch64-unknown-linux-musl.tar.gz` | 
| Windows x86_64 | `codex-x86_64-pc-windows-msvc.exe.zip` | 
| Windows arm64 | `codex-aarch64-pc-windows-msvc.exe.zip` | 

### System Requirements

- **macOS** : Apple Silicon or Intel (full sandbox support via Seatbelt)
- **Linux** : x86_64 or arm64 (sandbox via bubblewrap + seccomp)
- **Windows** : Windows 11 recommended (a fully updated Windows 10 is best effort). Three native sandbox implementations:`mxc` (recommended on compatible Windows 11 builds), then the legacy`elevated` and`unelevated` fallbacks; details under Platform-specific enforcement. WSL2 is also supported and uses the Linux sandbox<sup>2</sup>

### Authentication

```
codex login                  # Interactive OAuth (recommended)
codex login --device-auth    # OAuth device code flow (headless)
codex login --with-api-key   # API key from stdin
codex login status           # Check auth state (exit 0 = logged in)
codex logout                 # Clear stored credentials
```
Two authentication paths:

1. **ChatGPT Account** (recommended): Sign in with your existing Plus, Pro, Business, Edu, or Enterprise subscription. Full feature access including cloud tasks. The pricing page lists the CLI from Plus up; Free and Go get GPT-6 Luna at Standard speed in the desktop app, subject to rollout.<sup>203</sup>
2. **API Key** : for the interactive CLI, pipe the key in:`printenv OPENAI_API_KEY | codex login --with-api-key` . The`CODEX_API_KEY` environment variable is for non-interactive entry points:`codex exec` ,`codex review` , the TypeScript SDK, and`codex exec-server --remote` . Some features (cloud threads) may be unavailable.<sup>224</sup>

**Expert tip**: Credential storage is configurable via `cli_auth_credentials_store` in `config.toml`. Options: `file` (default; `auth.json` under `CODEX_HOME`), `keyring` (OS credential store; fails if it is unavailable), `auto` (keyring if available, file fallback), or `ephemeral` (credentials kept in memory for the current process only).<sup>206</sup>


**Amazon Bedrock login (v0.145.0, experimental):** Teams whose model access runs through Amazon Bedrock get an experimental login path with custom endpoint and authentication support, alongside the existing managed API-key authentication (v0.140.0+) and the AWS credential chain (see Custom Model Providers). GPT-5.6 Sol was the default Bedrock model at v0.145.0;<sup>117</sup> GPT-6 Sol took over in v0.157.0 and GPT-6.1 Sol in v0.159.1, and it is still the default at v0.167.1.<sup>175</sup><sup>179</sup><sup>201</sup> v0.150.0 fixes conversation compaction and multi-agent compatibility for Amazon Bedrock models.<sup>141</sup> **v0.151.0 adds a feature-gated Amazon Bedrock option to eligible TUI sign-in flows**: onboarding discovers existing AWS profiles and environment credentials, offers manual setup for profiles, access keys, environment variables, and Bedrock API keys, collects the AWS Region, masks secrets during entry, and restarts the embedded server when the provider changes.<sup>145</sup> **v0.152.0 surfaces credential-refresh progress**: the TUI and `codex exec` show progress while a model provider refreshes expired credentials, Amazon Bedrock session reauthentication included, backed by stable `modelProvider/authRecoveryStarted` and `modelProvider/authRecoveryCompleted` app-server notifications carrying the thread, turn, provider, and a user-facing message.<sup>147</sup>

**Sign in another way (v0.167.0):** the sign-in screen groups API-key and cloud-provider options under “Sign in another way.” With `gcp_setup_wizard = true` under `[features]`, it also offers Google Cloud Vertex AI setup with Application Default Credentials or a credential file; the flag ships off, and the `gcp` provider can always be configured by hand (see Custom Model Providers).<sup>200</sup><sup>201</sup>

### Shell Completions

```
# Generate completions for your shell
codex completion bash > /etc/bash_completion.d/codex
codex completion zsh > ~/.zsh/completions/_codex
codex completion fish > ~/.config/fish/completions/codex.fish
```
### Verify Installation

```
codex --version
# codex-cli 0.167.1
```
## Quick Start: Your First Session

Get from zero to productive in 5 minutes.

**1. Install and authenticate:**

```
npm i -g @openai/codex          # Install
codex login                      # Sign in with ChatGPT (opens a browser)
```
**2. Navigate to a project:**

```
cd ~/my-project                  # Any git repo works
```
**3. Start Codex:**

```
codex
```
You’ll see the interactive TUI. Codex reads your project structure automatically.

**4. Ask a question:**

```
> What does this project do? Summarize the architecture.
```
Codex reads key files and explains the codebase. A question like this makes no changes — Codex only patches files when you ask for a change, and the sandbox and approval policy govern when it must ask first.

**5. Make a change:**

```
> Add input validation to the login endpoint
```
Codex shows its edits as a diff. Whether it asks before applying them depends on the sandbox and approval policy. When it does ask, the prompt lists the choices: by default `y` is “Yes, proceed”, `a` is “Yes, and don’t ask again for these files”, and `n` or `Esc` is “No, and tell Codex what to do differently”.<sup>236</sup>

**6. Use a slash command:**

```
> /plan Refactor the database layer to use connection pooling
```
Codex creates a plan without executing. Review the plan, then approve to begin execution.

**7. Check your work:**

```
> /diff
```
See all changes Codex has made in the current session.

**What’s next:**

- Set up `AGENTS.md` with project instructions (see How Does AGENTS.md Work?)
- Configure a profile for your workflow (see Profiles)
- Try `codex exec` for non-interactive automation (see Non-Interactive Mode)

## Core Interaction Surfaces

Codex provides five distinct interfaces backed by the same intelligence. Each surface optimizes for a different workflow pattern.

### 1. Interactive CLI (Terminal UI)

```
codex                        # Launch TUI
codex "fix the failing tests" # Launch with initial prompt
codex -m gpt-6.1-terra      # Specify model
codex --sandbox workspace-write --ask-for-approval on-request
```
The terminal UI is a full-screen application with:

- **Composer** : Type prompts, attach files with`@` , run shell commands with`!` prefix
- **Output pane** : Streaming model responses, tool calls, and command output
- **Status bar** : Model, token usage, git branch, sandbox mode

**Key TUI shortcuts:**

| Shortcut | Action | 
|---|---|
| `@` | Fuzzy file search (attach to context) | 
| `!command` | Run shell command directly | 
| `Ctrl+G` | Open external editor ( `$VISUAL` /`$EDITOR` ) | 
| `Ctrl+R` | **Reverse history search (v0.121.0+)** — readline-style search through prior prompts, including slash commands issued in earlier turns<sup>82</sup> | 
| `Enter` (while running) | Inject new instructions mid-turn | 
| `Esc` twice | Edit previous messages | 
| Arrow keys | Navigate draft history | 

**Status line change (v0.121.0):** The old context-window meter in the status line has been replaced by a **context-percent indicator** showing how full your context window is. If you have scripts or hooks parsing the status line, check for this format change.<sup>82</sup> Codex will also surface a CLI update announcement when a new version is available.


**Slash commands** available in the TUI:

| Command | Description | 
|---|---|
| `/quit` or`/exit` | Exit the CLI | 
| `/new` | Start new conversation in same session | 
| `/resume` | Resume a saved conversation; since v0.167.0 the picker switches between connected backends, keeps your draft when you switch, and marks sessions with unread results <sup>200</sup> | 
| `/fork` | Fork current conversation into new thread | 
| `/worktree` | **Create an isolated managed-worktree checkout for a new or forked session (v0.154.0; on by default since v0.156.0).** Also`--worktree` at launch;`codex exec` supports managed worktrees, and the TUI adds worktree session commands and a browser for resuming worktree sessions.**v0.155.0:** the agents overview adds hide, archive, and delete for worktree sessions, ownership details, and confirmed deletion of clean managed worktrees (#43942, #44424, #44433).**v0.156.0:** worktrees become a stable, default-on feature (#44870), worktree sessions can use an existing local daemon (#46498), and worktree sessions can be created from the agents overview (#45276).**No off switch since v0.164.0:** the`worktrees` flag is removed with the feature left on, and Codex skips the key when it loads`[features]` , so the`worktrees = false` opt-out that worked from v0.156.0 through v0.163.0 no longer does anything.<sup>197</sup><sup>201</sup>**If worktree creation or `/cd` fails with “The local Codex service cannot check background terminals”** , your local daemon predates the method worktrees need: run`codex app-server daemon update` , then restart Codex (#44870).<sup>161</sup><sup>165</sup><sup>170</sup><sup>172</sup>**v0.162.0:** the model gets tools to create and list managed worktrees itself, in trusted local projects (#50148).<sup>188</sup> | 
| `/model` | Switch model and reasoning effort; since v0.164.0 `/model [model] --reasoning <level> --speed <tier>` applies to the current session without changing saved defaults (space or`=` flag values; a bare`/model` opens the picker)<sup>196</sup> | 
| `/presets` | Ten numbered slots for reusable prompts and slash commands, with a keyboard menu; a preset can capture composer text or the current model, reasoning effort, and speed, and persists under `tui.presets` in the active user configuration (v0.164.0)<sup>196</sup> | 
| `/compact` | Summarize conversation to free tokens | 
| `/recap` | **Summarize the current conversation now (v0.151.0).** On-demand version of the recap Codex generates automatically for eligible idle, unfocused conversations, with visible progress and actionable errors.<sup>145</sup>**v0.153.0:**`tui.auto_recap = false` turns off the automatic recaps; manual`/recap` stays available regardless.<sup>149</sup> | 
| `/diff` | Show git diff including untracked files | 
| `/review` | Code review of working tree | 
| `/plan` | Enter plan mode | 
| `/goal` | Create, pause, resume, or clear persisted work goals (v0.128.0+). **v0.133.0:** goals are enabled by default, backed by dedicated storage, and track progress across active turns; OpenAI now documents Goal mode as generally available across the Codex app, IDE extension, and CLI.<sup>98</sup><sup>100</sup> | 
| `/vim` | **Toggle modal Vim editing in the composer (v0.129.0+).** Configure via TUI keymap settings to set Vim as the default editing mode.**v0.150.0:** use`.` to repeat your last edit.<sup>89</sup><sup>141</sup>**v0.151.0:** line-local`f` /`F` /`t` /`T` find and till motions plus`gg` /`G` buffer jumps, all usable with`c` /`d` /`y` operators and dot-repeat.<sup>145</sup>**v0.152.0:** draft-local`/` and`?` literal search with highlighted matches and`n` /`N` repeat navigation, also composable with operators; fresh drafts start in Insert mode.<sup>147</sup>**v0.153.0:** bounded undo/redo with`u` and`Ctrl+R` , restoring complete drafts including pasted content and attachments.<sup>149</sup>**v0.154.0:**`R` replace mode with undo and dot-repeat, plus recovered Escape handling in legacy terminals.<sup>161</sup> | 
| `/voice` | **Live voice conversations in the TUI, on by default since v0.156.0.** Shipped experimental and opt-in in v0.155.0 (a live conversation with a streaming transcript and microphone controls, #43581, #43651, #44331; a configurable mute shortcut, #43690; signed macOS voice resources, #43983), then promoted to a stable, default-on feature in v0.156.0 (#44921), so`/voice` no longer needs`/experimental` on a supported build. v0.156.0 also adds a configurable**F8** toggle (#46071), a`/voice settings` picker for choosing the voice used by future conversations (#44622), bundled voice runtimes for Linux and Windows (#44714, #44922), voice-session analytics attribution (#46058), reduced-motion handling (#46835), and playback preserved across pauses and packet bursts (#46880).**No off switch since v0.164.0:** the`realtime_conversation` flag is removed with the feature left on (the source calls it a “Removed compatibility flag. TUI voice availability follows platform support.”), so`realtime_conversation = false` under`[features]` no longer hides`/voice` ; the config reference still described the old switch on November 10, 2026.<sup>197</sup><sup>201</sup> Distinct from the spacebar voice transcription that shipped in v0.105.0 and was removed in v0.118.0.<sup>165</sup><sup>170</sup><sup>172</sup><sup>237</sup>**v0.161.0:** pick your microphone, speaker, and microphone input channels; the choice is saved locally (#49437, #49836).<sup>187</sup> | 
| `/hooks` | **Browse and toggle lifecycle hooks from the TUI (v0.129.0+).** Discover available hooks, see which are active, and toggle individual hooks without leaving the session.<sup>89</sup> | 
| `/mention` | Attach file to conversation | 
| `/init` | Generate AGENTS.md scaffold | 
| `/status` | Session config and token usage; v0.148.0 adds estimated thread credits/cost for eligible workspaces <sup>127</sup> | 
| `/export` | Export the complete conversation to Markdown — clipboard or a new file (v0.148.0) <sup>127</sup> | 
| `/cd` ,`/pwd` ,`/cwd` | Manage the session’s working directory from inside the TUI (v0.149.0); as of v0.151.0, `/cd` preserves restored permission profiles and is rejected when a directory change cannot represent the profile safely, so it no longer weakens sandbox restrictions<sup>129</sup><sup>144</sup> | 
| `/usage` | **Daily, weekly, and cumulative account token activity (v0.140.0+).****v0.142.0:** also shows and redeems earned usage-limit reset credits, with confirmation, retry, and refreshed availability states.<sup>107</sup><sup>109</sup>**v0.144.0:** reset credits show their type and expiration, and the redemption picker lets you choose which credit to redeem.<sup>112</sup>**v0.156.0:**`/usage` becomes an**account analytics dashboard** – stacked charts, a Summary tab, Top chats (consumer included), gated plan usage history, and plugin and skill activity (#45763, #45764, #45765, #45766, #45768, #45769, #45770, #45772), with keyboard help, mouse navigation, and preserved reading positions (#46863, #46864, #46866).<sup>170</sup> | 
| `/tui` | **Choose the terminal UI mode for your next launch (v0.156.0).** A picker between Scrollback and Fullscreen that, after confirmation, saves`tui.fullscreen_transcript` to your user config; the running session keeps its current mode until you restart, and launch overrides still apply (#46883). Fullscreen adds a transcript viewer with search and per-activity detail controls, mouse selection and copying, and right-click copy for transcript and composer selections (#46732, #46734, #46895).**Fullscreen is on by default since v0.157.0** (#47178), after shipping off by default in v0.156.0: set`fullscreen_transcript = false` under`[tui]` , or choose Scrollback here, to keep terminal scrollback;`--no-alt-screen` and`tui.alternate_screen = "never"` take precedence. v0.157.0 also adds Shift-click to extend a selection (#47414). v0.158.0 adds`tui.copy_on_select` (`auto` , the default, copies mouse selections on release except in direct terminals that forward their own copy shortcut;`always` ;`never` ) and`tui.right_click_paste` (`auto` , the default, on Windows, WSL, and Linux;`on` adds macOS;`off` ), and copied selections keep Markdown formatting (#47639, #48469, #48118, #47896). The old`features.transcript_v2` flag is deprecated and ignored (#46849).<sup>170</sup><sup>172</sup><sup>175</sup><sup>177</sup> | 
| `/daemon` | **Update the local background server from the TUI (v0.156.0)** (#45854). Launch with`--no-daemon` to bypass the shared background server entirely (#46088); automatic background-server startup, opt-in in v0.156.0 (#46117), is on by default for eligible interactive launches since v0.157.0 (#47179); set`daemon_auto_start = false` under`[features]` to turn it off.`/status` identifies local background servers (#46905).<sup>170</sup><sup>175</sup> Since v0.163.0,`/daemon` can also restart a managed background server, warning about work the restart could interrupt and reconnecting afterwards (#53173).<sup>194</sup> | 
| `/import` | **Selectively import setup, project configuration, and recent chats from Claude Code (v0.140.0+).**<sup>107</sup>**v0.145.0:** expanded to migrate from**Cursor** as well as Claude Code — settings, MCP servers, plugins, sessions, commands, and project-scoped memories.<sup>117</sup> | 
| `/delete` | **Permanently delete the current session (v0.140.0+)** — also`codex delete` on the CLI and`thread/delete` on the app-server, all with confirmation safeguards.<sup>107</sup> | 
| `/permissions` | Choose what Codex is allowed to do (permission profile and approvals); since v0.164.0 `/permissions <profile-id>` selects a built-in or custom permission profile inline (`/permissions :read-only` ;`:ask-for-approval` and`:approve-for-me` are aliases)<sup>196</sup> | 
| `/personality` | Removed. The command is not in the TUI’s command list at v0.166.0 or v0.167.1; the personality feature flag was retired in v0.156.0 (#45809), and the `personality` config key is a deprecated compatibility setting:`friendly` and`pragmatic` no longer select a style, and`none` omits the built-in Personality section from model instructions<sup>170</sup><sup>201</sup><sup>206</sup> | 
| `/mcp` | List configured MCP tools; `/mcp verbose` for diagnostics, and since v0.161.0`/mcp login <name>` signs in to a server from inside the session<sup>187</sup> | 
| `/apps` | Browse ChatGPT connectors | 
| `/ps` | Show background terminals | 
| `/skills` | Access and invoke skills | 
| `/plugins` | Browse and manage installed plugins (v0.117.0+); v0.129.0 adds workspace sharing and marketplace operations. <sup>89</sup> | 
| `/title` | Configure which items appear in the terminal title (v0.117.0+) | 
| `/warnings` | View retained warnings and diagnostic details <sup>217</sup> | 
| `/pets` | Choose or hide the terminal pet ( `/pet` also works)<sup>217</sup> | 
| `/setup-default-sandbox` | Windows only: set up the elevated sandbox <sup>217</sup> | 
| `/config` | Searchable preferences panel (v0.163.0): Appearance, Notifications, Input, Sessions, and Text selection tabs; supported changes apply at once, the panel marks the ones that need a restart, and edits persist to the selected local config file (#52990, #53429) <sup>194</sup> | 
| `/debug-config` | Show config layers and requirement sources for debugging (before v0.163.0 there was no `/config` command)<sup>138</sup><sup>141</sup><sup>145</sup> | 
| `/statusline` | Configure TUI footer; v0.129.0 adds an optional theme-aware status line with PR + branch-change summaries. <sup>89</sup> | 
| `/theme` | Preview and persist a syntax-highlighting theme (v0.105.0+). **v0.156.0:** the picker adds six bundled themes (`ada` ,`babbage` ,`curie` ,`cushman` ,`dali` ,`davinci` ) and theme-aware accents for selected controls; a custom theme file with the same name takes precedence (#46504).<sup>61</sup><sup>170</sup> | 
| `/feedback` | Send logs to Codex maintainers | 
| `/logout` | Sign out | 
| `/keymap` | Remap TUI shortcuts; v0.150.0 adds bindable shortcuts that cycle permission modes <sup>138</sup><sup>141</sup> ; v0.163.0 adds a leader key (`tui.keymap.global.leader` , default`ctrl-x` ; bindings such as`leader c` follow it, and hints stay open until you choose or cancel) and moves voice mute to`F9`<sup>194</sup> | 
| `/rename` | Rename the current thread; v0.150.0 suggests an editable title based on the conversation, and unnamed terminal tasks receive descriptive titles automatically <sup>138</sup><sup>141</sup> | 
| `/approve` | Approve one retry of a recent auto-review denial <sup>138</sup> | 
| `/memories` | Configure memory use and generation <sup>138</sup> | 
| `/agents` | View and switch between all active agent sessions; `/subagents` switches between this session’s subagents<sup>138</sup> | 
| `/side` or`/btw` | Start a side conversation in an ephemeral fork <sup>138</sup> ; since v0.163.0 it returns to an existing side conversation after you switch threads (#52184)<sup>194</sup> | 
| `/raw` | Toggle raw scrollback mode for copy-friendly terminal selection <sup>138</sup> | 
| `/ide` | Include current selection, open files, and other context from your IDE <sup>138</sup> | 
| `/stop` or`/clean` | Stop all background terminals <sup>138</sup> | 
| `/experimental` | Toggle experimental features <sup>138</sup> . It gated live voice conversations and managed worktrees in v0.155.0; since v0.156.0 both are stable and on by default and no longer appear in this menu (#44921, #44870)<sup>165</sup><sup>170</sup> . Since v0.167.0 the menu lists “System proxy,” which makes Codex requests use the macOS or Windows system proxy settings (#58157)<sup>200</sup> | 
| `/app` | Continue this session in the desktop app (macOS and Windows only) <sup>138</sup> | 
| `/daybreak` | Turn Daybreak on or off (v0.161.0). Present by default since v0.163.0, when `cli_daybreak` became stable and on, for eligible OpenAI accounts, API-key accounts included;`cli_daybreak = false` under`[features]` hides it<sup>187</sup><sup>194</sup> | 

**Workflow picker redesign (v0.129.0):** Resume and fork are now easier to reach from a redesigned picker, and a new **raw scrollback mode** lets you scroll through the unrendered transcript when you need to copy commands or model output verbatim. Useful when triaging a long debugging session or piping output to another tool.<sup>89</sup>

**Unified `@` mentions menu (v0.140.0):** Typing `@` in the composer now opens a single mentions menu spanning files, plugins, and skills by default, replacing the file-only attach flow — one keystroke to reference any project resource.<sup>107</sup>


### 2. Desktop App (macOS, Windows, Linux preview)

```
codex app                    # Launch desktop app (auto-installs if missing; macOS and Windows)
```
OpenAI now calls it the ChatGPT desktop app: inside it you choose ChatGPT (Chat or Work) or Codex. It runs on macOS and Windows, with a Linux preview.<sup>216</sup> The desktop app adds capabilities the CLI doesn’t have:

- **Multi-tasking** : Run multiple parallel agents across different projects simultaneously
- **Git worktree isolation** : Each thread works on an isolated copy of your repo
- **Inline diff review** : Stage, revert, and commit changes without leaving the app
- **Integrated terminal** : Per-thread terminal for running commands
- **Conversation forking** : Branch conversations to explore alternatives
- **Floating pop-out windows** : Detach conversations into portable windows
- **Scheduled tasks** (formerly Automations): Schedule recurring tasks (issue triage, CI monitoring, alert response)
- **Appshots** : Attach the frontmost app window (macOS and Windows) to a thread with a screenshot plus available text
- **Built-in browser comments** : Preview local or public pages, leave element/area comments, and let Codex address precise visual feedback
- **Computer Use** : Let Codex operate allowed apps on macOS and Windows for scoped GUI work; locked use is macOS-only and opt-in for eligible remote Computer Use turns

**ChatGPT Voice, powered by GPT-Live (desktop 26.715, July 23, 2026)**: The desktop app can now be driven by speech: talk through work and coordinate tasks across Chat, Work, and **Codex** without typing. On macOS, turning on **Screen context** shares an appshot of your frontmost window with the conversation, so you can ask about what you’re looking at instead of describing it. Available on Plus, Pro, Business, Edu, and Enterprise, on the desktop app and through Remote on iOS. This is desktop-surface voice. The CLI has its own, separate voice feature: `/voice` live conversations in the TUI (on by default since v0.156.0).<sup>118</sup><sup>170</sup>

**Shared thread snapshots (August 20, 2026)**: On all Codex plans, the macOS desktop app can share a read-only snapshot of a local Codex thread. The snapshot does not update when the original thread changes; personal-account links open for anyone with the link, workspace-account links only for members of the originating workspace. Codex redacts known secret patterns before sharing, but OpenAI tells you to review the shared content because sensitive content may remain. View or revoke links in ChatGPT data controls under Shared links.<sup>137</sup>

**When to use the app vs CLI**: Use the desktop app when you’re coordinating multiple workstreams or need visual diff review. Use the CLI when you want terminal composability, scripting, or CI/CD integration.


### 3. IDE Extension (VS Code, Cursor, Windsurf)

The Codex IDE extension integrates directly into your editor:

- **Agent with permission modes** : Reads files, makes edits, runs commands; the permissions control beneath the composer can offer**Ask for approval** ,**Approve for me** , and**Full access**<sup>255</sup>
- **Inline edits** : Context-aware suggestions in your active files
- **Shared sessions** : Sessions sync between CLI and IDE extension
- **Same authentication** : Sign in with ChatGPT account or API key

Install from the VS Code Marketplace (VS Code and VS Code Insiders) or the Cursor and Windsurf extension stores. Xcode and JetBrains IDEs provide their own Codex integrations.<sup>3</sup>

### 4. Codex Cloud

Cloud tasks run asynchronously in OpenAI-managed environments (the `codex cloud` CLI command that reaches them is marked experimental):

- **Fire and forget** : Queue tasks that run independently of your local machine
- **Parallel execution** : Run multiple cloud tasks simultaneously
- **PR creation** : Codex creates pull requests from completed work
- **Local apply** : Pull cloud results into your local repo with`codex apply <TASK_ID>`

```
codex cloud list             # List recent cloud tasks
codex apply <TASK_ID>        # Apply diff from a specific cloud task
```
Cloud tasks are also accessible from chatgpt.com/codex.<sup>4</sup>

### 5. Browser Extension (Chrome, Edge, Brave, Opera, Vivaldi)

Codex ships as a browser extension for Google Chrome, Microsoft Edge, Brave, Opera, and Vivaldi, adding a fifth surface alongside the CLI, desktop app, IDE extension, and cloud. The extension launched for Chrome in May 2026; the August 25, 2026 changelog added the other four browsers, set up from **Settings > Computer Use** in the ChatGPT desktop app. All five support tab mentions and browser control; Opera does not support side chat, so start its tasks from the desktop app. The extension is designed to ride alongside your normal browsing rather than take it over: Codex works in parallel across tabs in the background, and you stay in control of which sites it touches.<sup>90</sup><sup>136</sup>

- **Parallel tab execution** : Codex operates across multiple tabs at once without locking the foreground tab.
- **Per-site control** : You allow-list which websites Codex can interact with; default is no access.
- **Browser as the workbench** : The extension is best for app-and-website work where the page is the source of truth — admin consoles, internal dashboards, content-management UIs, ticket systems — not a replacement for the CLI on local repos.
- **Same brain** : the browser extension runs the same GPT-6 models the other surfaces use, so an AGENTS.md or skills configuration that works in the CLI carries the same conventions into browser-driven work.

Install from the browser extension docs, the page formerly titled Codex for Chrome.<sup>136</sup>

## Configuration System Deep Dive

Codex uses TOML for configuration. Understanding the precedence hierarchy is critical because it determines which settings win when they conflict.

### Precedence (Highest to Lowest)

1. **Session overrides** (highest): CLI flags (`--model` ,`--sandbox` ,`--ask-for-approval` ,`--search` ,`--enable/--disable` ) and`-c key=value` overrides
2. **Project config** (`.codex/config.toml` files from the project root down to the current directory; the closest one wins; trusted projects only)
3. **Profile file** (`~/.codex/<name>.config.toml` , when you select it with`--profile <name>` )
4. **User config** (`$CODEX_HOME/config.toml` , defaults to`~/.codex/config.toml` )
5. **Cloud-managed defaults** (a`config.toml` delivered for the signed-in workspace, when there is one)
6. **System config** (`/etc/codex/config.toml` on Unix)
7. **Built-in defaults** (lowest)

A profile file therefore overrides your user config but not a trusted project’s `.codex/config.toml`, and cloud-managed defaults sit below anything you set yourself.<sup>207</sup>

`requirements.toml` acts as a **policy constraint layer** that restricts what values users can select after normal config merging. See Enterprise Deployment.


### Config File Locations

| Scope | Path | Purpose | 
|---|---|---|
| User | `~/.codex/config.toml` | Personal defaults | 
| Project | `.codex/config.toml` | Per-repo overrides | 
| Profile | `~/.codex/<name>.config.toml` | Per-mode overlay, selected with `--profile <name>` | 
| System | `/etc/codex/config.toml` | Machine-wide defaults | 
| Managed | `/etc/codex/requirements.toml` | Admin-enforced policy constraints | 

**Expert tip**: The `CODEX_HOME` environment variable overrides the default `~/.codex` directory. Useful for CI/CD or multi-account setups.

**Desktop multi-folder projects (26.715, July 23, 2026)**: A local project in the ChatGPT desktop app can now span several related folders, with one of them designated the **primary folder** (project menu → **Edit project**). Automatic discovery of `config.toml` runs against the primary folder only; secondary folders stay available for file search, reading, and editing but contribute no configuration. A `.codex/config.toml` sitting in a secondary folder is a file Codex can read, not a config it loads. This is desktop-app project scoping, not a change to the precedence rules above — the CLI still resolves project config by walking upward from the current working directory.<sup>118</sup>


### Complete Configuration Reference

Top-level keys must come before the first `[table]` header: in TOML, every key after a header belongs to that table, and Codex loads a misplaced key without an error and without applying it (pass `--strict-config` to make unrecognized fields an error). The block below is ordered that way, top-level keys first, then tables.<sup>213</sup>

```
# ~/.codex/config.toml: annotated reference
# Part 1: top-level keys. Keep these above every [table] header.
# ─── Model Selection ───────────────────────────────────
model = "gpt-6.1-sol"                  # Docs' example; unset = bundled default (gpt-6.1-sol since v0.159.1)
model_provider = "openai"               # Built-in ids: openai, ollama, lmstudio, amazon-bedrock, amazon-bedrock-runtime, gcp; or a custom provider id
model_context_window = 272000           # Token count available to active model (override)
model_auto_compact_token_limit = 200000 # Threshold triggering automatic history compaction
model_reasoning_effort = "medium"       # low|medium|high|xhigh|max|ultra (model-dependent; GPT-6.1 Sol's catalog default is low)
model_reasoning_summary = "auto"        # auto|concise|detailed|none
model_verbosity = "medium"              # low|medium|high
# personality = "none"                  # Deprecated key: friendly/pragmatic no longer select a style; "none" omits the built-in Personality section
review_model = "gpt-6.1-sol"           # Optional model for /review command
service_tier = "fast"                  # default|priority|flex; the legacy value "fast" selects the Fast tier (2.5x included usage)
oss_provider = "lmstudio"              # lmstudio|ollama (used with --oss)
# ─── Sandbox & Approval ───────────────────────────────
sandbox_mode = "workspace-write"        # read-only|workspace-write|danger-full-access
approval_policy = "on-request"          # on-request|never|{ granular = { ... } } (untrusted retired in v0.149.0)
# ─── Web Search ────────────────────────────────────────
web_search = "live"                     # disabled|cached|indexed|live (constrained by allowed modes)
# ─── Tools ─────────────────────────────────────────────
tools.update_plan.enabled = false       # Planning tool opt-in (v0.152.0); exposes update_plan when true
# ─── Instructions ──────────────────────────────────────
developer_instructions = ""             # Additional injected instructions
# model_instructions_file = "/path/to/instructions.md"   # Replaces the built-in model instructions (the schema discourages it)
compact_prompt = ""                     # Custom history compaction prompt (blank = built-in)
# ─── Shell ─────────────────────────────────────────────
allow_login_shell = false               # Default true; false means shell tools never use a login shell
# ─── Authentication ────────────────────────────────────
cli_auth_credentials_store = "file"     # file|keyring|auto|ephemeral
forced_login_method = "chatgpt"         # chatgpt|api
# mcp_oauth_callback_port = 5555        # Fixed port for the MCP OAuth callback; unset = a port chosen by the OS
mcp_oauth_credentials_store = "auto"   # auto|file|keyring
# ─── Storage ───────────────────────────────────────────
tool_output_token_limit = 10000         # Token budget per tool output stored in history (global; per-tool MCP overrides since v0.152.0)
# log_dir = "/path/to/codex-logs"       # Custom log directory (default: $CODEX_HOME/log)
# sqlite_home = "/path/to/codex-state"  # Where the SQLite state database lives (default: $CODEX_HOME)
# ─── UI & Display ──────────────────────────────────────
file_opener = "vscode"                  # vscode|vscode-insiders|windsurf|cursor|none
hide_agent_reasoning = false
show_raw_agent_reasoning = false
check_for_update_on_startup = true
# ─── Project Instructions ──────────────────────────────
project_doc_max_bytes = 32768           # Max combined project instruction bytes (32 KiB)
project_doc_fallback_filenames = []     # Alternative instruction filenames
project_root_markers = [".git"]         # Project root detection
# ─── Notifications ────────────────────────────────────
notify = ["terminal-notifier", "-title", "Codex"]  # Command for notifications
# Part 2: tables. Every key below a [table] header belongs to that table.
# ─── Sandbox Detail ────────────────────────────────────
[sandbox_workspace_write]               # These four keys are the whole table
writable_roots = []                     # Additional writable paths
network_access = false                  # Allow outbound network
exclude_tmpdir_env_var = false          # true removes $TMPDIR from the writable roots
exclude_slash_tmp = false               # true removes /tmp from the writable roots
# ─── Shell Environment ─────────────────────────────────
[shell_environment_policy]
inherit = "all"                         # all|core|none
ignore_default_excludes = false         # false strips KEY/SECRET/TOKEN vars; the default is true, which keeps them
exclude = []                            # Glob patterns to exclude
set = {}                                # Explicit overrides
include_only = []                       # Whitelist patterns
# ─── History ───────────────────────────────────────────
[history]
persistence = "save-all"                # save-all|none
# max_bytes = 10485760                  # Optional cap on history.jsonl; the oldest entries are dropped past it
# ─── Terminal UI ───────────────────────────────────────
[tui]
notifications = false                   # Enable notifications
notification_method = "auto"            # auto|osc9|bel
animations = true
show_tooltips = true
alternate_screen = "auto"               # auto|always|never
status_line = ["model", "context-remaining", "git-branch"]
mouse_scroll_speed = 1.0                # Mouse-wheel scroll multiplier, a finite positive number (v0.162.0)
# ─── Feature Flags ─────────────────────────────────────
# Use `codex features list` for current names/stages/defaults.
[features]
shell_tool = true                       # Shell command execution (stable)
unified_exec = true                     # PTY-backed exec (stable)
shell_snapshot = true                   # Shell env snapshots (stable)
enable_request_compression = true       # zstd request compression where supported (stable)
fast_mode = true                        # Service-tier selection and Fast-tier commands (stable)
instant_interrupt = true                # New input preempts the model's response; on by default since v0.162.0 (under development)
api_key_model_discovery = true          # Fetch the model catalog when signed in with an OpenAI API key; on by default since v0.161.0 (stable)
goals = true                            # Goal mode; stable and on by default in v0.133.0+
hooks = true                            # Lifecycle hooks (stable)
multi_agent = true                      # Enable multi-agent collaboration tools (stable)
plugins = true                          # Plugin system (stable)
plugin_sharing = true                   # Workspace plugin sharing (stable)
browser_use = true                      # In-app browser automation (stable)
browser_use_external = true             # Browser-extension browser use (stable)
computer_use = true                     # Computer Use, macOS and Windows (stable, plan/region gated)
in_app_browser = true                   # Shared rendered-page preview (stable)
image_generation = true                 # Image-generation tool (stable)
guardian_approval = true                # Auto-review approval path (stable)
skill_mcp_dependency_install = true     # Prompt/install missing skill MCP deps (stable)
tool_suggest = true                     # Tool/plugin suggestion surface (stable)
workspace_dependencies = true           # Workspace dependency discovery (stable)
memories = true                         # Local memories (stable flag, off by default; this line opts in)
network_proxy = false                   # Sandboxed-command network proxy (experimental); boolean or table, see below
prevent_idle_sleep = true               # Keep machine awake during active turns (experimental; off by default, this line opts in)
guardian_conversation_history_tools = false  # Guardian reads earlier user messages (under development, v0.160.0)
# Removed or deprecated feature names still appear in `codex features list`
# for migration diagnostics. Do not set removed flags such as
# `collaboration_modes`, `request_rule`, `codex_git_commit`,
# `apply_patch_freeform`, `search_tool`, or `js_repl` in new configs.
# Also removed, and skipped when the config loads (v0.167.1): `personality`,
# `plugin_hooks`, `terminal_resize_reflow`, `worktrees`, `realtime_conversation`.
# ─── Guardian Auto-Review (v0.160.0 history keys) ─────
[auto_review]
conversation_history_max_output_tokens = 4000   # Per history-tool response, estimated tokens (default 4,000)
experimental_conversation_history_prompt = ""   # Blank = built-in retrieval instructions
# ─── Multi-Agent Roles (v0.102.0+) ───────────────────
[agents]
max_concurrent_threads_per_session = 4  # Spawned-agent threads open at once, excluding the primary thread (legacy alias: max_threads)
[agents.explorer]
description = "Read-only codebase navigator"
config_file = "~/.codex/profiles/explorer.toml"
# ─── Per-Project Overrides ────────────────────────────
[projects."/absolute/path/to/repo"]
trust_level = "trusted"                 # Per-project trust override
```
**`features.network_proxy` is `boolean | table`.** The boolean form only starts or stops the proxy; the table form carries the policy. The proxy is what enforces permission-profile domain rules for sandboxed commands: with it off, `[permissions.<name>.network]` domain rules are not enforced and command network access is direct. It does not filter web search, apps, MCP servers, or other hosted tools.<sup>139</sup>

```
[features.network_proxy]
enabled = true
allow_local_binding = false
proxy_url = "http://127.0.0.1:3128"
[features.network_proxy.domains]
"api.github.com" = "allow"
"*.npmjs.org" = "allow"          # subdomains only
"**.example.com" = "deny"        # apex plus subdomains; deny wins on conflicts
```
| Key | Type | Default | Meaning | 
|---|---|---|---|
| `enabled` | boolean | `false` | Start the sandboxed-command network proxy when command network access is enabled | 
| `domains` | map of host to `allow` or`deny` | unset (no external destinations allowed) | Exact hosts, `*.example.com` (subdomains only),`**.example.com` (apex plus subdomains), or a global`*` allow rule;`deny` wins | 
| `unix_sockets` | map of path to `allow` or`deny` | unset | Unix socket policy; add `allow` entries for permitted sockets | 
| `allow_local_binding` | boolean | `false` | Broader local and private-network access; exact local IP or `localhost` allow rules still work without it | 
| `enable_socks5` | boolean | `true` | Expose SOCKS5 support | 
| `enable_socks5_udp` | boolean | `true` | Allow UDP over SOCKS5 | 
| `allow_upstream_proxy` | boolean | `true` | Chain through an upstream proxy from the environment | 
| `dangerously_allow_non_loopback_proxy` | boolean | `false` | Permit non-loopback listener addresses (exposes the proxy beyond localhost) | 
| `dangerously_allow_all_unix_sockets` | boolean | `false` | Permit arbitrary Unix socket destinations instead of allowlist-only access | 
| `proxy_url` | string | `"http://127.0.0.1:3128"` | HTTP listener URL | 
| `socks_url` | string | `"http://127.0.0.1:8081"` | SOCKS5 listener URL | 

Administrator-managed network requirements in `requirements.toml` can start the proxy without `features.network_proxy`. The table is `[network_proxy]` as of v0.166.0, with `[experimental_network]` still accepted as an alias; the docs say to use one name per requirements layer and to keep the alias in published policies until every client and policy validator supports the new name.<sup>139</sup>

### Profiles

Named configuration presets for different work modes. **Since Codex 0.134.0 each profile is its own file** (`~/.codex/<name>.config.toml`, holding top-level keys only), and `--profile <name>` overlays that file on `~/.codex/config.toml`. The older `[profiles.<name>]` tables inside `config.toml` are no longer read, and a leftover one makes `--profile <name>` refuse to start until you move its keys into the profile file. A leftover top-level `profile = "<name>"` selector is rejected outright: Codex stops with the message “legacy `profile = "<name>"` config is no longer supported; use `--profile <name>` with `<name>.config.toml` instead” until you delete the key.<sup>131</sup><sup>238</sup>

```
# ~/.codex/fast.config.toml
model = "gpt-6-luna"
# GPT-6 Sol/Luna start on the Fast tier (2.5x included usage) unless service_tier is set; /fast toggles it
model_reasoning_effort = "low"
approval_policy = "on-request"
sandbox_mode = "workspace-write"
```
```
# ~/.codex/careful.config.toml
model = "gpt-6-astra"
model_reasoning_effort = "xhigh"
approval_policy = "on-request"   # `untrusted` retired in v0.149.0
sandbox_mode = "read-only"
```
```
# ~/.codex/auto.config.toml
model = "gpt-6.1-sol"
# GPT-6.1 Sol starts at Standard speed; /fast toggles Fast (2.5x included usage)
model_reasoning_effort = "medium"
approval_policy = "never"
sandbox_mode = "workspace-write"
```
A profile file is a layer above your base user config and below project and CLI config, so it only needs the values that differ from your base.<sup>131</sup>

Activate a profile:

```
codex --profile fast "quick refactor"
codex --profile careful "security audit"
codex -p auto "fix CI"
```
**Expert tip**: There is no default-profile selector: since 0.134.0 a top-level `profile = "<name>"` key is an error that stops Codex from starting, so delete it. Keep your everyday settings in `~/.codex/config.toml` and use profile files for the exceptions, selected per session with `--profile fast`.<sup>131</sup><sup>238</sup>


### Custom Model Providers

Connect to Azure, AWS Bedrock, local models, or proxy services:

```
[model_providers.azure]
name = "Azure OpenAI"
base_url = "https://YOUR_PROJECT.openai.azure.com/openai"
wire_api = "responses"
query_params = { api-version = "2025-04-01-preview" }
env_key = "AZURE_OPENAI_API_KEY"
# Built-in amazon-bedrock provider (v0.123.0+, first-class in v0.124.0+)
# AWS SigV4 signing + credential-based auth; AWS profile selectable via the
# nested `aws.profile` field (NOT a top-level `aws_profile` key).
# v0.130.0+ also accepts credentials from `aws login` (the AWS console-login
# flow) — Codex resolves the cached console-login session for the chosen
# profile if static keys are absent.
# v0.140.0+ adds managed Amazon Bedrock API-key authentication, and stores
# CLI and MCP OAuth credentials in encrypted local storage.
# Select it with a top-level `model_provider = "amazon-bedrock"`. On this
# built-in provider you can set only `base_url`, `auth`, `http_headers`, and
# the `aws` fields (`profile`, `region`, `credential_export`, `auth_refresh`);
# any other key, a `name` included, stops config.toml from loading.
[model_providers.amazon-bedrock.aws]
profile = "default"                   # any profile from ~/.aws/credentials
# Region/credential resolution otherwise follows the standard AWS chain;
# v0.130.0 added support for `aws login` console-login profiles in addition
# to static access keys and IAM role assumption.
[model_providers.local_ollama]        # pick your own ID: `ollama` and `lmstudio` are reserved built-ins
name = "Ollama (custom endpoint)"
base_url = "http://localhost:11434/v1"
# wire_api defaults to "responses", the only supported value
```
On the built-in `amazon-bedrock` provider only `base_url`, `auth`, `http_headers` and the `aws` fields can be set; any other key, a `name` included, stops `config.toml` from loading.<sup>253</sup> Managed Bedrock API-key authentication and encrypted credential storage arrived in v0.140.0,<sup>107</sup> and `aws login` console-login profiles in v0.130.0.<sup>91</sup>

`env_key` names the environment variable that holds the provider’s API key; the key itself never goes in `config.toml`. A provider with `requires_openai_auth = true` shows the login screen on first run and stores its credentials in the backend chosen by `cli_auth_credentials_store` (`file`, `keyring`, `auto`, or `ephemeral`), not necessarily in `auth.json`. Since v0.160.0, a provider that sets `model_catalog_url` gets only the models in that catalog, with no bundled models merged in, and a failed catalog refresh discards cached entries rather than reusing stale ones.<sup>183</sup> Since v0.162.0 a custom Responses-compatible provider can also declare what it supports under `[model_providers.<name>.capabilities]`: `external_web_access = false` keeps hosted web search off the live web for that provider, and `remote_compaction = "v2"` opts it into the Responses `compaction_trigger` protocol, which otherwise only OpenAI and Azure Responses providers get (`"unsupported"` keeps compaction local).<sup>188</sup>

**Google Cloud Vertex AI (v0.165.0+).** The built-in `gcp` provider sends requests to Vertex AI with Google Application Default Credentials: set `model_provider = "gcp"` and, when the environment does not already carry them, the project and location (the provider’s fields take precedence over `GOOGLE_CLOUD_PROJECT` and `GOOGLE_CLOUD_LOCATION`; the location defaults to `global`). `credential_file` points the auth command at a specific ADC file instead of `GOOGLE_APPLICATION_CREDENTIALS`. GPT-6.1 Terra is in the Vertex AI catalog since v0.166.0. Since v0.167.0 the sign-in screen can run a setup wizard for this provider (Application Default Credentials or a credential file) when you set `gcp_setup_wizard = true` under `[features]`; the flag ships off.<sup>198</sup><sup>199</sup><sup>200</sup><sup>201</sup>

```
model_provider = "gcp"
[model_providers.gcp]
project_id = "my-project"              # or GOOGLE_CLOUD_PROJECT
location = "global"                    # or GOOGLE_CLOUD_LOCATION; global when neither is set
# credential_file = "/path/to/adc.json"  # overrides GOOGLE_APPLICATION_CREDENTIALS for the auth command
```
**Warning:** The `chat/completions` wire API is gone from Codex. OpenAI announced the removal for February 2026,<sup>34</sup> and current releases refuse to load a provider that sets `wire_api = "chat"`, with an error that says it “is no longer supported” and tells you to set `wire_api = "responses"`. `responses` is the only accepted value and the default when the key is omitted, so a custom provider has to expose a Responses-compatible endpoint. For Ollama and LM Studio, use the built-in local providers through `--oss` (below) instead of a Chat Completions URL.<sup>202</sup><sup>207</sup>


Use local models with the `--oss` flag:

```
codex --oss "explain this function"               # Uses default OSS provider
codex --oss --local-provider lmstudio "explain"   # Explicit LM Studio
codex --oss --local-provider ollama "explain"      # Explicit Ollama
```
Or set the default local provider for `--oss` in config. With neither `--local-provider` nor `oss_provider` set, the interactive CLI asks which one to use and `codex exec` exits with an error.<sup>207</sup>

```
oss_provider = "lmstudio"   # or "ollama"
```
### Inline Config Overrides

Override any config value from the command line:

```
codex -c model="gpt-6.1-sol" "refactor the API"
codex -c 'sandbox_workspace_write.network_access=true' "install dependencies"
codex -c model_reasoning_effort="xhigh" "debug the race condition"
```
## Which Model Should I Choose?

### Available Models (November 2026)

| Model | Input / Total Context | Default Reasoning | Best For | 
|---|---|---|---|
| **gpt-6.1-sol** | 1.05M (API); 500K default in Codex per the models docs; 272K in the catalog bundled with the CLI | `low` (catalog); the docs say to start with your client’s default | **Complex coding and repeated, long-running work; the bundled default since v0.159.1** (docs changelog September 29, 2026). The models docs: “Near-Astra performance for complex work at a lower cost than Astra,” suggested “for repeated, long-running work across code, apps, and documents”; the API page: “Use GPT-6.1 Sol for complex coding, computer use, and professional work.” First in the CLI picker and the Amazon Bedrock default since v0.159.1. Codex credits: 50 / 2.5 cached / 250 output per 1M tokens; API $2/$10 per MTok ($0.10 cached input), 128K max output, knowledge cutoff April 30, 2026. Reasoning Low through Ultra. The catalog sets no default service tier, so sessions start at Standard; Fast draws included limits at 2.5x and purchased credits at 2x. Ultrafast (Pro $500 and eligible Enterprise and Edu plans) draws included limits at 8x and purchased credits at 6x; on the API it is the`ultrafast` service tier at $12/$60 per MTok, and the catalog bundled with v0.167.1 advertises only Fast. Subscription usage carries no long-context surcharge. Available on Plus, Pro, Business, Enterprise, and Edu, and off in Enterprise and Edu until an administrator enables it; the Free and Go plan cards list only GPT-6 Luna<sup>179</sup><sup>202</sup><sup>203</sup><sup>204</sup><sup>205</sup><sup>233</sup><sup>234</sup> | 
| **gpt-6.1-terra** | 1.05M (API); 500K in the Codex catalog (872K maximum) | `low` (catalog);`medium` on the API, where`none` through`max` are accepted | **Everyday writing and visual work** (in ChatGPT Work and Codex since October 26, 2026; bundled catalog entry since v0.165.0, Vertex AI and Bedrock catalogs since v0.166.0) – the Codex models docs: “a fast, efficient choice for writing, visual understanding,” and clear, direct text; the API page: “A fast, efficient model for visual reasoning, precise computer use, and clear, natural writing,” with image input. Not available in Chat; no UAE inference residency; Enterprise access depends on workspace settings. Codex credits: 25 / 1.25 cached / 125 output per 1M tokens; API $1/$5 per MTok ($0.05 cached input), 128K max output, knowledge cutoff June 1, 2026; prompts over 272K input tokens pay 2x input and 1.5x output. The docs set a floor for selecting it by name: “GPT-6.1 Terra requires Codex CLI 0.159.0 or later. To use it, run`codex -m gpt-6.1-terra` .”<sup>198</sup><sup>202</sup> | 
| **gpt-6-astra** | 922K / 1.05M (API); 272K in the Codex catalog | `low` (the docs’ “Light” starting point and the bundled catalog agree) | **Flagship for the hardest end-to-end work** (launched September 3, 2026) – the models docs: “Choose Astra for complete workflows across code, apps, and research that need sustained reasoning and judgment.” The bundled default when no model is configured from v0.153.4 until v0.159.1, which made GPT-6.1 Sol the default and moved Astra to second in the picker. API: $10/$50 per MTok at standard short-context rates ($20/$75 above 272K input tokens, applied to the full request), 128K max output, knowledge cutoff April 30, 2026. Codex credits: 250 / 25 cached / 1,250 output per 1M tokens, the most expensive current model. Available options vary by plan, client, workspace settings, and rollout; the models page no longer has a Codex cloud availability line (on September 23, 2026 it marked Astra unavailable there). Reasoning ladder: Low, Medium, High, Extra High, Max, Ultra<sup>154</sup><sup>157</sup><sup>179</sup><sup>203</sup><sup>205</sup><sup>231</sup><sup>233</sup><sup>234</sup> | 
| **gpt-6-sol** | 922K / 1.05M (API); 272K in the Codex catalog | `medium` (catalog and API default) | **Previous-generation Sol** (docs changelog September 22, 2026; in the CLI picker since v0.156.1, labeled “Previous generation workhorse model” since v0.159.1) – the API page: “GPT-6 Sol is built for complex coding and agentic workflows”; the Codex models page no longer has a card for it. The docs’ configuration example now uses`gpt-6.1-sol` ; the CLI’s migration prompts still move saved GPT-5.6 Sol, GPT-5.6 Terra, GPT-5.5, and GPT-5.4 selections here. Codex credits: 50 / 5 cached / 250 output per 1M tokens (half of GPT-5.6 Sol’s promotional rate); API $2/$10 per MTok, knowledge cutoff April 20, 2026. Reasoning up to Ultra in the Codex catalog. The catalog defaults its service tier to Fast in the TUI (included limits at 2.5x); toggle it with`/fast` . The Amazon Bedrock default from v0.157.0 until v0.159.1. Not the same model as GPT-5.6 Sol<sup>172</sup><sup>175</sup><sup>179</sup><sup>203</sup><sup>205</sup><sup>231</sup><sup>233</sup><sup>234</sup> | 
| **gpt-6-luna** | 922K / 1.05M (API); 272K in the Codex catalog | `medium` (catalog); the docs suggest starting at High | **Focused, high-volume tasks** (in the CLI picker since v0.156.1) – “Our most efficient model for focused, high-volume tasks, including summarization, extraction, and focused coding.” The rate-limit switch prompt suggests it. Codex credits: 2.5 / 0.25 cached / 12.5 output per 1M tokens; API $0.10/$0.50 per MTok, knowledge cutoff May 18, 2026. Reasoning up to Max, not Ultra. Free and Go plans get it at Standard speed in the desktop app, subject to rollout; in Enterprise and Edu an administrator must enable it. Catalog default service tier is Fast in the TUI, as with Sol<sup>172</sup><sup>203</sup><sup>205</sup><sup>229</sup><sup>234</sup> | 
| **gpt-5.5** (API-key sessions and the API only since October 14, 2026) | 272K in the catalog bundled with the CLI; 1.05M on the API | `medium` | Previous-generation flagship (April 23, 2026) – 82.7% Terminal-Bench 2.0 at launch; labeled “Legacy coding model” in the picker. **Retired from Codex for ChatGPT sign-in on October 14, 2026** (19:00 UTC); the OpenAI API and Codex sessions authenticated with an API key are unaffected. The docs name GPT-6.1 Sol (`gpt-6.1-sol` ) as the replacement on Plus, Pro, Business, Enterprise, and Edu, and GPT-6 Luna in the desktop app on Free and Go. The April launch page gave 400K for Codex; the bundled catalog entry reads 272,000.<sup>83</sup><sup>202</sup><sup>205</sup> | 
| **gpt-5.5-pro** | 1M / 1M | `high` | Highest-effort tier on GPT-5.5 (April 24, 2026, API-available) <sup>83</sup> | 
| **gpt-5.4** | 1M / 1M | `medium` | Prior flagship; bundled selections migrated to GPT-5.6 Terra/Luna in v0.145.0; **retired from Codex for ChatGPT sign-in on August 31, 2026** ; since v0.156.1 the migration prompt points to GPT-6 Sol; removed from the bundled and Bedrock catalogs in v0.158.0, which keeps that prompt for saved selections<sup>117</sup><sup>172</sup><sup>177</sup><sup>205</sup><sup>229</sup> | 
| **gpt-5.4-mini** | 400K / 400K | `medium` | Subagent work, simpler tasks – superseded as the bundled pick by the GPT-5.6 variants in v0.145.0; **retired from Codex for ChatGPT sign-in on August 31, 2026** ; since v0.156.1 the migration prompt points to GPT-6 Luna<sup>76</sup><sup>117</sup><sup>172</sup><sup>205</sup><sup>229</sup> | 
| **gpt-5.6 Sol / Terra / Luna** | 272K / 272K | `low` (Sol),`medium` (Terra, Luna) in the catalog;`max` supported | **Previous generation, still in the bundled catalog and on the credit rate card.** Since v0.156.1 the picker labels them “Older” models and offers a migration prompt (Sol and Terra to GPT-6 Sol, Luna to GPT-6 Luna); GPT-5.6 Sol was the Amazon Bedrock default until v0.157.0 moved that to GPT-6 Sol (GPT-6.1 Sol since v0.159.1). Useful as a pin if your account does not have the GPT-6 models. Bundled CLI default family from v0.145.0 until v0.153.4; first shipped via Bedrock in v0.143.0; context corrected to 272,000 tokens in v0.144.6<sup>111</sup><sup>116</sup><sup>117</sup><sup>157</sup><sup>172</sup><sup>175</sup><sup>179</sup><sup>203</sup><sup>205</sup> | 
| **gpt-5.3-codex** | 272K / 400K | `medium` | Legacy coding specialist: deprecated in Codex when signed in with ChatGPT; still available via the API until its April 1, 2027 shutdown (recommended replacement: `gpt-6-sol` )<sup>227</sup><sup>233</sup> | 
| **gpt-5.3-codex-spark** | 128K / 128K | n/a | **Retired September 14, 2026.** No longer available in the ChatGPT desktop app, Codex CLI, or IDE extension, and absent from the bundled catalog; replace it in saved configurations, custom agents, and scripts. For low latency, use Fast mode on a current model. It was a text-only research preview for Pro users (Cerebras partnership)<sup>67</sup><sup>202</sup><sup>205</sup> | 
| **gpt-5.2-codex** | 272K / 400K | `medium` | Shut down July 23, 2026 per OpenAI’s deprecation table; recommended substitute is now `gpt-5.6-sol`<sup>88</sup> | 
| **gpt-5.1-codex-mini** | 272K / 400K | `medium` | Shut down July 23, 2026 per OpenAI’s deprecation table; recommended substitute is now `gpt-5.6-terra`<sup>88</sup> | 

**GPT-5.5 (in Codex from April 23 to October 14, 2026)** launched as OpenAI’s recommended choice for most Codex tasks and reached the API on April 24 at $5 input / $30 output per MTok. The launch page gave it a 400K context window in Codex and 1M in the API; the catalog bundled with the CLI now lists 272,000 tokens for it. Launch benchmarks: 82.7% Terminal-Bench 2.0, 84.9% GDPval, 78.7% OSWorld-Verified, and 98.0% Tau2-bench Telecom.<sup>83</sup><sup>205</sup> It retired from Codex for users signed in with ChatGPT on October 14, 2026, at 19:00 UTC; the OpenAI API and Codex sessions authenticated with an API key are not affected. For ChatGPT sign-in the docs say to choose GPT-6.1 Sol (`gpt-6.1-sol`) on Plus, Pro, Business, Enterprise, and Edu, and GPT-6 Luna (`gpt-6-luna`) in the desktop app on Free and Go, each “when available,” and to replace `gpt-5.5` in workspace defaults, saved model settings, managed configurations, custom agents, scheduled tasks, and scripts.<sup>202</sup>

**GPT-5.4 and GPT-5.4 mini retired from Codex with ChatGPT sign-in on August 31, 2026.**<sup>229</sup> The models docs name `gpt-6-sol` and `gpt-6-luna` as their replacements (in Enterprise and Edu, an administrator must enable Luna first); API-key use is unaffected.<sup>64</sup><sup>229</sup> The exact model list varies by account and rollout. Check your local cache: `~/.codex/models_cache.json`.

**Deprecation note (March 11, 2026)**: GPT-5.1 models are no longer available in ChatGPT. Existing conversations automatically continue on GPT-5.3 Instant, GPT-5.4 Thinking, or GPT-5.4 Pro. GPT-5.1-Codex-Mini remained available via API and CLI until the July 23, 2026 shutdown.<sup>71</sup><sup>88</sup>

**Free-tier note (May 5, 2026)**: GPT-5.5 Instant rolled out to the ChatGPT free tier on May 5, 2026. This widens the audience for the GPT-5.5 family beyond paid plans, though Codex CLI access requires Plus, Pro, Business, Enterprise or Edu, or an API key; Free and Go are limited to the desktop app.<sup>92</sup><sup>203</sup>

**GPT-5.4 mini (March 17, 2026; retired from Codex with ChatGPT sign-in on August 31, 2026)**: a smaller, faster variant of GPT-5.4 with 400K context at $0.75/$4.50 per MTok that used 30% of GPT-5.4 quota. It was the first model pitched for subagent delegation: a larger model plans and coordinates while smaller subagents handle narrower subtasks (codebase search, file review, document processing) in parallel.<sup>76</sup> The docs name `gpt-6-sol` as the replacement for `gpt-5.4` and `gpt-6-luna` for `gpt-5.4-mini`, so the same split now runs on the GPT-6 family.<sup>229</sup>

**Bedrock GPT-5.6 (Sol, Terra, Luna) (v0.143.0, July 8, 2026)**: Codex added first-class support for three GPT-5.6 model variants hosted on Amazon Bedrock, including support for the `max` reasoning-effort level. For teams whose model access runs through a Bedrock provider, these are selectable the same way as other custom-provider models (see Custom Model Providers); the `max` effort tier targets the hardest reasoning and agentic workloads.<sup>111</sup> **v0.144.6 (July 18, 2026)** refreshed the bundled instructions for Sol, Terra, and Luna and corrected their context windows to **272,000 tokens**.<sup>116</sup> **v0.145.0 (July 21, 2026)** adds experimental Bedrock login with custom endpoint and authentication support, makes **GPT-5.6 Sol the default Bedrock model**, and migrates bundled GPT-5.4 model selections to the corresponding **Terra and Luna** variants.<sup>117</sup>

**GPT-6-Astra (launched September 3, 2026; in Codex via v0.153.1-v0.153.4)**: the chronology, with explicit dates: v0.153.1 staged an API-only catalog entry on September 3, 2026; OpenAI announced GPT-6-Astra the same day; and v0.153.4 added the picker entry and the bundled-CLI-default flip on September 4. The announcement described a phased rollout that began with a limited set of organizations (cybersecurity-program partners first) and extended to ChatGPT Plus, Pro, Business, and Enterprise “over the coming days,” plus the OpenAI API and AWS; the rollout facts are press-reported (CNBC, Axios, Forbes, Al Jazeera, 9to5Mac).<sup>154</sup> Superseded September 4: OpenAI announced on X that GPT-6 Astra “is now available to all Pro, Enterprise, and Business Premium users in ChatGPT Work and Codex” and “live in the API,” with Plus and Business expected within days, and the Codex lead Tibo Sottiaux followed up the same evening: “Astra is now rolled out to all Plus and Business users too.” Entitlement was uneven in practice at first: September 5 community reports showed Plus accounts still seeing only GPT-5.6 models in Codex.<sup>158</sup>

**Astra pricing and positioning:** the API model page lists **gpt-6-astra** at **$10/MTok input and $50/MTok output at standard short-context rates, with a 1.05M-token context window, 922,000 max input tokens, 128K max output, and a knowledge cutoff of April 30, 2026**. Prompts above 272K input tokens are priced at 2x input and 1.5x output for the full request, which the pricing page lists as $20/MTok in and $75/MTok out. The page describes Astra as “our most capable model for the most demanding work.”<sup>234</sup> History: in September 2026 the Codex docs said Astra availability “depends on the rollout, your sign-in method, and your client”; the models page now lists four recommended models and keeps Astra for the hardest end-to-end work.<sup>155</sup><sup>233</sup>

**Astra reasoning ladder:** the Codex ladder is **Low, Medium, High, Extra High, Max, Ultra**. The bundled catalog sets `default_reasoning_level` to `low`, which matches the docs’ suggested starting point of Light (`low`); check `/status` in a session for the effective value. The catalog’s picker text for Ultra is “Maximum reasoning with automatic task delegation,” and the models docs say Ultra uses subagents to handle separate parts of a complex task in parallel.<sup>205</sup><sup>233</sup> History: in early September 2026 the models page marked Medium as the default while the v0.153.4 catalog shipped `low`; by September 23, 2026 the page gave Light as Astra’s starting point. The SDKs gained the `max` and `ultra` reasoning-effort values in v0.149.0 (#39662).<sup>129</sup><sup>155</sup><sup>157</sup><sup>231</sup>

**Astra in the CLI:** Astra has been API-configurable since v0.153.1 (`-m gpt-6-astra`), joined the Amazon Bedrock catalogs in v0.153.3 (GPT-5.6 Sol stayed the Bedrock default until v0.157.0 made it GPT-6 Sol<sup>175</sup>), and entered the model picker in v0.153.4, where it was the **bundled default when no model is explicitly configured** until v0.159.1. No minimum CLI version is documented, so treat those versions as the floors.<sup>151</sup><sup>156</sup><sup>157</sup><sup>179</sup> Codex cloud, as the docs read at the time: Astra was marked available there on September 15, 2026 and unavailable on September 23; the models page no longer has a Codex cloud availability line.<sup>155</sup><sup>231</sup><sup>233</sup> Entitlement still varies by account, so if the bundled default is a model your account cannot use, pin one in `config.toml` (or pass `-m`); the docs’ own example is now `model = "gpt-6.1-sol"`.<sup>233</sup>

**Astra speed tiers:** the Codex speed docs put Fast mode at **2.5x the Standard rate for included subscription limits and 2x for purchased credits**, and the API model page prices Fast at **2x the applicable rates** ($20/$100 per MTok short-context, $40/$150 long-context on the API pricing page). The speed page states no Fast speed multiplier for Astra; “2x speed” appears only in the bundled catalog’s description of the tier.<sup>205</sup><sup>234</sup> Ultrafast covers Astra on Pro $500 and eligible Enterprise and Edu plans: the speed docs say it “generates tokens up to 8x faster than GPT-6 Astra in Standard mode in Codex,” it draws included limits at 8x the Standard rate and purchased credits at 6x, and the API rate is $60/$300 per MTok.<sup>204</sup>

**How the recommendation moved:** on September 23, 2026, after the GPT-6 Sol and Luna launch, the docs pointed complex coding and agentic workflows at GPT-6 Sol and focused, high-volume tasks at Luna, marked Astra and Luna unavailable for Codex cloud, and gave Astra’s starting effort as Light (`low`), which matches the bundled catalog. Astra was still the bundled CLI default at that point.<sup>172</sup><sup>231</sup> On September 29, 2026, v0.159.1 made GPT-6.1 Sol the bundled default and moved Astra to second in the picker; the models docs recommended GPT-6.1 Sol for complex coding and agentic workflows.<sup>179</sup><sup>181</sup> The page now suggests GPT-6.1 Sol for repeated, long-running work, keeps Astra for the hardest end-to-end work, and uses `gpt-6.1-sol` in its configuration example.<sup>233</sup>

**Power presets:** in the desktop app and ChatGPT Work on the web, the picker’s Power presets map onto models and reasoning levels. The models docs currently show six, “Luna High, GPT-6 Sol Light (selected in this example), GPT-6 Sol Medium, Astra Light, Astra Medium, and Astra Extra High,” note that “Some paid plans omit Astra Extra High,” and say the available options and defaults vary by plan, client, workspace settings, and rollout. For starting effort they say to use the client’s default for GPT-6.1 Sol, High for Luna, and Light (`low`) for Astra.<sup>233</sup> History: as read on September 7, 2026, the docs mapped the presets onto Astra for eligible Pro, Business ($100), and Enterprise accounts (“Terra Light, Sol Light, Sol Medium, Astra Light, Astra Medium, and Astra Extra High”), and the September 23 page, after the GPT-6 Sol and Luna launch, suggested Medium as the start for Sol.<sup>155</sup><sup>231</sup>

**Experimental cross-context notes (history):** in September 2026 the Codex models docs carried a section on experimental context management, which read: “On supported Codex clients, users signed in with ChatGPT Plus or Pro can opt in to experimental context management. Astra keeps notes across context windows and can search earlier messages and tool results from the same task. This experiment is off by default and isn’t available with Business, Enterprise, or API-key sign-in at launch.” The opt-in was `features.context_management.experimental_mode = true` in `config.toml`, the same surface v0.153.0 shipped (see Context Management); the v0.153.0 release notes had named eligible Plus, Pro, and Pro Lite sessions. v0.154.0 tightened the code side by gating eligibility on the active model’s capabilities at session startup (#43147). The section is no longer on the page; the next note covers what replaced it.<sup>149</sup><sup>155</sup><sup>161</sup><sup>233</sup>

**Cross-context notes, later status.** **v0.157.0:** the bundled model catalog no longer marks GPT-6-Astra, or any other bundled model, as supporting experimental context (#47397), and the CLI turns the experiment on only for a model marked that way. With the bundled catalog, then, the opt-in has no effect; whether the catalog served to signed-in accounts differs is not documented. The models docs still described the opt-in on September 26, 2026.<sup>175</sup> **Since September 29, 2026:** the models page no longer carries the section, and the config reference describes `features.context_management.experimental_mode` as “Experimental context-management setting. The feature is not currently available.” GPT-6.1 Sol, added in v0.159.1, was not marked as supporting it either.<sup>179</sup><sup>233</sup>


### Model Selection Flowchart

```
Is this a quick fix, extraction, or other well-defined repeatable task?
├─ Yes → gpt-6-luna (most efficient; docs suggest starting at High)
└─ No
   ├─ Do you need real-time pairing speed?
   │  ├─ Yes → Fast mode on gpt-6.1-sol or gpt-6-luna (/fast; 2.5x included usage)
   │  └─ No
   │     ├─ Hardest end-to-end work across many steps and tools?
   │     │  ├─ Yes → gpt-6-astra (strongest; most expensive)
   │     │  └─ No → gpt-6.1-sol (near-Astra at lower cost; default since v0.159.1)
   └─ GPT-6.1 Sol not yet available on your account? → gpt-6-sol, then gpt-5.6-sol
```
### Reasoning Effort

Control how much the model “thinks” before responding:

| Level | Behavior | When to Use | 
|---|---|---|
| `minimal` | Minimal reasoning (older GPT-5 models only; no model in the catalog bundled with v0.167.1 offers it) | Trivial tasks, quick lookups | 
| `low` | Brief reasoning (the catalog default for GPT-6.1 Sol, GPT-6 Astra, GPT-6.1 Terra, and GPT-5.6 Sol) | Standard coding tasks, formatting | 
| `medium` | Balanced (the catalog default for GPT-6 Sol, GPT-6 Luna, GPT-5.6 Terra, and GPT-5.6 Luna) | Most development work | 
| `high` | Extended reasoning | Complex bugs, architecture | 
| `xhigh` | Extra-high reasoning ( `max` and`ultra` sit above it on models that offer them) | Security audits, deep analysis | 
| `persistent` | Model-advertised persistent reasoning (v0.151.0); shown as “Persistent” in the TUI, preserved in local config while the Responses API receives the `disabled` wire value | Long-lived proactive sessions where the model paces its own follow-ups | 
| `max` /`ultra` | Top rungs of Astra’s documented six-level ladder, above `xhigh` ; exposed in the SDKs since v0.149.0 (#39662); the bundled catalog describes Ultra as “Maximum reasoning with automatic task delegation,” and the models docs say it uses subagents to handle separate parts of a complex task in parallel | The hardest problems, on models that support them <sup>129</sup><sup>205</sup><sup>233</sup> | 

Supported levels and the default are model-dependent: the catalog bundled with v0.167.1 starts GPT-6.1 Sol at `low` and offers `low` through `ultra` (GPT-6 Luna stops at `max`). `minimal` is only available for older GPT-5 models. Not all models support every level.<sup>205</sup>

**`persistent` details (v0.151.0):** turns running at `persistent` effort default the current-time reminder and the interruptible `clock.sleep` tool on (explicit `features.current_time_reminder` settings and managed requirements are preserved), and ship bundled proactivity and follow-up guidance that model metadata can override or disable via `persistent_instructions`.<sup>145</sup>


```
codex -c model_reasoning_effort="xhigh" "find the race condition"
```
**Expert tip**: `xhigh` reasoning takes longer and uses more tokens than `medium` for the same prompt.<sup>254</sup> Reserve it for genuinely hard problems where the extra thinking pays off.


**TUI quick reasoning controls (v0.124.0+).**<sup>85</sup> In an interactive TUI session, **`Alt+,`** lowers reasoning one step and **`Alt+.`** raises it one step, which is useful when a hard problem mid-session warrants a temporary `medium → high → xhigh` ramp without `/model --reasoning` or `-c`. When you accept a model upgrade mid-session, reasoning resets to the new model’s default rather than carrying the prior level over.

### Model Switching

Switch models mid-session with the `/model` slash command, or set per-run via `--model` / `-m`:

```
codex -m gpt-6.1-sol -c service_tier="fast" "pair with me on this component"   # Fast tier: 2.5x included usage
```
## What Does Codex Cost?

See also Model Selection for capabilities and Decision Frameworks for choosing the right model per task.

### Access via ChatGPT Plans

Codex availability depends on your ChatGPT plan and organization settings:<sup>59</sup><sup>235</sup>

| Plan | Price | Codex Access | Rate Limits (5-hour window) | 
|---|---|---|---|
| **Free / Go** | $0 / $8 | GPT-6 Luna at Standard speed in the desktop app, subject to rollout <sup>203</sup> | Not published on the pricing page | 
| **Plus** | $20/mo | Web, CLI, IDE extension, and iOS, plus cloud integrations; the plan card names GPT-6.1 Sol and GPT-6 Luna <sup>235</sup> | Per-model estimates, e.g. GPT-6.1 Sol 15-160 msgs, GPT-6.1 Terra 35-285, GPT-6 Sol 15-150, GPT-6 Luna 350-3,000, GPT-6 Astra 5-45; weekly limits may also apply <sup>203</sup> | 
| **Pro** | $100, $200, or $500/mo | Everything in Plus; Ultrafast mode (GPT-6.1 Sol and GPT-6 Astra) on Pro $500 | No five-hour limit (“Pro plans currently have no five-hour limit”); check the usage dashboard for current limits <sup>203</sup><sup>204</sup> | 
| **Business** | $25/user/mo ($20 annual, 2+ users) | Standard seat includes Codex + ChatGPT, SAML SSO | Standard seats get the same five-hour estimates as Plus <sup>203</sup><sup>235</sup> | 
| **Business (Codex-only seat)** | Pay-as-you-go | Token-based billing with no fixed seat fee, as announced on April 2, 2026; the pricing page has no card for this seat type today, so confirm it with your workspace admin or OpenAI sales <sup>79</sup><sup>235</sup> | Token-based | 
| **Enterprise / Edu** | Contact sales | Everything in Business plus enterprise security and admin controls (SCIM, EKM, RBAC), audit logs through the Compliance API, and data retention and residency controls | With flexible pricing, no fixed rate limits (usage scales with credits); without it, the same per-seat limits as Plus for most features <sup>235</sup> | 
| **API Key** | Usage-based | CLI, SDK, IDE only (no cloud features) <sup>235</sup> | Token-based | 

**April 2026 pricing update:** Business annual pricing dropped from $25 to $20/seat/mo. Codex-only seats with pay-as-you-go pricing became available for Business and Enterprise workspaces, with no fixed seat fee and billing on token consumption.<sup>79</sup> The pricing page now publishes limits as per-model bands (see the table above) rather than the promotional 2x multipliers from the February 2026 Desktop App launch.<sup>16</sup><sup>125</sup><sup>203</sup>

**May 2026 usage limit boost (expired May 31, 2026):** Codex on the Plus plan ran at a **25× 5-hour limit** (vs the standard 20× boost), and the **$100/month tier was doubled** for the same window.<sup>89</sup>


**Rate-limit UX in the TUI (v0.152.0-v0.153.0):** hitting a limit is no longer a dead end. v0.152.0 renders **actionable rate-limit banners** above the composer – check usage, manage credits, reset limits, notify the workspace owner, and manage plans – filtered to the authenticated account and user; usage refreshes after limit errors, and when the banner instructs it, Codex switches to the first available fallback model without changing unrelated thread settings.<sup>147</sup> v0.153.0 adds **earlier warnings for Plus and Team plans**: a warning fires when less than half of the allowance remains in an approximately five-hour usage window, while other plans and window lengths keep the existing 75%, 90%, and 95% thresholds.<sup>149</sup>

### Credit Costs

Codex operations consume credits from your plan allocation:

| Model | Credits / 1M input tokens | Cached / Output | Notes | 
|---|---|---|---|
| GPT-6 Astra | 250 | 25 / 1,250 | Strongest and most expensive current model; for the hardest end-to-end work <sup>203</sup><sup>233</sup> | 
| GPT-6.1 Sol | 50 | 2.5 / 250 | Near-Astra performance at a lower cost; the docs suggest it for repeated, long-running work, and it is the bundled default since v0.159.1 <sup>179</sup><sup>203</sup><sup>233</sup> | 
| GPT-6.1 Terra | 25 | 1.25 / 125 | Everyday writing and visual work; in Codex since October 26, 2026 <sup>198</sup><sup>203</sup> | 
| GPT-6 Sol | 50 | 5 / 250 | Previous-generation Sol; half the GPT-5.6 Sol promotional rate <sup>203</sup> | 
| GPT-6 Luna | 2.5 | 0.25 / 12.5 | Focused, high-volume tasks; the cheapest current model <sup>203</sup><sup>229</sup> | 
| GPT-5.6 Sol | 100 | 10 / 500 | Previous-generation Sol; August 21 promotional cut (was 125 / 12.5 / 750), through at least November 21, 2026 <sup>125</sup><sup>130</sup><sup>203</sup> | 
| GPT-5.6 Terra | 50 | 5 / 300 | Previous-generation everyday model <sup>203</sup> | 
| GPT-5.6 Luna | 5 | 0.5 / 30 | Previous-generation fast tier <sup>203</sup> | 

The rates are for Standard speed, and the pricing page says GPT-5.6 Sol, Terra, and Luna rates “remain unchanged.” For scale, the page says “A typical GPT-5.6 Sol task may use 2-15 credits” (the page gave an average of 5-30 credits per message in September, and 5-40 in August).<sup>125</sup><sup>232</sup> Fast mode draws included subscription limits at 2.5x the Standard rate and purchased credits at 2x, on GPT-6.1 Sol, GPT-6.1 Terra, and GPT-6 Astra, Sol, and Luna where available. The bundled catalog makes Fast the default tier for GPT-6 Sol and Luna (not GPT-6.1 Sol) in the TUI unless you set `service_tier`, so toggle Fast off with `/fast`, or set `service_tier`, if credits drain faster than expected. Ultrafast, for GPT-6.1 Sol and GPT-6 Astra on Pro $500 and eligible Enterprise and Edu plans, draws included limits at 8x and purchased credits at 6x; other self-serve plans have no access to it, even with purchased credits. Image generation uses included limits 3-5x faster on average than comparable turns without it.<sup>203</sup><sup>204</sup><sup>205</sup><sup>234</sup>

Enterprise and Edu plans scale credits with contract allocation. Check `/status` in the TUI for current usage.


### API Billing

When using Codex through the API, OpenAI bills usage per token under standard OpenAI API pricing for the selected model (plus any applicable prompt-caching discounts). Current API rates per 1M tokens: GPT-6.1 Sol $2 input / $10 output ($0.10 cached input), GPT-6 Sol $2 / $10, GPT-6 Luna $0.10 / $0.50, GPT-6 Astra $10 / $50; for all four, prompts above 272K input tokens cost 2x input and 1.5x output for the full request, and Fast mode costs 2x.<sup>228</sup><sup>234</sup> Ultrafast, the `ultrafast` service tier on the API, costs 6x Standard: $12 / $60 for GPT-6.1 Sol and $60 / $300 for GPT-6 Astra.<sup>204</sup> ChatGPT credit multipliers do not apply to API-key sign-in. Check the API pricing page for current rates.<sup>20</sup><sup>228</sup>

### Cost Optimization Strategies

1. **Use profiles** : Create a`fast` profile with`gpt-6-luna` and`model_reasoning_effort = "low"` for routine tasks
2. **Reserve high reasoning** : Only use`xhigh` for genuinely hard problems, as higher effort takes longer and uses more tokens<sup>254</sup>
3. **Use `codex exec --ephemeral`** : skip writing session files for CI/CD runs you will never resume (the flag exists only on`codex exec` , and it does not change token cost)<sup>246</sup>
4. **Leave reasoning summaries off** :`model_reasoning_summary` already resolves to`none` for every model in the bundled catalog, and setting`auto` ,`concise` or`detailed` adds summary output; a model missing from the catalog falls back to`auto` , so set`none` there<sup>253</sup>
5. **Batch with exec mode** :`codex exec` avoids TUI overhead for automation workflows
6. **Monitor usage** : Check`/status` in the TUI and your org billing dashboards — as of v0.148.0,`/status` , status lines, and terminal titles show estimated thread credits or cost for eligible workspaces<sup>127</sup>

### Real-World Cost Examples

Representative API costs for common tasks at GPT-6.1 Sol Standard rates ($2 input / $10 output per 1M tokens), with no cached input and every request under the 272K-token long-context threshold:

| Task | Input Tokens | Output Tokens | Approx. Cost | 
|---|---|---|---|
| Explain a 500-line module | ~15K | ~2K | ~$0.05 | 
| Fix a failing test (1-2 files) | ~30K | ~5K | ~$0.11 | 
| Add a new API endpoint with tests | ~60K | ~15K | ~$0.27 | 
| Refactor auth module (10 files) | ~120K | ~30K | ~$0.54 | 
| Full repo audit via `codex exec` | ~200K | ~20K | ~$0.60 | 
| Cloud task: triage 20 open issues | ~250K | ~40K | ~$0.90 | 

Each figure is input tokens times the input rate plus output tokens times the output rate; the first row is 15,000 x $2/1M + 2,000 x $10/1M = $0.03 + $0.02. GPT-6 Astra ($10 / $50) costs five times these figures, GPT-6.1 Terra ($1 / $5) half, and GPT-6 Luna ($0.10 / $0.50) one twentieth. Real costs also vary with reasoning effort, caching (cached input is discounted), and conversation length.<sup>228</sup>


### Hidden Token Overhead

Every tool call adds tokens beyond your visible prompt:<sup>254</sup>

| Overhead Source | Approx. Cost | 
|---|---|
| System prompt + AGENTS.md | Grows with your AGENTS.md files, up to `project_doc_max_bytes` (32 KiB by default) | 
| File reads ( `@file` ) | Full file content tokens | 
| MCP tool definitions | Every connected server adds context to your messages | 
| Reasoning traces | Variable; higher effort uses more tokens | 

**Expert tip:** Monitor your actual usage via `/status` in the TUI. The token count includes all overhead, not just your visible messages. If costs surprise you, check how many MCP servers are connected — each adds tool definitions to every API call.


### Team Cost Management

| Team Size | Recommended Config | Monthly Seat Cost | 
|---|---|---|
| Solo developer | Plus or Pro; default model, `medium` reasoning | $20 (Plus), or $100, $200, or $500 (Pro) | 
| Small team (3-5) | Business; profiles ( `fast` /`careful` ), code reviews via`codex exec` | $75-125 billed monthly, or $60-100 on annual billing | 
| Medium team (10-20) | Business or Enterprise; managed `requirements.toml` , CI integration | $250-500 billed monthly, or $200-400 on annual billing (Business); Enterprise by quote | 
| Large org (50+) | Enterprise with admin controls, audit logging, allocated budgets | Custom pricing | 

The seat figures are the number of seats times the plan prices above: Business is $25 per user per month, or $20 on annual billing, for two or more users.<sup>203</sup> They cover the usage each plan includes. Purchased credits beyond the included limits, and API-key usage billed per token (see Real-World Cost Examples), come on top and depend on how much the team runs, so they are not estimated here.


Strategies for team cost control:

- **Use `requirements.toml` for what it can enforce** : it pins the model provider (`model_provider` ) and the model catalog file (`model_catalog_json` ), and`[models.new_thread]` sets the default model and reasoning effort for new threads. Those defaults are not limits (a user who picks another model or effort overrides both), and there is no model allowlist or reasoning-effort cap.<sup>219</sup>
- **Use `gpt-6-luna` for CI/CD** : automated pipelines rarely need maximum reasoning
- **Profile-based budgeting** — define`ci` ,`review` , and`dev` profiles with appropriate cost ceilings
- **Monitor via OpenTelemetry** — enterprise deployments can export usage telemetry to existing observability stacks

## Decision Frameworks

### When to Use Each Surface

| Scenario | Best Surface | Why | 
|---|---|---|
| Quick bug fix | CLI | Fast, focused, scriptable | 
| Multi-file refactor | CLI or App | CLI for deterministic patches; App for visual diff review | 
| Exploring unfamiliar code | CLI | Terminal composability, grep/find integration | 
| Parallel workstreams | Desktop App | Worktree isolation, multi-task management | 
| Active file editing | IDE Extension | Inline edits, tight compile-test loop | 
| Long-running migration | Cloud | Runs independently, creates PR when done | 
| CI/CD automation | `codex exec` | Non-interactive, JSON output, scriptable | 
| Code review | CLI or App | `/review` command with presets | 
| Team onboarding | Desktop App | Visual, guided, less terminal knowledge needed | 

### When to Use Each Sandbox Mode

| Scenario | Mode | Approval | Why | 
|---|---|---|---|
| Exploring unknown code | `read-only` | `on-request` | Maximum safety, can’t break anything ( `untrusted` retired in v0.149.0) | 
| Daily development | `workspace-write` | `on-request` | Good balance of speed and safety | 
| Trusted automation | `workspace-write` | `never` | Fast, uninterrupted, sandboxed | 
| System administration | `danger-full-access` | `on-request` | Needs full access; with no sandbox boundary left, `on-request` rarely prompts<sup>132</sup> | 
| CI/CD pipeline | `workspace-write` | `never` | Automated, sandboxed to workspace | 

### When to Use Each Reasoning Level

| Task Type | Reasoning | Model | Profile | 
|---|---|---|---|
| Formatting, linting | `low` | `gpt-6-luna` | `fast` | 
| Standard coding | `medium` | `gpt-6.1-sol` | default | 
| Complex debugging | `high` | `gpt-6.1-sol` or`gpt-6-astra` | `careful` | 
| Security audit | `xhigh` | `gpt-6-astra` | `careful` | 
| Rapid prototyping | `low` | `gpt-6-luna` | `fast` | 
| Migration/refactor | `medium` -`high` | `gpt-6.1-sol` or`gpt-6-astra` | default | 

### Plan Mode vs Direct Execution

```
Will Codex need to change more than 3 files?
│
├── YES → Use Plan Mode (/plan)
│         Codex designs the approach before making changes.
│         You review and approve the plan.
│         Best for: refactors, new features, migrations
│
└── NO → Is the change well-defined?
         │
         ├── YES → Direct execution
         │         Just describe the task. Codex executes immediately.
         │         Best for: bug fixes, small features, test additions
         │
         └── NO → Use Plan Mode (/plan)
                  Let Codex explore and propose an approach first.
                  Best for: unfamiliar codebases, ambiguous requirements
```
### Steer Mode: Enter vs Tab

| Situation | Use Enter | Use Tab | 
|---|---|---|
| Codex is about to make a mistake | Send correction immediately |  | 
| You have a follow-up task |  | Queue it for after current work | 
| Codex picked the wrong file | Send redirect immediately |  | 
| You want to add scope |  | Queue the addition | 
| Urgent priority change | Send new priority immediately |  | 
| Non-critical context |  | Queue it — no rush | 

**Rule of thumb**: Enter = “stop, listen to this now.” Tab = “when you’re done, also do this.”

### Desktop App vs CLI

```
How do you prefer to work?
│
├── Terminal-first → Use CLI
│   │
│   ├── Single focused task → codex (interactive TUI)
│   ├── Scripted automation → codex exec (non-interactive)
│   └── Quick one-shot → codex exec "prompt" -o result.txt
│
└── Visual/multi-project → Use Desktop App
    │
    ├── Multiple parallel tasks → Multi-thread with worktree isolation
    ├── Visual diff review → Built-in Git diff viewer
    ├── Scheduled automation → Scheduled in the sidebar
    └── Voice-driven → Dictate button in the composer
```
| Capability | CLI | Desktop App | 
|---|---|---|
| Interactive sessions | Yes | Yes | 
| Parallel agents | Manual (multiple terminals) | Built-in (worktree isolation) | 
| Diff review | `/diff` (text) | Visual inline diffs | 
| Automations | Cron + `codex exec` | GUI scheduler | 
| Voice input | Yes: `/voice` live conversations, on by default with an F8 toggle (v0.156.0; experimental and opt-in in v0.155.0)<sup>165</sup><sup>170</sup> | Yes: **Dictate** in the composer<sup>249</sup> | 
| CI/CD integration | `codex exec` + GitHub Action | No | 
| Session sync | Yes | Yes (shared with CLI) | 

### Which Profile?

Match your task to a pre-configured profile:

| Task Type | Profile | Key Settings | 
|---|---|---|
| Quick questions, formatting | `fast` | `model = "gpt-6-luna"` ,`model_reasoning_effort = "low"` | 
| Daily development | (default) | `model = "gpt-6.1-sol"` ,`model_reasoning_effort = "medium"` | 
| Architecture, security | `careful` | `model = "gpt-6-astra"` ,`model_reasoning_effort = "xhigh"` | 
| Real-time pairing | `pair` | `model = "gpt-6.1-sol"` ,`service_tier = "fast"` | 
| CI/CD automation | `ci` | `model = "gpt-6-luna"` ,`model_reasoning_effort = "low"` ,`sandbox_mode = "workspace-write"` | 

**Profile files** (one per preset, in `~/.codex/`; the legacy single-file `profile = "default"` + `[profiles.*]` layout is rejected since 0.134.0<sup>131</sup><sup>238</sup>):

```
# ~/.codex/fast.config.toml
model = "gpt-6-luna"
# GPT-6 Sol/Luna start on the Fast tier (2.5x included usage) unless service_tier is set; /fast toggles it
model_reasoning_effort = "low"
```
```
# ~/.codex/careful.config.toml
model = "gpt-6-astra"
model_reasoning_effort = "xhigh"
```
```
# ~/.codex/pair.config.toml
model = "gpt-6.1-sol"
service_tier = "fast"                    # Fast tier: 2.5x included usage on a ChatGPT plan
```
```
# ~/.codex/ci.config.toml
model = "gpt-6-luna"
# codex exec does not apply the Fast-tier default the TUI gives GPT-6 Sol/Luna; set service_tier here if you want it
model_reasoning_effort = "low"
sandbox_mode = "workspace-write"
```
Switch profiles per-session: `codex --profile careful`

## How Does the Sandbox & Approval System Work?

Codex uses a **two-layer security model** that separates *what is technically possible* from *when Codex asks for human approval*. The approach differs fundamentally from Claude Code’s permission system — Codex enforces restrictions at the OS kernel level.<sup>5</sup> See also Enterprise Deployment for `requirements.toml` constraints that administrators enforce organization-wide.

**v0.148.0 hardening:** sandbox restrictions now **fail closed** — a denied or unreadable path is treated as blocked instead of silently allowed — on both Linux and Windows.<sup>127</sup>


### Layer 1: Sandbox (What Is Possible)

The sandbox controls filesystem and network access using OS-native mechanisms:

| Mode | File Access | Network | When to use | 
|---|---|---|---|
| `read-only` | Read only everywhere | Blocked | Strictest; mutations need explicit approval | 
| `workspace-write` | Reads machine-wide on macOS (the Seatbelt profile allows `file-read*` unless unreadable roots are configured); writes in the workspace,`/tmp` and`$TMPDIR` , except`.git` ,`.agents` ,`.codex` and`.aws` inside a writable root, which stay read-only<sup>191</sup><sup>214</sup><sup>244</sup> | Blocked by default | Normal development; safe default | 
| `danger-full-access` | Full machine access | Enabled | Maximum capability; use with caution (denylist-only variant removed in v0.121.0 — now binary on/off) <sup>82</sup> | 

**Platform-specific enforcement:**

- **macOS** : Apple’s Seatbelt framework via`sandbox-exec` with mode-specific profiles compiled at runtime and enforced by the kernel<sup>6</sup> . As of v0.121.0, macOS sandbox profiles can allowlist specific**Unix sockets** (e.g.,`docker.sock` , editor IPC sockets) and private DNS resolution is no longer blocked by default.<sup>82</sup>
- **Linux** : bubblewrap (`bwrap` ) for filesystem and namespace isolation + seccomp for syscall filtering, run through a standalone helper process (`codex-linux-sandbox` ); the Landlock implementation<sup>7</sup> is retained as legacy, and v0.163.0 renamed the crate’s Landlock-named command type and syscall module without changing enforcement.<sup>5</sup><sup>195</sup> Codex prefers the first`bwrap` on`PATH` outside the working directory and falls back to the bundled`codex-resources/bwrap` (vendored since v0.100.0), with a startup warning when`bwrap` is missing or cannot create user namespaces.<sup>212</sup> v0.117.0 improved sandbox reliability on older distros with legacy kernel configurations.<sup>75</sup> v0.129.0 hardened sandbox startup on Linux and bumped vendored Bubblewrap to 0.11.2 with upstream security patches; v0.130.0 added further startup hardening.<sup>89</sup><sup>91</sup>
- **Windows** : three native sandbox implementations.`mxc` is the recommended one on compatible devices: native process isolation with no administrator setup, extra Windows accounts, or firewall rules. It needs Windows 11 24H2 (build 26100.9278) or 25H2 (build 26200.9278).`elevated` is the preferred legacy fallback: dedicated lower-privilege sandbox users, filesystem permission boundaries, and firewall rules, set up once with administrator approval (IT can provision it ahead of time with`codex sandbox setup --elevated --user 'DOMAIN\alice' --codex-home 'C:\Users\alice\.codex'` , naming the employee’s Windows identity and Codex home).`unelevated` is the secondary legacy fallback: a restricted Windows token with ACL-based boundaries, weaker network isolation than`elevated` , and no support for denied read paths. WSL2 is also supported and uses the Linux bubblewrap + seccomp sandbox; WSL1 has not been supported since v0.115.0.<sup>2</sup> History: the native sandbox was promoted from experimental in v0.100.0, v0.117.0 improved the restricted-token sandbox’s process isolation,<sup>75</sup> and v0.130.0 gave sandbox users access to the desktop runtime binary cache so the sandbox can resolve runtime binaries reliably.<sup>91</sup>

On Windows, pick the implementation in `config.toml`. The desktop app prefers MXC automatically for consumer accounts; for the standalone CLI, either select `mxc` explicitly or turn on `prefer_mxc`, which uses MXC when the device and policy support it and otherwise falls back to the legacy choice. The `prefer_mxc` flag is off by default and marked under development at v0.167.1. Administrators can block MXC (`allow_mxc = false`) or restrict the legacy choices (`allowed_sandbox_implementations = ["elevated"]`) under `[windows]` in `requirements.toml`.<sup>2</sup>

```
[windows]
sandbox = "elevated"    # mxc | elevated | unelevated
[features]
prefer_mxc = true       # Use MXC when available, else the legacy choice above
```
**Why this matters**: Unlike container-based sandboxing (Docker), OS-level sandboxing needs no image or daemon, and it restricts each command Codex spawns rather than isolating a whole environment. The operating system, not Codex, enforces the limits on the spawned process.


**Security fixes:**

- **zsh-fork sandbox bypass (v0.106.0):** Fixed a vulnerability where shell execution via zsh forking could bypass sandbox restrictions.<sup>60</sup> If you are on an earlier version, upgrade immediately.
- **Input size cap (v0.106.0):** Codex now enforces an approximately 1-million-character input cap to prevent hangs from excessively large payloads.<sup>60</sup>
- **Secure devcontainer profile (v0.121.0, removed in v0.155.0):** v0.121.0 added a hardened devcontainer profile to the Codex repository that ran Bubblewrap inside the container (WSL2 supported, WSL1 rejected).<sup>82</sup> v0.155.0 removed the repository’s devcontainer profiles, Dockerfiles, and firewall scripts (#43915), so at v0.167.1 there is no`.devcontainer/` directory to copy; build your own container and rely on the sandbox inside it.<sup>248</sup>
- **Guardian review + hooks (v0.121.0):** Hooks are**disabled** during Guardian review sessions so pre/post-tool hooks cannot interfere with the Guardian subagent’s decisions.<sup>82</sup> If you rely on hooks for logging or validation, be aware that a Guardian review skips them — fall back on app-server observability if you need a full audit trail.
- **Linux /dev filesystem (v0.105.0):** Sandboxed commands on Linux now receive a minimal`/dev` filesystem, improving compatibility with tools that expect device nodes.<sup>61</sup>

**Restricting reads:** `[sandbox_workspace_write]` has no read-restriction key. Its only keys are `writable_roots`, `network_access`, `exclude_tmpdir_env_var` and `exclude_slash_tmp`, and a key Codex does not recognize there loads without an error and without effect, so a made-up key such as `read_only_access` leaves reads machine-wide. To make paths unreadable, use a permission profile: a custom profile that does not extend a built-in one reads only the paths it grants, so start from `":minimal" = "read"` under `[permissions.<name>.filesystem]` and mark sensitive paths `"deny"` (see Permission Profiles below). The name `readOnlyAccess` comes from the app-server protocol, where it is a legacy sandbox-policy input and never a `config.toml` key; at v0.167.1 the app-server rejects its restricted form with an error that points to permission profiles.<sup>213</sup>

### Layer 2: Approval Policy (When To Ask)

The approval policy determines when Codex pauses to ask for human confirmation:

| Policy | Behavior | Use Case | 
|---|---|---|
| `untrusted` | Auto-runs safe reads; prompts for everything else | **Retired in v0.149.0.** A config that still names it stops Codex from starting with the message`approval_policy = "untrusted" is no longer supported; remove this setting` . Delete the line or set`on-request`<sup>129</sup><sup>243</sup> | 
| `on-request` | Approves within sandbox; prompts for boundary violations | Default; good balance | 
| `never` | No prompts whatsoever | CI/CD, trusted automation | 

`on-failure` still appears in some older examples. It is deprecated and no longer a policy of its own: the configuration reference says to use `on-request` or `never` instead, and at v0.167.1 the old name loads as an alias of `on-request`. Use `on-request` for interactive runs and `never` for non-interactive runs that already have an external safety boundary.<sup>87</sup><sup>243</sup>

As of v0.150.0 you can bind a keyboard shortcut that cycles permission modes from the composer – configure it via `/keymap`.<sup>141</sup>

### Distinct Approval IDs (v0.104.0+)

Codex now assigns distinct approval IDs to each command within a multi-step shell execution. This means approvals are granular — approving one command in a sequence doesn’t automatically approve subsequent ones in the same shell invocation.<sup>57</sup>

### Flexible Approval Controls (v0.105.0+)

The approval flow now supports extra sandbox permissions and granular rejection:<sup>61</sup>

- **Extra sandbox permissions** : When a command needs access beyond the current sandbox mode, Codex can request specific additional permissions rather than requiring a full mode change. At v0.167.1 this sits behind the`exec_permission_approvals` flag, which is under development and off by default<sup>247</sup>
- **Granular rejection** : Reject individual tool calls with feedback so Codex can adjust its approach rather than simply re-attempting the same command

### Runtime Permission Requests (v0.113.0+)

v0.113.0 added a built-in `request_permissions` tool that allows the model to request additional permissions at runtime.<sup>69</sup> When the model encounters a task requiring elevated access, it can formally request specific permissions (filesystem paths, network access, etc.) through the TUI approval flow rather than failing silently or requiring the user to restart with different flags. At v0.167.1 the tool is not exposed by default: it sits behind the `request_permissions_tool` flag, which is under development and off. Set `request_permissions_tool = true` under `[features]` to try it, and expect it to change.<sup>247</sup>

### App Approval Mode `writes` (v0.144.0+)

Apps and connectors gain a middle approval tier: the **`writes` app-approval mode** lets an app’s declared read-only actions run without prompting while still pausing for anything that writes.<sup>112</sup> Before this, app approvals were closer to all-or-nothing; `writes` matches the trust shape most operators actually want for integrations — free to look, ask to touch.

### Automatically Reviewed Approvals: `--approve-for-me` (v0.147.0+)
