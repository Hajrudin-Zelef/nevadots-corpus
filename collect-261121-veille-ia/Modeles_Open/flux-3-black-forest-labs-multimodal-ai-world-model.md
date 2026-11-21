---
id: collect-261121-veille-ia/modeles-open/flux-3-black-forest-labs-multimodal-ai-world-model
title: "Flux 3: Black Forest Labs' Multimodal AI World Model"
domain: modeles-open
role: reference
task: article
actors: []
dates: ["2026-07-23"]
keywords: ["flux", "video", "black", "forest", "labs", "model", "image", "audio"]
source: https://morphic.com/resources/models/flux-3
source_anchor: ""
source_lines: [1, 155]
section: "Modeles_Open"
---
# Flux 3

by Black Forest Labs

Black Forest Labs' world model for image, video, and audio, with sound that matches the action.

## Key features

## Technical specifications

5–20s

Whole seconds in a single pass, or automatic to suit the prompt

720p / 1080p

On Morphic; Black Forest Labs' API adds 2K and 4K

T2V·I2V·V2V

Text, image and keyframes, or continuation of an existing clip

Native

Synchronized audio generated with every clip, on by default

## Use cases

### Video with sound in one pass

A prompt returns a clip and its audio together, so an effect or a line is already synced to the action instead of added later.

### Multi-shot sequences

Block several angles in one generation, or extend a clip with continuation, holding one character across every cut.

### Product and brand film

Materials, weight, and reflections stay honest as the camera moves, so a product reads as filmed rather than simulated.

### Localized video

Multilingual dialogue and accurate on-screen text let one scene ship in several languages without a separate title or voice pass.

### Choreographed transitions

Pin the poses and compositions a shot has to move through, and the model builds the motion between them as one continuous take.

### Explainers with real physics

Gravity, collisions, and motion follow real rules, so science and how-it-works footage stays accurate while it stays clean.

## Prompt examples

## Overview

Flux 3 is Black Forest Labs' multimodal foundation model, announced on July 23, 2026, with video generation made generally available on August 4, 2026. It jointly learns from images, video, and audio within a single unified architecture, so one model can render a moving shot and the sound that belongs to it, holding a coherent sense of how the world looks, moves, and sounds.

### What Flux 3 does differently

Most generators treat each modality as a separate problem. Flux 3 treats image, video, and audio as evidence about one underlying reality, trained together on Black Forest Labs' Self-Flow approach, with over 95% of total training compute spent on video prediction. The payoff shows up as physical consistency: the sound matches the impact, the motion obeys the mass, and a subject keeps its identity from one shot to the next. That same world understanding is what lets the backbone extend to action prediction for robotics, tested on real production tasks at Audi through the FLUX-mimic partnership.

### What has shipped, and what has not

Flux 3 Video shipped first: text-to-video, image-to-video with keyframes, and continuation of an existing clip, all at up to 20 seconds with synchronized audio, with in-place editing added through Flux Video Edit. Since then Black Forest Labs has released Flux 3 Action, an open-weights robotics model, and Flux 3 Image for image generation and editing. Still to come are the open-weight Flux 3 Dev and video generation from combined image, video, and audio references. Release dates are in Black Forest Labs' release notes.

### Flux 3 on Morphic

Flux 3 runs on Morphic, beside video models like Veo, Kling, Seedance, and Vidu, and the rest of the Flux family for images. One prompt can go to two models on the same Canvas, which is the fastest way to find out whether Flux 3 is the right call for a given shot rather than taking a launch chart's word for it.

## Simple pricing

Get started for free today, with the option to upgrade or cancel anytime.

Basic

2400 monthly credits

1 user only

All models

Workflows

Standard

was ~~3625~~, now 4713 monthly credits

1 user only

All models

Workflows

Pro

was ~~6350~~, now 8255 shared monthly credits

1 user

All models

Workflows

Max

was ~~24650~~, now 32045 shared monthly credits

1 user

All models

Workflows

Enterprise

For higher limits

Custom

pricing and billing terms

Free

For playing around

$0

forever free

## FAQs

Ideogram 4.6

Ideogram

Ideogram's newest image model, out November 2026. Exact text, precise edits, real texture.

LTX 2.5 Reframe, Upscale and HDR

Lightricks

Lightricks' finishing tools for any clip. New shapes, more pixels and HDR, with no prompt.

Seedance 2.5 Flash

ByteDance

ByteDance's faster, lower-cost edition of Seedance 2.5. Quicker drafts and more takes at 480p and 720p.

Seedream 5.5

ByteDance

ByteDance's Seedream image model, released November 2026. Clean multi-round edits, lifelike realism, and sharper text.
