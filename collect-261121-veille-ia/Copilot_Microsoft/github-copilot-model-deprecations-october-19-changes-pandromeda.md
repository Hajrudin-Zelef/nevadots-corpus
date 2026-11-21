---
id: collect-261121-veille-ia/copilot-microsoft/github-copilot-model-deprecations-october-19-changes-pandromeda
title: "GitHub Copilot Model Deprecations: October 19 Changes | Pandromeda"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-10-19"]
keywords: ["gpt-5", "model", "october", "github", "copilot", "mini", "gemini", "flash"]
source: https://pandromeda.com/software/github-copilot-model-deprecations-what-changed-on-october-19
source_anchor: ""
source_lines: [1, 108]
section: "Copilot_Microsoft"
---
# GitHub Copilot Model Deprecations: What Changed on October 19

GitHub Copilot removed six AI models on October 19, 2026. Here is what was retired, what replaces each one, and how to update your settings.

GitHub retired six more GitHub Copilot AI models on October 19, 2026, pulling Gemini 3.7 Flash, GPT-5.5, GPT-5.4, GPT-5.4 mini, GPT-5 mini, and Grok 4.5 out of Copilot Chat, inline edits, ask and agent modes, and code completions. No developer action is required to remove them, but anyone whose workflows, extensions, or default model settings still point at the retired names needs to switch before those references simply stop resolving.

## What changed on October 19

GitHub's changelog confirms the cutover happened on schedule: "As of today, October 19, 2026, we have deprecated the following models across most GitHub Copilot experiences (including Copilot Chat, inline edits, ask and agent modes, and code completions)," the company wrote in its October 19 changelog post. The deprecations had been flagged a month earlier in a September 18 notice warning that the six models would be removed "in mid-October."

This is the second scheduled wave of Copilot model retirements in October 2026. The first batch went out on October 2 and removed a different set of models — Gemini 3.5 Flash, Gemini 3.6 Flash, Kimi K2.7 Code, and Claude Opus 4.7. The October 19 wave is unrelated to that round and covers an entirely different model lineup.

**The short version:**Gemini 3.7 Flash, GPT-5.5, GPT-5.4, GPT-5.4 mini, GPT-5 mini, and Grok 4.5 were deprecated across GitHub Copilot on October 19, 2026. No action is needed to remove them, but they will disappear from the model picker, so update any pinned defaults, CI configs, or extension settings that still name them.

## Which models were removed, and what replaces them

GitHub's changelog lists a suggested alternative for each retired model. Notably, the replacement names published on the October 19 effective-date post differ from the ones GitHub originally floated in its September 18 warning — itself a small illustration of how fast the underlying model lineup is moving even within a single deprecation cycle.

| Deprecated model (removed Oct. 19, 2026) | Suggested alternative (per Oct. 19 changelog) | 
|---|---|
| Gemini 3.7 Flash | Gemini 3.8 Flash | 
| GPT-5.5 | GPT-6.1 Sol | 
| GPT-5.4 | GPT-6.1 Sol | 
| GPT-5.4 mini | GPT-6 Luna | 
| GPT-5 mini | GPT-6 Luna | 
| Grok 4.5 | Grok 4.7 | 

When GitHub first announced the plan on September 18, the suggested replacements were listed as Gemini 3.8 Flash, GPT-5.6 Sol (for both GPT-5.5 and GPT-5.4), GPT-5.6 Luna (for both GPT-5.4 mini and GPT-5 mini), and Grok 4.6. By the time the deprecation actually took effect a month later, GitHub's live changelog pointed to newer successors instead — GPT-6.1 Sol, GPT-6 Luna, and Grok 4.7 — meaning the OpenAI and xAI model families Copilot draws from had already moved on before the October 19 deadline even arrived. Gemini 3.8 Flash was the one recommendation that stayed consistent between the two posts.

Practically, that means anyone who read the September notice and set a calendar reminder to switch to "GPT-5.6 Sol" should instead check the model picker on or after October 19 for whichever current-generation model GitHub is actually surfacing, rather than assuming the originally announced name is still the right target.

## Who actually notices this

The six retired models span three different cost and capability tiers, which is why this deprecation touches more workflows than a single high-end model swap would. Gemini 3.7 Flash and the "mini" GPT variants (GPT-5.4 mini, GPT-5 mini) are the lightweight, low-latency models developers and automated pipelines tend to pick for fast, high-volume tasks — quick completions, simple refactors, or agent steps that don't need a flagship reasoning model. GPT-5.5 and GPT-5.4 sit in the mid-to-upper tier many teams use as a default for Copilot Chat and agent mode. Grok 4.5 is xAI's model in the Copilot lineup. Losing all three tiers at once in a single changelog entry means the deprecation is as likely to hit a CI job calling a "mini" model for cheap, fast completions as it is to hit a developer's everyday Copilot Chat default.

Anyone using GitHub Copilot through Copilot Chat, inline edits, ask mode, agent mode, or code completions — on github.com, in VS Code, or in another supported IDE — is covered by this change, since GitHub's changelog specifies the deprecation applies "across most GitHub Copilot experiences." The one carve-out pattern GitHub has used in earlier rounds, and repeats for the upcoming November deprecation, is to let certain models remain available to individual subscribers on annual plans even after a broader deprecation; the October 19 notice itself does not list any such exception for the six models it covers.

## What happens to existing chats and code that used the removed models

GitHub's guidance is brief on this point: "No action is required to remove the deprecated models" once the deprecation date passes. The changelog does not describe any special handling for chat history or code that was generated using one of the six retired models — it simply instructs users and admins to "update your workflows and integrations to use supported models." In practice, that means new requests against a deprecated model name will no longer be served by that model; code and chat transcripts already produced are not retroactively altered, but any pinned configuration, API call, or extension setting that explicitly names a retired model (for example, Gemini 3.7 Flash or GPT-5.4 mini) needs to be pointed at a currently supported model to keep working.

## How to check and change your Copilot model

GitHub's documentation on changing the Copilot chat model describes the process for Copilot Chat specifically:

- **In VS Code:** open Copilot Chat from the chat icon in the title bar, then use the model dropdown at the bottom of the chat view to pick a model. Selecting "Manage Models" from that same dropdown lets you add a provider, enter credentials or a model ID, and choose which models appear in the list going forward.
- **On github.com:** the available models in Copilot Chat depend on your Copilot plan and the surface you're using; the model selector works the same way, showing whichever models your plan and admin policies currently enable.
- **Code completions:** switching your Chat model does not change the model used for inline code completions — that's a separate setting, configured through its own menu rather than the chat picker.
- **Auto model selection:** if you rely on Copilot's auto mode, which picks a model based on availability, GitHub recommends starting a new chat session after a model switch rather than continuing an existing thread.

The current list of supported and retired models — which changes frequently — is maintained on GitHub's supported AI models reference page, which is the most reliable place to confirm whether a given model is still active before you build a workflow around it.

## What Enterprise and Business admins need to know

GitHub's changelog notes that Copilot Enterprise administrators "may need to enable access to alternative models through their model policies in Copilot settings," and that admins can verify availability by checking their individual Copilot settings and confirming the relevant policy is enabled for a given model. Once enabled, the model shows up in the Copilot Chat model selector in VS Code and on github.com.

A related mechanism GitHub calls "default model enablement" governs how automatically this happens. Under that system, suggested alternative models are automatically enabled for Copilot Enterprise and Copilot Business customers, unless an administrator has specifically turned off the "global default" setting or explicitly disabled that model. If an organization has turned off the global default, admins have to opt in to each alternative model manually through model policies before their users can select it.

GitHub also shipped a companion change the same day: a REST API for Copilot model policies, now generally available for enterprises, enterprise teams, agent apps, and organizations. The new GET and PATCH endpoints let admins read and update which models are enabled at the enterprise, team, app, or organization level programmatically, and enterprise-level policies can delegate model control down to organizations or teams rather than requiring every change to go through a central admin console. For engineering organizations managing Copilot access across many teams, that API turns what used to be a settings-page chore into something that can be scripted and audited.

## Why GitHub keeps rotating Copilot's models

October 19 is not an isolated event. GitHub has now run two separate Copilot model deprecation cycles in October 2026 alone, and it has already announced a third, scheduled for November 16, 2026, that will retire Claude Opus 4.8, Claude Opus 5, and GPT-5.6 Terra. The pattern reflects how quickly the underlying foundation-model vendors — OpenAI, Google DeepMind, Anthropic, and xAI — are shipping new point releases of their own models. Copilot's model lineup is less a fixed menu than a rolling window onto whatever each vendor currently considers its best-available model at a given price and latency tier, and GitHub's changelog has become the fastest way to track that churn across providers in one place.

The cadence between announcement and effective date has also stayed fairly consistent across rounds: GitHub gave about a month's notice before the October 19 removals (announced September 18) and is giving roughly the same lead time before the November 16 round (announced October 15). That consistency is useful for planning, but it doesn't make the underlying churn any slower — if anything, the fact that GitHub's own suggested replacements for the October 19 batch changed between the September announcement and the October cutover shows that even a month-long notice window isn't always long enough to guarantee the recommended replacement will still be the newest option by the time the deadline arrives.

That pace creates real operational overhead for teams that have built automation, CI steps, or internal tooling around a specific Copilot model name rather than a model tier or an "auto" setting. It also explains why GitHub keeps repeating the same boilerplate guidance in each notice — update your workflows before the date, no cleanup required after — rather than offering migration tooling: with models rotating roughly every few weeks, the practical answer for most developers is to use GitHub's auto model selection or periodically check the supported-models reference rather than hard-coding any single model name. For engineering organizations, the new model-policy REST API points in the same direction: rather than manually toggling individual models in a settings page every time a changelog post lands, admins can now script policy checks and updates, which is arguably a tacit admission from GitHub that the rotation isn't slowing down.

## What's next

The next scheduled Copilot model deprecation lands November 16, 2026, retiring Claude Opus 5, Claude Opus 4.8 (with an exception for individual subscribers on annual plans, who keep Opus 4.8 access), and GPT-5.6 Terra, with Claude Opus 5.5 and GPT-6.1 Sol or GPT-6 Luna suggested as replacements. For teams that standardized on Copilot's newer Gemini 4 Argon option or are weighing Copilot against rival agent tooling in pricing terms, as covered in Pandromeda's Copilot vs. Cursor comparison, the safest long-term move is the same one GitHub keeps repeating: avoid pinning workflows to a specific model name, check the supported-models page before a deadline rather than after, and treat each changelog post as provisional until the effective date, since even GitHub's own suggested replacements shifted between its September warning and the October 19 cutover.

## Frequently asked questions

### Which GitHub Copilot models were removed on October 19, 2026?

GitHub deprecated six models across GitHub Copilot on October 19, 2026: Gemini 3.7 Flash, GPT-5.5, GPT-5.4, GPT-5.4 mini, GPT-5 mini, and Grok 4.5. The removal covered Copilot Chat, inline edits, ask and agent modes, and code completions.

### What are the suggested replacement models?

GitHub's October 19 changelog lists Gemini 3.8 Flash for Gemini 3.7 Flash, GPT-6.1 Sol for both GPT-5.5 and GPT-5.4, GPT-6 Luna for both GPT-5.4 mini and GPT-5 mini, and Grok 4.7 for Grok 4.5. These replacement names were updated from the ones GitHub originally suggested in its September 18 announcement.

### Do I need to do anything to remove the deprecated models?

No. GitHub's changelog states that no action is required to remove the deprecated models. However, any workflow, extension, or integration that explicitly names one of the six retired models should be updated to a currently supported model, since it will no longer be served by the old model name.

### What happens to code or chats created with a deprecated model?

GitHub's changelog does not describe any change to previously generated code or existing chat transcripts. The deprecation affects new requests going forward; code and chats already produced are not retroactively altered, but you can no longer continue using the retired model name for new requests.

### How do I change which AI model GitHub Copilot uses?

In VS Code, open Copilot Chat and use the model dropdown at the bottom of the chat view, or select Manage Models to add providers and choose which models appear. On github.com, the model selector in Copilot Chat shows whichever models your plan and admin policies currently enable. Code completions use a separate setting from the chat model.

### How does this affect GitHub Copilot Enterprise and Business admins?

Under GitHub's default model enablement system, suggested alternative models are automatically enabled for Copilot Enterprise and Business customers unless an admin has turned off the global default or explicitly disabled that model. Admins can also now manage model policies at scale through a new REST API covering enterprises, enterprise teams, agent apps, and organizations.

## Sources

- GitHub Changelog: GitHub Copilot models deprecated on October 19, 2026github.blog
- GitHub Changelog: Upcoming deprecation of selected GitHub Copilot models in mid-Octobergithub.blog
- GitHub Changelog: Copilot model policy API for enterprise, orgs, and teamsgithub.blog
- GitHub Changelog: Upcoming deprecation of selected GitHub Copilot models in mid-Novembergithub.blog
- GitHub Docs: Change the chat modeldocs.github.com
- GitHub Docs: Supported AI models in Copilotdocs.github.com

Felix Moreau writes Pandromeda's software coverage and how-to guides. He covers Windows, macOS and Linux updates, the apps people rely on, emulators and developer tools, and turns official documentation into clear, numbered steps that work on the current version.
