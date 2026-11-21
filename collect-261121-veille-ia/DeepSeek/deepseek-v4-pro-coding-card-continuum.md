---
id: collect-261121-veille-ia/deepseek/deepseek-v4-pro-coding-card-continuum
title: "DeepSeek V4 Pro coding card - Continuum"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: ["2026-08-19"]
keywords: ["deepseek", "artificial", "analysis", "deepswe", "chip", "printed", "source", "intelligence"]
source: https://continuumcode.ai/models/deepseek/deepseek-v4-pro/
source_anchor: ""
source_lines: [1, 128]
section: "DeepSeek"
---
- bench
- DeepSWE
- version
- v1.1
- split
- public 113 tasks
- harness
- mini-swe-agent
- effort
- max
- n
- 113
- metric
- Pass@1
- value
- 63%
- ci
- ±6%
- $/task
- $0.24
- tokens
- 106k out
- steps
- 155

Canonical current weights are 0813. OpenRouter also still lists `deepseek/deepseek-v4-pro` as 0423 preview. That is a lineage node, not this card.

MIT license on the official Hugging Face repo. Continuum hosted allowlist names `deepseek-v4-flash` only, now free via OpenCode Zen. We will not invent a Pro hosted id.

HF `config.json` (sha 72e1d323, fetched 19 Aug 2026): `DeepseekV4ForCausalLM`, 61 layers, hidden 7168, 384 routed experts, 6 experts/token, 1 shared expert, YaRN to 1,048,576, DSpark speculative block attached (`dspark_block_size` 5). No Jinja chat template: the repo ships an `encoding/` folder instead.

Use Pro when you want an open checkpoint and a published DeepSWE cost under a dollar. Official board: 63% Pass@1 ±6% at max, $0.24 per task, 106k output, 155 steps. The interval is the widest among the top rows. Treat 63% as a band, not a point.

Against Opus (74% ±4, $11.84) you are buying price and weights, not the same pass rate. Against Flash (53% ±4, $0.10) you are buying the Pro checkpoint and about ten points, still at cents.

vals.ai V4 Pro 0813 card, fetched 19 Aug 2026: Index 52.37% ±1.14, $3.376 per Index test, 58 min 18 s latency. The same page’s standout prose (not a hero chip) names SWE-bench Verified 96.40% (#2 of 82; highest open-weight on that board) and Terminal-Bench 2.1 54.68% (#33 of 52; 28.89% on hard tasks) plus EMB 52.80% (#24 of 37). SWE-bench Verified stays a footnote. It is not the hero and it is not DeepSWE.

Ranked view: Best coding models: the independent leaderboard puts this row and every other card that carries an independent coding score on one board, with the confidence intervals left visible.

Copied from AA or vals HTML we opened. 0.0% placeholder bars are omitted. Do not average these into the hero tiles, and do not invent CI, $/task, or steps for Artificial Analysis.

| Bench | Printed | Note | Source | URL | As of | 
|---|---|---|---|---|---|
| GDPval-AA v2 | 54.5% | Printed on the AA Intelligence Evaluations grid (Artificial Analysis DeepSeek V4 Pro). Not a DeepSWE chip. | Artificial Analysis DeepSeek V4 Pro | source | 2026-08-19 | 
| τ³-Banking | 39.6% | Printed on the AA Intelligence Evaluations grid (Artificial Analysis DeepSeek V4 Pro). Not a DeepSWE chip. | Artificial Analysis DeepSeek V4 Pro | source | 2026-08-19 | 
| Terminal-Bench v2.1 | 78.7% | Printed on the AA Intelligence Evaluations grid (Artificial Analysis DeepSeek V4 Pro). Not a DeepSWE chip. | Artificial Analysis DeepSeek V4 Pro | source | 2026-08-19 | 
| SciCode | 49.2% | Printed on the AA Intelligence Evaluations grid (Artificial Analysis DeepSeek V4 Pro). Not a DeepSWE chip. | Artificial Analysis DeepSeek V4 Pro | source | 2026-08-19 | 
| Humanity's Last Exam | 41% | Printed on the AA Intelligence Evaluations grid (Artificial Analysis DeepSeek V4 Pro). Not a DeepSWE chip. | Artificial Analysis DeepSeek V4 Pro | source | 2026-08-19 | 
| GPQA Diamond | 92.8% | Printed on the AA Intelligence Evaluations grid (Artificial Analysis DeepSeek V4 Pro). Not a DeepSWE chip. | Artificial Analysis DeepSeek V4 Pro | source | 2026-08-19 | 
| CritPt | 18% | Printed on the AA Intelligence Evaluations grid (Artificial Analysis DeepSeek V4 Pro). Not a DeepSWE chip. | Artificial Analysis DeepSeek V4 Pro | source | 2026-08-19 | 
| AA-Omniscience Accuracy | 49.1% | Printed on the AA Intelligence Evaluations grid (Artificial Analysis DeepSeek V4 Pro). Not a DeepSWE chip. | Artificial Analysis DeepSeek V4 Pro | source | 2026-08-19 | 
| AA-LCR | 75.3% | Printed on the AA Intelligence Evaluations grid (Artificial Analysis DeepSeek V4 Pro). Not a DeepSWE chip. | Artificial Analysis DeepSeek V4 Pro | source | 2026-08-19 | 

Plus is $25/mo with $25 weekly hosted usage. The Mac app stays free with your own keys. Get Plus is not Download for Mac.

Preview / prior weights: Prior weights: DeepSeek-V4-Pro (0423 lineage).

Community quants: Any GGUF or AWQ you find under other orgs is community, not official.

DSpark speculative decoding ships in the same checkpoint. vLLM: `--speculative-config` method dspark, 7 speculative tokens. SGLang: `--speculative-algorithm DSPARK`. Do not invent a separate draft repo.

Vendor DeepSWE 62.7 on the HF README is DeepSeek Harness / max / temp 1.0 / top_p 0.95. The hero chip above is the official DeepSWE board: 63% ±6% on mini-swe-agent, $0.24/task, 155 steps. Those are different harnesses.

This is the model repo, not Download for Mac. Downloads and likes from the Hugging Face API on 2026-08-19.

usedStorage and the sum of `.safetensors` sizes can differ (LFS pointers, non-shard files, duplicate copies). Cited separately. Source: recursive tree API, 2026-08-19.

| Path | Bytes | 
|---|---|
| `model-00001-of-00066.safetensors` | 1,853,358,176 | 
| `model-00002-of-00066.safetensors` | 13,874,726,024 | 
| `model-00003-of-00066.safetensors` | 13,874,726,024 | 
| `model-00004-of-00066.safetensors` | 13,910,006,752 | 
| `model-00005-of-00066.safetensors` | 13,868,522,072 | 
| `model-00006-of-00066.safetensors` | 13,903,802,792 | 
| `model-00007-of-00066.safetensors` | 13,868,522,072 | 
| `model-00008-of-00066.safetensors` | 13,903,802,792 | 
| `model-00009-of-00066.safetensors` | 13,868,522,072 | 
| `model-00010-of-00066.safetensors` | 13,903,802,792 | 
| `model-00011-of-00066.safetensors` | 13,868,522,072 | 
| `model-00012-of-00066.safetensors` | 13,903,805,136 | 

Showing the first 12 of 66 safetensor shards. The tree also holds 26 non-shard files, none of them listed here. The snapshot was capped at 12 shards when the tree API was read on 2026-08-19, so the remaining 54 are on the Files tab, not on this page. Totals above are the full tree API sum, not this slice. Byte sizes are Hub tree API `size` fields, never invented.

Not listed as a Continuum hosted id on 19 Aug 2026. First-party and OpenRouter `deepseek/deepseek-v4-pro-0813` are the hosts we will name.

If you want Continuum-hosted DeepSeek, the live id is Flash, not Pro.

We list first-party and OpenRouter, plus Continuum only when the live public allowlist named the id. This is not a 15-host routing table.

DeepSWE official: **$0.24 per task** at max. That is the hero number (106k out, 155 steps).

First-party DeepSeek API docs, fetched 19 Aug 2026: one USD schedule with peak and off-peak. Peak hours 01:00–04:00 and 06:00–10:00 UTC; all other hours off-peak at half. Pro cache-hit / cache-miss / output: peak **$0.044 / $1.32 / $3.96** per million; off-peak **$0.022 / $0.66 / $1.98**. Concurrency cap 500 (Flash is 2500).

OpenRouter catalog 19 Aug 2026 for `deepseek-v4-pro-0813` lists the same windows as UTC overrides: off-peak prompt/completion/cache-read $0.66 / $1.98 / $0.022, peak $1.32 / $3.96 / $0.044. Artificial Analysis printed the peak miss/out pair ($1.32 / $3.96) and $0.25 per AA task at 77 tok/s. Those are not two mystery tables.

Artificial Analysis Intelligence Index 53 (v4.1.1 = nine named evals), fetched 19 Aug 2026. The nine printed Index percents from live `currentModel` sit in the named-benches table, not as extra hero chips. AA also printed 1600B / 49B active, $1.32 / $3.96 (peak miss/out), $0.25 per AA task, 77 tok/s, 130M Index output tokens. Do not average AA cost with DeepSWE $0.24/task.

| Claim | Source | As of | 
|---|---|---|
| DeepSWE 63% ±6% at max | DeepSWE official board | 2026-08-13 | 
| Official Pro peak $0.044 / $1.32 / $3.96 and off-peak $0.022 / $0.66 / $1.98; peak 01:00–04:00 and 06:00–10:00 UTC | DeepSeek API Models & Pricing | 2026-08-19 | 
| OR catalog UTC overrides match that peak/off-peak schedule | OpenRouter catalog deepseek-v4-pro-0813 | 2026-08-19 | 
| HF safetensors.total 1,650,497,936,906; usedStorage 1,781,787,608,346 bytes; 66 shards; likes 625; created 2026-08-13T03:05:06Z | Hugging Face API DeepSeek-V4-Pro-0813 | 2026-08-19 | 
| vals Index 52.37% ±1.14; $3.376/test; 58 min 18 s; SWE-Verified 96.40% footnote only; Terminal-Bench 2.1 54.68% | vals.ai DeepSeek V4 Pro 0813 | 2026-08-19 | 
| AA 1600B / 49B active; Index 53; $1.32 / $3.96; $0.25/AA task; 77 tok/s; nine currentModel Index percents in named-benches | Artificial Analysis DeepSeek V4 Pro | 2026-08-19 | 
| Vendor DeepSWE 62.7 is DeepSeek Harness / max, not the official board | HF README DeepSeek-V4-Pro-0813 | 2026-08-19 | 
| Not a Continuum hosted id | GET /v1/chat/hosted/models/public | 2026-08-19 | 
| OpenRouter id deepseek/deepseek-v4-pro-0813, context 1,048,576 | OpenRouter /api/v1/models | 2026-08-19 | 
| HF downloads 37,583 | Hugging Face API deepseek-ai/DeepSeek-V4-Pro-0813 | 2026-08-19 | 
| 66 safetensor shards; shard bytes 892,744,322,880; tree files 92 | Hugging Face tree API deepseek-ai/DeepSeek-V4-Pro-0813 | 2026-08-19 | 
| Spaces API returned 2; official collection DeepSeek-V4 (4 items, 833 upvotes) | Hugging Face Spaces / collections API | 2026-08-19 | 

Same official DeepSWE harness (mini-swe-agent, public 113 tasks), fetched 2026-08-19. Different effort labels are the lab's own setting on that board, shown here rather than normalized.

Continue through DeepSeek's model family with DeepSeek V4 Flash.

0813 is current weights. 0423 is the preview id that kept the unversioned slug. This card is 0813 only.

The live public allowlist on 19 Aug 2026 named `deepseek-v4-flash` and did not name Pro. We report that, we do not invent a Pro id.

62.7 is DeepSeek Harness / max / temp 1.0 on the vendor README. 63% ±6 is the official DeepSWE mini-swe-agent board ($0.24/task, 155 steps). Different harness. The hero chip is the official board.

No. Official DeepSeek docs publish one peak / off-peak USD schedule. AA printed peak miss/out. OpenRouter’s default list is the off-peak miss/out pair, with UTC overrides for the peak windows.

Lab posts pick a harness, an effort, a split, and sometimes a private eval set. DeepSWE publishes the official mini-swe-agent row with cost, tokens, and steps. Scale SWE-bench Pro only counts when the public shared-harness board has a row we can fetch. A higher lab number is usually a different test, not a better one.

No Continuum hosted id is verified for this model, so this page does not invent a curl target. Use the first-party API or the OpenRouter slug `deepseek/deepseek-v4-pro-0813`.
