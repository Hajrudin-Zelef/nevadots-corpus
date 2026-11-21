---
id: collect-261121-veille-ia/kimi/kimi-k3-1-pro-model-overview-benchmarks-api-guide
title: "Kimi K3.1 Pro: Model Overview, Benchmarks & API Guide"
domain: kimi
role: reference
task: article
actors: ["Moonshot AI"]
dates: []
keywords: ["kimi", "model", "moonshot", "license", "benchmarks", "parameters", "active", "token"]
source: https://deepinfra.com/blog/kimi-k3-1-pro-model-overview
source_anchor: ""
source_lines: [1, 151]
section: "Kimi"
---
Kimi K3.1 Pro is Moonshot AI’s flagship open-weight model, released on October 23, 2026 and built for long-horizon agent work: coding, 3D creation, financial research and document-heavy tasks. It is a 2.8-trillion-parameter Mixture-of-Experts model with 104B parameters active per token, takes text and images as input, returns text, and has a 1,048,576-token context window.

K3.1 Pro is not a new pre-training run. Moonshot post-trained it from Kimi K3 “with substantially larger post-training compute” and large-scale reinforcement learning across hundreds of domains, and swapped K3’s full-attention layers for sparse attention to make long contexts cheaper to serve. It is the first model in the K3.1 series.

On DeepInfra the model runs behind an OpenAI-compatible API with zero data retention, JSON and structured output, function calling and image input. If you need a cheaper model for simpler agent steps, DeepSeek V4.1 Flash-1017 is on the same API. The model string is:

`moonshotai/Kimi-K3.1-Pro`

Moonshot’s claims below are vendor-reported unless marked otherwise.

| Component | Kimi K3.1 Pro | 
|---|---|
| Architecture | Mixture-of-Experts | 
| Total / active parameters | 2.8T / 104B | 
| Layers | 93 (1 dense) | 
| Attention | 69 KDA + 24 Gated DSA layers | 
| Experts | 896 routed, 16 selected per token, 2 shared | 
| Hidden dimension / attention heads | 7,168 / 96 | 
| Vocabulary | 160K | 
| Activation | SiTU-GLU | 
| Vision encoder | MoonViT-V2 (401M parameters) | 
| Released weights | MXFP4 weights / MXFP8 activations (quantization-aware training) | 
| Context window | 1,048,576 tokens | 
| License | Kimi K3.1-Pro License (modified MIT) | 

*Source: Hugging Face model card.*

The license is MIT-style with two conditions. Companies running a model-as-a-service business with more than $20 million in group revenue over any 12 months need a separate agreement with Moonshot before commercial use. Products with more than 100 million monthly active users or $20 million in monthly revenue must display “Kimi K3.1-Pro” in their interface. Internal use is exempt from both. If your product could cross either threshold, read the license text before you ship.

Moonshot ran K3.1 Pro at reasoning effort `max`, temperature 1.0 and top_p 0.95, mostly inside the Claude Code or Kimi Code harnesses. Several comparison scores are cited from leaderboards or other labs’ reports, closed models ran with fallback, and rows marked “internal” use Moonshot’s own benchmarks. Treat everything in this table as vendor-reported. The rows below are a selection; the model card has all 47.

| Benchmark | Kimi K3.1 Pro | Claude Opus 5.5 | GPT-6 Astra | Claude Fable 5.1 | DeepSeek V4.1 Flash-1017 | GLM-5.3 | 
|---|---|---|---|---|---|---|
| Terminal-Bench 4.0 | 65.7 | **66.4** | 58.2 | 57.9 | 42.3 | 41.8 | 
| SWE Marathon v1.1 | **51.7** | 46.3 | 42.5 | 45.6 | 40.0 | 42.5 | 
| DeepSWE v1.1 | 75.5 | 74.2 | 74.1 | 67.4 | **77.0** | 66.9 | 
| ProgramBench | 31.5 | **65.0** | 50.0 | 53.5 | 29.1 | 19.0 | 
| HLE-Full (no tools) | 51.1 | **59.8** | 53.6 | 57.6 | 42.6 | 43.0* | 
| HLE-Full w/ tools | **70.0** | 67.7 | 57.2 | 65.6 | 58.8 | 62.5 | 
| GPQA-Diamond | 94.2 | 93.9 | **96.1** | 93.7 | 92.2 | 91.7 | 
| BrowseComp | **92.1** | 90.8 | 91.5 | — | — | — | 
| Agents’ Last Exam (all) | **43.2** | 42.6 | 41.8 | — | 37.7 | 26.2 | 
| AutomationBench-AA | **73.3** | 69.5 | 68.5 | 59.4 | 66.6 | 62.2 | 
| GDPval-AA v2.1 (Elo) | 1846 | **1866** | 1573 | 1754 | 1746 | 1650 | 
| APEX-Agents | 69.4 | **73.5** | 64.7 | 68.6 | 69.8 | 56.6 | 
| OfficeQA Pro v2 | **64.4** | 62.2 | 54.4 | 54.5 | 40.4 | 31.1 | 
| OSWorld V2.1 | 79.6 | **81.8** | 75.8 | 80.7 | 69.3 | — | 
| MMMU-Pro w/ python | **90.0** | 89.8 | 87.8 | 88.4 | 82.5 | — | 
| ExploitGym | 20.7 | 34.5 | **42.4** | 30.4 | — | 15.0 | 
| Kimi Hallu Bench Hard w/ tools (internal, lower is better) | 8.3 | 15.0 | **5.5** | 23.3 | 23.3 | 22.6 | 

*Source: Kimi K3.1 Pro model card. Bold marks the best score in each row. * GLM-5.3’s HLE score is on the text-only subset.*

**Who leads which row.** K3.1 Pro has the top score on agentic and tool-using work: SWE Marathon, HLE with tools, BrowseComp, Agents’ Last Exam, AutomationBench, OfficeQA Pro v2 and MMMU-Pro with Python. Claude Opus 5.5 leads Terminal-Bench 4.0 by 0.7 points, plus ProgramBench, HLE without tools, GDPval-AA, APEX-Agents and OSWorld.

**Where it loses.** The widest gaps are on ProgramBench (31.5 against Opus 5.5’s 65.0), HLE-Diamond without tools (39.4 against GPT-6 Astra’s 65.8) and the cyber rows, where Moonshot capped capability on purpose (ExploitGym 20.7 against 42.4). Among open-weight models in the table, DeepSeek V4.1 Flash-1017 beats it on DeepSWE (77.0 against 75.5) and APEX-Agents (69.8 against 69.4) at a small fraction of the price.

**Independent view.** Artificial Analysis scores K3.1 Pro at **46 on its Intelligence Index v5.0.1**, first of 29 open-weight models of similar size (class median 23). The same page calls it “particularly expensive when comparing to other open weight models of similar size” and “somewhat verbose”: 113M output tokens to run the index against a 104M median. You can compare it against other models on DeepInfra on the model comparison page.

Pricing as of 2026-11-17, per 1M tokens (model page, pricing page):

| Tier | Input | Output | Cached input | 
|---|---|---|---|
| Standard, promotional (5% off) | **$1.90** | **$9.50** | **$0.19** | 
| Standard, list | $2.00 | $10.00 | $0.20 | 
| Flex (0.8×) | $1.52 | $7.60 | $0.152 | 
| Priority (1.5×) | $2.85 | $14.25 | $0.285 | 

**How the discounts stack.** The 5% promotion comes off list, and Flex takes a further 20% off the promotional rate: $1.90 × 0.8 = $1.52 input, $9.50 × 0.8 = $7.60 output. Priority applies 1.5× to the promotional rate. Cached input is 10% of the input rate in every tier. No end date has been published for the promotion.

**Against other providers.** On Artificial Analysis’s provider benchmarks, DeepInfra’s $1.90 / $9.50 is the second-lowest input, output and blended price of the providers it measures, and the lowest of those that returned no errors: DeepInfra’s measured error rate is 0.0%, while the one cheaper endpoint (PrimaLabs, $1.80 / $9.00) failed 15.4% of requests. Most providers charge Moonshot’s own $2.00 / $10.00, and high-speed tiers run $4.00–$6.00 input and $20–$30 output. If response speed is your product, test DigitalOcean or Baseten, which measure around 200 and 183 tokens per second at the standard $2.00 / $10.00 rate against DeepInfra’s 47; for batch, agents and cost-sensitive work, the per-token gap compounds over long outputs.

**Flex** is the cheapest way to run the model if your work can wait. Send `"service_tier": "flex"`: when capacity is short, the request waits up to 10 minutes and then returns HTTP 429, and a rejected Flex request is not charged. Overnight evaluations, batch extraction and background agents are a good fit. The Flex service tier announcement covers the details.

**Priority** (`"service_tier": "priority"`) is for traffic that should not queue. At $2.85 / $14.25 it costs more than Moonshot’s list price, so use it only where queueing costs you more than the premium.

**Caching** matters with a verbose model and long agent sessions: cached input costs $0.19 against $1.90 for fresh input. Use `prompt_cache_key` to route repeated prefixes to the same cache, and `prompt_cache_options` to keep it warm for longer (see prompt cache retention).

For worked monthly cost scenarios, see the Kimi K3.1 Pro pricing guide. Startups can apply for free credits through DeepStart.

The API is OpenAI-compatible:

`https://api.deepinfra.com/v1/openai``POST /chat/completions``Authorization: Bearer $DEEPINFRA_API_KEY`
The full request schema is on the model’s API page.

Model-specific parameters first. Send non-standard fields through `extra_body` in the OpenAI Python SDK.

| Parameter | Default | Notes | 
|---|---|---|
| `reasoning_effort` | model default | `none` ,`minimal` ,`low` ,`medium` ,`high` ,`xhigh` ,`max` .`"none"` turns thinking off. Moonshot’s benchmarks use`max` . Reasoning tokens bill as output | 
| `reasoning` | — | Object form: `{"effort": "...", "enabled": true/false}` | 
| `temperature` | 1 | Moonshot’s evaluations use 1.0 | 
| `top_p` | 1 | Moonshot’s evaluations use 0.95 | 
| `service_tier` | standard | `flex` (0.8×, may queue up to 10 min, then 429) or`priority` (1.5×) | 
| `fail_fast` | false | Return 429 immediately instead of queueing | 
| `models` | — | Up to 4 fallback models, tried in order if this one has no capacity | 
| `prompt_cache_key` ,`prompt_cache_options` | — | Share a prompt cache across requests; e.g. `{"ttl": "1h"}` | 
| `model` | (required) | `moonshotai/Kimi-K3.1-Pro` | 
| `messages` | (required) | Chat messages; user content can include images | 
| `max_tokens` | min(context − input, 65,536) | Up to 943,718 | 
| `stream` | false | Server-sent events | 
| `stop` | — | Up to 16 sequences | 
| `n` | 1 | Up to 4 | 
| `top_k` ,`min_p` | 0 | Optional sampling controls | 
| `presence_penalty` ,`frequency_penalty` | 0 | −2 to 2 | 
| `response_format` | text | `json_object` ,`json_schema` or`regex` | 
| `tools` ,`tool_choice` | — | OpenAI-style function calling | 

```
import os
from openai import OpenAI
client = OpenAI(
    api_key=os.environ["DEEPINFRA_API_KEY"],
    base_url="https://api.deepinfra.com/v1/openai",
)
response = client.chat.completions.create(
    model="moonshotai/Kimi-K3.1-Pro",
    messages=[
        {"role": "system", "content": "You are a senior equity research analyst."},
        {
            "role": "user",
            "content": [
                {"type": "text", "text": "Summarize the revenue trend in this chart and list three risks."},
                {"type": "image_url", "image_url": {"url": "https://example.com/revenue-chart.png"}},
            ],
        },
    ],
    temperature=1.0,
    top_p=0.95,
    max_tokens=32768,
    extra_body={
        "reasoning_effort": "high",  # none | minimal | low | medium | high | xhigh | max
        "service_tier": "flex",      # remove this line for the standard tier
    },
)
print(response.choices[0].message.content)
print(response.usage)
```
For short, well-defined steps such as classification, extraction or routing, start with `"reasoning_effort": "low"` or `"none"`. K3.1 Pro is verbose at high effort, and reasoning tokens are billed as output at $9.50 per 1M on the standard tier.

There is no 16,384-token output cap on this model. You can request up to **943,718 output tokens**. If you leave `max_tokens` unset, DeepInfra uses the smaller of 65,536 tokens and whatever remains of the 1,048,576-token window after your input. Input and output together must fit in that window.

Moonshot’s published results use `max` effort, and long agent runs at that setting can spend tens of thousands of tokens reasoning before the final answer starts. To get closer to Moonshot’s setup, set `max_tokens` explicitly above the 65,536 default, use `max` effort, and stream the response so long generations don’t hit client timeouts. Exact reproduction isn’t guaranteed, because Moonshot ran most benchmarks inside its own harnesses.

Kimi K3.1 Pro has the highest Artificial Analysis Intelligence Index score among the 29 open-weight models of its size, and Moonshot’s own table puts it at or near the top on agentic, tool-using and document work. Its weak spots are clear too: closed models still lead on knowledge without tools, ProgramBench and cyber tasks, and it is verbose, so output tokens drive most of the bill.

That makes price per output token the number to watch. On DeepInfra, K3.1 Pro costs $9.50 per 1M output tokens at the current promotion, or $7.60 on Flex. That is below Moonshot’s list price and below the model it replaces, with a measured 0% error rate.

**Next steps**

`reasoning_effort` set per step.
