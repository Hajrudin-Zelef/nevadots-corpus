---
id: collect-261121-veille-ia/grok-xai/xai-ships-grok-voice-3-0-a-speech-to-speech-model-that-leads-an-independent-voice-index-at-an-unchan
title: "xAI ships Grok Voice 3.0, a speech-to-speech model that leads an independent voice index at an unchanged price"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: ["2026-10-14"]
keywords: ["voice", "grok", "model", "audio", "speech-to-speech", "index", "bench", "spacexai"]
source: https://aipolix.com/en-us/news/xai-grok-voice-3-speech-to-speech-model
source_anchor: ""
source_lines: [1, 13]
section: "Grok_xAI"
---
SpaceXAI, the company that builds Grok and runs the x.ai developer platform, released Grok Voice 3.0 on October 14, 2026. It is a speech-to-speech model, meaning it listens to audio and answers in audio without a separate transcription and text-to-speech chain, and it is aimed at voice agents such as customer-support lines and in-car assistants. The company calls it the most intelligent voice model available, a claim that rests largely on third-party benchmark results published the day before launch.

Those results come from Artificial Analysis, which runs a public leaderboard for real-time voice models. Its composite Speech-to-Speech Index gives Grok Voice 3.0 a score of 86.9, ahead of OpenAI's GPT-Live-1 (Astra, medium) at 83.2, Google's Gemini 3.8 Live Extended Thinking (High) at 82.6 and xAI's own previous model, Grok Voice 2.0, at 81.3. The index blends spoken reasoning on Big Bench Audio, the τ-Voice support-call test, and preference and task-success data from blind head-to-head calls. On the individual tests, the new model posts 98.5% on Big Bench Audio, 97.0% on Full Duplex Bench, which checks turn-taking, pauses and interruptions, 94.8% task success and an Elo of 1,070 in the Speech Agent Arena.

SpaceXAI also says Grok Voice 3.0 scores 71.6% on τ-Voice, roughly level with GPT-Live-1 (Astra) while responding with about 40% less latency, and that it answers in under one second. Citing Artificial Analysis cost measurements, the company says the model is cheaper per completed task than GPT-Live-1 (Astra, Medium) and GPT-Realtime-2.1. On EVA-Bench, ServiceNow's open benchmark of more than 200 enterprise voice-agent scenarios, it reports the top accuracy score of 74%, and a ServiceNow product director is quoted endorsing that result. These latency, cost and EVA-Bench figures were published by SpaceXAI and were not checked independently during this review.

Pricing has not moved. Grok Voice 3.0 costs $0.08 per minute, or $4.80 per hour, in the Voice Agent API, identical to Grok Voice 2.0. Developers who call the grok-voice-latest alias were moved to the new model automatically, so existing integrations pick it up without code changes, which also means their agents may behave differently from one day to the next.

The company leans on deployments inside Elon Musk's other businesses as evidence of scale. It says the model handles millions of conversations each day in Tesla vehicles, where drivers can check calendars, answer email or place orders hands-free, and that it now powers the voice mode of Grok Bot. At Starlink, according to SpaceXAI, voice agents built on Grok Voice field about 20,000 customer calls daily in several languages, close roughly 70% without a human, and convert one in four sales calls into a purchase during the call. None of these operating numbers has been audited publicly.

The broader leaderboard adds useful perspective. Artificial Analysis evaluates more than 40 voice models, and on its single-metric tables Grok Voice 3.0 is fourth rather than first: StepAudio 3 Realtime leads Big Bench Audio with 99.7% and Full Duplex Bench with 98.9%, followed on both by Alibaba's Qwen Audio 3.0 Realtime Plus. The comparison chart in the launch post covers only OpenAI and Google models. Qwen Audio 3.0 Realtime Plus is also listed as the cheapest model for input audio.

For builders of phone and in-car agents, the practical change is a stronger default model at the same per-minute rate. Whether the composite lead holds up on real call traffic, accents and noisy lines will depend on results from customers outside Tesla and Starlink, which the launch did not include.
