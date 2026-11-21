---
id: collect-261121-veille-ia/deepseek/deepseek-v4-1-flash-1017-benchmarks-0-30-1-20-per-1m
title: "DeepSeek V4.1 Flash 1017 benchmarks: $0.30/$1.20 per 1M"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: ["2026-11-20"]
keywords: ["index", "reasoning", "artificial", "analysis", "benchmarks", "stated", "deepseek", "results"]
source: https://www.benchleader.com/models/deepseek-v4-1-flash-1017
source_anchor: ""
source_lines: [1, 260]
section: "DeepSeek"
---
## Versions

DeepSeek has shipped 2 models under this name. Each is ranked on its own results; a newer version often has fewer results so far, which holds its index nearer the average until more arrive.

## Reasoning-effort configurations

The same model behaves differently depending on how much it is allowed to think. Each row is one setting, scored only on the benchmarks that were run at that setting. âNot statedâ collects results from publishers that did not say which setting they used; for a reasoning model that is usually its thinking mode, but we do not assume it. Pick a setting here or at the top of the page to see its price, speed and category scores.

| Effort | Index | Rank | Speed | First answer | Chat reply cost | Categories | 
|---|---|---|---|---|---|---|
| no reasoning | 44.9 | #494 | 153 tok/s | 1.13 s | $0.0005 | Coding 36 Â· Knowledge 46 Â· Long context 53 Â· Multimodal 51 Â· Reasoning 31 | 
| low | â | â | 132 tok/s | 0.76 s | $0.0005 | Reasoning 64 | 
| high | 51.0 | #350 | 165 tok/s | 20 s | $0.0005 | Coding 46 Â· Knowledge 46 Â· Multimodal 62 Â· Reasoning 53 | 
| xhigh | â | â | 132 tok/s | 0.76 s | $0.0005 |  | 
| maxbest | 56.8 | #183 | 161 tok/s | 47 s | $0.0005 | Agents & tools 54 Â· Coding 62 Â· Composite 54 Â· Knowledge 48 Â· Long context 66 Â· Maths 60 Â· Multimodal 63 Â· Reasoning 55 | 
| not stated | â | â | 132 tok/s | 0.76 s | $0.0005 | Instruction following 67 Â· Knowledge 53 | 

## Benchmark results

See all LLM benchmarks. One column per reasoning effort. Rank is among every configuration of every model on that benchmark. Hover a score for the run it came from.

### Metadata

### Reasoning

| Benchmark | no reasoning | low | high | max | not stated | Source | Trend | 
|---|---|---|---|---|---|---|---|
| GPQA Diamond | â | â | â | 93.2%#27 | â | Epoch AI Benchmarking Hub |  | 
| LiveBench 2.0 Reasoningnot in index | â | â | â | 89.1%#19 | â | LiveBench |  | 
| Humanity's Last Exam (AA)not in index | 11.5%#315 | â | â | 43.3%#69 | â | Artificial Analysis |  | 
| HLE-Diamond (AA) | 7.1%#154 | â | 20.8%#94 | 25.3%#77 | â | Artificial Analysis |  | 
| ARC-AGI-1 | 5.7%#265 | 91.5%#81 | 94.5%#54 | 93.5%#65 | â | ARC Prize |  | 
| ARC-AGI-2 | 2.1%#220 | 73.6%#69 | 68.6%#79 | 78.1%#59 | â | ARC Prize |  | 
| ARC-AGI-3 | â | â | â | 5.8%#47 | â | ARC Prize |  | 
| CritPtnot in index | 0.3%#293 | â | â | 24.0%#58 | â | Artificial Analysis |  | 
| CritPt v2 hard subset (AA) | 1.0%#148 | â | 27.5%#100 | 25.0%#106 | â | Artificial Analysis |  | 
| HLE-Diamond (Scale)not in indexeffort not stated | â | â | â | â | 20.5%#23 | Scale AI SEAL |  | 
| Chess Puzzlesnot in index | â | â | â | 46.0%#31 | â | Epoch AI Benchmarking Hub |  | 
| Mystery Game Puzzlesnot in index | â | â | â | 49.0%#20 | â | Epoch AI Benchmarking Hub |  | 

### Coding

| Benchmark | no reasoning | high | max | not stated | Source | Trend | 
|---|---|---|---|---|---|---|
| SciCode | 38.5%#198 | â | 55.9%#52 | â | SciCode |  | 
| Arena WebDev | â | â | 1659#22 | â | Arena (LMArena) |  | 
| LiveBench 2.0 Codingnot in index | â | â | 80.4%#13 | â | LiveBench |  | 
| SciCode (AA)not in index | 38.5%#205 | â | 55.9%#51 | â | Artificial Analysis |  | 
| Code Migrationnot in index | â | â | 44.2%#25 | â | Vals AI |  | 
| Code Migration v1.1not in index | â | â | 67.5%#8 | â | Vals AI |  | 
| Vibe Code Bench v1.1not in index | â | â | 87.7%#16 | â | Vals AI |  | 
| Vibe Code Bench 1-100 v1.1not in index | â | â | 26.1%#9 | â | Vals AI |  | 
| ALE-Benchnot in index | â | â | 1622#23 | â | Epoch AI Benchmarking Hub |  | 
| WeirdML v3not in index | â | 9.1%#16 | â | â | WeirdML |  | 
| Terminal-Bench 4.0 (AA)not in index | 9.1%#137 | 28.8%#85 | 33.8%#70 | â | Artificial Analysis |  | 
| GameBench 2 Overallnot in indexacross settings | â | â | â | 61.4#18 | DuelLab GameBench 2 |  | 
| GameBench 2 by reasoning settingnot in index | 49.9#101 | 64.3#57 | 69.9#44 | â | DuelLab GameBench 2 |  | 
| Design Arena Websitenot in indexeffort not stated | â | â | â | 1292#23 | Design Arena |  | 
| Design Arena Codenot in indexeffort not stated | â | â | â | 1281#35 | Design Arena |  | 
| FrontierCode 1.1 (AA)not in index | â | â | 43.7%#25 | â | Artificial Analysis |  | 
| FrontierCode 2.0 (AA) | 6.1%#122 | 16.8%#94 | 28.5%#55 | â | Artificial Analysis |  | 

### Agents & tools

### Maths

| Benchmark | max | Source | Trend | 
|---|---|---|---|
| FrontierMath Tiers 1â3 | 71.2%#34 | Epoch AI Benchmarking Hub |  | 
| FrontierMath Tier 4 | 46.3%#34 | Epoch AI Benchmarking Hub |  | 
| OTIS Mock AIME | 99.4%#22 | Epoch AI Benchmarking Hub |  | 
| ArXivMathnot in index | 42.5%#6 | MathArena |  | 
| BrokenArXivnot in index | 48.7%#6 | MathArena |  | 

### Knowledge

| Benchmark | no reasoning | high | xhigh | max | not stated | Source | Trend | 
|---|---|---|---|---|---|---|---|
| SimpleQA Verified | â | â | â | 42.5%#53 | â | Epoch AI Benchmarking Hub |  | 
| LiveBench 2.0 Data Analysisnot in index | â | â | â | 79.5%#16 | â | LiveBench |  | 
| LiveBench Researchnot in index | â | â | â | 78.6%#27 | â | LiveBench |  | 
| AA-Omniscience v1.1 | 2.1#94 | 1.5#96 | â | 0.9#98 | â | Artificial Analysis |  | 
| PRBench Financeeffort not stated | â | â | â | â | 47.1%#21 | Scale AI SEAL |  | 
| PRBench Legaleffort not stated | â | â | â | â | 47.3%#26 | Scale AI SEAL |  | 
| Excel Modeling Benchmarknot in index | â | â | â | 61.8%#33 | â | Vals AI |  | 
| Excel Modeling Benchmark v1.1not in index | â | â | â | 69.2%#26 | â | Vals AI |  | 
| Access to Justice Benchmarknot in index | â | â | â | 73.1%#8 | â | Vals AI |  | 
| DrugDiscoveryBenchnot in index | â | â | 60.6%#9 | â | â | Scale AI SEAL |  | 
| AA-Omniscience v1.1: accuracynot in index | 21.9%#132 | 38.9%#89 | â | 39.6%#85 | â | Artificial Analysis |  | 
| AA-Omniscience v1.1: non-hallucinationnot in index | 74.7%#31 | 38.8%#114 | â | 35.9%#120 | â | Artificial Analysis |  | 
| FACTS Parametricnot in indexeffort not stated | â | â | â | â | 52.8%#17 |  |  | 

### Multimodal

| Benchmark | no reasoning | high | max | Source | Trend | 
|---|---|---|---|---|---|
| LiveBench Designnot in index | â | â | 79.8%#10 | LiveBench |  | 
| MMMU-Pro | 69.2%#177 | 79.7%#72 | 80.6%#60 | Artificial Analysis |  | 
| Humanity's Sixth Sensenot in index | â | â | 30.1%#25 | Scale AI SEAL |  | 
| VIALS-AAnot in index | â | â | 40.4%#17 | Handshake AI |  | 

### Long context

### Composite

| Benchmark | no reasoning | high | max | not stated | Source | Trend | 
|---|---|---|---|---|---|---|
| Epoch Capabilities Indexnot in indexeffort not stated | â | â | â | 159.4#24 | Epoch AI Benchmarking Hub |  | 
| LiveBench 2.0 | â | â | 70.3%#16 | â | LiveBench |  | 
| AA Intelligence Index v5.0.1not in index | 18.5#113 | 28.6#86 | 32.6#71 | â | Artificial Analysis |  | 
| AA Intelligence Index v4.3.2not in index | â | â | 45.1#39 | â | Artificial Analysis |  | 
| AA Progress Index v1not in index | 277.6#190 | 319.3#68 | 323.7#50 | â | Artificial Analysis |  | 
| Vals Indexnot in index | â | â | 61.3#16 | â | Vals AI |  | 
| Vals Finance Indexnot in index | â | â | 60.9%#15 | â | Vals AI |  | 
| Vals Router Indexnot in indexeffort not stated | â | â | â | 61.3%#8 | Vals AI |  | 
| Vals Legal Indexnot in index | â | â | 71.9%#14 | â | Vals AI |  | 
| AA Cyber Indexnot in index | â | â | 56.8#3 | â | Artificial Analysis |  | 

GameBench 2 Overall: 61.4. GameBench 2, best reasoning setting (Max): 69.9. Reference only, not part of the BenchLeader Index.

DuelLab GameBench 2, release gb2-20261114-ca113f475f5e (https://benchmarks.duellab.org/releases/gb2-20261114-ca113f475f5e/), CC BY 4.0 (https://creativecommons.org/licenses/by/4.0/). Scores mapped to BenchLeader model names; not endorsed by DuelLab.

## Where it wins

Benchmarks where this configuration ranks in the top five of every configuration measured.

## API pricing: where to run it

Every provider serving this model through OpenRouter, with throughput and first-token latency measured on live traffic over the last 30 minutes and each providerâs own price. Purple marks the best in each column.

| Provider | Speed | First token | Input $/M | Output $/M | Context | Quantisation | 
|---|---|---|---|---|---|---|
| Baseten | 434 tok/s | 0.41 s | $0.600 | $2.40 | 1.0M | â | 
| CoreWeave | 229 tok/s | 0.46 s | $0.300 | $1.20 | 1.0M | fp8 | 
| Morph | 227 tok/s | 0.44 s | $0.600 | $2.40 | 1.0M | fp8 | 
| Nebius Token Factory | 219 tok/s | 0.71 s | $0.180 | $0.720 | 1.0M | fp4 | 
| Mistral | 186 tok/s | 0.95 s | $0.300 | $1.20 | 1.0M | fp8 | 
| PrimaLabs | 177 tok/s | 0.66 s | $0.240 | $0.960 | 1.0M | â | 
| Crusoe | 173 tok/s | 0.69 s | $0.290 | $1.20 | 1.0M | fp8 | 
| DigitalOcean | 167 tok/s | 0.75 s | $0.180 | $0.720 | 1.0M | â | 
| Fireworks | 163 tok/s | 0.72 s | $0.300 | $1.20 | 1.0M | â | 
| Fireworks | 163 tok/s | 0.89 s | $0.450 | $1.80 | 1.0M | â | 
| Open Inference | 159 tok/s | 0.91 s | $0.060 | $0.800 | 1.0M | fp4 | 
| Together | 155 tok/s | 0.71 s | $0.300 | $1.20 | 1.0M | â | 
| Venice | 150 tok/s | 1.16 s | $0.300 | $1.20 | 1M | fp8 | 
| Wafer | 148 tok/s | 0.35 s | $0.066 | $0.500 | 1.0M | mxfp4 | 
| ModelRun [by Modular] | 148 tok/s | 0.81 s | $0.170 | $0.670 | 1.0M | fp4 | 
| Parasail | 147 tok/s | 0.64 s | $0.300 | $1.20 | 1.0M | fp8 | 
| Fireworks Spot | 144 tok/s | 0.88 s | $0.150 | $0.600 | 1.0M | â | 
| Relace | 143 tok/s | 0.97 s | $0.120 | $0.500 | 1.0M | fp4 | 
| Modal | 141 tok/s | 0.72 s | $0.250 | $1.00 | 1.0M | â | 
| Wafer | 140 tok/s | 0.34 s | $0.036 | $0.500 | 1.0M | mxfp4 | 
| inference.net | 139 tok/s | 0.25 s | $0.080 | $0.666 | 1.0M | fp8 | 
| Cohere | 138 tok/s | 0.70 s | $0.115 | $0.400 | 1.0M | fp8 | 
| GMICloud | 138 tok/s | 0.88 s | $0.180 | $0.720 | 1.0M | fp8 | 
| Pearl Research | 137 tok/s | 1.17 s | $0.050 | $0.300 | 1.0M | fp8 | 
| inference.net | 137 tok/s | 0.41 s | $0.080 | $0.666 | 1.0M | fp8 | 
| Ionstream | 137 tok/s | 0.83 s | $0.210 | $0.620 | 1.0M | â | 
| StepFun | 132 tok/s | 1.02 s | $0.105 | $0.420 | 1M | fp8 | 
| Particle AI | 131 tok/s | 0.51 s | $0.060 | $0.500 | 1.0M | â | 
| DeepSeek | 131 tok/s | 1.22 s | $0.150 | $0.600 | 1.0M | â | 
| Makora | 127 tok/s | 0.84 s | $0.150 | $0.800 | 1.0M | fp8 | 
| Baseten | 127 tok/s | 0.69 s | $0.300 | $1.20 | 1.0M | â | 
| SiliconFlow | 125 tok/s | 0.94 s | $0.150 | $0.600 | 1.0M | fp8 | 
| Alibaba Cloud Int. | 121 tok/s | 1.15 s | $0.090 | $0.360 | 1M | â | 
| Baidu Qianfan | 117 tok/s | 0.77 s | $0.150 | $0.605 | 1.0M | fp8 | 
| Particle AI | 109 tok/s | 0.83 s | $0.030 | $0.250 | 1.0M | â | 
| DeepInfra | 108 tok/s | 1.13 s | $0.147 | $0.441 | 1.0M | fp8 | 
| Prime Intellect | 107 tok/s | 0.82 s | $0.044 | $0.700 | 1.0M | â | 
| NovitaAI | 105 tok/s | 1.18 s | $0.165 | $0.660 | 1.0M | fp8 | 
| Morph | 95 tok/s | 0.68 s | $0.059 | $0.520 | 1.0M | fp8 | 
| DeepInfra | 93 tok/s | 0.80 s | $0.098 | $0.294 | 1.0M | fp8 | 
| Ant Digital | 93 tok/s | 1.39 s | $0.120 | $0.480 | 1.0M | fp8 | 
| River | 91 tok/s | 1.01 s | $0.120 | $0.480 | 1.0M | â | 
| DekaLLM | 89 tok/s | 1.79 s | $0.100 | $0.600 | 1.0M | â | 
| Cloudflare | 87 tok/s | 2.79 s | $0.100 | $0.600 | 1.0M | â | 
| AtlasCloud | 80 tok/s | 3.02 s | $0.195 | $0.780 | 1.0M | fp8 | 
| DeepInfra | 80 tok/s | 2.47 s | $0.078 | $0.235 | 1.0M | fp8 | 
| Inceptron | 71 tok/s | 1.05 s | $0.100 | $0.600 | 1.0M | fp4 | 
| NEAR AI | 56 tok/s | 0.74 s | $0.300 | $1.20 | 1.0M | â | 
| StreamLake | 54 tok/s | 1.24 s | $0.111 | $0.444 | 1.0M | fp8 | 
| Ambient | 37 tok/s | 2.63 s | $0.045 | $0.250 | 1.0M | fp4 | 
| Reka AI | 36 tok/s | 1.10 s | $0.300 | $1.20 | 1.0M | â | 

## Usage

How much DeepSeek V4.1 Flash 1017 is used in practice, as the gateways that publish it report. Usage is not part of the Index. Most used models and labs.

A typical request over the last 30 days: 63.0k tokens per request, 71 input tokens per output token, 94% of input from cache, 67% of output is reasoning, 0.53 tool calls per request.

Apps sending it the most tokens on OpenRouter: Hermes Agent (12.0T), Claude Code (10.3T), Strix (8.53T), pi (8.33T), DeepSeek Harness (5.35T).

| Provider on Vercel AI Gateway | Throughput | First token | Uptime, 1 day | 
|---|---|---|---|
| Boundless | 468 tok/s | 0.86 s | 99.94% | 
| Fireworks | 419 tok/s | 0.78 s | 99.99% | 
| DigitalOcean | 256 tok/s | 0.79 s | 99.48% | 
| Parasail | 248 tok/s | 0.99 s | 100.00% | 
| Together AI | 235 tok/s | 0.69 s | 99.97% | 
| Relace | 228 tok/s | 1.25 s | 99.88% | 

Source: Usage: OpenRouter rankings Â· Vercel AI Gateway, CC BY 4.0 Â· data as of 20 Nov 2026.

## What a task costs

Estimates from list price, output speed and time to first answer for the best configuration. âWith cachingâ assumes three-quarters of the input is served from the prompt cache at $0.006 per 1M. Reasoning tokens are not modelled.

| Workload | Tokens in / out | Cost | With caching | Time | 
|---|---|---|---|---|
| Chat reply | 400 / 300 | $0.0005 | $0.0004 | 48.9 s | 
| Summarize a 30-page report | 12,000 / 600 | $0.0043 | $0.0017 | 50.8 s | 
| Code edit | 6,000 / 1,500 | $0.0036 | $0.0023 | 56.4 s | 
| Agentic coding session | 60,000 / 4,000 | $0.023 | $0.0096 | 1.2 min | 
| Structured extraction | 2,000 / 200 | $0.0008 | $0.0004 | 48.3 s | 

## More from DeepSeek

## Compare DeepSeek V4.1 Flash 1017 withâ¦

## See also

## Embed or cite

Show where DeepSeek V4.1 Flash 1017 ranks on a model card or README. The badge redraws itself with every daily update, carries the data date and links back to this page.

`[![DeepSeek V4.1 Flash 1017 on BenchLeader: #88 overall](https://www.benchleader.com/badge/deepseek-v4-1-flash-1017.svg)](https://www.benchleader.com/models/deepseek-v4-1-flash-1017)`
### Cite this

The ranking changes daily, so a reference carries the data date. APA style:

BenchLeader. (2026, November 20). DeepSeek V4.1 Flash 1017 on the BenchLeader Index (method v1.5) [Data set]. Retrieved November 20, 2026, from https://www.benchleader.com/models/deepseek-v4-1-flash-1017

BibTeX:

```
@misc{benchleader_deepseek_v4_1_flash_1017_20261120,
  author       = {{BenchLeader}},
  title        = {{DeepSeek V4.1 Flash 1017 on the BenchLeader Index (method v1.5)}},
  year         = {2026},
  month        = nov,
  howpublished = {\url{https://www.benchleader.com/models/deepseek-v4-1-flash-1017}},
  note         = {Data as of 2026-11-20}
}
```
## Questions people ask

- What are DeepSeek V4.1 Flash 1017's benchmark scores?
- As of 20 Nov 2026, DeepSeek V4.1 Flash 1017 (max effort) scores 56.8 Â±3.7 on the BenchLeader Index, strongest in long context (66) and weakest in knowledge (48). That rests on independent benchmark results only, and every score on this page links to its publisher.
- How does DeepSeek V4.1 Flash 1017 rank?
- DeepSeek V4.1 Flash 1017 (max effort) is #183 of 654 ranked configurations on 20 Nov 2026. 270 other configurations score within its margin, so the exact position matters less than the band. The leader is Claude Fable 5.2 (max effort) at 70.2.
- How much does DeepSeek V4.1 Flash 1017 cost per 1M tokens?
- As of 20 Nov 2026, DeepSeek V4.1 Flash 1017 costs $0.300 per 1M input tokens and $1.20 per 1M output tokens, $0.525 blended (three input tokens per output token).
- Is DeepSeek V4.1 Flash 1017 better than Muse Spark 1.3?
- As of 20 Nov 2026, DeepSeek V4.1 Flash 1017 (max) and Muse Spark 1.3 are neck and neck on quality (56.8 vs 61.0 on the BenchLeader Index, inside the Â±8.9 margin of error), and DeepSeek V4.1 Flash 1017 (max) costs 3.8Ã less, which makes it the better buy for most workloads. The head-to-head page has the benchmark-by-benchmark detail.
- When was DeepSeek V4.1 Flash 1017 released?
- DeepSeek released DeepSeek V4.1 Flash 1017 on 17 Oct 2026. BenchLeader has results for it at 6 reasoning-effort settings.

Data as of 20 Nov 2026. Compare these configurations.

Cite as: BenchLeader, âDeepSeek V4.1 Flash 1017: benchmarks, pricing, speed and rankâ, data as of 20 Nov 2026, https://www.benchleader.com/models/deepseek-v4-1-flash-1017.
