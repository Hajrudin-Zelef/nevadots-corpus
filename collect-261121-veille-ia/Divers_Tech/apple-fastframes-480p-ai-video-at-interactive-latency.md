---
id: collect-261121-veille-ia/divers-tech/apple-fastframes-480p-ai-video-at-interactive-latency
title: "Apple FastFrames: 480p AI Video at Interactive Latency"
domain: divers-tech
role: reference
task: article
actors: []
dates: []
keywords: ["video", "apple", "latency", "fastframes", "fastframes-l", "causal", "model", "models"]
source: https://www.bottlerocketcontent.com/apple-fastframes-480p-video-interactive-latency-hugging-face-two-step-s-l/
source_anchor: ""
source_lines: [1, 63]
section: "Divers_Tech"
---
Apple has published two text-to-video models on Hugging Face, FastFrames-S and FastFrames-L, and describes them as autoregressive, two-step video diffusion models that generate 480p video at interactive latency.

Both run on Linux with an NVIDIA GPU and a CUDA 12.8-compatible driver.

### What Apple Released

FastFrames-S has 1.3B parameters and is finetuned from Wan2.1-T2V-1.3B.

FastFrames-L has 14B parameters and is distilled from Wan2.1-14B into a two-step causal model.

Both output 832 × 480 video at 81 frames and 16 fps, which works out to about five seconds per clip.

The FastFrames-L release includes a causal teacher, a consistency-distilled model, a DMD-distilled model, and the final generator, pixel decoder and KV refinement network.

### The Latency Figure

The only timing on Apple’s pages is for FastFrames-S.

The model page lists 35.4 ms total on one GPU at 480p with a full KV cache, and the paper reports 36 ms.

The breakdown is about 16 ms for each of two DiT passes, 2.2 ms for KV refinement and 0.9 ms for the pixel decoder.

The page doesn’t name the GPU, and it doesn’t define what each timed batch contains.

FastFrames-L has no latency figure, and its benchmark example uses 8 GPUs.

### How the Two-Step Approach Works

FastFrames-L follows the Causal Forcing++ recipe.

Causal Forcing++ is a separate May 2026 paper on distilling bidirectional video models into few-step autoregressive students, which generate video frame by frame in a causal rollout.

In that paper, a frame-wise 2-step setup beat the 4-step chunk-wise Causal Forcing baseline by 0.1 in VBench Total and 0.3 in VBench Quality, and reduced first-frame latency by 50%.

Those results belong to Causal Forcing++. Apple’s FastFrames pages don’t publish comparable numbers.

### Camera Control and Licensing

The FastFrames-L page covers text-to-video generation, a LingBot camera-control demo and VBench evaluation.

The model card lists the apple-amlr license, other FastFrames code uses the Apple Sample Code License, and the LingBot-World v2 code and weights are CC BY-NC-SA 4.0.

That last license is non-commercial, so the camera-control demo carries different terms from the rest of the release.

The L page lists demo Spaces, and no Inference Provider currently hosts FastFrames-S.

### Competitive Context

FastFrames is a research release at 480p and about five seconds per clip, so it sits well below the resolution of the video tools filmmakers use for finished work. BRC covered the higher-resolution end in Seedance 2.5’s 4K, 10-bit enterprise output.

Speed is the point here. For a lower-cost way to test prompts before committing to a full render, see how to use Seedance 2.5 draft mode.

Apple has taken a different route before. STARFlow-V used normalizing flows instead of the diffusion models behind tools like Sora and Veo. FastFrames goes back to diffusion and cuts the sampling down to two steps.

Mac users have a separate option. In August 2026, UCSD’s Hao AI Lab released FastMetal, three open-source video models at 1.3B, 5B and 14B that run on Mac chips through Metal and MLX. FastFrames, by contrast, lists NVIDIA GPUs as its requirement.

### The Signal in the Noise

FastFrames-S has a published latency figure and a 1.3B footprint, while FastFrames-L has neither. The “interactive” claim is supported by numbers for the smaller model only.

Apple’s page doesn’t state an intended use or list limitations. The pairing of a streaming-style generator with a camera-control demo points toward live previs and fast prompt testing, but that is an inference from the release, not something Apple says.

The apple-amlr license terms and the full paper are the two items to read before building on it.
