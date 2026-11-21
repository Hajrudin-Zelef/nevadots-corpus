---
id: collect-261121-veille-ia/qwen/qwen3-8-omni-flash-alibaba-s-audio-and-video-native-model-tested-against-gemini-3-8-flash-2026
title: "Qwen3.8-Omni-Flash: Alibaba's Audio and Video Native Model Tested Against Gemini 3.8 Flash (2026)"
domain: qwen
role: reference
task: article
actors: ["Alibaba"]
dates: ["2026-09-18"]
keywords: ["audio", "qwen3", "video", "model", "flash", "omni-flash", "gemini", "alibaba"]
source: https://contracollective.com/blog/qwen3-8-omni-flash-vs-gemini-3-8-flash-2026
source_anchor: ""
source_lines: [1, 54]
section: "Qwen"
---
# Qwen3.8-Omni-Flash: Alibaba's Audio and Video Native Model Tested Against Gemini 3.8 Flash (2026)

Alibaba's Qwen team released Qwen3.8-Omni-Flash on September 18, 2026, and it lands in a specific gap in the open model field: a genuinely omni modal model, meaning text, image, audio, and video all pass through a single native pipeline rather than a language model bolted to a separate audio or vision encoder, paired with a 1 million token context window and pricing aimed squarely at undercutting the closed, hosted competition. Alibaba's own evaluations put the model's average score more than 26 percent higher than its predecessor, Qwen3.5-Omni-Plus, across 30 benchmarks, and the company specifically frames the release around agentic audio video understanding and tool use rather than raw text generation quality.

| Attribute | Qwen3.8-Omni-Flash | Comparison point | 
|---|---|---|
| Released | September 18, 2026 |  | 
| Modality | Native text, image, audio, video in one pipeline |  | 
| Context window | 1,000,000 tokens total (991K input, 131K output) | Matches the scale of recent frontier context windows | 
| Input pricing | $0.15 per 1M tokens | Implicit cache hits at $0.016 per 1M | 
| Output pricing | $0.47 per 1M tokens |  | 
| Audio input cost change | Over 98 percent cheaper per hour vs Qwen3.5-Omni-Plus |  | 
| Audio visual input cost change | Over 93 percent cheaper per hour vs Qwen3.5-Omni-Plus |  | 
| Benchmark trend vs prior gen | Average score up more than 26 percent across 30 evaluations | Vs Qwen3.5-Omni-Plus | 

## Why native omni modality is the part worth reading past the price

Most multimodal pipelines in production today are compositions: a language model handling text and reasoning, with a separate vision encoder and often a separate audio pipeline feeding transcriptions or image descriptions into the context window as text. That composition works, but it means an agent reasoning over a video clip is really reasoning over a chain of intermediate summaries rather than the raw signal, and it means running the pipeline requires loading and serving multiple models with separate memory footprints and separate load times. Qwen3.8-Omni-Flash's pitch is that the model itself carries the audio and video pathway natively, the same architectural direction Z.ai took with GLM-5.3-Flash's native image and video handling. For an agentic tool calling loop that needs to interpret a screenshot, a short product video, or a voice note as part of a single reasoning step, that consolidation removes an entire component from the serving stack rather than just adding a capability to a component you already run.

The audio specific pricing cut is the more concrete signal for anyone already running voice or audio workloads. A drop of more than 98 percent in audio input cost per hour against Qwen3.5-Omni-Plus is not a marginal optimization, it suggests a genuinely different underlying encoding approach for audio rather than the same pipeline running at a discounted rate, and it puts sustained audio heavy workloads, transcription pipelines, voice agent memory, call analysis, in a materially different cost bracket than they were under the prior generation.

## How it stacks up against Gemini 3.8 Flash

Alibaba's own evaluations report Qwen3.8-Omni-Flash closely approaching Gemini 3.8 Flash on audio visual reasoning, with specific claims of higher consistency on multi speaker diarization and long form audio localization, the kind of task where knowing not just what was said but who said it and when matters as much as the transcription itself. We covered Gemini 3.8 Flash's own coding and agentic cost profile in detail in Gemini 3.8 Flash versus Claude Opus 5 on coding and agentic cost, and the pattern holds here too: Gemini 3.8 Flash remains a strong, fast, well benchmarked option, but Qwen3.8-Omni-Flash is pricing itself to compete on cost per workload rather than trying to win every benchmark outright. Treat Alibaba's own comparison numbers the way you would treat any vendor supplied benchmark, a reasonable signal to prioritize testing, not a substitute for running your own audio and video heavy tasks against both models before committing a pipeline to either.

## Where this fits against the rest of the Qwen family

Qwen3.8-Omni-Flash is a distinct model from Qwen3.8-Max, the larger text and code focused model in the same family that we have covered separately, including self hosting infrastructure requirements for Qwen3.8-Max as a 2.4T parameter MoE and its head to head against Claude Opus 5 Max on Code Arena and web dev tasks. Do not conflate the two when planning a deployment: Qwen3.8-Max is the model to reach for when the job is coding and agentic tool use at frontier scale, while Qwen3.8-Omni-Flash is the one built specifically for workloads where audio and video understanding are the actual bottleneck. A team building an agent that both writes code and interprets a user's screen recording of a bug report is a realistic case for running both models in the same pipeline, each doing the part it is actually optimized for, rather than forcing one model to do both jobs adequately instead of either job well.

## FAQ

**When was Qwen3.8-Omni-Flash released?**
September 18, 2026, by Alibaba's Qwen team.

**What makes it different from a typical multimodal model?**
It processes text, image, audio, and video natively in a single pipeline rather than composing a language model with a separate vision or audio encoder, and it ships with a 1 million token context window.

**How does its pricing compare to its predecessor?**
Audio input costs over 98 percent less per hour and audio visual input costs over 93 percent less per hour than Qwen3.5-Omni-Plus, alongside a claimed 26 percent average benchmark improvement across 30 evaluations.

**Is Qwen3.8-Omni-Flash the same model as Qwen3.8-Max?**
No. Qwen3.8-Max is a separate, larger model built for coding and agentic tool use. Qwen3.8-Omni-Flash is purpose built for native audio, video, and image understanding.

## When this applies to your stack

If your pipeline needs to interpret audio, video, or screenshots as part of an agentic workflow and you are currently composing a language model with separate vision or audio tooling to get there, Qwen3.8-Omni-Flash's pricing and native architecture make it worth an independent benchmark against Gemini 3.8 Flash on your actual workload rather than Alibaba's evaluation set. Evaluating a model like this against your real data instead of a vendor benchmark slide is the kind of AI integration work we do at Contra Collective.

## More from the *lab.*

 On its fiscal Q4 earnings call on November 2, 2026, Apple named two enterprise customers doing something more specific than the usual AI PC talking point: Gap runs AI loss prevention models across hundreds of stores entirely on Mac minis, which CFO Kevan Parekh described specifically as eliminating cloud token costs, and Christie's runs pricing estimate models on Mac Studio. Apple's pitch is that a few thousand dollar Mac can produce inference value worth tens of thousands over its life. That math is directionally right for a narrow set of workloads, and it is not a general argument against cloud inference, even though the earnings call framing invites you to read it that way.

Kimi K3.1 Pro went live in Moonshot's API on October 23, 2026, priced at 2 dollars per million input tokens and 10 dollars per million output tokens, with a 1 million token context window. Moonshot promised open weights by November 4. The model inherits Kimi K3's reported 2.8 trillion total parameters and 104 billion active parameters per token, which means the real question for most teams is not whether it is open but whether anything they own can actually run it.

oMLX search interest has climbed fast this year, and the pull is a specific engineering claim: a coding agent's time to first token dropping from 90 seconds to as little as 3 once a long prompt's key value cache is already sitting on SSD instead of being recomputed from scratch. The architecture is real and the restore speed is measurable. The claim is also softer than the headline number suggests, and this post works through both halves.
