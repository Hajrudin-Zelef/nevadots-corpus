---
id: collect-261121-veille-ia/grok-xai/grok-4-6-vs-grok-4-7-price-context-window-and-features-litellm
title: "Grok 4.6 vs Grok 4.7: Price, Context Window and Features | LiteLLM"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: []
keywords: ["grok", "tokens", "xhigh", "litellm", "model", "context", "window", "messages"]
source: https://models.litellm.ai/compare/grok-4.6-vs-grok-4.7
source_anchor: ""
source_lines: [1, 75]
section: "Grok_xAI"
---
# Grok 4.6 vs Grok 4.7

As of November 20, 2026, Grok 4.6 and Grok 4.7 cost the same: $2.00 per 1M input tokens and $6.00 per 1M output tokens, and both have a 500K-token context window.

## Grok 4.6 vs Grok 4.7 side by side

| Headline prices in US dollars per 1M tokens: each model's own lab where it sells it, otherwise its cheapest provider. Source: LiteLLM model price file, as of November 20, 2026. |  |  | 
|---|---|---|
|  | Grok 4.6 | Grok 4.7 | 
|---|---|---|
| Input, per 1M tokens | $2.00 | $2.00 | 
| Output, per 1M tokens | $6.00 | $6.00 | 
| Cache read, per 1M tokens | $0.50 | $0.50 | 
| Cache write, per 1M tokens | – | – | 
| Context window | 500,000 tokens | 500,000 tokens | 
| Max output | 500,000 tokens | 500,000 tokens | 
| Image input | Yes | Yes | 
| Tool calling | Yes | Yes | 
| Structured output (JSON) | Yes | Yes | 
| Prompt caching | Yes | Yes | 
| Reasoning | Yes | Yes | 
| Providers on LiteLLM | 10 | 10 | 
| Price shown is from | xAI | xAI | 
| Added to LiteLLM | August 13, 2026 | September 21, 2026 | 

## Cheapest provider for each

## Benchmark scores

| From the Epoch AI Benchmarking Hub (CC BY 4.0), data as of September 22, 2026. Some results were run by Epoch and others collected by Epoch from each benchmark's own leaderboard, linked in the first column. Each model's best reported reasoning setting is shown in brackets. Higher is better. Only benchmarks with a score for both models are shown. |  |  | 
|---|---|---|
| Benchmark | Grok 4.6 | Grok 4.7 | 
|---|---|---|
| Overall |  |  | 
| Epoch Capabilities Index | 157.8 | 156.4 | 
| Coding and agents |  |  | 
| FrontierCode | 48.0% | 47.6% | 
| FrontierSWE | 25.3% (xhigh) | 29.5% (xhigh) | 
| APEX-Agents | 65.3% | 70.0% | 
| Reasoning and knowledge |  |  | 
| GPQA Diamond | 94.0% (high) | 92.7% (xhigh) | 
| ARC-AGI-2 | 67.1% (xhigh) | 61.4% (xhigh) | 
| SimpleQA Verified | 49.3% (high) | 56.0% (xhigh) | 
| Math |  |  | 
| FrontierMath (tiers 1-3) | 66.0% (xhigh) | 53.0% (xhigh) | 
| FrontierMath (tier 4) | 31.7% (xhigh) | 17.1% (xhigh) | 
| OTIS Mock AIME 2024-2025 | 99.2% (xhigh) | 98.1% (xhigh) | 

## Call Grok 4.6 or Grok 4.7 with LiteLLM

Both use the OpenAI format through LiteLLM, so switching is a model name change. Set each provider's API key in the environment first.

```
from litellm import completion
messages = [{"role": "user", "content": "Hello!"}]
# Grok 4.6
response = completion(model="xai/grok-4.6", messages=messages)
# Grok 4.7
response = completion(model="xai/grok-4.7", messages=messages)
# Grok 4.6, falling back to Grok 4.7 if the call fails
response = completion(model="xai/grok-4.6", messages=messages, fallbacks=["xai/grok-4.7"])
```
## FAQ

### Which is cheaper, Grok 4.6 or Grok 4.7?

They cost the same: $2.00 per 1M input tokens and $6.00 per 1M output tokens, as of November 20, 2026.

### Which has the larger context window, Grok 4.6 or Grok 4.7?

Both have a 500,000-token context window.

### Can I switch from Grok 4.6 to Grok 4.7 without changing my code?

Yes. Through LiteLLM both use the OpenAI format, so switching means changing the model name from "xai/grok-4.6" to "xai/grok-4.7", or pointing a LiteLLM gateway model alias at the other model. You can also use one as a fallback for the other.
