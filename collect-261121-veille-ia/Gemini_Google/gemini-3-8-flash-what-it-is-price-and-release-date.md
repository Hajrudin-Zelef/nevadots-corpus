---
id: collect-261121-veille-ia/gemini-google/gemini-3-8-flash-what-it-is-price-and-release-date
title: "Gemini 3.8 Flash: What It Is, Price and Release Date"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["google", "flash", "gemini", "model", "reasoning", "coding", "price", "line"]
source: https://www.layer3labs.io/guides/gemini-3-8-flash-explained
source_anchor: ""
source_lines: [1, 136]
section: "Gemini_Google"
---
# Gemini 3.8 Flash Explained: Google's Fast Model for Reasoning and Coding

What the model is, what changed since 3.7 Flash, and where to start

Gemini 3.8 Flash is a fast, low-cost Google AI model, built for reasoning, coding, and agent work. Google announced it on September 2, 2026 (Google).

At Layer3Labs, whenever Google releases a new Flash model, we ship a family of launch pages covering what changed before the hype settles.

This page is the starting point for our full set of Gemini 3.8 Flash pages. Start here, then follow the links to pricing, limits, benchmarks, and the head-to-head with 3.7 Flash.

## What Is Gemini 3.8 Flash?

Gemini 3.8 Flash is a Google AI model made for reasoning, coding, and agent workflows. It is the newest model in the Gemini Flash line, Google's fast and cost-efficient tier.

Google calls it its best reasoning and coding model yet, and says it delivers gains over Gemini 3.7 Flash across software engineering, agent tasks, and multi-step reasoning (Google). Those gains mean fewer retries on hard tasks, which lowers both cost and waiting time.

Gemini 3.8 Flash follows Gemini 3.7 Flash, which launched a few weeks earlier. So this is a quick step up within the same model line, not a new generation.

Google positions the Flash line for high-volume work where speed and price matter as much as raw ability. On the sites we build and operate ourselves, that is exactly the tier we reach for first when a job runs thousands of times a day.

- Model line: Gemini Flash (Google's fast, low-cost tier)
- Focus: reasoning, coding, agents, and knowledge work
- Predecessor: Gemini 3.7 Flash, from a few weeks earlier
- Maker: Google

## The "Best Reasoning and Coding Model Yet" Positioning

Google positions Gemini 3.8 Flash as its best reasoning and coding model yet (Google). That one line tells you who the model is for.

A Flash model runs a lot of everyday work at low cost. It is not the top flagship, and it is not the cheapest lite option either.

The pitch is that you get stronger reasoning and coding without paying flagship prices. That fits teams building tools, writing code, and running automated agents at volume.

Google also says the model works harder on complex tasks, calling tools in more rounds and taking extra reasoning steps before it answers (Google). More steps can mean a better result, but they also use more tokens, so watch cost on long agent runs.

- Flash line = high-volume, everyday tasks at low cost
- Built for reasoning, coding, and agent workflows first
- Sits between a flagship model and a lite model
- Works harder on hard tasks, using more tool calls and reasoning steps (Google)

## Key Facts at a Glance

Here are the core facts about Gemini 3.8 Flash in one place. Every number below comes from Google's own materials, so verify current details on Google's pages before you rely on them.

On price, the introductory API rate runs through December 31, 2026: $0.75 per 1M input tokens and $3.75 per 1M output tokens (Google). From January 1, 2027, the regular rate is $1.50 per 1M input and $7.50 per 1M output (Google).

Those rates match what Gemini 3.7 Flash charges (Google). So the newer model launches at the same price as the one before it, with the intro rate at half the regular rate.

You can reach the model in the Gemini app on a Google AI Pro or Ultra plan, and developers can call it through the Gemini API in Google AI Studio (Google). Google's announcement did not state a context window, a maximum output, or rate limits, so confirm those on Google's model pages before you design a workload.

- Announced: September 2, 2026 (Google)
- Intro API price (through Dec 31, 2026): $0.75 input / $3.75 output per 1M tokens (Google)
- Regular API price (from Jan 1, 2027): $1.50 input / $7.50 output per 1M tokens (Google)
- App access: Gemini app for Google AI Pro or Ultra subscribers (Google)
- Developer access: Gemini API via Google AI Studio (Google)
- Context window, max output, and rate limits: not stated by Google (Google)

## What's New vs Gemini 3.7 Flash

Gemini 3.8 Flash is the step up from Gemini 3.7 Flash inside the same Flash line. Google reports gains across software engineering, agent tasks, and multi-step reasoning (Google).

The clearest published number is on reasoning. Google reports 54.9% on HLE-Verified, a test of multi-step reasoning (Google).

For the other tests, Google states the model leads without giving a score. It says 3.8 Flash outperforms most larger frontier models on DeepSWE v1.1, a software-fixing benchmark, and beats 3.7 Flash on finance and legal agent tests (Google).

One real change is behavior, not just scores. Google says the model works harder on hard tasks, running more tool calls and reasoning steps before it answers (Google). Treat these as Google's own claims, and run a short pilot on your own tasks before you switch.

- HLE-Verified reasoning: 54.9% (Google)
- DeepSWE v1.1: Google says it leads most larger frontier models, no score published (Google)
- Finance and legal agent tests: Google says it beats 3.7 Flash, no score published (Google)
- Behavior: more tool calls and reasoning steps on complex tasks (Google)

## The Gemini 3.8 Flash Cyber Sibling

Google launched a second model alongside the main one, called Gemini 3.8 Flash Cyber. It is a security-focused variant, not a replacement for the standard model.

Google restricts Cyber to trusted defenders through a program it calls Fairwind (Google). So it is not a general-access model you can pick up in the app.

The variant targets security work such as finding and patching software vulnerabilities (Google). For most teams, the standard Gemini 3.8 Flash is the one you will actually use.

We do not cover the Cyber variant in depth here, because it is gated and its details are limited at launch. If you run a security team, watch Google's own pages for access terms rather than third-party summaries.

- Gemini 3.8 Flash Cyber is a separate, security-focused variant (Google)
- Access is restricted to trusted defenders via Google's Fairwind program (Google)
- It targets vulnerability detection and patching (Google)
- The standard Gemini 3.8 Flash is the general-use model

## Who Gemini 3.8 Flash Is For

Gemini 3.8 Flash fits teams that reason over data, write code, or run AI agents at volume. The mix of reasoning gains and low intro pricing is the draw.

Developers get a cheap model to test in Google AI Studio. During the intro window, the API price is half the regular rate (Google).

Everyday users reach it in the Gemini app on a Google AI Pro or Ultra plan (Google). Google also surfaces the model in AI Mode in Google Search and in Google Sheets, so some people will use it without ever touching the API (Google).

It is a weaker fit if you need a proven flagship for the hardest reasoning, or if your workload depends on a fixed context limit Google has not yet published. If that is you, confirm the limits with Google before you commit.

If you are weighing the newer Gemini 4 model, see how Gemini 4 Hydrogen compares.

- Developers building tools, agents, or web apps at scale
- Teams that want reasoning and coding gains without flagship prices
- Gemini app users on a Google AI Pro or Ultra plan
- Less ideal if you need a Google-confirmed context window today

## Where to Go Next

Start with the page that matches your question, then come back to this hub. Each linked page covers one topic in depth.

For cost details, read the Gemini 3.8 Flash pricing guide. For size and rate caps, read the limits guide.

For a buyer's take, read the review and the worth-it breakdown. For a direct upgrade view, read Gemini 3.8 Flash vs Gemini 3.7 Flash.

If you want to try the model today, the how-to-use guide walks through every access path. The links section below points to each page.

For the newer model, read about Gemini 4 Hydrogen, Google's recommended upgrade from 3.8 Flash.

## How to Use Gemini 3.8 Flash

To use Gemini 3.8 Flash effectively, start by choosing an interface that exposes the exact model you need for your workload. Test a new configuration on a single representative task, such as drafting a routine email or parsing a short document, before expanding to full automation. Inspect the saved output directly to confirm formatting, accuracy, and adherence to your prompt instructions.

For coding tasks, Cursor serves as an editor option when the target model appears on its supported list. Check current model availability before setup to confirm the exact model is listed. Once connected, save your prompts and review the generated diffs locally before applying changes to your main codebase.

## Frequently Asked Questions

- Gemini 3.8 Flash is a fast, low-cost Google AI model, built for reasoning, coding, and agent work. Google announced it on September 2, 2026, as the next step up from Gemini 3.7 Flash (Google).
- Google announced Gemini 3.8 Flash on September 2, 2026 (Google). It arrived a few weeks after Gemini 3.7 Flash in the same model line.
- The intro API rate runs through December 31, 2026, at $0.75 per 1M input tokens and $3.75 per 1M output tokens (Google). From January 1, 2027, the regular rate is $1.50 input and $7.50 output per 1M tokens (Google). Prices change without notice, so confirm on Google's pricing page.
- Google says Gemini 3.8 Flash improves on 3.7 Flash across software engineering, agent tasks, and multi-step reasoning, and that it works harder on complex tasks (Google). Google published one score, HLE-Verified at 54.9%, and stated the other gains without numbers. Pilot on your own tasks before switching.
- Gemini 3.8 Flash Cyber is a separate, security-focused variant Google launched alongside the standard model (Google). Access is restricted to trusted defenders through Google's Fairwind program, and it targets vulnerability detection and patching. Most teams will use the standard Gemini 3.8 Flash instead.
- Google's announcement did not state a context window, a maximum output, or rate limits for Gemini 3.8 Flash (Google). Confirm the current limits on Google's model documentation before you design a workload around them.

### Planning a Gemini 3.8 Flash Rollout?

Book a free 30-minute AI workflow audit with Layer3 Labs. We will help you test Gemini 3.8 Flash on your own tasks, weigh it against your current model, and plan a safe, cost-aware rollout.

Book an Audit
