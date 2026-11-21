---
id: collect-261121-veille-ia/muse/muse-ember-meta-s-volume-model-at-the-same-0-10-and-0-50-as-haiku-5-5
title: "Muse Ember: Meta's Volume Model at the Same $0.10 and $0.50 as Haiku 5.5"
domain: muse
role: reference
task: article
actors: ["Meta"]
dates: []
keywords: ["meta", "muse", "haiku", "ember", "model", "luna", "same", "wonder"]
source: https://llm-stats.com/blog/research/muse-ember-launch
source_anchor: ""
source_lines: [1, 89]
section: "Muse"
---
Meta released Muse Ember on **November 9, 2026**. It is Meta's smallest Muse model, the first trained using Muse Wonder Full, and it ships at the same **$0.10 input / $0.50 output** list price as Claude Haiku 5.5 and GPT-6 Luna. The product story is volume work: tool calls, coding subtasks, routing, extraction, and long document parsing, with Muse Wonder or Muse Spark planning and Ember executing.

On Meta's self-reported, allowed launch rows at max reasoning effort, Ember is slightly ahead of Haiku 5.5 and clearly ahead of Luna. The real differentiators at that shared sticker are a cheaper cache ($0.004), a 200K cheap-tier window (twice Haiku's, short of Luna's), 524,288 max output (four times either peer), and a long-context rate that beats Haiku's and loses to Luna's. Default effort is max, so a default call bills more reasoning tokens than a default Haiku or Luna call. It is not a near-frontier model. See Meta's launch post and model docs.

## What Muse Ember is

Meta positions the Muse portfolio as **Wonder for the hardest problems, Spark for coding and agentic workflows, and Ember for repeatable coding and agentic tasks at scale**. Docs repeat the pairing line: the larger model plans, and Muse Ember handles the volume.

Recommended Ember work is bounded and repetitive: sub-agent coding and terminal commands, tool calls inside a larger workflow, routing and classification, and the same task over thousands of images, files, or records. For hard single-shot coding, knowledge-heavy questions, or visual coding, Meta says to use Spark or Wonder instead. Meta's choose-a-model docs also map Claude Code's `haiku` alias and sub-agent model to `muse-ember-1.0` (opus to Wonder, sonnet to Spark 1.3).

Ember is available today on the Meta Model API and Muse Code as `muse-ember-1.0`, pay-as-you-go and every Muse Code plan, in Standard mode only. Meta also says Muse Ember, Muse Spark, and Muse Wonder are available on Google Cloud and Oracle Cloud; Meta publishes no first-party per-token prices for those clouds, so treat that as availability only.

## Benchmarks

All scores below are **self-reported by Meta** at **max reasoning effort**, from the November 9, 2026 launch table. Methodology notes live in Meta's evaluation report. Several Haiku gaps are small. Terminal-Bench uses each model's best published harness, so it is not a common-harness comparison. This is a close Haiku-class result at the same list price, not a frontier claim.

### Allowed launch rows

Compact table of the same self-reported max-effort scores. Headline reading: ahead on every allowed row, close to Haiku on several, clearer lead on Personal Superintelligence Index and against Luna.

| Benchmark | Muse Ember | Haiku 5.5 | GPT-6 Luna | 
|---|---|---|---|
| Agents' Last Exam | 61.6% | 57.0% | 57.1% | 
| Terminal-Bench 4.0 | 40.9% | 39.2% | 16.4% | 
| OSWorld 2.1 partial | 69.5% | 68.9% | 50.5% | 
| OSWorld 2.1 strict | 33.8% | 27.8% | 17.0% | 
| PerceptionBench (with tools) | 80.6% | 78.6% | 62.8% | 
| Personal Superintelligence Index | 46.8 | 36.1 | 37.0 | 

- **Agents' Last Exam:** Meta internal, ALE v1.0 Linux subset. Five of 105 tasks excluded for every model. Common internal agent loop, not the official product-vs-product leaderboard. Mean task score with partial credit.
- **Terminal-Bench 4.0:** Ember in mini-swe-agent, Haiku 5.5 as Anthropic's self-reported Claude Code result, Luna as Codex on the Terminal-Bench leaderboard. Haiku gap is 1.7 points.
- **OSWorld 2.1:** Meta internal runner, 108 tasks, mean per-task partial credit plus a strict full-credit share. Haiku partial gap is 0.6 points.
- **PerceptionBench:** Meta internal, 3,000 questions, scored with tools, official judge. Haiku gap is 2.0 points.
- **Personal Superintelligence Index:** Meta's internal composite of eight personal and knowledge-work evaluations. Report the composite only.

## Pricing

The request's input-token count, cached tokens included, sets the Ember tier. A continued conversation via `previous_response_id` counts as input. Up to 200K input tokens: cached input $0.004, input $0.10, output $0.50. Over 200K, the **whole request**is billed at cached $0.012, input $0.30, output $1.50, not only for the tokens past 200K. Web search grounding is $2.50 per 1,000 search queries on top of token charges. On a Muse Code subscription, a request over 200K uses the plan's usage faster.

Ember is Standard only, matching Wonder on Model API. Meta publishes no cache-write, Batch, Flex, or Fast price for Ember. Do not invent any.

### Peer comparison at list rates

- Under 100K input, Ember, Haiku 5.5, and Luna share the $0.10 / $0.50 sticker. Ember's list-price edge in this band is cache reads at $0.004 vs $0.01.
- From 100K to 200K, Haiku is already on $0.50 / $2.50; Ember and Luna stay on $0.10 / $0.50.
- From 200K to 272K, Ember is on $0.30 / $1.50; Luna is still $0.10 / $0.50; Haiku is $0.50 / $2.50.
- Above 272K, Luna's long-context rate ($0.20 / $0.75) is cheaper than Ember's ($0.30 / $1.50).
- Ember's max output (524,288) is four times Haiku's and Luna's (128,000).
- Ember is 12.5x cheaper on input than Spark Standard ($0.10 vs $1.25) and 40x cheaper than Wonder ($0.10 vs $4.00). That is a list-price ratio, not a cost-per-task.

Default effort is max, and reasoning tokens are billed as output, so a default Ember call can cost more than a default Haiku or Luna call at the same per-token rate. There is no official allowed cost-per-task figure to quantify that. Treat it as a caveat.

Peer list prices: Anthropic pricing for Haiku 5.5 and OpenAI's GPT-6 Luna page. Family rates: Meta pricing and rate limits.

## Specs

| Detail | Value | 
|---|---|
| Model ID | `muse-ember-1.0` | 
| Context | 1,048,576 tokens | 
| Max output | 524,288 tokens (512K) | 
| Input | Text, image, video, audio, PDF | 
| Output | Text | 
| Reasoning effort | minimal, low, medium, high, xhigh, max. Default max. `none` returns HTTP 400. | 
| Endpoints | Responses, Chat Completions, Messages | 
| Rate limits | 3,000 requests per minute and 4,000,000 tokens per minute per team (same as Wonder and Spark Standard) | 
| Mode | Standard only (no Contributor) | 
| Cloud availability | Meta Model API, Muse Code, Google Cloud, Oracle Cloud (no official cloud token prices published) | 

Reasoning tokens stay private, count toward the output-token budget, and are billed as output. When a request sets no effort, Muse Wonder and Muse Ember use max and Muse Spark uses high. No param count, architecture, training tokens, or knowledge cutoff is published. More detail: reasoning docs, choose a model, and the developer model page.

## Safety

Before release, Meta evaluated Ember under its Meta Superintelligence Scaling Framework, including chemical and biological, cybersecurity, and loss-of-control risks, and says Ember remained below the framework's high-risk thresholds. It received the same core safety training as larger Muse models. Meta also evaluated agent behavior (user intent, sensitive information, indirect instructions, asking for human input before high-impact actions). No numeric safety scores were published.

Banner: Meta. Abstract illustration of blue orbital curves, arrows and points looping out from a single center.

## FAQ

- Meta released**Muse Ember** on**November 9, 2026** . The API id is`muse-ember-1.0` . It is available on the Meta Model API and Muse Code (pay-as-you-go and every Muse Code plan), and Meta also lists availability on Google Cloud and Oracle Cloud.
- Up to 200K input tokens:**$0.10 input** ,**$0.004 cached input** , and**$0.50 output** per million tokens. That short-context sticker matches**Claude Haiku 5.5** and**GPT-6 Luna** . Over 200K input, the whole request bills at**$0.30 / $0.012 / $1.50** . Web search grounding is**$2.50 per 1,000 search queries** on top of token charges. Meta publishes no Batch, Flex, or Fast price for Ember.
- Context is**1,048,576 tokens** . Max output is**524,288 tokens** (the launch post's "512K"      is the same number). That max output is four times Haiku 5.5 and GPT-6 Luna (128,000).
- **No.** On Model API, Muse Ember runs in Standard mode only. Muse Wonder is also Standard only. Muse Spark 1.3 and 1.2 offer a Contributor tier ($0.10 / $0.002 / $0.20) in exchange for permission to use content to train future Meta models. That option does not apply to Ember.
- Same short-context list price ($0.10 / $0.50). On Meta's allowed self-reported rows at max effort, Ember is slightly ahead (for example Agents' Last Exam 61.6% vs 57.0%, Terminal-Bench 4.0 40.9% vs 39.2% under different harnesses). Ember's cache read is cheaper ($0.004 vs $0.01), its cheap tier lasts to 200K vs Haiku's 100K, and max output is 524,288 vs 128K. Ember defaults to max effort; Haiku defaults to medium. Haiku has Batch; Ember does not. See our Claude Haiku 5.5 launch post.
- Same short-context sticker ($0.10 / $0.50). Ember leads on the allowed self-reported rows at max effort, with a clearer gap on Terminal-Bench 4.0 and OSWorld. Luna's long-context threshold is higher (272K vs Ember's 200K), and Luna's long-context rate ($0.20 / $0.75) is cheaper than Ember's ($0.30 / $1.50). Luna defaults to medium effort. Luna accepts text and image input only; Ember also accepts video, audio, and PDF.
- **Not for the hardest work.** Meta's guidance is that the larger model plans and Muse Ember handles the volume: sub-agents, tool calls, routing, and the same bounded job over many files. For hard single-shot coding, knowledge-heavy questions, or visual coding, Meta says to use Muse Spark or Muse Wonder.
- **No.** It is Meta's smallest Muse model, built for high-volume repeated agent work at a familiar Haiku-class list price. It is a volume executor in the Muse lineup, not a new capability class.

Continue Reading
