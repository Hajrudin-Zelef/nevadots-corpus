---
id: collect-261121-veille-ia/gemini-google/google-opens-genie-4-world-model-to-developers-in-gemini-api
title: "Google opens Genie 4 world model to developers in Gemini API"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["google", "genie", "gemini", "world", "model", "developers", "second", "interactive"]
source: https://www.testingcatalog.com/google-opens-genie-4-world-model-to-developers-in-gemini-api/
source_anchor: ""
source_lines: [1, 21]
section: "Gemini_Google"
---
Google has opened Genie 4, its newest world model from Google DeepMind, to developers through the Gemini API and Google AI Studio, just one day after it reached Google AI Ultra subscribers in Project Genie.

The model is exposed as models/genie-4, so builders can now call a real-time world model with their own API key instead of watching demos or holding a consumer subscription. Google describes it as its first world model for developers to build with.

Genie 4 starts from a text prompt, a reference image, or both, and generates a continuous 720p world at 24 frames per second with synchronized native audio, from ambient soundscapes and sound effects to character dialogue. Users move through the scene with WASD or arrow keys and can type live prompts mid-session to change the weather, lighting, objects or events, while Gemini reasoning keeps the world state consistent and coherent.

Google points to prototyping interactive experiences, visualizing spatial designs and training robotics and AI agents as the main use cases, and every output carries an imperceptible SynthID watermark. AI Studio adds an interactive canvas and starter apps for quick experiments.

Under the hood, Genie 4 streams over WebRTC for browser apps or WebSockets for server code, with single-session ephemeral tokens keeping the main API key off the client. It requires a paid-tier key and costs $1.50 per million input tokens, $9 per million text output tokens and $17.50 per million video output tokens, roughly $0.10 per second of streamed video with stereo audio included.

Sessions are capped at 120 seconds, keyboard moves land in about half a second while text prompts take one to two seconds, and serving comes from data centers in the United States, Western Europe and Japan.

The release caps a fast run of Google DeepMind launches. Genie 4 was first announced as a research model in late October, built on capabilities from the Gemini family, with Gemini Omni anchoring realistic appearance and other Gemini models handling the reasoning that keeps simulations coherent.

Google frames world simulation as a step toward AGI, offering safe training environments for robots and agents and new creative tools for filmmaking, gaming and manufacturing. Putting the model in the Gemini API moves that research out of a subscriber playground and into third-party apps, games and agent pipelines.

## Sources and related context

- Gemini API docs: Interactive world generation with Genie 4: Supports: Google's developer documentation details the WebRTC and WebSocket transports, paid-tier billing of about $0.10 per second, the 120-second session cap and the US, Western Europe and Japan serving regions.
- Google: Genie 4 is now available in Project Genie: Related context: The previous day's launch brought Genie 4, a Project Genie agent and a community gallery to eligible Google AI Ultra subscribers.
- Google: Announcing Genie 4 for real-time interactive simulation: Related context: The October research announcement explains how Genie 4 builds on Gemini Omni and other Gemini models and why Google sees world simulation as a step toward AGI.
