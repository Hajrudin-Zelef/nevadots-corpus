---
id: collect-261121-veille-ia/grok-xai/grok-4-7-features-benchmarks-and-pricing
title: "Grok 4.7: Features, Benchmarks, and Pricing"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: ["2026-09-21"]
keywords: ["grok", "model", "work", "coding", "benchmarks", "fable", "spacexai", "features"]
source: https://www.datacamp.com/blog/grok-4-7
source_anchor: ""
source_lines: [1, 381]
section: "Grok_xAI"
---
Grok 4.7: Features, Benchmarks, Pricing, and Access

xAI's Grok, a new coding and knowledge-work model, runs at $2/$6 per million tokens and tops frontier rivals on some benchmarks. Here's what's new, how it compares to the competition, and how it upgrades on its predecessor.

The frontier model race has settled into a pattern where the interesting differences are less about raw capability and more about what you pay for it.

Claude Fable 5.1 sits at $10 input and $50 output per million tokens, and the practical question for most teams is whether that premium buys anything a cheaper model can't match.

SpaceXAI released Grok 4.7 on September 21, 2026, positioning it as its most capable model for coding and knowledge work.

It runs at $2 per million input tokens and $6 per million output tokens, unchanged from Grok 4.6, and posts 46.3% on CursorBench 4.0 for longer-running coding tasks, slightly behind Fable 5.1.

In this article, I'll cover everything new with Grok 4.7, looking at the new features, exploring the benchmarks against GPT-5.6 Sol and Fable 5.1, and looking at access options.

Grok 4.7 is SpaceXAI's new default for coding and knowledge work, built on a larger base model than Grok 4.6.

The headline is price-performance: frontier-adjacent scores at $2/$6 per million tokens, an eighth of what Fable charges on output.

It dominates the Harvey Legal Agent Benchmark at 19.6%, crushing GPT-5.6 Sol (2.5%) and Fable 5.1 (6.7%).

Fable 5.1 still leads on absolute coding and clinical reasoning, so this is a value pick, not an outright capability leader.

Worth switching to if you run high-volume coding or agentic workloads where output token cost dominates your bill.

What Is Grok 4.7?

Grok 4.7 is SpaceXAI's flagship model for coding and knowledge work, released September 21, 2026, as the successor to Grok 4.6.

It uses a new, larger base model and was trained with a longer reinforcement learning run on a harder mix of tasks, weighted toward problems that take many hours to complete. SpaceXAI trained it to work longer on difficult tasks and to check its own work more carefully.

The generational jump over Grok 4.6 shows up most on long-running work. On Terminal-Bench 4.0, which measures multi-hour terminal tasks, Grok 4.7 nearly doubles its predecessor on SpaceXAI's internal harness, going from 20.3% to 38.0%. CursorBench 4.0 moves from 40.4% to 46.3%, and the AA Briefcase v1.1 office-work score climbs from 1,546 to 1,657.

The headline that matters for practitioners is the Harvey Legal Agent Benchmark, where Grok 4.7 posts 19.6%. That's more than 7x GPT-5.6 Sol's 2.5% and nearly 3x Fable 5.1's 6.7%, a gap wide enough that legal-adjacent agentic work is a genuine reason to reach for this model over the more expensive frontier options.

Grok 4.7 Key Features

Grok 4.7's improvements cluster around three things: sustained work on long tasks, professional document creation, and a new safeguard stack.

Here's what you can actually do with them.

Work longer on multi-hour tasks

Grok 4.7 was trained specifically on problems that take hours rather than minutes, so it holds up better on long-horizon coding and terminal work.

In practice, that means it can keep a coherent thread through a multi-step debugging session or a lengthy migration without losing the plot partway through.

The model manages longer context and verifies its own output more carefully than Grok 4.6 did.

If you've had a model confidently declare a task finished while leaving half the tests failing, this is the area SpaceXAI is targeting.

Create documents and presentations

Grok 4.7 is better at producing the kind of deliverables professionals actually hand over: reports, briefs, and slide decks.

SpaceXAI benchmarks this on GDPval and AA Briefcase, which task the model with work normally done by lawyers, nurses, and financial analysts.

For a data team, this matters when you want a model to turn an analysis into a written summary or a stakeholder-ready presentation rather than just returning raw output.

Grok 4.7 improves over Grok 4.6 on both benchmarks and performs comparably to other frontier models, so document generation is no longer a reason to pay a premium for Fable.

Native Grok Bot conversational handling

SpaceXAI trained Grok 4.7 to natively understand the Grok Bot harness, which makes it stronger on conversational tasks and general knowledge work.

If you're building a chat-style assistant on top of the Grok API, the model behaves more predictably inside that conversational loop.

This is a narrower feature than the coding gains, but it signals SpaceXAI treating conversational deployment as a first-class use case rather than an afterthought bolted onto a coding model.

New safeguard stack with high utility

Grok 4.7 ships with an entirely new safeguard stack, and SpaceXAI calls it the strongest model it has tested on refusals and jailbreak resistance.

The interesting part for practitioners is that it does this without over-refusing legitimate work.

On HackerBench v0.3, SpaceXAI's benchmark for risky and malicious cyber tasks, Grok 4.7 allows only 3.3% of risky dual-use prompts through while rarely blocking legitimate security work.

It also tops LatchBio's biosafety benchmark at 62.4%.

Grok 4.7's approach is to distinguish benign from dangerous rather than refuse the whole domain.

How Does Grok 4.7 Perform on the Benchmarks?

When looking at independent aggregates, Grok 4.7 lands solidly in the middle of the pack.

On the Artificial Analysis Intelligence Index v4.3.2, which combines ten benchmarks, Grok 4.7 scores a 46.

This places it well behind the current leaders, Claude Fable 5.1 and GPT-6, which both score a 53. Ultimately, Grok 4.7 is a price-performance leader rather than an outright capability leader.

It beats GPT-5.6 Sol on most coding and knowledge benchmarks while charging less than a third of Sol's output price, but Fable 5.1 still edges it on absolute coding scores.

The one benchmark where Grok 4.7 leads everyone by a wide margin is legal agent work.

Coding and agentic workflows

On CursorBench 4.0, which stresses longer-running coding tasks, Grok 4.7 scores 46.3%. That beats GPT-5.6 Sol's 41.7% and its own predecessor's 40.4%, though Fable 5.1 leads at 51.8%. On DeepSWE v1.1, Grok 4.7 posts 71.0% at high effort, close to Sol's 72.7% and Fable 5.1's 70.0%.

For terminal agent work, there is a notable discrepancy. While SpaceXAI's internal harness shows Grok 4.7 jumping to 38.0% on Terminal-Bench 4.0, Artificial Analysis measures Grok 4.7 at just 26% on the same benchmark. This places it well behind GPT-6 Astra (60%) and Claude Fable 5.1 (55%), and even slightly behind the cheaper DeepSeek V4.1 Flash (27%). If terminal agent work is your core workload, the frontier models still decisively win on raw capability.

The takeaway: on coding, Grok 4.7 trades blows with GPT-5.6 Sol at a fraction of the cost, but Fable 5.1 remains the pick if you need the absolute top of the leaderboard.

Professional knowledge work

Grok 4.7 leads all listed competitors on legal work.

Its 19.6% on the Harvey Legal Agent Benchmark towers over GPT-5.6 Sol's 2.5% and Fable 5.1's 6.7%, and improves on Grok 4.6's 15.8%. If your workflow touches legal document analysis or contract review, this gap is hard to ignore.

On multi-hour office work measured by AA Briefcase v1.1, Grok 4.7 scores 1,657, placing it ahead of GPT-5.6 Sol (1,487) and sitting just behind Fable 5.1 (1,678).

For clinical reasoning on HealthBench Professional, Grok 4.7 hits 56.7%, trailing Sol's 60.5% and Fable 5.1's 62.1% but beating its own predecessor's 48.5%.

Technical and engineering tasks

On electrical engineering tasks measured by EEBench, Grok 4.7 posts a strong 64.0%, leaping past its predecessor (53.0%), Fable 5.1 (56.4%), and GPT-5.6 Sol (39.4%).

Taken together, the pattern is consistent; Grok 4.7 performs very well on specialized professional benchmarks (legal, electrical engineering) and matches or beats Sol on general coding, while Fable 5.1 holds the top of the pure-coding leaderboard. For a model at $2/$6, that's a strong position.

Which Tier Should You Use?

Grok 4.7 ships in effort variants and a fast variant, so the choice comes down to how much you're willing to spend on latency and reasoning depth.

The default recommendation is the standard variant at $2/$6. The fast variant serves output at twice the speed for twice the price, so it only makes sense when response latency directly affects your product.

SpaceXAI's published benchmarks use the xhigh effort setting for Grok 4.7, with a note that its DeepSWE v1.1 score of 71.0% is a high-effort result.

Higher effort means the model works longer and spends more output tokens on verification, which is where the multi-hour task training pays off but also where your bill grows.

Use case

Variant

Why

High-volume coding pipelines

Standard xhigh

Best price-performance; output cost dominates so avoid the fast premium

Legal or electrical engineering agents

Standard xhigh

This is where Grok 4.7's benchmark leads live; effort helps verification

Latency-sensitive chat products

Fast variant

2x output speed matters when users are waiting on responses

Batch document and slide generation

Standard

No latency constraint, so the cheaper tier wins

Grok 4.7 Pricing and Availability

Grok 4.7 is priced starting at $2 per million input tokens and $6 per million output tokens, unchanged from Grok 4.6.

SpaceXAI also serves a fast variant with twice the output speed at twice the price, so $4 input and $12 output.

That base rate is an eighth of what Claude Fable 5.1 charges on output ($10 input and $50 output per million tokens). Meta's Muse Spark 1.3 stable tier undercuts Grok on input at $1.25 but is close on output at $4.25, while Meta's Contributor tier at $0.10/$0.20 is far cheaper in exchange for letting Meta train on your prompts.

Grok 4.7 is generally available today.

There is no free API tier, but you can try it at no cost through Grok Build.

Artificial Analysis lists Grok 4.7 as having a 500,000-token context window, though it is notably omitted from SpaceXAI's launch post. This is a recurring gap in Grok documentation compared to the ~1M token windows competitors like Muse Spark publish.

How to Access Grok 4.7

Grok 4.7 is a proprietary model reachable through several surfaces:

The Grok API via the SpaceXAI console

Cursor, for in-editor coding

Grok Build, free to get started

Third-party coding harnesses, model routers, and cloud platforms

To call it from the API, use the model ID as the exact string in your request. Here's a minimal example using the OpenAI-compatible client pointed at the SpaceXAI endpoint:

from openai import OpenAI
client = OpenAI(
api_key="YOUR_XAI_KEY",
base_url="https://api.x.ai/v1",)
response = client.chat.completions.create(
model="grok-4.7",
messages=[{"role":"user","content":"Refactor this SQL query for readability..."}],)print(response.choices[0].message.content)Powered by

Final Thoughts

Grok 4.7 is SpaceXAI making a clear value play in a market where Fable is betting that frontier capability justifies $50-per-million output pricing. This is similar to what we've seen from the recent spate of Flash models (see our comparison of DeepSeek V4.1 Flash vs Gemini 3.8 Flash).

By holding Grok 4.6's $2/$6 rates while posting frontier-adjacent scores, SpaceXAI is competing on cost-per-result rather than leaderboard position.

I'd switch to it for high-volume coding, legal agent work, or electrical engineering tasks, where its benchmark leads and low price line up.

If you need the absolute top of the coding leaderboard, Fable 5.1 still wins, and Grok's thin context-window documentation remains an annoyance.

If you want to get hands-on with building agentic workflows on models like this, I recommend our AI Agent Fundamentals skill track to build the foundation first.

FAQs

How does Grok 4.7 compare to Grok 4.6?⌄

Grok 4.7 uses a new, larger base model than Grok 4.6 and was trained with a longer reinforcement learning run on harder, multi-hour tasks. It improves across the board: CursorBench 4.0 rises from 40.4% to 46.3%, Terminal-Bench 4.0 nearly doubles from 20.3% to 38.0%, and EEBench climbs from 53.0% to 64.0%. Pricing stays the same at $2/$6 per million tokens.

How much does Grok 4.7 cost?⌄

Grok 4.7 is priced at $2 per million input tokens and $6 per million output tokens, unchanged from Grok 4.6. A fast variant serves output at twice the speed for twice the price ($4 input, $12 output). That base rate is about a quarter of GPT-6 Astra and Claude Fable 5.1, which both charge $10/$50.

Where can I access Grok 4.7?⌄

Grok 4.7 is generally available today through the Grok API via the xAI console, in Cursor, and in Grok Build (free to start). It's also reachable through third-party coding harnesses, model routers, and cloud platforms. Use the model ID grok-4.7 in your API requests.

Is Grok 4.7 better than GPT-5.6 Sol and Claude Fable 5.1?⌄

It depends on the task. Grok 4.7 leads both on legal work (19.6% on Harvey vs Sol's 2.5% and Fable's 6.7%) and electrical engineering (64.0% on EEBench). On general coding it roughly matches GPT-5.6 Sol, but Fable 5.1 still leads on CursorBench (51.8%) and Terminal-Bench (57.9%). Grok wins decisively on price-performance.

What is Grok 4.7 best used for?⌄

Grok 4.7 is strongest for high-volume coding pipelines, legal agent work, electrical engineering tasks, and document or presentation generation. Its low $2/$6 pricing makes it especially suited to output-heavy agentic workloads where token cost dominates the bill. It also has xAI's strongest safeguard stack to date, refusing risky dual-use prompts while rarely blocking legitimate security work.

SpaceXAI's new model, Grok 4.6, matches GPT-5.6 Sol's Intelligence Index score at a lower measured price. See benchmarks, agent features, pricing, and how it compares with Grok 4.5 and Claude Sonnet 5.

Grok 4.6 and GPT-5.6 Sol score the same 61 on the Artificial Analysis Intelligence Index, so the real decision is about cost shape, context size, and turns per task.

Grok 4.6 undercuts Claude Opus 5 at $2/$6 versus $5/$25, but Opus 5 still leads knowledge-work Elo and our aurora shader test. Here's who should use which.

SpaceXAI's new speech-to-text model ranks first for streaming accuracy on the public leaderboard at an unchanged hourly price. We cover what changed from 1.0, the benchmarks, pricing, and how to call it.

The frontier model race has settled into a pattern where the interesting differences are less about raw capability and more about what you pay for it.

Claude Fable 5.1 sits at $10 input and $50 output per million tokens, and the practical question for most teams is whether that premium buys anything a cheaper model can't match.

SpaceXAI released Grok 4.7 on September 21, 2026, positioning it as its most capable model for coding and knowledge work.

It runs at $2 per million input tokens and $6 per million output tokens, unchanged from Grok 4.6, and posts 46.3% on CursorBench 4.0 for longer-running coding tasks, slightly behind Fable 5.1.

In this article, I'll cover everything new with Grok 4.7, looking at the new features, exploring the benchmarks against GPT-5.6 Sol and Fable 5.1, and looking at access options.

For more on the competitive field, see our comparison of Claude Fable 5.1 and GPT-6 Astra and our look at Claude Opus 5 vs GPT-5.6 Sol. I also recommend reading up on TypeSafe's Jev.

## TL;DR

## What Is Grok 4.7?

Grok 4.7 is SpaceXAI's flagship model for coding and knowledge work, released September 21, 2026, as the successor to Grok 4.6.

It uses a new, larger base model and was trained with a longer reinforcement learning run on a harder mix of tasks, weighted toward problems that take many hours to complete. SpaceXAI trained it to work longer on difficult tasks and to check its own work more carefully.

The generational jump over Grok 4.6 shows up most on long-running work. On Terminal-Bench 4.0, which measures multi-hour terminal tasks, Grok 4.7 nearly doubles its predecessor on SpaceXAI's internal harness, going from 20.3% to 38.0%. CursorBench 4.0 moves from 40.4% to 46.3%, and the AA Briefcase v1.1 office-work score climbs from 1,546 to 1,657.

The headline that matters for practitioners is the Harvey Legal Agent Benchmark, where Grok 4.7 posts 19.6%. That's more than 7x GPT-5.6 Sol's 2.5% and nearly 3x Fable 5.1's 6.7%, a gap wide enough that legal-adjacent agentic work is a genuine reason to reach for this model over the more expensive frontier options.

## Grok 4.7 Key Features

Grok 4.7's improvements cluster around three things: sustained work on long tasks, professional document creation, and a new safeguard stack.

Here's what you can actually do with them.

## Work longer on multi-hour tasks

Grok 4.7 was trained specifically on problems that take hours rather than minutes, so it holds up better on long-horizon coding and terminal work.

In practice, that means it can keep a coherent thread through a multi-step debugging session or a lengthy migration without losing the plot partway through.

The model manages longer context and verifies its own output more carefully than Grok 4.6 did.

If you've had a model confidently declare a task finished while leaving half the tests failing, this is the area SpaceXAI is targeting.

## Create documents and presentations

Grok 4.7 is better at producing the kind of deliverables professionals actually hand over: reports, briefs, and slide decks.

SpaceXAI benchmarks this on GDPval and AA Briefcase, which task the model with work normally done by lawyers, nurses, and financial analysts.

For a data team, this matters when you want a model to turn an analysis into a written summary or a stakeholder-ready presentation rather than just returning raw output.

Grok 4.7 improves over Grok 4.6 on both benchmarks and performs comparably to other frontier models, so document generation is no longer a reason to pay a premium for Fable.

## Native Grok Bot conversational handling

SpaceXAI trained Grok 4.7 to natively understand the Grok Bot harness, which makes it stronger on conversational tasks and general knowledge work.

If you're building a chat-style assistant on top of the Grok API, the model behaves more predictably inside that conversational loop.

This is a narrower feature than the coding gains, but it signals SpaceXAI treating conversational deployment as a first-class use case rather than an afterthought bolted onto a coding model.

## New safeguard stack with high utility

Grok 4.7 ships with an entirely new safeguard stack, and SpaceXAI calls it the strongest model it has tested on refusals and jailbreak resistance.

The interesting part for practitioners is that it does this without over-refusing legitimate work.

On HackerBench v0.3, SpaceXAI's benchmark for risky and malicious cyber tasks, Grok 4.7 allows only 3.3% of risky dual-use prompts through while rarely blocking legitimate security work.

It also tops LatchBio's biosafety benchmark at 62.4%.

Grok 4.7's approach is to distinguish benign from dangerous rather than refuse the whole domain.

## How Does Grok 4.7 Perform on the Benchmarks?

When looking at independent aggregates, Grok 4.7 lands solidly in the middle of the pack.

On the Artificial Analysis Intelligence Index v4.3.2, which combines ten benchmarks, Grok 4.7 scores a 46.

This places it well behind the current leaders, Claude Fable 5.1 and GPT-6, which both score a 53. Ultimately, Grok 4.7 is a price-performance leader rather than an outright capability leader.

It beats GPT-5.6 Sol on most coding and knowledge benchmarks while charging less than a third of Sol's output price, but Fable 5.1 still edges it on absolute coding scores.

The one benchmark where Grok 4.7 leads everyone by a wide margin is legal agent work.

## Coding and agentic workflows

On CursorBench 4.0, which stresses longer-running coding tasks, Grok 4.7 scores 46.3%. That beats GPT-5.6 Sol's 41.7% and its own predecessor's 40.4%, though Fable 5.1 leads at 51.8%. On DeepSWE v1.1, Grok 4.7 posts 71.0% at high effort, close to Sol's 72.7% and Fable 5.1's 70.0%.

For terminal agent work, there is a notable discrepancy. While SpaceXAI's internal harness shows Grok 4.7 jumping to 38.0% on Terminal-Bench 4.0, Artificial Analysis measures Grok 4.7 at just 26% on the same benchmark. This places it well behind GPT-6 Astra (60%) and Claude Fable 5.1 (55%), and even slightly behind the cheaper DeepSeek V4.1 Flash (27%). If terminal agent work is your core workload, the frontier models still decisively win on raw capability.

The takeaway: on coding, Grok 4.7 trades blows with GPT-5.6 Sol at a fraction of the cost, but Fable 5.1 remains the pick if you need the absolute top of the leaderboard.

## Professional knowledge work

Grok 4.7 leads all listed competitors on legal work.

Its 19.6% on the Harvey Legal Agent Benchmark towers over GPT-5.6 Sol's 2.5% and Fable 5.1's 6.7%, and improves on Grok 4.6's 15.8%. If your workflow touches legal document analysis or contract review, this gap is hard to ignore.

On multi-hour office work measured by AA Briefcase v1.1, Grok 4.7 scores 1,657, placing it ahead of GPT-5.6 Sol (1,487) and sitting just behind Fable 5.1 (1,678).

For clinical reasoning on HealthBench Professional, Grok 4.7 hits 56.7%, trailing Sol's 60.5% and Fable 5.1's 62.1% but beating its own predecessor's 48.5%.

## Technical and engineering tasks

On electrical engineering tasks measured by EEBench, Grok 4.7 posts a strong 64.0%, leaping past its predecessor (53.0%), Fable 5.1 (56.4%), and GPT-5.6 Sol (39.4%).

Taken together, the pattern is consistent; Grok 4.7 performs very well on specialized professional benchmarks (legal, electrical engineering) and matches or beats Sol on general coding, while Fable 5.1 holds the top of the pure-coding leaderboard. For a model at $2/$6, that's a strong position.

## Which Tier Should You Use?

Grok 4.7 ships in effort variants and a fast variant, so the choice comes down to how much you're willing to spend on latency and reasoning depth.

The default recommendation is the standard variant at $2/$6. The fast variant serves output at twice the speed for twice the price, so it only makes sense when response latency directly affects your product.

SpaceXAI's published benchmarks use the xhigh effort setting for Grok 4.7, with a note that its DeepSWE v1.1 score of 71.0% is a high-effort result.

Higher effort means the model works longer and spends more output tokens on verification, which is where the multi-hour task training pays off but also where your bill grows.

## Grok 4.7 Pricing and Availability

Grok 4.7 is priced starting at $2 per million input tokens and $6 per million output tokens, unchanged from Grok 4.6.

SpaceXAI also serves a fast variant with twice the output speed at twice the price, so $4 input and $12 output.

That base rate is an eighth of what Claude Fable 5.1 charges on output ($10 input and $50 output per million tokens). Meta's Muse Spark 1.3 stable tier undercuts Grok on input at $1.25 but is close on output at $4.25, while Meta's Contributor tier at $0.10/$0.20 is far cheaper in exchange for letting Meta train on your prompts.

Grok 4.7 is generally available today.

There is no free API tier, but you can try it at no cost through Grok Build.

Artificial Analysis lists Grok 4.7 as having a 500,000-token context window, though it is notably omitted from SpaceXAI's launch post. This is a recurring gap in Grok documentation compared to the ~1M token windows competitors like Muse Spark publish.

## How to Access Grok 4.7

Grok 4.7 is a proprietary model reachable through several surfaces:

To call it from the API, use the model ID as the exact string in your request. Here's a minimal example using the OpenAI-compatible client pointed at the SpaceXAI endpoint:

## Final Thoughts

Grok 4.7 is SpaceXAI making a clear value play in a market where Fable is betting that frontier capability justifies $50-per-million output pricing. This is similar to what we've seen from the recent spate of Flash models (see our comparison of DeepSeek V4.1 Flash vs Gemini 3.8 Flash).

By holding Grok 4.6's $2/$6 rates while posting frontier-adjacent scores, SpaceXAI is competing on cost-per-result rather than leaderboard position.

I'd switch to it for high-volume coding, legal agent work, or electrical engineering tasks, where its benchmark leads and low price line up.

If you need the absolute top of the coding leaderboard, Fable 5.1 still wins, and Grok's thin context-window documentation remains an annoyance.

If you want to get hands-on with building agentic workflows on models like this, I recommend our AI Agent Fundamentals skill track to build the foundation first.
