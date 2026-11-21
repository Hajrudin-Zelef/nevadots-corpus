---
id: collect-261121-veille-ia/gpt-openai/gpt-6-luna-model-openai-api
title: "GPT-6 Luna Model | OpenAI API"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["input", "gpt-6", "luna", "model", "batch", "priced", "supports", "data"]
source: https://developers.openai.com/api/docs/models/gpt-6-luna?ref=hackernoon.com
source_anchor: ""
source_lines: [1, 26]
section: "GPT_OpenAI"
---
`gpt-6-luna`
GPT-6 Luna is our most efficient model for focused, high-volume tasks.

`reasoning.effort` supports `none`, `low`, `medium` (default), `high`, `xhigh`, and `max`.
Use the Responses API for built-in tools and function calling. Chat Completions supports function calling only with `reasoning_effort` set to `none`.

EU data residency is available with Standard, Fast, Flex, and Batch processing. See data residency eligibility.

The Responses API supports discounted background completion windows in public beta.

Cached input tokens are priced at 10% of the uncached input token rate.

Cache writes are billed at 1.25x the uncached input token rate.

Prompts with more than 272K input tokens are priced at 2x input and cache rates and 1.5x output for the full request.

Regional processing adds a 10% premium where available. EU data residency is available with Standard, Fast, Flex, and Batch processing.

Batch and Flex are priced at 50% of Standard rates. Fast mode is priced at 2x the applicable rates.

| Tier | RPM | TPM | Batch queue limit | 
|---|---|---|---|
| Free | Not supported |  |  | 
| Build | 5,000 | 2,000,000 | 20,000,000 | 
| Launch | 10,000 | 10,000,000 | 1,000,000,000 | 
| Grow | 30,000 | 180,000,000 | 15,000,000,000 |
