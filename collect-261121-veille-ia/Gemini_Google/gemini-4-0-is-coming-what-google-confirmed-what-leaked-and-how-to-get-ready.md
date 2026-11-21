---
id: collect-261121-veille-ia/gemini-google/gemini-4-0-is-coming-what-google-confirmed-what-leaked-and-how-to-get-ready
title: "Gemini 4.0 Is Coming: What Google Confirmed, What Leaked, and How to Get Ready"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-07-21"]
keywords: ["gemini", "google", "model", "confirmed", "date", "leaked", "launch", "most"]
source: https://huggingface.co/blog/liliruli/gemini-4-coming-confirmed-leaks-how-to-prepare
source_anchor: ""
source_lines: [1, 97]
section: "Gemini_Google"
---
*Excerpt: Gemini 4.0 is not released yet, but Google's own leaders now say it is in post-training and arriving "much earlier" than the end of the year. Here is the clean split between what Google confirmed, what is only leaked, and what builders should set up before launch day.*

Gemini 4.0 is coming. That is no longer a rumor bucket — Google has put it on the record twice. In July it confirmed it had started its "most ambitious pre-training run yet, for Gemini 4." In late September, Google DeepMind's new lead, Koray Kavukcuoglu, said the model has moved into post-training and is being aimed at a release "much earlier" than the end of 2026.

What Google has *not* published is the part you would actually build against: no model ID, no price, no context limit, and no entry in the Gemini API catalog. That gap is where most coverage blurs — official statements, anonymous benchmark leaks, prediction-market odds, and social screenshots all collapse into the same "it's almost here" headline.

This article keeps them apart. It sorts everything circulating about Gemini 4.0 into three buckets — **confirmed, leaked, and ruled out** — and then concentrates on the part you control: getting your stack ready so launch day is a config change, not a scramble.

There are only a handful of dated, official Gemini 4 statements, and they say remarkably little about capabilities. Keeping them in one place is the fastest way to stop re-litigating rumors.

| Date | What Google said | Where it came from | What it still does not say | 
|---|---|---|---|
| July 21, 2026 | Pre-training for Gemini 4 has started — the company's "most ambitious pre-training run yet" | Google's own blog | Any date, variant, or spec | 
| July 22–23, 2026 | Gemini 4 is a "significantly larger" frontier model; coding and autonomous agents are stated priorities; Google is "a bit behind" in agentic coding | Alphabet Q2 2026 earnings call | Any date, price, or model ID | 
| September 24–25, 2026 | Gemini 4 is in post-training, targeted "as soon as possible" and "much earlier" than year-end; internally powering Antigravity | DeepMind lead Koray Kavukcuoglu, at The Information's AI Agenda Live Summit | Any date, price, or model ID | 
| Late September 2026 | No Gemini 4 in the Gemini API catalog, pricing page, or changelog; newest models remain Gemini 3.8 Flash and 3.8 Live | Google developer documentation | — | 

The single most important line in that table is the last one. **A confirmed model is not a usable model.** Google has confirmed Gemini 4 exists and is nearly through training, yet there is still nothing to call. For anyone planning work, that distinction matters more than the launch date.

Two context points explain why the timeline is moving now. First, Google has not shipped a true flagship since Gemini 3 Pro in November 2025, and it quietly skipped the teased Gemini 3.5 Pro to focus on cheaper Flash releases — most recently Gemini 3.8 Flash in early September. Second, Kavukcuoglu took over day-to-day leadership of DeepMind in August after Demis Hassabis stepped back, which puts Gemini 4 on a new leader's release plan. The stated intent — "as soon as possible," "much earlier" than year-end — is a direction from that plan, not a commitment to a date.

"We'll ship soon" is not testable. Instead of guessing a date, watch for the three gates that turn an announcement into something you can actually call. They close in sequence, and a launch only becomes real for developers at the second one.

1. **Official naming.** A Google or DeepMind post names Gemini 4 and its variants, plus the launch scope. This confirms identity and tells you nothing about how to request it.
2. **API catalog entry.** A model ID string, a pricing row, rate limits, and a changelog line appear in the Gemini API docs (and reach AI Studio and Vertex AI). This is the first gate you can act on, because it fixes the request contract.
3. **A verified route.** A gateway lists an approved model ID, a price, and a successful authenticated call whose returned identity matches the request, with usage and billing that reconcile. A downstream listing only*claims* the model exists; verification is separate.

| Watch signal | What it would confirm | Status as of late September 2026 | 
|---|---|---|
| A Google post or keynote naming Gemini 4 | Identity, variants, launch scope | Not yet | 
| Gemini at Work event (October 8, 2026) | A likely launch venue | Scheduled | 
| Alphabet Q3 earnings call (around October 28) | A possible timing comment | Not yet announced | 
| Gemini API catalog entry | Model ID, price, limits — the actionable gate | Absent | 
| Prediction markets | Crowd odds, not facts | Roughly November favored | 

On timing, the honest answer is that no date exists. What circulating coverage calls a "release date" is a prediction. Prediction markets put the odds well above even by the end of November and give October meaningful weight, while leak-watchers have leaned later, toward November or December, based on traces in app code. Read those as a shared expectation, not a schedule — every one of them can shift on a single Google sentence.

The most credible unofficial thread concerns a checkpoint, not a product. Leak-watchers report that Google has been testing early checkpoints of **Gemini 4 Pro** internally under the code name **"Argon"** since mid-September. The corroborating observation is a ghost listing: starting around September 14, a model labeled with an existing Flash name (`gemini-3.8-flash`) began appearing in blind battles on LMArena, and testers read its output — strong SVG, 3D, and polished web layouts — as a Pro-tier checkpoint wearing a pseudonym.

It is a familiar technique. Labs test a frontier model in public under a muted label to gather real user preferences without revealing the name. It is also why the leak trail is noisy: the labels reuse public names, so "a Gemini 4 checkpoint" is an inference from behavior, not an identifier. Google has not confirmed "Argon," and no leaderboard carries a Gemini 4 entry.

The reported specifications should be handled with the same discipline. The frequently cited numbers are striking — up to 256,000 output tokens in a single response, an undecided 2-million-token context window, and reasoning runs that stretch from a couple of minutes to twenty in the highest mode. They also come from leak accounts that contradict one another, which is the normal state of pre-launch chatter.

| Leaked claim | Where it comes from | Evidence strength | How to use it | 
|---|---|---|---|
| Internal code name "Argon" for a Gemini 4 Pro checkpoint | Leak watchers (TestingCatalog, citing a source) | Medium | Plausible, unconfirmed | 
| Up to 256k output tokens | Leak accounts | Low–medium | Do not plan capacity around it | 
| 2M-token context window | Leak accounts, contradicted | Low | Explicitly undecided | 
| Benchmark scores across coding and agent tests | A single viral post | Low | Do not cite as fact | 
| Better SVG, 3D, and web-design quality | Multiple testers | Low–medium | Directional signal only | 

Sort every signal into a tier before it influences a decision:

- **Primary** — Google's blog, earnings calls, and API docs. Quotable and actionable.
- **Secondary leaks** — checkpoint sightings and spec claims. Interesting, unverified.
- **Market odds** — prediction-market prices. A crowd read, not evidence.
- **Social claims** — screenshots and viral posts. Treat as entertainment until corroborated.

The practical rule that falls out of this is simple: **act on the primary tier, note the secondary tier, and ignore the rest until the API catalog agrees.**

There is a second, more speculative door into the same question. If Google wanted to test a frontier model in public without attaching its name, it could do what several labs already do — release it as an anonymous "stealth" model and let the community argue about who built it. So could Gemini 4 be sitting behind a codename like Space Bunny or Pixel Canary?

That is the exact question we put to the test in our teardown, Gemini 4: Space Bunny vs Pixel Canary — JEV Test. Run through JEV, a typed decision layer for agents, the forced-choice answer leaned toward Space Bunny — and then, asked to justify itself against the evidence, it returned *insufficient evidence*. The strongest technical clue points away from Google: tokenizer studies place Space Bunny in the MiniMax family, not in any Gemini line. For the full reasoning trail, see the Space Bunny Alpha identity write-up, and for the tooling itself, the JEV API.

The verdict, stated plainly: the stealth-model theory is worth following, but the current evidence leans away from it. And the lesson holds regardless of the answer. A codename you cannot verify is not a foundation, which is exactly why the next section matters more than the guess.

Before you trust a headline, rule out three lookalikes:

- **Gemma 4** is Google's open-weights model family. It is a different product with different distribution, and a Gemma 4 release says nothing about Gemini 4.
- **NASA's Gemini 4** was a 1965 crewed spaceflight. It has nothing to do with the model.
- **Unlabeled checkpoints and "gemini-4" repos.** Anonymous arena entries and third-party repositories that borrow the name are speculation, not releases.

Here is the reassuring part: none of Gemini 4's unknowns block preparation. The work below is the same whether the model lands in October or December.

1. **Keep the model ID in configuration, not code.** When Google publishes the real ID, switching routes should be a reviewed config change, not a hunt for hard-coded strings. Leave the slot empty until the catalog entry exists — do not pre-fill a guessed ID.
2. **Freeze your current baselines.** Record today's cost per accepted task, latency percentiles, and failure rates on your own workload using the models you already call. These become the benchmark Gemini 4 has to beat.
3. **Save representative evaluation traces.** Export a fixed set of real requests and accepted outputs, including tool calls, structured-output schemas, and long-context cases. A new model is judged against your traces, not vendor demos.
4. **Define promotion and rollback gates in advance.** Write down the thresholds a new model must clear — accuracy on your traces, cost per accepted task, P95 latency, error rate — and the conditions that send traffic back. Deciding before launch removes the pressure to decide during it.
5. **Log the compatibility surface and keep a named fallback.** Note which parameters, tool-calling formats, and response fields your code depends on, and keep a second model on the same key so a rename or a bad release is a failover, not an outage.

Step five is where a multi-model gateway earns its place. Gemini 4's arrival is the moment "which model do I route to?" becomes urgent, and you do not want that answer locked to one provider's endpoint. BeatAPI serves 60+ models through a single OpenAI-, Anthropic-, and Gemini-compatible key, so the same integration that calls a model catalog entry today can point at Gemini 4 the day it ships — with named text models on the same key standing by as fallbacks. Live rates sit on the pricing page, so you can compare cost per accepted task before you commit traffic.

The strategy is not to bet the roadmap on one unreleased model. It is to make adoption a one-line change so that when Gemini 4.0 actually arrives, you are first in line without having been stopped in the meantime.

**Is Gemini 4.0 out yet?** No. As of late September 2026, Google has confirmed training and post-training, but there is no public model, model ID, price, or API entry. The newest shipping Google models are Gemini 3.8 Flash and 3.8 Live.

**Is it "Gemini 4" or "Gemini 4.0"?** Google's statements use only the phrase "Gemini 4." Variant names and the "4.0" formatting are community labels, so treat them as unconfirmed.

**Is Gemma 4 the same as Gemini 4?** No. Gemma 4 is Google's open-weights family; Gemini 4 is the next proprietary flagship. They ship separately.

**Should I wait for Gemini 4 before building?** Prepare now, evaluate later. Set up baselines, traces, and rollback gates today, and run the real evaluation only once an official model ID and pricing exist. Waiting for a launch date you cannot control is the slow option.

**What is "Argon"?** It is the reported internal code name for a Gemini 4 Pro checkpoint seen testing on LMArena under a disguised Flash label. Google has not confirmed it.

**How do I switch when it launches?** Keep the model name in configuration and route through a gateway that exposes Gemini 4 alongside the models you already use, so switching is a config edit and reverting is one line away.

Gemini 4.0 is coming, and the only thing you get to control is whether it finds you ready. Confirm what is official, discount what is not, and build so that the launch is a change you made on purpose — not one that happened to you.
