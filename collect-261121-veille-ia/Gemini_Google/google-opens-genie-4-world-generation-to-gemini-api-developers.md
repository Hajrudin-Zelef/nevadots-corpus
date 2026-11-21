---
id: collect-261121-veille-ia/gemini-google/google-opens-genie-4-world-generation-to-gemini-api-developers
title: "Google Opens Genie 4 World Generation to Gemini API Developers"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["genie", "generation", "google", "seconds", "sessions", "text", "developers", "gemini"]
source: https://www.unite.ai/google-opens-genie-4-world-generation-to-gemini-api-developers/
source_anchor: ""
source_lines: [1, 47]
section: "Gemini_Google"
---
## Key PointsGenie 4 sessions cap at 120 seconds because the model renders over a rolling window…

View more

- Genie 4 sessions cap at 120 seconds because the model renders over a rolling window without a persistent 3D scene.
- Developers must call complete() or cancel() explicitly since closing a browser tab does not stop server generation or billing.
- The API targets brief controlled simulations rather than hosted worlds that applications can retain and revisit across sessions.

Google made Genie 4 available through the Gemini API and Google AI Studio on November 19, 2026, giving developers access to interactive worlds that respond to keyboard controls and live text prompts. The model generates continuous 720p video at 24 frames per second with synchronized native audio.

The developer release uses `models/genie-4` on the `v1beta` Interactions API. A world can be seeded with text, a single reference image, or both; WASD and arrow keys control movement and camera orientation, while text events modify the scene or supply character dialogue. The API documentation distinguishes this responsive frame rendering from offline generation of a fixed video clip.

Google lists prototyping interactive experiences, visualizing spatial concepts, and training robotics and AI agents as possible uses, and says every output carries imperceptible SynthID watermarking.

The API announcement follows the Project Genie update on November 18, 2026, which gave eligible Google AI Ultra subscribers their first access to Genie 4 and allowed Google AI Pro members to join a waitlist. That subscriber release emphasized browser-based creation, a Project Genie agent and a community gallery; the API instead adds developer sessions, documentation, paid-tier pricing and usage quotas.

## Paid Sessions and Billing

Genie 4 requires a paid-tier Gemini API key and is not offered on the free tier. Its standard pricing is $1.50 per million tokens for text or reference-image input, $9 per million for text output, and $17.50 per million for video output with stereo audio included. At 240 video output tokens per frame, or 5,760 tokens per second, the listed effective rates are approximately $0.10 per second and $6.05 per minute. Batch API, Flex inference and Priority inference are unsupported.

To build a session, developers create an interaction, connect over WebRTC or WebSocket, and send one `world_setup` event before steering with `user_action` or text input events. Browser applications can use an ephemeral token scoped to one interaction, keeping the long-lived API key on the backend. WebRTC is recommended for interactive browser apps, while WebSocket supports Python and Node.js workflows, frame capture and custom pipelines.

Closing a browser tab or detaching the local transport does not immediately stop server generation. Calling `complete()` ends generation and billing, while `cancel()` aborts server-side generation, making explicit shutdown part of cost control.

## Session Limits and Latency

Across its model documentation and implementation guide, Google caps sessions at 120 seconds, identifies the first 45 to 90 seconds as the strongest period for visual coherence and object permanence, and warns that visual stability may degrade after 60 to 90 seconds.

The guide describes generation over a rolling temporal window without a permanent 3D scene. Looking away from an object for eight seconds or more, or turning 360 degrees, can make landmarks, doors and buttons move or reset. For robotics or agent experiments, the generated environment therefore cannot be assumed to provide a stable long-term map.

Output is fixed at a 16:9 aspect ratio, with no portrait mode or configurable frame rate, and sessions are served only from US data centers. Google’s implementation guide reports a visual response of about 0.2 to 0.5 seconds for native keyboard actions at low network latency, compared with roughly one to two seconds for mid-session text changes. Round-trip network latency above 150 milliseconds noticeably delays keyboard steering.

Genie 4 does not accept live microphone or webcam streams. It also lacks function calling, Google Search grounding, structured outputs and the ability to resume a completed interaction.

## Usage Quotas

Genie 4’s rate limits vary by paid project usage tier:

| Usage tier | Concurrent active sessions | Requests per day | 
|---|---|---|
| Tier 1 | 2 | 50 | 
| Tier 2 | 5 | 250 | 
| Tier 3 | 10 | 1,000 | 

Real-time control messages within a session do not consume separate per-minute or per-day request quotas.

The API makes Genie 4 a programmable environment for brief, controlled simulations rather than a hosted persistent world. Its central constraint is continuity: applications can generate responsive scenes on demand, but the current design does not provide a world they can retain and revisit across completed sessions.
