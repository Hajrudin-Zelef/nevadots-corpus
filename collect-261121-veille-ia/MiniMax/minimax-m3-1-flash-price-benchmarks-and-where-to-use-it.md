---
id: collect-261121-veille-ia/minimax/minimax-m3-1-flash-price-benchmarks-and-where-to-use-it
title: "MiniMax M3.1 Flash: Price, Benchmarks and Where to Use It"
domain: minimax
role: reference
task: article
actors: ["MiniMax"]
dates: ["2026-10-19"]
keywords: ["minimax", "flash", "price", "october", "model", "input", "tokens", "million"]
source: https://www.digitalapplied.com/blog/minimax-m3-1-flash-price-benchmarks-release
source_anchor: ""
source_lines: [1, 127]
section: "MiniMax"
---
MiniMax released M3.1 Flash on October 19, 2026. Its launch post calls it a generational leap in agent tasks, coding, computer use and cybersecurity over M3, with online inference nearly three times faster and usage costs 70% lower. The API price is $0.15 per million input tokens, $0.60 per million output and $0.015 per million cached input, flat regardless of how long the prompt is. It accepts text, images and video, holds a one-million-token context, and always reasons before it answers.

The price is the headline for anyone running agents at volume. It is half of M3's per-token price and identical to DeepSeek's off-peak input and output rates for V4.1 Flash, which shipped an update two days earlier; DeepSeek's off-peak cached input is lower, at $0.003. That makes M3.1 Flash part of a crowded week of cheap, fast models, which we compare in MiniMax M3.1 Flash vs DeepSeek V4.1 Flash vs Mercury 3. This post covers M3.1 Flash on its own: what it costs, what MiniMax claims and where you can use it.

Sources are MiniMax's launch post, API documentation and price list, read on the release day, plus OpenRouter's model listing and Artificial Analysis's October 19 results. Benchmark figures are MiniMax's own unless we name another source.

1. 01$0.15 in, $0.60 out, at any length.Cached input is $0.015 per million. M3 stays at $0.30 and $1.20 up to 512K input tokens and doubles above that; M3.1 Flash has no long-prompt surcharge on MiniMax's list.
2. 02Agent and coding scores, by MiniMax.MiniMax reports 73.7% on DeepSWE v1.1, 61.7 on OSWorld V2, 29.6 on Agents' Last Exam, 76.2 on BankerToolBench and 82% on CyberGym. Artificial Analysis's independent index, posted October 19, scores it 42, 13 points above M3.
3. 03Thinking is always on, and defaults to max.The API exposes five effort levels from low to max, with max as the default. Leaving it there on routine tasks spends output tokens on reasoning nobody needs.
4. 04Weights now available under a custom licence.Weights arrived October 23 under the MiniMax Community licence. Commercial use requires attribution plus either a one-time notice or, where the product or service exceeds $20M yearly revenue, prior written authorisation. The October 26 OpenRouter capture lists a paid route.
5. 05It already ran in public, anonymously.MiniMax says an early version, Space Bunny, processed more than 200 trillion tokens while anonymous. OpenRouter carried a stealth Space Bunny Alpha route from September 23 to October 5.

## 01 — ReleaseWhat MiniMax *shipped*.

MiniMax positions M3.1 Flash as the efficient tier of its next models and says a Pro model, aimed at the limits of capability, will arrive later. Our M3 release post from May covers the model it succeeds; MiniMax says M3 remains available.

The launch came with a reveal. MiniMax says Space Bunny, a model that processed more than 200 trillion tokens while operating anonymously, was an early version of M3.1 Flash. Our OpenRouter catalogue captures show a stealth route, `stealth/space-bunny-alpha`, listed free from September 23 with an expiry date of October 5, described as an anonymous model with fast inference, strong coding and native multimodal input. Several labs have used anonymous routes like this to gather real usage before attaching a brand name.

Why it matters: a model that has already handled heavy anonymous traffic has been exposed to real workloads, not only benchmarks. The failure case is assuming the launched model is the one you tested under the stealth name. Early versions change before release; rerun anything you evaluated on Space Bunny, or on the M3.1-Flash-Preview model MiniMax offered in MiniMax Code before launch, against M3.1 Flash itself.

## 02 — PricingThe price, flat at *any length*.

MiniMax's price list shows one row for M3.1 Flash and two tiers for M3. The difference in structure matters as much as the difference in price.

| Source: MiniMax API pricing page, standard tier, USD per million tokens, read October 19, 2026. |  |  |  | 
|---|---|---|---|
| Model | Input | Output | Cached input | 
|---|---|---|---|
| M3.1 Flash, any length | $0.15 | $0.60 | $0.015 | 
| M3, up to 512K input | $0.30 | $1.20 | $0.06 | 
| M3, over 512K input | $0.60 | $2.40 | $0.12 | 

Per token, M3.1 Flash costs half of M3 on short prompts and a quarter on prompts above 512K input tokens, where M3's price doubles. Cached input falls further, to a quarter of M3's short-prompt cache rate. For agents that re-read large codebases or long documents on every turn, the cache line is where most of the saving lands.

MiniMax's statement that "overall usage cost is 70% lower" is a different claim from the per-token cut: it describes total cost on agent tasks, which also depends on how many tokens the model spends to finish. It is MiniMax's measure on its own tasks. The worked example that matters is yours: run twenty of your real tasks on M3 and M3.1 Flash, and compare total spend per finished task rather than the price per million.

For teams already on M3, the move is a change of model name, from `MiniMax-M3` to `MiniMax-M3.1-Flash`, on the same API, and three behaviours change with it. Thinking: M3 thinks adaptively by default and skips thinking when a request sets it to disabled; M3.1 Flash always thinks, and MiniMax's API reference says a request that sends thinking disabled now returns an HTTP 400 error. Long prompts: M3's price doubles above 512K input tokens and M3.1 Flash's does not, which removes the reason to trim context just to stay under a threshold. Priority tier: M3.1 Flash does not accept the service_tier field, so M3's 1.5x priority option does not carry over. Inputs stay the same: both models take text, images and video.

M3 stays on MiniMax's price list, so there is no forced deadline. The failure case is switching production traffic in one go and finding that requests which disabled thinking on M3 now fail, or that latency rose because every request now reasons. Run the two side by side on the same tasks for a few days, then move traffic over in stages.

## 03 — CapabilitiesWhat it can *take in*.

MiniMax's API documentation lists three headline capabilities: a one-million-token context, multimodal input, and adjustable thinking depth. M3 already took images and video; the adjustable depth is what is new, since only M3.1 Flash honours reasoning_effort. Thinking is on by default and cannot be switched off; the reasoning comes back in a separate `reasoning_content` field, so an application can log it without showing it to users.

OpenRouter's listing describes it as built for multi-step work automation, understanding real codebases, fixing bugs and building features, and visual creation such as front-end pages and presentations. The API is OpenAI-compatible, and MiniMax also documents an Anthropic-compatible endpoint, which is how coding tools such as Claude Code connect to it.

The video input is the capability to test first if you have a use for it. Agents that check a screen recording of a bug, review a product demo or summarise a training video can now do it at half M3's per-token price. The failure case is assuming video means audio: MiniMax lists text, images and video as inputs and text as output.

## 04 — BenchmarksMiniMax's benchmark *figures*.

The launch post gives five figures in text, across four areas. MiniMax also publishes charts comparing M3.1 Flash with other models, but they are images, and we do not reproduce rival scores we cannot read from text.

| Source: MiniMax M3.1 Flash launch post, October 19, 2026. MiniMax-reported; harnesses not stated. Artificial Analysis's independent result is in the text below. |  |  | 
|---|---|---|
| Benchmark | Score | What it tests | 
|---|---|---|
| DeepSWE v1.1 | 73.7% | Developing features, fixing issues and validating them with tests | 
| OSWorld V2 | 61.7 | Operating a computer across applications | 
| Agents' Last Exam | 29.6 | Real professional tasks with economic value | 
| BankerToolBench | 76.2 | Multi-step planning, documents and chained tool calls | 
| CyberGym | 82% | Finding and validating vulnerabilities | 

MiniMax adds two claims without figures: that M3.1 Flash ranks among the top performers on AutomationBench-AA, which tests end-to-end business workflows across software tools, and that it ranks first among Chinese models on OSWorld V2 and Agents' Last Exam. For context, DeepSeek reports 77.0 on DeepSWE v1.1 and 35.4 on Agents' Last Exam for its V4.1 Flash 1017 update. DeepSeek names its DeepSWE harness (Mini-SWE-V2) and MiniMax names none, which is why the comparison post treats them as two self-reports rather than a ranking. Independent results have started to appear: Artificial Analysis posted on October 19 that M3.1 Flash, run at max effort, scores 42 on its Intelligence Index, 13 points above M3 and behind DeepSeek V4.1 Flash 1017, and 73% on its AutomationBench-AA.

The cybersecurity figure deserves a note. MiniMax says its internal security experts found the model strong at code auditing, vulnerability patching, penetration testing, malware analysis and reverse engineering. That is useful for defenders and a reason for teams deploying it in agents to keep the same guardrails they use for any capable coding model.

## 05 — SpeedSpeed: claimed and *observed*.

MiniMax credits more aggressive sparsity for the speed: output can exceed 200 tokens a second in everyday use, with a median of more than 170. The launch post gives no parameter count; MiniMax's MiniMax Code page for the pre-launch preview build lists a mixture-of-experts design with 428B total and about 23B active parameters, sparse attention and a native visual encoder, without saying which kind of sparsity changed.

OpenRouter's model page for the free route, read on October 19, showed a different picture: a median of 103 tokens a second and 1.15 seconds of latency over its three-day window. That is not a contradiction so much as a reminder of what a free, rate-limited route measures. Throughput depends on the provider, the load, the length of the output and the effort level. Measure speed on the route and effort setting you will actually pay for.

## 06 — AccessWhere to use it *today*.

##### Pay as you *go*

Model name MiniMax-M3.1-Flash. OpenAI-compatible and Anthropic-compatible endpoints, so existing SDKs and coding tools connect with a base URL change.

##### Subscription

All three tiers include M3.1 Flash as the text model, with Explore at three times Go's usage and Build at seven and a half times. Keys work in Claude Code, Codex, Cursor and OpenCode.

##### Paid route, checked October 26

$0.15 input / $0.60 output / $0.015 cached input per million tokens in the October 26 API capture, with 1,048,576 context. The launch-day free route is absent from that capture; provider quotes can vary.

MiniMax Code, the company's own agent app for web, desktop and mobile, also runs the model. For teams that already use Claude Code, our setup guide for running M3.1 Flash inside Claude Code covers the configuration MiniMax documents.

Update, October 26: the weights were uploaded on October 23 to MiniMax's official model repository. The MiniMax Community licence is a custom licence, not MIT or Apache. Commercial users must display the required attribution. Products or services generating more than $20 million in yearly revenue need MiniMax's prior written authorisation; otherwise the licence requires a one-time notice. Downloadable weights create a self-hosting option, but teams still need to evaluate hardware, operating costs and their licence terms. API data-processing terms remain a separate decision.

## 07 — SettingsChange the default *effort*.

M3.1 Flash accepts a `reasoning_effort` (`output_config.effort` on the Anthropic-compatible endpoint) of low, medium, high, xhigh or max, and the documented default is max. OpenRouter's route shows the same default, with reasoning marked as mandatory. For a model priced on output tokens, a default at the top of the scale means every request spends as much on reasoning as the model decides it needs.

The example: a classification or extraction step in a pipeline needs a short, correct answer, and low effort will usually get it at a fraction of the output tokens. A multi-file refactor earns high or above. The failure case is an agent framework that never sets the parameter, runs everything at max, and makes a cheap model expensive.

## 08 — FitWho should *try it*.

The routing below is ours, built from MiniMax's documentation and the pricing above, not from a trial of our own.

##### High-volume agent and coding work

Teams already on M3 or a similar mid-priced model, running many agent steps a day, get half the per-token price and a quarter on long prompts.

##### Agents that need to see

Screenshots, product images and screen recordings in an agent loop, at a price below most multimodal models.

##### Self-hosting or strict residency

Weights were uploaded October 23. Review the custom MiniMax Community licence, revenue threshold, attribution, notice-or-authorisation conditions and hardware needs before choosing self-hosting.

##### Already on DeepSeek Flash

Same off-peak price; DeepSeek doubles at peak and has MIT weights, MiniMax is flat and reads video. Run both on your tasks.

Our look at what the sub-$0.30 model tier unlocks covers the economics of moving bulk work to this price band. If you want help running a structured trial and moving production agents to a cheaper model safely, that is part of our AI transformation work.

## 09 — ConclusionHalf the price, with *eyes*.

### Trial M3.1 Flash on twenty real agent tasks with reasoning effort set per task, and compare cost per finished task against your current model.

MiniMax M3.1 Flash, released October 19, costs $0.15 per million input tokens and $0.60 output, flat at any length, with a million-token context and image and video input. That is half of M3's price on short prompts and a quarter on long ones.

MiniMax reports 73.7% on DeepSWE v1.1 and strong agent, computer-use and security scores, and nearly three times M3's speed. All of it is MiniMax's own measurement, and OpenRouter's free route ran slower than the claim on launch day.

The default effort is max. The weights are now available under a custom licence, and the price only pays off if the token count per task stays in check. Set effort per task, measure, then decide.
