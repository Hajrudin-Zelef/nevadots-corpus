---
id: collect-261121-veille-ia/modeles-open/flux-3-flux-3
title: "Flux 3 (FLUX.3)"
domain: modeles-open
role: reference
task: article
actors: []
dates: ["2026-08-06"]
keywords: ["flux", "model", "audio", "sound", "image", "frame", "video", "first"]
source: https://help.artlist.io/hc/en-us/articles/38170367122589-Flux-3-FLUX-3
source_anchor: ""
source_lines: [1, 60]
section: "Modeles_Open"
---
*Published: August 6, 2026*

## Video Model

FLUX.3 is Black Forest Labs' first video model, and the first FLUX release that generates video at all. Every clip comes back with synchronized audio produced in the same pass — speech with lipsync, effects, and ambience.

It runs up to 20 seconds in a single generation at 24 FPS.

## Variants at a Glance

| Variant | Type | Modalities | Max Res. | Talking Points | 
|---|---|---|---|---|
| Variant |  |  |  |  | 
| Text-to-Video | Text only | Text → V+A | 1080p | No reference material needed. Strongest on one clear event with the sound named in the prompt. | 
| Image-to-Video | Animate | Image → V+A | 1080p | Pins your image as the first frame and animates forward, holding the look of the source. | 
| First / Last Frame | Transition | 2 images → V+A | 1080p | Both ends fixed, model fills the move between them. Use it when the landing frame matters. | 

## Key Features

**Audio**

- 
**Same-pass generation:** Sound is rendered with the picture, so an effect lands on the exact frame the event happens.
- 
**Speech and lipsync:** Dialogue with strong lip sync, and multilingual delivery without a separate voice model.
- 
**Effects and ambience:** Foley and room tone come with the shot; naming the sound in the prompt is what makes it reliable.

**Output Control**

- 
**Free-form duration:** Any whole second from 5 to 20, or leave it on auto and let the model choose.
- 
**Aspect ratio:** Seven ratios from 21:9 through 9:21, plus an Auto mode that picks the best fit from the prompt.
- 
**Grounding:** Ties the shot to plausible real-world physics, materials, and scale. On by default; turning it off suits surreal or heavily stylized work.

## Technical Capabilities

| Spec | Details | 
|---|---|
| Quality / Resolution | Full quality (720p, 1080p) | 
| Duration | Any whole second, 5–20s, or auto | 
| Frame Rate | 24 FPS | 
| Aspect Ratios | 21:9, 16:9, 4:3, 1:1, 3:4, 9:16, 9:21, auto | 
| Audio | Native, generated in the same pass; on by default, can be disabled | 
| Input Modalities | Text, image, existing video | 

## Limitations

- Quiet, low-motion scenes often come back with little audible sound unless the prompt names an explicit audio cue.
- 20s is the ceiling per generation. No image generation — FLUX 3 Image is a separate release on its own timeline.

## Prompting Tips

- Build the shot around one clear event, and let motion travel visibly from one object to the next.
- Name the sound you want — "a single bell strike", "papery rustles", "rhythmic rail noise". Leaving audio implicit is what produces silent clips.
- Say what the camera does. Slow lateral dolly, orbit, focus rack — geometry holds through the move when the move is specified.
- Name the material behaviour you expect: cloth catching a gust, rain beading on the lens.
- Write at the level you actually think about the idea — a single line often lands.
