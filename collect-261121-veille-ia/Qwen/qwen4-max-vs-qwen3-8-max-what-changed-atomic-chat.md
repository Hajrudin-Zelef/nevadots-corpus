---
id: collect-261121-veille-ia/qwen/qwen4-max-vs-qwen3-8-max-what-changed-atomic-chat
title: "Qwen4-Max vs Qwen3.8-Max: What Changed? - Atomic Chat"
domain: qwen
role: reference
task: article
actors: ["Alibaba"]
dates: []
keywords: ["qwen3", "qwen4-max", "alibaba", "token", "high", "model", "tokens", "reasoning"]
source: https://atomic.chat/blog/llm-updates/qwen4-max-vs-qwen3-8-max-4dae3
source_anchor: ""
source_lines: [1, 126]
section: "Qwen"
---
Alibaba made Qwen4-Max generally available on QwenCloud and it replaces Qwen3.8-Max as the top Qwen model.

It is the first model of the Qwen4 generation. At the same 2.4T size, it activates 25% fewer parameters per token, uses a new sparse attention layer and costs less per token, with the biggest cut on cached input. If you run Qwen3.8-Max through the API, switching makes sense for agent, office and coding work; batch jobs and self-hosted setups should stay on Qwen3.8-Max for now.

Qwen4-Max against Qwen3.8-Max-0902 and four other frontier models on 16 benchmarks. Scores as reported in Alibaba's launch post.

Score against tokens per task at low, high and xhigh reasoning effort, Qwen4-Max vs Qwen3.8-Max-0902. Scores as reported by Alibaba.

## Qwen4-Max vs Qwen3.8-Max at a glance

Sources: QwenCloud pages for Qwen3.8-Max and Qwen4-Max (Singapore prices), Qwen4-Max docs, Qwen3.8 model card.

## What changed in the architecture

Both are 2.4T MoE models, but Qwen4-Max activates 71B parameters per token instead of 95B, so each token takes about 25% less compute.

The attention layers changed too. Qwen3.8-Max pairs three linear-attention layers with each full-attention layer. Qwen4-Max keeps that 3:1 layout but replaces full attention with Qwen Sparse Attention, which reads only the parts of the context an indexer selects. As a result, long prompts take less compute.

Fewer active parameters didn't cost quality in Alibaba's base-model tests. The Qwen4-Max base model predicts text 5â€“13% better than the Qwen3.8-Max base model, measured in bits per byte. Alibaba hasn't published a full model card for Qwen4-Max yet.

## How the benchmark results changed

The baseline in the Qwen4-Max launch benchmarks is Qwen3.8-Max-0902, the September update of Qwen3.8-Max. The gains are uneven: agent and professional benchmarks moved by 9 to 20 points, coding benchmarks by 3 to 10.

Agent work improved the most. Agents' Last Exam, which tests long multi-step tasks, rose from 27.9 to 46.7, and AutomationBench-AA, which tests SaaS workflows, from 56.2 to 72.5. Outside agent tasks, the largest jump is on HealthBench Professional, a clinical-reasoning test, from 39.6 to 60.0.

Coding moved less. DeepSWE gained 6.4 points and ProgramBench 3.5 (from 28.0 to 31.5). Terminal-Bench 4.0 gained 9.6, but other frontier models still score higher there: Claude Opus 5.5 at 59.6 and Kimi K3.1 at 67.2.

On the independent Artificial Analysis Intelligence Index v5.0.1, Qwen4-Max scores 43 and Qwen3.8-Max (0902) scores 34. The median for comparable models is 41.

## Reasoning modes and effort

Thinking is on by default in both models, and both take a `reasoning_effort` setting. Qwen4-Max has three levels, `low`, `high` and `xhigh`, with `high` as the default; `enable_thinking=false` turns thinking off. By default it spends up to 131,072 tokens thinking.

Effort changes both the score and the token bill. Approximate values from Alibaba's token-efficiency chart:

On Work360, Qwen4-Max at the default `high` already beats Qwen3.8-Max at `xhigh` with about 40% fewer tokens, so `high` is enough for office and document work. On Terminal-Bench 4.0, `high` only matches Qwen3.8-Max, though with half the tokens; the jump to 48 comes at `xhigh`, for about 20% more tokens than `high`. Terminal coding needs `xhigh`.

Two settings matter for multi-turn agents. `preserve_thinking` is on by default, as in Qwen3.8-Max, so earlier reasoning should go back in assistant messages. CoT Summary is new: the API returns a readable summary of the reasoning, and the full reasoning comes back encrypted for the next turn.

## Token use and pricing

Across the broader Artificial Analysis index, the token saving is smaller than on Alibaba's two benchmarks above: 127M output tokens against 150M, about 15%.

Input and output prices fell by 25%. Cached input fell further:

The largest savings show up on multi-step agent tasks. On the Artificial Analysis index, the cost per task fell from $10.02 to $3.27, about three times lower. On Alibaba's Work360, it fell from $4.43 to $1.67, about 2.7 times lower. A short request without cache hits gets less: the 25% price cut plus whatever token saving that task has.

## Availability and how to start

Qwen4-Max has been generally available since November 16, 2026. It runs on:

- the QwenCloud API, pay-as-you-go, with the model ID `qwen4-max` ;
- Token Plan Personal and Team subscriptions, billed in credits;
- QwenStudio, QwenWork and Qoder.

Code written for Qwen3.8-Max runs on Qwen4-Max: the endpoints, the context window, image and video input and tool calling stay the same. The supported APIs are OpenAI Chat Completions, OpenAI Responses, Anthropic Messages and DashScope, with setup guides for Claude Code, Codex and OpenCode. Explicit cache can now last an hour instead of 5 minutes, so a prefix stays cached even when calls are more than 5 minutes apart.

## Does Qwen4 have open weights?

Not yet. Smaller Qwen4 models will be open-sourced, built with the same techniques as Qwen4-Max. At the Apsara Conference in September, Alibaba showed a Qwen4 lineup of Max, Plus, Flash and 27B. The weights haven't been published yet, and there's no date. We'll update this section when they appear.

Neither Max model fits on a local machine: 2.4T parameters at 16 bits take about 4.8 TB. For local use, Atomic Chat runs the open Qwen3.8 models, such as Qwen3.8 27B or Qwen3.8 Flash Next, as GGUF or MLX builds.

## Our quick test

We gave both models the same prompt in Qwen Chat with thinking on:

Build a single self-contained HTML file (inline CSS and JS, no external libraries or fonts): a VRAM calculator for running local LLMs.

Inputs: parameters in billions; weight quantization (FP16, Q8_0, Q6_K, Q5_K_M, Q4_K_M); context length in tokens; number of layers; number of KV heads; head dimension; KV cache precision (FP16 or Q8_0).

Outputs: memory for weights, KV cache, runtime overhead and total, in GB; a stacked bar showing the split; and whether the total fits in 8, 16, 24, 32 and 64 GB.

Show the formulas you used under the result. Prefill the inputs with Llama 3.1 8B: 8B parameters, Q4_K_M, 8192 context, 32 layers, 8 KV heads, head dimension 128, FP16 KV cache.

Return only the HTML in one code block.

For these inputs the KV cache has one correct answer, 1.07 GB (exactly 1 GiB), so the test checks the math as well as the interface. One run per model, November 16, 2026.

Qwen4-Max: KV cache 1.07 GB, weights 4.83 GB, plus model presets, a GB/GiB switch and GPU examples for each memory tier.

Qwen3.8-Max: correct values in GiB, labeled as GB, so every figure reads about 7% low.

Both models got the formulas right. Qwen3.8-Max calculated in GiB but labeled the results GB. Qwen4-Max got the units right and built more than the prompt asked for: presets for seven models, GPU examples for each memory tier and how much context fits in 24 GB. It also took three and a half times longer. One run on launch day can't separate a slower model from a busy service, so treat the timing as a data point, not a speed benchmark.

## Early hands-on reports

Most hands-on reports so far used the preview build that opened on November 11, not the general release, and each covers a handful of prompts.

- Front-end and visual output got faster and cleaner. In a three-prompt head-to-head in Qwen Chat with thinking on, both models passed every task, and Qwen4-Max finished faster with a noticeably cleaner visual result.
- Self-correction improved. In MindStudio's tests, the model overthought less than Qwen 3 models, caught and fixed its own labeling mistake on a map, and spotted an incorrectly worn seatbelt strap in a photo that a Qwen 3 model missed.
- Not every result favors Qwen4-Max. On the game-balance category of the GameFeel benchmark, the preview scored 10.3 against 12.1 for Qwen3.8-Max. Other testers report overthinking on simple tasks and slow runs on long prompts.

## Which model to use

## Frequently asked questions

### Do I need to change my code to switch from Qwen3.8-Max?

Usually only the model ID, from `qwen3.8-max` to `qwen4-max`. Check `max_completion_tokens`: it now covers up to 262,144 tokens of thinking and answer together. On the native DashScope API, Qwen4-Max uses the multimodal endpoint even for text-only requests.

### Can I use Qwen4-Max for batch jobs?

Not yet. Qwen4-Max has no batch API. Qwen3.8-Max batch calls cost $1 input and $3 output per 1M tokens, below Qwen4-Max's standard $1.50 and $4.50, so batch work stays cheaper on Qwen3.8-Max for now.

### Can I run Qwen4 locally?

Not yet. Qwen4-Max is cloud-only, and the smaller open Qwen4 models haven't been released. The local option today is the Qwen3.8 family: Qwen3.8 27B and Qwen3.8 Flash Next run in Atomic Chat.

## Sources

Checked November 16, 2026.

- Qwen Team: Qwen4-Max: Your New Teammate Has Arrived
- Qwen Team: Qwen3.8-Max: A New Bar for Coding and Cowork
- QwenCloud docs: Latest model: Qwen4-Max
- QwenCloud model pages: Qwen4-Max, Qwen3.8-Max
- Hugging Face: Qwen/Qwen3.8-2.4T-A95B model card
- Artificial Analysis: Qwen4 Max, Qwen3.8 Max (0902)
- TechNode: Alibaba launches Qwen4-Max with lower API prices
- Pasquale Pillitteri: Alibaba unveils Qwen 4 at Apsara
- MindStudio: Qwen4 Max Hands-On: Testing Alibaba's New Model on Real Tasks
- Medium: I Tested Qwen4-Max Against Qwen 3.8 Max
- Offloop: Qwen4-Max Preview review: coding tests and GameFeel scores
- DataCamp: Qwen4-Max: Features, Benchmarks, and Pricing
