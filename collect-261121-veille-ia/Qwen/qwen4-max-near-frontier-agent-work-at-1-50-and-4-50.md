---
id: collect-261121-veille-ia/qwen/qwen4-max-near-frontier-agent-work-at-1-50-and-4-50
title: "Qwen4-Max: Near-Frontier Agent Work at $1.50 and $4.50"
domain: qwen
role: reference
task: article
actors: ["Alibaba"]
dates: []
keywords: ["alibaba", "qwen4-max", "gpt-6", "last", "exam", "agent", "qwen3", "agents"]
source: https://llm-stats.com/blog/research/qwen4-max-launch
source_anchor: ""
source_lines: [1, 119]
section: "Qwen"
---
Alibaba released Qwen4-Max on **November 16, 2026**. It is a near-frontier flagship at a mid-tier price: **$1.50 input / $4.50 output**per million tokens, with a 1,000,000-token context and text, image, and video input. On Alibaba's self-reported launch figure at xhigh effort, it leads Opus 5.5, GPT-6.1 Sol, GPT-6 Astra, and Kimi K3.1 on long-horizon and professional-domain agent work, and it trails them clearly on terminal coding, software replication, and chart and video understanding. Official materials: the Qwen team blog and the Model Studio docs.

Output list price is under a quarter of Claude Opus 5.5's and under half of GPT-6.1 Sol's. Against Qwen3.8-Max it is better on every shared row, 25% cheaper in Singapore, and uses fewer tokens. Two catches: the launch numbers are at xhigh while the API defaults to high, and unlike Qwen3.8-Max there are no open weights.

## What Qwen4-Max is

Qwen4-Max is a sparse MoE with a **2.4T** backbone and **71B** parameters activated per token, plus 328B N-gram Embedding parameters. That is 25% fewer activated parameters than Qwen3.8-Max (95B active) at the same 2.4T scale. The new Qwen4 design pairs Gated DeltaNet layers with Qwen Sparse Attention (three GDN layers per QSA layer), adds a Gated Residual with four parallel residual branches, and uses N-gram Embedding. Alibaba reports no loss spikes in pre-training. The linear-attention GDN layers with sparse attention are aimed at long context.

It is not open weights. Alibaba says smaller Qwen4 models will be open-sourced soon. Qwen3.8-Max did ship open weights. Alibaba also open-sourced a Qwen-Live-Delivery-Plugin: an agent delivers a usable first version early, then successive releases the user can redirect.

Availability: QwenCloud (Token Plan and pay-as-you-go API), QwenWork, QwenStudio, Qoder, and OpenRouter (`qwen/qwen4-max`). A preview id `qwen4-max-preview` launched November 11, 2026 on Token Plan only and retires after 24:00 November 25, 2026 (UTC+8).

## Benchmarks

All scores below are **self-reported by Alibaba** from its November 16, 2026 launch materials. Qwen4-Max is shown at **xhigh**effort. Comparator settings are not stated. Some rows are third-party leaderboard values Alibaba cites (APEX-Agents from Mercor, HLAB-Vals-1.1 from Vals, Agents' Last Exam from its leaderboard, CodeArena and AgentArena from Arena). This is not a common-harness comparison.

### Full launch table

Self-reported by Alibaba. Bold marks the highest among the six models shown. GPT-6 Astra on Agents' Last Exam is identified by Alibaba's chart color key (one OpenAI model appears on that row).

| Benchmark | Qwen4-Max | Qwen3.8-Max-0902 | Opus 5.5 | GPT-6.1 Sol | GPT-6 Astra | Kimi K3.1 | 
|---|---|---|---|---|---|---|
| DeepSWE v1.1 | 75.7 | 69.3 | 74.2 | 75.2 | 74.1 | 75.5 | 
| CodeArena (Elo) | 1,750 | 1,676 | 1,816 | 1,742 | 1,763 | 1,686 | 
| Terminal-Bench 4.0 | 48.5 | 38.9 | 59.6 | 56.1 | 59.1 | 67.2 | 
| ProgramBench | 31.5 | 28.0 | 65.0 | 52.5 | 55.0 | 25.0 | 
| Agents' Last Exam v1.1 | 46.7 | 27.9 | 43.4 | Not shown | 42.6* | 43.4 | 
| JobBench | 68.5 | 65.3 | 75.4 | 66.2 | 66.1 | 61.2 | 
| APEX-Agents v1.1 | 72.1 | 63.3 | 73.5 | 60.0 | 64.7 | 65.5 | 
| HLAB-Vals-1.1 | 21.3 | 15.4 | 9.2 | 6.3 | 7.1 | 11.7 | 
| AgentArena (net improvement) | 7.5 | Not shown | 18.2 | 11.8 | 13.9 | 3.9 | 
| BioMysteryBench | 85.0 | 84.1 | 81.9 | 84.4 | 78.9 | 80.6 | 
| HealthBench Professional | 60.0 | 39.6 | 59.9 | 65.1 | 66.0 | 57.2 | 
| Humanity's Last Exam | 51.7 | 43.9 | 59.8 | 54.0 | 53.6 | 51.1 | 
| Chartography | 54.2 | 42.0 | 66.3 | 67.8 | 71.1 | 50.0 | 
| Video-Index | 68.1 | 63.2 | 74.8 | 73.8 | 76.7 | 63.2 | 

* Alibaba's chart shows one OpenAI model on Agents' Last Exam; by the chart's color key it is GPT-6 Astra. Row notes: APEX-Agents cited from the Mercor leaderboard; HLAB-Vals-1.1 from Vals; Agents' Last Exam from its leaderboard; CodeArena and AgentArena from Arena. JobBench uses an updated dataset and eval code. Chartography Qwen rows used a higher minimum image resolution setting. HLE tool setting not stated.

### An honest reading

Qwen4-Max is first among the six on **4 of 14**allowed rows: DeepSWE v1.1, Agents' Last Exam v1.1, HLAB-Vals-1.1, and BioMysteryBench. That matches Alibaba's positioning around professional domains and long-horizon agents. The DeepSWE spread across all six models is only 1.6 points; call it a tie, not a win. Versus Opus 5.5 it is ahead on 5 rows and behind on 9. Versus Qwen3.8-Max-0902 it is ahead on all 13 rows where both appear.

The trail gaps are large where they matter for coding agents. ProgramBench sits at 31.5 against Opus 5.5's 65.0. Terminal-Bench 4.0 is 48.5 against Kimi K3.1 Pro's 67.2 and Opus 5.5's 59.6. This is not the pick for a terminal-first coding agent. Stay on Opus 5.5, GPT-6 Astra, or Kimi K3.1 Pro for that work.

Vision improved over Qwen3.8-Max but still trails the frontier on Chartography (54.2 vs Astra 71.1) and Video-Index (68.1 vs Astra 76.7). On Alibaba's in-house Work360 (26 domains, 182 subdomains, 721 specialized fields), Qwen4-Max scores 52.69 vs Qwen3.8-Max 47.74; only Qwen models are reported. On Code Arena WebDev it ranks 6th at 1,750 (xhigh), behind Claude Fable 5.2 (1,826), Claude Opus 5.5 (1,816), Claude Sonnet 5.5 at xhigh (1,772), Gemini 4 Carbon (1,768), and GPT-6 Astra (1,763).

## Effort and tokens

Thinking is on by default. Effort levels are `low`, `high`, and `xhigh`, with default **high**. Compatibility aliases map `minimal` to low, `medium` to high, and `max`to xhigh. Alibaba's launch scores are at xhigh. Its own score-vs-tokens chart shows high effort scoring well below xhigh on Terminal-Bench 4.0; treat launch numbers as an upper bound unless you set xhigh.

Token efficiency versus Qwen3.8-Max, per Alibaba: about 24% and 35% fewer tokens on Work360 at xhigh and high; about 48% and 49% fewer on Terminal-Bench 4.0. Thinking plus answer is capped at 262,144 tokens. Practical advice: set `reasoning_effort: "xhigh"` for hard agent runs and budget output tokens; keep high for routine work. The preview defaulted to xhigh, so anyone who tested the preview should set xhigh explicitly to match.

Secure coding, self-reported by Alibaba on Last Secure Code Bench: pass rate for both functionality and security rose from 22.0% (Qwen3.8-Max) to 31.8%; code secure at function, file, and repository level together rose from 8% to 14.7%.

## Pricing

List prices are flat across the full 1M context, thinking or not. No long-context surcharge. No Batch price.

| Region | Input | Output | Cache read | Cache write 5m | Cache write 1h | 
|---|---|---|---|---|---|
| Singapore (International), also OpenRouter | $1.50 | $4.50 | $0.075 | $1.875 | $3.00 | 
| Beijing and Global regions | $1.413 | $4.24 | $0.071 | $1.767 | $2.827 | 

Singapore pay-as-you-go includes 1M free tokens for 90 days. Explicit cache needs a 1,024-token minimum prefix (5 minute and 1 hour TTL). Implicit cache is also available.

Worked example at list prices with no caching: 10M input plus 2M output tokens costs **$24** on Qwen4-Max, $30 on Gemini 4 Hydrogen, $40 on GPT-6.1 Sol or Claude Sonnet 5.5, $80 on Opus 5.5, and $200 on GPT-6 Astra.

Versus Qwen3.8-Max Singapore list ($2 / $6), Qwen4-Max is 25% lower. In Beijing and Global, Qwen3.8-Max lists at $1.65 / $4.951; Qwen4-Max is about 14% lower. Alibaba's own Work360 cost claim (Alibaba's number): average cost per task fell from $4.43 to $1.67, a 62% reduction.

## Should you switch

- **From Qwen3.8-Max:** yes. Cheaper, better on every shared row, fewer tokens. Keep 3.8 only if you need self-hosted open weights.
- **From Opus 5.5 or GPT-6 Astra** on long-horizon research, legal, bio, or clinical agent work: test it. The self-reported scores are competitive at a fraction of the price.
- **For terminal-heavy coding agents** or reconstructing software from binaries: stay on Opus 5.5, GPT-6 Astra, or Kimi K3.1 Pro.
- **For chart-dense or video-heavy inputs** at the top end: GPT-6 Astra and Opus 5.5 lead.
- **Need Batch discounts or open weights:** not available.

### Migration notes

- Same model id across Chat Completions, Responses, and Anthropic Messages.
- Effort names map: `max` becomes xhigh,`medium` becomes high.
- Preserve and pass back reasoning in multi-turn tool calls (`preserve_thinking` defaults to true).
- Explicit cache needs a 1,024-token prefix.

## Specs

| Context | 1,000,000 tokens | 
| Max input | 991,808 (non-thinking) or 983,616 (thinking) | 
| Max output | 262,144 combined thinking plus answer (thinking mode); 131,072 (non-thinking). Default thinking budget 131,072. | 
| Modalities | Input: text, image, video (QwenCloud also lists PDF understanding). Output: text. No audio. | 
| Effort | low, high, xhigh. Default high. Thinking on by default ( `enable_thinking: false` to turn off). | 
| APIs | OpenAI Chat Completions, OpenAI Responses, Anthropic Messages, DashScope. | 
| Features | Function calling, parallel tool calls, web search, structured output, explicit and implicit cache, CoT summary and encryption, code interpreter (needs thinking), MCP via Responses. | 
| Not supported | Batch, fine-tuning, model deployment. | 
| Regions | Singapore (International); Beijing (Chinese mainland); Frankfurt, Hong Kong, Tokyo, US Virginia (Global). | 
| Architecture | Sparse MoE, 2.4T total / 71B active, plus 328B N-gram Embedding. | 

More detail: QwenCloud latest model, model prices, preview docs, and OpenRouter. Peer list prices from Anthropic, OpenAI, and Google (Hydrogen).

## Safety

Alibaba says it added code-security checks to software engineering training, filtered cybersecurity training data to keep defensive material and remove offensive attack content, and added dedicated CBRN safety data in post-training. Its internal expert review found no capabilities posing large-scale cybersecurity risk and no significant CBRN uplift. These are Alibaba's own assessments; no third-party system card is published.

Banner: Alibaba Cloud / Qwen. Qwen 4 is now available: Qwen logo with a line drawing of the Temple of Heaven.

## FAQ

- Alibaba's Qwen team released**Qwen4-Max** on**November 16, 2026** . It is the first model in the Qwen4 family and the most capable Qwen model to date. The API id is`qwen4-max` .
- Singapore (International) and OpenRouter:**$1.50 input** ,**$4.50 output** , and**$0.075 cache read** per million tokens. Cache writes are**$1.875** (5 minute TTL) and**$3.00** (1 hour TTL). Beijing and Global regions are**$1.413 / $4.24** with matching cache rates. Pricing is flat across the full 1M context. There is no Batch price. Singapore pay-as-you-go includes 1M free tokens for 90 days.
- Context is**1,000,000 tokens** . Max input is 991,808 (non-thinking) or 983,616 (thinking). Max output is**262,144** combined thinking plus answer in thinking mode, or**131,072** in non-thinking mode. The default thinking budget is 131,072.
- On Alibaba's self-reported figure at xhigh, Qwen4-Max is ahead of Opus 5.5 on**5 of 14** rows (DeepSWE, Agents' Last Exam, HLAB-Vals, BioMysteryBench, HealthBench Professional by 0.1) and behind on**9 of 14** , including Terminal-Bench 4.0 (48.5 vs 59.6) and ProgramBench (31.5 vs 65.0). Output list price is under a quarter of Opus 5.5's ($4.50 vs $20).
- Versus GPT-6.1 Sol on Alibaba's figure: ahead on DeepSWE (75.7 vs 75.2), CodeArena (1,750 vs 1,742), JobBench (68.5 vs 66.2), APEX-Agents (72.1 vs 60.0), HLAB-Vals (21.3 vs 6.3), and BioMysteryBench (85.0 vs 84.4); behind on Terminal-Bench 4.0, ProgramBench, AgentArena, HealthBench Professional, Humanity's Last Exam, Chartography, and Video-Index. Sol is not shown on Agents' Last Exam. That is**6 ahead, 7 behind** . Output list price is 45% of Sol's ($4.50 vs $10).
- The API defaults to**high** . Alibaba's launch scores are at**xhigh** , and its own chart shows high scoring well below xhigh on Terminal-Bench 4.0. Set`reasoning_effort: "xhigh"` for hard agent runs and keep high for routine work. Levels are low, high, and xhigh.
- `qwen4-max-preview` launched November 11, 2026 on Token Plan only and retires after 24:00 November 25, 2026 (UTC+8). Preview effort levels were low, medium, xhigh with default xhigh. GA uses low, high, xhigh with default high, and is available on pay-as-you-go. Anyone who tested the preview should set xhigh explicitly to match launch numbers.
- Yes. OpenRouter lists it as`qwen/qwen4-max` at the same Singapore prices ($1.50 / $4.50).
- It is**near-frontier** . It ranks 6th on Code Arena WebDev at 1,750 Elo and leads some long-horizon and professional agent rows on Alibaba's own table, but trails Opus 5.5 on 9 of 14 allowed rows. Treat it as a strong mid-price option for agent work, not as the overall best model.

Continue Reading
