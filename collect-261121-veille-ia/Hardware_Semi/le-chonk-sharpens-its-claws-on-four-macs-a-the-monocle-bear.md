---
id: collect-261121-veille-ia/hardware-semi/le-chonk-sharpens-its-claws-on-four-macs-a-the-monocle-bear
title: "Le Chonk sharpens its claws on four Macs â The Monocle Bear"
domain: hardware-semi
role: reference
task: article
actors: []
dates: []
keywords: ["mistral", "four", "every", "layer", "chonk", "sharpens", "claws", "bits"]
source: https://themonoclebear.com:443/en/blog/le-chonk-sharpens-its-claws-on-four-macs/
source_anchor: ""
source_lines: [1, 60]
section: "Hardware_Semi"
---
# Le Chonk sharpens its claws on four Macs

Mistral Large 4, Le Chonk, sharpens its claws on our test bench. Running Mistral on an MLX cluster is an adventure every time. This one went better than expected.

It was released as open weights on Hugging Face on Friday, and we loaded it onto our machines right away. No MLX conversion existed at that hour. We quantized the original weights to 4 bits, heads in 16 bits, group size 32, and ran them on four nodes of our cluster in tensor parallel.

Consistent with the Artificial Analysis ranking, Mistral Large 4 running locally is beaten by Ling 3.1 Flash, DeepSeek V4.1 Flash 1017 and GLM 5.3 Flash. It ranks ninth among local models on our scoreboard, at 92.7.

## A checkpoint for someone elseâs silicon

The adventure starts with the files. The MLX ecosystem, like transformers and everything around it, expects the Hugging Face layout: `model.layers.3.self_attn.q_proj.weight`, an index, scales in a known convention. Mistral publishes its own âconsolidatedâ format: `layers.3.attention.wq.weight`, `wkv_a_with_mqa`, `w1/w2/w3`. The weights are FP8 e4m3 with a separate float32 scale for every 128 Ã 128 block, stored as its own tensor, and a weight and its scale can land in two different shards. There is no index. The mlx-lm loader only looks for `model*.safetensors` and sees nothing at all.

FP8 with 128-block scales is the native path of Hopper and Blackwell tensor cores. Apple Silicon has no FP8 matmul, so every weight has to be decoded to bf16 and quantized again: what runs on the Mac shares nothing with the published bytes. The official NVFP4 release says it plainly: it âtargets Blackwell GPUs and supports vLLM and SGLang.â

On Friday, there was no `mistral_large4` in mlx-lm, in mlx-vlm, on mlx-community, or in the transformers release we run. The only correct description of the architecture lived in the main branch of transformers. Mistral does not publish for MLX. It publishes for CUDA. None of this is physics: the kernels MLX needs already exist. What is missing is the translation layer, and it has to be rewritten for every Mistral generation.

## Six departures from the textbook

The architecture is a MoE of 1 trillion parameters, 52 billion active, 72 layers, and it stacks every recent idea at once.

- **MLA, absorbed.** Keys and values are compressed into a 512-dimension latent. The decompression matrix is folded into the query and output projections at load time, so decode attends directly in latent space. A year ago, an earlier Mistral port that decompressed the whole cache at every step crawled at two to three tokens per second. Absorption is the condition for running at all.
- **ssmax.** Each head scales its queries by`sÂ·log(pos+1) + b` , learned per head and per layer, on the full query.
- **LeRoPE.** Learned per-frequency factors on top of YaRN, folded once into 32 rotary frequencies.
- **One layer in four without RoPE.** 18 of the 72 layers have no positional rotation and wider non-positional heads, which changes tensor shapes layer by layer.
- **Shared KV.** Nine layers, 39 to 71 in steps of four, have no key-value projection of their own. They read the latent cache of layer 35.
- **128 experts, 4 active, plus one shared expert** , routed in fp32, and a`30Â·tanh(x/30)` softcap on the logits.

Three components stayed out: the vision encoder, the multi-token prediction head, and the DSA indexer, which picks the 2,048 most relevant tokens for each query. What runs here is not all of Mistral Large 4. Up to 2,048 tokens of context, our attention is exact. Beyond that, it reads everything, where the original chooses what to read.

## The 4 bits nobody asked for

The first conversion was a 6-bit, text-only build: 857.6 GiB, every structural and round-trip check green. On the way there, it nearly shipped something else. A quantization predicate returned `{"group_size": 32}` without `bits`. mlx-lm took that to mean 4 bits, raised no error, printed no warning, and wrote the setting into the config. A round-trip on an 8-layer pilot caught the output projection packed at 4 bits per weight instead of 6. The full run had been going for twenty minutes. It was killed and restarted.

The 6-bit build never generated a token. Split four ways, it needs 215 GiB per rank, more than the 200 GiB of wired memory our 256 GB nodes allow. So the 4 bits came back, on purpose this time and only where they hurt least: the 128 routed experts in 4 bits, group size 32; attention, the shared expert and dense layers in 6 bits; embeddings, output head, the absorbed MLA projections and the latent projection in bf16. 621 GiB, 5.06 bits per weight, about 155 GiB per node. The vision weights travel in a separate file, 2 GB of bf16, unused for now.

Checked against an independent FP8 decoder: no missing or extra keys, every bf16 tensor bit-identical to the source, correlation 0.9997 on the 6-bit tensors and 0.9965 on the 4-bit experts.

## A shared cache, four machines

Tensor parallel splits the 64 attention heads into four groups of 16. Query projections are split per head, output projections are summed across ranks, ssmax parameters follow their heads, and the experts are sharded in place with one all-reduce per MoE layer. Five nodes are not an option: neither 64 heads nor 128 experts divide by five.

The shared KV looked like the hard part: nine layers on four machines reading a cache that belongs to a tenth. It resolved itself. The latent does not depend on the heads. Replicate the small latent projection on every rank, about 7 MB per layer, and each machine computes the full latent of layer 35, caches it, and serves it locally to layers 39 to 71. No extra communication. On a small random-weight model reproducing every quirk, sharded output against unsharded, two and four ranks: maximum deviation about 2e-6, identical tokens on every rank.

The day before, a 6-bit pipeline run across five machines had decoded at 13.5 tokens per second and answered â2â to 17 Ã 23. In tensor parallel, the same question gets 391.

## Thirteen hours, one fall

Mistral Large 4 decodes at 25 to 29 tokens per second, first token in about 1.3 seconds. The benchmark started on Saturday night at 23:36 and ran through the morning without a break: 13 hours, 806,600 tokens generated, about 17 tokens per second on average once prefills and pauses are counted.

On one test, the model wrote 51,089 tokens in a single answer: 3,648 seconds, 14 tokens per second on average. The next prompt was 722 tokens long. Three of the four nodes reported a Metal out-of-memory error, no token came for 600 seconds, the pool was marked degraded, and the three machines were rebooted. The likely cause, not yet verified, is the 51,000-token cache of the previous session still sitting in memory on top of 155 GiB of weights. After a reload, the remaining series ran alone and finished cleanly.

## Where the points went

Local, Mistral Large 4 scores 92.7 on our 43 tasks, judged criterion by criterion by Claude Opus 4.8. The version served on OpenRouter scores 93.7. Mistral Large 3 scored 84.3 in the cloud and 72.3 in MXFP4 on this same bench. The previous generation lost twelve points on the way to a Mac. This one loses one. Whether that point comes from the 4-bit experts, the missing indexer or what follows, we cannot tell.

On two of our five series, it is close to flawless. The points go elsewhere. Two answers came back empty and scored zero without reaching the judge. On an agentic debugging task, it described the fix instead of acting: no reproduction of the bug, no file written, no check. A long-form piece stopped at about 2,900 words against a 4,500 to 5,500 target. Once, two Chinese characters landed in the middle of a French word.

The vision weights are already on every node. The memory left behind by a 51,000-token session is the next thing to fix.

Sophie, The Monocle Bear
