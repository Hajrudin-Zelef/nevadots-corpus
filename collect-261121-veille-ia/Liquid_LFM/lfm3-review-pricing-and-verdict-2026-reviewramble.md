---
id: collect-261121-veille-ia/liquid-lfm/lfm3-review-pricing-and-verdict-2026-reviewramble
title: "LFM3 review: pricing and verdict (2026) | ReviewRamble"
domain: liquid-lfm
role: reference
task: article
actors: ["Liquid AI"]
dates: []
keywords: ["liquid", "model", "text", "lfm3", "models", "against", "speech", "decision"]
source: https://www.reviewramble.com/models/lfm3
source_anchor: ""
source_lines: [1, 75]
section: "Liquid_LFM"
---
## What is LFM3?

LFM3 is Liquid AI's third generation of small models, released on 27 October 2026. It has six models of about 2 billion parameters, all branched from the same LFM3-1.7B-Base checkpoint: LFM3-1.7B for text, LFM3-VL-2B for images and video, LFM3-Audio-2B for speech in and out, LFM3-Omni-2B, which sees, listens and talks in one model, and two decision models, d1-2B and d1-Omni-2B. The base itself is also published. Every model uses the same 24-layer backbone (13 gated short convolutions, 5 sliding-window attention layers and 6 global attention layers) pre-trained on 22 trillion tokens.

The text model has 1.73 billion parameters, a 256K-token context and reasoning switched on by default, and was post-trained inside agent harnesses such as Hermes Agent, Pi and OpenClaw. LFM3-VL-2B (1.92B) adds a new vision encoder, reads video at 2 frames a second and has a 128K context. LFM3-Audio-2B (2B) and LFM3-Omni-2B (2.17B) answer in text, 24 kHz speech or both, with a 32K context; Omni does not reason.

The two decision models do not write text at all. d1-2B, built on LFM3-VL-2B, and d1-Omni-2B, built on LFM3-Omni-2B and adding audio, return typed answers with probabilities in one forward pass, the kind of model this site files under its decision category. All the weights are under the LFM Open License v1.0, which bars commercial use by organisations with $10 million or more in annual revenue.

## Is LFM3 worth it?

LFM3 is the most complete small-model family Liquid has shipped, and on its own tests the text model punches well above 2B. Liquid's card has LFM3-1.7B at 45.60 on LiveClaw against 35.77 for Qwen3.5-9B, 41.29 on AutomationBench against 15.10, and 71.39 on BFCL V4 against 62.83, for an unweighted average of 49.37 across its whole table against Qwen3.5-9B's 47.24 and MiniCPM5-2B's 39.19. Liquid ran every model itself in vLLM, and VentureBeat notes that the benchmarks are Liquid's reported results with no independent validation; Artificial Analysis had not scored it when we checked.

The ceiling sits where size usually bites. Qwen3.5-9B leads SWE-bench Verified at 51.20 to 43.40, GPQA Diamond at 80.40 to 63.37 and RULER at 128K tokens at 87.11 to 65.56, and Liquid's own card advises against agentic coding and complex multi-step reasoning. One knowledge row repays a closer look: on AA-Omniscience the model is right only 7.07% of the time but avoids hallucinating 92.93% of the time, against 13.64% for Qwen3.5-4B. It mostly declines rather than invents, which makes retrieval close to mandatory.

The decision models are the less obvious half of the launch. d1-2B takes the same choice, score and yes/no questions as Jev and adds extraction of text, numbers and dates, boxes around objects and estimates with an interval. Liquid reports 77.9 on JevBench v1.4.1 and 5.7 ms per decision on an H100, ahead of its own older d1-3B, though it compares d1-2B only with other d1 models. The hosted d1 decision model costs $0.02 per million input tokens, and VentureBeat cautions that the hosted name may not be the same model as the download.

The weights are small: the 4-bit text GGUF is 1.10 GB and BF16 is 3.47 GB, and Liquid puts the KV cache for a full 128K context at 1.6 GB against 4.3 GB for Qwen3.5-4B. The licence is the same LFM Open License v1.0 as LFM2.5, with commercial rights only below $10 million in annual revenue, while Gemma 4 and Ministral 3 ship under Apache 2.0. Liquid has not published API prices on its own pages, so the $0.08 and $0.20 per million that VentureBeat read from the signed-in console are the only figures available.

## Where it earns its place

- A mobile developer at a 12-person app studio ### Ship an offline voice assistant that hears a request and answers aloudLFM3-Omni-2B takes speech, images and video and replies in text or 24 kHz speech, replacing separate speech recognition, language and speech synthesis models. The 4-bit GGUF is 1.10 GB plus a 0.54 GB Q8 vision and audio projector; the 32K context limits long sessions, and the studio needs a separate deal once revenue reaches $10 million.
- A platform engineer routing tickets for a support-software company ### Replace an LLM classification call with a fast local decisiond1-2B answers several named questions about one ticket in a single pass and returns a probability for each option, at 5.7 ms on an H100 and 1,132 states a second batched, by Liquid's figures. The hosted d1 model costs $0.02 per million input tokens with output free, so 1 billion input tokens a month is about $20; calibrate thresholds on your own tickets, as Liquid advises.
- An engineering manager at a retailer with $200 million in revenue ### Run a browser and tool agent on staff laptops without sending data outLFM3-1.7B was trained in Hermes Agent, Pi and OpenClaw and fits a laptop at about 1 GB, but above $10 million in revenue the open licence does not cover commercial use. Budget for Liquid's enterprise licence, which is priced by deployment scale through sales, or use the hosted API at the reported $0.08 input and $0.20 output per million tokens.

## Before you commit

- Setup
- The text model runs in Transformers with trust_remote_code, vLLM, SGLang, Ollama, LM Studio, MLX and ONNX Runtime; llama.cpp needs an LFM3-compatible build. The d1 models need Transformers 5.19 or later with their own model code, and llama.cpp supports only their choice, score and yes/no questions.
- Integrations
- Tool calls come out as XML, and the chat template supports deferred tool loading for harnesses with a tool search. Liquid documents Hermes Agent, OpenClaw and Pi setups; the models are on the Liquid API and Playground, and LFM3 was not on OpenRouter when we checked.
- Data and privacy
- Self-hosting keeps data local. The LFM Open License v1.0 allows commercial use only below $10 million in annual revenue, with a carve-out for non-profit research; Liquid's no-training promise on its pricing page is stated for custom deployments, and API retention terms are not published there.
- Cost at scale
- At the rates VentureBeat reported, 1 billion input and 100 million output tokens a month cost about $100 on LFM3-1.7B and $150 on LFM3-VL-2B, with audio tokens extra at $2 in and $10 out per million on Audio and Omni. Self-hosting needs 1.05 to 1.10 GB at 4-bit, 1.85 GB at Q8_0 or 3.47 GB in BF16; Liquid reports 3.6 billion tokens a day on one B300.
- Limits
- Hosted LFM3 models list a 128,000-token context, half the open text model's 256K, and usage limits are shared across an organisation's API keys. Turning reasoning off is unreliable on hard tasks, Omni cannot reason, and d1-2B grows less reliable past about 64K tokens of state.

## Whatâs good

- 45.60 on LiveClaw vs 35.77 for Qwen3.5-9B (self-reported)
- 1.10 GB at Q4_K_M; 1.6 GB of KV cache at 128K (Liquid)
- Text, vision, speech, omni and decision models on one backbone
- d1-2B returns typed answers in 5.7 ms on an H100 (Liquid)

## Whatâs not

- No commercial licence at $10M+ annual revenue
- Trails Qwen3.5-9B on SWE-bench Verified, GPQA and long context
- API prices not on Liquid's own pricing page
- No independent benchmark or leaderboard score yet

## Best for

Developers and companies under $10 million in revenue building on-device agents, voice features or fast classifiers that must run on a phone, a laptop or one GPU.

## LFM3 alternatives

## LFM3: common questions

- Is LFM3 worth it?
- Six capable 2B models that fit in about a gigabyte each, from agents to speech to typed decisions, under a licence that still stops at $10 million in revenue. We rate it 4.0 out of 5.
- How much does LFM3 cost?
- Free weights under the LFM Open License (commercial use only below $10M annual revenue). Liquid API, as reported by VentureBeat from the signed-in console: LFM3-1.7B $0.08 input / $0.20 output per 1M tokens; VL $0.10 / $0.50; Audio and Omni $0.10 / $1.00 plus audio tokens. Hosted d1 decisions $0.02 per 1M input, output free.
- What are the downsides of LFM3?
- No commercial licence at $10M+ annual revenue; trails Qwen3.5-9B on SWE-bench Verified, GPQA and long context; aPI prices not on Liquid's own pricing page; no independent benchmark or leaderboard score yet.
- Who is LFM3 best for?
- Developers and companies under $10 million in revenue building on-device agents, voice features or fast classifiers that must run on a phone, a laptop or one GPU.
- How much does LFM3 cost for a team or in production?
- At the rates VentureBeat reported, 1 billion input and 100 million output tokens a month cost about $100 on LFM3-1.7B and $150 on LFM3-VL-2B, with audio tokens extra at $2 in and $10 out per million on Audio and Omni. Self-hosting needs 1.05 to 1.10 GB at 4-bit, 1.85 GB at Q8_0 or 3.47 GB in BF16; Liquid reports 3.6 billion tokens a day on one B300.
- Is LFM3 safe for company data?
- Self-hosting keeps data local. The LFM Open License v1.0 allows commercial use only below $10 million in annual revenue, with a carve-out for non-profit research; Liquid's no-training promise on its pricing page is stated for custom deployments, and API retention terms are not published there.
- What are the best LFM3 alternatives?
- The closest alternatives we have reviewed are MiniCPM5-2B, Gemma 4, Ministral 3 (3B, 8B and 14B), LFM2.5-24B-A2B and Perplexity Decider (pplx-decider).

Answers checked Oct 28, 2026.
