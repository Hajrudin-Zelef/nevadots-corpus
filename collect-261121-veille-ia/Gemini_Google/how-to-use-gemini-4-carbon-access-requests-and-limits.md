---
id: collect-261121-veille-ia/gemini-google/how-to-use-gemini-4-carbon-access-requests-and-limits
title: "How to use Gemini 4 Carbon: access, requests and limits"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["gemini", "carbon", "google", "developers", "model", "reapi", "argon", "access"]
source: https://reapi.ai/resources/how-to-use-gemini-4-carbon
source_anchor: ""
source_lines: [1, 99]
section: "Gemini_Google"
---
Gemini 4 Carbon became available to paying developers on November 10, 2026, two weeks after Google first released it to cybersecurity defenders. Its request model ID is `gemini-4-carbon`, it accepts text, images, video, audio and PDFs, and it returns text.<sup>[1]</sup><sup>[2]</sup> Many guides written in late October still describe it as unavailable, so check the date on anything you copy.

This guide covers how to use Gemini 4 Carbon on reAPI: create a key, choose a group, write a first request in the native Gemini format and handle the output limits that catch people moving from older Gemini models. The Gemini 4 Carbon page includes the playground and current prices for each available group.

**Start on the Gemini 4 Carbon playground** or create a key in API Keys. The examples below describe the native request format; a successful short text call does not verify every tool or parameter. See the API reference for endpoints and billing behavior.<sup>[3]</sup>

- Developers get Gemini 4 Carbon through a paid Gemini API tier; it is not on the free tier. Google AI Ultra subscribers can also use it in the Gemini app, Google Antigravity and AI Mode in Search.<sup>[1]</sup><sup>[4]</sup>
- Moving from Gemini 4 Argon needs only a model ID change, but Carbon spends more thinking tokens per turn, so leave room in the output budget and the client timeout.<sup>[5]</sup>
- `thinkingLevel` is the main control:`low` ,`medium` (default) or`high` . Temperature, top-p and top-k are ignored.<sup>[5]</sup>
- A non-streaming REST call returns at most 32,768 output tokens per hop. Asking for more without a continuation token returns a 400 error.<sup>[6]</sup>
- On reAPI, Gemini 4 Carbon supports native `generateContent` and`streamGenerateContent` requests, plus OpenAI-compatible Chat Completions. One reAPI key works across the groups available for this model.<sup>[3]</sup>

Google's November 10 announcement lists several separate routes. They are different products with different accounts, so an Ultra subscription in the Gemini app is not an API key, and a paid API key does not unlock the consumer app.<sup>[1]</sup>

| Route | Who it is for | What you need | 
|---|---|---|
| Gemini API (Google AI Studio, Android Studio) | Developers | A paid Gemini API tier | 
| Gemini for business, Antigravity, CodeMender | Enterprises | A business account | 
| Gemini Enterprise Agent Platform | Google Cloud projects | A Cloud project; the global endpoint or the `us` /`eu` multi-regions<sup>[7]</sup> | 
| Gemini app, AI Mode in Search, Antigravity | Consumers and individual developers | A Google AI Ultra subscription | 
| Antigravity research access | Scientists and researchers | An accepted application (six months, rolling onboarding) | 
| Fairwind Program | Cybersecurity defenders | An accepted application, for the separate Carbon for cyber model | 
| reAPI | Developers using one account for multiple models | A reAPI account, API key and credit balance <sup>[3]</sup> | 

Free access is the most common question, and the answer is no for the API. Google's pricing page marks every Gemini 4 Carbon line as "Not available" on the free tier.<sup>[4]</sup> The consumer route requires Ultra, not the regular paid plan.

Google positions Argon as the right choice for most software engineering and knowledge work. Carbon is meant for tasks that strain a model's reasoning, such as multi-step debugging across a large codebase or open-ended research, and it uses more tokens than Argon to explore and verify its work.<sup>[1]</sup> Google's migration notes say the same thing in practical terms: keep Argon where you want faster responses and lower token spend per turn.<sup>[5]</sup>

The two models share a request schema, so you can route a hard task to `gemini-4-carbon` and keep everyday traffic on `gemini-4-argon` without maintaining two clients. Google also launched Carbon at the same introductory token price as Argon.<sup>[1]</sup> Since Carbon thinks longer, the same prompt can still cost more on Carbon even at an identical rate per token. Thinking tokens are billed as output.<sup>[4]</sup>

On the Gemini API, Google lists $2 per million input tokens and $10 per million output tokens through November 30, 2026, then $4 and $20 from December 1.<sup>[4]</sup> Cached input is $0.10 per million tokens during the introductory period and $0.20 afterward. These are Google's rates for its own API. For reAPI, use the live group prices on the model page; Google's list prices are not your reAPI quote.

Pick a task whose answer you can check, such as a short function with a known bug. Starting with a whole repository makes it hard to tell an access problem from a request problem or a weak prompt.

In the native Gemini format the model goes in the URL path, not the body, and `contents` is the only required field.<sup>[3]</sup> Here is a body for `POST /v1beta/models/gemini-4-carbon:generateContent`:

The `32768` budget follows Google's advice to set at least 32,768 when you set the field explicitly, because Carbon can spend much of the limit on thinking.<sup>[5]</sup> It is also the ceiling for one non-streaming REST call, which the next section explains.

On reAPI, send the request to `https://reapi.ai/api/v1beta/models/gemini-4-carbon:generateContent` with `x-goog-api-key: YOUR_API_KEY`. Bearer authentication is also accepted. Use your reAPI key, not a Google key. To choose a group, first read `GET https://reapi.ai/api/v1/models/gemini-4-carbon/groups`, then use the returned `gemini-4-carbon:GROUP_ID` model ID. The bare model ID uses the configured default; no custom group header is required. No group-specific key is required.<sup>[3]</sup>

Leave out what Gemini 4 Carbon ignores or rejects. Google says `temperature`, `topP` and `topK` are ignored if you send them, and explicit context caching is not supported. Implicit caching applies automatically to prompts of at least 4,096 tokens, so put large static material such as a codebase or long document at the start of the prompt.<sup>[5]</sup> If your client still sends `thinkingBudget` from an older model, replace it with `thinkingLevel`; Google's Agent Platform guide says `thinking_budget` and `MINIMAL` return HTTP 400 on Gemini 4 models there.<sup>[7]</sup>

Gemini 4 Carbon has a unified window of 1,048,576 tokens that input, thinking and output share. A single synchronous call stops far below that.<sup>[2]</sup> Google's long-decode guide sets the per-hop caps:<sup>[6]</sup>

| Call type | Output tokens per hop | Above the cap | 
|---|---|---|
| `generateContent` , non-streaming REST | 32,768 | `maxOutputTokens` above 32,768 without a`continuationToken` returns 400 | 
| `streamGenerateContent` | 65,536 | Continue with the returned token | 
| Long decode across hops | Up to 1,048,576 minus input | Requires continuation | 

Continuation works hop by hop. When a hop reaches its cap and budget remains, the response ends with `finishReason: "CONTINUATION"` and a `continuationToken`. Send the identical request again with that token added, and do not append the partial answer to `contents`, since the token already carries the model's state. Keep going while the finish reason is `CONTINUATION`.<sup>[6]</sup>

A hop can return empty text. On hard problems the model may spend a whole hop on thinking, and Google treats an empty `text` with a continuation token as normal progress rather than a failure.<sup>[6]</sup>

Plan for slow responses too. Google recommends HTTP read timeouts of 20 minutes for long decode.<sup>[6]</sup> reAPI also enforces its configured request deadline; a longer client timeout does not extend that deadline. Prefer `streamGenerateContent?alt=sse` for incremental output and handle explicit continuation in your client. reAPI does not retry or continue a generation automatically.<sup>[3]</sup> A dropped connection does not prove that generation failed, so check the partial result before sending the same request again.

`thinkingLevel` trades quality against latency and token use.<sup>[5]</sup>

| Level | Token use | Google's suggested use | 
|---|---|---|
| `low` | Fewest tokens, lowest latency | Simpler and cost-sensitive work; quality can drop on hard tasks | 
| `medium` (default) | Balanced | Most tasks, including complex code and agentic work | 
| `high` | Most tokens, slowest first token | Hard math and science, the most difficult code; pairs with long decode | 

Google's advice is to run your own evaluation at all three levels and pick the lowest one that meets your quality bar.<sup>[5]</sup> A level that worked on Argon is a reasonable starting point but not a conclusion, because Carbon spends tokens differently.

Read more than the answer text. A native response carries `candidates[].content.parts`, `finishReason` and `usageMetadata`. When returned, `continuationToken`, `safetyRatings` and `promptFeedback` also help your application decide what to do next.<sup>[3]</sup>

- `finishReason: "MAX_TOKENS"` means the output budget ran out. Google says you can resend with the returned token and a higher`maxOutputTokens` , or start over and pay for both runs.<sup>[6]</sup>
- `thoughtsTokenCount` shows the thinking tokens you were billed for as output. A short visible answer can sit on top of a large thinking count.<sup>[4]</sup><sup>[6]</sup>
- REST returns usage for one hop at a time. Add the fields across hops for cost, and take the prompt size from the first hop only.<sup>[6]</sup>
- Streamed usage on reAPI is cumulative, so read the latest counters instead of summing every chunk.<sup>[3]</sup>

Keep the complete content parts, including `thoughtSignature`, when you build conversation history.<sup>[3]</sup> For a code fix, run the tests before you accept the patch, since a 200 response only shows that the service accepted the request.

Google's October 28 announcement put Gemini 4 Carbon first on the Artificial Analysis Intelligence Index v5.0 with a score of 59. It reported 88.2% on SEC-bench Pro, 54.8% on ExploitGym and 71.0% on CWE-bench, all evaluated without guardrails.<sup>[8]</sup> These are vendor-reported results. Test the model on your own tasks before you route production traffic to it.

Not on the Gemini API. Google's pricing page lists it as unavailable on the free tier, and consumer access in the Gemini app requires a Google AI Ultra subscription.<sup>[1]</sup><sup>[4]</sup> Researchers can apply for six months of free Antigravity access, subject to selection.

Yes, since November 10, 2026, for paying Gemini API developers, Gemini for business and Google AI Ultra subscribers. Before that it was limited to defenders in the Fairwind Program.<sup>[1]</sup>

On Google's API, $2 per million input tokens and $10 per million output tokens through November 30, 2026, then $4 and $20.<sup>[4]</sup> Thinking tokens count as output. reAPI publishes current rates for each available group on the model page.

1,048,576 tokens shared by input, thinking and output. The default synchronous output limit is 65,536 tokens, or 32,768 for a non-streaming REST call, and long decode can go further.<sup>[2]</sup><sup>[6]</sup>

Yes. Google says the upgrade needs no request schema change, only the model ID. Allow for longer thinking in your output budget and timeouts.<sup>[5]</sup>

Yes. Use the playground with your signed-in reAPI account, or create an API key for your own client. The page displays current group prices; selecting a different group does not require another key.

Start with a small task, use `gemini-4-carbon` in the URL, set `thinkingLevel` and `maxOutputTokens` deliberately, and use streaming when a task may run long. Read the finish reason and token counts before making the prompt bigger. Open Gemini 4 Carbon on reAPI, compare group rates and run a prompt whose answer you can check.

1. *Gemini 4 Carbon is now available.* Published November 10, 2026, retrieved November 11, 2026 from blog.google/innovation-and-ai/models-and-research/gemini-models/gemini-4-carbon-now-available.
2. *Gemini 4 Carbon model page.* Retrieved November 11, 2026 from ai.google.dev/gemini-api/docs/models/gemini-4-carbon.
3. *Gemini 4 Carbon API reference.* Integration checked November 14, 2026. Native endpoints and request behavior; playground and live group prices.
4. *Gemini Developer API pricing: Gemini 4 Carbon.* Retrieved November 11, 2026 from ai.google.dev/gemini-api/docs/pricing.
5. *What's new in Gemini 4 Carbon.* Retrieved November 11, 2026 from ai.google.dev/gemini-api/docs/whats-new-gemini-4-carbon.
6. *Long decode (generateContent).* Retrieved November 11, 2026 from ai.google.dev/gemini-api/docs/long-decode.
7. *Get started with Gemini 4.* Gemini Enterprise Agent Platform documentation, retrieved November 11, 2026 from docs.cloud.google.com/gemini-enterprise-agent-platform/models/start/get-started-with-gemini-4.
8. *Gemini 4 Carbon: our next frontier model.* Published October 28, 2026, retrieved November 11, 2026 from blog.google/innovation-and-ai/models-and-research/gemini-models/introducing-gemini-4-carbon.
