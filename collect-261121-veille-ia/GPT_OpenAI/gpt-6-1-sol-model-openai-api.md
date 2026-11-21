---
id: collect-261121-veille-ia/gpt-openai/gpt-6-1-sol-model-openai-api
title: "GPT-6.1 Sol Model | OpenAI API"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["gpt-6", "input", "prices", "standard", "sol", "model", "openai", "api"]
source: https://developers.openai.com/api/docs/models/gpt-6.1-sol
source_anchor: ""
source_lines: [1, 28]
section: "GPT_OpenAI"
---
`gpt-6.1-sol`
Use GPT-6.1 Sol for complex coding, computer use, and professional work.

`reasoning.effort` supports `low`, `medium` (default), `high`, `xhigh`, and
`max`.

Tool calling requires the Responses API.

See the GPT-6 guide for prompting and migration guidance.

Cached input tokens are priced at 5% of the uncached input token rate.

Cache writes are billed at 1.25x the uncached input token rate.

API prompts with more than 272K input tokens are priced at 2x input and cache rates and 1.5x output for the full request.

Fast mode prices are 2x Standard. Batch and Flex prices are 50% lower than Standard.

Ultrafast mode prices are 6x Standard.

Regional processing adds a 10% premium where available.

| Tier | RPM | TPM | 
|---|---|---|
| Free | Not supported |  | 
| Build | 5,000 | 1,000,000 | 
| Launch | 10,000 | 4,000,000 | 
| Grow | 15,000 | 40,000,000 |
