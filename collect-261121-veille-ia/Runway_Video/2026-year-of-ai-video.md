---
id: collect-261121-veille-ia/runway-video/2026-year-of-ai-video
title: "2026: Year of AI Video"
domain: runway-video
role: reference
task: article
actors: ["Runway"]
dates: []
keywords: ["video", "people", "then", "like", "went", "year", "been", "most"]
source: https://www.localhosthq.com/media/blogs/2026-year-of-ai-video
source_anchor: ""
source_lines: [1, 189]
section: "Runway_Video"
---
### AI Video Moved Too Fast and I Need to Talk About It

Think back to like six weeks ago.

If you were paying attention, maybe you noticed some people on X posting these weird slick looking AI video clips. A fight scene here, a movie trailer there. Nothing that made you stop scrolling. The discourse was the same as it's been for two years, "AI video is getting better but it's still not there yet," and honestly most people, even most people in the industry, were treating it as a novelty. Something to watch from a safe distance.

Then February 12th ByteDance drops Seedance 2.0, within like 24 hours someone makes a clip of Tom Cruise fighting Brad Pitt from a two line prompt, 3.2 million views, then Spider Man clips everywhere, then Darth Vader, then someone made an alternate Stranger Things ending that honestly looked better than the actual finale, and then the Deadpool writer Rhett Reese tweets "I hate to say it, it's likely over for us"

Same day, Disney sends a cease and desist, Paramount sends one too, the MPA calls it unauthorized use on a massive scale, SAG AFTRA condemns it, Human Artistry Campaign calls it an attack on every creator around the world, all of that in one day, one single day

That was the moment everyone looked up and went ok this is real now

I've spent the last year building inside this space, I organized the Mumbai AI Film Festival, I run a company at the intersection of AI and filmmaking, I watch these model releases the way a stock trader watches the ticker. And I keep getting asked "so what's actually going on with AI video" and I keep giving the polite version, the version that doesn't make me sound unhinged at dinner. But the gap between what I've been saying and what's actually happening has gotten too big. The people I care about deserve to hear what's coming, even if it sounds extreme.

So here's the honest version.

TODO(video-1): Add final hosted video URL for "Seedance clips" (0:14 / 2:56).

TODO(video-1-notes): Poster extracted from source post. Replace with playable embed/video link once uploaded.

### This didn't happen overnight, it just felt like it

Seedance didn't just show up out of nowhere, the whole space been accelerating and the last six weeks broke it open

2025 was busy but you could keep up, Kling 2.0 in April, Veo 3 in May, MiniMax Hailuo O2 in June, Sora 2 in September as a social app that hit a million downloads in five days, Flux 2 and some others landing by end of year, each launch was a news cycle, people played around with it, some creators adopted it, life went on

Then 2026 started and it just went off

LTX 2.0 in January doing open source 4K on local GPUs, Veo 3.1 update in January, Kling 3.0 February 4, Seedance 2.0 February 12, Google redesigns Flow February 25, three major launches within days of each other, more happened in six weeks than all of Q3 and Q4 2025 combined

And like that's the part that's hard to explain to someone who's not watching this closely, it's not that any single model was shocking on its own, it's the pace, it's the fact that by the time you've processed one thing two more have happened, the cadence of major releases went from quarterly to monthly to basically weekly, the ground is moving faster than most people's ability to map it

I've been watching this space obsessively and even I had a moment this past month where I sat back and thought, I don't think I'm keeping up anymore. And I'm supposed to be keeping up.

generative media timeline

### The numbers nobody's putting in one place

Everyone talks about what looks cool on Twitter but the usage data paints a completely different picture and like nobody covers this

Veo 3.1 sitting at 96.4% market share on Vivideo, Sora 2 at 2%, orders went from 12000 in December to 62000 in January, 5x in one month, 205000 users across 220 countries, now this is one platform not the whole market but it's the most transparent data anyone has published and the picture is pretty clear

That is not a viral toy, that is adoption

And here's the thing, Sora dominates social conversation, Seedance dominates controversy, Veo quietly running the actual usage numbers, Kling and Runway dominate professional use, these are all winning in completely different ways in completely different arenas and if you're only watching one you're missing the rest

Also vertical video at 43.7% and climbing, landscape at 52.8%, square basically dead, people generating for specific platforms from the jump now, the workflow has inverted, you don't make a video and then optimize it for distribution, you decide where it's going first and build from there, that's a bigger shift than it sounds

AI Video Market Share

### What each model actually does

Not a ranking, rankings miss the point, each one of these is winning a different game and the mistake is treating them like they're competing for the same thing

**Sora 2**, best audio accuracy and instruction following right now, physics feel real, lip sync works, the Disney deal means 200 plus licensed characters which is wild to even type, but invite only, US and Canada only, basically a social app now people call SlopTok, Sora 2 Pro behind the 200 a month ChatGPT Pro wall, 25 seconds max

Pricing: ChatGPT Plus ($20/mo) gets you basic Sora 2 at 720p with 30 daily credits but honestly that's barely usable, Pro ($200/mo) unlocks 1080p and 10000 credits which sounds like a lot until you realize Pro burns 40 credits per second at 1080p vs 16 at 720p, so a 10 second 1080p clip is 400 credits, you're getting maybe 25 high res videos a month for 200 bucks, API pricing is $0.10/sec at 720p and $0.50/sec at full res so a 10 second polished clip through the API runs you about 5 dollars, that's expensive, and the free tier got suspended back in January so there's no zero cost option anymore

TODO(video-2): Add final hosted video URL for "Sora 2 output" (0:00 / 0:10).

TODO(video-2-notes): Poster extracted from source post. Replace with playable embed/video link once uploaded.

**Kling 3.0**, the directing tool, 6 camera cuts in one generation where you set shot size and camera movement per cut, native 4K 60fps, multi language audio with accents, 60 million creators 600 million videos total, if you want to actually direct something and not just generate a random clip this is it, and like the motion consistency thing is worth calling out specifically, Kling is winning that fight right now, objects stay coherent across cuts in a way that other models still struggle with, that's the actual reason filmmakers keep going back to it

Pricing: free tier with 66 daily credits for basic watermarked stuff, Standard at $10/mo with 660 credits, Pro at $37/mo with 3000 credits which gets you roughly 4 minutes of 1080p video, Premier at $92/mo with 8000 credits, most aggressive pricing in the space honestly, the Pro plan is the sweet spot for most people, real cost per usable clip works out to about 50 cents to $1.50 depending on how many tries it takes, API through third parties like [fal.ai](http://fal.ai/) runs about $0.90 per 10 second clip, with audio generation it doubles the credit cost

TODO(video-3): Add final hosted video URL for "Kling 3.0 multi shot demo" (0:00 / 0:10).

TODO(video-3-notes): Poster extracted from source post. Replace with playable embed/video link once uploaded.

**Veo 3.1**, nobody talks about it and they really should, best prompt adherence of any model, Ingredients to Video lets you feed 4 reference images for consistency across scenes which like changes everything about how you build longer content, Google merged ImageFX and Whisk into Flow so it's a full workspace now not just a generator, 1.5 billion images and videos in Flow, native 4K vertical support, the model itself is solid but the real thing is it just plugs into everything Google already has, that's hard to compete with

Pricing: confusing, Google AI Pro at $19.99/mo gives you about 1000 credits and roughly 90 Veo 3.1 Fast videos but the full quality model is limited, Ultra at $249.99/mo is the only way to unlock everything including 4K and priority processing, API through Vertex AI runs $0.50/sec for video only and $0.75/sec with audio which makes a single 8 second clip with sound cost 6 dollars, Veo 3.1 Fast is way cheaper at around $0.10 to $0.15/sec for testing, but the real cost trap is that every failed generation still costs you full price and you can't go past 8 seconds per generation so anything longer means paying twice, Google's education discount gives students free Pro for a year which is actually a great deal if you qualify

TODO(video-4): Add final hosted video URL for "Veo 3.1 demo" (0:08 / 0:08).

TODO(video-4-notes): Poster extracted from source post. Replace with playable embed/video link once uploaded.

**Seedance 2.0**, probably the highest raw quality out of anything right now, multimodal inputs, but legally on fire, China only through Jianying, features getting pulled back, CapCut timeline who knows, quality is there everything else is a mess

Pricing: the cheapest serious model by far, Jimeng membership starts at 69 RMB which is about $9.60/mo, a standard 5 second clip costs roughly 3 RMB which is like 42 cents, and the generation success rate is reportedly over 90% so you're not wasting credits on failed attempts like with other platforms, Dreamina international pricing runs $18 to $84/mo depending on tier, API not publicly available yet but projected at $0.10 to $0.80 per minute depending on resolution, there's also a free trial on Xiaoyunque where generations literally don't deduct points right now which is wild, the catch is access, you need a Chinese phone number and payment method for the good tiers, international access is still gated and the global API rollout got delayed indefinitely because of the copyright drama

**Runway Gen 4.5**, the editing ecosystem, Aleph lets you edit inside generated video without regenerating the whole thing which is like a genuinely different capability, Act Two for motion capture, number 1 benchmark at 1247 Elo, Gen 4 Turbo does 10 seconds in about 30 seconds, and honestly the thing nobody says about Runway is that this is the one indie filmmakers are actually building around, like with Aleph a solo creator can do stuff that used to need a whole post production team, the editing workflow is the moat not the generation quality

Pricing: Standard at $12/mo gets you 625 credits which is about 25 seconds of Gen 4.5 video, that's it, 25 seconds for twelve bucks, Pro at $28/mo bumps to 2250 credits so roughly 90 seconds, Unlimited at $76/mo gives you 2250 fast credits plus unlimited relaxed rate generations but relaxed means slow queue, Gen 4.5 costs 25 credits per second which is the most expensive per second rate of any model here, Gen 4 is 12 credits/sec and Turbo is 5 credits/sec so the smart move is prototyping on Turbo then doing finals on 4.5, API is $0.01 per credit so a 10 second Gen 4.5 clip through the API is $2.50, credits don't roll over which is annoying, you use them or lose them each month

TODO(video-5): Add final hosted video URL for "Aleph demo" (0:02 / 1:07).

TODO(video-5-notes): Poster extracted from source post. Replace with playable embed/video link once uploaded.

**Pika 2.5**, went a completely different direction, PikaSwaps PikAffects PikaFormance, 8 bucks a month, not trying to be cinema, competing on fun and TikTok native content, different game entirely, free tier with 150 monthly credits, Pro at $8/mo with 700 credits, Unlimited at $58/mo, honestly the most accessible pricing in the space

**Luma Ray3**, best physics right now, 4K HDR, 25 million users, good at the stuff that's hard to fake like dust settling and fabric moving and objects actually interacting with gravity

Pricing: Lite at $9.99/mo with 3200 credits, Plus at $23.99/mo with 10000 credits, Unlimited at $75.99/mo with 10000 fast credits plus unlimited relaxed, the credit math gets complicated though, a 10 second Ray3 720p clip costs 640 credits and 1080p costs 660, HDR cranks it way up to 2560 credits for 10 seconds at 720p, so on the Plus plan you're getting maybe 15 standard clips or like 4 HDR clips a month, API at $0.32 per million pixels through Amazon Bedrock, audio generation is free when available but Ray3 still doesn't have it which is kind of the big miss

**Open source**, honestly this might be the most important part and nobody talks about it enough, Wan 2.6 at like 5 cents a second via API, LTX 2 doing 4K 20 second video on local GPUs with built in audio and only needs 12GB VRAM so basically any decent gaming PC, Nvidia announced 3x faster gen and 60% less VRAM on RTX 50 series with TensorRT, HunyuanVideo from Tencent doing solid image to video, you can make broadcast quality AI video on a gaming PC now, that matters way more than any single model launch

Pricing: this is where it gets interesting, Wan 2.6 through API services runs $0.05 per second which makes a 10 second clip 50 cents, but if you're running locally the cost is literally just electricity, LTX 2 on a consumer GPU means after the hardware investment your marginal cost per video is basically zero, that's the real disruption, not which cloud model has better hair physics, and with RTX 50 series optimization making generation 3x faster the time cost is dropping too

cost comparison

### The money thing

Ok so here's the part that nobody puts in one place, what does this stuff actually cost to use, because the answer ranges from literally free to genuinely expensive and the pricing models are all over the place

What each model actually costs

Model Monthly Price Cost per 10s Clip Free Option LTX 2 free (own gpu) free yes, fully open source Wan 2.6 free local / $0.05/sec api ~$0.50 yes, open source Pika 2.5 $8 - $76/mo ~$0.20 80 credits/mo (watermark) Seedance 2.0 ~$9.60 - $84/mo ~$0.84 - $1.90 260 credits on signup Kling 3.0 $10 - $180/mo ~$1.00 - $1.50 66 daily credits (watermark) Runway 4.5 $12 - $76/mo ~$1.20 - $2.40 125 one-time credits Luma Ray3 $8 - $76/mo ~$1.50 - $3.00 limited daily gens Sora 2 $20 - $200/mo ~$3.20 - $5.00 none, killed jan 2026 Veo 3.1 $20 - $250/mo ~$1.60 - $6.00 limited on free gemini

sorted cheapest to most expensive, prices as of feb 2026, clip costs assume decent quality not lowest settings

What should you actually get

What You Need Best Pick Cost just testing stuff out Pika free or Kling free tier $0 social content, short clips Pika $8/mo or Kling $10/mo $8-10/mo serious content creation Kling Pro $37/mo or Runway Pro $28/mo $28-37/mo heavy production use Runway Unlimited or Kling Ultra $76-180/mo money is not the issue Sora Pro or Veo Ultra $200-250/mo you have a gpu at home LTX 2 or Wan 2.6 free

The stuff the table doesn't tell you

The retry tax, you rarely get a usable clip on the first try especially for anything with humans or complex motion, so that 50 cent clip is really more like $1.50 to $3 when you factor in the 3 to 5 attempts it takes, Seedance claims 90% success rate which would be massive but most models you're looking at maybe 20 to 30% of generations being actually usable

The credit confusion, every single platform uses a different system designed to make comparison impossible, Kling advertises "150 videos" on Pro but with professional mode and audio it's more like 20 to 40, Runway Standard's 625 credits is literally 25 seconds of Gen 4.5, Luma's credit costs double for HDR, Sora locks 1080p behind a $200 wall, it's like they all got together and agreed to make this as hard as possible

The resolution trap, most pricing you see quoted is at 720p but then it's 2x to 4x more for 1080p and forget about 4K, Veo charges 50% more when you want audio, Kling doubles credit cost for audio, nobody mentions this upfront

The real story though is the China vs US gap, Seedance at $9.60/mo vs Sora Pro at $200/mo, that's 20x, Kling Pro at $37/mo gives you roughly 3x more output than Runway Pro at $28/mo, the Chinese models are just cheaper and in most cases the quality matches or beats, which is probably why there's so much pressure to block them through legal channels rather than competing on price

And open source is the actual disruptor, when Wan 2.6 costs 5 cents a second and LTX 2 runs free on a gaming PC, the whole subscription model starts looking shaky, the cost of a single month of Sora Pro ($200) gets you 400 ten second videos through Wan API, that math changes everything, give it 6 months and paying $200/mo for Sora is gonna look like paying for AOL in 2005

### What's still broken

For all the progress every model still gets stuff wrong and nobody really calls it out

Like there's maybe three big aesthetic problems that all of them share right now, first is the sameness, a lot of these models default to this very specific look, very saturated very smooth very HDR, technically impressive but it all kind of looks the same, getting something that actually looks like it was shot on film or has a specific visual identity is still genuinely hard, second is the uncanny motion thing, humans move in ways that are slightly imperfect and these models make everything too smooth too clean too perfect, third is lighting, indoor scenes especially just look wrong, the way light bounces off surfaces and creates shadows is still off in a way that's hard to describe but you notice immediately

Physics are better but not solved, like two people handing each other something or pouring liquid into a glass, it looks close but something is always off, complex multi object interactions just fall apart still

Hands and fingers way better than a year ago but still not reliable, same with text in video, if your scene needs readable text on a sign or screen you're still gonna have issues with that

Temporal consistency over longer clips still the big one, anything past like 10 seconds and you start seeing drift, textures shift, proportions change slightly, subtle but it's there, and it compounds the longer the clip runs

These aren't minor things, for anyone trying to build a real production pipeline around this stuff these are the things that eat your time and your budget, the benchmark scores don't capture it, the Twitter clips don't show it, you have to actually try to use these things for real work before you feel it

### The workflow thing nobody talks about

This is something that gets missed in like every AI video conversation, the output quality doesn't matter as much as the workflow around it

Like you can have the best looking model in the world but if it takes 47 steps to get from prompt to final export nobody's using it for real work, Runway gets this, that's why Aleph and the editing pipeline matter more than benchmark scores, Kling gets this with the multi shot storyboarding, Veo gets this by just plugging into everything Google already has

And documentation is a huge part of this, some of these tools have incredible capability but the docs are so bad you'd never figure it out on your own, or there's features buried three menus deep that completely change how you'd use the tool, at this point docs and workflow matter more than output quality honestly, because the quality gap between models keeps shrinking but the gap in how easy they are to actually use is still massive

Long term the models that stick around are the ones you can actually build a repeatable process around, not the ones with the best looking single frame on Twitter

Workflow comparison

### Copyright in the age of AI video

Not trying to pick a side here just laying out what's happening

Disney sues ByteDance over Seedance, same quarter gives OpenAI a billion for Sora to use Mickey Mouse and Marvel and Star Wars, Google got a cease and desist too but Veo keeps running, the pattern is pretty obvious, if you're American and you pay you get IP access, if you're Chinese and you don't you get sued, like at some point you gotta call that what it is, that's geopolitics not copyright

And like who's actually at fault, users made the Spider Man clips not ByteDance, same thing happened when Sora 2 launched, people made copyrighted stuff immediately, Sora got a partnership Seedance got a lawsuit, platform vs user liability completely unresolved

The consent stuff is separate and honestly scarier, Seedance could generate someone's realistic voice from just their photo, that got rolled back but the tech exists now, SAG AFTRA's concern isn't just Disney characters it's likeness and voice rights for real people, deepfake celebrity stuff already circulating, Robin Williams and George Carlin estates publicly asked OpenAI to restrict deepfakes of their loved ones, this isn't about corporate IP anymore, this is about whether your face and voice belong to you

And there's a whole training data angle nobody really gets into, like these models are trained on massive amounts of video content and the question of whose content and from where matters a lot, Indian cinema for example is one of the largest film industries in the world, Bollywood, regional language films, decades of visual tradition, and if that data is being used to train these models without consent or compensation that's a whole different problem, it's not just about the outputs it's about what went in, and different film traditions have different visual languages that could genuinely change how these models handle cinematography if the training was done right and with actual consent

Regulation is weird too, China actually has stricter AI labeling laws than the US right now, penalized 13000 accounts, removed hundreds of thousands of posts, RedNote restricted unlabeled AI content, the US has basically nothing enforceable for AI generated video, the country everyone points fingers at is the one doing more about it

Disney OpenAI deal side by side with Disney ByteDance cease and desist

# Where this leaves us

No single model wins, that's the honest answer, it's a routing game now, you pick the right one for the shot and move on, Kling for directed multi shot sequences, Runway for anything you need to edit after generation, Veo for prompt adherence and integration, Sora when audio sync is everything, open source when the budget is the constraint and you have a GPU at home

The barrier to making good looking video content is basically zero now, what that changes is not whether content gets made, it's who gets to make it, the kid with a gaming PC in Hyderabad has access to the same fundamental capability as a studio in Los Angeles, that's not a small thing

Copyright gets messier before it gets cleaner, vertical probably overtakes landscape by mid 2026, and honestly the workflow and docs thing is gonna matter more than raw quality going forward, because the quality gap between models keeps shrinking but the usability gap is not

The thing I keep coming back to is this, I organized the Mumbai AI Film Festival four months ago, 25 million views, national headlines, 80% of participants landing positions at companies like Netflix India and Eros Now, that happened when the tools were significantly less capable than what dropped last week, the Seedance 2.0 launch week alone probably generated more AI video content than existed in total a year ago

Whatever we write here is outdated in like 6 weeks, that's where things are

The people who come out of this well aren't gonna be the ones who picked the right model, they're gonna be the ones who stayed curious enough to keep routing, kept building repeatable processes instead of chasing the best single output, and understood early that this is no longer a conversation about whether AI video is good enough, it is good enough, the conversation is about what you do with it now that anyone can make anything

That window where being early gives you an advantage is still open, not for much longer

If this helped you make sense of what's happening, share it with someone building in this space, most people are still treating AI video as a thing to watch from a distance, it stopped being that a while ago

by Hardeep and Sanchay.
