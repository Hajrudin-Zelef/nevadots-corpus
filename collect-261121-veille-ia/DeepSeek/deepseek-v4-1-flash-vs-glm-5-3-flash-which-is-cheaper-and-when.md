---
id: collect-261121-veille-ia/deepseek/deepseek-v4-1-flash-vs-glm-5-3-flash-which-is-cheaper-and-when
title: "DeepSeek V4.1 Flash vs GLM-5.3-Flash: Which Is Cheaper, and When"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: []
keywords: ["flash", "glm-5", "deepseek", "input", "october", "checkpoint", "same", "models"]
source: https://blog.laozhang.ai/en/posts/deepseek-v4-1-flash-vs-glm-5-3-flash
source_anchor: ""
source_lines: [1, 139]
section: "DeepSeek"
---
## On this page

DeepSeek V4.1 Flash and GLM-5.3-Flash both list $0.15 per million uncached input tokens, both take a 1M-token context, and both ship MIT-licensed open weights, so the per-token table doesn't pick a winner. The clock does. As of October 25, 2026, DeepSeek charges double from 01:00 to 04:00 and 06:00 to 10:00 UTC on weekdays and the base rate the rest of the week, while Z.ai's price for GLM-5.3-Flash is flat. On Artificial Analysis's benchmark tasks that works out to about $0.13 per task for DeepSeek off-peak, $0.26 at peak, and $0.28 for GLM.

So send coding-agent loops with a long reused prefix, and short calls that don't need reasoning, to DeepSeek's `deepseek-flash`, which has served the V4.1-Flash-1017 checkpoint since October 17. That holds especially if your traffic runs during US business hours, all of which fall off-peak. Send work to `glm-5.3-flash` when it runs inside DeepSeek's peak window, where GLM usually costs about the same or less, when you need video or file input, or when your own side-by-side review prefers its output. GLM-5.3-Flash can't turn thinking off and is the slower of the two; if speed is the blocker, Z.ai's paid `glm-5.3-flashx` is the GLM-side fix.

Four of the measurements below were run firsthand on October 25, 2026: token counts from both official tokenizers on the same texts, a list-price recompute of five workloads, OpenRouter's public provider catalog, and the checkpoint sizes on Hugging Face. No same-prompt quality or speed test of the two models has been run. The quality and speed figures below come from Artificial Analysis (AA), which tested both models the same way, and the last section shows how to run that trial on your own tasks.

## Which checkpoints you get: V4.1-Flash-1017 and GLM-5.3-Flash

`deepseek-flash` on DeepSeek's API is DeepSeek-V4.1-Flash-1017, and "1017" is the checkpoint date, October 17, 2026, not a version number. DeepSeek released V4.1 Flash on September 10 and swapped in the 1017 checkpoint on October 17 without changing the API name or price, describing it in its API change log as faster at completing tasks, with shorter outputs. The older names `deepseek-v4-flash` and `deepseek-v4-flash-vision-exp` still work, but the DeepSeek API pricing page says those models are retired and their requests are served by 1017 at the Flash price. Routers behave differently: OpenRouter's catalog on October 25 still listed `deepseek/deepseek-v4.1-flash` (the September checkpoint) and `deepseek/deepseek-v4-flash-0731` as separate models next to `deepseek/deepseek-v4.1-flash-1017`, so pin the `-1017` ID there.

GLM-5.3-Flash (`glm-5.3-flash`) launched in August 2026. Z.ai's GLM-5.3-Flash model guide also lists `glm-5.3-flashx`, a faster paid variant, and `glm-5.3-flashx-legal`, which is offered to enterprise legal customers by application. "Flash" doesn't mean small on either side. DeepSeek describes V4.1 Flash as a 552B-parameter mixture-of-experts model with 8B active parameters for input and 16B for output, and Z.ai lists GLM-5.3-Flash at 320B total and 18B active. If your choice is actually within Z.ai's lineup, GLM-5.3 vs GLM-5.3-Flash: Which One Should You Test First? covers that decision.

Numbers from August and September often describe models you can no longer call. An AA index score of 39 or a DeepSWE score of 74.2 attributed to "DeepSeek V4.1 Flash" belongs to the September checkpoint. GLM-5.3-Flash prices of $0.075 input and $0.25 output were a launch promotion that ended on September 9, 2026, China time. The "57" that Z.ai's guide quotes for GLM-5.3-Flash on the Artificial Analysis index came from an earlier index version at that discounted price, so it isn't on the same scale as today's scores.

## List prices per 1M tokens: same $0.15 input, 10× apart on cache hits

Off-peak, the two models charge the same for uncached input and differ mainly on cached input, where DeepSeek is ten times cheaper. DeepSeek doubles every rate in its peak window, while the Z.ai pricing page shows one flat rate for GLM-5.3-Flash with no peak hours and no length tiers. Prices below are USD per million tokens as of October 25, 2026, and output includes thinking tokens on both.

| Per 1M tokens | DeepSeek V4.1 Flash, off-peak | DeepSeek V4.1 Flash, peak | GLM-5.3-Flash | GLM-5.3-FlashX | 
|---|---|---|---|---|
| Cached input | $0.003 | $0.006 | $0.03 | $0.075 | 
| Uncached input | $0.15 | $0.30 | $0.15 | $0.37 | 
| Output, including thinking | $0.60 | $1.20 | $0.50 | $1.25 | 

Z.ai lists cache storage for GLM-5.3-Flash as "Limited-time Free", so that line could start costing money later. DeepSeek's peak runs from 01:00 to 04:00 and 06:00 to 10:00 UTC, Monday through Friday; Chinese public holidays are off-peak all day, like weekends. That is 35 of the week's 168 hours. In US Eastern Daylight Time (UTC−4, until November 1, 2026) it means 9 p.m. to midnight Sunday through Thursday and 2 a.m. to 6 a.m. Monday through Friday, so a 9-to-5 workday anywhere in the continental US is off-peak, while European mornings and the working day in China fall inside it. DeepSeek API Peak Hours: Current Prices and When to Wait has the schedule for other time zones.

A million tokens is also about the same amount of text on both models. On October 25, 2026, both official tokenizers from Hugging Face were run with the `tokenizers` library (0.23.3, no chat template) over the same inputs. An English article of about 23,500 characters came to 6,759 tokens on DeepSeek and 6,808 on GLM, a TypeScript source file to 5,309 and 5,060, and a JSON config to 848 and 784. Chinese, Japanese, Russian and Spanish versions of the same article also stayed within 5%; only the Korean version moved more, with GLM needing 13% more tokens. The rates in the table therefore compare almost one to one, and the larger difference is how many thinking tokens each model writes. For other low-cost models on the same axes, see AI API Price Comparison: Cheapest Model by Input and Output Mix.

## Which is cheaper per job: DeepSeek off-peak, DeepSeek peak, or GLM

Off-peak, DeepSeek V4.1 Flash is cheaper on every job below except a cold long document, where the two tie. At peak the result depends on the job: GLM-5.3-Flash costs about three-quarters as much on a cached agent session and half as much on a cold document, while DeepSeek stays slightly ahead on AA's benchmark tasks and when GLM writes more. These are list-price recomputations from October 25, 2026, in USD, with thinking tokens counted as output. In the last two columns, a value above 1 means GLM costs more.

| Job | DeepSeek off-peak | DeepSeek peak | GLM-5.3-Flash | GLM ÷ DeepSeek off-peak | GLM ÷ DeepSeek peak | 
|---|---|---|---|---|---|
| One AA Intelligence Index task, measured token use | $0.13 | $0.26 | $0.28 | 2.15× | 1.08× | 
| Coding-agent session: 40 turns, 60K cached prefix, 4K new input and 3K output per turn | $0.1032 | $0.2064 | $0.1560 | 1.51× | 0.76× | 
| 1,000 short calls (1.5K in, 50-token answer), DeepSeek thinking off, GLM thinks 300 tokens per call | $0.255 | $0.510 | $0.400 | 1.57× | 0.78× | 
| Same 1,000 calls, GLM thinks 1,000 tokens per call | $0.255 | $0.510 | $0.750 | 2.94× | 1.47× | 
| One cold 400K-token document, 2K output | $0.0612 | $0.1224 | $0.0610 | 1.00× | 0.50× | 
| Coding-agent session, but GLM writes twice the output | $0.1032 | $0.2064 | $0.2160 | 2.09× | 1.05× | 

The first row comes from the Artificial Analysis comparison of DeepSeek V4.1 Flash 1017 (Max) and GLM-5.3-Flash. AA prices DeepSeek at its peak rates ($0.30 input, $1.20 output, $0.006 cache hit), which is why the same page calls GLM-5.3-Flash the cheaper model at a blended $0.10 per million against $0.18. Every DeepSeek rate halves off-peak, so its per-task cost halves too. The per-task figures already include each model's own token use: DeepSeek wrote 89K output tokens per task (59K of them reasoning) and GLM wrote 69K (47K).

The other rows are illustrations with assumed token counts, so swap in your own. Each one is cached tokens × cache-hit rate + uncached input × input rate + output × output rate, divided by a million. For the agent session on DeepSeek off-peak, that is 2.4M cached × $0.003 + 0.16M uncached × $0.15 + 0.12M output × $0.60 = $0.0072 + $0.024 + $0.072 = $0.1032. On GLM it is 2.4M × $0.03 + 0.16M × $0.15 + 0.12M × $0.50 = $0.072 + $0.024 + $0.06 = $0.156. The session assumes the whole prefix hits the cache on every turn and that both models write the same number of tokens unless the row says otherwise.

## Why the matching $0.15 price doesn't decide the bill

Four things move the bill more than the shared input price: when your calls run, how much of each prompt is a cache hit, whether the job needs reasoning at all, and how many tokens each model writes on your task. Each one can flip the cheaper model on its own.

Time of day only matters on the DeepSeek side. In the agent-session row, DeepSeek is the cheaper model until the weekday peak window opens and the more expensive one inside it, with no change to the prompt. If you can schedule batch jobs, moving them out of the peak halves DeepSeek's cost. Z.ai's API price has no equivalent; only its GLM Coding Plan subscription charges 50% of the usual points off-peak, including all weekend, and that is a quota discount rather than a per-token price.

Cache hits are where the gap is widest. GLM's $0.03 cache-hit rate is 10 times DeepSeek's off-peak rate and 5 times its peak rate, while uncached input is equal off-peak and GLM's output is slightly cheaper. A coding agent that resends a 60K-token repo context on every turn is mostly cache hits, which is why DeepSeek wins that job off-peak. A pipeline that sends a fresh document every time has almost no cache hits, so it pays GLM's flat $0.15 against DeepSeek's $0.15 or $0.30. Neither vendor publishes how long a cached prefix stays warm.

GLM-5.3-Flash can't run without thinking. Z.ai's thinking mode documentation says GLM-5.3 and GLM-5.3-Flash "use forced thinking and cannot be disabled", while DeepSeek turns thinking off with `{"thinking": {"type": "disabled"}}`. For short calls of 1,500 input tokens and a 50-token answer, that makes the break-even very low. Per 1,000 calls, DeepSeek without thinking costs 1.5M × $0.15 + 0.05M × $0.60 = $0.255 off-peak, and GLM costs 1.5M × $0.15 + (0.05M + thinking tokens) × $0.50. GLM therefore costs more than off-peak DeepSeek once it thinks more than 10 tokens per call, and more than peak DeepSeek ($0.51) once it passes 520 tokens per call.

How much each model writes depends on the task, and the two published measurements point in opposite directions. On AA's index, DeepSeek wrote more: 89K output tokens per task against GLM's 69K. On ToneBench, a writing benchmark scored per script, its author reported on X (@Whats_AI, around October 20, 2026) that GLM-5.3-Flash thought more than twice as long, 37K thinking tokens against 17K, and took 5.2 minutes per script against 1.3, at $0.022 against $0.014, while the two ranked 23rd and 24th of 134 configurations. If GLM writes twice DeepSeek's output in the agent session, it loses off-peak by 2.09× and roughly ties at peak. Count your own output and reasoning tokens before trusting any row in the table.

## Which is better for coding and overall quality?

On the one independent comparison that ran both models the same way, DeepSeek V4.1 Flash 1017 at max effort leads GLM-5.3-Flash narrowly overall and ties it on agentic coding. The Artificial Analysis comparison page gives an Intelligence Index (v4.3.2) of 45 against 42 as of October 25, 2026. On Terminal-Bench 4.0, the coding-agent test inside that index, they score 34% and 33%. DeepSeek's clearer leads are on AutomationBench-AA (67% vs 60%), SciCode (56% vs 52%) and the physics test CritPt (24% vs 15%). GLM is ahead on AA-Omniscience, which rewards correct answers and penalizes hallucinations (7 vs 5), and long-context reasoning on AA-LCR is close at 83% against 80%.

GLM's advantage is in how its writing reads. AA's PersonalityBench puts the chance that GLM-5.3-Flash's answer is judged the more natural-sounding of the two at 97%, and the more eloquent at 95%. User reports lean the same way for design and character work. In an r/opencode thread with more than 80 comments, the original poster added that "DS definitely has less latency than GLM. However, GLM might be better in Design skills", and the top reply preferred GLM "for my case". If your output is UI, copy or dialogue that a person will judge, weigh your own blind review above a three-point index gap.

Each vendor also publishes its own coding scores: DeepSeek on DeepSWE v1.1 and Terminal-Bench 4.0, and Z.ai on DeepSWE v1.1, AutomationBench and its in-house Z.ai Code Bench. Each ran them in its own harness. DeepSeek names Mini-SWE-V2 and Claude Code, and Z.ai's guide doesn't name one for DeepSWE, so subtracting one vendor's number from the other's doesn't give you a comparison.

## Which is faster: 195 vs 69 tokens per second on AA

DeepSeek is faster on every speed measure AA publishes, which it takes from each vendor's own API: 195 against 69 output tokens per second, 1.14 against 3.29 seconds to the first token, 11.4 against 32.3 seconds to the first answer token once thinking is included, and 357.6 against 831.8 seconds of generation per index task, roughly 6 minutes against 14. In an interactive coding agent, that gap shows up on every turn.

The GLM-side fix is `glm-5.3-flashx`, which Z.ai says runs at 200 tokens per second for about 2.5 times GLM-5.3-Flash's price ($0.37 input, $0.075 cached, $1.25 output per million). That speed is Z.ai's own figure, FlashX isn't available on the GLM Coding Plan yet, and on October 25 OpenRouter listed Z.ai as its only FlashX provider.

## Which model gets which job: decision rules

For coding-agent and short-call traffic that runs during US working hours, start with DeepSeek V4.1 Flash. GLM-5.3-Flash earns the job when it runs inside DeepSeek's peak window, needs video or file input, or produces writing your reviewers prefer. The table pairs each job shape with the condition that would flip it.

| Job | Start with | What flips it | 
|---|---|---|
| Coding agent with a long cached prefix, off-peak | DeepSeek ( `deepseek-flash` ) | Running at peak, where GLM costs about 0.76× as much, until it writes roughly 1.8× or more of DeepSeek's output tokens | 
| Many short calls that need no reasoning | DeepSeek with thinking disabled | At peak, GLM is cheaper only while it thinks fewer than about 520 tokens per call | 
| One long document, no cache reuse | Either off-peak; GLM at peak | GLM's forced thinking on a long answer adds output DeepSeek can skip | 
| Interactive work where latency matters | DeepSeek | Needing GLM's output style, which points to `glm-5.3-flashx` | 
| Video, PDF or other file input | GLM-5.3-Flash | None: DeepSeek takes text and images only | 
| One response longer than 128K tokens | DeepSeek (384K maximum output) | Splitting the job into pieces under 128K | 
| UI, copy or dialogue judged by people | Test both; GLM rates higher on AA's style pairings | Your own blind review | 
| Self-hosting with limited memory | GLM-5.3-Flash (smaller checkpoint) | A quality gap on your own tests that favors DeepSeek | 

Splitting traffic is reasonable when your client already speaks the OpenAI format: off-peak agent work to DeepSeek, peak-hour document or file jobs to GLM, as long as the harness handles the thinking differences in the next section. If you use GLM-5.3-Flash through the GLM Coding Plan in your coding tool, usage is counted in points rather than tokens, so the per-token math above doesn't apply; Z.ai says the plan gives GLM-5.3-Flash three times GLM-5.3's quota. If neither model clears your quality bar, DeepSeek V4.1 vs Kimi K3.1: Flash vs Pro and a 9–28× Cost Gap prices the step up to a Pro-tier model.

## What changes in your code when you switch models

Both APIs accept OpenAI-style chat completions, so switching starts with a base URL and a model ID. The parameters that actually change behavior are thinking, reasoning history with tools, and the output cap. Settings below are from DeepSeek's pricing and thinking mode guide and Z.ai's GLM-5.3-Flash and thinking mode pages, as of October 25, 2026.

| Setting | DeepSeek V4.1 Flash | GLM-5.3-Flash | 
|---|---|---|
| Model ID | `deepseek-flash` (legacy`deepseek-v4-flash` is also served by 1017) | `glm-5.3-flash` ;`glm-5.3-flashx` for the faster variant | 
| Base URL | `https://api.deepseek.com` ; Anthropic format at`https://api.deepseek.com/anthropic` | `https://api.z.ai/api/paas/v4/` | 
| Thinking | On by default; disable with `{"thinking": {"type": "disabled"}}` | Always on; `thinking.type` accepts only`enabled` | 
| Effort | `reasoning_effort``low` ,`high` (default) or`max` | Z.ai recommends `reasoning_effort: max` | 
| Maximum output | 384K tokens | 128K tokens | 
| Sampling | In thinking mode, `temperature` and the penalties are ignored without an error;`top_p` works between 0.95 and 1.0 | Recommended `temperature: 1` ,`top_p: 0.95` | 
| Reasoning history with tools | Pass back every earlier `reasoning_content` , or the API returns 400 | With the recommended `thinking.clear_thinking: false` , return the complete, unmodified`reasoning_content` | 
| Input types | Text and images | Text, images ( `image_url` blocks), video and files | 
| Streaming tool calls | Standard streaming | Z.ai recommends `stream: true` with`tool_stream: true` | 
| Extras | JSON output, Responses API, chat prefix completion (beta), FIM in non-thinking mode only | JSON structured output, automatic context caching reported in `usage.prompt_tokens_details.cached_tokens` | 

The reasoning-history rule is the one that breaks agents. DeepSeek returns HTTP 400 when a request carrying `tools` leaves out earlier turns' `reasoning_content`, and Z.ai warns that edited or reordered reasoning blocks hurt performance and cache hit rates. If your harness strips reasoning to save tokens, fix that before you compare either model. Sampling is the other trap: DeepSeek ignores `temperature` without an error while it is thinking, so tuning it there changes nothing, and only `top_p` values from 0.95 to 1.0 take effect.

On a router, the same ID can behave differently by host. On October 25, OpenRouter's DeepSeek endpoint for `deepseek/deepseek-v4.1-flash-1017` allowed 393,216 completion tokens and Z.ai's endpoint for `z-ai/glm-5.3-flash` allowed 131,072, while third-party hosts of the DeepSeek model ranged from 131,072 to 943,718 and served FP4 or FP8 weights. Pin the provider you tested. To put Claude Code itself on DeepSeek, Claude Code with OpenRouter and DeepSeek: Setup, Routes, and First-Error Fixes covers the setup.

## Can you run GLM-5.3-Flash or DeepSeek V4.1 Flash locally?

Only with a lot of memory, and GLM needs less of it. On October 25, 2026, the Hugging Face file listings put the DeepSeek-V4.1-Flash-1017 checkpoint at 48 safetensors files totaling 475.3 GiB (FP8, with FP4 experts) and GLM-5.3-Flash at 62 files totaling 305.8 GiB (FP8). Those are weights alone; KV cache for long contexts, activations and the serving runtime come on top, so GLM starts about 170 GiB lighter. Both are MIT-licensed. For GLM's hardware fit and serving recipes, see GLM-5.3-Flash Guide: Ox Alpha, API Pricing, and Local Deployment.

If you'd rather rent than host, open weights mean plenty of choice. OpenRouter listed 29 providers for DeepSeek V4.1 Flash 1017 and 34 for GLM-5.3-Flash on October 25, with prices and quantization that differ from the first-party APIs, so the cost table above applies only to DeepSeek's and Z.ai's own endpoints.

## How to test both models on your own tasks before committing

A fair trial holds everything constant except the model and records cost per accepted task, not cost per token. Your own tasks are the only evidence that captures quality, token use and speed for your work at once.

1. Pull 20–50 real tasks from your logs, each with a pass/fail check you already trust: tests pass, JSON validates, or a reviewer accepts the result.
2. Pin exact models: `deepseek-flash` on DeepSeek's API or`deepseek/deepseek-v4.1-flash-1017` on OpenRouter, and`glm-5.3-flash` on Z.ai or`z-ai/glm-5.3-flash` . On a router, also pin the first-party provider.
3. Match effort. AA compared both at max, so start with `reasoning_effort: "max"` on each; for short calls, add a DeepSeek run with thinking disabled, since GLM has no such mode.
4. Keep the harness identical: same system prompt, tools, context and retry policy, with reasoning history passed back as described above.
5. Log every call's usage object: uncached input, cached input, output and reasoning tokens, plus wall-clock time.
6. Price each run three ways, at DeepSeek's off-peak and peak rates and at GLM's flat rate, then divide by the number of accepted tasks.
7. Pick the model with the lower cost per accepted task at the hours your traffic actually runs, and rerun the set when either vendor ships a new checkpoint.

If the two pass the same tasks, the decision reduces to the clock and the cache share of your prompts. If one fails noticeably more, that failure rate outweighs any of the price gaps above, because a retried or hand-fixed task costs more than either model's token bill.

## ## Sources7

External pages this guide links to, in the order they appear. Last updated Oct 25, 2026.


## Sources7

External pages this guide links to, in the order they appear. Last updated Oct 25, 2026.
