---
id: collect-261121-veille-ia/qwen/qwen4-max-1-50-4-50-pricing-benchmarks-effort-levels
title: "Qwen4-Max: $1.50/$4.50 Pricing, Benchmarks, Effort Levels"
domain: qwen
role: reference
task: article
actors: ["Alibaba"]
dates: ["2026-11-16"]
keywords: ["qwen4-max", "xhigh", "effort", "alibaba", "tokens", "input", "thinking", "output"]
source: https://lucenta.ai/blog/qwen4-max-launch
source_anchor: ""
source_lines: [1, 70]
section: "Qwen"
---
## What it is

Alibaba released `qwen4-max` on November 16, 2026. It is the first model in the Qwen4 family.

- Architecture: sparse MoE, 2.4T total parameters, 71B activated per token, plus 328B N-gram Embedding parameters.
- Context: 1,000,000 tokens. Max input is 991,808 (non-thinking) or 983,616 (thinking).
- Max output: 262,144 tokens combined thinking plus answer in thinking mode; 131,072 in non-thinking mode.
- Input modalities: text, image, video. Output: text only.
- No open weights. No Batch API. No fine-tuning.

Alibaba says smaller Qwen4 models will be open-sourced soon. `qwen4-max-preview`, which launched November 11, 2026 on Token Plan only, retires after 24:00 November 25, 2026 (UTC+8).

## Pricing

Pricing is flat across the full 1M context. No long-context surcharge. No Batch price.

| Region | Input | Output | Cache read | Cache write 5m | Cache write 1h | 
|---|---|---|---|---|---|
| Singapore (International) and OpenRouter | $1.50 | $4.50 | $0.075 | $1.875 | $3.00 | 
| Beijing and Global regions | $1.413 | $4.24 | $0.071 | $1.767 | $2.827 | 

Singapore pay-as-you-go includes 1M free tokens for 90 days. Explicit cache requires a 1,024-token minimum prefix.

- 10M input plus 2M output at list with no caching: $24 on `qwen4-max` , $40 on`gpt-6.1-sol` or Claude Sonnet 5.5, $80 on Claude Opus 5.5, $200 on GPT-6 Astra.
- Versus Qwen3.8-Max Singapore list ($2.00 input / $6.00 output): `qwen4-max` is 25% lower.

## Benchmarks

All scores are self-reported by Alibaba from November 16, 2026 launch materials. `qwen4-max` is shown at `xhigh` effort. Comparator settings are not stated. Some rows are third-party leaderboard values Alibaba cites. This is not a common-harness comparison.

| Benchmark | qwen4-max | Qwen3.8-Max-0902 | Opus 5.5 | gpt-6.1-sol | GPT-6 Astra | Kimi K3.1 | 
|---|---|---|---|---|---|---|
| DeepSWE v1.1 | 75.7 | 69.3 | 74.2 | 75.2 | 74.1 | 75.5 | 
| CodeArena (Elo) | 1,750 | 1,676 | 1,816 | 1,742 | 1,763 | 1,686 | 
| Terminal-Bench 4.0 | 48.5 | 38.9 | 59.6 | 56.1 | 59.1 | 67.2 | 
| ProgramBench | 31.5 | 28.0 | 65.0 | 52.5 | 55.0 | 25.0 | 
| Agents Last Exam v1.1 | 46.7 | 27.9 | 43.4 | not shown | 42.6 | 43.4 | 
| JobBench | 68.5 | 65.3 | 75.4 | 66.2 | 66.1 | 61.2 | 
| APEX-Agents v1.1 | 72.1 | 63.3 | 73.5 | 60.0 | 64.7 | 65.5 | 
| HLAB-Vals-1.1 | 21.3 | 15.4 | 9.2 | 6.3 | 7.1 | 11.7 | 
| AgentArena (net improvement) | 7.5 | not shown | 18.2 | 11.8 | 13.9 | 3.9 | 
| BioMysteryBench | 85.0 | 84.1 | 81.9 | 84.4 | 78.9 | 80.6 | 
| HealthBench Professional | 60.0 | 39.6 | 59.9 | 65.1 | 66.0 | 57.2 | 
| Humanity Last Exam | 51.7 | 43.9 | 59.8 | 54.0 | 53.6 | 51.1 | 
| Chartography | 54.2 | 42.0 | 66.3 | 67.8 | 71.1 | 50.0 | 
| Video-Index | 68.1 | 63.2 | 74.8 | 73.8 | 76.7 | 63.2 | 

`qwen4-max` leads the field on 4 of 14 rows: DeepSWE v1.1, Agents Last Exam v1.1, HLAB-Vals-1.1, and BioMysteryBench. It is ahead of Opus 5.5 on 5 rows and behind on 9. The DeepSWE spread across all six models is 1.6 points. ProgramBench is not close: 31.5 versus Opus 5.5 at 65.0. Terminal-Bench 4.0 sits at 48.5 versus Kimi K3.1 at 67.2.

## Effort levels and the preview gap

Thinking is on by default. Effort levels are `low`, `high`, and `xhigh`. The API default is `high`. Alibaba launch scores are at `xhigh`. Its own chart shows `high` scoring well below `xhigh` on Terminal-Bench 4.0. Treat launch numbers as an upper bound unless you set `xhigh` explicitly.

- Set `reasoning_effort: xhigh` for hard agent runs.
- Keep `high` for routine work.
- `qwen4-max-preview` defaulted to`xhigh` . Anyone who tested the preview should set`xhigh` explicitly to match launch numbers.
- Compatibility aliases: `max` maps to`xhigh` ,`medium` maps to`high` ,`minimal` maps to`low` .

Alibaba reports roughly 24 to 49% fewer tokens used versus Qwen3.8-Max depending on task and effort level. Thinking plus answer is capped at 262,144 tokens.

## Where to use it and where not to

The self-reported numbers point at two clusters. Long-horizon research agents, legal and clinical domain work, and biology benchmarks are where `qwen4-max` competes at a fraction of Opus 5.5 or GPT-6 Astra pricing. Terminal-heavy coding agents, software reconstruction from binaries, and chart-dense or video-heavy inputs are where Opus 5.5, GPT-6 Astra, or Kimi K3.1 lead clearly.

- Migrating from Qwen3.8-Max: straightforward. Better on every shared row, 25% cheaper at Singapore list. Keep Qwen3.8-Max only if you need self-hosted open weights.
- No Batch pricing and no open weights are hard constraints. If either matters, the decision is made.

## Availability

`qwen4-max` is available on QwenCloud (Token Plan and pay-as-you-go), QwenWork, QwenStudio, Qoder, and OpenRouter as `qwen/qwen4-max` at Singapore prices. APIs: OpenAI Chat Completions, OpenAI Responses, Anthropic Messages, DashScope. Features include function calling, parallel tool calls, web search, structured output, explicit and implicit cache, code interpreter (requires thinking), and MCP via Responses.
