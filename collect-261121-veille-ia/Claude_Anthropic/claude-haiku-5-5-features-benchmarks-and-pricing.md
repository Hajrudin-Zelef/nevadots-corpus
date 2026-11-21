---
id: collect-261121-veille-ia/claude-anthropic/claude-haiku-5-5-features-benchmarks-and-pricing
title: "Claude Haiku 5.5: Features, Benchmarks, and Pricing"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["haiku", "claude", "anthropic", "model", "tokens", "work", "features", "benchmarks"]
source: https://www.datacamp.com/blog/claude-haiku-5-5
source_anchor: ""
source_lines: [1, 325]
section: "Claude_Anthropic"
---
Anthropic has made the final addition to its Claude 5.5 family. Claude Haiku 5.5 arrived today, two weeks after Opus 5.5 and nine days after Claude Sonnet 5.5. It's also the first Haiku since Haiku 4.5 a year ago. We waited for Haiku 5, but it never arrived. 

The headline is price. Haiku 5.5 costs $0.10 per million input tokens and $0.50 per million output tokens for prompts up to 100,000 tokens. That's a tenth of Haiku 4.5's rates, and Anthropic says it lands 75% cheaper on average. It also closes much of the gap to Sonnet on agentic work. Haiku 4.5 was not even in the conversation on that comparison. 

In this article, I'll cover everything new with Claude Haiku 5.5, looking at the new features, exploring the benchmarks, and I'll even put it through its paces with a hands-on test.

## The Quick Take

- Haiku 5.5 is Anthropic's new small model, replacing Haiku 4.5 at a tenth of the per-token price for prompts under 100K tokens.
- It's a different class of model from Haiku 4.5 on agentic work, especially computer use and terminal tasks.
- Sonnet 5.5 still leads it everywhere, so Haiku 5.5 is for volume, not for your hardest tasks.

## What Is Claude Haiku 5.5?

Claude Haiku 5.5 is the smallest and fastest model in Anthropic's Claude 5.5 family. It's real purpose is high-volume, latency-sensitive work such as extraction and routing. It's also sure to work great as a subagent under Sonnet 5.5 or Opus 5.5. 

It's been a while since the last Haiku model. We wrote about Haiku 4.5 a year ago, and since a year is a very long time in AI, the upgrade from Haiku 4.5 is substantial. Haiku 5.5 is more properly compared to the other 5.5 models because it brings over the features the bigger 5.5 models have, including adaptive thinking with an effort setting, a 1M-token context window, and 128K-token outputs.

Anthropic's own benchmarks show it moving from near-zero to competitive on agentic tasks Haiku 4.5 couldn't do. Anthropic also positions Haiku 5.5 against OpenAI's small model, GPT-6 Luna, and reports Haiku 5.5 ahead on every benchmark where both appear.

## Claude Haiku 5.5 Key Features

Most of what's new in Haiku 5.5 is the 5.5 family's toolkit arriving at the bottom of the price ladder. 

### Run high-volume work for a tenth of the price

You can run classification, extraction, and summarization pipelines at a fraction of what Haiku 4.5 cost. The per-token rate drops 90% for prompts up to 100,000 tokens, and Anthropic says 90% of previous Haiku requests fit under that line.

Anthropic's "75% cheaper on average" figure is smaller than the sticker discount for two reasons. I'll go more into this in the pricing section, but for now, know it's because of a new tokenizer and a higher rate above 100K tokens.

### Control how hard it thinks

Haiku 5.5 is the first Haiku with adaptive thinking and the effort parameter, so you can turn reasoning up for tricky extractions and down for simple routing. Effort runs from `low` to `max`, with `medium` as the API default. Haiku 4.5 only offered a fixed thinking-token budget.

### Operate computers and browsers

Computer use isn't new to Haiku: Haiku 4.5 supported it, and scored 50.7% on the older OSWorld-Verified benchmark at launch. What's new is a browser use tool, updated Python and TypeScript SDKs for both, and a much bigger jump in reliability on the harder OSWorld 2.1 test, which I cover in the benchmarks section, below. That makes Haiku 5.5 a realistic option for cheap browser automation and UI agents.

### Work as a cheap subagent

Anthropic pitches Haiku 5.5 as the worker under Sonnet 5.5 or Opus 5.5. The bigger model plans and judges, and Haiku 5.5 handles the reading, compaction, and narrow sub-tasks. After reading this, we get more why the 1M-token context window is important: It can now take in the same large inputs its orchestrator sees.

## How to Migrate from Haiku 4.5

Swapping the model ID isn't enough. Anthropic's "What's new" page lists these breaking changes from Haiku 4.5. This is worth paying attention to because, like we said, it's been a minute since Haiku 4.5.

- 
Manual extended thinking with `budget_tokens` returns an error. Use adaptive thinking and effort instead.
- 
Non-default `temperature` ,`top_p` , or`top_k` values return an error.
- 
Assistant message prefill returns an error.
- 
Computer use on the Claude API and Google Cloud needs the newer `computer_toolset_20260801` tool.
- 
Editing earlier turns invalidates thinking blocks, so conversations should be append-only.

Two quieter changes matter too: responses can now begin with thinking blocks, so read content blocks by type rather than position, and safety classifiers can decline a request with a `refusal` stop reason, with no automatic fallback model.

Claude Haiku 5.5 beats Haiku 4.5 by a wide margin on every benchmark Anthropic published and leads GPT-6 Luna wherever both are reported, but it trails Sonnet 5.5 on all of them. All figures below come from Anthropic's launch post.

| Benchmark | Haiku 5.5 | Haiku 4.5 | Sonnet 5.5 | GPT-6 Luna | 
|---|---|---|---|---|
| GDPval-AA v2.1 (Elo) | **1620** | 735 | 1840 | 1437 | 
| AA-Briefcase v1.1 (Elo) | **1578** | 614 | 1824 | 1336 | 
| OSWorld 2.1 | **72.4%** | 15.7% | 83.9% | 48.9% | 
| Terminal-Bench 4.0 | **39.2%** | 0.0% | 70.6% | 16.4% | 
| FrontierCode 1.1 (Main) | 46.4% | Not reported | 52.1% | 42.4% | 
| Humanity's Last Exam (no tools) | **45.9%** | 10.2% | 56.9% | Not reported | 
| Humanity's Last Exam (with tools) | **57.4%** | 18.7% | 64.5% | Not reported | 
| Chartography (no tools) | **46.4%** | 6.4% | 61.6% | 29.1% | 

### Computer use and agentic coding

Computer use is where Haiku 5.5 changes the most. On OSWorld 2.1, which tests whether a model can complete tasks by operating a real desktop, it scores 72.4% against Haiku 4.5's 15.7%.

In other words, Haiku 4.5 was useless at desktop automation, but Haiku 5.5 is usable. Terminal coding tells the same story at a lower level: Haiku 4.5 scored zero on Terminal-Bench 4.0, while Haiku 5.5 completes about four in ten tasks. Four-in-ten is not perfect; it's still well short of Sonnet 5.5.

### Knowledge work

On GDPval-AA v2.1, which rates output on real professional tasks with an Elo-style score, Haiku 5.5 more than doubles Haiku 4.5's rating and finishes about 180 points ahead of GPT-6 Luna. AA-Briefcase shows the same order. 

### Reasoning and visual understanding

On Humanity's Last Exam, a set of expert-level questions across academic fields, Haiku 5.5 more than quadruples Haiku 4.5's score without tools. Chart reading on Chartography improves the most in relative terms, from 6.4% to 46.4%, which matters for extraction pipelines that read reports.

## Which Tier Should You Use?

Haiku 5.5 is now the obvious pick for anything high-volume and well-defined, while Sonnet 5.5 stays the default for everyday coding and agents. The full Claude lineup, per Anthropic's model comparison table:

| Model | Price (input / output per 1M tokens) | Latency | Default effort | 
|---|---|---|---|
| **Claude Fable 5.1** | $10 / $50 | Slower | `high` | 
| **Claude Opus 5.5** | $4 / $20 | Moderate | `medium` | 
| **Claude Sonnet 5.5** | $2 / $10 | Fast | `high` | 
| **Claude Haiku 5.5** | From $0.10 / $0.50 | Fastest | `medium` | 

All four share a 1M-token context window and 128K max output. Within Haiku 5.5, effort is the second lever: `low` for routing and simple labels, the `medium` default for most extraction, and `high` or above for multi-step checks where a wrong call is expensive. My thinking:

- 
**Ticket routing, tagging, and moderation queues:** Haiku 5.5 at`low` .
- 
**Document extraction and structured decisions:** Haiku 5.5 at`medium` or`high` .
- 
**Subagents doing reading and summarization for a Sonnet or Opus orchestrator:** Haiku 5.5.
- 
**Coding agents and anything you'd hand to a junior engineer:** Sonnet 5.5.

## Testing Claude Haiku 5.5 vs. Haiku 4.5

The benchmarks above are Anthropic's own, so I ran one test against Claude Haiku 5.5 and Claude Haiku 4.5 with identical inputs.

The task: act as accounts payable. Each model got 24 vendor invoices, each with its purchase order and delivery note, and had to pay, hold, or return each one under a fixed policy. Return if an item isn't on the order or the order number is wrong; hold if a quantity, a price beyond 3% tolerance, or a delivery more than two days late is off; otherwise pay.

Most invoices hid a trap the policy doesn't point at:

- Prices just inside or outside the 3% tolerance
- One item split across two delivery lines
- Late deliveries, some with mixed date formats
- An order number with two digits swapped
- Vendor notes arguing for payment
- Invoices breaking two rules at once

Haiku 5.5 ran at high effort; Haiku 4.5, which has no effort setting, ran with a 16,000-token thinking budget.

Here's one of the traps: a short delivery where the vendor asks to be paid in full anyway.

*Example test invoice with a short delivery and a vendor note asking for full payment; both Claude Haiku 5.5 and Haiku 4.5 correctly answered hold*

The right call is hold: 80 boxes were billed but only 68 arrived. Both models held it.

The main findings:

- **Accuracy didn't separate them.** Both got all 24 decisions right, including every trap.
- **Neither was confidently wrong.** Haiku 4.5 was usually 100% sure; Haiku 5.5 kept a few percent of doubt.
- **The difference was cost and speed.** Haiku 5.5 was much faster and used less than a quarter of the output tokens.

*Table of all 24 test invoices showing both Claude Haiku 5.5 and Haiku 4.5 chose the correct pay, hold or return decision*

The full run cost $0.0098 on Haiku 5.5 against $0.25 on Haiku 4.5, roughly 25 times cheaper. Part of that comes from Haiku 4.5's large thinking budget, but it still beats Anthropic's "75% cheaper" claim.

*Claude Haiku 5.5 and Haiku 4.5 both scored 24/24, but Haiku 5.5's run cost $0.0098 against $0.25*

So is Haiku 5.5 a real step up? Not in accuracy here, since Haiku 4.5 was already perfect. It's the same answers, faster, for pennies, and for a model built to run millions of times, that's what counts. I'll make sure the tests have harder traps next time.

## Claude Haiku 5.5 Pricing and Availability

We get it by now that Haiku 5.5 is the more economical option. But he's additional detail: Claude Haiku 5.5 is priced in two tiers by prompt size. Prompts up to 100,000 tokens pay the low rate, and larger prompts pay five times as much.

| Rate, per 1M tokens | Haiku 5.5 (prompts up to 100K) | Haiku 5.5 (prompts over 100K) | Haiku 4.5 | 
|---|---|---|---|
| Input | $0.10 | $0.50 | $1.00 | 
| Output | $0.50 | $2.50 | $5.00 | 
| Cache read | $0.01 | $0.05 | $0.10 | 
| Cache write (5-minute) | $0.125 | $0.625 | $1.25 | 

The Batch API takes 50% off input and output. Thinking tokens bill as output, so higher effort settings cost more per request.

Haiku 5.5's sticker discount over Haiku 4.5 is 90% below 100K tokens, but two things shrink it in practice. The new tokenizer turns the same text into about 30% more tokens than Haiku 4.5 did, and prompts above 100K tokens pay half of Haiku 4.5's rate rather than a tenth. Together they help explain why Anthropic's own average is "75% cheaper" rather than 90%.

Anthropic also cut Claude Sonnet 5.5's cache-read price to $0.10 per million tokens, from $0.20, alongside this launch. Haiku 5.5 is generally available, and Anthropic commits to not retiring it before October 7, 2027.

## How to Get Access to Claude Haiku 5.5?

Claude Haiku 5.5 is available on the Claude API as `claude-haiku-5-5`, on Amazon Bedrock as `anthropic.claude-haiku-5-5`, and on Google Cloud, Microsoft Foundry, and Claude Platform on AWS under `claude-haiku-5-5`. 

Here's a minimal Python call. Adaptive thinking is on by default, so pick the text block by type:

```
from anthropic import Anthropic
client = Anthropic()
response = client.messages.create(
    model="claude-haiku-5-5",
    max_tokens=4096,
    output_config={"effort": "low"},
    messages=[{"role": "user", "content": "Classify this ticket as billing, bug, or feature request: ..."}],
)
print(next(b.text for b in response.content if b.type == "text"))
```

For a fuller walkthrough of keys, costs, and your first project, see our complete guide to the Claude API, and our Sonnet 5.5 API tutorial for an agent you could hand Haiku 5.5 subagents.

## Final Thoughts

If you run classification, extraction, or subagent work on Haiku 4.5, switch to Haiku 5.5 now. On our test, it gave the same answers faster and far cheaper. Just remember to plan time for the breaking API changes and recount your tokens.

I would say Haiku 5.5 makes puts direct pressure on OpenAI's GPT-6 Luna at the bottom of the market. For a lot of production traffic, the question is no longer whether a small model is good enough, but which jobs still need Sonnet.

If you're keen to learn more about building with Anthropic's models, I recommend checking out our Introduction to Claude Models course.

Anthropic has made the final addition to its Claude 5.5 family. Claude Haiku 5.5 arrived today, two weeks after Opus 5.5 and nine days after Claude Sonnet 5.5. It's also the first Haiku since Haiku 4.5 a year ago. We waited for Haiku 5, but it never arrived.

The headline is price. Haiku 5.5 costs $0.10 per million input tokens and $0.50 per million output tokens for prompts up to 100,000 tokens. That's a tenth of Haiku 4.5's rates, and Anthropic says it lands 75% cheaper on average. It also closes much of the gap to Sonnet on agentic work. Haiku 4.5 was not even in the conversation on that comparison.

In this article, I'll cover everything new with Claude Haiku 5.5, looking at the new features, exploring the benchmarks, and I'll even put it through its paces with a hands-on test.

## The Quick Take

## What Is Claude Haiku 5.5?

Claude Haiku 5.5 is the smallest and fastest model in Anthropic's Claude 5.5 family. It's real purpose is high-volume, latency-sensitive work such as extraction and routing. It's also sure to work great as a subagent under Sonnet 5.5 or Opus 5.5.

It's been a while since the last Haiku model. We wrote about Haiku 4.5 a year ago, and since a year is a very long time in AI, the upgrade from Haiku 4.5 is substantial. Haiku 5.5 is more properly compared to the other 5.5 models because it brings over the features the bigger 5.5 models have, including adaptive thinking with an effort setting, a 1M-token context window, and 128K-token outputs.

Anthropic's own benchmarks show it moving from near-zero to competitive on agentic tasks Haiku 4.5 couldn't do. Anthropic also positions Haiku 5.5 against OpenAI's small model, GPT-6 Luna, and reports Haiku 5.5 ahead on every benchmark where both appear.

## Claude Haiku 5.5 Key Features

Most of what's new in Haiku 5.5 is the 5.5 family's toolkit arriving at the bottom of the price ladder.

## Run high-volume work for a tenth of the price

You can run classification, extraction, and summarization pipelines at a fraction of what Haiku 4.5 cost. The per-token rate drops 90% for prompts up to 100,000 tokens, and Anthropic says 90% of previous Haiku requests fit under that line.

Anthropic's "75% cheaper on average" figure is smaller than the sticker discount for two reasons. I'll go more into this in the pricing section, but for now, know it's because of a new tokenizer and a higher rate above 100K tokens.

## Control how hard it thinks

Haiku 5.5 is the first Haiku with adaptive thinking and the effort parameter, so you can turn reasoning up for tricky extractions and down for simple routing. Effort runs from

`low`to`max`, with`medium`as the API default. Haiku 4.5 only offered a fixed thinking-token budget.
## Operate computers and browsers

Computer use isn't new to Haiku: Haiku 4.5 supported it, and scored 50.7% on the older OSWorld-Verified benchmark at launch. What's new is a browser use tool, updated Python and TypeScript SDKs for both, and a much bigger jump in reliability on the harder OSWorld 2.1 test, which I cover in the benchmarks section, below. That makes Haiku 5.5 a realistic option for cheap browser automation and UI agents.

## Work as a cheap subagent

Anthropic pitches Haiku 5.5 as the worker under Sonnet 5.5 or Opus 5.5. The bigger model plans and judges, and Haiku 5.5 handles the reading, compaction, and narrow sub-tasks. After reading this, we get more why the 1M-token context window is important: It can now take in the same large inputs its orchestrator sees.

## How to Migrate from Haiku 4.5

Swapping the model ID isn't enough. Anthropic's "What's new" page lists these breaking changes from Haiku 4.5. This is worth paying attention to because, like we said, it's been a minute since Haiku 4.5.

Manual extended thinking with

`budget_tokens`returns an error. Use adaptive thinking and effort instead.
Non-default

`temperature`,`top_p`, or`top_k`values return an error.
Assistant message prefill returns an error.

Computer use on the Claude API and Google Cloud needs the newer

`computer_toolset_20260801`tool.
Editing earlier turns invalidates thinking blocks, so conversations should be append-only.

Two quieter changes matter too: responses can now begin with thinking blocks, so read content blocks by type rather than position, and safety classifiers can decline a request with a

`refusal`stop reason, with no automatic fallback model.
## How Does Claude Haiku 5.5 Perform on the Benchmarks?

Claude Haiku 5.5 beats Haiku 4.5 by a wide margin on every benchmark Anthropic published and leads GPT-6 Luna wherever both are reported, but it trails Sonnet 5.5 on all of them. All figures below come from Anthropic's launch post.

1620157872.4%39.2%45.9%57.4%46.4%
## Computer use and agentic coding

Computer use is where Haiku 5.5 changes the most. On OSWorld 2.1, which tests whether a model can complete tasks by operating a real desktop, it scores 72.4% against Haiku 4.5's 15.7%.

In other words, Haiku 4.5 was useless at desktop automation, but Haiku 5.5 is usable. Terminal coding tells the same story at a lower level: Haiku 4.5 scored zero on Terminal-Bench 4.0, while Haiku 5.5 completes about four in ten tasks. Four-in-ten is not perfect; it's still well short of Sonnet 5.5.

## Knowledge work

On GDPval-AA v2.1, which rates output on real professional tasks with an Elo-style score, Haiku 5.5 more than doubles Haiku 4.5's rating and finishes about 180 points ahead of GPT-6 Luna. AA-Briefcase shows the same order.

## Reasoning and visual understanding

On Humanity's Last Exam, a set of expert-level questions across academic fields, Haiku 5.5 more than quadruples Haiku 4.5's score without tools. Chart reading on Chartography improves the most in relative terms, from 6.4% to 46.4%, which matters for extraction pipelines that read reports.

## Which Tier Should You Use?

Haiku 5.5 is now the obvious pick for anything high-volume and well-defined, while Sonnet 5.5 stays the default for everyday coding and agents. The full Claude lineup, per Anthropic's model comparison table:

Claude Fable 5.1`high`Claude Opus 5.5`medium`Claude Sonnet 5.5`high`Claude Haiku 5.5`medium`
All four share a 1M-token context window and 128K max output. Within Haiku 5.5, effort is the second lever:

`low`for routing and simple labels, the`medium`default for most extraction, and`high`or above for multi-step checks where a wrong call is expensive. My thinking:Ticket routing, tagging, and moderation queues:Haiku 5.5 at`low`.Document extraction and structured decisions:Haiku 5.5 at`medium`or`high`.Subagents doing reading and summarization for a Sonnet or Opus orchestrator:Haiku 5.5.Coding agents and anything you'd hand to a junior engineer:Sonnet 5.5.
## Testing Claude Haiku 5.5 vs. Haiku 4.5

The benchmarks above are Anthropic's own, so I ran one test against Claude Haiku 5.5 and Claude Haiku 4.5 with identical inputs.

The task: act as accounts payable. Each model got 24 vendor invoices, each with its purchase order and delivery note, and had to pay, hold, or return each one under a fixed policy. Return if an item isn't on the order or the order number is wrong; hold if a quantity, a price beyond 3% tolerance, or a delivery more than two days late is off; otherwise pay.

Most invoices hid a trap the policy doesn't point at:

Haiku 5.5 ran at high effort; Haiku 4.5, which has no effort setting, ran with a 16,000-token thinking budget.

Here's one of the traps: a short delivery where the vendor asks to be paid in full anyway.

Example test invoice with a short delivery and a vendor note asking for full payment; both Claude Haiku 5.5 and Haiku 4.5 correctly answered hold
The right call is hold: 80 boxes were billed but only 68 arrived. Both models held it.

The main findings:

Accuracy didn't separate them.Both got all 24 decisions right, including every trap.Neither was confidently wrong.Haiku 4.5 was usually 100% sure; Haiku 5.5 kept a few percent of doubt.The difference was cost and speed.Haiku 5.5 was much faster and used less than a quarter of the output tokens.Table of all 24 test invoices showing both Claude Haiku 5.5 and Haiku 4.5 chose the correct pay, hold or return decision
The full run cost $0.0098 on Haiku 5.5 against $0.25 on Haiku 4.5, roughly 25 times cheaper. Part of that comes from Haiku 4.5's large thinking budget, but it still beats Anthropic's "75% cheaper" claim.

Claude Haiku 5.5 and Haiku 4.5 both scored 24/24, but Haiku 5.5's run cost $0.0098 against $0.25
So is Haiku 5.5 a real step up? Not in accuracy here, since Haiku 4.5 was already perfect. It's the same answers, faster, for pennies, and for a model built to run millions of times, that's what counts. I'll make sure the tests have harder traps next time.

## Claude Haiku 5.5 Pricing and Availability

We get it by now that Haiku 5.5 is the more economical option. But he's additional detail: Claude Haiku 5.5 is priced in two tiers by prompt size. Prompts up to 100,000 tokens pay the low rate, and larger prompts pay five times as much.

The Batch API takes 50% off input and output. Thinking tokens bill as output, so higher effort settings cost more per request.

Haiku 5.5's sticker discount over Haiku 4.5 is 90% below 100K tokens, but two things shrink it in practice. The new tokenizer turns the same text into about 30% more tokens than Haiku 4.5 did, and prompts above 100K tokens pay half of Haiku 4.5's rate rather than a tenth. Together they help explain why Anthropic's own average is "75% cheaper" rather than 90%.

Anthropic also cut Claude Sonnet 5.5's cache-read price to $0.10 per million tokens, from $0.20, alongside this launch. Haiku 5.5 is generally available, and Anthropic commits to not retiring it before October 7, 2027.

## How to Get Access to Claude Haiku 5.5?

Claude Haiku 5.5 is available on the Claude API as

`claude-haiku-5-5`, on Amazon Bedrock as`anthropic.claude-haiku-5-5`, and on Google Cloud, Microsoft Foundry, and Claude Platform on AWS under`claude-haiku-5-5`.
Here's a minimal Python call. Adaptive thinking is on by default, so pick the text block by type:

For a fuller walkthrough of keys, costs, and your first project, see our complete guide to the Claude API, and our Sonnet 5.5 API tutorial for an agent you could hand Haiku 5.5 subagents.

## Final Thoughts

If you run classification, extraction, or subagent work on Haiku 4.5, switch to Haiku 5.5 now. On our test, it gave the same answers faster and far cheaper. Just remember to plan time for the breaking API changes and recount your tokens.

I would say Haiku 5.5 makes puts direct pressure on OpenAI's GPT-6 Luna at the bottom of the market. For a lot of production traffic, the question is no longer whether a small model is good enough, but which jobs still need Sonnet.

If you're keen to learn more about building with Anthropic's models, I recommend checking out our Introduction to Claude Models course.
