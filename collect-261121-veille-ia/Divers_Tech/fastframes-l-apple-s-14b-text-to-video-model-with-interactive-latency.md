---
id: collect-261121-veille-ia/divers-tech/fastframes-l-apple-s-14b-text-to-video-model-with-interactive-latency
title: "FastFrames-L: Apple's 14B Text-to-Video Model With Interactive Latency"
domain: divers-tech
role: reference
task: article
actors: []
dates: ["2026-11-16"]
keywords: ["model", "apple", "latency", "video", "fastframes-l", "license", "interactive", "text-to-video"]
source: https://sciherald.com/article/fastframes-l-apple-s-14b-text-to-video-model-with-interactive-latency
source_anchor: ""
source_lines: [1, 94]
section: "Divers_Tech"
---
# FastFrames-L: Apple's 14B Text-to-Video Model With Interactive Latency

*Apple's FastFrames-L is a 14-billion parameter AI model that generates 480p video from text prompts at interactive latency under 36 milliseconds.*

By Sci Herald Editorial Team ·

Released on Hugging Face by apple — view the original study

**TL;DR:** Apple has released FastFrames-L, a 14-billion parameter AI model that generates 480p video from text descriptions. The model achieves what the researchers call "interactive latency," producing video frames in just 36 milliseconds.


**Key Facts**

- **Model Type:** 14-billion parameter autoregressive text-to-video diffusion model
- **Latency:** Generates 480p video with a total latency of 36 milliseconds
- **License:** Released under Apple's proprietary apple-amlr license
- **Base Model:** Built upon the Wan-AI/Wan2.1-T2V-14B architecture
- **Release:** Published on Hugging Face on November 16, 2026

## FastFrames-L at a Glance

| Specification | Detail | 
|---|---|
| **Publisher** | Apple | 
| **Model Size** | 14 billion parameters | 
| **Task** | Text-to-video generation | 
| **Output Resolution** | 480p (832 × 480) | 
| **Frame Rate** | 16 fps | 
| **License** | apple-amlr | 
| **Hugging Face Likes** | 354 | 

## What's New

FastFrames-L represents Apple's entry into high-speed video generation AI. The model uses a two-step diffusion process that the researchers have optimized for speed without sacrificing quality. Unlike many video generation models that can take seconds to produce output, FastFrames-L achieves what the developers describe as "interactive latency" - the ability to generate video quickly enough for real-time applications.

The model is part of a pair released by Apple, with FastFrames-S (1.3B parameters) serving as a smaller, faster alternative. Both models use an autoregressive approach, meaning they generate video frames sequentially while maintaining consistency between frames.

## Benchmarks

The model card provides detailed latency measurements for the smaller FastFrames-S variant running on a single GPU:

| Component | Calls | Latency per call | Total latency | 
|---|---|---|---|
| DiT evaluation, t=1000 | 1 | 16.1 ms | 16.1 ms | 
| DiT evaluation, t=833 | 1 | 16.2 ms | 16.2 ms | 
| KV refinement | 1 | 2.2 ms | 2.2 ms | 
| Pixel decoder | 1 | 0.9 ms | 0.9 ms | 
| **Total** |  |  | **35.4 ms** (paper: 36 ms) | 

*Scores as reported by apple on the model card; not independently verified.*

These measurements show how the model breaks down the video generation process into discrete components, with the diffusion transformer (DiT) evaluation representing the most computationally intensive step. The total latency of approximately 36 milliseconds means the model can theoretically generate about 28 frames per second - faster than standard video playback rates.

## How to Run It

The model requires Linux x86-64 with an NVIDIA GPU and a CUDA 12.8-compatible driver. Installation uses the uv package manager:

```bash uv sync` ``

After downloading the model weights and third-party components, users can generate video from text prompts using:

```bash uv run python scripts/generate.py --model ff-l --checkpoint-dir checkpoints \ --prompt "A corgi surfing a wave at sunset" --seed 0 --output outputs/corgi.mp4` ``

This command produces an 832 × 480 video with 81 frames at 16 frames per second. The repository also includes an interactive demo featuring LingBot, an optimized version that provides 16x faster performance than the original implementation and enables fluid camera-conditioned control.

## License and Commercial Use

FastFrames-L is released under Apple's apple-amlr license, which is a proprietary license agreement. The licensing terms for third-party components vary: LingBot-World v2 code and weights follow a CC BY-NC-SA 4.0 license, while other FastFrames code uses the Apple Sample Code License. Developers should carefully review all license terms before using the model in commercial applications.

## Limitations

The model card does not explicitly document limitations or potential failure modes. As with any generative AI system, users should evaluate the model's performance on their specific use cases and verify output quality and appropriateness for their applications. The researchers acknowledge building upon several existing works including Wan2.1, Self-Forcing, and Causal Forcing++, suggesting the model may share limitations common to these architectures.

## Frequently Asked Questions

### Is FastFrames-L free for commercial use?

The model uses Apple's proprietary apple-amlr license, which means commercial use requires reviewing and complying with Apple's license terms. Some components like LingBot use a CC BY-NC-SA 4.0 license that prohibits commercial use.

### What hardware do I need to run FastFrames-L?

The model requires Linux x86-64 systems with NVIDIA GPUs and CUDA 12.8-compatible drivers. The benchmarking example for the smaller FastFrames-S model uses a single GPU, while FastFrames-L benchmarking uses eight GPUs.

### Can I run an interactive demo with FastFrames-L?

Yes, the repository includes an interactive demo featuring LingBot that can be launched locally. The demo runs on localhost:8000 and provides camera-conditioned control for video generation.

### What's the difference between FastFrames-S and FastFrames-L?

FastFrames-S is a 1.3-billion parameter model while FastFrames-L is a 14-billion parameter model. The larger model likely produces higher quality video but requires more computational resources to run.

### Does FastFrames-L require an internet connection to run?

No, once downloaded and installed, the model runs locally on your hardware without requiring an ongoing internet connection for inference.
