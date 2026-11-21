---
id: collect-261121-veille-ia/grok-xai/grok-4-7-vs-grok-4-6-what-changed-and-when-to-switch
title: "Grok 4.7 vs Grok 4.6: What changed and when to switch"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: []
keywords: ["grok", "work", "model", "same", "switch", "effort", "according", "tasks"]
source: https://www.fullstack.com/blog/grok-4-7-vs-grok-4-6-what-changed-and-when-to-switch
source_anchor: ""
source_lines: [1, 267]
section: "Grok_xAI"
---
Grok 4.7 vs Grok 4.6: What changed and when to switch

Written by

FullStack Team

Last updated on:

November 13, 2026

xAI's newest model costs the same as the one it replaces and does much better on agentic work. It also scores lower on advanced math, which is worth knowing before you switch everything over.

xAI released Grok 4.7 on September 21, 2026, about six weeks after Grok 4.6 arrived on August 12. xAI describes it as "a notable improvement over Grok 4.6 at the same price and speed," and on most of our benchmark it is. Grok 4.7 nearly doubles Grok 4.6's Terminal-Bench 4.0 score and moves from rank 22 to rank 11 on FullStack's RFE leaderboard.

Both are Grok 4 models at the same price, so this new model is a straight swap in most code. You change the model ID from grok-4.6 to grok-4.7, and the rest of your request can stay the same.

The surprise is math. On FrontierMath, Grok 4.7 scores 13 points lower than Grok 4.6, and it also slips slightly on GPQA Diamond. For most teams, the upgrade is an easy call. For teams that lean on Grok for hard quantitative work, it's worth testing first.

What's the difference between Grok 4.7 and Grok 4.6?

On paper, the two models look almost identical. Both have a 500,000-token context window, both cost $2 input / $6 output per million tokens, and both support the same four reasoning effort levels (low, medium, high, and xhigh), according to xAI's developer docs and its Grok 4.6 announcement.

The differences are in how the model works. xAI says Grok 4.7 spends longer on hard problems and checks its own answers more often than Grok 4.6, according to Decrypt. xAI's docs describe it as a model "built for coding, agentic tasks, and knowledge work," while Grok 4.6 was positioned for long-running agents and interactive, visual work.

Grok 4.7 vs Grok 4.6: release date, context window, price, reasoning effort levels, and stated focus

Specification

Grok 4.7

Grok 4.6

Release date

September 21, 2026

August 12, 2026

Context window

500,000 tokens

500,000 tokens

Price per million tokens

$2 input / $6 output

$2 input / $6 output

Reasoning effort levels

Low, medium, high, xhigh

Low, medium, high, xhigh

xAI's stated focus

Coding, agentic tasks, and knowledge work

Long-running agents and interactive, visual work

One practical detail to keep in mind is that reasoning can't be turned off on Grok 4.7, according to Oracle's model documentation. Higher effort levels can also increase response time, so it's worth setting the level per task rather than leaving everything on the default.

What's new in Grok 4.7?

Most of the changes are in training. Grok 4.7 uses a new, larger base model than Grok 4.6, and xAI trained it with a longer reinforcement learning run on a harder mix of tasks, weighted toward problems that take many hours to complete, according to Unite.AI's coverage of the launch. xAI hasn't listed the parameter count in its docs, though Decrypt reports 2.1 trillion parameters, up from 1.5 trillion for Grok 4.6.

xAI says the result is a model that's better at checking its own work and managing longer context, and at creating documents and presentations. That fits the kind of difficult tasks agents run into on long jobs, where a small early mistake can throw off hours of work.

The clearest example in xAI's launch figures is legal work. On the Harvey Legal Agent Benchmark, which tests how well a legal agent completes realistic legal tasks, xAI reports that Grok 4.7 scores 19.6%, compared with 15.8% for Grok 4.6. In the same comparison, it reports 2.5% for GPT-5.6 Sol and 6.7% for Claude Fable 5.1. These are company-reported results, so treat them as a starting point rather than a final answer.

xAI also reports gains at high effort on DeepSWE, a coding benchmark: 71.0% for Grok 4.7, compared with 65.2% for Grok 4.6.

What tools and inputs does Grok 4.7 support?

Grok 4.7 accepts text and image input and returns text, according to xAI's docs. It supports function calling, web search, X search, and code execution, and Oracle's documentation confirms support for structured outputs. For agents that make many tool calls, xAI recommends setting a prompt cache key and using context compaction on long loops.

It's available in the Grok API, Cursor, and Grok Build, where it's the default model, and through third-party coding tools and cloud platforms. Rate limits for your team are listed on xAI's pricing and model pages.

Is Grok 4.7 safer than Grok 4.6?

xAI says it built Grok 4.7 with a new safeguard stack, according to Unite.AI, and calls it the strongest model it has tested on refusals and jailbreak resistance. On its own HackerBench test, xAI reports that Grok 4.7 let through 3.3% of risky dual-use prompts while rarely blocking legitimate security work. xAI also gives select cybersecurity partners invite-only access to the model's red-team capabilities for defense research.

It’s worth noting that these are xAI's own measurements. If safety matters for your use case, test the model with your own prompts and policies before you rely on it.

How do Grok 4.7 and Grok 4.6 compare on benchmarks?

Our RFE benchmark weighs capability alongside speed and price in a single score. Here's how the two models compare:

Grok 4.7 vs Grok 4.6: RFE benchmark rank, scores, and individual benchmark results

Benchmark

Grok 4.7

Grok 4.6

RFE rank

11

22

RFE score

50.6

46.6

Capability score

52.0

44.3 (below threshold)

Terminal-Bench 4.0

37.6%

20.3%

SimpleQA Verified

56.0%

49.3%

Arena Text

1441

1455

Arena WebDev

1637

1616

GPQA Diamond

92.7%

94.0%

FrontierMath Tiers 1-3

53.0%

66.0%

SWE-Bench Pro

Not tested yet

Not tested yet

Grok 4.6's capability score of 44.3 falls below the threshold the RFE benchmark uses, so it ranks after every model that reaches it. That's a big part of why the two models sit 11 places apart, even though their RFE scores differ by four points.

Where Grok 4.7 pulls ahead

The biggest gain is on Terminal-Bench 4.0, which tests whether a model can complete real tasks in a command-line environment. Grok 4.7 scores 37.6%, compared with 20.3% for Grok 4.6. That's the kind of multistep, tool-using work that agentic coding depends on, and it matches xAI's focus for this release. xAI's own launch figures put Grok 4.7 at 38.0% on Terminal-Bench 4.0, close to our number, and ahead of GPT-5.6 Sol (37.3%) but behind Claude Fable 5.1 (57.9%).

Grok 4.7 also does better on SimpleQA Verified (56.0% vs. 49.3%), which measures how often a model gets short factual questions right. A higher score here usually means fewer confident wrong answers. It also edges ahead on Arena WebDev, where people vote on which model builds better web apps.

Where Grok 4.6 still leads

FrontierMath is the surprise. It tests advanced math problems written by mathematicians, and Grok 4.7 scores 53.0%, compared with 66.0% for Grok 4.6. Grok 4.7 also scores a little lower on GPQA Diamond, a set of graduate-level science questions (92.7% vs. 94.0%).

Grok 4.6 also holds a small lead on Arena Text, where people compare answers to everyday prompts. The gap is 14 points, which is close enough that most users won't notice it.

xAI hasn't explained the math drop. Whatever the cause, it's a good reminder that a newer model in the same family isn't automatically better at everything. It's a bit like a new phone that takes better photos but has a slightly worse speaker: still an upgrade for most people, but not for everyone.

Is Grok 4.7 faster than Grok 4.6?

Slightly. On our benchmark, Grok 4.7 generates about 72 tokens per second, compared with 65 for Grok 4.6. Speed is a seven-day median, so it can shift as providers adjust capacity.

Here's what that looks like for a few response lengths. These are estimates, based on time to first token plus output length divided by speed:

Grok 4.7 vs Grok 4.6: estimated response times for 500, 1,000, and 2,000 output tokens

Output length

Grok 4.7 (estimate)

Grok 4.6 (estimate)

500 tokens

About 8.7 seconds

About 9.6 seconds

1,000 tokens

About 15.7 seconds

About 17.3 seconds

2,000 tokens

About 29.6 seconds

About 32.7 seconds

Real response times depend on the reasoning effort level and how many reasoning tokens the model uses. Since xAI says Grok 4.7 spends longer on hard problems, some tasks may take more time than these estimates suggest.

What is the Grok 4.7 fast variant?

If output speed matters most, xAI also offers Grok 4.7 Fast. xAI's docs describe it as "the same model served on faster infrastructure," and xAI says it delivers twice the output speed at twice the price. That works out to $4 per million input tokens and $12 per million output tokens, compared with the standard rate of $2 / $6. For long-context requests, the premium drops to 1.5x the standard rate.

The fast variant is currently available only in Cursor and Grok Build, not on the public xAI API, according to xAI's docs.

Does Grok 4.7 cost more than Grok 4.6?

No. Grok 4.7 has the same price as Grok 4.6: $2 per million input tokens and $6 per million output tokens, which works out to a $3.00 blended price on our benchmark (weighted three parts input to one part output).

Your actual bill can still change. If Grok 4.7 reasons longer on hard tasks, it may use more output tokens per request, even at the same effort level. Two other details from xAI's docs can also affect cost: the US regional endpoint adds a 10% premium, and requests without a prompt cache key may pay the full input price instead of the cached rate.

Should you switch from Grok 4.6 to Grok 4.7?

For most teams, yes. If you use Grok for agentic coding or knowledge work like legal research, Grok 4.7 is the more powerful model at the same price.

The exception is math-heavy work. If your application depends on advanced math or scientific reasoning, keep Grok 4.6 on those tasks until you've tested Grok 4.7 on your own examples. Benchmarks are a useful guide, but your own tasks are the real test.

A sensible approach is to run both models side by side on a sample of your real prompts for a week or two. Keep the same effort level for both, measure the quality of the answers and the cost per task, and compare the results in a simple table before you move traffic over.

Switching models is usually a small code change and a bigger testing job. Our forward-deployed engineers help companies build and evaluate AI features, including side-by-side model tests on real workloads. If you need hands-on help inside your own team, our team can run the evaluation and roll out the change with you.

What's the difference between Grok 4.7 and Grok 4.6?

Grok 4.7 is xAI's newer model, released September 21, 2026. It has the same price and 500,000-token context window as Grok 4.6, but it scores much higher on agentic coding benchmarks like Terminal-Bench 4.0. It scores lower on FrontierMath, an advanced math benchmark.

Is Grok 4.7 better than Grok 4.6?

For most tasks, yes. Grok 4.7 ranks 11th on FullStack's RFE benchmark, compared with 22nd for Grok 4.6. Grok 4.6 still scores higher on FrontierMath and GPQA Diamond, so it may be the better choice for math-heavy work.

How much does Grok 4.7 cost?

Grok 4.7 costs $2 per million input tokens and $6 per million output tokens, the same as Grok 4.6. A faster version costs twice as much and is available only in Cursor and Grok Build.

What is Grok 4.7's context window?

Grok 4.7 has a 500,000-token context window, the same as Grok 4.6. Its knowledge cutoff is May 2026.

Is Grok 4.7 faster than Grok 4.6?

Slightly. Grok 4.7 generates about 72 tokens per second on our benchmark, compared with 65 for Grok 4.6. Actual response times also depend on the reasoning effort level you choose.

How can you cut your AI coding costs without switching models?

This guide gives you the math, the meaningful benchmarks, and a six-step plan to cut spend without giving up capability.

Enjoyed the article? Get new content delivered to your inbox.

Subscribe below and stay updated with the latest developer guides and industry insights.

Thank you! Your submission has been received!

Oops! Something went wrong while submitting the form.

Iframe is blocked. Accept cookies to load it.

Thank you! Your submission has been received!

Oops! Something went wrong while submitting the form.
