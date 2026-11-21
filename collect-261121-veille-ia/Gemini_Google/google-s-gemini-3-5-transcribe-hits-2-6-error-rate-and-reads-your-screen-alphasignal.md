---
id: collect-261121-veille-ia/gemini-google/google-s-gemini-3-5-transcribe-hits-2-6-error-rate-and-reads-your-screen-alphasignal
title: "Google's Gemini 3.5 Transcribe Hits 2.6% Error Rate and Reads Your Screen | AlphaSignal"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["google", "gemini", "transcribe", "model", "screen", "audio", "streaming", "inside"]
source: https://alphasignal.ai/news/google-s-gemini-3-5-transcribe-hits-2-6-error-rate-and-reads-your-screen
source_anchor: ""
source_lines: [1, 51]
section: "Gemini_Google"
---
## Takeaways

- Google launched Gemini 3.5 Transcribe, its most precise speech-to-text model, in public preview.
- Reports 2.6% WER on pre-recorded audio and 4% WER streaming per Artificial Analysis benchmarks.
- Inside Google Antigravity, it uses screen context and chat history for accurate filename and identifier transcription.
- Ships as two APIs: `gemini-3.5-transcribe-live` for streaming,`gemini-3.5-transcribe` for recorded audio.
- Auto-detects 85+ languages, handles up to three speakers with word-level timestamps, removes filler words.
- Available in AI Studio, Gboard Rambler, Gemini macOS app, coming soon to Chrome.

Google has rolled out Gemini 3.5 Transcribe, a speech-to-text model that ships inside the Google Antigravity agentic development environment and several other Google surfaces. The pitch goes beyond dumping words on a screen: it cleans up filler, respects self-corrections, and, inside Antigravity, looks at what is on your screen before deciding how to spell things.

Inside Antigravity, 3.5 Transcribe pairs screen context and chat history, with your permission, to sharpen transcription of file names, agent thoughts, and active documents. That matters for anyone driving an agent by voice. When you say a variable name or a filename out loud, the model has a real shot at getting it right because it can see the same tokens you are looking at.

## Benchmarks Google is putting on the table

The headline numbers come from Artificial Analysis. Google claims 4% word-error-rate (WER) on streaming audio and 2.6% WER on pre-recorded files across real-world conditions, including background noise and conversational AI interactions. On the multilingual FLEURS benchmark, Google reports 5.50% WER streaming and 5.04% non-streaming, with time to final transcription improved by 70% over its previous Chirp 3 model.

Language coverage spans more than 85 languages, regional accents, and dialects. The system handles real-time language detection and identifies up to three distinct speakers in recorded files, with support for larger groups still experimental.

## Two APIs, two jobs

Google split the model into two developer surfaces instead of stuffing everything behind one endpoint:

- **Live API** using`gemini-3.5-transcribe-live` for continuous bidirectional streaming with sub-second latency. Aimed at voice agents, live captions, and anything that has to respond as the user talks.
- **Interactions API** using`gemini-3.5-transcribe` for pre-recorded audio, meetings, and call logs, with speaker attribution and word-level timestamps.

Both sit in public preview. Developers get access through the Gemini API via Google AI Studio and Google Antigravity. Enterprises get it through the Gemini Enterprise Agent Platform, with Gemini Enterprise for Customer Experience coming soon. Pricing details for the preview were not published in the launch materials.

## What the model does differently

Most ASR pipelines hand back a raw string and leave formatting, punctuation, and cleanup as your problem. Gemini 3.5 Transcribe does that work inside the transcription step itself, converting raw audio directly into formatted text and handling background noise, jargon, and disfluency in one pass.

In practice, that covers a few concrete behaviors:

- **Self-corrections resolved in place.** Say "let's meet Tuesday, no, Wednesday" and the output reads Wednesday.
- **Filler removal.** Ums and ahs get stripped without a separate cleanup pass.
- **Custom vocabulary.** You can feed in domain jargon and unique spellings so medical terms, product SKUs, or codebase identifiers survive intact.
- **Alphanumeric strings.** Order numbers, postal codes, and similar mixed sequences reportedly hold up in noisy conditions.
- **Function calling.** The model can delegate tasks like image generation to other Gemini models via function calls. That capability already ships in the Gemini macOS app for developers, and API support is expected soon.

## Where it shows up in practice

Beyond Antigravity, the model powers Rambler on Android's Gboard, the Gemini app on macOS, and Google AI Studio's Build mode for voice-driven prototyping. Google says Gemini 3.5 Transcribe will soon let you dictate into any web field in Chrome.

For anyone building on top of it, Google is leaning on the ecosystem already around the Live API. Partner platforms including Agora, Fishjam, LangChain, LiveKit, Pipecat, Vercel, and Vision Agents have integrations that handle the streaming media plumbing, so you can point a voice agent framework at the model without writing your own WebRTC layer.

## Why agent workflows benefit

Context awareness is the practical breakthrough. Anyone who has tried to dictate code, filenames, or agent instructions to a standard speech-to-text engine knows the failure mode: half the tokens get autocorrected into English words that do not exist in your project. By feeding the transcription model the same screen state the user is looking at, Antigravity biases decoding toward tokens that actually appear in the workspace.

The 2.6% offline WER puts the model in the same conversation as the best proprietary transcription systems, and the 4% streaming figure holds up for a general-purpose model that also covers 85 languages and speaker diarization. For teams already building voice-driven agents or meeting tools on Google's stack, swapping in the new endpoint is a small code change with a plausible accuracy win.
