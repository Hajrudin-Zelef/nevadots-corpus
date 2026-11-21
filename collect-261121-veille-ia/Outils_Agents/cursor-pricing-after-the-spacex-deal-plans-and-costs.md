---
id: collect-261121-veille-ia/outils-agents/cursor-pricing-after-the-spacex-deal-plans-and-costs
title: "Cursor Pricing After the SpaceX Deal: Plans and Costs"
domain: outils-agents
role: reference
task: article
actors: []
dates: ["2026-10-21"]
keywords: ["cursor", "plans", "pricing", "says", "legacy", "teams", "plan", "ultra"]
source: https://benchclaw.io/cursor-pricing/
source_anchor: ""
source_lines: [1, 144]
section: "Outils_Agents"
---
**Google’s AI Overview for “cursor pricing”, and four of the six pricing pages we could read, describe plans that Cursor now calls legacy.** The Overview lists Pro at $20, Pro+ at $60, Ultra at $200 and Teams at $40. Cursor’s own documentation calls those “legacy Cursor plans” and says new customers start on The X Subscription: Free, Plus at $8, Premium at $30, Super at $100 and Ultra at $200. Cursor announced on 14 August 2026 that SpaceX had acquired it, and it is now sold together with Grok and X. We read Cursor’s pricing page and documentation on 2026-10-21 and ran no Cursor session, so this page says nothing about how much work a plan buys.

Our methodology says how we treat evidence. The script, its raw output and the saved Google AI Overview are in the evidence directory of our public harness repository, and two runs of the script produced byte-identical output. Prices change, so every figure below is dated.

## How much does Cursor cost now?

Cursor’s pricing page and documentation list these individual plans on 2026-10-21:

| Plan | Monthly | Annual | Included usage | Third-party models (Claude, GPT, Gemini) | 
|---|---|---|---|---|
| Free | $0 |  | Limited | Not available | 
| Plus | $8 | $80 | Base weekly pool | Not available | 
| Premium | $30 | $300 | 4x Plus | At API list price, from prepaid credit | 
| Super | $100 | $1,000 | 15x Plus | At API list price, from prepaid credit | 
| Ultra | $200 | $2,000 | 50x Plus | At API list price, from prepaid credit | 

Annual billing saves 16.7% on every paid plan. Teams are priced per seat at $30, $100 or $200 a month (Premium, Super or Ultra seats at the same price and usage as the individual plans), and Enterprise is custom. Plus is not available for teams. Cursor states that prices exclude tax. The pricing page names X Subscription Corp as the seller and says Cursor authorises no resellers.

Plus is narrower than it looks. Cursor’s documentation says it runs Cursor’s own models (Grok 4.7, Grok 4.6, Grok 4.5 and Composer 2.5) at fixed settings, with “no Auto, Cloud Agents, Automations, Bugbot, Origin, third-party models, or your own API keys”. Our Cursor alternatives page compares Cursor with other tools.

## Why do some Cursor pricing pages show different plans?

Because Cursor changed its plans. Its documentation says legacy plans, “the plans Cursor sold before The X Subscription”, are Hobby, Start, Pro, Pro+, Ultra and Teams with Standard and Premium seats. They “keep working as they are”, but “new customers start on The X Subscription”. The legacy page maps each to the nearest current plan:

| Legacy plan | Legacy price | Closest current plan | Current price | Change | 
|---|---|---|---|---|
| Pro | $20 a month | Premium | $30 | +50% | 
| Pro+ | $60 a month | Super | $100 | +66.7% | 
| Ultra | $200 a month | Ultra | $200 | none | 
| Teams Standard | $40 a seat | Teams, Premium seat | $30 | -25% | 
| Teams Premium | $120 a seat | Teams, Super seat | $100 | -16.7% | 

Cursor says comparable means “the closest tier, not an identical one”. It also says each paid plan on The X Subscription “includes at least twice as much coding usage with Cursor models as the legacy plan it replaces”, adds Grok and X benefits, and “drops the included third-party model allowance”. Whether the individual changes net out as a rise or a cut for you depends on how much third-party model usage you have. We cannot compute that for you.

Seven pages rank for this query or are cited by Google’s AI Overview. We fetched them: six returned text and one returned HTTP 403. None of the six mentions The X Subscription, and four of them still list Pro+. That does not make them wrong about legacy plans. It means a reader who lands on them will not learn that new customers see different plans.

## Who owns Cursor, and why does it come with Grok and X?

Cursor’s own blog says: “Cursor has officially been acquired by SpaceX. This completes the acquisition process that started in April, when we announced our partnership with SpaceXAI”. The post is dated 14 August 2026. Cursor’s documentation now says it “is sold as part of The X Subscription, one subscription for Grok, Cursor, and X”. The paid plans share one weekly usage pool across Grok and Cursor, and the pricing page lists X features such as a Premium checkmark and fewer timeline ads next to the coding features. Switching a legacy account to The X Subscription is “optional and permanent”, and Cursor’s help page says “It can’t be undone.”

## Which Cursor plan gives the most usage per dollar?

Cursor does not state its weekly pool in tokens or dollars, only as multiples of Plus. Using those multiples, the price of one Plus-sized pool falls with each tier:

| Plan | Price | Pool, as a multiple of Plus | Price per Plus-sized pool | 
|---|---|---|---|
| Plus | $8 | 1x | $8.00 | 
| Premium | $30 | 4x | $7.50 | 
| Super | $100 | 15x | $6.67 | 
| Ultra | $200 | 50x | $4.00 | 

This is our arithmetic on published figures, and it is relative only. Plus lacks Auto, Cloud Agents and third-party models, so a Plus pool and an Ultra pool do not buy the same things. It does show why Cursor recommends Premium for new Agent users, Super for daily Agent users and Ultra for power users: the bigger tiers cost less per unit.

## What is billed separately?

Third-party models. Cursor’s documentation says they “aren’t included in your plan” and that on Premium and above you use them “at the model’s API list price from the first request, paid from your prepaid credit”. You top up credit yourself, or turn on Auto Top-up. Cursor models beyond your weekly pool are billed from the same credit, and a monthly additional usage limit caps paid usage.

Is “API list price” literal? We compared every Claude row in Cursor’s model table with Anthropic’s own published rate for the same model. Of 15 rows, all 15 matched on input, cache write, cache read and output. Two variants (a 1M-context model and a fast mode) have their own multipliers and were not compared. We did not compare the OpenAI, Google or other providers’ rows. On individual plans, then, Cursor adds no markup to Claude that we could find. Our Claude Code pricing page lists the same Anthropic rates.

## What does the Cursor Token Rate add on Teams?

A flat $0.25 per million tokens. Cursor’s documentation says that on Teams and Enterprise plans, eligible third-party model requests carry this rate “on top of model API pricing”, applying “to input tokens, output tokens, and cached tokens”. Individual plans do not pay it, and a list of excluded models includes Claude Haiku 5.5.

A flat per-token fee matters most when cache reads dominate. We took the one agent session printed in Anthropic’s Claude Code cost documentation (1,200 input, 5,300 output, 940,000 cache read and 50,000 cache write tokens, 94.3% of them cache reads) and priced it at Cursor’s listed rates:

| Model | Individual plan | Teams, all four token types counted | Extra | Teams, cache writes not counted | Extra | 
|---|---|---|---|---|---|
| Claude Sonnet 5.5 | $0.2744 | $0.5235 | +90.8% | $0.5110 | +86.2% | 
| Claude Opus 5.5 | $0.5488 | $0.7979 | +45.4% | $0.7854 | +43.1% | 

Cursor does not say whether “cached tokens” includes cache writes, so we show both readings. This is arithmetic on published rates with a Claude Code session’s token mix. We did not run a Cursor session, and Cursor’s own caching may produce a different mix.

## Is Cursor Auto unlimited?

Not on the current plans, according to Cursor’s documentation. Individual plans “have Cost mode only”, and “Cost mode (classic Auto) bills as a Cursor-model request and draws from your weekly pool whatever model serves it”. Plus has no Auto at all. Google’s AI Overview says Pro “includes unlimited Auto mode”, which describes a legacy plan.

## Is Cursor free?

There is a Free plan at $0 with “Agent and Chat with limited usage”. Cursor publishes no number for “limited”, so we cannot say how many requests you get. Third-party models are not available on Free.

## What does Google’s AI Overview say about Cursor pricing?

Every price in it is a legacy price: Pro $20 ($16 billed annually), Pro+ $60, Ultra $200, Teams $40 a user. It also says the monthly fee “provides a matching budget of usage credits for third-party frontier models”. Cursor’s current documentation says third-party models are not included in the plans and are billed separately. The Overview never mentions Plus, Premium, Super, Grok, SpaceX or The X Subscription. Its monthly legacy figures match Cursor’s legacy page, so they are accurate for people on those plans. They are the wrong place to start for anyone buying today.

## Check it yourself

Four commands reproduce the main findings. Run on 2026-10-21, they printed:

`curl -s https://cursor.com/docs/models-and-pricing.md | grep -E '^\| \*\*(Free|Plus|Premium|Super|Ultra)\*\*' | sed 's/  */ /g' | cut -c1-60````
| **Free** | $0 | Limited | Not available |
| **Plus** | $8/mo | Base | Not available |
| **Premium** | $30/mo | 4x Plus | API list price, from prep
| **Super** | $100/mo | 15x Plus | API list price, from prep
| **Ultra** | $200/mo | 50x Plus | API list price, from prep
```
`curl -s https://cursor.com/docs/account/pricing/legacy-cursor-plans.md | grep -E '^\| \*\*(Pro|Pro\+|Ultra|Teams Standard|Teams Premium)\*\*' | sed 's/  */ /g' | cut -c1-70````
| **Pro** | $20/mo | Premium ($30/mo) |
| **Pro+** | $60/mo | Super ($100/mo) |
| **Ultra** | $200/mo | Ultra ($200/mo) |
| **Teams Standard** | $40/user/mo | Teams, Premium seat ($30/user/mo)
| **Teams Premium** | $120/user/mo | Teams, Super seat ($100/user/mo) 
```
`curl -s https://cursor.com/docs/models-and-pricing.md | grep -o 'Cursor Token Rate of \$0.25 per million tokens' | head -1``Cursor Token Rate of $0.25 per million tokens``python3 -c "t=1200+5300+940000+50000; b=1.2e3*2/1e6+5.3e3*10/1e6+940e3*0.10/1e6+50e3*2.5/1e6; e=0.25*t/1e6; print(round(b,4), round(e,4), round(e/b*100,1))"``0.2744 0.2491 90.8`
The last line is the individual-plan cost of the documented session on Sonnet 5.5, the token-rate surcharge, and the surcharge as a percentage, counting all four token types.

## What we did not check

- **How much work a plan buys.** We ran no Cursor session. Cursor does not publish its weekly pool in tokens, and “limited” on the Free plan has no number.
- **Other providers’ list prices.** We compared Claude rows only, not OpenAI, Google, Z.ai, Moonshot or Meta.
- **Student, education and Enterprise terms.** The footer links a students page we did not read.
- **Bugbot, Origin and other add-ons.** We did not read their prices.
- **Whether “cached tokens” includes cache writes** in the Cursor Token Rate. Hence the two figures.
- **Usage dashboards and help-centre articles** on limits and top-ups.
- **Whether you would pay more or less after switching.** That depends on your usage, which we cannot see.

## Who should not use this page to choose a plan?

If you are on a legacy plan, do not switch because a price list looks cheaper. Cursor says “compare both before you switch”, and the switch is permanent. If you rely on a large third-party model allowance, work out what that usage costs at API list price first. If you need invoicing, SSO or pooled usage, talk to Cursor about Enterprise. For other tools, see our Claude Code vs Codex and Claude Code alternatives pages, and our agentic AI frameworks guide for the wider field.

## FAQ

### How much does Cursor cost per month?

On 2026-10-21 Cursor’s individual plans are Free at $0, Plus at $8, Premium at $30, Super at $100 and Ultra at $200. Annual billing is $80, $300, $1,000 and $2,000 respectively. Teams seats cost $30, $100 or $200. Third-party models such as Claude and GPT are billed separately at API list price.

### Is Cursor Pro still available?

Pro is a legacy plan at $20 a month. Cursor says legacy plans keep working for existing customers but that new customers start on The X Subscription, where the closest plan is Premium at $30. We did not test whether a new account can still buy Pro, and Cursor’s own pricing page no longer lists it.

### Is Cursor Auto unlimited?

According to Cursor’s documentation, no. Individual plans have Cost mode only, and it bills as a Cursor-model request that draws from your weekly pool, whatever model serves it. Plus includes no Auto at all. Google’s AI Overview describes unlimited Auto on Pro, which is a legacy plan.

### Is Cursor free?

Cursor has a Free plan at $0 with limited Agent and Chat usage, and no third-party models. Cursor does not publish how many requests “limited” means. Paid plans start at $8 a month, and the Plus plan is limited to Cursor’s own models.

### Are Claude and GPT models included in Cursor plans?

No. Cursor’s documentation says third-party models “aren’t included in your plan”. On Premium and above you can use them at API list price from prepaid credit. We compared 15 Claude rows with Anthropic’s published rates and all matched. Teams plans add a $0.25 per million token Cursor Token Rate.

### How can I make Cursor cheaper?

The published levers are annual billing, which saves 16.7%, a monthly additional usage limit that caps paid usage, and staying inside the weekly pool by using Cursor’s own models. On Teams, a few models such as Claude Haiku 5.5 skip the Cursor Token Rate. We did not measure how far each helps.
