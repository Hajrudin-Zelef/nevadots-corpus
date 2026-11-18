# Step 2 — AI Tools & Platforms (Feb–Sep 2026)
## Consolidated research report (English) — collected September 22, 2026

**Project:** RAG data-collection, Step 2 (Outils & plateformes IA)
**Coverage date:** February → September 22, 2026
**Consolidation:** September 22, 2026
**Status:** Research snapshot. Prices are dated snapshots; re-verify against official pricing pages before use. Star counts move weekly.

### Scope
- Coding agents & AI IDEs: Claude Code, OpenCode, OpenClaw, Cursor, VS Code + GitHub Copilot, ZCode, Grok Build, DeepSeek Harness
- Distribution platforms: OpenRouter, Hugging Face
- Other notable developer tools: GitHub Spec Kit, Google Antigravity 2.0, AWS Kiro, Cline × LG CNS, Huawei Cloud CodeArts Agent, Augment Code Cosmos

### Provenance legend
- **[official]** — vendor blog, docs, changelog, repository, or regulatory filing owned by the tool's maker.
- **[vendor-reported]** — figures claimed by the vendor (pricing, user counts, performance) without independent audit.
- **[independent]** — reputable third-party press/analysis (VentureBeat, SiliconANGLE, The Register, TechCrunch, Bloomberg, NYT, WSJ, MacRumors, etc.).
- **[secondary]** — community sources, docs mirrors, trackers, aggregator blogs; useful but unverified.
- **[unverified]** — single-source or conflicting claims; treat as uncertain.

### Cross-cutting notes for RAG indexing
- **2026 = the year of the agent harness.** Products converged on the same patterns: parallel subagents, plan/approval modes, git-worktree isolation, `AGENTS.md` project memory, MCP/plugin extensibility, remote/mobile steering, and headless/CI modes.
- **Spec-driven development (SDD)** displaced "vibe coding" as the dominant workflow narrative from mid-2026 (GitHub Spec Kit, AWS Kiro, CodeArts Agent).
- **Naming correction:** no official xAI product named "Grok Code" was found. The verified xAI product is **Grok Build** (CLI + model `grok-build-0.1`). "Grok Code" appears only informally in community writing.
- **"Jev"** (from the step-1 roadmap) was previously identified as a proprietary decision model from US-based TypeSafe AI — not open-weight, not Chinese — and does not appear in this step.
- Multiple large M&A moves hit this space in 2026 (see §10–11): Stripe→OpenRouter (announced Aug 19), NVIDIA→Hugging Face (announced Sep 3), plus a **single-source, unverified** claim of a SpaceXAI→Cursor acquisition. All pending acquisitions are regulatory-uncertain.

---

## 1. Claude Code (Anthropic)

### 1.1 Overview
Terminal-first agentic coding tool by Anthropic, originally released February 2025 alongside Claude Sonnet 3.7 **[independent — VentureBeat]**. Evolved from a Node.js CLI into a platform shipping as native Rust-compiled binaries, with a plugin marketplace, agent teams, and cloud/mobile remote-control surfaces **[secondary — toknow.ai, May 2026]**.

### 1.2 Latest versions (as of September 2026)
- **2.1.277** — September 18, 2026 (latest changelog entry found) **[official — generated from https://github.com/anthropics/claude-code CHANGELOG.md]**.
- **2.1.224** — August 7, 2026: cross-session messaging on macOS/Linux via two new tools, `ListAgents` and `SendMessage`; messages stay local on the same machine (never reach Anthropic servers); cross-machine messaging only as replies through Remote Control; also shipped `claude self-hosted-runner` for Team/Enterprise plans **[independent — MacRumors, 2026-08-08]**.
- **2.1.0** — infrastructure release: hooks for agents/skills/slash commands (`PreToolUse`, `PostToolUse`, `Stop`), hot reload for skills in `~/.claude/skills`, forked sub-agent context (`context: fork`), wildcard tool permissions (e.g. `Bash(npm *)`) **[independent — VentureBeat]**.
- June 2026 train (2026.6): nested sub-agents up to 3 levels deep, `fallbackModel` configuration (up to three fallback models), community tool marketplace, usage attribution with per-agent/per-task cost breakdowns, scoped permissions, streaming agent logs (beta), agent checkpointing (beta), inline cost budgets, custom agent templates, multi-repo orchestration (beta) **[secondary — SitePoint, June 2026]**.
- September 2026 entries: AGENTS.md support (2.1.277 — in projects without CLAUDE.md, Claude Code reads AGENTS.md instead); `claude plugin eval` (2.1.269); `/output-style` command (2.1.269); `CLAUDE_CODE_WORKFLOW_MAX_CONCURRENT_AGENTS` up to 256 (2.1.269); per-command `allowed_domains` in auto mode with sandboxing (2.1.271); fast mode in Remote sessions (2.1.271); gateway hint headers for LLM gateways (2.1.273) **[official — CHANGELOG via community mirrors]**.

### 1.3 Features & architecture
- **Agent runtime:** terminal CLI + IDE extensions + Remote Control via claude.ai/code; subagents, Agent Teams ("Claude Cowork"), background agents, plans mode (with "Ultraplan" in newer builds), auto mode classifier (billing docs at code.claude.com), `/rewind` (undo agent runs), `/plan`, `/btw`, vim mode, voice mode (reported 20 languages), Playwright MCP / computer use, DESIGN.md support, MEMORY.md persistent memory **[secondary — toknow.ai]**.
- **Integrations:** MCP servers (with mid-session disconnect notifications and reconnection), first-party GitHub Action (runs the same runtime in CI), skills in `.claude/skills/` with `SKILL.md` frontmatter, plugin system with marketplace, OpenTelemetry metrics, Bedrock / Vertex AI / Foundry / LLM-gateway support (Claude apps gateways, model discovery timeout config), sandbox mode on Linux/macOS, managed settings (`requiredMinimumVersion`/`requiredMaximumVersion`, `allowManagedMcpServersOnly`, `deniedMcpServers`) **[official — CHANGELOG; secondary — SitePoint]**.
- **Models:** deep integration with the Claude Opus 4.x / Sonnet 4.x family; 1M-token context window in beta on newer Claude models (reported for Opus 4.6/Sonnet 4.6) **[secondary]**. Mid-2026 references cite Claude Opus 4.5 as flagship and Opus 5 as the current flagship API model **[independent/secondary]**.

### 1.4 Pricing (snapshot — re-check https://claude.com/pricing before budgeting)

| Plan | Price | Claude Code included? |
|---|---|---|
| Free | $0 | No |
| Pro | $20/mo ($17/mo annual) | Yes — base usage tier |
| Max 5x | $100/mo | Yes — 5× Pro usage/session |
| Max 20x | $200/mo | Yes — 20× Pro usage/session |
| Team standard | $25/seat/mo ($20 annual, 2-seat min) | Yes — ~1.25× Pro |
| Team premium | $125/seat/mo ($100 annual) | Yes — ~6.25× Pro, plus Claude Cowork |
| Enterprise | $20/seat + usage at API rates | Yes |
| API (Console) | Pay per token | Works via API key |

API rates (per MTok, Aug–Sep 2026): Claude Opus 5 $5 in / $25 out / $0.50 cache-read; Claude Sonnet 5 $3/$15/$0.30 (raised from $2/$10 on Sep 1, 2026); Claude Haiku 4.5 $1/$5/$0.10; Claude Fable 5 $10/$50/$1.00 **[secondary — morphllm.com, verified Sep 19, 2026]**.
**Notable event:** ~April 21, 2026 Anthropic briefly tested removing Claude Code from the Pro plan for ~2% of new signups, then restored it; the reason cited was flat-rate plan economics breaking under long-running agentic sessions **[secondary — devtoolpicks.com, Sep 21, 2026]**.
**Policy:** since April 2026, Anthropic explicitly blocks subscription tokens from being used by third-party CLI tools and runners — use a real API key for anything outside the official Claude Code client **[secondary]**.

### 1.5 Availability & adoption
- Available wherever Claude subscriptions/API are sold; GitHub repo `anthropics/claude-code` (public, 52 listed contributors; internal team larger) **[secondary]**.
- Enterprise deal reported: Allianz making Claude Code available to all employees with custom agents (one of Anthropic's first major enterprise deals of 2026) **[independent — Medium/Cogni Down Under citing Jan 2026 timing — treat as secondary]**.
- License: not stated in collected sources — **not confirmed as open source; assume proprietary**.

### 1.6 Key sources
- https://venturebeat.com/orchestration/claude-code-2-1-0-arrives-with-smoother-workflows-and-smarter-agents
- https://www.macrumors.com/2026/08/08/claude-code-adds-cross-session-messaging/
- https://www.sitepoint.com/claude-code-june-2026-10-new-features-devs-need-to-know/
- https://www.morphllm.com/claude-code-pricing
- https://github.com/anthropics/claude-code (CHANGELOG source)
- https://devtoolpicks.com/blog/anthropic-claude-code-pro-plan-removed-2026

---

## 2. OpenCode (SST / anomalyco)

### 2.1 Overview
Terminal-first, model-agnostic, **open-source (MIT)** AI coding agent — one of the most-starred developer tools of 2026 **[independent/secondary — gcn.com, nerdleveltech.com]**.

### 2.2 Latest releases
- **v1.18.29** — current release tag as of September 5, 2026 **[secondary — GitHub commit pin reference]**.
- v1.18.18 in-tree on August 14, 2026; v1.18.8 stable referenced ~July 2026; v1.17.10 dated June 24, 2026 **[secondary — kaidera-ai research, datacamp.com]**.
- A **V2 beta** dual-track is mentioned in community config docs (September 2026) **[secondary — unverified]**.
- **Repository moved from `sst/opencode` to `anomalyco/opencode`** (docs and package fields updated) **[official-via-mirror — research doc citing repo state Aug 14, 2026]**.

### 2.3 Architecture
- **Headless server + TUI client**, shipped as a single binary / npm package (`opencode`); terminal-first design with desktop app in beta **[official-via-mirror]**.
- **OpenTUI**: native Zig terminal-UI core with TypeScript bindings and a SolidJS reconciler (`anomalyco/opentui`, ~13k stars, MIT) **[official-via-mirror]**.
- **Model-agnostic:** 75+ LLM providers via the models.dev registry; per-project config in `opencode.json` (or `~/.opencode/config.json`); `/model` switches models mid-session; supports Ollama/local models; in January 2026, after Anthropic blocked third-party tools from using consumer Claude subscriptions through unofficial channels, OpenCode added other subscription options and its own gateway **[secondary — datacamp.com]**.
- **Agents:** build / plan / general agent types; **parallel agent execution** (June 2026) — multiple agents working on different parts of the same codebase simultaneously **[secondary — synapse-news]**.
- **Project memory:** plain-file `AGENTS.md` (OpenCode's equivalent of CLAUDE.md), generated via `/init` **[secondary — devtoollab.com]**.
- **Compiler-aware:** integrates the Language Server Protocol so the agent is grounded in live diagnostics — cited as a differentiator vs. paid rivals **[independent — gcn.com]**.
- **LSP + local execution** make it the default pick for teams that cannot/will not ship code to a vendor cloud **[independent — gcn.com]**.

### 2.4 Pricing & the Zen / Go gateways
- **The tool itself is free (MIT).** Spend is model usage: pay-as-you-go API tokens, or existing subscriptions (Copilot, ChatGPT Plus/Pro) OpenCode can authenticate against **[secondary — nerdleveltech.com]**.
- **OpenCode Zen** — the team's curated, tested model gateway at `https://opencode.ai/zen` (`/v1/chat/completions`, `/v1/responses` endpoints), OpenAI-compatible, single API key (`OPENCODE_API_KEY`). Pay-as-you-go credits (auto-reload); sold at cost with no markup, plus processing fees; includes rotating **free models** (e.g. `deepseek-v4-flash-free`, `mimo-v2.5-free`, `qwen3.6-plus-free`, `minimax-m3-free`, `nemotron-3-ultra-free`, `north-mini-code-free`, stealth `big-pickle`). Paid models include DeepSeek V4, GLM-5.x, Kimi K2.5/K2.6, MiniMax M2.x, Grok Build. Teams/spend caps/BYO OpenAI/Anthropic keys supported **[official-via-mirror — docs.docker.com; secondary — dysonharness research, July 27, 2026]**.
- **OpenCode Go** — fixed-price subscription: **$5 first month, then $10/mo**, with dollar caps (~$12/5h, $30/week, $60/month); **open-weight coding models only**; hosted in US, EU, Singapore; optional "use balance" falls back to Zen credits **[secondary — dysonharness, July 27, 2026]**.
- Free-tier note: free models are promotional and can disappear; free/stealth models may have data used for training **[secondary — itsfree.ai]**.

### 2.5 Adoption
- GitHub stars: **~197k (Aug 14) → 201.4k (Aug 26) → 208.2k (early Sep)** on star-ranking trackers **[secondary]**; ~25k forks, ~900 contributors, ~5.2k open issues **[secondary — GitHub API via kaidera-ai research]**.
- Claimed **7.5M+ monthly developers** **[vendor-reported/secondary — gcn.com, nerdleveltech.com]** — **unverified; treat as marketing**.
- Android companion app ("OpenCode: AI Coding Agent") by third party VIBE TECHNOLOGIES, LLC (~7.6k downloads, 4.67★, updated Aug 14, 2026) **[secondary — AppBrain]**.
- Community forks exist (e.g. "Gorilla OpenCode" fork emphasizing token efficiency) **[secondary — GitHub]**.

### 2.6 License & availability
- **MIT**, TypeScript; install via `curl -fsSL https://opencode.ai/install | bash` or Homebrew (`opencode-ai/tap/opencode`); npm; desktop app beta at opencode.ai/download **[official-via-mirror]**.

### 2.7 Key sources
- https://github.com/anomalyco/opencode (official repo)
- https://opencode.ai/docs/zen/ (official docs)
- https://docs.docker.com/ai/docker-agent/providers/opencode-zen/
- https://gcn.com/7-5-over-million-developers-now/20116/
- https://www.datacamp.com/blog/what-is-opencode
- http://nerdleveltech.com/opencode-open-source-ai-coding-agent-explained

---

## 3. OpenClaw (OpenClaw Foundation — verify)

### 3.1 Overview
**Open-source (MIT), self-hosted personal AI agent** — a "shared agent control plane" rather than a coding-only tool. Formerly known as **Clawdbot / Moltbot** **[secondary — milvus-io community blog]**. Tagline: "Your own personal AI assistant. Any OS. Any platform." One architecture doc attributes the project to Peter Steinberger and notes a move to an open-source foundation **[secondary — skyclaw docs, March 2026; unverified]**.

### 3.2 Latest releases
- **2026.9.5** — npm `latest` tag, September 19, 2026: **atomic updates** (Gateway keeps running while preparing the next version; rolls back to last working config on failure), plugin hot reload, conversation sharing (read-only access for teammates), expanded GPT Live, shared browser pages, conversation archiving, specialist-agent setup. 4,179 PRs, 64 direct commits, 502–503 contributing accounts **[independent — MarkTechPost, 2026-09-19; AgentRiot]**.
- **2026.8.1 ("OpenClaw 2.0")** — August 30, 2026: major security overhaul — masked credential prompts (secret values never enter chat transcript or model context), explicit per-session permission modes, file access tied to recorded workspace/worktree, team roles, shared credential store, protected network access limiting secret use to approved hosts, clearer approval screens; plus conversation search, paired devices + cloud workers, durable session progress, interactive dashboards, rebuilt browser Control UI, shared multiplayer sessions. Built by 933 contributors across 16,000+ PRs **[independent — CyberPress; Analytics Insight]**.

### 3.3 Architecture
- **Hub-and-spoke:** central **Gateway** (Node.js/TypeScript) routes tasks to LLM backends, executes skills (file ops, shell, browser automation, API calls), and returns structured results **[secondary — architecture docs]**.
- **Channels:** one Gateway serves WhatsApp, Telegram, Discord, Slack, iMessage, Signal, Matrix, Microsoft Teams, Google Chat, Zalo, and more via channel plugins (~30 total) **[official-via-mirror — openclaw-android-assistant docs]**.
- **Surfaces:** web Control UI (browser dashboard), CLI, native macOS app, iOS/Android nodes, Apple Watch companion (v2026.2.19) **[independent/secondary]**.
- **Memory/state:** conversations, long-term memory, and skills stored as plain Markdown/YAML under the workspace and `~/.openclaw`; heartbeat daemon (`systemd`/`LaunchAgent`, default every 30 min, hourly with Anthropic OAuth) drives autonomous checks via `HEARTBEAT.md`; cron jobs, webhooks, subagents, cloud workers **[secondary — milvus-io blog]**.
- **Models:** any cloud provider (Anthropic, OpenAI, Google) or local via Ollama/LM Studio/OpenAI-compatible servers — fully local inference possible **[secondary — milvus-io blog]**.
- **ClawHub:** plugin/skill registry; 2.0 shows security audit info before install, requires force flag for arbitrary executable sources; plugin SDK with migration requirements **[independent — Analytics Insight; secondary — 2.0 release notes]**.
- **Security history:** v2026.2.17/2.19 patched a real-world infostealer targeting OpenClaw config files (documented by Hudson Rock), plus credential-theft path OC-09, config traversal, and session permission bugs **[secondary — YouTube breakdown citing Hudson Rock]**.

### 3.4 Pricing, licensing, availability
- **Free; self-hosted only** — no first-party paid hosted service. Only cost is your own model API keys. Requires Node 24.16+ or 26.1+ (Node 26 recommended) **[official-via-mirror; independent — Composio comparison]**.
- **License: MIT** (core Gateway; "OpenClaw Foundation" named as steward in one source) **[secondary — aleksandr-bogdanov/imprnt, citing openclaw.ai, Aug 26, 2026]**.
- Install: one-liner `curl -fsSL https://openclaw.ai/install.sh | bash` or npm; native macOS ZIP/DMG signed+notarized for 2026.8.1; Android via Google Play **[secondary — release notes mirror]**.

### 3.5 Adoption
- **~387.6k–389.5k GitHub stars** (Aug–Sep 2026 trackers) **[secondary]**; Composio (Sep 2026) reports ~389.5k stars vs. Hermes Agent ~244.7k — Hermes leads on OpenRouter token volume instead **[independent — Composio]**.
- Compared by Composio (Sep 2026) as the "shared agent control plane" vs. Hermes Agent's "persistent, self-improving agent runtime"; OpenClaw rated stronger on human collaboration, teams/shared infrastructure; sandboxing off by default (vs. Hermes' smart approvals) **[independent — Composio]**.

### 3.6 Key sources
- https://www.marktechpost.com/2026/09/19/openclaw-releases-2026-9-5/
- https://agentriot.com/news/ai-agents/openclaw-2026-9-5-atomic-updates
- https://cyberpress.org/openclaw-2-0-released-with-enhanced-ai-agent-security/
- https://www.analyticsinsight.net/artificial-intelligence/openclaw-explained-how-its-viral-ai-agent-platform-is-being-built-and-secured
- https://composio.dev/content/openclaw-vs-hermes-agent
- https://github.com/openclaw/openclaw (official repo)

---
## 4. Cursor (Anysphere)

### 4.1 Overview
AI-native code editor (VS Code fork) by **Anysphere**. In 2026 it pivoted to an agent-first product **[independent — The Decoder; blazetrends]**.

### 4.2 Latest versions
- **3.21.13** — released September 18, 2026 (per community download trackers; **secondary — unverified vs. official changelog**).
- 3.20.21 (Sep 13, 2026); 3.18.25 (Sep 1, 2026); 3.18.9 (Aug 28, 2026) **[secondary — cursor-download trackers]**.
- **Cursor 3.0** — launched April 2, 2026: interface rebuilt from scratch around agents; new "Agents Window" (`Cmd+Shift+P → Agents Window`); parallel agents across repo boundaries; unified sidebar for local and cloud agents; drag cloud↔local session handoff; integrated browser for agents; new diff view with staging/commit/PR management; plugin marketplace (MCPs, skills, subagents); traditional IDE layout still available as an option **[independent — The Decoder, April 2026; blazetrends]**.

### 4.3 Features (2026)
- **Composer 2** (March 2026): Cursor's own RL-trained coding model; frontier-level performance at ~4× generation speed; most interactive turns under 30 seconds **[secondary — rapidevelopers.com citing Cursor]**.
- **Automations:** always-on agents triggered by schedules or Slack/Linear/GitHub/PagerDuty/webhooks; agents spin up cloud sandboxes with configured MCPs/models **[secondary — rapidevelopers.com]**.
- **Agent Skills** (2.4+): structured knowledge/workflows via skill files **[secondary — igmguru]**.
- **JetBrains IDE support** via the Agent Client Protocol (IntelliJ, PyCharm, WebStorm) **[secondary — rapidevelopers.com]**.
- 30+ marketplace plugins (Atlassian, Datadog, GitLab, Glean, Hugging Face, monday.com, PlanetScale, …) **[secondary — rapidevelopers.com]**.
- **Cursor CLI** with its own changelog (Aug 11, 2026): steer running turns via Enter, subagent transcripts, Explore subagent model choice, sticky skills/custom modes, durable goals (`/goal`) **[official-via-mirror — CLI changelog mirror]**.
- Rules migration: Cursor 3.11 (July 10, 2026) splits `.cursorrules`/`.mdc` rules into AGENTS.md + skills + subagents + plugins; nested `AGENTS.md` supported **[secondary — start-debugging blog]**.
- Bugbot: AI code-review add-on for GitHub PRs (separate pricing) **[secondary — aivexify]**.

### 4.4 Pricing (snapshot — note conflicting reports)

| Plan | Price | Reported allowance |
|---|---|---|
| Hobby (free) | $0 | ~2,000 completions, 50 slow premium requests; 7-day Pro trial |
| Pro | $20/mo ($16/mo annual) | Credit pool: older docs say 500 fast requests/mo; 2026 credit-system docs say ~$20 credit pool (≈225 fast requests), unlimited slow, unlimited completions |
| Pro+ | $60/mo | 3× Pro credits |
| Ultra | $200/mo | 20× Pro credits, max throughput |
| Teams | $40/user/mo ($32 annual) | Pro credits/seat + admin controls; premium seats $120 reported |
| Enterprise | Custom | SSO, audit logs, advanced admin |
| Bugbot add-on | $40/user/mo | Unlimited reviews on up to 200 PRs |

- **Credit system:** since June 2025, Cursor bills dollar-denominated credits; one standard request ≈ $0.04 baseline, but models consume credits at different rates (Claude Opus burns faster than GPT-5.x Mini); overage charges apply beyond the pool in Auto mode **[secondary — aitooldiscovery.com, Aug 2026]**.
- **⚠ Discrepancy:** older guides (incl. 2026 H1) quote "500 fast requests/month" for Pro; newer credit-based guides quote a $20 credit pool (~225 fast requests). Present both as a snapshot conflict **[secondary]**.

### 4.5 Adoption & company
- Widely adopted flagship AI editor; r/cursor community 182k+ members **[secondary — aitooldiscovery.com]**.
- Competitive pressure: community reporting that OpenCode is displacing Cursor among cost-sensitive developers **[secondary — synapse-news]**; funding/valuation figures not confirmed in this research — **not reported here**.
- **Unverified claim:** an AI release-tracker summary of xAI's Grok 4.7 launch states SpaceXAI "bought [the] coding company [Cursor] earlier in the year" and that Grok 4.7 received supplemental training on anonymized Cursor workflow data — **no official acquisition announcement located; must be verified** (see §13).

### 4.6 License & availability
- **Proprietary** (closed-source client); available for Windows, macOS, Linux **[secondary]**.

### 4.7 Key sources
- https://the-decoder.com/new-cursor-3-ditches-the-classic-ide-layout-for-an-agent-first-interface-built-around-parallel-ai-fleets/
- https://blazetrends.com/cursor-3-0-ditches-vs-code-roots-for-full-ai-agent-dashboard/
- https://www.aitooldiscovery.com/guides/cursor-ai-pricing
- https://aivexify.com/cursor-ai-pricing-in-2026/
- https://github.com/oslook/cursor-ai-downloads (version tracker, secondary)

---

## 5. VS Code AI features / GitHub Copilot (Microsoft & GitHub)

### 5.1 Copilot modes in VS Code (2026)
Three distinct modes: **Ask** (conversational, no edits), **Edit** (targeted diffs), **Agent** (autonomous: plans, edits files, runs terminal commands, observes output, iterates) **[secondary — dev.to guide]**.

### 5.2 Recent release highlights (official GitHub Changelog)
- **March 2026 (VS Code 1.100):** faster agent edits, automatic follow-up fixes when new diagnostics appear, apply-patch editing format for GPT-4.1/o4-mini, Anthropic replace-string tool for Claude Sonnet 3.7/3.5, `#githubRepo` code-search tool, smarter prompt caching, conversation summarization **[independent — AIntelligenceHub]**.
- **April–May 2026 (v1.116–v1.119):** semantic indexing in all workspaces; experimental `/chronicle` (query own chat history from a local DB); deferred tool loading; **BYOK extends to Copilot Business/Enterprise**; group policies for agent-reachable domains; Copilot CLI sessions monitorable/steerable remotely from GitHub.com or mobile app **[official — GitHub Changelog via RSS mirror]**.
- **June 2026 (v1.123–v1.127):** **browser tools GA (July 1)** — agents browse/click through live apps, enabled by default for paid Copilot; **parallel agent sessions**; **1M-token context windows** with Anthropic and OpenAI models; session sync across machines; credit-visibility features **[independent — TechTimes]**.
- **July 2026 (v1.127–v1.131):** Agents window (public preview) redesign; multi-chat support for Claude (multiple chats per session, each with own history/title/model); fork into peer chat; **worktrees with any harness** (Copilot, Claude, or Codex sessions in isolated Git worktrees); subagent tracking (model, elapsed time, active tool call); PR updates from chat (failed CI, review comments) **[official — https://github.blog/changelog/2026-07-30-github-copilot-in-visual-studio-code-july-2026-releases/]**.
- **August 2026 (v1.132–v1.135):** side-by-side chats; `/btw` side-conversation sharing primary context/prompt cache; prompt timeline; **Agent Plugins 1.0 standard** (portable agent plugins across VS Code and compatible clients); open Agents window without GitHub sign-in (experimental, Claude API key); switch model providers mid Claude session (Anthropic ↔ Copilot subscription); continue external agent sessions; experimental `/rubber-duck` second-opinion command **[official — https://github.blog/changelog/2026-08-31-github-copilot-in-vs-code-august-2026-releases/]**.

### 5.3 Copilot pricing — GitHub AI Credits (since June 1, 2026)
GitHub replaced premium requests with **GitHub AI Credits (1 credit = $0.01)** billed on actual token usage (input/output/cached) at per-model API rates. Seat prices unchanged **[independent — Second Talent, Sep 11, 2026; Guickly, Aug 2026]**.

| Plan | Price | Included AI credits |
|---|---|---|
| Free | $0 | Limited (2,000 completions) |
| Pro | $10/mo | $15 ($10 base + $5 flex) |
| Pro+ | $39/mo | $70 ($39 base + $31 flex); premium models incl. Opus |
| Max | $100/mo | $200 ($100 base + $100 flex) |
| Business | $19/seat/mo | $19 pooled across org |
| Enterprise | $39/seat/mo | $39 pooled across org |

- Code completions + Next Edit Suggestions remain **unlimited on paid plans and consume no credits**; chat, agent mode, code review, cloud agent, CLI draw credits. Business/Enterprise credits pool org-wide; no rollover; admin spend caps **[independent — Second Talent]**.
- Copilot code review also consumes GitHub Actions minutes (two billing surfaces) **[secondary — future-stack-reviews]**.
- **Adoption signal:** in the April–June 2026 quarter (first month of usage-based billing), Copilot revenue grew **>60% QoQ** **[independent — Second Talent, citing quarter]**.

### 5.4 Key sources
- https://github.blog/changelog/2026-08-31-github-copilot-in-vs-code-august-2026-releases/
- https://github.blog/changelog/2026-07-30-github-copilot-in-visual-studio-code-july-2026-releases/
- https://Www.techtimes.com/articles/319982/20260709/github-closes-agentic-loop-vs-code-browser-tools-go-ga-sessions-run-parallel.htm
- https://www.secondtalent.com/resources/github-copilot-statistics/
- https://www.guickly.com/blogs/github-copilot-pricing-2026

---

## 6. ZCode (Z.ai / Zhipu AI)

### 6.1 What it is (and is not)
- **ZCode is the desktop "Agentic Development Environment" of Z.ai** (Beijing-based lab, formerly Zhipu AI), launched **July 2, 2026** as the native environment for its GLM coding models **[independent — VentureBeat]**.
- **Not a model** — the application layer around GLM-5.2 (analogous to Claude Code vs. Claude Opus): desktop interface, long-horizon task structure, workspace management, remote-control surfaces **[secondary — tokenkarma]**.
- An **Electron desktop app** (macOS, Windows, Linux) where the agent conversation is the interface center, surrounded by file manager, terminal, Git panel, and live browser preview **[secondary — pondero.ai]**.
- Tuned for **long-horizon "Goal" tasks**: user describes an outcome; the agent plans, edits, runs checks, iterates; sensitive commands require confirmation **[secondary — pondero.ai; community — ai-agent-map]**.
- **Remote steering:** monitor/direct a running session from **WeChat, Feishu, or Telegram** on a phone **[independent — VentureBeat; secondary — pondero.ai]**.
- **BYOK** for third-party models (Claude Code, Codex, Gemini reported); one community source also lists OpenCode as a supported agent **[secondary — ai-agent-map]**.
- **Proprietary client**; the GLM model line is separately open-weighted (GLM-5.2 described as MIT-licensed by one source) **[secondary — pondero.ai]**.

### 6.2 Pricing
- **Free to download**; revenue via GLM Coding Plan subscriptions: **Lite $16.20/mo**, **Max $144/mo** — undercutting Cursor ($20/$200) **[secondary — tokenkarma; pondero.ai]**.
- Free tier exists as a **promotional offer** (not committed permanent) **[secondary — tokenkarma]**.
- Launch promo: **1.5× usage-quota bonus** for Coding Plan subscribers through **July 31, 2026**; off-peak token consumption at **0.67×** coefficient **[secondary — cryptonwz/VentureBeat-derived]**.
- GLM-5.2 (unveiled June 13–16, 2026): 1M-token lossless context; Z.ai claims inference costs ~1/5 cheaper than comparable competitors **[vendor-reported]**.
- Noted in industry coverage as a pricing disruption even as real-world robustness remained unproven (WindowsForum relay, July 2026) **[secondary]**.

### 6.3 Version & uncertainties
- Community agent map lists **current version 3.11.2** and describes it as tuned around **GLM-5.3** — but launch coverage (July 2026) centers on **GLM-5.2**; **version/model drift is unverified — flag as uncertain** **[secondary — ai-agent-map]**.
- Gartner's ~$10B agentic-coding market estimate quoted in launch coverage **[independent — VentureBeat, citing Gartner]**.
- No adoption/user figures confirmed in this research.

### 6.4 Key sources
- https://venturebeat.com/technology/z-ai-launches-zcode-to-challenge-cursor-claude-code-and-github-copilot-in-ai-coding
- https://tokenkarma.app/blog/zcode-ai-coding-assistant-cursor-alternative-2026/
- https://pondero.ai/news/2026-07-04-zcode-launch/
- https://github.com/weijt606/ai-agent-map/blob/HEAD/agents/zcode.md

---
## 7. Grok Build / xAI developer tooling (SpaceXAI)

### 7.1 xAI API — current model catalog and pricing (official)
Source: SpaceXAI official API pricing page, crawled within hours of this report (`docs.x.ai/developers/pricing`) — **[official]**. Prices in USD per 1M tokens. Models with long-context pricing bill the long-context rates for **all** tokens in a request once the prompt reaches the model's long-context threshold (≥ 200K tokens). Requests to the US regional endpoint run inference in the United States and are billed at 1.1× the global token rates (10% premium).

| Model | Context | Input | Cached input | Output | Long-context (in / cached / out) |
|---|---|---|---|---|---|
| grok-4.7 | 500K | $2.00 | $0.50 | $6.00 | $4.00 / $1.00 / $12.00 |
| grok-build-0.1 | 256K | $1.00 | $0.20 | $2.00 | $2.00 / $0.40 / $4.00 |
| grok-4.6 | 500K | $2.00 | $0.50 | $6.00 | $4.00 / $1.00 / $12.00 |
| grok-4.5 | 500K | $2.00 | $0.30 | $6.00 | $4.00 / $0.60 / $12.00 |
| grok-4.3 | 1M | $1.25 | $0.20 | $2.50 | $2.50 / $0.40 / $5.00 |
| grok-4.20-multi-agent-0309 | 1M | $1.25 | $0.20 | $2.50 | $2.50 / $0.40 / $5.00 |
| grok-4.20-0309-reasoning | 1M | $1.25 | $0.20 | $2.50 | $2.50 / $0.40 / $5.00 |
| grok-4.20-0309-non-reasoning | 1M | $1.25 | $0.20 | $2.50 | $2.50 / $0.40 / $5.00 |

**API characteristics** **[official]**/**[vendor-reported]**:
- OpenAI-compatible REST endpoint (`https://api.x.ai/v1`); drop-in migration from OpenAI/Anthropic SDKs by swapping base URL and key.
- Function/tool calling, structured outputs, vision (image input), image generation (reported ~$0.07/image — **[secondary/unverified]**), real-time web and X search.
- Third-party routing (OpenRouter) was observed billing Grok 4.7 at $1.60/$4.80 (standard tier) — route prices can sit below list price **[secondary/unverified]**.

### 7.2 Grok 4.x release timeline (developer-relevant)
- **Grok 4.3** — opened to all API developers **May 6, 2026** (beta since April 17 behind SuperGrok Heavy). Pricing cut at launch: output $15.00 → $2.50 (-83%), input $3.00 → $1.25 (-58%). 1M-token context, native video input (up to 5 min: mp4/mov/webm), document generation (PDF, XLSX, PPTX). Five legacy models retired May 15, 2026 **[secondary/unverified]** (reported by Awesome Agents).
- **Grok 4.20** — API general availability reported **March 10, 2026** (three dated SKUs: `-0309-reasoning`, `-0309-non-reasoning`, `-multi-agent-0309`) **[secondary/unverified]** (reported by Venice model datasheet; no official xAI launch post located).
- **Grok 4.5** — public API **July 9, 2026**, after private testing inside SpaceX/Tesla engineering teams **[secondary/unverified]** (AI Cost Estimator). Positioned as flagship coding/agentic model at $2/$6.
- **Grok 4.6** — released **August 12, 2026**, announced by the official @SpaceXAI account **[vendor-reported]** (relay by pasqualepillitteri.it). Not a new base training run: reuses a 1.5-trillion-parameter "V9" base (per the same relay — **unverified**), improved via supervised fine-tuning + reinforcement learning; emphasis on long-horizon agents, autonomous coding, self-verification. Day-one availability in **Cursor**, **Grok Build**, **Grok Bot** agents, and the API; 2× usage included in Cursor and Grok Build for the first week.
- **Grok 4.7** — released **September 21, 2026** (one day before this report), announced by @SpaceXAI **[vendor-reported]** (relays: Metaverse Post, Unite.AI, The Decoder, XenoSpectrum). Key facts:
  - New, larger base than 4.6; extended RL run on harder, multi-hour tasks; pretraining data cutoff June 2026, supplemental data as late as August 2026. Parameter count not disclosed in the model card; a "2.1 trillion" figure attributed to Musk's pre-launch remarks is **unconfirmed** **[secondary/unverified]** (AI Release Tracker).
  - Vendor-reported benchmarks: CursorBench 4.0 46.3% (vs 40.4% for 4.6; vs GPT-5.6 Sol 41.7%), DeepSWE v1.1 high-effort 71.0% (vs 65.2% for 4.6; vs GPT-5.6 Sol 72.7%). Independent Artificial Analysis numbers: Intelligence Index v4.3.2 = 46 (vs 53 for Claude Fable 5.1 and GPT-6); Terminal-Bench 4.0 agentic coding = 26% (vs 60% GPT-6 Astra, 55% Claude Fable 5.1) **[independent]** (via The Decoder).
  - New safeguard stack: 3.3% pass-through of risky dual-use prompts on internal HackerBench v0.3; tops LatchBio's biosafety benchmark at 62.4% **[vendor-reported]**; invite-only red-team access for select cybersecurity partners.
  - Pricing unchanged at $2/$6; a **fast variant** offers 2× output speed at 2× price ($4/$12). Free access inside Grok Build at x.ai/build **[vendor-reported]**/**[secondary]**.
  - Distribution at launch: xAI API, Grok Build, Cursor (all plan tiers), Grok Bot harness, third-party harnesses, routers/cloud platforms; phased rollout to **GitHub Copilot** (Pro, Pro+, Max, Business, Enterprise) began the same day **[secondary/unverified]** (XenoSpectrum).

### 7.3 Grok Build — xAI's agentic coding CLI
- **Launch:** early beta **May 14–15, 2026**, initially restricted to **SuperGrok Heavy** subscribers (reported $300/month) **[vendor-reported]** (relays: AlternativeTo, fonearena, Crypto Briefing). Windows PowerShell installer released **May 25, 2026**.
- **Install commands** (as reported): macOS/Linux `curl -fsSL https://x.ai/cli/install.sh | bash`; Windows `irm https://x.ai/cli/install.ps1 | iex`.
- **Features** **[vendor-reported]**: interactive, mouse-capable fullscreen TUI; plan mode (AI proposes a plan, user approves/comments/rewrites before execution); clean diff tracking; parallel subagents with deep git-worktree integration; headless mode (`-p` flag) for scripts, bots, and CI pipelines; respects existing repo conventions out of the box (`AGENTS.md`, plugins, hooks, skills, MCP servers); **full ACP (Agent Client Protocol) support** for building custom bots and agent orchestration apps (ACP is the open agent-client protocol originated by Zed); built-in `/feedback` command.
- **Model:** powered by `grok-build-0.1`, a purpose-built agentic coding model (256K context, 100+ tok/s reported throughput). Public API beta **May 28, 2026** (no subscription required) at $1/$2 per 1M tokens **[secondary/unverified]** (GitHub issue tracker relay; consistent with the official pricing table above).
- **Availability evolution:** early beta → SuperGrok Heavy only; Grok 4.6 (Aug 12) and Grok 4.7 (Sep 21) both shipped day-one inside Grok Build; Grok 4.7 free inside Grok Build per xAI's launch messaging.

### 7.4 Grok inside OpenCode (integration, May 21, 2026)
On **May 21, 2026**, xAI rolled out an integration letting Grok subscribers use xAI models directly inside **OpenCode** (open-source, provider-agnostic terminal coding agent; reported 160K+ GitHub stars, 75+ providers supported) — no separate API key required; an existing SuperGrok or X Premium subscription acts as the credential **[secondary/unverified]** (Memeburn). Strategic note: the move followed Anthropic's January 2026 blocking of third-party tools from Claude Pro/Max subscriptions, which had pushed developers toward model-agnostic harnesses.

### 7.5 Voice and retrieval APIs
- **Grok Text-to-Speech API** — launched **March 16, 2026** **[vendor-reported]** (relay: BaseNor, citing @xai). 5 voices (Ara, Eve, Leo, Rex, Sal), 20+ languages with auto-detection + BCP-47 codes, inline expressive speech tags (pauses, laughter, whispers, emphasis); output formats MP3, WAV, PCM (Linear16), G.711 μ-law/A-law.
- **Voice Agent API** — launched late 2025 (realtime voice apps) **[secondary/unverified]**.
- **Grok Collections** — RAG/document-retrieval building block **[secondary/unverified]** (mentioned in a January 2026 Medium overview; no official docs page located — **verify before final consolidation**).

### 7.6 Subscriptions and access tiers
- **SuperGrok Heavy** — reported at **$300/month**; was the gate for Grok 4.3 beta, Grok Build early beta, and Grok 4.20-era "multi-agent" usage **[secondary/unverified]** (multiple relays; consistent across sources).
- Standard **SuperGrok** (~$30/mo class) and **X Premium+** also serve as credentials for integrations such as Grok-in-OpenCode **[secondary/unverified]**.

### 7.7 Corporate/brand context relevant to tooling
- xAI now announces products via the **@SpaceXAI** account, the name used after the **xAI–SpaceX merger** earlier in 2026 **[vendor-reported]**/**[secondary]**.
- **Grok Bot** — referenced in official launch messaging as the conversational-agent harness Grok 4.6/4.7 natively understand; no standalone product page located **[vendor-reported]** (details thin — verify).

### 7.8 Key sources
- https://docs.x.ai/developers/pricing (official pricing)
- https://mpost.io/xai-ships-grok-4-7-with-new-safeguard-stack-independent-benchmarks-confirm-gains-flag-doubled-token-consumption/
- https://www.unite.ai/spacexai-releases-grok-4-7-for-coding-and-knowledge-work/
- https://the-decoder.com/xai-launches-grok-4-7-at-bargain-prices-but-benchmarks-reveal-a-wide-gap-to-claude-and-gpt-6/
- https://xenospectrum.com/en/xai-grok-4-7-pricing-copilot/
- https://aireleasetracker.com/model/xai/grok-4.7
- https://awesomeagents.ai/news/xai-grok-4-3-api-launch/
- https://ai-cost-estimator.com/blog/grok-4-5-public-launch-spacexai-vs-claude-opus-gpt-5-6-cost-per-task
- https://pasqualepillitteri.it/en/news/10915/grok-4-6-released-cursor-grok-build-api
- https://cryptobriefing.com/xai-grok-cli-windows-powershell/
- https://www.fonearena.com/blog/482869/xai-grok-build-coding-agent-features.html
- https://alternativeto.net/news/2026/5/xai-launches-grok-build-in-beta-a-powerful-coding-agent-and-cli-for-advanced-engineering/
- https://memeburn.com/how-to-use-grok-in-opencode-full-setup-guide-with-xai-api/
- https://www.basenor.com/blogs/news/xai-launches-grok-text-to-speech-api-5-voices-20-languages

---

## 8. DeepSeek Harness (`dsh`)

### 8.1 What it is
**DeepSeek Harness** is an **open-source (MIT) agent harness** (not a model): the engineering layer that turns a language model into a working agent — context management, tool use, file read/write, terminal execution, session logging, permissions, observability. DeepSeek's framing, widely echoed: **Agent = Model + Harness** — the model reasons, the harness does everything else.
- **Released:** **August 13, 2026**, announced by the official @deepseek_ai account on X **[vendor-reported]** (relays: pasqualepillitteri.it, Memeburn, Medium/Generative AI). Published as the GitHub repository `deepseek-harness` under the DeepSeek AI org — **repo URL itself [secondary/unverified], verify**.
- **Version status:** v0.1 **Developer Preview** — DeepSeek warns that compatibility-breaking changes should be expected.
- **Language:** TypeScript monorepo (web UI, headless runner, sandbox/execution layers, plugin framework), with unusually direct architecture documentation **[secondary/unverified]** (Medium/Generative AI, Sep 2026).
- Before launch it was known informally as **"DeepSeek Code"** (Memeburn).
- **Positioning:** the direct open-source challenger to Anthropic's Claude Code and OpenAI's Codex (both closed-source) — the open MIT license is the strategic point.

### 8.2 Architecture
- **"Everything is a plugin":** the model adapter, tool registry, session log, and the agent loop itself are all swappable plugins — deployers can replace models, storage, security policies, tools, or the entire agent loop without touching the core framework.
- **Cordis kernel:** powered by the **Cordis meta-framework** (third-party engine, `cordiverse/cordis` on GitHub); design described in the paper *"A Programming Paradigm for Spatiotemporal Composability"* (arXiv:2608.25512) **[secondary/unverified]** (attribution via Medium explainer — verify the arXiv ID).
- **Session modes (four):** Standard (full agent: files, shell, search, subagents, workflows), Code/PTC (the model writes one typed program instead of many tool round-trips), Minimal, Creator (the agent drafts new presets from conversation) **[secondary/unverified]**.
- **Session log:** every run is recorded in an **append-only session log** that can be resumed, forked, and replayed — repeatedly cited as the standout design idea.

### 8.3 Interfaces and model support
- **Local web UI:** `npx @deepseek-ai/dsh web` → serves at `http://127.0.0.1:3080` (npm package `@deepseek-ai/dsh`, terminal command `dsh`).
- **Headless CLI** for single tasks (CI/automation-friendly) and a **Python SDK**.
- **Model-agnostic:** works with DeepSeek's own API (default model reported as `deepseek-v4-flash`), Anthropic, OpenAI, Google Vertex, Azure, Amazon Bedrock, any OpenAI-compatible endpoint, and local Ollama models — provider is a replaceable plugin, not the system's identity **[secondary/unverified]** (community setup guides).

### 8.4 Adoption (date-stamped — figures move weekly)

| Date | Reported scale | Source class |
|---|---|---|
| Aug 13, 2026 (launch day) | ~27K–50K stars within hours | secondary |
| ~Aug 14 | 70K+ stars within a day | secondary |
| ~Aug 16–17 | ~95K–135K stars within 2–4 days; ~8.8K forks at day 4 | secondary |
| Early Sep 2026 | 208K+ stars, ~24.2K forks | secondary |
| Sep 16, 2026 | 225K+ stars | secondary |

Community plugin counts are reported between "a few thousand catalogued" and "6,000+ repos" depending on the counter and date **[secondary/unverified]**.
One analysis notes the harness performs best with DeepSeek's own models and flags plugin-system and contribution-governance concerns **[secondary/unverified]** (Geeky Gadgets).

### 8.5 Caveats and open questions
- **Preview status:** release candidates only; DeepSeek explicitly promises breaking changes. Not production-hardened.
- **To verify before final consolidation:** exact GitHub repo URL, the arXiv paper ID, the `deepseek-v4-flash` default-model claim, and the four session-mode names — all currently **[secondary/unverified]**.

### 8.6 Key sources
- https://pasqualepillitteri.it/en/news/11027/deepseek-harness-mit-claude-code-rival
- https://memeburn.com/deepseek-v4-pro-launches-with-major-agent-upgrades-and-open-source-harness/
- https://www.geeky-gadgets.com/deepseek-harness-local-ai/
- https://generativeai.pub/i-argued-the-harness-was-the-product-then-deepseek-open-sourced-theirs-d092f8b3dcde
- https://medium.com/@axotopia/the-machine-wears-the-harness-now-you-just-talk-0a951e02fd88

---
## 9. OpenRouter

### 9.1 Company overview (2026)
- Founded 2023 by Alex Atallah and Louis Vichy. Team ~90 people as of August 2026. **[independent — SiliconANGLE, Aug 19 2026]**
- Unified, OpenAI-compatible API gateway aggregating 400–500+ models from 80+ inference providers behind a single endpoint. **[official — catalog/API docs; independent]**.
- OpenRouter.ai catalog exposes per-model pricing, supported parameters, context length, top provider data, architecture/modality info, `huggingFaceId`, creation date, knowledge cutoff, and supported voices. **[official — API docs/OpenAPI]**

### 9.2 Funding milestones — 2026
**Series B — May 26, 2026 ($113M, ~$1.3B post-money)**
- $113M Series B led by **CapitalG** (Alphabet's independent growth fund). **[official — openrouter.ai/announcements/series-b; independent — TechCrunch, SiliconANGLE]**
- Participants: NVentures (NVIDIA's venture arm), ServiceNow Ventures, MongoDB Ventures, Snowflake Ventures, Databricks Ventures, plus existing investors Andreessen Horowitz and Menlo Ventures. **[independent — SiliconANGLE]**
- Valuation ~$1.3B post-money, per The New York Times (company did not disclose). Up from ~$547M post-money after the $40M Series A in June 2025 (PitchBook). **[independent — TechCrunch, NYT]**
- Series A (June 2025, $40M) was led by a16z and Menlo Ventures with Sequoia participation. **[independent — TechCrunch]**
- Reported scale at Series B: ~25 trillion tokens/week (≈100T/month), 5× growth in six months; 8M+ global users. **[official/company-reported — Series B announcement; independent]**
- Company quote (CEO Alex Atallah): "Running inference at scale is fundamentally a multimodel problem. The era of picking a single model is over." **[independent — SiliconANGLE]**

**Stripe acquisition — announced August 19, 2026 (pending close)**
- Stripe announced a definitive agreement to acquire OpenRouter. Neither company disclosed terms; closing expected within weeks of the announcement. **[independent — SiliconANGLE, Payments Dive]**
- Reported valuations disagree: NYT ~$7.5B ($1.5B to founders, $6B to investors); Axios >$8B (mostly stock); Bloomberg >$7B; earlier WSJ reporting said talks opened nearer $10B. **[independent — NYT/Axios/Bloomberg/WSJ via SiliconANGLE]**
- Scale reported at acquisition: >10M developers/companies, >10T tokens/day (>200T/month), 400+ models, 80+ providers. **[independent — SiliconANGLE; secondary]**
- Revenue: ~$50M annualized in March 2026 (per ainvest analysis), up from ~$19M at end-2025; one source (cryptobriefing, Aug 2026) put annualized revenue at ~$140M in July 2026 growing 29% MoM. ⚠️ These figures conflict; treat both as secondary estimates, not company disclosures. **[secondary]**
- Customers cited: NVIDIA, Zoom, Lovable. **[independent — SiliconANGLE]**
- Stripe rationale: CEO Patrick Collison called tokens "the central currency" of AI companies; OpenRouter to sit in Stripe's agentic-commerce/payments stack for AI cost optimization and routing. **[independent — Payments Dive, SiliconANGLE]**
- TechCrunch reported Stripe outbid Databricks for the deal. **[independent — TechCrunch via zubnet.ai]** ⚠️ Second-hand attribution; mark as press-reported.

### 9.3 API & product features (changelog, 2026)
Sourced from the official API changelog (`openrouterteam/docs`, changelog.mdx — generated from OpenAPI diffs, human-reviewed for breaking changes) **[official]**:

| Date | Change |
|---|---|
| Aug 19, 2026 | BYOK API: manage credential restrictions (`allowed_models`, `allowed_user_ids`, `allowed_api_key_hashes`) via create/update |
| Jul 29, 2026 | `GET /benchmarks` updated; `UnifiedBenchmarksORItem` schema added |
| Jul 28, 2026 | Breaking (no action needed): `analyst_model` replaces `judge_model` in Fusion streaming events; `POST /responses` OpenAPI tag renamed `Responses`; Web search `blocked_domains`; guardrail creation gains `enable_free_model_publication` / `enable_free_model_training` / `enable_paid_model_training`; auto-router plugins gain `cost_tier` |
| Jul 27, 2026 | `POST /embeddings` request schema updated; `Quantization` and `ReasoningFormat` enums extended |
| Jul 25, 2026 | **Responses API GA** (was `beta.responses`; old SDK namespace kept as deprecated alias); provider options add `claude-on-aws`; BYOK provider enum extended; `voyageai` provider option (Jul 24) |
| Jul 24, 2026 | Anthropic Messages tool blocks widened to support MCP tools (`mcp_tool_reference`, `mcp_toolset_reference`); encrypted compaction content support |
| Jul 8, 2026 | Image streaming chunk events (`POST /images`) |
| Jul 7, 2026 | `GET /models` gains filters: `max_age_days`, `max_agentic_index`, `max_coding_index`, `max_intelligence_index`, `max_output_price`, `max_tool_success_rate` |

Broader API surface (all **[official]** via OpenAPI):
- OpenAI Chat Completions-compatible, Anthropic Messages passthrough, Responses API, presets, Files API, embeddings, rerank, images, audio (speech + transcriptions), video, benchmarks, guardrails, ZDR endpoints.
- Audio: `POST /audio/speech` (19 TTS models verified in July 2026 community test: Fish Audio, MiniMax, Deepgram Aura-2, Gemini 3.1 Flash TTS, Grok, MAI-Voice-2, Qwen, Kokoro, Orpheus, CSM, Zonos, Voxtral) with per-model `supported_voices` arrays. **[secondary — community-verified, not official]**
- Model-routing aliases: `openrouter/free` (auto-routed free models, guaranteed $0) and `openrouter/auto` (best-available, may cost). **[secondary — verified against docs by community]**

### 9.4 Model catalog additions (mid-2026)
Notable new catalog entries observed Aug 2026 **[secondary — community-compiled from live API, prices per 1M tokens]**:

| OpenRouter ID | Catalog created | Context | Input/output price |
|---|---|---|---|
| `anthropic/claude-sonnet-5` | 2026-06-30 | 1M | $2.00 / $10.00 |
| `deepseek/deepseek-v4-flash-0731` | 2026-07-31 | ~1.3M | $0.14 / $0.28 |
| `deepseek/deepseek-v4-pro-0813` | 2026-08-12 | ~1M | $0.66 / $1.98 base (UTC time-band overrides up to $1.32/$3.96 reported) |
| `x-ai/grok-4.6` | 2026-08-12 | 500K | $2.00 / $6.00 ($4.00/$12.00 over 200K prompts) |

Catalog also carries per-model `supported_parameters`, `supported_voices`, quantizations, and tags; endpoints expose provider, quantization, and pricing. **[official]**

### 9.5 Pricing model
- OpenRouter takes ~5–5.5% platform fee on inference spend routed through it (company-reported figures cited in press analyses); credits are non-refundable and do not expire. **[secondary — press analyses; exact current fee terms should be verified on openrouter.ai/docs]**
- "About 5% of the inference spending that runs through the platform stays with the company." **[independent — SiliconANGLE]**
- Pricing is per-model/per-provider with transparent catalog pricing; free `:free` model variants are offered by upstream providers but availability is unstable and controlled by those providers. **[official catalog + secondary]**
- 90%+ gross margin claimed (no own inference infra) — per ainvest analysis. **[secondary]** ⚠️ Analyst estimate, not company disclosure.

### 9.6 Usage trends (2026)
- Weekly token throughput: ~10B (Jan 2024) → 25T (May 2026) → ~90T (mid-2026), i.e., ~9,000× growth since Jan 2024. **[company-reported via press — cryptobriefing]**
- **Agents dominate**: daily agent token consumption 14× between Feb–Aug 2026 (0.51T → 7.3T/day); agents ≈71% of all token consumption by early Aug 2026; human usage grew only 2.8× in the same window. **[company-reported — cryptobriefing]**
- **Chinese models flip**: Chinese open models went from ~2% of OpenRouter token usage (2025) to >50% (mid-2026); US frontier models fell from ~70% to ~30%. Google's models ≈38% of usage in 2025 study; Anthropic 17%; OpenAI 4.4%. **[company-reported — OpenRouter "State of AI" study via ainvest; secondary framing]**
- Company publishes a "State of AI" study tracking ecosystem-wide token share (~13% of weekly ecosystem token volume captured in 2025). **[company-reported — ainvest]**

### 9.7 Key sources
- TechCrunch (Series B): https://techcrunch.com/2026/05/26/openrouter-more-than-doubles-valuation-to-1-3b-in-a-year/
- SiliconANGLE (Series B): https://siliconangle.com/2026/05/26/openrouter-raises-113m-bring-order-enterprise-ai-inference-routing/
- SiliconANGLE (Stripe deal): https://siliconangle.com/2026/08/19/stripe-buys-ai-model-router-openrouter-in-reported-7-5b-deal/
- Payments Dive (Stripe deal): https://www.paymentsdive.com/news/stripe-openrouter-finally-strike-a-deal/828326/
- API changelog (GitHub): https://github.com/openrouterteam/docs/blob/HEAD/changelog.mdx
- API base URL: https://openrouter.ai/api/v1

---

## 10. Hugging Face

### 10.1 Platform scale (2026)
- 18M+ developers, researchers, and creators; 200,000+ companies; 3M+ models; 500,000 datasets; 1M+ applications (Spaces). **[official — NVIDIA blog citing HF figures, Sept 3, 2026]**
- Growth Jan–Aug 2026: public model repos 2.43M → 2.96M (+21.5%); datasets 711K → 1M; Spaces 1.00M → 1.44M. Distribution highly skewed: ~85.6% of models have <200 lifetime downloads; 1.5% of repos account for 99.2% of downloads. **[official — HF "State of Open Models: Summer 2026", Aug 14, 2026]**
- Runtime layer growing fastest: repos declaring `gguf` +464%, `lerobot` +194%, `mlx` +148% vs +16% for `transformers`/`peft`. **[official — same report]**
- GGUF local-inference traffic: Qwen 39.6M GGUF downloads/month (vs Gemma 20.8M, Llama 7.5M); Qwen-based derivatives 151,448 (2.6× Meta's footprint). **[official — same report]**
- ~$150M annualized revenue (2026) — press-reported, not a company disclosure. **[secondary — cryptobriefing/Bloomberg Opinion]** ⚠️ estimate only.

### 10.2 NVIDIA acquisition — announced September 3, 2026 (pending close)
- NVIDIA signed a **definitive agreement on Sept 2, 2026** to acquire Hugging Face for **$12,930,300,000**; announced by CEO Jensen Huang in an NVIDIA blog post on Sept 3, 2026. Deal expected to close in **H1 2027**, subject to regulatory approvals and customary conditions. **[official — NVIDIA blog + SEC 8-K filing]**
- Structure: ~$11.9B payable to HF stockholders + up to ~$1.0B equity-based retention program for HF employees joining NVIDIA. **[official — 8-K]**
- Platform commitment: HF "will remain an open platform" — developers keep choosing their own models, frameworks, clouds, and compute providers; "NVIDIA compute will not be required"; multi-cloud and multi-accelerator support continues. **[official — NVIDIA blog]**
- HF's three co-founders (Clément Delangue, Julien Chaumond, Thomas Wolf) join NVIDIA with the whole team; per Bloomberg Billionaires Index each founder worth ~$1.8B. **[independent — Bloomberg]**
- Context: The Information, Bloomberg, and TechCrunch reported the deal under negotiation Aug 26–27, 2026 before confirmation. HF previously rejected a ~$500M NVIDIA investment (late 2025) that would have valued it at ~$7B. Multiple bidders were reported; NVIDIA moved after Delangue approached Huang directly. **[independent — TechCrunch/Bloomberg, via secondary]**
- This is NVIDIA's largest outright company acquisition (Mellanox: $6.9B, 2020). **[secondary — temperature2 editorial]**

### 10.3 Platform features & launches (June–September 2026)

**Hub & inference**
- **Baseten joins Inference Providers** (Aug 6, 2026): conversational/text-gen tasks at launch (Kimi K3, DeepSeek V4 Flash, GLM-5.2, etc.); more tasks to follow. Integration via `huggingface_hub` ≥1.26.1, `@huggingface/inference` JS, OpenAI-compatible router at `router.huggingface.co/v1` with `model:provider` syntax. Two billing modes: custom key (billed by provider) or routed-by-HF (billed on HF account, provider rates passed through with **no HF markup**). PRO users get $2/month Inference credits. **[official — https://huggingface.co/blog/baseten]**
- **Inference Providers in agent harnesses**: integrated with Pi, OpenCode, Hermes Agents, OpenClaw, etc. **[official — Baseten blog post]**
- **Agentic Resource Discovery launch** (June 17, 2026): let agents search Hub resources. **[official — https://huggingface.co/blog/agentic-resource-discovery-launch]**
- **Model pages show Every Eval Ever results** (June 30, 2026): community eval results featured on model pages. **[official — https://huggingface.co/blog/eee-community-evals]**
- **hf CLI redesigned as agent-optimized Hub interface** (June 4, 2026). **[official — https://huggingface.co/blog/hf-cli-for-agents]**
- **agent-usage dataset** (published July 2026): tracks `agent/<name>` tokens from coding agents calling the Hub — Claude Code led July with 44.4%, Codex rose 10.4%→20.8% Apr–Jul. **[official — State of Open Models report]**
- **Foundry Managed Compute** (Microsoft, July 7), **one-click to Amazon SageMaker Studio** (July 7), **zero-egress storage with SkyPilot** (July 7) — compute/storage integrations. **[official blog]**
- **Cerebras + Gemma 4 for real-time voice AI** (July 1). **[official blog]**
- **Funes** (Sept 3, 2026): "Give Your Coding Agents a Memory You Own". **[official blog]**

**Spaces & compute**
- **Spaces Dev Mode** (PRO): SSH/VS Code access into Spaces for fast iteration. **[official pricing/docs — via secondary guides; verify on huggingface.co/pricing]**
- **ZeroGPU**: shared GPU pool (RTX Pro 6000 Blackwell) giving free GPU-backed Spaces; PRO raises quota (third-party guides cite 25–40 min/day with over-quota credits). **[secondary]** ⚠️ exact quota varies across third-party guides; verify on official pricing page.
- **HF Jobs**: run training/jobs on HF infra — "Run a vLLM Server on HF Jobs in One Command" (June 26), migrate GitHub CI to HF Jobs (June 9), Async GRPO with LoRA across HF Jobs (Sept 10). **[official blog]**
- **Storage Buckets**: powering Papers with Code search + LeRobot/Strands Agents data loops (Aug 2026 posts). **[official blog]**
- **Gradio Workflow**: new workflow system posts Aug 25, Sept 10. **[official blog]**

**Browser / local AI**
- **@huggingface/kernels** (Sept 1, 2026): 207 WebGPU kernels as versioned Hub repos (Apache-2.0), JS loader, plus **Fleet** — in-browser crowdsourced GPU benchmarking. 2.57× geomean faster than ORT WebGPU on Apple M4 (vendor benchmark). Kernels page also covers CUDA/ROCm/Metal. **[official — https://huggingface.co/blog/webgpu-kernels]**
- **llama.cpp/ggml team joined Hugging Face** (Feb 2026); project stays open-source and community-governed. **Transformers now runs llama.cpp quants** (Sept 22, 2026). **[official — State of Open Models report; blog]**
- **tokenizers v1** (Sept 21, 2026). **Kernels major updates** (July 6). **[official blog]**
- **Jun Kim (oMLX creator) joins HF** to support the MLX community (Sept 22, 2026). **[official blog]**
- **oMLX, Nunchaku 4-bit diffusion in Diffusers** (July 23). **[official blog]**

**Ecosystem / research posts**
- **State of Open Models: Summer 2026** (Aug 14, 2026): flagship data report — Chinese labs lead the open frontier ceiling (754B–2.78T params/month vs US mostly <130B); Qwen became the community base model; licenses stay permissive (59% Apache-2.0 among Chinese >20B releases) though Kimi K3/Qwen 3.8 2.4T recently added non-commercial/revenue-share terms; small models take 83% of all-time downloads. **[official — https://huggingface.co/blog/state-of-open-models-summer-2026]**
- **UK AISI × EvalEval reproducible benchmarks** (Sept 22), **BenchMIRT** (AllenAI, Sept 1), **Open ASR Leaderboard global-south language** (Aug 28), **Real World VoiceEQ** (July 15), **FFASR leaderboard** (June 24). **[official blog]**
- **Meta Muse Glimmer** spotlight (Aug 10): local, agentic, multimodal, open source. **Thinking Machines Inkling** (July 15), **GLM-5.2** (Z.ai, June 17). **[official blog]**
- **LeRobot v0.6.0** (July 7); Strands Agents + LeRobot robotics posts (June 17, Aug 13). **Grabette** robot data (July 21). **[official blog]**

### 10.4 Security incident — July 2026
- Intrusion into production infra via dataset-processing pipeline (remote-code dataset loader + template injection); autonomous **AI agent swarm** ran thousands of actions across short-lived sandboxes over a weekend; limited internal datasets and service credentials accessed. **[official — https://huggingface.co/blog/security-incident-july-2026]**
- Response: vulnerabilities closed, nodes rebuilt, credentials rotated, law-enforcement report, outside forensic specialists engaged; no evidence of tampering with public models/datasets/Spaces; software supply chain verified clean. **[official]**
- Forensic analysis ran on **zai-org/GLM-5.2** (open-weight, on HF's own infra) because commercial hosted APIs' safety guardrails blocked incident-response payloads; HF notes the "asymmetry problem" for defenders. **[official — same post + technical timeline https://huggingface.co/blog/agent-intrusion-technical-timeline, July 27]**
- Press coverage linked the incident to an OpenAI model test. **[independent — AP via techxplore]** ⚠️ attribution details not confirmed by official HF statements.

### 10.5 Pricing (2026)
List prices from official pricing pages as compiled by third-party guides (all **[secondary]** — verify at huggingface.co/pricing before quoting):

| Product | Price |
|---|---|
| Hub Free | $0 — unlimited public repos, basic CPU Spaces |
| PRO | $9/user/mo — 8× ZeroGPU quota, $2/mo Inference Provider credits, 1 TB private storage, Spaces Dev Mode |
| Team | $20/user/mo — PRO org-wide, SSO, audit logs, 12 TB + 1 TB/seat storage |
| Enterprise Hub | $50/user/mo or custom — SSO, audit logs, BYO cloud, SLAs |
| Spaces hardware | $0–$23.50/hr depending on tier |
| Inference Endpoints (dedicated) | CPU from ~$0.033/hr; T4/L4 $0.50–0.80/hr; A10G/L40S $1.00–1.80/hr; A100/H100/H200 $2.50–10/hr; billed per minute, scale-to-zero |
| Inference Providers (serverless) | Provider rate passed through at cost — no HF markup |

Key pricing facts: endpoints billed per minute only while running; no HF markup on Inference Providers; credits included in subscriptions. **[secondary — forasoft.com, eesel.ai, metacto.com]**

### 10.6 Licenses
- Hub artifacts carry per-repo licenses; major open model families: Apache-2.0 (Qwen, DeepSeek, Z.ai/GLM), MIT (some Chinese releases), custom/community licenses (Llama, Gemma). Trend noted: most permissive at large scale; recent shift toward non-commercial/revenue-share terms on some very large models (Kimi K3, Qwen 3.8 2.4T). **[official — State of Open Models report]**
- Platform code: Transformers (Apache-2.0), Diffusers (Apache-2.0), Gradio (Apache-2.0), llama.cpp (MIT, community-governed). **[official — well-known project licenses]** ⚠️ verify per-repo at consolidation.
- NVIDIA deal commits to HF remaining open and multi-accelerator. **[official]**

### 10.7 Key sources
- Blog feed: https://huggingface.co/blog/feed.xml (official)
- State of Open Models Summer 2026: https://huggingface.co/blog/state-of-open-models-summer-2026
- Baseten Inference Providers: https://huggingface.co/blog/baseten
- Security incident: https://huggingface.co/blog/security-incident-july-2026 (+ technical timeline https://huggingface.co/blog/agent-intrusion-technical-timeline)
- WebGPU kernels: https://huggingface.co/blog/webgpu-kernels
- NVIDIA acquisition coverage: https://www.unite.ai/nvidia-signs-definitive-agreement-to-acquire-hugging-face-for-12-9b/ ; https://temperature2.com/p/2026-09-03-nvidia-confirms-hugging-face-acquisition/

---
## 11. Other notable AI developer tools / platforms (Feb–Sep 2026)

### 11.1 GitHub Spec Kit — late February 2026 **[secondary/unverified]**
Open-source (MIT) toolkit that structures AI-assisted development into phases, each producing artifacts that feed the next — the flagship of the **spec-driven development (SDD)** wave that displaced "vibe coding" by mid-2026. Reported **111K GitHub stars and 9.8K forks as of June 11, 2026**, described as the fastest-growing dev-tooling repo in recent memory (via the atomsbaza research compendium on GitHub).
- Source: https://github.com/atomsbaza/my-superpowers/blob/HEAD/docs/research/agentic-ai/2026-06-21-agentic-ai-coding-agent-trends.md

### 11.2 Google Antigravity 2.0 — May 19, 2026 (Google I/O) **[vendor-reported]**
Google's agent-first development platform, no longer an in-product experiment but a standalone production-grade offering. Four components shipped simultaneously: standalone **desktop app** (macOS/Windows/Linux) for multi-agent orchestration, **CLI**, **SDK** (programmatic access to the agent harness behind Google's consumer products; supports self-hosted deployment), and the **Gemini Enterprise Agent Platform** (managed sandboxed execution via the Gemini API). Powered by **Gemini 3.5 Flash** by default; native voice commands; integrations with Google AI Studio, Android Studio, Firebase. Positioned directly against Claude Code, Cursor, and OpenAI's Codex. Sources: Google Developers Blog, SiliconANGLE, MarkTechPost.
- Sources: https://github.com/archie0125/synapse-news/blob/HEAD/src/content/articles/2026-05-24-google-antigravity-20-agentic-dev-platform-en.md ; https://github.com/kovalovme/ai-news/blob/HEAD/news/2026-05-20/tools.md

### 11.3 AWS Kiro — June 2026 (AWS Summit New York) **[secondary/unverified]**
Agentic IDE powered by **Claude Sonnet + Amazon Nova via Bedrock**. Spec-driven: takes a prompt, generates a formal EARS-notation requirements doc with acceptance criteria, waits for human review, then generates code. (Via atomsbaza compendium — verify against AWS announcements before final consolidation.)
- Source: https://github.com/atomsbaza/my-superpowers/blob/HEAD/docs/research/agentic-ai/2026-06-21-agentic-ai-coding-agent-trends.md

### 11.4 Cline "Spec Driven for Enterprise" — June 8, 2026 **[secondary/unverified]**
**LG CNS × Cline** (US open-source AI coding-agent developer) partnership: agentic platform automating the full enterprise system lifecycle (requirements analysis, design, coding, QA) with per-stage specialized agents. Announced via PRNewswire relay (vir.com.vn).
- Source: https://vir.com.vn/lg-cns-and-cline-launch-agentic-ai-platform-for-enterprise-system-development-and-operations-154352.html

### 11.5 Z.ai ZCode — early July 2026 **[secondary/unverified]**
See §6 (full entry). Noted as a pricing disruption even as real-world robustness remained unproven (WindowsForum relay, July 2026).
- Source: https://windowsforum.com/windows-news.4/z-ai-zcode-launch-free-agentic-ai-coding-desktop-on-windows-macos-linux.433572/

### 11.6 Huawei Cloud CodeArts Agent — July 2026 (open beta, Thailand) **[vendor-reported]**
Launched at Huawei Cloud Summit Thailand 2026 as part of Huawei Cloud's **Agentic Infrastructure**: project-level code generation, completion, R&D knowledge queries, unit-test generation; "Agent Team" mode for multi-agent collaboration; specification-driven development workflow. The wider Agentic Infrastructure adds a UnifiedBus AI cluster service, petabyte-scale agentic memory storage, the AgentSphere runtime, and CCE VolcanoNext scheduling (TechNode/TNGlobal).
- Source: https://technode.global/2026/07/24/chinas-huawei-cloud-launches-agentic-ai-infrastructure-coding-agent-beta-in-thailand/

### 11.7 Augment Code Cosmos — mid-September 2026 **[secondary/unverified]**
**Augment Code** launched **Cosmos**, a team-level AI development platform: instead of agents per developer, a shared software-delivery surface where a validator agent checks specs against codebase and company practices, and corrections (e.g. via Slack) persist as shared cross-session memory for all agents and team members. Framing from the company: "2024 was chat, 2025 is agents, 2026 is agents for teams" (completeaitraining.com).
- Source: https://completeaitraining.com/news/augment-code-launches-cosmos-to-extend-agentic-ai-coding/

### 11.8 Items flagged but not verified (excluded pending confirmation)
- **Anthropic "Code with Claude" managed agents** (mid-May 2026) — referenced only via a news-aggregator slug; **no primary source located** — do not include without verification.
- **BMAD-METHOD** (open-source multi-agent spec-execution framework) — named in the SDD wave but not independently researched here.
- **Grok Bot** — official xAI harness name with no standalone documentation located.

---

## 12. Cross-tool comparison (September 2026 snapshot)

| Dimension | Claude Code | OpenCode | OpenClaw | Cursor | VS Code + Copilot | ZCode | Grok Build | DeepSeek Harness |
|---|---|---|---|---|---|---|---|---|
| Form factor | Terminal CLI + IDE ext. + remote | Terminal TUI (+ desktop beta) | Gateway + chat apps | AI-native IDE | Editor + agent mode | Desktop ADE (Electron) | Terminal CLI | Web UI + headless CLI + SDK |
| License | Proprietary (unconfirmed) | **MIT** | **MIT** | Proprietary | Proprietary | Proprietary client | Proprietary | **MIT** |
| Model lock-in | Claude only (official) | None (75+ providers) | None (any API/local) | Multi-model + Composer 2 | Multi-model (BYOK on Biz/Ent) | GLM-first, BYOK | Grok (xAI) | None (plugin-adapted) |
| Pricing entry | $20/mo | $0 + usage | $0 + own keys | $0 / $20/mo | $0 / $10/mo | $0 / $16.20/mo | SuperGrok Heavy early; 4.7 free in CLI | $0 (v0.1 preview) |
| Heavy tier | $200/mo | Zen PAYG / Go $10/mo | — | $200/mo | $100/mo | $144/mo | — | — |
| Standout 2026 feature | Cross-session messaging; plugin eval | Parallel agents; LSP grounding; Zen gateway | Atomic updates; masked credentials; 30 channels | Agent-first UI 3.0; cloud handoff | Browser tools GA; parallel sessions; AI credits | WeChat/Feishu remote steering; price | ACP support; parallel subagents in worktrees | Everything-is-a-plugin; append-only replayable session log |

**Platforms comparison:**

| Dimension | OpenRouter | Hugging Face |
|---|---|---|
| Role | Unified inference gateway (400+ models, 80+ providers) | Model/dataset/app hub (3M+ models, 1M+ Spaces) |
| Business model | ~5% platform fee on routed inference spend | Subscriptions (PRO $9 / Team $20 / Enterprise $50+) + usage billing; inference providers at cost |
| 2026 headline | Stripe acquisition announced Aug 19 (~$7.5B reported) | NVIDIA acquisition announced Sep 3 ($12.93B, close expected H1 2027) |
| Developer signal | Agents = 71% of token consumption (Aug 2026); Chinese open models >50% of usage | HF agent-usage dataset: Claude Code led July with 44.4% |

---

## 13. Open verification items (before final RAG consolidation)

### Coding agents & IDEs
1. Claude Code client license terms — verify from the official repo.
2. OpenCode "7.5M monthly developers" — vendor claim, unverified.
3. Cursor Pro allowance — "500 fast requests" (older docs) vs. "$20 credit pool ≈225 fast requests" (2026 credit docs); resolve against cursor.com/pricing.
4. ZCode current version (3.11.2?) and GLM-5.2 vs. GLM-5.3 tuning — verify at https://zcode.z.ai.
5. OpenClaw founder/foundation attribution — verify current stewardship.
6. Cursor funding/valuation and adoption numbers — not researched in this pass.
7. Copilot "Max" tier details and flex-credit policy changes — GitHub notes flex allotments may change; re-check the plans page.
8. DevToolPicks' claim of GitHub suspending new flat-rate Copilot signups — single secondary source; verify.
9. **SpaceXAI acquisition of Cursor** — single-source secondary claim; not confirmed by any official announcement; verify before final consolidation (belongs to step 3, labs/hyperscalers).

### Grok / DeepSeek Harness
10. DeepSeek Harness: exact GitHub repo URL, the arXiv paper ID (2608.25512), the `deepseek-v4-flash` default-model claim, and the four session-mode names — all currently **[secondary/unverified]**.
11. "Grok Code" as a product name is **unverified**; the real product is **Grok Build**.
12. xAI–SpaceX merger: no official announcement located; rests on secondary relays.
13. Grok 4.20 API GA (Mar 10, 2026): no official xAI launch post located.
14. Grok Collections (RAG building block): mentioned in one January 2026 overview; no official docs page located.
15. Grok Bot: official xAI harness name with no standalone documentation located.
16. API/consumer prices are snapshots dated September 21–22, 2026; xAI reprices frequently.

### Platforms
17. **OpenRouter current fee**: confirm the live platform-fee % and any post-acquisition Stripe pricing changes on openrouter.ai/docs/pricing. **[secondary only]**
18. **OpenRouter revenue figures**: $50M vs $140M annualized — conflicting secondary estimates; seek a company disclosure or reconcile dates.
19. **Stripe deal status**: closing expected "within weeks" of Aug 19 — check whether it has closed and whether terms were ever disclosed.
20. **NVIDIA/HF deal status**: definitive agreement signed; expected H1 2027 close pending regulators — track regulatory review progress.
21. **HF official pricing**: confirm PRO/Team/Enterprise prices and ZeroGPU quotas directly on huggingface.co/pricing (third-party guides disagree on quota minutes).
22. **Series B announcement URL**: https://openrouter.ai/announcements/series-b cited by community briefs; not directly fetched — verify.
23. **HF Inference Providers roster**: Baseten, Together AI, AWS, Google Cloud, Cerebras cited across sources; get the full current list from HF docs.

---

## 14. Collection metadata
- **Research waves:** Part A (coding agents & AI IDEs), Part B (OpenRouter & Hugging Face), Part C (Grok tooling, DeepSeek Harness, other tools).
- **Working files:** `etape2_partA.md`, `etape2_partB.md`, `etape2_partC.md` (kept alongside this consolidated file).
- **Priority sources used:** SiliconANGLE, VentureBeat, TechCrunch, Bloomberg, NYT, WSJ, The Register, MacRumors, official vendor blogs/repos/changelogs, Hugging Face blog, GitHub Changelog, SEC 8-K filings, community trackers.
- **Conventions kept:** per-item provenance tags; uncertainty flags; dates on every price, version, and star count; no direct comparison across different benchmark versions; vendor-reported vs independent numbers separated (e.g. Grok 4.7 CursorBench/DeepSWE vendor-reported vs Artificial Analysis independent).

*End of consolidated Step 2 report.*
