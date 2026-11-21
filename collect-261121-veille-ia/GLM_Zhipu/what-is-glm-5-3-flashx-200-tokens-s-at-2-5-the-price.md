---
id: collect-261121-veille-ia/glm-zhipu/what-is-glm-5-3-flashx-200-tokens-s-at-2-5-the-price
title: "What Is GLM-5.3-FlashX? 200 Tokens/s at 2.5× the Price"
domain: glm-zhipu
role: reference
task: article
actors: ["Zhipu AI"]
dates: ["2026-09-18"]
keywords: ["flashx", "glm-5", "tokens", "output", "tool", "flash", "does", "speed"]
source: https://gptproto.com/news/what-is-glm-5-3-flashx
source_anchor: ""
source_lines: [1, 160]
section: "GLM_Zhipu"
---
## What Is GLM-5.3-FlashX?

GLM-5.3-FlashX is a high-speed API tier built around the GLM-5.3-Flash capability base. Z.ai lists Flash and FlashX on the same model documentation page, with separate identifiers:

`glm-5.3-flash`
`glm-5.3-flashx`

The two entries share the same published family specifications. That supports treating FlashX as a serving choice rather than a new model generation. It does not prove that every implementation detail or weight file is identical; Z.ai has not published a separate FlashX checkpoint or a statement that resolves that question at the weight level.

| Specification | GLM-5.3-FlashX | 
| Developer | Z.ai / Zhipu AI | 
| Release date | September 18, 2026 | 
| Official API model ID | `glm-5.3-flashx` | 
| Positioning | High-speed hosted inference tier | 
| Advertised generation speed | Up to 200 output tokens/s | 
| Architecture | Mixture of experts; hybrid sparse and linear attention | 
| Total / active parameters | 320B total / about 18B active per token | 
| Context window | 1,048,576 tokens | 
| Maximum output | 131,072 tokens | 
| Inputs | Text, images, video, and files | 
| Output | Text and tool calls | 
| Thinking | Enabled; the documented hosted configuration does not allow it to be disabled | 
| Other documented features | Streaming, function calling, context caching, structured output | 

Z.ai recommends `temperature: 1`, `top_p: 0.95`, maximum reasoning effort, and retained thinking context. For streamed tool use, its documentation recommends enabling both response streaming and streamed tool arguments. Those settings describe Z.ai’s route. A gateway can expose different parameter names or omit some controls, which is one reason a copied payload should not be labeled portable without testing it on the exact endpoint.

The practical feature is native multimodal input inside a long-context agent loop. FlashX can inspect text, screenshots, files, and video, then return text or tool calls. It does not generate images or video. The one-million-token context also describes capacity, not guaranteed recall across a full million tokens; retrieval and acceptance tests still matter.

## GLM-5.3-FlashX Release and the 200 Tokens/s Claim

Z.ai launched GLM-5.3-FlashX on September 18, 2026. The company’s English documentation says the API is live and reports inference speed of 200 tokens/s. Release coverage describes that figure as up to five times the speed of GLM-5.3-Flash.

“Up to 200 tokens/s” is a peak output-rate claim. It is not a promise that every request finishes five times sooner.

An API request has several clocks:

**Time to first token:** queueing, prompt processing, multimodal preprocessing, and initial reasoning before visible output starts.
**Sustained output speed:** the rate at which tokens arrive after generation begins.
**End-to-end completion time:** the entire request, including hidden reasoning, a long answer, retries, and tool calls.
**Tail latency:** p95 or p99 behavior under real concurrency, rather than a median from clean traffic.

FlashX’s headline directly addresses the second clock. It may improve the others, but the launch page does not publish the test prompt, output length, region, concurrency, reasoning setting, or percentile behind 200 tokens/s.

Early public traffic illustrates the difference between a maximum and a distribution. A public routing marketplace showed roughly 80–87 tokens/s at the median during a short post-launch window, while its upper-percentile observations approached the advertised maximum. The same page showed a median time to first token of about 2.3 seconds. Those values are a changing observation of that route, not a benchmark run by GPT Proto and not a service-level guarantee from Z.ai. They are still useful for one conclusion: a 200-token/s ceiling can coexist with much lower typical requests. We do not link to API-aggregator comparison pages in this editorial article; retain the source and capture date in the internal fact card.

Claims of independent FlashX tests deserve the same scrutiny. One community post said short prompts often sustained more than 160 tokens/s, but it did not provide raw logs, a script, concurrency settings, or a reproducible prompt set. That number should not be used for budgeting. As of September 21, we did not find a public FlashX-versus-Flash developer test with enough original evidence to treat it as a controlled benchmark.

## GLM-5.3-FlashX Pricing

Launch coverage published on September 18 reproduced the following China price table in Chinese yuan per one million tokens. Z.ai’s English product page did not expose the same token table when checked on September 21, so these regional figures are attributed to the launch reporting rather than presented as a universal global rate:

| Usage | GLM-5.3-FlashX | GLM-5.3-Flash | FlashX premium | 
| New input | ¥2.00 | ¥0.80 | 2.5× | 
| Cached input | ¥0.50 | ¥0.20 | 2.5× | 
| Output | ¥7.00 | ¥2.80 | 2.5× | 
| Cache storage | Temporarily free | Temporarily free | — | 

The “temporarily free” cache-storage line is promotional and can change. It should not be treated as a permanent zero-cost feature.

Z.ai’s official model documentation does not currently show a FlashX token table. A public route carrying Z.ai as the provider displayed $0.37 per million input tokens, $0.075 per million cached input tokens, and $1.25 per million output tokens when checked on September 21. Those are useful third-party route prices, but they are not proof of one universal Z.ai direct price across regions, contracts, taxes, and currencies.

The 2.5× relationship is easier to verify than a converted USD figure. Consider a workload that consumes 10 million new input tokens and two million output tokens, with no cache hits:

`FlashX cost = 10 × ¥2 + 2 × ¥7 = ¥34`

`Flash cost = 10 × ¥0.8 + 2 × ¥2.8 = ¥13.6`

The speed tier adds ¥20.4 for that workload. That premium makes sense only if the reduced waiting time is worth more than ¥20.4 to the business or user.

For an overnight batch job, finishing earlier may produce no additional value. For an agent that blocks a developer through 12 sequential model turns, shaving time from every generation step can be worth considerably more than the token surcharge. The useful equation is therefore:

`value of time saved > additional token cost + migration and validation cost`

Do not estimate with visible answer tokens alone. Thinking tokens, retries, failed tool calls, and unnecessarily long answers can change the bill. The production metric should be cost per accepted task, not price per million tokens in isolation.

## GLM-5.3-FlashX vs GLM-5.3-Flash

GLM-5.3-FlashX and GLM-5.3-Flash answer different operational priorities. The public evidence does not establish a quality upgrade for FlashX; it establishes a higher-priced speed tier.

| Dimension | GLM-5.3-FlashX | GLM-5.3-Flash | 
| Main reason to choose | Lower interactive waiting time | Lower token cost | 
| Advertised output speed | Up to 200 tokens/s | Lower standard service tier; launch reports use it as the 1× baseline | 
| China list price | ¥2 input / ¥7 output per 1M | ¥0.8 input / ¥2.8 output per 1M | 
| Public specifications | 320B/18B, 1M context, native multimodal input | Same published family specifications | 
| Separate intelligence benchmark | None published for FlashX | Flash benchmark suite is available | 
| GLM Coding Plan | Not included at launch | Included with the documented plan quota | 
| Self-hosting | No separate FlashX weights announced | Base Flash weights are MIT-licensed | 
| Best workload shape | Interactive, sequential, human-in-the-loop | Batch, asynchronous, high-volume, price-sensitive | 

Choose FlashX when a person is watching the stream, output is long enough for decode speed to matter, and the workflow makes several model calls in sequence. Coding copilots, interactive research agents, live support assistants, and visual iteration loops fit that shape.

Keep Flash when the job runs unattended, external tools dominate elapsed time, answers are short, or the workload produces enough tokens that a 2.5× rate materially changes the budget. A database call that takes eight seconds is not fixed by generating the following sentence faster. Neither is a browser agent that spends most of its time waiting for pages, downloads, or approvals.

The self-hosting distinction is also important. GLM-5.3-Flash has MIT-licensed weights, although a 320B checkpoint still requires serious infrastructure. FlashX is a hosted performance tier, not a separate downloadable speed switch. Teams that need deployment control or fixed hardware economics should evaluate the base Flash weights instead of assuming FlashX can be self-hosted.

## Is GLM-5.3-FlashX Good for Coding?

GLM-5.3-FlashX is a plausible coding model when latency is part of the product experience. Faster token delivery can improve code completion, reduce pauses during long patches, and shorten agent loops that repeatedly plan, edit, run tools, and inspect results.

What the evidence does not show is that FlashX writes better code than Flash.

Z.ai has published strong coding and agent results for the underlying GLM-5.3-Flash capability base, including 63.4 on DeepSWE v1.1, 48.8 on AutomationBench, and 29.0 on Z.ai Code Bench at maximum reasoning effort. These are vendor-run results for Flash, including an in-house benchmark. They are not a separate FlashX quality evaluation. Faster serving may make the same capability feel better in an IDE, but only a matched test can establish quality parity on a specific repository.

A useful coding evaluation should keep the following fixed across Flash and FlashX:

- repository commit and dependency lockfile;
- prompt and system instructions;
- tools, schemas, and permissions;
- reasoning effort and output limits;
- timeout and retry policy;
- number and order of tasks;
- acceptance tests and human review rules.

Then record more than tokens per second:

| Metric | Why it matters | 
| Time to first token | Measures the pause before visible work begins | 
| Sustained output tokens/s | Tests the advertised speed where it applies | 
| End-to-end task time | Includes reasoning, tools, tests, and retries | 
| p95 task time | Reveals slow outliers hidden by averages | 
| Accepted patches | Prevents fast but unusable output from winning | 
| Tool-call error rate | Captures agent reliability | 
| Total billed tokens | Includes verbosity and retries | 
| Human correction time | Measures the work the model hands back to the developer | 

The decision rule is strict: upgrade only if Flash already passes the quality bar and FlashX lowers p95 or complete-task time enough to justify the higher accepted-task cost. A faster stream is pleasant; a faster accepted patch is economically useful.

GPT Proto has not yet completed a FlashX integration test, so we are not publishing a GPT Proto request sample. The official model ID is known, but a model ID alone does not confirm a gateway endpoint, authentication behavior, multimodal upload path, tool-call mapping, or billing.

## GLM-5.3-FlashX vs DeepSeek Flash

A precise GLM-5.3-FlashX vs DeepSeek Flash table requires one clarification that many comparison pages skip: “DeepSeek Flash” is not a stable, unambiguous product label. Current search results mix DeepSeek V4 Flash, dated experimental variants, unofficial route names, and price snapshots from different providers.

Combining those entries into one rival produces false precision. A benchmark for one checkpoint, a price from another route, and a context limit from a later release do not describe a model anyone can actually buy.

Lock the comparison to an exact DeepSeek model ID, provider, region, price date, and API configuration. Then run the same repository and agent evaluation setup used for FlashX. Compare accepted patches, tool reliability, total tokens, wall-clock time, human correction, and cost per accepted task.

GLM-5.3-FlashX has three clearly documented attributes in that test: native text, image, video, and file input; a one-million-token context window; and a dedicated high-speed hosted tier. A DeepSeek candidate may be cheaper or faster on a particular route, especially for cache-heavy coding traffic, but that conclusion must come from the selected endpoint rather than a generic “DeepSeek Flash” label.

If your application is text-only and price-sensitive, include a DeepSeek candidate. If the agent must inspect screenshots or video within the same model call, GLM-5.3-FlashX starts with a clearer documented fit. Neither observation replaces the matched workload test.

## Should You Upgrade to GLM-5.3-FlashX?

Upgrade from Flash to FlashX if all three conditions are true:

1. GLM-5.3-Flash already meets your quality and tool-reliability requirements.
2. Model decoding is a meaningful part of your end-to-end delay.
3. A controlled test shows that the saved time is worth a 2.5× token rate.

Do not upgrade by default for offline generation, scheduled analysis, or high-volume batch processing. Do not upgrade when retrieval, browser actions, databases, builds, or human approval dominate task time. And do not upgrade for better reasoning unless your own test demonstrates it; Z.ai has not published a separate FlashX intelligence benchmark.

A staged rollout is safer than a full switch. Route a representative slice of traffic to FlashX, preserve the same prompts and tools, and compare complete workflows. If the p95 improves while accepted-task quality remains stable, expand. If only the animated text stream looks faster while completion time and acceptance stay flat, keep the cheaper route.

## Is GLM-5.3-FlashX Available on GPT Proto?

Not yet. GPT Proto plans to add GLM-5.3-FlashX, but production availability has not been confirmed as of September 21, 2026.

Before a GPT Proto model page or code sample should be published, the integration needs to verify the endpoint, exact model string, text and multimodal inputs, streaming, tool calls, reasoning controls, context caching, error behavior, limits, and billed prices. Until that check is complete, “available through GPT Proto” would be an unsupported product claim.

For models you can use now, browse the current GPT Proto model directory. You can also check the GPT Proto homepage for platform updates. For a deeper explanation of the underlying capability base, read What Is GLM-5.3-Flash?.
