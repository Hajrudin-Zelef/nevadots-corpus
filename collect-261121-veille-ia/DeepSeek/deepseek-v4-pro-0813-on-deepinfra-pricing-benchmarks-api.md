---
id: collect-261121-veille-ia/deepseek/deepseek-v4-pro-0813-on-deepinfra-pricing-benchmarks-api
title: "DeepSeek V4 Pro 0813 on DeepInfra: Pricing, Benchmarks & API"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: ["2026-08-13"]
keywords: ["deepseek", "model", "deepswe", "deepinfra", "deepseek-v4-pro-0813", "terminal", "preview", "tools"]
source: https://deepinfra.com/blog/deepseek-v4-pro-deepinfra
source_anchor: ""
source_lines: [1, 128]
section: "DeepSeek"
---
*Updated November 2026 for the DeepSeek-V4-Pro-0813 release.*

DeepSeek’s GA release of V4 Pro moves its DeepSWE score from 12.8 to 62.7 and Terminal Bench 2.1 from 72.1 to 87.9, according to DeepSeek’s model card (vendor-reported). DeepSeek-V4-Pro-0813 shipped on August 13, 2026 and supersedes the April preview. It keeps the same 1.6-trillion-parameter Mixture of Experts design with 49B active parameters and a 1M-token context window, and adds new post-training, a built-in speculative decoding module and a three-level `reasoning_effort` control.

The DeepSeek V4 Pro 0813 release is built for agentic work: coding agents that edit whole repositories, terminal and tool-use loops, security tasks and long multi-step automations. DeepSeek-V4-Pro-0813 is now available on DeepInfra, live since August 18, 2026, at $1.30 per 1M input tokens and $2.60 per 1M output tokens. That is the lowest output price among the eight providers Artificial Analysis measures for this model, and the Flex tier takes output down to $2.08.

The weights are on Hugging Face under an MIT license, so self-hosting stays an option. DeepInfra runs the model for you on an OpenAI-compatible endpoint, so you can skip provisioning a 1.6T-parameter deployment.

| Benchmark | V4-Pro-0813 | V4-Pro (Preview) | Δ vs preview | GLM-5.2 | Kimi K3 | Opus-4.8 | Fable-5 (w/ fallback) | 
|---|---|---|---|---|---|---|---|
| HLE (no tools) | 42.7 | 37.7 | +5.0 | 40.5 | 43.5 | 49.8 | 53.3 | 
| HLE (with tools) | 60.0 | 48.2 | +11.8 | 54.7 | 56.0 | 57.9 | 63.0 | 
| Terminal Bench 2.1 | 87.9 | 72.1 | +15.8 | 81.0 | 88.3 | 85.0 | 88.0 | 
| NL2Repo | 61.5 | 38.5 | +23.0 | 48.9 | – | 69.7 | – | 
| Cybergym | 83.3 | 52.7 | +30.6 | – | 80.0 | 78.3 | 83.1 | 
| DeepSWE | 62.7 | 12.8 | +49.9 | 46.2 | 67.5 | 58.0 | 70.0 | 
| Toolathlon-Verified | 74.1 | 55.9 | +18.2 | 59.9 | 76.5 | 76.2 | 77.9 | 
| Agents’ Last Exam | 25.7 | 16.5 | +9.2 | 23.8 | 27.6 | 25.7 | – | 
| AutomationBench (Public) | 31.8 | 12.8 | +19.0 | 12.9 | 30.8 | 27.2 | 29.1 | 
| DSBench-FullStack* | 71.1 | 41.8 | +29.3 | 61.8 | 73.7 | 71.6 | 77.2 | 
| DSBench-Hard* | 67.2 | 31.1 | +36.1 | 54.5 | 63.0 | 71.7 | 68.3 | 

*Internal DeepSeek test sets. All scores are vendor-reported from the DeepSeek-V4-Pro-0813 model card, run by DeepSeek at `max` effort; peer columns are as DeepSeek reported them.*

0813 improves on the preview in every row. The biggest jumps are on agentic coding: DeepSWE (+49.9 points), Cybergym (+30.6) and repository-level generation on NL2Repo (+23.0). It leads the whole comparison set on Cybergym (83.3, just ahead of Fable-5 at 83.1) and AutomationBench (31.8). Among the open-weights models in the table, it also leads HLE with tools, NL2Repo and DSBench-Hard.

It doesn’t win everywhere. Closed models lead most rows: Fable-5 on HLE, DeepSWE, Toolathlon and DSBench-FullStack, and Opus-4.8 on NL2Repo and DSBench-Hard. The open-weights Kimi K3 edges 0813 on Terminal Bench 2.1 (88.3 vs 87.9), DeepSWE (67.5 vs 62.7), Toolathlon, Agents’ Last Exam and HLE without tools. DeepSeek ran every number above, and the two DSBench rows use internal test sets that nobody else can reproduce.

For an independent view, Artificial Analysis scores DeepSeek V4 Pro 0813 at 23 on its Intelligence Index v5.0.1 at Max effort. That equals the median for open-weights models of its size. On the same index, DeepSeek’s newer V4.1 Flash scores higher. Read the table above as evidence for agentic coding and tool use, not as a general intelligence ranking.

DeepSeek V4 Pro is a 1.6-trillion-parameter Mixture of Experts model that activates 49B parameters per token. Each token is routed to 6 of 384 experts plus a shared expert, so you get the capacity of a very large model at the per-token compute of a much smaller one.

The main architectural change from DeepSeek V3.2 is hybrid attention, which combines Compressed Sparse Attention (CSA) with Heavily Compressed Attention (HCA). DeepSeek says that at 1M tokens of context, V4 Pro needs 27% of the single-token inference FLOPs and 10% of the KV cache that V3.2 needs. Manifold-Constrained Hyper-Connections (mHC) stabilize signal flow across the 61 layers, and pre-training used the Muon optimizer on more than 32 trillion tokens. 0813 keeps all of this. The base structure is unchanged from the preview.

DeepSeek describes 0813 as the official release of V4 Pro, built on the preview’s model structure with new post-training and a speculative decoding module attached. In practice:

`reasoning_effort` replaces reasoning modes.`low` for simple tasks, `high` for everyday agent workflows and `max` for the hardest problems.
If you built on the preview, check four things:

`"none"` turns thinking off entirely, and the reasoning docs list `"high"` as the default.`max_tokens` for long reasoning.`deepseek-ai/DeepSeek-V4-Pro-0813`.
The model is text-only, so use DeepInfra’s multimodal model catalog if your pipeline needs image input. Deep reasoning also takes time: at Max effort, Artificial Analysis measures tens of seconds before the first answer token on every provider. Faster endpoints exist (DigitalOcean leads output speed on Artificial Analysis); if response speed is your product, test them. For batch, agents and cost-sensitive workloads, the per-token gap compounds.

DeepSeek-V4-Pro-0813 is available now in DeepInfra’s text generation catalog. Pricing is per token, with no minimums, and there’s no promotional discount on this model, so the standard rate is the list price to budget against.

| Tier | Input | Output | Cached input | 
|---|---|---|---|
| Standard | $1.30 | $2.60 | $0.10 | 
| Flex | $1.04 | $2.08 | $0.08 | 
| Priority | $1.95 | $3.90 | $0.15 | 

*Per 1M tokens. Pricing as of 2026-11-16. Source: model page and pricing page.*

Output is where the price gap shows. On Artificial Analysis’s providers page, six of the other seven providers list $3.96 per 1M output tokens, and the seventh lists $3.17. DeepInfra’s $2.60 is 34% lower than $3.96, and Flex at $2.08 is 47% lower. Artificial Analysis shows DeepSeek’s own API at its peak rate; DeepSeek charges half that off-peak. Artificial Analysis also recorded a 0% error rate on DeepInfra’s endpoint. Long agent runs produce a lot of output, including reasoning tokens, so the output rate tends to drive the bill.

**Choosing a tier:**

`"service_tier": "flex"`. A Flex request waits up to 10 minutes for capacity and then returns HTTP 429, and rejected Flex requests aren’t charged. It suits batch jobs, evals and background agents. See how the Flex tier works.`"service_tier": "priority"` for latency-sensitive traffic that shouldn’t queue.
**Model specs on DeepInfra:**

The endpoint is OpenAI-compatible. Point your client at `https://api.deepinfra.com/v1/openai`, call `/chat/completions` and authenticate with `Authorization: Bearer $DEEPINFRA_API_KEY`. DeepInfra runs a zero-retention data policy and is SOC 2 and ISO 27001 certified. If you need dedicated compute, GPU instances are available for private deployments.

Get an API key from the DeepInfra dashboard and export it as `DEEPINFRA_API_KEY`.

**curl**

```
curl "https://api.deepinfra.com/v1/openai/chat/completions" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $DEEPINFRA_API_KEY" \
  -d '{
      "model": "deepseek-ai/DeepSeek-V4-Pro-0813",
      "reasoning_effort": "high",
      "messages": [
        {
          "role": "user",
          "content": "Explain the difference between CSA and HCA attention in one paragraph."
        }
      ]
    }'
```
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
        {
            "role": "user",
            "content": "Review this function for race conditions and propose a fix:\n\n<paste code here>",
        }
    ],
    max_tokens=131072,  # unset defaults to 65,536; the ceiling is 393,216
    top_p=0.95,         # DeepSeek's recommendation for agentic use
    extra_body={
        "reasoning_effort": "max",   # low | high | max ("none" disables thinking)
        "service_tier": "flex",      # 0.8x price; drop this line for standard
    },
)
print(response.choices[0].message.content)
```
**JavaScript**

```
import OpenAI from "openai";
const openai = new OpenAI({
  apiKey: process.env.DEEPINFRA_API_KEY,
  baseURL: "https://api.deepinfra.com/v1/openai",
});
const response = await openai.chat.completions.create({
  model: "deepseek-ai/DeepSeek-V4-Pro-0813",
  reasoning_effort: "high",
  messages: [{ role: "user", content: "Explain the difference between CSA and HCA attention in one paragraph." }],
});
console.log(response.choices[0].message.content);
```
To try prompts before writing code, open the interactive demo on the DeepSeek-V4-Pro-0813 model page. The API reference lists every supported parameter, including `reasoning_effort`, `service_tier` and prompt-caching options. For tool use, see function calling on DeepInfra.

DeepSeek V4 Pro 0813 is a large step up from the preview where it counts for agent builders: repository-scale coding, terminal and tool-use loops, and long automations. Teams running coding agents, security tooling or multi-step workflows on open weights should evaluate it, ideally on their own tasks, since the benchmark gains are vendor-reported. The 1M context, 384K output ceiling and three-level `reasoning_effort` give you room to trade depth for cost per request.

It isn’t the default answer for every workload. On Artificial Analysis’s general index it sits at its class median, and DeepSeek’s newer DeepSeek-V4.1-Flash scores higher there at a fraction of the per-token price. Route classification, extraction and other easy steps to V4.1 Flash, and keep V4 Pro 0813 for the agentic coding work where its results are strongest. Mixture of Experts economics explains why per-token pricing on models this size has dropped so far.

For deeper numbers, see the DeepSeek V4 Pro pricing guide, the provider speed and latency benchmarks, the model overview and our list of the best API providers for DeepSeek V4.

Building a startup? DeepStart gives eligible startups about 50 billion free tokens (counted at DeepSeek V4 Flash prices) to spend on any model, including V4 Pro 0813. Or head to the model page, grab an API key and make your first call.
