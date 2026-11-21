---
id: collect-261121-veille-ia/modeles-open/a-survey-of-video-models
title: "A survey of video models"
domain: modeles-open
role: reference
task: article
actors: []
dates: []
keywords: ["editing", "video", "generation", "image", "reports", "blogs", "models", "audio"]
source: https://xiefan-guo.github.io/posts/video-models.html
source_anchor: ""
source_lines: [1, 751]
section: "Modeles_Open"
---
This article surveys video models for generation and editing, covering open-weight research systems, proprietary foundation models, and commercial releases. The scope includes text-to-video, image animation, reference-conditioned generation, source-video editing, and audio-video synthesis. Selected avatar and interactive-world models are included where they clarify a different temporal or conditioning interface. Each organization’s chapter links to its official reports, release posts, code, weights, and documentation where available.

Start with the model map or the live leaderboards. This is a companion to A survey of image models, with the same organization-based structure. Sources were reviewed on 27 October 2026; years attached to living documentation identify the version consulted. Historical releases remain included with their documented availability. Architecture and training claims use original papers, reports, or technical blogs; product controls use official documentation. Where only vendor descriptions are public, they establish what the developer reports rather than an independently reproduced result. The catalog is a selection of documented families, not an exhaustive inventory or a fixed ranking.

## 1. A map of the model families

Each row links to a chapter and its materials. A family can contain different generation, editing, audio, or acceleration releases; the task column describes those boundaries rather than assigning every capability to every version.

**Reading the task labels.** T2V creates video from text; I2V uses an image as a starting frame. Reference-to-video uses images, clips, or audio to guide a new shot without necessarily reproducing the first frame. Video-to-video (V2V) covers source-conditioned transformation, while instruction editing requests a particular change to existing footage. Keyframes constrain visual states at selected times. Native audio-video generation synthesizes a soundtrack with the picture; audio-driven animation instead receives its speech or sound as input.

| Selected releases, in chapter order. Access, duration, audio, and editing support are version- and endpoint-specific. |  |  |  | 
|---|---|---|---|
| Company / organization | Model families and releases covered | Generation, editing, and access | Official materials | 
|---|---|---|---|
| OpenAI | Sora 2 / Sora 2 Pro (historical API) | T2V / I2V + synchronized audio; historical editing and extension; product and API retired | Reports · Blogs · Docs | 
|  | Imagen Video; Lumiere; Veo 2 / 3 / 3.1 / Fast / Lite; Gemini Omni Flash / 1.1 Flash | Veo: text / image / reference to video + native audio in 3.x; Omni: generation + conversational video editing and extension | Reports · Docs · Blogs | 
| ByteDance | Seedance 1.0 / 1.5 Pro / 2.0 / 2.5 | Text / image / multimodal references to video; joint audio-video; editing and extension | Reports · Blogs · Docs | 
| Kuaishou | Kling 2.6 / O1 / 3.0 / 3.0 Omni / 4.0 / 4.0 Flash | Text / image / references to video; native audio; keyframes and instruction editing | Docs · Blogs | 
| Tencent | HunyuanVideo / I2V; HunyuanVideo 1.5; HunyuanVideo-Avatar | Open weights: text-to-video and image-to-video; Avatar: supplied-audio-driven character animation | Reports · Code · Blogs · Weights | 
| Alibaba | Wan 2.1 / 2.2 / S2V / Animate; hosted Wan 2.5 / 2.6 / 2.7 / 3.0 / 3.0 Prime | 2.1/2.2: open generation and specialist checkpoints; 3.0 API: audio-video generation, reference conditioning, editing, and extension | Reports · Code · Weights · Docs · Blogs | 
| Alibaba ATH | HappyHorse 1.0 / 1.1; HappyHorse 1.0 Video Edit | Hosted text/image/reference-to-video with audio; separate 1.0 instruction-editing endpoint | Docs | 
| MiniMax | Hailuo 02 / 2.3 / 2.3 Fast; H3 Base FL2VA / Ref2VA; H3 Max by fal | Text / image / first-last frames / multimodal references; audio-video; generation and editing | Blogs · Code · Weights · Docs | 
| ShengShu | Vidu Q1 / Q2 / Q3 Pro / Turbo / Q4 Preview; S2 Avatar / Editing | Text / image / reference generation; audio-video; separate real-time avatar and editing models | Reports · Docs · Blogs | 
| Runway | Gen-4 / Turbo; Gen-4.5; Aleph / Aleph 2.0; Act-Two | Gen-4.5: text / image to video; Aleph: source-video editing; Act-Two: performance transfer | Docs · Blogs | 
| Luma | Ray2 / Flash; Ray3 / HDR / Modify; Ray3.14; Ray3.2 | Generation, keyframe conditioning, source-video modification, extension, and reframing; HDR depends on operation | Docs · Blogs | 
| Black Forest Labs | FLUX 3 Video; Draft / Enhance; multimodal backbone | T2V / I2V / V2V + optional native audio; editing, keyframes, continuation; hosted API | Blogs · Docs | 
| fal | MiniMax H3 Max; FLUX 3 Max | Post-trained T2V / I2V releases; audio-video generation; hosted APIs | Blogs · Docs | 
| xAI | Grok Imagine Video / 1.5 Preview / 1.5 / 1.5 Lite | Text / image / reference generation with audio; grok-imagine-video for editing and extension | Blogs · Docs | 
| PixVerse | PixVerse V6; R1 / R2 interactive world models | Text / image to audio-video; separate real-time audiovisual interaction | Blogs · Docs | 
| Lightricks / LTX | LTX-Video; LTX-2 / 2.3 / 2.5; 2.5 dev / distilled and hosted Fast / Pro | Open audio-video models and hosted text/image/audio-to-video; editing and extension depend on endpoint/version | Reports · Code · Docs · Weights · Blogs | 
| Sand.ai | MAGI-1; MAGI-2 Preview | Open-weight T2V / I2V; MAGI-2 Preview generates synchronized audio and video | Reports · Code · Weights · Blogs · Docs | 
| NVIDIA | Cosmos Predict / Transfer; Cosmos 3 Super / Nano / Edge; I2V / 4Step | Video/world generation and controlled transformation; task-specific open weights | Reports · Weights · Docs · Code | 
| Skywork | SkyReels V1 / V2 / V3 / V4 | Open V2 generation / extension; open V3 reference / avatar models; hosted V4 audio-video and editing | Code · Weights · Reports · Docs | 
| Z.ai / THUDM | CogVideo; CogVideoX-2B / 5B / 5B-I2V; CogVideoX1.5-5B / I2V | Open text-to-video and separate image-to-video checkpoints; continuation and fine-tuning tools | Reports · Code · Blogs · Weights | 
| StepFun | Step-Video-T2V; Step-Video-TI2V | Open text-to-video and text-driven image-to-video; separate task-specific evaluation sets | Reports · Code · Weights | 
| Meituan | LongCat-Video; LongCat-Video-Avatar / Avatar 1.5 | Open text/image-to-video and continuation; Avatar: supplied-audio animation and long-video generation | Reports · Code · Blogs · Weights | 
| Adobe | Firefly Video Model; Generate Video and Creative Cloud integrations | Text / image to video, first/last frames, composition and camera guidance; separate editing and audio tools | Blogs · Docs | 
| Amazon | Nova Reel 1.0 / 1.1 (historical; retired September 2026) | Text / image to video; 1.1 multi-shot automated or manual generation; retired Bedrock endpoints | Blogs · Docs | 
| Pika | Pika 2.5; Pikaframes; Pikadditions / Pikaswaps / Pikaffects | Text / image / ordered keyframes to silent video; separate video editing and soundtrack tools | Blogs · Docs | 
| Midjourney | Video V1; SD / HD modes; extension and loop controls | Image to video with optional motion prompts, end frames, looping, and extension | Blogs · Docs | 
| HeyGen | HeyGen Video 1; Avatar III / IV / V | General T2V / I2V / reference-to-video + audio; separate speech-driven avatars | Reports · Docs · Blogs | 
| Higgsfield | DoP; Genjutsu / Genjutsu Light; Cinematic Studio interfaces | Image-to-video with camera presets; motion-preserving video transformations; hosted workflows | Docs · Code | 
| Utopai Studios | Utopai X; PAI production platform | Derived text-to-video model within PAI; production workflow | Blogs · Docs | 
| Sapiens AI | Agnes Video 2.5 / 2.5 Flash | T2V, first/last frames, multimodal references; hosted API | Docs | 
| Creatify | Boreal; Boreal-H3; Aurora 2.0 | Derived T2V / I2V and reference generation; separate audio-driven avatar model | Docs · Blogs | 
| Video Rebirth | BACH 1.0 Preview / 1.0 / 1.0 Pro | Hosted video generation and multimodal reference conditioning; enterprise customization | Blogs · Docs | 
| Vimoo | Vimoo 1.0 / Lite / Ecom; Enhancer | T2V / I2V; lighter unified variant; dedicated product-video service | Reports · Docs | 
| Meta | Movie Gen Video / Audio; personalization and editing research | T2V, identity-conditioned generation, instruction editing; separate audio generation | Reports | 
| Stability AI | SVD / SVD-XT | Open-weight image-to-video research checkpoints; no text control in the released SVD-XT interface | Reports · Weights · Code | 
| Genmo | Mochi 1 Preview; AsymmDiT / AsymmVAE | Open-weight T2V; inference and LoRA training | Blogs · Code · Weights | 
| HPC-AI Tech | Open-Sora 1.x / 2.0 | Open training pipeline, video generation, and released checkpoints | Code · Weights · Reports | 

## 2. Leaderboards and evaluation entry points

A preference score answers a question about a particular prompt distribution, generation configuration, and judging setup. Video adds several dimensions that a single image does not expose: identity through time, action completion, motion and camera behavior, temporal editing consistency, and the alignment of sound with visible events.

| Live comparisons and reproducible evaluation code serve complementary purposes. |  |  | 
|---|---|---|
| Evaluation | Task / entry point | How to read it | 
|---|---|---|
| Artificial Analysis | Text-to-video · Image-to-video · Video editing | Separate audio-enabled and silent comparisons; check benchmark version, exact model label, resolution, and preview or derived status. | 
| Arena | Text-to-video · Image-to-video · Video editing | Human preference with uncertainty and vote counts; generation and source-video editing have separate lists. | 
| VBench | Code, prompts, and released evaluations | Dimension-specific evaluation of video quality and semantic alignment; inspect sampling settings alongside the aggregate. | 
| VBench 2.0 | Intrinsic faithfulness suite | Five broad categories and 18 fine-grained dimensions with a prescribed prompt suite; use matching evaluation protocols for model comparisons. | 

Artificial Analysis’s methodology keeps audio-enabled and silent outputs in separate Elo pools, and separates editing from generation. At review time, the text-to-video and image-to-video boards also used different benchmark versions. Their raw scores should not be pooled into one cross-task ranking. Arena’s visible confidence intervals and vote counts likewise matter when interpreting small score differences.

The original VBench paper separates perceptual and semantic quality dimensions; the VBench 2.0 paper extends evaluation toward human fidelity, controllability, creativity, physics, and commonsense. Its implementation specifies standard prompts and supported custom-video dimensions. A result computed on a private prompt set, altered frame sampling, or different evaluator is a separate experiment. For an editor, assess both the requested change and preservation of unaffected content across frames; for an audio-video model, assess speech, synchronization, and visual motion together.

A dated source index accompanies this article. It records the materials linked below, not a frozen copy of leaderboard scores. Use the live boards for current results.

### Materials and further reading

## Leaderboards and evaluation (11 resources)

- Artificial Analysis: Text-to-video Leaderboard · 2026
- Artificial Analysis: Image-to-video Leaderboard · 2026
- Artificial Analysis: Video editing Leaderboard · 2026
- Artificial Analysis: Video benchmarking methodology Methodology · 2026
- Arena: Text-to-video Leaderboard · 2026
- Arena: Image-to-video Leaderboard · 2026
- Arena: Video editing Leaderboard · 2026
- VBench: Evaluation code, prompts, and results Benchmark · code · 2024
- VBench 2.0: Intrinsic faithfulness evaluation Benchmark · code · 2025
- VBench: Comprehensive benchmark suite for video generative models Research paper · 2023
- VBench-2.0: Advancing video generation benchmark suite for intrinsic faithfulness Research paper · 2025

## 3. OpenAI: Sora

The original Sora technical report (February 2024) describes a diffusion transformer operating on spacetime patches of compressed video and image latents. This is the documented architecture of the original research model, not a full specification of Sora 2. The Sora 2 release and system card describe synchronized dialogue, sound effects, and improved control. Its historical API offered `sora-2` and `sora-2-pro` as hosted variants; the model reference records text/image inputs and video/audio outputs, without downloadable weights.

The historical video generation guide documents asynchronous rendering, image conditioning, reusable characters, extensions, and targeted video editing. Its later `/v1/videos/edits` interface superseded remixing for new integrations and accepted either an existing generation ID or an uploaded video; uploaded-video editing was restricted to eligible customers. The prompting guide provides examples of shots, timed actions, dialogue, and sound.

Availability has changed: the release page states that the Sora product was no longer available as of 26 April 2026. Separately, OpenAI’s deprecation schedule and current model page state that Sora 2, Sora 2 Pro, their listed snapshots, and the Videos API shut down on 24 September 2026, with no one-to-one replacement API. The remaining guides document the earlier interface.

### Materials and further reading

## OpenAI: Sora (7 resources)

- Sora 2 model reference and shutdown notice Model documentation · 2026
- Video generation with Sora Historical API guide · 2026
- Sora 2 prompting guide Prompting guide · 2026
- API deprecations: Sora 2 and Videos API Lifecycle · 2026
- Video generation models as world simulators Technical report · 2024
- Sora 2 release and product-discontinuation notice Release blog · 2025
- Sora 2 system card System card · 2025

## 4. Google: Veo and Gemini Omni

Google’s research history includes Imagen Video, which uses cascaded video diffusion and spatial–temporal super-resolution, and Lumiere, whose Space-Time U-Net processes the full temporal extent at multiple scales. These research systems should be distinguished from the hosted Veo and Gemini Omni releases.

The Veo 3 report describes joint latent diffusion over audio and video, with separate autoencoders and a transformer denoiser. Veo 3.1 extends the family’s cinematic generation and audio controls. Its API guide documents reference-image conditioning, first-and-last-frame generation, and extension. Availability is endpoint-specific: the same guide records the shutdown of Veo 3.1, Fast, and Lite preview endpoints in the Gemini API on 22 October 2026, and points to Gemini Omni or GA models on the enterprise platform.

Gemini Omni Flash is separately documented as a multimodal transformer for video generation and conversational editing. Its model card covers text, image, audio, and video inputs and video-with-audio output. The 1.1 release adds scene extension, first/last-frame interpolation, 360p drafts, and 4K upscaling; extension builds a sequence in increments, up to 40 seconds. The current Gemini API model reference lists `gemini-omni-1.1-flash`, 3–10-second outputs, and text/image/video inputs. Model-card modalities, API inputs, and cumulative extension length describe different boundaries; 4K delivery should also be distinguished from native generation resolution.

### Materials and further reading

## Google: Veo and Gemini Omni (8 resources)

- Imagen Video: High definition video generation with diffusion models Paper · 2022
- Lumiere: A space-time diffusion model for video generation Paper · 2024
- Veo: A text-to-video generation system (Veo 3) Technical report · 2025
- Veo 3.1: Model overview and demonstrations Model overview · 2026
- Veo 3.1 API guide and preview shutdown notice Docs / lifecycle · 2026
- Gemini Omni Flash and 1.1 Flash model card Model card · 2026
- Build with Gemini Omni 1.1 Flash Blog · 2026
- Gemini Omni Flash: API model reference API docs · 2026

## 5. ByteDance: Seedance

The Seedance 1.0 report combines joint text-to-video and image-to-video training with native multi-shot generation, video-specific preference optimization, and inference distillation. Seedance 1.5 Pro introduces joint audio-video generation through a dual-branch diffusion transformer and cross-modal interaction. The Seedance 2.0 report extends the family to a unified architecture conditioned on text, images, audio, and video.

Seedance 2.5, announced on 31 July 2026, expands a single generation to 30 seconds and adds timestamp-level audio and video editing. Its announcement allows up to 30 images, 10 video clips, and 10 audio clips as references. The model page additionally describes two rounds of extension, camera-reference control, clay-render guidance, and green-screen editing.

These are hosted model releases with technical reports; the cited materials do not establish downloadable Seedance weights. The July announcement describes the initial Jimeng and Doubao rollout and forthcoming API access. For an explicit later deployment record, Runway’s October integration guide documents its official Seedance 2.5 API, including generation, editing, and extension. Provider-specific limits should accompany any comparison of those interfaces.

### Materials and further reading

## ByteDance: Seedance (6 resources)

- Seedance 1.0: Exploring the boundaries of video generation models Report · 2025
- Seedance 1.5 Pro: A native audio-visual joint generation foundation model Report · 2025
- Seedance 2.0: Advancing video generation for world complexity Report · 2026
- Introducing Seedance 2.5 Release blog · 2026
- Seedance 2.5 model overview Model page · 2026
- Seedance 2.5 API on Runway Dev Provider API guide · 2026

## 6. Kuaishou: Kling

Kling’s VIDEO 3.0 guide traces its generation line from VIDEO 2.6, while 3.0 Omni follows VIDEO O1. Both support native audio and multi-shot generation up to 15 seconds, as well as reusable subject elements with bound voices. Omni provides an all-in-one reference interface for images, videos, and elements; its documented native-audio availability depends on the selected input mode. These release names identify distinct capability sets within the same product.

The September 2026 announcement introduces Kling 4.0 and its faster Flash mode. The 26 October user guide documents standard Kling 4.0 with 3–30-second output, stereo audio, up to 15 combined reference assets, targeted video editing, and up to 10 keyframes. Omni Reference supplies appearance, motion, camera, and voice guidance; Keyframes specifies visual states along the timeline. The guide says these two entry points cannot be combined in one task. Flash has a separate 20-second, 720p limit and does not support start/end-frame or multiple-keyframe control.

Release boundaries still matter: the standard 4.0 guide lists output resolutions through 4K, while its introductory section marks 10-bit HDR and repeated extension to two minutes as coming soon. Those roadmap features should not be treated as generally available endpoint guarantees. The official materials here primarily document hosted capabilities and workflows, rather than reproducible training recipes or released model checkpoints.

### Materials and further reading

## Kuaishou: Kling (4 resources)

- Kling VIDEO 3.0 user guide Docs · 2026
- Kling VIDEO 3.0 Omni user guide Docs · 2026
- Introducing Kling 4.0 Release blog · 2026
- Kling 4.0 user guide Docs · 2026

## 7. Tencent: HunyuanVideo

The original HunyuanVideo report describes a large video-generation framework built around a spatiotemporal latent representation and a diffusion transformer. Its official repository preserves the original release and the subsequent image-to-video branch. These releases established a reproducible foundation for research on motion, conditioning, and inference efficiency, with model weights published separately from the paper.

HunyuanVideo 1.5 reduces the model to 8.3 billion parameters and combines selective and sliding tile attention, glyph-aware bilingual text encoding, progressive training, and a video super-resolution network. The release repository lists distinct 480P and 720P T2V/I2V checkpoints, classifier-free-guidance-distilled variants, and a 480P I2V step-distilled checkpoint. Training and LoRA-tuning code accompany inference code. A reported memory requirement or speed belongs to its particular offloading, precision, resolution, and sampling configuration; the existence of a small backbone alone does not determine end-to-end deployment cost.

HunyuanVideo-Avatar is a separate audio-driven human-animation model. Its emotion and face-aware audio modules target expressive motion and multi-character conditioning. This is an input-audio animation task: it should be distinguished from jointly synthesizing a new soundtrack and video from text. The 1.5 model card identifies the Tencent Hunyuan community license, so access to weights should be recorded separately from the terms of a hosted video product.

### Materials and further reading

## Tencent: HunyuanVideo (7 resources)

- HunyuanVideo: A systematic framework for large video generative models Report · 2024
- HunyuanVideo: Official code and release history Code · 2026
- HunyuanVideo 1.5 technical report Report · 2025
- HunyuanVideo 1.5: Inference, training, and checkpoint guide Code · release notes · 2026
- HunyuanVideo 1.5: Model card and weights Weights · model card · 2026
- HunyuanVideo-Avatar: Audio-driven animation report Report · 2025
- HunyuanVideo-Avatar: Official implementation Code · weights · 2026

## 8. Alibaba: Wan

The Wan report documents a diffusion-transformer family with a video VAE, large-scale data curation, and progressive training. Wan 2.1 and Wan 2.2 provide downloadable code and weights. In 2.2, T2V-A14B and I2V-A14B use experts specialized for different denoising stages; TI2V-5B instead provides a compact combined text/image-to-video model. Separate S2V and Animate releases address supplied-audio-driven generation and character animation/replacement. The TI2V-5B model card records its Apache 2.0 license and 720P generation interface.

The hosted line has a different release boundary. Alibaba Cloud's video-generation documentation distinguishes Wan 2.5 audio-video generation from 2.6 multi-shot and reference-to-video services, and 2.7 generation, continuation, and reference interfaces. The 2.7 API reference explicitly identifies `wan2.7-t2v-2026-06-12`, with 2–15-second output and optional input audio. A numbered hosted endpoint is not evidence that a corresponding open checkpoint exists.

Wan 3.0's August 2026 release introduced an API for up to 30 seconds of native audio-video generation at up to 1080P. The current guide documents text, first/last frames, image/video/audio references, documents, and web pages, alongside video editing and extension. These are distinct request modes: first/last-frame inputs cannot simply be mixed with every reference type. `wan3.0-video-prime` provides a faster hosted variant. The 3.0 product/API materials should therefore be read alongside, rather than substituted for, the earlier open-model reports and repositories.

### Materials and further reading

## Alibaba: Wan (9 resources)

- Wan: Open and advanced large-scale video generative models Report · 2025
- Wan 2.1: Official implementation and weights Code · weights · 2026
- Wan 2.2: T2V, I2V, TI2V, S2V, and Animate Code · weights · 2026
- Wan 2.2 TI2V-5B: Model card Weights · model card · 2026
- Wan hosted video-generation model overview Docs · 2026
- Wan 2.7 text-to-video API reference API docs · 2026
- Wan 3.0 officially launched Release announcement · 2026
- Wan 3.0 generation, editing, and extension guide API docs · 2026
- Wan 3.0 prompt guide and task boundaries Docs · 2026

## 9. Alibaba ATH: HappyHorse

HappyHorse is a separate Alibaba video-model family from Wan. Alibaba's 2026 corporate release records HappyHorse 1.0's April debut and the 1.1 update in June, alongside the consolidation of its AI businesses under Alibaba Token Hub. Model Studio's video-model catalog lists the two families independently; their version numbers should not be interpreted as successive checkpoints of a single architecture.

The current hosted interfaces include `happyhorse-1.1-t2v`, `happyhorse-1.1-i2v`, and `happyhorse-1.1-r2v`. They respectively generate from text, animate a supplied first frame, and use reference images. The official catalog specifies audio-bearing output at 24 fps, with 480P, 720P, and 1080P tiers and 3–15-second durations. The text-to-video reference documents an asynchronous job interface and separate choices for resolution, aspect ratio, duration, and seed.

Editing has its own release identity: `happyhorse-1.0-video-edit` remains the catalog's instruction-editing endpoint. A generation model's newer version therefore does not establish a corresponding 1.1 editor. These materials document hosted models and service capabilities; they do not provide the architecture, training recipe, or downloadable weights needed to reproduce the model. In particular, the 1.1 T2V model page marks service fine-tuning as unsupported.

### Materials and further reading

## Alibaba ATH: HappyHorse (5 resources)

- Alibaba's 2026 model releases: HappyHorse and ATH Announcement · 2026
- Model Studio: Video-generation and editing catalog Model docs · 2026
- HappyHorse text-to-video API reference API docs · 2026
- HappyHorse 1.1 T2V: Model and service capabilities Model docs · 2026
- HappyHorse 1.1 I2V: Model and service capabilities Model docs · 2026

## 10. MiniMax: Hailuo and H3

Hailuo 2.3 follows Hailuo 02 with improvements to motion, stylization, and facial performance; its Fast variant provides a separate accelerated option. MiniMax H3, announced in July 2026, shifts to general multimodal conditioning and joint video generation with stereo audio.

The released H3 model card distinguishes FL2VA, which accepts text and optional first/last frames, from Ref2VA, which takes image, video, and audio references. Both belong to an open-weight base-model release. The repository explains that H3-Base produces 768p output; hosted H3-Context-IR interprets and rewrites multimodal instructions, and H3-Regenerate-2K regenerates a base result with its original context at 2K. Reproducing the complete hosted workflow therefore involves more than downloading the base weights.

The weights use the MiniMax H3 Community License, whose deployment terms should be read with the model card. H3 Max and H3 Max Turbo are fal’s post-trained derivatives, developed together with fal’s serving stack. They are separate releases from the official H3 base checkpoints; performance measurements should identify the derivative, output settings, and endpoint.

### Materials and further reading

## MiniMax: Hailuo and H3 (6 resources)

- MiniMax Hailuo 2.3 release Release blog · 2025
- Introducing MiniMax H3 Technical blog · 2026
- MiniMax H3 repository and system overview Code · technical overview · 2026
- MiniMax H3 model card and weights Weights · model card · 2026
- MiniMax API model directory API docs · 2026
- Introducing H3 Max by fal Derived-model technical blog · 2026

## 11. ShengShu: Vidu

Vidu’s current API separates the Q-series clip generators from its S-series streaming models. Q3 Pro and Turbo expose text-to-video alongside image, start/end-frame, and reference interfaces. Q3 Pro supports 1–16-second clips through 1080p, with an explicit option for synchronized dialogue and sound effects. A family name therefore needs the selected variant and task endpoint to specify what is being evaluated.

The 7 October 2026 release entry introduces Q4 Preview, including reference voices and additional reference images, also described in ShengShu’s launch announcement. Its image-to-video API uses `viduq4-preview`, supports 3–16 seconds, and lists output resolutions through 4K. The separate reference-to-video interface supplies the appropriate route when reference assets should guide a new scene rather than fix its opening frame.

The Vidu S2 paper describes S2 Avatar and S2 Editing, also recorded in the platform’s September 2026 release log: the former targets live voice interaction and controlled character motion; the latter edits an incoming video stream using appearance and scene references. These interactive interfaces should be distinguished from offline Q-series clip generation when interpreting latency, duration, or editing claims.

### Materials and further reading

## ShengShu: Vidu (7 resources)

- Vidu Q3 text-to-video API API docs · 2026
- Vidu Q4 Preview image-to-video API API docs · 2026
- Vidu Q4 Preview reference-to-video API API docs · 2026
- Vidu API release history Release notes · 2026
- Vidu S2: Real-time interactive, editable, and spatial video generation Technical paper · 2026
- Shengshu Technology launches Vidu Q4 Preview Issuer-authored release · 2026
- Vidu S2 official research/demo page Official project page · 2026

## 12. Runway: Gen-4, Gen-4.5, Aleph, and Act-Two

Runway’s video models expose different conditioning interfaces. Gen-4 and Gen-4 Turbo require a starting image and generate five- or ten-second clips. Gen-4.5 expands generation quality and prompt following; its current usage guide documents both text-to-video and image-to-video, with durations from two to ten seconds. Reference-based scene construction and source-video editing should therefore be attributed to the corresponding model or workflow.

Aleph 2.0 powers Edit Studio. It transforms existing footage through keyframe-guided edits, including replacements, relighting, background changes, and effects. The API release notes document two-to-thirty-second source videos and up to five timed keyframe images under `aleph2`. This is an editing interface with an input clip, separate from generating a new shot with Gen-4.5.

Act-Two instead takes a driving performance video and a character image or video. It transfers facial performance and speech; gesture control is available with character-image inputs. Runway’s API model catalog labels each provider explicitly, so third-party models available inside Runway remain releases of their original organizations. Likewise, the platform’s audio and delivery-format tools do not establish native audio generation for every Runway video model.

### Materials and further reading

## Runway: Gen-4, Gen-4.5, Aleph, and Act-Two (7 resources)

- Gen-4 and Gen-4 Turbo: Model inputs and usage Docs · 2026
- Introducing Runway Gen-4.5 Research blog · 2025
- Creating with Gen-4.5 Docs · 2026
- Creating with Edit Studio: Aleph 2.0 Docs · 2026
- Runway API changelog: Aleph 2.0 and output formats Release notes · 2026
- Performance capture with Act-Two Docs · 2026
- Runway API: Model catalog and provider attribution API docs · 2026

## 13. Luma: Ray

Luma’s Ray3 family overview combines text-to-video, image-conditioned creation, video modification, reference controls, and HDR workflows. Ray3.2, announced in June 2026, emphasizes multi-keyframe direction and Modify Video V2. The launch describes up to sixteen keyframes and twenty-second, 1080p creative workflows. These product-level limits should not be substituted for the limits of a particular API operation.

The current Luma Agents model reference uses `ray-3.2` with separate `video`, `video_edit`, and `video_reframe` request types. It documents text/image generation, multi-keyframe conditioning, extension from a prior generation, and source-video transformation. Its generation table lists five- and ten-second outputs; reframing has its own constraints and is SDR-only. The migration guide maps older Ray2, Flash, Relaxed, Ray3 Reference, and Refiner interfaces onto these primitives, with retirement dates supplied to individual customers.

Luma’s training-content disclosure says Ray3 was initialized from a third party’s publicly released open-weight model and then further trained by Luma; Ray3.14 and Ray3.2 derive from Ray3. The disclosure does not identify that base model. This documented dependency does not make the hosted Ray checkpoints an open-weight release, and the vendor’s reasoning terminology alone does not disclose its complete inference architecture.

### Materials and further reading

## Luma: Ray (5 resources)

- Ray3: Model family and capabilities Living model overview · 2026
- Introducing Ray3.2 and its API Blog · 2026
- Luma Agents: Ray3.2 model reference API docs · 2026
- Migrate legacy Ray models Migration guide · 2026
- Public summary of training content: Ray models Training disclosure · 2026

## 14. Black Forest Labs: FLUX 3 Video

FLUX 3, announced in July 2026, extends Black Forest Labs’ image-model lineage into a multimodal flow model trained across images, video, and audio. The release presents these modalities as a shared learning problem: appearance, motion, and sound constrain different aspects of the same event. This is a different scope from applying an image model independently to successive frames.

The current FLUX 3 Video model page documents text-, image-, and video-conditioned workflows, clips up to 20 seconds, and optional native dialogue, sound effects, and ambience. The page also documents instruction-based video editing, keyframes, and continuation. Its Draft mode produces a cheaper HD preview; enhancement is a subsequent operation. The same page separates the hosted video service from the FLUX 3 Image releases and says open-weight access to the multimodal Dev backbone is still forthcoming. Open image weights therefore do not establish an open video checkpoint.

FLUX 3 Max is the collaboration with fal Research described in the following chapter. Keep its post-training, inference settings, and benchmark label separate from the base FLUX 3 Video service.

### Materials and further reading

## Black Forest Labs: FLUX 3 Video (3 resources)

- FLUX 3: Real world models Technical blog · 2026
- FLUX 3 Video: Inputs, output formats, and rollout boundaries Model documentation · 2026
- Introducing FLUX 3 Max by fal Release blog · 2026

## 15. fal: H3 Max and FLUX 3 Max

fal publishes derived models as well as hosting other developers’ checkpoints. Its H3 Max release explicitly identifies the base as the open-weight MiniMax H3 model and describes additional post-training data aimed at prompt adherence and visual quality. H3 Max should consequently have its own release entry rather than inheriting every property of the original H3 checkpoint.

The engineering account connects post-training and reinforcement-learning infrastructure with sampling, kernels, and serving optimizations. The account distinguishes inference execution from end-to-end waiting time, which also includes queueing and cold starts. A latency claim needs that measurement boundary as well as the host, GPU, resolution, and sampling schedule.

FLUX 3 Max follows a similar pattern in collaboration with Black Forest Labs: BFL supplies the foundation model, while fal contributes post-training and inference optimization. The model page documents text- and image-to-video access. Both Max families illustrate why a leaderboard needs the exact provider and variant: shared ancestry does not imply identical generated output, license, or execution cost.

### Materials and further reading

## fal: H3 Max and FLUX 3 Max (4 resources)

- Introducing H3 Max by fal Release blog · 2026
- H3 Max: Built with fal inference and training Engineering · 2026
- Introducing FLUX 3 Max by fal Release blog · 2026
- FLUX 3 Max model and access Model documentation · 2026

## 16. xAI: Grok Imagine Video

The June 2026 preview introduced Grok Imagine Video 1.5 as an image-to-video model. Later release notes add text-to-video and reference-to-video to the non-preview model. They explicitly state that its text-to-video route first produces an image and then animates it, an example of why an API task label alone does not identify the underlying generation pipeline.

The current capability matrix distinguishes three model IDs. `grok-imagine-video-1.5` supports native 1080p for text/image generation, reference images, preset voices, and pinned frames. `grok-imagine-video-1.5-lite` supports text/image generation with synchronized speech, but its 1080p output is upscaled from 720p and it lacks the same reference controls. Reference-conditioned 1.5 generation has its own 720p limit, separate from native 1080p text/image generation.

Video editing remains attached to `grok-imagine-video`, rather than either 1.5 variant. The overview likewise assigns extension to that model. A comparison labeled only “Grok Imagine” can consequently mix generation, editing, preview, and upscaled configurations that offer different capabilities.

### Materials and further reading

## xAI: Grok Imagine Video (4 resources)

- Grok Imagine 1.5 Preview announcement Release blog · 2026
- xAI API release notes Release notes · 2026
- Grok Imagine video capability matrix API docs · 2026
- Grok Imagine video editing API docs · 2026

## 17. PixVerse: V6 and R2

PixVerse V6, released in March 2026, targets clip generation with native audio, multi-shot structure, camera motion, and character performance. The official model page documents both text-to-video and image-to-video, with clips up to 15 seconds at 1080p. Its audio output includes dialogue, sound effects, and ambience; this is distinct from adding a soundtrack after rendering silent frames.

The release post also documents access through PixVerse’s CLI. That integration exposes generation to applications and agents, while the surrounding Canvas and marketing tools organize a broader production workflow. A product’s access to additional companies’ models should be recorded separately from the capabilities of the V6 model itself.

PixVerse R2 belongs to a different line: an interactive audiovisual world model whose next segment responds to history and new controls. Its public experience centers on movement and prompt-based interaction, with references and audio included in the broader model interface. Its R2 technical blog discusses causal state updates and memory. PixVerse describes the public experience as a demonstration of continuously generated worlds, with managed access, rather than a completed general game engine. Offline V6 clip benchmarks and R2 interactive demonstrations therefore measure different tasks.

### Materials and further reading

## PixVerse: V6 and R2 (4 resources)

- PixVerse V6 release Release blog · 2026
- PixVerse V6 model capabilities Model page · 2026
- PixVerse R2 interactive world model Technical overview · 2026
- PixVerse R2: Scaling real-time omni world models Technical blog · 2026

## 18. Lightricks / LTX: LTX-Video and LTX-2

The LTX-Video paper studies efficient latent video diffusion, coupling a highly compressed video representation with a decoder that also performs final denoising. LTX-2 adds joint sound generation through coupled video and audio transformer streams. Its report describes a 14B video stream and a 5B audio stream with bidirectional cross-attention, so native synchronized audio is part of the model design rather than solely a downstream soundtrack tool.

The LTX-2.5 documentation describes native multi-shot generation, a diffusion video decoder, and duration prediction. Its released weights include separate 22B development and distilled transformers, text-encoder and VAE components, and optional adapters. The package distinguishes full/trainable weights from fixed-schedule distilled inference and from format-specific quantized files. The official implementation also supplies inference and LoRA training. These open artifacts are governed by the LTX community license.

The commercial 2.5 API exposes Fast and Pro variants for text-, image-, and audio-to-video, with output up to 4K. Duration and frame-rate combinations differ by variant and resolution; Retake and Extend are explicitly absent from the 2.5 support table. Their appearance elsewhere in the service should not be assigned to every model. The API changelog records removal of the original LTX-2 hosted IDs in August 2026 and retirement of V1 generation endpoints after 26 October 2026 at 23:59 UTC. Downloadable historical weights and retired API names thus have different lifecycles.

### Materials and further reading

## Lightricks / LTX: LTX-Video and LTX-2 (7 resources)

- LTX-Video: Realtime video latent diffusion Paper · 2024
- LTX-2: Efficient joint audio-visual foundation model Report · 2026
- LTX-2: Official inference and LoRA training code Code · 2026
- LTX-2.5: Open-source model overview Technical docs · 2026
- LTX-2.5: Development and distilled weights Model card · weights · 2026
- LTX-2.5 Fast and Pro API support matrix API docs · 2026
- LTX API changelog and retirement notices Release notes · lifecycle · 2026

## 19. Sand.ai: MAGI

The MAGI-1 report describes an autoregressive video model that predicts successive chunks, each containing multiple frames. Diffusion within a chunk and autoregression across chunks serve different roles: the former models visual content, while the latter makes continuation explicit. The official repository provides implementation and checkpoint links for this research line.

MAGI-2 Preview is a later unified audio-video model with 114B total parameters and approximately 6B activated per token. Sand.ai explains its multi-head mixture-of-experts design, head-parallel communication, and kernels for many small expert operations. Its data discussion emphasizes retaining complicated motion, interactions, shot changes, and sound relationships instead of filtering training material down to only easy scenes.

MAGI-2 Preview also has an official inference repository and Apache-2.0 model weights. The released pipeline generates ten-second clips from text or a prompt plus an image, synthesizes audio alongside video, and runs a low-resolution generation stage followed by a 1080P refiner. The repository specifies eight NVIDIA Hopper GPUs and distinguishes the released base model from a step-distilled version still marked as forthcoming. “Preview” therefore describes the research release stage; it does not mean that weights are unavailable. The separate MAGI-1 and MAGI-2 implementations should be used to establish each version’s generation procedure.

### Materials and further reading

## Sand.ai: MAGI (6 resources)

- MAGI-1: Autoregressive video generation at scale Report · 2025
- MAGI-1 official implementation and checkpoints Code · weights · 2025
- MAGI-2 Preview Technical blog · 2026
- MAGI-2 Preview: Official inference implementation Code · checkpoint guide · 2026
- MAGI-2 Preview: Model card and base weights Weights · model card · 2026
- Sand.ai model overview Model page · 2026

## 20. NVIDIA: Cosmos

Cosmos connects video generation with physical-AI development. The Cosmos 3 report describes a mixture-of-transformers framework spanning text, images, video, sound, and action. This places visual prediction alongside reasoning and action modeling, while individual released artifacts still have specific input and output contracts.

The model reference lists Super, Nano, and Edge scales alongside specialized `Cosmos3-Super-Image2Video` and four-step variants. Those task-specific checkpoints are the relevant entries for image-to-video comparisons; a family-level claim about action output does not establish that every I2V checkpoint exposes an action interface. The model matrix is the appropriate companion for deployment requirements.

The documentation also retains the earlier Predict and Transfer families. Prediction generates future visual states, whereas controlled transformation uses structured conditions to change the appearance of a scenario. For physical-AI applications, the practical question extends beyond whether a clip looks convincing: the generated state must preserve the task-relevant geometry, controls, and action consequences. Human video preference alone does not test that requirement.

### Materials and further reading

## NVIDIA: Cosmos (4 resources)

- Cosmos 3: Omnimodal world models for physical AI Report · 2026
- Cosmos 3 model reference and checkpoint links Docs · weights · 2026
- Cosmos 3 model matrix Documentation · 2026
- Cosmos official repository Code · 2026

## 21. Skywork: SkyReels

SkyReels spans several release boundaries. The V2 paper and repository describe diffusion-forcing models for autoregressive continuation alongside dedicated image-to-video variants. Its long-video interface repeatedly generates new content from a rolling context; the length of a completed sequence should be distinguished from the temporal window synthesized in one step.

The V3 technical report accompanies a release with inference code and separate downloadable models for reference-to-video, video extension, and talking avatars. Its model table lists 14B reference and extension models and a 19B avatar model. Reference-to-video accepts one to four images, while the avatar task has an audio-conditioned interface. The official weight collection makes these task-specific checkpoints explicit.

The V4 technical report describes a dual-stream multimodal diffusion transformer for aligned video and audio, with text, images, clips, masks, and audio references. It unifies generation, inpainting, and editing through conditioning channels, and combines low-resolution full sequences with high-resolution keyframes before super-resolution and interpolation. The report documents output up to 1080p, 32 fps, and 15 seconds. V4’s API offering is a separate access path; the V2/V3 weight releases do not establish open weights for V4.

### Materials and further reading

## Skywork: SkyReels (7 resources)

- SkyReels V2 repository Code · weights · 2025
- SkyReels V3 repository Code · 2026
- SkyReels V3 official model collection Weights · 2026
- SkyReels V4: Multi-modal video-audio generation, inpainting and editing Report · 2026
- SkyReels API platform API overview · 2026
- SkyReels-V2: Infinite-length film generative model Technical paper · 2025
- SkyReels-V3 technique report Technical report · 2026

## 22. Z.ai / THUDM: CogVideo and CogVideoX

CogVideoX combines a 3D causal VAE with a diffusion transformer that uses modality-specific adaptive normalization to align text and video. Its report also studies progressive training, multi-resolution frame packing, and detailed video captioning. This provides a technical account of how temporal compression, conditioning, and training data interact, beyond comparisons of a few generated clips.

The official repository preserves the earlier CogVideo research release and documents the 2B, 5B, 5B-I2V, and CogVideoX1.5 lines. The 1.5 family includes separate T2V and I2V checkpoints and supports videos up to ten seconds. Its I2V model permits multiple aspect ratios within the dimensions specified in the model card; the release announcement's broad resolution language should not be interpreted as unrestricted dimensions. SAT and Diffusers paths, prompt rewriting, LoRA tuning, and inversion tools are documented separately.

Availability also varies within this family. The repository specifies Apache 2.0 for its code and the 2B model, while 5B-family weights use a distinct CogVideoX license. The 1.5 T2V card likewise links that model license. QingYing's consumer or commercial service is a related deployment surface, but its product updates do not establish that the public 1.5 weights have changed. Reproducible comparisons should name the checkpoint, inference library, frame count, and whether prompt rewriting was enabled.

### Materials and further reading

## Z.ai / THUDM: CogVideo and CogVideoX (6 resources)

- CogVideoX: Text-to-video diffusion models with an expert transformer Paper · 2024
- CogVideo and CogVideoX: Official implementation Code · release notes · 2026
- CogVideoX1.5-5B: Text-to-video weights Model card · weights · 2026
- CogVideoX1.5-5B-I2V: Image-to-video weights Model card · weights · 2026
- CogVideo: Official model collection Weights · captioning model · 2026
- CogVideo: Large-scale pretraining for text-to-video generation via transformers Paper · 2022

## 23. StepFun: Step-Video

Step-Video-T2V is a 30B-parameter text-to-video model whose report describes generation up to 204 frames. Its video VAE compresses space by 16 × 16 and time by a factor of eight, while two bilingual text encoders provide Chinese and English conditioning. The report makes the video representation, training system, and evaluation protocol part of the release, alongside the inference implementation and weights.

Step-Video-TI2V is a separate 30B model initialized from T2V and designed for text-driven image animation, with up to 102 generated frames. Its implementation encodes the conditioning image with the video VAE and incorporates it along the latent channel dimension. A motion-score condition lets users vary the strength of movement. This is first-frame conditioning, with its own generation limit and controls, rather than evidence of general video instruction editing.

Both releases provide task-specific evaluation materials, Step-Video-T2V-Eval and Step-Video-TI2V-Eval. Those comparisons should be read with their prompt or source-image distributions and judged criteria; results across the two tasks are not directly interchangeable. The T2V and TI2V model cards publish downloadable MIT-licensed weights. Their substantial backbone size and documented distributed-inference examples make them useful research baselines, while practical hardware requirements depend on the complete pipeline.

### Materials and further reading

## StepFun: Step-Video (6 resources)

- Step-Video-T2V technical report Report · 2025
- Step-Video-T2V: Implementation and evaluation Code · benchmark · 2026
- Step-Video-T2V: Model card and weights Weights · 2026
- Step-Video-TI2V technical report Report · 2025
- Step-Video-TI2V: Implementation and evaluation Code · benchmark · 2026
- Step-Video-TI2V: Model card and weights Weights · 2026

## 24. Meituan: LongCat-Video

The LongCat-Video report describes a 13.6B diffusion transformer trained for text-to-video, image-to-video, and video continuation within one model. Continuation is part of pretraining, which makes the family particularly relevant to research on extending clips over minutes. A coarse-to-fine temporal and spatial generation pipeline and block-sparse attention address inference cost, while multi-reward preference training targets output quality. The report's long-video examples should be understood as sequential continuation, with continuity assessed across segment boundaries.

The official code exposes separate demos for text generation, image animation, continuation, and long-video workflows. The base model card supplies MIT-licensed weights. These artifacts allow the continuation schedule and generation settings to be inspected, rather than treating a displayed final duration as the duration of a single independent generation.

LongCat-Video-Avatar adds audio-driven character animation. Its 1.5 report, published in May 2026, describes a Whisper Large audio encoder, expanded data curation and preference training, and step distillation to eight function evaluations. The Avatar 1.5 release supports single- and multiple-audio-stream workflows. Because speech is supplied as conditioning, comparisons with native text-to-audio-video systems must distinguish lip synchronization and identity preservation from the separate problem of generating the soundtrack itself.

### Materials and further reading

## Meituan: LongCat-Video (5 resources)

- LongCat-Video technical report Report · 2025
- LongCat-Video: Generation, continuation, and avatar code Code · release notes · 2026
- LongCat-Video: Base weights and model card Weights · 2026
- LongCat-Video-Avatar 1.5 technical report Report · 2026
- LongCat-Video-Avatar 1.5: Weights and usage Model card · weights · 2026

## 25. Adobe: Firefly Video

Adobe introduced the Firefly Video Model as part of its creative workflow, with text- and image-conditioned generation and camera controls. The April 2025 general-availability release documents five-second clips, first/last-frame guidance, multiple aspect ratios, and resolutions up to 1080p. These are capabilities of Adobe’s video model, whereas the broader Firefly application also supplies editing, audio, and partner-model workflows.

The generation guide includes composition-reference video and camera controls. Adobe’s model-specific prompting guide says each Firefly Video generation starts from a fresh prompt rather than carrying a conversation’s edits forward. It describes training on licensed material, Adobe Stock, and public-domain content; Adobe’s commercial-use positioning is a statement about its release and training policy, not a reported benchmark score.

Firefly’s partner-model documentation separately lists Veo, Gemini Omni, Kling, Ray, and Runway options. Controls and supported settings vary by the chosen model. A conversational clip editor, native audio option, or third-party video generated in Firefly should therefore be named with its actual model rather than automatically attributed to Firefly Video.

### Materials and further reading

## Adobe: Firefly Video (5 resources)

- Meet Firefly Video model Blog · 2025
- Firefly Video general availability and generation controls Release blog · 2025
- Generate videos using text prompts Docs · 2026
- Adobe Firefly Video: Prompting and controls guide Official guide · 2026
- Generate videos using partner models Docs · 2026

## 26. Amazon: Nova Reel

Nova Reel 1.1 extended the original six-second generator to multi-shot videos of up to two minutes. Automated mode expands one prompt into successive shots; manual mode lets the caller specify each shot. The generation guide describes 1280 × 720 output at 24 fps, with duration in six-second increments. In image-conditioned generation, the reference image supplies the starting frame.

The service card distinguishes a stitched multi-shot output from its individual six-second shots and documents intended use, limitations, and evaluations. The two-minute duration should be read as a multi-shot workflow, not a demonstrated two-minute continuous take. Its API used asynchronous invocation with results written to customer storage.

Availability is now historical. AWS’s lifecycle table records 30 September 2026 as the end-of-life date for both `amazon.nova-reel-v1:0` and `amazon.nova-reel-v1:1` in their listed regions. Retained generation examples do not override that notice. The separately announced Nova 2 Omni accepts video for understanding but its preview announcement specifies text and image output; it should not be counted as a replacement video generator on that evidence.

### Materials and further reading

## Amazon: Nova Reel (5 resources)

- Nova Reel 1.1: Up to two-minute multi-shot videos Blog · 2025
- Generating videos with Amazon Nova Reel Historical API docs · 2026
- Amazon Nova Reel AI service card Service card · 2026
- Bedrock model lifecycle: Nova Reel 1.0 and 1.1 end of life Lifecycle notice · 2026
- Nova 2 Omni preview: Input and output modalities Announcement · 2025

## 27. Pika: Pika 2.5 and editing tools

Pika 2.5 is the company’s current branded video model. Its dedicated page separates a single-shot path from Frames: text alone produces a five-second clip, a starting image supports five or ten seconds, and ordered images define transitions between keyframes. The model returns silent video. Pika Soundtrack is a separate tool for adding audio to a completed cut; its presence in the same workspace does not establish joint audio–video generation in Pika 2.5.

The Pikaframes guide accepts two to five images in order, with a shared optional motion prompt. Resolution and duration depend on the number of anchors and each transition’s length. This is keyframe-conditioned generation, distinct from editing a source video. Pika’s API catalog lists Pikadditions and Pikaswaps as separate video-to-video operations, alongside effect and audio tools.

Pika’s platform release and help center distinguish task-specific apps from the models that produce their output. The current developer catalog includes many third-party models as well as Pika’s own releases. For reproducibility, record the selected model and task endpoint: a Seedance, Kling, or Gemini clip made inside Pika is not evidence about the Pika 2.5 checkpoint.

### Materials and further reading

## Pika: Pika 2.5 and editing tools (7 resources)

- Pika 2.5: Inputs, duration, resolution, and silent output Model guide · 2026
- Pikaframes: Ordered-keyframe generation Guide · 2026
- Pika help center: Apps, models, and editing workflows Docs · 2026
- Pika API: Native and third-party model catalog API docs · 2026
- Pika API task and model matrix API docs · 2026
- Introducing Pika Soundtrack Technical / release blog · 2026
- Welcome to the new Pika Release blog · 2026

## 28. Midjourney: Video V1

Midjourney’s Video V1, released in June 2025, starts from an image rather than offering a standalone text-only video interface. Users can animate a generated or uploaded image, either with an automatic motion prompt or with their own instructions. The launch introduces low- and high-motion settings and four successive extensions of roughly four seconds each beyond the initial five-second clip.

The current video documentation adds operational details for end frames, looping, batch size, and Raw mode. These controls steer motion and transitions; they do not describe instruction-based editing of an arbitrary source video. The HD release documents native 720p generation. Access and GPU-time requirements differ by mode and plan, so the living usage guide is the appropriate place to check current availability.

The numbering of Midjourney’s image and video models is separate. The image version guide discusses V8.2 and its image-editing model, whereas the video launch and video-specific guide describe the animation workflow. A current image version should therefore not be relabeled as a correspondingly numbered video model. The cited video materials disclose user controls and outputs, without providing downloadable model weights or a detailed architecture report.

### Materials and further reading

## Midjourney: Video V1 (4 resources)

- Introducing our V1 video model Release blog · 2025
- Midjourney Video: Current usage and controls Docs · 2026
- HD mode for video Release notes · 2025
- Midjourney image versions: Distinct from Video V1 Docs · 2026

## 29. HeyGen: Video and avatar models

HeyGen Video is the company’s general-purpose video model, separate from its established avatar and voice products. The official guide describes text-to-video with synchronized audio, image-to-video, and reference-video conditioning for motion, composition, or style. Access is documented through the API and Creative Studio.

The avatar line solves a more constrained problem. HeyGen’s July 2026 update describes long-form talking-video generation for Avatar III, IV, and V, with identity consistency over extended speech. Its linked streaming inference report describes audio-driven generation, identity-aware super-resolution, and chunked decoding for the Avatar IV/V families. The same product update discusses Video Agent and HyperFrames, which organize scripts, presenters, graphics, and other media. Video Agent and HyperFrames’ assembled output length should not be read as the single-clip duration of HeyGen Video.

For comparisons, distinguish general scene synthesis, an audio-driven presenter, and an agent that assembles a complete video. The conditioning, failure modes, and evaluation sets differ even when all three are accessible within the same product.

### Materials and further reading

## HeyGen: Video and avatar models (3 resources)

- Introducing HeyGen Video Model guide · 2026
- July 2026 product updates: Long-form avatars and Video Agent Release notes · 2026
- Avatar Real-time: Streaming long-form AI video Technical report · 2026

## 30. Higgsfield: DoP and video tools

Higgsfield publishes both its own video tools and a platform that hosts other vendors’ models. Its DoP guide explicitly identifies DoP as a proprietary image-to-video model: a starting image, text prompt, and camera or effects preset determine a short directed shot. The documented interface uses the name Higgsfield Standard and supports three- or five-second generations.

The versioned API pages document Genjutsu and Genjutsu Light for transforming source footage while retaining motion. The `higgsfield/genjutsu-light/motion-transfer/v1.0` endpoint trims the source to 15 seconds and preserves its audio. The website guide separately documents Genjutsu Lite for 4–30-second clips; these product limits should not be substituted for the versioned endpoint contract. These source-conditioned transformations differ from generating an unconstrained scene from text.

The official CLI catalog also lists Cinematic Studio video interfaces alongside external models such as Veo, Seedance, and Wan. A workflow name or preset should therefore be kept distinct from a documented foundation-model release. The materials below establish product interfaces and their supported controls; they do not provide model weights or a complete training report for the current studio system.

### Materials and further reading

## Higgsfield: DoP and video tools (6 resources)

- Higgsfield DoP model and usage guide Docs · 2026
- Higgsfield API model catalog API catalog · 2026
- Higgsfield official CLI model schemas Code · interface schemas · 2026
- Genjutsu Light Motion Transfer v1.0 Versioned API endpoint · 2026
- Genjutsu Motion Transfer v1.0 Versioned API endpoint · 2026
- How do I use Higgsfield Genjutsu? Official product guide · 2026

## 31. Utopai Studios: Utopai X

The September 2026 release introduces Utopai X as a proprietary text-to-video model post-trained from MiniMax H3. The same announcement introduces PAI, a workspace that retains scripts, assets, shot plans, takes, and revisions. The generation model and the surrounding production system have different responsibilities.

The official FAQ locates Utopai X inside PAI. Its editing and version-management workflow should not automatically be interpreted as evidence that the underlying model performs arbitrary instruction-based video editing. The capability discussion emphasizes camera control, materials, lighting, and matching sound, drawing on a dated external evaluation. Those findings describe a specific tested release rather than a permanent ranking.

### Materials and further reading

## Utopai Studios: Utopai X (3 resources)

- Introducing the all-new PAI and Utopai X Release blog · 2026
- Utopai X: A video model built for filmmaking Model discussion · 2026
- Utopai Studios FAQ Product documentation · 2026

## 32. Sapiens AI: Agnes Video

The Agnes Video 2.5 documentation defines three useful conditioning modes: text, keyframes, and multimodal references. Keyframes specify the first frame, last frame, or both; reference media guide content, timing, style, or motion. The service exposes asynchronous video jobs through an OpenAI Videos-compatible request format, which is an interface choice rather than evidence of OpenAI model ancestry.

The official model catalog distinguishes `agnes-video-2.5` from `agnes-video-2.5-flash`. The base release accepts image, audio, and video references at multiple resolution tiers; Flash is restricted to 720p and excludes video references. Audio as an input reference does not, by itself, establish a particular joint audio-video training architecture. The public materials consulted provide an API contract, not a detailed architecture report or downloadable video weights.

### Materials and further reading

## Sapiens AI: Agnes Video (2 resources)

- Agnes Video 2.5 API documentation · 2026
- Official Agnes AI model catalog Model reference · 2026

## 33. Creatify: Boreal and Aurora

Creatify’s Boreal page describes post-training a 22B-parameter open-weight base model that generates audio and video jointly. It positions Boreal around efficient text- and image-to-video generation, with short clips and simple scenes as its stated strengths. The current page does not name that base checkpoint, so the parameter count alone should not be used to assign a specific model lineage.

Boreal-H3 is a separate release explicitly post-trained from MiniMax H3, centered on advertising workflows and reference consistency. Its technical discussion reports an advertising quality index and reference-fidelity measurements with explicit pass thresholds. These vendor-run evaluations measure a particular production objective and should be read alongside broader preference benchmarks.

The Aurora 2.0 model page separately documents talking video from a photograph and a supplied audio track. Aurora’s audio-driven performance, Boreal’s general generation, and Boreal-H3’s reference-guided advertising capabilities are therefore separate entries, even though they share the Creatify application.

### Materials and further reading

## Creatify: Boreal and Aurora (3 resources)

- Boreal: Post-trained text- and image-to-video Model page · 2026
- Introducing Boreal-H3: The next frontier in video is post-training Technical blog · 2026
- Aurora 2.0: Photo- and audio-driven video Model documentation · 2026

## 34. Video Rebirth: BACH

Video Rebirth’s May 2026 launch announcement introduced BACH as a production-oriented video engine with a Montage workflow for generating multi-shot sequences up to thirty seconds from references and shot descriptions. The company describes native 1080P output and sound effects, voiceover, and music generated within the same workflow. It names Dual Diffusion Transformer and Physics-Native Attention as proprietary designs; these are the developer’s architectural descriptions, without the reproducible implementation detail of an open technical report.

The current official enterprise site identifies BACH 1.0 Pro and describes conditioning with up to twelve image, video, and audio references. It separately offers private model customization, studio collaboration, and production services. Those engagements should be distinguished from the public generation product at bach.art. These are product-level capability descriptions: a leaderboard entry, enterprise offering, and a particular public API endpoint may expose different configurations. The linked materials establish a proprietary model and hosted product, rather than a downloadable-weight release.

### Materials and further reading

## Video Rebirth: BACH (3 resources)

- BACH launch: Company-issued announcement Release announcement · 2026
- Video Rebirth: BACH 1.0 Pro and enterprise capabilities Official product overview · 2026
- BACH: Official generation product Product · 2026

## 35. Vimoo: Vimoo 1.0

The Vimoo 1.0 technical report describes an approximately 11.7B-parameter dual-stream multimodal diffusion transformer, a pretrained text encoder, and a compressed video VAE. It separates 720p generation from a learned Enhancer that raises resolution to 1080p. Lite introduces a more efficient representation through spatial patchification; efficiency claims therefore belong to a particular model and inference configuration.

The model overview and API documentation enumerate text-to-video, image-to-video, a unified Lite path, and a dedicated e-commerce mode. They also illustrate why report and service specifications must remain separate: the report discusses ten-second generation, while the current model overview lists five-second standard outputs and a twenty-second product-video mode.

The project describes the family as open source, but its release page marks local-running access as coming soon and does not supply a public checkpoint or code download in the material reviewed. It also notes separate licenses for code, weights, acceleration adapters, and third-party components. This article therefore does not count Vimoo as a verified downloadable-weight release.

### Materials and further reading

## Vimoo: Vimoo 1.0 (4 resources)

- Vimoo 1.0 technical report Report · 2026
- Vimoo 1.0 model overview Model documentation · 2026
- Vimoo API documentation API documentation · 2026
- Vimoo release and artifact entry points Project page · 2026

## 36. Meta: Movie Gen

The Movie Gen report presents a family covering video generation, personalization, instruction-based editing, and audio generation. Its largest video transformer has 30B parameters; the reported context corresponds to 16 seconds at 16 frames per second. The family is useful as a research reference because it studies several media tasks under a common development program.

The distinction between family and model is especially important here. Video-to-audio and text-to-audio are separate tasks in the report, so synchronized audiovisual output does not imply that every Movie Gen component jointly generates sound and pixels in one network. Personalization from a subject image is also different from using an image as the literal first frame. This chapter refers to the documented research release; it does not equate that release with every subsequent Meta consumer video feature.

### Materials and further reading

## Meta: Movie Gen (2 resources)

- Movie Gen: A cast of media foundation models Research publication · 2024
- Movie Gen: A cast of media foundation models — full paper Technical report · 2024

## 37. Stability AI: Stable Video Diffusion

The Stable Video Diffusion paper studies a staged recipe: image pretraining, video pretraining, and high-quality video fine-tuning. It examines data curation as well as the extension of latent image diffusion into a temporal model. The research includes text-to-video experiments, but this does not make every released checkpoint text-conditioned.

The SVD-XT model card documents image-to-video generation and explicitly states that the released model cannot be controlled through text. It also records limited duration and possible weak motion. The official implementation and model card remain useful baselines for studying temporal adaptation, with the checkpoint’s license and conditioning interface kept distinct from later commercial video services.

### Materials and further reading

## Stability AI: Stable Video Diffusion (4 resources)

- Stable Video Diffusion: Scaling latent video diffusion models to large datasets Official research summary · 2023
- Stable Video Diffusion SVD-XT model card and weights Model card · weights · 2023
- Stability AI generative models Code · 2023
- Stable Video Diffusion: Scaling latent video diffusion models to large datasets — full paper Research paper · 2023

## 38. Genmo: Mochi

The Mochi 1 release blog and official implementation describe a 10B-parameter video diffusion model built around an asymmetric diffusion transformer. Its visual and text streams receive different representational capacity, with shared multimodal attention connecting them. The associated AsymmVAE compresses space and time before the generative model operates, making the latent representation part of the efficiency design.

The official model card identifies the downloadable release as Mochi 1 Preview under Apache 2.0. The repository includes inference and LoRA fine-tuning support. This makes Mochi useful for inspecting and adapting an actual generation pipeline, while the Preview checkpoint’s documented task remains text-to-video. A community workflow adding other controls should be distinguished from the original released interface.

### Materials and further reading

## Genmo: Mochi (3 resources)

- Mochi official implementation, architecture, and fine-tuning Code · technical overview · 2024
- Mochi 1 Preview Model card · weights · 2024
- Mochi 1: A new SOTA in open text-to-video Original technical blog · 2024

## 39. HPC-AI Tech: Open-Sora

Open-Sora is an independent open research project, with versioned implementations of data processing, video compression, training, and sampling. The 2.0 release contains an 11B video model and training code; earlier branches preserve the different architectures and conditioning interfaces of 1.0 through 1.3, with the 1.x research documented in its original technical report. The 2.0 repository identifies one checkpoint for T2V and I2V, but recommends a FLUX-assisted text-to-image-to-video pipeline for higher-quality text-prompt results; that auxiliary image model is part of the demonstrated system configuration.

The Open-Sora 2.0 report focuses on the cost of building a capable video generator and the training choices behind that result. Its published budget is a claim about a specific experiment and accounting scope, not a general estimate for reproducing every commercial system. The project’s value for research lies in the inspectable pipeline and released checkpoints. Despite its name, it is not an OpenAI Sora implementation or an official release of Sora weights.

### Materials and further reading

## HPC-AI Tech: Open-Sora (3 resources)

- Open-Sora official repository and release branches Code · weights · 2025
- Open-Sora 2.0: Training a commercial-level video generation model in $200k Report · 2025
- Open-Sora: Democratizing efficient video production for all Report · 2024

## 40. Temporal modeling, control, and release boundaries

**The temporal representation sets the modeling problem.** A video VAE compresses space and time before generation, as documented in HunyuanVideo, Wan, and CogVideoX. A model that denoises a complete latent clip and one that predicts successive chunks face different context and continuation problems. MAGI-1 and SkyReels V2 make chunking or rolling continuation explicit. Neither design, by itself, guarantees long-range identity or physical correctness.

**Generation length has several meanings.** A single clip, an extended sequence, a multi-shot assembly, and a streaming avatar are different outputs. Nova Reel 1.1 assembles shots; LongCat-Video supports repeated continuation; Vidu S2 targets an interactive stream. Report the elementary generation window, total delivered duration, and any stitching or extension procedure. The same principle applies to native resolution, upscaling, interpolation, and exported frame rate.

**Conditioning is a contract.** A first-frame image fixes an opening state, reference assets guide a new composition, a driving performance transfers movement, and source-video editing preserves parts of existing footage. Gen-4.5, Aleph, and Act-Two demonstrate these separate interfaces. Compare models on the control actually supplied, including whether a prompt rewriter or intermediate image generator contributes to the result.

**Sound can enter at different stages.** Veo 3 models audio and video jointly; HunyuanVideo-Avatar and LongCat-Video-Avatar animate from supplied speech; Pika Soundtrack adds sound after silent video generation. These pipelines require different tests. Audio quality alone does not measure lip synchronization, and a synchronized face does not establish that the system generated the words or voice.

**Checkpoint identity survives the product wrapper.** MiniMax H3 Base, fal’s H3 Max, and a hosted workflow with additional processing are separate systems for measurement. An open repository, downloadable weights, a license, and a public API are also separate release artifacts. A product may host several companies’ models, while a research family may expose only selected checkpoints.

**Evaluation should match the intended use.** For a short creative shot, prompt following and perceptual preference may be central. For editing, preservation and temporal consistency become essential; for a talking avatar, identity and speech alignment matter; for an interactive world, response latency and state consistency matter. The relevant comparison is the specified model, controls, and pipeline on that task, with its evaluation date and configuration recorded.
