---
id: collect-261121-veille-ia/outils-agents/cursor-pricing-2026-plans-spend-data-and-how-to-pay-less
title: "Cursor Pricing 2026: Plans, Spend Data, and How to Pay Less"
domain: outils-agents
role: reference
task: article
actors: []
dates: []
keywords: ["cursor", "usage", "models", "grok", "plan", "premium", "teams", "plans"]
source: https://www.spendhound.com/marketplace/cursor-pricing
source_anchor: ""
source_lines: [1, 255]
section: "Outils_Agents"
---
## **How Cursor pricing works**

Cursor uses a hybrid model: a subscription that includes a weekly amount of usage for Cursor's own models, plus prepaid credit for third-party models and for usage beyond that allowance. What you actually pay depends less on the plan price and more on how much your developers use AI agents and which models they choose.

Cursor is now sold as part of The X Subscription, one subscription for Grok, Cursor, and X. Usage works in two parts:

- Cursor Models: Cursor's own models (Composer 2.5 and the Grok 4.5 to 4.7 family). Each paid plan includes a pool of usage for these, shared across Cursor and Grok, that refreshes weekly.
- Other Models: third-party models from Anthropic, OpenAI, Google, and others, charged at each model's API price. They aren't part of the included pool. On Premium and above, they're paid from prepaid credit from the first request.

When the weekly pool runs out, you can wait for it to reset, keep working on prepaid credit (top-ups or Auto Top-up), or move to a higher tier. Additional usage continues at the same per-model rates, with no drop in quality or speed. Because models differ widely in cost, a developer's model choice determines how quickly included usage and credit are consumed.

Who buys what:

- Individuals buy Plus, Premium, Super, or Ultra.
- Teams comfortable self-serving buy the Teams plan with per-user seats at the Premium, Super, or Ultra tier.
- Organizations that need more control buy Enterprise, which adds pooled usage, invoicing, SCIM, and advanced security controls.
- Existing customers on the legacy Pro, Pro+, Ultra, Start, and Teams Standard and Premium plans keep them, with two monthly usage pools and on-demand usage.

Cursor sells only directly through X Subscription Corp and doesn't authorize resellers, so every purchase and negotiation goes directly through Cursor.

## **Cursor pricing plans**

### **Individual plans**

| **Plan** | **Billed monthly** | **Billed yearly** | **Best for** | 
|---|---|---|---|
| Free | Free | Free | Trying Cursor with limited agent requests | 
| Plus | $8/mo | $80/yr | Cursor's own models only, at fixed settings | 
| Premium | $30/mo | $300/yr | New agent users; 4x Plus usage | 
| Super | $100/mo | $1,000/yr | Daily agent users; 15x Plus usage | 
| Ultra | $200/mo | $2,000/yr | Power users running multiple agents or automations; 50x Plus usage | 

Premium, Super, and Ultra include:

- A weekly usage pool shared across Cursor and Grok
- Cloud agents and automations
- Code hosting via Origin
- AI code reviews via Bugbot
- Third-party models at API list price, paid from prepaid credit

Plus covers only Cursor's own models (Grok and Composer), with no third-party models. Start, the lower-priced plan for developers in India, is now a legacy plan.

### **Business plans**

| **Plan** | **Billed monthly** | **Billed yearly** | **Best for** | 
|---|---|---|---|
| Teams, Premium seat | $30/user/mo | Not listed | Developers on a team with typical agent usage | 
| Teams, Super seat | $100/user/mo | Not listed | Daily agent users; 15x Plus usage | 
| Teams, Ultra seat | $200/user/mo | Not listed | Heavy agent users; 50x Plus usage | 
| Enterprise | Custom | Custom | Organizations that need pooled usage, invoicing, SCIM, or advanced security | 

Teams adds:

- Centralized billing and admin
- A team marketplace for internal rules and plugins
- Agentic code review with Bugbot
- Cloud agents and automations with shared team context
- Usage analytics
- Team-wide privacy mode
- SAML/OIDC SSO

Enterprise adds usage pooled across Grok and Cursor for the whole team, invoice and PO billing, SCIM seat management, repository, model, and MCP access controls, audit logs, granular and group-level spend limits, and priority support.

### **Cursor and Grok model rates (per million tokens)**

| **Model** | **Input** | **Output** | 
|---|---|---|
| Composer 2.5 | $0.50 | $2.50 | 
| Composer 2.5 (Fast) | $3 | $15 | 
| Grok 4.7 | $2 | $6 | 
| Grok 4.7 (Fast) | $4 | $12 | 
| Grok 4.6 | $2 | $6 | 
| Grok 4.5 | $2 | $6 | 

These rates apply to Cursor model usage beyond the weekly pool, paid from prepaid credit.

### **Third-party model rates (per million tokens)**

| **Model** | **Input** | **Output** | 
|---|---|---|
| Claude Sonnet 5.5 | $2 | $10 | 
| GPT-5.6 Terra | $2 | $12 | 
| Claude Opus 5.5 | $4 | $20 | 
| Claude Fable 5.2 | $8 | $40 | 

Third-party models aren't in the weekly pool. They're billed at these API list prices from the first request.

## **Cursor hidden costs and fees**

Cursor's seat price is often well below the actual bill, because third-party models are billed on top of every plan. These are the costs that drive the gap:

| **Cost** | **What triggers it** | **Impact** | 
|---|---|---|
| Prepaid credit and top-ups | Third-party models, and Cursor models beyond the weekly pool | Third-party usage is billed from the first request, and Auto Top-up refills credit automatically unless you set a monthly limit | 
| Cursor Token Rate | Third-party models on Teams and Enterprise | $0.25 per million tokens on top of the model's API price, including Balance and Intelligence routes and bring-your-own-key usage. Cursor's own models, Cost mode requests, and third-party models with cache reads under $0.10 per million tokens are exempt. | 
| Model choice | Selecting premium models | Claude Fable 5.2 costs 16 times as much as Composer 2.5 for both input and output | 
| Fast mode | Choosing faster variants | Composer 2.5 Fast costs 6 times the standard rate, at $3 input and $15 output | 
| Unpooled usage | Uneven usage across a Teams plan | Included usage belongs to each user and doesn't transfer, so light users' unused allowance can't cover heavy users. Only Enterprise pools usage. | 
| Departing members | Removing someone who has used credits | Their seat stays billed until the end of the cycle | 
| Data residency | Opting into regional data residency | 10% uplift on eligible model pricing | 
| Taxes | Every plan | Prices exclude applicable taxes | 

## **How to negotiate lower Cursor pricing**

- Benchmark your total cost, not the seat price. Compare your seat count and monthly additional usage spend to what similar companies actually pay for Cursor. Usage drives the bill, so a seat-only comparison will understate your cost.
- Pay yearly. Annual billing costs the same as 10 months: Premium is $300 a year instead of $360, and Ultra $2,000 instead of $2,400. The discount applies to the subscription, not to third-party or additional usage, so it saves the most for teams that stay within their included usage.
- Match seat tiers to usage. A Super seat costs 3.3 times as much as Premium and includes 3.75 times the usage (15x Plus against 4x). That's better value for developers who would otherwise buy additional usage, and a waste for everyone else.
- Set a team-wide spending limit on day one. Teams share one prepaid balance, and Auto Top-up can refill it automatically. A monthly additional usage limit keeps overages within budget while you learn your team's real usage.
- Default to Cursor's own models where quality allows. Composer 2.5 and Grok are exempt from the Cursor Token Rate and are the only models covered by the included pool. Reserve frontier third-party models for work that needs them.
- Ask for a volume discount directly. Cursor's docs direct larger teams to enterprise@cursor.com for volume discounts. Because Cursor sells only direct, this is the only route to a negotiated rate.
- Consider Enterprise once usage is uneven. If a few heavy users drive most of your additional spend while others leave included usage unused, pooled usage on Enterprise can lower your total. Granular and group-level spend limits are also available only on Enterprise.
- Remove inactive seats before the billing cycle ends. Cursor bills for active seats and adjusts as you add or remove people. A member who used credits stays billed until the cycle closes, so make seat changes before renewal, not after.
- Bring a competing quote. Cursor competes with GitHub Copilot, Anthropic's Claude Code, and other AI coding tools. A credible alternative evaluation strengthens your position on volume pricing and Enterprise terms.

SMB Pricing*

(50-1,000 employees)

SMB Pricing*

(50-1,000 employees)

JetBrains

Cursor

**Delta**

Enterprise Pricing*

(1,000+ employees)

JetBrains

Cursor

**Delta**

*based on annual average spend

SMB Pricing*

(50-1,000 employees)

SMB Pricing*

(50-1,000 employees)

Sonar

Cursor

**Delta**

Enterprise Pricing*

(1,000+ employees)

Sonar

Cursor

**Delta**

*based on annual average spend

SMB Pricing*

(50-1,000 employees)

SMB Pricing*

(50-1,000 employees)

Progress

Cursor

**Delta**

Enterprise Pricing*

(1,000+ employees)

Progress

Cursor

**Delta**

*based on annual average spend

SMB Pricing*

(50-1,000 employees)

SMB Pricing*

(50-1,000 employees)

AG Grid

Cursor

**Delta**

Enterprise Pricing*

(1,000+ employees)

AG Grid

Cursor

**Delta**

*based on annual average spend

SMB Pricing*

(50-1,000 employees)

SMB Pricing*

(50-1,000 employees)

Embarcadero Technologies

Cursor

**Delta**

Enterprise Pricing*

(1,000+ employees)

Embarcadero Technologies

Cursor

**Delta**

*based on annual average spend

helps you track, optimize, and save on business expenses

JetBrains creates intelligent software development tools and advanced collaboration solutions, including IDEs like IntelliJ IDEA and PyCharm, and the Kotlin programming language.

Sonar (SonarSource SA): code-quality and security platform—SaaS & self-hosted SonarQube with IDE integrations and security rules to detect bugs, vulnerabilities and code smells for enterprises.

Progress delivers enterprise software: app platforms (OpenEdge), digital experience (Sitefinity), developer tools (Telerik), data connectivity (DataDirect), secure file transfer (MOVEit) and observability.

AG Grid Ltd. is a self-funded company specializing in high-performance JavaScript data grids for enterprise applications, offering both free and commercially-licensed versions.

Provider of strategic data management solutions. The company delivers database tools that companies use to design, develop and manage databases and the data they contain. The company's flagship database tools include: ER/Studio, DBartisan, rapid SQL and change manager. The company has offices in Melbourne, Australia, Munich, Germany and Maidenhead, United Kingdom.
