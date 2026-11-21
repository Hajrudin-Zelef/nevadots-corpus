---
id: collect-261121-veille-ia/claude-anthropic/cursor-pricing-every-plan-from-0-to-200-and-what-claude-costs-on-top-2026
title: "Cursor Pricing: Every Plan from $0 to $200, and What Claude Costs on Top (2026)"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["cursor", "grok", "models", "usage", "plan", "price", "claude", "month"]
source: https://fastero.com/blog/cursor-pricing
source_anchor: ""
source_lines: [1, 164]
section: "Claude_Anthropic"
---
On 12 October 2026 Cursor became part of The X Subscription, one plan for Grok, Cursor and X. What you pay now depends less on the plan than on which models you use, and if you mainly use Claude in Cursor, the plan price is only the start.

**The short answer:** Cursor has five individual plans: **Free** ($0), **Plus** ($8 a month), **Premium** ($30), **Super** ($100) and **Ultra** ($200), or $80, $300, $1,000 and $2,000 billed yearly. **Teams** seats cost $30, $100 or $200 a user a month, and **Enterprise** is a custom price. Every paid plan includes a weekly pool for Cursor's own models only: Grok 4.7, Grok 4.6, Grok 4.5 and Composer 2.5. Claude, GPT and Gemini are "billed at API list prices from the first use", from prepaid credit, on Premium and above; Plus and Free can't use them. Teams and Enterprise add a Cursor Token Rate of $0.25 per million tokens on most third-party models. The old Pro ($20), Pro+ ($60) and Teams ($40 a seat) plans still work for people who had them, but new customers can't buy them. Prices checked 12 November 2026.

## Cursor plans and prices (checked 12 November 2026)

| Plan | Monthly | Billed yearly | Included usage | Claude, GPT, Gemini | 
|---|---|---|---|---|
| Free | $0 | $0 | Limited | Not available | 
| Plus | $8 | $80 ($7 a month) | Base | Not available | 
| Premium | $30 | $300 ($25 a month) | 4x Plus | API list price, from prepaid credit | 
| Super | $100 | $1,000 ($84 a month) | 15x Plus | API list price, from prepaid credit | 
| Ultra | $200 | $2,000 ($167 a month) | 50x Plus | API list price, from prepaid credit | 
| Teams | $30, $100 or $200 a seat | Not stated | Matches the seat's tier | API list price plus the Cursor Token Rate | 
| Enterprise | Custom | Custom | Pooled | API list price plus the Cursor Token Rate | 

Prices exclude tax. What each step adds:

- **Free:** Agent, Chat and Tab with Cursor's own models, limited usage.
- **Plus:** Cursor's models at fixed settings (Fast off, medium effort), plus the Grok apps. No Auto, Cloud Agents, Automations, Bugbot code review, Origin code hosting, third-party models or your own API keys.
- **Premium:** four times Plus usage, plus everything Plus leaves out, and Grok Bot.
- **Super:** 15 times Plus usage and priority model access at peak times.
- **Ultra:** 50 times Plus usage and the earliest access to new products and models.

Cursor recommends "Premium for new AI users, Super for daily agent users, and Ultra for power users", and Plus for "social creators with limited AI usage". Paid plans also carry X benefits once you connect an X account.

## What the included pool covers

Each paid plan includes one pool of usage for Cursor models, shared across Agent in the editor, the CLI, Cloud Agents, Grok Bot and the Grok apps. It refreshes weekly: "Your allowance refreshes weekly, so one heavy day never wipes out your whole month." Unused usage doesn't roll over.

Cursor sizes the pools against each other (Premium is 4x Plus), not in tokens or dollars, and different models draw from them at different rates. You see what's left in the editor settings and under **Usage** at grok.com/dashboard. When the pool runs out you can wait for the reset, upgrade, or keep going from prepaid credit at these rates per million tokens:

| Cursor model | Input | Cache read | Output | 
|---|---|---|---|
| Grok 4.7, 4.6 and 4.5 | $2 | $0.50 | $6 | 
| Composer 2.5 | $0.50 | $0.20 | $2.50 | 

Fast mode costs more: $4 and $12 on Grok 4.7, $3 and $15 on Composer 2.5. Auto's Cost mode, the old Auto, draws from your pool whatever model serves it; on Teams and Enterprise, the Balance and Intelligence modes bill at the routed model's price, so a route to Claude bills as third-party usage.

## Claude, GPT and Gemini cost API list price

Third-party models aren't in the pool at all. A Cursor staff member put it plainly at launch: "There's no included allowance for third-party models on Premium, Super, or Ultra (individual or Teams)." Cursor charges the provider's list price per million tokens, the same figures Anthropic publishes:

| Model | Input | Cache write | Cache read | Output | 
|---|---|---|---|---|
| Claude Opus 5.5 | $4 | $5 | $0.20 | $20 | 
| Claude Sonnet 5.5 | $2 | $2.50 | $0.10 | $10 | 
| Claude Haiku 5.5 (prompts up to 100K tokens) | $0.10 | $0.125 | $0.01 | $0.50 | 
| Claude Fable 5.2 | $8 | $10 | $0.20 | $40 | 
| GPT-5.6 Sol | $4 | $5 | $0.40 | $20 | 
| Gemini 4 Argon | $2 | n/a | $0.10 | $10 | 

How you pay for it:

- **Prepaid credit.** Buy a top-up at grok.com/dashboard/billing, or turn on**Auto Top-up** . You're charged when you buy, not in arrears, and credit expires "currently 365 days after purchase".
- **A spending cap.** The**monthly additional usage limit** pauses paid usage when you reach it, even if credit remains, though "Enforcement is not instant".
- **Your own key.** On Premium, Super and Ultra you can paste an Anthropic, OpenAI or Google key. Your provider bills you, and nothing comes out of your pool or credit.

One trap: "Subagents can also run a named third-party model while the picker still shows Auto, Grok, or Composer." Those requests bill as third-party usage.

## Teams, Enterprise and the Cursor Token Rate

Teams costs the same per seat as the matching individual plan, $30, $100 or $200 a user a month, and you can mix tiers on one team; Plus isn't available. Seats don't share pools, but the team shares one prepaid balance for third-party models and overflow, with one monthly cap and per-member limits. Teams adds central billing, usage analytics, enforced privacy mode and SAML/OIDC single sign-on. **Enterprise** adds "pooled usage across Grok and Cursor, SCIM provisioning, audit logs, advanced admin controls, and invoice billing".

The **Cursor Token Rate** is the part individual plans don't pay. On Teams and Enterprise, eligible third-party requests carry $0.25 per million tokens on top of the model's price, counted on input, output and cached tokens, and it applies when you use your own API key too. It doesn't apply to Grok, Composer or Cost-mode requests, or to third-party models whose cache-read price is "strictly below $0.10 per million tokens across every pricing tier". Claude Haiku 5.5 ($0.01 and $0.05) is exempt; Claude Sonnet 5.5, at exactly $0.10, is not.

That rate matters more than it looks. In the agent session we price below, 94% of the tokens are cache reads, and Opus 5.5's own cache-read price ($0.20 per million) is less than the $0.25 Teams adds to each of them.

## What changed from the old Pro, Pro+ and Teams plans

Until June 2025, Pro counted requests: "There was a limit of 500 requests per month, with Sonnet models costing two requests." From 16 June 2025 it gave "$20 of frontier model usage per month at API pricing". On 12 October 2026, The X Subscription replaced it for new customers. The old plans, now called legacy plans:

| Legacy plan | Price | Closest plan now | 
|---|---|---|
| Hobby | Free | Free | 
| Pro | $20 a month | Premium, $30 | 
| Pro+ | $60 a month | Super, $100 | 
| Ultra | $200 a month | Ultra, $200 | 
| Teams Standard seat | $40 a user | Teams Premium seat, $30 | 
| Teams Premium seat | $120 a user | Teams Super seat, $100 | 

"Comparable means the closest tier, not an identical one." The differences:

|  | Legacy plans | The X Subscription | 
|---|---|---|
| Included usage | Two monthly pools: Cursor Models and Other Models | One weekly pool, Cursor models only, shared with Grok | 
| Claude, GPT, Gemini | Included through the Other Models pool on Pro, Pro+, Ultra and Teams | Not included; API list price from the first request | 
| Extra usage | On-demand, billed monthly in arrears | Prepaid top-ups | 
| Billing | cursor.com/dashboard | grok.com/dashboard | 

Cursor says each new plan "includes at least twice as much coding usage with Cursor models as the legacy plan it replaces". So the trade is more Grok and Composer, and no included Claude.

On a legacy plan, nothing happens yet: "Your current plan, price, and features continue unless you change or cancel." Switching is optional and refunded pro rata, and "The switch is permanent." Cursor staff added that "Users who don't choose to migrate will, sometime in the future, be migrated with advance notice."

## Worked examples: Claude in Cursor vs Claude Code

Claude Code is included in every paid Claude plan: Pro at $20 a month ($17 billed yearly), Max at $100 and Ultra at $200. Its limits reset every 5 hours and every week, and when you hit them you can "turn on usage credits to keep working at standard API rates". In Cursor, every Claude token is paid at those rates from the first one.

To put numbers on it we used the session Anthropic shows in its Claude Code cost docs: 1,200 input tokens, 5,300 output, 940,000 cache-read and 50,000 cache-write tokens, almost a million in all. We priced it at Opus 5.5 and Sonnet 5.5 list prices with a short script (Python 3.14.3):

The same months side by side, with Opus 5.5:

| A month of Opus 5.5 | Cursor Premium | Cursor Teams, Premium seat | Claude Code | 
|---|---|---|---|
| 40 sessions (two a working day) | $51.95 | $61.92 | Pro, $20, if it fits the plan's limits | 
| 200 sessions (ten a working day) | $139.76 | $189.59 | Max ($100) or Ultra ($200), if it fits | 
| Anthropic's enterprise average | $30 plus $150-250 | More, with the Token Rate | $150-250 on the API | 

The last row uses Anthropic's figure for Claude Code on API billing: "around $13 per developer per active day and $150-250 per developer per month".

Two cautions: Anthropic doesn't publish plan limits in tokens, so we can't say whether 200 sessions fit in Max, and the two agents won't spend the same tokens on the same task. What the arithmetic shows: for the same Claude tokens, Cursor Premium costs $30 plus list price, while Claude Pro costs $20 and covers some usage before list price starts. Claude Code's VS Code extension also installs in Cursor, so you can keep Cursor's editor and pay for Claude through a Claude plan. Our Cursor vs Claude Code comparison covers the rest, and Claude Code pricing and Claude API pricing go deeper on Anthropic's side.

## Which Cursor plan to pick

- **Grok or Composer only:** Plus ($8) for the editor and Agent; Premium ($30) for Cloud Agents and Bugbot.
- **Claude or GPT daily:** Premium at least, with the model cost on top, or your own key.
- **Agents all day on Cursor's models:** Super or Ultra, for the bigger weekly pool.
- **A team using Claude heavily:** price in the Token Rate, or use Haiku 5.5 and Cursor's models where they're good enough.
- **Legacy Pro or Pro+ and you use Claude:** staying put keeps your included third-party usage.

## When the agent's work is for a team

Whatever runs your agent, Cursor or Claude Code, the dashboards and queries it builds usually end up in a chat or a local file. Fastero is an AI and data platform where people and AI agents work together on a team's data, and a remote MCP server with OAuth sign-in:

- **One place to connect.****Settings → AI Agent Access** gives setup instructions for Claude Code, Cursor and other agents, which sign in through the browser. We've tested it with Claude and Claude Code.
- **Keys for scripts.** For anything that can't sign in, a key reads and runs SQL by default and expires within 90 days.
- **Results colleagues open.** Charts go onto a dashboard that refreshes on a timer, or when the data changes on Postgres and BigQuery. Colleagues sign in with their own login, and viewers are free.
- **Plans.** Free has no end date; Pro is $20 per editor a month. A new account starts with $20 of AI and 14 days of Pro.

*Settings, AI Agent Access: an agent signs in through the browser, so there is no key to copy.*

## FAQ

### How much does Cursor cost per month?

Free is $0, Plus $8, Premium $30, Super $100 and Ultra $200 a month, or $80, $300, $1,000 and $2,000 billed yearly. Teams seats cost $30, $100 or $200 a user. Prices exclude tax and were checked 12 November 2026.

### Is Cursor Pro still $20?

Only for people who already had it. Pro is now a legacy plan that keeps working at $20 a month, but new customers choose a plan on The X Subscription, where Premium ($30) is the closest match.

### Does Cursor include Claude?

No. On The X Subscription, Claude is billed at Anthropic's API list price from the first request, from prepaid credit, on Premium and above. Plus and Free can't use it. Legacy Pro, Pro+, Ultra and Teams plans still include some third-party usage.

### What is the Cursor Token Rate?

A charge of $0.25 per million tokens on Teams and Enterprise, added to third-party model usage, cached tokens included, and to requests made with your own key. Cursor's own models, Cost-mode Auto and models with a cache-read price below $0.10 are exempt.

### Can I use my own Anthropic API key in Cursor?

Yes, on Premium, Super and Ultra. Anthropic bills you, and those requests don't use your pool or credit. On Teams and Enterprise, the Cursor Token Rate still applies to them.

### Is Cursor cheaper than Claude Code?

For Grok and Composer, Cursor's pool is included in the plan. For Claude, the same tokens cost less through a Claude plan: Pro is $20 a month with usage included, while Claude in Cursor is $30 plus every token at list price.

## Sources

Checked 12 November 2026:

- Cursor: Pricing, Models & Pricing, Pricing and plans, Usage and limits, Usage-based charges, Bring your own API key, Cursor Token Rate, Cursor Router, Team pricing, Legacy Cursor plans, What is The X Subscription?, Clarifying our pricing (July 2025), Introducing The X Subscription (forum)
- SpaceXSI: Introducing The X Subscription, Pricing, Prepaid balance and automatic top-ups
- Anthropic: Claude pricing, API pricing, Claude Code costs
