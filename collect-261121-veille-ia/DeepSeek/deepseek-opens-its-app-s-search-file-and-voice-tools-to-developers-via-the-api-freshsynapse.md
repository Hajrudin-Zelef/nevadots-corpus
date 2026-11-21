---
id: collect-261121-veille-ia/deepseek/deepseek-opens-its-app-s-search-file-and-voice-tools-to-developers-via-the-api-freshsynapse
title: "DeepSeek Opens Its App's Search, File and Voice Tools to Developers via the API | FreshSynapse"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: ["2026-11-15"]
keywords: ["deepseek", "search", "freshsynapse", "developers", "tools", "apis", "file", "voice"]
source: https://freshsynapse.com/articles/deepseek-api-application-tools-2026-11-15/
source_anchor: ""
source_lines: [1, 16]
section: "DeepSeek"
---
*Photo: Hippopx (public domain). Illustrative image.*

**What happened.** DeepSeek has opened the toolset behind its own app to outside developers. A changelog entry dated 13 November 2026, “Releasing the full suite of application APIs”, makes the web search, file input, URL reading, voice and safety features of the DeepSeek app available through the DeepSeek API — the same capabilities the company’s own chatbot uses, now callable from any developer’s application.

**What developers actually get.** The headline tools are `deepseek_web_search`, a cited web search that can run a multi-step “deep thinking search” with the model’s reasoning switched on, and `deepseek_read_url`, which fetches and reads a page. File input parses PDFs, Office documents and images, so an application can feed a document straight into a conversation instead of pasting its text. A safety-check parameter returns a refusal marker when content trips it. All of it works with `deepseek-flash` — DeepSeek-V4.1-Flash-1017 — across the Messages, Responses and Chat Completions APIs, so developers do not have to rewrite their integration to use the new tools.

**The standalone APIs matter as much as the bundle.** Alongside the app tools, DeepSeek launched separate Search, Fetch, OCR, ASR and TTS APIs, so a developer can buy one capability without the rest. Per DeepSeek’s pricing pages, Search and Fetch cost $0.75 per 1,000 requests and OCR $0.225 per 1,000 pages, with voice services at 50% off for a limited time. That itemised pricing is the practical point: a small app that only needs to read URLs or transcribe audio can pay for that alone, rather than for a bundled model call.

**Why this is a developer story, not a model story.** DeepSeek’s open-weight releases usually get the attention — Kimi K3.1-Pro is the latest open-weight model to draw the same crowd — but this update is about distribution, not weights. The company is turning the features that made its app sticky into platform primitives, the same move that turned OpenAI’s ChatGPT into a payments surface this week. The open question is quality: app tools tuned for one chatbot do not always behave well in other people’s products, and DeepSeek’s own documentation is the only evidence so far. The test for developers is the one the site always recommends — run the tools on your own real tasks before building on them, and evaluate the new tool against the alternatives rather than the announcement.

## Sources

- DeepSeek — “Releasing the full suite of application APIs” (API changelog, 13 November 2026): https://api-docs.deepseek.com/updates/?release=deepseek-v4
- FreshSynapse — “Kimi K3.1-Pro Arrives as Open Weights”: https://freshsynapse.com/articles/kimi-k3-1-pro-open-weights-2026-11-05/
- FreshSynapse — “OpenAI Launches ChatGPT Wallet”: https://freshsynapse.com/articles/openai-chatgpt-wallet-agents-pay-2026-11-14/
- FreshSynapse — “How to Evaluate a New AI Tool”: https://freshsynapse.com/articles/how-to-evaluate-a-new-ai-tool/
