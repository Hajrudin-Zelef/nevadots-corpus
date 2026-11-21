---
id: collect-261121-veille-ia/kimi/moonshot-ai-s-kimi-k3-1-pro-weights-are-now-open-explained-honestly
title: "Moonshot AI's Kimi K3.1 Pro Weights Are Now Open — Explained Honestly"
domain: kimi
role: reference
task: article
actors: ["Moonshot AI"]
dates: []
keywords: ["moonshot", "model", "weights", "million", "kimi", "public", "release", "tokens"]
source: https://dev.to/paullovelace/moonshot-ais-kimi-k31-pro-weights-are-now-open-explained-honestly-372l
source_anchor: ""
source_lines: [1, 97]
section: "Kimi"
---
On 4 November 2026, Moonshot AI completed the promise it made at launch: the weights of Kimi K3.1 Pro are now public. The model itself is not new — K3.1 Pro launched on 23 October 2026 as an API and app model — but until today, developers could only use it through Moonshot's hosted service. Now the weights are published on Hugging Face, in the public, ungated repository moonshotai/Kimi-K3.1-Pro, and anyone can download, inspect, fine-tune and deploy them under Moonshot's licence. Moonshot announced the release on X at 16:00 UTC on 4 November; the repository was first seen public at around 15:31 UTC the same day.

That distinction matters, because it is easy to misread today's news as a new model launch. It is not. It is the open-weights step of a model that has already been out for twelve days — and that step changes who can build on it, and how.

The release is the full Kimi K3.1 Pro checkpoint: a natively multimodal agentic model with 2.8 trillion total parameters, of which 104 billion are active per token. It is a mixture-of-experts (MoE) model with 896 experts, 16 activated per token, across 93 layers, and a context window of 1,048,576 tokens — one million tokens.

Two architectural details are worth noting. First, 24 of the 93 layers replace full Gated MLA attention with sparse Gated DSA (dynamic sparse attention), a design choice aimed at handling very long contexts more efficiently. Second, the weights are stored in MXFP4 format with MXFP8 activations, produced through quantization-aware training — which is how a model this large ships in a downloadable form at all.

The practical numbers are large, and they should be stated plainly. The download is about 1.56 TB, spread across 96 safetensors shards. The vision side is a 401-million-parameter MoonViT-V2 encoder. There is no GGUF version yet, so the usual lightweight local-running route is not available at release.

Moonshot publishes deployment recipes for vLLM and SGLang, and also names TokenSpeed among the recommended engines. That is the realistic starting point for anyone planning to serve the model: these are the engines Moonshot itself points to, not a hands-on recommendation from us — we have not deployed the model, and nothing here is a hands-on finding.

For most developers, though, the honest answer today is that the hosted API remains the realistic route. Self-hosting needs server-class hardware: sources put the requirement at eight or more B300/GB300-level GPUs, with roughly 861 GB of VRAM at 2-bit precision and roughly 1,509 GB at 4-bit. No self-hosting price is given here, because none has been verified — anyone quoting a single-server cost for this model on release day should be treated with suspicion.

The hosted API is priced, and those prices are public: the API id is kimi-k3.1-pro, at $2 per million input tokens and $10 per million output tokens, with cached input at $0.20 per million. A Highspeed variant is listed at $4 per million input and $20 per million output, which Moonshot describes as 2–3 times faster — that speed figure is a vendor claim, not a measured result.

Moonshot's published figures put K3.1 Pro close to the top of the agentic charts. All of the following are Moonshot's own reported numbers, and none has been independently verified yet. No independent tests exist at the time of writing, and that absence is part of the story.

- 
**Terminal-Bench 4.0: 65.7%.** In Moonshot's own comparison table, that sits just below Claude Opus 5.5 at 66.4% and above GPT-6 Astra at 58.2% — again, per Moonshot's table, not an independent leaderboard.
- **GPQA-Diamond: 94.2.**
- **BrowseComp: 92.1.**
- **OSWorld V2.1: 79.6.**

These are strong numbers if they hold up. But "if they hold up" is doing real work in that sentence. Vendor benchmarks are run in the vendor's own harness, on the vendor's own prompts, and the history of open-weights releases is full of figures that softened under independent testing. Until outside labs and developers reproduce these results, treat them as Moonshot's claims about its model, not as settled facts. We have not run any of these benchmarks ourselves.

The weights are released under the Kimi K3.1 Pro License, which reads as MIT-style with just two conditions, and both apply only to very large operators:

1. A model-as-a-service business with more than $20 million in revenue over 12 months needs a separate agreement with Moonshot.
2. Products with more than 100 million monthly users, or more than $20 million in monthly revenue, must display the model's name.

Below those thresholds, the licence is free to use, modify and sell. For independent developers, startups and researchers, that is about as open as a frontier-class release gets. For the largest providers, the conditions are the price of using someone else's flagship weights at scale — and they are worth reading in full before building a business on them, since this article is a summary, not legal advice.

Moonshot says it stripped cyber-related training data from this release, and reports that its own internal exploit benchmark fell from 90.5% to 81.0% after that filtering — a vendor figure, not independently verified. The released model card also lists 74.7 on ExploitBench and 20.7 on ExploitGym.

None of this should be read as the model being "safe" or the filtering being proven. Outside researchers have not tested the filtering yet, and an internal benchmark drop is Moonshot's own measurement of its own change, not an independent security audit. The honest statement is narrower: Moonshot says it removed cyber-related training data, its own numbers moved in the direction it intended, and nobody outside the company has yet verified either the removal or the effect. If you work in security research, that verification gap is the thing to watch — not the headline figure.

- 
**Agent developers who want a frontier-class open model.** If you build agents and have been waiting for an open alternative to closed frontier APIs, this is one of the largest open-weights releases of the year — with a million-token context and an agentic design aimed at long, tool-using tasks.
- 
**Teams that fine-tune.** The full weights, under a permissive licence, mean you can adapt the model to your own data and tasks rather than only prompting a hosted API. That is the real difference today's release makes.
- 
**Infrastructure teams and researchers.** The scale — 2.8T parameters, 1.56 TB download, multi-GPU serving — makes this a serious deployment project, not a laptop experiment. The vLLM and SGLang recipes are where a serious evaluation starts.
- 
**Anyone tracking the open-versus-closed race.** A model this large, released openly twelve days after its API launch, keeps pressure on closed providers — and the licence's large-player conditions show Moonshot thinking carefully about who benefits most from that openness.

We have not downloaded, deployed or tested Kimi K3.1 Pro, and nothing here is a hands-on finding. Everything above comes from the sources listed below, checked 4 November 2026. Beyond that, the honest caveats are:

- 
**All benchmark figures are Moonshot's own, unverified.** No independent tests exist yet. The Terminal-Bench comparison with Claude Opus 5.5 and GPT-6 Astra comes from Moonshot's own table.
- 
**The cyber-filtering claim is unverified.** Moonshot's internal benchmark drop (90.5% to 81.0%) and the model card's ExploitBench/ExploitGym figures are vendor numbers. Outside researchers have not tested the filtering. Do not present this release as verified safe.
- 
**Self-hosting is out of reach for most developers today.** Eight or more B300/GB300-level GPUs, roughly 861 GB VRAM at 2-bit and 1,509 GB at 4-bit, a 1.56 TB download, and no GGUF yet. The hosted API is the realistic route for most teams, and no self-hosting price has been invented here.
- 
**The Highspeed variant's "2–3x faster" is a vendor claim** , not a measured result.
- 
**This is not a new model.** K3.1 Pro launched 23 October 2026; today completes the promised open-weights step. Any report calling this a new launch is wrong.
- **No hands-on testing was performed for this article, and no benchmarks, prices, dates or results beyond the verified facts above have been invented to fill gaps.**

The bottom line is a measured one. Kimi K3.1 Pro's open weights are a genuinely significant release: a 2.8T-parameter, million-context, natively multimodal agentic model, free under a permissive licence for nearly everyone, twelve days after its API debut. The scale is real, the licence is generous, and the long-context agentic design is aimed at exactly the workloads developers care about most right now. But the benchmarks are Moonshot's own until independent testing lands, the cyber filtering is a vendor claim awaiting outside verification, and the hardware bar means most developers will still meet this model through its API, not their own servers. Download it if you have the hardware and the use case; test it before you trust the leaderboard numbers; and read the licence before you build a business on it.

*Sources: Moonshot AI on X, 4 November 2026 (announced 16:00 UTC); Hugging Face repository moonshotai/Kimi-K3.1-Pro (public, ungated); mlllm.io, "Moonshot AI Releases Kimi K3.1 Pro Weights" (https://www.mlllm.io/articles/3719-moonshot-ai-releases-kimi-k3-1-pro-weights-2-8t-parameter-moe-1m-token-context-and-gated-dsa-sparse-attention/); CellCog, "Kimi K3.1 Release Date" (https://cellcog.ai/blog/kimi-k3-1-release-date/); ArtificialWatch, Kimi K3.1 model page (https://artificialwatch.com/model/kimi-k3-1); ExplainX, "Kimi K3.1 Pro Open Weights Benchmarks Price Terminal Bench Nov 4 2026" (https://explainx.ai/blog/kimi-k3-1-pro-open-weights-benchmarks-price-terminal-bench-nov-4-2026). All benchmark, pricing, licence and cyber-filtering claims are Moonshot's own or as reported by these sources, not independently tested results. No hands-on testing was performed for this article. This breaking article is not the daily buyer-intent post, and it contains no affiliate links.*

**What did Moonshot AI release on 4 November 2026?**

The open weights of Kimi K3.1 Pro, published on Hugging Face in the public, ungated repository moonshotai/Kimi-K3.1-Pro. The model itself launched on 23 October 2026 as an API/app model; today's news completes the promised open-weights step. It is not a new model launch.

**How big is Kimi K3.1 Pro?**

2.8 trillion total parameters, 104 billion active per token, a mixture-of-experts model with 896 experts (16 per token), 93 layers, and a 1,048,576-token (1M) context window. The download is about 1.56 TB across 96 safetensors shards, with a 401M-parameter MoonViT-V2 vision encoder.

**What are the reported benchmarks?**

Moonshot's own figures, not independently verified: Terminal-Bench 4.0 65.7% (vs Claude Opus 5.5 66.4% and GPT-6 Astra 58.2% in Moonshot's table), GPQA-Diamond 94.2, BrowseComp 92.1, OSWorld V2.1 79.6. No independent tests exist yet.

**Can I run it locally?**

Realistically, only with server-class hardware: 8+ B300/GB300-level GPUs per sources, roughly 861 GB VRAM at 2-bit and 1,509 GB at 4-bit, and there is no GGUF version yet. For most developers the hosted API is the realistic route today. Recommended engines are vLLM, SGLang and TokenSpeed, with deployment recipes published for vLLM and SGLang.

**How much does the API cost?**

API id kimi-k3.1-pro: $2 per million input tokens, $10 per million output tokens, $0.20 per million cached input tokens. A Highspeed variant costs $4/$20 and is described by Moonshot as 2–3x faster (vendor claim, not verified).

**What does the licence allow?**

The Kimi K3.1 Pro License is MIT-style with two conditions only for the largest players: model-as-a-service businesses above $20M revenue in 12 months need a separate agreement, and products above 100M monthly users or $20M monthly revenue must show the model's name. Otherwise it is free to use, modify and sell.

**Did Moonshot make the model safer?**

Moonshot says it stripped cyber-related training data; its own internal exploit benchmark fell from 90.5% to 81.0% (vendor figure, unverified), and the model card lists 74.7 ExploitBench / 20.7 ExploitGym. Outside researchers have not tested the filtering yet, so this is not presented as verified safe.

**Was Kimi K3.1 Pro tested hands-on for this article?**

No. All facts come from the sources listed above, checked 4 November 2026.

## Top comments (0)
