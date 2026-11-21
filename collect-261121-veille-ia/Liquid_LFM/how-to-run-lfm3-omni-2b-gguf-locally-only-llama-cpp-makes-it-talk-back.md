---
id: collect-261121-veille-ia/liquid-lfm/how-to-run-lfm3-omni-2b-gguf-locally-only-llama-cpp-makes-it-talk-back
title: "How to Run LFM3-Omni-2B GGUF Locally (Only llama.cpp Makes It Talk Back)"
domain: liquid-lfm
role: reference
task: article
actors: ["Liquid AI"]
dates: []
keywords: ["text", "lfm3-omni-2b", "omni", "audio", "speech", "llama", "video", "output"]
source: https://dev.to/alanwest/how-to-run-lfm3-omni-2b-gguf-locally-only-llamacpp-makes-it-talk-back-1fm4
source_anchor: ""
source_lines: [1, 124]
section: "Liquid_LFM"
---
LFM3-Omni-2B is Liquid AI's new 2B model that takes text, images, video and audio in and answers in text or 24 kHz speech. The Q4_K_M GGUF is 1.1 GB, plus a 292 MB mmproj file, and llama.cpp v0.8.0 runs all of it, voice output included. Ollama and LM Studio load it too, but neither of them makes it speak. If you just want it running:


```
llama serve -hf LiquidAI/LFM3-Omni-2B-GGUF:Q4_K_M --reasoning off --temp 0 --port 8080
```
When I covered d1-Omni-2B earlier this week, my main complaint was that it listens to audio but can only answer with a typed probability. LFM3-Omni-2B is the model it was built on, and it still has the speech output. The GGUF repo picked up 69,967 downloads in the few days since launch, so I'm clearly not the only one curious.

This post covers which quant to pick, the llama.cpp commands for ASR, image, video and TTS, the realtime voice endpoint, which apps can make it speak, and where it loses to LFM3-VL-2B and Gemma 4. I haven't run it myself yet. Everything here comes from Liquid's cards, the llama.cpp release notes and the PR that added realtime voice.

It came out on October 27, 2026, as part of the LFM3 family: six roughly 2B models, all built on LFM3-1.7B-Base. The Omni model combines four parts:

- the LFM3-1.7B backbone
- an LFM3 vision encoder (native resolution, each 2×2 patch block becomes one token)
- a causal audio encoder that takes 16 kHz input
- a "depthformer" plus the Liquid Audio Codec for speech output at 24 kHz

That adds up to 2.17B parameters. Input can be text, images, video (with or without the soundtrack) and audio. Output can be text, speech, or both interleaved. Text works in 16 languages. Speech covers 19 languages and locales, including British and American English as separate voices.

The context window is 32K tokens. That's the spec I'd worry about, since the two sibling models below get 128K.

All three are from the same family and were built for different jobs.

|  | LFM3-Omni-2B | LFM3-VL-2B | d1-Omni-2B | 
|---|---|---|---|
| Params | 2.17B | 1.92B | 2.17B | 
| Input | text, image, video, audio | text, image, video | text/JSON state, image, video, audio | 
| Output | text + 24 kHz speech | text | typed answers with probabilities | 
| Context | 32K | 128K | 128K | 
| Reasoning | no ( `enable_thinking` unsupported) | yes, `<think>` block | single forward pass | 
| License | LFM Open v1.0 | LFM Open v1.0 | LFM Open v1.0 | 

Sources: Omni card, VL card, d1-Omni card.

d1-Omni-2B is a decision model built on top of LFM3-Omni-2B, and its card says plainly that there are "no generated tokens or speech". It's useful for classifying a camera feed, but you can't hold a conversation with it.

The comparison with LFM3-VL is less flattering. Both cards publish the same four vision benchmarks:

| Benchmark | LFM3-Omni-2B | LFM3-VL-2B | 
|---|---|---|
| MMStar | 55.67 | 74.4 | 
| OCRBench | 806 / 1000 | 86.3 | 
| DocVQA | 92.47 | 96.7 (val) | 
| RealWorldQA | 66.54 | 79.3 | 

The cards don't say whether the two models were evaluated the same way, and only VL reasons before answering, so treat these as rough numbers. The gap is still big enough that for screenshots, PDFs and charts I'd stick with LFM3-VL-2B and its 128K context. Pick Omni when the input or output is a voice.

Liquid compares Omni with Qwen2.5-Omni 3B, Gemma 4 E2B and E4B, and Phi-4 MM, all bigger in total parameters. These are Liquid's own measurements from the base card:

| Benchmark | LFM3-Omni 2B | Qwen2.5-Omni 3B | Gemma 4 E2B | Gemma 4 E4B | Phi-4 MM | 
|---|---|---|---|---|---|
| Daily-Omni | 70.09 | 58.81 | 50.63 | 54.14 | 48.87 | 
| English ASR WER ↓ | 6.36 | 6.30 | 8.26 | 7.82 | 5.14 | 
| English TTS UTMOSv2 ↑ | 3.80 | 3.42 | — | — | — | 
| VoiceBench ↑ | 64.73 | 66.37 | 63.28 | 72.90 | 64.97 | 
| Tool calling (internal) ↑ | 52.20 | 44.30 | 55.60 | 72.40 | — | 

The audio-plus-video results are strong for its size, and ASR is close to Qwen2.5-Omni 3B. On the vision benchmarks it roughly ties Qwen2.5-Omni 3B too, so the gap to LFM3-VL is a specialist-vs-generalist thing. VoiceBench and tool calling are where it falls short: Gemma 4 E4B beats it by about 8 points on VoiceBench and 20 on Liquid's internal tool-calling test, and even the smaller Gemma 4 E2B edges it on tool calling. Those two scores are the ones that matter for a voice assistant. Most voice tasks are some version of "set a timer", and that is a tool call.

Liquid doesn't publish RAM figures per quant, so file size is the best estimate available. You need the main model plus an `mmproj` file. The multimodal parts live in that mmproj file, and the repo's commit history shows the speech codec decoder is in there as well ("Quantize the codec decoder in mmproj"). Both lists below come from the file listing:

| Quant | Model | mmproj (same quant) | Total on disk | 
|---|---|---|---|
| Q4_0 | 1.05 GB | 271 MB | ~1.3 GB | 
| Q4_K_M | 1.1 GB | 292 MB | ~1.4 GB | 
| Q8_0 | 1.85 GB | 507 MB | ~2.4 GB | 
| BF16 / F16 | 3.47 GB | 950 MB | ~4.4 GB | 

Add the KV cache and audio buffers on top of that. Liquid's blog says the hybrid LFM3 backbone needs only 1.6 GB of KV cache at 128K context for the text model, and Omni stops at 32K. On paper, an 8 GB laptop handles Q8_0 without trouble, though that's my estimate and I haven't measured it.

Someone on the repo has already asked whether the Q4 file uses QAD (quantization-aware distillation), as the text model's Q4_0 does. Nobody has answered yet. Until Liquid says so, I'd assume it doesn't and go with Q8_0 if you can fit it.

Support arrived in llama.cpp b11960 (v0.8.0), released October 28. That release also added a `POST /tts` endpoint and a `/v1/realtime` endpoint to the server (release notes). The commands below are copied from the model card. They use the `llama serve` / `llama cli` front-end, plus the separate `llama-tts` binary for speech. If your build is older than b11960, update first. Older builds don't support the model at all.

Start the web UI and the OpenAI-compatible API. `-hf` also downloads the projector:


```
llama serve -hf LiquidAI/LFM3-Omni-2B-GGUF:Q4_K_M --reasoning off --temp 0 --port 8080
```
Keep `--reasoning off`. It's a non-reasoning model, and the card says `enable_thinking=True` isn't supported.

Speech-to-text:


```
llama cli -hf LiquidAI/LFM3-Omni-2B-GGUF:Q4_K_M --audio speech.wav -sys "Perform ASR in English." -p "<__media__>" --temp 0
```
Image input:


```
llama cli -hf LiquidAI/LFM3-Omni-2B-GGUF:Q4_K_M --image photo.png -p "Describe this image." --temp 0
```
Video input works with `--video file.mp4`. It needs `ffmpeg` in your `PATH`, and the base card says the model samples at 2 fps by default, between 4 and 384 frames. Long clips use up the 32K window fast.

Text-to-speech, the feature d1-Omni didn't have:


```
llama-tts -m LFM3-Omni-2B-Q4_K_M.gguf -mm mmproj-LFM3-Omni-2B-Q8_0.gguf -p "Hello world" -o out.wav
```
`--tts-voice` accepts `female` (the default) or `male`, and `--tts-lang` defaults to `en`. Liquid recommends audio temperature 0.4 and repetition penalty 1.2. The card says both values are already stored in the GGUF.

PR #31269 was merged on October 28. It adds voice chat with streaming audio in and out, plus live transcription, to the server's `/v1/realtime` endpoint. The GGUF card advertises tool calls over the same endpoint. You choose the reply type with `output_modalities`: `["text"]`, `["audio"]`, or `["audio", "text"]` for interleaved voice chat. Leave it unset and the model decides. The PR includes `examples/realtime_client.py` to test with. It was smoke-tested on Mac and CUDA with F32, Q8_0 and Q4_0. The PR doesn't give latency numbers, so I'm not repeating the time-to-first-audio claims going around.

The GGUF card lists a lot of apps, but speech output is only available in a few of them:

- llama.cpp v0.8.0+: image, audio and video input, plus speech output via `llama-tts` ,`/tts` and`/v1/realtime`
- Llama.app (macOS, Windows): image, audio and video input
- Ollama v0.40.12+ (macOS): image and audio input
- LM Studio (llama.cpp engine 2.65.1+): image input only
- SGLang: text output only, according to the base card
- Transformers `>=5.20.2` : everything, in BF16

Only llama.cpp and Transformers document speech output. Everywhere else you get a vision-language model with a microphone, at best.

The license is the LFM Open License v1.0. It's free for commercial use until your company reaches $10M in annual revenue. Past that point you need to contact Liquid for a paid license. The threshold still applies to fine-tunes and other derivatives. Qualifying nonprofits are exempt for non-commercial and research use. That's fine for hobby projects and startups, but a bigger company can't ship it without talking to Liquid's sales team first.

This is the first 2B model I'd test as a fully local voice loop: ASR, a language model and TTS in one roughly 1.4 GB download, served over a WebSocket by a tool I already use. The 32K context, the drop in vision quality compared with LFM3-VL, and a tool-calling score 20 points behind Gemma 4 E4B are what you give up. For a kitchen-counter assistant that reads timers and the weather, I think it's enough. For an agent that has to remember a long conversation and pick the right tool reliably, I doubt it.

Would you use a 2B omni model as your local voice assistant, or is 2B still too small to trust with tool calls? If you've already tested `/v1/realtime`, tell me what latency you're getting.

## Top comments (0)
