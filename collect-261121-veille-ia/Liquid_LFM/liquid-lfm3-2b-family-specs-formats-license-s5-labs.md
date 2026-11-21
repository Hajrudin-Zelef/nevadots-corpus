---
id: collect-261121-veille-ia/liquid-lfm/liquid-lfm3-2b-family-specs-formats-license-s5-labs
title: "Liquid LFM3 2B Family: Specs, Formats, License | S5 Labs"
domain: liquid-lfm
role: reference
task: article
actors: ["Liquid AI"]
dates: []
keywords: ["text", "card", "model", "liquid", "lfm3", "lfm3-1", "models", "audio"]
source: https://s5labs.io/resources/insights/liquid-lfm3-2b-open-weights-local/
source_anchor: ""
source_lines: [1, 97]
section: "Liquid_LFM"
---
Liquid AI published **LFM3 on October 27**: six models in what it calls “the 2B parameter range”, all trained from one LFM3-1.7B-Base checkpoint. There is a text model, a vision model, an audio model, a combined omni model and two decision models. The weights sit in seven Hugging Face repos; the base and post-trained models are also on Liquid’s Playground and API. Liquid, LFM3 blog · Hugging Face collection

S5 Labs has not run any of these models. Every benchmark, speed and memory figure below is Liquid’s, measured on its own builds, and we found no independent tests.

## Seven repos, six models

| Model | Parameters | Context | In / out | Reasoning | 
|---|---|---|---|---|
| LFM3-1.7B-Base | 1.73B | 256K | text / text | For fine-tuning only | 
| LFM3-1.7B | 1.73B | 256K | text / text | On by default | 
| LFM3-VL-2B | 1.92B | 128K | text, image, video / text | Thinking mode | 
| LFM3-Audio-2B | 2.00B | 32K | text, 16 kHz audio / text, 24 kHz speech | Toggle in card example | 
| LFM3-Omni-2B | 2.17B | 32K | text, image, video, audio / text, speech | Not supported | 
| d1-2B | 1.92B | 128K | text, image, video / typed answers | Not a chat model | 
| d1-Omni-2B | 2.17B | 128K | text, image, video, audio / typed answers | Not a chat model | 

Sources: LFM3-1.7B, LFM3-1.7B-Base, LFM3-VL-2B, LFM3-Audio-2B, LFM3-Omni-2B, d1-2B, d1-Omni-2B model cards; parameter counts from the safetensors metadata.

The text model has 24 layers: 13 gated short-convolution blocks, 5 sliding-window attention layers and 6 global attention layers. It covers 16 languages. Liquid says the design came out of a search over 14 device and backend combinations, a Raspberry Pi 5 among them; no Pi benchmark appears anywhere in the release. Pre-training ran 22T tokens, the last 3.5T a curated phase that stretched context to 256K. Reasoning is on by default, and the card warns that turning it off “can be inconsistent, especially for reasoning-heavy tasks”. LFM3-1.7B card · Liquid, LFM3 blog

VL-2B adds a 164M vision encoder and a 25M projector, and is the first LFM vision model to take video, sampled at 2 fps for 4 to 768 frames by default. Audio-2B transcribes, translates and speaks; its English-only streaming mode (LiveASR) works in chunks of about 0.96 seconds, with a claimed time to first token as low as 50 ms. Its speech list has 19 locales, which the card counts as 18 languages; seven of them (Danish, Greek, Finnish, Hebrew, Dutch, Swedish, Turkish) are not text languages. Omni-2B carries the same list. LFM3-VL-2B card · LFM3-Audio-2B card · LFM3-Omni-2B card

The two d1 models are built on VL-2B and Omni-2B. They return typed answers with probabilities and generate no tokens, the same shape as the d1-3B in our decision-models comparison. d1-Omni’s speech list is shorter than Omni’s: the 16 text languages minus Hindi, Thai, Vietnamese and Indonesian. d1-2B card · d1-Omni-2B card

## What Liquid says each model is not for

The text and audio cards carry the same sentence: “Use retrieval or search for knowledge-heavy tasks. It is not recommended for agentic coding or complex multi-step reasoning due to its small size.” The VL card: “It is not recommended for coding, multi-turn conversations, or complex reasoning.” The Omni card has no such line and says the model “is designed to run efficiently on phones, laptops and other edge hardware.” d1-2B’s answers “get less reliable past about 64K tokens of state”, and its estimates are “less so on free quantities”. d1-Omni’s card says “Long spoken passages lose much of their accuracy, most of all outside Latin-script languages” and tells you to use d1-2B when there is no audio. LFM3-1.7B card · LFM3-VL-2B card · d1-2B card · d1-Omni-2B card

The coding caveat sits oddly beside the blog’s SWE-bench Verified (OpenCode) score of 43.40, above Qwen3.5-4B at 41.20 and below Qwen3.5-9B at 51.20; the card’s wording is the guidance Liquid will stand behind. Liquid, LFM3 blog

## Liquid’s benchmarks, including the rows it lost

Liquid ran the text comparisons on vLLM in BF16 at full context. Its unweighted average is 49.37 for LFM3-1.7B against 47.24 for Qwen3.5-9B and 39.27 for Qwen3.5-4B. The wins are agentic: WebArena-Verified 38.42 against the 9B’s 28.94, AutomationBench 41.29 against 15.10. The losses are in the same table: Terminal-Bench 2.1 at 12.92 trails Qwen3.5-4B’s 13.48, MCP-Atlas 32.57 trails 41.37, and GPQA Diamond 63.37 and tau3 68.01 both trail MiniCPM5-2B. On AA-Omniscience, accuracy of 7.07 with a non-hallucination rate of 92.93 means the model mostly declines to answer. LFM3-1.7B card

VL-2B averages 71.4 over 62 benchmarks with thinking, 62.1 without. That is 7.4 points above Qwen3.5 2B at 64.0 and below Qwen3.5 4B and GPT-5.6 Luna, both at 73.4. The card says “at least 8 points”; its own table gives 7.4. Math falls from 68.8 to 41.0 without thinking. LFM3-VL-2B card

| Audio measure (Liquid’s runs) | LFM3-Audio-2B | Best in Liquid’s tables | 
|---|---|---|
| English ASR WER | 6.23 | Kimi-Audio-7B 5.3 (blog), Voxtral Mini 3B 5.51 | 
| Multilingual ASR WER, 9-language FLEURS mean | 6.03 | LFM3-Audio-2B | 
| VoiceBench | 73.13 | MiMo-Audio-7B 77.3, Kimi-Audio-7B 76.9 (blog) | 
| MMAR | 50.60 | MiMo-Audio-7B 62.3 (blog) | 
| English TTS WER | 1.4 | LFM3-Audio-2B | 

Sources: LFM3-Audio-2B card, Liquid, LFM3 blog. The card’s tables leave out MiMo-Audio-7B and Kimi-Audio-7B; the blog’s include them, which is why the audio model leads rows on the card that it loses in the blog. No per-language WER is published.

On the Decision Index public suite v0.3, d1-2B scores 51.1, d1-Omni-2B 49.0 and the older d1-3B 49.0. The 2B leads on average and on most rows, but d1-3B still wins Fast Decisions (59.3 against 58.8) and Typed Decisions (65.0 against 61.3). None of this is comparable with the 48.57 on index version 0.2.1 we quoted for d1-3B in our earlier comparison. d1-Omni holds up on spoken questions (44.7 heard against 45.2 written) and loses badly on spoken short states (22.8 against 42.0). d1-2B card · d1-Omni-2B card

## Weight size is not memory

Liquid’s “about 1 GB at 4-bit” describes the weights. The GGUF files bear that out: QAD-Q4_0 is 1.05 GB and Q4_K_M 1.10 GB for LFM3-1.7B, Q8_0 is 1.85 GB and BF16 3.47 GB (GB as 10^9 bytes). The VL, Audio and Omni language-model files are the same sizes; the extra weight is the mmproj file, 0.38 GB (VL), 0.57 GB (Audio) or 0.95 GB (Omni) in BF16. The QAD build is “trained to recover the accuracy lost to Q4_0 quantization” and ships for the text and VL models; the Audio and Omni Q4_0 files are plain. LFM3-1.7B-GGUF · LFM3-Omni-2B-GGUF

The KV cache is small because only the 6 global attention layers keep a cache that grows with context. From the config (4 KV heads, head size 128, f16) that is 12,288 bytes per token: 0.40 GB at 32K, 1.61 GB at 128K (Liquid’s “1.6 GB”) and 3.22 GB at 256K. A Q4_K_M text model at 128K therefore needs about 2.7 GB for weights plus cache, and about 4.3 GB at 256K, before compute buffers and OS overhead. That arithmetic is ours. Liquid’s KV comparison figures, 4.3 GB for Qwen3.5-4B and 5.6 GB for MiniCPM5-2B, are its own and we did not check them. LFM3-1.7B card · Liquid, LFM3 blog

Liquid’s device claims come from llama.cpp at Q4_0 and 32K context on an AMD Ryzen AI Max+ 395, an Apple M5 Max and an iPhone 17 Pro: decoding 1.8 to 2.8 times faster than the next model, prefill at least 2.5x faster, least memory. The baselines ran on release b11744 while LFM3 ran on “a llama.cpp build with LFM3 support”, and the absolute numbers exist only as chart images. Liquid, LFM3 blog · LFM3-1.7B card

## Runtimes: what works where today

llama.cpp v0.8.0 (October 28) is the stable release that carries all of it: LFM3-1.7B and VL-2B “verified against the reference HF implementation”, Audio and Omni with streaming audio through a `/v1/realtime` WebSocket and a `POST /tts` endpoint, and both d1 models. The b11960 build named on Liquid’s cards is the same commit. llama.cpp v0.8.0

Ollama v0.40.12 (October 27 UTC) added LFM3, and the library now has `lfm3`, `lfm3-vl`, `lfm3-audio` and `lfm3-omni` tags. The release note says “lfm3 and d1 2b are supported on Apple hardware in this release. Support for Linux/Windows coming soon.” Liquid’s GGUF cards likewise list Ollama “for macOS”. Ollama v0.40.13 (October 28 UTC) then added Linux and Windows: its note says LFM3, including text-to-speech in `lfm3-audio` and `lfm3-omni`, “now run on Linux and Windows as well as Apple silicon”. It does not mention d1, and Liquid’s GGUF cards still say “for macOS”. Two tag traps: `d1:latest` is still the 3B model and `d1-omni:latest` is still the 600M, so pull `d1:2b` and `d1-omni:2b` by name. Ollama’s Audio and Omni tags show 125K context where the cards say 32K; we do not know why. Ollama v0.40.12 · Ollama v0.40.13 · Ollama, d1

LM Studio needs llama.cpp engine 2.65.1 or newer and takes image input only for VL and Omni; Llama.app on macOS and Windows takes image and video for VL, and image, audio and video for Omni. The text card lists BF16 on vLLM, SGLang and Transformers, with FP8 and NVFP4 on SGLang. For VL, vLLM support “is in progress” and SGLang is in review; the Omni card lists SGLang for text output only. Transformers needs `trust_remote_code` and at least version 5.19 for d1, 5.20.2 for Audio and Omni, 5.21.0 for VL. MLX repos exist for every post-trained model, though d1-Omni-2B has only 8-bit and fp16. Liquid’s docs library still marks several of these formats “coming soon”; trust the Hugging Face listing. LFM3-1.7B-GGUF · LFM3-Omni-2B-GGUF · LFM3-VL-2B card · Liquid docs, model library

## One license file, read against the pricing page

The LICENSE file is byte-identical across six of the seven repos and differs only in whitespace in the VL repo; every card declares `lfm1.0`. It is the LFM Open License v1.0 we went through for LFM2.5-24B-A2B, so briefly: the Threshold is “annual revenue of 10 million United States dollars ($10,000,000) or more”, counted over the “Legal Entity”, which includes every company under common control. Commercial use by an entity over it “is not licensed under this Agreement”, and the license “will terminate automatically and immediately” on any breach. The exemption in 5(c) covers a 501(c)(3)-type non-profit’s non-commercial or research use, nothing wider. LFM3-1.7B LICENSE · Liquid docs, model license

Liquid’s pricing page says “Research, education, and non-profit use: always free, with no revenue limit”, while its license guide says for-profit organizations using the models commercially remain subject to the threshold. The license text is narrower than the marketing line, and the license is what you agree to. We did not check the LICENSE bytes in the GGUF, MLX, ONNX, FP8 and NVFP4 variant repos. Liquid pricing · Liquid docs, model license

## API price, and where the models actually are

Liquid’s API lists LFM3-1.7B at $0.08 per million input tokens, $0.02 cached and $0.20 output, with $2 of credit for a new organization. The “d1” row at $0.02 input is a hosted model that Liquid’s library does not identify as d1-2B, so do not read it as the price of the open checkpoint. No API price is published for VL, Audio, Omni or either d1-2B model. The blog names OpenRouter and Vercel as day-one platforms; neither public catalog listed any LFM3 model when we checked on October 28. Liquid pricing · Liquid, LFM3 blog

## What to test

**Your context, not Liquid’s.** Its speed and memory claims are at 32K. Load the Q4_K_M text model at 128K and 256K and record peak memory against our 2.7 GB and 4.3 GB arithmetic.

**Reasoning off.** If you need short answers, count how often `enable_thinking=False` is honored on your prompts.

**The d1 swap.** Run d1-3B and d1-2B on the same labelled set; two of Liquid’s own rows go the 3B’s way.

**Audio in your languages.** The only multilingual number is a 9-language mean, LiveASR is English-only, and d1-Omni covers fewer speech languages than Omni.

**Revenue, including affiliates.** Add up the whole controlled group before calling the license free.

**Linux servers.** Ollama’s v0.40.13 note says LFM3 now runs on Linux and Windows, but Liquid’s cards still say macOS and the note skips d1. Confirm on your own hardware; llama.cpp v0.8.0 carries no platform note.

## Sources

Liquid announcements and docs: Introducing LFM3 · Pricing · Model license guide · Model library

Model cards: LFM3 collection · LFM3-1.7B · LFM3-1.7B-Base · LFM3-VL-2B · LFM3-Audio-2B · LFM3-Omni-2B · d1-2B · d1-Omni-2B · LFM3-1.7B-GGUF · LFM3-Omni-2B-GGUF · LFM3-1.7B LICENSE

Runtimes: llama.cpp v0.8.0 · Ollama v0.40.12 · Ollama v0.40.13 · Ollama, lfm3 · Ollama, d1

Related S5 Labs coverage: LFM2.5-24B-A2B and the LFM Open License · Open decision models: Clef, d1, Jev · Voice agent APIs and the self-hosted stack
