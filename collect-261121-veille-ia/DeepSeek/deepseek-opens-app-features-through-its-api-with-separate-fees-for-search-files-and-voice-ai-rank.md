---
id: collect-261121-veille-ia/deepseek/deepseek-opens-app-features-through-its-api-with-separate-fees-for-search-files-and-voice-ai-rank
title: "DeepSeek Opens App Features Through Its API, With Separate Fees for Search, Files, and Voice · AI Rank"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: []
keywords: ["search", "deepseek", "tools", "model", "through", "features", "developers", "built-in"]
source: https://airanks.app/news/deepseek-app-api-tools-pricing-1113
source_anchor: ""
source_lines: [1, 53]
section: "DeepSeek"
---
DeepSeek on November 13, 2026 opened the conversation features behind its app and website to developers through the existing DeepSeek API. Official API documentation says developers can turn on web search, file input, and URL reading with built-in tools, while search, fetch, OCR, speech-to-text, and text-to-speech are also available as standalone endpoints.

The company says these tools and the way their results are stitched into the conversation run on the server, so a client does not have to execute the tools itself. The built-in tools are documented for the `deepseek-flash` model, which currently maps to DeepSeek-V4.1-Flash-1017, on the Messages, Responses, and Chat Completions APIs.

## Key Facts

| Item | Detail | 
|---|---|
| Announcement | November 13, 2026, in the DeepSeek API changelog and a product post | 
| Model for built-in tools | `deepseek-flash` (DeepSeek-V4.1-Flash-1017) | 
| Supported APIs | Messages, Responses, and Chat Completions | 
| Built-in tools | `deepseek_web_search` for web or image search, and`deepseek_read_url` for reading links in a user message | 
| Other features in the same release | File input for PDFs, Office documents, and images; an optional safety-check parameter | 
| Standalone APIs | Search, Fetch, OCR, ASR, and TTS | 
| Demo | An open-source front-end demo that can import DeepSeek chat history, with a hosted preview at cdn.deepseek.com/chat-demo | 

According to the docs, web search with thinking turned off lets the model start a web or image search and answer with citations. With thinking turned on, the same `deepseek_web_search` tool is used for multi-step search and page reading, which DeepSeek calls deep thinking search. The `deepseek_read_url` tool fetches pages when a user message contains links. File input extracts text or mixed text-and-image layouts and adds them to the conversation context.

If the safety-check parameter is on, DeepSeek says it applies basic review of model inputs and outputs and returns a refusal marker when a risk is found. The documentation tells developers who offer a public app to enable that parameter, and to handle content review, applicable model registration, and AI-content labeling themselves.

## Pricing

Tool use is billed on top of ordinary token charges. The English Models & Pricing page lists these rates:

| Tool | Unit | Price | 
|---|---|---|
| Search (text or image) | Per 1,000 searches | $0.75 | 
| Web fetch | Per 1,000 fetches | $0.75 | 
| File parsing and OCR | Per 1,000 pages | $0.225 | 
| Text to speech (TTS) | Per 10,000 characters | $0.45 list price, $0.225 limited-time (50% off) | 
| Speech to text (ASR) | Per hour of audio | $0.116 list price, $0.058 limited-time (50% off) | 

Search and web fetch are charged per use, whether the developer calls the Search and Fetch APIs directly or the model uses them through the built-in tools. The pricing page does not say when the 50% speech discount ends. Token prices for `deepseek-flash` are unchanged on that page: off-peak output is $0.60 per million tokens, and peak output is $1.20, with lower rates for cached input. Peak hours are 01:00–04:00 and 06:00–10:00 UTC, Monday through Friday, excluding Chinese public holidays.

## Background

DeepSeek already offered a self-developed search API, documented in an October 17, 2026 changelog entry, and the official web and app products already combined search, files, links, and voice with the model. The November 13 release packages those app-side functions for third-party developers on the same API the company already operates. IT Home and Yicai reported the announcement the same day, matching the official feature list and the extra tool fees.

## What We Don't Know Yet

- The pricing page does not say how long the 50% discount on TTS and ASR will last.
- The announcement says developers can recreate the full DeepSeek app experience, but it does not list every app-only feature or guarantee feature parity in third-party clients.
- DeepSeek has not said in the announcement whether built-in tools will come to models other than `deepseek-flash` .

## Sources

- The full DeepSeek App experience, now via API, DeepSeek API Docs, November 13, 2026
- Change Log: Releasing the full suite of application APIs, DeepSeek API Docs, dated November 13, 2026
- Web Tools, DeepSeek API Docs
- Models & Pricing, DeepSeek API Docs
- deepseek-ai/chat-demo, official demo repository
- DeepSeek 发布全家桶应用 API：开发者将能 1:1 复刻完整 DeepSeek App, IT Home, November 13, 2026
- DeepSeek发布全家桶应用API, Yicai, November 13, 2026
