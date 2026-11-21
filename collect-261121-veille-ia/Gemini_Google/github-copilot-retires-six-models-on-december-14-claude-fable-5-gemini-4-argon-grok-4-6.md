---
id: collect-261121-veille-ia/gemini-google/github-copilot-retires-six-models-on-december-14-claude-fable-5-gemini-4-argon-grok-4-6
title: "GitHub Copilot retires six models on December 14: Claude Fable 5, Gemini 4 Argon, Grok 4.6"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-06-01"]
keywords: ["copilot", "model", "github", "claude", "gemini", "models", "fable", "december"]
source: https://www.datastudios.org/post/github-copilot-retires-six-models-december-14-claude-fable-5-gemini-4-argon
source_anchor: ""
source_lines: [1, 223]
section: "Gemini_Google"
---
GitHub Copilot retires six models on December 14, including Claude Fable 5

6 days ago

8 min read

GitHub Copilot will remove six AI models from every Copilot experience on December 14, 2026, GitHub announced in its official changelog on November 13. The list covers Claude Fable 5, Claude Haiku 4.5, Gemini 3.8 Flash, Gemini 4 Argon, Grok 4.6 and Kimi K3, and the deprecation applies across Copilot Chat, inline edits, ask and agent modes, and code completions. GitHub names a suggested successor for five of the six models and says the replacement for Kimi K3 will be announced later.

The change reaches every paid plan, with one carve-out. Claude Haiku 4.5 stays available to individual Copilot subscribers on annual plans, and GitHub states that the Haiku 4.5 deprecation does not apply to them. Business and Enterprise customers do not get that exception. For organizations that kept default model enablement switched on, the suggested alternatives are turned on automatically, while administrators who disabled the global default have to enable each replacement through its model policy in Copilot settings.

The swap also changes what each request costs. Since June 1, 2026 Copilot has billed model use at provider list prices under usage-based billing, with 1 AI credit equal to $0.01, so moving from a retired model to its successor can make the same coding session cheaper, identical in price, or twice as expensive, depending on which model a team relied on.

··········


THE SIX GITHUB COPILOT MODELS RETIRING ON DECEMBER 14.

GitHub's changelog pairs each retiring model with a recommended replacement. Three of the pairs are straightforward version upgrades inside the same family, one moves Google users from its previous flagship to its newer frontier model, one moves Flash users to Google's newest Flash model, and one has no successor yet.

........

Retiring model

Suggested replacement

Replacement plans in Copilot

Claude Fable 5

Claude Fable 5.2

Pro+, Max, Business, Enterprise

Claude Haiku 4.5

Claude Haiku 5.5

Not stated in the changelog

Gemini 3.8 Flash

Gemini 4 Hydrogen

Pro, Pro+, Max, Business, Enterprise

Gemini 4 Argon

Gemini 4 Carbon

Pro+, Max, Business, Enterprise

Grok 4.6

Grok 4.7

Not stated in the changelog

Kimi K3

To be announced

Not applicable

........

The plan column comes from the separate GitHub changelog entries that introduced each replacement: Claude Fable 5.2 on November 9, Gemini 4 Carbon on November 10 and Gemini 4 Hydrogen on November 13. Copilot Pro does not appear in the plan lists for Claude Fable 5.2 or Gemini 4 Carbon, so a Pro subscriber who loses access to a retiring model will find Gemini 4 Hydrogen as the newest Google option in the picker, while the two premium successors require Pro+ or a higher tier. GitHub's supported-models documentation also states that Copilot Free and Copilot Student users reach models only through automatic model selection, so those users will see the change through the router rather than the model picker.

GitHub's documentation still listed all six retiring models as generally available when this story was verified on November 14, and none of them yet appeared in the retirement table of the supported-models page. The retirement date therefore rests on the November 13 changelog entry, which is GitHub's official notice.

··········

WHAT COPILOT BUSINESS AND ENTERPRISE ADMINS NEED TO DO BEFORE DECEMBER 14.

GitHub asks customers to move workflows and integrations to supported models before the cutoff and says no action is needed to remove the old models afterwards. The work for administrators depends on how model policies are configured in each organization.

Organizations that kept default model enablement turned on will see the suggested alternatives switched on automatically, unless an administrator has explicitly disabled a particular model. Organizations that turned off the global default have to open Copilot settings and enable each replacement model policy by hand. Once a policy is enabled, the model appears in the Copilot Chat model selector in the supported Copilot clients.

Claude Fable 5.2 follows a stricter rule than the other replacements. GitHub's November 9 changelog says the Fable 5.2 policy is off by default, that Business and Enterprise administrators must enable it in Copilot settings, and that enabling it acknowledges a data retention requirement: Anthropic retains prompts and outputs for Fable 5, 5.1 and 5.2 to run safety classifiers, and GitHub says that retained data is not used to train Anthropic's models. Eligible enterprises can use the Fable models with zero data retention through the end of 2026 under a time-bound exemption, after which continued use requires Enterprise Frontier Safeguards, with eligibility handled by the GitHub account team. A company that relied on Claude Fable 5 therefore needs a deliberate policy decision before December 14, since the automatic enablement path described for the other models does not cover a policy that ships switched off. Our earlier report on Claude Fable 5.2 in GitHub Copilot and its data retention rules covers those terms in more detail.

Kimi K3 users have no announced landing spot. GitHub's documentation describes Kimi K3 as an open-weight model that is disabled by default regardless of the enterprise default-availability policy, so the teams using it had already opted in on purpose and will need to pick another model themselves until GitHub names a successor.

··········

GITHUB COPILOT PRICES FOR THE RETIRING MODELS AND THEIR REPLACEMENTS.

Copilot bills each model at the provider's list price per 1M tokens. The table below shows the rates for the retiring models as listed on GitHub's "Models and pricing for GitHub Copilot" page, and the replacement rates as published by GitHub for Claude Haiku 5.5 and Grok 4.7 and by the model providers for the other three, since GitHub's pricing page did not yet list Claude Fable 5.2, Gemini 4 Hydrogen or Gemini 4 Carbon on November 14. Prices were checked on the official pages on November 14, 2026.

........

Model pair

Input before / after

Cached input before / after

Output before / after

Claude Fable 5 to Fable 5.2

$10.00 / $8.00

$1.00 / $0.20

$50.00 / $40.00

Claude Haiku 4.5 to Haiku 5.5

$1.00 / $0.10

$0.10 / $0.01

$5.00 / $0.50

Gemini 3.8 Flash to 4 Hydrogen

$0.75 / $1.50

$0.075 / not stated

$3.75 / $7.50

Gemini 4 Argon to 4 Carbon

$2.00 / $2.00

$0.10 / not stated

$10.00 / $10.00

Grok 4.6 to Grok 4.7

$2.00 / $2.00

$0.50 / $0.50

$6.00 / $6.00

........

Several qualifications apply to these figures. The Claude Haiku 5.5 rates are its default tier for prompts up to 100,000 tokens; above that threshold GitHub lists $0.50 input and $2.50 output. Grok 4.6 and 4.7 rates are for prompts up to 200,000 tokens, with $4.00 input and $12.00 output above it. Claude Fable 5.2's figures are Anthropic's API list prices, which GitHub says it charges for the model in Copilot. Gemini 4 Hydrogen's $1.50 and $7.50 are the flat prices Google announced on November 12 across the model's full 1M-token context window. Gemini 4 Carbon's $2 and $10 are introductory prices: Google's launch post says they rise to $4 input and $20 output after the introductory period, without giving an end date.

Two Google promotions also shape the comparison. GitHub's pricing page states that Gemini 3.8 Flash is billed at promotional rates of $0.75 input and $3.75 output through December 31, 2026, and that Gemini 4 Argon's promotional rates of $2.00 and $10.00 run until December 1, 2026, two weeks before the model leaves Copilot. GitHub does not publish Argon's post-promotion rate on that page.

··········

[[ADPLUS_300x250_1]]

COST OF A COPILOT AGENT SESSION BEFORE AND AFTER THE MODEL SWITCH.

To make the price changes concrete, consider a single agent-mode session that sends 400,000 input tokens and receives 40,000 output tokens, with no cached tokens counted, so every model is measured on the same basis. At 1 AI credit per cent, the session costs the following on each pair.

........

Model pair

Cost per session before

Cost per session after

100 sessions before / after

Claude Fable 5 to Fable 5.2

$6.00 (600 credits)

$4.80 (480 credits)

$600 / $480

Claude Haiku 4.5 to Haiku 5.5

$0.60 (60 credits)

$0.06 (6 credits)

$60 / $6

Gemini 3.8 Flash to 4 Hydrogen

$0.45 (45 credits)

$0.90 (90 credits)

$45 / $90

Gemini 4 Argon to 4 Carbon

$1.20 (120 credits)

$1.20 (120 credits)

$120 / $120

Grok 4.6 to Grok 4.7

$1.04 (104 credits)

$1.04 (104 credits)

$104 / $104

........

The arithmetic favours teams that used Anthropic's models. Moving from Claude Fable 5 to Fable 5.2 cuts the session cost by 20 percent, and moving from Claude Haiku 4.5 to Haiku 5.5 cuts it by 90 percent as long as prompts stay under 100,000 tokens. Grok users see no change, and Gemini 4 Argon users pay the same while Carbon's introductory pricing lasts; once Carbon moves to $4 and $20, the same session would cost $2.40, or 240 credits, double the Argon figure.

Gemini 3.8 Flash users face the clearest increase. At Copilot's promotional Flash price, the sample session costs $0.45; on Gemini 4 Hydrogen at Google's list price it costs $0.90. Google says partners measured 12 to 23 percent lower token usage with Hydrogen on some workloads and that Hydrogen completes tasks 2.5 times faster than 3.8 Flash at its default setting. Applying those token reductions to the same session gives roughly $0.69 to $0.79, still 54 to 76 percent above the Flash figure, and those savings are Google's partner results rather than a guarantee for any given codebase. Teams that leaned on Flash for high-volume work should watch their AI credit consumption during the first weeks on Hydrogen. Our guide to AI coding agent pricing across Cursor, Claude Code, Codex and GitHub Copilot sets these per-token rates against the subscription plans of the competing tools.

··········


WHY CLAUDE HAIKU 4.5 SURVIVES FOR ANNUAL GITHUB COPILOT SUBSCRIBERS.

The single exception in GitHub's notice follows from the billing change of June 1, 2026. GitHub's documentation on legacy billing explains that its multipliers apply to Copilot Pro and Pro+ subscribers on an existing annual plan who remained on the older premium-request billing model after June 1, and states that users on legacy annual plans will not receive access to new models and features. Claude Haiku 4.5 is one of the few models in the legacy multiplier table, priced at 0.33 premium requests per interaction.

Because those subscribers do not receive new models, retiring Haiku 4.5 for them would remove a model from their plan without giving them Haiku 5.5 in its place. GitHub has not spelled out its reasoning, but keeping Haiku 4.5 alive for that group preserves the model list they paid for, while organization plans and monthly individual plans on usage-based billing move to Haiku 5.5.

··········

GITHUB COPILOT DECEMBER 14 RETIREMENTS: CONFIRMED FACTS AND OPEN QUESTIONS.

GitHub has confirmed the date, the six models, the surfaces affected, the suggested successors for five of them, the Haiku 4.5 exception for individual annual subscribers and the automatic enablement rule for organizations that kept default model enablement. The replacement models are already generally available in Copilot, with Gemini 4 Hydrogen the only one offered on Copilot Pro as well as the higher tiers. The price effect is measurable from official rates: cheaper for Claude users, unchanged for Grok users and, for now, for Argon users, and roughly double per token for teams moving from Gemini 3.8 Flash's promotional price to Hydrogen.

Several points remain open. GitHub has not named a successor for Kimi K3, has not yet added Claude Fable 5.2, Gemini 4 Hydrogen or Gemini 4 Carbon to its own pricing table, and has not said whether Claude Fable 5.2 or Gemini 4 Carbon will ever reach Copilot Pro, whose plan list for both models is currently empty. Google has not dated the end of Gemini 4 Carbon's introductory pricing, which decides whether the Argon to Carbon move stays cost-neutral. Organizations using Claude Fable 5 also face a policy step that automatic enablement does not cover, because Fable 5.2 ships switched off and requires an administrator to accept Anthropic's data retention terms before December 14.
