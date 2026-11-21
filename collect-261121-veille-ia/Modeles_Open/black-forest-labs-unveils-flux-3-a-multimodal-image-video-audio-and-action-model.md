---
id: collect-261121-veille-ia/modeles-open/black-forest-labs-unveils-flux-3-a-multimodal-image-video-audio-and-action-model
title: "Black Forest Labs Unveils FLUX 3, a Multimodal Image, Video, Audio and Action Model"
domain: modeles-open
role: reference
task: article
actors: []
dates: ["2026-07-23"]
keywords: ["flux", "video", "labs", "model", "black", "forest", "audio", "image"]
source: https://rits.shanghai.nyu.edu/ai/black-forest-labs-unveils-flux-3-a-multimodal-image-video-audio-and-action-model
source_anchor: ""
source_lines: [1, 38]
section: "Modeles_Open"
---
**Black Forest Labs unveiled FLUX 3 on July 23, 2026** â a multimodal frontier model that jointly learns from images, video, and audio in a single unified architecture. The German lab that made its name with the open-weight FLUX.1 image models is pivoting from still images toward what it calls “visual intelligence,” with a headline feature of text-to-video up to 20 seconds long carrying native, in-sync audio. It also marks the lab’s first move into physical AI, with a robotics variant already being tested on Audi production lines.

Rather than releasing a bigger image generator, Black Forest Labs reframed the whole product line. FLUX 3 is presented as a step toward a single model that can perceive, generate, and act across modalities. As co-founder and CEO Robin Rombach put it, “a model that only learns images can only generate images” â the argument being that learning from video and audio forces the model to build a working representation of how the physical world behaves.

## What FLUX 3 Can Do

The release is staged as a family of variants rather than one monolithic model:

- **FLUX 3 Video** â text-to-video up to 20 seconds with native, synchronized audio (dialogue, sound effects, and ambient noise), plus image-to-video, video-to-video with character consistency, keyframe-controlled transitions, multilingual dialogue, and agentic chaining for multi-shot sequences.
- **FLUX 3 Image** â synthesis and editing across styles, aspect ratios, and resolutions, with better handling of complex prompts and high-accuracy multilingual text rendering.
- **FLUX 3 Action / FLUX-mimic** â native action prediction for robot learning and dexterous manipulation, developed with Zurich-based robotics startup mimic.
- **FLUX 3 Dev** â a planned open-weight multimodal backbone, continuing the lab’s tradition of shipping open models.

Under the hood, Black Forest Labs describes a training method it calls **Self-Flow**, which it says aligns multimodal generation and understanding more efficiently than the Flow Matching approach behind earlier FLUX models. The company frames the unifying goal as a model that “must learn a representation of the world: how objects hold together, how things move, and how events sound.”

## The Benchmarks

Black Forest Labs published early, preliminary head-to-head preference results from human reviewers (it notes the model was still mid-training). FLUX 3 Video was preferred over:

- **Luma Ray 3.2** in 93% of comparisons
- **Runway Gen-4.5** in 77%
- **Grok Imagine Video** in 69%
- **Kling v3 Pro** in 60%
- **Seedance 2.0** and**Gemini Omni Flash** in 52% each â a narrow edge against the strongest competitors

The lab highlights the model’s strengths in capturing human facial expressions, tying sounds to physical events, and multilingual capability, with visual references enabling character consistency across sequences several minutes long.

## From Pixels to Robot Hands

The most unexpected part of the announcement is robotics. FLUX-mimic reuses FLUX 3’s video-prediction engine with a lightweight decoder that translates predicted frames into robot motion. Audi is testing it for flexible door-seal installation â a soft-body manipulation task the mimic team says was previously hard to automate. The system reportedly responds in roughly 101 milliseconds, comparable to human reflexes, and Black Forest Labs claims some tasks can be fine-tuned with as little as 30 minutes of robot data.

## What This Means

FLUX 3 is a limited release for now. Video and Action are in early access via API and private weights to selected partners; FLUX 3 Image is expected “in the coming weeks,” and the open-weight FLUX 3 Dev is planned for later in 2026. That staged rollout mirrors the strategy competitors like Runway, Luma, and Google (with its Gemini-based video models) have used, but the open-weight commitment keeps FLUX distinctive.

The bigger signal is direction. By folding image, video, audio, and robot action into one architecture â and shipping FLUX-mimic alongside a creative tool â Black Forest Labs is betting that the same world model that renders a convincing wave can also help a robot install a car door. Whether that unified bet holds up outside curated demos is the question the open-weight release, and independent benchmarks, will eventually answer.

*This post was drafted with AI assistance and reviewed by RITS staff.*
