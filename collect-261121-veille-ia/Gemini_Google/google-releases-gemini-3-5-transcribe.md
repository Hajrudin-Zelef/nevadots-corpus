---
id: collect-261121-veille-ia/gemini-google/google-releases-gemini-3-5-transcribe
title: "Google Releases Gemini 3.5 Transcribe"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["gemini", "transcribe", "google", "voice", "text", "releases", "audio", "across"]
source: https://www.speechtechmag.com/Articles/ReadArticle.aspx?ArticleID=176398
source_anchor: ""
source_lines: [1, 27]
section: "Gemini_Google"
---
# 
                    Google Releases Gemini 3.5 Transcribe

                
                Google has released Gemini 3.5 Transcribe, which it calls its "most precise speech-to-text model yet," for intelligent voice interactions and converting raw audio directly into formatted text.

Google built 3.5 Transcribe to plug seamlessly into developer workflows for building voice agents, real-time captioning tools, or post-call analytics pipelines. The model is available across the following two separate APIs:

- Real-time streaming: Delivers continuous, bi-directional streaming with sub-second latency for interactive voice apps via the Live API using gemini-3.5-transcribe-live.
- Pre-recorded audio processing: Transcribes recorded audio, meetings, call logs, and more with speaker attribution and word-level timestamps via the Interactions API using gemini-3.5-transcribe.

Gemini 3.5 Transcribe can capture natural speaking styles to understand intent and recognize custom vocabulary. It includes the following:

                - Smart transcription, which handles self-corrections (like "let's meet Tuesday—no, Wednesday"), removes filler words (ums and ahs), and auto-formats text.
- Function calling, which can delegate complex tasks (such as image generation and file analysis) to other Gemini models via function calls.
- Transcription that achieves an average word error rate of 4 percent for streaming and 2.6 percent for non-streaming use-cases. It shows strong performance across noisy, real-world environments, accurately capturing alphanumeric entities like postal codes and order IDs.
- Custom vocabulary, which recognizes specialized jargon and unique spellings.
- Global language support, which automatically detects and transcribes more than 85 languages, seamlessly handling regional accents and diverse dialects.
- Multi-speaker identification, which attributes speech in pre-recorded audio with timestamps for up to three speakers

In addition to the Gemini API in the Google AI Studio and Gemini Enterprise Agent Platform, 3.5 Transcribe brings context-aware understanding directly into everyday surfaces like Gboard, Antigravity, the Gemini app, and Chrome.

- On Gboard on Android, through the new Rambler feature, 3.5 Transcribe transforms spoken thoughts into text, filtering out filler words. Users can also use voice to make edits, correct misspellings, and change the writing style.
- On Google Antigravity 3.5 Transcribe pairs screen context and chat history to ensure pinpoint transcription accuracy across file names, agent thoughts, and active documents.
- In Google AI Studio, users can access 3.5 Transcribe in Build mode to vibe code apps with voice on the fly.
- In the Gemini app on macOS, 3.5 Transcribe not only transcribes free natural speech into clean formatted text, but also enables voice commands that can pair seamlessly with screen context to power complex workflows. By calling on other Gemini models in the background, the model can summarize local files, repurpose text across apps, or generate images right at the cursor using just the user's voice.
- Coming soon to Chrome, users will be able to talk to type in any web field to dictate replies, draft posts, or prompt Gemini in Chrome more naturally and easily with voice.
