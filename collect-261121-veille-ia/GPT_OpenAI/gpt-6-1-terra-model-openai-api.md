---
id: collect-261121-veille-ia/gpt-openai/gpt-6-1-terra-model-openai-api
title: "GPT-6.1 Terra Model | OpenAI API"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["gpt-6", "terra", "input", "tokens", "prices", "retention", "standard", "model"]
source: https://developers.openai.com/api/docs/models/gpt-6.1-terra
source_anchor: ""
source_lines: [1, 30]
section: "GPT_OpenAI"
---
`gpt-6.1-terra`
Use GPT-6.1 Terra to match product photos to catalog entries, interpret technical drawings, act on application screens, or draft and revise text.

`reasoning.effort` supports `none`, `low`, `medium` (default), `high`,
`xhigh`, and `max`.

Tool calling requires the Responses API.

See the GPT-6 guide for prompting and migration guidance.

The Responses API supports discounted background completion windows in public beta.

Cached input tokens are priced at 5% of the uncached input token rate.

Cache-write prices shown use 30-minute retention. Standard cache writes cost $1.25 per million tokens for 30-minute retention and $2.50 for 12-hour retention, before long-context adjustments.

Prompts with more than 272K input tokens are priced at 2x input and cache rates and 1.5x output for the full request.

Batch and Flex prices are 50% lower than Standard.

Regional processing adds a 10% premium where available.

Fast mode prices are 2x Standard.

| Tier | RPM | TPM | Batch queue limit | 
|---|---|---|---|
| Free | Not supported |  |  | 
| Build | 5,000 | 1,000,000 | 3,000,000 | 
| Launch | 10,000 | 4,000,000 | 200,000,000 | 
| Grow | 15,000 | 40,000,000 | 15,000,000,000 |
