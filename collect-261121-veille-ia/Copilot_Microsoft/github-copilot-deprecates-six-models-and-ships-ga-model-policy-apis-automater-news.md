---
id: collect-261121-veille-ia/copilot-microsoft/github-copilot-deprecates-six-models-and-ships-ga-model-policy-apis-automater-news
title: "GitHub Copilot Deprecates Six Models and Ships GA Model-Policy APIs | Automater News"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-10-19"]
keywords: ["copilot", "model", "enterprise", "github", "models", "gpt-6", "gpt-5", "model-policy"]
source: https://automater.ai/news/github-copilot-six-models-deprecated-model-policy-api-ga/
source_anchor: ""
source_lines: [1, 45]
section: "Copilot_Microsoft"
---
GitHub pulled six models out of GitHub Copilot on October 19, 2026, and on the same day moved its model-policy REST API to general availability. The pairing is not a coincidence: one change forces every Copilot user and CI agent lane onto newer models, and the other gives administrators the API surface to decide â centrally â which models their fleets may touch at all.

## What was deprecated, and what replaces it

The deprecation notice applies across most Copilot experiences: Copilot Chat, inline edits, ask mode, agent mode, and code completions. The deprecations were announced a month earlier, on September 18, 2026.

| Deprecated model | Deprecation date | Suggested alternative | 
|---|---|---|
| Gemini 3.7 Flash | 2026-10-19 | Gemini 3.8 Flash | 
| GPT-5.5 | 2026-10-19 | GPT-6.1 Sol | 
| GPT-5.4 | 2026-10-19 | GPT-6.1 Sol | 
| GPT-5.4 mini | 2026-10-19 | GPT-6 Luna | 
| GPT-5 mini | 2026-10-19 | GPT-6 Luna | 
| Grok 4.5 | 2026-10-19 | Grok 4.7 | 

The practical consequence is that anything that pinned a model by name â an agent harness config, a CI lane, a shared AGENTS.md convention â now resolves to nothing. GitHub says no action is required to remove the deprecated models; the required action is on your side: update workflows and integrations to the supported set.

For Copilot Enterprise, there is a second gate. Administrators may need to enable access to the alternative models through their **model policies** in Copilot settings. Until the policy is enabled for a specific model, users will not see it in the Copilot Chat model selector in VS Code or on github.com. So a team that âmigratedâ by picking GPT-6.1 Sol in their configs may still be down if the enterprise policy does not allow it.

## The model-policy API, now GA

The same day, GitHub announced that GET and PATCH endpoints for model policies are generally available at four levels:

- Enterprise: `/enterprises/{enterprise}/copilot/model-policies`
- Enterprise teams: `/enterprises/{enterprise}/teams/{team_slug}/copilot/model-policies`
- Enterprise apps: `/enterprises/{enterprise}/copilot/apps/model-policies`
- Organization: `/orgs/{org}/copilot/model-policies`

That means model allowlists are now a managed resource you can script: pin the enterprise to the post-deprecation set, delegate control downward, and audit drift with a GET instead of clicking through Copilot settings. Enterprise policies can delegate model control to organizations in organization mode, or to teams and apps in enterprise teams mode â so the delegation topology you choose determines which of these endpoints actually governs a given lane.

## A concrete migration sequence

1. **Inventory pins.** Grep configs for the six deprecated names â harness configs, CI workflows, docs, and any wrapper that calls Copilot by model.
2. **Rewrite to the replacement lane.** GPT-5.5 and GPT-5.4 to GPT-6.1 Sol; the minis to GPT-6 Luna; Gemini 3.7 Flash to 3.8; Grok 4.5 to 4.7.
3. **Check the policy before blaming the model.** If a replacement model does not appear in the picker, the enterprise model policy is the likely cause. Enable it, or PATCH it via the new API.
4. **Codify the allowlist.** Use the GA endpoints so the next deprecation is a one-line policy change rather than a scavenger hunt â the same continuity drill we recommend in Model Deprecation Routing Is a Continuity Drill, Not a Changelog Line.
5. **Log what actually answered.** A swapped lane is only trustworthy if your logs record the served model, as covered in Who Actually Answered? Log the Served Model.

## One more operational wrinkle

A separate October 19 changelog entry notes that some Git operations over HTTPS may temporarily require authentication. During periods of unusual traffic, GitHub may throttle unauthenticated clones and fetches of public repositories to protect availability. SSH operations, pushes, authenticated HTTPS, and unrelated web/API traffic are unaffected. If your agent fleet clones public repos unauthenticated, expect occasional failures: use the built-in `GITHUB_TOKEN` in Actions, or a GitHub App installation token in other CI systems, and retry operations that genuinely cannot authenticate once the temporary protection lifts.

## Why this matters

Deprecations are routine; the governance pairing is the story. Model access in Copilot is now API-managed policy at four organizational levels, which turns âwhich model may our agents useâ from a per-developer setting into an enterprise control plane â the same shift we have tracked in The Agent Gateway Is the Control Plane for Enterprise Agents. The teams that script their model policies this week will feel the next deprecation as a pull request, not an outage.
