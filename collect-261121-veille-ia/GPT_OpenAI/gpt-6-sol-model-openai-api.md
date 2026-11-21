---
id: collect-261121-veille-ia/gpt-openai/gpt-6-sol-model-openai-api
title: "GPT-6 Sol Model | OpenAI API"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["gpt-6", "input", "model", "priced", "data", "residency", "available", "batch"]
source: https://developers.openai.com/api/docs/models/gpt-6-sol
source_anchor: ""
source_lines: [1, 24]
section: "GPT_OpenAI"
---
`gpt-6-sol`
GPT-6 Sol is built for complex coding and agentic workflows. See GPT-6.1 Sol for the newer Sol model.

`reasoning.effort` supports `none`, `low`, `medium` (default), `high`, `xhigh`, and `max`.
Use the Responses API for built-in tools and function calling. Chat Completions supports function calling only with `reasoning_effort` set to `none`.

EU data residency is available with Standard, Flex, and Batch processing. EU data residency is not available with Fast processing. See data residency eligibility.

Cached input tokens are priced at 10% of the uncached input token rate.

Cache writes are billed at 1.25x the uncached input token rate.

Prompts with more than 272K input tokens are priced at 2x input and cache rates and 1.5x output for the full request.

Regional processing adds a 10% premium where available.

Batch and Flex are priced at 50% of Standard rates. Fast mode is priced at 2x the applicable rates.

| Tier | RPM | TPM | Batch queue limit | 
|---|---|---|---|
| Free | Not supported |  |  | 
| Build | 5,000 | 1,000,000 | 3,000,000 | 
| Launch | 10,000 | 4,000,000 | 200,000,000 | 
| Grow | 15,000 | 40,000,000 | 15,000,000,000 |
