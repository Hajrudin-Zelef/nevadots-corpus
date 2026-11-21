---
id: collect-261121-veille-ia/gpt-openai/compare-gpt-6-1-terra-vs-terra-pro-openrouter-price-and-context-a-wbroom
title: "Compare GPT-6.1 Terra vs Terra Pro OpenRouter price and context Â· wbroom"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: ["2026-10-30"]
keywords: ["gpt-6", "terra", "context", "tokens", "openai", "price", "pricing", "openrouter"]
source: http://wbroom.com/en/blog/gpt-6-1-terra-vs-terra-pro-openrouter-price-context/
source_anchor: ""
source_lines: [1, 82]
section: "GPT_OpenAI"
---
# Compare GPT-6.1 Terra vs Terra Pro OpenRouter price and context

Both OpenRouter rows use context 1050000 and $1.00 in / $5.00 out per million tokens. The list does not publish a quality difference. Hugging Face has no official Terra weights.

On 2026-10-30 this note reads two public endpoints: the OpenRouter model list, and a Hugging Face search for GPT-6.1. It records list price, context, and whether OpenAI published official weights. It does not record answer quality or speed.

The pair to check is compare GPT-6.1 Terra vs Terra Pro at `openai/gpt-6.1-terra` and `openai/gpt-6.1-terra-pro`. Rows with a `:batch` suffix, and the alias `~openai/gpt-terra-latest`, are not in this table.

## Numbers on the list, 2026-10-30

OpenRouter `pricing.prompt` and `pricing.completion` are USD per token. The âper 1,000,000 tokensâ rows are that field times 1000000. Both records use context 1050000 and a base price of $1.00 in and $5.00 out. The price and the context match, so this API does not publish a quality difference. `reasoning.mandatory` is false on one row and true on the other. That flag is a switch, not a quality score.

| Field | GPT-6.1 Terra | GPT-6.1 Terra Pro | 
|---|---|---|
| OpenRouter id | `openai/gpt-6.1-terra` | `openai/gpt-6.1-terra-pro` | 
| canonical_slug | `openai/gpt-6.1-terra-20261026` | `openai/gpt-6.1-terra-pro-20261026` | 
| context_length | 1050000 | 1050000 | 
| top_provider.context_length | 1050000 | 1050000 | 
| top_provider.max_completion_tokens | 128000 | 128000 | 
| pricing.prompt (USD/token) | `0.000001` | `0.000001` | 
| pricing.completion (USD/token) | `0.000005` | `0.000005` | 
| Input, per 1,000,000 tokens | **$1.00** | **$1.00** | 
| Output, per 1,000,000 tokens | **$5.00** | **$5.00** | 
| input_cache_read, per 1,000,000 tokens | **$0.05** | **$0.05** | 
| input_cache_write, per 1,000,000 tokens | **$1.25** | **$1.25** | 
| Prompt â¥ 272000 tokens, in / out | **$2.00 / $7.50** | **$2.00 / $7.50** | 
| Prompt â¥ 272000, cache read / write | **$0.10 / $2.50** | **$0.10 / $2.50** | 
| created (UTC) | 2026-10-26 18:13:29 UTC unix 1793038409 | 2026-10-26 18:15:56 UTC unix 1793038556 | 
| reasoning.mandatory | false | true | 
| default_effort | medium | medium | 
| hugging_face_id | empty | empty | 

## How to read the same fields again

1. 1### Pull the model listRequest https://openrouter.ai/api/v1/models and find these two ids. Do not substitute a `:batch` row or`~openai/gpt-terra-latest` .
2. 2### Multiply the per-token price by one millionRead pricing.prompt and pricing.completion. Multiply by 1000000 for USD per 1,000,000 tokens. Scale cache fields, and the overrides where min_prompt_tokens is 272000, the same way.
3. 3### Search the HubRequest https://huggingface.co/api/models?search=GPT-6.1 . On 2026-10-30 there was no official OpenAI GPT-6.1 Terra repo. hugging_face_id is empty on both rows.

```
USD per token Ã 1000000 = USD per 1,000,000 tokens
openai/gpt-6.1-terra
  pricing.prompt      0.000001  â $1.00
  pricing.completion  0.000005  â $5.00
openai/gpt-6.1-terra-pro
  pricing.prompt      0.000001  â $1.00
  pricing.completion  0.000005  â $5.00
```
## A matching list price is not a quality test

### The base price matches

0.000001 and 0.000005 USD per token become $1.00 in and $5.00 out per 1,000,000 tokens. Cache read is $0.05 and cache write is $1.25 on both rows.

### The long-prompt override matches

overrides sets min_prompt_tokens to 272000. Above that, input is $2.00 and output is $7.50; cache read is $0.10 and cache write is $2.50. Both rows match, so there is still no separate quality price.

### Context is one number

context_length and top_provider.context_length are both 1050000. top_provider.max_completion_tokens is 128000 on both. modality is text+image+fileâtext on both.

## No official GPT-6.1 Terra weights on Hugging Face

A GPT-6.1 search on 2026-10-30 did not return an official OpenAI GPT-6.1 Terra repository. Unrelated repos are not treated as official weights. search=GPT-6.1 Terra returned 0 models. hugging_face_id is empty on both OpenRouter rows. supported_efforts includes none on Terra and does not include none on Terra Pro. default_effort is medium on both. Those are fields, not eval scores. Both created dates are 2026-10-26: Terra at 18:13:29 UTC, Terra Pro at 18:15:56 UTC.

These figures describe the public lists only. To put the table on one screen with someone else, follow create and join a room. The room does not serve these models.

## Are the list prices different?

On 2026-10-30 the base prices match: $1.00 in and $5.00 out per 1,000,000 tokens. The override after 272000 prompt tokens also matches, at $2.00 in and $7.50 out.

## Does the API publish a quality difference?

No. The two rows show the same price and the same context, so this API does not publish a quality difference. reasoning.mandatory is false and true. That is a switch. This note does not report eval scores or latency.

## Are the weights on Hugging Face?

No. The 2026-10-30 search for GPT-6.1 has no official OpenAI GPT-6.1 Terra repo, and hugging_face_id is empty on both rows.

## Which context is longer?

Neither. Both are 1050000, on the model field and on top_provider.context_length.
