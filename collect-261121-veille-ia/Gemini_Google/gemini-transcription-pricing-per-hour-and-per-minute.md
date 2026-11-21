---
id: collect-261121-veille-ia/gemini-google/gemini-transcription-pricing-per-hour-and-per-minute
title: "Gemini transcription pricing per hour and per minute"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-10-06"]
keywords: ["gemini", "audio", "tokens", "hour", "transcribe", "live", "google", "minute"]
source: https://costperoutput.com/gemini-transcription-pricing
source_anchor: ""
source_lines: [1, 140]
section: "Gemini_Google"
---
# Gemini transcription pricing per hour and per minute

Gemini API's transcription costs $0.306 an hour for recorded audio and $0.536 an hour for live audio, in part an estimate, as some models bill tokens.

## What the numbers say

- For recorded audio at 100 hours a month, Gemini API's Gemini 3.5 Transcribe costs $0.306 an hour, 70% above the median of the 11 vendors ($0.18): 10th, after Deepgram at $0.258 and ahead of Amazon Transcribe at $0.36.
- For live audio at 100 hours a month, Gemini API's Gemini 3.5 Transcribe Live costs $0.536 an hour, the median of the 9 vendors ($0.536): 5th, after Deepgram at $0.462 and ahead of Amazon Transcribe at $0.60.
- Live audio costs 75% more than recorded audio on Gemini API's cheapest models; across the 9 vendors that price both, the median premium is 75%.

## Prices by model

| Model | Audio | Per minute | Per hour | 
|---|---|---|---|
| Gemini 3.5 Transcribe Estimate: Billed in tokens: $2.00 per million audio input tokens and $12.00 per million output tokens. Google's own basis (25 audio tokens a second, 175 output tokens a minute) gives $0.0051 a minute; Google rounds it to about $0.005. | batch | $0.0051 | $0.306 | 
| Gemini 3.5 Transcribe Live Estimate: Billed in tokens: $3.50 per million audio input tokens and $21.00 per million output tokens. On Google's basis that is $0.0089 a minute; Google rounds it to about $0.009. | streaming | $0.0089 | $0.536 | 

## What 10, 100, 1,000 and 10,000 hours a month cost

| Model | 10 hours | 100 hours | 1,000 hours | 10,000 hours | 
|---|---|---|---|---|
| Gemini 3.5 Transcribe batch | $3.06 $0.306/h | $30.60 $0.306/h | $306 $0.306/h | $3,060 $0.306/h | 
| Gemini 3.5 Transcribe Live streaming | $5.36 $0.536/h | $53.55 $0.536/h | $536 $0.535/h | $5,355 $0.535/h | 

Regular prices. One-off credits and promotions aren't counted.

## Free tier and credits

- The free tier charges nothing for input or output, but Google may use free-tier content to improve its products, which it doesn't on the paid tier.

Every vendor's free tier, in hours of audio: free speech-to-text APIs.

## Billing

- Billed in tokens: Google counts 25 audio tokens per second of input audio, plus the text tokens returned.

## Add-ons

| Feature | Audio | Price | 
|---|---|---|
| Speaker diarization | batch | Included Part of Gemini 3.5 Transcribe, no separate price. | 

## Volume discounts and commitments

- Enterprise deployments can get volume-based discounts through Google's sales team.

## Quick answers

## ### How is Gemini API priced?


Gemini API charges $0.306 an hour for recorded audio ($0.0051 a minute) with Gemini 3.5 Transcribe (an estimate, as it bills tokens), and $0.536 an hour for live audio ($0.0089 a minute) with Gemini 3.5 Transcribe Live (an estimate, as it bills tokens). The free tier charges nothing for input or output, but Google may use free-tier content to improve its products, which it doesn't on the paid tier. Billed in tokens: Google counts 25 audio tokens per second of input audio, plus the text tokens returned.

## ### How much does Gemini transcription cost per hour?


$0.306 an hour for recorded audio, and $0.536 an hour for live audio, at regular pay-as-you-go prices.

## ### What do 100 hours of audio a month cost on Gemini API?


$30.60 for recorded audio on Gemini 3.5 Transcribe (an estimate, as it bills tokens), or $53.55 for live audio on Gemini 3.5 Transcribe Live (an estimate, as it bills tokens).

## ### Does Gemini API have a free tier?


The free tier charges nothing for input or output, but Google may use free-tier content to improve its products, which it doesn't on the paid tier.

## ### Does Gemini API bill per second?


Billed in tokens: Google counts 25 audio tokens per second of input audio, plus the text tokens returned.

## ### Is anything cheaper than Gemini API for recorded audio?


Yes: at 100 hours a month, 9 of the 10 other vendors we price cost less: Groq Whisper Large V3 Turbo at $0.04 an hour, Speechmatics Melia 1 at $0.12 an hour, AssemblyAI Universal-2 at $0.15 an hour and 6 more.

## ### Is anything cheaper than Gemini API for live audio?


Yes: at 100 hours a month, 4 of the 8 other vendors we price cost less: AssemblyAI Universal-Streaming at $0.15 an hour, Speechmatics Melia 1 at $0.20 an hour, ElevenLabs Scribe v2 Realtime at $0.214 an hour and 1 more.

## Gemini API against the market: recorded audio

| 100 hours of recorded (batch) audio a month, each vendor's cheapest option: 5 of 11 vendors, ranked in the full field |  |  |  |  | 
|---|---|---|---|---|
| # | Vendor and option | Per month | Per hour | Per minute | 
|---|---|---|---|---|
| 1 | Groq Whisper Large V3 Turbo | $4 | $0.04 | $0.0007 | 
| 8 | Rev AI Reverb (English) | $20 | $0.20 | $0.0033 | 
| 9 | Deepgram Nova-3 Monolingual | $25.80 | $0.258 | $0.0043 | 
| 10 | Gemini API Gemini 3.5 Transcribe | $30.60 | $0.306 | $0.0051 | 
| 11 | Amazon Transcribe Standard | $36 | $0.36 | $0.006 | 

## Gemini API against the market: live audio

| 100 hours of live (streaming) audio a month, each vendor's cheapest option: 6 of 9 vendors, ranked in the full field |  |  |  |  | 
|---|---|---|---|---|
| # | Vendor and option | Per month | Per hour | Per minute | 
|---|---|---|---|---|
| 1 | AssemblyAI Universal-Streaming | $15 | $0.15 | $0.0025 | 
| 3 | ElevenLabs Scribe v2 Realtime | $21.45 | $0.214 | $0.0036 | 
| 4 | Deepgram Nova-3 Monolingual | $46.20 | $0.462 | $0.0077 | 
| 5 | Gemini API Gemini 3.5 Transcribe Live | $53.55 | $0.536 | $0.0089 | 
| 6 | Amazon Transcribe Standard | $60 | $0.60 | $0.01 | 
| 7 | Google Cloud V1 with data logging | $95.04 | $0.95 | $0.0158 | 

## Compare and calculate

- Speech-to-text cost calculatorPrice your own monthly hours
- What it costs to transcribe an hour of audioA single recording
- Cheapest speech-to-text API
- Gemini TTS pricing per minuteGenerating speech instead

## ## Sources

1 page read on 6 October 2026, with 4 passages quoted word for word

- ai.google.dev/gemini-api/docs/pricing (plain HTTP)

### The quoted text

Gemini 3.5 Transcribe Live gemini-3.5-transcribe-live Try it in Google AI Studio Our low-latency, real-time speech-to-text model for bidirectional streaming audio transcription over WebSockets. Standard Feature Free Tier Paid Tier, per 1M tokens in USD Input price Free of charge $3.50 or $0.005/min (audio) Output price (including thinking tokens) Free of charge $21.00 or $0.004/min (text) Grounding with Google Search Not Supported Used to improve our products Yes No Estimated pricing is based on 25 audio tokens per second for input and 175 text tokens per minute for output, for an effective blended rate of ~$0.009 per min for Live Transcribe.

ai.google.dev/gemini-api/docs/pricing

Standard Feature Free Tier Paid Tier, per 1M tokens in USD Input price Free of charge $2.00 or $0.003/min (audio) Output price (including thinking tokens) Free of charge $12.00 or $0.002/min (text) Grounding with Google Search Not Supported Used to improve our products Yes No Estimated pricing is based on 25 audio tokens per second for input and 175 text tokens per minute for output, for an effective blended rate of ~$0.005 per min for Transcribe.

ai.google.dev/gemini-api/docs/pricing

Gemini 3.5 Transcribe gemini-3.5-transcribe Try it in Google AI Studio Our speech-to-text model with automatic language detection, speaker diarization, word-level timestamps, and custom vocabulary biasing.

ai.google.dev/gemini-api/docs/pricing

Volume-based discounts (based on usage)

ai.google.dev/gemini-api/docs/pricing

Prices on this page were checked against the official pricing pages on .
