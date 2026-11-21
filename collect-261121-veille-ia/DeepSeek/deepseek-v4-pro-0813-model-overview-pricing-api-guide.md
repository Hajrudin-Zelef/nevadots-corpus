---
id: collect-261121-veille-ia/deepseek/deepseek-v4-pro-0813-model-overview-pricing-api-guide
title: "DeepSeek V4 Pro 0813: Model Overview, Pricing & API Guide"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: []
keywords: ["deepseek", "model", "preview", "effort", "tasks", "release", "guide", "table"]
source: https://deepinfra.com/blog/deepseek-v4-pro-model-overview
source_anchor: ""
source_lines: [1, 122]
section: "DeepSeek"
---
*Updated November 2026 for the DeepSeek-V4-Pro-0813 release.*

DeepSeek V4 Pro 0813 is the general-availability release of DeepSeek’s flagship V4 Pro model, published by DeepSeek on August 13, 2026 and available on DeepInfra since August 18, 2026. It is a text-in, text-out Mixture-of-Experts model with 1.6 trillion total and 49 billion active parameters, a 1M-token context window and an MIT license that allows commercial use (Hugging Face model card).

0813 is not a new architecture. DeepSeek describes it as “the official release of DeepSeek-V4-Pro, superseding the preview version,” built on the same model structure as the April 24 preview, with new post-training and a DSpark speculative decoding module attached. The practical changes are large agentic and coding gains (vendor-reported), a three-level `reasoning_effort` control and new sampling guidance.

DeepInfra serves the model in fp8 with the full 1M context and up to 384K output tokens, under a zero-retention policy. The model string for this guide is **`deepseek-ai/DeepSeek-V4-Pro-0813`**.

Every benchmark number in this section is **vendor-reported**: DeepSeek ran them with the minimal mode of its own DeepSeek Harness, at `max` reasoning effort, temperature 1.0 and top_p 0.95 for code-agent tasks (model card). No independent party has reproduced them.

The largest jump is on long-horizon software tasks:

`reasoning_effort`:`low` for simple tasks, `high` for daily agent workflows, `max` for complex tasks (DeepSeek release notes). The preview used three modes (Non-think, Think High, Think Max) and triggered Think Max with a special system prompt. That is no longer needed.
Context length (1M), modality (text only) and the base architecture are unchanged from the preview.

Artificial Analysis scores 0813 (Max) at **23 on its Intelligence Index v5.0.1**, exactly the median for open-weights models of similar size, and 21 at High effort. That index is weighted toward agentic and knowledge tasks run by AA itself, so it is a useful counterweight to the vendor table. AA has no v5.0.1 score for the preview, so there is no independent before/after comparison, and older index versions are not comparable. On the same page, DeepSeek-V4.1-Flash scores higher than 0813.

The architecture is the one DeepSeek introduced with the V4 preview, its first new design since V3:

The table below is DeepSeek’s own, run with its harness at `max` effort. DSBench-FullStack and DSBench-Hard are internal DeepSeek test sets that outsiders cannot reproduce.

| Benchmark | V4 Pro 0813 | V4 Pro (Preview) | GLM-5.2 | Kimi K3 | Opus-4.8 | Fable-5 | 
|---|---|---|---|---|---|---|
| HLE (no tools / with tools) | 42.7 / 60.0 | 37.7 / 48.2 | 40.5 / 54.7 | 43.5 / 56.0 | 49.8 / 57.9 | 53.3 / 63.0 | 
| Terminal Bench 2.1 | 87.9 | 72.1 | 81.0 | 88.3 | 85.0 | 88.0 | 
| NL2Repo | 61.5 | 38.5 | 48.9 | – | 69.7 | – | 
| Cybergym | 83.3 | 52.7 | – | 80.0 | 78.3 | 83.1 | 
| DeepSWE | 62.7 | 12.8 | 46.2 | 67.5 | 58.0 | 70.0 | 
| Toolathlon-Verified | 74.1 | 55.9 | 59.9 | 76.5 | 76.2 | 77.9 | 
| Agents’ Last Exam | 25.7 | 16.5 | 23.8 | 27.6 | 25.7 | – | 
| AutomationBench (Public) | 31.8 | 12.8 | 12.9 | 30.8 | 27.2 | 29.1 | 
| DSBench-FullStack (internal) | 71.1 | 41.8 | 61.8 | 73.7 | 71.6 | 77.2 | 
| DSBench-Hard (internal) | 67.2 | 31.1 | 54.5 | 63.0 | 71.7 | 68.3 | 

*Source: DeepSeek-V4-Pro-0813 model card. Fable-5 is DeepSeek’s “w/ fallback” configuration.*

**Who leads which row.** 0813 has the top score in the table on Cybergym and AutomationBench, and ties Opus-4.8 on Agents’ Last Exam. The closed models lead on HLE (both settings, Fable-5), NL2Repo (Opus-4.8), DeepSWE, Toolathlon and DSBench-FullStack (Fable-5), and DSBench-Hard (Opus-4.8).

**Where it loses to open weights.** Kimi K3 beats 0813 on HLE without tools, Terminal Bench 2.1, DeepSWE, Toolathlon, Agents’ Last Exam and DSBench-FullStack. 0813 leads the open-weights models in the table on HLE with tools, NL2Repo, Cybergym, AutomationBench and DSBench-Hard.

**Gaps vs the preview.** The widest gain is DeepSWE (+49.9 points); the narrowest is HLE without tools (+5.0). DeepSeek did not republish the preview’s knowledge and math rows (MMLU-Pro, GPQA, LiveCodeBench, Codeforces) for 0813, so those April numbers should not be read as 0813 results.

To see how 0813 compares with other models on DeepInfra, use the model comparison page. For provider-level speed and latency, see our DeepSeek V4 Pro API benchmarks.

DeepSeek V4 Pro 0813 has no promotional rate on DeepInfra; the standard price is the list price. Pricing as of 2026-11-16, per 1M tokens (model page, pricing page):

| Tier | Input | Output | Cached input | 
|---|---|---|---|
| Standard | $1.30 | $2.60 | $0.10 | 
| Flex (0.8×) | $1.04 | $2.08 | $0.08 | 
| Priority (1.5×) | $1.95 | $3.90 | $0.15 | 

Among the eight providers Artificial Analysis tracks for this model, DeepInfra has the **lowest output price** ($2.60), the second-lowest blended price ($0.59 on AA’s 7:2:1 cache-hit:input:output blend) and a **0% error rate** in AA’s runs. Output price matters more than usual here: at `max` effort, AA measured 119M output tokens to run its Intelligence Index, against a class median of 104M. Reasoning tokens are billed as output tokens. DeepInfra is not the fastest endpoint for this model; on AA, DigitalOcean and Baseten lead on output speed and time to first answer token.

**Flex** is a best-effort tier at 0.8× the standard price. Send `"service_tier": "flex"`; if capacity is short, the request waits up to 10 minutes and then returns HTTP 429, and a rejected Flex request is not charged. It suits batch jobs, evals and background agents that can retry. Details are in the Flex service tier announcement.

**Priority** (`"service_tier": "priority"`) costs 1.5× and is for latency-sensitive traffic that should not queue.

**Caching:** cached input is $0.10, 92% below the standard input rate. For long agent sessions you can keep the cache warm for up to an hour with `prompt_cache_options` (see prompt cache retention).

Startups can apply for free credits through DeepStart. For worked cost scenarios and a comparison with other providers, see the DeepSeek V4 Pro pricing guide.

The API is OpenAI-compatible:

`https://api.deepinfra.com/v1/openai``POST /chat/completions``Authorization: Bearer $DEEPINFRA_API_KEY`
Full request schema is on the model’s API page. The model supports JSON mode, structured output and tool calling.

Model-specific parameters first. Pass non-standard fields through `extra_body` in the OpenAI SDK.

| Parameter | Default | Notes | 
|---|---|---|
| `reasoning_effort` | `high` | DeepSeek’s three levels: `low` ,`high` ,`max` .`"none"` turns thinking off. Reasoning tokens bill as output | 
| `top_p` | 1 | DeepSeek recommends **0.95 for agentic use** , 1.0 otherwise | 
| `temperature` | 1 | DeepSeek recommends 1.0 | 
| `service_tier` | standard | `flex` (0.8×, may queue up to 10 min then 429) or`priority` (1.5×) | 
| `fail_fast` | off | Return 429 immediately instead of queueing | 
| `prompt_cache_key` ,`prompt_cache_options` | n/a | Route requests to a shared cache; e.g. `{"ttl": "1h"}` | 
| `model` | (required) | `deepseek-ai/DeepSeek-V4-Pro-0813` | 
| `messages` | (required) | Standard chat messages | 
| `max_tokens` | min(context − input, 65,536) | Up to 393,216 (384K) | 
| `stream` | false | Server-sent events | 
| `stop` | none | Up to 16 sequences | 
| `n` | 1 | Up to 4 | 
| `top_k` ,`min_p` | 0 | Optional sampling controls | 
| `response_format` | text | `{"type": "json_object"}` or a JSON schema | 
| `tools` ,`tool_choice` | none | OpenAI-style function calling | 

```
import os
from openai import OpenAI
client = OpenAI(
    api_key=os.environ["DEEPINFRA_API_KEY"],
    base_url="https://api.deepinfra.com/v1/openai",
)
response = client.chat.completions.create(
    model="deepseek-ai/DeepSeek-V4-Pro-0813",
    messages=[
        {"role": "system", "content": "You are a senior backend engineer."},
        {"role": "user", "content": "Plan a migration from REST polling to webhooks for an order service."},
    ],
    temperature=1.0,
    top_p=0.95,          # DeepSeek's recommendation for agentic work
    max_tokens=32768,
    extra_body={
        "reasoning_effort": "high",   # low | high | max; "none" disables thinking
        "service_tier": "flex",       # drop this line for standard tier
    },
)
print(response.choices[0].message.content)
print(response.usage)
```
For simple classification or extraction, try `"reasoning_effort": "low"` or `"none"` first; output tokens, and so cost, drop sharply. Reserve `max` for hard multi-step tasks.

DeepInfra’s common 16,384-token output cap does **not** apply to this model. You can request up to **393,216 output tokens (384K)**. If you leave `max_tokens` unset, DeepInfra uses the smaller of the remaining context and **65,536 tokens**. Input plus output must fit in the 1,048,576-token context window.

This matters because DeepSeek’s published results ran at `max` effort, and DeepSeek recommends a maximum output length of 384K tokens for `high` and `max`. A long reasoning trace can use most of a 65,536-token default before the final answer begins. To get closer to DeepSeek’s evaluation setup, set `max_tokens` explicitly (for example 131,072 or higher), use `max` effort, and stream the response so long generations do not hit client timeouts. Exact reproduction is still not guaranteed: DeepSeek used its own harness, and DeepInfra serves the model in fp8.

DeepSeek V4 Pro 0813 is a substantial update to the April preview on the same architecture. On DeepSeek’s own benchmarks it moves from far behind to competitive on agentic coding (DeepSWE, NL2Repo, Terminal Bench) and leads the compared set on Cybergym. Closed models still lead on hard reasoning (HLE) and several software-agent tests, Kimi K3 edges it on a handful of rows, and Artificial Analysis puts it at the class median on its independent index.

Where it fits best is long-context agent work that produces many output tokens, where DeepInfra’s $2.60 output rate (or $2.08 on Flex) does the most good. Teams that need faster time to first answer token or a stronger model per dollar on Artificial Analysis’s index should also test DeepSeek-V4.1-Flash.

**Next steps**
