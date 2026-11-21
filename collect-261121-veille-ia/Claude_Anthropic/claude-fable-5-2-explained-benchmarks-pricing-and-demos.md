---
id: collect-261121-veille-ia/claude-anthropic/claude-fable-5-2-explained-benchmarks-pricing-and-demos
title: "Claude Fable 5.2 explained: benchmarks, pricing and demos"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["fable", "claude", "video", "about", "models", "minutes", "reasoning", "model"]
source: https://oakgen.ai/youtube-summaries/anthropic-claude-fable-5-2-review-benchmarks-pricing-and-tests-2TM8W66NI_8
source_anchor: ""
source_lines: [1, 74]
section: "Claude_Anthropic"
---
1. Home
2. YouTube summaries
3. AI model news
4. Guide 3 of 8

0:00Intro

## Introduction to Claude Fable 5.2 and Mythos 5.2

0:33 to 1:49 in the video, about 1 minute

The AI landscape frequently sees incremental model upgrades, but significant leaps have been relatively rare in recent months. Anthropic's release of Claude Fable 5.2 and Mythos 5.2 represents a major new frontier in capability. While both models share the same foundational architecture, Mythos 5.2 is designed primarily for cybersecurity specialists with heightened safety guardrails, leaving Fable 5.2 as the public-facing powerhouse.

Interestingly, Anthropic itself notes that Fable 5.2 is not meant for standard, everyday prompting. Users are encouraged to maintain models like Claude Opus 3.5 as their primary daily driver, reserving Fable 5.2 for ambitious, open-ended technical challenges or as a top-level orchestrator that delegates simpler subtasks to smaller models.

## Benchmark Performance and Diminishing Returns

1:50 to 5:40 in the video, about 4 minutes

Across standard industry evaluations, Fable 5.2 captures top scores in multiple domains. On agentic coding benchmarks like FrontierCode 2.0, Fable 5.2 scores 49.1%, outpacing Opus 3.5's 45.5%. Even more dramatic gains appear in technical domains: in agentic scientific research, Fable reaches 74.6%, substantially higher than previous Anthropic models and competing frontier offerings.

However, test charts reveal notable diminishing returns when utilizing extended compute or 'maximum' reasoning modes. Allowing the model to think longer at peak effort sometimes results in worse benchmark accuracy despite driving up the execution cost. For many coding and reasoning tasks, setting reasoning to high or medium provides comparable quality at a significantly lower operational cost.

## Pricing, Availability, and the BuseyBench Evaluation

5:41 to 7:43 in the video, about 2 minutes

Fable 5.2 is available across Claude platforms, APIs, and select enterprise cloud partners. In the Claude interface, accessing Fable requires an active paid tier starting at the Pro tier level. For API users, input tokens are priced at $8 per million and output tokens at $40 per million. While roughly twice the cost of Opus 3.5, it remains slightly cheaper than its predecessor, Fable 5.1, as well as several competing high-end alternatives.

When put to the creator's idiosyncratic SVG portrait benchmark, BuseyBench, Fable 5.2 took nearly 20 minutes to process using over 122,000 tokens, landing in the middle of the leaderboard. This test highlights that extreme reasoning models are not always ideal for creative vector tasks compared to faster, lightweight alternatives.

## Real-World Engineering and Code Generation

7:44 to 9:08 in the video, about 1 minute

Anthropic documented striking internal trials with Fable 5.2, including an eight-day experiment where more than 1,000 sub-agents built a fully functional prototype operating system from an empty folder. Other scientific applications demonstrated novel approaches to predicting protein capsid structures.

When tasked with building a complete hardware desk toy from a short prompt, Fable 5.2 wrote a bill of materials using commercial off-the-shelf components, generated C firmware, authored clean Python scripts, provided printable 3D enclosure files, and compiled an illustrated step-by-step assembly manual with pinout diagrams.

## Browser Game Development and Community Highlights

9:09 to 11:52 in the video, about 3 minutes

In a live development test, Fable 5.2 was prompted to build a 3D horde survival game in the browser. In roughly 16 minutes at medium reasoning effort, the model produced a fully interactive WebGL title dubbed ShardStorm. The generated game featured responsive movement, leveling perks, double jumps, and working sound mechanics straight out of the box without manual debugging.

Community testers have demonstrated similar achievements across digital media. Highlights include third-person action game clones, full-fledged video editing interfaces, recreation of classic racing games, and voxelized world simulations where players can explore and alter urban cityscapes rendered entirely in code.

## Final Thoughts

Claude Fable 5.2 sets a new benchmark for what large language models can tackle autonomously, particularly when an objective demands end-to-end engineering, advanced scientific synthesis, or multi-component project orchestration. Because of its high token consumption and steep API price point, it will not replace daily conversational assistants for common queries.

Instead, Fable 5.2 functions best as a high-tier problem solver or project director. For complex developer tasks, sophisticated robotics logic, and hardware prototyping, it offers a tangible jump forward in autonomy and multi-step execution.

Do this yourself

## Background on the Fable model family

Matt Wolfe reviews Anthropic's benchmark table, the Artificial Analysis results, availability and pricing, then tries a game build and a gadget build. The Claude Fable 5 explainer covers the earlier models, and the Oakgen MCP guide shows how to connect Oakgen to Claude.

## Questions

## What is the difference between Claude Fable 5.2 and Mythos 5.2?

Both models share the same underlying architecture, but Mythos 5.2 includes enhanced guardrails specifically tailored for cybersecurity testers and enterprise safety requirements, whereas Fable 5.2 is available for broader technical use.

## How much does the Fable 5.2 API cost?

The API costs $8 per million input tokens and $40 per million output tokens. This makes it roughly twice the price of Claude Opus 3.5, though cheaper than Fable 5.1.

## Can free Claude users access Fable 5.2?

No, Fable 5.2 is restricted to paid subscriptions, starting with the Claude Pro tier, as well as API access.

This is a summary of a public video by Matt Wolfe. The method and the demo are theirs: watch the original. Oakgen wrote the summary.
