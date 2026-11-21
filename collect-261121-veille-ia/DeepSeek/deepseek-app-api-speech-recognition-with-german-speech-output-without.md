---
id: collect-261121-veille-ia/deepseek/deepseek-app-api-speech-recognition-with-german-speech-output-without
title: "DeepSeek App API: Speech Recognition With German, Speech Output Without"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: []
keywords: ["speech", "deepseek", "recognition", "output", "german", "search", "file", "endpoints"]
source: https://senn-tech.com/en/blog/deepseek-app-api-asr-tts-ocr-check
source_anchor: ""
source_lines: [1, 100]
section: "DeepSeek"
---
# DeepSeek App API: Speech Recognition With German, Speech Output Without

On 13 November 2026 DeepSeek opened the capabilities of its app as an API. Web search, file input, URL reading and safety checks run as parameters in the ordinary chat call. Search, fetch, OCR, speech recognition and speech output also exist as standalone endpoints. The two voice endpoints differ in one way that matters in operation: speech recognition knows German, speech output does not.

**The DeepSeek App API is the bundle of built-in tools and standalone endpoints with which DeepSeek rebuilds its app's functions for developers. The tools are switched on by parameters such as `deepseek_web_search` or `deepseek_read_url` in the chat call. OCR, speech recognition (ASR) and speech output (TTS) are separate endpoints under `/v1/tools/`, billed by volume and independent of the model.**

## What the vendor opens

The announcement describes two levels. On the first, `deepseek-flash` gets web search with citations, search during multi-step thinking, file input for PDF, Office files and images, URL reading, and a safety check that returns a refusal signal when it detects risk. Public-facing applications must switch the safety check on. Content moderation, model registration and labelling of AI-generated content stay with the developer, according to the same page.

On the second level sit Search, Fetch, OCR, ASR and TTS as standalone endpoints. An open demo front end comes with them and can import chat history from the app. The repository `deepseek-ai/chat-demo` is MIT-licensed, was created on 12 Nov 2026, last pushed on 13 Nov 2026 and shows 2,512 stars in the GitHub API on 15 Nov 2026. We measured these figures ourselves.

A side note on the model: according to the pricing page `deepseek-flash` serves DeepSeek-V4.1-Flash-1017. The old names `deepseek-v4-flash` and `deepseek-v4-flash-vision-exp` are still accepted; their models are retired, and requests run on the new model at the Flash price.

## Prices and limits

Every figure in the two tables is vendor-claimed, read on 15 Nov 2026 from the pricing page and the guides. We billed nothing, because we hold no key and no balance at DeepSeek.

| Item | Unit | List price | Promotional (50 %) | 
|---|---|---|---|
| Search (text) | 1,000 successful requests | $0.75 | not shown | 
| Web fetch | 1,000 successful requests | $0.75 | not shown | 
| File parsing and OCR API | 1,000 pages | $0.225 | not shown | 
| Text to speech (TTS) | 10,000 characters | $0.45 | $0.225 | 
| Speech to text (ASR) | 1 hour of audio | $0.116 | $0.058 | 

The page labels the promotion "Limited-time price". No end date appears there, so the list price is the safe basis for a calculation. For speech output a Chinese character counts double and every other character once.

| Endpoint | Limits according to the guide | 
|---|---|
| ASR | 8 MiB and 120 seconds per file, 16-bit PCM WAV or Ogg Opus, left channel only for stereo, no streaming, at most 50 requests in progress | 
| OCR | 100 MiB and 1,000 pages, upload within 120 seconds with no pause over 30 seconds, 180 seconds total, at most 15 recognitions in progress | 
| TTS | 10,000 billed characters and 64 KiB per request, up to 30 minutes of audio, no speed control, no resume, at most 50 requests in progress; plus a WebSocket mode for text sent in chunks | 

The OCR billing section holds a sentence worth knowing before you budget: a recognition that fails or times out is not billed. If the client disconnects after the file was received, recognition runs to the end and is billed when it completes.

## German appears on one language list only

For speech recognition the guide names twenty-one languages: Chinese, Cantonese, English, Japanese, Korean, German, French, Spanish, Portuguese, Italian, Russian, Arabic, Hindi, Indonesian, Vietnamese, Thai, Turkish, Malay, Filipino, Polish and Hungarian. There is no language parameter; the language is detected from the audio.

For speech output the guide names ten languages: Mandarin, Cantonese, English, Japanese, Korean, Malay, Arabic, Indonesian, Thai and Vietnamese. Again there is no language parameter, and there are two voices, `stella` and `tide`. A German announcement in a customer application is not what this endpoint is documented for. What the voices do with German text we did not test, because we have no key. The documentation does not promise it.

## Our test setup, measured on 15 Nov 2026

For comparison we looked at the voice chain of our test setup, read-only: a Home Assistant container on a private network with no link to the company environment. Method: process list inside the container, an HTTP query against the document service, and a look at the gateway's configuration file. Everything below applies to this test setup, not to our production.

| Component | Finding on 15 Nov 2026 in the test setup | 
|---|---|
| Speech output | Piper voice `de_DE-thorsten-medium` . German runs locally, no cloud. | 
| Speech recognition | `speech_to_phrase` . By the project's description it does not recognise free speech; it picks from the phrases that result from sentence templates and the device, area and floor names exposed to Assist. Whisper or any other free dictation does not run there. | 
| Document OCR | A local document service (docling-serve) answers its health query with HTTP 200. | 
| Further OCR lane | Whether a second OCR lane in the gateway is still active: unsettled. | 
| Audio in the gateway | The gateway configuration lists no audio model. | 

For the test setup three things follow. The gap is speech recognition for free text, exactly where DeepSeek covers German. Speech output we already have locally. And DeepSeek's endpoints are named `/v1/tools/asr` and `/v1/tools/tts`, so a configuration line in the gateway will not bridge the two without a translation layer.

We do not compare cloud and in-house prices. Vendor prices are per volume in US dollars; our costs are depreciation and power on cards that carry other work too. Squeezing both into one unit would be arithmetic without a measurement behind it.

## What this means in operation

First, audio and documents leave the building with these endpoints. The OCR guide only says the file is not stored in the Files API and not passed to a chat model. On storage location and retention we found no statement on the pages we read. That belongs in a data processing agreement discussion before any use.

Second, the limits: 120 seconds and 8 MiB for recognition suit a voice note and do not suit a meeting. 1,000 pages of OCR per request cover a batch of invoices, and a quarterly archive does not fit.

Third, anyone who wants German recognition and cannot let data leave needs a chain of their own for free dictation. Whether such a chain would do better in our test setup than the cloud endpoint, we only know after a test with our own recordings. We have no measurement for that.

Further reading: how prices of cheap models moved in 2026 is in AI price collapse 2026, what the 1017 weights behind `deepseek-flash` change in DeepSeek 1017: old model name, new weights, open speech recognition as a self-build in Nemotron 3.5 Transcribe, more operating measurements in the hub AI in production, and anyone who wants to build the chain in-house can start at AI on your own hardware.

(correction 20 November 2026: on 20 November the pricing page lists search only as "Search API (text)" billed per 1,000 successful requests; "image" in the table above comes from the 15 November version and is no longer supported. New since launch: from 12:00 Beijing time on 18 November 2026, search and fetch calls that the server triggers itself during a model call are no longer billed separately; their results count as tokens. The standalone Search and Fetch endpoints remain billed per call. The chat-demo repository has 5,090 stars per the GitHub API on 20 November 2026 (15 Nov: 2,512). Prices, language lists and limits in the guides match the table on 20 November. The Speech-to-Phrase description was re-read; the project notes that development has moved to another repository. We could not retrieve the Pandaily article on 20 November, so the sentence on its agreement is not re-checked. The test setup of 15 November was not re-measured.)

## Open points

- Prices and limits are vendor figures from 15 Nov 2026, and the promotion shows no end date. We measured no billing of our own; no key.
- How speech output handles German text: untested.
- Whether the second OCR lane in the gateway still runs: unsettled.
- Storage location and retention of uploaded files and audio are not stated on the pages we read.
- The Pandaily report (independent) matches the vendor documentation on prices and feature list; we take no additional facts from it.

Read next: running Nod from n8n Labs ourselves · Claude Fable 5.2: price and retention period

## Further Reading

- Announcement of 13 Nov 2026 (vendor, primary): scope of features, mandatory safety check.
- Pricing page (vendor, primary): all prices, model name, legacy names.
- Speech-to-text guide, text-to-speech guide, OCR guide (vendor, primary): language lists, limits, billing.
- chat-demo on GitHub (vendor, primary): MIT licence; stars and push date measured by us through the GitHub API.
- Speech-to-Phrase (project, primary): what the recogniser does and what it does not.
- Pandaily, 14 Nov 2026 (independent): a second reading of the announcement.

## Can the DeepSeek App API speak German?+

No. The text-to-speech guide lists ten languages: Mandarin, Cantonese, English, Japanese, Korean, Malay, Arabic, Indonesian, Thai and Vietnamese. German is missing, and there is no language parameter; the language is detected from the text. The speech-to-text guide lists twenty-one languages, German among them.

## What does DeepSeek OCR cost?+

The pricing page lists $0.225 per 1,000 pages for file parsing and the OCR API. According to its billing section only successfully recognised pages are charged. If the client disconnects after the file has arrived, recognition keeps running and is billed when it succeeds.

## Do I need the API to have speech output in operation?+

No. In our test setup a Piper voice runs locally, without a cloud and without a vendor's language list. For free-form dictation we have no chain of our own in the test; our test setup only recognises phrases fixed in advance.
