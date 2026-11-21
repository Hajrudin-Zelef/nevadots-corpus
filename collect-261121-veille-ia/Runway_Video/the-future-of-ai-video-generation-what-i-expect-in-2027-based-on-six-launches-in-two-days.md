---
id: collect-261121-veille-ia/runway-video/the-future-of-ai-video-generation-what-i-expect-in-2027-based-on-six-launches-in-two-days
title: "The Future of AI Video Generation: What I Expect in 2027, Based on Six Launches in Two Days"
domain: runway-video
role: reference
task: article
actors: ["Runway"]
dates: ["2026-11-18"]
keywords: ["video", "generation", "future", "model", "google", "source", "expect", "launches"]
source: https://medium.com/@kaleemullahyounas123/the-future-of-ai-video-generation-what-i-expect-in-2027-based-on-six-launches-in-two-days-23083993153f
source_anchor: ""
source_lines: [1, 119]
section: "Runway_Video"
---
# The Future of AI Video Generation: What I Expect in 2027, Based on Six Launches in Two Days

On 17 and 18 November 2026, six AI video releases landed across two days. HeyGen shipped an avatar model that needs 30 seconds of your footage. Google made personal avatars free for every personal Google account. Suno started turning songs into lip-synced clips. Apple posted a paper on video generated in under 50 milliseconds a frame. Pika showed actors you can direct while the scene plays. Synthesia put its video agent inside Claude.

I read every announcement, then the background docs. Here is where I think the **future of AI video generation** is heading in 2027, and what I’d do about it now. Everything below is from the sources linked. Anything about 2027 is my prediction, and I say so each time.

## What is the future of AI video generation?

**The short answer:** In 2027 I expect AI video to stop being a “type a prompt, wait, download a clip” product. The launches from this week point at three things: video that renders fast enough to steer while it plays, a free digital double of yourself in mainstream apps, and video made inside other tools by agents. Labelling rules arrive alongside.

That’s the thesis. The rest of this piece is the evidence, one shift per section, plus the parts I’m unsure about.

## What shipped on 17 and 18 November

- **HeyGen** (17 Nov): Avatar 6: audio-driven avatar model, native 2K, built from about 30 seconds of video. Who can use it: Available now, platform and API. Source: HeyGen press release
- **Google** (17 Nov): Personal avatars in Google Vids, now at no cost. Who can use it: Anyone with a personal Google account. Source: Google blog
- **Suno** (17 Nov): Visuals: images and clips up to 15 seconds that follow a song. Who can use it: Web, iOS, Android; separate credits. Source: Suno blog
- **Apple researchers** (17 Nov): FastFrames: 1.3B and 14B video models at interactive latency. Who can use it: Research release (paper plus weights). Source: arXiv paper
- **Pika** (18 Nov): Enact: a live scene you can re-stage, direct and film in real time. Who can use it: Research preview, not for consumers. Source: Pika blog
- **Synthesia** (18 Nov): Synthesia connector in Claude, with its Syren video agent. Who can use it: Starter, Pro and Enterprise plans. Source: Synthesia blog

Two of the six are research. Four are things you can open today. I’m treating them differently below.

## Prediction 1: AI video generation goes real-time in 2027

Look at the numbers in Apple’s FastFrames project page. The small model (1.3B parameters) generates 480p video at 36 ms per frame on one Nvidia B200 GPU. The large one (14B) runs at 45 ms per frame on an eight-GPU B200 node, and 158 ms on a single GPU. For comparison, the same page lists Krea-Realtime, a 14B model, at 1,675 ms on one GPU.

The researchers’ main finding surprised me. They profiled the pipeline and found that steps people treat as fixed plumbing, VAE decoding and KV cache updates, took over half of end-to-end latency. They swapped those for lighter parts. Much of the speed came from engineering around the model.

Pika’s Enact preview shows what that speed is for. You drag an actor across a playing scene with your mouse and the video keeps going. Pika says the model generates 1.8 times faster than it plays on a single server, and in its low-latency setting the picture responds in under 0.4 seconds on an H100. You can talk to the actors (“move closer to her”) and they act on the note. Its camera mode renders 640x480 at 24 fps at 1.5 times real time, on an eight-GPU B300 node.

Now the caveats. Both are at 480p or close to it. Both quote numbers on data-centre GPUs most of us will never rent by the hour. Enact isn’t available to consumers. The aiholics write-up of FastFrames (18 November) notes that the quality scores are Apple’s own and haven’t been reproduced independently, and that you should read the licence files before any commercial plan.

**My prediction:** by late 2027 I expect at least one mainstream video tool to offer a live, steerable preview: you move the camera or the subject and see the result within a second, then render the final take at higher quality. I don’t expect real-time 1080p on a laptop in 2027. The gap between 480p on a B200 and that is large.

## Prediction 2: a digital double of you becomes a free default

This one is already half true.

HeyGen’s Avatar 6 builds an avatar from as little as 30 seconds of video, and you can connect YouTube, Instagram or TikTok to feed it more footage. The model’s runtime context window went from about 3 seconds to about 30, so gestures follow what you’re saying across a whole paragraph instead of looping. HeyGen says 84% of participants in its internal Turing-style test rated the avatar as realistic as the real person. That’s the company’s own test, so I’d treat it as a claim until someone independent repeats it.

The most useful number in the release, for me, is a different one. HeyGen surveyed more than 1,000 small business owners in its user base and found that 71.6% had recorded a video for their business and then decided not to post it. That’s the real problem these tools are chasing. A take that felt fine while recording looks stiff on playback, and it sits in a drafts folder.

On the same day, Google made personal avatars in Google Vids free for anyone with a personal Google account. You scan a QR code, the avatar gets added to your video prompt, and you type what it should say. Google says the avatar is restricted to the account holder’s likeness.

Both companies put consent at the centre. HeyGen requires live identity verification and explicit consent for a realistic avatar of a real person, and keeps realistic videos of real people inside its own platform; third-party platforms only get photo-based and virtual avatars. That design choice matters more than the 2K output, in my view.

**My prediction:** in 2027, I expect “make an avatar of yourself” to be a standard free feature in the big productivity suites, the way background blur became standard in video calls. I also expect identity checks to become the line between serious vendors and the rest. A tool that lets you upload anyone’s face with no verification will be a liability for any business that uses it.

## Prediction 3: AI video moves inside other apps, and agents make it

Suno is a music app. On 17 November it added Visuals: drag a song in, get clips up to 15 seconds that follow the song’s structure, pick up the lyrics and lip-sync along. The Verge reported (18 November) that the model behind it, H3 Move, is based on H3 from MiniMax. So Suno’s video feature builds on another company’s model, which Suno says it “optimized for our community”, and puts it next to the thing its users already make.

Not everyone cheered. The Verge also noted users in the launch video’s comments asking Suno to fix its audio before adding features. Fair point. Bolting video onto an app doesn’t help if the core product has gaps.

Synthesia went a different way. Its Claude connector, announced 18 November, lets you ask for a video in the chat, or point Claude at a doc, and its agent Syren builds it. Synthesia says Claude Opus 5.5 plans the video, writes the narration and writes each scene as code, which Synthesia’s engine renders. Its example workflows include “whenever a new release is published in our GitHub repo… create a 60-second video on what shipped.”

That last example is the one I keep thinking about. A changelog video nobody filmed, triggered by a release tag.

**My prediction:** in 2027 most AI video that businesses publish will not be made in a dedicated video app. It’ll be made from inside a chat assistant, a docs tool or a music app, often by an agent on a schedule. The standalone video generator becomes the engine underneath, much as Suno now uses a MiniMax-based model.

## Prediction 4: price per second keeps falling, clips keep getting longer

This part leans on background docs, not this week’s news, so I’ve dated each one.

- Google’s Gemini Omni 1.2 Flash (generally available 5 November 2026) costs $0.07 per second of 720p video, 30% less than Omni 1.1 Flash. Clip length went from 10 to 15 seconds. Drafts render at 360p or 720p, then you upscale the keeper to 1080p or 4K.
- Kuaishou’s Kling 4.0 (launched 15 October 2026) generates up to 30 seconds natively in one pass. Kuaishou says video extension up to two minutes is coming soon.

At $0.07 a second, a 15-second 720p draft costs $1.05 by my arithmetic. Ten drafts to get one keeper is about $10.50, before upscaling. That’s cheap enough that the cost of AI video shifts from “per clip” to “per regeneration”, and the person who can get a usable take in two tries instead of ten is the one who saves money.

**My prediction:** I expect another clear price cut per second from at least one major provider during 2027, and 30-second single-pass clips to be normal rather than a headline. Both are extrapolations from two data points each, so hold me to the direction, not a number.

## Will AI-generated video have to be labelled in 2027?

In the EU, it already does. Article 50 of the AI Act has applied since 2 August 2026. The Commission’s guidelines on it were published in the Official Journal on 12 November 2026, as PPC Land reported. Three points matter for video:

1. Providers of generative systems must mark outputs in a machine-readable way and make them detectable. Systems already on the market before 2 August 2026 have until **2 December 2026** to comply with that marking duty.
2. Whoever publishes a deepfake (the “deployer”) must label it so a person can see or hear the label. Per the guidelines, the deployer can’t rely on the watermark the provider embedded.
3. Penalties go up to EUR 15 million or 3% of worldwide turnover. The guidelines themselves are non-binding; final interpretation sits with the EU Court of Justice.

The vendors are already building for this. Google says Omni 1.2 Flash outputs carry an imperceptible SynthID watermark. Suno says it applies embedded watermarks to Visuals before they can be shared off-platform. Under the EU reading, though, an invisible watermark alone doesn’t cover the person posting a realistic avatar video.

**My prediction:** by mid-2027 I expect visible “AI-generated” labels on realistic avatar and deepfake-style video to be routine on major platforms for EU audiences, and many teams to apply them everywhere rather than run two versions. I’m less sure how fast this spreads outside the EU.

## Which AI video tools should you watch in 2027?

My criteria, in order: can you use it today; does it require consent for real faces; does it watermark output; and is the licence clear for commercial work. Only things I could verify from the sources above are in the table.

- **HeyGen Avatar 6** : Usable today: Yes. Real-face consent: Live ID check plus explicit consent. Watermark stated: Not stated in the release. Commercial use: Built for business use (platform and API).
- **Google Vids personal avatars** : Usable today: Yes, free with a personal account. Real-face consent: Own likeness only. Watermark stated: Not stated in the post. Commercial use: Check Google’s terms.
- **Gemini Omni 1.2 Flash** : Usable today: Yes (API, Google Vids). Real-face consent: Audio references allowlisted. Watermark stated: SynthID. Commercial use: Yes (Google Cloud).
- **Suno Visuals** : Usable today: Yes. Real-face consent: Filters block artists and public figures. Watermark stated: Embedded watermark. Commercial use: Check Suno’s terms.
- **Synthesia in Claude** : Usable today: Yes, on Starter, Pro and Enterprise plans. Real-face consent: Not covered in the post. Watermark stated: Not stated in the post. Commercial use: Business plans.
- **Apple FastFrames** : Usable today: Research only. Real-face consent: n/a. Watermark stated: n/a. Commercial use: Read the licence files first.
- **Pika Enact** : Usable today: No (research preview). Real-face consent: n/a. Watermark stated: n/a. Commercial use: No.

If I had to pick one to learn properly this winter, it’d be an avatar tool with real identity verification, because that’s the category where I expect both demand and rules to grow fastest. If you’re a developer, I’d study FastFrames instead. The paper explains where video latency actually goes, and that knowledge will outlast any one model.

## What am I least sure about?

Three things.

Quality at speed. Real-time demos run at 480p. I don’t know whether the step to 1080p takes one year or three.

Whether people will watch. HeyGen’s 84% realism figure is from its own test. Viewers may get better at spotting avatars, the way we learned to spot stock photos.

Who pays the creators. Suno’s video filters block artists’ likenesses, and Universal and Sony’s copyright case against Suno is still running, as Music Business Worldwide noted on 18 November. Video models trained on footage face the same question, and I don’t see it resolved in 2027.

## How I’d prepare for the future of AI video generation in 2027

- **Make one avatar now, of yourself, on a tool with identity checks.** Write one 60-second script and compare it with a real recording. You’ll learn more from that than from any review.
- **Budget per regeneration, not per clip.** At $0.07 a second, the number of tries is the real cost.
- **Label realistic synthetic video visibly** if any of your audience is in the EU. Don’t count on the vendor’s invisible watermark.
- **Watch the agent route.** If your team already uses a chat assistant with connectors, a scheduled “release notes to video” workflow is a small experiment with a clear yes or no.
- **Read the latency research** (FastFrames, Pika’s Enact write-up) if you build products. Real-time is where I expect the next jump.

Six launches in two days isn’t a trend by itself. But four of them point the same way: video is turning into a live, personal, embedded feature. That’s what I’ll be checking against in a year.

If you want to build AI video or other AI features into your own product, or need help with AI development or a website, JitterStack works on exactly these projects, and you can reach out to the team there.
