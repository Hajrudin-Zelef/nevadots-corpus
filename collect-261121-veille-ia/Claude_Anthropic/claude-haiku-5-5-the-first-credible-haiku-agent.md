---
id: collect-261121-veille-ia/claude-anthropic/claude-haiku-5-5-the-first-credible-haiku-agent
title: "Claude Haiku 5.5: The First Credible Haiku Agent"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-10-07"]
keywords: ["haiku", "claude", "anthropic", "first", "effort", "thinking", "agent", "credible"]
source: https://llm-stats.com/blog/research/claude-haiku-5-5-launch
source_anchor: ""
source_lines: [1, 139]
section: "Claude_Anthropic"
---
Anthropic released Claude Haiku 5.5 on **October 7, 2026** as the successor to Claude Haiku 4.5. This is the first Haiku that reads as a credible agent, not just a cheap classifier for routing and extraction.

The buyer story has two halves. Capability: computer use and terminal work finally clear the bar that made Haiku 4.5 a non-starter for agent loops. Cost: Anthropic says about **75% cheaper on average**, but the durable details are the **100K prompt tier** and a tokenizer that uses about 30% more tokens for the same text. Misread either and your savings collapse.

Headline self-reported numbers at max effort: **72.4% on OSWorld 2.1** offline partial (from 15.7%), **39.2% on Terminal-Bench 4.0** (from 0.0%), **1620 Elo on GDPval-AA**, **1578 Elo on AA-Briefcase**. List starts at **$0.10 / $0.50** per 1M for prompts up to 100K. API id: `claude-haiku-5-5`. None of the launch scores are LLM Stats verified yet.

## At a Glance

- **API id:**`claude-haiku-5-5` (fixed id, no date suffix). Bedrock:`anthropic.claude-haiku-5-5`
- **Organization:** Anthropic
- **Release:** October 7, 2026. Successor to Haiku 4.5 (`claude-haiku-4-5-20251001` )
- **Pricing (<=100K prompts):** $0.10 input / $0.50 output per 1M. Cache read $0.01. 5m / 1h cache write $0.125 / $0.20. Batch $0.05 / $0.25
- **Pricing (>100K prompts):** $0.50 input / $2.50 output. Cache read $0.05. 5m / 1h write $0.625 / $1.00. Batch $0.25 / $1.25
- **Context:** 1M input (was 200K) / 128K max output (300K on Message Batches with beta header)
- **Modalities:** Text + image in (PDF supported), text out
- **Effort:** First Haiku with adjustable effort. Levels low / medium / high / xhigh / max. Default`medium` . Adaptive thinking on by default
- **Headline benches (self-reported, max effort):** OSWorld 2.1 partial 72.4%, Terminal-Bench 4.0 39.2%, GDPval-AA 1620 Elo, AA-Briefcase 1578 Elo, HLE no tools 45.9%, HLE with tools 57.4%
- **Knowledge cutoff:** June 2026
- **Availability:** Claude API, Claude.ai (all plans), Claude Code, Amazon Bedrock, Claude Platform on AWS, Google Cloud Vertex AI, Microsoft Foundry

## What Changed

From Haiku 4.5, almost every constraint that boxed Haiku into classification work moved:

- **1M context** (was 200K), with 128K max output
- **Effort controls** for the first time on a Haiku-class model, plus adaptive thinking
- **New tokenizer** (similar to Sonnet 5.5 and Opus 5.5), which uses more tokens per task
- **Two-tier pricing** keyed to the 100K prompt boundary
- **Computer and browser use tools** (`computer_toolset_20260801` ,`browser_toolset_20260801` ) on the Claude API and Google Cloud

Anthropic positions Haiku 5.5 for summaries, comps, database queries, classification, extraction, routing, chat, voice agents, live support, in-app assistants, and browser/desktop automation. It also "pairs well with Opus 5.5 and Sonnet 5.5 as a subagent on coding work": a stronger model plans, Haiku executes subtasks.

API constraints worth knowing before you swap ids: `temperature`, `top_p`, and `top_k` must be omitted (non-default values return 400). Assistant prefill is rejected with a 400 even with thinking off. Priority Tier is not supported. `thinking: {"type": "enabled", "budget_tokens": N}` returns a 400; adaptive thinking is the path.

## Benchmarks

Prefer Anthropic's launch table for the hero read. Every score below is **self-reported by Anthropic**. Haiku 5.5 ran at max effort (adaptive thinking, default sampling, five trials). Not LLM Stats verified. Interpret by job.

### Knowledge work

**GDPval-AA v2.1 at 1620 Elo**(+885 vs Haiku 4.5's 735) and **AA-Briefcase v1.1 at 1578 Elo** (+964 vs 614) are the agentic knowledge-work headlines. Elo is anchored so DeepSeek V4.1 Flash (max) = 1600, across 220 tasks and 44 occupations. GPT-6 Luna sits at 1437 / 1336. Sonnet 5.5 still leads at 1840 / 1824. The number that matters for Haiku buyers is the jump off Haiku 4.5, not catching Sonnet.

### Computer use

**OSWorld 2.1 offline partial at 72.4%** (from 15.7%, +56.7 pts) is the computer-use story. Caveats: 82 of 108 tasks, VM with no internet, 1080p, max 500 steps. The **strict pass rate is 37.1%**(Sonnet 5.5: 83.9% partial / 48.8% strict; Opus 5.5: 87.2% / 53.2%). GPT-6 Luna was run by Anthropic on the same 82 tasks through OpenAI's API with OpenAI's own context compaction (48.9%). Haiku 4.5 ran at a fixed thinking budget without server-side compaction. Always pair partial with strict.

Chartography without tools: **46.4%**(Haiku 4.5 6.4%, GPT-6 Luna 29.1%, Sonnet 5.5 61.6%). With tools, Haiku 5.5 reaches 86.2% vs Sonnet 5.5's 90.2%.

### Coding and terminal

**Terminal-Bench 4.0 at 39.2%**(from 0.0%) clears the prior Haiku floor. Setup: 66 tasks, 10 trials each, Claude Code in bare mode at max effort, safeguards on with no fallback (1.8% of trials flagged and failed). No internet egress; resources pre-cached, which "could lower scores". SE plus or minus 1.9. GPT-6 Luna is 16.4% from the public leaderboard (Codex CLI, max effort, not run by Anthropic). Sonnet 5.5 is 70.6% at max; Opus 5.5 is 66.4% at xhigh. Haiku is finally usable for terminal agents. It is not Sonnet.

**FrontierCode 1.1 Main at 46.4%**(no Haiku 4.5 result) sits near GPT-6 Luna's 42.4% and Sonnet 5.5's **52.1% at xhigh**. At matched max, Sonnet scored **46.2%**, essentially tied with Haiku. Haiku at xhigh: 45.8%. Extended: Haiku 58.4% (max); Sonnet 59.1% (max), 64.4% (xhigh). Built and run by Cognition; Claude models in Claude Code, GPT models in Codex. Effort mismatch is the honesty flag on the 52.1% cite.

### Reasoning

**Humanity's Last Exam, no tools: 45.9%** (from 10.2%, +35.7 pts). With tools: **57.4%**(from 18.7%, +38.7 pts). Sonnet 5.5: 56.9% / 64.5%. No GPT-6 Luna figure. Haiku 4.5's with-tools figure used a 60K thinking budget and 160K task budget to fit its 200K context.

### System card domains

Absolute percent-scale rows from the system card, grouped by domain. Haiku 5.5 at max effort unless noted. Elo stays above. Self-reported by Anthropic. Not LLM Stats verified.

## Effort Controls

Haiku 5.5 is the first Haiku with an adjustable effort setting. Levels: **low, medium, high, xhigh, max**. Default is **medium** on the Claude API and Claude Code. Thinking is adaptive and on by default. `thinking: {"type": "disabled"}` is allowed at high or below; at xhigh or max it returns a 400. Thinking blocks return empty (signature only) by default; `display: "summarized"` returns summaries. Thinking tokens bill as output and count toward max_tokens.

Medium-effort callouts matter because medium is the default: GDPval-AA at medium is **1277 Elo**"while using about a tenth of the output tokens it used at max" (Gemini 3.8 Flash scored 1435). AA-Briefcase at medium is **1372 Elo**"using under a quarter of the output tokens it used at max" (Gemini 3.8 Flash scored 1203). Max is available when you need the last points. Most production traffic should start at medium and measure.

## Pricing

Claude Platform list prices, USD per 1M tokens. A request is charged at the rate for that request's prompt length: a prompt over 100,000 tokens pays the higher rates on the whole request.

| Rate | Haiku 5.5 (<=100K) | Haiku 5.5 (>100K) | Haiku 4.5 | Sonnet 5.5 | 
|---|---|---|---|---|
| Input | $0.10 | $0.50 | $1.00 | $2.00 | 
| Output | $0.50 | $2.50 | $5.00 | $10.00 | 
| 5-minute cache write | $0.125 | $0.625 | $1.25 | $2.50 | 
| 1-hour cache write | $0.20 | $1.00 | $2.00 | $4.00 | 
| Cache read | $0.01 | $0.05 | $0.10 | $0.10 (was $0.20) | 
| Batch input / output | $0.05 / $0.25 | $0.25 / $1.25 | $0.50 / $2.50 | $1.00 / $5.00 | 

Anthropic: "On average, it now costs around 75% less to run." Sticker math: 90% lower than Haiku 4.5 for requests up to 100K, and 50% lower above 100K. Prompts up to 100K made up around 90% of Haiku 4.5 requests. The ~75% figure already accounts for the tokenizer. Same day, Sonnet 5.5 cache reads were cut 50% from $0.20 to $0.10, which Anthropic says reduces Sonnet 5.5's cost on most agentic tasks by around 20%.

### Worked examples

- **A. 10K in / 1K out (<=100K):** Haiku 5.5 $0.0015 vs Haiku 4.5 $0.0150 (90% cheaper). Same text at 1.3x tokens: $0.00195, still 87% below. Per 1M such requests: $1,500 vs $15,000.
- **B. 150K in / 4K out (>100K):** Haiku 5.5 $0.085 vs Haiku 4.5 $0.170 (50% cheaper). At 1.3x: $0.1105, 35% below $0.170.
- **C. Cached agent turn** (80K cache read + 5K fresh + 1K out, 85K prompt): Haiku 5.5 $0.0018 vs Haiku 4.5 $0.0180 (90% cheaper).
- **D. Tier-crossing trap:** 90K / 2K on Haiku 4.5 becomes ~117K / 2.6K on Haiku 5.5. Haiku 4.5 $0.100 vs Haiku 5.5 $0.065 (35% savings, not 90%).
- **E. Batch 10K / 1K:** Haiku 5.5 $0.00075 vs Haiku 4.5 $0.0075.

Modifiers: `inference_geo: "us"` adds a 1.1x multiplier. Bedrock and Google Cloud regional endpoints carry a 10% premium over global endpoints. New monthly API credits landed the same day for Max 5x ($100), Max 20x ($200), and Team (up to $500 pooled).

## Tokenizer Caveat

Haiku 5.5 ships an updated tokenizer similar to Sonnet 5.5 and Opus 5.5. The launch page says it "uses slightly more tokens per task." The migration guide is blunter: the same input text produces **approximately 30% more tokens** than on Haiku 4.5. The exact increase depends on content. Present both claims honestly.

Combined with the 100K tier, that is the trap. A workload that sat safely under 100K on Haiku 4.5 can cross the cliff after re-tokenization and pay the higher rate on the entire prompt. Input cost of a 100,000-token prompt is $0.010; a 100,001-token prompt is about $0.050 (5x the input rate on the whole prompt). Output jumps from $0.50 to $2.50 the same way. Anthropic's migration guide advises recounting prompts near 100K with `model: claude-haiku-5-5`. Do that before you celebrate a 90% cut.

## When to Use

- **Prefer Haiku 5.5** for high-volume classification, extraction, routing, chat and voice agents, live support, in-app assistants, and browser/desktop automation where Haiku 4.5 was too weak and Sonnet 5.5 is too expensive per turn.
- **Use as a subagent** under Opus 5.5 or Sonnet 5.5 for coding work: the stronger model plans, Haiku executes scoped subtasks. Cognition reports Devin Fusion with a Haiku 5.5 sidekick holds a FrontierCode score of 66.2.
- **Step up to Sonnet 5.5** when Terminal-Bench, OSWorld strict, or FrontierCode margins matter, or when open-ended judgment dominates cost. Sonnet still leads the hard agent rows.
- **Customer anecdotes** (attributed, not LLM Stats measured): HubSpot 92.8% averaged over three runs on its CRM suite; AlphaSense 0.84 vs 0.76 over Haiku 4.5 on 400 queries; Box 11 points higher than Haiku 4.5 at about half the latency; Asana over a 30% reduction in latency for task completions and up to 2.5x faster inference per agent turn.

## Migration from Haiku 4.5

- Swap to `claude-haiku-5-5` . No date suffix, no alias.
- Recount prompts near 100K. Expect about 30% more tokens for the same text.
- Drop non-default `temperature` /`top_p` /`top_k` . Drop assistant prefill. Drop Priority Tier.
- Use adaptive thinking. Do not send `budget_tokens` . Disable thinking only at high effort or below.
- Re-measure at default medium before assuming you need max.
- Haiku 4.5 remains active. No deprecation date has been announced.

## Availability

Claude API; Claude.ai on all plans (Free, Pro, Max, Team, Enterprise; web, iOS, Android); Claude Code; Amazon Bedrock (`anthropic.claude-haiku-5-5`) and Claude Platform on AWS; Google Cloud Vertex AI; Microsoft Foundry. Same id `claude-haiku-5-5`on Vertex, Foundry, and Claude Platform on AWS. Anthropic calls it "our fastest model to date" at each model's standard speed (it runs less quickly than Opus models in Fast Mode). No official tokens-per-second figure is published.

## Safeguards

Anthropic says Haiku 5.5's safeguards are more restrictive than Haiku 4.5's but somewhat less restrictive than those on other recent models. In cybersecurity they permit a wider range of defensive tasks than Sonnet 5.5's safeguards, but still block penetration testing and other techniques more likely to be used by attackers. Biology safeguards are the same as for Sonnet 5, Sonnet 5.5, and Opus 5. Safety classifiers can return `stop_reason: "refusal"`; there is no server-side fallback model when a classifier triggers.

## Outlook

Haiku 5.5 closes the gap that made the cheap tier a classifier-only tool. Computer use, terminal agents, and knowledge-work Elo finally clear a bar you can build on. The cost story only holds if you respect the 100K cliff and recount after the tokenizer.

Through-line: first credible Haiku agent, two-tier pricing from $0.10 / $0.50, effort controls with a medium default, and a migration that punishes sloppy token assumptions. For the primary sources, see Anthropic's Haiku 5.5 announcement, the product page, the system card, platform overview, migration guide, pricing, and effort docs.

- Anthropic released**Claude Haiku 5.5** on**October 7, 2026** . The API id is`claude-haiku-5-5` (fixed id, no date suffix). It succeeds Claude Haiku 4.5 (`claude-haiku-4-5-20251001` ).
- Claude Platform list is two-tier by prompt length. Prompts up to 100K tokens:**$0.10 / $0.50** input / output per 1M. Prompts over 100K:**$0.50 / $2.50** . Cache reads are $0.01 / $0.05 by tier. Anthropic says about**75% cheaper** on average than Haiku 4.5 after the tokenizer change. A whole request pays the rate for that request's prompt length.
- Haiku 5.5 supports a**1 million token** context window (Haiku 4.5 was 200K) with up to**128K** max output (300K on Message Batches with the`output-300k-2026-03-24` beta header). Input is text and images (PDF via Anthropic's general PDF support). Output is text only. Knowledge cutoff is June 2026.
- Yes. It is the first Haiku-class model with adjustable effort:**low, medium, high, xhigh, max** . Default is**medium** on the Claude API and Claude Code. Thinking is adaptive and on by default. You can disable thinking at high effort or below; at xhigh or max, disabling thinking returns a 400.
- Self-reported at max effort, Haiku 5.5 sits between Haiku 4.5 and Sonnet 5.5 on most launch rows, and ahead of GPT-6 Luna on OSWorld partial (72.4% vs 48.9%), Terminal-Bench (39.2% vs 16.4%), and GDPval-AA (1620 vs 1437 Elo). Sonnet 5.5 still leads OSWorld (83.9%), Terminal-Bench (70.6%), and both Elo benches. Prefer Sonnet when score margin matters; prefer Haiku when cost, latency anecdotes, and subagent volume matter.
- Haiku 5.5 uses a new tokenizer. The same input text produces about**30% more tokens** than on Haiku 4.5 (content-dependent). A 90K Haiku 4.5 prompt can become ~117K on Haiku 5.5 and cross into the higher pricing tier. Worked example: $0.100 on Haiku 4.5 vs $0.065 on Haiku 5.5 (35% savings, not 90%). Recount prompts near 100K with`model: claude-haiku-5-5` .
- No deprecation date has been announced. Haiku 4.5 remains active as a legacy model. Do not plan migrations around a retirement date that Anthropic has not published.
- Claude API, Claude.ai (Free, Pro, Max, Team, Enterprise; web, iOS, Android), Claude Code, Amazon Bedrock (`anthropic.claude-haiku-5-5` ), Claude Platform on AWS, Google Cloud Vertex AI, and Microsoft Foundry. Same API id`claude-haiku-5-5` on Vertex, Foundry, and Claude Platform on AWS.

Continue Reading
