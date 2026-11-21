---
id: collect-261121-veille-ia/gemini-google/3-15-gemini-4-neon-is-62-5-cheaper-than-astra
title: "$3/$15: Gemini 4 Neon is 62.5% cheaper than Astra"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-11-18"]
keywords: ["neon", "model", "gemini", "google", "carbon", "price", "astra", "input"]
source: https://dev.to/frankchu/315-gemini-4-neon-is-625-cheaper-than-astra-4a16
source_anchor: ""
source_lines: [1, 64]
section: "Gemini_Google"
---
Until November 30, Google's flagship reasoning model costs less than its own mid-tier. Gemini 4 Carbon is $2 per million input tokens and $10 per million output tokens through November 30 — then it doubles to $4/$20 on December 1. The new Gemini 4 Neon, announced today, is $3/$15 with no intro price and no end date. So for the next 12 days, the bigger model is the cheaper one, and on December 1 the ladder snaps into the shape Google actually designed: Hydrogen at $1.50/$7.50, Neon at $3/$15, Carbon at $4/$20. I pulled the pricing page and ran the comparisons.

All prices are the standard tier, per million tokens, from the official Gemini API pricing page (pulled today, November 18, 2026). Output price includes thinking tokens — there is no separate thinking charge, which matters for a reasoning model.


```
Model              Input / 1M   Output / 1M   Cached input   Free tier
Hydrogen           $1.50        $7.50         $0.075         yes
Carbon (to 11/30)  $2.00        $10.00        $0.10          no
Carbon (from 12/1) $4.00        $20.00        $0.20          no
Neon               $3.00        $15.00        $0.15          no
Argon (to 11/30)   $2.00        $10.00        $0.10          no
Argon (from 12/1)  $4.00        $20.00        $0.20          no
```
Batch API is exactly half the standard price across the board (Neon batch: $1.50/$7.50). Cached input is 95% off everywhere — Neon's $0.15 against $3.00. And yes, there is a fourth sibling: the pricing page lists a Gemini 4 Argon at Carbon's exact price, and Google's announcement says nothing about what it is. I'm not going to guess.

Neon sits in the middle on price, and Google positions it in the middle on capability too: Carbon for the hardest long-horizon work, Hydrogen for fast everyday execution, Neon for complex, high-frequency workflows — "frontier-level intelligence with fast end-to-end task completion." The blog's own numbers for where Neon lands:


```
Artificial Analysis Intelligence Index   61, #3 overall (xhigh thinking)
Vals Index                               #2, 70.9%
Terminal-Bench 4.1                       65.2%
FrontierCode 2.0                         46.2% (default high)
Terminal-Bench Science                   70.5%, best in class
Agents' Last Exam v1.1                   75.8%
Agent Arena Index                        1336, #2 globally (4,200+ sessions)
Vending-Bench 2                          $16,843, #1
```
Every one of those is Google's own claim, from its own announcement or the benchmarks it cites. I have not independently run any of them, and neither has anyone else yet — the model launched today. Treat the table as the manufacturer's spec sheet, not a review.

The comparison that matters for anyone choosing a model this week is against the two models OpenAI shipped yesterday, GPT-6.2 Astra and Sol, whose prices I verified in yesterday's post:


```
GPT-6.2 Astra   $8.00 in / $40.00 out
Gemini 4 Neon   $3.00 in / $15.00 out   → 62.5% cheaper on both
GPT-6.2 Sol     $2.00 in / $10.00 out
Gemini 4 Neon   $3.00 in / $15.00 out   → 50% more expensive on both
```
So Neon undercuts OpenAI's flagship by nearly two-thirds and costs half as much again as OpenAI's budget model. The honest framing is the ladder, not the headline: Neon is priced to be the model you actually call, all day, for work that is too hard for Sol and too frequent for Astra. Google's cost-per-task claim makes the same point in its own language — Neon at default high thinking costs 32% less per task than Carbon, and "roughly 50% to 80% lower cost per task than top competing frontier models" on the Vals and Agent Arena indexes. Cost per task is a vendor-constructed metric (it folds in how many tokens the model uses to finish, not just the sticker price), so I am quoting it as Google's claim, not mine. But the sticker-price math above is mine, and it checks out.

One more price wrinkle worth noting: Neon's cached input at $0.15 is the same 95% discount as everyone else's, but because Neon has no free tier at all — not even for input — the cheapest way to try it is the $1.50/$7.50 batch tier. Hydrogen is the only Gemini 4 model with a free tier.

The part of the announcement I actually care about is the section Google calls "Gemini 4 Neon for cyber," because it landed on the same day Wiz disclosed two unauthenticated RCEs in nginx — and Wiz says it used Gemini 4 Carbon to validate those vulnerabilities. The capability Google is now productizing is the same capability that found today's bug. Neon's cyber numbers, again all Google's:


```
WebExploitBench v1.2    51.26%  state of the art, "half the cost of competing large models"
SEC-bench Pro           90.5%   state of the art, ~half the cost per challenge
CWE-bench v2            70.7%   +3.4% over Carbon
ExploitGym              frontier performance, half the tokens of Carbon
AutoBaxBench (hard)     76%     state of the art, more secure code than Carbon
Gray Swan IPI benchmark 0.0%    attack success rate at highest thinking level
```
Neon for cyber is not generally available — it rolls out "to trusted defenders in the coming days" through Google's Fairwind Program, and you have to ask your Cloud account team for access. That is the right call and also the tell: the model that is good at finding vulnerabilities is being handed out selectively, while the general model ships to everyone. The same asymmetry showed up in the Wiz disclosure — the researchers who found the nginx bugs used Carbon to validate them, and their exploits are not public.

The safety numbers in the announcement are worth one line each because they are the kind of thing that is easy to overclaim: Neon refuses 1.98% of tasks on Terminal-Bench 4.0 and 0% on Terminal-Bench-Science, takes shortcuts on 2.45% of tasks in Google's coding-cheating evaluation (the lowest of any Gemini 4 model), and scores 0.0% cheating on Terminal-Bench 4.0. Under-service is down 43–56% and overreach down 15–35% versus Carbon. All internal evaluations, all Google's words.

The practical takeaway is a date, not a benchmark. Carbon's intro price — $2/$10, cheaper than the mid-tier Neon — expires November 30. On December 1 it becomes $4/$20, and Neon becomes the rational default for anything you were running on Carbon at the intro price. If you have a workload that fits Carbon, you have 12 days of flagship-at-mid-tier pricing left. If you are choosing a model for a system you will run in January, choose on the December prices, not today's: Hydrogen $1.50/$7.50, Neon $3/$15, Carbon $4/$20. The announcement is a launch; the pricing page is the actual product, and for once the pricing page is more interesting than the announcement.

Sources: Google, "Introducing Gemini 4 Neon" (Nov 18, 2026); Gemini API pricing (Gemini 4 Neon $3.00/$15.00/$0.15; Carbon $2.00/$10.00 through Nov 30, 2026, $4.00/$20.00 from Dec 1, 2026; Hydrogen $1.50/$7.50; Argon same as Carbon; batch 50% off; pulled Nov 18, 2026). OpenAI prices from my own verification yesterday: GPT-6.2 Astra/Sol (Astra $8/$0.40 cached/$40; Sol $2/$0.10/$10). All benchmark figures are Google's claims from its announcement, not independently verified.

## Top comments (0)
