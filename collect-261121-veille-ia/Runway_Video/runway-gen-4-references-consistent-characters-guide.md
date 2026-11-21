---
id: collect-261121-veille-ia/runway-video/runway-gen-4-references-consistent-characters-guide
title: "Runway Gen-4 References: Consistent Characters Guide"
domain: runway-video
role: reference
task: article
actors: ["Runway"]
dates: []
keywords: ["references", "reference", "shot", "gen-4", "character", "consistent", "images", "prompt"]
source: https://www.dreamdrive.ai/learn/runway/runway-gen-4-references-consistent-characters/
source_anchor: ""
source_lines: [1, 63]
section: "Runway_Video"
---
**Short answer**

Gen-4 References lets you upload up to three reference images, for example a character, an object and a location, and point at them in your prompt with @ tags. Runway then generates new images that keep those elements recognisable from shot to shot, and those images become the start frames for consistent video.

The hardest part of making a short film with AI tools is not the first shot; it is the second one, where the hero suddenly has a different jaw and the kitchen has moved. Runway’s Gen-4 References exists to solve this. You give it a small set of reference images and refer to them by name inside the prompt, and the model composes new scenes that keep those people, props and places recognisable. This guide explains how to pick references, how the @ tags work and how to build a shot list that stays consistent.

## What Gen-4 References does

References is an image generation mode within Runway’s Gen-4 family. You upload up to three reference images, each representing something you want to keep: a character, an object such as a car or a product, or a location. In the prompt you refer to each one with an @ tag, and the model combines them into a new composition following your description. The output is a still image, which you then send to image-to-video to make the actual shot (see the Gen-4 image-to-video guide).

The important shift is that consistency comes from the pictures rather than from ever-longer text descriptions. A paragraph about "a woman with a short auburn bob, freckles and a green parka" is still useful, but the reference image is what the model actually leans on. The exact labels and layout of the References panel change from time to time, so look for the reference-image slots next to the prompt box in the current interface.

## Choosing good reference images

The quality of your references decides the quality of everything downstream. A few rules that hold up in practice:

- **One subject per image.** A character reference should show the character alone against a plain or quiet background. If your reference is a busy group shot, the model has to guess which person you mean.
- **Front-facing, evenly lit.** For faces, a neutral expression and soft light give the model the most information. Dramatic side lighting looks great but hides half the face.
- **Show the distinctive details.** If the character always wears a particular jacket or carries a specific bag, make sure it is visible and in focus.
- **Locations need the layout, not the mood.** A wide, clear shot of the room or street works better than an atmospheric close-up of one corner.
- **Keep style consistent across the set.** A photographic character next to an illustrated location will pull the output in two directions.

If you are coming from Midjourney, the thinking is close to its character and style reference workflow, covered in Midjourney consistent characters. The difference is that Runway’s references are meant to be composed together into one scene.

## How to write prompts with @ tags

Each reference gets a short name, and you use that name in the prompt prefixed with @. Treat the tags as nouns and build an ordinary scene description around them.

1. **Name your references clearly.** Use words you will not confuse later: @mara, @redcar, @kitchen rather than @image1, @image2.
2. **Place each tag where the noun would go.** "@mara sits at the table in @kitchen, morning light through the window, medium shot."
3. **Describe the new elements, not the referenced ones.** You do not need to re-describe Mara’s hair; describe what she is doing, the framing and the light.
4. **Specify the shot type.** Close-up, medium, wide, over-the-shoulder. Consistency across a sequence comes partly from coherent framing choices.
5. **Use the same tags across every shot in the scene.** Reuse the identical reference set for the whole sequence; swapping one image mid-scene is where drift creeps in.

You can leave one of the three slots empty if you only need two references. Three is the maximum, so for scenes with more recurring elements you will need to prioritise: usually the character and the location matter most, and small props can be described in words.

## Settings at a glance

| Setting | What it does | Start with | 
|---|---|---|
| Reference images | Up to three stills the model keeps consistent (characters, objects, locations) | One clean image per element, consistent style | 
| @ tags | Names that link a word in the prompt to a reference | Short, memorable nouns | 
| Prompt | The new scene: action, framing, light | One sentence with every tag used once | 
| Aspect ratio | Shape of the generated still, which becomes your video ratio | Your final delivery ratio | 
| Fixed seed | Reuse one seed to compare prompt edits fairly | On once you have a good composition | 

## Building a consistent shot list

A practical sequence for a short scene looks like this. First, create or choose your three references and save them in a project folder with their names. Second, write the shot list in plain language, one line per shot, each using the tags. Third, generate each still with References, keeping the same reference set and aspect ratio throughout. Fourth, send each approved still through image-to-video with a short motion prompt. Finally, record the prompt, seed and reference files for each shot so you can regenerate a single shot later without redoing the scene.

Generate stills in order of difficulty, starting with the simplest framing. If the character holds together in a wide shot and a medium shot, close-ups will usually follow. If the face is already wrong in the first still, fix the reference image before generating more.

### Common problems

- **The character looks like a cousin, not a twin.** Replace the reference with a sharper, front-on image, and reduce how much the prompt asks the character to change (new outfit, new hairstyle).
- **Two references merge into one.** Make the references visually distinct and name them unambiguously in the prompt.
- **The location is right but the layout flips.** Add a clear spatial cue: "window on the left, door behind her".
- **The still is consistent but the video drifts.** Use smaller movements and 5 second clips in image-to-video; consistency in the still does not guarantee it through large motion.

## Try it

`@mara leans against the doorway of @kitchen holding a mug, looking out of the window, soft morning light, medium shot at eye level, 35mm film look`
Replace the two tags with your own reference names, change the action and the framing for each shot in your list, and keep the lighting phrase identical across the scene so the shots cut together. If the character’s face shifts, try a reference photo with a more neutral expression and flatter light.
