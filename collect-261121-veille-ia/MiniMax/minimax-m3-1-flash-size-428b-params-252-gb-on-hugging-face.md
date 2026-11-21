---
id: collect-261121-veille-ia/minimax/minimax-m3-1-flash-size-428b-params-252-gb-on-hugging-face
title: "MiniMax M3.1 Flash Size: 428B Params, 252 GB on Hugging Face"
domain: minimax
role: reference
task: article
actors: ["MiniMax"]
dates: ["2026-10-25"]
keywords: ["minimax", "flash", "model", "parameters", "experts", "token", "hugging", "face"]
source: https://blog.laozhang.ai/en/posts/minimax-m3-1-flash-size
source_anchor: ""
source_lines: [1, 116]
section: "MiniMax"
---
## On this page

MiniMax M3.1 Flash is not a small model. It has about 428B total parameters with 23B active per token, the same count as MiniMax M3, and the official Hugging Face repo `MiniMaxAI/MiniMax-M3.1-Flash` is 252.4 GB as of October 25, 2026. “Flash” refers to cost and speed: MiniMax trained the routed experts in 4-bit, which is why the download is under a third of M3's 854.2 GB BF16 repo. The “220B params” label on the Hugging Face page is a side effect of how those 4-bit weights are packed, not a different or smaller model.

All of those weights have to sit in memory, about 235 GiB for the official files, because the 23B active figure describes compute per token, not RAM. On four Blackwell GPUs (B200, B300, GB200, GB300) or four AMD MI355X, run the official checkpoint with vLLM 0.33.0 or later at tensor parallel 4; on Hopper, use a full 8× H100 or H200 node at TP8. A Mac Studio with 512 GB can hold `mlx-community/MiniMax-M3.1-Flash-Text-4bit`, while a 256 GB machine needs the 3-bit MLX build or an Unsloth GGUF at UD-Q3_K_XL or smaller, run on llama.cpp b11861 or newer. No version fits a single 24–32 GB GPU without pushing most of the model into system RAM, and if that is your machine, the MiniMax API at $0.15 input and $0.60 output per 1M tokens is the realistic way to use it.

Every file size below was read from the Hugging Face API on October 25, 2026, the parameter count was recomputed from the official `config.json`, and the memory fit table is arithmetic on those sizes. The weights were not downloaded or run, so none of the speed or quality figures below are first-hand: the few throughput numbers come from vLLM, SGLang, or a named community report.

## MiniMax M3.1 Flash size: 428B parameters in 252.4 GB of files

The size depends on whether you mean parameters or bytes, and for this model the two are far apart. MiniMax's model card on Hugging Face gives approximately 428B total parameters with 23B activated per token. On disk, the official repo held 72 files totaling 252,446,157,295 bytes on October 25, 2026: 250.1 GB (232.9 GiB) of main weights in 48 `model-*.safetensors` shards, 2.3 GB in a `dspark/` folder, and a handful of small tokenizer and config files.

The architecture explains the parameter count. The `config.json` describes 60 layers, 3 dense and 57 mixture-of-experts, and each MoE layer has 128 routed experts, of which 4 are picked per token, plus 1 shared expert. The routed experts alone come to 413.1B parameters. Per token, the 4 chosen routed experts use 12.9B parameters; adding the shared experts (3.23B), the dense layers (0.68B) and attention (6.42B) gives about 23.3B, or about 24.5B if you also count the 1.23B output head. Both land close to MiniMax's 23B. That figure sets the compute cost of each token. Any of the 128 experts can be chosen for the next token, so all 428B still have to live in GPU memory, unified memory, or RAM the engine can read from.

The files are small for a 428B model because the precision was set during training, not by a later quantization pass. The card says the routed experts and attention paths were quantized during training (quantization-aware training), and the `quantization_config` shows the layout: routed experts in NVFP4 (4-bit), attention and dense layers in MXFP8 (8-bit), and the embeddings, output head, vision tower and router gates in BF16. MiniMax publishes no BF16 version of M3.1 Flash, so the official repo is the full release, and there is no 850 GB-class file to compare against.

“Flash” does not mean fewer parameters. On the same date, the MiniMax-M3 repo stored 427.0B parameters in BF16 and totaled 854.2 GB, about 3.4 times the M3.1 Flash download, yet both models land at roughly 427B. According to its card, M3.1 Flash changed three things: sparse attention in every layer (M3 used full attention in its first three), the 4-bit training, and a new speculative-decoding draft model called DSpark. Anyone planning around a ~150B “lite” model will size their hardware or their download wrong.

The 2.3 GB `dspark/` folder is that draft model. vLLM loads it automatically when you turn on DSpark speculative decoding, and basic inference works without it. vLLM doesn't enable DSpark on Hopper GPUs at all, so on an H100 or H200 node the folder is dead weight; on Blackwell and AMD it is what makes the speculative-decoding speedup possible.

## Why Hugging Face says 220B params for MiniMax M3.1 Flash

The 220B figure in the Hugging Face sidebar counts stored elements, not parameters. The widget sums safetensors entries by data type, and the 4-bit experts are packed two weights per byte into `U8` tensors (named `weight_packed`, with separate scale tensors, and labeled `"nvfp4-pack-quantized"` in the config). Each U8 entry holds two weights, but the widget counts it once. Here is the metadata the Hugging Face API returned for the official repo on October 25, 2026, with the U8 count doubled:

The doubled U8 count, 413,122,166,784, is exactly the routed-expert count from `config.json` (57 layers × 128 experts × 3 matrices × 6,144 × 3,072), which confirms what those tensors hold. The corrected total of 427.1B matches the card's “approximately 428B” within rounding. The `unsloth/MiniMax-M3.1-Flash` mirror shows the same 220B because it carries the same files.

## Which MiniMax M3.1 Flash Hugging Face version to download

Choose the version by the runtime your hardware supports first, and by size second. For NVIDIA and AMD datacenter GPUs, the official repo is the default and the only one MiniMax's results cover; its card notes that “third-party quantizations may differ.” Macs need an MLX conversion, a consumer GPU with lots of system RAM needs a GGUF, and non-NVIDIA accelerators have FlagOS builds. The sizes below are the totals of the weight files in each repo or quant folder as of October 25, 2026, in decimal GB as Hugging Face displays them and in GiB, the unit that matters for memory.

| Version (Hugging Face repo or quant) | Weight files | Format | Runs on | 
|---|---|---|---|
| Official: `MiniMaxAI/MiniMax-M3.1-Flash` | 250.1 GB (232.9 GiB) + 2.3 GB DSpark | NVFP4 experts, MXFP8 dense | vLLM or SGLang on Blackwell, Hopper (TP8), AMD MI355X | 
| `unsloth/MiniMax-M3.1-Flash` | Identical to official | Mirror of the official files | Same as official | 
| `amd/MiniMax-M3.1-Flash-Quark-MXFP4` | 237.2 GB (220.9 GiB) + 2.3 GB DSpark | MXFP4 | SGLang on AMD MI350X / MI355X | 
| `FlagRelease/MiniMax-M3.1-Flash-W8A8-nvidia-FlagOS-Express` | 430.9 GB (401.3 GiB) + 2.3 GB DSpark | W8A8 (8-bit) | FlagOS vLLM plugin; sibling repos for other chips | 
| `mlx-community/MiniMax-M3.1-Flash-Text-4bit` | 248.4 GB (231.3 GiB) | MLX 4-bit, text only | Apple Silicon | 
| `mlx-community/MiniMax-M3.1-Flash-Text-3bit` | 186.5 GB (173.7 GiB) | MLX 3-bit, text only | Apple Silicon | 
| `unsloth/MiniMax-M3.1-Flash-GGUF` UD-Q8_K_XL | 259.1 GB (241.3 GiB) | GGUF | llama.cpp | 
| Unsloth GGUF UD-Q4_K_XL | 249.4 GB (232.3 GiB) | GGUF | llama.cpp | 
| Unsloth GGUF UD-Q3_K_XL | 194.0 GB (180.6 GiB) | GGUF | llama.cpp | 
| Unsloth GGUF UD-IQ3_S | 175.5 GB (163.4 GiB) | GGUF | llama.cpp | 
| Unsloth GGUF UD-IQ3_XXS | 160.0 GB (149.1 GiB) | GGUF | llama.cpp | 
| Unsloth GGUF UD-Q2_K_XL | 143.9 GB (134.0 GiB) | GGUF | llama.cpp | 
| Unsloth GGUF UD-IQ2_XXS | 134.9 GB (125.7 GiB) | GGUF | llama.cpp | 
| Unsloth GGUF UD-IQ1_M | 129.4 GB (120.5 GiB) | GGUF | llama.cpp | 

The 4-bit GGUF is the easiest trap. UD-Q4_K_XL is 249.4 GB, practically the size of the official checkpoint, because the official experts are already 4-bit; converting to a Q4 GGUF gives you a llama.cpp-compatible file, not a smaller one, and UD-Q8_K_XL is actually larger than the official release. Real savings start at 3 bits, and every step down re-quantizes weights that were trained to work at 4-bit.

The Unsloth GGUF repo's README opened with “Work in progress” and had last changed on October 25, 2026, so its quant list and sizes may still move, and its BF16 folder was incomplete. Image input needs the separate 3.5 GB `mmproj-F32.gguf` projector from the same repo, but no published guide says yet whether llama.cpp handles M3.1 Flash images correctly.

Many more builds exist: a Hugging Face search for the model name returned 49 repos on October 25, 2026, including other GGUF uploads, EXL3 builds, more MLX variants, and `primitive-ai/MiniMax-M3.1-Flash-pruned-272B-NVFP4` at 162.9 GB, a community-pruned 272B variant that is a different model from the official one. The “uncensored” and “abliterated” uploads in that list are modified models, not size options, and MiniMax's results don't extend to any of them.

## How much RAM to run MiniMax M3.1 Flash: fit by hardware

For a working setup, a version's weights should use no more than about 80% of the memory pool, leaving the rest for the KV cache, activations, and the runtime or operating system. That 80% is a rule of thumb applied to the table below, not a number from MiniMax or vLLM. By that rule the official checkpoint, 235.0 GiB including DSpark, wants a pool of about 294 GiB, more than any 256 GB machine has. A 256 GB machine holds only the 3-bit and smaller versions with room to spare.

| Hardware (memory pool) | Fits with ~20% headroom | Fits the weights only | What runs it | 
|---|---|---|---|
| One RTX 5090 or other 24–32 GB GPU | Nothing | Nothing | llama.cpp with experts in system RAM, or the API | 
| DGX Spark or 4× RTX 5090 (128 GB) | Nothing | UD-IQ1_M (120.5 GiB, 7.5 GiB left) or UD-IQ2_XXS (125.7 GiB, 2.3 GiB left) | llama.cpp, with almost no room for context | 
| Mac Studio 256 GB, 2× DGX Spark (256 GB) | MLX 3-bit; GGUF UD-Q3_K_XL and smaller | Official, AMD MXFP4, MLX 4-bit, UD-Q4_K_XL, UD-Q8_K_XL | Mac: MLX or llama.cpp; Spark pair: no published recipe | 
| Mac Studio 512 GB | Every MLX and GGUF version | — | MLX or llama.cpp; official, AMD and FlagOS files don't run on a Mac | 
| 4× H200 (564 GB) or 8× H100 (640 GB) | Every version, FlagOS W8A8 included | — | vLLM's Hopper recipe: TP8 on a full 8-GPU node | 
| 4× B200 (768 GB) or 4× B300 (1,152 GB) | Every version | — | vLLM at TP4 with DSpark; SGLang's B300 recipe at TP4 | 

Capacity is only half of fit. The official NVFP4 checkpoint needs vLLM or SGLang on CUDA or ROCm, so a 512 GB Mac Studio can hold it but cannot run it, and Mac owners need the MLX or GGUF versions instead. The same caution applies to 4× H200: the memory is enough, but vLLM's published Hopper setup uses all eight GPUs of a node. For DGX Spark, kie.ai cites a community report of four Sparks running the model on vLLM at TP4.

Context length adds to the bill on top of the weights. MiniMax says usable context and throughput “depend on GPU memory and the serving configuration,” and vLLM's Hopper example caps context at 262,144 tokens rather than the full 1M. The SGLang cookbook reports a KV pool of about 10.4M tokens on 4× B300 at TP4, but that pool is shared by every request on the server, so it is a server's total capacity, not what a single 1M-token conversation needs.

## Serving MiniMax M3.1 Flash with vLLM: TP4 on Blackwell, TP8 on Hopper

Serve the official repo with vLLM 0.33.0 or later. The vLLM recipe for MiniMax-M3.1-Flash, updated October 23, 2026, covers three setups on the same checkpoint: NVIDIA Blackwell (GB200, GB300, B200, B300) at TP4 with the NVFP4 experts running natively on FP4 tensor cores, AMD MI355X on ROCm 7.2+ at TP4 with the experts re-quantized to MXFP8 at load, and NVIDIA H100 or H200 at TP8 on a full node. Every variant needs `--block-size 128`, because MiniMax Sparse Attention uses 128-token blocks, and uses `--reasoning-parser minimax_m3`. Tool calling is a separate switch: only the Hopper example includes `--enable-auto-tool-choice --tool-call-parser minimax_m3`, so add those two flags to the Blackwell or AMD command if your client sends tools.

Download the weights with the Hugging Face CLI, as the model card shows, and make sure the target disk has room for the whole repo:

Single node, 4× GB200, from the recipe:

Single node, 8× H100 or H200, from the same recipe:

Hopper has no FP4 tensor cores, so on H100 and H200 the KV cache and the sparse-attention indexer cache use FP8, the NVFP4 experts run on the Marlin MoE kernel, there is no DSpark, and the `video: 0` limit turns off video input while images still work. If you leave out the block size, vLLM fails with `No common block size for 16`. Expect a slow first start as well: the recipe says a cold start with JIT builds and CUDA graph capture can take longer than 10 minutes, and its data-parallel examples raise `VLLM_ENGINE_READY_TIMEOUT_S` to 3600.

SGLang is the other engine MiniMax recommends, but as of October 25, 2026 its M3.1 Flash support is not in a release yet: the SGLang cookbook for MiniMax M3.1 says to install from a support pull request that it does not yet link, or to use the `lmsysorg/sglang:dev-minimax-m31-flash` dev image. The cookbook has recipes for B300 (4 GPUs, TP4, low latency), B200 (8 GPUs, TP8, high throughput) and AMD MI350X/MI355X at TP4 with the AMD MXFP4 build, and no Hopper recipe. Its sparse-attention kernels need CUDA toolkit 13.4 or newer; with an older toolkit, SGLang silently uses a slower path. For scale, the cookbook's own benchmark on 4× B300 with its MonoKernel path reports 1,028 tokens/s at 1K context and 723 tokens/s at 1M.

On non-NVIDIA accelerators, the BAAI-led FlagOS project publishes W8A8 builds converted from the official weights for nine chip vendors, among them Huawei Ascend, Hygon, MetaX, Moore Threads, Kunlunxin and T-Head, under `FlagRelease/` on Hugging Face and on ModelScope. Being 8-bit, they need well over 400 GB of memory, and the published example runs vLLM-plugin-FL at tensor parallel 8.

## Can you run MiniMax M3.1 Flash locally with llama.cpp or a Mac?

Yes, through community conversions, and for GGUF the llama.cpp version decides whether you get answers or noise. Builds before b11861 load M3.1 Flash GGUFs without any error and then produce garbled text, because llama.cpp's MiniMax M3 code ignored the per-expert NVFP4 scales. The fix, llama.cpp pull request #31097, was merged on October 24, 2026 and first shipped in release b11861 that day; b11871 was the newest release on October 25. If a GGUF returns token soup, update llama.cpp before you blame the quant.

Unsloth has no M3.1 Flash guide yet, so the closest reference is its MiniMax M3 local guide. It calls those GGUFs experimental and text-only, says MiniMax Sparse Attention is not supported, so inference falls back to dense attention, and advises keeping context modest because dense attention at long context uses a lot of memory. For M3 it lists UD-IQ1_M as needing at least 133 GB of RAM and recommends UD-IQ3_XXS. That is M3 guidance; whether the M3.1 Flash GGUFs keep the same limits is not documented.

A single consumer GPU works only with the experts offloaded to system RAM. In llama.cpp issue #31063, the reporter ran an RTX 5090 with 128 GB of RAM and a 146 GiB Q2_K file they had converted themselves, not one of the Unsloth quants, with the experts on the CPU (`-cmoe`). After applying the fix locally, the full 60-layer model answered coherently at 3.59 tokens/s for prefill and 5.12 tokens/s for decoding. That is one community sample, but at that decode rate a 2,000-token reasoning answer takes about six and a half minutes. If you want a model that truly fits one 32 GB card, the Qwen3.8-27B VRAM Guide: RTX 5090, Dual GPUs, and 262K Context is a better starting point.

The IQ1 and IQ2 files make the model fit smaller machines, but no published evaluation compares them with the official weights. If you plan to drive a coding agent with one, Local Coding Agent Fails? Diagnose Quantization, Context, or the Harness shows how to tell quantization damage from context or harness problems.

On a Mac, the official repo is not an option, because its NVFP4 and MXFP8 tensors are built for vLLM and SGLang on CUDA or ROCm. Use `mlx-community/MiniMax-M3.1-Flash-Text-4bit` on a 512 GB Mac Studio or the Text-3bit build on 256 GB, keeping in mind that both are text-only conversions, or run a GGUF in llama.cpp. Ollama's `minimax-m3.1-flash:cloud` tag is not a local option either: it runs the model on Ollama's servers and uses none of your memory.

## Is MiniMax M3.1 Flash free? Open weights under a community license

The weights are free to download, with no gated sign-up, and free to use for non-commercial purposes. They are open weights under the MiniMax Community License, not an OSI-approved open-source license. Commercial use, which the license defines to include paid products and services, commercial API use, and deploying fine-tuned or otherwise modified versions, comes with two conditions. You must prominently display “Built with MiniMax M3.1-Flash” on a related website, interface, blog post, about page or product documentation, and you must contact MiniMax at api@minimax.io: below US$20 million in yearly revenue a one-time notice with the subject “M3.1-Flash licensing — notice” is enough, while above that you need prior written authorization. An appendix also prohibits uses such as military purposes and harming minors. This summarizes the license file as of October 25, 2026 and is not legal advice.

## When the MiniMax M3.1 Flash API beats buying hardware

If you don't already have a 512 GB Mac Studio, a four-GPU Blackwell server, or a full Hopper node, the hosted model is the practical choice. As of October 25, 2026, the MiniMax API pricing page lists MiniMax-M3.1-Flash at $0.15 per 1M input tokens, $0.60 per 1M output tokens and $0.015 per 1M cached input tokens, half of MiniMax-M3's standard input and output rates ($0.30 and $1.20 up to 512K context). A month with 10M input and 2M output tokens costs 10 × $0.15 + 2 × $0.60 = $2.70 before caching. Third-party API guides report that thinking cannot be switched off and that effort defaults to `max` when you don't set it, so budget the output side for reasoning tokens.

The decision gets harder only when you own the hardware already or need the weights on your own machines for privacy or licensing reasons. For that broader trade-off in agent work, Best Local LLM for Coding Agents: When Local Beats Claude/Codex walks through the cost and quality routing. If you are choosing between open-weight “Flash” models, GLM-5.3-Flash raises the same question at a larger size, with a 305.8 GiB FP8 checkpoint, covered in GLM-5.3-Flash Guide: Ox Alpha, API Pricing, and Local Deployment.

## ## Sources9

External pages this guide links to, in the order they appear. Last updated Oct 25, 2026.


## Sources9

External pages this guide links to, in the order they appear. Last updated Oct 25, 2026.
