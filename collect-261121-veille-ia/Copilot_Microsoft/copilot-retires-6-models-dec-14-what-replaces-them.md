---
id: collect-261121-veille-ia/copilot-microsoft/copilot-retires-6-models-dec-14-what-replaces-them
title: "Copilot Retires 6 Models Dec 14: What Replaces Them"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["gemini", "input", "haiku", "claude", "copilot", "fable", "tokens", "grok"]
source: https://threatfrontier.com/articles/github-copilot-retires-six-models-december-14
source_anchor: ""
source_lines: [1, 55]
section: "Copilot_Microsoft"
---
Copilot drops Fable 5, Haiku 4.5, Gemini 4 Argon and three more on 14 December. Fable and Haiku replacements cost less; Gemini 3.8 Flash's costs double.

GitHub will retire six models from every Copilot experience on 14 December 2026: Claude Fable 5, Claude Haiku 4.5, Gemini 3.8 Flash, Gemini 4 Argon, Grok 4.6 and Kimi K3. The cut covers Chat, inline edits, ask and agent modes, and code completions. Five have a named replacement; Kimi K3's is still to be announced. The swap is not cost-neutral. Copilot bills in AI credits at each model's per-token rate, so moving to the replacement changes what every request draws from your monthly allowance: cheaper for Fable and Haiku users, twice the price for Gemini 3.8 Flash users.

GitHub announced the change on 13 November. It follows two earlier rounds we covered: the six models dropped on 19 October and Opus 4.8, Opus 5 and GPT-5.6 Terra on 16 November.

## What replaces what

| Retiring on 14 December | Suggested replacement | 
|---|---|
| Claude Fable 5 | Claude Fable 5.2 | 
| Claude Haiku 4.5 | Claude Haiku 5.5 | 
| Gemini 3.8 Flash | Gemini 4 Hydrogen | 
| Gemini 4 Argon | Gemini 4 Carbon | 
| Grok 4.6 | Grok 4.7 | 
| Kimi K3 | To be announced | 

One exception: Claude Haiku 4.5 stays available to individual Copilot subscribers on annual plans, and the deprecation does not apply to them. Everyone else loses it on 14 December.

## Why the replacement changes your credit burn

Copilot charges in AI credits, and 1 credit is $0.01. What a request costs depends on the model and on the input, cached and output tokens it uses. Code completions and next edit suggestions are not billed in credits. Individual plans include 1,500 credits a month on Pro ($10), 7,000 on Pro+ ($39) and 20,000 on Max ($100), and unused credits reset at 00:00 UTC on the 1st. (Our Pro vs Pro+ explainer covers how far each allowance goes.)

GitHub's pricing page lists these rates per million tokens (input / cached input / output, USD):

| Model | Rate per 1M tokens | 
|---|---|
| Claude Fable 5 | $10.00 / $1.00 / $50.00 | 
| Claude Fable 5.2 | $8.00 / $0.20 / $40.00 | 
| Claude Haiku 4.5 | $1.00 / $0.10 / $5.00 | 
| Claude Haiku 5.5 | $0.10 / $0.01 / $0.50 up to 100K input; $0.50 / $0.05 / $2.50 above | 
| Gemini 3.8 Flash | $0.75 / $0.075 / $3.75 | 
| Gemini 4 Hydrogen | $1.50 / $0.075 / $7.50 | 
| Gemini 4 Argon | $2.00 / $0.10 / $10.00 | 
| Gemini 4 Carbon | $2.00 / $0.10 / $10.00 | 
| Grok 4.6 and Grok 4.7 | $2.00 / $0.50 / $6.00 up to 200K input; $4.00 / $1.00 / $12.00 above | 
| Kimi K3 | $3.00 / $0.30 / $15.00 | 

*Output price per 1M tokens on GitHub Copilot, retiring model vs its replacement, log scale. Haiku 5.5 rate shown is for up to 100K input tokens; Grok for up to 200K. Source: GitHub Docs.*

Pair by pair:

- **Fable 5 to Fable 5.2: cheaper on every line.** Output drops from $50 to $40 per million tokens, input from $10 to $8, and cached input from $1.00 to $0.20, a fifth of the old rate. At $0.01 a credit, a million output tokens goes from 5,000 credits to 4,000.
- **Haiku 4.5 to Haiku 5.5: a tenth of the price** up to 100K input tokens ($0.50 against $5.00 for output). Above 100K input it is half the Haiku 4.5 rate.
- **Gemini 3.8 Flash to Gemini 4 Hydrogen: double (2x).** Input goes from $0.75 to $1.50 and output from $3.75 to $7.50 per million tokens; cached input stays at $0.075. The same work draws twice the credits on the input and output side.
- **Gemini 4 Argon to Gemini 4 Carbon: the same listed rate, for now.** Both are on promotional pricing that ends on 1 December. After that, GitHub lists Carbon input at $4.00 per million tokens. Gemini 3.8 Flash's own promotion runs to 31 December, but the model leaves Copilot on 14 December.
- **Grok 4.6 to Grok 4.7: no change.** Both are priced at $2.00 / $0.50 / $6.00 up to 200K input.
- **Kimi K3:** no replacement named yet, so there is nothing to compare.

## What admins and users should do

- **Business and Enterprise:** the suggested alternatives are enabled automatically unless an administrator turned off the global default or disabled the model. If you did, enable them in the model policies in Copilot settings before 14 December.
- **Pinned models:** update any workflow, script or integration that names a retiring model.
- **Gemini users:** budget for Hydrogen at twice the Flash rate, and check Carbon usage after 1 December, when its promotional pricing ends.
- **Fable and Haiku users:** the replacements cost less per token, so the same allowance stretches further.
