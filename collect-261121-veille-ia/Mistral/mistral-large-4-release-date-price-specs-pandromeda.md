---
id: collect-261121-veille-ia/mistral/mistral-large-4-release-date-price-specs-pandromeda
title: "Mistral Large 4: Release Date, Price, Specs | Pandromeda"
domain: mistral
role: reference
task: article
actors: ["Mistral AI"]
dates: []
keywords: ["mistral", "large", "date", "model", "release", "october", "preview", "window"]
source: https://pandromeda.com/ai/mistral-large-4-release-date-price-and-specs-explained
source_anchor: ""
source_lines: [1, 132]
section: "Mistral"
---
# Mistral Large 4: Release Date, Price and Specs Explained

Mistral AI put a 1-trillion-parameter, open-weight Mistral Large 4 into public preview on Oct. 6, 2026, with a 1M-token context window and open weights due by month end.

Mistral AI opened a public preview of **Mistral Large 4** on October 6, 2026 — a trillion-parameter, open-weight, natively multimodal Mixture-of-Experts model that the company is pitching for coding, AI agents, financial and legal analysis, and image-heavy work like satellite and engineering-drawing review. The preview is live now through the Mistral Studio API with a 1-million-token context window and per-token pricing of $1.36 (input) and $4.18 (output) per million tokens; Mistral says it will publish the open weights "by the end of the month," which several outlets have reported as October 27, 2026, though Mistral's own post does not name an exact day.

### Key facts

- **Announced:** October 6, 2026, as a public preview
- **Weights release:** "End of this month" per Mistral; widely reported as October 27, 2026 (unconfirmed by Mistral directly)
- **Parameters:** ~1 trillion total; Mistral's launch post says 49 billion active, while Mistral's own docs site lists 1.05 trillion total / 52 billion active plus a 1.6 billion-parameter vision encoder
- **Context window:** 1 million tokens (per docs.mistral.ai)
- **Architecture:** Open-weight, hybrid instruct-and-reasoning Mixture of Experts with multimodal (vision) input
- **Pricing:** $1.36 / $4.18 per million input/output tokens (standard); roughly half that on the batch API
- **Availability:** Mistral Studio API now, in multiple regions worldwide including a sovereign European deployment
- **Nickname:** Internally and in Mistral's own asset filenames, "Le Chonk"

## What Mistral Large 4 actually is

Mistral Large 4 — ML4 for short, or "Le Chonk" as Mistral calls it in its own launch copy and in the icon file name shipped on its docs site — is the French lab's new flagship model. Mistral describes it as "a 1 trillion-parameter natively multimodal model" built on a Mixture-of-Experts (MoE) architecture, meaning the full parameter count is split across many specialized sub-networks ("experts") and only a fraction of them activate for any given token, which keeps inference cost down relative to a dense model of similar scale.

This is a notable jump from Mistral's prior flagship. Mistral's own announcement frames ML4 as "our largest and most capable model to date," succeeding Mistral Large 3 (675 billion total / 41 billion active parameters, released in December 2025). Mistral says ML4 is still being refined post-launch and "continues to improve rapidly" even during the public-preview window.

## Release date: what's confirmed and what's still a rumor

The confirmed facts, straight from Mistral: the model entered public preview on October 6, 2026, accessible through the Mistral Studio API, and the open weights are coming "by the end of the month" — Mistral's own wording, with no specific calendar date attached in the post itself. Multiple outlets covering the launch have cited October 27, 2026 as the target date for the weight drop, attributed to Reuters reporting, but that specific day does not appear on Mistral's own blog or documentation as of this writing. Anyone tracking the release for deployment planning should treat "late October 2026" as the safe window and the 27th as a reported-but-unconfirmed specific date.

Until the weights ship, the only way to run ML4 is through Mistral's hosted API in Mistral Studio, which is also where Mistral says the public preview is being served — on the same infrastructure used to train the model.

## Specs: parameters, architecture and context window

Mistral's two primary sources give slightly different numbers, which is worth flagging rather than papering over. The launch blog post states ML4 is "a 1 trillion-parameter natively multimodal model with 49 billion active parameters." Mistral's documentation site, in the metadata for the Mistral Large 4 model page, describes it more precisely as featuring "1.05T total parameters" and "52B active parameters," plus a separate 1.6-billion-parameter vision encoder bolted on for image understanding. Both figures come directly from Mistral; the gap looks like ordinary rounding between a marketing post and a spec sheet rather than a real discrepancy, but we're citing both rather than picking one and presenting it as the only number.

| Spec | Mistral Large 4 (ML4) | Mistral Large 3 (prior flagship) | 
|---|---|---|
| Total parameters | ~1 trillion (1.05T per docs.mistral.ai) | 675 billion | 
| Active parameters | 49B (blog) / 52B (docs) | 41 billion | 
| Vision encoder | 1.6 billion parameters | Natively multimodal, no published encoder figure | 
| Context window | 1,000,000 tokens | ~256,000 tokens | 
| Architecture | Open-weight hybrid instruct-and-reasoning MoE | Granular Mixture-of-Experts | 
| Status (Oct 7, 2026) | Public preview; weights due "end of" October 2026 | Generally available, Hugging Face weights live | 

The context window — 1 million tokens, according to the pricing table on Mistral's docs site — is the headline spec for anyone doing long-document or large-codebase work: it's roughly a fourfold increase over Mistral Large 3's published window and puts ML4 in the same bracket as the largest-context frontier models from other labs.

## Pricing and availability

Mistral's own models-and-pricing documentation lists two pricing tiers for ML4 on the Studio API: a standard rate and a discounted batch rate (for asynchronous, non-latency-sensitive jobs), each roughly half the standard price.

| Token type | Standard price | Batch API price | 
|---|---|---|
| Input | $1.36 / million tokens | $0.68 / million tokens | 
| Cached input | $0.14 / million tokens | $0.07 / million tokens | 
| Output | $4.18 / million tokens | $2.09 / million tokens | 

Mistral says the model "will be available across multiple regions worldwide, including a European deployment that Mistral operates end-to-end, independently of other digital service providers and under European law" — language the company has used consistently as it pitches ML4 partly as a sovereignty play for European enterprises and governments that don't want to depend on US or Chinese infrastructure.

On licensing, Mistral's docs currently tag the model simply as "Open," without yet naming the specific license (Mistral Large 3 shipped under Apache 2.0). Since the weights themselves aren't out yet, the exact license terms will presumably be confirmed when they land.

## Benchmark claims, according to Mistral

All of the following are Mistral's own reported numbers from its launch post, not independently reproduced by anyone outside the company as of this writing:

- **Coding:** 61.7% on DeepSWE v1.1; 28.3% on Terminal-Bench 4.0; a 3.74/5 in blind human evaluation against other coding models.
- **Agentic workflows:** 59.9% on AutomationBench; 1,393 Elo on AA-Briefcase, a long-horizon knowledge-work benchmark that Mistral says puts it ahead of DeepSeek V4 Pro.
- **Cybersecurity:** 82% on a vulnerability-reproduction test and 93% on Cybench — scores Mistral calls "one of the highest reported for an open-weight model." Mistral also claims Claude Opus 5.5 and GPT-6 Astra "score near zero on the same test because they refuse to perform the task," framing ML4's lower refusal rate as a feature for defensive security research rather than a safety gap.
- **Finance and legal:** Mistral says ML4 outperforms GPT-6 Astra on its Finance Agent v2 and FinWorkBench evaluations, and beats "all open-source models" on Harvey's Legal Agent Benchmark.
- **Vision / visual grounding:** 42% vs. 41% against GPT-6 Astra on a benchmark Mistral calls Dense 200.
- **Safety:** 93.3% resistance on an internal "B3" attack-resistance test and a 1.691-out-of-2.0 score on something Mistral calls the KORA benchmark.

Treat the GPT-6 Astra comparisons as one lab's self-reported numbers against a rival's model rather than a neutral third-party leaderboard result; Pandromeda has covered what actually changed between GPT-6 Sol and Astra if you want the other side of that comparison.

## Coding and AI-agent use cases

Mistral is positioning ML4 heavily around software engineering and agentic tool use: repository-scale code understanding, terminal-based coding agents, and long-horizon tasks that chain multiple tool calls together. The million-token context window matters here specifically because it lets the model ingest much larger codebases or longer conversation histories in a single pass before it needs to summarize or truncate anything.

For readers building agents on top of ML4, Mistral's own tool-calling and agent-orchestration setup is conceptually similar to the broader shift toward standardized tool protocols that Pandromeda has written about in its explainer on the Model Context Protocol — the open standard several labs are now using to let models call external tools and data sources in a consistent way. On the coding side specifically, developers comparing agentic coding tools across vendors may also want Pandromeda's walkthrough of Claude Code, Anthropic's terminal-based coding agent, as a point of reference for how a rival approach handles the same repository-scale, tool-calling workflows ML4 is being pitched for.

## Financial, legal and geospatial use cases

Beyond coding, Mistral's launch post leans on three industry-specific pitches. For finance and legal teams, Mistral points to its Finance Agent v2 and Harvey legal-benchmark results and says ML4 is tuned for "professional deliverables that knowledge work actually produces: spreadsheets, slides, and PDFs." For vision-heavy fields, Mistral says ML4 "brings vision to the industries where perception is critical such as engineering, manufacturing, and earth observation," describing use cases like inspecting gigapixel satellite imagery for disaster response, verifying mechanical parts against engineering drawings, and retrieving evidence from dense PDFs.

One correction worth making explicitly: Mistral's own materials do not use the phrase "chip design." The closest documented use case is image-based verification of engineering drawings and mechanical parts — adjacent to, but not the same as, semiconductor layout work. (Separately, Mistral's CEO has talked publicly about the company exploring custom AI chips for its own infrastructure, which is a different story entirely from what ML4 the model is being marketed to do.) Readers should treat "chip design" as an overstatement of what Mistral has actually claimed for this model.

## How Mistral Large 4 stacks up against rivals

Every head-to-head number in Mistral's launch post is measured against GPT-6 Astra, OpenAI's current flagship model, plus occasional mentions of Claude Opus 5.5 and DeepSeek V4 Pro. Mistral claims wins on visual grounding, finance, and legal-agent benchmarks, and a notably higher score on its cybersecurity vulnerability-reproduction test — though that specific result is driven as much by competitors' safety refusals as by raw capability, according to Mistral's own explanation.

What's missing from the picture, at least for now, is independent verification. Every benchmark cited in this article comes from Mistral's own post; third-party leaderboards and open evaluations will need the actual weights in hand — expected by the end of October 2026 — before outside researchers can reproduce any of these numbers.

## European sovereignty and the training story

Mistral is leaning into a "built in Europe, for AI sovereignty" narrative around this launch. The company says ML4 "was trained from scratch on 3,800 NVIDIA Grace Blackwell GPUs in Mistral's own datacenters in Europe," and that the public preview is being served on that same infrastructure rather than a third-party cloud. Mistral also says a meaningful share of ML4's training data was multilingual, "spanning more than 160 languages, including every official language of the European Union" — consistent with the company's long-running pitch to European enterprises and governments that want an alternative to US and Chinese model providers for data-residency and regulatory reasons.

## What's next

The near-term calendar is simple: the public preview is live now on the Mistral Studio API, and Mistral has committed to releasing the open weights before November — "by the end of the month" in the company's own words, with late-October reporting (unconfirmed by Mistral itself) pointing to October 27, 2026 specifically. Once the weights are out, expect the usual follow-on activity: independent benchmark reproductions, and quantized and fine-tuned community variants showing up on Mistral's Hugging Face organization page, alongside a proper model card with the kind of granular technical detail — exact license, training-data breakdown, safety card — that a same-day launch post typically doesn't include. Pandromeda will update this coverage once the weights, and the finalized license, are actually public.

## Frequently asked questions

### When does Mistral Large 4 come out?

Mistral opened a public preview of Mistral Large 4 on October 6, 2026 through the Mistral Studio API. Mistral says the open weights will follow 'by the end of the month' (October 2026); some press reports cite October 27, 2026 specifically, though Mistral's own post does not name that exact date.

### How much does Mistral Large 4 cost?

Per Mistral's own pricing documentation, standard Studio API rates are $1.36 per million input tokens and $4.18 per million output tokens, with cached input at $0.14 per million tokens. Batch API pricing is roughly half those rates.

### What is Mistral Large 4's context window?

1 million tokens, according to Mistral's documentation — about four times the roughly 256,000-token window of the prior Mistral Large 3.

### How many parameters does Mistral Large 4 have?

Mistral's launch post describes it as a 1 trillion-parameter model with 49 billion active parameters; Mistral's documentation site separately lists 1.05 trillion total and 52 billion active parameters plus a 1.6 billion-parameter vision encoder. Both figures come from Mistral.

### Is Mistral Large 4 open-weight or open-source?

It's open-weight: Mistral plans to release the model weights publicly by the end of October 2026. Mistral's docs currently tag its license simply as 'Open' without naming a specific license yet; its predecessor, Mistral Large 3, shipped under Apache 2.0.

### How does Mistral Large 4 compare to GPT-6 Astra?

Mistral claims wins over GPT-6 Astra on its own visual-grounding (Dense 200), finance, and legal-agent benchmarks. These are Mistral's self-reported numbers; independent verification will require the public weights, due by the end of October 2026.

## Sources

- Mistral AI — Mistral Large 4 announcementmistral.ai
- Mistral AI Docs — Mistral Large 4 model pagedocs.mistral.ai
- Mistral AI Docs — Models overview & pricingdocs.mistral.ai
- Mistral AI — Studio platformmistral.ai
- Mistral AI on Hugging Facehuggingface.co

Theo Park runs the AI desk at Pandromeda. He follows model launches from the frontier labs and the open-weight community, tracks the assistants and developer tools built on them, and explains what each release changes on pricing, capability and safety. His reporting leans on primary sources: model cards, technical reports, API documentation and the companies' own announcements.
