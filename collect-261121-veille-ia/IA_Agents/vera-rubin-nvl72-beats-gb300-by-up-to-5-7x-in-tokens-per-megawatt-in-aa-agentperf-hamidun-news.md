---
id: collect-261121-veille-ia/ia-agents/vera-rubin-nvl72-beats-gb300-by-up-to-5-7x-in-tokens-per-megawatt-in-aa-agentperf-hamidun-news
title: "Vera Rubin NVL72 beats GB300 by up to 5.7x in tokens per megawatt in AA-AgentPerf — Hamidun News"
domain: ia-agents
role: reference
task: article
actors: []
dates: []
keywords: ["rubin", "gb300", "deepseek", "tokens", "vera", "nvl72", "beats", "aa-agentperf"]
source: https://news.hamidun.com/en/news/13160/vera-rubin-nvl72-beats-gb300-by-up-to-5-7x-in-tokens-per-meg
source_anchor: ""
source_lines: [1, 46]
section: "IA_Agents"
---
# Vera Rubin NVL72 beats GB300 by up to 5.7x in tokens per megawatt in AA-AgentPerf

Artificial Analysis has added the first results for the NVIDIA Vera Rubin NVL72 rack (72 Rubin GPUs and 36 Vera CPUs) to AA-AgentPerf. In throughput per megawatt, it beats GB300 NVL72 by up to 5.7x on DeepSeek V4 Pro, up to 5.4x on DeepSeek V4.1 Flash, about 3.0x on GLM-5.2 and 2.8x on Kimi K3. At peak load the lead is more modest, from 1.14x to 1.45x. The fastest result on the leaderboard is 906 tok/s per user on DeepSeek V4.1 Flash.

AI-processed from Artificial Analysis; edited by Hamidun News
The first independent numbers for NVIDIA Vera Rubin NVL72 show a very uneven lead over its predecessor, depending on the operating regime. Artificial Analysis has added the rack to its AA-AgentPerf benchmark. In total throughput per megawatt of GPU power, Rubin beats GB300 NVL72 by up to 5.7x on DeepSeek V4 Pro (at ~190 tok/s per user), up to 5.4x on DeepSeek V4.1 Flash (~580 tok/s), 3.0x on GLM-5.2 (~165 tok/s) and 2.8x on Kimi K3 (~67 tok/s). At peak throughput, however, the gap is much smaller: 1.14x, 1.45x, 1.35x and 1.26x respectively.

## Why agentic coding is hard to serve

AA-AgentPerf does not run fixed prompts. It replays real trajectories of coding agents that solved tasks in public repositories: 500 trajectories and 18 997 requests. A request carries up to 132K input tokens, while the response is often just a short tool call. On average, 97% of the input repeats the previous request. One of the long trajectories grows from 10K to 116K tokens over 118 requests and sends the model 9.7M input tokens, of which only 116K are new.

As a result, everything comes down to KV-cache capacity and reuse, uneven prefill (from a few tokens to 69K per turn) and time to first token. Between requests, an agent waits up to 5 seconds for a tool call, and the cache has to survive that pause.

## What changed in the hardware

The rack keeps the GB300 design: 72 GPUs and 36 CPUs in a single NVLink domain. Memory per GPU is unchanged at 288 GB, but it is now HBM4.

- Memory bandwidth: 19.2 TB/s versus 8 TB/s on Blackwell Ultra (2.4x).
- NVLink 6: 3.0 TB/s versus 1.8 TB/s.
- Dense NVFP4 compute: 35 PFLOPS versus 15 PFLOPS (2.3x).
- CPU: Vera with 88 cores versus 72 on Grace, and an NVLink-C2C link to the GPU of 1.8 TB/s versus 0.9 TB/s. The KV-cache offloaded to CPU memory travels over this link.
- Measured GPU power under load: 0.5–1.9 kW versus 0.3–1.0 kW.

At peak, Rubin delivers 272M tokens/s per MW on DeepSeek V4 Pro (GB300: 239M), 571M on DeepSeek V4.1 Flash (395M), 272M on GLM-5.2 (201M) and 70M on Kimi K3 (56M). The 571M figure is a record for the entire leaderboard. Per GPU, the advantage is wider: 2.0–2.5x at peak and up to ~18x on DeepSeek V4.1 Flash at ~580 tok/s.

The gap widens where GB300 falls apart. On DeepSeek V4 Pro, going from ~120 to ~165 tok/s per user costs GB300 81% of its throughput per MW, while Rubin loses only 16%. Rubin also sets speed records: 906 tok/s on DeepSeek V4.1 Flash (faster than any result for any model), 408 tok/s on GLM-5.2 and 255 tok/s on Kimi K3.

## Where the picture is less clear-cut

The results come with caveats, and Artificial Analysis lists them. Rubin's median time to first token on DeepSeek V4 Pro at ~165 tok/s is 1.65 s, versus 1.08 s for GB300. If you measure end-to-end speed, which includes that wait, GB300 matches or beats Rubin on throughput per MW in most regimes.

NVIDIA's GB300 racks reach only ~163 tok/s per user on DeepSeek V4 Pro. Beyond that, the GB300 line is built from small 4-GPU configurations assembled by Artificial Analysis itself. Above ~255 tok/s, these configs deliver more tokens per MW than Rubin.

On DeepSeek V4.1 Flash, NVIDIA capped responses at 2 048 tokens, while other results use a limit of 16 384. About 2% of responses hit the cap, and the average response length is roughly a tenth shorter. This slightly favors Rubin. In addition, the benchmark measures the power of the GPUs themselves, with HBM, but not CPUs, networking or cooling.

## What to check before buying

Rubin's advantage is strongest where the product depends on high per-user speed: fast agents with short responses and a long cached context. If you are constrained by your data center's power limit rather than the number of accelerators, the rule of thumb is this: at ~165 tok/s per user, the same power budget yields roughly 5x more DeepSeek V4 Pro tokens than on GB300.

Before deciding, compare against your own target metric: end-to-end speed or speed after the first token. Also check your sensitivity to startup latency: for interactive scenarios, a 0.5-second difference is noticeable. Every point on the leaderboard comes with an artifact: the serving configuration in the config browser. You can use it as a starting point for tuning without running your own performance study.

### The AI world, distilled — once a week

Seven stories that actually mattered, hand-picked. No noise, no reposts, no press releases.

Done! Check your inbox for a confirmation.
