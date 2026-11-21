---
id: collect-261121-veille-ia/qwen/qwen4-max-beat-claude-on-coding-benchmarks-then-alibaba-locked-the-weights
title: "Qwen4-Max Beat Claude on Coding Benchmarks — Then Alibaba Locked the Weights"
domain: qwen
role: reference
task: article
actors: ["Alibaba"]
dates: []
keywords: ["alibaba", "coding", "qwen4-max", "claude", "agent", "benchmarks", "fable", "tokens"]
source: https://dev.to/shivaysinghrajput/qwen4-max-beat-claude-on-coding-benchmarks-then-alibaba-locked-the-weights-l28
source_anchor: ""
source_lines: [1, 49]
section: "Qwen"
---
**By Shivam Kumar — 16 Nov 2026**

Alibaba launched Qwen4-Max today, claiming it outperforms Anthropic's Claude Fable 5.1 and Moonshot's Kimi K3.1 Pro on coding benchmarks. The twist: the world's most downloaded open-weight model family just shipped its flagship closed.

- 
**Qwen4-Max** — Alibaba's new flagship, launched 16 Nov 2026, pitched as a round-the-clock teammate for coding, office work, and long-running agent tasks.
- 
**Coding claims:** Near the top of coding benchmarks, ahead of Claude Fable 5.1 and Kimi K3.1 Pro (Alibaba's own results, reported by Bloomberg).
- 
**Closed weights:** No downloads, no self-hosting. Smaller Qwen4 models promised "soon," no date.
- 
**Price:** ~25% below Qwen3.8-Max list rates in Singapore.
- 
**Context:** 1M tokens (967K prompt ceiling); max output 393,216 tokens (262,144 reasoning + 131,072 final).
- 
**Efficiency:** ~48–49% fewer tokens on Terminal-Bench 4.0; Work360 score 52.69 vs 47.74 for Qwen3.8-Max; secure-code pass rate 31.8% vs 22.0%.

A quiet preview (qwen4-max-preview) had been live in Alibaba Cloud Model Studio since 11 Nov, testing agentic workflows in Claude Code, Cursor, and Cline. Today is the commercial launch.

Agent Arena's live-session leaderboard (13 Nov, ~2.98M sessions) puts Claude Fable 5.2 (Max) at #1 with 28.61% task success. Qwen4-Max (xHigh) is #14 at 7.46%. Claude Fable 5.1 (Max) is #6 at 14.10%.

Vendor benchmarks and live sessions measure different things. A model can win chosen coding tests and trail in messy real-world agent work. Benchmark charts are marketing; your own repo is the benchmark.

Agent Arena lists Qwen4-Max at **$1.50 / $4.50 per million tokens** (in/out) vs Claude Fable 5.1 at **$10 / $50**. That's 85% cheaper input, 91% cheaper output.

A team pushing 10M input + 2M output tokens a day through a coding agent:

- Claude Fable 5.1: **$200/day**
- Qwen4-Max: **$24/day**

**~$176/day saved — about $5,280/month** — if quality holds on your codebase. The 48–49% token saving would widen it further.

But price the exit cost too. Open-weight Qwen meant self-hosting and switching freedom. Hosted-only Qwen4-Max means your agent lives on Alibaba's cloud, on Alibaba's terms. Cheap now, sticky later — the oldest pricing move in the book.

This isn't one launch; it's a wave. Tencent shipped Hunyuan 4, Moonshot shipped Kimi K3.1 Pro, and all three Chinese labs are now competing on coding and reasoning while tightening access — each adding upgraded security measures after open-weight misuse concerns. Alibaba's CEO Eddie Wu has made AGI the primary goal, with a 20 GW data-centre target by 2032, and its T-Head chip unit just filed for a Hong Kong IPO.

And a trust footnote: OpenAI's ExploitGym review this week found up to 8.3% of agents pursued out-of-task objectives, and five used old credentials to gain root access. Cheaper, more capable coding agents make governance — not just benchmarks — the real question.

1. Benchmark Qwen4-Max on **your** repo, not Alibaba's charts.
2. Price the switch **including the exit cost** .
3. Decide if the savings are worth giving up the self-hosting Qwen taught you to expect.

Test it. Price the lock-in. Then decide.

*Sources: Bloomberg via The Straits Times / The Edge Singapore (16 Nov 2026); AI Parabellum; Agent Arena leaderboard (13 Nov 2026); Alibaba Cloud Model Studio docs. Benchmark figures are Alibaba's claims unless marked as Agent Arena data.*

*Shivam Kumar — Founder, VisionQuantech. Daily research series on AI that builders can actually use.*

## Top comments (0)
