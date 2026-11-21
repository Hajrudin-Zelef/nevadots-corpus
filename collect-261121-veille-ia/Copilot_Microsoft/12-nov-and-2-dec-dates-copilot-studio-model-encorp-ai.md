---
id: collect-261121-veille-ia/copilot-microsoft/12-nov-and-2-dec-dates-copilot-studio-model-encorp-ai
title: "12 Nov and 2 Dec Dates — Copilot Studio Model… | encorp.ai"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-11-03"]
keywords: ["gpt-5", "copilot", "agents", "studio", "model", "chat", "microsoft", "models"]
source: https://encorp.ai/en/blog/copilot-studio-model-retirement-2026-11-03
source_anchor: ""
source_lines: [1, 64]
section: "Copilot_Microsoft"
---
# Copilot Studio Model Retirement: Two Deadlines Before 2 December

**TL;DR:** Microsoft is replacing the AI models behind Copilot Studio agents and behind prompts in Power Apps, Power Automate and Copilot Studio in two waves. Agents on older GPT models move on 12 November 2026. Prompts on older GPT, Claude and Grok models move on 2 December 2026. Both migrations are automatic, and both can change how your automations behave. If your company runs Power Platform flows or Copilot Studio agents in production, the next four weeks are the window to find them, test them and assign an owner.

## Key facts

- **Wave 1, agents (12 November 2026):** In Copilot Studio, agents set to GPT-4.1, GPT-4.1 Mini, GPT-5 Chat, GPT-5 Auto or GPT-5 Reasoning on the Standard runtime move automatically to "GPT Chat Latest", according to Microsoft's message center notice MC1506318.
- **Wave 2, prompts (2 December 2026):** Prompts in Power Apps, Power Automate and Copilot Studio that use GPT-4.1 mini, GPT-4.1, GPT-5 chat, GPT-5.3 chat, GPT-5 reasoning, GPT-5.2 reasoning, Claude Sonnet 4.6, Claude Opus 4.6 or Grok 4.1 Fast are migrated to newer OpenAI models, per notice MC1536101, published on 3 November.
- **Pricing tiers stay, consumption may not:** Microsoft says tier pricing does not change, but actual consumption "can vary with usage, including input and output token counts".
- **Behaviour can change:** The same notice warns that responses can differ in wording, formatting and how instructions are followed.
- **EU exception for agents:** For environments outside the US where cross-geo data movement is restricted, GPT-4.1 and GPT-5 remain supported and do not retire in the November agent wave.

## What exactly changes

The two waves look similar, but they hit different parts of the Microsoft stack and follow different rules.

**Wave 1: the primary model of Copilot Studio agents.** The model that powers an agent's orchestration (deciding which tool to call, what to ask, how to answer) is being retired for five older GPT versions. The replacement, GPT Chat Latest, is described in Microsoft's model selection guide as an option that keeps agents on the latest supported GPT chat snapshot within the current major version. A move to a new major version is announced separately, with at least 30 days' notice. Agents built on the GitHub Copilot runtime follow a different table: GPT-5 Chat goes to GPT 5.6 Terra, GPT 5.6 Sol to GPT 6 Sol and GPT 5.6 Luna to GPT 6 Luna.

**Wave 2: prompts used inside apps, flows and agents.** Prompts are the reusable AI steps that many teams build into Power Automate flows and Power Apps, for example to classify an email, extract fields from an invoice or summarise a ticket. On 2 December, each retiring model is swapped for the replacement in its pricing tier:

| Tier | Retiring models | Replacement | 
|---|---|---|
| Basic | GPT-4.1 mini | GPT-6 Luna | 
| Standard | GPT-4.1, GPT-5 chat, GPT-5.3 chat, Claude Sonnet 4.6, Grok 4.1 Fast reasoning | ChatGPT-latest | 
| Premium | GPT-5 reasoning, GPT-5.2 reasoning, Claude Opus 4.6 | GPT-6 Reasoning | 

Source: MC1536101.

One practical difference matters for planning. For agents, Microsoft offers a setting to keep using a retired model for 30 days after an automatic upgrade, and its documentation lists this extension for the November models. The prompt notice offers no such buffer: after 2 December the previous models "will no longer be available in prompts". For flows, testing has to happen before the date, not after.

## Why "automatic" does not mean "unchanged"

A migration that keeps every flow running can still break a business process. The typical failure is quiet: the flow succeeds, but the output is different.

- **Structured outputs.** If a prompt returns JSON, a fixed label such as "urgent / normal / low" or a field list that a later step parses, a new model that formats slightly differently can push wrong values into a CRM, ERP or ticketing system. Microsoft explicitly asks makers to review solutions that depend on specific output structures.
- **Instruction following.** Newer models may read instructions more literally or more loosely. A prompt tuned over months for one model's habits may need adjustment.
- **Settings that no longer apply.** Microsoft's prompt settings documentation notes, for example, that temperature is not available for the GPT-5 reasoning model. Teams that relied on low temperature for consistent answers should confirm which settings the replacement model supports.
- **Consumption.** According to the same documentation, prompts consume Copilot Credits, and any AI Builder credits are used first when prompts run in Power Apps or Power Automate. The same documentation explains that one agent turn can trigger several model calls. A model that writes longer answers or reasons more can therefore change monthly consumption even when the tier price stays the same.

## EU details worth reading twice

For companies in Bulgaria and the rest of the EU, two details in Microsoft's documentation deserve attention from IT and the data protection officer.

First, the November agent wave has a regional exception. Where an environment outside the US restricts cross-geo data movement, GPT-4.1 and GPT-5 stay supported. Whether this applies depends on your environment settings, described on the Power Platform page on moving data across regions. It is worth checking per environment, not assuming per tenant.

Second, data location differs by model. In the regional table of the model selection guide, GPT Chat Latest is the default for Europe (except the UK) without a cross-geo flag, while the Claude models are marked cross-geo, meaning data might be processed outside the region. Microsoft's prompt documentation also states that Anthropic models are hosted outside Microsoft and subject to Anthropic's terms. A prompt moved from Claude to an OpenAI model may therefore change where data is processed. Record the change in your AI system inventory, so that internal AI governance documentation reflects what actually runs.

## A four-week plan before 2 December

1. **Inventory (this week).** In the Power Platform admin center, go to Manage > Copilot Studio and review agents by model. Microsoft notes that the model field is in preview and reflects published configurations only, so ask makers about unpublished drafts. For prompts, list every flow and app that calls one and note the model and tier.
2. **Rank by consequence.** A flow that drafts an internal summary is low risk. A flow that writes into finance, HR or customer systems, or that sends messages to customers, is high risk. Test the high-risk ones first.
3. **Save a baseline now.** While the old models are still available, run a set of real (or realistically anonymised) inputs and save the outputs, tool calls and credit use. Without a baseline, there is nothing to compare against after the switch.
4. **Test the replacement.** Select the replacement model in the prompt builder or agent settings and run the same inputs. Check output format, downstream parsing, refusals, latency and consumption.
5. **Adjust and assign owners.** Fix instructions or parsing where results differ, and name one owner per critical flow who watches results for two weeks after the migration date.

## Who should own this

Model retirements are no longer rare events. Microsoft's own retirement table lists separate waves on 15 October, 31 October and 12 November, and the company has said it will announce retirement dates for older Claude models separately. Companies that treat each wave as a surprise end up firefighting. Companies that keep an inventory, a test set and a named owner per automation handle the same change as routine maintenance.

In practice this sits between IT, the business teams who built the flows and whoever is accountable for AI governance. Citizen developers who build prompts also need to know why output formats and baselines matter, which is a natural topic for AI training for teams. And automations that keep breaking on model changes are often a sign that they need to be rebuilt with validation steps, as part of a broader AI automation review.

## Bottom line

Two dates, two different rules: 12 November for agents, with a 30-day fallback, and 2 December for prompts, with none. The work is not complex, but it has to start now. If your company wants an outside view on which flows and agents are at risk, and a monitoring routine for the next wave, Encorp.ai's AI Ops service covers model changes, cost tracking and quality checks after go-live.
