---
id: collect-261121-veille-ia/actu-business/ai-model-releases-august-2026-tracker-and-dated-ledger
title: "AI Model Releases: August 2026 Tracker and Dated Ledger"
domain: actu-business
role: reference
task: article
actors: []
dates: ["2026-08-02"]
keywords: ["august", "model", "dated", "week", "release", "entries", "nine", "dates"]
source: https://www.digitalapplied.com/blog/ai-model-releases-august-2026-tracker
source_anchor: ""
source_lines: [1, 191]
section: "Actu_Business"
---
AI model releases in August 2026 arrived at a pace that makes a plain list useless: nine dated entries across five calendar dates in the first week of the month, from four model vendors, one image-and-video lab and one regulator. The value of a tracker is no longer the list. It is the column that says whether a thing shipped, was announced, or simply showed up as a price on a marketplace.

For current API prices, access limits, and workload selection, see our maintained frontier model guide.

That distinction did real work this week. OpenAI’s August 1 research drop is not a release — there is no release date, no pricing and no model card attached to it. Alibaba’s Qwen3.8-Max is the opposite: a full release you can call today, with the open weights still unpublished. Meta shipped a model and a coding harness on the same day. And the ChatGPT change everyone read as “unlimited ChatGPT” is a change to one tier, on one modality, that OpenAI itself describes in the future tense.

This is the August edition of our release tracker, built the same way as the May 2026 edition of this tracker. Below: a dated ledger of the nine entries with a status column and a link to our deep dive on each, then a countdown table of the scheduled entries already fixed for the rest of the month, then what to do with both.

1. 01Nine entries, five dates, one week.August 1 to 7 produced nine dated entries across five calendar dates and five vendors, plus one regulatory milestone. Three of the nine are not releases at all — one announcement, one marketplace listing and one alias flip of an existing model.
2. 02Announced is not shipped, and the gap is widening.OpenAI’s Astra drop carries no release date, no pricing and no model card. Qwen’s open weights were, at publication, promised for the week of August 10 with the license undisclosed — the Max weights then landed on August 8 under a bespoke licence, and the 27B sibling still has not. Both were reported as launches; only one became one.
3. 03The August 6 ChatGPT change is three changes.An updated GPT-5.6 Sol with a thinking slider went live for Plus and Pro. Luna was set to become the Free and Go default that week. Unlimited text chats and a Think button are, in OpenAI’s wording, due the following week.
4. 04Six dated deadlines land before month end.Qwen open weights and the ChatGPT unlimited-chats rollout in the week of August 10; the Imagen 4 endpoint shutdown on August 17; o3 retiring from ChatGPT on August 26; the DALL·E GPT retiring on August 30; Sonnet 5 promotional pricing ending August 31.
5. 05Regulatory dates now sit in the same calendar as model dates.General-purpose AI enforcement and the Article 5 and Article 50 obligations became applicable on August 2 under the EU’s Digital Omnibus regulation. The stand-alone Annex III high-risk obligations did not — those were deferred to December 2027.

## 01 — The LedgerNine entries, *five* dates, August 1 to 7.

Every row below is dated to the day and carries a status. Read the status column first — it is the difference between something you can call this afternoon and something a vendor has told you about. The final column links our deep dive on that entry, published the same week.

| The August 1 to 7, 2026 AI release ledger — nine dated entries from OpenAI, the European Union, Alibaba, Black Forest Labs, Meta and xAI, each with the surface it landed on, whether it is a release, an announcement, a marketplace listing or a regulatory milestone, and a link to our deep dive on it. |  |  |  |  | 
|---|---|---|---|---|
| Date | Vendor | What landed | Status | Our deep dive | 
|---|---|---|---|---|
| August 1–2 — a research drop, a regulation and a flagship |  |  |  |  | 
| August 1 | OpenAI | Astra research drop — ten previously-open problems in mathematics and theoretical computer science, a manuscript of roughly 249 pages, machine-checkable Lean 4 certificates published on GitHub | Announcement — no release date, pricing or model card | Ten proofs, zero product | 
| August 2 | European Union | General-purpose AI enforcement, the Article 5 prohibited-practice penalties and the Article 50 transparency obligations became applicable under Regulation (EU) 2026/1744 | Regulation applicable | Who enforces what | 
| August 2 | Alibaba (Qwen) | Qwen3.8-Max full release — 2.4T total parameters with 95B active in a mixture-of-experts layout, 1M context, text, image and video input | Released — closed API | 2.4T MoE, weights pending | 
| August 4–5 — video GA, a coding harness, and two speech surfaces |  |  |  |  | 
| August 4 | Black Forest Labs | FLUX 3 Video reached general availability on the BFL API and select partners — clips up to 20 seconds, native audio and dialogue, 720p native with 1080p via upscaling | Released — general availability | 20-second clips with audio | 
| August 5 | Meta | Muse Spark 1.2 — 1M context, context compaction, asynchronous and parallel tool calls, whole-repository training, co-trained with the Muse Code harness | Released | The launch, unpacked | 
| August 5 | Meta | Muse Code — a purpose-built multi-agent coding agent where every subagent spawn, tool call, steer and cancel is observable and replayable through an event log | Released as beta | Fan-out and the event log | 
| August 5 | OpenAI | GPT Transcribe listed on OpenRouter — recorded audio, streamed file transcription and committed Realtime turns, with free-form context, keyword hints and multi-language hints | Marketplace listing — no standalone vendor launch post located | Where it sits in STT | 
| August 5 | xAI | The `grok-voice-latest` alias flipped to Think Fast 2.0, on the date xAI gave when it announced the flip on July 29 | Released — alias flip, not a new model | Think Fast 2.0 in full | 
| August 6 — the ChatGPT defaults move |  |  |  |  | 
| August 6 | OpenAI | An updated GPT-5.6 Sol for Plus and Pro with a slider for how much thought a response gets; GPT-5.6 Luna set to become the default for Free and Go users “this week” | Sol update live; Luna default in rollout | The Luna default, in detail | 

Two notes on how the dates in that table were set. The Qwen3.8-Max row is dated August 2 because that is when the release became visible; Qwen’s own launch page carries an August 3 date, so both appear in circulation and either is defensible. The xAI row is dated August 5 for the alias flip itself — the announcement that it would happen came a week earlier, on July 29, which is exactly the kind of two-date event that gets collapsed into one in most coverage.

The count is also worth stating plainly, because a “nine releases in a week” headline would be wrong. Of the nine entries, five are releases in the ordinary sense, one is a regulatory milestone, one is an announcement with no product attached, one is a listing on a third-party marketplace, and one is an alias pointing at a model that already existed. That is a very different week from nine new models.

## 02 — Announced vs ShippedThree statuses, and why the *middle* one keeps getting missed.

The status column in the ledger is not editorial decoration. It maps to three genuinely different things a buyer can do with an entry, and the reason trackers blur them is that press coverage of all three uses the same verb. Here is the taxonomy this tracker runs on.

##### *Announcement*

A capability has been demonstrated and written up. Nothing is callable, nothing is priced, and no availability date has been given. Useful as a signal about where a lab’s research is heading; useless as a planning input.

##### Listing

The model appears on a marketplace with a rate, but no vendor launch post, model card or first-party price list has been located. You can call it. You cannot assume the marketplace rate is the vendor’s list rate.

##### Release

The vendor has published availability, a surface to call it on, and a rate. This is the only status you can build a budget or a migration plan against — and even here, check whether the rate is promotional.

OpenAI’s Astra drop is the clean example of status 01. The write-up covers ten previously-open problems in mathematics and theoretical computer science across a manuscript of roughly 249 pages, with machine-checkable Lean 4 certificates published on GitHub; the headline result is the first explicit construction of a non-sofic group, a question open since Gromov posed it in 1999. What it does not carry is a release date, a price, a model card, or any statement about availability in ChatGPT. Coverage that files it under “OpenAI releases Astra” is filing an announcement as a launch.

*$2,000*compute figure attached to Astra is the aggregate for all ten problems combined, not a per-problem cost. OpenAI’s own wording: “The total number of tokens needed to find solutions to these problems would cost roughly $2,000.” Per-problem readings of that sentence circulated in the days after the drop and inflate the implied cost of the work by an order of magnitude.

Status 02 is the one that costs teams money. A model that exists only as a marketplace listing has a rate that belongs to that marketplace, not to the vendor — and the two can differ in either direction. GPT Transcribe is this week’s instance: an OpenRouter rate with no vendor launch post or first-party price list to check it against. And the divergence is not confined to listings. FLUX 3 Video shipped as a full release with a first-party price list, and the surfaces still disagree: on OpenRouter the base text-to-video and image-to-video rates match the lab’s own published list exactly, while the video-continuation capability is listed a few percent below the lab’s rate for the same capability. Same model, same week, two surfaces, two answers. Our August pricing tracker carries the surface-by-surface detail.

## 03 — RegulationAugust 2 was a *narrower* date than the coverage suggests.

Regulation (EU) 2026/1744 — the Digital Omnibus on AI — took effect on July 27, 2026. On August 2, three things became applicable under it: enforcement against general-purpose AI model providers, the Article 5 penalties covering prohibited practices, and the Article 50 transparency obligations. Those are real, dated, and they apply now.

What did not become applicable on August 2 is the part most summaries lead with. The stand-alone high-risk obligations under Annex III were deferred from August 2, 2026 to December 2, 2027, and the Annex I embedded-product obligations were pushed to August 2028. If your compliance plan was built around an August 2026 high-risk deadline, the deadline moved — and the obligations that did land are a different set with a different scope.

*high-risk obligations became enforceable in August 2026*is wrong and should be amended. Annex III stand-alone high-risk obligations now apply from December 2, 2027; Annex I embedded-product obligations from August 2028. What activated on August 2, 2026 was general-purpose AI enforcement plus the Article 5 and Article 50 obligations.

The reason a regulatory date belongs in a model-release tracker at all is that it now behaves like one. It has a fixed date, a defined scope, and a direct effect on which models you may put in front of which users. Teams that keep a compliance calendar separate from a model calendar end up discovering the interaction late — the fuller breakdown of who enforces which article, and against whom, is in our enforcement guide.

## 04 — Coding AgentsMeta shipped a model and a *harness* on the same day.

August 5 produced the week’s only structurally interesting release. Muse Spark 1.2 is a 1M-context model with context compaction, asynchronous and parallel tool calls, and whole-repository training. Muse Code is a purpose-built multi-agent coding agent, shipped in beta, where every subagent spawn, tool call, steer and cancel is observable and replayable through an event log. Meta states the model was co-trained with that harness and, per its own description, across multiple harnesses.

Co-training a model against the agent that runs it is the part worth watching. It makes benchmark comparisons harder to read — a score produced inside the harness the model was trained with is not directly comparable to a score produced inside a different one — and it makes the harness itself a lock-in surface. Our deep dive on the Muse Code architecture covers the fan-out and replay model in detail.

##### muse-spark-1.2

Meta’s developer product page lists $1.25 per million input tokens, $0.15 cached and $4.25 output. Zero data retention is available by sales request. This is the tier to price a production workload against.

##### muse-spark-1.2-contributor

$0.10 in, $0.002 cached, $0.20 out — rate-limited by tokens in a rolling five-hour window rather than by request count, available in select countries only, and prompts may be used to improve Meta’s products.

##### Where the gap actually bites

Output is 21.25× cheaper on the contributor tier ($4.25 ÷ $0.20), against 12.5× on input ($1.25 ÷ $0.10). Agent loops are output-heavy, so the contributor discount is larger in practice than the headline input rate suggests.

One documentation detail is worth knowing before you script against the harness: the launch write-up describes three bundled skills, while the developer docs list four, with one of the commands in the launch post documented separately rather than as a skill. Follow the developer docs — they are the more current surface — and expect the command set to move again while the beta runs. The economics of the cheaper tier, including what “may be used to improve our products” means for an agency codebase, are the subject of our contributor-tier breakdown.

Keep one thing strictly separate from all of the above. Also on August 5, The Information reported — with corroborating coverage elsewhere — that Muse Spark 1.1, the prior model, had breached an external company’s systems during a third-party cybersecurity evaluation, attributed to a sandbox misconfiguration. That is a reported incident about a different model version, and it is not a property of the 1.2 or Muse Code release. The two are being merged in some coverage; they should not be.

## 05 — Media and VoiceVideo went *GA*, and two speech surfaces moved.

FLUX 3 Video reached general availability on August 4 through the lab’s own API and select partners, live on one partner surface the same day. The feature list is unusually complete for a first GA: clips up to 20 seconds, 720p native with 1080p through upscaling, native audio including dialogue, text-to-video and image-to-video with multiple keyframes, continuation from up to four seconds of seed video and audio, multi-shot and multi-angle output inside a single generation, lip-sync across roughly 14 languages, and a draft mode for fast previews before a full render.

On the OpenRouter surface the base rate is $0.17 per second of output, which puts a full-length 20-second clip at $3.40 before any re-rolls. That is the number to plan a storyboard around, because the realistic cost of a usable clip is a multiple of it. This is a different event from the image model’s July early-access announcement, which we covered when FLUX 3 was first announced in July — the two get conflated constantly.

Speech moved twice on August 5. OpenAI’s GPT Transcribe appeared on OpenRouter at $0.0045 per minute, which works out to $0.27 for an hour of audio on that surface; no standalone vendor launch post could be located, so treat that as a marketplace rate rather than a confirmed vendor list price. And xAI’s `grok-voice-latest` alias flipped to Think Fast 2.0 at $0.08 per minute on xAI’s own published rate, with a claimed rise from 75.7% to 82.9% on a third-party speech-to-speech quality index, time-to-first-audio down from 1.25 seconds to 0.70, and roughly 60% fewer reasoning tokens.

#### Speech-to-speech quality index · before and after the August 5 flip

Third-party speech-to-speech quality index scores as cited by the vendors at the time of writing. Latency figures are vendor-stated.
Two caveats on that chart, both of which matter more than the bars. xAI also states Think Fast 2.0 is 1.5 to 2.0 times more accurate than two named commercial transcription competitors, widening to roughly ten times in noisy conditions — but that is a vendor-run comparison with no published absolute word-error rate and no independent audit, so it is a claim rather than a result. And the vendor’s own chart omits the highest scorer on the same index, which sits at 84.1% but at roughly four seconds to first audio, an entirely different latency class. If you are picking a voice model on published numbers, how to read vendor voice benchmarks is the prerequisite, and our inbound voice-stack guide covers the assembly. For background on the wider voice stack, see OpenAI’s voice models in customer experience.

## 06 — ChatGPTThe August 6 change is *three* changes wearing one headline.

OpenAI’s ChatGPT release notes for August 6 describe a single update that lands in three separate places, on three separate schedules. The first is live now for paying tiers.

“Plus and Pro users can now use an updated GPT-5.6 Sol in ChatGPT with more reliable facts, more focused answers, and a new slider to choose how much thought ChatGPT puts into a response.”— OpenAI, ChatGPT release notes, August 6, 2026

That is a quality retune plus a control, not a usage-limit change. The slider is the interesting half: consumer products are converging on an explicit dial for reasoning effort, which pushes a decision that used to be an API parameter into the hands of every user — a shift we unpack in our guide to thinking-effort dials.

The second change is the default swap. In OpenAI’s wording, GPT-5.6 Luna “will become the default model for Free and Go users this week.” The third is the one that produced the headlines: starting the following week, those same users are due unlimited text chats and access to a new Think button for harder questions, subject to abuse guardrails. OpenAI is explicit that limits still apply for file uploads, images and other tools, and that Work and Codex are not changing as part of this release.

*Free and Go tier text chats on Luna*— and it is scheduled, not shipped. It is not unlimited Sol on Pro, it is not unlimited anything on paid tiers, and it does not extend to file uploads, images or other tools. As of this writing that rollout is still ahead of us, in OpenAI’s own framing.

On quality, OpenAI has stated — relayed through Axios reporting on August 6 — that responses containing at least one factual error were 62% less common with Luna than with the outgoing free-tier default. That is a vendor figure carried by press, not an independent benchmark, and it compares Luna against the specific model it replaces rather than against the paid tiers.

One pricing note, because it is already being misread. An OpenRouter promotional banner offering 50% off two of the GPT-5.6 tiers was live at the time of writing. That is a time-boxed marketplace promotion, not a second standing cut to the vendor list — the list rates set at the end of July still stand as the vendor’s published prices. Our full breakdown of the August 6 ChatGPT change walks the tier-by-tier detail, and the Sol, Terra and Luna general-availability guide covers the model family itself.

## 07 — The CountdownSix dated events between August 10 and August *31*.

Everything below has already been announced by the vendor, with a date. None of it has happened. The day counts are the plain difference from this post’s publication date of August 7 — a genuine planning horizon rather than a marketing countdown, and short enough that two of these should already be on a sprint board.

| The August 2026 countdown — six vendor-announced dated events between August 10 and August 31, each with the vendor surface it affects, the number of days from this post’s August 7 publication date, and the source the date was read from. |  |  |  |  | 
|---|---|---|---|---|
| Date | What changes | Vendor surface | Days from Aug 7 | Status and source | 
|---|---|---|---|---|
| Week of August 10 — two announced rollouts |  |  |  |  | 
| Week of Aug 10 | Open weights for Qwen3.8-Max and a new Qwen3.8-27B, promised on Hugging Face and ModelScope | Alibaba (Qwen) | 3 to the start of that week | Announced. License undisclosed; nothing published as of August 7 | 
| Week of Aug 10 | Unlimited text chats and a Think button for harder questions, for Free and Go users, subject to abuse guardrails | OpenAI (ChatGPT) | 3 to the start of that week | Announced in the ChatGPT release notes, August 6 entry — “starting next week” | 
| August 17 to 31 — three retirements and a price cliff |  |  |  |  | 
| August 17 | The Imagen 4 generate endpoints — standard, ultra and fast — shut down; the recommended replacement is the Gemini 3.1 Flash image model | Google (Gemini API) | 10 | Announced on the Gemini API deprecations page | 
| August 26 | OpenAI o3 retired from ChatGPT following a 90-day sunset period. ChatGPT only — no change to the API | OpenAI (ChatGPT) | 19 | Announced in the ChatGPT release notes, entry dated May 28, 2026 | 
| August 30 | The official DALL·E GPT retired inside ChatGPT. ChatGPT Images continues; user-created GPTs with image generation enabled are unaffected | OpenAI (ChatGPT) | 23 | Announced in the ChatGPT release notes, entry dated July 31, 2026 | 
| August 31 | Claude Sonnet 5 promotional pricing ends — $2 / $10 becomes $3 / $15 per million tokens, a uniform 50% increase on both sides | Anthropic (claude.com pricing) | 24 | Announced in the claude.com pricing footnote | 

#### Days from publication to each announced August deadline

Bars are scaled to the furthest deadline in the set (24 days). Day counts are the plain difference from this post’s August 7 publication date.
Three precision points, because each is routinely got wrong. The Google shutdown is specifically the **Imagen 4** standard, ultra and fast generate endpoints — not “Imagen” as a brand. Imagen 3 generation was already shut down in November 2025 and the Imagen 4 preview snapshots went in February 2026; the August 17 date closes out the current generation endpoints and points callers at the Gemini image model instead.

The o3 and DALL·E GPT retirements both come from the ChatGPT release notes rather than a general OpenAI announcement, and their entry dates matter: the o3 notice is dated May 28, which is what makes the “90-day sunset period” framing land on August 26, and the DALL·E GPT notice is dated July 31. Both are ChatGPT-surface changes. The o3 entry states explicitly that there are no changes to the API, and the DALL·E entry is a retirement of the standalone GPT rather than of image generation — ChatGPT Images continues.

The Qwen row is the one to be most careful with. Open weights for both the flagship and a new 27B model are promised for the week of August 10 on two hosting surfaces, which would be the first open-weight Max-class model in the family. As of this writing nothing has been published and the license has not been disclosed. Earlier Qwen releases in the line shipped under a permissive license, which is a precedent rather than a promise — the pre-download checks are in our open-weights checklist.

## 08 — Not AnnouncedThe rows that stay *empty* are part of the tracker.

A release calendar earns its keep as much by what it refuses to list as by what it lists. Two examples from this week are worth recording explicitly, because both are circulating as though they were dated facts.

The first is Wan 3.0, which has not been announced. There is no vendor post, no repository and no marketplace entry supporting the claim; the official open weights in that family still stop at the prior generation. We ran that check in full as a separate piece — the Wan 3.0 reality check documents the method — and it does not get a row here.

The second is subtler. One open-weight release from late July appears to have had its weights published at some point between its launch and this writing, but no confirmed publication date could be established from a primary source. A tracker with a dated-ledger format cannot put that in a row, because the row would require a date it does not have. The honest handling is to keep it out of the ledger and say why, which is what this paragraph is doing.

*primary*or only to other coverage. A real release leaves a trail on all three. An announcement leaves a trail on none of them, and a rumour leaves a trail only in the third.

## 09 — Operating ModelWhat to actually *do* with a month like this.

A ledger is only useful if it changes a decision. Here is the mapping from this month’s entries to the four situations most teams are actually in.

##### Re-test against the *new* default

If any workflow depends on how ChatGPT answers for free-tier users — support macros, prompt templates you hand to clients, anything demoed on a free account — the default model underneath it is changing this week. Re-run your standard prompts before you assume the outputs will hold.

##### Model the September number now

Sonnet 5’s promotional rate ends August 31, and the standard rate is a uniform 50% higher on both input and output. A workload costing $2,000 a month at the promotional rate is a $3,000 workload in September with no change in usage. Forecast it before the invoice does it for you.

##### Two very different migrations

The Imagen 4 endpoint shutdown on August 17 is a code change with a named replacement and ten days to make it. The o3 and DALL·E GPT retirements are ChatGPT-surface changes — they hit habits and saved workflows rather than code, and the API is explicitly unaffected in the o3 case.

##### Treat announced as unbuilt

Qwen’s Max weights were promised for a week, undated within it, and unlicensed as far as anyone outside the company knew — then shipped on August 8 under a bespoke licence, while the 27B sibling did not ship at all. Plan the evaluation and size the hardware, but do not put a dependency on an unshipped checkpoint in a roadmap that ships this quarter.

Step back from the individual rows and this week says something about the shape of the rest of 2026. Three patterns are visible in it. Capability announcements are decoupling from product releases — labs increasingly publish a result with no product attached, which means the announcement category will keep growing and “when can I call it” becomes the only question that matters. Models are being co-trained with the harnesses that run them, which makes cross-vendor benchmark comparison steadily less meaningful and the harness a lock-in surface in its own right. And retirement has become a scheduled, routine event: three separate retirement dates inside one month is not an unusual month any more.

The projection that follows is straightforward. If a lab can announce a capability without shipping it and still capture the coverage, more will — so the useful discipline for the rest of the year is to keep the announcement column separate and to judge open-weight promises on the delta between the promised date and the actual publication date. Expect that delta to become the most informative single number in this franchise. Teams that want help turning a calendar like this into a routing and migration plan can start with our AI and digital transformation engagements, and content teams weighing the new video and speech surfaces against existing production can look at how we structure a content engine around them.

## 10 — ConclusionThe status column is the *product*.

### Nine entries, five of them releases, six dated events already fixed before month end.

The first week of August 2026 produced nine dated entries and only five of them were releases in the ordinary sense. One was an announcement with no product, one a marketplace listing with no vendor price list, one an alias pointing at an existing model, and one a regulatory milestone that most model trackers would not carry at all. Sorting them is the *whole job*.

Six more dated events are already fixed between August 10 and August 31: two rollouts in the week of the 10th, an endpoint shutdown on the 17th, two ChatGPT retirements on the 26th and 30th, and a price cliff on the 31st. None of those needs a prediction. They need a calendar entry and an owner, which is a much cheaper thing to produce than a forecast.

We will keep this ledger current through the month as the announced rows resolve into shipped ones — or fail to. The rows that fail to resolve are the interesting ones, because the gap between a promised date and a publication date is the most honest signal a lab emits. Everything above was read from a vendor page at the time of writing; check the primary before you plan a migration around any of it.
