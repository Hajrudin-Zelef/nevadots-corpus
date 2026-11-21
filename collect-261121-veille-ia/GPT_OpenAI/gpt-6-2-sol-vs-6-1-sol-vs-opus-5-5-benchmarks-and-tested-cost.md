---
id: collect-261121-veille-ia/gpt-openai/gpt-6-2-sol-vs-6-1-sol-vs-opus-5-5-benchmarks-and-tested-cost
title: "GPT-6.2 Sol vs 6.1 Sol vs Opus 5.5: Benchmarks and Tested Cost"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: ["2026-11-17"]
keywords: ["gpt-6", "opus", "agent", "medium", "same", "runs", "effort", "codex"]
source: https://blog.laozhang.ai/en/posts/gpt-6-2-sol-vs-gpt-6-1-sol-vs-claude-opus-5-5
source_anchor: ""
source_lines: [1, 136]
section: "GPT_OpenAI"
---
## On this page

Switch from GPT-6.1 Sol to GPT-6.2 Sol for API calls and single-request work, and start at `medium` effort. OpenAI released `gpt-6.2-sol` on November 17, 2026 at the same Standard price as `gpt-6.1-sol`, $2 input and $10 output per million tokens, and it scores higher at every reasoning effort on the Artificial Analysis Intelligence Index: 53 against 46 at medium, 57 against 55 at max. When the same 12 small coding and reasoning tasks went to both models on November 18, 2026, both passed all 12, and GPT-6.2 Sol needed 59.7 seconds of call time where GPT-6.1 Sol needed 259.4.

The exception is long agent runs in Codex. On the Artificial Analysis Coding Agent Index, Codex with GPT-6.2 Sol peaks at 49.8, at `xhigh` rather than `max`, which is below the 52.2 that Codex with GPT-6.1 Sol reaches at `max`, although each task costs 40% less. That gap is where the "6.2 Sol is worse than 6.1" posts come from. If your Codex workflow only passes with 6.1 Sol at `max`, run your own tasks on 6.2 Sol at `xhigh` before you move it.

Claude Opus 5.5 (`claude-opus-5-5`) is still the stronger model at the top, 59 against 57 on the same index and 58.2 against 49.8 as a coding agent, but it charges twice as much per token and about six times as much per task at an equal score. Work you now give Opus 5.5 at medium or high can move to GPT-6.2 Sol at `medium`, which scores 53 against Opus high's 52; keep Opus for long, hard agent sessions where the last few points decide whether the job lands. When you switch a request to 6.2 Sol, change three things first: put reusable instructions in a developer message, send tool calls through the Responses API, and use `low` wherever shared settings send effort `none`.

## Which to use: GPT-6.2 Sol for most work, Opus 5.5 for long agent runs

Choose by how long the job runs and what a missed task costs you. For single requests, API pipelines and short agent tasks, GPT-6.2 Sol at `medium` is the new default: it beats GPT-6.1 Sol at every effort for the same token price, and it beats Claude Opus 5.5 at `high` on the Artificial Analysis index for about an eighth of the cost per task. The two cases where you should keep the older model or pay more are Codex runs that only pass with 6.1 Sol at `max`, and long agent sessions where Opus 5.5's lead on the coding-agent benchmark is worth roughly 14 times the cost per task.

| Your work | Use | Start at effort | Change course when | 
|---|---|---|---|
| API calls, extraction, single-turn coding with tests you can run | `gpt-6.2-sol` | `medium` | Quality falls short; `high` scores the same as 6.1 Sol at`max` | 
| Codex or agent runs you now do on 6.1 Sol at `medium` to`xhigh` | `gpt-6.2-sol` | The same effort you use today | Your acceptance rate drops; then try `xhigh` , its best agent setting | 
| Codex runs that only pass with 6.1 Sol at `max` | Keep `gpt-6.1-sol` while you test 6.2 Sol | `xhigh` on 6.2 Sol | 6.2 Sol passes your tasks; it then costs 40% less per task | 
| Work you now send to Opus 5.5 at `medium` or`high` | `gpt-6.2-sol` | `medium` | Reviews find more misses than on Opus | 
| Long, multi-file agent coding where a failed run is expensive | `claude-opus-5-5` in Claude Code | `high` or above | The cost per accepted task stops being worth the higher score | 

The middle rows rest on one gap in the evidence: Artificial Analysis lists only one GPT-6.1 Sol configuration in its coding-agent results, at `max`, so its results don't show whether 6.2 Sol beats 6.1 Sol as an agent at `medium` or `high`. The general index says it does by 5 to 7 points, which is why the table moves those runs at the same effort and leaves the `max` runs for a test. If neither Sol is strong enough, GPT-6.2 Astra, released the same day, costs $8 input and $40 output per million tokens on the OpenAI API pricing page, four times Sol's rate.

## GPT-6.2 Sol vs 6.1 Sol pricing: identical $2 / $10, Opus 5.5 is 2x

GPT-6.2 Sol and GPT-6.1 Sol cost the same on OpenAI's Standard, Batch and Fast tiers, so switching doesn't change your token rate; any saving comes from 6.2 Sol using fewer tokens and less time per task. Claude Opus 5.5 costs exactly twice as much per token on input, output and cached reads. The rates below are per million tokens as of November 18, 2026, from the OpenAI API pricing page, the GPT-6.2 Sol model page and Anthropic's Claude pricing, which the Artificial Analysis comparison table still shows on November 18.

| Rate per 1M tokens | GPT-6.2 Sol | GPT-6.1 Sol | Claude Opus 5.5 | 
|---|---|---|---|
| Input / cached input / output | $2.00 / $0.10 / $10.00 | $2.00 / $0.10 / $10.00 | $4.00 / $0.20 / $20.00 | 
| Cache writes | $2.50 for 30 minutes, $5.00 for 12 hours | $2.50 for 30 minutes, $5.00 for 12 hours | $5 for 5 minutes, $8 for 1 hour | 
| Batch input / output | $1.00 / $5.00 | $1.00 / $5.00 | $2.00 / $10.00 | 

Two smaller differences separate the Sols. OpenAI's Ultrafast tier costs $6 input and $30 output for 6.2 Sol, 3x Standard through February 28, 2027 and 6x from March 1, 2027, against $12 and $60 for 6.1 Sol. GPT-6.2 Sol is eligible for OpenAI's discounted background requests, 30% off Standard for a 2-hour completion window and 40% off for an 8-hour window, and 6.1 Sol isn't on that list. On long prompts, the GPT side has a step that Anthropic doesn't: above 272K input tokens, 6.2 Sol charges the whole request at $4 input and $15 output, while Opus 5.5 keeps the same rate across its full 1M-token context.

GPT-6.1 Sol is still on OpenAI's price list as of November 18, 2026, and the OpenAI API changelog gives no retirement date for it. Headlines from the GitHub Blog and 9to5Mac say ChatGPT, Codex and GitHub are moving to the 6.2 models, but whether 6.1 Sol stays selectable inside Codex and ChatGPT isn't stated in OpenAI's API docs.

## Scores at each effort: GPT-6.2 Sol beats 6.1 Sol by 2–7 points

On the Artificial Analysis Intelligence Index v5.0.1, GPT-6.2 Sol scores higher than GPT-6.1 Sol at every reasoning effort: by 6 points at `low`, 7 at `medium`, 5 at `high`, 4 at `xhigh` and 2 at `max`. The most useful pair is 6.2 Sol at `high`, which matches 6.1 Sol at `max` with 55 points for 44% of the cost per task ($0.56 against $1.27). Each cell below is the index score and the cost of one index task.

| Effort | GPT-6.2 Sol | GPT-6.1 Sol | Claude Opus 5.5 | 
|---|---|---|---|
| `low` | 41 · $0.18 | 35 · $0.16 | 35 · $0.98 | 
| `medium` | 53 · $0.41 | 46 · $0.36 | 48 · $2.45 | 
| `high` | 55 · $0.56 | 50 · $0.59 | 52 · $3.46 | 
| `xhigh` | 56 · $0.65 | 52 · $0.76 | 57 · $6.05 | 
| `max` | 57 · $1.01 | 55 · $1.27 | 59 · $10.04 | 

The 6.2 Sol and Opus 5.5 figures come from the Artificial Analysis Claude Opus 5.5 vs GPT-6.2 Sol comparison as of November 18, 2026. The 6.1 Sol figures are the same index as of November 4, 2026, with the `max` figure as of November 11, because Artificial Analysis now marks the 6.1 Sol release as "deprecated" in its catalogue and its 6.1 Sol comparison pages are offline. That label is Artificial Analysis's own; OpenAI hasn't announced a retirement. Artificial Analysis also labels its Opus runs "Default Fallback", which describes its test setup, not an Anthropic setting.

Read down the 6.2 Sol column and the cost climbs slowly until `max`: from `medium` to `xhigh` the score rises 3 points for $0.24 more per task, and `max` adds 1 more point for another $0.36. Against 6.1 Sol, 6.2 Sol at `medium` (53) already beats 6.1 Sol at `xhigh` (52) for $0.41 instead of $0.76. So if you raised 6.1 Sol's effort to get quality, you can often drop one or two levels on 6.2 Sol and pay less per task.

## "6.2 Sol is worse than 6.1 at coding": only against 6.1 Sol max in Codex

Partly true, and only for long agent runs. On the Artificial Analysis Coding Agent Index, the best Codex score with GPT-6.2 Sol is 49.8 at `xhigh`, 2.4 points below Codex with GPT-6.1 Sol at `max` (52.2). That is the "52 down to 49" figure quoted in launch-day posts. The same table shows what the drop buys: 6.2 Sol at `xhigh` finishes a task for $0.90 in 12.6 minutes, against $1.51 and 21.9 minutes for 6.1 Sol at `max`, so each task costs 40% less and takes 42% less time.

| Agent and model | Effort | Coding Agent Index | Cost per task | Time per task | 
|---|---|---|---|---|
| Codex, GPT-6.2 Sol | `low` | 37.2 | $0.33 | 7.2 min | 
| Codex, GPT-6.2 Sol | `medium` | 45.4 | $0.57 | 8.4 min | 
| Codex, GPT-6.2 Sol | `high` | 47.5 | $0.76 | 10.6 min | 
| Codex, GPT-6.2 Sol | `xhigh` | 49.8 | $0.90 | 12.6 min | 
| Codex, GPT-6.2 Sol | `max` | 48.6 | $1.22 | 17.5 min | 
| Codex, GPT-6.1 Sol | `max` | 52.2 | $1.51 | 21.9 min | 
| Claude Code, Claude Opus 5.5 | `max` | 58.2 | $12.61 | 65.3 min | 

The index runs each model inside a real coding agent, Codex CLI 0.166.0 for the GPT models and Claude Code for Opus, on FrontierCode 2.0, SWE-Atlas QnA and Terminal-Bench v4, so it measures coding tasks completed through an agent harness. Two things in it matter when you set up Codex. GPT-6.2 Sol scores lower at `max` than at `xhigh` while costing more, so `max` is not the setting to pick for the top agent score. And the broader Artificial Analysis Intelligence Index moved the other way, with 6.2 Sol up 2 points over 6.1 Sol at `max`, which is why a thread on r/codex titled "Gpt 6.2 Astra is equivalent to 6 in coding, 6.2 Sol is worse than 6.1…" and a VentureBeat story headlined "'Essentially unusable': OpenAI launches GPT-6.2 Astra and Sol to negative reception from AI power users" sit next to reports of the same model being cheaper and faster. Both are describing real numbers: the broad index rose while the coding-agent index fell.

## Can GPT-6.2 Sol low or medium beat Opus 5.5 medium?

At `medium`, yes; at `low`, no, on the Artificial Analysis Intelligence Index as of November 18, 2026. GPT-6.2 Sol at `low` scores 41 against 48 for Claude Opus 5.5 at `medium`, but 6.2 Sol at `medium` scores 53, above Opus at both `medium` (48) and `high` (52). It also takes 3.1 minutes per index task against 6.8 minutes for Opus at `medium` and 9.3 minutes at `high`. Opus only pulls level at `xhigh`, where it ties 6.2 Sol at `max` on 57 in 15.6 minutes against 7.9, and it moves ahead only at `max`, with 59.

Where the remaining Opus lead sits matters for the choice. At `max` against `max`, the two are level on Terminal-Bench 4.0 (60.1% for 6.2 Sol against 59.6%), while Opus leads on FrontierCode-AA 2.0 (46.4% against 44.4%) and on the hard-knowledge test HLE-Diamond (67.9% against 64.0%), per the same Artificial Analysis comparison. Its biggest lead is on agentic coding: 58.2 against 49.8 on the coding-agent index above, where Opus spends 65.3 minutes per task against 12.6 for 6.2 Sol at `xhigh`. So Opus 5.5 keeps an edge on the hardest code and on long agentic coding tasks, and has none on terminal tasks.

For how the same choice looked against the previous Sol models, see Claude Opus 5.5 vs GPT-6.1 Sol and GPT-6 Sol: When Opus Pays Off. If you don't have Opus 5.5 yet, How to Access Claude Opus 5.5: 6 Ways In, Which Free Routes Work lists where it is available.

## Is GPT-6.2 Sol 10x cheaper than Opus 5.5? 2x per token, 3–14x per task

Per token, Claude Opus 5.5 costs exactly twice GPT-6.2 Sol. The "10x" in launch posts is a per-task figure, and whether it holds depends on which settings you put side by side: Opus spends more tokens per task as well as paying more per token. The multiplier ranges from about 3x on a long-log task to 14x on the coding-agent benchmark, and Opus scores higher in only two of these six pairings.

| Pairing | Opus 5.5 cost | GPT-6.2 Sol cost | Opus ÷ Sol | Score difference | 
|---|---|---|---|---|
| Intelligence Index, Opus `high` vs Sol`medium` | $3.46 | $0.41 | 8.4x | Sol 1 point higher | 
| Intelligence Index, equal score 57 (Opus `xhigh` vs Sol`max` ) | $6.05 | $1.01 | 6.0x | Tied | 
| Intelligence Index, both at `max` | $10.04 | $1.01 | 9.9x | Opus 2 points higher | 
| Coding Agent Index, Opus `max` vs Sol`xhigh` | $12.61 | $0.90 | 14.0x | Opus 8.4 points higher | 
| 12 test tasks, Opus `medium` vs Sol default (`medium` ) | $0.3699 | $0.0721 | 5.1x | Both 12 of 12 | 
| 146 KB log count, Opus `medium` vs Sol default | $0.4681 | $0.1581 | 3.0x | Both all correct | 

The multipliers are the first cost divided by the second; for example $10.04 ÷ $1.01 = 9.94 for the `max` pairing, which is the one case where "10x" is accurate, and Opus gets 2 more points for it. Community reports land in the same range and should be read as single examples. A Hacker News commenter on the launch thread wrote that Sol "went from $2.6 (V6.1) to $1.2 (V6.2)" with "similar accuracy in both", and still called Opus 5.5 "just the better model"; an X user reported one 3D scene prompt costing $2.07 and 30 minutes 47 seconds on 6.2 Sol against $31.85 and 2 hours 45 minutes on Opus 5.5, about 15x on cost.

## Same 12 tasks on November 18, 2026: all passed, 6.2 Sol about 4x faster

On November 18, 2026, the same 12 single-turn tasks went to GPT-6.2 Sol, GPT-6.1 Sol and Claude Opus 5.5 through one API gateway, and every configuration passed all 12. Because all three passed, the test separates them on time, tokens and cost, not on what they can solve. Each task had hidden tests or a single exact answer: a word problem, merging intervals, parsing durations, counting top words, a long-document question, an expression evaluator, a CSV line parser, a digit-sum problem, comparing and matching semantic versions, a calculation from a set of notes, and a standard-library question.

| Configuration | Passed | Output tokens | Call time, 12 tasks | Cost as billed | Cost with no input cached | 
|---|---|---|---|---|---|
| GPT-6.2 Sol, default ( `medium` ) | 12/12 | 4,220 | 59.7 s | $0.0721 | $0.0862 | 
| GPT-6.2 Sol, `high` | 12/12 | 6,765 | 76.6 s | $0.1217 | $0.1116 | 
| GPT-6.1 Sol, default ( `medium` ) | 12/12 | 6,167 | 259.4 s | $0.1158 | $0.1056 | 
| GPT-6.1 Sol, `high` | 12/12 | 10,380 | 363.1 s | $0.1579 | $0.1477 | 
| Claude Opus 5.5, `medium` | 12/12 | 11,833 | 130.5 s | $0.3699 | $0.3699 | 
| Claude Opus 5.5, `high` | 12/12 | 13,674 | 159.3 s | $0.4067 | $0.4067 | 

At default effort, GPT-6.2 Sol wrote 0.68x the output tokens of GPT-6.1 Sol and took 0.23x the time, so it finished the set about 4.3 times faster (259.4 ÷ 59.7). With no input cached it cost 0.82x as much ($0.0862 against $0.1056), which comes entirely from the shorter output, since the token rates are the same. The as-billed column moves both ways because it includes cache-write charges ($2.50 per million tokens against $2.00 for plain input) and one cache hit on 6.2 Sol, where the long-document prompt had just been sent at `high`. Opus 5.5 at `medium` used 2.80 times the output tokens of 6.2 Sol at default, 2.19 times the time and 5.13 times the money as billed (4.29 times with no input cached). If 6.1 Sol has felt slow in your own work, GPT-6.1 Sol Slow in Codex or API? Causes and Tested Speed Fixes covers why and which settings help.

A second task asked each model for 7 exact numbers from a 146 KB, 1,500-line log, and every model got all 7 right. GPT-6.2 Sol at default took 13.6 seconds and $0.1581 on November 18; GPT-6.1 Sol at default took 45.7 seconds and $0.1654 on November 16; Claude Opus 5.5 at `medium` took 57.6 seconds and $0.4681 on November 11. Opus counted the same log as 78,537 input tokens against 55,843 for both Sols, 1.41 times as many, so part of its per-task premium is paid before it starts reasoning. At 56K tokens the log is far below 272K, so it says nothing about 6.2 Sol's long-context price step.

The limits travel with these numbers. Each cell is one sample, and the tasks were easy enough for every model to pass. GPT requests went to `/v1/chat/completions`, where "default" means no `reasoning_effort` was sent and the model ran at `medium`. The Opus rows for 8 of the 12 tasks are from November 11, 2026 (streamed) and 4 are from November 18 (not streamed), and Opus at `high` passed the merge-intervals task only after its function and tests were split into two files at grading. Times are wall-clock time through the gateway, not vendor latency, and costs are list prices applied to the token usage each response returned. Every 6.2 Sol response reported its model as `gpt-6.2-sol-2026-11-17`, but OpenAI's docs list no dated snapshot for 6.2 Sol, so don't pin that name in production. The gateway was laozhang.ai, which serves `gpt-6.2-sol`, `gpt-6.1-sol` and `claude-opus-5-5` under one API key; its API documentation shows the request format if you want to rerun the same comparison on your own prompts.

Disclosure: laozhang.ai is our own service. Its prices are platform pricing, not an independent comparison.

## What to change when switching from GPT-6.1 Sol to 6.2 Sol

Change the model ID to `gpt-6.2-sol` and keep `medium` as the starting effort, then check the request details below, taken from OpenAI's GPT-6.2 migration guide as of November 18, 2026. Each one is independent, so fix whichever your integration uses.

- **Effort `none`.** The API rejects it for 6.2 Sol with HTTP 400 and the message "Unsupported value: 'none' is not supported with the 'gpt-6.2-sol' model. Supported values are: 'low', 'medium', 'high', 'xhigh', and 'max'." GPT-6.1 Sol didn't accept`none` either, so this only bites when shared settings or another model's config send it; OpenAI's guide says to use`low` if you sent`none` or`minimal` .
- **Reusable instructions.** For GPT-6.2 models the Responses API appends top-level`instructions` after`input` , so a long system prompt there no longer forms a cacheable prefix. Put reusable instructions in a`developer` message as the first item in`input` , and keep top-level`instructions` for guidance that applies to one request. On November 18, 2026, a 9,000-token`instructions` block sent three times returned 0 cached tokens, both through laozhang.ai and directly from OpenAI.
- **Conversation history.** Starting with 6.2, the Responses API drops reasoning items after edits to earlier turns, after certain model changes including downgrades, and after a change of end user or organization. OpenAI warns that answer quality can drop and token use can rise, so don't rewrite earlier messages or bounce one conversation between 6.2 Sol and 6.1 Sol.
- **Tool calling.** OpenAI's guide says 6.2 Sol supports Chat Completions but tool calling requires the Responses API. Separately, Chat Completions requests with`reasoning_effort: "max"` failed through laozhang.ai on November 18, 2026, so send`max` requests through Responses.
- **Sampling parameters.** When migrating Chat Completions requests, omit`temperature` ,`top_p` ,`logprobs` and`top_logprobs` .

Disclosure: laozhang.ai is our own service. Its prices are platform pricing, not an independent comparison.

A Responses request that follows all of these looks like this:

In Codex, the matching change is to try `xhigh` before `max` for 6.2 Sol, since that is where its agent score peaks. If you are still moving an older integration from GPT-6 Sol, the earlier step is covered in GPT-6.1 Sol: Pricing, Access, and What Breaks When You Switch.

## ## Sources7

External pages this guide links to, in the order they appear. Last updated Nov 18, 2026.


## Sources7

External pages this guide links to, in the order they appear. Last updated Nov 18, 2026.
