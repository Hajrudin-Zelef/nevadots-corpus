---
id: collect-261121-veille-ia/gemini-google/gemini-3-5-transcribe-pricing-features-api-guide
title: "Gemini 3.5 Transcribe: Pricing, Features & API Guide"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["gemini", "model", "audio", "transcribe", "live", "google", "prerecorded", "interactive"]
source: https://www.therundown.ai/tools/gemini-3-5-transcribe
source_anchor: ""
source_lines: [1, 160]
section: "Gemini_Google"
---
### Voice interfaces and live captions

Stream microphone or call audio into an interactive application with sub-second transcription latency.

Independent tool overview

Gemini 3.5 Transcribe is Google's public-preview speech-to-text model for low-latency live captions and prerecorded audio, with language auto-detection, custom vocabulary, smart formatting, diarization, and word timestamps.

Visit the official Gemini 3.5 Transcribe site ↗
Overview

Gemini 3.5 Transcribe is a dedicated speech-to-text model rather than a general Gemini model prompted to analyze audio. Google provides separate endpoints for live WebSocket streaming and prerecorded file processing, allowing developers to choose low latency or richer transcript metadata.

The model automatically detects more than 85 languages and can follow code-switching within a session. Custom vocabulary biasing helps with names, acronyms, product terms, and domain jargon. Prerecorded processing can add speaker labels and word-level timestamps, while the live model emphasizes sub-second interactive transcription.

Its distinctive feature is Smart transcription. Instead of preserving every spoken sound, it can remove filler words, repetitions, and disfluencies, resolve self-corrections, add punctuation, and normalize spoken values such as money or identification numbers into formatted text.

That cleanup is useful for dictation but inappropriate for every record. Legal, journalistic, research, support, compliance, and evidentiary workflows may require a verbatim transcript that preserves hesitation, false starts, or exact wording. Teams should store the original audio, expose whether cleanup is enabled, and review critical names, numbers, speakers, and quotations.

Use cases

The strongest fit depends on the job you need the product to complete, not the size of its feature list.

Process recorded audio with speaker labels, timestamps, custom terminology, and clean formatting.

Turn natural speech, self-corrections, and filler-heavy ideas into readable text for drafts, messages, and commands.

Support language auto-detection, regional accents, and conversations that switch languages.

Bias recognition toward product names, acronyms, people, locations, and specialized terms with a custom vocabulary.

Learn by doing

Put Gemini 3.5 Transcribe to work with step-by-step guides from The Rundown University.

Capabilities

The Live API uses a bidirectional WebSocket connection to return continuous low-latency text for interactive voice applications.

The Interactions API transcribes uploaded meetings, calls, interviews, and other audio files with richer metadata.

Optionally removes filler words, repetitions, and disfluencies, resolves self-corrections, and applies readable formatting.

Detects more than 85 languages and supports language switching within and between utterances.

Accepts up to 1,000 terms to bias recognition toward domain vocabulary, with Google recommending a smaller focused list for best results.

Prerecorded processing can attribute segments to as many as eight speakers, although attribution for three or more remains experimental.

Prerecorded transcripts can include start and end offsets for individual words for search, playback, clips, subtitles, and review.

Adds capitalization and punctuation and can convert spoken numbers or money into concise formatted values.

The same model powers voice experiences in the Gemini app on macOS, Rambler on supported Android devices, Google Antigravity, and other announced Google surfaces.

Process

Step 1

Decide whether the transcript must preserve fillers, repetitions, self-corrections, and exact spoken wording before enabling Smart transcription.

Step 2

Use the Live model for interactive latency; use the file model when diarization, word timestamps, or longer recordings matter.

Step 3

Capture clear consented audio, normalize levels, reduce avoidable noise, and provide only the highest-value names, acronyms, and technical terms.

Step 4

Request speaker labels and timestamps only when needed because they shorten the file limit and timestamps can reduce recognition accuracy.

Step 5

Check names, identifiers, addresses, dates, currencies, negations, medical or legal terms, speaker attribution, and direct quotations against the audio.

Step 6

Keep the original audio, model name, cleanup setting, vocabulary, transcript version, human edits, and word offsets so downstream users can audit the record.

Cost

Gemini 3.5 Transcribe has a free developer tier and token-based paid pricing. Google estimates an effective blended cost of about $0.005 per audio minute for prerecorded transcription and $0.009 per minute for Live transcription. Actual invoices are based on audio-input and text-output tokens, not the estimate.

Free within limits

Evaluation access through Google AI Studio and unpaid Gemini API quota.

About $0.005/audio minute

Paid Gemini API processing for uploaded audio.

About $0.009/audio minute

Paid real-time streaming over the Gemini Live API.

Usage-based or contracted

Access through Gemini Enterprise Agent Platform and related Google Cloud products.

Pricing checked . Check current pricing at the source ↗

Assessment

Compare

The right alternative depends on the specific output, workflow, controls and budget your project requires.

Content Creator

Choose ElevenLabs Scribe v2 when transcription accuracy, language coverage, diarization, and the ElevenLabs audio ecosystem are the priority.

Explore Scribe v2 →
Miscellaneous

Choose Voxtral Transcribe 2 for Mistral's batch and real-time speech stack, including an open-weight realtime option.

Explore Voxtral Transcribe 2 →
Consumer

Choose Cohere Transcribe when a free open-source speech-recognition model and self-managed deployment matter most.

Explore Cohere Transcribe →
Questions

Gemini 3.5 Transcribe is Google's dedicated public-preview speech-to-text model. It supports live streaming and prerecorded audio with automatic language detection, custom vocabulary, smart formatting, and file-only speaker and timestamp metadata.

gemini-3.5-transcribe processes audio files and supports speaker diarization and word timestamps. gemini-3.5-transcribe-live streams audio over WebSockets with sub-second latency but does not support those two metadata features.

Google estimates about $0.005 per minute for prerecorded transcription and $0.009 per minute for Live. Billing is actually calculated from separate audio-input and text-output token rates.

Smart transcription can remove fillers, repetitions, and disfluencies, resolve self-corrections, and apply punctuation and normalization. Disable cleanup when exact spoken wording must be preserved.

Google lists more than 85 supported languages and locales, with automatic detection and code-switching during a session.

The prerecorded model can label up to eight speakers, but attribution for three or more is experimental. Live Transcribe does not currently support diarization.

The prerecorded model can provide word start and end offsets. Live Transcribe cannot, and Google notes that word timestamps can reduce transcription accuracy.

Prerecorded audio can be up to one hour, or 30 minutes when diarization or word timestamps are enabled. A Live session is limited to 10 minutes.

Yes. The model accepts up to 1,000 vocabulary phrases for names, acronyms, and domain terms, though Google says customers generally see the best results with a focused list of roughly 100.

For unpaid Google AI Studio and Gemini API access, Google can use inputs and outputs to improve products and human reviewers may process them. For paid Gemini API access through an active billing project, Google says prompts and responses are not used for product improvement, though limited safety logging remains.

Bottom line

Gemini 3.5 Transcribe is a compelling new option for developers who want both live voice interaction and low-cost file transcription in the Gemini ecosystem. Language detection, code-switching, vocabulary biasing, diarization, timestamps, and especially Smart transcription cover a wide range of products. Its preview status and the distinction between polished dictation and verbatim records are the main caveats: production teams should use paid data terms, benchmark their own accents and audio conditions, preserve source audio, and require human verification for consequential transcripts.

Visit Gemini 3.5 Transcribe website ↗
Get access to all our AI courses, hundreds of real-world AI use cases, live expert-led workshops, an exclusive network of AI early adopters, and more.

Get unlimited access to all of our current & upcoming industry-specific AI courses for the duration of your subscription.

To keep up with the rapid pace of AI, our team publishes AI implementation guides daily. Our library contains 300+ practical use cases to automate real-world work.

Join weekly, live, interactive sessions with industry leaders who are at the forefront of AI for hands-on implementation guidance and exclusive insights.

Network with an exclusive community of AI-first professionals who are working smarter with AI. Learn how early adopters are using AI in their work and businesses.
