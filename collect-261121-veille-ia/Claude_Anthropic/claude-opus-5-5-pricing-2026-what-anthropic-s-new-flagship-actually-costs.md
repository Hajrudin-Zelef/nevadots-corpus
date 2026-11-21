---
id: collect-261121-veille-ia/claude-anthropic/claude-opus-5-5-pricing-2026-what-anthropic-s-new-flagship-actually-costs
title: "Claude Opus 5.5 Pricing 2026: What Anthropic's New Flagship Actually Costs"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["opus", "claude", "pricing", "tokens", "anthropic", "output", "cache", "costs"]
source: https://www.finout.io/blog/claude-opus-5.5-pricing-2026-what-anthropics-new-flagship-actually-costs
source_anchor: ""
source_lines: [1, 115]
section: "Claude_Anthropic"
---
Quick answer: Claude Opus 5.5 costs $4 per million input tokens and $20 per million output tokens on the standard API, a 20% cut from Opus 5, with cache reads down 60% to $0.20. It's available through the API, Amazon Bedrock, Google Cloud Vertex AI, and Microsoft Foundry, and through Claude's Pro, Max, Team, and Enterprise subscription plans (not Free).

## What Is Claude Opus 5.5

On September 22, Anthropic released Claude Opus 5.5, its first model since CEO Dario Amodei published an essay calling for the industry to "pace the frontier." The launch leaned heavily on efficiency: Anthropic says the model costs about 40% less on typical workloads and runs over 30% faster than Opus 5, while also posting the strongest alignment scores of any recent Claude model. Anthropic positions it for agentic coding, computer use, and knowledge work, and says its communication has been tuned to be clearer and less jargon-heavy in extended work sessions.

## Claude Opus 5.5 Pricing per Million Tokens

|  | Input | Output | 
|---|---|---|
| **Standard** | $4.00 | $20.00 | 
| **Fast mode** | $8.00 | $40.00 | 
| **Batch** | $2.00 | $10.00 | 

| Cache Type | Price per 1M Tokens | 
|---|---|
| Cache read | $0.20 | 
| Cache write (5-minute) | $5.00 | 
| Cache write (1-hour) | $8.00 | 

**Context window:** 1,000,000 tokens. **Max output:** 128,000 tokens. **Knowledge cutoff:** June 2026.

## What Each Pricing Tier Is Actually For

**Standard** is the default rate and covers most workloads: interactive coding sessions, agentic tasks, and one-off API calls where you're not optimizing for latency or willing to wait for batch processing.

**Fast mode** doubles the price in exchange for lower latency. It exists for workloads where response time matters more than cost, customer-facing chat, live coding assistance, anything where a user is waiting on the response in real time.

**Batch** cuts both input and output pricing in half in exchange for asynchronous processing, no immediate response guarantee. It's built for large-scale, non-interactive jobs: bulk document processing, evaluation runs, overnight data pipelines. It also unlocks the extended 300K-token output limit, useful for large generation or migration jobs that would otherwise hit the standard 128K ceiling.

## Why Always-On Thinking Changes What You're Actually Paying For

Claude Opus 5.5 runs with adaptive thinking always on. It can't be turned off, but the depth of that thinking is controlled through an effort parameter (low, medium, high, and above). Anthropic hasn't published a separate rate for thinking tokens, which means they're billed at the standard output rate, so the effort level you choose directly affects your bill, not just your latency. Anthropic's own cost breakdown gives a concrete example: setting effort to high can add roughly 20,000 thinking tokens to a task, which costs about $0.40 at Opus 5.5's output rate. That's a small number in isolation, but it compounds fast across thousands of daily tasks if effort is left at a high default when it doesn't need to be.

## Claude Opus 5.5 vs Claude Opus 5 Pricing

|  | Claude Opus 5 | Claude Opus 5.5 | Change | 
|---|---|---|---|
| Input | $5.00 | $4.00 | 20% lower | 
| Output | $25.00 | $20.00 | 20% lower | 
| Cache read | $0.50 | $0.20 | 60% lower | 
| Cache write (5-min) | $6.25 | $5.00 | 20% lower | 
| Cache write (1-hr) | $10.00 | $8.00 | 20% lower | 

For the full history on where Opus 5 and its predecessor landed, see Claude Opus 5 Pricing 2026: Complete Cost Guide & Comparison and Claude Opus 4.8 Pricing 2026.

## Where You Can Access Claude Opus 5.5

**Pay-per-token API access** is available through Anthropic's own API, Amazon Bedrock, Google Cloud Vertex AI, and Microsoft Foundry, all at the pricing above.

**Subscription access** through Claude's consumer and business plans works differently, a flat monthly rate instead of metered tokens. Opus 5.5 is available on the Pro plan (roughly $17 to $20/month), the Max plan (from $100/month, with 5x to 20x more usage than Pro), Team plans ($20 to $100 per seat/month), and Enterprise (from $20/seat/month plus usage-based API costs for anything beyond seat allowances). The Free plan does not include Opus 5.5, it's limited to Sonnet and Haiku-tier models.

Which path makes sense depends on usage pattern: predictable, moderate use by a small team often fits a flat seat price better, while variable or high-volume workloads (agents, automation, anything programmatic) usually make more sense metered through the API. We cover this trade-off in more depth in Claude Pricing in 2026 for Individuals, Organizations, and Developers.

## What Claude Opus 5.5 Actually Costs: Real Examples

Anthropic published its own cost breakdown alongside the launch, and a few figures from it are worth pulling out directly:

- A task using 2.8 million input tokens costs $11.20 with no caching, $1.62 at a 90% cache hit rate, and about $0.99 at a 96% hit rate. Caching isn't a minor optimization here, it's more than a 10x cost difference on the same task.
- 60,000 output tokens on a typical task cost about $1.20.
- Setting effort to high adds roughly 20,000 thinking tokens to a task, costing about $0.40.
- A ten-turn retry loop with 100,000 tokens of cached context and 10,000 total output tokens costs roughly the same as a single well-cached turn, cache hit rate matters more than turn count.
- Compacting a 150,000-token conversation costs roughly $0.25.
- At the enterprise level, Anthropic estimates typical usage at about $13 per developer per active day, with under $30 per active day covering 90% of users.

The throughline across all of these: cache hit rate is the single biggest lever on real-world Opus 5.5 spend, more than the headline per-token price.

## What the Benchmarks Say

Anthropic's reported results (at max/xhigh effort) show strong gains across agentic and reasoning tasks:

| Benchmark | Opus 5.5 | 
|---|---|
| Terminal-Bench 4.0 (agentic coding) | 66.4% | 
| AutomationBench (business workflows) | 40.0% | 
| FrontierCode v1.1 | 54.4% | 
| CursorBench 4.0 (IDE coding) | 57.8% | 
| GDPval-AA v2.1 (Elo) | 1846 | 
| Humanity's Last Exam (with tools) | 67.7% | 
| Terminal-Bench-Science | 58.7% | 
| OSWorld 2.0 (desktop use, partial credit) | 81.8% | 

Independent testing from Artificial Analysis reports somewhat lower figures on a couple of these (Terminal-Bench 4.0 at 59.6%, Humanity's Last Exam at 61.4%), which is typical of the gap between vendor-reported and third-party benchmark numbers. Artificial Analysis also notes Opus 5.5 set new highs on Humanity's Last Exam, SciCode, and its AA-Briefcase evaluation.

## What Real-World Workloads Reportedly Cost

Beyond synthetic benchmarks, Anthropic and early reporting cited a few concrete examples, though these are vendor-reported claims rather than independently reproduced results: a 680,000-line code migration completed in under a day, a 200,000-line codebase audit finished in under three hours (down from more than 20 hours on Opus 5), and a HAProxy C-to-Rust translation completed in 9.5 hours.

## How to Lower Your Claude Opus 5.5 Bill Without Losing Output Quality

**Optimize for cache hits, not just prompt length.** As the real examples above show, the difference between a 0% and 96% cache hit rate is roughly a 10x swing in input cost on the same task. Structuring prompts so stable context (system instructions, reference documents) stays cacheable, and only the variable part changes turn to turn, matters more than trimming tokens.

**Match effort to the task.** High effort adds real thinking-token cost on every call. Reserve it for tasks that actually need deep reasoning, and default lower-stakes or well-defined tasks to a lower effort setting.

**Move async work to the Batch API.** Anything that doesn't need an immediate response, evaluation runs, bulk generation, overnight jobs, gets a 50% discount on both input and output simply by moving off the standard endpoint.

**Route lighter tasks to Sonnet or Haiku.** Not every call needs Opus-tier reasoning. Classification, extraction, and routing logic often perform well enough on cheaper Claude models, reserving Opus 5.5 for the tasks that actually need it.

**Watch compaction and retry costs on long agent sessions.** Long-running agentic sessions accumulate context that eventually needs compacting, and retry loops can quietly add up. Both are small individually but scale with agent traffic.

## How to Forecast and Control Claude Opus 5.5 Spend With FinOps

A 20% base price cut and a 60% cut on cache reads sound like straightforward savings, but they only show up on your bill if usage patterns don't quietly offset them. A few things worth putting in place as Opus 5.5 rolls out:

**Allocate spend to teams and products.** Opus 5.5 sits alongside Sonnet and Haiku-tier Claude models, and it's easy for spend to drift toward the most capable (and most expensive) option without anyone deciding that on purpose. Finout's Virtual Tagging can allocate Anthropic spend to teams, products, or customers without code changes. See also Tracking AI Costs Per Customer and Per Feature in 2026.

**Set anomaly alerts on cache hit rate and effort settings.** As the numbers above show, a drop in cache hit rate or a default set to high effort can multiply cost on otherwise identical workloads. Finout's anomaly detection is built to catch these shifts early; more in How to Monitor AI Costs in Real Time.

**Watch for Fast Mode creep.** Fast mode doubles the standard rate. It's a reasonable trade for latency-sensitive work, but if it becomes a default rather than a deliberate choice, it quietly erases the savings from the base price cut.

**Forecast spend as agentic traffic scales.** Long agent sessions, retries, and compaction all add tokens in ways that don't scale linearly with user count. Finout's Financial Plans help model that growth ahead of time. For a broader framework, see FinOps for AI: The Definitive Overview and AI FinOps: 7 Steps to Manage and Optimize AI Costs.

## Bringing Claude Costs Into a Single Source of Truth

Most teams running Opus 5.5 are also running it across multiple access paths, API, Claude Code, subscription seats, and cloud marketplaces, often alongside other AI spend elsewhere in the org. Finout's MegaBill consolidates all of it into a single view, so cost decisions can be made with the full picture rather than one invoice at a time. Related reading: Why Your AI Cost Stack Is Becoming Another Reconciliation Project and Best FinOps Tools for Managing AI Costs in 2026.
