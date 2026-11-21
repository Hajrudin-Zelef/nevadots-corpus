---
id: collect-261121-veille-ia/cursor-ide/cursor-review-2026-features-pricing-verdict-pick-right
title: "Cursor Review 2026: Features, Pricing & Verdict | Pick Right"
domain: cursor-ide
role: reference
task: article
actors: ["Anysphere"]
dates: []
keywords: ["cursor", "premium", "grok", "pricing", "models", "pool", "teams", "claude"]
source: https://pick-right.com/tools/cursor/
source_anchor: ""
source_lines: [1, 187]
section: "Cursor_IDE"
---
Review draws on 14 primary sources (vendor announcements, named publications, benchmark results) and is updated continuously as the product changes. See the methodology page for the full research process.

**TL;DR:** Cursor is still the best AI *editor* â Tab autocomplete, Cmd+K inline edits and the visual diff loop have no equal â while Claude Code is the better *agent* for refactors spanning ten files. But **the $20 Pro plan closed to new signups on 12 October 2026**. New buyers choose **Plus at $8** (Grok models only) or **Premium at $30**, where Claude and GPT are still available but billed separately at API list prices on top of the subscription. If you hold legacy Pro, keep it. If you are buying now and your model is Claude or GPT, a flat-fee agent is cheaper than a metered editor. **OpenAI also ends model supply on 12 November 2026.**

## The rate card after the 12 October repricing

On 12 October 2026 SpaceXAI merged Cursor, Grok and X Premium into a single ladder called **The X Subscription**. This is the one Cursorâs pricing page now sells:

| Tier | Price | What it is | 
|---|---|---|
| **Free** | $0 | Limited Agent and Chat requests. Enough to evaluate, not to work. | 
| **Plus** | **$8/mo** | **Grok models only** in Cursor, plus Grok Chat, Voice, Imagine, Build, the Grok API and X Premium features. No Grok Bot, no third-party models. | 
| **Premium** | **$30/mo** | 4x the Plus pool. Cloud agents, automations, code hosting, AI code reviews, Grok Bot. Third-party models available, **billed separately at API list** . | 
| **Super** | **$100/mo** | 15x the Plus pool, plus priority model access at peak times. | 
| **Ultra** | **$200/mo** | 50x the Plus pool, plus earliest access to new products and models. | 
| **Teams** | **from $30/user/mo** | 4â50x the Plus pool, team marketplace, SSO, analytics, spend controls. | 
| **Enterprise** | Custom | Pooled usage, invoice/PO billing, SCIM, audit logs. | 

Prices verified 15 October 2026 against cursor.com/pricing; the pricing tracker carries the dated before-and-after.

**Annual billing exists, in the docs only.** Cursorâs billing documentation added it on 15 October 2026 â **$80/yr Plus, $300/yr Premium, $1,000/yr Super, $2,000/yr Ultra** â which is exactly two months free (16.67%) on each tier. It is offered on the individual tiers only, not Teams or Enterprise, and it appears nowhere on cursor.com/pricing. The pool also *refreshes weekly*, so prepaying a year buys 52 allowances rather than one large one. Why monthly is still the rational tenancy.

**The legacy ladder still exists, and staying on it is now documented as permanent.** Cursor Pro at $20, Pro+ at $60, Ultra at $200, Teams Standard at $40/user, Teams Premium at $120/user and the India-only Start plan at â¹649 all continue for existing subscribers â including their two included pools, âCursor Modelsâ and âOther Modelsâ, the second of which is included third-party usage drawn down at each modelâs API price. Cursorâs legacy plans page now states that âswitching to The X Subscription is optional and permanentâ and that âyour legacy plan, price, and features continue unless you change or cancel.â That is stronger than a grandfathered rate with an unannounced expiry: there is no migration date, and the published mapping â Pro to Premium, Pro+ to Super, Teams Standard to a Teams Premium seat â moves you to a tier that âdrops the included third-party model allowance.â If you hold it, keep it. Full alternatives guide with the metered-versus-flat arithmetic.

**The change that matters is not the price, it is the pool.** Cursorâs pricing page says plainly that on Plus *âonly Grok models are availableâ* and that on Premium and above *â3rd party models are available in Cursor, billed separately by usage.â* Claude and GPT have not been removed from the editor; they have been moved out of the subscription and onto a meter at list prices. Worked example at published rates: a developer running Claude Sonnet 5.5 ($2 in / $0.10 cached / $10 out per million tokens) through a heavy agentic day â about 3M cached-prefix tokens, 200K fresh input, 150K output â spends roughly $2.20, or about $46 across 21 working days, **on top of** Premiumâs $30. The same work on Grok 4.7 costs nothing extra because it is inside the pool.

**And no tier publishes its capacity.** The old ladder never stated an allowance; the new one formalises that by publishing only *relative* values â Premium is â4x Plusâ, Super 15x, Ultra 50x â while the absolute size of Plus is published nowhere. So the only verifiable numbers in a Cursor bill are the subscription fee and the API list rates for models outside the pool, and the second is not a cap. You still cannot compare *what one dollar buys* here against GitHub Copilot, which publishes 1,500 credits at $10 Pro and 20,000 at $100 Max. For a side-by-side of every published coding-tool allowance, see best AI coding tools.

**Recommendation:** Free to trial. **Legacy Pro holders: stay put** â you are on the better deal and nothing forces you off it. New buyers committed to Grok: **Plus at $8** is the best value on this page, and it bundles a frontier-ish model, an API and a social subscription with the editor. New buyers who want Claude or GPT: price **Premium at $30 plus metered tokens** against ChatGPT Plus at $20 and Claude Pro at $20, both of which include the model. Measure a month before you climb, because the vendor still will not tell you where the ceiling is.

## Who owns Cursor, and what that does to model choice

Cursorâs parent Anysphere was acquired by SpaceX for $60B in all stock; the deal **closed on 14 August 2026**. Cursor is no longer independent, and xAI sits inside the same parent â which has already changed what the product recommends.

Model breadth is narrowing rather than widening. On **28 August**, two weeks after the close, OpenAI invoked a change-of-control clause and moved to terminate model supply, proposing **12 November 2026** as the cutoff, with the position that **future OpenAI models will not be supplied to Cursor at all**. CEO Michael Truell put the exposure at ~5% of user traffic and said talks continue. A personal OpenAI API key restores **local Chat and Agent only** â not Tab, Auto mode, cloud agents, Automations or the CLI/SDK. Note also that 12 November is the *latest* date OpenAIâs change-of-control window permits and Cursor may end access sooner, so it is a ceiling rather than a deadline â our migration guide has the feature-by-feature survival table and the exits priced.

Meanwhile Grok has moved to the front of the shelf: Cursorâs pricing page now advertises âgenerous limits for Grokâ and âGrok Bot accessâ as named Pro features, alongside a generic âaccess to frontier modelsâ. Grok 4.7 went live in Cursor on 21 September 2026 at the same $2/$6 rate card as the model it replaced.

What remains is still a real multi-model dropdown â Claude Opus 5 and Sonnet 5, Gemini 3.7 Flash, Grok 4.7, plus Cursorâs in-house Composer and Tab models. Treat it as a narrowing advantage. If model neutrality is the reason you are choosing Cursor, that is the one claim on this page with a published expiry date.

## What Cursor is in 2026

A fork of VS Code with AI at every level. Same shortcuts, same extensions, same UI â then the AI surfaces appear.

**Tab autocomplete.** The feature users praise most consistently, and the one they report missing when they switch editors. It completes multiple lines, and predicts which file you will open next. Free and unlimited on every tier.

**Cmd+K inline edits.** Select code, describe the change, Cursor rewrites the selection. The workflow that defined the product.

**Composer and Agent mode.** Multi-file changes and autonomous task execution inside the editor â plan, edit, run terminal commands, iterate. You review a diff and accept hunk by hunk.

**Cloud agents, which now start themselves.** As of 19 August 2026 Subscriptions wake an agent on a pull request, Slack thread or schedule, `/goal` holds a long-lived objective across sessions, and subagents run on isolated VMs. Since **27 August** they can also start without a connected GitHub repository at all. Budget carefully: metering is per step and per file, so spend now tracks repository activity rather than how often anyone opens the editor.

**Projects, and two review bots.** **10 September 2026** added Projects â coordinator agents for bodies of work larger than one task. **23 September** added **Rollouts** (deployment monitoring) and **Security Review**, both on Teams and Enterprise only.

**Self-hosted machines.** From **2 September 2026**, tool execution can stay inside your own network. Note the limit: execution moves on-premises, the reasoning loop does not â the same boundary that applies to Coderâs Agent Relay.

**Origin, a Git forge.** Launched **18 August 2026** in early beta on all paid plans â repositories, code browsing, pull requests, on a from-scratch storage layer, designed to run alongside GitHub rather than replace it. One vendor now sells your editor, your agents, your models and your code host.

**Codebase indexing.** âUse our existing auth functionâ works because Cursor has indexed yours.

## What Cursor genuinely does well

**Rapid UI and frontend iteration.** The clean win. âSee the code, change a line, see the result in the browserâ is smoother here than in Claude Codeâs terminal-first loop. For React, Vue and Svelte visual work, Cursor is faster.

**Tab completion quality.** Still the category benchmark, and still ahead of Copilotâs completion on how much it predicts at once.

**Visual diff review.** Changes arrive as a diff you accept hunk by hunk â better than terminal-only output for reviewing AI-written code carefully.

**Mix-and-match models in one workspace.** Claude for reasoning, Composer for in-house speed, Tab for typing â in the same session. Narrowing, as above, but not gone.

**Zero learning curve for VS Code users.** Extensions, debugging, source control and terminal integration all work unchanged. You are not learning a new editor.

**Greenfield speed.** The right tool for âworking app in two hoursâ projects where iteration speed beats refactor depth.

## Where Cursor falls short

**Complex multi-file refactors.** When a change genuinely touches 10+ files and demands reasoning about side effects, Claude Codeâs agent is more reliable. Composer works but needs more back-and-forth. See Cursor vs Claude Code.

**Agent mode still reads as a response to competition** rather than an evolution of Cursorâs own product. For autonomous execution, Claude Code wins.

**Unpublished capacity is cognitive load.** You are permanently half-aware of whether you are on Auto mode or a frontier model, with no published allowance to check against.

**Stacking gets expensive.** $20 Cursor + $20 Claude + $20 ChatGPT is a real and common $60/month.

**Hallucinated code still happens.** Cursor runs the same frontier models as everyone else, and they still invent functions. Review AI-written code, especially in unfamiliar repositories.

**Extension compatibility is not 100%**, and initial indexing on very large repositories takes time.

**Windsurf keeps pace.** Windsurf matches most of the feature set and sometimes prices sharper.

## Security: pin 3.0.0 or later before you run agents

Pillar Security disclosed three separate Cursor sandbox escapes in July 2026, all fixed in **3.0.0**: CVE-2026-48124, where a workspace-controlled `.claude` hook config became unsandboxed command execution; a virtualenv interpreter the Python extension runs on its own during discovery; and git metadata abuse that bypassed path-based rules via `core.fsmonitor`. That last one matters beyond Cursor â an unrelated team used the same git primitive against Claude Code two months later.

None of these broke the sandbox. In each case the agent stayed inside it and wrote a file that a trusted tool outside it later executed â which is the class of bug to design against, not the specific CVEs. Cursor fixed a separate Accomplish AI report in about a week, materially faster than Anthropic managed on the equivalent bug, but that ordering has since reversed between disclosures and is not something to procure on. If you run agent or auto-run mode, pin **3.0.0 or later** and keep long-lived cloud credentials off the machine.

## Cursor vs Claude Code vs Copilot

| If your bottleneck isâ¦ | Pick | Why | 
|---|---|---|
| Autonomous multi-file engineering | Claude Code | More mature agent; 46% most-loved vs Cursorâs 19% | 
| In-editor rapid iteration | **Cursor** | The visual loop is smoother than a terminal | 
| âJust write this lineâ autocomplete | **Cursor Tab** | Best-in-class, and free on every tier | 
| Enterprise procurement and IP indemnity | Copilot | Deepest corporate integration, $10 entry | 
| Published capacity per dollar | Copilot | The only one of the three that publishes allowances | 
| Raw adoption | Copilot | Still the category leader by headcount | 

The JetBrains April 2026 survey is the source of the awareness and sentiment figures: Cursor at **69% awareness** (second only to Copilot), tied with Claude Code at **18% at-work adoption**, but down to **#2 at 19% most-loved** against Claude Codeâs **46%**. Cursor did not get worse. The category split in two, and Cursor owns the editor half.

The common 2026 stacks: Cursor alone for greenfield UI work; Claude Code for agent tasks plus Cursor for editing on complex systems; Copilot plus Claude Code (or Cursor Teams) for corporate engineering. The solo-developer stack that used to be Cursor Pro plus Claude Pro at $40 flat is now Cursor Premium plus Claude Pro at $50 plus metered third-party tokens, which is why a second flat-fee agent such as Codex has become the cheaper pairing. Devin enters only when you want cloud-handoff async work.

## Who should use Cursor

- **Frontend and UI developers** â the iteration loop is genuinely best-in-class
- **VS Code users who want AI without changing editors** â Cursor is a superset
- **Solo developers and indie hackers** â $20 Pro covers most professional needs
- **Anyone picking their first AI coding tool** â fastest way to understand AI-in-editor

## Who shouldnât use Cursor as their primary tool

- **Engineers doing mostly complex multi-file refactors** â Claude Code handles these better
- **Terminal-first minimalists** â the editor-centric model will not fit
- **Teams that need GPT models after 12 November 2026** â supply is ending
- **Budget-constrained developers** â Copilot at $10 remains credible
- **Buyers who must model cost per unit before signing** â Cursor publishes no allowances

## FAQ

**Can I still buy Cursor Pro at $20?**
No. Pro closed to new signups on 12 October 2026 when SpaceXAI merged Cursor into The X Subscription. Existing Pro subscribers keep the plan and their included third-party model usage, and Cursorâs legacy plans page now states that switching is âoptional and permanentâ and that a legacy plan âcontinue[s] unless you change or cancelâ â so there is no migration date to plan around. New buyers choose Plus at $8 (Grok models only) or Premium at $30 (third-party models available, billed separately at API list). Which to buy instead: Cursor alternatives, priced.

**What happened to the Business and Teams plans?**
âBusinessâ became **Teams Standard** at $40/user/month in 2026, with **Teams Premium** at $120/user above it. Both are now legacy and closed to new signups. The current team plan is **Teams at $30, $100 or $200 per seat/month**, each seat priced to match the tier it sits on, sized at 4â50x the Plus pool. There is no annual billing on Teams.

**Will GPT models stop working in Cursor?**
That is what OpenAI has proposed, with **12 November 2026** as the cutoff and no future OpenAI models supplied at all. A personal OpenAI API key restores local Chat and Agent only â not Tab, Auto mode, cloud agents, Automations or the CLI/SDK. If GPT is load-bearing in your workflow, plan the migration now rather than in November â see what survives, and what the exits cost.

**Cursor or Claude Code, if I can only pay for one?**
Claude Code, on hardest-problem coverage. But you give up a lot of daily editor quality to get there, which is why most professionals who can afford both run both.

**Is Cursor safe to run in agent mode?**
With version **3.0.0 or later**, and without long-lived cloud credentials on the machine. Three sandbox escapes were disclosed and fixed in July 2026; in every case the agent wrote a file that a trusted process outside the sandbox later ran.

**Does Cursor have a cheaper plan in some countries?**
Yes â a **Start** plan at â¹649/month, tax inclusive, documented for India only. It does not appear on the global pricing page.

## The verdict

Cursor is the best AI code editor in 2026 even though it is no longer the most-loved AI coding tool. Losing the top slot to Claude Code reflects a category that split rather than a product that declined: âeditor with AIâ and âagent in a terminalâ are now separate purchases, and Cursor owns the first one.

Buy Pro at $20. Add Claude Code when problems start exceeding what Composer can finish. Two things would make this pageâs recommendation weaker rather than stronger over time â a vendor that will not publish capacity, and a model dropdown with one supplier already leaving â so treat Cursor as an excellent editor you can switch out of, not a platform to standardise on.

Related reading: best AI coding tools Â· best AI harnesses, where the model-supply question is treated as a buying criterion Â· Cursor vs Claude Code Â· Cursor is now purchasable through the Claude Marketplace from 23 September 2026, if you hold committed Anthropic spend you risk underconsuming Â· Slack Codeâs launch partners, where Cursor is notably absent

## What changed

- **15 Oct 2026:** Annual billing appeared in the billing docs â $80/yr Plus, $300/yr Premium, $1,000/yr Super, $2,000/yr Ultra, two months free on each, individual tiers only and absent from the public pricing page (analysis).
- **21 Oct 2026:** Correction â the included pool is**not** Grok-only: Cursorâs docs say it covers Grok 4.5, 4.6, 4.7**and Composer 2.5** . Staying on legacy Pro is now documented as permanent, not merely undated. See Cursor alternatives.
- **13 Oct 2026:** Cursor folded into The X Subscription on 12 October â Pro at $20 closed to new signups, the ladder is now Free / $8 / $30 / $100 / $200, and the usage pool covers**Grok models only** , with Claude and GPT metered at API list on Premium and above.
- **5 Oct 2026:** 12 November re-verified as still**proposed and unconfirmed** â and clarified as the*latest* date OpenAIâs change-of-control window allows, not a chosen deadline, so it can only move earlier. See the migration guide.
- **3 Oct 2026:** Rate card re-verified: Cursorâs billing documentation lists**Pro+ $60, Ultra $200, Teams Premium $120 and an India-only Start plan at â¹649** , none of which appear on the public pricing page; no tier publishes a credit allowance. âBusinessâ is now âTeams Standardâ at the same $40.
- **23 Sep 2026:** Cursor added**Rollouts** and**Security Review** bots, Teams and Enterprise only.
- **21 Sep 2026:** Grok 4.7 went live in Cursor at the same $2/$6 rate card as Grok 4.6.
- **10 Sep 2026:****Projects** shipped â coordinator agents for work larger than a single task.
- **2 Sep 2026:****Self-hosted machines** keep tool execution in your network; inference still leaves it.

## Cursor â frequently asked questions

## What does Cursor do?

A fork of VS Code with AI at every level. Same shortcuts, same extensions, same UI â then the AI surfaces appear. Tab autocomplete. The feature users praise most consistently, and the one they report missing when they switch editors. It completes multiple lines, and predicts which file you will open next. Free and unlimited on every tier.

## What are the downsides of Cursor?

Complex multi-file refactors. When a change genuinely touches 10+ files and demands reasoning about side effects, Claude Code's agent is more reliable. Composer works but needs more back-and-forth. See Cursor vs Claude Code. Agent mode still reads as a response to competition rather than an evolution of Cursor's own product. For autonomous execution, Claude Code wins.

## Who should use Cursor?

Frontend and UI developers â the iteration loop is genuinely best-in-class VS Code users who want AI without changing editors â Cursor is a superset Solo developers and indie hackers â $20 Pro covers most professional needs Anyone picking their first AI coding tool â fastest way to understand AI-in-editor

## Who shouldn't use Cursor?

Engineers doing mostly complex multi-file refactors â Claude Code handles these better Terminal-first minimalists â the editor-centric model will not fit Teams that need GPT models after 12 November 2026 â supply is ending Budget-constrained developers â Copilot at $10 remains credible Buyers who must model cost per unit before signing â Cursor publishes no allowances

## Is Cursor worth it in 2026?

Cursor is the best AI code editor in 2026 even though it is no longer the most-loved AI coding tool. Losing the top slot to Claude Code reflects a category that split rather than a product that declined: "editor with AI" and "agent in a terminal" are now separate purchases, and Cursor owns the first one. Buy Pro at $20. Add Claude Code when problems start exceeding what Composer can finish. Two things would make this page's recommendation weaker rather than stronger over timâ¦
