---
id: collect-261121-veille-ia/copilot-microsoft/seven-mai-models-at-ignite-2026-where-mai-code-2-fits-in
title: "Seven MAI Models at Ignite 2026: Where MAI Code 2 Fits In"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["code", "microsoft", "model", "models", "says", "mai", "seven", "announcement"]
source: https://www.orcarouter.ai/blog/mai-code-2-ignite-seven-models
source_anchor: ""
source_lines: [1, 71]
section: "Copilot_Microsoft"
---
Microsoft used its Ignite 2026 keynote on 17 November to put seven new models on stage at once, and the one developers will argue about is **MAI Code 2**. It is sold not on a score but on a ratio: Microsoft's own announcement says it reaches 96% of **Claude Opus 5.5**'s performance across four coding benchmarks for 5% of the cost, and 98% of **GPT-6.1 Sol**'s for 40%. Those are vendor numbers from Microsoft's internal evaluation harness, they have not been reproduced by anyone outside Microsoft, and the model had no independent leaderboard row at the time of writing. What makes the launch worth reading anyway is that the price ratio is real and verifiable, the model card is unusually specific, and the distribution is narrower than the announcement's tone suggests.

This is the shape of the launch: what shipped, what the claims are averaged from, what the model card says that the blog post does not, what it costs and when that price changes, and why the coding model is the one that matters for a budget rather than the flashiest one in the set.

## What was actually announced

Seven models, one chip, one licensing programme. The models are MAI Cyber 2 Flash for defensive security work, MAI Code 2 and MAI Code 2 Flash for coding, MAI Image 2.7 for image generation and editing, MAI Realtime 1 for live speech-to-speech, Dragon Copilot Transcribe built on MAI Transcribe 2, and Decision 1.1, a router model Microsoft says is 50x faster and 79x cheaper than GPT-Sol 6.1. Alongside them Microsoft put Maia 200, its second-generation accelerator, into production, and introduced MAI Licensed Weights, which lets an organisation run a customised MAI model inside its own Azure environment.

The coding pair is where the money is. MAI Code 2 is the larger model, positioned for deep debugging, multi-file implementation, repository-wide reasoning and front-end work. MAI Code 2 Flash is the small one: code explanations, targeted edits, routine fixes, rapid iteration. Microsoft's framing is explicit that these are everyday-enterprise models rather than frontier-chasing ones, and the announcement says so twice.

## The claim Microsoft is leading with, and what it is an average of

The 96% and 98% figures are the mean of four benchmarks: DeepSWE 1.1, Terminal Bench 2.1, SWE Atlas and FrontierCode v1.1. Three of those were run on MSBench, Microsoft's internal evaluation platform, using the GitHub Copilot CLI at version 1.0.102 with subagents disabled and every model at maximum reasoning effort. The FrontierCode component is different in kind: those scores and costs come from Artificial Analysis, not from Microsoft's harness. So the headline is a blend of three self-run tests and one third-party test, then expressed as a percentage of two competitors' averages.

A second claim sits beside it, and it is the one that says most about what Microsoft thinks it built. In blind side-by-side evaluations of front-end work covering interfaces, games, data visualisations, 3D scenes and SVG graphics, reviewers preferred MAI Code 2's output over GPT-6 Sol's 64% of the time, across 258 annotators. That is a preference study, not a pass rate, and preferences are cheap to win with confident-looking UI code. It is also the only claim in the announcement that speaks to taste rather than correctness, and taste is where a cheap model can plausibly beat an expensive one on a real product team.

## What the model card says that the blog post does not

The 24-page model card is the more useful document, because it contains the numbers a buyer needs and the announcement omits.

• Architecture — a decoder-only transformer with sparse mixture-of-experts layers and an integrated vision encoder, in both models

• Parameters — MAI Code 2 at roughly 2.8 trillion total, 77 billion active, with 3 billion embedded; MAI Code 2 Flash at roughly 398 billion total and 12 billion active

• Training — about 50 trillion tokens of pre-training plus about 150 billion tokens of long-context extension for the large model, all between August and November 2026, with a knowledge cut-off of August 2026

• Context and limits — 1,050,000-token context, a 922,000-token maximum input and 128,000 maximum output tokens, text and image in, text out

• Reasoning control — five effort levels, low through max, trading latency and cost against quality

• Stated limits — optimised for English code production only at release, not tuned for fundamental science research or very long-horizon agentic work, and not intended for fully autonomous systems that take consequential actions without human oversight

That last line is the honest one, and it is the sentence a procurement team should read twice. Microsoft is not claiming a general frontier model at a discount. It is claiming a coding model that is good at the work GitHub Copilot customers actually do, and it says plainly where the model stops.

## Where you can call it, which is narrower than it sounds

MAI Code 2 and MAI Code 2 Flash are both in GitHub Copilot, across Copilot Pro, Pro+, Max, Business and Enterprise for the larger model, and from the Free tier upward for Flash. MAI Code 2 Flash is additionally in limited-access preview in Microsoft Foundry, callable through an OpenAI-compatible Responses API. MAI Code 2 itself has no Foundry deployment at launch.

That asymmetry is the practical detail buried under the benchmark slide. If you want MAI Code 2 inside Copilot, you have it today. If you want to call MAI Code 2 from your own application, your agent framework or your CI pipeline, there is no published endpoint to call: the only model in this pair with an API story is the smaller one, and it is a preview. Microsoft says third-party availability is coming; nothing is published yet and no date is given. Model weights are not distributed at all, and the licence is a set of product terms rather than an open-source licence.

## What it costs, and the date the price doubles

Copilot bills these models per token under usage-based billing, and both are on promotional pricing with a published end date.

• MAI Code 2 — $0.50 per million input tokens, $0.025 per million cached input, $2.50 per million output, until 17 May 2027; from that date, $1.00, $0.05 and $5.00

• MAI Code 2 Flash — $0.05, $0.005 and $0.25, until 17 May 2027; from that date, $0.10, $0.01 and $0.50

• Claude Opus 5.5, for scale — $4.00 input, $0.20 cached input and $20.00 output per million tokens

• GPT-6.1 Sol, for scale — $2.00 input, $0.10 cached input and $10.00 output per million tokens

Read the first and third lines together and the arithmetic gets interesting. At promotional rates MAI Code 2 costs 12.5% of Claude Opus 5.5's per-token price, and 25% of GPT-6.1 Sol's. Microsoft's headline is 5% and 40% of the *cost*, not of the price. Those headline ratios are cost per task across the four benchmarks, which folds the per-token rate together with however many tokens each model burns to finish a task — and the model card says every model was run at maximum reasoning effort, where token counts move most.

The gap between the two kinds of number is the thing to carry into a budget conversation. A 5% cost ratio sitting on top of a 12.5% price ratio implies MAI Code 2 finishes those tasks having spent roughly 40% of the tokens Claude Opus 5.5 spends. That is my arithmetic on Microsoft's published ratios, not a Microsoft statement, and it points the opposite way from the usual discount story: the saving is less about a cheap token than about a model that stops sooner. Blended input-and-output rates make it a rough figure at best, but the direction is worth knowing, because cheap tokens and a cheap task are not the same budget line, and only a task-level measurement tells you which one you are buying.

## Why this is a price launch, not a capability launch

Nothing in the launch material claims a new state of the art. Microsoft's own words are that these models are "explicitly not yet tuned for harder problems", and the model card adds that alignment is assessed as similar to GPT-5.6 Sol and less aligned than GPT-6. The strategy on display is the one the Ignite keynote stated outright: when a model is built to push the frontier of science, you pay for that capability on every routine bug fix, so build a model that is good at the routine work and price it accordingly.

Whether that works depends on a question Microsoft cannot answer alone. The comparison set is Microsoft's own harness against two competitors' published configurations, and the one independent component, FrontierCode, is a single benchmark out of four. A first-party pass rate on a first-party harness is a reasonable engineering signal and a weak purchasing one. The number to wait for is an independent row on a neutral leaderboard, run by someone with no stake in the answer and on a harness nobody at Microsoft configured.

## How to price the comparison yourself

Neither MAI Code 2 nor MAI Code 2 Flash is on OrcaRouter's catalogue today; the published route is GitHub Copilot, which means the comparison Microsoft is inviting you to make cannot currently be run on one key. The models it is measured against can. Claude Opus 5.5, GPT-6.1 Sol and Gemini 4 Carbon are all on OrcaRouter at provider list price with 0% markup, so the expensive side of this ledger is measurable today, with per-token rates that move the same day a vendor changes them. If the cheap side later becomes callable outside Copilot, the comparison becomes a routing decision rather than a spreadsheet exercise — and automatic failover is what makes a price-performance model safe to put in a production path before it has an independent track record.

What to watch, in order: an independent benchmark row for MAI Code 2; a Foundry deployment for the large model rather than only the Flash preview; and 17 May 2027, when the price on both models doubles. The last one is on a calendar, which makes it the only part of this launch you can plan around.

## Compared in this article1

Detected from this article · Benchmarks: Artificial Analysis · updated daily
