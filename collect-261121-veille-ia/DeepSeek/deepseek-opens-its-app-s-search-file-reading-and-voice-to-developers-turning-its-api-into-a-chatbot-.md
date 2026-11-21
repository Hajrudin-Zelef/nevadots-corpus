---
id: collect-261121-veille-ia/deepseek/deepseek-opens-its-app-s-search-file-reading-and-voice-to-developers-turning-its-api-into-a-chatbot-
title: "DeepSeek Opens Its App's Search, File Reading and Voice to Developers, Turning Its API Into a Chatbot Kit"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: ["2026-11-13"]
keywords: ["deepseek", "search", "model", "file", "reading", "voice", "developers", "page"]
source: https://dailymech.com/articles/deepseek-api-app-features-web-search-ocr-asr-tts.html
source_anchor: ""
source_lines: [1, 100]
section: "DeepSeek"
---
# DeepSeek Opens Its App's Search, File Reading and Voice to Developers, Turning Its API Into a Chatbot Kit

A Nov. 13 update lets DeepSeek's Flash model search the web, read links and parse documents on DeepSeek's own servers. It also adds standalone OCR, speech-to-text and text-to-speech APIs and an open-source demo client that can import a user's DeepSeek chat history.

Published

Last updated / Fact-checked on

*Illustrative photo of source code on a monitor. It does not show DeepSeek's software or API.*

Until this week, a developer who wanted to rebuild the DeepSeek app on top of the DeepSeek API had to add much of it alone. The model was there, but the app's document parsing, link reading and voice features were not offered as ready-made API tools.

On Nov. 13, DeepSeek changed that. In a post titled "The full DeepSeek App experience, now via API," the Chinese AI lab said the capabilities behind its consumer app are now available to developers, either as built-in features switched on with a few parameters or as separate APIs. "Build richer conversations or recreate the full DeepSeek App in your own application," the company wrote.

## At a glance

| Item | Detail | Source | 
|---|---|---|
| Announced | Nov. 13, 2026 | DeepSeek change log | 
| Model | `deepseek-flash` (DeepSeek-V4.1-Flash-1017) | DeepSeek pricing page | 
| Built-in features | Web search, deep thinking search, file input, URL reading, safety checks | DeepSeek announcement | 
| Standalone APIs | Search, Fetch, OCR, speech-to-text (ASR), text-to-speech (TTS) | DeepSeek announcement | 
| Search and fetch price | $0.75 per 1,000 searches or fetches | DeepSeek pricing page | 
| OCR and file parsing price | $0.225 per 1,000 pages | DeepSeek pricing page | 
| Voice prices | TTS $0.225 per 10,000 characters and ASR $0.058 per audio hour, both at a limited-time 50% discount | DeepSeek pricing page | 
| Demo | Open-source chat client that can import DeepSeek chat history | DeepSeek announcement | 

## What developers get

The built-in features run on `deepseek-flash`, which DeepSeek's pricing page identifies as the DeepSeek-V4.1-Flash-1017 model. They work across three API styles the company already supports: an Anthropic-style Messages API, an OpenAI-style Responses API and the older Chat Completions format. DeepSeek says the tool calls and the job of stitching results into the model's context are "handled server-side," so a developer's app does not have to run the searches itself.

DeepSeek lists five features:

- **Web search.** With thinking turned off and the`deepseek_web_search` tool enabled, the model searches the web or images as needed and answers with citations.
- **Deep thinking search.** With thinking turned on, the model searches and reads pages on its own over several reasoning steps.
- **File input.** PDFs, Office files, images and other documents are parsed into the conversation, either as plain text or with "rich layout" parsing.
- **URL reading.** With the`deepseek_read_url` tool enabled, the model spots links in a user's message, fetches the pages and answers with them in context.
- **Safety checks.** A parameter that screens inputs and outputs and returns a refusal marker when it detects a risk.

The company's chatbot guide adds smaller pieces that make an app feel finished: a "user metadata marker" that tells the model the date and the user's location and language, automatic conversation titles, read-aloud and voice input.

There is a trade-off for developers who like to tune their models. When the web tools, the high safety setting or automatic titles are switched on, DeepSeek says "the service decides" several settings, and values such as `temperature` and `top_p` in the request are ignored.

## The standalone pieces

Search, Fetch, OCR, speech-to-text and text-to-speech can also be called on their own, outside a chat.

The OCR endpoint reads PDFs, Word and PowerPoint files and images and returns text page by page. It offers a plain `text_recognition` mode and a `rich_layout_recognition` mode, and DeepSeek says the uploaded file is not stored in its Files API.

The speech-to-text endpoint takes a whole audio file in one request, in 16-bit PCM WAV or Ogg Opus format, and returns a full transcript.

The text-to-speech service has two voices, a default female voice called Stella and a male voice called Tide. It can be called with one block of text or streamed over a WebSocket while a model is still writing. DeepSeek lists 10 languages: Mandarin, Cantonese, English, Japanese, Korean, Malay, Arabic, Indonesian, Thai and Vietnamese. There is no language setting; the service detects the language from the text. One request can cover up to 10,000 billed characters and return up to 30 minutes of audio.

## What it costs

The new tools are billed on top of normal token charges. According to DeepSeek's pricing page:

- Web search (text or image) and web fetch each cost $0.75 per 1,000 uses, whether called directly or by the model.
- File parsing and OCR cost $0.225 per 1,000 pages.
- Text-to-speech lists at $0.45 per 10,000 characters and speech-to-text at $0.116 per hour of audio. Both are currently half price "for a limited time." A Chinese character counts as two characters for TTS billing.

The model itself stays cheap by Western standards. DeepSeek lists Flash at $0.30 per million input tokens without a cache hit and $1.20 per million output tokens at peak, with off-peak rates half that. Peak hours are 01:00 to 04:00 and 06:00 to 10:00 UTC on weekdays. Every hour of the weekend counts as off-peak.

## A demo that imports your history

DeepSeek also released an open-source demo chat client on GitHub. It can import chat history exported from DeepSeek's own chat service, so users can pick up old conversations in an app running on their own API key. DeepSeek says the key, conversations and uploaded images stay in the user's browser, and that the demo is "for demonstration only."

## The compliance fine print

DeepSeek is explicit that public apps carry obligations. "Public-facing apps must enable safety checks," the announcement says. It also says that developers stay responsible for content moderation, "applicable model registration" and labeling AI-generated content. DeepSeek's documents do not spell out which rules those duties come from. The safety check itself is described only as "basic risk control."

## Why it matters

The move turns DeepSeek's API from a model endpoint into something closer to a complete chatbot kit. For developers, the draw is fewer vendors and fewer moving parts. Search, document reading and voice now come from one provider and one bill.

It also builds on DeepSeek's search push. In an Oct. 17 change log entry, the company said it had opened its in-house search engine through the API and claimed the index covers "hundreds of billions of web pages." Charging per search and per fetch makes that engine a product of its own.

The catch is familiar for any China-based provider. Companies outside China will weigh low prices against where data is processed and the content rules built into the service. DeepSeek's own documents show the safety layer and registration duties are part of the deal.

## What to watch

Watch whether third-party developers adopt the built-in tools instead of running their own search and retrieval, and whether DeepSeek keeps the voice discounts. Also watch for independent tests of search quality and OCR accuracy. So far, every claim about those features comes from DeepSeek.

## How we reported this

DailyMech read DeepSeek's Nov. 13 announcement, its API change log, its chatbot guide, its pricing page and its OCR, speech-to-text and text-to-speech documentation in full. All features, limits and prices are as DeepSeek published them on Nov. 14, 2026. DailyMech did not test the API, and no independent evaluation was available. DailyMech contacted no one for this article.

*Published November 14, 2026. Facts checked against the cited sources by DailyMech's AI writer.*

## Sources

- The full DeepSeek App experience, now via API, DeepSeek API Docs, Nov. 13, 2026 (fully read).
- Change Log, DeepSeek API Docs, entries dated Nov. 13 and Oct. 17, 2026 (fully read).
- Build a Chatbot, DeepSeek API Docs, accessed Nov. 14, 2026 (fully read).
- Models & Pricing, DeepSeek API Docs, accessed Nov. 14, 2026 (fully read).
- OCR API reference, DeepSeek API Docs, accessed Nov. 14, 2026 (fully read).
- Speech to Text API reference, DeepSeek API Docs, accessed Nov. 14, 2026 (fully read).
- Text to Speech guide, DeepSeek API Docs, accessed Nov. 14, 2026 (fully read).

*DailyMech used AI tools to research and draft this article. Facts were checked against the cited sources by DailyMech's AI writer, not by a human editor.*
