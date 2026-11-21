---
id: collect-261121-veille-ia/modeles-open/flux-3-explained-every-model-price-and-how-they-compare
title: "FLUX 3 Explained: Every Model, Price and How They Compare"
domain: modeles-open
role: reference
task: article
actors: []
dates: ["2026-07-23"]
keywords: ["flux", "video", "black", "forest", "labs", "model", "image", "images"]
source: https://www.cinemadrop.com/blog/flux-3
source_anchor: ""
source_lines: [1, 172]
section: "Modeles_Open"
---
FLUX 3 is Black Forest Labs’ multimodal foundation model: one model trained on images, video and audio together, announced on July 23, 2026. It isn’t a single product. Black Forest Labs now lists five models under the name: FLUX 3 Video, FLUX 3 Max, FLUX 3 Image, the open-weights FLUX 3 Image Klein and the robotics model FLUX 3 Action. This guide explains what each one does, when it was released, what it costs, how the models compare with each other and with Seedance 2.5, and which ones matter if you make films.

**Quick answer: what is FLUX 3?**

- What it is: a multimodal model family from Black Forest Labs, the German lab behind FLUX. The same underlying model makes video with sound, images and, in a separate version, robot actions.
- For video: FLUX 3 Video makes clips of up to 20 seconds with native audio. FLUX 3 Max is fal’s faster post-trained version, about 12 seconds for a 10-second clip in **fal’s own benchmark** .
- For images: FLUX 3 Image (October 1, 2026) and the smaller, open-weights FLUX 3 Image Klein (October 15, 2026).
- Price: video from $0.15 per second at HD on Black Forest Labs and $0.17 per second at 720p on fal; images from $0.048 (FLUX 3 Image) or $0.009 (Klein) per 1-megapixel image.
- Open source? Partly. **Klein and FLUX 3 Action have downloadable weights.** The video models don’t.
- On CinemaDrop? Yes, except FLUX 3 Action. FLUX 3 Image and FLUX 3 Image Klein are on every plan; FLUX 3 (CinemaDrop’s name for FLUX 3 Video) and FLUX 3 Max are on Basic and up.

## What is FLUX 3?

Black Forest Labs calls it “our new multimodal foundation model”. Earlier FLUX models only made images. This one learns from images, video and audio within one architecture, on the idea that each one describes the same world from a different angle: images capture space, video adds time and motion, and audio ties sounds to physical events (Black Forest Labs, FLUX 3 announcement).

The model is built on Self-Flow, Black Forest Labs’ method for training generation and understanding in the same model. In practice that means one backbone can generate video and audio jointly, generate and edit images, and be fine-tuned to predict actions for robots.

Black Forest Labs is releasing it in phases, each after an early-access period: video first, then action prediction with partners, then images, then an open-weights backbone. So the name can mean the base model or any of the products below.

## The models in the family

These are the models Black Forest Labs lists on its model page as of October 20, 2026:

| Model | Made by | What it makes | Released | Open weights? | 
|---|---|---|---|---|
| FLUX 3 Video | Black Forest Labs | Video with native audio, up to 20 s | Announced Jul 23; API preview Aug 4, 2026 | No | 
| FLUX 3 Max | fal Research with Black Forest Labs | Text-to-video and image-to-video, 720p | Oct 19, 2026 (fal) | No | 
| FLUX 3 Image | Black Forest Labs | Image generation and editing, up to 4K | Oct 1, 2026 | No (commercial weights license for companies) | 
| FLUX 3 Image Klein | Black Forest Labs | Smaller 5.6B image model | Oct 15, 2026 | Yes | 
| FLUX 3 Action | Black Forest Labs | Robot and game actions plus predicted video | Sep 23, 2026 | Yes | 

A sixth piece, the Dev model, is announced but not out. Black Forest Labs describes it as open-weight access to the multimodal backbone and says it is “coming soon”.

### FLUX 3 Video

This is the model most people mean by “FLUX 3”. It makes clips of up to 20 seconds in one generation, with optional native audio that includes multilingual dialogue, synchronized speech, sound effects and ambience. Inputs cover text-to-video, image-to-video with 1 to 10 pinned keyframes, video-to-video from a reference clip and video continuation. Black Forest Labs also highlights multiple shots in one take and accurate on-screen text (Black Forest Labs, FLUX 3 Video).

Output runs from HD (about 1 megapixel per frame) up to UHD/4K. A Draft mode returns a cheaper, faster HD preview, which you can then render at full quality with the same composition. Video continuation is capped at 15 seconds, while the other modes go to 20 (Black Forest Labs release notes).

### FLUX 3 Max

FLUX 3 Max is a post-trained version of FLUX 3 made by fal, a platform that serves AI models through an API. fal Research kept training the base model with Black Forest Labs for visual fidelity, motion coherence, prompt adherence and image-to-video quality, then reworked how it runs so it generates faster. It launched on October 19, 2026, and fal says it is still post-training it (fal, FLUX 3 Max page).

fal lists two endpoints for Max, text-to-video and image-to-video, both at 720p. fal describes the draft, keyframe and extend endpoints as part of Black Forest Labs’ base FLUX 3 release, not Max. VentureBeat, citing fal’s documentation, reports 5- to 20-second clips, an optional 1080p upscale, audio on or off and low or high reasoning settings for text-to-video (VentureBeat, Oct 19, 2026). Check fal’s current docs before you plan around any one parameter.

### FLUX 3 Image

FLUX 3 Image is the image side of the same model, released through the API on October 1, 2026. Its standout feature is layout control. You place each element on the canvas with a bounding box and describe what goes in it, then edit the finished image one box at a time while everything else stays exactly as it was. It also takes up to 10 reference images and renders up to 4K at 15 aspect ratios (Black Forest Labs, FLUX 3 Image).

### FLUX 3 Image Klein

Klein is the open-weights member of the FLUX 3 Image family: a 5.6B-parameter model for text-to-image, image editing and multi-reference generation with up to 10 references, with the same bounding-box format as FLUX 3 Image. It ships as a base model for fine-tuning and LoRA training, and as a faster distilled model. Black Forest Labs says the INT8 build runs on NVIDIA GeForce RTX GPUs with as little as 8 GB of VRAM, and that the distilled model with the INT8 build makes a 1-megapixel image in about 4 seconds on an RTX 5090 (Black Forest Labs, FLUX 3 Image Klein).

### FLUX 3 Action

FLUX 3 Action is a 7B open-weights “world action model” released on September 23, 2026. It takes camera frames, a robot’s state and a text instruction and predicts the next couple of seconds of actions together with future video frames. It’s built for robotics, simulators and games, not filmmaking, but it shows how wide the shared backbone is meant to reach (Black Forest Labs on Hugging Face, FLUX 3 Action).

## Pricing and access

Every model in the family is pay-as-you-go, with no subscription. Video is billed per second of output and images per image. Prices below are as listed on October 20, 2026, rechecked against Black Forest Labs’ and fal’s pages on October 23, 2026, and against fal’s FLUX 3 Max page again on November 8, 2026.

| Video model (where) | 720p/HD | 1080p/FHD | 2K/QHD | 4K/UHD | 
|---|---|---|---|---|
| FLUX 3 Video (Black Forest Labs, text or image to video) | $0.15/s (Draft $0.06/s) | $0.24/s | $0.28/s | $0.41/s | 
| FLUX 3 (fal, text to video) | $0.17/s | $0.29/s | $0.32/s | $0.47/s | 
| FLUX 3 Max (fal), regular | $0.17/s | $0.29/s (upscaled) | – | – | 
| FLUX 3 Max (fal), 50% promo through November 22, 2026 (UTC) | $0.085/s | $0.145/s (upscaled) | – | – | 

Sources: Black Forest Labs’ FLUX 3 Video FAQ, fal’s FLUX 3 text-to-video page and fal’s FLUX 3 Max page. fal’s Max page lists both 720p and 1080p rates. On Black Forest Labs, video-to-video costs more ($0.41/s at HD), and audio is included at no extra charge.

| Image model (Black Forest Labs API) | 768 × 768 | 1K | 2K | 4K | 
|---|---|---|---|---|
| FLUX 3 Image | $0.041 | $0.048 | $0.100 | $0.607 | 
| FLUX 3 Image Klein | $0.008 | $0.009 | $0.014 | $0.036 | 

Image prices are from Black Forest Labs’ release notes. Klein is also free to run on your own GPU under its license (see the open-source section below).

The number that matters is cost per usable shot. A 10-second HD clip costs $1.70 at the standard rate on either platform, or $0.85 on FLUX 3 Max during fal’s 50% promo through November 22, 2026 (UTC). Ten takes of that shot cost $17 at the standard rate. Draft mode on Black Forest Labs ($0.60 per 10-second draft) is the cheaper way to explore before committing to a full render.

## How the models compare

For video, the real choice is between Black Forest Labs’ own FLUX 3 Video and fal’s FLUX 3 Max. They differ most in speed, features and resolution.

| Comparison point | FLUX 3 Video (BFL) | FLUX 3 Max (fal) | 
|---|---|---|
| Time for a 10-second clip (fal’s test) | 126.44 s | 12.45 s | 
| Image-to-video Elo (Artificial Analysis, preliminary, fal snapshot) | 1,089 | 1,139 | 
| Inputs | Text, image with 1–10 keyframes, video-to-video, continuation | Text, image (fal lists two endpoints) | 
| Highest resolution | UHD/4K | 720p native, 1080p upscale | 
| Draft mode | Yes | Not listed | 
| HD list price | $0.15/s | $0.17/s ($0.085/s, 50% promo through November 22, 2026 (UTC)) | 

The speed figures are fal’s own benchmark, averaged for a 10-second, 16:9 clip at 720p. In the same test, Gemini Omni Flash 1.1 took 47.37 s, MiniMax H3 83.26 s at 768p, Wan 3.0 180.16 s and Seedance 2.5 277.80 s. fal notes that settings and load affect completion time, so treat these as best-case averages.

On quality, FLUX 3 Max is statistically tied with the leaders. When VentureBeat checked the Artificial Analysis image-to-video leaderboard on October 19, MiniMax H3 led at 1,140 Elo, with FLUX 3 Max and Seedance 2.5 both at 1,138. The confidence intervals overlap, so FLUX 3 Max could rank anywhere from first to fifth (Artificial Analysis Video Arena). The arena is new, so scores will move as votes come in, and it only covers image-to-video.

Black Forest Labs’ own early tests, using 10-second 720p text-to-video clips with audio, had the base FLUX 3 preferred over Gemini Omni Flash in 52% of comparisons, close to a coin flip (Black Forest Labs, FLUX 3 announcement). Those are the vendor’s preliminary numbers, not independent results.

Which to pick: FLUX 3 Max when you want many fast takes of a shot at 720p. FLUX 3 Video when you need keyframes, video-to-video, Draft mode or output above 1080p. For stills, FLUX 3 Image gives the most control, and Klein is the cheaper or self-hosted option.

## How it stacks up against Seedance 2.5

| Comparison point | FLUX 3 Max | FLUX 3 Video | Seedance 2.5 | 
|---|---|---|---|
| Image-to-video quality (AA, preliminary) | 1,138–1,139 Elo | 1,089 Elo (fal snapshot) | 1,138–1,139 Elo | 
| Time for a 10-second clip (fal’s test) | 12.45 s | 126.44 s | 277.80 s | 
| Longest clip | 20 s (VentureBeat) | 20 s | See our Seedance 2.5 guide | 
| On CinemaDrop? | Yes, on the Basic plan and up | Yes, on the Basic plan and up (as FLUX 3) | Yes, on the Plus plan and up | 

On quality, FLUX 3 Max and Seedance 2.5 are level in the one public arena that ranks them both. FLUX 3 Max wins clearly on speed, in fal’s test. Both are on CinemaDrop (FLUX 3 Max on Basic and up, Seedance 2.5 on Plus and up), so you can test them side by side alongside your script and storyboard, without writing API code. For more of the field, see our Kling 4.0 guide.

## Is FLUX 3 open source?

Partly, and not for video yet. Two models in the family have downloadable weights: FLUX 3 Action (September 23) and FLUX 3 Image Klein (October 15). Both use the FLUX Kommunity License, which allows research and non-commercial use, plus free commercial use for individuals and companies with under $5M in annual revenue. Larger companies need a self-hosted license or the API.

FLUX 3 Video, FLUX 3 Max and FLUX 3 Image are API-only. Black Forest Labs offers private or commercial weight access to companies, and says open-weight access to the multimodal FLUX 3 Dev backbone is coming. If you need video models you can run yourself today, see our guide to open-source AI video generators.

## What it means for AI filmmaking

Three things stand out for filmmakers. Native audio means a clip comes back with dialogue and sound effects already synced to the picture. Keyframes and multi-shot generation let you set how a shot begins and ends and let the model fill in the motion, which is how a storyboard artist already thinks. And speed changes how you work. If a take arrives in seconds, pre-viz becomes a conversation: block the shot, look, adjust the prompt, look again. That loop matters most when the plan comes first.

You can work that way on CinemaDrop today with FLUX 3 Max. Write and break down the scene, plan shots in the free AI storyboard creator, create stills with the image models in the storyboard, then turn storyboard frames into video with FLUX 3 Max, Seedance 2.5 or the other models in the AI video generator. FLUX 3 Max needs the Basic plan or higher and Seedance 2.5 the Plus plan, so check CinemaDrop pricing first. For a ByteDance option for stills, see our Seedream 5.0 guide.

## Limits and open questions

- Vendor benchmarks. The speed figures come from fal, the preference tests from Black Forest Labs, and the only public quality ranking is preliminary and statistically tied.
- API-first. FLUX 3 Video, Max and Image are built for developers and API platforms, not as a full editing app.
- No open video weights. The open releases so far are an image model and a robotics model.
- FLUX 3 Max is 720p native. 1080p is an upscale, and fewer endpoints are listed than for FLUX 3 Video.
- Still changing. fal is still post-training Max, Black Forest Labs calls its evaluations early, and FLUX 3 Dev has no date.

## FAQ

### What is FLUX 3 and what does it do?

FLUX 3 is Black Forest Labs’ multimodal foundation model, trained on images, video and audio together. It powers a family of models: FLUX 3 Video for clips with sound, FLUX 3 Image and Image Klein for stills and editing, FLUX 3 Action for robotics, and fal’s faster FLUX 3 Max.

### Is FLUX 3 released?

Yes, in stages. Black Forest Labs announced FLUX 3 on July 23, 2026 and opened FLUX 3 Video as an API preview on August 4. FLUX 3 Action followed on September 23, FLUX 3 Image on October 1 and FLUX 3 Image Klein on October 15. fal launched FLUX 3 Max on October 19. FLUX 3 Dev is announced but not yet released.

### Will FLUX 3 be open source?

Some of it already is. FLUX 3 Image Klein and FLUX 3 Action have open weights under the FLUX Kommunity License. The video models don’t, though Black Forest Labs says an open-weight FLUX 3 Dev backbone is coming.

### Can you run FLUX 3 locally?

Only part of the family. FLUX 3 Image Klein is built for your own hardware: Black Forest Labs says its INT8 build runs on NVIDIA GeForce RTX GPUs with as little as 8 GB of VRAM, through ComfyUI or Diffusers (Black Forest Labs). FLUX 3 Action’s weights are downloadable too. FLUX 3 Video, FLUX 3 Max and FLUX 3 Image run through APIs, and their weights are offered only to companies under a license.

### What is FLUX 3 Max?

A version of the base model that fal Research post-trained with Black Forest Labs for quality and speed. In fal’s test it makes a 10-second 720p clip in 12.45 seconds, about 10 times faster than the base FLUX 3.

### How much does FLUX 3 cost?

Video starts at $0.15 per second at HD on Black Forest Labs and $0.17 per second at 720p on fal, with Black Forest Labs’ Draft mode at $0.06 per second. FLUX 3 Max is $0.085 per second at 720p during the 50% promo through November 22, 2026 (UTC). Images cost $0.048 per 1K image on FLUX 3 Image and $0.009 on Klein.

### Is FLUX 3 good?

Early signs are strong. FLUX 3 Max is statistically tied with the top image-to-video models on Artificial Analysis’s preliminary arena, and far faster than them in fal’s test. Independent hands-on tests are still scarce.

### Who is behind FLUX?

Black Forest Labs, led by CEO Robin Rombach, builds the FLUX models. fal Research post-trained FLUX 3 Max with them.

### Can I use FLUX 3 on CinemaDrop?

Yes. FLUX 3 Image and FLUX 3 Image Klein are on every plan, and FLUX 3 and FLUX 3 Max video are on Basic and up.

### Can I use FLUX 3 videos and images commercially?

fal says content generated through its API, including FLUX 3 Max, can be used commercially, subject to its terms. Check Black Forest Labs’ terms if you use its API directly. Running the open Klein weights commercially is free only under the $5M revenue threshold in the FLUX Kommunity License.
