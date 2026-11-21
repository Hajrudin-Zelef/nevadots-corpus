---
id: collect-261121-veille-ia/deepseek/deepseek-v4-pro-leaves-preview-as-flagship-ships-ga-build
title: "DeepSeek V4 Pro Leaves Preview as Flagship Ships GA Build"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: []
keywords: ["deepseek", "build", "preview", "cent", "model", "flagship", "behind", "weights"]
source: https://distillation.technology/newsroom/deepseek-v4-pro-leaves-preview
source_anchor: ""
source_lines: [1, 36]
section: "DeepSeek"
---
DeepSeek released the production version of its flagship model, DeepSeek V4 Pro, on 12 August 2026, ending a preview period that ran nearly four months. The build, designated V4 Pro 0813, appeared as the general-availability release on OpenRouter's model page, and DeepSeek's API documentation lists DeepSeek-V4-Pro-0813 as the version behind the deepseek-v4-pro endpoint.

The general-availability build completes a staged rollout: DeepSeek previewed the V4 series on 24 April 2026 with open weights for both Pro and Flash under the MIT licence, and graduated Flash to official status on 31 July 2026, saying the Pro release "will follow soon".

## Architecture and training

The Hugging Face model card describes V4 Pro as a mixture-of-experts system with 1.6 trillion total parameters and 49 billion active per token. The architecture combines two attention variants the company calls Compressed Sparse Attention and Heavily Compressed Attention, which DeepSeek says cut single-token inference compute to 27 per cent and KV cache to 10 per cent of what its V3.2 generation needed at the million-token setting. Both V4 models were pre-trained on more than 32 trillion tokens.

The post-training pipeline grows domain-specific experts separately, then consolidates them into a single model through on-policy distillation. Readers following the wider distillation dispute should note the distinction: this is an internal technique applied to DeepSeek's own experts, not the contested practice, alleged in that dispute, of extracting capability from another laboratory's model.

## The benchmark claims

The headline evaluations are vendor-reported, drawn from the model card and the company's own harness runs at the maximum reasoning setting, labelled V4-Pro-Max:

- SWE-bench Verified: 80.6 per cent resolved
- Terminal Bench 2.0: 67.9 per cent
- GPQA Diamond: 90.1 per cent pass@1
- Humanity's Last Exam: 37.7 per cent pass@1
- LiveCodeBench: 93.5 per cent pass@1
- Codeforces rating: 3,206

The card's comparison table places V4-Pro-Max behind GPT-5.4 at xHigh effort on Terminal Bench 2.0 (67.9 to 75.1) and behind Gemini-3.1-Pro on Humanity's Last Exam (37.7 to 44.4), while posting the table's top LiveCodeBench and Apex Shortlist scores. On SWE-bench Verified it is level with Gemini-3.1-Pro and a fraction behind Claude Opus 4.6 at 80.8. None of these results has been replicated by an independent evaluator for the 0813 build.

## Pricing, access and open weights

The API economics carry over from preview: $0.435 per million input tokens on a cache miss, $0.003625 per million on a cache hit and $0.87 per million output tokens, with a one-million-token context window and a maximum output of 384,000 tokens. The Pro endpoint carries a concurrency limit of 500 against 2,500 for Flash, and the API offers three operating modes plus support for the OpenAI ChatCompletions and Anthropic Messages formats alongside DeepSeek's own Responses API.

A notice on the pricing page states that DeepSeek plans "a significant increase" in overall API pricing, with specifics to come by official notice. For now the listed rates hold, and the average price paid through OpenRouter sits well below list, which OpenRouter attributes to caching and discounts.

The open item is new weights. The Hugging Face repositories still host the April preview builds, the Pro repository logged more than 1.4 million downloads in the last month, and DeepSeek has given no timeline for 0813 weights nor said whether the GA build differs from preview beyond post-training. The release lands amid a near-monthly cadence of flagship-class open-weight models from Chinese laboratories, from Moonshot's Kimi K3 to MiniMax's M2.7.

## What is established and what is merely claimed

Established: the 0813 build exists and serves the deepseek-v4-pro endpoint as of 12 August 2026; the listed prices, context window, output ceiling and concurrency limits appear on DeepSeek's own pages; the rollout timeline from the 24 April 2026 preview to the 31 July 2026 Flash graduation is documented in the company's change log; and the pricing-increase notice is published.

Merely claimed: every benchmark figure above is vendor-reported, with no independent replication for this build. The efficiency figures of 27 per cent compute and 10 per cent KV cache relative to V3.2 are company assertions. Whether the GA build differs from the April preview beyond post-training, and the size and timing of the promised price rise, remain unstated.
