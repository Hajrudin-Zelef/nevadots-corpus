---
id: collect-261121-veille-ia/deepseek/deepseek-v4-1-flash-specs-pricing-benchmarks-and-api
title: "DeepSeek V4.1 Flash: Specs, Pricing, Benchmarks, and API"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: []
keywords: ["deepseek", "flash", "model", "parameters", "pricing", "during", "input", "context"]
source: https://blog.invidelabs.com/deepseek-v4-1-flash/
source_anchor: ""
source_lines: [1, 111]
section: "DeepSeek"
---
DeepSeek V4.1 Flash is a 552-billion-parameter mixture-of-experts model designed to make large agent workloads faster and less expensive. It accepts text and images, supports a one-million-token context window, and is available through the DeepSeek API and as downloadable weights.

Its unusual architecture and lower API pricing make it a strong candidate for coding agents, long documents, and visual tasks. It leads on Terminal-Bench 2.1 and DeepSWE v1.1 but trails Opus 5.0 on Terminal-Bench 3.0 and 4.0, and trails V4 Pro on GPQA Diamond. Independent evidence is still limited because the model was released on September 10, 2026.

## DeepSeek V4.1 Flash specifications

| Specification | DeepSeek V4.1 Flash | 
|---|---|
| Architecture | Multimodal MoE with Causal Encoder-Decoder | 
| Backbone parameters | 552B | 
| Active parameters | 8B during input, 16B during output | 
| Context window | 1 million tokens | 
| Maximum API output | 384K tokens | 
| Input | Text and images | 
| API model name | `deepseek-flash` | 
| Model license | MIT | 

The parameter count needs some explanation. DeepSeek describes V4.1 Flash as having a 552B-parameter backbone, while its technical report separately describes 196B parameters used by its Engram memory component. Hugging Face currently displays 763B parameters. DeepSeek does not reconcile the three figures. The clearest summary for users is the official 552B backbone, with 8B parameters active while processing input and 16B during generation.

## What makes DeepSeek V4.1 Flash different?

V4.1 Flash uses what DeepSeek calls a Causal Encoder-Decoder architecture. Its 40 Transformer layers are divided into a 20-layer encoder and a 20-layer decoder. This makes prompt processing and output generation asymmetric: the model activates 8B parameters per input token, then 16B per generated token.

A coding agent may repeatedly process a large repository, tool results, and a growing conversation before producing a relatively short response. Activating fewer parameters for those input tokens reduces the compute required during prompt processing.

DeepSeek has also compressed the model's KV cache, which stores information needed to reuse earlier context during generation. The model card puts the global KV cache at 890 bytes per token, and DeepSeek's release notes say V4.1 Flash needs one-quarter the HBM and one-eighth the SSD storage of V4 Flash. These savings help explain the lower API price, particularly when an agent keeps returning to cached context.

The other visible change is native multimodal support. V4.1 Flash can process images alongside text, rather than relying on a separate experimental vision model. DeepSeek's API accepts JPEG, PNG, GIF, and WebP inputs, making screenshots, charts, scanned pages, and visual documents part of the same model workflow.

## DeepSeek V4.1 Flash API pricing and model name

The canonical API model name is `deepseek-flash`, not `deepseek-v4.1-flash`. DeepSeek continues to accept the older `deepseek-v4-flash` and `deepseek-v4-flash-vision-exp` identifiers temporarily, but both retired models now route to V4.1 Flash.

The API uses the OpenAI-compatible base URL `https://api.deepseek.com`. An Anthropic-compatible endpoint is also available at `https://api.deepseek.com/anthropic`. A minimal OpenAI-format request only needs the current model name:

```
from openai import OpenAI
client = OpenAI(
    api_key="YOUR_DEEPSEEK_API_KEY",
    base_url="https://api.deepseek.com",
)
response = client.chat.completions.create(
    model="deepseek-flash",
    messages=[{"role": "user", "content": "Explain this code."}],
)
```
Thinking mode is enabled by default at `high` reasoning effort in the API, so this request does not reproduce DeepSeek's benchmark configuration. The open weights accept a numeric reasoning effort from 1 to 100, and the reported instruct-model results use 100. The hosted API instead exposes `low`, `high`, and `max` settings. In thinking mode, the API ignores `temperature`; `top_p` still applies, with a minimum value of 0.95.

As checked on September 15, 2026, DeepSeek's pricing page lists these rates per million tokens:

| Token type | Off-peak | Peak | 
|---|---|---|
| Input, cache hit | $0.003 | $0.006 | 
| Input, cache miss | $0.15 | $0.30 | 
| Output | $0.60 | $1.20 | 

Peak hours are 01:00–04:00 and 06:00–10:00 UTC, Monday through Friday. All other hours use the off-peak rate. The pricing is attractive for input-heavy applications, but cached and uncached input are billed very differently. Actual cost will depend on whether an application can reuse context effectively.

## What do the V4.1 Flash benchmarks show?

DeepSeek reports a 90.6 score on Terminal-Bench 2.1 and 74.2 percent resolved on DeepSWE v1.1, both aimed at agentic coding work. The model does not lead as clearly on newer Terminal-Bench versions: it scores 30.0 on 3.0 and 31.2 on 4.0, compared with 43.3 and 51.8 for Opus 5.0 in the same table. Its 90.9 GPQA Diamond score also trails V4 Pro's 92.4.

According to the official model card, these instruct-model evaluations used maximum reasoning effort and specific scaffolds. DeepSeek Harness Minimal produced the 90.6 Terminal-Bench 2.1 result, while mini-SWE-agent produced the 74.2 DeepSWE result. Under the Claude Code scaffold, the same model scored 88.0 and 69.8 respectively. Readers should not expect one headline score to transfer unchanged between agent tools.

DeepSeek also reports 95.6 on DocVQA, but that is a separate four-shot, LLM-judged base-model evaluation rather than one of the maximum-reasoning instruct results. Overall, V4.1 Flash clearly improves on V4 Pro in several agentic coding tests and costs less. It does not surpass V4 Pro across every knowledge and reasoning measure: V4 Pro also leads the base-model SimpleQA-Verified result, 55.2 to 42.3.

## DeepSeek V4.1 Flash vs V4 Flash and V4 Pro

V4.1 Flash directly replaces the older V4 Flash and V4 Flash Vision Experimental models on the API. Compared with V4 Flash, it has a larger backbone, a different architecture, native image support, a smaller KV cache, and stronger vendor-reported agent results.

DeepSeek initially announced that requests for `deepseek-v4-pro` would be routed to V4.1 Flash after September 14. The company then changed that plan in response to user demand. Its current API changelog and pricing page continue to list DeepSeek V4 Pro separately, with its existing model name and pricing.

V4 Pro costs $0.022 for cached input, $0.66 for uncached input, and $1.98 for output per million tokens off-peak, versus $0.003, $0.15, and $0.60 for Flash. V4 Pro also lacks image input. V4.1 Flash is therefore the stronger first test for cost-sensitive or visual applications.

## Are the V4.1 Flash weights available to download?

DeepSeek has published the V4.1 Flash weights on Hugging Face under the MIT license. Hugging Face's generated interface offers starting points for Transformers, vLLM, SGLang, and Docker Model Runner, alongside more than 60 community quantizations.

Self-hosters face a less obvious integration problem too: the release does not include a Jinja chat template. DeepSeek instead supplies a reference `encoding.py` implementation for its prompt format and recommends the separate `deepseek-recipe` Rust library with Python bindings for production. Hugging Face's generated vLLM and SGLang examples send a plain string to `/v1/completions`; they demonstrate raw completion, not DeepSeek's chat format for thinking, tools, images, and multi-turn messages.

The complete model also requires substantial memory and storage despite activating only 8B or 16B parameters per token. The release documentation invites organisations planning large-scale deployments with 2,000 GPUs and a storage cluster to contact DeepSeek, while the official model card does not provide a consumer-hardware requirement.

## Who should use DeepSeek V4.1 Flash?

V4.1 Flash is most relevant to developers building coding agents, long-running tool workflows, document-analysis systems, and applications that send much more context than they generate. Native image input also makes it a candidate for screenshot analysis and visual document processing without a separate vision endpoint.

Teams should start with the hosted API, test quality on their own tasks, measure cache-hit rates, and compare total completion time rather than selecting the model from a single benchmark score. Self-hosting makes sense when control over weights or infrastructure justifies the hardware and integration cost.

## DeepSeek V4.1 Flash FAQ

### What is the DeepSeek V4.1 Flash API model name?

Use `deepseek-flash`. The temporary legacy identifiers `deepseek-v4-flash` and `deepseek-v4-flash-vision-exp` also route to V4.1 Flash, but new integrations should use the canonical name.

### How much does DeepSeek V4.1 Flash cost?

Off-peak pricing starts at $0.003 per million cached input tokens, $0.15 per million uncached input tokens, and $0.60 per million output tokens. Peak rates are twice those amounts. Prices may change, so check the official pricing page before deployment.

### Does DeepSeek V4.1 Flash support images?

Yes. It accepts text and images natively through the API, including JPEG, PNG, GIF, and WebP files.

### Is DeepSeek V4.1 Flash open source?

Its model weights and repository are published under the MIT license. The hosted API and DeepSeek applications remain separate services, so “open weights” is the more precise description when discussing the downloadable model.

### Can DeepSeek V4.1 Flash run locally?

It can be self-hosted with supported inference software and enough hardware, but it is not comparable to running a normal 8B or 16B model. Those numbers describe active parameters, not the full set of weights that must be stored and served.

## Member discussion
