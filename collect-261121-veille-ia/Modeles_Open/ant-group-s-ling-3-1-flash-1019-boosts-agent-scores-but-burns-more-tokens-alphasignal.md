---
id: collect-261121-veille-ia/modeles-open/ant-group-s-ling-3-1-flash-1019-boosts-agent-scores-but-burns-more-tokens-alphasignal
title: "Ant Group's Ling 3.1 Flash 1019 Boosts Agent Scores but Burns More Tokens | AlphaSignal"
domain: modeles-open
role: reference
task: article
actors: []
dates: []
keywords: ["ling", "flash", "reasoning", "tokens", "agent", "context", "more", "model"]
source: https://alphasignal.ai/news/ant-group-s-ling-3-1-flash-1019-boosts-agent-scores-but-burns-more-tokens
source_anchor: ""
source_lines: [1, 98]
section: "Modeles_Open"
---
## Takeaways

- InclusionAI released Ling 3.1 Flash 1019, scoring 44 on Artificial Analysis Intelligence Index v4.3, up from 41.
- 560B total parameters, 25B active, 1M context, MIT license, FP8 weights (~574 GB download).
- Agentic gains: AA-Briefcase Elo +124, AutomationBench-AA 62% to 68%, GDPval-AA Elo +36.
- Science reasoning: Humanity's Last Exam 39% to 43%, CritPt physics 18% to 26%.
- Pricing unchanged at $0.30/$0.90 per 1M tokens, but 40% more output tokens per task.
- Day-zero support in SGLang and vLLM; 1M context requires manual config beyond 256K.

Ant Groupâs InclusionAI team has released Ling 3.1 Flash 1019, an updated checkpoint of its hybrid reasoning model. It retains the original architecture, API prices, and MIT license while raising its Artificial Analysis Intelligence Index score from 41 to 44. Comparable open-weight models have a median score of 19.

The 1019 update improves agentic and scientific reasoning results, but it also generates substantially more output. Developers considering an upgrade should weigh those gains against higher per-task cost, longer latency, a 574 GB checkpoint, and additional configuration for the advertised one-million-token context window. InclusionAI has not attributed the gains to a specific training or post-training change.

## Inside the 560B sparse model

Ling 3.1 Flash 1019 uses a Mixture-of-Experts architecture with 560 billion total parameters and 25 billion active for each token. Activating only part of the network reduces inference compute compared with a dense 560B model, though deployment still requires cluster-scale hardware.

| Specification | Ling 3.1 Flash 1019 | 
|---|---|
| Total parameters | 560 billion | 
| Active parameters | 25 billion per token | 
| Context window | Up to 1 million tokens | 
| Attention stack | 63 KDA linear-attention layers and 9 gated MLA layers | 
| Expert routing | 512 routed experts, with 8 active per token | 
| Dense layers | 4 | 
| Reasoning mode | Enabled by default | 
| Checkpoint format | FP8 E4M3 with 128 Ã 128 block scaling | 
| License | MIT | 

The 72-layer attention stack uses a 7:1 ratio of KDA linear attention to gated multi-head latent attention. Linear attention limits the memory growth associated with long sequences, while the gated MLA layers retain full-attention capacity at selected points in the network.

## The million-token setting needs manual work

The Hugging Face checkpoint contains 54 weight shards of roughly 10.7 GB each, plus the tokenizer, chat template, and custom modeling code. The complete download is about 574 GB, before accounting for runtime overhead and the key-value cache used during inference.

Loading the model requires `trust_remote_code` for the custom `BailingMoeV3ForCausalLM` implementation. Because that option executes Python code from the repository, production deployments should pin a commit and review the code before loading it.

Stock SGLang and vLLM configurations default to a 256,000-token maximum. Reaching one million tokens requires an operator-supplied scaling configuration that is absent from the repository. The serving frameworks support the model, but their default setup does not expose its full context window.

## Agent and science scores rise

Artificial Analysis calculates its Intelligence Index from a collection of reasoning, coding, scientific, and agentic evaluations. Ling 3.1 Flash 1019 gains most clearly on tool-oriented agent tasks and scientific reasoning.

| Benchmark | Original | 1019 | Change | 
|---|---|---|---|
| AA-Briefcase v1.1 | 1397 Elo | 1521 Elo | +124 | 
| GDPval-AA v2.1 | 1617 Elo | 1653 Elo | +36 | 
| AutomationBench-AA | 62% | 68% | +6 percentage points | 
| Humanityâs Last Exam | 39% | 43% | +4 percentage points | 
| CritPt | 18% | 26% | +8 percentage points | 

AA-Briefcase, GDPval-AA, and AutomationBench-AA measure aspects of agent execution and task automation. Humanityâs Last Exam covers broad expert-level reasoning, while CritPt focuses on physics. Elo changes and percentage-point changes use different scales and should be read within each benchmark.

## Higher scores consume more tokens

Longer reasoning traces account for much of the updateâs higher task cost. The underlying token prices remain unchanged, but the model produces about 41% more output per Intelligence Index task than its predecessor.

| Usage metric | 1019 result | Comparison | 
|---|---|---|
| Total Index output | 350 million tokens | 140 million median for similarly sized open-weight models | 
| Output per task | 145,000 tokens | 103,000 for the original model | 
| Estimated cost per task | $1.49 | Driven mainly by longer output | 

Applications with strict latency or cost limits may need lower output caps, earlier stopping rules, or prompt changes that constrain the reasoning trace. Those controls can also affect task quality, so deployment tests should measure accuracy, latency, and token consumption together.

## Low token prices, heavy infrastructure

InclusionAIâs first-party API charges $0.30 per million input tokens and $0.90 per million output tokens. Cached input costs $0.06 per million tokens, an 80% discount.

| Token type | Price per million | 
|---|---|
| Input | $0.30 | 
| Cached input | $0.06 | 
| Output | $0.90 | 

At those listed rates, Ling 3.1 Flash 1019 undercuts Mistral Large 4 Preview and GLM 5.3 at similar Intelligence Index scores. Self-hosting removes dependence on the hosted endpoint but requires hundreds of gigabytes of accelerator memory, additional runtime headroom, and distributed inference infrastructure.

The MIT license permits commercial use, modification, and redistribution, subject to its license-notice requirement. SGLang and vLLM already support inference for the architecture, with the context-length configuration caveat described above.

## LingCode clouds coding comparisons

Several published coding results use LingCode, InclusionAIâs custom agent harness built on the open-source Pi agent. The harness targets long-running tasks that may require hundreds of tool calls and repeated context compaction.

Those results measure the combined behavior of the model and the agent system. InclusionAI labels the coding scores as unverified, and LingCode has not been released publicly. Independent teams therefore cannot reproduce the reported setup from public materials alone, and results from other coding agents may differ.

## Choosing between 1019 and the original

Deployment choice depends on whether the measured gains match the workload and justify the additional output, latency, and serving cost.

- **Use 1019 for:** extended tool-calling workflows, multi-step agents, and scientific reasoning tasks where its benchmark gains align with production requirements.
- **Keep the original for:** short chat, extraction, classification, and routing workloads that receive less benefit from longer reasoning traces.
- **Test before migrating:** compare both checkpoints with the same prompts, agent harness, output limits, and production-like traffic.

## A low API bill with a large serving footprint

Artificial Analysis places Ling 3.1 Flash 1019 near the price-performance frontier, where few measured models offer both a higher score and a lower API price. Its combination of MIT-licensed weights, 25 billion active parameters, framework support, and a configurable one-million-token context expands the options for teams building long-running agents.

The practical tradeoff remains concrete: hosted access is inexpensive, while self-hosting requires a large checkpoint, custom repository code, distributed hardware, and manual context scaling. The 1019 update is most useful when stronger agentic or scientific reasoning offsets its longer outputs.
