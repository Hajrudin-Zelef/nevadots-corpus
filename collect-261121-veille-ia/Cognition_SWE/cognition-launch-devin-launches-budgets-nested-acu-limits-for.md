---
id: collect-261121-veille-ia/cognition-swe/cognition-launch-devin-launches-budgets-nested-acu-limits-for
title: "Cognition launch — Devin launches budgets: nested ACU limits for"
domain: cognition-swe
role: reference
task: article
actors: ["Cognition"]
dates: ["2026-10-30"]
keywords: ["budgets", "cognition", "budget", "limits", "usage", "devin", "limit", "account"]
source: https://www.usagepricing.com/blueprint/activity/cognition-2026-10-30-enterprise-acu-budgets
source_anchor: ""
source_lines: [1, 19]
section: "Cognition_SWE"
---
Until now an Enterprise admin could cap Devin’s ACU use only per organization, through endpoints the budgets docs now call “deprecated organization-level ACU limit endpoints.” Budgets replace that with a tree: an Account budget at the root, child budgets for departments or projects under it, and per-user limit tiers inside each budget, all reset on the billing phase’s monthly window. Usage in a child counts toward every parent, so a department head can hand out limits without being able to exceed the total central IT set.

**What it means for buyers.** This is a hard stop, not an alert: when any budget on the path runs out, new Devin sessions cannot start, running sessions sleep, and CLI, Desktop and Devin Review requests are blocked until the window resets or an admin raises or temporarily lifts the limit. The feature is in beta and only for Enterprise accounts that ask their account team to turn it on.

## Evidence

1. Fig. 1Cognition's release notes announce budgets that organize user and organization spend in a hierarchy “Launching budgets” Release notes 2026 — Cognition · published Oct 30, 2026
2. Fig. 2Budgets are in beta and must be enabled for the enterprise by the account team “Budgets are in beta” Budgets — Cognition
3. Fig. 3Budgets cap ACU usage with one account limit, shared pools for teams or projects, and per-user limits inside each pool “cap ACU usage the way your company is organized” Budgets — Cognition
4. Fig. 4Each enterprise account has one budget tree, up to three levels deep “up to three levels deep” Budgets — Cognition
5. Fig. 5When a budget runs out, usage stops for its organizations until the next monthly window or a raised limit “usage stops for its orgs” Budgets — Cognition
6. Fig. 6Before budgets, ACU limits were set per organization; the docs now call those organization-level endpoints deprecated “deprecated organization-level ACU limit endpoints” Budgets — Cognition
7. Fig. 7Usage in a child budget counts toward its parent and the Account budget “Usage counts toward every ancestor” Budgets — Cognition
8. Fig. 8Budget limits reset at the start of each monthly window, which follows the billing phase “reset at the start of each monthly window” Budgets — Cognition
9. Fig. 9When a budget runs out, new Devin sessions cannot start and running sessions sleep “running sessions sleep” Budgets — Cognition

UsagePricing, "Cognition: Devin launches budgets: nested ACU limits for Enterprise accounts", observed Oct 30, 2026. https://www.usagepricing.com/blueprint/activity/cognition-2026-10-30-enterprise-acu-budgets

Quote freely with a link back and the date. Bulk reuse is covered by ourTerms; vendor screenshots belong to their owners.How to cite UsagePricing
