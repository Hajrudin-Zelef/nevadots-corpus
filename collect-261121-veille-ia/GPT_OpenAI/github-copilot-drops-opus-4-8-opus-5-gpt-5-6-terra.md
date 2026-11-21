---
id: collect-261121-veille-ia/gpt-openai/github-copilot-drops-opus-4-8-opus-5-gpt-5-6-terra
title: "GitHub Copilot Drops Opus 4.8, Opus 5, GPT-5.6 Terra"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["opus", "copilot", "models", "github", "model", "claude", "gpt-5", "terra"]
source: https://emergent.sh/news/github-copilot-drops-opus-4-8-opus-5-gpt-5-6
source_anchor: ""
source_lines: [1, 33]
section: "GPT_OpenAI"
---
# GitHub Copilot Drops Opus 4.8, Opus 5, GPT-5.6 Terra

GitHub officially removed three AI models from its Copilot platform on November 16, 2026, affecting users across chat, inline edits, ask and agent modes, and code completions. The deprecated models include Claude Opus 4.8, Claude Opus 5, and GPT-5.6 Terra. Business and Enterprise customers are automatically transitioned to newer alternatives unless administrators explicitly disable them.

## Deprecated Models and Availability

The three removed models no longer appear in the standard GitHub Copilot model picker for most users. Claude Opus 4.8 retains limited availability exclusively for individual GitHub Copilot subscribers on annual plans. Claude Opus 5 has been removed from the primary model selector but remains accessible as a runtime refusal fallback through existing authorized access channels. GPT-5.6 Terra has been fully retired across all Copilot experiences without exception.

GitHub announced the deprecation timeline on October 15, 2026, giving users approximately one month to prepare for the transition. Existing enterprise model policies continue to apply during the changeover period.

## Recommended Replacement Models

GitHub recommends Claude Opus 5.5 and GPT-6.1 Sol as primary replacements for the deprecated models. GPT-6 Luna serves as an additional alternative for teams requiring different performance characteristics. Under default model enablement settings, these alternatives are automatically activated for Copilot Enterprise and Copilot Business customers unless an administrator has disabled the global default or explicitly blocked the specific model.

Users who have turned off the global default can manually enable access to the replacement models through model policies in Copilot settings. Once enabled, the models appear in the Copilot Chat model selector across supported experiences.

## Release Date and Transition Details

Officially launched on November 16, 2026, the model deprecation requires no manual removal action from users. The deprecated models are automatically excluded from new coding sessions and chat interactions. Administrators managing Copilot Enterprise environments can review model policies to ensure teams have access to supported alternatives.

GitHub advises users to update workflows and integrations to reference the new model identifiers. Enterprise customers with questions or concerns are encouraged to contact their account manager for migration assistance.

## Impact on Developers and Teams

The deprecation affects daily coding workflows for developers who relied on the older models for specific tasks or performance profiles. Teams using Claude Opus 4.8 for cost optimization may need to evaluate Opus 5.5 performance and pricing. Organizations that hardcoded model references in API calls or automation scripts must update those integrations to avoid service interruptions.

- Chat and inline edit features automatically route to supported models
- Code completion requests default to the newest available alternatives
- Agent mode workflows transition seamlessly with no code changes required

## What This Means

GitHub's deprecation of three legacy AI models signals a continued push toward newer, more capable alternatives. The move simplifies the model landscape for Business and Enterprise customers while maintaining backward compatibility for annual individual subscribers. Developers should verify that their workflows reference supported models and monitor GitHub's changelog for future deprecation announcements. The automatic enablement of Claude Opus 5.5 and GPT-6.1 Sol ensures most teams experience minimal disruption, though custom integrations may require attention.
