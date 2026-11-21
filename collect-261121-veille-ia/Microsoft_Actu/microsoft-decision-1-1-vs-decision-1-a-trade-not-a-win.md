---
id: collect-261121-veille-ia/microsoft-actu/microsoft-decision-1-1-vs-decision-1-a-trade-not-a-win
title: "Microsoft-Decision-1.1 vs Decision 1: A Trade, Not a Win"
domain: microsoft-actu
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["model", "microsoft-decision-1", "decision", "against", "same", "different", "better", "trade"]
source: https://www.orcarouter.ai/blog/microsoft-decision-1-1-vs-microsoft-decision-1
source_anchor: ""
source_lines: [1, 59]
section: "Microsoft_Actu"
---
Every published figure for Microsoft-Decision-1.1 is better than the figure for Microsoft-Decision-1 except one, and the exception is the interesting part. On the Artificial Analysis Decision Index v2.1 the new model scores 67.78 against 62.07, is 2.2x faster at 123 ms per task against 274 ms, and abstains far more reliably — 49.4% non-hallucination on AA-Omniscience-Decision against 21.7%. Yet on the questions the old model got right, the new one gets fewer: 6.29% raw accuracy on HLE-MultiChoice v1.1 against 8.64%, and 47.5% of AA-Symmetry questions answered consistently against 51.7%. Microsoft-Decision-1.1 is not a strictly better Microsoft-Decision-1. It is a differently calibrated one, and the direction of that trade is what a migration decision turns on.

The two models are six weeks apart. Microsoft-Decision-1 reached general availability on 8 October 2026 as a text-only scorer post-trained from Qwen3.5-9B. Microsoft-Decision-1.1 arrived on 17 November 2026 at Ignite on Gemma 4 26B A4B, a mixture-of-experts base Microsoft describes as having enough active parameters for judgment while staying small enough to serve across A100, H100/200 and GB200/300 hardware. Same price. Same 32K ceiling. Same three question types. Different backbone, different latency class, and a measurably different set of strengths.

## The base model change is the release

Version 1 was a dense 9-billion-parameter model. Version 1.1 is a 26-billion-parameter mixture of experts with roughly 4 billion active per token, if you read the A4B suffix the way Google uses it on the Gemma line. That is not an incremental retrain; it is a different architecture, a different family, and a different set of serving characteristics. Microsoft's launch post is explicit that the swap was a cost and intelligence decision, and pairs it with a custom inference engine built for scoring rather than token-by-token generation.

What the swap bought, in the order a buyer would notice:

• Latency — 274.25 ms down to 123.25 ms per task, measured across the whole index. On the fastest single component, When2Call, the new model averages 94 ms against the old model's 230 ms.

• Calibration — the Decision Score, which is accuracy with the Brier penalty applied, rose on every one of the five decision components. Omniscience went 54.89 to 60.46, HLE 39.42 to 46.11, Paperclip 51.82 to 58.67, EvalSafe 86.82 to 88.85, When2Call 87.95 to 95.08.

• Refusal behaviour — non-hallucination on Omniscience more than doubled, 21.74% to 49.40%. On HLE it went 15.05% to 38.36%. For a model whose entire purpose is to be thresholded, this is the most consequential change on the list.

• Modality — images joined text and JSON as accepted input. Audio and video are still refused, and the model still produces no explanation, no rationale, and no output tokens.

• Context — unchanged at 32,768 tokens. Nothing here moved, and that is worth stating plainly because "1.1" invites the assumption that everything improved.

## Where 1.1 is worse, and why Microsoft's own framing predicts it

The regressions are not random. They cluster on the two components that ask the model to know something rather than decide something.

On HLE-MultiChoice v1.1, raw accuracy fell from 8.64% to 6.29% — a 27% relative drop in the share of hard questions answered correctly. The Decision Score still rose, from 39.42 to 46.11, because the model's confidence on the answers it did give became far better matched to whether those answers were right. Translated: it got worse at guessing and much better at knowing it was guessing. The composite rewards the second and barely notices the first.

On AA-Symmetry the picture is subtler and less flattering. Symmetry measures whether the same question, reworded — options reordered, labels swapped, a yes/no negated — produces the same answer with the same probability. Version 1 passed 51.7% of rewordings at the all-pass level; version 1.1 passes 47.5%. Its Symmetry *score* nevertheless improved, 70.42 to 75.72, because when it does drift, it now drifts less far. Fewer perfect runs, smaller misses. That is a defensible shape for a scorer but it is a regression in the one property a decision model is supposed to guarantee: that wording does not change the answer.

Microsoft's keynote said the quiet part out loud — these models are "explicitly not yet tuned for the harder problems, like fundamental science research and very long horizon agentic work", because tuning for those capabilities would raise the price of every routine task. Decision 1.1 is what that policy looks like when it is executed. The knowledge bench got worse on purpose, or at least without objection.

## What did not change: the price card, and the 32K wall

Both versions list $0.042 per million input tokens on Global Standard, $0.046 on US Data Zone and $0.05 on EU and APAC Data Zone deployments, with output billed at nothing because there is no output. Microsoft's phrase is "more than twice as fast at the same cost", and the token price supports it exactly.

The measured cost per task says something slightly different. Version 1 comes in at $142.56 per million tasks on the index; version 1.1 at $139.64. A 2% improvement, not a doubling. The reason is that cost per task is token price times input consumed, and the new model is not consuming half the tokens — it is answering in half the time. If your constraint is a wall-clock budget or a routing step inside a request path, the upgrade is large and obvious. If your constraint is spend, 1.1 does not move your bill, and Microsoft never claimed it would.

The other unchanged number is the ceiling. Both models read 32,768 tokens, and Microsoft's documentation for 1.1 repeats the "inputs of up to 32K tokens" phrasing nearly verbatim from the v1 page. There is no caching support documented for either, which means a 200-question rubric is paid for in full on every call, on both versions, at every volume tier. That is the single largest lever on this category's economics and neither release touches it.

## How to migrate without a leap of faith

The API shape is stable enough that migration is mostly configuration, but not entirely. Microsoft documents the v1.1 call as a *state* plus a map of typed questions — *noul*, *choice*, *score* — posted to */providers/microsoft/v1/systemone* on a Foundry resource, with the *model* field carrying your deployment name rather than the model name. The response echoes back the underlying model, so a deployment named for your workload can still tell you which version answered.

Three steps worth doing in order, rather than swapping the deployment name and watching a dashboard:

1. Re-record your labelled set's confidence distribution, not its accuracy. The shift from 21.7% to 49.4% non-hallucination means 1.1 will decline a materially larger share of questions at the same threshold. If your pipeline treats a missing answer as a hard failure rather than a routing signal, this upgrade will look like a regression in production while every benchmark says it is an improvement.

2. Re-tune the threshold. A cutoff chosen on version 1's confidence scale is being applied to a distribution that moved. Microsoft's own guidance is to validate thresholds against labelled examples before automating a gate, which is the same advice as ever and now load-bearing.

3. Test the symmetry regression on your own phrasing. If your prompts are generated from templates with variable option order — and most routing prompts are — the 47.5% all-pass rate is the number to worry about, not the composite. Duplicate twenty of your questions with options reordered and measure how often the argmax moves.

What makes that third step cheap is that the abstention has somewhere to go. A decision model that declines is only useful if the thing it declines to can answer, and that is the part of the pipeline OrcaRouter carries: one key across 200-plus models with automatic failover, so a low-confidence *choice* result can be routed to a general model instead of paging a human, and the routing DSL can compose the two into a single call sequence. Gemma 4 26B A4B — the base model inside Decision 1.1 — sits on our catalogue at $0.06 per million input and $0.33 per million output, at provider list price with 0% markup passed through, which makes the cheapest possible sanity check on any suspicious decision an actual call to its own backbone.

## Whether to move at all

There is a case for staying on version 1, and it is narrow but real. If your workload is hard knowledge questions where a wrong answer is worse than no answer — the HLE profile — version 1 answered more of them. If your prompts are templated in ways that reorder options, version 1 was more stable under rewording. And if you have already tuned a threshold and validated it against labelled production data, the new confidence distribution invalidates that work.

For everyone else the case is straightforward. Twice the throughput at the same token price, a document pass rate that went from 20.9% to 36.8%, image input that did not exist before, and a model that says "I don't know" twice as often in exactly the situations where a wrong commitment is expensive. Those are the properties a decision layer is bought for. The knowledge bench it gave up was not what anyone was buying this model to do.

The migration, in one line: move to 1.1 for the latency and the abstention, re-tune your thresholds the same week, and keep version 1's numbers in a drawer for the day someone asks why the new model stopped answering a question the old one used to take.
