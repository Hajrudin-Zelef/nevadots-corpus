---
id: collect-261121-veille-ia/divers-tech/apple-fastframes-video-generation-at-interactive-latency
title: "Apple FastFrames: Video Generation at Interactive Latency"
domain: divers-tech
role: reference
task: article
actors: []
dates: ["2026-11-16"]
keywords: ["apple", "milliseconds", "fastframes", "video", "model", "generation", "frames", "frame"]
source: https://www.orcarouter.ai/blog/fastframes-video-generation-apple-interactive-latency
source_anchor: ""
source_lines: [1, 93]
section: "Divers_Tech"
---
Thirty-six milliseconds per frame. That is the number at the top of **FastFrames Video Generation**, the release Apple published with Cornell on 16 November 2026 — weights on Hugging Face as **FastFrames-S** at 1.3 billion parameters and **FastFrames-L** at 14 billion, a paper titled "FastFrames: Video Generation at Interactive Latency" the following day. Do the arithmetic before reading anything else: a clip is 81 frames at 832×480 and 16 frames per second, which is 5.06 seconds of video. At 36 milliseconds a frame on a single B200, the small model produces that clip in 2.9 seconds. Not faster than the alternatives — faster than playback. The interesting question is not whether the number is real. It is what Apple traded away to get it, and whether that trade is one you can live with.

## What actually shipped, and when

Two checkpoints, both text-to-video, both ungated and downloadable as of this week. The Hugging Face repositories were created on 16 November 2026 and were still being touched on the morning of 18 November; the arXiv preprint (2611.23226) is dated 17 November. So this is a two-day-old release, not a model with a track record, and nothing here has been independently evaluated. Everything that follows is either the authors' own measurement or an attribute of the published weights.

• Sizes — FastFrames-S at 1.3B parameters, FastFrames-L at 14B (14.3B counting the added heads).

• Base models — distilled from Wan2.1-T2V-1.3B and Wan2.1-T2V-14B respectively, using a recipe the authors call Causal Forcing++.

• Output — 832×480 at 16 fps, 81 frames per generation, autoregressive so clips can be chained.

• Weights — open on Hugging Face, tagged with Apple's AMLR licence; the LingBot-World v2 demo code is CC BY-NC-SA 4.0, which is non-commercial, and the rest of the repository is under Apple's Sample Code Licence.

• Pricing — none. There is no hosted FastFrames endpoint from Apple; you run it on your own GPUs.

That last line is the one to internalise. FastFrames is a research release with downloadable weights, which is a different object from a product. There is no per-second rate, no queue, no SLA, and no way to call it without a NVIDIA GPU and CUDA 12.8.

## Where the milliseconds went

The architectural idea is narrower than "a faster model" and more interesting. FastFrames is an autoregressive video diffusion model, which means it generates a block of frames conditioned on what came before. In the baseline it is built on, Causal Forcing++, generating one 81-frame block costs 271.8 milliseconds per frame, and a large share of that is not the transformer at all — it is the variational autoencoder decoding latents back into pixels, plus the step that updates the KV cache so the next block knows what happened in this one.

Apple's team replaced both ends of that loop with small learned networks and left the diffusion transformer alone.

• The VAE decoder is gone. A 63.87 million-parameter pixel decoder — three causal ConvNeXt blocks and a final linear layer emitting 4×16×16 RGB values per token — produces frames directly. Decoding drops from about 147 milliseconds to roughly 1 millisecond.

• The KV cache update is now a small network. A shared stem, four 8-head self-attention blocks, and one residual repair head per DiT layer, costing 2 milliseconds on the small model and 6 on the large one.

• The transformer itself barely changed, and is frozen during the final stage. On the 1.3B model, DiT time goes from 41.6 to 16.4 milliseconds; on the 14B, from 89.2 to 75.6.

Add those up on the 1.3B model and 271.8 milliseconds becomes 35.8. That is the whole trick, and it is a good one: the authors did not make a diffusion transformer faster, they removed the parts around it that were never the point. The pixel decoder was trained on 32 B200s for about a week over 150,000 rollouts, with an adversarial stage on 8 B200s using a frozen DINOv3 ViT-S/16 discriminator to keep it from producing soft frames.

## The quality claim, and who checked it

Here is the part that needs a label. The VBench numbers below are the authors' own, run on the same prompts and seeds at 480p, and reported in Table 1 of the paper. Nobody independent has reproduced them, and VBench is a benchmark that rewards the kind of temporal smoothness autoregressive models are good at. Treat them as a claim about parity, not a measurement of it.

• FastFrames-S — 84.23 VBench total, 85.30 quality, 79.92 semantic, at 36 ms per frame.

• FastFrames-L — 84.44 total, 85.48 quality, 80.29 semantic, at 45 ms per frame on eight B200s.

• Causal Forcing++, the baseline it was distilled from — 84.11 total at 272 ms per frame, so roughly 7.6× slower for 0.1 to 0.3 points less.

• Krea-Realtime — 84.42 total at 1,675 ms per frame; the paper claims comparable realism, prompt adherence, composition and text rendering at over ten times the speed.

• Self-Forcing — 84.10 at 717 ms; CausVid 83.18 at 662 ms; LongLive2.0 83.65 at 505 ms.

• Wan2.1 1.3B, the base model — 83.80 at 30,444 ms per frame.

• Wan2.2 14.3B — 84.65 at 117,734 ms per frame. That is the number worth sitting with: the best total on the table, at roughly 2,600 times the per-frame cost of FastFrames-L.

• MiniMax-H3 33.1B — 84.43 at 38,454 ms; HunyuanVideo1.5 8.3B — 83.18 at 1,555,508 ms.

The pattern is a flat quality plateau from 83 to 85.5 across every model on the list, and a latency spread of four orders of magnitude. Whatever VBench total measures, it stopped discriminating between these systems a while ago. What separates them now is how fast they run and what resolution and audio they can produce — and FastFrames wins the first column by a distance while losing the second outright.

## What 36 milliseconds per frame actually buys

Convert to the units a product person cares about. The authors report throughput as well as latency, and the two tell different stories depending on how many GPUs you own.

• FastFrames-S, one B200 — 36 ms per frame, about 28 requests per second. A 5-second clip in 2.9 seconds, or 1.7× faster than you can watch it.

• FastFrames-L, eight B200s — 45 ms per frame, about 22 requests per second. A 5-second clip in 3.6 seconds.

• FastFrames-L, one GPU — 158 ms per frame, about 6.3 requests per second. The same clip in 12.8 seconds, which is slower than playback. The large model only becomes interactive when you spread it across a node.

• At 720p — 127 ms per frame for the small model and 116 ms for the large one on eight GPUs, which is the point where "interactive" stops being true for a single-stream session.

• On H100 rather than B200 — the paper puts the penalty at 25 to 50 percent, so the small model lands somewhere around 45 to 54 ms per frame.

The claim that survives is the one about the small model on current hardware. FastFrames-S on one B200 is genuinely faster than real time, which is what makes a live preview loop, a streaming avatar, or a controllable world model feasible without a render farm. The 14B is interactive only in the batched-throughput sense.

## Running it yourself

The repository ships a working inference path, not just weights. You need Linux x86-64, an NVIDIA GPU and CUDA 12.8; then uv sync, and uv run python scripts/generate.py --model ff-l --checkpoint-dir checkpoints --prompt "..." --seed 0 --output outputs/clip.mp4. There is a latency benchmark script that takes a GPU count, a VBench evaluation workflow, and a demo app under apps/lingbot that serves on port 8000. Apple also published every training-stage checkpoint for the large model — the causal teacher, the consistency-distilled stage, the DMD-distilled stage, and the final generator with its pixel decoder and KV refinement network — which is unusual and makes the release reproducible in a way most "open weights" drops are not.

If you do not have the GPUs, the honest statement is that OrcaRouter does not serve FastFrames, and neither does anyone else — there is no hosted endpoint to route to. What one OrcaRouter key does cover is the hosted video models that already work today, including Kling 3.0 and Kling 3 Turbo, Kling Video O1, and MiniMax H3, at provider list pricing passed through with 0% markup, so a vendor price change reaches you the same day rather than at the next billing cycle. The sensible pattern for a team curious about FastFrames is to prototype the product idea against a hosted model whose latency you can measure, and only buy GPU time once you know the loop is worth it.

## What it cannot do

Three limits, and none of them are going to be patched next week.

• Resolution — 480p, full stop. Apple reports 720p latency figures but the released configuration is 832×480. Every competitor in this price class offers at least 1080p.

• Audio — none. FastFrames is silent. Veo, Kling, Grok Imagine and HiDream all generate synchronised audio in the same call.

• Duration and control — 81 frames per generation with autoregressive chaining, text prompts only. No image conditioning, no reference assets, no keyframes, no camera control, no extension API.

• Licence — the model weights carry Apple's AMLR terms, and the LingBot-World v2 components are CC BY-NC-SA 4.0. Read that before you build a commercial product on the demo app; "open weights" and "free to commercialise" are not synonyms and this release is deliberately careful about the difference.

## The thing to watch

FastFrames is best understood as a demonstration that the cost of autoregressive video generation was never mostly the model. It was the decode and the cache update, and Apple deleted both for roughly two milliseconds and a megabyte of parameters. That result transfers: the pixel decoder is trained per base model, but the principle — freeze the transformer, learn the plumbing — applies to any causal video diffusion stack, and the paper already shows it ported to two third-party systems, taking LingBot-World from 1,085 to 52 milliseconds per frame on the 1.3B variant and from 3,862 to 233 on the 14B.

What that means for a buyer: if your product needs a video model in a real-time loop, the hardware requirement just fell by an order of magnitude and the quality cost appears to be small. If your product needs a finished clip at 1080p with sound, nothing in this release is aimed at you, and the hosted models remain the answer. The gap between those two use cases is where the next twelve months of video model releases will be fought.
