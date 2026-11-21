---
id: collect-261121-veille-ia/modeles-open/flux-3-model-overview-multimodal-flow-models-for-image-video-audio-and-action-prediction
title: "FLUX 3 Model Overview: Multimodal Flow Models for Image, Video, Audio, and Action Prediction"
domain: modeles-open
role: reference
task: article
actors: []
dates: ["2026-07-23"]
keywords: ["video", "audio", "flux", "model", "flow", "image", "multimodal", "matching"]
source: https://huggingface.co/blog/ResterChed/flux-3
source_anchor: ""
source_lines: [1, 152]
section: "Modeles_Open"
---
*For researchers and MLOps practitioners*

| Field | Value | 
|---|---|
| **Developer** | Black Forest Labs | 
| **Model type** | Multimodal flow matching foundation model (Diffusion Transformer) | 
| **Modalities** | Image, Video, Audio, Action Prediction (text-conditioned) | 
| **Training framework** | Self-Flow (self-supervised flow matching) | 
| **Training data** | Tens of millions of hours of general video, hundreds of thousands of hours of manipulation video, plus images and audio | 
| **Video length** | Up to 20 seconds with synchronized audio; early evaluations used 10-second 720p clips | 
| **Access** | Early Access (API + private weights); FLUX 3 Dev open-weight backbone coming later | 
| **Release date** | July 23, 2026 | 

*Note: Parameter count not disclosed at time of announcement. License terms for FLUX 3 not yet published.*

FLUX 3 is a diffusion transformer (DiT) trained jointly across images, video, and audio within a unified multimodal architecture. Unlike earlier FLUX models (FLUX.1 and FLUX.2, which focused on image generation), FLUX 3 expands into video, native audio, image synthesis and editing, and action prediction through the same underlying multimodal backbone.

The core architectural thesis: images, video, audio, and actions are partial projections of the same underlying physical reality. Learning all modalities jointly forces their mutual constraints to teach the model physics — the sound has to match the impact, the motion has to obey the mass, the future has to follow from the past.

The key training innovation is **Self-Flow**, a framework developed by Chefer, Esser et al. at Black Forest Labs and MIT. Self-Flow extends conditional flow matching with a self-supervised feature reconstruction objective that produces representations good for both generation and downstream understanding tasks.

Standard flow matching learns an implicit world model by predicting paths from noise to data — but its internal representations are not well-disentangled. A model can generate a perfect video of a ball falling without exposing the concept of gravity as a separable, extractable feature.

Self-Flow addresses this by jointly optimizing:

- **Generation quality** : the standard flow matching objective across modalities
- **Representation quality** : a self-supervised feature reconstruction objective designed to make the model's internal representations more useful without relying on external representation models

In preliminary evaluations, Self-Flow improved generation quality across image, video, and audio (measured by Fréchet distance, normalized to standard flow matching = 100), while simultaneously improving manipulation success rate on finetuned robotics tasks from 42% to 71% — a +29 percentage point gain.

The reciprocal feedback loop is the mechanism: better representations → better generation → more training signal → better representations.

Public materials describe FLUX 3 as one underlying multimodal flow matching model that learns across image, video, audio, and action-related signals. BFL has not published full implementation details for the production FLUX 3 stack, so encoder, decoder, attention-layout, and serving details should be treated as undisclosed until technical documentation or weights are released.

Video accounts for over 95% of FLUX 3's training compute. This is by design: video forces the model to learn contact, motion, weight, and cause-and-effect because getting any of them wrong produces visually incorrect outputs.

Audio is the easy modality by comparison — less than 0.5% of tokens in a 720p video with audio. Once the model learns visual world dynamics, mapping visual events to their acoustic correlates is a significantly simpler learning problem.

FLUX 3 generates video up to 20 seconds with synchronized audio in a single generation. Supported generation modes:

| Mode | Description | 
|---|---|
| Text-to-Video | Generate video + audio from a text description | 
| Image-to-Video | Animate from a starting frame or use images as visual references | 
| Video-to-Video | Transform a source clip into a new context while preserving key elements (e.g., characters) | 
| Generative Continuation | Extend input video + audio predictively | 
| Keyframe-to-Video | Generate smooth transitions between defined keyframes | 
| Agentic Chaining | Link individual clips into longer multi-shot sequences | 

Additional capabilities confirmed by BFL:

- Multilingual dialogue generation
- Typography generation and animated designs
- Style diversity — from candid camcorder footage to cinematic animation

Generation dimensions: 720p resolution at 10 seconds for benchmark evaluations; model supports up to 20 seconds.

Image synthesis and editing are supported through the same unified model. BFL reports significant improvements over FLUX.2:

- Complex prompt following with fine-grained spatial relationships and attribute binding
- High-accuracy text rendering in multiple languages
- Diverse output styles across a broad range of aspect ratios and resolutions

FLUX 3 Image access via API and private weights is planned for the weeks following the July 23 announcement.

FLUX 3's understanding of physical dynamics extends to action prediction through two routes:

1. **Native action prediction** integrated directly into FLUX 3 by scaling up the Self-Flow approach
2. **Lightweight action decoder** trained on intermediate features extracted from the pretrained video prediction path

The second route — pioneered in mimic's earlier mimic-video work — decodes robot actions from the backbone's learned world representations without retraining the full model.

FLUX-mimic is a video-action model built jointly by Black Forest Labs and mimic robotics on the FLUX 3 backbone. Key results:

- **Tested and deployed with Audi** for soft-body manipulation tasks previously impossible with conventional robotics: handling seals, cables, and flexible materials in kitting, assembly, and insertion operations
- **Frozen backbone** — the action decoder outperforms previous vision-language-action (VLA) models even with a completely frozen FLUX backbone, a setting where prior VLA models fail entirely
- **Finetuned backbone** achieves state-of-the-art success rates on manipulation benchmarks
- **Self-recovery behavior** : robots that miss a grasp correct themselves without explicit programming — a capability never demonstrated in training data, inferred from the backbone's learned physics understanding
- **Latency** : backbone processes from input to world representation in under 80ms on a single RTX 5090; full-stack reaction time (including sensors, model, actuators) of 101ms

The training data for manipulation readiness includes hundreds of thousands of hours of video focused on human and robot manipulation tasks, on top of the tens of millions of hours of general video content.

*Note: All results below are from BFL's preliminary evaluations during development. The company states "evaluations are early and we expect further improvements during the early access phase." Tests used 10-second text-to-video clips at 720p with audio.*

| Comparison | FLUX 3 Preference Rate | 
|---|---|
| vs. Luma Ray 3.2 | 93% | 
| vs. Runway Gen-4.5 | 77% | 
| vs. Grok Imagine Video | 69% | 
| vs. Kling v3 Pro | 60% | 
| vs. Happy Horse v1 | 59% | 
| vs. Happy Horse 1.1 | 57% | 
| vs. Seedance 2.0 | 52% | 
| vs. Gemini Omni Flash | 52% | 

BFL highlights particular strengths in:

- Human facial expressions and subtle emotional cues
- Audio-visual synchronization (sounds matching physical events)
- Multilingual capabilities
- Character consistency across multi-shot sequences when using visual references

From the Self-Flow research phase:

| Metric | Standard Flow Matching | Self-Flow | 
|---|---|---|
| Manipulation success rate (finetuned) | 42% | 71% | 
| Training steps to target success rate | Baseline | ~50% (2x sample efficiency) | 

mimic-video (precursor approach) additionally reported up to **10x sample efficiency** for video-action models compared to vision-language-action (VLA) models. FLUX-mimic combines both efficiency gains.

In Self-Flow experiments, BFL reports improved generation quality across image, video, and audio compared to standard flow matching at equivalent compute, using normalized Fréchet-distance-style evaluations. These normalized metrics served as validation for scaling Self-Flow to FLUX 3's production training run.

BFL's phased rollout, all built from the same underlying model:

| Capability | Access | Status | 
|---|---|---|
| FLUX 3 Video (video + audio generation) | API + private weight access | Early Access — available now | 
| FLUX 3 Image (synthesis + editing) | API + private weight access | Early Access — coming in following weeks | 
| FLUX-mimic (action prediction) | Selected research & commercial partners | Active (began with mimic robotics) | 
| FLUX 3 Dev (open-weight backbone for image, video, audio, action) | Open weights | Coming later | 

Early access requests: bfl.ai/models/flux-3

1. **Unified multimodal representations** : FLUX 3's multimodal backbone offers a testbed for probing whether a single model learns truly modality-agnostic world representations, or whether different modalities remain separated inside the model's internal feature space.
2. **Self-Flow reproducibility** : Can the Self-Flow objective be applied to other generative architectures (e.g., autoregressive transformers, GANs)? Does the generation-representation feedback loop generalize beyond flow matching?
3. **Low-rank action decoding** : The lightweight action decoder approach — extracting robot commands from a frozen video generation backbone — may generalize to other embodied AI domains. Testing this with different robot morphologies or simulation environments is an open question.
4. **Cross-modal transfer** : If FLUX 3 learns physics from video, does that understanding transfer to audio-only or action-only tasks? Can the model reason about sound without seeing the source?
5. **Scaling laws for multimodal training** : With video consuming >95% of training compute, how does the marginal benefit of additional image/audio data compare? Is there a point of diminishing returns?

- **Hardware requirements** : Not yet published. Expect FLUX 3 video generation and private-weight deployment to require significant GPU memory and serving infrastructure, but BFL has not disclosed parameter count, VRAM requirements, or production inference profiles.
- **Inference framework** : FLUX models are supported through Diffusers and the BFL API. Self-hosted deployment via private weight access requires a BFL commercial license. The open-weight FLUX 3 Dev will be the primary path for community self-hosting.
- **Serving considerations** : Video generation at 720p/20s is latency-intensive. BFL's own benchmarks focus on generation quality; throughput and cost-per-second metrics are not yet published.
- **Quantization readiness** : Not yet confirmed. Prior FLUX releases and community tooling suggest quantized inference may become important for FLUX 3 Dev, but BFL has not published FLUX 3 Dev weights, quantization formats, or deployment requirements.

Based on the July 23 announcement and associated materials:

1. **Early stage** : All benchmark results are preliminary and from mid-training evaluations. BFL explicitly states "further improvements" are expected during the Early Access phase. Community-verified benchmarks are not yet available.
2. **No public weights yet** : FLUX 3 Dev (open-weight backbone) has no release date. Until weights are available, independent evaluation and fine-tuning experiments are impossible outside the API.
3. **Video duration ceiling** : 20 seconds is a significant improvement over prior open models, but remains below the multi-minute generation of some proprietary systems.
4. **License uncertainty** : FLUX 3's license terms have not been published. FLUX.2 Klein is Apache 2.0; FLUX.2 dev uses a non-commercial license. The community will need to wait for clarity on commercial use, fine-tuning rights, and redistribution for FLUX 3 Dev.
5. **Training data not disclosed** : Beyond the high-level quantities (tens of millions of hours of video), BFL has not published training data composition, filtering methodology, or sources. This limits reproducibility research.
6. **Action prediction scope** : FLUX-mimic is currently deployed at a single industrial partner (Audi) for specific manipulation tasks. Generalization to broader robotics domains is not yet characterized.

For a broader competitive analysis of flux 3 against Seedance 2.5, Kling v3 Pro, and Runway Gen-4.5 — including capability comparison tables and a production-pipeline decision guide — see Flux 3 Analysis.

- FLUX 3 — Real World Models — Official announcement (July 23, 2026)
- FLUX 3 × mimic: Video-Action Models — Robotics collaboration details
- Self-Flow: Self-Supervised Flow Matching — BFL Research (Chefer, Esser et al.)
- Black Forest Labs on Hugging Face Hub — All FLUX.1 and FLUX.2 models, Spaces, and datasets
- FLUX.2 Klein 4B — Predecessor model (Apache 2.0) on the Hub
- Nunchaku 4-bit Diffusion Inference in Diffusers — Related: efficient inference for diffusion models
