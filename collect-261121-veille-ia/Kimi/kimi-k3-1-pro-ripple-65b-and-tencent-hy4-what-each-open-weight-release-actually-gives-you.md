---
id: collect-261121-veille-ia/kimi/kimi-k3-1-pro-ripple-65b-and-tencent-hy4-what-each-open-weight-release-actually-gives-you
title: "Kimi K3.1 Pro, Ripple-65B and Tencent Hy4: What Each Open-Weight Release Actually Gives You"
domain: kimi
role: reference
task: article
actors: ["Moonshot AI"]
dates: []
keywords: ["model", "kimi", "ripple-65b", "each", "three", "tencent", "license", "weights"]
source: https://medium.com/@abhinav.dobhal/kimi-k3-1-pro-ripple-65b-and-tencent-hy4-what-each-open-weight-release-actually-gives-you-d6dc1e9c583f
source_anchor: ""
source_lines: [1, 88]
section: "Kimi"
---
# Kimi K3.1 Pro, Ripple-65B and Tencent Hy4: What Each Open-Weight Release Actually Gives You

## Three model launches shared the Hacker News front page on November 4. I read the model cards, license files and threads to sort out parameters, licenses, hardware and which one a developer can realistically run.

On November 4, three model launches sat on the Hacker News front page at once: Moonshot AI’s Kimi K3.1 Pro weights at 431 points, Ricursive Intelligence’s Ripple-65B Preview at 260 and Tencent’s Hy4 at 210. Kimi is a 1.56 TB download, Ripple is small enough for a desk, and Hy4’s weights haven’t shipped yet.

A note on how this was made: I wrote it with help from Claude, Anthropic’s AI model, and checked each claim against the three vendors’ model cards and launch posts, the Kimi license file, the Hugging Face API, the kimi-k3-in-c README and the three Hacker News threads. The images were generated with ChatGPT.

I haven’t run any of these models. Every benchmark below is the vendor’s own number, and every size is what the model card or the Hugging Face API reports.

## The short version

- Kimi K3.1 Pro is a 2.8T-parameter MoE (104B active) with about 1.56 TB of weights and an MIT-style license with revenue and attribution conditions. It needs a GPU cluster.
- Ripple-65B Preview is a dense 65B model under Apache 2.0 with a 57.2 GB NVFP4 checkpoint. It’s the only one of the three a single developer can realistically host.
- Tencent Hy4 is available by API, but its weights are only planned. Hy4 preview (770B total, 49B active, Apache 2.0) is downloadable today.
- Every benchmark score in these launches is the vendor’s own claim.

## Kimi K3.1 Pro: a 2.8T model with a revenue clause

Moonshot’s model card describes K3.1 Pro as a natively multimodal mixture-of-experts model with 2.8T total parameters and 104B active per token. It has 93 layers, 896 experts with 16 selected per token plus 2 shared, a 401M-parameter MoonViT-V2 vision encoder and a context length of 1,048,576 tokens. Compared with Kimi K3, the card says the full-attention layers were replaced with Gated DeepSeek Sparse Attention for cheaper long context.

The weights ship as MXFP4 with MXFP8 activations from quantization-aware training. The Hugging Face repo, created November 2, holds 96 safetensors shards that add up to about 1.56 TB by the API’s count. The card recommends vLLM, SGLang and TokenSpeed, each with its own recipe, and I found no llama.cpp support mentioned anywhere on it.

The license reads like MIT with two conditions. A company running a “Model as a Service” business with more than $20 million in revenue over any 12 months must sign a separate agreement with Moonshot before commercial use. Any commercial product with more than 100 million monthly users or $20 million in monthly revenue must show “Kimi K3.1-Pro” prominently in its UI, and internal use is exempt from both rules.

On benchmarks, Moonshot reports 65.7 on Terminal-Bench 4.0 against 66.4 for Claude Opus 5.5 and 58.2 for GPT-6 Astra, and 92.1 on BrowseComp. The footnotes say K3.1 Pro was run with the Claude Code harness for most coding tests, while several rival scores were copied from leaderboards or system cards. With methods that mixed, I read the table as Moonshot’s claim and nothing firmer.

## Ripple-65B Preview: a dense 65B model under Apache 2.0

Ripple is the only dense model of the three. Ricursive’s launch post describes a dense 65B transformer trained from scratch on 12T tokens, with up to 1M context. The model card adds the shape: 80 layers, a d_model of 5120, sliding-window attention mixed with global layers, gated attention and value residuals.

The license is Apache 2.0, with no usage thresholds. The Hugging Face API counts 64.78B BF16 parameters in a 129.6 GB checkpoint, and the separate NVFP4 repo is 57.2 GB. Ricursive also published a BF16 base model and a DFlash2 draft model for speculative decoding.

Ricursive’s headline claim is 80.52 on TerminalBench 2.1, which it calls a U.S. open-weight state of the art. It lists Reflection Beam 501B at 80.1, Meta Muse Spark Open 816B at 76.4 and NVIDIA Nemotron 3 Ultra 550B at 56.4. The same table shows Ripple behind Reflection Beam on SWE-bench Verified (76.5 vs 81.6), and the company says its total R&D cost was $32M.

## Tencent Hy4: announced, priced, not yet downloadable

Tencent’s November 4 post introduces Hy4 as a scaled-up successor to the Hy4 preview, adding vision and computer use for the first time. It is available through Tencent Cloud, OpenRouter and Tencent’s own products. The listed API price is $0.834 per million input tokens and $2.501 per million output.

The post says Tencent “plans to open-source” the weights, has partnered with the vLLM and SGLang communities, and will publish a system card when it does. I couldn’t find parameter counts for the final Hy4 in the post, so I won’t guess.

What you can download today is Hy4 preview from August. Its card lists 770B total and 49B active parameters, 78 layers, 256 routed experts with 8 active per token, 1M context and an Apache 2.0 license. The BF16 repo is about 1.56 TB and the FP8 one about 814 GB, and the card’s vLLM and SGLang examples use 8-way tensor parallelism.

Tencent’s own claims for Hy4 are mostly internal evaluations. In a test with 213 finance professionals over 579 tasks, it scores Hy4 at 3.04, ahead of DeepSeek-V4.1-Flash-1017 at 2.97 and Kimi K3.1 Pro at 2.68. It also reports cutting its internal hallucination rate from 15.76% on the preview to 5.45%.

## What Hacker News made of it

The Kimi thread focused on hardware. The top comment guessed you’d need “something like an 8x B300 node,” and another said these releases are “meant for companies, not average Joe.” One commenter noted that third-party hosts were already serving it on OpenRouter, some with higher throughput than Moonshot’s own endpoint.

The Ripple thread was skeptical of the framing. Several people said a “U.S. open-weight SOTA” claim narrows the category too far, and that the comparison leaves out Qwen; one cited Qwen 3.8 Flash Next at 84.3 on the same benchmark. Others welcomed the size, with one Mac Studio owner calling it “a model that will actually run.”

On Hy4, readers liked that Tencent admits “a notable gap remains” with top proprietary models. A commenter benchmarking it for data analytics called it very thorough “and therefore verrrrry slow.” Another pointed out that Kimi K3.1 Pro and Hy4 both trimmed their offensive security abilities.

## Which one can you actually run?

My reading of the cards:

- Ripple-65B Preview is the only one a single developer can realistically host. Ricursive says it fits on a DGX Spark, and the NVFP4 checkpoint is 57.2 GB. The bundled SGLang 0.5.19 server targets two B200s, where Ricursive reports 250 tok/s at batch size 8. llama.cpp support is open as PR #31692, and community GGUF repos say you need that patch until it merges.
- Kimi K3.1 Pro is a cluster model. At 1.56 TB of weights, you are serving it on vLLM or SGLang across many GPUs, or calling the API at platform.kimi.ai or a third-party host.
- Hy4 can’t be self-hosted yet. If you want Tencent’s stack on your own GPUs, Hy4 preview is the option, and its card points to 8-GPU vLLM and SGLang setups.

Fitting in memory doesn’t promise speed. Ripple activates all 65B parameters for every token, while Hy4 preview activates 49B of 770B. A Hacker News commenter argued that a 65B dense model “will be prohibitively slow on consumer grade hardware.”

## The kimi-k3-in-c footnote

GitHub’s trending list also had FareedKhan-dev/kimi-k3-in-c, a portable C99 engine with about 10,400 stars. Its README says it runs the 2.78T-parameter Kimi K3 on one CPU with a measured peak of 8.24 GB of RAM, streaming the 1.56 TB checkpoint from NVMe.

Speed is where it falls down: the README reports 26.5 seconds per token at 8 GB and 5.6 seconds per token once 128 GB or more holds the model, on a 124-core machine with a fast NVMe drive. It targets Kimi K3, not K3.1, and I found no mention of K3.1 support. K3.1 changed its attention layers, so I wouldn’t assume compatibility.

I write about AI agents, local models and Rust. If you had to pick one of these to build on this month, would you take the dense model you can own or the giant one you’ll rent?

## Quick answers

### Can I run Kimi K3.1 Pro locally?

Not in any normal sense. Moonshot AI’s model card lists 2.8T total parameters and 104B active, the Hugging Face checkpoint is about 1.56 TB, and the card recommends vLLM, SGLang or TokenSpeed on a multi-GPU server. A community C engine, kimi-k3-in-c, streams the earlier Kimi K3 from disk on a CPU, but its README reports tens of seconds per token on small machines and does not mention K3.1.

### Is Tencent Hy4 open weight yet?

Not as of November 5, 2026. Tencent’s Hy4 announcement says it plans to open-source the weights and has partnered with the vLLM and SGLang communities. The earlier Hy4 preview (770B total, 49B active) is on Hugging Face under Apache 2.0.

### What hardware does Ripple-65B Preview need?

Ricursive Intelligence says the 65B dense model fits on an NVIDIA DGX Spark. Per the Hugging Face API, the BF16 checkpoint is about 129.6 GB and the official NVFP4 checkpoint is about 57.2 GB. The bundled SGLang server targets two B200 GPUs, and llama.cpp support is an open pull request.

### What are the Kimi K3.1 Pro license restrictions?

The license is permissive by default, but companies running a Model as a Service business with more than $20 million in revenue over 12 months need a separate agreement with Moonshot AI for commercial use. Products with over 100 million monthly active users or $20 million in monthly revenue must display Kimi K3.1-Pro in their UI. Internal use is exempt from both.
