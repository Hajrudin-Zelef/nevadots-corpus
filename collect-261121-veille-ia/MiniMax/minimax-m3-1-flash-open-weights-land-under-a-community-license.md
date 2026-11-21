---
id: collect-261121-veille-ia/minimax/minimax-m3-1-flash-open-weights-land-under-a-community-license
title: "MiniMax M3.1 Flash open weights land under a community license"
domain: minimax
role: reference
task: article
actors: ["MiniMax"]
dates: []
keywords: ["weights", "flash", "license", "minimax", "release", "model", "community", "hosted"]
source: https://www.deai.org/news/minimax-m31-flash-open-weights-community-license
source_anchor: ""
source_lines: [1, 63]
section: "MiniMax"
---
MiniMax released the M3.1 Flash open weights on Hugging Face on schedule, and the release ends the model's hosted-only era: a 428B-parameter mixture-of-experts model with about 23 billion active parameters per token can now be self-hosted, quantized, and benchmarked by anyone. The catch is in the license file, not the download.

## Key facts

- 428B total parameters, ~23B active per token, shipped in MXFP8 on the MiniMaxAI Hugging Face org, ungated.
- The model ships under a community license with commercial-use conditions, not Apache 2.0 or MIT.
- Community GGUF conversions appeared within hours of the weights drop.
- The hosted endpoint has been priced at $0.15/$0.60 per 1M tokens in/out, with $0.015 cached input — a figure MiniMax self-reports.
- Artificial Analysis reported a 93% hallucination rate against the hosted endpoint — a claim the weight release now makes independently testable.

## What happened

MiniMax had promised an October 23 weights drop for M3.1 Flash, and the delivery landed on time. The model card on Hugging Face hosts the full checkpoint ungated: no application form, no waitlist, no terms-of-use click-through before download. Within hours, community quantizers had published GGUF conversions, which is the usual first signal that a release is real and runnable rather than a placeholder repo.

The architecture follows the pattern builders now expect from the budget-agentic tier: a very large total parameter count with a small active slice per token. At 428B total and roughly 23B active, the model trades per-token compute against a wide expert pool, which is the same shape that makes models like Kimi K3.1 Pro cheap to serve relative to their benchmark tier. The weights ship in MXFP8, a low-precision format that keeps the download size manageable but assumes serving infrastructure that can handle the format.

The license is where the release stops being simple. M3.1 Flash ships under a community license with commercial-use conditions — the same family of terms Qwen has moved toward rather than the Apache 2.0 grant that Nanbeige chose for its 4.3-33B release the same week. The conditions are readable directly in the license file on the model card. Nothing about the hosted pricing carried over: teams that served M3.1 Flash through the API at $0.15 per million input and $0.60 per million output tokens, with cached input at $0.015 — prices MiniMax publishes for its own endpoint — now have a second option whose cost structure is their own GPU bill, provided the license terms fit their use.

The third strand of the story is a claim that has hung over the hosted endpoint since before the release: Artificial Analysis measured a hallucination rate around 93% for M3.1 Flash through the API. That number is a third-party measurement of a hosted service, and hosted endpoints differ from self-hosted deployments in system prompts, sampling defaults, and scaffolding. With weights in hand, the claim is finally testable against the model itself rather than against MiniMax's serving stack. No independent re-test has been published yet.

## Why it matters

For teams deciding where to run models, this release moves M3.1 Flash from "trust the vendor's endpoint" to "verify on your own hardware." That matters most for the hallucination-rate finding: a 93% rate, if it reproduces on self-hosted weights, disqualifies the model from most production agentic work no matter how cheap it is. If it does not reproduce, the finding was an artifact of the hosted configuration, and the budget tier gets a credible new entrant. Either outcome changes buying decisions, which is why the re-test — not the release — is the real event.

The license is the second pressure point. The M3.1 Flash terms are the same pattern now visible across the Chinese labs' releases: weights are free to download, but commercial serving carries conditions that a legal review has to clear. Two other releases this month bracket the question — Mistral Large 4's weights are due within days with no license announced, and Nanbeige shipped a smaller coder under Apache 2.0 outright. If the Mistral drop also lands as a conditional community license, the practical rule for self-hosters hardens: "open weights" describes availability, not freedom, and the license file decides whether a model can anchor a commercial deployment.

## Background

MiniMax had positioned M3.1 Flash as the budget tier of its agentic lineup, with the hosted endpoint priced at $0.15/$0.60 per million tokens — prices MiniMax self-reports — since the model's API debut. The open-weights commitment was made at the same time, with October 23 as the delivery date, which made this one of the few releases in the tier to ship exactly when the vendor said it would.

The release fits into a two-week stretch that is reshaping the self-hosting menu. Mistral AI's Large 4, a trillion-parameter MoE, is expected in open-weight form by October 31. Moonshot AI has promised Kimi K3.1 Pro weights around November 4. Both of those drops face the same open question M3.1 Flash just answered for itself: what the license says. DeAI's open-weights coverage has tracked the tightening pattern since the Qwen image-model license change, and M3.1 Flash is the latest data point in that direction.

The hallucination-rate claim has its own history. Artificial Analysis runs standardized evals across hosted endpoints, and its 93% finding circulated widely enough that MiniMax's budget positioning looked fragile while the weights stayed unavailable. The distinction that matters now: a hosted endpoint's hallucination rate reflects the provider's serving configuration, and a weights release breaks the link between the vendor's stack and the model's measured behavior.

## What's next

The expected next news cycle is the independent re-test: benchmark groups and quant maintainers running hallucination and agentic evals against the released weights rather than the hosted endpoint. Watch for whether the 93% figure survives contact with the actual model. On the licensing side, read the community license conditions against your own deployment before committing serving capacity, and watch October 31 for Mistral Large 4's weights — the license terms Mistral publishes will tell you whether this month's conditional-license pattern holds or breaks.

Questions

- Are the MiniMax M3.1 Flash weights really open?
- The weights are downloadable on Hugging Face without an access gate, but the model ships under a community license with commercial-use conditions rather than Apache 2.0 or MIT. Teams planning commercial serving should read the license file on the model card before committing.
- How big is MiniMax M3.1 Flash and can one GPU run it?
- It is a 428B-total-parameter mixture-of-experts model with roughly 23 billion active parameters per token, shipped in MXFP8. Community GGUF quantizations appeared within hours of release; the largest quants still exceed single-consumer-GPU memory, so plan for multi-GPU or heavy quantization.
- How much does MiniMax M3.1 Flash cost through the hosted API?
- MiniMax's hosted endpoint has been priced at $0.15 per million input tokens and $0.60 per million output tokens, with cached input at $0.015 — figures MiniMax self-reports. Self-hosting cost now depends on your GPU fleet and the license terms instead.
- What was the MiniMax M3.1 Flash hallucination rate claim?
- Artificial Analysis reported a roughly 93% hallucination rate for the hosted M3.1 Flash endpoint. That figure is a third-party measurement of the hosted service, not of the released weights — and the weight release is what first makes an independent re-test possible.

Sources

1. MiniMax M3.1 Flash model card and license (Hugging Face) — Hugging Face (MiniMaxAI)
2. MiniMax M3.1 Flash community GGUF conversions — Hugging Face (community)
3. Artificial Analysis hallucination-rate finding against the hosted M3.1 Flash endpoint — Artificial Analysis
4. Superpower Daily coverage of the M3.1 Flash weights release — Superpower Daily

About DeAI

DeAI is an independent publication covering open-weight AI models, private inference, and decentralized infrastructure — the tools for running AI you actually control. We test providers on price, privacy, and refusal behavior and publish the numbers, not the vibes. DeAI is powered by Morpheus (mor.org), a decentralized inference marketplace, and covers it on the same terms as every other provider.

Powered by Morpheus and StrandCMS

Morpheus is a decentralized inference marketplace, covered on the same terms as every other provider — we rank it wherever the data lands. StrandCMS is the open-source, agent-first framework this site is built on.
