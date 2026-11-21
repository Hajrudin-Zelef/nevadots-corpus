---
id: collect-261121-veille-ia/ia-agents/vera-rubin-nvl72-vs-gb300-nvl72-agentx-results-inferencex
title: "Vera Rubin NVL72 vs GB300 NVL72: AgentX Results | InferenceX"
domain: ia-agents
role: reference
task: article
actors: []
dates: []
keywords: ["rubin", "nvl72", "vera", "gb300", "nvlink", "pflop", "user", "agentx"]
source: https://inferencex.semianalysis.com/hardware/vera-rubin-nvl72-vs-gb300-nvl72-agentx-results
source_anchor: ""
source_lines: [1, 135]
section: "IA_Agents"
---
| Model | tok/s/user | VR / GB300 per $ | Largest VR / GB300 per $ (tok/s/user) | 
|---|---|---|---|
| DeepSeek-V4-Pro | 100 | 1.90x | ~8.3x (~183) | 
| MiniMax-M3 | 100 | 1.93x | ~10.2x (~337) | 
| Qwen3.8-Flash-Next | 100 | 2.03x | ~4.2x (~176) | 
| GLM-5.2 | 120 | 1.81x | ~3.9x (~322) | 

AgentX replays real Claude Code sessions as deterministic AIPerf workloads. AgentX v1.0 has 393 published sessions, the median request in the full v1.0 corpus has 142,016 input tokens and 444 output tokens, and each run is profiled for 1 hour after a seeded warmup and prefix primers. The x-axis of every chart is p90 interactivity, 1 / p90 inter-token latency, in tok/s/user. The y-axis is the dashboard's default metric, total tokens per $1 TCO: total token throughput, which counts input tokens (including KV-cache hits) and output tokens, divided by the hourly cost of the GPUs serving it. Input is ~99% of that total.

Per-dollar figures use the hyperscaler owning cost from the SemiAnalysis AI Cloud TCO Model: $3.61/GPU/hr for Vera Rubin NVL72 and $2.31/GPU/hr for GB300 NVL72. InferenceX's external and internal TCO bases differ only for TPU v7, so both systems cost the same on either basis and every per-dollar ratio in this article holds on both. Per-MW figures divide throughput by the all-in power provisioned per GPU in the SemiAnalysis Datacenter Industry Model: 3.3 kW for Vera Rubin NVL72 and 2.12 kW for GB300 NVL72. These are planning figures for a deployed rack, including CPUs, networking and cooling. They are not measured draw during these runs.

Matched-interactivity values are interpolated along each system's Pareto frontier, with no extrapolation past a curve's measured ends. GLM-5.2 is compared at 120 tok/s/user because its Vera Rubin curve starts at 111.5 tok/s/user. At each target the comparison uses the strongest GB300 NVL72 line at that point, whichever engine that is. Per dollar is the primary metric in the sections below and the only ratio in the results tables, since each per-MW ratio of Vera Rubin to GB300 NVL72 is within 0.4% of the matching per-dollar ratio.

Charts load live dashboard data and can include newer runs than the tables and numbers in the text.

Vera Rubin NVL72 keeps the 72-GPU NVLink domain and MGX rack design of GB300 NVL72. Each Rubin GPU has the same 288 GB of HBM capacity and moves to HBM4 and NVLink 6.

| Per GPU | Rubin (Vera Rubin NVL72) | Blackwell Ultra (GB300 NVL72) | Rubin / BU | 
|---|---|---|---|
| HBM capacity | 288 GB HBM4 | 288 GB HBM3E (278 GB usable) | 1.00x | 
| HBM bandwidth | 19.2 TB/s | 8 TB/s | 2.40x | 
| Dense NVFP4 | 35 PFLOP/s | 15 PFLOP/s | 2.33x | 
| Dense FP8 | 17.5 PFLOP/s | 5 PFLOP/s | 3.50x | 
| Dense BF16 | 4 PFLOP/s | 2.5 PFLOP/s | 1.60x | 
| NVLink per GPU (uni-di) | 1.5 TB/s (NVLink 6) | 900 GB/s (NVLink 5) | 1.67x | 
| Scale-up world size | 72 | 72 | 1.00x | 
| TCO, owning at hyperscaler volume | $3.61/GPU/hr | $2.31/GPU/hr | 1.56x | 
| All-in power per GPU (provisioned) | 3.3 kW | 2.12 kW | 1.56x | 

Rubin values come from the Rubin GPU column of NVIDIA's Vera Rubin NVL72 specifications, which also lists 50 PFLOP/s per GPU of NVFP4 inference compute, footnoted as a sparse specification. NVIDIA's Rubin page says Vera Rubin NVL72 has a new Transformer Engine with adaptive compression to boost NVFP4 inference performance. NVIDIA quotes NVLink 6 at 3 TB/s per GPU, counting both directions, and 1.7x NVLink 5's 1.8 TB/s on the same basis. Blackwell Ultra values come from the InferenceX GPU specs. TCO and power are the figures the dashboard uses.

Divided by the 1.56x cost ratio, the silicon ratios give Rubin 1.54x per dollar on HBM bandwidth, 1.49x on dense NVFP4 compute and 2.24x on dense FP8 compute. The per-dollar leads in the Summary table are above the HBM-bandwidth and NVFP4 figures and below the FP8 figure.

Our previous Vera Rubin article reported up to 7x better token throughput per megawatt for Vera Rubin NVL72 on DeepSeek-V4-Pro with pre-release software, including ~7.2x over GB300 NVL72 SGLang at 150 tok/s/user and 2.09x over the best GB300 NVL72 line at 100 tok/s/user. Both systems have newer runs since. The per-MW lead is now 6.92x at 150 tok/s/user. At 100 tok/s/user it fell from 2.09x to 1.90x, because GB300 NVL72's best line there rose from 28.5M to 44.5M tok/s/MW and Vera Rubin's from 59.4M to 84.6M.

On DeepSeek-V4-Pro the largest ratio to GB300 NVL72 is ~8.3x at ~183 tok/s/user, per dollar and per MW. Above 137 tok/s/user every GB300 NVL72 frontier point serves 4 agents or fewer. At ~183 tok/s/user the strongest GB300 NVL72 line is Dynamo SGLang, whose value is interpolated between a 120-agent disaggregated config at 137 tok/s/user and a 4-agent config at 241 tok/s/user.

DeepSeek-V4-Pro has 1.6T total and 49B active parameters, with a hybrid of compressed sparse attention and heavily compressed attention that keeps its KV cache small at long context (model card; see the model page). Vera Rubin runs Dynamo TensorRT-LLM with FP4 weights and MTP, with prefill/decode disaggregation on 26 to 56 GPUs per configuration.

The chart below shows total tokens per $1 TCO for both systems on DeepSeek-V4-Pro:

As you can see, the newer Vera Rubin Dynamo TRT-LLM run sits above the earlier TRT-LLM run behind the previous article at every interactivity both runs cover, and its peak is 35% higher. The chart opens on GB300 NVL72's vLLM and TensorRT-LLM lines because the dashboard does not overlay vLLM and SGLang lines for the same chip on one AgentX chart. The table uses the strongest GB300 NVL72 line at each target, which is Dynamo SGLang at 150 and 200 tok/s/user. Select it in the legend to see it.

Values are M tokens per $1 TCO, and the ratio is Vera Rubin / GB300.

| tok/s/user | Vera Rubin | GB300 | Ratio | 
|---|---|---|---|
| 90 | 282.5 | 176.8 (Dynamo vLLM) | 1.60x | 
| **100** | **278.6** | **147.0 (Dynamo vLLM)** | **1.90x** | 
| 125 | 260.2 | 70.8 (Dynamo vLLM) | 3.67x | 
| 150 | 226.1 | 32.8 (Dynamo SGLang) | 6.89x | 
| 200 | 93.8 | 12.7 (Dynamo SGLang) | 7.37x | 
| 250 | 7.6 | 6.7 (Dynamo vLLM) | 1.13x | 

At peak throughput Vera Rubin NVL72 serves 284.6M tokens per $ at 80 tok/s/user against 244.1M for GB300 NVL72 Dynamo vLLM at 56 tok/s/user, a 1.17x lead per dollar (1.82x per GPU). At 250 tok/s/user Vera Rubin is serving ~30 agents and GB300 NVL72 one, and the lead falls to 1.13x per dollar. Above ~256 tok/s/user GB300 NVL72 Dynamo vLLM serves more tokens per dollar than Vera Rubin (0.92x at 260 and 0.65x at 290). The fastest Vera Rubin point reaches 310 tok/s/user and the fastest GB300 NVL72 point reaches 298 tok/s/user.

On MiniMax-M3 the largest per-dollar ratio to GB300 NVL72 is ~10.2x at ~337 tok/s/user, the fastest GB300 NVL72 point, a disaggregated Dynamo vLLM config serving 1 agent. The next-fastest GB300 NVL72 point, at 271 tok/s/user, serves 24 agents, and Vera Rubin's value at ~337 tok/s/user is interpolated between its measured points at 244 and 413 tok/s/user.

MiniMax-M3 has ~428B total and ~23B active parameters with MiniMax Sparse Attention (model card; see the model page). Vera Rubin runs Dynamo vLLM with FP4 and MTP. Its highest-throughput points come from aggregated 2-GPU TP2 workers, and its highest-interactivity points from a 10-GPU disaggregated config.

The chart below shows total tokens per $1 TCO for both systems on MiniMax-M3:

As you can see, GB300 NVL72's only MiniMax-M3 AgentX line, from an older run, starts at 99 tok/s/user, so there is no GB300 NVL72 high-throughput point to compare against Vera Rubin's 136.6M tokens per $ peak. At the other end, Vera Rubin reaches 483 tok/s/user with its 10-GPU disaggregated config.

Values are M tokens per $1 TCO, GB300 runs Dynamo vLLM in every row, and the ratio is Vera Rubin / GB300.

| tok/s/user | Vera Rubin | GB300 | Ratio | 
|---|---|---|---|
| **100** | **132.3** | **68.4** | **1.93x** | 
| 150 | 114.3 | 54.5 | 2.10x | 
| 200 | 91.0 | 38.4 | 2.37x | 
| 300 | 54.5 | 14.4 | 3.78x | 

B300 TensorRT-LLM at TP2 was the leading MiniMax-M3 system at 100 tok/s/user before the Vera Rubin and MI355X ATOM runs (background). It serves 106.4M tokens per $ there, and Vera Rubin's lead over it is 1.24x per dollar at 100, 1.76x at 150 and 2.14x at 200 tok/s/user.

MI355X on AMD's ATOM engine is now the strongest other MiniMax-M3 system per dollar at 100 tok/s/user, at 120.7M tokens per $. Vera Rubin leads it by 1.10x there and 1.32x at 200 tok/s/user, and stays ahead at every interactivity both curves cover. Per MW the lead at 100 tok/s/user is 1.67x, because MI355X's cost per GPU-hour is low relative to its provisioned power.

On Qwen3.8-Flash-Next the largest per-dollar ratio to GB300 NVL72 is ~4.2x at ~176 tok/s/user. There the GB300 NVL72 SGLang value is interpolated between configs serving 4 agents at 160 tok/s/user and 2 agents at 203 tok/s/user, and the Vera Rubin value between its measured points at 173 and 271 tok/s/user.

Qwen3.8-Flash-Next is a 176B-parameter model (a 125B main model plus a 51B n-gram embedding table) with 6B active parameters and a Gated DeltaNet and sparse-attention hybrid (model card; see the model page). Vera Rubin runs Dynamo SGLang with FP4 and MTP on 1 to 6 GPUs per config. GB300 NVL72 runs single-GPU SGLang.

The chart below shows total tokens per $1 TCO for both systems on Qwen3.8-Flash-Next:

As you can see, Vera Rubin has no measured point between 24 and 130 tok/s/user, so its values at 50 and 100 tok/s/user are interpolated across that span. Its first measured point above 100, at 130 tok/s/user, serves 190.9M tokens per $, which is 1.85x GB300 NVL72's value at 100.

Values are M tokens per $1 TCO, GB300 runs SGLang in every row, and the ratio is Vera Rubin / GB300.

| tok/s/user | Vera Rubin | GB300 | Ratio | 
|---|---|---|---|
| 50 | 226.4 | 182.6 | 1.24x | 
| **100** | **209.6** | **103.1** | **2.03x** | 
| 150 | 167.5 | 50.4 | 3.32x | 
| 200 | 109.6 | 27.0 | 4.06x | 

At peak throughput both systems run one GPU per server. Vera Rubin delivers 1.64x the throughput per GPU but costs 1.56x as much per GPU-hour, so its lead per dollar is 1.05x (231.6M vs 220.3M tokens per $).

Above 25 tok/s/user every Vera Rubin frontier point is a disaggregated prefill/decode config, and the GB300 NVL72 results have no disaggregated Qwen3.8-Flash-Next line. Against B200 SGLang, the strongest other Qwen3.8-Flash-Next system per dollar at 100 tok/s/user, Vera Rubin's lead is 1.75x.

On GLM-5.2 the largest per-dollar ratio to GB300 NVL72 is ~3.9x at ~322 tok/s/user. Every GB300 NVL72 point above 246 tok/s/user serves 1 agent, and the strongest GB300 NVL72 value at ~322 tok/s/user is interpolated between two such Dynamo SGLang configs, at 283 tok/s/user and at 342 tok/s/user, the fastest GB300 NVL72 point.

GLM-5.2 has ~753B parameters in its checkpoint, with DeepSeek-style sparse attention and 8 of 256 routed experts active per token (model page). The dashboard groups GLM-5.2 and GLM-5.3, which share a base model. The Vera Rubin run uses the GLM-5.3 checkpoint and the GB300 NVL72 runs use GLM-5.2. Vera Rubin runs Dynamo TensorRT-LLM with FP4 and MTP, disaggregated on 12 to 28 GPUs per config.

The chart below shows total tokens per $1 TCO for both systems on GLM-5.2:

As you can see, Vera Rubin has no measured point between 111 and 262 tok/s/user, so its values at 150 and 200 tok/s/user are interpolated across that span. Vera Rubin's fastest GLM-5.2 point reaches 459 tok/s/user.

Values are M tokens per $1 TCO, and the ratio is Vera Rubin / GB300.

| tok/s/user | Vera Rubin | GB300 | Ratio | 
|---|---|---|---|
| **120** | **110.2** | **60.8 (Dynamo SGLang)** | **1.81x** | 
| 150 | 89.0 | 35.2 (Dynamo SGLang) | 2.53x | 
| 200 | 57.2 | 22.6 (Dynamo SGLang) | 2.53x | 
| 300 | 23.1 | 6.9 (Dynamo TRT-LLM) | 3.36x | 

Vera Rubin's peak is 116.3M tokens per $ at 111 tok/s/user against 99.2M for GB300 NVL72 Dynamo SGLang at 89 tok/s/user, 1.17x per dollar and 1.83x per GPU. At 300 tok/s/user, B200 Dynamo SGLang is the strongest system per dollar other than Vera Rubin, at 11.8M tokens per $ against 6.9M for the best GB300 NVL72 line. Vera Rubin's per-dollar lead over B200 there is 2.0x.

The table uses the strongest GB300 NVL72 line at each target: Dynamo SGLang at 120, 150 and 200 tok/s/user and Dynamo TensorRT-LLM at 300. Both lines use FP4 and MTP, as Vera Rubin does. The Dynamo SGLang run is 7 days newer than the Vera Rubin run and uses an SGLang nightly build. The Dynamo TensorRT-LLM run is 1 day older and uses a TensorRT-LLM 1.3.0rc28 dev build from NVIDIA's release registry, where Vera Rubin uses an internal TensorRT-LLM build.

First, engines differ and some runs are older. Vera Rubin runs Dynamo TRT-LLM on DeepSeek-V4-Pro and GLM-5.2, Dynamo vLLM on MiniMax-M3 and Dynamo SGLang on Qwen3.8-Flash-Next. The best GB300 NVL72 line at a given point can be a different engine, and some GB300 lines use a draft model for speculative decoding where Vera Rubin uses MTP. The GB300 NVL72 MiniMax-M3 results come from an older run. Second, Vera Rubin software is early, and the dashboard labels the Vera Rubin results an official preview. The images are internal TensorRT-LLM and vLLM builds and an SGLang nightly, and Vera Rubin's DeepSeek-V4-Pro curve moved 43% at 100 tok/s/user in under four weeks. Third, cost and power are modeled. The per-dollar numbers use modeled TCO per GPU-hour and the per-MW numbers use planning figures for all-in power per GPU. Neither comes from billing or telemetry for these runs. A different Vera Rubin cost or power figure would scale every Vera Rubin per-dollar or per-MW ratio in this article by the same factor. Fourth, some ratios span coverage gaps. Where a curve has no measured config across a wide span of interactivity, values inside that span are interpolated, and a ratio there measures the gap as well as the hardware. This is the main reason for the 6.9x to ~8.3x figures on DeepSeek-V4-Pro from 150 to 200 tok/s/user. Vera Rubin's own curves have gaps on DeepSeek-V4-Pro (97 to 165 and 230 to 310 tok/s/user), MiniMax-M3 (128 to 244 and 244 to 413 tok/s/user), Qwen3.8-Flash-Next (24 to 130 and 173 to 271 tok/s/user) and GLM-5.2 (111 to 262 tok/s/user).

The Vera Rubin NVL72 configurations behind each curve:

|  | DeepSeek-V4-Pro | MiniMax-M3 | Qwen3.8-Flash-Next | GLM-5.2 | 
|---|---|---|---|---|
| Serving stack | Dynamo TensorRT-LLM | Dynamo vLLM | Dynamo SGLang | Dynamo TensorRT-LLM | 
| Precision | FP4 | FP4 | FP4 | FP4 | 
| Speculative decoding | MTP | MTP | MTP | MTP | 
| Prefill and decode | Disaggregated, decode TP4 to TP32 with EP | Aggregated TP2 at peak, disaggregated at 400+ tok/s/user | Aggregated TP1 at peak, disaggregated above 25 tok/s/user | Disaggregated, decode TP4, or TP8 with EP | 
| KV cache offload | Off | On | On | On | 
| GPUs per config | 26 to 56 | 2 to 10 | 1 to 6 | 12 to 28 | 

Kimi K3 and DeepSeek-V4.1-Flash have no Vera Rubin AgentX results yet. Kimi K3 already has AgentX results on GB300 NVL72, MI355X and TPU v7. A GB300 NVL72 DeepSeek-V4-Pro config serving more than 4 agents at 150 to 200 tok/s/user would replace the interpolated GB300 NVL72 values behind the DeepSeek-V4-Pro ratios in that range. The per-MW results use provisioned power, and none of the Vera Rubin runs here carry valid GPU power telemetry.

All articles and posts are © SemiAnalysis. All rights reserved. The AGPL-3.0 license covering the application source code does not apply to article content.
