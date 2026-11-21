---
id: collect-261121-veille-ia/minimax/minimax-m3-1-flash-vs-deepseek-v4-1-flash-1017-cheaper-tokens-higher-bill
title: "MiniMax M3.1 Flash vs. DeepSeek V4.1 Flash 1017: cheaper tokens, higher bill"
domain: minimax
role: reference
task: article
actors: ["MiniMax"]
dates: []
keywords: ["flash", "deepseek", "card", "minimax", "model", "october", "same", "tokens"]
source: https://chatoss.ai/blog/minimax-m3-1-flash-vs-deepseek-v4-1-flash-1017
source_anchor: ""
source_lines: [1, 129]
section: "MiniMax"
---
DeepSeek spent October making its cheapest model behave like an expensive one. On October 17 it published **DeepSeek-V4.1-Flash-1017**, a checkpoint it calls a minor update, and the card's own deltas are anything but minor —      Terminal-Bench 4.0 jumps 11.1 points, ProgramBench 8.8, and the model gets faster while writing less. Two days later MiniMax shipped **MiniMax M3.1 Flash**, a 428B-parameter open-weight MoE at exactly the same headline rate card: $0.15 input, $0.60 output, $0.015 cached input per million tokens. Same sticker price, near-identical context window, both claiming the agentic-coding crown. So the only useful question is the one the rate card cannot answer: which one finishes the work, and what does a finished task actually cost?

The short version: **DeepSeek V4.1 Flash 1017 wins agentic coding** — and it wins on both axes at once. On vals.ai's independently run **Vibe Code Bench** v1.1 it scores 87.73% against MiniMax M3.1 Flash's 73.94%, a 13.79-point gap, and it does that at $0.58 per test versus $1.15 —      less than half the cost per task, from the model with the *identical* token price. Artificial Analysis's v4.3.2 Intelligence Index agrees on direction (45 vs 43), as does its Terminal-Bench 4.0 run (34% vs 26%) and LMArena's Code Arena: WebDev board (1665 vs 1589 Elo).

MiniMax's wins are real, just not in the lane the marketing leads with. M3.1 Flash takes AutomationBench-AA (73% vs 67%), Humanity's Last Exam (48% vs 43%) and AA-Omniscience (16 vs 5), which is the business-workflow and factual-recall lane — and it is cheaper per Artificial Analysis task ($0.22 vs $0.26) because it writes fewer reasoning tokens per task. Everything below names the exact benchmark variant and the runner, because these two models are the best argument yet that “same benchmark name” does not mean “same test.”

## What “1017” actually is

It is a date, not a version number: DeepSeek-V4.1-Flash-1017 is the October 17, 2026 checkpoint of V4.1 Flash, and it replaces the September 10 release behind the same `deepseek-flash` model name. DeepSeek's pricing page now lists **DeepSeek-V4.1-Flash-1017** as the model version serving that alias, and the legacy `deepseek-v4-flash` names route to it at Flash prices. The weights are on Hugging Face and ModelScope under MIT, and — this matters — the architecture is unchanged. It is a post-training refresh, not a new model: DeepSeek says the gains come from reinforcement learning with self-feedback, which it credits for better end-to-end project building and shorter outputs.

The company's own card quantifies the refresh. Terminal-Bench 4.0 goes from 31.2 to 42.3, DeepSWE v1.1 from 74.2 to 77.0, ProgramBench from 20.3 to 29.1, and HLE without tools from 36.8 to 42.6. The visual benchmarks move too (BabyVision with tools 89.6 to 92.5), which is odd for a “minor” checkpoint but consistent with a full post-training pass. Here is that table with the deltas spelled out:

| Vendor benchmark | V4.1 Flash (Sep 10) | V4.1 Flash 1017 | Delta | Reported by | 
|---|---|---|---|---|
| Terminal-Bench 4.0 | 31.2 | 42.3 | +11.1 | DeepSeek card | 
| DeepSWE v1.1 | 74.2 | 77.0 | +2.8 | DeepSeek card | 
| ProgramBench | 20.3 | 29.1 | +8.8 | DeepSeek card | 
| NL2Repo-Bench | 65.4 | 68.2 | +2.8 | DeepSeek card | 
| Automation-Bench v1.0.6 | 54.8 | 60.5 | +5.7 | DeepSeek card | 
| Agents' Last Exam | 31.8 | 35.4 | +3.6 | DeepSeek card | 
| HLE without tools | 36.8 | 42.6 | +5.8 | DeepSeek card | 

DeepSeek-V4.1-Flash-1017 model card on Hugging Face, October 17, 2026. Every number in this table is vendor-reported — DeepSeek ran the harnesses — so treat the deltas as the company's claim, not as independent measurement. The independent runs below tell a similar but smaller story.

## What MiniMax M3.1 Flash is

M3.1 Flash is the October 19 refresh of MiniMax's M3 line: a native multimodal MoE with roughly **428B total parameters** and **23B activated per token**, a 1M-token context, and text, image and video input with text output. Three changes explain the speed claims: MiniMax Sparse Attention (MSA) now runs through the whole model including the first three full-attention layers, the routed experts and attention paths get quantization-aware training, and DSpark speculative decoding replaces the earlier EAGLE-style draft head. MiniMax quotes 200+ tokens per second in everyday use and a 170+ TPS median; Artificial Analysis measures 118 tokens per second, so the marketing number is the peak, not the average.

The weight story is the sharpest difference between the two. MiniMax said at launch that weights would come later; they landed on Hugging Face on October 22 — under the **MiniMax Community License**, not MIT. Non-commercial use is free; commercial use must display “Built with MiniMax M3.1-Flash,” and anything above $20 million in yearly revenue needs prior written authorization. DeepSeek's 1017 is plain MIT. If you are embedding a model in a product, that is a legal-review difference, not a footnote.

## Independent benchmarks: where the two Flashes actually met

Three organizations have run both models on the same harness. Artificial Analysis ran its ten-evaluation Intelligence Index v4.3.2 at max effort; vals.ai ran Vibe Code Bench v1.1 with OpenHands; LMArena has both on its Code Arena: WebDev leaderboard, where the votes are human preferences rather than unit tests. Everything in the next table is independent of the two vendors.

| Benchmark (variant) | What it tests | MiniMax M3.1 Flash | DeepSeek V4.1 Flash 1017 | Run by | 
|---|---|---|---|---|
| Artificial Analysis Intelligence Index v4.3.2 | ten-eval composite, max effort | 43 | 45 | Artificial Analysis | 
| Terminal-Bench 4.0 | agentic coding and terminal use | 26% | 34% | Artificial Analysis | 
| AutomationBench-AA | agentic SaaS workflows | 73% | 67% | Artificial Analysis | 
| SciCode | scientific coding | 53% | 56% | Artificial Analysis | 
| Humanity's Last Exam | reasoning and knowledge | 48% | 43% | Artificial Analysis | 
| CritPt | physics reasoning | 22% | 24% | Artificial Analysis | 
| AA-Omniscience | factual recall, hallucination-penalized | 16 | 5 | Artificial Analysis | 
| AA-LCR v1.1 | long-context reasoning | 80% | 83% | Artificial Analysis | 
| AA-Briefcase v1.1 (Elo) | agentic knowledge work | 1473 | 1570 | Artificial Analysis | 
| GDPval-AA v2.1 (Elo) | agentic real-world work | 1466 | 1746 | Artificial Analysis | 
| Vibe Code Bench v1.1 | building web apps from scratch | 73.94% | 87.73% | vals.ai (OpenHands) | 
| Code Arena: WebDev (Elo) | human preference, front-end builds | 1589 | 1665 | LMArena | 

Artificial Analysis comparison page for both models (Intelligence Index v4.3.2, max effort) and vals.ai's Vibe Code Bench v1.1 board, updated October 26, 2026. LMArena's Code Arena: WebDev snapshot is October 22, 2026 (923,854 votes); both models carry a ±13 Elo interval. Elo columns are not percentages and should not be compared with the score columns.

The independent picture is more lopsided than the vendor tables, and it points the same way. On vals.ai's Vibe Code Bench v1.1 — the harness that actually builds an app from a prompt and runs it — DeepSeek wins by 13.79 points, and the per-test accounting explains why the cheaper sticker loses:

| Vibe Code Bench v1.1 | Score | Cost per test | Tokens per test | Duration per test | 
|---|---|---|---|---|
| DeepSeek V4.1 Flash 1017 | 87.73% | $0.58 | 43.65M | 16m14s | 
| MiniMax-M3.1-Flash | 73.94% | $1.15 | 66.86M | 21m04s | 

vals.ai, Vibe Code Bench v1.1, updated October 26, 2026, OpenHands harness, pass@1. This is the single most useful row in the article: a model with the same per-token price costs nearly twice as much per finished task because it spends 66.86M tokens getting there instead of 43.65M.

### Independent scores — MiniMax M3.1 Flash vs. DeepSeek V4.1 Flash 1017 (%)

Harnesses run by someone other than the two vendors: vals.ai (Vibe Code Bench v1.1, updated October 26, 2026) and Artificial Analysis Intelligence Index v4.3.2 components (Terminal-Bench 4.0, AutomationBench-AA, SciCode, Humanity's Last Exam, AA-LCR v1.1). Every benchmark shown has a score for both models. The AutomationBench bars are Artificial Analysis's own AutomationBench-AA, not the “Automation-Bench v1.0.6” table each vendor publishes.

Two honest gaps. First, neither model has an independent SWE-bench Verified score: vals.ai stopped running that board on new releases after September 1, 2026 (it says performance saturated), so the newest entry anywhere near this pair is DeepSeek V4 Pro 0813 at 96.40%. Second, DeepSeek's own Terminal-Bench 4.0 number (42.3) and Artificial Analysis's (34%) are not the same measurement — DeepSeek uses a Claude Code harness, AA uses its own — which is exactly why both are printed here instead of one blended “score.”

## What the vendors claim — same names, different harnesses

Both labs publish tables that include the other model, which makes a rare like-for-like vendor comparison possible. Read it as claims, not evidence, and note that the harnesses differ inside the same row: MiniMax ran Terminal-Bench 4.0 through Codex, DeepSeek through Claude Code; DeepSWE v1.1 is mini-SWE-agent 2.4.5 on MiniMax's card and Mini-SWE-V2 on DeepSeek's.

| Vendor benchmark | MiniMax card | DeepSeek card | Where it lands | 
|---|---|---|---|
| Terminal-Bench 4.0 | 38.1 | 42.3 | DeepSeek | 
| DeepSWE v1.1 | 73.7 | 77.0 | DeepSeek | 
| NL2Repo-Bench | 61.2 | 68.2 | DeepSeek | 
| Automation-Bench v1.0.6 | 62.8 | 60.5 | MiniMax | 
| Agents' Last Exam | 29.6 | 35.4 | DeepSeek | 
| HLE without tools (text-only subset) | 43.4 | 44.0 | DeepSeek | 
| BabyVision (with tools) | 91.0 | 92.5 | DeepSeek | 
| BankerToolBench | 76.2 | — | MiniMax (no 1017 number) | 
| OSWorld 2.0 | 61.7 | — | MiniMax (not published) | 

MiniMaxAI/MiniMax-M3.1-Flash and deepseek-ai/DeepSeek-V4.1-Flash-1017 model cards on Hugging Face, both read October 27, 2026. Vendor-reported throughout; a dash means that vendor did not publish a number for the other model. MiniMax's BankerToolBench column scores the September V4.1 Flash (73.7), not the 1017 checkpoint, so it cannot be used as a head-to-head.

## Pricing: the same headline number, different fine print

Both vendors advertise $0.15 input and $0.60 output per million tokens, and that is where the similarity ends. MiniMax bills one flat rate with no time-of-day clock and no peak tier on M3.1 Flash. DeepSeek bills off-peak and peak, with peak exactly double, and peak hours are 01:00 to 04:00 and 06:00 to 10:00 UTC Monday through Friday, excluding Chinese public holidays. The cache line is the real divergence: $0.015 per million cached reads on MiniMax against $0.003 off-peak on DeepSeek — five times the price for the tokens an agent reads most.

| Model / window | Input $/M | Output $/M | Cache read $/M | Context / max output | 
|---|---|---|---|---|
| MiniMax M3.1 Flash (flat) | $0.15 | $0.60 | $0.015 | 1M / not published on the API pricing page | 
| DeepSeek V4.1 Flash 1017 (off-peak) | $0.15 | $0.60 | $0.003 | 1M / 384K | 
| DeepSeek V4.1 Flash 1017 (peak) | $0.30 | $1.20 | $0.006 | 1M / 384K | 

MiniMax API pricing overview and DeepSeek Models & Pricing, checked October 27, 2026. Both cards are quoted as $0.15/$0.60 in the headline, which is why the per-token price is useless for choosing between them. MiniMax does not publish a max-output figure on its pricing page; the model card lists only the 1M context window.

## Cost per task, cache-aware

An agent re-reads its context on every step, so its bill is mostly cache reads, not fresh input. We reuse the profile from our real-bill teardown of a one-hour agent run (August 14, 2026): a 99.4% cache-hit rate, **31.0M cache reads, 175K uncached input tokens and 107K output tokens** across 157 API calls. That mix is a modeled assumption measured from one real agent, not a benchmark average — which is why the ladder below re-runs the same total input at 90% cache hits and with caching off entirely.

The per-line math at list prices. MiniMax M3.1 Flash: 31.0M cache reads × $0.015 = $0.47, plus 175K uncached input × $0.15 = $0.03, plus 107K output × $0.60 = $0.06 — **$0.56** for the hour. DeepSeek V4.1 Flash 1017 off-peak: 31.0M × $0.003 = $0.09, plus 175K × $0.15 = $0.03, plus 107K × $0.60 = $0.06 — **$0.18** for the same work, 3.0 times cheaper. At DeepSeek's peak rates the same hour is $0.37, still a third less than MiniMax's flat bill.

Then move the cache share, because that is the only variable that matters here. At 90% cache hits DeepSeek's bill rises to $0.62 against MiniMax's $0.95 — a 1.5-times gap instead of 3.0 times. With caching switched off entirely the two are identical at $4.74, because input and output prices are the same; the entire difference in this matchup is the cache discount (90% off on MiniMax, 98% off on DeepSeek). And in DeepSeek's peak window the 90% case flips: $1.23 against MiniMax's $0.95. Cold traffic is a wash, but peak-hour traffic with a partial cache makes MiniMax the cheaper model.

| Scenario (token mix) | MiniMax M3.1 Flash | DeepSeek 1017 (off-peak) | DeepSeek 1017 (peak) | 
|---|---|---|---|
| Cache-heavy hour — 31.0M cache reads + 175K uncached + 107K output | $0.56 | $0.18 | $0.37 | 
| 90% cache hits — 28.1M cache reads + 3.1M uncached + 107K output | $0.95 | $0.62 | $1.23 | 
| Zero cache — 31.2M uncached input + 107K output | $4.74 | $4.74 | $9.48 | 

Modeled at MiniMax's flat $0.15/$0.60/$0.015 rate card and DeepSeek's off-peak/peak rates from the table above, on the cache-heavy token mix from our real agent-bill teardown. The 90% and zero-cache rows keep the same total input (31.175M tokens) so only the cache share moves. These are per-hour figures for one modeled workload, not a claim about any specific benchmark's cost.

### Cost per modeled agent hour — caching decides the winner ($)

Modeled at MiniMax's flat $0.15/$0.60/$0.015 rate card and DeepSeek's $0.15/$0.60/$0.003 off-peak, $0.30/$1.20/$0.006 peak rates. The token mix is an assumption taken from our real agent-bill teardown, not a benchmark average; the 90% and zero-cache rows re-run the same total input so only the cache share changes.

The independent cross-check lands in the same place, and it is worth stating plainly because it uses a different method. Artificial Analysis prices every token an Intelligence Index task consumes — input, cache hits, reasoning and answer — and reports **$0.22 per task** for M3.1 Flash against **$0.26 per task** for V4.1 Flash 1017 at DeepSeek's peak rates. MiniMax wins that comparison because AA tasks are output-heavy and M3.1 Flash writes 79k output tokens per task to DeepSeek's 89k. Flip to vals.ai's coding harness, where the context is re-read rather than regenerated, and the ranking inverts: $1.15 vs $0.58 per test. The lesson is not that either number is wrong. It is that “cost per task” is meaningless without the token mix attached.

## Which Claude or OpenAI tier is this?

On Artificial Analysis's composite the pair lands at 43 and 45 — the same band as Claude Haiku 5.5, which also scores 43 on Intelligence Index v4.3.2. That is the honest tier: these are small-flagship models, not frontier reasoners. The nuance is that their agentic components punch above that composite. M3.1 Flash's AutomationBench-AA score of 73% is more than double Claude Haiku 5.5's 35%, and on the vendors' own DeepSWE v1.1 tables DeepSeek 1017's 77.0 edges Claude Opus 5's 74.0 and GPT-5.6 Sol's 73.0. So the useful description is **Haiku-5.5-tier breadth with Opus-tier agentic execution**: cheap enough to run in a loop, competent enough to finish the loop.

## What to pick

**Default to DeepSeek V4.1 Flash 1017 for coding agents and anything that builds software.** It wins every independent coding harness both models appear on — Vibe Code Bench by 13.79 points, Terminal-Bench 4.0 by 8 points, LMArena's WebDev board by 76 Elo — and it is 3.0 times cheaper on cache-heavy agent traffic and roughly half the cost per finished code task in vals.ai's measured runs. The MIT license removes the commercial-use paperwork, and the 384K max output is explicitly documented. Nothing in the October 17 checkpoint made it more expensive: the rate card, the 1M context and the 384K output ceiling are unchanged from September 10.

**Pick MiniMax M3.1 Flash for computer use, document-heavy back-office automation and long-context reading.** It wins AutomationBench-AA by 6 points, Humanity's Last Exam by 5, AA-Omniscience by 11, and its 61.7 on OSWorld 2.0 is a score DeepSeek does not publish at all. It is also genuinely cheaper per AA task ($0.22 vs $0.26) because it writes fewer reasoning tokens. Just budget the verbosity and the latency: 66.86M tokens per Vibe Code Bench test is what turns a $0.15 rate into a $1.15 bill, and AA measures it at 582.81s per task against DeepSeek's 383.96s, with a first answer token at 18.11s versus 12.94s.

One caveat cuts against each model, and one against the pair. DeepSeek wins the coding lane while sitting eleven points below MiniMax on AA-Omniscience (5 vs 16), so it is the more confident hallucinator when it does not know something — do not put it in front of customers as a knowledge base. MiniMax's 93% hallucination rate among non-correct answers, reported by Artificial Analysis and covered by VentureBeat, is the number to keep in view: it almost never declines to answer, so a better Omniscience score is not the same as safe for factual work. And both are priced per token at rates that invite overuse: at zero cache they cost exactly the same $4.74 per modeled hour, which is 26 times DeepSeek's cache-heavy bill. Caching is the product, not an optimization.
