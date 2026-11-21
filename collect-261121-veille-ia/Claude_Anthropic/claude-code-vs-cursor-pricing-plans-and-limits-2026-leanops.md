---
id: collect-261121-veille-ia/claude-anthropic/claude-code-vs-cursor-pricing-plans-and-limits-2026-leanops
title: "Claude Code vs Cursor Pricing: Plans and Limits 2026 | LeanOps"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["cursor", "claude", "pool", "weekly", "models", "grok", "code", "usage"]
source: https://leanopstech.com/blog/claude-code-vs-cursor-pricing-limits/
source_anchor: ""
source_lines: [1, 143]
section: "Claude_Anthropic"
---
## Key takeaway

Cursor is now sold as The X Subscription: Plus at $8, Premium at $30, Super at $100 and Ultra at $200, with a weekly pool that covers only Cursor's own Grok and Composer models. Claude models in Cursor need Premium or higher and bill at Anthropic's API list price from prepaid credit, from the first request. Claude Code runs on a flat Claude plan, Pro at $20 or Max at $100, with five-hour and weekly limits instead. If you mostly want Claude models, Claude Code is usually the cheaper ceiling; if you want Cursor's editor and its own models, Cursor's pool is the better deal.

## What changed in October

Cursor changed how it sells plans in October 2026, and most pricing comparisons still describe the old ones. New customers no longer see Pro at $20 or Pro+ at $60. Cursor is now sold as part of The X Subscription, "one subscription for Grok, Cursor, and X."

Here is what that means for anyone comparing it with Claude Code, as of November 18, 2026:

- **New plans and prices.** Plus is $8, Premium $30, Super $100 and Ultra $200 a month.
- **A weekly pool.** Each paid plan includes one pool of usage that refreshes every week, shared with the Grok apps.
- **Claude isn't in the pool.** Third-party models such as Claude, GPT and Gemini bill separately, at API list price.
- **Max Mode is gone** from current plans. It survives only on legacy request-based plans.

A few terms first. A **token** is a small piece of text, often part of a word. Models are priced per million tokens. **Context** is everything sent to the model with each message, including the chat so far.

### Quick answer

| Price a month | Claude (Claude Code included) | Cursor (The X Subscription) | 
|---|---|---|
| $8 | No plan | **Plus** : Cursor's own models only, at fixed settings | 
| $20 | **Pro** : five-hour and weekly limits. $17 a month billed yearly | No plan for new customers | 
| $30 | No plan | **Premium** : 4x Plus usage, plus Claude at API price from credit | 
| $100 | **Max** : 5x Pro every five hours and every week | **Super** : 15x Plus usage | 
| $200 | **Ultra** : no five-hour limit for verified accounts, 10x Pro weekly | **Ultra** : 50x Plus usage | 

Cursor also bills yearly: $80 for Plus, $300 for Premium, $1,000 for Super and $2,000 for Ultra.

## Cursor's weekly pool vs Claude's five-hour and weekly windows

**Cursor's pool** pays for Cursor's own models: Grok 4.7, Grok 4.6, Grok 4.5 and Composer 2.5. It's shared across Agent in the editor, the CLI, Cloud Agents, Grok Bot and the Grok apps. A long Grok chat on your phone draws from the same pool as your coding.

The pool refreshes weekly, and "different models draw from the pool at different rates." When it runs out, Cursor lists three options: wait for the reset, keep working from prepaid credit, or upgrade. Cursor says "requests are never downgraded in quality or speed." You see your remaining usage and reset date in your editor settings and under Usage at grok.com/dashboard.

**Claude's limits** work in two windows. The five-hour limit starts with your first message and resets in full five hours later. The weekly limit resets at a fixed time assigned to your account. Your usage in claude.ai, Claude Code and Claude Desktop all counts toward the same limits.

The difference in shape matters. Cursor's pool lets one heavy day use up most of a week. Claude Pro and Max cap each five-hour stretch, so a heavy morning pauses you but leaves the rest of the week intact. Claude Ultra drops the five-hour window for verified accounts and keeps only the weekly one.

Neither company publishes its limits in tokens. To see where you stand, use `/usage` in Claude Code or the usage ring in Cursor's agent panel.

## Using Claude models inside Cursor: the API-rate catch

This is the part most comparison pages get wrong. On The X Subscription, Claude models in Cursor never use your weekly pool. Cursor's docs say they're "available in Cursor at the model's API list price from the first request, paid from your prepaid credit."

So you pay twice: the plan price, then every Claude token on top. Plus and Free don't include third-party models at all.

Here are the Claude rates Cursor lists today, per million tokens. They match Anthropic's own API prices.

| Model | Input | Cache write | Cache read | Output | 
|---|---|---|---|---|
| Claude Opus 5.5 | $4 | $5 | $0.20 | $20 | 
| Claude Sonnet 5.5 | $2 | $2.50 | $0.10 | $10 | 
| Claude Haiku 5.5 | $0.10 | $0.125 | $0.01 | $0.50 | 

A **cache read** is context the model saw recently and reuses at a lower rate. A **cache write** is new context stored for reuse.

Prepaid credit expires 365 days after you buy it. You cap it with the **Monthly additional usage limit** at grok.com/dashboard/billing. Auto Top-up is off by default; leave it off unless you mean to use it.

One more catch: subagents. A subagent that names a Claude model bills as third-party usage, even when your chat shows Auto, Grok or Composer.

## Cost of one week, side by side

Here is a worked example, using the published rates and plain arithmetic. It's an example, not a measurement.

Say a busy day of agent work makes 50 requests that each carry about 200,000 tokens of context. That's 10M tokens read from cache. Add 1M tokens written to the cache and 0.2M tokens of output. Five such days make a working week.

| Setup | One day | One week | A four-week month, with the plan | 
|---|---|---|---|
| Cursor Premium + Claude Opus 5.5 | $2 + $5 + $4 = $11 | $55 | $30 + $220 = $250 | 
| Cursor Premium + Claude Sonnet 5.5 | $1 + $2.50 + $2 = $5.50 | $27.50 | $30 + $110 = $140 | 
| Claude Pro, within its limits | Included | Included | $20 | 
| Claude Max, within its limits | Included | Included | $100 | 

The Claude rows depend on one question: does your week fit inside the plan's limits? Anthropic doesn't publish them in tokens, so nobody can promise it. If it doesn't fit, you can wait for the reset or turn on usage credits. Usage credits bill at the same API rates as the Cursor rows.

So the real comparison is the ceiling. On a Claude plan, the most you pay is the plan price unless you turn on usage credits. In Cursor, Opus has no ceiling except the spending cap you set.

### What about Cursor's own models?

The same day on Composer 2.5, at the rates Cursor charges beyond the pool, costs $2 + $0.50 + $0.50 = $3. Composer has no cache-write charge. With Fast on, Composer's default on Premium and above, the same day costs $5 + $3 + $3 = $11, the same as Opus.

Inside the pool, those rates aren't charged to your card. But Fast still uses the pool faster. The "Turn Fast off" lesson in the free Cursor course shows how to switch it off in the picker and the CLI.

## Legacy Cursor Pro and Pro+ users

If you subscribed before the change, you're on a legacy Cursor plan, and it keeps working as it is.

- **Two monthly pools.** Cursor Models covers Grok and Composer. Other Models covers Claude and other third-party models at API price.
- **On-demand usage.** Extra usage is billed monthly in arrears, off by default on individual plans.
- **Max Mode.** Still available on legacy request-based plans, at the API rate plus 20%.

Cursor maps Pro ($20) to Premium ($30) and Pro+ ($60) to Super ($100). It says each new plan includes "at least twice as much coding usage with Cursor models" as the legacy plan. But the new plans drop the included allowance for third-party models.

So if you mostly use Claude in Cursor, switching can cost you more. Switching is optional and permanent, so compare your Other Models usage on cursor.com/dashboard/spending first.

## When to run both

Many developers use both, and the pricing makes that reasonable.

- **Cursor Plus at $8 plus Claude Pro at $20.** Cursor's editor and its own models, plus Claude Code for Claude work, for $28 a month. Both stop at a fixed price unless you add credit.
- **Claude Code inside Cursor.** The Claude Code extension installs in Cursor, and you can run the CLI in Cursor's terminal. That usage counts against your Claude plan, not your Cursor pool.
- **Cursor Premium for the extras.** Premium adds Cloud Agents, Automations and Bugbot. Pick it for those, and run Claude through Claude Code to keep Claude off your credit.

The setup to avoid is Cursor Premium with Opus as your everyday model. That's the most expensive row in the table above.

On Teams, the gap widens. Cursor adds a Cursor Token Rate of $0.25 per million tokens on eligible third-party requests, on top of the API price. Individual plans don't pay it.

## Spend less in whichever you pick

The habits are the same in both, because both bill the context each message sends.

1. **Check Fast first.** On Cursor Premium and above, Fast is the default for Composer 2.5 and Grok 4.7. Plus runs with Fast off. Turn it off unless someone is waiting on every reply.
2. **One task per chat.** Start a new chat when the task changes. In Claude Code,`/clear` .
3. **Trim always-on rules.** Cursor always applies CLAUDE.md, and always-on rules load with every message. Keep them short. The AGENTS.md guide shows what each agent loads, and CLAUDE.md best practices covers Claude Code.
4. **Match the model to the job.** Sonnet or Composer for routine edits, Opus for hard problems. The Opus vs Sonnet guide covers Claude Code.
5. **Set a cap.** In Cursor, set the monthly additional usage limit. On a Claude plan, set a monthly spend limit before you turn on usage credits.

For more, see how to reduce token usage in any AI coding agent and how to reduce Claude Code token usage. To compare every agent's plans at once, read the AI coding agent pricing comparison. For Claude Code against Codex, see Claude Code vs Codex usage limits.

Practise this in your own project: the free Cursor course. On Claude Code, the free Claude Code course teaches the same habits with its own commands.

## Frequently Asked Questions

### Does Cursor still have a $20 Pro plan?

Not for new customers. Pro at $20 and Pro+ at $60 are now legacy plans that keep working for people who already have them. New customers choose from The X Subscription: Plus at $8, Premium at $30, Super at $100 or Ultra at $200 a month.

### Can I use Claude in Cursor on the $8 plan?

No. Plus includes only Cursor's own models: Grok 4.7, Grok 4.6, Grok 4.5 and Composer 2.5. Claude, GPT and Gemini need Premium or higher, where they bill at the model's API list price from prepaid credit.

### Does Cursor's weekly pool reset if I renew?

Cursor's docs say the pool refreshes weekly, on the date shown under Usage at grok.com/dashboard. They don't say that a renewal or a plan change resets it early, and unused usage doesn't roll over. Check that date before you plan a heavy week.

### Is Claude Code cheaper than Cursor for Opus?

Usually, if your work fits inside your Claude plan's limits. In Cursor, Opus 5.5 costs its API list price from the first request, on top of the plan. On a Claude plan, you pay the plan price until you hit a limit, and only pay API rates if you turn on usage credits.

### What is Cursor's Max Mode now?

Max Mode exists only on legacy request-based plans, where it extends a model's context window and bills at the API rate plus 20%. Cursor's current plans don't include it.
