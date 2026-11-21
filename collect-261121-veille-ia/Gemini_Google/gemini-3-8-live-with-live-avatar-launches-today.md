---
id: collect-261121-veille-ia/dev-langages/gemini-3-8-live-with-live-avatar-launches-today
title: "Gemini 3.8 Live with Live Avatar Launches Today"
domain: dev-langages
role: reference
task: article
actors: ["Google"]
dates: ["2026-09-24"]
keywords: ["live", "avatar", "gemini", "model", "google", "conversational", "streaming", "launches"]
source: https://emergent.sh/news/gemini-3-8-live-live-avatar-launches-today
source_anchor: ""
source_lines: [1, 29]
section: "Gemini_Google"
---
# Gemini 3.8 Live with Live Avatar Launches Today

**Officially launched on September 24, 2026.**

Google DeepMind officially launched Gemini 3.8 Live with Live Avatar on September 24, 2026, introducing a conversational AI model that pairs real-time voice interaction with animated visual avatars. The release extends the Gemini 3.8 Flash family into synchronous streaming applications, targeting use cases where visual presence strengthens user engagement and trust.

## Real-Time Streaming Architecture

Gemini 3.8 Live processes multimodal input and output streams with sub-200ms end-to-end latency, according to Google DeepMind. The model accepts voice input, generates natural language responses, and synchronizes speech with avatar facial animations and gestures in real time. Engineers can interrupt the model mid-response, mimicking natural human conversation patterns. The architecture supports WebRTC and gRPC streaming protocols, enabling deployment in web browsers and mobile applications without custom infrastructure.

## Live Avatar Capabilities

The Live Avatar feature renders photorealistic or stylized human faces that respond to conversational context. Avatars display lip-sync accuracy within 50ms of speech output, eye contact that follows user gaze direction, and procedural expressions tied to sentiment analysis of the model's responses. Developers can customize avatar appearance, voice profiles, and nonverbal behaviors through API parameters. Google DeepMind trained the avatar system on diverse facial datasets to minimize demographic bias in rendering quality.

## Comparison to Prior Flash Models

Unlike Gemini 3.8 Flash, which optimizes for batch text and code generation, Gemini 3.8 Live sacrifices throughput for latency and interactivity. Benchmark tests show 3.8 Live achieves 87 percent of 3.8 Flash's accuracy on MMLU while maintaining conversational coherence across 20-turn dialogues. The model supports 128,000-token context windows but prioritizes the most recent 8,000 tokens for low-latency responses. Pricing follows a per-minute usage model rather than per-token billing, reflecting the streaming workload.

## Target Applications

Google positions Gemini 3.8 Live for customer service automation, virtual health assistants, language tutoring, and accessibility tools. Early pilots include a telecommunications provider using avatars for billing support and a university deploying conversational tutors for STEM courses. The model's interruptibility and natural pacing reduce user frustration in high-stakes interactions where traditional chatbots fail. Accessibility features include real-time captioning, sign language avatar variants, and tone-adjusted voice profiles for neurodiverse users.

## Availability and Integration

Gemini 3.8 Live with Live Avatar is now available through Google Cloud's Vertex AI platform and the Gemini API. Developers can access the model via REST endpoints, Python and JavaScript SDKs, and pre-built UI components for rapid prototyping. Google offers a free tier for up to 500 minutes per month, with enterprise pricing starting at $0.12 per minute for standard avatars and $0.18 per minute for custom avatar training. The model requires OAuth 2.0 authentication and complies with SOC 2 Type II and ISO 27001 security standards.

## What This Means

Gemini 3.8 Live with Live Avatar marks a shift from text-first AI toward embodied conversational agents that mimic face-to-face interaction. By integrating speech, vision, and procedural animation into a single streaming model, Google DeepMind addresses use cases where user trust and engagement depend on nonverbal communication cues. The release intensifies competition with OpenAI's rumored voice-avatar projects and Anthropic's emphasis on text-based safety, positioning Google as the leader in multimodal real-time AI for 2026.
