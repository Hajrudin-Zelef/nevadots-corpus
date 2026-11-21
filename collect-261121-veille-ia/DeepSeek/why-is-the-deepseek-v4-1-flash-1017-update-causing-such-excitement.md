---
id: collect-261121-veille-ia/deepseek/why-is-the-deepseek-v4-1-flash-1017-update-causing-such-excitement
title: "Why Is the DeepSeek V4.1 Flash 1017 Update Causing Such Excitement?"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: []
keywords: ["deepseek", "model", "update", "deepswe", "flash", "bench", "version", "tests"]
source: https://www.gptunnel.ru/en/blog/deepseek-v4-1-flash-1017
source_anchor: ""
source_lines: [1, 332]
section: "DeepSeek"
---
DeepSeek released the V4.1 Flash 1017 update and called it a minor one (the same model, just further trained), yet users keep asking one thing: why is it still 4.1 and not 4.2 or 4.5 - **that is how much they liked the update**. In this article we go through the company's own measurements, benchmarks from independent teams (sference, Bug Hunt Bench, ChessBench) and user reviews - read our review to the end.

Let's start with the core of the update - on the price-versus-quality chart for the DeepSWE agentic test it ended up higher and further left than all its rivals: **it solves 77% of tasks, and a task costs it dozens of times less than Claude Opus 5 or GPT-6 Astra, which solve about 74%.** The update came out on October 17, the weights are open, and the new version already runs on the website, in the app and in the API. Below you will find the full table of tests, the chart, the first independent measurements and the model's weak spots.


*DeepSeek's chart. Competitor data comes from the public DeepSWE leaderboard as of September 22, 2026, and from OpenAI charts.*

## What DeepSeek promises

The announcement makes three main claims:

- **A Pareto improvement.** The model is stronger on tests, finishes tasks faster and answers more briefly than the September version. In other words, it is better and cheaper at the same time, without trading one for the other.
- **Whole projects, not pieces of code.** DeepSeek says the model can build a project from start to finish on its own: for example, write a game or create a 3D model. DeepSeek posted example videos on X. To get there, the model checked and scored its own results during training and then learned from those scores. There are no human checkers and no separate judge model in this scheme (The Decoder).
- **Its own harness.** The model is tuned for DeepSeek Harness (the company's open agent framework), Tencent CodeBuddy and WorkBuddy. For Harness, version 0.2.1-rc.1 or newer is recommended.

"Projects from scratch" can so far be checked only through DeepSeek's videos and individual reviews. For example, one user asked the model to build a showcase of Angular components through DeepSeek Harness and was happy with the result. But it is a single review.

## The full table: where 1017 leads and where it does not

DeepSeek compared the update with the previous version and four competitors. The best result in each row is in bold.

| Test | 1017 | September | Kimi K3 | GLM 5.3 | GPT-5.6 Sol | Opus 5 | 
|---|---|---|---|---|---|---|
| HLE (no tools) | 42.6 / 44.0* | 36.8 / 39.1* | 43.5 | 42.0* | 44.5 | **56.3** | 
| Terminal-Bench 4.0 | 42.3 | 31.2 | 12.6 | 37.9 | 39.9 | **51.8** | 
| DeepSWE v1.1 | **77.0** | 74.2 | 67.5 | 66.9 | 73.0 | 74.0 | 
| ProgramBench | 29.1 | 20.3 | 17.5 | 19.0 | 23.0 | **37.0** | 
| NL2Repo-Bench | 68.2 | 65.4 | 58.0 | 58.0 | 56.8 | **75.3** | 
| Automation-Bench v1.0.6 | **60.5** | 54.8 | 46.7 | 48.8 | 45.8 | 50.3 | 
| Agents' Last Exam | **35.4** | 31.8 | 27.6 | 28.5 | 26.7 | 28.6 | 
| Apex Bench v1.1 | **72.1** | 64.6 | 47.8 | 56.6 | 54.3 | 65.8 | 
| Chartography (with tools) | 83.0 | 78.9 | 68.1 | - | 79.9 | **84.0** | 
| BabyVision (with tools) | 92.5 | 89.6 | 85.7 | - | 88.9 | **94.1** | 
| ZeroBench-main (with tools) | 51.0 | 49.0 | 41.0 | - | **53.0** | 52.0 | 

** text-only part of HLE. Terminal-Bench 4.0 was run in the Claude Code environment, DeepSWE in the Mini-SWE-V2 framework, and ZeroBench was scored by Pass@5.*

What you can see if you read more than the bold numbers:

- **Every one of the 11 tests improved over September.** The biggest gains are in Terminal-Bench (+11.1), ProgramBench (+8.8) and Apex Bench (+7.5), the smallest in ZeroBench (+2.0).
- **1017 beats Kimi K3, GLM 5.3 and GPT-5.6 Sol almost everywhere.** The exceptions are HLE, where K3 and Sol are slightly higher, and ZeroBench, where Sol is higher.
- **Against Opus 5 the score is 4:7.** 1017 wins the agentic tests (DeepSWE, Automation-Bench, Agents' Last Exam) and Apex Bench, while Opus 5 leads in knowledge (HLE, a gap of almost 14 points), terminal work, programming (ProgramBench, NL2Repo) and vision tests.
- **Opus 5 is already the previous generation.** Opus 5.5 came out on September 22 and, according to Anthropic, scores 66.4% on Terminal-Bench 4.0 (Opus 5 scores 52.3% on the same data). It is not in DeepSeek's table, so do not look for a comparison with Anthropic's current flagship in these numbers.

On Reddit people noticed something else: this time the list of tests is shorter and more agentic than in the card for the September version. The tests are standard, but they are chosen so that the model's strengths show better than its weaknesses.

## The cost-per-task chart: how to read it

On the chart at the start of the article, the vertical axis is the share of DeepSWE tasks solved, and the horizontal axis is the average cost of one task at vendors' API prices. The scale is logarithmic: each step to the right is roughly two to three times more expensive. The low, high and max points are reasoning levels: the higher the level, the longer the model thinks and the more expensive the task.

Here is what you get if you estimate from the chart:

- **1017 in max mode** solves 77% of tasks, and a task costs roughly 2.5 times less than for the September version in the same mode (74.2%).
- **1017 in low mode** solves about 72.5% - slightly less than September's max, but 4-5 times cheaper.
- **The closest competitor in quality** is GPT-6.1 Sol in high mode, about 75%, with a task costing roughly three times more than 1017 in max.
- **Gemini 3.8 Flash, GPT-6 Astra and Claude Opus 5** stay around 73-74%, but a task costs 12-60 times more.
- **Claude Fable 5** does not reach 70% even in max mode and is the most expensive point on the chart.
- **Models in the same price zone** - GPT-6 Luna, GLM 5.3 Flash and the September Flash in low mode - solve about 61-67%.

The chart has caveats. It was drawn by DeepSeek, and the competitors' data comes from the DeepSWE leaderboard as of September 22 - the day Opus 5.5 came out, so it is not on the chart. This is one test, one harness (mini-swe-v2) and vendors' API prices, not ours.

## Independent measurement: reasoning 20-30% shorter, same answers

The most detailed check on release day was done by sference, a company that provides access to open models itself. It compared the old and new V4.1 Flash weights and kept everything else the same: the same tasks, the same hardware, the same generation settings. This way the difference in results comes only from the update.

They tested on three sets of tasks that have an exact correct answer. Answers were checked by a program, not by another neural network acting as a judge, so the score does not depend on anyone's taste:

- **AIME 2024** - problems from the American math olympiad (American Invitational Mathematics Examination). They took the 10 problems on which models reason the longest.
- **MATH-500** - a set of 500 math problems of varying difficulty, from school level to olympiad level. They took 10 problems, a few from each difficulty level.
- **BFCL** (Berkeley Function Calling Leaderboard) - a test of calling tools, that is, functions the model calls as it works. 20 tasks: call one function, pick the right one from several, call several functions at once, and refuse to call anything if no function fits.

Each task was run three times. The old weights were run twice in a row to measure random variation: any difference smaller than this variation does not count as a result. In total, 1,440 requests.

### Shorter reasoning, same answers

- On AIME the median answer got 21% shorter, from 1,681 to 1,124 tokens, and the reasoning text itself 30% shorter.
- The number of correct answers is almost the same: 101 out of 120 for the old version and 100 out of 120 for the new one. A difference of one task is within random variation.
- On MATH-500 there were about 27% fewer tokens.


*Reasoning length on AIME 2024 problems: white bars are the previous version, red bars are 1017, the median of three runs. Both versions solved all the problems on the chart correctly. The longer the old version's reasoning was, the more the new one shortened it: the longest problem shrank by half, from 3,814 to 1,880 tokens. Short problems barely changed, and the two shortest even got slightly longer. Chart: sference.*

### The reasoning-length setting now works

In the API, V4.1 Flash has a `reasoning_effort` parameter with values from 1 to 100: the lower the number, the shorter the model is supposed to think. In the old version it barely worked below the default. Even at 1 the median on hard problems was 1,974 tokens, and at 10 the reasoning came out even longer, 2,202 tokens.

In 1017 the setting works:

- at value 1 the median dropped to 790 tokens - 60% less than the old version with the same setting;
- the longest reasoning chain shrank from 10,280 to 5,940 tokens;
- according to the author, accuracy held at all tested values: at 1 the model solved 29 of 30 hard problems;
- at value 25 the model solved every problem in both math sets, and it cost about 20% less than with the default setting.

For those who pay for the API this is the main news of the update. Before, lowering `reasoning_effort` to save money barely worked; now it does.

### Tool calling has not changed

In BFCL the old and new versions made mistakes on the same tasks:

| Category | Old version | 1017 | 
|---|---|---|
| Call one function | 15 of 18 | 15 of 18 | 
| Pick one function out of 2-4 | 9 of 12 | 9 of 12 | 
| Call one function several times in parallel | 12 of 12 | 12 of 12 | 
| Call different functions in parallel | 3 of 12 | 3 of 12 | 
| Refuse when no suitable function exists | 2 of 6 | 2 of 6 | 

Two weak spots are visible right away. The model can barely call different functions in parallel, but this is a common problem: Kimi K3 and GLM 5.3 solved 3-5 of 12 tasks in the same test. And when there is no suitable function, the model more often invents a call than answers without one. The author advises keeping the tool list short and stating directly in the instructions that answering without a function call is allowed.

### Comparison with Kimi K3 and GLM 5.3

sference ran the same tasks on two other open models, all three with default settings:

| Model | AIME | MATH-500 | Tool calling | Cost of a correct answer to a hard task | 
|---|---|---|---|---|
| DeepSeek V4.1 Flash 1017 | 29 of 30 | 30 of 30 | 41 | baseline | 
| Kimi K3 | 28 of 30 | 30 of 30 | 39 | 5.7 times more expensive | 
| GLM 5.3 | 26 of 30 | 29 of 30 | 40 | 3.4 times more expensive | 

The price here is calculated at the API rates that sference uses. GLM 5.3 reasons the shortest, about half as long as V4.1 Flash, but makes more mistakes on hard tasks. For V4.1 Flash itself, a correct answer to a hard task became about 14% cheaper after the update, and overall on tasks with long reasoning the author estimates the savings at 20-30%. If you set `reasoning_effort` to 25, you can save about another 20%.

### Caveats

- The math sets have only 10 tasks each, so exact scores should be read with an error of one task. But the direction of change was the same on every task.
- AIME and MATH-500 are most likely in the training data of all large models, so accuracy here may be hitting a ceiling. This does not affect the conclusion about reasoning length.
- Function calls were checked against a reference by name and arguments, but not executed.
- Answer length and answer time are different things: sference did not measure speed.

## Bug Hunt Bench: finding bugs in real repositories

### How the test works and what came out

Paweł Huryn (@PawelHuryn) tested 1017 on his own benchmark, Bug Hunt Bench: models get two repositories with 105 hidden bugs and must find and fix whatever they can. According to the author, these are real bugs that frontier models missed at the start of 2026. Only planted bugs count; fixes are verified blind against a withheld answer key. Real issues found beyond the key are counted separately and do not go into the score: the author explained that some OpenAI models report many real but irrelevant issues to inflate the result.

After the first run, 1017 (max mode) fixed 34 of 105 bugs, while the September V4.1 Flash in the same mode fixed 21.7. The author called it a great improvement and wrote that it is strange the model was not named V4.2. He then did two more runs and noted that the result for 1017 varies more than usual: the individual runs gave 34, 37 and 45 bugs, 38.7 on average. Before that, he had expected the score to change by about 1.7 points after three runs.


*Screenshot of an X post: results after three runs.*

Results from this post:

| Model (mode) | Bugs fixed out of 105 | 
|---|---|
| Muse Wonder Full 1.0 (max) | 42 | 
| Claude Opus 5.5 (max) | 41.7 | 
| Gemini 4 Argon (high) | 40.7 | 
| **DeepSeek V4.1 Flash 1017 (max)** | **38.7** | 
| Grok 4.7 (xhigh) | 29 | 
| Qwen3.8-Max (max) | 25.7 | 
| DeepSeek V4.1 Flash, September (max) | 21.7 | 
| GLM-5.3 (max) | 19 | 

The average result of 1017 is lower than that of Muse Wonder Full 1.0, Opus 5.5 and Gemini 4 Argon, but clearly higher than that of Grok 4.7, Qwen3.8-Max and GLM-5.3, and 1.8 times higher than the September version. The best run of 1017 (45 bugs) is higher than any average in this table. Because of the large spread across three runs, it is too early to draw precise conclusions.

On the full benchmark table (it shows 23 of 117 runs, updated October 17), Sonnet 5.5 (max) is on top with 51.3 bugs, followed by GPT-6 Astra (max) with 45 and GPT-6.1 Sol (max) with 44.3. The author separately clarified that Sonnet's result is correct: it comes from the model making 7 times more turns than GPT-6 Astra (max). On this table 1017 so far has one run: 34 bugs, 50.6 minutes and $0.90. This is a real API bill; for most other models the cost is estimated from list prices. For comparison, Opus 5.5 (max) scored 41.7 bugs in 66.9 minutes for $58.53, Gemini 4 Argon (high) scored 40.7 in 56.5 minutes for $7.16, and Sonnet 5.5 (max) took 287.1 minutes and $154.03.


*Bug Hunt Bench table (Paweł Huryn): bugs fixed, time and cost of a run. The arrow marks the move from the September version to 1017.*

### Price and result

The same author built a "score against run cost" chart. The cost axis is logarithmic: the further left a point is, the cheaper the run, and the higher it is, the more bugs were fixed. The chart shows 47 of 118 runs, and 1017 has two of them: max mode (38.7 bugs, about $1) and high mode (32 bugs, about $0.8). The cost of 1017 is a real bill; for other models it is an estimate from list prices, so the comparison is approximate. Amounts without an exact label (about $1, about $0.8, about $3.3, $1.3 and $1.8) were read from the logarithmic axis and are approximate, while scores are taken from the chart legend.

What you can see on the chart:

- **Max mode.** Among runs scoring above 35 bugs, 1017 (max) has the lowest cost. The closest is GPT-6.1 Sol (high) with 36.5 bugs for about $3.3. GPT-6.1 Sol (xhigh) fixes 42.7 bugs and costs $4.35, which is roughly 4 times more than 1017 for a result 4 bugs higher.
- **High mode.** With 32 bugs, 1017 (high) beats GPT-6.1 Sol (low) with 22.5 bugs for $1.3 and GPT-6.1 Sol (medium) with 29 bugs for $1.8, and costs less than both.
- **The expensive end.** Claude Opus 5.5 (max) with 41.7 bugs costs $58.53, and Fable 5.1 (max) with 43 bugs costs $87.18. Their result is higher than that of 1017, but the price is dozens of times higher.


*Bug Hunt Bench chart (Paweł Huryn): score against run cost, 47 of 118 runs.*

## User reviews and benchmarks

In the first day users posted several tests of their own: on game and 3D tasks, in a chess benchmark and in everyday development. These are single measurements on different tasks, but they were made by outside people, not by DeepSeek.

### Rocket launch: DeepSeek V4.1 Flash 1017 vs GPT-6.1 Sol

User Bhavy (@Bhavani_00007) asked DeepSeek V4.1 Flash 1017 and GPT-6.1 Sol to make a rocket launch scene and compared the result on video. According to him, DeepSeek did it in 3 minutes for $0.78, while GPT-6.1 Sol took 5 minutes and $6.10: almost 8 times more expensive and 1.7 times longer. The scenes came out different: in DeepSeek's the rocket stands by a lattice tower against green hills, in GPT-6.1 Sol's the rocket has side boosters, with trees, sea and clouds. The author was very happy with DeepSeek's result.


*Screenshot of an X post: DeepSeek V4.1 Flash 1017 on the left, GPT-6.1 Sol on the right.*

### Floating cities: DeepSeek V4.1 Flash 1017 vs GPT-6.1 Sol Ultra

User Iam_ (@SPAC89) compared DeepSeek V4.1 Flash 1017 in max mode and GPT-6.1 Sol Ultra on a 3D scene: two cities floating above the clouds, connected by a looping railway with a train. According to him, he still cannot believe that a model that is 90% cheaper produces a result this good. Both models took roughly the same time, because most of it went on rendering. In the screenshot DeepSeek's scene has a night sky with a moon, clouds, round islands with towers and a glowing ring in the middle, while GPT-6.1 Sol Ultra's has mesh-like tracks in a figure-eight shape and towers in blue and gold tones.


*Screenshot of an X post: DeepSeek V4.1 Flash 1017 (max) on top, GPT-6.1 Sol Ultra below.*

### 3D model of Bowser: DeepSeek V4.1 Flash 1017 vs Claude Opus 5.5

User Luckey Faraday (@luckeyfaraday) gave DeepSeek V4.1 Flash 1017 and Claude Opus 5.5 the same one-line prompt: build a 3D model of Bowser. The results card says DeepSeek worked in max mode and Opus in xhigh mode. DeepSeek spent 32 minutes 42 seconds, 8.05 million tokens and $0.15; Opus spent 23 minutes 42 seconds, 3.88 million tokens and $3.85 (API cost is an estimate). So DeepSeek is almost 26 times cheaper, but 9 minutes slower and uses twice as many tokens. Both Bowsers are recognizable: DeepSeek's is darker and more textured, with a dark green shell and an orange belly, while Opus's is light green, smoother and more cartoonish.


*Screenshot of an X post: DeepSeek V4.1 Flash 1017 on the left, Claude Opus 5.5 on the right. One prompt, cost is an estimate.*

### A honeybee on a sunflower in 3D

User Loktar (@loktar00) wrote that 1017 handles 3D well and showed a video: in DeepSeek Harness, in a single turn, the model built a scene with a honeybee on a sunflower. The video caption shows 964k tokens. This is a single example with no comparison to other models, so it shows that the result is possible, not how much better it is than others.


*Screenshot of an X post: a frame from the video, the caption in the corner - DeepSeek Harness, one turn, 964k tokens.*

### Parkour run: four models, one prompt

One user gave four "fast" models the same assignment: make a first-person parkour run in a single HTML file, four stages, one fixed round. According to him, DeepSeek produced the best effects of the four: a city of thousands of glowing windows. Here is what each model spent (data from the post, video with the thehype logo):

| Model | Tokens | Time | Run cost compared with DeepSeek | 
|---|---|---|---|
| DeepSeek V4.1 Flash 1017 | 298k | 1 h 27 min | baseline | 
| Claude Haiku 5.5 | 62.3k | 23 min 33 s | 43% cheaper | 
| GPT-6 Luna | 62.4k | 8 min 7 s | 3.9 times cheaper | 
| Qwen 3.8 Flash | 448k | 2 h 4 min | 61% more expensive | 


*Screenshot of an X post (video with the thehype logo): the scenes of the four models and the spend of each. DeepSeek V4.1 Flash 1017 is top left.*

In the comments under the post people agreed that DeepSeek produced the best version among GPT-6 Luna, Claude Haiku 5.5 and Qwen 3.8 Flash. The price of that is consumption: it uses 4.8 times more tokens than GPT-6 Luna, and takes 10.7 times longer. So the claim that "answers got shorter" refers to the comparison with DeepSeek's previous version, not with all competitors. Nobody compared against the previous version in this test.

### Tesla Roadster track in voxels

Another user (a post linking to the atomic.chat service) asked DeepSeek V4.1 Flash 1017 and GPT-6 Luna to recreate the Tesla Roadster event in 3D voxels. DeepSeek's run, according to the author, cost 17% less than GPT-6 Luna's. The screenshot shows that both models made a working scene with G-force and altitude counters. GPT-6 Luna has a more elaborate metal structure around the track, DeepSeek's is simpler, but it has a robot and spotlights.


*Screenshot of an X post: DeepSeek V4.1 Flash 1017 on the left, GPT-6 Luna on the right.*

### Interactive character: Claude Opus 5.5 vs DeepSeek V4.1 Flash 1017

User @karanC_12 compared DeepSeek V4.1 Flash 1017 with Anthropic's flagship. Both models built a 3D playground with a soft character that can be grabbed, squashed, stretched and poked, with a mood switcher and outfit choice (the video is taken from its author, @marcthecreatorr). Claude Opus 5.5 worked in xhigh mode, DeepSeek in max mode. According to the author, Opus finished in 20 minutes for $6, DeepSeek in an hour for $0.23. The author thinks Opus did better, but DeepSeek came surprisingly close at a price 26 times lower.


*Screenshot of an X post: Claude Opus 5.5 (xhigh) on top, DeepSeek V4.1 Flash 1017 (max) below.*

### Chess: ChessBench

On chessbench.org language models play chess against each other and get an Elo rating. At the start of each move the model is given a list of legal moves, and an illegal move counts as "cheating" and is rejected. The rating compares models only within this test and is not equal to ratings on chess sites. Here is the table for October 17, 17:16 UTC (models that are in our catalog or interesting for comparison were picked from it):

| Rank | Model | Elo rating | Wins | Illegal moves | 
|---|---|---|---|---|
| 1 | Gemini 4 Argon | 2159 | 92% | 0.17% | 
| 2 | Claude Opus 5.5 | 2109 | 89% | 0.54% | 
| 4 | GPT-6 Astra | 2033 | 85% | 0.02% | 
| 6 | GPT-6.1 Sol | 1817 | 67% | 0.00% | 
| **8** | **DeepSeek V4.1 Flash 1017** | **1692** | **52%** | **0.32%** | 
| 9 | Kimi K3 | 1609 | 45% | 1.15% | 
| 10 | DeepSeek V4.1 Flash (September) | 1540 | 42% | 0.87% | 
| 12 | GLM 5.3 | 1497 | 38% | 1.54% | 
| 14 | GPT-6 Luna | 1400 | 20% | 0.42% | 

The updated version gained 152 points and two places: from 10th to 8th. It beat Kimi K3, GLM 5.3 and GPT-6 Luna, but trails Claude Opus 5.5, Gemini 4 Argon, GPT-6 Astra, GPT-6.1 Sol and three more Anthropic models: Sonnet 5.5, Fable 5.1 and Haiku 5.5. The benchmark's author (@bylupu) noted that the price and token usage barely changed (218.7k tokens per batch instead of 228.9k), while the share of illegal moves dropped noticeably: from 0.87% to 0.32%. Time per move, however, grew from 32.0 to 38.3 seconds. This is the only measurement in the article where 1017 can be compared both with the previous version and with flagships on one task, and the picture agrees with DeepSeek's table: clearly better than its predecessor and than competitors of its class, but not on the level of flagships.

### Developers' impressions

- **Animations.** User 花叔 writes on X (a post in Chinese) that in animation effects 1017 is "incredibly impressive" and only a step below Claude, while with GPT-6 he could not get such results. He adds that he used his own animation skill, which he had just made, and the result was obtained together with it.
- **Speed.** User 张晓宇 (@xiaoyu_agi) compared three 3D scenes: Claude worked them out more carefully, but the task took 40 minutes to an hour, while 1017 took a little over ten minutes.
- **Projects from scratch.** Developer @D2KX_ used DeepSeek Harness to give the simplest possible prompt: create an Angular app showing Angular Material components and the CDK library. The prompt relied on official Angular skills that had been connected beforehand, and the model built the demo on its own, with no extra hints.
- **Faster, but not in chat.** A r/LocalLLaMA user ran both versions on his own set of chat and tool tasks and posted a table: the median response time fell from 16.8 to 7.4 seconds, and his overall quality score rose from 3.46 to 3.62. The author did not say how this score is calculated, so rely on the time. He ran the old version through a different provider, so the numbers are approximate.

### What is still missing

Artificial Analysis has not measured 1017 yet. They only have a measurement of the September version: an index of 39 points and 9th place out of 120 at the time of checking. So there is no composite quality index from Artificial Analysis for 1017 yet, but there are two independent quality measurements: Bug Hunt Bench and the ChessBench chess rating.

Taken together, the reviews show four things. First, on visual tasks (animations, games, 3D) users are happy with the result and often compare it with models from Anthropic and OpenAI. Second, in every comparison with flagships a DeepSeek run is much cheaper: almost 8 times cheaper than GPT-6.1 Sol and 26 times cheaper than Opus 5.5. Third, the speed data is mixed: 1017 is faster than its own previous version (7.4 seconds versus 16.8), than GPT-6.1 Sol in the rocket scene (3 minutes versus 5) and than Claude in three 3D scenes (a little over 10 minutes versus 40-60), but in two other comparisons Opus 5.5 finished earlier (23 minutes versus 32 and 20 minutes versus an hour), and against GPT-6 Luna and Claude Haiku 5.5 it depends on the task. Fourth, token savings are confirmed only on math: in the parkour run the model spent 4.8 times more tokens than GPT-6 Luna, in the Bowser scene twice as many as Opus 5.5, and in chess usage barely changed.

## Why "Flash" has 763 billion parameters

According to the model card, it has 763 billion parameters: 552 billion in the main part and 196 billion in Engram - n-gram memory tables the model looks up by token. For each token only 8 billion parameters work when reading the prompt and 16 billion when generating. That is why "Flash" here is about speed, not size.

Another detail explains why DeepSeek can serve a 1-million-token context cheaply: the KV cache takes 890 bytes per token, roughly four times less than in V4 Flash. The license is MIT and the weights can be downloaded, but this is server class: you cannot run such a model on a home computer.

## Weak spots

- **Hallucinations.** The most frequent complaint about past versions in both Reddit threads. Whether this update fixed it, nobody has shown yet.
- **Invented tool calls.** The sference measurement above: when there is no suitable function, the model would rather invent a call than say "I can't".
- **The agent does what was forbidden.** One user writes that V4.1 Flash read files with secrets despite a ban, and the keys had to be reissued. In the replies people advise writing "read only in this folder" instead of "don't read secrets", and keeping secrets out of the agent's reach: in a separate container, a secrets manager or under test keys.
- **Tied to its own harness.** The model was tuned specifically for DeepSeek's own Harness and two tools from partner Tencent. The MIT license lets you run it anywhere, but in another environment the result may differ from what DeepSeek shows.
- **The model is updated under the same name.** DeepSeek recommends calling the model as`deepseek-flash` . The old names`deepseek-v4-flash` and`deepseek-v4-flash-vision-exp` are still accepted, but they are now answered by 1017. If you compared models using old runs, repeat them.

## Search for agents: free until October 31

On the same day DeepSeek opened Search and Fetch in the API to everyone: search on its own engine and page loading. The price is 5 yuan per 1,000 calls of each (the price list shows $0.75). Calls made through DeepSeek Harness, Claude Code or Codex are not billed for now. They become paid from 00:00 Beijing time on November 1, which is 16:00 UTC on October 31.

## What it costs in GPTunneL

The cost of a task is the price of a token times the number of tokens the model spends. The first is fixed with us, the second depends on your task. Here are the prices in our catalog per 1,000 tokens for the models from the chart:

| Model | Input, $ | Output, $ | Output compared with DeepSeek V4.1 Flash | 
|---|---|---|---|
| DeepSeek V4.1 Flash | 0.0006 | 0.0024 | baseline | 
| GLM 5.3 Flash | 0.0003 | 0.001 | 2.4 times cheaper | 
| GPT-6 Luna | 0.0004 | 0.002 | 17% cheaper | 
| Gemini 3.8 Flash | 0.003 | 0.015 | 6.3 times more expensive | 
| Kimi K3 | 0.006 | 0.03 | 12.5 times more expensive | 
| GPT-6.1 Sol | 0.008 | 0.04 | 16.7 times more expensive | 
| Claude Opus 5.5 | 0.012 | 0.06 | 25 times more expensive | 
| GPT-6 Astra | 0.02 | 0.1 | 41.7 times more expensive | 

A request of 2,000 tokens with a 1,000-token answer costs about $0.0036 in DeepSeek V4.1 Flash, $0.056 in GPT-6.1 Sol and $0.084 in Opus 5.5. This assumes the same answer length. In practice models spend different numbers of tokens, and DeepSeek reasons for a long time: the September version, according to Artificial Analysis, spent 250 million output tokens on their index. For 1017, answers are 21-30% shorter according to sference, but it is still better to compare the price of a completed task.

## When to pick DeepSeek V4.1 Flash and when not

**A good fit:**

- **An agent triages tickets in a repository or automates routine work.** This is the scenario where 1017 is strongest in both quality and cost per task. Run 10-20 of your own tasks and compare the cost of a completed task, not the price of a token.
- **You already work with V4.1 Flash through the API.** Try lowering`reasoning_effort` , for example to 25: in the sference measurement at this value the model solved all the math tasks, and it cost about 20% less than with the default setting. Check on your own tasks whether accuracy holds.

**Better to pick another model:**

- **You need exact answers to hard questions without tools.** DeepSeek is weaker here: 42.6 against Opus 5's 56.3 on HLE, and Opus 5.5's figures are higher still. If the cost of an error for you is higher than the difference in request price ($0.0036 versus $0.084 in the example above), take Opus 5.5.
- **You need a very large number of simple answers for pennies.** GPT-6 Luna and GLM 5.3 Flash are cheaper per token, but on the DeepSWE chart they have about 61-64% against 77%. If the tasks are simple, the quality gap may not show up.

**Check before switching:** a scenario with parallel calls of many different tools. 1017 passed it on 3 of 12 in the sference measurement. Kimi K3 and GLM 5.3 did no better there, so switching to them will not solve this problem.

Separately, for those who follow open weights: on October 16 the Chinese authorities published a draft of export rules for AI technologies, in which open releases get a simplified filing procedure, and DeepSeek is not mentioned in the document (MOFCOM clarification, the draft is still under discussion).

## Where to try it

DeepSeek V4.1 Flash is in the GPTunneL catalog, and all models of the family are collected on the DeepSeek page. The easiest way to compare is on one and the same task: send it to V4.1 Flash, GPT-6.1 Sol and Opus 5.5 and see who handles it and what it costs. We covered how DeepSeek Flash differs from Pro separately, and how the agents this model is built for work in the article on AI agents.

## Sources

- DeepSeek announcement on X, 17.10.2026 - the release's promises and demos
- DeepSeek API changelog, entry of 17.10.2026
- DeepSeek-V4.1-Flash-1017 card - the full table of tests
- DeepSeek-V4.1-Flash card and technical report - how the model is built
- DeepSeek documentation: models and Search & Fetch - model names, the free search period
- sference: 1017 against the previous version, 17.10.2026
- ChessBench - a rating of language models at chess, snapshot of 17.10.2026, 17:16 UTC
- Bug Hunt Bench - a bug-finding rating (Paweł Huryn), table of 17.10.2026
- The Decoder: release review - on training with self-scoring
- Discussions on r/DeepSeek and r/LocalLLaMA
