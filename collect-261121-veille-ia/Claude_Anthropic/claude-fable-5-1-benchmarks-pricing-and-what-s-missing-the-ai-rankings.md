---
id: collect-261121-veille-ia/claude-anthropic/claude-fable-5-1-benchmarks-pricing-and-what-s-missing-the-ai-rankings
title: "Claude Fable 5.1: Benchmarks, Pricing and What's Missing | The AI Rankings"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["fable", "claude", "opus", "anthropic", "price", "agentic", "mtok", "knowledge"]
source: https://theairankings.com/anthropic/claude-fable-5-1/
source_anchor: ""
source_lines: [1, 204]
section: "Claude_Anthropic"
---
# Claude Fable 5.1

- Provider
- Anthropic
- Status
- Legacy
- Context
- 1,000,000 tok
- Price
- $10 / $50 /MTok
- Knowledge
- 2026-06

Claude Fable 5.1 was Anthropicâs Mythos-class flagship from 1 September to 9 November 2026, when Claude Fable 5.2 replaced it â the tier that sits *above* the Opus class. It was released on **1 September 2026**, just under three months after Fable 5. It was the first model to pass 65 on the independent Artificial Analysis Intelligence Index, scoring **66** at launch against Opus 5âs 63 and Fable 5âs 62; on the harder v4.3 index Artificial Analysis introduced on 7 September it read **53, tied first with GPT-6 Astra** at the time, above Opus 5 (51) and Fable 5 (50).

The launchâs own framing is about cost rather than capability: token prices are **unchanged at $10/$50 per million**, and the entire advertised saving comes from cutting cache reads 75% to **$0.25 per million**. That is a real cut, and it is also the claim that most needs checking â see the cost story below, because Artificial Analysis measures Fable 5.1 costing *more* per completed task than Fable 5 did.

Fable 5.1 shipped alongside **Mythos 5.1**, the same underlying model with its cyber and bio safeguards relaxed, restricted to vetted US organisations. This page covers Fable 5.1, the model you can actually buy.

## Quick specs

| **Provider** | Anthropic | 
| **Tier** | Mythos-class (above Opus) | 
| **Released** | 1 September 2026 | 
| **Status** | Available â paid Claude plans and API accounts | 
| **API model ID** | `claude-fable-5-1` | 
| **Context window** | 1,000,000 tokens | 
| **Max output** | 128,000 tokens | 
| **Knowledge cutoff** | June 2026 | 
| **Input price** | $10.00 / MTok | 
| **Output price** | $50.00 / MTok | 
| **Cache reads** | $0.25 / MTok (down 75% from $1.00) | 
| **AA Intelligence Index** | **66 at launch** (v4.1, then a record); 53 on v4.3 (7 Sep 2026), then tied first with GPT-6 Astra | 
| **Terminal-Bench-Science 0.1** | 52.6% (Fable 5: 24.7%) | 
| **Terminal-Bench 4.0** | 55.8% (Opus 5: 52.3%) | 
| **SWE-bench** | **Not published â no figure of any kind** | 
| **Best for** | Long-horizon agentic work, agentic scientific research, frontier knowledge work | 
| **Limitations** | 2x Opus 5âs token price and higher cost per task; no SWE-bench figure; verbose and slow | 

## What actually changed

Anthropic positions Fable 5.1 as a step up on âcoding, knowledge work, and long-running problem-solving.â Four changes matter in practice.

**Agentic science is the standout.** On Terminal-Bench-Science 0.1 â an agentic scientific-research eval â Fable 5.1 scores **52.6%** against Fable 5âs 24.7%. That is more than double, and it is the largest single jump in the launch table. It also clears Opus 5 (29.0%) and GPT-5.6 Sol (22.4%) by a wide margin.

**Cache reads got much cheaper.** $1.00 to $0.25 per million tokens. On workloads that re-read a large stable prefix â long agent loops, big repository contexts â this is where the money goes, which is why Anthropicâs own estimate of the saving is larger for agentic work (up to ~45%) than for typical use (~25%).

**The safeguards fire less often.** Anthropic reports Claude Code users should see roughly **60% fewer cyber-safeguard interventions per session** than on Fable 5, and that the biology safeguards fire **85% less often on benign elementary-biology and medical questions**. The scope also shifted: Fable 5.1 âcan now be used to discover software vulnerabilities â though not to develop exploits for them.â This addresses the loudest complaint about the redeployed Fable 5, whose tightened classifier tripped on routine coding.

**Effort now buys more.** Anthropic says that at **Low or Medium** effort Fable 5.1 matches or beats Fable 5 at substantially lower cost â which is the practical form of the price claim. Product defaults reflect it: High in Claude Code, Medium in agent sessions and on Claude.ai.

## Benchmark performance

All figures below are Anthropic-reported and vendor-run, from the 1 September launch comparison, except where marked independent.

### Agentic science and coding

| Benchmark | Fable 5.1 | Fable 5 | Opus 5 | GPT-5.6 Sol | 
|---|---|---|---|---|
| **Terminal-Bench-Science 0.1** | **52.6%** | 24.7% | 29.0% | 22.4% | 
| **Terminal-Bench 4.0** | 55.8% | â | 52.3% | 37.3% | 
| **CursorBench 3.2.0** | **73.4%** | 70.5% | â | â | 

Read the Terminal-Bench 4.0 row carefully. Anthropicâs table compares Fable 5.1âs 55.8% against **Mythos 5âs 42.0%**, not against Fable 5, which has no published figure on that version â so the headline â55.8 against 42.0â is a comparison with a *different, restricted* model. On the same launch table the restricted Mythos 5.1 led the test at 60.9%.

### Reasoning, knowledge work and computer use

| Benchmark (1 September launch table) | Fable 5.1 | Fable 5 | 
|---|---|---|
| **Humanityâs Last Exam** (no tools) | **60.9%** | 57.8% | 
| **Humanityâs Last Exam** (with tools) | **65.0%** | 63.8% | 
| **GDPval-AA v2** (knowledge work) | **1853 Elo** | 1723 Elo | 
| **OSWorld 2.0** (partial) | **77.9%** | 72.9% | 
| **OSWorld 2.0** (strict) | **41.7%** | 36.1% | 
| **AutomationBench** | **31.4%** | 17.1% | 

AutomationBench nearly doubles and OSWorld moves several points on both scorings â consistent with Anthropicâs long-horizon-agent pitch. The gains on Humanityâs Last Exam are real but modest: three points without tools, roughly one with.

### The independent number

Artificial Analysis scored Fable 5.1 at **66 on its Intelligence Index** at launch (adaptive reasoning, max effort, default fallback), **1st of 192 models tracked** and the first score above 65 the index had recorded, with Opus 5 at 63 and Fable 5 at 62. Artificial Analysis then released new versions of the index (v4.2 on 4 September, v4.3 on 7 September), which drop GPQA Diamond and add the harder Terminal-Bench 4.0 and AutomationBench-AA, so every model scores lower on them. On v4.3 Fable 5.1 scored **53, tied first with GPT-6 Astra**, above Opus 5 (51) and Fable 5 (50); it led Astra on GDPval-AA v2 (1764 to 1662), SciCode and AA-Omniscience accuracy, and trailed it on Terminal-Bench 4.0 (52.0% to 59.1%) and AutomationBench-AA. This is what earns the placement: a vendor table alone would cap the model in our Strong tier.

The same measurement carries the caveats. Fable 5.1 generated **140 million output tokens** running the index against a median of 71 million â roughly double the field â at **66 tokens per second** (slower than average) with a **285-second** time to first token. It cost **$8,523** to run the index, the highest Artificial Analysis has recorded.

### What Anthropic did not publish

**No SWE-bench figure of any kind** â neither Pro nor Verified â appears in Anthropicâs launch comparison. For a model pitched on coding, from the lab whose predecessor set the SWE-bench Pro record, that is a conspicuous gap. The consequence is concrete: Fable 5.1 has no SWE-bench Pro figure to set against **Fable 5âs 80.3%**, a predecessorâs number, so there is no evidence that Fable 5.1 beats it.

There is a second gap worth naming. Anthropicâs launch table puts Fable 5.1 at **1853 Elo on GDPval-AA v2**; Anthropicâs own Opus 5 System Card reports **1861** for Opus 5 on the same version. The two figures come from different Anthropic tables and are not a clean head-to-head, but on Anthropicâs published knowledge-work numbers Fable 5.1 does not clear the model that costs half as much.

## The cost story: cheaper tokens, dearer tasks

This is the claim to test, because the launch leads with it.

|  | Input / MTok | Output / MTok | Cache read / MTok | AA cost per index task | 
|---|---|---|---|---|
| **Claude Fable 5.1** | $10.00 | $50.00 | **$0.25** | **$3.69** ($3.76 at max effort);**$7.63 on v4.3** | 
| Claude Fable 5 | $10.00 | $50.00 | $1.00 | $3.08â$3.25 | 
| Claude Opus 5 | $5.00 | $25.00 | $0.50 | $2.34; $5.86 on v4.3 | 

Anthropicâs estimate â about 25% off a typical workload, up to about 45% off highly agentic work â is a statement about *cache-heavy* workloads, and on those it is credible: nothing else changed. But Artificial Analysis, running the same standard suite it ran against Fable 5, measures Fable 5.1 at **$3.69 per completed task against Fable 5âs $3.08â$3.25** â about 20% *more*. The reason is verbosity: at roughly double the median output tokens, Fable 5.1 spends the savings and then some on its own reasoning. On v4.3 the pattern held at a higher level: $7.63 per task against Fable 5âs $8.75 and Opus 5âs $5.86 â and against GPT-6 Astraâs $3.26 for the same index score.

Both things are true, and which one you get depends on your workload. If you re-read a large cached prefix on every turn, the cut is real money. If your bill is dominated by output tokens, expect to pay more than you did on Fable 5 â and roughly 60% more per task than on Opus 5.

## Safeguards, watermarks and enterprise privacy

Fable 5.1 keeps the Mythos-class safeguard architecture but retunes it. Anthropic reports **~60% fewer cyber-safeguard interventions per session** in Claude Code versus Fable 5, and biology safeguards firing **85% less often** on benign elementary-biology and medical questions. The permitted scope widened in one specific direction: **vulnerability discovery is now allowed, exploit development is not**.

Two things are new at the platform level:

- **Output watermarking.** Anthropic adds an invisible numerical watermark to outputs from models released after**2 August 2026** , for EU AI Act compliance. It is imperceptible without detection tooling and Anthropic says it carries no user or organisation data.
- **Enterprise Frontier Safeguards (EFS).** Misuse detection that stores the data in the**customerâs own cloud infrastructure** rather than Anthropicâs. It begins a phased rollout in**autumn 2026** across Claude Code, Claude Enterprise, AWS, Google Cloud and Microsoft Azure â and Anthropicâs 9 November 2026 launch post for Fable 5.2 describes it as in private beta.

Mythos-class models remain âCovered Modelsâ with **mandatory 30-day data retention**; there is no zero-data-retention option.

## How to access Claude Fable 5.1

Fable 5.1 is available to anyone on a paid Claude plan or with an API account, as `claude-fable-5-1` across Claude.ai, the Claude API, Claude Code and Claude Enterprise, plus Amazon Bedrock (`anthropic.claude-fable-5-1`), Google Cloud Vertex AI and Microsoft Foundry. Anthropic commits to retiring it no sooner than **1 September 2027**.

```
from anthropic import Anthropic
client = Anthropic()
message = client.messages.create(
    model="claude-fable-5-1",
    max_tokens=4096,
    effort="medium",  # Anthropic: matches or beats Fable 5 at low/medium, for less
    messages=[{"role": "user", "content": "Your prompt here"}],
)
```
Integration notes for Mythos-class models: **adaptive thinking is always on** and cannot be disabled, the **raw chain-of-thought is never returned**, and the API default effort is `high` while agent sessions and Claude.ai default to `medium`. Supported features include the memory tool, code execution, programmatic tool calling, context editing, compaction, task budgets and vision.

For the restricted sibling with relaxed cyber and bio safeguards, see **Mythos 5.1**.

## How Claude Fable 5.1 compares

### vs Claude Opus 5

Opus 5 is the tier below at **half the token price** ($5/$25) and, on Artificial Analysisâs v4.3 measurement, **$5.86 per task against Fable 5.1âs $7.63** ($2.34 against $3.69 at launch, on v4.1). Fable 5.1 led the independent index (66 vs 63 at launch; 53 vs 51 on v4.3, 7 September) and clears Opus 5 comfortably on agentic science (52.6% vs 29.0%) and by three points on Terminal-Bench 4.0 (55.8% vs 52.3%). But Opus 5 has a published SWE-bench Pro figure (79.2%) and Fable 5.1 has none, and Anthropicâs own GDPval-AA v2 numbers put Opus 5 marginally ahead. **Fable 5.1 earns its premium on long-horizon agentic and scientific tasks, not across the board.**

### vs Claude Fable 5

Same price per token, better on every published benchmark they share, and much cheaper on cache reads. The honest catch is verbosity: about 20% more per completed task on the independent measurement. Fable 5 keeps one thing 5.1 cannot claim â a published SWE-bench Pro score, at Fable 5.1âs launch the highest one published, at 80.3%.

### vs Claude Mythos 5.1

Same underlying model, different safeguard levels. Mythos 5.1 leads Terminal-Bench 4.0 at **60.9% against 55.8%** because it is not routing or declining the same requests, but it is restricted to vetted US organisations through the Cyber Verification Program and the Life Sciences Verification Program. If you can buy it, you are buying Fable 5.1.

### vs GPT-5.6 Sol

On Anthropicâs table Fable 5.1 leads GPT-5.6 Sol clearly on both terminal benchmarks (52.6% vs 22.4% on science, 55.8% vs 37.3% on coding), and it led on the independent index (66 vs 61 at launch, on v4.1; 53 vs 47 on v4.3, 7 September). Sol is far cheaper â $4/$20 on its promotional rate to 21 November â and carries a METR-flagged reward-hacking concern. See best AI models and best AI for coding for where this lands across the field.

## Known limitations

**No SWE-bench figure of any kind.** Neither Pro nor Verified. On the siteâs coding-weighted methodology this is the single largest reason Fable 5.1 does not simply take every coding pick.

**Cost per task went up, not down.** Despite the cache-read cut, Artificial Analysis measured $3.69 per index task against Fable 5âs $3.08â$3.25 at launch â and $7.63 on v4.3, more than double GPT-6 Astraâs $3.26 for the same score â driven by roughly double the median output tokens.

**Slow.** 66 output tokens per second and a 285-second time to first token on the independent measurement â at the high end for reasoning models, and a real constraint on interactive use.

**Vendor-run headline benchmarks.** Every figure except the Artificial Analysis index and cost measurements is Anthropicâs own, and the Terminal-Bench 4.0 comparison is drawn against Mythos 5 rather than Fable 5.

**Enterprise Frontier Safeguards is announced, not shipped.** A phased rollout begins in autumn 2026; it is not something you can deploy today.

**Mandatory 30-day data retention.** Mythos-class models are âCovered Modelsâ â no zero-data-retention option.

**Twice Opus 5âs token price.** $10/$50 against $5/$25, for a model that does not lead Anthropicâs own published knowledge-work numbers.

## Version history

| Version | Released | Key points | 
|---|---|---|
| **Claude Fable 5.1 / Mythos 5.1** | 1 Sep 2026 | AA Intelligence Index 66 at launch (1st), 53 on v4.3, 7 Sep (tied 1st); Terminal-Bench-Science 52.6%; cache reads cut 75% to $0.25; no SWE-bench figure | 
| **Claude Fable 5 / Mythos 5** | 9 Jun 2026 | First public Mythos-class tier; SWE-bench Pro 80.3%; suspended 12â30 Jun under US export controls | 
| **Claude Opus 5** | 24 Jul 2026 | The Opus tier below, at half the price | 

## Frequently asked questions

### Is Claude Fable 5.1 the best AI model right now?

No. At launch it scored 66 on the Artificial Analysis Intelligence Index, the first model above 65, and on v4.3, released on 7 September 2026, it scored 53, level with GPT-6 Astra and ahead of Claude Opus 5 (51) and Claude Fable 5 (50), leading Astra on knowledge work and factual accuracy. Claude Opus 5.5 scored 58 on the same version at its 22 September 2026 launch. It is not the best value: Opus 5.5 costs less than half as much per token, and on v4.3 Astra reached the same index score at less than half the cost per task. For current standings, see best AI models.

### How much does Claude Fable 5.1 cost?

$10 per million input tokens and $50 per million output â unchanged from Fable 5 and double Claude Opus 5. Cache reads are the change: $0.25 per million, cut 75% from $1.00. Anthropic estimates roughly 25% off a typical workload and up to about 45% off highly agentic work. Batch requests are 50% off.

### Is Fable 5.1 actually cheaper than Fable 5?

It depends on the workload. On cache-heavy agentic loops, yes â the cache-read cut is real and nothing else went up. But Artificial Analysis measured Fable 5.1 at $3.69 per completed benchmark task against Fable 5âs $3.08â$3.25 at launch ($7.63 against $8.75 on v4.3), because it produces roughly double the median output tokens. If your bill is output-dominated, expect to pay more.

### What is Claude Fable 5.1âs SWE-bench score?

Anthropic did not publish one â no SWE-bench Pro or Verified figure of any kind accompanied the launch announcement. At Fable 5.1âs launch the highest published SWE-bench Pro score was Claude Fable 5âs 80.3%.

### Whatâs the difference between Fable 5.1 and Mythos 5.1?

They are the same underlying model at different safeguard levels. Fable 5.1 ships the standard cyber and bio safeguards and is available to any paid Claude or API customer. Mythos 5.1 relaxes those safeguards for vetted US organisations through the Cyber Verification Program and the Life Sciences Verification Program, and scores higher on Terminal-Bench 4.0 (60.9% vs 55.8%) as a result.

### What is the context window?

1,000,000 tokens, with up to 128,000 output tokens on the synchronous Messages API. Anthropic gives the reliable knowledge cutoff and the training data cutoff as June 2026.

### Can Fable 5.1 be used for security work?

For finding vulnerabilities, yes â Anthropic explicitly permits vulnerability discovery while continuing to block exploit development, and reports around 60% fewer cyber-safeguard interventions per session in Claude Code than Fable 5. Work that needs the safeguards relaxed further runs through the Cyber Verification Program on Mythos 5.1.
