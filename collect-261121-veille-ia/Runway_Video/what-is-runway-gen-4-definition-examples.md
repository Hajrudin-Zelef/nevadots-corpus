---
id: collect-261121-veille-ia/runway-video/what-is-runway-gen-4-definition-examples
title: "What Is Runway Gen-4? Definition + Examples"
domain: runway-video
role: reference
task: article
actors: ["Runway"]
dates: ["2026-10-01"]
keywords: ["gen-4", "runway", "frame", "turbo", "credits", "across", "character", "clip"]
source: https://www.8frame.co/blog/what-is-runway-gen-4
source_anchor: ""
source_lines: [1, 59]
section: "Runway_Video"
---
**Correction, 2026-10-01:** we called Veo 3.1 and Kling v3 "stronger picks" for cinematic shots and cheap variants, and said a driving video "beats" prompting expressions. We have not measured either, and on 8frame Gen-4 Turbo (40 credits for 5 seconds) is cheaper per clip than Kling v3 Standard (57 silent). Those lines now compare price and features, and the examples say what to check.

**Correction, 2026-09-30:** we said Gen-4 outputs up to 60 seconds at 4K with native audio and described a Motion Brush 3.0 tool. Runway's own pages state no length, resolution or audio for Gen-4, and we could not verify the Motion Brush version, so those claims are removed and the Gen-4 Turbo specs on 8frame are added.

Runway Gen-4 is Runway's flagship AI video generation model, built around a World Consistency engine that keeps characters, locations, and objects visually stable across a multi-shot sequence generated from a single reference image.

Runway has since released Gen-4.5, which its API prices at 12 credits a second against 5 for Gen-4 Turbo. The pitch was not raw clip quality, which most leading models already deliver, but continuity: the ability to keep the same character recognizable across cuts, camera angles, and lighting conditions without stitching clips by hand or re-rolling until faces match. That is the problem Gen-4 was built to solve, and it is the reason to reach for it over a general text-to-video model. Runway's full Gen-4 suite is not on the 8frame canvas (8frame carries Runway Gen-4 Turbo and Act Two for generation), so treat this as a field guide to Runway's own platform rather than something you run end-to-end from your 8frame workflows.

## How Runway Gen-4 works

Most text-to-video models generate each clip independently. Ask for the same character in two shots and you get two people who look vaguely related. Gen-4 holds a latent representation of your characters and locations across the whole generation session, so identity persists frame to frame and shot to shot. You upload one reference image, write a scene description with motion and camera parameters, and the model renders clips where the subject stays recognizably the same across endless lighting conditions and locations.

Runway's Gen-4 page describes consistency ("Precisely generate consistent characters, locations and objects across scenes") and states no clip length, resolution or audio, so we do not quote any. On 8frame, Gen-4 Turbo runs from a start image, 2 to 10 seconds at 8 credits a second, at 720p class, with no sound setting. Two related Runway tools matter here. Act-Two is performance capture: feed it a driving performance video plus a character reference and the character mimics the actor's expressions, head motion, and body language. Aleph, in Runway's words, makes "a wide range of edits on an input video such as adding, removing, and transforming objects, generating any angle of a scene, and modifying style and lighting". Act-Two runs on 8frame at 8 credits a second of reference (3 to 30 seconds); Aleph and Gen-4.5 are not hosted here.

Runway prices on a subscription-plus-credits model, not per clip. Plans run Standard at $12 (625 credits a month), Pro at $28 (2,250), and Max at $76 (9,500), with generation billed in credits per second: 12 for Gen-4.5 and 5 for Gen-4 Turbo, at $0.01 a credit. That structure matters when you compare it to per-clip pricing, which we do below.

## When you use Runway Gen-4

Use it when continuity across shots is the whole job. A narrative sequence, a branded explainer with a recurring on-screen character, or a multi-shot ad where the same product and the same presenter have to read as identical from cut to cut. This is where Gen-4's World Consistency earns its place, and where single-clip models force you into manual cleanup.

Act-Two is the reason to pick Gen-4 for performance-driven work. If you have an actor delivering a line and you want an animated or stylized character to carry that exact performance, a driving video hands the model the timing and expressions directly instead of describing them in a prompt. Aleph is the reason to pick it for editing rather than generation: reshooting a scene from a new angle or swapping a prop without regenerating the whole clip is a genuinely different capability from most video models.

For a single shot or a variant batch, pick on price and features. Gen-4 Turbo on 8frame needs a start image and has no sound setting: 40 credits for 5 seconds. If the shot needs sound in the same pass, Veo 3.1 generates it with the picture (448 credits for 8 seconds, 168 on Veo 3.1 Fast) and so does Kling v3 Standard (86 for 5 seconds from a start image, 57 silent). For cheap drafts, Wan 3.0 is 24 credits for 5 seconds at 480p. Price the batch on each before you commit subscription credits on Runway's own platform.

## Examples

**Recurring brand spokesperson across a 30-second explainer:** Upload one reference portrait of your presenter. Write four scene descriptions (desk intro, product close-up, walking transition, sign-off) with camera and lighting cues per shot. Gen-4 is built to keep characters consistent across scenes, so the job is to check it: compare the presenter's face and wardrobe across all four shots before you cut them together. Add room tone in the edit; Gen-4 Turbo has no sound setting on 8frame.

**Performance-captured character for a stylized ad:** Record a 10-second driving clip of your talent delivering the hook to camera on a phone. Feed it to Act-Two with a stylized character reference. The character is meant to take on the actor's timing, blinks and head tilt; check those against the driving clip, since they are the part that reads as human. Then use Aleph to change the background lighting from day to golden hour without re-rendering the performance.

## Related concepts

Character consistency in AI is the core problem Gen-4's World Consistency engine targets. Understanding how identity drift happens across shots helps you judge when you actually need a continuity model versus a single-clip generator.

Image-to-video AI is the input mode Gen-4 relies on: one reference image conditions the whole session. Reading how image conditioning constrains motion output makes Gen-4's reference-first approach easier to prompt well.

## FAQ

### Is Runway Gen-4 on 8frame?

Gen-4 Turbo and Act-Two are; the full Gen-4 suite, Aleph and Gen-4.5 are not. Gen-4 Turbo runs from a start image for 2 to 10 seconds at 8 credits a second, and Act-Two costs 8 credits a second of reference video (3 to 30 seconds).

### How does Runway price Gen-4?

Per second, out of plan credits: Standard $12 a month billed yearly (625 credits), Pro $28 (2,250), Max $76 (9,500). Gen-4 Turbo uses 5 credits a second and Gen-4.5 uses 12, at $0.01 a credit.

### Does Gen-4 Turbo generate sound?

Not on 8frame: it has no sound setting. Add sound in the edit, or use a model that generates it with the picture, such as Veo 3.1 or Kling v3.

Runway's full suite lives outside the 8frame canvas, but the models you would compare it against do not. See how the leading video models stack up in the best AI video generator guide, or read the head-to-head in Runway Gen-4 vs Veo 3.1.

## Sources

- Gen-4 consistency claim: Runway's Gen-4 page; Aleph: runway.com/research/introducing-runway-aleph (2025-07-25); plan prices and per-second rates: runwayml.com/pricing and docs.dev.runwayml.com/guides/pricing, checked 2026-09-30
- Gen-4 Turbo and Act-Two on 8frame: the 8frame app, checked 2026-09-30
- Veo 3.1, Kling v3 Standard and Wan 3.0 prices on 8frame: the 8frame app, checked 2026-10-01

**Try it without signing up:** the 8frame canvas opens on a real board. The canvas is free and unlimited; generation is paid from $19/month.
