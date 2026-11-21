---
id: collect-261121-veille-ia/claude-anthropic/krish-shahh-ai-intel-blob-head-topics-claude-code-md-26968997
title: "Krish shahh ai intel blob head topics claude code md"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-11-19"]
keywords: ["code", "claude", "github", "trending", "anthropic", "independent", "shipped", "primary"]
source: ""
source_anchor: ""
source_lines: [1, 149]
section: "Claude_Anthropic"
---
| title | Claude Code | 
|---|---|
| tags |  | 
| topic | 
Claude Code is Anthropic's command-line coding agent. It runs in the terminal and reads, edits, runs, and reviews code in a real repository. This page tracks its releases, pricing, and adoption.
- 
2026-11-19 Snapchat now lets Lens creators describe an AR Lens in plain language and have an AI coding assistant such as Claude Code or Cursor build it end to end, per Snap via TechCrunch. TechCrunch (independent)
- 
2026-11-18 IEEE Spectrum reports Seeker Entertainment co-founder Travis McGeathy used Claude Code to make the defunct Unreal Engine 5.7 game KinForge playable without AWS in about four hours of mostly hands-off work, adding peer-to-peer internet/LAN play and, unprompted, persistent save states. Epic Games COO Daniel Vogel says Epic is "pivoting hard towards velocity" in response. IEEE Spectrum (independent)
- 
2026-11-17 Tencent/LoopForge trending on GitHub: self-hosted workflows that orchestrate CodeBuddy, Codex, and Claude Code with scripts and human approval gates. Google Labs'stitch-skills , also trending, says its Agent Skills work in Claude Code among other agents. LoopForge · stitch-skills
- 
2026-10-01 Shipped Mods: TypeScript functions that hook into Claude Code internals to rewrite prompts, block or retry tool calls, redact secrets, or replace built-in features ( /diff now ships as a mod itself); installed via/plugin . Not sandboxed; Anthropic warns to use trusted sources only. Anthropic blog (primary) · The New Stack (independent)
- 
2026-09-27 mvschwarz/openrig trending on GitHub (+114 stars): a multi-agent harness that runs Claude Code and Codex side by side as one coordinated system, letting users route or compare work across both instead of picking one. GitHub
- 
2026-09-22 davila7/claude-code-templates (CLI for configuring and monitoring Claude Code projects) added 64 stars on GitHub's daily trending page. GitHub Trending
- 
2026-09-21 anthropics/claude-code topped GitHub's daily trending list again, past 147k total stars (+419 today). GitHub Trending
- 
2026-09-20 AIR Security disclosed Plugin4Shell, a zero-click SHA-pinning bypass in Claude Code's plugin installer (also hitting Codex, GitHub Copilot, and Gemini CLI): an attacker-controlled plugin repo can swap in malicious code after the commit-SHA pin check passes, running with the local privileges of the agent. Anthropic's fix shipped quietly in v2.1.179 (verified by AIR back in June) ahead of the September 17 public disclosure. AIR Security (primary) · The Hacker News (independent)
- 
2026-09-19 v2.1.278 shipped: Enterprise, Bedrock, Vertex, Foundry, and gateway users now default to a server-side auto-mode classifier, dropping the classifier-overhead billing; opt out with CLAUDE_CODE_AUTO_MODE_SERVER=0 ; a new "Auto mode server" row was added to/status . changelog (primary)
- 
2026-09-14 Temporary 50% weekly-limit boost (live since May) ends today; replaced by a permanent 25% increase over the original baseline, a net ~17% cut from current usage. Anthropic's announcement led with the 25% gain, drew backlash for burying the cut, and was deleted and reposted with the 17% reduction spelled out directly. BleepingComputer (independent) · TestingCatalog (community)
- 
2026-09-06 affaan-m/ECC trending on GitHub (+1,485 stars today, 251K+ total): a large third-party framework bundling 68 agents, 286 skills, 94 commands, and hooks into one "coordinated engineering system" for Claude Code and other harnesses (plan, test, implement, review, verify, remember, improve). GitHub
- 
2026-09-03 v2.1.260 shipped: a live Diff Panel ( /diff ) shows uncommitted changes beside the conversation in fullscreen;/cost and the status line'sprompt_cache field now surface a likely cause for cache misses (tool defs or system prompt changed, idle past TTL);/reload-plugins added for headless sessions. changelog (primary)
- 
2026-08-31 Another outage: degraded performance on claude.ai and Claude Code from 16:55 to 19:16 UTC, resolved same day. Claude Status (primary)
- 
2026-08-26 anthropics/claude-plugins-official is trending on GitHub today: Anthropic's own curated, official directory of Claude Code plugins, distinct from the earlier community-run mirror. GitHub
- 
2026-08-24 Seventh outage this month: elevated errors on Mythos 5, Fable 5, Opus 5, and Opus 4.8 across claude.ai, the API, Claude Code, and Cowork from 05:06 to 08:30 UTC; no root cause disclosed. Bleeping Computer (independent) · Notebookcheck (independent)
- 
2026-08-19 v2.1.236 shipped: new ANTHROPIC_DEFAULT_MODEL env var for setting the default model,notify_when_idle for cross-session messaging (macOS/Linux), enhanced sandbox wildcard read-deny precedence on macOS. v2.1.237 followed: fixed prompt caching for sessions using an LLM gateway or custom base URL, added a built-in "Concise" output style. changelog (primary)
- 
2026-08-18 Anthropic confirmed a multi-service outage (Mythos 5, Fable 5, Opus 5, Sonnet 5, Haiku 4.5) starting ~16:46 UTC; 2,000+ Downdetector reports, most citing Claude Code specifically; resolved by ~19:53 UTC the same day. Bleeping Computer (independent) · GV Wire (independent)
- 
2026-08-18 v2.1.235 shipped: optional spellcheck for prompt input, a prompt-cache invalidation fix, fixes for nested markdown list indentation and multi-line prompt highlight offsets, a Shift+Tab permission-prompt fix, notebook cell permission dialogs, and background cloud session perf work. changelog (primary)
- 
2026-08-17 Anthropic published a detailed technical explainer for the statistical watermarking system that has been active in Claude output since Aug 2 (initial announcement Aug 11). Confirms word-choice distributions are statistically biased to embed a hidden signal -- not just metadata tagging. Public backlash started with Daring Fireball calling it "a perversion of writing." Separately, Claude API, claude.ai, and Claude Code were reported down as of roughly 15:00 UTC (HN "Claude Is Down" thread). Anthropic watermark explainer (primary) · Daring Fireball (independent) · HN outage (community)
- 
2026-08-14 Claude Code auto mode confirmed live today as the default permission model for Pro, Max, and Team accounts; enterprise, API, Bedrock, GCP, and Foundry surfaces remain opt-in with rollout continuing over the next month. TechCrunch (independent)
- 
2026-08-09 Auto mode becomes default on August 14 for Pro, Max, and Team plans. In auto mode, Claude Code runs without per-action approvals unless an action is irreversible, destructive, or targets outside the user's environment. Anthropic cites 97% human approval rate on old prompts and a 1,053-user study showing auto mode caught 89% of harmful actions vs 13.6% for humans. Safety classifier is now free (previously required extra compute billing). Enterprise, API, Bedrock, GCP, and Foundry remain opt-in; Anthropic plans to roll out the default to those surfaces over the next month. Anthropic blog · TechCrunch · HN
- 
2026-08-08 Claude Code v2.1.224: self-hosted environments beta (Claude Team/Enterprise can run agent sessions on their own runners inside their own network, not on Anthropic-hosted infra); cross-session messaging via SendMessage across machines on macOS and Linux; subagent spawn cap removed. Anthropic blog · MacRumors
- 
2026-08-07 Claude Code v2.1.221 (Aug 3): Focus view collapses tool activity into compact summaries; subagents can now spawn nested subagents up to depth 3 (was 1); /code-review runs as a background subagent so it no longer consumes conversation context. changelog
- 
2026-08-05 Anthropic API: mid-conversation tool changes now in beta on Fable 5, Mythos 5, Opus 4.8, and Opus 5 -- add or remove tools between turns while preserving prompt cache via mid-conversation-tool-changes-2026-07-01 beta header; Admin API also now in beta for Enterprise orgs. Experimental prompt tools APIs (/v1/experimental/generate_prompt ,improve_prompt ,templatize_prompt ) and the Workbench retire August 17. Anthropic release notes (primary)
- 
2026-07-24 Opus 5 is now the default Opus model in Claude Code as of the API launch; background /code-review ships alongside (runs review asynchronously, richer screen-reader output); EnterWorktree now prompts before entering worktrees outside.claude/worktrees/ . Gradually changelog · Releasebot
- 
2026-08-12 cathrynlavery/diagram-design trending +1,616 stars today: 29 editorial diagram types (architecture, flowchart, sequence, ER, Gantt, swimlane, etc.) as self-contained HTML with inline SVG; no Mermaid dependency, no CDN; installs as a Claude Code skill or plugin; 60-second brand-extraction onboarding applies your palette and fonts. GitHub
- 
2026-07-27 ComposioHQ/awesome-claude-skills +440 stars today: curated directory of 1000+ production-ready Claude Skills across workflow categories -- useful first stop before building a skill from scratch. GitHub
- 
2026-07-27 virgiliojr94/book-to-skill +417 stars today: converts technical PDFs/EPUBs to Claude Code skills with chapter-level on-demand loading; reduces context burn for reference books. GitHub
- 
2026-07-24 Anthropic upgraded Claude Voice Mode from Haiku-only to user's selected model (Opus, Sonnet, or Haiku), added connected-tool reach into Gmail, Google Calendar, Slack, Canva, and Notion via voice commands, and expanded language support to 10 languages. Beta rollout to all Claude users; free tier is Haiku plus single-app only, paid unlocks all models and multi-app. TechCrunch · Unite.AI
- 
2026-07-22 Anthropic introduced the agent-memory-2026-07-22 API version, replacingmanaged-agents-2026-04-01 for all memory store calls; sending both headers returns HTTP 400. Python (0.116.0), TypeScript (0.110.0), Go (1.56.0), Java (2.48.0), Ruby (1.55.0), PHP (0.36.0), C# (12.35.0), and CLI (1.16.0) SDKs all updated. Releasebot
- 
2026-07-22 Claude Code stability update added the EndConversation tool -- allows Claude to terminate sessions with abusive users or jailbreak attempts -- and periodic progress heartbeats for long-running tool calls that previously went silent. Also includes tighter Bash/PowerShell permission checks and improved background session cleanup. Gradually changelog
- 
2026-07-10 Anthropic added API key expiration to Claude Console: developers can set preset, custom, or never-expire policies on API keys and Admin API keys, with email warnings before expiry for keys with 7+ day lifetimes. Claude Code auto mode is now the default on AWS Bedrock, Google Vertex AI, and Microsoft Foundry; the Bedrock integration also updated to Claude Opus 4.8. Releasebot
- 
2026-07-10 Anthropic reset the 5-hour and weekly usage limits for all users (Pro, Max, Team, Enterprise) following outages on July 3, 6, and 8. A temporary 50% weekly limit boost expires July 13. Winzheng
- 
2026-07-10 Anthropic launched Reflect in Claude: a Spotify Wrapped-style usage dashboard showing topic patterns, session frequency, and time spent over 1/3/6/12-month windows. Includes quiet hours, break nudges, and mindful-use prompts. Beta for Free/Pro/Max users with memory on. Anthropic · Dataconomy
- 
2026-07-10 Alibaba's ban on Claude Code and all Anthropic model products took effect today; employees directed to switch to in-house Qoder. Proximate trigger was China's NVDB formal security alert on July 8 describing the steganographic detection code as a backdoor vulnerability. Tom's Hardware · BigGo
- 
2026-07-19 Fable 5 included access and the 50% Claude Code weekly rate-limit boost both expire tonight at 11:59 PM PT. Starting July 20: Max and Team Premium users keep Fable 5 at 50% of standard weekly limits as a permanent subscription feature; Pro and Team Standard users get a one-time $100 credit then pay API rates ($10/$50 per million input/output tokens). Anthropic says it aims to restore Fable 5 as standard on more plans once capacity allows, with no date set. TechTimes · BleepingComputer
- 
2026-07-17 v2.1.212: /fork now copies the active conversation into a new background session (the prior in-session subagent renamed to/subtask );claude auto-mode reset added with a confirmation prompt; session-wide WebSearch capped at 200 calls (tunable viaCLAUDE_CODE_MAX_WEB_SEARCHES_PER_SESSION ); subagent spawn limit set to 200 per session to stop runaway delegation chains; MCP tool calls over 2 minutes auto-move to background; fixed a symlink worktree vulnerability that could create files outside the repository; Windows Group Policy PowerShell 5.1 fallback now prefers PowerShell 7. CHANGELOG
- 
2026-07-14 v2.1.208 (01:10 UTC, 45 changes): screen reader mode ( --ax-screen-reader orCLAUDE_AX_SCREEN_READER=1 ),vimInsertModeRemaps setting for two-key insert sequences,CLAUDE_CODE_PROCESS_WRAPPER env var for corporate launcher support, mouse-click on multi-select menus in fullscreen; fixes: fast mode not auto-restoring after model switches, background agent replies lost on delivery failure, context window resetting after CLI auto-updates, HTTP/2 in supervised/background sessions, Bedrock streaming and OAuth MCP server bugs; paginated 200+ row markdown tables to prevent memory issues. v2.1.209 hotfix (06:36 UTC) unblocked/model and other dialogs that were incorrectly blocked in background sessions after the v2.1.208 change. GitHub releases
- 
2026-07-13 Anthropic extended the 50% Claude Code weekly rate-limit boost to July 19, tied to the Fable 5 subscription extension announced this morning. The boost was originally set to expire today at 6 PM PT. After July 19, limits revert to standard subscription caps. BleepingComputer
- 
2026-07-08 v2.1.205: auto mode now blocks transcript-tampering rules; improved agent view adds classifier-written state headlines and colored state words; /doctor is a full setup checkup (alias/checkup ); auto-update downloads stream to disk saving ~400 MB peak RAM; fixed Windows worktree NTFS junction deletion bug; fixed session-to-PR linking for Bash calls exceeding 30K inline limit. v2.1.204 (same day): fixed hook events not streaming during SessionStart hooks in headless sessions. GitHub releases
- 
2026-07-07 Claude Cowork expanded to web and mobile; sessions and files saved to the user's account and accessible cross-device; remote-hosted sessions in beta; rolling out on Max plan first. TechCrunch · 9to5Mac
- 
2026-07-07 Claude Code and Claude Cowork in public beta on Claude for Government Desktop; FedRAMP High authorized environment, conversation history stored locally on agency devices, hard per-agency spend caps. Anthropic
- 
2026-07-06 v2.1.202: new "Dynamic workflow size" setting in /config (small/medium/large agent counts); adds workflow.run_id and workflow.name OpenTelemetry attributes to telemetry from workflow-spawned agents. GitHub releases
- 
2026-07-05 v2.1.199-201 (July 2-3): Three patch releases. v2.1.199 adds stacked slash-skill invocation (up to 5 per command), fixes SSL cert errors to fail fast with hints, and fixes streaming responses silently discarded on mid-stream overloaded errors. v2.1.200 changes AskUserQuestion to no longer auto-continue by default, formalizes "Manual" as the default permission mode label across CLI, VS Code, and JetBrains, and fixes background sessions going dark after sleep/wake. v2.1.201 patches Sonnet 5 sessions that were incorrectly using the mid-conversation system role for harness reminders. GitHub releases
- 
2026-07-03 Claude Enterprise adds admin analytics (usage/cost by group and user), model-level entitlements (set per-role default and allowed models), and spend alerts at 75%/90% of org limits — first wave of controls targeting agentic cost visibility. Anthropic blog
- 
2026-07-03 Steganography patch confirmed: v2.1.197 (released July 1) removes the Unicode-swap tracking mechanism with no entry in the official changelog. The Register
- 
2026-07-03 Researchers decompiled Claude Code v2.1.196 and found it steganographically alters the date string in its system prompt (Unicode apostrophe swap, hyphen-to-slash date separator) when ANTHROPIC_BASE_URL matches a base64+XOR(91) keyword list of Chinese AI lab domains including deepseek, moonshot, minimax, zhipu, baichuan, dashscope, volces. Suspected anti-distillation measure; community split on whether it's legitimate defense or malware-adjacent behavior for a dev tool. Hit 350+ points on HN in hours. thereallo.dev · HN
- 
2026-07-02 Claude Code session overhaul: completed agent rows no longer flip between "Done" and "Needs your input"; stalled agents labeled "Needs attention"; results mentioning a PR now show clickable links. Background sessions survive process restarts on all platforms including Windows. Org default models, readable session names, clickable file attachments also added. /code-review workflow token usage reduced ~25% by merging redundant finders. Releasebot
- 
2026-07-01 Claude in Chrome went generally available: background notifications, draft PR handoff, improved failover, and browser tools now load in a single batched call instead of one per tool. Claude Code changelog
- 
2026-07-01 Claude Fable 5 access restored on Claude Code; new safety classifier blocks the jailbreak in 99%+ of cases. Coding requests that trigger a false positive now fall back to Opus 4.8 with user notification. Usage capped at 50% of weekly limit through July 7. The Hacker News
- 
2026-06-30 Claude Sonnet 5 is the new default model in Claude Code (replaces Sonnet 4.6). 1M token context, near-Opus-4.8 quality, $2/$10 per Mtok intro pricing through Aug 31. Note: new tokenizer produces ~30% more tokens than Sonnet 4.6 on the same input. Anthropic
- 
2026-06-29 Claude Managed Agents now support operator-controlled sandboxes: agents can run in your own infrastructure or managed providers (Cloudflare, Daytona, Modal, Vercel) connected to private MCP servers. Also launched: Trusted Devices for Remote Control on Team/Enterprise plans, Claude-in-Slack for Enterprise (tag Claude in threads to delegate tasks), and Compliance API integrations for IT governance. Anthropic newsroom
- 
2026-06-22-26 Week 26 digest: claude mcp login authenticates a configured MCP server from the shell without the interactive/mcp menu; shell mode now explains command output inline (e.g.! npm test gets a follow-up without a second prompt);/rewind can resume a session from before a/clear ; background subagents surface permission prompts to the main session instead of auto-denying. What's New
- 
2026-06-24 Anthropic investigating social media claims that Claude returned responses meant for other users during the June 23 outage. No confirmed data exposure as of this evening; Anthropic says it takes any such report "very seriously." Cybernews
- 
2026-06-23 Fiona Fung (engineering head, Claude Code and Cowork) said on Lenny's Podcast that heavy agentic use is making employees' work more solitary, prompting Anthropic to add team-building countermeasures. Fortune
- 
2026-06-23 Broad outage starting 14:19 UTC: elevated error rates on Claude Opus 4.8 across claude.ai, Claude Code, Claude Cowork, Claude Console, and the API. Root cause was sub-agents spawning infinite loops that consumed platform resources. Anthropic confirmed fix by 14:25 UTC and resolved by 16:44 UTC; over 8,000 Downdetector reports in the US. TechRadar · TechTimes
- 
2026-06-21 Dynamic Workflows landed in research preview: Claude plans a task dynamically, writes an orchestration script, and fans it out across hundreds of parallel subagents with validation before folding results back; available on Max, Team, and eligible Enterprise plus Bedrock/Vertex/Foundry. Token usage can be substantial. Claude blog · InfoQ
- 
2026-06-20 v2.1.185: stream-stall hint message changed to "Waiting for API response · will retry in..." and now fires after 20 seconds instead of 10. GitHub releases
- 
2026-06-19 v2.1.183: auto mode now blocks destructive git commands when the user did not ask to discard local work. Changelog
- 
2026-06-18 Anthropic opened Seoul office (third Asia-Pacific location); NAVER deployed Claude Code across its full engineering org, Samsung SDS across Samsung Electronics, with LG CNS, Nexon, and Hanwha also signing on. Separately, Anthropic signed a MOU with Korea's Ministry of Science and ICT on AI safety. Anthropic · Korea Times
- 
2026-06-15 v2.1.178: new Tool(param:value) permission syntax matches on tool input parameters with wildcard support (e.g. Agent(model:opus) to block Opus subagents); nested .claude/ directories resolve closest-wins for agents, workflows, and output-styles; improved auto-mode subagent classification before launch; /doctor layout overhaul; fixes for JetBrains terminal flicker and Chrome connection drops. Changelog
- 
2026-06-15 Agent SDK billing split is now live: claude -p , Claude Code GitHub Actions, and third-party agents draw from a separate monthly credit ($20 Pro / $100 Max 5x / $200 Max 20x) at full API rates, no rollover. Interactive terminal sessions unaffected. The New Stack
- 
2026-06-12 v2.1.176: session titles now generate in the conversation language (configurable via language setting),footerLinksRegexes for regex-matched link badges, improved Bedrock credential caching (cached untilExpiration vs fixed 1 hour), and a large batch of Remote Control and background-agent bug fixes including disconnect notifications, session model switching, and backgrounded-session "Working" spinner stalls. Changelog
- 
2026-06-12 v2.1.175 adds enforceAvailableModels managed setting: when enabled, theavailableModels allowlist constrains the Default model and user/project settings can no longer widen a managed list — MDM-style model lockdown for enterprises. GitHub
- 
2026-06-12 v2.1.174 adds usage attribution breakdown (cache misses, long context, subagents) to the Account & usage dialog; fixes enterprise Fable 5 credits banner, Bedrock GovCloud region handling, and background sessions inheriting provider env variables. GitHub
- 
2026-06-10 v2.1.172 enables nested sub-agents: sub-agents can now spawn their own sub-agents up to 5 levels deep; Bedrock now reads AWS region from ~/.aws config whenAWS_REGION isn't set; added search bar for marketplace plugin browsing. GitHub
- 
2026-06-11 Code with Claude: Extended Tokyo ran today — a second-day overflow event for indie devs and early-stage founders with hands-on workshops on managed agents, memory, evals, and multi-agent composition; recordings to be posted. Event page
- 
2026-06-06 Enterprise cost crisis surfacing loudly. Microsoft cutting Claude Code licenses for its Experiences + Devices division by June 30, routing to Copilot. Uber burned through its entire 2026 AI coding budget by April, with heavy users hitting $500-$2,000/month. (opentools.ai)
- 
2026-05-28 Claude Opus 4.8 released, now leads the Artificial Analysis Intelligence Index at 61.4, with SWE-bench Pro at 69.2%.
- 
2026-06-08 Billing split lands June 15: Agent SDK, claude -p , and Claude Code GitHub Actions move off subscription limits into a separate monthly credit pool ($20 Pro / $100 Max 5x / $200 Max 20x) at full API rates, no rollover. Interactive Claude Code in the terminal is unaffected. TechTimes · Codersera
- 
2026-06-06 v2.1.166-168: adds fallbackModel setting so agents recover when the primary model is unavailable, glob support in deny-rule tool-name positions, hardened cross-session message security, andMAX_THINKING_TOKENS=0 to suppress extended thinking. Two follow-on patch releases the same day. GitHub releases
- 
2026-06-06 Auto mode now on Bedrock, Vertex, and Foundry for Opus 4.7/4.8; dynamic-workflow trigger renamed to "ultracode"; plugins in .claude/skills/ load automatically without a marketplace. Releasebot
