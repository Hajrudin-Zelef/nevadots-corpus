---
id: collect-261121-veille-ia/google-genie/genie-4-a-real-time-world-model-with-audio-and-dialogue
title: "Genie 4: a real-time world model with audio and dialogue"
domain: google-genie
role: reference
task: article
actors: []
dates: []
keywords: ["genie", "google", "world", "audio", "gemini", "deepmind", "real-time", "model"]
source: https://www.neuraldaily.news/en/a/google-deepmind-genie-4-world-model
source_anchor: ""
source_lines: [1, 49]
section: "Google_Genie"
---
Updated: November 20, 2026, 09:31 (Europe/Budapest): Genie 4 is now available in the Gemini API and Google AI Studio, with pricing. Details in the new section below.

## What was announced?

Google DeepMind announced Genie 4, the successor to Genie 3 (Google DeepMind). The model generates real-time, interactive world simulations: users navigate and move the camera with the keyboard and steer the scene with text prompts as it evolves. Genie 4 is natively multimodal and generates music, ambient sound and dialogue together with the world.

## What changed since Genie 3?

- Real-time steering in natural language: weather, camera, or a new person or object mid-simulation. Gemini adapts the prompt to the scene and keeps the world coherent between inputs.
- More realistic physics (aerodynamics, collisions) and what DeepMind calls the biggest leap: human characters that move naturally and can be steered.
- DeepMind's first real-time world model with native audio, with characters that talk and react in real time.
- Gemini 3.8 Flash runs as an agent in the loop: it breaks goals into steps, watches incoming frames and checks whether each step succeeded.

DeepMind is also experimenting with Gemini 4 Argon: in one demo, Apptronik's Apollo humanoid plays checkers in simulation while Argon breaks the task into real-time steps.

## Who can try it, and when?

It is currently a research preview for trusted testers. It is coming soon to Google AI Ultra subscribers in Project Genie and to early access partners of the Gemini API and Google AI Studio. A waitlist for Project Genie is open. All generated video and audio carries a SynthID watermark.

## Limitations

DeepMind itself calls it an early, experimental model. It stays coherent over short sessions, but details and object permanence can drift in longer runs or after several complex prompt edits. Complex physical interactions, audio accuracy, legible text and long-term consistency remain open problems. The announcement gives no resolution, frame rate or benchmark figures.

## Update: now live in Project Genie

On November 18, Google began rolling out a new Project Genie experience to eligible Google AI Ultra subscribers (Google DeepMind). This is the first place Genie 4 is available, right in the browser. Google AI Pro members can join a waitlist.

- Genie 4: edit the world mid-session with a new prompt (weather, new characters, the direction of the action), with real-time native audio.
- Genie 3 upgrade: up to 3 minutes per generation (up from 60 seconds), world chaining, worlds seeded from 360-degree panoramas and promptable in-world events.
- Project Genie agent: turns a natural language description into the world's look and soundtrack using Google's multimodal models, while Gemini watches the screen to suggest next steps or trigger events.
- Community gallery: curated and user-built worlds you can explore and remix, plus publishing your own.

DeepMind still describes it as a frontier research model: camera latency, intricate physical interactions, audio precision and long-term scene permanence remain open challenges. The announcement says nothing new about Gemini API early access.

## Update: now in the Gemini API

On November 19, Genie 4 became generally available in the Gemini API through the Interactions API (models/genie-4) and in Google AI Studio (Google). Starting from text, a 16:9 reference image or both, it streams a continuous 720p, 24 fps world with synchronized native audio that you steer with WASD, arrow keys and live text prompts.

- Connects over WebRTC or WebSocket, with 48 kHz stereo audio.
- Paid-tier pricing: $1.50 per million input tokens, $9 per million text output tokens and $17.50 per million video output tokens. Google puts that at about $0.10 per second, audio included.
- Every output carries an imperceptible SynthID watermark. AI Studio has an interactive canvas and starter apps to get going.

At about $0.10 per second, that is roughly $6 per minute or $360 per hour (Gemini API changelog). It suits prototypes, design visualization and short agent or robotics training sessions; a continuously running game server is still expensive.

## Why it matters

World models are moving toward simulation environments for training robots and agents, and wiring in Gemini as a planner makes that more concrete. If you build games, prototypes or simulations, watch the Gemini API early access, since that is where Genie 4 will become something you can actually integrate.

Update · Genie 4 GA in the Gemini API (Interactions API, models/genie-4) and AI Studio; pricing $1.50/$9/$17.50 per M tokens, ~$0.10/sec.
