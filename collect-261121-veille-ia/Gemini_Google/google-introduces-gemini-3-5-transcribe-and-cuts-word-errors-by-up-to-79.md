---
id: collect-261121-veille-ia/gemini-google/google-introduces-gemini-3-5-transcribe-and-cuts-word-errors-by-up-to-79
title: "Google Introduces 'Gemini 3.5 Transcribe' and Cuts Word Errors by Up to 79%"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["audio", "gemini", "model", "google", "transcribe", "live", "transcription", "through"]
source: https://www.zeniteq.com/google-introduces-gemini-3-5-transcribe-and-cuts-word-errors-by-up-to-79-2grcad
source_anchor: ""
source_lines: [1, 68]
section: "Gemini_Google"
---
Google introduced Gemini 3.5 Transcribe on August 25, 2026, positioning it as a specialized AI model for accurate, low-latency speech recognition. It is available in public preview through the Gemini API and Google AI Studio, with separate options for live audio and prerecorded files.

The headline performance claim is substantial. Google says the model delivers up to 79% lower word error rates for streaming transcription and up to 48% lower rates for non-streaming audio than leading specialized and general-purpose models.

Accuracy is only part of the release, however. Gemini 3.5 Transcribe can also interpret instructions about how a transcript should be produced, allowing an application to clean up disfluencies, organize speech, identify speakers, or adapt its output to a particular workflow.

## Live and Recorded Audio Use Different Endpoints

Gemini 3.5 Transcribe is based on Gemini 3.5 Flash, but it is not a single API endpoint serving every transcription scenario. Google provides one model for continuous audio through the Live API and another for recorded audio through the standard `generateContent` API.

The Gemini 3.5 Transcribe model documentation describes both versions as audio-input, text-output models with support for 91 languages. Their formats, limits, and feature sets differ.

| Mode | Intended Use | Main Limit | Notable Capabilities | 
|---|---|---|---|
| Live API | Calls, captions, assistants, meetings | 15-minute sessions | Streaming output, language switching, prompting, tool calls | 
| Recorded audio | Interviews, podcasts, media archives | 120 minutes per request | Speaker diarization, word-level timestamps, smart formatting | 

The live model accepts raw 16-bit PCM audio at 16 kHz in mono. Applications using browser, mobile, or telephony audio may therefore need to resample or convert incoming streams before sending them to the API.

The recorded-audio version supports WAV, MP3, AIFF, AAC, OGG, and FLAC files. Its two-hour limit is sufficient for many interviews, lectures, and podcast episodes, although longer recordings will still require segmentation. Live sessions present a stricter engineering constraint because they currently end after 15 minutes and do not support session resumption.

## Smart Transcription Produces Usable Text

Google’s more distinctive addition is a feature it calls Smart transcription. Instead of always returning a literal stream of recognized words, developers can use natural-language prompts to control how the model handles the audio.

The official audio transcription guide shows that prompts can ask the model to remove filler words, correct obvious false starts, improve punctuation, and organize the result into readable paragraphs. Developers can also supply contextual information, such as expected names or specialized vocabulary, through the prompt.

That could reduce the amount of post-processing required in meeting assistants, customer-support systems, media workflows, and clinical or legal note-taking tools. A recording can move directly from raw audio to a structured document rather than passing through a separate cleanup model.

It also creates an important distinction between **verbatim transcription** and **edited transcription**. If the model is instructed to rewrite fragmented speech into polished prose, the result may be easier to read but less suitable as an exact record. Applications in regulated or high-stakes environments should preserve the original audio and clearly label any AI-cleaned version.

For prerecorded files, Gemini 3.5 Transcribe can add word-level timestamps and speaker diarization, which separates dialogue by participant. The live model focuses instead on continuous streaming and can detect when a speaker changes languages during a session. Google says this includes code-switching within a conversation, not merely selecting one language at the beginning.

## The Accuracy Gains Need Benchmark Context

Google says Gemini 3.5 Transcribe reduces streaming word error rate by as much as 79% and non-streaming word error rate by up to 48%. Those figures are based on testing reported through Artificial Analysis and cover English, multilingual, short-form, long-form, and noisy audio.

Word error rate, or WER, measures substitutions, deletions, and insertions relative to a human reference transcript. Lower scores are better, but a single average cannot capture every deployment condition.

As of August 27, 2026, the Artificial Analysis non-streaming leaderboard ranks Gemini 3.5 Transcribe first with an AA-WER of 4.3%. Its listed English score is 5.1%, while its multilingual score is 3.8%. The results place it ahead of models including OpenAI’s GPT-4o Transcribe, ElevenLabs Scribe v2, and Speechmatics Ursa 2 on that particular evaluation.

Artificial Analysis calculates AA-WER across five public speech datasets that include conversational recordings, earnings calls, audiobooks, and multilingual speech. Its speech-to-text benchmark methodology is broader than testing on one clean English dataset, but it still cannot reproduce every production environment.

The phrase “up to” also matters. Google is reporting the largest relative improvement found across the tested conditions, not a guaranteed 79% reduction for every recording. Performance can change with background noise, overlapping speakers, accents, uncommon names, compressed audio, and domain-specific terminology.

Streaming systems introduce another tradeoff: a transcript can be accurate after a sentence is complete while still delivering unstable partial results. Developers building subtitles or conversational agents should measure time to the first partial transcript, time to finalization, and how often previously displayed words are revised.

## Pricing Makes Large Transcription Jobs Practical

Google’s Gemini API pricing lists Gemini 3.5 Transcribe audio input at $3 per million tokens, which it estimates at approximately $0.006 per minute. Text input and output cost $0.30 per million tokens. There is currently no free tier for either transcription model.

At the published audio rate, transcribing one hour costs about $0.36 before text tokens, storage, networking, and application infrastructure. Processing 1,000 hours would cost roughly $360 in audio-input charges.

That pricing makes the model plausible for large media libraries, voicemail archives, contact centers, and meeting platforms. The final cost will still depend on how much text context an application sends, whether recordings need preprocessing, and how often a live connection must be restarted.

The preview limits deserve attention. A production live-transcription service must account for 15-minute session boundaries, while prerecorded files longer than two hours need to be divided without losing speaker or timestamp continuity. These are manageable constraints, but they prevent the preview from functioning as a completely drop-in replacement for every existing speech API.

## Google Is Taking Transcribe Beyond the API

## Frequently Asked Questions

4 questions

## 1
### What Is Gemini 3.5 Transcribe?


Gemini 3.5 Transcribe is Google’s specialized AI speech-to-text model based on Gemini 3.5 Flash. It supports live audio streams and prerecorded files, recognizes 91 languages, and can follow prompts that control formatting and cleanup. Google released it in public preview through the Gemini API and Google AI Studio on August 25, 2026.
