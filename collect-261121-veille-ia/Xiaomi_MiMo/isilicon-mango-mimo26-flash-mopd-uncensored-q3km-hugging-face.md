---
id: collect-261121-veille-ia/xiaomi-mimo/isilicon-mango-mimo26-flash-mopd-uncensored-q3km-hugging-face
title: "isilicon/mango-mimo26-flash-mopd-uncensored-q3km · Hugging Face"
domain: xiaomi-mimo
role: reference
task: article
actors: ["Xiaomi"]
dates: []
keywords: ["wilson", "score", "agent", "local", "mango", "matched", "workload", "mechanical"]
source: https://huggingface.co/isilicon/mango-mimo26-flash-mopd-uncensored-q3km
source_anchor: ""
source_lines: [1, 230]
section: "Xiaomi_MiMo"
---
**Q3_K_M · spiritfather / Yamz** · Measured configuration

**79.39 / 100** Mango weighted score · **220.28 min** matched workload · **12/12** mechanical agent passes

The selected Q3_K_M GGUF from spiritfather's MiMo V2.6 Flash MOPD Yamz Uncensored release, pinned to the exact package used in Mango.

Benchmark results · Files · Download · Runtime · Provenance · Full collection

**Join Infinity on Discord**

Get model support, request models and quants, discuss benchmarks, and follow new releases and model improvements.


**Qwen effort study — separate full-404 cohort:** medium 82.87/100 versus xhigh 84.41/100; paired difference +1.54 points (95% interval [-1.71, +4.82]). 404 collected questions, 403 valid questions graded per effort; one invalid reference annotation excluded symmetrically. The historical 202-question cross-model comparison below is unchanged. Study, timing and blind subjective results.

Preserved and documented by **isilicon / Mango**. The original model, modification and quantization remain the upstream creators' work. This repository adds attributed preservation and exact-configuration evaluation, with the selected source bytes unchanged.

| Property | Recorded value | 
|---|---|
| Artifact | GGUF | 
| Selected source payload | 151.61 GB / 141.20 GiB | 
| Primary selected files | 1 | 
| Mango weighted score | 79.39 / 100 | 
| Matched workload | 220.28 minutes · 202 questions + 12 agent cases | 
| Mechanical agent passes | 12/12 | 
| Infrastructure errors / length finishes | 0 / 2 | 
| Evaluation status | Measured configuration | 
| Source publisher | spiritfather | 
| Source revision | `f1adc2e15d07b84bc35b7e0ba6e2b7df5094ff3f` | 
| Declared license | mit | 

GB is decimal and GiB is binary. Payload size is disk/storage size, not a claim that the model fits in the same amount of VRAM. Runtime, KV cache, working memory and companions add their own requirements.

It passed 18/20 local coding tests and all 12 mechanical agent cases. Its 85.00 cyber domain and 90.00 instruction-following score are stronger than its 65.47 combined social/EQ score.

The workload took 220.28 minutes, about 9.19 times Qwen27's. Two genuine native length finishes remain part of the results. Time sums successful saved requests across the host wipe and excludes downtime.

| Domain | Score / 100 | Overall weight | 
|---|---|---|
| Social / emotional understanding | 65.47 | 30% | 
| Cybersecurity | 85.00 | 20% | 
| Coding | 90.00 | 10% | 
| Agentic checks | 82.50 | 30% | 
| Instruction following | 90.00 | 5% | 
| General knowledge / math | 90.00 | 5% | 

Domain scores equally average their component task means. The overall score applies the weights above. EQ is continuous; the 12-case mechanical agent pass rate is only one component of the agentic domain.

| Task / local grader | n | Mean / pass % | 95% interval | Method | 
|---|---|---|---|---|
| EQ-Bench v2 | 30 | 80.95 | 73.52–87.44 | Bootstrap mean | 
| OpenToM labels | 18 | 50.00 | 29.03–70.97 | Wilson | 
| CTI multiple choice | 20 | 75.00 | 53.13–88.81 | Wilson | 
| CTI root-cause / CWE | 15 | 80.00 | 54.81–92.95 | Wilson | 
| CyberMetric | 20 | 100.00 | 83.89–100.00 | Wilson | 
| Local BFCL-style calls | 40 | 65.00 | 49.51–77.87 | Wilson | 
| Local HumanEval+ tests | 20 | 90.00 | 69.90–97.21 | Wilson | 
| IFEval strict prompt pass | 20 | 90.00 | 69.90–97.21 | Wilson | 
| MMLU-Pro subset | 14 | 100.00 | 78.47–100.00 | Wilson | 
| MATH-500 levels 4–5 | 5 | 80.00 | 37.55–96.38 | Wilson | 
| Agent-loop heuristic | 12 | 100.00 | 75.75–100.00 | Wilson | 

These are small released subsets and local graders. HumanEval/BFCL scores are not the complete official benchmark pipelines. Agent checks cover completion, evidence, permissions and applicable pricing totals; other answer semantics remain unjudged.

| Measurement | Value | 
|---|---|
| Full matched workload | 220.28 min | 
| Suite-request time | 208.59 min | 
| 12 agent-case time | 11.69 min | 
| Matched tasks per minute | 0.97 | 
| Suite request latency · p50 / p95 | 13.62 / 147.27 s | 
| Suite TTFT · p50 / p95 | 3.65 / 6.41 s (202/202 recorded) | 
| Median reported output-token / request-second rate | 19.28 | 
| Reported completion tokens | 270,256 (202/202 requests) | 
| Length finishes retained | 2 | 
| Mechanical agent passes | 12/12 | 

Workload time excludes loading, restarts, wipe downtime and idle gaps. Latency/TTFT summaries cover suite requests only. The reported token rate divides engine usage counts by request time; it is not native decode throughput and token accounting differs across engines. No concurrent-serving result is implied.

| Configuration | Weighted / 100 | Workload min | Agent checks | Length finishes | 
|---|---|---|---|---|
| Flash Next · mradermacher | 84.19 | 35.33 | 12/12 | 0 | 
| Flash Next · direct Orca | 83.42 | 37.36 | 12/12 | 0 | 
| DeepSeek GSQ IQ2_XS | 83.19 | 71.43 | 12/12 | 0 | 
| Historical GLM · capped | 82.55 | 27.49 | 12/12 | 0 | 
| Qwen122B Q4_K | 80.72 | 120.76 | 11/12 | 0 | 
| Qwen27 MLX4 + DFlash2 | 80.64 | 23.98 | 12/12 | 0 | 
| MiMo Flash Q3_K_M | 79.39 | 220.28 | 12/12 | 2 | 
| GLM · Bolding IQ2_XXS | 79.11 | 40.98 | 12/12 | 0 | 
| GLM · Orca Q2_K | 75.64 | 54.76 | 12/12 | 0 | 
| Cyber Qwen · IQ4_XS + MTP | 70.27 | 29.74 | 12/12 | 0 | 
| Nanbeige33 Q6_K | 67.46 | 175.31 | 7/12 | 3 | 
| Cyber Qwen v2 · Q6_K + Q8 MTP | 66.98 | 45.60 | 12/12 | 0 | 

The old mixed GLM row is a historical, internally capped baseline whose exact weights are unavailable. Other rows also differ in quant, engine, context and effort. Point ranks do not settle the quality ordering.

| This configuration minus comparator | Difference / 100 | Paired 95% interval | 
|---|---|---|
| Flash Next · mradermacher | -4.80 | [-10.03, +0.42] | 
| Qwen27 MLX4 + DFlash2 | -1.25 | [-5.69, +3.20] | 
| GLM · Orca Q2_K | +3.75 | [-0.85, +8.23] | 
| Flash Next · direct Orca | -4.03 | [-8.55, +0.35] | 

Intervals use 10,000 paired task-stratified bootstrap resamples, seed 41, with fixed task/domain weights. An interval containing zero does not establish a quality winner. See methodology for scope and definitions.

Download the current full numerical report and methodology. The original compact evaluation is retained as the earlier preservation snapshot; its model score is unchanged, while the current report includes the expanded cohort. The numerical report contains aggregate statistics and hashes; no prompts or generated answers are published.

Mango used llama.cpp with automatic GPU fitting/offload, host RAM and Q8 KV caches. The 151.61GB selected package exceeds the test GPU's VRAM capacity; its disk size is not a VRAM requirement estimate.

32,768-token configured benchmark context; saved responses report native engine-default effort.

The local evaluation covers text/tool tasks. It does not establish the full advertised context ceiling, image/audio quality or every optional speculative-decoding path. Publisher capabilities and Mango-verified behavior are separate scopes.

No separate companion mirror is declared in this selected manifest. This does not imply that every optional upstream projector, draft head or multimodal path is included; consult the exact file list.

| Selected file · original path | GB | Identity prefix | 
|---|---|---|
| MiMo-V2.6-Flash-MOPD-Yamz-Uncensored.Q3_K_M.gguf | 151.60844 | `c342b29566a9755c…` (SHA-256) | 

Full paths, byte sizes and complete SHA-256/Git blob identities are in SOURCE_MANIFEST.json. Available original README/license/notice files are also preserved. MIRROR_STATUS.json records the earlier source-file verification; MODEL_CARD_STATUS.json records this documentation refresh separately.

The commands pin the already-verified mirror weight revision. Documentation can advance on main without changing that preserved weight snapshot. Keep original subfolders and all shards.

```
python -m pip install -U huggingface_hub
hf download isilicon/mango-mimo26-flash-mopd-uncensored-q3km \
  MiMo-V2.6-Flash-MOPD-Yamz-Uncensored.Q3_K_M.gguf \
  --revision e842719b9d6ad87fdda51699c920095e9771ef0c \
  --local-dir ./weights/mimo26-flash-mopd-uncensored-q3km
```
```
llama-server \
  --model ./weights/mimo26-flash-mopd-uncensored-q3km/MiMo-V2.6-Flash-MOPD-Yamz-Uncensored.Q3_K_M.gguf \
  --host 127.0.0.1 --port 8080 --ctx-size 32768 \
  --jinja --flash-attn on --cache-type-k q8_0 --cache-type-v q8_0
```
This is a minimal text-serving example for a compatible build, not a replay of every benchmark fitting/offload flag. The recorded configuration and engine caveats define the tested scope.

The source credits the XiaomiMiMo base and the Yamz modification lineage. The publisher's own benchmarks, context claims and quantization descriptions are retained in the upstream reference; Mango's measurements below cover only this Q3_K_M setup.

| Responsibility / identity | Attribution | 
|---|---|
| Mirror / local evaluation | isilicon · Mango | 
| Immediate source | spiritfather/MiMo-V2.6-Flash-MOPD-Yamz-Uncensored-GGUF | 
| Pinned source commit | `f1adc2e15d07b84bc35b7e0ba6e2b7df5094ff3f` | 
| Declared upstream base(s) | XiaomiMiMo/MiMo-V2.6-Flash-MOPD | 
| Source-declared license | mit | 

Original notices, base-model conditions and the publisher's stated limitations still apply. Downloadable preservation does not add new license rights. Source-described refusal modification and publisher-side benchmark claims have not been independently re-measured by this Mango task cohort.

The unchanged upstream README and pinned source card retain the original usage guidance, technical discussion, attribution, caveats and citations.

## Read the complete original publisher card

**Source reference.** The following is the original publisher's documentation. Its other quants, full-model files, context claims and benchmark tables can exceed the selection in this mirror. Relative asset/file URLs are qualified to the pinned original source; publisher measurements remain separate from Mango measurements.

    🧬 Ablation: yamz-labs/MiMo-V2.6-Flash-MOPD-EXL3-Yamz-Uncensored · base: XiaomiMiMo/MiMo-V2.6-Flash-MOPD
    

    📦 Source GGUF: QuantaPlanta/MiMo-V2.6-Flash-MOPD-MXFP4-GGUF
    

    🛠️ Runs on llama.cpp (master)
    

    📊 Benchmarked on CaliperBench
    

    💬 Discord

All credit for the uncensoring goes to **yamz-labs**, who fitted the refusal direction and per-layer strengths for MiMo-V2.6-Flash-MOPD and published them as `uncensor_direction.st` + `uncensor_spec.json`. Credit for the base model goes to **XiaomiMiMo** (309B total / ~15B active MoE, 256 experts, 8 active), and for the lossless GGUF conversion these quants start from to **QuantaPlanta**. These are quantizations only.


**Why this repo exists.** yamz-labs' uncensor is applied at *runtime* by their Kyojin engine on an EXL3 pack (tested on AMD Strix Halo); stock ExLlamaV3, TabbyAPI, llama.cpp, LM Studio and Ollama ignore it and serve the base model with its refusals. This repo bakes the same edit into the weights so it works in **any llama.cpp-based runtime** — Apple Silicon, CUDA, CPU — with no special engine or flag.


Benchmarked on **CaliperBench** — a creative-writing benchmark scoring prose craft, roleplay and willingness rather than general intelligence.


100 held-out harmful prompts (`harmful_behaviors` eval split), thinking on, greedy, 1024-token cap, classified by CaliperBench's refusal detector. Same prompts and settings for every row.

| Model | Refused | Notes | 
|---|---|---|
| Stock MiMo-V2.6-Flash-MOPD (QuantaPlanta MXFP4) | 97 / 100 |  | 
| Attention-only bake (experts left stock) | 74 / 100 | what you get if the expert half of the edit is lost | 
| **This repo, Q4_K_M** | **3 / 100** | 0 empty or truncated answers | 
| yamz-labs EXL3 + Kyojin runtime edit (their card) | 4 / 100 | their prompt set and regex detector, not directly comparable | 

The edit is one global direction: it lowers refusals and over-refusals together. It is not a safety evaluation and says nothing about what the model will or will not produce.

KLD, PPL and same-top-token are measured against the **stock** model (QuantaPlanta's MXFP4 GGUF, which is lossless against the release) on `wiki.test.raw`, 100 chunks at ctx 512, `llama-perplexity --kl-divergence`. So each number is the *combined* cost of the uncensor edit and the quantization, which is what you actually pay versus running stock. The table fills in as each size lands.

| Quant | Size | Mixture (default / gate exps / up exps / down exps) | PPL | vs stock | KLD vs stock | Same top token | 
|---|---|---|---|---|---|---|
| Q3_K_M | 141.2 GiB | Q8_0 / Q3_K / Q3_K / Q4_K+Q5_K | 5.3146 ± 0.0745 | +3.65% | 0.0635 ± 0.0009 | 89.57% | 
| IQ4_XS | 158.4 GiB | Q8_0 / IQ4_XS / IQ4_XS / IQ4_XS+Q5_K | 5.2126 ± 0.0726 | +1.66% | 0.0333 ± 0.0004 | 91.81% | 
| Q4_K_M | 176.5 GiB | Q8_0 / Q4_K / Q4_K / Q4_K+Q6_K | 5.1724 ± 0.0724 | +0.88% | 0.0243 ± 0.0003 | 93.26% | 
| Q5_K_M | 206.5 GiB | Q8_0 / Q5_K / Q5_K / Q5_K+Q6_K | 5.1632 ± 0.0722 | +0.70% | 0.0173 ± 0.0002 | 94.22% | 
| Q6_K | 238.4 GiB | Q8_0 / Q6_K / Q6_K / Q6_K | 5.1515 ± 0.0720 | +0.47% | 0.0146 ± 0.0002 | 94.67% | 
| stock (reference) | 162.9 GiB | BF16 / MXFP4 / MXFP4 / MXFP4 | 5.1275 ± 0.0714 | 0 | 0 | 100% | 

```
llama-server -m MiMo-V2.6-Flash-MOPD-Yamz-Uncensored.Q4_K_M.gguf -ngl 99 -fa on -c 32768 -ctk q8_0 -ctv q8_0 --jinja
```
**Thinking:** on by default in MiMo's template. For non-thinking replies pass `chat_template_kwargs: {"enable_thinking": false}` per request.

**Sampling:** yamz-labs and Xiaomi recommend temperature 1.0, top-p 0.95; greedy decoding is not recommended for generation.

1. **Source.** QuantaPlanta/MiMo-V2.6-Flash-MOPD-MXFP4-GGUF: a stock`convert_hf_to_gguf.py` of the release — bf16 attention/dense, routed experts in MXFP4. MiMo ships its experts natively in MXFP4, so this file is lossless against the original weights.
2. **Bake the edit.** yamz-labs' spec states the equivalent weight form of their runtime edit,`W ← W − w(L)·r̂ r̂ᵀ W` , applied to the writers into the residual stream:`attn_output` (layers 11–47) and the MLP/MoE output (`ffn_down` in dense layer 0, routed`ffn_down_exps` in layers 1–46), with their per-layer strengths. MTP layers, router, gate/up experts, norms and embeddings are untouched. Math is float32; the edited tensors are written as BF16 into a master GGUF (~320 GB), and every edited tensor was checked against the expected`|1 − w|` shrink of the direction component (e.g. layer 20 experts 0.222× vs 0.221× expected).
  - The edited experts are deliberately **not** re-encoded to MXFP4: that erased ~80% of the expert edit, and that build refused 74/100.
3. The edited experts are deliberately 
4. **Quantize** each size from that master with`llama-quantize` : routed experts take the nominal type; every non-expert tensor (attention, dense and MTP FFN,`nextn.eh_proj` , token embeddings, output head) is pinned to Q8_0; router, norms and attention sinks stay F32. Static quants, no imatrix.`--allow-requantize` is needed only because the unedited gate/up experts are MXFP4 in the master — the model's native precision, so this is still a first quantization.```
llama-quantize --allow-requantize \
  --tensor-type attn_qkv=q8_0 --tensor-type attn_output=q8_0 \
  --tensor-type 'ffn_(gate|up|down)\.weight=q8_0' --tensor-type nextn.eh_proj=q8_0 \
  --token-embedding-type q8_0 --output-tensor-type q8_0 \
  MASTER.gguf MiMo-V2.6-Flash-MOPD-Yamz-Uncensored.Q4_K_M.gguf Q4_K_M
```

Built with llama.cpp master `4e416ee` (stock `mimo2` support, no patches).

The edit lowers refusals. You decide how to use the model and you answer for it under the laws that apply to you. Intended uses: research, evaluation, red-teaming, creative writing and private deployment. Do not use it to produce illegal content or to target people, or in a public service without your own filtering and moderation.

MIT, inherited from XiaomiMiMo/MiMo-V2.6-Flash-MOPD.

Selected source bytes are verified against pinned metadata; the original eight graded mirrors also passed the previously recorded anonymous 1 MiB weight-prefix checks. This is not a full independent-device restore/load test. Hugging Face repository copies share a provider and do not guarantee permanent storage.

- Downloads last month
- 174
