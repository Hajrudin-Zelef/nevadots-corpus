---
id: collect-261121-veille-ia/tencent-hunyuan/tencent-hy4-flash-benchmarks-how-it-stacks-up-on-cost-vs-deepseek-and-hy4
title: "Tencent Hy4-Flash Benchmarks: How It Stacks Up on Cost vs DeepSeek and Hy4"
domain: tencent-hunyuan
role: reference
task: article
actors: []
dates: []
keywords: ["hy4-flash", "cost", "tencent", "benchmarks", "full", "benchmark", "task", "scores"]
source: https://www.mindstudio.ai/blog/tencent-hy4-flash-benchmarks
source_anchor: ""
source_lines: [1, 71]
section: "Tencent_Hunyuan"
---
## What is Hy4-Flash and why does its benchmark data matter?

Hy4-Flash is Tencent’s newest open-weight Hunyuan model, built to be the cheapest model in the Hy4 line to run while staying close to its bigger siblings on real task quality. It has 220 billion total parameters (including 64 billion Engram parameters) but only 12 billion active per token, a mixture-of-experts design meant to cut inference cost without gutting output quality. The benchmark data matters because it’s the clearest public evidence yet that Tencent is optimizing for cost per task, not just leaderboard scores, and the numbers show a real tradeoff: lower raw accuracy on academic benchmarks, but dramatically lower cost on the work-style tasks Tencent says it actually tracks.

## TL;DR

- **Hy4-Flash trades some raw intelligence for cost.** It scores below full Hy4 on most academic benchmarks (MMLU 88.7 vs 91.6, SuperGPQA 57.8 vs 62.7) but still beats DeepSeek-V4-Flash-Base on several knowledge and reasoning tests.
- **On Tencent’s internal WorkIndex benchmark, Hy4-Flash actually edges out Hy4 Preview** , scoring 42.1 versus 39.7, while costing a fraction as much per task.
- **The cost gap is the real story** : Hy4-Flash’s average cost per task is about 1/13 of Hy4 Preview, 1/20 of full Hy4, and 1/8 of DeepSeek-V4.1-Flash-1017 at Max tier peak-hour pricing.
- **In blind human evaluation** , Hy4-Flash scored 2.79 out of 4 on user satisfaction, nearly matching Claude Haiku 5.5’s 2.81, while finishing tasks in a median 6.7 minutes at roughly RMB 0.26 per task.
- **Architectural changes drive the savings** : hybrid sparse plus sliding-window attention, a latent-space MoE routing design, and n-gram “Engram” embeddings shrink KV cache size to 1/6 of full Hy4 and 1/24 of Hy3.
- **Tencent positions Hy4-Flash for everyday productivity work** (deep search, document editing, slide creation, simple frontend demos, computer use) and still recommends full Hy4 for long-horizon software engineering.

**›**Build a loan-approval app for 2,000 bankers.

*already included:*✓ SOC 2✓ role-based access✓ audit log✓ self-hosted

## How does Hy4-Flash compare to Hy4 on standard benchmarks?

On the few-shot pretrain benchmark suite, full Hy4 Base beats Hy4-Flash Base on nearly every listed metric, which is expected given the gap in active parameters and overall scale. On MMLU, Hy4 scores 91.6 against Hy4-Flash’s 88.7. On MMLU-Pro the gap is wider, 73.9 versus 71.1. SuperGPQA shows one of the larger drops, 62.7 for Hy4 versus 57.8 for Hy4-Flash. SimpleQA is the starkest gap in the table: Hy4 hits 57.2 while Hy4-Flash lands at 37.5, suggesting Flash’s smaller active parameter count costs it meaningfully on long-tail factual recall.

Despite that, Hy4-Flash generally holds its own or wins against Hy3 Preview, an earlier-generation model, and against DeepSeek-V4-Flash-Base on several categories. On Social IQa, Hy4-Flash scores 80.1, clearly ahead of DeepSeek-V4-Flash-Base’s 72.5. On U-MATH, Hy4-Flash posts 69.8 against DeepSeek-V4-Flash-Base’s 57.4. DeepSeek-V4-Pro-Base, the larger DeepSeek variant, generally outperforms both Hy4-Flash and sits closer to full Hy4 territory, topping the table on OCW Courses, MBPP-plus, and Belebele.

The pattern across the benchmark table is consistent: Hy4-Flash is not meant to beat full Hy4 on raw knowledge and reasoning tests, and it mostly doesn’t. It’s meant to get close enough while running far cheaper.

## What is WorkIndex and why does Hy4-Flash score higher than Hy4 Preview on it?

WorkIndex is Tencent’s internal benchmark, built around tasks drawn from its WorkBuddy product rather than from public academic test sets. It spans eight evaluation sets: slide creation, slide editing, human-AI slide co-writing, Markdown human-AI co-writing, document editing, spreadsheet repair, audio/video processing, and visual understanding. Tencent designed it specifically to measure things academic benchmarks miss: deliverable quality, how long a single-turn output runs, and how fast a task actually gets done.

On this benchmark, Hy4-Flash scores 42.1, ahead of Hy4 Preview’s 39.7. Full Hy4 still leads with 51.4, so Hy4-Flash isn’t surpassing the top-end model, but it does beat the “Preview” checkpoint that presumably represents an earlier or lighter version of the full model. That’s a meaningful result: Tencent’s post-training pipeline for Flash includes self-distillation from full Hy4 plus reinforcement learning that cut average end-to-end output tokens by half, and that combination appears to have produced a model that’s more efficient at completing WorkIndex-style tasks than an earlier Hy4 checkpoint, even though it’s far smaller on paper.

Tencent also ran Hy4-Flash against seven public agent benchmarks (DeepSWE, ProgramBench, Terminal-Bench 4.0, APEX-Agents, AutomationBench, HLE, and OSWorld 2.1) under the “Open Benchmarks” cost/performance comparison, though the model card presents those results as a combined chart rather than a full numeric table.

## How much cheaper is Hy4-Flash per task than DeepSeek and Hy4?

This is where the model card makes its strongest claims. Average cost per task on WorkIndex for Hy4-Flash comes in at roughly 1/13 the cost of Hy4 Preview, 1/20 the cost of full Hy4, and 1/8 the cost of DeepSeek-V4.1-Flash-1017 under Max tier, peak-hour pricing. Those are steep multiples, and they come from a mix of fewer active parameters, a shorter KV cache, and the reinforcement learning step that trims output length.

Build an onboarding app for 4,000 new hires.

Build ↵
The architecture backs up the cost story. Hy4-Flash interleaves sparse attention (IndexPool) with sliding-window attention at a 1:3 ratio, uses a latent-space mixture-of-experts layout where each token activates only 16 of 512 routed experts plus one shared expert, and adds n-gram “Engram” embeddings (bigrams and trigrams retrieved through hash lookups) to boost quality without inflating the active compute path. Combined, these cuts bring KV cache size down to 1/6 of full Hy4 and 1/24 of Hy3. On the inference side, Tencent pairs 4-bit quantization with a built-in speculative decoding module called DFly2 (a 5-layer, 0.85B parameter drafter that reuses the target model’s KV cache) to push decoding speed past 150 tokens per second.

## Is Hy4-Flash worth using over the full Hy4 model?

It depends on the task. Tencent’s own guidance is explicit: use Hy4-Flash for everyday productivity work like deep search, document processing, slide creation, simple frontend demos, and computer use. Reach for full Hy4 when the job is long-horizon software engineering or anything that needs the higher intelligence ceiling that comes with more active parameters.

The blind evaluation data supports treating Flash as a legitimate default rather than a budget fallback. Across 259 tasks spanning 27 categories, tested with 85 internal WorkBuddy users, Hy4-Flash scored 2.79 out of 4 on user satisfaction, just behind Claude Haiku 5.5’s 2.81. It also posted the lowest cost per task and the shortest median completion time among the models in that comparison, finishing in a median 6.7 minutes at about RMB 0.26 per task. That combination, near-parity satisfaction with a frontier-adjacent model at the lowest cost and fastest turnaround, is a stronger practical signal than the raw academic benchmark table, where Flash trails both full Hy4 and DeepSeek-V4-Pro-Base.

For teams building agent or productivity tooling where cost per task and latency matter as much as peak accuracy, Hy4-Flash’s numbers suggest it’s a reasonable default, with an easy escalation path to full Hy4 for harder jobs.

## Frequently Asked Questions

### What are Hy4-Flash’s total and active parameter counts?

Hy4-Flash has 220 billion total parameters, including 64 billion Engram parameters, with 11.3 billion active text parameters plus 0.43 billion active vision parameters per token, for roughly 12 billion active parameters total.

### How does Hy4-Flash perform against DeepSeek-V4 variants?

Against DeepSeek-V4-Flash-Base, Hy4-Flash wins on several benchmarks including Social IQa and U-MATH, but DeepSeek-V4-Pro-Base generally scores higher across knowledge, math, and code benchmarks. On cost, Hy4-Flash claims roughly 1/8 the cost per task of DeepSeek-V4.1-Flash-1017 at Max tier peak-hour pricing on WorkIndex.

### What is the context window for Hy4-Flash?

Hy4-Flash supports a context window of up to 1,048,576 tokens (1M), enabled in part by its hybrid sparse and sliding-window attention design and the DFly2 speculative decoding module for long-context efficiency.

### Does Hy4-Flash support image inputs?

Yes. Hy4-Flash natively accepts text and image inputs and generates text output, using a vision encoder adapted from SigLIP2-So400m-Patch16-Res512 connected to the language backbone through an MLP projector.

### What formats is Hy4-Flash released in?

Tencent released three versions: Hy4-Flash (instruct model), Hy4-Flash-FP8 (FP8 quantized instruct model), and Hy4-Flash-base (pre-trained base model, not instruction-tuned), available on Hugging Face, ModelScope, and GitCode.
