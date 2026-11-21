---
id: collect-261121-veille-ia/nvidia-hardware/how-to-use-nvidia-nemotron-3-5-transcribe-for-free-the-1-open-speech-to-text-model
title: "How to Use NVIDIA Nemotron 3.5 Transcribe for Free (the #1 Open Speech-to-Text Model)"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: ["2026-10-28"]
keywords: ["nvidia", "model", "free", "space", "open", "transcribe", "nemotron", "only"]
source: https://dev.to/hassann/how-to-use-nvidia-nemotron-35-transcribe-for-free-the-1-open-speech-to-text-model-5fij
source_anchor: ""
source_lines: [1, 181]
section: "NVIDIA_Hardware"
---
NVIDIA has shipped the most accurate open-weight speech-to-text model on the Hugging Face Open ASR Leaderboard, and you can use it for free today. Nemotron 3.5 Transcribe (full name `NVIDIA-Nemotron-3.5-Transcribe-30B-A4B`) landed on October 28, 2026 under the OpenMDW-1.1 license, which allows commercial use. That means three $0 routes: NVIDIA's own demo Space, the free weights on your own GPU, and community Spaces that wrap the same checkpoint.

This guide covers each free route, what it can and can't do, and the cheapest paid fallback when free stops being enough. Every fact comes from the official model card, the NVIDIA launch post on Hugging Face and the Open ASR Leaderboard, all read on October 29, 2026.

If you only need the hosted API, skip to our Nemotron 3.5 Transcribe API guide.

Yes, in two ways. The weights are free to download and the license permits commercial and non-commercial use, so self-hosting costs you only hardware. NVIDIA also runs a free demo Space on Hugging Face where you can try it in the browser.

What isn't free: the hosted APIs. DeepInfra charges $0.002 per minute of audio and OpenRouter lists it at $0.000033 per second. Neither shows a free variant. And as of October 29, 2026, the model doesn't appear on build.nvidia.com, where many NVIDIA models get a free trial API. A search there for "transcribe" returned one unrelated Spanish ASR model.

Nemotron 3.5 Transcribe is a speech-augmented language model. A 600M-parameter ASR encoder and a speaker encoder feed a hybrid Mamba-2 and Transformer mixture-of-experts language model. It has 34B parameters in total, but only about 4.3B are active per token, which is why it runs on a single GPU.

What it does in one checkpoint:

- 
**Transcription in 40 languages** , with 13 rated "leading" (English, Spanish, French, German, Hindi, Korean, Portuguese and others)
- 
**Recordings up to one hour** in a single request (65,536-token context)
- 
**Speaker attribution** with up to eight labeled speakers
- 
**Word-level timestamps** through a CTC alignment head
- 
**Speech translation** to and from English across 31 languages
- 
**Context prompting** : pass names and jargon so it spells them right
- 
**Audio question answering** (English only)

On the Open ASR Leaderboard, updated October 28, 2026, it scores a 5.33% average WER. That puts it 9th of 61 models. The eight above it (Otter, Zoom Scribe v2 Pro, ElevenLabs Scribe v2, Azure Speech and others) are all closed, paid APIs. Among models you can download, nothing ranks higher. OpenAI's `whisper-large-v3` sits 41st at 7.16%.

NVIDIA's own model card reports 4.71% mean WER on the nine public sets. The leaderboard's 5.33% adds two private sets, so both numbers are honest; they measure different mixes.

One limit to know before you start: it's an offline model. It transcribes recorded files, not a live microphone stream.

| Route | Cost | Sign-in | Limits | Best for | 
|---|---|---|---|---|
| NVIDIA demo Space ( `nvidia/nemotron-transcribe` ) | $0 | No | 60-second preview on the sample tabs; shared queue | A quick quality check | 
| Community Spaces | $0 | Usually no | Varies per Space; can sleep or vanish | Trying other UIs | 
| Self-hosted weights (BF16, FP8, NVFP4) | $0 license, your GPU | Hugging Face account to download | One H100 80 GB for BF16; smaller cards need FP8 or NVFP4 | Unlimited private transcription | 
| DeepInfra API | $0.002 per minute | Account and API key | Paid usage | Production without GPUs | 
| OpenRouter | $0.000033 per second | Account and credits | Routed to DeepInfra only | One key for many models | 
| build.nvidia.com | Not listed yet | n/a | n/a | Watch this space | 

Only the first three rows cost nothing.

The fastest way to hear what the model can do is NVIDIA's official Space at huggingface.co/spaces/nvidia/nemotron-transcribe.

1. Open the Space in your browser. No account is needed to load it.
2. Pick a tab. On October 29, 2026 the Space had three: **Karaoke** (word-timed lyrics),**Podcast transcript** (speaker-attributed text) and**Movie subtitles** (timed captions).
3. Choose a sample or paste a link, as the tab allows. The Karaoke tab notes that its preview covers the first 60 seconds.
4. Run it and read the output. The Karaoke view highlights each word as it's sung, which shows off the word timestamps.

Treat the Space as a demo, not a free API. Public Spaces share GPUs, queue requests, and can change without notice. Don't build a product on one.

This is the real free tier: unlimited transcription on hardware you already have, with nothing leaving your machine. The license (OpenMDW-1.1) allows commercial use, so you can ship it inside a product.

The catch is hardware. NVIDIA's reference setup is one H100 80 GB in BF16. The compatibility table on the model card lists:

- 
**BF16:** H100, H200, A100 80 GB, B200, GB200, DGX Spark
- 
**FP8:** H100, H200, L40S 48 GB
- 
**NVFP4 only:** RTX 5090 (32 GB) and RTX PRO 6000 Blackwell

So a consumer RTX 5090 works, with the NVFP4 checkpoint. Older 24 GB cards like the RTX 4090 aren't on the list.

The quickest test needs only Transformers installed from source:


```
pip install git+https://github.com/huggingface/transformers
```
```
from transformers import AutoModelForMultimodalLM, AutoProcessor
model_id = "nvidia/NVIDIA-Nemotron-3.5-Transcribe-30B-A4B"
processor = AutoProcessor.from_pretrained(model_id)
model = AutoModelForMultimodalLM.from_pretrained(model_id, device_map="auto")
inputs = processor.apply_transcription_request("meeting.wav").to(model.device)
outputs = model.generate(**inputs, max_new_tokens=1024)
print(processor.decode(outputs[:, inputs.input_ids.shape[1]:], skip_special_tokens=True))
```
That's fine for a handful of files. For batches, long recordings and an OpenAI-compatible endpoint, serve it with vLLM 0.32.0 and the NeMo Speech plugin. Our step-by-step guide to running Nemotron 3.5 Transcribe locally has the exact install and `vllm serve` command from the model card.

No local GPU? Renting one by the hour is the cheapest "almost free" route for big backlogs. NVIDIA says one RTX PRO 6000 Blackwell can process 100 hours of audio in under three minutes with the NVFP4 build, though that figure uses serving optimizations that aren't open source yet.

The model page lists several community Spaces built on the same checkpoint, including `hugging-apps/nemotron-3-5-transcribe` and `mrfakename/nemotron-3.5-transcribe`. They're useful when the NVIDIA Space queue is long or you want a plain upload box. They're run by individuals, so expect sleeps, quotas and the occasional dead link.

Nemotron 3.5 Transcribe takes plain-language instructions. These come from the model card and the DeepInfra listing. Keep the system prompt empty for transcription and use temperature 0.

**Clean, readable text:**


```
Audio transcription. Language: as spoken. Wording: omit fillers and accidental repetitions; keep the remaining spoken wording unchanged. Formatting: punctuation and capitalization. Output: transcript only.
```
**Who said what:**


```
Transcribe verbatim, including fillers. Write numbers as spoken, without punctuation or capitalization. Start every speaker turn with <spk:0>, <spk:1>, etc. Keep one stable tag per speaker. Output only the speaker-tagged transcript.
```
**Product names and jargon:**


```
Transcribe with punctuation and capitalization. Context terms: Apidog, OpenAPI, Kubernetes, gRPC.
```
**Translation:**


```
Please translate the given speech to English.
```
Context terms matter more than you'd expect. On NVIDIA's ContextASR benchmark, WER drops from 3.33% with no context to 1.71% with fine-grained context.

If you don't own a supported GPU and need more than a demo, the hosted API is cheap. At DeepInfra's $0.002 per minute, a one-hour recording costs $0.12, and 1,000 hours costs $120. OpenRouter's $0.000033 per second works out to about $0.119 per hour, routed to the same provider.

A minimal DeepInfra request, taken from its API page:


```
curl -X POST \
  -H "Authorization: bearer $DEEPINFRA_API_KEY" \
  -F audio=@meeting.mp3 \
  'https://api.deepinfra.com/v1/inference/nvidia/NVIDIA-Nemotron-3.5-Transcribe-30B-A4B'
```
The response includes `text`, `segments` with start and end times, `words`, and the detected `language`. Our API guide walks through keys, response fields and costs in detail.

Whether you self-host or use DeepInfra, you want a check that fails loudly when a transcription comes back empty or in the wrong language. Hand that job to your coding agent and Apidog CLI instead of eyeballing JSON.


```
npm install -g apidog-cli@latest
apidog login --with-token <YOUR_TOKEN>
npx -y skills add https://apidog.com   # pick Claude Code, Codex, Cursor or OpenCode
```
Then give your agent one [REDACTED PROMPT]

In Apidog project , create a test case for POST {{baseUrl}}/v1/inference/nvidia/NVIDIA-Nemotron-3.5-Transcribe-30B-A4B

that uploads samples/hello.mp3 as the "audio" form field, asserts status 200, a non-empty "text",

"language" equal to "en", and at least one item in "words", then run it against the

deepinfra environment and summarize any failures.


```
The agent reads the schema, validates its payload, writes the case and runs it:
```

bash

apidog cli-schema get test-case-create

apidog cli-schema validate test-case-create --file case.json

apidog test-case create --project  --file case.json

apidog run --test-case  --project  -e  -r cli,json

```
Swap the environment to point at your local server when you move from the API to self-hosting. The same `apidog run` with `-r junit` drops into CI; see [running Apidog CLI in a CI/CD pipeline](https://apidog.com/blog/apidog-cli-ci-cd-pipeline?utm_source=dev.to&utm_medium=wanda&utm_content=n8n-post-automation) and [Apidog CLI in Claude Code](https://apidog.com/blog/apidog-cli-in-claude-code?utm_source=dev.to&utm_medium=wanda&utm_content=n8n-post-automation) for the setup.
## Limits to know before you rely on it
- **Offline only.** It doesn't stream. NVIDIA points to a separate Nemotron 3.5 ASR Streaming model for live audio.
- **Audio Q&A is English-only.** Transcription covers 40 languages, but audio understanding and spoken instructions were evaluated only in English.
- **Speaker labels aren't identities.** `<spk:0>` means "the first voice," not a named person.
- **Long outputs need room.** A dense one-hour conversation can produce about 20,000 output tokens, so set `max_new_tokens` accordingly.
- **It can hallucinate.** The card warns it may "generate a plausible transcript" for unclear audio. Review anything legal or medical.
## FAQ
**Is Nemotron 3.5 Transcribe free for commercial use?** Yes. The OpenMDW-1.1 license permits commercial and non-commercial use of the weights. Hosted APIs charge for compute.
**Is there a free Nemotron 3.5 Transcribe API key?** Not that we found on October 29, 2026. DeepInfra and OpenRouter are paid, and it isn't on [build.nvidia.com](https://build.nvidia.com) yet. Compare that with [NVIDIA's free API for Nemotron Nano 12B v2 VL](https://apidog.com/blog/nemotron-nano-12b-v2-vl-free-nvidia-api?utm_source=dev.to&utm_medium=wanda&utm_content=n8n-post-automation), which did get a free trial endpoint.
**Can I run it on a Mac?** Not from the official list. Supported runtimes target NVIDIA GPUs on Linux or Windows (WSL2). GGML support is listed for RTX 5090 and RTX PRO 6000 only.
**How does it compare to Whisper?** On the October 28 leaderboard, it averages 5.33% WER versus 7.16% for `whisper-large-v3`. See our [Whisper API guide](https://apidog.com/blog/whisper-api?utm_source=dev.to&utm_medium=wanda&utm_content=n8n-post-automation) for OpenAI's hosted option and our [best open-source speech-to-text models](https://apidog.com/blog/best-open-source-speech-to-text-models?utm_source=dev.to&utm_medium=wanda&utm_content=n8n-post-automation) ranking for the full field.
**Does it support MP3?** Yes. It accepts WAV, FLAC, MP3, OGG or anything that decodes to 16 kHz mono PCM. Use [FFmpeg to convert other formats](https://apidog.com/blog/convert-audio-ffmpeg?utm_source=dev.to&utm_medium=wanda&utm_content=n8n-post-automation).
## The bottom line
Nemotron 3.5 Transcribe is free where it counts: the weights, the license and NVIDIA's demo. Try the Space for a quality check, self-host on an H100, L40S or RTX 5090 for unlimited private use, and fall back to DeepInfra at $0.12 per hour when you don't have the hardware. Whichever route you pick, let your agent run [Apidog](https://apidog.com?utm_source=dev.to&utm_medium=wanda&utm_content=n8n-post-automation) checks on the endpoint so a bad transcript never reaches production.
```
## Top comments (0)
