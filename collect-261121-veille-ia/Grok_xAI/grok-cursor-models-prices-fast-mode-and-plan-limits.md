---
id: collect-261121-veille-ia/grok-xai/grok-cursor-models-prices-fast-mode-and-plan-limits
title: "Grok Cursor Models: Prices, Fast Mode and Plan Limits"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: []
keywords: ["grok", "cursor", "fast", "pool", "models", "plan", "spacexsi", "prices"]
source: https://grokmag.com/grok-build/cursor/
source_anchor: ""
source_lines: [1, 84]
section: "Grok_xAI"
---
# Grok in Cursor: models, prices and what your plan unlocks

Which Grok models Cursor offers, how the shared weekly pool works, what Plus and Premium change, and what OpenAI leaving Cursor means.

**Checked**October 18, 2026

**By**GrokMag editorsIndependent · not affiliated with SpaceXSI, xAI or X

Cursor offers **Grok 4.7, 4.6 and 4.5**, each with a Fast version. Grok 4.7 costs **$2 input and $6 output per million tokens** once your **shared weekly pool** runs out. On **Plus ($8)** Grok runs at fixed settings; **Fast mode and effort levels need Premium ($30)** or higher. Grok 4.7 Fast is only available in Cursor and Grok Build.

## Grok models in Cursor and their prices

Cursor, the AI code editor, is now part of the same company as Grok: SpaceX’s acquisition of Cursor closed on August 14, 2026. SpaceXSI (formerly xAI) has worked with Cursor for longer than that. Grok Code Fast 1 launched with Cursor as a partner in August 2025, and SpaceXSI says Grok 4.5 was “trained alongside Cursor.”

As of October 18, 2026, Cursor lists Grok 4.7, Grok 4.6 and Grok 4.5, each in a standard and a Fast version. Cursor’s rates for Grok 4.7 are:

| Cursor model | Input | Cache read | Output | 
|---|---|---|---|
| Grok 4.7 | $2.00 | $0.50 | $6.00 | 
| Grok 4.7 Fast | $4.00 | $1.00 | $12.00 | 
| Grok 4.7 500k | $4.00 | $1.00 | $12.00 | 
| Grok 4.7 500k Fast | $6.00 | $1.50 | $18.00 | 

Prices are USD per million tokens. Cursor’s pricing page lists the Grok 4.6 and 4.5 rates. For model specs, see our Grok 4.7 guide.

## The shared weekly pool

The per-token rates above are not what most subscribers pay day to day. On the X Subscription, Grok models in Cursor draw from one weekly usage pool shared with the Grok apps and the Grok tab on X. The same pool also covers Cursor’s own Composer 2.5 model.

- **Inside the pool:** Grok use in Cursor costs nothing extra.
- **After the pool runs out:** the token rates apply, paid from prepaid credit. Auto top-up and a monthly limit are optional.
- **Third-party models:** non-Grok models in Cursor, such as Claude or GPT, sit outside the pool and are billed at API list price from the first use.

Because the pool is shared, a long coding session in Cursor leaves less for Grok chat, Grok Build and other Grok products that week. SpaceXSI does not publish the pool size in absolute numbers.

## Plus vs Premium in Cursor

The plan you choose changes how Grok behaves in Cursor, not just how much you can use it.

| Setting | Plus ($8) | Premium ($30) and up | 
|---|---|---|
| Fast mode | Off, fixed | Available; Fast is the default speed | 
| Effort level | Fixed at medium | Adjustable | 
| Grok 4.7 context | 256k, fixed | 256k standard; 500k model listed | 
| Usage pool | Base pool | 4x Plus (Premium), 15x (Super), 50x (Ultra) | 

Legacy Cursor Pro, Pro+ and Ultra subscribers keep Fast mode as well. Note that Cursor’s legacy Ultra plan and the X Subscription Ultra plan both cost $200 but are different plans. Our Grok Bot pricing guide and free vs paid guide compare the tiers in more detail. Still on a legacy Cursor plan? Our Cursor Grok Bot guide explains whether to switch.

## Grok 4.7 Fast: only in Cursor and Grok Build

Grok 4.7 Fast is the same model as Grok 4.7, served on faster infrastructure. SpaceXSI describes it as roughly twice the output speed for twice the price, or 1.5x for long-context requests.

It is only available in Cursor and Grok Build. It is not on the public API, and it is not included in the Grok Build free tier. In Cursor, you need Premium or higher to switch it on, unless you are on a legacy plan that keeps Fast.

## Context windows and long-prompt pricing

Grok 4.7 behaves differently in Cursor and on the SpaceXSI API, which can confuse cost estimates.

- **Cursor:** 256k standard window, 500k maximum. Tokens beyond 256k are billed at 2x, or 3x for Fast.
- **SpaceXSI API:** 500k window. Long-context rates start at 200k tokens and double the price for the whole request.

If you move a workflow from Cursor to the API, or the reverse, re-check your costs.

## OpenAI is ending its models in Cursor

On August 28, 2026, OpenAI said it will stop serving its models to Cursor following the SpaceX acquisition. The proposed shutoff date is November 12, 2026, and OpenAI says no future OpenAI models will come to Cursor.

If you use GPT models in Cursor today, plan for that date. Grok models are the obvious in-house replacement, and Cursor still lists other third-party models. For a broader look at the two model families, see Grok vs ChatGPT.

Want the same Grok models in a terminal instead? Install Grok Build.

## Questions people ask

## Is Grok free in Cursor?

On the X Subscription, Grok use in Cursor comes out of your shared weekly pool, so there is no extra charge until the pool runs out. After that, token rates apply.

## How do I get Fast mode for Grok in Cursor?

Fast mode needs the Premium plan ($30) or higher. Legacy Cursor Pro, Pro+ and Ultra plans also keep it. On Plus, Fast is off.

## When will OpenAI models leave Cursor?

OpenAI proposed November 12, 2026 as the shutoff date and says no future OpenAI models will come to Cursor.
