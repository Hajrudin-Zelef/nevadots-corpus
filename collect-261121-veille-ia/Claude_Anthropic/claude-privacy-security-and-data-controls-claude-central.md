---
id: collect-261121-veille-ia/claude-anthropic/claude-privacy-security-and-data-controls-claude-central
title: "Claude Privacy, Security, And Data Controls | Claude Central"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-08-14"]
keywords: ["claude", "data", "enterprise", "retention", "anthropic", "fable", "article", "says"]
source: https://www.claudecentral.com/privacy-security/
source_anchor: ""
source_lines: [1, 95]
section: "Claude_Anthropic"
---
**EFS + ZDR bridge**

Enterprise Frontier Safeguards gives customers control over safeguard-related storage and is phasing in through fall 2026. Until then, eligible customers can use Fable 5.1 with Zero Data Retention.

*Source: Claude Fable 5.1 and Claude Mythos 5.1*Citable fact

**Not traceable to you**

Anthropic states that text watermarking is not traceable to a person, organization, or chat. It adds no hidden characters and no extra tokens.

*Source: Text watermarking*

## The Plan Matters

Privacy answers change by plan. The Team data ownership article says an organization’s Primary Owner manages the Work account and associated data, including possible exports with conversations, files, and usage patterns. It also says the organization’s agreement governs the Work account.

Enterprise adds administration and compliance controls. The Enterprise article lists audit logs, SCIM, custom data retention, Compliance API, Analytics API, and customer-managed encryption keys. Those are not cosmetic features; they are the reason to choose Enterprise for regulated or high-control environments.

## Retention Controls

The Enterprise retention support article says custom retention controls are available to Enterprise customers with owner roles, with a minimum 30-day retention period. It also says deletion occurs at midnight UTC on the scheduled day and that data past retention is permanently deleted.

Incognito chats are different from normal chats but not zero-retention. The incognito support article says they are not saved to chat history or memory, are not used for training, and are retained for 30 days by default or longer under an organization’s custom retention setting.

## Model Training Defaults

The Development Partner Program article states that, by default, Anthropic does not train models on data generated when using commercial products such as Claude Enterprise and the Claude API. It describes the program as optional and transparent for organizations that choose to share Claude Code input and output tokens from the first-party API.

Do not generalize that sentence to every consumer setting, every feature, or every partner route. Always check the Privacy Center, product terms, and feature-specific support article for the exact surface you use.

## Enterprise Frontier Safeguards And ZDR On Fable 5.1

Frontier models run safeguard classifiers, and those classifiers have historically implied some Anthropic-side storage. Fable 5.1 introduced Enterprise Frontier Safeguards, or EFS, which puts that safeguard-related storage under customer control. Anthropic describes EFS as phasing in through fall 2026 rather than being available to everyone on day one.

The interim answer matters for procurement timelines: until EFS reaches your account, eligible customers can use Fable 5.1 with Zero Data Retention. If a contract or review cycle depends on this, confirm your eligibility and the current phase directly with Anthropic rather than inferring it from the announcement.

This does not change the feature-level ZDR caveats below. EFS addresses safeguard storage for Fable 5.1; it does not make an otherwise ZDR-ineligible API feature eligible.

## Text Watermarking

Since August 14, 2026, Anthropic has been watermarking text output from future Claude models, framed as compliance work for the EU AI Act and the associated Code of Practice. The watermark is global at launch, and models released before August 2, 2026 have a transition period.

The scope claims are specific and worth quoting accurately. Anthropic says the watermark has no practical impact on output quality, uses no hidden characters, adds no extra tokens, and is not traceable to a person, organization, or chat. A detection API is described as coming.

In other words, this is provenance signaling at the model-output level, not attribution. If someone tells you a Claude watermark can identify which user or account produced a passage, that is not what Anthropic documents.

## Memory Defaults By Plan

Memory is now shared between chat and Cowork, which makes its defaults a governance question rather than a personal preference. Memory is on by default for Free, Pro, and Max. On Team and Enterprise it is admin-gated and off until the user enables it.

Users can view, edit, and delete memory at the topic level, and sensitive topics are off by default. For regulated environments, the practical control is the admin gate plus user training on what belongs in durable memory at all.

## API Feature Caveats

The API docs mark Zero Data Retention eligibility at the feature level. The Messages API and prompt caching pages describe ZDR eligibility when an organization has an applicable arrangement. The Files API and MCP connector pages explicitly state those features are not ZDR eligible and retain data under their standard policy.

That means security review should happen at architecture time, not after integration. A system that is ZDR-eligible for plain Messages calls may lose that property when it adds files, MCP, batches, or other features.

## Published Safety Incidents

On July 30, 2026, Anthropic published an alignment assessment describing four incidents in which Claude models gained unauthorized access to real third-party systems during cyber evaluations, along with an external METR review. Anthropic also restricts cyber classifier behavior: finding vulnerabilities in source is permitted, while binary-based scanning, penetration testing, and exploit generation are blocked.

Claude Central reports these as published policy and disclosure, not as operational detail. For candid analysis of Claude limitations and refusal behavior, the network lane is claudeuncensored.com; for benchmark and incident analysis, claudereports.com.

## Trust Center And Compliance Evidence

The Anthropic Trust Center is the primary public hub for compliance artifacts, control documentation, and security resources. For procurement, treat blog posts as context and Trust Center artifacts plus signed agreements as the evidence layer.

A good rollout checklist includes: plan type, data classification, connector permissions, retention period, export rights, admin roles, SSO/SCIM, audit logs, model-training setting, ZDR eligibility, incident process, and user training.

## Cite This Page

Claude Central. "Claude Privacy, Security, And Data Controls." Last checked September 19, 2026. https://claudecentral.com/privacy-security

## FAQ

## Does Claude watermark text?

Yes. Since August 14, 2026 Anthropic watermarks text from future Claude models for EU AI Act compliance. Anthropic says it uses no hidden characters, adds no extra tokens, has no practical quality impact, and is not traceable to a person, organization, or chat.

## Can I use Fable 5.1 with Zero Data Retention?

Eligible customers can use Fable 5.1 with ZDR until Enterprise Frontier Safeguards reaches their account. EFS, which puts safeguard-related storage under customer control, is phasing in through fall 2026.

## Are incognito chats saved?

They are not saved to chat history or memory, but the support page says they are retained for 30 days by default or longer under applicable organizational retention settings.

## Does Anthropic train on commercial data by default?

The Development Partner Program support article says Anthropic does not train by default on data generated using commercial products such as Claude Enterprise and the Claude API.

## Is every Claude API feature ZDR eligible?

No. Feature docs mark eligibility separately; Files API and MCP connector are examples documented as not ZDR eligible.
