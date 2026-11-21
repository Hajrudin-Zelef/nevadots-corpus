---
id: collect-261121-veille-ia/deepseek/deepseek-api-web-search-file-input-and-url-reading
title: "DeepSeek API web search, file input and URL reading"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: ["2026-11-13"]
keywords: ["deepseek", "search", "chat", "reading", "file", "input", "model", "switches"]
source: https://www.systemdesign.academy/explained/deepseek-application-apis
source_anchor: ""
source_lines: [1, 9]
section: "DeepSeek"
---
## What DeepSeek released

DeepSeek's chat app has done more than call a model for a long time. It searches the web, reads the links you paste, answers from files you upload, reads answers aloud and checks content for safety. On 13 November 2026 DeepSeek opened all of that to developers. Its news post says you can "recreate the full DeepSeek App in your own application".

There are two parts. The first is a set of switches on the normal chat request: web search, deep thinking search, file input, URL reading and a safety check, plus two smaller ones, a conversation title and a "user metadata marker" that tells the model today's date and the user's location and language. The second is a set of standalone APIs for the same pieces: Search, Fetch, OCR (reading text from images and scans), speech to text and text to speech.

DeepSeek also published a full chat app built on these APIs, as open source under the MIT licence, at github.com/deepseek-ai/chat-demo. You enter your own API key, and your key, chats and images stay in your browser. It can import the chat history you exported from DeepSeek Chat.

DeepSeek lists these switches for the deepseek-flash model only (DeepSeek-V4.1-Flash-1017 on the pricing page) and does not mention deepseek-v4-pro. This page covers how the switches work, what an answer costs, and the two places where a team moving its chatbot onto them is most likely to get it wrong.
