---
id: collect-261121-veille-ia/deepseek/deepseek-api-web-search-ocr-and-voice-pricing-0-75-per-1-000-searches
title: "DeepSeek API web search, OCR and voice pricing: $0.75 per 1,000 searches"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: []
keywords: ["deepseek", "search", "tool", "searches", "text", "model", "pricing", "page"]
source: https://www.datastudios.org/post/deepseek-api-web-search-ocr-voice-0-75-per-1000-searches
source_anchor: ""
source_lines: [1, 279]
section: "DeepSeek"
---
DeepSeek API adds web search, OCR and voice at $0.75 per 1,000 searches

7 days ago

8 min read

Updated: 7 days ago

DeepSeek has opened the features behind its consumer app to developers through the DeepSeek API. A changelog entry dated November 13, 2026 says the `deepseek-flash` model can now run web search, deep thinking search, URL reading, file input and a content safety check inside the Messages, Responses and Chat Completions APIs, with DeepSeek handling the tool calls and the assembly of context on its own servers. The same capabilities are also sold as standalone endpoints: a Search API, a Fetch API, document parsing with OCR, speech to text (ASR) and text to speech (TTS).

The price list is the part developers will look at first. DeepSeek charges $0.75 per 1,000 searches and $0.75 per 1,000 page fetches, $0.225 per 1,000 pages for file parsing and OCR, and, at limited-time rates, $0.058 per hour of audio for speech recognition and $0.225 per 10,000 characters for speech synthesis. Model tokens are billed separately at the existing `deepseek-flash` rates, which on DeepSeek's pricing page start at $0.15 per 1M input tokens on a cache miss and $0.60 per 1M output tokens off-peak.

For comparison, Anthropic lists its Claude web search tool at $10 per 1,000 searches and OpenAI lists its web search tool at $10 per 1,000 calls, both plus token costs. On a per-search basis, DeepSeek's built-in search is priced at about one thirteenth of those two tools. All prices in the tables below are as listed on the providers' official pricing pages on November 13, 2026.

··········


DEEPSEEK API APPLICATION FEATURES: WHAT DEEPSEEK-FLASH CAN NOW DO.

The new features are switched on with existing API parameters. Web search uses a server tool called `deepseek_web_search`. With thinking disabled, the model searches when it needs to and answers with citations. With thinking enabled, the same tool becomes what DeepSeek calls deep thinking search, in which the model can run up to about 20 tool calls before answering, alternating between searching, opening pages and finding text inside an opened page. When a user message contains images, the tool can also run image searches.

URL reading is a second server tool, `deepseek_read_url`. The model scans only the last user message, considers the first 10 distinct links and decides which ones to read. Declaring this tool alone lets an application read links without allowing open web searches. DeepSeek's documentation says the page text is not returned to the client in readable form and travels encrypted in a `signature` field, which the client must send back unchanged in the next turn for the page to be restored into context.

File input works through the Files API. Developers upload a document with one of two parsing purposes: `text_recognition`, which extracts text and drops charts and photos, or `rich_layout_recognition`, which keeps figures, crops them and places them at their original positions in the text. Parsing covers PDF, Word, PowerPoint and common image formats. Plain text, spreadsheets and ebooks are converted to Markdown on upload without parsing and are not billed.

The content safety check is a request parameter, `"safety_check": {"level": "high"}`, which reviews both the input, including tool results, and the model's output. When the review fails, the request returns an error or the response ends with a refusal marker. DeepSeek says public-facing services should enable it, and adds that it provides only basic risk control and does not replace the provider's own obligations on content review, model registration and AI labeling.

DeepSeek also published an open-source demo chat client built on these APIs. According to the documentation, it stores API keys and conversations only in the browser and can import a user's DeepSeek chat history.

··········

DEEPSEEK API PRICING FOR SEARCH, FETCH, OCR, ASR AND TTS.

The tool prices are new line items on DeepSeek's Models & Pricing page. Before this release the API offered no built-in web search, URL reading, document parsing or voice services, so developers who wanted these features had to connect separate providers themselves.

........

Service

Unit

Before November 13

Price now

Web search (text, image)

1,000 searches

Not offered

$0.75

Web fetch / URL reading

1,000 fetches

Not offered

$0.75

File parsing and OCR

1,000 pages

Not offered

$0.225

Speech to text (ASR)

1 hour of audio

Not offered

$0.116 list, $0.058 limited-time

Text to speech (TTS)

10,000 characters

Not offered

$0.45 list, $0.225 limited-time

Model content safety

Review tokens

Not offered

Billed as output tokens

........

DeepSeek does not say how long the limited-time ASR and TTS prices will last. The billing rules for search are specific. A search is billed per query only when it returns results, so a single call carrying three queries counts as three searches, while failed or empty searches are free. Image searches count one unit per image plus one per region the model selects. URL reading is billed per successfully fetched page, and pages that cannot be fetched are free. During deep thinking search, reopening a page, scrolling within it or opening a page whose content already came with the search results costs nothing, and finding text in a page is also free. Safety review tokens are counted as output tokens and reported separately as `safety_tokens`.

The model token prices themselves are unchanged. DeepSeek applies peak and off-peak rates, with off-peak at half the peak price. Peak hours are 01:00 to 04:00 and 06:00 to 10:00 UTC, Monday through Friday, excluding Chinese public holidays.

........

Model

Cache-hit input

Cache-miss input

Output

Context / max output

deepseek-flash, off-peak

$0.003

$0.15

$0.60

1M / 384K

deepseek-flash, peak

$0.006

$0.30

$1.20

1M / 384K

deepseek-v4-pro, off-peak

$0.022

$0.66

$1.98

1M / 384K

deepseek-v4-pro, peak

$0.044

$1.32

$3.96

1M / 384K

........

Prices are in USD per 1M tokens. DeepSeek lists no batch tier and no separate cache-write price; its usage reports show `cache_creation_input_tokens` as always zero. The new web tools and file input work only with `deepseek-flash`, which is currently served by DeepSeek-V4.1-Flash-1017. The documentation does not list `deepseek-v4-pro` as a supported model for them. Readers who want the full background on DeepSeek's token billing can see our guide to DeepSeek API pricing, granted balance and peak rates.

··········

DEEPSEEK WEB SEARCH PRICE COMPARED WITH CLAUDE, OPENAI AND GEMINI.

The three largest Western API providers all sell a search tool, and the official pricing pages show a wide gap.

........

Provider

Search tool

Price

Token costs

DeepSeek

deepseek_web_search

$0.75 per 1,000 searches

deepseek-flash rates

Anthropic

Claude web search tool

$10 per 1,000 searches

Standard token costs

OpenAI

Web search (incl. images)

$10 per 1,000 calls

Search content tokens at model rates

Google

Grounding with Google Search, Gemini 3 models

5,000 prompts free per month, then $14 per 1,000 queries

Standard token costs

........

The comparison is not exactly like-for-like. DeepSeek bills per query, Anthropic counts each search as one use regardless of how many results come back, and OpenAI bills per call. Fetching pages differs too: Anthropic does not charge a fee for its web fetch tool beyond tokens, while DeepSeek charges $0.75 per 1,000 fetched pages. Search quality, index coverage and citation behavior are also outside what a price table can show, and DeepSeek has not published independent search-quality benchmarks with this release.

··········

[[ADPLUS_300x250_1]]

WHAT A SEARCH CHATBOT COSTS ON THE DEEPSEEK API: A WORKED EXAMPLE.

To make the prices concrete, consider a research assistant that handles 10,000 conversations a day. Each conversation triggers two search queries and reads one web page, sends 6,000 input tokens to the model with 30% of them served from cache, and produces 800 output tokens. The figures below use DeepSeek's listed rates and cover 30 days.

........

Cost item

Off-peak

Peak

Model tokens per day

$11.15

$22.31

20,000 searches per day

$15.00

$15.00

10,000 page fetches per day

$7.50

$7.50

Total per day

$33.65

$44.81

Total per 30 days

$1,009.62

$1,344.24

........

The token line comes from 4,200 cache-miss input tokens, 1,800 cache-hit input tokens and 800 output tokens per conversation, which costs $0.0011154 at off-peak rates and twice that at peak. Tool fees do not change with the time of day. With the same 20,000 daily searches, the search fee alone on Anthropic's or OpenAI's tool would be $200 a day, or $6,000 over 30 days, before any token costs. On DeepSeek the search fee for the same volume is $450 over 30 days.

Document and voice workloads are cheap on the new price list. Parsing 10,000 PDF pages costs $2.25. Transcribing 100 hours of audio costs $5.80 at the limited-time rate and $11.60 at list price, and synthesizing 1 million characters of speech costs $22.50 at the limited-time rate and $45 at list. The arithmetic favors high-volume consumer chat, search-heavy agents and document pipelines, where tool calls rather than tokens tend to dominate the bill.

··········


HOW DEVELOPERS ENABLE DEEPSEEK WEB SEARCH AND URL READING.

DeepSeek's chatbot documentation gives a Messages API example that turns on thinking, both web tools, the safety check and automatic conversation titles in one request.

Several constraints apply. `deepseek_web_search` can be declared only once and cannot be combined with other server tools apart from `safety_check` and `title`. The `allowed_domains`, `blocked_domains` and `user_location` options are rejected, so developers cannot restrict results to particular sites through the built-in tool. `tool_choice` cannot force either web tool, and a reply that used web tools cannot be continued with a partial assistant message. When the web tools, the high safety level or titles are active, the service ignores sampling parameters such as `temperature` and `top_p`, and `max_tokens` limits each inference step rather than the whole round.

The standalone endpoints are simpler. The Search API at `POST /v1/tools/search` accepts a query of up to 128 characters, returns up to 50 results with relevance and authority scores, and supports operators such as `site:`, `lang:`, `after:` and `before:`. The Fetch API at `POST /v1/tools/fetch` returns a page as Markdown. DeepSeek sets account-level limits of 100 queries per second for search and 25 for fetch, separate from model concurrency limits. For document parsing, files can be up to 100 MiB and 1,000 pages, with up to 20 documents per request, which is well above the 64 MiB image limit described in our article on the DeepSeek Files API.

For people who use the free DeepSeek app rather than the API, nothing changes: the app keeps its existing features, and the new prices apply only to API usage drawn from a topped-up or granted balance. The consumer side is covered in our guide to what is free and what is paid on DeepSeek.

··········

DEEPSEEK APPLICATION APIS: CONFIRMED PRICES AND OPEN POINTS.

The release is documented in detail on DeepSeek's own pages. The changelog date, the supported APIs, the `deepseek-flash` restriction, the tool parameters, the per-unit prices and the billing rules are all published, and the search price of $0.75 per 1,000 queries undercuts the $10 listed by Anthropic and OpenAI by a factor of about 13.

Some points remain open. DeepSeek has not said when the limited-time ASR and TTS prices end, has not published a separate price for the safety review beyond counting it as output tokens, and has not released search-quality or OCR-accuracy figures that would let developers compare results rather than prices. The built-in search also gives developers less control than the standalone Search API, since domain filters and location settings are rejected inside the model tool. Teams that need those controls can call the Search API directly at the same $0.75 rate and pass the results to any model they choose.
