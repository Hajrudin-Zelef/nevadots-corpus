---
id: collect-261121-veille-ia/copilot-microsoft/github-rolls-out-copilot-model-policy-api-amid-a-wave-of-model-deprecations-mail-studio
title: "GitHub Rolls Out Copilot Model Policy API Amid a Wave of Model Deprecations | Mail Studio"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["copilot", "model", "models", "enterprise", "policy", "github", "gpt-5", "organization"]
source: https://www.mail-studio.com/en/ai/copilot-model-policy-api/
source_anchor: ""
source_lines: [1, 46]
section: "Copilot_Microsoft"
---
On October 19, GitHub announced general availability of a REST API dedicated to Copilot model policies. Previously limited to the admin UI, controlling which models are allowed becomes scriptable across an enterprise, an organization, or a team. The announcement lands the same day GitHub retired six legacy Copilot models — not quite a coincidence, since the constant churn of models is making manual management increasingly unworkable for large organizations.

## An API to manage models at enterprise scale

The new API exposes GET and PATCH endpoints to read and change the list of models enabled for Copilot Chat, code completions, agent mode, and inline edits. It replaces manual, seat-by-seat configuration in GitHub’s admin settings with an automatable mechanism: syncing with an internal governance tool, applying an identical policy across several organizations, or bulk-disabling a model deemed non-compliant.

## Four levels of delegation

The system relies on a delegation hierarchy. An enterprise can keep control of the global policy, or delegate it to its organizations, specific teams, or third-party applications.

| Level | REST endpoint | Typical use | 
|---|---|---|
| Enterprise | `/enterprises/{enterprise}/copilot/model-policies` | Global default policy | 
| Enterprise teams | `/enterprises/{enterprise}/teams/{team_slug}/copilot/model-policies` | Delegation to a cross-functional team | 
| Enterprise apps | `/enterprises/{enterprise}/copilot/apps/model-policies` | Control over third-party integrations | 
| Organization | `/orgs/{org}/copilot/model-policies` | Policy specific to one organization | 

In practice, a GET call returns the current state of each model for the targeted scope, and a PATCH enables or disables one in particular:

```
curl -X PATCH \
  -H "Authorization: Bearer $GH_TOKEN" \
  -H "Accept: application/vnd.github+json" \
  https://api.github.com/orgs/acme-corp/copilot/model-policies \
  -d '{"model": "claude-opus-5-5", "state": "enabled"}'
```
## Six models retired on the same day

On October 19, GitHub removed six models from Copilot that had been flagged for deprecation in mid-September: Gemini 3.7 Flash (replaced by Gemini 3.8 Flash), GPT-5.5 and GPT-5.4 (redirected to GPT-5.6 Sol), GPT-5.4 mini and GPT-5 mini (to GPT-5.6 Luna), and Grok 4.5 (to Grok 4.6). The removal covers Copilot Chat, code suggestions, ask mode, and agent mode.

For Enterprise and Business customers, replacement models are enabled by default unless an administrator has turned off the global default setting or disabled the model itself.


An administrator who had disabled the “enable new models by default” setting needs to manually verify, via the API or the UI, that the replacement models (GPT-5.6 Sol, GPT-5.6 Luna, Gemini 3.8 Flash, Grok 4.6) are actually active. Without that check, affected teams lose access to deprecated models with no automatic fallback.

## What it means for development teams

For a technical team, the stakes go beyond a single model’s availability. The new API opens the door to cost and compliance governance managed like code: a versioned policy, reviewed through a pull request, applied automatically when a new organization is onboarded. It fits into a broader trend across the AI agents in production ecosystem, where fine-grained model and access management is becoming an infrastructure concern in its own right, alongside running servers compliant with the MCP protocol.

## Key takeaways

The Copilot model policy API reaches general availability with four delegation levels, from the enterprise down to third-party applications. The same day, six legacy models disappear from Copilot in favor of their successors. Organizations that disabled automatic activation of new models need to check their configuration to avoid a silent service interruption.

This API arrives at the right time. Model deprecation cycles among providers have clearly accelerated in 2026, and managing that by hand, organization by organization, was becoming untenable for Enterprise customers. The real test will be how stable this API stays over time — GitHub has a habit of evolving its admin interfaces without always giving enough advance notice. — **Simon Janvier**

Further reading: the official announcement on the GitHub Changelog.
