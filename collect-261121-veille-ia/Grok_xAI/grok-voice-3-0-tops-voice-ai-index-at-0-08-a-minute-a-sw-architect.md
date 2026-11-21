---
id: collect-261121-veille-ia/grok-xai/grok-voice-3-0-tops-voice-ai-index-at-0-08-a-minute-a-sw-architect
title: "Grok Voice 3.0 Tops Voice AI Index at $0.08 a Minute Â· SW Architect"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: ["2026-10-15"]
keywords: ["voice", "grok", "index", "gpt-live-1", "test", "agent", "gemini", "leads"]
source: https://www.swarchitect.net/en/realtime-voice-ai-compared-2026-10/
source_anchor: ""
source_lines: [1, 228]
section: "Grok_xAI"
---
AI Â· VOICE MODELS

# Grok Voice 3.0 Tops the Voice AI Index; GPT-Live-1 Leads on Agent Tasks

Grok Voice 3.0, out October 14, tops the Artificial Analysis voice index at 86.9 for $0.08 a minute. GPT-Live-1 leads agent tasks; Gemini 3.8 Live costs least.

The essentials

- Grok Voice 3.0 took first place on the AA speech-to-speech index with 86.9. AA leaderboard
- Test by test, the leader changes. On agent tasks, GPT-Live-1 Astra scores higher than Grok.
- Gemini 3.8 Live is the cheapest by AAâs cost measure, but it has the lowest agent-task score of the seven settings.

Opening

## xAI released Grok Voice 3.0 to its Voice Agent API on October 14

xAI released Grok Voice 3.0, a speech-to-speech model, on October 14 (xAIâs date). It is available right away in the Voice Agent API at the same $0.08 a minute as version 2.0. On the speech-to-speech leaderboard of the independent evaluator Artificial Analysis (AA), it ranks first with an index of 86.9. It does not lead every test, though. OpenAIâs GPT-Live-1 leads on agent tasks, and Googleâs Gemini 3.8 Live on cost. xAI announcement

A speech-to-speech model hears a person and answers in sound right away. Rather than chaining separate transcription, text-model and speech-synthesis steps, it takes in audio and sends audio back. Voice agents such as call-center support, in-car assistants and phone booking run on models like these. So which test a model leads on matters more for choosing than a few index points.

This article compares the three companiesâ models on AAâs shared tests, explains why official prices and AAâs cost figures differ, and lists what to check before switching. We checked the AA leaderboard and the three companiesâ docs on the evening of October 15, Seoul time. The leaderboard does not date individual scores; xAI labels its figures as AA data from October 13.

Source: Artificial Analysis speech-to-speech leaderboard (checked 2026-10-15; scores undated). Cost is a 40-question Big Bench Audio subset normalized to one hour of input audio, so it differs from a bill (section 3). Live-1 is GPT-Live-1; Gemini ET is Gemini 3.8 Live Extended Thinking (High).

01 Â· THE INDEX

## Grok Voice 3.0 took first place on the AA index with 86.9

On the AA speech-to-speech index, Grok Voice 3.0 scores 86.9 and ranks first. Second is GPT-Live-1 Astra (medium) at 83.2. Third is Gemini 3.8 Live Extended Thinking (High) at 82.6. AA leaderboard

The previous Grok Voice 2.0 scored 81.3. The price stayed put while the score rose 5.6 points (our calculation). xAI says 3.0 also powers Grok Botâs voice mode. The announcement does not say which Grok app plans get it.

Developers already on the API have little to do. xAI writes that agents using grok-voice-latest âhave already been switched over automatically.â In xAIâs model list, Grok Voice 2.0 now carries a âDeprecatedâ label. No end date is given. xAI model list

xAI also pointed to real deployments. It says Grok Voice handles millions of conversations a day in Tesla vehicles. It says Starlinkâs call centers take 20,000 calls a day and resolve about 70% without a human. These are all company claims with no independent check.

02 Â· TEST BY TEST

## GPT-Live-1 leads on agent tasks, Gemini 3.8 Live on human preference

The index rolls several tests into one number. Look test by test and the leader changes. Grok Voice 3.0 leads on three: the overall index, spoken reasoning, and the item AA labels âtask success.â Switch the measure in the figure below and the order re-sorts.

Ï-Voice is the spoken task test AA uses for its agent category. xAI says it includes diverse accents, background noise and interruptions. Here GPT-Live-1 Astra scores 74.5%, above Grokâs 71.6%. xAI itself calls the two âcomparableâ in its announcement.

On conversational dynamics, GPT-Live-1 Sol (low) is highest at 97.3%. Grok Voice 3.0 trails by 0.3 points at 97.0%. The top six settings sit between 94.9% and 97.3%, so gaps on this item are small.

On the preference score people gave by choosing, Gemini 3.8 Live is highest at 1088. Grok has 1070. Yet the same Gemini 3.8 Live scores 30.1% on agent tasks, the lowest of the seven settings. A pleasing voice and a low price do not settle it for support work that calls tools; test that separately.

Overall index leader: Grok Voice 3.0 Â· 86.9

Source: Artificial Analysis speech-to-speech leaderboard (checked 2026-10-15). Preference bars start at 900. Spoken reasoning is rounded to whole numbers in the leaderboard table. ET is Extended Thinking.

## Show as a table Â· every value for the seven settings

| Model Â· setting | Overall index | Agent tasks | Conversation | Spoken reasoning | Task success | Preference | First audio | AA cost | 
|---|---|---|---|---|---|---|---|---|
| Grok Voice 3.0 | 86.9 | 71.6% | 97.0% | 99% | 94.8% | 1070 | 0.81 | $4.80 | 
| GPT-Live-1 Astra (medium) | 83.2 | 74.5% | 94.9% | 90% | 87.4% | 1056 | 1.34 | $5.83 | 
| Gemini 3.8 Live ET (High) | 82.6 | 68.6% | 91.9% | 98% | 89.1% | 993 | 1.35 | $3.50 | 
| Grok Voice 2.0 | 81.3 | 56.5% | 95.1% | 97% | 94.6% | 1011 | 0.70 | $4.80 | 
| GPT-Live-1 Sol (low) | 80.1 | 59.3% | 97.3% | 89% | 90.9% | 1057 | 1.24 | $4.47 | 
| Gemini 3.8 Live | 76 | 30.1% | 96.1% | 92% | 93.2% | 1088 | 1.18 | $0.84 | 
| GPT-Realtime-2.1 High | 75.2 | 45.7% | 95.7% | 96% | 91.5% | 939 | 1.21 | $10.75 | 

Cost is USD per hour of input audio; first audio is in seconds.

03 Â· THE BILL

## AAâs cost is per hour of input audio, not your bill

AAâs cost figure is not a bill. AA takes what it cost to run a 40-question subset of Big Bench Audio and normalizes it to one hour of input audio. It says the figure leaves out cached-token discounts and tool-call costs. AA leaderboard

On that basis, Grok Voice 3.0 costs $4.80 an hour. That is exactly its official $0.08 a minute times 60. Gemini 3.8 Live is cheapest at $0.84. GPT-Live-1 Astra is $5.83.

For GPT-Live-1, the official price and AAâs figure differ widely. OpenAIâs docs say $0.05 a minute, billed per second. That is $3 an hour. But the docs also say âbackend model and tool usage is billed separately.â AA does not break down what went into its $5.83, so we could not confirm the difference. GPT-Live-1 docs

Gemini 3.8 Live is priced by tokens. Per million tokens, audio input is $3.00 and audio output $12.00. The pricing page also lists these as $0.005 and $0.018 a minute. Listening and speaking are charged separately. Thinking tokens are billed at the output rate, and there is a free tier. Gemini pricing

Take 10,000 minutes of support calls a month (our assumption). Grok comes to $800. GPT-Live-1 is $500 for the front end alone, plus the backend model. For Gemini, assuming 10,000 minutes of input and 4,000 minutes of the model speaking, it is $122. All three are our calculations, before tax.

READING THE EVIDENCE

## Official prices match AAâs cost only for Grok

Official prices come from each companyâs docs and price list; AA cost is from the leaderboard (per hour of input audio). All checked 2026-10-15.

| Model | Official price | Billing unit and extras | AA cost (per hour) | 
|---|---|---|---|
| Grok Voice 3.0 | $0.08 a minute | Listed per minute; text input $0.004 (unit not stated) | $4.80 | 
| GPT-Live-1 Astra (medium) | $0.05 a minute | Billed per second; backend model and tools extra | $5.83 | 
| GPT-Live-1 Sol (low) | $0.05 a minute | Same as above | $4.47 | 
| Gemini 3.8 Live | Per million tokens: audio in $3.00, out $12.00 | Per token; also listed as $0.005/min in, $0.018/min out. Thinking billed as output. Free tier | $0.84 | 
| Gemini 3.8 Live ET (High) | No separate price listed | The price list groups Extended Thinking under Gemini 3.8 Live | $3.50 | 

AAâs cost is a 40-question Big Bench Audio subset normalized by input audio duration, excluding cache discounts and tool-call costs. Grokâs AA figure equals $0.08 Ã 60. AA does not say where GPT-Live-1âs gap comes from. ET stands for Extended Thinking.

04 Â· LATENCY

## First audio in 0.81s: faster than GPT-Live-1, 0.11s slower than 2.0

Time to first audio is 0.81 seconds for Grok Voice 3.0. GPT-Live-1 Astra takes 1.34 seconds and Gemini 3.8 Live 1.18. AA leaderboard

xAI says its latency is 40% lower than GPT-Live-1 (Astra). AAâs figures put it 39.6% lower, consistent with the claim (our calculation).

It is slower than its predecessor, though. Grok Voice 2.0 took 0.70 seconds; 3.0 adds 0.11. On first words alone, 2.0 is still the fastest of the seven settings. But 2.0 now carries a deprecation label, so it is not a model to pick up now.

These are measurements in AAâs test setup. Latency in a real service depends on server location, network and attached tools. If latency matters, measure again on your own network with the same conversations.

05 Â· SWITCHING

## Agents on grok-voice-latest are already running 3.0

The way to connect has not changed. The endpoint in xAIâs voice docs is wss://api.x.ai/v1/realtime?model=grok-voice-latest. After connecting, a session.update sets the voice, the instructions, how the end of a turn is detected and which tools to use. xAI voice docs

xAIâs model list has a separate name, grok-voice-3.0. But the examples in the voice docs only use grok-voice-latest. We could not confirm from the docs whether you can pin the connection to version 3.0.

An automatic switch means agents already running may now answer differently. Agent-task and conversational scores differ from 2.0. It is safer to rerun the test conversations you already have (our advice). The steps are below.

1. From a server, connect to wss://api.x.ai/v1/realtime?model=grok-voice-latest with your API key. Browser apps use ephemeral tokens so the API key is never exposed.
2. After connecting, send a session.update. The docsâ example sets voice âeveâ (the default), instructions, a turn_detection type of âserver_vadâ and the web_search tool.
3. If you need your own voice, create a custom voice from a reference clip of up to 120 seconds. The docs say it works with the speech-to-speech API too.
4. Rerun the test conversations you used with 2.0 (tool calls, interruptions, noisy recordings) and compare answers and latency. The price stays at $0.08 a minute.

06 Â· UNKNOWNS

## None of the three companiesâ docs states Korean voice support

For readers serving Korean speakers, the biggest gap is language. xAIâs voice docs say speech-to-text supports â38+ languages.â They also say the built-in voices can speak âevery supported language.â But there is no list. xAI voice docs

OpenAIâs GPT-Live-1 docs and Googleâs Gemini 3.8 Live docs do not list supported languages either. In none of the three companiesâ docs, opened on October 15, could we confirm Korean speech-to-speech support. Gemini 3.8 Live docs

AAâs scores do not speak to Korean quality either. The leaderboard does not state the test language, and AAâs methodology page did not open. To use these for Korean support calls, test with Korean recordings yourself.

None of the three docs gives a session-length limit. OpenAI sets limits by concurrent sessions: 50, 300 or 500 by usage tier, and the free tier cannot use the model. Gemini 3.8 Live accepts up to 131,072 input tokens.

READING THE EVIDENCE

## Terms of use for the three models, and what the docs leave out

From each companyâs docs and price list (checked 2026-10-15). âNot statedâ means the documents we opened that day did not say.

| Item | Grok Voice 3.0 | GPT-Live-1 | Gemini 3.8 Live | 
|---|---|---|---|
| Languages | STT 38+ (no list) | Not stated | Not stated | 
| Limits | Not stated | 50 / 300 / 500 concurrent sessions by tier | 131,072 input, 65,536 output tokens | 
| Session length | Not stated | Not stated | Not stated | 
| Free tier | Not stated | Unsupported | Yes | 
| Tools and function calling | Yes (example: web_search) | Yes | Yes (incl. search grounding) | 
| Model name | grok-voice-latest (list shows grok-voice-3.0) | gpt-live-1 | gemini-3.8-live | 
| Previous version | grok-voice-2.0 deprecated, no end date | Not stated | Not stated | 

None of the three documents states Korean speech-to-speech support. The GPT-Live-1 and Gemini 3.8 Live docs give no release date (the Gemini page was last updated 2026-09-15).

Reading the evidence

## Reading the evidence

1. AA scores are as shown on the leaderboard on the evening of October 15, Seoul time. The leaderboard does not date each score, and it changes when models are added or re-measured. The index is not a ratio scale: 86.9 is not some percentage better than 83.2.
2. Sub-scores mix rounding. The leaderboard table shows Grok Voice 3.0âs spoken reasoning as 99%; xAIâs announcement and AAâs explainer say 98.5%. Our figure and tables use the leaderboard table values.
3. AAâs cost is a 40-question Big Bench Audio subset normalized to one hour of input audio, without cached-token discounts or tool-call costs. It is not your bill, and we could not confirm why GPT-Live-1âs official price and AAâs figure differ.
4. AA says the preference score (Arena Elo) is frozen when a model first becomes eligible for publication. Scores of models listed early and late may not compare the same period.
5. Millions of Tesla conversations a day, 20,000 Starlink calls a day and about 70% resolved without a human are xAIâs claims. The 40% latency claim matches AAâs figures, but xAI stated it in the context of Ï-Voice.
6. Prices are in US dollars as listed, before tax. The 10,000-minute example is our assumption.

Conclusion

## Grok for overall and speed, GPT-Live-1 for agent tasks, Gemini for price

Grok Voice 3.0 raised its score at the same price and took first place on the AA speech-to-speech index. Its first audio also comes faster than GPT-Live-1âs or Geminiâs. GPT-Live-1 Astra is slightly ahead on tool-calling agent tasks, and Gemini 3.8 Live has the lowest AA cost, though its agent-task score is far lower.

Services on grok-voice-latest are already running 3.0, so rerun your test conversations and check answers and latency. For Korean-language services, none of the three documents its supported languages, so test with Korean recordings before you choose.

Quick check Â· 30 seconds

## On the AA leaderboard, which item does Grok Voice 3.0 not lead?

Sources

## References

Checked 2026-10-15. Claims and availability are distinguished.

- xAI Â· Grok Voice 3.0Company announcement Â· 2026-10-14 (xAIâs date); $0.08/min (same as 2.0); grok-voice-latest switched automatically; Grok Bot voice mode; cites AA data of 10-13; Tesla and Starlink figures (company claims) Â· checked 2026-10-15
- xAI Docs Â· Models and pricingOfficial price list Â· grok-voice-3.0 and grok-voice-2.0 both $0.08/min, $0.004 text input; 2.0 marked âDeprecatedâ (no end date) Â· checked 2026-10-15
- xAI Docs Â· VoiceOfficial docs Â· endpoint, session.update example (voice eve, server_vad, web_search), ephemeral tokens, 120-second reference clip, STT 38+ languages (no list) Â· checked 2026-10-15
- Artificial Analysis Â· Speech to Speech leaderboardIndependent measurement Â· index, sub-scores, cost per hour of input audio (40-question Big Bench Audio subset, excluding cache discounts and tool costs), time to first audio. Scores undated Â· checked 2026-10-15
- OpenAI API Docs Â· GPT-Live-1Official docs Â· $0.05/min billed per second; backend model and tools billed separately; 50/300/500 concurrent sessions; no free tier; languages and release date not stated Â· checked 2026-10-15
- Gemini API Docs Â· Gemini 3.8 LiveOfficial docs Â· 131,072 input and 65,536 output tokens; function calling and search grounding; thinking_level unsupported; languages not stated; page updated 2026-09-15 Â· checked 2026-10-15
- Gemini API Docs Â· PricingPrice list Â· Gemini 3.8 Live per million tokens: audio in $3.00 ($0.005/min), audio out $12.00 ($0.018/min), text $0.75 / $4.50; thinking billed as output; free tier Â· checked 2026-10-15
- FRED Â· South Korean Won to U.S. Dollar (DEXKOUS, Fed H.10)Official statistics Â· 1,342.13 won on 2026-10-09, the latest value as of 2026-10-15; used for the Korean editionâs won conversions
- Flickr Â· dejankrsmanovic, âDetails of Modern Condenser MicrophoneâCover photo source Â· CC BY 2.0, cropped to 16:9

## Source and terminology notes

Underlined links in the article open these notes as explanation cards.

- xAI Â· Grok Voice 3.0 announcement
- Company announcement, checked 2026-10-15. Dated 2026-10-14 (xAIâs date, no time). $0.08 a minute ($4.80 an hour) in the Voice Agent API, the same as 2.0. Agents on grok-voice-latest were switched automatically. Benchmarks are labeled as AA data from 2026-10-13. Millions of Tesla conversations a day, 20,000 Starlink calls a day, about 70% resolved without a human and one in four sales calls ending in a purchase are company claims. No language list. Source document â
- xAI docs Â· models and pricing
- Official price list, checked 2026-10-15. The Speech to Speech entries list grok-voice-3.0 and grok-voice-2.0, both at $0.08 a minute with $0.004 text input (unit not stated). grok-voice-2.0 is marked âDeprecatedâ with no end date. Source document â
- xAI docs Â· voice
- Official docs, checked 2026-10-15. Speech-to-speech endpoint wss://api.x.ai/v1/realtime?model=grok-voice-latest. session.update example: voice âeveâ (default), instructions, turn_detection type âserver_vadâ, tools web_search. Browser apps are told to use ephemeral tokens. Custom voices come from a reference clip of up to 120 seconds. Speech-to-text supports â38+ languagesâ and the built-in voices speak âevery supported language,â but there is no list. No session length or price is given. Source document â
- Artificial Analysis Â· speech-to-speech leaderboard
- Independent measurement, checked 2026-10-15. Index: Grok Voice 3.0 86.9, GPT-Live-1 Astra (medium) 83.2, Gemini 3.8 Live Extended Thinking (High) 82.6, Grok Voice 2.0 81.3, GPT-Live-1 Sol (low) 80.1, Gemini 3.8 Live 76.0, GPT-Realtime-2.1 High 75.2. Cost is âthe cost to complete a fixed 40-question Big Bench Audio subset, normalized to an hourly basis using input audio duration,â excluding cached-token discounts and tool-call costs. Scores are not dated. The methodology page did not open. Source document â
- OpenAI API docs Â· GPT-Live-1
- Official docs, checked 2026-10-15. Voice sessions cost $0.05 a minute, billed per second. âBackend model and tool usage is billed separately.â Streaming and function calling supported; structured outputs and fine-tuning not. Limits are concurrent sessions (Build 50, Launch 300, Grow 500); the free tier is unsupported. No release date or language list. Source document â
- Gemini API docs Â· Gemini 3.8 Live
- Official docs, checked 2026-10-15. Model name gemini-3.8-live. 131,072 input and 65,536 output tokens. Inputs are text, images, audio and video; outputs text and audio. Function calling, search grounding and thinking supported; thinking_level is not. No language list or session length. Page last updated 2026-09-15. Source document â
- Gemini API docs Â· pricing
- Price list, checked 2026-10-15. Gemini 3.8 Live paid tier per million tokens: text input $0.75, audio input $3.00 ($0.005/min), image and video input $1.00 ($0.002/min), text output $4.50, audio output $12.00 ($0.018/min). Output includes thinking tokens. The free tier is free of charge. No separate price is listed for Extended Thinking. Search grounding is free for 5,000 requests a month, then $14 per 1,000. Source document â
- Speech-to-speech model
- A model that takes speech in and answers in speech right away. Developers do not chain transcription, a text model and speech synthesis themselves: audio goes in and audio comes out. Some products, such as OpenAIâs GPT-Live-1, also call a backend model and bill its usage separately. It powers real-time âvoice agentsâ such as support lines and in-car assistants. Companies ship them under names like Realtime, Live or Voice Agent.
- AA speech-to-speech index
- A single score from Artificial Analysis, running the same tests on every speech-to-speech model. Sub-scores include spoken reasoning, conversational dynamics and agent tasks. We could not confirm the weights this time. It is not a ratio scale, so do not read gaps as âso many percent better.â
- Ï-Voice (agent tasks)
- The task test AA uses for the agent category of its speech-to-speech leaderboard. xAI says it includes diverse accents, background noise and interruptions. It looks at getting a job done the way a support agent would, and its ranking can differ from the overall index.
- Conversational dynamics (Full Duplex Bench)
- The item AA groups as âconversational dynamics.â xAIâs announcement names the test Full Duplex Bench. It looks at handling back-and-forth such as interruptions in a conversation where listening and speaking overlap.
- Preference score (Arena Elo)
- A score built from people comparing two modelsâ answers and picking the better one. Like chess ratings it is relative, so read the order and gaps rather than the number itself. AA says the value is frozen when a model first becomes eligible for publication.
- Time to first audio
- How long it takes, once a person stops talking, for the modelâs first sound to come out. AA lists it in seconds. Shorter is needed for it to feel like human conversation. In real services, network and server location add to it.
- server_vad (server-side turn detection)
- VAD stands for Voice Activity Detection. With server_vad, the server judges from the audio whether the person has finished speaking, then lets the model answer. The session.update example in xAIâs docs uses this value.
- Ephemeral token
- A short-lived access key issued by your server. Putting the API key in a browser or app lets anyone extract it, so the server fetches an ephemeral token and hands that over instead. xAIâs voice docs recommend this for client-side apps.
- FRED Â· won per dollar (DEXKOUS)
- Official Fed H.10 statistics, checked 2026-10-15. 1,342.13 won on 2026-10-09, the latest value at the time. The Korean editionâs won figures use this rate. Source document â
