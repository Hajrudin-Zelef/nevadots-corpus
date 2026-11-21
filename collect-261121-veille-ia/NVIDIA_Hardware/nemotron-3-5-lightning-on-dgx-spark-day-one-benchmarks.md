---
id: collect-261121-veille-ia/nvidia-hardware/nemotron-3-5-lightning-on-dgx-spark-day-one-benchmarks
title: "Nemotron 3.5 Lightning on DGX Spark: day-one benchmarks"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: []
keywords: ["lightning", "model", "nemotron", "incumbent", "spark", "production", "every", "nano"]
source: https://www.rawrtech.ai/blog/nemotron-lightning-day-one-bench
source_anchor: ""
source_lines: [1, 77]
section: "NVIDIA_Hardware"
---
NVIDIA released Nemotron 3.5 Lightning on a Tuesday. Twenty-four hours later
it was running on our DGX Spark with tracked benchmarks — **67.4 tok/s, 98% on
GSM8K, 93% on IFEval** — and by Wednesday night it had *replaced* the model our
production agent uses for background traffic. Elapsed time from "I heard
there's a new model" to verified head-to-head numbers: about four hours, most
of it the machine benching unattended. The promotion followed the same day.

The model is a good story. The better story is what made those four hours possible: the deliberately boring benchmark infrastructure we stood up the week before, which caught two things we would otherwise have missed — a 14% performance regression hiding inside a required upgrade, and a failure mode in our incumbent model that no leaderboard would ever show us.

## The contender: 30B parameters, 3B active

Nemotron 3.5 Lightning is NVIDIA's new small end of the Nemotron 3 family: 30B total parameters with only 3B active per token (hybrid Mamba-2 mixture-of-experts), a 1M-token context window, native tool calling, built-in speculative decoding, and an open license (OpenMDW) with weights, data, and recipes. NVIDIA explicitly targets the DGX Spark — one of the speculative-decoding draft models is literally named "DSpark."

For a 128 GB unified-memory box like ours, the 25 GB q4_K_M build is the sweet
spot: it loads *beside* our resident 36 GB production brain with ~45 GB to
spare. No downtime to evaluate it — that alone is a change from July, when
every big-model test meant stopping production.

It also mattered that we run a tracked benchmark system now: MLflow as the system of record, one fixed prompt, per-run engine versions, and validity guards (GPU-placement checks, free-memory preflights) learned the hard way in round 2. Adding a model is one entry in a YAML file and two make targets.

## The numbers

Head-to-head against the incumbent fast lane (Nemotron 3 Nano, same 30B class) and our production brain as reference — 100 samples per quality suite, same prompt and settings for every speed run, all placements GPU-verified:

| Model | tok/s (single-stream) | GSM8K | IFEval (strict) | 
|---|---|---|---|
| **Nemotron 3.5 Lightning** (q4_K_M) | 67.4 | **0.98** | **0.93** | 
| Nemotron 3 Nano (incumbent) | 67.6 | 0.96 | **did not finish — twice** | 
| Qwen3.6-35B brain (reference, vLLM NVFP4) | 94.6 | 0.97 | 0.95 | 

Speed-tied with the incumbent, better at math, near our production brain on instruction following at a third less speed but a quarter of the memory. On those numbers alone, Lightning earns the fast lane. But the two footnotes are where the value of measuring actually lives.

## Catch #1: the upgrade tax nobody mentions

Lightning requires Ollama 0.32.9, released the same day as the model — older versions don't know its architecture. Routine bump, right?

Our harness logs the engine version with every run, so the first thing we did
after upgrading was re-bench the *incumbent* on the new engine. Result: Nano
dropped from **78.2 to 67.6 tok/s — a 14% regression** in the runtime path both
models share. We ran solo controls (each model alone in memory) to rule out
interference; the regression is real and reproducible.

That changes the comparison entirely. Naively benched — Lightning today versus
Nano's number from last week — Lightning "loses" by 14%. Same-engine, it's a
tie. Every public "model A vs model B" table that mixes runtime versions has
this problem, and almost none of them disclose it. If you benchmark locally:
**the engine version is part of the result.** Log it, and re-bench your
incumbent every time it changes.

## Catch #2: the failure a leaderboard can't show you

The incumbent's IFEval entry says "did not finish" because Nano hit our 15-minute-per-request ceiling — twice, deterministically. At least one prompt in the suite sends it into a reasoning loop that generates for over 900 seconds at full speed (60k+ tokens of thinking) without producing an answer. The identical harness scored the suite cleanly for three other models, so this is model behavior, not our plumbing.

A leaderboard would show you Nano's score on the samples it completes. Running the suite yourself shows you the distribution's tail — and for an always-on agent lane, a model that occasionally vanishes into an hour of silent thinking isn't a lower score, it's a disqualification. That behavior, more than the 2-point GSM8K gap, is what settled the promotion.

## What we actually changed

Same-day, the promotion shipped as one reviewed change: the model swap in our
router's fast lane, the environment contract, the benchmark lane registry, and
the docs — deployed, then verified by checking *which model actually loaded*,
not just that an endpoint returned 200. (That last habit comes from July, when
a silent CPU fallback corrupted numbers we'd already published. Verify what
ran, not that something ran.)

Honest caveats, because vendor-adjacent numbers deserve them:

- NVIDIA's "up to 4× output speed" claim did not show up in our prose-decode test. Lightning's speculative decoding makes throughput depend on how predictable the output is — community numbers show ~85 tok/s on JSON versus ~72 on prose, and our fixed benchmark prompt is prose. Different workload, different number.
- The faster vLLM serving path (community reports 88–108 tok/s with the DSpark draft model) is untested here — NVIDIA's own Spark container doesn't support the architecture yet.
- 100 samples per suite is a screening bench, not a paper. It was enough to make this decision; it wouldn't survive peer review.

## The real lesson: infrastructure beats hustle

Nothing we did Tuesday night was clever. The research pass was verified web search. The benchmarks were two make targets. The comparison was a query against runs that already existed. The cleverness was spent a week earlier, building a boring system that logs versions, checks GPU placement, refuses to start without enough free memory, and keeps every number in one queryable place.

That's the part that transfers to any business adopting AI: models will keep dropping on Tuesdays. The teams that can say "we measured it on our workload by Wednesday" — and catch the regression hiding in the upgrade — are the ones whose model choices compound instead of churn.

If you want that kind of measured, evidence-first look at where AI actually fits your operation — including whether your own numbers can be trusted enough to build on — start with a free 45-minute AI Opportunity Briefing.
