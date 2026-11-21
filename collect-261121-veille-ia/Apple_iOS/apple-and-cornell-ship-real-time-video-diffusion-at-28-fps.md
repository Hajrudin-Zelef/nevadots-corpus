---
id: collect-261121-veille-ia/apple-ios/apple-and-cornell-ship-real-time-video-diffusion-at-28-fps
title: "Apple and Cornell Ship Real-Time Video Diffusion at 28 FPS"
domain: apple-ios
role: reference
task: article
actors: ["Apple"]
dates: ["2026-11-18"]
keywords: ["video", "diffusion", "latency", "real-time", "apple", "cornell", "single", "frame"]
source: https://emergent.sh/news/apple-cornell-ship-real-time-video-diffusion
source_anchor: ""
source_lines: [1, 26]
section: "Apple_iOS"
---
# Apple and Cornell Ship Real-Time Video Diffusion at 28 FPS

Apple and Cornell researchers have officially released FastFrames, a family of autoregressive video diffusion models that run at interactive speeds on a single GPU. Officially launched on November 18, 2026, the 1.3B-parameter variant achieves 36 milliseconds per frame, or 28 responses per second, on a single Nvidia B200 GPU while matching the visual quality of a baseline model that is nearly eight times slower. The team has published code, weights, and a project page, marking a step change in the latency profile of real-time video generation.

## Bottleneck Redefined: Decoding and KV-Cache Updates

The core insight driving FastFrames is laid out in the paper's opening analysis. When the authors profiled Causal Forcing++, the fastest few-step autoregressive video diffusion baseline at the time of writing, they found that only 30 percent of the 271-millisecond-per-frame budget at 480p was spent on diffusion sampling. The rest was consumed by VAE decoding and a DiT evaluation step used solely to refresh the key-value cache after each denoised latent. As the paper states, a single VAE decoding step takes as long as 3.5 DiT passes, a bottleneck that has been hiding in plain sight while researchers optimized the wrong stage of the pipeline.

FastFrames addresses both. The VAE decoder is replaced with a ConvNeXt-based pixel head that operates at low spatial resolution, followed by a single linear layer that unpatchifies and upsamples the latents to pixel dimensions. This change reduces decoding latency by 147 times while maintaining visual quality. The KV-cache DiT pass is swapped for a lightweight network that cuts the update cost by 10x without measurable degradation in output fidelity.

## Performance Benchmarks and Interactive Latency

The 1.3B variant runs at 36 milliseconds per frame on a single B200 GPU, delivering 28 responses per second. The 14B variant, which the authors describe as producing significantly better visual quality and realism, achieves 45 milliseconds per frame on an eight-GPU B200 node. Both models stay under the 50-millisecond delay-detection threshold cited from human-computer interaction research as the ceiling for seamless interactivity, making them viable for applications where user input and video output must feel instantaneous.

- 1.3B model: 36ms per frame, 28 RPS on a single B200 GPU
- 14B model: 45ms per frame, 22 RPS on an 8x B200 node
- Decoding latency reduced by 147x via ConvNeXt pixel head
- KV-cache update cut by 10x with lightweight network

## Downstream Applications and Open Release

The authors point to two immediate use cases enabled by the latency improvements: camera-conditioned video generation, where real-time input from a physical camera drives the diffusion process, and physics-conditioned video generation, where simulated constraints guide the output. The release includes full code and model weights, lowering the barrier for research teams and product developers working on interactive video systems. The timing aligns with a broader wave of KV-cache and pixel-decode latency work moving through AI video research in late 2026.

## What This Means

FastFrames reframes the latency conversation in autoregressive video diffusion by showing that the decoder and cache-refresh steps, not the denoising loop, were the real bottleneck. By replacing the VAE decoder with a lightweight ConvNeXt head and cutting the KV-cache update cost by an order of magnitude, Apple and Cornell have delivered a system that hits interactive speeds on commodity hardware. The open release of code and weights gives the research community a new baseline for real-time video work, and the sub-50ms latency opens the door to applications where video generation must keep pace with human interaction without perceptible delay.
