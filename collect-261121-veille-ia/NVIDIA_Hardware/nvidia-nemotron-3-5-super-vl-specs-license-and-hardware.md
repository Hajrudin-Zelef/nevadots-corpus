---
id: collect-261121-veille-ia/nvidia-hardware/nvidia-nemotron-3-5-super-vl-specs-license-and-hardware
title: "NVIDIA Nemotron 3.5 Super VL: Specs, License and Hardware"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: ["2026-10-15"]
keywords: ["nvidia", "license", "model", "nemotron", "super", "active", "tokens", "open-weight"]
source: https://www.faradaymachines.com/models/nemotron-3-5-super-vl/
source_anchor: ""
source_lines: [1, 175]
section: "NVIDIA_Hardware"
---
# NVIDIA Nemotron 3.5 Super VL: Specs, License and Hardware

NVIDIA’s 120B-parameter open-weight vision-language model, with 12B active per token, released on October 15, 2026 under the permissive OpenMDW license. At 4-bit it runs on a single 256 GB Mac Studio, Faraday’s Growth tier.

## Key takeaways

- NVIDIA released Nemotron 3.5 Super VL on October 15, 2026: a hybrid Mamba-2 and mixture-of-experts model with 120B total and 12B active parameters that takes text, images and video, with up to 1M tokens of context according to its model card.
- NVIDIA lists the OpenMDW License Agreement 1.1 for the model, a permissive Linux Foundation license that allows commercial use and derivative models without requiring you to publish your changes.
- Artificial Analysis scores it 26 on its Intelligence Index v4.3, well above the median of 8 for open-weight models of similar size, but below larger open models such as GLM-5.3-Flash (42), MiMo-V2.6-Flash (38) and MiniMax M3 (29).
- NVIDIA reports 71.1 on SWE-bench Verified, 51.78 on SWE-bench Pro and 62.82 on MMLongBench-Doc; these are vendor-reported scores, not independent tests.
- Unsloth’s 4-bit GGUF build is 86.9 GB and Unsloth recommends 128 GB of memory for it, but in Faraday’s own tests it didn’t fit a 128 GB Mac Studio with room to work, so we run it on Growth (one Mac Studio M5 Ultra, 256 GB), which also takes the 8-bit build (132 GB); full BF16 (about 240 GB, our estimate) needs Scale.

## Model Story

NVIDIA released NVIDIA-Nemotron-3.5-Super-VL-120B-A12B on October 15, 2026, on Hugging Face and build.nvidia.com. It belongs to NVIDIA’s Nemotron 3.5 family, alongside the smaller Nemotron 3.5 Lightning (30B total, 3B active). There are two checkpoints, NVFP4 (labelled “v1.0 - NVFP4 GA” on its model card) and full-precision BF16, plus an NVIDIA NIM container served with vLLM.

The design mixes three kinds of layers: Mamba-2, mixture-of-experts and attention. Of its 120 billion parameters, only 12 billion are active for each token, though all of them have to sit in memory. NVIDIA also uses Multi-Token Prediction, which allows speculative decoding without a separate draft model. A C-RADIO vision encoder lets it read images and video as well as text; one review says up to about two minutes of video. NVIDIA’s technical report says Super beats the larger Nemotron 3 Ultra while using about 4.6× fewer active parameters.

NVIDIA is a US company, so this is a US open-weight model. Our article on the US–China open-weight race sets US releases against Chinese ones, and the open-weight AI license tracker compares their licenses.

## Key Specifications

| Developer | NVIDIA (United States) | 
|---|---|
| Release Date | October 15, 2026 (Hugging Face and build.nvidia.com) | 
| Architecture | Hybrid mixture-of-experts with interleaved Mamba-2, MoE and attention layers; Multi-Token Prediction; C-RADIO vision encoder | 
| Parameters | 120 billion total / 12 billion active per token | 
| Context Window | Up to 1M tokens per the model card; served context depends on the provider (OpenRouter mentions 300K for some deployments) | 
| Modalities | Text, image and video input | 
| License | OpenMDW License Agreement 1.1 (Linux Foundation, permissive, commercial use allowed) | 
| Checkpoints | NVFP4 and BF16 on Hugging Face; NVIDIA NIM container (vLLM) | 
| API Price Range | About $0.10–0.25 per 1M input tokens and $0.40–0.60 per 1M output tokens, depending on provider | 
| AA Index v4.3 | 26 (Artificial Analysis Intelligence Index v4.3.x) | 
| Faraday Tier | Growth (1× Mac Studio M5 Ultra, 256GB) at 4-bit | 

## API Pricing

API prices vary by provider. OpenRouter has listed $0.10–0.15 per million input tokens and $0.40–0.50 per million output tokens, along with a free variant, while Artificial Analysis puts the median across providers at $0.23 and $0.57. The context window you get also depends on who serves the model.

## Benchmarks

Most published scores come from NVIDIA’s own runs; the one independent measure is the Artificial Analysis Intelligence Index.

| Benchmark | Area | Score | Source | 
|---|---|---|---|
| SWE-bench Verified | Coding and agents | About 71.1 | NVIDIA-reported (model card 71.10; MarkTechPost 71.07) | 
| SWE-bench Pro | Coding and agents | 51.78 | NVIDIA-reported | 
| Terminal-Bench 2.1 | Coding and agents | About 61 (sources vary, 60.67–66.70) | NVIDIA-reported | 
| OSWorld-Verified | Coding and agents | 51.14 | NVIDIA-reported | 
| BrowseComp | Coding and agents | 77.5 | NVIDIA-reported, via MarkTechPost | 
| GPQA-Diamond | Reasoning | About 89.5–89.9 | NVIDIA-reported (89.52 model card, per one review; 89.90 NVFP4, per MarkTechPost) | 
| V* (BF16) | Vision and documents | 85.60 | NVIDIA-reported | 
| MMLongBench-Doc | Vision and documents | 62.82 | NVIDIA-reported | 
| CharXiv reasoning | Vision and documents | 80.82 | NVIDIA-reported | 
| GraphWalks | Long context | 89.71 (BF16); 67.69 (NVFP4) | NVIDIA-reported (model card) | 
| AA Intelligence Index v4.3 | Overall | 26 (26.49) | Independent (Artificial Analysis) | 

As of October 19, 2026. Vendor-reported scores come from the vendor’s own runs and harnesses, so they aren’t strictly comparable with other labs’ numbers.

On SWE-bench Pro, NVIDIA’s 51.78 is ahead of its own Nemotron 3 Ultra (49.66) but behind Qwen3.8 Flash-Next (60.37), both also NVIDIA-reported. NVIDIA also claims up to 5× the throughput of open models in its size class, and MarkTechPost reports over 600 output tokens per second on Artificial Analysis agentic tasks through a Baseten endpoint. Both come from data-centre GPUs, not Macs, and depend on the workload.

The independent picture is more modest. Artificial Analysis scores it 26 on Intelligence Index v4.3, well above the median of 8 it notes for open-weight models of similar size, but well below the larger open models Faraday lists: GLM-5.3-Flash at 42, MiMo-V2.6-Flash at 38 and MiniMax M3 at 29. It costs roughly $0.20–0.35 per Index task, depending on provider pricing, and generated about 170M tokens on the Index against a 100M median. Scores from v4.1 use a different scale.

## How It Compares by Size and License

| Model | Total / active params | License | AA Index v4.3 | Faraday fit at 4-bit | 
|---|---|---|---|---|
| Nemotron 3.5 Super VL (NVIDIA) | 120B / 12B | OpenMDW-1.1 | 26 | Growth (~87 GB) | 
| Qwen3.8-27B (Alibaba) | 27B dense | Apache 2.0 | — | Starter (~17 GB) | 
| MiniMax M3 (MiniMax) | ~428B / ~23B | See the license tracker | 29 | Scale (~240 GB, a tighter fit) | 
| MiMo-V2.6-Flash (Xiaomi) | 309B / 15B | MIT | 38 | Scale (~170 GB) | 
| GLM-5.3-Flash (Z.ai) | 320B / 18B | MIT | 42 | Scale (~180 GB) | 

AA Index is the Artificial Analysis Intelligence Index v4.3.x. “—” means we don’t have a reliable v4.3 score. Sizes for the other models are roughly 0.55 bytes per parameter at 4-bit. Unsloth’s 1- to 2-bit builds of GLM-5.3-Flash fit Growth, with some loss of quality.

Nemotron 3.5 Super VL is more than four times the size of Qwen3.8-27B, which is why it needs Growth while Qwen3.8-27B runs on Starter. GLM-5.3-Flash and MiMo-V2.6-Flash score higher on the independent index but need roughly twice the memory at 4-bit, which puts them on Scale. Nemotron’s strengths are its size, vision and document work, and its license, not top general intelligence.

## Quantized Versions

Which build you run depends on your hardware and how much quality you’ll trade for memory.

| Build | Size | Memory Unsloth recommends | Faraday fit | 
|---|---|---|---|
| NVIDIA NVFP4 checkpoint | ~75 GB (community report) | — (NVIDIA Blackwell format) | For NVIDIA GPUs; on a Mac, use the MLX or GGUF builds below | 
| MLX 4-bit / MLX nvfp4 (community) | ~70–80 GB (69.5 GB text-only 4-bit; 79.6 GB nvfp4) | — | Growth (close to the 4-bit GGUF size, which didn’t fit 128 GB with room to work in our tests) | 
| Unsloth GGUF UD-IQ1_M (1-bit) | 46.1 GB | 64 GB | Not our pick: we run the 4-bit build on Growth rather than trade quality at 1-bit | 
| Unsloth GGUF UD-Q4_K_XL (4-bit) | 86.9 GB | 128 GB | Growth (in our tests it didn’t fit 128 GB with room to work) | 
| Unsloth GGUF UD-Q5_K_XL / UD-Q6_K_XL | 111 GB / 121 GB | — | Growth (too tight for 128 GB once working memory is counted) | 
| Unsloth GGUF Q8_0 (8-bit) | 132 GB | 192 GB | Growth | 
| BF16 (full precision) | ~240 GB (our estimate, 2 bytes per parameter) | — | Scale | 

Sizes from Unsloth’s guide and Hugging Face repository, community MLX reports and community reports for NVFP4. “—” means our sources give no recommendation. Starter is 128 GB, Growth 256 GB and Scale 512 GB of unified memory, each on a single Mac Studio.

NVFP4 is NVIDIA’s 4-bit format for its Blackwell GPUs, so on a Mac you use a community MLX build or one of Unsloth’s GGUF builds instead. The GGUF builds run in llama.cpp, and LM Studio lists the model. The 69.5 GB MLX build is text-only, so it drops vision.

File size is not the same as the memory a model needs. Unsloth notes that the operating system, the KV cache (working memory that grows with the conversation), the vision projector and long contexts or video all need room on top of the file. That is why Unsloth recommends 128 GB for an 86.9 GB 4-bit file. In our own tests on a 128 GB Mac Studio, though, the 4-bit build didn’t leave enough room to work once working memory was counted, so we run it on Growth (256 GB); the 111 GB and 121 GB builds are tighter still. Our rule: a model fits a tier only when the whole model sits in unified memory with headroom for working memory.

Lower precision also costs some quality. NVIDIA’s own model card shows a GraphWalks long-context score of 67.69 for the NVFP4 checkpoint against 89.71 for BF16, so a 4-bit build can lose ground on long-context work. That figure covers NVIDIA’s checkpoint, not the MLX or GGUF builds, but lower-bit builds generally trade quality most on long or precise work, which is also why we run the 4-bit build on Growth rather than a 1-bit build.

In tier terms: Growth runs the model at 4-bit or as an 8-bit build that is closer to full quality, and Scale runs full BF16. Starter’s 128 GB didn’t leave enough room in our tests.

## Running It on Faraday

Faraday Machines installs open-weight models on Mac Studio hardware in Toronto. Every plan is one machine on a 36-month term: Starter is a Mac Studio M5 Max with 128 GB of unified memory ($299 US a month), Growth a Mac Studio M5 Ultra with 256 GB ($599 a month) and Scale a Mac Studio M5 Ultra with 512 GB ($999 a month).

Nemotron 3.5 Super VL runs on Growth at 4-bit. Unsloth recommends 128 GB for its 4-bit file, but in our tests it didn’t fit a 128 GB Mac Studio with room to work, so we don’t run it on Starter. Next to Qwen3.8-27B, the small, quick option at about 17 GB that runs on Starter, it brings a much larger model to images, documents and agent tasks.

Growth also has room for a higher-precision 8-bit build, and Scale runs full BF16. That matters most for long-context work. The larger models use most of a tier’s memory, so they run one at a time rather than side by side. On Growth, for example, the 8-bit Nemotron build (132 GB) and a 1- to 2-bit GLM-5.3-Flash build (about 93–109 GB) would fill nearly all of the 256 GB between them.

We don’t quote Mac speeds for this model; the vendor throughput figures above don’t carry over to a Mac Studio. For general reasoning, GLM-5.3-Flash on Scale scores higher on the independent index; see pricing for what each tier includes.

## License

NVIDIA’s Nemotron page lists the OpenMDW License Agreement, version 1.1, for all Nemotron 3.5 models. OpenMDW is a permissive license for AI models from the Linux Foundation, developed with Amazon, Meta, IBM and Microsoft, and version 1.1 was released on May 28, 2026. It covers weights, data, code and documentation under one license. Commercial use and derivative models are allowed, and you don’t have to publish your modifications or fine-tuned weights. It was submitted to the Open Source Initiative in mid-August 2026; approval hasn’t been granted yet.

That is simpler than several custom licenses in our license tracker. Kimi K3 uses a custom license with thresholds for very large commercial deployments. GLM-5.3 requires model-as-a-service businesses above $10 billion in annual revenue to pass Z.ai’s security review. Muse Spark Open’s license carries a “Built with” display duty, and our sources show no such duty for OpenMDW. Still, read the license text for notice and redistribution terms before you ship anything built on the model. This is not legal advice.

## Frequently Asked Questions

## ### Can Nemotron 3.5 Super VL run on a Mac Studio?


                            Yes, on a 256 GB machine. At 4-bit it runs on a single Mac Studio M5 Ultra with 256 GB of unified memory, which is Faraday Machines’ Growth tier. Unsloth’s 4-bit GGUF build is 86.9 GB and Unsloth recommends 128 GB of memory for it, but in our own tests it didn’t fit a 128 GB Mac Studio with room to work once working memory was counted, so we run it on Growth rather than Starter. Community MLX 4-bit builds are about 70–80 GB. On a Mac you use these builds, not NVIDIA’s own NVFP4 checkpoint, which targets NVIDIA Blackwell GPUs. We don’t publish Mac speed figures for this model.

## ### What license does Nemotron 3.5 Super VL use?


                            It uses the OpenMDW License Agreement, version 1.1, which NVIDIA lists for all Nemotron 3.5 models. OpenMDW is a permissive license for AI models from the Linux Foundation, covering weights, data, code and documentation. It allows commercial use and derivative models, and you don’t have to publish your modifications or fine-tuned weights. Check the license text for notice and redistribution terms before you deploy. This is not legal advice.

## ### How does Nemotron 3.5 Super VL compare with GLM-5.3-Flash and Qwen3.8-27B?


                            By size it sits between them, and by independent score it trails GLM-5.3-Flash. Artificial Analysis rates Nemotron 3.5 Super VL 26 on Intelligence Index v4.3, against 42 for GLM-5.3-Flash, an MIT-licensed 320B model that needs Scale at 4-bit. Qwen3.8-27B is a 27B dense Apache 2.0 model of about 17 GB at 4-bit. Nemotron, at 120B under OpenMDW-1.1, runs on Growth at 4-bit.

## ### What is the NVFP4 version of Nemotron 3.5 Super VL?


                            It is NVIDIA’s own 4-bit checkpoint, built for NVIDIA Blackwell GPUs. NVIDIA made it with quantization-aware distillation from the BF16 model, and community reports put it at about 75 GB. MarkTechPost says it runs on one DGX Spark, one H200, two H100s or one GB200. NVIDIA’s model card shows a GraphWalks long-context score of 67.69 for NVFP4 against 89.71 for BF16. Macs use MLX or GGUF builds instead.

## ### Is Nemotron 3.5 Super VL good at documents and images?


                            Documents and images are among its strengths, according to NVIDIA’s own scores. NVIDIA reports 62.82 on MMLongBench-Doc, 80.82 on CharXiv reasoning and 85.60 on V* for the BF16 model. It reads text, images and video through a C-RADIO vision encoder, and one review says it handles up to about two minutes of video. These are vendor-reported figures, so test it on your own documents first.

## References

**[1]** NVIDIA. (2026). “NVIDIA-Nemotron-3.5-Super-VL-120B-A12B-NVFP4” model card. Available at: huggingface.co

**[2]** NVIDIA. (2026). “NVIDIA-Nemotron-3.5-Super-VL-120B-A12B-BF16” model card. Available at: huggingface.co

**[3]** NVIDIA. (2026). “Nemotron 3.5 Super VL 120B A12B” model card. Available at: build.nvidia.com

**[4]** NVIDIA. (2026). “Nemotron 3.5 Super and Lightning” technical report. Available at: research.nvidia.com

**[5]** MarkTechPost. (2026). NVIDIA Nemotron 3.5 Super VL release. October 15, 2026. Available at: marktechpost.com

**[6]** Artificial Analysis. (2026). “Nemotron 3.5 Super VL” model page. Available at: artificialanalysis.ai

**[7]** OpenRouter. (2026). “Nemotron 3.5 Super VL” listing. Available at: openrouter.ai

**[8]** Unsloth. (2026). “Nemotron 3.5 Super VL” documentation and GGUF builds. Available at: unsloth.ai; huggingface.co

**[9]** NVIDIA. (2026). Nemotron model license table. Available at: nvidia.com

**[10]** OpenMDW License: Linux Foundation on Hugging Face; BetaNews

**[11]** MLX build sizes: OrcaRouter; DEV Community

**[12]** Artificial Analysis. (2026). “Artificial Analysis Intelligence Index v4.3” and model pages for GLM-5.3-Flash and MiMo-V2.6-Flash. Available at: artificialanalysis.ai

## Talk to Us About Running Nemotron Yourself

Faraday Machines installs open-weight models on Mac Studio hardware in Toronto, so your prompts and documents stay on equipment you control. If you’re weighing whether a 120B vision model on a single unit fits your work, we’re happy to talk it through.

Talk to Us
