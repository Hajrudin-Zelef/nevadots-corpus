---
id: collect-261121-veille-ia/qwen/alibaba-qwen4-max-3-27-ai-model-benchmark-results
title: "Alibaba Qwen4-Max: $3.27 AI Model Benchmark Results"
domain: qwen
role: reference
task: article
actors: ["Alibaba"]
dates: []
keywords: ["qwen4-max", "model", "alibaba", "benchmark", "token", "percent", "task", "index"]
source: https://aiincontext.com/articles/alibaba-s-qwen4-max-undercuts-rivals-at-3-27-per-benchmark-task
source_anchor: ""
source_lines: [1, 55]
section: "Qwen"
---
## Why it matters

- Alibaba's Qwen4-Max has 2.4 trillion parameters, activates 71 billion per token, and offers a one-million-token context window.
- Average cost per Work360 task falls 62 percent, from $4.43 to $1.67; the benchmark score rises from 47.74 to 52.69.
- Qwen4-Max scores 43 on the Artificial Analysis Intelligence Index at $3.27 per task, versus Kimi K3.1 Pro at 46 points and $3.68.
- Paired with Deep Principle's Mira, the model ran autonomously for 48 hours, screening 2.7 million molecular candidates for a chip coolant.
- API pricing is $1.50 per million input tokens and $4.50 per million output tokens; only smaller Qwen4 models will get open weights.

Alibaba says its new flagship Qwen4-Max cuts the average cost of an industrial work task by 62 percent, from $4.43 to $1.67, while raising the benchmark score from 47.74 to 52.69. The company positions the model directly against Anthropic's Claude Opus 5.5, OpenAI's GPT-6, and Moonshot AI's Kimi K3.1 Pro, targeting professional work in industry, science, and finance. The efficiency claim is the story's center of gravity; the external benchmark picture is more mixed.

The mixture-of-experts model (a design where only a fraction of the network fires for each token) has 2.4 trillion parameters and activates 71 billion per token, according to the Qwen team. It processes images natively and offers a one-million-token context window.

## How does the new architecture cut long-context costs?

Qwen4-Max debuts the revamped architecture of the Qwen4 series. Instead of relying only on standard attention layers, Alibaba combines two methods: three layers compress the prior context into a condensed state, and a fourth layer picks out only the parts that matter. That removes the requirement to give every part of the context equal attention, sharply cutting compute costs on long inputs.

The architecture also adds four parallel connections between model layers, with dynamic gates meant to stabilize information flow. An N-gram embedding layer (a component that supplies common token sequences as ready-made building blocks) carries 328 billion parameters. Alibaba says training the 2.4-trillion-parameter configuration ran stably, with training loss falling steadily and no sudden spikes.

Alibaba's internal evaluations measure model quality in bits per byte across knowledge, code, question answering, and STEM. Qwen4-Max-Base scored best among the five checkpoints tested, improving 5 to 13 percent over Qwen3.8-Max-Base while activating 25 percent fewer parameters per token. The caveat: these are Alibaba's own measurements, not independent ones.

## What does the efficiency gain mean in tokens and dollars?

Work360, Alibaba's benchmark for industrial tasks across 721 specialized fields, shows Qwen4-Max using roughly a quarter to a third fewer tokens than its predecessor on the same tasks. On the agent benchmark Terminal-Bench 4.0, token use drops by nearly half — about 105,000 output tokens per benchmark task versus 171,000 for Qwen3.8-Max.

At its highest reasoning setting, Qwen4-Max scores 43 points on the current Artificial Analysis Intelligence Index. That is nine points more than Qwen3.8-Max and three behind Kimi K3.1 Pro, the top Chinese model at 46. Artificial Analysis puts Qwen4-Max's cost at $3.27 per Intelligence Index task — about a third of Qwen3.8-Max's $10.02 and roughly 11 percent below Kimi K3.1 Pro's $3.68. The savings come from three sources: token prices 25 percent lower than the predecessor, cache hits at 70 percent less, and lower overall token consumption.

Tencent's Hy4 is cheaper still at $1.03 per task, but scores eight points lower on the index. In short: Qwen4-Max is the second-ranked Chinese model on this index, at a third of what its predecessor cost.

## Where does the model fall short?

External results are uneven. On long-horizon agent tasks like Agents' Last Exam v1.1, Qwen4-Max scores 46.7 points, the best in the comparison group. It trails Opus 5.5 and the GPT models on Humanity's Last Exam, and on HealthBench Professional it falls behind the GPT models while edging past Opus 5.5. On Terminal-Bench 4.0 it scores 48 percent, well behind Kimi K3.1 Pro at 67 percent. On Cognition's FrontierCode 2.0 coding benchmark, it ties Kimi at 26 percent and trails Tencent Hy4 at 31 percent.

The biggest gains over the predecessor appear on HLE-Diamond (up 21 points to 45 percent), AA-Omniscience (up 18), and AutomationBench-AA (up 17). Through its own API, Alibaba charges $1.50 per million input tokens, $4.50 per million output tokens, and $0.075 for cache hits.

## What is Live Delivery?

Like much of the industry, Alibaba builds this release around agents that run long tasks on their own. If users only see results at the end, they have almost no way to fix mistakes. Alibaba's answer is "Live Delivery": the model delivers a usable first version early, builds it out in later passes, and lets users change course at any point. Alibaba has open-sourced the accompanying Qwen-Live-Delivery-Plugin.

The company cites partner deployments. Automotive partners SANF and Pacific Union reportedly used Qwen4-Max to cut manual programming time for a single workpiece from about 475 minutes to 24. In finance, the model taps 90 MCP tools (a protocol for connecting models to external software) to pull market data from multiple sources and deliver results as verifiable Excel and PowerPoint files, with every number traceable to its original source. Alibaba also uses the model internally: paired with the in-house solver MindOpt, it cut the cost of allocating compute jobs for data annotation by an average of 10.42 percent and found optimal solutions for CPU task distribution in data centers more than 14 times faster than the comparison method.

## Can it run labs and build games?

Paired with Mira, a system from startup Deep Principle, the model ran autonomously for 48 hours, according to Alibaba. It designed, synthesized, and tested a new coolant for high-performance data center chips, searching 2.7 million molecular candidates and completing four full cycles of design, synthesis, and measurement. With PharmMaster, a platform from Beijing-based drug discovery startup DP Technology, the model optimized 24 candidate molecules targeting an inflammatory response linked to Alzheimer's disease. Alibaba says the final candidate binds to the target protein about 3.6 times better than the known starting compound from a patent, and DP Technology's experts consider it promising enough for further lab tests.

Qwen4-Max also works with Unity, Unreal Engine, and Godot. From a single natural-language prompt, it is supposed to handle the entire development process — scene setup, game logic, asset matching, materials, UI, and visual effects — until the game is playable. During development, the model runs the game itself and uses visual and interactive feedback to adjust camera work, character animations, hit reactions, and lighting. The sample project is "Meow of Magic," an Unreal Engine adventure about a cat wizard. Alibaba says the model also builds reusable 3D models in Blender. Notably, Anthropic recently moved the other way: after unofficial Halo and GTA: Vice City clones, it barred Claude Code from recreating existing games.

## What did Alibaba say about safety?

On Last Secure Code Bench, the share of code that is both functional and secure rose from 22 to 31.8 percent, according to Alibaba. The share rated secure at all three testing levels nearly doubled, from 8 to 14.7 percent. On CBRN risks (chemical, biological, radiological, and nuclear), Alibaba says internal reviewers went through the model's full chains of thought and found no meaningful capability uplift for users. These are self-reported evaluations.

## Where does this leave the model race?

Qwen4-Max is available through QwenCloud, QwenWork, Qoder, and QwenStudio; smaller Qwen4 models will be released as open source later, but the flagship's weights stay closed for now. The lineage is short: Qwen3.8-Max, released in August, had the same 2.4 trillion parameters but 95 billion active per token, needed 64 steps per task instead of 14, and cost more than twice as much per task as Qwen3.7-Max, according to Artificial Analysis. Moonshot AI answered in October with the 2.8-trillion-parameter Kimi K3.1 Pro. DeepSeek uses a similar hybrid attention design in V4-Pro, while Zhipu AI got GLM-5.3's gains entirely from extra post-training. The competitive question is now whether Alibaba's efficiency-first bet — second place on quality, first on price-performance — pulls professional users away from the US frontier labs.

Original: qwen.ai
