---
id: collect-261121-veille-ia/copilot-microsoft/github-copilot-model-deprecations-on-dec-14-2026-what-to-switch-to
title: "GitHub Copilot Model Deprecations on Dec 14, 2026: What to Switch To"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["github", "copilot", "model", "models", "chat", "click", "claude", "gemini"]
source: https://dev.to/epifive/github-copilot-model-deprecations-on-dec-14-2026-what-to-switch-to-138b
source_anchor: ""
source_lines: [1, 110]
section: "Copilot_Microsoft"
---
GitHub announced on November 13, 2026 that it will deprecate six models across all GitHub Copilot experiences on December 14, 2026: Claude Fable 5, Claude Haiku 4.5, Gemini 3.8 Flash, Gemini 4 Argon, Grok 4.6 and Kimi K3. That is 30 days from today (November 14, 2026). To get ready, open the model picker in your editor, switch to a supported model, and, if you manage an organization, review its Copilot model policies.

GitHub's changelog post, "Upcoming deprecation of selected GitHub Copilot models in December", says the six models will be deprecated "across all GitHub Copilot experiences (including Copilot Chat, inline edits, ask and agent modes, and code completions)" on December 14, 2026. After that date the models are no longer available to choose in those experiences.

This is not a change to Copilot itself. Copilot keeps working; only the list of selectable models shrinks. The practical risk is anything that depends on a specific model: a model you picked by habit in the chat picker, a team convention ("use this model for agent mode"), or documentation and scripts that name one.

GitHub's suggested alternatives, as listed in the announcement:

| Deprecated model | Deprecation date | Suggested alternative | 
|---|---|---|
| Claude Fable 5 | 2026-12-14 | Claude Fable 5.2 | 
| Claude Haiku 4.5 | 2026-12-14 | Claude Haiku 5.5 | 
| Gemini 3.8 Flash | 2026-12-14 | Gemini 4 Hydrogen | 
| Gemini 4 Argon | 2026-12-14 | Gemini 4 Carbon | 
| Grok 4.6 | 2026-12-14 | Grok 4.7 | 
| Kimi K3 | 2026-12-14 | To be announced | 

The announcement says the deprecation covers all Copilot experiences, so it is not limited to one editor or one feature. Which models you can see today still depends on your plan and where you use Copilot, as GitHub's documentation on changing the chat model notes ("such as GitHub.com or an IDE"). If you never selected one of the six models, you will probably not notice anything.

| Who | What GitHub says | 
|---|---|
| Individual Copilot subscribers on annual plans | Claude Haiku 4.5 is the exception: the deprecation does not apply to these subscribers for that model. The announcement does not list a similar exception for the other five models. | 
| Everyone else | The six models are deprecated on December 14, 2026. | 
| Copilot Business and Enterprise organizations | Suggested alternatives are enabled automatically under default model enablement, unless an administrator has turned off the global default or disabled the model. | 
| Enterprise customers with questions | GitHub points them to their account team. | 

The announcement does not mention regional differences, and the changelog post gives no prices. We did not find a separate price change tied to this deprecation on the pages we opened.

- A GitHub Copilot subscription (individual, Business or Enterprise).
- For individuals: access to the model picker in the Copilot Chat client you use.
- For Copilot Business: GitHub's documentation says your organization must allow members to switch models.
- For admins: organization owner rights to open **Settings** for the organization and manage Copilot model policies. If an enterprise owner has already set a model policy, GitHub's documentation says you "cannot override that setting at the organization level".

GitHub's announcement asks users to update workflows and integrations to supported models before the deprecation date. The steps below come from GitHub's documentation page on changing the chat model, opened on November 14, 2026.

In Visual Studio Code:

1. Click the chat icon in the title bar.
2. At the bottom of the chat view, click the **CURRENT-MODEL** dropdown, then click the AI model you want.

In Visual Studio:

1. In the menu bar, click **View** , then**GitHub Copilot Chat** .
2. In the bottom right of the chat view, click the **CURRENT-MODEL** dropdown, then click the AI model you want.

In JetBrains IDEs:

1. Click the **Copilot** icon in the status bar.
2. In the popup menu, click **Open GitHub Copilot Chat** .
3. In the bottom right of the chat view, click the **CURRENT-MODEL** dropdown, then click the AI model you want.
4. Optional: for a reasoning model that supports it, hover over the model and select an effort level in the **Thinking Effort** submenu.

In Eclipse IDE:

1. Click the **Copilot** icon in the status bar.
2. In the popup menu, click **Open Chat** .
3. In the bottom right of the chat panel, click the current model, then select another model from the popup menu.

In Xcode (Multi-model Chat requires the GitHub Copilot for Xcode extension):

1. In the menu bar, click **Editor** , then**Copilot** , then**Open Chat** . Chat opens in a new window.
2. In the bottom right of the chat view, click the **CURRENT-MODEL** dropdown, then click the AI model you want.

As of November 14, 2026, the documentation page we opened did not give steps for choosing a model on GitHub.com. It says only that available models depend on your plan and where you use Copilot. We have left those steps out rather than guess menu names.

GitHub's documentation gives this path for an organization owner:

1. Click your profile picture in the upper-right corner of GitHub, then choose **Organizations** .
2. Click the organization you want to manage.
3. Under the organization name, open **Settings** . If you do not see it, use the**More** (three-dot) menu and select**Settings** there.
4. In the sidebar, under "Code, planning, and automation," click **Copilot** , then click**Models** .
5. For each model policy you want to set, open its dropdown and select an enforcement option.

The documentation does not list the enforcement option names, so read the dropdown on screen. It also notes that model policies beyond the basic models may incur additional costs.

The announcement explains what happens automatically. For Enterprise and Business customers, the suggested alternatives are enabled "under default model enablement", unless an administrator has turned off the global default or disabled the model. If the global default is off, admins can enable the alternatives through model policies in Copilot settings. So if your organization turned the default off, the replacement models will not appear on their own, and users may be left with fewer choices after December 14, 2026.

1. List which of the six models your team actually uses. Ask in chat or check shared documentation, since the announcement does not describe a usage report.
2. Pick the suggested alternative for each, or another supported model from GitHub's supported models page.
3. Update team conventions, onboarding notes and any integrations that name a deprecated model.
4. Admins: confirm the replacement models are allowed in the organization's model policies, and check for an enterprise-level policy that overrides it.
5. Revisit on or after December 14, 2026 to confirm the picker shows only supported models.

- GitHub has not named a replacement for Kimi K3. The announcement says "to be announced"; pick another supported model for now.
- The Claude Haiku 4.5 exception is only for individual subscribers on annual plans. Monthly individual plans, Business and Enterprise are not covered by it, according to the announcement.
- The announcement does not say whether one replacement behaves like the model it replaces. Quality, speed and cost can differ, so test your own prompts on the suggested model.
- Whether a replacement counts the same against any usage limits or premium request allowances is not stated in the changelog post. Check your plan's documentation.
- We could not read the deprecation sections of GitHub's long supported models reference page in this run (it is very large and our fetch covered only part of it), so we rely on the changelog post for the list and dates.

The announcement says nothing about data use or training for these models, and this guide does not make any claims about it. Model providers differ, so admins who restrict models for compliance reasons should review each replacement model before allowing it in their organization's model policies.

For governance, the key control is the organization **Settings** > **Copilot** > **Models** page described above. Because the automatic enablement of alternatives depends on default model enablement and on whether a model was disabled, an organization that deliberately disabled specific models keeps that choice. Replacement models that were never enabled will not be enabled for you in that case.

- Opened the GitHub Changelog announcement on 2026-11-14 and confirmed the six models, the shared deprecation date of December 14, 2026, and the suggested alternatives.
- Confirmed the Claude Haiku 4.5 exception for individual annual subscribers and the Business and Enterprise default-enablement wording in the same announcement.
- Opened GitHub's "change the chat model" documentation on 2026-11-14 and copied the VS Code, Visual Studio, JetBrains, Eclipse and Xcode steps and labels from it.
- Opened GitHub's documentation on managing Copilot model policies for an organization on 2026-11-14 and copied the Settings > Copilot > Models path.
- Counted the days from 2026-11-14 to 2026-12-14: 30 days.
- Not verified: steps for choosing a model on GitHub.com, the enforcement option names in the model policy dropdown, and whether each suggested model is available on every plan. Not tested on a live Copilot account.

Last tested: 2026-11-14

- GitHub Changelog, "Upcoming deprecation of selected GitHub Copilot models in December" (November 13, 2026): https://github.blog/changelog/2026-11-13-upcoming-deprecation-of-selected-github-copilot-models-in-december
- GitHub Docs, supported AI models in Copilot: https://docs.github.com/copilot/reference/ai-models/supported-models
- GitHub Docs, change the chat model: https://docs.github.com/en/copilot/how-tos/use-ai-models/change-the-chat-model
- GitHub Docs, manage Copilot policies for an organization: https://docs.github.com/en/copilot/how-tos/administer-copilot/manage-for-organization/manage-policies

*This guide was researched and drafted with AI assistance and checked against the vendor documentation listed in its References. Read the maintained version and related guides on EpiFive.*

## Top comments (0)
