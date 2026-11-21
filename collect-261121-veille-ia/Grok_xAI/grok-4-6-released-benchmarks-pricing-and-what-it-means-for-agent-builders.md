---
id: collect-261121-veille-ia/grok-xai/grok-4-6-released-benchmarks-pricing-and-what-it-means-for-agent-builders
title: "Grok 4.6 Released: Benchmarks, Pricing, and What It Means for Agent Builders"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: []
keywords: ["grok", "fable", "gpt-5", "work", "behind", "ahead", "released", "model"]
source: http://dev.to/jamilxt/grok-46-released-benchmarks-pricing-and-what-it-means-for-agent-builders-28ob
source_anchor: ""
source_lines: [1, 52]
section: "Grok_xAI"
---
On August 12, 2026, xAI released Grok 4.6, the successor to Grok 4.5 that shipped in July. The positioning is different from the last release. This is not pitched as a raw intelligence jump. It is a model built for long-running agents and ambitious interactive and visual work: researching a topic across many steps, working through a codebase, or turning a rough product idea into a polished first version.

The headline claim is measured. xAI says Grok 4.6 matches GPT-5.6 Sol on the Artificial Analysis Intelligence Index, a composite of nine benchmarks. Across the rest of the published evals it trades leads with GPT-5.6 Sol and Anthropic's Fable 5, winning some and losing others. Pricing starts at $2 per million input tokens and $6 per million output tokens, with a faster variant at double that.

I build AI agents with Spring AI for a living, so the agentic framing is what I read first. Here is what the release actually contains, where the numbers hold up, and what it signals for the frontier race.

The official announcement is short on scale and long on training. It never states a parameter count. Earlier reports disagreed: some pointed to the same 1.5T V9 base as Grok 4.5 with heavy post-training, others to a larger 2T model. Either way, xAI's framing is that this release is about the training recipe, not the model size.

What the company did describe:

- A longer supplemental training run than Grok 4.5, with curated model-generated data for reasoning and advanced technical concepts, high-quality engineering data, and an improved optimizer and training recipe.
- A supervised fine-tuning stage where Grok 4.5 itself regenerated the SFT trajectories across reasoning efforts, agent harnesses, and domains like STEM, software engineering, and knowledge work. Problematic traces were filtered out with model-based checks.
- Reinforcement learning across a wide range of agentic tasks: general coding, knowledge work, and domain-specific environments for kernel optimization, web development, and computer-aided design.

The behavioral claim is the interesting part. On longer trajectories, xAI says it started seeing more self-testing and verification, with the model checking its own work before moving on. For visual and interactive projects, Grok 4.6 produces stronger first passes than Grok 4.5, establishing the structure and visual language of an application in one pass rather than through several rounds.

xAI published ten evaluations comparing Grok 4.6 with Grok 4.5, GPT-5.6 Sol Max, and Fable 5 Max. Competitor figures come from published system cards and leaderboards, and all of it is self-reported, so treat the numbers as directional.

The scorecard:

- AA Intelligence Index: 61, tied with GPT-5.6 Sol Max (61), one point behind Fable 5 Max (62), five ahead of Grok 4.5 High (56).
- GDPVal-AA v2: 1753, the highest of the four.
- CursorBench v3.2: 69.9%, behind Fable 5 Max (70.5%) and ahead of GPT-5.6 Sol Max (67.2%).
- FrontierCode v1.1 (Extended): 61.3%, ahead of GPT-5.6 Sol Max (60.6%), behind Fable 5 Max (63.6%).
- DeepSWE v1.1: 65.9%, a clear gap to GPT-5.6 Sol Max (73%) and Fable 5 Max (70%). This is the weakest relative showing.
- Terminal-Bench v3.0: 26%, well behind both rivals (34.6% and 34.1%).
- APEX-Agents: 57.5%, between the two.
- APEX-SWE: 56.4%, behind Fable 5 Max (58.8%).
- AA-Briefcase: 1577, narrowly ahead of Fable 5 Max (1574).
- Harvey LAB (Vals): 15.8%, comfortably ahead of both rivals.

Grok 4.6 beats Grok 4.5 High on every benchmark by a wide margin. Against the top tier the pattern is mixed: wins on GDPVal-AA, AA-Briefcase, and Harvey LAB, a tie on the composite index, and clear losses on DeepSWE and Terminal-Bench. That last one matters. Terminal-Bench measures real terminal work, and 26% against 34% is not a rounding error.

Grok 4.6 starts at $2 per million input tokens and $6 per million output tokens. A faster variant costs double. xAI frames that as roughly half of what comparable frontier models charge, and the pricing is the part of the release that needs no benchmark interpretation.

It is live today in Cursor and Grok Build, and available through the API at console.x.ai with OpenRouter, Vercel, and Cloudflare among the partners. For the first week, xAI is offering double the included usage in Grok Build and Cursor.

Grok 4.5 shipped on July 16, 2026, and Musk confirmed 4.6 was in the pipeline two days later. His public timeline had Grok 4.6 in two weeks and Grok 4.7 in four, and a leak in early August pointed to 4.7 carrying a much larger 2.1T architecture. If that holds, 4.6 is the post-training release and 4.7 is the scale jump. xAI is running two experiments in parallel: how much you can extract from an existing base, and how much a bigger base adds on top.

The benchmark mix xAI chose to publish is itself a statement. DeepSWE, CursorBench, Terminal-Bench, APEX, AA-Briefcase: these are all agentic and coding evals, not the knowledge and math benchmarks that dominated releases a year ago. The frontier conversation has moved to long-horizon agent work, and every lab is now optimizing for it.

For anyone shipping agent features, two things in this release are worth acting on.

First, the self-verification behavior. A model that checks its own output on long trajectories changes what your harness has to do. Instead of wrapping every tool call in manual validation, you can let the model catch its own mistakes and reserve your checks for the seams that matter. This is the pattern I care about in my own Spring AI agents: the loop of act, observe, verify is where agents either earn their keep or fall apart.

Second, the price. Long agent runs burn tokens in ways that short prompts do not. Every retry and every failed tool call is another request. At $2 and $6 per million tokens, with a fast path at double the price, the economics of leaving an agent on a long task get better than the frontier norm.

The caveats are real. The benchmarks are self-reported, Terminal-Bench shows a genuine weakness, and the model is live everywhere at once: Cursor, Grok Build, the API, and partner platforms. The agentic scores need to reproduce in your own scaffold before they matter. But for a release explicitly about long-running agents, the combination of strong agentic evals, half-price tokens, and a training story built on verification is a concrete step forward.

The official announcement has the full benchmark table, and OfficeChai's breakdown walks through the same numbers. If you are building agents, Grok 4.6 is worth a test run this week, especially while the doubled usage offer is active. Which agent workload would you point it at first?

## Top comments (0)
