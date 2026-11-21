---
id: collect-261121-veille-ia/deepseek/deepseek-api-tools-pricing-search-ocr-voice
title: "DeepSeek API Tools Pricing: Search, OCR, Voice"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: ["2026-11-13"]
keywords: ["deepseek", "search", "tools", "chat", "model", "thinking", "fetch", "tool"]
source: https://metarix.io/deepseek-api-tools-pricing
source_anchor: ""
source_lines: [1, 184]
section: "DeepSeek"
---
## On this page

On November 13, 2026, DeepSeek opened the features behind its consumer app to developers: web search, deep thinking search, URL reading, file input and safety checks now run server-side inside the DeepSeek API, and Search, Fetch, OCR, speech-to-text and text-to-speech are sold as standalone endpoints. Each tool is billed per use on top of tokens, and the rates are low: $0.75 per 1,000 searches, $0.225 per 1,000 OCR pages.

That makes DeepSeek the cheapest way to give a chatbot live web answers and document reading from a single vendor, by a wide margin on list price. It also comes with constraints you should know before you wire it into an agent. Here are the prices, the billing rules, how it compares with Claude, OpenAI and Gemini, and where it fits.

## Quick verdict

If you are building a chat product or research assistant on DeepSeek V4.1 Flash, turn the built-in tools on: you drop a separate search vendor, an OCR service and a speech vendor, and web search costs about a thirteenth of what Anthropic and OpenAI list for theirs. If you run a multi-tool agent, need domain allow-lists, or must keep data with a US or EU provider, treat this as a cheap standalone Search, Fetch and OCR layer to test, not a drop-in replacement for your current stack.

Scroll the table horizontally on smaller screens.

| DeepSeek full-suite API release at a glance |  | 
|---|---|
| Item | Detail | 
|---|---|
| Released | Nov 13, 2026 (DeepSeek API change log) | 
| Model | deepseek-flash (DeepSeek-V4.1-Flash-1017); not deepseek-v4-pro | 
| APIs | Messages (Anthropic format), Responses and Chat Completions | 
| Built-in tools | deepseek_web_search (web, image and deep thinking search), deepseek_read_url, file input, safety_check | 
| Standalone endpoints | Search, Fetch, OCR (/v1/tools/ocr), ASR (/v1/tools/asr), TTS (/v1/tools/tts) | 
| Tool pricing | Search and fetch $0.75 per 1K; OCR $0.225 per 1K pages; ASR $0.058/hour and TTS $0.225 per 10K characters at limited-time 50% off | 
| Extras | Open-source demo chat client that imports DeepSeek Chat history | 

## What DeepSeek shipped

DeepSeek's release note frames it as "the full DeepSeek App experience, now via API". In practice there are two halves. The first is a set of server tools on the deepseek-flash model: you declare them in a normal request, the model decides when to search or read a link, and DeepSeek's servers run the calls and assemble the context. Your client never executes a tool. The second half is a set of plain REST endpoints for the same capabilities, so you can call search, page fetch, OCR and voice from your own pipeline without a chat model in the loop.

DeepSeek also published an open-source demo chat client built only on these APIs. It keeps your API key and conversations in the browser and can import history exported from DeepSeek Chat, so it doubles as a reference implementation for streaming, citations and voice.

### The five built-in capabilities

- **Web search:** with thinking off and deepseek_web_search declared, the model searches the web or by image and answers with citations.
- **Deep thinking search:** with thinking on, the same tool becomes a multi-step research loop that searches, opens pages and finds text in them, making up to about 20 tool calls before it answers.
- **URL reading:** with deepseek_read_url declared, the model fetches links it finds in the user's message.
- **File input:** PDFs, Office files and images are parsed and added to the conversation context.
- **Safety checks:** a safety_check parameter screens input and output and returns a refusal marker when it detects a risk. DeepSeek says public-facing apps must enable it.

### What a request looks like

This is the shape of a Chat Completions request with search and URL reading turned on, adapted from DeepSeek's Web Tools guide. It is illustrative; we did not run it for this article.

`curl https://api.deepseek.com/chat/completions \`  `-H "Authorization: Bearer $DEEPSEEK_API_KEY" \`  `-H "Content-Type: application/json" \`  `-d '{`    `"model": "deepseek-flash",`    `"thinking": {"type": "enabled"},`    `"tools": [{"type": "deepseek_web_search"}, {"type": "deepseek_read_url"}],`    `"safety_check": {"level": "high"},`    `"messages": [{"role": "user", "content": "What changed in the DeepSeek API this week?"}]`  `}'`

Two details matter for multi-turn apps. You must send the full history every time and pass previous tool blocks back unchanged, because page text is carried in an encrypted signature field rather than returned to you. And search results and opened pages from earlier turns are not re-added to later context; only the answer text is.

## DeepSeek API tools pricing

All prices come from DeepSeek's Models & Pricing page. Prices as of Nov 14, 2026. Tool fees are added to normal token charges and deducted from your prepaid or granted balance. DeepSeek notes that it may adjust prices.

Scroll the table horizontally on smaller screens.

| DeepSeek tool API prices |  |  |  | 
|---|---|---|---|
| Tool | Unit | Price | Notes | 
|---|---|---|---|
| Search (text, image) | 1,000 searches | $0.75 | Same rate via the Search API or the built-in tool | 
| Web fetch / URL reading | 1,000 fetches | $0.75 | Only successful fetches are billed | 
| File parsing, OCR | 1,000 pages | $0.225 | OCR: up to 1,000 pages and 100 MiB per file | 
| Speech to text (ASR) | 1 hour of audio | $0.058 (list $0.116) | Limited-time 50% off; billed per second | 
| Text to speech (TTS) | 10,000 characters | $0.225 (list $0.45) | Limited-time 50% off | 

### The model tokens underneath

Tokens are billed at deepseek-flash rates, which are among the lowest of any frontier-class API. Off-peak rates are half the peak rates. Peak hours are 01:00 to 04:00 and 06:00 to 10:00 UTC on weekdays, excluding Chinese public holidays; weekends are off-peak all day.

Scroll the table horizontally on smaller screens.

| deepseek-flash token prices per 1M tokens |  |  | 
|---|---|---|
| Price | Off-peak | Peak | 
|---|---|---|
| Input, cache hit | $0.003 | $0.006 | 
| Input, cache miss | $0.15 | $0.30 | 
| Output | $0.60 | $1.20 | 

### Billing rules that change the math

- **Search is billed per query, not per call.** A search call that returns results and carries three queries counts as three searches. Calls that fail or return nothing are free.
- **Image search** counts one search per image plus one for each region the model selects in it.
- **Fetches are billed per page fetched.** Links that cannot be fetched are free. In deep thinking search, reopening a page, scrolling within it, finding text in it, or opening a page whose content came with the search results costs nothing.
- **Failures mid-answer still bill.** If generation breaks after the response started, the searches, page reads and inference that already ran are charged as usual.
- **The usage object tells you what you paid for.** The server_tool_use fields tool_search, tool_fetch and tool_image_search are the billed counts; web_search_requests and web_fetch_requests are display counts and are not what you are billed on.

## How it compares with Claude, OpenAI and Gemini

The obvious comparison is with the built-in web search tools of the big three closed-model APIs. All four bill tokens on top of the tool fee, and all four count the units differently, so per-1,000 prices are a guide, not an exact match. Prices as of Nov 14, 2026, from each provider's official pricing page: DeepSeek, Anthropic, OpenAI and Google.

Scroll the table horizontally on smaller screens.

| Built-in web search and fetch pricing by provider |  |  |  |  | 
|---|---|---|---|---|
| Provider | Web search fee | Unit billed | Page fetch | Notable limits | 
|---|---|---|---|---|
| DeepSeek API | $0.75 per 1K | Each query in a call that returns results | $0.75 per 1K pages | No domain filters or user location; deepseek-flash only | 
| Claude API | $10 per 1K | Each search; failed searches free | Web fetch tool: no fee beyond tokens | Results count as input tokens in later turns | 
| OpenAI API | $10 per 1K | Each web search call | No separate fetch line item | Search content tokens billed at model rates | 
| Gemini API | 5K free a month, then $14 per 1K | Each search request | No separate fetch line item | Default cap of 1,500 search requests a day on the Developer API | 

### An illustrative month of tool fees

Take a support or research assistant handling 1,000 chats a day, 30,000 a month, where each chat triggers three searches and two page reads. That is 90,000 searches and 60,000 fetches. The arithmetic below covers tool fees only, assumes each provider runs the same number of searches, and ignores tokens, which differ by model. It is a calculation from list prices, not a test.

Scroll the table horizontally on smaller screens.

| Tool fees: 90,000 searches and 60,000 fetches a month |  |  |  | 
|---|---|---|---|
| Provider | Search fees | Fetch fees | Total tool fees | 
|---|---|---|---|
| DeepSeek API | $67.50 | $45.00 | $112.50 | 
| Claude API | $900.00 | $0 (tokens only) | $900.00 | 
| OpenAI API | $900.00 | n/a | $900.00 | 
| Gemini API | $1,190.00 (after 5K free) | n/a | $1,190.00 | 

Even if DeepSeek's per-query billing doubles your search count compared with a per-call provider, it stays several times cheaper on tool fees. The bigger question is answer quality per dollar. When DeepSeek started charging for its Search API on November 1, our DeepSeek Search API pricing analysis found it was the cheapest paid search API on Artificial Analysis's board, but that its results made the answering model work harder. Cheap searches that need more follow-up calls can erode the gap, which is why we argue for measuring cost per successful task rather than cost per call.

## OCR, speech to text and text to speech

The standalone endpoints are the less flashy half of the launch, but they are the part most teams can adopt without changing models. None of them involve a chat model, and all three use the same DeepSeek API key.

Scroll the table horizontally on smaller screens.

| DeepSeek standalone tool endpoints: limits |  |  |  | 
|---|---|---|---|
| Endpoint | Inputs | Per-request limits | Other details | 
|---|---|---|---|
| OCR, POST /v1/tools/ocr | PDF, DOC/DOCX, PPT/PPTX, PNG, JPEG, WebP, GIF, BMP, TIFF | 100 MiB, 1,000 pages, 180 seconds total | Two modes: text_recognition and rich_layout_recognition; files not stored | 
| ASR, POST /v1/tools/asr | 16-bit PCM WAV or Ogg Opus | 8 MiB and 120 seconds of audio | 21 languages auto-detected; no streaming; stereo uses left channel only; 50 requests in flight | 
| TTS, POST or WebSocket /v1/tools/tts | Text, Markdown stripped by default | 10,000 billed characters; up to 30 minutes of audio | Two voices (stella, tide); 10 languages; WAV, PCM or Opus; no speed control | 

At $0.225 per 1,000 pages, OCR is cheap enough to run over whole archives: 10,000 pages costs about $2.25. Each page comes back as raw model output with a finish_reason, so check for pages marked length, which were cut off. We've added a DeepSeek OCR API page with the details. The voice endpoints are clearly aimed at chat apps: ASR caps clips at two minutes, which suits voice input but not meeting transcription, and TTS can stream over WebSocket while the model is still generating.

## Limits and gotchas before you switch

- **Web tools don't mix with everything.** DeepSeek's error table says declaring another server tool next to deepseek_web_search, deepseek_read_url, safety_check or title returns a 400 ("cannot be used together ... yet"), and the Responses API rejects custom, namespace and tool_search tools. Test your agent's full tool list before you commit.
- **No domain controls.** allowed_domains, blocked_domains and user_location return errors. If you need to restrict sources, as Bedrock now allows (see our Bedrock web search domain filtering piece), use the standalone Search API and filter yourself.
- **You can't force a search.** tool_choice cannot name the web tools; the model decides. tool_choice none turns all of them off.
- **No continuing a reply that used web tools.** Resend the previous request instead.
- **Compliance is on you.** DeepSeek says developers remain responsible for content moderation, applicable model registration and labeling AI-generated content. Review where your data is processed against your own requirements.
- **Promotional voice prices.** The ASR and TTS rates are 50% off for a limited time with no end date, so budget at list price.

## Who should use it

Scroll the table horizontally on smaller screens.

| Fit by use case |  |  | 
|---|---|---|
| You are building | Use DeepSeek's tools? | Why | 
|---|---|---|
| Consumer or internal chatbot with web answers | Yes, test first | Single vendor, citations included, tool fees far below the closed-model APIs | 
| Document Q&A or RAG ingestion | Yes for OCR | $0.225 per 1K pages and no model call required | 
| Voice input for a chat app | Worth a pilot | Cheap at promo rates, but 120-second clips and no streaming ASR | 
| Multi-tool coding or ops agent | Not yet | Server web tools can't be combined freely with other tools | 
| Regulated workloads needing source control | Standalone Search only | No domain filters on the built-in tool | 

## Verdict

DeepSeek has turned its app into a cheap, mostly complete backend for chat products: search with citations, page reading, documents, safety screening and voice, all metered at a fraction of the closed-model rates. For chatbots and research assistants on deepseek-flash, the built-in tools are the easiest cost win of the week. For agents, the restrictions on mixing tools and the lack of domain filters mean the better move today is to adopt the standalone Search API and OCR as components, measure answer quality per dollar against your current search provider, and revisit the built-in tools when DeepSeek lifts the "yet" in its error message.

## Sources

- DeepSeek: The full DeepSeek App experience, now via API (Nov 13, 2026)
- DeepSeek API change log (Nov 13, 2026 entry)
- DeepSeek API: Models & Pricing (accessed Nov 14, 2026)
- DeepSeek API: Web Tools guide and Build a Chatbot overview (accessed Nov 14, 2026)
- DeepSeek API: OCR, Speech to Text and Text to Speech guides (accessed Nov 14, 2026)
- DeepSeek chat demo on GitHub
- DeepSeek on X: full-suite API announcement (Nov 13, 2026)
- Anthropic: Claude API pricing, OpenAI: API pricing and Google: Gemini Developer API pricing (accessed Nov 14, 2026)
- MODEL: DeepSeek puts its chat app's search, file reading and voice into the API (Nov 13, 2026)
- TokenPost: DeepSeek expands developer API with search, file and voice features (Nov 13, 2026)

## Tools covered in this article

## Frequently asked questions

- How much does DeepSeek API web search cost?
- DeepSeek charges $0.75 per 1,000 searches, on top of deepseek-flash token charges, whether you use the built-in deepseek_web_search tool or the standalone Search API. Each query in a search call that returns results counts as one search, and failed or empty searches are free. Web fetch and URL reading cost $0.75 per 1,000 pages fetched.
- How much does the DeepSeek OCR API cost?
- File parsing and OCR cost $0.225 per 1,000 pages as of November 14, 2026. The OCR endpoint accepts PDF, Word, PowerPoint and common image files up to 100 MiB and 1,000 pages per request, returns text page by page, and does not store the file.
- Which DeepSeek models support the built-in tools?
- The built-in web search, deep thinking search, URL reading, file input and safety check features run on deepseek-flash, currently DeepSeek-V4.1-Flash-1017, in the Messages, Responses and Chat Completions APIs. DeepSeek's release does not list deepseek-v4-pro for these tools.
- Is DeepSeek web search cheaper than Claude or OpenAI web search?
- On list price, yes. As of November 14, 2026, Anthropic charges $10 per 1,000 searches for Claude's web search tool and OpenAI charges $10 per 1,000 web search calls, against DeepSeek's $0.75 per 1,000 searches. The providers count units differently and all add token costs, so compare total cost per answered question on your own workload.
