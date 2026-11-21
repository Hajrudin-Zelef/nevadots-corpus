---
id: collect-261121-veille-ia/gemini-google/gemini-3-5-transcribe-vs-deepgram-vs-whisper-2026-metawhisp
title: "Gemini 3.5 Transcribe vs Deepgram vs Whisper (2026) — MetaWhisp"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-08-26"]
keywords: ["whisper", "gemini", "transcribe", "deepgram", "audio", "model", "google", "benchmark"]
source: https://metawhisp.com/blog/gemini-3-5-transcribe-vs-deepgram-whisper/
source_anchor: ""
source_lines: [1, 198]
section: "Gemini_Google"
---
**Short version:**on the one benchmark that measures all of them the same way, Gemini 3.5 Transcribe lands at

**2.6% word error rate**, Whisper Large v3 at

**4.1%**, and Deepgram Nova-3 at

**5.2%**. Gemini costs about

**$0.005 per minute**, Nova-3 about

**$0.0043**, the Whisper API

**$0.006**. Deepgram is roughly

**6× faster**at chewing through recorded audio. Running Whisper on your own machine costs nothing per minute and never uploads the audio — which is a different axis entirely, and the one most of these comparisons skip.

## What Google actually shipped

On August 26, 2026, Google announced Gemini 3.5 Transcribe — a speech-to-text model in two flavours: a batch model for recorded audio and a Live variant for streaming.

What it claims, from Google's own announcement:

- **2.6% word error rate** non-streaming,**4.0%** streaming, measured by Artificial Analysis
- **0.40 seconds** from end of speech to final text in streaming mode
- **85+ languages** with automatic detection
- **Up to 3 speakers** identified, with timestamps
- Filler-word removal and self-correction handling built in — it drops the "umm" and keeps the corrected version when you restate something
- Custom vocabulary, and function calling to hand work off to other Gemini models
- Roughly **an hour of audio in a single pass**

That last one deserves more attention than it got. The Whisper API caps uploads at 25MB — about 20 minutes — so long recordings have to be split and stitched back together, and every cut point is a place where a word can go missing. An hour in one request removes that whole class of bug.

## The benchmark trap everyone fell into

Within a day of the announcement, half a dozen posts compared Gemini's 2.6% against Whisper's "2.1% on LibriSpeech" or similar, and declared a winner.

Those numbers are not comparable, and it is worth being precise about why.

**LibriSpeech test-clean** is read English audiobook speech, recorded cleanly. It describes a very specific, very easy condition.

**AA-WER v2**, the Artificial Analysis benchmark Google cited, is a different animal: roughly 8 hours of audio blended from agent conversations (50%), European Parliament recordings (25%), and earnings calls (25%), across varied accents and acoustic conditions. Messy, real, interruption-heavy speech.

A model scoring 2.6% on the hard benchmark and a model with a similar score on the easy one are not tied. They were never on the same test.

**The honest framing:**when you put them on the

*same*scale — AA-WER v2 — Whisper Large v3 scores 4.1% and Gemini 3.5 Transcribe scores 2.6%. On difficult real-world audio, Google's model is measurably more accurate than Whisper. I run a Whisper-based product and that is still the number.

## Accuracy, all on one scale

Artificial Analysis runs every model through the identical AA-WER v2 set, which makes it the only apples-to-apples comparison available right now. Lower is better.

| Word error rate on AA-WER v2 (Artificial Analysis, retrieved 2026-08-28) |  |  |  | 
|---|---|---|---|
| Model | WER | Price / 1,000 min | Speed | 
|---|---|---|---|
| ElevenLabs Scribe v2 | 2.2% | $3.67 | — | 
| **Gemini 3.5 Transcribe** | 2.6% | $5.00 | 78× | 
| AssemblyAI Universal-3 Pro | 3.1% | $3.50 | — | 
| AssemblyAI Universal | 3.8% | — | — | 
| OpenAI GPT-4o Transcribe | 4.0% | $6.00 | — | 
| **Whisper Large v3** | 4.1% | — | — | 
| Whisper Large v2 | 4.1% | — | — | 
| **Deepgram Nova-3** | 5.2% | $4.30 | 501× | 

Two things jump out. Gemini bought its accuracy at a speed cost — 78× realtime against Nova-3's 501×. And Deepgram, which sits last on accuracy here, is six times faster and cheaper per minute. If you are transcribing a million minutes of call-centre audio overnight, that trade is not obviously wrong.

## Prices, checked twice

Every figure below came off the vendors' own pricing pages, first on 2026-08-28 and re-checked on 2026-09-01. Two of them moved in those four days, which is the whole argument for checking before you budget rather than trusting any comparison post, this one included.

### Gemini 3.5 Transcribe

Google prices audio in and text out separately:

- **Batch:** $0.003/min audio in + $0.002/min text out —**~$0.005/min blended**
- **Live:** $0.005/min in + $0.004/min out —**~$0.009/min blended**
- Free tier available

The blended rate assumes 25 audio tokens per second in and 175 text tokens per minute out. Talk faster than average and your bill moves.

### Deepgram

Deepgram is the most granular of the three, and diarization is included rather than billed extra:

| Deepgram pay-as-you-go rates per minute (re-checked 2026-09-01) |  |  | 
|---|---|---|
| Model | Pre-recorded | Streaming | 
|---|---|---|
| Nova-3 Monolingual | $0.0043 | $0.0048 | 
| Nova-3 Multilingual | $0.0052 | $0.0058 | 
| Flux English | — | $0.0065 | 
| Flux Multilingual | — | $0.0078 | 
| Whisper Large (hosted by Deepgram) | $0.0048 | — | 

The streaming rates above are flagged on Deepgram's own page as **limited-time promotional pricing**: Nova-3 Monolingual streaming is $0.0048/min today against a $0.0077/min regular rate, and Nova-3 Multilingual $0.0058 against $0.0092. Budget on the regular number if the promo ending would hurt. **$200 in free credit** to start, no expiry. Smart formatting and speaker diarization are bundled in. Redaction adds $0.0020/min, keyterm prompting $0.0013/min, entity detection $0.0017/min. The Growth plan starts at $4K/year for up to 20% off.

### OpenAI

- **gpt-transcribe:** $0.0045/min — the newest full-size model, and cheaper than the 4o line it replaces
- **gpt-4o-transcribe:** $0.006/min
- **gpt-4o-mini-transcribe:** $0.003/min — the cheapest hosted option here
- **gpt-4o-transcribe-diarize:** $0.006/min, diarization included
- **Live transcription:** $0.017/min (`gpt-live-transcribe` ,`gpt-realtime-whisper` ) — notably expensive next to Gemini Live at ~$0.009

**One thing changed between the two times I checked this page.** On 28 August the lineup still carried a plain `whisper-1` row at $0.006/min. On 1 September it is gone: OpenAI sells transcription as `gpt-transcribe` and the 4o pair now, and the word Whisper survives only on the $0.017/min live SKU. If you budgeted against the old Whisper API line, re-read your invoice.

### Whisper on your own machine

**$0.00 per minute.** The weights are open, the model runs on Apple Silicon, and there is no meter. That is the whole pitch, and it is a category difference rather than a discount: at 1,000 hours of audio, Gemini runs about $300 and local Whisper runs the electricity.

## What each one is actually good at

### Gemini 3.5 Transcribe — accuracy on messy audio

Best hosted accuracy short of ElevenLabs, an hour of audio per request, filler-word cleanup and self-correction handling that produce text you can paste without editing. The 0.40s streaming latency is genuinely quick. Three speakers with timestamps covers most interviews and 1:1s, though it will not carry a six-person roundtable.

### Deepgram Nova-3 — throughput and unit cost

501× realtime means an hour of audio in about seven seconds. Sub-300ms streaming latency. Diarization included, no per-speaker cap advertised. It is last on this accuracy table, but the gap between 5.2% and 2.6% matters far less when the output is feeding search or analytics rather than a document a human will read.

### Whisper — the one you can run yourself

4.1% on hard audio, 99 languages, and the only option on this page you can point at a folder and run with the Wi-Fi off. Hosted, it is unremarkable at $0.006/min. Local, it is the only model here that never sends a client's voice to a third party. Whisper large-v3-turbo gives up about 0.3 percentage points of accuracy for roughly 4× the speed, which is the trade most desktop apps take — not this one, which runs large-v3 on the Mac.

## Choosing by what you're actually doing

### Transcripts a person will read

Interviews, podcasts, client calls. Accuracy and formatting win here — Gemini 3.5 Transcribe at ~$0.005/min, or ElevenLabs Scribe v2 if the 0.4 points matter more than the ecosystem.

### Millions of minutes into a pipeline

Support calls feeding analytics or search. Deepgram Nova-3: 501× speed, $0.0043/min, diarization bundled. Nobody reads these transcripts word for word, and 5.2% doesn't break keyword search.

### Audio that must not leave the machine

Therapy sessions, legal calls, anything under NDA, anything where uploading is the thing you are trying to avoid. Local Whisper is the only answer on this page — not because it wins the accuracy table, but because it is the only one that doesn't require trusting a vendor with the recording. See private voice-to-text on Mac.

## Where this leaves on-device Whisper

Being straight about it: Gemini 3.5 Transcribe is more accurate than Whisper on hard audio, by a real margin, on the only shared benchmark available. If your only criterion is "fewest wrong words on a noisy recording," Google's model wins and it costs half a cent a minute.

What that comparison leaves out is that accuracy is one axis of four. The other three — cost at volume, whether the audio leaves your machine, and whether it works with no connection — are exactly where a local model is unbeatable, and no amount of WER improvement changes them. A 2.6% cloud model still uploads the recording. A 4.1% local model still runs on a plane, for free, forever.

MetaWhisp runs Whisper large-v3 on your Mac's GPU, on-device, with zero telemetry — that is the trade it makes by default. If cloud accuracy is what a particular recording needs, the honest advice is to use the cloud model for that recording. Which is why, as of version 1.3.27, you can.

## Using Gemini 3.5 Transcribe from MetaWhisp

MetaWhisp 1.3.27 added Gemini 3.5 Transcribe as an optional engine for meeting recordings, on your own API key. Dictation doesn't use the key and keeps its engine, the local model unless you've set cloud transcription — this is specifically for the case where a recorded call needs the accuracy.

What it does when a key is present:

- Your microphone and the system audio go to Google's `gemini-3.5-transcribe` in verbatim mode, with`diarization_mode: speaker` and word-level timestamps
- Long meetings are sliced under Google's 30-minute diarization ceiling — 25-minute slices, stitched back with each line labelled Me or Them
- No key set? Nothing changes: the meeting is transcribed as before (on-device unless you've set cloud transcription) and labelled Me and Them — see how recording works

**What this changes about speaker labels: nothing.**On the local path MetaWhisp labels your mic Me and everyone else Them — which is certainty about your own lines but no individual identification of remote speakers. With a Gemini key on a meeting, the labels are still Me and Them: MetaWhisp resolves each speaker Google finds to one of the two. What the key changes is the engine and where the audio goes: private by default, sent to Google while a key is set.

The trade is the one this whole article is about, made explicit inside a single app: local costs nothing and never uploads; Gemini costs ~$0.005/min, sends the audio to Google under your own account, and comes back more accurate, still labelled Me and Them.

## Frequently asked questions

### Is Gemini 3.5 Transcribe more accurate than Whisper?

On the AA-WER v2 benchmark, yes — 2.6% against Whisper Large v3's 4.1%. Be careful with comparisons that put Gemini's 2.6% next to a Whisper score from LibriSpeech test-clean: those are different benchmarks, and LibriSpeech is substantially easier audio.

### What does Gemini 3.5 Transcribe cost?

About $0.005 per minute blended for recorded audio ($0.003/min audio in plus $0.002/min text out), and about $0.009 per minute for the Live streaming variant. There is a free tier. Rates from Google's pricing page, re-checked 2026-09-01.

### Is Deepgram cheaper than Gemini?

Slightly — Nova-3 Monolingual is $0.0043/min pre-recorded against Gemini's ~$0.005/min, and Deepgram includes diarization and gives $200 of free credit. Deepgram is also about 6× faster. It scores worse on accuracy: 5.2% versus 2.6%.

### How many speakers can Gemini 3.5 Transcribe identify?

Up to three, with timestamps. Deepgram's diarization does not advertise a speaker cap, and OpenAI ships a dedicated gpt-4o-transcribe-diarize at $0.006/min.

### Can I use Gemini 3.5 Transcribe in MetaWhisp?

Yes, since version 1.3.27 — for meeting recordings, with your own Gemini API key. Dictation doesn't use the key. With or without a key, the transcript is labelled Me and Them; without a key, meetings transcribe as before, on-device unless you've set cloud transcription (Pro or your own key).

### Can I run Gemini 3.5 Transcribe locally?

No. It is API-only — the Live API and Interactions API, plus Google AI Studio and the Gemini Enterprise Agent Platform. Whisper is the model in this comparison whose weights you can download and run yourself.

### Which is best for long recordings?

Gemini handles roughly an hour in a single pass. The Whisper API caps at 25MB (~20 minutes), so longer files need splitting and rejoining. Running Whisper locally has no upload limit at all — file length is bounded by your disk.

## Sources

- Google — Intelligent transcription with Gemini 3.5 Transcribe (2026-08-26)
- Artificial Analysis — Speech-to-Text WER Index (AA-WER v2, retrieved 2026-08-28)
- Deepgram pricing (retrieved 2026-08-28)
- Gemini API pricing (retrieved 2026-08-28)
- OpenAI API pricing (retrieved 2026-08-28)
