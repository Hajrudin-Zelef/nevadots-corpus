---
id: collect-261121-veille-ia/claude-anthropic/claude-haiku-5-5-s-scorecard-stop-reading-the-rank-start-routing-tasks
title: "Claude Haiku 5.5's scorecard: stop reading the rank, start routing tasks"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["haiku", "sonnet", "work", "tasks", "anthropic", "claude", "rank", "terminal-bench"]
source: http://dev.to/sichi_chen_a4a87b20aa2dbf/claude-haiku-55s-scorecard-stop-reading-the-rank-start-routing-tasks-1m54
source_anchor: ""
source_lines: [1, 64]
section: "Claude_Anthropic"
---
Anthropic released **Claude Haiku 5.5** (model ID `claude-haiku-5-5`) on 7 October 2026 and published a comparison table against Haiku 4.5, GPT-6 Luna and Sonnet 5.5. Most takes I've seen boil it down to "where does it rank". That's the least useful way to read it. The better question: **which work can I move down to the cheap tier now?**

All figures below come from Anthropic's announcement, not from an independent measurement.


| Benchmark | Haiku 5.5 | Haiku 4.5 | GPT-6 Luna | Sonnet 5.5 | 
|---|---|---|---|---|
| GDPval-AA v2.1 (knowledge work) | 1620 | 735 | 1437 | 1840 | 
| AA-Briefcase v1.1 (knowledge work) | 1578 | 614 | 1336 | 1824 | 
| OSWorld 2.1 (computer use) | 72.4% | 15.7% | 48.9% | 83.9% | 
| HLE, no tools | 45.9% | 10.2% | — | 56.9% | 
| HLE, with tools | 57.4% | 18.7% | — | 64.5% | 
| Terminal-Bench 4.0 (agentic coding) | 39.2% | 0.0% | 16.4% | 70.6% | 
| FrontierCode 1.1 (agentic coding) | 46.4% | — | 42.4% | 52.1% | 
| Chartography (visual reasoning) | 46.4% | 6.4% | 29.1% | 61.6% | 

These measure different things. Collapsing them into a single "it's #2" hides the most important signal: the **spread** across tasks.

**1. A generational jump over Haiku 4.5.** OSWorld goes from 15.7% to 72.4%, Terminal-Bench from 0.0% to 39.2%. Both are long action chains: understand, call a tool, handle feedback, recover from a mistake. This is not "nicer wording"; small models are no longer just for short classification jobs.

**2. Ahead of GPT-6 Luna on the items reported side by side.** But it's the vendor's table and the vendor's config. Fair phrasing: *on the items Anthropic published*, Haiku 5.5 scored higher. Don't extrapolate to untested tasks.

**3. Close to Sonnet 5.5 in places, but the tiers are real.** OSWorld 72.4% vs 83.9% is a gap; Terminal-Bench 39.2% vs 70.6% is a canyon. Long-horizon, multi-file, error-recovering agent work still belongs to the bigger model.

Up to 100K tokens: $0.10 / MTok input, $0.50 / MTok output, $0.01 cache read. Over 100K: $0.50 / $2.50 / $0.05. Listed unit price vs Haiku 4.5 drops 90%, but a newer tokenizer uses slightly more tokens, so Anthropic quotes about 75% lower *average* running cost.

The metric that matters in production:


```
cost per successful task = total spend to finish the work / tasks finished successfully
```
If a cheap call needs two retries, or a chunk of requests gets escalated to Sonnet anyway, the saving shrinks. If the output is auto-checkable and retries are cheap, the economics are excellent.

The 100K boundary also nudges you toward a layered design instead of stuffing everything into one prompt:

1. Haiku classifies and filters documents for relevance
2. Compress raw material into structured summaries
3. Send only relevant passages to the larger model
4. Cache repeated context
5. Store conversation state for retrieval instead of resending whole threads

Haiku 5.5 is the first Haiku with an adjustable **effort** setting. One score = one configuration. What you really want is a cost–quality curve. Low effort for classification, format conversion, fixed-field extraction; higher effort for long-document summaries, cross-paragraph synthesis, multi-step browser work. But cranking effort doesn't erase the Terminal-Bench gap.

| Task | Default | 
|---|---|
| Classification, routing, labels | Haiku 5.5 | 
| Field extraction / data cleanup | Haiku 5.5 | 
| Summaries, context compaction | Haiku 5.5 | 
| Short browser flows | Haiku 5.5 | 
| Code explanation, small fixes | Haiku or Sonnet (depends on tests + cost of mistakes) | 
| Multi-file edits | Sonnet 5.5 | 
| Long-running agentic coding | Sonnet or Opus 5.5 | 
| Final review, high-stakes calls | Sonnet or Opus 5.5 | 

Validate on your own tasks: can failures be detected automatically? Is a retry worth it? Does lower latency actually improve the product? Does human review eat the savings?

Haiku 5.5 doesn't beat Sonnet 5.5. It's strong because it can be used **in volume**: a batch of work that used to need a mid-size model now fits in a faster, cheaper execution layer.

The full write-up (with the reading method for official benchmarks) is on Folkbench, and browsing it needs no signup: Folkbench – Haiku 5.5 official scorecard

How are you splitting work between small and large models today?

## Top comments (0)
