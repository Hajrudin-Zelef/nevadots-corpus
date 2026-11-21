---
id: collect-261121-veille-ia/modeles-open/flux-3-black-forest-labs-goes-multimodal-frontier
title: "FLUX 3: Black Forest Labs Goes Multimodal Frontier"
domain: modeles-open
role: reference
task: article
actors: []
dates: ["2026-07-23"]
keywords: ["flux", "video", "multimodal", "model", "image", "access", "black", "forest"]
source: https://www.digitalapplied.com/blog/flux-3-black-forest-labs-multimodal-launch
source_anchor: ""
source_lines: [1, 155]
section: "Modeles_Open"
---
FLUX 3 is Black Forest Labs' first unified multimodal frontier model — announced July 23, 2026 from Freiburg, Germany, and the first time the company behind the FLUX image line has shipped video at all. One architecture, jointly trained across image, video, audio, and action prediction, replaces the separate-models-behind-one-interface pattern the rest of the field mostly still follows.

The launch matters beyond the spec sheet. BFL built its reputation on image generation — and on open weights. FLUX 3 stakes the company's next act on a bigger claim: that video generation, simulation, computer use, and robotics are connected applications of one "visual intelligence" capability, not separate product categories. The launch bundle backs that up with FLUX-mimic, a video-action robotics model already being tested by Audi on production manufacturing tasks.

This guide covers what actually shipped versus what was merely announced, the vendor-measured preference evals and the caveats BFL itself attaches to them, the four-tier rollout with access and pricing status in one table, the robotics angle, and what agencies and engineering teams should do while access is still gated. Every number below traces to BFL's announcement or same-day independent coverage.

1. 01BFL's first video model — and first unified multimodal architecture.FLUX 3 is jointly trained across image, video, audio, and action within one architecture built on BFL's Self-Flow approach — not separate models stitched behind a shared interface. It is the company's first-ever video generation model after the image-only FLUX.1 and FLUX.2 lines.
2. 02Video ships with native audio, up to 20 seconds per generation.FLUX 3 Video generates clips up to 20 seconds in a single pass with synchronized dialogue, sound effects, and ambient audio. It supports text-to-video, image-to-video, video-to-video, and keyframe-to-video, with multi-shot sequences chained agentically.
3. 03Access is gated and pricing does not exist yet.Only FLUX 3 Video and FLUX 3 Action entered early access on launch day — application required, BFL approves each applicant. There is no public API from BFL or partners, and no public pricing has been announced for any FLUX 3 tier as of July 24, 2026.
4. 04The benchmark wins are vendor-measured and preliminary.BFL's own preference testing claims 93% wins over Luma Ray 3.2 and 77% over Runway Gen-4.5 — but BFL labels the numbers a preliminary evaluation of an early FLUX 3 candidate, run on 10-second 720p clips. Against Seedance 2.0 and Gemini Omni Flash, the result is a 52% coin flip.
5. 05Open weights arrive last, and the backbone points at robotics.FLUX 3 Dev — the open-weight release of the full multimodal backbone — is explicitly the final tier in the rollout, due later in 2026. Meanwhile FLUX-mimic, built with Zurich's mimic robotics, is already being tested by Audi on soft-body manufacturing tasks.

## 01 — What ShippedFour product lines, *two* actually accessible.

Black Forest Labs announced FLUX 3 on July 23, 2026. It is the company's first-ever video generation model — until now, BFL had shipped image-only models, from the original FLUX.1 through the FLUX.2 line we covered in our FLUX.2 Max guide in December 2025. FLUX 3 will ship across four product lines: FLUX 3 Video, FLUX 3 Image, FLUX 3 Action, and FLUX 3 Dev.

The availability picture is narrower than the announcement's breadth suggests. FLUX 3 Video and FLUX 3 Action entered a gated "Early Access" program on launch day — anyone can apply, but BFL must approve each applicant. FLUX 3 Image is described as coming "in the coming weeks," followed by general availability. FLUX 3 Dev — the open-weight tier — is planned for later in 2026, last in the rollout sequence. And there is no public API access for FLUX 3 yet, from BFL or from partners.

##### FLUX 3 *Video*

BFL's first video model. Single generations up to 20 seconds with synchronized dialogue, sound effects, and ambient audio. Text-, image-, video-, and keyframe-to-video modes; multi-shot via agentic chaining.

##### FLUX 3 Action

Built, per BFL, for creative tooling, media, design, e-commerce, and physical AI — a stated use-case span far beyond BFL's historical image-generation customer base.

##### FLUX 3 Image

The successor to the FLUX.2 image line. Described as arriving in the coming weeks, followed by general availability. No image-model benchmark numbers were disclosed at launch.

##### FLUX 3 Dev

Planned open-weight release covering video, audio, image, and action prediction — a broader scope than any prior FLUX Dev release. Explicitly the last tier to ship.

*approval-gated*application. BFL has also not disclosed production SLA commitments, full evaluation methodology, sample sizes, rater counts, or any image-model benchmarks — and no pricing exists for any tier.

## 02 — ArchitectureOne model, *four* modalities.

The architectural bet is the real story. FLUX 3 is built on "Self-Flow," BFL's approach for aligning multimodal generation and understanding within one architecture. Rather than training an image model, a video model, and an audio model and wiring them together behind an interface, BFL trained one model jointly across all of those signals — extendable to action prediction for robotics.

CEO Robin Rombach's framing in the launch announcement is that each signal teaches something the others cannot: images convey structure, video teaches dynamics, audio carries timing and physical events, and actions reveal causal relationships. "Joint training within one unified architecture is what will get us there, because each training modality strengthens the others," he said. The announcement explicitly frames "visual intelligence" as spanning creative generation, simulation, computer use, and robotics — connected applications of one capability, not separate product categories.

“You can’t cheat reality. A model that only learns images can only generate images. But the world is not made of still frames. It moves, sounds, changes, and responds. That’s why FLUX 3 is trained across those signals together, so the model can build a deeper understanding of how the world works. That is what visual intelligence requires, whether the application is video generation, simulation, or robotics.”— Robin Rombach, Co-Founder and CEO, Black Forest Labs

Whether joint training delivers on that thesis is exactly what no outside party can verify yet — there are no independent evaluations, and the model is behind an application gate. But the direction matches where the frontier has been heading: unified multimodal architectures over tool federations. What makes BFL's version distinctive is the action-prediction extension, which is where the robotics story in Section 06 comes from.

## 03 — FLUX 3 VideoTwenty seconds, *native* audio.

FLUX 3 Video generates clips up to 20 seconds in a single generation, with native, synchronized audio — dialogue, sound effects, and ambient noise. That combination puts it in the same conversation as the audio-native leaders we mapped in our Gemini Omni guide, and ahead of the many models that still bolt audio on as a separate pass.

The mode coverage is broad for a first video release: text-to-video, image-to-video, video-to-video, and keyframe-to-video, plus multilingual dialogue and typography. Multi-shot sequences are handled via "agentic chaining" rather than a single extended generation — the model strings shots together rather than generating a minutes-long clip in one pass. One caveat worth carrying into any evaluation: BFL's preference evals were conducted on 10-second, 720p text-to-video clips with audio — shorter and lower-resolution than the 20-second max-generation spec, so the published eval scores don't reflect the longest clips the model can produce.

##### Max clip length

With native synchronized audio — dialogue, sound effects, ambient noise. Multi-shot sequences are chained agentically rather than generated in one extended pass.

##### Ways in

Text-to-video, image-to-video, video-to-video, and keyframe-to-video — plus multilingual dialogue and typography support inside generations.

##### Already testing

Canva, Burda, Magnific (formerly Freepik), Krea, and Picsart are testing FLUX 3, per BFL. FLUX models already power generative features inside Adobe Photoshop, Picsart, and Nous Research's Hermes Agent.

## 04 — BenchmarksThe preference evals, *honestly*.

Every number in this section is vendor-measured: BFL ran the preference tests itself, and it labels the results a "preliminary evaluation of an early FLUX 3 candidate" — a pre-release checkpoint, not necessarily the exact model now entering early access. BFL states that no independent tests are available yet. With that firmly on the table, here is what the company's own chart claims, and — more usefully — how much each comparison actually matters in the market we mapped in the AI video market after Sora.

The asymmetry is the story. The loudest numbers — 93% versus Luma Ray 3.2, 77% versus Runway Gen-4.5 — come against the softer end of the field. The hardest comparisons land at 52%: statistical coin flips against ByteDance's Seedance 2.0 and Google's Gemini Omni Flash. VentureBeat's analysis sharpened both edges: the Seedance 2.0 result is "a statistical coin flip against a model most Western enterprises cannot currently procure" — ByteDance indefinitely postponed Seedance 2.0's international rollout after Netflix, Warner Bros., Disney, Paramount, and Sony sent legal threats over alleged copyright infringement — while the Gemini Omni Flash comparison is the one that "matters much more," because it is an already-available Google API.

| FLUX 3 preference-eval scorecard: BFL-measured win rates against eight competitor video models, each competitor's availability to Western enterprises as of July 2026, and our read on how much each comparison matters. |  |  |  | 
|---|---|---|---|
| Competitor | BFL-measured preference | Availability (Jul 2026) | Our read | 
|---|---|---|---|
| Luma Ray 3.2 | 93% | Not an already-available API, per VentureBeat | The headline number — and the softest comparison on the list | 
| Runway Gen-4.5 | 77% | Generally available | A strong claimed win over an established incumbent — still vendor-run | 
| Grok Imagine Video | 69% | Generally available | Comfortable claimed margin | 
| Kling v3 Pro | 60% | Generally available | Narrower edge over a commercial volume leader | 
| Happy Horse v1 | 59% | Limited public data | Modest claimed edge | 
| Happy Horse 1.1 | 57% | Limited public data | Modest claimed edge | 
| Seedance 2.0 | 52% | Not procurable in Western markets — international rollout postponed amid studio legal threats | A coin flip against a model most enterprises can't buy anyway | 
| Gemini Omni Flash | 52% | Generally available — live Google API | The comparison that matters most — and it's a wash | 

Preference percentages: Black Forest Labs' own testing as reported by VentureBeat, Jul 23, 2026 — vendor-measured, preliminary, run on 10s / 720p clips from an early FLUX 3 candidate. Availability and read columns: Digital Applied analysis.

Two footnotes matter for anyone building a vendor shortlist. First, the Seedance comparison is against Seedance 2.0 — ByteDance has since moved its line forward, as we covered in our guide to ByteDance's Seedance 2.5, so even that coin flip is against a superseded checkpoint. Second, the Gemini Omni Flash on the other side of the 52% result is the fast, widely deployed tier we profiled in our Gemini Omni Flash breakdown — available today through a Google API while FLUX 3 sits behind an application form. For a full side-by-side of how FLUX 3 slots into the current video field, see our companion piece, FLUX 3 vs Seedance 2.5 vs Gemini Omni.

## 05 — AvailabilityThe rollout matrix — every tier, *one* glance.

Launch coverage scattered the four tiers across paragraphs. Here is the consolidated view an agency or engineering team actually needs before deciding whether to apply for early access — modality, status, access path, and pricing, as of July 24, 2026.

| FLUX 3 rollout and availability matrix: the four product tiers with modality, status as of July 24 2026, access method, and pricing status. |  |  |  |  | 
|---|---|---|---|---|
| Tier | Modality | Status · Jul 24, 2026 | How to access | Pricing | 
|---|---|---|---|---|
| FLUX 3 Video | Text / image / video / keyframe → video with native audio, up to 20s | Gated early access since Jul 23 | Application via BFL; each applicant approved individually | Not announced | 
| FLUX 3 Action | Video-action — creative tooling, media, design, e-commerce, physical AI | Gated early access since Jul 23 | Application via BFL; each applicant approved individually | Not announced | 
| FLUX 3 Image | Next-generation image tier | Not shipped — "coming weeks," then general availability | Not yet available | Not announced | 
| FLUX 3 Dev | Open-weight release of the full multimodal backbone — video, audio, image, action | Planned — last in the rollout, later in 2026 | Weights download when released | Not announced · license TBD | 

Compiled by Digital Applied from BFL's July 23, 2026 announcement and VentureBeat's launch coverage. No public API exists for any tier — from BFL or partners — as of July 24, 2026.

The sequencing deserves a plain reading. BFL built extraordinary brand equity with developers precisely because open-weight Dev releases historically arrived close to its flagship launches. With FLUX 3, the open-weight tier ships *last* — later in 2026, behind two gated tiers and a closed image release. VentureBeat notes the staged, limited rollout mirrors recent release patterns from Anthropic and OpenAI — though those companies tied their phasing to stated security reasons, and BFL has not framed its staging the same way. For the audience that adopted FLUX specifically because of open weights, that is a real change in posture, even if the Dev release ultimately delivers a broader open backbone than any prior FLUX drop.

## 06 — Physical AIFLUX-mimic: the backbone reaches for *robots*.

The launch's most surprising bundle is FLUX-mimic — a video-action model built on the FLUX 3 backbone, developed jointly with Zurich-based mimic robotics for general-purpose robotic manipulation. Bloomberg framed the launch as BFL's move into "physical AI" — its first robotics-oriented model. This is the action-prediction modality of the joint-training thesis made concrete: a model that has learned how scenes move and sound, applied to predicting what a robot arm should do next.

It is not just a demo. Audi is testing FLUX-mimic on production manufacturing tasks, including fitting flexible door seals — soft-body manipulation work that conventional automation has struggled with. Audi's Christoph Schneider says the robots now "solve complex soft-body manipulation work" that older machines could not handle. BFL states the FLUX-mimic system reacts in roughly 101 milliseconds — a company-stated figure it describes as in the neighborhood of human visual reflexes.

“Audi represents the kind of manufacturing partner we built FLUX-mimic for.”— Stephan-Daniel Gravert, Co-Founder, mimic robotics

The strategic significance is larger than the Audi pilot. If video-generation backbones genuinely transfer to manipulation, every frontier video lab is suddenly a latent robotics company — and BFL just planted its flag first among the independent ones. We unpack the robotics thread in full in our companion piece, FLUX 3 Action and the physical-AI turn.

## 07 — Our AnalysisThe *European* independence read.

Here is our editorial read — not a claim BFL or the launch press made: FLUX 3 lands at a moment when Black Forest Labs is arguably the last major independent European AI lab operating at frontier scale in its domain. The comparison point is concrete. Heidelberg-based Aleph Alpha — once Germany's flagship LLM hope — has been merging into Canada's Cohere since April 2026, a deal that leaves Aleph Alpha's existing shareholders with only around 10% of the combined entity. BFL, by contrast, closed 2025 with a $300M Series B led by AMP and Salesforce Ventures at a $3.25B post-money valuation, bringing total funds raised past $450M — and remains independent.

The company's trajectory makes the independence more notable, not less. Founded in August 2024 by researchers who worked on the original Stable Diffusion at Stability AI, BFL put FLUX 1.1 Pro at the top of the Artificial Analysis image arena by October 2025, shipped FLUX.2 in November 2025, counts Martin Scorsese as a company advisor — and also lost the "best open-source image generator" crown to Alibaba's Z-Image Turbo in late 2025, which matched FLUX's open-source quality on cheaper consumer hardware. FLUX 3 is the answer to that squeeze: change the game from image quality, where the field caught up, to unified multimodality, where almost nobody independent competes.

*structurally*different bet than a US or Chinese platform — on data residency, on regulatory alignment, and on the risk that a vendor's roadmap gets absorbed into someone else's. That said, treat this as our analysis: BFL itself makes no independence claims, and independence is only worth a premium if the model clears your quality bar in your own evals.

## 08 — PlaybookWhat to do *now* — by team type.

With no pricing, no public API, and no independent benchmarks, FLUX 3 is not a production decision yet — it is a pipeline decision. The right move depends on which seat you're in.

##### Apply for early access now

The application is open to anyone, approval-gated by BFL. Native 20-second audio-synced generation is worth evaluating on your own briefs — but treat it as a pilot: no pricing or SLA exists to build a client cost model on.

##### Stay on FLUX.2 / current stack

FLUX 3 Image is weeks away with zero disclosed benchmarks. There is nothing to migrate to yet — keep your current image pipeline and re-evaluate at the Image tier's general availability.

##### Wait for *Dev*, verify the license

The open-weight backbone is promised for later in 2026, last in the rollout. Don't re-platform on a promise — and when Dev ships, read the license before assuming FLUX.1-era permissiveness carries over.

##### Treat vendor evals as directional

BFL's own numbers say the fight that matters — versus an available Google API — is a coin flip. Run head-to-head evals on your own prompts when access lands, and price against the incumbents you can buy today.

The connective thread: every one of these positions depends on running your own evaluations rather than inheriting a vendor's chart. That is the discipline we bring to our AI transformation engagements — comparative evals on your actual briefs, cost modeling once pricing exists, and a routing decision per workload rather than per headline. And if the goal is publishing video at volume once a model clears the bar, that slots into the production system we run as our content engine service.

Looking forward, two things will settle FLUX 3's real position within months. First, independent evals on the shipping model — not the early candidate — will either confirm or deflate the 77–93% headlines. Second, pricing. BFL's FLUX.2 Max precedent ($70 per 1,000 text-to-image generations at launch) tells us the company prices at the premium end of its category; if FLUX 3 Video lands similarly premium against a Gemini Omni Flash that BFL's own testing scores as a 52% coin flip, the availability and price gap — not the preference chart — will decide adoption.

## 09 — ConclusionA frontier bet, *gated* at the door.

### FLUX 3 is a genuine architectural bet — behind an application form.

Black Forest Labs shipped the most ambitious release in its two-year history: one architecture jointly trained on image, video, audio, and action, a first video model with *native* 20-second audio-synced generation, and a robotics extension already on an Audi factory floor. The multimodal thesis is coherent and the partner list is real.

What it did not ship is just as defining: no public API, no pricing, no independent benchmarks, and open weights explicitly last in line. The vendor-measured eval chart reads strongest against the softest competitors and lands at a coin flip against the one rival you can actually buy through an API today. None of that makes FLUX 3 weak — it makes it unproven, by BFL's own preliminary-evaluation framing.

The projection we're comfortable making: FLUX 3's significance won't be decided by the preference chart but by two later events — the open-weight Dev release, which will either restore BFL's developer covenant or mark its end, and the first independent head-to-heads against Gemini Omni Flash and the current Seedance line. Until then, apply for access, run your own briefs, and keep your production pipeline on models with a price tag.
