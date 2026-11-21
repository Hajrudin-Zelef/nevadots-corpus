---
id: collect-261121-veille-ia/muse/meta-muse-ember-a-0-10-m-token-reasoning-model-built-for-the-agent-subtask-tier
title: "Meta Muse Ember: a $0.10/M-token reasoning model built for the agent subtask tier"
domain: muse
role: reference
task: article
actors: ["Meta"]
dates: []
keywords: ["meta", "muse", "ember", "model", "tokens", "reasoning", "million", "haiku"]
source: https://www.harelasaf.com/articles/meta-muse-ember-agent-subtask-tier-e4b1
source_anchor: ""
source_lines: [1, 92]
section: "Muse"
---
# Meta Muse Ember: a $0.10/M-token reasoning model built for the agent subtask tier

Meta released Muse Ember on November 9: a small reasoning model for tool calls, coding subtasks and document parsing, priced at $0.10 per million input tokens with a 1M context window. Meta claims Haiku 5.5-level intelligence at a third of the cost. The numbers, the caveats, and when a cheap subagent model makes sense.

Meta released Muse Ember on November 9, and it completes a product category that has been forming all year: a dedicated cheap tier for the small, structured steps agents repeat all day. Muse Ember is a small reasoning model for tool calls, coding subtasks, routing, extraction and long document parsing, priced at $0.10 per million input tokens and $0.50 per million output tokens, with a 1M-token context window and up to 512K output tokens. Meta claims it matches Claude Haiku 5.5's best score at roughly a third of the cost per task and beats every GPT-6 Luna reasoning setting. It is available today on the Meta Model API as muse-ember-1.0, on Muse Code, and on Google Cloud and Oracle Cloud. The benchmarks are a mix of third-party Artificial Analysis runs and Meta internal evaluations, and the harness details matter, so read the fine print before re-routing your agent fleet.

## What Meta Muse Ember is

Per Meta's announcement:

- A small reasoning model aimed at the work agents repeat: tool calls, coding subtasks, routing, extraction, and long document parsing. Meta explicitly positions it as the volume tier under Muse Wonder: "Muse Wonder plans, Muse Ember handles the volume."
- Natively multimodal: text, images, audio and video in one model, so agents can read screenshots, charts and scanned documents without a separate vision model.
- 1M-token context window with up to 512K output tokens, and support for the Responses API.
- The first model Meta trained using Muse Wonder Full, rounding out a portfolio of Muse Wonder (hardest problems), Muse Spark (coding and agentic workflows) and Muse Ember (repeatable tasks at scale).

## Pricing and availability

- $0.10 per million input tokens, $0.004 per million cached input tokens, and $0.50 per million output tokens.
- The catch: those prices apply to prompts up to 200K tokens. Beyond 200K, pricing triples to $0.30 per million input, $0.012 cached input, and $1.50 per million output. If your plan is to stuff entire repositories into that 1M window, the cheap tier pricing does not apply to you.
- Meta estimates about $0.14 per Artificial Analysis task, against $2.39 for Muse Spark 1.3.
- Available now on the Meta Model API as muse-ember-1.0 and in Muse Code. Meta also says Muse Ember, Muse Spark and Muse Wonder are now on Google Cloud and Oracle Cloud.

## The benchmark picture

All figures below are from Meta's post. Some come from Artificial Analysis (third party, published November 9), and several are Meta internal evaluations. Treat both as vendor-reported until independent replication appears.

- Cost-efficiency: on the Artificial Analysis Intelligence Index, Meta says no model scores higher for less per task. Ember matches Haiku 5.5's best score (36) at roughly a third of the cost, and beats all GPT-6 Luna settings.
- Agent work: AutomationBench-AA 37.8% versus 13.9% for Haiku and 20.2% for Luna. Agents' Last Exam 61.6% versus 57.0% and 57.1% - but that run is Meta internal, Linux subset.
- Coding: Terminal-Bench 4.0 at 40.9% versus 39.2% for Haiku and 16.4% for Luna, with a real caveat: each model ran in its best published harness (mini-swe-agent for Ember, Claude Code for Haiku, Codex for Luna), so this measures model-plus-harness, not the model alone. Coding Agent Index-AA 46 versus 37 and 31.
- The honest counterpoints in Meta's own table: on FrontierCode 2.0-AA (real engineering tasks judged on mergeability), Haiku 5.5 wins at 34.9% versus Ember's 27.2%. On GDP.pdf, Ember ties Luna at 22.8%. Ember is not uniformly better than Haiku; it is cheaper at comparable average quality.
- Partner reports: Cloudflare says Ember ran up to 30% faster at the tail than the comparable model they tested; You.com reports p95 latency of 2.2 seconds on its coding quests, 4x to 13x faster than leading open-weight models, with 36 to 48% fewer output tokens.

## Why the subagent tier matters

Agentic systems spend most of their tokens on many small structured steps, not on deep reasoning. Until now, the usual pattern was to run one frontier model for everything and eat the cost, or self-host an open-weight model and eat the operational burden. A hosted model at $0.10 per million input tokens with competitive agent benchmarks changes the routing math: the planner can stay expensive while extraction, file reads, test runs and targeted fixes go to the cheap tier. Cline's founder makes exactly this point in Meta's announcement, describing Ember as purpose-built for subagent work in a harness. The Fable 5.2 launch this week points the same direction from the top end: Anthropic describes Fable acting as an Orchestrator that delegates to smaller models. The industry is converging on explicit model hierarchies inside one agent system.

## What I would check

My untested suggestions before adopting:

- Benchmark Ember on your own agent traces, especially the tasks you repeat most: extraction, test-running, file summarization. The aggregate indices hide per-task variance.
- Watch the 200K-token pricing cliff. Long-context calls cost 3x, so chunking strategy matters more than the headline price.
- Test the cached-input rate ($0.004 per million) with stable prefixes; at that price, prompt caching discipline pays for itself quickly.
- Reproduce the latency claims on your workload. Tail latency is what users feel, and partner quotes are curated.
- Keep Haiku 5.5 in the mix for mergeable-code tasks; Meta's own table shows Haiku ahead on FrontierCode 2.0-AA.

## What I would not conclude

Do not conclude Ember beats Haiku 5.5 overall: on the headline Artificial Analysis index they tie at 36, and Haiku wins the mergeability benchmark in Meta's own table. Do not treat the internal evaluations (Agents' Last Exam, PSI, OSWorld) as independent evidence. Do not assume the Terminal-Bench lead transfers to your harness, since each model was tested in a different one. And do not plan around open weights: Meta's post describes hosted API access only, with no weights release mentioned.

## Frequently asked questions

### What is Meta Muse Ember?

Meta's smallest and most efficient model, released November 9, designed for the repetitive structured steps agents perform: tool calls, coding subtasks, routing, extraction, and long document parsing. It is natively multimodal across text, images, audio and video.

### How much does Muse Ember cost?

$0.10 per million input tokens, $0.004 per million cached input tokens, and $0.50 per million output tokens, for prompts up to 200K tokens. Longer prompts cost $0.30 per million input and $1.50 per million output.

### Where can I use it?

On the Meta Model API as muse-ember-1.0, in Muse Code, and on Google Cloud and Oracle Cloud. It supports the Responses API and offers a 1M-token context window with up to 512K output tokens.

### Is Muse Ember better than Claude Haiku 5.5?

On Meta's numbers, they tie on the Artificial Analysis Intelligence Index at 36, while Ember costs roughly a third as much per task. Ember leads on most agent and coding benchmarks Meta published, but Haiku wins FrontierCode 2.0-AA, the benchmark closest to real mergeable engineering work. Ember's claim is efficiency, not superiority.

### Is Muse Ember open source?

Meta's announcement describes hosted access through its API and cloud partners only. It does not mention a weights release.

### Are the benchmarks independent?

Partially. The Artificial Analysis figures are third-party results published November 9, but Agents' Last Exam, the Personal Superintelligence Index and OSWorld runs are Meta internal evaluations, and Terminal-Bench 4.0 used a different harness per model.

### What is the 200K pricing cliff?

The headline prices cover prompts up to 200K tokens. Above that, input tokens triple to $0.30 per million and output tokens triple to $1.50 per million. If you plan to use the full 1M context window regularly, budget at the higher tier.

## Sources

- Introducing Our Smallest, Most Efficient Model: Muse Ember | Meta Research - November 9, 2026, read November 10, 2026. The announcement, pricing, availability, benchmark tables, harness footnotes, safety evaluation summary, and partner quotes.
- TLDR AI newsletter, November 10, 2026 - the tip that surfaced the launch and the Claude Haiku 5.5 comparison framing.

Not read: the Muse Ember evaluation report referenced in the post, the Artificial Analysis leaderboard itself, and Meta's Superintelligence Scaling Framework document. All benchmark figures are vendor-reported or vendor-selected; I have not run the model.

## Related reading

- Claude Fable 5.2 and Mythos 5.2: Anthropic's top models get cheaper, 30% faster, and a 2.5x fast modeAnthropic released Claude Fable 5.2 and Mythos 5.2 on November 9: same underlying model, $8/$40 per million tokens, an optional 2.5x fast mode, and benchmarks that top Opus 5.5 at roughly half the cost per task. What launched, when to use it, and the catches.
- Google TPU 8tx and 8i Max: confidential inference and million-token context get dedicated systemsGoogle announced TPU 8tx and TPU 8i Max on November 9, completing four eighth-generation TPU systems in one year: 8t for training, 8i for reasoning, 8tx for hardware-level confidential inference, and 8i Max for massive context windows. What each system does, the open software stack around them, and what is still unannounced.
- Anthropic's agent memory findings: one wrong note can loop your agent, and 'dreaming' fixes itAnthropic published how it builds memory for Claude agents: a four-layer hierarchy, offline 'dreaming' passes that correct notes against transcripts, and evals showing a single wrong note trapped an agent in a loop. The practical rules for anyone building agent memory.
