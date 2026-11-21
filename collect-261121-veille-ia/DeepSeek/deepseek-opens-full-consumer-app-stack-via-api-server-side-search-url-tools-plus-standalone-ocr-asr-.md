---
id: collect-261121-veille-ia/deepseek/deepseek-opens-full-consumer-app-stack-via-api-server-side-search-url-tools-plus-standalone-ocr-asr-
title: "DeepSeek Opens Full Consumer App Stack via API: Server-Side Search & URL Tools Plus Standalone OCR, ASR, and TTS â my ai daily"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: []
keywords: ["code", "deepseek", "tools", "search", "standalone", "full", "stack", "chat"]
source: https://www.myaidaily.net/news/deepseek-full-suite-app-api-search-ocr-asr-tts/
source_anchor: ""
source_lines: [1, 29]
section: "DeepSeek"
---
THE STORY IN THREE POINTS

- DeepSeek exposed its full DeepSeek App stack via API on November 13, enabling deepseek_web_search, deepseek_read_url, file parsing, safety_check, auto-titling, and user metadata markers on deepseek-flash across Messages, Responses, and Chat Completions.
- Five standalone endpoints under api.deepseek.com/v1/tools cover Search ($0.75/1K), Markdown Fetch ($0.75/1K), page-by-page OCR with coordinate boxes ($0.225/1K pages), 21-language ASR ($0.058/hr promo vs $0.116), and streaming WebSocket/HTTP TTS ($0.225/10K chars promo vs $0.45).
- DeepSeek also open-sourced chat-demo (GitHub deepseek-ai/chat-demo and cdn.deepseek.com/chat-demo), a pure-frontend browser client storing API keys in localStorage and supporting ZIP conversation imports from DeepSeek Chat.

## Built-in server-side Search, URL Reading, File Parsing, and Safety Checks

In a November 13 developer release (<code>news261113</code>), DeepSeek opened the complete feature pipeline behind its consumer DeepSeek App to API callers. Rather than building custom scraping queues or document chunkers, developers querying DeepSeek-V4.1-Flash (<code>deepseek-flash</code>) through the Anthropic Messages, OpenAI Responses, or Chat Completions formats can declare <code>deepseek_web_search</code> (which toggles between cited web/image search when thinking is off and multi-step deep-thinking web research when thinking is enabled) and <code>deepseek_read_url</code> to auto-fetch links pasted in user prompts.

Document attachments uploaded via the Files API support two parsing modes detailed alongside DeepSeekâs document vision stack: <code>text_recognition</code> for pure prose extraction or <code>rich_layout_recognition</code> to crop charts, tables, and diagrams back into context at their original page position. Developers serving public apps are instructed to enable <code>safety_check: {"level": "high"}</code>, which screens prompts, <code>tool_result</code> blocks, uploaded images, and full model responses before closing the streamâreturning protocol-specific refusal markers (while still billing tokens generated prior to refusal) and keeping primary compliance duties on the operator.

## Standalone /v1/tools endpoints: OCR coordinates, 21-language ASR, and WebSocket TTS

For custom RAG pipelines and audio apps that decouple tools from chat turns, DeepSeek exposed five standalone REST and WebSocket endpoints billed per usage on top of base token rates. Building on its earlier Search and Fetch rollout, <code>POST /v1/tools/search</code> (1â50 results with <code>site:</code>/<code>-site:</code> filters) and <code>POST /v1/tools/fetch</code> (returning clean Markdown) both cost $0.75 per 1,000 requests, while <code>POST /v1/tools/ocr</code> charges $0.225 per 1,000 pages for PDFs, Office files, or images up to 100 MiB and 1,000 pagesâemitting 0â1000 normalized bounding-box coordinates without persisting files to storage.

Speech endpoints introduce aggressive promotional pricing at 50% off list rates: <code>POST /v1/tools/asr</code> transcribes 16-bit PCM WAV or Ogg Opus clips up to 8 MiB / 120 seconds across 21 auto-detected languages (including Mandarin, Cantonese, English, Japanese, and Korean) at $0.058 per audio hour ($0.116 list), billed by exact seconds at up to 50 concurrent jobs. Text-to-Speech runs over <code>POST /v1/tools/tts</code> or WebSocket streaming (<code>wss://api.deepseek.com/v1/tools/tts</code>) using <code>stella</code> (female default) and <code>tide</code> (male) voices across 10 languages at $0.225 per 10,000 billed characters ($0.45 list; 1 Chinese character counts as 2 characters).

## Open-source chat-demo: Recreating DeepSeek Chat locally with ZIP history import

To demonstrate all capabilities end to endâincluding automatic conversation titling and <code>latest_reminder</code> metadata for user date, city, and languageâDeepSeek published the MIT-licensed <code>deepseek-ai/chat-demo</code> static web app alongside a live host at <code>cdn.deepseek.com/chat-demo</code>.

Designed to run with zero server backend (keeping API keys in browser <code>localStorage</code> and chats in <code>IndexedDB</code>), the client renders Markdown, KaTeX, and Mermaid diagrams cleanly in Chinese and English and supports importing ZIP conversation archives exported directly from consumer DeepSeek Chat.

Sources: Full DeepSeek App API Launch Announcement (@deepseek_ai)ï½X â DeepSeek Full-Suite App API Release (2026/11/13)ï½DeepSeek API Docs â Build a Chatbot, Web Tools, OCR, ASR, TTS & Pricingï½DeepSeek API Docs â deepseek-ai/chat-demo Open-Source Clientï½GitHub â

THE BROADER CONTEXT Â· DEEPSEEK DOSSIER

Explore full product specifications, pricing analysis, and continuous coverage in the **DeepSeek Desk Hub** and **DeepSeek Models Dossier**. Independent benchmarks available on the **AI Intelligence Index** and **Coding Agents Leaderboard**.
