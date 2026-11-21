---
id: collect-261121-veille-ia/claude-anthropic/claude-fable-5-1-and-mythos-5-1-when-ai-started-doing-real-science
title: "Claude Fable 5.1 and Mythos 5.1: When AI Started Doing Real Science"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["fable", "mythos", "research", "claude", "anthropic", "just", "science", "protein"]
source: https://thecoelab.com/blog/claude-fable-mythos-5-1-anthropic-ai-scientific-research
source_anchor: ""
source_lines: [1, 59]
section: "Claude_Anthropic"
---
Anthropic's new Claude Fable 5.1 and Mythos 5.1 models aren't just better at coding — they're designing proteins, mapping Venus, and optimizing GPU kernels for biologists. The gap between AI as a chatbot and AI as a research collaborator is closing.

Anthropic just dropped Claude Fable 5.1 and Claude Mythos 5.1, and the AI world is paying attention. With 1,250 upvotes on Hacker News and nearly 1,200 comments in 16 hours, this isn't just another model release — it's a signal that AI is crossing from code assistant into something that might actually contribute to scientific discovery.

The two models are technically the same underlying system, but with different safeguard levels. Fable 5.1 is generally available. Mythos 5.1 is restricted to trusted access programs, specifically designed for cybersecurity and life sciences research where tighter safety guardrails matter.

## A New Performance Frontier

The benchmark numbers are striking. On Terminal-Bench-Science 0.1, a benchmark for agentic scientific research, Fable 5.1 scores 52.6% — more than double Fable 5's 24.7% and nearly 23 points ahead of GPT-5.6 Sol. On agentic coding (Terminal-Bench 4.0), Mythos 5.1 hits 60.9%, beating Fable 5.1's 55.8% and leaving both Opus 5 and GPT-5.6 Sol behind.

On Humanity's Last Exam, the multi-disciplinary reasoning benchmark, Fable 5.1 scores 60.9% without tools and 65% with tools — both improvements over its predecessor. The AutomationBench score for business workflows jumped from 17.1% to 31.4%.

But benchmarks only tell part of the story. Investment firm Millennium reported that Fable 5.1 found the cause of a rare crash in their internal systems that none of their engineers — and no other AI model — had been able to explain after several years of trying. Jane Street's Head of Quantitative Research noted that the model achieves state-of-the-art on trading intuition while remaining readable over long, multi-step tasks.

## AI That Does Science, Not Just Talk About It

The most compelling part of this announcement isn't the coding benchmarks. It's the scientific research capabilities. Anthropic tested Mythos 5.1 across multiple scientific domains, and the results suggest AI is approaching the point where it can contribute meaningfully to real research.

## Protein Design

Mythos 5.1 was given access to open-source protein design and folding tools and asked to design high-affinity binders — the molecules that drugs use to bind to targets in the body. Its designs were sent to two external organizations for experimental validation. The results were extraordinary:

- Binding affinities 10x higher than the best designs submitted to Adaptyv Bio's protein design competitions
- Hit rate of nearly 50% across 12 targets (typical rates are 10-15% in protein design today)
- The strongest hit rates Anthropic has measured to date

If you're not a biologist, here's why this matters: designing a high-affinity binder is the first step in developing many modern medicines. A 50% hit rate means half of the AI's designs actually work — which could dramatically accelerate the early stages of drug development.

## Mapping Venus From 30-Year-Old Data

Fable 5.1 trained a neural network to create a new high-resolution elevation map of a third of Venus, using radar images from NASA's Magellan mission — which orbited the planet over 30 years ago. The new map reveals details down to 2-3 kilometers instead of the previous 10-20, and shows heights up to 25% more accurately.

Anthropic released this map under a Creative Commons license to help NASA's upcoming VERITAS and ESA's EnVision missions decide which geological features to target. An AI model just contributed to actual planetary science planning.

## GPU Optimization for Computational Biology

Mythos 5.1 took a different approach to helping biologists: it wrote custom GPU kernels and cached intermediate results for seven open-source deep learning models used in protein and genomics research. The result: up to 2.5x speedup with identical outputs, cutting estimated GPU costs by 30-60%.

This kind of optimization normally requires a team of performance engineers working for weeks. Mythos 5.1 did it in days using only publicly available source code. Anthropic plans to open-source these optimizations, which could be a meaningful cost reduction for academic labs that run these models thousands of times per experiment.

## Price, Privacy, and Safeguards

Beyond raw capability, Anthropic addressed three major customer complaints:

- Price: Fable 5.1 costs ~25% less than Fable 5 for typical workloads, with savings up to 45% for highly agentic work, thanks to cheaper cache reads
- Data retention: New Enterprise Frontier Safeguards (EFS) give customers complete privacy with zero data retention while still preventing adversarial use — data stays in customer-controlled infrastructure
- Safeguards: 60% fewer false positives in cybersecurity, and Fable 5.1 can now discover software vulnerabilities (but not develop exploits for them)

The biology access program for Mythos 5.1 was developed in partnership with the US government, with enrollment for scientists opening soon. This is a notable detail — it suggests Anthropic is treating advanced biology capabilities with the seriousness they deserve, rather than just shipping them openly.

## What This Means

There's a pattern emerging across AI lab announcements in 2026: the models are getting better at the boring, hard parts of real work. Not writing essays or generating images — but debugging obscure crashes, optimizing GPU kernels, designing drug candidates, and processing 30-year-old planetary data. The gap between AI as a conversational tool and AI as a research collaborator is closing.

Fable 5.1 won't replace scientists. But it might give a biology lab the equivalent of a dedicated performance engineering team. It might help a quant firm find a bug that humans couldn't. It might help a planetary scientist decide where to point a spacecraft. And at 25% lower cost with real data privacy guarantees, it's becoming the kind of tool that institutions can actually deploy without compromising on security or budget.

The dual-model approach is also worth noting. By separating Fable (general availability) from Mythos (restricted access for sensitive domains), Anthropic is threading a needle that other labs have struggled with: how do you give researchers powerful tools without also handing them to bad actors? It's not a perfect solution, but it's a more nuanced approach than either full openness or total restriction.

The HN community's response — 1,250 upvotes and counting — suggests that developers and researchers are paying attention. When the top comment on a model release is about protein binding affinities rather than token pricing, you know the conversation is shifting. AI is starting to do real work, and people are starting to notice.
