---
id: collect-261121-veille-ia/modeles-open/beam-1-0-501b-a23b-benchmarks-pricing-0-30-1-per-1m
title: "Beam 1.0 501B A23B benchmarks & pricing: $0.30/$1 per 1M"
domain: modeles-open
role: reference
task: article
actors: []
dates: ["2026-11-21"]
keywords: ["arena", "reasoning", "gamebench", "model", "benchmarks", "index", "lmarena", "setting"]
source: https://www.benchleader.com/models/beam-1-0-501b-a23b
source_anchor: ""
source_lines: [1, 114]
section: "Modeles_Open"
---
## Reasoning-effort configurations

The same model behaves differently depending on how much it is allowed to think. Each row is one setting, scored only on the benchmarks that were run at that setting. âNot statedâ collects results from publishers that did not say which setting they used; for a reasoning model that is usually its thinking mode, but we do not assume it. Pick a setting here or at the top of the page to see its price, speed and category scores.

| Effort | Index | Rank | Speed | First answer | Chat reply cost | Categories | 
|---|---|---|---|---|---|---|
| low | â | â | 132 tok/s | 0.35 s | $0.0004 |  | 
| medium | â | â | 132 tok/s | 0.35 s | $0.0004 |  | 
| xhigh | â | â | 132 tok/s | 0.35 s | $0.0004 |  | 
| not statedbest | 53.6 | #272 | 132 tok/s | 0.35 s | $0.0004 | Agents & tools 37 Â· Coding 52 Â· Human preference 61 Â· Maths 65 Â· Reasoning 62 | 

## Benchmark results

See all LLM benchmarks. One column per reasoning effort. Rank is among every configuration of every model on that benchmark. Hover a score for the run it came from.

### Reasoning

### Coding

| Benchmark | low | medium | xhigh | not stated | Source | Trend | 
|---|---|---|---|---|---|---|
| Arena Codingeffort not stated | â | â | â | 1505#74 | Arena (LMArena) |  | 
| Arena WebDeveffort not stated | â | â | â | 1437#94 | Arena (LMArena) |  | 
| GameBench 2 Overallnot in indexacross settings | â | â | â | 42.8#48 | DuelLab GameBench 2 |  | 
| GameBench 2 by reasoning settingnot in index | 39.1#150 | 35.9#162 | 53.3#92 | â | DuelLab GameBench 2 |  | 

### Agents & tools

### Instruction following

### Human preference

| Benchmark | not stated | Source | Trend | 
|---|---|---|---|
| Arena Text | 1434#116 | Arena (LMArena) |  | 
| Arena Creative Writingnot in index | 1387#143 | Arena (LMArena) |  | 
| Arena Multi-turnnot in index | 1432#121 | Arena (LMArena) |  | 
| Arena Longer Queriesnot in index | 1437#123 | Arena (LMArena) |  | 

GameBench 2 Overall: 42.8. GameBench 2, best reasoning setting (XHigh): 53.3. Reference only, not part of the BenchLeader Index.

DuelLab GameBench 2, release gb2-20261114-ca113f475f5e (https://benchmarks.duellab.org/releases/gb2-20261114-ca113f475f5e/), CC BY 4.0 (https://creativecommons.org/licenses/by/4.0/). Scores mapped to BenchLeader model names; not endorsed by DuelLab.

## API pricing: where to run it

Every provider serving this model through OpenRouter, with throughput and first-token latency measured on live traffic over the last 30 minutes and each providerâs own price. Purple marks the best in each column.

| Provider | Speed | First token | Input $/M | Output $/M | Context | Quantisation | 
|---|---|---|---|---|---|---|
| Baseten | 414 tok/s | 0.14 s | $0.400 | $1.20 | 1.0M | fp4 | 
| CoreWeave | 411 tok/s | 0.27 s | $0.300 | $1.00 | 1.0M | nvfp4 | 
| Nebius Token Factory | 368 tok/s | 0.38 s | $0.250 | $1.00 | 1.0M | fp4 | 
| Together | 304 tok/s | 0.19 s | $0.400 | $1.20 | 1.0M | â | 
| DigitalOcean | 173 tok/s | 0.38 s | $0.400 | $1.20 | 1.0M | â | 

## Price history

The price above, per 1M tokens, every day it changed.

## What a task costs

Estimates from list price, output speed and time to first answer for the best configuration. âWith cachingâ assumes three-quarters of the input is served from the prompt cache at $0.050 per 1M. Reasoning tokens are not modelled.

| Workload | Tokens in / out | Cost | With caching | Time | 
|---|---|---|---|---|
| Chat reply | 400 / 300 | $0.0004 | $0.0003 | 2.6 s | 
| Summarize a 30-page report | 12,000 / 600 | $0.0036 | $0.0018 | 4.9 s | 
| Code edit | 6,000 / 1,500 | $0.0030 | $0.0021 | 11.7 s | 
| Agentic coding session | 60,000 / 4,000 | $0.019 | $0.010 | 30.7 s | 
| Structured extraction | 2,000 / 200 | $0.0007 | $0.0004 | 1.9 s | 

## Compare Beam 1.0 501B A23B withâ¦

## See also

## Embed or cite

Show where Beam 1.0 501B A23B ranks on a model card or README. The badge redraws itself with every daily update, carries the data date and links back to this page.

`[![Beam 1.0 501B A23B on BenchLeader: #134 overall](https://www.benchleader.com/badge/beam-1-0-501b-a23b.svg)](https://www.benchleader.com/models/beam-1-0-501b-a23b)`
### Cite this

The ranking changes daily, so a reference carries the data date. APA style:

BenchLeader. (2026, November 21). Beam 1.0 501B A23B on the BenchLeader Index (method v1.5) [Data set]. Retrieved November 21, 2026, from https://www.benchleader.com/models/beam-1-0-501b-a23b

BibTeX:

```
@misc{benchleader_beam_1_0_501b_a23b_20261121,
  author       = {{BenchLeader}},
  title        = {{Beam 1.0 501B A23B on the BenchLeader Index (method v1.5)}},
  year         = {2026},
  month        = nov,
  howpublished = {\url{https://www.benchleader.com/models/beam-1-0-501b-a23b}},
  note         = {Data as of 2026-11-21}
}
```
## Questions people ask

- What are Beam 1.0 501B A23B's benchmark scores?
- As of 21 Nov 2026, Beam 1.0 501B A23B scores 53.6 Â±8.6 on the BenchLeader Index, strongest in maths (65) and weakest in agents & tools (37). That rests on independent benchmark results only, and every score on this page links to its publisher.
- How does Beam 1.0 501B A23B rank?
- Beam 1.0 501B A23B is #272 of 650 ranked configurations on 21 Nov 2026. 433 other configurations score within its margin, so the exact position matters less than the band. The leader is Claude Fable 5.2 (max effort) at 70.2.
- How much does Beam 1.0 501B A23B cost per 1M tokens?
- As of 21 Nov 2026, Beam 1.0 501B A23B costs $0.250 per 1M input tokens and $1.00 per 1M output tokens, $0.438 blended (three input tokens per output token). The list price last changed on 19 Nov 2026.
- Is Beam 1.0 501B A23B better than MiniMax M3.1 Flash?
- As of 21 Nov 2026, Beam 1.0 501B A23B and MiniMax M3.1 Flash are neck and neck on quality (53.6 vs 53.1 on the BenchLeader Index, inside the Â±10.2 margin of error), and MiniMax M3.1 Flash costs 1.7Ã less, which makes it the better buy for most workloads. The head-to-head page has the benchmark-by-benchmark detail.
- When was Beam 1.0 501B A23B released?
- Reflection released Beam 1.0 501B A23B on 5 Oct 2026. BenchLeader has results for it at 4 reasoning-effort settings.

Data as of 21 Nov 2026. Compare these configurations.

Cite as: BenchLeader, âBeam 1.0 501B A23B: benchmarks, pricing, speed and rankâ, data as of 21 Nov 2026, https://www.benchleader.com/models/beam-1-0-501b-a23b.
