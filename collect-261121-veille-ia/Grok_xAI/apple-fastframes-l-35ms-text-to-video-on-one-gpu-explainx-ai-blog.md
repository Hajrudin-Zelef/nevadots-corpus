---
id: collect-261121-veille-ia/grok-xai/apple-fastframes-l-35ms-text-to-video-on-one-gpu-explainx-ai-blog
title: "Apple FastFrames-L: 35ms Text-to-Video on One GPU | explainx.ai Blog"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: []
keywords: ["apple", "fastframes-l", "latency", "fastframes", "video", "model", "demo", "lingbot"]
source: https://explainx.ai/blog/apple-fastframes-l-interactive-text-to-video-open-weights-2026
source_anchor: ""
source_lines: [1, 209]
section: "Grok_xAI"
---
**Apple ML Research open-sourced FastFrames, a family of autoregressive two-step video diffusion models that target interactive latency at 480p**, with weights on Hugging Face and code at github.com/apple-aiml-research/ml-fastframes. The project page positions FastFrames-S (1.3B) at roughly **36 ms per frame** on one B200-class GPU and FastFrames-L (14B) at **45 ms per frame** on eight GPUs, while matching or beating slower baselines on quality metrics.

This is not an iPhone photo filter. It is a **research release** for builders who want **local, camera-steerable video generation** with measurable frame latency.

## What Apple shipped

Apple lists four deliverables on the FastFrames-L model card:

| Asset | Size | Role | 
|---|---|---|
| FastFrames-S | 1.3B | Speed-first autoregressive generator | 
| FastFrames-L | 14B | Higher-quality distillation from Wan2.1-14B | 
| LingBot optimized | 1.3B | 16x faster camera-conditioned control | 
| Code + checkpoints | GitHub + HF | `uv sync` , benchmarks, demo server | 

Outputs are **832 x 480**, **81 frames**, **16 fps** for standard text-to-video generation via `scripts/generate.py`.

## Why latency, not just FVD

Open model weights release metaphor for Apple FastFrames Hugging Face checkpoints

Most video diffusion stacks spend multiple seconds per clip. That is fine for offline ads, but **useless for live camera steering** where each new keystroke or joystick move should refresh the scene.

The FastFrames abstract argues prior autoregressive models were **at least an order of magnitude too slow** for fluid interaction. Apple's response is engineering on the **whole pipeline**, not only the transformer:

- Replace heavy **VAE decode** with a lightweight pixel decoder path
- Treat **KV cache refresh** as a first-class cost center
- Keep generation at **two diffusion steps** per frame via Causal Forcing++

On one GPU, the model card breaks down FastFrames-S component timing:

| Stage | Latency | 
|---|---|
| DiT at t=1000 | 16.1 ms | 
| DiT at t=833 | 16.2 ms | 
| KV refinement | 2.2 ms | 
| Pixel decoder | 0.9 ms | 
| **Total** | **35.4 ms** | 

Those numbers come from Apple's published benchmark script `evaluation/latency/benchmark.py ff-s`.

## FastFrames-L and the Wan2.1 distillation chain

FastFrames-L is not trained from scratch in one shot. The README on Hugging Face documents a **four-checkpoint distillation ladder** from Wan-AI/Wan2.1-T2V-14B:

1. Causal teacher on Wan2.1 latents
2. Consistency-distilled two-step causal model
3. DMD-distilled model
4. Final generator plus pixel decoder and KV refinement nets

FastFrames-S instead **freezes** a frame-wise Causal Forcing++ generator and trains surrounding networks.

For readers tracking open video stacks, this sits beside **MBZUAI PAN 2** (simulative reasoning) and consumer tools like **Suno Visuals** (music video). FastFrames optimizes **per-frame responsiveness**, not long narrative coherence.

## Interactive demo: LingBot and camera control

FastFrames demo showing LingBot camera control optimized for interactive video generation latency

*Image: Apple ML Research*

Apple highlights **camera-conditioned** interaction. After demo setup, you can run:

```
CUDA_VISIBLE_DEVICES=0 uv run --extra lingbot --extra demo python apps/lingbot/demo.py --gpus 1
```
Then open `http://localhost:8000`. An eight-GPU path exists for throughput testing.

The project page hosts side-by-side clips comparing **Causal Forcing++**, **Krea**, and **FastFrames** variants on shared prompts. That is the fastest way to see what "interactive latency" buys visually before you download weights.

## How to install and run text-to-video

Requirements from Apple's README:

- **Linux x86-64**
- **NVIDIA GPU** with**CUDA 12.8** driver
- **`uv`** package manager

Workflow:

1. Clone `apple-aiml-research/ml-fastframes`
2. `uv sync` at repository root
3. Download checkpoints per the checkpoint guide in `docs`
4. `uv run python scripts/generate.py --model ff-s` or`ff-l`

Latency sweep:

```
uv run python evaluation/latency/benchmark.py ff-s
uv run python evaluation/latency/benchmark.py ff-l --gpus 8
```
Quality reproduction uses the **VBench** guide referenced on the model card for Table 1 **Quality / Semantic / Total** scores.

### Apple text to video versus iPhone editing

Searchers often confuse **FastFrames** with **adding text captions in Apple Photos**. FastFrames is **generative video from prompts** on a Linux workstation, not an iOS editing feature. If you need on-device SLMs, see **Mobile AI Factory** instead.

## Licensing and what you can ship

Research and internal demos are the natural fit. **Commercial redistribution** requires a legal read of Apple's sample code terms and LingBot's NC clause.

## What developers should benchmark first

Before you compare FastFrames to a hosted API:

1. **Cold versus warm latency:** first`generate.py` run includes compilation and IO.
2. **GPU class:** Apple quotes B200-class nodes; your 4090 numbers will differ.
3. **Prompt suite:** use`scripts/generate_project_page.py` for the 20+ standard prompts.
4. **Camera demo path:** test LingBot if your product needs**live control** , not batch MP4s.

## Training pipeline: Causal Forcing++ to product checkpoints

Apple documents two training recipes on Hugging Face:

**FastFrames-S (1.3B):** Start from a **frame-wise Causal Forcing++** generator, keep it **frozen**, and train peripheral networks. That limits drift in the causal backbone while you optimize decoders.

**FastFrames-L (14B):** Distill **Wan2.1-14B** into a **two-step causal student**, then apply the same **pixel decoder + KV refinement** recipe as FastFrames-S. Released artifacts include the **teacher**, **consistency** model, **DMD** student, and final generator bundle.

For researchers comparing distillation fidelity, Apple publishes **project page videos** with shared seeds across **Causal Forcing++**, **Krea**, and **FastFrames** exports. Use those clips before you trust a single VBench row.

### Hardware expectations

| Workload | Documented hardware | 
|---|---|
| FastFrames-S latency bench | 1 GPU, 480p, full KV cache | 
| FastFrames-L latency bench | 8 GPUs ( `--gpus 8` ) | 
| LingBot interactive demo | 1 or 8 GPUs | 
| Paper headline (S) | Single B200, ~36 ms/frame | 
| Paper headline (L) | 8x B200 node, ~45 ms/frame | 

Consumer **RTX 4090** runs may work with smaller batches, but Apple does not list them as reference platforms. Benchmark before you promise SLA numbers in a product.

## Cost versus hosted video APIs

Closed APIs charge per second or per job with opaque queueing. FastFrames shifts cost to **your GPU-hour bill** plus **engineer time** downloading Wan-derived checkpoints.

| Factor | Self-hosted FastFrames | Hosted video API | 
|---|---|---|
| Latency control | You measure `benchmark.py` | Vendor queue + network | 
| License | Apple sample code + NC LingBot | Terms of service | 
| Custom prompts | Full `generate.py` access | Rate limits | 
| Camera steering | LingBot demo path | Often unavailable | 

For teams already running **vLLM Rubin throughput experiments**, FastFrames is a separate stack (uv, DiT, KV refinement). Do not assume one CUDA environment serves both without container separation.

## Evaluation and reproducibility

Apple points to **VBench** reproduction for Table 1. The workflow in the model card:

1. Install per README.
2. Download checkpoints listed in the checkpoint guide.
3. Follow `docs/evaluation.md` (linked from Hugging Face) for quality metrics.

Run **`evaluation/latency/benchmark.py`** before quality sweeps if your claim is **interactive** generation. Marketing teams often mix latency and FVD from different tables; keep them separate in slide decks.

### Project page prompts

`scripts/generate_project_page.py` regenerates **20+** showcase prompts for both **ff-s** and **ff-l**. That script is the fastest way to build an internal A/B panel without inventing prompts that favor your fine-tune.

## Wan2.1 lineage and why distillations matter

FastFrames-L sits on the **Wan 2.1 text-to-video** ecosystem explainx.ai readers already encounter in open video research. Apple does not hide that dependency: the Hugging Face model tree shows **Wan-AI/Wan2.1-T2V-14B** as the base with **88** downstream finetunes including FastFrames-L.

Distillation buys two things:

1. **Step reduction:** two diffusion evaluations per frame instead of long schedules.
2. **KV refinement:** a dedicated network that keeps autoregressive memory sharp without full attention recomputation every frame.

If you maintain Wan forks, compare your **teacher checkpoint hashes** against Apple's published ladder before merging weights. Silent mismatch produces colorful but incoherent motion.

### When to pick S versus L

| Choose FastFrames-S | Choose FastFrames-L | 
|---|---|
| One GPU prototyping | Eight-GPU quality panels | 
| Latency demos under 40 ms | Marketing clips where detail wins | 
| LingBot-style interaction | Batch MP4 for storyboards | 

You can run both from one clone; only checkpoint downloads differ.

## Security and content policy notes

Interactive text-to-video at 16 fps enables **rapid iteration** on sensitive prompts. Apple's license is research-oriented; your product still needs **abuse monitoring** like any generative media stack.

Pair organizational policies with human review when prompts come from end users, not fixed `generate_project_page` strings.

## Common setup failures

| Failure | What to check | 
|---|---|
| `uv sync` missing CUDA | Driver must match CUDA 12.8 note in README | 
| Checkpoint not found | `--checkpoint-dir` must match download layout | 
| LingBot demo import error | Install `--extra lingbot --extra demo` | 
| Huge GIF in docs | Demo assets are large; serve from local `outputs/` | 

## How this fits Apple's wider ML posture

Apple rarely drops large generative checkpoints. FastFrames follows other open research artifacts explainx.ai has covered in the Apple orbit, including on-device efficiency work and multimodal research releases.

For builders comparing **open weights** against **Gemini or OpenAI video APIs**, FastFrames is a **latency anchor**: you trade managed scaling for **known milliseconds per frame** on hardware you rent.

## Maintainer citations

If you publish academic work using these weights, cite the BibTeX block on Hugging Face (`chou2026fastframes`). List **checkpoint revision**, **GPU count**, and whether clips came from **ff-s** or **ff-l** so others can reproduce latency claims.

Store raw MP4 outputs alongside `generate.py` logs. Apple’s project page comparisons use fixed seeds; drifting seeds make internal demos look worse than public clips even when weights match. Pin the Git commit hash in lab notes when you bisect regressions across Apple uploads.

## Related reading

*Sources: apple/FastFrames-L, FastFrames project page, and ml-fastframes GitHub as of November 18, 2026.*
