---
id: collect-261121-veille-ia/claude-anthropic/p-happycapy-fable-5-1-is-here-and-it-builds-small-worlds-95ef8100
title: "P happycapy fable 5 1 is here and it builds small worlds"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["fable", "model", "right", "built", "bird", "here", "builds", "small"]
source: ""
source_anchor: ""
source_lines: [1, 42]
section: "Claude_Anthropic"
---
Fable 5.1 is here, and it builds small worlds
Fable 5.1 is here — and it builds small worlds
Anthropic’s newest model just went live in the Happycapy model picker. The benchmark story is that it more than doubled its predecessor on frontier science. The better story is what it does with a free afternoon.
Fable 5.1, Anthropic’s newest model, went live on September 1, 2026, and it’s in Happycapy today. It’s the latest model in the Claude 5 family and part of the Mythos-class tier — tuned for long agentic work, the kind where a model has to plan, reach for the right tool, run code, check its own output, and stay coherent across a task far too long to hold in your head.
The headline number is the one every launch post will lead with: on Terminal-Bench-Science, a benchmark for autonomous scientific computing, Fable 5.1 scores 52.6% — more than double Fable 5’s 24.7%, and comfortably ahead of Opus 5’s 29.0%. It ships with a 1-million-token context window, up to 128K tokens of output, and pricing that finally makes long agent runs feel cheap: cache reads up to 75% cheaper, and agentic workloads up to 45% cheaper end to end.
That’s the serious version of the story, and it’s true. Here’s the one we’d rather tell. One afternoon, we stopped handing Fable 5.1 science problems and started asking it to make things — beautiful, faintly useless, entirely delightful things. It built three. Watch first; we’ll walk through them underneath.
The whole pitch, in one line
Anthropic built it to solve frontier science. We asked it to shake a snow globe. Turns out that’s the same skill.
Before the tour, the claim worth testing: the thing that lets a model top a science benchmark — holding a hard, multi-step goal in mind, using the right tool at the right moment, and getting the last five percent exactly right — is the same thing that separates a web page from a world. You’ll see it three times below. It always hides in the detail nobody asked for.
World One · A museum that happens to be a bird
Aviary Studio — a 3D Rainbow Lorikeet you can rotate, with a species list, an anatomy panel, and a range map, built with Tripo
Aviary Studio — a rotatable, wireframe-able Rainbow Lorikeet, generated with Tripo and wired in mid-build.
Ask most models for “a 3D bird viewer” and you get a bird you can spin. Fable 5.1 built Aviary Studio: a rotatable Rainbow Lorikeet — solid, translucent, or wireframe — generated with Tripo and wired in as a tool while it worked. Then it built the museum around the bird. A species shelf down the left: Gouldian Finch, Wilson’s Bird-of-Paradise, Great Blue Turaco. An anatomy panel down the right: brush tongue, hooked bill, zygodactyl feet. Body length in centimetres, weight in grams, IUCN status: Least Concern. A little map of exactly where the lorikeet lives.
And then the field notes, which no prompt on earth asked for: “A parrot that gave up seeds for sugar.” That’s the tell. A model that only wanted to pass stops at the spinning bird. This one knew that a bird, to a curious person, is a doorway — and built the whole room behind it.
World Two · A website that forgot it was a website
Origins of Colour — a scroll-driven cinematic website; the Carmine chapter shows red cloth drying over cactus terraces
Origins of Colour, chapter IV: Carmine — “Dactylopius coccus · the highland terraces.”
This one is a scroll-driven film about where colour comes from. “Every colour worth having was once a place someone had to go.” You scroll and the screen doesn’t update so much as dissolve — indigo cloth steaming on bamboo racks, then a hillside of blood-red fabric drying over cactus terraces. The chapter is titled Carmine, and the caption knows exactly what carmine is: a red made not from a plant but from an insect the size of a grain of rice, raised on cactus and dried in the sun.
Its own words: “Seventy thousand of them for a pound of colour that once dressed cardinals and kings.” Getting the cochineal right is a fact-check. Making the page breathe between chapters is taste. Fable 5.1 did both without being asked for either. Most “cinematic websites” are a hero image and a parallax trick. This one is edited like a film.
World Three · The snow globe that knew about the spacebar
Winterholm — a Three.js voxel snow globe in WebGL, a snow-covered village under glass with a windmill and a frozen pond
Winterholm — a voxel village sealed under glass, rendered in Three.js. Drag to orbit, scroll to zoom, press Space to shake.
Winterholm is a voxel village sealed under glass: snow-dusted rooftops, a windmill, a frozen pond with skaters the size of a pixel. You can orbit it, zoom in, tune the snowfall, switch on an aurora, make the glass more reflective. All expected. But the detail that matters is the one you feel before you notice it — press the spacebar, and the globe shakes, and the snow lifts and settles.
A snow globe you can’t shake is a diorama. Fable 5.1 built the one interaction that turns a model of a snow globe into a snow globe. Ask yourself which line of that prompt said “and make the spacebar shake it.” None of them did. It’s the same instinct that added the range map and checked the cochineal: knowing what the thing is for, and finishing it there.
Under the hood · What actually changed: the numbers
The playful builds are downstream of some unglamorous gains. Here is what Anthropic shipped, side by side.
| Metric | Fable 5.1 | For comparison | 
|---|---|---|
| Terminal-Bench-Science (autonomous scientific computing) | 52.6% | Fable 5: 24.7% · Opus 5: 29.0% | 
| Context window | 1,000,000 tokens | hold a whole codebase at once | 
| Max output | 128,000 tokens | a full build in one pass | 
| Prompt-cache reads | up to 75% cheaper | vs. standard input | 
| Agentic workloads | up to 45% cheaper | end to end | 
Source: Anthropic, Fable 5.1 launch (September 1, 2026). Benchmark figures as reported by Anthropic.
Translated for the three worlds above: a million-token window means it can hold an entire design system, a long conversation, and every file it has touched at once without losing the thread. Cheaper cache and agent runs mean a build like Winterholm — dozens of iterations, tool calls, and self-corrections — now costs a fraction of what the same run cost a version ago. The benchmark jump and the snow globe are the same story, told twice.
Try it · It’s already in your picker
Fable 5.1 is live in the Happycapy model picker now — next to Fable 5, Opus 5, Grok 4.6, DeepSeek V4, GPT and Gemini. One subscription, no API keys, and you can switch to it mid-task whenever a job gets heavy. The fastest way to see the tour for yourself is to steal the prompts: ask it for “a 3D studio for hummingbirds,” or “a scroll-driven site about the history of a single colour,” or “a voxel snow globe I can shake with the spacebar.” Then watch it build the part you didn’t ask for.
Conclusion: Fable 5.1 is Anthropic’s newest model, released September 1, 2026 and available now in the Happycapy model picker. It scores 52.6% on Terminal-Bench-Science — more than double its predecessor Fable 5 (24.7%) and ahead of Opus 5 (29.0%) — with a 1M-token context window, 128K max output, prompt-cache reads up to 75% cheaper, and agentic runs up to 45% cheaper. But the clearest way to understand the jump isn’t the benchmark; it’s the three things it built in one afternoon — Aviary Studio (a 3D bird studio built with Tripo), Origins of Colour (a scroll-driven cinematic site on the history of pigments), and Winterholm (a Three.js voxel snow globe you shake with the spacebar). The same precision that tops a science benchmark is what lets it finish the detail nobody asked for. Try Fable 5.1 in the Happycapy model picker — one subscription, 20+ frontier models, no API keys.
Meet Fable 5.1 — It’s already in your model picker — one subscription, twenty-plus frontier models, no API keys. Bring it something impossible and something beautiful. It’s oddly good at both.
Replies
Be the first to reply
Have a question or a thought to share? Add a comment above to start the conversation.
