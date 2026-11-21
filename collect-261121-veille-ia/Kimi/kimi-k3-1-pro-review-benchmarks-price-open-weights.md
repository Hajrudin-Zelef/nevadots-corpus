---
id: collect-261121-veille-ia/kimi/kimi-k3-1-pro-review-benchmarks-price-open-weights
title: "Kimi K3.1 Pro Review: Benchmarks, Price, Open Weights"
domain: kimi
role: reference
task: article
actors: ["Moonshot AI"]
dates: ["2026-11-05"]
keywords: ["kimi", "best", "models", "weights", "benchmarks", "million", "moonshot", "price"]
source: https://springprompt.com/blog/kimi-k3-1-pro-review
source_anchor: ""
source_lines: [1, 156]
section: "Kimi"
---
Test Kimi K3.1 Pro on your prompt → It runs beside the models you could use instead, with every answer checked and costed.

Moonshot released Kimi K3.1 Pro through its API on 23 October and promised the weights by 4 November. They arrived on time. It is a post-trained version of Kimi K3 with 2.8 trillion parameters, and Moonshot says it is close to Claude Opus 5.5 on Terminal-Bench 4.0.

On our benchmarks it is the highest-placed open-weight model, 12th of 31 overall, and did well on our ad budget simulation. It is also very slow: a single playground page took up to about half an hour. And on most of our survey tasks it thought until it ran out of room and returned nothing.

## Kimi K3.1 Pro at a glance

- **Use it for** work where open weights matter and nobody is waiting for the answer, with a generous output limit.
- **Don't use it for** anything interactive or time-bound, or long analysis with a tight token limit.
- **Price:** $2/$10 per million input/output tokens, $0.20 cached.
- **Weights:** published on Hugging Face, about 1.56 TB of files.
- **Speed:** about 21 minutes a slide deck and 33 minutes a survey task in our runs of 27 October.

## What Moonshot says

Moonshot's launch post reports 65.7% on Terminal-Bench 4.0, against 66.4% for Claude Opus 5.5 and 58.2% for GPT-6 Astra, and a fall in claim-level hallucination on its own stress test from 23.4% for K3 to 8.3%. Its model card shows it well behind on others, such as Program Bench (31.5 against 65.0). It has a 1,048,576-token context and takes text and images; a Highspeed version is said to run two to three times faster. For the open-weight release Moonshot says it "identified and removed cyber-related data from both training stages". These are the developer's claims.

The licence allows commercial use and modification. A business running a model-as-a-service operation with over $20 million of revenue in 12 months needs a separate agreement, and products with over 100 million monthly users or $20 million in monthly revenue must display the model's name. Internal use is exempt from both.

## Kimi K3.1 Pro benchmarks, measured by Spring Prompt

The model page has every result. How to read this table: each row is one benchmark, with Kimi's score and setting, its rank among models we have tested that are on sale today, and its rank among models priced $3 to $10 per million tokens.

| Benchmarkwhat it measures | Kimi K3.1 Proits score | Rank among all modelsavailable today | Rank at its price$3–10 per million tokens, blended | 
|---|---|---|---|
| ROASBenchscore in a 12-month ad budget simulation | 55.1default setting | 3rd of 28Best: GPT-6.1 Terra, 58.4 | 1st of 12Best: Kimi K3.1 Pro, 55.1 | 
| DeckBenchhead-to-head rating of slide decks | 1,247default setting | 12th of 28Best: GPT-6 Astra, 1,606 | 7th of 13Best: GPT-6 Sol, 1,488 | 
| SurveyBenchhead-to-head rating of survey plans and analyses | 608default setting | 16th of 17Best: Muse Wonder, 1,510 | 7th of 7 (last)Best: Muse Wonder, 1,510 | 
| BulletBench, one minutechess rating against a 60-second clock | 147low reasoning | 22nd of 29Best: Gemini 3.8 Flash, 1,068 | 9th of 11Best: Gemini 3.8 Flash, 1,068 | 

Ranked on the release of 5 November 2026 (44c5fb63d296). A group only counts models that benchmark has measured. Ranks are provisional. CatalogBench is left out because it ran one repeat per brief; see below.

Overall it scores 56, 12th of 31, and first of the 8 open-weight models and the 10 models from Chinese developers on our board. Its score leaves out the groups with no results yet: it has 2 of 5, and both are our own benchmarks. CatalogBench isn't counted either, so treat this rank as a rough position.

### Surveys: out of room, not out of ideas

On SurveyBench its plans were excellent, 97 of 100. Then on 10 of 12 tasks the analysis came back empty, because it spent our whole 32,000-token limit thinking. It rated 608 (range 473 to 731), 16th of 17. That shows what happens at default thinking under our limit, not how good its analyses are: we barely saw any. Moonshot's default output limit is 131,072 tokens, so a higher limit or a lower reasoning setting might go better; we haven't tested either. Each task still cost about $0.74 and took about 33 minutes.

### Ad budgets and decks

In ROASBench, a 12-month simulation of a skincare brand's ad budget, it scored 55.1 and finished with $858,023 of simulated profit, with two loss-making months and one reply that needed repair. It is one simulated year, not evidence of real returns. The run cost $1.60 against GPT-6.1 Terra's $0.11.

On DeckBench all eight decks arrived and it rated 1,247 (range 1,123 to 1,388), 12th of 28, level with Kimi K3 given that range. None of the six public decks could be presented as it was, because every one had a layout defect. Each took about 21 minutes and $0.54.

### Product listings: one pass only

We ran CatalogBench once per brief instead of three times, so these figures aren't comparable with models averaged over three repeats, and it has no reliability score. On that pass 78.6% of its 56 listings were publish-ready on the factual brief and 75.0% on the sales brief, with no unsupported claims on the first and 12 on the second, where two products also came back empty. It took about 114 seconds a product, the slowest of 30 results, against 10 for GPT-6.1 Terra; our API speed page has the timings for every model. The products are invented.

### Speed chess: mostly lost on time

Four of its five BulletBench configurations lost their first four games on time and were screened out, including every default-setting run. Only low reasoning at one minute got through, rating 147 (range 0 to 381), with seven of eight games lost on time and a median 5 seconds a move, the slowest of 44 results. This is chess against a clock, not general intelligence.

### In the playground

It answered all 29 of our playground prompts on 27 October, one of them at the second try. The median page took 15 minutes and the slowest about half an hour (29 minutes), for $9.61 in all. GPT-6.1 Terra's median was about a minute, for $0.85. We have since opened every answer by hand, and it is not our pick on any page. These are one-off prompts, not a benchmark, so read them as examples of what it makes. All 29 answers are here.

## Third-party results

None count towards its rank yet, because the Artificial Analysis data in our 5 November release dates from 20 October, so the model page shows it as not measured. Artificial Analysis's page, read on 5 November, gives 46 on Intelligence Index v5.0.1, #22 of 66, with 33 tokens a second, 36.0 minutes and $3.68 per index task. That page still calls the model proprietary. A score of 55 was quoted on Hacker News on launch day, and we haven't traced why the two differ. Data sourced from Artificial Analysis.

## What people are saying

**How it read on Hacker News: 63% positive, 12% mixed, 24% negative** of the 41 comments that took a side

On 5 November we read every top-level comment on the launch thread and the weights thread, and labelled each one positive, mixed, negative, neutral or off-topic. Neutral and off-topic comments are left out. Labels are by Claude Opus 5.5 against a written rubric, in one pass. One-word reactions count as positive, and most comments came within hours of launch, before people had used it for long. Every labelled item.

A selection, with how we read each one:

Big improvement over Kimi K3 in our data analytics benchmark

Much slower than both though.

So far K3.1 pro is almost twice as fast as k3 on our benchmark.

If you are expecting any sort of usable limits on their subscription plan I suggest adjusting your expectations.

Very strong open model release that is a bit off the frontier

One comment on the weights thread matters for speed: minimaxir reported on 4 November that Together had about double the throughput of Moonshot's endpoint at the same price. We haven't measured that.

## Kimi K3.1 Pro vs GPT-6.1 Terra, Claude Opus 5.5 and Kimi K3

Prices are blended per million tokens (three input to one output) at list price. Scores are measured by Spring Prompt at each model's best configuration; a dash means no result.

| Modelwhy it's here | Priceblended, list | Overallprovisional where marked | DeckBenchrating | SurveyBenchrating | ROASBenchscore | Compare | 
|---|---|---|---|---|---|---|
| Kimi K3.1 Prothis first look | $4.00 | 56, provisional | 1,247 | 608 | 55.1 | – | 
| GPT-6.1 Terraclosed, launched the same week | $2.00 | 83, provisional | 1,344 | 1,273 | 58.4 | Side by side | 
| Claude Opus 5.5the model Moonshot measures itself against | $8.00 | 84 | 1,391 | 1,486 | 51.7 | Side by side | 
| Gemini 3.8 Flashjust above it overall | $3.00 | 60 | 823 | – | 51.4 | Side by side | 
| Kimi K3the predecessor | $6.00 | 48 | 1,233 | – | 44.1 | Side by side | 

If open weights don't matter to you, GPT-6.1 Terra is the better buy on our tests: cheaper, far quicker and ahead on all three. If they do, K3.1 Pro is a clear step up from K3 on ad budgets at two-thirds of its price, and level on decks. Our Terra review has the detail, and you can run your own prompt on both models to compare the answers, cost and time.

## Is Kimi K3.1 Pro good? Who should use it

- **Teams that need open weights** and have the hardware or a host. It is the top open-weight model on our board.
- **Background jobs with no one waiting,** with the output limit raised well above 32,000 tokens.
- **Not for interactive work.** Expect minutes, not seconds. Compare hosts first, as speed may differ from our runs.
- **Not on a tight token limit.** Thinking can use all of it, and you still pay for those tokens.

These benchmarks aren't your data, so test it on your own prompts first, and time it on the host you plan to use.

## What we'll update

Refreshed Artificial Analysis data and the other third-party sources will go in as they enter the catalogue. The Kimi K3.1 Pro model page has our results as they stand.

Choosing or switching the model behind a product feature? Our custom evaluations test the models you are weighing on your own prompts and data, against your own definition of good. If AI writes your product content, the catalogue feed diagnostic measures how far you can trust it. Talk to us, or email hello@springprompt.com.

## Cheapest model that passes

On our benchmarks that define a pass for each task: what one passing answer cost in our run, failed attempts included, and whether a cheaper model passed.

- **CatalogBench** · cost per 1,000 accepted listings
Kimi K3.1 Pro: $57.83 (44 of 56 attempts), 23 of 27. Cheapest: GPT-6 Luna at $0.70 (114 of 168 attempts). Cheaper and passed at least as often: GPT-6.2 Sol at $10.44 (133 of 168 attempts).
- **DeckBench** · cost per presentable deck
Kimi K3.1 Pro: no presentable decks (0 of 6 tasks). Cheapest: GPT-6.2 Sol at $0.09 (5 of 6 tasks).
- **SurveyBench** · cost per sound survey task
Kimi K3.1 Pro: no sound survey tasks (0 of 9 tasks). Cheapest: GPT-6 Luna at $0.03 (3 of 9 tasks).

Cost per attempt divided by the share that passed, at the run date's prices, excluding judging. Each model at its cheapest setting per passing answer. With a handful of tasks, a pass or two either way is within chance. No estimate where a run had missing outputs, because their cost may not have been recorded.

## Quick answers

### How much does Kimi K3.1 Pro cost?

$2 per million input tokens and $10 per million output on OpenRouter, with cached input at $0.20. Moonshot bills at a flat rate across the 1M-token context, and says batch requests cost 60% of the standard price.

### Are Kimi K3.1 Pro's weights available?

Yes. The weights are on Hugging Face, in a repository dated 2 November 2026 and announced on Hacker News on 4 November, Moonshot's stated deadline. The files are about 1.56 TB. The licence is permissive, with extra terms for very large businesses.

### How fast is Kimi K3.1 Pro?

Slow. In our runs of 27 October it took about 21 minutes a slide deck and 33 minutes a survey task. Artificial Analysis reported 33 tokens a second on 5 November. Other hosts may be faster now that the weights are out.

### Is Kimi K3.1 Pro good?

It is the top open-weight model on our provisional leaderboard, 12th of 31 overall on 5 November 2026. It was 3rd of 28 on our ad budget simulation, but it often runs out of token budget on long jobs.

### Why did Kimi K3.1 Pro do so badly on SurveyBench?

On 10 of 12 tasks its analysis came back empty, because it spent our whole 32,000-token limit thinking. Its plans scored 97 of 100. The result shows what happens under that limit, not how good its analyses are.

### Kimi K3.1 Pro or GPT-6.1 Terra?

GPT-6.1 Terra, unless you need open weights. On our tests Terra scored higher on decks, surveys and ad budgets, at half the list price and many times the speed.

Release dates: every Moonshot AI model release · new AI models released this week

## Will Kimi K3.1 Pro work for your product?

Benchmarks aren't your task. Paste the prompt your product uses and run it beside the models you could use instead, with every answer checked and costed.

New here? Your email starts a free trial. No credit card needed.
