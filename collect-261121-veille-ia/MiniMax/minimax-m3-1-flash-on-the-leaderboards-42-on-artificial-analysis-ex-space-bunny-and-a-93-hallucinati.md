---
id: collect-261121-veille-ia/minimax/minimax-m3-1-flash-on-the-leaderboards-42-on-artificial-analysis-ex-space-bunny-and-a-93-hallucinati
title: "MiniMax M3.1 Flash on the Leaderboards: 42 on Artificial Analysis, Ex-'Space Bunny', and a 93% Hallucination Rate Â· Terapep"
domain: minimax
role: reference
task: article
actors: ["MiniMax"]
dates: ["2026-10-19"]
keywords: ["flash", "minimax", "artificial", "analysis", "openrouter", "space", "bunny", "tokens"]
source: https://www.terapep.com/blog/minimax-m3-1-flash-leaderboards-artificial-analysis-space-bunny-2026/
source_anchor: ""
source_lines: [1, 136]
section: "MiniMax"
---
# MiniMax M3.1 Flash on the Leaderboards: 42 on Artificial Analysis, Ex-"Space Bunny", and a 93% Hallucination Rate

MiniMax M3.1 Flash (19 Oct 2026) scores 42 on Artificial Analysis at $0.22 per task, was the stealth "Space Bunny" that topped OpenRouter for 10 days, and bluffs on 93% of questions it misses. Board by board, independent vs company numbers.

**TL;DR** â MiniMax released **M3.1 Flash** on 19 October 2026. On Artificial Analysis' independent Intelligence Index it scores **42**, up 13 points from M3 (29). That ties GLM-5.3 Flash, edges Gemini 3.8 Flash (41) and trails DeepSeek V4.1 Flash-1017 (45). It costs **$0.15 in / $0.60 out per million tokens**, about $0.22 per index task. It turns out to be the anonymous **"Space Bunny"** model that sat at No. 1 on OpenRouter for 10 days. The catch: Artificial Analysis measured a **93% hallucination rate** on its knowledge test, meaning it almost never says "I don't know". Open weights are promised for 23 October.

This is a follow-up to our LLM leaderboards roundup for October 2026, focused on one model: where M3.1 Flash actually lands, board by board, and which numbers are independent.

## The model in one table

|  | MiniMax M3.1 Flash | 
|---|---|
| Released | 19 Oct 2026 (a preview had appeared in MiniMax Code from 27 Sep) | 
| Context | 1 million tokens | 
| Input / output | Text and images in, text out | 
| Price (first-party API) | $0.15 / 1M input, $0.60 / 1M output, $0.015 / 1M cached input | 
| Speed (MiniMax claim) | 200+ tokens/s, median above 170 tokens/s, "nearly 3Ã faster" than M3 | 
| Speed (Artificial Analysis) | 120.1 tokens/s measured | 
| Weights | Proprietary at launch; open weights planned for 23 Oct 2026, licence not yet published | 
| Where to use it | MiniMax API, M Plan subscription, MiniMax Code app, OpenRouter (free for the first 72 hours) | 

## Leaderboard 1: Artificial Analysis Intelligence Index (independent)

This is the most useful independent number so far. Artificial Analysis runs the same eval suite on every model at maximum reasoning effort.

| Model | Intelligence Index | Cost per index task | 
|---|---|---|
| DeepSeek V4.1 Flash-1017 | **45** | ~$0.26 | 
| **MiniMax M3.1 Flash** | **42** | **~$0.22** | 
| GLM-5.3 Flash | 42 | ~$0.28 | 
| Gemini 3.8 Flash | 41 | ~$1.24 | 
| MiniMax M3 (previous) | 29 | ~$0.51 | 

- Artificial Analysis ranks it **#4 of 173 models in its class** . The class median is 13.
- It is the **cheapest per task** of the four "Flash-tier" leaders above.
- It still trails the top Anthropic and OpenAI models on the overall index. This is a budget-tier leader, not a frontier leader.
- **Verbosity:** it used about 180â189 million output tokens to run the index. That is less than GLM-5.3 Flash (~324M) and Gemini 3.8 Flash (~334M), but well above the median of 99M. Artificial Analysis labels it "very verbose".

Selected sub-scores reported from Artificial Analysis:

| Eval | M3.1 Flash | M3 | DeepSeek V4.1 Flash-1017 | GLM-5.3 Flash | Gemini 3.8 Flash | 
|---|---|---|---|---|---|
| Humanity's Last Exam | **48%** | 39% | 43% | 40% | 48% | 
| CritPt (research physics) | 22% | 4% | **24%** | 15% | 18% | 
| AA-Omniscience, % correct | **56%** | â | 43% | 28% | â | 

## Leaderboard 2: OpenRouter, as "Space Bunny"

Before launch, an anonymous model called **Space Bunny** ran on OpenRouter. MiniMax has now confirmed it was an early version of M3.1 Flash.

- OpenRouter says it was the **No. 1 model on its platform for 10 straight days** .
- It processed **56.7 trillion tokens** according to OpenRouter, the most of any stealth model OpenRouter has hosted. MiniMax's own figure is "over 200 trillion". The two numbers have not been reconciled, and MiniMax's may count traffic outside OpenRouter.
- It left OpenRouter on 5 October and came back under its real name on 19 October.
- Reported improvements during the stealth run:
  - time to first token 1.95 s â 1.22 s
  - throughput 70 â 100 tokens/s
  - error rate 1.4% â 0.7%

OpenRouter usage rank is a popularity board, not a quality board. A free or cheap model with good tool use climbs fast. Still, ten days at No. 1 while anonymous means developers picked it on results rather than on brand.

## Leaderboard 3: MiniMax's own benchmark table (company-reported)

These come from MiniMax's launch materials. Treat them as claims until third parties reproduce them; evaluation settings differ between labs.

| Benchmark | M3.1 Flash | Compared with | 
|---|---|---|
| Automation-Bench v1.0.6 | **62.2** | DeepSeek V4.1 Flash-1017 60.5; GPT-5.6 Sol and Claude Opus 5 lower | 
| BankerToolBench | 76.2 | Claude Opus 5 76.2 (tie) | 
| DeepSWE v1.1 (coding) | 73.7% | DeepSeek V4.1 Flash-1017 77%, Claude Opus 5 74%, GPT-5.6 Sol 73% | 
| OSWorld 2.0 (computer use) | 61.7 | MiniMax says first among Chinese models | 
| Agents' Last Exam | 29.6 | MiniMax says first among Chinese models | 
| CyberGym | 82.2 | â | 
| BabyVision | 91 | â | 
| Terminal-Bench 4.0 | 38.1 | its weakest published result | 

Small inconsistencies exist between MiniMax's launch post and what OpenRouter relayed:

| Benchmark | Launch post | OpenRouter | 
|---|---|---|
| DeepSWE | 74% | 73.67% | 
| Agents' Last Exam | 30% | 29.6 | 
| AutomationBench-AA (a different variant from Automation-Bench) | 62% | 72.5% | 

Read these as rounding and variant differences, not as contradictions. They are still a reason to wait for independent runs.

## The number to watch: 93% hallucination rate

Artificial Analysis' **AA-Omniscience** test asks hard factual questions and rewards a model for saying "I don't know" when it doesn't know.

- **Correct answers: 56%.** That is the best of its peer group: DeepSeek V4.1 Flash-1017 43%, GLM-5.3 Flash 28%.
- **Hallucination rate: 93%.** This is the share of*non-correct* responses that were wrong answers rather than abstentions. It does**not** mean 93% of all answers were wrong. It means the model attempted nearly every question and almost never declined.
- Overall AA-Omniscience score: 16.

**What this means in practice:** M3.1 Flash knows more than its rivals but bluffs when it doesn't know. For agent pipelines and coding with tests, that matters less, because the environment checks the answer. For anything knowledge-heavy and unchecked (research summaries, customer answers, legal or medical text), you need retrieval, verification or a human in the loop.

## Price: where it sits

| Model | Input $/1M | Output $/1M | 
|---|---|---|
| GPT-6 Luna | 0.10 | 0.50 | 
| **MiniMax M3.1 Flash** | **0.15** | **0.60** | 
| DeepSeek V4.1 Flash-1017 (off-peak) | 0.15 | 0.60 | 
| MiniMax M3 | 0.30 | 1.20 | 
| DeepSeek V4.1 Flash-1017 (peak) | 0.30 | 1.20 | 
| Gemini 3.8 Flash (to 31 Dec 2026) | 0.75 | 3.75 | 
| GLM-5.3 | 1.40 | 4.40 | 
| Claude Opus 5.5 | 4.00 | 20.00 | 

M3.1 Flash costs half as much as M3 per token. MiniMax says agent-task costs are 70% lower overall, because it also uses fewer steps. Artificial Analysis' per-task figure fell from about $0.51 to $0.22, a 57% drop. That independent number is the better one to quote.

## Who should try it

- **Agent and tool-use workloads on a budget:** yes. This is where its independent score, price and OpenRouter track record line up.
- **Coding:** competitive. DeepSeek V4.1 Flash-1017 is slightly ahead on MiniMax's own DeepSWE table, and M3.1 Flash is weak on Terminal-Bench.
- **Factual Q&A without verification:** no, not until the hallucination behaviour improves.
- **Self-hosting:** wait for the 23 October weights and the licence text.

MiniMax says a larger **Pro** model is coming, with no date announced.

## FAQ

**Is MiniMax M3.1 Flash better than DeepSeek V4.1 Flash?**
Close, not better overall. DeepSeek scores 45 to M3.1 Flash's 42 on Artificial Analysis' index. M3.1 Flash is cheaper per task and wins on HLE and factual recall, but loses on CritPt and on MiniMax's own coding table.

**What was "Space Bunny"?**
An anonymous model on OpenRouter that was No. 1 there for 10 days. MiniMax confirmed it was an early M3.1 Flash.

**Is it open source?**
Not yet. MiniMax says open weights will be released on 23 October 2026. The licence has not been published.

**What does the 93% hallucination rate mean?**
When it does not get a question right, it almost always guesses wrong instead of saying it doesn't know. It still has the highest share of correct answers among its peers on that test.

*Sources: Artificial Analysis model page, VentureBeat, The Decoder, Unite.AI, MarkTechPost. Benchmark figures marked company-reported have not been independently reproduced.*

â Back to all posts
