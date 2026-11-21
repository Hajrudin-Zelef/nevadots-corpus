---
id: collect-261121-veille-ia/deepseek/deepseek-hands-developers-the-whole-chat-app-server-side-search-file-parsing-speech-and-a-clone-read
title: "DeepSeek Hands Developers the Whole Chat App: Server-Side Search, File Parsing, Speech and a Clone-Ready Client | Writeble"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: ["2026-11-15"]
keywords: ["search", "deepseek", "developers", "file", "model", "tool", "text", "chat"]
source: https://www.writeble.com/2026/11/15/deepseek-api-full-app-stack-server-side-search-files-ocr-asr-tts/
source_anchor: ""
source_lines: [1, 44]
section: "DeepSeek"
---
**DeepSeek has opened the features behind its consumer chat app to developers.** In an API update dated **November 13, 2026**, the company said web search, file input, URL reading, voice and content-safety checks, which together make up the DeepSeek App experience, are now available through its API. Developers can switch them on with a few parameters on existing endpoints or call them as standalone services.

The pitch is direct: build richer conversations, or ârecreate the full DeepSeek Appâ inside another product.

The built-in features run on **`deepseek-flash`**, the alias for DeepSeekâs current V4.1 Flash model, across all three API styles it supports: **Messages, Responses and Chat Completions**. The main design choice is that **the tools run on DeepSeekâs side**. The client does not execute any search or fetch. It renders the blocks the service returns and passes them back unchanged in the next round.

The feature set:

- **Web search.** With thinking disabled and the`deepseek_web_search` tool declared, the model decides whether a question needs the web, runs text or image searches, and answers with citations.
- **Deep thinking search.** With the same tool and thinking enabled, the model searches, opens pages and finds text in them across several reasoning steps, checking facts as it reasons.
- **URL reading.** Declaring`deepseek_read_url` makes the model detect links in the userâs message, fetch the pages and answer with their content in context.
- **File input.** PDFs, Word, PowerPoint and image files are parsed, either as plain text or with rich layout, and added to the conversation.
- **Safety checks.** Setting`safety_check` to`{"level": "high"}` reviews both input and output and returns a refusal marker when a risk is found.

Developers cannot force a search. The documentation says `tool_choice` cannot name the two web tools, and such a request fails. The model decides when to go online, and setting `tool_choice` to `none` switches every tool off, including URL reading.

The safety parameter comes with a condition. DeepSeek says public-facing apps must enable it, and that developers remain responsible for content moderation, model registration and AI-generated content labelling where those apply. The check helps with compliance but does not replace a providerâs own review.

Alongside the in-model tools, DeepSeek lists **Search, Fetch, OCR, ASR and TTS** as standalone APIs. Search and Fetch first opened in October. The speech and OCR endpoints have their own limits:

- **Speech-to-text** (`POST /v1/tools/asr` ) takes one 16-bit PCM WAV or Ogg Opus file of up to**8 MiB and 120 seconds** and returns the full transcript. Streaming recognition is not supported. It detects 21 languages automatically, including English, Chinese, Cantonese, Hindi, Arabic, Japanese, Russian and Turkish. For stereo files, only the left channel is transcribed.
- **Text-to-speech** works as a full-text HTTP request of up to 10,000 billed characters or over a**WebSocket** that accepts text in chunks while a model is still generating. Two voices are offered,`stella` and`tide` . The language is detected from the text across ten languages, including Mandarin, Cantonese, English, Japanese, Korean, Arabic and several Southeast Asian languages. Speed control is not supported.
- **OCR** reads PDF, Word, PowerPoint and common image formats page by page, up to**1,000 pages** , with a plain text-recognition mode and a rich-layout mode. Recognised files are not stored in the Files API.

Tool use is billed on top of normal token charges. According to DeepSeekâs pricing page:

- Text or image search and web fetch: **$0.75 per 1,000** each.
- File parsing and OCR: **$0.225 per 1,000 pages** .
- TTS: **$0.45 per 10,000 characters** at list price, currently**$0.225** under a limited-time 50% discount.
- ASR: **$0.116 per hour of audio** at list price, currently**$0.058** .

DeepSeek also published an open-source demo client at `deepseek-ai/chat-demo` under the **MIT licence**, plus a hosted version. It supports reading answers aloud, voice input, language and location settings passed to the model as user metadata, and import and export of chat history. Users can import conversations exported from DeepSeek Chat and keep going in the demo, though an import replaces any existing history.

- #DeepSeek
- #API
- #Web Search
- #Speech
- #Developer Tools

Author

Contributor

AI Systems Architect is a seasoned technology leader with over 15 years of experience in the IT industry working with Fortune 500 companies. With a solid foundation in multi-agent systems, open-source LLM infrastructure, and enterprise deployment, he excels at building scalable production-grade AI platforms.
