---
id: collect-261121-veille-ia/grok-xai/grok-4-7-where-it-lands-on-the-benchmarks
title: "Grok 4.7: Where It Lands on the Benchmarks"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: ["2026-09-21"]
keywords: ["grok", "output", "index", "tokens", "agent", "task", "about", "intelligence"]
source: https://tess.im/en/blog/grok-4-7-where-it-lands
source_anchor: ""
source_lines: [1, 130]
section: "Grok_xAI"
---
Grok 4.7, released on September 21, 2026, does not take the Intelligence Index. It does something more useful for a team that already has a frontier model in the workspace: it gets close on the work agents actually do, at Grok 4.6's price. The invoice still changes, because the model spends far more tokens getting there.

## Why this matters for Tess users

A benchmark write-up is easy to read as a leaderboard. The decision in Tess is narrower. Astra, Fable, Sol, and Grok can already sit in the same workspace, on the same agent, against the same documents. Grok 4.7 earns a place where the task is long, the output is an analysis or a code change, and a person reviews the result. It is a weak default where someone is waiting on a short chat reply, or where the last mile is a deck a client will see untouched.

Artificial Analysis measured the model at xhigh effort. xAI's launch table is a second picture, from different harnesses. Read them together. Do not average them.

## Pricing: the card did not move, the task did

The list price is unchanged from Grok 4.6: $2 per million input tokens and $6 per million output tokens. Cache hits stay at $0.50 per million, a 75% discount. xAI also sells a fast variant at twice the output speed and twice the price.

That sticker is the wrong number to budget with. On the Intelligence Index, Grok 4.7 (xhigh) uses about 81,000 output tokens per task, against about 38,000 for Grok 4.6 (xhigh). At $6 per million, output alone goes from about $0.23 a task to about $0.49. Same contract, roughly twice the output bill, before input, cache, tools, and retries.

Set that next to GPT-6 Astra. Astra's published output rate is $50 per million, and Artificial Analysis measured it at about 27,000 output tokens per intelligence task, or about $1.35 of output. Grok uses about three times the tokens and is still near a third of Astra's output bill, because the per-token price is so much lower. The token gap eats part of the discount. A large part remains.

The model page puts the all-in cost of one Intelligence Index task at $3.74, input and cache included. Running the whole index cost $4,967, on 240 million output tokens. Verbose models get expensive in bulk even when the rate card looks friendly.

Quick takeaway: against Grok 4.6, you pay more per task for a small intelligence gain and a real gain on agent work. Against the $10 / $50 frontier models, Grok 4.7 is still the cheaper way to get close, and it is not the way to get past them.


## Highlights from the Coding Agent Index

### ➤ Close enough that price starts to matter

With Grok Build, Grok 4.7 (xhigh) scores 56 on the Coding Agent Index, up from 47 for Grok 4.6 (xhigh). It ranks fourth among models in their own harnesses. Claude Fable 5.1 and GPT-6 Astra lead at 62. Claude Opus 5 is at 60. GPT-5.6 Sol (max), in Codex, is at 55.

Six points behind the leaders is a real gap. It is not a different sport. For a team running many agent tasks a day, those six points have to be worth Fable's $10 / $50 rate against Grok's $2 / $6. On volume, they often are not. The index is the reason to pilot Grok. It is not a reason to retire the model you already trust on your hardest repos.

### ➤ The average hides the job it is bad at

The nine-point jump is spread across three tests, and they do not tell the same story. In Grok Build, DeepSWE v1.1 rises from 65% to 73% and leads the published chart, just ahead of GPT-5.6 Sol and Muse Spark 1.3 at 72%. SWE-Atlas-QnA rises from 58% to 63%. Terminal-Bench 4.0 rises from 18% to 33%.

That last number is the one to sit with. 33% is a large improvement on Grok 4.6 and a long way from Fable 5.1 (58%), GPT-6 Astra (56%), and Opus 5 (55%) on the same chart. A coding agent that mostly implements against a repo can use Grok 4.7. A coding agent that lives in the terminal, reads command output, and has to finish the session alone still has a better tool at the top of that chart.


*Source: Artificial Analysis — Intelligence Index and Coding Agent Index*


*Source: Artificial Analysis — DeepSWE, Terminal-Bench, and SWE-Atlas-QnA in Grok Build*

These scores use Grok Build. The Intelligence Index below uses one shared harness for every model. A gain in one does not transfer to the other.

## Highlights from the Intelligence Index

### ➤ The headline score is the least interesting one

Grok 4.7 (xhigh) scores 46. Grok 4.6 (xhigh) scores 44. On the same chart, GPT-5.6 Sol (max) is at 47, Muse Spark 1.3 (max) at 48, and both Fable 5.1 and GPT-6 Astra at 53. The model page ranks it 16th of 202 models in its class, well above a median of 24, and well short of the models a team would call frontier.

Two points is a refresh, not a migration. If the only thing you needed was "a smarter general model," stay where you are. The case for Grok 4.7 is in the slices underneath that average.

### ➤ Knowledge work is where "near the frontier" is actually true

On AA-Briefcase, long professional work with linked tasks and a large file set, Grok 4.7 scores 1,657 Elo. That is 111 above Grok 4.6 (high) at 1,546, just behind Fable 5.1 (1,678) and Opus 5 (1,673), and ahead of GPT-6 Astra (1,569). On GDPval-AA, documents, spreadsheets, and slides drawn from real occupations, it scores 1,695, up 90 from 1,605. Fable still leads, at 1,735.

The quality split matters more than the Elo. Analytical quality jumps from 1,690 to 1,994. Presentation quality slips from 1,519 to 1,499. The model got better at the thinking and no better at the artifact. Use it to pressure-test an analysis, reconcile a long brief, or draft the substance of a memo. Put a review pass, or another model, on anything a client will judge by how it looks.

### ➤ You pay in tokens, not in a much longer wait

About 81,000 output tokens per task breaks down as roughly 22,000 answer tokens and 59,000 reasoning tokens. Most of the new spend is the model thinking, not the model talking. Artificial Analysis puts that at 125% more output than Grok 4.6 (high), at 36,000, and 196% more than Astra, at 27,000.

The surprise is the clock. Decode time is about 7.1 minutes per intelligence task, next to 6.6 minutes for both Astra (max) and Grok 4.6 (high). That figure excludes time to first token. On this test, triple the tokens does not mean triple the wait, because answer throughput on long prompts is about 188 tokens per second.

Interactive use is a different test. The model page measures 38.8 output tokens per second and ranks Grok 4.7 154th of 202 on speed, with a time to first token of 0.85 seconds. An agent that goes away and comes back with an analysis will not feel the 38.8. A person watching a chat bubble will.

### ➤ Factuality improved by a little

In AA-Omniscience, the hallucination rate falls from 34% for Grok 4.6 (high) to 29%. Accuracy stays flat, 47% against 48%. The index moves from 30 to 32. That is a cleaner model, not a careful one. It is not the kind of drop that justifies switching a fact-sensitive workflow on its own. Review still belongs in the loop.

### ➤ The rest of the index is a wash

In the shared harness, against Grok 4.6 (high), Terminal-Bench 4.0 rises 4.5 points and GDP.pdf rises 3.0. AA-LCR falls 3.7 and AutomationBench-AA falls 1.1. Hold that Terminal-Bench figure next to the 18% to 33% jump inside Grok Build. Same name, different test, different story. Quoting either number alone will mislead a rollout plan.


*Source: Artificial Analysis — AA-Briefcase and GDPval-AA v2*


*Source: Artificial Analysis — output tokens per Intelligence Index task*


*Source: Artificial Analysis — time per Intelligence Index task*


*Source: Artificial Analysis — AA-Omniscience*


*Source: Artificial Analysis — Intelligence Index evaluation breakdown*

## What xAI reports, and how to weigh it

xAI's launch post is the company's case: longer work, more self-checking, the same price and speed as Grok 4.6. The table below compares Grok 4.7 (xHigh) with Grok 4.6 (High), GPT-5.6 Sol (Max), and Fable 5.1 (Max). Treat it as a hypothesis to test, not as a second Intelligence Index.

The row that lines up with the independent coding story is CursorBench. xAI puts Grok 4.7 (xHigh) at 46.3% and about $6.01 per task, against 41.7% and $8.23 for Sol (Max), and 51.8% and $17.28 for Fable (Max). Lower effort is a real dial, not a footnote: High is 43.9% at $4.69, Medium is 41.6% at $3.49, Low is 33.1% at $1.58. If your agent does not need the top setting, the bill falls faster than the score.

The rows to be careful with are the specialist ones. EEBench at 64% leads the four models xAI chose to show, and the Harvey legal benchmark at 19.6% is several times Sol (2.5%) and Fable (6.7%). Those are reasons to run a domain pilot on your own matters. They are not a certification. 19.6% is still a low absolute pass rate. HealthBench Professional, at 56.7%, goes the other way and trails both Sol and Fable. A model with a sharp legal spike and a weaker clinical score is a specialist candidate, not a general upgrade.

One naming trap belongs in the open. Terminal-Bench 4.0 is 38.0% in xAI's table, 33% in Artificial Analysis's Grok Build run, and only 4.5 points higher than Grok 4.6 in the shared harness. Anyone comparing "Terminal-Bench" across these pages without the harness in the sentence is comparing different exams.

| Benchmark | Grok 4.7 | Grok 4.6 | GPT-5.6 Sol | Fable 5.1 | 
|---|---|---|---|---|
| Input / output, $ per million tokens | $2 / $6 | $2 / $6 | $4 / $20 | $10 / $50 | 
| CursorBench 4.0 | 46.3% | 40.4% | 41.7% | 51.8% | 
| DeepSWE v1.1 | 71.0%* | 65.2% | 72.7% | 70.0% | 
| EEBench | 64.0% | 53.0% | 39.4% | 56.4% | 
| AA Briefcase v1.1 | 1,657 | 1,546 | 1,487 | 1,678 | 
| Terminal-Bench 4.0 | 38.0% | 20.3% | 37.3% | 57.9% | 
| Harvey Legal Agent Benchmark | 19.6% | 15.8% | 2.5% | 6.7% | 
| HealthBench Professional | 56.7% | 48.5% | 60.5% | 62.1% | 

* xAI labels the Grok 4.7 DeepSWE result as high effort.

The context window is 500,000 tokens, with text and image input and text output. xAI also says the model leads its own safety tests, including 62.4% on LatchBio's biosafety benchmark and a 3.3% pass-through of risky prompts on HackerBench v0.3. Those are xAI's measurements of xAI's tests. For a sensitive deployment they are a starting point for review, alongside your own access policy.

## In practice

- For repo-heavy coding agents: pilot Grok 4.7 at xhigh. The Coding Agent Index and DeepSWE are where the price looks rational. Keep a frontier model on long terminal sessions.

- For analysis, long briefs, and multi-step knowledge work: this is the strongest independent case. Analytical quality moved into the frontier band. Do not ship the slides or the formatted deliverable without a review pass.

- For a general swap off Astra, Fable, or Sol: the Intelligence Index does not support it. Two points, with a much larger token count, is a sidegrade.

- For the budget: expect about twice the output spend of Grok 4.6 on hard tasks, and still a lower output bill than Astra on the same index. Drop effort to high or medium when the task allows. The CursorBench curve shows the score falling slower than the cost.

- For chat that has to feel instant: the 38.8 tokens per second on the model page is the relevant clock. Use the fast variant only if that latency is worth twice the price.

- For facts, legal, or clinical work: hallucinations dipped, they did not collapse. Harvey and EEBench are worth a private eval on your own files. They are not a reason to skip review.

The comparison that settles it is a slice of your own work: one repo, one long brief, one deck, run on Grok 4.7 and on the model you use today. Tess is built for that test. Several models, one workspace, the same task.

Original charts and methodology: Artificial Analysis — “Benchmarking Grok 4.7”, published September 21, 2026. Operating profile: Artificial Analysis — Grok 4.7 (xhigh). Company-reported scores: xAI — “Introducing Grok 4.7”. Output-cost figures in this article use those published token counts and token prices, and cover output tokens only.
