---
id: collect-261121-veille-ia/gpt-openai/gpt-6-2-sol-model-openai-api
title: "GPT-6.2 Sol Model | OpenAI API"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["gpt-6", "costs", "standard", "rate", "requests", "cache", "times", "sol"]
source: https://developers.openai.com/api/docs/models/gpt-6.2-sol
source_anchor: ""
source_lines: [1, 20]
section: "GPT_OpenAI"
---
`gpt-6.2-sol`
GPT-6.2 Sol accepts text and image inputs and produces text. Use it with the Responses API to call tools such as web search and computer use, or to delegate work to subagents with the Multi-agent beta.

The API rejects requests that set reasoning effort to `none`.

The Responses API supports discounted background completion windows in public beta.

The cache write price is for 30 minutes of retention. A 12-hour cache write costs $5 per million tokens for short-context requests at Standard pricing.

For requests with more than 272K input tokens, input and cache rates double and output costs 1.5 times the short-context rate for the full request.

Batch costs 50% of Standard rates. Fast mode costs twice the Standard rate for the same context length.

Ultrafast costs 3 times the Standard rate through February 28, 2027, and 6 times the Standard rate starting March 1, 2027 (Pacific time).

| Tier | RPM | TPM | 
|---|---|---|
| Build | 5,000 | 1,000,000 | 
| Launch | 10,000 | 4,000,000 | 
| Grow | 15,000 | 40,000,000 |
