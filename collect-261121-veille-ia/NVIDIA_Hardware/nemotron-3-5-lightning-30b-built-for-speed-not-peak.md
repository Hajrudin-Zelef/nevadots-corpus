---
id: collect-261121-veille-ia/nvidia-hardware/nemotron-3-5-lightning-30b-built-for-speed-not-peak
title: "Nemotron 3.5 Lightning: 30B Built for Speed, Not Peak"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: ["2026-08-11"]
keywords: ["model", "nvidia", "lightning", "nemotron", "speed", "card", "openrouter", "nvfp4"]
source: https://www.digitalapplied.com/blog/nvidia-nemotron-3-5-lightning-30b-a3b-efficiency-tier-2026
source_anchor: ""
source_lines: [1, 173]
section: "NVIDIA_Hardware"
---
NVIDIA Nemotron 3.5 Lightning launched on August 11, 2026 — a 30-billion-parameter open-weight model that activates roughly 3B parameters per token, built as the fast “execution layer” for long-running agents rather than as an accuracy leader. NVIDIA’s own model card makes the trade explicit: across the 14 benchmark rows it publishes, Qwen 3.6 35B A3B posts the higher number on 13.

That is not a gotcha — it is the product thesis. The launch post leads with output speed and cost per task; the model card publishes the accuracy table that concedes almost every row to a similarly sized rival. Reading both documents together is the only honest way to evaluate this release.

This guide covers what shipped and where, the full 14-row scoreboard with every number attributed to NVIDIA’s own evaluation harness, the third-party speed data from Artificial Analysis, the architecture and the two context-window figures that are both vendor-true, the OpenMDW-1.1 license, per-route pricing on OpenRouter, and where an execution-tier model actually fits in an agent stack.

1. 01NVIDIA shipped an efficiency tier, not an accuracy leader.Nemotron 3.5 Lightning is a 30B hybrid Mamba-2 plus MoE model with roughly 3B active parameters per token, released August 11, 2026 as BF16 and NVFP4 checkpoints plus draft-model variants for speculative decoding.
2. 02Qwen 3.6 35B A3B scores higher on 13 of 14 rows — in NVIDIA's own table.Every number comes from NVIDIA's NeMo Gym / NeMo Evaluator harness, and the card itself warns results may differ from vendors' self-reported figures. Lightning's only head-to-head win is IFBench (loose) — where Gemma 4 26B tops both.
3. 03The speed story is real, but the score is third-party.Artificial Analysis measures 293.2 output tokens per second (#3 of 134 models on its tracker) and an Intelligence Index of 24 (#14 of 134). Those are AA's numbers, not NVIDIA's — the launch post cites the chart, the card cites the harness.
4. 04Context is two true numbers, not one.The model card says up to 1M tokens, with 256K used for single-H100 deployment. The BF16 config sets 262,144 max positions; the NVFP4 config sets 1,048,576. OpenRouter displays 262K while Artificial Analysis displays 1M — both trace to real configs.
5. 05Pricing is per-route, and the license is OpenMDW-1.1.On OpenRouter, DeepInfra serves it at $0.05 / $0.20 per 1M tokens and CoreWeave at $0.10 / $0.25 — attribute the price to the route. The license is OpenMDW-1.1, not Apache 2.0 and not the NVIDIA Open Model License.

## 01 — What LaunchedAn *execution* tier, shipped on five checkpoints.

NVIDIA published the launch post on its developer blog on August 11, 2026, and the Hugging Face model repos carry the same release date. So does the OpenRouter listing — unlike several recent open-weight releases, there was no marketplace listing lag on this one.

Five checkpoints shipped under the `nvidia/NVIDIA-Nemotron-3.5-Lightning-30B-A3B` family: a full-precision BF16 reference, a deployment-optimized NVFP4 quantization, a Base-BF16 pre-trained checkpoint, and two draft-model variants (NVFP4-DFlash and NVFP4-DSpark) for speculative decoding. The positioning is unambiguous: this is the small, fast “execution layer” in NVIDIA’s system-of-models framing — a frontier reasoning model plans and orchestrates, and a model like Lightning executes the high-volume steps: tool calls, result validation, subagent delegation, routine command runs.

##### Lightning 30B-A3B *BF16*

The customization and post-training reference. Its config sets max_position_embeddings to 262,144 — the figure OpenRouter's 262K context display traces back to.

##### Lightning 30B-A3B *NVFP4*

NVIDIA's recommended path for direct inference, using the same NVFP4 kernels that power Nemotron 3 Ultra across Blackwell, Hopper and Ampere (vendor-stated). Its config is where the 1M-token figure lives.

*OpenClaw*and Hermes Agent as example harnesses running under its NemoClaw security and management stack, and is shipping a companion routing library, NeMo Switchyard, to move tasks between frontier and execution-tier models.

Lightning is the smallest tier of a family we have tracked since launch season began: Nemotron 3 Ultra at 550B is the reasoning and orchestration flagship, and Nemotron 3 Super at 120B sits in the middle — both appear as comparison columns in the benchmark table below. Lightning completes the ladder at 30B total, 3B active, and its job description is different from theirs: be cheap and fast enough to run constantly.

## 02 — The ScoreboardNVIDIA’s own table gives Qwen *13 of 14* rows.

The full accuracy table below is reproduced from the BF16 model card. One attribution rule governs every cell: these are **NVIDIA’s own measurements** of all six models, run under a single harness — not Alibaba’s numbers for Qwen, not Google’s for Gemma, not OpenAI’s for GPT-OSS. The card itself says so.

*every*model in the table, carries that caveat.

| Fourteen benchmark rows from the Nemotron 3.5 Lightning BF16 model card comparing Nemotron 3.5 Lightning, Qwen 3.6 35B A3B, Gemma 4 26B A4B, Nemotron 3 Nano, Nemotron 3 Super, and GPT-OSS 20B, all measured by NVIDIA under its NeMo Gym and NeMo Evaluator harness, with a derived row-leader column naming the highest score among the models shown. The four category headings that group the rows are ours, not the card’s. |  |  |  |  |  |  |  | 
|---|---|---|---|---|---|---|---|
| Benchmark | Lightning 3.5 | Qwen 3.6 35B A3B | Gemma 4 26B A4B | Nemotron 3 Nano | Nemotron 3 Super | GPT-OSS 20B | Row leader | 
|---|---|---|---|---|---|---|---|
| Knowledge and science |  |  |  |  |  |  |  | 
| MMLU Pro | 81.94 | 85.63 | 85.20 | 78.46 | 83.89 | 76.40 | Qwen 3.6 35B | 
| AA-Omniscience | 17.50 | 19.47 | 22.17 | 20.15 | 26.68 | 16.62 | Nemotron 3 Super | 
| GPQA Diamond (no tools) | 75.44 | 83.40 | 79.61 | 74.05 | 78.60 | 71.46 | Qwen 3.6 35B | 
| HLE (text-only, no tools) | 11.72 | 19.56 | 17.42 | 10.89 | 20.30 | 13.76 | Nemotron 3 Super | 
| SciCode | 32.60 | 35.33 | 40.28 | 30.08 | 35.11 | 38.63 | Gemma 4 26B | 
| Software engineering and terminal |  |  |  |  |  |  |  | 
| SWE-bench Verified | 51.56 | 70.12 | 57.40 | 34.08 | 63.08 | 52.44 | Qwen 3.6 35B | 
| SWE-bench Multilingual | 39.33 | 63.40 | 43.40 | 14.07 | 49.80 | 41.93 | Qwen 3.6 35B | 
| Terminal-Bench 2.1 | 24.58 | 44.38 | 37.22 | 8.29 | 39.61 | 15.17 | Qwen 3.6 35B | 
| Agentic task completion |  |  |  |  |  |  |  | 
| PinchBench | 85.37 | 88.07 | 74.70 | 66.11 | 80.36 | 57.20 | Qwen 3.6 35B | 
| BrowseComp | 36.97 | 48.74 | 26.30 | 13.74 | 22.77 | — | Qwen 3.6 35B | 
| τ³-bench (Banking) | 9.28 | 10.52 | 14.02 | 7.01 | 12.37 | — | Gemma 4 26B | 
| GDPval-AA-V2 | 832 | 1015 | 807 | 473 | 746 | — | Qwen 3.6 35B | 
| Instruction following and long context |  |  |  |  |  |  |  | 
| IFBench (loose) | 71.88 | 63.71 | 77.25 | 72.17 | 71.92 | 68.50 | Gemma 4 26B | 
| AA-LCR | 52.00 | 61.06 | 57.56 | 32.75 | 58.44 | 32.88 | Qwen 3.6 35B | 

All scores measured by NVIDIA under its NeMo Gym / NeMo Evaluator harness, as published on the Lightning BF16 model card. The “Row leader” column is our derivation: the highest value among the six models shown on that row. The four category headings that group the rows are ours too — the card publishes the fourteen rows as one flat list. The model card leaves the GPT-OSS 20B cell blank on three rows (shown as —); on those rows the leader is computed across the five reported columns.

Three readings of that table, all computed directly from NVIDIA’s published cells. First, the head-to-head: **Qwen 3.6 35B A3B posts the higher number on 13 of the 14 rows**. Lightning’s single win is IFBench (loose), at 71.88 versus Qwen’s 63.71. Second, the wider field: across all six models, Lightning is not the top score on a single row — Qwen leads 9 of the 14 rows outright, Gemma 4 26B A4B leads three, and Nemotron 3 Super leads two. Third, even Lightning’s lone head-to-head win is crowded: on IFBench, Gemma (77.25), Nemotron 3 Nano (72.17) and Nemotron 3 Super (71.92) all sit above Lightning’s 71.88.

The gaps are not uniform, though — and the shape of them is the useful part. Dividing Lightning’s score by Qwen’s on the same row (both from NVIDIA’s harness), Lightning lands within about 3 to 10 percent of Qwen on PinchBench, MMLU Pro and GPQA Diamond, but falls to roughly half to three-quarters of Qwen’s level on the agentic software rows — SWE-bench and Terminal-Bench — where sustained multi-step accuracy compounds.

#### Lightning as a share of Qwen 3.6 35B A3B, row by row · selected rows

Derived from the Lightning BF16 model card: Lightning's score divided by Qwen 3.6 35B A3B's score on the same row; both measured by NVIDIA's harness. IFBench (loose), the one row Lightning wins, is excluded — its ratio is 112.8%.
## 03 — The Speed StoryTwo documents, *two* stories — from the same vendor.

Here is the striking part: the accuracy table above does not appear in NVIDIA’s launch post at all. Of the two vendor documents we fetched, the 14-row table lives only on the Hugging Face model card. The launch post tells a different story from the same lab — it claims Lightning delivers “leading accuracy at the highest output speed in its class, winning the accuracy-versus-speed Pareto frontier on the Artificial Analysis Intelligence Index,” and cites “up to 4x output speed compared to similar-sized models” without naming a specific comparison model or benchmark for that particular figure.

The post’s sharpest benchmark claim is built on one row of the fourteen. Verbatim: “Nemotron 3.5 Lightning reaches 86% accuracy while completing 10,000 tasks 30% faster than Qwen3.6 35B at similar accuracy.” Set that against the card’s own PinchBench row — Lightning 85.37, Qwen 88.07 — and the “similar accuracy” phrasing is smoothing over a real 2.7-point gap. Both figures are NVIDIA’s own numbers; they are simply presented differently in two different vendor documents.

##### The *model card*

Concedes 13 of 14 rows to Qwen 3.6 35B A3B and warns that its harness numbers may differ from vendors' self-reported figures. The candid document.

##### The *launch post*

Leads with output speed, a third-party index chart, and a one-benchmark speed-at-similar-accuracy claim. Never reproduces the 14-row table. The marketing document.

*Intelligence Index of 24*(ranked #14 of the 134 models in its comparison set — AA calls that “well above average among other open weight models” of similar size, against a median of 9) and measures output speed at 293.2 tokens per second, ranked #3 of 134. AA also flags the model as “very verbose” — roughly 100M output tokens generated across its eval versus a 42M median for its tracked set — and “somewhat expensive” per task among open-weight peers. Even the claim that the index “combines nine evaluations” is NVIDIA’s description of AA’s methodology, not AA’s own statement on the page we fetched.

Our read: this split is not hypocrisy, it is a genre forming. The model card is written for engineers who will benchmark before deploying; the launch post is written for the routing decision — “is this fast and cheap enough to be my agents’ workhorse?” Vendors are learning that an efficiency-tier model does not need to win accuracy rows to be a rational product, so the honest table ships on the card while the speed narrative ships on the blog. Meta made a similar small-model play the same week — Muse Glimmer 30B is the companion read. The lesson for buyers is mechanical: always read both documents, and treat any “similar accuracy” phrasing as a prompt to find the table it is summarizing.

## 04 — Under the HoodA hybrid Mamba-2 + MoE stack with *3B* active.

The speed positioning rests on architecture, and the specifics are unusually well documented. The model card describes the network as interleaved Mamba-2 and mixture-of-experts layers with select attention layers — summarized on the card as a “Mixture-of-Experts Hybrid (Mamba + Transformer)” — and the `config.json` on both repos fills in the numbers: 52 hidden layers, hidden size 2,688, a 131,072-token vocabulary, and a router that picks 6 of 128 routed experts per token plus one always-on shared expert. NVIDIA states 30B total parameters with roughly 3B active per token; Hugging Face’s own file-metadata scan reports about 31.6B, consistent with 30B as a rounded figure.

Multi-Token Prediction (MTP) layers are baked in during a dedicated continued-pretraining stage — the same technique NVIDIA used on Nemotron 3 Super and Ultra — so speculative decoding is a first-class feature rather than an afterthought. Training ran from December 2025 to May 2026 (vendor-stated), with a September 2025 pre-training data cutoff and a May 2026 post-training cutoff.

##### of 30B total

Per-token compute is the product: 6 of 128 routed experts fire per token, plus 1 shared expert (config.json, both repos). Hugging Face's file scan puts the total at ~31.6B in BF16.

##### tokens (vendor-stated)

The card's corpus breakdown lists roughly 9.1T tokens of Common Crawl and 2.1T of GitHub code, plus math, science and legal data and heavy synthetic generation — with Qwen, DeepSeek and GPT-OSS named explicitly as synthetic-data teacher models.

##### ending in GRPO RL

NVFP4-recipe pre-training via Megatron-LM, continued pre-training to align the MTP heads, supervised fine-tuning on synthetic code, math, science, tool-calling and long-range-retrieval data, then reinforcement learning via GRPO across seven environment types using NeMo RL and NeMo Gym, with an async architecture that decouples training from inference.

Language coverage is English-plus-code first, with Spanish, French, German, Italian and Japanese supported and Chinese called out for multilingual reasoning and translation in post-training, per the card. One more detail worth noting for reproducibility watchers: NVIDIA published the RL recipe surface alongside the model — NeMo Gym carries a Lightning 3.5 reproducibility document, and the launch includes an open agentic RL dataset, `Nemotron-RL-Agentic-Terminal-Pivot-v1-nano35-release`, that NVIDIA says trained some of the coding-agent capabilities.

## 05 — Context Window256K and 1M are *both* true.

Ask three surfaces for Lightning’s context window and you get two answers. OpenRouter’s listing displays 262K tokens. Artificial Analysis displays 1M. Neither is wrong, because the two checkpoints genuinely differ: the BF16 reference config sets `max_position_embeddings` to 262,144, while the config in the NVFP4 deployment repo sets 1,048,576 — a clean 4× apart, and both fetched directly from the repos’ `config.json` files.

*both numbers*— up to 1M tokens, with 256K as NVIDIA’s own single-H100 deployment guidance. A flat “262K context” and a flat “1M context” are each half the story.

Practically, that means the context you get depends on the checkpoint and the hardware you deploy on: the quantized NVFP4 path is where the 1M configuration lives, while a single 80GB H100 running NVIDIA’s recommended setup operates at 256K. Marketplace listings will keep disagreeing with each other about this model precisely because both configs are real — expect the same dual-figure pattern on any model that ships reference and deployment checkpoints with different position limits.

## 06 — The LicenseOpenMDW-1.1 — *not* Apache 2.0.

Lightning ships under the OpenMDW-1.1 model license — a distinct license from Apache 2.0 (different attribution scope, and its patent language takes a different shape than Apache’s grant) and equally distinct from the NVIDIA Open Model License that some other Nemotron-family releases have used. Getting the name right matters, because the terms are unusually permissive: the license grants permission “free of charge, to deal in the Model Materials without restriction” — commercial use included, with no field-of-use restrictions.

Three terms stand out for production use. Attribution obligations apply only when you redistribute the Model Materials themselves (retain the license and copyright notices) — not to downstream use of outputs. Rights terminate for any licensee who initiates patent or copyright litigation over the Model Materials, with defensive counterclaims excepted. And everything is provided as-is, with no warranties.

OpenMDW-1.1 permits redistribution of the Model Materials outright, which is what keeps the downstream tooling path short: re-quantized and repackaged builds can be published by anyone, without a separate grant from NVIDIA.

## 07 — Pricing & AccessPriced per *route*, deployable on one H100.

NVIDIA does not sell Lightning by the token — it is an open-weight release, with a hosted preview on build.nvidia.com. Hosted pricing therefore lives with the inference providers, and on the OpenRouter listing the two paid provider routes at the time of writing quote meaningfully different rates. There is no single “OpenRouter price” for this model — attribute the rate to the route.

| Per-provider pricing for Nemotron 3.5 Lightning on OpenRouter at the time of writing, showing the two paid provider routes, DeepInfra and CoreWeave, per one million tokens, plus the separately listed free variant, with notes on how each rate relates to the others. |  |  |  |  | 
|---|---|---|---|---|
| Route | Input / 1M | Output / 1M | Cache read / 1M | Notes | 
|---|---|---|---|---|
| DeepInfra | $0.05 | $0.20 | — | The listing’s headline/default rate — also the rate Artificial Analysis shows for this model | 
| CoreWeave | $0.10 | $0.25 | $0.05 | 2× DeepInfra on input and 1.25× on output, but adds a cache read rate | 
| nemotron-3.5-lightning:free | $0 | $0 | — | A separate rate-limited OpenRouter listing rather than a provider route on the main one; we did not confirm the exact limits | 

Rates from the OpenRouter listing for nvidia/nemotron-3.5-lightning at the time of writing. Provider routes and prices change frequently — verify before committing volume.

Self-hosting is the other half of access, and the vendor-stated floor is one 80GB GPU: a single H100 or A100, with support across NVIDIA Blackwell (GB200, RTX 5090), Hopper (H100, H200) and Ampere (A100). NVIDIA also markets the model for local and on-device use on DGX Spark, Jetson and RTX 5090, with third-party local tooling support listed for LM Studio, llama.cpp, Ollama and Unsloth. If you are sizing a machine for this class of model, our local-AI hardware guide by price bracket covers the trade-offs.

The launch-day ecosystem list NVIDIA published is long — its post names inference software (Ollama, LM Studio, Unsloth, Exo, Canonical), agent harnesses (Cline, LangChain, OpenHands, OpenCode, Factory AI and others), cloud platforms (Amazon SageMaker JumpStart, Microsoft Foundry, OCI) and hosted providers (Baseten, CoreWeave, Crusoe, DeepInfra, Fireworks AI, Together AI and more). Treat that as what it is: a vendor-published day-one partner list, not independently verified per partner.

## 08 — ImplicationsWhere an *execution tier* fits in your stack.

The right way to evaluate Lightning is not “is it better than Qwen” — NVIDIA’s own table already answers that on 13 of 14 rows — but “which steps in my agent workloads are currently overpaying for reasoning they don’t need.” That framing produces four concrete calls:

##### Tool calls, validation, routine agent steps

This is the workload the model is explicitly built for: roughly 3B active parameters, MTP speculative decoding baked in, and third-party-measured 293.2 tokens/sec output (Artificial Analysis). Measure cost per completed task at your target accuracy, not benchmark rows.

##### Hard reasoning and agentic coding

Qwen 3.6 35B A3B posts the higher score on 13 of 14 rows of NVIDIA's own table, and the gaps are widest on SWE-bench and Terminal-Bench. Where a single task's accuracy decides the outcome, an efficiency tier is the wrong default.

##### DGX Spark, RTX 5090, *Jetson*

NVIDIA markets Lightning for local deployment, the day-one tooling list covers LM Studio, llama.cpp, Ollama and Unsloth, and OpenMDW-1.1 removes most licensing friction. A strong shortlist candidate for on-device agent stacks.

##### Planning, decomposition, review

Keep a frontier reasoning model on top. NVIDIA's own system-of-models framing pairs Lightning with its larger Nemotron siblings, routed by the new NeMo Switchyard library — plan with a big model, execute with a fast one.

The speculative-decoding menu deserves a note, because it is where the speed positioning becomes configurable. MTP is built into the model and, per NVIDIA, is best suited for medium-to-high concurrency, with the optimal draft length shrinking as concurrency rises. DSpark is a provided draft model recommended for DGX Spark and low-concurrency data-center workloads — NVIDIA cites an arXiv paper as its basis, which we have not evaluated. DFlash is a separate draft model released alongside, with its own earlier NVIDIA technical post; NVIDIA has published no Lightning-specific DFlash performance figure that we can cite, so treat any speedup number you see attached to this model with suspicion until it is sourced.

Looking forward, we expect the execution-tier category to formalize fast. Routing libraries like Switchyard turn “which model do we use” into a per-step decision rather than a per-project one; per-route pricing spreads like DeepInfra-versus-CoreWeave make the marketplace layer part of the cost model; and vendor cards that openly concede accuracy rows suggest benchmark tables will matter less than dollars per completed task at a target accuracy. Teams running small-model and on-device agent stacks should treat this launch as the template for how those releases will be marketed from here on. If you want help building the eval-and-routing layer that makes these decisions with data rather than launch posts, that is exactly what our AI transformation engagements do.

## 09 — ConclusionThe scoreboard *candor* is the story.

### Speed is the product. Accuracy is the tradeoff. NVIDIA said both out loud.

Nemotron 3.5 Lightning is a well-documented, permissively-licensed, genuinely fast 30B model whose own vendor table concedes 13 of 14 benchmark rows to Qwen 3.6 35B A3B. Both halves of that sentence are the release. NVIDIA is not pretending to have shipped an accuracy leader — it shipped an *execution tier*, priced from $0.05 per million input tokens on the cheapest route, deployable on a single H100, and licensed so that weights, data and recipes travel freely.

The evaluation discipline this launch demands is the same one we keep returning to: read the model card, not just the launch post; attribute every number to whoever measured it; and when a vendor says “similar accuracy,” find the table. Here the card was candid enough to make that easy — the harness caveat and the losing rows are published side by side.

The forward bet is that this genre grows. System-of-models architectures need cheap executors far more often than they need new frontier peaks, and a model that runs at hundreds of tokens per second for cents per million will win a lot of routing decisions it would lose on a leaderboard. Judge Lightning — and every efficiency release that follows it — on cost per completed task at your target accuracy, measured on your own workloads.
