---
id: collect-261121-veille-ia/claude-anthropic/claude-opus-5-5-explained-faster-cheaper-and-what-it-means
title: "Claude Opus 5.5 Explained: Faster, Cheaper and What It Means"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-09-22"]
keywords: ["opus", "claude", "anthropic", "tokens", "percent", "faster", "million", "output"]
source: https://applescoop.org/story/claude-opus-5-5-explained-features-pricing-what-changes
source_anchor: ""
source_lines: [1, 75]
section: "Claude_Anthropic"
---
# Claude Opus 5.5 Explained: Faster, Cheaper and What It Means

- Anthropic released Claude Opus 5.5 on September 22, saying it matches its larger Fable 5.1 on most work at a lower cost
- API prices fall to $4 and $20 per million input and output tokens, and it runs about 30 percent faster than Opus 5
- Paid Claude plans got higher usage limits, and Sonnet 5.5 and Haiku 5.5 followed on September 28 and October 7

Claude Opus 5.5 is Anthropic's main Opus model and the first release in its Claude 5.5 family. Anthropic says it performs at the level of its larger Fable 5.1 model on most work while costing less and running faster than Opus 5. Sonnet 5.5 and Haiku 5.5 have since completed the family, which changes which model makes sense for many jobs. Here is what Opus 5.5 does, what it costs and what it means if you use Claude on your iPhone or Mac.

## What Anthropic announced

Claude Opus 5.5 launched on September 22, 2026. It accepts text and images and is designed for complex professional work such as coding, analysis and working through long documents. It has a one-million-token context window, enough to process very large codebases or document collections in one go, and the API model name is claude-opus-5-5.

Anthropic says Opus 5.5 is more than 30 percent faster at generating output than Opus 5, and that at default settings it costs about 40 percent less to run on typical workloads. One of its examples is a 200,000-line code audit that took Opus 5.5 under three hours, against more than 20 hours for Opus 5, which also used two and a half times as many tokens.

*Photo via Anthropic // A glowing orange and blue horizon against black, artwork from Anthropic's Claude Opus 5.5 announcement.*

## Pricing

For developers using the API, Opus 5.5 costs $4 per million input tokens and $20 per million output tokens, down from $5 and $25 for Opus 5. Cache reads, which make up much of the cost of coding agents, fall from $0.50 to $0.20 per million tokens, and cache writes from $6.25 to $5. There is no extra charge for long prompts above 200,000 tokens.

The headline saving needs one qualification. The 40 percent figure compares each model at its default setting, and Opus 5.5 defaults to a lower "medium" effort level than Opus 5's "high." Teams that keep a higher effort setting will see smaller savings, because Opus 5.5 tends to think more per turn at the same setting. Thinking also can no longer be switched off entirely, so effort is now the main dial for cost and speed.

A worked example makes the difference concrete. An agent job that reads 10 million tokens, most of them from cache, and writes 500,000 tokens of output would have cost about $5 for cached input on Opus 5 and $12.50 for output. On Opus 5.5 the same token counts cost about $2 and $10. In practice the saving is often larger, because Opus 5.5 tends to take fewer steps to finish the same task.

## How it compares

On Anthropic's own benchmark table, Opus 5.5 leads on several coding, knowledge-work and computer-use tests. It scores 66.4 percent on Terminal-Bench 4.0, against 57.9 percent for OpenAI's GPT-6 Astra and 55.8 percent for Fable 5.1, and 54.4 percent on FrontierCode against Astra's 53.3. It trails Astra on two agent tests, AutomationBench and Terminal-Bench-Science. Most of those scores were measured at maximum effort; at the default medium setting, Anthropic lists a lower CursorBench result of 52.5 percent instead of 57.8. Anthropic itself cautions that benchmark margins have become a less reliable guide to real-world differences at this level.

The competition has also moved since launch. OpenAI released GPT-6.1 Sol a week later at $2 per million input tokens and $10 per million output, pitching it as close to Astra at a fraction of the price. Early independent testing by Vals AI found large Opus 5.5 gains on long, multi-step agent tasks, with smaller dips on some retrieval-heavy professional tasks. The best way to judge remains to try it on your own work, and our comparison of Fable 5.1, Opus 5.5 and Sonnet explains where each model fits.

## Sonnet 5.5 and Haiku 5.5 complete the family

Anthropic released Claude Sonnet 5.5 on September 28 and Haiku 5.5 on October 7. Sonnet 5.5 costs $2 per million input tokens and $10 per million output, half the price of Opus 5.5, and Anthropic cut its cache-read price to $0.10 when Haiku arrived. It comes surprisingly close to Opus: on Anthropic's figures it scores 70.6 percent on Terminal-Bench 4.0, above Opus 5.5's 66.4, and almost matches it on professional-work tests, while trailing on the hardest open-ended problems.

*Photo via Anthropic // A close-up of a metal speed dial, artwork from Anthropic's Claude Sonnet 5.5 announcement. Sonnet 5.5 is the faster, cheaper sibling to Opus 5.5.*

Haiku 5.5 is the budget option at $0.10 per million input tokens and $0.50 per million output for prompts up to 100,000 tokens, rising to $0.50 and $2.50 above that. It is meant for summaries, classification and helper tasks inside larger agents, and it is the first Haiku model with an adjustable effort setting. For most people the practical rule is simple: Haiku for bulk and background work, Sonnet for everyday coding and writing, Opus when a task is long, ambiguous or expensive to get wrong.

## What changes for Claude app users

Anthropic raised five-hour usage limits on its Pro, Max, Team and seat-based Enterprise plans, without publishing the new figures, and offered subscribers a one-time usage reset they can save for later. That reset expires on October 22. Because Opus 5.5 is cheaper for Anthropic to run, the same allowance also stretches further than it did with Opus 5. Our explainer on why Claude and ChatGPT keep changing usage limits covers the meters in detail.

Opus 5.5 is now the default model in Claude Code for Pro, Max, Team, Enterprise and API accounts, running at medium effort unless you change it. The writing has changed too: Anthropic says Opus 5.5 puts the key information first, uses less jargon and follows your writing rules more closely, a response to complaints that explain why people disliked Opus 5.

## Where you can use it

Opus 5.5 is available in the Claude apps, on the Claude API and through Amazon Web Services, Google Cloud and Microsoft's cloud. Not every cloud has caught up inside Claude Code, though: on Microsoft Foundry, Claude Code's opus shortcut still points to an older model, so enterprise teams should set the full model name. A fast mode in Claude Code and on the Claude Platform runs Opus 5.5 at up to 2.5 times the speed for $8 per million input tokens and $40 per million output, double the normal rate.

Some safety limits apply. Anthropic routes most cybersecurity requests to the older Opus 4.8 rather than Opus 5.5, with a verification program for security professionals who need more. For API accounts created on or after August 31, preserved thinking blocks attempts to edit earlier context to extract the model's reasoning, a safeguard against distillation by rival labs.

## Claude and your Apple devices

Claude is increasingly part of the Apple ecosystem. The Claude app runs on iPhone, iPad and Mac, and Claude works in CarPlay for hands-free conversations. iOS 27 also lets you add third-party AI assistants such as Claude as extensions to Siri AI, so you can send some requests to Claude from Apple's own assistant. Opus 5.5 is not something you choose inside Siri, but in the Claude app it appears as the default Opus option in the model menu.

## For developers: test before you switch

Opus 5.5 is not a drop-in swap for every Opus 5 system. Thinking can no longer be turned off, the default effort is lower, and Anthropic's migration guide lists further behavior changes to check. Long unattended agent loops deserve particular testing, because a model that writes more concisely can also end a turn with a progress report rather than another tool call. Anyone switching production systems should read the migration notes, compare costs at the effort level they actually use and test before changing the model ID. If Sonnet 5.5 handles the workload, it may be the better switch at half the price.

## Why it matters

The AI race is increasingly about cost and speed, not just capability. A model that matches a flagship at a lower price makes advanced AI practical for more apps, including the assistants and tools now arriving on iPhone and Mac, and Sonnet 5.5 pushes that further a week later. For most people, the visible change from Opus 5.5 is more generous usage, faster answers and shorter, clearer replies in the Claude app. For developers, it is a reason to rethink which model handles which job, rather than defaulting to the most expensive one.

*Photo via Wikimedia Commons // Anthropic CEO Dario Amodei speaking at TechCrunch Disrupt in 2023. Cheaper, faster models like Opus 5.5 are central to his company's pitch to businesses.*

## Keep following what matters.

Continue with reporting selected around this story, join the conversation, or share it with someone who should see it.

**See more Apple Scoop on Google**Add us as a preferred source in Top Stories

*Add*More from

**Anthropic**

*Claude, and the AI safety company that builds it.*
