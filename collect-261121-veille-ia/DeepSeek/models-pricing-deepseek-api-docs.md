---
id: collect-261121-veille-ia/deepseek/models-pricing-deepseek-api-docs
title: "Models & Pricing | DeepSeek API Docs"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: []
keywords: ["model", "search", "price", "fetch", "models", "pricing", "unit", "text"]
source: https://api-docs.deepseek.com/quick_start/pricing/?nb_kwd=best+social+media+ai&nb_mt=p&nb_ti=kwd-2297875758714&scroll_speed=instant
source_anchor: ""
source_lines: [1, 44]
section: "DeepSeek"
---
# Models & Pricing

Model prices are listed per 1M tokens. A token, the smallest unit of text that the model recognizes, can be a word, a number, or even a punctuation mark. We bill model usage based on the total number of input and output tokens by the model.

## Model Detailsâ


(1) Use `deepseek-flash` as the model name. The legacy names `deepseek-v4-flash` and `deepseek-v4-flash-vision-exp` are still accepted, but the corresponding models have been retired, their requests are served by the DeepSeek-V4.1-Flash-1017 model and billed at the Flash price.

(2) Off-peak rates are half of the peak rates. Peak hours are 01:00 - 04:00 and 06:00 - 10:00 UTC, Monday through Friday, excluding Chinese public holidays. All other hours are off-peak, including weekends and Chinese public holidays in full.

(3) For more details on concurrency limits, please refer to Rate Limit & Isolation.

## Search & Fetch API Pricingâ

The Search and Fetch APIs are billed per use.

| Item | Unit | Price | 
|---|---|---|
| Search API (text) | Per 1,000 successful requests | $0.75 | 
| Fetch API | Per 1,000 successful requests | $0.75 | 

For the billing rules, see Search & Fetch.

Starting 12:00 on November 18, 2026 (Beijing Time, UTC+8), the server-side search and web fetch tool calls triggered when calling the model are no longer billed; the results they return are appended to the model context and billed as model token usage. Server-side search is triggered when you use the web tools, the `web_search` tool in the Anthropic API or the `web_search` tool in the Responses API.

## File Parsing & OCR Pricingâ

| Item | Unit | Price | 
|---|---|---|
| File parsing, OCR API | Per 1,000 pages | $0.225 | 

## Speech Pricingâ

| API | Unit | List price | Limited-time price (50% off) | 
|---|---|---|---|
| Text to Speech (TTS) | Per 10,000 characters | $0.45 | $0.225 | 
| Speech to Text (ASR) | Per hour of audio | $0.116 | $0.058 | 

## Deduction Rulesâ

For model usage, the expense = number of tokens Ã price. Search API and Fetch API calls are charged per use at the rates above. The corresponding fees will be directly deducted from your topped-up balance or granted balance, with a preference for using the granted balance first when both balances are available.

Product prices may vary and DeepSeek reserves the right to adjust them. We recommend topping up based on your actual usage and regularly checking this page for the most recent pricing information.
