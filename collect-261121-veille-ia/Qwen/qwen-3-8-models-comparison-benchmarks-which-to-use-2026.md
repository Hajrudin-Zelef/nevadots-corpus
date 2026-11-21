---
id: collect-261121-veille-ia/qwen/qwen-3-8-models-comparison-benchmarks-which-to-use-2026
title: "Qwen 3.8 Models Comparison, Benchmarks & Which to Use (2026)"
domain: qwen
role: reference
task: article
actors: ["Alibaba"]
dates: []
keywords: ["qwen3", "model", "flash-next", "qwen", "models", "hosted", "benchmarks", "open"]
source: https://www.respan.ai/articles/qwen-3.8-models-comparison
source_anchor: ""
source_lines: [1, 161]
section: "Qwen"
---
Qwen3.8-Max went generally available on August 3, 2026, and the open-weight Qwen3.8-27B followed about ten days later. By August 26, Alibaba's Qwen team had added Qwen3.8-Flash-Next and its hosted version, Qwen3.8-Flash, rounding out a four-model family.

The 27B has drawn much of the interest, and it's easy to see why. It's a vision-capable model released under Apache 2.0 that you can run on your own hardware, which puts a capable open model within reach of teams that don't want to depend on an API. On the hosted side, Max lists at $2 per million input tokens and $6 per million output, against $4 and $20 for Claude Opus 5.5.

The catch is that these are reasoning models, and how long a model thinks drives the bill as much as its token price does. Choosing a Qwen 3.8 model means choosing an effort level to run it at too, and the reliable way to price that pair is to measure cost and quality per request on your own traffic.

Here's how the Qwen 3.8 models compare on benchmarks and pricing, and which one to use for your workload.

## What models does Qwen 3.8 offer?

Qwen 3.8 ships as four models. Two are hosted APIs and two are open-weight models you can download, and each hosted model is built on an open checkpoint.

- **Qwen3.8-Max** - The hosted flagship, a mixture-of-experts model with 2.4 trillion total parameters and 95 billion active per token. It accepts image and video input alongside text, has a 1M-token context window, and runs in both thinking and non-thinking modes. Its open checkpoint, Qwen3.8-2.4T-A95B, is text-only and ships under a custom license.
- **Qwen3.8-27B** - A dense 27B vision-language model released under Apache 2.0. It supports 262K tokens of context natively, extensible to 1M, and Alibaba Cloud also hosts it with a 1M-token window.
- **Qwen3.8-Flash** - The hosted production version of Qwen3.8-Flash-Next, the open-weight model below. It adds a 1M-token context window by default and built-in tools.
- **Qwen3.8-Flash-Next** - An open-weight mixture-of-experts model with 125B parameters and 6B active per token, plus a separate 51B n-gram embedding table. Qwen released it as an early preview of the architecture planned for Qwen4, under the qwen-community-1.0 license.

Every model in the family reasons before it answers unless you turn thinking off. The open model cards set `reasoning_effort` to `xhigh` by default, which is the top of three levels, and that default is where cost per task starts to drift away from the price sheet.

## Qwen 3.8 benchmarks

Qwen's own benchmark tables cover Max, Flash-Next, and the 27B on a shared set of public benchmarks. Flash is built on Flash-Next, so the Flash-Next column covers both.

Max's scores come from the Qwen3.8-2.4T-A95B model card, and the Qwen3.8-Flash-Next model card reports Flash-Next and the 27B side by side:

| Benchmark | Max | Flash-Next | 27B | 
|---|---|---|---|
| SWE-bench Pro | 67.7 | 62.5 | 61.7 | 
| DeepSWE 1.1 | 56.6 | 58.7 | 42.2 | 
| Toolathlon Verified | 72.5 | 73.5 | 67.1 | 
| GPQA Diamond | 92.6 | 91.7 | 89.2 | 
| HLE | 43.6 | 35.9 | 30.8 | 
| IFBench | 82.8 | 81.3 | 79.5 | 

On coding and agentic work, Max scores higher on SWE-bench Pro, while Flash-Next edges it on DeepSWE 1.1 and Toolathlon Verified with 6B active parameters against Max's 95B. The 27B stays close on SWE-bench Pro but falls well behind on DeepSWE. If you're building a multi-step coding agent, that gap is worth more weight than its single-task scores.

Reasoning and knowledge scores sit closer together. GPQA Diamond spans only 89.2 to 92.6 across the three, whereas HLE opens a wider spread, with Max at 43.6 and the 27B at 30.8.

For vision, the Flash-Next card puts Flash-Next ahead of the 27B on RealWorldQA (88.5 vs 85.9) and AndroidWorld (84.5 vs 81.9). Max's card has no vision scores because the open checkpoint behind it is text-only, even though the hosted Max accepts images and video.

Artificial Analysis runs its own evaluation suite, and its Qwen3.8 27B results break out each reasoning effort level separately. At `xhigh`, the 27B scored 34 on the Intelligence Index and cost $1.01 per task. Medium scored 28 at $1.13, low scored 26 at $1.05, and with reasoning off it scored 20 at $2.49 per task. Flash-Next scored 40 on the same index.

In other words, the default `xhigh` setting scored higher than every other 27B setting and still cost less per task than any of them. The token price was identical across those runs, so the difference came entirely from how many tokens each task consumed.

Max shows the same effect from the other direction. In Artificial Analysis's Max evaluation, it scored 45 and cost $5.41 per task, but it generated 190M output tokens across the evaluation against a median of 88M for comparable models.

## Qwen 3.8 models comparison

Hosted output prices across the family differ by more than 12x, from $0.47 per million tokens on Flash to $6 on Max. Alibaba Cloud's Model Studio pricing lists the hosted models at the international rates below. Flash-Next has no hosted price of its own because Flash is its hosted version.

| Model | Access | License | Context | Price (in / out per 1M) | AA Index | 
|---|---|---|---|---|---|
| Qwen3.8-Max | Hosted API | Custom (open checkpoint) | 1M | $2 / $6 | 45 | 
| Qwen3.8-27B | Open weights, hosted | Apache 2.0 | 262K native, 1M hosted | $0.50 / $3.00 | 34 | 
| Qwen3.8-Flash-Next | Open weights | qwen-community-1.0 | 262K native | Self-hosted only | 40 | 
| Qwen3.8-Flash | Hosted API | N/A (hosted) | 1M | $0.15 / $0.47 | N/A | 

The last two factors only tell you something useful when you read them together. A higher index score at a higher token price can still come out cheaper per completed task, and a lower one can still come out more expensive, depending on how many tokens each model spends to get there.

## Qwen 3.8 vs Qwen 3.8 Max

Comparisons of "Qwen 3.8" against Qwen 3.8 Max usually mean the 27B, since it's the open-weight release designed to run on your own hardware.

On capability, Max scores higher on every benchmark the two share. The gap is small on GPQA Diamond (92.6 vs 89.2) and large on DeepSWE 1.1 (56.6 vs 42.2) and HLE (43.6 vs 30.8), which puts Max's advantage in long, multi-step tasks more than in single questions. Artificial Analysis's index shows a similar spread, with Max at 45 and the 27B at 34.

Cost moves further apart than the token prices suggest. Hosted, Max costs 4x the 27B on input and 2x on output, but on Artificial Analysis's per-task figures Max came to $5.41 against $1.01 for the 27B at `xhigh`, a bit over 5x. Some of that gap comes from Max's heavier token use rather than its rate card.

Deployment is where the 27B changes the decision. It's Apache 2.0, so you can download it, fine-tune it, and serve it on your own infrastructure. Max's full feature set, including vision and non-thinking mode, is only available through the API, and its open checkpoint is a 2.4-trillion-parameter text-only model built for cluster-scale serving.

**Choose Qwen3.8-27B if:** you want to self-host or fine-tune under Apache 2.0, and your tasks don't hinge on long multi-step runs where its DeepSWE gap shows up.

**Choose Qwen3.8-Max if:** you're running long-horizon coding or agent work where its higher DeepSWE and HLE scores earn the extra cost per task, and a hosted API fits your stack.

## Qwen 3.8 Flash Next vs Qwen 3.8 27B

Flash-Next and the 27B are the two open-weight models in the family, but they're built very differently. Flash-Next is a mixture-of-experts model that computes with only 6B parameters per token, while the 27B is dense and uses all 27B parameters on every token.

That difference shows up in hardware. Flash-Next stores roughly 180B parameters in total once the n-gram table and multi-token prediction head are counted, although Qwen designed the n-gram table to be offloaded from GPU memory more easily than expert weights. The 27B needs far less memory overall but more compute per token.

On benchmarks, Flash-Next scores higher on every row in Qwen's shared table, and the gap is widest on DeepSWE 1.1 (58.7 vs 42.2).

Licensing points the other way. The 27B is Apache 2.0, whereas Flash-Next uses Qwen's own qwen-community-1.0 license, so read its terms before you build a commercial product on the weights. Hosted, the gap runs in Flash-Next's favor too, since its hosted version, Flash, costs $0.15 / $0.47 against the hosted 27B's $0.50 / $3.00.

**Choose Qwen3.8-Flash-Next if:** you have the memory for a roughly 180B-parameter checkpoint and its license works for your use, or call it through its hosted version, Flash, if you'd rather not serve it yourself.

**Choose Qwen3.8-27B if:** you're self-hosting on limited GPU memory, or an Apache 2.0 license is a requirement.

## Qwen 3.8 vs Claude Opus 5.5

Claude Opus 5.5 is priced at $4 per million input tokens and $20 per million output, against Max's $2 and $6. That makes Max's output rate less than a third of Opus's. The Opus 5.5 pricing and benchmarks guide covers the Anthropic side in more depth.

On benchmarks, Qwen's model cards don't include Opus 5.5, so Artificial Analysis is the like-for-like comparison. Artificial Analysis's Opus 5.5 results put it at 58 on the Intelligence Index at max effort, against Max's 45.

The per-task numbers are closer than the token prices. Opus 5.5 at max effort cost $5.98 per task, using about 119K output tokens per task, while Max cost $5.41. The output rates differ by more than 3x, yet the cost of a finished task differs by about 10%.

Both models offer a 1M-token context window, and both are available through a hosted API. The difference is that Opus 5.5 is closed, while the Qwen 3.8 family includes open-weight options, so a team can prototype on Max and later move some traffic to self-hosted Qwen models without changing families.

**Choose Qwen3.8-Max if:** you want a lower token price and a path to open-weight models in the same family, and a lower index score (45 vs 58) is acceptable for your tasks.

**Choose Claude Opus 5.5 if:** you need its higher score on Artificial Analysis's index, since its cost per completed task lands within about 10% of Max's.

## Gemma 4 vs Qwen 3.8

Gemma 4 is Google DeepMind's open-weight family, so it competes with the open half of Qwen 3.8 rather than with Max or Flash.

According to the Gemma 4 model card, the family comes in five sizes: E2B, E4B, 12B, a 26B A4B mixture-of-experts model, and a 31B dense model. The 31B is the closest match to Qwen's 27B. The smaller E2B, E4B, and 12B models also accept audio input and target phones and laptops, with 128K context on the two edge models and 256K on the rest.

On the benchmarks both vendors report, the Qwen 27B scores higher than the Gemma 31B:

- **GPQA Diamond** - 89.2 vs 84.3.
- **LiveCodeBench v6** - 90.3 vs 80.0.
- **HLE** - 30.8 vs 19.5.

Both vendors ran their own evaluations, so treat small gaps with caution, but these margins are wide enough to matter. On licensing, every Gemma 4 model is Apache 2.0, while only the 27B carries that license in Qwen 3.8.

**Choose Gemma 4 if:** you're deploying on-device or need audio input in a small model.

**Choose Qwen3.8-27B if:** you want the higher benchmark scores at roughly the same size, with hosted Qwen models available in the same family when you need them.

## Which Qwen 3.8 model should I use?

For most hosted workloads, start with Qwen3.8-Flash and move up to Max only for long-horizon coding and agent tasks where a failed run costs more than the tokens. For self-hosting, the 27B fits a smaller footprint under Apache 2.0, while Flash-Next trades more memory for higher scores.

Choosing the reasoning effort level matters as much as choosing the model. The open models default to `xhigh`, with `medium` and `low` available along with a switch to turn thinking off. Artificial Analysis's 27B results show the default cost less per task than any lower setting, but that result came from its evaluation suite rather than your workload.

Lower effort can also backfire in multi-turn agent loops, where a thinner analysis on one turn leads to failures and retries later in the run. The practical approach is to run the same set of real requests at each model and effort level you're considering, then compare quality scores against cost per request before you commit.

## Measure Qwen 3.8 cost and quality per request with Respan

Switching from Max to Flash, or from `xhigh` to `low`, is a one-line change. Knowing whether it saved money without costing you quality is the hard part, and without the right data you find out when the bill arrives or a user complains.

Respan is the LLM engineering platform that puts your gateway, observability, evals, and prompt management in one place. Route every Qwen 3.8 call through Respan's LLM gateway, and switch between the four models, or any of 1,000+ others, by changing one word.

- **Know where every $ goes** - See cost broken down by model, by request, and by end customer. Compare what a task costs on Flash against Max, and set hard spend limits so a verbose run never surprises you on the invoice.
- **Close the loop** - Find a weak answer in a trace, turn real requests into a dataset, and test each model and effort level against it. Watch the score move before anything ships.
- **Catch regressions in production** - Run the same evaluators on live traffic, so a quality drop after a model switch surfaces in real time instead of in support tickets.
- **Stay up when a provider fails** - Define a fallback chain once, and traffic reroutes to the next model automatically when a provider errors or rate-limits.

Route, observe, and evaluate every LLM call. The Qwen 3.8 setup you ship is then one you've measured on your own traffic.

## FAQ

### Is Qwen 3.8 open source?

Partly. Qwen3.8-27B is released under Apache 2.0, and Qwen3.8-Flash-Next is open-weight under the qwen-community-1.0 license. Qwen3.8-Max and Qwen3.8-Flash are hosted APIs, although Max's open checkpoint, Qwen3.8-2.4T-A95B, is available as a text-only model under a custom license.

### Can I run Qwen 3.8 locally?

Yes, with the 27B. Quantized builds are available for llama.cpp, Ollama, and LM Studio, and it supports 262K tokens of context natively. Set the reasoning effort deliberately when you run it, because the `xhigh` default produces long reasoning traces that fill a small context limit quickly. Flash-Next needs considerably more memory, and the 2.4T checkpoint requires cluster-scale hardware.

### What is the Qwen 3.8 context window?

Max and Flash both have a 1M-token context window on the hosted API. The open 27B and Flash-Next models support 262,144 tokens natively and can extend to 1M with YaRN scaling, and Alibaba Cloud's hosted 27B runs with a 1M window by default.

### How do I turn off thinking in Qwen 3.8?

Set `enable_thinking` to `false`. When self-hosting with vLLM or SGLang, pass it inside `chat_template_kwargs`, and on Qwen Cloud pass it directly in the request body. To keep thinking on but make it shorter, set `reasoning_effort` to `medium` or `low` instead. The Qwen3.8-2.4T-A95B checkpoint can't disable thinking at all, so use the hosted Max if you need non-thinking mode at that size.

### Can I access Qwen 3.8 through an API?

Yes. All four Qwen 3.8 models are available through Respan's gateway, which lets you call them with one API key alongside 1,000+ other models and track cost and quality for every request. You can also call Max, Flash, and the 27B directly through Qwen Cloud or Alibaba Cloud Model Studio, which accept OpenAI-style Chat Completions requests.
