---
id: collect-261121-veille-ia/xiaomi-mimo/xiaomi-mimo-v2-6-pro-vs-flash-pricing-benchmarks-api
title: "Xiaomi MiMo V2.6 Pro vs Flash: Pricing, Benchmarks & API"
domain: xiaomi-mimo
role: reference
task: article
actors: ["Xiaomi"]
dates: []
keywords: ["flash", "xiaomi", "mimo", "tokens", "input", "model", "output", "coding"]
source: https://spectrumailab.com/blog/mimo-v2-6-guide-pricing-builds-2026
source_anchor: ""
source_lines: [1, 200]
section: "Xiaomi_MiMo"
---
Xiaomi MiMo V2.6 is an open-weight AI model family with Pro and Flash variants. Both accept text, images, video and audio and return text. On Xiaomi's overseas API, Pro costs $0.435 per million uncached input tokens and $0.87 per million output tokens; Flash costs $0.14 and $0.28. Pro is the capability-first option to evaluate for harder coding work. Flash is the lower-cost option for narrow, repeatable tasks. Xiaomi updated both API aliases on September 25 to reduce repeated tool calls, so check which version a review describes.

This guide was checked on **October 5, 2026** against official documentation, independent measurements and public build artifacts. We opened one browser demo and exercised its Paint tool. We did not run a paid API comparison or reproduce Xiaomi's benchmarks.

MiMo's token prices leave room for a lot of experimentation. A failed coding run can still take an hour of your time. The useful question is how often it finishes the task and how much correction it needs.

Below are the current prices, the differences between Pro and Flash, inspectable projects, and the API details that can trip up an existing agent setup.

## What is Xiaomi MiMo V2.6?

Xiaomi released **MiMo V2.6 Pro, Flash and Pro UltraSpeed on September 22**. Old posts about V2-Flash or V2.5 describe different releases. Start with the official release history if a review just says “MiMo.”

The current Pro and Flash pages both list text, image, video and audio input, text output, a one-million-token context window and up to 128K output tokens, including reasoning and the final answer. They support thinking and tool calls. A large context window gives you room to send material; it does not tell you how reliably the model will use every detail.

UltraSpeed is the expensive speed-focused API option. Xiaomi advertises up to 20 times Pro's output speed, with customized quota arrangements. That is a vendor claim about generating tokens. It does not establish that a complete agent job finishes 20 times faster once thinking, tools and retries are included.

Then there is **Distill-Qwen-9B**, a smaller downloadable research model based on Qwen3.5-9B. Keep it separate from the hosted Pro and Flash products when reading local-computer reviews.

## MiMo V2.6 Pro vs Flash: which should you try?

**Start by evaluating Pro for difficult, multi-step coding work and Flash for narrow, repeatable jobs with clear pass/fail checks.** Both have the same advertised context and input modalities. The useful tradeoff is task success, total waiting time and cost after retries.

| Option | Starting point | What to check | 
|---|---|---|
| V2.6 Pro | Harder coding changes and multi-step agent tasks | Whether fewer failed attempts justify its higher rates than Flash | 
| V2.6 Flash | High-volume, testable jobs and small coding tasks | Whether it passes your checks with little correction | 
| Pro UltraSpeed | Latency-sensitive work after measuring ordinary Pro | Whether less waiting is worth 10× Pro's token rates | 
| Distill-Qwen-9B | Local experimentation with a separate small model | Hardware, quantization and serving support; not a Pro substitute | 

These are evaluation starting points, not findings from a controlled comparison for this article. The benchmark section below includes a test where Flash outscored Pro.

## MiMo V2.6 API pricing

These are Xiaomi's published overseas USD rates on October 5, per one million tokens:

| Model | Uncached input | Cached input | Output | 
|---|---|---|---|
| V2.6 Pro | $0.435 | $0.0036 | $0.87 | 
| V2.6 Flash | $0.14 | $0.0028 | $0.28 | 
| V2.6 Pro UltraSpeed | $4.35 | $0.036 | $8.70 | 

The live table has no separate long-context price tier. Xiaomi removed input-length tiers in May, and the V2.6 announcement says these models retain the V2.5 rates. Older comparisons with a higher price above 256K tokens can mislead you.

For a concrete example, suppose an agent makes 20 calls, each averaging 50,000 input tokens and 2,000 total output tokens. That adds up to one million input tokens and 40,000 output tokens. With no cache hits, the model charge would be about **$0.47 on Pro or $0.15 on Flash**.

If 80% of that input qualifies for cached pricing, the same arithmetic becomes **$0.125 on Pro or $0.041 on Flash**. For Pro, that is 0.2 × $0.435 + 0.8 × $0.0036 + 0.04 × $0.87.

Those are example budgets, not measured costs of completing a coding task. The assumed cache rate might not happen. The models may use different numbers of tokens or require different amounts of correction. Reasoning counts toward the completion-token budget, so a short final answer can still have a long billable run behind it.

Search, external tools and hosting add costs too. Xiaomi's web-search documentation lists $5 per 1,000 overseas tool uses, and returned search text becomes model input. For independent jobs that can wait, Batch halves Pro and Flash token rates. A sequential coding agent usually needs the previous tool result before it can continue.

### The $6 plan needs a closer look

The individual Token Plan starts at $6 a month, followed by $16, $50 and $100 tiers. It uses credits with model-specific conversion rates. A headline like “4.1 billion credits” does not mean 4.1 billion Pro tokens.

The more important restriction: **the published plan rules exclude custom app backends and clearly non-coding automated API scripts**. It is a coding-tool subscription. Check the permitted use before building around it, and compare your expected usage with pay-as-you-go rather than assuming a subscription saves money.

For the wider pricing picture, use our AI API price index. Keep the currency, cache assumptions and workload the same when comparing.

## September tool-call update: what changed

On September 27, Xiaomi published a tool-call repetition diagnosis. The team acknowledged that its models could repeat tool calls and waste context and time. It described a targeted training-and-distillation update to reduce that behavior.

The revised Pro and Flash APIs went live on **September 25 at 06:00 Beijing time**, or September 24 at 22:00 UTC. Their API names stayed the same. A launch-day review and a test today can therefore say `mimo-v2.6-pro` while referring to different served versions.

The reported metric counts exact duplicate calls within a turn. Cross-turn loops, almost-identical calls and calls inside code-execution tools fall outside it. Lower scores do not prove that all repetition has disappeared.

Open Xiaomi's full-resolution chart to read the small labels.

The practical fix on your side is straightforward: log the provider, model ID, date, agent version and settings. Keep a step limit and a cost limit. Detect repeated actions. An inexpensive model still needs a way to stop when it gets stuck.

## Benchmarks: Xiaomi and independent tests

Xiaomi's technical report, Table 3, puts Pro near its selected closed-model baselines on some agent tests and behind on others:

| Xiaomi-reported benchmark | V2.6 Pro | V2.6 Flash | Claude Opus 5 | 
|---|---|---|---|
| DeepSWE v1.1 | 71.9 | 67.9 | 74.0 | 
| AutomationBench v1.0.6 | 53.1 | 52.3 | 50.3 | 
| OSWorld-Verified | 82.0 | 80.8 | 83.4 | 
| Terminal Bench 4.0 | 34.9 | 28.8 | 49.0 | 

These are vendor-reported scores against the specific baselines in the report. They are not a fresh comparison with every model available in October. The table also does not pin every result to a serving checkpoint and fully specified run. It appears after the report's distillation stage, while the RL-named model cards reuse the numbers. Calling the whole table “raw RL results” would be too confident.

For a separate measurement, Artificial Analysis showed Intelligence Index v4.3.2 scores of 46 for Pro and 38 for Flash in the October 5 snapshot. Its Terminal-Bench 4.0 results were 35% and 23%, respectively. Its AutomationBench-AA result went the other way: Flash scored 64% against Pro's 59%.

Even within one evaluator, the cheaper model can do better on a particular test. Keep Xiaomi's AutomationBench and AA's AutomationBench-AA results separate. Their labels and setups differ.

AA's ordinary Xiaomi endpoints showed about 41 output tokens a second for Pro and 56 for Flash in that snapshot. These are not UltraSpeed results. Its performance methodology uses a defined prompt size, output workload and testing location, with measurements normally summarized over 72 hours. Your agent's waiting time will also include its reasoning and tools.

For a broader task-by-task choice, our canonical model guide keeps the comparison in one place. This page is about evaluating MiMo on its own terms.

## Coding examples and public demos

There are public outputs worth opening. The examples below have different levels of proof, and two come from companies promoting their own tools. None establishes broad production adoption.

### A browser desktop with a working Paint window

Julian Goldie's MiMo V2.6 Pro gallery includes a browser desktop called NebulaOS. It has a dock and windows for Notes, Paint and Terminal. Goldie attributes the output to Pro through OpenRouter inside Agent OS and describes one-shot generation. Those generation conditions are his claim.

We opened the public desktop demo, clicked Paint and drew a pink stroke on the canvas. That interaction worked. We did not verify persistence, exports or every other app. The 3D wallpaper did not render in our browser. Goldie also sells Agent OS and an AI community, so this is a promotional gallery with an inspectable result.

### A small runner game, with the source attached

Command Code's team published Metro Rush's HTML source under a MiMo V2.6 Flash directory. It contains a 2D canvas runner with lane changes, jumping and sliding. The team's original post reports roughly four iterations and a $0.018 cost.

The useful bit is having source to inspect. The cost and generation history are publisher-reported, and we did not playtest the game. Its September 22 UTC commit also predates the API update. Despite the post's “3D” framing, this MiMo output is a 2D game. The same repository includes a Flash-attributed Flappy Chick, which is another artifact from the same team.

### A local utility on a 16 GB Mac

In a September 30 write-up, Dragos Roua ran **Distill-Qwen-9B** on an M1 MacBook Pro using MLX Core. He asked for a Python script to count words in files. He reports about four minutes of thinking, a working script and a stray line in the output.

His conclusion was modest: useful for small utilities or summaries, less appealing for a long coding-agent session on that machine. This is a firsthand account of the small distilled model. It tells us very little about hosted Pro's speed. We read his article; we did not independently run the script or verify the video.

### The complaints also have useful details

One launch-period Flash user reported more than an hour spent on a palette change through Hermes and Command Code. A reply in the same thread described better results on tightly scoped delegated tasks. Neither provided a controlled comparison.

A more actionable local-serving report described empty replies, missing reasoning-history replay and an unintended 2,048-token limit in a Flash-RL setup. This was a local Flash-RL deployment, not a test of Xiaomi's hosted API. The author says template and parser fixes made six test cases pass, and links a repository issue. That is a reminder to check the agent and serving code before blaming every failure on the weights. These reports predate Xiaomi's API revision and should not be presented as fresh tests of it.

## Open weights: RL, MOPD and the 9B download

Xiaomi's official V2.6 collection contains Pro-RL, Flash-RL, Pro-MOPD, Flash-MOPD and Distill-Qwen-9B. The Pro-MOPD and Flash-MOPD cards describe the repetition-focused update. The repositories carry MIT license metadata; hosted-service terms and any base-model obligations remain separate.

Pro-RL lists 1.02 trillion total parameters with 42 billion active per token. Flash-RL is about 310 billion total with 15 billion active. The active count describes how much of the mixture-of-experts model participates in a token; it does not mean you only need memory for those active weights.

The 9B model card describes an SFT checkpoint based on Qwen3.5-9B, trained on MiMo-generated agent data. Xiaomi also publishes a 7,780-row research task dataset and training recipes in its verl fork. These give researchers something concrete to work with. They do not amount to a release of the entire frontier training corpus or a promise that you can reproduce Pro's full training run.

Xiaomi's own demos go beyond coding: its materials-research case describes expert-guided candidate design and simulations for materials that could capture PFAS. The page explicitly says the candidates still need wet-lab validation. It is an expert-guided computational workflow, with physical validation still ahead.

## How to use the MiMo V2.6 API

Use the pay-as-you-go API product for an application backend. Xiaomi's first-call guide lists the OpenAI-compatible base URL as `https://api.xiaomimimo.com/v1`. The model IDs are `mimo-v2.6-pro`, `mimo-v2.6-flash` and `mimo-v2.6-pro-ultraspeed`; UltraSpeed quota arrangements need to be confirmed separately.

Create a key in the official console, store it in an environment variable, and keep it out of client-side code and repositories. The example below is adapted from the official Chat Completions reference. **It has not been executed for this article.** It disables thinking and sets a small completion limit for a simple connection check; a successful call can incur token charges.

```
curl https://api.xiaomimimo.com/v1/chat/completions \
  -H "api-key: $MIMO_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "mimo-v2.6-flash",
    "messages": [{"role": "user", "content": "Reply with a short greeting."}],
    "thinking": {"type": "disabled"},
    "max_completion_tokens": 256
  }'
```
This only checks basic connectivity. Before using tools or replaying a conversation, check the following compatibility details. The familiar API shape helps, but there are enough differences to break an existing integration.

1. **Keep the thinking history intact.** Xiaomi's dedicated guide says tool-calling conversations in thinking mode must preserve historical`reasoning_content` . Dropping it can produce a 400 error. An adapter that keeps only the visible answer needs attention.
2. **Set an output budget deliberately.** The Chat Completions reference counts reasoning and the final answer together. It also documents limitations such as automatic tool choice rather than full parity with every OpenAI option.
3. **Do not assume stateful Responses support.** Xiaomi's Responses reference lists`previous_response_id` , background mode and`context_management` as unsupported. Its non-`none` reasoning-effort values currently enable the same behavior; “high” is not evidence of a distinct compute setting.
4. **Check the runner, not just the model ID.** MiMo Code is an OpenCode-derived application with its own tools, permissions, memory and MCP integrations. Changing a model inside another agent will not automatically reproduce that environment. Our MCP guide explains that tool layer.
5. **Review the data terms before sending private work.** We did not verify an API-specific no-training or zero-retention commitment in this research. Keep an initial evaluation on non-sensitive material until the applicable terms and processing arrangements are clear.

#### Already using V2.5? Put the migration date on your calendar

Xiaomi's deprecation notice says `mimo-v2.5-pro` and `mimo-v2.5` retire on October 21, 2026 at 10:00 Beijing time, or 02:00 UTC. It lists no automatic replacement. Change the model ID, check the adapter, and rerun your tests before that date.

## How to evaluate MiMo on your own tasks

A useful first evaluation is a task you already understand: fix a known bug, add a small UI behavior, or extract a defined set of fields from a document. Write the acceptance checks before running it. Give Pro, Flash and your current model the same starting files and tool access.

Record whether the checks pass, how long the whole job takes, the billable tokens, repeated actions and any human corrections. Run more than one attempt. A single attractive screenshot is a poor way to choose the model that will maintain your app.

The next useful number is **cost per accepted result**. Add the failed attempts and the time spent fixing them. If MiMo clears your checks reliably, the pricing leaves room for a lot of experimentation. If it keeps revisiting the same file without moving the task forward, stop the run and inspect the trace.

## Sources and what was checked

Prices, specifications and API behavior were checked against Xiaomi's current pages on **October 5, 2026**. The four official screenshots are actual captures of those sources; the abstract banner is original generated artwork. Public model aliases and live benchmark pages can change.

- **Official product evidence:** release log, pricing, Pro specification and September 27 repetition analysis
- **Research and independent tests:** Xiaomi technical report, official weight collection, Artificial Analysis comparison and its intelligence methodology
- **Build evidence:** Goldie's public demo was opened and one Paint interaction checked; Command Code's pinned source and original post were inspected; Roua's dated account was read. Publisher timings, token costs and generation claims were not independently reproduced

## FAQ

### How much does MiMo V2.6 cost?

On October 5, 2026, Xiaomi lists Pro at $0.435 uncached input and $0.87 output per million tokens; Flash at $0.14 and $0.28; and Pro UltraSpeed at $4.35 and $8.70. Cached input has separate lower rates. These are API token prices, not complete task costs.

### Is MiMo V2.6 open source?

Xiaomi publishes Pro-RL, Flash-RL, Pro-MOPD, Flash-MOPD and Distill-Qwen-9B checkpoints with MIT license metadata, plus selected research tasks and training code. Open weights do not establish that every training input or production component has been released. Hosted API terms are separate.

### Can I run MiMo V2.6 locally?

Xiaomi publishes downloadable Pro and Flash checkpoints, but their total weights are much larger than the active-parameter counts. The separate Distill-Qwen-9B model is the more plausible starting point for local experimentation; hardware, quantization and serving support determine what will fit.

### Can I use the MiMo Token Plan for my app backend?

The published Token Plan restrictions exclude custom application backends and clearly non-coding automated API scripts. Use the appropriate pay-as-you-go API product for a backend and check its current terms.

### When does the MiMo V2.5 API retire?

Xiaomi says the `mimo-v2.5-pro` and `mimo-v2.5` aliases retire on October 21, 2026 at 10:00 Beijing time, or 02:00 UTC. No automatic replacement is listed. Migrate and test before that deadline.

### Stay ahead of the AI curve

We test new AI tools every week and share honest results. Join our newsletter.
