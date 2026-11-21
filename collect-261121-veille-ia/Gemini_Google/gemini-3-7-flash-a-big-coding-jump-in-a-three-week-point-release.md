---
id: collect-261121-veille-ia/gemini-google/gemini-3-7-flash-a-big-coding-jump-in-a-three-week-point-release
title: "Gemini 3.7 Flash: A Big Coding Jump in a Three-Week Point Release"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-08-13"]
keywords: ["flash", "gemini", "against", "coding", "moves", "claude", "sonnet", "benchmark"]
source: https://rits.shanghai.nyu.edu/ai/gemini-3-7-flash-three-week-coding-jump/
source_anchor: ""
source_lines: [1, 44]
section: "Gemini_Google"
---
**On August 13, 2026, Google DeepMind released Gemini 3.7 Flash** â just 23 days after Gemini 3.6 Flash, and built on the same architecture and training data. The model card is explicit that this is not a new pretraining run but “algorithmic improvements to its core reasoning foundation.” The gains are concentrated almost entirely in coding and agentic work: DeepSWE v1.1 jumps from 48.6% to 65.3%, and enterprise workflow automation nearly doubles. Elsewhere on the eval sheet, the needle barely moves â and in two places it moves backwards.

## What Actually Changed

Gemini 3.7 Flash keeps the specifications of its predecessor: a transformer-based mixture-of-experts architecture derived from Gemini 3 Pro, text, image, audio and video input, a 1M-token context window, and a 64K-token maximum output. Knowledge cutoff is March 2026, though Google notes some domains reflect information only through January 2025. Thinking budget remains configurable, trading quality against cost and latency.

What changed is post-training. Because the pretrained base is shared with 3.6 Flash, the delta between the two models is an unusually clean read on what three weeks of reasoning and post-training work buys you â and where it doesn’t reach.

The coding numbers are the headline. FrontierCode 1.1 Main, which scores production-code quality rather than puzzle-solving, goes from 34.4% to 43.6% â ahead of Claude Sonnet 5 (42.7%) and GPT-5.6 Terra (41.3%). DeepSWE v1.1, a long-horizon software-engineering benchmark, climbs from 48.6% to 65.3%. Code Arena Elo for web development rises from 1538 to 1588, the top score in Google’s comparison set. AutomationBench, Google’s private enterprise-workflow set, goes from 17.0% to 30.4%. Terminal-bench 2.1 moves from 78.0% to 85.8%.

Document comprehension moved too. GDP.pdf, an expert PDF-comprehension benchmark aimed at finance, law and biosciences, goes from 22.0% to 34.0%, the best score in Google’s table against 28.0% for Claude Sonnet 5 and 24.7% for GPT-5.6 Terra. Harvey LAB-AA (complex legal workflows) rises from 85.1% to 90.7%, and long-context retrieval at 128k improves from 91.8% to 97.0%.

## Where the Gains Stop

Read the full table rather than the highlights and a narrower picture emerges. On the Artificial Analysis Intelligence Index â a composite score â 3.7 Flash moves from 52 to 56, which puts it above Claude Sonnet 5 (55) but below both GPT-5.6 Terra and Muse Spark 1.2 (57 each). It is not the most capable model in its own comparison set; it is the most capable model at its price point in that set.

Several categories show the improvement is targeted rather than general. On CharXiv Reasoning, which tests information synthesis from complex charts, 3.7 Flash scores slightly *below* its predecessor in both configurations â 84.5% against 85.2% without tools, and 88.7% against 89.4% with them. On GDPVal-AA v2, an Elo benchmark for knowledge work, it improves from 1422 to 1525 but still finishes last in the five-model comparison, behind Muse Spark 1.2 at 1628 and Claude Sonnet 5 at 1598. Agent’s Last Exam moves only from 24.2% to 26.3%, well behind Claude Sonnet 5’s 33.3%.

And on the hardest agentic tests, GPT-5.6 Terra still leads: DeepSWE v1.1 at 69.6% against 65.3%, Terminal-bench 2.1 at 87.4% against 85.8%, OSWorld-2.0 at 50.2% against 47.9%. Terminal-bench 3.0, a newer and much harder general-agent benchmark, is worth noting for the absolute numbers rather than the ranking â 3.7 Flash scores 14.9% and the leading model in the set scores 20.8%. That gap between the coding benchmarks and the general-agent ones is the more interesting result on the sheet.

## Pricing

Gemini 3.7 Flash launches at an introductory $0.75 per million input tokens and $3.75 per million output tokens. The framing in Google’s announcement â “half the original 3.6 Flash cost” â is worth reading against the footnote in Google’s own benchmark table: 3.6 Flash currently carries the same introductory rates, and the promotion for both models expires on December 31, 2026, after which the list price is $1.50 input and $7.50 output. So the comparison is against 3.6 Flash’s original list price, not against what a developer is paying for 3.6 Flash today. Teams budgeting past the new year should plan on the standard rate.

At those rates the model sits well inside the cost-performance frontier on DeepSWE. The comparison that matters for agent builders is against Claude Sonnet 5 at $2.00/$10.00 and GPT-5.6 Terra at $2.00/$12.00 â roughly three times the output cost for scores that, on the coding benchmarks specifically, are at or below Gemini’s.

The model is generally available now through the Gemini API, Google AI Studio, Android Studio and Google Antigravity, in the Gemini Enterprise Agent Platform and app for enterprise customers, and in Gemini Spark for AI Pro and Ultra subscribers across 160-plus countries.

## What This Means

The most useful thing about this release is what it isolates. Same base model, same training data, three weeks apart, one variable changed â and coding scores moved by 8 to 17 points while chart reasoning slid slightly and general knowledge work stayed mid-pack. That is a fairly direct demonstration that post-training on a fixed base is now a real lever on agentic capability, and also that it is a lever with a specific reach.

For anyone running coding agents at volume, the practical read is straightforward: this is currently the strongest production-code and web-development model in Google’s comparison set, at roughly a third of the output cost of its nearest competitors, with the caveat that the price advantage is contractual and expires at year end. For work that is agentic but not primarily code â desktop automation, long-horizon general tasks, knowledge work â 3.7 Flash closes some distance without taking the lead, and the single-digit Terminal-bench 3.0 scores across the whole field are a reminder that general-purpose agents remain a substantially unsolved problem regardless of vendor.

The cadence is its own signal. Three point releases of the Flash tier since May, each landing inside a month of the last, suggests Google is treating the workhorse tier as something closer to continuously deployed software than a periodic model launch. For teams pinning a model version in production, that is a maintenance question as much as a capability one.

*This post was drafted with AI assistance and reviewed by RITS staff.*

## Sources

- Gemini 3.7 Flash: our most intelligent workhorse model â Google, August 13, 2026
- Gemini 3.7 Flash Model Card â Google DeepMind
- Google launches Gemini 3.7 Flash for coding, AI agent projects â SiliconANGLE, August 13, 2026
- Gemini 3.7 Flash â API Provider Performance Benchmarking & Price Analysis â Artificial Analysis
