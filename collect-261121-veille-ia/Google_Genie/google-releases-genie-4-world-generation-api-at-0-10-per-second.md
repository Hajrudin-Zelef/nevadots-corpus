---
id: collect-261121-veille-ia/google-genie/google-releases-genie-4-world-generation-api-at-0-10-per-second
title: "Google Releases Genie 4 World Generation API at $0.10 Per Second"
domain: google-genie
role: reference
task: article
actors: []
dates: ["2026-11-18"]
keywords: ["google", "genie", "second", "generation", "tokens", "world", "text", "seconds"]
source: https://af.net/realtime/google-releases-genie-4-world-generation-api-at-0-10-per-second/
source_anchor: ""
source_lines: [1, 11]
section: "Google_Genie"
---
On November 19, 2026, Google made its interactive world generation model Genie 4 available to developers via the Gemini API and Google AI Studio, according to reporting by Unite.AI. Operating under the model identifier models/genie-4 through the v1beta Interactions API, the system allows programmers to generate and navigate responsive simulated environments in real time using text prompts, reference images, and keyboard controls.

Genie 4 generates fixed 16:9 aspect ratio 720p video at 24 frames per second accompanied by synchronized native stereo audio, streaming outputs via WebRTC or WebSocket. The system renders over a rolling temporal window rather than a permanent 3D scene, capping each individual session at a strict 120-second maximum. Offered exclusively on Google's paid tier, pricing is set at $1.50 per million tokens for input prompts or reference images, $9.00 per million text output tokens, and $17.50 per million video output tokens (including audio), translating to roughly $0.10 per second or $6.05 per minute at 240 video tokens per frame (5,760 tokens per second). The model does not offer free-tier access, Batch API, Flex inference, Priority inference, live microphone or webcam streaming, function calling, Google Search grounding, structured outputs, or session resumption.

To construct an environment, developers establish an interaction session, connect over WebRTC or WebSocket, and send an initial world_setup event containing descriptive text, a single reference image, or both. Movement and camera perspective are driven directly via WASD and arrow keys, while mid-stream text inputs can alter scene characteristics or inject character dialogue. Google recommends WebRTC for interactive browser applications using ephemeral tokens scoped to one interaction, while WebSocket supports Python and Node.js workflows, frame capture, and custom pipelines. Technical documentation notes that closing a client connection or browser tab does not immediately stop server-side generation, requiring developers to explicitly call complete() or cancel() to manage billing. Local visual response latencies measure between 0.2 and 0.5 seconds for native keyboard actions under low network latency, extending to one to two seconds for mid-session text changes, while round-trip network delays exceeding 150 milliseconds noticeably delay steering.

The API release follows an update to Project Genie on November 18, 2026, which granted initial access to Google AI Ultra subscribers and opened a waitlist for Google AI Pro members. While that release emphasized browser-based creation, a Project Genie agent, and a community gallery, the API release provides developer sessions, documentation, paid-tier pricing, and usage quotas. Google lists prototyping interactive experiences, visualizing spatial concepts, and training robotics and AI agents as target use cases, and states that every output carries imperceptible SynthID watermarking.

Google's implementation guide outlines architectural constraints that limit the model's permanence. Because generation occurs over a rolling temporal window without a permanent 3D scene, visual coherence and object permanence are strongest during the first 45 to 90 seconds before stability potentially degrades after 60 to 90 seconds. Looking away from an object for eight seconds or more, or turning 360 degrees, can cause landmarks, doors, or buttons to move or reset, meaning the environment cannot be assumed to provide a stable long-term map for robotics or agent experiments.

Usage capacity is governed across paid project tiers: Tier 1 accounts are limited to 2 concurrent active sessions and 50 daily requests; Tier 2 supports 5 concurrent sessions and 250 daily requests; and Tier 3 permits 10 concurrent sessions and 1,000 daily requests. Real-time control messages sent within an active session do not consume separate per-minute or per-day request quotas. Output is served exclusively from data centers in the United States, without portrait mode or configurable frame rates.
