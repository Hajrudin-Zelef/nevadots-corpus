---
id: collect-261121-veille-ia/copilot-microsoft/github-copilot-retires-four-models-and-makes-balanced-its-default-code-review-effort
title: "GitHub Copilot retires four models and makes Balanced its default code review effort"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["github", "copilot", "review", "code", "balanced", "effort", "models", "default"]
source: https://webairadar.com/en/news/github-copilot-retires-four-models-and-makes-balanced-its-default-code-review-effort
source_anchor: ""
source_lines: [1, 48]
section: "Copilot_Microsoft"
---
GitHub published two Copilot changelog entries on October 2, 2026. The first removes four models from every Copilot experience. The second adds API access to Copilot code review and confirms that Balanced is now the default review effort level. The second entry matters for cost, because GitHub's documentation says Balanced reviews use more AI credits than Lite reviews.

## Which models are gone

As of October 2, 2026, four models are deprecated across all GitHub Copilot experiences. GitHub lists Copilot Chat, inline edits, ask and agent modes, and code completions. For each model the changelog names a suggested alternative:

- Gemini 3.5 Flash: the suggested alternative is Gemini 3.8 Flash.
- Gemini 3.6 Flash: the suggested alternative is Gemini 3.8 Flash.
- Kimi K2.7 Code: the suggested alternative is Kimi K3.
- Claude Opus 4.7: the suggested alternative is Claude Opus 5.5.

## What administrators need to do

GitHub asks you to update workflows and integrations that name the deprecated models. No action is required to remove them. Copilot Enterprise administrators may need to enable the alternative models through model policies in Copilot settings. Once a policy is enabled, the model appears in the Copilot Chat model selector in VS Code and on github.com. The changelog entry does not mention a transition period.

## Code review through the API

You can now request a Copilot code review through the REST and GraphQL APIs. Each request can optionally set the review effort level. GitHub says this lets teams start reviews from their own scripts, workflows, and internal tools. The change is generally available on the Copilot Pro, Pro+, Max, Business, and Enterprise plans.

## Balanced is the default, and it costs more than Lite

The Default review effort level now uses Balanced for new and existing repositories and organizations. GitHub announced the change on August 28, 2026, and it took effect on September 28, 2026. If you had explicitly selected Lite, GitHub says that selection was respected.

The documentation describes Lite as a standard review that gives fast, targeted feedback on common issues. Balanced routes pull requests to a higher-reasoning model for longer analysis of complex logic, security-sensitive code, and cross-service changes. GitHub's documentation says Balanced reviews use more AI credits than Lite reviews and may consume marginally more GitHub Actions minutes.

GitHub gives an estimate for both levels. A review typically consumes an estimated $0.05 to $1 worth of AI credits at Lite effort, and $0.25 to $5 at Balanced effort. When a repository requests reviews automatically, the consumption is attributed to the pull request author. When another user requests the review manually, it is attributed to that user.

You can switch from Default to Lite in four places: enterprise, organization, repository, and personal settings. Each level can override the one above it. According to the documentation, an effort level chosen when the review is requested takes precedence over every saved setting. A third level, Max, appears in the settings with a Coming soon label and is not available yet.

## Sources

## Once a week, in your inbox

The whole week's news, gathered into one email. No ads, no passing your address on, one-click unsubscribe.

The address is used only for this roundup. It is not sold and not passed to anyone.

## Related

Google has added Tool Retrieval to the Gemini API in preview. An agent keeps its whole tool library on standby and pulls in a tool's full parameter schema only when a task calls for it, which can cut input tokens for agents with many tools.

Visual Studio Code 1.146 deprecates the Local harness, the older component that runs GitHub Copilot agent sessions in the editor, and removes it in February 2027. Its replacement, the Copilot harness, has been the default for new sessions since version 1.143. The same day, GitHub gave enterprise administrators a setting that limits Copilot customizations to plugins and managed settings.

GitHub Changelog

OpenClaw released version 2026.11.4 on November 16, 2026. When an agent looked at a photo, a camera capture or the screen of a paired phone, the saved image could be attached to its next reply, including in group chats. The release marks those images private. It also gives each viewer of a shared session their own Side chat and stops the iOS app from saving setup codes in plain text.

OpenClaw
