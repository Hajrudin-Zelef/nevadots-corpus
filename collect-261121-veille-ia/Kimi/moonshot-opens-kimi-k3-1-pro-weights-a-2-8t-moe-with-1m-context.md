---
id: collect-261121-veille-ia/kimi/moonshot-opens-kimi-k3-1-pro-weights-a-2-8t-moe-with-1m-context
title: "Moonshot Opens Kimi K3.1 Pro Weights: A 2.8T MoE With 1M Context"
domain: kimi
role: reference
task: article
actors: ["Moonshot AI"]
dates: []
keywords: ["kimi", "moonshot", "weights", "model", "context", "open", "hugging", "face"]
source: https://i10x.ai/news/moonshot-kimi-k3-1-pro-open-weights
source_anchor: ""
source_lines: [1, 95]
section: "Kimi"
---
Moonshot AI has put the weights of Kimi K3.1 Pro on Hugging Face. The company posted the release on X on November 4, and the model card at moonshotai/Kimi-K3.1-Pro lists a 2.8-trillion-parameter Mixture-of-Experts (MoE) model with 104 billion active parameters per token, a 1,048,576-token context window, and native image input via MoonViT-V2. It is the open-weight drop Moonshot promised when K3.1 Pro launched in late October, and it is a different story from the earlier Kimi K3 technical report covered on i10x as Kimi K3.

## Quick Take

1. **What:** Moonshot published full open weights for Kimi K3.1 Pro on Hugging Face on November 4, 2026.
2. **Scale:** 2.8T total / 104B active MoE, 93 layers, 896 experts (16 selected + 2 shared), MXFP4 weights, ~1.56 TB across 96 shards, text + image.
3. **License catch:** Commercial use is allowed, but Model-as-a-Service operators above $20M annual revenue need a separate Moonshot agreement; large products must display the model name.
4. **Why it matters:** Another frontier-class open checkpoint enters the self-host and hoster market, with API remaining the practical path for most teams.

## What happened

Kimi.ai (@Kimi_Moonshot) posted: “Releasing the model weights of Kimi K3.1 Pro” with a link to Hugging Face, adding “We’ll keep advancing open intelligence.” Coverage such as GIGAZINE notes that the model itself shipped around October 24 with a commitment to open the weights by November 4. The Hugging Face tree shows 96 safetensors shards. Inference recipes already exist for vLLM (0.34.0+) and SGLang; Moonshot also lists TokenSpeed and an OpenAI/Anthropic-compatible API at platform.kimi.ai under the id `kimi-k3.1-pro`.

## Background: from K3 to K3.1 Pro

Kimi K3 was Moonshot’s earlier open MoE technical report (2.8T-class architecture with Kimi Delta Attention). K3.1 Pro is described on the model card as post-trained from Kimi K3 with large-scale joint reinforcement learning across hundreds of downstream domains, and as the first model in the K3.1 series. Versus K3, Moonshot says it replaced full-attention Gated MLA layers with Gated DeepSeek Sparse Attention (Gated DSA) on 24 attention layers for more efficient long-context modeling. Vision comes from MoonViT-V2 (401M parameters). Quantization is MXFP4 weights / MXFP8 activations from quantization-aware training.

## How it works (and what hosting looks like)

Architecturally, each token selects 16 of 896 routed experts plus 2 shared experts. Context length is 1,048,576 tokens. The card recommends vLLM, SGLang and TokenSpeed. Community posts and Featherless AI note the checkpoint is about 1.56 TB on disk; vLLM recipes describe TP8 on two GB300 trays or one B300 node for the MXFP4 checkpoint. For most builders, the hosted API remains the path of least resistance. Reasoning effort is configurable (`low`, `high`, `max`, default `max`), and multi-turn tool use requires passing back full assistant messages including `reasoning_content`.

| Spec | Kimi K3.1 Pro (model card) | 
|---|---|
| Total / active parameters | 2.8T / 104B | 
| Experts | 896 routed (16/token) + 2 shared | 
| Context | 1,048,576 tokens | 
| Modalities | Text, Image (MoonViT-V2, 401M) | 
| Quantization | MXFP4 weights / MXFP8 activations | 
| Approx. weight size | ~1.56 TB (96 shards) | 
| Terminal-Bench 4.0 (vendor) | 65.7 | 
| GPQA-Diamond (vendor) | 94.2 | 

## License: open, with commercial gates

The Kimi K3.1 Pro License grants broad rights to use, modify, fine-tune and distribute, with conditions. Model-as-a-Service operators whose aggregate revenue exceeds $20M over any consecutive 12 months must sign a separate Moonshot agreement before commercial use of the Software or derivatives. Products or services with more than 100 million monthly active users, or more than $20M monthly revenue, must prominently display “Kimi K3.1-Pro”. Internal use and use through Moonshot’s own products or certified partners are carved out. This is not a pure Apache-style free-for-all for large hosts.

## Pros and cons

### For users

- **Pro:** Another strong open multimodal option appears in chat and coding products that choose to host it; Moonshot’s card claims competitive scores on several agent and coding benches.
- **Con:** You will mostly meet it through an API or a hoster. A 1.56 TB checkpoint is not a laptop download.

### For developers

- **Pro:** Open weights, vLLM/SGLang recipes, and an OpenAI/Anthropic-compatible API make integration straightforward; 1M context and image input expand agent designs.
- **Con:** Self-hosting needs multi-node Blackwell-class GPUs; license Section 2 bites if you grow a MaaS business past $20M revenue.

### For companies

- **Pro:** A credible open alternative to closed frontier APIs for coding and agent workloads, with on-prem control when the budget allows.
- **Con:** Benchmarks on the card are vendor-reported with harness footnotes; procurement should demand independent evals. Large commercial hosts need legal review of the MaaS clause.

## What X is saying

The official drop and a few early hoster reactions set the tone: celebration of open weights, realism about hardware.

Releasing the model weights of Kimi K3.1 Pro: https://t.co/tJXcYUeSCj We’ll keep advancing open intelligence.

— Kimi.ai (@Kimi_Moonshot) November 4, 2026 · View on X

Kimi-K3.1-Pro is now open-weights

— Lisan al Gaib (@scaling01) November 4, 2026 · View on X

Kimi K3.1 Pro weights have been posted on Hugging Face.

— Andrew Curran (@AndrewCurran_) November 4, 2026 · View on X

Moonshot AI just open-sourced Kimi K3.1-Pro today. At 2.8 trillion total parameters (104B active per token) and 1.56 TB on disk, Kimi K3.1-Pro is one of the largest open-weight multimodal models ever released.

— Featherless AI (@FeatherlessAI) November 4, 2026 · View on X

## What Grok says

We did not find a dedicated Grok analysis thread on the weight drop at the time of writing. Broader commentary on open Chinese frontier models this week has stressed the same split: the open ecosystem gains a serious checkpoint, while practical access for most teams still runs through APIs and specialized hosters.

## Critical analysis

Three caveats. First, scale: 1.56 TB and multi-node serving mean “open” is not the same as “widely runnable.” Second, evaluation: Moonshot’s card reports strong numbers (for example Terminal-Bench 4.0 at 65.7 with the Claude Code harness, BrowseComp 92.1, GPQA-Diamond 94.2) but many rows use internal benches, specific harnesses, or vendor-max settings; treat them as claims until third parties reproduce them. Third, the license is deliberately commercial-aware. Open weights with a $20M MaaS threshold is a competitive strategy, not altruism alone. Still, shipping the weights on the promised day matters in a market where many “open” announcements stay gated.

## What’s still open

- Will independent labs reproduce the headline coding and agent scores?
- How do Ollama, Databricks, vLLM and hosters price and rate-limit production serving?
- Will Moonshot publish a fuller K3.1 technical report beyond the model card?
- How will Western closed labs respond on price and open-weight strategy?

## About the analysis

This independent, research-based i10x analysis is based on the Hugging Face model card and license, Moonshot’s X announcement, vLLM recipes, GIGAZINE coverage and posts on X from November 4–5, 2026. Benchmark figures are as reported by Moonshot unless noted. More on Moonshot. Related: Kimi K3 technical report.

## i10x Perspective

The open-weight race at trillion-parameter scale is no longer theoretical. Kimi K3.1 Pro’s drop is as much about distribution strategy as about architecture: Moonshot keeps the API business, lets smaller teams fine-tune and experiment, and forces large hosts to pick up the phone. For buyers, the checklist is simple (independent evals, serving cost, and a lawyer’s read of Section 2) before treating any vendor card as a procurement brief.
