---
id: collect-261121-veille-ia/gemini-google/google-cloud-speech-to-text-alternatives-2026-5-apis-compared-on-accuracy-and-price-assemblyai
title: "Google Cloud Speech-to-Text alternatives (2026): 5 APIs compared on accuracy and price | AssemblyAI"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["google", "cloud", "speech-to-text", "transcribe", "audio", "price", "alternatives", "batch"]
source: https://www.assemblyai.com/blog/google-cloud-speech-to-text-alternatives
source_anchor: ""
source_lines: [1, 169]
section: "Gemini_Google"
---
Google sells two different speech-to-text products. There's Cloud Speech-to-Text V2, built on the Chirp models. And there's Gemini 3.5 Transcribe, which Google calls the successor to Chirp 3 and which has been generally available since August 26, 2026. If you built on one of them and are weighing a move, the question is what each alternative does better on your audio, and what it costs.

This guide answers that with numbers you can check. It compares five Google Cloud Speech-to-Text alternatives: AssemblyAI, ElevenLabs Scribe, AWS Transcribe, Microsoft Azure AI Speech and Deepgram. Every price carries a date and a source. Accuracy figures come from the AssemblyAI-STT-3.7 launch post and research post (November 2026), which ran each vendor's production API through the same harness at default settings.

## Google Cloud Speech-to-Text alternatives comparison table

Google Cloud Speech-to-Text alternatives are speech-to-text APIs that transcribe audio without Google Cloud. Teams compare them on accuracy on real audio, speaker labels, streaming support, language coverage and per-hour price.

Here's how the options compare as of November 17, 2026. List prices are pay-as-you-go rates converted to per hour. The WER column is word error rate averaged over 16 pre-recorded code-switching datasets (English mixed with German, Spanish, French, Italian or Portuguese). Lower is better.

| Provider | Pre-recorded list price | Streaming list price | Languages | Code-switching WER (pre-recorded) | 
|---|---|---|---|---|
| Google Cloud Speech-to-Text V2 (Chirp 3) | $0.016/min ($0.96/hr) standard; $0.003/min ($0.18/hr) dynamic batch | $0.016/min ($0.96/hr) | More than 100 locales, most in preview | Not measured | 
| Google Gemini 3.5 Transcribe | $2.00/1M audio input + $12.00/1M text output tokens (about $0.005/min, Google's estimate) | $3.50/1M audio input + $21.00/1M text output tokens (about $0.009/min, Transcribe Live) | 85+ | Not in published results | 
| AssemblyAI (AssemblyAI-STT-3.7 / 3.7 realtime models) | $0.21/hr | $0.30/hr Realtime; $0.45/hr Realtime-Agent | 32, or 99 with Universal-2 | 6.2% | 
| ElevenLabs Scribe v3 | $0.20/hr | $0.22/hr (Scribe v3 Live) | 90+ | 6.8% | 
| AWS Transcribe | $0.006/min ($0.36/hr) | $0.01/min ($0.60/hr) | 100+ locales | 22.9% | 
| Microsoft Azure AI Speech | $0.18/hr batch | $1.00/hr real-time | 100+ locales | 17.4% (Batch Transcription) | 
| Deepgram Nova-3 | $0.0043/min ($0.258/hr) | $0.0048/min promotional, $0.0077/min list | 60+ (Nova models) | 49.4% | 

Prices come from each vendor's own pricing page or official price feed on November 17, 2026. Volume tiers, committed-use plans and promotions other than Deepgram's aren't included. List prices change often, so check each page before you commit.

## What is Google Cloud Speech-to-Text?

Google Cloud Speech-to-Text is Google's managed transcription API on Google Cloud. The current version is V2, and its latest model, Chirp 3, is generally available in the us and eu multi-regions with diarization and automatic language detection. Chirp 3 diarization runs only in batch recognition (`BatchRecognize`). You call V2 from a Google Cloud project, with audio sent inline or read from Cloud Storage.

V2 standard pricing is $0.016/min for the first 500,000 minutes a month and steps down at higher volumes. Dynamic batch, which processes audio at lower urgency, is $0.003/min. Google's medical dictation and conversation models ($0.078/min) and its 60-minute monthly free tier are listed under the V1 API.

The second product is Gemini 3.5 Transcribe, in the Gemini API. It has a pre-recorded model (`gemini-3.5-transcribe`) and a streaming model (`gemini-3.5-transcribe-live`), bills by audio input and text output tokens, and lists 85+ languages. Diarization supports up to 8 speakers in batch, and Google marks attribution for three or more speakers as experimental. Custom vocabulary takes up to 1,000 terms but can't be combined with diarization, and Live sessions are capped at 10 minutes.

## Why look for Google Cloud Speech-to-Text alternatives?

Most teams start looking for one of four reasons, and three of them show up in published numbers.

**Speaker labels.** Gemini 3.5 Transcribe's diarization marks three or more speakers as experimental and can't be combined with custom vocabulary, and Chirp 3 diarization is batch-only. If your calls and meetings have several speakers and named products, compare diarization error rate (DER) and cpWER (speaker-attributed WER) across providers. Neither Google product was in the published diarization table.

**Code-switched audio.** Callers who mix English with Spanish or German in one conversation are a hard case. Gemini 3.5 Transcribe isn't in the published pre-recorded code-switching results. In realtime, Gemini 3.5 Transcribe Live averages 10.16% WER across five English language pairs, against 6.67% for AssemblyAI-STT-3.7-Realtime.

**Entities in live conversations.** A voice agent that mishears an account number fails the call. On a 12,460-call voice-agent benchmark from the AssemblyAI-STT-3.7-Realtime-Agent research post, Gemini 3.5 Transcribe Live has an 8.48% WER and a 19.2% entity error rate. AssemblyAI-STT-3.7-Realtime-Agent is at 4.53% and 13.2%, the lowest word error rate and the lowest entity error rate of the 26 streaming models AssemblyAI ran.

**Billing you can predict.** Gemini 3.5 Transcribe bills by tokens, so Google's per-minute figures are estimates. V2 bills per minute, but the price varies with volume tier and urgency.

Google is close on recorded English. On AppTek's call-center dialogues, Gemini 3.5 Transcribe has a 7.52% WER against 7.44% for AssemblyAI-STT-3.7. If your workload is clean, monolingual English, test the difference directly.

## What to look for in a Google Cloud Speech-to-Text alternative

Spec sheets all read the same. These are the criteria that separate APIs once real audio hits them.

- **Accuracy on your audio type.** Mean WER on clean speech is table stakes. Check the conditions you face: noise, accents, code-switching, and the names and numbers your workflow reads. Our guide to measuring speech-to-text accuracy covers WER, entity error and cpWER.
- **Recorded and live from one provider.** Most products need both eventually: live transcripts during a call and a cleaner pass afterward.
- **Speaker labels that hold up.** Look for cpWER, which scores words and speaker labels together; a "supports diarization" checkbox tells you little. Our roundup of speaker diarization APIs explains the metrics.
- **Language coverage that matches your users** , including whether the model handles a language switch mid-sentence.
- **A price you can predict.** A per-hour list price is easier to budget than token billing you estimate from audio length.
- **What comes after the transcript.** Summaries, PII redaction and entity extraction either come from the same API or from a pipeline you build.

Upload the code-switched calls, noisy clips and multi-speaker meetings that trip up your current setup. Compare transcripts and speaker labels side by side, no code required.

## Top 5 Google Cloud Speech-to-Text alternatives

### 1. AssemblyAI

AssemblyAI released three speech-to-text models on November 18, 2026. AssemblyAI-STT-3.7 handles recorded audio at $0.21/hr. AssemblyAI-STT-3.7-Realtime handles live transcripts with realtime speaker labels at $0.30/hr, and AssemblyAI-STT-3.7-Realtime-Agent handles voice agents at $0.45/hr. All three cover 32 languages with automatic detection and native code-switching, and Universal-2 extends recorded audio to 99 languages when you list both models.

On recorded audio, AssemblyAI-STT-3.7 has a 7.44% WER on AppTek's call-center dialogues, ahead of Google Gemini 3.5 Transcribe, Soniox, Deepgram, Speechmatics and OpenAI, and a 10.7% diarization error rate, the lowest of the pre-recorded APIs measured, ahead of ElevenLabs Scribe v3, Speechmatics, Gladia, AWS Transcribe, Deepgram and Soniox. On code-switched audio, averaged over 16 datasets, it has a 6.2% WER, the lowest of the systems measured, and the lowest error rate on 12 of the 16 datasets. In realtime, AssemblyAI-STT-3.7-Realtime has a 3.23% short-form English WER, ahead of AWS Transcribe Streaming, ElevenLabs Scribe v3 Live, Speechmatics, Soniox and Deepgram, and well ahead of Gemini 3.5 Transcribe Live at 5.04%.

The transcript is only the first step. Speech Understanding adds speaker identification, PII redaction and entity detection on the same API key. The LLM Gateway gives you one OpenAI-compatible endpoint for running LLMs over transcripts. On recorded audio, keyterms prompting takes up to 1,000 words or phrases ($0.05/hr), and a natural-language `prompt` ($0.05/hr) describes the recording.

Why choose AssemblyAI:

- **Accuracy where Google is weakest:** code-switching, voice-agent entities and speaker attribution.
- **One API for both modes:** recorded and live, plus speech understanding, on one key.
- **Published per-hour pricing:** $0.21/hr recorded, $0.30/hr or $0.45/hr live. The free tier covers up to 185 hours of pre-recorded and 333 hours of streaming transcription, with no credit card required.
- **Enterprise controls:** SOC 2 Type 2, and a Business Associate Addendum (BAA) available for healthcare workloads.

### 2. ElevenLabs Scribe

ElevenLabs' current speech-to-text model is Scribe v3, sold alongside Scribe v2. Batch is $0.20/hr and Scribe v3 Live is $0.22/hr on every plan, and it lists 90+ languages.

In the published tables, Scribe v3 has a 12.0% diarization error rate (AssemblyAI-STT-3.7: 10.7%) and a 7.34% AppTek call-center WER. On code-switching it's at 6.8% (AssemblyAI-STT-3.7: 6.2%), and in realtime Scribe v3 Live has a 4.18% short-form English WER (3.23%). Speech understanding beyond the transcript isn't part of the same API, so summaries, redaction and extraction are a separate build.

### 3. AWS Transcribe

AWS Transcribe is Amazon's speech-to-text service, at $0.006/min ($0.36/hr) for batch and $0.01/min ($0.60/hr) for streaming in US East. PII redaction, Call Analytics and Transcribe Medical are priced separately.

In the published tables, AWS Transcribe has a 16.9% diarization error rate and a 22.9% code-switching WER. AWS Transcribe Streaming has a 3.93% short-form English WER and a 9.13% realtime code-switching WER. Moving off Google onto AWS swaps one cloud dependency for another: you need an AWS account, IAM and usually S3. Our Amazon Transcribe alternatives guide covers that side.

### 4. Microsoft Azure AI Speech

Azure AI Speech offers real-time, fast and batch transcription billed per audio hour, plus Custom Speech models trained on your own data. List prices in East US are $1.00 per hour for real-time and $0.18 per hour for batch, and Fast Transcription is on a promotional rate.

In the published tables, Azure Batch Transcription has a 17.4% code-switching WER, and Azure's realtime model 14.60%. Azure wasn't in the diarization or AppTek tables. Integration assumes an Azure subscription and the Speech SDK, so it's mainly a fit for teams already standardized on Microsoft.

### 5. Deepgram

Deepgram sells Nova-3 for general transcription and Flux for streaming conversations. Nova-3 lists at $0.0043/min ($0.258/hr) pre-recorded, and monolingual streaming is on a promotional $0.0048/min (list $0.0077/min).

In the published tables, Nova-3 has an 8.59% AppTek WER, a 17.4% diarization error rate and a 49.4% code-switching WER, and a 7.46% short-form English WER in realtime. For more, see our Deepgram alternatives roundup.

### A note on self-hosting Whisper

OpenAI's Whisper model is open source, so you can run it on your own GPUs with no per-minute fee. You take on serving, scaling and monitoring yourself, and Whisper has no native streaming or speaker diarization. It makes sense when audio can't leave your infrastructure and you have ML ops capacity. OpenAI's hosted whisper-1 API is deprecated and shuts down on February 26, 2027. If on-prem is the requirement, our Speechmatics alternatives guide covers vendors with self-hosted options.

Moving thousands of hours a month off Google Cloud? Talk through volumes, languages, compliance and add-ons with our team and get an estimate for your workload.

## Migration guide: switching from Google Cloud Speech-to-Text

Most of the work in a migration is mapping features and response fields. These four steps apply to any provider.

### Step 1: Map your current features

List the Google features you use: model (chirp_3, telephony), diarization, word timestamps, phrase hints, language detection. Then find the equivalent on the new provider. With AssemblyAI, phrase hints map to keyterms prompting, and speaker labels, PII redaction and entity detection come back in the same response as the transcript.

### Step 2: Update authentication and endpoints

Replace Google service-account credentials with the new provider's API key. For recorded audio, swap your Cloud Storage URI for a public or signed audio URL, or upload the file directly, and set `speech_models` to `["assemblyai-stt-3-7"]`. For streaming, point your WebSocket client at the new endpoint and set `speech_model` to `assemblyai-stt-3-7-realtime` for live transcripts or `assemblyai-stt-3-7-realtime-agent` for voice agents.

### Step 3: Adjust response parsing

Google returns results and alternatives in its own JSON shape. Update your parser to the new provider's fields: utterances with speaker labels, word timestamps in milliseconds, and turn events for streaming. A typed SDK cuts most of this work.

### Step 4: Test and deploy

Run a representative sample of your production audio through both systems and score the output against human references before you cut over. Include the edge cases: long silences, cross-talk, accented speakers and code-switched calls. Our post on how accurate speech-to-text is explains how to read the results. For live audio, check rate limits too: AssemblyAI paid accounts start at 200 new streaming sessions per minute and scale up automatically, with no cap on open sessions, per the streaming rate limits docs.

## How to choose the right Google Cloud Speech-to-Text alternative

Start with your own audio. Pick 25 or more files that represent production, including the hard ones, and run each candidate on the same set. Then match the results to what your product needs:

- **Voice agents and live calls that act on names and numbers:** AssemblyAI-STT-3.7-Realtime-Agent, with a 13.2% entity error rate against 19.2% for Gemini 3.5 Transcribe Live.
- **Multilingual or code-switched audio:** AssemblyAI-STT-3.7 for recordings, AssemblyAI-STT-3.7-Realtime for live audio.
- **Meetings and calls that need speaker labels:** AssemblyAI-STT-3.7, with a 10.7% diarization error rate against 16.2% for Speechmatics Enhanced and 17.4% for Deepgram Nova-3.
- **An AWS-only stack:** AWS Transcribe, with the accuracy trade-offs above.
- **A Microsoft-only stack:** Azure AI Speech.

Then price the whole job. Add the add-ons you need, the engineering time to stitch services together, and the human review time that every transcription error costs downstream. The AssemblyAI pricing page lists every model and add-on per hour, and our speech-to-text API pricing guide compares how vendors bill.

## Final thoughts

Google's lineup now has two GA products, Chirp 3 on V2 and Gemini 3.5 Transcribe, with different pricing, limits and diarization rules. That makes this a good moment to decide what your product needs from speech-to-text for the next few years.

If your audio is clean, monolingual English and already on Google Cloud, Gemini 3.5 Transcribe is close to AssemblyAI-STT-3.7 on call-center WER and staying put can be reasonable. If you handle code-switched callers, multi-speaker conversations, or voice agents that act on what they hear, the published numbers point elsewhere. The fastest way to know is to run your own files.

Get an API key and run AssemblyAI-STT-3.7 and the 3.7 realtime models on your own audio. The free tier covers up to 185 hours of pre-recorded and 333 hours of streaming transcription.

## Frequently Asked Questions (FAQs)

### Which speech-to-text API is most accurate compared to Google?

On AppTek's call-center dialogues, AssemblyAI-STT-3.7 (7.44% WER) and Google's Gemini 3.5 Transcribe (7.52%) are within 0.1 points of each other, and both are ahead of Deepgram Nova-3 (8.59%). The gaps are wider on live audio: AssemblyAI-STT-3.7-Realtime has a 3.23% short-form English WER against 5.04% for Gemini 3.5 Transcribe Live, and AssemblyAI-STT-3.7-Realtime-Agent has a 13.2% voice-agent entity error rate against 19.2%.

### How much do Google Cloud Speech-to-Text alternatives cost per hour?

As of November 17, 2026, pre-recorded list prices run from $0.20/hr (ElevenLabs Scribe) and $0.21/hr (AssemblyAI-STT-3.7) to $0.36/hr (AWS Transcribe). Google Cloud Speech-to-Text V2 standard recognition is $0.96/hr, or $0.18/hr with dynamic batch, and Gemini 3.5 Transcribe is token-billed at about $0.005/min by Google's estimate. Streaming runs from $0.22/hr (Scribe v3 Live) to $1.00/hr (Azure).

### Is there a free alternative to Google Cloud Speech-to-Text?

AssemblyAI's free tier covers up to 185 hours of pre-recorded and 333 hours of streaming transcription with no credit card. AWS Transcribe includes 60 free minutes a month for 12 months, and Azure includes 5 free audio hours a month of real-time transcription. Self-hosting the open-source Whisper model has no per-minute fee, but you pay for GPUs and operations.

### Which Google Speech-to-Text alternatives support real-time streaming?

All five covered here offer streaming: AssemblyAI (AssemblyAI-STT-3.7-Realtime and -Realtime-Agent), ElevenLabs (Scribe v3 Live), AWS Transcribe, Azure AI Speech and Deepgram (Nova-3 and Flux). On short-form English, AssemblyAI-STT-3.7-Realtime has a 3.23% WER, the lowest of the realtime systems measured, ahead of AWS Transcribe Streaming (3.93%) and Scribe v3 Live (4.18%).

### How hard is it to migrate from Google Cloud Speech-to-Text V2?

Most of the work is mapping features. You replace service-account credentials with an API key, swap Cloud Storage URIs for audio URLs or direct uploads, map phrase hints to keyterms, and update your response parser. Plan time to validate accuracy on a sample of your own audio before you cut over.

### Which alternative handles code-switching best?

AssemblyAI-STT-3.7 has a 6.2% WER averaged over 16 code-switching datasets of English with German, Spanish, French, Italian or Portuguese, the lowest of the systems measured, with ElevenLabs Scribe v3 at 6.8% and Azure at 17.4%. In realtime, AssemblyAI-STT-3.7-Realtime averages 6.67% against 10.16% for Gemini 3.5 Transcribe Live. Gemini 3.5 Transcribe isn't in the published pre-recorded code-switching results.
