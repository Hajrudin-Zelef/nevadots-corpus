---
id: collect-261121-veille-ia/glm-zhipu/glm-5-3-flashx-benchmarks-pricing-0-37-1-25-per-1m
title: "GLM 5.3 FlashX benchmarks & pricing: $0.37/$1.25 per 1M"
domain: glm-zhipu
role: reference
task: article
actors: ["Zhipu AI"]
dates: ["2026-11-20"]
keywords: ["flashx", "benchmarks", "gamebench", "results", "benchleader", "tokens", "index", "model"]
source: https://www.benchleader.com/models/glm-5-3-flashx
source_anchor: ""
source_lines: [1, 57]
section: "GLM_Zhipu"
---
## Versions

Zhipu AI has shipped 2 models under this name. Each is ranked on its own results; a newer version often has fewer results so far, which holds its index nearer the average until more arrive.

## Reasoning-effort configurations

The same model behaves differently depending on how much it is allowed to think. Each row is one setting, scored only on the benchmarks that were run at that setting. âNot statedâ collects results from publishers that did not say which setting they used; for a reasoning model that is usually its thinking mode, but we do not assume it. Pick a setting here or at the top of the page to see its price, speed and category scores.

## Benchmark results

See all LLM benchmarks. One column per reasoning effort. Rank is among every configuration of every model on that benchmark. Hover a score for the run it came from.

### Coding

| Benchmark | low | high | max | not stated | Source | Trend | 
|---|---|---|---|---|---|---|
| GameBench 2 Overallnot in indexacross settings | â | â | â | 56.3#26 | DuelLab GameBench 2 |  | 
| GameBench 2 by reasoning settingnot in index | 49.5#108 | 50.6#99 | 68.6#49 | â | DuelLab GameBench 2 |  | 

GameBench 2 Overall: 56.3. GameBench 2, best reasoning setting (Max): 68.6. Reference only, not part of the BenchLeader Index.

DuelLab GameBench 2, release gb2-20261114-ca113f475f5e (https://benchmarks.duellab.org/releases/gb2-20261114-ca113f475f5e/), CC BY 4.0 (https://creativecommons.org/licenses/by/4.0/). Scores mapped to BenchLeader model names; not endorsed by DuelLab.

## API pricing: where to run it

Every provider serving this model through OpenRouter, with throughput and first-token latency measured on live traffic over the last 30 minutes and each providerâs own price. Purple marks the best in each column.

| Provider | Speed | First token | Input $/M | Output $/M | Context | Quantisation | 
|---|---|---|---|---|---|---|
| Z.ai | 89 tok/s | 1.32 s | $0.370 | $1.25 | 1.0M | fp8 | 

## What a task costs

Estimates from list price, output speed and time to first answer for the best configuration. âWith cachingâ assumes three-quarters of the input is served from the prompt cache at $0.075 per 1M. Reasoning tokens are not modelled.

| Workload | Tokens in / out | Cost | With caching | Time | 
|---|---|---|---|---|
| Chat reply | 400 / 300 | $0.0005 | $0.0004 | 4.4 s | 
| Summarize a 30-page report | 12,000 / 600 | $0.0052 | $0.0025 | 7.6 s | 
| Code edit | 6,000 / 1,500 | $0.0041 | $0.0028 | 16.9 s | 
| Agentic coding session | 60,000 / 4,000 | $0.027 | $0.014 | 43.0 s | 
| Structured extraction | 2,000 / 200 | $0.0010 | $0.0005 | 3.4 s | 

## See also

## Questions people ask

- Why does GLM 5.3 FlashX have no BenchLeader Index score?
- The index needs at least five independent results across at least two categories, and as of 20 Nov 2026 GLM 5.3 FlashX does not have them yet. Launch-post scores are never used, so it is ranked once independent publishers have measured it.
- How much does GLM 5.3 FlashX cost per 1M tokens?
- As of 20 Nov 2026, GLM 5.3 FlashX costs $0.370 per 1M input tokens and $1.25 per 1M output tokens, $0.590 blended (three input tokens per output token).
- When was GLM 5.3 FlashX released?
- Zhipu AI released GLM 5.3 FlashX on 18 Sep 2026. BenchLeader has results for it at 4 reasoning-effort settings.

Data as of 20 Nov 2026. Compare these configurations.

Cite as: BenchLeader, âGLM 5.3 FlashX: benchmarks, pricing, speed and rankâ, data as of 20 Nov 2026, https://www.benchleader.com/models/glm-5-3-flashx.
