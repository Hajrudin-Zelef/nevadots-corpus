---
id: collect-261121-veille-ia/xiaomi-mimo/mimo-v2-6-pro-score-77-whatmodel-sh
title: "MiMo-V2.6-Pro: score 77 · whatmodel.sh"
domain: xiaomi-mimo
role: reference
task: article
actors: ["Xiaomi"]
dates: ["2026-09-22"]
keywords: ["mimo-v2", "beats", "score", "xiaomi", "whatmodel", "model", "flash", "test"]
source: https://whatmodel.sh/model/mimo-v2-6-pro
source_anchor: ""
source_lines: [1, 107]
section: "Xiaomi_MiMo"
---
whatmodel.sh / MiMo-V2.6-Pro

# MiMo-V2.6-Pro

## ↗ share

## + contribute

MiMo-V2.6-Pro is an open-weights language model from Xiaomi, released September 2026. Its published size is 1020B parameters (42B active per token) and a 1M-token context window. Licence: mit.

## Catalogue identity notes

Arena entry 'mimo-v2.6-pro' (webdev). Maker repo XiaomiMiMo/MiMo-V2.6-Pro-RL @73875d0: licence tag mit. Parameters: card: 'Sparse MoE, 1.02T total / 42B activated parameters'. Released 2026-09: Xiaomi MiMo model updates: '2026-09-22 MiMo-V2.6 Series Release mimo-v2.6-pro'. The webdev split (published 2026-09-24) predates the MiMo-V2.6-Pro-MOPD upload (2026-09-27), so the rated API model is taken as the launch checkpoint MiMo-V2.6-Pro-RL ('the flagship checkpoint of the MiMo-V2.6 series'); Xiaomi does not state which checkpoint the API id serves.

Score within the LMArena (text) comparison set only.

## $ how it compares

The LLMs ranked either side of MiMo-V2.6-Pro, out of 897 with a score, then more from Xiaomi.

| # | model | score | where |  |  | 
|---|---|---|---|---|---|
| 21 | GPT-5.5 | 78±0 | api $11.25/M |  |  | 
| 22 | Hy4 | 78±1 | api price unknown |  |  | 
| 23 | **MiMo-V2.6-Pro** | 77±1 | api $0.54/M +local |  |  | 
| 24 | DeepSeek V4.1 Flash 1017 | 76±1 | api $0.26/M |  |  | 
| 25 | GPT-5.6 Terra | 76±1 | api $4.50/M |  |  | 
|  | more from Xiaomi |  |  |  |  | 
| 38 | MiMo-V2.6-Flash (309B-A15B) | 75±1 | api $0.17/M +local |  |  | 
| 108 | MiMo-V2-Pro | 68±1 | comparison only |  |  | 
| 110 | MiMo-V2.5-Pro (1T-A42B) | 68±1 | api $0.65/M +local |  |  | 

## $ what it's good at

## agent tasks▁▁▃▆█▄▅▅▃▃▂▁ **#28** of 319 · 58% of its score · 8 tests

Beats GPT-5.5 here, though they rank higher overall. Trails GLM-5.3-Flash, GLM-5.3, DeepSeek V4.1 Flash here, though they rank lower overall.

| test | result | vs the others in its run | counts | 
|---|---|---|---|
| Arena Agent (October 2026 aggregate IPS) | 0.0240 IPS ✎ | ▁▁▁▃▃█▂▁▁▁·▁ beats 43/63 | 29% | 
| Terminal-Bench 4.0 lab | 34.9% ✎ | ▂·▁··▂▄▄▅█▇▄ beats 21/41 | 8.9% | 
| Terminal-Bench 4.1 | 27.0% ✎ | ▂▂▂··▅·▂▅█▃▆ beats 8/20 | 6.9% | 
| Terminal-Bench 2.1 lab | 89.9% ✎ | ▁·▁·▁▁▄▃▅█▆█ beats 66/68 | 4.5% | 
| OSWorld-Verified lab | 82.0% ✎ | █▃·▂▅▂▅▂▅▃▆▆ beats 24/28 | 3.7% | 
| Agents' Last Exam (metric/harness unspecified) lab | 31.6% ✎ | ▂·▂·█▄▂▂▂▂▄▄ beats 6/15 | 2.5% | 
| Toolathlon-Verified lab | 76.9% ✎ | ▂·▂▂▅▃···▃██ beats 14/19 | 1.5% | 
| DeepSWE v1.1 (resolved) lab | 71.9% ✎ | ▂····▃▁▁▄█▆█ beats 26/36 | 0.8% | 

## coding▁▁▁▃▆▇█▆▆▃▃▁ **#27** of 474 * · 18% of its score · 2 tests

## chat & writing▁▁▁▃▅▄▆█▇▇▇▃ **#26** of 405 * · 10% of its score · 1 test

## vision & documents▁▁▂▂▁▃▄▂▆▇█▄ **#53** of 154 * · 6.7% of its score · 1 test

## other tests3 not in a task · 7.6% of its score

| test | result | vs the others in its run | counts | 
|---|---|---|---|
| AutomationBench v1.0.6 lab | 53.1% ✎ | ▂···▂·▂▂··█▆ beats 7/10 | 2.8% | 
| SEC-Bench Pro (pass@1) lab | 66.3% ✎ | ▄·▄·▄·█▄···▄ beats 5/6 | 2.6% | 
| ExploitGym (pass@1) lab | 17.8% ✎ | ▄·▄··█·▄▄··▄ beats 5/6 | 2.2% | 

## $ where to run it

### Through an API

api · Xiaomi MiMo · $0.54/M · $0.435 in / $0.87 out per 1M tokens · 1M context · mimo.mi.com ↗ · price sourceStandard base token rate from models.dev (revision 6b32416, observed 2026-10-07). Provider-specific tiers, long-context surcharges, caching and promotions may differ.

Memory is what a version takes in GPU memory (or a Mac's unified memory) with an 8k-token conversation. With less, it runs partly from system RAM, more slowly. Enter your machine to see what runs at full speed.

### llama.cpp · recommended

version

1. Install llama.cpp · Homebrew and winget installs name the commands llama-cli and llama-server instead.macOS / Linux`curl -LsSf https://llama.app/install.sh | sh` Windows`irm https://llama.app/install.ps1 | iex`other ways to install ↗
2. Run MiMo-V2.6-Pro · needs 443 GB memory, 420.4 GB download · from huggingface.co/ggml-orgchat in the terminal`llama cli -hf ggml-org/MiMo-V2.6-Pro-RL-GGUF:Q2_K`
3. Or start a local server and open the address it prints (http://localhost:9931)`llama serve -hf ggml-org/MiMo-V2.6-Pro-RL-GGUF:Q2_K`

### LM Studio

version

1. Install LM Studio · Open the app once before using the lms command.download it (macOS / Windows / Linux) ↗
2. Run MiMo-V2.6-Pro · needs 443 GB memory, 420.4 GB download · from huggingface.co/ggml-orgopen in LM Studio ↗
3. Or download it in the terminal`lms get https://huggingface.co/ggml-org/MiMo-V2.6-Pro-RL-GGUF@Q2_K -y`
4. Then run lms ls to see its name, and lms chat followed by that name

Model repository and documentation: huggingface.co/XiaomiMiMo ↗

## $ how the score was worked out · LMArena leaderboard dataset 63%, Xiaomi MiMo official model ca… 29%, Terminal-Bench 4.0 official l… 7%

Each result implies a score on its own, after adjusting for how hard its test is and who ran it. The score is their weighted average, with a small pull toward the middle for thinly tested models. Each test's weight is its counts column above; here it's added up by who ran the tests. How it works

| source | results | share of the score | 
|---|---|---|
| LMArena leaderboard dataset | 4 | 63% | 
| Xiaomi MiMo official model cards and documentation · maker-reported | 9 | 29% | 
| Terminal-Bench 4.0 official leaderboard submissions | 1 | 7% | 
| prior · pull toward the middle |  | 0% | 
| weighted average |  | = 76.6 | 

## $ improve this page

Use a short GitHub form or edit the data through a pull request. Every number needs a source; here's which ones we can use.

data updated · sources and licences · json
