---
id: collect-261121-veille-ia/minimax/minimax-m3-1-flash-open-weights-428b-moe-23b-active-1m-context-nvfp4-and-dspark
title: "MiniMax-M3.1-Flash open weights: 428B MoE, 23B active, 1M context, NVFP4 and DSpark"
domain: minimax
role: reference
task: article
actors: ["MiniMax"]
dates: []
keywords: ["model", "flash", "layers", "minimax", "minimax-m3", "attention", "weights", "context"]
source: https://www.datastudios.org/post/minimax-m3-1-flash-open-weights-428b-moe-1m-context
source_anchor: ""
source_lines: [1, 305]
section: "MiniMax"
---
MiniMax releases MiniMax-M3.1-Flash open weights with 428B parameters, 23B active and a 1M-token context

Oct 25

9 min read

Updated: Oct 28

MiniMax has published the weights of MiniMax-M3.1-Flash on Hugging Face, four days after the model became generally available through the company's own API on October 19. The repository, MiniMaxAI/MiniMax-M3.1-Flash, was initialized on October 23 and carries the "minimax-community" license tag. It contains a native multimodal mixture-of-experts model that MiniMax positions for coding, agentic workflows, computer use and cybersecurity, with text, image and video input, text output and a context window of up to one million tokens.

According to the model card, M3.1-Flash has about 428 billion total parameters, of which roughly 23 billion are active per token. The checkpoint is not a conventional BF16 release that users quantize afterwards: MiniMax applied quantization-aware training to the routed experts and the attention paths, and the published files store dense layers in MXFP8 and the routed experts in NVFP4. The release also bundles a DSpark speculative-decoding draft head inside the checkpoint, so compatible serving engines can run speculative decoding without a separate draft model.

The October 19 launch post already promised that "model weights will be released later" without giving a date, and it priced the hosted model at $0.15 per million input tokens, $0.60 per million output tokens and $0.015 per million cached input tokens, figures that also appear on MiniMax's pricing page. The open-weight release changes the deployment picture rather than the model: the same system that MiniMax serves on its platform can now be downloaded and run on four Blackwell or MI355X GPUs, or on a full eight-GPU Hopper node with reduced context and features.

··········


FROM MINIMAX-M3 TO M3.1-FLASH: THREE ARCHITECTURAL CHANGES.

MiniMax describes M3.1-Flash as an evolution of MiniMax-M3, the multimodal model family released earlier this year with an accompanying technical report. The model card states that M3.1-Flash keeps M3's core mixture-of-experts architecture and introduces three changes, each aimed at inference efficiency rather than a new training paradigm.

The first change concerns attention. In MiniMax-M3, the first three layers used full attention, while the rest of the network relied on MiniMax Sparse Attention (MSA). In M3.1-Flash those three full-attention layers are replaced with sparse attention, so the published configuration enables MSA in all 60 layers. For a model advertised with a one-million-token window, removing the last quadratic-cost layers matters because a handful of full-attention layers can dominate memory traffic and latency at very long context lengths.

The second change is quantization-aware training. MiniMax says the routed MoE experts and the attention paths were quantized during training in order to reduce the memory footprint and improve serving efficiency. The practical consequence is that the released low-precision checkpoint is the reference model, and the company states that its benchmark results refer to these official weights, adding that third-party quantizations may behave differently.

The third change replaces the EAGLE-like multi-token prediction approach used in M3 with DSpark speculative decoding. The draft head ships with the release, stored in a dedicated subfolder of the checkpoint, and both the vLLM recipe and the SGLang cookbook treat DSpark as the default speculative method for supported hardware.

··········


THE PUBLISHED CONFIGURATION: LAYERS, EXPERTS AND SPARSE ATTENTION.

The config.json in the repository gives a more precise picture of the network than the model card alone. The text backbone has 60 transformer layers with a hidden size of 6,144, 64 attention heads, 4 key-value heads and a head dimension of 128, which means grouped-query attention with a high query-to-KV ratio. The first three layers are dense feed-forward layers, and layers 3 through 59 are mixture-of-experts layers.

Each MoE layer contains 128 routed experts plus one shared expert, and the router selects 4 experts per token using sigmoid scoring with a routing bias and a routed scaling factor of 2.0. The vocabulary has 200,064 tokens, and positional encoding uses RoPE with a base of 10 million applied to half of each head's dimensions. The maximum position embedding is 1,048,576 tokens, consistent with the advertised one-million-token window.

The sparse attention configuration selects the top 16 blocks of 128 tokens for each query, scored through a lightweight indexer with 4 index heads and a 128-dimensional index, and always keeps the local block in view. This block structure has direct consequences for serving: the vLLM recipe states that a KV-cache block size of 128 is mandatory because the MSA sparse block size must match the cache block size.

........

Component

Published value

Total / active parameters

~428B / ~23B per token

Layers

60 (3 dense, 57 MoE)

Hidden size / heads / KV heads

6,144 / 64 / 4

Experts per MoE layer

128 routed + 1 shared, 4 routed active

Attention

MiniMax Sparse Attention in all 60 layers

Sparse selection

Top 16 blocks of 128 tokens

Max positions

1,048,576 tokens

Vision encoder

32 layers, hidden 1,280, patch 14, 672 px

Weight formats

MXFP8 dense paths, NVFP4 routed experts

Speculative decoding

DSpark draft head included

License tag

minimax-community

........

The vision tower is a separate 32-layer encoder with a hidden size of 1,280, a patch size of 14 and a native image size of 672 pixels, projected into the 6,144-dimensional text space. Together with video input support, this is what MiniMax means by a native multimodal model, although the output side remains text only.

··········

[[ADPLUS_300x250_1]]

PRECISION AND WHY THE HUB REPORTS A SMALLER NUMBER.

The quantization block of the configuration specifies MXFP8 with dynamic activation scaling and a weight block size of 32 for the general weights, and NVFP4 with a group size of 16 in a packed format for the MoE experts. The router gates of every MoE layer, the language-model head, the token embeddings, the multimodal projector and the vision tower are excluded from quantization and remain at higher precision. In other words, the components that are most sensitive to rounding errors, such as routing decisions and output logits, were left untouched.

This mixed format explains an apparent inconsistency on the Hugging Face page. The model card states about 428 billion parameters, while the Hub's automatic metadata lists the repository at 220 billion. MiniMax has not commented on the gap, but the most plausible reading is that the Hub counts stored tensor elements, and packed 4-bit expert weights store two values per byte, so the automatic count underestimates the logical parameter total. For comparison, the earlier MiniMax-M3 repository is listed at 427 billion, which is in line with the figure in the M3.1-Flash model card.

For operators, the format choice cuts both ways. NVFP4 runs natively on Blackwell, which is why the vLLM recipe needs no MoE quantization override on B200, B300, GB200 or GB300. On AMD MI355X the recipe sets an MXFP8 MoE path, and on Hopper it falls back to a Marlin MoE backend, a sign that older hardware can load the model but does not execute its intended number formats natively.

··········

HARDWARE AND SERVING SUPPORT AT RELEASE.

MiniMax lists vLLM, SGLang, Docker Model Runner and TokenSpeed as supported frameworks on day one, with Transformers, KTransformers, Unsloth and ATOM integrations described as in progress, and multi-chip deployment available through FlagOS. The basic commands on the model card are short:

The production recipes are considerably more detailed. The vLLM recipe, last updated on October 23 and built against vLLM 0.33.0, runs the model with tensor parallelism across four GPUs on both AMD MI355X and NVIDIA Blackwell systems, using an NVFP4 KV cache and the dedicated minimax_m3 reasoning and tool-call parsers. On Hopper it requires a full eight-GPU H100 or H200 node, caps the model length at 262,144 tokens, uses an FP8 KV cache, disables video input and does not enable DSpark.

........

Platform

vLLM single-node layout

DSpark

Notes

NVIDIA B200 / B300 / GB200 / GB300

4 GPUs, TP4

Yes, 7 draft tokens

Native NVFP4

AMD MI355X (ROCm 7.2+)

4 GPUs, TP4

Yes, 3 draft tokens

MXFP8 MoE path

NVIDIA H100 / H200

8 GPUs, TP8

Not enabled

262,144-token cap, no video

........

The SGLang cookbook adds performance data for a specialized single-kernel decoding path, which it calls MonoKernel, on four B300 GPUs at batch size 1 with the speculative acceptance length fixed at 5. Under those conditions it reports 1,028 tokens per second at 1K context against 479 for the standard path, 919 against 465 at 256K tokens, and 723 against 408 at one million tokens. These are single-stream decoding measurements under a fixed acceptance length, so they describe the ceiling of the serving stack on top-end hardware rather than the throughput a shared deployment would deliver.

The same cookbook notes that the MSA kernels on Blackwell require a CUDA toolkit of version 13.4 or newer, that AMD deployments need ROCm 7.2, and that the high-throughput AMD configuration with hierarchical caching uses roughly 350 GB of host memory per rank. Disaggregated prefill and decode setups are documented for both vendors. The barrier to entry is therefore lower than for the largest open models, but it remains datacenter hardware: four of the newest accelerators, or eight Hopper GPUs with a reduced feature set.

··········

BENCHMARKS REPORTED BY MINIMAX.

The model card compares M3.1-Flash with DeepSeek V4.1 Flash in its original and 1017 versions, Kimi K3, GLM-5.3, GPT-5.6 Sol and Opus 5. All of the figures are reported by MiniMax, and each benchmark comes with its own harness and settings, in many cases a specific Claude Code, Codex, OpenCode or mini-SWE-agent version and a context limit of 200K, 512K or 1M tokens. The comparison set also deserves attention: the closed baselines are GPT-5.6 Sol and Opus 5 rather than the newest frontier releases, so the table measures M3.1-Flash against an earlier generation of proprietary models.

........

Benchmark

M3.1-Flash

DeepSeek V4.1 Flash-1017

Kimi K3

GPT-5.6 Sol

Opus 5

Automation-Bench v1.0.6

62.8

60.5

46.7

45.8

50.3

BankerToolBench

76.2

—

69.1

74.6

76.2

Terminal Bench 4.0

38.1

42.3

12.6

39.9

51.8

LOCA-Bench 256K

60.3

—

49.7

60.0

53.7

OSWorld 2.0

61.7

—

58.3

65.7

70.6

DeepSWE v1.1

73.7

77.0

67.5

73.0

74.0

NL2Repo-Bench

61.2

68.2

58.0

56.8

75.3

CyberGym

82.2

—

80.0

83.6

—

HLE (no tools, text-only)

43.4

42.6

43.5

49.5

55.6

........

The pattern is consistent with MiniMax's positioning of Flash as an efficiency-oriented model. M3.1-Flash leads the full comparison on Automation-Bench and LOCA-Bench at 256K tokens and ties Opus 5 on BankerToolBench, but it trails DeepSeek V4.1 Flash-1017 on Terminal Bench 4.0, DeepSWE and NL2Repo-Bench, and it trails both closed baselines on OSWorld 2.0 and on the text-only subset of Humanity's Last Exam. On CyberGym it reports 82.2, slightly behind GLM-5.3 at 84.5 and GPT-5.6 Sol at 83.6; the launch post rounds the same result to 82%.

Several footnotes affect how the numbers should be read. The OSWorld 2.0 column uses the highest score reported for each model at release, except for DeepSeek V4.1, which MiniMax reproduced in-house. The Agents' Last Exam column, where M3.1-Flash scores 29.6, takes only full-set results from the official leaderboard, and blank cells there mean no official result rather than zero. GDP.pdf figures are taken from Artificial Analysis, while HLE is measured on a 2,158-question text-only subset. No independent reproduction of the full table had been published with the weights.

··········

LICENSE, PRICING AND WHAT OPEN WEIGHTS MEAN HERE.

The repository is tagged "minimax-community", and the model card itself contains no license section explaining the terms. That label indicates a custom community license rather than a standard open-source license such as Apache 2.0 or MIT, and organizations planning commercial deployment will need to read the license text in the repository rather than rely on the tag. The release is accurately described as open weights; the training data, training code and the quantization-aware training pipeline are not part of it.

The hosted API remains the simplest way to use the model. MiniMax's pricing page lists M3.1-Flash at $0.15 per million input tokens, $0.60 per million output tokens and $0.015 per million cache-read tokens, without the long-context tier that applies to MiniMax-M3, whose standard pricing doubles above 512K input tokens. In its launch post the company also claims online inference nearly three times faster than its predecessor, output speeds above 200 tokens per second with a median above 170, and agentic usage costs 70% lower than the previous model; these figures describe MiniMax's own service and were not independently verified.

The same post said a Pro version aimed at higher capability will follow later, without a date. The October 19 announcement also disclosed that an early anonymous version of the model, deployed under the name "Space Bunny", had processed more than 200 trillion tokens before launch, which gives some indication of the scale of real-world usage behind the release.

··········

TECHNICAL ASSESSMENT.

What is established is concrete and verifiable from the repository: MiniMax has released a roughly 428B-parameter, 23B-active multimodal MoE model whose weights are stored natively in MXFP8 and NVFP4, with sparse attention in all 60 layers, a configuration that supports positions up to 1,048,576 tokens, and an integrated DSpark draft head. Day-one recipes from vLLM and SGLang show that the model runs on four current-generation Blackwell or MI355X accelerators, and on eight Hopper GPUs with a 262K-token cap and without video or speculative decoding.

The release is notable less for its benchmark position than for its packaging. Shipping a quantization-aware trained low-precision checkpoint as the official artifact, rather than a BF16 model that the community later compresses, moves the efficiency work upstream and gives operators a reference model whose quality MiniMax has measured directly. Removing the last full-attention layers in favor of block-sparse attention follows the same logic, prioritizing long-context serving cost.

Uncertainties remain. The benchmark table is entirely vendor-reported, uses heterogeneous harnesses and compares against an older generation of closed models. The parameter count differs between the model card and the Hub metadata, most likely because of packed 4-bit storage, but MiniMax has not clarified it. The terms of the minimax-community license are not summarized in the model card, and practical quality at the full one-million-token window on hardware other than Blackwell has not yet been documented in public.
