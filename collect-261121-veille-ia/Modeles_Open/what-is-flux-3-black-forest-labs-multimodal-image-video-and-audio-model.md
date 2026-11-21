---
id: collect-261121-veille-ia/modeles-open/what-is-flux-3-black-forest-labs-multimodal-image-video-and-audio-model
title: "What Is FLUX 3? Black Forest Labs' Multimodal Image, Video, and Audio Model"
domain: modeles-open
role: reference
task: article
actors: []
dates: []
keywords: ["video", "flux", "audio", "model", "labs", "black", "forest", "image"]
source: https://digitalmatters.me/artificial-intelligence-ai/what-is-flux-3/
source_anchor: ""
source_lines: [1, 108]
section: "Modeles_Open"
---
**FLUX 3** is Black Forest Labs’ new multimodal foundation model, announced on July 23, 2026, that the company says learns from images, video and audio inside one architecture rather than stitching separate models together. The headline capability is video generation of up to 20 seconds with native synchronized audio, which would be the first video model and the first audio model the company has ever shipped. The more useful thing to understand about the announcement is how little of it you can currently use: FLUX 3 was launched as a staged rollout, and a week later only the video piece is open, behind an application form. This piece walks through what the company claims, what the evidence supports, and which parts are still a roadmap.

## What Black Forest Labs actually announced

The launch post is not a product release. It describes four things arriving "over the next few weeks and months," each behind its own early-access phase:

- **FLUX 3 Video** : open now, but gated. Access runs through an application form that Black Forest Labs approves manually.
- **FLUX 3 Image** : “an early access phase in the following weeks.” As of today it has not opened and carries no date.
- **FLUX 3 Action** and**FLUX-mimic** : available “through selected research and commercial partners,” starting with the robotics company mimic.
- **FLUX 3 Dev** : the open-weight backbone. Listed last, with no date and no license.

The company’s own product page for FLUX 3 currently reads "Coming Soon" with a request-access button, which sits awkwardly next to the blog’s statement that Video is "now available in Early Access." Neither the API documentation nor the public playground lists FLUX 3 at all. If you are evaluating this for a workflow, the practical status today is: you can apply, and Black Forest Labs decides.

## One backbone, three modalities, and an asterisk

The architectural claim is the reason the announcement matters, so it is worth quoting precisely. The blog says: "FLUX 3 is our new multimodal foundation model. It jointly learns from images, videos, and audio within a unified architecture."

Action prediction is then described as an extension rather than a fourth co-trained modality, reached by two routes: native action prediction built into FLUX 3 directly, and using the pretrained video backbone as a foundation that specialized action models can be fine-tuned from with limited task-specific data.

That distinction gets lost in the company’s own marketing. The press release subhead says "Jointly trained across image, video, audio, and action prediction modalities," and the homepage tagline reads "One multimodal model for Image / Video / Audio / Action-Prediction," while the body of the same press release reverts to the narrower framing. Read the technical prose, not the tagline.

The thesis behind it is stated cleanly by CEO Robin Rombach: "You can’t cheat reality. A model that only learns images can only generate images." Black Forest Labs is arguing that visual generation and physical prediction are the same problem, and that a model which has learned how sound and motion accompany events understands the world better than one trained on stills. That is a real position, and it puts the company on a collision course with both the video-generation specialists and the robotics foundation-model labs.

It is also a departure from how the rest of the image-generation field has developed. Google’s Imagen line and the image models we looked at in Nano Banana treat still generation as its own specialism, and the tools that shaped the category, including the ones covered in our Midjourney walkthrough, never attempted video or audio at all. Black Forest Labs is betting that the specialists are building on too narrow a foundation.

## What FLUX 3 Video generates

On the video side the company is specific. Every output "comes with native audio generation," and the model "can create highly diverse videos with audio up to 20 seconds in length in a single generation." The listed modes are text-to-video, image-to-video (either continuing from a starting frame or using images as visual references), video-to-video from a reference clip that carries elements such as a character into a new scene, generative video-audio continuation from an input video and audio, and keyframe-to-video for controlled transitions between defined moments.

Two capabilities are worth flagging for anyone thinking about production use. The model handles multilingual dialogue, which very few video models attempt. And it supports what the company calls "agentic chaining of individual clips into longer, multi-shot sequences," with visual references used to keep characters consistent, producing sequences the company says can last several minutes. That is a workflow claim rather than a single-generation claim, and it has not been demonstrated publicly.

## The comparison numbers, and the gap inside them

Black Forest Labs published preference rates against six competitors. FLUX 3 was preferred over Luma Ray 3.2 in 93% of comparisons, over Runway Gen-4.5 in 77%, and over Grok Imagine Video in "up to" 69%. Further down the same list, and to the company’s credit published rather than buried, are Kling v3 Pro at 60% and both Seedance 2.0 and Gemini Omni Flash at 52%, which is statistical parity.

These are the company’s own evaluations, and the disclosure stops at the test condition: "we generated 10-second text-to-video clips in 720p with audio." Nothing is published about rater counts, rater sourcing, prompt sets, blinding, confidence intervals, or which competitor versions were tested. The company labels the results "preliminary" and says the harness is "still in development."

The most substantive problem is the one hiding in that sentence. The headline capability is 20 seconds. The evaluation ran at 10. Nothing published tests whether quality holds across the full advertised length, and temporal coherence is exactly where video models degrade. There is also no independent verification available and no realistic prospect of any while access is gated: FLUX 3 does not appear in third-party video arenas, and it cannot until the model is open enough for someone else to run it. For context on how a competing multimodal video model presents its own numbers, our coverage of Seedance 2.5 ran into the same vendor-benchmark pattern.

## The open-weight promise has not shipped

Black Forest Labs built its reputation on open weights, so FLUX 3 Dev is the part of the launch plan that its existing users care about most. It does not exist. The company’s Hugging Face organization currently lists the FLUX.2 family and the older FLUX.1 line, with nothing from FLUX 3 in any form. The only commitment is one launch-plan bullet describing "open-weight access to a multimodal backbone," plus a press-release line saying open-weight versions will follow "later this year."

No license has been announced. That matters more than it sounds, because the precedent is restrictive: FLUX.2-dev ships under the FLUX [dev] Non-Commercial License, where the weights are non-commercial even though generated outputs can be used commercially. Nobody should assume FLUX 3 Dev inherits those terms, and nobody should assume it improves on them either. The honest summary is that an open-weight multimodal backbone was announced and sequenced last, behind three commercial phases.

## Robotics is the best-evidenced part of the launch

The section of the announcement most likely to be dismissed as a roadmap slide is the one with the most substance behind it.

The foundation is a peer-reviewed paper. "Self-Supervised Flow Matching for Scalable Multi-Modal Synthesis" (arXiv 2603.06507) was accepted at ICML 2026, and its authors include Rombach and Patrick Esser alongside MIT’s Antonio Torralba. It covers image, video, audio and robot action prediction, evaluated on the RT-1 dataset in the SIMPLER simulator, and the code and a checkpoint are released under Apache 2.0. The scale is research-grade rather than production, but the artifact is real and independently readable.

FLUX-mimic is further along than that. The companion post describes a lightweight action decoder sitting on intermediate features from the video prediction path, reporting backbone inference under 80ms on a single RTX 5090 and full-system reaction times of 101ms. The company says it is tested and deployed at Audi on kitting parts into trays, inserting control units into tight fixtures, and handling soft materials.

That post also contains the most credible engineering detail in the whole launch, because it is a cost rather than a benefit. Adding action prediction to the training curriculum initially dropped human ratings on text-to-video and image-to-video by up to 10%, recovering after 3,500 steps. Vendors do not usually publish the part where their idea made something worse first.

## What you cannot buy yet

There is no published pricing for FLUX 3 anywhere: not on the blog, not on the pricing page, not in the API documentation. Partner platforms including Canva, Krea and Picsart are named as testing the model, not shipping it. The infrastructure partner fal.ai lists specifications and collects emails under a "coming soon" banner.

This gap has produced a small ecosystem of sites advertising "FLUX 3 API" access, waitlist skips and per-generation pricing. Black Forest Labs has published no endpoint and no price, and its own launch partner says the model has not arrived. Treat any FLUX 3 price you encounter as fabricated until the company publishes one. The same caution applies to parameter counts, which have not been disclosed for any FLUX 3 variant.

None of that makes the announcement empty. Black Forest Labs raised $300 million in December 2025 at a $3.25 billion valuation, it employs roughly 100 people between Freiburg and San Francisco, and its founders wrote the latent diffusion work that Stable Diffusion was built on. A company with that lineage arguing that image, video, audio and action belong in one model is a claim the rest of the field will have to answer. It is simply not a thing you can use this week, and the parts that would let anyone check it, the open weights and the methodology, are the parts scheduled last.

## Frequently Asked Questions

## What is FLUX 3?

FLUX 3 is a multimodal foundation model announced by Black Forest Labs on July 23, 2026. The company describes it as jointly learning from images, video and audio within a unified architecture, with action prediction for robotics added on top of the video backbone. It is the company’s first video model and its first model to generate audio.


## Can I use FLUX 3 right now?

Only partially, and only if approved. FLUX 3 Video is in gated early access through an application form that Black Forest Labs reviews. FLUX 3 Image has not opened. FLUX 3 Action is limited to selected research and commercial partners. There is no public API documentation and no self-serve access.


## How long can FLUX 3 videos be?

Black Forest Labs says the model generates videos with audio up to 20 seconds in a single generation, and that clips can be chained agentically into multi-shot sequences lasting several minutes. Note that the company’s own published evaluation used 10-second clips at 720p, so quality across the full 20-second length has not been publicly demonstrated.


## Are the FLUX 3 open weights available?

No. An open-weight backbone called FLUX 3 Dev is listed in the launch plan, with no date and no license announced. Nothing from FLUX 3 currently appears in Black Forest Labs’ Hugging Face organization. The press release says open-weight versions will follow “later this year.” The previous generation, FLUX.2-dev, ships under a non-commercial license, so the terms for FLUX 3 Dev should not be assumed.


## Are the FLUX 3 benchmark numbers independent?

No. The preference rates against Luma Ray 3.2, Runway Gen-4.5, Grok Imagine Video and others are Black Forest Labs’ own internal evaluations, described by the company as preliminary and run with a harness it says is still in development. Rater counts, prompt sets and protocol are not disclosed. No third party has evaluated FLUX 3, and none can while access remains gated.


## What does FLUX 3 cost?

Black Forest Labs has not published pricing for FLUX 3 on its blog, its pricing page or its API documentation. Third-party sites advertising FLUX 3 API pricing or waitlist access are not reselling anything the company has released. Pricing should be expected to land with general availability.


## What is FLUX-mimic?

FLUX-mimic is the robotics application of the FLUX 3 backbone, built with the robotics company mimic. It attaches a lightweight action decoder to intermediate features from the video prediction path. Black Forest Labs reports backbone inference under 80ms on a single RTX 5090 and full-system reaction times of 101ms, and says the system is deployed at Audi on assembly and parts-handling tasks.


## How does FLUX 3 compare to other video models?

On Black Forest Labs’ own preference testing it leads clearly against Luma Ray 3.2 and Runway Gen-4.5, and lands at statistical parity with Seedance 2.0 and Gemini Omni Flash. Since those are vendor-run comparisons at 10 seconds and 720p with no methodology published, they establish that the model is competitive rather than that it leads. Independent comparison will only be possible once access widens.


**Update (2026-07-31): this article was expanded.**

The version published on July 30 stated the same facts but cited no sources. This revision adds primary references for the announcement date, the preference-rate comparisons, the licensing precedent and the access terms, and expands the sections on what has actually shipped versus what is only announced. No claim from the original version was withdrawn or corrected; the additions make each one checkable.
