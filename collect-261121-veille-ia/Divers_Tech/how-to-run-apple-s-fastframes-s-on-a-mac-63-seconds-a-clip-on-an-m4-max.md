---
id: collect-261121-veille-ia/divers-tech/how-to-run-apple-s-fastframes-s-on-a-mac-63-seconds-a-clip-on-an-m4-max
title: "How to Run Apple's FastFrames-S on a Mac: 63 Seconds a Clip on an M4 Max"
domain: divers-tech
role: reference
task: article
actors: []
dates: ["2026-11-17"]
keywords: ["apple", "fastframes-s", "paper", "model", "card", "repo", "clip", "b200"]
source: https://blog.authon.dev/apple-fastframes-s-run-locally-mac
source_anchor: ""
source_lines: [1, 93]
section: "Divers_Tech"
---
Yes, Apple's FastFrames-S runs locally on a Mac. Apple doesn't support it, the official repo won't install on macOS, and on an M4 Max with 64 GB the first community MPS port takes about 63 seconds for a five-second 480p clip. The paper's headline number is 36 ms per frame on a B200. The fork to use and the setup checklist are further down.

Last time I wrote about FastFrames-L, and the joke wrote itself: Apple released a video model you can't run on Apple hardware. FastFrames-S is the 1.3B sibling, so the obvious follow-up is whether it fits on Apple Silicon or a normal consumer GPU. I haven't run it myself. Everything below comes from the model card, the repo, the paper and the first community port, with links.

## FastFrames-S vs FastFrames-L: the specs side by side

Both models come from the paper FastFrames: Video Generation at Interactive Latency, submitted 17 November 2026. They are autoregressive, two-step video diffusion models. Most of the speedup comes from the parts around the DiT. The authors found that VAE decoding and KV cache updates were eating more than half of end-to-end latency, so they swapped them for a small pixel decoder and a KV refinement network.

| | FastFrames-S | FastFrames-L |

|---|---|---|

| Parameters | 1.3B | 14B |

| Base | Wan2.1-T2V-1.3B (generator frozen) | Wan2.1-T2V-14B, distilled to two steps |

| Output | 832 × 480, 81 frames, 16 fps | 832 × 480, 81 frames, 16 fps |

| Paper hardware | 1× B200 | 8× B200 node |

| Paper latency | 36 ms per frame | 45 ms per frame |

| HF repo size | 1.79 GB | 174 GB |

| VRAM stated by Apple | not stated | not stated |

| Official platform | Linux x86-64, NVIDIA, CUDA 12.8 | same |

| MPS / MLX | not mentioned | not mentioned |

Sources: FastFrames-S card, FastFrames-L card, arXiv, installation docs.

Resolution and length are the same for both. 81 frames at 16 fps is roughly a five-second clip at 480p. L gets you better quality, which the paper describes as "significantly better visual quality and realism", and costs you an 8-GPU B200 node. S is the one with any chance of running on a desk.

Apple gives no VRAM figure for either model, and that's my biggest complaint about the release. For S, the repo's own latency table says "1 GPU, 480p, with a full KV cache" and leaves out which GPU it was. You have to open the paper to learn it was a B200. For a model pitched as the small one, that's an odd thing to leave off the card.

## What Apple's repo actually supports

The ml-fastframes README is blunt about it: Linux x86-64, an NVIDIA GPU, a CUDA 12.8-compatible driver. Install is `uv sync`, which pulls Python 3.10 and pinned PyTorch 2.9.1 builds from the cu128 wheel index. The `pyproject.toml` even notes that the paper's numbers were produced with exactly those builds.

On a Linux box with an NVIDIA card, the official path looks like this:

```
git clone https://github.com/apple-aiml-research/ml-fastframes
cd ml-fastframes
uv sync
# download weights per docs/installation.md, then:
uv run python scripts/generate.py --model ff-s --checkpoint-dir checkpoints \
    --prompt "A corgi surfing a wave at sunset" --seed 0 --output outputs/corgi.mp4
```
For S you need three sets of weights: `apple/FastFrames-S`, the Causal Forcing++ generator `framewise-2step.pt` from `zhuhz22/Causal-Forcing`, and `Wan-AI/Wan2.1-T2V-1.3B`. The install guide has one tip worth knowing. For generation only, you can exclude `diffusion_pytorch_model*` from the Wan download and skip its base DiT, which you only need for training.

The wrappers use cuDNN SDPA, so you don't need flash-attn. Nice on Linux, useless on a Mac, where cuDNN doesn't exist.

There's no MPS code path and no MLX port, and the lockfile points at CUDA wheels. Apple published a video model whose official code can't run on any Mac Apple sells.

## Someone ported it to PyTorch MPS anyway

The first thread on the model card is from user postpostmodern: discussion #1, "Running FastFrames-S on Apple Silicon (MPS)". It's the most useful thing on the page. Their numbers, on an M4 Max with 64 GB:

- About 63 s per 81-frame 480p video
- About 20 GB peak memory
- Output matches the CUDA math: everything is bit-exact except fp32 RoPE, which is "within one bf16 ulp"

The changes they had to make tell you exactly where the CUDA assumptions are baked in:

`"cuda"` replaced with device selection. cuDNN SDPA is used only on CUDA.
The fourth change is the one I'd remember. MPS SDPA caches a compiled graph and an FP32 score buffer for every input shape, and `empty_cache()` doesn't release them. The KV prefix grows with every frame, so each step is a new shape. Over 21 shapes, that held about 60 GB. Calling SDPA one head at a time brought it down to about 6 GB and ran about 12% faster. If you've ever watched a PyTorch MPS job slowly eat all your unified memory on something autoregressive, this could be why.

Their numbers add up, too. Wan's VAE compresses time 4×, so 81 frames become 21 latent frames, which matches the 21 shapes. The author reports about 3 s per latent frame, and 21 × 3 s is the 63 s total.

## So can you actually use FastFrames-S on a Mac?

For batch generation, yes. For the thing the paper is about, no. The port's author puts it plainly: "Interactive latency isn't reachable here." At around 3 s per latent frame they estimate it's roughly 100× slower than a B200. A five-second clip in about a minute is fine for trying prompts, but it's nowhere near the interactive, camera-controlled demo in the paper.

My take: the "Fast" in FastFrames describes what Apple did to the pipeline on a datacenter GPU, and none of that speed comes with you to a laptop. S still matters on a Mac because it's the first version of this work that runs there at all, and 20 GB peak means a 32 GB machine is plausible. That last part is my inference. Nobody has posted numbers from a 32 GB Mac yet.

If you want to try it, this is the checklist I'd follow:

- Use the fork linked in discussion #1, eaglstun/ml-fastframes on branch `mps-support` , with setup in`MAC.md` . Don't fight the official`uv sync` on macOS.
- Budget about 20 GB peak memory. 64 GB is the only configuration anyone has reported.
- Install ffmpeg with libx264, because the bundled encoder won't work.
- If memory keeps climbing during generation, check whether you're on the per-head SDPA path.

## The license: research only, so don't ship it

The weights are under the Apple Machine Learning Research Model License, tagged `apple-amlr`. The license text limits use "exclusively for Research Purposes" and says that "does not include any commercial exploitation, product development or use in any commercial product or service." The license is also revocable. The code is under the Apple Sample Code License, and the LingBot-World v2 parts carry CC BY-NC-SA 4.0.

So you can't build a product on it, even if your Mac renders it fast enough. If you need something for an actual project, look at FastMetal-5B-QAD from FastVideo. It's a Wan 2.2 TI2V 5B distill with an INT8 DiT, targets MPS directly, outputs 704×1280 at 121 frames, and is Apache-2.0. It publishes no speed numbers, so I can't say whether it's faster, but it was built for Macs and you're allowed to ship it.

## What are you running on your Mac?

Has anyone gotten a local text-to-video model running at usable speed on a Mac? Tell me your chip, RAM, model and seconds per clip. I'd especially like to know whether FastFrames-S fits on a 32 GB machine, or whether the 64 GB M4 Max number is the floor.
