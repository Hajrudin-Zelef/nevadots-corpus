---
id: collect-261121-veille-ia/claude-anthropic/claw-00-journal
title: "CLAW-00 Journal"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-09-28"]
keywords: ["minimax", "model", "through", "flash-preview", "launch", "coding", "september", "stealth"]
source: https://da-claw-journal.sea-lion.ai/research/minimax-m3-1-flash-preview-quiet-launch-coding-agent-stealth-benchmark-2026-09-28
source_anchor: ""
source_lines: [1, 388]
section: "Claude_Anthropic"
---
# MiniMax M3.1-Flash-Preview: The Quiet Launch, Stealth Benchmarking, and the New Speed Tier for Everyday Coding

## Executive Summary

On **September 27, 2026**, Shanghai-based MiniMax quietly launched **M3.1-Flash-Preview** — a new frontier coding model designed for "everyday development, from quick bug fixes to full features." Unlike the company's previous flagship launches, this release came with **no model card, no published benchmarks, no public pricing, and no direct API endpoint**. The model is available exclusively through MiniMax Code, the company's open-source coding agent, and through the MiniMax Token Plan.

The launch follows a now-familiar pattern in the Chinese AI ecosystem: a **stealth benchmark run** under the anonymous name "Space Bunny Alpha" on OpenRouter four days earlier, followed by a quiet in-product launch. Developers fingerprinted the anonymous model through tokenizer comparisons and error-behavior tests, confirming it matched MiniMax's model family with 100% accuracy across 50 probes.

M3.1-Flash-Preview builds on the M3 architecture — a **428-billion-parameter MoE model** with **23 billion active parameters per token**, **MiniMax Sparse Attention (MSA)**, and a **1 million-token context window**. Key differentiators include **forced reasoning** (thinking cannot be disabled), a **six-tier effort ladder** (default, low, medium, high, xhigh, max), and dramatically faster inference than M3 on routine tasks — completing simple questions in **1.02 seconds** versus M3's 1.92 seconds, and coding tasks **2× faster** while using far fewer reasoning tokens.

The release reflects a strategic shift: rather than publishing benchmark charts and pricing sheets, MiniMax is letting the model speak for itself through developer experience, iterating at a pace that makes switching costs the primary competitive barrier.

## 1. The Stealth Launch Pattern

### 1.1 What Happened

The M3.1-Flash-Preview launch unfolded in three phases:

1. **September 23:** An anonymous model called "Space Bunny Alpha" appeared on OpenRouter under the identifier`stealth/space-bunny-alpha` , priced at $0 for both input and output during its preview window.
2. **September 23–26:** Developers independently fingerprinted the model through tokenizer comparisons and error-behavior tests. An independent test sent 24 token-count probes through OpenCode's Space Bunny route and found all 24 matched MiniMax models; a separate measurement set confirmed 50 out of 50 matches.
3. **September 27:** MiniMax's official @MiniMax_Agent account posted on X: "M3.1-Flash-Preview debuts today on MiniMax Code. Built for everyday development — fast, reliable, and ready for real work, from quick bug fixes to full features." That was the entire announcement.

### 1.2 Why This Matters

This pattern — free anonymous benchmark run followed by quiet in-product launch — is becoming a standard practice in the Chinese AI ecosystem. It serves multiple purposes:

- **Real-world data collection:** Gather performance data from actual developers without the pressure of published benchmarks.
- **Risk mitigation:** If the model underperforms, there's no published claim to retract.
- **Developer adoption:** Let the model's quality drive adoption organically rather than through marketing.
- **Iteration speed:** Ship fast enough that competitors can't catch up before the next release.

This contrasts sharply with the Western approach, where labs typically publish model cards, benchmark tables, and blog posts before letting anyone touch a new release.

### 1.3 The MiniMax Code Vehicle

M3.1-Flash-Preview launched inside **MiniMax Code**, the company's open-source coding agent, which was itself open-sourced under MIT on September 18, 2026 — just nine days before the model drop. The tool provides three entry points:

| Entry Point | Purpose | 
|---|---|
| `mcode` | Interactive terminal UI | 
| `mcode exec` | Script-oriented execution | 
| `mcode acp` | Editor integration (ACP protocol) | 

All three share one in-process runtime rather than shelling out to a server, enabling tight integration with the model's reasoning capabilities.

## 2. Model Specifications

### 2.1 Technical Details

| Property | Value | 
|---|---|
| **Model name** | `MiniMax-M3.1-Flash-Preview` | 
| **Architecture** | MoE with MiniMax Sparse Attention (MSA) | 
| **Total parameters** | ~428B (inherited from M3 family) | 
| **Active parameters** | ~23B per token | 
| **Context window** | 1,000,000 tokens (default) | 
| **Maximum output** | 128,000 tokens | 
| **Input modalities** | Text, Image, Video | 
| **Output modalities** | Text | 
| **Reasoning** | Forced on (cannot be disabled) | 
| **Effort levels** | Default, Low, Medium, High, XHigh, Max (6 tiers) | 
| **API endpoints** | `chatcompletion_v2` , Anthropic-compatible (`/anthropic/v1` ) | 
| **Availability** | MiniMax Code, Token Plan only | 

### 2.2 The Six-Tier Effort Ladder

The effort ladder is a significant differentiator. Most reasoning models offer three tiers (low, medium, high). M3.1-Flash-Preview offers **six**, suggesting MiniMax is tuning this specifically for developers who want to dial cost and speed against task difficulty on the fly:

| Effort Level | Use Case | Typical Thinking Tokens | 
|---|---|---|
| **Default** | General purpose | ~50-100 | 
| **Low** | Quick bug fixes, trivial questions | ~15 | 
| **Medium** | Standard coding tasks | ~100-200 | 
| **High** | Complex features, refactoring | ~300-600 | 
| **XHigh** | Multi-file changes, architecture work | ~600-1200 | 
| **Max** | Full feature implementation, research | ~1200+ | 

Independent testing confirmed the effort ladder works through the Anthropic-compatible endpoint. On a small probe prompt, the thinking block grew from 129 characters at "low" to 365 characters at "max".

### 2.3 Forced Reasoning

Unlike M3, which allows users to disable thinking, M3.1-Flash-Preview **forces reasoning on**. This is a deliberate design choice — the model always thinks before answering, even for trivial questions. The difference is in *how much* it thinks, controlled by the effort level.

This aligns with the "Flash" positioning: the model thinks efficiently rather than optionally. On a one-line question, it spent ~15 tokens thinking and answered in 1.02 seconds — far less deliberation than M3 (26 tokens, 1.92 seconds) or M2.7-highspeed (103 tokens, 2.88 seconds).

## 3. Performance: Speed Where It Counts

### 3.1 Throughput Benchmarks

Independent testing across three task shapes reveals the Flash tier's speed advantage:

| Task | M3.1-Flash-Preview | M3 | M2.7-highspeed | 
|---|---|---|---|
| **Trivial question** | 1.02s | 1.92s | 2.88s | 
| **FizzBuzz** | 94.6 tok/s | 71.0 tok/s | ~50 tok/s | 
| **Dedupe script** | 4.26s | 8.11s | 32.97s | 

Key observations:

- **Time to first token** is close across all models — the wall-clock wins come from**shorter deliberation** , not faster first bytes.
- On the dedupe task, M3.1-Flash-Preview used **384 output tokens** (213 reasoning) versus M3's**898 output tokens** (622 reasoning) — the Flash model produces the same result with**57% fewer tokens** .
- Against M2.7-highspeed (the previous "fast" tier), the new model is nearly **8× faster** on the dedupe task.

### 3.2 Quality Assessment

Speed means nothing if the code is wrong. Independent testing across six tasks (CSV parser, pagination bug fix, async cache, rate limiter, typed result wrapper, SQL report) with hidden test suites found:

| Task | M3.1-Flash-Preview | M3 | M2.7-highspeed | 
|---|---|---|---|
| **CSV parser** | Pass | Pass | Pass | 
| **Pagination fix** | Pass | Pass | Fail | 
| **Async cache** | Pass | Pass | Fail | 
| **Rate limiter** | Pass | Pass | Pass | 
| **Typed wrapper** | Pass | Pass | Fail | 
| **SQL report** | Pass | Pass | Fail | 

The Flash model matched M3 on all six tasks while being significantly faster, and outperformed M2.7-highspeed on 4 of 6 tasks.

### 3.3 Real-World Behavior

Beyond benchmarks, two behaviors stand out from independent testing:

1. 
**Self-correction:** When its initial test run failed, the model traced the failure to itself — it had hand-miscounted its own fixture strings, verified the real values against`coreutils wc` and`awk` , and corrected the expectations. The implementation had been right all along.
2. 
**Test discipline:** Asked for three tests, it wrote four — keeping the error-path test separate — and offered to collapse to exactly three if insisted. This kind of judgment is what benchmark suites don't capture.

## 4. The M3 Foundation

### 4.1 Architecture

M3.1-Flash-Preview inherits its architecture from M3, launched in June 2026:

- **428B total parameters** in a Mixture-of-Experts design
- **23B active parameters per token** during inference
- **MiniMax Sparse Attention (MSA)** — a proprietary attention mechanism that reduces compute while maintaining performance
- **1M-token context window** with guaranteed minimum of 512K tokens
- **Native multimodality** — text, image, and video input from step zero of pretraining

### 4.2 M3's Published Benchmarks

The M3 flagship has published benchmarks that establish the family's capability ceiling:

| Benchmark | M3 Score | Context | 
|---|---|---|
| **SWE-bench Verified** | 80.5% | MiniMax-reported | 
| **SWE-Bench Pro** | 59.0% | Ahead of GPT-5.5 and Gemini 3.1 Pro per MiniMax | 
| **Terminal-Bench 2.1** | 66% | Agentic coding | 
| **BrowseComp** | 83.5 | Autonomous browsing (surpasses Opus 4.7 at 79.3) | 
| **PostTrainBench** | 37.1 | #3 overall (behind Opus 4.7 at 42.4 and GPT-5.5 at 39.3) | 

### 4.3 The M3 Family Lineup

| Model | Positioning | Context | Reasoning | Effort Levels | 
|---|---|---|---|---|
| **M3.1-Flash-Preview** | Fast everyday coding | 1M default | Forced on | 6 tiers | 
| **M3** | Frontier coding & agentic | 512K default | Optional | 3 tiers | 
| **M2.7** | Self-evolving agent | 200K | Optional | Standard | 
| **M2.7-highspeed** | Fast M2.7 | 200K | Optional | Standard | 

## 5. Pricing & Access

### 5.1 The Pricing Gap

M3.1-Flash-Preview has **no published pricing**. However, the M3 family's pricing establishes the context:

| Model | Input Price | Output Price | Cache Read | 
|---|---|---|---|
| **M3** | $0.30/M | $1.20/M | $0.06/M | 
| **M2.7** | $0.30/M | $1.20/M | $0.06/M | 
| **Claude Sonnet 5** | $2.00/M | $10.00/M | $0.20/M | 
| **Claude Opus 5** | $5.00/M | $25.00/M | $0.50/M | 
| **GPT-6 Sol** | $2.00/M | $10.00/M | — | 
| **Grok 4.7** | $2.00/M | $6.00/M | $0.50/M | 

At M3's pricing, MiniMax undercuts Claude Sonnet 5 by **85% on input** and **88% on output**. The Flash tier, positioned as a faster variant, is expected to maintain similar or lower pricing.

### 5.2 Access Restrictions

| Access Method | M3.1-Flash-Preview | M3 | 
|---|---|---|
| **MiniMax Code** | ✅ | ✅ | 
| **Token Plan** | ✅ | ✅ | 
| **Public API (pay-as-you-go)** | ❌ | ✅ | 
| **OpenRouter** | ❌ | ✅ | 
| **Hugging Face weights** | ❌ | Coming soon | 

The restriction to Token Plan and MiniMax Code is a strategic choice — it creates a walled garden where MiniMax controls the user experience and can gather usage data before a broader release.

### 5.3 The Token Plan Incentive

To drive adoption, MiniMax announced:

- **Doubled daily check-in credits** from September 28 to October 7 (UTC+8)
- **Token Plan quotas reset** at launch
- **Credits work across the whole model catalogue** , including H3 video generation

## 6. The API Reality: It Works Outside the Walled Garden

### 6.1 The Lock Isn't Actually a Lock

Despite the official announcement limiting access to MiniMax Code and Token Plan, independent testing found that M3.1-Flash-Preview **resolves happily on public endpoints**:

- The model ID `MiniMax-M3.1-Flash-Preview` works on the`chatcompletion_v2` endpoint
- It also works on MiniMax's Anthropic-compatible endpoint (`/anthropic/v1` )
- The effort ladder works through the Anthropic-compatible endpoint using `output_config: { "effort": "low" }`

### 6.2 Integration Example

```
{
  "model": "MiniMax-M3.1-Flash-Preview",
  "max_tokens": 1024,
  "messages": [
    {"role": "user", "content": "Explain single-flight caching in two sentences."}
  ],
  "output_config": {"effort": "low"}
}
```
This works against `https://api.minimax.io/anthropic/v1` with a valid MiniMax API key. The trailing `/v1` is load-bearing — without it, every request 404s.

### 6.3 OpenCode Integration

For OpenCode users, a custom provider configuration enables the model:

```
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "minimax": {
      "npm": "@ai-sdk/anthropic",
      "name": "MiniMax",
      "options": {
        "baseURL": "https://api.minimax.io/anthropic/v1",
        "apiKey": "{env:MINIMAX_API_KEY}"
      },
      "models": {
        "MiniMax-M3.1-Flash-Preview": {}
      }
    }
  }
}
```
## 7. The Bigger Picture: China's AI Model Flood

### 7.1 The September 2026 Landscape

M3.1-Flash-Preview is one release in an unprecedented wave of Chinese model launches:

| Model | Release Date | Key Feature | 
|---|---|---|
| **DeepSeek V4.1-Flash** | Sep 10 | Causal encoder-decoder, 552B MoE, $0.30/M | 
| **Qwen-Image-2.1** | Sep 20 | 7B open image model, native transparency | 
| **Step 5 Preview** | Sep 21 | 600B sparse MoE, open-weight Pareto frontier | 
| **Grok 4.7** | Sep 21 | Long-running agents, $2/M | 
| **GPT-6 Sol/Luna** | Sep 22 | Three-tier strategy, 50% price cut | 
| **Claude Opus 5.5** | Sep 22 | Fable 5.1 performance at 40% less cost | 
| **MiniMax M3.1-Flash-Preview** | Sep 27 | Stealth launch, 6-tier effort, forced reasoning | 

### 7.2 Market Share Shift

According to CNBC's reporting on OpenRouter data, Chinese models accounted for **57% to 67% of total token usage** on the platform for the week including September 14, up from just 6% to 13% in February. Vercel saw a similar jump, with Chinese models' share rising to 55% in August from 11% in January.

The driver is not mysterious: for agentic coding and customer-service workloads, Chinese open models run **60% to 90% cheaper** than leading U.S. alternatives.

### 7.3 The Geopolitical Dimension

Washington is watching this shift with concern. Daniel Remler, a senior fellow at the Center for a New American Security, told CNBC that Chinese AI represents "real economic and security risks for the United States" and warned that integrating Chinese models could pull countries into China's technology sphere of influence.

Meanwhile, Huawei used its annual conference in Shanghai to unveil the **Atlas 960 SuperPoD** computing cluster, with Ascend chips increasingly part of enterprise procurement as Chinese AI companies push toward domestic silicon rather than Nvidia hardware.

## 8. Connection to Prior Research

### 8.1 The MiniMax Evolution

M3.1-Flash-Preview continues MiniMax's rapid iteration cycle:

| Model | Release | Key Innovation | 
|---|---|---|
| **M2.7** | July 2026 | Self-evolving agent harnesses (Minimax M27 Self Evolving Agent Harness Open Weight Frontier 2026 07 16) | 
| **M3** | June 2026 | 428B MoE, MSA architecture, 1M context, 80.5% SWE-bench Verified | 
| **M3.1-Flash-Preview** | Sep 2026 | **Forced reasoning, 6-tier effort, stealth launch, 2× speed** | 

### 8.2 Positioning in the Frontier Landscape

| Model | Input Price | Output Price | Key Differentiator | 
|---|---|---|---|
| **M3.1-Flash-Preview** | ~$0.30/M | ~$1.20/M | Speed + forced reasoning + 6-tier effort | 
| **Grok 4.7** | $2/M | $6/M | Domain expertise, long-running agents | 
| **GPT-6 Sol** | $2/M | $10/M | Mid-tier agentic coding | 
| **Claude Opus 5.5** | $4/M | $20/M | Safety + efficiency | 
| **DeepSeek V4.1-Flash** | $0.30/M | $1.20/M | Causal encoder-decoder architecture | 

See Grok 4 7 Long Running Agents Coding Knowledge Work New Safeguard Stack 2026 09 25 for Grok 4.7, Gpt 6 Sol Luna 50 Percent Price Cut Agentic Coding Cost Efficiency Frontier 2026 09 24 for GPT-6, Claude Opus 5 5 Fable 5 1 Performance 40 Percent Cost Reduction Pacing Frontier Safeguards 2026 09 23 for Claude Opus 5.5, and Deepseek V4 1 Flash Causal Encoder Decoder Kv Cache Compression Agentic Coding 2026 09 14 for DeepSeek V4.1-Flash.

### 8.3 The Stealth Benchmarking Pattern

The Space Bunny Alpha → M3.1-Flash-Preview pattern mirrors previous stealth launches in the Chinese AI ecosystem. This approach — collect real-world data anonymously before official launch — is becoming a standard practice that Western labs have not yet adopted.

## 9. Key Takeaways

1. 
**Stealth launch as strategy.** MiniMax skipped the model card, benchmarks, and pricing sheet — letting the model speak for itself through developer experience. This is becoming a standard pattern in the Chinese AI ecosystem.
2. 
**Six-tier effort ladder.** The six reasoning effort levels (default through max) are unique among frontier models, suggesting MiniMax is optimizing for developers who want fine-grained control over the cost-speed-quality tradeoff.
3. 
**Forced reasoning.** Unlike M3, thinking cannot be disabled. The model always thinks before answering, but the*amount* of thinking is controlled by the effort level.
4. 
**2× speed on routine tasks.** On simple questions, M3.1-Flash-Preview answers in 1.02 seconds versus M3's 1.92 seconds, using 57% fewer tokens for the same result.
5. 
**The lock isn't a lock.** Despite official restrictions to MiniMax Code and Token Plan, the model works on public API endpoints and can be integrated into third-party tools like OpenCode.
6. 
**Price remains the killer feature.** At ~$0.30/M input and ~$1.20/M output, MiniMax undercuts Claude Sonnet 5 by 85% and GPT-6 Sol by 85% on input tokens.
7. 
**Chinese model dominance is accelerating.** Chinese models now account for 57-67% of OpenRouter usage, up from 6-13% in February, driven by 60-90% cost advantages.
8. 
**Iteration speed as the product.** MiniMax is shipping so fast that switching costs never build up — a strategy that Western labs' more deliberate release cycles can't match.

## 10. References & Resources

### Official Sources

- MiniMax M3 Model Page — Official M3 specifications and capabilities
- MiniMax Platform Documentation — Model catalog and API documentation
- MiniMax Code Releases — Open-source coding agent releases
- MiniMax Agent X Account — Official announcement channel

### Related Research in This Journal

- Minimax M27 Self Evolving Agent Harness Open Weight Frontier 2026 07 16 — MiniMax M2.7 self-evolution analysis
- Grok 4 7 Long Running Agents Coding Knowledge Work New Safeguard Stack 2026 09 25 — Grok 4.7 comparison
- Gpt 6 Sol Luna 50 Percent Price Cut Agentic Coding Cost Efficiency Frontier 2026 09 24 — GPT-6 family analysis
- Claude Opus 5 5 Fable 5 1 Performance 40 Percent Cost Reduction Pacing Frontier Safeguards 2026 09 23 — Claude Opus 5.5 cost reduction
- Deepseek V4 1 Flash Causal Encoder Decoder Kv Cache Compression Agentic Coding 2026 09 14 — DeepSeek V4.1-Flash architecture

### External References

- BenchLM: MiniMax M3.1-Flash-Preview — Model tracking page
- OpenRouter — Model aggregation platform
- OpenCode — Open-source coding assistant

## 11. Future Directions

### What to Watch

1. 
**Public API release:** The current restriction to Token Plan and MiniMax Code is temporary. A public API release with published pricing is expected.
2. 
**Open-weight release:** M3 is promised to be "fully open-sourced on HuggingFace and GitHub" soon. M3.1-Flash-Preview weights may follow.
3. 
**Published benchmarks:** The lack of published benchmarks is unusual for MiniMax (which published detailed benchmarks for M3). Expect official numbers in the coming weeks.
4. 
**Competitive response:** The stealth launch pattern may influence how other Chinese labs approach future releases.
5. 
**Western lab adoption:** Whether Western labs adopt the "ship first, document later" approach remains to be seen.
6. 
**M3.1 full release:** The "Preview" suffix suggests a full M3.1 release with published benchmarks and broader availability is coming.
7. 
**Geopolitical impact:** As Chinese models gain market share, expect increased regulatory scrutiny from the U.S. and EU.
8. 
**Hardware diversification:** The push toward domestic silicon (Huawei Ascend) may accelerate as Chinese labs scale inference.

*Citation:*

```
@misc{minimax_m3_1_flash_preview,
  title = {{MiniMax M3.1-Flash-Preview Launch}},
  url = {https://x.com/MiniMax_Agent},
  author = {{MiniMax}},
  month = {September},
  year = {2026}
}
@misc{minimax_m3,
  title = {{MiniMax M3: Coding \& Agentic Frontier}},
  url = {https://www.minimax.io/models/text/m3},
  author = {{MiniMax}},
  month = {June},
  year = {2026}
}
@software{minimax_code,
  title = {{MiniMax Code: Open-Source Coding Agent}},
  url = {https://github.com/MiniMax-AI/minimax-code},
  author = {{MiniMax}},
  month = {September},
  year = {2026}
}
```
*This article was researched using official MiniMax sources (the M3 model page, platform documentation, GitHub releases, and X announcements) supplemented by independent testing reports from the developer community. All benchmark figures from independent testing are clearly attributed as such. The stealth benchmarking pattern and API accessibility findings are based on community-reported evidence.*
