---
id: collect-261121-veille-ia/divers-tech/apple-fastframes-open-weights-36-ms-interactive-video
title: "Apple FastFrames Open Weights: 36 ms Interactive Video"
domain: divers-tech
role: reference
task: article
actors: []
dates: ["2026-11-16"]
keywords: ["apple", "model", "weights", "frame", "latency", "video", "card", "open"]
source: https://aitecharchive.com/articles/apple-fastframes-open-weights
source_anchor: ""
source_lines: [1, 79]
section: "Divers_Tech"
---
## TL;DR

- Apple and Cornell published FastFrames-S (1.3B) and FastFrames-L (14B) weights on November 16, 2026 (HF model card).
- FastFrames-S: 36 ms per frame, 28 responses per second, on one Nvidia B200 (arXiv paper).
- FastFrames-L: 45 ms per frame on eight B200 GPUs, with better realism (project page).
- The bottleneck was not the transformer: VAE decode and KV-cache updates were over half of latency, so Apple replaced both with small learned networks (arXiv paper).
- Licenses: code is Apple Sample Code License, the LingBot demo is CC BY-NC-SA 4.0, weights carry the AMLR tag - open weights, not Apache-2.0 open source.

## What Apple shipped on November 16, 2026

The FastFrames paper was submitted to arXiv on November 17, 2026 by Gene Chou, Guandao Yang, Fujun Luan, Lars Mescheder, Wei Dong, Stephan Richter and Vladlen Koltun - a joint Apple and Cornell team. The Hugging Face repositories were created a day earlier, on November 16, 2026, per the FastFrames-L model card, which also states both models are "autoregressive, two-step video diffusion models that generate 480p video at interactive latency" under 50 ms.

| Model | Parameters | Latency | Hardware | Output | 
|---|---|---|---|---|
| FastFrames-S | 1.3B | 36 ms per frame, 28 RPS | 1x Nvidia B200 | 832 x 480, 81 frames at 16 fps | 
| FastFrames-L | 14B | 45 ms per frame, 22 RPS | 8x Nvidia B200 | same format, better realism | 

Both checkpoints are ungated: no request form, no approval queue - download and run locally. One generation pass is 81 frames at 16 fps, about 5.06 seconds of video; the pipeline is autoregressive, so clips chain into longer takes (HF model card).

## Where the milliseconds went

The common assumption is that video-diffusion latency lives in the diffusion transformer. Apple's profiling of state-of-the-art pipelines found the opposite: operations usually treated as fixed implementation details - VAE decoding and KV-cache updates - accounted for over half of end-to-end latency (arXiv paper). The team left the diffusion transformer alone and replaced both slow components with lightweight learned alternatives: a pixel decoder for VAE decoding and a KV-refinement network for cache updates.

The component table from the FastFrames-L model card, measured for FastFrames-S on one GPU at 480p with a full KV cache:

| Component | Calls | Per call | Total | 
|---|---|---|---|
| DiT eval t=1000 | 1 | 16.1 ms | 16.1 ms | 
| DiT eval t=833 | 1 | 16.2 ms | 16.2 ms | 
| KV refinement | 1 | 2.2 ms | 2.2 ms | 
| Pixel decoder | 1 | 0.9 ms | 0.9 ms | 
| **Total** |  |  | **35.4 ms (paper: 36 ms)** | 

Our arithmetic check on the published spec: 81 frames at 16 fps is 5.06 seconds of video, and at 36 ms per frame one B200 renders that clip in about 2.9 seconds - faster than playback speed. Put differently: the two diffusion-transformer evaluations cost 32.3 ms, and everything Apple replaced contributed only about 3.1 ms. In the Causal Forcing++ baseline this build starts from, one 81-frame block costs 271.8 ms per frame, so the same pipeline now runs about 7.5x faster (OrcaRouter analysis).

## Tested setup path (Linux, single Nvidia GPU)

This procedure follows the GitHub repository and its checkpoint guide.

1. Install Linux x86-64 with an NVIDIA GPU and a CUDA 12.8-compatible driver.
2. Clone the repository and run `uv sync` from the repository root.
3. Download model and third-party weights with the checkpoint guide linked above.
4. Generate one test clip with the README command: `uv run python scripts/generate.py --model ff-s --checkpoint-dir checkpoints --prompt "A corgi surfing a wave at sunset" --seed 0 --output outputs/corgi.mp4` (HF model card).
5. Reproduce the latency table on your own card: `uv run python evaluation/latency/benchmark.py ff-s` - add`--gpus 8` for the L variant.
6. Try the interactive demo once the base model is set up: `python apps/lingbot/demo.py --gpus 1` , then open port 8000 on localhost.

## Limitations a buyer should price in

- Resolution is capped at 480p, 16 fps; there is no 720p or 4K path at release.
- The headline numbers assume B200-class datacenter GPUs; Apple published no consumer-GPU figures.
- One pass is an 81-frame clip, about 5.06 seconds; longer video means chaining, with drift risk.
- Output carries no audio track.
- There is no hosted API: Apple serves no FastFrames endpoint, so there is no pricing or SLA - self-hosted only (OrcaRouter analysis).
- Every latency and quality figure is author-reported; nothing has been independently benchmarked as of November 18, 2026 (Superpower Daily).
- Check the license layer before commercial use: weights carry the AMLR tag, code ships under the Apple Sample Code License, and the LingBot-World v2 demo code is CC BY-NC-SA 4.0, which forbids commercial use (HF model card).

Apple's own VBench numbers (author-reported) put FastFrames-S at 84.23 total and FastFrames-L at 84.44, with L realism comparable to Krea-Realtime at over 10x the speed (project page).

## FAQ

**Q: What did Apple release on November 16, 2026?**
A: FastFrames-S (1.3B) and FastFrames-L (14B) video-generation weights, ungated on Hugging Face (HF model card), plus an optimized LingBot 1.3B that runs 16x faster than the official model.

**Q: Is FastFrames open source?**
A: The weights are open to download and self-host, but the code is not OSI open source - it ships under the Apple Sample Code License, the LingBot demo under CC BY-NC-SA 4.0 (HF model card).

**Q: Does FastFrames-S really run almost 8x faster?**
A: Nearly 8x on the same pipeline: the Causal Forcing++ baseline costs 271.8 ms of frame and FastFrames-S does 36 ms (OrcaRouter analysis).

**Q: How long is one FastFrames clip?**
A: 81 frames at 16 fps, about 5.06 seconds of 480p video per pass; runs chain autoregressively for longer output.

## Related read next

*Every claim here is traced to a primary source, dated, and listed under Sources. Research and drafting are AI-assisted; editing, verification and publication are human decisions, and a person is accountable for what appears on this page. How we work →*

## Discussion

0 comments
