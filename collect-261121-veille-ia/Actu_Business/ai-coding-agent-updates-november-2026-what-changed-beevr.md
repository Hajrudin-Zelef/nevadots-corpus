---
id: collect-261121-veille-ia/actu-business/ai-coding-agent-updates-november-2026-what-changed-beevr
title: "AI Coding Agent Updates (November 2026): What Changed — BeevR"
domain: actu-business
role: reference
task: article
actors: []
dates: []
keywords: ["november", "claude", "copilot", "models", "model", "enterprise", "october", "gemini"]
source: https://beevr.ai/blog/ai-coding-agents-november-2026
source_anchor: ""
source_lines: [1, 64]
section: "Actu_Business"
---
**From 2 October to 13 November 2026, the changes that matter most: GitHub retired 10 Copilot models (3 more go on 16 November) and made its model-policy API GA; Devin added budgets and self-serve SSO; Claude Code added pooled group budgets and a cost-report API; Claude Fable 5.2 shipped; Google shuts Jules down on 31 December.**

Updated 13 November 2026. This is our monthly roundup of AI coding agent updates, written for engineering managers and CTOs who have to keep a fleet of agents working, governed and paid for. We only list changes we could confirm in each vendor's own changelog, docs or announcement. Rumours, aggregator posts and leaked benchmarks are left out. If you are choosing a tool rather than tracking one, start with our Cursor vs Claude Code vs Codex vs Devin comparison.

The month was about control, not features: model retirements you must plan for, APIs to enforce which models people can use, and budget controls for agent spend. New frontier models also landed. Claude Fable 5.2 shipped on 9 November, and Gemini 4 Carbon reached GitHub Copilot on 10 November.

Here is the short list, by vendor:

GitHub deprecated 10 models across Copilot Chat, inline edits, ask and agent modes, and code completions in two waves, on 2 October and 19 October. A third wave of 3 models is scheduled for 16 November. Each one has a suggested replacement. Enterprise admins may have to enable the replacement in their model policy before developers can see it.

| Deprecated model | Date | Suggested replacement | 
|---|---|---|
| Gemini 3.5 Flash, Gemini 3.6 Flash | 2 Oct 2026 | Gemini 3.8 Flash | 
| Kimi K2.7 Code | 2 Oct 2026 | Kimi K3 | 
| Claude Opus 4.7 | 2 Oct 2026 | Claude Opus 5.5 | 
| Gemini 3.7 Flash | 19 Oct 2026 | Gemini 3.8 Flash | 
| GPT-5.5, GPT-5.4 | 19 Oct 2026 | GPT-6.1 Sol | 
| GPT-5.4 mini, GPT-5 mini | 19 Oct 2026 | GPT-6 Luna | 
| Grok 4.5 | 19 Oct 2026 | Grok 4.7 | 
| Claude Opus 4.8, Claude Opus 5 | 16 Nov 2026 (scheduled) | Claude Opus 5.5 | 
| GPT-5.6 Terra | 16 Nov 2026 (scheduled) | GPT-6.1 Sol or GPT-6 Luna | 

Sources: GitHub changelog entries for 2 October, 19 October and the mid-November notice. One exception: Claude Opus 4.8 stays available to individual subscribers on annual plans.

GitHub says you do not need to remove deprecated models yourself. The real risk is elsewhere: scripts, Actions workflows, custom agents or prompt files that pin a model name. They fail or fall back silently. Search your repos for hard-coded model IDs before 16 November. Then run your own evaluation set against the replacement, because a "suggested alternative" is not guaranteed to behave the same on your codebase. Our LLM model migration guide covers how to do that without regressions.

Since 19 October 2026, admins can read and change which Copilot models are enabled, through GA REST endpoints rather than only the settings UI. That lets you manage model allow-lists as code and check them in CI.

The changelog entry lists GET and PATCH at four levels:

`/enterprises/{enterprise}/copilot/model-policies``/enterprises/{enterprise}/teams/{team_slug}/copilot/model-policies``/enterprises/{enterprise}/copilot/apps/model-policies``/orgs/{org}/copilot/model-policies`
Enterprise policies can hand model control down to organizations, or to teams and apps in enterprise-teams mode. This matters more now that new models are arriving fast. Claude Fable 5.2 reached Copilot on 9 November with its policy *off* by default for Business and Enterprise. Gemini 4 Carbon arrived on 10 November and is enabled automatically if your global default policy is on. Both are billed at provider list price under usage-based billing. We show how to wire this into a wider control plane in governing AI coding agents in the enterprise.

Devin's release notes from this period are mostly enterprise controls: budgets, SSO, token policy and session security. The biggest admin features are the 30 October budget hierarchy and self-serve SSO. The Devin release notes confirm:

`GET /v3/enterprise/consumption/daily/sessions`.
Also notable: Devin Oncall went GA on 19 October, and playbooks began migrating to skills on 23 October.

Anthropic released Claude Fable 5.2 on 9 November at $8/$40 per million input/output tokens. Claude Code shipped versions 2.1.305 to 2.1.322 between 20 October and 10 November, most of them adding admin and cost controls.

From the Fable 5.2 announcement: Anthropic reports 49.1% on FrontierCode 2.0 and 69.9% on FrontierSWE v2. These are vendor-run benchmarks. Fast mode costs $16/$80 per million tokens. Eligible enterprises can get zero data retention if they add Enterprise Frontier Safeguards; other customers have 30-day retention for safety monitoring. That retention point also applies inside Copilot, where Fable 5.2 keeps prompts and outputs to run safety classifiers. Check it against your data policy before you enable the model.

From the Claude Code changelog (dates from GitHub releases):

`permissions.blockWritesOutsideWorkingDirectories`, which makes Write, Edit and NotebookEdit refuse paths outside the working directories in every permission mode. Gateway upstreams can also be limited to specific IdP groups.`managed.policy_mode: layered`. Fast mode for Opus 4.8 is deprecated, with removal planned for 30 November.`permissions.denyInsteadOfPrompting`, so a tool call that would wait for approval is denied and the turn continues. That is useful for unattended runs.
Codex shipped incremental releases rather than a headline launch. The most useful changes are the new GPT-6.1 Terra model in more catalogs and centrally managed network rules. The Codex changelog lists:

`permissions.network`, and a Google Cloud Vertex AI setup flow behind a flag.
The Codex pricing page notes that GPT-5.5 retired from Codex with ChatGPT sign-in on 14 October 2026. API-key sessions are unaffected. GPT-6.1 Sol is off by default for Enterprise and Edu until an admin enables it.

Composer 2.5 is not new this month. Cursor published it on 18 May 2026, priced at $0.50/$2.50 per million tokens for the standard tier. In this window Cursor shipped platform features instead.

The commercial change matters more. Cursor became part of SpaceX on 14 August, and its pricing page now sells plans through "The X Subscription". The $8 Plus tier covers Grok models only. Third-party models such as Claude and GPT start at Premium ($30) and are billed at API list price. If your team standardised on Cursor for model choice, re-read your plan.

Yes. Google's Jules changelog announced the turndown on 29 October 2026. Users could finish sessions until 11 November. Jules is now read-only and shuts down on 31 December 2026. Code, branches and pull requests already live in GitHub and are not affected. The export covers only the settings and session history stored in Jules. Google points users toward Antigravity, its "agent-first platform", but has not said which Jules features will move there or when. If Jules runs in any of your workflows (scheduled tasks, issue triage), replace it now, not in late December.

Do four things before the end of November: remove pinned model names before 16 November, put model policies under version control, set budgets on every agent platform you pay for, and replace any Jules automation.

`denyInsteadOfPrompting` and blocking writes outside the working directory are small changes that close large gaps. The security benchmarks show why.
None of this changes the main finding from the AI coding agent productivity research: review and testing, not code generation, are the bottleneck.

BeevR is a senior, founder-led software and AI studio in Hanoi. We build production AI agents on the rule that the model is an untrusted component. Our open-source Kite framework is built on that rule. For engineering teams, we can set up the controls described here as one fixed-scope phase: model allow-lists as code, budget caps, sandboxed execution and review gates. You get a working demo every Friday, and you own all source code and IP from day one. Phases are priced from $10K, with no per-user fees.

If you want an outside team to build or harden your AI agent setup, book a 30-minute call and we will tell you plainly which parts are worth doing first.
