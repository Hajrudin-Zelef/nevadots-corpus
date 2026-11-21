---
id: collect-261121-veille-ia/minimax/minimax-m3-1-flash-review-benchmarks-price-who-it-s-for
title: "MiniMax M3.1 Flash Review: Benchmarks, Price, Who It's For"
domain: minimax
role: reference
task: article
actors: ["MiniMax"]
dates: []
keywords: ["minimax", "best", "flash", "price", "setting", "model", "free", "route"]
source: https://springprompt.com/blog/minimax-m3-1-flash-review
source_anchor: ""
source_lines: [1, 142]
section: "MiniMax"
---
MiniMax released M3.1 Flash on 19 October and said it matches frontier models on agent, coding and visual tasks at a third of the cost of MiniMax M3. It also said the anonymous "Space Bunny" model that has been busy on OpenRouter was an earlier version of it.

We ran six of our benchmarks on it the same evening. It writes some of the most reliable product listings of any cheap model, and it adds far fewer made-up claims than DeepSeek's new Flash. But on long jobs it often thinks until it runs out of room and returns nothing.

## MiniMax M3.1 Flash at a glance

- **Use it for** product data and short copy at volume, where it was 9th of 24 on our listing test and first among models at its price.
- **Don't use it for** long analysis or report writing without a fallback: half our survey tasks and a quarter of our slide decks came back empty.
- **Price:** $0.15/$0.60 per million input/output tokens, flat. Weights promised for 23 October.
- **Speed:** Artificial Analysis measures 175 tokens a second on MiniMax's API. We could only reach it through a free, rate-limited route, so our own timings don't count.

**What we ran.** On launch day OpenRouter only offered `minimax/minimax-m3.1-flash:free`, served by MiniMax, so every result here comes from that free route at the default thinking setting, on 19 October 2026. Free routes are rate-limited, so we don't report its speed, and we will rerun everything once the paid route is up. The results are listed under their own "free route" configuration so the rerun stays separate. Results are measured by Spring Prompt; costs are list-price estimates. Not yet in: Arena's text ratings and the agent and hallucination leaderboards.

## What MiniMax says

MiniMax's announcement claims 62% on Automation-Bench, 30% on Agents' Last Exam, 74% on DeepSWE and 91% on BabyVision, "at 1/3 the cost of M3 and 3x the speed". The flat price is $0.15 per million input tokens and $0.60 per million output, with cached input at $0.015, and the weights are due on 23 October. Those are the developer's claims.

## MiniMax M3.1 Flash benchmarks, measured by Spring Prompt

The model page has every result as it lands. How to read this table: each row is one benchmark, with the model's score, its rank among the models we have tested on it, and the best model in each group.

| Benchmarkwhat it measures | MiniMax M3.1 Flashits score, free route | Rank among all modelsavailable today | Rank at its price$0.25–1 per million tokens, blended | Rank at its intelligencewithin 4 points on the AA index | 
|---|---|---|---|---|
| CatalogBenchshare of product listings reliably ready to publish | 50.0%default setting | 9th of 24Best: GPT-6 Astra, 75.0% | 1st of 4Leads this group | 2nd of 9Best: Grok 4.7, 57.1% | 
| CatalogBench sales briefthe same, writing to sell | 37.5%default setting | 6th of 24Best: GPT-6 Astra, 69.6% | 1st of 4Leads this group | 1st of 9Leads this group | 
| ROASBenchscore in a 12-month ad budget simulation | 40.3default setting | 12th of 23Best: GPT-6 Astra, 56.6 | 2nd of 3Best: DeepSeek-V4.1-Flash-1017, 41.4 | 4th of 9Best: Gemini 3.8 Flash, 51.4 | 
| DeckBenchhead-to-head rating of slide decks | 1,094default setting | 14th of 23Best: GPT-6 Astra, 1,572 | 2nd of 3Best: DeepSeek-V4.1-Flash-1017, 1,138 | 6th of 8Best: Grok 4.7, 1,312 | 
| SurveyBenchhead-to-head rating of survey plans and analyses | 434default setting | 12th of 12 (last)Best: Muse Wonder, 1,507 | 3rd of 3 (last)Best: DeepSeek-V4.1-Flash-1017, 1,224 | 4th of 4 (last)Best: Claude Haiku 5.5, 1,282 | 

Ranked on the release we publish with this post. A group only counts models that benchmark has measured. GPT-6 Luna and Claude Haiku 5.5 ($0.20 blended) fall just below the price group; see the alternatives. BulletBench is not in the table: MiniMax lost its first four games on time at every clock and setting, on a rate-limited route.

### Product data: careful, and cheap

CatalogBench gives a model 56 invented products, with source data and images, and asks for a listing that could go live with no human fix, three times over. With a factual brief, MiniMax passed 63.1% of listings and was right in all three repeats for 50.0% of products, 9th of 24 and just behind GPT-6 Luna (55.4%). It added about 22 unsupported claims per run, against 95 for DeepSeek-V4.1-Flash-1017 and 30 for the September V4.1 Flash.

The sales brief is where it stands out. With a brief to sell, most models start inventing benefits; MiniMax added 34 unsupported claims per run and was reliably right on 37.5% of products, 6th of 24. Only the GPT-6 models and Muse Wonder did better. One product per run came back empty.

### Long jobs: it runs out of room

On SurveyBench, which asks a model to plan a survey and then analyse supplied responses, MiniMax used its whole 32,000-token output budget thinking on 18 of 24 steps and returned nothing. Every model gets the same budget, and DeepSeek-V4.1-Flash-1017 finished every step within it. Half the tasks got no answer at all, so its rating is last of 12. On DeckBench two of its eight decks never arrived and none of the other six was ready to present; head to head it still rated 1,094, 14th of 23, close behind DeepSeek's new Flash.

Its first ROASBench run stopped in month 3 for the same reason; we set it aside and the one rerun scored 40.3, 12th of 23, level with DeepSeek-V4.1-Flash-1017. ROASBench is one simulated year per model, not evidence of real advertising returns.

### In the playground

All 28 of our playground prompts ran on the free route too. These are one-off tests, not a benchmark. Its spinning hexagon kept the ball inside even at the top spin setting, which DeepSeek's new Flash failed, and its chess game gets every rule right down to stalemate. Three answers never drew anything: its cassette recorder sits on "Warming up the transport" because its code uses a material it never defines, and two of the craft scenes crash on load.

## Third-party results

- **Artificial Analysis** (our fetch, 19 October): 42.4 on the Intelligence Index, against 45.1 for DeepSeek-V4.1-Flash-1017 and 43.4 for Claude Haiku 5.5, at 175 tokens a second on MiniMax's API. Its model page puts the cost of running the index at $0.22 a task, against DeepSeek's $0.26, and calls the model very verbose. Data sourced from Artificial Analysis.
- **Arena** (reported 19 October): 12th among open models on Code Arena's WebDev board, 32nd overall with 1,588.

Pending: Arena's text and factuality ratings, tau2-bench and Vectara's hallucination leaderboard. Until Arena rates it, its overall score on our leaderboard is provisional.

## What people are saying

**How the launch read on X: 61% positive, 12% mixed, 27% negative** of the 41 posts that took a side

On 19 October, under four hours after the launch, we read the first 150 posts X served under "Top" for the model's name and labelled each one positive, mixed, negative, neutral or off-topic. Posts from MiniMax and its partners are left out, as are neutral and off-topic items. Labels are by Claude Opus 5.5 against a written rubric, in one pass. Four of the negative posts are from one account, and the Hacker News thread had no discussion yet. X's Top tab favours the loudest posts. Every labelled item.

A selection, with how we read each one:

Very impressive benchmarks, especially considering that DeepSeek 4.1 Flash is only a few days old - and yet Minimax's new Flash model outperforms it in several key benchmarks!

really surprised by how well minimax did. old m3 couldn't make anything like this

Opus still wins on realism. But MiniMax got most of the way there for less than a coffee...

This doesn't look too impressive based off of AA's benchmarks, this is a bit of a letdown

So far MiniMax-M3.1-Flash is not wowing me on the game development front.

Yes, Space Bunny - which has topped global leaderboards with 200T+ tokens - was an earlier version of M3.1 Flash.

## MiniMax M3.1 Flash vs DeepSeek-V4.1-Flash-1017, GPT-6 Luna and Claude Haiku 5.5

These are the cheap models people will weigh it against, including DeepSeek's new Flash from two days earlier. Prices are blended per million tokens (three input to one output) at list price.

| Modelwhy it's here | Priceblended | CatalogBenchreliably publishable | Sales briefreliably publishable | DeckBenchrating | ROASBenchscore | Compare | 
|---|---|---|---|---|---|---|
| MiniMax M3.1 Flashfree route | $0.26 | 50.0% | 37.5% | 1,094 | 40.3 | – | 
| DeepSeek-V4.1-Flash-1017released two days earlier | $0.52 | 21.4% | 0.0% | 1,138 | 41.4 | Side by side | 
| GPT-6 Lunacheaper, ahead on every row | $0.20 | 55.4% | 51.8% | 1,337 | 53.4 | Side by side | 
| Claude Haiku 5.5cheaper, better on long jobs | $0.20 | 26.8% | 10.7% | 1,191 | 24.1 | Side by side | 

On product data MiniMax is far more careful than DeepSeek's new Flash at half its price, and well ahead of Claude Haiku 5.5. GPT-6 Luna is still ahead of it on every row here, costs a little less, and finished every task, so it remains the cheap default for business writing. Haiku 5.5 is the safer cheap choice for long analysis.

## Is MiniMax M3.1 Flash good? Who should use it

- **Product data and short copy at volume,** if you want a cheap model that mostly sticks to what you give it. Check the gaps: about one listing in 56 came back empty per run.
- **Not for long analysis, reports or decks** without a retry or fallback model. It often thinks until it runs out of room.
- **Not for anything that must answer in a second or two,** at least on the free route.
- **Wait for the paid route and the weights** before switching production work; we will rerun our tests on the paid route.

These benchmarks aren't your data, so test it on your own prompts first. Its 28 playground answers sit beside other models' answers to the same prompts.

## What we'll update

We'll rerun every benchmark once OpenRouter has a paid route, and add Artificial Analysis, Arena and the other sources as they publish. Until then, the MiniMax M3.1 Flash model page has every result as it lands.

Choosing or switching the model behind a product feature? Our custom evaluations test the models you are weighing on your own prompts and data, against your own definition of good. If AI writes your product content, the catalogue feed diagnostic measures how far you can trust it. Talk to us, or email hello@springprompt.com.

## Cheapest model that passes

On our benchmarks that define a pass for each task: what one passing answer cost in our run, failed attempts included, and whether a cheaper model passed.

- **CatalogBench** · cost per 1,000 accepted listings
MiniMax M3.1 Flash passed 106 of 168 attempts, with no estimate: no matching cost record. Cheapest: GPT-6 Luna at $0.70 (114 of 168 attempts).
- **DeckBench** · cost per presentable deck
MiniMax M3.1 Flash: no presentable decks (0 of 6 tasks). Cheapest: GPT-6.2 Sol at $0.09 (5 of 6 tasks).
- **SurveyBench** · cost per sound survey task
MiniMax M3.1 Flash: no sound survey tasks (0 of 9 tasks). Cheapest: GPT-6 Luna at $0.03 (3 of 9 tasks).

Cost per attempt divided by the share that passed, at the run date's prices, excluding judging. Each model at its cheapest setting per passing answer. With a handful of tasks, a pass or two either way is within chance. No estimate where a run had missing outputs, because their cost may not have been recorded.

## Quick answers

### How much does MiniMax M3.1 Flash cost?

MiniMax lists it at a flat $0.15 per million input tokens and $0.60 per million output tokens, with cached input at $0.015. That is half the price of DeepSeek-V4.1-Flash-1017. On launch day OpenRouter only offered a free, rate-limited route.

### Is MiniMax M3.1 Flash open-weight?

Yes. MiniMax said the weights would be published on 23 October 2026, and they are now on Hugging Face.

### Is MiniMax M3.1 Flash the same as Space Bunny?

MiniMax says Space Bunny, the anonymous model on OpenRouter, was an earlier version of M3.1 Flash. We did not benchmark Space Bunny, so we can't compare the two.

### Does MiniMax M3.1 Flash make things up?

Less than most cheap models on our product listing test: about 22 unsupported claims per run of 56 listings with a factual brief and 34 with a sales brief. DeepSeek-V4.1-Flash-1017, released two days earlier, added 95 and 224.

### Is MiniMax M3.1 Flash good at long tasks?

Not in our tests. On 18 of 24 survey planning and analysis steps it used its whole 32,000-token budget thinking and returned nothing, and two of its eight slide decks never arrived.

### MiniMax M3.1 Flash or GPT-6 Luna?

GPT-6 Luna, for now. It scored higher on product listings, slide decks and ad budgets on our tests, at a lower blended price, and it finished every task. MiniMax was close behind it on product listings.

Release dates: every MiniMax model release · new AI models released this week
