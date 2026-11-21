---
id: collect-261121-veille-ia/outils-agents/cursor-pricing-changes-6-plan-shifts-since-2025-dated
title: "Cursor Pricing Changes: 6 Plan Shifts Since 2025, Dated"
domain: outils-agents
role: reference
task: article
actors: []
dates: ["2025-06-16"]
keywords: ["cursor", "usage", "pricing", "auto", "plan", "june", "requests", "request"]
source: https://zandigital.in/cursor-pricing-changes-dated
source_anchor: ""
source_lines: [1, 213]
section: "Outils_Agents"
---
From Aryan Vatsa | Product & Market Analysis

# Cursor Pricing Changes: Every Plan Shift From June 2025 to the X Subscription

## On this page

In June 2025, $20 a month bought a Cursor Pro user 500 requests. Seventeen months later, the plan that legacy Pro maps to costs $30, includes no Claude or GPT usage at all, and resets weekly. Cursor pricing has shifted 6 times in between, and each shift changed what a heavy user actually got for the money. This is the dated record, built from Cursor's own posts and documentation.

Key takeaways

- **Cursor Pro stopped counting requests on 16 June 2025.** The old plan capped Pro at 500 requests a month, with Sonnet models costing 2 requests each. The new default gave at least $20 of model inference at API prices.
- **The refund covered 16 June to 4 July 2025 only.** Cursor's CEO wrote that the change "was not communicated clearly" and offered refunds for unexpected usage in that window, via email to a dedicated address.
- **Auto lost its unlimited status in two steps.** It began drawing from included usage at renewals after 15 September 2025. In late August 2026, its flat token rate gave way to the list price of whichever model served the request.
- **The X Subscription drops third-party models from every included pool.** Cursor says each new tier carries at least twice the Cursor-model usage of the legacy plan it replaces. Claude and GPT now bill from prepaid credit from the first request.

**500 to $20**Pro moved from a 500-request monthly cap to $20 of API-priced usage. Source: Cursor blog, July 2025.

**The short answer.** Cursor pricing changed from a request cap (500 a month on Pro) to a dollar pool of API-priced usage in June 2025. Cursor apologised and refunded in July, then metered its Auto model from September 2025. It restructured Teams in mid 2026. In October 2026 it moved new customers to a weekly pool shared with Grok. Legacy plans still run unchanged unless you switch.

## The Cursor pricing changelog, dated

Every row below links to the document that carries the change. Where Cursor published the change itself, that is the link. Where only a third party recorded the date, the row says so.

The reader this is written for is the practitioner paying for Cursor personally or on a small team card. That is the person whose bill moved most at each step, and the person least likely to have a sales rep explaining it.

| Cursor plan and pricing changes, June 2025 to November 2026. |  |  |  | 
|---|---|---|---|
| Date | Change | What a heavy individual user got | Source | 
|---|---|---|---|
| Before 16 Jun 2025 | Pro at $20 with a request cap | 500 requests a month; Sonnet models cost 2 requests each. | Cursor blog, 4 Jul 2025. | 
| 16 Jun 2025 | Pro moves to compute limits; Ultra launches at $200 | At least $20 of inference at API prices, unlimited Auto, no tool-call limits. Ultra: 20x Pro usage. | Cursor blog, 16 Jun 2025. | 
| 4 Jul 2025 | Clarification, apology and refunds | Unlimited applies to Tab and Auto only; $20 of frontier usage; refunds for 16 Jun to 4 Jul. | Cursor blog, 4 Jul 2025. | 
| Renewals after 15 Sep 2025 | Teams moves to variable request costs; Auto starts drawing on included usage | The $20 pool now has to cover Auto too, at a flat token rate. | Cursor blog and forum, 12 Aug 2025. | 
| 1 Jun 2026 (renewals from 1 Jul) | Teams seats get two pools; new $120 Premium seat | Standard seat stays $40 with more usage; Premium seat gets 5x Standard. | Cursor blog, 1 Jun 2026. | 
| 24 Aug to 3 Sep 2026 | Flat Auto rate retired; Enterprise legacy rate to 7 Sep 2026 | Auto bills at the list price of the routed model. | UsagePricing captures of Cursor docs. | 
| October 2026 | The X Subscription: Plus $8, Premium $30, Super $100, Ultra $200 | Weekly pool shared with Grok; third-party models excluded from the pool. | Cursor pricing page and docs (undated). | 

The September 2025 row counts as one change even though it touched Teams and Auto, because Cursor announced both in the same post. The headline count of 6 excludes the pre-June baseline row.

## June 2025: Cursor Pro moves from request counts to compute

On 16 June 2025, Cursor CEO Michael Truell announced that "the Pro plan will move from request limits to compute limits". The same post promised that "all users will get at least $20 of model inference at API prices per month".

The old model was simple to read. According to Cursor's later account, Pro carried "a limit of 500 requests per month, with Sonnet models costing two requests". A heavy Sonnet user therefore had about 250 Sonnet requests a month for $20.

The announcement was framed as a gift, and partly it was. Cursor said it was "rolling out unlimited access to the Auto model and lifting all limits on tool calls". Auto is Cursor's router, which picks a model for each request instead of letting you choose.

Existing users were told they could "stay with the 500 request limit method if they prefer". That opt-out mattered later, because many users did not realise the default had changed.

### Ultra arrived the same day

The same post launched Ultra, described as "a $200 / mo plan with 20x more usage than Pro". Ultra cost 10 times Pro and carried 20 times the usage, so it delivered 2x the usage per dollar. That ratio is worth remembering, because Cursor rebuilt the same 2x ladder in 2026.

Cursor's legacy documentation also lists a Pro+ plan at $60 a month. We could not find a Cursor post dating its launch, so it is absent from the changelog rather than placed on a guessed date.

## July 2025: the Cursor refunds and what they covered

The backlash came from the gap between "unlimited" and the bill. Users read the June post as unlimited usage, ran Claude models heavily, and met overage charges or cut-offs well before month end.

On 4 July 2025, Truell published "Clarifying our pricing". He wrote: "Our recent pricing changes for individual plans were not communicated clearly, and we take full responsibility". He also named the specific confusion: "We were not clear that unlimited usage was only for Auto and not all other models".

Cursor offered "usage refunds for those with unexpected usage between June 16 and July 4". Users with a surprise bill were told to email pro-pricing@cursor.com for "a full refund". There was no automatic credit. You had to ask.

The clarified Pro plan carried three items. They were "Unlimited usage of Tab and models in Auto," "$20 of frontier model usage per month at API pricing," and "An option to purchase more frontier model usage at cost".

### Why the refund was the right call and still not enough

My view is that the refund was correct and the window was too short. A refund that requires an email reaches only the customers who noticed, and the customers who noticed were already the angriest. A credit applied automatically to every affected account would have cost more and bought more trust.

The broader pattern, where a vendor holds the price and quietly lowers the allowance, is covered in the analysis of AI shrinkflation in usage limits. Cursor's June change was the reverse case: the allowance arguably rose, and the communication made it feel like a cut.

## September 2025: Cursor usage based pricing reaches Auto and Teams

On 12 August 2025, Cursor announced two changes in one post. Teams would move "from fixed request costs to variable request costs," matching what Pro had already done. And Auto, which "has been unlimited for individuals" since June, would now "contribute to your included monthly usage at competitive token rates".

The timing rule was generous. A Cursor staff member wrote on the company forum that the changes "will occur at your next billing renewal on or after September 15". An annual subscriber who bought in June 2025 kept the old terms until June 2026.

Cursor's announcement linked to its docs for the rate rather than stating it. Third-party records and a Cursor staff post on the forum give the flat Auto rate as $1.25 per million input tokens and $6 per million output tokens. Cache reads cost $0.25. We could not open an archived copy of that docs page ourselves, so treat the figure as well corroborated rather than primary.

At that rate, a $20 pool covers 16 million input tokens or about 3.3 million output tokens of Auto alone. An agent session that rereads a large repository can spend input tokens far faster than a chat session does. That is the mechanism behind the daily cost spread for terminal coding agents.

### What the heavy user lost

Before September 2025, the heavy user had two things for $20: $20 of frontier usage and uncapped Auto. After the renewal, the same $20 had to cover both. The per-dollar value of the frontier pool did not change. The free floor underneath it disappeared.

This is the change I think deserved the most attention and got the least. The June change was loud. The September change was the one that actually reduced included usage for anyone who leaned on Auto.

## 2026: Teams pools and Auto list pricing

Cursor's next pricing post came on 1 June 2026, titled "Improvements to Teams Pricing". It opened: "We are increasing our Teams plan usage limits, introducing a Premium seat to support the heaviest agent users, and making it easier for admins to forecast and control spend".

Every Teams seat now came with "two separate pools of included usage". One was a first-party pool for Auto and Composer 2.5. The other was a third-party pool for models such as Claude and GPT. The Standard seat stayed at $40 a month, or $32 on annual billing, with "significantly more usage, with no change in cost".

The new Premium seat cost $120 a month, or $96 annually, with "5x the included usage of the Standard seat, at only 3x the cost". That works out to 1.67x the usage per dollar of a Standard seat. The change applied to new customers immediately and to renewals from 1 July 2026.

### The flat Auto rate goes

Archived captures of Cursor's models and pricing docs, recorded by UsagePricing, show the change. Between 24 August and 3 September 2026, the flat Auto rate was withdrawn. The replacement wording, as quoted there, reads: "All Auto modes bill at the list price of the model each request is routed to". Enterprise customers kept the old rate until 7 September 2026.

For a buyer, list-price routing makes Auto less predictable, not more expensive by definition. A cheap routed model now costs less than the old flat rate, and an expensive one costs more. You lose the ability to price Auto before you use it. That is the same forecasting gap described in the breakdown of credits, tokens and AI billing confusion.

The ownership change sits in the same window. Cursor's blog announced on 14 August 2026 that "Cursor has officially been acquired by SpaceX". The pricing restructure that followed two months later makes more sense in that light, since the new plans are sold through a sister company.

## October 2026: the X Subscription replaces the Cursor Pro plan

Cursor's pricing page now lists five individual tiers. They are Free, Plus at $8, Premium at $30, Super at $100 and Ultra at $200. The page states that "Subscriptions are only sold directly through X Subscription Corp". UsagePricing dated the switch to 15 October 2026 from before and after captures. Cursor's own pages carry no date.

The new plans replace the two monthly pools with one weekly pool. Cursor's FAQ says: "Your allowance refreshes weekly, so one heavy day never wipes out your whole month". The pool covers Cursor's own models, Grok 4.5 to 4.7 and Composer 2.5, and is "shared across Cursor and Grok".

Third-party models sit outside it entirely. The docs say: "Third-party models like Claude, GPT, and Gemini aren't part of the included pool. On Premium and above, they're available in Cursor at the model's API list price from the first request, paid from your prepaid credit".

Cursor publishes each tier as a multiple of Plus: Premium is 4x, Super 15x and Ultra 50x. It does not publish what Plus contains in dollars or tokens. That choice makes tiers comparable with each other and impossible to compare with anything else.

I would ask Cursor to publish the Plus pool in tokens per week. A multiplier of an undisclosed number is not a price. Buyers deserve the same transparency Cursor gave in June 2025, when the pool was stated as $20 at API prices.

### What happens to legacy Pro, Pro+ and Ultra

Existing plans continue. The help centre says: "Your current plan, price, and features continue unless you change or cancel". Switching is a one-way door: "The switch is permanent. It can't be undone, and Support can't undo it for you".

Cursor's legacy plan docs map each old plan to a new tier: Pro ($20) to Premium ($30), Pro+ ($60) to Super ($100), and Ultra ($200) to Ultra ($200). Cursor says each new plan "includes at least twice as much coding usage with Cursor models as the legacy plan it replaces" and "drops the included third-party model allowance". The pattern of keeping old customers on old terms until they move is examined in the analysis of grandfathered pricing at renewal.

## Cursor pricing per dollar: what $20 bought at each step

The question a heavy user cares about is not the sticker price. It is how much usage each dollar buys, and which models that usage can reach. The units changed three times, so the table keeps them separate rather than forcing a false conversion.

| Included usage per dollar for a heavy individual Cursor user, by period. |  |  |  | 
|---|---|---|---|
| Period | Plan and price | Included usage | Per dollar | 
|---|---|---|---|
| Before 16 Jun 2025 | Pro, $20 | 500 requests; Sonnet at 2 requests each. | 25 requests, or 12.5 Sonnet requests. | 
| 16 Jun to renewal after 15 Sep 2025 | Pro, $20 | $20 at API prices plus unlimited Auto. | $1.00 of frontier usage, plus uncapped Auto. | 
| Renewal after 15 Sep 2025 | Pro, $20 | $20 at API prices, Auto included in the pool. | $1.00 of usage covering frontier and Auto. | 
| 2025 onward | Ultra, $200 | 20x Pro. | 2x Pro's usage per dollar. | 
| From Jul 2026 renewals | Teams Premium seat, $120 | 5x Standard seat. | 1.67x a $40 Standard seat. | 
| From Oct 2026 | Premium, $30 | At least 2x legacy Pro's Cursor-model usage; no third-party pool. | At least 1.33x on Cursor models; $0 included for Claude or GPT. | 

Per-dollar figures are Zan Digital arithmetic on Cursor's published numbers. The October 2026 row divides Cursor's "at least twice" claim by the 1.5x price rise from $20 to $30. Cursor publishes no dollar size for the legacy or new pools in 2026.

On the new tiers, usage per dollar rises with price. Plus delivers 1 unit for $8, or 0.125 per dollar. Ultra delivers 50 units for $200, or 0.25 per dollar. That is exactly 2x, the same ratio Ultra held over Pro in June 2025.

### The heavy Claude user pays more, by arithmetic

Take a user who spends most of their pool on Claude. On legacy Pro, the last figure Cursor published was $20 of frontier usage at API prices inside the $20 plan. On Premium, the same $20 of Claude usage is paid from prepaid credit on top of the $30 subscription, so the total becomes $50.

That is 2.5x the monthly cost for the same third-party usage, plus a larger Cursor-model pool the user may not want. If your work depends on Claude inside Cursor, my recommendation is to stay on legacy Pro and not press the switch button. The comparison of which coding tool fits which workflow is in the Claude Code, Cursor and Copilot verdict.

## Where this rebuild is weakest

A changelog is only as good as its sources, and three parts of this one are thinner than the rest.

### No archived pricing pages were opened

The brief for this post was to rebuild each change from archived copies of Cursor's pricing page. The Internet Archive was not reachable from our research environment, so every row rests on Cursor's blog posts, current docs and a forum staff post instead. Those are primary sources, but they record what Cursor announced, not what the pricing page displayed on a given day.

### Two dates come from a third party

The August 2026 Auto change and the October 2026 X Subscription date come from UsagePricing's captures of Cursor's pages. Cursor's own pricing page and docs are undated, and we found no Cursor blog or changelog entry announcing either change. A second aggregator gives 24 August for the Auto change; UsagePricing gives 26 August. We report the range.

### The case for the new plans is real

If you mostly use Cursor's own models, the X Subscription is a better deal. Cursor says each tier carries at least twice the Cursor-model usage of the plan it replaces. A weekly reset also protects you from one bad day consuming the month. For a user who has already moved to Composer or Grok, the $20 to $30 move buys at least 1.33x more usage per dollar.

The per-dollar figures in this post also rest on Cursor's own multipliers, which nobody outside Cursor can audit. If the Plus pool shrinks, every multiplier shrinks with it, and the published ratios would not show it.

## Frequently asked questions

### How much does Cursor Pro cost in 2026?

Legacy Cursor Pro still costs $20 a month for existing subscribers, and Cursor says the plan continues unchanged unless you switch or cancel. New customers see the X Subscription tiers instead: Plus at $8, Premium at $30, Super at $100 and Ultra at $200 a month. Cursor's legacy plan docs map Pro to Premium as the closest tier, not an identical one.

### What changed with Cursor Pro pricing in June 2025?

On 16 June 2025, Cursor moved Pro from a 500-request monthly limit to compute limits, promising at least $20 of model inference at API prices each month. It also made the Auto model unlimited and removed tool-call limits. Cursor later admitted it did not make clear that unlimited usage applied only to Auto and Tab, not to frontier models such as Claude.

### Did Cursor give refunds for the pricing change?

Yes. On 4 July 2025, Cursor offered usage refunds to users with unexpected charges between 16 June and 4 July 2025. Affected users had to email pro-pricing@cursor.com to request a full refund; it was not applied automatically. Charges outside that window were not covered by the offer, according to Cursor's own post announcing it.

### Is Cursor Auto still unlimited?

No. Auto was unlimited for individual plans from June 2025, then began drawing on included usage at renewals after 15 September 2025, at a flat token rate. In late August 2026 the flat rate was withdrawn and Auto now bills at the list price of whichever model serves each request. On the new plans, Auto draws from the weekly pool.

### Should I switch from Cursor Pro to the X Subscription?

Switch only if you mostly use Cursor's own models, Grok and Composer. Each new tier includes at least twice the Cursor-model usage of the legacy plan it replaces. If you rely on Claude or GPT, the new plans include none of that usage, so you pay list price from prepaid credit. The switch is permanent and Cursor support cannot reverse it.

### What does Cursor Ultra include?

Ultra costs $200 a month on both the legacy and the new plans. The June 2025 version carried 20 times Pro's usage. The current version carries 50 times the usage of the $8 Plus plan, drawn from a weekly pool shared with Grok. On a per-dollar basis, Ultra delivered twice the usage of the entry tier in both versions.

## How to pick a Cursor plan this week

Open your Cursor usage dashboard and split last month's spend into two buckets: Cursor's own models and third-party models. That one split answers the switching question better than any pricing page.

If more than half of your usage is Claude or GPT, stay on your legacy plan and note your renewal date. If most of it is Auto, Composer or Grok, the X Subscription will likely give you more for the money. Run the switch on a test account first, because it cannot be undone.

Related analysis

Cursor's pricing sits behind one of the fastest revenue climbs in software. The business case is in the analysis of Cursor's valuation and growth.

References

1. Cursor, New tier announcement, Michael Truell, 16 June 2025. Used for the move to compute limits, unlimited Auto and Ultra at $200.
2. Cursor, Clarifying our pricing, Michael Truell, 4 July 2025. Used for the 500-request baseline, the apology and the refund window.
3. Cursor, August 2025 pricing update, Michael Truell, 12 August 2025, and forum post by Cursor staff, same date. Used for the Teams and Auto changes and the 15 September renewal rule.
4. Cursor, Improvements to Teams Pricing, 1 June 2026. Used for Teams seat prices and pools.
5. Cursor, Pricing and The X Subscription overview, read 4 November 2026. Used for current tiers, multipliers, the weekly pool and the switching terms.
6. Cursor, Legacy Cursor plans, read 4 November 2026. Used for legacy prices and the mapping to new tiers.
7. UsagePricing, Cursor's Auto Cost mode drops its flat per-token rate, 26 August 2026, and its X Subscription entry of 15 October 2026. Used for change dates, based on its captures of Cursor's docs.

The weakest part of the source base is dating: Cursor's current pricing pages carry no dates, so two 2026 dates rest on a third party's captures. All figures were last verified on 4 November 2026.

Related reading
