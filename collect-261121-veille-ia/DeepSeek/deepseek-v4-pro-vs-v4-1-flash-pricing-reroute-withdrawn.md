---
id: collect-261121-veille-ia/deepseek/deepseek-v4-pro-vs-v4-1-flash-pricing-reroute-withdrawn
title: "DeepSeek V4 Pro vs V4.1-Flash Pricing: Reroute Withdrawn"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: ["2026-09-10"]
keywords: ["deepseek", "flash", "model", "pricing", "page", "tokens", "reroute", "input"]
source: https://www.aiagencycalculator.com/deepseek-v4-1-flash-pricing
source_anchor: ""
source_lines: [1, 190]
section: "DeepSeek"
---
**The short answer.** DeepSeek V4.1-Flash is live on the API as `deepseek-flash`, and the two legacy Flash names - `deepseek-v4-flash` and `deepseek-v4-flash-vision-exp` - are still accepted: DeepSeek serves those requests with the V4.1-Flash model and bills them at the Flash price. The previously announced reroute of `deepseek-v4-pro` to V4.1-Flash, which was due to begin at 04:00 UTC, Sept 14, 2026, was withdrawn on 2026-09-11, so V4 Pro keeps serving at unchanged billing. What survives the retraction is the spread inside the Flash row itself - $0.003 per 1M cached input off-peak against $0.30 per 1M cache-miss input at peak, a 100x per-token rate gap.

## V4 Pro stays: what we re-verified on 14 September 2026

This is the current status, re-checked against DeepSeek's own API documentation at **13:38 UTC on 14 September 2026** - not carried over from announcement week. The 14 September reroute did not happen, and the pricing page, the docs home page and the changelog all carry the same sentence:

"In response to user demand, we have decided to continue providing API services for DeepSeek V4 Pro after September 14, 2026, with the billing method remaining unchanged. We will provide further notice should there be any changes. Thank you for your understanding and support!"

That text is footnote (2) on Models and Pricing (23,359 bytes fetched 2026-09-14 13:38 UTC) and the same wording sits in the changelog (50,934 bytes fetched the same minute) under the entry dated **2026-09-10**. Four things follow from the citation itself:

- **The retraction is an edit, not an announcement.** There is no dated DeepSeek post for it. The changelog's newest heading still reads 10 September, because the reversal was written into that existing entry - which is why so few trackers noticed it.
- **The wording is narrower than "price freeze".** DeepSeek commits to the billing*method* staying unchanged and says it will give further notice of any change. No future rate is promised, and the pricing page reserves the right to adjust rates.
- **V4 Pro keeps its own row and its own model version** -`deepseek-v4-pro` , model version`DeepSeek-V4-Pro-0813` , off-peak $0.66 input / $1.98 output / $0.022 cached input per 1M tokens.
- **Two official pages have still not been corrected.** See the contradiction section below - it is the part most migration guides currently ranking for these queries still get wrong.

## What shipped: DeepSeek-V4.1-Flash as `deepseek-flash`

DeepSeek shipped DeepSeek-V4.1-Flash on **2026-09-10**. The launch note says to set your model to `deepseek-flash`, and the API docs repeat that as the model name.

- **Architecture:** 552B-parameter MoE in a new Causal Encoder-Decoder design - just**8B active parameters for input and 16B for output** .
- **Native vision** in the base model rather than a bolt-on variant: the Hugging Face card states the model "natively processes images and text".
- **Context: 1M tokens.** The API docs list a context length of 1M and the model card states support for up to one million tokens. Note the attribution: DeepSeek's launch news page does*not* state the 1M context - the docs and the model card do. OpenRouter lists 1,048,576 tokens with up to 384,000 completion tokens.
- **License: MIT.** The model card carries "License: mit" and states that the repository and weights are MIT-licensed. The news page never says MIT, so cite the model card for this, not the launch note.

One launch claim we do not repeat as a finding: the news page says tests put V4.1-Flash ahead of V4 Pro on performance, cost, speed and total runtime, but the parties who ran them are not named anywhere, so the claim cannot be checked.

## The DeepSeek V4.1-Flash price sheet (USD per 1M tokens)

First-party DeepSeek rates and third-party marketplace rates are separated below, because they are not the same product: `$0.30/$1.20` is DeepSeek's own *peak* pair, and it is also what competing hosts charge - it is not the model's headline price.

| Model / endpoint | Input /1M | Output /1M | Cached input /1M | Notes | 
|---|---|---|---|---|
| `deepseek-flash` - DeepSeek first-party,**off-peak** | $0.15 (cache miss) | $0.60 | **$0.003** | Official sheet. Off-peak is every hour outside the peak windows below. | 
| `deepseek-flash` - DeepSeek first-party,**peak** | $0.30 | $1.20 | $0.006 | Peak = Mon-Fri 01:00-04:00 and 06:00-10:00 UTC; exactly 2x off-peak. | 
| `deepseek-v4-pro` - DeepSeek first-party, off-peak | $0.66 | $1.98 | $0.022 | Still served after 2026-09-14; model version DeepSeek-V4-Pro-0813. | 
| `deepseek-v4-pro` - DeepSeek first-party, peak | $1.32 | $3.96 | $0.044 | Unchanged rates. | 
| `deepseek/deepseek-v4.1-flash` - OpenRouter headline | $0.15 | $0.60 | $0.003 | OpenRouter's own "In / Out Price"; equals DeepSeek off-peak. Not a max-effort variant price. | 
| Via SiliconFlow, Modal, Wafer, GMICloud, io.net, NovitaAI, Morph (third-party hosts) | $0.30 | $1.20 | $0.006-$0.03 | Third-party hosts on OpenRouter; cache read varies by host. | 
| Via DeepInfra | $0.20 | $0.60 | $0.006 | OpenRouter provider row, context 1,048,576. | 
| Via Fireworks | $0.22 | $0.66 | $0.007 | OpenRouter provider row. | 
| Via Venice | $0.375 | $1.50 | $0.0075 | OpenRouter provider row. | 
| Artificial Analysis listing - "V4.1 Flash (Reasoning, Max Effort)" | $0.30 | $1.20 | cache discount 98% | Artificial Analysis states these are based on DeepSeek's API - i.e. DeepSeek *peak* rates, not an OpenRouter price. | 

**The 100x, stated precisely.** The headline multiple is a per-token rate comparison, not a reduction in your total bill, and it depends on which two rates you put side by side: **$0.003 per 1M cache-hit input off-peak vs $0.30 per 1M cache-miss input at peak = 100x**. Compare the cached rate with off-peak cache-miss input ($0.15 per 1M) instead and the same pair is **50x**. A mixed workload that misses the cache most of the time lands far below either number - the cache-hit ratio, not the list price, decides what you pay.

**Where $0.30/$1.20 actually comes from.** It is DeepSeek's own peak cache-miss input and peak output pair, and it is the pair Artificial Analysis lists for "V4.1 Flash (Reasoning, Max Effort)". On OpenRouter it is what third-party hosts charge; OpenRouter's own headline for the model is $0.15 in / $0.60 out with $0.003 cache reads.

Predecessor Flash rates, for anyone still carrying them in a spreadsheet: the retired Flash rows were off-peak $0.007 cache-hit / $0.22 cache-miss / $0.66 output, now $0.003 / $0.15 / $0.60. Those old rows are no longer on the live price sheet, so this comparison comes from two independent third-party pricing trackers, not from DeepSeek.

Cost-per-task reference, Artificial Analysis: $0.27 per Intelligence Index task and $476.89 to run the full Intelligence Index evaluation on this model.

## The Sept 14 reroute that was withdrawn after a day

This is the part most coverage still gets wrong, because DeepSeek's own two sets of pages still disagree. The announced reroute of `deepseek-v4-pro` is **not in force**.

- **Announced 2026-09-10 (07:46 UTC, archived changelog):** "After 12:00 Beijing Time on September 14, 2026, and until the future release of V4.1 Pro, all requests to`deepseek-v4-pro` will be routed to V4.1 Flash and billed at the V4.1 Flash price." The same wording sat as a footnote on the pricing page. That is the only sense in which 04:00 UTC, Sept 14, 2026 is a real timestamp here - it is when a plan that was later withdrawn would have started.
- **Withdrawn 2026-09-11, between 07:47 and 16:55 UTC.** The 07:47 UTC snapshot of the pricing page still carries the routing footnote; the 16:55 UTC snapshot carries the retraction instead. The exact minute is not public.
- **What the API docs say now** (pricing page, docs home page and changelog - all three): "In response to user demand, we have decided to continue providing API services for DeepSeek V4 Pro after September 14, 2026, with the billing method remaining unchanged. We will provide further notice should there be any changes."
- **What that leaves in place:**`deepseek-v4-pro` keeps its own pricing row and its own model version, DeepSeek-V4-Pro-0813, at unchanged rates. It is not retired, and there is no Sept 14 migration to plan for.

**Why almost nobody noticed.** The reversal was inserted into the *existing* 2026-09-10 changelog entry instead of being published as a new dated entry, so the changelog's newest heading still reads Sept 10 and a reader checking "what is new since the launch" sees nothing new. The withdrawal also came with no notice period, and DeepSeek publishes no versioned deprecation policy - the announcement gave four days, the reversal gave none. Independent trackers caught it: a pricing tracker published a before-and-after of the footnote, and a same-week analysis piece reached the same conclusion from the two first-party pages.

**The operational lesson:** on this platform, a dated first-party announcement is not a commitment until the API docs carry it, and even then it can be edited in place without a new changelog entry. Pin nothing to a future date you have not re-checked that week.

## Two official pages still say V4 Pro is being phased out

DeepSeek corrected the surfaces that govern billing and left the ones that carry the announcement untouched. Both were live and uncorrected when we fetched them on **14 September 2026 at 13:38 UTC**:

- **api-docs `/news/news260910`** - 27,838 bytes, HTTP 200 - still reads: "Tests by multiple parties put V4.1-Flash ahead of V4-Pro on performance, cost, speed & total runtime.**We're phasing out V4-Pro.** " and "Starting at 04:00 UTC on Sept 14, 2026, all`deepseek-v4-pro` requests will route to V4.1-Flash at V4.1-Flash rates."
- **deepseek.com `/en/news/deepseek-v4-1-flash/`** - 92,061 bytes, HTTP 200 - carries the phase-out wording twice and never mentions the retraction.

Neither page contains the phrase "continue providing API services" - the sentence that now describes what the API bills. The pricing page and the changelog are the only first-party surfaces that carry it, so a reader who lands on the launch post or the corporate news page, both of which use the retirement framing, is reading a plan that was withdrawn three days before it was due to start.

**What that means commercially.** Any migration guide or agency cost model written from those two pages assumes `deepseek-v4-pro` traffic moves to V4.1-Flash rates from 14 September 2026. That assumption is void, and it is not a rounding difference: the Pro row is 3.30x to 7.33x the Flash row per bucket. The third-party guides ranking for "DeepSeek V4 Pro deprecation date 2026" were still describing the reroute as live when we checked on 14 September - two of them were last updated before the retraction was published.

**The rule to take from it:** on this platform the announcement pages behave like marketing and the API documentation behaves like the contract. Where the two disagree, the documentation is what your invoice follows - and it can be edited in place, without a new changelog entry, so re-verify it in the same week you quote.

## How to update your AI cost model for the V4 Pro extension

If your model contains the line "V4 Pro traffic billed at V4.1-Flash rates from 04:00 UTC, 14 September 2026", delete it. What replaced it is a premium on the Pro row, and the size of the correction depends on your cache-hit mix:

| Bucket (per 1M tokens, off-peak) | V4.1-Flash | V4 Pro | Pro premium | 
|---|---|---|---|
| Input, cache hit | $0.003 | **$0.022** | **7.33x** | 
| Input, cache miss | $0.15 | **$0.66** | **4.40x** | 
| Output | $0.60 | **$1.98** | **3.30x** | 
| Peak band, all three buckets | 2x the off-peak rate | 2x the off-peak rate | unchanged by band | 

**Worked example - 100M uncached input plus 20M output tokens, off-peak:** V4.1-Flash $27.00 against V4 Pro **$105.60** - **+$78.60, or +291%**. At one such run a month that is **$943.20 a year** on the Pro line. The same workload inside the peak band is $54.00 against $211.20 (+$157.20, $1,886.40 a year). A cache-heavy agent profile - 900M cached input, 100M uncached input, 30M output - lands at $35.70 on Flash against **$145.20** on Pro, **+307%**. Those two numbers are the cost consequence of the withdrawal stated plainly: a budget built on the reroute understates the Pro line by 291% off-peak, and by twice that at peak.

Then work the five levers, in this order:

1. **Keep Pro only where the premium buys something you can name.** On Artificial Analysis's composite index, V4 Pro 0813 sits at 36 against V4.1-Flash's 40, so the 3.30x output premium has to be justified by task-level behaviour you have measured on your own prompts rather than by model tier. Re-run your eval before and after any switch.
2. **Price the line by traffic mix, not by headline rate.** The 7.33x multiple applies only to the share of input that hits the cache; the 4.40x applies to the rest. The same pair of rates produces two very different monthly numbers at a 40% hit rate and at a 90% one.
3. **Schedule batch work off-peak.** Off-peak is every hour outside Monday-Friday 01:00-04:00 and 06:00-10:00 UTC, and it halves every bucket on both models. It is the only lever here that is free.
4. **Log the model version that answered.** Aliases such as`deepseek-v4-flash` are still accepted and served by V4.1-Flash at the Flash price, and the response identifies`DeepSeek-V4-Pro-0813` or`DeepSeek-V4.1-Flash` . A rate card reconciled against a model ID that can change underneath it is not a rate card.
5. **Re-verify the sheet before you re-quote, and write the position into the contract.** DeepSeek reserves the right to adjust rates, the footnote promises further notice, and last time the notice arrived as an inline edit to an existing changelog entry. Date-stamp every figure you send a client. Our contract notes cover how to write model and rate changes in.

To run your own numbers: the AI Agency Pricing Calculator carries the DeepSeek rows side by side - V4.1-Flash, the aliased legacy `deepseek-v4-flash` row, and the cache-aware V4 Pro row - and the AI Agent API Cost Calculator lets you put your own input, output and cached split against per-1M rates. Both are modelling aids, not quotes.

## Migration checklist

Six checks, in order. The first four are the ones that cost money if you skip them.

-  **Confirm your model ID.** If you are still sending`deepseek-v4-flash` or`deepseek-v4-flash-vision-exp` , you are already being served by V4.1-Flash and billed at the Flash price; switching to`deepseek-flash` only makes that explicit. No API error is raised either way, and no version pin is available for the legacy names.
-  **Re-verify output quality and evals.** V4.1-Flash is a different model from the Flash you last benchmarked, and because the legacy IDs cannot be pinned to a version, your eval suite is the only pin you have. Re-run it before you widen traffic.
-  **Re-check cost projections against the new sheet.** Off-peak $0.15 in / $0.60 out with $0.003 cached input; peak doubles both. Any model built on the old $0.22/$0.66 Flash pair now overstates your input cost.
-  **Check your cache-hit assumptions.** The 100x spread only pays out on traffic that actually hits the cache. Measure your real cache-hit ratio at the prompt shapes you send; do not carry the ratio over from another model.
-  **Update internal docs and client-facing rate cards** that still quote the pre-V4.1 Flash rates or the retired Flash model names.
-  **Review any client contract that pins a model ID or a rate** - and specifically any quote priced on the assumption that`deepseek-v4-pro` traffic would move to Flash rates on Sept 14. That change is off, so Flash rates would misprice the work; V4 Pro bills at $0.66/$1.98 off-peak with $0.022 cached input.

## Benchmarks: what was measured, and by whom

Every figure below names the party that produced it. DeepSeek's own table was run in DeepSeek's harness at maximum reasoning effort - those numbers are vendor-reported, not independent, and they are labelled that way throughout.

| Measure | V4.1-Flash | Comparators | Measured by | 
|---|---|---|---|
| Intelligence Index (composite) | **40** (Reasoning, Max Effort; #6 of 113 in its class) | GPT-5.6 Sol (max) 47; Claude Opus 5 (adaptive reasoning, max effort) 51; DeepSeek V4 Pro 0813 36 | Artificial Analysis | 
| Output speed | 198.6 output tokens/sec (#5 of 113) | - | Artificial Analysis | 
| Cost per Intelligence Index task | $0.27 (#19 of 113) | - | Artificial Analysis | 
| Intelligence Index, same-day third-party test | 40 - ahead of V4 Pro 0813 (36), behind Kimi K3 (44) and GLM-5.3 (45) | Kimi K3 44; GLM-5.3 45 | KuCoin news report (third party) | 
| DeepSWE v1.1 | 74.2 (DeepSeek-run) | Claude Opus 5 74.0; GPT-5.6 Sol 73.0 (both as reported in DeepSeek's table) | VentureBeat, reporting DeepSeek-run evaluations | 
| Terminal-Bench 3.0 | 30.0 | Claude Opus 5 43.3 | VentureBeat, reporting DeepSeek's table | 
| Terminal-Bench 4.0 | 31.2 | Claude Opus 5 51.8 | VentureBeat, reporting DeepSeek's table | 
| GPQA Diamond / SEC-Bench Pro | GPT-5.6 Sol leads V4.1-Flash in DeepSeek's own table | GPT-5.6 Sol | VentureBeat, reporting DeepSeek's table | 

**What Artificial Analysis supports, and what it does not.** It does support a narrower claim: V4.1-Flash is fast and cheap, and it beats its own predecessor and V4 Pro (40 vs 36). It does *not* support the idea that V4.1-Flash tops GPT-5.6 Sol or Claude Opus 5 - on the composite it sits below both, 40 against 47 and 51. VentureBeat's own body makes the same point: it lists benchmarks where Opus 5 and Sol lead, calls the results "not uniformly dominant", and frames the evidence as a price-performance thesis rather than a clean intelligence lead. Only the headline goes further than the numbers do.

**Vendor-reported table (DeepSeek's own harness, max reasoning effort, temperature 1.0, top_p 0.95):** GPQA Diamond 90.9; HLE 36.8 (39.1 text-only); Codeforces 3471; MathArena Apex 65.6; Terminal-Bench 2.1 90.6; Terminal-Bench 3.0 30.0; Terminal-Bench 4.0 31.2; DeepSWE v1.1 74.2; ProgramBench 20.3; NL2Repo-Bench 65.4; CyberGym 88.1; SEC-Bench Pro 62.8; ExploitGym 15.3; HLE with tools 63.9; Automation-Bench 54.8; Agents' Last Exam 31.8; Chartography with tools 78.9; BabyVision with tools 89.6; ZeroBench-main with tools 49.0. All of these are DeepSeek's numbers about DeepSeek's model.

Effort caveat on any quoted score: DeepSeek ran its comparison table at its maximum effort setting of 100. Raising effort from 25 to 100 lifts DeepSWE v1.1 from 66.0% to 74.2% and Terminal-Bench 2.1 from 82.4% to 90.6%, but consumes roughly 2.5 times as many output tokens. The public API presets are low / high / max = 50 / 75 / 100 (VentureBeat, quoting DeepSeek). Benchmark a score and a token bill together, or you are comparing two different products.

## Frequently asked questions

### DeepSeek V4.1 Flash pricing

DeepSeek's own API prices deepseek-flash at $0.15 per 1M input tokens and $0.60 per 1M output tokens off-peak, with cache hits at $0.003 per 1M. Peak rates are exactly double - $0.30, $1.20 and $0.006 - during Monday to Friday 01:00-04:00 and 06:00-10:00 UTC; every other hour is off-peak. deepseek-v4-pro keeps its own live row at $0.66 input, $1.98 output and $0.022 cached input off-peak (peak $1.32, $3.96, $0.044). On OpenRouter the headline for deepseek/deepseek-v4.1-flash is the same off-peak pair as DeepSeek's - $0.15 in, $0.60 out, $0.003 cache read - while third-party hosts on OpenRouter list $0.30 in and $1.20 out.

### Is deepseek-v4-pro retired?

No. DeepSeek announced on 2026-09-10 that deepseek-v4-pro requests would be routed to V4.1-Flash from 04:00 UTC on Sept 14, 2026, then withdrew that plan on 2026-09-11. The current wording on DeepSeek's own API docs, pricing page and changelog is that DeepSeek will continue providing API services for DeepSeek V4 Pro after September 14, 2026 with the billing method remaining unchanged. deepseek-v4-pro keeps its own pricing row (model version DeepSeek-V4-Pro-0813) at $0.66 input, $1.98 output and $0.022 cached input off-peak. There is no Sept 14 migration and no retirement date in force.

### DeepSeek V4.1 Flash vs GPT-5.6 Sol

On Artificial Analysis's Intelligence Index, V4.1-Flash at Reasoning, Max Effort scores 40 while GPT-5.6 Sol at max effort scores 47 (Claude Opus 5 scores 51) - so the composite index favors the rivals, not V4.1-Flash. VentureBeat's own body reported the same picture, listing benchmarks where Claude Opus 5 leads V4.1-Flash 43.3 to 30.0 on Terminal-Bench 3.0 and 51.8 to 31.2 on Terminal-Bench 4.0, and where GPT-5.6 Sol leads on GPQA Diamond and SEC-Bench Pro in DeepSeek's own table; its headline says further than its body does. Where V4.1-Flash leads is price: $0.003 per 1M cached input off-peak against the $0.40 per 1M cache-read rate press reporting attributes to GPT-5.6 Sol, and $0.15/$0.60 against Sol's $4/$20 headline. Artificial Analysis also measures V4.1-Flash as fast - 198.6 output tokens per second, 5th of 113 models - and cheap per task, at $0.27 per Intelligence Index task.

### cheapest 1M context model 2026

Scope first: this compares the 1M-context models whose published rates we were able to verify on 2026-09-11, at list price, and it is not a global cheapest-model claim. DeepSeek V4.1-Flash has a 1M-token context and the lowest cached-input rate in that set - $0.003 per 1M off-peak, $0.006 at peak - against press-reported cache-read rates of $0.40 per 1M for GPT-5.6 Sol, $0.50 for Claude Opus 5 and $0.30 for Kimi K3. Headline pairs in the same set: V4.1-Flash $0.15/$0.60 off-peak ($0.30/$1.20 at peak), GPT-5.6 Sol $4/$20, Claude Opus 5 $5/$25, Kimi K3 $3/$15. Hosted variants can price differently - third-party hosts on OpenRouter list V4.1-Flash at $0.30/$1.20 - so verify the endpoint you actually call.

### DeepSeek Flash off-peak cached pricing

A cached input token on DeepSeek's own API costs $0.003 per 1M off-peak and $0.006 per 1M at peak - off-peak is exactly half of peak. Off-peak means every hour outside Monday to Friday 01:00-04:00 and 06:00-10:00 UTC. That $0.003 rate is the low end of the 100x per-token spread against the $0.30 per 1M peak cache-miss input rate; the same cached token costs more through third-party hosts on OpenRouter, whose cache-read rates vary by host and sit above DeepSeek's own. The spread is a per-token rate comparison, not a discount on your whole bill - it only pays out on the share of your traffic that actually hits the cache.

### When was the DeepSeek V4 Pro deprecation reversed?

The plan was withdrawn on 11 September 2026, the day after DeepSeek published it, by editing the changelog entry dated 2026-09-10 and the pricing-page footnote in place. There is no separate dated announcement for the reversal, which is why so little coverage caught it: the changelog's newest heading still reads 10 September. Archive snapshots of the pricing page bracket the edit to 11 September 2026 and press reporting picked it up the same day. The statement now in force is that DeepSeek will continue providing API services for DeepSeek V4 Pro after September 14, 2026 with the billing method remaining unchanged.

### Is DeepSeek V4 Pro still available after September 14, 2026?

Yes. As of 2026-09-14 the API documentation still reads: In response to user demand, we have decided to continue providing API services for DeepSeek V4 Pro after September 14, 2026, with the billing method remaining unchanged. We will provide further notice should there be any changes. deepseek-v4-pro keeps its own pricing row (model version DeepSeek-V4-Pro-0813) at $0.66 input, $1.98 output and $0.022 cached input per 1M off-peak, with peak rates exactly double. No retirement date is in force and there is no forced migration. Note that two of DeepSeek's own announcement pages still say V4 Pro is being phased out; the pricing page and changelog, which govern what the API bills, do not.

### How much more does V4 Pro cost than V4.1-Flash?

Per 1M tokens off-peak, V4 Pro costs 7.33x V4.1-Flash on cached input ($0.022 against $0.003), 4.40x on uncached input ($0.66 against $0.15) and 3.30x on output ($1.98 against $0.60); the peak band is exactly double on both models, so the ratios hold in either band. On a run of 100M uncached input plus 20M output tokens that is $105.60 against $27.00 - +$78.60, or +291% - which annualises to $943.20 if you run it monthly off-peak ($1,886.40 at peak). A budget built on the withdrawn reroute, which would have billed Pro traffic at Flash rates from 14 September 2026, understates the Pro line by 291%.

**Put the Flash row next to the models you already run**

The calculator carries the DeepSeek rows - V4.1-Flash through the legacy `deepseek-v4-flash` name, plus V4 Pro - alongside the other models you mix in production.

## Sources

- DeepSeek API docs - Models and Pricing (model name `deepseek-flash` , per-1M rates, peak windows, retraction wording): api-docs.deepseek.com/quick_start/pricing/ (23,359 bytes, fetched 2026-09-14 13:38 UTC)
- DeepSeek API docs - Change Log (the 2026-09-10 entry that later carried the withdrawal): api-docs.deepseek.com/updates/ (50,934 bytes, fetched 2026-09-14 13:38 UTC)
- DeepSeek API docs home (model name and retraction wording): api-docs.deepseek.com
- DeepSeek news (2026-09-10 launch; still advertising the phase-out, uncorrected as of 2026-09-14 13:38 UTC, 92,061 bytes): deepseek.com/en/news/deepseek-v4-1-flash/
- DeepSeek API docs news post (2026-09-10 launch; still reads "We're phasing out V4-Pro", 27,838 bytes, fetched 2026-09-14 13:38 UTC): api-docs.deepseek.com/news/news260910
- PANews (2026-09-11T11:41Z) - the first public report of the reversal, crediting Star Market Daily: panews.io
- Hugging Face model card - DeepSeek-V4.1-Flash (MIT license, 1M-token context, architecture, native image and text input): huggingface.co/deepseek-ai/DeepSeek-V4.1-Flash
- OpenRouter - model page and provider endpoints API (headline price, cache read, per-host rates, context length): openrouter.ai/deepseek/deepseek-v4.1-flash and /api/v1/models/deepseek/deepseek-v4.1-flash/endpoints
- Artificial Analysis - V4.1 Flash (Reasoning, Max Effort), GPT-5.6 Sol (max), Claude Opus 5, DeepSeek V4 Pro 0813 (Intelligence Index, speed, cost per task): artificialanalysis.ai/models/deepseek-v4-1-flash
- VentureBeat (Carl Franzen, Sept 10, 2026) - launch coverage, DeepSeek-run comparison table, effort caveat and the in-body qualifications: venturebeat.com
- Wayback Machine snapshots used for the retraction window - DeepSeek pricing page 2026-09-10 22:29 UTC: web.archive.org 2026-09-10; 2026-09-11 07:47 UTC (footnote still live): web.archive.org 2026-09-11 07:47; 2026-09-11 16:55 UTC (retraction live): web.archive.org 2026-09-11 16:55; changelog 2026-09-10 07:46 UTC: web.archive.org changelog
- UsagePricing tracker - withdrawal of the V4 Pro reroute, with a before-and-after of the footnote (independent corroboration): usagepricing.com
- Cherry Creek News - "the retirement that lasted a day" (analysis; corroborates the two-page contradiction): thecherrycreeknews.com
- KuCoin news - same-day third-party test placing V4.1-Flash at 40 on the Intelligence Index: kucoin.com
- Competitor cache-read and headline rates quoted above (GPT-5.6 Sol $4/$20/$0.40, Claude Opus 5 $5/$25/$0.50, Kimi K3 $3/$15/$0.30) are press-reported, not read off the vendors' own sheets: VentureBeat, with the Sol and Opus 5 headline pairs independently matching Artificial Analysis listings.
- Secondary pricing trackers used only where DeepSeek no longer publishes the old rows (predecessor Flash rates): usagepricing.com and asapai.co.kr (both secondary; treat as medium confidence).

Accuracy note: every figure on this page traces to the research fact sheet compiled on 2026-09-11 (sources fetched 2026-09-11 between 22:00 and 22:12 UTC; fact sheet sha256 `9f3fbbe21866d9f9ab40c433fc50bb246e94f0c6ce06a2541944111e32b48703`). No hands-on model testing was performed for this page: benchmark numbers are reproduced from the party that measured them and are attributed in the tables above, and DeepSeek's own harness results are labelled vendor-reported. One claim carried by other coverage is deliberately absent here: the assertion that V4.1-Flash beats GPT-5.6 Sol and Claude Opus 5 - Artificial Analysis measures the opposite order on its composite index, and a separate reported AutomationBench figure could not be verified on Artificial Analysis's own page, so no part of it is repeated. Rates move frequently - re-verify before quoting, and note that DeepSeek has edited first-party pricing text in place within a single day. Re-verified 2026-09-14 13:38 UTC (card t_acc02f0a): the pricing page (23,359 bytes) and the changelog (50,934 bytes) still carry the retraction wording, and the two pages named in the contradiction section (27,838 bytes and 92,061 bytes) still carry the phase-out wording. No price on this page changed in that re-check.
