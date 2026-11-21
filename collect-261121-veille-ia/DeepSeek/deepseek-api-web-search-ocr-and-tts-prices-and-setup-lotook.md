---
id: collect-261121-veille-ia/deepseek/deepseek-api-web-search-ocr-and-tts-prices-and-setup-lotook
title: "DeepSeek API Web Search, OCR, and TTS: Prices and Setup | Lotook"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: []
keywords: ["deepseek", "search", "model", "deepseek-flash", "chat", "november", "alongside", "fetch"]
source: https://lotook.com/articles/deepseek-api-web-search-ocr-voice/
source_anchor: ""
source_lines: [1, 148]
section: "DeepSeek"
---
**DeepSeek opened web search, deep thinking search, URL reading, file input, and a safety check to developers on `deepseek-flash` across its Messages, Responses, and Chat Completions endpoints in its November 13, 2026 Change Log, alongside standalone Search, Fetch, OCR, speech-to-text, and text-to-speech APIs. Starting 12:00 on November 18, 2026 (Beijing Time, UTC+8), server-side search and web fetch triggered when calling the model (web tools, including in-chat web search, image search, URL reading, and the `web_search` tools) are no longer billed separately and only incur model token fees, while standalone Search (text) and Fetch APIs remain billed alongside OCR, ASR, and TTS.**

## What Did DeepSeek Announce on November 13, 2026?

DeepSeek published a November 13, 2026 entry in its official Change Log titled “Releasing the full suite of application APIs,” bringing the capabilities behind the DeepSeek App and web client to developers. DeepSeek stated that web search, file input, URL reading, voice interaction, and safety already run alongside model inference inside its app, adding: “Today we open these capabilities to developers,” with tool execution and context assembly managed by the service. DeepSeek also shared the release on its official X account on November 13 Beijing time.

The rollout covers five integrated capabilities for `deepseek-flash` chats and four standalone utility APIs: Search and Fetch, OCR, automatic speech recognition (ASR), and text to speech (TTS). This follows an October 17, 2026 Change Log update in which DeepSeek introduced its in-house search API (DeepSeek Search), which it says indexes hundreds of billions of web pages, alongside the `DeepSeek-V4.1-Flash-1017` release under the `deepseek-flash` model name.

To demonstrate the endpoints, DeepSeek open-sourced a browser-based chat client (live demo and GitHub repository) that can import exported DeepSeek chat histories. DeepSeek notes that API keys, conversations, and uploaded images stay inside the browser and that the client is for demonstration only.

## How Do the Five In-Chat Capabilities Work on deepseek-flash?

All five in-chat capabilities run on `deepseek-flash` (currently listed as `DeepSeek-V4.1-Flash-1017` on DeepSeek’s pricing page) across three API endpoints:

- **Messages (Anthropic format)** :`POST https://api.deepseek.com/anthropic/v1/messages`
- **Responses** :`POST https://api.deepseek.com/responses`
- **Chat Completions** :`POST https://api.deepseek.com/chat/completions`

According to the Build a Chatbot guide and Web Tools guide, each capability maps to specific request settings:

- **Web search** : With thinking disabled and the`deepseek_web_search` tool enabled,`deepseek-flash` runs web or image searches as needed and answers with citations. Image search triggers automatically when the user message contains images.
- **Deep thinking search** : With thinking enabled alongside`deepseek_web_search` , the model searches the web and reads links autonomously across multiple reasoning steps, making up to roughly 20 tool calls before answering.
- **URL reading** : Enabling`deepseek_read_url` lets the model detect links in the user prompt, fetch those pages, and include them in context when answering.
- **File input** : Uploading PDFs, Office documents, images, and other files through the Files API parses them into context using either`text_recognition` (plain text only, dropping charts and photos) or`rich_layout_recognition` (text plus cropped figures placed at their original positions).
- **Model safety** : Adding`safety_check` screens inputs and outputs and returns a refusal marker when risk is detected.

The chatbot guide also describes optional user metadata markers (passing current date, user location, and language) and automatic conversation title generation via `title`. Client applications must display the returned content blocks and send them back unchanged on the next turn. Because DeepSeek handles search calls and context assembly server-side, this workflow contrasts with traditional client-side tool calling, where your application executes every tool call locally.

## How Do You Enable DeepSeek API Web Search in a Request?

Developers enable DeepSeek web search, URL reading, high-level safety screening, and automatic conversation titles by adding those parameters directly to a `deepseek-flash` JSON request body:

```
{
  "model": "deepseek-flash",
  "tools": [
    { "type": "deepseek_web_search" },
    { "type": "deepseek_read_url" }
  ],
  "safety_check": { "level": "high" },
  "title": { "enabled": true },
  "messages": [
    {
      "role": "user",
      "content": "Summarize the latest pricing updates on https://api-docs.deepseek.com/quick_start/pricing"
    }
  ]
}
```
When a request declares the web tools, sets `safety_check` to `high`, or enables `title`, DeepSeek’s service overrides several sampling parameters and ignores client values such as `temperature` and `top_p`. The `max_tokens` parameter remains active, though it caps each individual inference step rather than the entire multi-step search round.

## Standalone DeepSeek Search, Fetch, OCR, ASR, and TTS APIs

Alongside the integrated chat features, DeepSeek opened four standalone APIs so developers can invoke search, page extraction, OCR, and voice processing outside a chat session.

### Search and Fetch APIs

Described in the Search & Fetch guide, `POST https://api.deepseek.com/v1/tools/search` queries DeepSeek’s web index directly, while `POST https://api.deepseek.com/v1/tools/fetch` retrieves a web page as Markdown. Unlike the in-chat `deepseek_web_search` tool, the standalone Search API does not support image search yet.

### DeepSeek OCR API

Exposed at `POST https://api.deepseek.com/v1/tools/ocr`, the OCR API supports PDF, Word, PowerPoint, and common image formats, returning extracted text page by page. Files sent to the OCR endpoint are neither stored by DeepSeek nor passed into a chat model.

### DeepSeek ASR (Speech to Text)

Available at `POST https://api.deepseek.com/v1/tools/asr`, the ASR API transcribes whole 16-bit PCM WAV or Ogg Opus audio files in a single request, with no streaming recognition. It automatically detects 21 languages, including Chinese, Cantonese, English, Japanese, Korean, German, French, Spanish, Portuguese, Italian, Russian, Arabic, and Hindi.

### DeepSeek TTS (Text to Speech)

Available via `POST https://api.deepseek.com/v1/tools/tts` for full-text synthesis or WebSocket streaming, the TTS API offers two voices: `stella` (female, default) and `tide` (male). It auto-detects 10 languages (Mandarin, Cantonese, English, Japanese, Korean, Malay, Arabic, Indonesian, Thai, and Vietnamese) and outputs `wav` by default or `pcm`.

## What Does DeepSeek API Web Search, OCR, and Voice Cost?

Starting 12:00 on November 18, 2026 (Beijing Time, UTC+8), server-side search and web fetch triggered when calling the model are no longer billed separately and only incur `deepseek-flash` token usage, according to DeepSeek’s official Models & Pricing page. The standalone Search and Fetch APIs, file parsing, OCR, ASR, and TTS remain billed separately in addition to model token fees.

### Tool and standalone API prices

| Item | Price | Unit | Notes | 
|---|---|---|---|
| Search API (text) | $0.75 | Per 1,000 successful requests | Standalone API only; in-chat web tools search and image search are no longer billed | 
| Fetch API | $0.75 | Per 1,000 successful requests | Standalone API only; in-chat URL reading and page fetch are no longer billed | 
| File parsing and OCR API | $0.225 | Per 1,000 pages | Charged per parsed page | 
| Text to Speech (TTS) | $0.45 list / $0.225 limited-time | Per 10,000 characters | 50% off limited-time price | 
| Speech to Text (ASR) | $0.116 list / $0.058 limited-time | Per hour of audio | 50% off limited-time price | 

### deepseek-flash token prices (per 1M tokens)

| Token type | Off-peak | Peak | 
|---|---|---|
| Input (cache hit) | $0.003 | $0.006 | 
| Input (cache miss) | $0.15 | $0.30 | 
| Output | $0.60 | $1.20 | 

`deepseek-flash` supports a 1M token context window and up to 384K output tokens. Off-peak token prices are half of peak rates. Peak hours run Monday through Friday from 01:00 to 04:00 UTC and 06:00 to 10:00 UTC, excluding Chinese public holidays; all other hours and weekends use off-peak rates. DeepSeek has not announced an end date for the 50% TTS and ASR discount and states that prices may vary and are subject to adjustment. For broader token-rate context across providers, see our AI API pricing comparison.

## Which Billing Rules Surprise Developers Most?

DeepSeek’s billing model separates server-side model tools from standalone utility endpoints, uploaded documents, and safety checks:

- **In-chat search and fetch no longer carry separate fees** : Since 12:00 Beijing Time on November 18, 2026, server-side web search, image search, and page fetches triggered inside a model call (including deep thinking search and URL reading) incur only model token usage. The usage object may still report`server_tool_use` counts for searches, fetches, and image searches as usage metrics, without billing them separately.
- **Standalone Search and Fetch bill only on non-empty HTTP 200 responses** : Calling the standalone Search API or Fetch API directly is billed per successful request rather than by tokens or result count (`count` does not affect the per-request price). A request is billable only when it returns HTTP 200 with a non-empty`results` array for Search or page content for Fetch; failed requests, empty results, and timeouts are free.
- **Safety checks bill as output tokens** : Tokens used for safety review are reported as`safety_tokens` and charged at output token rates.
- **File parsing bills once at upload** : Document parsing charges per page once upon successful upload; subsequent chat turns referencing the file pay standard input token rates.
- **Voice billing** : ASR bills audio duration to the second.

## Key Limits, Safety Checks, and Compliance Obligations

DeepSeek enforces strict payload caps, tool restrictions, and compliance caveats across the new endpoints.

### Technical and Concurrency Limits

- **Web tools** :`allowed_domains` ,`blocked_domains` , and`user_location` are not supported on`deepseek_web_search` . Other server tools cannot be combined with`deepseek_web_search` ,`deepseek_read_url` ,`safety_check` , or`title` yet. Deep thinking search stops after about 20 tool calls.
- **File parsing and OCR** : Both cap files at 100 MiB and 1,000 pages. File parsing enforces a 120-second upload limit, a 120-second parsing limit, and 15 concurrent uploads per account; original files and parsed outputs cannot be downloaded. Standalone OCR allows 180 seconds total.
- **ASR and TTS** : ASR limits files to 8 MiB and 120 seconds, with up to 50 concurrent requests per account. TTS caps requests at 10,000 billed characters and does not support speed control.

### Model Safety Checks and Public Service Compliance

By default, the API runs `low` level safety checks. Passing `safety_check` set to `{"level": "high"}` invokes the Model Content Safety review across both inputs (including tool results) and outputs. Violations return `400 Content Exists Risk` or end the response with a refusal (`stop_reason` `"refusal"` in Messages; `finish_reason` `"content_filter"` in Chat Completions). Because already-streamed tokens cannot be withdrawn, clients must replace streamed text with their own refusal message when a refusal arrives.

DeepSeek instructs operators providing public-facing services to enable `safety_check` and fulfill obligations including content safety review, large model registration, and AI labeling. DeepSeek notes that `safety_check` aids compliance without replacing a service provider’s own content review duties.

## When Should You Use In-Chat Web Tools vs. Standalone Search and Fetch?

Choosing between DeepSeek’s in-chat web tools and its standalone Search and Fetch APIs depends on whether you want DeepSeek or your own code controlling retrieval. Use `deepseek_web_search` and `deepseek_read_url` inside `deepseek-flash` when building conversational assistants that benefit from server-managed search orchestration, image search, multi-step deep thinking, and automatic citations.

Use the standalone `/v1/tools/search` and `/v1/tools/fetch` endpoints when building custom retrieval-augmented generation (RAG) pipelines that require your own domain filtering, chunking, or downstream model selection over Markdown page extracts. Standalone OCR, ASR, and TTS similarly fit modular pipelines where files or audio do not belong in chat state. From a cost standpoint, server-side search and fetch triggered inside a model call no longer add separate search or fetch fees, whereas direct calls to the standalone Search and Fetch APIs remain billed per successful request.

## FAQ

### Which DeepSeek models support web search and URL reading?

DeepSeek’s documentation specifies `deepseek-flash` (currently listed as `DeepSeek-V4.1-Flash-1017` on the pricing page) across the Messages, Responses, and Chat Completions endpoints. The guides do not list support for other model IDs, so developers needing server-handled web search, deep thinking search, URL reading, file input, and safety checks should target `deepseek-flash`.

### Can you restrict DeepSeek web search to specific domains?

No. DeepSeek’s Web Tools guide confirms that `allowed_domains`, `blocked_domains`, and `user_location` are not supported on `deepseek_web_search`. If you need strict domain allowlists or blocklists, call the standalone `/v1/tools/search` API, filter the returned URLs in your own backend, and retrieve approved pages using `/v1/tools/fetch`.

### Are failed searches or unreachable URLs billed on the DeepSeek API?

No. Server-side search, image search, and page fetches triggered inside a model call are not billed at all, whether they succeed or fail, leaving only model token usage. On the standalone Search and Fetch APIs, timeouts, failed requests, and empty results incur no charge; only HTTP 200 responses with non-empty payloads are billed at $0.75 per 1,000 requests.

### How long will DeepSeek’s 50% discount on TTS and ASR last?

DeepSeek’s Models & Pricing page lists a limited-time 50% discount lowering TTS to $0.225 per 10,000 characters and ASR to $0.058 per hour of audio, without stating an end date. DeepSeek also states that prices may vary and reserves the right to adjust published API rates.

### Does enabling safety_check satisfy public AI compliance rules by itself?

No. DeepSeek states that operators offering a service to the public must enable `safety_check` and complete obligations covering content safety review, large model registration, and AI labeling. The documentation specifies that the parameter helps with compliance without replacing a service provider’s own content review responsibilities.
