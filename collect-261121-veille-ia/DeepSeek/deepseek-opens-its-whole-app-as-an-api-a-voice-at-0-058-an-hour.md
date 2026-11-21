---
id: collect-261121-veille-ia/deepseek/deepseek-opens-its-whole-app-as-an-api-a-voice-at-0-058-an-hour
title: "DeepSeek opens its whole app as an API â voice at $0.058 an hour"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: []
keywords: ["deepseek", "search", "model", "hour", "voice", "price", "audio", "characters"]
source: https://siliconnoon.com/deepseek-opens-its-whole-app-as-an-api-voice-at-0058-an-hour/
source_anchor: ""
source_lines: [1, 43]
section: "DeepSeek"
---
DeepSeek spent Friday evening in Beijing publishing the recipe for its own consumer app. Web search, deep-thinking search, file input, link reading, content screening, OCR, speech recognition and text-to-speech are now callable separately as well as inside the chat model, priced per call, and the company open-sourced a chat client that rebuilds the app it ships to consumers â down to importing your exported DeepSeek history. "Developers can even recreate the full DeepSeek App 1:1," QbitAI's writeup of the launch noted. Voice is the genuinely new part: before this release there was no public DeepSeek endpoint for listening or speaking.

The move answers a question the labs have mostly dodged. If your model is a commodity, what exactly are you selling? DeepSeek's answer, in a price list, is the plumbing around the model.

## What is on the meter

The rates are in DeepSeek's own documentation. Speech recognition lists at $0.116 an hour of audio and is currently half price at $0.058. Text-to-speech lists at $0.45 per 10,000 characters and is discounted to $0.225. OCR is $0.225 per 1,000 pages. Search and fetch are $0.75 per 1,000 successful requests. Beijing Business Today, which covered the launch in yuan, reports the same rates as Â¥0.39 an hour, Â¥1.5 per 10,000 characters, Â¥1.5 per 1,000 pages and Â¥5 per 1,000 calls, and makes clear the tools are billed on top of token charges rather than bundled into them (åäº¬åæ¥).

That stacking is the part buyers should internalise. The deepseek-flash model â the V4.1-Flash build â runs $0.30 per million input tokens and $1.20 per million output at peak, half of that off-peak. Turn search on in a chat call and you pay for the searches as well as every token the retrieved pages put into the context window. We covered the first half of this a month ago, when search and fetch became standalone products in Deep Dive â DeepSeek V4.1-Flash-1017: 77% on DeepSWE for 20 cents; what shipped Friday is the rest of the app joining the same meter.

The comparisons are brutal. OpenAI charges $10 per 1,000 web searches, per its own price list, against DeepSeek's $0.75 â before the tokens the search results consume. On voice, OpenAI's live transcription lists at $0.017 a minute, roughly $1.02 an hour, against $0.058.

## The spec sheet is why the price is that low

Those two numbers are not the same product, and DeepSeek's documentation says so in its limits. One speech-to-text request carries one audio file, capped at 8 MiB and 120 seconds, WAV or Ogg Opus, with stereo audio reduced to the left channel. Language is detected rather than chosen. There is no streaming â you send a clip, you get a transcript. Each account can have 50 recognitions in flight.

Read-aloud is narrower still. Two voices, stella and tide, 10,000 billed characters per request, at most 30 minutes of audio per session, no speed control, no cloning, and a Chinese character counts as two characters on the meter. OCR allows 15 documents in progress per account. Nobody runs a contact centre on 50 concurrent two-minute clips, which is exactly why the hourly rate can sit an order of magnitude under a realtime vendor's.

Then there is the clause in the developer guide almost nobody will read: when web search, URL reading, safety screening or automatic titles are switched on, DeepSeek's service takes over the sampling parameters. Temperature, top_p, top_k, seed, stop sequences, parallel tool calls â the values in your request are ignored, because the service is running the turn. You are buying the app's behaviour, not a dial. For a chatbot that is a feature. For a product whose differentiation lives in how the model is steered, it is a price you pay in control.

## Who carries the compliance

The discount comes with obligations, and they are the developer's. DeepSeek's guidance is explicit that a public-facing service must enable the safety check and that the operator then handles "content safety review, large model registration and AI labeling" on its own account. In China that is the generative-AI regime, correctly assigned to whoever serves the user; for a Hangzhou team shipping a WeChat Mini Program it is paperwork they already do. For a European or American team, it is the moment to have a conversation with counsel. The provenance fight is already running in Washington: federal agencies are barred from DeepSeek while its weights remain legal to download, as we set out in Deep Dive â DeepSeek banned from federal agencies. Its weights aren't.

## Why sell the app instead of the model

Two motives, both visible in the announcement. The first is candour: DeepSeek calls its existing chat product "only a fairly basic form" of the idea, per QbitAI, and it has never pretended the consumer app was the business. Open-sourcing a cloneable front end costs it nothing it values.

The second is margin. After two years of price war, Chinese inference has a reseller layer that undercuts the labs and answers for almost nothing, which is the problem Baidu tried to solve this week by opening a token exchange with supplier fingerprinting and credit grades â our read on that one is in Deep Dive â Baidu Token Exchange: verified models, uninsured buyers. A lab that cannot defend a per-token price can still own the index, the OCR engine and the voice model, and bill them per call, where no middleman can substitute a cheaper supplier.

Set that against the American direction of travel and the split is clean. OpenAI keeps pulling the app *into* the model â a wallet, commerce, agents, all inside ChatGPT. DeepSeek pushes the app *out* of the model into a parts catalogue. One bets the assistant is the product. The other bets whoever builds the assistant will buy the parts, and that the parts are where a model company still has a moat.

## The counter-case

Cheap components do not make an app. DeepSeek has handed a few thousand developers a working chat client, and the honest forecast is that most of what gets built is another chat box with a different logo â the demo even ships with light and dark themes and a Chinese/English toggle, which is the level of differentiation on offer. The company's own framing concedes the point: it says the next super app probably will not be a chat window, and that it does not know what it will be, so it would rather hand out the tools than guess.

The pricing is also promotional, not structural. List prices are double what is charged today, and the 50 percent cut has no stated end date. If voice is still $0.058 an hour in February, the floor for building a transcription product has genuinely moved. If it snaps back to $0.116, the discount was customer acquisition, and the real comparison with OpenAI's $1.02 an hour is 9x rather than 17x â still a gap, but a smaller claim.

## What to watch

Whether the 50 percent promo survives the quarter, because every build-or-buy spreadsheet in voice currently rests on it. Whether Google or OpenAI cut tool pricing â $10 per 1,000 searches is the least defensible line on a frontier price list when a competitor sells the same call for 75 cents. And whether any Western cloud starts reselling these endpoints, which would settle the compliance question faster than any lawyer's memo.

*If your model is a commodity and your app is open source, what exactly are you selling? Write to us via our contact page.*
