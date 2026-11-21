---
id: collect-261121-veille-ia/modeles-open/apple-fastframes-open-research-video-models-that-generate-480p-frames-in-under-50-ms
title: "Apple FastFrames: open research video models that generate 480p frames in under 50 ms"
domain: modeles-open
role: reference
task: article
actors: []
dates: []
keywords: ["models", "apple", "comfyui", "video", "frames", "model", "fastframes", "research"]
source: https://thereelstack.com/lab/apple-fastframes-open-video-models-interactive-latency-wan-2-1
source_anchor: ""
source_lines: [1, 33]
section: "Modeles_Open"
---
# Apple FastFrames: open research video models that generate 480p frames in under 50 ms

Apple researchers released FastFrames, two open-weight text-to-video models (1.3B and 14B) built to generate frames fast enough to respond live to keyboard or drag input. They're research-only and output 480p, so this isn't a production tool yet, but it's a clear look at where real-time previs and interactive world models are heading.

What dropped. The weights went up on Hugging Face (FastFrames-S at 1.3B and FastFrames-L at 14B). The code repo went public at 2:30 PM MST Nov 17, with a project page and an arXiv paper marked 'coming soon'. Both models are autoregressive, two-step video diffusion models: they make the next frames from what came before plus any new input, instead of rendering a whole clip at once. The text-to-video script outputs 832×480 at 16 fps, 81 frames per run. FastFrames-L is trained from Wan2.1-T2V-14B.

The speed claims, per Apple. The 1.3B model runs at about 36 ms per frame (about 28 responses per second) on a single B200 GPU, and the authors say it matches the quality of a state-of-the-art model that's roughly 8× slower. The 14B model hits about 45 ms per frame, but on an 8×B200 node, with noticeably better realism. Apple also released an optimized build of the LingBot 1.3B world model that it says runs 16× faster, which is what powers the keyboard-navigation demo. A second demo lets you drag objects around with physics (RealWonder).

The interesting finding. When the team profiled current models, they found that parts usually treated as fixed overhead, like decoding frames out of the VAE and updating the KV cache, took up more than half of total latency. FastFrames swaps those for a lightweight pixel decoder and a small KV refinement network. The repo includes latency benchmarks, VBench evaluation and training recipes for both sizes.

Who can use it. The release needs Linux x86-64, an NVIDIA GPU and CUDA 12.8, and the headline speeds come from B200-class hardware. The license is Apple's Machine Learning Research Model license, which permits research purposes only, so no client or commercial work. There's no ComfyUI node yet.

Why video makers should care. A model that answers in under 50 ms per frame means you could steer previs, camera blocking or a game-like world in real time instead of waiting for a render. 480p and a research license put that a few steps away from a real pipeline. Keep an eye on it if you follow world models or interactive storyboarding.

Signals: weights uploaded Nov 16 and the code went public Nov 17. It was picked up by @_akhaliq, Hugging Face's @victormustar and @HuggingPapers in the afternoon. No official Apple social post or GitHub Trending yet, so this isn't HOT by our rule.

## Skills & repos in our directory

- FastFrames · Repo, Generation
- Comfy Previs · Repo, Generation
- seriesmith · ComfyUI, Generation
- ComfyUI Wan 3.0 templates · ComfyUI, Generation
- Comfy-Org workflow_templates · ComfyUI, Generation
- ComfyUI-WanVideoWrapper · ComfyUI, Generation

## More from the Lab

- Apple's Depth Pro 2 makes steady, metric depth maps from ordinary video, at full resolution
- PAN 2: MBZUAI's open 29B world model is now in ComfyUI, with an int8 build for 32 GB cards
- Comfy's lip-sync dubbing workflow redraws only the mouth: one shot into any language with ElevenLabs and MiniMax H3; plus Genie 4 and HeyGen Decision 1 in ComfyUI
- MiniMax H3-Image: open-weights 16 MP image model from the H3 family, with 16 references and day-one ComfyUI support

Topics: Open-weight video & audio models · Wan
