---
id: collect-261121-veille-ia/gemini-google/gemini-3-5-transcribe-intelligent-transcription
title: "Gemini 3.5 Transcribe Intelligent Transcription"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-08-26"]
keywords: ["gemini", "transcription", "transcribe", "model", "intelligent", "more", "accurate", "audio"]
source: https://docs.tokennode.io/en/news/2026-08-26-gemini-3-5-transcribe-智能转录/
source_anchor: ""
source_lines: [1, 47]
section: "Gemini_Google"
---
August 26, 2026

Our latest speech-to-text model, designed for accurate, intelligent real-time transcription.

**Diego Melendo Casado**

Senior Director of Engineering, Gemini Audio

**Luke Leonhard**

Chief of Staff, Gemini Audio, representing the Gemini Audio team

Today, we’re introducing Gemini 3.5 Transcribe, our most accurate speech-to-text model to date, designed for intelligent voice interactions. Traditional speech recognition models often struggle with background noise, complex terminology, and disfluencies in natural speech. Gemini 3.5 Transcribe, however, converts raw audio directly into accurate, polished, and formatted text.

Consumers are already experiencing new voice features powered by this transcription model in products such as the Gemini app and Android, including Rambler on Android and related features in the Gemini app for macOS. Now, developers can use Gemini 3.5 Transcribe to build similar experiences through the Gemini API in Google AI Studio and the Gemini Enterprise Agent Platform.

We designed 3.5 Transcribe to integrate seamlessly into developers’ workflows. Whether you’re building voice agents, real-time captioning tools, or post-call analysis pipelines, you can use this model through two separate APIs:

- Real-time streaming: Use `gemini-3.5-transcribe-live` through the Live API for continuous, bidirectional streaming in interactive voice applications, with latency below one second.
- Pre-recorded audio processing: Use `gemini-3.5-transcribe` through the Interactions API to transcribe recordings, meeting notes, call logs, and more, with support for speaker attribution and word-level timestamps.

## More Accurate, More Intelligent Transcription

Gemini 3.5 Transcribe is designed to capture the natural way people speak, enabling it to better understand user intent and recognize custom vocabulary so users can accomplish tasks with their voice.

- Intelligent transcription: Seamlessly handles self-corrections, such as “See you Tuesday—no, Wednesday,” removes filler words such as “um” and “uh,” and automatically formats the text.
- Function calling: Through function calling, the model can delegate complex tasks—such as image generation and file analysis—to other Gemini models. This capability is currently available in the Gemini app for macOS.
- More accurate transcription: According to measurements by Artificial Analysis, the model achieves an average Word Error Rate (WER) of 4.0% in streaming scenarios and 2.6% in non-streaming scenarios. It also delivers strong performance in noisy, real-world environments, accurately recognizing alphanumeric entities such as postal codes and order numbers.
- Custom vocabulary: Transcription can be seamlessly adapted to user-provided custom vocabulary, enabling the recognition of specialized terms and unique spellings.
- Global language support: Automatically detects and transcribes more than 85 languages while naturally handling regional accents and diverse dialects.
- Multi-speaker identification: Accurately labels speakers in pre-recorded audio and provides timestamps for up to three speakers. Support for more than three speakers remains experimental.

Gemini 3.5 Transcribe represents a significant improvement over our previous transcription model, Chirp 3, delivering new capabilities, lower word error rates, and substantially lower latency. For example, according to measurements by Artificial Analysis, the time required to generate the final transcription has been reduced by 70%. On the FLEURS benchmark, which covers a range of major languages and regions, the model demonstrates accurate multilingual performance, improving further on Chirp 3 with a WER of 5.50% in streaming mode and 5.04% in non-streaming scenarios.

## Experience Intelligent Transcription and Advanced Dictation

Beyond the Gemini API in Google AI Studio and the Gemini Enterprise Agent Platform, 3.5 Transcribe goes beyond standard speech-to-text functionality to provide a more natural, intuitive experience across the Google ecosystem. By bringing contextual awareness directly into everyday experiences such as Gboard, Antigravity, the Gemini app, and Chrome, it can effortlessly capture subtle expressions, user intent, and edits to text.

- In Gboard for Android, 3.5 Transcribe uses the new Rambler feature to turn spoken thoughts into well-formatted text while filtering out filler words. Users can also edit by voice, correct spelling errors, and change their writing style.
- In Google Antigravity, with the user’s permission, 3.5 Transcribe combines on-screen context with chat history to ensure accurate transcription of content such as file names, agent thought processes, and the current document.
- In Google AI Studio, users can use 3.5 Transcribe in Build mode to engage in vibe coding instantly by voice.
- In the Gemini app for macOS, 3.5 Transcribe not only turns natural, free-form speech into clean, well-formatted text, but also supports voice commands and can seamlessly incorporate on-screen context to execute complex workflows. The model invokes other Gemini models in the background to handle demanding tasks, enabling users to summarize local files, rewrite text across different applications, or generate images directly at the cursor using only their voice.
- This capability is coming soon to Chrome: Users will be able to dictate into any input field on a webpage, making it easy to dictate replies, write posts, or prompt Gemini in Chrome with their voice in a more natural and convenient way.

## See Early Reviews

With the Gemini Live API, developer platforms such as Agora, Fishjam, LangChain, LiveKit, Pipecat, Vercel, and Vision Agents make it easy for developers to build and deploy high-performance voice-driven interfaces.
