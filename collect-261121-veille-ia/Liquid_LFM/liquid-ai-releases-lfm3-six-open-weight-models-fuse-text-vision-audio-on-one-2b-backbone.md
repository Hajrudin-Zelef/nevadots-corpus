---
id: collect-261121-veille-ia/liquid-lfm/liquid-ai-releases-lfm3-six-open-weight-models-fuse-text-vision-audio-on-one-2b-backbone
title: "Liquid AI Releases LFM3: Six Open-Weight Models Fuse Text, Vision, Audio on One 2B Backbone"
domain: liquid-lfm
role: reference
task: article
actors: ["Liquid AI"]
dates: []
keywords: ["attention", "layers", "lfm3", "global", "liquid", "key-value", "cache", "open-weight"]
source: https://www.techtimes.com/articles/329588/20261028/liquid-ai-releases-lfm3-six-open-weight-models-fuse-text-vision-audio-one-2b-backbone.htm
source_anchor: ""
source_lines: [1, 87]
section: "Liquid_LFM"
---
Liquid AI today released LFM3, a family of six open-weight models that collapses text, vision, audio, and structured-decision capabilities into a single shared backbone of just 1.7 billion parameters — and made all six available immediately for download and fine-tuning on a single consumer GPU. The release, published on Hugging Face, is the company's most significant update since the open-source LFM2 release in 2025 and represents the commercial output of an architecture search conducted across 14 different device and backend combinations.

The headline performance figure: on the WebArena-Verified benchmark, LFM3-1.7B scores 38.42 against the 3.02 scored by LFM2.5-2.6B — a 12.7x improvement on agentic tasks — despite using 35% fewer parameters than its predecessor. That number did not come from scaling up. It came from a new three-regime architecture and a dedicated 3.5-trillion-token mid-training stage built explicitly for agentic tasks.

### Three Memory Regimes, Not One

The defining technical change in LFM3 is its tri-hybrid layer design — and to understand why it matters, it helps to understand what it replaced.

LFM2 combined two types of layers: gated short convolutions (which process tokens locally, holding O(1) state regardless of sequence length) and global attention layers (which attend to every prior token but grow the key-value cache linearly with sequence length). The dual-hybrid approach was already more memory-efficient than a standard transformer at edge scale, but it left a gap: there was no intermediate mechanism for capturing patterns at medium range — dependencies spanning dozens to a few hundred tokens that are too long for a convolution but don't require global context.

LFM3 fills that gap with a third layer type: sliding-window attention (SWA), drawn from the same design lineage as Mistral and Gemma's efficient-inference layers. The result is 24 layers organized across three distinct memory and compute regimes:

- **13 gated short convolutions** (kernel size 7, up from 3 in LFM2): O(1) state, fastest on CPU and NPU, no key-value cache contribution at all
- **5 sliding-window attention layers** (256-token window): bounded O(256) key-value cache, capturing local-to-medium context
- **6 global attention layers** : full O(n) key-value cache for long-range reasoning, but only 6 of 24 layers contribute

The practical consequence of this three-way split is that LFM3 reduces total key-value cache footprint to approximately 27% of what an equivalent 24-layer standard transformer would require. The details are documented in Liquid AI's Hyena Edge research. Convolutions handle local pattern recognition — syntax, word-level dependencies, token sequences — with no memory cost. SWA captures paragraph-scale context. Global attention handles the rare cases where a token 10,000 positions away is decisive. Each layer type does what it is actually suited for.

Three additional refinements accompany this design. Head-wise gated attention applies a sigmoid gate per attention head, allowing individual heads to suppress their output entirely when they have nothing to contribute — which helps prevent the activation outliers that degrade quantization quality. These architecture refinements are detailed in the Hugging Face blog. Partial RoPE rotates only the highest-frequency half of each global attention head's embeddings, leaving the lower half position-agnostic: global attention heads can therefore match tokens on semantic content regardless of how far apart they appear, useful for long documents where the same concept recurs throughout. Sandwich norm normalizes both input and output for each block, stabilizing activations during training and inference.

### Hardware-in-the-Loop: Why the Ratios Were Chosen

The specific mix of 13 convolutions, 5 SWA, and 6 global attention layers was not designed by hand. Liquid AI ran its STAR hardware-in-the-loop architecture search across 14 device and backend combinations — phones, Raspberry Pi 5, laptops, cloud CPUs, cloud GPUs, and the Qualcomm Hexagon NPU — profiling latency-quality tradeoffs on real hardware rather than optimizing purely for perplexity on a server. This hardware-in-the-loop approach means the layer ratios reflect what actually runs well on the devices the models are intended for, not what would be theoretically optimal on datacenter GPUs.

The resulting base model, LFM3-1.7B-Base, was pretrained on 22 trillion tokens. The final 3.5 trillion tokens constitute a dedicated mid-training stage on curated data with a higher proportion of agentic traces, tool calls, and long-context sequences — this stage also extends the context window to 256K tokens. At 4-bit precision, the text model fits in approximately 1 GB, placing it within range of mid-range smartphones.

### Agentic Performance: Where the 12x Jump Comes From

LFM3-1.7B-Base is post-trained to operate inside agentic harnesses including Hermes Agent, Pi, and OpenClaw. The WebArena-Verified score of 38.42 versus LFM2.5-2.6B's 3.02 reflects a combination of architectural improvements and the dedicated mid-training stage. The full WebArena benchmark results are documented in the release blog. The architecture gives the model longer effective context at lower memory cost; the mid-training data gives it task-specific experience with browser navigation, tool calls, and multi-step decision sequences.

Liquid AI also reports that LFM3-1.7B outperforms Qwen3.5-9B — a model more than five times its size — on long tasks in browsers and business applications, though that comparison is self-reported and has not been independently verified. LFM3-1.7B is also approximately 25% faster than LFM2 at 8K context on equivalent hardware, reflecting the KV cache reduction benefit.

### Custom Vision Encoder: Fewer FLOPs, Comparable Accuracy

For the vision-language variant LFM3-VL-2B, Liquid AI built a custom 164M-parameter vision encoder rather than reusing SigLIP2 or a Qwen encoder. The encoder combines convolution and attention layers, using 3x less compute than SigLIP2 according to Liquid AI's internal measurements. Benchmarked against Qwen3.5-2B's vision encoder on a shared LFM2.5-230M language backbone, Liquid AI's encoder matches it while requiring 2.2x fewer FLOPs.

For video, the encoder embeds each consecutive pair of sampled frames into a single temporal token, halving the temporal sequence length compared to per-frame encoding.

The resulting model, LFM3-VL-2B, raises the average score across 62 vision benchmarks from 57.6 (LFM2.5-VL-3B) to 71.4 — and uses 1.9B parameters where its predecessor used 3.1B. Liquid AI says the result is on par with Gemma 4 12B, a model more than six times its size; that Gemma 4 12B comparison is self-reported. LFM3-VL-2B is the first LFM vision model to support video input and performs chain-of-thought reasoning before answering.

### Robotics: From Vision Model to Action Model

Liquid AI used LFM3-VL-2B as the starting point for three complementary robotics models: an orchestrator that grounds objects in images, invokes motion planners via tool calls, and verifies outcomes from subsequent camera frames; a navigator that predicts spatial targets and waypoint trajectories from RGB observations; and a vision-language-action model trained across 15 robotic embodiments.

The VLA model achieved 96.2% average success across the four LIBERO benchmark suites and 45.7% on RoboCasa-GR1 tabletop tasks — both self-reported figures. These LIBERO and RoboCasa benchmark results appear in the release documentation. On a Jetson AGX Orin running TensorRT FP16, action chunks generate in under 50 ms, fast enough for real-time control on Jetson hardware.

### LiquidCodec and the Audio Models

The audio encoder in LFM3-Audio-2B mixes sliding-window attention and short convolutions and is fully causal — it can begin prefilling while the user is still speaking, enabling time-to-first-token as low as 50 ms. The speech tokenizer, LiquidCodec, converts model output to audio at approximately 87x real time on a single CPU thread on an Apple M4 Max, and supports streaming decoding so spoken replies can begin playing before generation is complete.

On the UTMOSv2 speech quality benchmark, LFM3-Audio-2B scores 3.83 versus the 3.22 scored by LFM2.5-Audio-1.5B — a 19% improvement. The UTMOSv2 audio benchmark results are included in the release blog. The model handles voice chat, speech recognition, synthesis, and translation across 18 languages, and can convert spoken requests directly into tool calls.

LFM3-Omni-2B — the first omni model in the family — accepts text, images, video, and audio simultaneously and produces both text and speech. Liquid AI reports it outperforms models up to 10.7B parameters, including Qwen2.5-Omni-7B, on the Daily-Omni, WorldSense, and AV-SpeakerBench benchmarks; these Qwen2.5-Omni benchmark comparisons are self-reported.

### Decision Models: Calibrated Probability in One Forward Pass

The d1 series takes a structurally different approach from the rest of the LFM3 family. Rather than generating open-ended text, d1-2B and d1-omni-2B output structured decisions: given a situation — a support ticket, an invoice, an app screenshot, or a voice note — and a list of questions, they answer all questions simultaneously in a single forward pass, each with a calibrated probability score.

Supported answer types extend beyond binary yes/no: the models can identify and point at objects in an image, read values from documents, estimate numbers within a range, and select from multi-select structured output.

d1-2B replaces the earlier d1-3B model, cutting parameter count by a third while outperforming it on Liquid AI's internal benchmarks — 75.5 versus 64.0 on ImajevBench. The d1-2B ImajevBench result is self-reported. d1-omni-2B extends these capabilities to audio input and retains accuracy with voice input, according to Liquid AI.

### What Developers Can Do Today

All six LFM3 models — including LFM3-1.7B-Base for teams running their own post-training — are available as open weights on Hugging Face. They run in Hugging Face `transformers` and are compatible with additional inference backends documented on each model card. Liquid AI is releasing 10 Colab notebooks covering supervised fine-tuning, DPO, GRPO, and continued pretraining across all model variants. Example tasks in the notebooks include JSON extraction from documents, new-language learning from raw text, and mapping spoken smart-home commands to function calls.

Every model in the family can be fine-tuned on a single GPU — the base model's 1.7B parameter count and efficient architecture keep memory requirements within reach of developer-grade hardware without cloud-scale infrastructure.

A note on benchmarks: all performance figures cited in this article are self-reported by Liquid AI and have not yet been independently reproduced by third-party researchers. WebArena-Verified, UTMOSv2, LIBERO, and RoboCasa are established benchmarks, but independent evaluation of LFM3 specifically had not been published at time of writing.

## Frequently Asked Questions

### What is the tri-hybrid architecture and why does it matter for edge AI?

LFM3's tri-hybrid design arranges its 24 layers into three types: 13 gated short convolutions (no key-value cache, fastest on CPU), 5 sliding-window attention layers (bounded 256-token cache), and 6 global attention layers (full cache, longest range). This three-regime structure cuts total key-value cache to roughly 27% of a standard transformer of the same depth, which is what allows a 1.7B-parameter model to fit in approximately 1 GB at 4-bit precision and run on phones and edge hardware without a cloud connection.

### Can I run LFM3 on my phone or laptop right now?

Yes. LFM3-1.7B fits in approximately 1 GB at 4-bit precision, which is within the memory budget of mid-range smartphones and most modern laptops. Liquid AI's hardware-in-the-loop architecture search specifically optimized the model across phones, Raspberry Pi 5, laptops, and NPUs including the Qualcomm Hexagon. All models are available as open weights on Hugging Face and can run through inference backends compatible with the `transformers` library or llama.cpp.

### How does LFM3 achieve a 12x improvement on agentic tasks over LFM2?

Two factors combine. First, the tri-hybrid architecture reduces key-value cache overhead and extends the effective context window to 256K tokens at lower memory cost — giving the model more context to work with when navigating multi-step tasks like browser automation. Second, the final 3.5 trillion tokens of training were a dedicated mid-training stage on agentic traces, tool calls, and long-context sequences, giving the model direct experience with the reasoning patterns that WebArena-style tasks require. Neither factor alone accounts for the full gap; the architecture makes the long context affordable, and the training data teaches the model how to use it.

### Are the benchmark results independent or self-reported?

All benchmark figures in this article — WebArena-Verified, UTMOSv2, LIBERO, RoboCasa, ImajevBench, and the 62-benchmark vision average — are self-reported by Liquid AI. The benchmarks themselves are established community standards, but Liquid AI ran the evaluations internally. Independent third-party reproduction of LFM3 results had not been published at the time this article was written. As with any self-reported model release, readers and developers should plan to run their own evaluations on task-relevant workloads before committing to a production deployment.
