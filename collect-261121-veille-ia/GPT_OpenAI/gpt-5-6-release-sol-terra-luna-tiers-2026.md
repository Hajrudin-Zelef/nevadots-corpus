---
id: collect-261121-veille-ia/gpt-openai/gpt-5-6-release-sol-terra-luna-tiers-2026
title: "GPT-5.6 Release: Sol, Terra & Luna Tiers (2026)"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: ["2026-06-26"]
keywords: ["gpt-5", "openai", "terra", "luna", "tiers", "release", "july", "price"]
source: https://codersera.com/blog/gpt-5-6-release-date-whats-new-2026/
source_anchor: ""
source_lines: [1, 91]
section: "GPT_OpenAI"
---
**Quick answer.** GPT-5.6 is fully released. OpenAI previewed it on June 26, 2026 as a three-tier family — **Sol** (flagship, $5/$30 per 1M tokens), **Terra** (balanced, $2/$12), and **Luna** (fast and cheap, $0.20/$1.20 since the July 30, 2026 price cut) — and it reached **general availability on July 9, 2026** across ChatGPT, the API, Codex, and GitHub Copilot. For most production work, OpenAI positions **Terra** as GPT-5.5-level quality at a fraction of the cost; reach for **Sol** (or Sol Ultra) on the hardest agentic and coding tasks.

This post used to track GPT-5.6 while it was still a rumor. That phase is over. On **June 26, 2026**, OpenAI previewed GPT-5.6 for real — and it did not ship the way earlier point releases did. Instead of one frontier model with adjustable effort, GPT-5.6 arrives as **three named tiers**, each tuned to a different point on the cost, speed, and capability curve. It reached general availability on July 9, 2026.

Here is what is confirmed, what to actually run, and where the numbers still need independent verification.

## What is the GPT-5.6 release date?

The GPT-5.6 release date was **June 26, 2026**, when OpenAI previewed it — initially as a limited-access release to a small set of partner organizations through the API and OpenAI Codex. At the U.S. government's request, OpenAI started with a restricted preview rather than an open launch, and has said it does not expect that access process to become the long-term default. Broader **general availability arrived on July 9, 2026**, across ChatGPT, the API, Codex, and GitHub Copilot. So the correct framing is no longer "unreleased" — it is fully released.

## What are the GPT-5.6 tiers (Sol, Terra, Luna)?

The biggest change in GPT-5.6 is the naming and packaging. The number (5.6) marks the generation; **Sol, Terra, and Luna** are durable model tiers you pick between, not effort toggles on a single model.

| Tier | Built for (per OpenAI) | Price (input / output per 1M) | 
|---|---|---|
| **Sol** (flagship) | Frontier coding, agents, biology, cybersecurity | $5.00 / $30.00 | 
| **Terra** (balanced) | Everyday production — OpenAI targets GPT-5.5 quality at ~2.5× lower cost | $2.00 / $12.00 | 
| **Luna** (fast) | High-volume, latency-sensitive work; chatbots, classification | $0.20 / $1.20 | 

Prices above are current as of August 20, 2026: on July 30, 2026 OpenAI cut Luna by 80% and Terra by 20%, while Sol kept its launch price. Sol also offers higher reasoning-effort settings, and **Sol Ultra** is its top configuration for the hardest reasoning and agentic tasks — at the same price as base Sol ($5 / $30). Prompt caching is supported across all tiers, which matters a lot for cost on repetitive agent loops.

## GPT-5.6 benchmarks: how much better is it?

On **Terminal-Bench 2.1** (agentic coding), OpenAI reports:

- **Sol Ultra: 91.9%**
- **Sol (base): 88.8%**
- Claude Fable 5: 88.0%
- GPT-5.5: 88.0%

Read that honestly. Sol Ultra's reported ~3.9-point lead is meaningful but not a landslide, and base Sol's reported edge over GPT-5.5 (0.8 points) is narrow. OpenAI's system card also reports gains on SecureBio biology evaluations (exact scores unpublished) and says the models stay *below* its "Cyber Critical" threshold — i.e., not capable of autonomous, full-chain cyberattacks in OpenAI's own tests. These are OpenAI's own numbers; treat them as directional until independent, third-party testing lands after general availability.

## GPT-5.6 vs GPT-5.5: what actually changed?

Three things matter for a working developer:

- **Tiering.** You now choose Sol / Terra / Luna up front instead of tuning one model. Terra is the headline value: OpenAI positions it as GPT-5.5-class quality at roughly half the price.
- **Context window.** All three GPT-5.6 tiers ship a**1,050,000-token** context window with up to 128K output tokens.
- **Capability.** Modest reported gains on the hardest coding, agentic, biology, and cybersecurity evaluations — incremental rather than a generational jump.

We go deeper on the upgrade decision in GPT-5.6 vs GPT-5.5: what changed and should you upgrade, and on the coding head-to-head in GPT-5.6 vs Claude Opus 4.8. For the full tier breakdown, see the GPT-5.6 Sol, Terra and Luna guide.

## What's new since launch? (August 2026 update)

Everything below happened after GA, verified August 20, 2026:

- **July 30, 2026 — official price cuts.** Luna −80% ($1/$6 → $0.20/$1.20) and Terra −20% ($2.50/$15 → $2/$12) on the OpenAI API; Sol unchanged at $5/$30. Permanent list-price changes.
- **August 10, 2026 — GPT-5.6-Cyber.** A security-tuned model on the Sol base, available only via OpenAI's Daybreak Red trusted-access program. On OpenAI's own eval it scores 95.0% Advanced Cybersecurity Completion Rate vs 57.3% for GPT-5.5-Cyber — vendor-reported, not independently verified. No public API, no pricing.
- **August 13, 2026 — Sol "Ultrafast."** A service tier, not a new model: the same Sol served on Cerebras wafer-scale hardware at up to 750 output tokens per second — "up to 14× faster than Standard," per OpenAI/Cerebras' own numbers. Limited preview, pricing undisclosed. Distinct from Sol Ultra, the high-compute mode.
- **August 17, 2026 — OpenRouter Sol promo.** OpenRouter lists Sol at $2.50/$15 — half of list — as an OpenRouter-only, non-BYOK, time-limited promotion. OpenAI's API price is still $5/$30.
- **August 17, 2026 — vision results.** Roboflow's benchmark (its own harness): Sol scores 46.2 mAP@50 on object detection vs GPT-5.5's 13.8. The author's own caveat: Gemini 3.5 and 3.7 Flash still lead most vision metrics at a lower per-image price.

Full details and provenance labels in our GPT-5.6 Sol, Terra & Luna guide.

## What should you run right now?

- **Most production workloads → Terra.** OpenAI positions it as GPT-5.5 quality at ~2.5× lower cost ($2/$12), so it's the first tier to benchmark if cost reduction is the goal.
- **Hardest agentic coding / research → Sol or Sol Ultra.** Worth the premium only when the task genuinely needs the top of the range.
- **High-volume, latency-sensitive (chatbots, classification) → Luna.** Cheapest and fastest.
- **Still on GPT-5.5?** GPT-5.6 has been generally available since July 9, 2026 — benchmark Terra against your current model with your own prompts before switching; at $2/$12 it is usually the cost-saving move.

For where GPT-5.5 still fits and how the family evolved, see our GPT-5.5 complete developer guide. If you're weighing OpenAI against Anthropic, the Claude Opus complete guide is the companion read.

## How will you know when GPT-5.6 is fully available?

It already is. GPT-5.6 reached general availability on July 9, 2026 — `gpt-5.6-sol`, `gpt-5.6-terra`, and `gpt-5.6-luna` are callable in the OpenAI API model list, and the family is live in ChatGPT, Codex, and GitHub Copilot. For anything a third-party article claims about pricing or availability, the model list and OpenAI's deprecations page remain the primary sources to trust.

## FAQ

### When was GPT-5.6 released?

OpenAI previewed GPT-5.6 on June 26, 2026 as a limited partner release via the API and Codex, then made it generally available on July 9, 2026 across ChatGPT, the API, Codex, and GitHub Copilot.

### What are Sol, Terra, and Luna?

They are the three durable tiers of GPT-5.6. Sol is the flagship ($5/$30 per million tokens), Terra is the balanced everyday model ($2/$12, which OpenAI positions as GPT-5.5 quality at a fraction of the cost), and Luna is the fast, cheap option for high-volume work ($0.20/$1.20). Sol Ultra is Sol's top high-effort configuration at the same price.

### How much does GPT-5.6 cost?

Per million tokens (after OpenAI's July 30, 2026 cuts): Sol and Sol Ultra $5 input / $30 output, Terra $2 / $12, and Luna $0.20 / $1.20. Prompt caching is available on all tiers. OpenRouter also runs a time-limited, non-BYOK promo listing Sol at $2.50/$15 — OpenAI's own list price is unchanged.

### Is GPT-5.6 better than GPT-5.5?

Modestly, on OpenAI's own numbers. On Terminal-Bench 2.1, OpenAI reports Sol Ultra scored 91.9% and base Sol 88.8% versus GPT-5.5's 88.0%. The bigger practical win is pricing: after the July 30, 2026 cuts, Terra runs $2/$12 and Luna $0.20/$1.20 per million tokens.

### What context window does GPT-5.6 have?

All three GPT-5.6 tiers ship a 1,050,000-token context window, with up to 128K output tokens per response.

*Standardizing your team on frontier models and agentic coding tools?* *Codersera helps you hire vetted remote developers* *who already build with GPT, Claude, and Codex in the loop.*
