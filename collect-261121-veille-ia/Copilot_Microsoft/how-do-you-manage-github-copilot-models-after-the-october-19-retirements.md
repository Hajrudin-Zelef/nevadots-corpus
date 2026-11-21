---
id: collect-261121-veille-ia/copilot-microsoft/how-do-you-manage-github-copilot-models-after-the-october-19-retirements
title: "How do you manage GitHub Copilot models after the October 19 retirements?"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["models", "model", "gpt-5", "github", "copilot", "enterprise", "october", "default"]
source: https://www.hardiai.com/blog/github-copilot-model-retirements-model-policy-api/
source_anchor: ""
source_lines: [1, 101]
section: "Copilot_Microsoft"
---
## Key takeaways

- GitHub retired six Copilot models on October 19, 2026, including GPT-5.5, GPT-5.4 and Grok 4.5.
- The suggested replacements are now GPT-6.1 Sol, GPT-6 Luna, Gemini 3.8 Flash and Grok 4.7.
- GET and PATCH model policy endpoints are now GA for enterprises, enterprise teams, enterprise apps and organizations.
- The Copilot default policy for new features takes effect on October 22, 2026.

GitHub retired six models from GitHub Copilot on October 19, 2026, including GPT-5.5, GPT-5.4 and Grok 4.5, across chat, completions and agent mode <sup>[1]</sup>. The same day, GitHub made GET and PATCH REST endpoints for Copilot model policies generally available at the enterprise, enterprise team, enterprise app and organization levels <sup>[2]</sup>. Together, these decide which models your agents can switch to, and let you set that in code.

Six models were deprecated on October 19, 2026, and GitHub names a suggested alternative for each <sup>[1]</sup>. The retirement covers most Copilot experiences: Copilot Chat, inline edits, ask and agent modes, and code completions.

| Retired model | Suggested alternative (October 19 entry) | Alternative named on September 18 | 
|---|---|---|
| Gemini 3.7 Flash | Gemini 3.8 Flash | Gemini 3.8 Flash | 
| GPT-5.5 | GPT-6.1 Sol | GPT-5.6 Sol | 
| GPT-5.4 | GPT-6.1 Sol | GPT-5.6 Sol | 
| GPT-5.4 mini | GPT-6 Luna | GPT-5.6 Luna | 
| GPT-5 mini | GPT-6 Luna | GPT-5.6 Luna | 
| Grok 4.5 | Grok 4.7 | Grok 4.6 | 

The third column matters if you planned your migration a month ago. The September 18 announcement suggested GPT-5.6 Sol, GPT-5.6 Luna and Grok 4.6 <sup>[3]</sup>. The October 19 entry points to newer models instead <sup>[1]</sup>. GitHub's supported models page lists both sets as generally available: GPT-6.1 Sol, GPT-6 Luna, Gemini 3.8 Flash and Grok 4.7, as well as GPT-5.6 Sol, GPT-5.6 Luna and Grok 4.6 <sup>[7]</sup>. If you are choosing a target today, the October 19 list is the current advice.

You do not need to do anything to remove the retired models, according to GitHub <sup>[1]</sup>. What you may need to do is enable the alternatives. Once a model is enabled through your model policies, it shows up in the Copilot Chat model selector in VS Code and on github.com.

Usually yes, but only if you left the default on. Under default model enablement, Copilot Business and Copilot Enterprise customers receive the alternatives automatically, unless an administrator turned off the global default or disabled the specific model <sup>[3]</sup>.

The setting behind this is the "Default availability for released models" policy. A new generally available model follows it until someone configures that model explicitly <sup>[6]</sup>. Some models never follow it: pre-GA models, open-weight models and models not covered by GitHub's data retention agreement are disabled by default. Enterprises that restrict models for data residency or FedRAMP also skip models that do not meet those restrictions.

Model policies flow down from an enterprise baseline. At the enterprise level, each model is Enabled, Disabled or delegated to a lower level. Models nobody has configured show "Delegate to Default Policy" and follow the default availability policy <sup>[5]</sup>.

Delegation works in one of two modes:

- **Organization mode** is the default. Enterprise owners delegate per-model decisions to organizations, and organization owners set each model to enabled or disabled. An organization that leaves a model unconfigured falls back to "Default availability for released models"<sup>[5]</sup> .
- **Enterprise teams mode** is an opt-in preview. Organization-level model settings are turned off, and models are enabled for specific enterprise teams. You can roll back during the preview, but enterprise-level policy changes made after switching are not preserved<sup>[5]</sup> .

In teams mode, the baseline can only be added to. A model disabled at the enterprise level cannot be enabled for a team, and a model enabled at the enterprise level is always enabled for every team. Teams have no "disabled" option. A person in several teams gets every model that any of their teams enables <sup>[5]</sup>.

Agent apps get a rule of their own. They use the models enabled in the organization or personal account that installed them, and in teams mode their models are controlled at the enterprise level <sup>[5]</sup>. If a third-party agent app on your repositories stopped working on October 19, check which models are enabled for its installing organization first.

The model policy API lets you read and change the same settings that sit under AI controls, then Copilot, then Configure models in the enterprise settings <sup>[5]</sup>. The changelog lists GET and PATCH at four paths <sup>[2]</sup>:

| Level | Path | 
|---|---|
| Enterprise | `/enterprises/{enterprise}/copilot/model-policies` | 
| Enterprise teams | `/enterprises/{enterprise}/teams/{team_slug}/copilot/model-policies` | 
| Enterprise apps | `/enterprises/{enterprise}/copilot/apps/model-policies` | 
| Organization | `/orgs/{org}/copilot/model-policies` | 

The REST reference documents the enterprise, team and app endpoints with API version `2026-03-10` <sup>[4]</sup>. For those three endpoints, reads need the `read_enterprise_ai_controls` permission and writes need `write_enterprise_ai_controls`; enterprise owners also have access. Classic personal access tokens and OAuth app tokens need the `read:enterprise` scope to read and `admin:enterprise` to write <sup>[4]</sup>.

The allowed values differ by level <sup>[4]</sup>:

| Endpoint | Values you can PATCH | Notes | 
|---|---|---|
| Enterprise | `enabled` ,`disabled` ,`delegate_to_teams_and_apps` ,`delegate_to_organizations` per model;`enabled` ,`disabled` ,`delegate_to_organizations` for`default_availability` | `delegate_to_organizations` only works for enterprises that manage Copilot through organizations | 
| Enterprise teams | `enabled` ,`disabled` ,`optional` | `optional` removes the team's policy; locked models cannot be updated | 
| Enterprise apps | `enabled` ,`disabled` ,`optional` | `optional` removes the apps policy; locked models cannot be updated | 

On a GET, the enterprise endpoint can also return `delegate_to_default` and `unconfigured`. The team and app endpoints return each model's `enterprise_policy_state` and an `editable` flag, so a script can skip models it is not allowed to change <sup>[4]</sup>.

Start by reading the current enterprise policy, then enable the models you want by their `model_name`. The example below uses the GitHub CLI so the token stays in its own store. Replace `your-enterprise` with your enterprise slug.

Each entry in the response has a `model_name`, a `display_name` and a `policy_state` <sup>[4]</sup>. Copy the `model_name` of each replacement from that list, because the reference shows only one identifier, `gpt-5.6-sol`, in its examples. Then send a PATCH:

A short checklist for this week:

1. Read the enterprise policy and confirm whether default availability is `enabled` ,`disabled` or delegated.
2. If it is not enabled, turn on the October 19 alternatives you want: GPT-6.1 Sol, GPT-6 Luna, Gemini 3.8 Flash and Grok 4.7 <sup>[1]</sup> .
3. Search your agent configurations, prompt files and scripts for the retired model names and switch them.
4. Decide on the "Default policy for new features" before October 22, 2026 <sup>[6]</sup> .
5. Commit the PATCH bodies to a repository so model access changes go through review.

For teams that give coding agents real work, a retired model is a silent configuration change. An agent mode session or an agent app that was pinned to GPT-5.4 needs a model it is allowed to use, and that depends on policies set two or three levels up <sup>[5]</sup>.

The API turns that into something you can audit. A nightly GET against each level can compare the enabled models with the list your agents expect and flag the difference before someone notices a failed run. Teams that coordinate several agents in a shared workspace such as Hardi AI can post that diff in the channel where the agents' owners already talk.

The documentation has not fully caught up with the changelog. The changelog writes the team path with `{team_slug}` <sup>[2]</sup>, while the REST reference documents `{team_id}` as an integer <sup>[4]</sup>. Test both against a non-production team before you script it. We could not find a reference page for the organization endpoint, so its exact body is not documented here.

Model identifiers are the other gap. The supported models page uses display names only <sup>[7]</sup>, so read `model_name` values from a GET rather than guessing them. Enterprise teams mode is still a preview, and rolling back drops enterprise-level policy changes made after the switch <sup>[5]</sup>. Finally, GitHub changed its suggested alternatives between September 18 and October 19, so treat any list of replacements as current as of October 20, 2026.

## Frequently asked questions

### Which GitHub Copilot models were deprecated on October 19, 2026?

Gemini 3.7 Flash, GPT-5.5, GPT-5.4, GPT-5.4 mini, GPT-5 mini and Grok 4.5. The retirement covers Copilot Chat, inline edits, ask and agent modes, and code completions.

### What should I use instead of GPT-5.4 in GitHub Copilot?

GitHub's October 19, 2026 changelog suggests GPT-6.1 Sol for GPT-5.5 and GPT-5.4, and GPT-6 Luna for GPT-5.4 mini and GPT-5 mini. The September 18 announcement had named GPT-5.6 Sol and GPT-5.6 Luna instead.

### Do I have to remove deprecated Copilot models myself?

No. GitHub says no action is required to remove them. You may need to enable the replacement models in your model policies if default availability is turned off or the model was disabled.

### Is there an API for Copilot model policies?

Yes. Since October 19, 2026, GET and PATCH endpoints for Copilot model policies are generally available for enterprises, enterprise teams, enterprise apps and organizations. For the enterprise, team and app endpoints, writes need the write_enterprise_ai_controls permission or, for classic tokens, the admin:enterprise scope.

### Can a team enable a Copilot model that the enterprise disabled?

No. In enterprise teams mode the enterprise baseline is additive: a model disabled at the enterprise level cannot be enabled for a team, and teams can only add models on top of the baseline.
