---
id: collect-261121-veille-ia/deepseek/deepseek-api-adds-search-ocr-and-speech-120-second-audio-cap-vendor-bound-by-chinese-laws
title: "DeepSeek API Adds Search, OCR and Speech: 120-Second Audio Cap, Vendor Bound by Chinese Laws"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: []
keywords: ["search", "deepseek", "tool", "model", "audio", "speech", "text", "vendor"]
source: https://techtimes.com/articles/330789/20261116/deepseek-api-adds-search-ocr-speech-120-second-audio-cap-vendor-bound-chinese-laws.htm
source_anchor: ""
source_lines: [1, 93]
section: "DeepSeek"
---
DeepSeek's Nov. 13 release lets developers send audio, documents, web links and search queries through one Chinese vendor, but its speech recognition service accepts only 120 seconds of audio per request, its file parser can miss charts and diagrams, and every call goes to a Hangzhou-based company subject to China's state-security laws. Developers weighing the convenience of a single vendor need those three conditions in hand before they compare per-unit prices.

The changelog entry dated Nov. 13 lists web search, URL reading, file input, a safety check setting, and standalone search, fetch, OCR, speech recognition (ASR) and speech synthesis (TTS) services, plus an open-source demo chat client that can import a user's DeepSeek chat history. Korean outlet TokenPost reported the update on Nov. 14. The chat features run on deepseek-flash across DeepSeek's Messages, Responses and Chat Completions APIs.

Teams building agents or retrieval-augmented generation (RAG) apps can now get live search, document parsing and voice from one provider. That consolidation also puts audio, uploaded files, fetched web pages and search queries on one data path under one legal jurisdiction, a concentration the changelog does not discuss. The sections below cover the decision in four parts: what the tools cannot do, how much outside verification exists, what operating them involves, and which legal conditions travel with the data.

### Server-Side Tool Calling Moves Search and Fetch Loops Onto DeepSeek's Servers

DeepSeek's servers handle the tool calling and context assembly, according to TokenPost. Developers display the content blocks the API returns and pass them back in the next request instead of orchestrating each search or fetch themselves. With thinking disabled, the model decides when to run web or image searches and answers with citations; with thinking enabled, the same tool lets it search and read links across several reasoning steps. A separate URL-reading tool lets the model spot links in a user's message and fetch those pages, and TokenPost notes that this is treated as a different tool from opening results found through search.

The tradeoff is visibility. Because the loop runs on DeepSeek's side, a developer sees what the model searched and read after the turn finishes rather than approving each call, and each successful call is billed. That matters for security. In indirect prompt injection, hidden instructions in a web page, email or document that a model reads can steer its behavior, and the UK's National Cyber Security Centre has said there is no surefire fix. DeepSeek's safety guide says the high setting reviews model input and output, including tool results, but it does not describe the check as a defense against prompt injection.

The search engine behind the tool scores every result. A rerank score from 0 to 3 combines relevance, authority and freshness, and an authority tier from 0 to 4 rates the host. DeepSeek says its engine filters SEO spam, manipulation aimed at generative search engines and low-quality AI-generated text. The same changelog claims hundreds of billions of pages in the index, a figure DeepSeek does not substantiate, and the search guide itself gives no index size.

File input follows a similar pattern. Files are uploaded first, parsed, and then referenced by a file ID in a chat message. Text recognition mode extracts text only and drops charts, photos and diagrams. Rich layout recognition crops figures and places them in the text at their original positions so the model can interpret them, and DeepSeek warns that figures are detected automatically and not every figure is guaranteed to be found. The model cannot see a figure that was not cropped, and cropped figures consume input tokens. Plain text files skip parsing, while spreadsheets and e-books are converted to Markdown during upload.

The standalone OCR service returns text page by page and embeds position coordinates, normalized to a 0-to-1,000 grid with the origin at the top-left corner of the page. The guide describes no switch for turning the coordinates off. It accepts PDFs, Word and PowerPoint files and common image formats of 100 MiB and 1,000 pages, and upload, recognition and result must finish within 180 seconds.

### Speech Limits and Figure Detection Gaps Show Where the New Tools Break

Speech recognition is the tightest constraint. It accepts 16-bit PCM WAV or Ogg Opus files of up to 120 seconds and 8 MiB, detects the language automatically across 21 listed languages, and does not support streaming, so a client must send the whole file and wait for the complete transcript. For stereo files, only the left channel is recognized, which matters for call recordings that put each speaker on a separate channel. Longer recordings must be split before upload.

Speech synthesis offers two voices, stella and tide, and 10 listed languages. HTTP requests return WAV or raw PCM, WebSocket sessions return PCM or Opus, and each request is capped at 10,000 billed characters, with each Chinese character counting as two. The guide does not mention voice cloning, and the pages for OCR, speech recognition and speech synthesis name no underlying model.

| Tool | Documented limits | Practical effect | 
|---|---|---|
| Speech recognition guide | 120 seconds, 8 MiB, no streaming, 21 languages, left channel only for stereo | Long recordings need client-side splitting | 
| Speech synthesis guide | Two voices, 10 languages, 10,000 billed characters per request | Long scripts need chunking | 
| OCR guide limits | 100 MiB, 1,000 pages, 180 seconds end to end, 15 concurrent recognitions per account | Large archives need batching | 
| Document parsing guide | 20 documents and 8 MiB of combined text per request, 15 concurrent uploads, 120-second upload and parse windows | Figures can be missed in rich layout mode | 
| Search guide limits | Search 100 queries per second and fetch 25 per account, 1 to 50 search results per request | Fetch also has a per-domain limit DeepSeek does not quantify | 

Parsed documents also stay in context across turns. If the context grows too long, the request returns an error instead of silently truncating the file, and every chat request that references the document is billed for its input tokens again.

### What Independent Testing Exists for DeepSeek's New Tools?

One outside test of the search tool turned up. Suganthan Mohanadasan ran 110 calls from Dubai for about $0.08 and published the results Oct. 20 on Search Engine Journal. He found that authority behaved at host level rather than per page, that spam uploads on government and university hosts earned the top authority tier, and that a gambling page on a former politician's domain ranked near the top for a casino query. Date filters also removed about 11% of results the system could not date. The test covered one day, one region and six queries, and the author calls the findings directional.

Nothing comparable surfaced for OCR, speech recognition, speech synthesis or file parsing, and no head-to-head comparison with Western search, OCR or speech services turned up either. DeepSeek's pages publish no accuracy figures for the new tools. A paper with code in DeepSeek's GitHub organization described an open-weight OCR model in October 2025, but the API page does not say the service runs it, so its reported results cannot be assumed to describe the API. Retrieval quality is also the weak point of any RAG system, because it shifts risk to the retrieval layer, where poorly sourced or outdated documents can produce confident answers that look sourced. Treat DeepSeek's capability claims for the new tools as unverified until a named third party tests them on your own audio and documents.

### Fetch Requests Carried No DeepSeek Identity in One Test, and Tool Fees Stack on Token Costs

In Mohanadasan's test, DeepSeek's fetch tool presented itself as a Firefox browser from a Hong Kong cloud network, with a Chinese language header and no DeepSeek identifier. It returned content from disallowed URLs listed in robots.txt, and he could not confirm whether it checks robots.txt at all. The robots.txt convention is voluntary, and the Internet Engineering Task Force formalized it as RFC 9309 in 2022, so publishers who rely on it have no enforcement if a tool ignores it. Site owners trying to spot these requests must rely on log patterns, which the tester cautions could match other traffic.

Tool fees are billed separately from tokens. Search costs $0.75 per 1,000 searches and web fetch costs $0.75 per 1,000 fetches, file parsing and OCR cost $0.225 per 1,000 pages, speech synthesis costs $0.45 per 10,000 characters, and speech recognition costs $0.116 per hour of audio. The speech prices are halved, to $0.225 and $0.058, for a limited time, and the pricing page gives no end date. TokenPost converted the search rate to 5 yuan (approximately $0.75 USD) per 1,000 calls and file analysis and OCR to 1.5 yuan (approximately $0.22 USD) per 1,000 pages, charged only when parsing succeeds.

An agent that needs page content usually calls both tools, because search returns URLs and snippets while fetch returns the page, so one useful lookup costs $1.50 per 1,000 rather than $0.75, as one price analysis noted. Billing for server-side tool calls made through DeepSeek Harness, Claude Code and Codex began at 00:00 Beijing time on Nov. 1, which was noon ET on Oct. 31.

Model tokens are billed on top. The deepseek-flash model costs $0.15 per million cache-miss input tokens and $0.60 per million output tokens off-peak, and both double at peak. Peak runs on weekdays from 01:00 to 04:00 and 06:00 to 10:00 UTC, excluding Chinese public holidays, which is 8 p.m. to 11 p.m. ET and 1 a.m. to 5 a.m. ET now that daylight saving time has ended. Re-referencing a file in later turns costs input tokens each time, though the parsing fee is not charged again.

### Chinese Security Laws Can Require DeepSeek to Cooperate, Whatever Its Privacy Policy Says

DeepSeek is a Hangzhou-based company that was spun out of High-Flyer, a Chinese hedge fund, in 2023. Three Chinese laws bear on the data it holds. Article 7 of the National Intelligence Law (2017) says all organizations and citizens shall support, assist and cooperate with national intelligence efforts in accordance with law, and Article 14 lets intelligence institutions request that support. Article 35 of the Data Security Law (2021) obliges organizations to cooperate when public security or national security organs need data for national security or criminal investigations, subject to approval procedures. Article 28 of the Cybersecurity Law requires network operators to provide technical support and assistance to those organs.

Legal analysts dispute how far Article 7 reaches. A China Law Translate analysis argues that the article has no clear enforcement mechanism and that it is far from clear it was meant to require active participation in information gathering, though it cautions that direct requests from security agencies would be hard to refuse. Debates about overseas subsidiaries are separate from this case: DeepSeek is itself a Chinese organization, so the legal exposure attaches to the organization regardless of what its privacy policy says, even if how often these provisions are invoked in practice is unknown.

DeepSeek's privacy policy, last updated Feb. 10, 2026, says it stores personal data in the People's Republic of China. It lists text, voice and other prompts, uploaded files and photos among the inputs it collects, and it says inputs may be used to train its models, with an opt-out right. It also says DeepSeek may share data with authorities when it believes law or a government request requires it. The policy states that it does not cover end-user data from third-party apps built on the open platform, leaving those disclosures to developers. On the tool pages, the speech recognition, speech synthesis and search guides make no statement about retention, and the OCR guide says the file is not stored in the Files API or passed to a chat model but gives no retention period.

No independent security audit of the new tools turned up. In a prior incident, Wiz Research reported on Jan. 29, 2025 that an unauthenticated DeepSeek database exposed more than a million log entries, and DeepSeek secured it promptly after disclosure.

Readers can limit exposure but not remove the legal condition. Keep sensitive audio, contracts and regulated records out of the hosted tools, strip personal data before sending files to OCR or speech recognition, log what each agent sends, and send only what you would accept a Chinese authority requesting. The documentation reviewed for this article describes these tools only as hosted services, with no self-hosted option, and no setting in them changes who the obligation binds.

### Safety Check Does Not Transfer Compliance Duties, and Refusals Arrive After Text Streams

Setting the safety check to high turns on what DeepSeek calls basic risk control; the low default applies only standard content checks. A flagged input returns an HTTP 400 error. A flagged output ends in a refusal signal: a stop reason of refusal in the Messages API and content filter in the other two formats. The connection stays open while the full output is checked, so text may already have streamed before a refusal arrives, and DeepSeek tells clients to remove the displayed text and show their own message. Tokens used before a refusal are billed, safety review tokens are billed as output tokens, and a flagged input is still billed at the cache-hit price. The response does not state a specific reason for the refusal, so a developer cannot tell from the response alone whether the check over-blocked or caught a real risk.

DeepSeek says the feature does not replace the developer's own obligations. The guide says developers bear primary responsibility for content safety review and advises public-facing services to handle obligations such as large-model registration and AI labeling themselves. The guide does not name a country, but those three duties line up with China's rules. The Interim Measures for the Management of Generative AI Services took effect Aug. 15, 2023 and apply to services offered to the Chinese public, including by foreign providers, and 2025 labeling measures require visible labels and provenance metadata for AI-generated text, images, audio and video. Developers whose products reach users in mainland China should confirm those duties with counsel; this article is not legal advice.

### What Should Developers Send to DeepSeek's New Tools?

Public web pages, public documents and text meant for public speech synthesis carry the least legal exposure, and they are where a team can test the limits above on its own material before committing. Contracts, customer calls, medical or financial records and internal links belong on a separate decision path, because they are the inputs that Chinese law, DeepSeek's privacy policy and the missing retention terms bear on most directly.

DeepSeek's new tools consolidate search, parsing, OCR and voice behind one key at low per-unit prices, but four conditions that price does not resolve govern the decision. The limits are real: 120-second audio with no streaming, figure detection that can miss charts, and per-account caps on every service. Verification is thin: no named models behind OCR or voice, no published accuracy figures, and one directional outside test of search that found spam uploads earning top authority scores. Operations add friction: a fetch tool that showed no DeepSeek identifier in one test, tool fees that stack on token costs and peak-hour doubling, and a safety check that leaves compliance duties with the developer. And three Chinese laws can bind DeepSeek as an organization, with no retention terms on the tool pages and no independent audit of the new services found.

Currency conversions are approximate, based on the exchange rate as of Nov. 15, 2026.

## Frequently Asked Questions

### Does using DeepSeek's all-in-one tools put every kind of data under one legal regime?

For everything sent through the hosted services, yes. Search queries, fetched URLs, uploaded files, OCR input and audio all go to the same Hangzhou-based operator, which is subject to the National Intelligence Law, the Data Security Law and the Cybersecurity Law, and the tool pages state no retention periods for audio. One way to keep different kinds of data under different legal conditions is to split workloads across vendors.

### Can a web page hijack an agent built on DeepSeek's URL reading?

It is a recognized risk for any model that reads web pages or files. Hidden text in a page or document can carry instructions the model may follow, and DeepSeek describes no specific defense for its URL-reading tool. Reasonable safeguards include treating retrieved content as data rather than instructions, limiting tool access, and requiring human approval for sensitive actions.

### Can DeepSeek's fetch tool be blocked with robots.txt?

Not reliably. One tester saw the tool retrieve pages that robots.txt disallowed and found no DeepSeek identifier in its requests, and robots.txt works only when a bot chooses to honor it. Publishers who need enforcement must block at the server level, which requires a way to recognize the requests.

### Does turning on the safety check make an app compliant with Chinese AI rules?

No. DeepSeek says the check does not replace the developer's obligations for content review, large-model registration and AI labeling. Developers serving users in mainland China should confirm their duties with counsel.
