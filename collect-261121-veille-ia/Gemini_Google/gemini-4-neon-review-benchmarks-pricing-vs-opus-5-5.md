---
id: collect-261121-veille-ia/gemini-google/gemini-4-neon-review-benchmarks-pricing-vs-opus-5-5
title: "Gemini 4 Neon Review: Benchmarks, Pricing & vs Opus 5.5"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-11-18"]
keywords: ["neon", "gemini", "opus", "pricing", "carbon", "tier", "benchmarks", "hydrogen"]
source: https://www.alekseialeinikov.com/en/blog/topics/ai/gemini-4-neon-vs-claude-opus-5-5-hydrogen-carbon
source_anchor: ""
source_lines: [1, 107]
section: "Gemini_Google"
---
Google released `gemini-4-neon` on **November 18, 2026**, just six days after Gemini 4 Hydrogen and right in the middle of a crowded frontier week alongside GPT-6.2 Astra and Sol. On paper, Google’s periodic-table naming (`Hydrogen`, `Neon`, `Carbon`, `Argon`) tells you nothing about model size or tier. In practice, **Neon is the release that forces teams to rethink their default coding and agent model**.

At **$3.00 / $15.00 per million tokens** (25% cheaper than Claude Opus 5.5 and Gemini 4 Carbon), Neon scores **61** on the Artificial Analysis Intelligence Index at `xhigh` thinking, lands at **58.6–59** on the Coding Agent Index, and serves at roughly **~127 tokens/sec** on OpenRouter — nearly 2.7× faster than Carbon’s ~46 tokens/sec.

**The short verdict.** Gemini 4 Neon breaks the old trade-off between **Hydrogen’s speed** and **Opus 5.5 / Carbon’s intelligence**. For daily agentic coding, PR reviews and multi-step tool use, Neon is the strongest default choice in Google’s lineup and a genuine 25%-cheaper rival to Opus 5.5. Keep **Hydrogen** for high-volume subagents, and keep **Opus 5.5** or **Carbon** only for open-ended architecture and deepest long-horizon research.


## Specs, Pricing and the Periodic-Table Lineup

Developers on Hacker News immediately pointed out the biggest UX problem with Gemini 4: **you cannot tell from the name `Neon` whether it sits above or below `Carbon`**. Here is the actual hierarchy as of November 19, 2026, compiled from Google AI Studio pricing, Anthropic’s model overview and OpenRouter:

| Specs, Pricing and the Periodic-Table Lineup |  |  |  |  |  | 
|---|---|---|---|---|---|
| Metric | **Gemini 4 Hydrogen** | **Gemini 4 Neon** | **Gemini 4 Carbon** | **Claude Opus 5.5** | **GPT-6.2 Sol** | 
|---|---|---|---|---|---|
| API model ID | `gemini-4-hydrogen` | `gemini-4-neon` | `gemini-4-carbon` | `claude-opus-5-5` | `gpt-6.2-sol` | 
| Input / output (1M) | **$1.50 / $7.50** | **$3.00 / $15.00** | $4.00 / $20.00 | $4.00 / $20.00 | ~$2.00 / $10.00 | 
| Flex / batch tier | Available | **$1.50 / $7.50** | Available | Batch discount | Batch discount | 
| Cache read (1M) | **$0.075** | **$0.15** (~95% off) | $0.20 | $0.20 | Cached tier | 
| Context window | 1,048,576 | **1,048,576** | 1,048,576 | 1,000,000 | 400K–1M | 
| Max output | 65K (1M cont.) | **65,536 (1M cont.)** | 65K (1M cont.) | 128K | 128K | 
| Thinking control | `low` →`xhigh` | **`low` → `xhigh`** (`high` default) | `low` →`xhigh` | Adaptive | Reasoning effort | 
| Observed speed (OpenRouter) | Very fast | **~127 tok/s** | ~46 tok/s | Medium | Fast | 
| Native modalities | Text, img, audio, video, PDF | **Text, img, audio, video, PDF, Office** | Multimodal | Text, image | Text, image | 
| Free API tier | Limited | **None (paid only)** | None | None | None | 

Three architectural details stand out in Neon’s spec sheet:

1. **Unified 1,048,576-token multimodal window:** Input, thinking blocks and output share one flat 1M window, with native ingestion of PDFs and Office/OpenDocument files — making it practical to drop entire RFCs, compliance spreadsheets and repository snapshots into a single prompt.
2. **95% context-cache discount ($0.15/1M):** For coding agents that replay an 80K–200K repository prefix on every tool call, cache-read pricing matters far more than headline input pricing. At`$0.15/1M` , Neon undercuts Opus 5.5 (`$0.20/1M` ) by 25% on every cached turn.
3. **Flex tier cuts rates in half ($1.50 / $7.50):** Non-interactive pipelines — nightly test generation, security triage, doc indexing, PR batch audits — get Neon’s intelligence at Hydrogen’s standard price.

## Independent Benchmarks: Where Neon Wins (and Where It Doesn’t)

Unlike announcements that rely purely on internal vendor charts, Gemini 4 Neon landed on independent trackers within 24 hours:

| Independent Benchmarks: Where Neon Wins (and Where It Doesn’t) |  |  |  |  |  | 
|---|---|---|---|---|---|
| Benchmark / Tracker | **Gemini 4 Neon** | **Claude Opus 5.5** | **Gemini 4 Carbon** | **GPT-6.2 Sol** | **Gemini 4 Hydrogen** | 
|---|---|---|---|---|---|
| **AA Intelligence Index** (`xhigh` / max) | **61** | 59 | ~60 | 57 | 54 ( `xhigh` ) / 51 (`high` ) | 
| **AA Coding Agent Index** (Antigravity / top harness) | **58.6–59** | 58 | — | — | — | 
| **Vals Index v2.2** | **70.9%** | Slightly behind | Behind Neon | Behind Neon | Lower | 
| **Terminal-Bench v4.1** (cheating-adjusted) | **65.2%** | Top tier | Top tier | Competitive | Strong flash-class | 
| **Agent Arena** | **#2 overall** | Top tier | Top tier | — | — | 
| **Throughput (OpenRouter)** | **~127 tok/s** | Lower | ~46 tok/s | Fast | Very fast | 

Two important nuances keep the benchmark numbers honest:

1. **The Carbon paradox (aggregate score vs long-horizon depth):** Even though Neon edges out Gemini 4 Carbon on the aggregate Artificial Analysis Intelligence Index (`61` vs`~60` ) while costing**~32% less per task** , Carbon still leads on long-horizon research evaluations such as`CritPt` . Neon is tuned to climb the Pareto frontier fast; Carbon is tuned to keep grinding through deep, multi-hour research chains.
2. **The harness effect is massive:** In bare, text-only minimal SWE harnesses, Neon scores dramatically lower (~12.8%) than inside a full agentic harness like Google Antigravity (~39%+ on the same style of software engineering suite and 58.6–59 on the Coding Agent Index). That is not a flaw unique to Neon — it confirms that in late 2026,**tool definitions, terminal feedback loops and context management matter almost as much as the base weights** .

## Real 10-Turn Agent Session Math

Headline per-million rates hide how coding agents actually spend tokens. Let’s apply the exact same transparent formula we used in our Hydrogen vs Opus 5.5 breakdown:

- **Session shape:** 10 agent turns
- **Per turn:**`80,000` cached prefix tokens +`4,000` fresh input tokens +`6,000` output tokens (including thinking tokens)
- **Session totals:**`800K` cache read (`0.8M` ) +`40K` fresh input (`0.04M` ) +`60K` output (`0.06M` )

| Real 10-Turn Agent Session Math |  |  |  |  |  | 
|---|---|---|---|---|---|
| Model | Cache Read ( `0.8M` ) | Fresh Input ( `0.04M` ) | Output ( `0.06M` ) | **Total 10-Turn Cost** | **vs Opus 5.5** | 
|---|---|---|---|---|---|
| **Gemini 4 Hydrogen** | $0.06 | $0.06 | $0.45 | **$0.57** | **−62.5%** | 
| **Gemini 4 Neon (Flex)** | ~$0.06 | $0.06 | $0.45 | **~$0.57** | **−62.5%** (async) | 
| **Gemini 4 Neon (Standard)** | $0.12 | $0.12 | $0.90 | **$1.14** | **−25.0%** | 
| **Claude Opus 5.5** | $0.16 | $0.16 | $1.20 | **$1.52** | Baseline | 
| **Gemini 4 Carbon** | $0.16 | $0.16 | $1.20 | **$1.52** | Baseline | 

Notice where the money actually goes: **79% of the bill on every model is output tokens** (including reasoning/thinking tokens). That is why Neon’s efficiency advantage over Carbon (~32% lower real task cost) is even bigger than the 25% gap in list prices — Neon reaches its answer in fewer output tokens and delivers them at ~127 tok/s instead of ~46 tok/s.

## Where You Can Use Gemini 4 Neon Today

Unlike preview-only launches, Google shipped `gemini-4-neon` as **Generally Available (GA)** across six surfaces on day one:

- **Gemini Web App (`gemini.google.com`):** Available for**Google AI Pro** and**Google AI Ultra** subscribers (replacing older Pro slots).
- **Google Antigravity IDE & Android Studio:** Built-in for AI Pro and Ultra plans with direct controls for`low` ,`medium` ,`high` and`xhigh` reasoning depth.
- **GitHub Copilot:** Available for**Copilot Pro+, Business and Enterprise** across VS Code, JetBrains, Xcode and compatible extensions (including open-source agents that support GitHub Copilot providers).
- **Gemini API / Google AI Studio (`gemini-4-neon`):** Available on paid billing tiers only —**there is no free API tier** .
- **OpenRouter (`google/gemini-4-neon`):** Live at`$3 / $15` list pricing with observed ~127 tok/s streaming throughput.

## What Developers Are Actually Saying

Early reactions across Hacker News and engineering teams split cleanly along practical lines:

- **Speed changes the workflow:** Going from Carbon’s ~46 tok/s to Neon’s ~127 tok/s makes multi-file agent loops feel interactive again. Waiting 10 seconds instead of 28 seconds per edit step keeps developers in flow.
- **The “Opus feel” debate is still alive:** Several engineers testing both side by side note that while Neon wins on Intelligence Index (61 vs 59) and Vals.ai,**Claude Opus 5.5 still feels subtly more calibrated on fuzzy, under-specified design questions** where the prompt itself has hidden contradictions.
- **Naming fatigue is real:** Without a lookup table, nobody knows whether`Hydrogen` ,`Argon` ,`Neon` or`Carbon` is the flagship. Teams wiring models through an AI Gateway should alias them by role (`fast-worker` ,`daily-coder` ,`deep-architect` ) instead of hardcoding element names across microservices.

## Production Routing: Hydrogen → Neon → Carbon / Opus 5.5

The real takeaway from November 2026 is that you no longer have to pick one model for everything. A three-tier router captures both Hydrogen’s cost floor and Neon’s new Pareto peak:

1. **Tier 1 — High-volume execution (`gemini-4-hydrogen`, `high`):** Unit test sweeps, log classification, boilerplate refactors, parallel search subagents (`$0.57` per 10-turn session).
2. **Tier 2 — Default daily engineering (`gemini-4-neon`, `high` default / `xhigh` for tough bugs):** Multi-file feature work, PR reviews, terminal debugging, MCP tool loops, PDF/spec ingestion (`$1.14` per 10-turn session, ~127 tok/s). Route overnight batch jobs to**Neon Flex** (`$1.50/$7.50` ).
3. **Tier 3 — Deep escalation (`claude-opus-5-5` or `gemini-4-carbon`):** Ambiguous system design, high-stakes security reviews and long-horizon research where extra latency is worth the last few percent of judgment (`$1.52` per 10-turn session).

If you were previously paying `$4 / $20` for Opus 5.5 or Carbon on every coding prompt, moving 70–80% of daily engineering traffic to **Gemini 4 Neon** cuts your bill by a quarter immediately while making your agent loops more than twice as fast.

From the community

## Discussion on Mastodon & Bluesky

Replies to this article from the open web, pulled in live. No ads, no tracking scripts.

Reply there — it shows up here automatically.
