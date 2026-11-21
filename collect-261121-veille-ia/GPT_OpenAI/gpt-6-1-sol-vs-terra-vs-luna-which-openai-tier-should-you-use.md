---
id: collect-261121-veille-ia/gpt-openai/gpt-6-1-sol-vs-terra-vs-luna-which-openai-tier-should-you-use
title: "GPT-6.1 Sol vs Terra vs Luna: Which OpenAI Tier Should You Use?"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["tier", "gpt-6", "terra", "luna", "astra", "openai", "generation", "only"]
source: https://www.truefoundry.com/blog/gpt-6-1-sol-terra-luna
source_anchor: ""
source_lines: [1, 79]
section: "GPT_OpenAI"
---
# GPT-6.1 Sol vs Terra vs Luna: Which OpenAI Tier Should You Use?

### Built for Speed: ~10ms Latency, Even Under Load

Blazingly fast way to build, track and deploy your models!

- Handles 350+ RPS on just 1 vCPU — no tuning needed
- Production-ready with full enterprise support

## How OpenAI’s tier system works

The naming can be confusing at first, so here is the mental model. The **number** is the generation (GPT-5.6, GPT-6, GPT-6.1). The **name** is the tier within that generation, ordered roughly from most to least capable: **Astra → Sol → Terra → Luna**. A higher generation of a lower tier can match or beat a lower generation of a higher tier — which is exactly why cheaper tiers keep getting more useful over time.

For most teams, the decision is not “which generation” but “which tier for this task.”

## The GPT-6.1 tiers, one by one

### Astra — the flagship

Astra is the top tier: maximum capability, maximum price, reserved for the hardest reasoning. (OpenAI reportedly held back a GPT-6.1 Astra over safety testing, so the flagship you reach for may be GPT-6 Astra.) Use it only where quality is non-negotiable and the token cost is noise next to the cost of a wrong answer.

### Sol — high capability

Sol is the strong, high-capability tier below Astra. GPT-6.1 Sol is priced at **$2 input / $10 output** per million tokens. Reach for it when a task is genuinely hard — multi-step reasoning, complex coding, planning — but you don’t need the absolute flagship.

### Terra — balanced, production

Terra is the workhorse: **$1 input / $5 output** per million tokens, a 1.05M-token context, and first-class **computer use**. It is tuned to be fast and cheap enough to run the routine majority of agentic work at volume. For most production traffic, Terra is the sensible default.

### Luna — fastest and cheapest

Luna is the smallest, fastest, cheapest tier, built for high-volume, latency-sensitive, well-scoped tasks — classification, extraction, routing, short answers. For GPT-5.6, Luna undercut Terra further still; expect the GPT-6.1 Luna tier to play the same role as the value floor. Use it wherever the task is simple and the volume is high.

## GPT-6.1 tiers at a glance

*Pricing reflects published GPT-6.1 figures where available; verify current rates before relying on them. Luna’s GPT-6.1 rate may vary by release.*

## How to choose the right tier

A simple, durable rule: **start one tier below where you think you need to be, and escalate only on measured failure.** Pick the cheapest tier, run your real evaluation set, and promote a task to a higher tier only where quality actually drops below your bar — not on a hunch. Most teams that do this discover a large share of traffic runs fine on Terra or Luna, with only a thin tail needing Sol or Astra.

- **Classification, extraction, routing, short answers:** Luna.
- **General production work, RAG, agentic loops, computer use:** Terra.
- **Hard multi-step reasoning and complex coding:** Sol.
- **The non-negotiable hard tail:** Astra.

## Routing across tiers in production

The savings only materialize if routing is automatic. Hard-coding one tier everywhere either overpays (flagship for everything) or underperforms (cheap tier on hard tasks). The pattern that works is a gateway that routes each request to the right tier by complexity and cost, with automatic fallback and per-team budgets.

The TrueFoundry AI Gateway puts every GPT-6.1 tier — and your Claude, Gemini and self-hosted models — behind one OpenAI-compatible endpoint, with cost-aware routing, fallbacks, budget enforcement, and a single cross-provider cost ledger. You change the tier mix from config, not code.

## Related reading

TrueFoundry AI Gateway delivers ~3–4 ms latency, handles 350+ RPS on 1 vCPU, scales horizontally with ease, and is production-ready, while LiteLLM suffers from high latency, struggles beyond moderate RPS, lacks built-in scaling, and is best for light or prototype workloads.

## Recent Blogs

## Frequently asked questions

### What do Astra, Sol, Terra and Luna mean?

They are OpenAI’s capability tiers within a generation, ordered roughly Astra (flagship) > Sol (high) > Terra (balanced) > Luna (fastest/cheapest). The number, like 6.1, is the generation.

### Which GPT-6.1 tier is cheapest?

Luna is the cheapest, fastest tier. Among the tiers with published GPT-6.1 pricing, Terra ($1/$5 per million tokens) is cheaper than Sol ($2/$10).

### Which tier should I use by default?

For most production traffic, Terra is a sensible default — balanced capability, low price, and computer use. Drop to Luna for simple high-volume tasks and escalate to Sol or Astra only for the hard tail.

### How do I use multiple tiers without rewriting code?

Route them through an AI gateway. You point your app at one endpoint and the gateway sends each request to the right tier, with fallback and budgets, so the mix is a config change rather than a code change.

### Is there a GPT-6.1 Astra?

OpenAI reportedly held back a GPT-6.1 Astra over safety testing, so the current flagship you reach for may be GPT-6 Astra. Verify availability at the time you build.
