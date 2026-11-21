---
id: collect-261121-veille-ia/modeles-open/black-forest-labs-latent-space
title: "Black Forest Labs — Latent Space"
domain: modeles-open
role: reference
task: article
actors: []
dates: []
keywords: ["flux", "models", "editing", "diffusion", "black", "forest", "labs", "video"]
source: https://latent.space/wiki/black-forest-labs-437bc88f/
source_anchor: ""
source_lines: [1, 74]
section: "Modeles_Open"
---
## Browse · Organizations

# Black Forest Labs

The developer of FLUX evolved from text-to-image models into reference-based editing and unified image, video, audio, and action modeling, combining downloadable weights with commercial APIs.

Black Forest Labs (BFL) develops the FLUX family of generative models. Its work expanded from text-to-image synthesis to image editing and a unified architecture trained on images, video, and audio, with extensions for action prediction. On August 4, 2026, it made initial FLUX 3 Video generation generally available through its API and selected partners. Across generations, BFL has combined commercial endpoints with downloadable models under variant-specific licenses—an approach it calls **open core**.[2][5][9][10]

## Origins and the 2024 launch

That product line began with BFL’s public launch on August 1, 2024. Founders Robin Rombach, Patrick Esser, and Andreas Blattmann brought earlier generative-model research experience: at launch, investor Andreessen Horowitz identified them as co-creators of the Stable Diffusion series. BFL’s team also credited its members with contributions to VQGAN, latent diffusion, Stable Diffusion XL, Stable Video Diffusion, rectified-flow transformers, and adversarial diffusion distillation. This earlier work supplied the team’s technical background; it preceded the company.[2][3]

BFL’s launch announcement reported the successful closing of a **$31 million seed round**, led by Andreessen Horowitz. Angel participants included Brendan Iribe, Michael Ovitz, Garry Tan, Timo Aila, and Vladlen Koltun; BFL also named follow-up investments from General Catalyst and MätchVC. Its initial advisory board included Ovitz and neural-style-transfer researcher Matthias Bethge.[2]

The first release, FLUX.1, established the company’s distribution strategy. **[pro]** offered commercial API access, including through Replicate and fal.ai. **[dev]** provided open weights for noncommercial applications and was guidance-distilled directly from [pro]. **[schnell]**, intended for local development and personal use, used the Apache 2.0 license. Hugging Face hosted downloadable weights, while inference code and integrations included GitHub, Diffusers, and ComfyUI. These choices made licensing a property of each variant rather than of the FLUX family as a whole.[2]

The public FLUX.1 models described at launch had 12 billion parameters and combined multimodal and parallel diffusion-transformer blocks. They used **flow matching**, a generative training method that includes diffusion as a special case, alongside rotary positional embeddings and parallel attention layers. BFL also announced plans for text-to-video models, making video a stated direction from its first release.[2]

## From image generation to reference-based editing

On May 29, 2025, BFL introduced FLUX.1 Kontext, combining generation and editing conditioned on both text and images. Users could change a specific element, carry a reference character into another scene, preserve a reference style, or build on earlier edits through successive instructions. One demonstration changed the text “YOU HAD ME AT BEER” to “YOU HAD ME AT CONTEXT,” then changed the setting to a nightclub. BFL acknowledged that excessive iterative editing could degrade images, alongside occasional instruction failures, limited world knowledge, and artifacts introduced by distillation.[4]

Kontext [pro] and experimental [max] were offered through the BFL API and partners; the 12B open-weight [dev] variant initially entered a private beta for research and safety testing. **OpenArt and KreaAI supported preference-data collection.** BFL simultaneously introduced its Playground, a browser interface for testing models and demonstrating use cases before integrating the API.[4]

BFL announced FLUX.2 on November 25, 2025. The family combined image generation and editing from text and multiple references in one model, supporting up to ten reference images and editing at up to four megapixels. Its architecture coupled a Mistral-3 24B vision-language model with a rectified-flow transformer and used a retrained latent space. Managed [pro] and [flex] endpoints sat alongside a 32B open-weight [dev] checkpoint; [flex] exposed inference-step and guidance controls. **NVIDIA and ComfyUI collaborated on an optimized fp8 reference implementation** for running [dev] on consumer GPUs. BFL also released the FLUX.2 variational autoencoder under Apache 2.0.[5]

## Financing and reported scale

On December 1, 2025, BFL announced a $300 million Series B at a **$3.25 billion post-money valuation**, naming Salesforce Ventures and Anjney Midha (AMP) as co-leads. It listed existing partners including a16z, NVIDIA, Northzone, Creandum, Earlybird VC, BroadLight Capital, and General Catalyst, alongside participants Temasek, Bain Capital Ventures, Air Street Capital, Visionaries Club, Canva, and Figma Ventures. BFL said the funding would accelerate research and development.[6]

In the same announcement, BFL reported “Millions of users” of its FLUX models and named Adobe, Canva, Meta, and Microsoft as partners building creative experiences on them. The user figure came without an activity period or paying-customer definition. BFL described a small team and headquarters in Freiburg and San Francisco, without giving a headcount.[6]

## Compact models for interactive workflows

On January 15, 2026, BFL released [FLUX.2 [klein]](https://bfl.ai/blog/flux2-klein-towards-interactive-visual-intelligence), compact models combining generation, editing, and multiple-reference input. The family included 4B and 9B variants and undistilled base versions for customization and fine-tuning. The **4B variants used Apache 2.0; the 9B variants used the FLUX Non-Commercial License**. The distilled 9B variant paired a 9B flow model with an 8B Qwen3 text embedder and required four inference steps.[7]

BFL reported that the 4B model fit in approximately 13GB of VRAM and presented subsecond inference results; its performance-analysis chart measured speed on a GB200 in bf16. NVIDIA collaborated on FP8 and NVFP4 versions to reduce the hardware footprint. Separate quantization benchmarks used RTX 5080/5090 GPUs for 1024×1024 text-to-image generation, making the reported efficiency gains specific to those conditions.[7]

## Self-Flow and the multimodal transition

Self-Flow integrated representation learning into flow matching without external representation supervision. Its **Dual-Timestep Scheduling** applied different noise levels across tokens: a student learned to denoise a mixed-noise input while reconstructing features supplied by a less-noised teacher. Hila Chefer and Patrick Esser were equal contributors, joined by Dominik Lorenz, Dustin Podell, Vikash Raja, Vinh Tong, Antonio Torralba, and Robin Rombach. The authors were affiliated with BFL, with Torralba also affiliated with MIT.[8]

The researchers tested joint image, video, and audio generation with a single 4B FLUX.2 backbone. Their reported experiment used six million training videos, 200 million training images, and 100,000 high-resolution fine-tuning steps over a low-resolution multimodal model. Action-prediction experiments separately fine-tuned 675M-parameter video-weighted models and evaluated them in the SIMPLER simulator. The authors reported improvements over vanilla flow matching, including a sustained advantage on complex multi-object and sequential manipulation tasks.[8]

On July 23, 2026, BFL announced FLUX 3 in early access, explicitly building on Self-Flow and jointly training on images, video, and audio in one architecture. Its action work followed two routes: native action prediction and specialized models fine-tuned from the video backbone. With **mimic robotics**, BFL jointly developed FLUX-mimic, combining that backbone with mimic’s expertise in dexterous robot learning and production deployment. BFL referenced testing on real production tasks at Audi.[9]

The August 4 video release made initial generation capabilities generally available: clips up to 20 seconds long with native audio, HD 720p output, and Full HD 1080p through upscaling. Inputs could include text, images, keyframes, or up to four seconds of existing video and audio for continuation. A draft mode supplied a cheaper preview before full-quality rendering. BFL worked with Cinder on pre-release risk evaluation. FLUX 3 Image and the open-weight FLUX 3 Dev remained roadmap items in that announcement, keeping the video rollout distinct from the broader architecture’s capabilities.[10]

## Timeline

## Financing

**$31 million seed round**

BFL’s launch announcement reported the successful closing of a $31 million seed round, led by Andreessen Horowitz.[2]

**$300 million Series B**

On December 1, 2025, BFL announced a $300 million Series B at a $3.25 billion post-money valuation, naming Salesforce Ventures and Anjney Midha (AMP) as co-leads.[6]

## Sources

1. 1
2. 2
3. 3
4. 4
5. 5
6. 6
7. 7
8. 8
9. 9
10. 10

Model source and teaching reviews passed. Human editorial review pending. This profile describes the sources as of 2026-09-17.
