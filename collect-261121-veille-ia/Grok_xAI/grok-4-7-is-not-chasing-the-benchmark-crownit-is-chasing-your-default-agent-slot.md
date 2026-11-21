---
id: collect-261121-veille-ia/grok-xai/grok-4-7-is-not-chasing-the-benchmark-crownit-is-chasing-your-default-agent-slot
title: "Grok 4.7 Is Not Chasing the Benchmark Crown—It Is Chasing Your Default Agent Slot"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: []
keywords: ["grok", "model", "agent", "coding", "artificial", "benchmark", "analysis", "index"]
source: https://dev.to/beatapi/grok-47-is-not-chasing-the-benchmark-crown-it-is-chasing-your-default-agent-slot-5h73
source_anchor: ""
source_lines: [1, 170]
section: "Grok_xAI"
---
Most model launches are narrated as a race for one number: the highest composite score, the best coding benchmark, or the largest context window.

Grok 4.7 is more interesting when you stop asking whether it won the benchmark crown and ask a more operational question:

**Could this become the model your coding agent routes to by default?**

xAI released Grok 4.7 on September 21, 2026. Artificial Analysis scored it at **46** on its Intelligence Index at xhigh reasoning effort—two points above Grok 4.6 high, but below the leading score of 53. That does not look like a clean frontier-model victory.

The picture changes when the model is evaluated inside the product surfaces it was designed to inhabit. With Grok Build, Grok 4.7 scores **56** on the Artificial Analysis Coding Agent Index, up from **47** for Grok 4.6. It also arrives in the xAI API, Cursor, Grok Build, GitHub Copilot rollout, and third-party gateways.

This is not simply a stronger chatbot. It is a bid for distribution inside agent workflows.

*Source: Introducing Grok 4.7*

Grok 4.7 is a reasoning model for coding, agentic execution, and long-form knowledge work. The current API documentation lists:

- model ID: `grok-4.7`
- a **500K-token context window**
- text and image input, with text output
- 
`low` ,`medium` ,`high` , and`xhigh` reasoning effort
- both Responses and Chat Completions APIs
- a May 2026 knowledge cutoff in the API documentation

The launch materials emphasize a larger base model, longer reinforcement-learning runs, better self-verification, and training aimed at difficult multi-hour tasks. Those claims matter because the biggest independent gains are not in short factual questions. They appear in coding agents and work-product benchmarks where the model must keep a plan alive, operate tools, recover from errors, and deliver an artifact.

There is also a faster Grok 4.7 variant in Cursor and Grok Build. It uses the same model at higher serving speed and roughly twice the token price, but it is not currently offered through the public xAI API.

Artificial Analysis reports two seemingly different stories:

- 
**Intelligence Index: 46** , only two points above Grok 4.6 high
- 
**Coding Agent Index with Grok Build: 56** , nine points above Grok 4.6

That gap is not an inconvenience to explain away. It is the central lesson.

A coding agent is not just a model. It is a stack:

model + system prompt + tool schema + repository context + planning loop + error recovery + verification policy


Change the harness and you change the result. The model may receive better context, call tools more effectively, persist longer, or verify its own work before returning. A weaker harness can hide model capability; a strong first-party harness can amplify it.

*Source: Artificial Analysis, Benchmarking Grok 4.7*

This is why the phrase “Grok 4.7 scored X on Terminal-Bench” is incomplete.

At launch, three Grok 4.7 results for Terminal-Bench 4.0 appeared in circulation:

1. 
**38.0%** in the xAI model card, evaluated with the Grok Build harness and results attributed to Harbor
2. 
**33%** in the Artificial Analysis Coding Agent Index using the native Grok Build setup
3. approximately **26%** in Artificial Analysis’s standardized Intelligence Index harness

These are not interchangeable numbers. They answer different questions.

The 38% and 33% results ask how the model performs with its native agent system. The standardized result asks how the underlying model compares when scaffolding is controlled across providers. The native result is closer to “what can I get from this product stack?” The standardized result is closer to “what capability belongs to the model under a common protocol?”

Both are useful. Mixing them in the same leaderboard without naming the harness is not.

*Source: Grok 4.7 model card*

*Source: Artificial Analysis data reproduced by The Decoder*

For developers, the practical conclusion is simple: **benchmark the combination you will deploy**. If production uses Cursor, Grok Build, OpenCode, or your own tool loop, a bare-model score cannot predict the entire system.

The strongest gains are in long-horizon work.

On AA-Briefcase, which evaluates realistic professional deliverables, Grok 4.7 reaches **1657 Elo**, 111 points above Grok 4.6 high. On GDPval-AA it reaches **1695 Elo**, a 90-point improvement. These tasks require models to produce documents, spreadsheets, presentations, and other usable work products—not merely select an answer.

With Grok Build, all three components of the Coding Agent Index improve:

- DeepSWE: **65% → 73%**
- Terminal-Bench 4.0: **18% → 33%**
- SWE-Atlas-QnA: **58% → 63%**

Artificial Analysis also reports a lower hallucination rate on AA-Omniscience: **29%**, down from 34%, while raw accuracy remains roughly flat. That suggests better restraint in this evaluation, not a universal end to hallucinations.

*Source: Artificial Analysis*

These results point toward good candidate workloads:

- repository-scale implementation and debugging
- terminal tasks that require planning, tool use, and recovery
- research that must end in a structured deliverable
- document, spreadsheet, and presentation generation
- workflows with large context and explicit verification steps

They do not prove that Grok 4.7 is the best choice for every one of those jobs. They tell you where to spend evaluation time.

xAI’s standard list price is **$2 per million input tokens**, **$0.50 for cached input**, and **$6 per million output tokens** for requests below the long-context threshold. Above 200K tokens, the published input and output rates rise to $4 and $12.

Those rates look competitive. But price per token is only one term in the production bill.

Artificial Analysis reports that Grok 4.7 at xhigh used approximately **81K output tokens per Intelligence Index task**. The same article reports 36K for Grok 4.6 high; its chart shows 38K for Grok 4.6 xhigh. GPT-6 Astra max used about 27K.

The important denominator is therefore not “one million tokens.” It is **one accepted task**.

Use this model:

accepted-task cost = total spend across attempts ÷ number of outputs that pass your acceptance test


That includes reasoning tokens, retries, timeouts, tool calls, and failed outputs. A model with a lower list price can still be more expensive if it loops, overproduces, or needs more retries. A more expensive model can win if it finishes cleanly on the first attempt.

*Source: Artificial Analysis*

For teams that want to test the same public `grok-4.7` model through a compatible API, BeatAPI currently lists the corresponding token rows at **35% of xAI’s published list price**:

| Usage row | xAI list price | BeatAPI price | BeatAPI as a share of list | 
|---|---|---|---|
| Input, below 200K | $2.00 / 1M | $0.70 / 1M | 35% | 
| Cached input, below 200K | $0.50 / 1M | $0.175 / 1M | 35% | 
| Output, below 200K | $6.00 / 1M | $2.10 / 1M | 35% | 
| Input, 200K+ | $4.00 / 1M | $1.40 / 1M | 35% | 
| Output, 200K+ | $12.00 / 1M | $4.20 / 1M | 35% | 

That is a **65% reduction from the matching official list rows**, not a claim that every workload becomes 65% cheaper. The same accepted-task economics still apply: output length, retries, cache behavior, and success rate determine the final bill.

The comparison intentionally includes only rows published on both pricing pages; BeatAPI does not currently publish a separate 200K+ cached-input row. On xAI, once a prompt reaches 200K tokens, long-context rates apply to every token in that request.

BeatAPI exposes Grok 4.7 through OpenAI Responses, Chat Completions, Anthropic Messages, and Gemini-compatible request formats. If your stack already speaks one of those formats, the migration surface is mainly the API key and base URL.

Current pricing and request details: Grok 4.7 API on BeatAPI

Do not choose Grok 4.7 from a launch chart. Run a small, controlled evaluation against your current default model.

Use 20–50 real tasks from your own backlog. Include easy, median, and failure-prone examples. Remove secrets and customer data, but preserve the structure that makes the task difficult.

Keep the system prompt, tool definitions, repository snapshot, timeout, maximum turns, and acceptance tests constant. If you also want to compare native harnesses, run that as a separate experiment and label it clearly.

`xhigh` is useful for the hardest tasks, but it is not automatically the best production default. Test at least `high` and `xhigh`; for simpler routing, include `medium`. Measure whether additional reasoning changes the pass rate enough to justify its token and latency cost.

For multi-turn loops, also set a stable `prompt_cache_key` (or `x-grok-conv-id` with Chat Completions), as recommended by xAI. Otherwise, related requests may reach cache-cold servers and be billed at the full input rate. Test context compaction separately on long tool-heavy sessions.

For every attempt, record:

- accepted or rejected result
- input, cached-input, and output tokens
- wall-clock latency
- number of tool calls and retries
- failure category
- human review time
- final dollar cost

The winning model is not necessarily the one with the highest pass rate or the lowest token price. It is the model that reaches your required quality at the best combination of cost, latency, and operational predictability.

Grok 4.7 deserves an immediate evaluation if:

- your workload is dominated by long coding or terminal sessions
- you already use Cursor or want to test Grok Build
- your agent produces documents, analyses, spreadsheets, or presentations
- a 500K context window is operationally useful
- you can measure completion quality rather than relying on vibes

Keep your current model as the default if it already wins on accepted-task cost, if you need stronger standardized performance, or if predictable output length matters more than native-harness gains.

*Source: GitHub Changelog*

Grok 4.7 is not a universal benchmark winner. It is a serious attempt to become the default model inside coding and knowledge-work agents.

Its strongest evidence is not one composite score. It is the combination of better native-harness coding results, stronger long-horizon work products, broad developer distribution, and competitive API pricing. Its main caution is equally clear: xhigh reasoning can consume a great deal of output.

So evaluate the system, not the model name. Name the harness. Measure accepted outputs. Count retries and reasoning tokens. Then decide whether Grok 4.7 has earned the default slot in your stack.

If you want to run that comparison without paying the full xAI list rate, BeatAPI currently offers Grok 4.7 at 35% of the matching official token-price rows.

## Top comments (0)
