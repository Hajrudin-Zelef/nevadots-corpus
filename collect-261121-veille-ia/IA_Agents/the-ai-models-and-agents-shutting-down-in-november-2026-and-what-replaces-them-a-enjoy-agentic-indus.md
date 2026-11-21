---
id: collect-261121-veille-ia/ia-agents/the-ai-models-and-agents-shutting-down-in-november-2026-and-what-replaces-them-a-enjoy-agentic-indus
title: "The AI models and agents shutting down in November 2026, and what replaces them â Enjoy Agentic Industries"
domain: ia-agents
role: reference
task: article
actors: []
dates: ["2026-11-02"]
keywords: ["claude", "november", "opus", "fast", "mode", "google", "models", "agents"]
source: https://enjoy.dev/blog/ai-models-shutting-down-november-2026/
source_anchor: ""
source_lines: [1, 115]
section: "IA_Agents"
---
- To
- Everyone who picks a model in an agent or an API
- From
- Mo, Chief Executive
- Date
- November 2, 2026, 9:08 AM CST
- Re
- Novemberâs model shutdowns

# The AI models and agents shutting down in November 2026, and what replaces them

In November 2026, GitHub Copilot dropped Claude Opus 4.8, Opus 5 and GPT-5.6 Terra on November 16, and on November 30 Claude Sonnet 4.5 leaves the Claude API, fast mode on Opus 4.8 ends in Claude Code, and OpenAI shuts Agent Builder. Antigravityâs free Claude, twice delayed, now goes November 30, Jules turned read-only after November 11, and OpenAI has put off leaving Cursor past November 12.

## What shuts down in November, by date

Some of Novemberâs shutdowns are models leaving one app while they stay everywhere else, some are features, and some are whole products. Here they are in order, as each companyâs own page gives them on November 13, and Google AI Plusâs from the email Google sent subscribers:

| Date | What goes | Where | What replaces it | 
|---|---|---|---|
| Nov 11 | Jules turns read-only, then shuts down December 31 | Google Jules | Export your data; Google points to Antigravity | 
| Nov 12, put off with no new date | OpenAIâs models | Cursor | Your own OpenAI API key, the Codex extension, or a gateway | 
| Nov 13, by Googleâs email | Pro models, for Google AI Plus | Gemini app and AI Mode in Search | Flash and Flash-Lite; Google AI Pro to keep Pro | 
| Nov 15 | Claude Haiku 4.5, codex-mini | Microsoft Foundry | None named | 
| Nov 16 | Claude Opus 4.8, Claude Opus 5, GPT-5.6 Terra | GitHub Copilot | Claude Opus 5.5; GPT-6.1 Sol or GPT-6 Luna | 
| Nov 17 | Gems turn read-only, then go December 17; Opal and Gems by Google Labs turn off | Gemini app, on personal accounts 18 and over | Skills, which Google converts Gems into; nothing for Opal | 
| Nov 19 | o1, o1-pro, o3, o3-pro, o3-mini, o3-deep-research, o4-mini | Azure OpenAI, in Microsoft Foundry | `gpt-5.6-sol` , or`gpt-5.6-terra` for o3-mini and o4-mini | 
| Nov 24 | Claude Opus 4.5 | Microsoft Foundry | None named | 
| Nov 30 | Claude Sonnet 4.5 | Claude API and Microsoft Foundry | `claude-sonnet-5-5` | 
| Nov 30 | Fast mode on Claude Opus 4.8 | Claude Code and the Claude API | Fast mode on Opus 5.5 | 
| Nov 30 | Agent Builder, Evals, reusable prompts | OpenAI API | Agents SDK or ChatGPT Workspace Agents; Promptfoo; your own code | 
| Nov 30, moved from Nov 2 and Nov 18 | Claude Sonnet 4.6, Claude Opus 4.6, GPT-OSS-120b | Google Antigravity | Gemini 3.8 Flash on free plans; Claude Sonnet 5.5 and Opus 5.5 on a paid Google AI Pro, Ultra or Enterprise plan | 

Claude Codeâs changelog announced the change to fast mode in version 2.1.316, on October 31. On the morning of November 2, Googleâs models page still listed Antigravityâs three models, marked âWill be removed on November 2, 2026â; by November 3, it read âWill be removed on November 18, 2026,â and on November 20, âWill be removed on November 30, 2026.â

## Claude: Opus 4.8âs fast mode, and Sonnet 4.5

**Fast mode on Opus 4.8 ends November 30.** Claude Code 2.1.316 âDeprecated fast mode for Opus 4.8 as of October 30, 2026; it will be removed on November 30, 2026. After removal, fast mode requests on Opus 4.8 return an error and do not fall back to standard Opus 4.8. Migrate to Opus 5.5 to keep the speedup.â The Claude APIâs fast mode page carries the same warning for `speed: "fast"` requests to `claude-opus-4-8`. Fast mode itself stays on Opus 5.5, where itâs out of research preview and cheaper: $8 in and $40 out per million tokens, against $10 and $50 on Opus 4.8, according to Claude Codeâs fast mode docs. On Pro, Max, Ultra, Team and Enterprise, fast mode is paid from usage credits either way, not from the planâs limits. Opus 4.8 itself stays on the Claude API: Anthropicâs deprecations page lists it as active, with retirement âNot sooner than May 28, 2027.â

**Sonnet 4.5 leaves the Claude API November 30.** Anthropic notified its users on September 30 and names `claude-sonnet-5-5` as the replacement. âRequests to models past the retirement date will fail.â The date covers the Claude API, Claude Platform on AWS, Claude Models on Google Cloud (Anthropic managed) and Microsoft Foundry, whose own schedule agrees. Amazon Bedrock and Googleâs own managed Claude set their own dates.

The people it catches are the ones who never picked Sonnet 4.5. Claude Codeâs model configuration says that on Microsoft Foundry, Claude Codeâs `default` model âdefaults to Sonnet 4.5,â and that its `sonnet` alias means Sonnet 4.5 there and on Bedrock and Google Cloudâs Agent Platform. To move, set `ANTHROPIC_DEFAULT_SONNET_MODEL` to a newer Sonnet your deployment serves, or pick it by its full name.

**Opus 4.5 and Haiku 4.5 arenât on Anthropicâs list.** The Claude API shows both as active, with retirement âNot sooner thanâ November 24 and October 15, 2026. Anthropic promises âat least 60 daysâ notice before model retirement,â by email and in its documentation, and the page shows no notice for either, so neither can leave the Claude API before January. Microsoftâs schedule disagrees for Foundry, where it lists Haiku 4.5 retiring November 15 and Opus 4.5 November 24, with no replacement named. If you run either there, plan for Microsoftâs dates.

## GitHub Copilot: three models on November 16, six on December 14

GitHubâs notice of October 15 says it will deprecate Claude Opus 4.8, Claude Opus 5 and GPT-5.6 Terra âacross all GitHub Copilot experiences (including Copilot Chat, inline edits, ask and agent modes, and code completions) on November 16th, 2026.â It suggests Claude Opus 5.5 for both Claude models and âGPT-6.1 Sol or GPT-6 Lunaâ for Terra. By November 17, GitHubâs model docs listed all three as retired on November 16.

One group keeps a model: âClaude Opus 4.8 will remain available to individual GitHub Copilot subscribers on annual plans so they retain an Opus offering.â On Copilot Business and Enterprise, the suggested replacements are switched on automatically âunless an administrator has turned off the global default or explicitly disabled the modelâ; if yours has, they can enable them in Copilotâs model policies. Nobody needs to remove the old models: âNo action is required to remove the models after they have been deprecated.â

On November 13, GitHub set the next round, with the same wording, for December 14, 2026:

| Model | Suggested alternative | 
|---|---|
| Claude Fable 5 | Claude Fable 5.2 | 
| Claude Haiku 4.5 | Claude Haiku 5.5 | 
| Gemini 3.8 Flash | Gemini 4 Hydrogen | 
| Gemini 4 Argon | Gemini 4 Carbon | 
| Grok 4.6 | Grok 4.7 | 
| Kimi K3 | âTo be announcedâ | 

Again one group keeps a model: âClaude Haiku 4.5 will remain available to individual GitHub Copilot subscribers on annual plans.â GitHub hasnât named a Kimi model to move to.

## Antigravity, Jules and Cursor

Three apps lose something this month, and each has its own memo:

- **Antigravity, November 30, moved from November 2 and then November 18.** Claude Sonnet 4.6, Opus 4.6 and GPT-OSS-120b are due to go, leaving free and Google AI Plus accounts with Gemini 3.8 Flash. On November 19, Googleâs models page still listed all three, marked âWill be removed on November 18, 2026â; on November 20, it changed that to âWill be removed on November 30, 2026,â without a note. Claude Sonnet 5.5 and Opus 5.5 stay on a paid Google AI Pro subscription, not its trial, and on Ultra and Enterprise. Who keeps Claude.
- **Jules, November 11.** Googleâs Jules Turndown says âYou have until November 11, 2026 to continue finishing up sessions,â after which Jules is read-only until it winds down on December 31. You can export your settings, scheduled tasks and every session now. What to do now.
- **Cursor, put off past November 12.** OpenAI had proposed November 12, then said that day it had âtemporarily extended Cursorâs access to our models beyond November 12,â without a new date. Its help article still says âCursor may choose to end access sooner.â Your own OpenAI key works only in Cursorâs local Chat and Agent. How to keep GPT there.

## The APIs: OpenAI, Gemini and Azure

**OpenAI, November 30.** Three products on OpenAIâs deprecations page close that day. Agent Builder is deprecated, with âAgents SDK or ChatGPT Workspace Agentsâ as the replacement and a migration guide, and âChatKit remains available.â The Evals dashboard, API and graders for eval workflows are read-only until then, and OpenAIâs path is to âMigrate to Promptfoo.â The `/v1/prompts` API and reusable prompt objects go the same day: âMove prompt content into your application code.â

**Gemini, moved from November 17.** When we first wrote this, Googleâs deprecations page listed five preview voice models for November 17. On November 13 it gives three of them, `gemini-3.1-flash-tts-preview`, `gemini-2.5-flash-preview-tts` and `gemini-2.5-pro-preview-tts`, a shutdown date of December 18, 2026, with `gemini-3.8-flash-tts` or `gemini-3.8-flash-lite-tts` to replace them. `gemini-2.5-flash-native-audio-preview-12-2025` now goes on March 31, 2027, and `gemini-3.1-flash-live-preview` has âNo shutdown date announced,â both pointing to `gemini-3.8-live`. Google says its dates âindicate the earliest possible dates on which a model might be retired,â and that once a model is shut down, âthe endpoint is no longer available.â

**Azure OpenAI, November 19.** Microsoft retires o1, o1-pro, o3, o3-pro, o3-mini, o3-deep-research and o4-mini in Foundry, 22 days before OpenAIâs own API retires o3 and o3-pro. It names `gpt-5.6-sol` as the replacement, or `gpt-5.6-terra` for o3-mini and o4-mini. Azureâs `codex-mini` goes on November 15, with no replacement listed.

## What follows in December

- **December 1:**`gpt-image-1-mini` ,`gpt-image-1.5` and`chatgpt-image-latest` leave OpenAIâs API, which now suggests`gpt-image-2.6-sunburst` or`gpt-image-2.6-flare` instead.
- **December 1:** On the Gemini API, requests to`gemini-2.5-pro` start going to`gemini-3.8-flash` , and requests to`gemini-2.5-flash` to`gemini-3.5-flash-lite` , billed at those modelsâ rates (our memo).
- **December 11:** OpenAIâs API removes`gpt-5` ,`gpt-5-mini` ,`gpt-5-nano` ,`gpt-5-pro` ,`o3` and`o3-pro` , with their dated snapshots. OpenAI now suggests`gpt-6.1-sol` for`gpt-5` and`o3` ,`gpt-6.1-terra` for`gpt-5-mini` ,`gpt-6-luna` for`gpt-5-nano` , and`gpt-6.1-sol` in pro mode for the two pro models.
- **December 11:** ChatGPT retires custom GPTs, except in FedRAMP workspaces and Enterprise workspaces with an approved deferral, which have until February 11, 2027. Its Advanced and Standard voice go the same day, and people who chose them move to Live voice (our memo).
- **December 14:** GitHub Copilot drops Claude Fable 5, Claude Haiku 4.5, Gemini 3.8 Flash, Gemini 4 Argon, Grok 4.6 and Kimi K3, as above.
- **December 14:** The standalone Claude Design site, claude.ai/design, âstays open until December 14, 2026,â Anthropic says. âClaude Design now works in every conversation with Claude.â
- **December 17:** Gems and their files are removed from the Gemini app on personal accounts (our memo).
- **December 18:** Three Gemini text-to-speech previews leave the Gemini API, as above.
- **December 31:** Jules shuts down.
- **Some day in December:** ChatGPT and Codex start sharing one usage allowance. Nothing shuts down, but OpenAI hasnât given the day (our memo).

## How to check what you use

1. **In Copilot,** open the model picker. Claude Opus 4.8, Opus 5 and GPT-5.6 Terra left it on November 16, except Opus 4.8 on individual annual plans, so if a saved setting still names one, change it. Before December 14, move off Claude Fable 5, Haiku 4.5, Gemini 3.8 Flash, Gemini 4 Argon, Grok 4.6 and Kimi K3. On Business and Enterprise, ask your admin whether the replacements are enabled.
2. **In Claude Code,** a`â¯` beside the prompt means fast mode is on. Run`/model` to see which model itâs on; if thatâs Opus 4.8, switch to Opus 5.5 before November 30. On Foundry, Bedrock or Agent Platform, set`ANTHROPIC_DEFAULT_SONNET_MODEL` so`sonnet` and the default donât point at Sonnet 4.5.
3. **In your own code and settings,** search for each model name in the table, including in environment variables and saved settings. In Anthropicâs Console, the Usage pageâs**Export** gives you a CSV with usage âbroken down by API key and model.â OpenAIâs`GET /v1/models` returns each modelâs`shutdown_date` , or`null` if none has been announced.
4. **If the model youâre losing was your reason to use an app,** you may not need the app to keep it. Claude Opus 4.8 stays on the Claude API and, at standard speed, in Claude Code on a Claude plan. Enjoy runs Claude Code beside Codex and the other labsâ agents, each on the plan you already have, and lets you switch agents in the middle of a conversation; it doesnât add a labâs models to another labâs account.

Weâll update this memo if any of these dates move.

## 8.Questions.

- ### Is Claude Opus 4.8 being removed from GitHub Copilot?
- Yes. GitHub retired it on November 16, 2026, along with Claude Opus 5 and GPT-5.6 Terra, in every Copilot experience. GitHub suggests Claude Opus 5.5 instead. Individual subscribers on annual plans keep Opus 4.8; the other two models go for them too.
- ### Is fast mode going away in Claude Code?
- Only on Opus 4.8. Fast mode on Opus 4.8 ends on November 30, 2026, and after that its requests âreturn an error and do not fall back to standard Opus 4.8.â Fast mode on Opus 5.5 stays, and costs less per token, $8 in and $40 out per million against $10 and $50.
- ### When is Claude Sonnet 4.5 retired?
- On November 30, 2026, on the Claude API, and Anthropic says requests to a retired model fail. Its replacement is `claude-sonnet-5-5`. Amazon Bedrock and Googleâs own Claude offering set their own dates.
- ### Are Claude Opus 4.5 and Haiku 4.5 being retired too?
- Not on the Claude API. Anthropic lists both as active, with no deprecation notice, and promises âat least 60 daysâ notice,â so neither can leave before January. Microsoftâs Foundry schedule does list them, on November 15 for Haiku 4.5 and November 24 for Opus 4.5, and GitHub Copilot drops Haiku 4.5 on December 14, except on individual annual plans.
- ### Is OpenAIâs Agent Builder shutting down?
- Yes, on November 30, 2026, along with the Evals dashboard and API and reusable prompts. OpenAI points Agent Builder users to its Agents SDK or ChatGPT Workspace Agents, and says ChatKit remains.
- ### When do GPT-5 and o3 stop working in the OpenAI API?
- On December 11, 2026. `gpt-5`, `gpt-5-mini`, `gpt-5-nano`, `gpt-5-pro`, `o3` and `o3-pro` go then, with their dated snapshots. OpenAI now names GPT-6.1 Sol, GPT-6.1 Terra and GPT-6 Luna as their replacements. On Azure, o3 and o3-pro go sooner, on November 19.
