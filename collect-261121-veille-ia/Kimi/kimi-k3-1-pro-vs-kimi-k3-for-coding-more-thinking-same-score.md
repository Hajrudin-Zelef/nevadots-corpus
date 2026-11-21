---
id: collect-261121-veille-ia/kimi/kimi-k3-1-pro-vs-kimi-k3-for-coding-more-thinking-same-score
title: "Kimi K3.1 Pro vs Kimi K3 for coding: more thinking, same score"
domain: kimi
role: reference
task: article
actors: ["Moonshot AI"]
dates: []
keywords: ["kimi", "more", "long", "session", "score", "sessions", "coding", "short"]
source: https://bito.ai/blog/kimi-k3-1-pro-vs-kimi-k3/
source_anchor: ""
source_lines: [1, 97]
section: "Kimi"
---
Teams already running Kimi K3 want to know one thing about Moonshot’s new Kimi K3.1 Pro: is it worth switching? The change you notice first is how much longer the new model thinks. We ran both back to back through the same agentic coding benchmark, with no tuning for either one, to see whether all that extra thinking pays off.

Short answer: Kimi K3.1 Pro isn’t better than Kimi K3 for agentic coding. Both scored 72% overall. The new model thinks more than twice as much, which helps it diagnose problems, but its fixes and audits are no better, its short-question score dropped 6 points, and a long session costs $4.28 and takes 28 minutes, against $2.46 and 17 minutes for K3.

These are our own runs on real coding work, not numbers the vendor reported.

## Is Kimi K3.1 Pro better than Kimi K3?

No, not on this work. The overall scores are identical, and the 1-point edge K3.1 Pro has on long sessions is inside our noise.

| Model | Overall score | Long agentic sessions | Cost per long session | Time per long session | Short sessions | Cost per question | 
|---|---|---|---|---|---|---|
| NewKimi K3.1 Pro | 72% | 71% | $4.28 | 28.2 min | 74% | $0.13 | 
| PreviousKimi K3 | 72% | 70% | $2.46 | 16.8 min | 80% | $0.31 | 

Scores are percentages of the maximum. The overall score weights long agentic sessions at 75% and short sessions at 25%. A long session is a multi-turn job on a real service: trace the code, work out why something fails, plan a fix, implement it, then audit it. Short sessions are one-shot asks, like explaining how a setting gets resolved or fixing a small bug.

For context, Claude Opus 5.5 scores 87% overall on the same tasks and Claude Sonnet 5.5 75%.

## What changed in Kimi K3.1 Pro?

Mostly, it thinks more. K3.1 Pro wrote about 98,000 output tokens per long session, against 44,000 for K3, and made more than twice as many requests. Some of that paid off. It’s 8 points better at working out why something fails and 4 points better at planning the fix.

| Per long agentic session | Kimi K3 vs Kimi K3.1 Pro | 
|---|---|
| Output tokens |  | 
| Minutes |  | 
| Cost |  | 
| Overall score |  | 


Each row is scaled to its own largest value. K3.1 Pro writes more than twice the output, takes 68% longer and costs 74% more, for the same overall score.

| Turn in a long session | Kimi K3 vs Kimi K3.1 Pro | 
|---|---|
| Trace the code |  | 
| Analyze the failure |  | 
| Plan the fix |  | 
| Implement |  | 
| Audit |  | 


Then the gains disappear. Implementation and the final audit are flat or slightly worse, and K3.1 Pro had the bigger misses. In two sessions its fix for a Redis outage checked a readiness flag that the service’s production Redis client doesn’t have, so in the shipped configuration every protected request would have been rejected. In one session the final audit was the model’s own half-written planning notes instead of an answer.

Its long-session scores also spread more than K3’s from one session to the next.

## Is Kimi K3.1 Pro good at short coding questions?

It’s worse than K3 here. On one-shot asks K3.1 Pro scored 74%, 6 points below K3’s 80%.

Most of that is one task. Asked what happens when nobody sets an admin token, K3.1 Pro said the bypass is disabled, in all four sessions, with accurate file and line citations wrapped around the wrong conclusion. The shipped default is a literal value any client can send, so the bypass is open. K3 scored 81% on that task.

## How much does Kimi K3.1 Pro cost compared with Kimi K3?

More per long session, less per short question. A long agentic session cost $4.28 with K3.1 Pro and $2.46 with K3, about 74% more, because K3.1 Pro writes more than twice the output.

Per short question it’s the other way round: $0.13 against $0.31. Part of that is the prompt cache. 91% of K3.1 Pro’s input came from cache, against 76% for K3.

Kimi K3.1 Pro’s list price is $2 per million input tokens and $10 per million output tokens. All costs here are at standard list prices.

## How fast is Kimi K3.1 Pro?

Slower than K3. A long session took 28.2 minutes against 16.8, which tracks the extra reasoning and the extra requests. For comparison, Claude Sonnet 5.5 finishes the same session in 6.8 minutes.

## Should you upgrade from Kimi K3 to K3.1 Pro?

For agentic coding, we wouldn’t yet. K3.1 Pro gives you better failure analysis and planning, and pays for it with longer, costlier sessions and a lower score on short asks.

If you’re picking an open-weight model for this kind of work, DeepSeek V4.1 Flash 1017 scored 80% overall on the same tasks at $0.14 a long session, ahead of both Kimis. We compare them in Best open-weight model for coding, and we put K3.1 Pro against Claude in Kimi K3.1 Pro vs Claude Opus 5.5 and Sonnet 5.5.

## How we tested

The benchmark is a set of agentic coding tasks taken from real work on our own services. We run every model inside Claude Code with default settings and no extra prompting, with the version pinned. We’ve found models score about the same in Claude Code, Codex and Cursor, so the tool isn’t what we’re measuring.

Every turn is graded against an answer key checked against the code, by a judge scoring five dimensions. The judge is Claude Opus 5.5. We ran 8 long sessions and 16 short ones per model, both Kimis on the same day. Session-to-session noise is about 3 points, so read 1 to 2 point gaps as ties.

## FAQ

**Is Kimi K3.1 Pro better than Kimi K3?**

Not for agentic coding in our tests. Both scored 72% overall, and K3.1 Pro scored 6 points lower on short questions (74% against 80%).

**What is new in Kimi K3.1 Pro?**

In our runs the biggest change was how much it reasons: about 98,000 output tokens per long session against 44,000 for K3. That lifted failure analysis from 62% to 70%.

**How much does Kimi K3.1 Pro cost?**

List price is $2 per million input tokens and $10 per million output. A long agentic session cost $4.28 in our runs, against $2.46 for Kimi K3.

**Is Kimi K3.1 Pro slower than Kimi K3?**

Yes. A long session took 28.2 minutes against 16.8 for K3.

**What is the best Kimi alternative for coding?**

Among open-weight models, DeepSeek V4.1 Flash 1017 scored 80% overall at $0.14 a long session, against 72% for both Kimis.
