---
id: collect-261121-veille-ia/deepseek/deepseek-api-adds-web-search-file-input-and-voice-tools
title: "DeepSeek API adds web search, file input and voice tools"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: ["2026-11-13"]
keywords: ["deepseek", "search", "input", "file", "tools", "model", "voice", "safety"]
source: https://routerplus.com/news/deepseek-api-app-features-search-ocr-tts
source_anchor: ""
source_lines: [1, 63]
section: "DeepSeek"
---
# DeepSeek API adds web search, file input and voice tools

The DeepSeek App's search, file, link, safety and voice features now work on deepseek-flash.

DeepSeek released its Full-Suite App API on November 13, 2026. The features behind the DeepSeek App (search, file input, link reading, safety checks and voice) now work through the DeepSeek API, on the `deepseek-flash` model. DeepSeek already sold Search and Fetch as standalone APIs (earlier coverage).

## What changed

- `deepseek-flash` (DeepSeek-V4.1-Flash-1017) now supports web search, deep thinking search, file input, URL reading and safety checks across the Messages, Responses and Chat Completions APIs. DeepSeek runs the tool calls and context assembly server-side.
- Turn on search with the `deepseek_web_search` tool and link reading with the`deepseek_read_url` tool. With thinking off, the model searches the web or images and answers with citations. With thinking on, it searches and reads pages over multiple reasoning steps.
- Public-facing apps must enable safety checks (the `safety_check` parameter). DeepSeek says developers remain responsible for content moderation, model registration and AI-generated content labeling.
- Search, Fetch, OCR, ASR (speech to text) and TTS (text to speech) are also available as standalone APIs.
- DeepSeek published an open-source chat frontend demo (`deepseek-ai/chat-demo` ) that can import DeepSeek chat history.

## Tool prices

From DeepSeek's pricing page on November 13, 2026. Tool fees are charged on top of token charges. DeepSeek lists the speech discounts as limited-time and gives no end date.

| Tool | Unit | List price | Current price | 
|---|---|---|---|
| Search (text, image) | 1,000 searches | $0.75 | $0.75 | 
| Web fetch | 1,000 fetches | $0.75 | $0.75 | 
| File parsing and OCR | 1,000 pages | $0.225 | $0.225 | 
| Text to speech (TTS) | 10,000 characters | $0.45 | $0.225 | 
| Speech to text (ASR) | 1 hour of audio | $0.116 | $0.058 | 

**Update, November 19, 2026:** From 12:00 Beijing time on November 18 (04:00 UTC), DeepSeek no longer bills search and fetch calls that the model triggers through its built-in tools. Their results are billed as model tokens. The standalone Search and Fetch APIs still cost $0.75 per 1,000 successful requests. Details.

DeepSeek's own token prices for `deepseek-flash` are $0.15 input, $0.003 cached input and $0.60 output per million tokens off-peak, and double that at peak (01:00 to 04:00 and 06:00 to 10:00 UTC, Monday to Friday), per the same page.

## DeepSeek V4.1 Flash 1017 on RouterPlus

DeepSeek documents the built-in tools for its own API at api.deepseek.com. For model tokens only, the RouterPlus catalog lists DeepSeek V4.1 Flash 1017 as `deepseek/deepseek-v4.1-flash-1017` on 7 hosts. Per million tokens, from the catalog on November 13, 2026:

| Provider | Input | Cached input | Output | 
|---|---|---|---|
| Sail Research | $0.09 | $0.006 | $0.30 | 
| Wafer | $0.13 | $0.008 | $0.50 | 
| Doubleword | $0.15 | $0.003 | $0.60 | 
| RunInfra | $0.15 | $0.01 | $0.60 | 
| Particle AI | $0.2125 | $0.0255 | $0.85 | 
| Munito AI | $0.27 | $0.0054 | $1.00 | 
| Nebius | $0.30 | $0.006 | $1.20 | 

## What to do

To try the tools on DeepSeek's API, add them to a `deepseek-flash` request. From DeepSeek's chatbot guide (Messages API):

```
curl https://api.deepseek.com/anthropic/v1/messages \
  -H "Content-Type: application/json" \
  -H "x-api-key: ${DEEPSEEK_API_KEY}" \
  -d '{
    "model": "deepseek-flash",
    "thinking": {"type": "enabled"},
    "tools": [
      {"type": "deepseek_web_search", "name": "deepseek_web_search"},
      {"type": "deepseek_read_url", "name": "deepseek_read_url"}
    ],
    "safety_check": {"level": "high"},
    "messages": [{"role": "user", "content": "What is new in DeepSeek V4.1?"}]
  }'
```
