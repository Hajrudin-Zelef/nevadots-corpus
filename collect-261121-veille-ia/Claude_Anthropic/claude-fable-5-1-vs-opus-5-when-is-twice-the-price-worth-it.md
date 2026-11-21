---
id: collect-261121-veille-ia/claude-anthropic/claude-fable-5-1-vs-opus-5-when-is-twice-the-price-worth-it
title: "Claude Fable 5.1 vs Opus 5: When Is Twice the Price Worth It?"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-09-01"]
keywords: ["fable", "opus", "claude", "tokens", "anthropics", "cache", "model", "agentic"]
source: https://apidog.com/blog/claude-fable-5-1-vs-opus-5/
source_anchor: ""
source_lines: [1, 118]
section: "Claude_Anthropic"
---
Claude Fable 5.1 costs $10 and $50 per million tokens. Claude Opus 5 costs $5 and $25. Anthropic’s own docs tell you to start with Opus 5 and move to Fable 5.1 only “when your evals on Claude Opus 5 at higher effort still fall short.” That is a testable rule, and this comparison is built around it: where Anthropic’s numbers show Fable 5.1 pulling clear of Opus 5, where they show a few points that prompt tuning could close, and where the pricing math gets interesting because Fable 5.1’s cache reads are half of Opus 5’s.

The sources are Anthropic’s launch post, the Fable 5.1 model page, and the pricing page. For each model’s own overview, see what Claude Fable 5.1 is and what Claude Opus 5 is.

## Side by side

|  | Claude Fable 5.1 | Claude Opus 5 | 
|---|---|---|
| Model ID | `claude-fable-5-1` | `claude-opus-5` | 
| Released | September 1, 2026 | July 24, 2026 | 
| Positioning | Demanding reasoning and long-horizon agentic work | Complex agentic coding and enterprise work; the recommended default | 
| Input / output | $10 / $50 | $5 / $25 | 
| Cache read | $0.25 | $0.50 | 
| Cache write (5m / 1h) | $12.50 / $20 | $6.25 / $10 | 
| Batch | $5 / $25 | $2.50 / $12.50 | 
| Fast mode | No | Yes ($10 / $50) | 
| Priority Tier | No | No | 
| Context / max output | 1M / 128K | 1M / 128K (300K on Batch with beta) | 
| Knowledge cutoff | June 2026 | May 2026 | 
| Comparative latency | Slower | Moderate | 
| Thinking | Always on; `disabled` returns 400 | On by default; `disabled` allowed at`high` or below | 
| Forced `tool_choice` | 400 | Accepted | 
| Safety classifiers | cyber, bio, frontier_llm, reasoning_extraction, general_harms | cyber only | 
| Zero data retention | Not available unless authorized | Available | 
| Per-message effort (beta) | Yes | Yes | 
| History-editing check | Yes | No | 

## What the benchmarks say

All numbers are Anthropic’s, run by Anthropic, with no independent reproduction yet.

| Benchmark | Fable 5.1 | Opus 5 | Gap | 
|---|---|---|---|
| Terminal-Bench-Science 0.1 | 52.6% | 29.0% | +23.6 | 
| AutomationBench | 31.4% | 26.9% | +4.5 | 
| Humanity’s Last Exam (no tools) | 60.9% | 56.6% | +4.3 | 
| Terminal-Bench 4.0 | 55.8% | 52.3% | +3.5 | 
| CursorBench 3.2.0 | 73.4% | 70.0% | +3.4 | 
| OSWorld 2.0 (partial) | 77.9% | 75.4% | +2.5 | 
| OSWorld 2.0 (strict) | 41.7% | 39.6% | +2.1 | 
| Humanity’s Last Exam (with tools) | 65.0% | 63.6% | +1.4 | 
| GDPval-AA v2 | 1853 | 1824 | +29 Elo | 

Read that table as two stories. On Terminal-Bench-Science, Fable 5.1 nearly doubles Opus 5, and that is a long-horizon, tool-heavy research benchmark where the model runs experiments in a terminal for a long time. On everything else, the gap is between one and five points. A five-point gap on AutomationBench is meaningful. A 1.4-point gap on Humanity’s Last Exam with tools, or 29 Elo on GDPval, is inside the range where your prompt and your effort setting move the result more than the model choice does.

The other data point worth holding onto: Opus 5 launched in July at 52.3% on Terminal-Bench 4.0, ahead of Fable 5’s 42.0%. Fable 5.1 retook the lead at 55.8%, but by 3.5 points, not 13.8. The Fable tier’s advantage over the Opus tier on agentic coding is real and narrower than it was in June. The Opus 5 vs Fable 5 comparison captured the earlier state of that race.

## The pricing math is not simply 2x

Uncached, Fable 5.1 is exactly twice Opus 5 on every line. But cache reads invert: $0.25 on Fable 5.1 versus $0.50 on Opus 5. So the effective multiplier depends on how much of your input is cached prefix.

Take an agentic turn with 150,000 cached tokens, 1,000 uncached input tokens, and 800 output tokens. Opus 5: `5(0.001) + 0.5(0.15) + 25(0.0008)` = $0.005 + $0.075 + $0.02 = $0.10. Fable 5.1: `10(0.001) + 0.25(0.15) + 50(0.0008)` = $0.01 + $0.0375 + $0.04 = $0.0875. On that turn, Fable 5.1 is cheaper.

Now take a code review with 30,000 uncached input tokens and 4,000 output tokens. Opus 5: $0.15 + $0.10 = $0.25. Fable 5.1: $0.30 + $0.20 = $0.50. Exactly double.

The crossover: Fable 5.1’s per-turn cost falls below Opus 5’s when cached input tokens exceed roughly 20 times the sum of uncached input tokens and 5 times output tokens. For prefix-heavy agent loops that condition is common. For anything output-heavy it never happens. Cache writes are also double on Fable 5.1, so a session that resets its cache often loses the advantage. The Fable 5.1 pricing breakdown and the Opus 5 pricing breakdown have the full tables.

## Where Opus 5 wins outright

**Anything you can run with thinking off.** Opus 5 accepts `thinking: {"type": "disabled"}` at `high` effort or below. Fable 5.1 never does. For classification, extraction, and short-answer routes, Opus 5 with thinking off (or at `low` effort) is both cheaper and faster.

**Fast mode.** Opus 5 has a research-preview fast mode at up to 2.5x output speed for $10 and $50. Fable 5.1 has no equivalent. If time-to-answer matters more than the last few benchmark points, that is decisive.

**Latency generally.** Anthropic lists Fable 5.1 as “slower” and Opus 5 as “moderate.” Single Fable 5.1 turns on hard tasks at high effort can run many minutes.

**Zero data retention.** Opus 5 is available under ZDR. Fable 5.1 is a Covered Model and returns a 400 unless Anthropic expressly authorizes ZDR access.

**Fewer refusals.** Opus 5 runs cybersecurity-only classifiers. Fable 5.1 runs the full Fable set: cyber, bio, frontier LLM development, reasoning extraction, and general harms. A benign life-sciences or ML workload that never trips Opus 5 can trip Fable 5.1, and when it does the fallback target is Opus 5 anyway.

**Forced tool use.** Opus 5 still accepts `tool_choice` `any` and `tool`. Fable 5.1 returns a 400. If your integration leans on forced calls, Opus 5 saves you the rewrite.

**No history-editing check.** Opus 5 does not care if you edit earlier turns. Fable 5.1 invalidates later thinking blocks when you do. A harness that compacts client-side or injects per-turn reminders works on Opus 5 today and needs an audit before Fable 5.1.

## Where Fable 5.1 wins outright

**Long-horizon research and terminal agents.** The 52.6% versus 29.0% Terminal-Bench-Science result is the single largest gap in Anthropic’s table. If your agents run experiments, chase multistep web research, or operate for hours, this is the workload the model was built for.

**Business workflow automation.** 31.4% versus 26.9% on AutomationBench is a real gap on a benchmark that models end-to-end tasks across applications.

**Cached-prefix-heavy agents.** Covered above: the $0.25 cache read makes Fable 5.1 cheaper per turn on prefix-dominated loops.

**Knowledge cutoff.** June 2026 versus May 2026. One month, but it is the freshest of any Claude model.

**Vision on dense documents.** Anthropic lists improved reading of charts, filings, and tables nested in PDFs, especially with crop-and-zoom tools, as a Fable 5.1 gain area. Opus 5 is not compared on a published vision benchmark, so treat this as a claim to test.

**When Opus 5 at `xhigh` fails.** This is Anthropic’s rule, and it is the right one. If a task fails your eval on Opus 5 at `xhigh`, try Fable 5.1 at `high` before you try anything else.

## Effort changes the comparison

Both models expose five effort levels, and Anthropic makes a specific claim about Fable 5.1 at the low end: at `low` effort it is “often competitive with Claude Opus and Claude Sonnet models on cost per task while scoring higher.” That reframes the choice. Instead of Opus 5 at `high` versus Fable 5.1 at `high`, the fair comparison for routine work might be Opus 5 at `high` versus Fable 5.1 at `low` or `medium`. The output-token count at lower effort is smaller, which offsets the 2x per-token price.

The reverse also holds. Fable 5.1’s gains over Fable 5 are largest at `xhigh` and `max`, which means the capability ceiling above Opus 5 is highest at the effort levels that also cost the most time. Sweep both models across effort on your own evals before concluding anything from the launch tables. The Opus 5 effort guide covers the five levels; the semantics are the same on Fable 5.1.

## A decision framework

1. **Start on Opus 5 at `high`.** Anthropic’s default recommendation, half the price, fewer classifiers, ZDR available.
2. **Raise Opus 5 to `xhigh` on the tasks that fail.** Cheaper than switching models.
3. **Move the still-failing tasks to Fable 5.1 at `high`.** Per-route, not globally. Keep Opus 5 on everything that passes.
4. **Check the cache profile of what you moved.** If it is a prefix-heavy loop, Fable 5.1 may end up cheaper per turn than Opus 5 was.
5. **Audit the harness before step 3.** Forced`tool_choice` and history edits both break on Fable 5.1. The migration guide has the checklist.

## Running the comparison in Apidog

Build one collection with your production prompts and `model` as an environment variable. Run it against `claude-opus-5` at `high`, again at `xhigh`, then against `claude-fable-5-1` at `high` and `low`. For each run, record the answer, `usage.output_tokens`, and `usage.cache_read_input_tokens` on a repeat send. Multiply by each model’s rates in a post-response script and you have cost per task per model per effort in one table. Apidog keeps every run as history, so the comparison survives the next model launch. Download Apidog to set it up.

## FAQ

**Is Claude Fable 5.1 better than Opus 5?** On every benchmark Anthropic published, yes, by margins from 1.4 points to 23.6 points. The large gap is on Terminal-Bench-Science; most others are under five points. These are vendor-run results.

**Is Fable 5.1 worth twice the price of Opus 5?** For long-horizon research and terminal agents, business automation, and prefix-heavy agent loops (where cheaper cache reads can make it cost less per turn), often yes. For output-heavy, latency-sensitive, or classification work, no. Anthropic’s own rule: move up only when Opus 5 at higher effort fails your evals.

**Which is faster, Fable 5.1 or Opus 5?** Opus 5. Anthropic lists it as “moderate” latency versus “slower” for Fable 5.1, and Opus 5 has a fast mode at up to 2.5x output speed. Fable 5.1 has none.

**Does Opus 5 have the same safety classifiers as Fable 5.1?** No. Opus 5 runs cyber-only classifiers. Fable 5.1 adds bio, frontier LLM, reasoning extraction, and general harms. When Fable 5.1 refuses, Opus 5 is one of the two permitted fallback targets.

**Can I use Fable 5.1 under zero data retention like Opus 5?** No. Fable 5.1 requires 30-day retention and is a Covered Model. Opus 5 is available under ZDR.

**Can a conversation move between Opus 5 and Fable 5.1?** Up, yes: Fable 5.1 reads Opus 5’s thinking blocks. Down, the API drops Fable 5.1’s thinking blocks before Opus 5 sees them, unbilled, and Opus 5 re-plans that turn.
