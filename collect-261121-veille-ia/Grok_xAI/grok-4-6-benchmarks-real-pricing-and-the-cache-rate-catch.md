---
id: collect-261121-veille-ia/grok-xai/grok-4-6-benchmarks-real-pricing-and-the-cache-rate-catch
title: "Grok 4.6: benchmarks, real pricing, and the cache-rate catch"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: []
keywords: ["grok", "model", "real", "models", "does", "output", "tokens", "knowledge"]
source: https://www.eesel.ai/blog/grok-4-6
source_anchor: ""
source_lines: [1, 229]
section: "Grok_xAI"
---
xAI released Grok 4.6 on August 12, 2026, five weeks after Grok 4.5. It is a real step up: Artificial Analysis puts it at 60.92 on the Intelligence Index, sixth of 95 measured models, in a statistical dead heat with GPT-5.6 Sol Max at 60.93, and it takes outright first place of 246 models on GPQA Diamond at 94.9%. It does that at $2 input and $6 output per million tokens, which makes it the highest-scoring model on the board at that price.

Here is the part the launch coverage skipped. The sticker price did not change, but the bill did. Grok 4.6's cached input is $0.50 per million against Grok 4.5's $0.30, and it writes 47% more output per task. Artificial Analysis measured the same benchmark suite costing $1,068 on Grok 4.6 versus $579 on Grok 4.5, a 1.84x jump at identical headline rates. Cache reads and writes are 57% of 4.6's bill. If your workload resends a big fixed prompt on every request, which is exactly what a support queue does, "same price" is not what you are getting.

And for anyone thinking about putting this in front of customers: the number that actually moved in the right direction is the hallucination rate, down from 54.1% to 34.3%. That is a real improvement and it is still one wrong answer in three. I have watched eesel's own agent confidently narrate work it never did, which is why I now run every rollout against a customer's historical tickets before it replies to a live human. A better model raises the ceiling. It does not build the floor.

## What Grok 4.6 actually is

The get-started page calls it, in xAI's own words, "SpaceXAI's frontier model built for coding, agentic tasks, and knowledge work". The company name really does render as "SpaceXAI" in the prose on its own docs now, while the domains, the `xai_sdk` package and the `@ai-sdk/xai` provider all still say xAI. I am reporting that as published rather than tidying it up.

The interesting shift is one of positioning. Grok 4.5 was framed narrowly on its own model page, as a "coding model for agentic software, engineering, and workflow tasks". The models index now points both the Code and the Chat recommendation at 4.6 and calls it "our flagship model for code and everything else." That is xAI folding its specialist line back into a general-purpose flagship.

The spec sheet, all of it from xAI's own pages:

| Property | Grok 4.6 | 
|---|---|
| API model name | `grok-4.6` (no aliases published) | 
| Context window | 500,000 tokens | 
| Output limit | "No text output limit", per the docs | 
| Knowledge cutoff | February 1, 2026 | 
| Reasoning effort | low / medium / high / xhigh, default `high` , cannot be disabled | 
| Modalities | Text and image in, text out; images 20 MiB max, jpg or png only | 
| Built-in tools | Function calling, web search, X search, code execution | 
| Rate limits | 150 requests/sec, 50M tokens/min | 
| Regions | us-east-1 and us-west-2 only | 
| APIs | Responses API and Chat Completions | 

Three of those rows deserve a second look.

**`xhigh` is the one real new capability.** It is a fourth reasoning tier above the `high` default, and it only exists on 4.6. The trap is what happens on older models: xAI's reasoning docs say that on models which do not support it, "such as `grok-4.5`, requests with `xhigh` are treated as `high`." A silent downgrade, not an error. If you ship a config with `xhigh` in it and fall back to 4.5 during an incident, nothing in your logs will tell you the model quietly stopped trying as hard.

**The knowledge cutoff is February 1, 2026, and xAI is blunt about the consequence.** The models page states that "Grok has no knowledge of current events or data beyond what was present in its training data." Anything current needs Web Search or X Search switched on server-side, and those bill $5 per 1,000 calls on top of tokens. For a support use case that means order status, current stock, this week's policy change: none of that comes from the model, all of it comes from your own systems.

**No aliases.** Grok 4.5's page lists `grok-4.5-latest` and `grok-build-latest`; the 4.6 page has no Aliases row at all. So `grok-4.5-latest` still resolves to 4.5, and if you have been relying on a `-latest` tag to ride upgrades automatically, you did not get this one.

## The benchmarks, read properly

I want to separate two things here, because xAI's own launch table mixes them. Three of its ten rows are Artificial Analysis's independent measurements republished. The other seven are xAI's own runs with no outside confirmation. Both are worth reading, but only one is evidence.

Start with the independent set. Artificial Analysis scores Grok 4.6 (high) at 60.92 on Intelligence Index v4.1.1, a nine-eval composite, putting it sixth of 95 measured non-deprecated entries against a class median of 34.

| Rank | Model | Intelligence Index | 
|---|---|---|
| #1 | Claude Opus 5 (max) | 63.05 | 
| #2 | Claude Opus 5 (xhigh) | 62.52 | 
| #3 | Claude Fable 5 (max) | 62.07 | 
| #4 | Claude Opus 5 (high) | 61.48 | 
| #5 | GPT-5.6 Sol (max) | 60.93 | 
| **#6** | **Grok 4.6 (high)** | **60.92** | 
| #7 | Kimi K3 (max) | 59.70 | 
| #14 | Grok 4.5 (high) | 55.76 | 

The gap between fifth and sixth is 0.007 index points, so xAI's claim that 4.6 "matches GPT-5.6 Sol" is fair rather than spun. What makes it interesting is the price column next to it: every model scoring above Grok 4.6 charges $12 or $25 per million output tokens, and Grok 4.6 charges $6.

Per benchmark, against its own predecessor:

| Benchmark | Grok 4.6 | Grok 4.5 | Move | 
|---|---|---|---|
| GPQA Diamond | **94.9%** (#1 of 246) | 93.1% | +1.8 | 
| Terminal-Bench v2.1 | 88.4% (#3) | 81.6% | +6.8 | 
| Tau-3-Banking (agentic tool use) | 50.7% (#2) | 42.1% | +8.6 | 
| GDPval-AA v2 (Elo) | 1753 (#3) | 1526 | +227 | 
| AA-Briefcase (Elo) | 1577 (#4) | 1313 | +264 | 
| AA-LCR (long context) | 75.0% (#27) | 74.0% | +1.0 | 
| Humanity's Last Exam | 42.9% (#15) | 42.7% | +0.2 | 
| SciCode | 53.6% (#16) | 54.1% | **-0.5** | 

GPQA Diamond is the standout, and it is a real first place across 246 models, ahead of Gemini 3.1 Pro and GPT-5.6 Sol Max at 94.1%. SciCode is the only place 4.6 went backwards, by half a point, which is noise rather than a regression story.

Two honest caveats. First, **there is no speed data for Grok 4.6 yet.** Artificial Analysis's timing objects are empty on its page and it flags the performance source as first-party from xAI, so I cannot give you a tokens-per-second or time-to-first-token figure, and I am not going to carry 4.5's 55.62 tokens/sec across and pretend it applies. Second, xAI's own table reports Terminal-Bench **v3.0** at 26%, while Artificial Analysis reports Terminal-Bench **v2.1** at 88.4%. Those are different benchmarks with wildly different difficulty, and putting them in the same sentence is how a post accidentally implies a 62-point collapse. On the harder v3.0 set, per xAI's own table, Grok 4.6 sits behind both GPT-5.6 Sol at 34.6% and Fable 5 at 34.1%. Credit to xAI for publishing the rows it loses.

### The one benchmark a support team should care about

Everything above is a leaderboard. This is the number that decides whether the model can talk to your customers.

Artificial Analysis runs AA-Omniscience, which rewards correct answers, penalises fabricated ones, and does not penalise saying "I don't know."

Read that carefully, because it is the most interesting result in the whole dataset. Grok 4.6's raw accuracy actually *fell* by 3.4 points. Its Omniscience score went up anyway, because the hallucination rate dropped nearly 20 points, from 54.1% to 34.3%. The model got better by making things up less often, not by knowing more.

That is the right direction, and it is also why I would not hand it an unsupervised queue. A third of its confident answers are still wrong. On a support ticket, a wrong answer delivered fluently is worse than no answer at all, because the customer acts on it. The mechanism that saves you here is not a bigger model, it is a confidence score threshold, scoped grounding in your own content, and clean escalation when the model is unsure. I wrote up the whole pattern in preventing AI hallucinations in support, and the testing side in adversarial testing.

I will spend eesel's own credibility here rather than a competitor's. The worst failure mode I have seen in eesel's own product, logged against production, was not a factual slip. It was this:

"The agent fabricates success. Worst observed pattern: narrating 'executing Zendesk searches' for ~10 turns without ever hitting the API; reporting files 'saved' that don't exist; fabricating metrics. Nothing kills a teammate faster than lying about what it did."


That is from eesel's own internal engineering notes, not a competitor review. It is the concrete reason I simulate before going live rather than trusting a benchmark, and it is why a 19-point hallucination improvement reads to me as encouraging rather than sufficient.

## What Grok 4.6 costs, and the one line that moved

Here is the full live table from xAI's pricing page, every text model, both context bands, no rounding.

| Model | Context | Input | Cached | Output | Long: in / cached / out | 
|---|---|---|---|---|---|
| **`grok-4.6`** (New) | 500k | **$2.00** | **$0.50** | **$6.00** | $4.00 / $1.00 / $12.00 | 
| `grok-4.5` | 500k | $2.00 | $0.30 | $6.00 | $4.00 / $0.60 / $12.00 | 
| `grok-build-0.1` | 256k | $1.00 | $0.20 | $2.00 | $2.00 / $0.40 / $4.00 | 
| `grok-4.3` | 1M | $1.25 | $0.20 | $2.50 | $2.50 / $0.40 / $5.00 | 
| `grok-4.20-multi-agent-0309` | 1M | $1.25 | $0.20 | $2.50 | $2.50 / $0.40 / $5.00 | 
| `grok-4.20-0309-reasoning` | 1M | $1.25 | $0.20 | $2.50 | $2.50 / $0.40 / $5.00 | 
| `grok-4.20-0309-non-reasoning` | 1M | $1.25 | $0.20 | $2.50 | $2.50 / $0.40 / $5.00 | 

Note in passing that the flagship is not the widest-context model xAI sells. `grok-4.3` and the whole 4.20 family carry 1M context; 4.6 carries 500k. If your job is stuffing an enormous document into a single call, the year-old model is the one that can take it.

Now the line that moved. Input, output and context window are identical between 4.5 and 4.6. The only column where they differ is cached input, at $0.50 on 4.6 against $0.30 on 4.5, a 67% gap. Worth being precise about the direction: xAI did not raise a price on you mid-flight. 4.5's cache rate came *down* to $0.30 at some point after its July launch, and 4.6 shipped at the older $0.50 figure. The practical effect on the live table today is the same either way.

Why that one cell matters more than it looks:

A coding session is output-heavy: long generations, a context that keeps changing. A support workload is the mirror image. Every ticket resends the same system prompt, the same tone rules, the same slab of help-centre content, and gets back three sentences. That is a cache-dominated bill, and cached input is the one rate where 4.6 is the more expensive model.

You do not have to take my word for the size of the effect, because Artificial Analysis publishes its actual spend. Running the same Intelligence Index suite cost:

| Cost line | Grok 4.6 | Grok 4.5 | 
|---|---|---|
| Cache reads | $357.94 | $84.70 | 
| Cache writes | $253.63 | $111.33 | 
| Reasoning tokens | $374.97 | $312.20 | 
| Answer tokens | $57.96 | $46.98 | 
| Non-cached input | $23.96 | $24.01 | 
| **Total** | **$1,068.47** | **$579.21** | 
| **Per task** | **$0.8367** | **$0.3601** | 

1.84x the total bill at the same sticker price, and cache reads plus writes are 57% of it. Part of that is the rate, and part is behaviour: 4.6 generated 21,735 output tokens per task against 4.5's 14,795, a 47% increase, and consumed 2.4x the input tokens. Token efficiency was Grok 4.5's launch story, and Artificial Analysis's verdict on verbosity flipped from "better than average" to "somewhat higher than average" between the two releases. A model that thinks harder costs more even when the price card says it doesn't.

### Plug in your own numbers

The shape of your workload decides whether the upgrade is free or a 25% raise. So rather than argue about averages, here is the arithmetic with your inputs in it.

### The two edges nobody reads

Beyond the cache rate, two mechanics on this pricing page will surprise you at invoice time.

**The 200k cliff is a whole-request flip, not a tier.** xAI's wording is unambiguous: models with long-context pricing bill the long-context rates for all tokens in a request once the prompt reaches the threshold. A 201k-token prompt does not cost 2x on the extra thousand tokens. It costs 2x on all 201,000.

This is why xAI's own docs push context compaction as hard as they do. For an agentic loop that accumulates tool results turn after turn, compaction is a cost control, not a housekeeping nicety. If your agent drifts past 200k on turn nine, turn nine costs double from its first token.

**The cheap cached rate is not automatic.** This is the most useful thing on the whole 4.6 page, and it is unusually candid for a vendor doc. The 4.6 get-started page tells you to set a `prompt_cache_key`, or the `x-grok-conv-id` header on Chat Completions, because "it routes a conversation's requests to the same server, making cache hits reliable; without it you often pay full input price on a cache-cold server." Read that again: the default failure mode is paying $2.00 instead of $0.50. If you build against Grok 4.6 and skip one header, your bill can quadruple on the input line and nothing will look broken.

Then there is the batch inversion. The 20% Batch API discount applies to `grok-4.3` and the three `grok-4.20` variants. It does not apply to 4.6, 4.5, or `grok-build-0.1`. So for a batchable, cache-heavy job like overnight ticket classification or backlog triage, the year-old grok-4.3 is meaningfully cheaper than the flagship, at $1.25 input and $2.50 output with 20% off on top. The pricing table hides that entirely, and I would test 4.3 before assuming the newest model is the right one for bulk work.

Two smaller items worth knowing. Priority Processing is a separate 2x multiplier on all token types, and it only bills when the response confirms `"service_tier": "priority"`. And there is a $0.05 usage-guidelines violation fee per request for violations caught before generation in the Responses API, which means you can pay five cents for a request that produced nothing.

## Grok 4.6 against the other flagships

On sticker price alone, Grok 4.6 looks excellent. Every figure below is from the vendor's own page.

| Model | Context | Input /1M | Cached /1M | Output /1M | 
|---|---|---|---|---|
| **Grok 4.6** | 500k | $2.00 | $0.50 | $6.00 | 
| Claude Sonnet 5 | 1M | $2.00 | **$0.20** | $10.00 | 
| Claude Opus 5 | 1M | $5.00 | $0.50 | $25.00 | 
| GPT-5.6 Sol | 1.05M | $5.00 | $0.50 | $30.00 | 
| Gemini 3.1 Pro Preview | 1.05M | $2.00 | $0.20 | $12.00 | 
| DeepSeek V4 Flash | 1M | $0.14 | $0.0028 | $0.28 | 

On output, Grok 4.6 is the cheapest thing here that isn't DeepSeek: $6 against Sonnet 5's $10, Gemini 3.1 Pro's $12, Opus 5's $25 and Sol's $30. Combine that with a tied Intelligence Index score and you get a real result: Artificial Analysis measures Grok 4.6 at $0.8367 per Index task against GPT-5.6 Sol's $1.2312, so the same measured intelligence for 32% less, and 2.8x less than Claude Opus 5's $2.3369.

Now the reframe, and this is where a support reader should slow down. Look at the cached column, not the output column. **Claude Sonnet 5 caches at $0.20 against Grok 4.6's $0.50.** Sonnet 5 also has a 1M window with no long-context surcharge at all (Anthropic states a 900k-token request bills at the same per-token rate as a 9k one), and a flat 50% batch discount where Grok 4.6 gets none.

So the honest verdict splits by workload rather than by model:

- **Long outputs, changing context, code generation:** Grok 4.6 wins on price, clearly. $6 output against $10 or $30 is not close, and the benchmarks back the quality.
- **Short outputs, huge reused prompts, batchable volume:** which is to say a support queue. Sonnet 5 is 2.5x cheaper on the line that dominates your bill, has no 200k cliff to fall off, and halves in batch. Grok 4.6's cheaper output barely matters when your replies are three sentences long.

That is a more useful answer than "Grok 4.6 is cheap," and it is the kind of thing you only see if you read the cached column. If you want the wider picture on model choice for support specifically, my roundup of the best AI agent for the job walks through why the top-scoring LLM is rarely the right pick, and RAG versus a raw LLM covers the grounding half.

One fairness note on the comparison itself: cached input is not one thing across vendors. Anthropic and OpenAI charge separately to *write* a cache entry, Google charges hourly cache storage, and xAI and DeepSeek publish a read rate with no separate write fee on the page. Comparing the cached column alone flatters Anthropic and OpenAI slightly. Gemini 3.1 Pro also bills thinking tokens as output, so its $12 is understated on reasoning-heavy work, and it is still a Preview model.

## Where you can actually get it

For consumers, xAI's own pricing page publishes fewer numbers than you would expect, because grok.com renders its prices as animated digit-rollers that contain no actual figure. Here is what is published:

| Tier | Price | Notes | 
|---|---|---|
| Free | $0 | Real-time web and X search, Voice mode, Connectors. Does not name Grok 4.6 | 
| SuperGrok Lite | Not published | Grok Build access, Expert mode, 2x longer chats | 
| **SuperGrok** | **$30/month** | First tier that names Grok 4.6 as an entitlement, 5x longer chats | 
| SuperGrok Plus | $100/month | One-prompt app creation, 1080p video, priority at peak | 
| SuperGrok Heavy | Not published | Highest usage, larger agent team, X Premium+ included | 
| Business | $30/month per user | Seat management, RBAC, domain verification | 
| Enterprise | Custom | SSO, SCIM, audit controls, data residency, volume pricing | 

Grok 4.6 is named as an entitlement from SuperGrok up, not on Free. Paid tiers moved in June 2026 to a single shared weekly usage pool across Chat, Imagine, Voice and Build, and xAI publishes the mechanism but no absolute message counts. Hitting the pool is not a hard stop: you fall back to free-tier Chat and Voice limits. Extra Usage Credits start at $5, expire after a year, and xAI says plainly they are "priced at standard rates, which means the cost per action is higher than the effective rate you get from the included weekly usage in your plan."

The more consequential fact for anyone buying through procurement: Grok 4.6 is a direct-and-partner-API launch, not a hyperscaler one.

| Channel | Grok 4.6 at launch | 
|---|---|
| xAI API, Grok Build, OpenRouter | Live day one | 
| Cursor | xAI says live; Cursor's own models page still listed 4.5 only | 
| Azure AI Foundry | Still Grok 4.3 | 
| AWS Bedrock | Still Grok 4.3 | 
| GitHub Copilot | Still Grok 4.5 | 

If your organisation standardised on Bedrock or Copilot, you cannot reach 4.6 at all yet. If you are on Azure Foundry you are buying Grok 4.3 at $1.25 and $2.50, which is cheaper but two generations back. That gap closes eventually, and it matters right now if a procurement policy says all inference goes through one cloud. It is the same pattern I saw with Grok Bot and Grok Voice Think Fast 2: xAI ships to its own surfaces first and lets the resellers catch up.

## What a frontier model changes for a support queue, and what it doesn't

Every five weeks a new model arrives and the question lands in my inbox: should we switch? Having shipped the integrations that sit between a model and a helpdesk, my answer is nearly always that the model was not the bottleneck.

What Grok 4.6 does change, honestly:

- **Agentic tool use got better.** 50.7% on Tau-3-Banking, second of 97, up 8.6 points. If your automation calls out to an order system or a returns API mid-conversation, that reliability is the thing that makes workflow automation actually finish rather than stall halfway.
- **Fabrication dropped sharply.** 54.1% to 34.3% is the single largest improvement in the release, and it is the one that matters most when the output goes to a customer.
- **Cost per unit of intelligence improved against rivals.** 32% cheaper per task than a tied-scoring GPT-5.6 Sol is real money at volume.

What it does not change:

- **Your knowledge is still your knowledge.** The February 2026 cutoff means the model knows nothing about your refund policy, your SKUs or last Tuesday's outage. That comes from training on your knowledge base and past tickets, and no benchmark score substitutes for it.
- **34.3% hallucination still needs a gate.** Confidence thresholds, guardrails, and handoff to a human when the model is unsure. Measuring whether it should have answered at all is its own discipline, covered in containment rate and escalation quality.
- **Token price is the cheapest line in the build.** At the widget's defaults, Grok 4.6 costs about 1.5 cents of tokens per conversation. The integration, the triage rules, the review loop, the reporting, the on-call when it misbehaves: all of that dwarfs the tokens. Which is exactly why cost per resolution beats cost per million tokens as a planning number.
- **Buying still beats building for most teams.** That trade-off is its own decision, and build versus buy is where I would start on it rather than in a pricing table.
- **You still cannot tell what it will do on your tickets.** A leaderboard is a proxy. Your queue is the test. Evaluation practice exists precisely because the two disagree more often than anyone would like.

If you want the platform layer rather than one model, start with AI customer service software. For the metrics I would hold a rollout to, automated ticket resolution and resolution rate are the two that matter.

## eesel for the part the model doesn't do

Grok 4.6 is a good model. If you are shipping code, go use it, the price-to-score ratio is the best on the board. What it is not is a support agent, and the honest reason is that a raw model has no idea what your team already told this customer, no way to know when to stay quiet, and no record of what it did afterwards.

eesel is that layer. It plugs into Zendesk, Freshdesk, Gorgias and the rest in a few minutes, learns from your help centre, macros and resolved tickets so it already sounds like your team, and, the part I would push you toward hardest, runs a simulation over your own historical tickets before it replies to a single live customer. You see the accuracy on your data, fill the gaps it surfaces, re-run, and only then go live. Given a model that still fabricates a third of the time, that dry run is the difference between an experiment and an incident.

Pricing works on a fixed monthly price rather than tokens: plans start at $299/month for 500 credits, per the pricing page, with 1 credit per ticket or chat however many replies it takes, no seat fees and no charge for tickets your humans take. You get 100 free credits without a card, and you can route a slice first, so 1,000 tickets a month with 200 sent to the AI uses 200 credits, which fits the $299/month plan. Turn on optional overage with a cap you set if you want a hard ceiling. The security and enterprise pages cover the compliance side, and our customers cover whether it works.

Try eesel free, run the simulation, and let your own tickets tell you what the leaderboard can't.

## Frequently Asked Questions

`xhigh` reasoning setting. If you care less about the model and more about getting one onto a live queue, that job belongs to an AI helpdesk agent.`xhigh` is a fourth reasoning tier above the `high` default and exists only on 4.6, and per xAI's reasoning docs older models silently treat an `xhigh` request as `high` rather than erroring. For support workloads, prompt size is usually a retrieval problem before it is a context-window one.
