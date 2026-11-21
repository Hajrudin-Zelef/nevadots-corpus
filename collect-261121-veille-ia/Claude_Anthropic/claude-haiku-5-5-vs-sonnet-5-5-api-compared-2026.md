---
id: collect-261121-veille-ia/claude-anthropic/claude-haiku-5-5-vs-sonnet-5-5-api-compared-2026
title: "Claude Haiku 5.5 vs Sonnet 5.5 API Compared (2026)"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-09-28"]
keywords: ["haiku", "sonnet", "claude", "output", "versus", "same", "anthropic", "list"]
source: https://aireiter.com/blog/claude-haiku-5-5-vs-sonnet-5-5-api
source_anchor: ""
source_lines: [1, 122]
section: "Claude_Anthropic"
---
Haiku 5.5 costs one-twentieth of Sonnet 5.5 until a prompt crosses 100,000 tokens. Then the gap shrinks to 4x, and agent loops cross that line faster than most teams expect. For short, repeatable API work Haiku 5.5 is enough; for debugging and multi-step coding Sonnet 5.5 still earns its premium.

## The price gap is 20x until it isn't: the 100K cliff

Claude Haiku 5.5 and Claude Sonnet 5.5 share the same headline capacity but not the same bill. Both models offer a 1M-token context window and 128K-token maximum output, with model IDs `claude-haiku-5-5` and `claude-sonnet-5-5`. Sonnet 5.5 launched September 28, 2026 and Haiku 5.5 launched October 7, 2026, according to Anthropic's Haiku 5.5 announcement and Anthropic's Sonnet 5.5 announcement. The Claude platform docs for Sonnet 5.5 list the same 1M context and 128K output limits.

| Spec | Claude Haiku 5.5 | Claude Sonnet 5.5 | 
|---|---|---|
| Model ID | `claude-haiku-5-5` | `claude-sonnet-5-5` | 
| Release | Oct 7, 2026 | Sep 28, 2026 | 
| Context / max output | 1M / 128K | 1M / 128K | 
| Input / output under 100K | $0.10 / $0.50 per 1M | $2 / $10 per 1M | 
| Input / output over 100K prompt | $0.50 / $2.50 per 1M | $2 / $10 per 1M (no tier) | 
| Cache read | $0.01 per 1M | $0.10 per 1M | 
| Default effort | medium | high | 

Prices are Anthropic list rates. Haiku 5.5 applies the higher tier to the entire request once prompt tokens exceed 100,000, including cache reads and writes, per APIKO's summary of Anthropic pricing.

For 1,000 identical 99K-token documents with 1K output each, the batch costs about $10.40 at list (APIKO lists $4.54 for the same batch at its rates), while 1,000 101K-token documents cost about $53.00 at list, five times more for 2,000 extra tokens per doc, versus about $212.00 for Sonnet 5.5 on the same 101K batch. The math is arithmetic from list prices reported by APIKO's Haiku 5.5 vs Sonnet 5.5 test. A modeled batch of 1,000 calls at 10K input plus 1K output each totals $30.00 on Sonnet 5.5 versus $1.50 on Haiku 5.5, a 95% reduction, per Sim's Sonnet 5.5 vs Haiku 5.5 pricing comparison.

Real agent loops hit the threshold without trying. In one r/ClaudeAI thread on Haiku 5.5 token usage, a user found runs crossing 100K around the twentieth model call, with 145 of the next 164 calls billed at the higher tier, because history and tool output accumulate. Suggested fixes from that thread were smaller tickets, targeted file scope, quiet test output, and fresh subagents rather than one long-lived conversation. Short jobs stay cheap on Haiku. Long-context pipelines need Sonnet in the budget.

"Haiku 5.5's cheap price only holds under 100K context. Over 100K input the price jumps 5x; small jobs like translation and summaries save money, long-context work should use Sonnet." — @zookmas1314


Pair with Claude Haiku 5.5 API pricing and Claude Sonnet 5.5 API review and pricing for model-deep dives.

## What the Haiku 5.5 vs Sonnet 5.5 benchmarks separate

Published benchmarks show Sonnet 5.5 ahead in all head-to-head results Anthropic reported, with the gap largest where agents write and execute code. Terminal-Bench 4.0 for agentic coding sits at 39.2% for Haiku 5.5 versus 70.6% for Sonnet 5.5. OSWorld 2.1 computer-use is closer at 72.4% versus 83.9%. Humanity's Last Exam without tools is 45.9% versus 56.9%, and with tools 57.4% versus 64.5%, per the benchmark table compiled in APIKO's comparison.

Effort settings compress or stretch those gaps. Artificial Analysis results cited in the same comparison put Haiku `xhigh` and Sonnet `medium` both at 41 on the Intelligence Index with near-identical Terminal-Bench scores (29.3% vs 29.8%), but Haiku `xhigh` cost $0.12 per task versus $0.48 for Sonnet `medium` while taking 295 seconds versus 122 seconds. No Haiku configuration reached Sonnet's default `high` score of 47 in that dataset. Those cost figures exclude Haiku's over-100K tier, so treat them as a floor for long-context work.

Narrow extraction and classification do not need Sonnet's coding margin. Lead-agent coding, long tool loops, and visual reasoning (Chartography 46.4% vs 61.6%) do. Do not read a single Elo or index number as a universal quality rank. One Reddit test found Haiku-medium matching Sonnet on a mixed Rust and legacy codebase for $0.08 versus $1.68, while Haiku-high made 385 requests with no quality gain in that same workflow, per r/ClaudeCode's controlled Haiku 5.5 coding test.

## Effort matters more than the badge: Haiku low vs Sonnet low

The default effort setting explains many "Haiku is verbose" complaints. Haiku 5.5 defaults to `medium` and Sonnet 5.5 defaults to `high`, and the thinking configuration differs per model. Haiku turns thinking off with `{"type": "disabled"}` while Sonnet uses `{"type": "between_tools"}` at `high` effort and below, with `adaptive` or omitted thinking at `xhigh`/`max`, as documented in the Claude platform docs and the APIKO behavior notes.

In APIKO's 22-task, 528-request Messages-endpoint evaluation on October 10, 2026, Haiku at `low` passed 65 of 66 runs versus 64 to 66 for Sonnet depending on configuration, at roughly one-eighteenth the gateway cost for those short tasks. Turning thinking fully off dropped Haiku to 56 of 66, concentrated in reasoning and strict-format failures such as a CSV sum and a code trace. Haiku at default `medium` passed 66 of 66 but emitted 4.4x more output tokens than Haiku at `low`, mostly thinking tokens.

Practical rules from that data and user reports:

- Start bounded tasks (classification, routing, extraction, short JSON) on Haiku `low` , not Haiku default`medium` .
- Try Haiku `high` or`xhigh` before Sonnet`low` when Haiku-level pricing with better quality is the goal, per Artificial Analysis figures cited in APIKO's comparison: Haiku`max` scored 43 vs Sonnet`low` at 36 at about 60% of the cost.
- Reserve Sonnet `medium` and above for debugging, multi-step implementation, and long tool sequences where Haiku-high's extra latency buys no further quality.
- Set thinking explicitly per model in shared code. A single template that sends `between_tools` to Haiku or`disabled` to Sonnet returns a 400 upstream, per APIKO's gateway test.

"Opus/Sonnet as main orchestrator and reviewer and Haiku as the grunt worker, with a new Haiku instance per feature." — u/ohrajaaa on r/ClaudeAI


## Route by consequence of a silent miss, not by difficulty

The useful routing question is not "is this task hard" but "what happens if Haiku is plausibly wrong and no reviewer catches it." Several cited user tests report the same split: Haiku fails quietly on judgment work, while Sonnet earns its keep where verification is expensive.

| If the task looks like | Lead model | 
|---|---|
| Sentiment classification, ticket routing, permit-to-JSON, log summaries, OCR and annotation, SQL selection, compaction subagents | Haiku 5.5 | 
| Codebase search, one-file edits, single-turn doc processing | Haiku 5.5 | 
| Lead coding agent, real debugging, multi-step refactors, fact-checking, document attribution | Sonnet 5.5 | 

Users report 3x speed and large usage reductions on single-turn, high-volume Haiku patterns, including one case processing several megabytes of documents for a couple of dollars, per r/ClaudeAI's "What is Haiku 5.5 best suited for?".

A six-task subagent comparison found Haiku cheap but significantly less reliable for research and factual judgment, with missed files that "look like a clean result," per r/ClaudeCode's Haiku vs Sonnet vs Opus subagent test. The guardrail from that thread is blunt:

"If the output triggers an action … it either goes to a bigger model or a bigger model checks it." — u/vladgladi on r/ClaudeCode


One production caveat: structured-output compliance is not free on Haiku. APIKO's eval logged a strict-format miss at `low` (correct labels wrapped in a JSON object instead of a bare array), and r/ClaudeCode users report skipped verification and ignored constraints at low effort, per the same controlled coding test.

## A Haiku-first setup that survives production

Setup code must handle model-specific thinking, tool choice, and history, per APIKO's Oct 10 gateway test of documented patterns. Haiku accepts forced `tool_choice` while Sonnet rejects it upstream (use `auto` or structured outputs), and Haiku cannot read Sonnet thinking blocks while Sonnet can read Haiku's if history stays append-only; replaying edited Sonnet blocks can return a 400.

A Haiku-first pattern that users keep independently converging on:

1. Call Haiku 5.5 at `low` effort for the first pass (classify, extract, draft); to disable thinking entirely use`thinking: {"type": "disabled"}` .
2. Accept only allow-listed labels or schema-valid JSON; escalate anything uncertain, invalid, or consequential to Sonnet 5.5 at `medium` .
3. Keep subagents fresh and disposable: one Haiku instance per file or feature, instead of growing one conversation past 100K.
4. Put Sonnet or Opus as reviewer whenever output triggers an action, sends a message, or edits production code.

"Haiku is very good if you know what to do and how to review its quality." — u/Awric on r/ClaudeCode


Anthropic positions Haiku 5.5 as a subagent for high-volume tasks like summaries alongside Sonnet or Opus, per @ClaudeDevs. One shared setup runs Haiku swarms for repo search while Sonnet holds diffs and tests, per @mirku21.

## Haiku 5.5 vs Sonnet 5.5 FAQ

### How much cheaper is Haiku 5.5 than Sonnet 5.5?

At list, 20x under 100K prompt tokens ($0.10/$0.50 vs $2/$10 per 1M) and 4x over 100K ($0.50/$2.50 vs $2/$10). Cache reads are $0.01 vs $0.10 per 1M.

### Do Haiku 5.5 and Sonnet 5.5 have the same context window?

Yes. Both list a 1M-token context window and 128K maximum output, per Anthropic and the platform docs.

### Is Haiku 5.5 good enough for production API use?

For narrow, high-volume, inspectable tasks, yes, usually at `low` effort with schema validation. For lead-agent coding and judgment-heavy research, users report higher miss and false-positive rates than Sonnet 5.5.

### Which model is better for coding agents?

Sonnet 5.5 for the lead agent and debugging (Terminal-Bench 70.6% vs 39.2%). Haiku 5.5 for searcher, typist, and single-file subagent work with a Sonnet or Opus reviewer.

### How do I stay under Haiku's 100K pricing threshold?

Split documents before the threshold, scope subagents to targeted files, silence verbose test output, and start fresh subagents per task. Agentic history crosses 100K in roughly 20 calls.

### Can I move a conversation from Sonnet 5.5 to Haiku 5.5 halfway?

Not cleanly. Haiku cannot read Sonnet thinking blocks, so either stay on Sonnet or restart on Haiku. Moving Haiku history up to Sonnet preserves reasoning if append-only.

## The one trade-off you can't route around

| If your workload looks like | Pick | Why | 
|---|---|---|
| Hundreds of classifications, extractions, summaries, JSON normalizations | Haiku 5.5 at `low` | Near-Sonnet pass rates on short tasks at about 1/18 cost when validated | 
| One big codebase, long tool loops, debugging that must be right | Sonnet 5.5 | 31-point Terminal-Bench lead and fewer silent misses | 
| Documents that straddle 100K | Price both | Haiku's 5x tier erases most of the gap; Sonnet is only 4x more above the line | 
| Anything where a quiet error ships | Haiku draft plus Sonnet review | Cost per verified success beats nominal token price | 

Haiku 5.5 wins on price per attempt. Sonnet may be cheaper once review and reruns dominate token cost. Build the router around that distinction and either model stops being a gamble.
