---
id: collect-261121-veille-ia/claude-anthropic/why-does-claude-or-gpt-feel-worse-than-its-benchmarks
title: "Why Does Claude or GPT Feel Worse Than Its Benchmarks?"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["model", "effort", "same", "claude", "does", "benchmark", "worse", "feel"]
source: https://www.mindstudio.ai/blog/claude-opus-benchmark-effort-settings
source_anchor: ""
source_lines: [1, 79]
section: "Claude_Anthropic"
---
## Why does Claude or GPT feel worse than its benchmark scores suggest?

Because the benchmark number and your daily chat often aren’t running the same configuration. Model providers test flagship scores using maximum reasoning effort, the highest setting for how long and hard a model is allowed to “think” before answering. Your default experience, especially in consumer apps, frequently runs at a lower effort setting to save compute and return answers faster. Same weights, same model, different dial. Anthropic has confirmed this directly: they once quietly lowered the default effort in Claude Code from high to medium to speed things up, then later admitted it was the wrong tradeoff.

## TL;DR

- **Benchmark scores use max effort** , while everyday usage in apps like Claude Code or ChatGPT often defaults to medium effort, which produces measurably weaker output on the same prompts.
- **Anthropic has published postmortems** admitting Claude got worse for users even when their internal evaluations showed nothing wrong, meaning the company’s own tests failed to catch what people were noticing.
- **The underlying model weights don’t appear to change.** Independent trackers that test the same prompts daily haven’t caught evidence of the base model itself being swapped for a smaller or rounded-down version.
- **Four adjustable “dials” sit around the model** : reasoning effort, system prompt instructions, routing (which model actually answers), and the surrounding app infrastructure, and any of these can quietly get turned down.
- **Routing failures can silently swap models entirely.** One widely cited case involved a safety filter rerouting requests away from a flagship model to a different one without clearly telling users it happened.
- **No law currently requires disclosure** when a company adjusts these settings after launch, so the burden of noticing and adjusting falls on users.
- **You can raise effort yourself** in most interfaces, trading cost and speed for quality when a task actually needs it.

### *4 hours* from idea to a live business app.

**Free**live class · Wed, Dec 2

## What is the “effort” setting and why does it matter?

Reasoning effort controls how much internal computation a model does before producing a response. Think of it as how long the model is allowed to deliberate, check its own work, or explore alternatives before committing to an answer. Higher effort generally means better accuracy on hard problems, longer response times, and higher compute cost per query.

When companies publish flagship benchmark numbers, those scores typically come from running the model at maximum effort. That’s a legitimate way to show what the model is capable of. The problem is that the default effort setting most people get when they open the app is often lower. Anthropic’s own launch materials reportedly showed a meaningful gap between a model’s max-effort coding benchmark and the score it produced at the effort level most users actually experience, a difference of several percentage points on the same evaluation.

This isn’t a conspiracy exclusive to one company. It’s an economic reality: every unit of reasoning effort costs real compute, and compute costs real money. Right after a splashy launch, API traffic can spike dramatically (OpenAI’s Sam Altman has said GPT-5 traffic roughly doubled within 24 hours of release), and dialing back default effort is one of the fastest ways to keep a service responsive under load.

## Is the model itself actually getting nerfed, or is it something else?

As far as publicly available evidence shows, the core model weights don’t appear to be the thing changing. Independent benchmark trackers that run identical prompts against the same model every day haven’t caught clear evidence of the underlying weights being swapped for a smaller or quantized version. One such tracker ran the same set of questions daily for 50 days without detecting a drop in the base model’s performance.

That said, real drops in output quality have been documented, just not at the weights level. One daily benchmark caught a measurable decline in a coding harness, but traced it back to an update in the surrounding app, not the model underneath. This matters because it means the “nerf” people feel is usually happening in the scaffolding around the model, not in the model’s brain itself.

The dials that do appear to move include:

- **Effort** : how much the model is allowed to reason before answering.
- **System prompt instructions** : the hidden rules every app wraps around a model. Anthropic reportedly added an instruction limiting Claude’s intermediate text to a short word count, and their own testing later showed this made outputs worse.
- **Routing** : which model actually answers a given request. In one documented case, a safety filter silently rerouted a meaningful share of requests away from a flagship model to a different one, and a debugging benchmark score for the intended model collapsed as a result, mostly because the harder tasks never actually reached it.
- **App infrastructure** : bugs and misconfigurations in the product layer. Anthropic has acknowledged shipping a bug that wiped a model’s working memory between turns, and OpenAI has found misconfigured servers degrading performance for a portion of traffic.

Build an onboarding app for 4,000 new hires.

Build ↵
None of these require touching the model’s trained weights to produce a noticeably worse experience.

## How can you check what effort level you’re actually getting?

Most platforms that expose a reasoning effort control let you see or set it directly, often as a setting labeled something like low, medium, or high (or a numeric equivalent). If you’re using an API or a developer-facing tool, check the request parameters or documentation for an effort or reasoning parameter. If you’re using a consumer chat app, look in the settings menu or model picker for any mention of “thinking,” “extended reasoning,” or effort level.

A practical test: run the same non-trivial prompt (a coding task, a logic puzzle, a multi-step writing request) at the default setting and then again at the highest available effort setting. If the outputs differ meaningfully, you’ve confirmed the default isn’t using the model’s full capability. This is the same kind of comparison that revealed the gap between Anthropic’s headline benchmark score and the score most users actually experience day to day.

## Is it worth manually increasing effort to max?

It depends on the task. Max effort costs more time and more money per query, and for simple requests (quick lookups, short rewrites, casual conversation) it’s wasted overhead. For anything that resembles the hard end of a benchmark, complex debugging, multi-step reasoning, or tasks where a wrong answer is expensive, pushing the effort setting up is usually worth the tradeoff.

The practical discipline is matching effort to the task rather than leaving a model permanently on its lowest or highest setting. Running everything at max effort burns budget on easy requests. Running everything at the default can quietly shortchange your hardest work, especially since most people push a new model hardest during launch week and then throw real, complex work at it weeks later, exactly when effort and other settings may have already been quietly dialed down.

## Frequently Asked Questions

### Why do AI models feel worse a few weeks after launch?

Companies sometimes adjust the settings surrounding a model after launch, including default reasoning effort, system prompt instructions, and request routing, usually to manage cost and compute load as traffic grows. Anthropic has confirmed this happened with Claude Code’s default effort setting.

### Do AI companies actually change the model’s weights after release?

There’s no confirmed public evidence of this happening. Daily benchmark trackers that test identical prompts against the same model haven’t caught clear signs of the underlying weights changing, though some trackers acknowledge small changes could slip past their detection.

### What is reasoning effort in an AI model?

It’s a setting that controls how much computation a model uses to think through a problem before answering. Higher effort settings generally produce better accuracy on hard tasks but cost more and take longer. Benchmark scores are typically measured at maximum effort.

### Are companies legally required to disclose when they change these settings?

Remy.

Generally no. Some jurisdictions, including Connecticut, have passed laws requiring disclosure of usage limits before signup or renewal, but nothing currently mandates telling users when effort, routing, or system prompts change after launch.

### How do I know if my request got silently routed to a different model?

This is hard to detect without transparency from the provider. In at least one documented case, a safety filter rerouted a significant share of requests to a different model without clearly surfacing that to users, and only testing against a known benchmark exposed the pattern.
