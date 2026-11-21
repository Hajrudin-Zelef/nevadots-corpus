---
id: collect-261121-veille-ia/deepseek/deepseek-lets-developers-rebuild-its-chatbot-app-through-paid-api-tools-a-eastfrontier
title: "DeepSeek Lets Developers Rebuild Its Chatbot App Through Paid API Tools Â· EastFrontier"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: ["2026-11-16"]
keywords: ["search", "model", "deepseek", "chatbot", "developers", "yuan", "safety", "speech"]
source: https://eastfrontier.com/2026/11/16/deepseek-lets-developers-rebuild-its-chatbot-app-through-paid-api-tools/
source_anchor: ""
source_lines: [1, 27]
section: "DeepSeek"
---
Companies can now ship a chatbot with DeepSeek's own search, document reading and moderation without building those layers, though operators keep China's content review, registration and labeling duties.

DeepSeek has turned the features of its consumer chatbot into paid building blocks for other companies. On the evening of November 13, the Hangzhou developer’s announcement opened web search, search during step-by-step reasoning, file input, link reading and a safety filter on its Flash model API, together with standalone services for search, page fetching, text recognition (OCR), speech recognition and speech synthesis. The company said developers could use them to "1:1 å¤å»åºå®æ´ç DeepSeek App", rebuilding its full app one to one. IT Home and Ifeng Tech reported the launch within 12 minutes of each other that night.

The tools work with deepseek-flash, the model name that has pointed to DeepSeek-V4.1-Flash-1017 since October 17, through its Messages, Responses and Chat Completions interfaces. The server runs the tool calls and assembles the context itself, so a developer switches a feature on with a parameter instead of wiring together a search engine, a document parser and a moderation service. Every tool call costs money on top of the usual token charges.

Part of the package is not new. DeepSeek’s changelog shows the standalone Search API, built on DeepSeek’s own search engine, went live on October 17, alongside the Flash model update EastFrontier covered at the time. What changed on November 13 is that the model can now search, read pages and parse files inside a conversation, and that the voice, OCR and safety layers of the app are on sale too.

## Search inside the model is priced like the standalone service

DeepSeek charges 5 yuan, or US$0.75, per 1,000 web searches or page fetches, whether a developer calls the tool directly or the model searches on its own. Parsing documents and OCR cost 1.5 yuan per 1,000 pages, its Chinese price list shows. Speech synthesis is listed at 3 yuan per 10,000 characters and speech recognition at 0.78 yuan per hour of audio, both currently halved in a limited-time offer for which no end date is given. The English price list gives the same rates in dollars, from US$0.225 per 1,000 pages to US$0.058 per hour of discounted speech recognition.

Usage can add up quickly in reasoning mode. The web tools guide says the model makes up to about 20 tool calls per answer in deep search, that each query counts separately, and that failed or empty searches are free. Link reading considers only the first 10 distinct links in a user’s message. The file parsing guide accepts PDF, Word, PowerPoint and image files of up to 100 MiB and 1,000 pages, billed once per page at upload. The model itself costs 4 yuan per million output tokens off-peak and 8 yuan at peak hours on Chinese weekdays.

The open-source demo client, released under the MIT license, shows what the parts add up to: search, link reading, uploads, voice input and spoken answers in a browser page that keeps keys and chats on the user’s device. It can import chat history exported from DeepSeek’s own web app, according to the chatbot guide. The repository had about 1,900 stars and 314 forks by November 15. TechWeb, in a report carried by Sina Tech, said the release reflects a shift in AI competition from a contest over model parameters toward building application ecosystems.

## The safety switch shifts compliance to the operator

The moderation layer comes with obligations. With safety_check set to "high", DeepSeek reviews both what users send and what the model writes, and returns a refusal when it flags something. Its English announcement says public-facing apps must enable the check and that "Developers remain responsible for content moderation, applicable model registration, and AI-generated content labeling." The safety guide is blunter in Chinese: "æ¨æ¯å å®¹å®å ¨å®¡æ ¸çç¬¬ä¸è´£ä»»äºº", meaning the operator carries primary responsibility for content review.

The check is also billed. Review tokens are charged at the output-token rate, and a rejected input still costs its input tokens at the cache-hit price. Because streamed text cannot be recalled, the guide tells developers to delete anything already shown when a refusal arrives after generation.

## DeepSeek’s own app may be moving toward paid tiers

Signs that DeepSeek may start charging its own app users surfaced the morning after the API launch. A later IT Home report said on November 14 that developers had found quota-limit code on chat.deepseek.com, including a "usage limit reached" prompt and a five-minute quota check, which users linked to a package called "Coding Plan". DeepSeek has announced no subscription, price or launch date. IT Home also put the app at about 130 million monthly users, the figure research firm QuestMobile gave for DeepSeek in data TechNode reported in July, and said annualized revenue above US$1 billion came almost entirely from the API. 36Kr reported in September, citing The Information, that annual recurring revenue had passed US$1 billion.

The API is where that revenue is earned, and the new tools give it more to sell per conversation. They also tie in with DeepSeek’s Harness agent, whose team IT Home said was merged in October with the web, app and platform teams; EastFrontier reported this month that DeepSeek plans an official plugin marketplace for Harness. The next marker is whether the Coding Plan quota appears in the app with a price, and whether DeepSeek extends the new tools beyond its Flash model.
