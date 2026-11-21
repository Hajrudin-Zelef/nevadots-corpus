---
id: collect-261121-veille-ia/minimax/minimax-m3-1-flash-for-marketers-the-cheapest-model-leading-business-workflows-and-the-93-it-gets-wr
title: "MiniMax M3.1 Flash for Marketers: The Cheapest Model Leading Business Workflows — and the 93% It Gets Wrong"
domain: minimax
role: reference
task: article
actors: ["MiniMax"]
dates: []
keywords: ["minimax", "model", "flash", "input", "tokens", "benchmark", "business", "workflows"]
source: https://www.aimarketingfocus.com/posts/minimax-m3-1-flash-marketers/
source_anchor: ""
source_lines: [1, 112]
section: "MiniMax"
---
## Contents

Two numbers from the same model, released six days apart, pointing in opposite directions.

On October 19, 2026, MiniMax shipped M3.1 Flash: 428B total parameters, 23B active per token, a 1M context window, text/image/video in and text out. $0.15 per million input tokens, $0.60 per million output, $0.015 for cached input. On AutomationBench-AA — the benchmark for multi-step business workflows executed across SaaS systems, the kind where the model has to pull data out of a CRM, write to a spreadsheet, draft an email, and hand the whole thing back — MiniMax reports 62.8. That is the best score in the cheap tier: ahead of DeepSeek V4.1 Flash (60.5), Claude Opus 5 (50.3), GLM-5.3 (48.8), GPT-5.6 Sol (45.8), and Kimi K3 (46.7).

Then, from Artificial Analysis on the same model: on AA-Omniscience, M3.1 Flash answers 56% of factual questions correctly. Better than GLM-5.3 Flash (28%) and better than DeepSeek V4.1 Flash-1017 (43%). But it attempts almost every question. When it misses, it does not say "I don't know." It confabulates. Hallucination rate: 93%. Overall score on that benchmark: 16.

Highest accuracy in its class, worst honesty in its class, one model.

Everything below is built on published numbers — MiniMax's own launch post plus Artificial Analysis's independent runs — read six days after release. I haven't put my own marketing prompts through M3.1 Flash yet. Treat the routing as a hypothesis with a price tag attached, and the cost math as exact.

## What actually shipped

- **Architecture:** 428B total, 23B active per token, Mixture-of-Experts. Efficiency comes from MiniMax Sparse Attention plus DSpark speculative decoding.
- **Context:** 1,000,000 tokens in, 524,288 tokens out.
- **Input:** text, image, and video. Output: text only.
- **Reasoning:** always on. Five levels —`low` ,`medium` ,`high` ,`xhigh` ,`max` — with`max` as the default. You cannot turn it off.
- **Price:** $0.15/M input, $0.60/M output, $0.015/M cached input.
- **Access:** MiniMax's first-party API, the Token Plan subscription, and MiniMax Code. Open weights landed on Hugging Face and ModelScope on October 23.
- **Speed claims:** over 200 tokens/second at peak, median above 170. Nearly 3x faster than M3 online, and 70% cheaper on agent workloads.

The rollout detail that matters if you're on an older MiniMax model: starting October 19, Token Plan subscribers' M2.5 and M2.7 requests were automatically rerouted to M3.1 Flash. If your bill or your outputs shifted in the last week, that's why.

Also worth knowing before you wire it up on OpenRouter: the 72-hour free launch period ended on October 22 and the `:free` slug was retired. `minimax/minimax-m3.1-flash` is the base slug. If you copied that URL in a hurry, you have a dead endpoint. My OpenRouter routing workflow covers why I keep a single key across providers instead of one per vendor.

## Three numbers that matter more than the leaderboard

**One: 62.8 on AutomationBench-AA, and why that benchmark is the marketer's benchmark.**

Almost every model benchmark measures either coding or chat quality. AutomationBench-AA measures the thing that actually consumes my team's hours: multi-step business process execution across multiple SaaS tools. MiniMax's own framing of the launch names sales, marketing, operations, customer service, finance, and HR explicitly as the target workflows.

A cheap model that leads the expensive tier on that specific axis changes one thing: the per-task cost of an agent that touches your stack. At $0.15/M in and $0.60/M out, a 30-tool-call workflow that burns 400K input and 40K output tokens costs 8.4 cents. The same workflow on Opus 5.5 pricing ($4/$20) costs $2.40 — nearly 30 times as much. That ratio decides whether you can run a workflow 1,000 times a day or 50.

"Cheapest" is worth qualifying, because the headline per-token price is not the whole bill. The number that is comparable across vendors is Artificial Analysis's cost per Intelligence Index task, and there M3.1 Flash lands at $0.22 — against $0.26 for DeepSeek V4.1 Flash-1017 (which scores higher, 45), $0.28 for GLM-5.3 Flash (which ties at 42), and $1.24 for Gemini 3.8 Flash (which scores lower, 41). So M3.1 Flash is the cheapest of the models scoring 42 or above, by a narrow margin over DeepSeek. Gemini 3.8 Flash is the one to avoid on this axis: roughly five and a half times the cost for a point less.

One caveat, and it's a good one: those 62.8 figures are MiniMax-reported, run through Fireworks. But AutomationBench-AA is one of the ten evaluations inside Artificial Analysis's Intelligence Index v4.3.2, which scored M3.1 Flash 42 overall — up from M3's 29. So the independent number doesn't contradict the vendor claim as hard as it first looks. If M3.1 Flash were secretly weak at business workflows, the aggregate would have dragged the index down.

**Two: 93% — and the specific shape of it.**

AA-Omniscience is designed to separate "knows it" from "guesses it." The measure that matters here isn't raw accuracy — 56% is the *best* accuracy in the flash tier, ahead of DeepSeek V4.1 Flash-1017 at 43% and GLM-5.3 Flash at 28%. The measure is what happens on the misses. MiniMax's model answers 56% right and then, on essentially every remaining question, produces a confident wrong answer instead of abstaining. Result: a 93% hallucination rate and an overall benchmark score of 16.

Compare that shape against DeepSeek V4.1 Flash-1017: lower accuracy (43%), and it knows it doesn't know. Compare against GLM-5.3 Flash: 28% accuracy, and it abstains a lot. MiniMax is the only one of the three that is *both* the most accurate and the most willing to invent.

This is the opposite failure mode from the one I wrote about in the Kimi K3.1 Pro post — where Moonshot published an 8.3% hallucination figure and the independent number came back at 44%, so the vendor was flattering itself by 5x. MiniMax published no hallucination figure at all. If you only read the launch post, you'd have no idea there was a number to worry about.

**Three: the speed claims don't reconcile, and the honest ones are the slowest.**

Three numbers for the same model:

- MiniMax: over 200 tokens/second at peak, median above 170
- Artificial Analysis: 93 tokens/second
- OpenRouter, live on the model page: 69 tokens/second at P50

That's a spread of roughly 3x between the vendor's peak claim and the provider-side median. For an interactive surface the gap matters — a 30-second first-token wait is a different product from a 3-second one. For a batch job none of it matters. Where MiniMax's throughput claim is *hard* to dismiss is on the inference-stack side: SGLang reports over 1,000 tokens/second at batch size 1, and NVIDIA says it ran the model at up to 200K tokens/second per GPU on Vera Rubin. That's the self-hosting path, not the API path.

## Where I'd route it

Five jobs I'd hand M3.1 Flash this month:

1. **Volume classification at the top of the funnel.** Lead scoring, support ticket triage, comment moderation, review sentiment. Ten thousand rows in, JSON out with a label and a one-line reason. The schema*is* the check — an unparseable row is a failure, not a confident error. I already run a version of this on a local model for email triage; the reason it works there and works here is that nobody ever reads the output without reading the input first.
2. **SEO metadata at page count.** Title tags, meta descriptions, H2 structures, schema markup for 500 city pages or a large product catalog. Bounded output per item, mechanically checkable, and the actual quality bar — does it sell — is a human judgment you make on ten of them, not five hundred.
3. **Multi-step SaaS workflow steps.** This is the AutomationBench-AA result translated into practice: lead arrives → CRM field written → follow-up email drafted → task created → summary posted to Slack. The daily digest shape is exactly this, and it's the one that scales without you watching it — see my Reddit monitoring agent for the architecture.
4. **Ad creative variants at volume.** 50 headlines and 50 descriptions per keyword cluster, per channel, per offer. Bounded output, human picks the winners, no single variant carries a factual claim.
5. **The first pass of anything that gets checked later.** A research compile, a brief skeleton, a competitor page scrape, a first-draft ranking sheet. This is the one that changes with the 93%. A first pass that a human or a second model reviews is exactly the job where "confident and often wrong" is cheap.

Three I'd keep elsewhere:

1. **Anything that becomes a claim in a client deliverable.** Statistics, market sizing, competitor pricing, "our rankings show." At 93%, M3.1 Flash will produce a number that reads like a number, and I cannot tell you which ones are invented without checking each one. If the number is going in a deck, it comes from a source, not from a Flash-tier model.
2. **Regulated or claim-sensitive copy.** Financial, health, and legal claims. Same logic as any hallucination-prone model, but the cost of being wrong is asymmetric.
3. **Brand prose.** Nothing in this release measures writing quality. My rule for voice hasn't moved through a year of releases and it isn't moving here: the model that writes the words stays where it is.

## Where a marketer actually meets it

MiniMax Code is the terminal coding agent. Not your surface. **MiniMax Agent** is — the web app and the desktop app for macOS and Windows, which works inside a local folder, drives your logged-in browser session with your permission, and schedules work on a recurring cadence.

The piece that matters for this article: MiniMax Agent has an **Agent Team** mode where a leader agent breaks a job into parallel assignments and routes the results through verifier agents before delivery. The company that shipped a model with a 93% hallucination rate also shipped a product whose main selling point is a verification layer in the loop. Read that as a design hint, not a marketing line — if the model needs a verifier to be safe at volume, then your workflow probably needs one too, and you'd be building the same thing by hand.

It also has Experts and Skills for recurring behavior, MCP connectors, and optional long-term memory. Token Plan tiers run $20 (Plus), $50 (Max), and $120 (Ultra) per month, sharing a quota across models, with annual billing cheaper. There are credit top-ups at $5, $25, and $100 if you blow through a quota, and there's peak-hour throttling on weekdays between 15:00 and 17:30 — schedule your nightly competitor sweep outside that window if you can.

## The license is the part to read twice

Open weights on October 23, no access request, roughly 252GB of files. Under the MiniMax Community License — not MIT, not Apache. What that actually obligates you to do for commercial use:

- Prominently display **"Built with MiniMax M3.1-Flash"** on a related website, UI, blog post, about page, or product documentation. "Powered by AI" does not satisfy it.
- Send a one-time notice email to MiniMax if the products and services in scope generate $20M or less in yearly revenue.
- Obtain **prior written authorization** before commercial use if they generate more than $20M yearly. A request is not an authorization.
- Fine-tunes and modified versions are explicitly inside the commercial-use definition — you don't escape it by training.

For a marketer building a client-facing product on self-hosted weights, that display string is a real constraint: someone else's brand goes on your product. Put it in the contract early.

And on the hardware side — this is not a laptop play. The smallest community quant is 155.9GB, which needs a 192GB machine; the Q4_K_M file is 258GB and wants 384GB. This is multi-GPU server territory, which puts it in the same category as the Llama 3.3 self-host math rather than next to it. The inference stack is maturing fast though: SGLang supports it day one, and Inco AI open-sourced a DFlash 2 drafter under Apache 2.0 claiming up to 3.5x faster, lossless generation.

One note on the wider release context: reports from launch week say MiniMax obtained permission to open-weight the model under new Chinese rules following the October 16 AI export restrictions — the same window in which DeepSeek got approval for V4.1F-1017. Treat that as reported, not confirmed, and treat it as the thing to watch rather than a settled fact.

## Three things to watch

**The hallucination number at lower reasoning effort.** Nobody has published it. Every measurement above is at `max`, which is the default — the most expensive setting. If `low` or `medium` abstains more, the model becomes usable as a first-pass engine on far more work. If it just gets dumber and still never says "I don't know," the routing is narrower than I wrote above. This is the single number that would most change this post.

**M3.1 Pro, promised for later.** The Flash tier is the cheap workhorse; Pro is where MiniMax will have to answer for this release's reputation. Watch whether Pro inherits the same never-abstains behavior. If the bigger model also refuses to say "I don't know," the behavior is architectural, not a small-model artifact, and it will spread.

**The self-hosted cost curve.** If SGLang, the DFlash drafter, and the reported >1K tokens/second decode hold up, then at a couple of thousand requests a day the API price stops being the deciding factor. The day the cheap open model is fast enough on hardware you already rent, "is it worth self-hosting" stops being a data-center question.

## The rule this release hands you

I'd write one line on the card above the router config: **let M3.1 Flash be wrong where the next step in the pipeline can catch it, and never where a human has to.**

That's it. That's the whole routing decision. It doesn't need a benchmark table, it doesn't need the vendor's 3x speed claim, and it doesn't care which reasoning level you're running. The model's strength — leading the business-workflow tier — and its weakness — inventing facts confidently at 93% — describe the same thing from two sides: M3.1 Flash is extremely willing to produce output. It just isn't willing to admit when the output isn't real.

That's a useful shape for anything cheap, and it's why the cheap tier keeps getting more dangerous rather than safer. A model that says "I don't know" costs you a retry. A model that says "the Q3 conversion rate was 3.8%" costs you a retraction, or a client finding out you didn't check.

Both numbers came out of the same six days. One tells you the model can run a marketing workflow end to end for pennies. The other tells you that when it invents a statistic, it invents it in complete sentences, at volume, at a cost that makes checking it feel optional. The first is why you'll deploy M3.1 Flash. The second is why you won't let it near a claim without a check. That combination — cheap enough to run on everything, unreliable enough to gate everything — is the shape of the next year of model routing, and M3.1 Flash is simply the first model where it's this visible.
