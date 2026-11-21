---
id: collect-261121-veille-ia/deepseek/deepseek-just-opened-its-whole-app-as-an-api-search-files-and-voice-for-pennies
title: "DeepSeek Just Opened Its Whole App as an API — Search, Files and Voice for Pennies"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: ["2026-11-13"]
keywords: ["deepseek", "search", "voice", "model", "tools", "chat", "whole", "opened"]
source: https://dev.to/paullovelace/deepseek-just-opened-its-whole-app-as-an-api-search-files-and-voice-for-pennies-2n1g
source_anchor: ""
source_lines: [1, 60]
section: "DeepSeek"
---
If you have used DeepSeek's consumer app, you know the feeling: ask a question, it searches the web, reads the PDF you attached, and answers with sources attached. Until now, that whole experience lived behind the app's front door. On 13 November 2026, DeepSeek opened the door — the tools built into its app are now APIs any developer can call.

This is not another model release. There is no new flagship to benchmark and no leaderboard to argue about. It is something quieter and, for people who build things, more useful: the capabilities that made DeepSeek's app feel complete are now rentable by the request, at prices that make experimentation almost free.

There are two layers to the announcement, and it is worth keeping them separate.

**Built-in tools inside chat.** When you send a chat request to the `deepseek-flash` model (DeepSeek-V4.1-Flash-1017), you can now switch on the same tools the consumer app uses: web search, deep (multi-step) search, file input, URL reading, and content safety checks. They work across the Messages, Responses and Chat Completions APIs, so you are not locked into one integration style. DeepSeek runs the tools server-side and assembles the results into the model's context — you send a question, the model decides when to search or read, and the answer comes back with the work already done.

**Standalone tool APIs.** If you would rather use the tools without the chat model, they are available on their own: Search and Fetch, OCR, speech-to-text (ASR) and text-to-speech (TTS). You can wire them into your own pipeline, your own model, or your own product, and pay only for what you use.

All prices below are DeepSeek's own published figures, reported by the vendor, not independently verified by us. The pattern is the story: every line is cheap enough that cost stops being the reason not to try something.

| Tool | Price (vendor-reported) | 
|---|---|
| Search and Fetch | $0.75 per 1,000 successful requests | 
| OCR | $0.225 per 1,000 pages | 
| Speech-to-text (ASR) | $0.116 per hour — currently 50% off at $0.058 | 
| Text-to-speech (TTS) | $0.45 per 10,000 characters — currently 50% off at $0.225 | 
| deepseek-flash tokens | $0.30 per 1M input / $1.20 per 1M output at peak; half that off-peak | 

There is no monthly fee. It is pay-as-you-go, and the off-peak discount on the flash model means a lot of real workloads — overnight batch jobs, background research, queued document processing — run at half the already-low headline rate.

To put the voice numbers in plain terms: at the current 50%-off rate, an hour of transcribed audio costs under six cents, and ten thousand characters of spoken output costs about twenty-two cents. That is the kind of pricing where a voice feature stops being a premium tier and starts being a default.

Search and file-reading are useful, but they are extensions of things DeepSeek already offered. Voice is genuinely new. Before this announcement, there was no public DeepSeek endpoint for listening or speaking at all — if you wanted a DeepSeek-powered voice product, you had to bolt on somebody else's speech stack and pay somebody else's prices.

Now the loop closes inside one provider: transcribe the user's speech, reason over it with a cheap flash model that can search and read files, and speak the answer back. That is the full shape of a voice assistant, and it arrives at a moment when every other major provider charges noticeably more for the same pieces. We have not tested voice quality hands-on, but the price alone will pull a wave of builders in to find out.

DeepSeek also open-sourced a demo chat client on GitHub that rebuilds the consumer app one-to-one, including importing history exported from the DeepSeek app. It is a working reference implementation, not a toy example: you can see exactly how the tools are meant to be wired together, fork it, and have something real running the same afternoon.

Some context, because this story is easy to confuse with an earlier one. On 17 October, DeepSeek opened its in-house search engine as a Search and Fetch API — we covered that on 19 October in "DeepSeek Shipped Search + Fetch APIs for Agents", and that article covered *only* search and fetch. This announcement is the rest of the app: file input, URL reading, safety checks, OCR, and voice, plus the built-in tools inside chat requests. Same direction, much bigger scope.

It is also not a new model. `deepseek-flash` is the existing V4.1-Flash model; what changed is what it can reach for while it works.

The honest answer is: the boring, useful things that used to need three vendors and a budget meeting.

- 
**A research assistant that reads your files.** Drop in a contract, a paper, or a folder of reports, and ask questions with web search on tap for anything the files do not cover.
- 
**A voice interface for an existing product.** A support bot that listens, a note-taking tool that transcribes, a language app that talks back — all without a second speech provider.
- 
**Cheap document pipelines.** OCR at $0.225 per thousand pages turns a pile of scanned PDFs into searchable text for less than the cost of a coffee.
- 
**Agents that check their own work.** Because the tools run server-side inside the chat request, an agent can search, read a URL, and cite what it found without you writing the orchestration glue yourself.

A few things to keep in mind before you rebuild your stack around this.

First, every price and capability above comes from DeepSeek's own announcements and documentation, as reported in the sources below. We have not run these APIs hands-on, so treat the figures as vendor-reported until independent testing confirms them — and expect the 50%-off voice rates to be promotional, not permanent.

Second, the convenience has a trade-off built in: your files, URLs and audio pass through DeepSeek's servers, and the content safety checks run there too. If you handle sensitive documents or regulated data, read the data-handling terms before you send anything, and keep anything you cannot share out of the pipeline entirely.

Third, server-side tools mean less control. When DeepSeek assembles search results into context for you, you are trusting its retrieval and its judgement about what is relevant. For many uses that is a fair trade. For anything where the sources matter as much as the answer, inspect what the tools returned, not just what the model said about it.

DeepSeek keeps doing the same thing: taking a capability that used to be a product in its own right and pricing it like infrastructure. Search, OCR, voice — each of these has companies built around it. DeepSeek just made all of them a line item on one API bill, most of them for pennies.

That does not make it the right choice for everyone. Voice quality, reliability, and data handling still need to be proven in real use, and the cheapest tool is not always the best tool. But the direction is unmistakable: the app is becoming the API. If you build with AI tools, this one is worth an afternoon of testing — the cost of finding out is almost nothing.

*Sources: DeepSeek API docs change log, 13 November 2026; siliconnoon.com — "DeepSeek Opens Its Whole App as an API — Voice at $0.058 an Hour" (https://siliconnoon.com/deepseek-opens-its-whole-app-as-an-api-voice-at-0058-an-hour/); blog.go5.si — DeepSeek tool APIs (https://blog.go5.si/posts/deepseek-tool-apis/); kingy.ai — DeepSeek API update: web search, file reading, voice (https://kingy.ai/news/deepseek-api-update-web-search-file-reading-voice/). All prices and capabilities are DeepSeek's as reported there (vendor-reported). No hands-on testing was performed for this article. This breaking article is not the daily buyer-intent post, and it contains no affiliate links. AI disclosure: this article was drafted with AI assistance and checked against the DeepSeek API docs change log.*

## Top comments (0)
