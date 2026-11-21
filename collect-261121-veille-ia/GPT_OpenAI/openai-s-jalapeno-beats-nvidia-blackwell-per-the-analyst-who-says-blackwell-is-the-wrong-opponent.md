---
id: collect-261121-veille-ia/gpt-openai/openai-s-jalapeno-beats-nvidia-blackwell-per-the-analyst-who-says-blackwell-is-the-wrong-opponent
title: "openai's jalapeño beats nvidia blackwell, per the analyst who says blackwell is the wrong opponent"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: ["2026-08-25"]
keywords: ["nvidia", "openai", "jalapeno", "blackwell", "rubin", "same", "against", "semianalysis"]
source: https://www.benchbait.dev/story/jalapeno-blackwell
source_anchor: ""
source_lines: [1, 81]
section: "GPT_OpenAI"
---
AI NEWS, TRACKED AND ROASTED.

# openai's jalapeño beats nvidia blackwell, per the analyst who says blackwell is the wrong opponent

OpenAI's Jalapeño inference ASIC beats Nvidia Blackwell. That is the headline SemiAnalysis published on 2026-08-25 — [25] OpenAI Jalapeño: Better Than Nvidia Blackwell ↗ — and the body of that same piece is where the headline goes to die.

The figures first. Against Nvidia GB200 and GB300 rack systems on SemiAnalysis's InferenceX suite: 1.5x to 1.9x more throughput per kilowatt and 1.7x to 3.6x lower end-to-end latency, across GPT-OSS 120B, DeepSeek R1 670B and Moonshot AI's 1-trillion-parameter Kimi K2.5. A 700W part against accelerators rated at 1,200W and 1,400W, with OpenAI stating its measured sustained power stayed at or below 550W. At the GB300's fastest previous time-between-tokens settings, OpenAI claims 8.6x to 104.3x more throughput per kilowatt. [27] Tom's Hardware reported the same figures ↗ the same day, under Luke James's byline, with the standfirst "Nvidia's Vera Rubin wasn't in the comparison."

Two outlets, one dataset. SemiAnalysis is explicit about where the numbers came from: "First, all numbers are provided to us by OpenAI. We verified the InferenceX runs in person in the lab, but we did not run the full suite of InferenceX benchmarks nor have we seen AgentX results." Tom's Hardware carries OpenAI's figures too. Publishing a number twice is publication twice. It is measurement once, by the company that built the chip.

## the article argues with its own headline

Second paragraph of the caveats, same piece: "we believe that comparison to Blackwell is somewhat incomplete and unfair. Jalapeno is really competing against chips like Rubin that also use HBM4." Vera Rubin — the Nvidia platform OpenAI agreed to deploy for the first gigawatt of Nvidia systems in the second half of 2026 — is not in the throughput comparison at all.

Where Rubin does appear is on cost, and there the gap closes: "On perf/TCO, Vera Rubin and Jalapeno are head-to-head, producing almost the same number of output tokens per $. However, as previously mentioned, Jalapeno's results are obtained without speculative decoding and Vera Rubin's results use speculative decoding."

That asymmetry runs through the throughput charts too. SemiAnalysis: "All this is done without Multi Token Prediction (MTP), while the other chips on the chart are the best performing configs of each respective SKU, all with MTP." Tom's Hardware puts the same thing the other way round — the major comparisons ran Jalapeño's single-token prediction against GB300 configurations doing the same, "even though Nvidia deployments commonly use multi-token prediction in production." Turn MTP on for the GB300 and, per OpenAI's own appendix as reported by Tom's Hardware, the peak efficiency lead "shrinks to roughly 1.5 times." Measure all-in utility power per accelerator instead of published package TDP and it is 1.18kW against 2.55kW, which is also not 1.9x.

**published package TDP, no MTP on Jalapeño**

- the lead
- 1.5x to 1.9x throughput per kilowatt

**GB300 running multi-token prediction**

- the lead
- roughly 1.5x at peak efficiency

**all-in utility power per accelerator**

- the lead
- 1.18kW against 2.55kW

**perf per total cost of ownership, against Vera Rubin**

- the lead
- head-to-head, and Rubin had speculative decoding on

The workload is one workload. SemiAnalysis: the runs "are just 8k1k, a much easier workload to tune for, and there are no AgentX runs yet" — single-turn, no multi-turn long-context suite, which is the suite SemiAnalysis says it prefers for comparing chips and has not seen. The models are not the current ceiling either, by its own account: "the models being tested are not on the open frontier. NVIDIA and AMD have published results on larger models such as DeepSeek V4 Pro and Kimi K3, using AgentX." And Jalapeño does not train models — the workload where, as Tom's Hardware puts it, Nvidia's hardware remains unchallenged.

None of the charts can be checked against any of this. All 23 figures in the SemiAnalysis article are bitmap images with empty alt text, captioned "Source: OpenAI", "Source: SemiAnalysis", "Source: OpenAI, SemiAnalysis" or "Source: Computex 2026 keynote". OpenAI's own two results pages could not be opened for this story: both returned a persistent Cloudflare human-verification interstitial, so every OpenAI figure above is carried on SemiAnalysis and Tom's Hardware, not read from OpenAI.

## what people are arguing about

Not the numbers. The provenance and the opponent.

On the [28] Hacker News thread ↗, submitted 5 minutes 24 seconds after the article's publication timestamp and at 303 points and 207 comments when read, "bjourne" goes at the speculative-decoding framing: "How much speculative decoding improves throughput is workload-dependent. Yes, it can improve performance by 5x, but it can also slow down performance by 2x... The tech journos didn't ask themselves if speculative decoding improves perf so much why wasn't it on by default?" "ilaksh" asks the comparison question directly — "Yeah but is it really even as good as Rubin? Seems just competitive." "luciana1u" supplies the epitaph: "everyone's silicon beats everyone else's benchmarks until it has to run someone's actual production workload." "epistasis" reports the article's text disagreeing with its own table, which is unresolvable here because the tables are pictures. "doctorpangloss" asserts "All their benchmarks are flawed," with nothing behind it in the thread.

On [29] r/OpenAI ↗, at 75 points and 34 comments, the top reply is "Mlluell" at 32 points: "Blackwell is a 2 year old architecture to be fair." "a_triple_spiral" raises provenance without ceremony: "Aren't the numbers also self reported or did I misread that?" A [30] bounded X search for "Jalapeno OpenAI" ↗ returned 25 results that are overwhelmingly restatement of OpenAI's figures in several languages, with no independent measurement in the sample; "Neeraj_Kumar222" at least asks the untested question — "How does Jalapeno perform when tool calls and long context dominate latency?" None of that is corroboration. It is people reading the same press release out loud.

## the timing

The [26] Hot Chips 2026 program ↗ lists OpenAI's session as "You Can Just Build Things ... Chips," presented by Richard Ho, Ravi Narayanaswami and Chris Leary, scheduled 4:45–6:15 PM PDT on 2026-08-25. The program does not name Jalapeño. The SemiAnalysis article carries a publication timestamp of 2026-08-25T14:00:38.397Z, roughly ten hours before that session starts — though a Substack timestamp can reflect a scheduled send, and nothing here settles which it is.

The two sources also disagree on how fast this was built, possibly by measuring different things: Tom's Hardware says a nine-month RTL-to-tapeout cycle, SemiAnalysis says design work began in mid-2024 and ran roughly 16 months from initial team hiring to manufacturing tape-out.

## claims

- **1.5x–1.9x throughput per kilowatt, 1.7x–3.6x lower latency versus GB200 and GB300.** Supported, and supplied by OpenAI.
- **The numbers are OpenAI's.** Confirmed. SemiAnalysis verified InferenceX runs in OpenAI's lab, did not run the full suite, has not seen AgentX results.
- **The headline contradicts the body.** Confirmed. "Somewhat incomplete and unfair"; the real peer is an HBM4-class part like Rubin.
- **The configuration is asymmetric.** Confirmed. Jalapeño without MTP and without speculative decoding; the other chips at their best per-SKU configs, with MTP.
- **MTP narrows the lead to roughly 1.5x.** Supported, from OpenAI's appendix via Tom's Hardware.
- **The workload is single-turn 8k1k.** Confirmed. No AgentX runs.
- **Vera Rubin was absent from the throughput comparison.** Supported. It is the platform OpenAI committed to at gigawatt scale in H2 2026.
- **Perf/TCO is head-to-head with Vera Rubin.** Supported — with Rubin using speculative decoding and Jalapeño not.
- **Jalapeño is inference-only.** Supported. It does not train models.
- **Every chart is a bitmap.** Confirmed. Empty alt text, vendor-credited captions, nothing machine-readable.
- **The tape-out timeline is disputed.** Nine months RTL-to-tapeout against roughly 16 months from hiring; different measuring points, unreconciled.
- **"Beating every Nvidia, AMD, and Google chip we have been able to test" is unverified.** It rests on vendor-supplied numbers, a partial suite, one easy workload, an asymmetric config, and a model set SemiAnalysis itself says is off the open frontier.

## receipts

- [25] SemiAnalysis's Jalapeño analysis ↗ — analyst newsletter, published 2026-08-25, running on numbers provided by OpenAI.
- [27] Tom's Hardware's report on the Jalapeño figures ↗ — trade press, 2026-08-25, independently published and sharing the same OpenAI dataset.
- [26] Hot Chips 2026 advance program ↗ — primary venue listing for the OpenAI session.
- [28] Hacker News on the Blackwell comparison ↗ — discourse, 2026-08-25.
- [29] r/OpenAI on the self-reported benchmarks ↗ — discourse, 2026-08-25.
- [30] Bounded X search for "Jalapeno OpenAI" ↗ — 25-result sample, restatement only.

Nvidia has not responded to the comparison; no request for response has been sent.
