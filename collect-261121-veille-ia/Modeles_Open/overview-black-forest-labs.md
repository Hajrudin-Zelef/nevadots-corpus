---
id: collect-261121-veille-ia/modeles-open/overview-black-forest-labs
title: "Overview - Black Forest Labs"
domain: modeles-open
role: reference
task: article
actors: []
dates: []
keywords: ["flux", "output", "image", "video", "pricing", "resolution", "frame", "cost"]
source: https://docs.bfl.ai/quick_start/pricing
source_anchor: ""
source_lines: [1, 62]
section: "Modeles_Open"
---
**Simple credit-based pricing:** 1 credit = $0.01 USD • Pay per image, or per second for video • Same price for API and Playground

## Pricing

The following table shows pricing for all FLUX models and their variants:
### FLUX 3 Video

Use the FLUX 3 video pricing calculator to calculate the cost of your video.
FLUX 3 generates video with synchronized audio and uses **per-second pricing** — cost scales with clip length, resolution (`hd`, `fhd`, `qhd`, or `uhd`), and whether you run a full render or a fast draft.
Full-render rates are per second of output, by resolution class:
Drafts render at `hd`. Use the published draft rates above to estimate the cost of exploring variants before you enhance the shot you keep. `fhd`, `qhd`, and `uhd` are the same generation finished by the video upsampler at a larger canvas — see delivered dimensions — and are billed at the delivered class. See the FLUX 3 overview for length limits and supported aspect ratios. ### FLUX 3 Max

FLUX 3 Max currently runs
on fal and is billed through your fal account. See fal’s model pages for current
text-to-video,
image-to-video and
keyframes-to-video prices.
### FLUX 3 Image

FLUX 3 Image is priced per image by `resolution`:
Requests with `width` and `height`, or with `resolution` set to `match_input`, are billed at the smallest tier with at least as many pixels. Anything above about 4 megapixels is billed at the `4k` price.
The submission response includes the `cost` of each request.
### FLUX 3 Image Klein

FLUX 3 Image Klein is priced per image by output resolution. Text to image and image editing cost the same.
FLUX Video Upscale is priced **per output megapixel-second**: the megapixels per output frame multiplied by the output duration in seconds.
¹ One megapixel-second is one megapixel (1,048,576 pixels, i.e. 1024 × 1024) of output resolution for one second of output video. Charges are calculated on the delivered output’s actual frame dimensions and duration: megapixels per output frame × output duration in seconds, with fractional values charged pro rata. Example: upscaling a 10-second clip to 1080p (1920 × 1080 = 1.98 megapixels) is approximately 19.8 megapixel-seconds, i.e.  (Precise) or  (Creative).
² Charges are based on output only. Input resolution, upscale factor and optional prompts are not charged separately.
³ Rates apply to output at the standard frame rate of 24 frames per second. Where output at a higher frame rate is requested and supported, charges scale proportionally with frame rate (equivalently, the listed rate ÷ 24 per megapixel per frame). Supported input durations, resolutions, upscale factors and frame rates are set out in the documentation.
⁴ Input videos up to 20 seconds. Sources longer than 20 seconds are rejected before processing; they are not truncated, and no charge applies.
FLUX Video Edit [fast] is priced **per second of output video**. The output keeps the source clip’s length.
### FLUX.2 Models

FLUX.2 uses **megapixel-based pricing** — cost scales with output resolution.
**Klein pricing explained:** The first megapixel costs a flat rate, then each additional MP adds to the total. For example, a 2MP image with Klein 4B costs  +  = .

### Fine-tuned Endpoints (Public Beta)

During public beta, fine-tuned FLUX.2 endpoints are billed at the **same rate as their base endpoints**. For example, a request to `flux-2-klein-9b-kv-finetuned` costs the same as the equivalent `flux-2-klein-9b` request at the same resolution.
Pricing for fine-tuned endpoints may change once out of public beta. See the LoRA inference guide for details on how to call these endpoints. ### FLUX.1 Models (Previous Generation)

## Batch Pricing

**Batch requests multiply the base cost by the number of images**

**Example:** FLUX.2 [pro] batch of 4 images
- Single image: from 
- Batch of 4: from 

## Getting Started

## Quick Start

Create account, add credits, and get API keys

## Generate Images

Start creating with our API

## Try Playground

Try our models without writing code
