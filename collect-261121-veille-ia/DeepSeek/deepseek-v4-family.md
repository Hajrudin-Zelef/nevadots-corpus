---
id: collect-261121-veille-ia/deepseek/deepseek-v4-family
title: "DeepSeek V4 family"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: ["2026-10-02"]
keywords: ["deepseek", "v4-pro", "flash", "family", "active", "card", "v4-flash", "release"]
source: https://www.thirdbrain.tech/KB-AI/ai-models/language-models/deepseek-family
source_anchor: ""
source_lines: [1, 52]
section: "DeepSeek"
---
# DeepSeek V4 family

By DeepSeek (company note: deepseek); succeeded V3.2 (deepseek-v3-dot-2) with the V4 line, all MIT-licensed per the Hugging Face cards. This note is also the model overview for the family (it absorbed the former `deepseek` model-overview note, 2026-10-02).

## Timeline (DeepSeek API changelog, primary: api-docs.deepseek.com/updates, checked 2026-10-02)

- **2026-04-24** DeepSeek-V4 preview:`deepseek-v4-pro` and`deepseek-v4-flash` , usable through OpenAI ChatCompletions and Anthropic-compatible interfaces. Third-party summaries give V4-Pro about 1.6T total / 49B active and V4-Flash 284B total (secondary).
- **2026-07-31** V4-Flash update (official public beta; native Responses API format).
- **2026-08-13** V4-Pro update (`0813` ): GA across app, web and API; low/high/max thinking effort; peak/off-peak pricing.
- **2026-09-10** DeepSeek-V4.1-Flash: 552B MoE with a Causal Encoder-Decoder architecture, 8B active for input and 16B for output, native multimodal; API name`deepseek-flash` ;`deepseek-v4-flash` and`deepseek-v4-flash-vision-exp` retired/routed to it; KV cache reduced to about 1/4 of V4-Flash HBM. DeepSeek says multiple tests put it ahead of V4-Pro. Its launch post said V4-Pro requests would route to V4.1-Flash from 2026-09-14; the changelog later says V4-Pro service continues beyond 2026-09-14 with unchanged billing, with no date for a V4.1-Pro.

## Models

| Model | Size | Notes | 
|---|---|---|
| DeepSeek-V4-Pro-0813 | 1.7T total (active not stated) | “Official release of V4-Pro”, supersedes the preview; 1M context, 384K max output recommended; low/high/max reasoning; DSpark speculative decoding. Card: HLE with tools 60.0, Terminal Bench 2.1 87.9, DeepSWE 62.7 (vendor) | 
| DeepSeek-V4-Flash-0731 | ~304B | Most downloaded (4.47M); Aug 1 update | 
| DeepSeek-V4-Flash-Vision-Exp | ~305B | multimodal experiment | 
| DeepSeek-V4.1-Flash | 552B backbone, 8B active at prefill / 16B at decode (card and DeepSeek release post agree; the HF org listing shows 763B, probably including the vision encoder, unverified) | 1M context; image+text; Compressed Sparse Attention 2 cuts global KV cache to 890 bytes/token (~1/4 of V4-Flash); MIT | 

Card benchmarks for V4.1-Flash (vendor, not independently reproduced): DeepSWE v1.1 74.2%, Terminal-Bench 2.1 90.6%; 45T-token multimodal pre-training; reasoning effort 1-100.

## Not found

No R2 or other reasoning-only successor was found (searched 2026-09-30 and 2026-10-02). Release dates are now taken from DeepSeek’s changelog.

## Unverified points

- V4-Pro active parameters: the card gives none; 1.6T/49B (preview) is from secondary sites and the card says 1.7T for the 0813 release.
- Whether the 763B Hugging Face figure for V4.1-Flash includes the vision encoder is unverified. Current prices are not recorded (they vary with peak/off-peak).

## Hands-on coverage of V4 (YouTube, 2026-04; creator claims, not benchmarks)

- Income Stream Surfers (2026-04-24) ran V4 Pro through OpenCode (via OpenRouter) with the Superpowers plugin and judged landing-page and SVG output close to Claude Opus 4.7, at a very low cost for the test session; V4 Flash was reported to struggle with complex multi-step prompts and suited cheap experiments.
- Jack Roberts (2026-04-30) paired V4 with Claude Code and described the pipeline as roughly 100x cheaper than Anthropic models.
- Access routes mentioned: DeepSeek API, OpenRouter, Hugging Face open weights.

## Earlier generation: V3.2-Exp

DeepSeek V3.2-Exp (announced late Sept 2025, ~671B parameters) introduced DeepSeek Sparse Attention (DSA) with 50%+ API price cuts; it is superseded by the V4 line. Details: deepseek-v3-dot-2.

## Related

- DeepSeek V3.2-Exp (superseded predecessor), deepseek-harness, multi-token-prediction

## Sources

- https://api-docs.deepseek.com/updates/ and https://api-docs.deepseek.com/news/news260910/ (accessed 2026-10-02)
- https://huggingface.co/deepseek-ai (accessed 2026-09-30)
- https://huggingface.co/deepseek-ai/DeepSeek-V4.1-Flash (accessed 2026-09-30)
- https://huggingface.co/deepseek-ai/DeepSeek-V4-Pro-0813 (accessed 2026-09-30)
- https://www.youtube.com/watch?v=aIY1pxmzlVQ (Income Stream Surfers, 2026-04-24) and https://www.youtube.com/watch?v=tn7zXRv3Xmo (Jack Roberts, 2026-04-30)
