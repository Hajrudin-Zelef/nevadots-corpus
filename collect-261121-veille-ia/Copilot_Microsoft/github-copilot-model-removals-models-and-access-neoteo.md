---
id: collect-261121-veille-ia/copilot-microsoft/github-copilot-model-removals-models-and-access-neoteo
title: "GitHub Copilot model removals: models and access | NeoTeo"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-11-16"]
keywords: ["claude", "github", "copilot", "opus", "model", "models", "access", "gpt-5"]
source: https://www.neoteo.com/en/github-copilot-deprecated-three-models-effective-november-16-2026
source_anchor: ""
source_lines: [1, 26]
section: "Copilot_Microsoft"
---
GitHub deprecated Claude Opus 4.8, Claude Opus 5 and GPT-5.6 Terra across most GitHub Copilot experiences effective November 16, 2026. It recommends Claude Opus 5.5 for both Claude models, and GPT-6.1 Sol or GPT-6 Luna for GPT-5.6 Terra. GitHub’s changelog lists the changes for Copilot Chat, inline edits, ask and agent modes, and code completions. GitHub said it had announced the selected deprecations on October 15.

## Which models did GitHub deprecate?

The three models are Claude Opus 4.8, Claude Opus 5 and GPT-5.6 Terra. GitHub’s suggested alternatives are:

| Deprecated model | GitHub-suggested alternative | 
| Claude Opus 4.8 | Claude Opus 5.5 | 
| Claude Opus 5 | Claude Opus 5.5 | 
| GPT-5.6 Terra | GPT-6.1 Sol or GPT-6 Luna | 

These are GitHub’s recommendations for what to use instead; the mapping is not a claim that the models are equivalent.

## What access remains for the Claude models?

Claude Opus 4.8 remains available to individual GitHub Copilot subscribers on annual plans. Claude Opus 5 has been removed from the model picker, but remains available for runtime refusal fallback through existing authorized access. Those are distinct exceptions: one applies to individual subscribers on an annual plan, while the other concerns a specific fallback path.

## What should Copilot administrators check?

For Copilot Enterprise and Copilot Business, GitHub says its suggested alternatives are automatically enabled when the global model default is on, unless an administrator has disabled a model. GitHub says no action is needed to remove the deprecated models. If the global default is off, administrators can enable alternatives through model policies in Copilot settings.

For an organization, check the global model default first: it affects whether GitHub automatically enables the suggested alternatives.

## Does deprecation change code already in a repository?

C# Corner’s general explanation of Copilot model deprecation says it affects future Copilot interactions, not source code already stored in a repository. That is general guidance, rather than a GitHub statement specific to these three deprecations.
