---
id: collect-261121-veille-ia/grok-xai/ai-tools-grok-what-is-grok-voice-3-a30d5c8c
title: "Ai tools grok what is grok voice 3"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: []
keywords: ["grok", "voice", "speech", "model", "minute", "alias", "call", "pricing"]
source: ""
source_anchor: ""
source_lines: [1, 93]
section: "Grok_xAI"
---
What Is Grok Voice 3.0? xAI's Speech-to-Speech Model, Pricing and Benchmarks
Grok Voice 3.0 is xAI's flagship speech-to-speech model: it listens to a caller and talks back in one real-time audio session. xAI released it on October 14, 2026, at the same per-minute price as Grok Voice 2.0. Below: what changed for developers, what a session costs, and what the launch benchmarks do and do not prove.
What Grok Voice 3.0 is
xAI's Grok Voice 3.0 announcement markets it as "the world's most intelligent speech-to-speech model," with "class-leading latency at under one second." Those are xAI's own words, not an independent finding; the benchmark section below shows where the published tests support them and where they do not. Speech to speech means the model takes audio in and returns audio out inside one live session. You build a voice agent on it: a phone line, an in-car assistant or a support desk that hears the caller, reasons, calls tools, and answers out loud.
The model sits behind xAI's Speech to Speech API. It is not a new product you sign up for. If you already run a voice agent on Grok, the question is whether your sessions moved to the new model, and xAI's release notes answer that: sessions on the grok-voice-latest alias were upgraded automatically. For the rest of the Grok family, see our Grok breakdown.
What changed for developers
The short version from xAI's Speech to Speech documentation: a new model name, a moved alias, a deprecated predecessor, and no change to the API itself. Grok Voice 3.0 uses the same endpoint, session settings and events as Grok Voice 2.0, at the same price.
| Model name | Status after October 14 | What to do | 
|---|---|---|
| grok-voice-latest | Alias, now points to grok-voice-3.0 | Nothing. Sessions using the alias were upgraded automatically. | 
| grok-voice-3.0 | Flagship voice model | Pin this name if you want a fixed version that will not move under you. | 
| grok-voice-2.0 | Deprecated | If you pinned it, change the model to grok-voice-3.0 or the alias. | 
Alias or pin? The alias gave every agent on it a new model on launch day with no code change. That is convenient, and it also means your agent's behavior changed without a deploy on your side. xAI's docs say to "use a versioned name to pin to a specific release." If you run regression tests on a voice agent, pin one and move it on purpose after testing.
What it costs
Grok Voice 3.0 is billed per minute of session. The pricing page gives the per-minute rate but does not say what counts toward a billed minute, such as silent time while a call stays connected, or how a partial minute is rounded. We could not confirm either, so check the pricing page or ask xAI before you budget a high-volume deployment. These are the list prices on xAI's API pricing page, read on October 26, 2026. For Grok's subscription plans and the rest of the API price list, see our Grok pricing guide.
| Voice service | List price | 
|---|---|
| Speech to Speech, grok-voice-3.0 | $0.08 per minute ($4.80 per hour) | 
| Speech to Speech, grok-voice-2.0 (deprecated) | $0.08 per minute ($4.80 per hour) | 
| Speech to Text | $0.10 per hour (REST batch), $0.20 per hour (streaming) | 
| Text to Speech | $15.00 per 1M characters | 
Tools cost extra. When a voice agent uses a server-side tool that xAI runs, each call is billed on top of the minutes: web_search at $5 per 1,000 calls and Collections Search at $2.50 per 1,000 calls. x_search is billed per post and profile it returns rather than per call. The pricing page also lists other line items for Speech to Speech sessions, so read it before you budget. Pick a call pattern to see what the per-minute list price works out to, before any other line items. Each example assumes every minute of the call is billed.
What does a Grok Voice 3.0 call cost at $0.08 a minute?
One 5-minute call
5 × $0.08 = $0.40 for the session, before any tool calls.
A small support desk
Take 1,000 calls of 10 minutes. That is 10,000 minutes. 10,000 × $0.08 = $800 in session minutes. Your real number depends on average call length, which is worth measuring before a pilot.
An hour of session time
60 × $0.08 = $4.80, which is the hourly figure xAI publishes.
Agents that search the web
If each of those 1,000 calls runs two web searches, that is 2,000 calls at $5 per 1,000, or $10, on top of the session minutes. Tool fees are small next to minutes at this volume, but they scale with how chatty your agent is with its tools.
For ways to keep Grok API spend in check across models, see our guide to Grok API cost optimization.
What the benchmarks show
The launch post's numbers come from Artificial Analysis, an independent benchmarking firm, dated October 13, 2026. Independent measurement is a real strength. Two cautions still apply: xAI chose which rivals and which settings to show, and the live leaderboard keeps moving, so some values there already differ from the launch table. A benchmark also tells you about its test calls, not yours.
| Measure (higher is better) | Grok Voice 3.0 | Grok Voice 2.0 | GPT-Live-1 (Astra, Medium) | Gemini 3.8 Live (Ext. Thinking, High) | 
|---|---|---|---|---|
| AA Speech-to-Speech Index | 86.9 | 81.3 | 83.2 | 82.6 | 
| Big Bench Audio (speech reasoning) | 98.5% | 97.2% | 90.1% | 97.7% | 
| Speech Agent Arena Elo (human preference) | 1070 | 1011 | 1048 | 990 | 
| Task Success Rate | 94.8% | 94.6% | 87.4% | 89.1% | 
| Full Duplex Bench (pauses, turn-taking, interruptions) | 97.0% | 95.1% | 94.9% | 91.9% | 
| τ-Voice (noisy support calls with accents and interruptions) | 71.6% | 56.5% | 74.5% | 68.6% | 
Source: Artificial Analysis, October 13, 2026, as published in xAI's announcement. Check the live leaderboard for current values. The Elo row is a preference rating from blind head-to-head calls. On Task Success Rate, 3.0 and 2.0 are 0.2 points apart, so that row shows no real change.
Read the τ-Voice row closely
τ-Voice is the test closest to a real support line: diverse accents, background noise, and callers who interrupt. Here Grok Voice 3.0 is not first. It scores 71.6% against 74.5% for GPT-Live-1 (Astra, Medium). xAI's claim is narrower: comparable accuracy "at 40% lower latency." The jump over Grok Voice 2.0 (56.5%) is the larger story for existing Grok users.
Cost per task and EVA-Bench
xAI's announcement also includes a chart of average cost per task, credited to Artificial Analysis's Big Bench Audio run. As xAI presents it, Grok Voice 3.0 comes in at $0.025 per task, against $0.030 for GPT-Live-1 (Astra, Medium) and $0.055 for GPT-Realtime-2.1 (High). xAI picked those two comparisons, and cost per task on a benchmark is not the same as your per-minute bill. Separately, on ServiceNow's open-source EVA-Bench, which scores voice agents on both accuracy and conversation experience across 200+ enterprise scenarios, xAI reports Grok Voice 3.0 reached the highest accuracy at 74%. A ServiceNow product leader quoted in xAI's post calls it a new Pareto frontier, meaning no other model scored better on accuracy and experience at once. ServiceNow publishes the framework, so you can run it yourself.
Before you commit. Record a sample of your own calls, with your accents, your line noise and your product names, and run them through the models you are choosing between. A one-point index lead does not predict how an agent handles your refund policy.
Where it runs today
xAI points to Tesla and Starlink as proof at scale. Treat these as vendor-reported operating figures, not independent case studies. For how Grok connects to Tesla, SpaceX and X more broadly, see how Grok integrates with Tesla, SpaceX and X.
- Tesla vehicles. xAI says Grok Voice 3.0 handles millions of conversations a day in Tesla vehicles. In the announcement's examples, drivers can use Connectors for simple requests like checking their calendar or answering an email, and Grok Bot for bigger tasks like ordering a coffee or booking dinner. xAI also says the model powers Grok Bot's voice mode.
- Starlink call centers. According to xAI's announcement, Grok Voice answers 20,000 customer calls a day across multiple languages. xAI says roughly 70% are resolved with no human in the loop, and one in four sales calls ends in a purchase before the caller hangs up.
The Starlink example is the more useful one for buyers, because it describes an AI agent taking actions rather than chatting. xAI says a single agent built on Grok Voice 3.0 verifies customers, troubleshoots hardware, issues credits and ships replacement equipment. Those are the steps where a wrong action costs money, so they are where your own pilot should focus its checks.
How you build on it
A session is a live WebSocket connection to wss://api.x.ai/v1/realtime. Audio and text flow both ways, and you configure the agent by sending a session.update event. The settings below are listed in xAI's Speech to Speech reference.
The pieces that matter most for a voice agent:
- Authentication. Server code passes an xAI API key as a Bearer token. Browser and mobile apps should use ephemeral tokens, short-lived credentials that keep your API key off the device.
- Turn-taking. Setting turn detection to server_vad (server-side voice activity detection) lets the server detect when the caller has finished speaking, and when they interrupt.
- Tools. The agent can use web_search ,x_search and your own functions through function calling during a live call.
- Reasoning. reasoning.effort defaults to high, and can be set to none.
- Phones. SIP phone calls route calls from phone systems into a session, and the docs list native support for the G.711 mu-law and A-law codecs used on telephone lines. The docs name SIP, WebSocket and LiveKit as connection options and include a Twilio phone-agent example.
- Voices. Pick a built-in voice or a custom voice cloned from a reference clip up to 120 seconds long. According to xAI's Custom Voices documentation, the API endpoint for creating them is gated to teams on an Enterprise plan. The same page says Custom Voices is currently only available in the United States, with the exception of Illinois.
Least privilege for voice agents. Give the agent only the tools a call needs, and keep actions that move money or ship hardware behind your own checks in the function you expose. Never put the long-lived API key in a browser or app; that is what ephemeral tokens are for.
Limits and open questions
xAI's Voice overview states that with Zero Data Retention, audio "is processed in real time and never stored or used for training," and lists SOC 2 Type II, HIPAA eligibility with a Business Associate Agreement (BAA), and GDPR compliance with EU data residency options. Confirm which of these apply to your contract. Several other things are not in the launch material:
Language support is stated loosely
The launch post says only that Starlink runs "across multiple languages." The Speech to Speech docs page says the API "supports 20+ languages." Test the ones you need.
Session limits not in the announcement
The launch post does not state a maximum context or session length. Check the API reference before designing long calls.
Consumer privacy not covered
The announcement does not describe retention for drivers in Tesla vehicles or callers to Starlink. The Zero Data Retention statement is about xAI's Voice APIs.
Selected comparisons
The rivals and their settings were chosen by xAI, and the Tesla and Starlink numbers come from companies tied to xAI.
Open questions worth tracking: how the live leaderboard settles over the coming weeks, and independent EVA-Bench runs by other teams. We will update this page as xAI publishes more.
FAQ
What is Grok Voice 3.0?
Grok Voice 3.0 is xAI's flagship speech-to-speech model, released on October 14, 2026. It powers real-time voice agents through xAI's Speech to Speech API under the model name grok-voice-3.0, and it runs Grok Bot's voice mode and voice conversations in Tesla vehicles.
How much does Grok Voice 3.0 cost?
xAI lists Speech to Speech with grok-voice-3.0 at $0.08 per minute, or $4.80 per hour, the same price as Grok Voice 2.0. Server-side tools such as web_search are billed per call on top, at $5 per 1,000 calls.
Do I need to change my code to use Grok Voice 3.0?
Not if you use grok-voice-latest: that alias now points to grok-voice-3.0, and those sessions were upgraded automatically. If you pinned grok-voice-2.0, which is now deprecated, change the model name. xAI says the endpoint, session settings and events are unchanged.
Is Grok Voice 3.0 the best voice model?
It ranked first on the Artificial Analysis Speech-to-Speech Index at launch, with 86.9 in the October 13, 2026 results xAI published. It did not lead every test: on τ-Voice, GPT-Live-1 (Astra, Medium) scored 74.5% to its 71.6%.
Does xAI store voice API audio?
xAI's Voice documentation describes Zero Data Retention, under which audio is processed in real time and never stored or used for training. Confirm the terms that apply to your account, since the launch post does not cover consumer use in Tesla vehicles or Starlink calls.
Sources & further reading
The xAI pages and independent benchmark references behind this article, all read on October 26, 2026.
xAI announcement and documentation
Independent benchmarks
Video resources
Grok Voice 3.0 is new, so these are topic searches rather than picked videos. Check each video's upload date before relying on it.
More on Grok
Resources from across Tech Jacks Solutions
