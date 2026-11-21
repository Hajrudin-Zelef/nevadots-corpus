---
id: collect-261121-veille-ia/gemini-google/gemini-4-neon-ahead-of-opus-5-5-and-gpt-6-2-sol-on-8-of-10-benchmarks-at-3-15
title: "Gemini 4 Neon: Ahead of Opus 5.5 and GPT-6.2 Sol on 8 of 10 Benchmarks at $3/$15"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-11-18"]
keywords: ["gemini", "neon", "google", "carbon", "opus", "hydrogen", "tokens", "input"]
source: https://llm-stats.com/blog/research/gemini-4-neon-launch
source_anchor: ""
source_lines: [1, 180]
section: "Gemini_Google"
---
## At a Glance

With official scores in, Gemini 4 Neon reads as a measured middle tier: Google's own table puts it ahead of Claude Opus 5.5 and GPT-6.2 Sol on 8 of 10 shared rows, strongest past 256K tokens and on agentic knowledge work, at $3 / $15 flat to 1M. Through November 30, 2026 it still costs more per token than Carbon; from December 1 it sits cleanly between Hydrogen and Carbon on price while leading Carbon on most shared family rows.

- Generally available **November 18, 2026** . Stable API id:`gemini-4-neon` . Model page: Gemini 4 Neon.
- Price: **$3 input / $15 output** per million tokens, flat across a 1,048,576-token unified window. Cached input $0.15. Paid tier only.
- Twice Gemini 4 Hydrogen ($1.50 / $7.50). 25% under Gemini 4 Carbon's list from December 1, 2026 ($4 / $20). 50% above Carbon's introductory $2 / $10 through November 30, 2026.
- No free tier. No introductory price for Neon.
- Default thinking is `high` ;`xhigh` is available (same shape as Hydrogen).
- Self-reported by Google: Neon leads Opus 5.5 and GPT-6.2 Sol on 8 of 10 shared rows, with the largest leads on GraphWalks 256K to 1M (+28.4 over Opus, +15.1 over Sol) and AutomationBench (+13.8 over Opus, +15.0 over Sol). It trails Opus on FrontierCode 2.0 (46.2 vs 47.3) and Sol on Chartography (73.5 vs 75.1).
- Against its siblings on Google-reported family rows, Neon leads Carbon on 7 of 9 shared rows; Carbon still leads GraphWalks (96.2 vs 95.2) and Agents' Last Exam v1.1 (77.4 vs 75.8).
- Opus 5.5 lists at $4 / $20; Sol at $2 / $10, rising to $4 / $15 above 272K input. Neon is 25% under Opus per token, 50% over Sol up to 272K, and cheaper than Sol above that surcharge.
- Google names Neon the upgrade for Gemini 3.1 Pro Preview, with the clearest savings on prompts over 200K tokens.

## What Gemini 4 Neon Is

Google describes Gemini 4 Neon as **frontier-level intelligence with fast end-to-end task completion across complex software engineering, knowledge work, and science** workflows. In Google's words, it sits between Gemini 4 Hydrogen and Gemini 4 Carbon for high-frequency complex tasks: deeper reasoning than Hydrogen and faster task completion than Carbon. Google says it finishes faster with fewer output tokens than Carbon while handling harder reasoning than Hydrogen, and that you should choose it where Hydrogen is not enough and you do not need Carbon's extra verification. Google still publishes no token counts for that fewer-output-tokens claim.

Google's speed labels put Carbon at Deliberate, Neon at Moderate, Hydrogen at Fast, and Gemini 3.5 Flash-Lite at Fastest. Surfaces: Gemini API paid tier only (`generateContent` and the Interactions API); Google AI Studio on Pro and Ultra plans; Vertex AI (GA for verified enterprise customers in select regions: global, US, EU); the Antigravity agent in Gemini Managed Agents (alone or paired with Hydrogen); and OpenRouter as `google/gemini-4-neon`. Knowledge cutoff is March 2026. See Google's what's new notes, model page, and changelog.

### Specs

Context is a unified **1,048,576-token** window shared by input, thinking, and output. Output defaults to 65,536 tokens per synchronous request (also the single-request cap, streaming or not). Long decode reaches up to 1,048,576 minus input with `background: true` and `max_output_tokens` above 65,536, or with client-side continuation in the latest SDKs. If you set background without raising max output, it still stops at 65,536.

Input: text, image, video, audio, PDF (Office and OpenDocument files also accepted). Output: text only. No image generation, no audio generation, no Live API. Tools include code execution, computer use, file search, function calling, Google Search and Maps grounding, structured outputs, and URL context. Consumption modes: Standard, Batch, Flex, Priority. Caching is implicit only on prompts of at least 4,096 tokens (95% off cached input). Explicit caching is not supported. Sampling parameters (`temperature`, `top_p`, `top_k`) are ignored; out-of-range values return a 400. Google says hard prompts can take several minutes, so use background execution or client timeouts of at least 15 minutes.

### What Google claims Neon does

According to Google, Neon does repository-level debugging and multi-file refactoring in fewer steps with clean edits; in long tool-heavy sessions it reduces redundant file reads, handles context compaction reliably, and minimizes fabricated claims and task shortcuts; it synthesizes dense documents, spreadsheets, and charts across the 1M window; it writes code with fewer security vulnerabilities and reduces agentic overreach; and its dual-use offensive cyber capabilities are offered to defenders through Google's Fairwind Program. Those descriptions now sit next to numbers on Google's table: long-document synthesis maps to GraphWalks 256K to 1M (95.2) and LVBench (89.0); tool-heavy agent work maps to AutomationBench (56.3), Agents' Last Exam v1.1 (75.8), and Vending-Bench 2 ($16,843 final balance); security claims map to SEC-bench Pro (90.5) and WebExploitBench v1.2 (51.3, Google-reported). Everything else remains Google's description without a matching score in this post.

## Where Neon Sits

The figure below places the Gemini 4 family by output price and by Google's stated reasoning order. Vertical position is ordinal only. Measured scores live in the Benchmarks section.

### The ladder flips on December 1

Through November 30, 2026 the paid-tier input ladder is Hydrogen $1.50, Carbon $2, Neon $3. From December 1, 2026 it becomes Hydrogen $1.50, Neon $3, Carbon $4. Neon has no introductory price, so it was priced for the post-promo ladder. Through November 30, Carbon is cheaper per token than Neon. Say that plainly when you plan a switch. If this post is still open after November 30, the intro window is closed and the Dec 1 order is the live ladder.

That flip is the main reason Neon looks expensive today and mid-tier later. Google released Carbon on the Gemini API paid tier on November 10, 2026 with a short introductory price. Neon arrived eight days later at its permanent list. The middle seat only exists cleanly once Carbon moves to $4 / $20.

### Cost per task against Opus 5.5 and Sol

Output price includes thinking, so cost per task depends on how many tokens each model spends. At Carbon's December 1 list price, Neon matches Carbon's cost per task even if it spends 1.33x Carbon's tokens. At Carbon's introductory price, Neon only wins if it spends under 0.67x. Against Opus 5.5 at $4 / $20, Neon at $3 / $15 matches cost per task if it spends up to 1.33x Opus's tokens. Against Sol at $2 / $10 on prompts up to 272K, Neon must spend 0.67x Sol's tokens or fewer. Above 272K input, Sol bills $4 / $15 across the whole request and Neon is cheaper at equal token counts. Google gives no token-use numbers, so these break-evens are arithmetic only.

Worked requests at the same token counts on every model (illustrative only; real token use differs by model and thinking level): 100K in / 10K out costs Sol $0.30, Neon $0.45, Opus 5.5 $0.60. A 500K in / 20K out long document costs Neon $1.80, Sol $2.30 (surcharged), Opus 5.5 $2.40; Hydrogen $0.90 and Carbon from Dec 1 $2.40. Google also says that at its default high thinking level Neon sits on the Pareto frontier of the Vals Index and Agent Arena "with roughly 50% to 80% lower cost per task than top competing frontier models." That cost-per-task claim is unverified; Google has published no token counts behind it.

Batch is a quiet anchor here. Neon on Batch or Flex ($1.50 / $7.50) costs exactly Hydrogen's standard price. If you can tolerate Batch or Flex latency, Neon's middle-tier intelligence lands at the same per-token rate as Hydrogen Standard. Priority goes the other way: Neon Priority at $5.40 / $27 sits 25% under Carbon Priority from December 1 ($7.20 / $36).

### Neon thinks like Hydrogen, not Carbon

Neon defaults to `high` and offers `xhigh`, matching Hydrogen. Carbon (and Argon) defaults to `medium` and caps at `high`. A Carbon user who switches without setting `thinking_level` is running a heavier default. That changes both latency and bill, before any quality delta shows up.

Carbon, Neon, and Hydrogen share a request schema, window, and tools, so you can move steps between them with `previous_interaction_id`. Google's own pitch is that kind of pairing: use Neon where Hydrogen is not enough, and keep Carbon for turns that need extra verification.

## Benchmarks

On Google's Gemini 4 Neon model card, Neon leads Claude Opus 5.5 and GPT-6.2 Sol on 8 of 10 shared rows. It is strongest on long context (GraphWalks 256K to 1M) and agentic knowledge work (AutomationBench, Agents' Last Exam), and it trails Opus on FrontierCode 2.0 and Sol on Chartography. All scores are self-reported by Google and not independently verified.

### Where Neon leads

GraphWalks BFS at 256K to 1M is the headline: Neon 95.2 vs Opus 66.8 and Sol 80.1. That gap matters because Neon's 1M window is flat priced, while Sol adds a 272K input surcharge ($4 / $15 across the whole request). If your workload lives past a few hundred thousand tokens, Neon's score and its price both point the same way.

On agentic knowledge work, AutomationBench (56.3 vs 42.5 and 41.3) and Agents' Last Exam v1.1 (75.8 vs 71.6 and 73.0) are the clearest leads. Terminal-Bench-Science 0.1 (70.5 vs 63.3 and 68.0) and RiemannBench (84.0 vs 80.0 and 80.0) extend that pattern into science and math, with the caveat that RiemannBench sits near the top of its scale, so a 4-point gap there is thin.

### Where it trails

FrontierCode 2.0 is the one Opus win: 47.3 vs Neon's 46.2. Neon ran that row at high thinking (its default), not xhigh. Coding-first teams already on Opus 5.5 should treat that as a reason to keep Opus until their own evals say otherwise. Chartography is the one Sol win: 75.1 vs Neon's 73.5. Chart-heavy multimodal work on Sol can stay put, especially at Sol's lower list price on short prompts.

### Against Carbon and Hydrogen

Neon leads Carbon on 7 of 9 shared Google-reported rows while priced 25% under Carbon from December 1 (and 50% above Carbon through November 30). Carbon still leads GraphWalks (96.2 vs 95.2) and Agents' Last Exam v1.1 (77.4 vs 75.8). Vending-Bench 2 final balances are Neon $16,843, Carbon $16,484, and Hydrogen $14,549. These family numbers come from separate Google tables; Neon is scored at xhigh, a level Carbon does not offer (Carbon tops out at high); and Carbon's Terminal-Bench-Science number is a third-party run while Neon's and Hydrogen's are Google's own mini-swe-agent runs.

### More rows Google reported

Four extra rows appear on the model card outside the main comparison chart. They are Google-reported. On SWE-sweep, the mini-SWE harness row is the fair comparison (same harness for all three); the provider-harness row (Neon 39.0) reflects each vendor's own agent harness and is not like-for-like.

| Benchmark | Neon | Opus 5.5 | GPT-6.2 Sol | 
|---|---|---|---|
| CursorBench 4.1 | 65.4% | 58.1% | n/r | 
| SWE-sweep, mini-SWE harness | 12.8% | 3.6% | 11.6% | 
| SWE-sweep, provider harness | 39.0% | 10.1% | 8.3% | 
| WebExploitBench v1.2 | 51.3% | 37.1% | n/r | 

### How to read these numbers

Several rows mix Google's own Neon runs with published numbers for Opus and Sol (Agents' Last Exam, Terminal-Bench-Science, Chartography, SEC-bench Pro). LVBench used different frame budgets across models. RiemannBench, LVBench, SEC-bench Pro, and GraphWalks sit near the top of their scales, so gaps of 1 to 4 points there are thin. Treat the table as Google's same-source snapshot, not as an independently audited ranking. Primary sources: model card, evaluation methodology, and launch post.

## Pricing

Gemini API paid tier, flat across the full 1M window (no long-context surcharge). Every token is billed, including thinking tokens. Google: Neon has no introductory price. That matters for planning: there is no temporary Neon discount to race toward or wait out. The only calendar price move that changes Neon's relative seat is Carbon's climb from $2 / $10 to $4 / $20 on December 1, 2026. Full list: Gemini API pricing.

| Mode | Input | Output (incl. thinking) | Cached input | 
|---|---|---|---|
| Standard | $3.00 | $15.00 | $0.15 | 
| Batch | $1.50 | $7.50 | $0.075 | 
| Flex | $1.50 | $7.50 | $0.075 | 
| Priority | $5.40 | $27.00 | $0.27 | 

Search and Maps grounding: 5,000 free requests a month shared across Gemini 3 and newer models, then $14 per 1,000. Vertex AI global matches Standard at $3.00 / $15.00 / $0.15. Non-global endpoints are 10% higher ($3.30 / $16.50 / $0.165). See Vertex AI pricing.

### Against Opus 5.5 and Sol

Claude Opus 5.5 lists at $4 / $20 / $0.20 with a 1M context and no long-context surcharge listed. GPT-6.2 Sol lists at $2 / $10 / $0.10; prompts above 272K input tokens cost 2x input and cache and 1.5x output across the whole request ($4 / $15). Neon is 25% cheaper per token than Opus and 50% more per token than Sol on prompts up to 272K. Above 272K, Neon input is 25% under Sol's $4 and output equals Sol's $15, so Neon is cheaper than Sol at equal token counts. See our Opus 5.5 launch post and GPT-6.2 Astra and Sol launch post.

### Family ladder

Sibling prices on the Gemini API standard paid tier. Argon matches Carbon. Carbon has been GA on the Gemini API paid tier since November 10, 2026.

| Model | Input | Output | Cached | Free tier | Default thinking | 
|---|---|---|---|---|---|
| Gemini 4 Hydrogen | $1.50 | $7.50 | $0.075 | Yes | high (has xhigh) | 
| Gemini 4 Neon | $3.00 | $15.00 | $0.15 | No | high (has xhigh) | 
| Gemini 4 Carbon, through Nov 30, 2026 | $2.00 | $10.00 | $0.10 | No | medium (max high) | 
| Gemini 4 Carbon, from Dec 1, 2026 | $4.00 | $20.00 | $0.20 | No | medium (max high) | 
| Claude Opus 5.5 | $4.00 | $20.00 | $0.20 | n/a | n/a | 
| GPT-6.2 Sol, up to 272K | $2.00 | $10.00 | $0.10 | n/a | n/a | 
| GPT-6.2 Sol, above 272K | $4.00 | $15.00 | $0.20 | n/a | n/a | 
| Gemini 3.1 Pro Preview, up to 200K | $2.00 | $12.00 | $0.20 | n/a | n/a | 
| Gemini 3.1 Pro Preview, over 200K | $4.00 | $18.00 | $0.40 | n/a | n/a | 

Batch and Flex: Hydrogen $0.75 / $3.75; Carbon $1 / $5 through November 30, then $2 / $10. Priority: Hydrogen $2.70 / $13.50; Carbon $3.60 / $18 through November 30, then $7.20 / $36. Neon Batch and Flex ($1.50 / $7.50 / $0.075) equal Hydrogen's standard price. Neon Priority ($5.40 / $27) is 25% under Carbon's Priority from December 1 ($7.20 / $36).

A fully cached 100K-token prefix costs $0.015 on Neon instead of $0.30. Vs Gemini 3.1 Pro Preview on prompts up to 200K, Neon is 50% more on input and 25% more on output. Over 200K, Neon is 25% less on input, about 17% less on output ($15 vs $18), and 62.5% less on cached input ($0.15 vs $0.40). See Google's choosing a model guide.

## Thinking Levels

Neon supports four thinking levels: `low`, `medium`, `high` (default), and `xhigh`. `minimal` returns an error.

- `low` : fewest tokens, lowest latency. Google says still capable on agentic tasks.
- `medium` : interactive coding and latency-sensitive tool calls.
- `high` : balanced intelligence and speed. The recommended default.
- `xhigh` : most tokens, deepest reasoning. For hard multi-step problems where`high` falls short.

Google's advice: run evals at all four levels and pick the lowest `thinking_level`that meets your quality bar. The default is heavier than Carbon's `medium`, so set the level explicitly when you migrate from Carbon or Argon.

## Should You Switch?

Short decision guide from Google's self-reported scores, the price ladder, and the family bracket.

| Your situation | Recommendation | Why | 
|---|---|---|
| On Gemini 3.1 Pro Preview | Move to Neon | Google's named path. Biggest savings on prompts over 200K. Re-test short-prompt costs (+50% input). | 
| On Hydrogen, hitting a reasoning ceiling | Try Neon at 2x the price | Test `medium` first. Google places Neon above Hydrogen for harder reasoning, and Neon leads Hydrogen on the shared family rows both report. | 
| On Carbon, before Dec 1 | Stay | Carbon is cheaper per token through November 30, 2026. | 
| On Carbon, from Dec 1, high-volume tasks without hard verification needs | Test Neon | 25% cheaper per token and ahead of Carbon on 7 of 9 shared Google-reported rows. Carbon keeps GraphWalks and Agents' Last Exam. Set `thinking_level` . | 
| Long-context work over 272K tokens | Prefer Neon | Top GraphWalks 256K to 1M score (95.2) and cheaper than Sol once Sol's 272K surcharge applies. | 
| Agentic workflows on Opus 5.5 | Test Neon | Ahead on AutomationBench and Agents' Last Exam at 25% less per token. | 
| Coding-first teams on Opus 5.5 | Stay on Opus, or eval carefully | Opus still edges FrontierCode 2.0 (47.3 vs 46.2). Test before switching. | 
| Chart-heavy multimodal work on Sol | Stay on Sol, or eval carefully | Sol edges Chartography (75.1 vs 73.5) at a lower list price on short prompts. | 
| Need a free tier | Hydrogen | Neon has no free tier. | 
| Offline or bulk jobs | Neon Batch or Flex | $1.50 / $7.50, equal to Hydrogen Standard. | 

### Migration checklist

From Carbon, Argon, or Hydrogen: no request schema changes. Set `model` to `gemini-4-neon`. Set `thinking_level` explicitly. Use a paid-tier project.

From Gemini 3.8 Flash, 3.1 Pro Preview, or earlier: change the model string. Plan for one unified 1,048,576-token window. Remove low `max_output_tokens` caps (for example 8,192) or raise them to 65,536, and for long outputs use background mode with 131,072 or 262,144. `thinking_level` defaults to `high` and rejects `minimal`. Remove explicit caching (`cached_content`). Remove `temperature`, `top_p`, `top_k`. Declare tools natively or use structured outputs (raw-text JSON tool calls with no declared tools can return `MALFORMED_FUNCTION_CALL`). Return thought signatures unchanged and use `previous_interaction_id`. Raise client timeouts to at least 15 minutes.

Google's upgrade table: Gemini 3.1 Pro Preview goes to `gemini-4-neon` (or `gemini-4-carbon` for your hardest problems). Gemini 3.8 Flash and 2.5 Pro go to Hydrogen.

## FAQ

- Google released**Gemini 4 Neon** on**November 18, 2026** . The stable API id is`gemini-4-neon` . It is generally available on the Gemini API paid tier, Google AI Studio (Pro and Ultra), Vertex AI for verified enterprise customers in select regions, the Antigravity agent in Gemini Managed Agents, and OpenRouter as`google/gemini-4-neon` .
- On the Gemini API paid tier:**$3.00 input** ,**$0.15 cached input** (implicit), and**$15.00 output** per million tokens, flat across the full 1,048,576-token window. Batch and Flex are $1.50 / $7.50. Priority is $5.40 / $27.00. There is no free tier. Vertex AI global matches the standard list; non-global endpoints are 10% higher. Google says Neon has no introductory price.
- Through**November 30, 2026** , no: Carbon's introductory price is $2 / $10, so Neon at $3 / $15 costs 50% more per token. From**December 1, 2026** , yes on list price: Neon is 25% under Carbon's $4 / $20. On Google-reported family rows Neon also leads Carbon on 7 of 9 shared benchmarks. Cost per task still depends on token use, and Google has not published token counts.
- Neon is**twice** Hydrogen on input, output, and cache ($3 / $15 / $0.15 vs $1.50 / $7.50 / $0.075). Both default to`high` thinking and support`xhigh` . Google places Neon above Hydrogen for deeper reasoning and positions Hydrogen as the faster, cheaper worker. Neon Batch and Flex ($1.50 / $7.50) equal Hydrogen's standard price. See our Gemini 4 Hydrogen launch post.
- In Google's self-reported model card table, Neon leads Claude Opus 5.5 and GPT-6.2 Solon 8 of 10 shared rows. Its clearest leads are GraphWalks 256K to 1M (95.2 vs 66.8 and 80.1) and AutomationBench (56.3 vs 42.5 and 41.3). It trails Opus on FrontierCode 2.0 (46.2 vs 47.3) and Sol on Chartography (73.5 vs 75.1). Those scores are not independently verified. See Google's model card.
- On Google's same-table scores, Neon leads both on 8 of 10 rows and is strongest on long context and agentic knowledge work. Opus 5.5 still edges FrontierCode 2.0; Sol edges Chartography. On price, Neon at $3 / $15 is 25% under Opus ($4 / $20) and 50% over Sol ($2 / $10) on prompts up to 272K; above 272K Sol surcharges to $4 / $15 and Neon is cheaper at equal token counts. Pick Opus if coding agent work is the priority, Sol if short multimodal chart work at a lower list price matters most, and Neon when long-context agent work and the flat 1M window are the job.
- Context is a**unified 1,048,576-token** window shared by input, thinking, and output. Synchronous responses return up to**65,536 tokens** by default (streaming or not). Long decode extends output up to 1,048,576 tokens minus input with`background: true` and`max_output_tokens` above 65,536, or via client-side continuation. Setting`background: true` without`max_output_tokens` still stops at 65,536.
- Neon supports`low` ,`medium` ,`high` (default), and`xhigh` . Requests with`minimal` return an error. Google's guide:`low` for fewest tokens and lowest latency;`medium` for interactive coding;`high` as the balanced default;`xhigh` for hard multi-step problems where`high` falls short. Google says run evals at all four levels and pick the lowest that meets your quality bar. Carbon defaults to`medium` and caps at`high` , so a Carbon user who switches without setting`thinking_level` runs a heavier default.
- Google names Neon as the upgrade path for Gemini 3.1 Pro Preview (or Carbon for your hardest problems). On prompts over 200K tokens, Neon is cheaper: 25% less on input, about 17% less on output ($15 vs $18), and 62.5% less on cached input. On prompts up to 200K, Neon costs 50% more on input and 25% more on output, so re-test short-prompt workloads.
- Yes. Vertex AI lists Gemini 4 Neon as generally available for verified enterprise customers in select regions (global, US, EU). OpenRouter offers it as`google/gemini-4-neon` at the same price. See Google's Vertex model page.

## Outlook

Google now has a full three-step Gemini 4 ladder: Hydrogen for volume and free-tier work, Neon for the middle, Carbon for deliberate verification. Neon's capability is measured on Google's table: ahead of Opus 5.5 and Sol on 8 of 10 shared rows, and ahead of Carbon on 7 of 9 shared family rows. The open questions are independent replication, token use per task (no counts published), Carbon's December 1, 2026 price change, and wider Vertex availability beyond verified enterprise customers in select regions.

Watch three items. Independent runs that confirm or revise Google's Neon scores. Carbon's December 1, 2026 price change, which is when the ladder finally matches Neon's list. And Vertex availability widening. The clearest switch that already lines up on both score and price is long-context work past 272K tokens, where Neon leads GraphWalks and undercuts Sol once the surcharge applies, plus moving off Gemini 3.1 Pro Preview on long prompts where Neon already undercuts on list price above 200K tokens.

Continue Reading
