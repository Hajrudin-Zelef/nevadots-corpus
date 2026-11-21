---
id: collect-261121-veille-ia/gpt-openai/voice-agent-apis-compared-gpt-live-grok-voice-voxtral-gemini
title: "Voice agent APIs compared: GPT-Live, Grok Voice, Voxtral, Gemini"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["voice", "model", "audio", "openai", "session", "backend", "gemini", "agent"]
source: https://infro.io/blog/voice-agent-apis-compared-2026
source_anchor: ""
source_lines: [1, 61]
section: "GPT_OpenAI"
---
A voice agent API takes live audio in and sends spoken audio back, and the four vendors selling one in October 2026 bill it in four different units. OpenAI's GPT-Live 1 charges $0.05 per minute of session, billed per second, plus whatever its backend model costs<sup>1</sup>. OpenAI's gpt-realtime-2.1 charges per audio token<sup>2</sup>. xAI's Grok Voice 3.0 is $0.08 per minute<sup>3</sup>. Mistral's new Voice Agents pipeline is $0.04 or $0.08 per minute of conversation<sup>4</sup>, and Google's Gemini 3.8 Live lists per-minute equivalents of token prices<sup>5</sup>. None of those numbers is the cost of a call. This guide puts them on one table, works through what a five-minute call costs on each, and sets out when a native speech-to-speech model beats a pipeline you assemble yourself.

OpenAI's own voice agent guide draws the distinction that matters for every vendor here<sup>6</sup>:

- **Full duplex with a separate backend.** A voice model listens and speaks at the same time and hands reasoning and tool use to a backend model or agent you choose. This is GPT-Live: OpenAI calls the hand-off delegation, and the user can keep talking while backend work runs<sup>7</sup> .
- **One speech-to-speech model.** A single model hears audio, decides, calls tools and answers in speech in one session. This is the Realtime API (gpt-realtime-2.1), Grok Voice 3.0 and Gemini 3.8 Live.
- **A managed cascade.** The vendor runs speech-to-text, a text model and text-to-speech as one product. Mistral's Voice Agents SDK does this: it transcribes the user, generates an answer with a model, speaks it and plays it back, and Mistral says plainly that "each one adds to the time the user waits for a reply"<sup>8</sup> .
- **Your own cascade.** You call a transcription API, a text model and a speech API yourself. OpenAI's guide recommends this when you need to "inspect or transform intermediate text", for example to run policy checks before the agent responds<sup>6</sup> .

| API | Architecture | Price unit | List rate | Billed separately | 
|---|---|---|---|---|
| OpenAI GPT-Live 1 | Full duplex + backend | Session time, per second | $0.05 / minute | Backend model and tools at their own rates <sup>1</sup> | 
| OpenAI gpt-realtime-2.1 | Speech-to-speech | Tokens | Audio $32 in, $64 out, $0.40 cached per 1M; text $4 in, $24 out | Input transcription, if enabled <sup>9</sup> | 
| OpenAI gpt-realtime-2.1-mini | Speech-to-speech | Tokens | Audio $10 in, $20 out, $0.30 cached per 1M | Input transcription <sup>10</sup> | 
| xAI Grok Voice 3.0 | Speech-to-speech | Minutes | $0.08 / minute ($4.80 / hour) | Not stated on the pages read <sup>3</sup> | 
| Mistral Voice Agents | Managed cascade | Conversation minutes | $0.04 / min with Mistral Small 4; $0.08 / min with larger models | Not stated on the pages read <sup>4</sup> | 
| Google Gemini 3.8 Live | Speech-to-speech | Tokens, with per-minute equivalents | Audio in $3.00 per 1M or $0.005 / min; audio out $12.00 per 1M or $0.018 / min | Google Search grounding: 5,000 free a month, then $14 per 1,000 <sup>5</sup> | 

The units are not interchangeable, and each one hides a different cost driver.

**Session time (GPT-Live).** OpenAI's cost guide says active session time includes time when the user speaks, the assistant speaks, "both are silent, or the backend is working". Muting the microphone does not close the session. Creating a WebRTC session bills 15 seconds while it initialises, applied against the session's duration once it runs, so a reconnect is not free<sup>9</sup>. The guide's own formula is voice seconds divided by 60 times the rate, plus backend costs.

**Tokens (Realtime, Gemini Live).** OpenAI counts user audio at one token per 100 ms and assistant audio at one token per 50 ms, and "the entire conversation is sent to the model for each Response", so later turns cost more than early ones; prompt caching applies automatically and is best-effort<sup>9</sup>. Gemini 3.8 Live prices are per million tokens with per-minute equivalents beside them, and its output price includes thinking tokens<sup>5</sup>.

**Minutes (Grok Voice, Mistral).** A flat per-minute rate is the easiest to forecast. The pages read do not say whether tool calls such as Grok's built-in `web_search` and `x_search`<sup>11</sup>, or Mistral's server-side web search<sup>8</sup>, add charges of their own; ask before you budget a tool-heavy agent.

- **Grok Voice 3.0 (14 October).** Same endpoint, session parameters and events as 2.0, same price;`grok-voice-latest` now points to it, so sessions on the alias moved automatically, and`grok-voice-2.0` is deprecated with no date given<sup>12</sup><sup>11</sup> .
- **GPT-Live prompt-to-voice (16 October).** You can create a custom voice from a text description through`/v1/audio/voices` . Creating it is free; speech in it bills at normal GPT-Live session rates. Prompt-generated voices work with GPT-Live, not with OpenAI's Text to Speech API. Nine more built-in voices for German, French, Hindi, Japanese and Korean arrived the same day<sup>13</sup><sup>14</sup> .
- **Mistral Voxtral 3 and Voice Agents (20 October).** Four Voxtral 3 models in public preview: a text-to-speech model, two offline transcription models and a realtime transcription model, released the same day as the Voice Agents pipeline<sup>15</sup> . Phone calls over SIP are available to enterprise customers at launch, through the account team<sup>8</sup> .
- **Legacy OpenAI realtime models.**`gpt-realtime` and`gpt-realtime-mini` shut down on 20 January 2027, replaced by`gpt-realtime-2.1` and`gpt-realtime-2.1-mini`<sup>16</sup> .

Gemini 3.8 Live and Gemini 3.8 Live Extended Thinking went GA on 15 September; Google describes the first as the default for low-latency voice agents and the second for background reasoning during a conversation<sup>17</sup>.

Two practical conclusions follow. First, for short calls the flat per-minute products are in the same range, and the backend model behind GPT-Live can matter as much as the voice rate. OpenAI's guide makes the point itself: a larger backend model can cost less overall "if it completes the task faster and the voice-session savings exceed its additional token costs"<sup>9</sup>. Second, for long calls the token-billed products change shape: cost per minute rises with the conversation unless you truncate. OpenAI documents a `truncation` setting with `retention_ratio` and a token limit for exactly this<sup>9</sup>.

- **Gemini Live session lifetime.** Without context window compression, audio-only sessions end after 15 minutes and audio-video sessions after 2 minutes. A connection lasts about 10 minutes; session resumption carries one session across connections, and resumption tokens are valid for 2 hours after the session ends<sup>18</sup> . Audio goes in as 16 kHz PCM and comes out as 24 kHz PCM over a WebSocket<sup>19</sup> .
- **GPT-Live concurrency.** Rate limits are counted in concurrent sessions: 50 on the Build tier, 300 on Launch and 500 on Grow; the Free tier is not supported<sup>1</sup> .
- **gpt-realtime-2.1 context.** 128,000 tokens of context and 32,000 maximum output tokens<sup>2</sup> ; at one token per 100 ms of caller audio, a long call reaches truncation sooner than a text chat would.
- **Grok Voice transport.** A WebSocket to`wss://api.x.ai/v1/realtime` ; browsers authenticate with an ephemeral token, and session resumption is opt-in<sup>11</sup> .

No vendor publishes a guaranteed figure, and nothing here was measured. xAI says Grok Voice 3.0 has "class-leading latency at under one second"<sup>3</sup>. Mistral says its realtime transcription model runs "at sub-500ms latency in over 35 languages"<sup>20</sup>, but that is one stage of a cascade whose stages, in its own words, add up<sup>8</sup>. OpenAI declines to quote a number and tells you to measure audible response time at the median and 95th percentile, holding the caller, recording, backend model and transport fixed when you compare<sup>6</sup>. That is the right test for all four.

| Requirement | Native speech-to-speech | Cascaded pipeline | 
|---|---|---|
| Natural interruptions and turn-taking | Stronger: one model hears and speaks | Weaker: end-of-turn detection is a separate stage | 
| Inspect, redact or approve text before it is spoken | Hard: there is no text step to hold | Natural: the text exists between stages | 
| Swap one component (a new transcription model, another voice) | Not possible; the model is the stack | Change one stage, keep the rest | 
| Predictable cost | Flat-rate products, yes; token-billed ones vary with call length | Three line items, each with its own unit | 
| Audit trail | Transcripts are an add-on (OpenAI bills input transcription separately) | The transcript is the pipeline's own input | 

The cascade loses on feel: every stage adds to the wait, and turn detection becomes your problem unless a managed product such as Mistral's handles it. The native model loses on control: if a regulated workflow needs a policy check between understanding and speaking, a single speech-to-speech model gives you nowhere to put it. GPT-Live's split keeps the native feel for the conversation and moves the rules into a text backend you control<sup>7</sup>, which is why its backend cost belongs in every comparison.

- What is our median call length, and how much of it is silence or tool waits? That decides between per-minute and per-token billing.
- Which backend model will answer, and what does it cost per completed task, retries included?
- Do we need a text checkpoint for compliance, and if so, where?
- What happens at 15 minutes on Gemini, at a dropped connection on any of them, and how do we resume?
- Which voice and transcription vendors will we fall back to if our first choice degrades during a live call?

INFRO is a control plane between your applications and the provider accounts you already hold; the providers bill you directly, and INFRO's charge is a software subscription. It does not serve realtime or live voice sessions: GPT-Live, the Realtime API, Grok Voice and Gemini Live connect straight to the vendor, and their session minutes do not appear in INFRO.

What it can carry is everything around the voice layer. The backend model a GPT-Live agent delegates to through client delegation is an ordinary chat request, and the stages of a cascaded pipeline are `POST /v1/audio/transcriptions`, `POST /v1/chat/completions` and `POST /v1/audio/speech` on INFRO's gateway, each served by your own connection. For those calls INFRO records the model, latency and estimated provider cost of every request, groups a call's requests under one trace id, checks budgets before the provider is called, and retries on another of your connections when a provider fails before the response starts. It does not judge how natural a voice sounds, and it does not price minutes it never sees.
