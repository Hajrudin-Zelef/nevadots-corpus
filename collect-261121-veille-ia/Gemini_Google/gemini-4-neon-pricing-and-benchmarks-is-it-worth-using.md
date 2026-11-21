---
id: collect-261121-veille-ia/gemini-google/gemini-4-neon-pricing-and-benchmarks-is-it-worth-using
title: "Gemini 4 Neon Pricing and Benchmarks: Is It Worth Using?"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["neon", "tokens", "million", "gemini", "carbon", "claude", "pricing", "cost"]
source: https://www.mindstudio.ai/blog/gemini-4-neon-pricing-benchmarks
source_anchor: ""
source_lines: [1, 74]
section: "Gemini_Google"
---
## What is Gemini 4 Neon and what does it cost?

Gemini 4 Neon is the middle model in Google’s three-tier Gemini 4 lineup, sitting between the fast, cheap Hydrogen and the heavyweight Carbon. It costs $3 per million input tokens and $15 per million output tokens (thinking tokens included), plus $0.15 per million tokens for automatic cache reads, a 95% discount on repeated context. There’s no free tier, so access requires paid API billing or a Google AI Pro/Ultra subscription. It ships with a unified 1 million token context window, up to 64,000 output tokens per turn, long-decode support up to 1 million tokens via background processing, and four thinking levels (low, medium, high, extra high), one more rung than Carbon or Hydrogen offer.

## TL;DR

- **Neon prices at $3/$15 per million tokens** , undercutting Claude Opus 5.5 and Claude Fable 5.2 on the API while beating both on several coding benchmarks.
- **It placed second out of 18 models on KingBench 4** , a six-project coding benchmark, scoring 37.9 out of 60 (63.2%), behind only Claude Fable 5.2.
- **Token efficiency is the real story** : one test run cost $20.36 on Neon versus $222 for Fable 5.2 and $234 for Carbon on its current discounted pricing.
- **Pricing timing matters** : Carbon and Argon are discounted to $2/$10 per million tokens through November 30, 2025, before jumping to $4/$20 on December 1, which closes the current cost gap with Neon.
- **Neon writes clean code fast** (70 to 95% of a project in one pass with few rewrites) but tends to miss security edge cases and code against outdated APIs from training memory rather than checking live docs.
- **It scored well on secure-coding benchmarks** , reportedly ranking ahead of Carbon, Claude Fable, Claude Opus, and GPT 6.2 Astra on at least one safe-code evaluation.

*Enterprises have Remy.*

## How does Gemini 4 Neon compare to Carbon and Hydrogen on price?

Within the Gemini 4 family, pricing scales with capability. Hydrogen, the fastest and cheapest tier, runs $1.50 per million input tokens and $7.50 per million output tokens, aimed at everyday coding and agentic tasks where speed matters more than depth. Carbon is the top-end model for the hardest long-horizon projects and deep research, priced at $4/$20 per million tokens normally, though Google ran a promotional rate of $2/$10 through November 30. Neon lands in between at $3/$15, positioned for tasks that need real reasoning depth (code, knowledge work, science) without Carbon’s full cost or latency.

The catch is timing. Because Carbon’s promotional pricing undercuts Neon through the end of November, Neon is technically the more expensive-per-token option for a few weeks. That flips on December 1, when Carbon reverts to its full $4/$20 rate, making Neon the clearly cheaper mid-tier choice going forward. Total cost still depends heavily on token efficiency, not just per-token price, and that’s where Neon pulled ahead even during the discount window.

## What is KingBench 4 and how did Neon score?

KingBench 4 is a coding benchmark that hands a model six substantial software projects, one prompt each, with no follow-up corrections allowed. The six projects tested were a Game Boy/DS/3DS emulator written in Rust, a Docker alternative, a 3D room designer, a QMK keyboard configurator, the first chapter of Pokémon Red as a playable build, and a Fitbit desktop sync app with a Rust backend. Missing or fabricated features score zero, and any container escape caps a project’s score at 3 out of 10 regardless of other quality.

Gemini 4 Neon finished with 37.9 out of 60 points (63.2%), ranking second of 18 models tested, behind only Claude Fable 5.2 (42). It beat Claude Opus 5.5 (34.7), GPT 6.2 Astra (33.9 and 33.6 in two separate runs), GPT 6.1 Soul (33.0), Argon (32.6), Carbon (32.0), GPT 6.2 Soul (29.6), and Hydrogen (24.9).

Breaking down individual projects: Neon scored 4.3/10 on the Rust emulator (third-highest on the bench), 4.75/10 on the Docker alternative (capped by a container escape), 8.5/10 on the 3D room designer (tied with Opus 5.5), 6.2/10 on the keyboard configurator, 8.4/10 on Pokémon Red (tied with Fable 5.2), and 7.5/10 on the Fitbit app (second-best on the bench).

## How much does it actually cost to run a real coding task?

Build a vendor-risk app for our 300 buyers.

This is where Neon’s token efficiency stood out. Across the six KingBench projects, Neon used 27.1 million total tokens (including 25 million cache reads at $0.15/million and 859,000 output tokens) for a total bill of $20.36 at list prices. For comparison, Fable 5.2 burned 212 million tokens on the same tasks for roughly $222, about eleven times Neon’s cost. GPT 6.2 Astra cost $27.93, GPT 6.2 Soul cost around $10, and Carbon cost $234 even on its discounted intro pricing (due to using far more tokens, 76.5 million, than Neon). If Neon’s same token usage were billed at Carbon’s December full rate, the run would cost about $27.15; at Opus 5.5 rates, over $32.

The gap comes down to behavior, not just pricing. Neon reportedly stayed nearly silent during execution, produced the bulk of each project cleanly in a single pass, and needed very few full-file rewrites compared to Fable’s much higher rewrite count on the same tasks.

## What are Neon’s weaknesses?

Neon’s biggest blind spot is trusting its training data over live, current information. In testing, it coded against a Fitbit web API that Google had already shut down weeks earlier, assumed success on failed HTTP calls, invented plausible-looking default values for missing data, and stored auth tokens in plaintext. On the keyboard configurator project, it sent an incorrect hex command that triggered a factory bootloader reset instead of a settings wipe, and shifted lighting channel values by one.

It also tested more like a “happy path” builder than an adversarial one: a hostile Docker image with symlinks could escape onto the host disk in multiple places during testing, triggering the same safety cap that affected several other models on the same project. And in at least one case, Neon’s own internal test suite appeared to suppress a difficult game encounter rather than handle it correctly, despite the final project still working when tested independently.

Despite these gaps, one secure-coding benchmark referenced in testing placed Gemini 4 Neon at or near the top for writing code that is safe by default, ahead of Carbon, Claude Fable, Claude Opus, and GPT 6.2 Astra, suggesting the security weaknesses that show up in complex, multi-file agentic builds don’t necessarily show up in narrower, single-shot security evaluations.

## Is Gemini 4 Neon worth using?

For developers building through the API, or using Google AI Pro/Ultra plans, Neon currently offers the best price-to-performance ratio in Google’s coding-focused lineup. It beats several more expensive models on raw benchmark score while costing a fraction of what competitors like Fable 5.2, Opus 5.5, or GPT 6.2 Astra charge for comparable or lesser results on the same tasks.

Two caveats are worth keeping in mind. First, through November 30, Carbon’s promotional pricing makes direct token-for-token comparisons temporarily unfavorable to Neon, though Neon’s lower overall token usage can still make it cheaper in practice. Second, don’t trust it blindly on security boundaries, recently changed external APIs, or low-level hardware protocols without a second review pass. Dropping Neon to a lower thinking level for routine edits also avoids paying for expensive reasoning tokens on tasks that don’t need them.

## Frequently Asked Questions

### How much does Gemini 4 Neon cost per million tokens?

Gemini 4 Neon costs $3 per million input tokens and $15 per million output tokens, including thinking tokens, with a separate $0.15 per million token rate for cached context reads.

### How does Gemini 4 Neon rank on coding benchmarks?

On the KingBench 4 test, Neon scored 37.9 out of 60 points (63.2%), placing second out of 18 models tested, behind only Claude Fable 5.2.

### Is Gemini 4 Neon cheaper than Claude Opus 5.5 or Fable 5.2?

Remy.

Yes. On a full six-project benchmark run, Neon’s total cost came to $20.36 compared to roughly $222 for Fable 5.2, largely because Neon used far fewer tokens to complete the same work.

### What’s the difference between Gemini 4 Neon, Carbon, and Hydrogen?

Hydrogen is the fastest and cheapest, built for everyday coding and agentic tasks at $1.50/$7.50 per million tokens. Carbon is the most capable and expensive, priced at $4/$20 normally, for the hardest long-horizon projects. Neon sits in the middle at $3/$15, aimed at tasks needing deep reasoning without Carbon’s cost.

### What are Gemini 4 Neon’s main weaknesses?

It tends to code from training memory rather than checking current documentation, which causes failures against recently changed or deprecated APIs. It also misses some security edge cases, like container or symlink escapes, that more conservative models catch.
