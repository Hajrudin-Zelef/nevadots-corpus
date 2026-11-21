---
id: collect-261121-veille-ia/microsoft-actu/mai-decision-1-vs-microsoft-decision-1-what-changed
title: "MAI Decision 1 vs Microsoft Decision 1: What Changed"
domain: microsoft-actu
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-10-08"]
keywords: ["decision", "microsoft", "version", "against", "model", "base", "price", "foundry"]
source: https://orcarouter.ai/blog/mai-decision-1-vs-microsoft-decision-1
source_anchor: ""
source_lines: [1, 77]
section: "Microsoft_Actu"
---
This matchup has a trap in it, and the trap is the premise. **MAI Decision 1** and **Microsoft Decision 1** are not two competing models. They are the same Microsoft product line, one version apart: the model the Ignite coverage labelled MAI Decision 1 is **Microsoft-Decision-1.1**, and Microsoft-Decision-1 is the October release it replaces. So the useful comparison is not "which vendor wins" — it is "what did five weeks and a new base model buy, and is the upgrade worth a migration for anyone already in production on the older endpoint". The short answer: six index points, half the latency, image input, and a price list that did not move.

## Naming, settled once

The label on the left of this comparison comes from keynote coverage, not from Microsoft. The vendor's own strings are "Decision 1.1" in the Ignite lineup post and "Microsoft-Decision-1.1" on the Foundry blog and model card; the MAI prefix travelled with MAI Code 2 and MAI Cyber 2 Flash through the same third-party write-ups and stuck. So this page compares version 1.1 against version 1 of one product line, and the interesting question is not which vendor wins but what five weeks and a different base model changed.

Version 1 has not been withdrawn. It is still listed in Foundry and still scored on the independent board, which is why the two names keep appearing side by side in comparison tables — sometimes as rivals from different companies, which they are not.

## Six weeks, two models, one price

Version 1 went GA on 8 October 2026: a text-only post-train of Alibaba's Qwen3.5-9B, calibrated probabilities over a fixed option set, a 32,768-token ceiling, no weights, no published benchmark figures. Version 1.1 landed on 17 November with a Gemma 4 26B-A4B base, image input and a full performance story — while the price list stayed exactly where it was, at $0.042 per million tokens.

• Base model — Qwen3.5-9B, dense, 9B total vs Gemma 4 26B-A4B, mixture-of-experts with about 4B active; both post-trained in-house

• Modality — text only vs text, JSON and PNG/JPEG images, up to 8 per question

• Input ceiling — 32,768 tokens vs 32,768 tokens, unchanged

• Question types — yes/no, multiple-choice, rating, classification, rubric vs the same set, exposed as three typed primitives (noul, choice, score)

• Price — $0.042/M tokens Global Standard, $0.05 EU and APAC vs identical

• Measured latency — 274.25 ms per task vs 123.25 ms per task, Artificial Analysis

• Measured cost — $142.56 per million tasks vs $139.64 per million tasks, Artificial Analysis

• Decision Index v2.1 — 62.07 vs 67.78

• Weights — none, hosted in Foundry only vs none, hosted in Foundry only

The one line that did not change is the one most teams budget on, and the one that did change most is the one nobody advertises: latency halved while the per-token price stayed flat, because the new base is a sparse model that does far less work per call.

## What the six points are made of

The composite gap of 5.71 is not evenly spread, and the unevenness tells you which workloads the upgrade was built for. On Artificial Analysis's six constituent evaluations, reading the board on 18 November 2026:

• When2Call tool calling — 95.1 on 1.1 against 87.9 on version 1, a 7.1-point gain and the one that matters for agent guardrails

• EvalSafe-O*NET workplace judgement — 88.8 against 86.8

• AA-Symmetry, decision invariance under rewording — 75.7 against 70.4

• AA-Paperclip document decisions — 58.7 against 51.8, the largest absolute jump

• AA-Omniscience, knowledge and hallucination — 60.5 against 54.9

• HLE multiple-choice reasoning — 46.1 against 39.4

Every sub-score improved, which is unusual and makes the aggregate trustworthy; a composite that rises on one evaluation and falls on two is usually a weighting artefact. The invariance gain is the quiet one worth flagging. Symmetry measures whether a decision survives having its options reordered or its question rephrased — precisely the failure mode Microsoft's own documentation warns about, and precisely the property that decides whether a threshold you tuned in staging still fires in production. Moving from 70 to 76 on that is a bigger operational change than moving from 62 to 68 on the headline.

Calibration improved in the same direction. The Brier score, which measures the squared gap between a stated probability and the truth, fell from 0.1965 to 0.1604 — lower is better. Accuracy of the chosen option rose from 40.6 to 48.5. Both numbers are Artificial Analysis measurements on its own tasks, not Microsoft's, which is the reason to weight them: Microsoft's own launch framing for 1.1 is a score of 68 "up six points from our first model", and the independent board lands within half a point of that at 67.78 with a 5.71-point gain. For once the vendor and the auditor broadly agree.

## Where the older model is still the right call

There are three defensible reasons to stay on Microsoft-Decision-1, and none of them is performance.

The first is a frozen evaluation. If you calibrated thresholds against version 1 on your own labelled data — and you should have — then a base-model change invalidates that work. A dense 9B post-trained for scoring and a 26B MoE post-trained for scoring do not fail in the same places, and the docs for 1.1 are explicit that calibration is strongest on familiar task types and can shift with wording. Re-tuning is a real cost, and it lands on the team, not the vendor.

The second is language coverage. Version 1 carried a 25-language post-training story on a Qwen base with well over 200 languages available. Version 1.1 carries a 25+ language story on a Gemma base documented at 35+. Twenty-five plus is not twenty-five, and if you run a multilingual queue the honest move is to test rather than assume the number went up.

The third is the shape of your dependency. Both versions are hosted-only, single-vendor, one price list, one rate-limit regime. Upgrading changes your failure domain without reducing it. If you have already wrapped the endpoint behind your own fallback, staying on 1 costs you latency and accuracy but not availability; if you have not, upgrading buys you a faster model behind the same single point of failure.

## Running the two side by side before you commit

The migration itself is small, which is the pleasant part. The endpoint shape is unchanged — a state plus typed questions to the decision route on a Foundry resource — and the deployment is a model-version change rather than an integration rewrite. What changes is everything downstream of the response: the probabilities, the thresholds, and the sub-scores above.

So run both. Send the same labelled batch to both deployments, compare chosen-option accuracy at the threshold you actually use, and look specifically at the tails — the cases where version 1 returned 0.55 and version 1.1 returns 0.88 are the ones that will silently change your escalation rate. The symmetry gain suggests 1.1 should be more stable under option reordering; verify that on your own prompts rather than trusting the number.

The judge and the thing being judged on one key is the setup most teams end up wanting, and neither Decision model can give you that: both are Foundry-only hosted endpoints, so the router in front of them is your own code. What the router can do meanwhile is supply controls. **Gemma 4 26B A4B** — the base version 1.1 was post-trained from — routes through OrcaRouter at $0.25 per million input tokens and $2.90 per million output, and **Jev 1.14**, the decision model Microsoft benchmarks itself against, at $0.042 per million in and out. Neither reproduces 1.1's calibrated answers. Both tell you whether a general model on your own data comes within a few points of the decision model, which is the question that decides whether you need one at all.

## The version question, answered

If you are choosing today and have no legacy thresholds to defend, take 1.1. It is faster on an independent measurement by a factor of 2.2, slightly cheaper per task, better calibrated, better at tool-call gating, and the only one of the two that will read a screenshot. The price list is identical, so the upgrade is free at the margin.

If you are already in production on version 1, the upgrade is worth doing and is not urgent in the way a security patch is urgent. Budget the re-tuning, keep the old deployment alive until the new thresholds hold on live traffic, and treat the symmetry improvement as the reason to expect fewer surprises rather than as a promise that there will be none.

What this matchup is not is a choice between vendors. Both names point at Microsoft's decision line, five weeks and one base model apart, at the same $0.042 per million tokens. The interesting question was never which one wins; it is whether the second one is enough better to pay for a recalibration. On an independent board, it is.
