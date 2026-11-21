---
id: collect-261121-veille-ia/actu-business/seedance-2-5-flash-pricing-what-it-costs-per-second-of-video
title: "Seedance 2.5 Flash Pricing: What It Costs Per Second of Video"
domain: actu-business
role: reference
task: article
actors: []
dates: []
keywords: ["seedance", "flash", "flagship", "model", "second", "pricing", "video", "reference"]
source: https://www.mindstudio.ai/blog/seedance-2-5-flash-pricing
source_anchor: ""
source_lines: [1, 78]
section: "Actu_Business"
---
## What does Seedance 2.5 Flash cost per second?

Seedance 2.5 Flash runs at roughly 15 cents per second of generated video, with the exact number shifting depending on which platform or API provider you access it through. ByteDance positions Flash as the budget option in the Seedance 2.5 family, pricing it about 34% cheaper than the flagship Seedance 2.5 model. The tradeoff isn’t just speed, it’s a hard ceiling on resolution and a much smaller allowance for reference images, both of which limit what the model can realistically do compared to its full-size sibling.

## TL;DR

- **Seedance 2.5 Flash costs around 15 cents per second** , roughly 34% less than the flagship Seedance 2.5 model, though actual pricing varies by provider.
- **Resolution tops out at 480p or 720p** , while the flagship model can generate at 1080p with noticeably better texture detail and color.
- **Reference images are capped at 20** , down from the 50 references supported by the flagship, which matters for anyone building consistent characters or scenes.
- **Output quality is inconsistent** , with decoherence, object vanishing, and morphing showing up more often than in flagship generations using identical prompts.
- **Heavier prompting and more reference material noticeably improve results** , suggesting Flash needs more hand-holding to stay coherent than the full model does.
- **This is effectively a day-one API release** , so quality and reliability are likely to shift as ByteDance tunes the model further.
- **Flash follows “draft mode” as ByteDance’s second attempt at a cheaper Seedance variant** , signaling a broader push to compete with lower-cost rivals like MiniMax Hailuo.

### Built like a system. Not vibe-coded.

Remy manages the project — every layer architected, not stitched together at the last second.

## How does Seedance 2.5 Flash pricing compare to the flagship model?

The headline number is a roughly 34% discount versus the flagship Seedance 2.5 model. In practice that works out to around 15 cents per second of video, though this varies depending on the platform you’re generating through, since third-party API resellers and aggregators often mark pricing up or down from the base rate.

That discount comes with two concrete restrictions:

- **Resolution cap** : Flash tops out at 480p or 720p. The flagship model can push to 1080p, which shows up directly in output quality, sharper textures and noticeably better color reproduction.
- **Reference image limit** : Flash supports up to 20 reference images, compared to 50 for the flagship. For workflows that lean on multiple character, location, and storyboard references to keep a scene consistent, that’s a meaningful cut.

For anyone pricing out a project, the math is straightforward: if you’re generating large volumes of short clips where imperfections are tolerable, the 34% savings adds up fast. If you need consistent characters across many shots or final-resolution output, the flagship’s higher price may still be the cheaper option once you account for wasted regenerations.

## Is Seedance 2.5 Flash actually good, or just cheap?

Testing shows a mixed picture. On straightforward scenes, text-to-video generations with simple character counts and dialogue, Flash produces outputs that hold up reasonably well on first glance. A western bar scene with two characters and a short dialogue exchange came out mostly coherent, though background elements showed decoherence (a glass refilling itself with whiskey on its own, for instance).

Running the identical prompt through the flagship Seedance 2.5 model produced a similar scene without that decoherence, and with the added benefit of 1080p output, which noticeably improved texture and color quality over Flash’s capped resolution.

The gap widens on more complex or action-heavy scenes. A test involving a getaway car fleeing from police showed the vehicle vanishing partway through the clip. Image-to-video generations fared worse in some cases, with props (like a sniper rifle’s scope) appearing and disappearing inconsistently, and explosion effects rendering in a visually mismatched style compared to the rest of the scene.

The likely explanation: Seedance’s flagship models are believed to run on an underlying language model architecture that functions as a reasoning layer for video generation, essentially a “thinking” step before frames get produced. Flash appears to have that reasoning step dialed down significantly, trading coherence for speed and lower cost.

## What actually improves Flash’s output quality?

Across multiple tests, the clearest pattern is that Flash performs much better with heavier, more detailed prompting and more reference material. A detailed prompt using up to nine references, including a named character, a specific location, and scene-by-scene storyboards, produced a workable result with no major vanishing or morphing issues, even if the output looked a bit noisy or grainy.

*Nothing gets lost.*

Techniques like depth map referencing also helped hold scenes together more reliably, avoiding the decoherence seen in lighter-prompted generations. The practical takeaway: Flash isn’t a model you can treat like the flagship and expect similar reliability from similar effort. It needs more explicit guidance, more references, and more structural scaffolding to produce consistent results.

Not every input helps, though. One test using an audio reference to drive video generation produced an odd result where a character’s voice and appearance drifted partway through the clip, suggesting audio-driven generation is still a weaker point for the model.

## Is Seedance 2.5 Flash worth using right now?

For some use cases, yes. Simple scenes, heavily detailed prompts, and projects where occasional regeneration is acceptable can benefit from the lower per-second cost. For anything requiring consistency across a longer sequence, accurate action, or final-resolution output, the current version of Flash introduces enough unpredictability that the savings may get eaten up by wasted generation credits.

It’s also worth noting that this is close to the model’s initial API release. ByteDance has already iterated once before on cost-reduction with “draft mode,” a lower-resolution generation mode that fully regenerates footage rather than upscaling it. Flash is the second attempt at the same problem: making Seedance’s output more affordable without gutting what makes the flagship model distinct. Given that pattern, further tuning and likely improvement to Flash’s coherence and reliability over time wouldn’t be surprising.

The broader context matters too. Seedance 2.5’s flagship model has held a reputation as one of the strongest video generation models available, but also one of the most expensive. Competitors like MiniMax’s Hailuo have closed the quality gap at a fraction of the cost, and Flash reads as ByteDance’s direct response to that competitive pressure.

## Frequently Asked Questions

### How much does Seedance 2.5 Flash cost per second of video?

Pricing comes in around 15 cents per second, though the exact rate depends on which platform or API provider is used. It’s roughly 34% cheaper than the flagship Seedance 2.5 model.

### What’s the maximum resolution for Seedance 2.5 Flash?

Flash caps out at 480p or 720p. The flagship Seedance 2.5 model supports 1080p output, with visibly better texture and color detail.

### How many reference images can Seedance 2.5 Flash use?

Flash supports up to 20 reference images, compared to 50 for the flagship model, which limits how much visual consistency you can enforce across a scene or character.

### Is Seedance 2.5 Flash the same as draft mode?

No. Draft mode generates at lower resolution and fully regenerates footage rather than upscaling it, and was ByteDance’s earlier attempt at a lower-cost option. Flash is a separate, newer model aimed at the same cost problem, with its own resolution and reference limits.

### Does cheaper pricing mean worse output quality?

Generally yes, in its current form. Flash shows more decoherence, object vanishing, and morphing issues than the flagship model, especially on action-heavy or image-to-video generations. Heavier prompting and more reference material can reduce these issues but don’t eliminate them entirely.
