---
id: collect-261121-veille-ia/qwen/qwen3-8-omni-flash-cheaper-audio-and-video-ai-agents
title: "Qwen3.8-Omni-Flash: Cheaper Audio and Video AI Agents"
domain: qwen
role: reference
task: article
actors: ["Alibaba"]
dates: ["2026-09-18"]
keywords: ["model", "qwen", "audio", "qwen3", "video", "input", "omni-flash", "price"]
source: https://www.digitalapplied.com/blog/qwen3-8-omni-flash-omnimodal-agents-audio-video-cost
source_anchor: ""
source_lines: [1, 92]
section: "Qwen"
---
On September 18, 2026 Qwen released Qwen3.8-Omni-Flash, a model that takes text, images, audio and video in one request and holds a 1M-token context window. It is served through Alibaba Cloud Model Studio and, at the international list price published the same week, an hour of audio input costs well under a cent. Qwen says the per-hour price of audio input is more than 98% lower than its own previous omnimodal model. That comparison is with Qwen3.5-Omni-Plus, not with anyone else's model.

"Omnimodal" means one model reads the sound and the pictures together, rather than a transcription service feeding text to a language model. For anyone building meeting-minutes, call-review, captioning, dubbing or long-video search products, the decision this release changes is whether to keep a separate speech-to-text step at all. This post explains what the price claim compares, works the per-hour cost from Qwen's published token formula, tables the headline benchmarks with the model each is compared against, and sets out a test worth running before switching. Sources are Qwen's release post and the Model Studio documentation, all read on September 18, 2026. Every benchmark figure is Qwen's own.

1. 01The 98% is against Qwen's own last model.Qwen's per-hour audio price is compared with Qwen3.5-Omni-Plus, using 30 times the input cost of a two-minute clip. Audio-visual input at 720p and 1 frame per second is down more than 93% on the same basis.
2. 02About $0.004 per hour of audio input, worked from the docs.Model Studio bills audio at 7 tokens per second and lists qwen3.8-omni-flash at USD 0.15 per million input tokens internationally. One hour is 25,200 tokens, or roughly $0.0038 before output.
3. 03Vendor-run wins on agent and meeting tasks, losses elsewhere.Qwen reports large gains on its multimodal tool-use and meeting-transcription benchmarks and says the model is close to Gemini 3.8 Flash on audio-visual tasks. Its own table also shows Gemini ahead on several video-reasoning rows.
4. 04Test it on your own ten recordings.Same files through your current pipeline and through this model, scored on cost per hour and error count. Meeting minutes from video, dubbing and long-video question answering are the workloads Qwen built it for.

## 01 — The releaseWhat shipped on September 18

Qwen describes the model as its next native omnimodal model, built to move audio and video from things a model can describe to things an agent can act on. The release post gives a set of example workflows: editing video, making music videos, translating and dubbing short dramas, writing film commentary, summarising long recordings, and holding real-time conversations. Alongside the model, Qwen expanded its open-source Qwen-MM-Plugins, a set of skills and tool servers that let coding agents such as Claude Code, Codex and Qwen Code read images, video and audio, and announced an open-source runtime for real-time omnimodal interaction called Qwen-Live Harness.

##### Context window

Text, image, audio and video input; text output. Maximum output 131,072 tokens. Up to 2 hours of audio or video per file and 64 files per request, per the Model Studio page.

##### Per hour of audio input

Compared with Qwen3.5-Omni-Plus, by Qwen's own estimation method. Audio-visual input is down more than 93% on the same basis. Not a comparison with any other vendor.

##### Across 29 evaluations

Qwen's average score across 29 audio, audio-visual and agent benchmarks against Qwen3.5-Omni-Plus. All runs are Qwen's; the list of benchmarks is in the post's footnote.

Two things the post does not say. It does not announce open weights, and we found no model card for the model on September 18, so treat Qwen3.8-Omni-Flash as an API model until Qwen says otherwise. It is also not the same release as Qwen3.8-Max, the text-and-vision flagship we covered earlier this month; the Omni model is the one that hears.

## 02 — The priceThe price claim, *read correctly*

Qwen's footnote states the method: the hourly price of audio or audio-visual input is estimated as 30 times the input cost of two minutes of source material, with audio-visual input measured at 720p and one frame per second. The comparison model is Qwen3.5-Omni-Plus. The post's own table puts text prices in Chinese yuan and converts Gemini and Muse prices at 6.7191 yuan to the dollar. So the 98% figure is a like-for-like estimate of Qwen's own two generations, and it says nothing about how the model prices against OpenAI or Google.

The dollar figure is on the documentation side, not in the post. The Model Studio pricing page lists the international deployment of qwen3.8-omni-flash at USD 0.15 per million input tokens, USD 0.016 per million on a cache hit and USD 0.47 per million output tokens, and the Qwen-Omni documentation gives the audio formula: total tokens equal audio duration in seconds multiplied by seven. Working from those two published numbers:

- Tokens in one hour of audio3,600 seconds × 7 tokens per second
- 25,200
- Input cost for that hour25,200 ÷ 1,000,000 × USD 0.15
- $0.0038International list price, Sep 18, 2026
- Output for a one-page summaryAbout 1,000 tokens × USD 0.47 per million
- $0.0005Add per request; thinking tokens bill as output

Two cautions on that arithmetic. The seven-tokens-per-second rate is the audio-only formula; video frames are billed separately by resolution and frame rate, so an hour of audio-visual input costs more and depends on the settings you choose. And the price row we read is the international deployment scope; other deployment scopes are priced separately on the same page. Our cost-per-hour census puts this figure beside other providers on one stated method.

## 03 — The claimsThe headline claims, vendor-run

The table keeps each claim next to the model it is compared with, because the post mixes two comparisons: the previous Qwen omni model, which is where the big deltas come from, and Gemini 3.8 Flash, where the picture is mixed. Qwen's summary is that the model is close to Gemini 3.8 Flash on audio-visual work and ahead of it on audio overall. Its own table supports the second claim more clearly than the first.

| Source: Qwen, "Qwen3.8-Omni-Flash: Omni Senses. Agentic Delivery.", benchmark tables, read September 18, 2026. All runs by Qwen; agent benchmarks used the Claude Code or OpenClaw harnesses. |  |  |  | 
|---|---|---|---|
| Benchmark | Qwen3.8-Omni-Flash | Qwen3.5-Omni-Plus | Gemini 3.8 Flash | 
|---|---|---|---|
| WildClawBench-MM, multimodal tool use | 71.0 | 34.5 | 58.9 | 
| AgenticVBench, multimodal tool use | 36.8 | 14.5 | 45.0 | 
| UniClawBench, multimodal tool use | 69.6 | 67.1 | 69.0 | 
| OmniVideoBench, audio-visual reasoning | 63.4 | 53.8 | 65.2 | 
| LongAudioSpan, accuracy | 82.7 | 74.4 | 79.3 | 
| AliMeeting, speaker error / word error (lower is better) | 3.4 / 17.2 | 88.1 / 89.6 | 72.6 / 53.1 | 
| FLEURS-ASR, 60-language word error (lower is better) | 9.3 | 7.2 | 7.9 | 
| VoiceBench, audio interaction | 91.6 | 92.9 | 92.3 | 

Read the rows in pairs. The multi-speaker meeting row is the standout: Qwen reports its previous model essentially failing the AliMeeting test and the new one bringing speaker error to 3.4 and word error to 17.2, which is the capability behind the meeting-minutes use case. The two rows where the new model trails its own predecessor, multilingual transcription and VoiceBench, are small regressions Qwen prints rather than hides. On the three agent benchmarks, Qwen's footnote says two were run inside the Claude Code harness and one inside OpenClaw, so the scores measure the model plus a harness, not the model alone, a distinction our harness-tax post showed can move results by a wide margin.

## 04 — The mechanismThe agentic video mode

The most useful engineering idea in the post is not a score. For a recording that runs for hours, the usual approach feeds the whole thing to the model even when the answer sits in three minutes of it. Qwen's agentic mode starts from the question, decides which parts to watch and listen to, and gathers evidence in several passes from coarse to fine, so most frames are never processed. On OmniVideoBench, Qwen reports accuracy rising from 63.4 in the static setting to 67.8 in the agent setting while tokens per query fell from 145,736 to 79,117, a reduction of about 45.7%.

Two details keep that honest. The agent setting ran inside Qwen Code, Qwen's coding-agent harness, and Qwen ran Gemini 3.8 Flash through the same harness; on that benchmark Gemini scored 70.1 in agent mode, ahead of Qwen's 67.8, while Qwen led on the long-video LVOmniBench at 73.6 against 70.7. And the token saving is a saving on the model's own static mode, measured on one benchmark. The pattern, though, transfers to any long-media product: let the model index first and read selectively, and the bill falls with the accuracy intact. That is the same instinct behind Google's live model, which we compared for voice agents in our Gemini 3.8 Live post.

Qwen-MM-Plugins installs each capability as a skill plus an optional tool server for agents such as Claude Code, Codex, Gemini CLI and Qwen Code. The Omni set includes a video-to-notes tool that turns a tutorial into an illustrated PDF, a skill creator that turns a demonstration video into a reusable agent skill, and an audio-visual memory that records who was present and who said what in a long video. The README notes that most harnesses cannot yet pass audio to the main model natively, so audio goes through the API. Repository on GitHub.

## 05 — Your testWho should test it, and how

The model is worth a week of testing for three kinds of product, and not yet for a fourth. The routing below is ours; Qwen's post names the first three as target workflows.

The test itself is simple and should not be skipped. Take ten recordings you have already processed, with the outputs your team accepted. Run them through the current stack and through this model with the same prompt, using the meeting, subtitle or summary prompts in the Model Studio guide as a starting point. Score two things: the cost per hour of media, from the token counts the API returns rather than from anyone's estimate, and the number of errors a person finds in each output, with speaker mistakes counted separately from wording mistakes. If the model wins on both, move one workflow and keep the old pipeline as a fallback for a month.

## 06 — PracticalitiesAvailability and limits

Everything below is from the model page, the Qwen-Omni guide and the pricing page on Alibaba Cloud Model Studio, read September 18, 2026.

- **Regions.** China (Beijing), Singapore, China (Hong Kong), Japan (Tokyo), Germany (Frankfurt) and US (Virginia). API keys are per region.
- **APIs.** OpenAI-compatible Chat Completions and Responses. Function calling and web search are supported; thinking is on by default with adjustable effort; implicit context caching applies.
- **Input limits.** Up to 64 files per request, 2 GB per file by URL, and 2 hours of duration per file. Audio input covers 113 languages and dialects; spatial audio is accepted through a multichannel flag.
- **Output.** Text only. For spoken replies the docs route you to the Qwen3.5-Omni models.
- **Not yet elsewhere.** No listing on OpenRouter at our September 18 check, and the Qwen-Live Harness repository the post links to returned a not-found page on GitHub the same day. The plugins repository is live.

If your product already runs on an OpenAI-shaped client, the switch is a base URL and a model name, which is what makes the ten-file test cheap. If you want a second pair of hands to run it and to wire the winner into a production pipeline with the cost logging described above, our AI transformation service does exactly that work.

## 07 — Next stepA model that hears, priced by the second

### Run your ten recordings through it before the end of the month

The claim that survives scrutiny is narrow and useful: audio input at seven tokens a second and fifteen cents a million makes an hour of recording cost a fraction of a cent to read, and Qwen's meeting benchmarks say the reading is good enough to attribute speakers. Whether that holds on your recordings is a one-week question with a cheap answer. Ask it, keep the token counts, and decide from those.
