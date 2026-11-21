---
id: collect-261121-veille-ia/gpt-openai/gpt-5-6-sol-model-openai-api
title: "GPT-5.6 Sol Model | OpenAI API"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["gpt-5", "model", "input", "tokens", "output", "pricing", "sol", "openai"]
source: https://developers.openai.com/api/docs/models/gpt-5.6-sol
source_anchor: ""
source_lines: [1, 18]
section: "GPT_OpenAI"
---
`gpt-5.6-sol`
GPT-5.6 Sol is a flagship model in the GPT-5.6 family. It roughly
corresponds to the unsuffixed model tier used in earlier GPT-5 families.
The `gpt-5.6` alias routes requests to GPT-5.6 Sol.
Reasoning.effort supports: none, low, medium (default), high, xhigh, and max.

GPT-5.6 Sol costs $4 per million input tokens and $20 per million output tokens, a 20% reduction in input pricing and a 33% reduction in output pricing. GPT-5.6 Solâs promotional pricing is available until November 30, 2026.

Prompts with >272K input tokens are priced at 2x input and 1.5x output for the full request.

Cache writes are billed at 1.25x the uncached input token rate.

| Tier | RPM | TPM | Batch queue limit | 
|---|---|---|---|
| Free | Not supported |  |  | 
| Build | 5,000 | 1,000,000 | 3,000,000 | 
| Launch | 10,000 | 4,000,000 | 200,000,000 | 
| Grow | 15,000 | 40,000,000 | 15,000,000,000 |
