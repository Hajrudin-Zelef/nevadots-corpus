---
id: collect-261121-veille-ia/divers-tech/fastframes-by-apple-real-time-video-model-36-ms-frame-ai-tldr
title: "FastFrames by Apple: Real-Time Video Model, 36 ms/Frame | AI/TLDR"
domain: divers-tech
role: reference
task: article
actors: []
dates: ["2026-11-17"]
keywords: ["apple", "video", "fastframes", "model", "models", "frame", "latency", "real-time"]
source: https://ai-tldr.dev/releases/apple-fastframes/
source_anchor: ""
source_lines: [1, 36]
section: "Divers_Tech"
---
Apple Â· 2026-11-17 Â· notable

# FastFrames â Apple's video models generate 480p frames in under 50 ms

In short

Apple's FastFrames are 1.3B and 14B autoregressive video diffusion models that reach interactive latency at 480p: 36 ms per frame on one B200 for the small model, 45 ms on an 8x B200 node for the 14B.

Apple Â· 2026-11-17 Â· 4 sources checked

Apple speeds up autoregressive video diffusion to interactive latency by fixing the decoder and KV-cache bottlenecks.

## Key specs

| Latency (1.3 b, 1x b200) | 36 ms / frame | 
|---|---|

## What is it?

FastFrames is a pair of text-to-video models from Apple researchers: FastFrames-S (1.3B) and FastFrames-L (14B, fine-tuned from Wan2.1-T2V-14B). Both generate 480p video one chunk at a time fast enough to react to input, and FastFrames-L sits in the Hugging Face trending top 10.

## How does it work?

The paper profiled existing streaming video pipelines and found VAE decoding and KV cache updates took over half of end-to-end latency. FastFrames swaps them for a KV cache refinement step and a lightweight pixel decoder, plus systems work that speeds up diffusion sampling. The authors report one generation step falling from 271.8 ms to 35.8 ms.

## Why does it matter?

The 1.3B model matches the quality of a model nearly 8x slower, and the project page shows the method speeding up LingBot camera-controlled generation by 16x, so it applies to interactive world models, not only clips. Licensing is restrictive: Apple's research license on the weights, Apple Sample Code License on the code, and CC BY-NC-SA 4.0 for the LingBot parts.

## Who is it for?

researchers building real-time video and world models

## Try it

`https://github.com/apple-aiml-research/ml-fastframes`
