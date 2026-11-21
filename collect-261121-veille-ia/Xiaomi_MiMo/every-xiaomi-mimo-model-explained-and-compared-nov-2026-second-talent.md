---
id: collect-261121-veille-ia/xiaomi-mimo/every-xiaomi-mimo-model-explained-and-compared-nov-2026-second-talent
title: "Every Xiaomi MiMo Model Explained and Compared (Nov 2026) - Second Talent"
domain: xiaomi-mimo
role: reference
task: article
actors: ["Xiaomi"]
dates: []
keywords: ["xiaomi", "mimo", "model", "mimo-v2", "tokens", "index", "every", "retired"]
source: https://www.secondtalent.com/resources/xiaomi-mimo-models-explained/
source_anchor: ""
source_lines: [1, 391]
section: "Xiaomi_MiMo"
---
TL;DR: Xiaomi's current MiMo models are MiMo-V2.6-Pro (1.02T parameters, $0.435 in and $0.87 out per million tokens) and MiMo-V2.6-Flash (309B, $0.14 and $0.28), both MIT-licensed with a 1M-token context; V2.5 and V2.5-Pro left the API on October 21, 2026. On the Artificial Analysis Intelligence Index v5.0.1, V2.6-Pro scores 30, behind the open-weights leader Kimi K3.1 Pro at 46, but costs only $0.26 per index task.

In March 2026 an anonymous model called Hunter Alpha appeared on OpenRouter and passed one trillion tokens of usage before Xiaomi said who had built it. Xiaomi then revealed it as an early test build of MiMo-V2-Pro.

Eleven months earlier, the entire MiMo line had been a single 7-billion-parameter reasoning model.

We first published this guide on October 1, 2026. On November 20 we rechecked every price, score and retirement date against Xiaomi's and Artificial Analysis's own pages, and tried V2.6-Pro ourselves in Xiaomi's MiMo Studio.

The number I would watch now is cost per task, not leaderboard rank: that is where MiMo still stands out.

What decides the pick

1Every open MiMo checkpoint ships under plain MIT, so commercial use and fine-tuning need no extra license.

2The Batch API halves the V2.6 price, and UltraSpeed costs ten times the standard rate for up to 20x faster output.

3MiMo Studio, Xiaomi's free chat demo, needs a Xiaomi account and has no web search, so it will not answer current-events questions.

4The smallest new model is a 9B distill on a Qwen base, small enough for a single consumer GPU.

The MiMo Models at a Glance

Xiaomi has published 30 repositories under its XiaomiMiMo Hugging Face account since April 2025, plus three API-only models. The table covers the main checkpoint of each release; base, SFT and quantized variants sit beside them.

Model

Released

Total / active params

Context

Weights

API status

MiMo-V2.6-Pro

Sep 22, 2026

1.02T / 42B

1M

MIT

Current, $0.435 / $0.87

MiMo-V2.6-Flash

Sep 22, 2026

309B / 15B

1M

MIT

Current, $0.14 / $0.28

MiMo-V2.6-Distill-Qwen-9B

Sep 2026

9B dense

Not stated

MIT

Weights only

MiMo-V2.5-Pro

Apr 23, 2026

1.02T / 42B

1M

MIT

Retired Oct 21, 2026

MiMo-V2.5

Apr 23, 2026

310B / 15B

1M

MIT

Retired Oct 21, 2026

MiMo-V2-Pro

Mar 18, 2026

1T+ / 42B

1M

None

Retired Jun 30, 2026

MiMo-V2-Omni

Mar 18, 2026

Not disclosed

256K

None

Retired Jun 30, 2026

MiMo-V2-Flash

Dec 16, 2025

309B / 15B

256K

MIT

Retired Jun 30, 2026

MiMo-7B family

Apr 2025

7B dense

32K

MIT

Weights only

Reading the prices. API prices are Xiaomi's overseas list rates in US dollars per million tokens, input on a cache miss and output, from the MiMo pricing page, rechecked on November 20, 2026 and unchanged since October 1. Release dates follow Xiaomi's model update log.

MiMo-V2.6-Pro: The Flagship

30 on AA Index v5.0.1384 experts, 8 active55 tokens a second

V2.6-Pro is no longer the top open-weights model. At launch it scored 46 on Artificial Analysis's v4.3 index, the best open mark at the time.

Version 5 replaced that index in late October on a new scale, and on v5.0.1 the V2.6-Pro page shows 30, read on November 20, 2026.

On the same open-weights leaderboard, Kimi K3.1 Pro leads at 46, ahead of DeepSeek V4.1 Flash (33), GLM-5.3 (32) and Inkling 2 Small (31). The two scales do not convert, so 46 to 30 is not a 16-point fall.

Cost is where Pro still wins. Artificial Analysis spent $0.26 per index task on it, against $3.68 on Kimi K3.1 Pro, and it generated 131 million tokens doing so.

It is slow: 55 tokens a second and 45.39 seconds before the first answer token.

Under the hood it is a 70-layer mixture-of-experts. Sixty layers use sliding-window attention over the last 128 tokens and ten use global attention, which keeps the KV cache small at a 1M-token context.

A 681M-parameter vision encoder and a 308M audio tokenizer sit in front of the language backbone, and a five-layer drafter predicts seven tokens per pass for speculative decoding. The MiMo-V2.6-Pro-RL model card lists all of it.

Xiaomi's own benchmark table puts the jump from V2.5-Pro at its widest on long agent tasks. DeepSWE v1.1, a long-horizon software engineering test, went from 19.0 to 71.9.

Terminal Bench 4.0 went from 1.5 to 34.9, still well short of the 49.0 the same table gives Claude Opus 5.

Five days after launch Xiaomi pushed a second checkpoint, MiMo-V2.6-Pro-MOPD. Its model card says the first release sometimes repeated the same tool call over and over inside agent loops, "appearing busy while making no progress".

The MOPD checkpoint folds a short specialized-teacher pass into training to cut that repetition. If you self-host V2.6-Pro for agents, start from the MOPD weights.

We ran V2.6-Pro through seven test prompts in Xiaomi's MiMo Studio on November 20, 2026.

It got the math and JSON prompts right, refused to invent a fake paper and wrote a working password checker, but took 7 to 8 minutes on the code, reversed one step of a debugging explanation and declined a World Cup fact-check, since Studio has no web search.

27 on AA Index v5.0.1$0.13 per index task256 experts, 8 active

Flash costs a third of Pro per token and finishes within a few points of it on most of Xiaomi's agent tests: 67.9 against 71.9 on DeepSWE, 87.6 against 89.9 on Terminal Bench 2.1. The catch is length.

On index v5.0.1, Artificial Analysis counted 163 million tokens from Flash against 131 million from Pro, so Flash's $0.13 per task is half of Pro's $0.26, not a third. It scores 27 to Pro's 30.

The Flash model card lists 309B total and 15B active parameters on a 48-layer backbone, with the same encoders and drafter as Pro.

MiMo-V2.6-Pro-UltraSpeed is not a separate model. It is a faster serving mode of Pro that runs at up to 20 times its output speed.

Xiaomi described the V2.5 version as an FP4-quantized backbone with a block-diffusion drafter in its MiMo-V2.5-Pro-FP4-DFlash release, and has not published the V2.6 setup.

It costs $4.35 in and $8.70 out per million tokens, exactly ten times standard Pro, and it has no batch option.

What the V2.6 Training Run Cost

Xiaomi streamed the V2.6 reinforcement learning run live and published the bill on its launch page.

Each model completed 30 RL steps over roughly 750,000 trajectories in under six days, with 1,568 samples per update and 3.5 to 3.7 billion tokens per step.

The two runs together cost about $3.47 million. Over those 30 steps, V2.6-Pro's DeepSWE score rose from 58.4 to 72.57 and Flash's from 48.8 to 65.68.

Xiaomi says it is releasing the technical report, training environments and RL code so others can reproduce the run. The report is a PDF inside the Pro model repository.

"In an era when compute is brutally scarce, we still chose to dedicate a team of several dozen people to one goal over an extended period: scaling up RL."

Fuli Luo, head of Xiaomi MiMo, quoted by VentureBeat, September 2026

MiMo-V2.5 and V2.5-Pro: Retired on October 21

The V2.5 pair entered public beta on April 23, 2026, and Xiaomi released the weights under MIT a few days later, base models included. V2.5-Pro shares V2.6-Pro's 1.02T size and was pretrained on 27 trillion tokens.

MiMo-V2.5 was the first natively omnimodal open MiMo, a 310B model trained on about 48 trillion tokens with Xiaomi's own vision and audio encoders.

On May 27, 2026 Xiaomi cut V2.5 API prices permanently, by up to 99% on some rates, and dropped the surcharge for long inputs. V2.6 kept those prices.

The retirement went ahead on schedule. From October 14, Xiaomi routed mimo-v2.5-pro calls to mimo-v2.6-pro and mimo-v2.5 calls to mimo-v2.6-flash.

The V2 generation is where MiMo turned from a research model into a product. Three months separated the first open MoE from the first trillion-parameter one.

Dec 16, 2025

MiMo-V2-Flash: 309B open MoE, 256K context, $0.10 in and $0.30 out per million tokens

Feb 4, 2026

V2-Flash update: 78.6 on SWE-bench Verified in thinking mode, tool-call success up from 64% to 97%

Mar 18, 2026

V2-Pro, V2-Omni and V2-TTS: the first API-only MiMo models; V2-Pro reaches 1M tokens of context

Jun 30, 2026

All four V2 API names retired, after automatic routing to V2.5 from June

MiMo-V2-Flash introduced the design every later model kept: five sliding-window layers to one global layer, a 128-token window, and three multi-token prediction layers that Xiaomi says roughly triple output speed.

It launched at 73.4 on SWE-bench Verified.

MiMo-V2-Pro raised the ratio to seven to one and the size past one trillion parameters, priced at $1 in and $3 out up to 256K tokens and double that beyond.

V2-Omni added image, video and audio understanding with a 256K window, and V2-TTS was a speech model pretrained on more than 100 million hours of audio. Only V2-Flash ever had open weights, and they are still on Hugging Face.

The 7B Generation: Reasoning, Vision, Audio and Robots

Xiaomi's first MiMo release was MiMo-7B, a reasoning model trained from scratch on about 25 trillion tokens and released in four checkpoints: Base, SFT, RL-Zero (reinforcement learning straight from the base) and RL.

Its RL version scored 55.4 on AIME 2025, ahead of OpenAI o1-mini's 50.7 in Xiaomi's table, and a May 2025 refresh (MiMo-7B-RL-0530) lifted that to 70.2. Three spin-offs followed on the same 7B backbone.

MiMo-7B-RLApr 2025

95.8

MATH-500, pass@1, with a 32K context

MiMo-VL-7BMay / Aug 2025

70.6

MMMU for the 2508 RL version, with a switch to turn thinking off

MiMo-Audio-7BSep 2025

100M+ hrs

Of audio in pretraining, which Xiaomi says produced few-shot learning

MiMo-Embodied-7BNov 2025

29

Benchmarks across embodied AI (17) and autonomous driving (12)

MiMo-VL pairs the 7B backbone with a vision encoder and a 128K context, so it runs on modest hardware.

MiMo-Embodied is the most Xiaomi-specific: one model for both robot task planning and driving scenes, which fits a company that also builds cars. All of these are MIT-licensed weights with no hosted API.

Speech Models: MiMo-V2.5-TTS and ASR

Xiaomi's speech line now sits on its own version track.

The MiMo-V2.5-TTS series launched on April 23, 2026 as three models: mimo-v2.5-tts with preset voices, mimo-v2.5-tts-voicedesign, which builds a new voice from a one-sentence description, and mimo-v2.5-tts-voiceclone, which copies a voice from a few samples.

All three are still free for a limited time. MiMo-V2.5-ASR is the open one: MIT weights for Mandarin, English, code-switched speech and dialects including Wu, Cantonese, Hokkien and Sichuanese. On the API it costs $0.074 per hour of audio.

MiMo-V2.6-Distill-Qwen-9B: The Small One

The 9B release is not a shrunken MiMo. It is Alibaba's Qwen3.5-9B fine-tuned on 77.4 billion tokens of data that MiMo-V2.6 generated, and Xiaomi calls it a starting point for agentic RL research rather than a product.

The biggest gains over the base Qwen are on agent work: AutomationBench rose from 5.0 to 30.3 and SWE-bench Pro from 32.0 to 44.6, while SWE-bench Verified barely moved, 60.0 to 61.1.

Xiaomi releases it under MIT, and the Qwen3.5-9B base underneath is Apache 2.0, so both sets of terms are permissive. For how the Qwen line itself developed, see our guide to every Qwen model.

MiMo Pricing and the Token Plan

Xiaomi has cut MiMo prices since launch. V2-Pro opened at $3 per million output tokens; the V2.6-Pro that replaced it costs $0.87. The one rate that went up is Flash input, from $0.10 on V2-Flash to $0.14 today.

Xiaomi has not released a new MiMo model since V2.6 in September; its update log and Hugging Face account show nothing newer as of November 20.

Model

Input, cache hit

Input, cache miss

Output

Batch output

MiMo-V2.6-Pro

$0.0036

$0.435

$0.87

$0.435

MiMo-V2.6-Flash

$0.0028

$0.14

$0.28

$0.14

MiMo-V2.6-Pro-UltraSpeed

$0.036

$4.35

$8.70

Not offered

MiMo-V2.5-ASR

$0.074 per hour of input audio

Not offered

A cache hit on V2.6-Pro costs less than 1% of a miss, and cache writes are free for now, so long agent sessions that resend the same context cost a fraction of the list rate. Web search is billed on top at $5 per 1,000 calls outside China.

For developers who work inside coding agents, the Token Plan is a monthly subscription at $6, $16, $50 or $100, for 4.1 billion to 82 billion credits.

A V2.6-Pro output token costs 600 credits and a Flash output token 200, and usage between 00:00 and 08:00 Beijing time burns 20% fewer credits. Xiaomi says it works with OpenCode, OpenClaw and Claude Code.

The models are also on OpenRouter, where Xiaomi's own route charges the list price and DeepInfra and GMICloud sell V2.6-Pro for less, from $0.33 in and $0.65 out.

Context Windows and Output Limits

MiMo went from a 32K context to 1M tokens in under a year, and the jump came with V2-Pro in March 2026.

Every current text model now accepts 1M tokens in and returns up to 128K tokens out, at the same per-token price whatever the input length since the May 2026 repricing.

The base checkpoints are the exception. MiMo-V2.5-Base and V2.5-Pro-Base stop at 256K, because Xiaomi extends the window from 32K to 256K to 1M during post-training. Anyone fine-tuning from a base model has to redo that extension.

The ASR and TTS models take 8K tokens per request. The default rate limit on the API is 100 requests and 10 million tokens a minute per account.

License and Self-Hosting

MIT is as permissive as model licenses get. There is no revenue threshold, no attribution rule and no separate agreement for model-as-a-service businesses, which is not true of Kimi K3 or the big Qwen3.8 weights.

Wiring MiMo into an agent harness, or serving a trillion-parameter model on your own GPUs, takes engineers who have done it before. We place vetted LLM developers from Asia, and our LLM developer cost guide shows what a hire costs.

Most of it. Every MiMo model on Hugging Face, from MiMo-7B to V2.6-Pro, is MIT-licensed, and V2.6 also comes with Xiaomi's RL code and training environments.

The exceptions were MiMo-V2-Pro, V2-Omni and the TTS models, which were only ever available through the API.

Can I use MiMo for free?

Partly. The open weights cost nothing to download, the TTS series is free on the API for a limited time, and MiMo Studio lets you chat with V2.6-Pro on a free Xiaomi account; Xiaomi labels Studio a developer demo.

API use of the text models is paid, by the token or through the Token Plan.

Is MiMo-V2.6-Pro still the best open-weights model?

No. On the Artificial Analysis Intelligence Index v5.0.1 it scores 30, and Kimi K3.1 Pro leads the open-weights models at 46. Its 46 from September was on the older v4.3 scale.

At $0.26 per index task it still costs less to run than Kimi K3.1 Pro ($3.68), DeepSeek V4.1 Flash ($0.60) or GLM-5.3 ($5.01).

Can I use the MiMo API outside China?

Yes. The MiMo API Platform bills overseas accounts in US dollars at separate rates from the RMB price list, and the V2.6 models are also sold through OpenRouter.

Get developers who ship with AI agents. Starts at $25/Hour.

Matt Li is a tech-driven entrepreneur with deep expertise in global talent strategy, digital experience optimization, e-commerce, and Web3 innovation.
He is the Co-Founder of Second Talent, a US-based company that connects businesses with top-tier tech professionals worldwide. Since launching the company in 2024, Matt has led its growth by leveraging technology to streamline remote hiring and scale distributed teams.
With a background spanning product, operations, and innovation, Matt brings a cross-disciplinary perspective to the evolving digital economy. His work sits at the intersection of global talent, emerging technology, and scalable digital transformation.
