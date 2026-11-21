---
id: collect-261121-veille-ia/grok-xai/grok-models-pricing-spacexsi-docs
title: "Grok Models & Pricing | SpaceXSI Docs"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: ["2026-09-21"]
keywords: ["image", "models", "grok", "model", "search", "latest", "modelname", "reasoning"]
source: https://docs.x.ai/developers/models?cluster=us-east-1
source_anchor: ""
source_lines: [1, 53]
section: "Grok_xAI"
---
### Grok 4.7

Our flagship model for code and everything else: agentic tool calling, minimal hallucinations, configurable reasoning.

### Voice API

Real-time conversations, speech-to-text, and text-to-speech.

- STS
- $0.08 / min
- TTS
- $15.00 / 1M chars
- STT (Batch)
- $0.10 / hour
- STT (Streaming)
- $0.20 / hour

### Image & Video API

Turn ideas into reality with image and video generation.

- Modes
- Generation & editing
- Speed
- Industry-leading

Your choice depends on your use case. We have dedicated models and APIs for audio, image, and video capabilities. For everything else, including code, use Grok 4.7. It is the most capable model we’ve built.

- **No access to realtime events without search tools enabled**  - Grok has no knowledge of current events or data beyond what was present in its training data.
  - To incorporate realtime data with your request, enable server-side search tools (Web Search / X Search). See Web Search and X Search.
- **Chat models**  - No role order limitation: You can mix `system` ,`user` , or`assistant` roles in any sequence for your conversation context.
  - `logprobs` and`top_logprobs` are not supported by models`grok-4.20` and newer. These fields will be silently ignored if set.
- No role order limitation: You can mix 
- **Image input models**  - Maximum image size: `20MiB`
  - Maximum number of images: No limit
  - Supported image file types: `jpg/jpeg` or`png` .
  - Any image/text input order is accepted (e.g. text prompt can precede image prompt)
- Maximum image size: 
- **Batch API**  - Not every model accepts Batch API requests. See Details on each model page.
- **Encrypted reasoning**  - On the Responses API, `grok-4.7` always returns`reasoning.encrypted_content` , even when`include` does not list it. See Encrypted reasoning content.
- On the Responses API, 

The knowledge cut-off date of Grok 4.7 is May 2026.

Some models have aliases to help users automatically migrate to the next version of the same model. In general:

- `<modelname>` is aliased to the latest stable version.
- `<modelname>-latest` is aliased to the latest version. This is suitable for users who want to access the latest features.
- `<modelname>-<date>` refers directly to a specific model release. This will not be updated and is for workflows that demand consistency.

For most users, the aliased `<modelname>` or `<modelname>-latest` are recommended, as you would receive the latest features automatically.

Last updated: September 21, 2026
