---
id: collect-261121-veille-ia/gemini-google/google-gives-gemini-3-8-live-a-face-with-live-avatar
title: "Google Gives Gemini 3.8 Live a Face With Live Avatar"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["google", "live", "avatar", "video", "gemini", "model", "audio", "latency"]
source: https://rits.shanghai.nyu.edu/ai/gemini-3-8-live-avatar/
source_anchor: ""
source_lines: [1, 36]
section: "Gemini_Google"
---
**On September 24, 2026, Google released Gemini 3.8 Live with Live Avatar**, which adds near real-time generated video to its speech-to-speech dialogue model. The avatar has a face that lip-syncs to the voice, changes expression, and takes turns in the conversation, so an enterprise voice agent can now appear as a video call. The feature is generally available in Gemini Enterprise through US and EU endpoints. Google says it was first previewed at Google Cloud Next 2026.

## What Google Shipped

Gemini 3.8 Live is Google’s native speech-to-speech foundation model. Audio goes in and audio comes out, with no separate transcription and text-to-speech stages in between. Live Avatar adds a video-generation layer on top of that model. According to Google Cloud’s announcement, the combined system offers:

- **Video avatars** that lip-sync to the generated speech with “natural expressions and fluid turn-taking.”
- **Interruption handling** : the model recovers when a user talks over it, as you would expect from a speech-native model.
- **Asynchronous tool calling** : API calls run in the background while the avatar keeps talking, so the conversation doesn’t stall on lookups.
- **Live visual input** : the model processes camera feeds and screen shares alongside the audio.
- **97 languages** with automatic detection. The Google blog post says users can switch languages mid-conversation “without video degradation or visual drift,” and lip movements adapt to the new language.

## Custom Avatars and Safeguards

Customers can pick from a library of preset avatars. Custom avatars are built from system instructions, a single reference photo, and an audio sample, and they are available only through what Google calls a “strict enterprise allowlisting and verification process.” Organisations request access through Google Cloud sales rather than self-serve.

All generated audio and video carry **SynthID** watermarks, which Google describes as imperceptible and “woven directly into the audio and video output.” Google has published no latency figures, benchmarks, or pricing for Live Avatar. The Engadget report also notes that pricing was not disclosed.

## Early Customers

Google’s announcement names four customers or partners:

- **Cox Automotive (Autotrader)** is building a conversational avatar for vehicle discovery that highlights parts of the screen and calls tools for search, comparison, and financing.
- **Equal AI** runs a personal assistant that, according to CEO Akhilesh Damaraju, “handles over a million live calls daily across nine Indian languages.” He said Gemini 3.8 Live “improved interruption handling, multilingual conversations, and tool-call reliability.”
- **Salesforce** is pairing Gemini 3.8 Live with Agentforce for customer service.
- **Specs** pointed to improvements in voice activity detection and overall latency.

## What This Means

The usual way to build a talking-head agent is to chain several systems: speech recognition, a language model, text-to-speech, and then a separate avatar-rendering service. Each step adds latency and another place for audio and video to fall out of sync. Google is folding the video layer into the same speech-native model that already handles turn-taking and interruptions. If that works as described, lip-sync and expression can follow the actual audio being generated rather than being fitted onto it afterwards. That matters most for mid-sentence language switching, which is where chained systems tend to break.

Several things are still unknown. Google has given no latency numbers, no pricing, and no evaluations. For a product whose selling point is “near real-time,” latency is the figure that decides whether a conversation feels natural. The custom-avatar pipeline also raises a likeness question: one photo plus one voice sample is enough to build a speaking video persona. Google’s controls here are the allowlist and SynthID. The allowlist limits who can build an avatar. SynthID only helps if someone actually runs a detector on the output, and an end user on a support call has no easy way to do that. Reception may also be mixed. Engadget’s coverage suggested that, given people’s “distaste for AI slop,” the avatars “may not be universally popular.”

For researchers and developers, the more immediately useful parts are probably underneath the avatar: asynchronous tool calls during speech, camera and screen input in the same session, and the open-source ADK sample agents Google published with the release.

*This post was drafted with AI assistance and reviewed by RITS staff.*
