---
id: collect-261121-veille-ia/muse/meta-muse-ember-sets-a-new-agent-price-floor
title: "Meta Muse Ember Sets a New Agent Price Floor"
domain: muse
role: reference
task: article
actors: ["Meta"]
dates: ["2026-11-09"]
keywords: ["muse", "meta", "ember", "model", "price", "tokens", "agent", "floor"]
source: https://aragonresearch.com/meta-muse-ember-sets-a-new-agent-price-floor/
source_anchor: ""
source_lines: [1, 35]
section: "Muse"
---
# Meta Muse Ember Sets a New Agent Price Floor

By Adam Pease

## Meta Muse Ember Sets a New Agent Price Floor

On November 9, 2026, Meta released Meta Muse Ember, its smallest and most efficient model. Meta built it for the repetitive work that AI Agents do all day. The model costs $0.10 per million input tokens, and Meta says it delivers more intelligence per dollar than Claude Haiku 5.5 or GPT-6 Luna at every reasoning setting. This blog overviews the Meta Muse Ember announcement and offers our analysis.

## Why Did Meta Announce Muse Ember?

Meta Superintelligence Labs designed Muse Ember for “the work agents repeat all day: tool calls, coding subtasks, routing, extraction, and long document parsing.” The model is available as `muse-ember-1.0` on the Meta Model API and Muse Code. It costs $0.10 per million input tokens, $0.004 per million cached input tokens and $0.50 per million output tokens. It has a 1M-token context window, produces up to 512K output tokens, and handles text, images, audio and video natively.

The motive is to round out a portfolio. Meta positions Muse Wonder for the hardest problems, Muse Spark for coding and agentic workflows, and Muse Ember for repeatable tasks at scale. In the company’s words, “Muse Wonder plans, Muse Ember handles the volume.” Muse Ember is also the first model Meta trained using Muse Wonder Full. According to TechCrunch, it is the fourth model under the Muse brand since April.

Meta leads with cost per task. On the Artificial Analysis Intelligence Index, Muse Ember scores 36 at about $0.14 per task. Muse Spark 1.3 scores 37 at $2.39. Meta says Ember matches Haiku’s best score at about a third of the cost. TechCrunch adds that it is cost-competitive with open models such as DeepSeek V4.1 Flash and GLM 5.3 Flash.

## Analysis

At Aragon Research, we view Muse Ember as a new price floor for the work that makes up most agent traffic. Agents spend most of their tokens on small, structured steps, and those steps don’t need a frontier model. Meta’s own lineup makes the case: Ember gives up one index point to Muse Spark 1.3 at about one seventeenth of the cost per task. When a vendor prices against its own mid-tier model this hard, high-volume agent execution has become a commodity.

We have expected frontier models to become an escalation tier rather than the default, and this release supports that view. The sensible architecture is a router. The cheap model handles the volume, and the frontier model takes the planning, the hard cases and the failures. Enterprises that send every tool call and extraction step to a frontier model are paying for capability that most of those steps never use.

Some caution is in order, though. Several of the benchmarks Meta leads with are its own internal evaluations, including Agents’ Last Exam, OSWorld 2.1 and its Personal Superintelligence Index. Haiku 5.5 beats Ember on FrontierCode 2.0-AA, which judges real engineering tasks on whether the code can be merged: 34.9 percent to 27.2 percent. Long prompts also cost more. Above 200K tokens, the price triples to $0.30 per million input tokens and $1.50 per million output tokens. Customer-reported results look encouraging, but they come from early partner testing. Cloudflare reports up to 30% faster tail latency, and You.com reports a p95 of 2.2 seconds.

Price alone isn’t the decision. The announcement says nothing about rate limits or data retention terms, and the Muse line is less than a year old. Meta’s availability on Google Cloud and Oracle Cloud helps with procurement. Even so, enterprises should weigh how durable Meta is as an enterprise API provider before they put production volume on it. For Anthropic and OpenAI, the pressure is clear: they now have to compete on price for small models or prove reliability that Meta can’t match.

## What Should Enterprises Do About Meta Muse Ember?

Enterprises should break their agent workflows into steps and price each step on its own. Build an evaluation set from your own tasks, such as contract clauses, support tickets, extraction jobs and coding subtasks, rather than relying on vendor charts. Then route each step to the cheapest model that passes those tests. Measure cost per completed task, not cost per token, because retries and output length can erase a low list price.

Treat frontier models as an escalation tier with explicit triggers, such as low confidence, failed validation or high-impact actions. Keep the routing layer independent of any single model provider so you can swap models as prices fall again. Before committing volume to Muse Ember, procurement and security teams should review its data handling terms, rate limits, regional availability and long-context pricing.

## Bottom Line

Meta Muse Ember sets a new price floor for agent execution and turns the repetitive steps in agent workflows into a commodity. Small models from Anthropic and OpenAI now face direct price pressure, and frontier models become the escalation tier. Enterprises should pay frontier prices only where their own tests show they are needed, and check limits, data terms and vendor durability before they move production volume.
