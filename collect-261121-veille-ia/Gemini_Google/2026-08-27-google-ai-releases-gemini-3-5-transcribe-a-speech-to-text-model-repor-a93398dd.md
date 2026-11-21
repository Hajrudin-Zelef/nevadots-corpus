---
id: collect-261121-veille-ia/gemini-google/2026-08-27-google-ai-releases-gemini-3-5-transcribe-a-speech-to-text-model-repor-a93398dd
title: "Google ai releases gemini 3 5 transcribe a speech to text model repor"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-08-27"]
keywords: ["model", "google", "transcription", "gemini", "streaming", "transcribe", "releases", "live"]
source: ""
source_anchor: ""
source_lines: [1, 34]
section: "Gemini_Google"
---
Google has released Gemini 3.5 Transcribe, a speech-to-text model for real-time voice interfaces and recorded audio. It ships as two endpoints, not one. gemini-3.5-transcribe handles pre-recorded files through the Interactions API. gemini-3.5-transcribe-live handles bidirectional streaming through the Live API. Google reports average word error rates of 4.0% streaming and 2.6% non-streaming, as measured by Artificial Analysis. Time to final transcription improves 70% over Chirp 3, the previous model. Automatic detection covers more than 85 languages, including mid-sentence code-switching. The split between the two endpoints is the part worth planning around. They do not share the same feature set, limits, or price.
Is it deployable?
Yes, but API-only. There are no open weights and no self-hosted path. This is a managed-service decision, not an infrastructure one.
▲ AI DEV SIGNALS · FREE NEWSLETTER
Get the AI releases that matter, before they trend.
Model releases, open weights, agents and papers with code — the 5 that matter, in 1 email. Free.
- Company level: Any. Solo developers and startups can start on the Gemini API free tier via Google AI Studio. Mid-market teams move to the paid tier for higher rate limits. The paid tier also guarantees content is not used to improve Google’s products. Regulated enterprises route through the Gemini Enterprise Agent Platform, which adds provisioned throughput, compliance controls, and volume discounts. Both developer and enterprise tracks are in public preview, so treat production commitments accordingly.
- Industries: Contact centers and CX platforms, clinical documentation, media captioning and localization, legal and insurance intake, meeting tooling, and voice-driven developer tools.
- Applications: Real-time voice agents, live captioning, post-call analytics pipelines, meeting transcription with speaker attribution, dictation, and voice-controlled interfaces.
Two API surfaces, two different products
The Live API delivers sub-second, continuous transcription. It emits interim_input_transcription for speculative partials while someone is still talking, then input_transcription when the turn finalizes. Audio goes in as raw 16-bit PCM at 16kHz mono, in 100ms chunks. It supports automatic, hybrid, and manual voice-activity detection. Ephemeral tokens let mobile and web clients stream without holding an API key.
The constraints are real. Live sessions cap at 10 minutes of continuous streaming. Speaker diarization is not supported. Word-level timestamps are not supported.
The Interactions API covers what streaming cannot. It offers speaker diarization, word-level start and end offsets, and custom vocabulary biasing. The vocabulary list takes up to 1,000 terms, with best results below 100. Standard requests accept up to one hour of audio. That drops to 30 minutes once diarization or word timestamps are enabled.
Verbatim and smart are the real design decision
Both endpoints expose two modes. verbatim is the default and returns everything, including fillers, repetitions, and false starts. smart removes disfluencies, resolves spoken self-corrections inline, and applies structured formatting.
Google’s own documented example: “Um, so for the meeting, I think we should, uh, invite Alice and, wait no, Bob and Carol.” Verbatim keeps all of it. Smart returns “For the meeting, I think we should invite Bob and Carol.”
Smart mode cannot be combined with word timestamps or diarization. That is the tradeoff to plan around. A readable summary and an auditable transcript are now two different API calls.
Performance
As measured by Artificial Analysis, Google reports an average word error rate of 4.0% for streaming and 2.6% for non-streaming. On the multilingual FLEURS benchmark, across a set of top languages and locales, the model reports 5.50% streaming and 5.04% non-streaming.
Against Chirp 3, Google’s previous transcription model, time to final transcription improves by 70%. Language coverage spans over 85 locales with automatic detection and code-switching handled without configuration.
Ecosystem
The Live API is already wired into LiveKit, Pipecat, Agora, Fishjam, Vercel, and Vision Agents. On the consumer side, the model powers Rambler on Android, the Gemini app on macOS, and Google Antigravity. Chrome is listed as coming soon.
Key Takeaways
- Two endpoints, not one: streaming trades diarization and word timestamps for sub-second latency.
- Reported WER is 4.0% streaming and 2.6% non-streaming, per Artificial Analysis.
- Smart mode cannot be combined with timestamps or diarization — pick one per call.
- Blended cost runs about $0.005/min batch and $0.009/min live; no open weights.
- Hard limits: 10-minute live sessions, 1-hour files, 30 minutes with diarization on.
Check out the Technical details here. Also, feel free to follow us on Twitter and don’t forget to join our 150k+ML SubReddit and Subscribe to our Newsletter. Wait! are you on telegram? now you can join us on telegram as well.
Need to partner with us for promoting your GitHub Repo OR Hugging Face Page OR Product Release OR Webinar etc.? Connect with us
YOU READ TO THE END
Get the stories that didn’t make it on the homepage.
Last issue: ▲ what's inside: ai attackers, gemini 4 neon, daisyrose-27b, olala mini
Asif Razzaq is the CEO of Marktechpost AI Media Inc.. As a visionary entrepreneur and engineer, Asif is committed to harnessing the potential of Artificial Intelligence for social good. His most recent endeavor is the launch of an Artificial Intelligence Media Platform, Marktechpost, which stands out for its in-depth coverage of machine learning and deep learning news that is both technically sound and easily understandable by a wide audience. The platform boasts of over 2 million monthly views, illustrating its popularity among audiences.
