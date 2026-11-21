---
id: collect-261121-veille-ia/gemini-google/gemini-3-5-transcribe-api-pricing-context-window-providers
title: "Gemini 3.5 Transcribe API Pricing, Context Window & Providers"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-11-20"]
keywords: ["transcribe", "gemini", "litellm", "google", "model", "tokens", "pricing", "context"]
source: https://models.litellm.ai/models/gemini-3.5-transcribe
source_anchor: ""
source_lines: [1, 93]
section: "Gemini_Google"
---
# Gemini 3.5 Transcribe API Pricing

`gemini/gemini-3.5-transcribe`
- Price
- $2.00 per 1M tokens
- Context
- 96K tokens
- Max output
- 32K tokens

As of November 20, 2026, Gemini 3.5 Transcribe costs $2.00 per 1M tokens on Google AI Studio. It has a 98,304-token context window and returns up to 32,768 tokens per response.

## Gemini 3.5 Transcribe pricing by provider

2 listings across 2 providers. Token prices are in US dollars per 1M tokens. The highlighted row is the price quoted above.

| Gemini 3.5 Transcribe prices by provider in US dollars, token prices per 1M tokens, as of November 20, 2026. Source: LiteLLM model price file. |  |  |  |  |  | 
|---|---|---|---|---|---|
| Provider | Model name in LiteLLM | Per minute | Input | Output | Context | 
|---|---|---|---|---|---|
| Google AI Studio | `gemini/gemini-3.5-transcribe` | – | $2.00 | $12.00 | 96K | 
| Google Vertex AI | `vertex_ai/gemini-3.5-transcribe` | $0.003 | – | $12.00 | – | 

## Other Gemini 3.5 Transcribe prices on Google AI Studio

| Audio input | $2.00 per 1M tokens | 

## Gemini 3.5 Transcribe features

Input: text, audio. Output: text.

- Function calling
- Audio input

## Use Gemini 3.5 Transcribe with LiteLLM

Call Gemini 3.5 Transcribe through the LiteLLM Python SDK, or put it behind the LiteLLM proxy and send OpenAI-format requests to `/v1/audio/transcriptions`. LiteLLM tracks the cost of every request at these prices.

```
from litellm import transcription
response = transcription(
    model="gemini/gemini-3.5-transcribe",
    file=open("audio.mp3", "rb"),
)
```
```
model_list:
  - model_name: gemini-3.5-transcribe
    litellm_params:
      model: gemini/gemini-3.5-transcribe
      api_key: os.environ/GEMINI_API_KEY
```
curl http://0.0.0.0:4000/v1/audio/transcriptions \
  -H "Authorization: Bearer $LITELLM_API_KEY" \
  -F model="gemini-3.5-transcribe" \
  -F file=@audio.mp3

## Gemini 3.5 Transcribe FAQ

### How much does Gemini 3.5 Transcribe cost?

Gemini 3.5 Transcribe costs $2.00 per 1M tokens on Google AI Studio.

### What is the context window of Gemini 3.5 Transcribe?

Gemini 3.5 Transcribe has a 98,304-token context window and returns up to 32,768 tokens per response.

### Which providers offer Gemini 3.5 Transcribe?

Gemini 3.5 Transcribe is available from 2 providers through LiteLLM: Google AI Studio and Google Vertex AI.

### How do I call Gemini 3.5 Transcribe with an OpenAI-compatible API?

Use the LiteLLM Python SDK with model="gemini/gemini-3.5-transcribe", or add the model to the LiteLLM proxy's config.yaml and send requests to /v1/audio/transcriptions from any OpenAI client. LiteLLM tracks the cost of every request at the prices on this page.

### What features does Gemini 3.5 Transcribe support?

Gemini 3.5 Transcribe supports audio input. It does not support function calling.

### When did LiteLLM add Gemini 3.5 Transcribe?

Gemini 3.5 Transcribe was added to LiteLLM's model price file on August 27, 2026.

## Sources

Prices come from LiteLLM's open-source model_prices_and_context_window.json, which LiteLLM uses to calculate the cost of every request. Maintainers update it from each provider's published pricing, and this page is usually rebuilt within 20 minutes of every change. Provider pricing pages:

- ai.google.dev/gemini-api/docs/pricing
- cloud.google.com/gemini-enterprise-agent-platform/generative-ai/pricing

LiteLLM docs: Google AI Studio, Google Vertex AI.

Provider pages: Google AI Studio, Google Vertex AI.
