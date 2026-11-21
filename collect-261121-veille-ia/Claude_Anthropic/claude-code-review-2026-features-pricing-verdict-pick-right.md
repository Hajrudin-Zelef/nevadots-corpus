---
id: collect-261121-veille-ia/claude-anthropic/claude-code-review-2026-features-pricing-verdict-pick-right
title: "Claude Code Review 2026: Features, Pricing & Verdict | Pick Right"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["claude", "seat", "code", "month", "team", "credit", "credits", "plan"]
source: https://pick-right.com/tools/claude-code/
source_anchor: ""
source_lines: [1, 223]
section: "Claude_Anthropic"
---
Review draws on 16 primary sources (vendor announcements, named publications, benchmark results) and is updated continuously as the product changes. See the methodology page for the full research process.

Claude Code is Anthropicâs terminal coding agent, and in October 2026 it remains the strongest tool in the category for multi-file refactors, debugging and long autonomous tasks.

Buy **Pro at $20/month** if Claude Code is the only Anthropic product you pay for. Buy **Max at $100/month** if you also call the Claude API for anything, because Max returns $100 a month in API credits and Pro returns none â so the step up is cheaper in net outlay for anyone who already has an API bill. Buy **Ultra at $200** only to escape the five-hour window, not for a better rate: it replaced Max 20x on 30 October 2026 with 10x Pro usage weekly instead of 20x. For teams, **Team Standard at $25/seat now includes Claude Code**, which makes the long-standing advice to buy Premium purely for terminal access obsolete.

Skip it if your work is mostly in-editor autocomplete â Cursor and GitHub Copilot fit that shape better â or if you need a published capacity or uptime guarantee, because Anthropic publishes neither.

## Which plan to buy

| Plan | Price per month | Claude Code | Usage | Monthly API credit | 
|---|---|---|---|---|
| Free | $0 | Not included | Sonnet and Haiku only | â | 
| Pro | $20 ($17 annual, $200 up front) | Included | Baseline | **None** | 
| Max | $100 | Included | 5x Pro, 5-hour and weekly caps | $100 | 
| Ultra | $200 | Included | 10x Pro weekly, no 5-hour cap* | $200 | 
| Team Standard | $25/seat ($20 annual) | Included | 1.25x Pro per session | $20/seat, pooled | 
| Team Premium | $125/seat ($100 annual) | Included | 6.25x Pro per session | $100/seat, pooled | 
| Enterprise | $20/seat annual + usage at API rates | Included | Consumption-based | â | 

*No five-hour limit on Ultra applies only to accounts Anthropic has automatically verified from âaccount and payment historyâ; until then a temporary session limit applies and there is no way to apply for verification.

Prices verified 30 October 2026 against claude.com/pricing, the Team plan documentation, the Ultra plan documentation and Anthropicâs API credit article. Max and Ultra are monthly-only. Team plans require a minimum of two members, not five. Claude Code shares one usage pool with Claude on web, desktop and mobile â terminal work and chat draw down the same allowance.

### The API credit is the part nobody prices

Every paid Claude plan except Pro hands back a monthly API credit, and at the list rates the credit is worth exactly what the plan costs:

- **Max** â $100/month, $100 in credits.**1.00x.**
- **Ultra** â $200/month, $200 in credits.**1.00x.**
- **Team Standard** â $20/seat in credits against a $20 annual seat.**1.00x** annually, 0.80x if you pay monthly.
- **Team Premium** â $100/seat in credits against a $100 annual seat.**1.00x** annually, 0.80x monthly.
- **Pro** â $20/month,**no credits at all. 0.00x.**

Two consequences follow, and neither appears on Anthropicâs pricing page.

**First, Pro is the worst value per dollar of any paid plan for anyone who also uses the API.** If you have at least $100/month of genuine API spend, moving from Pro to Max takes your net outlay from $20 to $0 *and* multiplies your Claude Code allowance by five. The usual framing â â$80 more for 5x the usageâ â is wrong for that buyer.

**Second, Team Premiumâs credit pool caps at $500 a month**, which inverts the per-seat economics as a team grows. Five annual Premium seats cost $500 and return the full $500 cap: the API credit covers the entire subscription. Ten seats cost $1,000 and still return $500 â half. Twenty seats cost $2,000 and return a quarter. The seat price is flat; the credit behind it is regressive, so the per-head value of a Premium seat is highest at exactly five seats and falls from there.

The honest caveats: credits **expire at the end of each billing cycle** and never roll over, they are shared across the organisation and cannot be split between organisations, new subscribers must hold the plan for seven days before claiming, and â importantly â Anthropic states they âdonât change your usage limits in Claude, Claude Code, or Claude Cowork.â The credit buys API calls, not a bigger terminal allowance. If you have no API workload at all, it is worth $0 to you and Pro remains the right plan.

### What $20 of API billing buys instead

Claude Code also runs on pay-per-token API auth with no weekly cap, which makes the subscription a buy-or-not decision rather than a given. Anthropic publishes no absolute token allowance for any plan, so the only way to size the trade is to price the API side.

Take one turn in an established session: roughly 30,000 tokens of cached context read back, 2,000 tokens of fresh input, 1,500 tokens of output. At the current rate card:

| Model | Rate (in / out / cache read) | Cost per turn | Turns for $20 | 
|---|---|---|---|
| Haiku 5.5 | $0.10 / $0.50 / $0.01 | ~$0.0013 | ~16,000 | 
| Sonnet 5.5 | $2 / $10 / $0.10 | ~$0.022 | ~900 | 
| Opus 5.5 | $4 / $20 / $0.20 | ~$0.044 | ~450 | 

So $20 of API billing buys about **450 Opus 5.5 turns or 900 Sonnet 5.5 turns a month** â roughly 20 Opus turns per working day. Below that volume, API auth costs less than Pro and has no weekly ceiling. Above it, the subscription wins and the gap widens quickly: at 100 Opus turns a day the API bill passes $130/month while Max stays at $100 with credits attached.

The cache-read line is what decides this, not the headline rate. It is 5% of base input on Opus 5.5 and Sonnet 5.5, and on a long session the cached context is most of what you pay for. Sonnet 5.5âs cache read was cut from $0.20 to $0.10 on 7 October 2026, halving the dominant line item of an agent session.

One trap on the cheap end: **Haiku 5.5 is priced by prompt length.** A request whose prompt exceeds 100,000 tokens pays 5x on every rate, cache reads included, and the threshold counts cache hits toward the total. Agent sessions cross 100,000 tokens routinely, so Haikuâs 17x advantage over Sonnet evaporates exactly when a long session needs it. Opus 5.5, Sonnet 5.5 and Fable 5.1 include the full 1M-token window at standard pricing.

### A seatâs value has moved six times in six months

Anthropic has never published an absolute weekly token figure, so plan capacity is a percentage of a base only the vendor can see. What can be dated is each change:

| Date | What moved | For the buyer | 
|---|---|---|
| 6 May 2026 | 5-hour limits doubled on Pro, Max, Team and seat-based Enterprise; peak-hour throttling removed for Pro and Max | Better | 
| 13 May 2026 | A temporary 50% weekly boost came into force | Better | 
| 24 Jul 2026 | Opus 5 became the default Opus model at $5/$25 | Flat | 
| 24 Aug 2026 | Priority Tier withdrawn â the only service tier with a published uptime target | Worse | 
| 14 Sep 2026 | Weekly limits reset to +25% over the pre-May baseline â a 17% cut against the summer allowance | Worse | 
| 22 Sep 2026 | Opus 5.5 cut the frontier rate to $4/$20 and cache reads to $0.20 | Better | 
| 7 Oct 2026 | Sonnet 5.5 cache reads cut from $0.20 to $0.10 | Better | 

The pattern worth planning around: **token prices have fallen consistently and subscription capacity has not.** The September reduction landed in the same week the *Financial Times* reported Anthropic expecting a second consecutive quarter of adjusted operating profit on inference gross margins above 80%, against at least 14.8 GW of contracted compute. Budget a seat as a pricing instrument, not a capacity guarantee â and if a pipeline depends on a fixed monthly throughput, put it on API auth where the number is yours to compute.

## What Claude Code actually is

A terminal-based coding agent. You run `claude` in a project directory, describe a task in plain English, and it reads the relevant files, plans, edits across the codebase, runs tests, debugs failures and iterates until done. It installs with `npm install -g @anthropic-ai/claude-code` and integrates with VS Code, JetBrains IDEs, the Claude desktop app and the browser.

The model lineup as of October 2026: **Opus 5.5** at the top ($4/$20, shipped 22 September), **Sonnet 5.5** as the mid-tier default ($2/$10, 28 September), **Haiku 5.5** for cheap high-volume work, and **Fable 5.1** with a 1M-token context window. An optional fast mode runs Opus 5.5 at $8/$40 â double the base rate for faster output, on the first-party API only. Note that Haiku 4.5âs earliest retirement date is 15 October 2026; it is still marked Active and no deprecation notice has been issued, and Anthropicâs 60-day minimum notice means it cannot retire before mid-December at the earliest. Sonnet 4.5 is the one with a firm date: deprecated 30 September, retiring 30 November 2026.

The features that distinguish it:

**Agentic execution.** âRefactor the authentication system to use JWT with refresh tokensâ is a reasonable prompt. It will plan the change, edit a dozen files, update tests, run the suite and fix failures.

**Agent Teams and subagents.** Multiple agents work different parts of a task in parallel with a coordinator merging results. Subagents run in the background by default, and fork mode lets one inherit the full conversation.

**Projects (public beta).** Rebuilt in September, a project is one conversation where Claude coordinates parallel cloud threads, each a full session on its own branch. It is the strongest orchestration layer in the category, with two sharp edges covered under permissions below.

**MCP integration.** Connects to thousands of external tools and services â Drive, Jira, Slack, databases. MCP has become the de facto standard for wiring agents to real systems, and the Claude Marketplace now wraps the plugin directory in a 2,000+ connector catalogue.

**Skills and plugins.** Reusable instructions and community extensions, with `/skill-doctor` reporting what each skill costs in context and how often it is actually used.

**Terminal-native.** Unlike Cursor, it does not ask you to change editors.

## What it genuinely does well

**Multi-file refactors.** The single most valuable thing it does. Adding types across a module and its imports, converting class components to hooks, renaming a function across code, tests and docs â tasks that took hours, done in minutes with fewer mistakes.

**Understanding unfamiliar codebases.** It reads the key files, builds a model of the architecture and answers questions about how things work, catching patterns a human skim misses.

**Test-driven loops.** Write a failing test, hand it over with âmake this pass without breaking anything else,â and it iterates until green.

**Debugging.** Give it a stack trace and the expected behaviour, and it forms hypotheses, tests them and usually fixes the problem. The reasoning is visible rather than a black box.

**Migrations.** Parallel agents across schema, API layer and frontend, with a coordinator merging. This is where Agent Teams earns its keep.

**Documentation, tests and code review.** Reliably good output on command, and competent PR review â enough to substitute for human review on low-stakes changes.

## Where it falls short

**Terminal-first is a real barrier.** Cursor and Copilot meet developers inside the editor they already use; Claude Code asks for a different working paradigm. Developers already comfortable in a terminal will not notice. Others will.

**Capacity is undisclosed and has been cut.** See the dated table above. The practical effect is that you cannot forecast a month of work from the plan page, because the plan page contains no number to forecast from.

**It over-engineers.** Ask for a simple function, get a class with factories, interfaces and a test suite. Promptable, but the default bias is toward production scaffolding over a quick fix.

**Cost at the top is real, and it got worse per unit.** Ultra at $200/month is the most expensive mainstream developer subscription in the category, and the API credit only offsets it if you have API work to spend it on. Until 30 October 2026 the same $200 bought 20x Pro usage as Max 20x; Ultra publishes 10x weekly, so the top rung no longer buys capacity at a discount â it buys the removal of the five-hour session cap. On price alone, high-volume low-stakes workloads do better elsewhere: DeepSeek V4.1 Flash runs $0.15/$0.60 and halves off-peak, against Opus 5.5âs $4/$20 â though the quality gap at the frontier is wide enough that this is a workload decision, not a straight substitution.

**No uptime commitment.** Claude Code was named in most of Anthropicâs August 2026 incidents, including a near-three-hour outage on 24 August across four model tiers, and Priority Tier â the only service tier with a published uptime target â was withdrawn. For interactive use this is an annoyance. For CI or scheduled automation it is a design constraint: give the pipeline a fallback path.

**Billed refusals on API auth.** Since 24 September, refusals in the `bio`, `frontier_llm` and `reasoning_extraction` categories are billed even when they produce no output, and the charge lands in `usage.iterations` rather than the top-level counts. `reasoning_extraction` is a prompt shape rather than a harm category â it fires on instructions asking the model to write its internal reasoning into the response, which is exactly what legacy âshow your workâ wording in a custom system prompt or eval harness does. `cyber`, the category most likely to catch security-adjacent coding work, stays unbilled. Subscription plans are not metered per token and are not exposed at all.

**MCP coverage is uneven**, and for tight-loop visual work â change CSS, look, change again â Cursorâs preview integration is still smoother.

## Permissions, sandboxing and what you still own

Auto mode has been the default for new sessions on Pro, Max and Team plans since 14 August 2026: a classifier screens each shell command and network call instead of prompting, allowing routine work while blocking destructive and exfiltrating categories. Anthropicâs data showed humans caught 13.6% of planted dangerous commands against the classifierâs 89%, so it is a genuine improvement on rubber-stamp prompting. Since Claude Code 2.1.278 the classifier runs server-side and is no longer billed on API auth â but eligibility depends on the serverâs checks reaching the session, so an LLM gateway that strips the `safeguards` field, rewrites the `anthropic-beta` header or drops the `safeguard_results` key silently falls back to the billed path. Check `/status`; the **Auto mode server** row reads `Enabled` or `Disabled`.

Anthropicâs own position is that auto mode is a convenience feature backed by a best-effort classifier, **not a security guarantee** â and the boundary has failed publicly. In September 2026 Accomplish AI published Beltdown, a sandbox escape that never touched the sandbox: a malicious repo ships a *nested* `.git` folder carrying a poisoned `core.fsmonitor`, and because skill loading calls `git ls-files` outside the sandbox, git executes the payload with your privileges and no prompt. Update to **2.1.247 or later**; the partial hardening in 2.1.223 was not sufficient, and the fix shipped with no CVE and no advisory roughly 44 days after the report â so a disciplined upgrade process had no signal that this was the release that mattered. Cursor, Codex CLI and Gemini CLI were escaped through the same structural gap the same quarter, by other teams. It is not a vendor trait.

Two more gaps worth an hour before you roll this out:

- **Projects only enforces permissions in single-repository projects.** Add a second repository and Anthropicâs documentation is explicit that no repositoryâs settings file is read â the thread still loads every`CLAUDE.md` , so your instructions arrive while your enforcement does not. Plugin-provided hooks still run, which makes the gap harder to notice. Threads also run in auto mode, with approval prompts inside the thread rather than in the project conversation, and a new project defaults every thread to Opus at high effort, which Anthropic says âdraws on your plan fastest.â Change**Thread model** and**Thread effort** in Project settings first.
- **Plugins are reviewed at the door, not continuously.** Anthropicâs own directory warns it âdoes not control what MCP servers, files, or other software are included in plugins and cannot verify that they will work as intended or that they wonât change.â Scope credentials narrowly and re-check after updates.

For organisations that need execution inside their own perimeter, self-hosted environments are in public beta on Team and Enterprise â but note that **inference still goes to `api.anthropic.com`** and transcripts are stored. It relocates execution, not inference, and cannot route through Bedrock or Vertex. Self-hosted sandboxes with MCP tunnels remain the containment answer where blast radius matters. The controls Anthropic itself names as real: OS-level isolation, outbound network traffic blocked by default, identity verification between services, and reduced standing access to credentials.

## Claude Code vs the alternatives

| If you want | Buy | Why | 
|---|---|---|
| Complex multi-file changes with visible reasoning | **Claude Code** | Context handling and explanation are category-leading | 
| Async delegation â tasks that run while you are away | OpenAI Codex | Different philosophy: Codex codes while you sleep, Claude Code while you watch | 
| Rapid UI work with live preview | Cursor | Visual loop is tighter | 
| In-editor autocomplete | GitHub Copilot or Cursor Tab | Claude Code is a higher-level tool and is not designed for this | 
| The cheapest credible option | GitHub Copilot at $10/month | Copilot publishes an allowance you can compute; Anthropic does not | 
| Vendor independence and your own model choice | Aider | Apache-licensed, bring your own model | 
| A free tier that does real work | Gemini CLI | Still the free-tier leader | 

The plan question between the two â why Max and Ultra return their own price in API credits while every ChatGPT Pro tier returns nothing, and what each one does when you hit the wall â is worked through in Claude Code vs OpenAI Codex.

Two framing notes. OpenAI documented the Codex app-server as a platform to embed in August 2026, which puts it in the same category as the Claude Agent SDK â the SDK is a library linked into your runtime, the app-server a separate process over JSON-RPC. The process model isolates and restarts more cleanly; the library model deploys as one artefact. And when Google opened Opus 5 to all of its engineers but pointedly not Claude Code, it showed how large employers behave: they will approve a rivalâs *weights* long before a rivalâs *agent*, because the harness is where audit logs, sandbox policy and code retention live. Expect the model-only counter-offer if you are pushing for approval at a security-conscious employer.

Most professional developers run a stack rather than a single tool: Claude Code for refactors, debugging and architectural work; Cursor or Copilot for autocomplete while typing; Claude chat for planning. The subscription question above is really about how much of the first layer you buy.

## Who should buy it

- **Professional software engineers** â the most capable coding agent available
- **Technical founders and solo developers** â the multiplier on one personâs output is real
- **Anyone working in large or unfamiliar codebases** â navigation and comprehension are the standout strengths
- **Teams doing migration or refactoring work** â Agent Teams and Projects are built for exactly this
- **Developers already comfortable in a terminal**

## Who shouldnât

- **Occasional coders** writing a few scripts a month â Copilot at $10 is enough, or API auth below ~450 Opus turns a month
- **Beginners still learning to code** â you learn less when the agent writes everything
- **Developers whose work is 90% autocomplete** â Cursor or Copilot fit the workflow
- **Anyone who needs a contractual capacity or uptime figure** â Anthropic publishes neither, and Priority Tier is gone

## FAQ

### Is Claude Code included in the $20 Pro plan?

Yes. Pro at $20/month ($17 on annual billing) includes Claude Code alongside Claude on web, desktop and mobile, with access to Opus, Sonnet and Haiku. One usage pool covers both, so terminal work and chat draw down the same allowance. The free plan does not include Claude Code.

### Do I need Team Premium to get Claude Code for a team?

No â this changed. Claude Code is now included with every Team **Standard** seat at $25/seat/month ($20 annual), which carries 1.25x Proâs per-session allowance. Premium at $125/seat ($100 annual) buys 6.25x Pro per session, not access. Team plans require a minimum of two members.

### Is the subscription cheaper than paying per token?

It depends on volume. At current rates a typical Claude Code turn costs about 4.4 cents on Opus 5.5 and 2.2 cents on Sonnet 5.5, so $20 of API billing buys roughly 450 Opus or 900 Sonnet turns a month. Below that, API auth is cheaper and has no weekly cap. Above it the subscription wins â and if you have $100/month of API spend anyway, Max returns its entire price in credits.

### How much usage does each plan actually give me?

Anthropic publishes no absolute token figure for any plan, only multiples: Max is 5x or 20x Pro, Team Standard 1.25x Pro per session, Team Premium 6.25x. Limits reset on a rolling five-hour window with a separate weekly cap, Max plans carry a separate weekly limit for Fable, overflow can be bought at API rates, and sessions auto-continue when limits reset rather than stalling. The weekly cap was cut 17% against the summer allowance on 14 September 2026.

### Is it safe to let Claude Code run commands on my machine?

Treat auto mode as a useful filter on top of a boundary, not as the boundary. Run it on **2.1.247 or later** (the Beltdown fix), keep `ask` or `deny` rules on production deploys and credential handling, run unattended sessions in a container or VM with egress restricted at the network rather than in the harness, and keep long-lived cloud and SSH credentials off the machine the agent runs on. Anthropicâs own position is that the classifier is best-effort, not a security guarantee.

## What changed

- **30 Oct 2026:** Anthropic replaced Max 5x and Max 20x with**Max at $100 and Ultra at $200** . Its help centre calls it a rename âwith no price changeâ, but Ultra publishes 10x Pro usage weekly against the twenty the retired plan was named for, trading the five-hour session cap for half the weekly multiple â so the top rung no longer buys capacity at a discount. Plan table, credit arithmetic and recommendation updated; all prices re-verified. Analysis.
- **11 Oct 2026:** Team Standard now includes Claude Code â this page previously said Premium was required â and the Team seat minimum is two, not five. Added the API-credit arithmetic and the subscription-vs-API break-even.
- **8 Oct 2026:** Haiku 5.5 arrived at $0.10/$0.50 with a 5x surcharge above 100,000-token prompts, which agent sessions cross routinely.
- **7 Oct 2026:** Sonnet 5.5 cache reads cut from $0.20 to $0.10 â a 50% cut on the dominant line item of a long session.
- **28 Sep 2026:** Sonnet 5.5 shipped at $2/$10, inheriting Sonnet 5âs price unchanged.
- **22 Sep 2026:** Opus 5.5 shipped at $4/$20 with $0.20 cache reads; default effort dropped from`high` to`medium` and forced tool use now returns a 400.
- **21 Sep 2026:** Auto modeâs classifier moved server-side and stopped being billed on API auth â unless a gateway strips the`safeguards` field.
- **18 Sep 2026:** Projects beta â permission rules apply only in single-repository projects.

Related:

- GitHub Copilot vs Claude Code â Copilot publishes an allowance you can compute; Anthropic does not, and the shared Claude/Claude Code usage pool is the fact most buyers miss
- Cursor vs Claude Code â the editor-versus-terminal decision
- Best AI coding tools in 2026
- Best AI harnesses in 2026 â the model-agnostic case
- Model retirements tracker â every Claude lifecycle date on one page
- API cost calculator â price your own turn shape against every vendor

## Claude Code â frequently asked questions

## What does Claude Code do?

A terminal-based coding agent. You run `claude` in a project directory, describe a task in plain English, and it reads the relevant files, plans, edits across the codebase, runs tests, debugs failures and iterates until done. It installs with `npm install -g @anthropic-ai/claude-code` and integrates with VS Code, JetBrains IDEs, the Claude desktop app and the browser. The model lineup as of October 2026: Opus 5.5 at the top ($4/$20, shipped 22 September), Sonnet 5.5 as the mâ¦

## What are the downsides of Claude Code?

Terminal-first is a real barrier. Cursor and Copilot meet developers inside the editor they already use; Claude Code asks for a different working paradigm. Developers already comfortable in a terminal will not notice. Others will. Capacity is undisclosed and has been cut. See the dated table above. The practical effect is that you cannot forecast a month of work from the plan page, because the plan page contains no number to forecast from.
