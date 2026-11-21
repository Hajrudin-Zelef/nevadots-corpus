---
id: collect-261121-veille-ia/deepseek/deepseek-v4-1-flash
title: "DeepSeek V4.1-Flash"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: []
keywords: ["flash", "deepseek", "off-peak", "ai-digest", "cached-input", "rates", "active", "multimodal"]
source: https://aidigest.shelfcritter.com/topics/deepseek-v4-1-flash
source_anchor: ""
source_lines: [1, 25]
section: "DeepSeek"
---
# DeepSeek V4.1-Flash

## Overview

DeepSeek V4.1-Flash is a 552B-parameter MoE model with ~8B active per token, MIT-licensed weights on HuggingFace, native multimodal (image-in, text-out), and a 1M-token context. Trained on 45T mixed text-and-image tokens; priced at $0.15/M input and $0.60/M output (off-peak) on the official API with cached-input as low as $0.003/M. A MoE-style refresh of the V4 family rather than a novel architecture — the practitioner takeaway is memory-and-cost efficiency for agent inference loops. See also the earlier DeepSeek-V4-Flash for the prior V4-family Flash variant.

## Timeline

- 
2026-09-11-AI-Digest — DeepSeek released V4.1-Flash on Sep 10 — 552B MoE / ~8B active, MIT weights on HuggingFace, 1M context, multimodal, 45T tokens. Off-peak pricing $0.15/M input, $0.60/M output; cached-input as low as $0.003/M. DeepSeek’s own benchmark table claims narrow leads on Terminal-Bench 2.1, DeepSWE, AutomationBench, ALE, and CyberGym against GPT-5.6 Sol, Claude Fable 5.1, and Opus 5 — but no LMArena or Simon Willison independent evaluation has yet surfaced, so the frontier-parity claim rides on DeepSeek-selected agentic-coding benchmarks and off-peak cached-input rates. Carry as frontier-grade agent-inference-cost floor lowers again on published DeepSeek evals — awaiting independent verification.
- 
2026-09-14-AI-Digest — **The API cutover landed at 04:00 UTC today — every DeepSeek V4-Pro call now reroutes to V4.1-Flash at Flash pricing until V4.1-Pro ships.** Bloomberg Intelligence’s per-token math puts the effective cut at**as much as 32%** (off-peak output at $0.60/M tokens, cached input as low as $0.003/M, peak rates 2×), reversing the August V4-Pro hike that followed DeepSeek’s coding-agent-rival launch.**MiniMax and Z.ai fell >8% in Hong Kong on the news; Alibaba closed -2%.** This is the operational-cost side of the 2026-09-11-AI-Digest release — the model didn’t change today, but the price floor for anyone still paying V4-Pro rates actually collapses at 04:00 UTC. The Aider polyglot top-5 still doesn’t list a DeepSeek entry inside the 88.0–81.3% band gpt-5 and o3-pro occupy — the frontier-parity claim rides on DeepSeek-selected agentic-coding benchmarks and off-peak cached-input rates until independent evals surface. Reframe worth carrying:`frontier-grade agent-inference-cost floor keeps lowering on DeepSeek-selected benchmarks and off-peak cached-input rates` , not`V4.1-Flash is now the price-quality frontier` . Log against MOC - Open Source Models and MOC - AI Infrastructure.
- 
2026-09-18-AI-Digest — **Technical paper drops: “DeepSeek-V4.1-Flash: Pushing the Limits of KV Cache Compression”** (arXiv:2609.19969, ▲14). Details the**Causal Encoder-Decoder** architecture: 552B params total, only**8B active at prefill / 16B active at decode** ;**CSA2 cross-layer KV reuse + FP4 caching** cut the HBM KV footprint to**890 bytes/token** — roughly**1/4** of V4-Flash — and**SWA Bounded Replay** cuts the persistent footprint further to**~1/8** . Trained on a**45T-token multimodal corpus** ; checkpoint already on Hugging Face Hub. Load-bearing framing to carry:**paper is the technical companion to the 2026-09-14-AI-Digest cutover** — the storage/bandwidth bottleneck that dominates long-horizon agent economics now has a concrete number attached (890 bytes/token, ~1/4 → ~1/8 of prior generation). Reads with the Bloomberg Intelligence 32% effective price cut as the*mechanism-plus-market-reaction* pair. Log against MOC - Open Source Models and MOC - AI Infrastructure.
- 
2026-09-19-AI-Digest — **The V4.1-Flash arXiv paper (2609.19969) hits ▲63 on HuggingFace today (up from ▲14 on Sep 18)** — practitioner scale-out of the technical companion piece to the Sept 14 API cutover. Corpus framing worth carrying: this is the**first frontier-lab paper that treats the memory-bandwidth-per-token axis as the real 1M-context lever** , with concrete numbers practitioners can copy into inference-cost planning — 552B multimodal MoE, Causal Encoder-Decoder activating 16B at decode / 8B at prefill,`CSA2` cross-layer KV reuse stacked on FP4 KV caching, ~890 bytes/token global KV footprint (~1/4 of V4-Flash, ~1/8 with SWA Bounded Replay). Read paired with Anthropic’s Sept 18 metrics post and the Anthropic-Accenture pact: the frontier labs are simultaneously formalising external evaluation*and* publishing model-productivity numbers, raising the reputational cost of the “black-box lab” framing while stopping short of transparency mandates. Log against MOC - Open Source Models and MOC - AI Infrastructure.
- 
2026-10-09-AI-Digest — **HN front-page thread “Why isn’t the industry freaking out about DeepSeek 4.1 Flash?” (`523` pts / `436` cmts) argues V4.1 Flash is being under-reacted-to given its price/capability curve.** The`436` comments split on whether frontier labs are now structurally undercut on cost. Load-bearing softener the digest carries: the “structurally undercut” claim is HN sentiment, not benchmark-anchored — V4.1 Flash has no public Aider polyglot slot, and Anthropic‘s Claude Haiku 5.5 pricing reset earlier this week pushes back on the frame directly. Reframe worth carrying:`HN sentiment flags V4.1 Flash as under-reacted-to on price/capability, but the claim is not benchmark-anchored against this week's small-tier moves` .
- 
2026-10-18-AI-Digest — **DeepSeek posted a `-1017` point release of V4.1-Flash over the weekend across HuggingFace, ModelScope, the DeepSeek web/app, and the developer platform — same `763B`-parameter MIT-licensed `F8_E4M3` architecture, `1M` context, with a refreshed checkpoint positioned on agent coding.** DeepSeek’s own numbers (vendor-reported, not independently confirmed):`77.0%` on DeepSWE at`<$0.20/task` vs the September V4.1-Flash’s`74.2%` at`~$0.50/task` , with claimed wins over Claude Opus 5, GPT-5.6-Sol, Kimi K3, and GLM 5.3 across coding/agent categories. New Search + Fetch APIs priced at`$0.75/1k` calls per endpoint, with free access through DeepSeek Harness, Claude Code, and Codex integrations (duration unverified in open coverage — The Decoder says “free for now”). Load-bearing softener:**every performance figure here is DeepSeek-reported** . Reframe worth carrying:`this isn't a new base model — it's a point-release of a September release — but the pattern (open weights + aggressive per-task-cost cut + Search/Fetch tool pricing) extends the supply-side-pressure beat the digest has been tracking` . Log against MOC - Open Source Models and MOC - AI Infrastructure.

## Key Developments

1. **V4.1-Flash release (Sep 2026)** : 552B MoE with ~8B active per token, MIT weights, 1M context, native multimodal — a MoE-style refresh of the V4 family, not a novel architecture. Trained on 45T tokens. Pricing lands memory-and-cost efficiency for agent inference loops as the load-bearing practitioner takeaway; independent third-party benchmark corroboration still pending on Claude Fable / Opus 5 comparisons.
2. **KV Cache Compression Paper (Sep 2026)** : arXiv:2609.19969 documents the Causal Encoder-Decoder architecture and the CSA2 cross-layer KV reuse + FP4 caching stack that drops KV footprint to ~890 bytes/token (~1/4 of V4-Flash, ~1/8 with SWA Bounded Replay). First frontier-lab paper treating memory-bandwidth-per-token as the real 1M-context lever — concrete inference-cost-planning numbers practitioners can copy.
