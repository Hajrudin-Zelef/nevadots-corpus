---
id: collect-261121-veille-ia/claude-anthropic/claude-haiku-5-5-90-cheaper-than-haiku-4-5-pricing-explainx-ai-blog
title: "Claude Haiku 5.5: 90% Cheaper Than Haiku 4.5 (Pricing) | explainx.ai Blog"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-10-07"]
keywords: ["haiku", "anthropic", "claude", "tokens", "model", "browser", "lower", "effort"]
source: https://explainx.ai/blog/claude-haiku-5-5-launch-pricing-benchmarks-sonnet-cache-cut-2026
source_anchor: ""
source_lines: [1, 164]
section: "Claude_Anthropic"
---
Anthropic released **Claude Haiku 5.5** on October 7, 2026, and the headline is price: **$0.10 per million input tokens and $0.50 per million output tokens** for prompts up to 100,000 tokens, a 90% cut from Haiku 4.5. Anthropic calls it its "cheapest, fastest, and most capable small model" yet, aimed at summaries, compaction, classification, subagents, live customer support, and browser use. The model ID is `claude-haiku-5-5`, and it is live on the Claude Platform, AWS, Google Cloud, and Microsoft Azure.

The same announcement bundles two other value changes: **Sonnet 5.5 cache reads are now 50% cheaper**, and **Max and Team subscribers get a monthly API credit**. This post walks through the numbers, where Haiku 5.5 fits next to the models we already covered, and what to check before you migrate. Everything below comes from Anthropic's launch post unless noted.

Claude model-tier eval comparison chart from the Claude 5.5 family, the tiers Haiku 5.5 now sits beneath

## TL;DR: the questions people will ask

| Question | Answer | 
|---|---|
| What does it cost? | $0.10 in / $0.50 out per million tokens up to 100k prompt; $0.50 / $2.50 above 100k | 
| How does that compare to Haiku 4.5? | 90% lower for short prompts, 50% lower for long ones; about 75% lower on average per Anthropic | 
| Is it a Sonnet replacement? | No. Sonnet 5.5 and Opus 5.5 stay better for complex agentic coding | 
| What is it best at? | Summaries, compaction, DB queries, classification, subagents, browser use, support chat | 
| New in this tier? | First Haiku with adjustable effort levels | 
| Where is it available? | Claude Platform, Amazon Web Services, Google Cloud, Microsoft Azure | 
| Any catch? | New tokenizer uses slightly more tokens per task; cyber safeguards are stricter than Haiku 4.5 | 

## What Anthropic actually shipped

Haiku has always been the small, cheap tier of the family. Haiku 5.5 changes its role: Anthropic positions it as the model you pair with a bigger one. In its words, it "pairs well with Opus 5.5 and Sonnet 5.5 as a subagent on coding work," and it is the fastest model Anthropic has released at standard speed. A footnote adds that it runs less quickly than the Opus models in Fast Mode, so "fastest" refers to default speed.

Three product changes stand out:

1. **Pricing.** Anthropic says it costs around 75% less to run on average than Haiku 4.5. The footnote explains the math: 90% lower for requests up to 100,000 tokens, 50% lower above that, weighted by the fact that about 90% of Haiku 4.5 requests fell under the 100k line, and adjusted for token usage.
2. **Effort setting.** It is the first Haiku-class model with adjustable effort, matching the control we described when covering Claude Code subagent effort levels.
3. **Computer and browser use.** The Python and TypeScript SDKs add computer use and browser use in beta, and Anthropic says Haiku 5.5 is "especially well-suited" to them. See the browser use SDK docs.

## Update (October 8, 2026): Vals AI puts Haiku 5.5 third on Vibe Code Bench, with a cost caveat

Independent evaluator Vals AI reported on X that Haiku 5.5 scored 90.4% on Vibe Code Bench, up 79% and 89 places from Haiku 4.5, which makes it the third-ranked model on that leaderboard. The Vals AI Vibe Code Bench leaderboard lists it at third with 90.44% and a cost of about $6.07 per run (OpenHands harness), against 11.39% for Haiku 4.5 (Thinking).

The same thread adds a caution that matters for routing decisions. Because Haiku 5.5 uses far more tokens than its predecessor, Vals AI says it costs more per test than Haiku 4.5 on every benchmark both models were run on. In its legal research task, the model reportedly averaged 59 steps and wrote roughly 15 times as much as Haiku 4.5, with about 80% of that output being reasoning. Per-token price (the $0.10 input and $0.50 output rates below, for requests under 100k tokens) and per-task cost are different numbers, and the effort setting is the lever to test before you move a workload over.

Practical takeaway: Haiku 5.5 is now a credible small coding model on an independent benchmark, not only on Anthropic's own table. Measure cost per completed task on your own workload, at a lower effort setting first, before assuming the cheaper sticker price carries through.

## Pricing table: Haiku 5.5 vs Haiku 4.5 vs Sonnet 5.5

Prices per million tokens, from Anthropic's table. Haiku 5.5 has two columns: prompts up to 100k tokens, then over 100k.

| Token type | Haiku 5.5 (up to 100k / over 100k) | Haiku 4.5 | Sonnet 5.5 | 
|---|---|---|---|
| Input | $0.10 / $0.50 | $1.00 | $2.00 | 
| Output | $0.50 / $2.50 | $5.00 | $10.00 | 
| Cache reads | $0.01 / $0.05 | $0.10 | $0.10 (was $0.20) | 
| Cache writes | $0.125 / $0.625 | $1.25 | $2.50 | 

A quick worked example. Suppose a pipeline pushes 10 million input tokens and 2 million output tokens a day through short prompts:

- Haiku 5.5: 10 x $0.10 + 2 x $0.50 = **$2.00**
- Haiku 4.5: 10 x $1.00 + 2 x $5.00 = **$20.00**
- Sonnet 5.5: 10 x $2.00 + 2 x $10.00 = **$40.00**

That is a 20x gap between Haiku 5.5 and Sonnet 5.5 on list price, before accounting for the tokenizer. Anthropic notes that Haiku 5.5 has an updated tokenizer, similar to Sonnet 5.5 and Opus 5.5, so it uses slightly more tokens per task than Haiku 4.5. Re-measure with your own traffic rather than assuming the headline 90%.

## Benchmarks: a big jump, with honest limits

Anthropic's comparison table sets Haiku 5.5 against Haiku 4.5, OpenAI's GPT-6 Luna, and Sonnet 5.5 for reference.

| Benchmark | Haiku 5.5 | Haiku 4.5 | GPT-6 Luna | Sonnet 5.5 | 
|---|---|---|---|---|
| GDPval-AA v2.1 | 1620 | 735 | 1437 | 1840 | 
| AA-Briefcase v1.1 | 1578 | 614 | 1336 | 1824 | 
| OSWorld 2.1 (offline subset) | 72.4% | 15.7% | 48.9% | 83.9% | 
| Humanity's Last Exam, no tools | 45.9% | 10.2% | not reported | 56.9% | 
| Humanity's Last Exam, with tools | 57.4% | 18.7% | not reported | 64.5% | 
| Terminal-Bench 4.0 | 39.2% | 0.0% | 16.4% | 70.6% | 
| FrontierCode 1.1 (Main) | 46.4% | not reported | 42.4% | 52.1% (xhigh) | 
| Chartography, no tools | 46.4% | 6.4% | 29.1% | 61.6% | 

Three readings of that table:

- **The generational jump is huge.** Computer use on OSWorld goes from 15.7% to 72.4%, and Terminal-Bench from 0.0% to 39.2%. Haiku 4.5 was not really an agent model; Haiku 5.5 is.
- **It beats the rival small model in Anthropic's own table.** Against GPT-6 Luna, Haiku 5.5 leads on every row where both are reported. Treat that as a vendor-run comparison; Anthropic points to the Haiku 5.5 system card for how the evaluations were run.
- **The ceiling is Sonnet.** On Terminal-Bench 4.0, Haiku 5.5 trails Sonnet 5.5 by more than 30 points. Anthropic is explicit: Sonnet 5.5 and Opus 5.5 "remain better choices for complex agentic coding tasks," while Haiku 5.5 is "best suited to more narrowly scoped tasks that might otherwise have been cost-prohibitive."

For deeper context on the bigger models, see our Opus 5.5 vs Sonnet 5.5 comparison and the Opus 5.5 launch benchmarks.

## What customers say (and what to discount)

Anthropic's launch page quotes early testers. These are vendor-curated, so read them as use-case hints, not independent results.

- **Asana** reported over a 30% latency reduction on task completions and up to 2.5x faster inference per agent turn versus its current model, in evals for bug triage and project setup.
- **HubSpot** said Haiku 5.5 hit 92.8% averaged over three runs on its simulated-portal CRM suite, the best it had seen from a smaller model.
- **AlphaSense** runs about 8 million "Ask in Document" calls a week and measured 0.84 versus 0.76 for Haiku 4.5 on 400 queries.
- **Box** saw a score 11 points above Haiku 4.5 at about half the latency.
- **Cognition** said that in Devin Fusion, Haiku 5.5 as the sidekick with Opus 5.5 as lead holds a FrontierCode score of 66.2 while cutting cost and latency, and that it can be tried in the Devin CLI today.

The pattern across quotes is consistent with the pricing story: high-volume, bounded tasks where latency and cost dominate.

## The other two announcements

### Sonnet 5.5 cache reads cut 50%

Cache reads on Sonnet 5.5 fall from $0.20 to **$0.10 per million tokens**, effective immediately. Because cache reads are a large share of tokens in long agent loops, Anthropic estimates this lowers Sonnet 5.5 cost on most agentic tasks by around 20%. If you run Claude Code or a custom harness with prompt caching, this lands without any code change. Our earlier analysis of Opus 5.5 task cost in Claude Code explains why cache reads dominate agent bills.

### Monthly API credits for Max and Team

This week Anthropic is rolling out a monthly Claude Platform credit for subscribers:

| Plan | Monthly API credit | 
|---|---|
| Max 5x | $100 | 
| Max 20x | $200 | 
| Team | up to $500, pooled across users | 

The credits work on any model and are meant for building tools, apps, and agents that call the API. Anthropic points to a Help Center article for details. If you are already using a subscription for Claude Code, this is effectively free prototyping budget for the API side. Check the terms for expiry and eligibility before counting on it.

**Update, October 8, 2026:** Anthropic's documentation is now live. Team credits are $20 per Standard seat and $100 per Premium seat, pooled and capped at $500; credits expire each billing cycle; and they do not cover interactive Claude Code. Full claim steps and limits: Claude Max and Team API credits guide.

## Where Haiku 5.5 fits: a practical routing guide

A reasonable default for teams already on Claude 5.5:

| Workload | Suggested model | Why | 
|---|---|---|
| Complex multi-file coding, long refactors | Opus 5.5 or Sonnet 5.5 | Terminal-Bench gap is large | 
| Planner / lead agent | Opus 5.5 | Quality of decomposition matters most | 
| Search, file reading, summarizing logs as subagents | Haiku 5.5 | Cheap, fast, accurate enough | 
| Context compaction between turns | Haiku 5.5 | Anthropic names compaction explicitly | 
| Classification, routing, extraction at scale | Haiku 5.5 | 90% cheaper on short prompts | 
| Live support chat, voice-adjacent flows | Haiku 5.5 | Latency | 
| Browser or computer-use agents on a budget | Haiku 5.5 | OSWorld 72.4% on the offline subset | 

A minimal call looks like this:

```
import anthropic
client = anthropic.Anthropic()
resp = client.messages.create(
    model="claude-haiku-5-5",
    max_tokens=1024,
    messages=[{"role": "user", "content": "Summarize this incident log in five bullets: ..."}],
)
print(resp.content)
```
Before cutting over, read the official migration guide. Sonnet 5.5 introduced breaking API changes around thinking and tool choice, which we covered in the Sonnet 5.5 building guide, and it is worth confirming which apply to Haiku 5.5 rather than assuming. For orientation on Anthropic's developer resources, see the claude.dev developer hub.

## Safety and safeguards

Anthropic says Haiku 5.5 shows major improvements across almost all alignment evaluations versus Haiku 4.5, with far fewer instances of misaligned behavior and lower willingness to cooperate with misuse. Details are in the system card.

On safeguards:

- **Cyber:** more restrictive than Haiku 4.5 but somewhat less restrictive than on other recent models. It permits a wider range of defensive tasks than Sonnet 5.5's safeguards, but still blocks penetration testing and techniques more likely used by attackers.
- **Biology:** the same safeguards as Sonnet 5, Sonnet 5.5, and Opus 5, allowing research biology questions while restricting likely-harmful requests.
- **Exceptions:** organizations doing broader work can apply to the Life Sciences Verification Program and Cyber Verification Program. We broke down the latter in Anthropic's Cyber Verification Program tiers.

If your product wraps a security workflow, test refusals early; a cheap subagent that refuses is more expensive than one that works.

## What to watch

- **Real-world token counts.** The tokenizer change means list-price savings will vary. Log tokens per task for a week.
- **Independent evals.** Everything in the table is from Anthropic. Third-party scores on agentic tasks will show whether the OSWorld and GDPval gains hold outside vendor harnesses.
- **Competitive response.** Anthropic benchmarked against GPT-6 Luna, so expect pricing moves in the small-model tier. Our Sonnet 5.5 vs GPT-6 Astra comparison shows how quickly these tables shift.
- **Where the leak ended.** Our Sonnet 5.5 registry leak post noted Haiku 5.5 was still "coming weeks" at that point; it has now arrived.

## Related reading

*Prices, benchmarks, and availability are as published by Anthropic on October 7, 2026; verify current docs before production use.*

**Update — October 8, 2026:** Worked math on the Sonnet 5.5 cache-read cut, and when the 20% saving holds: cache read cost analysis.

**Update — October 8, 2026:** New comparison — Haiku 5.5 vs GPT-6 Luna vs DeepSeek V4.1 Flash vs Gemini 3.7 Flash vs GLM-5.3-Flash.
