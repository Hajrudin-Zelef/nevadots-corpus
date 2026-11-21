---
id: collect-261121-veille-ia/grok-xai/grok-4-7-developer-guide-spacexsi-docs
title: "Grok 4.7 Developer Guide | SpaceXSI Docs"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: ["2026-10-10"]
keywords: ["model", "grok", "reasoning", "responses", "text", "pricing", "spacexsi", "grok-4"]
source: https://docs.x.ai/developers/grok-4-7
source_anchor: ""
source_lines: [1, 49]
section: "Grok_xAI"
---
Grok 4.7 is SpaceXSI's frontier model built for coding, agentic tasks, and knowledge work.

If you already have an API key, set the model name to `grok-4.7`:

```
import { xai } from '@ai-sdk/xai';
import { generateText } from 'ai';
const { text } = await generateText({
  model: xai.responses('grok-4.7'),
  prompt:
    'Find and fix the bug, then explain it: function median(a){a.sort();return a[a.length/2]}',
});
console.log(text);
```
New to the xAI API? Follow the Quickstart to create an account and make your first request.

| Property | Value | 
|---|---|
| Model name | `grok-4.7` | 
| Context window | 500,000 tokens | 
| Knowledge cutoff | May 2026 | 
| Modalities | Text and image input; text output | 
| Output limit | No text output limit | 
| Input price | $2.00 / 1M tokens | 
| Output price | $6.00 / 1M tokens | 
| Reasoning | Low, medium, high (default), or xhigh | 
| APIs | Responses API | 
| Tools | Function calling, web search, X search, code execution | 

Rate limits and live pricing for your team are on the model detail page and Pricing.

- **We highly recommend setting a `prompt_cache_key`** (Responses API;`x-grok-conv-id` header on Chat Completions). It routes a conversation's requests to the same server, making cache hits reliable; without it you often pay full input price on a cache-cold server. See What Breaks Caching for common mistakes.
- **Long agent loops** additionally benefit from context compaction; for tool-heavy workloads see function calling.
- **Encrypted reasoning is always returned on the Responses API.**`POST /v1/responses` responses from`grok-4.7` include`reasoning.encrypted_content` even when`include` does not list it, so multi-turn conversations keep the model's reasoning without extra configuration. Pass the reasoning items back unchanged in the next request's`input` ; see Encrypted reasoning content. Chat Completions is unchanged.

Grok 4.7 Fast is the same model served on faster infrastructure. It costs 2x the standard token rates, or 1.5x for long-context requests. It's available only in Cursor and Grok Build, and is billed through your plan there. It is not included in Grok Build's free tier. It is not available on the public xAI API. Rates are on the Pricing page.

- **xAI API** : get a key from the console
- **US regional endpoint** : also served at`https://us.api.x.ai/v1` , which keeps inference in the United States, with token usage priced at a 10% premium; see Regional Endpoints
- **Grok Build** : the default model of the coding agent
- **Cursor** : available on all plans
- **Model gateways** : OpenRouter, Vercel, and Cloudflare

- Reasoning - controlling `reasoning_effort` , including`"xhigh"`
- Announcement - launch post with demos and full benchmark figures
- Models - compare available models and their capabilities
- Pricing - token pricing for all models

Last updated: October 10, 2026
