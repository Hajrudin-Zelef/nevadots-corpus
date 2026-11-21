---
id: collect-261121-veille-ia/modeles-open/black-forest-labs-flux-3-generates-video-audio-and-images-from-one-model-alphasignal
title: "Black Forest Labs' FLUX 3 Generates Video, Audio, and Images From One Model | AlphaSignal"
domain: modeles-open
role: reference
task: article
actors: []
dates: []
keywords: ["video", "audio", "flux", "model", "images", "draft", "full", "together"]
source: https://alphasignal.ai/news/black-forest-labs-flux-3-generates-video-audio-and-images-from-one-model
source_anchor: ""
source_lines: [1, 79]
section: "Modeles_Open"
---
## Takeaways

- **FLUX 3 Video is now live** via the BFL API, generating clips up to 20 seconds at 1080p with native synchronized audio in one request.
- **Natively multimodal:** audio, video, and images are trained together in one architecture (Self-Flow), not stitched from separate models, unlike Runway, Kling, and Luma.
- **Draft mode** lets you preview at $0.06/s, then deterministically upgrade the winner to full quality at $0.17/s HD, avoiding wasted full-render spend.
- **Benchmark claims:** BFL's internal tests show FLUX 3 preferred over Runway Gen-4.5 (77%) and Luma Ray 3.2 (93%), but near-even with Seedance 2.0 and Gemini Omni; numbers are vendor-reported and unverified.
- **Multilingual lip-sync** across 14+ languages is a standout feature; community testing shows it excels at dialogue-driven scenes but trails Seedance on high-energy action.
- **Open weights (FLUX 3 Dev)** and 2K/4K output are confirmed for later in 2026; the same backbone already powers a robotics model running on Audi factory floors.

FLUX 3 Video is now generally available via the BFL API and select partner tools. Black Forest Labs, the team behind the FLUX.1 image models, is making its first move into video generation with an architecture that differs from every other system in the field. Rather than training separate image, video, and audio models and wiring them together, BFL trained one model jointly across all three signals using their Self-Flow approach.

## One model, not three

Self-Flow is BFL's method for aligning multimodal generation and understanding within a single architecture. FLUX 3 scales that approach with significantly more compute and data, training on video, images, and audio simultaneously. The design logic is straightforward: a model that jointly learns from all three modalities can build a representation of the world rather than a representation of any one format. It learns how objects hold together, how things move, and how events sound. No single modality provides a complete description on its own.

For audio, this has a concrete consequence. Joint training means audio is generated from the same flow-matching pass as the video frames, so a single generation call produces a complete audiovisual clip with no separate diffusion model for sound and no lip-sync post-processing. Every major competitor in the space, including Runway, Luma, and Kling, assembles audio and video through separate pipelines or attaches audio as a post-generation pass.

## What it can actually do

The model generates clips up to 20 seconds long in HD resolution, with Full HD output via upscaling and native audio created alongside the video. The full capability set at launch:

- **Text-to-Video:** Simple or complex prompts, with natural movement, scene logic, and audio generated together.
- **Image-to-Video and Keyframes:** Start with an image, specify an end frame, or set multiple keyframes in a clip. FLUX 3 Video connects these in sequence while following the intended visual language.
- **Video Continuation:** Provide up to four seconds of existing video and audio and tell it what should happen next. The model accounts for both components to continue movement, camera behavior, dialogue, and audio across the seam.
- **Multiple Shots:** Create multiple scenes and camera angles within a single generation while keeping the sequence coherent.
- **Multilingual Dialogue:** Supported languages include English (various dialects), Chinese, Spanish, French, German, Japanese, Portuguese, Russian, Italian, Indonesian, Turkish, Hindi, Punjabi, and more, with precise lip-syncing.

## Draft mode is the sleeper feature

The most practically useful addition for anyone building on this API is Draft mode, a two-step commit: generate a fast, cheap preview first, then lock in the full render only when you like what you see. When a draft is satisfactory, FLUX 3 renders the video at full quality with the same subjects, composition, and motion. The draft cache is passed back to the model, so the full render is a deterministic upgrade of the exact draft you approved, not a fresh generation.

The timing breakdown from the docs: a draft takes about 61 seconds, the enhance pass adds another 83 seconds, while a direct full render takes around 108 seconds. You save time on rejected generations and get a guaranteed match on the one you keep.

## The API in practice

The API is asynchronous: you POST a request and get back a `polling_url`, then poll until the status flips to `Ready`. Every mode, text-to-video, image-to-video, and video continuation, hits the same `/v1/flux-3-video` endpoint. Here is the core request shape:

```
import os, requests
resp = requests.post(
    "https://api.bfl.ai/v1/flux-3-video",
    headers={"x-key": os.environ["BFL_API_KEY"]},
    json={
        "mode": "t2v",          # or i2v, v2v
        "prompt": "a fox running through dawn mist",
        "resolution": "hd",    # or fhd
        "duration": 5,           # 5-20 seconds
        "generate_audio": True,  # on by default
    },
).json()
polling_url = resp["polling_url"]
```
For image-to-video, swap in a `keyframes` field that accepts a single image URL, a pair for start and end frames, or a list of `[timestamp, image]` tuples for precise storyboarding. For continuation, pass `start_video`. The full API reference covers all constraints and dimensions.

## Pricing

The pricing structure rewards the draft-then-enhance workflow. Full renders run from **$0.17/s at HD** and **$0.29/s at Full HD** for text-to-video and image-to-video. Video continuation costs more at **$0.43/s HD** and **$0.54/s FHD**. Draft mode runs **$0.06/s** for standard modes and **$0.12/s** for continuation drafts, roughly a third of the full render price. A 10-second HD clip costs $1.70 at full quality, or $0.60 as a draft preview.

## How it stacks up

In BFL's internal preference tests, FLUX 3 was preferred in 60% of comparisons against Kling v3 Pro, 77% against Runway Gen-4.5, and 93% against Luma Ray 3.2, while margins were near-even at about 52% against Seedance 2.0 and Gemini Omni Flash. These are vendor-reported figures from a pre-release checkpoint and have not been independently verified, so they indicate promise rather than confirmed ranking.

Early community testing adds nuance. Side-by-side testing against a demanding Seedance prompt showed FLUX 3 hitting every element requested, but without the same frantic camera work, fisheye trick shots, or fast-action rhythm. For high-energy action sequences, Seedance currently has an edge. For dialogue-driven or dramatic scenes, FLUX 3 held up well, including community-generated clips with convincing rapid-fire dialogue and believable delivery.

## Where it fits and what comes next

The use cases BFL highlights are a reliable signal for where the model is strongest right now:

- Short-form content, social video, and influencer clips that need realistic dialogue
- Advertising and product video where you want to animate a still image or brand asset
- Multilingual content where lip-sync accuracy matters
- Educational and documentary-style clips that benefit from the model's world knowledge
- Any pipeline where you want to iterate cheaply before committing to a full render

Based on testing so far, FLUX 3 sits alongside Seedance and Kling rather than replacing them in most creator workflows, particularly for creators who need longer clips or want to feed in audio and multiple image references at once.

The roadmap extends well beyond video. BFL found that video generation and action prediction share enough structure that the same architecture could be extended to action prediction without sacrificing what it learned from video. That extension is already in production: the FLUX-mimic action model, derived from FLUX 3's video prediction capabilities, is being tested with Audi in factories for complex tasks like installing flexible door seals, with system reaction times around 101 milliseconds.

An open-weight Dev version intended for local use is scheduled for later in 2026. The fine-tuners, tool-builders, and studios that carried FLUX.1 are being asked to wait while paying customers go first. Whether developers treat FLUX 3 Dev as a genuine open release or a delayed, cut-down teaser will shape how much of that community stays engaged. For now, the official blog post and the API docs are the best starting points for anyone who wants to build on it today.
