---
id: collect-261121-veille-ia/divers-tech/apples-fastframes-shows-the-denoiser-was-never-the-bottleneck-in-interactive-video
title: "Apple’s FastFrames Shows the Denoiser Was Never the Bottleneck in Interactive Video"
domain: divers-tech
role: reference
task: article
actors: []
dates: []
keywords: ["fastframes", "video", "decoder", "model", "interactive", "frame", "diffusion", "apples"]
source: https://medium.com/@sajjadpakdamansavoji/apples-fastframes-shows-the-denoiser-was-never-the-bottleneck-in-interactive-video-589b4109f380
source_anchor: ""
source_lines: [1, 124]
section: "Divers_Tech"
---
# Apple’s FastFrames Shows the Denoiser Was Never the Bottleneck in Interactive Video

## Swap the VAE decoder and the KV refresh for small networks and a 1.3B Wan model drops from 272 ms to 36 ms per frame.

*FastFrames interactive demo poster. Credit: Apple, FastFrames project page. (**source**)*

Most work on fast video generation tries to cut denoising steps. Apple’s FastFrames suggests that is mostly the wrong place to look. The authors profile the fastest prior 1.3B model, Causal Forcing++, and find that only about 30% of its 272 ms per frame is spent on diffusion sampling.

If you are building anything interactive on top of a video model (a world model, a game, a robotics simulator), the practical result is this: the same 1.3B Wan 2.1 backbone drops to 36 ms per frame on one B200 at 480p, with similar VBench scores. The authors report that this is the first autoregressive video diffusion model to land under their 50 ms interactive target.

It is trending. The 14B model, FastFrames-L, is the #3 trending model on Hugging Face (281 likes), and the paper has 27 upvotes on HF Daily Papers and 16 votes on alphaXiv this week. The repo ships a profiling script, so the numbers are checkable.

*The apple/FastFrames-L page on Hugging Face, currently #3 trending. Credit: Hugging Face, apple/FastFrames-L. (**source**)*

## What it is

FastFrames is a family of autoregressive, two-step video diffusion models. “Autoregressive” here means the model generates one latent frame at a time and conditions on earlier frames through a KV cache, so it can react to new inputs such as a camera command. Two sizes are released: FastFrames-S (1.3B) and FastFrames-L (14B), both built on Wan 2.1 and generating 480p video (832x480).

Each frame in these models goes through three stages: update the KV cache with the previous frame, run a few denoising steps, then decode the latent to pixels with a VAE (the network that maps compressed latents back to RGB). The paper’s argument is that the first and third stages are treated as fixed plumbing, and they turn out to dominate the wall time.

*Latency profile of one autoregressive frame: only about 30% of Causal Forcing++’s 271 ms is diffusion sampling, and FastFrames cuts latency nearly 8x. Credit: Chou et al., FastFrames, Figure 2. (**source**)*

The numbers for Causal Forcing++ on one B200: each DiT (diffusion transformer) evaluation takes 41.6 ms. The KV update is one more DiT evaluation. The VAE decoder takes 147 ms. Total: 271.8 ms. In the authors’ words, “dominant costs lie not in diffusion denoising steps, but rather in operations often treated as fixed implementation details, such as VAE decoding and KV cache updates.” A single VAE decoding step costs as much as 3.5 DiT passes.

## How it works: shrink the accessories, then fix the systems

The 50 ms budget leaves room for roughly one diffusion step at that profile, so the generator cannot simply be tuned. FastFrames keeps the DiT frozen (FastFrames-S starts from the two-step Causal Forcing++ checkpoint) and replaces what surrounds it.

*The two stages that get small networks: the KV refresh and the pixel decoder, with denoising left as two DiT steps. Diagram: made for this article from the FastFrames paper. (**source**)*

### The VAE becomes a readout

The pixel decoder is three causal 3D ConvNeXt-style blocks that run on the transformer’s low-resolution 21x30x52 token grid. A final linear layer turns each token into a 4x16x16 RGB patch. It has 63.9M parameters against 73.3M for the Wan VAE decoder, but takes 1 ms instead of 147 ms. A single linear layer alone already gives blurry but recognizable frames, which is what motivated the design.

*Wan VAE (left), a single linear layer (middle) and the FastFrames decoder (right), which renders hair and raindrops. Credit: Chou et al., FastFrames, Figure 3. (**source**)*

The small decoder has a second benefit. A decoder update needs about 68 GB of peak GPU memory for the VAE but about 5 GB for the pixel decoder (batch size one, 81 frames, 832x480). That makes end-to-end training in pixel space with adversarial losses practical. The authors use an internal set of 10K real videos and a frozen DINOv3 ViT-S/16 discriminator, and they attribute the sharper output of FastFrames-S over Causal Forcing++ (same DiT weights) to this training. Their summary: “we can turn pixel decoding into a lightweight readout rather than a general-purpose reconstruction problem.”

### The KV refresh gets a repair network

Baselines run an extra DiT pass on the clean latent only to overwrite the noisy keys and values in the cache, then discard the output. Removing that pass naively “leads to substantial degradation.” FastFrames instead uses a shared extractor (the DiT’s frozen patch embedding plus four small transformer blocks) feeding per-layer heads that predict residual corrections to the noisy keys and values.

*Refinement with an extra DiT pass is costly but effective, and removing it naively degrades output. Credit: Chou et al., FastFrames, Figure 6. (**source**)*

This takes about 2 ms for 1.3B and 6 ms for 14B on a B200, around 20x and 12x faster than the baselines’ refinement, with no measurable quality loss reported.

### Systems work does the rest

With the accessories gone, the DiT is the bottleneck again, so the last step is ordinary engineering: persistent KV buffers, fused QKV projections, split-KV attention and CUDA graphs. Splitting a 32,760-token KV cache into five segments gave a 13% latency reduction on its own. A DiT evaluation drops from 41.6 ms to 16.4 ms (1.3B, one B200) and from 89.2 ms to 75.6 ms (14B). On 8x B200 with context parallelism it falls to 5.8 ms (1.3B) and 20.3 ms (14B). The authors tried tensor parallelism, head parallelism and KV sharding too, and report that only context parallelism paid for its communication cost.

The model card gives example component timings for FastFrames-S that you can compare with the 272 ms profile above:

*Same 1.3B DiT weights, different surroundings: the pixel decoder and the KV update shrink the most. Diagram: made for this article from the FastFrames paper and model card (card total 35.4 ms, paper 36 ms). (**source**)*

## What the results show

The comparison that matters is against models with the same backbone. In Table 1 (VBench Total, Quality, Semantic):

- **Causal Forcing++ (1.3B):** 272 ms, 84.11 / 85.09 / 80.20.
- **FastFrames-S (1.3B):** 36 ms on one B200, 84.23 / 85.30 / 79.92. On 8 GPUs it takes 14 ms.
- **Krea-Realtime (14.3B):** 1,675 ms, 84.42 / 85.08 / 81.81.
- **FastFrames-L (14B):** 158 ms on one GPU and 45 ms on 8 GPUs, 84.44 / 85.48 / 80.29.

So FastFrames-S matches the Total and Quality scores of the fastest baseline at roughly one eighth of the latency, and FastFrames-L matches Krea-Realtime’s Total score at over 10x lower latency. Semantic is the exception: FastFrames-S is slightly lower than Causal Forcing++ (79.92 vs 80.20), and FastFrames-L is lower than Krea-Realtime (80.29 vs 81.81). Earlier 1.3B models are further behind in the paper’s steady-state measurement: Self-Forcing at 717 ms and CausVid at 662 ms.

*FastFrames-S vs Causal Forcing++ at 1.3B, and FastFrames-L vs Krea-Realtime at 14B; look at the sculpture detail, the origami dragon and the legible “stay curious” text. Credit: Chou et al., FastFrames, Figure 4. (**source**)*

### Why RPS, not FPS

The paper also argues for a different metric. “FPS alone does not establish whether a model can respond to user inputs in real time.” A model that generates a 30-frame clip as one batch in a second reaches 30 FPS, but the user still waits a second. The authors instead count responses per second (RPS = 1,000 / latency in ms), measured from input to the first frame that shows its effect, and cite delay-detection thresholds of about 50 to 60 ms, which gives a 20 RPS target.

By that yardstick, Causal Forcing++ reaches 3.68 RPS, 5.4x short of the target. FastFrames-S reaches 27.8 RPS and FastFrames-L on 8 GPUs reaches 22.2 RPS. The same logic is why the paper discounts batching and pipeline parallelism: they raise throughput but do not improve per-frame latency.

## The tricks transfer, with a cost

The authors apply the same techniques to two off-the-shelf action-conditioned models. LingBot 1.3B (camera control) drops from 842 ms (official, 2 GPUs) to 52 ms on 8 GPUs, a 16x speedup. RealWonder (physics conditioned) drops from 1141 ms to 51 ms. LingBot 14B drops from 1373 ms to 233 ms on 8 GPUs.

*LingBot camera control, official implementation vs FastFrames optimizations, plus a latency chart; a single keypress gets a response 16x faster. Credit: Chou et al., FastFrames, Figure 5. (**source**)*

These stay slower than the text-to-video models. The paper says they need more sampling steps (four for LingBot, three for RealWonder), predict multiple latent frames per step and need extra compute for conditioning.

## Try it

The repo needs Linux x86–64, an NVIDIA GPU and a CUDA 12.8-compatible driver. Install with `uv sync` and download the weights using the checkpoint guide. Then reproduce the per-frame numbers:

```
uv run python evaluation/latency/benchmark.py ff-s
uv run python evaluation/latency/benchmark.py ff-l --gpus 8
```
Generate an 832 x 480, 81-frame video at 16 fps (use `--model ff-l` for the 14B model):

```
uv run python scripts/generate.py --model ff-s --checkpoint-dir checkpoints \
    --prompt "A corgi surfing a wave at sunset" --seed 0 --output outputs/corgi.mp4
```
And launch the LingBot camera-control demo on one GPU, served at `http://localhost:8000` (after the demo setup in the docs):

`CUDA_VISIBLE_DEVICES=0 uv run --extra lingbot --extra demo python apps/lingbot/demo.py --gpus 1`
*Demo preview from the model card: LingBot camera control with and without FastFrames optimizations. Credit: Apple, apple/FastFrames-L model card. (**source**)*

The repo also includes a VBench evaluation guide and training recipes, and the 14B release includes the intermediate checkpoints (causal teacher, consistency-distilled and DMD-distilled models).

## Limitations and open questions

The headline 272 ms to 36 ms speedup is for the 1.3B model on one B200. The 14B FastFrames-L, which is the trending model, needs an 8x B200 node to reach 45 ms and takes 158 ms on one GPU.

*Only the three FastFrames entries in Table 1 are under the 50 ms target, and only with 8 GPUs for the 14B model. Diagram: made for this article from Table 1 of the FastFrames paper. (**source**)*

- **Scope.** Evaluation is text-to-video at about 480p on B200 GPUs. Higher resolutions, lighter hardware and action conditioning without extra latency are listed by the authors as future work.
- **Quality evidence is thin.** It is mostly aggregate VBench plus a few qualitative examples, and the Semantic sub-scores are slightly lower, so “similar” holds for Total and Quality but not every sub-score.
- **Gains come from the surroundings.** FastFrames-S keeps the Causal Forcing++ DiT frozen, so it is not a better generator. Fine-tuning the DiT at a low learning rate was unstable.
- **Per-model decoders.** The pixel decoder must be fine-tuned for each model, unlike the Wan VAE decoder, which is shared across sizes.
- **Long sessions.** Long-term consistency and physical plausibility are named as open directions, so the evidence says little about long interactive use.
- **Licensing.** The HF page shows 0 downloads last month and an apple-amlr license tag. The repo says non-LingBot code is under the Apple Sample Code License and LingBot-derived parts are CC BY-NC-SA 4.0, so check terms before commercial use.

Open questions I would want answered: how much of the sharpness gain comes from adversarial pixel-space training rather than faster components, and whether human evaluation agrees with VBench; whether the decoder and KV repair network hold up at higher resolution and longer rollouts; and whether interactive latency is reachable on hardware lighter than a B200.

## Why it matters

The transferable lesson is about method, not about Apple’s checkpoints. Before distilling or pruning denoising steps, profile the whole critical path (KV update, sampling, pixel decoding) and rank components by wall time. Here the biggest wins were in components most people never look at, and the generator itself was left untouched. Judging interactivity by input-to-response latency and RPS, rather than FPS, also makes such components impossible to ignore.

For world models and simulators used in robotics or games, this lowers the cost of closing the loop between an action and a visible response. The caveat is that today’s 45 ms for the 14B model still takes eight B200s. What to watch next is whether the readout-decoder and KV-repair ideas survive higher resolutions, longer rollouts and other backbones, and whether anyone gets the 14B model under 50 ms on fewer GPUs.

*The paper’s page on Hugging Face Papers, one of the attention signals (27 upvotes on Daily Papers). Credit: Hugging Face, FastFrames paper page. (**source**)*
