---
id: collect-261121-veille-ia/deepseek/deepseek-put-its-app-inside-the-api-search-files-voice-and-a-bill-with-three-extra-lines
title: "DeepSeek Put Its App Inside the API: Search, Files, Voice — and a Bill With Three Extra Lines"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: []
keywords: ["search", "deepseek", "tool", "model", "first", "conversation", "fees", "deepseek-flash"]
source: https://dev.to/euk_ela_a3e7ed01aa3f7314e/deepseek-put-its-app-inside-the-api-search-files-voice-and-a-bill-with-three-extra-lines-3omn
source_anchor: ""
source_lines: [1, 69]
section: "DeepSeek"
---
On November 13, DeepSeek shipped a quiet but genuinely useful update in its API changelog: the search, link-reading, file-parsing, voice, and safety-checking capabilities that lived in the DeepSeek app are now callable directly from the API. No rebuilding the stack yourself.

The headline first: the update is not a new model. The model is still deepseek-flash (DeepSeek-V4.1-Flash-1017). What changed is how you call it — the "chat experience" has been broken down into billable building blocks.

Two layers.

The first layer is capabilities integrated into the conversation itself, available in the Messages, Responses, and Chat Completions APIs, switched on with parameters:

- 
**Web search** : the`deepseek_web_search` tool. With thinking mode off, the model runs web or image search on demand and answers with citations. With thinking on, it can search and read links repeatedly across multiple reasoning rounds.
- 
**Read URLs** : the`deepseek_read_url` tool pulls in links that appear in the message and reads them.
- 
**File input** : upload PDFs, Word docs, PPTs, and images; they are parsed into context before you ask. Parsing comes in two tiers — plain text extraction, or rich-layout recognition that preserves chart positions.
- 
**Safety checks** : a`safety_check` parameter that screens inputs and outputs, returning a refusal flag when something is risky.
- 
**Auto titles and user-context metadata** : the app's automatic conversation titles and context memory are in the API too.

The second layer is five standalone APIs, billed per call and not tied to a conversation: Search, Fetch, OCR, ASR, and TTS.

DeepSeek also open-sourced a demo chat client on GitHub, which can import your exported DeepSeek chat history — an official "copy this" reference implementation.

The easiest mistake here is the pricing. Model tokens and tool calls are billed separately, and tool fees stack on top of token fees.

| Capability | Price (DeepSeek pricing page, checked Nov 14) | 
|---|---|
| Search | $0.75 per 1,000 successful requests | 
| Fetch | $0.75 per 1,000 successful requests | 
| File parsing / OCR | $0.225 per 1,000 pages | 
| Speech-to-text (ASR) | 50% off for a limited time (no end date published) | 
| Text-to-speech (TTS) | $0.45 per 10,000 characters, $0.225 for a limited time | 

The model itself, deepseek-flash: $0.30 input / $1.20 output per million tokens at peak hours, half price off-peak ($0.15 / $0.60).

A quick worked example: one conversation with 5 searches and a 10-page PDF parsed costs about 5 × $0.00075 + 10 × $0.000225 = $0.006 in tool fees — under a cent. Search is the line item that scales: if your agent searches dozens of times per conversation, tool fees can overtake token fees over a month. Test on small traffic first, then scale.

Chat Completions API — turning on web search is one tool parameter:


```
{
  "model": "deepseek-flash",
  "messages": [
    {"role": "user", "content": "What changed in DeepSeek's November 13 API update?"}
  ],
  "tools": [
    {"type": "deepseek_web_search"}
  ]
}
```
Two details worth knowing. First, the built-in web tools require thinking mode to be off; with thinking on, the model takes a different multi-step search path, and the behavior is not the same — don't mix them up. Second, files go through a two-step upload-then-parse flow: you get a file_id first, then reference it in the conversation, and the parsing tier is chosen at upload time — it can't be changed later.

1. 
**Only deepseek-flash.** The built-in web and file tools work on deepseek-flash only. deepseek-v4-pro can't use them — don't try it on Pro.
2. 
**The standalone Search/Fetch APIs are not new.** They have existed since October 17. What's new this time is the in-conversation integration. Don't write this up as "DeepSeek launches search API for the first time."
3. 
**Tool fees are separate, and billed per successful request.** Failed searches don't cost anything, but the definition of "successful" follows DeepSeek's billing page — check your invoice breakdown before going live.
4. 
**I haven't tested this hands-on.** The facts above come from DeepSeek's official changelog and pricing page (checked November 14), cross-checked against two independent write-ups. Run the demo client end to end before putting it in production.

Three labs in the same week are all turning "capabilities" into billable building blocks: Google shipped its Decision 1 decision model on November 11, Anthropic moved Claude into the meeting room, and DeepSeek took its app apart into an API. The gap between models is narrowing; the gap is moving to whose blocks are cheaper and easier to snap together.

The yardstick for developers is changing too. It used to be benchmark scores. Now it's the total bill for one complete conversation. Search, parsing, and voice each have their own price list — only added together do they show the real cost.

Would you build your own chat client on this DeepSeek stack, or wait for a hosted version? Let me know in the comments.

## Top comments (0)
