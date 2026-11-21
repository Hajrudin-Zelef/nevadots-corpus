---
id: collect-261121-veille-ia/outils-agents/cursor-pricing-2026-new-plans-explained-is-pro-worth-it
title: "Cursor Pricing 2026: New Plans Explained, Is Pro Worth It?"
domain: outils-agents
role: reference
task: article
actors: []
dates: []
keywords: ["cursor", "plans", "models", "pricing", "worth", "premium", "ultra", "plan"]
source: https://xvibers.com/blog/cursor-pricing-2026-new-plans-explained-is-pro-worth-it
source_anchor: ""
source_lines: [1, 117]
section: "Outils_Agents"
---
# Cursor Pricing 2026: New Plans Explained, Is Pro Worth It?

· 6 min read · Tools

Cursor pricing changed on October 12, 2026, so most guides you'll find (including ones listing Hobby, Pro and Pro+) are out of date for new buyers. If you're signing up today, you pick from Free, Plus ($8), Premium ($30), Super ($100) or Ultra ($200). The old $20 Pro plan only survives for people who already had it.

So "is Pro worth it?" has two answers, depending on whether you're a new customer or an existing one. Both are below.

## What changed in October 2026

Cursor is now sold as part of a bundle. Cursor's own pricing help page says Cursor is part of The X Subscription, one subscription for Grok, Cursor and X, and new customers choose a plan there. That follows a corporate change: the acquisition by SpaceX closed on August 14, 2026.

For you as a builder, three things matter:

- **New names and prices.** Hobby, Pro, Pro+ and the old Ultra are now "legacy" plans.
- **A different usage model.** Legacy plans have two monthly pools (Cursor Models and Other Models). The new plans have one weekly pool for Cursor models, shared with Grok.
- **Third-party models cost extra.** Claude, GPT and Gemini are no longer bundled into the new plans. More on that below.

## The current plans (new customers)

Here's the lineup from Cursor's help page. Cursor states the paid allowances as multiples of Plus, not as dollar amounts, so I can't tell you exactly how many agent runs each tier buys.

| Plan | Price | Included usage | 
|---|---|---|
| Free | $0 | Limited | 
| Plus | $8/mo | Base | 
| Premium | $30/mo | 4x Plus | 
| Super | $100/mo | 15x Plus | 
| Ultra | $200/mo | 50x Plus | 
| Teams | $30, $100 or $200 per seat/mo | Matches the seat's tier | 

Yearly billing exists too. One third-party tracker reading Cursor's docs lists yearly prices of $80, $300, $1,000 and $2,000 for the four paid plans. Confirm on the pricing page before you commit, since the annual numbers aren't always shown there.

### The catch: Claude and GPT cost extra

This is the part that will surprise people. On the new plans, third-party models stay available in Cursor on Premium and above, billed at API list price from your prepaid credit. So the monthly price buys Cursor's own models (Composer and Grok). If you want Claude or GPT in the agent, you're topping up a credit balance on top.

Also, the new Ultra isn't the same as the legacy $200 Ultra, and third-party models still cost extra there. Same price, different plan.

## What each plan is actually for

### Free ($0)

You can use Agent, Chat and Tab completions with Cursor's own models (Grok and Composer). That's enough to find out whether you like the editor. It's not enough to build an app over a weekend if you lean on the agent all day.

### Plus ($8)

This is the "I like Cursor, I'm happy with its own models" tier. Per one of the third-party writeups, Plus covers Cursor's own Grok and Composer models only. If you're a hobbyist doing a few sessions a week, start here, not at $30.

### Premium ($30)

This is the closest thing to the old Pro. Cursor's legacy-plans docs map Pro at $20 to Premium at $30. It's where you get 4x Plus usage and the option to run third-party models from prepaid credit. For a solo builder shipping regularly, this is the default tier.

### Super ($100) and Ultra ($200)

These are for people running agents for hours a day. If you're not sure whether that's you, it isn't. Buy Premium, watch how fast the pool drains for a month, and only then step up.

### Teams

Teams seats now cost the same as the individual tiers: $30, $100 or $200 per seat. Cursor says that's lower per seat than the older Teams plan, with 3x more coding usage, plus SSO, admin controls, consolidated billing, usage analytics and SOC 2 compliance. If you're a solo founder or a two-person team without SSO needs, you don't need Teams. Individual plans are simpler.

## Is Pro worth it?

### If you're a new customer

Pro isn't on the menu. Legacy plans aren't available to new customers, but existing subscribers keep them until they switch or cancel. The honest question is whether Premium at $30 is worth it. My rule of thumb:

1. Start on Free for a few days on a real project, not a toy.
2. If you hit the limit and it interrupts work you were going to keep, move to Plus ($8) first.
3. Go to Premium ($30) only if you want third-party models or you keep running out of the Plus pool.
4. Budget a small credit top-up if you want Claude in the agent. Treat it like API spend.

### If you're already on legacy Pro

Don't rush. Legacy Pro, Pro+, Ultra and Teams keep working unless you switch, and switching is permanent. Your plan also still includes a pool for third-party models, which the new plans don't. No end date for legacy plans has been published. That could change, so check the dashboard now and then.

Cursor's pitch for switching is more usage on its own models. It says each new paid tier includes at least twice as much coding usage with Cursor models as the plan it replaces. That's worth it if you mostly use Composer. It's probably not if you mostly use Claude.

## A 10-minute check before you pay

1. **Look at which models you actually use.** Open your usage dashboard. If it's mostly Cursor's own models, Plus or Premium works. If it's mostly Claude or GPT, add the API cost to your math.
2. **Find where billing lives.** For new plans, billing moves from cursor.com to grok.com/dashboard/billing. Legacy plans stay at`cursor.com/dashboard/billing` .
3. **Set a spend limit** before turning on top-ups. Don't find out about overage from your card statement.
4. **Compare against a CLI agent.** If you do heavy agent work, the cost may look different in another tool. Our Claude Code pricing breakdown is a useful side-by-side, and Cursor vs Claude Code vs Windsurf covers fit, not just cost.

## Make your usage last longer

Most overspending in agent editors comes from vague prompts that make the model read the whole repo, guess, and redo. A tight prompt costs less and works better. Paste this at the start of a task:

```
Task: [one specific change].
Before editing, list the files you plan to touch and your plan in 5 bullets or fewer.
Do not read or modify anything outside those files.
Make the smallest change that works, then tell me how to test it.
If you need more context, ask me instead of guessing.
```
Other habits that help: start a fresh chat per task, use a cheaper model for boring edits, and commit before every big agent run so a bad run doesn't cost you twice. The same logic applies if you're paying per token, and we cover it in How to Reduce AI API Costs. For the bigger picture on where these tools are heading, see AI Coding Agents 2026.

## A caution about pricing articles (including this one)

This page moved fast. Even in the past few weeks, third-party guides disagreed on details like which plans still exist. Cursor's pricing page lists Free, Plus, Premium, Super, Ultra, Teams and Enterprise, and never mentions Hobby, Pro or Pro+. Those old names live only in the docs. Treat the official pricing page and your dashboard as the source of truth, and treat any blog's table, mine included, as a starting point.

## FAQ

### Is Cursor still free?

Yes. The Free plan is $0 with limited usage, and it covers Agent, Chat and Tab with Cursor's own models. Third-party models like Claude aren't included on it. Existing customers may still have the legacy Hobby plan, which works the same way for Cursor.

### Can I still buy Cursor Pro for $20?

Not as a new customer. Pro is a legacy plan for people who subscribed before October 12, 2026. The nearest equivalent is Premium at $30, according to Cursor's own plan mapping.

### Should I switch from legacy Pro to Premium?

Only if you mostly use Cursor's own models and want the extra usage. The switch is permanent, and the new plans don't include a pool for third-party models. If you rely on Claude or GPT inside Cursor, staying on legacy Pro is the safer move for now.

Cover photo: Jared Brashier on Unsplash
