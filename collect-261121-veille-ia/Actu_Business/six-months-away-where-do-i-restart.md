---
id: collect-261121-veille-ia/actu-business/six-months-away-where-do-i-restart
title: "Six Months Away. Where Do I Restart?"
domain: actu-business
role: reference
task: article
actors: []
dates: ["2026-10-28"]
keywords: ["model", "models", "months", "away", "preview", "agents", "agent", "copilot"]
source: https://dev.to/htekdev/six-months-away-where-do-i-restart-20pp
source_anchor: ""
source_lines: [1, 91]
section: "Actu_Business"
---
It has been almost six months since I went on paternity leave. I'm slowly coming back, and I'm overwhelmed.

I was away from the developer-AI news stream while I looked after our twins and the rest of the family. (I'll tell the kitchen version of that in a companion article.) Now I'm looking at a changelog stream that doesn't fit in one sitting, and I need a map more than another feed.

So this is the map I wanted: what actually shipped between May and October 28, 2026, what you can do with it today, and where it's still preview, beta or plan-limited. Dated claims link to the vendor changelog or release notes they came from. Everything after October 28 is out of scope, and I haven't tried any of this hands-on. The exercises near the end are a plan, not a report.

Agents became a much more normal surface. You can now hand a model a long task in your editor, terminal or a vendor-hosted sandbox, and the products around it (permissions, plugins, cost controls, observability) matured. The other half of the story is migration: models were retired, endpoints changed shape, and new model versions broke old request parameters. If you only have an hour, the "do first" list at the end is built for you.

Agent workflows existed before May, so none of this is "prompting became agents." What changed is how much of the surrounding product shipped and stabilized.


*Headline items per month. Statuses are as of October 28, 2026; the table above has the sources.*

The practical shift: you can run, supervise and cost-check agent work from the editor, the terminal or the cloud, and pull in outside tools without hand-built setup.

- 
**Terminal:** the redesigned Copilot CLI interface adds tabs for issues, pull requests and gists, and an in-session`/mcp add` ,`/skills` and`/plugin` flow instead of hand-edited config files.
- 
**Plugins:** Agent Plugins 1.0 packages skills and MCP servers into one installable unit that works across compatible clients. GitHub says AWS, Anysphere, Microsoft, OpenAI and Vercel published it with Google joining as core maintainer.
- 
**Orchestration:** dynamic workflows are code-defined processes that mix scripted steps and agents, with checkpoints you can pause and resume. They're in public preview on all plans; in the CLI you need`--experimental` or`/experimental on` .
- 
**Permissions:** assisted permissions let a model classify each command, path or URL request. Clearly safe requests proceed; everything else still asks you. Admins can block Allow All while keeping Assisted.
- 
**Model choice:** Fusion routes each task across models at Standard, High or Max effort, and only uses models your org policy enables.
- 
**Connectors:** GitHub-verified connectors cover 26 services at launch (Slack, Figma, Notion, Sentry, Linear and others). They're public preview on all plans, but Business and Enterprise need an admin to enable the`Editor preview features` policy, add GitHub-verified servers to any MCP allowlist, and sometimes approve access on the service's side (Slack, for example).
- 
**Local models:** from CLI 1.0.94-0,`/model` can discover models from a running Ollama instance (Oct 7). Choosing one doesn't turn on offline mode by itself.
- 
**Plan note:** on Oct 28, GitHub Enterprise Cloud can include Copilot for customers working with a GitHub or Microsoft account team. That path has no monthly per-seat AI credits (usage is metered against the enterprise budget), and self-serve availability was still "coming in stages."

Cost is now part of the interface too. Since June 1, Copilot usage draws on AI Credits calculated from token consumption, while code completions and next edit suggestions stay included and don't use credits. For a longer cost walkthrough, see my Copilot billing and harness engineering article.

**OpenAI: a hosted agent API, and an endpoint that's gone.** On September 10, OpenAI released the Agents API in public beta: a managed Codex harness with durable sessions, your own tools and MCP servers, and OpenAI-hosted sandboxes (or your own). On September 29 it added computer use, where agents drive an OpenAI-hosted browser and your app handles sign-in and website approvals. Constraints: it's beta, sandbox compute is billed during active turns, and tool calling on GPT-6 Astra (released Sep 3) requires the Responses API. If you still call the Assistants API, it shut down on August 26; the migration target is Responses plus Conversations.

**Anthropic: computer use is stable, and new models reject old parameters.** On August 19, Anthropic took computer use out of beta as `computer_toolset_20260801` on the Claude API, and launched a browser use tool for a browser your application hosts (available on Google Cloud too from Aug 20). Agent Skills and the Files API left beta the same day. The catch: Opus 5.5 (Sep 22) and Sonnet 5.5 (Sep 28) return a 400 error for the older `computer_20251124` tool on the Claude API and Google Cloud, and for forced `tool_choice` types. Sonnet 4.5 is scheduled to retire on the Claude API on November 30, 2026, and Anthropic recommends Sonnet 5.5. For longer jobs, Claude Managed Agents gained dynamic workflows in beta on October 9, which needs the `managed-agents-2026-04-01` beta header.

**Google: live voice, video and an agent whose tools changed.** On September 15, Google made Gemini 3.8 Live and Live Extended Thinking GA for real-time voice through the Live API. Gemini 3.8 Flash went GA on September 2, and Gemini 4 Argon on October 13. The migration trap is the Antigravity managed agent: `antigravity-preview-09-2026` replaced the May version on September 17, which shut down October 5. If you run tools locally, built-in tool names and parameters changed. Also on October 8, `gemini-3.7-flash` was deprecated and requests are auto-routed to `gemini-3.8-flash`.

This is where evidence was strongest, so it gets real specifics.

- 
**Voice:** OpenAI made GPT-Live 1 GA on September 10 at $0.05 per minute (backend model and tool usage billed separately), and on October 16 added prompt-to-voice custom voices. It also deprecated`whisper-1` and the GPT-4o transcribe models, shutting down February 26, 2027, in favor of`gpt-transcribe` and`gpt-live-transcribe` . Google's counterpart is Gemini 3.8 Live (above) and Gemini 3.5 Transcribe (Aug 26, 85+ languages).
- 
**Image:** OpenAI released GPT Image 2.5 Sunburst and Flare on September 8, Sunburst for editing precision and Flare for fast everyday generation. Alibaba says its Qwen-Image-2.1 weights shipped September 20 (research license only) and a commercial API on October 9.
- 
**Video:** Google made Gemini Omni Flash GA on August 27, adding video extension, first-and-last-frame interpolation and up to 4K output (1080p and 4K are upscaled). The preview endpoint was set to deprecate September 30. On September 1, Google also added agentic video understanding, which it says uses up to 88% fewer tokens on long videos.

**Coverage limit:** I verified these against OpenAI's and Google's own changelogs and one vendor blog. I did not find or verify image, video or voice launches from Anthropic in the sections I read, and I did not research other video vendors, so I'm not claiming a market-wide picture.

- 
**Meta Muse Glimmer** (August 10): a 30B-parameter agentic model with weights on Hugging Face under Apache 2.0. Meta says it runs on a Mac or PC with a single consumer GPU and accepts text and images.
- 
**Mistral Large 4** (October 6): a public preview API of a roughly 1T-parameter multimodal model. Mistral promised weights "by the end of the month." I could not confirm a release as of October 28, so treat it as hosted preview only.
- 
**Qwen-Image-2.1:** open weights, but the license covers research and evaluation; commercial production requires Alibaba's Model Studio API or a separate commercial license from Qwen.
- 
**Copilot CLI** can now discover local Ollama models, as above.

The earlier Gemma 4 release (April 2) falls just before this window, so I left it out.

Three takeaways from the research. First, agents run longer, so permissions and cost limits matter more than which model is top this week. Second, provider churn is real maintenance: Assistants API gone, Sonnet 4.5 retiring, Antigravity tools renamed, Gemini routing changed. Third, previews are everywhere. Of the October launches, several are preview or rolling out by plan, so check your plan before planning around them.

I haven't run these yet. This is a suggested starting order you can adapt.

**First 60 minutes**

1. Open the Copilot changelog and the changelog for any provider you call from code. Search for "deprecat" and "shut down."
2. Update VS Code and Copilot CLI. Run one small, low-risk task in the Agents window or CLI and read every permission prompt.
3. Check your billing view so you know what a session costs.


*A practical first-hour reset flow: gather context, execute one bounded task, verify evidence, then hand off for human review before shipping.*

**First week**

1. Try assisted permissions on a throwaway repo.
2. Install one plugin from the marketplace, then one connector if your plan allows it.
3. Fix any retired or deprecated model or endpoint your code still calls (Assistants API, Sonnet 4.5, old computer use tools, Gemini 3.7 and 3.5 Flash strings).
4. If you build voice features, prototype one session each on GPT-Live and Gemini 3.8 Live and compare latency and billing.

| Learn first | Try next | Ignore for now | 
|---|---|---|
| Agent session basics, permissions, cost view, deprecation notices | Dynamic workflows (preview), connectors (preview), Fusion, voice APIs | Leaderboards, every new model name, unreleased open weights | 

A companion article explores context engineering (grounding a model with the right material and feedback) through cooking and baking for my family. I'll link it here once it's published.

Start at the vendor pages: GitHub changelog, VS Code updates, OpenAI API changelog, Claude Platform release notes, Gemini API release notes and deprecations. Statuses were checked on October 28, 2026 and will change.

## Top comments (0)
