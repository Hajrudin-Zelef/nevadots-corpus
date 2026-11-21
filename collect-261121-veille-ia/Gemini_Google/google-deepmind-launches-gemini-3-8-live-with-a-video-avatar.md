---
id: collect-261121-veille-ia/gemini-google/google-deepmind-launches-gemini-3-8-live-with-a-video-avatar
title: "Google DeepMind launches Gemini 3.8 Live with a video avatar"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["avatar", "live", "google", "gemini", "video", "exchange", "says", "speech"]
source: https://www.aiaffairs.com/technology/google-deepmind-gemini-3-8-live-avatar-enterprise/
source_anchor: ""
source_lines: [1, 31]
section: "Gemini_Google"
---
Google DeepMind introduced Gemini 3.8 Live with Live Avatar on 24 September 2026, adding generated video to its live conversational model for customers using Gemini Enterprise. The company says the feature combines speech with a visual persona that can respond to what it hears and sees, while fetching information in the background without stopping the conversation. It was announced by Shuo-yiin Chang, a research scientist, and CJ Zheng, a software engineer, on behalf of the Gemini Audio Team.

## Key points

- Google says Live Avatar generates video alongside speech, with lip movements and expressions matched to the conversation.
- The feature can call tools and retrieve data in the background while dialogue continues, according to Google.
- Live Avatar became available in Gemini Enterprise on 24 September; pre-made characters and custom avatars are options.

## Gemini 3.8 Live gives speech a face

Google describes Live Avatar as a combination of its live dialogue capabilities and a low-latency stream of generated video. In a voice-only exchange, a response can be heard but has no visible speaker. Here, the model produces a moving character alongside its speech, with lip movements and facial expressions that Google says follow the conversation. The company also describes fluid turn-taking: the avatar is intended to respond in an exchange rather than deliver a prepared video clip after each prompt.

The input is multimodal too. Google says Live Avatar processes sight and sound at the same time, then responds with generated audio and video in near real time. That matters for the customer-service conversations and interactive walkthroughs Google proposes, because a response could draw on something visible as well as something spoken. The published announcement describes those capabilities and points to demonstrations.

Google launched the underlying Gemini 3.8 Live model the week before the avatar announcement. The addition also follows its 23 September release of Gemini 3.8 Flash TTS and Flash-Lite TTS, text-to-speech models supporting more than 100 languages. Those models generate speech from text; Live Avatar is aimed at an ongoing exchange in which generated speech has a visible character attached to it. The two releases address different parts of a spoken interaction.

## Background tool calls keep Live Avatar talking

Live Avatar can call a tool asynchronously, Google says, letting it request data while it continues to talk. A tool call is a request to another system for information or an action; making it asynchronous means the conversation need not stop while that request is handled. Google uses a hotel guest check-in to demonstrate the feature, with the avatar maintaining the exchange as it calls tools in the background. The distinction is important whenever an answer depends on information the conversational model must fetch rather than produce from the exchange alone.

Checking in at a hotel could involve a spoken exchange that carries on while information for the check-in is retrieved. If the background call works as Google describes, it would allow the avatar to keep responding during that wait, rather than making the exchange pause for the request to finish.

Google says dialogue can continue while Live Avatar makes background tool calls. The company describes a hotel check-in demonstration in which the avatar calls tools as the exchange continues. For a company building a service around the avatar, the capability at issue is whether a conversation remains usable while the data it needs comes from elsewhere.

## Gemini Enterprise offers pre-made and custom avatars

Google made Gemini 3.8 Live with Live Avatar available in Gemini Enterprise from 24 September. Characters can be selected from a pre-made library, while a custom avatar can be deployed when enterprise allow-listing is enabled, The Register reported. The publication also reported that the avatar can converse in 97 languages. Those options give companies a choice between an existing character and one admitted through an enterprise control, alongside the ability to build conversations for different languages.

A face adds another consideration to that choice. A 2021 DeepMind paper warned that human-like behaviour could lead people to overestimate a conversational system’s abilities, The Register reported. Its concern was not confined to whether an imitation looked convincing: people might also infer qualities such as empathy or consistent reasoning from the way a system presents itself. Live Avatar’s expressions and lip-syncing put that older concern directly alongside the capabilities Google is offering to enterprises.

Chang and Zheng said Google had built safeguards intended to protect identity and make generated content transparent. In research published in December 2025, Google researchers reported that technology workers in focus groups worried that human-like language and tone could give a false impression of reliability and obscure errors, The Register reported.
