---
id: collect-261121-veille-ia/xiaomi-mimo/mimo-v2-6-pro-official-technical-report-architecture-scaled-rl-and-evaluation-conditions
title: "MiMo-V2.6-Pro Official Technical Report: Architecture, Scaled RL, and Evaluation Conditions"
domain: xiaomi-mimo
role: reference
task: article
actors: ["Xiaomi"]
dates: ["2026-09-22"]
keywords: ["report", "mimo-v2", "official", "parameters", "training", "audio", "agent", "model"]
source: https://go.tabbit.ai/model/mimo-v2-6-pro/reviews/xiaomi-mimo-technical-report-architecture-training-and-evaluation-conditions
source_anchor: ""
source_lines: [1, 149]
section: "Xiaomi_MiMo"
---
The official report defines MiMo-V2.6-Pro as a native multimodal sparse MoE with 1.02T total parameters and approximately 42B active parameters, and reports strong agent benchmark results from large-batch, multi-environment RL training with multiple harnesses and groupwise graders; however, most figures in the tables are vendor-reported, and the public information remains insufficient to independently reproduce every benchmark.

**Suitable tasks**: Long-context coding agents, general tool workflows, visual web/design tasks, vulnerability reproduction, and agent scenarios that require joint understanding of text, images, video, and audio.

**Unsuitable tasks**: Judging production success rate, cost efficiency, or statistical significance from the official tables alone; treating internal benchmarks, training curves, or demo cases as independent blind tests; or equating Pro-RL weight results directly with the API's `mimo-v2.6-pro-ultraspeed`.

**Applicable model version**: Primarily the open checkpoint `XiaomiMiMo/MiMo-V2.6-Pro-RL`; the official API uses the lowercase alias `mimo-v2.6-pro`, but the report does not prove that the API route and public weights are identical in provider, sampling, or post-processing. `MiMo-V2.6-Flash-RL`, `MiMo-V2.6-Distill-Qwen-9B`, and UltraSpeed must be recorded separately.

**Applicable client, agent, or API**: The report's experiments use a proprietary agent harness; the model card provides deployment entry points for SGLang, vLLM, AI Studio, MiMo Code, MiMo Desktop, Open Platform API, and OpenRouter. The official materials do not disclose whether the evaluation harnesses of each external client are equivalent.

**Recommended reasoning level and parameters**: The report says that baselines were compared using each model's highest available reasoning setting (`max`); the model card recommends `temperature=1.0` and `top_p=0.95` for sampling. Other parameters, tool timeouts, random seeds, and API endpoints should be recorded explicitly during retesting.

The report covers two series members, MiMo-V2.6-Pro and Flash; this document uses the Pro data as the primary result.

| Item | Official report value for MiMo-V2.6-Pro-RL | 
|---|---|
| Total parameters / active parameters | 1.02T / approximately 42B | 
| Backbone | Sparse MoE Transformer; the first layer uses global attention + dense FFN, followed by interleaved SWA and GA | 
| Backbone layers | 70 layers, including 60 SWA and 10 GA | 
| Hidden dimension | 6144 | 
| Experts | 384 routed experts, with 8 activated per token | 
| Sliding window | 128 | 
| Context | Training pipeline extended to 1M tokens | 
| Modalities | text, image, video, audio | 
| Vision encoder | MiMo-ViT, 681M parameters; 28 layers (24 SWA / 4 GA); patches of 2 × 16 × 16; spatial merge of 2 × 2 | 
| Audio encoder | AudioTokenizer, 308M parameters; audio patch encoder, 127M parameters | 
| MTP / speculative decoder | 5 SWA layers; conditioned on at most 1,024 backbone context positions and predicts the next 7 tokens at a time | 

The report describes a two-stage pretraining setup: first, the language backbone is trained on text-only data, then it is trained jointly with the vision and audio encoders for full multimodal training. Pro's total pretraining volume is 30T tokens, consisting of 27T in the text stage and 3T in the omni stage; context is expanded from 32K to 256K. Mid-training then takes place at 256K, followed by a final expansion to 1M and a switch to the large-batch-oriented Muown variant; embedding, LM head, and MoE router continue to use AdamW, alongside MXFP4 quantization-aware training.

The report says that a short SFT stage is followed by one mixed-task RL stage. The core settings are:

| Item | Official report value | 
|---|---|
| Algorithm | GRPO, asynchronous partial rollouts, with staleness of 4 | 
| Prompts per step | 1,568 | 
| Rollouts per prompt | 16 | 
| Global training batch | Approximately 25K trajectories | 
| Training tokens per step | Approximately 2.7B–3.7B; approximately 110K–150K tokens / sequence | 
| Pro RL cost | Approximately $2.6 million (260 × $10,000; the report's page 8 says approximately $2.6M; the official release page says approximately $2.62M) | 
| RL task mix | coding 68%, general tool use 12%, aesthetic design 13%, context following 3%, cybersecurity 4% | 
| Optimizer | Muown, learning rate `3e-6` , no weight decay / warmup, gradient clipping 1.0; router frozen during the RL stage | 
| Reasoning level for evaluation baselines | The highest supported setting, `max` , for baselines with configurable reasoning effort | 

The training signal does not rely only on binary test rewards. For high-pass-rate code tasks, Groupwise Reward Synthesis (GRS) generates task-specific solution/behavior rubrics offline, then combines the reward as `R_i = R_test × S_sol × S_beh`; the remaining code tasks mainly use Groupwise Advantage Redistribution (GAR), which compares successful and failed trajectories within the same rollout group and assigns more positive advantage to higher-quality successful solutions. The report also uses a group-relative length penalty, tool/format behavior penalties, and batch-level advantage rebalancing to encourage shorter, more stable solutions.

The report treats the harness as a training variable rather than a fixed product shell. Training uses four minimal, composable mini-harnesses, each consisting of a system prompt, tools, context management, and a minimal agent loop; after training, transfer is checked on three unseen harnesses (Codex, Claude Code, and mini-swe-agent). Figure 10 in the report shows that DeepSWE v1.1's average held-out-harness Pass@1 rises from approximately 50% to 66%, but the figure does not provide per-question inputs, sample counts, or confidence intervals.

The environments fall into four categories:

**Code**: GitHub issues/PRs, day-to-day development, specification-driven, source-driven, and long-horizon engineering tasks; tasks run executable tests, with specification–test alignment audited through four rollouts.

**General agent**: Locally resettable workspaces, software mocks, files, and databases; code checks and LLM rubric checks are used, with cross-model rollout reviews of rubrics that are too strict or too lenient.

**Visual agent**: Web pages, interactive applications, games, 3D, slides, SVG, video, and Figma; open-ended design combines pointwise and groupwise grading, while high-fidelity replication combines pixel similarity with LLM judging.

**Cybersecurity**: Vulnerability types in ASan/MSan/UBSan reports and project-level top-of-stack locations serve as rule-based oracles; a PoC passes only when it matches both.

The report also describes defenses against reward hacking: caches, build artifacts, subsequent Git commits, and network access are cleaned or restricted; a hack agent repeatedly probes for leaks, followed by offline trajectory audits during training. The authors state that the share of confirmed reward-hacking trajectories during final training was below 2% for both Pro and Flash. This is a vendor-reported monitoring metric, not an external audit result.

The following values come from Table 3 of the technical report (Pro, Flash, the previous-generation Pro, and several closed-source models for comparison). `-` means that the report did not provide a value and should not be filled with 0. Except for the GDPval-AA score scale, the table values are recorded exactly as reported; the official materials do not provide the sample count, random seed, or complete harness configuration for each item.

| Category | Benchmark | MiMo-V2.6-Pro | Flash | MiMo-V2.5-Pro | Claude Opus 5 | GPT-5.6 Sol | Claude Fable 5 | 
|---|---|---|---|---|---|---|---|
| Code | DeepSWE v1.1 | 71.9 | 67.9 | 19.0 | 74.0 | 73.0 | 70.0 | 
| Code | ProgramBench | 26.5 | 26.0 | 12.5 | 37.0 | 25.0 | 33.0 | 
| Code | MiMo Code Bench | 63.2 | 61.2 | 40.4 | 68.6 | 59.3 | - | 
| General | AutomationBench v1.0.6 | 53.1 | 52.3 | 16.0 | 50.3 | 45.8 | 46.2 | 
| General | Toolathlon-Verified | 76.9 | 73.6 | 49.1 | 80.6 | 74.9 | 77.9 | 
| General | GDPval-AA 2.1 | 1673 | - | 1107 | 1708 | 1588 | 1595 | 
| General | Agents’ Last Exam | 31.6 | 27.6 | 13.2 | 31.6 | 30.8 | 25.7 | 
| General | Terminal Bench 4.0 | 34.9 | 28.8 | 1.5 | 49.0 | 39.9 | 42.4 | 
| General | Terminal Bench 2.1 | 89.9 | 87.6 | 65.2 | 89.1 | 88.8 | 84.3 | 
| General | OSWorld-Verified | 82.0 | 80.8 | - | 83.4 | 83.0 | 86.0 | 
| General | JobBench | 62.0 | 61.2 | 25.0 | 65.7 | 45.4 | 57.4 | 
| Cyber | CyberGym | 94.0 | 95.1 | 40.0 | - | - | - | 
| Cyber | MiMo Cyber Bench | 80.2 | 77.2 | 0.0 | - | - | - | 
| Cyber | ExploitGym | 17.8 | 6.0 | 0.2 | 22.1 | 30.3 | 28.4 | 
| Cyber | ExploitBench | 47.9 | 25.3 | 16.6 | 70.0 | 78.5 | 78.0 | 
| Cyber | SEC Bench Pro | 66.3 | 47.5 | 17.7 | - | 79.1 | - | 
| Visual | MiMo Visual Coding | 72.3 | 71.5 | - | 70.0 | 73.4 | 69.1 | 

The report provides two additional pieces of process evidence:

DeepSWE v1.1's average@3 rises from 58.4 to 72.6 during Pro RL, while Flash rises from 48.7 to 65.7; this is a before-and-after change within the same training pipeline, not an independent model comparison.

When Pro's router is not frozen, the report observes in decoder layer 9 that CV rises from 0.78 to 2.0, peak load from approximately 6× to 16×, and cold experts from 0.5% to 22%; the run with a frozen router keeps CV at approximately 0.7, peak load at approximately 5.5×, and cold experts at approximately 1%, while benchmark scores continue to grow. This ablation supports the engineering choice to freeze the MoE router during the RL stage.

To review the report's results, at least the following conditions should be fixed, and public-weight results should be kept separate from API results:

Download `XiaomiMiMo/MiMo-V2.6-Pro-RL` and record the commit SHA, weight precision, inference engine, and GPU topology; do not mix Flash, Distill-Qwen-9B, or UltraSpeed into the Pro results.

Start inference according to the model card's official configuration. Key vLLM parameters are `--tensor-parallel-size 8`, `--trust-remote-code`, `--gpu-memory-utilization 0.95`, `--max-model-len auto`, `--reasoning-parser mimo`, `--tool-call-parser mimo`, `--enable-auto-tool-choice`, and `--generation-config vllm`; first record sampling settings of `temperature=1.0` and `top_p=0.95`.

For each benchmark, save the version, task set, sample count, system prompt, tool schema, maximum turns/tokens, timeout, random seed, per-question outputs, and scoring logs. The report does not disclose all of these fields; missing items must be marked “not disclosed.”

First rerun public DeepSWE v1.1, ProgramBench, Terminal Bench 2.1, OSWorld-Verified, Toolathlon-Verified, CyberGym, ExploitBench, and other benchmarks, then implement or request access to MiMo's internal benchmarks separately; internal tasks must not be presented as reproduced from public figures.

To reproduce harness transfer, run the same coding tasks on the four training mini-harnesses and the three held-out harnesses, Codex, Claude Code, and mini-swe-agent, and report each harness's Pass@1, sample count, and mean-calculation method.

To review reward hacking, record environment cleanup, hack-agent probes, trajectory audits, and verifier outputs in an isolated, network-free environment; do not judge reward reliability from the final score alone.

Page 1 of the technical report: overview of the abstract, 1,568 samples, 2.7–3.7B tokens per step, four environment types, multiple harnesses, groupwise grading, and the frozen router.

Pages 4–6: the Hybrid-SWA backbone, MiMo-ViT, two-stage audio encoder, MTP decoder, and Pro/Flash configuration table.

Pages 7–9: Pro pretraining with 30T tokens (27T text + 3T omni), mid-training context expansion, RL cost, and batch/rollout settings.

Pages 10–16: code/general/visual/cyber environment construction, test stability, multi-harness design, and reward-hacking defenses.

Pages 17–20: GRS, GAR, length-penalty, and behavior-penalty formulas; the public definition of GRS is `R_i = R_test_i × S_sol_i × S_beh_i`.

Pages 20–24: RL task mix, Muown parameters, evaluation categories, held-out harnesses, router-freeze ablation, and failure analysis.

Page 26: 17 Pro benchmark results in Table 3; pages 34–36: open-environment and multi-harness auxiliary experiments for Distill-Qwen-9B.

Official model-card deployment configuration and model metadata: https://huggingface.co/XiaomiMiMo/MiMo-V2.6-Pro-RL/blob/main/README.md.

**Vendor-reported**: Table 3, training cost, pre- and post-training scores, reward-hacking share, and harness-transfer curves all come from Xiaomi MiMo; this document does not present them as independent retests.

**Missing reproduction requirements**: The report publishes benchmark names and aggregate scores, but does not provide a complete prompt, system prompt, tool schema, sample count, random seed, per-question logs, confidence interval, parallelism, or complete scoring script for each benchmark.

**Internal tasks cannot be directly compared**: MiMo Code Bench, MiMo Cyber Bench, and MiMo Visual Coding are internal benchmarks; without the complete task sets, they can only be treated as vendor-reported metrics.

**Different models/endpoints cannot be combined**: The Pro-RL weights, API `mimo-v2.6-pro`, `mimo-v2.6-pro-ultraspeed`, Flash, and Distill-Qwen-9B may use different computation graphs, service layers, and inference configurations.

**Training-process scores are not final comparisons**: DeepSWE's 58.4→72.6 is a checkpoint change during RL, jointly affected by data, environments, graders, harnesses, and the optimizer; it cannot be attributed to a single architectural factor.

**Approximate chart values retain uncertainty**: The “approximately 50%→66%” held-out-harness result comes from the report's curve, which does not provide exact step-by-step data in the body; it must not be presented as a precise statistic.

**No unified failure rate is provided**: Although the report analyzes OOM, DBE, unreachable grader networks, expert imbalance, and partial-rollout failures, it does not provide the failure-sample distribution for each benchmark, so scores do not represent end-to-end stability.

**High deployment requirements**: Pro is a 1.02T sparse MoE, and the model card's official vLLM/SGLang examples require multi-GPU/multi-node configurations; results from a single GPU or from different quantization or parallelism strategies cannot be directly aligned with Table 3.

The core wording in the report title is **“Scaling Reinforcement Learning Towards Self-Improvement”**, indicating that its main contribution is an RL training system and environments rather than merely a static model release.

The report's training setup explicitly states **“1,568 prompts with 16 rollouts per prompt”**; this explains why the official results cannot be reproduced directly through an ordinary single-turn chat call.

The model card labels the Pro technical-report entry **“Technical Report”** and publishes `MiMo_V2_6_technical_report.pdf`, architecture diagrams, SGLang/vLLM configurations, and the complete benchmark table; all figures in this document were primarily checked against the PDF rather than search snippets or reposts.

The figures, task set, reasoning tier, and client conditions apply only to the listed source and collection snapshot. Different versions, harnesses, or providers must not be compared directly; undisclosed parameters remain unknown.

For a reproduction, fix the model version, provider or client, reasoning tier, tools, task-set version, sample count, and collection date, and record failures, retries, and human corrections. Full steps are in the source notes below.

**Hugging Face (XiaomiMiMo official model card and technical report)** · LLM-Core Xiaomi / Xiaomi MiMo Team · Original publication date 2026-09-22 · Site edit date 2026-09-22

MiMo-V2.6-Pro

Download the Tabbit client to check model access
