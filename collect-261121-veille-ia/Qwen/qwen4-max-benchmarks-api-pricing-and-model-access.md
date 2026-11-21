---
id: collect-261121-veille-ia/qwen/qwen4-max-benchmarks-api-pricing-and-model-access
title: "Qwen4-Max benchmarks, API pricing and model access"
domain: qwen
role: reference
task: article
actors: ["Alibaba"]
dates: []
keywords: ["model", "qwen4-max", "qwen", "tokens", "qwen3", "gpt-6", "november", "input"]
source: https://blog.invidelabs.com/qwen4-max-benchmarks-pricing/
source_anchor: ""
source_lines: [1, 64]
section: "Qwen"
---
Alibaba released Qwen4-Max on November 16, giving developers API access to a new flagship model for coding, visual input and agent workflows. QwenCloud lists it at $1.50 per million input tokens and $4.50 per million output tokens, 25% below the corresponding list prices for Qwen3.8-Max.

In Qwen's launch chart, Qwen4-Max posts the top DeepSWE v1.1 score among six models, ahead of Claude Opus 5.5, GPT-6.1 Sol, GPT-6 Astra and Kimi K3.1 by 0.2 to 1.6 points. It trails all four by 7.6 to 18.7 points on Terminal-Bench 4.0. Artificial Analysis estimates more than twice the median time per index task, and the smaller open-weight Qwen4 models are still to come.

## What is available and what changed?

Qwen4-Max follows a preview that began on November 11. The November 16 release is available through the QwenCloud API and Token Plan, as well as QwenWork, Qoder and QwenStudio. The API model ID is `qwen4-max`. Both Individual and Team editions of the Token Plan support it. The separate preview model remains available through November 25, 24:00 UTC+8, with credit consumption as low as 20% of the standard rate during the preview, according to QwenCloud. It also lists an additional 60% off between 22:00 and 08:00 UTC+8.

The QwenCloud listing says the model accepts text, images and video and produces text. It supports tool calling, structured outputs and web search, with OpenAI- and Anthropic-compatible APIs. The listed context window is one million tokens. The same page lists separate input and output limits: up to 991K input and 131K output tokens in the standard configuration, and 983K input and 262K output tokens in thinking mode. Qwen3.8-Max lists 131K output tokens in thinking mode. QwenCloud also lists a rate limit of 5 million tokens per minute for Qwen4-Max, compared with 2 million for Qwen3.8-Max. Check the current limits for the chosen endpoint and configuration before designing around the full window.

Qwen describes a mixture-of-experts architecture with a **2.4 trillion-parameter backbone**, of which 71 billion parameters are activated per token. Its launch post also describes an additional 328 billion N-gram embedding parameters. The active count describes the computation selected for a token; it is not the amount of memory needed to host the whole model. The cloud model is available to call now, so developers do not have to provision hardware to try it.

Qwen3.8-Max also had a 2.4 trillion-parameter backbone. Qwen says its new base model activates 25% fewer parameters per token than Qwen3.8-Max-Base. Its architecture changes include sparse attention for long sequences and gated residual connections.

## How does Qwen4-Max compare on coding tests?

Qwen's launch chart compares the new model with Qwen3.8-Max-0902 and several other flagships. The selected scores below are **as presented by Qwen**, and higher is better within each benchmark.

| Model | DeepSWE v1.1 | Terminal-Bench 4.0 | ProgramBench | 
|---|---|---|---|
| Qwen4-Max | 75.7 | 48.5 | 31.5 | 
| Qwen3.8-Max-0902 | 69.3 | 38.9 | 28.0 | 
| Claude Opus 5.5 | 74.2 | 59.6 | 65.0 | 
| GPT-6.1 Sol | 75.2 | 56.1 | 52.5 | 
| GPT-6 Astra | 74.1 | 59.1 | 55.0 | 
| Kimi K3.1 | 75.5 | 67.2 | 25.0 | 

*Compare models within each benchmark. Qwen's chart does not specify every model's test configuration.*

DeepSWE tests agentic coding. Terminal-Bench tests work in a terminal, and the same chart puts Qwen4-Max behind Opus 5.5, GPT-6.1 Sol, GPT-6 Astra and Kimi K3.1. ProgramBench tests black-box software replication: Qwen's 31.5 is above Kimi's 25.0 but well below the listed Opus and Sol results.

The launch chart combines results from different sources. Qwen identifies external leaderboards for some of its other rows, including Arena and Artificial Analysis, but the post does not specify every harness, reasoning setting or test configuration behind the coding scores above. Test terminal work and software replication separately from repository code changes.

On November 17, Artificial Analysis listed Qwen4-Max at 43 on its Intelligence Index. Its leaderboard placed Claude Fable 5.2 at maximum effort at 65, Gemini 4 Carbon at high effort at 60, and Claude Opus 5.5 at maximum effort at 59. The index combines nine evaluations across knowledge work, coding, science and general knowledge. Scores depend on the evaluated model and effort setting, and the combined index does not predict the outcome of a particular repository task.

## API pricing and cost per task

QwenCloud lists the following prices per million tokens, checked November 17:

| Token type | Qwen4-Max | Qwen3.8-Max | 
|---|---|---|
| Input | $1.50 | $2.00 | 
| Output | $4.50 | $6.00 | 
| Implicit cached input | $0.075 | $0.25 | 

Sources: Qwen4-Max and Qwen3.8-Max model listings. Explicit cache creation and reads have their own listed rates.

The input and output rates are each 25% lower for Qwen4-Max. The implicit cached-input rate is 70% lower. A repeated repository prefix that qualifies as a cache hit is billed at the cached-input rate, so a workload's cache-hit rate affects its cost. Provider and plan pricing can also differ from these QwenCloud list rates.

Qwen reports a larger reduction in its own Work360 test of professional tasks. It says average cost per task fell from $4.43 for Qwen3.8-Max to $1.67 for Qwen4-Max, a 62% reduction. Qwen says Qwen4-Max used approximately 24% fewer tokens at Xhigh effort and 35% fewer at High effort on Work360. On Terminal-Bench 4.0, it reports reductions of about 48% and 49%, respectively. These are comparisons with Qwen3.8-Max under Qwen's evaluation settings. A model that produces more reasoning tokens or needs another attempt can change the bill even when its per-token rate is lower.

Artificial Analysis separately lists an average cost of $3.27 per Intelligence Index task for Qwen4-Max. Its index uses a different set of tasks from Qwen's Work360 benchmark, so the two cost figures are not directly comparable.

Measure both completed tasks and token usage in the same agent setup. For long-running work, track elapsed time and retries too. Artificial Analysis estimates 32.1 minutes per Intelligence Index task for Qwen4-Max by dividing output tokens by output speed and weighting the results across its evaluations. The estimate uses Alibaba's API and is more than twice the 13.3-minute median for comparable models. Qwen4-Max generated 127 million output tokens across the index, compared with a median of 78.6 million for models in a similar price tier.

## Can you run Qwen4-Max locally?

Alibaba says smaller open-weight Qwen4 models are coming, but its launch announcement does not provide their release dates, sizes or licenses. It does not promise downloadable Qwen4-Max weights. The current Qwen4-Max path described in the announcement is hosted access through QwenCloud and Qwen applications.

That differs from the earlier Qwen3.8 generation, for which Alibaba subsequently released Max-class weights.

Start with representative repository changes and terminal tasks, record completed work, latency and total cost, and compare them against the models already in use.

## Member discussion
