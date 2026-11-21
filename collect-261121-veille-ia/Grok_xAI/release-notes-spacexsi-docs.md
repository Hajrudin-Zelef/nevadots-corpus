---
id: collect-261121-veille-ia/grok-xai/release-notes-spacexsi-docs
title: "Release Notes | SpaceXSI Docs"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: []
keywords: ["grok", "image", "quality", "text", "available", "speech", "docs", "spacexsi"]
source: https://docs.x.ai/developers/release-notes
source_anchor: ""
source_lines: [1, 87]
section: "Grok_xAI"
---
Requests to grok-imagine-image-quality and grok-imagine-image-pro are now served by grok-imagine-image-2.0-low: grok-imagine-image-2.0 with quality set to low, at a lower per-image price. No request changes are needed. usage.cost_in_usd_ticks on each response reflects the lower price, and gRPC responses (xAI SDK) report grok-imagine-image-2.0-low as the model. See the migration guide.

You can now set content guidelines on text API requests. Add a guidelines array to a Chat Completions or Responses API request to set how Grok handles content in the nsfw, violence, and profanity categories. Grok follows the guidelines as it writes its response. See Custom Moderation Controls.

grok-voice-3.0 is now available with Speech to Speech, at the same price as Grok Voice 2.0. grok-voice-latest now points to grok-voice-3.0, so sessions that use the alias are upgraded automatically. The API is unchanged. To get started, see the Speech to Speech docs.

grok-voice-transcribe-1.0 is deprecated and reaches end of life on October 2, 2026. All requests to that slug are routed to grok-voice-transcribe-2.0 at the same price, with higher accuracy. See the Speech to Text docs.

You can now send safety_identifier, an opaque end-user identifier assigned by your application, on Chat Completions, the Responses API, deferred chat completions, the Batch API, and the gRPC GetCompletionsRequest. It lets SpaceXSI attribute a policy violation to one of your end users rather than to your API key. The legacy user field is still accepted. See the Security FAQ.

Grok 4.7, SpaceXSI's frontier model for coding, agentic tasks, and knowledge work, is now available on the xAI API as grok-4.7. It has a 500k context window, text and image inputs with text-only output, and no text output limit. Pricing is $2 / $0.50 / $6 per 1M tokens (input / cached input / output) below 200k prompt tokens, and $4 / $1 / $12 above. Reasoning effort supports low, medium, high (default), and xhigh. On the Responses API, grok-4.7 always returns reasoning.encrypted_content, even when include does not list it. It is also served on the US regional endpoint. Grok 4.7 Fast is the same model and costs 2x the standard token rates, or 1.5x for long-context requests. It's available only in Cursor and Grok Build, not on the public xAI API, and is billed through your plan there. See the Grok 4.7 overview and the announcement.

grok-voice-transcribe-2.0 is now available. Use grok-voice-transcribe-1.0 or grok-voice-transcribe-2.0; the default is grok-voice-transcribe-2.0. See the Speech to Text docs.

On November 2, 2026, grok-imagine-image-quality is retired. Requests to the slug will be served by grok-imagine-image-2.0 with quality set to low, with no change to the request or response shape and at a lower per-image price. grok-imagine-image (1.0) is not affected. See the migration guide.

Auto quality. The quality parameter on grok-imagine-image-2.0 now accepts auto, and the default when quality is omitted has moved from medium to auto. Auto currently uses low for image generation and medium for image editing. Images are billed at the quality they are served at. Pass low or medium explicitly to pin a specific quality. See Image Generation.

Five reference images. Image editing now accepts up to 5 source images per request (was 3). See Multi-Image Editing.

New aspect ratios. Image generation and editing accept 21:9 (cinematic widescreen) and 5:2 (wide banners). See Image Generation.

Grok 4.6, SpaceXSI's frontier model for coding, agentic tasks, and knowledge work, is now available on the xAI API. It has a 500k context window, text and image inputs with text-only output, and no text output limit. Pricing is $2 / $0.50 / $6 per 1M tokens (input / cached input / output) below 200k prompt tokens, and $4 / $1 / $12 above. Reasoning effort supports low, medium, high (default), and xhigh. See the Grok 4.6 overview and the announcement.

Grok Bot is now available. Durable AI teammates that work on a persistent cloud computer, with messaging, approvals, connectors, and routines. See the Grok Bot overview and Get started.

grok-imagine-video-1.5 now supports text-to-video, image-to-video, and reference-to-video (including optional preset voices), with native 1080p for T2V and I2V. Text-to-video on this model runs as text-to-image then image-to-video under the hood. See Video Generation, Image-to-Video, and Reference-to-Video.

grok-voice-2.0 is now available with Speech to Speech. grok-voice-latest will route to this model starting August 5, 2026. To get started, see the Speech to Speech docs. For more details, see our announcement.

Speech to Text now accepts a vad_threshold parameter (streaming query param and batch multipart field) to tune the voice-activity gate that skips non-speech audio. Lower values transcribe quieter or noisier speech — useful for narrowband telephony — and 0 disables the gate. See the Speech to Text docs.

Grok 4.5, SpaceXSI's model for coding, agentic tasks, and knowledge work, is now available on the xAI API. Priced at $2 / 1M input tokens and $6 / 1M output tokens, with configurable reasoning effort (low, medium, or high; default high). See the Grok 4.5 overview and the announcement.

You can now request higher scheduling priority per request by setting service_tier: "priority" on text inference endpoints (Chat Completions and Responses). The response's service_tier field reports the tier actually applied, and priority rates are billed only when priority is used. For more details, see the Priority Processing docs.

Public URLs for Files — turn any file in your Files API storage into a permanent, unauthenticated URL that anyone can open, embed, or share. Revocable at any time, or set an auto-expiry between 1 hour and 30 days. See the Public URLs docs.

Reference stored files as image & video inputs — substitute image_file_id, video_file_id, or reference_image_file_ids for URL inputs across every image & video endpoint, with no need to re-upload bytes or make the file public. See Files API integration for image & video requests.

Persist image & video outputs to Files — set storage_options on any image or video request to save the generated asset to your Files storage; pair with storage_options.public_url to publish a shareable link in one round trip. See Files API integration for image & video requests.

The streaming Speech to Text API now supports Smart Turn end-of-turn detection. When enabled via the smart_turn query parameter, an ML model predicts whether the speaker has finished their thought at silence boundaries — reducing false endpointing during dictation, number sequences, and mid-sentence pauses. Use smart_turn_timeout to set a maximum silence fallback. For more details, see the Smart Turn docs.

The Context Compaction API is now available. You can shrink long conversations into a shorter context and reuse it in follow-up requests for lower cost, faster time-to-first-token, and sharper responses on long agent loops. For more details, see the Context Compaction docs.

WebSocket Responses API mode is now available. Drive the Responses API over a single, long-lived WebSocket connection for lower end-to-end latency on tool-heavy agent workloads. For more details, see the WebSocket Mode docs.

Web Search now supports explicitly searching for images. Enable enable_image_search to let Grok search directly for relevant images; responses can include returned images as Markdown image embeds. For details, see Enable Image Search.

Grok Build is now available in beta. Use the interactive TUI, run headlessly in scripts, or build apps and orchestrators with the Agent Client Protocol.

You can now clone a voice from a short audio clip and use it across the Text-to-Speech and Speech to Speech APIs. Create and manage your voice catalog from the SpaceXSI console. For more details, check out the Custom Voices docs and our blog post.

Every API response now includes the exact cost of the request via a cost_in_usd_ticks field in the usage object. Works across chat completions, Responses API, image generation, video generation, and streaming. For more details, see the Cost Tracking docs.

You can now set an expiration policy on uploaded files using expires_after or an explicit expires_at timestamp. Expired files are automatically deleted. For more details, see the Files API docs.

Grok Voice 1.0 is now available with the Speech to Speech API. To get started, check out the Speech to Speech docs. For more details, see our blog post.

The SpaceXSI Speech to Text API is now generally available. Transcribe audio to text in 25 languages with batch and streaming modes. For more details, check out the Speech to Text docs.

The Text-to-Speech API is now generally available. Generate natural-sounding speech from text with Grok. For more details, check out the Text-to-Speech docs.

You can now create batches by uploading a JSONL file via the Files API. Supports all batch endpoints including chat, image, and video in a single file.

Batch API is available for all customers. It enables efficient batch processing of multiple requests, providing a better experience for users who need to submit large volumes of requests at once.

Collections Search Tool: You can now search through uploaded knowledge bases (collections) in chat conversations via the API. For more details, check out the docs.

Remote MCP Tools: You can now use tools from remote MCP servers in chat conversations via the API. For more details, check out the docs.

Mixing client-side and server-side tools: You can now mix client-side and server-side tools in the same chat conversation. For more details, check out the docs.

We're excited to announce that Grok now supports structured outputs. Grok can
now format responses in a predefined, organized format rather than free-form text. 1. Specify the
desired schema

A month ago, we launched the public
beta of our enterprise API with grok-beta and grok-vision-beta. We’re adding grok-2-1212 and
grok-2-vision-1212, offering better accuracy, instruction-following,
and multilingual capabilities.

We are happy to announce the immediate availability of our API, which
gives developers programmatic access to our Grok series of foundation models. To get started, head
to console.x.ai and sign up to create an account. We are excited to see
what developers build using Grok.
