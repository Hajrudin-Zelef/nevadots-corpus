---
id: collect-261121-veille-ia/gpt-openai/gpt-6-2-sol-openai-s-everyday-model-price-specs-access
title: "GPT 6.2 Sol: OpenAI's Everyday Model, Price, Specs & Access"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: ["2026-11-17"]
keywords: ["astra", "openai", "model", "gpt-6", "input", "output", "tokens", "everyday"]
source: https://beta.synexa.ai/models/gpt-6-2-sol
source_anchor: ""
source_lines: [1, 378]
section: "GPT_OpenAI"
---
1. Synexa
2. Model explainers
3. GPT-6.2 Sol

## What is GPT 6.2 Sol?

GPT 6.2 Sol, officially written **GPT-6.2 Sol**, is a large language model from OpenAI, launched on November 17, 2026. OpenAI calls it "our go-to model for everyday work, from coding to powering agents at scale." GPT 6.2 Sol reads text and images and writes text.

The GPT-6.2 release has two models. GPT-6.2 Astra is OpenAI's frontier model for the hardest projects; GPT 6.2 Sol is the efficient, versatile one for everyday work. OpenAI says both deliver better results with fewer output tokens, so you get more done within the same usage limits.

Sol succeeds GPT-6.1 Sol at the same API price. OpenAI describes the new version as smarter and more efficient, letting you complete more tasks on the same paid plan, and says many of its own employees use it as their default model for technical and non-technical work.

## GPT 6.2 Sol specs: context, output and model ID

Published figures from OpenAI's model page and GPT-6.2 guide, as of November 2026.

- Maker
- OpenAI
- Released
- November 17, 2026
- API model ID
- `gpt-6.2-sol`
- Input
- Text and images
- Output
- Text
- Context window
- 1,050,000 tokens
- Max input
- 922,000 tokens
- Max output
- 128,000 tokens
- Knowledge cutoff
- June 1, 2026
- Reasoning effort
- Start at `medium` ;`none` is rejected
- Fine-tuning
- Not supported

## How much does GPT 6.2 Sol cost?

In the OpenAI API, GPT 6.2 Sol costs $2 per million input tokens and $10 per million output tokens, carried over from GPT-6.1 Sol. That is a quarter of Astra's token price. Standard rates as of November 2026.

Input

- 6.2 Sol
- $2.00
- 6.2 Astra
- $8.00
- 6.1 Sol
- $2.00

Cached input

- 6.2 Sol
- $0.10
- 6.2 Astra
- $0.40
- 6.1 Sol
- $0.10

Cache write, 30 minutes

- 6.2 Sol
- $2.50
- 6.2 Astra
- $10.00
- 6.1 Sol
- $2.50

Output

- 6.2 Sol
- $10.00
- 6.2 Astra
- $40.00
- 6.1 Sol
- $10.00

Batch (in / out)

- 6.2 Sol
- $1.00 / $5.00
- 6.2 Astra
- $4.00 / $20.00
- 6.1 Sol
- $1.00 / $5.00

Fast mode (in / out)

- 6.2 Sol
- $4.00 / $20.00
- 6.2 Astra
- $16.00 / $80.00
- 6.1 Sol
- $4.00 / $20.00

Ultrafast (in / out)

- 6.2 Sol
- $6.00 / $30.00
- 6.2 Astra
- $24.00 / $120.00
- 6.1 Sol
- $12.00 / $60.00

Example: 100K in + 10K out

- 6.2 Sol
- $0.30
- 6.2 Astra
- $1.20
- 6.1 Sol
- $0.30

| Rate, per MTok | 6.2 Sol | 6.2 Astra | 6.1 Sol | 
|---|---|---|---|
| Input | $2.00 | $8.00 | $2.00 | 
| Cached input | $0.10 | $0.40 | $0.10 | 
| Cache write, 30 minutes | $2.50 | $10.00 | $2.50 | 
| Output | $10.00 | $40.00 | $10.00 | 
| Batch (in / out) | $1.00 / $5.00 | $4.00 / $20.00 | $1.00 / $5.00 | 
| Fast mode (in / out) | $4.00 / $20.00 | $16.00 / $80.00 | $4.00 / $20.00 | 
| Ultrafast (in / out) | $6.00 / $30.00 | $24.00 / $120.00 | $12.00 / $60.00 | 
| Example: 100K in + 10K out | $0.30 | $1.20 | $0.30 | 

The example row is our arithmetic from the list prices: 0.1M × $2 plus 0.01M × $10 = $0.30. For requests with more than 272K input tokens, Sol's input and cache rates double and output costs 1.5 times as much. Ultrafast for the GPT-6.2 models is 3 times Standard through February 28, 2027, and 6 times Standard from March 1, 2027.

## GPT-6.2 Sol benchmarks from OpenAI

Scores OpenAI published at launch in its launch post and the GPT-6.2 system card. We have not run them ourselves.

GPQA Diamond (no tools)

- 6.2 Sol
- 96.0%
- 6.2 Astra
- 97.0%
- 6.1 Sol
- not listed

SEC-Bench Pro (vulnerability reproduction)

- 6.2 Sol
- 88.34%
- 6.2 Astra
- 86.75%
- 6.1 Sol
- 78.8%

ExploitBench Internal Port (V8 and Chrome)

- 6.2 Sol
- 50.5%
- 6.2 Astra
- 53.5%
- 6.1 Sol
- 21.5%

AA-3D (human-voted Elo)

- 6.2 Sol
- 1,259
- 6.2 Astra
- 1,318
- 6.1 Sol
- not listed

| Benchmark | 6.2 Sol | 6.2 Astra | 6.1 Sol | 
|---|---|---|---|
| GPQA Diamond (no tools) | 96.0% | 97.0% | not listed | 
| SEC-Bench Pro (vulnerability reproduction) | 88.34% | 86.75% | 78.8% | 
| ExploitBench Internal Port (V8 and Chrome) | 50.5% | 53.5% | 21.5% | 
| AA-3D (human-voted Elo) | 1,259 | 1,318 | not listed | 

Vendor-reported. OpenAI also says GPT 6.2 Sol comes within 0.5 points of Astra on FrontierCode 2.0 at about one-fifth of the cost (both at max), beats GPT-6 Astra on LifeVal at about one-seventh of the cost, and gains 5.3 points over GPT-6.1 Sol on its internal GDPval v2.

## GPT 6.2 Sol vs Gemini 4 Neon and Claude Opus 5.5

Google published this head-to-head in its Gemini 4 Neon model card the day after Sol launched. These are Google's numbers, not OpenAI's.

Artificial Analysis Intelligence Index v5.0.1

- GPT 6.2 Sol
- 57
- Gemini 4 Neon
- 61
- Claude Opus 5.5
- 59

Vals Index v2.2

- GPT 6.2 Sol
- 66.1%
- Gemini 4 Neon
- 70.9%
- Claude Opus 5.5
- 70.1%

Terminal-Bench 4.1 (cheating-adjusted)

- GPT 6.2 Sol
- 61.5%
- Gemini 4 Neon
- 65.2%
- Claude Opus 5.5
- 65.8%

FrontierCode 2.0

- GPT 6.2 Sol
- 43.8%
- Gemini 4 Neon
- 46.2%
- Claude Opus 5.5
- 47.3%

Agents' Last Exam v1.1

- GPT 6.2 Sol
- 73.0%
- Gemini 4 Neon
- 75.8%
- Claude Opus 5.5
- 71.6%

Chartography

- GPT 6.2 Sol
- 75.1%
- Gemini 4 Neon
- 73.5%
- Claude Opus 5.5
- 66.3%

List price per MTok (in / out)

- GPT 6.2 Sol
- $2 / $10
- Gemini 4 Neon
- $3 / $15
- Claude Opus 5.5
- not compared

| Benchmark | GPT 6.2 Sol | Gemini 4 Neon | Claude Opus 5.5 | 
|---|---|---|---|
| Artificial Analysis Intelligence Index v5.0.1 | 57 | 61 | 59 | 
| Vals Index v2.2 | 66.1% | 70.9% | 70.1% | 
| Terminal-Bench 4.1 (cheating-adjusted) | 61.5% | 65.2% | 65.8% | 
| FrontierCode 2.0 | 43.8% | 46.2% | 47.3% | 
| Agents' Last Exam v1.1 | 73.0% | 75.8% | 71.6% | 
| Chartography | 75.1% | 73.5% | 66.3% | 
| List price per MTok (in / out) | $2 / $10 | $3 / $15 | not compared | 

Artificial Analysis, the independent tracker, lists Sol (Max) at 57 with an average cost of $1.01 per index task, against $6.44 for Gemini 4 Neon at xhigh. Our Gemini 4 Hydrogen guide covers Google's cheaper model.

## What changed from GPT-6.1 Sol?

OpenAI kept the price and raised the quality, with gains concentrated in agent work and efficiency.

### Same price, more done

### Near-Astra coding

### Better knowledge work

### Everyday tasks

### Interactive 3D and web

### Stronger cyber, tighter safeguards

## Where can you use GPT 6.2 Sol?

OpenAI released the model on launch day in ChatGPT Work, Codex and the API.

### ChatGPT Work and Codex

### Ultrafast in ChatGPT

### OpenAI API

`gpt-6.2-sol` through the Responses API or Chat Completions, billed per token at the rates above. Tool calling requires Responses, and Ultrafast was available at launch.
## GPT-6.2 Sol API gotchas before you migrate

From OpenAI's GPT-6.2 guide and pricing page: settings that cost money or return errors.

- Start a new integration at `medium` reasoning effort; move`minimal` users to`low`
- Use the Responses API whenever the model needs to call tools
- Put reusable instructions in a `developer` message first in`input` so they cache
- Use discounted background windows, 30% or 40% off, instead of Flex

- Send reasoning effort `none` ; the API rejects it
- Send `temperature` ,`top_p` or logprobs parameters
- Edit earlier turns mid-conversation; the API drops reasoning items
- Cross 272K input tokens by accident: input then costs double

## Can you run GPT 6.2 Sol on Synexa?

No. Synexa does not serve GPT 6.2 Sol, Astra or any other GPT language model, and it offers no chat LLMs; its only text outputs are LLaVA-13B image answers, Whisper speech transcripts and an NSFW image-check label. For the language side, use the OpenAI API, ChatGPT Work or Codex.

Synexa covers the media half of an AI product through one REST API. One key gives you image models such as GPT Image 2 and Nano Banana Pro at $0.10 a request, music, speech, upscaling, and image-to-3D models such as Hunyuan3D 3.1 at $0.16 a request. A common pattern is to have the language model write the prompt, then send it to Synexa to make the asset. Every price is listed in Explore, new accounts get $0.10 of free credit, and the quickstart has Python and Node examples.

- Pay per request, no subscription
- Python and Node clients, plus plain HTTP
- Image, video, audio, upscaling and 3D in one catalog

## Synexa models to pair with a Sol agent

Media a Sol-driven agent can request from Synexa. Flat prices per request, as listed on each model page in November 2026.

| Task | Synexa model | Price per request | 
|---|---|---|
| Posters and slides with readable text | GPT Image 2 | $0.10 | 
| Product photos and ad visuals | Nano Banana Pro | $0.10 | 
| Textured 3D model from one image | Hunyuan3D 3.1 | $0.16 | 
| Draft 3D mesh | TRELLIS | $0.02 | 
| Background music clip | Lyria 2 | $0.10 | 
| Upscale a photo | Real-ESRGAN | $0.0015 | 
| Meeting or video transcript | Whisper v3 | $0.004 | 

Synexa catalog prices, not OpenAI's. Synexa does not run any GPT language model.

## GPT 6.2 Astra vs Sol: which should you pick?

Start with GPT 6.2 Sol. OpenAI's own guidance is to use Astra for the most demanding tasks and Sol for complex work at lower cost, and on FrontierCode 2.0 the gap between them is half a point. Sol's tokens cost a quarter of Astra's ($2 / $10 against $8 / $40), so a 100K-in, 10K-out call is $0.30 instead of $1.20. Move up to Astra for long professional deliverables such as slides, workbooks and legal memos, which OpenAI trained Astra to do better.

Against Google's new Gemini 4 Neon, the trade-off is quality against cost. Google's numbers put Neon ahead on most benchmarks, while GPT 6.2 Sol is cheaper per token and per task. If your product also generates images, voice or 3D, compare media APIs in our Runware alternative and WaveSpeed AI alternative pages, or read our Claude Opus guide and check per-request prices on pricing.

## Related pages

More model explainers and API comparisons.

## Frequently asked questions

## ### What is GPT 6.2 Sol?


GPT 6.2 Sol, written GPT-6.2 Sol by OpenAI, is OpenAI's efficient language model for everyday work, launched November 17, 2026 together with the frontier model GPT-6.2 Astra. It reads text and images, writes text, and has a 1.05M-token context window with up to 128K output tokens. It succeeds GPT-6.1 Sol at the same API price.

## ### How much does GPT 6.2 Sol cost?


In the OpenAI API it costs $2 per million input tokens, $0.10 per million cached input tokens and $10 per million output tokens, as of November 2026. Batch halves that to $1 / $5, Fast mode doubles it, and Ultrafast is three times Standard until February 28, 2027. A call with 100K tokens in and 10K out costs $0.30.

## ### What is the difference between GPT-6.2 Astra and Sol?


Astra is OpenAI's frontier model for the hardest projects; Sol is the efficient model for everyday work. Astra costs $8 / $40 per million tokens against Sol's $2 / $10. OpenAI reports Sol within 0.5 points of Astra on FrontierCode 2.0 at about a fifth of the cost, while OpenAI pitches Astra for slides, spreadsheets, legal work and science.

## ### Can I use GPT-6.2 Sol in ChatGPT?


Yes, in ChatGPT Work and Codex. OpenAI made it available to Plus, Pro, Enterprise and Edu users at launch, in preview on Work and Codex desktop; Enterprise and Edu need an admin to enable it. Regular Chat keeps its existing model choices. Pro 200 and Pro 500 subscribers can also run Sol with Ultrafast.

## ### What is the context window and knowledge cutoff?


OpenAI's model page lists a 1,050,000-token context window, up to 922,000 input tokens and 128,000 output tokens per response, with a knowledge cutoff of June 1, 2026. Long prompts cost more: above 272K input tokens, input and cache rates double and output costs 1.5 times the normal rate for the whole request.

## ### What is Ultrafast mode?


Ultrafast is the fastest service tier in the OpenAI API, offered for GPT-6.2 Astra and Sol plus GPT-6 Astra and GPT-6.1 Sol. You set `service_tier` to `ultrafast`. For the GPT-6.2 models it costs three times the Standard rate through February 28, 2027, then six times Standard from March 1, 2027, and it runs on US data residency only.

## ### Is GPT 6.2 Sol better than Gemini 4 Neon?


On Google's own model card, no: Gemini 4 Neon leads Sol on most listed benchmarks, such as 61 against 57 on the Artificial Analysis Intelligence Index. Sol wins on Chartography and on price, at $2 / $10 against $3 / $15 per million tokens, and Artificial Analysis measured a much lower cost per index task for Sol.

## ### Can I use GPT 6.2 Sol through Synexa?


No. Synexa does not serve GPT 6.2 Sol or any other chat language model. Synexa is a pay-per-request API for generative media: images, video, speech and music, upscaling and image-to-3D. Many teams call a language model for planning and writing, then call Synexa to render the image, clip, voice-over or 3D model. New Synexa accounts get $0.10 of free credit.

## Pair GPT 6.2 Sol with a media API

Use GPT 6.2 Sol for reasoning and writing, then generate the images, video, music and 3D models it plans on Synexa. Pay per request, start with $0.10 free credit.
